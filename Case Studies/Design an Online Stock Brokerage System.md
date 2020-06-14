# Design an Online Stock Brokerage System


## 系统需求
1. 任何用户可以买卖股票
2. 任何用户有不同的watchlists收藏不同的股票
3. 用户下不同类型的股票订单： 1,Market: 跟市场价格; 2, Limit: 指定价格; 3, Stop-loss: 止损单(到一定价格卖出); 4, Stop-limit

        REF: http://greenhornfinancefootnote.blogspot.com/2008/04/market-orders-limit-orders-and-stop.html

4. 用户可以有不同的股票的仓
5. 系统可以生成报表给用户
6. 用户可以存取现金
7. 一旦交易成功，系统可以发通知给用户

## Usecase diagram

### Actors
1. 管理员： 管理会员，审核，封和解封会员帐号
2. 会员： 搜索股票，买卖股票，有自己的watchlists
3. 系统： 发送通知，获取股票报价

### Top use cases
1. 用户注册和取消
2. 添加删除编辑 Watchlists
3. 搜索股票by Symbols
4. 下订单
5. 取消订单
6. 存取现金

## Class

- Account: Membre, Admin

```python
from abc import ABC, abstractmethod

class Account(ABC):
  def __init__(self, id, password, name, address, email, phone, status=AccountStatus.NONE):
    self.__id = id
    self.__password = password
    self.__name = name
    self.__address = address
    self.__email = email
    self.__phone = phone
    self.__status = AccountStatus.NONE

  def reset_password(self):
    None
```

Member类 继承Accout类
```python
class Member(Account):
  def __init__(self):
    self.__available_funds_for_trading = 0.0
    self.__date_of_membership = datetime.date.today()
    self.__stock_positions = {}
    self.__active_orders = {}

def place_sell_limit_order(self, stock_id, quantity, limit_price, enforcement_type):
    # check
    pass
    # 生成order
    order = LimitedOrder(stock_id, quantity, limit_price, enforcement_type)
    order.is_buy_order = False
    order.save_in_DB()
    success = StockExchange.place_order(order)
    if sucess:
        #update order status
    else:
        # add to active_orders
        self.active_orders.add(order.get_order_id(), order)
    return success

def place_buy_limit_order():
    # check
    pass
    # 生成order
    order = LimitedOrder(stock_id, quantity, limit_price, enforcement_type)
    order.is_buy_order = True
    order.save_in_DB()
    success = StockExchange.place_order(order)
    if sucess:
        #update order status
    else:
        # add to active_orders
        self.active_orders.add(order.get_order_id(), order)
    return success

def callback_stock_exchange(self, order_id, order_parts, status):
    # this function will be invoked whenever there is an update from stock exchange against an order
    order = self.active_orders[order_id]
    order.add_order_parts(order_parts)
    order.set_status(status)
    order.update_in_DB()

    if status == OrderStatus.FILLED or status == OrderStatus.CANCELLEd:
      self.active_orders.remove(order_id)

```


- Order

```python
from abc import ABC, abstractmethod
import datetime

class Order(ABC):
  def __init__(self, id):
    self.__order_id = id
    self.__is_buy_order = False
    self.__status = OrderStatus.OPEN
    self.__time_enforcement = TimeEnforcementType.ON_THE_OPEN
    self.__creation_time = datetime.datetime.now()

    self.__parts = {}

  def set_status(self, status):
    self.status = status

  def save_in_DB(self):
  # save in the database

  def add_order_parts(self, parts):
    for part in parts:
      self.parts[part.get_id()] = part

```

不同order类型， 继承Order类
```python

class LimitOrder(Order):
  def __init__(self):
    self.__price_limit = 0.0

class MarketOrder(Order):
  def __init__(self):
    pass

class StopLossOrder(Order):
  def __init__(self):
    self.__price_limit = 0.0

class StopLimitOrder(Order):
  def __init__(self):
    self.__price_limit = 0.0
```

- StockExchange 股票交易市场，Singleton class

```python
class StockExchange:
  # singleton, used for restricting to create only one instance
  instance = None

  class __OnlyOne:
    def __init__(self):
      None

  def __init__(self):
    if not StockExchange.instance:
      StockExchange.instance = StockExchange.__OnlyOne()

  def place_order(self, order):
    return_status = self.get_instance().submit_order(Order)
    return return_status
```