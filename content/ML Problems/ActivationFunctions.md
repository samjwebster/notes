---
tags:
    - nc-ml
title: Activation Functions; Sigmoid and ReLU
---

Activation functions are what enable multidimensionality in ML models - otherwise, everything would be a combination of linear functions, which are always reduceable to a single linear operation. 

## Sigmoid

Sigmoid smooths the input between 0 and 1 with a curve, where sigmoid(0) = 0.5. Positive numbers continually approach 1, while negative numbers continually approach 0. 

## ReLU (Rectified Linear Unit)

ReLU returns 0 for any input less than or equal to 0 and the input itself otherwise. 

## Code

```
import numpy as np
from numpy.typing import NDArray


class Solution:
    
    def sigmoid(self, z: NDArray[np.float64]) -> NDArray[np.float64]:
        # z is a 1D NumPy array
        # Formula: 1 / (1 + e^(-z))
        # return np.round(your_answer, 5)
        return np.round(1 / (1 + np.exp(-z)), 5)


    def relu(self, z: NDArray[np.float64]) -> NDArray[np.float64]:
        # z is a 1D NumPy array
        # Formula: max(0, z) element-wise
        return np.maximum(0, z)
```