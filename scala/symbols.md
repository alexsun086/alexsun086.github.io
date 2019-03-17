<-, for循环
```
for (arg <- args)  
println(arg)

for (i <- 0 to 2)  
print(greetStrings(i))
```
->, map映射
```
object Test {
   def main(args: Array[String]) {
      val colors = Map("red" -> "#FF0000",
                       "azure" -> "#F0FFFF",
                       "peru" -> "#CD853F")
 
      val nums: Map[Int, Int] = Map()
 
      println( "colors 中的键为 : " + colors.keys )
      println( "colors 中的值为 : " + colors.values )
      println( "检测 colors 是否为空 : " + colors.isEmpty )
      println( "检测 nums 是否为空 : " + nums.isEmpty )
   }
}
```
=>, 方法参数=> 方法体,匿名函数
```
val l = List(1,2,3)
var ll = l.map(x => x*x)//返回 ll=(1,4,9)
```
模式语句case与后面表达式的分隔符
```
a match {
case 1 => "match 1"
case _ => "match _"
}
```
创建函数实例的语法糖  
例如：A => T，A,B => T表示一个函数的输入参数类型是“A”，“A,B”，返回值类型是T。请看下面这个实例：
```
scala> val f: Int => String = myInt => "The value of myInt is: " + myInt.toString()
f: Int => String = <function1>

scala> println(f(3))
The value of myInt is: 3
```
上面例子定义函数f：输入参数是整数类型，返回值是字符串。    
另外，() => T表示函数输入参数为空，而A => Unit则表示函数没有返回值。

_ 下划线
- 作为“通配符”，类似Java中的*。如import scala.math._
- :_*作为一个整体，告诉编译器你希望将某个参数当作参数序列处理！例如val s = sum(1 to 5:_*)就是将1 to 5当作参数序列处理。
- 指代一个集合中的每个元素。例如我们要在一个Array a中筛出偶数，并乘以2，可以用以下办法：
a.filter(_%2==0).map(2*_)。
- 在元组中，可以用方法_1, _2, _3访问组员。如a._2。其中句点可以用空格替代。
- 使用模式匹配可以用来获取元组的组员，例如val (first, second, third) = t
但如果不是所有的部件都需要，那么可以在不需要的部件位置上使用_。比如上一例中val (first, second, _) = t    
- 下划线_代表的是某一类型的默认值。     
对于Int来说，它是0。    
对于Double来说，它是0.0    
对于引用类型，它是null。

<: 与 >: 上下界约束符号 
```
def using[A <: Closeable, B](closeable: A) (getB: A => B): B =
  try { 
    getB(closeable)
  } finally {
    closeable.close() 
  }
```
例子中A <: Closeable(java.io.Cloaseable)的意思就是保证类型参数A是Closeable的子类（含本类），语法“A <: B"定义了B为A的上界；同理相反的A>:B的意思就是A是B的超类（含本类），定义了B为A的下界。
其实<: 和 >: 就等价于java范型编程中的 extends，super

<% 视界   
它比<:适用的范围更广，除了所有的子类型，还允许隐式转换过去的类型