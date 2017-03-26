---
title:  "Options, Futures, And Other Derivatives Notes - Chapter 1"
date:   2017-03-26 12:00:00
---

I've recently started reading "Options, Futures, And Other Derivatives" (8th edition) by Hull. Logging notes and takeaways here.

Derivatives are traded either on Exchanges or Over the Counter (OTC). On an exchange, the contracts are standardized. OTC trades are completely customizable between financial institutions. They can be much bigger than exchange trades. Exchanges manage counter party risk though, in case the other side reneges on the contract,

A spot contract is an agreement to buy/sell an asset today.

#### Forwards/Futures

A forward contract is an agreement to buy/sell an asset on a future date for a fixed price. Forwards are traded OTC. A future is similar to a forward, but traded on an exchange.

If you agree to buy the asset in the future, you have a long position. Your expected payoff is \\(S_T - K\\), where \\(S_T\\) is the spot price and \\(K\\) is the delivery price of the forward. If the spot price goes up in relation to the delivery price, you make a profit.

Similarly, if you agree to sell the asset in the future, you have a short position and the expected payoff is \\(K - S_T\\).


#### Options

A call option is the right to buy an asset by a certain date for a certain price. A put option is the right to sell an asset by a certain date for a certain price. The price is the exercise/strike price and the date is the expiration date/maturity date.

The vast majority of options traded are American options, which allow the option holder to exercise the option any time up to the expiration date. European options can only be exercised on the expiration date.

Contracts are usually sized in 100 shares.

The bid-offer spread on an option is usually greater than the underlying asset.

The expected payoff for buying a call option is \\(max(S_T - K, 0) - B\\), where \\(B\\) is the bid price. If the spot price is lower than the strike price, then you don't exercise the option and just lose the bid price. If the spot price is higher, you exercise the option and sell the asset on the market, making a profit.

The expected payoff for buying a put option is \\(max(K - S_T, 0) - B\\). If the spot price is lower than the strike price, you buy the asset on the market for the spot price and sell it at the strike price for profit. If the spot price is higher than the strike price, you don't exercise the option and just lose the bid price.

The expected payoff for writing (selling) a call option is \\(min(K - S_T, 0)+ O\\). If the spot price is lower than the strike price, the counterparty doesn't exercise and you make money on the offer price. If the spot price is higher than the strike price, you lose money when it's exercised.

The expected payoff for writing a put option is \\(\min(S_T - K, 0) + O\\). If the spot price is lower than the strike price, you lose money when it's exercised. If the spot price is higher than the strike price, the option isn't exercised and you make money on the offer price.

Options tend to become more expensive as the maturity date increases.

##### Relation between price of an option and strike price

$$sgn(\Delta CallPrice) = -1 * sgn(\Delta K)$$

$$sgn(\Delta PutPrice) = sgn(\Delta K)$$

As the strike price increases, the price of call option decreases.

As the strike price increases, the price of put option increases.

#### Hedging

You can hedge your risk with both forwards and options.

With forwards, you fix the price on the delivery date to neutralize risk. You can still lose money if you're long and \\(S_T < K\\) on the delivery date.

With options, you buy insurance to limit the downside, but keep upside. For example, if you own a stock and are worried it'll go down in price, you can buy a put option at the current price. If the spot price goes down, you can sell it for the strike price and just lose out on the cost of the option. If the spot price goes up, you don't exercise the option and are just out the cost of the option.

#### Speculating

You can speculate on the future price of an asset with both futures and options. Imagine you think the price of an asset will go up in the future.

Without derivatives, you just buy the asset and hold it. Assuming the price goes up, your potential profit is limited by the initial cash you could buy with.

With futures, you can buy a futures contract. By putting cash in a margin account, you can leverage a small amount of cash for a bigger potential profit (or loss).

Similarly with options, you can buy options contracts to leverage a small amount of cash. If the spot price goes up, you can exercise the option for a profit. If the spot price goes down, you don't exercise the option and just lose the option cost.