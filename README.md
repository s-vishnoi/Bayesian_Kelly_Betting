Bayesian Kelly Betting
======================

How do you bet when odds are uncertain?
---------------------------------------


[Listen](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2Fplans%3Fdimension%3Dpost_audio_button%26postId%3D2305bdc5d41e&operation=register&redirect=https%3A%2F%2Fmedium.com%2F%40s-vishnoi%2Fbayesian-kelly-betting-2305bdc5d41e&source=---header_actions--2305bdc5d41e---------------------post_audio_button------------------)



![Image by @_alexiabryan_ on IG](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*Vt_pXVfz-zkDhdM8muOlFA.png)

Suppose you have found a strategy that wins more often than it loses. Great!

But now comes the harder question:

> How much of your money should you actually risk each time?

*   Bet too little, and you barely benefit from your edge.
*   Bet too much, and even a good strategy can blow up from a few unlucky outcomes.

This is exactly the problem the **Kelly criterion** tries to solve.

### Kelly Criterion

Formulated by John R. Kelly (Bell Labs), the Kelly criterion is a rule for maximizing long-run log wealth across repeated bets.

In simpler words, it tells you how much to bet if your goal is not to win one round, but to grow wealth efficiently through compounding.

But there is one very important catch:

> _Classic Kelly assumes you know your_ true _probability of winning._

In real life, you almost never do. You estimate it, which makes winning trickier.

This article gives a practical introduction to classic Kelly betting, why overbetting is so dangerous, and demonstrates a Bayesian version that naturally accounts for the uncertainty in your estimated edge.

Let’s dive in!

Classic Kelly Criterion
-----------------------

Let’s start with the simplest possible betting setup.

Suppose we repeatedly play a binary-outcome game:

*   With probability θ, we win.
*   With probability 1 − θ, we lose.
*   If we win, we gain the amount we bet.
*   If we lose, we lose the amount we bet.

This is an even-money bet. If we bet a fraction **_f_** of our current wealth, then after one round our wealth follows this update rule:

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*gdUE2v7PB12wk6t6U90opQ.png)

Kelly’s insight was to maximize the long-run expected log wealth.

### _Why log wealth?_

Because betting is multiplicative. Wealth compounds. A sequence of wins and losses does not add up neatly; they multiply over time.

If you gain 50% and then lose 50%, you are not back to where you started.

1.5*0.5 = 0.75 => You are down 25%.

This is why **log wealth** is such a natural objective- it makes time returns additive, making total returns a simple running sum.

log(1.5) + log(0.75/1.5) = log(0.75) ~ -28%

Note that logarithmic transformations track the overall return closely, but not identically. Mathematically, the log transformation **_oversells losses_** and **_undersells gains_.**

Thus, under the Kelly formulation, the expected log-growth rate is:

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*Z7BzJMvBF9QQHQaRCRP7UQ.png)

To find the optimal betting fraction, we trunk the calculus crank- differentiate, set the derivative equal to zero, and solve for _f_:

![captionless image](https://miro.medium.com/v2/resize:fit:1060/format:webp/1*uH0IZlKqtQmkvXoEuzwxKg.png)

That is the _classic_ Kelly fraction for an even-money binary bet.

### Example 1:

Suppose your true win probability is θ=0.6

Then,

_f_ = 2*(0.6)−1 = 0.2

In other words, if you really win 60% of the time on an even-money bet, the Kelly criterion says to bet 20% of your bankroll each round.

Seems _high_? It is!
But remember, this is under the assumption that the true win probability is 60%; the assumption is doing a LOT of work here.

### **Long-run wealth**

Now let’s evaluate the Kelly optimality by comparing three strategies:

*   **Underbetting:** _f_ = 0.1
*   **Kelly betting:** _f_ = 0.2
*   **Overbetting:** _f_ = 0.4

I simulated 5,000 wealth paths over 300 rounds. Every bettor starts with wealth 1$. The only difference is the fraction of wealth they bet each round.

![Monte-Carlo simulations of fixed-fraction betting with an edge of 0.6](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*Rbv_VNTJUeM73FDG10t5Kg.png)

The plot shows Monte-Carlo simulations of wealth over time on a log scale, with median wealth highlighted.

*   The **underbetting** strategy grows slowly but steadily. It is not optimal, but it is relatively stable.
*   The **Kelly** strategy grows much faster. It accepts volatility, but not so much that compounding is destroyed.
*   The **overbetting** strategy is the strange one. Even though the bettor has a real edge, the median wealth path declines over time. Betting 40% of wealth each round is simply too much volatility for the compounding process to tolerate.

This is one of the most important ideas in Kelly betting:

> A good edge can still be ruined by bad sizing.

### Mean vs. Median wealth

Now here is where things get even more interesting. This second plot compares the mean and median final wealth after 300 rounds.

![Mean and Median wealth divergence in fixed-fraction betting](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*zrLia7TPE33lltoVRQONGw.png)

*   For _f_ =0.1, the mean final wealth is $365, while the median is $91
*   For _f_=0.2, the mean is solid $74,149, while the median is about $420
*   For _f_=0.4, the mean is about $6.25M, while the median is a measly $0.48

That last result is the _dangerous_ one.

Overbetting has an enormous mean final wealth because a few lucky paths explode upward. If someone hits enough early wins while betting 40% of wealth, their final wealth can become huge. Those rare outcomes drag the average upward.

But the median path is terrible.Most paths do not experience that lucky explosive trajectory. The typical overbettor ends up worse off than they started.

If you want more control over the outcome of your betting, rememeber this mantra:

> You don’t live the average of all lives. You live one path.

So when evaluating a betting strategy, the mean wealth can tell a very different story from the median wealth. Overbetting looks good in a spreadsheet because of rare explosive outcomes, but it can be terrible for the typical realized trajectory.

I find that this applies to more than betting, to life in general.

Once you realize the dangers of overbetting for your portfolio, you can glean the inherent danger in being overconfident in your edge.

### What If θ Is Unknown?

Classic Kelly assumes that θ is known.

But in most real situations, we do not know the true win probability.
We estimate it from data. The most obvious estimate is the raw sample win rate:

![captionless image](https://miro.medium.com/v2/resize:fit:580/format:webp/1*Rj-D_wJZCZWaffvv78QFLw.png)

assuming we observed w wins and l losses.

Although used extensively, this sample win rate is highly biased/un-converged in lower sample space.

### **Example 2:**

Think: scoring 3 heads in 4 fair coin tosses would give θ (win = heads) = 3/4. with _f_ = 2 (3/4) -1 = 0.5 i.e. You’ll end up betting 50% of your wealth on a random coin toss!

Basically, you want your data to have matured, and knowing this is not a guarantee in more complex spaces.

This is exactly the kind of problem Bayesian thinking is built for.

### The Bayesian POV

In a Bayesian framework, we do not treat θ as a fixed known value. Instead, we treat it as uncertain.

The parameter θ represents the true win probability, BUT… since we do not know it, we place a probability distribution over it.

For binary win/loss data, the natural prior is the Beta distribution:

![captionless image](https://miro.medium.com/v2/resize:fit:716/format:webp/1*Mi0mnTyPMyOw6tDpRZNsQw.png)

### **Why Beta?**

Because θ is a probability, so it must live between 0 and 1. The Beta distribution also plays very nicely with binary data, making the math clean and interpretable.

After observing w wins and l losses, the posterior becomes:

![captionless image](https://miro.medium.com/v2/resize:fit:1096/format:webp/1*bGDZ_sUv-dNFQrpSyz9xqQ.png)

*   The prior looks like pseudo-data.
*   The parameter α acts like prior wins.
*   The parameter β acts like prior losses.

So after observing real wins and losses, we simply update the prior counts.

The posterior mean is,

![captionless image](https://miro.medium.com/v2/resize:fit:1184/format:webp/1*Uup2xVNKsnJ8mA-HTbYPKg.png)

This leads to our next natural estimate.

### **Bayesian Kelly** fraction

Now we can define Bayesian Kelly in the simplest possible way.

Instead of plugging the raw sample win rate into Kelly, plug in the posterior mean. Using the Beta posterior mean gives:

![captionless image](https://miro.medium.com/v2/resize:fit:1212/format:webp/1*jITa9777lCv_BRLcEYxX3Q.png)

This is the Bayesian Kelly fraction, AND.. it also has a nice interpretation-

*   The numerator captures the evidence that wins outweigh losses, including both prior belief and observed data.
*   The denominator captures how much total information we have.

So, the more data we have, the more confidently we bet. The less data we have, the more the prior regularizes our decision.

### Example 2 (Bayesian):

Let’s return to the earlier example where we observe 3 wins and 1 loss.

Now let’s use a neutral prior: _θ_ ∼ Beta(1,1). This is a uniform prior over possible win probabilities.

After observing 3 wins and 1 loss, the posterior, posterior mean, and Bayesian Kelly fraction are:

![captionless image](https://miro.medium.com/v2/resize:fit:1104/format:webp/1*AY71q5PZ6fE2KsP1EKcCTg.png)

Raw Kelly says bet 50%. Bayesian Kelly says bet ~33%.

Both agree that the observed data is promising. But Bayesian Kelly is more cautious because it recognizes the obvious problem of low samples.

This is the _core_ benefit.

### Online Learning: The Continuous Better

Now let’s make example 2 more realistic.

Suppose the true win probability is still θ=0.6.

BUT, the bettor does not know this. Instead, they must learn this from outcomes over time.

After every round, they update their estimate of θ, compute a new Kelly fraction, and place the next bet.

Compare two strategies.

1.  The first is **raw running Kelly**.

2. The second is **online Bayesian Kelly.**

![Wealth outcomes for classic vs Bayesian Kelly updating](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*YTI5a8t5O-Voh6pwuGpOmA.png)

This plot compares median wealth for raw running Kelly and Bayesian Kelly over 200 rounds.

### The result: **brutal**

The raw running Kelly median wealth collapses to **zero** in **5** rounds. This happens because early overconfidence can lead to betting the full bankroll. A single loss after that wipes the bettor out.

The Bayesian strategy avoids that behavior. It grows more cautiously early on, taking around 70 rounds to break even. After that, it compounds more reliably as evidence for the edge accumulates. Eventually, not only median but also mean Bayesian wealth shows better weath outcomes.

> After 200 rounds, the Bayesian Kelly strategy has a median final wealth of about 47.8 in the simulation. The raw running Kelly median is **zero**.

Again, the mean can be misleading. Raw running Kelly still has a large mean final wealth because some paths get lucky early and explode upward. But the typical path is much worse.

This is _exactly_ the problem Bayesian Kelly solves.

Concluding Remarks
------------------

Of course, real world problems are going to be much harder to track. θ could very well be non-stationary, or too high-dimensional.

But the key lesson remains: the danger is not being wrong. The danger is being too confident, too early.

Classic Kelly is powerful when the edge is known, but fragile when the edge is estimated from limited data.

That is the broader value of the Bayesian approach. It does not _eliminate_ risk, and it does not _magically_ discover an edge. But it gives us a disciplined way to update beliefs, manage uncertainty, and avoid turning a promising advantage into ruin through overconfidence.

Check my Github for figure and simulation code.
