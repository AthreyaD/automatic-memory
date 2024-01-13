I was a couple lines into this [paper](https://arxiv.org/pdf/2106.01345.pdf) when I had to Google my first term. That term was Temporal Difference Learning (TDL), and the goal of this post is a) to explain the basic ideas behind it and b) to be the first of a set of posts devoted to improving my ability to read, synthesize, and articulate quickly, precisely, and at the appropriate level of detail.

One of my New Year's resolutions was to think in a more structured and concrete way. In that spirit, I've decided to start this next segment of posts with a roadmap. Here's the roadmap for this one:
1. What is Temporal Difference Learning?
2. How does it work?
3. How is it applied?

### What is Temporal Difference Learning?
I think this definition is a good starting point: "Temporal difference (TD) learning refers to a class of model-free reinforcement learning methods which learn by bootstrapping from the current estimate of the value function."

Let's start by thinking about the "reinforcement learning" component. Broadly, there a few ways we could approach our prediction. Tne first is to feed our model data on various days and their weather outcomes, then have it predict the weather outcome for a representation of Saturday. The second is to let the model infer it's own patterns from the five days worth of weather data we give it, then have it predict the 6th day. This sort of model might cluster points by some criteria (and presumably new data points would be most closely associated with one of those clusters) or find correlational links between different data points (see associational rule mining).

A third approach, called reinforcement learning (RL), is to allow a model to learn from "rewards" and "punishements" within it's environment (admittedly, this seems a bit orthogonal to the context of supervised/unsupervised learning, but bear with me). Think about the way a child learns to walk. You try standing, fall, try standing again in a slightly different way, fall again, and repeat this process until you stumble upon a stance that allows you to balance. Now let's say you get a cookie each time you're successfully able to balance, and no cookie if you fall. The goal of RL would be to learn a series of actions that allow you to maximize the number of cookies you get (and by extension, in this example, walk most consistently).

I think the best way to unpack the rest of TDL is through an example. We have a sequence of states—let's say the weather on different days of the week (Monday = cloudy, Tuesday = sunny, Wednesday = rainy) etc. Our objective is to train a model (our agent) to predict the weather on Saturday. The key insight TDL (and RL algorithms like it) offer is that the weather data we have on Thursday and Friday (the days leading up to Saturday) might be more relevant to our estimation than data we have on other days of the week. They're able to account for the element of time by tackling prediction in a stepwise manner (we'll see how this works under the hood in the next section).

### How does TDL work?
Before we jump in, i think it's worth recapping the primary components of TDL:
1. Temporal: capture time component of data
2. Difference: We'll get into this in a minute
3. Learning: Learns through RL

I think a good place to start is actually with comparing the "learning" that happens in a TDL with it's more supervised counterpart. If you've gotten this far in the post, I'd guess you have some basic familiarity with neural networks, and have probably seen some form of this formula:

![updating weights](https://miro.medium.com/v2/resize:fit:566/1*2wULsk4M4HG12bZ5cB-bPA.png)

In short, neural networks learn by updating weights: parameters that determine how much each input matters to the output. All this formula is saying is "update a weight in a network by taking the partial of the cost with respect to the current weight" (for a deeper dive into how the chain rule makes this work, see [this excellent medium article](https://towardsdatascience.com/neural-networks-backpropagation-by-dr-lihi-gur-arie-27be67d8fdce)).

Now, what's important to notice here is that we need the system's outcome (i.e. the weather on Saturday) to compute the gradient and update the weights. But what we really want, and what TDL does, is to update the weights at each time step. So instead of this formula:

![sum update weights SL](https://web.stanford.edu/group/pdplab/pdphandbook/handbook110x.png)

We have this one:

![sum update weights TDL](https://web.stanford.edu/group/pdplab/pdphandbook/handbook111x.png)

where the error at each time step is computed by summing the changes in our prediction at adjacent time steps (k, k+1).

A bit of algebra on that insight leads to a modified form of our original formula for updating weights:

![weight update TDL](https://web.stanford.edu/group/pdplab/pdphandbook/handbook115x.png)

Again, pay attention to the subtle changes here:
1. We're not reliant on knowing the value of the target state;instead we compute error based on the difference in successive predictions
2. We calculate the gradient based on V<sub>t</sub> (a prediction at each time step) rather than V(s<sub>t</sub>) (relative to our target state).

There is one other critical point here--TDL maintains the gradient as a running sum. As we update current weights, in other words, we're also updating past predictions. Intuitively, this makes sense, because past states are linked to the more recent states we're observing.

It's also worth mentioning TDL(λ), a commonly used variation of TDL that places more/less weight (depending on the value of λ) on recent states by multiplying a constant by the gradient at each time step.

### Interesting Applications of TDL
1. Games (see SARSA for Mountain Car [here](https://ha-nguyen-39691.medium.com/playing-mountain-car-with-q-learning-and-sarsa-4e7327f9e35c))
2. Navigation (see [this](https://pubmed.ncbi.nlm.nih.gov/10706212/) paper on TDL & hippocampally dependent navigation
3. Control (see section 9.3.2 [here](https://web.stanford.edu/group/pdplab/pdphandbook/handbookch10.html#x26-1370009.3.2)--TD + connectionist networks for backgammon)


### Citations
 - https://web.stanford.edu/group/pdplab/pdphandbook/handbookch10.html
 - http://incompleteideas.net/papers/sutton-88-with-erratum.pdf
 - https://en.wikipedia.org/wiki/Temporal_difference_learning
 - https://www.autoblocks.ai/glossary/temporal-difference-learning