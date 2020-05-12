鸭子可以有各种种类，但是鸭子都会游泳，都有显示外观的方法。所以将样貌和外观方法抽象出来

```java
public abstract class Duck{
    public void display(){

    }

    public void swim(){

    }
}
```

鸭子的叫声不同；有些鸭子会飞，有些不会。将叫声，飞行特性抽象成接口，并将实现不同的特性
```java
public interface FlyBehavior{
    void fly();
}

public interface QuackBehavior{
    void quack();
}

public class FlyWithWings implements FlyBehavior
{
	@Override
	public void Fly() {
		System.out.println("我会飞啦~！");
	}
}

public class FlyNoWay implements FlyBehavior
{
	@Override
    public void Fly() {
       //什么都不做，它不会飞
    }
}

class Quack implements QuackBehavior
{
   public void quack()
    {
	   System.out.println("呷呷!");
    }
}

class Squeak implements QuackBehavior
{
   public void quack() {
	   System.out.println("吱吱！");//橡皮鸭
    }
}

class MuteQuack implements QuackBehavior
{
   public void quack()
    {
	   System.out.println(".......");//"诱饵鸭"不会叫
    }
}
```

将接口放入鸭子抽象类中

```java
public abstract class Duck{
    public FlyBehavior flyBehavior;
    public  QuackBehavior quackbehavior;

    public void performQuack() {
        quackbehavior.quack();
    }
    public void performFly()
    {
        flybehavior.Fly();
    }
    
    public void display(){

    }

    public void swim(){
        System.out.println("~~游~~");
    }
}
```

实现一种鸭子
```java
public class MallarDuck extends Duck{
    public MallarDuck(){
        flyBehavior = new FlyWithWings();
        quackbehavior = new Quack()
    }
    @override
    public void performFly() {
        flybehavior.Fly();
    }

    @Override
	public void Display(){
		System.out.println("我是一只美丽的绿头鸭！");
	}
}
```


测试
```java
public class Test {  
    public static void main(String[] args) {  
    	Duck mallard = new MallarDuck();
        mallard.Display();
        mallard.Swim();
        mallard.performQuack();
        mallard.performFly();
    }  
} 
```