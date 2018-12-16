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

Note that significant underperformance does not contradict the efficient market hypothesis, but significant overperformance does.

**Fair game model**\
The **fair game model** directly specifies the expectation of the excess returns ε<sub>i,t+1</sub> regardless of the model being used to produce expected prices or returns:\
**E[ε<sub>i,t+1</sub> | Φ<sub>t</sub>] = 0**

**Martingale model**\
The **martingale model** specifies the expectation of excess returns by modeling the time series properties of returns or prices. The expected future price must be the present price times the expected return:\
**E[P<sub>i,t+1</sub> | Φ<sub>t</sub>] = P<sub>i,t+1</sub>(1 + E[r<sub>i,t+1</sub> | Φ<sub>t</sub>])**\
It means that the future prie, properly discounted, is equal to the current price, which makes it a **martingale**.

**Random walk model**\
**Random walks** have the **markov property** of **memorylessness**. This means that past returns and patterns in past returns cannot be used to predict patterns in future returns. The **random walk model** requires the expected return to be constant and the excess return to be zero, and have constant variance and higher moments in each future period. The expected return of a random walk is called the **drift*.

**The three effiecient market categories**
- **Weak form** market efficiency occurs when all past price histories are fully reflected in the current prices.
- **Semi-strong form** market efficiency occurs when all publicly available information, as well as all price histories are reflected in the current price.
- **Strong form** market efficiency occurs if all information, including inside information is included in the current price.

### 4.1.3 Empirical implications

**Underreaction**\
When new information becomes available, if the market does not immediately adjust to reflect the new information, then it means that the market is underreacting. An **underreacting** market is not efficient, and can be exploited by traders, but by doing so, they effectively make the market more efficient.

**Overreaction**\
A market is said to be **overreacting** if the price adjustment overestimates the change in expected value after new information becomes available. If for instance, the the new information would imply a 10% drop, the actual price may drop 20%, and then readjust to reflect the expected value after some time. Just like underreactions, overreactions can be exploited by traders. By doing so, they make the market more efficient.

### 4.2.5 Event studies

**Event studies**\
**Event studies** measure the effect of a well-defined event on firm value. **Event studies** are designed to answer whether price development follows developments on the market, or if it contradicts the semi-strong form of the **efficient market hypothesis**.

**Market model**\
The **market model** is a model used to filter out the continuous stream of other, more general financial information about interest and exchange rates, prices of other stocks bonds and commoditie as well as mactro-economic information, using the firm's historical sensitivity for changes in the market index. The model calculates the **normal return** which could be expected if it wasn't for the event which is being studied.

**r<sub>it</sub>** = return of an individual index.\
**r<sub>mt</sub>** = return of the market as a whole.\
**γ<sub>0,1</sub>** = estimated coefficients.\
**ε<sub>it</sub>** = an error term.\
**t** = time

**r<sub>it</sub> = γ<sub>0i</sub> + γ<sub>1i</sub>r<sub>mt</sub> + ε<sub>it</sub>**

**Abnormal return**\
The **abnormal return** is the difference between the actual return (**realized return**) and the return according to the market model (**normal return**). The **abnormal return** is the part of the return which is attributed to the event.

**Calculate Abnormal return: Mean-adjusted returns model**\
The **mean-adjusted returns model** (**MAR**) calculates **abnormal return** (**AR**) by simply looking at the difference between the return on a stock a particular day, and the average return on that stock.

**AR<sub>t</sub> = R<sub>t</sub>-R̄<sub>j</sub>**

**Calculate abnormal return: Market-adjusted returns model**\
The **market-adjusted returns model** (**MKAR**) calculates **abnormal return** (**AR**) by taking the difference between the return on a stock on a particular day and the corresponding return on a stock market index.

**AR<sub>t</sub> = R<sub>t</sub>-R<sub>Mt</sub>**

**Calculate abnormal return: Risk-adjusted returns model**\
The **risk-adjusted returns model** (**RAR**) is the most superior model for calculating **abnormal return** (**AR**) because it considers risk adjustments. It taking the difference between the return on a stock on a particular day and a systematic component which takes the stock's risk into account.

**AR<sub>t</sub> = R<sub>t</sub>-(α + ꞵR<sub>M,t</sub>**

**Event window**\
The **event window** is the period over which we want to analyze the impact of an event. If we continue to observe significant abnormal returns after the event window, then we have to question the validity of the semi-strong form efficient market hypothesis with respect to the particular event. Also, if we see abnormal return within the event window before the actual event occured, then that is a sign of insider trading.

**Estimation window**\
The **estimation window** is the period over which we estimate the market model. This is where we find the **normal return** (the average return of a particular stock).

**CAAR**\
**Cumulative average abnormal return** (**CAAR**) are calculated like this:
- Calculate daily abnormal returns in the days surrounding the event.
- Calculate the average abnormal return (“AAR”) for each day in the event window.
- Finally, sum the average abnormal returns over the T days in the event window (i.e. over all times t) to form the cumulative average abnormal return (CAAR).

**AR<sub>it</sub>** = Abnormal return of firm i over period t\
**r<sub>it</sub>** = observed (realized) return of the firm\
**(α<sub>i</sub> + β<sub>i</sub>r<sub>mt</sub>)** = normal return of firm i over period t according to the market model

**AR<sub>it</sub> = r<sub>it</sub> - (α<sub>i</sub> + β<sub>i</sub>r<sub>mt</sub>)**

The CAAR is a useful statistical analysis in addition to the AAR because it
helps us get a sense of the aggregate effect of the abnormal returns. Particularly if the influence of the event during the event window is not
exclusively on the event date itself, the CAAR can prove very useful.

**Sample selection bias**\
**Sample selection bias** is a type of bias caused by choosing non-random data for statistical analysis. The bias exists due to a flaw in the sample selection process, where a subset of the data is systematically excluded due to a particular attribute. The exclusion of the subset can influence the statistical significance of the test, or produce distorted results.

**Example of event study**\
Given a table of studies of 113 individual public events, each affecting their respective companies...
|                             | All companies | Resource companies | Non-resource companies |
|-----------------------------|---------------|--------------------|------------------------|
| Number of companies         | 113           | 12                 | 101                    |
| CAAR from day -20 to day -3 | -3.90%        | -2.72%             | -4.33%                 |
| t-statistic                 | -2.23*        | -0.60              | -2.30*                 |
| CAAR from day -2 to day +2  | 0.94%         | 0.92%              | 1.67                   |
| t-statistic                 | 0.99          | 0.45               | 1.15                   |
| CAAR from day +3 to +20     | -3.95%        | 1.28%              | -4.42%                 |
| t-statistic                 | -2.44*        | 0.39               | -2.48*                 |
**\*** means CAAR i significantly ≠ 0.\
**t-statistic** describes how extreme the deviation from the mean is in proportion to the error.

Let's find out if any of the results in this study contradicts the efficient market hypothesis. According to the efficient market hypothesis, the CAAR should be about the same before the event window as after the event window.

Together, all companies had a CAAR of -3.90% before the event and -3.95% after the event. This is a very minimal change. However, the t-statistic suggests that the change is larger than what can be expected from the error, so it does indeed contradict the efficient market hypothesis.

Resource companies went from a CAAR of -2.72% before the event to 1.28% after the event. This is a significant change which suggests an underreaction from the market to the event. This initially appears to contradict the semi-strong form of the efficient market hypothesis, since this underreacting behavior could be used to predict the future. However, the t-statistic is not large enough to be sure that the change in CAAR is larger than the error, which means that we cannot be sure that this cotradicts the efficient market hypothesis.

The non-resource companies went from a CAAR of -4.33% before the event to -4.42% after the event. This isn't a very large change, but the t-statistic suggests that the change is larger than what should be expected from the error, so this contradicts the efficient market hypothesis.

In summary, the behavior observed for all companies and non-resource companies contradict the efficient market hypothesis. The event is public information, which means that it is the semi-strong efficient market hypothesis which is being contradicted.