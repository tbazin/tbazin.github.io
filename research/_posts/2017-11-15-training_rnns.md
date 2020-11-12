---
title: "Notes on training RNNs"
date: 2017-11-15
tags:
  - machine learning
  - training
  - models
  - RNNS
---

## Sliding window and BPTT

### How to devise window size and rate of updates during training
Source: [StackExchange: When to apply BPTT or update weights](https://stats.stackexchange.com/questions/219914/rnns-when-to-apply-bptt-and-or-update-weights/220111#220111)


> 1. Forward pass: Step through the next $k_1$ time steps, computing the input,
    hidden, and output states.
> 2. Compute the loss, summed over the previous time steps (see below).
> 3. Backward pass: Compute the gradient of the loss w.r.t. all parameters,
    accumulating over the previous $k_2$ time steps (this requires having stored
    all activations for these time steps).
    Clip gradients to avoid the exploding gradient problem (happens rarely).
> 4. Update parameters (this occurs once per chunk, not incrementally at each
    time step)
> 5. If processing multiple chunks of a longer sequence, store the hidden state
    at the last time step (will be used to initialize hidden state for
    beginning of next chunk).
    If we've reached the end of the sequence, reset the memory/hidden state and
    move to the beginning of the next sequence (or beginning of the same
    sequence, if there's only one).
> 6. Repeat from step 1.

> Gradient computation and updates are performed every $k_1$ time steps because
it's computationally cheaper than updating at every time step.
Updating multiple times per sequence (i.e. setting $k_1$ less than the sequence
length) can accelerate training because weight updates are more frequent.

> Backpropagation is performed for only $k_2$ time steps because it's
computationally cheaper than propagating back to the beginning of the sequence
(which would require storing and repeatedly processing all time steps).
Gradients computed in this manner are an approximation to the 'true' gradient
computed over all time steps.
But, because of the vanishing gradient problem, gradients will tend to approach
zero after some number of time steps; propagating beyond this limit wouldn't
give any benefit.
Setting $k_2$ too short can limit the temporal scale over which the network can
learn.
However, the network's memory isn't limited to $k_2$ time steps because the
hidden units can store information beyond this period (e.g. see Mikolov 2012
and [this post](https://stats.stackexchange.com/questions/167482/capturing-initial-patterns-when-using-truncated-backpropagation-through-time-rn)).

### Memory capacity of a truncated RNN
Source: https://stats.stackexchange.com/a/167800

> Limiting the gradient during training is more like limiting the window over
which your model can assimilate input features and hidden state with high
confidence.
Because at test time you apply your model to the entire input sequence, it will
still be able to incorporate information about all of the input features into
its hidden state.
It might not know exactly how to preserve that information until it makes its
final prediction for the sentence, but there might be some (admittedly weaker)
connections that it would still be able to make.
