---
tags:
    - nc-ml
title: Gradient Descent
---

## Problem

How does the core optimization algorithm for neural networks? It's pretty simple!

The idea of gradient descent, at its most basic, states: 

    For the loss surface of this model during training, and given the current position on it, "descend" to a lower value of loss. That is, whatever the gradient a.k.a direction of positive change on the loss surface is, go down (subtract) by that direction

This problem on Neetcode asks you to implement how this would look in a very simple case where the loss surface is one dimensional and trivially defined: `f(x) = x^2`. The derivative of this function is `f'(x) = 2x`. 

Implement a method that, given a number of iterations, a learning rate, and an initial position, write a gradient descent optimizer for `f(x) = x^2`.

## My solution

```
class Solution:
    def get_minimizer(self, iterations: int, learning_rate: float, init: int) -> float:
        # Objective function: f(x) = x^2
        # Derivative:         f'(x) = 2x
        # Update rule:        x = x - learning_rate * f'(x)
        # Round final answer to 5 decimal places

        def der(x):
            return 2*x

        res = init

        for i in range(iterations):
            res = res - learning_rate*der(res) # Descend (subtract) by learning rate at the direction of the derivative at the current position
        
        return round(res, 5)
```