---
layout: post
title:  "On technical debt"
date:   2020-03-04 14:23:05 +0100
categories: garbage
---

# On Technical Debt or Why All Projects Have Technical Debt.

## Leverage

In finance, there is a concept called Leverage which essentially means how
much debt you have in relation to you assets. If you have too much debt, you
are _over leveraged_. Which means you have borrowed too much in relation to
your assets to reliably handle your interest, or more than you can pay back.

This is of course not good. On the other hand you can also be what is called
_under leveraged_ which might sound good. But what it actually means is that
you are leaving money on the table. You have a great opportunity to borrow
money and do something useful with it. Money today is often more useful than
money tomorrow.

But what has this to do with technical debt? Well, everything, really.

The analogue between technical debt and fiscal debt is an apt one. It holds up
in many cases. If you have too much technical debt you can be considered _over
leveraged_. You have cowboy coded without any structure for way too long. It
got you somewhere and it did it fast. But anytime you want to make any changes
it will require a lot of work. We are all familiar with this situation.

However a situation not many consider is that you can be under leveraged in
technical debt. This can manifest itself in that you spent too much time making
your project perfect without ever delivering an MVP. You did not take advantage
of any available leverage.

So how much technical debt should you accumulate? Impossible to know. Some
projects probably has too much. Some too little. We as developers want our
codebase to feel nice to work with and spend a lot of time paying of technical
debt just for the fun of it. But we often fail to forget the expected life span
of the project we are working on. We assume that our code will live on forever,
be a glorious example of good design choices and it will live forever. However
this is rarely the case.

## Project life span

In reality your projects are usually expected to live for a couple of months to
a couple of years. With some exceptions of course. But technical dept is not
really affected if the code does not change. If the code is just run and never
touched, the dept cannot be seen as increasing. It's only if the code needs to
be changed that the dept can be considered "taking interest". So the question
is really, for how long can the code you are working on right now be considered
"actively developed"? Since we only really have one data-point for this - the
length of time in which the code has already been actively developed - we can
only assume one thing: The code will be actively developed as long as it has
already being actively developed.

For example if you are 4 months into a new project. You can only assume that it
will be developed for another 4 months. If you are 7 years into a project. You
can assume that it will live one around 7 more years.

Extrapolating from this theory. You should start out all projects fugly-hacking
everything. Just trying to get it going on a first solution. Because a week or
two in you might end up with requirement changes, or an unexpected fire in
another project or you might hit on unexpected roadblocks. And at the end of
the day. You might be closer to an MVP if you do it this way than if you try to
plan your cathedral from the start.

In other words, start by leveraging your project and pay of the debt later on
in the project.

## Discounted Cash Flow

To borrow a term from the ML-community. This line of thinking can also be
thought of as a "Discounted future reward".

In Q-learning. We discount a reward function for future rewards. Mathematically
we do this to get our reward function to converge, but there are a very natural
way of thinking about it. Since the world and the assumptions we make about it
are imperfect, we can expect that our planning about it is too.

A similar concept in finance are called Discounted cash flow.

Therefore we should mostly keep chasing the nearest goalpost which vaguely
takes us in the direction in the direction of the end goal, without trying to
prematurely optimize our path to get there a better way.

<!-- ## Something something -->

