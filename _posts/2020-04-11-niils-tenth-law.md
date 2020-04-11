---
layout: post
title:  "Niil's Tenth Rule of Programming"
date: 2020-04-11 12:45:35 +0100
categories: garbage
---

# Greenspun's Tenth Rule of Programming

> Any sufficiently complicated C or Fortran program contains an ad-hoc,
> informally-specified, bug-ridden, slow implementation of half of CommonLisp.

I recently came across this quote from 1993 by Philip Greenspun. It describes
how many programs, tools, and programming languages slowly evolves all the
tools and language features from Lisp in bad ways. In many ways this is still
true. Although modern programming languages have usually embraced some of the
most useful parts of Lisp such as functional programming, they have not gone
all in on the Lisp syntax and meta programming functionality of Lisp.

There is more modern version of this law which I chose to call "Niil's tenth
rule of programming"

## Niil's Tenth Rule of Programming

> Any sufficiently complicated asynchronous program not using an FRP library
> contains an ad-hoc, informally-specified, bug-ridden, slow implementation of
> half of Rx.

There are a couple of caveats here. Of course there are different models of
asynchronous programming. A program written in Erlang most likely won't be
anything similar to Rx. And I'm including GUI programs within "any asynchronous
program".

This is because as a project grows and it has to handle more and more state,
usually utility functions are added to the project that more and more resembles
functional reactive programming patterns.
