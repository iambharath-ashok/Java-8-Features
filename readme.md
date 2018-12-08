
Java 8 Features

1. 	Functional Interfaces
2.	Lambda Expressions
3.	Default Methods
4.	Predicates
5.	Functions 
6.	Method Mapping or Scope Resolution Operator
7. 	Stream API
--------------------------

  1. 	Lambda Expressions

    -	Main goal of  Lambda Expressions is to introduce Functional Programming in Java
    -	A lambda is a anonymous function in Java that doesn't have name return type and access modifiers
    -	Lambda Expressions are also know as Closure or anonymous function

    Benefits of Lambda Expressions

      1.	Less Code
      2.	Easy to implement Anonymous Inner Classes
      3.  Pass parameters to methods 


  2.	Functional Interfaces

    -	If an interface is having one and only abstract method then its known as functional interfaces
    -	Built in Functional Interfaces of Java are 
      -	Runnable which has run method
      -	Comparator which has compareTo method

    - 	We can define any number of default methods in an functional interfaces
    -	But there should one and only abstract method
    -	@FunctionalInterface annotation can be used to mark the interface as functional interface

  3.	Default Methods

    -	A default method in an interface is a method thats having body or has concrete implementation
    -	Default methods in interface can result in Diamond Problem in Java
      - 	A common child interface can't inherit from two diff interface have same default method structure

  4. 	Predicates

    -	Predicate is a function ion Java 1.8 with single argument it returns a boolean value
    -	To use Predicate we need to implement Predicate interface 
    -	Predicate is a functional interface with only one abstract method ---> test(T t)

      -	Predicate method can take any value and returns boolean true or false

        public interface  Predicate<T> {

          public boolean test(T t);
        }

    -	Since predicate is a functional interface we can express it as Lambda Expression


    Predicates Joins

      -	We can use more than one Predicates by Joining them
      -	and(), or() and negate()


  5.	Functions

    -	Functions are similar to Predicate - Except that it can return any type 
    -	Function is a FunctionalInterface with only one Abstract Method


      public interface Fucntion<T,R> {

        public R apply(T t) {
        }
      }

  6.	Scope Resolution Operator (::)

    -	:: is a scope resolution operator in Java
    -	Functional interfaces can be expressed in Lambda Expressions or using :: Scope Resolution Operator
    -	:: is used to map methods and constructor to Functional interfaces
    -	Both instance and static method can be mapped to Functional interfaces with :: Operator
    - 	Constructor of class can be initialized via Lambda and Method Mapping

    @FunctionalInterface
    public interface MyInterface {

      public String sayHello(Sting name);

    }

    public class MyClass {

      public String myMethod(String name) {

      }
    }


    MyClass mc = new MyClass();
    MyInerface mi = mc :: myMethod;
    String name = mi.sayHello("Guru");


    -	Method Params structure should match with FunctionalInterface




  7. 	Stream API

    -	Stream helps to process the data in a declarative manner -> similar to SQL 
    -	Stream makes it easy the process the data or Objects inside a Collections
    -	Processing Collections

      -	java.util.stream.Stream -> is an interface
      -	We can get a Stream on Collections by invoking stream() method on top of Collection Implementation Type


        Collection java.util.stream.Stream stream() 

      -   stream() method was added to Collection Interface in Java 1.8

    -	Once we have Stream we can process the collections is two steps

      1.	Configuring the Stream
        -	Configuration can be done in two ways 

          1.	Filter

              public Stream filter(Predicate<T> p)

                -	Takes boolean expressions
                -	Filters few Objects on the Collection and returns Stream with new set of Collections

          2.	Map

              public Stream map(Function<T,R> f)

                -	Takes function as argument
                -	It may create a new Collection or modify the existing objects on the collections


      2.	Processing the Collections

          -	 To process the collections Stream provides multiple methods like 

              collect(), count(), sum(), min(), max()
