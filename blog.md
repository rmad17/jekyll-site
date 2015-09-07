---
layout: page
title: Blog
---

<p class="lead">
In this section, I will be posting useful stuff that I have found to be helpful in grad school, mostly regarding computing applications.
</p>


---

# TOC

- [Rutgers PhD Dissertation Format in LaTeX](#latexrutgers)
- [Write replicable papers using the R package "Knitr"](#knitr)

---


<div id='latexrutgers'/>
## Write a PhD dissertation using the required Rutgers formatting style

Rutgers requires a very specific formatting [style](http://gsnb.rutgers.edu/academics/electronic-thesis-and-dissertation-style-guide) when writing your dissertation. In this post, I will show you how to do so by using the *ruthesis* LaTeX style. I have outsourced this style from the [RU Math Department](http://www.math.rutgers.edu/grad/phd_requirements/thesis.html).

Now follow the next steps:

* I would assume you have LaTeX up and running.
* Download the style file [here](/resources/ruthesis.cls).
* Put the style file into your dissertation root folder.
* Write your dissertation: I have also outsourced an example from the [math folks](http://www.math.rutgers.edu/grad/phd_requirements/thesis.html), where I did some modifications. See the LaTeX code below.

```tex

\documentclass{ruthesis} 
\special{papersize=8.5in,11in} %***for A4-default configurations on servers 
\begin{document} 
\phd % modify this if you're a MA MS student

\title{Title} 
\author{H\'ector Bahamonde} 
\program{Political Science} 

\director{The principal advisor's name} 

\approvals{4} 
\submissionyear{2017} 
\submissionmonth{May} 

\abstract{Abstract here} 

\beforepreface 
\acknowledgements{acknowledgements} 

\dedication{dedication} 

\afterpreface 

% Dissertation's contents
\chapter{Introduction} 
\chapter{Chapter I here} 
\section{Section I here} 

%CV section
\begin{vita} 
\heading{Author of the thesis} 

\vspace{15pt} 

\begin{descriptionlist}{xxxxx-xxxxx} %reverse chronological order 
\item[2017] Ph.D. in Political Science, Rutgers University 
\item[2009] B.A. in Political Science from some PUC. 
\item[2002] Graduated from high school. 
\end{descriptionlist} 

\medskip 

\begin{descriptionlist}{xxxxx-xxxxx} %positions held since BS degree 
\item[2014-2017] TA, Department of Political Science, Rutgers University
\item[2012-2017] RA, Department of Political Science, Rutgers University 
\end{descriptionlist} 
\end{vita} 

\end{document} 

```


That's it.


---


<div id='knitr'/>
## Literate programming: Write replicable papers using the R package "Knitr"

Political *Science* is not a *science* because of *what* we study, but because of *how* we study it, that is, it is a *science* (**regardless of the precision of our instruments**) because we use the scientific method. No more, no less.

As you might remember from your intro to epistemology class, *replication* plays a fundamental role in scientific progress. It allows *us*, the scientific community, to evaluate (more or less) collectively if new findings are to be considered "better" than past knowledge. In this process, we ought to know, among many other things, how this new piece of finding was produced. 

Some research projects are more reproducible than others. And so, *cherry-picking* is very common among faculty and grad students, unfortunately. In order to make this process more transparent and verifiable, quantitative researchers need to have access to one's coding and data. And not only that, but also to one's own methodological/empirical decisions: Why did I choose this or that model? What did I do with my missing data? And so own and so forth. 

As [some](http://kbroman.org/knitr_knutshell/) have explained it, "KnitR is a really important tool for reproducible research. You create documents that are a mixture of text and code; when processed through KnitR, the code is replaced by the results and/or figures produced". It was developed by [Yihui Xie](http://yihui.name/). That's exactly what **literate programming** is: a combination of `LaTeX` code (your text) plus `R` code (your analyses), in *the same document*. Thus, everything you see in the document can be easily accessed by looking at the very code that produced the results.

### Installation

In this section I will describe *my* take on literate programming: not the best, not the only one. Just the one I use.

* Text processor and necessary dependencies: First you will need a software to handle code and syntax highlighting. I use [Sublime Text 2](http://www.sublimetext.com/2). Have Sublime Text 2 [installed](http://www.sublimetext.com/2). 

	Of course you can just **R-Studio** to compile all these, of course, under the **knitr** mode: **Preferences --> Sweave --> Weave Rnw files using --> knitr**.

* Inside Sublime Text: 
	* Install [Package Control](https://packagecontrol.io/installation).
	* `shift+cmd+p`, then **Install Packages**, and install:
		* [R-Box](https://github.com/randy3k/R-Box).
		* [Sublime Knitr](https://github.com/andrewheiss/SublimeKnitr).
		* [LaTexing](http://www.latexing.com/installation.html) or [LaTeX Tools](https://github.com/SublimeText/LaTeXTools).

* *If you're using LaTeX Tools*, follow the instructions to patch it [here](https://github.com/andrewheiss/SublimeKnitr#manual-patch-for-latextools) - that's right, patch the 6 files!

* In **R** you have to install **knitr**: 

```r
install.packages(c("knitr"), repos = "http://cran.rstudio.com")
```

* Writing **R** and **LaTeX** code together:

Create a `.rnw` file. Here is a small example I modified from [Yihui Xie](https://github.com/yihui/knitr-examples/blob/master/002-minimal.Rnw), the developer of **knitr**.

Example: Lets try an example. See the output below.

---

Here is a code chunk.

```r 
<<label, fig.cap='Your Title Goes Here', tidy=TRUE>>=
# Speaking of reproducibility, don't forget to set a seed so others can generate the SAME sequence!
set.seed(602) 
library(MASS) # to generate multi-var distribution
e <-  as.numeric(mvrnorm(n = 100, mu = 0, Sigma = 1))
plot(e)
@
```

<img src="/resources/label-1.pdf" alt="Example Plot 1" style="width:350px;height:350px;">

The function `tidy=TRUE` keeps your code within the page margins. See how the line *Speaking of reproducibility...* behaves.

You can also write inline expressions, e.g. \\( \pi \\) = **3.141592**. Here I printed the number \\( \pi \\) by calling the `\Sexpr{pi}` function. 

Also, you can use information specified in your code such as the mean of \\( \epsilon \\) which is **-0.1682144**, which I computed calling the `mean` function inside the `Sexpr` expression, like so `\Sexpr{mean(e)}`.

Editors and (**most**) readers are **not** interested in your coding. That's fine. The important thing is to make your code available to the audience that needs it. You can hide it by calling the `echo = FALSE` function in the preamble, i.e. within the `<<>>=` symbols. Below, you will only see the plot, not the coding that generated it.

```r 
<<histogram, echo = FALSE, fig.cap='Your Title Goes Here'>>=
hist(e)
@
```
<img src="/resources/histogram-1.pdf" alt="Example Plot 2" style="width:350px;height:350px;">

Finally, following the standard  **LaTeX** tools, you can call the first figure using the `\autoref{fig:label}` function. Similarly, you can call the second one by typing the `\autoref{fig:histogram}` function.


---

Here below is the LaTeX code. Copy and paste into a `.rnw` file.

```tex
\documentclass{article}
\usepackage{hyperref} % to generate figure and (sub)section hyperlinks within document.
\begin{document}

Here is a code chunk.

<<label, fig.cap='Your Title Goes Here', tidy=TRUE>>=
# Speaking of reproducibility, don't forget to set a seed so others can generate the SAME sequence!
set.seed(602) 
library(MASS) # to generate multi-var distribution
e <-  as.numeric(mvrnorm(n = 100, mu = 0, Sigma = 1))
plot(e)
@

The function \texttt{tidy=TRUE} keeps your code within the page margins. See how the line \emph{Speaking of reproducibility...} behaves.

You can also write inline expressions, e.g. $\pi=\Sexpr{pi}$. Here I printed the number $\pi$ by calling the \texttt{Sexpr} function. 

Also, you can use information specified in your code such as the mean of {\bf e} which is {\bf -0.1682144}, which I computed calling the \texttt{mean} function inside the \texttt{Sexpr} expression.

Editors and ({\bf most}) readers are {\bf not} interested in your coding. That's fine. The important thing is to make your code available to the audience that needs it. You can hide it by calling the \texttt{echo = FALSE} function in the preamble, i.e. within the \texttt{<<>>=} symbols. Below, you will only see the plot, not the coding that generated it.

<<histogram, echo = FALSE, fig.cap='Your Title Goes Here'>>=
hist(e)
@

Finally, following the standard  \LaTeX tools, you can call the first figure using the \autoref{fig:label} function. Similarly, you can call the second one by typing the \texttt{\autoref{fig:histogram}} function.

\end{document}
```










