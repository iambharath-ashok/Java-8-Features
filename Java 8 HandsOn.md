

# Java 8 Examples of Lambda Expressions

## Stream API

	-	Sequence of elements
	-	Source
	-	Aggregate operations
	
	1. Stream Operations
	
		1.	Pipelining
		2.	Internal Iteration
		
		Transactions 
			
			-	filter  <- Stream<Transaction>
				
				-	filter elements based on the stream
				
			-	sort  <- Stream<Transaction>
			-	map  <- Stream<Integer>
				
				-	map() method extract the elements and transform it from one form to another form
				
			-	collect	<-	List<Integer>
	2. Stream vs Collections

		-	Collections is Data Structure for storing the data
		-	Stream is for performing computations
	
	
	3.	Stream Operations: Exploiting Streams to Process Data
	
		-	filter, sorted, map which can be connected together to form a pipeline
		-	collect which closes the pipeline and return the result
	
	4.	Filtering - There are several operations that can be used to filter elements from a stream: 

		-	filter(Predicate): Takes a predicate (java.util.function.Predicate) as an argument and returns a stream including all elements that match the given predicate
		-	distinct: Returns a stream with unique elements (according to the implementation of equals for a stream element)
		-	limit(n): Returns a stream that is no longer than the given size n
		-	skip(n): Returns a stream with the first n number of elements discarded 
		
	5.	Finding and matching
		
		-	anyMatch, allMatch and noneMatch
		-	findFirst, findAny - returns Optional
		
	6.	Optional<T> is java.util.Optional is a container class to represent the existence or absence of value
	
		-	Optional has several methods to verify the existence of value 
		
	7.	Mapping 

		-	map method takes Function FunctionalInterface as an argument
		-	takes each element at a time and transform it to another form or new element	
	
	8.	Numeric Streams
		
		-	Java 8 has added 3 new primitive specialized interface to tackle the issue - IntStream, DoubleStream, LongStream
		-	Common methods to convert a stream to specialized version are mapToInt, mapToDouble, mapToLong
		-	These methods return specialized stream instead of Stream<T>
		-	mapping to Integer Stream
		
			transactions.stream
				.mapToInt(Transaction::getValue)
				.sum()
				
		-	Numeric Range using NumericStreams
			
			-	range and rangeClosed
			
			IntStream i = IntStream.rangeClosed(10,20)
							.filter(i -> i%5==0)
							
	9.	Building Stream
			
		-	We can create a stream from file or collection, array or even numbers
		-	Stream.of and Arrays.stream() can be used to build the stream out of array			
		
----------------------------------------------------------------------------------------

# Contents 

	1.	Comparator
	2.	forEach
	3.	filter
	4.	collect
	5.	findAny or orElse
	6.	Collectors.groupingBy
	7.	convert Stream to List
	8.	Array to Stream
	9.	Stream is already Operated upon and Supplier
	10.  Sort a Map
	11.  List to Map
	12.	 Filter a Map
	13.	 flatMap
	14.	 convert Map to List
	15.	 Optional in Depth
	16.	 String Joiner
	17.	 Stream File Reader
	18.	 Join Arrays
	19.	 String to Char Array
	20.	 Covert Primitive Primitive array to List

----------------------------------------------------------------------------------------
		
## 1. Comparator to sort the List Custom Objects

	- 	List is a Data Structure
	-	Custom Objects with few fields
	
		Sorting Techniques
		
			- Before Java 8
				-	Collections.sort(ListImpl, ComparatorImpl)
			- Java 8
				-	Collections.sort(ListImpl, Lambda Expres of CompImpl);
				-	Java 8 has added sort method on List Interface
				-	We can call sort(CompIml) on List Impl
				-	Can pass Lambda Expr to ListImpl.sort((T1,T2) -> T1.compareTo(T2))
				-	list.sort(Comparator.comparingLong())| list.sort(Comparator.comparingDouble())| list.sort(Comparator.comparingInt())
				
				-	list.sort() and list.stream().sorted()
				
					-	sort() and sorted() method is a utility to sort the elements of Collection
					-	sort() and sorted() takes Comparator as an argument
					-	We can pass either impl of Comparator Interface or Lambda Expression of Comparator interface
					-	We can make use of Comparator interface methods like comparingDouble, comparingInt, comparingLong methods
					-	comparingDouble, comparingInt, comparingLong methods is Primitive specialized functions that takes primitive functional interface impl as an argument
					
						@FunctionalInterface
						public interface ToIntFunction<T> {

							int applyAsInt(T value);
						}
						
						@FunctionalInterface
						public interface ToDoubleFunction<T> {

							double applyAsDouble(T value);
						}
						
						@FunctionalInterface
						public interface ToLongFunction<T> {

							long applyAsLong(T value);
						}

						
			
###	Code Snippets of Comparator


-	Using Collections.sort() 
	
	1.	Sorting String List in Natural Order
	
		Code Snippets:
		
			public class SortingUsingCollectionsSort {
				public static void main(String[] args) {
					String[] stringArray = new String[] { "ffff", "rrr", "dddd", "aaaa", "bbbbbb" };
					List<String> stringList = Arrays.asList(stringArray);
					
					Collections.sort(stringList);
				}
			}
		Output :
				[aaaa, bbbbbb, dddd, ffff, rrr]		
			

		
	2.	Sorting String List In Reverse Order
			
		Code Snippets:
			
			public class SortingUsingCollectionsSortReverseOrder {
				public static void main(String[] args) {
					String[] stringArray = new String[] { "ffff", "rrr", "dddd", "aaaa", "bbbbbb" };
					List<String> stringList = Arrays.asList(stringArray);

					Collections.sort(stringList, new Comparator<String>() {
						@Override
						public int compare(String s1, String s2) {
							return s2.compareTo(s1);
						}
					});
					System.out.println(stringList);
				}
			}
		Output :
				[rrr, ffff, dddd, bbbbbb, aaaa]
				
	3.	Sorting Custom Objects
	
		Custom Objects can be sorted with Collections.sort() by implementing Comparable and Comparator Interface
		
		1.	Comparable
			
			-	Comparable will be used at class declaration
			-	Something like default sorting 
			-	Below is the example used to sort by last name ... if both last name are same then will use first name as criteria for sorting
			
			
			Code Snippets :	
				
				class Author implements Comparable<Author> {

					String firstName;
					String lastName;
					String bookName;

					Author(String firstName, String lastName, String bookName) {
						this.firstName = firstName;
						this.lastName = lastName;
						this.bookName = bookName;
					}

					public int compareTo(Author a) {
						int last = this.lastName.compareTo(a.lastName);
						return last == 0 ? this.firstName.compareTo(a.firstName) : last;
					}

					@Override
					public String toString() {
						return "Author [firstName=" + firstName + ", lastName=" + lastName + ", bookName=" + bookName + "]";
					}
					
				}

				public class SortingCustomListObjectsByComparable {

					public static void main(String[] args) {
						List<Author>  authors = getAuthors();
						
						System.out.println("Before Sorting");
						authors.forEach(System.out::println);
						
						Collections.sort(authors);
						System.out.println("After Sorting");
						authors.forEach(System.out::println);

					}

					public static List<Author> getAuthors() {
						List<Author> al = new ArrayList<Author>();
						al.add(new Author("Henry", "Miller", "Tropic of Cancer"));
						al.add(new Author("Nalo", "Hopkinson", "Brown Girl in the Ring"));
						al.add(new Author("Frank", "Miller", "300"));
						al.add(new Author("Deborah", "Hopkinson", "Sky Boys"));
						al.add(new Author("George R. R.", "Martin", "Song of Ice and Fire"));
						return al;
					}

				}
				
			Output :
			
					Before Sorting :
				
					Author [firstName=Henry, lastName=Miller, bookName=Tropic of Cancer]
					Author [firstName=Nalo, lastName=Hopkinson, bookName=Brown Girl in the Ring]
					Author [firstName=Frank, lastName=Miller, bookName=300]
					Author [firstName=Deborah, lastName=Hopkinson, bookName=Sky Boys]
					Author [firstName=George R. R., lastName=Martin, bookName=Song of Ice and Fire]
					
					After Sorting
					
					Author [firstName=Deborah, lastName=Hopkinson, bookName=Sky Boys]
					Author [firstName=Nalo, lastName=Hopkinson, bookName=Brown Girl in the Ring]
					Author [firstName=George R. R., lastName=Martin, bookName=Song of Ice and Fire]
					Author [firstName=Frank, lastName=Miller, bookName=300]
					Author [firstName=Henry, lastName=Miller, bookName=Tropic of Cancer]

		2.	Comparator
		
			-	Comparator will be used for custom sorting at runtime

			Code Snippets :
			
				List<Author> authors = getAuthors();

				System.out.println("Before Sorting");
				authors.forEach(System.out::println);

				Collections.sort(authors, new Comparator<Author>() {

					@Override
					public int compare(Author a1, Author a2) {
						return a1.bookName.compareTo(a2.bookName);
					}

				});
				
				System.out.println("After Sorting");
				authors.forEach(System.out::println);
				
			Output :

				Before Sorting
				
				Author [firstName=Henry, lastName=Miller, bookName=Tropic of Cancer]
				Author [firstName=Nalo, lastName=Hopkinson, bookName=Brown Girl in the Ring]
				Author [firstName=Frank, lastName=Miller, bookName=300]
				Author [firstName=Deborah, lastName=Hopkinson, bookName=Sky Boys]
				Author [firstName=George R. R., lastName=Martin, bookName=Song of Ice and Fire]
				
				After Sorting
				
				Author [firstName=Frank, lastName=Miller, bookName=300]
				Author [firstName=Nalo, lastName=Hopkinson, bookName=Brown Girl in the Ring]
				Author [firstName=Deborah, lastName=Hopkinson, bookName=Sky Boys]
				Author [firstName=George R. R., lastName=Martin, bookName=Song of Ice and Fire]
				Author [firstName=Henry, lastName=Miller, bookName=Tropic of Cancer]
	
-	List of Strings
	
	1.	Implementing Comparator Interface	
	
		Code Snippets:
		
			public class ListStringSortingBeforeJava8 {

				public static void main(String[] args) {

					String[] stringArray = new String[] { "ffff", "rrr", "dddd", "aaaa", "bbbbbb" };
					List<String> stringList = Arrays.asList(stringArray);
					
					*Comparator<String> c = new StringComparator();*
					stringList.sort(c);
				}
			}
			
			class StringComparator implements Comparator<String> {
				public int compare(String s1, String s2) {
					return s1.compareTo(s2);
				}
			}
			
		Output :
				[aaaa, bbbbbb, dddd, ffff, rrr]		
			
	2.	Lambda Expression
	
		Code Snippets:

			public class ListStringSortingLambda {
				public static void main(String[] args) {
					String[] stringArray = new String[] { "ffff", "rrr", "dddd", "aaaa", "bbbbbb" };
					List<String> stringList = Arrays.asList(stringArray);

					Comparator<String> c = (s1, s2) -> s1.compareTo(s2);
					stringList.sort(c);
					System.out.println(stringList);
				}
			}
			
		Output :
			[aaaa, bbbbbb, dddd, ffff, rrr]						
			
	3.	Method Reference
	
		Code Snippets
		
			public class ListStringSortingMethodReference {
				public static void main(String[] args) {
					String[] stringArray = new String[] { "ffff", "rrr", "dddd", "aaaa", "bbbbbb" };
					List<String> stringList = Arrays.asList(stringArray);
					
					Comparator<String> stringComparator = new StringComparator();
					stringList.sort(stringComparator::compare);
					System.out.println(stringList);
				}
			}
			
		Output :
			[aaaa, bbbbbb, dddd, ffff, rrr]

-	List of Custom Objects
	
	
	1.	Implementing Comparator Interface using it on List.sort() method
	
		Code Snippets :
		
			class Hosting {
				private int id;
				private String name;
				private Long websites;
				
				//getters and setters
			}

			class HostingComparator implements Comparator<Hosting> {
				@Override
				public int compare(Hosting h1, Hosting h2) {
					return h1.getWebsites().compareTo(h2.getWebsites());
				}
			}

			public class SortingCustomObjectsUsingListSort {
				public static void main(String[] args) {
					List<Hosting> hostingList = getHostings();
					
					System.out.println("Before Sorting");
					System.out.println("=========================================================");
					hostingList.forEach(System.out::println);
					
					Comparator<Hosting> c = new HostingComparator();
					hostingList.sort(c);
					
					System.out.println("After Sorting");
					System.out.println("=========================================================");
					
					hostingList.forEach(System.out::println);
				}
				public static List<Hosting> getHostings() {
					return Arrays.asList(new Hosting(1, "aws.amazon.com", 200000l), new Hosting(2, "linode.com", 9999l),
							new Hosting(4, "heroku.com", 4444001l), new Hosting(3, "digitalocean.com", 109903l));
				}
			}
		
		Output :
			*Before Sorting
			=========================================================
			Hosting [id=1, name=aws.amazon.com, websites=200000]
			Hosting [id=2, name=linode.com, websites=9999]
			Hosting [id=4, name=heroku.com, websites=4444001]
			Hosting [id=3, name=digitalocean.com, websites=109903]
			After Sorting
			=========================================================
			Hosting [id=2, name=linode.com, websites=9999]
			Hosting [id=3, name=digitalocean.com, websites=109903]
			Hosting [id=1, name=aws.amazon.com, websites=200000]
			Hosting [id=4, name=heroku.com, websites=4444001]*
			
			
	2.	Using Lambda Expression of Comparator Impl on List.sort() method
	
		Code Snippets :
			
				Comparator<Hosting> c = (h1,h2) -> h1.getWebsites().compareTo(h2.getWebsites());
				hostingList.sort(c);
				hostingList.forEach(System.out::println);
			
	3.	Using Method Reference of Comparator Impl on List.sort() method
		
		Code Snippets :
			
				HostingComparator hc = new HostingComparator();
				Comparator<Hosting> c = hc::compare;
				hostingList.sort(c);
				hostingList.forEach(System.out::println);
				
	4.	Using Lambda Expression of Custom Comparator on List.stream().sorted() 
		
		-	If we use stream it will not modify the existing source or list
		-	After sorting we call the terminal stream operation and collect it to new List 
		
		Code Snippets :
		
			Comparator<Hosting> c = new HostingComparator();
			List<Hosting> sortedHostingList = hostingList.stream().sorted(c).collect(Collectors.toList());
			sortedHostingList.forEach(System.out::println);
			
			
	6.	Using Comparator Lambda Expression and Method Expression on List.stream().sorted()

		Code Snippets :
			
			1. Method Reference
			
				List<Hosting> sortByWebsites = hostingList.stream().sorted(Comparator.comparingLong(Hosting::getWebsites)).collect(Collectors.toList());
				List<Hosting> sortById = hostingList.stream().sorted(Comparator.comparingInt(Hosting::getId)).collect(Collectors.toList());
				List<Hosting> sortByName = hostingList.stream().sorted(Comparator.comparing(Hosting::getName)).collect(Collectors.toList());
			
			2. Lambda Expression
			
				List<Hosting> sortByWebsites = hostingList.stream().sorted(Comparator.comparingLong(h -> h.getWebsites())).collect(Collectors.toList());
				List<Hosting> sortById = hostingList.stream().sorted(Comparator.comparingInt(h -> h.getId())).collect(Collectors.toList());
				List<Hosting> sortByName = hostingList.stream().sorted(Comparator.comparing(h -> h.getName())).collect(Collectors.toList());
			
	
	8.	Sorting Reverse Order using Lambda Expression and Method Reference on List.stream().sorted()
	
		Code Snippets :
			
			1. Method Reference
			
				List<Hosting> sortByWebsites = hostingList.stream().sorted(Comparator.comparingLong(Hosting::getWebsites).reversed()).collect(Collectors.toList());
				List<Hosting> sortById = hostingList.stream().sorted(Comparator.comparingInt(Hosting::getId).reversed()).collect(Collectors.toList());
				List<Hosting> sortByName = hostingList.stream().sorted(Comparator.comparing(Hosting::getName).reversed()).collect(Collectors.toList());
			
			2. Lambda Expression
			
				List<Hosting> sortByWebsites = hostingList.stream().sorted(Comparator.comparingLong(h -> h.getWebsites())).collect(Collectors.toList());
				List<Hosting> sortById = hostingList.stream().sorted(Comparator.comparingInt(h -> h.getId()).reversed()).collect(Collectors.toList());
				List<Hosting> sortByName = hostingList.stream().sorted(Comparator.comparing(h -> h.getName()).reversed()).collect(Collectors.toList());
				
----------------------------------------------------------------------------------------
## Sorting Set Data Structure


	-	Set impl HashSet doesn't allow duplicates and doesn't follow insertion order by default
	-	HashSet doesn't allow null values 
		
		-	Throws NullPointerException if any 
	
	-	Set interface doesn't have sort method 
	-	In-order to sort Set we can use TreeSet(SortedSet) or Convert it to list and then Sort ... return back as Set
		
### TreeSet and Sorted Set 
	
		-	SortedSet is a Set which follows some sort of ordering on its elements
		-	Elements in SortedSet can be sorted in natural order using Comparable interface or in any custom order with Comparator interface
		- 	TreeSet is a implementation of SortedSet interface
		
		Sorting using SortedSet and TreeSet
			
			-	SortedSet should know how to sort its elements as they are being added by using Comparable or Comparator interfaces
			-	If elements implement Comparable interface then SortedSet will use compareTo() method to sort elements
				-	This is called sorting in natural order
			-	We can pass Comparator to do custom sorting
			-	If Comparator is passed then it will ignore Comparable and uses Comparator
			-	TreeSet is a implementation of SortedSet interface

-	Sorting Set of Strings before Java 8
	
	1. Using Collections.sort() method to Sort Set
	
		Convert Set to List and use Collections.sort() method
		
	
		1.	Natural Order:
		
			Code Snippets :	
			
				String[] stringArray = new String[] { "dddd", "hhhh", "eeee", "bbbb", "pppp", "aaaa" };
				Set<String> stringSet = new HashSet<>(Arrays.asList(stringArray););
				System.out.println("Set before sorting" + stringSet);
				List<String> stringArrayList = new ArrayList<>(stringSet);
				Collections.sort(stringArrayList);
				System.out.println("Set After Sorting" + stringArrayList);
			
			Output :
			
				Set Before sorting[hhhh, pppp, aaaa, dddd, bbbb, eeee]
				Set After Sorting[aaaa, bbbb, dddd, eeee, hhhh, pppp]
		
		2.	Reverse Order by providing custom Comparator
		
			Code Snippets :
			
				String[] stringArray = new String[] { "dddd", "hhhh", "eeee", "bbbb", "pppp", "aaaa" };
				Set<String> stringSet = new HashSet<>(Arrays.asList(stringArray));
				System.out.println("Set before sorting" + stringSet);
				List<String> stringArrayList = new ArrayList<>(stringSet);
				
				Comparator<String> c = new Comparator<String>() {
					public int compare(String s1, String s2) {
						return s2.compareTo(s1);
					}
				};
				
				Collections.sort(stringArrayList, c);
				System.out.println("Set After Sorting" + stringArrayList);
				
			Output :

				Set Before sorting[hhhh, pppp, aaaa, dddd, bbbb, eeee]
				Set After Sorting[pppp, hhhh, eeee, dddd, bbbb, aaaa]

	2.	Using SortedSet to Sort Set
	
		String implements Comparable Interface and compareTo() method so SortedSet will sort in natural order using CompareTo() method
		
		1. Natural Order:

			Code Snippets :
			
				String[] stringArray = new String[] { "dddd", "hhhh", "eeee", "bbbb", "pppp", "aaaa" };
				List<String> stringList = Arrays.asList(stringArray);
				System.out.println("Set Before sorting" + stringList);
				Set<String> sortedSet = new TreeSet<>(stringList);
				System.out.println("Set After sorting" + sortedSet);
				
			Output :
				
					Set Before sorting[dddd, hhhh, eeee, bbbb, pppp, aaaa]
					Set After Sorting[aaaa, bbbb, dddd, eeee, hhhh, pppp]
					
		2.	Custom Order using Comparator
		
			Code Snippets :
			
				String[] stringArray = new String[] { "dddd", "hhhh", "eeee", "bbbb", "pppp", "aaaa" };
				List<String> stringList = Arrays.asList(stringArray);
				System.out.println("Set Before sorting" + stringList);
				Comparator<String> c = new Comparator<String>() {
					@Override
					public int compare(String s1, String s2) {
						return s2.compareTo(s1);
					}
				};
				Set<String> sortedSet = new TreeSet<>(c);
				sortedSet.addAll(stringList);
				System.out.println("Set After sorting" + sortedSet);
				
			Output :

				Set Before sorting[hhhh, pppp, aaaa, dddd, bbbb, eeee]
				Set After Sorting[pppp, hhhh, eeee, dddd, bbbb, aaaa]	
		
-	Sorting Set of Custom Objects

	
-	Sorting Set of Custom and String in a particular order
-	Sorting Set using Comparator
-	Sorting Set in Java 8

----------------------------------------------------------------------------------------
			
## 2.	forEach() method of Iterable Interface

	-	forEach() is a terminal method in Java Functional Programming
	-	forEach() method is added to Iterable Interface from Java 8 
			
		-	Collection interface extends Iterable interface
		-	Will be available for all the Collection Impl Classes
		
	
	1.	Map Interface 
	
		- 	forEach() of Map will take BiConsumer Functional Interface as an argument
		-	BiConsumer - will take two input params
		-	Skeleton of BiConsumer FunctionalInterface

				@FunctionalInterface
				public interface BiConsumer<T, U> {
					
					void accept(T t, U u);
				}
						
			
	2. List Interface
	
		-	forEach() method will take the Consumer FunctionalInterface as an argument 
		-	Consumer Interface skeleton
			
				@FunctionalInterface
				public interface Consumer<T> {
					void accept(T t);
				}
			
	
## 3.	filter() method of Stream Interface

	-	Stream API is added from Java 8 
	- 	filter() method is used to filter some the stream elements 
	-	filter() method take Predicate<T> as the input argument
	-	filter() method is intermediate method that will gets executed only when terminal method gets invoked
	-	filter will used commonly used along with collect() or findAny(), findAll(), anyMatch(), allMatch()
		
		
		Predicate<T>
		
			-	Predicate is a FunctionalInterface in Java 
			-	Predicate take one argument and returns boolean value
			
			-	Skeleton of Predicate interface
				
					@FunctionalInterface
					public interface Predicate<T> {
						
						public boolean test(T t);
					}
					
	-	Map -> filter()
		
		-	map.entrySet().stream().filter()
		-	filter() on Map Impl will Key-Value pair Entry as an argument
			
			Ex: 
				
				1.
					
					a).	Return String :	
							map.entrySet().stream().filter(m -> m.getValue().equals("string")).map(m -> m.getValue()).collect(Collectors.joining(",")); //returns String
							map.entrySet().stream().filter(m -> m.getValue().equals("string")).map(Map.Entry::getValue).collect(Collectors.joining(",")); //returns String
						
					b).	Return Map :
							map.entrySet().stream().filter(m -> m.getValue().equals("string"))
							.map(m -> m.getValue()).collect(Collectors.toMap(Map.Entry::getKey,Map.Entry::getValue));
							
							map.entrySet().stream().filter(m -> !m.getValue().equals("linode.com"))
							.sorted(Map.Entry.comparingByValue(Comparator.reverseOrder()))
							.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue, (o, n) -> n, LinkedHashMap::new))
							.entrySet().stream().forEach(System.out::println);
				
							
				2. Using Predicate
					
					a).							
						Predicate<String> p = s -> s.equals("");
						map.entrySet().stream().filter(m -> p.test(m.getValue()))
						.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue));
		
					b).	
						private static Map<Integer, String> someMap;
						
						public static <K, V> Map<K, V> filterByValue(Map<K, V> map, Predicate<V> p) {
							 return map.entrySet().stream().filter(m -> p.test(m.getValue()))
									.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue));
						}
						
						Map<Integer, String> map1 = filterByValue(someMap, s -> s.contains("someString"));
						Map<Integer, String> map2 = filterByValue(someMap, s -> s.length()>3);
		
----------------------------------------------------------------------------------------

## 4.	collect() 

	-	collect() method is a terminal method ---> means it closed the stream and returns the values other than Stream 
	- 	Values can List, Set or Map
	-	collect() method takes Collectors as an argument that returns set, map, count, sum of elements
	
----------------------------------------------------------------------------------------
	
## 5.	findAny() and orElse

	-	findAny() method can be used once we filtered some of the values 
	-	findAny() returns Optional<T> Object that may or not can contain the value
	-	Optional is best way to avoid the null pointer exception
	-	orElse() will get executed and returns default value
	
----------------------------------------------------------------------------------------
## 6.	Stream Collectors groupingBy Examples

	##	https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html

	1.	Group, Sort, Count values of List 
	
		-	To group and count we need to use Collectors methods like
		
			-	Collectors.groupingBy()
			
			




----------------------------------------------------------------------------------------

## 16.	 String Joiner and String.join()

	1. StringJoiner
	
		-  	StringJoiner is a Class added in Java 8
		-	StringJoiner is a final class
		-	StringJoiner is used to join the strings
		-	StringJoiner has overloaded constructors 
			
				-	Takes delimiter and another takes prefix and suffix	 
					
					public StringJoiner(CharSequence delimiter) {}
					public StringJoiner(CharSequence delimiter, CharSequence prefix, CharSequence suffix){}
				
				-	StingJoiner.add("string")
					
					-	add() method is used to add the strings
		-	Code Samples

			StringJoiner sj = new StringJoiner(",");
				sj = new StringJoiner("|","Prefix- ", " -Suffix");
				sj = new StringJoiner("|","{", "}");
				sj.add("dddd");
				sj.add("eeee");
				sj.add("ffff");
				sj.add("gggg");
				sj.add("ooooooooo");
			sj = new StringJoiner("|","Prefix- ", " -Suffix");
			sj = new StringJoiner("|","{", "}");		
	
	2.	String.join()

		-	Added since Java 8
		-	join() method takes delimiter and elements as the params
		-	join() is a static method and has two overloaded methods
		-	Takes CharSequence or Collection Impl
		
			public static String join(CharSequence delimiter, CharSequence ... elements)
			public static String join(CharSequence delimiter, Iterable<? extends CharSequence> elements)
			
			
	3.	Collectors.joining()
	
		-	Collectors.joining() will takes the delimiter, prefix and suffix
		-	Returns string with joined the set of strings
		
		- 	joining() method of Collectors interface have 3 overloaded methods 
				
			public static Collector<CharSequence, ?, String> joining(CharSequence delimiter)
			public static Collector<CharSequence, ?, String> joining(CharSequence delimiter, CharSequence prefix, CharSequence suffix)
		
----------------------------------------------------------------------------------------

## 17.	 Stream File Reader		
		
		- 	From Java 8 we can read file as Streams using Files.lines(Paths.get())
			
			String fileName = "c://lines.txt";
			try (Stream<String> stream = Files.lines(Paths.get(fileName))) {
				stream.forEach(System.out::println);
			} catch (IOException e) {
				e.printStackTrace();
			}
		
		-	Using Stream + BufferedReader
			try (BufferedReader br = Files.newBufferedReader(Paths.get(fileName))) {
				list = br.lines().collect(Collectors.toList());
			} catch (IOException e) {
				e.printStackTrace();
			}		
			
		
	
----------------------------------------------------------------------------------------		

## 18. Join Arrays
	
	1. 	How to join two Arrays
	
		-	Arrays can be joined by below ways
		
			1.	Apache Common Lang - ArrayUtils
			2.	Java API
			3.	Java 8 Stream
			
			
			1. Apache Common Lang
			
				-	Apache Common Lang is utility jar that contains lots of utility classes and method that be used to write bug free code
				
				a.	ArrayUtils
					
					ArrayUtil.addAll() method can be used to add merge the two or more arrays
				
				<dependency>
					<groupId>org.apache.commons</groupId>
					<artifactId>commons-lang3</artifactId>
					<version>3.4</version>
				</dependency>
					
			2. 	Java 8 Stream
								
				Sting[] - String Array
					String [] result = Stream.of(s1, s2, s3).flatMap(Stream::of).toArray(String[]::new);
		
				int[] - int Array
					int[] result2 = IntStream.concat(Arrays.stream(int1), Arrays.stream(int2)).toArray();
					int[] result3 = IntStream.concat(Arrays.stream(int1), 
					IntStream.concat(Arrays.stream(int2), Arrays.stream(int3))).toArray();
				
	
----------------------------------------------------------------------------------------	
	
	
## 19.	 String to Stream of Char Array
	
		-	Converting String to Char Array
		
			Char[] charArray = "String".toCharArray();
			
		-	Java 8
		
			Stream<Character> streamChar =   "StringToChar".char().mapToObj(c -> (char)c);
	
	
	
	
	
	
	
	
----------------------------------------------------------------------------------------	
	
	
	
	
	
