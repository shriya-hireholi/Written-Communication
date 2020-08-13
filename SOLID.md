# **SOLID**

SOLID is an acronym for:

**[S]** ingle Responsibilty Principle <br />
**[O]** pen Closed Principle <br />
**[L]** iskov Substitution Principle <br />
**[I]** nterface Segregation Principle <br />
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

public class Circle : Shape{
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
<hr >

## Liskov Subustitution Principle:
*“Subtypes should be replaceable by their base types.”*

To illustrate the understanding, let's observe the following code snippet.
```java
class rectangle{
    void setWidth(double w)
    void setHeight(double h)
    double setWidth()
    double setHeight()
}
class square{
    void setWidth(double w) //set both width and heigth to w
    void setHeight(double h) //set both width and heigth to h
    double setWidth()
    double setHeight()
}
```
Suppose the *sqaure* class tries to extend the *rectangle* class, and in the *setWidth* method it will set both the width and height to w and in the *setHeight* method will set both height and width to h.

Consider the following code snippet:
```java
void test(rectangle r){
    r.setWidth(5)
    r.setHeight(4)
    assertEquals(5*4,r.setWidth*r.setHeight)
}
```
If we consider to test the *rectangle* class and set width and height to 5 and 4 respectively. The *rectangle* class will pass this test, but will the *square* class pass the test?. NO, becaue the *setHeight* will set both height and width to the same value as h.

**You should use inheritance only when the super class is replaceable by the subclass in all the instances.**
<hr >

## Interface Segregation Principle :
*“Many specific interfaces are better than a general interface.”*

The dependency of one class to another one should depend on the *smallest* possible interface.
* Clients should not be forced to implement interfaces they don't use.
* Instead of one fat interface many small interfaces are preffered based on groups of methods, each one serving one submodule.

The below code snippet will illustrate the example of Liskov substitution.
```java
Animal
    void feed(); //abstract

Dog implements Animal
    void feed() //implementation

Tiger implements Animal
    void feed() //implementation
}
```
The *Dog* and *Tiger* class both implement the feed method from the *Animal* class.

Suppose another method is beign added to the *Animal* class.
```java
Animal
    void feed(); //abstract
    void groom(); //abstract

Dog implements Animal
    void feed(); //implementation
    void groom(); //implementation

Tiger implements Animal
    void feed(); //implementation
    void groom(); //DUMMY implementation
}
```
Over here it can be seen that the *groom* method is only applicable to the domestic animals, but for *Tiger* it's just a dummy implementation.

The ideal way to handle it is, to add a new interface extending the exsisting iterface. Istead of adding the new features to the exsisting interface it is better to add the features to the new interface.

The following code snippet shows how to handle the proble.
```java
Animal
    void feed(); //abstract

Pet extends Animal
    void groom(); //abstract

Dog implements Pet
    void feed(); //implementation
    void groom(); //implementation

Tiger implements Animal
    void feed(); //implementation
}
```
<hr>

## Dpendancy Inversion Principle :
*“Depend on abstractions and not on concrete classes.”*

To illustrate the understanding, let's observe the following code snippet.
```java
enum OutputDevide{printer, disk};
void copy(OutputDevide dev){
    int c;
    while((c = ReadKeyboard()) != EOF ){
        if(dev == printer)
            writePrinter(c)
        else
            writeDesk(c)
    }
}
```
The issue with the aove code snippet is that, when the number of the *OutputDevide* increases the *copy* method needs to keep on changing.

The better way to solve it is to create a simple interface like *Reader* and an interface *Writer*. The copy method, can  write from any *Reader* to any *Writer*. So the *copy* method does not change when there is a new writer or new reader.
```java
interface Reader
    char read();

interface Writer
    char write(char ch);

void copy(Reader r, Writer w){
    char ch;
    while((ch = r.read()) != EOF){
        w.write(ch);
    }
}
```


Refrences:<br >
https://www.youtube.com/watch?v=yxf2spbpTSw
https://medium.com/@mari_azevedo/s-o-l-i-d-principles-what-are-they-and-why-projects-should-use-them-50b85e4aa8b6