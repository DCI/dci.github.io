---
layout: default
title: BB3Greed
---

# Visualizing a simple process with Roles and Interactions

The animation program, BB2Shapes, shows a system as a colored field with objects as shapes within this field. It has two use cases that visualize the dynamic nature of system behavior, ShapesAnimation and ArrowsAnimation. These animations are designed to pose a challenge: Find a way to describe how the visualized system works. We find the basic class based paradigm inadequate, and introduce the new DCI paradigm; the Data-Context-Interaction paradigm.

The ShapesAnimation visualizes how objects come and go during an execution by making shapes appear and disappear randomly within the system field. The objects can be instances of different classes; this is visualized by stars and circles. Click here to watch a movie of the running Squeak program.

The ArrowsAnimation visualizes a runtime structure that "consists of rapidly changing networks of communicating objects". It visualizes that messages are sent from object to object by showing arrows that grow from sender to receiver objects. Click here to watch a movie of the running Squeak program. The stars and circles are instances of different classes and they appear at random in both animations. The classes are irrelevant and do not reveal everything about how the system works. The class based object paradigm is inadequate and we need to extend it to be able to describe a process involving networks of communicating objects. The ArrowsAnimation visualizes the dichotomy between the static system state and the dynamic system behavior. Dynamically, the process of selecting objects and drawing arrows between them repeats itself again and again, but the process selects different objects every time around the loop.

