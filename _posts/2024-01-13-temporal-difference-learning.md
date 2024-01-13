I was a couple lines into this [paper] (https://arxiv.org/pdf/2106.01345.pdf) when I had to Google my first term. That term was Temporal Difference Learning (TDL), and the goal of this post is a) to explain the basic ideas behind it and b) to be the first of many like it. 

One of my New Year's resolutions was to think in a more structured and concrete way. In that spirit, I've decided to start this next segment of posts with a roadmap. Here's the roadmap for this one:
1. What is Temporal Difference Learning?
2. How does it work?
3. How is it applied?

### What is Temporal Difference Learning?
I think this definition is a good starting point: "Temporal difference (TD) learning refers to a class of model-free reinforcement learning methods which learn by bootstrapping from the current estimate of the value function."

Let's start by thinking about the "reinforcement learning" component. Broadly, there a few ways we could approach our prediction. Tne first is to feed our model data on various days and their weather outcomes, then have it predict the weather outcome for a representation of Saturday. The second is to let the model infer it's own patterns from the five days worth of weather data we give it, then have it predict the 6th day. This sort of model might cluster points by some criteria (and presumably new data points would be most closely associated with one of those clusters) or find correlational links between different data points (see associational rule mining).

A third approach, called reinforcement learning (RL), is to allow a model to learn from "rewards" and "punishements" within it's environment (admittedly, this seems a bit orthogonal to the context of supervised/unsupervised learning, but bear with me). Think about the way a child learns to walk. You try standing, fall, try standing again in a slightly different way, fall again, and repeat this process until you stumble upon a stance that allows you to balance. Now let's say you get a cookie each time you're successfully able to balance, and no cookie if you fall. The goal of RL would be to learn a series of actions that allow you to maximize the number of cookies you get (and by extension, in this example, walk most consistently).

I think the best way to unpack the rest of TDL is through an example. We have a sequence of states—let's say the weather on different days of the week (Monday = cloudy, Tuesday = sunny, Wednesday = rainy) etc. Our objective is to train a model (our agent) to predict the weather on Saturday. The key insight TDL (and RL algorithms like it) offer is that the weather data we have on Thursday and Friday (the days leading up to Saturday) might be more relevant to our estimation than data we have on other days of the week. They're able to account for the element of time by tackling prediction in a stepwise manner (we'll see how this works under the hood in the next section).


### Citations
 - https://web.stanford.edu/group/pdplab/pdphandbook/handbookch10.html
 - http://incompleteideas.net/papers/sutton-88-with-erratum.pdf
 - https://en.wikipedia.org/wiki/Temporal_difference_learning