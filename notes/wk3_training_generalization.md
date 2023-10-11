---
id: 5x7topfgrz8e3hbdwffwyi8
title: Wk3_training_generalization
desc: ''
updated: 1696983059223
created: 1696972972982
---

## Testing v. Training 
Testing follows the familiar Hoeffding Inequality: 
$$
\mathbb{P}[\lvert E_{\text{in}}(g) - E_{\text{out}}(g) \rvert > \epsilon] \leq 2*e^{-2\epsilon^2*N}
$$
Where in-sample refers to the tests themselves and out-of-sample being anything else. 

Training is almost the same, but with an added $M$ constant as below:
$$
\mathbb{P}[\lvert E_{\text{in}}(g) - E_{\text{out}}(g) \rvert > \epsilon] \leq 2*M*e^{-2\epsilon^2*N}
$$
This is because we are training across multiple sets of input -> but our out-of-sample performance may suffer since we end up "memorizing" across the different training sets (as part of the hypothesis set). 

The motivation behind this lecture is to replace that $M$ with something that does not vary across an hypothesis set (also since $M$ could always just go to $\infty$ which makes the entire inequality useless.)


## Where did M come from again?
Essentially, if say some bad events $B_m$ are
$$
\mathbb{P}[\lvert E_{\text{in}}(h_m) - E_{\text{out}}(h_m)] > \epsilon
$$ 
Where the Union Bound is defined as:
$$
\mathbb{P}[B_1 or B_2 or .... or B_m] =  \mathbb{P}[B_1] + \mathbb{P}[B_2] + ... + \mathbb{P}[B_m] 
$$ 

However, the above is essentially saying we have been bounding a particular hypothesis's error with the sum across EVERY hypothesis's error. This approach is clearly problematic as we are overlapping the same error lots!

## Ok, but what to replace M with?
Dichotomies! (_what??_)  
-> Idea: instead of defining hypothesis for the ENTIRE input space, we only look at some pre-selected points and more mathematically:   
a hypothesis: _h_ : _X_ $\rightarrow$ $\{-1,1\}$  
a dichotomy: _h_ : $\{x_1, x_2, ..., x_N\}$ $\rightarrow$ $\{-1,1\}$  
$\lvert H\rvert$ can be infinite, but $\lvert H(x_1,x_2,...,x_n)\rvert$ does not have to be (just don't $n \rightarrow \infty$)

Note: we will essentially be comparing the error of our hypothesis functions wrt some selected points 

## The Growth Function 
So, what is actually replacing M? -> $m$! (not kidding)  
$$
m_H(N) = max_{x_1,x_2, ... x_n \in X}\lvert H(x_1,...,x_N)\rvert
$$
The above is the growth function that counts the MOST dichotomies (rmb: similar to a hypothesis but with a limited input space) on any N points. 
We can say that this growth function satisfies:
$$
m_H(N) \leq 2^N
$$
Why? since there are only $2^N$ possible values that the entirety of $x_1...x_N$ could represent (either they are a 1 or a -1).

Note: $m_H(N)$ is essentially just finding the max number of unique functions that lead to a set of unique values for $x_1...x_N$ (in the case of PLA - how many unique straight lines can we draw that split the chosen points s.t. the set of values assigned (values from {1,-1} ) to each point is unique)

Note: we eventually want to replace $M$ with $m_H(N)$

## Why do we care?  
$$
\mathbb{P}[\lvert E_{\text{in}}(g) - E_{\text{out}}(g) \rvert > \epsilon] \leq 2*M*e^{-2\epsilon^2*N}
$$
In the inequality above, if we replace $M$ with $m_H(N)$ and prove that $m_H(N)$ is a polynomial -> the exp term will essentially make the entire RHS very small as $N$ increases. (intuitively, this is exactly what we would expect - the more data, the lesser the error rate).

Problem: how to prove $m_H(N)$ will be a polynomial? (with more definitions of course) 

## The break point of $H$
The break point of some hypothesis set $H$ is defined as:  
"If no data set of size $k$ can be _shattered_ by $H$, then $k$ is the break-point of $H$" -> $m_H(k) < 2^k$  

what?  
- _shattered_ here refers to the fact that we can (either with a 2D line or some hyperplane), split the input space s.t. we can divide all the chosen points, so every possible output is viable (i.e. by saying one side is a +1 and the other is -1)
- if we **cant** shatter the hypothesis set due to the number of points picked (e.g. in your head imagine PLA, if you pick 4 random points on a 2D plane - can you draw a line to represent every possible unique output? No coz see the pic below) - then we can guarantee that the total number of unique classifications will be < $2^k$ (closer to polynomial - and actually, it is guaranteed to be a polynomial, more on this below)

![Alt text](assets/image-13.png)
- cant draw any line to create this very classification 

## What does a break point help with?
Here are the properties:  
- if there is no break point -> $m_H(k) = 2^k$   
- if there is a break point  -> $m_H(k)$ is **polynomial** in N (proof later on)

