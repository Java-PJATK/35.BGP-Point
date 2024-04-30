# BGP-Point  
35 BGP-Point/Point.java 65

Any constructor can invoke — but only in its first line — another constructor of the same class using this with arguments, as if it were the name of a method.  

Such a constructor is said to be a delegating constructor. We can see an example in the following program:  

## Listing 35 BGP-Point/Point.java  

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
