# Log 4 - Volume

  | Date | Branch | Commit |
  |----------- | ----------- | ----------- |
  | 12/03/2023 | Main | 53538b5 |

## Algorithm update:
At this point we have a profitable solution. One which uses moving averages, and a fixed z-score indicator. Partner has produced some information regarding which spread of maoving averages provided the most profitable solution amongst all the market data provided. Here is the summary:

===============INSERT SUMMARY============================

As you can see that the best pair of moving averages to use would be =======INSERT======, so the moving averages have been changed to:

--- 

## Log Goal:
To optimise this algorithm by adjusting volume of trades

--- 

## Research:

When Adam and I first even mentioned optimisation, it was clear that volume was going to be a big factor in creating a profitable solution.

Here are 3 different ways that we found volume can be manipulated in order to achieve our goals:

1. **Buy the full amount of available volume** This is our current strategy. The idea is simple; if indicators show that the current market price is signalling a buy or sell, why not just order the full amount that is available. If our algorithm is solid and profitable, buying/selling the full amount should only lead to best possible outcome of that signal. 

2. **Set a fixed trade size:** Setting a fixed trade size for each trade is one way to control the volume. For instance, you might decide to execute a pair trade with 100 shares of each stock. This strategy will guarantee consistency in your trades and make it simple for you to monitor your progress over time.

3. **Adjust trade size based on z-score:** Another strategy is to modify your trade size in accordance with the z-score. You would create some kind of propotionality between the z-score and the volume of trade. i.e the (moduluarly) higher the z-score the greater volume we buy since there is a greater probability that the market will move in our favour; a 'stronger signal' if you will. 

Out of these three I have chosen to test No.3 against our current solution, to see if we can possibly get a more profitable solution. Then hopefully we can ramp up the LOT sizes and make some serious money!

---

## Design
The solution to adjusting trade size based on z-score comes down to two main decisions. 

1. What type of proportionality it is. i.e is it directly propositional with some constant (c). Or will it be exponentially proportional to some power constant (c)
2. The magnitude of that constant (c)

So the plan would be to create an implementation of both proportions, and see what different magnitudes produce the most profitable solutions. 

### Direct Proportion
```python
  INPUT: a z_score indicating some kind of signal, constant C, availableVolume 
  OUTPUT: the volume of which we buy

  #mod z-score, we only care about its maginitude of strength
  z_score = |z_score|
  #see how far it is getting away from our indicator of 1
  k = z_score - 1
  #multiply by our constant
  k = k*C

  if k >= 1:
    #definitaly need to buy full amount
    #maybe even implement the idea of buying away from market price because the volume is simply not enough
    VolumetoBuy = availbleVolume
  else:
    VolumetoBuy = availableVolume * k

  return (VolumetoBuy)
```
### Exponential Proportion
```python
  INPUT: a z_score indicating some kind of signal, constant C, availableVolume 
  OUTPUT: the volume of which we buy


  z_score = |z_score|
  k = z_score - 1
  #power by our constant, as long as our constant is less than 1 this works.
  k = k**C

  if k >= 1:
    VolumetoBuy = availbleVolume
  else:
    VolumetoBuy = availableVolume * k

  return (VolumetoBuy)
```

---

## Implementation

###


## Conclusion



## Next Steps



Another thing to consider in the future is to incorporate the minimisation of transaction costs. When trading, transaction costs like commissions and fees must be taken into account. It's crucial to incorporate these costs into your trading strategy because they can reduce your profits. Depending on the anticipated transaction costs, you could change the size of your trades, or you could look for strategies to reduce transaction costs by using inexpensive brokers or trading platforms. In this simulated case, it will be very dependant on purely the size of the trade.

Though my partner disagrees, I think that as long as we get enough profitability, the graze of transaction fees will be minimal. 

### Resources used: 
https://www.forexfactory.com/