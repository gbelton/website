---
title: A different birthday problem
author: ''
date: '2018-08-13'
slug: a-different-birthday-problem
categories:
  - probability
tags:
  - probability
image:
  caption: ''
  focal_point: ''
---
At an event I attended yesterday, the icebreaker was to line up in order of our birthdays. There were about 800 people in attendance. Of course, a number of people found their “<a href="https://en.wikipedia.org/wiki/Birthday_problem">birthday buddy</a>.” The organizer also announced that one person in the group had a birthday that day, and we all sang “Happy Birthday” to her.

I was surprised that there was only one person in the group with that birthday. “What are the odds?” I wondered.

Which reminded me of this exchange from The Big Bang Theory:

>**Sheldon Cooper:** What are the odds that two individuals as unique as ourselves would be connected by someone as comparatively workaday as your son?<br>
**Beverly:** Is that a rhetorical point, or would you like to do the math?<br>
**Sheldon Cooper:** I'd like to do the math.

In this case, the math isn’t hard.

For each person in the room, there are two possible outcomes: It is their birthday, or it isn’t. Statisticians call this a “Bernoulli Trial.” We will count “It is their birthday” as a success. Without accounting for leap years, the probability of success is 1/365.

If you conduct a number of Bernoulli Trials and count the successes, the result follows the <a href="https://en.wikipedia.org/wiki/Binomial_distribution">Binomial Distribution</a>. The Binomial Distribution has two parameters: $n$, the number of independent experiments, and $p$, the probability of success for each experiment.

Let’s let $x$ represent the number of successes in our birthday experiment; that is, the number of people who have today as their birthday when we do the experiment. The random variable $X$ follows the binomial distribution with parameters $n$ and $p$, and we write this as:

$$X \sim B(n,p)$$ 

The mean of a Binomial distribution is simply $n * p$, in this case 800 * 1/365 = 2.19. If we repeated this experiment day after day, we would expect the average value of x to be 2.19.

Already, the outcome of x=1 doesn’t seem as improbable as I first thought.

The probability of getting exactly x successes in n trials is given by the probability mass function:

$${Pr}\left (X=x&nbsp; \right ) = \binom{n}{x}p^x\left (1-p&nbsp; \right )^{n-x}$$

Plugging our values into the formula, we get:
$${Pr}\left (X=1&nbsp; \right ) = \binom{800}{1}\frac{1}{365}^1\left (1-\frac{1}{365}&nbsp; \right )^{800-1}$$

(An aside about the notation: $\binom{n}{x}$ is read as “n Choose x,” and a scientific calculator has a key for this. It is equivalent to $\frac{n!}{x!(n-x)!}$ )

Having set up the equation, we can grab our calculator and plug-and-chug to get

${Pr}(X=1)\approx 0.2448$

In other words, the odds of getting 1 birthday in that crowd is about 1:3.
