1. 非变, trait Queue[T] {} 
这是非变情况。这种情况下，当类型S是类型A的子类型，则Queue[S]不可认为是Queue[A]的子类型或父类型，这种情况是和Java一样的。 
2. 协变, trait Queue[+T] {} 
这是协变情况。这种情况下，当类型S是类型A的子类型，则Queue[S]也可以认为是Queue[A}的子类型，即Queue[S]可以泛化为Queue[A]。也就是被参数化类型的泛化方向与参数类型的方向是一致的，所以称为协变。 
```
class VendingMachine[+A] {
  // .. don't worry about implementation yet
}

def install(softDrinkVM: VendingMachine[SoftDrink]): Unit = {
  // Installs the soft drink vending machine
}

// covariant subtyping
install(new VendingMachine[Cola])
install(new VendingMachine[TonicWater])
// invariant
install(new VendingMachine[SoftDrink])

// Compile error ! contravariant subtyping
install(new VendingMachine[Drink])
```
3. 逆变, trait Queue[-T] {} 
这是逆变情况。这种情况下，当类型S是类型A的子类型，则Queue[A]反过来可以认为是Queue[S]的子类型。也就是被参数化类型的泛化方向与参数类型的方向是相反的，所以称为逆变。
```
class GarbageCan[-A] {
  // .. don't worry about implementation yet
}

def setGarbageCanForPlastic(gc: GarbageCan[PlasticItem]): Unit = {
  // sets garbage can for PlasticItem items
}

// contravariant subtyping
setGarbageCanForPlastic(new GarbageCan[Item])

// invariant
setGarbageCanForPlastic(new GarbageCan[PlasticItem])

// Compile error ! covariant subtyping
setGarbageCanForPlastic(new GarbageCan[PlasticBottle])
```

Another Sample
```
class GParent 

class Parent extends GParent

class Child extends Parent

class Box[+A]

class Box2[-A]

def foo(x : Box[Parent]) : Box[Parent] = identity(x)

def bar(x : Box2[Parent]) : Box2[Parent] = identity(x)

foo(new Box[Child]) // success

foo(new Box[GParent]) // type error

bar(new Box2[Child]) // type error

bar(new Box2[GParent]) // success
```
4. 类型下界, U >: T
这是类型下界的定义，也就是U必须是类型T的父类(或本身，自己也可以认为是自己的父类)。
5. 类型上届, S <: T
这是类型上界的定义，也就是S必须是类型T的子类（或本身，自己也可以认为是自己的子类)。
```
abstract class Animal {
 def name: String
}

abstract class Pet extends Animal {}

class Cat extends Pet {
  override def name: String = "Cat"
}

class Dog extends Pet {
  override def name: String = "Dog"
}

class Lion extends Animal {
  override def name: String = "Lion"
}

class PetContainer[P <: Pet](p: P) {
  def pet: P = p
}

val dogContainer = new PetContainer[Dog](new Dog)
val catContainer = new PetContainer[Cat](new Cat)

// this would not compile
val lionContainer = new PetContainer[Lion](new Lion)
```

1. 泛型函数
ClassTag[T]保存了泛型擦除后的原始类型T,提供给被运行时的。
```
/*
     *  泛型[]，中括号F、S、T都表示运行时参数类型，
     * ClassTag[T]保存了泛型擦除后的原始类型T,提供给被运行时的。
     */
    class Triple[F: ClassTag, S, T](val first: F, val second: S, val third: T)
    
    object HelloTypeParam {
      def main(args: Array[String]): Unit = {
    
        // 运行执行代码：val triple: Triple[String, Int, Double]
        val triple = new Triple("Spark", 3, 3.1415)
        
        // 运行执行代码：val bigData: Triple[String, String, Char]
        val bigData = new Triple[String, String, Char]("Spark", "Hadoop", 'R');
        
        // getData函数传入泛型为T的运行时List类型参数，返回list.length / 2的整数。
        def getData[T](list:List[T]) = list(list.length / 2)
        // List索引从0开始，执行结果：Hadoop
        println(getData(List("Spark","Hadoop",'R')));
        
        // 获得getData函数引用
        val f = getData[Int] _
        // 调用getData函数，执行结果：4
        println(f(List(1,2,3,4,5,6)));
        
      }
    }
```

2. 类型变量界定
泛型参数类型限定，限定具体类的可以调用特定的方法。
```
/*
     * <:泛型类型限定符，表示只限定Comparable子类
     * Comparable[T]:为T下界，T:为Comparable[T]上界
     */
    class Pair[T <: Comparable[T]](val first: T, val second: T) {
      // compareTo方法进行比较，如果大于0返回first
      def bigger = if (first.compareTo(second) > 0) first else second
    }
    
    
    // 声明带T泛型参数的类
    class Pair_Lower_Bound[T](val first: T, val second: T) {
        // 传入的参数泛型T 必须为 R的父类(超类),返回构造Pair_Lower_Bound对象 
        // R:为T的上界，T:为R下界  
        def replaceFirst[R>:T](newFirst:R) = new Pair_Lower_Bound[R](newFirst,second)
    }
    
    object TypeVariablesBounds {
      def main(args: Array[String]): Unit = {
        // 函数调用
        var pair = new Pair("Spark", "Hadoop")
        // 执行结果：Spark
        println(pair.bigger)
      }
    }
```

3. 泛型视图限定

泛型视图限定：表示把传入不是Comparable[T]类型的隐式传换为Comparable[T]类型，Comparable[T]:为T下界，T:为Comparable[T]上界。
```
/*
     * <%泛型视图限定符，表示把传入不是Comparable[T]类型的 隐式传换 为Comparable[T]类型
     * Comparable[T]:为T下界，T:为Comparable[T]上界
     */
    class PairNotPerfect[T <% Comparable[T]](val first: T, val second: T) {
      // compareTo方法进行比较，如果大于0返回first
      def bigger = if (first.compareTo(second) > 0) first else second
    }
    
    /*
     * <%泛型视图限定符，表示把传入不是Ordered[T]类型的 隐式传换 为Ordered[T]类型
     * Ordered[T]:为T下界，T:为Ordered[T]上界
     * Ordered继承： extends Any with java.lang.Comparable[A]
     */
    class PairBetter[T <% Ordered[T]](val first: T, val second: T) {
      def bigger = if (first.compareTo(second) > 0) first else second
    }
    
    object ViewVariablesBounds {
      def main(args: Array[String]): Unit = {
        // 函数调用
        var pair = new PairNotPerfect("Spark", "Hadoop");
        // 执行结果：Spark
        println(pair.bigger)
        
        // 函数调用,Int类型进行隐式转换，将Int -> RichInt,RichInt实现了Comparable接口
        var pairInt = new PairNotPerfect(3,5)
        // 执行结果：5
        println(pairInt.bigger);
        
        // 函数调用,Int类型进行隐式转换，将String -> RichString,RichString实现了Comparable接口
        var pairBetterStr = new PairBetter("Java","Scala");
        println(pairBetterStr.bigger);
        
        // 函数调用
        var pairBetterInt = new PairBetter(20, 12);
        // 执行结果：Spark
        println(pairBetterInt.bigger)
      }
    }
```

4. 上下文界定
上下文界定：上下文界定是隐式参数的语法糖。如：Ordering：可以进行隐式转化的T类型。
```
class PairOrdering[T: Ordering](val first: T, val second: T) {
      // compareTo方法进行比较，如果大于0返回first
      def bigger(implicit ordered: Ordering[T]) = if (ordered.compare(first, second) > 0) first else second
    }
    
    object ContextBounds {
      def main(args: Array[String]): Unit = {
        // 函数调用
        var pair = new PairOrdering("Spark", "Hadoop")
        // 执行结果：Spark
        println(pair.bigger)
      }
    }
```

5. Manifest关键字
Manifest关键字：数组在声明时必须要求指定具体的类型，在函数泛型是无法知道具体类型，通过Manifest关键字使得运行时可以根据这个Manifest参数做更多的事情。
```
def main(args: Array[String]): Unit = {
    /*
     *  定义方法array
     *  Manifest:需要运行时存储T的实际类型，运行时是做为参数运行在方法的上下文中。
     *  数组在定义时必须知道具体的类型，所以在声明方法时，需要添加Manifest
     */
    def arrayMake[T: Manifest](first: T, second: T) = {
      val r = new Array[T](2);
      r(0) = first;
      r(1) = second;
      // 返回r
      r;
    }

    /*
     *  执行结果：
     *  1
     *  2
     */
    arrayMake(1,2).foreach(println)
  }
```

6. ClassTag关键字

ClassTag[T]保存了泛型擦除后的原始类型T,提供给被运行时的。
```
def main(args: Array[String]): Unit = { 
     
        /*
         * ClassTag:在运行时指定，在编译时无法确定的
         */
        def mkArray[T:ClassTag](elems:T*) = Array[T](elems:_*)
        
        /*
         *  执行结果：
         *  42
         *  13
         */
        mkArray(42,13).foreach(println)
        
        /*
         * 执行结果：
         * Japan
         * Brazil
         * Germany
         */
        mkArray("Japan","Brazil","Germany").foreach(println)
    }
```

7. ClassManifest关键字
在引入Manifest的时候，还引入了一个更弱一点的ClassManifest，所谓的弱是指类型信息不如Manifest那么完整。用TypeTag替代了Manifest，用ClassTag替代了ClassManifest，原因是在路径依赖类型中，Manifest存在问题。
```
class A[T]
    val m = manifest[A[String]]
    // 执行结果：com.scala.type_param.Manifest_ClassTag$A$1[java.lang.String]
    println(m);
    val cm = classManifest[A[String]]
    // 执行结果：com.scala.type_param.Manifest_ClassTag$A$1[java.lang.String]
    println(cm);
```

8. 多重界定符
```
/*
      // 表示：A和B为T上界
      T <: A with B
      
      // 表示：A和B为T下界
      T >: A with B
      
      // 表示：同时拥有上界和下界，并且A为下界，B为上界，A为B的子类，顺序不能颠倒。
      T >: A <: B
      
      // 表示：类型变量界定，即同时满足AT这种隐式值和BT这种隐式值
      T:A:B
      
      // 表示：视图界定，即同时能够满足隐式转换的A和隐式转换的B
      T <% A <% B 
    */
```

9. Scala类型约束
```
def main(args: Array[String]): Unit = {
      // A =:=B // 表示A类型等同于B类型
      // A <:<B  // 表示A类型是B类型的子类
      def rocky[T](i: T)(implicit ev: T <:< java.io.Serializable) {
        // 执行结果：Life is short ,you need spark!!!
        println("Life is short ,you need spark!!!")
      }
      rocky("Spark")
    }
```