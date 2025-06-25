# Where is the math in Casino Risk of Ruin by Ivan Contreras Arvizu
This repository explores the mathematical principles behind the concept of “Risk of Ruin” in casinos. It introduces a probabilistic model that estimates the likelihood of a casino going bankrupt, based on concepts like binomial distributions, Poisson approximation, and house edge.

## Introduction
Since I was young, I’ve been curious about how randomness and chance work. Although I never took formal courses in probability, I’ve always wanted to understand it better. When I had the chance to freely choose a topic, I saw this project as the perfect opportunity to explore probability through something unusual but engaging: the **Risk of Ruin** in casino games.

That curiosity led me to the following question:  
**Can a casino actually go bankrupt if a gambler gets lucky enough?**  
This is the essence of the Risk of Ruin (ROR) the probability that a casino loses all its capital due to a rare sequence of wins by a player. While casinos are designed with a statistical advantage (house edge), randomness introduces a non-zero risk of failure.

To analyze this, I study a probabilistic model based on **binomial distributions**, **Poisson approximations**, and the concept of **house edge**, inspired by the paper from (Siu et al., 2023). 
This project allowed me to apply mathematical modeling techniques commonly used in real world for analyzing failure, uncertainty, and risk to a dynamic and unpredictable scenario.


### Motivational videos
Before diving into the math, here are a couple of short videos that introduce some of the key concepts in a simple and engaging way. They caught my attention from the start I hope they do the same for you.
- [YouTube – Introductory Explanation](https://www.youtube.com/watch?v=ErunjUChFdg)
- [TikTok – Concept Poisson](https://vt.tiktok.com/ZSkrYqc4X/)
- [TikTok – Concept Binomial](https://vt.tiktok.com/ZSkr2R3Wo/)
  
## Preliminary Video
This is an intrioductory video is a visual summary of my project *“Where is the math in the Casino’s Risk of Ruin?”* It introduces the key ideas behind the concept, such as how a casino can lose money over time despite having the advantage. I also explain in very basic terms the mathematical tools used like binomial, Poisson distributions, and the house edge.  
[Watch the full video on YouTube](https://youtu.be/p2Rm3tjNqXc)

## Hands-On (Start with Technicalities)
Now that we’ve introduced the general idea of the project, let’s move on to the technical side of the model.

In the article that inspired this work, the authors propose a mathematical approach to analyze how and when a casino might face bankruptcy due to a series of consecutive wins by a player. The goal is to understand the dynamics of this situation using probability theory and to provide a way to **measure the Risk of Ruin** a casino takes in different games.

Based on this, we will start with the introduccion of a simplified model that treats the interaction between the player and the casino as a kind of **probabilistic race**, modeled as a **binomial random walk**. In this context, the **Poisson distribution** helps estimate the chances of a rare event such as a gambler winning multiple times in a row. Additionally, we incorporate the effect of the **house edge**, which shifts the balance in favor of the casino. The result is a formula that allows us to build what we call a **Risk of Ruin**,the probability that a casino will lose all its capital under specific game conditions. It serves as a practical reference for understanding how factors like the number of allowed bets, win probability, and house edge influence the casino’s financial risk across different games.

To begin modeling the Risk of Ruin, we consider a **binomial random walk** from the perspective of the casino.

Let **`n`** be the number of times the casino can afford to lose before going bankrupt. In other words:

- `n` represents the **casino's capital**.

This value changes depending on the outcome of each round:

$$
n \leftarrow
\begin{cases}
n + 1, & \text{if the casino wins} \\
n - 1, & \text{if the gambler wins}
\end{cases} \quad \text{(1)}
$$


If `n ≤ 0`, the casino has lost all of its capital — this means **bankruptcy**.

From the **gambler’s perspective**:

- Let **`p`** be the probability that the gambler wins a single round.
- Then, **`1 - p`** is the probability that the casino wins that round.

These probabilities define how the value of `n` changes round by round, creating a dynamic back and forth that resembles a race between the gambler and the casino.

To estimate the likelihood that the casino goes bankrupt, we define a probability function:

$$
\text{Pro}(n) =
\begin{cases}
p^{n-k}, & \text{if } k \leq n \\
1, & \text{if } k > n
\end{cases} \quad \text{(2)}
$$

>Here:
>- `k` is the **number of wins accumulated by the gambler**.
>- When `k ≤ n`, we calculate the chance that the gambler wins enough times to bring the casino's capital down to zero.
>- If `k > n`, the gambler has already passed the casino’s bankroll, so the risk of ruin becomes 100% basically 1.

This expression gives us the "first version" of the Risk of Ruin (ROR) and shows how the win probability `p`, the number of gambler wins `k`, and the casino’s initial capital `n` are related.

To move from a simple random walk to a probabilistic expression, we make the following assumption:

> The **gambler is allowed to play `t` times**, as many as possible, meaning the number of games is very large. With that, we can model the probability that the gambler wins exactly `k` times in `t` independent games using the **binomial distribution**:

$$
P(X = k) = \binom{t}{k} \cdot p^k \cdot (1 - p)^{t - k}
\quad \text{(3)}
$$

>Where:
>- \( t \): total number of games played (number of experiments)
>- \( k \): number of wins by the gambler
>- \( p \): probability of winning a single game
>- \( 1 - p \): probability of losing (or the casino winning)
>- \( $$\binom{t}{k} \$$): number of combinations of `k` wins in `t` trials

This formula gives us the probability of a specific outcome — for example, that the gambler wins **exactly 5 out of 100 games**.

This is a natural extension of the binomial walk model, because now we’re not just looking at what happens *step by step*, but what happens *after a large number of games*, and what the **distribution of outcomes** looks like.

At this point, we move from the binomial model to something more practical for analyzing extreme but *rare scenarios* for example, a gambler getting incredibly lucky across many games.

To do this, we assume:

> The gambler has too much credit and is allowed to play a very large number of games, say `t → ∞`,  
> and the probability of winning a single game `p` is very small.  
> But the **expected number of wins** stays constant:  
> 
> $$
> \mu = t \cdot p
> $$

Is this situation perfect for applying the **Poisson approximation**?, YES!.

We start with the binomial probability:

$$
\lim_{t \to \infty} \binom{t}{k} \cdot p^k \cdot (1 - p)^{t - k}
$$

Substituting $$\( p = \mu / t \)$$, we get:

$$
\lim_{t \to \infty} \binom{t}{k} \cdot \left( \frac{\mu}{t} \right)^k \cdot \left(1 - \frac{\mu}{t}\right)^{t - k}
$$

---

#### How does this simplify?

##### 1. **Simplifying the combination term**:

The binomial coefficient (Das, 2016):
$$\binom{t}{k} = \frac{t!}{k!(t-k)!}$$

When `t` is very large and `k` is small (say `k = 2` or `3`), we can approximate:

$$
\frac{t!}{(t - k)!} \approx t^k
$$

So:

$$
\binom{t}{k} \approx \frac{t^k}{k!}
$$

##### Example with small numbers:

Let’s say \( t = 100 \) and \( k = 2 \):

$$
\binom{100}{2} = \frac{100!}{2! \cdot 98!} = \frac{100\cdot 99 \cdot 98!}{2! \cdot 98!}= \frac{100 \cdot 99}{2} = 4950
$$

Now compare with the approximation:

$$
\frac{100^2}{2!} = \frac{10000}{2} = 5000
$$

> **Note:**  
> Even though $$\( \binom{t}{k} \)$$ and $$\( \frac{t^k}{k!} \)$$ are not exactly equal, they become very close when `t` is large and `k` is small.  This is why we can replace the binomial coefficient with a simpler form when applying the **Poisson approximation** it simplifies the math significantly, while still giving us a good estimate (that's the idea behind).

---

##### 2. **Simplifying the exponential term**:

As `t → ∞`, we use the identity:

$$
\lim_{t \to \infty} \left(1 - \frac{\mu}{t} \right)^{t} = e^{-\mu}
$$

So the full expression becomes:

$$
\lim_{t \to \infty} \frac{t^k}{k!} \cdot \left( \frac{\mu}{t} \right)^k \cdot \left(1 - \frac{\mu}{t} \right)^{t - k}
= \frac{\mu^k e^{-\mu}}{k!}
$$

---

#### What does this mean?

We now have a clean, simple way to calculate the probability that the gambler wins exactly `k` times **when the number of games is very large**:

$$
P(X = k) = \frac{\mu^k e^{-\mu}}{k!}
$$

This is the **Poisson distribution**, and it becomes the foundation of the final model.

#### Poisson Approach – Total Probability of Ruin

Now that we have the Poisson distribution, we use it to calculate **how many rounds `k` the player needs to break the casino**.

But we don't just want the probability of **one specific `k`**, right?  
We want to compute the **total probability** that the player wins in any possible way that causes the casino to go bankrupt.

That means summing up **all possible values of `k`** where the gambler succeeds. So, we define:

$$
\text{Pro}(n) = \sum_{k=0}^{\infty} \frac{\mu^k}{k!} e^{-\mu} \cdot
\begin{cases}
p^{n-k}, & \text{if } k \leq n \\
1, & \text{if } k > n
\end{cases}
\quad \text{(4)}
$$

This function splits into two parts:

- When $$\( k \leq n \)$$: the gambler hasn't caught up yet.
- When $$\( k > n \)$$: the gambler has already surpassed the casino's capital → ruin is certain.

So, we split the summation:

$$
\text{Pro}(n) = e^{-\mu} \left( \sum_{k=0}^{n} \frac{\mu^k}{k!} p^{n-k} + \sum_{k=n+1}^{\infty} \frac{\mu^k}{k!} \cdot 1 \right)
$$

---

#### Algebraic Simplification

We want to simplify this infinite sum:

$$
\sum_{k = n+1}^{\infty} \frac{\mu^k}{k!}
$$

Calculating an infinite sum directly isn't easy, but we know a useful identity:

$$
\sum_{k=0}^{\infty} \frac{\mu^k}{k!} = e^{\mu}
$$

So, we can rewrite the part we care about like this:

$$
\sum_{k = n+1}^{\infty} \frac{\mu^k}{k!} =
\sum_{k = 0}^{\infty} \frac{\mu^k}{k!} -
\sum_{k = 0}^{n} \frac{\mu^k}{k!}
$$

This is like subtracting the first few terms we don’t need.

---

Simple idea behind it, if you have:

$$
1 + 2 + 3 + 4 + 5 + \cdots
$$

And only wanted from 4 ahead, you’d subtract:

$$
(1 + 2 + 3 + 4 + 5 + \cdots) - (1 + 2 + 3) = 4 + 5 + \cdots
$$

Same logic applies here just with factorials and powers of $$\( \mu \)$$.

>This trick lets us avoid calculating to infinity by using a known identity and subtracting a manageable sum.
---
So we now subtract and rearrange the second summation:

$$
\sum_{k=n+1}^{\infty} \frac{\mu^k}{k!} = \sum_{k=0}^{\infty} \frac{\mu^k}{k!} - \sum_{k=0}^{n} \frac{\mu^k}{k!}
$$

By applying the identity of the exponential series, and using the earlier trick to avoid calculating an infinite sum directly, we can rewrite the expression in a more manageable form (teachoo, 2019).

$$
\sum_{k=0}^{\infty} \frac{\mu^k}{k!} = e^{\mu}
$$

We now have:

$$
\text{Pro}(n) = e^{-\mu} \left( \sum_{k=0}^{n} \frac{\mu^k}{k!} p^{n-k} + e^{\mu} - \sum_{k=0}^{n} \frac{\mu^k}{k!} \right)
$$

Group the terms:

$$
\sum_{k=0}^{n} \frac{\mu^k}{k!} p^{n-k} - \sum_{k=0}^{n} \frac{\mu^k}{k!} = \sum_{k=0}^{n} \frac{\mu^k}{k!} (p^{n-k} - 1)
$$

Which becomes:

$$
\-\sum_{k=0}^{n} \frac{\mu^k}{k!} (1 - p^{n-k})
$$

Finally, take out the common factor:

$$
\text{Pro}(n) = e^{-\mu} e^{\mu} - e^{-\mu} \sum_{k=0}^{n} \frac{\mu^k}{k!} (1 - p^{n-k})
$$

Since $$\( e^{-\mu} e^{\mu} = 1 \)$$, the final expression becomes:

---

#### Final Probability of Ruin:

$$
\boxed{
\text{Pro}(n) = 1 - e^{-\mu} \sum_{k=0}^{n} \frac{\mu^k}{k!} (1 - p^{n-k})
}
\quad \text{(5)}
$$

This formula gives us the total probability that a gambler **eventually bankrupts the casino**, by summing up all the ways it could happen up to the point $$\( k = n \)$$, where `n` is the number of rounds the casino can resist.

---

#### House Edge $$\rightarrow$$ the house always wins… Right?

So... wait!, the formula we’ve seen still isn’t complete.

Why? Well… we forgot one important thing: **the house always has an edge** Casinos don’t play fair they’re designed to win *in the long run*.  This advantage is what we call the **house edge**, and we represent it with the letter **`a`**.

What does house edge really mean?

##### Example:

Let’s take a practical example: imagine a player at a blackjack table. On average, for every $100 the player bets, they only win back around $99 the casino keeps $1. That $1 is the **house edge** a small built-in advantage (in this case, 1%). It might seem small, but over hundreds or thousands of rounds, it adds up. Mathematically, this means the casino doesn't just move one step forward when it wins it moves more than one, putting the player at an increasing disadvantage the longer the game goes on. That’s why the gambler needs to win more often (and more consistently) just to catch up and that’s the core idea behind adding the house edge to the Risk of Ruin model.

Back to the formula is a race between the gambler and the casino. Every time the gambler wins, they take a step forward. But every time the casino wins, it moves more than one step it gains a lead because of the house edge.

$$
n \leftarrow
\begin{cases}
n + (1 + a), & \text{if the casino wins} \\
n - 1, & \text{if the gambler wins}
\end{cases}
\quad \text{(1.1)}
$$

The effect? The gambler has to win "more times" to reach $$\( n \)$$ and bankrupt the casino.  
This extra effort this "distance advantage" is what makes the casino so resilient.

---

#### Updated Formula With House Edge

We now update our original formula using a simplified notation.  
Instead of writing the step-by-step addition $$\( n + (1 + a) \)$$, we switch to the more compact form:

$$
n(1 + a)
$$

This expresses the **total lead** the casino accumulates over time due to the house edge, and it helps make the math easier to handle without losing meaning.

So the updated probability formula becomes:

$$
\text{Pro}(n) = \sum_{k=0}^{\infty} \frac{\mu^k}{k!} e^{-\mu}
\begin{cases}
p^{n(1 + a) - k}, & k \leq n \\
1, & k > n
\end{cases}
\quad \text{(4.1)}
$$

And simplifying:

$$
\boxed{
\text{Pro}(n) = 1 - e^{-\mu} \sum_{k=0}^{n} \frac{\mu^k}{k!} \left( 1 - p^{n(1 + a) - k} \right)
}
\quad \text{(5.1)}
$$

> Note: Using $$\( n(1 + a) \)$$ instead of $$\( n + (1 + a) \)$$ doesn’t change the meaning. It’s just a compact way to express the casino’s overall lead due to its advantage.

## Summary and Conclusions

The final formula gives us a mathematical approximation of the **Risk of Ruin** the probability that a casino might go bankrupt due to an unlikely but possible winning streak by a gambler. It combines concepts from **Poisson distribution** and incorporates the **house edge** to model this situation in a more realistic way.

This formula is useful as a tool for risk estimation. With it, a casino (or even a game designer) can assess how likely it is to suffer major losses depending on the game’s win probabilities, betting patterns, and house advantage. It offers a clear and basic way to measure vulnerability, and that could help guide decisions on bet limits or search another strategies for the casino.

While powerful, the model does have some limitations. First, it *assumes independence* between rounds, which doesn’t always reflect how real gamblers behave (for example, people often change bets based on previous wins or losses). Also, it treats all rounds as equal and doesn’t consider human factors like quitting behavior, strategies, or table limits. And although the house edge is included, *other variables* like emotional bias, side bets, or variations in game rules are not.

So, while the formula offers a reliable statistical insight, it’s still a simplified model. And as I said could be a guide, it’s great for exploring theoretical risk, but shouldn’t be used as the only tool when making real-world decisions.

So throughout this project, I faced multiple challenges especially learning more about probability, a topic I hadn’t formally studied before. Even though I found the topic engaging, some of the math behind it (like Poisson approximation and binomial distribution) was difficult at first. However, because I was genuinely interested in the subject, it helped me stay motivated and make sense of the formulas step by step.

This work also helped me revisit previous math concepts like summations and its algebra, limits, factorials, and exponential functions, and it taught me how to formulate a theoretical problem with real-world analogies. Although casinos going bankrupt from one lucky player is rare, the project helped me connect abstract math with practical ideas like managing risk or designing fair systems. Overall, I think the project was worth the effort, both technically and personally.

## References and Their Role

* These resources were key to helping me understand how binomial distributions work, and more importantly, how far they can go when modeling real-world scenarios. They gave me the tools to grasp concepts like success/failure trials and approximation methods that set the foundation for the Poisson approach later on.

  - Das, S. (2016). *A brief note on estimates of binomial coefficients*. http://discretemath.imp.fu-berlin.de/DMII-2015-16/binomials.pdf  
  - FísicayMates. (2022, October 31). *Aproximación de Poisson en la distribución binomial* https://www.youtube.com/watch?v=AM1Lph7MDdM  
  - Matemóvil. (2020). *Distribución binomial | Ejercicios resueltos | Introducción* https://www.youtube.com/watch?v=-XxZGvNClkg

* This group of sources helped me move beyond binomial logic and into Poisson territory. They guided me through the transition to the Poisson approximation, including understanding exponential identities and how limits help simplify complex probability formulas. Without these, simplifying the binomial model and understanding how it behaves as trials approach infinity wouldn’t have been possible.

  - FísicayMates. (2019, October 9). *Distribución de Poisson | ¿Cuándo usarla? | Ejercicio resuelto*. https://www.youtube.com/watch?v=sloXWU-TmDc  
  - Teachoo. *Limits Formula Sheet - Chapter 13 Class 11 Maths Formulas*. https://www.teachoo.com/9725/1564/Limits-Formula-Sheet/category/Limits---Limit-exists/  
  - Math Easy Solutions. (2014, May 6). *e^x as a Limit*. https://www.youtube.com/watch?v=n0VFDT3ZObY  
  - OpenStax. (2013). *4.4 Distribución de Poisson - Introducción a la estadística empresarial*.https://openstax.org/books/introducci%C3%B3n-estad%C3%ADstica-empresarial/pages/4-4-distribucion-de-poisson


* These articles gave me the real-world grounding I needed to understand how the **house edge** works in practice. They don’t just define it they show how it affects gamblers over time, and how it ensures casinos remain profitable in the long run. Their explanations helped me connect math to reality, making that part of the model more meaningful.

  - Maverick, J. B. (2023, September 18). *Why Does the House Always Win? A Look at Casino Profitability*. Investopedia. https://www.investopedia.com/articles/personal-finance/110415/why-does-house-always-win-look-casino-profitability.asp  
  - Mirroman, T., & Ward, B. (2025, April 23). *Everything you need to know about house edge in gambling*. Racingpost.com. https://www.racingpost.com/online-casino/articles/what-is-the-house-edge-in-gambling/
