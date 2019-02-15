## Paper: [_**Prior Convictions: Black-Box Adversarial Attacks with Bandits and Priors**_](https://arxiv.org/abs/1807.07978)

*Andrew Ilyas, Logan Engstrom, Aleksander Madry*

This work attempts on solving the problem of generating adversarial examples in a black-box setting with only loss-oracle access to a model. The works seeks to leverage prior information about the gradient of the objective function and incorporates the priors with an algorithm based on bandit optimization. In particular, the work demonstrates two examples of priors - time based and data based and shows its effectiveness in gradient estimation and subsequently reducing the number of queries to the model.

Recent research has been focused on generating adversarial examples that are misclassied by a CNN. Highly effective attacks have been developed for white box settings where adversary has access to model and its internals including gradients. Now research is focused on breaking models in black box setting where only the class probabilities, or in the harder case, only class labels are available. The attacks can be targeted, i.e getting a particular class as an output or untargeted where the concern is only getting an incorrect output from the classifier.



The problem is formalized as finding a $$x'$$ given a pair $$(x_0, y_0)$$ and that maximizes the classifier loss function $$L: X \times Y \rightarrow R$$ but is within a given range of $$x_0$$:

$$x' = argmax_{x':||x'-x||\leq\epsilon}L(x', y)$$

where $$X$$ and $$Y$$ are domain and range of the classifier respectively and $$\epsilon$$ is a threshold of deviation from original example.

A very interesting idea that I encountered in this paper is that of gradient estimation. Gradient evaluation is a highly query intensive task: to estimate a gradient at a point one needs to make queries of the order of number of pixels, and hence makes the application of gradient descent infeasible. Instead the paper focuses on getting a good estimate of the gradient and then employ projected gradient descent to solve the problem. The gradient is estimated as a solution to the least squares problem where we want to find a vector that has maximum inner product with the gradient. If we take a random unit vector $$u_t$$, then the directional derivative in that direction is given by $$\hat{g}^Tu_t$$ where $$\hat{g}$$ is the gradient at that point. The directional derivative can be easily approxiamated with finite difference methods. If we have k such equations then the gradient can be estimated by:

$$min_{\hat{g}} ||\hat{g}||^2$$

st. $$A\hat{g}=y$$, where A is a matrix with unit vectors as rows and y is vector of length k containing directional derivatives. If k is very low as compared to the dimensions of the image, then this gradient estimation is very helpful. The paper shows that this estimation is equivalent to the Natural Evolution Strategies (NES) based estimation of gradient.

In addition to this estimation, the paper incorporates time and data dependent priors to estimate gradients. The authors argue that the gradients during the minization process are highly correlated and hence the gradients upto iteration $$t$$ can be employed to estimate gradient at iteration $$t+1$$. This argument is validated by plotting the cosine similarity between gradients estimated in the trajectory stays at around 0.9.

The next prior is based on the structures of the input (image at that point). The argument here is that the values of gradient at two pixles very close to each other will be close. This is quantified by comparing a downsampled or blurred version of gradient with itself. The analysis points to the fact that the dimensionality of the problem can be reduced by a factor of $$k^2$$ by performing a pooling operation with a filter of size k and still get a good estimate of the gradient.

Finally the framework incorporated by the authors is that of bandit optimization (Hazan, 2016) wherein an agent plays a game where he has to incur a loss of every action he choses in each round and the objective of the agent is to reduce average loss over the game. The agent will then have to keep track of certain information in an aggregated manner. The action in our particular case is estimating gradient and loss corresponds to the inner product between the gradient and its estimation.

**Results**: This work reduces the number of queries by two to four times on ImageNet attacks on Inception v3 and is two to five times less prone to failure than the previous state of the art. The number of queries are maxed at 10000 queries. This is a great improvement in the black box setting. It should be noted here that the method is not full proof and has a non zero failure rate.

**Conclusions**: This paper successfully incorporates prior information about gradients using a bandit based optimization framework to estimate the gradient of the loss funciton in the black box setting for adversarial attacks.