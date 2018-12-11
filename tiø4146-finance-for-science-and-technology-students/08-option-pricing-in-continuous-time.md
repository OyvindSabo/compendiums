# Option pricing in continuous time

## 8.3 Working with Black and Scholes

### 8.3.1 Interpretation

**Black and Scholes formula**\
The **Black and Scholes formula** is used to calculate the price of a European-style (can only be exercised at its maturity date) call option.

T = time to maturity in fraction of interst-giving periods (e.g. 0.247 if 90 days to maturity and interest is given for one year)\
S = Spot price of the asset (the current value of the asset which the option is for).\
r = risk free rate.\
X = exercise price.
N(d<sub>2</sub>) = probability that the option will be finished in the money and exercised

O<sub>C,0</sub> = (S<sub>0</sub>) * N(d<sub>1</sub>) - (Xe<sup>-rT</sup>) * N(d<sub>2</sub>)

**Example**\
Consider the following option:
- An asset is worth €120 million right now.
- The option offers the right to buy an asset for €120 million two years from now.
- The market price of the asset will either increase 15% or be reduced by 13% for each years.
- The risk free rate is 4%.

Let's find the value of this option using the **Black and Scholes formula**.

T = time to maturity in fraction of interst-giving periods = 2\
S = Spot price of the asset = €120 million\
r = risk free rate = 4% = 0.04\
X = €120 million

To find N(d<sub>1</sub>) and N(d<sub>2</sub>) we need to first find d<sub>1</sub> and d<sub>2</sub>.

σ = volatility of returns = ln(1.15) = 0.14 //Because 1.15 is the deviation of the asset price per year

d<sub>1</sub> = <sup>1</sup>/<sub>σ*sqrt(T)</sub>(ln(<sup>S</sup>/<sub>X</sub>) + (r + <sup>σ<sup>2</sup></sup>/<sub>2</sub>)T)\
d<sub>1</sub> = <sup>1</sup>/<sub>0.14 * sqrt(2)</sub>(ln(<sup>€120 million</sup>/<sub>€120 million</sub>) + (0.04 + <sup>0.14<sup>2</sup></sup>/<sub>2</sub>)2) = 0.503

d<sub>2</sub> = d<sub>1</sub> - σsqrt(T)\
d<sub>2</sub> = 0.503 - 0.14* sqrt(2) = 0.305

From a table, we can find that this gives:

N(d<sub>1</sub>) = N(0.503) = 0.691\
N(d<sub>2</sub>) = N(0.305) = 0.637

O<sub>C,0</sub> = (S<sub>0</sub>) * N(d<sub>1</sub>) - (Xe<sup>-rT</sup>) * N(d<sub>2</sub>)\
O<sub>C,0</sub> = €120 million * 0.691 - (€120 000 000 * e<sup>-0.04 * 2</sup>) * 0.637\
O<sub>C,0</sub> = €12.36 million