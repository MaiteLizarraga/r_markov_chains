# Stocastic Processes and Discrete Time Markov Chains-project in R

Knowing how to model a system using discrete-time dynamical systems can be very useful in the field of data science. This is exactly what we aim to achieve with this project.

## Starting hypotheses

According to Kemeny, Snell and Thompson (Introduction to Finite Mathematics), the Land of Oz is fortunate in many things, but not in nice weather:

They never have two nice days in a row.

*   If it rains, the probability that the next day will be sunny is 1/4.
*   If it rains, the probability that it will snow the next day is also 1/4.
*   If they have a nice day, they are equally likely to have snow or rain the next day.
*   If it snows, the next day, half the time, they have a nice day.
*   If it snows, the probability that the next day it will snow again is α times higher than the probability that it will rain the next day.

## Step 1: Transition State Matrix Creation and Graphic representation of the Markov Chain.

We start by installing the required packages and we go on to create the transition matrix. We need to take into account that there are several probabilities that are not given to us and that we need to calculate using the Law of Total Probability.

*   SOL: Sunny (S)
*   LLUVIA: Rainy (L)
*   NIEVE: Snowy (N)

![R Project transition state matrix creation](https://github.com/MaiteLizarraga/r_markov_chains/blob/main/capturas/1-transition-state-matrix-creation.jpg)
![R Project transition state matrix](https://github.com/MaiteLizarraga/r_markov_chains/blob/main/capturas/transition_state_matrix.png)

## Step 2: Markov Chain Creation in R.

We generate the markov chain, called the P matrix, by rows, and check that the sum of the rows of P equals 1. We addition the columns as well.

![R Project Markov Chain creation](https://github.com/MaiteLizarraga/r_markov_chains/blob/main/capturas/3-Markov-Chain-creation.jpg)

## Step 3: What is the probability that next Sunday will also be sunny?

Let's calculate the conditional probability that it will be sunny on Sunday given that Monday was sunny. To do this, we will use our transition matrix P, which indicates the probability that the chain passes from one state to another in a unit of time, and Pi, the vector of state probabilities, for our initial state (sunny).

![R Project future probability](https://github.com/MaiteLizarraga/r_markov_chains/blob/main/capturas/4-future-probability.jpg)

> Vector of the current day Pi0 - it is sunny:
> Pi0 <- c(1,0,0)

> Vector we want to find: within 6 days - Sunday:
> Pi6

We know from theory that we can use the Chapman-Kolmogorov formula to calculate future states: the probability vector of a future state n will be the product of the probability vector of the current state, our Pi0, and the transition matrix P raised to the n-th future state. We perform this calculation and we get the probabilities for next sunday.

![R Project sunday probability](https://github.com/MaiteLizarraga/r_markov_chains/blob/main/capturas/5-sunday-probability.jpg)

## Step 4: What is the probability that next Sunday will be sunny GIVEN THAT it snowed on Monday?

As in the previous exercise, we calculate the conditional probability that it will be sunny on Sunday given that it snowed on Monday. In order to do this we are going to take the product of the P matrix raised to n for the vector of probabilities of the current day.

![R Project sunday snow probability](https://github.com/MaiteLizarraga/r_markov_chains/blob/main/capturas/6-future-probability-2.jpg)

> Current day vector Pi0nieve - it is snowing:
> Pi0nieve <- c(0,0,1)

> Vector we want to find: within 6 days - Sunday:
> Pi6nieve

We see that the probability of sunshine on Sunday after snowfall on Monday is 26.77%, somewhat lower than in the previous case.

![R Project sunday snow probability](https://github.com/MaiteLizarraga/r_markov_chains/blob/main/capturas/7-snow-probability.jpg)

## Step 5: If it is sunny today, what is the probability that it rained yesterday? And what is the probability it snowed yesterday?

In this case, we need to call upon the Bayes' Theorem and the Law of Total Probability.

> BAYES: p(n=L|n+1=S) = p(n+1=S|n=L) * p(n=L) / p(n+1=S)

If it's sunny today, the probability that it rained yesterday is 33.33%.
With regards to the snow, the probability that it snowed yesterday if it's sunny today is 66.67%.

![R Project prediction in the past](https://github.com/MaiteLizarraga/r_markov_chains/blob/main/capturas/8-prediction-in-the-past.jpg)

## Step 6: Is this chain regular or irregular?

The chain is regular because when we raise P to several integers, we see that all pij are positive.

## Step 7: Calculate the stationary distribution

Stationary (steady) state = limit state distribution. This stationary state represents the probabilities of being in each state after a large number of steps (or transitions), assuming the system is ergodic and has converged to a steady state.

#### We check that P and Pt (the transposed matrix of the P matrix) have an eigenvalue = 1

The eigenvalue of 1 signifies that 𝜋, the stationary distribution, remains unchanged after applying the transition matrix 𝑃. This property is essential for describing the long-term behavior of a Markov chain, as the system stabilizes in the stationary distribution over time. All other eigenvalues of 𝑃 are typically less than 1 in absolute value (assuming an irreducible, aperiodic Markov chain), which means that, over time, the effect of any initial distribution decays, and the system approaches the stationary distribution 𝜋.

Moreover, a Markov chain is reversible if it satisfies the detailed balance condition. In reversible Markov chains, Pt has the same eigenvalue spectrum as 𝑃, and importantly, both 𝑃 and Pt share the eigenvalue 1. The eigenvalue of 1 for Pt ensures that there exists a right eigenvector corresponding to 𝜋𝑇, meaning that the transpose of 𝜋 (the stationary distribution) is a right eigenvector of 𝑃𝑇.

The following conditions describe a Markov chain in steady-state, where 𝜋 is the stationary distribution.

#### 1. First Condition: All the elements of 𝜋 are positive and their sum is 1 (for P and Pt)
*    𝜋 represents a probability distribution over the states of the Markov chain.
*    The requirement that all elements of 𝜋 are positive and sum to 1 ensures that 𝜋 is a valid probability distribution. This distribution describes the long-term behavior of the Markov chain

#### 2. Second Condition: 𝜋⋅𝑃=𝜋 and 𝜋𝑖≥0 (for P and Pt)
*    P is the transition matrix of the Markov chain, where each element 𝑃𝑖𝑗 gives the probability of transitioning from state 𝑖 to state 𝑗.
*    The equation 𝜋𝑃=𝜋 is the stationary distribution equation. It means that the distribution 𝜋 does not change after applying the transition matrix 𝑃.
*    The condition 𝜋𝑖≥0 again ensures that 𝜋 is a valid probability distribution (no negative probabilities).

![R Project stationary distribution](https://github.com/MaiteLizarraga/r_markov_chains/blob/main/capturas/9-stationary-distribution.jpg)

End result:

![R Project end result](https://github.com/MaiteLizarraga/r_markov_chains/blob/main/capturas/10-end-result.jpg)