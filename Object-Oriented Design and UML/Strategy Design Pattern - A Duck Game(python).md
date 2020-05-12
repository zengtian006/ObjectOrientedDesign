鸭子可以有各种种类，但是鸭子都会游泳，都有显示外观的方法。所以将样貌和外观方法抽象出来

```python
from abc import ABCMeta, abstractmethod

class Duck(metaclass=ABCMeta):
    def display(self):
        None

    def swim(self):
        None

```

鸭子的叫声不同；有些鸭子会飞，有些不会。将叫声，飞行特性抽象成接口，并将实现不同的特性
```python
from abc import ABCMeta, abstractmethod

class FlyBehavior(metaclass=ABCMeta):
    def fly(self):
        None


class QuackBehavior(metaclass=ABCMeta):
    def quack(self):
        None


class FlyWithWings(FlyBehavior):

	def fly(self):
		print("我会飞啦~！")



class FlyNoWay(FlyBehavior):

	def fly(self):
		None  #什么都不做，它不会飞


class Quack(QuackBehavior):
   def quack(self):
	   print("呷呷!")

class Squeak(QuackBehavior):
   def quack(self):
	   print("吱吱！"); #橡皮鸭

class MuteQuack(QuackBehavior):
   def quack(self):
	   print(".......") #"诱饵鸭"不会叫

```

将接口放入鸭子抽象类中

```python
class Duck(metaclass=ABCMeta):
    def __init__(self,flyBehavior, quackbehavior):
        self.flyBehavior = flyBehavior
        self.quackBehavior = quackbehavior

    def performQuack(self):
      self.quackBehavior.quack()

    def Display(self):
        None

    def swim(self):
        print("~~游~~")

```

实现一种鸭子 并测试
```python

m = Duck(FlyWithWings(),Quack())
m.Display()
m.swim()
m.performFly()
m.performQuack()

```