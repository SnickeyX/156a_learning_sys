---
id: 5chp3hefvxek92fdjlngqlu
title: Wk5_linear2_neural_net
desc: ''
updated: 1698456751076
created: 1698428867086
---
# Linear Model 2 (L9)
What we have done:
- linear classification (perceptron algorithm)
- linear regression 
- non-linear transformations (except analysis on generalization)

# Nonlinear transforms
Recall that if we are able to transform a nonlinear input space with some function $\phi$ to a linear space, then finding a hypothesis $g$ in the input space is just: $g(x) = \text{sign}( \tilde{w}^T \cdot \phi(\bold{x}))$ 
(tilde weight is just weights in the transformed space than the input space!)

## The price of nonlinear transforms
- from the VC dim analysis, we know that for a $d$ dimensional point in the input space, we get a $d_{vc}$ = $d+1$.
- after we have a transformation $\phi$, and go from some $x$ to some $z$ ($Z$ space denoting the transformed space), the new dimensionality of the points in $Z$ will be greater since we are adding extra information to describe the nonlinearity better. That is to say, $\tilde{d} > d$, and so the VC dim of the $Z$ space is $d_{vc} \leq \tilde{d} + 1$. 
- why less than for the transformed space? well because we still only care about the VC dim of the actual nonlinear space $X$. A transformed space $Z$ may add extra points which no point in $X$ maps to, and as a result the number of points taken to shatter $Z$ could very well be higher than $X$. Hence we say less than to reflect that since we only care about $X$ here.

## Two non-separable cases
![Alt text](assets/image-30.png)
- For the left hand side, it is almost linearly separable. It will be an overkill to do transformations on it and go through all the analysis just to try to get an $E_{in} = 0$
- For the right hand side, we definitely need to consider transformation. 
- However, by looking at the data, we might be tempted to change the transformation to something along the lines of:
![Alt text](assets/image-31.png)
- DONT! This harms generalization since we are now completely telling the algorithm to cater to this particular data. The term for this is **data snooping** and it hurts $E_{out}$. (the analogy is, instead of you telling the machine to learn and adapt, you are instead learning and telling the machine to pick up from where you left of - how you analyze human learning is beyond this topic!)

# Logistic Regression
Define the logistic function $\theta$ as:
$$
\theta{(s)} = \frac{e^2}{1+e^s}
$$
![Alt text](assets/image-32.png)

Now define our hypothesis to be $h(x) = \theta{(s)}$, where we interpret this as a probability (e.g. probability of an occurrence, in order to predict it)

![Alt text](assets/image-33.png)
- note: the output of the target function **MUST** be binary (i.e. something has or has not occurred from the prev e.g.)

![Alt text](assets/image-34.png)
- i.e. the probability of a $y$ is our hypothesis $h(x)$, and for $\neg y$ its $1 - h(x)$

![Alt text](assets/image-35.png)
- i.e. we get the likelihood of $y$ occurring (for example), by multiplying the probability of $y$ occurring across the entire training data set 
- we also simplified the function from piecewise to a single line by noticing that, for a logistic function, $\theta(-s) = 1 - \theta(s)$
- next, if we would like to maximize the likelihood of some event (for example to train a neural net):
![Alt text](assets/image-36.png)
- why $ln$, $-1$ and $\frac{1}{N}$? basically for the sake of bounding the in-sample error
- note: we can take the natural log without changing the meaning of anything since the logistic function is strictly non-negative and multiplying by $\frac{1}{N}$ just creates proportional result
- $-1$ means we minimize instead of maximize!
- the rest of the derivation is just algebra (you can easily do this in your head if you think about how log works)
- in the result, the inner statement is the "cross-entropy" error and we are just assigning its average to be $E_{in}$

## Ok, but how do I learn after all that?
- recall, learning = minimizing $E_{in}$
- how to minimize $E_{in}$ for logistic reg (the monstrosity derived from earlier)

![Alt text](assets/image-37.png)
- we can't really do it in one step like linear regression, but we can iterate to it!

## Gradient Descent 
- this is one iterative method
![Alt text](assets/image-38.png)
- in the picture above, its supposed to be in 3D or even higher dimensions, which is why we are using vectors (but also that comes from the definition of a gradient -> i.e. vector of PDEs of some function where each PDE is taken w.r.t to the parameters of that function)
- $\eta$ above is the "learning-step" or "learning-rate". All it is, is a bound on how far we want to travel down the gradient. If it's just 1, then we are travelling all of $\hat{v}$, if its 0.5, then we are travelling half the way and so on.
- why we do the above is essentially to minimizing the total number of iterations it takes to get to the lowest $E_{in}$ point.
![Alt text](assets/image-39.png)
- We want the change in $E_{in}$ to be minimized (i.e. a negative number would be great!)
- the gradient is what we have on the second line with an extra something in $\eta^2$ which we ignore
- which means, the gradient term is all we need to bound
- $\hat{v}$ is a unit vector, so as far as the left term is concerned, its magnitude is not affected by the $\hat{v}$. Therefore, the magnitude of the left term is bounded by the gradient of the change in $E_{in}$, which at its lowest can be $-\eta$ * the gradient (in the case that the gradient vector and $\hat{v}$ are completely pointing opposite. )
- note that we want it to be at its lowest (we wanted to minimize it!), and since we can choose $\hat{v}$, we choose it to be directly opposite to the gradient term and normalize it to maintain it being a unit vector and we are done!

![Alt text](assets/image-40.png)
- we want $\eta$ to vary and be proportional to the magnitude of the gradient change in $E_{in}$, and this is quite easy and leads to the below 

![Alt text](assets/image-41.png)
- by always ensuring that $\eta$ is a multiple of the magnitude of the change in gradient, we essentially can now have it be "fixed" 

![Alt text](assets/image-42.png)


# Neural Networks (L10)
## Stochastic gradient descent 
- from before, using the normal gradient descent, the plan was to calculate $E_{in}$ by summing up across multiple examples and taking the average,
this is called "batch" GD
- instead of summing up across, what if we just tried to minimize $E_{in}$ using only a single ($x_n, y_n$) -> this is stochastic gradient descent (SGD)
![Alt text](assets/image-43.png)
- i.e. we are able to do this since the expected value of the gradient is the exact same as before 

![Alt text](assets/image-44.png)
- randomization helps because sometimes it helps the algorithm navigate out of relatively worse local minimas like the very left one in the diagram above

## Creating layers
![Alt text](assets/image-45.png)
-  understand that we can create the "and" and "or" function quite easily with perceptrons above, which should work as a proof of concept for how powerful they can be when combined 
![Alt text](assets/image-46.png)

![Alt text](assets/image-46.png)
- although perceptrons when combined, can be powerful - they still present the two issues as above. However, at least we have VC Dim to analyze generalization, but what about optimization ? -> that's where neural nets come in which are much more efficient due to backprop 

![Alt text](assets/image-47.png)
- the above just defines some bounds

![Alt text](assets/image-48.png)
- notation!
- notice how each node in the network was just a logistic function, but instead of what we used for logistic regression - we use the hyperbolic tan in order to vary the output from -1 to 1 instead of just 0 to 1 (we don't only want to add to the weights, we might also want to sub from them!)
- notice also how there is only a single output for this network 

## Backpropagation 
![Alt text](assets/image-49.png)

![Alt text](assets/image-50.png)
- for the bottom left result, every $x_i^{l-1}$ is simply a constant in the PDE
- what do we do for the $\delta$? -> we calculate it from the final layer back! (how do we find the final layer's $\delta$? -> look below)

![Alt text](assets/image-51.png)
- notice: the error function for the weights vector here is MSE, but could be swapped for wtv else
- notice: the logistic function here is $tanh$, but again can be swapped for something else
- up until this point, we did not need to consider either one of the two above!

![Alt text](assets/image-52.png)
- the big thing above is the summation that we need to be careful of! Due to the feedforward design, a node in some layer $l$ affects every node in the layer $l+1$ 

![Alt text](assets/image-53.png)
