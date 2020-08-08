# **SOLID**

SOLID goes by the term:

**[S]** ingle Responsibilty Principle <br />
**[O]** pen Closed Principle <br />
**[L]** iskov Substitution Principle <br />
**[I]** nterface Segregation principle <br />
**[D]** pendancy Inversion Principle <br />
<hr>

## Single Responsibilty Principle: 
*“A Class must have only one reason to change.”*
* SRP is all about having only one reason to change, whether it is a package or component or application or class, they should all have one responsibility.
* The application or class should be able to define in one line abiut the functionality of it.

The below code snippet will illustrate the example of a class with many responsibilities
  
```java
public class Task{
    public void DownloadFile(String location)...
    public void ParseFile(File file)...
    public void PersistData(Data data)
}
```
<hr >

## Open Closed Principle: 
*“Software entities (classes, modules, functions, etc.) must be opened for an extension, but closed for modification.”*

To illustrate the understanding, let's observe the *Area* class.
```java
public double Area(object[] shapes){
    double area = 0
    foreach (var shape in shapes){
        if(shape is rectangle){
            rectangle rectangle = (Rectangle)shape;
            area += rectangle.Width*rectangle.Height;
        }else{
            Circle circle = (Circle)shape;
            area += circle.Radius * circle.Radius *math.PI;
        }
    }
    return area;
}
```

This class calculates the area of the shape by passing in the array of shapes. Then it is checking if the shape is either Rectangle or Circle. What if few more shapes has to be added into this?, then all the logic would be centralised in the *foreach loop*. Therefore the better  solution would be to allow the shapes to define their own *Area* method.
```java
public abstract class Shape{
    public abstract double Area();
} 

public class Rectangle : Shape{
    public double Width{get;set}
    public double Heigth{get;set}

    public override double Area(){
        return Width*Height;
    }
}

pubblic class Circle : Shape{
    public double Radius{get;set}

    public override double Area(){
        return Radius*Radius*Math.PI;
    }
}

public double Area(Shape[] shapes){
    double area = 0;
    foreach (var shape in shapes){
        area == shape.Area();
    }
    return area;
}
```
The *Area* function will basically call on each shape from the array of *Shapes* and it's following *Area* function.