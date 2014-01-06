---
layout: default
title: Squeak Examples
category: examples

examples:
  - short: BB2Shapes
    description: An animation of a universe of interacting objects
  - short: BB3Greed
    description: A clumsy implementation of the game of Greed
---

# DCI in Squeak

Squeak is a dialect of Smalltalk. DCI/Squeak is an extension of Squeak to support code written according to the DCI paradigm. BabyIDE is an experimental interactive development environment that supports the DCI paradigm with specialized browsers for its different perspectives. The IDE is described in section 3 in The Common Sense of Object Oriented Programming.

A ZIP file containing all you need for running Squeak/DCI in Windows XP can be downloaded from BabyIDE.ZIP. See its README file for more details.For other operating systems, you need to install Squeak on your computer. Go to http://www-1.squeak.org/ for details. The above ZIP file contains the .image and .changes files you need to browse the code and run the examples.

The code may look unreadable to a reader more used to a mainstream language such as Java or C#. The main stumbling block is the syntax for message passing.The mainstream syntax is

The Smalltalk (and Squeak) syntax is more informative

```java
receiver.selector(arg1, arg2, ...) 
fileDirectory.copy(file1, file2);
```

The Smalltalk (and Squeak) syntax is more informative

```smalltalk
receiver arg1Name: arg1 arg2Name: arg2. 
fileDirectory copyFrom: file1 to: file2.
```

Internally, the Squeak compiler immediately transforms the above syntax to the mainstream form:

```smalltalk
receiver arg1name:arg2Name (arg1, arg2). 
fileDirectory copyFrom:to: (file1, file2).
```

This works because the colon (:) is a legal character in a message selector. It follows that the number of colons in a selector equals the number of arguments.

More about the Squeak language can be found at http://wiki.squeak.org/squeak/1859.

The links below give readable listings of the Squeak/DCI examples.
The listings have been produced programmatically from a Squeak image containing the BabyIDE development environment and all examples.

{% for link in page.examples %}
- [{{link.short}}]({{link.short}}/) - {{link.description}}{% endfor %}
