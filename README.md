# Welcome

<p class="lead">
I'm Héctor Bahamonde, a PhD candidate in the Department of Political Science in Rutgers University (New Brunswick, NJ). This is my website.
</p>


## Open Source and Replicability

In this website I will be posting my work. I use the [knitr](http://yihui.name/knitr/) package, so all I do is fully replicable. When available, you can find the data and the respective `R` and `LaTeX` codings in my repositories.

### This website
This website is proudly open sourced under the [MIT license](https://github.com/hbahamonde/hbahamonde.github.io/blob/master/LICENSE.md) and freely hosted in [GitHub Pages](https://pages.github.com). I modified the free [Lanyon](http://lanyon.getpoole.com) theme using the free [Jekyll](jekyllrb.com) and [Markdown](http://daringfireball.net/projects/markdown/) languages, as well as some HTML/CSS. Feel free to fork and make your own. 

#### Installation

Mac users: open command line and enter the following lines, separately.

1. Install Homebrew: 
`ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
2. Install Ruby: `brew install ruby`
3. Install Ruby Gems: `gem update —system`
4. Install Jekyll: `gem install jekyll`

Then, open your browser and,
5. [Install](https://nodejs.org) `NODE JS`.

Finally, download the fonts:

6. [Source Pro Code](https://www.google.com/fonts/download?kit=5CnRSlG29fo96WRM6evqx3XmVIqD4Rma_X5NukQ7EX0).
7. [Lato](https://www.google.com/fonts/download?kit=NdjKCQMCiQM2g3qf94rrwQ).

#### MathJax

If you want to include math symbols using **LaTeX** in your webiste, you will need **[MathJax](https://www.mathjax.org)**.

##### Installation

In this section, I borrow from a [post](http://stackoverflow.com/questions/10987992/using-mathjax-with-jekyll) in Stackoverflow, which some modification by myself.

1. Install **kramdown** which will render your math symbols within the Markdown framework.

`gem install kramdown`

2. I recommend calling the latest MathJax version from your website instead of downloading the file. Put the code below in the `head` section of `head.html` document. It will open a **secure** connection and download/render (?) your math from MathJax directly.

```html
<!-- MathJax Installation-->
<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
```

3. Introduce the next line in your `_config.yml` file:

`markdown: kramdown`

###### Usage
Now, instead of the `$` signs you just use `\( ... \)`, like so `\( e^10 \)` which produces \( e^10 \). That's for inline math. For equations use ``$$ ... $$`. For more details, see the official info [here](http://docs.mathjax.org/en/latest/tex.html#supported-latex-commands).








