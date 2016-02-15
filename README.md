# Welcome

<p class="lead">
I'm Héctor Bahamonde, a PhD candidate in the Department of Political Science in Rutgers University (New Brunswick, NJ). This is my website.
</p>


## Open Source and Replicability

In this website I will be posting my work. I use the [knitr](http://yihui.name/knitr/) package, so all I do is fully replicable. When available, you can find the data and the respective `R` and `LaTeX` codings in my repositories.

### This website
This website is proudly open sourced under the [MIT license](https://github.com/hbahamonde/hbahamonde.github.io/blob/master/LICENSE.md) and freely hosted in [GitHub Pages](https://pages.github.com). I modified the free [Lanyon](http://lanyon.getpoole.com) theme using the free [Jekyll](jekyllrb.com) and [Markdown](http://daringfireball.net/projects/markdown/) languages, as well as some HTML/CSS. Feel free to fork and make your own. 

#### Installation

Mac users: open **command line** and enter the following lines, separately.

* Install **[Homebrew](http://brew.sh)**: 

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

* Install Ruby: `brew install ruby`
* Install Ruby Gems: `gem update —system`
* Install Jekyll: `gem install jekyll`

* Then, open your browser and,

	* [Install](https://nodejs.org) **NODE JS**.

* Finally, download the fonts:

	* [Source Pro Code](https://www.google.com/fonts/download?kit=5CnRSlG29fo96WRM6evqx3XmVIqD4Rma_X5NukQ7EX0).
	* [Lato](https://www.google.com/fonts/download?kit=NdjKCQMCiQM2g3qf94rrwQ).

#### MathJax

If you want to include math symbols from **LaTeX** symbols library, *within* Markdown using in your webiste, you will need **[MathJax](https://www.mathjax.org)**.

##### Installation

In this section, I borrow from a [post](http://stackoverflow.com/questions/10987992/using-mathjax-with-jekyll) in Stackoverflow, which some modifications by myself.

* Install **kramdown** which will render your math symbols within the Markdown framework: 

```
gem install kramdown
```

* I recommend calling the latest MathJax version from your website instead of downloading the file. Put the code below in the `head` section of `head.html` document. It will open a **secure** connection and render your math from MathJax directly.

```html
<!-- MathJax Installation-->
<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
```

* Then, introduce the next line in your `_config.yml` file: 

```html	
 markdown: kramdown		
```

=======
Show an open sidebar on page load by modifying the `<input>` tag within the `sidebar.html` layout to add the `checked` boolean attribute:

```html
<input type="checkbox" class="sidebar-checkbox" id="sidebar-checkbox" checked>
```

###### Usage
Now, instead of the `$` signs you just use `\\( ... \\)`, like so `\\( x^{2} \\)` which produces the desired LaTeX output. That's for inline math. For equations use ``$$ ... $$``. For more details, see the official info [here](http://docs.mathjax.org/en/latest/tex.html#supported-latex-commands).








