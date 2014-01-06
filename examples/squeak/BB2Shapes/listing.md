---
layout: default
title: BB2Shapes
description: An animation of a universe of interacting objects
---

```smalltalk
" Data perspective "
" Class Arrow "

" See comment in Window>>Window

An ETHDemo3ArrowMorph is visible on the screen as an arrow.

Instance Variables
    endMorph:    <ETHDemo3StarMorph> The shape at the tail end of the arrow.
    startMorph:  <ETHDemo3StarMorph> The shape at the head of the arrow.
    stepCounter: <Integer> An arrow is drawn through several steps. The stepCounter counts down; the arrow is complete when stepCounter = 0. 
    stepMax:     <Integer> The number of steps used to draw a complete arrow.

"

LineMorph subclass: #Arrow
    instanceVariableNames: ''
    category: 'Shapes-Data'

" Arrow instance methods in category: access "

color: aColor
    super color: aColor.
    self borderColor: aColor.

" Arrow instance methods in category: animation "
```