## Learning to Execute
***Wojciech Zaremba, Ilya Sutskever (2015)***

The core problem this paper attempts to solve is learning to execute simple programs from scratch (luckily, we can gather this from the aptly named title). That is, given the text representation of a simple program, the system should learn to execute the program instructions and produce the correct output with limited prior knowledge about the program semantics and syntactics and in a fully data-driven process.

When posing this problem in its simplest form, there are a number of different ways one may consider to approach it, and I think as a researcher, it's important to step back and think about different kinds of approaches and evaluate their merits and shortcomings. Various options are listed here:

1) If we remove the condition of limited prior knowledge, we might want to try to encode the semantics of simple operators into our system. For example, we want to give our system access to a set of operators or functions that it can learn to call, such as a function for adding a non-empty list of numbers, or

This paper really speaks to the various things I've been thinking about lately, albeit in a simple, relatively straightforward way.

**Summary**

**Main Contributions**

**Thoughts**

