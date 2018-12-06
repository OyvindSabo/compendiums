# Market efficiency

## 4.1 The concept of market efficiency

# 4.1.1 Why prices change randomly
If fluctuations caused by sales figures, costs, inflation, taxation, etc. are truly new information, then they are random by nature.

# 4.1.2 Formalization and operationalization

**Efficient market**\
A market in which prices always fully reflect available information is called **efficient**.

**Efficient market hypothesis**\
If the available information set is fully reflected in the expected price, then it is impossible to use that same information set to design trading strategies that have expected excess returns larger than zero. That means that if we at time **t** have information about an extra return at time **t+1**, then that information will be reflected in the current price **P<sub>t+1</sub>**.

If one could be sure the price would rise, it would have already risen.

**Fair game model**\
The **fair game model** directly specifies the expectation of the excess returns ε<sub>i,t+1</sub> regardless of the model being used to produce expected prices or returns:\
**E[ε<sub>i,t+1</sub> | Φ<sub>t</sub>] = 0**

**Martingale model**
The **martingale model** specifies the expectation of excess returns by modeling the time series properties of returns or prices. The expected future price must be the present price times the expected return:\
**E[P<sub>i,t+1</sub> | Φ<sub>t</sub>] = P<sub>i,t+1</sub>(1 + E[r<sub>i,t+1</sub> | Φ<sub>t</sub>])**\
It means that the future prie, properly discounted, is equal to the current price, which makes it a **martingale**.

**Random walk model**\
**Random walks** have the **markov property** of **memorylessness**. THis means that past returns and patterns in past returns cannot be used to predict patterns in future returns. The **random walk model** requires the expected return to be constant and the excess return to be zero, and have constant variance and higher moments in each future period. The expected return of a random walk is called the **drift*.

**The three effiecient market categories**
- **Weak form** market efficiency occurs when all past price histories are fully reflected in the current prices.
- **Semi-strong form** market efficiency occurs when all publicly available information, as well as all price histories are reflected in the current price.
- **Strong form** market efficiency occurs if all information, including inside information is included in the current price.