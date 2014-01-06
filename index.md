---
layout: default
title: DCI - Data Context Interaction
---

# DCI - Data Context Interaction
## A New Role-Based Paradigm for specifying collaborating objects

The essence of object orientation is that networks of collaborating objects work together to achieve a common goal. The common sense of object oriented programming should reflect this essence with code that specifies how the objects collaborate. Our industry has, unfortunately, chosen differently and code is commonly written in terms of classes. A class tells us everything about the properties of the individual objects that are its instances. It does not tell us anything about how these instances work together to achieve the system behavior.

The result is that our code does not reveal everything about how a system will work. This is clearly not satisfactory, and we need a new paradigm as the foundation for more expressive code. The DCI (Data-Context-Interaction) is such a paradigm. DCI separates a program into different perspectives where each perspective focuses on certain system properties. Code in the *Data* perspective specifies how information is represented by stand-alone objects. Code in the *Context* perspective specifies runtime networks of interconnected objects as similar structures of interconnected roles. Code in the *Interaction* perspective specifies how the networked objects collaborate to achieve the system behavior.

Trygve Reenskaug is the originator of DCI. Jim Coplien is its leading contributor and prime mover in the advancing DCI from early theory to accepted practice.
