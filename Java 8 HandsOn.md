

# Java 8 Examples of Lambda Expressions

## Stream API

-	Stream operations are mainly divided into two categories : 
	
	-	Intermediate operations
	-	Terminal operations
	
- 	Intermediate operations such as filter() or map() returns a new Stream
-	While terminal operations such as Stream.forEach() produce a result or side effect
-	After the terminal operation, the stream pipeline is considered consumed, and can no longer be used
-   Intermediate operations are also of two types:
	
	-	Stateless
	-	Stateful
	
-	As name suggests, Stateless operations doesn't retain any state from previously processed element
-	filter() and map() are two examples of stateless intermediate operation
-	On the other hand distinct() and sorted() are example of Stateful operations	
- 	Which can have state from previously processed elements, while processing new elements




	
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
	
```java
	transactions.stream
		.mapToInt(Transaction::getValue)
		.sum();
````		
	-	Numeric Range using NumericStreams
		
		-	range and rangeClosed
```java		
	IntStream i = IntStream.rangeClosed(10,20)
					.filter(i -> i%5==0);
````
						
9.	Building Stream
		
	-	We can create a stream from file or collection, array or even numbers
	-	Stream.of and Arrays.stream() can be used to build the stream out of array			
		
----------------------------------------------------------------------------------------

# Contents 

#### 1.	  Comparator
#### 2.	  forEach
#### 3.	  filter
#### 4.	  collect
#### 5.	  findAny or orElse
#### 6.	  Collectors.groupingBy
#### 7.	  Convert Stream to List
#### 8.	  Array to Stream
#### 9.	  Stream is already Operated upon and Supplier
#### 10.  Sort a Map
#### 11.  List to Map
#### 12.  Filter a Map
#### 13.  flatMap
#### 14.  convert Map to List
#### 15.  Optional in Depth
#### 16.  String Joiner
#### 17.  Stream File Reader
#### 18.  Join Arrays
#### 19.  String to Char Array
#### 20.  Covert Primitive Primitive array to List
      
----------------------------------------------------------------------------------------
		
## 1. Comparator to sort the List Custom Objects

- 	List is a Data Structure
-	Custom Objects with few fields

### Sorting Techniques
	
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
		
```java

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

````
						
			
###	Code Snippets of Comparator


### 1.	Using Collections.sort() 
	
####	1.	Sorting String List in Natural Order
	
#####		Code Snippets:
```java		
			public class SortingUsingCollectionsSort {
				public static void main(String[] args) {
					String[] stringArray = new String[] { "ffff", "rrr", "dddd", "aaaa", "bbbbbb" };
					List<String> stringList = Arrays.asList(stringArray);
					
					Collections.sort(stringList);
				}
			}
````
			
#####		Output :
				[aaaa, bbbbbb, dddd, ffff, rrr]				

		
####	2.	Sorting String List In Reverse Order
			
#####		Code Snippets:

```java		
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
````
			
#####		Output :
				[rrr, ffff, dddd, bbbbbb, aaaa]


####	3.	Sorting Custom Objects
	
		Custom Objects can be sorted with Collections.sort() by implementing Comparable and Comparator Interface
		
1.	Comparable
	
	-	Comparable will be used at class declaration
	-	Something like default sorting 
	-	Below is the example used to sort by last name ... if both last name are same then will use first name as criteria for sorting
	
			
#####			Code Snippets :	

```java			
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
````
				
#####			Output :
			
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

#####			Code Snippets :

```java
			
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
````	
				
#####			Output :

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



### 2.	List of Strings
	
####	1.	Implementing Comparator Interface	
	
#####		Code Snippets:
		
```java
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
````
		
#####		Output :
				[aaaa, bbbbbb, dddd, ffff, rrr]		
				
####	2.	Lambda Expression
	
#####		Code Snippets:
```java
			public class ListStringSortingLambda {
				public static void main(String[] args) {
					String[] stringArray = new String[] { "ffff", "rrr", "dddd", "aaaa", "bbbbbb" };
					List<String> stringList = Arrays.asList(stringArray);

					Comparator<String> c = (s1, s2) -> s1.compareTo(s2);
					stringList.sort(c);
					System.out.println(stringList);
				}
			}
````			
#####		Output :
			[aaaa, bbbbbb, dddd, ffff, rrr]						
			
####	3.	Method Reference
	
#####		Code Snippets
```java		
			public class ListStringSortingMethodReference {
				public static void main(String[] args) {
					String[] stringArray = new String[] { "ffff", "rrr", "dddd", "aaaa", "bbbbbb" };
					List<String> stringList = Arrays.asList(stringArray);
					
					Comparator<String> stringComparator = new StringComparator();
					stringList.sort(stringComparator::compare);
					System.out.println(stringList);
				}
			}
````			
#####		Output :
			[aaaa, bbbbbb, dddd, ffff, rrr]

### 3.	List of Custom Objects
	
	
####	1.	Implementing Comparator Interface using it on List.sort() method
	
#####		Code Snippets :
```java		
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
````
		
#####		Output :
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
			
			
####	2.	Using Lambda Expression of Comparator Impl on List.sort() method
	
#####		Code Snippets :
```java			
				Comparator<Hosting> c = (h1,h2) -> h1.getWebsites().compareTo(h2.getWebsites());
				hostingList.sort(c);
				hostingList.forEach(System.out::println);
````			
####	3.	Using Method Reference of Comparator Impl on List.sort() method
		
#####		Code Snippets :
```java			
				HostingComparator hc = new HostingComparator();
				Comparator<Hosting> c = hc::compare;
				hostingList.sort(c);
				hostingList.forEach(System.out::println);
````	
			
####	4.	Using Lambda Expression of Custom Comparator on List.stream().sorted() 
		
-	If we use stream it will not modify the existing source or list
-	After sorting we call the terminal stream operation and collect it to new List 

#####		Code Snippets :
```java		
			Comparator<Hosting> c = new HostingComparator();
			List<Hosting> sortedHostingList = hostingList.stream().sorted(c).collect(Collectors.toList());
			sortedHostingList.forEach(System.out::println);
````			
			
####	5.	Using Comparator Lambda Expression and Method Expression on List.stream().sorted()

#####		Code Snippets :
			
1. Method Reference
```java			
				List<Hosting> sortByWebsites = hostingList.stream().sorted(Comparator.comparingLong(Hosting::getWebsites)).collect(Collectors.toList());
				List<Hosting> sortById = hostingList.stream().sorted(Comparator.comparingInt(Hosting::getId)).collect(Collectors.toList());
				List<Hosting> sortByName = hostingList.stream().sorted(Comparator.comparing(Hosting::getName)).collect(Collectors.toList());
````			
2. Lambda Expression
```java			
				List<Hosting> sortByWebsites = hostingList.stream().sorted(Comparator.comparingLong(h -> h.getWebsites())).collect(Collectors.toList());
				List<Hosting> sortById = hostingList.stream().sorted(Comparator.comparingInt(h -> h.getId())).collect(Collectors.toList());
				List<Hosting> sortByName = hostingList.stream().sorted(Comparator.comparing(h -> h.getName())).collect(Collectors.toList());
````			
	
####	6.	Sorting Reverse Order using Lambda Expression and Method Reference on List.stream().sorted()
	
#####		Code Snippets :
			
1. Method Reference
```java			
				List<Hosting> sortByWebsites = hostingList.stream().sorted(Comparator.comparingLong(Hosting::getWebsites).reversed()).collect(Collectors.toList());
				List<Hosting> sortById = hostingList.stream().sorted(Comparator.comparingInt(Hosting::getId).reversed()).collect(Collectors.toList());
				List<Hosting> sortByName = hostingList.stream().sorted(Comparator.comparing(Hosting::getName).reversed()).collect(Collectors.toList());
````			
2. Lambda Expression
```java			
				List<Hosting> sortByWebsites = hostingList.stream().sorted(Comparator.comparingLong(h -> h.getWebsites())).collect(Collectors.toList());
				List<Hosting> sortById = hostingList.stream().sorted(Comparator.comparingInt(h -> h.getId()).reversed()).collect(Collectors.toList());
				List<Hosting> sortByName = hostingList.stream().sorted(Comparator.comparing(h -> h.getName()).reversed()).collect(Collectors.toList());
````				
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

	2.	Using SortedSet to Sort Set of Strings
	
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
		
	
	
	3.	Sorting Set of Custom Objects
		
		1.	Collections.sort() 
			
			1.Comparable
			
				1.	Implement POJO with Comparable Interface
				
					public class Author implements Comparable<Author> {
						
						public String firstName;
						public String lastName;
						public String book;
									
							Getters and Setters	
						
						public int compareTo(Author author) {
							return this.firstName.compareTo(author.firstName);
						} 
					}
					
				2.	Convert Set to List
					
					List<Author> authorList = new ArrayList<Author>(SetImpl);
					Collections.sort(authorList);
					
					
				3.	Convert List to Set if required	
					
					Set<Author> authorSet = new ArrayList<>(authorList);
				
			2.	Comparator
			
				1.	Convert Set to List 
					
						Set<Author> authorSet =  new HashSet<>(); 
						authorSet.add(new Author("aa","dd","bbb"));
						authorSet.add(new Author("aa","dd","bbb"));
						
					
						List<Author> authorList = new ArrayList<>(authorSet);
					
				2.	Sort using Collections.sort(listIml, Comparator)
					
						Comparator c = new Comparator<Author>() {
						
							public int compare(Author a1, Author a2){
								int lastName = a1.lastName.compareTo(a2.lastName);
								return lastName == 0 ? a1.firstName.compareTo(a2.firstName) : lastName;
							}
						}	
						Collections.sort(authorList, c );
				
				3.	Convert List to Set if required	
					
					Set<Author> authorSet = new ArrayList<>(authorList);
					
					
		2.	SortedSet or TreeSet
			
			1.Comparable with TreeSet
			
				1.	Implement POJO with Comparable Interface
						
						class Author implements Comparable<Author> {
							private String firstName;
							private String lastName;
							private String book;

							public int compareTo(Author author) {
								int last = this.lastName.compareTo(author.lastName);
								return last == 0 ? this.firstName.compareTo(author.firstName) : last;
							}

							public Author(String firstName, String lastName, String book) {
								super();
								this.firstName = firstName;
								this.lastName = lastName;
								this.book = book;
							}

							@Override
							public String toString() {
								return "Author [firstName=" + firstName + ", lastName=" + lastName + ", book=" + book + "]";
							}
						}
					
				2.	Items added to TreeSet will be sorted based on the compareTo() method of Comparable Interface
				
						Code Snippets : 
						
							public class SortedSetCustomObjectsComparable {
								public static void main(String[] args) {
									List<Author> authorsList = getAuthors();
									
									System.out.println("Before Sorting.......");
									authorsList.forEach(System.out::println);
									
									Set<Author> authorSet = new TreeSet<>(authorsList); // Adding unsorted List to TreeSet
									System.out.println("After Sorting.......");
									authorSet.forEach(System.out::println);
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
						
							Before Sorting.......
							Author [firstName=Henry, lastName=Miller, book=Tropic of Cancer]
							Author [firstName=Nalo, lastName=Hopkinson, book=Brown Girl in the Ring]
							Author [firstName=Frank, lastName=Miller, book=300]
							Author [firstName=Deborah, lastName=Hopkinson, book=Sky Boys]
							Author [firstName=George R. R., lastName=Martin, book=Song of Ice and Fire]
							After Sorting.......
							Author [firstName=Deborah, lastName=Hopkinson, book=Sky Boys]
							Author [firstName=Nalo, lastName=Hopkinson, book=Brown Girl in the Ring]
							Author [firstName=George R. R., lastName=Martin, book=Song of Ice and Fire]
							Author [firstName=Frank, lastName=Miller, book=300]
							Author [firstName=Henry, lastName=Miller, book=Tropic of Cancer]



			2.	Comparator with TreeSet
				
				1.	Create TreeSet Instance by providing Comparator Impl
				2.	Added elements will be sorted according to Comparator 
				2.	Or add all the elements in to TreeSet ... it will sorted according to Comparator
				
				Code Snippets : 
					
					public class SortedSetCustomObjectsComparator {
						public static void main(String[] args) {

							Comparator<Author> authorComparator = new Comparator<Author>() {

								public int compare(Author a1, Author a2) {
									int lastName = a1.lastName.compareTo(a2.lastName);
									return lastName == 0 ? a1.firstName.compareTo(a2.firstName) : lastName;
								}
							};

							Set<Author> authorSet = new TreeSet<>(authorComparator);

							authorSet.add(new Author("Henry", "Miller", "Tropic of Cancer"));
							authorSet.add(new Author("Nalo", "Hopkinson", "Brown Girl in the Ring"));
							authorSet.add(new Author("Frank", "Miller", "300"));
							authorSet.add(new Author("Deborah", "Hopkinson", "Sky Boys"));
							authorSet.add(new Author("George R. R.", "Martin", "Song of Ice and Fire"));
							
							authorSet.forEach(System.out::println);
						}
					}
				
				Output :
					
					Author [firstName=Deborah, lastName=Hopkinson, book=Sky Boys]
					Author [firstName=Nalo, lastName=Hopkinson, book=Brown Girl in the Ring]
					Author [firstName=George R. R., lastName=Martin, book=Song of Ice and Fire]
					Author [firstName=Frank, lastName=Miller, book=300]
					Author [firstName=Henry, lastName=Miller, book=Tropic of Cancer]

	

	4.	Sorting Set in Java 8
	
		1.	Comparator
		
			
			1. Integer or Wrapper class implements Comparable Interface
			
			    Code  Snippets  :		
				
					Integer[] intArray = new Integer[] { 9, 88, 423, 3, 345, 5, 1, 789, 7, 8 };
					Set<Integer> integerSet = new HashSet<>(Arrays.asList(intArray));
					System.out.println("Before Sorting Set ...");
					integerSet.forEach(System.out::println);
					/*
					 * Sorting Set in Java 8
					 */
					Set<Integer> sortedIntegerSet = integerSet.stream().sorted(Comparator.naturalOrder())
							.collect(Collectors.toCollection(LinkedHashSet::new));
							
					Set<Integer> sortedIntegerSet = integerSet.stream().sorted(Comparator.reverseOrder())
							.collect(Collectors.toCollection(LinkedHashSet::new));
							
					
					/*
					 * Sorting Set in Java 8 using Comparator comparing() methods
					 */
					
					Set<Integer> sortedIntegerSet = integerSet.stream().sorted(Comparator.comparing(Integer::intValue))
					.collect(Collectors.toCollection(LinkedHashSet::new));	

					Set<Integer> sortedIntegerSet = integerSet.stream().sorted(Comparator.comparingInt(Integer::intValue))
						.collect(Collectors.toCollection(LinkedHashSet::new));	
					
					Set<Integer> sortedIntegerSet = integerSet.stream().sorted(Comparator.comparingInt(Integer::intValue).reversed())
						.collect(Collectors.toCollection(LinkedHashSet::new));						
						
					System.out.println("After Sorting Set ...");
					sortedIntegerSet.forEach(System.out::println);
					
				Output :
					
					Natural Order :
						Before Sorting Set ...[1, 3, 5, 789, 423, 7, 88, 8, 9, 345]
						After Sorting Set ...[1, 3, 5, 7, 8, 9, 88, 345, 423, 789]
						
					Reverse Order :
						Before Sorting Set ...[1, 3, 5, 789, 423, 7, 88, 8, 9, 345]
						After Sorting Set ...[789, 423, 345, 88, 9, 8, 7, 5, 3, 1]

			2.	Strings
			
					Code Snippet 1 :
					
						String[] stringArray = new String[] { "ffff", "rrr", "dddd", "aaaa", "bbbbbb" };
	
						Set<String> stringSet = new HashSet<>(Arrays.asList(stringArray));
						List<String> sortedList = stringSet.stream().sorted((s1,s2)-> s2.compareTo(s1)).collect(Collectors.toList());
						System.out.println(sortedList);
					
					
					Output :
						
						Before Sorting : [rrr, aaaa, bbbbbb, dddd, ffff]
						After Sorting : [rrr, ffff, dddd, bbbbbb, aaaa]
						
						
					Code Snippet 2 : TreeSet allowing Null values
					
						Set<String> stringSet = new TreeSet<>(Comparator.nullsFirst(Comparator.reverseOrder()));
						stringSet.add(null);
						stringSet.add("node");
						stringSet.add("Aws");
						stringSet.add("Angular");
						stringSet.add("mongo");
						stringSet.add("camel");
						stringSet.add(null);
						
						System.out.println(stringSet);
								
					Output :
						
						[null, node, mongo, camel, Aws, Angular]
						
						


			3.	Custom Objects
			
				DataSource :
				
					public static Set<Person> getPersons() {
						return new HashSet<>(Arrays.asList(
							null, 
							new Person("hhhh", 45, 7880.09),
							new Person("qqq", 85, 0880.09),
							new Person("oooo", 95, 6880.09),
							new Person("zzzz", 15, 1880.09),
							new Person("aaa", 25, 3880.09),
							null,
							new Person("zzzz", 35, 7880.09),
							new Person("aaa", 15, 6880.09)));
					}
				Code Snippets :	
					
					Set<Person> sortedPersonsByName = personsSet.stream()
						.sorted(Comparator.nullsFirst(Comparator.comparing(Person::getName)))
						.collect(Collectors.toCollection(LinkedHashSet::new));
					
					Set<Person> sortedPersonsByName = personsSet.stream()
						.sorted(Comparator.nullsFirst(Comparator.comparing(Person::getAge)))
						.collect(Collectors.toCollection(LinkedHashSet::new));
				
					Set<Person> sortedPersonsByName = personsSet.stream()
						.sorted(Comparator.nullsFirst(Comparator.comparing(Person::getSalary)))
						.collect(Collectors.toCollection(LinkedHashSet::new));

					/*
					 * Reversed Order
					*/
					
					Set<Person> sortedPersonsByName = personsSet.stream()
						.sorted(Comparator.nullsFirst(Comparator.comparing(Person::getName)).reversed())
						.collect(Collectors.toCollection(LinkedHashSet::new));	
				Outputs:

					Natural Order :
						
						null
						Person [name=aaa, age=25, salary=3880.09]				
						Person [name=aaa, age=15, salary=6880.09]
						Person [name=hhhh, age=45, salary=7880.09]
						Person [name=oooo, age=95, salary=6880.09]
						Person [name=qqq, age=85, salary=880.09]
						Person [name=zzzz, age=35, salary=7880.09]
						Person [name=zzzz, age=15, salary=1880.09]

					Reversed Order :
				
						Person [name=zzzz, age=35, salary=7880.09]
						Person [name=zzzz, age=15, salary=1880.09]
						Person [name=qqq, age=85, salary=880.09]
						Person [name=oooo, age=95, salary=6880.09]
						Person [name=hhhh, age=45, salary=7880.09]
						Person [name=aaa, age=25, salary=3880.09]
						Person [name=aaa, age=15, salary=6880.09]
						null
					
					
			
		2.	SortedSet or TreeSet
		
		
			-	Sorted will not allow two Duplicated Custom Object
			-	null values are not allowed at all - duplicated null value will throw NullPointerException as TreeSet internally uses TreeMap
			-	Duplicates Objects will be removed
			-	Either Custom Object should implement Comparable interface or TreeSet Constructor should be passed with Comparator


			Code Snippets :
			
				Snippet 1 : Passing List to Sorted Set
					
					public class SortedSetSortingCustomSetObjectsJava8 {

						public static void main(String[] args) {
							Set<Person> personsSet = getPersons();
							personsSet.forEach(System.out::println);
						}
						
						// Null is not allowed TreeSet while using with Custom Objects
						//	Throws NullPointerException
						public static Set<Person> getPersons() {
							List<Person> personsList = Arrays.asList(null, new Person("hhhh", 45, 7880.09), new Person("qqq", 85, 0880.09),
									new Person("oooo", 95, 6880.09), new Person("zzzz", 15, 1880.09), new Person("aaa", 25, 3880.09),
									new Person("zzzz", 35, 7880.09), new Person("aaa", 15, 6880.09));
							
							Set<Person> personsSet = new TreeSet<>(Comparator.comparing(Person::getName));
							
							personsSet.addAll(personsList);
							
							return personsSet;
						}
					}
					
				Output :
				
					// After removing null values :
					//	Duplicate values are removed 
					
					Person [name=aaa, age=25, salary=3880.09]
					Person [name=hhhh, age=45, salary=7880.09]
					Person [name=oooo, age=95, salary=6880.09]
					Person [name=qqq, age=85, salary=880.09]
					Person [name=zzzz, age=15, salary=1880.09]	
					
				Snippet 2 : Adding elements to TreeSet by Passing Comparator to TreeSet Comparator
				
					Set<Person> personSet = new TreeSet<>(Comparator.comparingInt(Person::getAge));
					//personSet.add(null);
					personSet.add(new Person("hhhh", 45, 7880.09));
					personSet.add(new Person("qqq", 85, 0880.09));
					personSet.add(new Person("oooo", 95, 6880.09));
					personSet.add(new Person("zzzz", 35, 7880.09));
					personSet.add(new Person("aaa", 15, 6880.09));
					personSet.add(new Person("zzzz", 15, 1880.09));
					personSet.add(new Person("aaa", 25, 3880.09));
					//personSet.add(null);
					
					personSet.forEach(System.out::println);
							
		
				Output :
					
					Person [name=aaa, age=15, salary=6880.09]
					Person [name=aaa, age=25, salary=3880.09]
					Person [name=zzzz, age=35, salary=7880.09]
					Person [name=hhhh, age=45, salary=7880.09]
					Person [name=qqq, age=85, salary=880.09]
					Person [name=oooo, age=95, salary=6880.09]

----------------------------------------------------------------------------------------
## Sorting Map Data Structure

						
1. 	Sorting Map by Keys and Values with List and Collections.sort() before Java 8

	Code Snippet 1 :
	
		public class SortingHashMapCollections {

			public static void main(String[] args) {

				Map<String, Integer> map = new HashMap<>();

				map.put("cloths", 200);
				map.put("transportation", 300);
				map.put("grocery", 500);
				map.put("rent", 700);
				map.put("cell", 300);
				map.put("broadband", 600);
				map.put("electricity", 300);

				/*
				 * 
				 * Sorting by Keys in Of Type Comparable before Java 8
				 * Also using Comparator to sort in Reverse order
				*/
				//Comparator to Sort in Reverse Order
				Comparator<String> c = new Comparator<String>() {
					public int compare(String s1, String s2) {
						return s2.compareTo(s1);
					}
				};
				List<String> billsList = new ArrayList<>(map.keySet());
				Collections.sort(billsList);
				Collections.sort(billsList, c);
				billsList.sort(c);// Only one method of sort in the List Interface
				
				
				/*
				 * 
				 * Sorting by Values in Of Type Comparable before Java 8
				 * Also using Comparator to sort in Reverse order
				*/
				//Comparator to Sort in Reverse Order
						Comparator<Integer> ic = new Comparator<Integer>() {
							public int compare(Integer i1, Integer i2) {
								return i2.compareTo(i1);
							}
						};
				List<Integer> amountList = new ArrayList<>(map.values());
				Collections.sort(amountList);
				Collections.sort(amountList,ic);
				amountList.sort(ic);// Only one method of sort in the List Interface
				

			}
		}
		
		Output :
			[broadband, cell, cloths, electricity, grocery, rent, transportation]
			[transportation, rent, grocery, electricity, cloths, cell, broadband]
			[200, 300, 300, 300, 500, 600, 700]
			[700, 600, 500, 300, 300, 300, 200]

	Code Snippet 2 : 
		
		public class SortingHashMapEntyUsingList {
			public static void main(String[] args) {

				Map<String, Integer> map = new HashMap<>();

				map.put("cloths", 200);
				map.put("transportation", 300);
				map.put("grocery", 500);
				map.put("rent", 700);
				map.put("cell", 300);
				map.put("broadband", 600);
				map.put("electricity", 300);

				List<Map.Entry<String, Integer>> entrySetList = new ArrayList<Map.Entry<String, Integer>>(map.entrySet());
				
				Comparator<Map.Entry<String,Integer>> cK = new Comparator<Map.Entry<String,Integer>>() {
					public int compare(Map.Entry<String, Integer> m1, Map.Entry<String, Integer> m2) {
						return m2.getKey().compareTo(m1.getKey());
					}
				};
				
				Comparator<Map.Entry<String,Integer>> cV = new Comparator<Map.Entry<String,Integer>>() {
					public int compare(Map.Entry<String, Integer> m1, Map.Entry<String, Integer> m2) {
						return m2.getValue().compareTo(m1.getValue());
					}
				};
				
				//Collections.sort(entrySetList);// Gives Compilation Error that entryList is un-sortable ... we need provide Comparator impl
				Collections.sort(entrySetList, cK);
				entrySetList.sort(cK);// Should Provide Comparator
				
				Collections.sort(entrySetList, cV);
				entrySetList.sort(cV);// Should Provide Comparator
				
			}
		}

	
	Output :
		Sorting By Keys. Natural Order: [broadband=600, cell=300, cloths=200, electricity=300, grocery=500, rent=700, transportation=300]
		Sorting By Values. Natural Order: [cloths=200, cell=300, electricity=300, transportation=300, grocery=500, broadband=600, rent=700]
		Sorting By Keys. Reverse Order: [transportation=300, rent=700, grocery=500, electricity=300, cloths=200, cell=300, broadband=600]
		Sorting By Values. Reverse Order: [rent=700, broadband=600, grocery=500, transportation=300, electricity=300, cell=300, cloths=200]

2.	Sorting Map by TreeSet


		Code Snippet :
		
			public class SortingHashMapWithTreeSet {

				public static void main(String[] args) {
					Map<String, Integer> map = new HashMap<>();

					map.put("cloths", 200);
					map.put("transportation", 300);
					map.put("grocery", 500);
					map.put("rent", 700);
					map.put("cell", 300);
					map.put("broadband", 600);
					map.put("electricity", 300);
					map.put("electricity", 900);
					
					Set<String> set1 = new TreeSet<String>(map.keySet());
					Set<Integer> set2 = new TreeSet<Integer>(map.values());
					
					//Comparator to Sort in Reverse Order
					Comparator<String> comparator = new Comparator<String>() {
						public int compare(String s1, String s2) {
							return s2.compareTo(s1);
						}
					};
					
					// Using Comparator While Instanstiating TreeSet and Adding KeySet() ... like wise we can do for Values
					Set<String> set3 = new TreeSet<String>(comparator);
					set3.addAll(map.keySet());
					
					
					System.out.println("Sorting Keys in Natural Order :"+set1);
					System.out.println("Sorting Values in Natural Order :"+set2);
					System.out.println("Sorting Keys in Reverse Order :"+set3);
				}
			}

			
		Output :
		
			Sorting Keys in Natural Order :[broadband, cell, cloths, electricity, grocery, rent, transportation]
			Sorting Values in Natural Order :[200, 300, 500, 600, 700, 900]
			Sorting Keys in Reverse Order :[transportation, rent, grocery, electricity, cloths, cell, broadband]





3.  Sorting Map in Java 8
	
	
	Code Snippet :
	
		public class SortingHashMapInJava8 {

			public static void main(String[] args) {
				Map<String, Integer> map = new HashMap<>();

				map.put("cloths", 200);
				map.put("transportation", 300);
				map.put("grocery", 500);
				map.put("rent", 700);
				map.put("cell", 300);
				map.put("broadband", 600);
				map.put("electricity", 300);
				map.put("electricity", 300); //Duplicate
				//map.put(null, 800); // throws Null pointer Exception
	
				// Sorting By Keys
				LinkedHashMap<String, Integer> sortedMapByKeys = map.entrySet().stream()
						.sorted(Map.Entry.comparingByKey())
						.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue, (o, n) -> o, LinkedHashMap::new));
				
				// Sorting By Values
				LinkedHashMap<String, Integer> sortedMapByValues = map.entrySet().stream()
						.sorted(Map.Entry.comparingByValue())
						.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue, (o, n) -> o, LinkedHashMap::new));

				System.out.println("Keys: " + sortedMapByKeys);
				System.out.println("Values: " + sortedMapByValues);
				
				
				//Reversed Order
				LinkedHashMap<String, Integer> sortedMapByValuesReversed = map.entrySet().stream()
						.sorted(Map.Entry.<String, Integer>comparingByValue().reversed())
						.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue, (o, n) -> o, LinkedHashMap::new));
				System.out.println("Values Reversed: "+sortedMapByValuesReversed);
				
				//Using CustomComparator in comparingByKey
				LinkedHashMap<String, Integer> sortedMapByKeysUsingComparator = map.entrySet().stream()
						.sorted(Map.Entry.<String, Integer>comparingByKey((m1, m2)-> m2.compareTo(m1)))
						.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue, (o, n) -> o, LinkedHashMap::new));
				System.out.println("Comparator Revesed With Keys: "+sortedMapByKeysUsingComparator);
				
				
				//Using CustomComparator with sorted
				LinkedHashMap<String, Integer> sortedMapByValuesUsingComparator = map.entrySet().stream()
						.sorted((m1, m2)-> m2.getValue().compareTo(m1.getValue()))
						.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue, (o, n) -> o, LinkedHashMap::new));
				System.out.println("Comparator Revesed With Values: "+sortedMapByValuesUsingComparator);
			}
		}
	Output :
		
		Keys: {broadband=600, cell=300, cloths=200, electricity=300, grocery=500, rent=700, transportation=300}
		Values: {cloths=200, electricity=300, cell=300, transportation=300, grocery=500, broadband=600, rent=700}
		Values Reversed: {rent=700, broadband=600, grocery=500, electricity=300, cell=300, transportation=300, cloths=200}
		Comparator Revesed With Keys: {transportation=300, rent=700, grocery=500, electricity=300, cloths=200, cell=300, broadband=600}
		Comparator Revesed With Values: {rent=700, broadband=600, grocery=500, electricity=300, cell=300, transportation=300, cloths=200}



4.	Sorting Map with Custom Objects
	
	1.	Custom Objects as Values with Comparable and Comparator by Using Collections.sort() and List.sort()
		
		Code Snippet : 
		
			public class SortingHashMapWithCustomObjects {

				public static void main(String[] args) {

					Map<String, Worker> workerMap = new HashMap<>();
					workerMap.put("david", new Worker(3, "david", 5000d));
					workerMap.put("joy", new Worker(5, "joy", 1000d));
					workerMap.put("abel", new Worker(1, "abel", 3000d));
					workerMap.put("ruby", new Worker(4, "ruby", 7000d));
					System.out.println("Before Sorting: "+workerMap.values());
					
					//Converting Map Values to List to use with Collections.sort()
					List<Worker> workerList = new ArrayList<>(workerMap.values());
					Collections.sort(workerList);
					System.out.println("After Sorting: "+workerList);
					
					Collections.sort(workerList, comparatorById);
					workerList.sort(comparatorById); // List.sort(Comparator) //only one Method 
					System.out.println("Sorting By Id: "+workerList);
					
					//Sorting with TreeSet by using Comparable
					Set<Worker> workerSortedSet = new TreeSet<>(workerMap.values());
					System.out.println("Sorting With TreeSet: "+workerSortedSet);
				}

			}

			class Worker implements Comparable<Worker> {

				private Integer id;
				private String name;
				private Double salary;

				

				@Override
				public String toString() {
					return "Employee [id=" + id + ", name=" + name + ", salary=" + salary + "]";
				}

				public Worker(int id, String name, Double salary) {
					super();
					this.id = id;
					this.name = name;
					this.salary = salary;
				}
				
				//Providing Custom Implementation to Remove Duplicates of Custom Objects
				@Override
				public boolean equals(Object o) {
					Worker worker = (Worker) o;
					return this.id == worker.id && this.name.equals(worker.name) && this.salary.equals(worker.salary) ? true
							: false;
				}
				
				@Override
				public int hashCode() {
					return (int) (this.id + this.name.length()+  this.salary);
				}

				@Override
				public int compareTo(Worker w) {
					 return (int) (w.getSalary() - this.getSalary());
					// return (int) (w.getId() - this.getId());
					//return this.getName().compareTo(w.getName());
					
			//		if(w.getSalary() > this.getSalary()) {
			//			return -1;
			//		}else if(w.getSalary() < this.getSalary()){
			//			return 1;
			//		} else {
			//			return 0;
			//		}
				}
			}
	
				
		Output :
			
			Salary :
				Before Sorting: [Employee [id=5, name=joy, salary=1000.0], Employee [id=1, name=abel, salary=3000.0], Employee [id=3, name=david, salary=5000.0], Employee [id=4, name=ruby, salary=7000.0]]
				After Sorting: [Employee [id=4, name=ruby, salary=7000.0], Employee [id=3, name=david, salary=5000.0], Employee [id=1, name=abel, salary=3000.0], Employee [id=5, name=joy, salary=1000.0]]
				
			Id :
				Sorting By Id: [Employee [id=5, name=joy, salary=1000.0], Employee [id=4, name=ruby, salary=7000.0], Employee [id=3, name=david, salary=5000.0], Employee [id=1, name=abel, salary=3000.0]]
		
	2.	Custom Objects as Keys with Comparable and Comparator by Using Collections.sort() and List.sort()
	
		Code Snippet :
		
			public class SortingHashMapWithCustomObjectsKeys {

				public static void main(String[] args) {
					Map<Worker, Double> workerMap = new HashMap<>();

					workerMap.put(new Worker(3, "david", 5000d), 5000d);
					workerMap.put(new Worker(5, "joy", 1000d), 1000d);
					workerMap.put(new Worker(1, "abel", 3000d), 3000d);
					workerMap.put(new Worker(4, "ruby", 7000d), 7000d);
					workerMap.put(new Worker(4, "ruby", 7000d), 7000d);//Duplicate Key-Value
					
					List<Worker> list = new ArrayList<>(workerMap.keySet());
					Collections.sort(list);
					
					System.out.println(list);

				}

			}
	3.	Sorting Map with Custom Objects with Java 8

		Code Snippet :
		
			public class SortingHashMapWithCustomObjectsKeys {

				public static void main(String[] args) {
					Map<Worker, Double> workerMap = new HashMap<>();

					workerMap.put(new Worker(3, "david", 5000d), 5000d);
					workerMap.put(new Worker(5, "joy", 1000d), 1000d);
					workerMap.put(new Worker(1, "abel", 3000d), 3000d);
					workerMap.put(new Worker(4, "ruby", 7000d), 7000d);
					workerMap.put(new Worker(4, "ruby", 7000d), 7000d);//Duplicate
					workerMap.put(new Worker(5, null, 89d), null);// Throws NullPointerException
					
					
					// Sorting With Collections Sort
					List<Worker> list = new ArrayList<>(workerMap.keySet());
					Collections.sort(list);
					System.out.println(list);

					//Sorting By Key Reversed
					LinkedHashMap<Worker, Double> collect = workerMap.entrySet().stream()
							.sorted(Map.Entry.<Worker, Double>comparingByKey().reversed())
							.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue, (o, n) -> n, LinkedHashMap::new));
					System.err.println(collect);
					
					//Sorting By comparingByKey 
					LinkedHashMap<Worker, Double> collect1 = workerMap.entrySet().stream().sorted(Map.Entry.comparingByKey())
							.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue, (o, n) -> n, LinkedHashMap::new));
					
					//Sorting By comparingByValue 
					LinkedHashMap<Worker, Double> collect3 = workerMap.entrySet().stream().sorted(Map.Entry.comparingByValue())
							.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue, (o, n) -> n, LinkedHashMap::new));
					System.err.println(collect3);

					//Sorting By comparingByValue Reversed
					LinkedHashMap<Worker, Double> collect4 = workerMap.entrySet().stream()
							.sorted(Map.Entry.<Worker, Double>comparingByValue().reversed())
							.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue, (o, n) -> n, LinkedHashMap::new));
					System.out.println(collect4);
					
					//Sorting By Comparator
					LinkedHashMap<Worker, Double> collect5 = workerMap.entrySet().stream()
							.sorted((map1, map2) -> map1.getKey().getName().compareTo(map2.getKey().getName()))
							.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue, (o, n) -> n, LinkedHashMap::new));
					System.out.println(collect5);
				}
			}
			
		Output :

			[Employee [id=4, name=ruby, salary=7000.0], Employee [id=3, name=david, salary=5000.0], Employee [id=1, name=abel, salary=3000.0], Employee [id=5, name=joy, salary=1000.0]]
			{Employee [id=5, name=joy, salary=1000.0]=1000.0, Employee [id=1, name=abel, salary=3000.0]=3000.0, Employee [id=3, name=david, salary=5000.0]=5000.0, Employee [id=4, name=ruby, salary=7000.0]=7000.0}
			{Employee [id=5, name=joy, salary=1000.0]=1000.0, Employee [id=1, name=abel, salary=3000.0]=3000.0, Employee [id=3, name=david, salary=5000.0]=5000.0, Employee [id=4, name=ruby, salary=7000.0]=7000.0}
			{Employee [id=4, name=ruby, salary=7000.0]=7000.0, Employee [id=3, name=david, salary=5000.0]=5000.0, Employee [id=1, name=abel, salary=3000.0]=3000.0, Employee [id=5, name=joy, salary=1000.0]=1000.0}
			{Employee [id=1, name=abel, salary=3000.0]=3000.0, Employee [id=3, name=david, salary=5000.0]=5000.0, Employee [id=5, name=joy, salary=1000.0]=1000.0, Employee [id=4, name=ruby, salary=7000.0]=7000.0}

				
7.	Sorting Map with TreeMap

	1.	String and Wrappers
	
		Code Snippet : 
	
			public class SortingHashMapWithTreeMap {

				public static void main(String[] args) {

					Map<String, Integer> hashMap = new HashMap<>();
					hashMap.put("Java Developer", 3000);
					hashMap.put("Java Developer", 9000);
					hashMap.put("Java Developer", 33000);
					hashMap.put("Full Stack Developer", 15000);
					hashMap.put("Full Stack Developer", 55000);
					hashMap.put("Solutions Architect", 25000);
					hashMap.put("Data Scientist", 20000);
					hashMap.put("Data Scientist", 23000);// Duplicated Key

					
					// Sorting Key with TreeMap
					/*
						1.  String is of Type Comparable
						2.  Once adding the map with string as key it will sort in natural order
						3.  To change the order of sorting we needs to provide the Comparator for Key
						4.	  
					*/		
					System.out.println("Original Map with Duplicates ... Before Sorting: "+hashMap);
					Map<String, Integer> sortedMap = new TreeMap<>(hashMap);
					System.out.println("Sorting by Keys With TreeMap: "+sortedMap);
					
					
					// Sorting TreeMap Keys with Comparator 
					//Value sorting is not possible 
					Map<String, Integer> sortedMapComparator = new TreeMap<>((s1,s2)-> s2.compareTo(s1));
					sortedMapComparator.putAll(hashMap);
					System.out.println("Sorting by Keys in Reversed Order With TreeMap using Comparator: "+sortedMapComparator);
					
					
					// Value sorting is not possible with TreeMap
					/*
					 * 1. We sort the TreeMap or any with values using below Java 8 or 
					 * 2. By converting TreeMap or HashMap  into List<Map.Entry<K,V>> and then use 
					 * 	Collections.sort(list,Comparator) or List(Comparator) 
					 * 3. Provide Custom Comparator<Map.Entry<K,V>>		
					*/		
					LinkedHashMap<String, Integer> collect = hashMap.entrySet().stream().sorted(Map.Entry.<String, Integer>comparingByValue().reversed())
					.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue,(o,n)->n,LinkedHashMap::new));
					System.out.println("Sorting By Value Reversed: "+collect);
					LinkedHashMap<String, Integer> collect2 = hashMap.entrySet().stream().sorted(Map.Entry.<String, Integer>comparingByValue())
							.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue,(o,n)->n,LinkedHashMap::new));
					System.out.println("Sorting By Value Natural Order: "+collect2);

				}
			}
			
		Output :
			
			Original Map with Duplicates ... Before Sorting: {Java Developer=33000, Full Stack Developer=55000, Solutions Architect=25000, Data Scientist=23000}
			Sorting by Keys With TreeMap: {Data Scientist=23000, Full Stack Developer=55000, Java Developer=33000, Solutions Architect=25000}
			Sorting by Keys in Reversed Order With TreeMap using Comparator: {Solutions Architect=25000, Java Developer=33000, Full Stack Developer=55000, Data Scientist=23000}
			Sorting By Value Reversed: {Full Stack Developer=55000, Java Developer=33000, Solutions Architect=25000, Data Scientist=23000}
			Sorting By Value Natural Order: {Data Scientist=23000, Solutions Architect=25000, Java Developer=33000, Full Stack Developer=55000}


	2.	TreeMap Custom Object With Comparable
	
		Code Snippet :

			public class SortingTreeMapWithCustomObjects {

				public static void main(String[] args) {
					
					/*
						Sorting Custom Object TreeMap with Comparable
					*/
					Map<Car, Integer> carMap = new TreeMap<>();

					carMap.put(new Car(5000d, "BMW", 2018), 2);
					carMap.put(new Car(7000d, "AUDI", 2019), 5);
					carMap.put(new Car(4000d, "FORD", 2017), 3);
					carMap.put(new Car(6000d, "BENZ", 2020), 1);
					carMap.put(new Car(1000d, "ROYCE", 2018), 4);
					carMap.put(new Car(4000d, "FORD", 2017), 3);
					carMap.put(new Car(7000d, "AUDI1", 2019), 5);
					
					System.out.println(carMap);
				}
			}

			class Car implements Comparable<Car> {

				private double price;
				private String brand;
				private int model;

				@Override
				public String toString() {
					return "Car [price=" + price + ", brand=" + brand + ", model=" + model + "]";
				}

				public Car(double price, String brand, int model) {
					super();
					this.price = price;
					this.brand = brand;
					this.model = model;
				}

				// Getter and Setters
				
				
				@Override
				public int compareTo(Car car) {
					return this.brand.compareTo(car.brand);
				}
			}
		Output : 
			
			{Car [price=7000.0, brand=AUDI, model=2019]=5, Car [price=7000.0, brand=AUDI1, model=2019]=5, Car [price=6000.0, brand=BENZ, model=2020]=1, Car [price=5000.0, brand=BMW, model=2018]=2, Car [price=4000.0, brand=FORD, model=2017]=3, Car [price=1000.0, brand=ROYCE, model=2018]=4}

	3.	TreeMap Custom Object With Comparator


		Code Snippet :
		
			public class SortingTreeMapWithCustomObjectsComparator {

				public static void main(String[] args) {

					Map<Author, Integer> authorMap = new HashMap<>();
					authorMap.put(new Author("aaa ", "book3", 3), 9999);
					authorMap.put(new Author("ccc ", "book1", 1), 1111);
					authorMap.put(new Author("ccc ", "book1", 1), 1111);
					authorMap.put(new Author("bbb ", "book2", 4), 6666);
					authorMap.put(new Author("aaa ", "book3", 2), 7777);
					authorMap.put(new Author("ddd ", "book4", 5), 2222);
					authorMap.put(new Author("ddd ", "book4", 5), 2222);
					authorMap.put(new Author("bbb ", "book5", 6), 8888);

					System.out.println("Orginal HashMap: ");
					authorMap.forEach((k, v) -> System.out.println((k + ":" + v)));

					Map<Author, Integer> authorByIdTreeMap = new TreeMap<Author, Integer>((a1, a2) -> a1.getId() - a2.getId());
					authorByIdTreeMap.putAll(authorMap);
					System.out.println("Sorting By Id: ");
					authorByIdTreeMap.forEach((k, v) -> System.out.println((k + ":" + v)));
					
					Map<Author, Integer> authorByBookTreeMap = new TreeMap<Author, Integer>((a1, a2) -> a1.getBook().compareTo( a2.getBook()));
					authorByBookTreeMap.putAll(authorMap);
					System.out.println("Sorting By BookName: ");
					authorByBookTreeMap.forEach((k, v) -> System.out.println((k + ":" + v)));
					
					Map<Author, Integer> authorByNameTreeMap = new TreeMap<Author, Integer>((a1, a2) -> a2.getName().compareTo( a1.getName()));
					authorByNameTreeMap.putAll(authorMap);
					System.out.println("Sorting By Author Name: ");
					authorByNameTreeMap.forEach((k, v) -> System.out.println((k + ":" + v)));

				}

			}

			class Author {

				private String name;
				private String book;
				private int id;

				@Override
				public String toString() {
					return "Author [name=" + name + ", book=" + book + ", id=" + id + "]";
				}

				public Author(String name, String book, int id) {
					super();
					this.name = name;
					this.book = book;
					this.id = id;
				}

				// Getter and Setters
			}
			
		Output :
			
			Orginal HashMap: 
			Author [name=aaa , book=book3, id=3]:9999
			Author [name=ccc , book=book1, id=1]:1111
			Author [name=ddd , book=book4, id=5]:2222
			Author [name=bbb , book=book5, id=6]:8888
			Author [name=aaa , book=book3, id=2]:7777
			Author [name=bbb , book=book2, id=4]:6666
			Author [name=ddd , book=book4, id=5]:2222
			Author [name=ccc , book=book1, id=1]:1111
			Sorting By Id: 
			Author [name=ccc , book=book1, id=1]:1111
			Author [name=aaa , book=book3, id=2]:7777
			Author [name=aaa , book=book3, id=3]:9999
			Author [name=bbb , book=book2, id=4]:6666
			Author [name=ddd , book=book4, id=5]:2222
			Author [name=bbb , book=book5, id=6]:8888
			Sorting By BookName: 
			Author [name=ccc , book=book1, id=1]:1111
			Author [name=bbb , book=book2, id=4]:6666
			Author [name=aaa , book=book3, id=3]:7777
			Author [name=ddd , book=book4, id=5]:2222
			Author [name=bbb , book=book5, id=6]:8888
			Sorting By Author Name: 
			Author [name=ddd , book=book4, id=5]:2222
			Author [name=ccc , book=book1, id=1]:1111
			Author [name=bbb , book=book5, id=6]:6666
			Author [name=aaa , book=book3, id=3]:7777

			
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
				
				
		Coding Snippet :

			Map<String, Integer> map = new HashMap<>();

			map.put("Apple", 100);
			map.put("Orange", 50);
			map.put("Pineapple", 60);
			map.put("Grapes", 30);
			map.forEach((k,v)-> System.out.println("[ Key: "+ k+ " | "+ "Value: "+v +" ]"));
			
		Output :

			[ Key: Apple | Value: 100 ]
			[ Key: Grapes | Value: 30 ]
			[ Key: Pineapple | Value: 60 ]
			[ Key: Orange | Value: 50 ]

			
	2. List Interface
	
		-	forEach() method will take the Consumer FunctionalInterface as an argument 
		-	Consumer Interface skeleton
			
				@FunctionalInterface
				public interface Consumer<T> {
					void accept(T t);
				}
				
		Code Snippet :

			List<String> stringList = Arrays.asList(new String[] {"a","b","b","a", null, null});
			stringList.forEach(System.out::println);
			
			persons.forEach(p -> p.setLastName("Doe"));
	
		Output :
		
			a
			b
			b
			a
			null
			null

	3.	Set Interface
	
		-	forEach() method will take the Consumer FunctionalInterface as an argument 
		-	Consumer Interface skeleton
			
				@FunctionalInterface
				public interface Consumer<T> {
					void accept(T t);
				}
		
		
		Code Snippet :
		
			Set<String> stringSet =  new HashSet<>();
			List<String> stringList = Arrays.asList(new String[] {"a","b","b","a", null, null});
			stringSet.addAll(stringList);// HashSet Removes Duplicates but allows null only once
			stringSet.forEach(System.out::println);

		Output :

			null
			a
			b

----------------------------------------------------------------------------------------	
## 3.	filter() method of Stream Interface

	-	Stream API is added from Java 8 
	- 	filter() method is used to filter some the stream elements 
	-	filter() method takes Predicate<T> as the input argument
	-	filter() method is intermediate method that will gets executed only when terminal method gets invoked
	-	filter will used commonly used along with terminal methods like collect() or findAny(), findFirst(),  boolean anyMatch(predicate), boolean allMatch(Predicate p)
	
	- orElse(), orElseGet(), get(), orElseThrow(), ofNullable(T), of(T) are terminal methods can be applied after filter methods
	
		
		
		Predicate<T>
		
			-	Predicate is a FunctionalInterface in Java 
			-	Predicate take one argument and returns boolean value
			
			-	Skeleton of Predicate interface
				
					@FunctionalInterface
					public interface Predicate<T> {
						
						public boolean test(T t);
					}
					
	1.	Map -> filter()
		
		-	map.entrySet().stream().filter()
		-	filter() on Map Impl will Key-Value pair Entry as an argument
			
			Ex: 
				
				1.Streams Filter and collect
					
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
					
					a).	filter collect and toMap						
						Predicate<String> p = s -> s.equals("string");
						map.entrySet().stream().filter(m -> p.test(m.getValue()))
						.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue));
		
					b).	Generics
						private static Map<Integer, String> someMap;
						
						public static <K, V> Map<K, V> filterByValue(Map<K, V> map, Predicate<V> p) {
							 return map.entrySet().stream().filter(m -> p.test(m.getValue()))
									.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue));
						}
						
						Map<Integer, String> map1 = filterByValue(someMap, s -> s.contains("someString"));
						Map<Integer, String> map2 = filterByValue(someMap, s -> s.length()>3);
						
	2.	List -> filter() 
	
			Code Snippet :

				List<Person> persons = new ArrayList<>();
				persons.add(new Person("aaa", 1, 200.00));
				persons.add(new Person("ttt", 2, 500.00));
				persons.add(new Person("vvv", 3, 700.00));
				persons.add(new Person("bbb", 4, 900.00));
				persons.add(new Person("aaa", 5, 400.00));
				persons.add(new Person("qqq", 6, 300.00));
				persons.add(new Person("aaa", 7, 600.00));
				persons.add(new Person("bbb", 8, 870.00));
				persons.add(new Person("yyy", 9, 780.00));

				 Person orElse = persons.stream().filter(p -> p.salary > 600).filter(p -> p.name.equals("aaa"))
						.findFirst().orElse(new Person("Bharath",8,800));
				 
				 Person person = persons.stream().filter(p -> p.salary > 100).filter(p -> !p.name.equals("zzz"))
					.findAny().get();
				 
				 Person personNull = persons.stream().filter(p -> p.salary < 400).filter(p -> p.name.equals("zzz"))
						 .findAny().orElse(null);
						 
				List<Person> persons = …
				Stream<Person> personsOver18 = persons.stream().filter(p -> p.getAge() > 18);		 
				 
				System.out.println(orElse);
				System.out.println(person);
				System.out.println(personNull);
		
			Output :
			
				Person [name=Bharath, id=8, salary=800.0]
				Person [name=aaa, id=1, salary=200.0]
				null
				
	3.	filter() and map()			
	
		Code Snippet :
		
			// Filter with multiple conditions
			List<Double> collect = persons.stream().filter(p -> p.id > 3 && p.id <10).map(p -> p.salary).collect(Collectors.toList());
			System.out.println(collect);
			
			// Filter With Map
			Stream<Student> students = persons.stream()
				.filter(p -> p.getAge() > 18)
				.map(person -> new Student(person));
			
			// Filter With Map using Method Reference
			Stream<Student> students = persons.stream()
				.filter(p -> p.getAge() > 18)
				.map(Student::new);	
				
			List<Student> students = persons.stream()
				.filter(p -> p.getAge() > 18)
				.map(Student::new)
				.collect(Collectors.toList());
				
			List<Student> students = persons.stream()
				.filter(p -> p.getAge() > 18)
				.map(Student::new)
				.collect(Collectors.toCollection(ArrayList::new));	
		
			
			// Using Sequential and parallel
			List<Student> students = persons.stream()
				.parallel()
				.filter(p -> p.getAge() > 18)  // filtering will be performed concurrently
				.sequential()
				.map(Student::new)
				.collect(Collectors.toCollection(ArrayList::new));
				
			boolean anyMatch = persons.parallelStream().sequential().anyMatch(p -> p.salary==4900.00);	
			
			List<Student> students = persons.stream()
				.parallel()
				.filter(p -> p.getAge() > 18)  // filtering will be performed concurrently
				.sequential()
				.map(Student::new)
				.collect(Collectors.toCollection(ArrayList::new));
				
		Output :

			[900.0, 400.0, 300.0, 600.0, 870.0, 780.0]
----------------------------------------------------------------------------------------

## 4.	collect() 

	-	collect() method is a terminal method ---> means it closed the stream and returns the values other than Stream 
	- 	Values can be List, Set or Map
	-	collect() method takes Collectors as an argument that returns set, map, count, sum of elements
		
	Code Snippet :
	
		list.stream().sorted().filter().collect(Collectors.toList());
		list.stream().sorted().collect(Collectors.toSet());
		map.entrySet().stream().sorted(Map.Entry.comparingByKey().reversed()).filter().collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue, (o,n)-> n, LinkedHashMap::new));
		list.stream().sorted().collect(Collectors.groupingBy(Pizza::getName, Collectors.toList()));

	
		
	
----------------------------------------------------------------------------------------
	
## 5.	findAny() and orElse, get(), orElseGet, orElseThrow()

-	findFirst(), findAny() and orElse, get(), orElseGet, orElseThrow() methods are terminal operations on stream
-	boolean anyMatch(Predicate p), boolean allMatch(Predicate p) are terminal methods that return boolean on Predicate		
-	findAny() method can be used once we filtered some of the values 
-	findAny() returns Optional<T> Object that may or not can contain the value
-	Optional is best way to avoid the null pointer exception
-	orElse() will get executed and returns default value


----------------------------------------------------------------------------------------
	
## 6.	map() method of Stream

-	map() method is used to transform one type to another type
-	map() method takes Function FunctionalInterface as an argument
-	map() method is intermediate operation or processing operation
- 	map() method needs to be pipelined with terminal methods like collect(), forEach(), findAny()
-	map() method can be pipelined with intermediate methods like filter(), sorted(),serial(), parallel()


	Code Snippets :
	
		1.	A List of Strings to Uppercase
		
			List<String> list = Arrays.asList("a","c","b","e");
			list.stream().map(s -> s.toUpperCase()).collect(Collectors.toList());
			list.stream().map(String::toUpperCase).collect();
		
		2.	List of objects -> List of String

			List<Staff> staffsList = new ArrayList<>();
			List<String> staffNames =   staffList.stream().map(Staff::getName).filter(name -> !name.equals("bharath")).collect(Collectors.toList());
			
		3.	List of objects -> List of other objects


			List<Staff> staffsList = new ArrayList<>();
			List<Person> personsList = new ArrayList<>();
			
			
			List<Person> personsList = staffList.stream().map(staff -> {
				Person person = new Person();
				person.setName(staff.getName());
				person.setIncome(staff.getSalary());
				person.setUniqueId(staff.getId());
				
				return person;// Returning Person Object
			}).collect(Collectors.toList());
	
----------------------------------------------------------------------------------------
## 7.	Collectors and Stream collect() Examples

	##	https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html

	1.	Group, Sort, Count values of List 
	
		-	To group and count we need to use Collectors methods like
		
			-	Collectors.groupingBy()
			
	Code Snippet :
	
		The following are examples of using the predefined collectors to perform common mutable reduction tasks:


		 // Accumulate names into a List
		 List<String> list = people.stream().map(Person::getName).collect(Collectors.toList());

		 // Accumulate names into a TreeSet
		 Set<String> set = people.stream().map(Person::getName).collect(Collectors.toCollection(TreeSet::new));

		 // Convert elements to strings and concatenate them, separated by commas
		 String joined = things.stream()
							   .map(Object::toString)
							   .collect(Collectors.joining(", "));

		 // Compute sum of salaries of employee
		 int total = employees.stream()
							  .collect(Collectors.summingInt(Employee::getSalary)));

		 // Group employees by department
		 Map<Department, List<Employee>> byDept
			 = employees.stream()
						.collect(Collectors.groupingBy(Employee::getDepartment));

		 // Compute sum of salaries by department
		 Map<Department, Integer> totalByDept
			 = employees.stream()
						.collect(Collectors.groupingBy(Employee::getDepartment,
													   Collectors.summingInt(Employee::getSalary)));

		 // Partition students into passing and failing
		 Map<Boolean, List<Student>> passingFailing =
			 students.stream()
					 .collect(Collectors.partitioningBy(s -> s.getGrade() >= PASS_THRESHOLD));
			
		// groupingBy()	
		Map<BigDecimal, List<Item>> collect = items.stream().collect(Collectors.groupingBy(Item::getPrice,Collectors.toList()));
			
		Map<String, List<Item>> collect2 = items.stream().collect(Collectors.groupingBy(Item::getName));
		
		Map<Integer, List<String>> collect3 = items.stream().collect(Collectors.groupingBy(Item::getQty, Collectors.mapping(Item::getName, Collectors.toList())));
		
		Map<String, Long> counting = items.stream().collect(Collectors.groupingBy(Item::getName, Collectors.counting()));
		
		Map<String, Integer> sum = items.stream().collect(Collectors.groupingBy(Item::getName, Collectors.summingInt(Item::getQty)));
		
		fruitsNames.stream().collect(Collectors.groupingBy(java.util.function.Function.identity(), Collectors.counting()));
		
		
		// joining
		String collect = stringList.stream().collect(Collectors.joining());
		String collect2 = stringList.stream().collect(Collectors.joining("||"));
		String collect3 = stringList.stream().collect(Collectors.joining("||","{","}"));
			 
				

----------------------------------------------------------------------------------------
## 8.	groupingBy() method
	
		Code Snippet :
		
			Map<BigDecimal, List<Item>> collect = items.stream().collect(Collectors.groupingBy(Item::getPrice,Collectors.toList()));
			
			Map<String, List<Item>> collect2 = items.stream().collect(Collectors.groupingBy(Item::getName));
			
			Map<Integer, List<String>> collect3 = items.stream().collect(Collectors.groupingBy(Item::getQty, Collectors.mapping(Item::getName, Collectors.toList())));
			
			Map<String, Long> counting = items.stream().collect(Collectors.groupingBy(Item::getName, Collectors.counting()));
			
			Map<String, Integer> sum = items.stream().collect(Collectors.groupingBy(Item::getName, Collectors.summingInt(Item::getQty)));
			
			fruitsNames.stream().collect(Collectors.groupingBy(java.util.function.Function.identity(), Collectors.counting()));
			
			
	
	
		Output :
		
			=========================================================================
			19.99:[Item [name=banana, qty=20, price=19.99], Item [name=banana, qty=10, price=19.99]]
			29.99:[Item [name=orang, qty=10, price=29.99], Item [name=watermelon, qty=10, price=29.99]]
			9.99:[Item [name=apple, qty=10, price=9.99], Item [name=papaya, qty=20, price=9.99], Item [name=apple, qty=10, price=9.99], Item [name=apple, qty=20, price=9.99]]
			=========================================================================
			papaya:[Item [name=papaya, qty=20, price=9.99]]
			banana:[Item [name=banana, qty=20, price=19.99], Item [name=banana, qty=10, price=19.99]]
			apple:[Item [name=apple, qty=10, price=9.99], Item [name=apple, qty=10, price=9.99], Item [name=apple, qty=20, price=9.99]]
			orang:[Item [name=orang, qty=10, price=29.99]]
			watermelon:[Item [name=watermelon, qty=10, price=29.99]]
			=========================================================================
			20:[banana, papaya, apple]
			10:[apple, orang, watermelon, apple, banana]
			=========================================================================
			
			//Group by + Count
			{
				papaya=1, banana=2, apple=3, orang=1, watermelon=1
			}

			//Group by + Sum qty
			{
				papaya=20, banana=30, apple=40, orang=10, watermelon=10
			}
			

----------------------------------------------------------------------------------------
## 9.	filter NULL values stream

	Code Snippet :
	
		Stream<String> languages = Stream.of("java", "python", "node", null, "ruby", null, "php");
		
		languages.collect(Collectors.toList());
		
		// Filtering null values 
		
		languages.filter(l -> l != null).collect(Collectors.toSet());
		
		// Filtering Null Values using Objects.nonNull() static method
		
		languages.filter(Objects::nonNull).collect(Collectors.toList());
		
		
----------------------------------------------------------------------------------------
## 10.	Convert a Stream to List

##### Code Snippet: 

```java
	Stream<Integer> stream = Stream.of(1,34,5656,664,4646);
	List<Integer> list = stream.collect(Collectors.toList());
````

----------------------------------------------------------------------------------------
## 11.	Arrays to Stream 

#### Code Snippets

	Arrays.java 
	
		public static <T> Stream<T> stream(T[] array) {
			return stream(array, 0, array.length);
		}
		
		public static IntStream stream(int[] array) {
			return stream(array, 0, array.length);
		}
	
	Stream.java
		
		public static<T> Stream<T> of(T... values) {
			return Arrays.stream(values);
		}
		
		public static<T> Stream<T> of(T t) {
			return StreamSupport.stream(new Streams.StreamBuilderImpl<>(t), false);
		}

		@FunctionalInterface
		public interface ToIntFunction<T> {
		
			int applyAsInt(T value);
		}
		
		@FunctionalInterface
		public interface Function<T, R> {
		
			R apply(T t);
			
		}
		
		IntStream mapToInt(ToIntFunction<? super T> mapper)
		IntStream flatMapToInt(Function<? super T, ? extends IntStream> mapper);
		
###	1. String Array to Stream 

##### Code Snippet: 

		String[] stringArray = new String [] { "d", "a", "g", "y" };
		Stream<String> stringStream = Stream.of(stringArray);
		Stream<String> stringStream = Arrays.stream(stringArray);
		
		
		
###	2.	Primitive int Array(int[]) to Stream


##### Code Snippets: 

	int[] intArray = { 1, 2, 3, 4, 5, 6 };

	
	// Using Arrays.stream()
	IntStream integerStream = Arrays.stream(intArray);
	intgerStream.forEach(i -> System.out.println(i));
	integerStream.forEach(System.out::println);
	
	// Using Stream.of()
	
	
	// We need to do additional Step for Stream.of() for Primitive Arrays
	// We need to flat Primitive Array to integer Stream using flatMapToInt(), flatMapToLong(), flatMapToDouble()
	
	Stream<int[]>  intStream = Stream.of(intArray);
	IntegerStream integerStream = intStream.flatMapToInt(iArray -> Arrays.stream(iArray));
	integerStream.forEach(System.out::println);
	
	
	//Below will not work
	// Return type of Arrays.stream(iArray) is IntStream;
	// Return type of ToIntFunction is int
	
	intStream.mapToInt(iArray -> Arrays.stream(iArray)); // Compilation Error: Type mismatch: cannot convert from IntStream to int
	 
	
-----------------------------------------------------------------------------------------

## 12.	Stream already operated upon

##### Code snippets

	String [] stringArray = { "d", "q", "g", "r" };
	Stream<String> streamString = Stream.of(stringArray);
	
	streamString.forEach(System.out::println);
	
	streamString.filter(s -> !s.equals(a)).collect(Collectors.toList());// Will Throw java.lang.IllegalStateException: Stream Already been Operated or closed Exception
	
#### Solution: use Stream Supplier

	String [] stringArray = new String[] { "d", "q", "g", "r" };
	Supplier<Stream<String>>  supplier = () -> Stream.of(stringArray);
	
	supplier.get().forEach(System.out::println);
	supplier.get().filter(s -> !s.equals(a)).collect(Collectors.toList());
	
-----------------------------------------------------------------------------------------

## 13.	sorted() 

-	sorted() method is a intermediate operation 
-	sorted() method is used to sort the Collection Elements
-	sorted() 


### Comparator Methods:
	
	Comparator.naturalOrder();
	Comparator.reverseOrder();
	Comparator.comparing(Function);
	Comparator.comparing(Function, Comparator);
	Comparator.comparingInt(IntFunction);
	Comparator.comparingDobule(DoubleFunction);
	Comparator.comparingLong(LongFunction);
	Comparator.nullsFirst();
	Comparator.nullsLast();

### List/Set: 

	list.stream().sorted(Comparator....)
	
### Map:

	map.entrySet().stream().sorted(Map.Entry.comparingByKey());
	map.entrySet().stream().sorted(Map.Entry.comparingByValue());
	map.entrySet().stream().sorted(Map.Entry.comparingByKey(Comparator.....));
	map.entrySet().stream().sorted(Map.Entry.comparingByKey(Comparator....));
	
-----------------------------------------------------------------------------------------	
	
	
##	14.	flatMap() Converting to Stream


Stream<String[]>	
Stream<Set<String>>	
Stream<List<String>>	
Stream<List<Object>>

String [][] stringArray -> Arrays.stream() -> Stream<String[]> -> flatMap() -> Stream<String>
Stream<String[]>		-> flatMap ->	Stream<String>  


Stream<Set<String>>	-> flatMap ->	Stream<String>
Stream<List<String>>	-> flatMap ->	Stream<String>
Stream<List<Object>>	-> flatMap ->	Stream<Object>



public class Student {

    private String name;
    private Set<String> book;

    public void addBook(String book) {
        if (this.book == null) {
            this.book = new HashSet<>();
        }
        this.book.add(book);
    }
    //getters and setters

}



 Student obj1 = new Student();
        obj1.setName("mkyong");
        obj1.addBook("Java 8 in Action");
        obj1.addBook("Spring Boot in Action");
        obj1.addBook("Effective Java (2nd Edition)");

        Student obj2 = new Student();
        obj2.setName("zilap");
        obj2.addBook("Learning Python, 5th Edition");
        obj2.addBook("Effective Java (2nd Edition)");
		
		
		
 List<Student> list = new ArrayList<>();
        list.add(obj1);
        list.add(obj2);

list.stream().map(x -> x.getBook()).flatMap(x -> x.stream()).distinct().collect(Collectors.toList());

-----------------------------------------------------------------------------------------

##	15. Optional 

-	Optional is a utility class from java.util package
-	Optional is container class that holds at most one object at a time
-	Optional represents whether values is present or not 

### Advantages of Optional 

-	No need of null checks
-	No Runtime NullPointerException 
-	Clear and Clean APIs

### Methods of Optional 

1.	of(T)

	-	Throws NullPointerException Exception for Optional.of(null)
	
2.  ofNullable(T)

3.	filter(Predicate predicate)

4.	ifPresent(Consumer consumer)

5.	boolean isPresent()

6.	Optional.map(Function)

7. Optional.flatMap()

8.	orElse(), orElseThrow, get(), orElseGet()
		
	Code Snippet:
	
		Optional<String> stringOptional = Optional.of(new String("Bharath"));
		
		System.out.println(stringOptional);
		
		//Optional.of(null);
		
		Optional<Object> empty = Optional.empty();
		
		
		System.out.println(Optional.ofNullable(null));
		System.out.println(Optional.ofNullable(Optional.ofNullable(Optional.ofNullable(89))));
		Optional<Double> ofNullable = Optional.ofNullable(7.93d);
		System.out.println(ofNullable);
		
		Optional<StringBuffer> map = Optional.ofNullable("").map(s -> new StringBuffer().append("Bharath"));
		System.out.println(map);
		
		Optional<String> map2 = map.map(s -> s.toString()).filter(s -> s.equals("Bharath")).map(s -> "Sharath");
		
		System.out.println(map2);
		
		
		System.out.println(map.isPresent());
		
		System.out.println(Optional.ofNullable("").isPresent());
		Optional.ofNullable("").ifPresent(s -> IntStream.of(1000).forEach(System.out::println));
		
		Optional<Integer> optionalInteger = Optional.of(56);
		Optional<Integer> filter = optionalInteger.filter(integer -> integer > 10);
		System.out.println(filter);
		
			Optional.ofNullable(89).ifPresent(OptionalEx::processOptional);
	
	
		static void processOptional(Integer intger) {
	
		}
-----------------------------------------------------------------------------------------
## 16.	 String Joiner and String.join()

1. StringJoiner

	-  	StringJoiner is a Class added in Java 8
	-	StringJoiner is a final class
	-	StringJoiner is used to join the strings
	-	StringJoiner has overloaded constructors 
	
		-	Takes delimiter and another takes prefix and suffix	 
		
```java		
	public StringJoiner(CharSequence delimiter) {}
	public StringJoiner(CharSequence delimiter, CharSequence prefix, CharSequence suffix){}
````
		-	StingJoiner.add("string")
			
			-	add() method is used to add the strings
		
		
##### Code Samples
	
```java
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
````	
2.	String.join()

	-	Added since Java 8
	-	join() method takes delimiter and elements as the params
	-	join() is a static method and has two overloaded methods
	-	Takes CharSequence or Collection Impl
	
```java
	public static String join(CharSequence delimiter, CharSequence ... elements)
	public static String join(CharSequence delimiter, Iterable<? extends CharSequence> elements)
````	
		
3.	Collectors.joining()

	-	Collectors.joining() will takes the delimiter, prefix and suffix
	-	Returns string with joined the set of strings

	- 	joining() method of Collectors interface have 3 overloaded methods 
		
```java
	public static Collector<CharSequence, ?, String> joining(CharSequence delimiter)
	public static Collector<CharSequence, ?, String> joining(CharSequence delimiter, CharSequence prefix, CharSequence suffix)
````
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
					
					
				Code Snippet: 
				
					ArrayUtil.addAll(array1, array2);
				
				<dependency>
					<groupId>org.apache.commons</groupId>
					<artifactId>commons-lang3</artifactId>
					<version>3.4</version>
				</dependency>
					
			2. 	Java 8 Stream
								
				1.	Object 
				
					Sting[] - String Array
					String [] result = Stream.of(s1, s2, s3).flatMap(Stream::of).toArray(String[]::new);
		
				2.	Primitive Arrays
					
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
	
## 20. Random Class


-	Random is a class from java.util package
-	Random is a utility class used to generate random integers 


### Code Snippet :

	Random random = new Random();
	
	random.nextInt(bound);
	random.nextInt(10); [0(min) ... 9(max)] // max is exclusive
	
-	Include max as boundary 
	
	random.nextInt(10 + 1);[0 (min) ... 10(max)] // max 11 is exclusive 
	
-	Integer b/w max and min

#### Formula for Boundary
	
-	random.nextInt((max - min)+1) + min;
	
	
### Code Snippet for Generating 10 random integer b/w boundary


	main() {
	
		for(int i=0; i<10; i++){
			generateRandomNumbers(10, 30);
		}
	}
	
	public void generateRandomNumbers(int min, int max) {
		
		if(min >= max) {
			
			throw new IllegalArgumentException("");
		} else {
			Random random = new Random();
			Integer integer = random.nextInt((max - min) + 1) + min;
			System.out.println(intger);
		}
	
	}
	
	
### Java 8

-	In Java 8, two methods have been added to Random class

	public IntStream ints(int min, int max);
	public IntStream ints(long streamSize, int min, int max);

-	max is exclusive


#### Code Snippets:

		Random random = new Random();
		IntStream ints = random.ints(10);
		
		System.out.println(ints);
		System.out.println(random.ints());
		
		random.ints(1,10).forEach(System.out::println);// Result is infinite Loop
		int asInt = random.ints(10, 20 + 1).limit(10).findFirst().getAsInt();
		System.out.println(asInt);
		
		
		random.ints(5, 10, 16).forEach(System.out::println);
		
		
-------------------------------------------------------------

## 21.	Primitive Array to List

int [] intArray = {1, 3, 4,5,3,2,5,3};
List<int[]> ints = Arrays.asList(intArray);


-	To convert int[] array to List  

	List<Integer> list = Arrays.stream(intArray).boxed().collect(Collectors.toList());

-------------------------------------------------------------

## 22. IntStream or Primitive Stream

-	Examples of IntStream


#### Code Snippet of Primitive Stream
	
		int [] intArray = {1, 2, 3, 4, 5, 6, 7, 8, 9, 0};
		
		Stream<Integer> intStream = Arrays.stream(intArray).boxed().collect(Collectors.toList());
		
		IntStream intStream = IntStream.of(intArray);	
		IntStream.of(10);
		IntStream.of(1,3,5,6,7);
		
		IntStream.concat(Arrays.stream(intArray1), Arrays.stream(intArray2));
		
		
	
		IntStream.range(0, 5);// Output: 0, 1, 2, 3, 4
		IntStream.rangeClosed(0, 5); // Output: 0, 1, 2, 3, 4 
			
		IntStream.range(startInclusive, endExclusive);
		IntStream.rangeClosed(startInclusive, endInclusive);		
		
		
		IntStream.range(0, 5)// can be used instead of for loop
		
		for(int i = startInclusive; i < endExclusive; i ++ ) {
		}
		
		
#####	Finding if Array Contains elements 

		IntStream.of(intArray).anyMatch(x -> x == 9); // true // at-least one element should be there
		
		IntStream.of(intArray).allMatch(x -> x == 9); // false // All elements should match with element ie. ..9
		
####	Finding all the elements of IntStream of Int Array


		IntStream.of(1, 4, 5, 5, 6, 8).sum();
		IntStream.of(1, 4, 5, 5, 6, 8).min();
		IntStream.of(1, 4, 5, 5, 6, 8).max();
		IntStream.of(1, 4, 5, 5, 6, 8).average();



-------------------------------------------------------------

## 23. Reading Files in Java 8


### 1.	Files.lines and Paths.get

-	Files and Paths are classes from java.nio package
-	Non-Blocking package was introduced in Java 7
-	Files.lines() method is introduced in Java 8
- 	Files.lines() method returns a Stream<String> of String

### 2.	Stream

#### Code Snippet:
	
		try (Stream<String> lines = Files.lines(Paths.get("files/file.txt"), StandardCharsets.ISO_8859_1)) {
			
			lines.forEach(System.out::println);
			
		} catch(IOException e) {
			e.printStackTrace();
		}
		
### 3.	BufferedReader and Stream with Files and Paths
		
#### Code Snippet:
		
		try(BufferedReader br = Files.newBufferedReader(Paths.get("files/file.txt"))) {
			
			List<String> collect =  br.lines().collect(Collectors.toList());
		
		} catch(IOException e) {
			e.printStackTrace();
		}
		
### 4.	Classic BufferedReader and Scanner

#### Code Snippet #1 of BufferedReader:

		try {
			BufferedReader br = new BufferedReader(FileReader("files/file.txt")); 
			
			String line = "";
			while((line = br.readLines()) != null){
				System.out.println(line);
			}
		
		} catch(IOException e){
			e.printStackTrace();
		}

#### Code Snippet #2 of Scanner:

		try{
			Scanner scan = new Scanner(new File("files/file.txt"));
			
			while(scan.hasNext()) {
				System.out.println(scan.nextLine());
			}
			
		} catch(IOException e) {
			e.printStackTrace();
		}	

-------------------------------------------------------------

## 24. Java 8 Date Time API


### Need for new Date Time API

####	1.	Not consistent and Poor Design

-	No uniformity b/w month, year, date
-	No direct utility methods 
-	Complex to parse the dates across different formats
-	Difficult to parse and format with SimpleDateFormatter
- 	Two different Confusing java.util.Date and java.sql.Date Classes
 
####	2.	Not Thread Safe and Mutable

-	java.util.Date is not Thread Safe 
-	java.util.Date is mutable
-	Developers needs to handle Thread Safety by implementing Synchronization
-	SimpleDateFormat is not Thread Safe Class --> Multiple Threads can't work on SimpleDataFormatter


####	3.	Difficult TimeZone Handling

-	Needs to write lot of code for handling timezone support
-	TimeZone is difficult to handle



### Time Zone


-	Time Zone is a region of the earth where the same standard time is used
-	Each Time Zone is described and identified by an identifier 
-	Identifier has a format of Region/City (Asia/Kolkata) and an offset from Greenwich/UTC 
-	EX: Offset for Kolkata is +05.30
-	


---------------------------

### Java 8 API Changes and Advantages

####	1. 	Thread Safe and Immutable 

-	All Classes in java.time package are immutable and Thread Safe 
-	Class from java.time package are good for Multi-Threaded  enviroment


####	2.	Separation of Concerns 

-	Separate classes for Date, Time, DateTime, TimeZone, TimeStamp	
-	Easy to perform operations
-	Defines Principle Date-Time concepts like Duration, Date, Time, Period, Instant, TimeZone based on ISO Calender System

####	3.	Clarity

-	All the classes uses Factory Design and Strategy pattern for better handling
-	Methods are clear and performs same operation across all classes
	
	-	Ex: now() is defined all classes and performs same operation 
	-	now() gives the current instance
	-	format() and parse() methods are defined in all of these classes
	

#### 4.	Utility Operations

-	All Date and Time API classes comes with utility methods for common operations like plus(), minus(), format(), parsing()

Local : Simplified date-time API with no complexity of timezone handling.
Zoned : Specialized date-time API to deal with various timezones.



---------------------------

###	Java 8 DateTime Packages

####	1.	java.time package

-	java.time package is a base package
-	java.time contains base classes like 

	-	LocalDate
	-	LocalTime
	-	LocalDateTime
	-	Instant 
	-	Period
	-	Duration
	
####	2.	java.time.format package

-	java.time.format package contains classes that are used for formatting and parsing 
-	Not used frequently
-	Base classes provides methods() to parse and format dates


####	3.	java.time.temporal 

-	package contains classes used to find first or last day of months


####	4.	java.time.zone package 

-	Contains classes to support TimeZone operations


-------------------------------------------------------------

## 	Java 8 Date/Time API Classes 


### 1. LocalDate 

-	Immutable Class and Thread Safe 
-	Default Format is yyyy-MM-dd	
-	LocalDate.now() method is used to get current date 
-	Can provide Year, month, date while creating LocalDate instance
-	LocalDate class provides overloaded now() methods ... where we can pass ZoneId to get date by ZoneId



#### Code Snippets 

```java
	// Current Date
	LocalDate date =  LocalDate.now(); // 2019-01-17
	
	
	//Creating by LocalDate by ZoneId
	LocalDate date = LocalDate.now(ZoneId.of("Aisa/Kolkata")); // Current Date in IST 2019-01-17
	
	//Creating LocalDate by providing input arguments
	LocalDate date = LocalDate.now(2015, Month.SEPTEMBER, 7); // 2015-09-07
	
	// Creating date by giving invalid date as input
	// Throws Exception
	LocalDate date = LocalDate.of(2014, Month.FEBRAURY, 29);
	
	//Base date is 01/01/1970
	// Getting date from Base Date of 01/01/1970
	LocalDate date = LocalDate.ofEpochDay(365); // 01/01/1971
	
	
	//Getting 100th day of 2019
	LocalDate date = LocalDate.ofYearDay(2019, 100); // 
````		
		
###	2. LocalTime

-	Immutable and Thread Safe
-	Default format is hh:mm:ss:ZZZ
-	now() has overloaded methods
-	Provides TimeZone support
-	Can create instance by passing hour, minute, seconds


	Code Snippets

```java	
	LocalTime time = LocalTime.now();
	
	
	LocalTime time = LocalTime.now(ZoneId.of("Australia/Sydney")); //
	
	LocalTime time = LocalTime.of(12, 20, 25, 40); // 12:20:25.00000040
	
	LocalTime time = LocalTime.ofSecondDay(1000); // 02:46:40
````

		
###	3.	LocalDateTime

-	Immutable and Thead Safe
-	Default Date Time format is : yyyy-MM-dd-HH:mm:ss.zzz
-	Provides current date and time 
-	Provides Factory methods to take LocalTime and LocalDate as input and creates LocalDateTime instance from them
-	If we provide invalid arguments for creating Date/Time, then it throws java.time.DateTimeException 
-	That is a RuntimeException, so we don’t need to explicitly catch it

	Code snippets:
```java	
	LocalDateTime dateTime = LocalDateTime.now();
	
	
	LocalDateTime dateTime = LocalTimeDate.now(ZoneId.of("Asia/Kolkata"));	
		
	// LocalDateTime dateTime = LocalDateTime specificDate = LocalDateTime.of(2014, Month.JANUARY, 1, 10, 10, 30);

	
	//Current DateTime using LocalDate and LocalTime
	LocalDateTime today = LocalDateTime.of(LocalDate.now(), LocalTime.now());
	
	//Try creating date by providing invalid inputs
		//LocalDateTime feb29_2014 = LocalDateTime.of(2014, Month.FEBRUARY, 28, 25,1,1);
		//Exception in thread "main" java.time.DateTimeException: 
		//Invalid value for HourOfDay (valid values 0 - 23): 25

		
	//Getting date from the base date i.e 01/01/1970
		LocalDateTime dateFromBase = LocalDateTime.ofEpochSecond(10000, 0, ZoneOffset.UTC);
````

		

### 3. Instant	

-	Instant class is Immutable and Thread Safe
-	Used to work machine readable time format
-	It store time in unix timestamp 
-	Instant class needs to be used for representing the specific timestamp ant any moment
-	Instant class represents an instant in time to an accuracy of nanoseconds
-	Instant - It represents a time-stamp e.g. 2014-01-14T02:20:13.592Z 
-	Operations on an Instant include comparison to another Instant and adding or subtracting a duration
-   Can be obtained from java.time.Clock class as shown below :
	
		Instant current = Clock.system(ZoneId.of("Asia/Tokyo")).instant();
	
#### Code Snippets

```java
	//Current timestamp
	Instant timestamp = Instant.now();
	System.out.println("Current Timestamp = "+timestamp);
	
	//Instant from timestamp
	Instant specificTime = Instant.ofEpochMilli(timestamp.toEpochMilli());
	System.out.println("Specific Time = "+specificTime);
	

	
	Instant instant = Instant.now();
	System.out.println(instant.toString());                                 //2013-05-15T05:20:08.145Z
	System.out.println(instant.plus(Duration.ofMillis(5000)).toString());   //2013-05-15T05:20:13.145Z
	System.out.println(instant.minus(Duration.ofMillis(5000)).toString());  //2013-05-15T05:20:03.145Z
	System.out.println(instant.minusSeconds(10).toString());                //2013-05-15T05:19:58.145Z
````		
		
		
###	4.	Duration

-	Thread Safe and Immutable
-	Duration class is a whole new concept brought first time in java language.
-	It represents the time difference between two time stamps
-	Duration deals with small unit of time such as milliseconds, seconds, minutes and hour.
-	They are more suitable for interacting with application code.


#### Code snippets 

```java
	//Duration example
	Duration thirtyDay = Duration.ofDays(30);
	System.out.println(thirtyDay);	 //PT720H
	
	Duration duration = Duration.ofMillis(5000);
	System.out.println(duration.toString());     //PT5S
	 
	duration = Duration.ofSeconds(60);
	System.out.println(duration.toString());     //PT1M
	 
	duration = Duration.ofMinutes(10);
	System.out.println(duration.toString());     //PT10M
	 
	duration = Duration.ofHours(2);
	System.out.println(duration.toString());     //PT2H
	 
	duration = Duration.between(Instant.now(), Instant.now().plus(Duration.ofMinutes(10)));
	System.out.println(duration.toString());  //PT10M	
````


### 5.	Period 

-	To interact with human, you need to get bigger durations which are presented with Period class.


#### Code Snippets :

	Period period = Period.ofDays(6);
	System.out.println(period.toString());    //P6D
	 
	period = Period.ofMonths(6);
	System.out.println(period.toString());    //P6M
	 
	period = Period.between(LocalDate.now(),
				LocalDate.now().plusDays(60));
	System.out.println(period.toString());   //P1M29D

### 6.	DateTimeFormatter
	
-	Thread Safe and Immutable
-	DateFormatter class provides numerous predefined formatters and also can define your own
-	DateTimeFormatter has parse() method to convert String to Date in Java and throws DateTimeParseException
-	format() method to format dates in Java, and it throws DateTimeException


### 7.	Utility classes over existing enums

#### DayOfWeek

#####	Code Snippets:
	
			//day-of-week to represent, from 1 (Monday) to 7 (Sunday)
			System.out.println(DayOfWeek.of(2));                    //TUESDAY
			 
			DayOfWeek day = DayOfWeek.FRIDAY;
			System.out.println(day.getValue());                     //5
			 
			LocalDate localDate = LocalDate.now();
			System.out.println(localDate.with(DayOfWeek.MONDAY));  //2013-05-13  i.e. when was monday in current week ?
			


#### Date Adjusters
	
#####	Code Snippets:

			LocalDate date = LocalDate.of(2013, Month.MAY, 15);                     //Today
			 
			LocalDate endOfMonth = date.with(TemporalAdjusters.lastDayOfMonth());
			System.out.println(endOfMonth.toString());                              //2013-05-31
			 
			LocalDate nextTue = date.with(TemporalAdjusters.next(DayOfWeek.TUESDAY));
			System.out.println(nextTue.toString());      



#### Creating date objects

-	Creating date objects now can be done using builder pattern 
-	The builder pattern allows the object you want to be built up using individual parts.
-	This is achieved using the methods prefixed by “at”.
			
			
	Code Snippets:
		
		//Builder pattern used to make date object
		 OffsetDateTime date1 = Year.of(2013)
								.atMonth(Month.MAY).atDay(15)
								.atTime(0, 0)
								.atOffset(ZoneOffset.of(&quot;+03:00&quot;));
		 System.out.println(date1);                                     //2013-05-15T00:00+03:00
		 
		//factory method used to make date object
		OffsetDateTime date2 = OffsetDateTime.
								of(2013, 5, 15, 0, 0, 0, 0, ZoneOffset.of(&quot;+03:00&quot;));
		System.out.println(date2);     
	
	
-------------------------------------------------------------
## 	Examples of Java 8  DateTime API

### 1.	Example 1 - How to get today's date in Java 8



	Code Snippet :
	
		LocalDate today = LocalDate.now(); 
		System.out.println("Today's Local date : " + today);

		Output Today's Local date : 2014-01-14
		

### 2: Example 2 - How to get current day, month and year in Java 8


	Code Snippet: 
	
		LocalDate today = LocalDate.now();
		int year = today.getYear();
		int month = today.getMonthValue();
		int day = today.getDayOfMonth();
		System.out.printf("Year : %d Month : %d day : %d \t %n", year, month, day); 
		
		Output: 
			Today's Local date :	2014-01-14 
			Year : 2014 Month : 1 day : 14 


###	3.	Example 3 - How to get a particular date in Java 8


	Code Snippet:
	
		LocalDate dateOfBirth = LocalDate.of(2010, 01, 14);
		System.out.println("Your Date of birth is : " + dateOfBirth);

		Output :

			Your Date of birth is : 2010-01-14
		
	
### 4. Example 4 - How to check if two dates are equal in Java 8

	
	Code Sippets :
	
		LocalDate date1 = LocalDate.of(2014, 01, 14);

		if(date1.equals(today))
		{ 
			System.out.printf("Today %s and date1 %s are same date %n", today, date1); 
		} 
			
		Output: 
			Today 2014-01-14 and date1 2014-01-14 are same date

	

### 5.	Example 5 - How to check for recurring events e.g. birthday in Java 8


	Code Snippets:
	
		LocalDate dateOfBirth = LocalDate.of(2010, 01, 14);
		MonthDay birthday = MonthDay.of(dateOfBirth.getMonth(), dateOfBirth.getDayOfMonth());
		MonthDay currentMonthDay = MonthDay.from(today); 
		if(currentMonthDay.equals(birthday)){ 
			System.out.println("Many Many happy returns of the day !!"); 
		}else{ 
			System.out.println("Sorry, today is not your birthday");
		}

		Output:
		
			Many Many happy returns of the day !!
		
		
		
### 6.	Example 6 - How to get current Time in Java 8


	Code Snippets:
	
		LocalTime time = LocalTime.now();
		System.out.println("local time now : " + time);

		Output: 
		    local time now : 16:33:33.369  // in hour, minutes, seconds, nano seconds


###	7. 	Example 7 - How to add hours in time

		
		
		Code Snippets:
		
			LocalTime time = LocalTime.now();
			LocalTime newTime = time.plusHours(2); 
			// adding two hours
			System.out.println("Time after 2 hours : " + newTime);
		
		Output : 
			Time after 2 hours : 18:33:33.369

###	8. 	Example 8 - How to find Date after 1 week

		Code Snippets:
		
			LocalDate nextWeek = today.plus(1, ChronoUnit.WEEKS);
			System.out.println("Today is : " + today);
			System.out.println("Date after 1 week : " + nextWeek);


			Output: Today is:
				2014-01-14 
				Date after 1 week : 2014-01-21


### 9.	Example 9 - Date before and after 1 year


	LocalDate previousYear = today.minus(1, ChronoUnit.YEARS);
	System.out.println("Date before 1 year : " + previousYear);
	LocalDate nextYear = today.plus(1, YEARS); 
	System.out.println("Date after 1 year : " + nextYear);

	Output: 
	
		Date before 1 year : 2013-01-14
		Date after 1 year : 2015-01-14
		
		
### 10. Example 10 - Using Clock in Java 8

-	Clock is used to get Current Instant using Time, Date, Zone
-	Clock can be used in place of System.currentTimeInMillis()


	Code Snippet:
	
		// Returns the current time based on your system clock and set to UTC.
		Clock clock = Clock.systemUTC();
		System.out.println("Clock : " + clock);
		// Returns time based on system clock zone 
		Clock defaultClock = Clock.systemDefaultZone(); 
		System.out.println("Clock : " + clock);

		Output:
			
			Clock : SystemClock[Z]
			Clock : SystemClock[Z]


### 11.	Example 11 - How to see if a date is before or after another date in Java



	Code Snippet:
	
		LocalDate tomorrow = LocalDate.of(2014, 1, 15);
     
		if(tommorow.isAfter(today)){
			System.out.println("Tomorrow comes after today");
		}
			 
		LocalDate yesterday = today.minus(1, DAYS);

		if(yesterday.isBefore(today)){
			System.out.println("Yesterday is day before today");
		}
			 
		Output:
		Tomorrow comes after today
		Yesterday is day before today



### 12. 	Example 12 - Dealing with time zones in Java 8


	Code Snippet:
	
		// Date and time with timezone in Java 8 
		ZoneId america = ZoneId.of("America/New_York");
		LocalDateTime localtDateAndTime = LocalDateTime.now();
		ZonedDateTime dateAndTimeInNewYork = ZonedDateTime.of(localtDateAndTime, america );
		System.out.println("Current date and time in a particular timezone : " + dateAndTimeInNewYork); 
		
		Output : 
		
			Current date and time in a particular timezone : 2014-01-14T16:33:33.373-05:00[America/New_York]

			
### 13.	Example 13 - How to represent fixed date e.g. credit card expiry, YearMonth

		
	Code Snippets:
	
		YearMonth currentYearMonth = YearMonth.now();
		System.out.printf("Days in month year %s: %d%n", currentYearMonth, currentYearMonth.lengthOfMonth());
		YearMonth creditCardExpiry = YearMonth.of(2018, Month.FEBRUARY);
		System.out.printf("Your credit card expires on %s %n", creditCardExpiry);

		Output:

		Days in month year 2014-01: 31 Your credit card expires on 2018-02 


###	14.	Example 14 - How to check Leap Year in Java 8
	
	
	Code Snippets:
	
		if(today.isLeapYear()){ 
			System.out.println("This year is Leap year"); 
		}else { 
			System.out.println("2014 is not a Leap year"); 
		} 
		
		Output:
			
			2014 is not a Leap year


###	15.	Example 15 - How many days, month between two dates



	Code Snippets:
		
		LocalDate java8Release = LocalDate.of(2014, Month.MARCH, 14);
		Period periodToNextJavaRelease = Period.between(today, java8Release); 
		System.out.println("Months left between today and Java 8 release : " + periodToNextJavaRelease.getMonths() );


	Output:

		Months left between today and Java 8 release : 2


		
		
###	16. Example 16 - Date and Time with timezone offset

	
	Code Snippets:

		LocalDateTime datetime = LocalDateTime.of(2014, Month.JANUARY, 14, 19, 30);
		ZoneOffset offset = ZoneOffset.of("+05:30");
		OffsetDateTime date = OffsetDateTime.of(datetime, offset);
		System.out.println("Date and Time with timezone offset in Java : " + date); 
		
	Output :
		
		Date and Time with timezone offset in Java : 2014-01-14T19:30+05:30

		
### 17. Example 17 - How to get current timestamp in Java 8


-	Instant is used for machine readable format	


	Code Snippets:
	
		Instant timestamp = Instant.now();
		System.out.println("What is value of this instant " + timestamp);

		Output :
			
			What is value of this instant 2014-01-14T08:33:33.379Z   
		
		


###	18.	Example 18 -  How to parse/format date in Java 8 using predefined formatting



	Code Snippets:
	
		String dayAfterTommorrow = "20140116"; 
		LocalDate formatted = LocalDate.parse(dayAfterTommorrow, DateTimeFormatter.BASIC_ISO_DATE);
		System.out.printf("Date generated from String %s is %s %n", dayAfterTommorrow, formatted); 
		
	Output :

		Date generated from String 20140116 is 2014-01-16



###	19.	Example 19 - How to parse date in Java using custom formatting

	
	
	Code Snippets:
	
		String goodFriday = "Apr 18 2014";
		try { 

			DateTimeFormatter formatter = DateTimeFormatter.ofPattern("MMM dd yyyy"); 
			LocalDate holiday = LocalDate.parse(goodFriday, formatter); 
			System.out.printf("Successfully parsed String %s, date is %s%n", goodFriday, holiday); 

		} catch (DateTimeParseException ex) {
			System.out.printf("%s is not parsable!%n", goodFriday); 
			ex.printStackTrace();
		} 
		
	Output : 
	
		Successfully parsed String Apr 18 2014, date is 2014-04-18

	

	
###	20.	Example 20 - How to convert Date to String in Java 8, formatting dates


	Code Snippets:
	
		LocalDateTime arrivalDate  = LocalDateTime.now();
		try {
			DateTimeFormatter format = DateTimeFormatter.ofPattern("MMM dd yyyy  hh:mm a");
			String landing = arrivalDate.format(format);
			System.out.printf("Arriving at :  %s %n", landing);
		} catch (DateTimeException ex) {
			System.out.printf("%s can't be formatted!%n", arrivalDate);
			ex.printStackTrace();
		}
		
		
	Output : Arriving at :  Jan 14 2014  04:33 PM
	
	

	### 7.	Java 8 API utilities

-	Principle Date, Time classes provides various utility methods such as plus/minus days, weeks, months


#### Code Snippets:

```java
	LocalDate today = LocalDate.now();
	today.getYear();
	today.getMonth();
	today.isLeapYear();
	today.isBefore(LocalDate.of(2015,1,1));
	
	//Create LocalDateTime from LocalDate
	today.atTime(LocalTime.now());
	
	//Plus minus operations
	today.plusDays(10);
	today.plusWeeks(3);
	today.plusMonths(20);
	today.minusWeeks(4);
	today.minusDays(10);
	today.minusMonths(20);
	
	
	//Temporal adjusters for adjusting the dates
	
	LocalDate firstDayOfMonth =  today.with(TemporalAdjusters.firstDayOfMonth());
	LocalDate lastDayOfYear =   today.with(TemporalAdjusters.lastDayOfYear());
	
	Period period = today.until(lastDayOfYear);
	System.out.println("Period Format= "+period); //P8M3D
	System.out.println("Months remaining in the year= "+period.getMonths());	//Months remaining in the year= 8
````

### 21. Java 8 Date Parsing and Formatting

- 	Parsing Date across different formats
-	Converting String to Date and vice versa



#### Code Snippets:

```java
	LocalDateTime dateTime = LocalDateTime.now();
	//default format
	System.out.println("Default format of LocalDate="+date); // Default format of LocalDate=2014-04-28
	
	System.out.println("Default format of LocalDateTime="+dateTime);
	
	
	//specific format
	System.out.println(dateTime.format(DateTimeFormatter.ofPattern("d::MMM::uuuu HH::mm::ss")));
	System.out.println(dateTime.format(DateTimeFormatter.BASIC_ISO_DATE));
	
	// Output
	Default format of LocalDateTime=2014-04-28T16:25:49.341
	28::Apr::2014 16::25::49
	20140428
	
	Instant timestamp = Instant.now();
	//default format
	System.out.println("Default format of Instant="+timestamp);
	
	//Parse examples
	LocalDateTime dt = LocalDateTime.parse("27::Apr::2014 21::39::48", DateTimeFormatter.ofPattern("d::MMM::uuuu HH::mm::ss"));
	System.out.println("Default format after parsing = "+dt);
	
	
	// Output
	Default format of Instant=2014-04-28T23:25:49.342Z
	Default format after parsing = 2014-04-27T21:39:48
	
	
	// Parsing from Date to String
	LocalDateTime current = LocalDateTime.now();
	DateTimeFormatter format =  DateTimeFormatter.ofPattern("dd-MM-yyyy HH:mm:ss");
	String formatedDateTime = current.format(format);   
	
	Month month = current.getMonth(); 
	int day = current.getDayOfMonth(); 
	int seconds = current.getSecond(); 
 
	// Output 
	current date and time : 2018-04-09T06:21:10.410
	in foramatted manner 09-04-2018 06:21:10
	
	
	LocalDate date2 = LocalDate.of(1950,1,26); 
	System.out.println("the repulic day :"+date2);

	LocalDateTime specificDate =  
	current.withDayOfMonth(24).withYear(2016);	
````

### 22.	Java 8 Date API Legacy Date Time Support

-	Java 8 supports Legacy Java Date Time APIs, parsing from vice versa

	
--------------------------------------------------------------------------












































	
	
	
