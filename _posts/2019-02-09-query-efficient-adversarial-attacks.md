A novel approach to generate adversarial images for machine learning model in a black box setting, this is the current state of the art approach in terms of queries.
![equation](http://latex.codecogs.com/gif.latex?O_t%3D%5Ctext%20%7B%20Onset%20event%20at%20time%20bin%20%7D%20t)

$$ e^{ix}=cos(x)+isin(x) $$
​	There has been a lot of work recently to study the robustness of deep neural networks. An adversarial example is a an image that is very close to another image but is classified differently by the CNN than the original image. The difference between two images is mostly imperceptible by humans. The most famous example is that of a modified STOP sign image among traffic symbols that appears as STOP sign to humans but is misclassified as some other symbol by a CNN. As evident this can lead to disastrous implications and hence the neural networks need to be made robust to such attacks.

​	These attacks are studied in 3 broad settings based on the accessibility of the model being attacked. First is the white box setting where the attacker has access to the internals of the neural networks - weights/gradients. This has been the most common case of study in the early phases of adversarial attacks research. Here model can be attacked by a simple gradient descent in input space starting from a sample until it is misclassified by the classifier. This is a rare case in a real world setting. Second case is the soft label setting where probabilities for each class are provided for a given input query. This is observed in most real world classifier and has been dealt with various approaches like finite difference based gradient descent to find the input that is misclassified. The next case is the hard label setting which is very difficult to crack since the output is a non continuous step function which is insensitive to small perturbation in inputs. Previous works have trained substitute models by querying original models but transferring the attack from substitute to original model is not very efficiently achieved.

​	This paper deals with the hard label setting and overcomes the problem of discontinuity of hard labels by defining a new continuous function g(theta) and optimizing it.

​			$g(\theta) = \argmin_{\lambda>0} f(x_0 + \lambda\theta) \neq y_0$

This function gives the minimum distance from sample $x_0$ in the direction theta that gives us a mis classified example. Then we achieve the optimum adversarial example by minimizing $g(\theta)$. This function is claimed to be continuous since small change in theta will lead only to small change in distance to boundary. This continuity is the gist of the paper.

​	$g(\theta)$ is calculated by a linear search first, evaluating  $f$ on $\{x_0 + a\theta, x_0 + 2a\theta,...,x_0 + ra\theta\}$ until a misclassified example is achieved. Then a binary search is performed in $\{x_0 + (r-1)a\theta, x_0 + ra\theta\}$. An optimization is added in algorithm to start for a new $\theta$ from the value of g previously evaluated on a $\theta_0$’ very close to $\theta$. The function g is minimized using Randomized Gradient-Free method.

Results: This paper presents the current state of the art in breaking a black box classifier in terms of queries. This method can also be extended to other black box classifiers, as the authors have shown its use in attacking gradient boosting decision tree classifier.
g
Future scope: Although an improvement, there is still a lot of scope in improvement of number of queries  which are of the order of tens of thousands for simple MNIST and CIFAR-10 datasets.