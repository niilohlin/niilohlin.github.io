---
layout: post
title:  "Word ladders"
date: 2020-04-18 09:23:35 +0100
categories: garbage
---

# What is a Word Ladder?

I recently stumbled upon what I believe is called "Word Ladders". It is a
sequence of words with the same length and with only one letter differing. For
example this is a word ladder of length 3 between the words "Word" and "Good"

{:refdef: style="text-align: center;"}
![Basic word ladder](/assets/images/basic_word_ladder.png)
{: refdef}

We can start asking some interesting questions using this concept. Since all
words of the same length will be connected in a undirected graph - a network -
we can try to figure out things like: What is the most "important" word in the
network? What does it mean for a word to be "important" in the graph?

## The Network

Let's take a look at how the network actually looks for words of some lengths
in English.

I'm using python, networkx and matplotlib to generate these graphs.


{:refdef: style="text-align: center;"}
![words of length 3](/assets/images/3words.png)
{: refdef}

{:refdef: style="text-align: center;"}
*The "ladder network" for words of length 3*
{: refdef}


{:refdef: style="text-align: center;"}
![words of length 4](/assets/images/4words.png)
{: refdef}

{:refdef: style="text-align: center;"}
*The "ladder network" for words of length 4*
{: refdef}

{:refdef: style="text-align: center;"}
![words of length 5](/assets/images/5words.png)
{: refdef}

{:refdef: style="text-align: center;"}
*The "ladder network" for words of length 5*
{: refdef}

We can see a pretty obvious trend here. As word length gets increases. The
network becomes less densely connected. I am going to focus on words of
length 4. Mostly because I think that it's a nice middle ground between just
connected enough to have fun connections and unconnected enough to not have a
too many totally unconnected words.

## Importance

So what is the most "important" word in the network? What does even importance
mean in this setting? If we just run a script that takes two random words in
the dictionary and tries to find a word ladder between them we get this output:

```bash
$ python word_graph.py
path from "seat" to "land"
['seat', 'sent', 'send', 'sand', 'land']
path from "tall" to "slut"
['tall', 'ball', 'bill', 'biol', 'bios', 'bros', 'pros', 'prot', 'plot', 'slot', 'slut']
path from "hurt" to "volt"
['hurt', 'hart', 'part', 'port', 'fort', 'foot', 'boot', 'bolt', 'volt']
path from "rose" to "alot"
['rose', 'role', 'roll', 'toll', 'tool', 'bool', 'biol', 'bios', 'bros', 'pros', 'prot', 'plot', 'alot']
path from "sees" to "herb"
['sees', 'fees', 'feel', 'fell', 'fill', 'file', 'fire', 'hire', 'here', 'herb']
path from "roof" to "hand"
['roof', 'root', 'boot', 'bolt', 'bold', 'bond', 'band', 'hand']
path from "laid" to "send"
['laid', 'land', 'sand', 'send']
path from "lake" to "ripe"
['lake', 'lace', 'race', 'rice', 'ripe']
path from "ward" to "noon"
['ward', 'word', 'wood', 'mood', 'moon', 'noon']
path from "proc" to "beef"
['proc', 'pros', 'bros', 'bios', 'biol', 'bool', 'boot', 'boat', 'beat', 'bear', 'beer', 'beef']
path from "leaf" to "here"
['leaf', 'lead', 'load', 'lord', 'word', 'ward', 'ware', 'were', 'here']
path from "slot" to "nude"
['slot', 'plot', 'prot', 'pros', 'bros', 'bios', 'bits', 'bite', 'bike', 'nike', 'nuke', 'nude']
path from "wake" to "with"
['wake', 'ware', 'wire', 'wise', 'wish', 'with']
```

This is just a random sample of different word ladders between words. But we
find some interesting things. The first thing that stands out is that the sub
ladder

{:refdef: style="text-align: center;"}
![Common word ladder](/assets/images/common_word_ladder.png)
{: refdef}

Seems to come up a lot. This feels like a very intuitive definition of
importance.

> The importance of a word is defined by how many direct paths goes through
> it.

We can now run a simulation where we take two random words in our network and
find the shortest path between them. If we then mark the words in the paths
we will get a score for every word representing the total number of paths using
that word.

## Unfinished

I would have liked to finish this properly. Do animations and such. Turns
out that the most common word in the word ladder is actually "bios" for some
reason. I did not have the energy to learn how to make animations using
networkx and matplotlib.
