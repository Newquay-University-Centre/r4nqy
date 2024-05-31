--- 
title: "Data analysis for Newquay University Centre"
author: "Michael Hunt"
date: "2024-05-31"
site: bookdown::bookdown_site
documentclass: book
bibliography: [book.bib, packages.bib]
# url: your book url like https://bookdown.org/yihui/bookdown
# cover-image: path to the social sharing image like images/cover.jpg
description: |
  This covers aspects of data analysis for undergraduate and postgraduate students at Newquay University Centre.
  The HTML output format for this example is bookdown::gitbook, set in the _output.yml file.
link-citations: yes
github-repo: rstudio/bookdown-demo
---



# Welcome

This book is intended to help anyone who wants to analyse their data in a scientific context. It is aimed in particular at those studying CORN276 Research Methods and GIS for Zoology, CORN4004 Action Reserch Project and CORN328 Honours Project at Newquay University Centre, but we hope it will be of interest and use to anyone with similar needs.

We will touch on every stage of your analysis, from designing the spreadsheet where you will first keep your data, to summarising the data, plotting it and subjecting it to whatever statistical test is appropriate. Finally we will learn how to interpet the output of these tests and then report this in language appropriate for a scientific report.

## Usage 

Each **bookdown** chapter is an .Rmd file, and each .Rmd file can contain one (and only one) chapter. A chapter *must* start with a first-level heading: `# A good chapter`, and can contain one (and only one) first-level heading.

Use second-level and higher headings within chapters like: `## A short section` or `### An even shorter section`.

The `index.Rmd` file is required, and is also your first book chapter. It will be the homepage when you render the book.

## Render book

You can render the HTML version of this example book without changing anything:

1. Find the **Build** pane in the RStudio IDE, and

1. Click on **Build Book**, then select your output format, or select "All formats" if you'd like to use multiple formats from the same book source files.

Or build the book from the R console:


```r
bookdown::render_book()
```

To render this example to PDF as a `bookdown::pdf_book`, you'll need to install XeLaTeX. You are recommended to install TinyTeX (which includes XeLaTeX): <https://yihui.org/tinytex/>.

## Preview book

As you work, you may start a local server to live preview this HTML book. This preview will update as you edit the book when you save individual .Rmd files. You can start the server in a work session by using the RStudio add-in "Preview book", or from the R console:


```r
bookdown::serve_book()
```



