# javase

## 8.面向对象编程

### 1.访问修饰符 

java 提供四种访问控制修饰符号，用于控制方法和属性(成员变量)的访问权限（范围）: 

1) 公开级别:用 **public** 修饰,对外公开 
2) 受保护级别:用 **protected** 修饰,对**子类和同一个包中的类公开** 
3) 默认级别:没有修饰符号,向同一个包的类公开.
4) 有级别:用 **private** 修饰,**只有类本身可以访问,不对外公开**

### 2.三大特征

面向对象编程有三大特征：封装、继承和多态

#### 1.继承

```java
public class Pupil extends Student{
    public Pupil(String name, int age, int score) {
        super(name, age, score);
    }
    public void showInfo(){
        System.out.println("父类的姓名："+name);
        //score是stuudent中的私有方法，不能在子类puplic中直接访问
        System.out.println("父类的成绩："+score);//java: score可以在Student中访问private
    }
}
```

继承的深入讨论/细节问题 

1) 子类继承了所有的属性和方法，非私有的属性和方法可以在子类直接访问, **但是私有属性和方法不能在子类直接访问，要通过父类提供公共的方法去访问** 
1) 子类必须调用父类的构造器， 完成父类的初始化 
1) 当创建子类对象时，不管使用子类的哪个构造器，默认情况下总会去调用父类的无参构造器，如果父类没有提供无参构造器，则必须在子类的构造器中用 super 去指定使用父类的哪个构造器完成对父类的初始化工作，否则，编译不会通过
1)  如果希望指定去调用父类的某个构造器，则显式的调用一下 : **super(参数列表)**
1) super 在使用时，**必须放在构造器第一行**(super 只能在构造器中使用) 
1) **super() 和 this() 都只能放在构造器第一行，因此这两个方法不能共存在一个构造器**
1) **java 所有类都是 Object 类的子类, Object 是所有类的基类.**
1) 父类构造器的调用不限于直接父类！将一直往上追溯直到 Object 类(顶级父类)
1)  子类最多只能继承一个父类(指直接继承)，即 **java 中是单继承机制**。

#### 2.super关键字

super 代表父类的引用，用于访问父类的属性、方法、构造器；super关键字不能访问父类的私有属性和方法

访问父类的构造器：**super(参数列表);**只能放在构造器的第一句，只能出现一句！

#### 3.多态

方法或对象具有多种形态。是面向对象的第三大特征，多态是建立在封装和继承基础之上的。

**关于运行类型和编译类型**

- 编译类型在对象创立之初就已经确立，不能改变；而运行类型是可以改变的；
- 编译类型看=的左边，运行类型看=的右边

### 3.动态绑定机制

###  4.Object 类详解

#### 1.equals和==

equals()方法和==都是比较两个数据是否相等的，不同的是==用于比较两个基本数据类型，而equals()方法比较两个引用数据类型。

==没有什么说的，它比较的是变量里面存放的值，直接拿两个变量进行比较就完了，

而equals()方法本来也是直接比较两个变量里面存放的值（地址值），但是如果我们重写了equals()方法就比较的是两个对象了。

#### 2.hashCode

提高具有哈希结构的容器的效率！

两个引用，如果指向的是同一个对象，则哈希值肯定是一样的！ 

两个引用，如果指向的是不同对象，则哈希值是不一样的 

哈希值主要根据地址号来的！， **不能完全将哈希值等价于地址**

#### 3.toString 

默认返回：**全类名+@+哈希值的十六进制**， 子类往往重写 toString 方法，用于返回对象的属性

#### 4.finalize方法

当对象被回收时，系统自动调用该对象的 finalize 方法。子类可以重写该方法，做一些释放资源的操作

### 5.静态变量和方法

**类变量也叫静态变量/静态属性**，是该类的所有对象共享的变量，任何一个该类的对象去访问它时，取到的都是相同的值，同样任何一个该类的对象去修改它时，修改的也是同一个变量。

2、类变量的访问，必须遵守相关的访问权限。

​			静态变量是类加载的时候，就创建了，可以通过**类名.类变量名**来访问；

​			**非静态方法，不能通过类名调用**

```java
public static void main(String[] args) {
	B b = new B(); 
	//System.out.println(B.n1); //不能访问
	System.out.println(B.n2); //通过类名.类变量名来访问
}
class B {
	public int n1 = 100;
	public static int n2 = 200;
}
```
- 静态方法，只能访问静态成员 

- 非静态方法，可以访问所有的成员 

- 在编写代码时，仍然要遵守访问权限规则

#### 1.理解main方法

- 在 main()方法中，我们可以直接调用 main 方法所在类的静态方法或静态属性。
-  但是，**不能直接访问该类中的非静态成员**，必须创建该类的一个实例对象后，才能通过这个对象去访问类中的非静态成员。

#### 2.代码块

代码化块又称为初始化块,属于类中的成员[即是类的一部分]，类似于方法，将逻辑语句封装在方法体中，通过**{}**包围起来。但和方法不同，**没有方法名，没有返回，没有参数，只有方法体，而且不用通过对象或类显式调用，而是加载类时，或创建对象时隐式调用。**

注意

- ​	修饰符 可选，要写的话，也只能写 static

- ​	代码块分为两类，使用static修饰的叫静态代码块，没有static修饰的，叫普通代码块/非静态代码块

- ​	逻辑语句可以为任何逻辑语句（输入、输出、方法调用、循环、判断等)


3、老师理解：

- ​	相当于另外一种形式的构造器(对构造器的补充机制)，可以做初始化的操作；

- ​	场景: 如果多个构造器中都有重复的语句，可以抽取到初始化块中，提高代码的重用性

```java
//将公共代码抽取出来，不用每次在构造器中重写，提高效率
{
    System.out.println("电影屏幕打开...");
    System.out.println("广告开始...");
    System.out.println("电影正是开始...");
}
public Move(String name) {
    System.out.println("Movie(String name) 被调用...");
    this.name = name;
}
public Move(String name, double price) {
    this.name = name;
    this.price = price;
}
```

静态代码块

​	static代码块也叫静态代码块，作用就是对类进行初始化，而且它随着类的加载而执行，并且*只会执 行一次*。

​	如果是普通代码块，每创建一个对象就执行。

​	类什么时候被加载

- 创建对象实例时(new)

- 创建子类对象实例，父类也会被加载

- 使用类的静态成员时(静态属性，静态方法)


​	普通的代码块，在创建对象实例时，会被隐式的调用。被创建一次，就会调用一次。

​	如果只是使用类的静态成员时，普通代码块并不会执行。

小结:

- static代码块是类加载时执行，只会执行一次

- 普通代码块是在创建对象时调用的，创建一次，调用一次

- 类加载的3种情况，需要记住.

#### 3.final 关键字

1、如果我们要求 A 类不能被其他类继承，可以使用 final 修饰A类；

```java
final class A { }
```

2、当不希望类的的某个属性的值被修改,可以用 final 修饰

```java
public final double TAX_RATE = 0.0;
```

3、如果我们要求 hi 不能被子类重写 ，可以使用 final 修饰 hi方法；

```java
class C { 
 	public final void hi() {}
 }
```

4、当不希望某个局部变量被修改，可以使用 final 修饰；

注意

​	如果 final 修饰的属性是静态的，则初始化的位置只能是 

- 定义时 
- 在静态代码块，不能在构造器中赋值；

```java
public static final double TAX_RATE = 99.9; 
public static final double TAX_RATE2 ; 
static { 
		TAX_RATE2 = 3.3;
 }
```

​	**final 类不能继承，但是可以实例化对象**

```java
final class CC {}
public static void main(String[] args) { 
	 CC cc = new CC();
	 new EE().cal();
 }
```

​	**如果类不是 final 类，但是含有 final 方法，则该方法虽然不能重写，但是可以被继承**

```java
class DD {
 public final void cal() { 
     System.out.println("cal()方法");
 }
} 
class EE extends DD { }
```

-    final 和 static 往往搭配使用，效率更高，不会导致类加载，底层编译器做了优化处理


```java
class BBB { 
	public final static int num = 10000; 
	static { System.out.println("BBB 静态代码块被执行");
	 }
}
```

- 一般来说，如果一个类已经是 final 类了，就没有必要再将方法修饰成 final 方法

- final不能修饰构造器

- 包装类（Integer、Double、String等）也是final类

#### 4.抽象类

- ​	当类中的某些方法，需要声明，但是又不确定如何实现的时候就将其声明为抽象类；

- ​	抽象类，不能被实例化；

- ​	抽象类不一定要包含 abstract 方法。也就是说,抽象类可以没有 abstract 方法。但是，一旦类包含了 abstract,则这个类必须声明为 abstract

- ​    **abstract 只能修饰类和方法，不能修饰属性和其它的 方法**

- ​	**抽象方法不能使用 private、final 和 static 来修饰**，因为这些关键字都是和重写相违背的

- ​	如果一个类继承了抽象类，则它必须实现抽象类的所有抽象方法，除非它自己也声明为 abstract 类

```java
public class AbstractDetail01 { 
    public static void main(String[] args) { 
        //抽象类，不能被实例化 
        //new A(); 
    } 
} 
//抽象类不一定要包含 abstract 方法。也就是说,抽象类可以没有 abstract 方法 ，还可以有实现的方法。 
abstract class A { 
    public void hi() { 
        System.out.println("hi"); 
    } 
} 
//一旦类包含了 abstract 方法,则这个类必须声明为 abstract 
abstract class B { 
    public abstract void hi(); 
} 
//abstract 只能修饰类和方法，不能修饰属性和其它的 
class C { 
    // public abstract int n1 = 1; 
}
```

#### 5.接口

​	接口就是给出一些没有实现的方法,封装到一起,到某个类要使用的时候，在根据具体情况把这些方法写出来

​	小结：

- ​		**接口是更加抽象的抽象的类，抽象类里的方法可以有方法体，接口里的所有方法都没有**
- ​		接口体现了程序设计的多态和高内聚低偶合的设计思想。

- ​		特别说明：Jdk8.0后接口类可以有静态方法，默认方法，也就是说接口中可以有方法的具体实现

- ​	    接口不能被实例化 

- ​	    接口中所有的方法是 public 方法, 接口中抽象方法，可以不用 abstract 修饰

- ​	     一个普通类实现接口,就必须将该接口的所有方法都实现,可以使用 alt+enter 来解决 

- ​	     **抽象类去实现接口时，可以不实现接口的抽象方法**

```JAVA
Interface IA { 
    void say();
    //修饰符 public protected 默认 
    private void hi(); 
} 
class Cat implements IA{ 
    @Override public void say() { } 
    @Override public void hi() { } 
} 
abstract class Tiger implements IA { }//抽象类去实现接口时，可以不实现接口的抽象方法
```

-    一个类同时可以实现多个接口


```java
class Pig implements IB,IC { 
    @Override public void hi() { 
    } 
    @Override public void say() { } 
}
```

接口的修饰符 只能是 public 和默认，这点和类的修饰符是一样的 

```java
interface IE{}
```

接口不能继承其它的类,但是可以继承多个别的接口 

```java
interface ID extends IB,IC {

 }
```

#### 6. 内部类

#####  	1.局部内部类

​		局部内部类是定义在外部类的局部位置,通常在方法 

​		不能添加访问修饰符,但是可以使用 final 修饰

​		作用域 : 仅仅在定义它的方法或代码块

​		可以直接访问外部类的所有成员，包含私有的，**局部内部类可以直接访问外部类的成员**

```java
public class OuterClass02 {
        private int n1 = 100;
        private void m2() {
            System.out.println("Outer02 m2()");
        }
        public void m1() {//方法
        	//1.局部内部类是定义在外部类的局部位置,通常在方法
        	//3.不能添加访问修饰符,但是可以使用 final修饰
        	//4.作用域 : 仅仅在定义它的方法或代码块中
        	final class Inner02 {//局部内部类(本质仍然是一个类)
            	//2.可以直接访问外部类的所有成员，包含私有的
            	private int n1 = 800;
            	public void f1() {
            		//5. 局部内部类可以直接访问外部类的成员，比如下面 外部类 n1 和 m2()
            		//7. 如果外部类和局部内部类的成员重名时，默认遵循就近原则，如果想访问外部类的成员，
        			// 使用 外部类名.this.成员）去访问
        			//Outer02.this 本质就是外部类的对象, 即哪个对象调用了 m1, Outer02.this 就是哪个对象
                	System.out.println("n1=" + n1 + " 外部类的 n1=" + OuterClass02.this.n1);
                	System.out.println("Outer02.this hashcode=" + OuterClass02.this);
                	m2();
            }
        }
        //6. 外部类在方法中，可以创建 Inner02 对象，然后调用方法即可
        Inner02 inner02 = new Inner02();
        inner02.f1();
    }
}
```

#####  	2.匿名内部类

- ​		本质是类，内部类，且该类没有名字

- ​		同时还是一个对象，**匿名内部类定义在外部类的局部位置，比如方法中，且无类名；**

- ​		可以直接访问外部类的所有成员，包括私有的，但是不能添加访问修饰符，因为他的本质上还是一个局部变量

- ​		作用域：仅仅定义在他的方法或代码块中。

- ​		外部类的能直接访问匿名内部类，因为他的本质上是一个局部变量

- ​		匿名内部类使用一次，就不能再使用。

```java
public class Outer04 {
    public static void main(String[] args) {
        Outer04 outer04=new Outer04();
        outer04.dog.eat();
    }
private int age=10;
    Animal dog = new Animal() {
        
        @Override
        public void eat() {
            System.out.println("狗在吃饭");
            System.out.println(age);
        }

        @Override
        public void sleep() {
            System.out.println("狗在睡觉");
        }
    };

}
```

##### 3.成员内部类

- ​    成员内部类是定义在外部类的成员位置，并且没有static修饰。可以直接访问外部类的所有成员，包含私有的。

- ​     可以添加任意访问修饰符(public、protected 、默认、private),因为它的地位就是一个成员,

- ​     作用域和外部类的其他成员一样，为整个类体，在外部类的成员方法中创建成员内部类对象，再调用方法.

- ​	成员内部类---访问---->外部类成员   [访问方式：直接访问](说明)

- ​	外部类---访问------>成员内部类	访问方式： 创建对象，再访问

- ​	如果外部类和内部类的成员重名时，内部类访问的话，默认遵循就近原则，
- ​    如果想访问外部类的成员，则可以使用（外部类名.this.成员）去访问

```java
class Outer08{
    private int n1=10;
    public String name="张三";
    private void hi(){
        System.out.println("hi()方法...");
    }
    public class Inner08{//成员内部类，
        private double sal = 99.8;
        private int n1 = 66;
        public void say(){
            System.out.println("n1 = " + n1 + " name = " + name + " 外部类的 n1=" + Outer08.this.n1);
            hi();
        }
    }
        //返回成员内部类的一个实例
        public Inner08 getInner08Instance(){
            return new Inner08();
        }
        public  void  t1(){
            Inner08 inner08 = new Inner08();
            inner08.say();
            System.out.println(inner08.sal);
        }
}
```

##### 	4.静态内部类

​	说明：静态内部类是定义在外部类的成员位置，并且有static修饰

​		1.可以直接访问外部类的所有静态成员，包含私有的，**但不能直接访问非静态成员**

​		2.可以添加任意访问修饰符(public、protetted、默认、private),因为它的地位就是一个成员。

​		3.作用域：同其他的成员，为整个类体

​		4.静态内部类---访问---->外部类(比如：静态属性)【访问方式：直接访问所有静态成员]

 		5.外部类---访问------>静态内部类 访问方式：创建对象，再访问

​		6.外部其他类---访问----->静态内部类

​		7.如果外部类和静态内部类的成员重名时，静态内部类访问的时，默认遵循就近原则；

​			如果想访问外部类的成员，则可以使用（外部类名.成员）去访问

## 9.枚举和注解

枚举属于一种特殊的类，里面只包含一组有限的特定的对象。是一组常量的集合。

#### 自定义实现枚举

```java
//定义了四个对象, 固定. 
public static final Season SPRING = new Season("春天", "温暖");
public static final Season WINTER = new Season("冬天", "寒冷");
public static final Season AUTUMN = new Season("秋天", "凉爽");
public static final Season SUMMER = new Season("夏天", "炎热");
```

注意：

自定义实现枚举的时候有一下两点：

- 将构造器私有化,目的防止 直接 new 
- 去掉 setXxx 方法, 防止属性被修改

#### enum 关键字

```java
enum Season2 {
	//如果使用了 enum 来实现枚举类
	//1. 使用关键字 enum 替代 class
	//2. public static final Season SPRING = new Season("春天", "温暖") 直接使用
	// SPRING("春天", "温暖") 解读 常量名(实参列表)
	//3. 如果有多个常量(对象)， 使用 ,号间隔即可
	//4. 如果使用 enum 来实现枚举，要求将定义常量对象，写在前面
	//5. 如果我们使用的是无参构造器，创建常量对象，则可以省略 ()
    SPRING("春天", "温暖"), WINTER("冬天", "寒冷"), AUTUMN("秋天", "凉爽"), SUMMER("夏天", "炎热");
    private String name;
    private String desc;//描述
    private Season2() {//无参构造器
    }
    private Season2(String name, String desc) {
        this.name = name;
        this.desc = desc;
    }
    public String getName() {
        return name;
    }
    public String getDesc() {
        return desc;
    }
    @Override
    public String toString() {
        return "Season{" +
                "name='" + name + '\'' +
                ", desc='" + desc + '\'' +
                '}';
    }
}
```

## 10.异常处理

一段代码可能出现异常问题，可以使用 try-catch 异常处理机制，从而保证程序的健壮性。异常分为**编译时异常**和**运行时异常**，运行时异常可以不做处理，但是，**编译时异常是编译器必须解决的异常**。

### 1.try-catch解决异常

- 如果异常发生了，则异常发生后面的代码不会执行，直接进入到 catch 块 ；
- 如果异常没有发生，则顺序执行 try 的代码块，不会进入到 catch 
- 如果希望不管是否发生异常，都执行某段代码(比如关闭连接，释放资源等)则使用如下代码- finally
- 如果 try 代码块有可能有多个异常 ，可以使用多个 catch 分别捕获不同的异常，相应处理 ，要求子类异常写在前面，父类异常写在后面。

try-catch-finally执行顺序小结

- 如果没有出现异常，则执行try块中所有语句，不执行catch块中语句，如果有finally，最后还需要执行finally里面的语句
- 如果出现异常，则try块中异常发生后，try块剩下的语句不再执行。将执行catch块中的语句，如果有finally，最后还需要执行finally里面的语句！

### 2.throws 异常处理

- 对于编译异常，程序中必须处理，比如 try-catch 或者 throws 

- 对于运行时异常，程序中如果没有处理，默认就是 throws 的方式处理

- 子类重写父类的方法时，对抛出异常的规定:子类重写的方法，所抛出的异常类型要么和父类抛出的异常一致，要么为父类抛出的异常类型的子类型 。在 throws 过程中，如果有方法 try-catch ，就相当于处理异常，就可以不必throws

## 11.常用类

### 1.包装类

装箱：基本类型->包装类型；反之就是拆箱

```java
int n1=100;
//int->Integer 装箱
Integer integer=new Integer(n1);
//Integer->int 拆箱
int i = integer.intValue();
//jdk5 后，就可以自动装箱和自动拆箱
int n2 = 200;
//自动装箱 int->Integer
Integer integer2 = n2; //底层使用的是 Integer.valueOf(n2)
//自动拆箱 Integer->int
int n3 = integer2; //底层仍然使用的是 intValue()方
```

### 2.String类

#### 1.创建 String 对象的两种方式

​	方式一：直接赋值 **String s="tom";**

​		先从常量池查看是否有“tom”数据空间，如果有，直接指向；

​		如果没有，则先创建然后指向，s最终指向的是常量池的空间地址。

​	方式二：调用构造器 **String s2 = new String("tom");**

​		先在堆中创建空间，里面维护了value属性，指向常量池的tom空间;

​		如果常量池没有"tom”，重新创建，如果有，直接通过value指向。最终指向的是堆中的空间地址。

```java
		String a="zjl";
        String b="zjl";
  		System.out.println(a.equals(b));//true
        System.out.println(a==b);//true
        String m=new String("zjl");
        String n=new String("zjl");
       	System.out.println(m.equals(n));//true
        System.out.println(m==n);//false
```

#### 2.字符串的特性

​	1）String是一个final类，代表不可变的字符序列。

​	2）字符串是不可变的，一个字符串对象一旦被分配，其内容不可变。

​	String类是保存字符串常量的。**每次更新都需要重新开辟空间，效率较低**，因此java设计者还提供了StringBuilder 和 StringBuffer 来增强String的功能,并提高效率。

#### 3.String类的常见方法一览

- ​	equals		//区分大小写，判断内容是否相等

- ​	equalslgnoreCase		//忽略大小写的判断内容是否相等

- ​	length	 // 获取字符的个数，字符串的长度

- ​	indexOf 	//获取字符在字符串中第1次出现的索引，索引从0开始，如果找不到，返回-1

- ​	lastlndexOf 	//获取字符在字符串中最后1次出现的索引，索引从0开始,如找不到，返回-1

- ​	substring	 //截取指定范围的子串

- ​	trim 		//去前后空格

- ​	charAt		//获取某索引处的字符，注意不能使用Str[index] 这种方式.

- ​	toUpperCase

- ​	toLowerCase

- ​	concat

- ​	replace 	//替换字符串中的字符

- ​	split 		//分割字符串,对于某些分割字符，我们需要 转义比如|\\等

- ​	compareTo	//比较两个字符串的大小

- ​	toCharArray	//转换成字符数组

- ​	format	//格式字符串，**%s字符串%c字符%d整型%.2f浮点型**

### 3.StringBuffer 类

​	1、StringBuffer表示可变字符序列，可以对字符串的内容进行增删，其很多方法与String类相同，但是其长度可变。

​	2、 StringBuffer 是一个 final 类，不能被继承。

​	3、StringBuffer 的直接父类 是 AbstractStringBuilder ，StringBuffer 实现了 Serializable, 即**StringBuffer 的对象可以串行化** 

​	4、在父类中 AbstractStringBuilder 有属性 char[] value,不是 final

​	5、因为 StringBuffer 字符内容是存在 char[] value, 所有在变化(增加/删除)， 不用每次都更换地址 (即不是每次创建新对象)， 所以效率高于 String

#### String与StringBuffer转换

```java
//string->Stringbuffer 		
StringBuffer stringBuffer = new StringBuffer("HELLO");
String str="zhangjialiang";
//方式 2 使用的是 append 方法
StringBuffer stringBuffer1 = new StringBuffer();
stringBuffer1 = stringBuffer1.append(str);    
str=stringBuffer.toString();// StringBuffer ->String
System.out.println(str);
String str1=new String(stringBuffer);//方式 2: 使用构造器来搞定
```

### 4.StringBuilder类

- ​	一个可变的字符序列。此类提供一个与 StringBuffer 兼容的API，**但不保证同步(StringBuilder 不是线程安全)**。

  ​	该类被设计用作 StringBuffer 的一个简易替换，用在字符串缓冲区被单个线程使用的时候。

  ​	如果可能，建议优先采用该类,因为在大多数实现中，它比 StringBuffer 要快。

- ​	在 StringBuilder 上的主要操作是 append 和 insert 方法，可重载这些方法，**以接受任意类型的数据**。

- ​	StringBuilder 的直接父类 是 AbstractStringBuilder，StringBuilder 实现了 Serializable, 即 StringBuilder的对象可以串行化 

- ​	StringBuilder 对象字符序列仍然是存放在其父类 AbstractStringBuilder 的 char[] value; 因此，字符序列是堆中

- ​	StringBuilder 是 final 类, 不能被继承。


- ​	StringBuilder 的方法，没有做互斥的处理,**即没有 synchronized 关键字,因此在单线程的情况下使用**


####  1.String、StringBuffer 和 StringBuilder 的比较

- ​	StringBuilder 和 StringBuffer 非常类似，均代表可变的字符序列，而且方法也一样

- ​	String：不可变字符序列，效率低，但是复用率高。

- ​	StringBuffer：可变字符序列、效率较高(增删)、线程安全

- ​	StringBuilder：可变字符序列、效率最高、线程不安全


String使用注意说明：

```java
	string s="a";//创建了一个字符串
	s+="b";//实际上原来的“a"字符串对象已经丢弃了，现在又产生了一个字符串s+"b"(也就是"ab")。
```

​		如果多次执行这些改变串内容的操作，会导致大量副本字符串对象存留在内存中，降低效率。如果这样的操作放到循环中，会极大影响程序的性能

结论：**如果我们对String 做大量修改,不要使用String 。**

###  5.其他类

#### 	1.Math 类

​			**Math.random() * (b-a) 	// 0 <= 数 <= b-a**

​			**Math.random()*6 返回的是 0 <= x < 6 小数**

​			**2 + Math.random()*6 返回的就是 2<= x< 8 小数**

```java
int random= (int) (Math.random()*(10-1));//返回0-9的随机数
int random1= (int) (Math.random()*6);//返回[0,6)
int random2= (int) (Math.random()*6+2);//返回[2,7)
```

#### 	2.Arrays 类

​	Arrays里面包含了一系列静态方法，用于管理或操作数组(比如排序和搜索).

​	1）toString 返回数组的字符串形式

```java
Arrays.toString(arr);
```

​	2) sort 排序（自然排序和定制排序) 

```java
Integer arr[] = {1,-1, 7, 0, 89};
```

​	3) binarySearch 通过二分搜索法进行查找，**要求必须排好序**

```java
int index= Arrays.binarySearch(arr, 3);
```

​	4)copyOf 数组元素的复制

​	5)fill数组元素的填充

```java
Integer[] newArr = Arrays.copyOf(arr, arr.length);
Integer[] num = new Integer[]{9,3,});
Arrays.fill(num, 99);
```

​	6)equals 比较两个数组元素内容是否完全一致

```java
boolean equals = Arrays.equals(arr, arr2);
```

​	7) asList 将一组值，转换成list

```java
List<Integer> asList = Arrays.asList(2,3,4,5,6,1);
```

#### 3.System 类

​	1、exit(0) 表示程序退出，0 表示一个状态 , 正常的状态。

​	2、arraycopy ：复制数组元素，比较适合底层调用，一般使用 Arrays.copyOf 完成数组复制。

​	3、currentTimeMillens:返回当前时间距离 1970-1-1 的毫秒数。

#### 4.BigInteger 和 BigDecimal 类

在对 BigInteger 进行加减乘除的时候，需要使用对应的方法，不能直接进行 + - * /，可以创建一个要操作的 BigInteger 然后进行相应操作。

```java
BigInteger add = bigInteger.add(bigInteger2);//add、subtract、multiply、divide
```

对 BigDecimal 进行运算，比如加减乘除，需要使用对应的方法

```java
bigDecimal.add(bigDecimal2)；	//add、subtract、multiply、divide
```

#### 5.日期类

格式化日期（将日期转换为字符串）

```java
Date date = new Date();
SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 hh:mm:ss E");
String format=sdf.format(date);
```

字符串转日期类，使用的 **sdf 格式需要和你给的 String 的格式一样**，否则会抛出类型转换异常。

```java
String s="2022年12月24日 10:23:30 星期六";
Date parse = sdf.parse(s);
System.out.println("parse="+sdf.format(parse));
```

Calendar类

Calendar 类是一个抽象类，它为特定瞬间与一组诸如 YEAR、MONTH.DAY OF MONTH、HOUR 等 日历字段之间的转换提供了一些方法，并为操作日历字段（例如获得下星期的日期）提供了一些方法。

```java
Calendar c=Calendar.getInstance();//首先，实例化对象
System.out.println("年："+c.get(Calendar.YEAR));
System.out.println("月"+c.get(Calendar.MONTH)+1);
System.out.println("日"+c.get(Calendar.HOUR_OF_DAY));
System.out.println("天"+c.get(Calendar.DAY_OF_MONTH));
System.out.println("秒"+c.get(Calendar.SECOND));
```

第三代日期类

前面两代日期类的不足分析

JDK 1.0中包含了一个java.util.Date类，但是它的大多数方法已经在JDK 1.1引入

Calendar类之后被弃用了。而Calendar也存在问题是:

- ​	可变性：像**日期和时间这样的类应该是不可变的**。

- ​	偏移性：Date中的年份是从1900开始的，而月份都从0开始。

- ​	格式化：格式化只对Date有用，Calendar则不行。

- ​	此外，它们也不是线程安全的；不能处理闰秒等（每隔2天，多出1s）。


LocalDate(年月日)、LocalTime(时间/时分秒)、LocalDateTime(日间/年月日时分秒)JDK8加入

​	LocalDate只包含日期，可以获取日期字段

​	LocalTime只包含时间，可以获取时间字段

​	LocalDateTime包含日期+时间，可以获取日期和时间字段

```java
LocalDateTime ldt = LocalDateTime.now(); //LocalDate.now();
DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
String format = dateTimeFormatter.format(ldt);
System.out.println("格式化日期"+format);
System.out.println(ldt.getYear()+"-"+ldt.getMonth()+"-"+ldt.getMonthValue()
+"-"+ldt.getHour()+":"+ldt.getMinute()+":"+ldt.getSecond());
LocalDate now = LocalDate.now(); //可以获取年月日
LocalTime now1 = LocalTime.now();//获取到时分秒
//看看 890 天后，是什么时候 把 年月日-时分秒
LocalDateTime localDateTime = ldt.plusDays(890);
//看看在 3456 分钟前是什么时候，把 年月日-时分秒输出
LocalDateTime localDateTime2 = ldt.minusMinutes(3456);
```

Instant 时间戳

```java
//1.通过 静态方法 now() 获取表示当前时间戳的对象 
Instant now = Instant.now(); System.out.println(now); 
//2. 通过 from 可以把 Instant 转成 
Date Date date = Date.from(now)
//3. 通过 date 的 toInstant() 可以把 date 转成 Instant 对象 
Instant instant = date.toInstant()
```

## 12.集合和泛型

###  1.Collection接口

1. 集合主要是两组(单列集合 , 双列集合) 
2. Collection 接口有两个重要的子接口 List Set , 他们的实现子类都是单列集合 
3. Map 接口的实现子类 是双列集合，存放的 K-V

collection接口的特点

1. colletion实现子类可以存放多个元素，每个元素可以是Object
2. 有些Collection的实现类，可以存放重复的元素，有些不可以

3. 有些Collection的实现类，有些是**有序的(List)**，有些**不是有序(Set)**

4. **Collection接口没有直接的实现子类，是通过它的子接口Set 和 List 来实现的**

```java
List list = new ArrayList();
// add:添加单个元素
list.add("jack");
list.add(10);//list.add(new Integer(10))
list.add(true);
System.out.println("list=" + list);
// remove:删除指定元素
list.remove(0);//删除第一个元素
list.remove(true);//指定删除某个元素
System.out.println("list=" + list);
System.out.println(list.contains("jack"));//T	// contains:查找元素是否存在
System.out.println(list.size());//2	// size:获取元素个数
System.out.println(list.isEmpty());//F // isEmpty:判断是否为空
list.clear();// clear:清空
System.out.println("list=" + list);
// addAll:添加多个元素
ArrayList list2 = new ArrayList();
list2.add("红楼梦");
list2.add("三国演义");
list.addAll(list2);
System.out.println("list=" + list);
// containsAll:查找多个元素是否都存在
System.out.println(list.containsAll(list2));//T
list.add("聊斋");	
list.removeAll(list2);	// removeAll：删除多个元素
```

#### 使用迭代器遍历

1. lterator对象称为迭代器，主要用于遍历 Collection 集合中的元素。

2. **所有实现了Collection接口的集合类都有一个iterator()方法**，用以返回一个实现了Iterator接口的对象，即可以返回一个迭代器。
3. lterator 仅用于遍历集合，**Iterator 本身并不存放对象**。

```java
Collection col = new ArrayList();
col.add(new Book("三国演义", "罗贯中", 10.1));
col.add(new Book("小李飞刀", "古龙", 5.1));
col.add(new Book("红楼梦", "曹雪芹", 34.6));
//1. 先得到 col 对应的迭代器
Iterator iterator= col.iterator();
//使用while循环来遍历
while(iterator.hasNext()){
    //返回下一个元素，类型是 Obj
    Object next = iterator.next();
    System.out.println(next.toString());
}
```

#### 使用增强for遍历

```java
for (Object book:col) {
    System.out.println(book.toString());
}
```

### 2.List 接口

List接口是 Collection 接口的子接口 

- ​	List集合类中元素有序(即添加顺序和取出顺序一致)、且可重复

- ​	List集合中的每个元素都有其对应的顺序索引，即支持索引


List容器中的元素都对应一个整数型的序号记载其在容器中的位置，可以根据序号存取容器中的元素。

常用api

```java
/**
 * Object get(int index)//获取指定 index 位置的元素
 * int indexOf(Object obj):返回 obj 在集合中首次出现的位置
 * int lastIndexOf(Object obj):返回 obj 在当前集合中末次出现的位置
 * Object remove(int index):移除指定 index 位置的元素，并返回此元素
 * Object set(int index, Object ele):设置指定 index 位置的元素为 ele , 相当替换
 * List subList(int fromIndex, int toIndex):返回从 fromIndex 到 toIndex 位置的子集合
 *
 */
```

#### List 的三种遍历方式

```java
List list = new ArrayList();
	list.add(new Book("三国演义", "罗贯中", 10.1));
	list.add(new Book("小李飞刀", "古龙", 5.1));
	list.add(new Book("红楼梦", "曹雪芹", 34.6));
//1. 迭代器
Iterator iterator = list.iterator();
while (iterator.hasNext()) {
    Object obj = iterator.next();
    System.out.println(obj);
}
System.out.println("=====增强 for=====");
//2. 增强 for
for (Object o : list) {
    System.out.println("o=" + o);
}
System.out.println("=====普通 for====");
//3. 使用普通 for
for (int i = 0; i < list.size(); i++) {
    System.out.println("对象=" + list.get(i));
}
```

#### ArrayList

ArrayList 的注意事项

- permits all elements, including null，ArrayList 可以加入null，并且多个

- ArrayList 是由数组来实现数据存储的[后面老师解读源码]

- ArrayList 基本等同于Vector，除了ArrayList是线程不安全（执行效率高）看源码.在多线程情况下，不建议使用ArrayList


ArrayList的底层操作机制源码分析）

- ArrayList中维护了一个Object类型的数组elementData.


```java
transient Objectll elementData;//transient 表示瞬间，短暂的，表示该属性不会被序列号
```

- 当创建ArrayList对象时，如果使用的是无参构造器，则初始elementData容量为0，第1次添加，则扩容elementData为10，

  如需要再次扩容，则扩容elementData为1.5倍。

- 如果使用的是指定大小的构造器，则初始elementData容量为指定大小，如果需要扩容,则直接扩容elementData为1.5倍。


#### Vector集合

1. Vector底层也是一个对象数组，protected Object[] elementData;

2. Vector 是线程同步的，即线程安全,Vector类的操作方法带有synchronized

```java
public synchronized void copyInto(Object[] anArray) {
    System.arraycopy(elementData, 0, anArray, 0, elementCount);
}
```

#### Vector和ArrayList比较																																			

| ArrayList | 可变数组 | jdk1.2 | 线程不安全，效率高 | 如果有参构造1.5倍，如果是无参第一次10，从第二次开始安1.5扩   |
| --------- | -------- | ------ | ------------------ | ------------------------------------------------------------ |
| Vector    | 可变数组 | jdk1.0 | 安全，效率不高     | 如果是无参，默认10，满后，就按2倍扩容，如果指定大小，则每次直接按2倍扩容. |

#### LinkedList						                   

LinkedList的全面说明

- LinkedList底层实现了双向链表和双端队列特点

- 可以添加任意元素(元素可以重复)，包括null 

- 线程不安全，没有实现同步


LinkedList的底层操作机制

- ​	LinkedList底层维护了一个双向链表.

- ​	LinkedList中维护了两个属性first和last分别指向 首节点和尾节点

- ​	每个节点（Node对象），里面又维护了prev、next、item三个属性，其中通过prev指向前一个，通过next指向后一个节点。最终实现双向链表.

- ​	所以LinkedList的元素的添加和删除，不是通过数组完成的，相对来说效率较高。


#### ArrayList 和 LinkedList 的比较					          

|            | 底层结构 | 增删的效率         | 改查的效率 |
| ---------- | -------- | ------------------ | :--------- |
| Arra       | 可变数组 | 较低数组扩容       | 较高       |
| LinkedList | 双向链表 | 较高，通过链表追加 | 较低       |

如何选择

- 如果我们改查的操作多，选择ArrayList

- 如果我们增删的操作多，选择LinkedList

- 一般来说，在程序中，80%-90%都是查询，因此大部分情况下会选择ArrayList，但是，在一个项目中，根据业务灵活选择，


### 3.Set 接口

特点：

- 无序，没有索引。

- 不允许重复元素，所以最多包含一个null。
- 和 List 接口一样, Set 接口也是 Collection 的子接口，因此，常用方法和 Collection 接口一样

#### 遍历方式

```java
Set set = new HashSet();
set.add("john");
set.add("lucy");
set.add("john");//重复
set.add("jack");
set.add("hsp");
Iterator iterator=set.iterator();
//迭代器遍历
while (iterator.hasNext()){
    Object obj=iterator.next();
    System.out.println(obj);
}
//增强for
for (Object objs:set) {
    System.out.println(objs);
}
```

#### HashSet

- HashSet实现了Set接口

- HashSet实际上是HashMap.

  ```java
  public HashSet() { 
      map = new HashMap<>(); 
  }
  ```

- 可以存放null值，但是只能有一个null

- HashSet不保证元素是有序的,取决于hash后，再确定索引的结果.(即，不保证存放元素的顺序	和取出顺序一致）

- 不能有重复元素/对象.在前面Set接口使用已经讲过

#### HashSet 的底层逻辑

1. HashSet 底层是HashMap
2. 添加一个元素时，先得到hash值-会转成->索引值
3. 找到存储数据表table，看这个索引位置是否已经存放的有元素
4. 如果没有，直接加入
5. 如果有，调用equals比较，如果相同，就放弃添加，如果不相同，则添加到最后
6. 在Java8中，如果一条链表的元素个数到达TREEIFYTHRESHOLD（默认是8)，并且table的大小>=MIN TREEIFY_CAPACITY(默认64)，就会进行树化(红黑树）

#### LinkedHashSet

- ​	LinkedHashSet 是 HashSet 的子类

- ​	LinkedHashSet 底层是一个 LinkedHashMap，底层维护了一个 数组+双向链表

- ​	LinkedHashSet 根据元素的hashCode 值来决定元素的存储位置，同时使用链表维护元素的次序，这使得元素看起来是以插入顺序保存的。

- ​	LinkedHashSet不允许添重复元素

### 4.Map 接口

#### Map 接口实现类的特点

1. **Map与Collection并列存在**。用于保存具有映射关系的数据:Key-Value
2. Map 中的key 和 value 可以是**任何引用类型的数据**，会封装到HashMap$Node对象中

3. Map 中的 key 不允许重复

4. Map中的value 可以重复

5. Map 的key 可以为 null,value 也可以为null，注意 key 为null，只能有一个，value 为null，可以多个.

6. 常用String类作为Map的key

7. key 和 value 之间存在单向一对一关系，即通过指定的key 总能找到对应的 value

#### Map相关的Api

```java
Map map = new HashMap();
map.put("李四", new Book("三国演义", "罗贯中", 10.1));
map.put("张三","100");
map.put("李四","120");//此处会替换
System.out.println(map.get("李四"));
// remove:根据键删除映射关系
map.remove(null);
System.out.println("map=" + map);
// get：根据键获取值
Object val = map.get("鹿晗");
System.out.println("val=" + val);
// size:获取元素个数
System.out.println("k-v=" + map.size());
// isEmpty:判断个数是否为 0
System.out.println(map.isEmpty());//F
// clear:清除 k-v
map.clear();
System.out.println("map=" + map);
// containsKey:查找键是否存在
System.out.println("结果=" + map.containsKey("hsp"));//T
```

#### Map 接口遍历方法

##### 方式一：

先取出所有的key，然后再获得value值

```java
Map map = new HashMap();
    map.put("张三","100");
    map.put("李四","120");
//第一组: 先取出 所有的 Key , 通过 Key 取出对应
Set keys=map.keySet();
//第一种遍历方式，增强for
for (Object key:keys) {
    System.out.println("key="+key+":"+"value="+map.get(key));
}
System.out.println("----第二种方式迭代器--------");
Iterator iterator = keys.iterator();
 while (iterator.hasNext()) {
   Object key = iterator.next();
       System.out.println(key + "-" + map.get(key));
 }
```

##### 方式二：

把所有的 values 取出来

```java
Collection collections=map.values();
//foreach
for (Object value:collections) {
    System.out.println(value);
}
//迭代器
Iterator iterator=collections.iterator();
while (iterator.hasNext()){
    Object objs=iterator.next();
    System.out.println(objs);
}
```

##### 方式三:

通过 **EntrySet** 来获取 k-v

```java
Set entrySet = map.entrySet();// EntrySet<Map.Entry<K,V>>
//(1) 增强 for
for (Object entry : entrySet) {
//将 entry 转成 Map.Entry
    Map.Entry m = (Map.Entry) entry;
    System.out.println(m.getKey() + "-" + m.getValue());
}
//(2) 迭代器
Iterator iterator = entrySet.iterator();
while (iterator.hasNext()) {
    Object entry = iterator.next();
    //System.out.println(next.getClass());//HashMap$Node -实现-> Map.Entry (getKey,getValue)
    //向下转型 Map.Entry
    Map.Entry m = (Map.Entry) entry;
    System.out.println(m.getKey() + "-" + m.getValue());
}
```

#### HashMap总结

- Map接口的常用实现类：HashMap、Hashtable和Properties。

- HashMap是 Map接口使用频率最高的实现类。

- HashMap 是以key-val 对的方式来存储数据(HashMapSNode类型）

- key 不能重复，但是值可以重复，允许使用null键和null值。


- 如果添加相同的key，则会覆盖原来的key-val，等同于修改.（key不会替换，val会替换)6）与HashSet一样，不保证映射的顺序，因为底层是以hash表的方式来存储的.(jdk8的hashMap 底层 数组+链表+红黑树）


- HashMap没有实现同步，因此是线程不安全的，方法没有做同步互斥的操作，没有synchronized


#### Hashtable

- 存放的元素是键值对：即K-V

- hashtable的键和值都不能为null，否则会抛出NuPointerException
- hashTable 使用方法基本上和HashMap一样
- hashTable 是线程安全的(synchronized),hashMap 是线程不安全的

#### Properties

- Properties类继承自Hashtable类并且实现了Map接口，也是使用一种健值对的形式来保存数据。
- Properties 还可以用于从xcxc.properties 文件中，加载数据到Properties类对象

```java
properties.put(null, "abc");//抛出 空指针异常
properties.put("abc", null); //抛出 空指针异常
properties.put("john", 100);//k-v
properties.put("lic", 100);
properties.put("lic", 88);//如果有相同的 key ， value 被替换
```

### 5.泛型

泛型的作用是：可以在类声明时通过一个标识表示类中某个属性的类型，

## 13.多线程基础

Java 提供了三种创建线程的方法：

- 通过实现 Runnable 接口；
- 通过继承 Thread 类本身；
- 通过 Callable 和 Future 创建线程。

### Thread

创建一个新的类，该类继承 Thread 类，然后创建一个该类的实例。继承类必须重写 run() 方法，该方法是新线程的入口点。它也必须调用 start() 方法才能执行。

该方法尽管被列为一种多线程实现方式，但是本质上也是实现了 Runnable 接口的一个实例。

```java
public class Cat  extends  Thread{
  int times=0;
    @Override
    public void run() {
        while (true){
            System.out.println("我是小猫"+(++times)+"线程名："+(Thread.currentThread().getName()));
            //让该线程休眠 1 秒 ctrl+alt+
            try {
                Thread.sleep(1000);
            }catch (InterruptedException e) {
                e.printStackTrace();
            }
            if(times==80){
                break; //当秒数等于80退出
            }
        }
    }
}
```

```java
Cat cat=new Cat();
cat.start();
System.out.println("主线程继续执行" + Thread.currentThread().getName());//名字 main
for(int i = 0; i < 60; i++) {
    System.out.println("主线程 i=" + i);
    //让主线程休眠
    Thread.sleep(1000);
}
/**
控制台打印的结果
我是小猫19线程名：Thread-0
主线程 i=19
我是小猫20线程名：Thread-0
主线程 i=20
我是小猫21线程名：Thread-0
主线程 i=21
我是小猫22线程名：Thread-0
主线程 i=22
我是小猫23线程名：Thread-0
**/
```

### 实现 Runnable 接口

java是单继承机制，在某些情况下，程序可能已经继承了某个父类，此时，再使用thread就不太合适，所以我们提供了Runnable接口

```java
public class T1 implements Runnable{
    int counts=0;
    @Override
    public void run() {
        while (true){
            System.out.println("你好，我是t1，这是第"+(++counts)+"次");
            try {
                //休眠1s
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
            if(counts==70){
                return;
            }
        }
    }
}
public class T2 implements  Runnable{

        int counts=0;
        @Override
        public void run() {
            while (true){
                System.out.println("你好，我是t2，这是第"+(++counts)+"次");
                try {
                    //休眠1s
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
                if(counts==50){
                    return;
                }
            }
        }

}
//main
 		T2 t2 = new T2();
        T1 t1 = new T1();
        t1.run();
        t2.run();
```

### Thread vs Runnable

1.从java的设计来看，通过继承Thread或者实现Runnable接口来创建线程本质上没有区别，从jdk帮助文档我们可以看到Thread类本身就实现了，Runnable接口

2.实现**Runnable接口方式更加适合多个线程共享一个资源的情况**，并且避免了单继承的限制，建议使用Runnable

### 线程常用方法

```java
setName //设置线程名称，便之与参数name 相同
getName //返回该线程的名称
start//使该线程开始执行；Java 虚拟机底层调用该线程的startO方法
run//调用线程对象 run方法；
setPriority //更改线程的优先级
getPriority //获取线程的优先级
sleep//在指定的毫秒数内让当前正在执行的线程休眠（暂停执行）
interrupt //中断线程
```

注意事项和细节

1.start 底层会创建新的线程，调用run，run就是一个简单的方法调用，不会启动新线程

2.线程优先级的范围

3.interrupt，中断线程，但并没有真正的结束线程。所以一般用于中断正在休眠线程

4.sleep：线程的静态方法，使当前线程休眠

- yield：线程的礼让。让出cpu，让其他线程执行，但礼让的时间不确定，所以也不一定礼让成功

- join：线程的插队。插队的线程一旦插队成功，则肯定先执行完插入的线程所有的任务

```java
public class SonThread extends  Thread{
    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            try {
                //休眠1s
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
            System.out.println("儿子线程吃了"+i+"个包子");
        }
    }
}

public static void main(String[] args) throws InterruptedException {
        SonThread sonThread=new SonThread();
        sonThread.run();
        for (int i = 0; i < 20; i++) {
            Thread.sleep(1000);
            System.out.println("父线程吃包子，吃了第"+i+"个包子");
            if(i == 5) {
                System.out.println("父线程让 子线程先吃");
                //join, 线程插队
                //sonThread.join();// 这里相当于让子线程先执行完毕
                Thread.yield();//礼让，不一定成功..
                System.out.println("子线程吃完了 父线程接着吃..");
            }
        }
    }
```

### 用户线程和守护线程

**守护线程定义：** 所谓守护线程，是指在程序运行的时候在后台提供一种通用服务的线程。比如**垃圾回收线程**就是一个很称职的守护者，并且这种线程并不属于程序中不可或缺的部分。因此，当所有的非守护线程结束时，程序也就终止了，同时会杀死进程中的所有守护线程。反过来说，只要任何非守护线程还在运行，程序就不会终止。

**用户线程定义：** 某种意义上的主要用户线程，只要有用户线程未执行完毕，JVM 虚拟机不会退出。

**区别：** 在本质上，用户线程和守护线程并没有太大区别，唯一的区别就是当最后一个非守护线程结束时，JVM 会正常退出，而不管当前是否有守护线程，也就是说守护线程是否结束并不影响 JVM 的退出。言外之意，只要有一个用户线程还没结束， 正常情况下 JVM 就不会退出。

```java
public class MyDaemonThread extends Thread{
    public static void main(String[] args) throws InterruptedException {
        MyDaemonThread myDaemonThread=new MyDaemonThread();
        //如果我们希望当main线程结束后，子线程自动结束
        //,只需将子线程设为守护线程即可
        myDaemonThread.setDaemon(true);
        myDaemonThread.start();
        for (int i = 0; i <10 ; i++) {
            Thread.sleep(1000);
            System.out.println("辛苦工作赚钱");
        }
    }
    @Override
    public void run() {
        for (;  ;) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
            System.out.println("我是守护线程，我不推出");
        }
    }
}
```

### 线程的同步和互斥锁

#### 线程同步

处理多线程问题时，多个线程访问同一个对象，并且某些线程还想修改这个对象。这时需要线程同步，线程同步是一种等待机制

由于同一进程的多个线程共享一块存储空间，在带来方便的同时，也带来了访问冲突问题，为了保证数据在方法中被访问时的正确性，在访问时加锁机制```synchronized```，当一个线程获取到对象的排他锁，独占资源，其他线程必须等待，使用后释放锁即可，存在以下问题一个线程持有锁会导致其他所有需要此锁等待线程挂起在多线程竞争下，加锁，释放锁会导致比较多的上下文切换和调度延时，引起性能问题，如果一个优先级高的线程等待一个优先级低的线程释放锁会导致优先级倒置，引起性能问题

同步代码块

```java
synchronized（对象）{//得到对象的锁，才能操作同步代码
        //需要被同步代码；
}
//synchronized还可以放在方法声明中，表示整个方法-为同步方法
public synchronized void m (String name){
        //需要被同步的代码
}
```

#### 互斥锁

1.Java语言中，引入了对象互斥锁的概念，来保证共享数据操作的完整性。

2.每个对象都对应于一个可称为“互斥锁”的标记，这个标记用来保证在任一时刻，只能有一个线程访问该对象。

3.关键字synchronized 来与对象的互斥锁联系。当某个对象用```synchronized```修饰时，表明该对象在任一时刻只能由一个线程访问

4.同步的局限性：导致程序的执行效率要降低

5.同步方法（非静态的）的锁可以是this，也可以是其他对象（要求是同一个对象)

6.同步方法（静态的）的锁为当前类本身。

## 14.IO流

### 1.文件操作

#### 创建文件

```java
//方式1 new File(String pathname)
@Test
public void create01() {
    String filePath = "e:\\news1.txt";
    File file = new File(filePath);
    try {
        file.createNewFile();
        System.out.println("文件创建成功");
    } catch (IOException e) {
        e.printStackTrace();
    }

}
//方式2 new File(File parent,String child) //根据父目录文件+子路径构建
//e:\\news2.txt
@Test
public  void create02() {
    File parentFile = new File("e:\\");
    String fileName = "news2.txt";
    //这里的file对象，在java程序中，只是一个对象
    //只有执行了createNewFile 方法，才会真正的，在磁盘创建该文件
    File file = new File(parentFile, fileName);
    try {
        file.createNewFile();
        System.out.println("创建成功~");
    } catch (IOException e) {
        e.printStackTrace();
    }
}
//方式3 new File(String parent,String child) //根据父目录+子路径构建
@Test
public void create03() {
    //String parentPath = "e:\\";
    String parentPath = "e:\\";
    String fileName = "news4.txt";
    File file = new File(parentPath, fileName);
    try {
        file.createNewFile();
        System.out.println("创建成功~");
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

#### 获取文件信息

```java
	//先创建文件对象 	
	File file = new File("e:\news1.txt");
	//调用相应的方法，得到对应信息
    System.out.println("文件名字=" + file.getName());
    //getName、getAbsolutePath、getParent、length、exists、isFile、isDirectory
    System.out.println("文件绝对路径=" + file.getAbsolutePath());
    System.out.println("文件父级目录=" + file.getParent());
    System.out.println("文件大小(字节)=" + file.length());
    System.out.println("文件是否存在=" + file.exists());//T
    System.out.println("是不是一个文件=" + file.isFile());//T
    System.out.println("是不是一个目录=" + file.isDirectory());//F
```
### 2.IO流

inputstream 字节输入流		 reader（字符输入流）

outputstream 字节输出流		writer（字符输出流）

#### 1.FileInputStream介绍

```java
//单个字节的读取，效率比较低
@Test
public void readFile01() throws IOException {
    String filePath = "f:\\hello.txt";
    FileInputStream fileInputStream=null;
    int readData=0;
    try {
        //创建字节输入流对象
        fileInputStream = new FileInputStream(filePath);
        //如果返回-1，表示读取完毕
        while((readData=fileInputStream.read())!=-1){
            System.out.print((char)readData);
        }
    } catch (IOException e) {
        e.printStackTrace();
    }finally {
        //关闭流释放资源
        fileInputStream.close();
    }
}
```

```java
@Test
public void readFile02() throws IOException {
    FileInputStream fileInputStream = null;
    int readLen = 0;
    //定义一个字符数组，每次读取8个字节效率高
    byte[] bytes = new byte[8];
    String filePath = "f:\\hello.txt";
    try {
        //创建字节输入流对象
        fileInputStream = new FileInputStream(filePath);
        //从该输入流读取最多 b.length 字节的数据到字节数组。 此方法将阻塞，直到某些输入可
        //如果返回-1，表示读取完毕
        //如果读取正常，返回实际读取的字节数
        while ((readLen = fileInputStream.read(bytes)) != -1) {
            System.out.print(new String(bytes, 0, readLen));
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        //关闭流释放资源
        fileInputStream.close();
    }
}
```

#### 2.FileOutputStream介绍

用于将数据写入到文件之中

```java
 /**
     * 演示使用FileOutputStream 将数据写到文件中,
     * 如果该文件不存在，则创建该文件
     */
    @Test
    public void writeFile() {
        //创建 FileOutputStream对象
        String filePath = "e:\\news1.txt";
        FileOutputStream fileOutputStream = null;
        try {
            //得到 FileOutputStream对象 对象
            //说明
            //1. new FileOutputStream(filePath) 创建方式，当写入内容是，会覆盖原来的内容
            //2. new FileOutputStream(filePath, true) 创建方式，当写入内容是，是追加到文件后面
            fileOutputStream = new FileOutputStream(filePath, true);
            //写入一个字节
            //fileOutputStream.write('H');//
            //写入字符串
            String str = "hello,world!";
            //str.getBytes() 可以把 字符串-> 字节数组
            //fileOutputStream.write(str.getBytes());
            /*
            write(byte[] b, int off, int len) 将 len字节从位于偏移量 off的指定字节数组写入此文件输出流
             */
            fileOutputStream.write(str.getBytes(), 0, str.length());

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                fileOutputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
```

#### 3.FileReader 相关方法：

1) new FileReader(File/String)

2)read:每次读取单个字符，返回该字符，如果到文件末尾返回-1

```java
String path="f:\\a.txt";
FileReader fileReader=null;
int data=0;
try {
    fileReader=new FileReader(path);
    //单个字符读取
    while((data = fileReader.read())!=-1){
        System.out.print((char)data);
    }
} catch (IOException e) {
    e.printStackTrace();
}finally {
    fileReader.close();
}
```

3)read(char[])：批量读取多个字符到数组，返回读取到的字符数，如果到文件末尾返回-1

```java
@Test
public void readFile01() throws IOException {
    String path="f:\\a.txt";
    FileReader fileReader=null;
    int readLen=0;
    char[] buf=new char[8];
    try {
        fileReader=new FileReader(path);
        //单个字符读取
        while((readLen = fileReader.read(buf))!=-1){
            System.out.print((new String(buf,0,readLen)));
        }
    } catch (IOException e) {
        e.printStackTrace();
    }finally {
        fileReader.close();
    }
}
```

相关API：

**1) new String(char[]):将char[]转换成String**

**2)new String(char[],off,len):将charll的指定部分转换成String**

#### 4.FileWriter 常用方法

1)new FileWriter(File/String)：覆盖模式，相当于流的指针在首端

2)new FileWriter(File/String,true)：追加模式，相当于流的指针在尾端

3)write(int):写入单个字符

4)write(char[]):写入指定数组

5)write(char[],off,len)：写入指定数组的指定部分

6)write(string）：写入整个字符串

7)write(string,off,len)：写入字符串的指定部分

```java
@Test
    public void fileWriter_01(){
        String path="f:\\hello.txt";
        FileWriter fileWriter=null;
        char[] chars={'a','b','c'};
        try {
            fileWriter=new FileWriter(path);
            //写入单个字符
//            fileWriter.write('z');
            //写入一个字符数组
            fileWriter.write(chars);
            //写入整个字符串
            fileWriter.write("张佳亮".toString(),0,3);
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            //一定要关闭流，才能将数据真正的写进去
            try {
                fileWriter.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        System.out.println("程序结束");
    }
```

**相关API：String类：toCharArray:将String转换成char[]**

注意：

FileWriter使用后，必须要关闭(close)或刷新(flush)，否则写入不到指定的文件!

#### 5.节点流和处理流

- 节点流可以从一个特定的数据源读写数据，如FileReader、FileWriter 

- 处理流(也叫包流)是“连接”在已存在的流（节点流或处理流）之上，为程序提供更为强大的读写功能，也更加灵活，如:BufferedReader、BufferedWriter 

节点流和处理流的区别和联系

- ​	节点流是底层流/低级流，直接跟数据源相接。便的方法来完成输入输出。

- ​	处理流(包装流)包装节点流，既可以消除不同节点流的实现差异，也可以提供更方处理流(也叫包装流）对节点流进行包装，使用了修 饰器设计模式，不会直接与数据源相连


处理流的功能主要体现在以下两个方面：

- ​	性能的提高：主要以增加缓冲的方式来提高输入输出的效率。

- ​	操作的便捷：处理流可能提供了一系列便捷的方法来一次输入输出大批量的数据，使用更加灵活方便


##### **BufferedReader 和 BufferedWrite**

```java
		String filePath = "e:\\a.java";
        //创建bufferedReader
        BufferedReader bufferedReader = new BufferedReader(new FileReader(filePath));
        //读取
        String line; //按行读取, 效率高
        //说明
        //1. bufferedReader.readLine() 是按行读取文件
        //2. 当返回null 时，表示文件读取完毕
        while ((line = bufferedReader.readLine()) != null) {
            System.out.println(line);
        }
        //关闭流, 这里注意，只需要关闭 BufferedReader ，因为底层会自动的去关闭 节点流
        bufferedReader.close();
```

```java
 @Test
    public void testBufferWriter() throws IOException {
        String filePath = "e:\\ok.txt";
        //创建BufferedWriter
        //说明:
        //1. new FileWriter(filePath, true) 表示以追加的方式写入
        //2. new FileWriter(filePath) , 表示以覆盖的方式写入
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(filePath));
        bufferedWriter.write("hello, mss!");
        bufferedWriter.newLine();//插入一个和系统相关的换行
        bufferedWriter.write("hello2, mss!");
        bufferedWriter.newLine();
        bufferedWriter.write("hello3, mss!");
        bufferedWriter.newLine();
        //说明：关闭外层流即可 ， 传入的 new FileWriter(filePath) ,会在底层关闭
        bufferedWriter.close();
    }
```

##### **BufferedInputStream 和 BufferedOutputStream**

```java
//处理流实现文件的拷贝
String srcPath = "f:\\hnt.png";
String destPath = "f:\\hnt01.png";
BufferedInputStream bis= null;
BufferedOutputStream bos = null;
try {
    bis=new BufferedInputStream(new FileInputStream(srcPath));
    bos=new BufferedOutputStream(new FileOutputStream(destPath));
    //循环读取
    byte[] bytes=new  byte[1024];
    int readLen = 0;
    while((readLen=bis.read(bytes))!=-1){
        bos.write(bytes,0,readLen);//边读边取
    }
} catch (FileNotFoundException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
}
```

#### 8.对象流

​	定义：就是能够将基本数据类型或者对象进行序列化和反序列化操作,提供了对基本类型或对象类型的序列化和反序列化的方法 

- ​	ObjectOutputStream ,提供序列化功能 

- ​	ObjectInputStream,提供反序列化功


###### 	序列化和反序列化

- ​		序列化就是在保存数据时，保存数据的值和数据类型

- ​		反序列化就是在恢复数据时，恢复数据的值和数据类型


需要让某个对象支持序列化机制，则必须让其类是可序列化的，为了让某个类是可序列化的，该类必须实现如下两个接口之一：

```java
Serializable 		// 这是一个标记接口, 没有方法
Externalizable 		// 该接口有方法需要实现，因此我们一般实现上面的 Serializable接口
```

###### 序列化操作

```java
	@Test
    //演示ObjectOutputStream的使用, 完成数据的序列化
    public void objectOutput() throws IOException {
        String path="E://add.dat";
        ObjectOutputStream oos=new ObjectOutputStream(new FileOutputStream(path));
        //序列化数据到 e:\data.dat
        oos.writeInt(100);// int -> Integer (实现了 Serializable)
        oos.writeBoolean(true);// boolean -> Boolean (实现了 Serializable)
        oos.writeChar('a');// char -> Character (实现了 Serializable)
        oos.writeDouble(9.5);// double -> Double (实现了 Serializable)
        oos.writeUTF("张三");//String
        //保存一个dog对象
        oos.writeObject(new Dog("旺财", 10, "日本", "白色"));
        oos.close();
        System.out.println("数据保存完毕(序列化形式)");
    }
```

###### 反序列化操作

```java
@Test
    public void objectInputStream() throws IOException, ClassNotFoundException {
        String path="E://add.dat";
        ObjectInputStream ois=new ObjectInputStream(new FileInputStream(path));
        //2.读取，注意顺序
        System.out.println(ois.readInt());
        System.out.println(ois.readBoolean());
        System.out.println(ois.readChar());
        System.out.println(ois.readDouble());
        System.out.println(ois.readUTF());
        System.out.println(ois.readObject());
        System.out.println(ois.readObject());
        System.out.println(ois.readObject());
        //3.关闭
        ois.close();
        System.out.println("以反序列化的方式读取(恢复)ok~");
    }
```

关于序列化与反序列化对象操作

```java
//如果需要序列化某个类的对象，实现 Serializable
public class Dog implements Serializable {
    private String name;
    private int age;
    //序列化对象时，默认将里面所有属性都进行序列化，但除了static或transient修饰的成员
    private static String nation;
    private transient String color;
    //序列化对象时，要求里面属性的类型也需要实现序列化接口
    private Master master = new Master();
    //serialVersionUID 序列化的版本号，可以提高兼容性
    private static final long serialVersionUID = 1L;

    public Dog(String name, int age, String nation, String color) {
        this.name = name;
        this.age = age;
        this.color = color;
        this.nation = nation;
    }
    @Override
    public String toString() {
        return "Dog{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", color='" + color + '\'' +
                '}' + nation + " " +master;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
}
```

###### 注意事项和细节说明

- ​	读写顺序要一致

- ​	要求序列化或反序列化对象，需要 实现 Serializable

- ​	序列化的类中建议添加SerialVersionUID,为了提高版本的兼容性

- ​	序列化对象时，默认将里面所有属性都进行序列化，但除了static或transient修饰的成员

- ​	序列化对象时，要求里面属性的类型也需要实现序列化接口

- ​	序列化具备可继承性，也就是如果某类已经实现了序列化，则它的所有子类也已经默认实现了序列化


#### 9. 标准输入输出流

​	System.in 	  标准输入流    

​	System.out	标准输出流

#### 10.转换流

- ```InputStreamReader:Reader```的子类，可以将```InputStream(字节流)```包装成(转换)```Reader(字符流)```
- ```OutputStreamWriter:Writer```的子类，实现将```OutputStream(字节流)```包装成```Writer(字符流)```

- 处理纯文本数据时，如果使用字符流效率更高，并且可以有效解决中文问题，所以建议将字节流转换成字符流

- 可以在使用时指定编码格式(比如 ```utf-8```,```gbk```,```gb2312,ISO8859-1``` 等)


```java
    @Test
    public void inputStream() throws IOException {
        String path="e://news1.txt";
        //1. 把 FileInputStream 转成 InputStreamReader
        //2. 指定编码 gbk
        InputStreamReader is=new InputStreamReader(new FileInputStream(path),"utf-8");
        //3.把 InputStreamReader 传入 BufferedReader
        BufferedReader br=new BufferedReader(is);
        /**
        将上面的第二部与第三步连起来写
        BufferedReader br=new BufferedReader(new InputStreamReader(new FileInputStream(path),"utf-8"));
        */
        System.out.println(br.readLine());
        //5. 关闭外层流
        br.close();
    }
```

```java
/**
 * 将字节流转换成为字符流，并指定编码方式
 * @param args
 */
  @Test
    public void outputStream() throws IOException {
        String path="e://news1.txt";
        OutputStreamWriter osw=new OutputStreamWriter(new FileOutputStream(path),"utf-8");
        osw.write("今天适合出去玩耍");
        osw.close();
        System.out.println("添加文件成功");
    }
```

#### 11.打印流

打印流只有输出流，没有输入流，在默认情况下，```PrintStream``` 输出数据的位置是标准输出

```java
//字节打印流
PrintStream out = System.out;
out.print("你好");
out.write(12);
//修改打印输出的位置
System.setOut(new PrintStream("f:\\f1.txt"));
out.write(122);
out.close();
```

```java
//字符打印流
PrintWriter printWriter=null;
printWriter = System.setOut("f:\\f1.txt");
printWriter.print("hi, 北京你好~~~~");
printWriter.close();//flush + 关闭流, 才会将数据写入到文件..
```

#### 12.Properties 

专门用于读写配置文件的集合类

```java
//配置文件的格式：
键=值
```

注意：键值对不需要有空格~值不需要用引号一起来。默认类型是String

Properties的常见方法

- ​	load:加载配置文件的键值对到Properties对象

- ​	list:将数据显示到指定设备

- ​	getProperty(key)：根据键获取值

- ​	setProperty(key,value)：设置键值对到Properties对象

- ​	store:将Properties中的键值对存储到配置文件，在idea 中，保存信息到配置文件，如果含有中文，会存储为unicode码


http://tool.chinaz.com/tools/unicode.aspx unicode码查询工具

```java
	//1. 创建 Properties 对象
   Properties properties = new Properties();
	//2. 加载指定配置文件
   properties.load(new FileReader("F:\\workstudy\\hspJavaSe\\chapter0\\src\\mysql.properties"));
	//3. 把 k-v 显示控制台
   properties.list(System.out);
	//4. 根据 key 获取对应的值
   String user = properties.getProperty("user");
   String pwd = properties.getProperty("pwd");
   System.out.println("用户名=" + user);
   System.out.println("密码是=" + pwd);
```

```java
	//使用 Properties 类来创建 配置文件, 修改配置文件
	Properties properties = new Properties()；
	properties.setProperty("charset", "utf8"); 
	properties.setProperty("user", "汤姆");//注意保存时，是中文的 unicode码值
	properties.setProperty("pwd", "888888"); //将 k-v 存储文件中即可 
	properties.store(new FileOutputStream("src\\mysql2.properties"), null);
 	System.out.println("保存配置文件成功~");
```

## 15.网络编程

### 1.网络的相关概念

**网络通信**

​	1.概念：两台设备之间通过网络实现数据传输

​	2.网络通信：将数据通过网络从一台设备传输到另一台设备

​	3.java.net包下提供了一系列的类或接口，供程序员使用，完成网络通信

**网络**

​	概念：两台或多台设备通过一定物理设备连接起来构成了网络

​	根据网络的覆盖范围不同，对网络有以下的分类

- 局域网：覆盖范围最小，仅仅覆盖一个教室或一个机房

- 城域网：覆盖范围较大，可以覆盖一个城市

- 广域网：覆盖范围最大，可以覆盖全国，甚至全球，万维网是广域网的代表

**域名和端口号**

​	将ip地址映射成ip地址，为了方便记忆，解决记ip的困难

端口号

​	概念：用于标识计算机上某个特定的网络程序

​	表示形式：以整数形式，端口范围0~65535[2个字节表示端口0~2^16-1]

​	0~1024已经被占用，比如ssh 22,ftp 21, smtp 25 http 80

​	常见的网络程序端口号:

​		tomcat :8080	mysql:3306 		oracle:1521 		sqlserver:1433 

  网络通信协议（七层协议）

​		应用层、表示层、会话层、传输层、网络层、数据链路层、物理层

**tcp和udp**

TCP协议： 传输控制协议

​		1.使用TCP协议前，须先建立TCP连接，形成传输数据通道

​		2.传输前，采用“三次握手“方式，是可靠的

​		3.TCP协议进行通信的两个应用进程：客户端、服务端

​		4.在连接中可进行大数据量的传输

​		5.传输完毕，需释放已建立的连接，效率低

UDP协议：用户数据协议

​		1.将数据、源、目的封装成数据包，不需要建立连接

​		2.每个数据报的大小限制在64K内，不适合传输大量数据

​		3.因无需连接，故是不可靠的

​		4.发送数据结束时无需释放资源（因为不是面向连接的），速度快

​		5.举例：厕所通知：发短信

### 2.InetAddress 

相关方法

- 获取本机InetAddress对象getlocalHost

- 根据指定主机名/域名获取ip地址对象getByName 

- 获取InetAddress对象的主机名 getHostName 

- 获取InetAddress对象的地址getHostAddress

```java
InetAddress localhost=InetAddress.getLocalHost();
System.out.println(localhost);
InetAddress host3=InetAddress.getByName("www.hsp.com");
System.out.println(host3);
//获取InetAddress对象的主机名getHostName
String host3Name=host3.getHostName();
System.out.println(host3Name);
//获取InetAddress对象的地址getHostAddress
String host3Address=host3.getHostAddress();
System.out.println(host3Address);
```

### 3.Socket

​	1.套接字(Socket)开发网络应用程序被广泛采用，以至于成为事实上的标准。

​	2.通信的两端都要有Socket，是两台机器间通信的端点

​	3.网络通信其实就是Socket间的通信。

​	4.Socket允许程序把网络连接当成一个流，数据在两个Socket间通过IO传输。

​	5.一般主动发起通信的应用程序属客户端，等待通信请求的为服务端

​	6、TCP网络通信编程

​		1.基于客户端—服务端的网络通信

​		2.底层使用的是TCP/IP协议

​		3.应用场景举例：客户端发送数据，服务端接受并显示控制台

 案例一:实现客户端与服务器段通信

服务器端

```java
SocketTCP01Server
    public static void main(String[] args) throws IOException {
             //1. 在本机 的9999端口监听, 等待连接
     		// 细节: 要求在本机没有其它服务在监听9999
     		//细节：这个 ServerSocket 可以通过 accept() 返回多个Socket[多个客户端连接服务器的并发]
        ServerSocket serverSocket=new ServerSocket(9998);
        System.out.println("服务器段在9999端口等待监听");
        //如果一直没人响应就一直阻塞
        Socket socket = serverSocket.accept();
        System.out.println("服务端 socket =" + socket.getClass());
        //3. 通过socket.getInputStream() 读取客户端写入到数据通道的数据, 显示
        InputStream is=socket.getInputStream();
        //读取
        byte[] buf=new byte[8];
        int readLen=0;
        while ((readLen=is.read(buf))!=-1){
            System.out.println(new String(buf, 0, readLen));//根据读取到的实际长度，显示内容.
        }
        //5.关闭流和socket
        is.close();
        socket.close();
        serverSocket.close();
    }
```

客户端

```java
Clint01Server
	public static void main(String[] args) throws IOException {
        //1. 连接服务端 (ip , 端口）
        //解读: 连接本机的 9999端口, 如果连接成功，返回Socket对象
        Socket socket=new Socket(InetAddress.getLocalHost(),9998);
        System.out.println("服务端收到"+socket.getClass());
        //2. 连接上后，生成Socket, 通过socket.getOutputStream()得到和socket对象关联的输出流对象
        OutputStream os=socket.getOutputStream();
        os.write("hello,server".getBytes());
        os.close();
        socket.close();
        System.out.println("客户端退出.....");
    }
```

案例二:实现客户端与服务器端的通信，并且服务器端在收到客户端的消息后回复给客户端

服务器端

```java
public static void main(String[] args) throws IOException {
    ServerSocket serverSocket=new ServerSocket(9999);
    System.out.println("服务器端在等着客户端的回应");
    Socket socket = serverSocket.accept();
    System.out.println("客户端回应了消息");
    //3. 通过socket.getInputStream() 读取客户端写入到数据通道的数据, 显示
    InputStream inputStream = socket.getInputStream();
    //4. IO 读取
    byte[] buf = new byte[1024];
    int readLen = 0;
    while ((readLen = inputStream.read(buf)) !=-1) {
        System.out.println(new String(buf, 0, readLen));//根据读取到的实际长度，显示内容
    }
    //向客户端回应消息
    OutputStream os=socket.getOutputStream();
    os.write("hello client".getBytes());
    //设置结束标记
    socket.shutdownOutput();
    //5.关闭流和socket
    os.close();
    inputStream.close();
    socket.close();
    serverSocket.close();//关闭
    System.out.println("服务器退出了");
}
```

客户端

```java
public static void main(String[] args) throws IOException {
    Socket socket=new Socket(InetAddress.getLocalHost(),9999);
    System.out.println("准备向服务器端写消息");
    OutputStream outputStream = socket.getOutputStream();
    outputStream.write("hello,server".getBytes());
    //设置结束标记
    socket.shutdownOutput();
    //接收服务器端发过来的消息
    InputStream inputStream= socket.getInputStream();
    byte[] buf = new byte[1024];
    int readLen = 0;
    while ((readLen = inputStream.read(buf)) !=-1) {
        System.out.println(new String(buf, 0, readLen));//根据读取到的实际长度，显示内容
    }
    inputStream.close();
    outputStream.close();
    socket.close();
    System.out.println("客户端退出了");
}
```

### 4.netstat 指令

```cmd
netstat -an #查看所有网络连接信息
netstat -an|more #分页查看
```

### 5.TCP通讯

<img src="D:\ruanjian\Typora\image-20231029124051659.png" alt="image-20231029124051659" style="zoom:80%;" />

当客户端连接到服务器之后，实际上客户端也是通过端口和服务器通讯的

### 6.UDP 网络通信编程

**基本介绍**

1.类DatagramSocket 和DatagramPacket[数据包/数据报]实现了基于UDP协议网络程序。

2.UDP数据报通过数据报套接字DatagramSocket 发送和接收，系统不保证UDP数据报一定能够安全送到目的地，也不能确定什么时候可以抵达。

3.DatagramPacket 对象封装了UDP数据报，在数据报中包含了发送端的IP地址和端口号以及接收端的IP地址和端口号。

4.UDP协议中每个数据报都给出了完整的地址信息，因此无须建立发送方和接收方的连接

**基本流程**

1.核心的两个类/对象 DatagramSocket与DatagramPacket

2.建立发送端，接收端（没有服务端和客户端概念)

3.发送数据前，建立数据包/报 DatagramPacket对象

4.调用DatagramSocket的发送、接收方法

5.关闭DatagramSocket

```java
//UDP接收端
 public static void main(String[] args) throws IOException {
        //1. 创建一个 DatagramSocket 对象，准备在9999接收数据
        DatagramSocket socket = new DatagramSocket(9999);
        //2. 构建一个 DatagramPacket 对象，准备接收数据
        //   在前面讲解UDP 协议时，老师说过一个数据包最大 64k
        byte[] buf = new byte[1024];
        DatagramPacket packet = new DatagramPacket(buf, buf.length);
        //3. 调用 接收方法, 将通过网络传输的 DatagramPacket 对象
        //   填充到 packet对象
        //当有数据包发送到 本机的9999端口时，就会接收到数据
        //   如果没有数据包发送到 本机的9999端口, 就会阻塞等待.
        System.out.println("接收端A 等待接收数据..");
        socket.receive(packet);
        //4. 可以把packet 进行拆包，取出数据，并显示.
        int length = packet.getLength();//实际接收到的数据字节长度
        byte[] data = packet.getData();//接收到数据
        String s = new String(data, 0, length);
        System.out.println(s);
        //===回复信息给B端
        //将需要发送的数据，封装到 DatagramPacket对象
        data = "好的, 明天见".getBytes();
        //说明: 封装的 DatagramPacket对象 data 内容字节数组 , data.length , 主机(IP) , 端口
        packet =new DatagramPacket(data, data.length, InetAddress.getByName("192.168.12.1"), 9998);
        socket.send(packet);//发送
        //5. 关闭资源
        socket.close();
        System.out.println("A端退出...");
    }
```

```java
//发送端B ====> 也可以接收数据
public static void main(String[] args) throws IOException {
        //1.创建 DatagramSocket 对象，准备在9998端口 接收数据
        DatagramSocket socket = new DatagramSocket(9998);
        //2. 将需要发送的数据，封装到 DatagramPacket对象
        byte[] data = "hello 明天吃火锅~".getBytes(); 
        //说明: 封装的 DatagramPacket对象 data 内容字节数组 , data.length , 主机(IP) , 端口
        DatagramPacket packet =new DatagramPacket(data, data.length, InetAddress.getLocalHost(), 9999);
        socket.send(packet);
        //3.=== 接收从A端回复的信息
        //(1)   构建一个 DatagramPacket 对象，准备接收数据
        //   在前面讲解UDP 协议时，老师说过一个数据包最大 64k
        byte[] buf = new byte[1024];
        packet = new DatagramPacket(buf, buf.length);
        //(2)    调用 接收方法, 将通过网络传输的 DatagramPacket 对象
        //   填充到 packet对象
        //老师提示: 当有数据包发送到 本机的9998端口时，就会接收到数据
        //   如果没有数据包发送到 本机的9998端口, 就会阻塞等待.
        socket.receive(packet);
        //(3)  可以把packet 进行拆包，取出数据，并显示.
        int length = packet.getLength();//实际接收到的数据字节长度
        data = packet.getData();//接收到数据
        String s = new String(data, 0, length);
        System.out.println(s);
        //关闭资源
        socket.close();
        System.out.println("B端退出");
    }
```