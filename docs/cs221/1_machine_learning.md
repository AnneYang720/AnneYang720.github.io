# Machine Learning

## Linear Regreassion & Classification
- Score: The score on an example $(x,y)$ is how <b>confident</b> we are in this predicting.

    $f_w(x) = w \cdot \phi(x)$

- Margin: The margin on an example $(x,y)$ is how <b>correct</b> we are.

    $(w \cdot \phi(x)) y$

- Loss function: $Loss(x, y, w)$ quantifies how unhappy you would be if you used $w$ to make a prediction on $x$ when the correct output is $y$. It is the object we want to minimize. Loss minimization: learning as optimization.

    $TrainLoss(w) = \frac{1}{|Train|} \sum_{(x,y) \in Train} Loss(x, y, w)$

- Gradient $\nabla_w TrainLoss(w)$ is the direction that increases the loss the most. Gradient descent(GD):

    Initialize $w = [0, \dots, 0]$
    
    For $t = 1, \dots, T$: epochs

    &nbsp;&nbsp; $w \leftarrow w - \underbrace{\eta}_\text{step size} \underbrace{\nabla_w TrainLoss(w)}_\text{gradient}$

- Stochastic gradient descent(SGD): optimization algorithm

    Initialize $w = [0, \dots, 0]$

    For $t = 1, \dots, T$:

    &nbsp;&nbsp; For each $(x, y) \in Train$:

    &nbsp;&nbsp; &nbsp;&nbsp; $w \leftarrow w - \eta {\nabla_w Loss(x, y, w)}$

### Summary
|   | Classification  |  Regression | 
|---|---|---|
| Predictor $f_w$ | $sign(score)$  | $score$ |
| Relate to correct $y$  | margin ($score \ y$) | residual ($score - y$)  |
| Loss functions  | zero-one, hinge, logistic | squared, absolute deviation  |
| Algorithm  | SGD  |  SGD |

## Non Linear

### Features
- Feature templates: a group of features all computed in a similar way
- Hypothesis class: defined by features (what is possible)

    $F = \left\{ f_w : w \in \R^d \right\}$
- Linear classifiers: can produce non-linear decision boundaries, <b>not linear in $x$</b>

### Neural Networks
Neural networks allow one to automatically learn the features of a linear classifier which are geared towards the desired task, rather than specifying them all by hand.

![](/images/cs221/nn.png)

#### Activation function $\sigma$
- Threshold: $1 [z>=0]$
- Logistic: $\frac{1}{1+e^{-z}}$
- <b>ReLU</b>: $ReLU(z) = max(z, 0)$

#### Computation Graph & Backpropagation

Forward/backward values:
- Forward: $f_i$ is value for subexpression rooted at $i$
- Backward: $g_i = \frac{\partial \text{loss}}{\partial f_i}$ is how $f_i$ influences loss

Backpropagation algorithm:
- Forward pass: compute each $f_i$ (from leaves to root)
- Backward pass: compute each $g_i$ (from root to leaves)

### Differentiable Programming
![](/images/cs221/differentiable.png)

### Generalization
$w \in \R^d$
1. dimensionality: Reduce the dimensionality  (number of features)
2. norm: Reduce the norm (length) $\|w\|$

    $L_2$ regularization: $ \min_{w} TrainLoss(w) + \frac{\lambda}{2} \|w\|^2$

3. early stopping


### Best Practice
- Hyperparameters: Design decisions (hypothesis class, training objective, optimization algorithm) and Properties of the learning algorithm (features, regularization parameter $\lambda$, number of iterations $T$, step size $\eta$, etc.) that need to be made before running the learning algorithm

- Validation set: taken out of the training set and used to optimize hyperparameters