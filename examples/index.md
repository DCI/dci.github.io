---
layout: default
title: Examples
weight: 3
---

# Examples

This page is a repository for examples of DCI code. Programmers often let their code go through three stages:

> Make it work. Make it right. Make it fast.

'Make it work' means that the code does what it is supposed to do and that there are no known bugs.

'Make it right' means that the code is easy to read and understand. We assume that the reader's mental model is founded on the DCI paradigm and that the code clearly exhibits this paradigm.

'Make it fast' comes latst and is not our concern here.

Our yardstick for evaluating the code is how it supports the reader's mental model. Classes are classes as they have always been. Only the DCI classes can be much simpler since they are static and do not describe system behavior. System behavior is realized by Contexts, each Context implements a system operation (which may be derived from a use case). A Context encapsulates a network of interacting objects. The objects are represented by the Role they play in the Interaction. The Roles are aliases for the objects so that a reader can think in terms of interacting Roles rather than concrete objects. The reader should never be surprised by some kink in the implementation.

A system operation is realized by the collaboration of the participating Roles. The behavior of any object played by a Role is given by its RoleMethods. The RoleMethods are parts of the Roles and, indirectly, of the participating objects. A RoleMethod overrides any instance methods with the same name of these objects. 

A Context is the namespace of its Roles. A Role is the namespace of its RoleMethods. This means that a Role is not visible from outside its Context, and a RoleMethod is not visible from outside the Role.

In the following, the examples are organized by programming language. Each set of examples is introduced by a short description of the language and its conventions for implementing DCI.

{% for p in site.pages %}
{% if p.category == "examples" %}
- [{{p.title}}]({{p.url}})
{% endif %}
{% endfor %}
