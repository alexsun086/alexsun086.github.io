## Java
### Covariant
Sub.getSomething() is a covariant
````
class Super {
  Object getSomething(){}
}
class Sub extends Super {
  String getSomething() {}
}
````

### Contravariant
Sub.getSomething() is a contravariant
````
class Super{
  void doSomething(String parameter)
}
class Sub extends Super{
  void doSomething(Object parameter)
}
````

### Generics
````
List<String> aList...
List<? extends Object> covariantList = aList;
List<? super String> contravariantList = aList;
covariantList.add("d"); //wrong
Object a = covariantList.get(0);
contravariantList.add("d"); //OK
String b = contravariantList.get(1); //wrong
Object c = contravariantList.get(2);
````

## Scala
### Covariant
````
class Animal {}
class Bird extends Animal {}
class Consumer[+T](t: T) {
}
class Test extends App {
	val c:Consumer[Bird] = new Consumer[Bird](new Bird)
	val c2:Consumer[Animal] = c
}
````

### Contravariant
````
class Animal {}
class Bird extends Animal {}
class Consumer[-T](t: T) {
}
class Test extends App {
	val c:Consumer[Bird] = new Consumer[Bird](new Bird)
	val c2:Consumer[Hummingbird] = c
}
````

### lower bounds
Compile error:"Covariant type T occurs in contravariant position in type T of value t"
````
class Consumer[+T](t: T) {
 	def use(t: T) = {}
}
````
````
class Consumer[+T](t: T)(implicit m1:Manifest[T]) {
 	def get(): T = {m1.runtimeClass.newInstance.asInstanceOf[T]}
}
````
````
class Consumer[+T](t: T) {
	def use[U >: T](u : U) = {println(u)}
}
````

### Upper bounds
````

2
3
class Consumer[-T](t: T) {
	def get[U <: T]()(implicit m1:Manifest[U]): U = {m1.runtimeClass.newInstance.asInstanceOf[U]}
}
````

### All in one example
````
class Animal {}
class Bird extends Animal {}
class Consumer[-S,+T]()(implicit m1:Manifest[T]) {
	def m1[U >: T](u: U): T = {m1.runtimeClass.newInstance.asInstanceOf[T]} //协变，下界
	def m2[U <: S](s: S)(implicit m2:Manifest[U]): U = {m1.runtimeClass.newInstance.asInstanceOf[U]} //逆变，上界
}
class Test extends App {
	val c:Consumer[Animal,Bird] = new Consumer[Animal,Bird]()
	val c2:Consumer[Bird,Animal] = c
	c2.m1(new Animal)
	c2.m2(new Bird)
}
````
