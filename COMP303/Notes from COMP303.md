# Chapter 2

## 2.1 
main idea: encapsulation is data hiding

## 2.2
To store informaiton like "Ace of clubs", is `card="AceofClubs"` good practice? No. Use primitive data types if they actually make sense. Don't store things as integers if they do not
naturally make sense as integers. Instead, we create classes. 

We would have some class card with fields 'Suit' and 'Rank'.
Suit would be enumerated type, a special kind of class:

```
enum Suit {
    CLUBS, DIAMONDS, SPADES, HEARTS
}
Suit mysuit = Suit.CLUBS;`
```


## 2.3 - Visibility
Fields should almost always be private, make get and set methods.

2.4 - Object diagrams

![qownnotes-media-tJEyRy](media/qownnotes-media-tJEyRy.png)

Here, fields of primitive or referenced type are explained for a class. The solid arrows in the flowcharts indicate the properties of objects recursively.

## 2.5
what is the issue with this getter code?

```
public class deck {
    private List<Card> aCards = new ArrayList<>();
    
    public List<Card> getCards() {
        return aCards;
    }
}
```

It returns a reference to an object! This refers to an <b>escaping</b> of the scope of the class.
This can be done in 3 ways:    
1. Returning a reference to an internal object
    - when you return an object in a getter function
2. storing an external reference internally
    - e.g. getter method takes object argument, assigns internal object to function argument
3. Leaking references through shared structures

## 2.6 - Immutability
Good encapsulation means you can't modify the internal state of an object without going through its methods.

However, since strings are __immutable__, this is less of an issue.

## 2.7 - Exposing Internal Data
A challenge is how to provide access to objects without escaping scope. One way is to grant access to only immutable subclasses. Another way is to simply return a copy
e.g.
```
public List<Card> getCards() {
    return new ArrayList<>(aCards);
}
```
This code uses the constructor of new ArrayList to place all elements.of aCards.

### Shallow vs deep copy
Shallow copy: give references to objects in array, but not array itself<br>
Deep copy: make copy of both array and elements

## 2.8 - Input Validation
You could be given null or unsupported inputs from setter methods.
Throw exceptions for this.

## 2.9 - Design by Contract
use format:

```
//@pre pRank != null
public Card(rank pRank) {
    assert pRank != null;
    //dothing
}
```


# Chapter 3
## 3.1
The interface to a class is the set of its public methods
The interface to an object is the set of methods that can be called on the object.

#### Abstract method
An abstract method is a method without an implementation.

All abstract methods do is get the compiler to enforce the presence of certain methods

Syntax 

```
public interface MyInterface {
    Field myField = new Field;
    int myMethod();
}
```
```
public class Deck implements MyInterface { ... }
```

## 3.2 - Specifying Behaviours in Interfaces
With some random class, we can't just call some sorting method from a library. The sort method doesn't know how to compare different objects. To do this, we implement the interface Comparable and define the following method

```
public class implements Comparable<Card> {

    public int compareTo(Card pCard) {
        return this.rank - pcard.rank
        }
}
```

All `Collections.sort()` needs to sort anything is just a compareTo function that returns -1, 0 , 0, or 1, depending on if an object should come before, at the same spot, or after.

__Nomenclature for interfaces:__ end in -able

## 3.3 - Class Diagrams

![qownnotes-media-eHJsbb](media/qownnotes-media-eHJsbb.png)

a + next to a field or method: public<br>
a -: private<br>
a #: protected<br>
a ~: default/package


### Terminology
**Inheritance:** shown with solid line. superclass/class, parent/child class<br>
**Association**: no dependency, just an association, e.g. otter class eats sea urchin class<br>
**Abstract** classes are often inherited<br>
**Aggregation** specifies a whole and its parts. e.g. a flock class is an aggregation of a Crow class. aka a class can exist in an aggregation (or not)<br>
**Composition:** when a child can't exist without its parent, e.g. a classroom class inside a schoolbuilding class. Depict with closed diamond.
#### Numerical notation on class lines:
-x-: only x amounts of class exist<br>
-x...y-: anywhere from x to y classes exist


## 3.5 - Iterators
they are swag because they let you access list elements without de-encapsulation. You just need to make sure there's some .next() and .hasNext() method.
-> exercise: implement hasNext for some made up classv

## 3.6 - Iterator DP
**Def**: provide access to a collection of objects encapsulated in another without violating the encapsulation of the object.<br>
![qownnotes-media-eJyUMt](media/qownnotes-media-eJyUMt.png)

## 3.7 - Strategy DP
**Def:** a famiy of algorithms that are each encapsulated, and made interchangeable
![qownnotes-media-lbLyAR](media/qownnotes-media-lbLyAR.png)
It seems to just be that both strategies can implement AbstractStrategy

e.g. Implement comparator by rank fo a card vs by suit of a card.

## 3.8 - Interface Segregation Principle

# Chapter 4 - Object State
## 4.1 - Static vs dynamic
Static: there exists a class Deck with field aCards that is an ArrayList of Card objects.
Dynamic:  Deck contains 0 cards. Then 3 cards. Then a card is removed, so it has two cards.
Aka compile time vs runtime

## 4.2 - Object State
**def** the information the object represents at a given moment

Concrete vs abstract state: 
- Concrete: values stored
- Abstract: a subset of the concrete state space. e.g. a score of 0. or an empty deck.  Or a full deck. Information in objects that are relevant to design or implementation.

**State space:** how many concrete states exist. e.g. if it is an int, it can have 2^32 states.

Function objects are stateless.
Immutable objects have only one state.

## 4.3 - State Diagrams
![qownnotes-media-aIbTuX](media/qownnotes-media-aIbTuX.png)<br>
Abstract example

![qownnotes-media-kYGVLY](media/qownnotes-media-kYGVLY.png)<br>
Real example

The abstract `Empty` state implies the constructor does not populate the object with any cards

## 4.4
An example of unnecessary state information is storing the size of a list as a field and not as the return of list.size(). Manually changing the card size int field takes up memory and makes code less elegant. Just use .size()

## 4.5 - Nullability
Why is this bad? Nulls can exit for many reasons:
- object never initialized
- object initialized and then deinitialized temporarily, intentionally
- value has meaning, e.g. absence of a value is a value
    - What meaning ? who knows.

Thus, to be a good programmer is to not do this.

Strategies for avoiding null values
- Design by contract
- Input validation
- Optional fields
- Null object design pattern

### Null object DP 
Used anytime a null object is returned
Create an object with default values as opposed to checking if object is null.
e.g. an object with an image: 
w/o DP: check if img is null
w/: image would just return a default image aka a null image
- e.g. a user system will have a default "guest" profile if user not logged in, as opposed to null errors.    

## 4.6 - Final fields and vars
You can make primitives immutable with final. But not references! Though it'll always reference the same location in memory, the contents can be changed.


## 4.7 - Object Identity, Equality, and Uniqueness
Two objects with the same state are nonetheless stored in different places, are different objects.
Equality between objects is always technically false. Thus, it is up to a programmer to define object equality.
ie __let two objects are the same if their fields are the same.__

Accomplish this by __overriding the equals() method__
Note: overriding equals() requires overriding hashCode()

If all objects of a class are unique, then this equality is not needed

## 4.8 - Flyweight DP
Lets you fit more objects in RAM by sharing data that's the same
Let's say you have 10 million books. But there exists fields where the contents are often the same across books.
e.g. distributors, type, extraInfo. Some type will be only carried by some distributor who has the same extraInfo.
Thus, have some MoreInfo class where objects store these smaller possibilities of data. This is called a flyweight class.

### Flyweight factories
Returns all the flyweight possibilities. 
Note clients don't create flyweights. The factory manages this.

### Requirements
- Private constructors so foreigners cannot instantiate a new object
- A static flyweight store so that clients cannot control the creation of the objects of the class
- Static access method, checks if flyweight exists that meets criteria. If yes, returns object. If no, creates, and returns obj.<br>
NOTE: SEE STATIC VS NONSTATIC

## 4.9 - Singleton DP
A way of creating a single object shared amongst many different resources without any data being lost or changed. 
essentially a global variable

3 elements to Singleton
- A private constructor. Clients cannot create new objects
- A global variable that holds the reference to the singleton object
- An accessor method, instance(), that returns the singleton instance.

e.g. in solitaire, a game object could encapsulate the entire game

```
public class GameModel {
    private static final GameModel INSTANCE = new GameModel()
    private GameModel() { ... }
    public static GameModel instance() { return INSTANCE; }
}
```

*We instantiate the Singleton object within the class.*

## 4.10 - Objects of Nested Classes
Two types of nested classes:
1. Static nested classes
2. Inner classes
    - Anonymous inner classes
    - Local inner classes

![qownnotes-media-rFxyiA](media/qownnotes-media-rFxyiA.png)


### Inner classes
- Declared within another class. e.g:

```
public class Deck {
    public void shuffle() { ... }
    public Shuffler newShuffler() { return new Shuffler(); }
    
    public class Shuffler {
        private int aNumberOfShuffles = 0;
        private Shuffler() {}
        
        public void shuffle() {
            aNumberOfShuffles++;
            Deck.this.shuffle();
        }
        
        public int getNumberOfShuffles() {
            return aNumberOfShuffles;
        }
    }
}
```

Here, Deck.this refers to the instance of the outer class, as opposed to the inner class. You could have Shuffler.this to refer to the inner class and its methods/fields.

### Anonymous classes
E.g.
```
public class Deck {
    public static Comparator<Deck> createRankComparator(Rank pRank){
        return new Comparator<Deck>() {
            public int compare(Deck pDeck1, Deck pDeck2) {
                return countCards(pDeck1) - countCards(pDeck2);
            }
            
            private int countCards(Deck pDeck) {
            /* returns the number of cards in pDeck with pRank */
            }
        };
    }
}
```

An anonymous class is an unnamed class that creates only ONE object.
Make it by instantiating a new object
```
Animal bigfoot = new Animal() {
    public void myMethod() { ... }
    int myfield ...  
  }
```

#### Method 2 of creating: with an interface

Say you have some interface

```
interface MyInterface {
    void doSomething();
}
```

```
MyInterface myObject = new MyInterface() {
    @Override
    public void doSomething() {
        System.out.println("Doing something in the anonymous class");
    }
};
```

Here you can create an anonymous class that overloads the method for only that.


# Chapter 5 - Unit Testing
## 5.1 
an `oracle` is the expected result of a unit test.
Comparing a test result with the oracle is an assertion.

Exhaustive testing is when you test all possible inputs.

Regression testing is when you test things that were workign before to ensure they still test.

## 5.2 - JUnit 
In JUnit, a unit test maps to a method.

We use junit.Assert to verify the outcome of a unit test.

## 5.3 - Organizing Test Code
Organize test codes into suites
use @Test before a test

Create a different test package. For each class, create a unit test file

## 5.4 - Metaprogramming
**Def** to write code that operates on a reprsentation of a program's code.
This is called reflection in java


### Introspection
The code below uses the `Class.forName()` method which takes a string and returns an object. (Class<Card>) casts it into the appropriate class. Since the `Class.forName()` is brittle, we wrap it in a try catch block.
```
try {
    String fullyQualifiedName = "cards.Card";
    Class<Card> cardClass = (Class<Card>) Class.forName(fullyQualifiedName);
} 
catch(ClassNotFoundException e) {
    e.printStackTrace();
}
```
Note Card.class holds a reference to the Class object representing the class Card. This is a **class literal**

You can also edit the fields of a class with metaprogramming.

You can you can also attach metadata in forms of annotations, ie extra information about code. We do this as humans using comments,but code cannot read comments. Thus, we use an annotation type

```
public @interface Test {}
```

Then, annotation instances can be added to the code, in the form @Test

## 5.5 - Structures of Tests
Tests should be
1. Fast
2. Independent
3. Repeatable (same results on different systems)
4. Specific and focused
5. Readable

Use @Before to setup certain objects that are used in all tests to avoid reprocessing superfluous items

## 5.6 - Tests, Exceptions
We can test for exceptions using JUNIt's `expected` property

```
@Test(expected = EmptyStackException.class) 
public void testPeek_Empty() {
    new FoundationPile().peek();
}
```
This fails test if exception not raised

## 5.7 -  Encapsulation and Unit Testing
Sometimes we need functionality not part of the interface of the class being tested. e.g. we want a size() method for an object which doesn't need it to run internally, but we want it for testing.

To do this, we can simply define helper methods in our testing class.

We can get around private methods by metaprogramming, ie `method.setAccessible(true);`

## 5. 8 - Testing with Stubs
A **stub** is a greatly simplified version of an object that mimics its behavior sufficiently to support the testing of a UUT that uses this object. E.g. you have a program where when it is the turn of player B, AI decides to execute any of 10 moves. It would be difficult to test all 10 moves. Instead, we create a stub and only test to make sure on Player B's turn, it selects a move, any move.

# Chapter 6

## Composition and Aggregation
An object being **composed** means one object stores a reference to one or more other objects.
e.g. a String is a collection of chars.
Adding too much functionality in only one class creates a God class which is bad design. We delegate services to some objects.

Note: aggregation and composition is white diamodn    

## Composite Design Pattern
Store data as trees ?
Design some very basic interface Box. One unit can be a Box. Boxeso f many units can be a CompositeBox that supports many units. A leaf implements Box. a subtree implements Compositebox.

![qownnotes-media-QLQHaE](media/qownnotes-media-QLQHaE.png)

## Command Pattern DP
Build a command class with execute(). A request or behaviour class then implements this. It encapsulates information to perform action or trigger an event.

## Sequence Diagram
Actors and objects - Actors are clients, objects are classes

Response/return messages are dashed lines.

## Decorator DP
Lets you attach new behaviours to an object. Place object inside a special wrapper with these behaviours.

E.g. you want to send a message via whatsapp, facebook, email, sms, etc.

You have a NotifierDecorator with a send() method and getUsername method.



# Java Reflection
Getting fields with reflection of some class Cat
e.g. Cat class has name, and age
```
Cat myCat = new Cat("John", 6);
Field[] catFields = myCat.getClass().getDeclaredFields(); //returns array of type fields 
for (Field field : catFields) {
    field.setAccessible(true); // this sets private field to changeable
    if (field.getName().equals("name")) {
         field.set(myCat, "Jimmy"); //this sets the field of object myCat to a new name.
    }
}
Method[] catMethods = myCat.getClass().getDeclaredMethods();
catMethods.get(0).invoke(myCat, arg1, arg2, ...); // executes first method as object myCat
```

Introspection
`String fullyQualifiedName = "cards.Card"; // string with format: in package cards, get class Card`


`Class<Card> cardClass = (Class<Card>) Class.forName(fullyQualifiedName); // get the class Card with Class.forName(string) and cast it to Class<Card> as it usuallyl returns Class<?>`

You want to wrap this code in 
```
try { 
     //getclass \
} catch(ClassNotFoundException e)  {   
     e.printStackTrace(); 
}
```

Optional.of(object); if you are sure it's not null
Optional.ofNullable(something that might be null);
an empty optional: Optional.empty()

TO use optionals, you create an object of  class Optional<desiredclass> optionalValue = ...
to get the object stored, do optionalObject.get(). Check if optional with: optionalObj.isPresent()

use optionalObj.orElse(new Cat("Unknown", 0)); -> creates null cat object if optional doesn't have value

same idea: optionalCat.orElseGet()


Lambda expressions
Used to pass in a specific implementation of an interface

e.g. there is some interface Printable with method print(). Instead of defining print method for cat class that implements Printable, do this:
`printThing( (/* params */) -> System.out.println("Meow"));`

```
static void printThing(Printable thing) {
    thing.print();
}
```

How to get the class for an object?
1. Class<T> myClass = Class.forName("package.ClassName");
2. Class<T> myClass = myObj.getClass();
3. Class<T> myClass = ClassName.class;


Decorator Design Pattern
Add functionality to some classes without affecting others
A car is decorated with attributes
Let's decorate a base car with armor or a gun. A base car shouldn't know what to do with a gun. Just the gun-clad car


Decorator design pattern:
Pizza interface. Implemented by PlainPizza and PizzaTopping abstract class.

Abstract class has a pizza field. Has a constructor with a pizza object. In argument, pizza object.

# Chapter 7
## 7.1 - Inheritance
Denoted in class diagrams by solid lines with white triangles from subclass to superclass

## 7.2 - Inheritance and Typing
Implemented with `extends` keyword.

An object will be created using declaration of the subclass AND the superclass. You choose the runtime type with `new`keyword. But the variable can be of type superclass. e.g. `SuperClass myObj = new SubClass();`The concrete state is SubClass, but the variable is of type SuperClass.

The *run-time type* is the most specific type, of the one following the `new` keyword.
The *compile-time type*type is the type of the variable which stores the address to an object.

```
superclass myObj = new subclass();
subclass myObj2 = new subclass();
myObj2 = myObj;
```
will this work? No. They need to have the same compiletime type.

```
public static void myMethod(superclass p) {
        return;
}
```
Which ones of these will work?
`myMethod(subObj), myMethod(supObj);` They will both work! If the superclass is specified, then it will take both the compiletime type sub or super, as they are both of type superclass. But if you change the argument to be `subclass p` in the method, then it will not work for both. You can fix this by **casting!**

Note: if you have a method in subclass and only in subclass, you cannot do it by calling myObj.myMethod(); as superclass variable type does not know where to find `subclass.mymethod();`. We can fix this by downcasting: ((subclass)myObj).makeNoise();

To verify safe downcasting, 
```
if (myObj instanceof subclass) {
    return ( (subclass) myObj).myMethod();
}
```

Note: java is only single inheritance. But all classes inherit `java.Object `if they don't inherit any other class. So `java.superclass` inherits `java.Object`, and `subclass` inherits `superclass` and `java.Object` by transitivity

If subclass also defines a method in superclass, there's nothing you can ever do to run superclass's method from subclass object.

## 7.3 - Inheriting Fields
Can a subclass access a `private` field of superclass? No. You can still have encapsulation if you use `protected` instead of `private`.

With inheritance, fields are generated from the top down, from the most general to the most specific class. e one named in `new`). This is because the __first action of a constructor is to call the constructor of its superclass__. Order of constructors is bottom up.

The constructor of a superclass may be explicitly called with `super()`. Note this happens automatically anyway. It must be the first statement. This way we can pass up values to the constructors of superclasses

## 7.4 - Inheriting Methods
What is static? __Belonging to a class.__
An __implicit parameter__ is the obj in `obj.myClass();`

To refer to the method of a superclass, we can use `super.method()`. It finds the first method found by going up in the lcass hierarchy, so not necessarily the first superclass. 

Override using @Override

## 7.5 - Overloading Methods
If you do method(subclass);, but only method(superclass) is defined, it will run method(superclass). Try to avoid overloading except for very well known methods, or constructors. Otherwise, just make different methods.

## 7.6 - Inheritance versus Composition


## 7 - Abstract Classes
Abstract classes aren't used by client, but internally.
use interface to represent type, use abstract classes to collect

Not all interfaces can be abstract classes and vice versa.



midterm feedback
q2 -> checking for unique toppings, check for uniqueness
remember data structures from 250 ! 
know strings, maps, sets

return array.length > 0;
instead of if (arraylength > 0) { return true; } else { return false; }

Exceptions in Java: objects, new IllegalArgumentException();

comparator: 
public class myClass implements Comparator<String> {
    return 01.length() - 02.length(); // this is very fast
}