
1. SUPPORTS Propagation

	For _SUPPORTS_, Spring first checks if an active transaction exists. If a transaction exists, then the existing transaction will be used. If there isn’t a transaction, it is executed non-transactional:
	
	@Transactional(propagation = Propagation.SUPPORTS)
	public void supportsExample(String user) { 
	    // ... 
	}

2. Collectors.partitioningBy  by true and false

	Map<Boolean, List<LocalBusiness>> partitionedResults = localBusinesses.stream()  
	        .collect(Collectors.partitioningBy(  
	                business -> TRADE_LICENCE_PREFIX.equalsIgnoreCase(business.getTradeLicencePrefix().getPrefix())  
	        ));

3.  Stream Concat:
	Stream.concat( list1.stream(),  list2.stream() ).collect(Collectors.toList());

4. Stream All Match:
		list.stream().allMatch(predicate);
		
5. IntStream range:
		IntStream.range(0, 6)  
        .mapToObj(i -> currentYearMonth.minusMonths(6 - i))  
        .map(date -> date.format(DateTimeFormatter.ofPattern("MMM")))  
        .collect(Collectors.toList());


6. removeIf method
	`fieldsToRemove.removeIf(field -> categoryFieldIds.contains(field.getId()));`

7. CollectionUtils.isNotEmpty()
8. StringUtils class methods
9. anyMatch method :
`localBusiness.getCategoryFields().removeIf(field -> fieldsToRemove.stream().anyMatch(fieldToRemove -> field.getFieldId().equals(fieldToRemove.getId())));`


10. Optional.ofNullable
	Returns an Optional describing the specified value, if non-null, otherwise returns an empty Optional.
	
11. Collectors.toMap function:
	Collector<T, ?, Map<K,U>> toMap(Function<? super T, ? extends K> keyMapper,
	  Function<? super T, ? extends U> valueMapper).
	  Example:
		  public Map<String, String> listToMap(List<Book> books) {
	    return books.stream().collect(Collectors.toMap(Book::getIsbn, Book::getName));
	}

12. Duplicate Key issue with Collectors.toMap function.
 List to Map with duplicate key:
		1. public Map<Integer, Book> listToMapWithDupKeyError(List<Book> books) {
	    return books.stream().collect(
	      Collectors.toMap(Book::getReleaseYear, Function.identity()));
	}
	@Test(expected = IllegalStateException.class)
	public void whenMapHasDuplicateKey_without_merge_function_then_runtime_exception() {
	    convertToMap.listToMapWithDupKeyError(bookList);
	}

13. Merge Function:
	1. Collector<T, ?, M> toMap(Function<? super T, ? extends K> keyMapper,
  Function<? super T, ? extends U> valueMapper,
  BinaryOperator<U> mergeFunction)
  
	Let’s introduce a merge function that indicates that, in the case of a collision, we keep the existing entry:

public Map<Integer, Book> listToMapWithDupKey(List<Book> books) {
    return books.stream().collect(Collectors.toMap(Book::getReleaseYear, Function.identity(),
      (existing, replacement) -> existing));
}

**we can return different _Map_ implementations**:

**List to ConcurrentMap**

Collector<T, ?, M> toMap(Function<? super T, ? extends K> keyMapper,
  Function<? super T, ? extends U> valueMapper,
  BinaryOperator<U> mergeFunction,
  Supplier<M> mapSupplier)
  
where the _mapSupplier_ is a function that returns a new, empty _Map_ with the results.


public Map<Integer, Book> listToConcurrentMap(List<Book> books) {
    return books.stream().collect(Collectors.toMap(Book::getReleaseYear, Function.identity(),
      (o1, o2) -> o1, ConcurrentHashMap::new));
}

14. Function.Identity Method:
	The Function. identity() method is **a static utility method that returns a function that always returns its input argument**. In simpler terms, it's a function where the output is always the same as the input.

15. Binary Operator:
	 BinaryOperator extends the BiFunction.
	 
	1. The `BinaryOperator` takes two arguments of the same type and returns a result of the same type of its arguments.
		
	@FunctionalInterface
	public interface BinaryOperator<T> extends BiFunction<T,T,T> {
	}

	 2. The `BiFunction` takes two arguments of any type, and returns a result of any type.
		 
		@FunctionalInterface
		public interface BiFunction<T, U, R> {
		      R apply(T t, U u);
		}

16. ClassUtils class
	ClassUtils.getUserClass(o).equals(ClassUtils.getUserClass(this))

17. Map putIfAbsent
18.  Collectors groupingBy:
	1. Grouping and Counting:
			import java.util.*;
		import java.util.stream.*;
		import static java.util.stream.Collectors.*;
		
		public class GroupingAndCountingExample {
		    public static void main(String[] args) {
		        List<String> items = Arrays.asList("apple", "banana", "cherry", "date", "apple", "banana", "apple");
		
		        Map<String, Long> groupedAndCounted = items.stream()
		                .collect(groupingBy(item -> item, counting()));
		
		        System.out.println(groupedAndCounted);
		    }
		}
		2. Grouping and Summarizing:
				`List<Person> people = Arrays.asList( new Person("Alice", 30), new Person("Bob", 25), new Person("Charlie", 30), new Person("David", 25) ); 
				
				Map<Integer, IntSummaryStatistics> ageSummary = people.stream() .collect(groupingBy(Person::getAge, summarizingInt(Person::getAge))); System.out.println(ageSummary);`
				
				{25=IntSummaryStatistics{count=2, sum=50, min=25, average=25.000000, max=25}, 30=IntSummaryStatistics{count=2, sum=60, min=30, average=30.000000, max=30}}



	19. Collectors.mapping
		In Java 8, the `Collectors.mapping` method is used in conjunction with other collectors to perform additional mapping transformations on the elements before collecting them. It is particularly useful when used with `groupingBy` to transform the grouped elements.
	
			import java.util.*;
			import java.util.stream.*;
			import static java.util.stream.Collectors.*;
			
			public class GroupingAndMappingExample {
			    public static void main(String[] args) {
			        List<String> items = Arrays.asList("apple", "banana", "cherry", "date");
			
			        Map<Integer, List<String>> groupedAndMapped = items.stream()
			                .collect(groupingBy(
			                        String::length, 
			                        mapping(String::toUpperCase, toList())
			                ));
			
			        System.out.println(groupedAndMapped);
			    }
			}
	
			{4=[DATE], 5=[APPLE], 6=[BANANA, CHERRY]}


			import java.util.*;
			import java.util.stream.*;
			import static java.util.stream.Collectors.*;
			
			public class GroupingAndMappingExample {
			    public static void main(String[] args) {
			        List<String> items = Arrays.asList("apple", "apricot", "banana", "avocado", "cherry", "date", "apple");
			
			        Map<Character, Set<String>> groupedByFirstLetterAndSet = items.stream()
			                .collect(groupingBy(
			                        item -> item.charAt(0), 
			                        mapping(String::toUpperCase, toSet())
			                ));
			
			        System.out.println(groupedByFirstLetterAndSet);
			    }
			}

		 {a=[APRICOT, AVOCADO, APPLE], b=[BANANA], c=[CHERRY], d=[DATE]}

20. Collectors.collectingAndThen
		In Java 8, the `Collectors.collectingAndThen` method is a powerful utility that allows you to perform an additional transformation on the result of a collection operation. It is particularly useful when you need to modify the result of a collector before returning it.

		### Syntax and Usage
		
		The `collectingAndThen` method takes two arguments:
		
		1. A `Collector` that performs the initial collection.
		2. A finishing function that transforms the result of the first collector.
		
		The method signature looks like this:
			public static <T, A, R, RR> Collector<T, A, RR> collectingAndThen(
			    Collector<T, A, R> downstream,
			    Function<R, RR> finisher)

		Example 1: Collecting to an Immutable List:

			import java.util.*;
			import java.util.stream.*;
			import static java.util.stream.Collectors.*;

			public class CollectingAndThenExample {
			    public static void main(String[] args) {
			        List<String> items = Arrays.asList("apple", "banana", "cherry", "date");
			
			        List<String> immutableList = items.stream()
			                .collect(collectingAndThen(
			                        toList(),
			                        Collections::unmodifiableList
			                ));
			
			        System.out.println(immutableList);
			        // Trying to modify the list will throw an UnsupportedOperationException
			        // immutableList.add("new item"); // Uncommenting this line will cause an exception
			    }
			}

		Example 2: Grouping and Counting, then Converting to Immutable Map:
		
			Let's consider a scenario where you want to group items by their length, count them, and then convert the resulting map to an immutable map.

			import java.util.*;
			import java.util.stream.*;
			import static java.util.stream.Collectors.*;
			
			public class CollectingAndThenExample {
			    public static void main(String[] args) {
			        List<String> items = Arrays.asList("apple", "banana", "cherry", "date", "apple", "banana", "apple");
			
			        Map<Integer, Long> immutableMap = items.stream()
			                .collect(collectingAndThen(
			                        groupingBy(String::length, counting()),
			                        Collections::unmodifiableMap
			                ));
			
			        System.out.println(immutableMap);
			        // Trying to modify the map will throw an UnsupportedOperationException
			        // immutableMap.put(5, 2L); // Uncommenting this line will cause an exception
			    }
			}


		Example 3: Collecting to a Custom Collection:
		
			You can also use `collectingAndThen` to collect elements into a custom collection. For instance, let's collect items into a `TreeSet` (which sorts elements) and then convert it to an immutable set.

			 import java.util.*;
			import java.util.stream.*;
			import static java.util.stream.Collectors.*;
			
			public class CollectingAndThenExample {
			    public static void main(String[] args) {
			        List<String> items = Arrays.asList("apple", "banana", "cherry", "date");
			
			        Set<String> immutableSortedSet = items.stream()
			                .collect(collectingAndThen(
			                        toCollection(TreeSet::new),
			                        Collections::unmodifiableSet
			                ));
			
			        System.out.println(immutableSortedSet);
			        // Trying to modify the set will throw an UnsupportedOperationException
			        // immutableSortedSet.add("new item"); // Uncommenting this line will cause an exception
			    }
			}


		Example 4: Using Custom Finisher:

			In some cases, you might want to perform more complex transformations as the finishing step. For example, you could collect the lengths of strings and then return the maximum length found.
			
			import java.util.*;
			import java.util.stream.*;
			import static java.util.stream.Collectors.*;
			
			public class CollectingAndThenExample {
			    public static void main(String[] args) {
			        List<String> items = Arrays.asList("apple", "banana", "cherry", "date");
			
			        int maxLength = items.stream()
			                .collect(collectingAndThen(
			                        mapping(String::length, toList()),
			                        lengths -> {
				                     System.out.println(lengths);
			                         return Collections.max(lengths);
			                        } 
			                ));
			
			        System.out.println("Max Length: " + maxLength);
			    }
			}

			Output:
			[5, 6, 6, 4]
			Max Length: 6


21. FlatMap method:
	The `flatMap` method in Java 8 is a powerful tool in the `Stream` API that allows you to transform and flatten the elements of a stream. This method is particularly useful when you have a stream of collections or arrays and you want to process all their elements as a single stream.

### How `flatMap` Works

The `flatMap` method takes a function that maps each element of the stream to a new stream. It then flattens these nested streams into a single stream.

### Syntax

The syntax of the `flatMap` method is as follows:
		<R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper)


	Example 1: Flattening a List of Lists:
	
			import java.util.*;
			import java.util.stream.*;
			
			public class FlatMapExample {
			    public static void main(String[] args) {
			        List<List<Integer>> listOfLists = Arrays.asList(
			                Arrays.asList(1, 2, 3),
			                Arrays.asList(4, 5),
			                Arrays.asList(6, 7, 8, 9)
			        );
			
			        List<Integer> flatList = listOfLists.stream()
			                .flatMap(Collection::stream)
			                .collect(Collectors.toList());
			
			        System.out.println(flatList);
			    }
			}



	Example 2: Flattening a List of Arrays:


			import java.util.*;
			import java.util.stream.*;
			
			public class FlatMapExample {
			    public static void main(String[] args) {
			        List<String[]> listOfArrays = Arrays.asList(
			                new String[]{"apple", "banana"},
			                new String[]{"cherry", "date"},
			                new String[]{"elderberry", "fig", "grape"}
			        );
			
			        List<String> flatList = listOfArrays.stream()
			                .flatMap(Arrays::stream)
			                .collect(Collectors.toList());
			
			        System.out.println(flatList);
			    }
			}

	Example 3: Flattening and Processing Nested Elements:

			import java.util.*;
			import java.util.stream.*;
			
			public class FlatMapExample {
			    public static void main(String[] args) {
			        List<String> sentences = Arrays.asList(
			                "Hello world",
			                "Java 8 streams are powerful",
			                "FlatMap is very useful"
			        );
			
			        List<String> words = sentences.stream()
			                .flatMap(sentence -> Arrays.stream(sentence.split(" ")))
			                .collect(Collectors.toList());
			
			        System.out.println(words);
			    }
			}


	Example 4: Using `Optional` with `flatMap`:


			import java.util.*;
			public class FlatMapOptionalExample {
			    public static void main(String[] args) {
			        Optional<String> optional = Optional.of("Hello");
			        Optional<String> transformedOptional = optional
			                .flatMap(value -> Optional.of(value.toUpperCase()));
			
			        System.out.println(transformedOptional);
			    }
			}


22. `Arrays.stream` method:
		The `Arrays.stream` method in Java 8 is used to create a sequential stream from an array. It is part of the `java.util.Arrays` class and provides a convenient way to process arrays using the Stream API, which allows for functional-style operations on streams of elements.


		Basic Usage

		The `Arrays.stream` method has several overloads to handle different types of arrays (e.g., object arrays, int arrays, long arrays, and double arrays).
		
		Example 1: Stream from an Object Array:


			import java.util.Arrays;
			import java.util.List;
			import java.util.stream.Collectors;

			public class ArraysStreamExample {
			    public static void main(String[] args) {
			        String[] fruits = {"apple", "banana", "cherry", "date"};
			
			        List<String> fruitList = Arrays.stream(fruits)
			                .map(String::toUpperCase)   // Convert each string to uppercase
			                .collect(Collectors.toList()); // Collect the results into a list
			
			        System.out.println(fruitList);
			    }
			}

		 Example 2: Stream from a Primitive Array:


			import java.util.Arrays;
			public class ArraysStreamExample {
			    public static void main(String[] args) {
			        int[] numbers = {1, 2, 3, 4, 5};
			
			        int sum = Arrays.stream(numbers)
			                .sum(); // Sum all the elements
			
			        System.out.println("Sum: " + sum);
			    }
			}


		  Example 3: Stream from a Subarray:
		  
			import java.util.Arrays;
			public class ArraysStreamExample {
			    public static void main(String[] args) {
			        int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
			
			        int sumOfRange = Arrays.stream(numbers, 2, 7) // Stream from index 2 to 6 (7 is exclusive)
			                .sum();
			
			        System.out.println("Sum of range: " + sumOfRange);
			    }
			}

		Example 4: Combining Arrays.stream with flatMap:

				import java.util.Arrays;
				import java.util.List;
				import java.util.stream.Collectors;
				
				public class ArraysStreamExample {
				    public static void main(String[] args) {
				        String[][] data = {
				                {"a", "b", "c"},
				                {"d", "e", "f"},
				                {"g", "h", "i"}
				        };
				
				        List<String> flatList = Arrays.stream(data)
				                .flatMap(Arrays::stream)
				                .collect(Collectors.toList());
				
				        System.out.println(flatList);
				    }
				}

Pattern compile:

	Pattern.compile(regex, Pattern.CASE_INSENSITIVE).matcher(email).find() returns boolean.












