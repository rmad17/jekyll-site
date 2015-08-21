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
- [Knitr](#knitr)

---


<div id='latexrutgers'/>
## Write a PhD dissertation using the required Rutgers formatting style

Rutgers requires a very specific formatting [style](http://gsnb.rutgers.edu/academics/electronic-thesis-and-dissertation-style-guide) when writing your dissertation. In this post, I will show you how to do so by using the *ruthesis* LaTeX style. I have outsourced this style from the [RU Math Department](http://www.math.rutgers.edu/grad/phd_requirements/thesis.html).

Now follow the next steps:

0. I would assume you have LaTeX up and running.
1. Download the style file [here](/resources/ruthesis.cls).
2. Put the style file into your dissertation root folder.
3. Write your dissertation: I have also outsourced an example from the [math folks](http://www.math.rutgers.edu/grad/phd_requirements/thesis.html), where I did some modifications. See the LaTeX code below.

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
## Write replicable papers using the R package "Knitr"
WIP.





