## java修饰词

---

#### 1.volatile介绍


volatile是java最轻量级的同步机制。

**特性：**

*	可见性。变量读写直接操作主存而不是CPU Cache。当一个线程修改了volatile修饰的变量后，无论是否加锁，其它线程都可以立即看到最新的修改。
*	禁止指令重排序优化。
*	保证变量可见性，但无法保证原子性。也就是说非线程安全

##### java内存模型：

![image](../basic-knowledge/img/16.png)

[深入分析volatile的实现原理](https://mp.weixin.qq.com/s/mcR8_FHHGA2zb0aW1N02ag?from=groupmessage&isappinstalled=0)

#### 2.synchronized介绍

线程安全，锁区域内容一次只允许一个线程执行，通过锁机制控制。

```
     一、当两个并发线程访问同一个对象object中的这个synchronized(this)同步代码块时，一个时间内只能有一个线程得到执行。另一个线程必须等待当前线程执行完这个代码块以后才能执行该代码块。

     二、然而，当一个线程访问object的一个synchronized(this)同步代码块时，另一个线程仍然可以访问该object中的非synchronized(this)同步代码块。

     三、尤其关键的是，当一个线程访问object的一个synchronized(this)同步代码块时，其他线程对object中所有其它synchronized(this)同步代码块的访问将被阻塞。
```


* 同步方法

```
public synchronized void method(int i);
```

每个类实例对应一把锁，类的两个实例没有这个限制。类实例中所有的synchronized方法共用这一把锁，锁的范围有点大。


* 同步块

相比上面的同步方法，锁的范围可缩小。


```
synchronized(syncObject) {
//允许访问控制的代码
}
```

其中的代码执行前必须获得对象 syncObject 锁，可以针对任意代码块，且可任意指定上锁的对象，故灵活性较高。

#### 3.final介绍

如果修饰变量标识为常量，运行过程中会将值直接替换到变量这个占位符中（避免根据内存地址再次查找这层消耗）；如果修改方法，方法不允许被覆盖；修饰类，类不允许被继承。

基础类型，如String，不允许修改。

集合，如Map、List，引用地址不允许改，但可以put、get等操作。

java8编译会检查，如果是修改常量，会编译失败。

![image](../basic-knowledge/img/Snip20160626_24.png)


#### 4.static

* 声明属性

	为全局属性，放在全局数据区，只分配一次
* 声明方法

	类方法，可以由类名称直接调用

* 声明类

#### 5.transient

如果一个对象中的某个属性不希望被序列化，则可以使用transient关键字进行声明。

## 6.final 关键字

**final关键字主要用在三个地方：变量、方法、类。**

1. **对于一个final变量，如果是基本数据类型的变量，则其数值一旦在初始化之后便不能更改；如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象。**

2. **当用final修饰一个类时，表明这个类不能被继承。final类中的所有成员方法都会被隐式地指定为final方法。**

3. 使用final方法的原因有两个。第一个原因是把方法锁定，以防任何继承类修改它的含义；第二个原因是效率。在早期的Java实现版本中，会将final方法转为内嵌调用。但是如果方法过于庞大，可能看不到内嵌调用带来的任何性能提升（现在的Java版本已经不需要使用final方法进行这些优化了）。类中所有的private方法都隐式地指定为final。

## 7.static 关键字

**static 关键字主要有以下四种使用场景：**

1. **修饰成员变量和成员方法:** 被 static 修饰的成员属于类，不属于单个这个类的某个对象，被类中所有对象共享，可以并且建议通过类名调用。被static 声明的成员变量属于静态成员变量，静态变量 存放在 Java 内存区域的方法区。调用格式：`类名.静态变量名`    `类名.静态方法名()`
2. **静态代码块:** 静态代码块定义在类中方法外, 静态代码块在非静态代码块之前执行(静态代码块—>非静态代码块—>构造方法)。 该类不管创建多少对象，静态代码块只执行一次.
3. **静态内部类（static修饰类的话只能修饰内部类）：** 静态内部类与非静态内部类之间存在一个最大的区别: 非静态内部类在编译完成之后会隐含地保存着一个引用，该引用是指向创建它的外围类，但是静态内部类却没有。没有这个引用就意味着：1. 它的创建是不需要依赖外围类的创建。2. 它不能使用任何外围类的非static成员变量和方法。
4. **静态导包(用来导入类中的静态资源，1.5之后的新特性):** 格式为：`import static` 这两个关键字连用可以指定导入某个类中的指定静态资源，并且不需要使用类名调用类中静态成员，可以直接使用类中静态成员变量和成员方法。

## 8.this 关键字

this关键字用于引用类的当前实例。 例如：

```java
class Manager {
    Employees[] employees;

    void manageEmployees() {
        int totalEmp = this.employees.length;
        System.out.println("Total employees: " + totalEmp);
        this.report();
    }

    void report() { }
}
```

在上面的示例中，this关键字用于两个地方：

- this.employees.length：访问类Manager的当前实例的变量。
- this.report（）：调用类Manager的当前实例的方法。

此关键字是可选的，这意味着如果上面的示例在不使用此关键字的情况下表现相同。 但是，使用此关键字可能会使代码更易读或易懂。



## 9.super 关键字

super关键字用于从子类访问父类的变量和方法。 例如：

```java
public class Super {
    protected int number;

    protected showNumber() {
        System.out.println("number = " + number);
    }
}

public class Sub extends Super {
    void bar() {
        super.number = 10;
        super.showNumber();
    }
}
```

在上面的例子中，Sub 类访问父类成员变量 number 并调用其其父类 Super 的 `showNumber（）` 方法。

**使用 this 和 super 要注意的问题：**

- super 调用父类中的其他构造方法时，调用时要放在构造方法的首行！this 调用本类中的其他构造方法时，也要放在首行。
-  this、super不能用在static方法中。

**简单解释一下：**

被 static 修饰的成员属于类，不属于单个这个类的某个对象，被类中所有对象共享。而 this 代表对本类对象的引用，指向本类对象；而 super 代表对父类对象的引用，指向父类对象；所以， **this和super是属于对象范畴的东西，而静态方法是属于类范畴的东西**。



## 参考

- https://www.codejava.net/java-core/the-java-language/java-keywords
- https://blog.csdn.net/u013393958/article/details/79881037
