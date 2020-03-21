---
layout: post
title:  "How programmers stop an outbreak"
date:   2020-03-21 14:08:05 +0100
categories: garbage
---

There is currently an outbreak going around the world in the form of the
COVID-19 virus. Governments and institutions are doing their best to contain
the outbreak. People are doing their best to self-quarantine.

## Programming

Programmers are constantly trying to contain the spread of bugs. Over the years
programmers have developed multiple ways to do this. Bugs, legacy code and
errors are inevitable. Let's take a look at what governments and institutions
can learn from programmers.

### Tests

The first and probably most important tool a programmer has to to make sure
that new bugs don't spread in the code base is _unit tests_. This would probably
correspond to the regular decease testing that the governments are doing.

A good unit test tests a behaviour's "happy path", i.e. that the expected piece
of code works in the way it's supposed to. When the behaviour is changed or
breaks then the happy path test should catch the error. In the viral realm,
this would correspond to a test that has a low false negative rate. A test
which leaves little to no error if a patient is infected.

A good unit test suite also has tests for the "unhappy path" or error "error
path", i.e. tests that expect errors and detects when errors are not given.
These tests would correspond to a test that has a low false positive rate.

Another set of tests are _integration tests_. These usually test multiple
behaviours in one single test. Having integration tests usually increases
coverage, but decreases specificity. So if an integration test catches an
error, there might still be need to debug to find out where exactly the error
came from.

This might be likened to mixing a couple of people's saliva and testing them
together. If 5 people's mixed test come back positive for an illness. Then
you would have to test everyone of them individually. But if they come back
negative. You have saved 4 tests and a lot of time.

### Separation of Concerns

Another thing that programmers try to achieve in any code base is a good
_separation of concerns_. That means that any one part of the code only "knows"
or "touches" the parts that they absolutely need to. The main reason for this
is that isolated code is easier to reason about. But it has the added benefit
that once a bug is discovered. It's more likely to be localized to only one
part of the code base.

During a pandemic, we instead practice social isolation and we close borders.
This is for the same reasons. If we find a "bug" (an infected person). And we
know where that person has been. We can easily find the source of the infection
and treat the infected people.

### Continuous Integration

One really important thing that keeps bugs away is _Continuous Integration_ -
the practice of automatically building and testing all code every time a
change is made. Not only the code that's in the "risk zone" or the code that
changed. But all code, every day.

That would mean to continuously test as many people as possible. Even people
who have previously been sick and gotten better. And not stopping. This is the
strategy employed by South Korea, with great success. Although it is evident
that's it's not really possible as not all countries has access to tests.

## Conclusion

I could go on with a couple more analogies. But I feel like I'm stretching
the analogy a little bit too thin. Another couple of examples:

* Linting - Hand washing
* Code review - Travel restrictions
* Debuggers - antivirals/antibiotics
* Pure functions - Total isolation

The main takeaway is that testing is the primary measure to keep the pandemic
under control while respecting people's freedoms.
