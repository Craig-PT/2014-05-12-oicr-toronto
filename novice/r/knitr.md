Literate programming in R using `knitr`
========================================================

**Basic idea:** Write **data** + **software** + **documentation** (or in this case manuscripts, reports) together.

Analysis code can be divided into text and code "chunks".
Doing so allows us to extract the code for machine readable documents (`tangle`) or produce a human-readable document (`weave`).

Literate programming involves three main steps:

- Separate the narrative from the code.
- Execute source code and return the results.
- Combine the results from the source code with the original narratives to produce a final document.

Why this is important?
-------------------------
Results from scientific research have to be easy to reproduce so others can verify results making them trustworthy. Otherwise we risk producing one off results that no one outside the original research group can reproduce. In this lesson we will learn reproducible research, which is one of the by products of dynamics report generation. However, this process alone will not always guarantee reproducibility. 
