Java

[TOC]



# Java基础

## 常量与变量

### 常量：在程序运行期间，固定不变的量

- 字符串常量：用双引号引起来的部分，"abd","123"

- 整数常量：直接写上的整数，200,2,3

- 浮点数常量：有小数点的数字，330.99,3.0

- 字符常量：用单引号引起来的单个字符常量，'a','A',' '。' '不能用于打印输出

- 布尔常量：只要取值true,false

- 空常量：null,代表没有任何数据。

### 变量：

程序运行期间，内容会改变的量

​				a.变量名称不可以重名。

​				b. byte与short使用时需要注意取值范围。

​				c. 变量在使用时，需先创建，先赋值。

​				d. 变量使用需在变量的作用域中使用。

### 局部变量与成员变量：

| 变量类型 |            | 作用域 | 默认值 | 内存位置    | 生命周期     |
| -------- | ---------- | ------ | ------ | ----------- | ------------ |
| 局部变量 | 方法内     | 方法内 | 无     | Stack       | 与方法同周期 |
| 成员变量 | 类内方法外 | 类内   | 有     | Heap        | 与对象同周期 |
| 静态变量 | 类内       |        |        | Mathod Area | 与类同周期   |

**变量的默认值**

| 变量类型                  | 默认值 |
| ------------------------- | ------ |
| 浮点类型(float,double)    | 0.0    |
| 整型(byte,short,int,long) | 0      |
| 引用类型                  | null   |
| 字符型(char)              | \u0000 |
| 布尔类型(boolean)         | false  |

【注】：

​			**final修饰的变量不会有默认值，需要手动进行一次赋值**。

## 数据类型

### 基本数据类型：

包括：整数，浮点数(浮点数不一定是精确数)，字符，布尔。

【基本数据类型的包装类也不能被继承。】

| 数据类型     | 关键字  | 位数 | 内存占用(字节) |            |
| ------------ | ------- | ---- | -------------- | ---------- |
| 字节型       | byte    | 8    | 1              | -2^7~2^7-1 |
| 短整型       | short   | 16   | 2              |            |
| 整型         | int     | 32   | 4              |            |
| 长整型       | long    | 64   | 8              |            |
| 单精度浮点型 | float   | 32   | 4              |            |
| 双精度浮点型 | double  | 64   | 8              |            |
| 字符型       | char    | 16   | 2              |            |
| 布尔型       | boolean | 8    | 1              |            |

【注】：

a.浮点数默认类型为double,如果需要使用float类型，需要添加后缀F。

b.整数默认为int类型，如果要使用long类型。则需要添加L后缀。

### 引用数据类型:

类、数组、接口，字符串，Lambda。

#### String

String类型是引用类型，String被声明为final，因此它不可以被继承。

Java 8中，String内部使用char数组存储数据。

```java
public final class String implements java.io.Serializable, Comparable<String>, CharSequence {
	 /** The value is used for character storage. */
    private final char value[];
}
```

在Java 9 之后，String类的实现改用byte数组存储字符串，同时使用coder来标识使用哪种编码。

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final byte[] value;

    /** The identifier of the encoding used to encode the bytes in {@code value}. */
    private final byte coder;
}
```

value数值被声明为final，这意味着value数组初始化之后就不能再引用其他数组。并且String内部没有改变value数组的方法，因此可以保证String不可变。

【**不可变的好处**】

- 可以换成hash值

  因为String的hash值经常被引用，例如String用做HashMap的key。不可变的特性可以使得hash值也不可变，因此只需要进行一次计算。

- String Pool的需要

  如果一个String对象已经被创建过，那么就会从String Pool中取得引用。只有String是不可变的，才可能使用String Pool。

- 安全性

  String经常作为参数，String不可变性可以保证参数不可变。

- 线程安全

  String不可变性天生具备线程安全，可以在多个线程中安全的使用。

#### StringBuffer

被final修饰，长度可变，同时被synchronized修饰，效率低。

#### StringBuilder

被final修饰，长度可变，效率高。

![image-20200509215443004](F:\Document\note\jimages\String-StringBuffer-StringBuilder.png)

### 数据类型转换

- 自动转换：数据范围由小到大，可以实现自动转换

  ![](F:\Document\note\jimages\类型自动转换.png)

- 强制转换：类型数据范围小，需要由范围大的类型转为合适的数据类型，转换中肯能导致数据精度损失。

  格式:范围小的类型 范围更小的变量名=(范围小的类型)原本范围打的数据；

- 注意事项：强制类型转换一般不推荐，在转换时可能出现数据精度损失。byte/short/char都可以进行数学运算，在运算时会被提升成为int类型然后进行计算。

## 运算符：

Java中有算数运算符，赋值运算符，比较运算符，逻辑运算符，三元运算符

### 算数运算符

| 运算符 | 名称      |
| ------ | --------- |
| +      | 加        |
| -      | 减        |
| *      | 乘        |
| /      | 除        |
| %      | 取模/余数 |
| ++     | 加加      |
| --     | 减减      |

注：运算中，结果会以数据类型范围更大的数据类型展示。与String类型一起进行"+"操作时，是进行字符串拼接。前"++"/"--",先加/减，后使用；后"++"/"--"先用，后加/减。

### 赋值运算符

| 赋值运算符 | 名称   |
| ---------- | ------ |
| =          | 等于号 |
| +=         | 加等   |
| -=         | 减等   |
| *=         | 乘等   |
| /=         | 除等   |
| %=         | 取模等 |

注：将"="右边的数据交给左侧的变量。

### 比较运算符

| 比较运算符 | 名称                             |
| ---------- | -------------------------------- |
| ==         | 比较两边数据是否相等             |
| <          | 比较左边数据是否小于右边数据     |
| >          | 比较左边数据是都大于右边数据     |
| <=         | 比较左边数据是否小于等于右边数据 |
| >=         | 比较左边数据是否大于等于右边数据 |
| !=         | 比较左右两边数据是否不相等       |

注：比较结果为boolean值true/false。

### 逻辑运算符

| 逻辑运算符 | 名称     |
| ---------- | -------- |
| &&         | 与(并且) |
| \|\|       | 或(或者) |
| !          | 非(取反) |

注：结果为boolean值true/false。作用于两个布尔值之间进行运算。

### 三元运算符

用来完成简单的选择逻辑，根据条件判断，从两个中选择一个执行。

使用格式：

(条件表达式)?表达式1:表达式2；

运算规则：

​		a)判断条件表达式，结果为一个布尔表达式。

​		b)true，运算结果为表达式1的值。

​		c)false，运算结果为表达式2的值。

### 位运算及位运算符

使用位运算，可以减少运行开销，优化算法。位移运算符中，除~以外，其余均为二元运算符。操作数只能为整数和字符型数据。

- 按位与(&)

  规则为：只有两个操作数对应位同时为1时结果为1，其余全为0。

- 按位或(|)

  规则为：只有两个操作数对应位同时为0时，结果为0，其余全为1。

- 按位非(~)

  规则为：对操作数的每一位进行取反操作。

- 按位异或(^)

  规则为：两个操作数对应位相同时为0，不相同时为1。

- 左位移运算符(<<)

  m<<n

  规则为：把操作数所有位都左移规定位数，相当于把操作数在无溢出的前提下乘以2的n次方。

- 右位移运算符(>>)

  m>>n

  规则为：把操作数所有位都右移规定的次数，相当于把m除以2的n次方。

  【注】：如果是单数，也就是二进制末尾为1，则结果是将m除以2的n次方的整数商。

- 无符号右移运算符(>>>)

  m>>>n

  规则为：把操作数所有位都右移规定的次数，相当于把m除以2的n次方的整数商，但是结果全变成正数。

  ```java
  public class Move {
      public static void main(String[] args) {
          int a = 16;
          int b = -16;
          // 左移4位数，相当于乘以16，预期结果是256
          System.out.println(a << 4);
          // 右移4位数，相当于除以16，预期结果是1
          System.out.println(a >> 4);
          // 右移4位数，相当于除以16，预期结果是1
          System.out.println(a >>> 4);
          // 右移4位数，相当于除以16，预期结果是1
          System.out.println(b >>> 1);
  
      }
  }
  ```

  

# 面向对象

## 类和对象

### 类：

一组相关属性和行为的集合。属性描述的是该事物的熟悉，行为是具体的可操作行为。

### 对象：

类是对象的抽象化描述，对象是类的具体实例。

类的定义包含属性和行为；在Java类中成员变量(属性)，成员方法(行为)。

成员变量定义在类内方法外。局部变量定义在方法内。

### 内部类：

内部类介绍

1）内部类可以很好的实现隐藏(一般的非内部类不允许有private与protected权限);

2）内部类拥有外围类的所有元素的访问权限；

3）内部类可以间接实现多重继承，最重要的是，内部类允许继承多个非接口类型(类或抽象类);

4）内部类可以有多个实例，每个实例都有自己的状态信息，并且与外围类对象信息相互独立；

【注】：

​			1）在类中定义一个类(成员内部类，静态内部类)；

​			2）**【在方法中定义一个类(局部内部类，匿名内部类)，没有访问修饰符】。**

#### 静态内部类

1）最简单的内部类形式，只要在类定义时加上static关键字；

2）不能与外部类有相同的名字；

3）被编译成独立的.class文件，名称为OuterClass$InnerClass.class的形式。

4）【**只可以访问外部类的静态成员变量和静态方法，包括私有静态成员变量和方法;**】

5）生成静态内部类对象的方式：

```java
OuterClass.InnerClass inner=new OuterClass.InnerClass();
//外部类名称.内部类名称 对象名称=new 外部类名称.内部类名称();
```

```java
public final  class StaticClass {
    static class  InnerClass{
        //静态内部类
    }
    public class InnerClasss{
        //成员内部类
    }
}
```

#### 成员内部类

1）在类中定义另一个类，但是【不能用static修饰】；

2）成员内部类和静态内部类可以类比为非静态成员变量和静态成员变量。

3）**成员内部类就像是一个实例变量，可以访问外部类内所有的成员变量和方法，【不区分是否静态】**。

4）在外部类内创建成员内部类实例：

```java
this.new InnerClass();
//this.new 内部类();
```

5）在外部类之外创建成员内部类实例：

```java
OuterClass.InnerClass inner=new OuterClass().new InnerClass();
//外部类名.内部类名 对象=new 外部类名().new 内部类名();
```

6)在内部类里访问外部类的成员：

```java
OuterClass.this.member;
//外部类名.this.外部类成员
```

7)内部类的使用：

【注】：内部类变量与外部类重名时的使用，通过添加类名限定：

```java
public class Outer {    
    /**     
    * 外部类的成员变量     
    */    
    int num=10;   
    /**
    *成员内部类
    */
    public  class  Inner{        
        /**         
        * 内部类的成员变量         
        */        
        int num=20; 
        /**         
        * 内部类的成员方法         
        */   
        public void  methodInner(){            
            //内部类方法的局部变量            
            int num=30;            
            //局部变量，就近原则  
            System.out.println(num); 
            //内部类的成员变量 
            System.out.println(this.num);            
            //外部类的成员变量
            System.out.println(Outer.this.num);       
        }    
    }
    
    public static void main(String[] args) {
        //创建内部类对象
        Outer.Inner s=new Outer().new Inner();
    }
}
```

#### 局部内部类

1）**局部内部类【不能有访问修饰符】，因为不是外围类的组成，但是可以访问当前代码块内的【常量及此外围类的所有成员】；**

2）局部内部类定义在方法中，比方法范围小；

3）与局部变量一致，**【不能被public、protected、private和satic修饰】；**

4）**只能访问方法中定义的【final类型的局部变量】；**

5）**【局部内部类只能在方法中使用，即只能在方法中生成局部内部类实例并调用其方法】。**

```java
public final class StaticClass {
    private static  final String str="abc";
    private static String b;
    private  String a;

    static class InnerClass {
        // 静态内部类
    }
    public class InnerClasss {
        // 成员内部类
    }

    public void method() {
        // 成员方法

        class MethodInnerClass {
            // 局部内部类
        }
    }
}
```

#### 匿名内部类

1）**匿名内部类就是没有名字的局部内部类，不使用class、extends、implements关键字，【没有构造方法】。**

2）**匿名内部类隐式地继承一个父类或实现一个接口**。

3）通常作为一个方法参数。

4）如果定义一个匿名内部类，并希望使用一个在其外部定义的对象，则编译器会要求参数引用是final的，否则编译错误。

【注】：

1. 匿名内部类在【创建对象】、【调用方法】的时候，只能使用唯一一次，如果希望使用多次，则需要创建实现类；
2. 匿名内部类省略了【**实现类/子类名称**】，但是匿名对象是省略了【**对象名称**】，二者不相同。
3. 匿名内部类**【不能有任何静态成员变量、方法】**。
4. 匿名内部类**【类中的方法不能是抽象的】**。
5. **匿名内部类【必须实现接口或抽象父类的所有抽象方法】。**
6. 匿名内部类**【只能访问外部类的静态成员方法或成员变量】。**

```java
public class DemoMain {
    public static void main(String[] args) {
        /**
         * 使用匿名内部类
         */
        MyInterface obj=new MyInterface() {
            @Override
            public void method() {
                System.out.println("实现类覆盖重写方法!");
            }
        };
        /**
         * 使用了匿名内部类，省略了对象名称，也是匿名对象
         */
        new MyInterface(){

            @Override
            public void method() {
                System.out.println("匿名内部类");
            }
        }.method();
    }
}
```

#### 匿名内部类与局部内部类区别

1）使用场景：匿名内部类是用于实例初始化，如果需要重构构造器则需要局部内部类，局部内部类的名字在方法外是不可见的。

2）使用局部内部类：需要布置一个该类的对象。

## 类的初始化顺序

### 普通类

1. 静态变量
2. 静态代码块
3. 普通变量
4. 普通代码块
5. 构造函数

### 继承的子类

1. 父类静态变量
2. 父类静态代码块
3. 子类静态变量
4. 子类静态代码块
5. 父类普通变量
6. 父类普通代码块
7. 父类构造函数
8. 子类普通变量
9. 子类普通代码块
10. 子类构造函数

### 抽象的实现子类(接口-抽象类-实现类)

1. 接口静态变量
2. 抽象类静态变量
3. 抽象类静态代码块
4. 实现类静态变量
5. 实现类静态代码块
6. 抽象类普通变量
7. 抽象类普通代码块
8. 抽象类构造函数
9. 实现类普通变量
10. 实现类普通代码块
11. 实现类构造函数

## 面向对象特性

### 封装 ：

1）private关键字的作用及使用：

关键字封装成员属性，使其只能在类内可见，如需获取或改变值，则需要通过get/set方法实现。对于基本类型中的boolean类型的属性，get方法命名为isXxx形式。

2）this关键字：

**【当方法的局部变量和类的成员变量重名时，优先使用局部变量】**，如需要访问本类中的成员变量，则使用this关键字。<谁调用，谁就是this>。

### 继承

使用父类已有属性与方法，再添加新的熟悉与方法。

Java继承为单继承，其直接父类只能有一个，但一个父类可以有多个子类。

【注】：

1. 成员变量重名：

   1. 在父子类的继承关系中，如果成员变量重名，则创建子类时，访问方式有两种：

   - 直接通过子类对象访问成员变量：在等号左边是谁就优先用谁，没有就向上查找。

   - 间接通过成员方法访问成员变量：调用的方法属于谁就优先用谁，没有就向上查找。

   - 子类访问父类成员变量：使用super.父类成员变量名进行访问。

2. 成员方法重名：优先使用子类方法，覆盖重写优先使用子类方法。

3. 方法重写的返回值必须小于等于父类的返回值范围，访问修饰符必须大于等于父类访问修饰符<public>protected>(default)>private>。

4. 继承关系中构造方法的访问特点：子类构造方法可以通过super调用父类重载构造。且super的父类构造调用必须是子类构造方法的第一个语句，不能调用多次super构造。

### 多态

多态的前提是继承与实现。两种表现形式：方法重载和方法重写。

多态的使用：

​	**父类名称 对象名=new 子类名称();  或   接口名称 对象名 =new 实现类名称();**

多态在实际开发与运用中，成员方法：编译看左边，运行开右边；成员变量编译开发均看左边。

## 上下转型

​		向上转型：子类转父类，创建子类可以当做父类使用，向上转型是安全的；

​						注：子类向上转型后，无法使用子类的方法。

​		向下转型：将父类对象还原为子类对象。

​						注：向下转型必须判读是否为子类对象。

## 构造方法：

方法名与类名一致，无返回值

### 	无参构造方法：

无参数的构造方法，new 的时候就是调用无参构造方法创建新的对象；如果未编写构造方法，编译器会创造一个默认无参构造方法。一旦编写构造方法，编译器则不会创建默认构造方法。构造方法可以进行重载。

### 有参构造方法：

有参数的构造方法，全参或部分参数。

### 可变参数

当方法的参数列表数据类型已经确定，但是参数个数不确定时，可使用可变参数。

使用格式：

定义方法时使用

修饰符  返回值类型 方法名(数据类型... 变量名){}

### 匿名对象：

只有右边对象，没有名字和赋值运算符。匿名对象只能使用一次，再次使用必须创建。对于确定只使用唯一一次的对象可以使用匿名对象。

​		new 类名称();

​		new Person().name="张三";

匿名对象传参：

```java
//传入参数
methodParam(new Scanner(System.in));
//作为返回值
Scanner sc=methodReturn();
public static Scanner methodReturn(){
    //匿名对象
    return new Scanner(System.in);
}
```

## 类的权限修饰符

| 是否可选(Y/N) | public | protected | default | private |
| ------------- | ------ | --------- | ------- | ------- |
| 外部类        | Y      | N         | Y       | N       |
| 成员内部类    | Y      | Y         | Y       | Y       |
| 局部内部类    | N      | N         | N       | N       |



## 泛型

泛型，是指在定义的时候，不知道具体的数据类型，在创建对象的时候确定其数据类型。泛型在集合中的所有元素只能是同一类型元素，而且泛型类型只能是引用类型，不能是基本类型。

泛型确定数据类型，避免数据类型引起的异常。泛型通配符<?>只能用于接收参数，用于方法参数，不能用于创建对象。

## 抽象类与抽象方法

抽象方法只有方法的定义，没有方法实现。

抽象类：abstract修饰的类。

```java
public abstract class{
public abstract void eat();
}
```

【注】：

1. 只要包含抽象方法的类一定是抽象类，但抽象类不一定包含抽象方法。
2. 抽象类不能直接创建抽象对象，抽象类的使用必须通过子类来使用。
3. 抽象类中可以有构造方法，是提供给子类创建对象时，初始化父类成员使用的。
4. 抽象类的子类必须重写抽象父类的所有抽象方法，否则编译报错，除非该子类也是抽象类。

## 接口

接口是多个类的公共规范，接口属于引用数据类型，最重要的内容是其抽象方法，其使用必须实现后才可以使用。

```java
public class interface{
	//常量 public static final 数据类型  常量名称=数据值；
    public static final int num=10;
    //抽象方法,修饰符只能为public abstract,修饰符可以选择性省略
    public abstract void method();
    abstract void method();
    abstract void method();
    void method();
    //默认方法,解决接口升级问题
    public default void defaultMethod(){};
    default void defaultMethod(){
        System.out.println("这是默认方法");
    };
    //静态方法
    public static void staticMethod(Para para,....){
        //方法体
    }
    //私有方法，java9
    private void privateMethod(){
        //普通私有方法
    }
    private static void privateStaticMethod(){
        //静态私有方法
    } 
}
```

1. 接口默认方法：接口默认方法可以在接口中通过default进行定义，并在接口实现类中进行调用，也可以在实现类中进行重写使用。
2. 静态方法：不能通过接口实现类的对象调用接口当中的静态方法，它的使用通过接口名称进行调用。
3. 接口私有方法：
   1. 普通私有方法：抽取出的接口中复用的方法。
   2. 静态私有方法：抽取出的可复用的静态复用方法。
4. 接口常量：接口中的常量必须赋值，被final修饰，一旦赋值不可更改，名称必须是大写字母。public static final修饰符可以省略不写。通过【接口名称.】的方式获取。
5. 实现类必须覆盖重写接口所有的抽象方法，除非实现类是抽象类。

【注】：

1. **接口没有静态代码块、构造方法、普通变量与普通代码块**。
2. **接口中的变量必须实例化，且都被final修饰，子类无法修改**。
3. 一个类的父类是唯一的，实现的接口可以是多实现的。接口与接口之间的继承是多继承的。
4. 如果实现类中所实现的多接口中，存在重复的抽象方法，则只需要覆盖重写一次即可。
5. 如果实现类没有覆盖所有接口中的所有抽象方法，那么实现类就必须是一个抽象类。
6. 如果实现类所实现的多接口中，存在重复的默认方法，那么实现类一定要对冲突的默认方法进行覆盖重写。
7. 如果实现类的直接父类中的方法与接口中的默认方法产生了冲突，优先使用父类中的方法。继承优先于接口实现。
8. **多继承的接口中的默认方法如果重复，那么子接口必须进行默认方法的覆盖重写，且需要添加default关键字【接口中的默认方法default关键字不能省略】**。

## 面向对象的原则

单一职责、开放封闭、里氏替换、依赖倒置、合成聚合复用、接口隔离、迪米特法则。

- 单一职责：

  一个类只做它该做的事。

- 开放封闭：

  对扩展开放，对修改关闭。

- 里氏替换：

  任何时候都可以用子类型替换父类型。

- 依赖倒置：

  面向接口编程(抽象类型可被任何一个子类所替代)。

- 合成聚合复用：

  优先使用聚合或者合成关系复用代码。

- 接口隔离：

  一个接口只应描述一种能力，接口应该高内聚。

- 迪米特法则：

  最少知识原则，一个对象应该对其他对象尽可能的少了解。



# Java内存

Run-Time Data Areas  运行时数据区域

![image-20200517165138840](F:\Document\note\jimages\jmm.png)



## 程序计数器(Program Couter Register)：

线程私有内存，保存当前线程所执行的字节码的行号指示器，这里的程序计数器与计算机组成原理中的程序计数器不太一样，计算机组成原理中的PC指的是下一条要执行的指令地址。JVM中常有多个线程执行，故每一个线程都需要有一个独立的程序计数器。如果线程执行的是Java方法，那计数器记录的就是当前正在执行的虚拟机字节码指令的地址，如果执行的是Native方法，则这个计数器为空。

## 栈(Stack)：

用来存放方法中的局部变量(成员变量存放在堆区)，以及对象的引用，每个线程包含一个栈区，并且为自身独有，其他栈不可访问，存在多少个线程，就有多少个栈。栈有3个部分：基本类型变量区、执行环境上下文、操作指令区。

## 本地方法栈(Native Method Stacks)：

与虚拟机栈类似，本地栈为Native方法服务(类似于C语言中的栈)

## 堆(Heap)：

堆是被所有线程共享的一块内存区域。一般来说所有的对象实例和数组都要在堆上分配，但是一些优化技术导致不一定所有的对象实例都在堆上分配。存储类的对象，在堆中不存放基本类型和对象引用，只存放对象本身，被所有线程共享。

## 方法区(Method area)：

储存所有方法的信息，.class文件，字符串常量，static类型变量。这些变量在整个程序中唯一，被所有进程共享。存储虚拟机加载的类信息、常量、静态常量和即时编译器编译后的代码数据等。

# Java注解



 Java注解又称Java标注，是Java语言5.0版本开始支持加入源代码的特殊语法元数据。为我们在代码中添加信息提供了一种形式化的方法，使我们可以在稍后某个时刻非常方便的使用这些数据。
Java语言中的类、方法、变量、参数和包等都可以被标注。和Javadoc不同，Java标注可以通过反射获取注解内容。在编译器生成类文件时，注解可以被嵌入到字节码中。Java虚拟机可以保留注解内容，在运行时可以获取到注解内容 。

##   内置注解

Java定义了一套注解，一共有7个，3个在java.lang中，4个在java.lang.annotation中。

1. 作用在代码的注解是

   - @Override --检查改方法是否是重写方法

   - @Deprecated --标记过时方法。如果使用该方法，会报编译警告。

   - @SuppressWarnings  --指示编译器忽略注解中声明的警告。

2. 作用在其他注解的注解(元注解)是

   - @Retention --标识注解如何保存，是只在代码中，还是编入class文件中，或者是在运行时通过反射访问。
   - @Document --标记这些注解是否包含在用户文档中。
   - @Target --标记这个注解应该是哪种Java成员。
   - @Inherited --标记这个注解是继承于哪个注解类。
   - @SafeVarags --忽略任何使用参数为泛型变量的方法或构造函数调用产生的警告。
   - @FunctionalInterface  --标识一个匿名函数或函数式接口。
   - @Repeatable --标识某注解可以在同一声明上使用多次。

## 元注解

1. @Retention  annotation指定标记注释的存储方式：

   - RetentionPolicy.SOURCE  --标记的注释仅保留在源码级别中，并由编译器忽略。
   - RetentionPolicy.CLASS  --标记的注释在编译时由编译器保留，但JVM会忽略。
   - RetentionPolicy.RUNTIME --标记注释由JVM保留，因此运行时环境可以使用它。

2. @Documented

   - @Documented注释表明，无论何时使用指定的注释，都应使用Javadoc工具记录这些元素。

3. @Target 注释标记另一个注释，以限制可以应用注释的Java元素类型。目标注释指定以下元素类型之一作为其值：

   - ElementType.TYPE 

     可以应用于类的任何元素。

   - ElementType.FIELD

     可以应用于字段或属性。

   - ElementType.METHOD 

     可以应用于方法级注释。

   - ElementType.PARAMETER 

     可以应用于方法的参数。

   - ElementType.CONSTRUCTOR 

     可以应用于构造函数。

   - ElementType.LOCAL_VARIABLE 

     可以应用于局部变量。

   - ElementType.ANNOTATION_TYPE 

     可以应用于注释类型。

   - ElementType.PACKAGE 

     可以应用于包的声明。

   - ElementType.TYPE_PARAMETER

   - ElementType.TYPE_USE

4. @Inherited

5. @Repeatable

# 异常



# API

## 键盘输入(Scanner)

```java
//传入参数
methodParam(new Scanner(System.in));
//作为返回值
Scanner sc=methodReturn();
public static Scanner methodReturn(){
    return new Scanner(System.in);
}
```

## 随机数(Random)：

随机数生成

```java
public static void main(String[] args) {
        Random random=new Random();
        //无范围
        int num=random.nextInt();
        System.out.println(num);
        //添加范围,左闭右开期间
        int num1=random.nextInt(9);
        System.out.println(num1);
        for (int i = 0; i < 100; i++) {
            int n=random.nextInt(10);
            System.out.println(n);
        }
    }
```



## 对象数组(ArrayList)

如果需要往ArrayList中添加基本类型数据，必须使用基本类型对应的"包装类"，从1.5之后，基本类型支持自动装箱与拆箱。

ArrayList常用方法：

```java
public boolean add(E e);//集合中添加元素，参数类型与泛型一致。
public E get(int index);//获取集合中的几号元素，入参为索引编号
public E remove(int index);//从集合中删除元素
public int size();//获取集合的长度
```



## 字符串(String)

String类代表字符串，Java程序中的所有的双引号字符串都是String类<public final class>的对象。是永不可变的。字符串的底层存储使用的是字符数组进行存储。

字符串常量池：程序中通过双引号直接创建的字符串全存在于常量池中。字符串常量池存在于堆(Heap)中，在字符串常量中，字符串对象存放byte[]的地址。

new的字符串不在常量池中。

对于引用类型来说，==进行的是地址的比较。

常用方法：

```java
/**
*参数可以是任何对象，只有参数是一个字符串且内容相同才会返回true
*/
public boolean equals(Object obj);
```

注：equals方法具有对称性，a.equals(b)与b.equals(a)相等。如果一个常量与一个变量进行比较时，推荐把常量写在前面。

## Object类

所有类的超级父类，所有类都继承于Object类。

Object类中的equals方法默认比较对象的地址值，比较结果默认就是false，所以在使用时需要重写equals方法，比较对象的内容。重写时，判断是否为具体对象实例与实例本身。

## Objects类

防止空指针异常；

## Date类

java.util.Date表示日期和时间类，表示特定的瞬间，精确到毫秒。

## DateFormat类与SimpleDateFormat类

在java.text.DateFormat中

```java
String format(Date date);//按照指定模式，把Date日期格式化为符合模式的字符串

Date parse(String source);//把符合模式的字符串解析未Date日期。
/**
* DateFormat是一个抽象类，无法直接使用，可直接使用其子类
* SimpleDateFormat
*/
```

## System类

## StringBuilder类

在内存中，存储数组默认长度为16，每次扩容为16。

# 关键字

## this

为当前对象的引用。当方法的局部变量和类的成员变量重名时，优先使用局部变量，如需要访问本类中的成员变量，则使用this关键字。<谁调用，谁就是this>。

1. 用法：用于访问本类的内容

   1. 在本类的成员方法中，访问本类的成员变量。
   2. 在本类的成员方法中，访问本类的另一个成员方法。
   3. 在本类的构造方法中，访问本类的另一个构造方法。

   在【c】的调用方法中，this()调用构造方法，必须是构造方法的第一个语句，也是唯一一个。super和this两种构造，不能同时使用。

## super

1. 用法：

   - 在子类成员方法中，访问父类的成员变量；
   - 在子类的成员方法中，访问父类的成员变量；
   - 在子类的构造方法中，访问父类的构造方法。

   ![](F:\Document\note\jimages\super-this.png)

## private

访问修饰符，private修饰的方法，成员变量的可见性只在本类中，实现的是封装性。

## break

跳出循环

## continue

直接跳过此次循环，进入下一次循环。

## static

1）static修饰的类成员变量，属于类(对象)共享数据，不属于对象私有数据，可通过对象调用，也可以通过类名调用。

```java
public class StaticVar{
    private static String name="静态变量";
    private String str;
    public static main(String[] args){
        //直接通过类名可以访问
        StaticVar.name;
    }
}
```

2）static修饰方法时，则成为静态方法，静态方法不属于对象，属于类，可以通过对象使用它，也可以【通过类名称使用】。

```java
public class StaticMethod{
    public static void method(){
        //静态方法
    };
    public static void main(String[] args){
        //方式一：直接通过类名访问
        StaticMethod.method();
        //方法二：通过对象访问
        StaticMethod staticMethod=new StaticMethod();
        staticMethod.method();
    }
}
```

3）static修饰类时，只能用于修饰内部类成为静态内部类，可以通过外部类名调用。

```java
public class OuterClass{
    static InnerClass{
        //静态内部类
        public void InnerMethod(){
            //静态内部类的方法
        }
    }
    public static void main(String[] args){
        //静态内部类对象创建
        OuterClass inner=new OuterClass().InnerClass();
		//静态内部类的使用 
        inner.InnerMethod();
    }
}

```

4）static修饰代码块：静态代码块 

```java
static{
/**
*当第一次用到本类时，静态代码块执行唯一的一次，且静态内容优先于非静态内容。静态代码块适用于只需要一次处理的场景。
*/
}
```

【注】：

**静态不能直接访问非静态(原因：在内存中，先有静态，后有非静态)，静态方法中不可以使用this与super关键字<因为this代表对象，而静态时，有可能没有对象，所以this，super无法使用>，静态随着类的加载而加载，而且优先于对象存在。**

## final、finally、finalize

final所修饰的类、成员变量、成员方法、局部变量均为最终的、不可改变的。

对于基本数据来说，其值不可改变，对于引用类型来说，其地址值不可变。

- final修饰类

  final 类为最终类，不能有子类。

- final修饰成员方法

  final方法为最终方法，不能被重写。

- final修饰局部变量

  final修饰局部变量，基本数据类型的值不能被改变，引用类型的地址值不能被改变，但是对象中的属性可以改变。

- final修饰成员变量

  成员变量被final修饰后，必须手动赋值，编译器不会再给默认值。赋值要么使用直接赋值，要么通过构造方法赋值，二选其一。

  对于成员变量来说，一旦使用final关键字，也是一样不能改变
  a、和局部变量的不同点在于，成员变量有默认值，因此必须手动赋值
  b、final的成员变量可以定义的时候直接赋值，或者使用构造方法在构造方法体里面赋值，但是只能二者选其一
  c、如果没有直接赋值，那就必须保证所有重载的构造方法最终都会对final的成员变量进行了赋值。

finally异常处理操作的统一出口

finalize是Object类所提供的一个方法，用于对象回收(GC)之前进行收尾工作。

## try、throw、throws

- throw和throws区别

  throw是语句抛出一个异常。throws是方法可能抛出异常的声明。(用在声明方法时，表示该方法可能要抛出异常)。

  【注】：

  throw语句用在方法体内，表示抛出异常，由方法体内的语句处理。

  throws语句用在方法声明方法后面，表示再抛出异常，由该方法的调用者来处理。

- try和throws区别

  如果该功能内部可以将问题处理，用try、如果处理不了，则交由调用者处理，用throws进行抛出异常。

  【区别】：后程序需要运行，则用try；后程序不需要继续运行，则用throws；

## switch

```java
switch(变量){
        //此处的变量类型支持byte,short,int,char,string
    case 常量1:
        //待执行的代码
        break;
    case 常量2:
        //待执行的代码
        break;
    case 常量3:
        //待执行的代码
        break;
    default:
        //待执行的代码
}
```

## instanceof

判断对象是否是子类对象，在对象向下转型的时候，一定要进行实例对象判断。

## 权限修饰符

|              | public | protected | (default) | private |
| ------------ | ------ | --------- | --------- | ------- |
| 同一class    | Y      | Y         | Y         | Y       |
| 同一package  | Y      | Y         | Y         | N       |
| 不同包子类   | Y      | Y         | N         | N       |
| 不同包非子类 | Y      | N         | N         | N       |

# Lambda表达式

Lambda表达式实质上是一个匿名方法，但是该方法并非独立执行，而是用于实现由函数式接口定义的唯一抽象方法。

使用lambda表达式时，会创建实现了函数式接口的一个匿名类实例。

可以将lambda表达式视为为一个对象，可以将其作为参数传递。

基本语法：

(parameters)->expression或(parameters)->{satements;}

重要特征：

- 可选类型声明：

  不需要声明参数类型，编译器可以统一识别参数值。

- 可选的参数圆括号：

  一个参数无需定义圆括号，但多个参数需要定义圆括号。

- 可选的大括号：

  如果主体只包含一个语句，就不需要大括号。

- 可选的返回关键字：

  如果主体只有一个表达式返回值则编译器会自动返回值，大括号需要指明表达式返回了一个数值。

# 异常

1. 编译时异常(Checked Exception)

   除了RuntimeException及其子类，Exception中所有的子类都是，这种异常必须要处理，否则编译不通过。

2. 运行时异常(Unchecked Exception)

   RuntimeException及其子类都是，这种异常不用处理，编译通过，会被JVM抛出。

3. 严重错误异常(Error)

   用Error进行描述，这个问题发生后，一般不编写针对程序进行处理，而是要对程序进行修正，通常都是由虚拟机抛出的问题。

   ![image-20200510225029018](F:\Document\note\jimages\execption.png)

# 浅拷贝和深拷贝

- 浅拷贝介绍

  基本数据类型直接拷贝，但是对象，直接将源对象中的引用值拷贝给新对象的字段，即所有的对其他对象的引仍然【指向原来的对象】。任何一个对该对象的修改、都会反映到其他引用上。

  拷贝一个对象时，拷贝这个对象里的所有字段，而引用对象不拷贝，直接拷贝指向这个引用对象的变量字段，所以如果副本中引用对象发生改变，源对象也会改变。

- 深拷贝介绍

  基本数据类型直接拷贝，但是对象，复制一个相同的对象，并将这个新的对象的引用赋给新拷贝的字段，即那些引用其他对象的变量将指向被复制过的对象。

  拷贝一个对象时，对象的字段和引用对象都会拷贝，所以副本和源对象是相互独立的，相等于一个克隆体。

- Java中的克隆

  Object中定义的clone()方法，是Protected，只有实现了Cloneable接口的类才可以在其实例上调用clone()方法，否则会抛出CloneNotSupportException。

- 深拷贝一个对象

  1、该对象实现Cloneable接口，实现clone方法，并且在clone方法内部，把该对引用的其他对象也clone一份，该被引用的对象也必须实现Cloneable接口并实现clone方法。

  2、利用串行化做深复制，将对象序列化，然后再进行反序列化。使对象实现Serializable接口。写在流里的是对象的一个拷贝，而原对象仍然存在于JVM里面，因此在Java语言里深复制一个对象，常常可以先使对象实现Serializable接口，然后把对象写到一个流里，再从流里读取出来遍可以重建对象。

- clone()方法的工作流程

  1、需要克隆的类实现Cloneable接口并重写clone()方法，创建副本；

  2、在改类中调用clone方法被委派给super.clone()，可以是自定义的super class或者是默认的java.lang.Object；

  3、最后调用到达java.lang.Object的clone()，验证相关的类是否实现了Cloneable接口，如果没有实现那么抛出CloneNotSupportedException，否则创建filed-by-filed。

# 集合

## Collection接口

![image-20200505114356042](F:\Document\note\jimages\Collection.png)

HashSet底层为一个Hash表，查询速度快，不存在重复元素，无索引，不能通过普通for循环遍历，只能通过iterator与增强for循环进行遍历。HashSet底层为:数组+链表或者数组+红黑树。使用HashSet存储数据，需要重写equals()与hashCode()方法。

操作集合的工具类Collections类；

操作数组的工具类Arrays类；

## Map接口

![image-20200505114447993](F:\Document\note\jimages\Map.png)

<K,V>键值对存储数据，键不可重复，值可以重复。HashMap实现Map接口，HashMap集合的底层是数组+单向链表/红黑树(链表数组长度超过8)，提高查询的速度，可以存储null值，null键。LinkedHashMap是有序集合，底层为哈希表+链表。

Entry对象遍历Map集合：

1、使用Map集合中的方法entrySet(),把Map集合多个Entry对象取出，存储到一个Set集合中。

2、遍历Set集合，获取每一个Entry对象。

3、使用Entry对象方法中的getKey()和getValue()获取键值对。

HashTable实现了Map接口，属于单线程安全集合，速度慢，底层是哈希表。键值均不能为空。

## Iterator接口

(java.util.Iterator)

```java
/**
* 判断是否有下一节点
*/
boolean hasNext();
/**
* 取出下一个节点元素
*/
E next();
//创建集合对象
Collection<String> coll=new ArrayList<>();
//多态 接口				实现类对象
Iterator<String> it=coll.iterator();
```

Iterator接口的泛型类跟随集合类泛型。迭代器获取集合元素。Iterator可以用来遍历Set和List集合，且只能向前遍历；但是ListIterator只能用于List集合遍历，可以先前或者向后遍历。

增强for循环：

for(集合/数组的类型 变量名：集合名/数组名){

}

# Java IO

从传输方式可以分为输入流与输出流。按照文件类型可以分为File、数组、管道操作、基本数据类型操作、缓冲操作、打印、对象序列化与反序列化、转换等。

![image-20200508230327516](F:\Document\note\jimages\IO-Class.png)

## 四个抽象类

| 类名         |            | 主要方法                                      |
| ------------ | ---------- | --------------------------------------------- |
| InputStream  | 字节输入流 | int read();void close();                      |
| OutputStream | 字节输出流 | void write(int);void flush();void close();    |
| Reader       | 字符输入流 | int read();void close();                      |
| Writer       | 字符输出流 | void write(String);void flush();void close(); |

## 标准步骤

1. 创建源。
2. 选择流。
3. 选择操作(读或写)。
4. 释放系统资源，非JVM。
5. 字节数组流可以不用关闭，由JVM GC时关闭。

## 磁盘操作(File)

File是基于磁盘的操作，File类包含对文件及目录结构的操作及属性。File实现了Serializable与Comparable接口。

```java
/**
* File类构造方法
*/
public File(String child,File parent);
public File(String pathname);
public File(String parent,String child);
public File(File parent,String child);
public File(URI uri);
/**
* File类获取功能的方法
*/
public File getParentFile();
public String getPath();
...
/**
* File类判断功能的方法
*/
public boolean canWrite();
public boolean canRead();
....
    
/**
 * 文件创建
 */
 public boolean createNewFile();
/**
*文件删除
*/
public boolean delete();
/**
*File类遍历
*/
public File[] listFiles();
...


```

文件字节流：

```java
public class StreamDemo {
    public static void main(String[] args)  {
        // 获取文件对象
        File file = new File("D:/Workspace/test.txt");
        OutputStream outputStream=null;
        try {
           outputStream=new FileOutputStream(file,true);
           //写出
            String msg="File Output Stream";
            //字符串-->字节数组(编码)
            byte[] bytes=msg.getBytes();
            for (int i = 0; i < bytes.length; i++) {
                System.out.print(bytes[i]);
            }
            outputStream.write(bytes, 0, bytes.length);
            outputStream.flush();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (null!=outputStream){
                    outputStream.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
}
```



## 字节操作(InputStream和OutputStream)

## 字符操作(Reader和Writer)

## 网络操作(Socket)

## 新的输入/输出(NIO)

NIO主要有三大核心部分：Channel(通道)，Buffer(缓冲区)，Selector。传统IO基于字节流和字符流就行操作，而NIO基于Channel和Buffer进行操作，数据总是从通道读取到缓冲区中或者从缓冲区写入到通道中。Selector(选择区)用于监听多个通道的事件(比如：连接打开，数据到达)。因此，单个线程可以监听多个数据通道。

NIO和传统IO之间的区别:IO是面向流的，NIO是面向缓冲区的。IO的各种流是阻塞的，当一个线程调用read()或write()时，该线程被阻塞。NIO的非阻塞模式。IO中的Stream是单向的，Channel是双向的。Channe既可以用来进行读操作，又可以用来进行写操作。

## 序列化与反序列化

### 简介

序列化是指将实现Serializable接口和Externalizable接口的对象转换成字节序列，并能通过该字节序列完全恢复原来的对象，序列化可以弥补不同操作系统之间的差异。只有实现了Serializble接口和Externalizable接口的对象才能被序列化，Externalizable接口继承自Serializable接口，实现Externalizable接口的类完全由自身控制序列化的行为，而仅实现Serializable接口的类可以采用默认的序列化方式。

### 序列化与反序列化步骤

1. 创建对象的输出(入)流，可以包装一个其他类型的目标输出流，如文件输出流(JDK java.io.Object.OutputStream)(JDK java.io.Object.InputStream)；
2. 通过对象输出(入)流的writeObject(Object obj)(readObject(Object obj))方法写对象。

### 序列化注意事项

1. static修饰的熟悉、方法不会被序列化；
2. 通过网络、文件进行序列化时，必须按照写入的顺序读取对象；
3. 反序列化时必须有序列化对象的class文件；
4. serializabeID要声明，从而避免由于不同的JVM而导致反序列化失败；
5. 序列化协议：protobuf占用空间小、反序列化快，可读性差，所以应用于跨网络之间数据通讯比较合适；json适用于局域网和同机器之间通信；xml适用于数据存储；
6. 序列化时，A中有B的引用，A序列化时，会将B一并地序列化；若B不能被序列化，则需要transient关键字来修饰，表示该变量不会被序列化。 

# 网络编程

## 简介

​		网络编程主要运用java.net包以及java.io包，编写运行在多个设备的程序，这些设备通过网络连接起来。

## TCP/IP协议

​		IP协议：准确定位到网络的一台主机，所以网络层负责网络主机的定位，数据传输路由，由IP确定网络中的唯一一台主机。

​		TCP协议：进行可靠高效的数据传输，所以传输层负责面向应用的可靠的或非可靠的数据传输机制，这是网络编程的主要对象。

## TCP/UDP

​		TCP是数据传输控制协议，是一种面向连接的保证可靠传输的传输层协议，可以得到一个顺序的无差错的数据流，必须建立连接。

​		UDP是用户数据报协议，是一种面向无向连接的传输层协议，每个数据报都是独立的信息，包括完整的源地址或目的地址，在网络上以任何可能的路径传往目的地，到达目的地的时间及准确性是无法保证。

**【区别】：**

| 区别     | TCP                                            | UDP                                                    |
| -------- | ---------------------------------------------- | ------------------------------------------------------ |
| 有无连接 | 面向连接，三次握手，四次挥手                   | 面向无连接，每个数据保证都有完整的地址信息             |
| 通讯方式 | 需要连接，点对点通讯                           | 不需要连接，可以实现广播发送                           |
| 数据大写 | 无大小限制                                     | 有大小限制，每个数据报限定在64KB之内                   |
| 可靠性   | 可靠，能保证接收方完全正确顺序获取发送方的数据 | 不可靠协议，发送方的数据报不一定以相同的次序到达接收方 |
| 传输效率 | 基于可靠传输效率低于UDP                        |                                                        |
| 应用场景 | TCP适用于远程连接和文件传输                    | UDP适用于视频会议系统(保持连贯性)                      |

## Socket编程

1）Socket为“套接字”，即IP地址+端口；

2）Socket通讯过程：

​		1、服务器程序将一个套接字绑定到一个特定的端口，并通过此套接字等待和监听某个端口是否有连接请求(ServerSocket类的accept()方法)；

​		2、客户端向服务端发送连接请求；

​		3、服务端收到连接请求向客户端发出接收消息，这样一个连接就建立起来了，客户端和服务端都可以相互发送消息与对方进行通讯；

​		4、关闭Socket。

3）Socket的基本工作：

​		1、创建Socket;

​		2、打开连接到Socket的输入输出流；

​		3、按照一定的协议对Socket进行读写操作；

​		4、关闭Socket。

4）客户机/服务器C/S结构：

​		通讯双方一方作为服务器等待客户端提出请求并予以响应。客户端则在需要服务时向服务器提出申请，服务器始终运行，监听网络端口，一旦有客户端请求，就会启动一个服务端线程来响应客户端，同时自己继续监听服务端口，使后来的服务端也能及时的得到服务。

## 网络模型

### OSI七层参考模型

![](F:\Document\note\jimages\OSI7.png)

### TCP/IP模型

<img src="F:\Document\note\jimages\TCP-IP.png" alt="image-20200511142012478" style="zoom: 80%;" />

### OSI7与TCP/IP差异

![image-20200511135605348](F:\Document\note\jimages\osi.png)

### TCP三次握手

TCP的状态标识：

| 标识 | SYN      | ACK  | FIN      | PSH        | RST      | URG  |
| ---- | -------- | ---- | -------- | ---------- | -------- | ---- |
| 含义 | 建立连接 | 响应 | 关闭连接 | 有数据传输 | 连接重置 |      |

TCP的连接建立是一个三次握手的过程，目的是为了通讯双方确认开始序号，以便于后续通讯的有序进行：

1. 连接请求

   连接开始时，连接建立方(Client)发送SYN包，并包含了自己的初始序列号a;

2. 请求确认

   连接接受方(Server)收到SYN包后会回复一个SYN包，其中包含了对上一个a包的回应信息ACK，回应的序列号为下一个希望收到包的序号，即a+1，然后还包含了自己初始的序号b；

3. 连接确认

   连接建立方(Client)收到回应的SYN包以后，回复一个ACK包做响应，其中包含了下一个希望接收到的包的序号即b+1。

![image-20200511142258482](F:\Document\note\jimages\tcp_connect.png)



### TCP四次挥手

TCP终止连接的四次握手过程如下：

1. 首先进行关闭的一方(即发送第一个FIN)将执行主动关闭，而另一方(收到这个FIN)执行被动关闭。
2. 当服务器收到这个FIN，它发回一个ACK，确认序号为收到的序号+1。与SYN一样，一个FIN将占用一个序号。
3. 同时TCP服务器还向应用程序(即丢弃服务器)传送一个文件结束符。接着这个服务器程序就关闭它的连接，导致它的TCP端发送一个FIN。
4. 客户端(Clinet)客户必须发回一个确认，并将确认序号设置为收到序号+1。

![image-20200511142333474](F:\Document\note\jimages\tcp_disconnect.png)

# 多线程

## 并发与并行、线程与进程

并发：指两个或多个事件在同一时间段内发生(交替进行)。

并行：指两个或多个时间在同一时刻发生(同时发生)。

线程：指进程的一个执行单元，负责当前进程中程序的执行，一个进程中至少有一个线程。

进程：指一个内存中运行的应用程序，每个进程都有独立的内存空间，一个应用程序可以同时运行多个进程。

## 线程调度

### 分时调度

所有线程轮流拥有CPU的使用权，轮流使用CPU，平均分配每个线程占用CPU的时间。

### 抢占式调度

优先级高的线程优先使用CPU，如果线程优先级相同，那么随机选择一个，Java使用的为抢占式调度。

## 多线程

### 主线程

执行主(main)方法的线程

### 实现多线程

#### 创建Thread类的子类：

1. 创建一个Thread类的子类。

2. 在Thread类的子类中重写Thread类中的run方法，设置线程开始任务。

3. 创建Thread类的子类对象。

4. 调用Thread类中的start方法，开启新的线程，执行run方法。

   void start() 使该线程开始执行；Java虚拟机调用该线程的run方法。

```java
public class ThreadDemo {

    public static void main(String[] args) {
        /**
         * 2.创建Thread子类对象
         */
        MyThread myThread = new MyThread();
        /**
         * 3.调用Thread类中的start()方法,执行run方法
         */
        myThread.start();
        for (int i = 0; i < 20; i++) {
            System.out.println("main:" + i);
        }
    }
}
```

```java
public class MyThread extends Thread {
    /**
     * 1.继承Thread类，重写run方法
     */
    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            System.out.println("run:" + i);
        }
    }
}
```

![image-20200507155552008](F:\Document\note\jimages\mutilThread.png)

#### 实现Runnable接口

步骤：

1. 创建一个Runnable接口的实现类。

2. 在实现类中重写Runnable接口的run方法，设置线程任务。

3. 创建一个Runnable接口的实现类对象。

4. 创建Thread类对象，构造方法中传递Runnable接口的实现类对象。

5. 调用Thread的start()方法，执行线程。

   

   ```java
   public class RunnableImpl implements Runnable {
       /**
        * 1.创建Runnable接口的实现类
        * 2.重写Runnable接口中的run方法
        */
       @Override
       public void run() {
           for (int i = 0; i <20 ; i++) {
               System.out.println(Thread.currentThread().getName()+"-->"+i);
           }
       }
   }
   ```

   ```java
   public class ThreadDemo {
   
       public static void main(String[] args) {
           /**
            * 2.创建Thread子类对象
            */
           MyThread myThread = new MyThread();
           /**
            * 3.调用Thread类中的start()方法,执行run方法
            */
           myThread.start();
           for (int i = 0; i < 20; i++) {
               System.out.println("main:" + i);
           }
           /**
            * 匿名内部类实现接口
            */
           new Thread(){
               @Override
               public  void run(){
                   for (int i = 0; i < 20; i++) {
                       System.out.println("匿名内部类实现接口:" + i);
                   }
               }
           }.start();
           /**
            * 多态匿名内部类创建线程
            */
          Runnable r= new Runnable(){
             @Override
             public  void  run(){
                 for (int i = 0; i < 20; i++) {
                     System.out.println("多态创建接口:" + i);
                 }
             }
           };
          new Thread(r).start();
       }
   }
   ```

### 两种实现多线程方式的区别

实现Runnable接口创建多线程：避免了单继承的局限性(类为单继承，接口可多实现)；增强了程序的扩展性，降低了程序的耦合性，实现Runnable接口的方式，把设置线程任务和开启新的线程进行了解耦。

## 同步与异步

### 同步

指本线程所有操作都已经处理完成，其他线程才可以使用该资源，如果未处理完成则继续等待。

### 异步

本线程还在使用该资源，其他线程就可以使用该资源，不用等待。

### 解决同步安全问题

同步方法(synchronized)、静态同步方法(static synchronized)与Lock锁。

```java
public synchronized void save(){
    /**
    * Java的每个对象都会有一个内置锁，当使用synchronized修饰方法时，	 * 内置锁会保护整个方法。在使用该方法前，需要获得内置锁，否则就处于		* 阻塞状态。
    * 【注】synchronized关键字可以修饰静态方法，如果执行时调用该静态		* 方法，将会锁住整个类。
    */
}
```

```java
synchronized(object){
    /**
	* 被synchroized修饰的语句块会加上内置锁，从而实现同步。
	* 【注】同步是一种高开小操作，因此应尽量减少同步的使用，通常没必要	  *	同步整个方法，使用synchronized代码块同步关键代码即可。
	*/
}
```



## 线程状态及转换

![image-20200508153922612](F:\Document\note\jimages\Thread-status.png)

- New

  创建后尚未启动

- Runnable

  可能正在运行，可能正在等待CPU时间片。包含了操作系统线程状态中的Running和Ready。

- Blocked

- Time Waiting

- Waiting

- Terminated








