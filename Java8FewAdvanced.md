


# Java 8 and Functional Programming


- 	From 80's we are following the Object Oriented Programming
-	But now the focus is on the Functional Programming
	
	-	Instead of focusing on the Object we will focus on the behavior of Object
	-	Behavior is the function
	

	
	
## 1. 	Types of Programming 

	a.	Imperative Programming 
	b.	Declarative Programming
	
		a.	Imperative Programming
		
			-	In imperative programming we focus what to do and how to do
			
		b.	Declarative Programming 
		
			-	In declarative programming we focus on what to do and no needs to think about how to do it
			

			
			
## 2.	Lambda Expression

	-	Whenever we write Anonymous Inner Classes then we write lot's of boiler plate code
		-	Instead of that we can focus on the actual thing 
		-	In Lambda Expression we write the main required thing instead of writing the boiler plate code
		

		
## 3. 	Stream API

	-	Stream API overcomes the problem of variable mutation in concurrency environment
	-	Stream API will make sure that there will no bugs related to Mutation in Multi-threaded Environment
	-	Stream API helps to process the data in a declarative manner 
		
		
		-	Stream API will help in working with Collections and DB Connections

	-	Stream API helps in Concurrency and Functional Programming
	
		-	Stream API will focus on what to do instead how to do it



## 4.	Interfaces in Java 8


	-	Since lots of new methods are added as part additional features like Stream API 
	-	Oracle has defined a methods in the interface instead of making them as abstract
	
		-	From Java 1.8 an interface can have concrete methods - methods can have body
		-	Concrete methods in an Interface needs to be defined with default keyword
		
		
	-	Multiple inheritance with interface is not possible in Java 1.8 with one than interface having same default methods
	-	Static methods can be defined in interfaces from Java 1.8
	
	
		
## 5.	forEach Method


	-	forEach method is a internal loop 
	-	forEach method is faster than external for loops
	-	forEach method takes Consumer Interface as an argument
	- 	Param for forEach method is implementation of Consumer Interface
	
	
## 6.	Consumer Interface

	-	Consumer is a functionalInterface 
	-	Consumer is having only abstract method called accept
	-	forEach method of stream will have the Consumer as a Param 
	
	
	 
	
## 7.	Modifying Interfaces once they are published


	-	We should not modify interfaces once they are published, since it will start giving error to existing things
	-	Till Java 1.7 we cant able to modify the interfaces since interface can only support abstract methods
	-	From Java 1.8 we can modify the interfaces since its support the default methods
	-	Using the default keyword we can define the methods inside an interface 
		
		-	Defining methods will not give errors to existing classes 
		


## 8.	What is functional Programming?
			
	-	Functional Programming is a declarative programming 
	-	Functional Programming focuses on what to do rather than how to do 
	-	Functional Programming in Java is achieved through Stream API
		
		

## 9.	Parallel Stream and Stream

	-	Each system will have cores -->> quad core or octa core
	-	To use each core we need Threads
	-	By default there will be only Thread
	-	Programmer needs to create those to make use of System cores
	-	With Stream API -> programmer no needs to create the thread to process the values
	
		-	Stream API will create the threads and process them automatically in a multi-threaded mode
		-	Stream API will create threads according to number of core on System
	
	-	Parallel Stream will process the data in multi-threaded mode
	-	In Stream we have two types of methods 
	
		1.	Intermediate Method
		2.	Terminate Method
		
	-	Stream is a lazy  and Intermediate Method of Stream is Lazy evaluation method
	- 	To get the results in Stream we need to use and end with terminate methods like count(), findFirst(), collect(), min()
	
	-	In Stream once used values can't reused again - will throw exception
	
## 10.	Lazy Evaluation and Terminal Function


	-	Intermediate methods like map() and filter() methods are lazy methods that will not gets executed when there are no terminal methods
	-	Based on the kind of terminal methods intermediate will work
	-	For Ex:
	
			 - terminal method is findFirst() then
			
				-	only map() (if there) will ask the filter() method to get the first values
				-	Instead of working on each or all the elements of stream --> intermediate methods are lazy and work based on the terminal method funtion type
				
				
## 11.	Date and Time API


	### Interview Example for Stream and Lambda for finding the date and time based on the time zone for the Jenkins plugin
	### Enabling the code freeze based on the Date and time of the Zone - 
	LocalTime ltzone = LocalTime.now(ZoneId.getAvailableZoneIds().stream().filter(s -> s.equals("America/Antigua"))
				.map(ZoneId::of).findFirst().orElse(ZoneId.systemDefault()));
				
	-	Earlier Date and Time classes are not Thread safe
	-	Java 8 Classes are Thread Safe
## 12.	Method Reference 


	-	Anonymous inner classes can be expresses in Java 8 either by Lambda Expressions or Method Reference 		
