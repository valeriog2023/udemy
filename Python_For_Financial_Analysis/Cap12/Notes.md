# Qunatopia / zipline

Next section was based on Quantopia that does not exist anymore.
The link to where the old code is supposed to be is definitely something different..  

HOWEVER the original zipline code apparently is here: https://github.com/quantopian/zipline
(last update is from 5 years ago..)

However this platform seems to be still available: https://blueshift.quantinsti.com

This seems to be the platform / organization behind it https://quantra.quantinsti.com and they have some interesting courses

- https://quantra.quantinsti.com/learning-track/algorithmic-trading-beginners

- https://quantra.quantinsti.com/learning-track/advanced-algorithmic-trading-strategies

 etc..  

 ## Start creating a Jupyter notebook or strategy

 - Select : **Research Lab** and **Create Strategy**

 you can create a net notebook or a open a development environment..

Note: very likely you need to import blueshift library modules, e.g:
```
from blueshift.research import list_datasets, use_dataset
from blueshift.research import symbol, current, history
list_datasets()

>>> ['nse', 'forex', 'crypto', 'nse-latest', 'us-equities']

use_dataset('nse')
#
# you define the symbol you want a symbol which is part of the blueshift library
# documented here
# https://blueshift.quantinsti.com/api-docs/api.html#blueshift.core.algorithm.algorithm.TradingAlgorithm.symbol

asset = symbol('ACC')
px = history(asset, ['close'], 100, '1d', dt='2023-06-30')
px.close.plot()
```

However this seems to take forever honestly.. (just to generate the plot above which is part of the initial blueshift jupyter template). However maybe some transient issue.. sencod run was ok..

Note that I tried with random assets like AAPL, TSLA and they are not part of the nse dataset so it failed..
Also was not able to re-run it with the original asset: `ACC` and appaently it as a matter of the time specified
in the dt value that was too old had to update it to this year
It seems NSE is referred to the India Market...

### Building a strategy
This is explained here: https://blueshift.quantinsti.com/docs/quickstart/

This option opens an IDE evnironemtn (you need to select a template, e.g. buy and hold NYSE).
The IDE is already prepopulated with some code to fetch some asstes and run a basic algo that:

- create a function: **rebalance** that calls the library function: `order_target_percent(security, 1.0/10)` for every asset 
  in the context: long_portfolio<p>
  What does this function do??
- the rebalence functino is assigned to a scheduler that runs every month, 2h 30m after the market close
- the context is created with long_portfolio list of symbols and the scehduler


```
from blueshift.api import(    symbol,
                            order_target_percent,
                            schedule_function,
                            date_rules,
                            time_rules,
                       )

def initialize(context):
    """
        A function to define things to do at the start of the strategy
    """
    
    # universe selection
    context.portfolio = [
                               symbol('AMZN'),
                               symbol('AAPL'),
                             ]
    
    # Call rebalance function on the first trading day of each month after 2.5 hours from market open
    schedule_function(rebalance,
                    date_rules.month_start(days_offset=0),
                    time_rules.market_close(hours=2, minutes=30))


def rebalance(context,data):
    """
        A function to rebalance the portfolio, passed on to the call
        of schedule_function above.
    """
    #
    assets_number = len(context.portfolio)
    # 
    for security in context.portfolio:
        order_target_percent(security, 1.0/assets_number)         

```
Now fill in your paramters for backtest.. when you backtest howe er you need to select the dataset but not sure the dataset for these symbols is available..
The job fails.. and no message is returned.. pretty annoying..
So If I remove most of the symbols (except AMAZ and AAPL, the backtest works with the us equities dataset)

In the github reference
https://github.com/quantopian/
there are also other things you can look at (all pretty old tbh)

- pyfolio: pyfolio is a Python library for performance and risk analysis of financial portfolios developed by Quantopian Inc. It works well with the Zipline open source backtesting library

- alphalens: Alphalens is a Python Library for performance analysis of predictive (alpha) stock factors. Alphalens works great with the Zipline open source backtesting library

You can run zipline library offline on your local computer (e.g. docker container)


## functions

### **initialize(context)**: 
called only once and requries context which is a python dictionary (basically)
You can add property using dot notation

### handle_data(context, data):
This function is called once at the end of **each minute** and require context and data as input
- context is a reference to the context defined in initialize
- data, stores several API functions

### order_target_percent(asset, percent)
- It will try to acquire or sell an asset so that it is the specified percentage of the portfolio

### data

- data.current(assetsm columns=['close'])
  return the value of the assets (from the colum) as pandas dataseries

- data.is_stale: this function is not available...  

- data.can_trade: this function is also not available from data but it is a standalone function
  that must be imported. It returns a boolean if the market has access to the symbol..
  This should always be included when trying to run a order on the market, e.g.
  ```
       if data.can_trade(symbol("AMZN")):
        order_target_percent(context.amzn, 1.0)
  ```

