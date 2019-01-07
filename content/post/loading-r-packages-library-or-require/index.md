---
title: 'Loading R Packages: library() or require()?'
author: ''
date: '2017-10-23'
slug: loading-r-packages-library-or-require
categories: []
tags: []
image:
  caption: ''
  focal_point: ''
---
When I was an R newbie, I was taught to load packages by using the command `library(package)`. In my Linear Models class, the instructor likes to use `require(package)`. This made me wonder, are the commands interchangeable? What's the difference, and which command should I use?

## Interchangeable commands . . .

The way most users will use these commands, most of the time, they are actually interchangeable. That is, if you are loading a library that has already been installed, and you are using the command outside of a function definition, then it makes no difference if you use `require` or `library`." They do the same thing.

## >... Well, almost interchangeable

There are, though, a couple of important differences. The first one, and the most obvious, is what happens if you try to load a package that has not previously been installed. If you use `library(foo)` and foo has not been installed, your program will stop with the message "Error in library(foo) : there is no package called 'foo'." If you use `require(foo),` you will get a warning, but not an error. Your program will continue to run, only to crash later when you try to use a function from the library "foo."

This can make troubleshooting more difficult, since the error is going to happen in, say, line 847 of your code, while the actual mistake was in line 4 when you tried to load a package that wasn't installed.

I have come to prefer using `library(package)` for this reason.

There is another difference, and it makes `require(package)` the preferred choice in certain situations. Unlike`library`, `require` returns a logical value. That is, it returns (invisibly) TRUE if the package is available, and FALSE if the package is not. This can be very valuable for sharing code. If you are loading a package on your own machine, you may know for sure that it is already installed. But if you are loading it in a program that someone else is going to run, it's a good idea to check! You can do this with, for example:

```r 
if (!require(package)) install.packages('package')
library(package)
```

will install "package" if it doesn't exist, and then load it.

One final note: if you look at the source code, you can see that `require` calls `library`. This suggests that it is more efficient to use `library`. As a practical matter, it seems unlikely that this would make a difference on any reasonably modern computer.
