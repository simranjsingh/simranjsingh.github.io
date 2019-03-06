## Paper: [_**Critical Learning Periods in Deep Networks**_](https://arxiv.org/abs/1711.08856)

*Alessandro Achille, Matteo Rovere, Stefano Soatto*

The connection between neural networks and how the human brain works has always fascinated me. Though a bunch of neurons put together and trained by back propogation does not represent a biological system, there are interesting insights in the paper that point out similarities in the way neural networks and animals learn. This paper was published in ICLR 2019.

As per the findings in the paper, DNNs exhibit critical periods during learning similar to animals. If the network is provided with information deficit during these periods, it can have an impact on the network that cannot be undone by further training i.e. *temporary stimulus deficit can impair the development of a skill*. 

Inspired by the affect of amblyopia on humans, wherein visual acuity is reduced in one eye to cataracts during infancy or childhood, authors perform experiments on training DNNs for CIFAR-10 classification by blurring the images for some initial epochs and then continuing training normally thereafter. It was observed that DNNs infact exhibit critical period and if the blur is not removed in the first 40-60 epochs, the final performance is severly affected as compared to baseline. However these periods are not caused by high level deficits such as vertical flipping of the image.

Authors use the Fisher Information Matrix of weights to study the information processed by the network. It is a metric that measures how much perturbation of a single weight can alter the output of the network and is measure of the quantity of the training data information contained in the network. Experiments showed that there is an initial phase when the strength of network connections increases rapidly and then the overall strength of the connections start decreasing. Authors argue that this can be seen as **forgetting** or **compression** during which redundant information is discarded and points to similarities with human learning, where forgetting is essential. The performance of the network counter-intuitively keeps on increasing.



An interesting observation of layer wise information distribution is that the stronger connections are found in the intermediate layers, which can be seen as the most informative level. However when trained with blurred data, initially the connections strength is strongest at top layer since data is smooth and then this part shifts to the intermediate layers when proper training data is provided. This reorganization of the network is referred to as __Information Plasticity__ by the authors.

Authors mention that the initial rapid memoization phase is followed by a loss of Information Plasticity which improves performance.

The paper hence draws interesting parallels between the way DNNs learn and the way humans learn. I personally believe that insights from human learning and other biological systems can help boost DNNs performance. Human brain is a result of ages of modifications and is undoubtedly a successful model and hence can be a great guide towards boosting neural networks.