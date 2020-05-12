我们要设计一个工厂，可以生产各种不同颜色的笔。首先定义这几种笔可以画出什么颜色

```java
public interface Pen{
    void draw();
}

public class BluePen implements Pen{
    @Override
    public void draw(){
        System.out.println("Drawing a Blue Line");
    }
}

public class RedPen implements Pen{
    @Override
    public void draw(){
        System.out.println("Drawing a Red Line");
    }
}

public class YellowPen implements Pen{
    @Override
    public void draw(){
        System.out.println("Drawing a Yellow Line");
    }
}
```


创建一个工厂
```java
public class PenFactory{
    public Pen getPen(String type){
        if(type == null){
            return null;
        }
        if(type == 'Blue'){
            return new BluePen()
        }
        if(type == 'Red'){
            return new RedPen()
        }
        if(type == 'Yellow'){
            return new YellowPen()
        }
        return null
    }
}

```

使用该工厂，并画一条线
```java
PenFactory pf =  new PenFactory();
Pen pen = pf.getPen("Blue");
pen.draw();
```