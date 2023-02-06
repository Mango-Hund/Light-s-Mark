# 接口、lambda表达式与内部类
## 接口
在java程序设计语言中，接口不是类，而是对类的一组需求描述，这些类要遵从接口描述的统一风格进行定义。

有些接口可以包含一个到多个简单方法，但是不能引用实例域--接口没有实例。提供实例域和方法实现的任务应该由实现接口的那个类来完成。因此可以将接口看成是没有实例域的抽象类。但这两个概念还是有区别的。

为了让类实现一个接口，通常需要
1. 将类声明为实现给定的接口
2. 对接口中的所有方法进行定义  

要将类声明为实现某个接口，需要使用关键字`implements`：
```java
class Employee implements Comparable
```
举例
```java
public class EmployeeSortTest {
	public static void main(String[] args)
	{
		Employee[] staff = new Employee[3];
		
		staff[0] = new Employee("Harry Hacker",35000);
		staff[1] = new Employee("Carl Cracker",75000);
		staff[2] = new Employee("Tony Tester",38000);
		
		Arrays.sort(staff);
		
		//print out information about all Employee objects
		for (Employee e: staff)
			System.out.println("name=" + e.getName() + ",salary=" + e.getSalary());
	}
}
```

```java
public class Employee implements Comparable<Employee>
{
	private String name;
	private double salary;
	
	public Employee(String name, double salary)
	{
		this.name = name;
		this.salary = salary;
	}
	
	public String getName()
	{
		return name;
	}
	
	public double getSalary()
	{
		return salary;
		
	}
	
	public void raiseSalary(double byPercent)
	{
		double raise = salary * byPercent/100;
		salary += raise;
	}
	
	/**
	 * Compares employees by salary
	 * @param other another Employee object
	 * @return a negative value if this employee has a lower salary than other Object
	 */
	public int compareTo(Employee other)
	{
		return Double.compare(salary, other.salary);
	}
}
```
* 接口不是类，不能使用new运算符实例化一个接口：
```java
x = new Comparable(...);//Error
```
* 然而，尽管不能构造接口的对象，却能声明接口的变量：`Comparable x`    
* 接口变量必须引用实现了接口的类对象
```java
x = new Employee(...);//OK provided Employee implements Comparable
```
* 如同使用 `instanceof`检查一个对象是否属于某个特定类一样， 也可以使用 `instance` 检查一个对象是否实现了某个特定的接口：
```java
if(anObject instanceof Comparable){...}
```
* 与可以建立类的继承关系一样，接口也可以被扩展。这里允许存在多条从具有较高通用性的接口到较高专用性的接口的链。
```java
//假设有一个Moveable的接口
public interface Moveable
{
	void move(double x, double y);
}
//然后，可以以它为基础扩展一个叫做Powered的接口：
public interface Powered extends Moveable
{
    double milesPerGallen();
}
//虽然在接口中不能包含实例域或静态方法，但是却可以包含常量
public interface Powered extends Moveable
{
    double milesPerGallon();
	double SPEED_LIMIT = 95; //a public static final constant
}
```
接口中的域将被自动设为`public static final`   

但每个类可以实现多个接口，但是不能多继承，因此开发者提出接口的概念而不是直接使用抽象类。  

默认方法，如果先在一个接口中将一个方法定义为默认方法，然后又在超类或另一个接口中定义了同样的方法：
1. 超类优先。如果超类提供了一个具体方法，同名而且有相同参数类型的默认方法会被忽略。
2. 接口冲突。 如果一个超接口提供了一个默认方法，另一个接口提供了一个同名而且 参数类型（不论是否是默认参数）相同的方法， 必须覆盖这个方法来解决冲突。  

**回调(callback)** 是一种常见的程序设计模式。在这种模式中，可以指出某个特定事件发生时应该采取的动作。  

在java.swing包中有一个Timer 类，可以使用它在到达给定的时间间隔时发出通告。例如，假如程序中有一个时钟，就可以请求每秒钟获得一个通告，以便更新时钟的表盘。在构造定时器时，需要设置一个时间间隔，并告知定时器，当达到时间间隔的时候调用传入对象的方法。   

```java
package interfaces;
import java.awt.*;
import java.awt.event.*;
import java.util.*;
import javax.swing.*;
import javax.swing.Timer;
//to resolve conflict with java.util.Timer

public class TimerTest {
	public static void main(String[] args)
	{
		ActionListener listener = new TimePrinter();
		
		//construct a timer that calls the listener
		//once every 10 seconds
		Timer t = new Timer(10000,listener);
		t.start();
		JOptionPane.showMessageDialog(null, "Quit program?");
		System.exit(0);
	}
}

class TimePrinter implements ActionListener
{
	public void actionPerformed(ActionEvent event)
	{
		System.out.println("At the tone, the time is " + new Date());
		Toolkit.getDefaultToolkit().beep();
	}
}
```  

对于每一个类，需要确定：
1. 默认的clone方法是否满足要求；
2. 是否可以在可变的子对象上调用clone来修补默认的clone方法
3. 是否不该使用clone   
实际上第3个选项是默认选项。如果选择第1项或者第2项，类必须：
1. 实现Cloneable接口；
2. 重新定义clone方法，并指定public访问修饰符。   

## Lambda表达式
lambda表达式是一个可传递的代码块，可以在以后执行一次或多次。   
lambda的一个表达式形式为：参数，箭头(->)以及一个表达式。
```java
(String first, String second)->
{
	if (first.length()<second.length()) return -1;
	else if (first.length()>second.length()) reutrn 1;
	else return 0;
}
```
即使lamda表达式没有参数，仍然要提供空括号，就像无参数方法一样：
```java
()->{for (int i=100; i>=0; i--) System.out.println(i);}
```
如果可以推导出一个lambda表达式的参数类型，则可以忽略其类型。
```java
Comparator<String> comp
= (first,second)//Same as (String first,String second)
->first.length()-second.lengrh();
```
在这里， 编译器可以推导出 first 和 second 必然是字符串，因为这个 lambda 表达式将赋 给一个字符串比较器。    
如果方法只有一个参数，而且这个参数的类型可以推导得出，那么甚至还可以省略小括号：
```java
ActionListener listener = event -> 
	System.out.println("The time is " + new Date());
```
如果一个 lambda 表达式只在某些分支返回一个值， 而在另外一些分支不返回值，这是不合法的。例如，`(int x)-> { if(x >= 0) return 1;}`就不合法。  
举例：  
```java
package lambda;
import java.util.*;
import javax.swing.*;
import javax.swing.Timer;
/**
 * 
 * @author 23974
 *@version 1.0 2021-05-26
 */

public class LambdaTest {
	public static void main(String[] args)
	{
		String[] planets = new String[] {"Mercury","Venus","Earth","Mars","Jupiter","Saturn","Uranus","Neptune"};
		System.out.println(Arrays.toString(planets));
		System.out.println("Sorted in dictionary order:");
		Arrays.sort(planets);
		System.out.println(Arrays.toString(planets));
		System.out.println("Sorted by length: ");
		Arrays.sort(planets,(first,second)->first.length()-second.length());
		System.out.println(Arrays.toString(planets));
		
		Timer t = new Timer(1000,event -> 
		System.out.println("The time is " + new Date()));
		t.start();
		
		//keep program running until user select "OK"
		JOptionPane.showMessageDialog(null,"Quit program");
		System.exit(0);
	}
}
```
对于只有一个抽象方法的接口，需要这种接口的对象时，就可以提供一个lambda表达式。这种接口称为函数式接口（functional interface）。

* 方法引用   
用::操作符分割方法名与对象或类名。主要有3种情况：
1. object::instanceMethod
2. Class::staticMethod
3. Class::instanceMethod
在前两种情况中，方法引用等价于提供方法参数的lambda表达式。例如`System.out::println`等价于`x->System.out.println(x)`;`Math::pow`等价于`(x,y)->Math.pow(x,y)`。  
对于第3种情况，第一个参数会成为方法的目标。例如`String::compareToIgnoreCase`等同于`(x,y)->x.compareToIgnoreCase(y)`。     
可以在方法引用中使用 this 参数。例如，`this::equals` 等同于`x -> this.equals(x)`。使用 super 也是合法的。  

* 构造器引用
构造器引用与方法引用很类似，只不过方法名为`new`。例如，`Person::new` 是 `Person` 构造器的一个引用。    
可以用数组类型建立构造器引用。例如，`int[]::new`是一个构造器引用，它有一个参数，即数组的长度。这等价于lambda表达式`x->new int[x]`。   

Java有一个限制，无法构造泛型类型T的数组。数组构造器引用对于克服这个限制很有用。表达式`new T[n]`会产生错误，因为这会改为`new Object[n]`  , 假设我们需要一个Person对象数组，Stream接口有一个toArray方法可以返回Object数组：
```java
Object[] people = stream.toArray();
```
用户最终想得到的是Person引用数组
```java
Person[] people = stream.toArray(Person[]::new);
```
toArray方法调用这个构造器来得到一个正确类型的数组。然后填充这个数组并返回。   

Comparator接口包含很多静态方法可供lambda表达式引用








