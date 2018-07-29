---
layout: post
title: Custom Django Serializer
date:   2018-07-29 19:16:01 +0530

---

This is a story of how I faced issues with serializers of [django-rest-framework](http://www.django-rest-framework.org/)  and how I overcame the issues. 

### The Issue

<hr>

Normally if one is using `django-rest-framework` we tend to use the serializer classes provided as well. 

A simple serializer example from the [docs](http://www.django-rest-framework.org/api-guide/serializers/) of `django-rest-framework`.

```python
class CommentSerializer(serializers.Serializer):
    email = serializers.EmailField()
    content = serializers.CharField(max_length=200)
    created = serializers.DateTimeField()

    def create(self, validated_data):
        return Comment(**validated_data)

    def update(self, instance, validated_data):
        instance.email = validated_data.get('email', instance.email)
        instance.content = validated_data.get('content', instance.content)
        instance.created = validated_data.get('created', instance.created)
        return instance

# Its not mandatory to implement create and update methods, there are default implementations.
```



Then you might come across a `ModelSerializer`. An example of a `ModelSerializer` as mentioned from [here](http://www.django-rest-framework.org/api-guide/serializers/#modelserializer).

```python
class AccountSerializer(serializers.ModelSerializer):
    class Meta:
        model = Account
        fields = ('id', 'account_name', 'users', 'created')
```



Needless to say `ModelSerializers` were much neater and nicer to use if your serializers were supposed to update databases. It allowed adding of related fields as well and specifying/excluding fields as well.

```python
class AccountSerializer(serializers.ModelSerializer):
    url = serializers.CharField(source='get_absolute_url', read_only=True)
    groups = serializers.PrimaryKeyRelatedField(many=True)
    exclude = ('nickname',)

    class Meta:
        model = Account
```

For quite some time I extensively used this as it also had an validator which worked quite well.

```python
serializer = CommentSerializer(data={'email': 'foobar', 'content': 'baz'})
serializer.is_valid()
# False
serializer.errors
# {'email': [u'Enter a valid e-mail address.'], 'created': [u'This field is required.']}
```

However, despite all the goodies one of the problems I faced frequently were:

1. I have to write a new `Serializer` class everytime I create a new model, even though I will mostly exclude same fields.

2. Most APIs involved updating multiple models where one would be a foreign key in another.  This is disadvantageous since `serializer.data` returns a `dict` which contains the primary key id and I have make another db operation to fetch the model object which would be required in the next update as a foreign key.

3. I already had a layer called `dbapi` which would contain classes on a per model level. There were methods for specific queries to fetch the model obj which was again a lot of repetition. This layer was required because even for fetching queries or updating any object using serializers I needed to fetch the model object for which I had to query via some id. Having two mechanisms for the same layer seemed inconsistent and not a great idea.



### The Solution

<hr>

Initiatially I thought of inheriting the `Serializer` class but that was not simple because the [`Meta` class was not automatically inherited](https://docs.djangoproject.com/en/dev/topics/db/models/#meta-inheritance) and [2]  and [3] still remained unresolved.  



I decided to ensure the very classes at the `dbapi` layer can be used to make queries with very minimal change. 

First, I created a base Serializer class. It took a dictionary as a param, iterated over the model fields and removed non attributes from the dict,  ran some django validations and then finally saved it,

```python

class AbstractBaseSerializer(metaclass=ABCMeta):
    @property
    @abstractmethod
    def model(self):
        raise NotImplementedError('missing model!')

    def _iterate_fields(self, kwargs):
        fields = [f.name for f in self.model._meta.get_fields()]
        for k, v in kwargs.items():
            if k in fields or k.split('__')[0] in fields:
                yield k, v

    def _clean_save_object(self, obj):
        obj.full_clean()
        obj.save()
        return obj

    def _get_clean_data(self, kwargs):
        clean_data = {}
        for k, v in self._iterate_fields(kwargs):
            clean_data[k] = v

        if not clean_data:
            raise ModelOperationError(
                INVALID_PARAMS.format(self.model.__name__))
        return clean_data

    def create_obj(self, kwargs, **extra):
        try:
            clean_data = {}
            for k, v in self._iterate_fields(kwargs):
                clean_data[k] = v
            obj = self.model(**clean_data)
            return self._clean_save_object(obj)
        except IntegrityError:
            msg = FAILED_OPS.format('save', self.model.__name__)
            self._raise_error(msg, **extra)

    def update_obj(self, obj, kwargs, **extra):
        try:
            for k, v in self._iterate_fields(kwargs):
                setattr(obj, k, v)
            return self._clean_save_object(obj)
        except (TypeError, ValueError, ValidationError):
            msg = FAILED_OPS.format('update', self.model.__name__)
            self._raise_error(msg, **extra)

    def get_object(self, kwargs, **extra):
        clean_data = self._get_clean_data(kwargs)
        try:
            return self.model.objects.get(**clean_data)
        except self.model.DoesNotExist:
            msg = OBJ_NOT_FOUND.format(self.model.__name__)
            self._raise_error(msg, **extra)

    def filter_objects(self, kwargs):
        clean_data = self._get_clean_data(kwargs)
        return self.model.objects.filter(**clean_data)

    @staticmethod
    def _raise_error(msg, **kwargs):
        if not kwargs.get('fail_silently', False):
            raise ModelOperationError(msg)
```



And then in my existing  `dbapi` layer class, I just needed to add inheritance with specifying a particular model the class is tied to. Since the classes were already on a model level it did not require a lot of effort.

 

```python
class CommentDbIO(AbstractBaseSerializer):
  
  @property
  def model(self):
    return Comment
```

Doing it this way then hugely reduced my code required to say getting a model object  for comment by email. A sample example would be:

```python
>> obj = CommentDbIO().get_object({'email': 'foo@bar'})
```

This solved all code repetition issues, returned an object and also created a single point of interaction for db queries. However, the ability to have serialized data was also an useful thing especially when returning `json` via APIs.  While I did not require it at that point, it could be achieved with a slight modification to our Model class. 

All my model classes all inherited from a base class for some common fields like `created_at`, `modified_at`, etc.

I used django's [serializers module](https://docs.djangoproject.com/en/2.0/topics/serialization/#serializing-django-objects) to serialize the model object to json format.

```python
import json
from django.core import serializers


class AbstractModel(models.Model):
    created_at = models.DateTimeField(auto_now_add=True, verbose_name="Created At")
    modified_at = models.DateTimeField(auto_now=True, verbose_name="Last Modified At")

    class Meta:
        abstract = True
		
    def to_json(self, fields=None):
      	data = json.loads(serialize("json", (self,)))[0]
        clean_data = {}
        if fields:
          	for k, v in data.items():
              	if k not in fields:
                  clean_data[k] = v
        else:
        	clean_data = data.pop('fields')
        clean_data['id'] = data.pop('pk')
        return clean_data
```

So finally  if you have the model object, you can simply do `obj.to_json()` and also pass optional parameters if required. 

Let me know if you have any questions or have a better way of achieving this same target. 






















