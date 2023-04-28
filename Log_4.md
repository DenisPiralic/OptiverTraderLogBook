# Log 4 - Volume Continued

## Algorithm Update
No updates to the algorithm code itself.

My partner did gather some intelligence concerning the optimal pair of moving averages. Here are the results: 

![zscore table](./LOG4PICS/movavg.png)

We found that the sweet spot between the short term moving average and the long term moving average is one where they sit at a ratio of 0.05 to one another. And we want the moving average to really show what the market is telling us, so we want to keep the short term as short as possible (without losing its effectiveness), so a moving average of 5 and 100 seemed to be the best choice moving forward. So hopefully I can get that to work with the volume solution.

![Original](./LOG4PICS/InitialRun.png)

---

## Log Goal
1. To implement a more simplistic volume adjustment on the original algorithm. Minimise computation as much as possible. 

2. Implement the moving averages

---

## Design

Throughout the debugging process of the previous volume solution, I noticed that the z-score was actually very jumpy. It does not gradually change from one value to another (since the market is so volatile). So when we do notice that the signal strength is high, we should act on it straight away, rather than waiting around to see if it will go higher, or lower.

---

## Implementation

As previously done, when the z score is taken in, we see how strong the signal is.

``` python

#see the volume based on z score
K=BASE_LOT_SIZE/LOW_INDICATOR
if abs(self.zscore) >= STRONG_INDICATOR:
    VolumeToOrder = K*STRONG_INDICATOR
elif abs(self.zscore) >= MEDIUM_INDICATOR:
    VolumeToOrder = K*MEDIUM_INDICATOR
elif abs(self.zscore) >= LOW_INDICATOR:
    VolumeToOrder = K*LOW_INDICATOR

VolumeToOrder = int(VolumeToOrder)

```

And then using the original code, I would alternate between buy and sell signals where the volume changes each time.

I also need to choose how I manage the price adjustment for each ask price and buy price. It makes sense to me to only adjust relative to the base lot since that is the lot I am trying to multiply:

``` python
price_adjustment = - (self.position // VolumeToOrder) * TICK_SIZE_IN_CENTS
new_ask_price = ask_prices[0] + price_adjustment if ask_prices[0] != 0 else 0
new_bid_price = bid_prices[0] + price_adjustment if bid_prices[0] != 0 else 0
```

I'll then see if setting the price adjustment relative to every volume change will work after this. 

---
## Results - Using Market_Data_2

With the first implementation of having the base lot of 10 and having the price adjusted only to the base lot

![10BASE LOT, price only adjusted to base lot](./LOG4PICS/10LOT-noPA.png)

Now lets see what happens if we change the price compared to each individual lot volume we buy. But keep base lot to 10

![10 BASE, price adjusted](./LOG4PICS/10LOT-PA-Variable.png)

Now lets see what happens if we pump up that base lot to 20.

![20 BASE, price adjusted](./LOG4PICS/20LOT-PA-Variable.png)

Finally, some excitement! What a huge success

Then we change the base lot to 40 and see what happens

![40 BASE](./LOG4PICS/40LOT.png)

What happened here is that we reached the lot limit way to quickly at the start so it was not profitable. Also, as it stands 40 is way to high, for strong signals it we were buying lot sizes of up to 80 and 100. So it is not efficient. 

Lets try and change the volume to buy code a little bit.

## Implementation 2

``` python

#see the volume based on z score
K=10
if abs(self.zscore) >= STRONG_INDICATOR:
    VolumeToOrder = BASE_LOT_SIZE + 2*K
elif abs(self.zscore) >= MEDIUM_INDICATOR:
    VolumeToOrder = BASE_LOT_SIZE + K
elif abs(self.zscore) >= LOW_INDICATOR:
    VolumeToOrder = BASE_LOT_SIZE

VolumeToOrder = int(VolumeToOrder)

```

## Result 2

This is the result

![40 BASE](./LOG4PICS/40LOT-new.png)

I think if we go back to the original solution and use a base lot of 30. So we can really use the z-score effectively, since I think this is the best way to bullet proof values. 

Unfortuntely, the results are not the best. 

![30 BASE](./LOG4PICS/30LOT-PA.png)

I believe this is due to the fact that having such a large lot size ask means that some trades won't actually go through because there is not enough volume avaible on the market.


---
## Conclusion
In the end, I found that the more simple a solution, the more profitable it is. Adam and I decided to look at every single order individually, so speed is important in that situation. 

Ordering and playing around with too much volume will only lead to an increase risk of things going wrong and prices going out of control.

With a manageable constant volume it allows us to guarantee consistency in our trades and make it simple for us to monitor our progress over time. And with such a quick changing market like this one, it is important to be as consistent as possible.

I am happy with the final product.

---

## Evaluation of goals
I think that through being obssessed with trading profitability and optimising our solution, we had forgotten one of the other key goals set out in the initial log; trading in such a monor which would invite more competition into the market.

One way we could do this is by improving liquidity. If we could figure out a way to offer more prices in a profitable manor, and even more volume so that market participants increase. By attracting more traders in the market, you can increase liquidity and make it easier for everyone, including yourself, to buy and sell the ETF and future contracts.

So deffinetly moving forward, it will be key to see how we can split up trades into multiple different signals in a profitable manor, and split up at different price points at high volumes. However, it will need to be noted that the futures contracts have a lot more volume than ETFs. 


## Next Steps

Though this is my final log, I will continue to play around and hopefully implement something I can use on actual markets. 

For next steps I believe I need to knuckle down on really understanding markets, and how I can manipulate them. 

Looking back, I have been looking at my logs at a data-science angle. When actually, most of the issues I encountered was figuring out how orders in the market worked. I believe that the key to getting into the tens of thousands of dollars of profit comes down to knowing how to deal with order books and filing them in such a way that we can not only benefit from having a difference in price (some profit) but also setting the pace of the market to allow it to go into a direction which would further benefit our position. 

In thoery I understand why an algorithm would want to compete in such a way, it is just about how I would implement that in a practical way in any simulated market. I think that I will need to know how to control my orders in the future, rather than just filing them and then killing them, hoping for the best. 

Adam and I are ready for next year...