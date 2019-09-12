考研复试结束，毕业设计也做完了，这段时间温习一下java，使用书籍是java核心技术 卷I ，本文只记录自己略为生疏的java语法知识点。

---

### 第三章 Java的基本程序设计结构
 - #### 字符单元和码点
 字符单元和码点的关系，就如同码元和比特，字和字节的关系。 举个例子，在C语言中，字符串就是字符数组的最后再加一个'\0'， 此时，字符单元就是一个字节，码点是一个字节（码点一直都是以字符大小为一个单位）。 而由于java是unicode编码，所以有些字符是由两个字节构成（比如汉字），而有些字节又是由一个字节构成（比如英文），所以才需要用字符单元和码点来进行区分。这也是为什么java的String跟C/C++里的字符串大不一样的地方。
 
 - ####  尽管在使用Java的时候不要像C那样思考处理字符串，但如果非要转换成C那样的字符数组应该怎么搞？（尽量别用这个方法，最好还是用String的拼接）

```java
/*
	把String变成字符数组，使用C/C++的方式把Hello! 变成Help!
*/

 String s = "Hello!";
int [] s_codePoint = s.codePoints().toArray();  // 把String变成码点数组
s_codePoint[3] = 'p'; //按照C/C++的方式来修改字符串
s_codePoint[4] = '!';
s = new String(s_codePoint, 0, s_codePoint.length-1); // 码点数组变回String
```

- #### 频繁使用到字符串的拼接时， 请用StringBuilder()
```java
// 打印Help!! 
StringBuilder builder = new StringBuilder();
builder.append('H');
builder.append('e');
builder.append("lp");
builder.append("!!");
System.out.println(builder.toString());
```

### 2. 打印与Arrays

 - ###### 用C语言的printf : System.out.printf
 - ###### ```System.out.println(System.getProperty("user.dir"));```//打印当前工作目录
 - ###### 带标签的break label (就是goto label)
 - ###### Arrays.toString(数组)：把数组按[a1,a2,a3,..,an]的形式输出
 - ###### Arrays.copyOf(数组名，新数组长度）：数组拷贝(多余的元素赋值为0)
 - ###### Arrays.sort() 排序
 - ###### Arrays.deepToString(数组) : 打印多维数组
 
 ---
## 第四章 对象与类
 - #### 基于类的访问权限 
```java
// Employee类方法可以访问Employee类任何一个对象的私有域
...
class Employee{
	...
	private String name;	
	public boolean equals_name(Employee other){
		return this.name.equals(other.name); // 尽管name是私有变量，类方法中还是可以访问该类所生成的对象的私有变量.
	}
}
```

 - #### final type_name param 是指param中的对象引用不会再指向其他type_name类型的对象 

	也就是说final并不意味着内容不改变（对于数值类型来说可能确实是不改变），而是意味着不能再被赋值。

 - ### 静态域 
	静态域里的变量是所有对象共享的。
	
   使每一个对象都有一个**独一无二**的标号的方法：
 ```java  
  class Employee{
  	private static int nextId = 1;
  	private int Id;
  	 ...
  	public void setId(){
		   id = nextId;
		   nextId++;	
	 }
  }
  
```
 - #### 静态方法不对任何对象实施操作，没有this，通过类名来调用
  类名.静态方法()
  
  以下两种情况使用静态方法：
 - 不需要访问对象，显示传参，如Math.pow();
 - 一个方法只需要访问静态域 (因为静态域是私有的)

 - #### 每一个类（一个文件一个类）都可以有个main方法，可以用于单元测试。

 - #### 方法参数
  参数的传递方法一般分为按值调用和按引用调用，就是一个会改变传入参数的值，一个不会。
  对于Java来说，这一块比较乱，直接上结论吧：
  	- 对于数值、布尔值这种基本数据类型，是按值传入（不能修改）。
  	- 对于对象这种变量，方法可以修改这个对象的状态（即这个方法内部的一些变量情况，例如把Employee对象中的salary提高10% 这样子）
  	- 对于对象这种变量，方法虽然可以改变其状态，但却不能改变其引用情况，比如：
  	```java
  	void Swap(Employee a, Employee b){
		 Employee c = a;
		 a  = b;
		 b = c;
	}
  	```
  	这个样子是无法改变a和b的引用值的。

 - #### 在构造器中调用构造器的方法：
```java
public Empolyee(String name, double salary){
	this.name = name;
	this.salary = salary;
}
public Empolyee(double salary){
	this("Employee #" + nextId, salary);
	nextId++;
}
```
通过使用```this(...)```或```this()```来在构造器中调用构造函数。

 - ## Package
   不说底层原理，只说说package这个语法怎么用（import就不说了，你明白的）
   
    当前工作目录为C:\Users\Administrator\Desktop\java\PackageTest ， PackageTest是个目录，在这个目录下就是工作目录

    当前目录下只有两个文件，一个Package.java，一个com文件夹：
   	  - ##### ./PackageTest.java 
   	  - ##### ./com/horstmann/corejava/Employee.java
   	  
  
  	如果在PackageTest类中想用Employee类，那么如果直接在PackageTest中 ```import com.horsemann.corejava.*;```会提示错误。 而解决办法就是，在Employee.java中加入一句```package com.horsemann.corejava;```然后在执行，就 **import** 成功了。
	
	也就是说，把一个写好的类放到一个文件夹（包）里，集中存起来希望被别的类import使用时，需要在这些类的中加入```package  xxx.xxx.xxx;```这样才能够被import，所以把package xx语句理解成**我要把当前的这个类放入进xx包中**，只有放入包中的类，才能被import
	
	然后，当我们自己写了很多好用的Java类作为一个工具包，或者从别的地方搞了个工具包，如何能在任意位置去访问这些呢（上面只能在./（当前工作目录下）才能访问）
	方法就是，把当前工作目录存入到ClassPath中（在Windows上就是在环境变量那里设置），例如 ./ 是 C:\Users\Administrator\Desktop\java\PackageTest ，那么就把 C:\Users\Administrator\Desktop\java\PackageTest; 加到CLASSPATH里，记得用 ; 隔开，最后也要加一个 ; 
	**(不同的shell的修改方法不一样，不同的修改方法持续时间也不一样，不过核心都是在classpath中添加一个包的所在目录 )**

-  ## <font color=RED size=5 >不在类前加public、private则默认表示只有能被在同一个包内的类的来访问</font>
-  ## 一个源文件只能有一个public类

---
## 第五章 继承
 - #### super的两个用途：
     - 调用超类的方法 : super.func()
     - 调用超类的构造器  super(...)
  
  - ### 多态
    假设Manager继承了Employee
    ```java
    class Manager extends Employee{
	    private double bonus;
	    public Manager(String name){
	        super(name);
	        bonus = 0.0;
	    }
		public double getSalary(){
			double salaries = super.getSalary();
			return salaries + bonus;
		}
	    public void setBonus(double bonus){
	        this.bonus = bonus;
	    }
	    public void print_bonus(){
	        System.out.println(this.bonus);
    }
    ```
   看下面这段代码，尽管staff[0]和boss指向同一个，但staff[0]是Employee类型，并没有setBouns方法，故```staff[0].setBouns(5000);```会出现错误。
    
```java
	    Manager boss = new Manager("czf");
	    Employee [] staff = new Employee[10];
	    staff[0] = boss;
	    staff[0].setBouns(5000); //Error
	    boss.setBouns(50000); //Success 
```

   还值得一提的是，Manager重写了getSalary()方法，那么staff[0].getSalary()时，尽管staff是Employee类，但还是会优先调用已经被重写的过的getSalary, 也就是Manager的getSalary

- #### 静态绑定和动态绑定 
    对于static、private、final、构造器 方法，编译器可以知道是由哪个函数调用哪个方法，因此是静态绑定。 
    若调用方法依赖于隐式参数的实际类型（例如调用f("czf")，会自动调用f(String)而不是f(int)），运行时就会动态绑定，每个类都有一个方法表，调用时会去查表，找到对应匹配的方法。

 - #### 阻止继承：final类和final方法
    final修饰的方法不允许被重写，且final修饰的类不能被继承，final修饰的类中的所有方法都是final方法。

 - #### 对象的强制类型转换
    子类的引用赋值给一个超类，编译器是允许的。
    超类的引用赋值给一个子类，需要进行强制类型转换；但需要注意的是，超类赋值给子类的时候，只有当这个超类的本质是子类的时候，才不会报错，如果超类的本质就是个超类，那么即使强制转化成了子类，编译虽然可以过，但运行时会抛出ClassCastException ， 下面举个例子来看：
   
```java
public class code5_1{
    public static void main(String[] args) {
        Employee czf = new Manager("czf"); //虽然类型是Employee，但本质是Manager
        Employee my = new Employee("my");  //不仅类型是Employee，本质依然是Employee
        Manager test1 = (Manager)czf;  //成功通过，不会报错
        Manager test2 = (Manager)my;   //会抛出异常，因为my的本质是Employee，无法强转成Manager
        /***************************************************/
		test.setName("czf2");
        czf.print();
        test.print();
        /*
			打印结果为:
				czf2, I am a Manager
				czf2, I am a Manager
			可见强制类型转换后，依然是指向同一个对象
		*/
    }
}

class Manager extends Employee{
    private double bonus;
    public Manager(String name){
        super(name);
        bonus = 0.0;
    }

    public void print(){
        System.out.println(super.getName()+", I am a Manager.");
    }

    public void setBonus(double bonus){
        this.bonus = bonus;
    }
    public void print_bonus(){
        System.out.println(this.bonus);
    }
}

class Employee{
    private String name;
    public Employee(String name){
        this.name = name;
    }
    public String getName(){
        return name;
    }
    public boolean equals(Employee other){
        if ( this.name.equals(other.name) )
            return true;
        else return false;
    }
    public void setName(String name){
        this.name = name;
    }
    public void print(){
        System.out.println(this.name + ", I am a Employee.");
    }
}
```
 - #### A instanceof B : 若对象A是类B，那么返回true，否则返回false
   强制类型转换对象类型前，使用instanceof来判断一下，避免出现ClassCastException。
   
   不过最好还是不要使用强制类型转换来变化对象类型，因为多态的动态绑定总是能帮我们找到我们最想调用的那个方法。

 -  ### 抽象类
    几个特点，总结一下：
      - 只有包含抽象函数的类，肯定是抽象类（也就是说，只要不是抽象类，则该类内肯定没有抽象函数）
      - 只要是抽象类，则一定不可以被实例化（也就是说，如果类A是抽象类，那么
       ``A a = new A();`` 会报错，因为A是抽象类，不能创建对象）
      - 抽象类是用来被继承的，不是用来实例化的，但是，并不是说抽象类里一定全是抽象函数（抽象类中也可以存在非抽象函数，用来被继承使用）。 
      - 可是，许多程序员认为，抽象类中最好不要包含具体函数（尽管可以包含）
      - 一个类即使不包含抽象方法，也可以被定义为抽象类（但这个类不能被实例化了）
      - 尽管抽象类不能实例化，但可以定义一个抽象类的变量来引用一个非抽象类的实例（这个非抽象类应是该抽象类的子类，例如 Person是抽象类，Student继承了Person，但Student不是抽象类，那么``Person p = new Student();`` 也是OK的）
      - 更多抽象方法，到第六章会讲，可以空降第六章

 - #### protected属性
   被protected修饰的变量或方法，可以被其子类或<font color=RED size=3 >在同一包内的其他类</font>可见，安全性相对较差。
    小结一下：
      - 仅对本类可用——private
      - 对所有类可见——public
      - 对本包和所有子类可见——protected
      - 对本包可见——什么也不加

- ### Object : 所有类的超类
   挨个介绍一下Object类的方法，所有对象都包含的方法：
     - **equals()** ： a.equals(b)判断a和b是否相同。 
     - **hashCode()** : 根据对象导出一个整型值。 需要注意的是，Object实现的hashCode方法是直接返回该对象的存储地址，但不排除有些已存在的类已经重写了该方法，比如String类就重写了该方法，String类生成hashCode的方法是根据字符串内容来生成而不是地址。 但对于没有重写该方法的类，会自动继承Object的hashCode() 
    
        这里多说一句，在重写Object的hashCode的时候，可以利用Object的hashCode来辅助我们实现新的hashCode
     - **toString()**: 返回表示对象值的String ， Object的toString()返回的字符串是
       “该对象所属类名@该对象的散列码” ， 如果一个类没有实现toString(), 则调用时，返回的就是i这个。
    - **getClass()** :  返回Class对象，该对象包含了对象信息。
    - **getName()**: 返回一个字符串，是这个类的名字。
    - getSuperClass(): 返回Class对象，包含了该类的超类信息。
    
- ### ArrayList<> () 方法简介 （看详细的去查API）
    1. ArrayList<>(int 最初容量)
    2. boolean add(Elemtype obj)
    3. int size()
    4. void ensureCapacity(int 最终容量不变了)
    5. void trimToSize()  ： 把容量缩减到当前尺寸
    6. void set(int index, Elemtype obj)  : 等价于 a[index] = obj
    7. Elemtype get(int index) : 等价于 a[index] , 这里要多说两句，最好在使用的时候，前面加个强制类型转换，就是这样子： (Elemtype) Arr.get(i) , 貌似这个get返回的是Object类型 
    8. void add(int index, Elemtype obj) : add的重载，index为插入位置，obj为新元素
    9. Elemtype remove(int index) ： 删除index位置上的元素并返回该元素
    10. 以上的index都介于0~size()-1之间  
    11.  值得注意的是，<>中只能填对象名，也就是说，里面不能写int，而一个写int的对象包装器，即Integer。
    12. ArrayList比数组慢很多，但很方便。

 - ### 对象包装器
      **以Integer为例**
      1. 使用对象包装器的时候, 不要忘记它是个对象，==操作是判断是否指向同一个对象而不是值是否相同。
      2. Integer是个final的，无法继承它，也无法改变对象内部的值，所以不要想着因为它是个对象，就通过这种方法来通过函数来改变参数。
   
    常用方法：
    1. int intValue() : 返回Integer中的值
    2. static String toString() 
    3. static Integer valueOf(String s) : 字符串变Integer对象
   
---	
# 第六章 接口、lambda表达式和内部类
- ### 接口 
```java
public interface Comparable<T>{ 
	int compareTo(T other); 
}

public class Employee implements Comparable<Employee>{
	...
	...
	public int compareTo(Employee other){
		...
	}
} 
```
上面的``T``只是代表一个类名，具体是什么值要看真正使用的时候``<>``里面填的是什么；
不使用泛型时，默认是Object，使用时需要强制类型转换一下，不过最好别这样。
       
- #### 接口不能实例化，但可以引用实现了接口的类对象
```java
	Comparable x = new Comparable(); //Error
	Comparable x = new Employee(); //OK
```

- #### 接口也可以被继承
     虽然接口中不能有实例域 ( java se8中允许在接口中增加静态方法 )，但却可以包含常量，<font color=RED size=5>接口的方法都自动设置成public, 接口中的域也被自动设置成public static final</font>
 ```java
    public interface Moveable{
		   void move(double x,double y);	
	}
	public interface Powered extends Moveable{
		   double milesPerGallon();
		   double SPEED_LIMIT = 95; // a public static final constant
	}
 ```
 - #### 接口中的 default  方法
    如果想在接口中实现一个方法的实例（<font color=RED size=4>这个方法可以被重写</font>），那么用default来修饰这个方法就可以了，之后使用这个接口的所有类都会得到这个方法，并且可以对其重写。 

	默认方法的另一个好处就是，当要对一个接口来扩充方法时，若不使用default定义，那么其他使用这个接口的类就会因为没有对新加的方法实现而出现error， 这个时候如果是使用default进行增加，就没有这个问题。

- #### 当默认方法冲突的时候，处理方法：
     1. 如果是接口之间存在的方法命名冲突，那么重写这个方法，
       并调用``接口名.super.冲突的方法名()`` ， 例如``interface1``与``interface2``都实现了``getName()``, 但是你想调用``interface2``的``getName()`` ， 那么在重写时，只要调用``interface2.super.getName()``就行了。
	```java
	class Student implements interface1,interface2{
		...
		public String getName(){
			return interface2.super.getNmae();
		}
	}
	```
	2. 如果是继承的类和接口之间存在了方法的冲突，则会Java会按照“类优先”的原则去调用。

- #### 接口与回调
  	每隔特定的一个时间，就调用一次某个函数（感觉挺有用的就先记录一下）
  	```java
	    import java.awt.*;
		import java.awt.event.*;
		
		import javax.swing.JOptionPane;
		import java.util.*;
		import javax.swing.*;
		import javax.swing.Timer;
		
		public class TimerTest{
		    public static void main(String[] args) {
		        ActionListener listener = new TimePrinter();
		        Timer  t = new Timer(10000,listener); //10000ms，通告listener一次
		        t.start();   //启动Timer
		        JOptionPane.showMessageDialog(null, "Quit?"); 
		        System.exit(0);
		    }
		}
		
		class TimePrinter implements ActionListener{
		    public void actionPerformed(ActionEvent event){
		        System.out.println("At the tone, the time is " + new Date());
		    }
		}
  	```
  	**函数说明：**
  		
  	    1.  Timer( int interval, ActionListener listener )  
  	     	  构造一个定时器，每隔interval毫秒，告诉一次Litener
 	     2. ActionListener 接口的构成：
 ```java
 		public interface ActionListener{    
  	     	void actionPerformed(ActionEvent event);  //这个函数是要被重写的，定时调用这个函数
  	    }
  ```
  		 利用这个接口来实现多态，也就是说，这个接口可以引用实现这个接口的对象！

   - #### 自定义sort --- Comparator接口
```java
  
public class Sort{
    public static void main(String[] args) {
        Comparator<String> comp = new LengthComparator();        
        String [] friend = {"czf","my","llzy","bdbbb"};
        Arrays.sort(friend,comp);
        System.out.println(Arrays.toString(friend));
    }
}

class LengthComparator implements Comparator<String>{
     public int compare(String First,String Second){
         // 第一个参数减去第二参数，就是小的在前大的在后
         // 反之就是大的在前小的在后
         return First.length() - Second.length();
     }
}

```

 - #### 浅拷贝和深拷贝
   Object类有一个方法是clone() ， 但是克隆出来的对象``A_``与被克隆的对象``A``会有一些变量引用是共享的（比如``A.name``引用一个String，`A_.name`也会引用这个String）
   
   <font color=RED size=4>因为Object类的clone()是protect, 而不是public</font>，因此在设计一个类的时候要考虑：
   1.  要不要实现克隆方法?
   2.  如果要实现克隆方法，原有的clone（浅拷贝） 能否满足？

   当已有的浅拷贝clone方法不能满足我们的需求，我们要重新写克隆的时候，这个时候就要使用Cloneable接口（是记号接口之一） 。 深拷贝需要自己去实现，把那些指向同一个对象的变量也给clone了，这么递归clone下去。（  <font color=GREN size=4>但这种尽管安全，但效率不高，卷二中会利用Java的串行化特性再来介绍克隆</font>）
```java
public interface Cloneable {
	//因为已经继承了Object的clone()， 所以这个接口里啥也没有
	// 属于记号接口
}
   ```

 - ### Lambda表达式
 
 <font color=GREEN size=4>就是把一个**函数**写成**代码块**,  用来代替一个函数（即某个接口中的那个唯一函数，这个接口的存在就是为了传入这个接口中这个唯一函数），**这个函数用于延迟执行（函数指针？）**</font>
     
 形如：<font color=red size=5> (形参列表)->{函数实体}</font>  ，举个例子
```java
(String first, String second)->{ return first.length() - second.length(); }
```
如果能够知道其类型（就是引用对象是包含<>的，
例如`Comparator<String> comp = (first,second)->first.length() - second.length()`），那么可以省去
类型的说明，若又只有一个返回语句，则可以return，分号和花括号，如下：
```java
(first,second) -> first.length() - second.length()
```
    
进一步说明： 以Arrays.sort方法为例， 在回调的时候，往往是传入一个函数，然后在某个特定的时刻，程序会调用这个传入的函数。在Java中，是通过传入是实现了某个接口的对象，然后调用这个对象所实现的方法（也就是那个接口要求实现的方法），这也就相当于传入了一个函数。，但是实现起来却听烦的（又要多写一个实现某个接口的类，又要实例化）， 此时可以用**lambda表达式** 来传入，比如下面重新实现```actionPerfomed```函数：
```java
Timer t = new Timer(1000, event->
					{
						System.out.println("At the tone, the time is "+ new Data());
					});
```
**最后再次强调，能不能省去参数类型，要看接收的那个变量（被赋值的变量）是不是一个含有<>的对象.**

```
 String [] friend = {"czf","my","llzy","bdbbb"};
```
排序的那个语句，可以写成
`Arrays.sort(friend,(a,b)->a.length()-b.length);` 或
`Arrays.sort(friend,(String a,String b)->{return a.length()-b.length();});`

**最后！**

<font color=red size=5>lambda表达式的代码块中，所有变量都要是final！ 也就是说变量都不可以变！！</font>


- ### 方法的引用

   使用::来表达方法的引用（有点像C++的域），有如下几个组合：
   	1.  对象 :: 实例方法
   	2. 类 :: 实例方法
   	3. 类 :: 静态方法
  
- ### 构造器的引用 
     构造器的引用和方法的引用很类似，只不过方法名为new。 例如，``Person::new``是Person构造器的一个引用。 
     举例：
     ```java
      Object [] people = stream.toArray();
      Person [] people = stream.toArray(Person[]::new)
      ```

- ### 编写处理lambda表达式的方法
 1. **不传参（void）**
	使用Lambda表达式的场合就是异步（延迟执行）
   例如想让某个动作执行10次（下例动作为打印操作）
 ```java
 repeat(10, ()->System.out.println("Hello World!"));
 public static void repeat(int n, Runnable action){
	for ( int i=0; i<n; ++i )
			action.run();
} 
  ```

2. **传入参数 i**                  
```java
public class test{
    public static void main(String[] args) {
        repeat(10, (i)->System.out.println(i));
    }

    public interface IntConsumer_{
        void accept_(int i);
    }

    public static void repeat(int n,IntConsumer_ action){
        for ( int i=0; i<n; ++i )
            action.accept_(i);
    }
}
```
对上面的代码解读： 在Java中想要传函数的一般方法就是，专门为这个函数设一个接口（这个接口中只有这一个要传的函数），然后再写一个实现了这个接口的类，实例化以后，利用多态的特性（形参是接口而非类），把这个实例化的对象传过去，可见非常繁琐。  

`repeat`的第二个参数可以：
 1.  传实现了IntConsumer这个接口的类的对象(在对象中实现accept_函数)
 2.  不用实现类，而直接用lambda表达式实现出待传函数（accept_），上面的代码是仅仅实现了包装待传函数（accept_）的接口（IntConsumer_).  

使用异步小结: 
 1.  callback函数不含参数，用`Runnable action`来接收lambda函数，`action.run()`为调用
 2.  callback函数含参数时，专门为callback函数写一个接口，可以用Lambda表达式里实现callback，也可以写一个实现该接口的类，在类中实现callback



---

- ### 内部类
 
 1.  <font color=blue size=4>内部类可以访问外围类对象（在类A中又写了一个内部类B，那么A就是B的外围类），有一个名为outer的引用（即在自动生成的构造器代码中，会有一个``outer=外围类;``的语句），outer并不是Java的关键字。
```java
public class TimePrinter implements ActionListener{
	//=======自动生成的代码========
	public TimePrinter(外围类 A){ //外围类new TimePrinter()时，会自动补上参数变成new TimerPrinter(this) 
		outer = A;
	} 
	//============================
	public void actionPerformed(ActionEvent event){
		...
	}
}
```
2. 这里只说结论，内部类会被编译器自动翻译成 **“外围类$内部类”** 这种类，这是编译器干的活，虚拟机完全不知情。 

3. 外围类作用于之外想引用外部类的内部类：
				OuterClass.InnerClass
4. 内部类尽量不要使用static方法。
5. 下面来看一个例子：
```java
class TalkingClock
{
   private int interval;
   private boolean beep;
   ...
   ...
   public class TimePrinter implements ActionListener
   {
      public void actionPerformed(ActionEvent event)
      {
         System.out.println("At the tone, the time is " + new Date());
         if (TalkingClock.this.beep) Toolkit.getDefaultToolkit().beep();
      }
   }
}
```
`TimerPrinter`是`TalkingClock`的内部类，会生成一个`TalkingClock$TimePrinter.class`的文件，然后利用
`javap -private TalkingClock$TimePrinter` 来查看这个类，会看到如下的显示：
```java
public class TalkingClock$TimePrinter implements java.awt.event.ActionListener {
 	final TalkingClock this$0;
  	public TalkingClock$TimePrinter(TalkingClock);
  	public void actionPerformed(java.awt.event.ActionEvent);
}
```
可以清楚的看到，编译器为了让内部类（`TimerPrinter`）能够引用外部类（`TalkingClock`），它自动生成了一个附加的实例域 `this$0` (这是由编译器自动生成的，代码中不能使用它)，同时还能看到内部类构造器使用了`TimerClock`参数。

 <font color=Red size=5>注意！！！</font> 如果我们在自己的程序中，利用这个技术呢（这里就是把`TimerPrint`拿出来，不再是内部类，然后利用同样的方式，尝试去访问**原来**的外围类（因为已经不是包含关系而是评级关系了，所以是**原来**的外围类））中的变量，这个时候就会发现并不能够访问！  


 <font color=blue size=4>（ 其实想想也很简单，因为是包含关系，所以内部类能够访问外围类的数据，但一旦从包含变成了平级，那就要严格按照变量是public 还是private还是默认的规则来访问了）</font>
 
 
<font color=Red size=5>那为什么我们自己这么搞不可以，但编译器自动转换的程序却可以呢？？！！！</font> 用反射程序再来看一下外围类（`TalkingClock`）,使用指令`javap -private TalkingClock` , 这时候我们可以看到:
 ```java
 class TalkingClock {
	  private int interval;
	  private boolean beep;
	  public TalkingClock(int, boolean);
	  public void start();
	  static boolean access$000(TalkingClock);
}
 ```
这里多出来个 **静态方法** access$000，返回的还是**boolean**类型，**和beep一样**， 这下就豁然开朗了！

<font color=DBLUE size=4>这根本不是什么新东西，之所以内部类可以访问外围类，是因为编译器通过自动生成一个静态方法( 这里是access$000 )，专门用来返回那些被内部类访问的外围类的变量（这里是beep变量）！！！！

因此，当编译器一顿操作之后，在虚拟机眼里
```
if (beep)
```
就变成了
```
if (TalkingClock.access&000(outer)) //outer是在自动生成的构造器里 outer = TalkingClock; 这条语句实现的
```
熟悉文件结构的黑客可以使用十六进制编辑器轻轻松松地创建一个用虚拟机指令调用那个方法的类文件，因此并不是100%封闭的。


---

- ### 局部类
  

```java
// 内部类形式
class TalkingClock
{
   private int interval;
   private boolean beep;
   ...
   ...
   public void start()
   {
      ActionListener listener = new TimePrinter();
      Timer t = new Timer(interval, listener);
      t.start();
   }

   public class TimePrinter implements ActionListener
   {
      public void actionPerformed(ActionEvent event)
      {
         System.out.println("At the tone, the time is " + new Date());
         if (TalkingClock.this.beep) Toolkit.getDefaultToolkit().beep();
      }
   }
}
```

 还是这段代码，我们可以看到，TimePrinter这个类只被使用了一次！ 因此可以在把它变成**局部类**  
```java
class TalkingClock
{
   private int interval;
   private boolean beep;
   ...
   ...
   public void start()
   {
   	   // 局部类
   	   class TimePrinter implements ActionListener{
	      public void actionPerformed(ActionEvent event){
	         System.out.println("At the tone, the time is " + new Date());
	         if (beep) Toolkit.getDefaultToolkit().beep();
	      }
	   }
	   
      ActionListener listener = new TimePrinter();
      Timer t = new Timer(interval, listener);
      t.start();
   }
}
```
局部类不可以用public或private这种说明符进行说明，必须是什么也不加的默认形式的，这个类就像局部变量一样被隐藏起来。

<font color=RED size=4>局部类最大的优点就是：可以访问局部变量！因为和程序是在同一个作用域中（可以把局部类类比成局部变量那样！）</font> (其实还是换汤不换药，编译器最终还是会把局部类给转换成一个平级的外部类，但是会把用到的局部变量保存到外围类的一个默认的final修饰的引用中（例如`final boolean val$beep`），可供外面(但在同一个包中)调用(风险尽管很微小，但还是存在的，正如上文提到的，这里不再赘述)  )

#### 约定与trick:
**同样的，在“里面”访问“外面”的变量时， 里面尽量是以final的形式来接收外面传来的变量。**
**但如果无论如何都要在“里面”进行改变，这里有一个小技巧**：
	 **<font color=READ  size=4>这个技巧就是，例如要传入counter，用来作计数器，但又要求是final，这时候可以把counter变成 `int [] counter = int[1];` 然后传入counter[], 这个时候就可以改变counter[0]了！</font>**

--- 

- ### 匿名类
 如果有一个类，他的作用是只创建一个对象，那么这个类就不必给他命名了，也就是内部类。
 因为不给命名，所以必须继承一个父类或实现一个接口，这样才有类型给他引用。同一个例子，来看下面这个代码：
```java
class TalkingClock
{
   public void start(int interval, boolean beep)
   {
   		 //匿名类
      	 ActionListener listener = new ActionListener()
         {
            public void actionPerformed(ActionEvent event)
            {
               System.out.println("At the tone, the time is " + new Date());
               if (beep) Toolkit.getDefaultToolkit().beep();
            }
         };
      	 Timer t = new Timer(interval, listener);
      	 t.start();
   }
}

```

匿名类的形式：
```
new SuperType(construction parameters){
	inner  methods and data 
}

or 

new InterfaceType(construction parameters){
	inner  methods and data 
}
```

双括号初始化：
```java
ArrayList<String> friends = new ArrayList<>();
friends.add("czf");
friends.add("ayano");
invite(friends);
```
如果只在invite方法里使用到了这个friends，也就是说之后不会再用到friends了，那么可以利用**匿名列表**：
```java
invite( new ArrayList<String>() {{ add("czf"); add("ayano"); }}  )
注意，这里是双括号{{ }} 
```
但当使用equals方法的时候，或是要打印调试信息的时候，尽量就别这么搞了。
因为涉及到this，super这样的东西， 匿名类都会出点问题。。

---
 - ### 静态内部类
 
 	一般来说，使用内部类的时候，内部类总会产生一个对外围类的引用。 但我们使用内部类的时候，并不是所有场合都要引用外部类，为了取消这个产生的引用（不让编译器自动产生这个引用），我们可以将内部类生命为static，也就是静态内部类。
 	
 	**静态内部类的一个使用场景** ： 我们想定义一个Pair类，但是Pair这个名字太大众了，可能会与其他命名冲突，那么就可以在这个类前再套一个类， 例如类ArrayAlg中有一个Pair类，那么使用Pair的时候就是`ArrayAlg.Pair` ,这个时候就避免了命名冲突的情况。
		
		所以，在实现内部类的时候，若不会使用到外围类的数据，那么应将其设为静态内部类。

---
#	本篇完
 遗留知识点： 反射，代理。