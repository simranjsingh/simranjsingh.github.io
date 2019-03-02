## Paper: [_**Universality of Deep Convolutional Neural Networks**_](https://arxiv.org/abs/1805.10769)

*Ding-Xuan Zhou*

Shallow neural networks have been shown to be able to universally approximate any continuous functions with any arbitrary accuracy. In the 1980's and 90's it was shown that given a sufficiently large number of neurons, neural networks can approximate any continuos function. Though the number of neurons required for a good approximation acccuracy is very huge. For a vector with dimensions $$d$$ the number of hidden neurons were of the order $$O(c/\epsilon)^{d/r}$$ for an accuracy of $$\epsilon$$ where $$c$$ is a constant dependent on $$d$$. This grows exponentially with dimensions $$d$$.

CNNs overcome this limitations by using the translation-invariance property of image, speech and other data. In this paper, the author has shown that CNNs can also approximate any continuous function to an arbitrary accuracy given sufficiently large depth.

Deep CNNs have two main ingredients: the activation function which is *ReLU* in this paper and a sequence of convolution filter masks $$\boldsymbol{w} = \{w^{(j)}\}_j$$ where each filter mask is a sequence of filter coefficients $$w = (w_k)_{k=-\infty}^{\infty}$$ and $$w_k^{(i)} \neq 0$$ only for $$0 \leq k \leq s$$. The convolution of a filter mask $$w$$ with another sequence $$v = (v_0, v_1,...,v_D)$$ is then defined as a sequence with $$i^{th}$$ element as $$(w*v)_i = \sum_{k=0}^{D}w_{i-k}v_k$$. For example the third element of the resulting convolution sequence is $$(w*v)_3 = w_3v_0 + w_2v_1 + w_1v_2 + w_0v_3$$. Thus the convolution can be expressed as a matrix multiplication $$w*v = T_wv$$ where $$T_w$$ is a matrix of the form:

$$T_w = \left( \begin{array}{cccccc}
w_0 & 0 & 0 & 0 & \cdot & 0\\
w_1 & w_0 & 0 & 0 & \cdot & 0\\
\vdots & \ddots & \ddots & \ddots & \ddots & \vdots\\
w_s & w_{s-1} & \cdots & w_0 & 0\cdots & 0\\
0 & w_s & \cdots & w_1 & w_0\cdots & 0\\
\vdots & \ddots & \ddots\ddots & \ddots & \ddots\ddots & \vdots\\
\cdots & \cdots & 0 & w_s & \cdots & w_0\\
\cdots & \cdots & \cdots & 0 & w_s\cdots & w_1\\
\vdots & \ddots & \ddots & \ddots & \ddots & \vdots\\
0 & \cdots & \cdots & \cdots0 & w_s & w_{s-1}\\
0 & \cdots & \cdots & \cdots & \cdots0 & w_s\end{array} \right)$$





Notice that the number of rows is $s$ greater than columns and thus resulting sequence is of length $$D+s$$ where $$D$$ is the size of the input. And this leads us to a network with increasing widths $$\{d_j = d + js\}$$. CNN of depth $$J$$ is then defined as a sequence of $$J$$ vectors $$h^{(j)}(x)$$ on $$\mathbb{R}^d$$ given by $$h^{0}(x) = x$$ and $$h^{(j)}(x) = \sigma(T^{(j)}h^{(j-1)}(x) - b^{(j)})$$ where $$T^{(j)}_{(i, k)} = (w^{(j)}_{i-k})$$ is a $$d_j\times d_{j-1}$$ matrix and $$b^{(j)}$$'s are bias vectors. Each layer has $$3s+2$$ free parameters and in total the network has $$(5s+2)J + 2d -2s-1$$ free parameters which is much less than fully connected networks. The hypothesis class of these networks is:

$$H_J^{w, b} = \Bigg\{\sum_{k=1}^{d_J}c_kh_k^{(J)}(x): c\in \mathbb{R}^{d_J}\Bigg\}$$

and represents continuous piecewise linear functions in $$\mathbb{R}^d$$.

The author has proved the approximation for functions that belong to a Sobolev space of index $$r$$, which means that the weak derivatives of these functions exist upto order $$r$$ and have a finite $$L2$$ norm. $$L2$$ norm of a function is defined as $$\|f\|_{L^2}^2 = \int_{x\in \Omega}\|f(x)\|^2dx$$. The paper states three main theorems. First is about the ability of hypothesis class defined above to approximate any continuous function to an arbitrary accuracy.

## Theorem A

> Let $$2\leq s\leq d$$. For any compact subset $$\Omega$$ of $$\mathbb{R}^d$$ and any $$f\in C(\Omega)$$, there exist sequences $$\pmb{w}$$ of filter masks, $$\pmb{b}$$ of bias vectors and $$f_J^{w, b} \in H_J^{w, b}$$ such that
> ​	
>
> $$\lim_{J \rightarrow \infty}\|f - f_J^{w, b}\|_{C(\Omega)} = 0$$ 

The norm $$\|f\|_{C(\Omega)}$$ is defined as $$ \sup_{x\in \Omega}\|f(x)\|$$. This theorem is a result of a more general theorem:

## Theorem B

> Let $$2\leq s\leq d$$ and $$ \Omega \subseteq [-1, 1]^d$$. If $$J\geq2d/(s-1)$$ and $$f$$ is a restriction of  $$F$$ to $$\Omega$$ with $$F \in H^r(\mathbb{R}^d)$$ and an integer $$r > 2 + d/2$$, then there exist $$\pmb{w}, \pmb{b}$$ and $$f_J^{w, b} \in H_J^{w, b}$$ such that
>
>
>
> $$\|f - f_J^{(w,b)}\|_{C(\Omega)} \leq c\|F\|\sqrt{\log J}(1/J)^{\frac{1}{2} + \frac{1}{d}}$$
>
> where c is an absolute constant and $$\|F\|$$ denotes the Sobolev norm of $$F\in H^r(\mathbb{R}^d)$$

The above theorem bounds the supremum of the difference of approximated function and the original function based on the norm of the function which is finite because it belongs to the Sobolev space. Given an input of dimension $$d$$ and we want to find a CNN with filters of length $$s$$, the required number of layers is greater than $$\frac{2d}{s-1}$$ to achive the approximation accuracy mentioned above.

The key point of the paper is Theorm C below which allows us to split an arbitrarily large sequence into a convolutions of sequences of length $$s$$:

## Theorem C

> Let $$s\geq2$$ and $$W = (W_k)_{-\infty}^\infty$$ be a sequence supported in $$\{0,...,M\}$$ with $$M\geq 0$$. Then there exists a finite sequence of filter masks $$\{w^{(j)}\}_{j=1}^{J}$$ supported in $$\{0,...,s\}$$ with $$J \leq \frac{M}{s-1}+1$$ such that the convolutional factorization $$W = w^{J}*....*w^{2}*w^{1}$$.

The support of a sequence is the domain (indices in this case) where the values of the sequence are non-zero.



Theorem A can be proved by Theorem B so I will briefly talk about the proof of theorem B. First the function $$F$$ is approximated to a linear combination of activated splines by using a result from Klusowski et al., 2016 to get a function of the form:



$$F_m(x) = \beta_0 + \alpha_0\cdot x + \frac{v}{m}\sum_{k=1}^{m}\beta_k(\alpha_k\cdot x - t_k)_+$$

such that $$\sup_{x\in D}\vert F(x) - F_m(x)\vert\leq c_{F, 2}\|F\|\max\{\sqrt{log m},\sqrt{d}\}m^{-1/2-1/d}$$. Here $$m$$ is a measure of approximation accuracy, higher the $$m$$, higher the accuracy. Each $$\alpha_k$$ is a $$d$$ dimensional vector and each $$\beta_k$$ is a scalar. Now we find a possibly large $$W$$ such that $$W*x$$ captures components of $$F_m$$ as:

$$W = [W_{(m+1)d-1}...W_1W_0] = [\alpha_m^T...\alpha_1^T\alpha_0^T]$$



Note that $$(k+1)d$$th element of above sequence is $$\alpha_k\cdot x$$. Now we apply Theorem C to get $$J$$ filter masks$$w_1, w_2,..,w_J$$ of length $$s$$ such that $$W = w^{(J)}*w^{(J-1)}...*w^{(2)}*w^{(1)}$$. Then we defined bias vectors recursively so that they cancel out in each layer and then we can define the bias vector of the final layer. Each layer has $$b^{(j)} = B^{j-1}T^{j}\pmb{1}_{d_{j-1}} - B^{j}\pmb{1}_{d_j}$$ so that we finally have


$$h^{(j)} = T^{(j)}...T^{(1)}x + B^{(j)}\pmb{1}_{d_j}$$

where $$B^{(j)} = \|w^{(j)}\|_1...\|w^{(1)}\|_1B^{(0)}$$, $$B_0 = \max_{x\in\Omega}\|x\|_{\infty}$$.  We then define the $$l^{th}$$ component of bias vector of last layer as last layer as

$$b_l^{(J)} = 
\begin{cases}	
B^{(J-1)}(T^{(J)}\pmb{1}_{d_{J-1}})_l - B^{(J)} & l=d, d+Js	\\
B^{(J-1)}(T^{(J)}\pmb{1}_{d_{J-1}})_l + t_k & (k+1)d, 1\leq k\leq m \\
B^{(J-1)}(T^{(J)}\pmb{1}_{d_{J-1}})_l + B^{(J)} & \text{otherwise}
\end{cases}$$

This leads us to the activations in the final layer as 

$$h^{(J)}(x) =
​	\begin{cases}
​		\alpha_0\cdot x + B^{(J)} & l=d\\
​		B^{(J)} & l=d+Js \\
​		(\alpha\cdot x - t_k)_+ & l=(k+1)d,1\leq k\leq m\\
​		0 & otherwise
​	\end{cases}$$

the span of which contains an $$f_J^{w, b}$$ such that

$$\|f - f_J^{w,b}\|_{C(\Omega)} \leq \|F-F_m\|_{C([-1, 1]^d)} \leq 2c_0c'\|F\|\sqrt{\log J}J^{-1/2-1/d}$$

which completes the proof. For better details, you can refer to my presentation on the paper [here](https://drive.google.com/file/d/1hgAvcjYE3XsrfHL7SzMmc4XU932lBzUr/view?usp=sharing)

The paper has some limitations: it only considers a specific class of functions, considers only filters of fixed length $$s$$ and the size of the layers grows linearly with $$s$$ which can result in very wide layers. The proof above though gives us great insights into why deep CNNs work so well and is a good result in the theory of deep learning.