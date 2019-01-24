# Edge Detection

Edges in picture is basically "discontinuity in pixel intensity of an image". The goal of *edge detection* is therefore to have a filter system that produce big response when the pixels in images is showing high discontinuity; and no response when the pixels are uniform in intensity value. But how exactly do we measure the degree of "discontinuity" among pixels? Before we proceed, one concept of discrete mathematics needs to be clarified as the rest of the text will depend heavily on the following operation: **discrete convolution**

## Discrete Convolution
Convolution itself is a complex mathematics, which is not what this document aims to explain. In discrete version however, convolution is simply applying a "mask" (or "filter" or "kernel") over the image in order to produce "impulse response". Note that the term "impulse response" is just a generic term which can be replaced with whatever users are interested to get. For instance, if a Gaussian smoothing kernel is applied, the "impulse response" will simply means the output smoothed image; On the other hand if a Sobel operator (more on this later) is applied, the "impulse response" simply means the output edge image. 

Concretely, given a Gaussian Averaging Kernel and an image path:

$G = \frac{1}{16}\begin{bmatrix}1 & 2 & 1 \\ 2 & 4 & 2 \\ 1 & 2 & 1\end{bmatrix} \qquad img = \begin{bmatrix}10 & 20 & 30 \\ 40 & 50 & 60 \\ 70 & 80 & 90\end{bmatrix}$

To convolve the Gaussian kernel $G$ on the image path $img$, the kernel will first needs to be inverted **horizontally** and **vertically**. For Gaussian kernel, coincidently flipping it in both direction will not affects its values and location, but for others kernel like Sobel operators it might result in some values in different location.

To convolve, simple apply the kernel onto the image, and multiple the value **element wise** and summing it up. 

For more detailed graphical visualization, head to this [source](https://www.tutorialspoint.com/dip/concept_of_convolution.htm)


## Edge detection through simple gradient value
The idea is that on images, edges are identified when it shows a great discontinuity in pixel intensity value.

### Gradient as a measure of discontinuity
Gradients of pixels are computed as a measure of how dissimilar is the current pixel's intensity value with its neighbor pixel's intensity value. The higher the gradient, the sharper the edge. 

#### Gradient computation
To compute the gradient of pixel intensity value, discrete differentiation operator is being employed and there are three different way to compute its value:

1. Backward
$$\frac{df}{dx} = f(x) - f(x-1) = f^\prime(x)$$$$[0, 1, -1]$$
Which literally means "The gradient at point $x$ is equals to the intensity value at current index $f(x)$ minus the intensity value of previous index $f(x-1)$" where $f$ is the image intensity function that maps the parameter $x$ to its respective intensity value.

2. Forward
$$\frac{df}{dx} = f(x) - f(x+1) = f^\prime(x)$$$$[-1, 1, 0]$$
Again it means "The gradient at point $x$ is equals to the intensity value at current index $f(x)$ minus the intensity value of next index $f(x+1)$."

3. Central
$$\frac{df}{dx} = f(x+1) - f(x-1) = f^\prime(x)$$$$[1, 0, -1]$$
For central, it means "The gradient at point $x$ is equals to the intensity value at next index $f(x+1)$ minus the intensity value of previous index $f(x+1)$." It is therefore relying on its neighbor to decide the gradient at point $x$.

### Gradient Computation in action.
Imagine an image with a single black line in front of a white background. On the edge of this black line, its pixel are approximately as the following matrix sampled. 
$$\begin{bmatrix}
254 & 254 & 0 & 0 & 0 & 254 & 254 \\
254 & 254 & 0 & 0 & 0 & 254 & 254 \\ 
254 & 254 & 0 & 0 & 0 & 254 & 254 \\
\end{bmatrix}$$

And its intensity values can be obtained using the intensity value function $f(x)$. The addressing of the matrix is done using the following address pattern:

$$\begin{bmatrix}
x_{11} & x_{12} & x_{13} & x_{14} &  ... \\
x_{21} & x_{22} & ... & ... & ... \\
... & 
\end{bmatrix}$$

Where intensity value of 0 represent color *"black"* and intensity value of 254 represent *"white"*. Imagine now we apply the operators discussed above, starting from $x_{11}$ using the *forward* computation, we will get the following value as our gradient:

$$
\begin{aligned} 
f^\prime(x) &= f(x) - f(x+1) \\
 &= 254 - 254 \\
 &= 0\\
\end{aligned}$$

Which is not surprising, seeing how there is no *variation* in pixel value when we go to our neighbor index, therefore the gradient should be 0. Moving to $x_{12}$ and again using the same computation, we will get the following value as our gradient:
$$
\begin{aligned} 
f^\prime(x) &= f(x) - f(x+1) \\
 &= 254 - 0 \\
 &= 254\\
\end{aligned}$$

The discontinuity is fairly obvious to us, and the computed result reflects that: it shows that the variation in pixel intensity value between $x_{12}$ and $x_{13}$ is $254$, which will be the gradient value for index $x_{12}$ using the *forward* computation. The rest of the operators will demonstrate more or less the same result with the exception that the location of gradient values might be shifted. 

In the case the computation can't be performed due to the current index as the last in the row, several methods can be used to alleviate this: a) ignoring the boundary's gradient which cause the gradient image to be smaller than original image, b) including filler value beyond the boundary. 

#### Gradient convolution mask is .. wrong? 
Sharp reader by now should notice that the *backward computation*'s kernel has actually inverted the $+1$ and $-1$ sign in the convolution matrix. Reason being that to perform convolution, the mask (the matrix $\begin{bmatrix}0 & 1 & -1\end{bmatrix}$) will need to first, by definition be "inverted" into $\begin{bmatrix}-1 & 1 & 0\end{bmatrix}$ and then only it can be applied to the image to calculate the gradient. For more information, refers to the following [source](https://www.tutorialspoint.com/dip/concept_of_convolution.htm).

## Sobel Operator: Detecting edge without high frequency components
The idea of *Sobel operator* is to first smoothen the pixel intensity value of the image, so that high frequency components in pixel values are removed. The reasoning behind this idea is that for a "real" edge, the variation in pixel intensity value should be fairly persistent across larger pixel windows, as compared to noise which might causes sharp discontinuity in pixel intensity value across a single pixel. 

The operator is a mixture of smoothing kernel and central gradient computation kernel. Concretely, 

$$ G_x = 
\begin{bmatrix}
1 & 0 & -1 \\
2 & 0 & -2 \\
1 & 0 & -1 \\
\end{bmatrix}
\qquad G_y = 
\begin{bmatrix}
1 & 2 & 1 \\
0 & 0 & 0 \\
-1 & -2 & -1 \\
\end{bmatrix}
$$

Where $G_x$ is the Sobel operator across x-axis and $G_y$ is the Sobel operator across y-axis. 

The matrices can be decompose into their individual components (smoothing kernel and gradient computation kernel) as shown below:

$$ G_x = 
\begin{bmatrix}
1 & 0 & -1 \\
2 & 0 & -2 \\
1 & 0 & -1 \\
\end{bmatrix}
= \begin{bmatrix}
1 \\ 2 \\ 1
\end{bmatrix}
\begin{bmatrix}
1 & 0 & -1
\end{bmatrix}
$$
Where $\begin{bmatrix} 1 \\2 \\ 1 \end{bmatrix}$ is the averaging operator and $\begin{bmatrix} 1 & 0 & -1 \end{bmatrix}$ is the *forward gradient computation*.

Generally, the output of Sobel operators tends to be less affected by grain-like noise (high ISO setting) as compared to the simple vanilla approach, due to the addition of an averaging component in the kernel.

After applying both kernels $G_x$ and $G_y$ on the image, one should obtain a "gradient" map of both kernels that tells at exactly point $x$, what's the gradient value. At this point, both maps are only referring to the gradient at x direction and y direction. To get an actual gradient values that are incorporating both direction's gradient, its magnitude can be calculated as such:

$G_{xy_{mag}} = \sqrt{{G_x}^2 + {G_y}^2}$ 

and the direction of gradient $\theta$ can be obtained as such:

$\theta_{xy} = \arctan(\frac{G_y}{G_x})$

However, for applications that are only interested in the outline of objects in the picture, Sobel operator might not work very well as it naively classify all the discontinuity as "edges". The following text explains the "Canny Edge" detector which is a more sophisticated approach in edge detection that can alleviate such problem using continuity analysis.

## Canny Edge Detector
Do not misunderstand, Canny Edge detector is not an "evolution" of Sobel operator, but an extension. Concretely, Canny Edge detector takes as input the output of Sobel operator, it requires the information output by Sobel operator (magnitude of gradient and direction $\theta$ of gradient) in order to work.

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTMwNzA5MjkyNl19
-->