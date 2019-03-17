## Generic Class
```
// A Simple Java program to show working of user defined 
// Generic classes 
   
// We use < > to specify Parameter type 
class Test<T> 
{ 
    // An object of type T is declared 
    T obj; 
    Test(T obj) {  this.obj = obj;  }  // constructor 
    public T getObject()  { return this.obj; } 
} 
   
// Driver class to test above 
class Main 
{ 
    public static void main (String[] args) 
    { 
        // instance of Integer type 
        Test <Integer> iObj = new Test<Integer>(15); 
        System.out.println(iObj.getObject()); 
   
        // instance of String type 
        Test <String> sObj = 
                          new Test<String>("GeeksForGeeks"); 
        System.out.println(sObj.getObject()); 
    } 
}
```

```
// A Simple Java program to show multiple 
// type parameters in Java Generics 

// We use < > to specify Parameter type 
class Test<T, U> 
{ 
	T obj1; // An object of type T 
	U obj2; // An object of type U 

	// constructor 
	Test(T obj1, U obj2) 
	{ 
		this.obj1 = obj1; 
		this.obj2 = obj2; 
	} 

	// To print objects of T and U 
	public void print() 
	{ 
		System.out.println(obj1); 
		System.out.println(obj2); 
	} 
} 

// Driver class to test above 
class Main 
{ 
	public static void main (String[] args) 
	{ 
		Test <String, Integer> obj = 
			new Test<String, Integer>("GfG", 15); 

		obj.print(); 
	} 
}
```

## Generic Function
```
// A Simple Java program to show working of user defined 
// Generic functions 

class Test 
{ 
	// A Generic method example 
	static <T> void genericDisplay (T element) 
	{ 
		System.out.println(element.getClass().getName() + 
						" = " + element); 
	} 

	// Driver method 
	public static void main(String[] args) 
	{ 
		// Calling generic method with Integer argument 
		genericDisplay(11); 

		// Calling generic method with String argument 
		genericDisplay("GeeksForGeeks"); 

		// Calling generic method with double argument 
		genericDisplay(1.0); 
	} 
}
```

Advantages of Generics:

Programs that uses Generics has got many benefits over non-generic code.
1. Code Reuse: We can write a method/class/interface once and use for any type we want.
2. Type Safety : Generics make errors to appear compile time than at run time (It’s always better to know problems in your code at compile time rather than making your code fail at run time). Suppose you want to create an ArrayList that store name of students and if by mistake programmer adds an integer object instead of string, compiler allows it. But, when we retrieve this data from ArrayList, it causes problems at runtime.

```
// Using generics converts run time exceptions into 
// compile time exception. 
import java.util.*; 

class Test 
{ 
	public static void main(String[] args) 
	{ 
		// Creating a an ArrayList with String specified 
		ArrayList <String> al = new ArrayList<String> (); 

		al.add("Sachin"); 
		al.add("Rahul"); 

		// Now Compiler doesn't allow this 
		al.add(10); 

		String s1 = (String)al.get(0);  //Typecasting is not needed 
		String s2 = (String)al.get(1);  //String s1 = al.get(0);
		String s3 = (String)al.get(2); 
	} 
} 
```

##Upper Bounded

The question mark (?), represents the wildcard, stands for unknown type in generics. There may be times when you'll want to restrict the kinds of types that are allowed to be passed to a type parameter. For example, a method that operates on numbers might only want to accept instances of Number or its subclasses.

To declare a upper bounded Wildcard parameter, list the ?, followed by the extends keyword, followed by its upper bound.

```
import java.util.Arrays;
import java.util.List;

public class GenericsTester {

   public static double sum(List<? extends Number> numberlist) {
      double sum = 0.0;
      for (Number n : numberlist) sum += n.doubleValue();
      return sum;
   }

   public static void main(String args[]) {
      List<Integer> integerList = Arrays.asList(1, 2, 3);
      System.out.println("sum = " + sum(integerList));

      List<Double> doubleList = Arrays.asList(1.2, 2.3, 3.5);
      System.out.println("sum = " + sum(doubleList));
   }
}
```

## Unbounded
There may be times when any object can be used when a method can be implemented using functionality provided in the Object class or When the code is independent of the type parameter.

To declare a Unbounded Wildcard parameter, list the ? only.
```
import java.util.Arrays;
import java.util.List;

public class GenericsTester {
   public static void printAll(List<?> list) {
      for (Object item : list)
         System.out.println(item + " ");
   }

   public static void main(String args[]) {
      List<Integer> integerList = Arrays.asList(1, 2, 3);
      printAll(integerList);
      List<Double> doubleList = Arrays.asList(1.2, 2.3, 3.5);
      printAll(doubleList);
   }
}
```

## Lower Bounded
There may be times when you'll want to restrict the kinds of types that are allowed to be passed to a type parameter. For example, a method that operates on numbers might only want to accept instances of Integer or its superclasses like Number.

To declare a lower bounded Wildcard parameter, list the ?, followed by the super keyword, followed by its lower bound.

```
import java.util.ArrayList;
import java.util.List;

public class GenericsTester {

   public static void addCat(List<? super Cat> catList) {
      catList.add(new RedCat());
      System.out.println("Cat Added");
   }

   public static void main(String[] args) {
      List<Animal> animalList= new ArrayList<Animal>();
      List<Cat> catList= new ArrayList<Cat>();
      List<RedCat> redCatList= new ArrayList<RedCat>();
      List<Dog> dogList= new ArrayList<Dog>();

      //add list of super class Animal of Cat class
      addCat(animalList);

      //add list of Cat class
      addCat(catList);

      //compile time error
      //can not add list of subclass RedCat of Cat class
      //addCat(redCatList);

      //compile time error
      //can not add list of subclass Dog of Superclass Animal of Cat class
      //addCat.addMethod(dogList); 
   }
}
class Animal {}

class Cat extends Animal {}

class RedCat extends Cat {}

class Dog extends Animal {}
```