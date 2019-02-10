## Paper: [_**Query-Efficient Hard-label Black-box Attack: An Optimization-based Approach**_](https://arxiv.org/abs/1807.04457)

This paper presents a novel approach to generate adversarial images for machine learning model in a black box setting and is currently the state of the art in term of number of queries required to make a successful adversarial attack.

​There has been a lot of work recently to study the robustness of deep neural networks. An adversarial example is a an image that is very close to another image but is classified differently by the CNN than the original image and the difference between two imperceptible by humans. A famous example is that of a modified STOP sign image among traffic symbols that appears as STOP sign to humans but is misclassified as some other symbol by a CNN classifier. As evident, this can lead to disastrous implications and hence the neural networks need to be made robust to such attacks.

​These attacks are studied in 3 broad settings based on the accessibility of the model being attacked. First is the white box setting where the attacker has access to the internals of the neural networks - weights/gradients. This has been the most common case of study in the early phases of adversarial attacks research. Here the model can be attacked by a simple gradient descent in input space starting from a sample until it is misclassified by the classifier though this is a not a common case in a real world setting. Second case is the soft label setting where probabilities for each class are provided for a given input query. This is observed in most real world classifier and has been dealt with various approaches like finite difference based gradient descent to find the input that is misclassified. The next case is the hard label setting which is very difficult to crack since the output is a discontinuous step function which is insensitive to small perturbation in inputs. Previous works to tackle this case have trained substitute models by querying the original model but transferring the attack from a substitute to original model is not very efficiently achieved.

​This paper deals with the hard label setting and overcomes the problem of discontinuity of hard labels by defining a new continuous function and optimizing it:

$$g(\theta) = argmin_{\lambda>0} f(x_0 + \lambda\theta) \neq y_0$$

This function gives the minimum distance from a source image $$x_0$$ in the direction $$\theta$$ that gives us a mis-classified example. Then we achieve the optimum adversarial example by minimizing $$g(\theta)$$ over $$\theta$$. This function is continuous since small change in theta will lead only to small change in distance to boundary and this continuity is the gist of the paper.

​$$g(\theta)$$ is calculated by a linear search first, evaluating  $$f$$, the CNN model, on values $$\{x_0 + a\theta, x_0 + 2a\theta,...,x_0 + ra\theta\}$$ until a misclassified example is achieved. Then a binary search is performed in the range $$\{x_0 + (r-1)a\theta, x_0 + ra\theta\}$$. An optimization is added in the algorithm: To calculate for a new $$\theta$$ which is very close to $$\theta'$$, the value of $$f$$ at $$g(\theta')$$ is calculated and search is performed towards $$x_0$$ or away from it depending on the value of $$g(\theta')$$. 

To minimize $$g$$, a randomized gradient free method based on the Gaussian oracles defined by Nesterov. The algorithm, though very simple to implement, expectedly converges to a minimum.

**Results**: This paper presents the current state of the art in breaking a black box classifier in terms of queries. This method can also be extended to other black box classifiers, as the authors have shown its use in attacking gradient boosting decision tree classifier.

**Future scope**: Although an improvement, there is still a lot of scope in improvement of number of queries  which are of the order of tens of thousands for simple MNIST and CIFAR-10 datasets.