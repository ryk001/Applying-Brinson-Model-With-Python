# Applying Brinson Model to a portfolio in 2022 with Python!

## Intro
One of my classmate in Fudan told me that he had a great performance in HK stock market in 2022, is it true?
Let's find out with Brinson Model.

Before we started calculation, we have picked Heng Seng Index as our benchmark(cuz our portfolio traded at HK stock market).
We also collected the four types of necessary data mentioned above.

![run](https://i.postimg.cc/44Pm6Jvw/2023-01-09-1-40-18.png)

Now we let our Python 🐍 do the job.

## Calculation

First import packages

```python
import matplotlib.dates as mdates
import matplotlib.pyplot as plt
from google.colab import files, drive
import matplotlib as mpl
import pandas as pd
import numpy as np

try:
  !gdown --id 1fsKERl26TNTFIY25PhReoCujxwJvfyHn
  zhfont = mpl.font_manager.FontProperties(fname='SimHei .ttf') # make mpl show chinese
except:
  pass
```

then we import the data.

```python
benchmark_and_portfolio = pd.read_excel('benchmark_and_portfolio.xlsx')
benchmark_and_portfolio['excess_return'] = (benchmark_and_portfolio.r_portfolio - benchmark_and_portfolio.r_benchmark).round(5)
```

then we can do the calculation, here we use the BHB Model, instead of BF Model.

```python
brinson_model = pd.DataFrame(columns=['date', 'sector', 'allocation_effect', 'selection_effect', 'interaction_effect', 'excess_return'])
brinson_model['date'], brinson_model['sector'] = benchmark_and_portfolio['date'], benchmark_and_portfolio['sector']

#allocation effect (bhb model)
brinson_model['allocation_effect'] = ((benchmark_and_portfolio['w_portfolio']-benchmark_and_portfolio['w_benchmark'])*(benchmark_and_portfolio['r_benchmark']))
brinson_model.loc[brinson_model['sector']=='Total', ['allocation_effect']] = brinson_model[brinson_model['sector']!='Total'].groupby('date').sum(['allocation_effect']).reset_index()['allocation_effect'].to_list()

#selection effect
brinson_model['selection_effect'] = ((benchmark_and_portfolio['w_benchmark'])*(benchmark_and_portfolio['r_portfolio']-benchmark_and_portfolio['r_benchmark']))
brinson_model.loc[brinson_model['sector']=='Total', ['selection_effect']] = brinson_model[brinson_model['sector']!='Total'].groupby('date').sum(['selection_effect']).reset_index()['selection_effect'].to_list()

#interaction effect
brinson_model['interaction_effect'] = ((benchmark_and_portfolio['w_portfolio']-benchmark_and_portfolio['w_benchmark'])*(benchmark_and_portfolio['r_portfolio']-benchmark_and_portfolio['r_benchmark']))
brinson_model.loc[brinson_model['sector']=='Total', ['interaction_effect']] = brinson_model[brinson_model['sector']!='Total'].groupby('date').sum(['interaction_effect']).reset_index()['interaction_effect'].to_list()

# excess return
brinson_model['excess_return'] = brinson_model[['allocation_effect', 'selection_effect', 'interaction_effect']].sum(axis=1)

# round to 5th digit
brinson_model = brinson_model.round(5)

# show
brinson_model
```

Now we get a beautiful Brinson Model DataFrame~

![run](https://i.postimg.cc/Hn3LttfB/2023-01-09-1-55-30.png)

Before we further do the visualization, let's check our calculation with the `excess_return` data. Since the `excess_return` equals to 

`return of portfolio` minus `return of benchmark`,

also equals to 

`allocation_effect` add `selection_effect` add `interaction_effect`


