

# Java 8 Examples of Lambda Expressions

## Stream API

-	Sequence of Elements
-	Source
-	Aggregate Operations
	
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
#### 7.	  convert Stream to List
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

			public class ListStringSortingLambda {
				public static void main(String[] args) {
					String[] stringArray = new String[] { "ffff", "rrr", "dddd", "aaaa", "bbbbbb" };
					List<String> stringList = Arrays.asList(stringArray);

					Comparator<String> c = (s1, s2) -> s1.compareTo(s2);
					stringList.sort(c);
					System.out.println(stringList);
				}
			}
			
#####		Output :
			[aaaa, bbbbbb, dddd, ffff, rrr]						
			
####	3.	Method Reference
	
#####		Code Snippets
		
			public class ListStringSortingMethodReference {
				public static void main(String[] args) {
					String[] stringArray = new String[] { "ffff", "rrr", "dddd", "aaaa", "bbbbbb" };
					List<String> stringList = Arrays.asList(stringArray);
					
					Comparator<String> stringComparator = new StringComparator();
					stringList.sort(stringComparator::compare);
					System.out.println(stringList);
				}
			}
			
#####		Output :
			[aaaa, bbbbbb, dddd, ffff, rrr]

### 3.	List of Custom Objects
	
	
####	1.	Implementing Comparator Interface using it on List.sort() method
	
#####		Code Snippets :
		
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


----------------------------------------------------------------------------------------
## 8.	filter NULL values stream

	Code Snippet :
	
		Stream<String> languages = Stream.of("java", "python", "node", null, "ruby", null, "php");
		
		languages.collect(Collectors.toList());
		
		// Filtering null values 
		
		languages.filter(l -> l != null).collect(Collectors.toSet());
		
		// Filtering Null Values using Objects.nonNull() static method
		
		languages.filter(Objects::nonNull).collect(Collectors.toList());




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
		
		
	-	Code Samples
	
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
	
	
	
	
	
