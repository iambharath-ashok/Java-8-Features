
## Java 8 Hands On

	1.	Comparator
	
		-	Get Stream() on Collection and use Sorted() method
		-	Sorted() method will take a Comparator implementation 
		-	Can be Lambda or Method Reference or even Anonymous Inner Class
		
			-	Comparator is a functional interface with compare method takes to params
			
				int compare(T t1, T t2)
				
			-	For String Comparison we need to use compareTo() method
			
				
			-	list.stream().sorted((t1,t2) -> t1.compareTo(t1)).collect(Collectors.toList());
			
			
	2.	filter
	
		-	filter is intermediate method thats used to filter the values from the stream
		
			
			list.stream().filter(k -> k.equals("")).findFist().orElse(null);
			
	3.	map
