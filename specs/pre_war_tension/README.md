# Pre-War Tensions

*Author*: A. West  
*Date*: 2023-Dec-01


## Context

From the beginning the RPG's world has been envisioned as composed of
spatially contiguous subworlds. Some are situated in the past, some in
the future, some historical, some fictional, some
speaking English, some speaking a foreign language,
and some polyglot. A character passing through different subworlds would
thus have to deal with challenges particular to each.


## Overview

This document outlines the design of a historical subworld consisting
of two groups of mutually hostile characters. The goal of each character
is to use its group's slogan to try to recognize friend or foe, *ie*,
to classify other characters as belonging
either to the same group or to the opposing one.
The purpose of this
exercise is to see whether these dynamics will give rise
to spatial segregation of the groups as an emergent property.
The details are as follows.

When characters *A* and *B* are in the same location, one of them,
assumed without loss of generality to be *A*, spontaneously whispers
its slogan to *B*. Then *A* waits for *B*'s response, and reacts as follows:
* If *B* recognizes the slogan as its own, it smiles at *A* and classifies
  *A* as friend. *A* reciprocates likewise.
* If *B* does not recognize the slogan, it scowls at *A* and
  classifies it as foe. *A* reciprocates likewise.
* If *B* does not respond, *A* scowls at *B* and classifies it as
  a potential foe.

Because smiling and scowling affect health, a character should leave the
current location if it receives too many scowls. 

To convey real-world feeling,
the two groups should belong to a well known historical conflict. To prepare
for a future application of natural language processing (NLP), a slogan
should consist of two short rhyming halves. For example if the setting is
just prior to World War II, the English
slogan might be "Kill a Kraut, wipe 'im out." (Slogans should use
historically realistic language.)

The first implementation will set the scene in the American colonies of
the late eighteenth century. The American slogan will be "Kill a Brit,
show some grit." The English will be "Kill a Yank on the bank" (of the
Hudson River).

A slogan is to be distinguished from other speech by the prefix "Watchword:".
For example, a German opposed to France just prior to World War I might whisper,
"Watchword: Put a Frog in a bog."


## Goals

The following are this project's goals:
* To give a live player more variety of experience in the RPG.
* To discover the parameter space that gives rise to
  emergent segregation. Parameters might include the density of
  characters, the time
  elapsed before one whispers its slogan to another, and the
  threshold past which a character being scowled at will move to
  another place, *ie*, how much "heat" it can take before it has
  to "get out of the kitchen".
* To challenge a live player to sense whether segregation
  has emerged and, if so, whether he or she can identify the
  group dominating a set of places.


## Non-Goals

The first stage of development will not attempt to use NLP.
In a subsequent stage, characters will be able to propose
a change of slogan to fellow group members. NLP will be needed to verify
that, like the examples above, it consists of two short rhyming halves.


## Existing Solution

This feature is currently unimplemented. There are two
candidates in which to develop it. One is the `agent` service implemented
in Haskell; the other is the web-scraping service implemented in Python.
An earlier version of the Python code separated common functionality
out into its own Python package, facilitating reuse in any future
stand-alone Python service.


## Proposed Solution

We propose to implement this feature in neither of the Haskell and
Python services just mentioned. That would conflate purposes.

Instead these characters of "pre-war tension" are to be implemented
in their own service. We propose to do so in Python, because of the
factored-out common functionality just mentioned, because Python
facilitates rapid prototyping, and because Python's large ecosystem
offers much functionality for AI in general and NLP in particular.


## Alternative Solution

This feature could alternatively be implemented in Java. It would
be served well by the virtual threads of Java 21, and it would
not need a full-fledged framework such as Spring. On the other hand
Java uses more memory and OpenJDK alone has a large-ish footprint
on disk (a compressed Docker image of 200-400 MB).

Another alternative language might be Go. Compared to Java it has a
faster startup time, uses less memory, and has a smaller footprint.
Its goroutines would be similarly well adapted to this feature.
It is however not as expressive a language as Java is, and its
ecosystem may have some catching-up to do in AI and NLP. But these
deficits might not be decisive here.


## Testability

Unlike the Python webscrapers, this service will have to be a
full-fledged RPG driver. The game engine itself will have to
be mocked, and more `asyncio` mocking will be needed in unit
tests to verify correctness of individual characters' behaviors.

Integration testing would require a large ensemble of experiments
conducted in one or more instances of the RPG. Each would apply
a clustering algorithm to detect segregation. Since we lack the
resources to undertake such exhaustive tests, the
feature will have to be tested by a live player instead.


## Scoping and Timeline

Refurbishment of the old "common" Python packages will take
a single software developer approximately three 8-hour days.
Since these packages are somewhat bit-rotten, this figure
and the following ones are not perfectly reliable.

The addition of driver functionality to the Python client code
will take approximately 5 days: one to implement the mock engine,
two to write unit tests against it, one to implement the actual
driver functionality, and one to debug and make the tests pass.

Then the character stereotypical of this feature will have to
be added. It will take approximately 2 days to write the unit tests,
and another 2 to implement the actual characters. Functionality from
the scraping bots should be reused as much as possible.

A pass will have to be made over the code to document it. This will
take approximately 1 day.

Then the service will have to be containerized: there should be
at least two Dockerfiles, for production and for local development.
This will take approximately 2 days.

The time required for live testing is unknown. It may conceivably
take weeks of parameter-tuning to observe segregation;
each test run will require restarting the container and waiting.

In short, the projected timeline for implementation, unit testing,
and Dockerization, is about two weeks. The time needed for live
testing is unknown.
