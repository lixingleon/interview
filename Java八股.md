# 成员变量、局部变量

1. 默认值
   1. 成员变量有默认值

# 单例模式

1. DCL doublely checked locking

   ```java
   public Singleton{
   	private static volatile Singleton singleton;
   	private Singleton(){
   	
   	}
   	public static getInstance(){
   		if(singleton == null){
               synchronized(Singleton.class){
                   if(singleton == null){
                       singleton = new Singleton();
                   }
               }
           }
           return singleton;
   	}
   }
   在java 5之前， volatile 并不能避免指令重排问题
   ```

2. 静态内部类

   ```java
   public Singleton{
   	private static class SingletonHolder{
   		static final Singleton singleton = new Singleton();
   	}
   	private Singleton(){
   	
   	}
   	public static Singleton getInstance(){
   		return SingletonHolder.singleton;
   	}
   }
   静态内部类不能接受传参
   ```

   

# 封装、继承、多态

继承：

1. 子类继承父类。得到父类所有变量和方法。但无法获取访问修饰符规定外的属性和方法
2. 子类可以有自己的属性和方法
3. 子类可以重写父类方法

封装：把一些操作细节隐藏起来，只提供一个接口给外部使用，告知外部此部分代码是做什么的，但外部不知道具体细节。方法就是对实现细节的封装。类就是对数据和数据操作的封装。

多态：父类引用指向不同的子类对象时，如果子类重写了父类方法，调用同一个方法就会产生不同的效果。

# 重载与重写

重载：同一个类，方法名相同，参数个数，类型，顺序有一个不同就是重载

重写：子类的方法名和参数与父类一样。访问范围要大于等于父类，抛异常等级要小于等于父类。



# 访问修饰符

| 修饰符    | 等级               |
| --------- | ------------------ |
| public    | 所有类可见         |
| private   | 只有本类可见       |
| protected | 本包和所有子类可见 |
| default   | 只有本包可见       |

![image-20211103174843086](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20211103174843086.png)



# 关键字

### final关键字

where: 

可以修饰成员变量，局部变量，方法，类。（字符串类和包装类都是final的）

how: 

1. 一般修饰变量表示常量，和static搭配。final变量的值不能改变，但引用的内容还可以改变。
2. final变量使用前一定要赋值。
3. final方法不能被重写，final类不能被继承。

why：

1. 确保final方法在子类中不会改变语义。
2. ~~内联机制加快final方法执行~~（现代jvm已经足够优化）

### static关键字

where(主要用在哪)：static可以用来修饰成员变量，方法，代码块和类。

how(怎么用的)：

1. 静态的成员变量，方法和代码块都在类被加载时被加载，且只会加载一次。静态变量和方法是属于类的，独立于对象，可以直接用类来调用
2. static只能修饰内部类，修饰后可当作普通类使用，而不需要实例化一个外部类。static内部类不能使用外部类的非静态属性和方法

2. 静态方法不可以访问非静态的方法和成员变量

why(好处是什么)：

1. 方便在没有创建对象的情况下调用方法和变量
2. 每个对象都要用到的变量或方法，可以声明成static的，避免每次创建对象都加载一次。

### this关键字

类的当前实例的引用。

### super关键字

用于访问父类的变量和方法

# 集合/容器

### Collection

![image-20211105130045830](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20211105130045830.png)

### Map

![image-20211105130129870](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20211105130129870.png)

3. List，Set，Map的区别

List：元素有序且可以重复

Set：元素无序且不可重复

Queue：元素有序且可以重复，只能从两头获取元素

Map：key无序且不可重复，value无序且可重复

4. 为什么要使用集合

数组相对于集合：数组的特点是元素可重复，有序。集合可以更灵活地存储数据。比如Set就是不可重复的，Queue就只能从头尾取数据。Map可以存储键值对

5. ArrayList和LinkedList的区别

都不保证线程安全？

ArrayList

底层用Object数组存储，支持快速随机访问O(1)，在list结尾会预留一定的空间。

增删元素复杂度：

1. 在list开头：O(n)
2. 在List中间：O(n)
3. 在List结尾: amortized O(1)

LinkedList

底层用双向链表存储，不支持随机访问。每个元素都需要多存储前后指针。

增删元素复杂度：

1. 在list开头：O(1)，
2. 在list中间：O(n)
3. 在list结尾： O(1);



6. ArrayList扩容机制

   默认初始大小是10

   Arrays.copyOf，扩容为原来容量的1.5倍。原理是创建一个新数组，把旧数组复制过去。copyOf底层调用System.arraycopy (Object src, int srcPos, Object dest, int destPos, int length)

   缩容？ 没有缩容

### HashMap

7. HashMap

   1. 简介：默认数组大小16，每次扩容乘以2。loadFactor默认0.75 当实际元素数量大于容量乘以loadFactor，就要扩容。

   2. 底层数据结构：

      1. JDK1.8以前是数组加链表
      2. JDK1.8及以后，链表长度大于8且数组长度大于64，就把链表转化为红黑树

   3. 常用方法：

      ```java
      HashMap<String ,String> map = new HashMap<>();
      //1. 获取所有的key
      Set<String> keys = map.keySet();
      //2. 获取所有的values
      Collection<String> values = map.values();
      //3. 获取键值对
      Set<java.util.Map.Entry<String,String>> set = map.entrySet();
      
      ```

   4. 如何解决哈希冲突？
      1. 链表加数组的形式
   5. put方法怎么实现的？
      1. put的过程：
         1. 如果hashmap内部的Node数组还没初始化，就会先初始化。数组默认大小是16。然后通过hash方法得到扰动后的hashcode，再通过和数组长度-1 做and^操作，得到数组中的index。
         2. 如果index中没有值，直接插入。插入后要考虑扩容问题
         3. 如果有值，且key相等。则直接更新value
         4. 如果key不等，则判断这个位置存储的是不是TreeNode。如果是的话直接插入。
         5. 如果不是，则遍历链表，遇到key相同的就覆盖。如果没遇到相同的，就加在链表最后。如果链表长度超过8，且数组大小大于64，则将链表变成红黑树，否则就只是扩容数组。
   6. 如何获取索引的？
      1. 用hash函数将key对象的hashcode经过哈希扰动得到新的哈希值
      2. 再将这个值和（length-1）做and位运算得到索引
   7. 为什么数组容量都是2的次幂？
      1. 由6.2我们知道，若length是2的次幂，那length-1的低位就都是1。举例：16 = 10000， 15 = 01111， 32=100000， 31= 011111
      2. 则扩容以后，length-1只会在高位多一个1，只要key对象的哈希值在那个1位置上是0，那扩容后index会保持不变。这样就大大减少了老数组中位置的重新调换。
      3. 再一个。低位都是1会让获得的数据索引更均匀。如果低位是0，那key对象的哈希值在这个位置不管是0还是1，得到的索引都是一样的，这样提高了哈希碰撞的可能性。
      4. ![image-20211105163616609](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20211105163616609.png)
      5. ![image-20211105163700021](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20211105163700021.png)
   8. resize过程
      1. 如果数组大小已经到达最大：2^30，则不再扩容。
      2. 创建一个大小为两倍的新数组，遍历每个位置的每个节点，通过新的数组size计算新的index，把旧数据拷贝到新数据上去

### ConcurrentHashMap

1. HashTable给全局上锁，效率太低。
2. ConcurrentHashMap使用分段锁。维护一个segment数组（类似一个哈希表数组）。一个segment数组维护一个HashEntry数组（类似一个Node对象）

# 泛型

### 1. 解释什么是泛型

泛型其实就是把对象类型参数化。目的是让一份代码适用多种对象。

泛型传入的应该是对象。基本数据类型不能当作泛型。

##### 1. 泛型类：

声明方式：在类名后面，添加一个或多个泛型，用尖括号括起来。

实现方式： 在实例化泛型类时，传入数据类型参数。

##### 2. 泛型方法：

声明方式：修饰符后面，返回值之前，普通类和泛型类中都可以有泛型方法

修饰符  <T, R..> 返回类型 方法名( 参数列表){

}

实现方式：调用泛型方法时，传入数据类型参数（在函数名称前的尖括号里传入数据类型）

类型推断：

1. 当不传入类型参数时，默认类型是所有形参的最小公共父类
2. 当传入类型参数时，类型只能是该类或它的子类

##### 3. 泛型接口：

1. 实现泛型接口，不指定类型
2. 实现泛型接口，指定类型

##### 4. 泛型擦除

​	编译器会把所有的泛型信息都擦除，用原始类型来代替。

编译器在编译的时候，会自动添加强制转换，将原始类型再转换成参数类型，如：

![image-20211101160026667](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20211101160026667.png)

##### 泛型擦除造成的问题和解决方案：

泛型擦除导致多态失效：子类重写父类方法。但由于T都被替换成Object，实际效果是子类重载父类方法。

![image-20211122171740825](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20211122171740825.png)

![image-20211122171819995](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20211122171819995.png)

解决方案：桥方法： 在子类创建桥方法，内部调用子类重写的方法。这样当调用父类方法时，会调用桥方法，桥方法在内部调用子类重写的方法。

##### 5. 泛型约束

1. 泛型类型不能是基础数据类型 如：(byte,short,int,long,float,double,char,boolean）

   因为泛型擦除后，T都变成了Object，Object不能存储基本数据类型

2. 不能getClass获取类型，不能instanceof判断类型。

   因为泛型擦除，所有类型都一样了。比如ArrayList<String>和ArrayList<Integer> 编译后都变成了 ArrayList<Object>

3. 不能实例化T。

   因为编译器根本不知道T是什么。can not find symbol。

4. 不能用static修饰T。

   因为泛型需要创建对象时侯传入，而static在创建对象之前就被执行了。

5. 不能创建泛型类型的数组。如ArrayList<String>[] arrayOfLists = new ArrayList<String>[2];//不能编译。

   因为泛型擦除后，ArrayList<String>[] arrayOfLists变成ArrayList[]，可以向上转型成Object[]。

   arrayOfLists[0] = new ArrayList<String>();

   arrayOfLists[1] = new ArrayList<Integer>();

   本来数组类型检测会抛ArrayStoreException，但擦拭机制会使检测无效。进而使上面的情况能通过数组检查。



##### 6.泛型限定

List<T extends Number> 限定list只能传入Number或Number的子类对象。

##### 7.通配符

1. 上界通配符：List<? extends Number>可以实现泛型的向上转型。但只能读不能写。因为编译器只能确定list里存放的是Number的子类，但不确定是哪个子类。
2. 下界通配符：List<? super Fruit> 可以存放Fruit的所有父类对象。只能写，不能读。因为读出来不能确定强转成什么类型，除非转成Object，这样是安全的。

举例：

Collections的copy方法：

![image-20211101164321780](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20211101164321780.png)

src只能用来读取，dest只能用来写入。

可以确保我们将List<Integer>复制到List<Number>

阻止反过来的情况。



# 包装类

基本数据类型：

| 数据类型 | 大小   |
| -------- | ------ |
| byte     | 8bits  |
| short    | 16bits |
| int      | 32bits |
| long     | 64bits |
| float    | 32bits |
| double   | 64bits |
| char     | 16bits |
| boolean  | 1bit   |



自动装箱、拆箱其实是编译器帮我们做了如下操作：

int a = 1;

int to Integer： Integer A = Integer.valueOf(a);

Integer to int:  a = intValue(A);

### 包装类的缓存：

给包装类的对象赋值时，如果值已经存在在缓存里了，那就会赋值缓存里的对象。不会重复创建。

### 各包装类的缓存：

Byte, Short, Integer, Long 都默认创建了[-128, 127]的缓存数据（也就是一个Byte的数据量）。

Character创建了char数值在[0,127]的缓存数据。（其实就是ASCII table）

Float和Double没有缓存



# Object类

一共有11个方法。

```java
//1. 比较两个对象的地址是否相等（this和obj是否指向同一个对象）
public boolean equals(Object obj) {
        return (this == obj);
    }
//2. 返回哈希码
public native int hashCode();
//3. 默认返回类名+这个对象的十六进制地址
public String toString() {
        return getClass().getName() + "@" + Integer.toHexString(hashCode());
    }
//4. 执行的是浅拷贝。必须实现Cloneable接口，不然会throw Exception
protected native Object clone() throws CloneNotSupportedException;
//5. 不能被子类重写
public final native Class<?> getClass();
//6. 实例被垃圾回收器GC回收时触发的方法
protected void finalize() throws Throwable { }
还有3个wait 2个notify
```

1. hashcode的作用：
   1. 生成哈希码，从而确定该对象在哈希表中的索引。当新的对象加入，只需要看该索引处有没有值，而不需要和表中每个对象都equals一遍。大大提高了效率。
2. 为什么重写equals就要重写hashcode()？

为了避免两个object相等但hashcode不等的情况。

hashcode（）原本返回由object地址生成的32位的整数

2. 浅拷贝和深拷贝

浅拷贝：对于对象内的引用变量，只复制引用地址，不将引用的对象也复制一份。

要想执行深拷贝，需要将引用变量也执行一次浅拷贝。执行完全的深拷贝几乎是不可实现的。https://www.cnblogs.com/javastack/p/12759352.html

finalize方法？？？

# String 类

常用方法：

charAt(), length(), substring(), split(), compareTo(), replace(), strip(), valueOf()

```java

void test4() {
        String s1="helloworld";
        String s2="hello";
        String s3="world";
    	//s2和s3在编译时都是地址，编译器无法识别对应的具体Stirng，所以不能直接引用s1，是通过StringBuilder append之后toString得到的新对象。
        String s4=s2+s3;
        System.out.println(s1==s4);//false;

    void test5() {
        String s1="helloworld";
        final String s2="hello";
        final String s3="world";
        //s2和s3都是final变量，在编译时就是常量，于是直接拼接后引用s1，没有创建新对象
        String s4=s2+s3;
        System.out.println(s1==s4);
    }
```

StringBuffer和StringBuilder的区别：

StringBuffer是线程安全的。因为给方法加了synchronized关键字

# 内部类

介绍一下内部类？

内部类有四种：成员内部类，静态内部类，局部内部类和匿名内部类。

一般来说内部类可以访问外部类的所有属性和方法。外部类也可以通过new内部类对象的方式访问内部类的所有属性和方法。

静态内部类比较特殊，独立于外部类，只能访问外部类的静态变量，创建静态内部类的对象也不用先创建外部类的对象。

局部内部类访问外部类的局部变量时，变量应设为final。

内部类的好处：

实现多重继承

可以把一个类用private修饰，隐藏起来，体现良好的封装性

匿名内部类简化接口实现



成员内部类：可以用修饰符修饰。

- 内部成员类可以访问和修改外部类的所有属性和方法

- 外部类的对象可以通过new内部类对象，访问和修改内部类的所有属性和方法

  - ```java
    Inner inner = new Outer().new Inner();
    ```

- 内部通过Outer.this获得外部类当前对象的引用。

- 内部对象不能脱离外部对象单独存在。

静态内部类：

- 外部类通过new内部类对象来访问其属性和方法。

- 但不再依附于外部类，也失去了Outer.this引用。所以只能访问外部的静态资源。

局部内部类：不能有任何修饰符修饰，同样可以访问父类所有资源，但父类只能在方法内访问该内部类。

匿名内部类：不能有任何修饰符修饰，可以访问父类所有资源。Comparator的匿名实现类

**为什么局部内部类和匿名内部类使用的局部变量要加final？**

因为局部变量和内部类对象的生命周期不同。方法执行完后，变量被销毁了，对象可能还在。为了不失去对变量的访问，编译器帮我们自动在内部类中创建了一个局部变量的备份。不想让备份一直改变，所以规定把局部变量设成final。



![img](https://uploadfiles.nowcoder.com/images/20190109/242025553_1547012774538_BA9669C5826A238ACEC0BD86755FA5DB)



# 反射

什么是反射：

反射是指在程序运行时通过Class对象来获取类的信息，包括属性和方法。

# 动态代理

动态代理核心涉及1个类一个接口：Proxy类和InvocationHandler接口。

Proxy的newProxyInstance静态方法接收三个参数：

1. 一个classloader
2. 一个需要代理的interface的对象
3. 一个InvocationHandler的实现类对象。

InvocationHandler只需要重写invoke方法。也接收三个参数：

1. Object proxy
2. Method类对象（其实就是被代理的方法）
3. String[] args Method对象的传参

在方法里调用method.invoke(target, args);即用了反射来调用被代理类的额方法。

```java
interface Sing;// 有个Sing interface
Singer singer; //一个Singer类
//创建这个Singer类的代理类
Sing singproxy = (Sing)Proxy.newProxyInstance(singer.getClass().getClassLoader(), singer.getClass().getInterfaces(), new InvocationHandler(){
   @Override
   public Object invoke(Object proxy, Method method, String[] args){
       //代理一些其他事务
       method.invoke(singer, args);//通过反射调用被代理对象的method
       //代理一些其他事务
   }
});
动态代理的好处：
    1. 不修改被代理类（好像有点像开闭原则）
    2. 不会产生过多的代理类，不需要事先定义代理类。
```



```java
//一个例子
public static void main(String[] params) {
    District district = new District("皇小明"); // 区长儿子 皇小明
    Behaviour districtProxy = (Behaviour) getProxy(district); // 这里为什么拿到的是 Behaviour 类型，因为接口不能实例化，Proxy.newProxyInstance 顾名思义是生成代理接口对象
    districtProxy.eat(); // 区长儿子吃饭饭
    districtProxy.write("爸爸的爸爸学代理"); // 区长儿子写作文
    System.out.println(districtProxy);
}

public static Object getProxy(final Object target) {
    Object proxy = Proxy.newProxyInstance(
            target.getClass().getClassLoader(), // 代理目标类的 class 的 class 生成器
            target.getClass().getSuperclass().getInterfaces(), // 代理类要实现的接口 本例中是 目标类的父类的接口
            new InvocationHandler() { // 中转反射器，代理类执行方法，到这里，中转反射到目标类上，如果感觉名字怪怪的那就对了，我自己瞎起的，不在意这些细节
                @Override
                public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                    System.out.println("孩子们" + method.getName() + "了...");
                    Object result = method.invoke(target, args);
                    // System.out.println(result);
                    System.out.println("孩子们" + method.getName() + "结束了...");
                    return result;
                }
            }
    );
    return proxy;
}
```

# 面向对象设计原则

```
SOLID
1. single responsibility 单一职责原则
一个类只负责一个职责
2. open closed 开闭原则
一个类对修改关闭，对扩展开放
3. 里氏替换原则
任何父类对象可以出现的地方，子类对象也应该可以出现
4. 接口隔离原则
接口要尽可能小，做的事情要尽可能统一
5. dependence inversion 依赖倒置原则
面向抽象编程，而不要面向抽象的具体实现编程
```



# 异常处理

# IO流

Java内部采用UTF-16编码格式。一个char占16bits（2bytes）。

UTF-16是code unit=16位的变长编码格式。用1个或2个code unit代表字符，即2bytes或4bytes

### InputStream

节点流：FileInputStream

处理流：BufferedInputStream

### OutputStream

节点流：FileOutputStream

处理流：BufferedOutputStream



# 序列化

# 抽象类

抽象类和接口的异同：

抽象类拥有成员变量和成员方法。接口只有抽象方法（jdk8以后可以有成员方法）。

抽象类和接口都不能创建实例，一个类继承抽象类或实现接口都需要实现所有抽象方法。

接口是对行为的抽象。抽象类是对事物的抽象



init和clinit区别
①init和clinit方法执行时机不同

init是对象构造器方法，也就是说在程序执行 new 一个对象调用该对象类的 constructor 方法时才会执行init方法，而clinit是类构造器方法，也就是在jvm进行类加载—–验证—-解析—–初始化，中的初始化阶段jvm会调用clinit方法。

②init和clinit方法执行目的不同

init is the (or one of the) constructor(s) for the instance, and non-static field initialization.
clinit are the static initialization blocks for the class, and static field initialization.
上面这两句是Stack Overflow上的解析，很清楚init是instance实例构造器，对非静态变量解析初始化，而clinit是class类构造器对静态变量，静态代码块进行初始化。看看下面的这段程序就很清楚了。

# JVM

## 创建对象时代码执行顺序

父类static

子类static

父类成员变量

父类构造器

子类成员变量

子类构造器

## JVM内存区域

1. 堆
   1. 存放内容：
      1. 对象实例
2. 方法区
   2. 存放内容：
      1. 类的信息（包括类的名称，方法信息，变量信息）还有静态变量，常量
         - 方法存储文章https://blog.csdn.net/wqq3670/article/details/105401190
      2. 运行时常量池
         1. class文件的常量池包含字面量和符号引用，在类加载的时候加载进运行时常量池。
         2. 有动态性。在运行过程中可以添加对象进常量池。
3. 虚拟机栈
   1. 一个方法的调用就是一个栈帧，每个栈帧包含局部变量表，操作数栈，方法返回地址等等。
4. 本地方法栈
5. 程序计数器
   1. 指向当前线程所执行的字节码的行数。
   2. 线程切换回来后知道从哪里继续执行。

## Java内存模型

有一块主内存，每个线程都有独立的工作内存。工作内存中的变量是主内存中变量的副本，不同工作内存之间不能直接互相访问。变量传递要通过主内存。

这里的变量指成员变量和静态变量，这有这些才是线程共享的。局部变量和方法参数是线程私有的，不会造成冲突。

## 类加载过程

加载 连接（校验，准备，解析） 初始化

### 加载：

1. 通过类的全限定名把描述类的二进制字节流传入方法区。
2. 把静态数据结构转换成运行时数据结构
3. 在堆中生成一个Class对象作为方法区这些数据的访问入口

##### 类加载器

applicationClassLoader：加载classpath下的内容

extensionClassLoader：加载jre/lib/ext下的一些内容

BootstrapClassLoader：加载核心类，如jre/rt.jar包

##### 双亲委派机制

自底向上检查类是否被加载。自顶向下尝试加载类

好处：避免重复加载类。因为一个类和加载它的类加载器共同确定类的唯一性。保证核心API不被篡改。



### 连接：

	1. 校验：确保Class文件信息符合JVM虚拟机规范
	2. 准备：在方法区中为static变量分配内存，赋上默认值。如果是常量则直接赋值
	3. 解析：把运行时常量池里的符号引用替换成直接引用

### **初始化**：

真正开始执行java字节码，调用<clinit>方法，执行static变量的赋值和static代码块。

什么时候一定要初始化：

```java
1. 当遇到new,getstatic,putstatic,invokstatic四个字节码指令时
    分别对应new对象，获取静态变量，给静态变量赋值，调用静态方法
2. 反射调用
3. 子类初始化时会先初始化父类
4. main方法所在类会先初始化
```



获取静态变量、方法，设置静态变量，new对象，子类被初始化时，父类要先初始化， main方法所在的类。

**什么时候不一定要初始化**：static final常量不需要初始化类就能获取。因为在连接的准备阶段，常量就被赋值并放进方法区里了。

## 垃圾回收机制

### 判断对象是否存活

1. 引用计数法。
   1. 给对象添加一个引用计数器，每当有一个地方引用它，计数器+1，当引用失效，计数器-1。弊端是无法解决对象间循环引用问题。
2. 可达性分析。
   1. 通过GC Roots对象开始搜索所有引用链，如果没有引用链和该对象相连，则该对象可被回收。

### 哪些对象可作GC Roots

1. 虚拟机栈和本地方法栈中引用的对象
2. 方法区中静态变量和常量引用的对象
3. 被synchronized锁持有的对象

### 不可达的对象一定会被回收吗

不可达的对象如果没有重写finalize方法或者重写了但已经被调用过了，就会被立刻回收。

如果重写了finalize方法且没被调用过，就会被加入一个F-Queue队列中，由优先级较低的Finalizer线程去执行。执行完finalize方法如果还是不可达，就被回收。

### 四种引用

1. 强引用。我们自己写的代码绝大部分都是强引用。即使内存溢出也不回收
2. 软引用 SoftReference。内存够用就不会被回收。存活到内存溢出前，就会被回收。用作缓存。
3. 弱引用 WeakReference。存活到下一次GC发生之前。
4. 虚引用。跟没有引用一样随时可能被回收。需要跟ReferenceQueue搭配使用，回收对象前会把虚引用加入队列。所以可以用虚引用来跟踪对象被垃圾回收的活动。

### 垃圾回收算法

1. 标记-清除算法
   1. 先标记，再清除。会导致大量不连续的碎片。
2. 标记-复制算法
   1. 把内存分成两块，每次只使用其中的一块。当这一块内存用完后，会把还存活的对象复制到另一块，把这块使用的空间清除干净。
   2. 优点：不会导致不连续碎片
   3. 缺点：内存可用空间减半。且如果存活对象数量多，复制会很耗时
3. 标记-压缩算法
   1. 标记后将存活的对象都向一端压缩移动。然后清理边界之外的空间。

将堆分成新生代和老年代

新生代对象存活周期短，每次GC都有大量对象被回收，所以使用标记复制算法。将新生代区域分为一个较大的Eden和两个较小的suvivor空间。每次使用Eden和一个suvivor，GC时将存活对象都复制到另一个suvivor

老年代对象存活周期长。使用标记-压缩算法





# 多线程

## 进程和线程

**进程**是程序的一次执行过程，是系统运行程序的基本单位，也是分配资源的基本单位。

**线程**是在进程中执行的一个任务，是比进程更小的执行单元。一个进程里面的多个线程共享堆和方法区。在进程中创建线程比直接创建进程消耗的资源要少，在线程间切换也比在进程间切换消耗的资源要少，而且由于线程们共享内存空间，线程间通信也要更方便。

## 创建多线程

两种方式

1. 继承Thread类

   ```java
   class MyThread extends Thread{
       public MyTread(){
           
       }
       //必须重写run方法
       @Override
       public void run(){
           
       }
   }
   public class Test{
       //创建实例后start线程
       MyThread mythread = new MyThread();
       mythread.start();
   }
   ```

2. 实现Runnable接口

   ```java
   class MyRunnable implements Runnable{
       public MyRunnable(){
           
       }
       @Override
       Public void run(){
           
       }
       
   }
   public class Test{
       MyRunnable runnable = new MyRunnable();
       //Runnable实例必须当作参数传递给Thread构造函数
       Thread thread = new Thread(runnable);
       thread.start();
   }
   ```

## 线程不安全的例子

1. 票超售 （读取-修改-写入）
2. 懒汉式单例模式（先检查-后执行）
3. 对象溢出（实例化还没结束就把this对象泄露出去了）

## Thread类的方法

1. start()
   1. start方法用来启动线程，调用start后线程进入new状态，等待系统为线程分配资源，分配好资源后进入ready状态
2. run()
   1. 当线程获得CPU执行时间，run方法就会被调用
3. sleep(long millis) //参数为毫秒
   1. 调用sleep后交出CPU，进入timed_waiting状态，sleep时间到了进入ready状态
   2. sleep方法不会释放锁
   3. 必须捕获 InterruptedException 异常或者将该异常向上层抛出
4. yield()
   1. 调用yield后交出CPU，直接进入ready状态
   2. yield不会释放锁
5. join(),  join(long millis) //参数为毫秒
   1. 假如在main线程中调用thread.join()方法，main会等到thread执行完毕后再继续执行。如果带参数，就会等待参数时间后。？？
   2. main线程实际调用了wait方法，交出CPU，并释放锁
6. interrupt()
   1. interrupt方法可以中断由sleep(), join(), wait()方法导致的线程阻塞。

**还有几个跟线程属性相关的方法**

1. getId
2. getName, setName
3. getPriority, setPriority
4. Thread.currentThread()获取当前线程
5. setDaemon, isDaemon 设置线程和判断线程是否为守护线程

守护线程会依赖于创建它的线程，当创建它的线程结束后，守护线程也会结束。垃圾回收线程就是守护线程。而用户线程则不会，会一直运行到程序结束。

## synchronized关键字

### 说说对synchronized关键字的了解

synchronized是使用了对象的内置锁，来实现对变量的同步操作，保证同一时间只有一个线程可以执行该段代码。能够保证变量操作的原子性、有序性和其他线程对变量的可见性。从而确保了并发安全。

### 怎么使用synchronized的

1. 修饰实例方法，获取的是该对象实例的内置锁
2. 修饰静态方法，获取的是该类对象的内置锁
3. 修饰代码块，在括号里指定利用哪个对象的锁

### synchronized实现原理

synchronized代码块：

synchronized代码块在编译后，代码前有一个monitorenter指令，代码后有一个monitorexit指令。在执行monitorenter指令时，首先要尝试获取对象的锁。如果该锁的计数为0，或者该线程已经获得了该锁，那就把锁的计数+1， 在执行monitorexit时，将锁的计数-1， 当锁的计数变为0时，线程就释放锁。如果一个线程已经把锁占用，那另一个线程只能阻塞等待。

synchronized方法：

JVM通过ACC_SYNCHRONIZED标志来确定方法是同步方法。

## volatile关键字

只能用来修饰变量。保证了变量的可见性，禁止了指令重排。但不保证原子性。 

### 如何实现可见性

volatile修饰的变量，在一个线程中修改后写入到工作内存，之后会立刻强制被写入主内存，并且会导致其他线程的工作内存中该变量的缓存失效，其他线程就要重新从主内存中读取该变量的值。

### 如何避免指令重排造成问题

加了volatile关键字后，在汇编代码中会增加一个lock前缀指令。相当于一个内存屏障，不会把这个屏障之前的指令排到屏障之后，也不会把屏障之后的指令排到屏障之前。即在执行到内存屏障之前，前面的所有指令都已经执行完毕了。

##### 什么是指令重排

出于代码优化的考虑，JVM实际执行顺序会和书写顺序不一致，但最终结果会和按书写顺序执行代码的结果一致。在单线程中没有问题，但在多线程中会造成问题。

## wait和notify，notifyall

这三个方法都只能在synchronized代码块或方法中使用，且必须由synchronized使用的内置锁的对象调用。

thread1中this.wait()之后，thread1进入wait状态。

thread2中this.notify()之后，处于wait状态的thread1被唤醒，进入ready状态，这时thread2还不会立刻释放锁，会将现在执行的synchronized代码执行完毕，之后再释放锁。

## sleep方法和wait方法的区别

1. sleep是thread对象的方法。只能睡指定时间。调用sleep方法后会让出CPU但不会释放锁。
2. wait是内置锁对象的方法。可以一直wait直到被唤醒，也可以wait指定时间后自动醒。调用wait会释放锁。wait只能在同步方法和代码块中使用。

## lock接口和synchronized关键字

1. Lock是一个接口，synchronized是java内置的关键字
2. synchronized发生异常时会自动释放锁，lock不会，所以一般都需要在finally块中释放锁unlock()
3. lock可以让等待锁的线程中断，synchronized不行。
4. lock可以知道有没有获得锁，synchronized不行。
5. lock有读写锁，可以提高多个线程读操作的效率。

## 可重入锁等

可重入锁：锁是基于线程分配的，不是基于方法分配的。

可中断锁：Lock是可中断锁，synchronized不是。

公平锁：ReentrantLock lock = new ReentrantLock(true) 是公平的。等待最久的线程最先得到锁。synchronized不是。

读写锁：读写分开的锁。

synchronized voltile 指令重排 死锁 异常 lock锁

## 线程安全的容器

1. vector
2. SynchronizedList
3. CopyonWriteArrayList
   1. 写时复制
4. ConcurrentHashMap

## 原子类

AtomicInteger 类

compareAndSet(int expected, int newValue);

这个方法会先比较现在的值跟expected是否相等。如果相等才会把新值赋给对象。

compareAndSet底层用到了CAS指令。如果现在的值和expected值不等，就不会赋予新值。

## ThreadLocal

每个线程对象都有一个ThreadLocalMap对象。

这个map以ThreadLocal对象为key，以真正的存储对象为value。

ThreadLocal对象重写initialValue方法

ThreadLocal.get() 方法底层是获取这个线程的ThreadLocalMap对象，map对象通过get(this)的方式获取ThreadLocal对象对应的value值。

## 线程池

```java
new ThreadPoolExecutor（
	int CorePoolSize,
	int maximumPoolSize,
	long keepAliveTime, //核心线程之外的空闲线程的最大存活时间
	TimeUnit unit,
	BlockingQueue<Runnable> workQueue,//核心线程都在工作，则新进来的任务进入阻塞队列等待。
	ThreadFactory threadFactory,
	RejectedExecutionHandler handler // 当任务总数大于maximumPoolSize+workQueue大小时，再进来的任务会被拒绝。
）
```

Runnable对象和Callable对象

Callable能够抛出异常和返回结果



# 设计模式

## 单例模式

```java
class Singleton{
    private static volatile Singleton singleton;
    private static Singleton(){
        
    }
    public static Singleton getInstance(){
        if(singleton == null){
            synchronized(Singleton.class){
                if(singleton == null){
                    singleton = new Singleton();
                }
            }
        }
        return singleton;
    }
}

class Singleton{
    private static class SingletonHolder{
        private static final Singleton singleton = new Singleton();
    }
    public static Singleton getInstance(){
        return SingletonHolder.singleton;
    }
}
```

