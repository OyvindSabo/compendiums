# Option pricing in discrete time

## 7.1 Options as securities

### 7.1.1

**Call option**\
A **call option is a contract that gives its holder the right, but not the obligation, to buy something at a specific price on or at any time before a specific date.

**Exercise date**\
The **exercise date** (also called **maturity**) is the date on or before which something can be bought by a call option holder.

**Put option**\
A **put option** is a contract which gives its holder the right, but not the obligation, to sell something at a specified price.

**Exercise price**\
The **exercise price** (also called **strike price**) is the specified price at which a put option holder can sell something.

**Exercise**\
To **exercise** an option is to buy utilize the right an option gives to buy or sell something.

**Write**\
To **write** an option is to sell an option. The writer of a sell option is obliged to sell at the buyer's request and the writer of a put otion is obliged to buy at the seller's request.

**Option premium**\
The **option premium** is the price that you pay when buying an option, or that you receive when selling an option. An option seller (**writer**) cannot expect to earn any money when an option is exercised. The writer is compensated for the expected loss in the **option premium**.

**Example without using any option pricing model**
- You own 1000 shares of a stock.
- The stock is currently trading at €100 per share.
- You bought the stock at €60.
- You do not want to sell the shares immediately, because you think they will continue to increase in value.
- You have to sell the shares in six months.
- You cannot sell them for less than €90 per stock.

Let's create an option which suits your needs. Since you want to be guaranteed to be able to sell the shares, you have to buy a put option. Any option for **1000 shares** which satisfies the following characteristics will suffice:

**exercise price * 1000 - option premium ≥ €90 * 1000**

If we assume that we find a put option which gives the right to sell at the current prie in the future, then...

**€100 * 1000 - option premium ≥ €90 * 1000**

**option premium ≤ (€100 - €90) * 1000**

**option premium ≤ €10 000**

This means that for you, an option to sell at the current price in six months is worth €10 000 in option premium.

**Example of evaluating an option using a model**
