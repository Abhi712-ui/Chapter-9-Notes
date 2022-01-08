## Object Hierarchy
 - Hierarchy is just relationships
![Animal Hierarchy](AnimalHierarchy.PNG)
 - Hierarchies help to avoid redundancy in coding
 
## Inheritance
 - Allows a subclass (lower on the hierarchy) to extend the functionality 
of a base class (or superclass, higher up), inheriting all state & 
behavior
- Superclass – the parent 
- Subclass – the child, which extends the superclass by adding and/or 
replacing state/behavior of the superclass
A subclass can ONLY inherit from one superclass
 - Java has "SINGLE INHERITANCE"
 - Can have multiple levels (examples above)
 - Can affect variables, constructors, and methods
 - In Java, all objects at the top most level are of the class "Object"
 
**Subclass syntax (Extending but not replacing state/behavior)**
```
public class Knight extends Warrior{
//new var 
private String horseName;  
 
// new method
public void ride(int speed){
```
**Overriding superclass methods**
 - Can provide new versions of superclass methods specific to the subclass
```
public class Knight extends Warrior{
...
//override existing
//@Override is a note to the compiler 
 // method signatures must match
  @Override
public int getBaseDefense(){
return 200;
}
```

## Calling Override Methods

 - If you override a method in a subclass, you can still call the 
method from the superclass
 - We use this to refer to the current object, and it works for the 
current class' variables, but we must use the super class 
methods to get at the superclass'variables
 - And we can use super  to refer to methods in the superclass 
that were overridden. Example in subclass:

```
public int getHealth(){
return super.getHealth() + 100;
}
```

 - Constructors are not inherited
 - But you can use the super keyword to call its constructor to do much of the work.
```
Superclass for Warrior: 
public Warrior(String name){
private int attackPoints;
...
}

Subclass for Knight:
public Knight (String name){
super(name);
horseName = makeRandName();
...
}
This calls the constructor for Warrior 
first, then adds special behaviors
```

