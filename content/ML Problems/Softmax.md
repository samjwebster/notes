---
tags:
    - nc-ml
title: Softmax and Temperature
---

## Softmax

Softmax converts an array of logits into probabilities. This is useful for classification tasks; the class-associated logit with highest probability is the predicted class, for example.

Let's process Softmax on the following array:
`arr = [1.0, 2.0, 3.0]`
1. Subtract the `max` from all elements. In this case, it is `3.0`
`arr = [-2.0, -1.0, 0.0]`
2. Exponentiate all elements. That is, `val` -> `e ** val`.
`arr = [np.e ** -2.0, np.e ** -1.0, np.e ** 0.0] = [0.1353, 0.3679, 1.0]`
3. Calculate the sum of all exponentiated shifted logits:
`sum = 0.1353 + 0.3679 + 1.0 = 1.5032`
4. Normalize all values by the sum; that is, divide them by it. This final array is the probabilities at each logit, and all elements sum to 1.
`arr =  [0.1353, 0.3679, 1.0]/1.5032 = [0.0900, 0.2447, 0.6652]`

### Temperature

Not asked for in the Neetcode implementation, but another parameter that can be used in softmax is temperature. It essentially sharpens or smooths the probability curve according to value. 

To add temperature in the prediction, divide the raw logits by the parameter:
```
z = get_raw_logit_scores() # get your raw scores by doing whatever, prediction
T = 1.5 # higher temmperature, for example. Will smooth the probability curve.
probs = softmax(z/T) # divide the raw scores by the temperature
```

Larger temperature values (> 1.0) will smooth out the probability curve. In calculation, it divides all raw scores, which doesn't affect their relative distance but lowers the overall range of the scores. After subtracting max(logits) and exponentiating, we're working at lower values of the `e**x` function, which has a flatter slope. This results in the final probability curve being smoother and perhaps less dominated by one or few high-value logits.

Similarly, lower temperature values (< 1.0) will sharpen the probability curve. Dividing by a value less than one increases a number, so it raises all raw scores to a higher range. When the softmax calculation is applied, it again is functioning in a different range of the `e**x` function. At higher values, the `e**x` function has a very steep slope. This result emphasizes logits with high raw probabilities over ones with relatively lower probabilities, sharpening the final predictions.

## Implementation

This basic python implementation achieves the Softmax algorithm as described above, rounded to 4 decimal places:

```
import numpy as np
from numpy.typing import NDArray


class Solution:

    def softmax(self, z: NDArray[np.float64]) -> NDArray[np.float64]:
        # z is a 1D NumPy array of logits
        # Hint: subtract max(z) for numerical stability before computing exp
        # return np.round(your_answer, 4)
        z -= np.max(z)
        z = np.e ** z
        s = np.sum(z)
        return np.round(z/s, 4)
```


