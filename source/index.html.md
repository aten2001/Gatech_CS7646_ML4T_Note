---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - python
  - shell


toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>


includes:
  - errors


search: true
---

# Introduction

Welcome to Gorgia Tech Machine Learning for Trading (ML4T, CS7646) Spring 2018 notes. This static website is used
as a tool for myself to document some important information (especially coding examples) while doing homework and studying for exams.
Official class information can be found [here](http://quantsoftware.gatech.edu/CS7646_Spring_2018).

Author:  Xiaoguang(Jason) Wang  <xgwang@gatech.edu>

## Example Code

>Example code

```python
def function():
    a = 1
```

```shell
>>> def function():
...     a = 1
```

Example code is shown to the right. To make this line at the same vertical level as the "Example code" section
in the right panel, this line has to be right after `>Example code` instead of before. 

# Environment Setup

# HW1: Assess Portfolio
Here is the link to [HW1 official page](http://quantsoftware.gatech.edu/Assess_portfolio).

##Pipeline for Assessing Portfolio Value


> 1.1 get_data() function from util.py

```python
def get_data(symbols, dates, addSPY=True, colname = 'Adj Close'):
    """Read stock data (adjusted close) for given symbols from CSV files."""
    df = pd.DataFrame(index=dates)
    if addSPY and 'SPY' not in symbols:  # add SPY for reference, if absent
        symbols = ['SPY'] + symbols

    for symbol in symbols:
        df_temp = pd.read_csv(symbol_to_path(symbol), index_col='Date',
                parse_dates=True, usecols=['Date', colname], na_values=['nan'])
        df_temp = df_temp.rename(columns={colname: symbol})
        df = df.join(df_temp)
        if symbol == 'SPY':  # drop dates SPY did not trade
            df = df.dropna(subset=["SPY"])
```
> 1.2 Example for preparing ```symbols``` and `dates` variables

```python
import pandas as pd
import datetime as dt

start_date = dt.datetime(2008,1,1)
end_date = dt.datetime(2009,1,1)
dates = pd.date_range(start_date, end_date)

symbols = ['GOOG','AAPL','GLD','XOM']
```
> 1.3 Get `prices` by calling `get_data()`

```python
from util import get_data

prices_all = get_data(symbols, dates)  # automatically adds SPY
prices = prices_all[symbols]  # only portfolio symbols
prices_SPY = prices_all['SPY']  # only SPY, for comparison later
```
> 2.1 Calculate `daily_por_values`

```python
# normalize price with prices on start date
prices_norm = prices / prices.ix[0, :] 

# initialize allocation by evenly distributing to portfolio stocks
l = len(symbols)
alloc_init = np.empty(l)
alloc_init.fill(1.0/l)  

# apply initial allocation
prices_alloced = prices_norm * alloc_init  
daily_por_values = prices_alloced.sum(axis = 1)
```

1. Read in individual prices data
    
    Prices data `prices` are read in as pandas dataframe using the `get_data()` function from util.py. 
    To call the `get_data()` function, two input variables `symbols` and `dates` are needed.
    
    Input Variable | Description
    -------------- | -----------
    symbols | List of symbol names as string. eg `['APPL', 'GOOL', 'ZTS']` 
    dates | DatetimeIndex generated with `pandas.date_range()` method. See example code 1.2


2. Calculate daily portfolio values

    See example code 2.1 for calculating daily portfolio values `daily_por_values`.

# HW2: Optimize Something
Here is the link to [HW2 official page](http://quantsoftware.gatech.edu/Optimize_something).


<!---
# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>
--->

<!---
# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

--->

