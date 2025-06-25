# Where is the math in Casino Risk of Ruin by Ivan Contreras Arvizu
This repository explores the mathematical principles behind the concept of “Risk of Ruin” in casinos. It introduces a probabilistic model that estimates the likelihood of a casino going bankrupt, based on concepts like binomial distributions, Poisson approximation, and house edge.

## Introduction
Since I was young, I’ve been curious about how randomness and chance work. Although I never took formal courses in probability, I’ve always wanted to understand it better. When I had the chance to freely choose a topic, I saw this project as the perfect opportunity to explore probability through something unusual but engaging: the **Risk of Ruin** in casino games.

That curiosity led me to the following question:  
**Can a casino actually go bankrupt if a gambler gets lucky enough?**  
This is the essence of the Risk of Ruin (ROR) the probability that a casino loses all its capital due to a rare sequence of wins by a player. While casinos are designed with a statistical advantage (house edge), randomness introduces a non-zero risk of failure.

To analyze this, I study a probabilistic model based on **binomial distributions**, **Poisson approximations**, and the concept of **house edge**, inspired by the paper from Siu et al. (2023). This project allowed me to apply mathematical modeling techniques commonly used in real world for analyzing failure, uncertainty, and risk to a dynamic and unpredictable scenario.

> **Reference:**  
> Siu, K.-M., Chan, K.-H., & Im, S.-K. (2023). A study of assessment of casinos’ risk of ruin in casino games with Poisson distribution. *Mathematics, 11*(7), 1736.  
> https://doi.org/10.3390/math11071736

### Motivational videos
Before diving into the math, here are a couple of short videos that introduce some of the key concepts in a simple and engaging way. They caught my attention from the start I hope they do the same for you.
- [YouTube – Introductory Explanation](https://www.youtube.com/watch?v=ErunjUChFdg)
- [TikTok – Concept Poisson](https://vt.tiktok.com/ZSkrYqc4X/)
- [TikTok – Concept Binomial](https://vt.tiktok.com/ZSkr2R3Wo/)
  
## Preliminary Video
This is an intrioductory video is a visual summary of my project *“Where is the math in the Casino’s Risk of Ruin?”* It introduces the key ideas behind the concept, such as how a casino can lose money over time despite having the advantage. I also explain in very basic terms the mathematical tools used like binomial, Poisson distributions, and the house edge.  
[Watch the full video on YouTube](https://www.youtube.com/watch?v=TU_LINK_AQUI)

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
\end{cases}
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
\end{cases}
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

The binomial coefficient:
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
\binom{100}{2} = \frac{100 \cdot 99}{2} = 4950
$$

Now compare:

$$
\frac{100^2}{2!} = \frac{10000}{2} = 5000
$$

> **Note:**  
> Even though $$\( \binom{t}{k} \)$$ and $$\( \frac{t^k}{k!} \)$$ are not exactly equal, they become very close when `t` is large and `k` is small.  This is why we can replace the binomial coefficient with a simpler form when applying the **Poisson approximation** — it simplifies the math significantly, while still giving us a good estimate.


$$
\binom{t}{k} \approx \frac{t^k}{k!}
$$

---

##### 2. **Simplifying the exponential term**:

As `t → ∞`, we use the identity:

$$
\lim_{t \to \infty} \left(1 - \frac{\mu}{t} \right)^{t} = e^{-\mu}
$$

So the full expression becomes:

$$
\lim_{t \to \infty} \binom{t}{k} \cdot \left( \frac{\mu}{t} \right)^k \cdot \left(1 - \frac{\mu}{t} \right)^{t - k}
= \frac{\mu^k e^{-\mu}}{k!}
$$

---

#### What does this mean?

We now have a clean, simple way to calculate the probability that the gambler wins exactly `k` times **when the number of games is very large**:

$$
P(X = k) = \frac{\mu^k e^{-\mu}}{k!}
$$

This is the **Poisson distribution**, and it becomes the foundation of the final model.

