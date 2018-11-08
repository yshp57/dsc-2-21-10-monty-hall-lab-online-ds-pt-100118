
#  Bayes' Rule and The Monte Hall Problem - Lab

## Introduction

This lab requires you to deal with one of the popular probability problems. i.e. the Monty Hall Problem. Do some quick background search about Monty Hall and his quiz show to see how this problem came about. Here we shall quickly introduce the problem and take you through a series of exercises towards its solution.

## Objectives

You will be able to:
* Understand and describe the Monty Hall problem in probabilistic terms
* Solve the Monty Hall problem using Bayesian Logic and mathematical manipulations
* Run a simulation to find the optimal answer for the problem, and check if it matches your calculations

## The Problem

#### You are on a game show, and you're given the choice of three doors.  Behind one door is a car, behind the others, goats. You pick a door, say No. 1, and the host, who knows what's behind the doors, opens another door, say No. 3, which has a goat. He then says to you, "Do you want to pick door No. 2?" 

### Is it to your advantage to change your choice , or would you rather stick with your initial choice?

<img src="https://i.ytimg.com/vi/4Lb-6rxZxx0/maxresdefault.jpg" width=600>

This is a classical probability problem and shown here in the simplest form. Wikipedia maintains an excellent document on this problem , explanations and solutions presented, along with a critique of approaches. [Visit think link](https://en.wikipedia.org/wiki/Monty_Hall_problem) and have a quick read through introduction part to understand why this is such a popular puzzle to solve. 

Once you clearly understand the problem, answer following questions:


## Part A: 
What does your intuition say?  Is it in your best interest to switch the door ? 


```python
# Your solution here  
```

***
**Solution**:  It is in your interest to switch.  It turns out that if you don't switch doors you have a 1/3 probability of winning the car.  If you do switch doors you have a 2/3 probability.  We'll prove this using Bayes rule in the next part, but if this seems sketchy to you, consider a modified game where there are 100 doors with 99 goats and one car.  After making your guess the host (knowing where the car is) opens 98 doors that have goats behind them.  Now does your intuition change? 
***

## Part B
Let's assume that you pick door number 1 and Monty opens door number 3.  The question then is whether you stick with door number 1 or switch to door number 2. Let $D_i$ be the event that the car is actually behind door $i$.  Let $H$ be the event that Monty opens door number 3. First, compute $P(H \mid D_i)$ for $i=1,2,3$.  You'll need to think carefully about the particular situation described above.


```python
# Your solution here 
```

***
**Solution**: The likelihoods, given our assumptions, are as follows: 

- $D_1$: If the car is behind Door 1 then it doesn't matter which of Doors 2 and 3 that Monte opens.  Thus this probability is $P(H \mid D_1) = \frac{1}{2}$

- $D_2$: If the car is behind Door 2 then Monte must open Door 3.  Thus this probability is $P(H \mid D_2) = 1$

- $D_3$: If the car is behind Door 3 then there is no way that Monte will open Door 3.  Thus this probability is $P(H \mid D_3) = 0$

***

## Part C
Use your results from **Part B** and the Law of Total Probability to compute $P(H)$


```python
# Your solution here 
```

***
**Solution**: Assuming that it is equally likely that the car is behind any door, we have $P(D_i) = \frac{1}{3}$ for $i=1, 2, 3$. We then have 

\begin{eqnarray}
\nonumber P(H) &=& P(H \mid D_1)P(D_1) + P(H \mid D_3)P(D_3) + P(H \mid D_3)P(D_3) \\
\nonumber      &=& \frac{1}{2}\cdot\frac{1}{3} + 1\cdot\frac{1}{3} + 0\cdot\frac{1}{3} \\ 
\nonumber      &=& \frac{1}{2}
\end{eqnarray}

***

## Part D 
Now, use Bayes' Rule to compute $P(D_i \mid H)$ for $i=1$ and $2$ (because these are the doors we care about). 


```python
# Your solution here
```

***
**Solution**: From Bayes' Rule, we have 

$$
p(D_1 ~|~ H) = \frac{P(H \mid D_1)P(D_1)}{P(H)} = \frac{\frac{1}{2} \cdot \frac{1}{3}}{\frac{1}{2}} = \frac{1}{3} ~~~~~~~~ \textrm{and} ~~~~~~~~
p(D_2 ~|~ H) = \frac{P(H \mid D_2)P(D_2)}{P(H)}  = \frac{1 \cdot \frac{1}{3}}{\frac{1}{2}} = \frac{2}{3}
$$

Thus if we switch we have a probability of $\frac{2}{3}$ of winning the car. 

***

## Part E

Write some simple code in Python and Numpy to simulate the Monte Hall problem and verify your results from **Parts A-D**. We are providing you with the structure of the code, fill it the formulas for calculations/polling/switching etc. 


```python
import numpy as np 
def make_a_deal(switch=True):
    doors = list(range(3))
    car = np.random.choice(doors)
    first_choice = np.random.choice(doors)
    montes_options = list(set(doors)-set([car])-set([first_choice]))
    goat = np.random.choice(montes_options)
    final_choice = (set(doors)-set([goat])-set([first_choice])).pop() if switch else first_choice
    return final_choice == car

def monte_hall_sim(switch=True, num_trials=int(1e3)): 
    winners = np.sum([make_a_deal(switch) for kk in range(num_trials)])
    state = "switching)" if switch else "not switching)"
    print("P(winning by "+state+" = {:.4f}".format(winners/num_trials))
```


```python
monte_hall_sim(switch=True, num_trials=int(1e5))

# P(winning by switching) = 0.6675
```

    P(winning by switching) = 0.6675



```python
monte_hall_sim(switch=False, num_trials=int(1e5))

# P(winning by not switching) = 0.3351
```

    P(winning by not switching) = 0.3351



```python
# Record your observations here 
```

## Summary 

In this lab, we used Bayesian calculations to solve the Monty Hall problem. We also looked at running a simulation in Numpy to prove our results through repeated random sampling from the given probability distributions. We found the results to be same as what we calculated by hand. Once again, we see how Bayesian logic can truly reflect a real life phenomenon in terms of prior and posterior knowledge. 
