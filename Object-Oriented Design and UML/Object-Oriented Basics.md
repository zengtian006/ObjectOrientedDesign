# Object-Oriented Basics

## 基础概念

- 对象(Object):

    代表了现实世界的一个实体，例如网上购物系统里，购物车，消费者，产品就是一个对象

- 类(Class):

    是对象的原型，是对象的属性和方法的模版定义，例如消费者对象，有送货地址，信用卡等属性，以及下订单，取消订单的方法等等

-  封装(Encapsulation)：

    仅保留有限的外部接口，隐藏具体细节，状态。用户只能通过外部接口访问该状态或进行所需操作

    ```java
        class Woman
        {
            private weight

            public Woman(int w){
                this.weight = w
            }

            public int getWeight(){
                return this.weight
            }

            public void growWeight(int w){
                this.weight = this.weight+w
            }
        }
    ```

    ```python
       class Woman:
            __weight = 0
            
            def __init__(self, w):
                self.__weight = w

            def getWeight():
                return self.__weight
            
            def growWeight(w):
                self.weight+=w
    ```
- 抽象(Abstraction)
    
    隐藏除对象以外的所有数据，以降低系统复杂度

    抽象类：只可以继承(extends)，是对事务的抽象，可以提供成员方法的实现细节
    
    接口： 只可以实现(implements), 是对方法/行为的抽象，不提供具体细节 (Python 没有接口类)

    继承是一个“是不是”的关系，实现是“有没有”的关系
    

    ```java

    public interface IHuman{
        
        void eat();
        void drink();
        String getName();
        Void setName(String name);
        
    }

    public abstract class Human implements IHuman{
        protected name

        @override
        public void setName(String name){
            this.name = name
        }

        @override
        public String getName(){
            return this.name
        }

        public int talk(){
            print("this is "+this.name)
        }
    }

    public class Man extends Human{
        @override
        public eat(){
            <!-- eat sth -->
        }

        @override
        public drink(){
            <!-- drink sth -->
        }
    }
    ```

    ```python
    from abc import ABCMeta, abstractmethod

    class IHuman(metaclass=ABSMeta):
    
        def eat(self):
            None
            
        def drink(self):
            None
        
        def setName(self, name):
            None
        
        def getName(self):
            None

    class Human(IHuman):
        name = ""

        def setName(self,name):
            self.name = name
        
        def getName(self):
            return self.name

    class Man(Human):
        def drink(self):
            <!-- Drink Sth -->
        
        def eat(self):
            <!-- Eat Sth -->
    ```

- 继承(Inheritance)
    
    在现有的类中创建新的类

    子类继承父类所有的public 和 protected的成员变量和方法
    
    不继承Private 和 父类的构造函数

    如果成员变量或者方法重名，则子类覆盖父类

    ```java

    class Human{
        public String name = 'Human'

        public void eat(){
            <!-- eat -->
        }
    }

    class Man extends Human{
        // 成员变量name覆盖了父类的name
        public String name = "Man"

        //Man 继承了 eat 方法

    }

    ```

    ```python

    class Human:    
        name = "Human

        def eat(self):
            <!-- eat -->
    
    class Man(Human):
        name = 'Man'
    ```

- 多态(Polymorphism)

    对象根据上下文以不同的方式响应同一方法，例如象棋里，棋子可以是多样的，车，马，炮。。这些对象对于 “移动”这一个方法有不同的响应

    当使用多态方式调用方法时，首先检查父类中是否有该方法，如果没有，则编译错误；如果有，再去调用子类的同名方法。

    Parent p = new Child()

    ```java

    abstract class Animal {
        abstrack void eat()
    }

    class Cat extends Animal {
        public void eat(){
            System.out.println("吃鱼");
        }
    }

    class Dog extends Animal {
        public void eat(){
            System.out.println("吃骨头");
        }
    }


    Animal a = new Cat()
    Animal b = new Dog()
    a.eat()
    b.eat()

    ```

    ```python

    class Animal:
        def eat(self):
            pass
    
    class Cat(Animal):
        
        def eat(self):
            print("吃鱼")

    class Dog(Animal):
        
        def eat(self):
            print("吃骨头")

    def eat_what(animal):
        animal.eat()

    eat_what(Cat())
    eat_what(Dog())

    ```