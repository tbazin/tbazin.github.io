---
title: "_Automatic Generation of Sound Synthesis Techniques_ (Garcia 2001)"
date: 2018-11-20
tags:
  - machine learning
  - genetic programming
  - audio
  - sound synthesis
  - music
  - control
---

_Master's thesis at MIT Media Lab_

## Summary

This thesis proposes a framework for the automatic construction of so-called
Sound Synthesis Techniques (SST) based on Genetic Programming.
An SST is defined by its "_functional form_" and its "_internal parameters_".
The functionl form is the arrangement of the circuit of synthesis blocks used,
the internal parameters are relative to each synthesis block.

    > Design of SSTs is usually done by selecting a fixed functional form from a
    handful of commonly used SSTs, and performing a parameter estimation
    technique to find a set of internal parameters that will best emulate the
    target sound.

The thesis proposes a way to explore the space of functional forms using
Genetic Programming, given a set of inputs (e.g. $A(t), f(t)$) and an expected
target output. The internal parameters are optimized within a sub-loop using e.g. gradient descent on the selected functional forms ("Lamarckian evolution").

A system doing this is developed, based on a custom-designed sound engine.

Some fitness functions are proposed.

## Outcomes

The AGeSS system.

## Limitations

* The computation time for a given pair of inputs and target is extremely long
  (on order of 10 hours)
