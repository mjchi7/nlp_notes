# Statistical Significance

Yesterday while scrolling through twitter, I came across a thread by **Nassim Nicholas Taleb** that demonstrates the commonplace arguments that *"correlation is not causation"* with the following illustration.
![ppl-drown-vs-nicholas-cage](https://pbs.twimg.com/media/DwRCYtYXcAYAPgU.jpg)
Figure 1: Number of people who drowned by falling into pool v.s. Films Nicolas Cage appeared in.

While I recall in my 2nd year engineering mathematic subject, we did have certain topics that explore *significance testing*, *null hypothesis* and the likes, but I can't articulate what exactly are they (there, classic example of cramming materials just to score well in exam but not actively learning). This particular thread sparks interest in me due to the following reasons:
> I have totally ignored any notion of statistical testing of our data, and instead comes up with a conclusion based solely on correlation value (obtainable with `pandas.DataFrame.corr()` or `numpy.corrcoef()`.

In the video on the thread, Taleb demonstrate how by generating two independent, random variables with low sample size, you will generally get a random non zero correlation value. To do it, he first generate two random independent variables with 18 samples each. He then proceed to get the correlation values. At every run, it is observed that the correlation values fluctuates widely, ranging from 0.8 to -0.5, and occasionally getting 0, which is the actual correlation value given that both random variables are totally independent. 

He then proceed to run the script for 1000 times, and storing the correlation value at each run. The correlation values (y-axis) are then plotted on the graph against the run number (x-axis), which result in the following graph

![graph-18-samples](https://pbs.twimg.com/media/DwW21QZVYAAMNak.jpg:large)
Figure 2: Graph of correlation value of two independent variables against run number.

We can see that it oscillates between the value 0, but sometimes it can goes up to 0.8 or even down to -0.8. This should be a major concern for people who hasn't been considering the importance of statistical test before this (me included) as we wouldn't know whether the correlation value we get from our samples is due to coincidence, or it indeed exhibit correlation between the variables.


### Pearson Correlation
- Accessible using `scipy.stats.pearsonr(x, y)` which returns `correlation_value` and `p_value`. To interpret `p_value`, the following [source](https://stackoverflow.com/questions/33405715/what-is-the-non-correlation-test-in-scipy-stats-pearsonrx-y) can be used.
- Generally, the function `scipy.stats.pearsonr(x, y)` is testing a null hypothesis that *x and y are uncorrelated*.
- Logic of null hypothesis testing: [source](https://opentextbc.ca/researchmethods/chapter/understanding-null-hypothesis-testing/). In which it states the exact logic behind null hypothesis testing.
- Given `correlation_value = 0.385` and `p_value = 0.4889`, the interpretation can be as of the following manner: It is 0.4889 probable that the `correlation_value` of 0.385 occur solely due to chance. Recall that to reject the null hypothesis, we need the `p_value` to be on the 0.05 range, therefore in this case, we can firmly accept the null hypothesis
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzgwODMxODM0XX0=
-->