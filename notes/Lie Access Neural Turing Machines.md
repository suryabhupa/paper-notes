# Lie Access Neural Turing Machines
***Greg Yang, Alexander M. Rush (2016)***

(Warning: Brief -- see more details in the paper).

This paper takes a very mathematically elegant take on external memory for use by neural networks. Namely, the authors replace the traditional tape-based memory approach used in previous work by mapping memories to a continuous manifold. In addition, in order to navigate the manifold to read and write memories, the authors use Lie group actions, such as rotations about a sphere or translation on a 2D plane. They find that when compared to several baselines, the Lie Acces Neural Turing Machine (LANTM) offers great improvements on many simple algorithmic tasks, including repeat copy, priority sort, etc.

The primary contributions of this work are the following:
* Using Lie group actions instead of convolutions to manipulate read and write head; mathematically, this assures that all actions are associative and invertible, and assures the identity transformation, none of which are guarenteed by the original formulation of the NTM (and later ones too).
* Using manifolds instead of traditional tape-based systems to store memories; this guarentees local smoothness and naturally provides a distance metric.
* Use of inverse-square and comparison to softmax weighting of memories when reading.

Open questions:
* How to address the unbounded memory; because manifolds are continuous, any number of memories can be written, but this obviously isn't practical. Several solutions exist for this; delete memories using an LRU-like cache, etc.
* Certain manifolds and Lie groups may offer better performance on certain problems, and different options have yet to be explored.
* RAM-based memory access is built into the model; it would be very interesting to see if the model using the location-based addressing would be sufficient.
