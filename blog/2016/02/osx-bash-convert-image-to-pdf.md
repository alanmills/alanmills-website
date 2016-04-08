How to convert images to a pdf file from the Mac Bash terminal
==============================================================
**Author:** Alan Mills  
**Date:** [02 February 2016 22:31](/blog/history/2016-02.md)  
**Tags:** [OS X](/blog/categories/osx.md), [bash](/blog/categories/bash.md), [ImageMagick](/blog/categories/imagemagick.md), [Homebrew](/blog/categories/homebrew.md), [Portable Document Format](/blog/categories/portable-document-format.md)   
**Status**: Draft

Running my own limited companies I deal with a lot of forms that I have to print, sign, scan and email, as well as letters that I need to scan and store.

To save time I have started to build command line processes to speed up the processing time and in this post I am focusing on leveraging ImageMagick to convert images to pdf.

## Install ImageMagick
To get started you will need to install [ImageMagick](http://imagemagick.org/script/index.php) using [Homebrew](http://brew.sh)

``` Bash
$ brew install imagemagick
```

## Convert a single image to a pdf
Once you have scanned the letter and or form

``` Bash
$ convert image.tiff image.pdf
```
