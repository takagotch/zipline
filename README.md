### zipline
---
https://github.com/quantopian/zipline

```py
from zipline.api import order_target, record, symbol

def initialize(context):
  context.i = 0
  context.asset = symbol('AAPL')

def handle_data(context, data):
  context.i += 1
  if context.i < 300:
    return
  
  short_mavg = data.history(context.asset, 'price', bar_count=100, frequency="1d").mean()
  long_mavg = data.history(context.asset, 'price', bar_count=300, frequency).mean()

  if short_mavg > long_mavg:
    order_target(context.asset, 0)
  elif short_mavg < long_mavg:
    order_target(context.asset, 0)
    
  record(AAPL=data.current(context.asset, 'price'),
    short_mavg=short_mavg,
    long_mavg=long_mavg)


class TFSExchangeCelendar(TradingCalendar):
  """
  """
  
  @property
  def name(self):
    """
    """
    return "TFS"
    
  @property
  def tz(self):
    """
    """
    return timezone("UTC")
    
  @property
  def open_time(self):
    """
    """
    return time(0, 0)
    
  @property
  def close_time(self):
    """
    """
    return time(23, 59)

  @lazyval
  def day(self):
    """
    """
    weekmask = "Mon Tue Wed Thu Fri Sat Sun"
    return CustomBusinessDay(
      weekmask=weekmask
    )

from panda.tseries.holiday import (
  Holiday,
  DateOffset,
  MO
)

SomeSpecialDay = Holiday(
  "Some Special Day",
  month=1,
  day=9,
  offset=DateOffSet(weekday=MO(-1))
)


class LSEExchangeCalendar(TradingCalendar):
  """
  """
  @property
  def name(self):
    return "LSE"
  
  @property
    return timezone('Europe/London')
    
  @property
  def open_time(self):
    return time(8, 1)
    
  @property
  def close_time(self):
    return time(16, 30)
    
  @property
  def regular_holidays(self):
    return HolidayCalendar([
      LSENewYearsDay,
      goodFriday,
      EasterMonday,
      MayBank,
      SpringBank,
      SummerBank,
      Christmas,
      WeekendChistmas,
      BoxingDay,
      WeekendBoxingDay
    ])
```

```
pip install zipline

QUAND_API_KEY=<yourkey> zipline ingest -b quandl
zipline run -f dual_moving_average.py --start 2014-1-1 --end 2018-1-1 -o dma.pickle
```

```
```


