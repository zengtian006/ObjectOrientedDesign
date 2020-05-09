# Design Amazon - Online Shopping System

- 系统需求和目标

1. 用户可以添加新产品去卖
2. 用户可以用产品名或者类别搜索
3. 用户可以浏览多有产品，但是必须注册成为会员才可以购买
4. 用户可以添加，删除购物车里的商品
5. 用户可以check out购物车里的商品
6. 用户可以给产品评价
7. 用户可以指定送货地址
8. 如果没有运送，用户可以取消订单
9. 如果运送状态发生该表，用户应该要得到通知
10. 用户可以用信用卡或者电子转账付费
11. 用户可以追踪订单运送状态

- Use cases

1. 添加/更新产品，更新catalog
2. 搜索产品by name， category
3. 添加删除购物车的商品
4. Check out 
5. 付钱
6. 添加新的产品类别
7. 如运送状态有改变。发送通知给用户

- Class

1. Account: Admin/Member
2. Guest
3. Catalog
4. ProductCategory
5. Product
6. ProductReview
7. ShoppiingCart
8. Item: 封装Product。例如一支笔可以是Product，如果库存中有10支笔，那每一支都是一个Item。Product是一种产品的抽象，包含商品的属性，特征。Item是一件商品，除Product的属性外，还有卖家，库存，描述等等销售属性
9. Order
10. OrderLog
11. ShipmentLog
12. Notification
13. Payment

- Sequence diagram

- Activity diagram

- Code

1. 数据类型和常量

```python
class Address:
    def __init__(self, street, city, state, zip_code, country):
        self.__street = street #  私有变量
        self.__city = city
        self.__state = state
        self.__zip_code = zip_code
        self.__country = country

class OrderStatus(Enum):
    UNSHIPPED, PENDING, SHIPPED, COMPLETED, CANCELED, REFUND_APPLIED = 1, 2, 3, 4, 5, 6

class AccountStatus(Enum):
    ACTIVE, BLOCKED, BANNED, COMPROMISED, ARCHIVED, UNKNOWN = 1, 2, 3, 4, 5, 6

class ShipmentStatus(Enum):
    PENDING, SHIPPED, DELIVERED, ON_HOLD = 1, 2, 3, 4

class PaymentStatus(Enum):
    UNPAID, PENDING, COMPLETED, FILLED, DECLINED, CANCELLED, ABANDONED, SETTLING, SETTLED, REFUNDED = 1, 2, 3, 4, 5, 6, 7, 8, 9, 10

```

2. 用户类：Account, Customer(Admin, Member and Guest - 继承Customer)

```python
class Account:
  def __init__(self, user_name, password, name, email, phone, shipping_address, status=AccountStatus):
    self.__user_name = user_name
    self.__password = password
    self.__name = name
    self.__email = email
    self.__phone = phone
    self.__shipping_address = shipping_address
    self.__status = status.ACTIVE
    self.__credit_cards = []
    self.__bank_accounts = []

    def add_product(self, product):
        None

    def add_productReview(self, review):
        None

    def reset_password(self):
        None

from abc import ABCMeta, abstractmethod
class Customer(metaclass=ABCMeta): #抽象类
    def __init__(self, cart, order):
        self.__cart = cart
        self.__order = order
    
    def get_shopping_cart(self):
        return self.__cart
    
    def add_item_to_cart(self, item):
        None
        
    def remove_item_from_cart(self, item):
        None

class Guest(Customer):
    def register_account(self):
        None

class Member(Customer): #继承
    def __init__(self, account):
        self.__account = account
    
    def place_order(self, order):
        None
    
    def add_product(self, product):
        None

class Admin(Customer): #
    def __init__(self, account):
        self.__account = account
    
    def block_member(self, member):
        None

    def add_productCatogory(self, productCategory):
        None

```

3. 与产品有关的类：ProductCatogory, Prodcut and ProductReview

```python
class ProductCatogory:
    def __init__(self, name, desc):
        self.__name = name
        self.__desc = desc

class ProductReview:
    def __init__(self, rating, review, reviewer):
        self.__rating = rating
        self.__review = review
        self.__reviewer = reviewer

class Product: 
    def __init__(self, id, name, description, price, category, seller_account):
        self.__product_id = id
        self.__name = name
        self.__description = description
        self.__price = price
        self.__category = category
        self.__available_item_count = 0

        self.__seller = seller_account    
    
    def get_available_count(self):
        return self.__available_item_count

    def update_price(self, new_price):
        self.__price = new_price
```

4. 与下订单相关的：ShoppingCart, Order and OrderLog. 用户将Item加入购物车然后下订单

```python
class Item:
    def __init__(self, id, quantity, price):
        self.__product_id = id
        self.__quantity = quantity
        self.__price = price

    def update_quantity(self, quantity):
        None

class ShoppingCart:
    def __init__(self):
        self.__items = []

    def add_item(self, item):
        self.__items.append(item)

    def remove_item(self, item):
        None

    def update_item_quantitiy(self, item, quantitiy):
        self.__items.get(item).update_quantity(quantitiy)
    
    def get_items(self):
        return self.__items

    def checkout(self):
        None
        
class OrderLog:
    def __init__(self, order_number, status=OrderStatus.PENDING):
        self.__order_number = order_number
        self.__creation_date = datetime.date.today()
        self.__status = status


class Order:
    def __init__(self, order_number, status=OrderStatus.PENDING):
        self.__order_number = 0
        self.__status = status
        self.__order_date = datetime.date.today()
        self.__order_log = []

    def send_for_shipment(self):
        None

    def make_payment(self, payment):
        None

    def add_order_log(self, order_log):
        None
```

5. 成功下订单后，将生成运送记录； Shipment, ShipmentLog and Notification

```python
class ShipmentLog:
    def __init__(self, shipment_number, status=ShipmentStatus.PENDING):
        self.__shipment_number = shipment_number
        self.__status = status
        self.__creation_date = datetime.date.today()

class Shipment:
    def __init__(self, shipment_numbe, shipment_methodr):
        self.__shipment_number = shipment_number
        self.__shipment_date = datetime.date.today()
        self.__estimated_arrival = datetime.date.today()
        self.__shipment_method = shipment_method
        self.__shipmentLogs = []

    def add_shipment_log(self, shipment_log):
        None

from abc import ABCMeta, abstractmethod
class Notification(metaclass=ABCMeta):
  def __init__(self, id, content):
    self.__notification_id = id
    self.__created_on = datetime.date.today()
    self.__content = content

  def send_notification(self, account):
    None
```

6. 搜索接口: 用Catalog 去时间搜索功能

```python

from abc import ABCMeta, abstractmethod
class Search(metaclass=ABCMeta):
    def search_products_by_name(self, name):
        None

    def search_products_by_category(self, category):
        None

# This class will keep an index of all products for faster search. 每次添加新product 就会更新Catalog
class CataLog(Search):
    def __init__(self):
        self.__product_names = {}
        self.__product_categories = {}
        
    def search_products_by_name(self, name):
        return self.__product_names.get(name)

    def search_products_by_category(self, category):
        return self.__product_categories.get(catogory)


cataLog = CataLog()
catalog.search_products_by_name("Sports")


```

