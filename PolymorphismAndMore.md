## Polymorphism, Objects, and InstanceOf
## The Built in Object Class
 - All types of objects have a superclass named `Object`.
 - Every class implicitly extends `Object`
 - Object has no superclass
 - The `Object` class defines several methods:
 - `public String toString()` - Returns a text representation of the object, often so that it can be printed.
 - `public boolean equals(Object other)` - Compare the object to any other for equality. 
 - Returns `true` if the objects have equal state. 
 - For object, this means the objects being compared are the same object at the same address
## Object Variables
 - You can store any object in a variable of type `Object`
```
Object o1 = new Point(5, -3);
Object o2 = "hello there";
Object o3 = new Scanner(System.in);
```

 - An `Object` variable only knows how to do general things.
```
String s = o1.toString(); //ok
int len = o2.length(); //error
String line = o3.nextLine(); //error
```
**You can write methods that accept an `Object` parameter.**

```
public void checkForNull(Object o){
if (o == null){
throw new IllegalArgumentException();
}
}
```
## Recall: Comparing Objects

**The `= =` operator does not work well with objects. It 
compares the references (addresses), not their state.
It only produces `true` when you compare an object to itself.**
```
Point p1 = new Point(5, 3);
Point p2 = new Point(5, 3);
Point p3 = p1;  // p3 and p1 refer to same object 
if (p1 == p2) {   // false
    System.out.println("equal");
}
if (p1 == p3) {   // true
    System.out.println("equal");
}
```

## The Equals Method
 - The `equals` method compares the state of objects.
  ```
if (str1.equals(str2)) {
    System.out.println("the strings are equal");
}
 ```
 - But if you write a class, its `equals` method behaves like the 
comparator `= =`
```
if (p1.equals(p2)) {   // false for Points :-(
    System.out.println("equal");
}
```

 - This is the behavior we inherit from class `Object`
 - Java doesn't understand how to compare `Point`s by default.
## Flawed Equals Methods

 - The body can be shortened to the following:
```
// boolean zen
return x == other.getX() && y == other.getY();
```
 - It should be legal to compare a `Point` to any object
(not just other `Point`s):
```
/ this should be allowed
Point p = new Point(7, 2);
if (p.equals("hello")) {   // false
    ...
```
 - `equals` should always return `false` if a non-`Point` is passed.
## Equals and Object
```
public boolean equals(Object name) {
    statement(s) that return a boolean value ;
}
```

 - The parameter to `equals` must be of type `Object`.
 - `Object` is a general type that can match any object.
 - Having an `Object` parameter means any object can be passed.
 - If we don't know what type it is, how can we compare it?
## Another Flawed `equals` implementation
Another flawed `equals` implementation:
```
public boolean equals(Object o) {
return x == o.getX() && y == o.getY();
}
```
It does not compile:
```
Point.java:36: cannot find symbol
symbol  : variable x
location: class java.lang.Object
return x == o.getX() && y == o.getY();
```
*The compiler is saying that o could be any object, and the Object class 
(very generic) doesn't have an x or y variable*
## Typecasting Objects
 - Solution: Type-cast  the object parameter to a `Point`.
```
public boolean equals(Object o) {
    Point other = (Point) o;
    return x == other.getX() && y == other.getY();
}
```
 - Casting objects is different than casting primitives.
 - Really casting an `Object` reference into a `Point` reference.
 - Doesn't actually change the object that was passed.
 - Tells the compiler to assume that o refers to a `Point` object.
**Casting Objects Diagram**
```
Point p1 = new Point(5, 3);
Point p2 = new Point(5, 3);
if (p1.equals(p2)) {
    System.out.println("equal");
}
```
![photo](CastingObject.PNG)

## Comparing Different types (Not quite there yet)
```
Point p = new Point(7, 2);
if (p.equals("hello")) {   // should be false
    ...
}
```
 - Currently our method crashes on the above code
```
Exception in thread "main"
java.lang.ClassCastException: java.lang.String
        at Point.equals(Point.java:25)
        at PointMain.main(PointMain.java:25)
```
 - it's because of the line with the typecast
```
public boolean equals(Object o) {
    Point other = (Point) o;
```

## The `InstanceOf` Method
```
if (variable instanceof type) {
    statement(s);
}
```
 - Asks if a variable refers to an object of a given type.
 - Used as a `boolean` test.
```
String s = "hello";
Point p = new Point();
```

![table](InstanceOfTable.PNG)

**Final `Equals` Method:**
```
// Returns whether o refers to a Point object with 
// the same (x, y) coordinates as this Point.
public boolean equals(Object o) {
    if (o instanceof Point) {
        // o is a Point; cast and compare it
        Point other = (Point) o;
        return x == other.getX() && y == other.getY();
    } else {
        // o is not a Point; cannot be equal
        return false;
    }
}
```
## Polymorphism

 - polymorphism: Ability for the same code to be used with different types of objects and behave differently with each
 - Lots of different ways to make this happen
 - `System.out.println` can print any type of object.
 - Each one displays in its own way on the console
**Polymorphism Example**
```
public class Employee{
    private String fName;
    private String lName;
    protected double salary;
    protected int vacationDays;
    protected String vacaForm;
    
    public Employee(String fName, String lName){
        this.fName = fName;
        this.lName = lName;
        salary = 50000;
        vacationDays = 10;        
    }
    
    public double getSalary(){
        return salary;
    }
    
    public int getVacationDays(){
        return vacationDays;
    }
    
    public String getVacationForm(){
        return vacaForm;
    }
}

public class Lawyer extends Employee{
    private double billableHoursRate;
    public Lawyer(String fName, String lName){
        super(fName, lName);
        vacationDays = 15;   // can do this because it's marked
        salary = 100000;        // protected in the Employee class
        vacaForm = "pink";  // this one too
        billableHourseRate = 50;
    }
    
    public double getBillableHours(){
        return billableHoursRate;
    }
}

public class Executive extends Employee{
    private double stockOptions;
    public Executive(String fName, String lName){
        super(fName, lName);
        vacationDays = 20;
        salary = 200000;
        vacaForm = "yellow";
        stockOptions = 1000;
    }
    
    public double getStockOptions(){
        return stockOptions;
    }
}
```

 - Polymorphism: variables of the super class type can also hold instances of sub classes
 - A variable of one type can refer to an object of any of its subclasses
 - For example, if a Lawyer is a subclass of Employee, then this works:
```
Employee e1 = new Lawyer();
Employee e2 = new Executive();
```
when a method is called on `e1` or `e2`, it first checks the specific 
class for the method, then the super class(es) in order

```
System.out.println(e1.getSalary());         
System.out.println(e2.getVacationForm());
System.out.println(e1.getBillableHours());   // won't work on e2
```

 - The code crashes if you cast an object too far down the tree.
```
Employee eric = new Secretary();
eric.takeDictation("hi")    // fails / exception – no method in 
class 
((Secretary) eric).takeDictation("hi");      // ok
((LegalSecretary) eric).fileLegalBriefs();  // exception 
//  (Secretary object doesn't know how to file briefs)
```

You can cast only up and down the tree, not sideways.

```
Lawyer linda = new Lawyer();
((Secretary) linda).takeDictation("hi");    // error
```

Casting doesn't actually change the object's behavior.
It just gets the code to compile/run.

```
((Employee) linda).getVacationForm()    // pink (Lawyer's)
```

*If you define a method parameter to take the super class (Employee), you can also pass instances of the sub class to the method (Executive,Lawyer)*

```
public class EmployeeMain{    
public static void main(String[] args) {
Executive e1 = new Executive("Lisa", "Thompson");
Lawyer e2 = new Lawyer("Fred", "Smith");
printInfo(e1);
printInfo(e2);    
} 
public static void printInfo(Employee empl){
System.out.println("name: " + empl.getFName() + " " + empl.getLName());
System.out.println("salary: " + empl.getSalary());
System.out.println("vacation days: " + empl.getVacationDays());
System.out.println("vacation form: " + empl.getVacationForm());        
System.out.println();
}
}
```

Circle and Square are sub classes of Shape
Use InstanceOf when you want different behavior based on the actual sub class passed to the method:

```
public int draw(Shape s){   // can pass in a Circle or Square
  if (s instanceOf Circle){
          //draw the circle
  else if (s instanceOf Square)
       // draw the square
...
}
```

Polymorphism with subclasses

A subclass can override a method - This is a form of polymorphism as well

```
Shape s = new Square();
s.printDetails();
public class Shape {
...
public printDetails(){
//print basic details here
}
}
public class Square extends Shape{     
  public printDetails(){
   super.printDetails();
     // print extra details because we know it's a square
  }
}
```

![WeirdFaces](WeirdFaces.PNG)

`Given the classes above, what output is produced by the following code?
Assume the "drawXXXXX" methods exist to draw the associated shapes shown`
```
Face[] emojis = {new AngryFrown(), new Face(), new Frown(), new Smiley()};
for (int i = 0; i < emojis.length; i++) {
   emojis[i].drawFace();
}
```

Polymorphism is about the same object calling/using different code
So technically, overloading a method is also polymorphism
Overloading, remember, is having the same method or constructor 
name used multiple times, but having different parameter lists
Examples:
• `public Person(String personID)`...
• `public Person()`...
• `public Person(String personID, String fName, String lName)`...





