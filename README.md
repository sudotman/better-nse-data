# Documentation

https://marketsetup.in/documentation/jugaad-data/

# Introduction

`jugaad-data` is a python library to download historical/live stock, index as well as economic data from NSE and RBI website using.

![Build Badge](https://github.com/jugaad-py/jugaad-data/actions/workflows/run-tests.yml/badge.svg)


# Features

* Supports [new NSE website](https://www.nseindia.com/), (All libraries based on old NSE website might stop working)
* Powerful CLI (Command line interface), Even non-coders can use it easily
* Built-in caching mechanism to play nice with NSE. Avoid making un-necessary requests to NSE's website and getting blocked
* Optional `pandas` support 

**Road map**

| Website  | Segment    | Supported? |
|----------|------------|------------|
| NSE      | Stocks     | Yes        |
| NSE      | Stocks F&O | Yes        |
| NSE      | Index      | Yes    |
| NSE      | Index F&O  | Yes        |
| RBI	   | Current Rates| Yes |

# Installation

`pip install jugaad-data` 

# Getting started

## Python inteface

### Historical data

```python
from datetime import date
from jugaad_data.nse import bhavcopy_save, bhavcopy_fo_save

# Download bhavcopy
bhavcopy_save(date(2020,1,1), "/path/to/directory")

# Download bhavcopy for futures and options
bhavcopy_fo_save(date(2020,1,1), "/path/to/directory")

# Download stock data to pandas dataframe
from jugaad_data.nse import stock_df
df = stock_df(symbol="SBIN", from_date=date(2020,1,1),
            to_date=date(2020,1,30), series="EQ")
```

### Historical data (In Loop)
If trying to download files in a loop, It is important to delete the object explicitly otherwise download does not work for next date when running in loop. Library creates session at object initialization time which gets reset when object is deleted and thus NSE server do not treat next call as bot call.

```python
from datetime import date
from jugaad_data.nse import full_bhavcopy_save
from time import sleep

for m_date in [date(2021,1,1), date(2021,1,2), date(2021,1,3)]:
  # Create new object for each call
  full_bhavcopy_save = jugaad_data_nse.full_bhavcopy_save

  # Download full bhavdata
  full_bhavcopy_save(m_date, "/path/to/directory")

  # Very important else second download does not happen. Clear session and create new one
  del full_bhavcopy_save

  # Sleep to avoid getting blocked on NSE server
  sleep(5)
```
Note: Full bhavdata download is not available prior to October 2019.


### Live data

```python
from jugaad_data.nse import NSELive
n = NSELive()
q = n.stock_quote("HDFC")
print(q['priceInfo'])
```

```
{'lastPrice': 2635,
 'change': -49.05000000000018,
 'pChange': -1.8274622305843848,
 'previousClose': 2684.05,
 'open': 2661,
 'close': 2632.75,
 'vwap': 2645.57,
 'lowerCP': '2415.65',
 'upperCP': '2952.45',
 'pPriceBand': 'No Band',
 'basePrice': 2684.05,
 'intraDayHighLow': {'min': 2615.6, 'max': 2688.45, 'value': 2635},
 'weekHighLow': {'min': 1473.45,
  'minDate': '24-Mar-2020',
  'max': 2777.15,
  'maxDate': '13-Jan-2021',
  'value': 2635}}
```

## Command line interface

```
$ jdata stock --help

Usage: jdata stock [OPTIONS]

  Download historical stock data

  $jdata stock --symbol STOCK1 -f yyyy-mm-dd -t yyyy-mm-dd --o file_name.csv

Options:
  -s, --symbol TEXT  [required]
  -f, --from TEXT    [required]
  -t, --to TEXT      [required]
  -S, --series TEXT  [default: EQ]
  -o, --output TEXT
  --help             Show this message and exit.
```

```
$ jdata stock -s SBIN -f 2020-01-01 -t 2020-01-31 -o SBIN-Jan.csv
SBIN  [####################################]  100%

Saved file to : SBIN-Jan.csv
```

## Download historical derivatives (F&O) data

```
$ jdata deriviatives --help
Usage: cli.py derivatives [OPTIONS]

  Sample usage-

  Download stock futures-

  jdata derivatives -s SBIN -f 2020-01-01 -t 2020-01-30 -e 2020-01-30 -i FUTSTK -o file_name.csv

  Download index futures-

  jdata derivatives -s NIFTY -f 2020-01-01 -t 2020-01-30 -e 2020-01-30 -i FUTIDX -o file_name.csv

  Download stock options-

  jdata derivatives -s SBIN -f 2020-01-01 -t 2020-01-30 -e 2020-01-30 -i OPTSTK -p 330 --ce -o file_name.csv

  Download index options-

  jdata derivatives -s NIFTY -f 2020-01-01 -t 2020-01-30 -e 2020-01-23 -i OPTIDX -p 11000 --pe -o file_name.csv

Options:
  -s, --symbol TEXT  Stock/Index symbol  [required]
  -f, --from TEXT    From date - yyyy-mm-dd  [required]
  -t, --to TEXT      To date - yyyy-mm-dd  [required]
  -e, --expiry TEXT  Expiry date - yyyy-mm-dd  [required]
  -i, --instru TEXT  FUTSTK - Stock futures, FUTIDX - Index Futures, OPTSTK -
                     Stock Options, OPTIDX - Index Options  [required]

  -p, --price TEXT   Strike price (Only for OPTSTK and OPTIDX)
  --ce / --pe        --ce for call and --pe for put (Only for OPTSTK and
                     OPTIDX)

  -o, --output TEXT  Full path of output file
  --help             Show this message and exit.
```

## Buy me a coffee

If my work has helped you in anyway, you can buy me a coffee 

[!["Buy Me A Coffee"](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.buymeacoffee.com/Jugaader)
