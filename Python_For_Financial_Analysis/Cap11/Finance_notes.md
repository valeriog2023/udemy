# Capital Assets Pricing Model (CAPM)

Describes Risk and help separate market return vs Portfolio  startegy return

Remember that a Portfolio is a set of weighted securities:

```
            n
r_p(t) == SUM( w_i * r_i(t)  )
            i 

This is the return of the portfolio at time t
```

We can also consider the entire market (or maybe the SP500) as a portfolio where the weights are given by
the **market cap** ,i.e. : `# of shares * price`

So we have 
```
              MarketCap_i
W_i  == -------------------------- 
             n
          SUM (MarketCap_j)
             j
``` 

The CAPM equation states the following:
```

r_i(t)  ==  Beta_i * r_m(t)  + Alpha_i(t)

```
The return of a specific instance i is equal to 2 terms (both at time t):
- the capital market return scaled of a factor Beta specific for  the instance i
- an additional term specific to the instance i 

This is very similar to a simple regression line y = (m * x) + b

E.g. If Beta = 1 than the stock moves up and down with the market; if it is = 2 then it goes up and down
twice as fast as the market. The alpha term is used to adjust the actual value of the stock, however 
CAPM states that you should expect the Alpha_i term to be zero!
Note that (he says) 
- given that CAPM expects alpha to be zero, it should not be possible to beat the general market
- also alpha is basically random and can't be predicted

Active investors claim is that alpha can be predicted up to a certain point (it's enough to be right more thann 50% of the times
to beat the market)
we define the R_p as below:
```
                                   n
r_p(t)  == Beta_p * r_m(t)   + SUM   W_i * Alpha_i(t)
                                   i
```


# Stock splits, dividends and survivor bias

**Stock splits**: usually happens when the price of a stock become unreasonably high, so companies essentially just create
a ratio split (e.g 2:1, 3:1, 4:1..)
Stock splits might create errors in algorithms that look for price changes so we look instead at `Adj. Close` which 
adjusts the historical prices to match up and take into account the stock splits

**Dividends**: for each unit of stock, each shareholder receives some payout
This causes the price to jump before the dividend announcement and then drop after the dividend is paid out.
The `Adj Close` price also takes this into account.. so it's important to always use that

**Survivor Bias**: the companies that are part of the SP500 change with time so you might have to be careful about
how it is made and if the composition has chaged in your historical data or you might end up with a bias
Keep that in mind if you want to backtest your strategy around 2007 (.com bubble crash financial crisis).
Some sources of data can give you Survivor Bias Free Data (at a premium)



# Efficient Market Hypothesis

EMH is an investment theory that states it is impossible to beat the market because stock market efficiency causes
existing share prices to always incorporate and reflect all relevant information.
So it should not be possible to perform better than the market expect for taking higher risks investment..

Is this theory true though? Likely not given the success of Hedge funds..
Also market get definitely over valued at some points..
so we will not consider the market 100% efficient.