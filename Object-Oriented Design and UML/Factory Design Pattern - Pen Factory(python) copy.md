我们要设计一个工厂，可以生产各种不同颜色的笔。首先定义这几种笔可以画出什么颜色

```python
from abc import ABCMeta, abstractmethod

class Pen(metaclass=ABCMeta):
    def draw(self):
        None

class BluePen(Pen):
    def draw(self):
        print("Drawing a Blue Line")

class RedPen(Pen):
    def draw(self):
        print("Drawing a Red Line")


class YellowPen(Pen):
    def draw(self):
        print("Drawing a Yellow Line")

```


创建一个工厂
```python
class PenFactory:
    def getPen(self, type):
        if(type == None):
            return None

        if(type == 'Blue'):
            return BluePen()
        
        if(type == 'Red'):
            return RedPen()
        
        if(type == 'Yellow'):
            return YellowPen()
        
        return None
```

使用该工厂，并画一条线
```python
pf =  PenFactory()
pen = pf.getPen("Blue")
pen.draw()
```