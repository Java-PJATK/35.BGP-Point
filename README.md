# BGP-Point  
35 BGP-Point/Point.java 65

Cont. from [33.34.BGI-VerySimple](https://github.com/Java-PJATK/33.34.BGI-VerySimple)

Any constructor can invoke — _but only in its first line_ — another constructor of the same class using `this` with arguments, as if it were the name of a method.  

Such a constructor is said to be a _delegating constructor_. We can see an example in the following program:  

## Listing 35 BGP-Point/Point.java  

```java
public class Point {
    private int x, y;

    public Point(int x, int yy) {
        System.out.println("Point(int,int) with " +
                            x + " and " + y);
        this.x =  x;
        y      = yy;
    }

    public Point(int x) {
        this(x,0);
        System.out.println("Point(int) with " + x);
    }

    public Point() {
        this(0);
        System.out.println("Point()");
    }

    public Point translate(int dx,int dy) {
        x += dx;
        y += dy;
        return this;
    }

    public Point scale(int sx,int sy) {
        x *= sx;
        y *= sy;
        return this;
    }

    public int getX() { return x; }
    public int getY() { return y; }

    /**/
    @Override
    public String toString() {
        return "[" + x + "," + y + "]";
    }
    /**/

    public static void main(String[] args) {
        System.out.println("\n*** Creating point p1 (1,2)");
        Point p1 = new Point(1,2);

        System.out.println("\n*** Creating point p2 (1)");
        Point p2 = new Point(1);

        System.out.println("\n*** Creating point p3 ()");
        Point p3 = new Point();

        p3.translate(4,4).scale(2,3).translate(-1,-5);

        System.out.println("\np1: [" + p1.getX() + "," +
                                       p1.getY() + "]");

        System.out.println("\np1: " + p1 + "  p2: " +
                              p2 + "  p3: " + p3);
    }
}
```

After executing another constructor, the control flow returns to the invoking (delegating) constructor. 

It can be seen from the output of the above program

```
*** Creating point p1 (1,2)
Point(int,int) with 1 and 0

*** Creating point p2 (1)
Point(int,int) with 1 and 0
Point(int) with 1

*** Creating point p3 ()
Point(int,int) with 0 and 0
Point(int) with 0
Point()  

p1: [1,2]  
p1: [1,2]   p2: [1,0]   p3: [7,7] 
```

This program illustrates a special role of the `toString` method. It is defined in class `Object`.

As we will learn later (Sec. 10, p. 87), any class inherits (is an extension) of this special class. 

Usually, in our classes we override (redefine) this method — it is then called automatically if an object is provided, but a string describing it is needed (for example, it will be called automatically by function `println`.) 

As it exists in all classes, either because we redefined it or, if not, we inherited it from class `Object`, the method `toString` can be safely invoked on any object. 

The version from class `Object` doesn’t return a very useful string, but, what is important, it does exists and returns a string.  

Next [36.37.BGR-BasicClass](https://github.com/Java-PJATK/36.37.BGR-BasicClass)  



---
Java in a Nutshell  

## Composition Versus Inheritance  

Inheritance is not the only technique at our disposal in object-oriented design. Objects can contain references to other objects, so a larger conceptual unit can be aggregated out of smaller component parts; this is known as _composition_. 

One important related technique is _delegation_, where an object of a particular type holds a reference to a secondary object of a compatible type, and forwards all operations to the secondary object.

This is frequently done using interface types, as shown in this example where we model the employment structure of software companies:  

```java
public interface Employee {
    void work();
}

public class Programmer implements Employee {
    public void work() { /* program computer */ }
}

public class Manager implements Employee {
    private Employee report;
    public Manager(Employee staff) {
      report = staff;
}

public Employee setReport(Employee staff) {
      report = staff;
}

public void work() {
    report.work();
  }
}
```

The Manager class is said to _delegate_ the `work()` operation to their direct report, and no actual work is performed by the `Manager` object. Variations of this pattern involve some work being done in the delegating class, with only some calls being forwarded to the delegate object.   

Another useful, related technique is called the _decorator pattern_. This provides the capability to extend objects with new functionality, including at runtime. The slight overhead is some extra work needed at design time. Let’s look at an example of the decorator pattern as applied to modeling burritos for sale at a taqueria. To keep things simple, we’ve only modeled a single aspect to be decorated—the price of the burrito:  

```java

// The basic interface for our burritos
interface Burrito {
    double getPrice();
}

// Concrete implementation-standard size burrito
public class StandardBurrito implements Burrito {
    private static final double BASE_PRICE = 5.99;

    public double getPrice() {
        return BASE_PRICE;
    }
}

// Larger, super-size burrito
public class SuperBurrito implements Burrito {
    private static final double BASE_PRICE = 6.99;

    public double getPrice() {
        return BASE_PRICE;
    }
}
```

These cover the basic burritos that can be offered—two different sizes, at different prices. Let’s enhance this by adding some optional extras— jalapeño chilies and guacamole. The key design point here is to use an abstract base class that all of the optional decorating components will subclass:  



```java      

    /*
    * This class is the Decorator for Burrito. It represents optional
    * extras that the burrito may or may not have.    
    */

public abstract class BurritoOptionalExtra implements Burrito {
    private final Burrito burrito;
    private final double price;

    protected BurritoOptionalExtra(Burrito toDecorate,
        double myPrice) {
      burrito = toDecorate;
      price = myPrice;
    }

    public final double getPrice() {
    return (burrito.getPrice() + price);
}

}
```

> [!Note]
> Combining an `abstract` base, `BurritoOptionalExtra`, and a protected constructor means that the only valid way to get a `BurritoOptionalExtra` is to construct an instance of one of the subclasses, as they have public constructors (which also hide the setup of the price of the component from client code).  

Let’s test the implementation out:

```java
Burrito lunch = new Jalapeno(new Guacamole(new SuperBurrito()));
// The overall cost of the burrito is the expected $8.09.
System.out.println("Lunch cost: "+ lunch.getPrice());
```

The decorator pattern is very widely used—not least in the JDK utility classes. When we discuss Java I/O in Chapter 10, we will see more examples of decorators in the wild.
