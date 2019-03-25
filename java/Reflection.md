### 反射机制是什么
反射机制就是在运行状态（区别于编译期(CompileTime)之外的运行期(Runtime)）中，对于任意一个类，都能够知道这个类的所有属性和方法（即字节码，包括接口、变量、方法等信息）；对于任意一个对象，都能够调用它的任意一个方法和属性（在运行期实例化对象，通过调用get/set方法获取变量的值）；这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制。

用一句话总结就是反射可以实现在运行时可以知道任意一个类的属性和方法。

### 反射机制能做什么
- 在运行时判断任意一个对象所属的类；
- 在运行时构造任意一个类的对象；
- 在运行时判断任意一个类所具有的成员变量和方法；
- 在运行时调用任意一个对象的方法；
- 生成动态代理(Java设计模式-代理模式)

### Java反射机制应用场景
- 逆向代码 ，例如反编译
- 与注解相结合的框架 例如Retrofit
- 单纯的反射机制应用框架 例如EventBus
- 动态生成类框架 例如Gson
- 后端框架Spring，Status…

### 反射机制的优点与缺点
- 静态编译：在编译时确定类型，绑定对象。即一次性编译。在编译的时候把你所有的模块都编译进去
- 动态编译：运行时确定类型，绑定对象。动态编译最大限度发挥了Java的灵活性，体现了多态的应用，有以降低类之间的藕合性。也可以说是按需编译。程序在运行的时候，用到那个模块就编译哪个模块。

### 通过已有的类得到一个字节码 (Class)对象，共有三种方式
````
Class c1 = Code.class;//这说明任何一个类都有一个隐含的静态成员变量class，这种方式是通过获取类的静态成员变量class得到的
Class c2 = code1.getClass(); //code1是Code的一个对象，这种方式是通过一个类的对象的getClass()方法获得的 
Class c3 = Class.forName("com.trigl.reflect.Code"); //这种方法是Class类调用forName方法，通过一个类的全量限定名获得。如果写错类的路径会报 ClassNotFoundException 的异常。

public class ReflectDemo {
    public static void main(String[] args) throws ClassNotFoundException {
        //第一种：Class c1 = Code.class;
        Class class1 = ReflectDemo.class;
        System.out.println(class1.getName());

        //第二种：Class c2 = code1.getClass();
        ReflectDemo demo2 = new ReflectDemo();
        Class c2 = demo2.getClass();
        System.out.println(c2.getName());

        //第三种：Class c3 = Class.forName("com.trigl.reflect.Code");
        Class class3 = Class.forName("com.tengj.reflect.ReflectDemo");
        System.out.println(class3.getName());
    }
}
````

### 利用反射机制能获得什么信息（主要）
#### 获取 Class 对象
````
Class c = Class.forName("className");
Object obj = c.newInstance();//创建对象的实例
````

#### 获得构造函数
````
Constructor getConstructor(Class[] params)//根据指定参数获得public构造器

Constructor[] getConstructors()//获得public的所有构造器

Constructor getDeclaredConstructor(Class[] params)//根据指定参数获得public和非public的构造器

Constructor[] getDeclaredConstructors()//获得public的所有构造器
````


#### 获得类方法
````
//两个参数分别是方法名和方法参数类的类类型列表。
Method getMethod(String name, Class[] params),根据方法名，参数类型获得方法，

Method[] getMethods()//获得所有的public方法

Method getDeclaredMethod(String name, Class[] params)//根据方法名和参数类型，获得public和非public的方法

Method[] getDeclaredMethods()//获得所以的public和非public方法
````

#### 获得类中属性
````
Field getField(String name)//根据变量名得到相应的public变量

Field[] getFields()//获得类中所以public的方法

Field getDeclaredField(String name)//根据方法名获得public和非public变量

Field[] getDeclaredFields()//获得类中所有的public和非public方法
````

### 利用反射机制能获得什么信息（详细）
#### Class 对象
````
String className = ... ;//在运行期获取的类名字符串
Class class = Class.forName(className);
````

#### 类名
````
Class aClass = ... //获取Class对象，具体方式可见Class对象小节
String className = aClass.getName();

Class aClass = ... //获取Class对象，具体方式可见Class对象小节
String simpleClassName = aClass.getSimpleName();
````

#### 修饰符
````
Class aClass = ... //获取Class对象，具体方式可见Class对象小节
int modifiers = aClass.getModifiers();

Modifier.isAbstract(int modifiers);
Modifier.isFinal(int modifiers);
Modifier.isInterface(int modifiers);
Modifier.isNative(int modifiers);
Modifier.isPrivate(int modifiers);
Modifier.isProtected(int modifiers);
Modifier.isPublic(int modifiers);
Modifier.isStatic(int modifiers);
Modifier.isStrict(int modifiers);
Modifier.isSynchronized(int modifiers);
Modifier.isTransient(int modifiers);
Modifier.isVolatile(int modifiers);
````

#### 包信息
````
Class  aClass = ... //获取Class对象，具体方式可见Class对象小节
Package package = aClass.getPackage();
````

#### 父类
````
Class superclass = aClass.getSuperclass();
````

#### 实现的接口
````
Class  aClass = ... //获取Class对象，具体方式可见Class对象小节
Class[] interfaces = aClass.getInterfaces();
````

#### 构造器
````
Class aClass = ...//获取Class对象
Constructor[] constructors = aClass.getConstructors();

Class aClass = ...//获取Class对象
Constructor constructor =
aClass.getConstructor(new Class[]{String.class});

Constructor constructor = ... //获取Constructor对象
Class[] parameterTypes = constructor.getParameterTypes();

Constructor constructor = MyObject.class.getConstructor(String.class);
MyObject myObject = (MyObject)
 constructor.newInstance("constructor-arg1");
 ````
 
 #### 方法
 ````
 Class aClass = ...//获取Class对象
 Method[] methods = aClass.getMethods();
 
 Class  aClass = ...//获取Class对象
 Method method = aClass.getMethod("doSomething", new Class[]{String.class});
 
 Class  aClass = ...//获取Class对象
 Method method = aClass.getMethod("doSomething", null);

Method method = ... //获取Class对象
Class[] parameterTypes = method.getParameterTypes();

Method method = ... //获取Class对象
Class returnType = method.getReturnType();

//获取一个方法名为doSomesthing，参数类型为String的方法
Method method = MyObject.class.getMethod("doSomething", String.class);
Object returnValue = method.invoke(null, "parameter-value1");
 ````
 
 #### 变量
 ````
 Field[] method = aClass.getFields();
 
 //访问私有变量
 public class PrivateObject {
 
   private String privateString = null;
 
   public PrivateObject(String privateString) {
     this.privateString = privateString;
   }
 }
 PrivateObject privateObject = new PrivateObject("The Private Value");
 
 Field privateStringField = PrivateObject.class.
             getDeclaredField("privateString");
 
 privateStringField.setAccessible(true);
 
 String fieldValue = (String) privateStringField.get(privateObject);
 System.out.println("fieldValue = " + fieldValue);
 
 //访问私有方法
 public class PrivateObject {
 
   private String privateString = null;
 
   public PrivateObject(String privateString) {
     this.privateString = privateString;
   }
 
   private String getPrivateString(){
     return this.privateString;
   }
 }
 PrivateObject privateObject = new PrivateObject("The Private Value");
 
 Method privateStringMethod = PrivateObject.class.
         getDeclaredMethod("getPrivateString", null);
 
 privateStringMethod.setAccessible(true);
 
 String returnValue = (String)
         privateStringMethod.invoke(privateObject, null);
 
 System.out.println("returnValue = " + returnValue);
 ````
 
 #### 泛型
 ````
 //泛型方法函数
 public class MyClass {
   protected List<String> stringList = ...;
 
   public void setStringList(List<String> list){
     this.stringList = list;
   }
 }
 
 //获取方法的泛型参数
 method = Myclass.class.getMethod("setStringList", List.class);
 
 Type[] genericParameterTypes = method.getGenericParameterTypes();
 
 for(Type genericParameterType : genericParameterTypes){
     if(genericParameterType instanceof ParameterizedType){
         ParameterizedType aType = (ParameterizedType) genericParameterType;
         Type[] parameterArgTypes = aType.getActualTypeArguments();
         for(Type parameterArgType : parameterArgTypes){
             Class parameterArgClass = (Class) parameterArgType;
             System.out.println("parameterArgClass = " + parameterArgClass);
         }
     }
 }
 
 //泛型变量类型
 method = Myclass.class.getMethod("setStringList", List.class);
 
 Type[] genericParameterTypes = method.getGenericParameterTypes();
 
 for(Type genericParameterType : genericParameterTypes) {
 
     if (genericParameterType instanceof ParameterizedType) {
         ParameterizedType aType = (ParameterizedType) genericParameterType;
         Type[] parameterArgTypes = aType.getActualTypeArguments();
         for(Type parameterArgType : parameterArgTypes){
             Class parameterArgClass = (Class) parameterArgType;
             System.out.println("parameterArgClass = " + parameterArgClass);
         }
     }
 }
 ````