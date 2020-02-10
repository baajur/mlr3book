# mlr3book

[![Build Status](https://img.shields.io/travis/mlr-org/mlr3book/master?label=Linux&logo=travis&style=flat-square)](https://travis-ci.org/mlr-org/mlr3book)
[![StackOverflow](https://img.shields.io/badge/stackoverflow-mlr3-orange.svg)](https://stackoverflow.com/questions/tagged/mlr3)

Package to build the [mlr3](https://mlr3.mlr-org.com) [bookdown](https://bookdown.org/) book.

## Rendered Versions

- [HTML](https://mlr3book.mlr-org.com)

- [PDF](https://mlr3book.mlr-org.com/mlr3book.pdf)

## Building the book

To install all necessary dependencies for the book, install this R package using [remotes](https://cran.r-project.org/package=remotes):

```r
remotes::install_github("mlr-org/mlr3book", dependencies = TRUE)
```

To build the book, run one of the following commands:

```r
# HTML
withr::with_dir("bookdown", bookdown::render_book("index.Rmd",
  output_format = "bookdown::gitbook"))

# PDF
withr::with_dir("bookdown", bookdown::render_book("index.Rmd",
  output_format = "bookdown::pdf_book")) 
```

### Serve the book

Alternatively, you "serve" the book via a local server:

```r
bookdown::serve_book("bookdown")
```

The command above starts a service which automatically (re-)compiles the bookdown sources in the background whenever a file is modified.
If your browser does not open automatically, go to http://127.0.0.1:4321/.

### Makefile approach

Alternatively, you can use the provided `Makefile` (c.f. see `make help`).
This way, you can

- install dependencies
- build the HTML book -> `make html`
- build the PDF book (`bookdown:pdf_book`) -> `make pdf`

## File system structure

The root directory is a regular R package.
The book itself is in the subdirectory "bookdown".

## Style Guide

### Lists

For lists please use `*` and not `-`.

### Chunk Names

Chunks are named automatically as `[chapter-name]-#` by calling `name_chunks_mlr3book()`:

```r
mlr3book::name_chunks_mlr3book()
```

or alternatively executing `make names` from the terminal.

### Blocks

You can add certain ["blocks"](https://bookdown.org/yihui/bookdown/custom-blocks.html) supported by [bookdown](https://github.com/rstudio/bookdown) for notes, warnings, etc.
Start the code chunk with `block` instead of `r` and add `type='caution'`.

````
```{block <name>, type='caution'}
<text>
```
````

### Figures

#### Include existing figures

To include figures in the `Rmd` follow these rules:

* Use `knitr::include_graphics()` to add figures instead of markdown syntax `[](<figure>)`. `knitr::include_graphics()` works for the HTML and PDF output and allows to control the width + height of the figure.
* If available, include the `svg` version in the `Rmd` source, e.g. `knitr::include_graphics("images/some_figure.svg")`.
* If no `svg` version is available, include the `png` version.
* Never include the `pdf` version of a figure.

#### Adding a new figure

To add a new figure into the repository consider the following rules:

* Add the file in the `bookdown/images` folder without any subdirectory.
* Store the figure as `svg` file if possible, i.e. if it is a vector graphic.
  This allows us to re-use or modify images in the future.
* For any `svg` file you need to supply a `pdf` version with the exact same name.
* For `png` files only one version needs to be supplied.
  - `png` files should have reasonable resolution, i.e. the width of a pixel graphic should be between `400px` and `2000px`.
    If a higher resolution is needed to obtain a readable plot you are probably doing something wrong, e.g. use a pixel graphic where you should use a vector graphic.
* Please look at the file size.
  - If your `pdf` or `svg` file is larger than `1MB` it probably contains unnecessary unplotted content or unvectorized parts.
  - If your `png` file is larger than `1MB` the resolution is probably too big.

#### Further aspects

* How do I convert `svg` to `pdf`?
  - Use Inkscape, `rsvg-convert`, `convert` (ImageMagick) or any tool you like.
* How do I convert `pdf` to `svg`?
  - Use Inkscape which allows you to also remove unwanted parts of the `pdf`.
* Do not use screenshots!
  - *Google Slides* allows `svg` export.
  - *PDF* can be converted to `svg` and you can even cut parts.
  - *HTML* can be converted to `svg`.
* The difference between vector (`svg`) and pixel (`png`) graphics should be known.
  - Attention: `svg` and `pdf` also support to include pixel graphics.
    There is no guarantee that a `svg` or `pdf` is a pure vector graphic.
    If you paste a pixel graphic (e.g. a screenshot) into Inkscape and save it as `svg` it does not magically become a vector graphic.

### Spacing

- Always start a new sentence on a new line, this keeps the diff readable.
- Put an empty line before and after code blocks.
