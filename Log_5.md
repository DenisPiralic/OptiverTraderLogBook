# Log 5 - Volume Continued

## Algorithm Update
No updates to the algorithm code itself.

My partner did gather some intelligence concerning the optimal pair of moving averages. Here are the results: 

![zscore table](./LOG5PICS/movavg.png)

We found that the sweet spot between the short term moving average and the long term moving average is one where they sit at a ratio of 0.05 to one another. And we want the moving average to really show what the market is telling us, so we want to keep the short term as short as possible (without losing its effectiveness), so a moving average of 5 and 100 seemed to be the best choice moving forward. So hopefully I can get that to work with the volume solution.

## Log Goal
1. To implement a more simplistic volume adjustment on the original algorithm. Minimise computation as much as possible. 

2. Implement the moving averages

## Design

Throughout the debugging process of the previous volume solution, I noticed that the z-score was actually very jumpy. It does not gradually change from one value to another (since the market is so volatile). So when we do notice that the signal strength is high, we should act on it straight away, rather than waiting around to see if it will go higher, or lower.




## Next Steps

Though this is my final log, I will continue to play around and hopefully implement something I can use on actual markets. 

For next steps I believe I need to knuckle down on really understanding markets, and how I can manipulate them. 

Looking back, I have been looking at my logs at a data-science angle. When actually, most of the issues I encountered was figuring out how orders in the market worked. 

So I need to research more thoroughly, how making orders on a market actually works. Then I can look at making 