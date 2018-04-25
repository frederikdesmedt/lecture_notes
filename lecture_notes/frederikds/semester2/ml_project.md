Machine Learning: Project
=========================

How good a move is does not only depend on local information but could also depend on the situation at the opposite side of the board.

Difference between MCST and reinforcement learning: reinforcement learning can be a lot faster, since Q-function can be evaluated very fast.

Pipeline example: Conversion to feature -> ML -> reinforcement learning -> ...
Pipeline for:

- Learning
- Evaluating

Dataset: History of games previously played by our system, yet there might be a dataset coming on Toledo, look out for this.

Allowed to use libraries, yet the more libraries we use, the more advanced our solution should be (because we are only evaluated on the work we do ourselves).

How to work for arbitrary board sizes? Use local areas and annotate them with features of the kind "a chain starts here, ends here, etc".

Initialization (at start of game) is not included in timelimit.

Normalization is useful for defining thresholds in an increasing board size, e.g., half of the boxes are filled, rather than 10 boxes are filled.

Run multiple experiments and report what worked and didn't work.
