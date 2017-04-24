---
title:  "Options, Futures, And Other Derivatives Notes - Chapter 2"
date:   2017-04-23 17:00:00
---

## Mechanics of Futures Markets

#### Future Contract Specification

An exchange can specify many attributes of the contract.

Grade. If a range of grades can be delivered, the exchange specifies how the price changes based on the grade.

Amount of an asset per contract. Has to be small enough to attract investors, but large enough to make trading fees worth it.

Where the asset is delivered. If multiple options are offered, price may be adjusted based on location chosen by the party with the short position.

When contracts start/stop trading and when they are delivered (usually anytime in the delivery month). The *first notice day* is the first day the party with the short position can give notice to deliver. The *last notice day* is the last day. The *last trading day* for the contract is usually a few days before the last notice day.

How prices are quoted (dollars, fractions of dollar, cents, etc.)

Maximum daily price movement. If price goes down the limit, it's *limit down*. *limit up* for the opposite case. In general, this is a *limit move*. Trading can stop when this happens, or exchange can modify limits.

The exchange can limit how much of a contract any given speculator may hold.

#### Margins

Margins are used to avoid a party reneging on the futures contract.

When entering a long position, the broker requires investor to deposit an *initial margin* in a *margin account*. If the futures price goes up, the margin account increases in value and the investor may redeem the excess of the initial margin. If the futures price goes down, the margin account decreases in value. If it hits the *maintenance margin*, the investor is required to top it off up to the initial margin. If they don't, the broker can close out the position.

The initial margin can be payed in cash or with securities. Subsequent calls have to be in cash. 

If the futures price goes up, the long position broker has to pay the exchange and the exchange pays the short position broker.

Brokers pay interest on the margin account.

The minimum initial and maintenance margin are specified by the exchange. The broker may have higher minimums.

A clearing house is an intermediary of futures transactions. Clearing house members have a margin account (*clearing margin*) with the house. Brokers who are not clearing house members go through a member and have a margin account with them.

Clearing house members must always maintain the original margin in their margin account. There is no maintenance margin. The margin is usually based on the net basis (difference between outstanding long and short positions). It can also be on the gross basis (sum of outstanding long and short positions).

#### OTC Markets

In OTC markets, participants can enter a collateralization agreement. This is similar to a margin account. If the value of the contract increases for A, B is required to pay A the difference. A will pay interest on this. This is used a security deposit to make sure a party doesn't renege.

Since the 2007-2009 crisis, some OTC transactions require having an intermediary clearing house. This is similar to the exchange market.

#### Market Quotes

Market quotes have multiple interesting fields.

*Open* is the price at start of trading that day.

*High* is the highest price for trading that day.

*Low* is the lowest price for trading that day.

*Settlement price* is the price at end of day. It's used for calculating daily gains/losses and margin requirements.

*Volume* is the number of contracts traded that day.

*Open interest* is the number of outstanding contracts.

#### Delivery

Most futures contracts are not delivered. The owner of the contract tends to take the other side to close out the position. e.g. If you're long 100 units, you sell (short) 100 units to avoid delivery. The profit/loss is based on the price difference from when you entered the position to when you closed it.
 
For actual delivery, the owner of the short position files a *notice of intention to deliver* with the exchange, specifying the grade of the asset and the location of the delivery. The exchange then picks a party with a long position to accept the delivery. If the notice is transferable, the party with the long position can find another party with a long position to accept the delivery.

The price paid at the delivery is the most recent settlement price. Imagine you were long at price \\(X\\), and the settlement price went up to \\(Y\\), You now need to pay \\(Y\\). However, since you held the contract, you received \\(Y-X\\) from margin. Your net price is \\(Y - (Y - X) = X\\).

Some financial futures are settled in cash since it's difficult/impossible to deliver the underlying instrument. Indices are an example. The settlement price is determined by either the open/close spot price on an predetermined day.

#### Types of Orders

There are many types of orders an investor can place with a broker.

A *market order* is executed immediately at the best available price.

A *limit order* is executed only at the specific (or more favorable price). A limit buy order at \\(X\\) is only carried out at \\(X\\) or below.

A *stop order* / *stop-loss order* is executed when there is a bid/offer made at a specific (or less favorable price). This helps close out a position and limit losses if the market moves in an unfavorable direction.

A *stop-limit order* is a combination of a limit order and a stop order. This becomes a limit order of \\(X\\) when the market hits price \\(Y\\). \\(X\\) and \\(Y\\) can be the same. You may want to do this if you want to buy a contract once it starts increasing in price. The stop portion will trigger the order, while the limit portion will make sure you don't buy at too high a price.

A *market-if-touched* order is executed when a trade occurs at a specific (or more favorable price). This is similar to a stop order. Primary difference is a stop order is used to minimize losses, while a market-it-touched order is used to ensure profits are taken. For example, if you're long at price $50, you can place a stop order to sell at $40 and market-if-touched order to sell at $60. If the price goes down to $40, you sell immediately and cut your losses. if the price goes up to $60, you sell immediately and guarantee a profit.

A *discretionary order* is similar to a market order, but the broker may delay execution to try to get a better price.

#### Accounting

For hedge contracts, gains/losses are realized in same period as gains/losses are realized for the items being hedged.

For non-hedge contracts, gains/losses are realized in the period they occur. This can span multiple accounting periods.