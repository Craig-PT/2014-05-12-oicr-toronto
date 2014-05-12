Reproducible Research
=====================

Independent replication of a scientific study is the best way to evaluate its truth.

Short of replication, a study should be reproducible.

Research is reproducible if data and analysis methods are shared so that others can reproduce the results.
With code, this means sharing your raw data and analytic code for others to use and evaluate.

Sharing code ensures transparency, but not validity.
However, it allows others to more easily judge validity.

Raw data -> processed data -> data analysis -> presentable results

Code allows you to make your workflow/pipelines reproducible.

How can we ensure our research is reproducible?
-----------------------------------------------

- Don't do things by hand.
  - No Excel!
  - Don't download data by clicking a link. 
    - download.file()
  - No copy and pasting to new files.
  - Don't edit data manually.
  - Analysis programs with GUIs can sometimes be problematic.
  - If it has to be done by hand, document it thoroughly.

- Code allows a computer to do all the boring work while keeping an accurate record.
  - Almost guarantees reproducibility.
  - Record your software environment (Computer details, OS, software versions)
    - sessionInfo()
    - date()

- Treat your code as part of your lab notebook.
  - A lab notebook should be written so that a colleague can read it and do exactly what you did.
  - Code should be written the same way, with lots of comments.
  
- Everything of importance should be done in scripts, not interactively.

- Work to ensure reproducibility from the start of a project.

- Use version control.

- Use a non-proprietary file format (e.g. .csv instead of .xls).

- If you generate random numbers or sample from a larger data set, set your seed to ensure the same results each time.
  - set.seed()


Literate programming in R using `knitr`
=======================================

**Basic idea:** Write **data** + **software** + **documentation** (or in this case reports) together.

You can place your code and results along with an appropriate description in a logical order in a single document.

This makes linking text, code, and results much easier.

Analysis code can be divided into text and code "chunks".
Doing so allows us to extract the code for machine readable documents (`tangle`) or produce a human-readable document (`weave`).

Literate programming involves three main steps:

- Separate the narrative from the code.
- Execute source code and return the results.
- Combine the results from the source code with the original narratives to produce a final document.

Installing `knitr`

```r
install.packages("knitr", dependencies = TRUE)
```


Knitr supports a variety of documentation formats including `markdown`, `html` and `LaTeX`. 
It also allows for easy export to `PDF` and `HTML`.

What is markdown?
-----------------

Markdown is an incredibly simple semantic file format, not too dissimilar from .doc, .rtf, or .txt.
Markdown makes it easy to write any sort of text (including with links, lists, bullets, etc.) and have it parsed into a variety of formats.
To learn more about the basics of markdown:

- [Markdown cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
- [Original markdown reference](http://daringfireball.net/projects/markdown/basics)

## Creating a basic knitr document

In RStudio, choose new R Markdown file (easiest way)
or you can create a new text file and save it with extension `.Rmd`.

A basic code chunk looks like this:

<pre><code>```{r}
# some R code
```
</code></pre>
---

You can knit this document using the knit button or do it programmatically using the `knit()` function.

```
library(knitr)
knit("file.Rmd")
```

Creating a knitr report
-----------------------

<pre><code>    ```{r}
    x <- rnorm(100)
    y <- x + rnorm(100)
    plot(x,y)
    ```
</code></pre>


```r
x <- rnorm(100)
y <- x + rnorm(100)
plot(x, y)
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.png) 


**What just happened?**

knitr read the Rmd file, then located and ran all the code chunks identified by the backticks, and replaced it with the output of R function calls. If figures are generated from any such calls, they will be included in markdown syntax.

Rmd -> md -> html

Once your output markdown files (`.md`) files are generated, you should never edit them because they are automatically generated. 
Next time you knit the original `.Rmd` files, all the changes in the `.md` file will get wiped out. 

## Chunk labels

You can also name your code chunks. This allows you to keep all the code in a separate script and just refer to code chunks using meaningful names (e.g. data-processing, analysis, model-fitting, visualization, figures, tables)


```
{r, chunk_name}
```

__Some rules on naming chunks__
* Chunk labels are supposed to be unique id’s in a document.  
* knitr will throw an error if two chunks have the same name.  
* If no chunk names are given, knitr will simply increment from chunk 1, 2,3 etc.

In addition to naming chunks within the curly braces, you can also add a bunch of other options on how that particular code chunk should behave. 

**Other options you can add to the tag**

| Option | Description |
| ------ | ------------ |
| **echo** =   TRUE or FALSE |  to show or hide code.  |
| **eval** =   TRUE or FALSE | to run or skip the code.  |
| **warning** =   TRUE or FALSE | to show or hide function warnings.  |
| **message** =  TRUE or FALSE | to show or hide function R messages.  |
| **results** = "hide"  | will hide results. They will still be executed |
| **fig.height** = | Height of figure  |
| **fig.width** =  | width of figure  |
