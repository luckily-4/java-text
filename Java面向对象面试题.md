# Java面向对象面试题（一）

##### 1.String类可以被继承吗？

因为Stirng类在声明时被final关键词修饰，而被final修饰的类无法被继承

源码如下：

```java
public final class String implements Serializable, Comparable<String>, CharSequence {
    @Stable
    private final byte[] value;
    private final byte coder;
    private int hash;
    private static final long serialVersionUID = -6849794470754667710L;
    static final boolean COMPACT_STRINGS = true;
    private static final ObjectStreamField[] serialPersistentFields = new ObjectStreamField[0];
    public static final Comparator<String> CASE_INSENSITIVE_ORDER = new String.CaseInsensitiveComparator();
    static final byte LATIN1 = 0;
    static final byte UTF16 = 1;

```

##### 2.为什么把String类定义为final的呢？

1. 有当字符串是不可变的，字符串池才有可能实现,字符串池的实现可以在运行时节约很多heap空间
2. 如果字符串是可变的，那么String interning将不能实现
3. 如果字符串是可变的，那么会引起很严重的安全问题。譬如，数据库的用户名、密码都是以字符串的形式传入来获得数据库的连接，或者在socket编程中，主机名和端口都是以字符串的形式传入.

##### 3.final关键字的用法呢？

1. final修饰的变量，一旦赋值，不可重新赋值；
2. final修饰的方法无法被覆盖；
3. final修饰的实例变量，必须手动赋值，不能采用系统默认值；
4. final修饰的实例变量，一般和static联用，用来声明常量；
5. 注意：final不能和abstract关键字联合使用。总之，final表示最终的、不可变的。

##### 4.两个对象值相同equals结果为true，但却可有不同的 hashCode，这句话对不对？

​    不对，如果两个对象x和y满足x.equals(y) == true，它们的哈希值（hashCode）应当相      同。Java 对于equals方法和hashCode方法是这样规定的：

（1）如果两个对象相同（equals方法返回true），那么它们的hashCode值一定要相同；

（2）如果两个对象的 hashCode相同，它们并不一定相同。

##### 5.在 Java 中，如何跳出当前的多重嵌套循环？

在最外层循环前加一个标记如outfor，然后用break outfor;可以跳出多重循环。例如以下代码：

```java
public class TestBreak {
    public static void main(String[] args) {
        outfor: for (int i = 0; i < 10; i++){
            for (int j = 0; j < 10; j++){
                if (j == 5){
                    break outfor;
                }
                System.out.println("j = " + j);
            }
        }
    }
}
```

##### 6.重载（overload）和重写（override）的区别？重载的方法能否根据返回类型进行区分？

重载：实现的是编译时的多态性，重载发生在一个类中，同名的方法如果有不同的参数列表（类型不同、个数不同、顺序不同）则视为重载。

重写：实现的运行时的多态性.重写发生在子类与父类之间，重写要求子类重写之后的方法与父类被重写方法有相同的返回类型，比父类被重写方法更好访问，不能比父类被重写方法声明更多的异常（里氏代换原则）。

**● 方法重载的规则：**

方法名一致，参数列表中参数的顺序，类型，个数不同。

重载与方法的返回值无关，存在于父类和子类，同类中。

可以抛出不同的异常，可以有不同修饰符。

**● 方法重写的规则：**

参数列表、方法名、返回值类型必须完全一致；

构造方法不能被重写；

声明为 final 的方法不能被重写；

声明为 static 的方法不存在重写（重写和多态联合才有意义）；

访问权限不能比父类更低；

重写之后的方法不能抛出更宽泛的异常；

##### 7.当一个对象被当作参数传递到一个方法后，此方法可改变这个对象的属性，并可返回变化后的结果，那么这里是值传递还是引用传递?

是值传递。Java 语言的方法调用只支持参数的值传递。当一个对象实例作为一个参数被传递到方法中时，参数的值就是对该对象的内存地址。这个值（内存地址）被传递后，同一个内存地址指向堆内存当中的同一个对象，所以通过哪个引用去操作这个对象，对象的属性都是改变的。

##### 8.为什么方法不能根据返回类型来区分重载？

在Java语言中，调用一个方法，即使这个方法有返回值，我们也可以不接收这个返回值，Java编译器无法区分调用的具体是哪个方法。所以对于编译器来说是重复了，编译器报错。所以区分这两个方法不能依靠方法的返回值类型。

##### **9、抽象类(abstract class)和接口(interface)有什么异同？**

不同点：

● 抽象类中可以定义构造器，接口不能；

● 抽象类可以有抽象方法和具体方法，接口不能有具体方法；

● 接口中的成员全都是 public 的，抽象类中的成员可以使用private、public、protected、默认等修饰；

● 抽象类中可以定义成员变量，接口中只能是常量；

● 有抽象方法的类必须被声明为抽象类，而抽象类未必要有抽象方法；

● 抽象类中可以包含静态方法，接口中不能有静态方法；

● 一个类只能继承一个抽象类，一个类可以实现多个接口；

相同点：

● 不能够实例化；

● 可以将抽象类和接口类型作为引用类型；

● 一个类如果继承了某个抽象类或者实现了某个接口都需要对其中的抽象方法全部进行实现，否则该类仍然需要被声明为抽象类；

##### 10.char 型变量中能不能存储一个中文汉字，为什么？

char 类型可以存储一个中文汉字，因为Java中使用的编码是Unicode（不选择任何特定的编码，直接使用字符在字符集中的编号，这是统一的唯一方法），一个char 类型占2个字节（16 比特），所以放一个中文是没问题的。

补充：使用Unicode 意味着字符在JVM内部和外部有不同的表现形式，在JVM内部都是 Unicode，当这个字符被从JVM内部转移到外部时（例如存入文件系统中），需要进行编码转换。所以 Java 中有字节流和字符流，以及在字符流和字节流之间进行转换的转换流，如 InputStreamReader和OutputStreamRead

##### 11.、抽象的(abstract)方法是否可同时是静态的(static)， 是否可同时是本地方法(native)，是否可同时被 synchronized？

都不能。

● 抽象方法需要子类重写，而静态的方法是无法被重写的，因此二者是矛盾的。

● 本地方法是由本地代码（如 C++ 代码）实现的方法，而抽象方法是没有实现的，也是矛盾的。

● synchronized 和方法的实现细节有关，抽象方法不涉及实现细节，因此也是相互矛盾的。

##### 12、==和equals的区别？

equals和==最大的区别是一个是方法一个是运算符。

● ==：如果比较的对象是基本数据类型，则比较的是数值是否相等；如果比较的是引用数据类型，则比较的是对象的地址值是否相等。

● equals()：用来比较方法两个对象的内容是否相等。equals方法不能用于基本数据类型的变量，如果没有对equals方法进行重写，则比较的是引用类型的变量所指向的对象的地址。

##### 13、阐述静态变量和实例变量的区别？

不管创建多少个对象，静态变量在内存中有且仅有一个；实例变量必须依存于某一实例，需要先创建对象然后通过对象才能访问到它。静态变量可以实现让多个对象共享内存。

##### 14、break和continue的区别？

● break和continue 都是用来控制循环的语句。

● break 用于完全结束一个循环，跳出循环体执行循环后面的语句。

continue 用于跳过本次循环，继续下次循环。

##### 15、String s = "Hello";s = s + " world!";这两行代码执行后，原始的 String 对象中的内容变了没有？

没有。因为 String被设计成不可变类，所以它的所有对象都是不可变对象。

对于字符串常量，如果内容相同，Java 认为它们代表同一个 String 对象。而用关键字 new 调用构造器，总是会创建一个新的对象，无论内容是否相同。

# Java面向对象面试题（二）

##### 1、面向对象包括哪些特性，怎么理解的？

（1）封装：通常认为封装是把数据和操作数据的方法绑定起来，对数据的访问只能通过已定义的接口。面向对象的本质就是将现实世界描绘成一系列完全自治、封闭的对象。我们在类中编写的方法就是对实现细节的一种封装；我们编写一个类就是对数据和数据操作的封装。可以说，封装就是隐藏一切可隐藏的东西，只向外界提供最简单的编程接口。

（2）继承：继承是从已有类得到继承信息创建新类的过程。提供继承信息的类被称为父类（超类、基类）；得到继承信息的类被称为子类（派生类）。继承让变化中的软件系统有了一定的延续性，同时继承也是封装程序中可变因素的重要手段。

（3）多态：多态性是指允许不同子类型的对象对同一消息作出不同的响应。简单的说就是用同样的对象引用调用同样的方法但是做了不同的事情。多态性分为编译时的多态性和运行时的多态性。如果将对象的方法视为对象向外界提供的服务，那么运行时的多态性可以解释为：当 A系统访问B系统提供的服务时，B 系统有多种提供服务的方式，但一切对 A 系统来说都是透明的。方法重载（overload）实现的是编译时的多态性（也称为前绑定），而方法重写（override）实现的是运行时的多态性（也称为后绑定）。运行时的多态是面向对象最精髓的东西，要实现多态需要做两件事：

第一：方法重写（子类继承父类并重写父类中已有的或抽象的方法）；

第二：对象造型（用父类型引用指向子类型对象，这样同样的引用调用同样的方法就会根据子类对象的不同而表现出不同的行为）。

（4）抽象：抽象是将一类对象的共同特征总结出来构造类的过程，包括数据抽象和行为抽象两方面。抽象只关注对象有哪些属性和行为，并不关注这些行为的细节是什么。

##### 2、访问权限修饰符 public、private、protected， 以及不写（默认）时的区别？

| 修饰符       | 当前类 | 同包 | 子类 | 其他包 |
| ------------ | ------ | ---- | ---- | ------ |
| public       | √      | √    | √    | √      |
| protected    | √      | √    | √    | ×      |
| 默认（缺省） | √      | √    | ×    | ×      |
| private      | √      | ×    | ×    | ×      |

##### 3、Java中为什么要用 clone？

在实际编程过程中，我们常常要遇到这种情况：有一个对象 A，在某一时刻 A 中已经包含了一些有效值，此时可能会需要一个和 A 完全相同新对象 B，并且此后对 B 任何改动都不会影响到 A 中的值，也就是说，A 与 B 是两个独立的对象，但 B 的初始值是由 A 对象确定的。在 Java 语言中，用简单的赋值语句是不能满足这种需求的。要满足这种需求虽然有很多途径，但clone()方法是其中最简单，也是最高效的手段。

**● 说到对象的克隆，涉及到深克隆和浅克隆？**

浅克隆：创建一个新对象，新对象的属性和原来对象完全相同，对于非基本类型属性，仍指向原有属性所指向的对象的内存地址。

深克隆：创建一个新对象，属性中引用的其他对象也会被克隆，不再指向原有对象地址。

##### 4、new一个对象的过程和clone一个对象的区别？

new 操作符的本意是分配内存。程序执行到 new 操作符时，首先去看 new 操作符后面的类型，因为知道了类型，才能知道要分配多大的内存空间。分配完内存之后，再调用构造函数，填充对象的各个域，这一步叫做对象的初始化，构造方法返回后，一个对象创建完毕，可以把他的引用（地址）发布到外部，在外部就可以使用这个引用操纵这个对象。

clone 在第一步是和 new 相似的，都是分配内存，调用 clone 方法时，分配的内存和原对象（即调用 clone 方法的对象）相同，然后再使用原对象中对应的各个域，填充新对象的域，填充完成之后，clone方法返回，一个新的相同的对象被创建，同样可以把这个新对象的引用发布到外部。

##### 5、Java中实现多态的机制是什么？

Java中的多态靠的是父类或接口定义的引用变量可以指向子类或具体实现类的实例对象，而程序调用的方法在运行期才动态绑定，就是引用变量所指向的具体实例对象的方法，也就是内存里正在运行的那个对象的方法，而不是引用变量的类型中定义的方法。

##### 6、谈谈你对多态的理解？

多态就是指程序中定义的引用变量所指向的具体类型和通过该引用变量发出的方法调用在编程时并不确定，而是在程序运行期间才确定，即一个引用变量到底会指向哪个类的实例对象，该引用变量发出的方法调用到底是哪个类中实现的方法，必须在程序运行期间才能决定。因为在程序运行时才确定具体的类，这样，不用修改源代码，就可以让引用变量绑定到各种不同的对象上，从而导致该引用调用的具体方法随之改变，即不修改程序代码就可以改变程序运行时所绑定的具体代码，让程序可以选择多个运行状态，这就是多态性。

##### 7、谈谈你对面向对象的理解？

所谓对象就是由一组数据结构和处理它们的方法组成的，重点“数据”包括对象的特性、状态等的静态信息；“方法” 也就是行为，包括该对象的对数据的操作、功能等能动信息。把相同行为的对象归纳为类，类是一个抽象的概念，对象是类的具体。简单点说：对象就是类的实例。例如：小品演员就是一个类，赵本山就是一个对象。

面向对象的目的：解决软件系统的可扩展性，可维护性和可重用性。

**● 面向对象的三大特性：封装、多态和继承：**

（1）封装（对应可扩展性）：隐藏对象的属性和实现细节，仅对外公开接口，控制在程序中属性的读和修改的访问级别。封装是通过访问控制符（public protected private）来实现。一个类就可看成一个封装。

（2）继承（重用性和扩展性）：子类继承父类，可以继承父类的方法和属性。可以对父类方向进行覆盖（实现了多态）。但是继承破坏了封装，因为他是对子类开放的，修改父类会导致所有子类的改变，因此继承一定程度上又破坏了系统的可扩展性，只有明确的IS-A关系才能使用。继承要慎用，尽量优先使用组合。

（3）多态（可维护性和可扩展性）：接口的不同实现方式即为多态。接口是对行为的抽象，刚才在封装提到，找到变化部分并封装起来，但是封装起来后，怎么适应接下来的变化？这正是接口的作用，接口的主要目的是为不相关的类提供通用的处理服务，我们可以想象一下。比如鸟会飞，但是超人也会飞，通过飞这个接口，我们可以让鸟和超人，都实现这个接口。

面向对象编程（OOP）其实就是一种设计思想，在程序设计过程中把每一部分都尽量当成一个对象来考虑，以实现软件系统的可扩展性，可维护性和可重用性。

# Java面向对象面试题（三）

##### 1、final、finally、finalize 的区别？

● final：用于声明属性，方法和类，分别表示属性不可变，方法不可覆盖，被其修饰的类不可继承。

● finally：异常处理语句结构的一部分，表示总是执行。

● finalize：Object 类的一个方法，所以Java对象都有这个方法，当某Java对象没有更多的引用指向的时候，会被垃圾回收器回收，该对象被回收之前，由垃圾回收器来负责调用此方法，通常在该方法中进行回收前的准备工作。该方法更像是一个对象生命周期的临终方法，当该方法被系统调用则代表该对象即将“死亡”，但是需要注意的是，我们主动行为上去调用该方法并不会导致该对象“死亡”，这是一个被动的方法（其实就是回调方法），不需要我们调用。

##### 2、Java中异常分为哪些种类？

按照异常需要处理的时机分为编译时异常(也叫受控异常)也叫 CheckedException 和运行时异常(也叫非受控异常)也叫 UnCheckedException。Java认为Checked异常都是可以被处理的异常，所以Java程序必须显式处理Checked异常。如果程序没有处理Checked 异常，该程序在编译时就会发生错误无法编译。这体现了Java 的设计哲学：没有完善错误处理的代码根本没有机会被执行。对Checked异常处理方法有两种：

● 第一种：当前方法知道如何处理该异常，则用try...catch块来处理该异常。

● 第二种：当前方法不知道如何处理，则在定义该方法时声明抛出该异常。

运行时异常只有当代码在运行时才发行的异常，编译的时候不需要try…catch。Runtime如除数是0和数组下标越界等，其产生频繁，处理麻烦，若显示申明或者捕获将会对程序的可读性和运行效率影响很大。所以由系统自动检测并将它们交给缺省的异常处理程序。当然如果你有处理要求也可以显示捕获它们。

##### 3、error和exception的区别？

Error类和Exception类的父类都是Throwable类，他们的区别如下：

● Error类一般是指与虚拟机相关的问题，如系统崩溃，虚拟机错误，内存空间不足，方法调用栈溢出等。对于这类错误的导致的应用程序中断，仅靠程序本身无法恢复和预防，遇到这样的错误，建议让程序终止。

● Exception类表示程序可以处理的异常，可以捕获且可能恢复。遇到这类异常，应该尽可能处理异常，使程序恢复运行，而不应该随意终止异常。

●Exception类又分为未检查异常（UnCheckedException）和受检查的异常(CheckedException)。运行时异常ArithmeticException，IllegalArgumentException编译能通过，但是一运行就终止了，程序不会处理运行时异常，出现这类异常，程序会终止。而受检查的异常，要么用 try…catch 捕获，要么用throws字句声明抛出，交给它的父类处理，否则编译不会通过。

##### 4、调用下面的方法，得到的返回值是什么？

```java
1. public int getNum() {
2.     try {
3.         int a = 1 / 0;
4.         return 1;
5.     } catch (Exception e) {
6.         return 2;
7.     } finally {
8.         return 3;
9.     }
10.}
```

代码走到第3行的时候遇到了一个MathException，这时第4行的代码就不会执行了，代码直接跳转到catch语句中，走到第 6 行的时候，异常机制有一个原则：如果在catch中遇到了return或者异常等能使该函数终止的话那么有finally就必须先执行完finally代码块里面的代码然后再返回值。因此代码又跳到第8行，可惜第8行是一个return语句，那么这个时候方法就结束了，因此第6行的返回结果就无法被真正返回。如果finally仅仅是处理了一个释放资源的操作，那么该道题最终返回的结果就是2。因此上面返回值是3。

##### 5、Java 异常处理机制的理解？

Java对异常进行了分类，不同类型的异常分别用不同的Java类表示，所有异常的根类为 java.lang.Throwable，Throwable下面又派生了两个子类：Error和Exception。

Error表示应用程序本身无法克服和恢复的一种严重问题。

Exception表示程序还能够克服和恢复的问题，其中又分为系统异常和普通异常。

系统异常是软件本身缺陷所导致的问题，也就是软件开发人员考虑不周所导致的问题，软件使用者无法克服和恢复这种问题，但在这种问题下还可以让软件系统继续运行或者让软件死掉，例如，数组下标越界（ArrayIndexOutOfBoundsException），空指针异常（NullPointerException）、类转换异常（ClassCastException）。

普通异常是运行环境的变化或异常所导致的问题，是用户能够克服的问题，例如，网络断线，硬盘空间不够，发生这样的异常后，程序不应该死掉。

Java为系统异常和普通异常提供了不同的解决方案，编译器强制普通异常必须try..catch处理或用throws声明继续抛给上层调用方法处理，所以普通异常也称为checked异常，而系统异常可以处理也可以不处理，所以编译器不强制用try..catch处理或用throws声明，所以系统异常也称为unchecked异常。

##### 6、说出最常见的5个RuntimeException？

● *java.lang.NullPointerException* 空指针异常；出现原因：调用了未经初始化的对象或者是不存在的对象。

● *java.lang.ClassNotFoundException* 指定的类找不到；出现原因：类的名称和路径加载错误；通常都是程序试图通过字符串来加载某个类时可能引发异常。

● *java.lang.NumberFormatException* 字符串转换为数字异常；出现原因：字符型数据中包含非数字型字符。

● *java.lang.IndexOutOfBoundsException* 数组角标越界异常，常见于操作数组对象时发生。

● *java.lang.IllegalArgumentException* 方法传递参数错误。

● *java.lang.ClassCastException* 数据类型转换异常。

● *java.lang.NoClassDefFoundException* 未找到类定义错误。

● *SQLException SQL 异常*，常见于操作数据库时的 SQL 语句错误。

● *java.lang.InstantiationException* 实例化异常。

● *java.lang.NoSuchMethodException* 方法不存在异常。

##### 7、throw 和 throws 的区别？

● throw：

throw 语句用在方法体内，表示抛出异常，由方法体内的语句处理。

throw是具体向外抛出异常的动作，所以它抛出的是一个异常实例，执行throw一定是抛出了某种异常。

● throws：

throws语句是用在方法声明后面，表示如果抛出异常，由该方法的调用者来进行异常的处理。

throws主要是声明这个方法会抛出某种类型的异常，让它的使用者要知道需要捕获的异常的类型。

● throws表示出现异常的一种可能性，并不一定会发生这种异常。

# Java常用API面试题（四）

**1、Math.round(11.5)等于多少？Math.round(- 11.5) 又等于多少?**

Math.round(11.5)的返回值是12，Math.round(-11.5)的返回值是-11。四舍五入的原理是在参数上加0.5然后进行取整。

##### 2、switch是否能作用在byte 上，是否能作用在long上，是否能作用在String上?

Java5以前switch(expression)中，expression只能是byte、short、char、int，严格意义上来讲Java5以前只支持int，之所以能使用byte short char是因为存在自动类型转换。从 Java 5 开始，Java中引入了枚举类型，expression也可以是 enum 类型。从 Java 7 开始，expression还可以是字符串（String），但是长整型（long）在目前所有的版本中都是不可以的。

##### 3、数组有没有length()方法？String有没有length()方法？

数组没有length()方法，而是有length属性。String有length()方法。JavaScript 中，获得字符串的长度是通过length属性得到的，这一点容易和Java混淆。

##### 4、String 、StringBuilder 、StringBuffer 的区别？

Java平台提供了两种类型的字符串：String 和 StringBuffer/StringBuilder，它们都可以储存和操作字符串，区别如下：

● String 是只读字符串，也就意味着 String 引用的字符串内容是不能被改变的。初学者可能会有这样的误解：

String str = “abc”；

str = “bcd”;

如上，字符串 str 明明是可以改变的呀！其实不然，str 仅仅是一个引用对象，它指向一个字符串对象“abc”。第二行代码的含义是让 str 重新指向了一个新的字符串“bcd”对象，而“abc”对象并没有任何改变，只不过对象”abc”已经没有引用指向它了。

● StringBuffer/StringBuilder 表示的字符串对象可以直接进行修改。

● StringBuilder是Java5中引入的，它和StringBuffer的方法完全相同，区别在于它是在单线程环境下使用的，因为它的所有方法都没有被 synchronized 修饰，因此它的效率理论上也比 StringBuffer要高。

##### 5、请说出下面程序的输出？

```java
class StringEqualTest {
    public static void main(String[] args) {
        String s1 = "Programming";
        String s2 = new String("Programming");
        String s3 = "Program";
        String s4 = "ming";
        String s5 = "Program" + "ming";
        String s6 = s3 + s4;
        System.out.println(s1 == s2);    //false
        System.out.println(s1 == s5);    //true
        System.out.println(s1 == s6);    //false
        System.out.println(s1 == s6.intern());    //true
        System.out.println(s2 == s2.intern());    //false
    }
}
```

补充：解答上面的面试题需要知道如下两个知识点：

● String 对象的 intern()方法会得到字符串对象在常量池中对应的版本的引用（如果常量池中有一个字符串与String 对象的 equals 结果是 true），如果常量池中没有对应的字符串，则该字符串将被添加到常量池中，然后返回常量池中字符串的引用；

● 字符串的+操作其本质是创建了 StringBuilder 对象进行 append 操作，然后将拼接后的  StringBuilder  对象用 toString 方法处理成 String 对象，这一点可以用 javap -c StringEqualTest.class 命令获得 class 文件对应的 JVM 字节码指令就可以看出来。

##### 6、如何取得年月日、小时分钟秒？

```java
import java.time.LocalDateTime;
import java.util.Calendar;

class DateTimeTest {
    public static void main(String[] args) {
        Calendar cal = Calendar.getInstance();
        System.out.println(cal.get(Calendar.YEAR));
        System.out.println(cal.get(Calendar.MONTH)); // 0 - 11
        System.out.println(cal.get(Calendar.DATE));
        System.out.println(cal.get(Calendar.HOUR_OF_DAY));
        System.out.println(cal.get(Calendar.MINUTE));
        System.out.println(cal.get(Calendar.SECOND));
        // Java 8
        LocalDateTime dt = LocalDateTime.now();
        System.out.println(dt.getYear());
        System.out.println(dt.getMonthValue()); // 1 - 12
        System.out.println(dt.getDayOfMonth());
        System.out.println(dt.getHour());
        System.out.println(dt.getMinute());
        System.out.println(dt.getSecond());
    }
}
```

##### 7、如何取得从 1970 年 1 月 1 日 0 时 0 分 0 秒到现在的毫秒数？

```java
class GetTime {
    public static void main(String[] args) {
        System.out.println("第一种：" + Calendar.getInstance().getTimeInMillis());
        System.out.println("第二种：" + System.currentTimeMillis());
        System.out.println("第三种：" + Clock.systemDefaultZone().millis());
    }
}
```

**8、如何取得某月的最后一天？**

```java
class GetLastDay {
    public static void main(String[] args) {
        SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd");
        //获取当前月第一天：
        Calendar c = Calendar.getInstance();
        c.add(Calendar.MONTH, 0);
        c.set(Calendar.DAY_OF_MONTH, 1);//设置为 1 号，当前日期既为本月第一天
        String first = format.format(c.getTime());
        System.out.println("first:" + first);
        //获取当前月最后一天
        Calendar ca = Calendar.getInstance();
        ca.set(Calendar.DAY_OF_MONTH, ca.getActualMaximum(Calendar.DAY_OF_MONTH));
        String last = format.format(ca.getTime());
        System.out.println("last:" + last);
        //Java 8
        LocalDate today = LocalDate.now();
        //本月的第一天
        LocalDate firstday = LocalDate.of(today.getYear(), today.getMonth(), 1);
        //本月的最后一天
        LocalDate lastDay = today.with(TemporalAdjusters.lastDayOfMonth());
        System.out.println("本月的第一天" + firstday);
        System.out.println("本月的最后一天" + lastDay);

    }
}
```

运行结果：

first:2019-04-01

last:2019-04-30

本月的第一天2019-04-01

本月的最后一天2019-04-30

##### 9、如何格式化日期？

java.text.DataFormat的子类（如 SimpleDateFormat 类）中的 format(Date)方法可将日期格式化。Java 8 中可以用 java.time.format.DateTimeFormatter 来格式化时间日期，代码如下所示：

```java
import java.text.SimpleDateFormat;
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.util.Date;

class DateFormatTest {
    public static void main(String[] args) {
        SimpleDateFormat oldFormatter = new SimpleDateFormat("yyyy/MM/dd");
        Date date1 = new Date();
        System.out.println(oldFormatter.format(date1));
        // Java 8
        DateTimeFormatter newFormatter = DateTimeFormatter.ofPattern("yyyy/MM/dd");
        LocalDate date2 = LocalDate.now();
        System.out.println(date2.format(newFormatter));
    }
}
```

运行结果：

2019/04/22

2019/04/22

补充：Java 的时间日期 API 一直以来都是被诟病的东西，为了解决这一问题，Java 8 中引入了新的时间日期 API， 其中包括 LocalDate、LocalTime、LocalDateTime、Clock、Instant 等类，这些的类的设计都使用了不变模式，因此是线程安全的设计。

##### 10、打印昨天的当前时刻？

```java
import java.util.Calendar;

class YesterdayCurrent {
    public static void main(String[] args) {
        Calendar cal = Calendar.getInstance();
        cal.add(Calendar.DATE, -1);
        System.out.println(cal.getTime());
    }
}

//java-8
import java.time.LocalDateTime;

class YesterdayCurrent {
    public static void main(String[] args) {
        LocalDateTime today = LocalDateTime.now();
        LocalDateTime yesterday = today.minusDays(1);
        System.out.println(yesterday);
    }
}
```

**11、JSR310 规范 Joda-Time 的区别？**

其实 JSR310 的规范领导者 Stephen Colebourne，同时也是 Joda-Time 的创建者，JSR310 是在 Joda-Time 的基础上建立的，参考了绝大部分的 API，但并不是说 JSR310=JODA-Time，下面几个比较明显的区别是：

● 最明显的变化就是包名（从 org.joda.time 以及 java.time）

● JSR310 不接受 NULL 值，Joda-Time 视 NULL 值为 0

● JSR310 的计算机相关的时间（Instant）和与人类相关的时间（DateTime）之间的差别变得更明显

● JSR310 所有抛出的异常都是 DateTimeException 的子类。虽然 DateTimeException 是一个RuntimeException。

# Java数据类型面试题（五）

1、Java 的基本数据类型都有哪些各占几个字节？

按照口诀记忆：

● 数据类型：byte short int long float double boolean char

● 占用字节数：12484812（byte对应1，short对应2，以此类推）

##### 2、String 是基本数据类型吗?

通过JDK源代码可以看到，Stirng是class，是引用类型，底层用 char 数组实现的。

##### 3、short s1 = 1; s1 = s1 + 1; 有错吗?short s1 = 1; s1 += 1 有错吗？

前者不正确，后者正确。

对于 short s1 = 1; s1 = s1 + 1;由于 1 是 int 类型，因此 s1+1 运算结果也是 int 型，需要强制转换类型才能赋值给 short 型。

而 short s1 = 1; s1 += 1;可以正确编译，因为 s1+= 1;相当于 s1 = (short)(s1 + 1);其中有隐含的强制类型转换。

##### 4、int和Integer有什么区别？

java 是一个完全面向对象编程语言，但是为了编程的方便还是引入了基本数据类型，为了能够将这些基本数据类型当成对象操作，Java 为每一个基本数据类型都引入了对应的包装类型（wrapper class），int 的包装类就是Integer，从 Java 5 开始引入了自动装箱/拆箱机制，使得二者可以相互转换。

java 为每个原始类型提供了包装类型：

● 基本数据类型: boolean，char，byte，short，int，long，float，double

● 包装类型：Boolean，Character，Byte，Short，Integer，Long，Float，Double

##### 5、下面 Integer 类型的数值比较输出的结果为？

```java
public class Test{
    public static void main(String[] args) {
        Integer f1 = 100, f2 = 100, f3 = 150, f4 = 150;
        System.out.println(f1 == f2);
        System.out.println(f3 == f4);
    }
}
```

如果不明白原理很容易认为两个输出要么都是 true 要么都是 false。首先需要注意的是 f1、f2、f3、f4 四个变量都是 Integer 对象引用，所以下面的==运算比较的不是值而是引用。装箱的本质是什么呢？当我们给一个Integer 对象赋一个 int 值的时候，会调用 Integer 类的静态方法 valueOf，如果看看valueOf的源代码就知道发生了什么。

```java
public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
}
```

IntegerCache 是 Integer 的内部类，其代码如下所示：

```java
private static class IntegerCache {
    static final int low = -128;
    static final int high;
    static final Integer cache[];

    static {
        // high value may be configured by property
        int h = 127;
        String integerCacheHighPropValue =
            sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
        if (integerCacheHighPropValue != null) {
            try {
                int i = parseInt(integerCacheHighPropValue);
                i = Math.max(I, 127);
                // Maximum array size is Integer.MAX_VALUE
                h = Math.min(I, Integer.MAX_VALUE - (-low) -1);
            } catch( NumberFormatException nfe) {
                // If the property cannot be parsed into an int， ignore it.
            }
        }
        high = h;

        cache = new Integer[(high - low) + 1];
        int j = low;
        for(int k = 0; k < cache.length; k++)
            cache[k] = new Integer(j++);

        // range [-128, 127] must be interned (JLS7 5.1.7)
        assert IntegerCache.high >= 127;
    }

    private IntegerCache() {}
}
```

简单的说，如果整型字面量的值在-128 到 127 之间，那么不会 new 新的 Integer 对象，而是直接引用常量池中的Integer对象，所以上面的面试题中f1==f2的结果是 true，而f3==f4 的结果是false。

##### 6、String 类常用方法？

| ***\*方法\****                                      | ***\*解释说明\****                                           |
| --------------------------------------------------- | ------------------------------------------------------------ |
| char charAt(int index)                              | 返回指定索引处的 char 值。                                   |
| boolean contains(CharSequence s)                    | 当且仅当此字符串包含指定的 char 值序列时，返回 true。        |
| boolean endsWith(String suffix)                     | 测试此字符串是否以指定的后缀结束。                           |
| boolean equals(Object anObject)                     | 将此字符串与指定的对象比较。                                 |
| boolean equalsIgnoreCase(String anotherString)      | 将此 String 与另一个 String 比较，不考虑大小写。             |
| byte[] getBytes()                                   | 使用平台的默认字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中。 |
| int indexOf(String str)                             | 返回指定子字符串在此字符串中第一次出现处的索引。             |
| int lastIndexOf(String str)                         | 返回指定子字符串在此字符串中最右边出现处的索引。             |
| int length()                                        | 返回此字符串的长度。                                         |
| String replaceAll(String regex, String replacement) | 使用给定的 replacement 替换此字符串所有匹配给定的正则表达式的子字符串。 |
| String[] split(String regex)                        | 根据给定正则表达式的匹配拆分此字符串。                       |
| boolean startsWith(String prefix)                   | 测试此字符串是否以指定的前缀开始。                           |
| String substring(int beginIndex)                    | 返回一个新的字符串，它是此字符串的一个子字符串。             |
| String substring(int beginIndex, int endIndex)      | 返回一个新字符串，它是此字符串的一个子字符串。               |
| char[] toCharArray()                                | 将此字符串转换为一个新的字符数组。                           |
| String toLowerCase()                                | 使用默认语言环境的规则将此 String 中的所有字符都转换为小写。 |
| String toUpperCase()                                | 使用默认语言环境的规则将此 String 中的所有字符都转换为大写。 |
| String trim()                                       | 返回字符串的副本，忽略前导空白和尾部空白。                   |

7、String、StringBuffer、StringBuilder的区别？

● 可变不可变

String：字符串常量，在修改时不会改变自身；若修改，等于重新生成新的字符串对象。

StringBuffer：在修改时会改变对象自身，每次操作都是对 StringBuffer 对象本身进行修改，不是生成新的对象；使用场景：对字符串经常改变情况下，主要方法：append()，insert()等。

● 线程是否安全

String：对象定义后不可变，线程安全。

StringBuffer：是线程安全的（对调用方法加入同步锁），执行效率较慢，适用于多线程下操作字符串缓冲区大量数据。

StringBuilder：是线程不安全的，适用于单线程下操作字符串缓冲区大量数据。

● 共同点

StringBuilder与StringBuffer有公共父类 AbstractStringBuilder(抽象类)。

StringBuilder、StringBuffer 的方法都会调用 AbstractStringBuilder 中的公共方法，如 super.append(...)。只是 StringBuffer 会在方法上加 synchronized 关键字，进行同步。最后，如果程序不是多线程的，那么使用StringBuilder 效率高于 StringBuffer。

##### 8、数据类型之间的转换？

● 字符串如何转基本数据类型？

调用基本数据类型对应的包装类中的方法 parseXXX(String)或valueOf(String)即可返回相应基本类型。

● 基本数据类型如何转字符串？

一种方法是将基本数据类型与空字符串（“”）连接（+）即可获得其所对应的字符串；另一种方法是调用 String类中的 valueOf()方法返回相应字符串。

# Java IO面试题（六）

1、Java 中有几种类型的流？

按照流的方向：输入流（inputStream）和输出流（outputStream）

按照实现功能分：节点流（可以从或向一个特定的地方（节点）读写数据。如 FileReader）和处理流（是对一个已存在的流的连接和封装，通过所封装的流的功能调用实现数据读写。如 BufferedReader。处理流的构造方法总是要带一个其他的流对象做参数。一个流对象经过其他流的多次包装，称为流的链接。）

按照处理数据的单位： 字节流和字符流。字节流继承于 InputStream 和 OutputStream， 字符流继承于InputStreamReader 和 OutputStreamWriter 。

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190509/1557370976@f847084bda30e33e8c628fa873d09aa6.png)

##### 2、字节流如何转为字符流？

字节输入流转字符输入流通过 InputStreamReader 实现，该类的构造函数可以传入 InputStream 对象。

字节输出流转字符输出流通过 OutputStreamWriter 实现，该类的构造函数可以传入 OutputStream 对象。

##### 3、如何将一个 java 对象序列化到文件里？

在 java 中能够被序列化的类必须先实现 Serializable 接口，该接口没有任何抽象方法只是起到一个标记作用。

```java
public class Test {
    public static void main(String[] args) throws Exception {
        //对象输出流
        ObjectOutputStream objectOutputStream =
                new ObjectOutputStream(new FileOutputStream(new File("D://obj")));
        objectOutputStream.writeObject(new User("zhangsan", 100));
        objectOutputStream.close();
        //对象输入流
        ObjectInputStream objectInputStream = new ObjectInputStream(new FileInputStream(new File("D://obj")));
        User user = (User) objectInputStream.readObject();
        System.out.println(user);
        objectInputStream.close();
    }
}
```

##### 4、字节流和字符流的区别？

字节流读取的时候，读到一个字节就返回一个字节；字符流使用了字节流读到一个或多个字节（中文对应的字节数是两个，在 UTF-8 码表中是 3 个字节）时。先去查指定的编码表，将查到的字符返回。字节流可以处理所有类型数据，如：图片，MP3，AVI视频文件，而字符流只能处理字符数据。只要是处理纯文本数据，就要优先考虑使用字符流，除此之外都用字节流。字节流主要是操作 byte 类型数据，以 byte 数组为准，主要操作类就是 OutputStream、InputStream字符流处理的单元为 2 个字节的 Unicode 字符，分别操作字符、字符数组或字符串，而字节流处理单元为 1 个字节，操作字节和字节数组。所以字符流是由 Java 虚拟机将字节转化为 2 个字节的 Unicode 字符为单位的字符而成的，所以它对多国语言支持性比较好！如果是音频文件、图片、歌曲，就用字节流好点，如果是关系到中文（文本）的，用字符流好点。在程序中一个字符等于两个字节，java 提供了 Reader、Writer 两个专门操作字符流的类。

##### 5、如何实现对象克隆？

有两种方式：

● 实现 Cloneable 接口并重写 Object 类中的 clone()方法；

● 实现 Serializable 接口，通过对象的序列化和反序列化实现克隆，可以实现真正的深度克隆，代码如下：

```java
class MyUtil {
    private MyUtil() {
        throw new AssertionError();
    }
    public static <T extends Serializable> T clone(T obj) throws Exception {
        ByteArrayOutputStream bout = new ByteArrayOutputStream();
        ObjectOutputStream oos = new ObjectOutputStream(bout);
        oos.writeObject(obj);
        ByteArrayInputStream bin = new ByteArrayInputStream(bout.toByteArray());
        ObjectInputStream ois = new ObjectInputStream(bin);
        return (T) ois.readObject();
        // 说明：调用 ByteArrayInputStream 或 ByteArrayOutputStream 对象的 close 方法没有任何意义
        // 这两个基于内存的流只要垃圾回收器清理对象就能够释放资源，这不同于对外部资源（如文件流）的释放
    }
}
```

测试代码：

```java
import java.io.Serializable;

/**
 * 人类
 */
class Person implements Serializable {
    private static final long serialVersionUID = -91020170202878978L;
    private String name; // 姓名
    private int age; // 年龄
    private Car car; // 座驾

    public Person(String name, int age, Car car) {
        this.name = name;
        this.age = age;
        this.car = car;
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

    public Car getCar() {
        return car;
    }

    public void setCar(Car car) {
        this.car = car;
    }

    @Override
    public String toString() {
        return "Person [name=" + name + ", age=" + age + ", car=" + car + "]";
    }
}

/**
 * 小汽车类
 */
class Car implements Serializable {
    private static final long serialVersionUID = -57138907627603702L;
    private String brand; // 品牌
    private int maxSpeed; // 最高时速

    public Car(String brand, int maxSpeed) {
        this.brand = brand;
        this.maxSpeed = maxSpeed;
    }

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public int getMaxSpeed() {
        return maxSpeed;
    }

    public void setMaxSpeed(int maxSpeed) {
        this.maxSpeed = maxSpeed;
    }

    @Override
    public String toString() {
        return "Car [brand=" + brand + ", maxSpeed=" + maxSpeed + "]";
    }
}

class CloneTest {
    public static void main(String[] args) {
        try {
            Person p1 = new Person("dujubin", 33, new Car("Benz", 300));
            Person p2 = MyUtil.clone(p1); // 深度克隆
            p2.getCar().setBrand("BYD");
            // 修改克隆的 Person 对象 p2 关联的汽车对象的品牌属性
            // 原来的 Person 对象 p1 关联的汽车不会受到任何影响
            // 因为在克隆 Person 对象时其关联的汽车对象也被克隆了
            System.out.println(p1);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

注意：基于序列化和反序列化实现的克隆不仅仅是深度克隆，更重要的是通过泛型限定，可以检查出要克隆的对象是否支持序列化，这项检查是编译器完成的，不是在运行时抛出异常，这种是方案明显优于使用 Object 类的 clone 方法克隆对象。让问题在编译的时候暴露出来总是好过把问题留到运行时。

##### 6.什么是 java 序列化，如何实现 java 序列化？

序列化就是一种用来处理对象流的机制，所谓对象流也就是将对象的内容进行流化。可以对流化后的对象进行读写操作，也可将流化后的对象传输于网络之间。序列化是为了解决在对对象流进行读写操作时所引发的问题。序 列 化 的 实 现 ： 将 需 要 被 序 列 化 的 类 实 现 Serializable 接 口 ， 该 接 口 没 有 需 要 实 现 的 方 法 ， implements Serializable 只是为了标注该对象是可被序列化的，然后使用一个输出流(如：FileOutputStream)来构造一个 ObjectOutputStream(对象流)对象，接着，使用 ObjectOutputStream 对象的 writeObject(Object obj)方法就可以将参数为 obj 的对象写出(即保存其状态)，要恢复的话则用输入流。

# Java集合面试题（七）

##### 1、HashMap排序题

已知一个 HashMap<Integer，User>集合， User 有 name（String）和 age（int）属性。请写一个方法实现对HashMap 的排序功能，该方法接收 HashMap<Integer，User>为形参，返回类型为 HashMap<Integer，User>，要求对 HashMap 中的 User 的 age 倒序进行排序。排序时 key=value 键值对不得拆散。

注意：要做出这道题必须对集合的体系结构非常的熟悉。HashMap本身就是不可排序的，但是该题偏偏让HashMap排序，那我们就得想在API中有没有这样的 Map 结构是有序的，我们不难发现其中LinkedHashMap就具有这样的结构，是链表结构有序的，更可喜的是他是  HashMap的子类，我们返回LinkedHashMap<Integer，User>即可，还符合面向接口编程的思想。

但凡是对集合的操作，我们应该保持一个原则就是能用JDK中的API就用JDK中的 API，比如排序算法我们不应该去用冒泡或者选择，而是首先想到用 Collections 集合工具类。

```java
import java.util.*;

class HashMapTest {
    public static void main(String[] args) {
        HashMap<Integer, User> users = new HashMap<>();
        users.put(1, new User("张三", 25));
        users.put(3,new User("李四",22));
        users.put(2, new User("王五", 28));
        System.out.println(users);
        HashMap<Integer, User> sortHashMap = sortHashMap(users);
        System.out.println(sortHashMap);
        /**
         * 控制台输出内容
         * {1=User [name=张三, age=25], 2=User [name=王五,age=28], 3=User [name=李四, age=22]}
         * {2=User [name=王五, age=28], 1=User [name=张三, age=25], 3=User [name=李四, age=22]}
         */
    }

    public static HashMap<Integer, User> sortHashMap(HashMap<Integer, User> map) {
        // 首先拿到 map 的键值对集合
        Set<Map.Entry<Integer, User>> entrySet = map.entrySet();
        // 将 set 集合转为 List 集合，为什么，为了使用工具类的排序方法
        List<Map.Entry<Integer,User>> list = new ArrayList<Map.Entry<Integer, User>>(entrySet);
        // 使用 Collections 集合工具类对 list 进行排序，排序规则使用匿名内部类来实现
        Collections.sort(list, new Comparator<Map.Entry<Integer, User>>() {
            @Override
            public int compare(Map.Entry<Integer, User> o1, Map.Entry<Integer, User> o2) {
                //按照要求根据 User 的 age 的倒序进行排
                return o2.getValue().getAge() - o1.getValue().getAge();
            }
        });
        //创建一个新的有序的 HashMap 子类的集合
        LinkedHashMap<Integer, User> linkedHashMap = new LinkedHashMap<Integer, User>();
        //将 List 中的数据存储在 LinkedHashMap 中
        for (Map.Entry<Integer,User> entry : list) {
            linkedHashMap.put(entry.getKey(), entry.getValue());
        }
        return linkedHashMap;
    }
}

class User {
    private String name;
    private int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

##### 2、请问 ArrayList、HashSet、HashMap 是线程安全的吗？如果不是怎么获取线程安全的集合？

通过以上类的源码进行分析，每个方法都没有加锁，显然都是非线程安全的。在集合中Vector 和HashTable是线程安全的。打开源码会发现其实就是把各自核心方法添加上了synchronized 关键字。Collections工具类提供了相关的 API，可以让上面那3个不安全的集合变为安全的。

```java
Collections.synchronizedCollection(c);
Collections.synchronizedList(list);
Collections.synchronizedMap(m);
Collections.synchronizedSet(s);
```

上面几个函数都有对应的返回值类型，传入什么类型返回什么类型。打开源码其实原理非常简单，就是将集合的核心方法添加上了synchronized关键字。

##### 3、ArrayList内部用什么实现的？

回答这样的问题，不要只回答个皮毛，可以再介绍一下ArrayList内部是如何实现数组的增加和删除的，因为数组在创建的时候长度是固定的，那么就有个问题我们往ArrayList中不断的添加对象，它是如何管理这些数组呢？通过源码可以看到ArrayList内部是用Object[]实现的。接下来我们分别分析ArrayList的构造以及add()、remove()、clear()方法的实现原理。

● 无参数构造方法

```java
/**
 * Constructs a new {@code ArrayList} instance with zero initial capacity.
 */
public ArrayList(){
    array=EmptyArray.OBJECT;
}
```

array 是一个 Object[]类型。当我们 new 一个空参构造时系统调用了 EmptyArray.OBJECT 属性，EmptyArray 仅仅是一个系统的类库，该类源码如下：

```java
public final class EmptyArray {
    private EmptyArray() {
    }
    public static final boolean[] BOOLEAN = new boolean[0];
    public static final byte[] BYTE = new byte[0];
    public static final char[] CHAR = new char[0];
    public static final double[] DOUBLE = new double[0];
    public static final int[] INT = new int[0];
    public static final Class<?>[] CLASS = new Class[0];
    public static final Object[] OBJECT = new Object[0];
    public static final String[] STRING = new String[0];
    public static final Throwable[] THROWABLE = new Throwable[0];
    public static final StackTraceElement[] STACK_TRACE_ELEMENT = new StackTraceElement[0];
}
```

也就是说当我们 new 一个空参 ArrayList 的时候，系统内部使用了一个 new Object[0]数组。

● 带容量参数的构造器

```java
/**
 * Constructs a new instance of {@code ArrayList} with the specified
 * initial capacity.
 * @param capacity the initial capacity of this {@code ArrayList}.
 */
public ArrayList(int capacity) {
    if (capacity < 0) {
        throw new IllegalArgumentException("capacity < 0: " + capacity);
    }
    array = (capacity == 0 ? EmptyArray.OBJECT : new Object[capacity]);
}
```

该构造函数传入一个 int 值，该值作为数组的长度值。如果该值小于 0，则抛出一个运行时异常。如果等于 0，则使用一个空数组，如果大于 0，则创建一个长度为该值的新数组。

● 带集合参数的构造器

```java
/**
 * Constructs a new instance of {@code ArrayList} containing the elements of
 * the specified collection.
 *
 * @param collection the collection of elements to add.
 */
public ArrayList(Collection<? extends E> collection) {
    if (collection == null) {
        throw new NullPointerException("collection == null");
    }

    Object[] a = collection.toArray();
    if (a.getClass() != Object[].class) {
        Object[] newArray = new Object[a.length];
        System.arraycopy(a, 0, newArray, 0, a.length);
        a = newArray;
    }
    array = a;
    size = a.length;
}
```

如果调用构造函数的时候传入了一个 Collection 的子类，那么先判断该集合是否为 null，为 null 则抛出空指针异常。如果不是则将该集合转换为数组 a，然后将该数组赋值为成员变量 array，将该数组的长度作为成员变量 size。

● add方法

```java
/**
 * Adds the specified object at the end of this {@code ArrayList}.
 *
 * @param object the object to add.
 * @return always true
 */
@Override
public boolean add(E object) {
    Object[] a = array;
    int s = size;
    if (s == a.length) {
        Object[] newArray = new Object[s +
                (s < (MIN_CAPACITY_INCREMENT / 2) ? MIN_CAPACITY_INCREMENT : s >> 1)];
        System.arraycopy(a, 0, newArray, 0, s);
        array = a = newArray;
    }
    a[s] = object;
    size = s + 1;
    modCount++;
    return true;
}
```

● 第一：首先将成员变量 array 赋值给局部变量 a，将成员变量 size 赋值给局部变量 s。

● 第二：判断集合的长度 s 是否等于数组的长度（如果集合的长度已经等于数组的长度了，说明数组已经满了，该重新分 配 新 数 组 了 ） ， 重 新 分 配 数 组 的 时 候 需 要 计 算 新 分 配 内 存 的 空 间 大 小 ， 如 果 当 前 的 长 度 小 于MIN_CAPACITY_INCREMENT/2（这个常量值是 12，除以 2 就是 6，也就是如果当前集合长度小于 6）则分配 12 个长度，如果集合长度大于 6 则分配当前长度 s 的一半长度。这里面用到了三元运算符和位运算，s >> 1，意思就是将s 往右移 1 位，相当于 s=s/2，只不过位运算是效率最高的运算。

● 第三：将新添加的 object 对象作为数组的 a[s]个元素。

● 第四：修 改 集 合 长 度size为s+1。

● 第五：modCount++，该变量是父类中声明的，用于记录集合修改的次数，记录集合修改的次数是为了防止在用迭代器迭代集合时避免并发修改异常，或者说用于判断是否出现并发修改异常的。

● 第六：return true，这个返回值意义不大，因为一直返回 true，除非报了一个运行时异常。

● remove方法

```java
/**
 * Removes the object at the specified location from this list.
 *
 * @param index the index of the object to remove.
 * @return the removed object.
 * @throws IndexOutOfBoundsException when {@code location < 0 || location >= size()}
 */
@Override
public E remove(int index) {
    Object[] a = array;
    int s = size;
    if (index >= s) {
        throwIndexOutOfBoundsException(index, s);
    }
    @SuppressWarnings("unchecked") E result = (E) a[index];
    System.arraycopy(a, index + 1, a, index, --s - index);
    a[s] = null; // Prevent memory leak
    size = s;
    modCount++;
    return result;
}
```

● 第一：先将成员变量 array 和 size 赋值给局部变量 a 和 s。

● 第二：判断形参 index 是否大于等于集合的长度，如果成了则抛出运行时异常

● 第三：获取数组中脚标为 index 的对象 result，该对象作为方法的返回值

● 第四：调用 System 的 arraycopy 函数完成数组拷贝。

● 第五：接下来就是很重要的一个工作，因为删除了一个元素，而且集合整体向前移动了一位，因此需要将集合最后一个元素设置为 null，否则就可能内存泄露。

● 第六：重新给成员变量 array 和 size 赋值。

● 第七：记录修改次数。

● 第八：返回删除的元素。

● clear方法

```java
/**
 * Removes all elements from this {@code ArrayList}, leaving it empty.
 *
 * @see #isEmpty
 * @see #size
 */
@Override
public void clear() {
    if (size != 0) {
        Arrays.fill(array, 0, size, null);
        size = 0;
        modCount++;
    }
}
```

如果集合长度不等于 0，则将所有数组的值都设置为 null，然后将成员变量 size 设置为 0 即可，最后让修改记录加 1。

##### 4、并发集合和普通集合如何区别？

并发集合常见的有ConcurrentHashMap、ConcurrentLinkedQueue、ConcurrentLinkedDeque等。并发集合位于java.util.concurrent包下，是jdk1.5之后才有的，在 java 中有普通集合、同步（线程安全）的集合、并发集合。

普通集合通常性能最高，但是不保证多线程的安全性和并发的可靠性。线程安全集合仅仅是给集合添加了 synchronized 同步锁，严重牺牲了性能，而且对并发的效率就更低了，并发集合则通过复杂的策略不仅保证了多线程的安全又提高的并发时的效率。

参考阅读：ConcurrentHashMap 是线程安全的 HashMap 的实现，默认构造同样有 initialCapacity 和 loadFactor 属性， 不过还多了一个 concurrencyLevel 属性，三属性默认值分别为 16、0.75 及 16。其内部使用锁分段技术，维持这锁Segment 的数组，在 Segment 数组中又存放着 Entity[]数组，内部 hash 算法将数据较均匀分布在不同锁中。put 操作：并没有在此方法上加上 synchronized，首先对 key.hashcode 进行 hash 操作，得到 key 的 hash 值。hash 操作的算法和map 也不同，根据此hash 值计算并获取其对应的数组中的Segment 对象(继承自ReentrantLock)，接着调用此 Segment 对象的 put 方法来完成当前操作。ConcurrentHashMap 基于 concurrencyLevel 划分出了多个 Segment 来对 key-value 进行存储，从而避免每次 put 操作都得锁住整个数组。在默认的情况下，最佳情况时可允许 16 个线程并发无阻塞的操作集合对象，尽可能地减少并发时的阻塞现象。get(key)首先对 key.hashCode 进行 hash 操作，基于其值找到对应的 Segment 对象，调用其 get 方法完成当前操作。而 Segment 的 get 操作首先通过 hash 值和对象数组大小减1的值进行按位与操作来获取数组上对应位置的HashEntry。在这个步骤中，可能会因为对象数组大小的改变，以及数组上对应位置的 HashEntry 产生不一致性，那么 ConcurrentHashMap 是如何保证的？对象数组大小的改变只有在 put 操作时有可能发生，由于 HashEntry 对象数组对应的变量是 volatile 类型的，因此可以保证如 HashEntry 对象数组大小发生改变，读操作可看到最新的对象数组大小。在获取到了 HashEntry 对象后，怎么能保证它及其 next 属性构成的链表上的对象不会改变呢？这点ConcurrentHashMap 采用了一个简单的方式，即 HashEntry 对象中的 hash、key、next 属性都是 final 的，这也就意味着没办法插入一个 HashEntry 对象到基于 next 属性构成的链表中间或末尾。这样就可以保证当获取到 HashEntry 对象后，其基于 next 属性构建的链表是不会发生变化的。ConcurrentHashMap 默认情况下采用将数据分为 16 个段进行存储，并且 16 个段分别持有各自不同的锁Segment，锁仅用于 put 和 remove 等改变集合对象的操作，基于 volatile 及 HashEntry 链表的不变性实现了读取的不加锁。这些方式使得 ConcurrentHashMap 能够保持极好的并发支持，尤其是对于读远比插入和删除频繁的 Map 而言，而它采用的这些方法也可谓是对于 Java 内存模型、并发机制深刻掌握的体现。

##### 5、List 和 Map、Set 的区别？

● 结构特点：List 和 Set 是存储单列数据的集合，Map 是存储键和值这样的双列数据的集合；List 中存储的数据是有顺序，并且允许重复；Map 中存储的数据是没有顺序的，其键是不能重复的，它的值是可以有重复的，Set 中存储的数据是无序的，且不允许有重复，但元素在集合中的位置由元素的 hashCode 决定，位置是固定的（Set 集合根据 hashCode 来进行数据的存储，所以位置是固定的，但是位置不是用户可以控制的，所以对于用户来说 Set 中的元素还是无序的）；

● 实现类：List 接口下的实现类（LinkedList：基于链表实现，链表内存是散乱的，每一个元素存储本身内存地址的同时还存储下一个元素的地址。链表增删快，查找慢；ArrayList：基于数组实现，非线程安全的，效率高，便于索引，但不便于插入删除；Vector：基于数组实现，线程安全的，效率低）。Map 接口下的实现类（HashMap：基于 hash 表的 Map 接口实现，非线程安全，高效，支持 null 值和 null 键；Hashtable：线程安全，低效，不支持 null 值和  null 键；LinkedHashMap：是HashMap 的一个子类，保存了记录的插入顺序；SortedMap 接口：TreeMap，能够把它保存的记录根据键排序，默认是键值的升序排序）。Set 接口下的实现类（HashSet：底层是由 HashMap 实现，不允许集合中有重复的值，使用该方式时需要重写 equals()和 hashCode()方法；LinkedHashSet继承与 HashSet，同时又基于LinkedHashMap 来进行实现，底层使用的是LinkedHashMp）。

● 区别：List集合中对象按照索引位置排序，可以有重复对象，允许按照对象在集合中的索引位置检索对象，例如通过list.get(i)方法来获取集合中的元素；Map中的每一个元素包含一个键和一个值，成对出现，键对象不可以重复，值对象可以重复；Set集合中的对象不按照特定的方式排序，并且没有重复对象，但它的实现类能对集合中的对象按照特定的方式排序，例如 TreeSet类，可以按照默认顺序，也可以通过实现 java.util.Comparator接口来自定义排序方式。

##### 6、HashMap和Hashtable有什么区别？

HashMap是非线程安全的，HashMap是Map的一个实现类，是将键映射到值的对象，不允许键值重复。允许空键和空值；由于非线程安全，HashMap的效率要较 Hashtable 的效率高一些。Hashtable 是线程安全的一个集合，不允许 null 值作为一个 key 值或者value 值；Hashtable是sychronized，多个线程访问时不需要自己为它的方法实现同步，而 HashMap 在被多个线程访问的时候需要自己为它的方法实现同步。

##### 7、数组和链表分别比较适合用于什么场景，为什么？

● 数组和链表的区别

数组是将元素在内存中连续存储的；它的优点：因为数据是连续存储的，内存地址连续，所以在查找数据的时候效率比较高；它的缺点：在存储之前，我们需要申请一块连续的内存空间，并且在编译的时候就必须确定好它的空间的大小。在运行的时候空间的大小是无法随着你的需要进行增加和减少而改变的，当数据两比较大的时候，有可能会出现越界的情况，数据比较小的时候，又有可能会浪费掉内存空间。在改变数据个数时，增加、插入、删除数据效率比较低。链表是动态申请内存空间，不需要像数组需要提前申请好内存的大小，链表只需在用的时候申请就可以，根据需要来动态申请或者删除内存空间，对于数据增加和删除以及插入比数组灵活。还有就是链表中数据在内存中可以在任意的位置，通过应用来关联数据（就是通过存在元素的地址来联系）

● 链表和数组使用场景

数组应用场景：数据比较少；经常做的运算是按序号访问数据元素；数组更容易实现，任何高级语言都支持；构建的线性表较稳定。

链表应用场景：对线性表的长度或者规模难以估计；频繁做插入删除操作；构建动态性比较强的线性表。

##### 8、Java中ArrayList和LinkedList区别？

ArrayList和Vector使用了数组的实现，可以认为 ArrayList 或者 Vector 封装了对内部数组的操作，比如向数组中添加、删除、插入新的元素或者数据的扩展和重定向。

LinkedList 使用了循环双向链表数据结构。与基于数组的 ArrayList 相比，这是两种截然不同的实现技术，这也决定了它们将适用于完全不同的工作场景。

LinkedList 链表由一系列表项连接而成。一个表项总是包含 3 个部分：元素内容，前驱表和后驱表，如图所示：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190510/1557476054@a3fbf8a49f2e9cf5a54478ccc9a9d840.png)

在下图展示了一个包含 3 个元素的 LinkedList 的各个表项间的连接关系。在 JDK 的实现中，无论 LikedList 是否为空，链表内部都有一个 header 表项，它既表示链表的开始，也表示链表的结尾。表项 header 的后驱表项便是链表中第一个元素，表项 header 的前驱表项便是链表中最后一个元素。

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190510/1557476130@3b3411a65547803e24883fb86e0187c3.png)

ArrayList 是实现了基于动态数组的数据结构，LinkedList 基于链表的数据结构。如果集合数据是对于集合随机访问 get 和 set，ArrayList 绝对优于 LinkedList，因为 LinkedList 要移动指针。如果集合数据是对于集合新增和删除操作 add 和 remove，LinkedList 比较占优势，因为ArrayList要移动数据。

ArrayList 和 LinkedList 是两个集合类，用于存储一系列的对象引用(references)。例如我们可以用 ArrayList 来存储一系列的 String 或者 Integer。那么 ArrayList 和 LinkedList 在性能上有什么差别呢？什么时候应该用 ArrayList 什么时候又该用 LinkedList 呢？

● 时间复杂度

首先一点关键的是，ArrayList 的内部实现是基于基础的对象数组的，因此，它使用 get 方法访问列表中的任意一个元素时(random access)，它的速度要比 LinkedList 快。LinkedList 中的 get 方法是按照顺序从列表的一端开始检查，直到另外一端。对 LinkedList 而言，访问列表中的某个指定元素没有更快的方法了。

假设我们有一个很大的列表，它里面的元素已经排好序了，这个列表可能是 ArrayList 类型的也可能是 LinkedList 类型的，现在我们对这个列表来进行二分查找(binary search)，比较列表是 ArrayList 和 LinkedList 时的查询速度，看下面的程序：

```java
public class TestList {
    public static final int N = 50000;    //50000 个数
    public static List values; //要查找的集合
    //放入 50000 个数给 value；
    static {
        Integer vals[] = new Integer[N];
        Random r = new Random();
        for (int i = 0, currval = 0; i < N; i++) {
            vals = new Integer(currval);
            currval += r.nextInt(100) + 1;
        }
        values = Arrays.asList(vals);
    }
    //通过二分查找法查找
    static long timeList(List lst) {
        long start = System.currentTimeMillis();
        for (int i = 0; i < N; i++) {
            int index = Collections.binarySearch(lst, values.get(i));
            if (index != i)
                System.out.println("***错误***");
        }
        return System.currentTimeMillis() - start;
    }
    public static void main(String args[]) {
        System.out.println("ArrayList 消耗时间：" + timeList(new ArrayList(values)));
        System.out.println("LinkedList 消耗时间：" + timeList(new LinkedList(values)));
    }
}
```

LinkedList 做随机访问所消耗的时间与这个 list 的大小是成比例的。而相应的，在 ArrayList 中进行随机访问所消耗的时间是固定的。

这是否表明 ArrayList 总是比 LinkedList 性能要好呢？这并不一定，在某些情况下 LinkedList 的表现要优于ArrayList，有些算法在 LinkedList 中实现时效率更高。比方说，利用 Collections.reverse 方法对列表进行反转时，其性能就要好些。看这样一个例子，加入我们有一个列表，要对其进行大量的插入和删除操作，在这种情况下LinkedList 就是一个较好的选择。请看如下一个极端的例子，我们重复的在一个列表的开端插入一个元素：

```java
public class ListDemo {
    static final int N = 50000;
    static long timeList(List list) {
        long start = System.currentTimeMillis();
        Object o = new Object();
        for (int i = 0; i < N; i++)
            list.add(0, o);
        return System.currentTimeMillis() - start;
    }
    public static void main(String[] args) {
        System.out.println("ArrayList 耗时：" + timeList(new ArrayList()));
        System.out.println("LinkedList 耗时：" + timeList(new LinkedList()));
    }
}
```

输出结果是：

ArrayList 耗时：2463

LinkedList 耗时：15

● 空间复杂度

在 LinkedList 中有一个私有的内部类，定义如下：

```java
private static class Entry {
    Object element;
    Entry next;
    Entry previous;
}
```

每个 Entry对象reference列表中的一个元素，同时还有在 LinkedList 中它的上一个元素和下一个元素。一个有1000个元素的LinkedList 对象将有 1000 个链接在一起的 Entry 对象，每个对象都对应于列表中的一个元素。这样的话，在一个 LinkedList 结构中将有一个很大的空间开销，因为它要存储这 1000 个 Entity 对象的相关信息。

ArrayList 使用一个内置的数组来存储元素，这个数组的起始容量是10。当数组需要增长时，新的容量按如下公式获得：新容量=(旧容量*3)/2+1，也就是说每一次容量大概会增长 50%。这就意味着，如果你有一个包含大量元素的 ArrayList 对象，那么最终将有很大的空间会被浪费掉，这个浪费是由ArrayList 的工作方式本身造成的。如果没有足够的空间来存放新的元素，数组将不得不被重新进行分配以便能够增加新的元素。对数组进行重新分配，将会导致性能急剧下降。如果我们知道一个ArrayList将会有多少个元素，我们可以通过构造方法来指定容量。我们还可以通过 trimToSize 方法在 ArrayList 分配完毕之后去掉浪费掉的空间。

● 总结

ArrayList 和 LinkedList 在性能上各有优缺点，都有各自所适用的地方，总的说来可以描述如下：

第一：对 ArrayList 和 LinkedList 而言，在列表末尾增加一个元素所花的开销都是固定的。对 ArrayList 而言，主要是在内部数组中增加一项，指向所添加的元素，偶尔可能会导致对数组重新进行分配；而对 LinkedList 而言，这个开销是统一的，分配一个内部 Entry 对象。

第二：在 ArrayList 的中间插入或删除一个元素意味着这个列表中剩余的元素都会被移动；而在 LinkedList 的中间插入或删除一个元素的开销是固定的。

第三：LinkedList 不支持高效的随机元素访问。

第四：ArrayList 的空间浪费主要体现在在 list 列表的结尾预留一定的容量空间，而 LinkedList 的空间花费则体现在它的每一个元素都需要消耗相当的空间。

可以这样说：当操作是在一列数据的后面添加数据而不是在前面或中间，并且需要随机地访问其中的元素时，使用ArrayList 会提供比较好的性能；当你的操作是在一列数据的前面或中间添加或删除数据，并且按照顺序访问其中的元素时，就应该使用LinkedList了。

##### 9、List a=new ArrayList()和ArrayList a =new ArrayList()的区别？

List list = new ArrayList();这句创建了一个 ArrayList 的对象后赋给了List。此时它是一个 List 对象了，有些ArrayList 有但是 List 没有的属性和方法，它就不能再用了。而ArrayList list=new ArrayList();创建一对象则保留了ArrayList 的所有属性。 所以需要用到 ArrayList 独有的方法的时候不能用前者。实例代码如下：

```java
List list = new ArrayList();
ArrayList arrayList = new ArrayList();
list.trimToSize(); //错误，没有该方法。
arrayList.trimToSize();    //ArrayList 里有该方法。
```

10、请用两个队列模拟堆栈结构？

两个队列模拟一个堆栈，队列是先进先出，而堆栈是先进后出。模拟如下队列 a 和 b：

● 入栈：a 队列为空，b 为空。例：则将”a，b，c，d，e”需要入栈的元素先放 a 中，a 进栈为”a，b，c，d，e”出栈：a 队列目前的元素为”a，b，c，d，e”。将 a 队列依次加入 Arraylist 集合 a 中。以倒序的方法，将 a 中的集合取出，放入 b 队列中，再将 b 队列出列。代码如下：

```java
public static void main(String[] args) {
    Queue<String> queue = new LinkedList<String>(); //a 队 列
    Queue<String> queue2 = new LinkedList<String>();    //b 队列
    ArrayList<String> a = new ArrayList<String>();    //arrylist 集合是中间参数
    //往 a 队列添加元素
    queue.offer("a");
    queue.offer("b");
    queue.offer("c");
    queue.offer("d");
    queue.offer("e");
    System.out.print("进栈：");    //a 队列依次加入 list 集合之中
    for (String q : queue) {
        a.add(q);
        System.out.print(q);
    }
    //以倒序的方法取出（a 队列依次加入 list 集合）之中的值，加入 b 对列
    for (int i = a.size() - 1; i >= 0; i--) {
        queue2.offer(a.get(i));
    }
    //打印出栈队列
    System.out.println("");
    System.out.print("出栈：");
    for (String q : queue2) {
        System.out.print(q);
    }
}
```

运行结果为（遵循栈模式先进后出）：

进栈：a b c d e

出栈：e d c b a

##### 11、Map中的key和value可以为null？

HashMap 对象的 key、value 值均可为 null。Hahtable 对象的 key、value 值均不可为 null。且两者的的 key 值均不能重复，若添加 key 相同的键值对，后面的 value 会自动覆盖前面的 value，但不会报错。测试代码如下：

```java
public class Test {
    public static void main(String[] args) {
        Map<String, String> map = new HashMap<String, String>();//HashMap 对象
        Map<String, String> tableMap = new Hashtable<String, String>();//Hashtable 对象
        map.put(null, null);
        System.out.println("hashMap 的[key]和[value]均可以为 null:" + map.get(null));
        try {
            tableMap.put(null, "3");
            System.out.println(tableMap.get(null));
        } catch (Exception e) {
            System.out.println("【ERROR】：hashtable 的[key]不能为 null");
        }
        try {
            tableMap.put("3", null);
            System.out.println(tableMap.get("3"));
        } catch (Exception e) {
            System.out.println("【ERROR】：hashtable的[value]不能为null");
        }
    }
}
```

运行结果：

hashMap 的[key]和[value]均可以为 null:null

【ERROR】：hashtable 的[key]不能为 null

【ERROR】：hashtable 的[value]不能为 null

# MySQL面试题全集

##### 1， 事务

 事务：事务是访问和更新数据库的程序执行的一个逻辑单元；事务中可能包含一个或多个sql语句，这些语句要么都执行，要么都不执行。作为一个关系型数据库，MySQL支持事务。

     事务的特性（ACID）：

原子性(Automicity)：即整个事务是最小的一个执行单元，不可再分。事务的操作要么完成，要么都不做；如果事务中一个sql语句执行失败，则已执行的语句也必须回滚，数据库退回到事务前的状态。
一致性(Consistency)：即事务执行前后的状态变化是一致的。
隔离性(Isolation)：隔离性是指事务内部的操作与其他事务是隔离的，并发执行的各个事务之间不能互相干扰。
持久性(Durability)：即事务执行完之后，结果是持久的，哪怕发生宕机，结果仍然是事务完成之后的状态
读操作可能会存在的问题：ACID事务特性，能够很好地保证单个事务的数据准确性。但是，在并发的情况下，多个事务共同操作一个数据库时，可能会产生脏读、不可重复读、幻读问题

脏读：当前事务(A)中可以读到其他事务(B)未提交的数据（脏数据），这种现象是脏读。
不可重复读：在事务A中先后两次读取同一个数据，两次读取的结果不一样，这种现象称为不可重复读。脏读与不可重复读的区别在于：前者读到的是其他事务未提交的数据，后者读到的是其他事务已提交的数据。
幻读：在事务A中按照某个条件先后两次查询数据库，两次查询结果的条数不同，这种现象称为幻读。不可重复读与幻读的区别可以通俗的理解为：前者是数据变了，后者是数据的行数变了。
事务的隔离级别：克服产生脏读、幻读等问题的方法是提高事务的隔离级别。SQL标准中定义了四种隔离级别，一般来说，隔离级别越低，开销越低，可支持并发性越高，但隔离性越差。隔离级别与会产生的问题如下：

读未提交隔离级别最低、可支持并发度最高，无法克服脏读、不可重复读和幻读任何一种问题；读已提交可以克服脏读现象
可重复读克服脏读和不可重复读；可串行化（序列化）可以克服全部三种问题，但是可串行化强制事务串行，并发效率很低，只有当对数据一致性要求极高且可以接受没有并发时使用，因此使用也较少。因此在大多数数据库系统中，默认的隔离级别是读已提交(RC:ORACLE,SQLSERVER)或可重复读（RR:MYSQL）

现在互联网工程一般默认选择RC,主要原因有：

1 在RR隔离级别下，存在间隙锁，导致出现死锁的几率比RC大的多！
        此时执行语句“select * from test where id <3 for update” ，在RR隔离级别下，存在间隙锁，可以锁住(2,5)这个间隙，防止其他事务插入数据！而在RC隔离级别下，不存在间隙锁，其他事务是可以插入数据！

       2 在RR隔离级别下，条件列未命中索引会锁表！而在RC隔离级别下，只锁行。
    
        在RC隔离级别下，其先走聚簇索引，进行全部扫描加锁，但是MySQL做了优化，在MySQL Server过滤条件，发现不满足后，会调用unlock_row方法，把不满足条件的记录放锁。而在RR隔离级别下，会直接把整张表加锁。

MVCC（Multi-Version Concurrency Control，即多版本的并发控制协议）:在Mysql中，通过MVCC来利用RR解决脏读、不可重复读、幻读等问题。

数据的读取可以分为两种：快照读和当前读。快照读适用于简单的select语句，当前读是基于临键锁（行锁 + 间歇锁）来实现的，适用于 insert，update，delete， select ... for update， select ... lock in share mode 语句，以及加锁了的 select 语句        

mysql默认是快照读
出现(delete、update、insert)时改为当前读，最新的数据也会被读取。
第一步：mysql会为每一条数据，隐式加上两个字段，一个是创建版本号赋值，另一个是删除版本号赋值。在快照读的状态下，表的数据发生变化即会制作成一个新的版本。select时读取数据的规则为：创建版本号<=当前事务版本号，删除版本号为空或>当前事务版本号。通过MVCC机制，虽然让数据变得可重复读，但我们读到的数据可能是历史数据，不是数据库最新的数据。这种读取历史数据的方式，我们叫它快照读 (snapshot read)，而读取数据库最新版本数据的方式，叫当前读 (current read)。

第二步：locks由record locks(行锁、索引加锁) 和 gap locks(间隙锁），读取数据的附近记录也加锁，保证能够读取到最新数据，附近数据不被修改

2 B树和B+树

参考博文：程序员小灰有关B树和B+树的讲解

这里之所以提到B树和B+树，是因为mysql里面最常用的索引就是B+树，有时候面试会问到为什么使用B+树，B+树有什么优势。

B数和B+树简单来说是一种多路搜索树。

B树：一个m阶的B树具有如下几个特征：

根结点至少有两个子女。
每个中间节点都包含k-1个元素和k个孩子，其中 m/2 <= k <= m
每一个叶子节点都包含k-1个元素，其中 m/2 <= k <= m
所有的叶子结点都位于同一层。
每个节点中的元素从小到大排列，节点当中k-1个元素正好是k个孩子包含的元素的值域分划。
如图，是一个三阶的B树

B树的关键字分布在整棵树中，任何关键字出现且仅出现一次在一个节点中，其查找复杂度相当于一个二分查找

B+树：B+树是B树的一种变体，有着比B树更好的查询性能。对于一个B+树，除了B树的特点之外，还有如下特点：

有k个子树的中间节点包含有k个元素（B树中是k-1个元素），每个元素不保存数据，只用来索引，所有数据都保存在叶子节点（这就是所谓的卫星数据。卫星数据就是指节点的具体信息）。
所有的叶子结点中包含了全部元素的信息，及指向含这些元素记录的指针，且叶子结点本身依关键字的大小自小而大顺序链接。
每一个父节点都出现在子节点中，是子节点元素中是最大（或最小）元素。
如图，一个三阶B+树

B+树的优势：

B+树的中间节点没有卫星数据，所以同样大小的磁盘页可以容纳更多的节点元素，使得查询的IO次数更少。
所有查询都要查找到叶子节点，查询性能稳定。
所有叶子节点形成有序链表，范围查询只需在链表上做遍历即可，便于范围查询。
hash索引和B+树索引区别：Mysql里面一共有四种索引，经常会问的索引除了B+数之外，还有hash索引（hash索引就是采用一定的hash算法建立索引，键值对）

如果是等值查询，那么哈希索引明显有绝对优势，只需要经过一次算法即可找到相应的键值；
如果是范围查询检索，原先是有序的键值，经过哈希算法后，有可能变成不连续的了，不能利用索引完成范围查询检索；
哈希索引也没办法利用索引完成排序，以及like ‘xxx%’ 这样的部分模糊查询
哈希索引也不支持复合索引的最左匹配规则；
B+树索引的关键字检索效率比较平均，不像B树那样波动幅度大，在有大量重复键值情况下，因为哈希碰撞问题，哈希索引的效率也会变低。
3 建立索引

索引：索引是帮助Mysql高效获取数据的数据结构。常用的索引有主键索引、普通索引、唯一索引和全文索引。

主键索引（PRIMARY ）：唯一且不能为空，在一个表里面，主键索引也就是这个表的主键。
普通索引（INDEX）：普通的索引。
唯一索引（UNIQUE）：唯一索引是普通索引的特殊情况，索引不允许有重复，主键索引就是一种唯一索引。
全文索引（FULLTEXT ）：用于在一篇文章中，检索文本信息的。
什么情况建立索引：

适合创建索引条件

主键自动建立主键索引
频繁作为查询条件的字段应该建立索引
查询中与其他表关联的字段，外键关系建立索引
单键/组合索引的选择问题，组合索引性价比更高
查询中排序的字段，排序字段若通过索引去访问将大大提高排序效率
查询中统计或者分组字段
不适合创建索引条件

数据量不大的
经常增删改的表或者字段
where条件里用不到的字段不创建索引
数据存在大量重复的
优势：提高数据检索的效率，降低数据库IO成本

劣势：索引也需要维护

4 组合索引（多列索引）

组合索引：MySQL能在多个列上创建索引。一个索引可以由最多15个列组成，组合索引的性价比相对来说更高。

最左原则：组合索引是先按照第一列进行排序，然后在第一列排好序的基础上再对第二列排序，如果跳过第一列直接访问第二列，直接访问后面的列就用不到索引了。例如，组合索引（a，b，c），一般都是先匹配a，然后匹配b，最后匹配c。

适用场景：

全字段匹配
匹配部分最左前缀
匹配第一列范围查询(可用用like a%,但不能使用like %b，最左原则)
精确匹配某一列和和范围匹配另外一列
索引失效的几种情况：

使用like  '%   '进行查询模糊查询
组合索引不符合最左匹配原则
使用了or关键字
where之后的使用了函数
mysql内部有一个优化器，在进行查询的时候，会把使用普通索引、主键索引、全表扫描的消耗都计算出来选择最优的方法，在某些情况下，全表扫描的性能更优就会出现索引失效。
5 聚簇索引和非聚簇索引（针对B+树索引）

无论是聚簇索引还是非聚簇索引，都不是一种单独的数据结构，而是一种数据存储方式。

聚簇索引：聚簇索引的叶子节点就是数据节点。在InnoDB引擎就是聚簇索引，聚簇索引默认是主键（如果表中没有定义主键，InnoDB会选择一个唯一的非空索引代替，也可以自己设置聚簇索引），一张表内只能有一个聚簇索引，在聚簇索引之上创建的索引称之为辅助索引，辅助索引访问数据需要二次查找。辅助索引叶子节点存储的不再是行的物理位置，而是主键值。通过辅助索引首先找到的是主键值，再通过主键值找到数据。

非聚簇索引：非聚簇索引（主要是为了区别聚簇索引，MyISAM引擎用的就是非聚簇索引）的叶子节点仍然是索引节点，只不过有指向对应数据块的指针。

优点：聚簇索引数据访问更快，因为聚簇索引将索引和数据保存在同一个B+树中；

缺点：聚簇索引更新代价特别高。

6 数据库引擎（主要就是MyISAM和InnoDB的区别）

区别

1. InnoDB支持事务，MyISAM不支持，对于InnoDB每一条SQL语言都默认封装成事务，自动提交，这样会影响速度，所以最好把多条SQL语言放在begin和commit之间，组成一个事务； 

2. InnoDB支持外键，而MyISAM不支持。对一个包含外键的InnoDB表转为MYISAM会失败； 

3. InnoDB是聚集索引，使用B+Tree作为索引结构，数据文件是和（主键）索引绑在一起的（表数据文件本身就是按B+Tree组织的一个索引结构），必须要有主键，通过主键索引效率很高。但是辅助索引需要两次查询，先查询到主键，然后再通过主键查询到数据。因此，主键不应该过大，因为主键太大，其他索引也都会很大。

       MyISAM是非聚集索引，也是使用B+Tree作为索引结构，索引和数据文件是分离的，索引保存的是数据文件的指针。主键索引和辅助索引是独立的。
    
       也就是说：InnoDB的B+树主键索引的叶子节点就是数据文件，辅助索引的叶子节点是主键的值；而MyISAM的B+树主键索引和辅助索引的叶子节点都是数据文件的地址指针。（具体参考上面的聚簇索引和非聚簇索引）

4. InnoDB不保存表的具体行数，执行select count(*) from table时需要全表扫描。而MyISAM用一个变量保存了整个表的行数，执行上述语句时只需要读出该变量即可，速度很快（注意不能加有任何WHERE条件）；

那么为什么InnoDB没有了这个变量呢？

    因为InnoDB的事务特性，在同一时刻表中的行数对于不同的事务而言是不一样的，因此count统计会计算对于当前事务而言可以统计到的行数，而不是将总行数储存起来方便快速查询。InnoDB会尝试遍历一个尽可能小的索引除非优化器提示使用别的索引。如果二级索引不存在，InnoDB还会尝试去遍历其他聚簇索引。
    如果索引并没有完全处于InnoDB维护的缓冲区（Buffer Pool）中，count操作会比较费时。可以建立一个记录总行数的表并让你的程序在INSERT/DELETE时更新对应的数据。和上面提到的问题一样，如果此时存在多个事务的话这种方案也不太好用。如果得到大致的行数值已经足够满足需求可以尝试SHOW TABLE STATUS


5. Innodb不支持全文索引，而MyISAM支持全文索引，在涉及全文索引领域的查询效率上MyISAM速度更快高；PS：5.7以后的InnoDB支持全文索引了

6. MyISAM表格可以被压缩后进行查询操作

7. InnoDB支持表、行(默认)级锁，而MyISAM支持表级锁

       InnoDB的行锁是实现在索引上的，而不是锁在物理行记录上。潜台词是，如果访问没有命中索引，也无法使用行锁，将要退化为表锁。

    t_user(uid, uname, age, sex) innodb;

    uid PK

    无其他索引

    update t_user set age=10 where uid=1;             命中索引，行锁。


    update t_user set age=10 where uid != 1;           未命中索引，表锁。

    update t_user set age=10 where name='chackca';    无索引，表锁。


8、InnoDB表必须有唯一索引（如主键）（用户没有指定的话会自己找/生产一个隐藏列Row_id来充当默认主键），而Myisam可以没有

9、Innodb存储文件有frm、ibd，而Myisam是frm、MYD、MYI

        Innodb：frm是表定义文件，ibd是数据文件
    
        Myisam：frm是表定义文件，myd是数据文件，myi是索引文件


如何选择：
    1. 是否要支持事务，如果要请选择innodb，如果不需要可以考虑MyISAM；

    2. 如果表中绝大多数都只是读查询，可以考虑MyISAM，如果既有读也有写，请使用InnoDB。
    
    3. 系统奔溃后，MyISAM恢复起来更困难，能否接受；
    
    4. MySQL5.5版本开始Innodb已经成为Mysql的默认引擎(之前是MyISAM)，说明其优势是有目共睹的，如果你不知道用什么，那就用InnoDB，至少不会差。


InnoDB为什么推荐使用自增ID作为主键？

    答：自增ID可以保证每次插入时B+索引是从右边扩展的，可以避免B+树和频繁合并和分裂（对比使用UUID）。如果使用字符串主键和随机主键，会使得数据随机插入，效率比较差


7 主从复制（读写分离、数据备份）

概念： mysql主从分离其实也就是读写分离，将读操作和协操作分别导入到不同的服务器集群；

 原理： 主从分离是如何工作的

      在主从分离里面有主服务器（master）和从服务器（slaver），如图，其工作步骤主要分为三步：

首先主服务器（master）将对数据的操作都记录到二进制日志（binary log）中。
从服务器（slaver）将binary log拷贝到其中继日志（Relay log）中
slaver重做Relay log里面的事件，更新slaver里面的数据与master达到数据一致。
binary log有三种形式，分别是：

statement:记录的是修改SQL语句
row：记录的是每行实际数据的变更
mixed：statement和row模式的混合
       Mysql在5.0这个版本以前，binlog只支持STATEMENT这种格式！而这种格式在读已提交(Read Commited)这个隔离级别下主从复制是有bug的，因此Mysql将可重复读(Repeatable Read)作为默认的隔离级别！

7 分库、分表、分区

8 SQL优化：

           在说mysql优化之前，首先谈一谈explain关键字，如果面试的时候说mysql优化提到了explain关键字可能会给面试官你是真的做过mysql优化的感觉。
    
          explain：explain被称为执行计划，如果在select语句前放上关键词explain，mysql将解释它如何处理select，提供有关表如何联接和联接的次序。

EXPLAIN SELECT * FROM tb_area,tb_shop WHERE tb_area.area_id=tb_shop.area_id
          在select语句前面加 EXPLAIN 关键字，会出现如下

     explain属性：
     id | select_type | table | type | possible_keys | key | key_len | ref | rows | Extra |

 id

1. id相同时，执行顺序由上至下

2. 如果是子查询，id的序号会递增，id值越大优先级越高，越先被执行

3.id如果相同，可以认为是一组，从上往下顺序执行；在所有组中，id值越大，优先级越高，越先执行

 select_type   查询中每个select子句的类型

(1) SIMPLE(简单SELECT,不使用UNION或子查询等)

(2) PRIMARY(查询中若包含任何复杂的子部分,最外层的select被标记为PRIMARY)

(3) UNION(UNION中的第二个或后面的SELECT语句)

(4) DEPENDENT UNION(UNION中的第二个或后面的SELECT语句，取决于外面的查询)

(5) UNION RESULT(UNION的结果)

(6) SUBQUERY(子查询中的第一个SELECT)

(7) DEPENDENT SUBQUERY(子查询中的第一个SELECT，取决于外面的查询)

(8) DERIVED(派生表的SELECT, FROM子句的子查询)

(9) UNCACHEABLE SUBQUERY(一个子查询的结果不能被缓存，必须重新评估外链接的第一行)

Table:输出行所引用的表

表示MySQL在表中找到所需行的方式，又称“访问类型”。

常用的类型有： ALL, index,  range, ref, eq_ref, const, system, NULL（从左到右，性能从差到好）

ALL：Full Table Scan， MySQL将遍历全表以找到匹配的行

index: Full Index Scan，index与ALL区别为index类型只遍历索引树

range:只检索给定范围的行，使用一个索引来选择行

ref: 表示上述表的连接匹配条件，即哪些列或常量被用于查找索引列上的值

eq_ref: 类似ref，区别就在使用的索引是唯一索引，对于每个索引键值，表中只有一条记录匹配，简单来说，就是多表连接中使用primary key或者 unique key作为关联条件

const、system: 当MySQL对查询某部分进行优化，并转换为一个常量时，使用这些类型访问。如将主键置于where列表中，MySQL就能将该查询转换为一个常量system是const类型的特例，当查询的表只有一行的情况下，使用system

NULL: MySQL在优化过程中分解语句，执行时甚至不用访问表或索引，例如从一个索引列里选取最小值可以通过单独索引查找完成。

possible_keys : 指出能在该表中使用哪些索引有助于 查询。如果为空,说明没有可用的索引。

key:实际从 possible_key 选择使用的索引。 如果为 NULL,则没有使用索引。很少的情况 下,MYSQL 会选择优化不足的索引。这种情 况下,可以在 SELECT 语句中使用 USE INDEX (indexname)来强制使用一个索引或者用IGNORE INDEX(indexname)来强制 MYSQL 忽略索引

key_len:  使用的索引的长度。在不损失精确性的情况 下,长度越短越好。

ref:  显示索引的哪一列被使用了 

rows:   认为必须检查的用来返回请求数据的行数

extra中出现以下 2 项意味着  根本不能使用索引,效率会受到重大影响。应尽可能对此进行优化。

         我们可以利用EXPLAIN关键字来分析一个SELECT语句的执行情况。
    
    总的来说，SQL优化的原则有三点：1，尽量避免放弃索引而导致全表扫描；2 避免使用select *返回多余数据；3 合理建立索引

优化方式如下：

在表中建立索引，优先考虑where、group by使用到的字段。

尽量避免使用select *，返回无用的字段会降低查询效率。如下：SELECT * FROM t 

优化方式：使用具体的字段代替*，只返回使用到的字段。

尽量避免使用in 和not in，会导致数据库引擎放弃索引进行全表扫描。如下：

SELECT * FROM t WHERE id IN (2,3)

SELECT * FROM t1 WHERE username IN (SELECT username FROM t2)

优化方式：如果是连续数值，可以用between代替。如下：

SELECT * FROM t WHERE id BETWEEN 2 AND 3

如果是子查询，可以用exists代替。如下：

SELECT * FROM t1 WHERE EXISTS (SELECT * FROM t2 WHERE t1.username = t2.username)

尽量避免使用or，会导致数据库引擎放弃索引进行全表扫描。如下：

SELECT * FROM t WHERE id = 1 OR id = 3

优化方式：可以用union代替or。如下：

SELECT * FROM t WHERE id = 1

UNION

SELECT * FROM t WHERE id = 3

（PS：如果or两边的字段是同一个，如例子中这样。貌似两种方式效率差不多，即使union扫描的是索引，or扫描的是全表）

尽量避免在字段开头模糊查询，会导致数据库引擎放弃索引进行全表扫描。如下：

SELECT * FROM t WHERE username LIKE '%li%'

优化方式：尽量在字段后面使用模糊查询。如下：

SELECT * FROM t WHERE username LIKE 'li%'

尽量避免进行null值的判断，会导致数据库引擎放弃索引进行全表扫描。如下：

SELECT * FROM t WHERE score IS NULL

优化方式：可以给字段添加默认值0，对0值进行判断。如下：

SELECT * FROM t WHERE score = 0

尽量避免在where条件中等号的左侧进行表达式、函数操作，会导致数据库引擎放弃索引进行全表扫描。如下：

SELECT * FROM t2 WHERE score/10 = 9

SELECT * FROM t2 WHERE SUBSTR(username,1,2) = 'li'

优化方式：可以将表达式、函数操作移动到等号右侧。如下：

SELECT * FROM t2 WHERE score = 10*9

SELECT * FROM t2 WHERE username LIKE 'li%'

当数据量大时，避免使用where 1=1的条件。通常为了方便拼装查询条件，我们会默认使用该条件，数据库引擎会放弃索引进行全表扫描。如下：

SELECT * FROM t WHERE 1=1


# 经典Java面试题及答案（八）（1~41企业真题）

##### 1、下面程序的运行结果是

```java
public static void main(String[] args) {
    String str1 = "hello";
    String str2 = "he" + new String("llo");
    String str3 = "he" + "llo";
    System.err.println(str1 == str2);
    System.err.println(str1 == str3);
}
```

运行结果：

false

true

##### 2、HashSet 里的元素是不能重复的， 那用什么方法来区分重复与否呢?

往集合在添加元素时，调用 add(Object)方法的时候，首先会调用Object的 hashCode()方法判断hashCode 是否已经存在，如不存在则直接插入元素；如果已存在则调用Object对象的 equals()方法判断是否返回 true，如果为true则说明元素已经存在，如为false则插入元素。

##### 3、List ，Set， Map是否继承来自Collection接口? 存取元素时， 有何差异?

● List、Set 是继承 Collection 接口； Map不是。

● List：元素有放入顺序，元素可重复，通过下标来存取。

● Map：元素按键值对存取，无放入顺序。

● Set：元素无存取顺序，元素不可重复（注意：元素虽然无放入顺序，但是元素在 set 中的位置是有该元素的 hashCode 决定的，其位置其实是固定的）。

##### 4、简述 Java 中的值传递和引用传递?

按值传递是指的是在方法调用时，传递的参数是按值的拷贝传递。按值传递重要特点：传递的是值的拷贝，也就是说传递后就互不相关了示例如下：

```java
class TempTest {
    private void test1(int a) {
        a = 5;
        System.out.println("test1 方法中的 a=" + a);
    }

    public static void main(String[] args) {
        TempTest t = new TempTest();
        int a = 3;
        //传递后，test1 方法对变量值的改变不影响这里的 a
        t.test1(a);
        System.out.println("main 方法中的 a =" + a);
    }
}
```

运行结果是：

test1 方法中的 a=5

main 方法中的 a =3

按引用传递是指的是在方法调用时，传递的参数是按引用进行传递，其实传递的是引用的地址，也就是变量所对应的内存空间的地址。传递的是值的引用，也就是说传递前和传递后都指向同一个引用（也就是同一个内存空间）。示例如下：

```java
class TempTest {
    private void test1(A a) {
        a.age = 20;
        System.out.println("test1 方法中的 age=" + a.age);
    }

    public static void main(String[] args) {
        TempTest t = new TempTest();
        A a = new A();
        a.age = 10;
        t.test1(a);
        System.out.println("main 方法中的 age =" + a.age);
    }
}

class A {
    public int age = 0;
}
```

运行结果：

test1 方法中的 age=20

main 方法中的 age =20

##### 5、请写出几个常见的运行时异常？

NullPointerException - 空指针引用异常

ClassCastException - 类型强制转换异常。

IndexOutOfBoundsException - 下标越界异常

NumberFormatException - 数字格式异常

##### 6、简述数据库事务和实际工作中的作用？

所谓事务是用户定义的一个数据库操作序列，这些操作要么全做要么全不做，是一个不可分割的工作单位。

例如在关系数据库中，一个事务可以是一条 SQL 语句、一组 SQL 语句或整个程序。简单举个例子就是你要同时修改数据库中两个不同表的时候，如果它们不是一个事务的话，当第一个表修改完，可是第二表改修出现了异常而没能修改的情况下，就只有第二个表回到未修改之前的状态，而第一个表已经被修改完毕。而当你把它们设定为一个事务的时候，当第一个表修改完，可是第二表改修出现了异常而没能修改的情况下，第一个表和第二个表都要回到未修改的状态！这就是所谓的事务回滚。

例如，在将资金从一个帐户转移到另一个帐户的银行应用中，一个帐户将一定的金额贷记到一个数据库表中，同时另一个帐户将相同的金额借记到另一个数据库表中。由于计算机可能会因停电、网络中断等而出现故障，因此有可能更新了一个表中的行，但没有更新另一个表中的行。如果数据库支持事务，则可以将数据库操作组成一个事务，以防止因这些事件而使数据库出现不一致。如果事务中的某个点发生故障，则所有更新都可以回滚到事务开始之前的状态。如果没有发生故障，则通过以完成状态提交事务来完成更新。

##### 7、已知一棵二叉树，如果先序遍历的节点顺序是：ADCEFGHB ，中序遍历是：CDFEGHAB ，则后序遍历结果为：（D）

A.CFHGEBDA

B.CDFEGHBA

C.FGHCDEBA

D. CFHGEDBA

原因：对于二叉树的遍历方式一般分为三种先序、中序、后序三种方式：

先序遍历（根左右）若二叉树为空，则不进行任何操作，否则

● 访问根结点。

● 先序方式遍历左子树。

● 先序遍历右子树。

中序遍历 （左根右）若二叉树为空，则不进行任何操作，否则

● 中序遍历左子树。

● 访问根结点。

● 中序遍历右子树。

后序遍历 （左右根）若二叉树为空，则不进行任何操作，否则

● 后序遍历左子树。

● 后序遍历右子树。

● 访问根结点。

因此，根据题目给出的先序遍历和中序遍历，可以画出二叉树：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190510/1557477130@37181b3f4b77ffcd1e1b992a8a5cca95.png)

##### 8、下列哪两个数据结构，同时具有较高的查找和删除性能？

A.有序数组

B.有序链表

C.AVL 树

D.Hash 表

看下图：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190510/1557477198@c9d611e91dbef6eb396de5617e40bd40.png)

平衡二叉树的查找，插入和删除性能都是 O(logN)  ，其中查找和删除性能较好；哈希表的查找、插入和删除性能都是 O(1) ，都是最好的。所以最后的结果选择：CD。

##### 9、下列排序算法中，哪些时间复杂度不会超过 nlogn？（BC）

A.快速排序

B.堆排序

C.归并排序

D.冒泡排序

看下图：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190510/1557477265@99f6c5f13536113eaa8104f3b59d03ce.png)

根据上图，观察平均情况，最好最差情况的时间复杂度基本可以知道答案了，最后结果选择： BC。

##### 10、初始序列为 1 8 6 2 5 4 7 3 一组数采用堆排序，当建堆（小根堆）完毕时，堆所对应的二叉树中序遍历序列为：（ A ）

A. 8 3 2 5 1 6 4 7

B. 3 2 8 5 1 4 6 7

C. 3 8 2 5 1 6 7 4

D. 8 2 3 5 1 4 7 6

初始化序列：1 8 6 2 5 4 7 3，小根堆就是要求结点的值小于其左右孩子结点的值，左右孩子的大小没有关系，那么小根堆排序之后为：1 2 4 3 5 6 7 8；中序遍历：左根右，故遍历结果为：8 3 2 5 1 6 4 7，故最后选择的结果：A

##### 11、当 n = 5 时，下列函数的返回值是：（A）

```java
int foo(int n) {
    if (n < 2) return n;
    return foo(n - 1) + foo(n - 2);
}
```

A．5

B．7

C．8

D．1

##### 12、S市 A ，B 共有两个区，人口比例为 3：5 ，据历史统计 A 区的犯罪率为 0.01% ，B 区为 0.015% ，现有一起新案件发生在 S 市，那么案件发生在A 区的可能性有多大？

A．37.5%

B．32.5%

C．28.6%

D．26.1%

做这道题首先得了解犯罪率是什么？犯罪率就是犯罪人数与总人口数的比。因此可以直接得出公式：( 3 0.01% ) / ( 30.01% + 5 * 0.015% ) = 28.6%，当然如果不好理解的话，我们可以实例化，比如B区假设 5000 人，A区 3000 人，A 区的犯罪率为 0.01%，那么 A 区犯罪人数为 30 人，B 区的犯罪率为 0.015% ，那么 B 区的犯罪人数为 75 人 ，求发生在 A 区的可能性，就是说 A 区的犯罪人数在总犯罪人数的多少，也就是 30/(30+75)=0.2857，当然，也可以回归到我们高中遗忘的知识：

假设 C 表示犯案属性

在 A 区犯案概率：P(C|A)=0.01%

在 B 区犯案概率：P(C|B)=0.015%

在 A 区概率：P(A)=3/8

在 B 区概率：P(B)=5/8

犯案概率：P(C)=（3/80.01%+5/80.015%)

根据贝叶斯公式：P(A|C) = P(A，C) / P(C) = [P(C|A) P(A)] / [ P(C|A) P(A)+ P(C|B) P(B) ] 也可以算出答案来，最后结果选择为： C

##### 13、Linux系统中，哪些可以用于进程间的通信？（ABCD ）

A．Socket

B．共享内存

C．消息队列

D．信号量

管道（Pipe）及有名管道（named pipe）：管道可用于具有亲缘关系进程间的通信，有名管道克服了管道没有名字的限制，因此，除具有管道所具有的功能外，它还允许无亲缘关系进程间的通信；

信号（Signal）：信号是比较复杂的通信方式，用于通知接受进程有某种事件发生，除了用于进程间通信外，进程还可以发送信号给进程本身；linux 除了支持 Unix 早期信号语义函数 sigal 外，还支持语义符合 Posix.1 标准的信号函数 sigaction（实际上，该函数是基于 BSD 的，BSD 为了实现可靠信号机制，又能够统一对外接口，用 sigaction 函数重新实现了 signal 函数）；

报文（Message）队列（消息队列）：消息队列是消息的链接表，包括 Posix 消息队列 system V 消息队列。有足够权限的进程可以向队列中添加消息，被赋予读权限的进程则可以读走队列中的消息。消息队列克服了信号承载信息量少，管道只能承载无格式字节流以及缓冲区大小受限等缺点。

共享内存：使得多个进程可以访问同一块内存空间，是最快的可用 IPC 形式。是针对其他通信机制运行效率较低而设计的。往往与其它通信机制，如信号量结合使用，来达到进程间的同步及互斥。

信号量（semaphore）：主要作为进程间以及同一进程不同线程之间的同步手段。

套接口（Socket）：更为一般的进程间通信机制，可用于不同机器之间的进程间通信。起初是由 Unix 系统的 BSD分支开发出来的，但现在一般可以移植到其它类 Unix 系统上：Linux 和 System V 的变种都支持套接字。

故最后选择的结果为： ABCD

##### 14、静态变量通常存储在进程哪个区？（ C ）

A．栈区

B．堆区

C．全局区

D．代码区

原因：静态变量的修饰关键字：static，又称静态全局变量。故最后选择的结果为： C

##### 15、如何提高查询 Name 字段的性能（ B ）

A．在 Name 字段上添加主键

B．在 Name 字段上添加索引

D．在 Age 字段上添加索引

##### 16、IP地址 192.168.14.69 是一个（B）类 IP 地址

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190510/1557477537@f5f312c1b9f34fc581bb5ac1ddaa5ad4.png)

##### 17、浏览器访问某页面，HTTP协议返回状态码为 403 时表示：（ B ）

A.找不到该页面

B.禁止访问

C.内部服务器访问

D.服务器繁忙

##### 18、TCP 和 IP 分别对应了 OSI 中的哪几层？（CD）

A.Application layer

B.Presentation layer

C.Transport layer

D.Network layer

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190510/1557477610@ccfcbb4bb2e1f2fccf5255b445050458.png)

19、一个栈的入栈序列是 A，B，C，D，E，则栈的不可能的输出序列是？（C）

A．EDCBA

B．DECBA

C．DCEAB

D．ABCDE

堆栈分别是先进后出，后进先出，选项 a 是 abcde 先入栈，然后依次出栈，正好是 edcba。选项 b 是 abcd 先依次入栈，然后d出栈， e再入栈， e出栈选项c是错误的，不可能a先出栈。选项 d 是 a 入栈，然后 a 出栈；b 再入栈， b 出栈。依此类推。最后的结果选择 C。

##### 20、同一进程下的线程可以共享以下？（BD）

A．stack

B．data section

C．register set

D．file fd

**● 线程共享的内容包括：**

进程代码段

进程的公有数据(利用这些共享的数据，线程很容易的实现相互之间的通讯)

进程打开的文件描述符

信号的处理器

进程的当前目录和进程用户 ID 与进程组 ID

**● 线程独有的内容包括：**

线程 ID

寄存器组的值

线程的堆栈

错误返回码

线程的信号屏蔽码

所以选择为 BD。

##### 21、递归函数最终会结束，那么这个函数一定？（B）

A.使用了局部变量

B.有一个分支不调用自身

C.使用了全局变量或者使用了一个或多个参数

D.没有循环调用

##### 22、编译过程中，语法分析器的任务是（BCD）

A.分析单词是怎样构成的

B.分析单词串是如何构成语言和说明的

C.分析语句和说明是如何构成程序的

D.分析程序的结构

● 词法分析（lexical analysis）词法分析是编译过程的第一个阶段。这个阶段的任务是从左到右的读取每个字符，然后根据构词规则识别单词。词法分析可以用 lex 等工具自动生成。

● 语法分析（syntax analysis）语法分析是编译过程的一个逻辑阶段。语法分析在词法分析的基础上，将单词序列组合成各类语法短语，如“程序”，“语句”，“表达式”等等。语法分析程序判断程序在结构上是否正确。

● 语义分析（semantic analysis）属于逻辑阶段。对源程序进行上下文有关性质的审查，类型检查。如赋值语句左右端类型匹配问题。

所以 BCD 都属于词法分析，选择结果为 BCD。

##### 23、同步机制应该遵循哪些基本准则？（ABCD）

A．空闲让进

B．忙则等待

C．有限等待

D．让权等待

##### 24、进程进入等待状态有哪几种方式？（D）

A.CPU 调度给优先级更高的线程

B.阻塞的线程获得资源或者信号

C.在时间片轮转的情况下，如果时间片到了

D.获得 spinlock 未果

##### 25、设计模式中，属于结构型模式的有哪些？（BC）

A.状态模式

B.装饰模式

C.代理模式

D.观察者模式

##### 26、下面代码的运行结果是（C）

```java
public class Test {
    public static void main(String[] args) {
        List<String> a = null;
        test(a);
        System.out.println(a.size());
    }

    public static void test(List<String> a) {
        a = new ArrayList<String>();
        a.add("abc");
    }
}
```

A．０

B．１

C．java.lang.NullPointerException

D．以上都不正确

##### 27、Linux下查看进程占用的 CPU 的百分比，使用工具（A）

A.ps

B.cat

C.more

D.sep

##### 28、JVM 内存里哪个区域不可能发生OutOfMerncyError( A)

A.程序计数器

B.堆

C.方法区

D.本地方法栈

##### 29、下面关于阻塞队列（java.util.concurrent.BlockingQueue）的说法不正确的是(C)

A.阻塞队列是线程安全的

B.阻塞队列的主要应用场景是“生产者-消费者”模型

C.阻塞队列里的元素不能为 null

D.阻塞队列的实现必须显示地设置容量

##### 30、如果现在需要创建一组任务，他们并行的执行工作，然后进行下一个步骤之前等待，直至所有的任务都完成，而去这种控制可以重用多次， 这种情形使用 java.util.concurrent包中引入哪种同步工具最适合（B）

A.CountDownLatch

B.CyclicBarrier

C.Semaphore

D.FutureTask

##### 31、java 中， 为什么基类不能做为 HashMap 的键值， 而只能是引用类型，把引用类型作为 HashMap 的键值，需要注意哪些地方？

引用类型和原始类型的行为完全不同，并且它们具有不同的语义。引用类型和原始类型具有不同的特征和用法，它们包括：大小和速度问题，这种类型以哪种类型的数据结构存储，当引用类型和原始类型用作某个类的实例数据时所指定的缺省值。对象引用实例变量的缺省值为 null，而原始类型实例变量的缺省值与它们的类型有关。

##### 32、编写一个工具类 StringUtil， 提供方法 int compare(char[] v1 ，char[] v2)方法，比较字符串v1，v2 ，如果按照字符顺序 v1>v2 则 return 1 ，v1=v2 则 return 0， v1<v2 则 return-1 。

```java
class StringUtil {
    int compare(char[] v1, char[] v2) {
        String str1 = new String(v1);
        String str2 = new String(v2);
        int result = str1.compareTo(str2);
        return result == 0 ? 0 : (result > 0 ? 1 : -1);
    }
}
```

##### 33、Java 出现 OutOfMemoryError(OOM)的原因有那些？出现 OOM 错误后，怎么解决？

触发 java.lang.OutOfMemoryError:最常见的原因就是应用程序需要的堆空间是大的，但是 JVM 提供的却小。这个的解决方法就是提供大的堆空间即可。

除此之外还有复杂的原因：内存泄露：特定的编程错误会导致你的应用程序不停的消耗更多的内存，每次使用有内存泄漏风险的功能就会留下一些不能被回收的对象到堆空间中，随着时间的推移，泄漏的对象会消耗所有的堆空间，最终触发java.lang.OutOfMemoryError: Java heap space 错误。

解决方案：你应该确保有足够的堆空间来正常运行你的应用程序，在 JVM 的启动配置中增加如下配置：-Xmx1024m，流量/数据量峰值：应用程序在设计之初均有用户量和数据量的限制，某一时刻，当用户数量或数据量突然达到一个 峰 值 ， 并 且 这 个 峰 值 已 经 超 过 了 设 计 之 初 预 期 的 阈 值 ， 那 么 以 前 正 常 的 功 能 将 会 停 止 ， 并 触 发java.lang.OutOfMemoryError: Java heap space 异常解决方案，如果你的应用程序确实内存不足，增加堆内存会解决 GC overhead limit 问题，就如下面这样，给你的应用程序 1G 的堆内存：java -Xmx1024m com.yourcompany.YourClass。

##### 34、下列关于栈的描述错误的是（B）

A.栈是先进后出的线性表

B.栈只能顺序存储

C.栈具有记忆功能

D.对栈的插入和删除操作中，不需要改变栈底指针

##### 35、对于长度为 n 的线性表，在最坏的情况下，下列个排序法所对应的比较次数中正确的是（D）

A.冒泡排序为 n/2

B.冒泡排序为 n

C.快速排序为 n

D.快速排序为 n(n-1)/2

##### 36、阅读下列代码后，下列正确的说法是（A）

```java
class Person {
    int arr[] = new int[10];
    public static void main(String args[]) {
        System.out.println(arr[1]);
    }
}
```

A.编译时将产生错误

B.编译时正确，运行时将产生错误

C.输出空

D .输 出 0

##### 37、执行以下程序后输出的结果是（D）

```java
public class Test {
    public static void main(String[] args) {
        StringBuffer a = new StringBuffer("A");
        StringBuffer b = new StringBuffer("B");
        operator(a, b);
        System.out.println(a + "," + b);
    }

    public static void operator(StringBuffer x, StringBuffer y) {
        x.append(y);
        y = x;
    }
}
```

A.A,A

B.A,B

C.B,B

D.AB,B

##### 38、下列不属于持久化的是（A）

A.把对象转换成为字符串的形式通过网络传输，在另一端接收到字符串把对象还原出来

B.把程序数据从数据库中读出来

C.从 XML 配置文件中读取程序的配置信息

D.把程序数据保存为文件

##### 39、下列代码输出的结果是（C）

```java
int x = 0;
int y = 10;
do {
    y--;
    ++x;
} while (x < 6);
System.out.println(x + "," + y);
```

A. 5,6

B. 5,5

C. 6,4

D. 6,6

##### 40、下列程序段输出的结果是（B）

```java
public static void complicatedexpression_f() {
    int x = 20, y = 30;
    boolean j;
    j = x > 50 && y > 60 || x > 50 && y < -60 || x < -50 && y > 60 || x < -50 && y < -60;
    System.out.println(j);
}
```

A.true

B.false

C.1  

D.001

##### 41、一个栈的输入序列为123，则下列序列中不可能是栈输出的序列的是（C）

A. 2 3 1

B. 3 2 1

C. 3 1 2

D. 1 2 3

# 初级Java工程师面试题（42~81企业真题）

42、设有一个二维数组 A[m][n]，假设A[0][0]存放的位置在 644（10），A[2][2]存放的文职在676（10）每个元素占一个空间，问 A[3][3]（10）存放在什么位置? 脚注（10）表示用 10进制表示（C）

A.688

B.678

C.692

D.699

##### 43、下列代码执行结果是（B）

```java
public static void main(String args[]) {
    Thread t = new Thread() {
        public void run() {
            pong();
        }
    };
    t.run();
    System.out.print("ping");
}
static void pong() {
    System.out.print("pong");
}
```

A.pingpong

B.pongping

C.pingpong 和 pongping 都有可能

D.都有可能

##### 44、下面程序能正常运行吗（可以）

```java
class NULL {
    public static void haha() {
        System.out.println("haha");
    }
    public static void main(String[] args) {
        ((NULL) null).haha();
    }
}
```

##### 45、解释一下什么是 Servlet， 说一说 Servlet 的生命周期

Servlet 是一种服务器端的 Java 应用程序，具有独立于平台和协议的特性，可以生成动态的 Web 页面。 它担当客户请求（Web 浏览器或其他 HTTP 客户程序）与服务器响应（HTTP 服务器上的数据库或应用程序）的中间层。Servlet是位于 Web 服务器内部的服务器端的 Java 应用程序，与传统的从命令行启动的 Java 应用程序不同，Servlet 由 Web 服务器进行加载，该 Web 服务器必须包含支持 Servlet 的 Java 虚拟机。

Servlet 生命周期可以分成四个阶段：加载和实例化、初始化、服务、销毁。当客户第一次请求时，首先判断是否存在 Servlet 对象，若不存在，则由 Web 容器创建对象，而后调用 init()方法对其初始化，此初始化方法在整个 Servlet 生命周期中只调用一次。完成 Servlet 对象的创建和实例化之后，Web 容器会调用 Servlet 对象的 service()方法来处理请求。当 Web 容器关闭或者 Servlet 对象要从容器中被删除时，会自动调用 destory()方法。

##### 46、过滤器有哪些作用和用法？

对于一个 web 应用程序来说，过滤器是处于 web 容器内的一个组件，它会过滤特定请求资源请求信息和响应信息。一个请求来到时，web 容器会判断是否有过滤器与该信息资源相关联，如果有则交给过滤器处理，然后再交给目标资源，响应的时候则以相反的顺序交给过滤器处理，最后再返回给用户浏览器。

常见的过滤器用途主要包括：对用户请求进行统一认证、对用户的访问请求进行记录和审核、对用户发送的数据进行过滤或替换、转换图象格式、对响应内容进行压缩以减少传输量、对请求或响应进行加解密处理、触发资源访问事件等。

##### 47、写出一个冒泡排序

```java
public static void bubbleSort() {
    int arr[] = {-5, 29, 7, 10, 5, 16};
    for (int i = 1; i < arr.length; i++) {
        for (int j = 0; j < arr.length - i; j++) {
            if (arr[j] < arr[j + 1]) {
                int temp;
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
    for (int i = 0; i < arr.length; i++) {
        System.out.print(" " + arr[i] + " ");
    }
}
```

##### 48、写出一个单例的实现（懒加载方式）

```java
class LazySingleton {
    private LazySingleton() {
    }

    private static class SingletonHolder {
        private static LazySingleton instance = new LazySingleton();
    }

    public static LazySingleton getInstance() {
        return SingletonHolder.instance;
    }
}
```

 49、2006 年某人连续打工 24 天，共赚了 190 元（日工资 10 元，星期日工资 5 元，星期日休息无工资）。已知他打工是从 1 月下旬的某一天开始的，这个月的 1 日恰好是星期日， 这人打工结束的那一天是 2 月（C）日

A.2 月 6 日

B.2 月 14 日

C.2 月 18 日

D.2 月 21 日

##### 50、由甲地到乙地有一天线路的巴士，全程行驶时间 42 分钟，到达总站后，司机至少休息 10 分钟，巴士就掉头行驶，如果这条线路甲，乙两边总站每隔 8 分钟都发一辆（不必是同一时间），则这条线路至少需要是多少俩巴士（C）

A.15

B.14

C.13

D.12

##### 51、编号为 1 至 10 的 10 个果盘中，每盘都盛有水果，共盛放 100 个。其中第一盘里有 16 个，并且编号相邻的三个果盘中水果是的和都相等，求第 8 盘中水果最多可能有几个（A）

A.11

B.12

C.13

D.14

##### 52、假设一个池塘，里面有无穷多的水，现在有 2 个空水壶，容积分别是 5 升和 6 升，问如何用这两只水壶取得 3 升水。

5L 桶打满水，全部倒入 6L 桶；

5L 桶再次打满，往 6L 桶倒水至其满。此时 5L 桶留下 4L 水；

6L 桶清空，将 5L 桶中的 4L 水倒入 6L 桶；

5L 桶打满水，往 6L 桶倒水至其满，则 5L 桶中得 3L 水。

##### 53、在房里有三盏灯，房外有三个开关，在房外看不见房内的情况，你只能进门一次，你用什么方法来区分那个开关控制哪一盏灯。

先打开第一个开关，开一会再关上，然后打开第二个开关进入房间再摸一下每个灯，发热的那盏是第一个开关的，亮的那盏是第二个开关的，没变化的那盏是第三个开关的。

##### 54、两个盲人，他们各自买个两双黑袜和白袜，8 双袜子的布质，大小完全相同，每双袜子都有 1 张商标纸连着，两位盲人不小心把 8 双袜子混在的一起，问他们怎样才能取回黑袜和白袜各两双。

把每双袜子分成两只。每人各拿一只。这样，每人手中就有四只黑袜，四只白袜。每人也就有两双黑袜，两双白袜了。

##### 55、一楼到十楼的每层电梯门口都方和一颗钻石，钻石大小不一，你乘坐电梯从一楼到十楼，每层楼电梯门都会打开一次，手里只能拿一颗钻石，问怎样才能拿到最大的钻石。

电梯每层都会开一下的，所以，在第一层就拿，到第二层，看到更大就换一下，更小就不换，一直这样上去，到最上层后，拿到的就是最大的。

##### 56、ArrayList list = new ArrayList(20);语句中的 list 集合大小扩充了几次（A）

A.0

B.1

C.2

D.3

##### 57、如果去掉了 main 方法的 static 修饰符会怎样（B）

A.程序无法翻译。

B.程序能正常编译，运行时或抛出 NoSuchMethodError 异常。

C.程序能正常编译，正常运行。

D.程序能正常编译，正常运行一会会立刻退出。

##### 58、启动 java 程序进程时，输入一下哪个参数可以实现年轻代的堆大小为 50M(C )

A.-Xms50M

B.-Xmx50M

C.-Xmn50M

D.-Xss50M

##### 59、下面程序输出的结果是（A）

```java
static boolean foo(char c) {
    System.out.print(c);
    return true;
}

public static void main(String[] args) {
    int i = 0;
    for (foo('A'); foo('B') && (i < 2); foo('C')) {
        i++;
        foo('D');
    }
}
```

A.ABDCBDCB

B.ABDCDBCB

C.ABDBCDCB

D.ABDBCDCB

##### 60、下面哪些是 Thread 类的方法（A，B，D）

A.start()

B.run()

C.exit()

D.getPriority()

##### 61、以下语句输出的结果是什么（C）

System.out.print(Integer.MAX_VALUE*2);

System.out.print(Integer.MIN_VALUE*2);

A.-2 -1

B.-1 -2

C.-2 0

D.-1 -1

##### 62、log4j的优先级从高到低的排序为（A）

A.error>warn>info>debug

B.warn>info>debug>error

C.warn >debug>error>info

D.error>warn>debug>info

##### 63、下列哪些方法可以使线程从运行状态进入到阻塞状态（BCD）

A.notify

B.wait

C.sleep

D.yield

##### 64、下列关于 Thread 类提供的线程控制的方法中，错误的一项是（A）

A.在线程A中执行线程 B 的 join()方法，则线程 A 等待直到 B 执行完成。

B.线程A通过调用 interrupt()方法来中断其阻塞状态。

C.currentThread()方法返回当前线程的引用。

D.若线程A调用方法 isAlive()返回为 true，则说明A正在执行中。

##### 65、设String s1 =”Topwalk”;String s2 =”Company”; 以 下 方 法 可 以 得 到 字 符 串“TopwalkCompany” 有：（ABD）

A.s2+s1;

B.s1.concat(s2)

C.s1.append(s2);

D.StringBuffer buf = new StringBuffer(s1); buf.append(s2);

##### 66、String a = new String(“1”+”2”)最终创建了几个对象（D）

A.1

B.2

C.3

D.4

##### 67、int 类型占用（B）个字节？

A.2

B.4

C.8

D.16

##### 68、下列那一条语句可以实现快速的复制一张数据库表（C）

A.select * into b from a where 1<>1;

B.creat table b as select * from a where 0=1;

C.insert into b as select * from a where 1<>1;

D.insert into b select * from a where 1<>1;

##### 69、属于单例模式的特点的是（ACD）

A.提供了对唯一实现的受控访问

B.允许可变数目的实例

C.单例模式的抽象层会导致单例类扩展有和那的困难

D.单例模式很容易导致数据库的连接池溢出

##### 70、选择 Oracle 的分页语句的关键字（A）

A.rownum

B.limit

C.TOP

D.pagenum

##### 71、选出可以查询出所有的表和视图的方法：（B）

A.preparedStatement.getMetaData().getTables(***);

B.connection.getMetaData().getTables(***);

C.result.getMetaData().getTables(***);

D..DiverManager.getMeta().getTables(***);

##### 72、可以监控到数据库变化的机制有哪些（ABC）

A.存储过程

B.数据库日志

C.触发器

D.物化视图

##### 73、清空表所有数据的性能最优的语句是哪一个(B)

A.delete from tsuer;

B.truncate table tuser;

C.drop table tuser;

D.delete tuser;

##### 74、文件对外共享的协议有哪几个（AB）

A.FTP

B.Windows 共享

C.TCP

D.SSH

##### 75、关于Java中特殊符号的用法正确的是（AD）

A.判断一个字符串 str 中是否含有“.”，可以根据 str.indexOf(“.”)是否等于-1 判断。

B.判断一个字符串 str 是否含有“.”，可以根据 str.indexOf(“\\.”)是否等于-1 判断。

C.根据“.”分隔字符串 str 的写法可以是 str.split(“\\.”)

D.根据“.”分隔字符串 str 的写法可以是 str.split(“.”)

##### 76、根据以下代码回答问题，放置什么方法在第 6 行，会引起编译错误的是（B）

```java
1.class Super {
2.    public float getNum() {
3.    }
4.}
5.public class Sub extends Super {
6.
7.}
```

A.public float getNum{return 4.0f;}

B.public void getNum(){}

C.public void getNum(double d()){}

D.public double getNum（float d）{return 4.0d;}

##### 77、根据以下代码回答问题：输出结果是什么？(B)

```java
class Foo {
    public static void main(String args[]) {
        try {
            return;
        } finally {
            System.out.println("Finally");
        }
    }
}
```

A.print out nothing;

B.print out “Finally”

C.编译错误

D.以上都不对

##### 78、根据以下代码回答问题，请问输出 i 和 j 的值是多少（D）

```java
int i = 1, j = 10;
do {
    if (i++ > --j) continue;
} while (i < 5);
```

A.i=6 j=5

B.i=5 j=5

C.i=6 j=4

D.i=5 j=6

##### 79、请问以下是java关键字的有？（CD）

A.run

B.low

C.import

D.implements

##### 79、以下哪些不属于约束（CD）

A.主键

B.外键

C.索引

D.唯一索引

E.not null

##### 80、下列关于数据库连接池的说法中哪个是错误的（D）

A.服务器启动时会初始建立一定数量的池连接，并一直维持不少于此数目的池连接。

B.客户端程序需要连接时，池驱动程序会返回一个使用的池连接并将其使用计数加 1。

C.如果当前没有空闲连接，驱动程序就会再新建一定数量的连接，新建连接的数量可以由配置参数决定。

D.当使用池连接调用完成后，池驱动程序将此连接标记为空间，其他调用就可以使用这个连接。

# Java基础面试题精选（82~113企业真题）

81、以下哪句是对索引的错误描述（C）

A.选择性差的索引只会降低 DML 语句的执行速度

B.选择性强的索引只有被 Access Path 使用到才是有用的索引

C.过多的索引只会阻碍性能的提升，而不是加速性能

D.在适当的时候将最常用的列放在复合索引的最前面

E.索引和表的数据都存储在同一个 Segment 中

##### 82、关于锁 locks，描述正确的是（A）

A.当一个事务在表上防止了共享锁（shared lock），其他事务，能阅读表里的数据

B.当一个事务在表上防止了共享锁（shared lock），其他事务，能更新表里的数据

C.当一个事务在表上防止了排他锁（exclusive lock），其他事务，能阅读表里的数据

D.当一个事务在表上防止了排他锁（exclusive lock），其他事务，能更新表里的数据

##### 83、如下那种情况下，Oracle不会使用 Full Table Scean(D)

A.缺乏索引，特别是在列上使用了函数，如果要利用索引，则需要使用函数索引。

B.当访问的数据占整个表中的大部分数据时。

C.如果时一个表的 high water mark 数据块数少于初始化参数 DB_FILE_MULTIBLOCK_READ_COUNT。

D.本次查询可以用到该张表的一个引用，但是该表具有多个索引包含用于过滤的字段。

##### 84、System.out.println(3/2);System.out.println(3.0/2); System.out.println(3.0/2.0); 分别会打印什么结果？

1

1.5

1.5

##### 85、SpringMVC拦截器用过吗？什么场景会用到，过滤器，拦截器，监听器有什么区别？

拦截器：是指通过统一拦截从浏览器发往服务器的请求来完成功能的增强。使用场景：解决请求的共性问题（乱码问题、权限验证问题）。

过滤器：Servlet中的过滤器Filter是实现了javax.servlet.Filter接口的服务器端程序，主要的用途是过滤字符编码、做一些业务逻辑判断等。其工作原理是只要你在web.xml文件配置好要拦截的客户端请求，它都会帮你拦截到请求，此时你就可以对请求或响应(Request、Response)统一设置编码，简化操作；同时还可进行逻辑判断，如用户是否已经登录、有没有权限访问该页面等等工作。它是随你的web应用启动而启动的，只初始化一次，以后就可以拦截相关请求，只有当你的web应用停止或重新部署的时候才销毁。

监听器：现在来说说 Servlet 的监听器 Listener，其中有一个监听器是监听上下文的，它实现了javax.servlet.ServletContextListener接口，它也是随web应用的启动而启动，只初始化一次，随web应用的停止而销毁。主要作用是：做一些初始化的内容、设置一些基本的内容、比如一些参数或者是一些固定的对象等等。

##### 86、ThreadLocal 的原理和应用场景

每一个ThreadLocal能够放一个线程级别的变量，可是它本身能够被多个线程共享使用，并且又能够达到线程安全的目的，且绝对线程安全。

ThreadLocal的应用场景：最常见的ThreadLocal使用场景为用来解决数据库连接、Session 管理等

##### 87、简述 TCP 的三次握手

在TCP/IP协议中，TCP协议提供可靠的连接服务，采用三次握手建立一个连接。

● 第一次握手：建立连接时，客户端发送 syn 包(syn=j)到服务器，并进入 SYN_SEND 状态， 等待服务器确认； SYN：同步序列编号(Synchronize Sequence Numbers)。

● 第二次握手：服务器收到 syn 包，必须确认客户的 SYN(ack=j+1)，同时自己也发送一个 SYN 包(syn=k)，即SYN+ACK 包，此时服务器进入 SYN_RECV 状态。

● 第三次握手：客户端收到服务器的 SYN＋ACK 包，向服务器发送确认包 ACK(ack=k+1)，此包发送完毕，客户端和服务器进入 ESTABLISHED 状态，完成三次握手。完成三次握手，客户端与服务器开始传送数据

##### 88、SpringMVC request接收设置是线程安全的吗？

是线程安全的，request、response 以及 requestContext 在使用时不需要进行同步。而根据 spring的默认规则，controller对于BeanFactory而言是单例的。即controller只有一个， controller 中的request等实例对象也只有一个。

##### 89、列举 Maven 常见的六种依赖范围

● compile: 编译依赖范围(默认)，对其三种都有效。

● test: 测试依赖范围，只对测试 classpath 有效。

● runtime: 运行依赖范围，只对测试和运行有效，编译主代码无效，例如 JDBC。

● provided: 已提供依赖范围，只对编译和测试有效，运行时无效，例如 selvet-api。

● system: 系统依赖范围.谨慎使用.例如本地的，maven 仓库之外的类库文件。

● import(maven2.0.9 以上): 导入依赖范围，不会对其他三种有影响。

##### 90、Mybatis 如何防止 sql 注入？mybatis 拦截器了解过吗，应用场景是什么？

Mybatis使用#{}经过预编译的，是安全的，防止sql 注入。

Mybatis拦截器只能拦截四种类型的接口：Executor、StatementHandler、ParameterHandler和ResultSetHandler。这是在Mybatis的Configuration中写死了的，如果要支持拦截其他接口就需要我们重写Mybatis的Configuration。

Mybatis可以对这四个接口中所有的方法进行拦截。

Mybatis拦截器常常会被用来进行分页处理。

91、简单解释自动装配的各种模式，或者叫装配方式

在Spring框架中共有5种自动装配：

● no：这是 Spring 框架的默认设置，在该设置下自动装配是关闭的，开发者需要自行在bean定义中用标签明确的设置依赖关系。

● byName：该选项可以根据bean名称设置依赖关系。当向一个bean中自动装配一个属性时，容器将根据bean的名称自动在在配置文件中查询一个匹配的bean。如果找到的话，就装配这个属性，如果没找到的话就报错。

● byType：该选项可以根据bean类型设置依赖关系。当向一个bean中自动装配一个属性时，容器将根据bean的类型自动在在配置文件中查询一个匹配的 bean。如果找到的话，就装配这个属性，如果没找到的话就报错。

● constructor：造器的自动装配和 byType 模式类似，但是仅仅适用于与有构造器相同参数 的bean，如果在容器中没有找到与构造器参数类型一致的bean，那么将会抛出异常。

● autodetect：该模式自动探测使用构造器自动装配或者 byType 自动装配。首先，首先会尝试找合适的带参数的构造器，如果找到的话就是用构造器自动装配，如果在bean内部没有找到相应的构造器或者是无参构造器，容器就会自动选择byTpe的自动装配方式。

##### 92、mvc的各个部分都有哪些技术来实现？如何实现的？

MVC是Model－View－Controller的简写。Model代表的是应用的业务逻辑（通过 JavaBean，EJB组件实现），View 是应用的表示面（由JSP页面产生），Controller是提供应用的处理过程控制（一般是一个Servlet），通过这种设计模型把应用逻辑，处理过程和显示逻辑分成不同的组件实现。这些组件可以进行交互和重用。

##### 93、反射机制一般应用在什么场景？

● 逆向代码，例如反编译

● 与注解相结合的框架，例如Retrofit

● 单纯的反射机制应用框架，例如EventBus 2.x

● 动态生成类框架，例如Gson

##### 94、设计Java程序，假设有50瓶饮料，喝完三个空瓶可以换一瓶饮料，依次类推，请问总共喝了多少饮料。

```java
class Buy {
    public static void main(String[] args) {
        int n = 50;
        int i = 0;
        while (true) {
            n -= 3;
            n++;
            if (n < 3) {
                System.out.println("共喝了" + (50 + i) + "瓶");
                break;
            }
        }
    }
}
```

##### 95、根据某年某月某日，输出这是一年中第几天。

```java
public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    System.out.print("请输入3个整数,分别表示年月日：");
    int year = in.nextInt();
    int month = in.nextInt();
    int day = in.nextInt();
    int sum = 0;
    int[][] a = {
            {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31},
            {31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31}
    };
    for (int i = 0; i < month - 1; i++) {
        if (year % 4 == 0 & year % 100 != 0 | year % 400 == 0)
            sum = sum + a[1][i];
        else
            sum = sum + a[0][i];
    }
    sum = sum + day;
    System.out.println(year + "年" + month + "月" + day + "是这一年的第" + sum + "天");
}
```

##### 96、利润与奖金，某公司销售 10 万元到 20 万元的奖金 10%，在 20 万元的奖金 10 万元以上的奖金 7.5%，到 40 万元超出 20 万元的部分奖金为 5%，到 60 万元的超出 40 万元的部分奖金 3%，到 100 万元的超出 60 万元部分奖金 1%，请输出说的奖金。

```java
public class Test {
    public static void main(String[] args) {
        float money = 0;
        Scanner scan = new Scanner(System.in);
        System.out.print("请输入利润：");
        float num = scan.nextInt();
        if (num <= 100000) {
            money = (float) (num * 0.1);
        } else if (num <= 200000) {
            money = (float) ((num - 100000) * 0.075 + 100000 * 0.1);
        } else if (num <= 400000) {
            money = (float) ((num - 200000) * 0.5 + 100000 * 0.175);
        } else if (num <= 600000) {
            money = (float) ((num - 400000) * 0.3 + 100000 * 0.175 + 200000 * 0.5);
        } else if (num <= 1000000) {
            money = (float) ((num - 600000) * 0.015 + 100000 * 0.175 + 200000 * 0.5 + 200000 * 0.3);
        } else {
            money = (float) ((num - 1000000) * 0.01 + 100000 * 0.175 + 200000 * 0.5 + 200000 * 0.3 + 400000 * 0.015);
        }
        System.out.println("奖金：" + money);
    }
}
```

##### 97、除了懒汉式和饿汉式你还了解那些单例模式？

```java
//双重检查锁模式
public class DoubleCheckLock {
    private static DoubleCheckLock instance = null;

    private DoubleCheckLock() {
    }

    public static DoubleCheckLock getInstance() {
        if (instance == null) {
            synchronized (DoubleCheckLock.class) {
                if (instance == null) {
                    instance = new DoubleCheckLock();
                }
            }
        }
        return instance;
    }
}

//静态内部类方式
public class StaticInner {
    private static StaticInner instance;

    public static StaticInner getInstance() {
        return SingletonHolder.STATIC_INNER;
    }

    private static class SingletonHolder {
        private static final StaticInner STATIC_INNER = new StaticInner();
    }
}
```

##### 98、简述SSH的概念以及主要的设计思想？

SSH是Struts+Spring+Hibernate的一个集成框架，是目前比较流行的一种Web应用程序开源框架。

集成SSH框架的系统从职责上分为四层：表示层、业务逻辑层、数据持久层和域模块层，以帮助开发人员在短期内搭建结构清晰、可复用性好、维护方便的 Web 应用程序。

其中使用Struts作为系统的整体基础架构，负责MVC的分离，在Struts框架的模型部分，控制业务跳转，利用Hibernate框架对持久层提供支持，Spring做管理，管理Struts和Hibernate。

具体做法是：用面向对象的分析方法根据需求提出一些模型，将这些模型实现为基本的 Java对象，然后编写基本的DAO(Data Access Objects)接口，并给出 Hibernate的DAO实现，采用Hibernate架构实现的DAO类来实现Java类与数据库之间的转换和访问，最后由Spring 做管理。

##### 99、Linux下如何让命令在后台执行？

要让程序在后台执行，只需在命令行的最后加上“&”符号。例如：$ find . -name abc -print&

##### 100、Linux中rm -i 与 rm -r 个实现什么功能？

rm –i： 交互模式删除文件，删除文件前给出提示。

rm -r：递归处理，将指定目录下的所有文件与子目录一并处理。

##### 101、什么是乐观锁，什么是悲观锁，两者的区别是什么？

悲观锁(Pessimistic Lock)，顾名思义，就是很悲观，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会block直到它拿到锁。传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁。它指的是对数据被外界（包括本系统当前的其他事务，以及来自外部系统的事务处理）修改持保守态度，因此，在整个数据处理过程中，将数据处于锁定状态。悲观锁的实现，往往依靠数据库提供的锁机制（也只有数据库层提供的锁机制才能真正保证数据访问的排他性，否则，即使在本系统中实现了加锁机制，也无法保证外部系统不会修改数据）。

乐观锁(Optimistic Lock)，顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号等机制。乐观锁适用于多读的应用类型，这样可以提高吞吐量，像数据库如果提供类似于write_condition机制的其实都是提供的乐观锁。

两种锁各有优缺点，不可认为一种好于另一种，像乐观锁适用于写比较少的情况下，即冲突真的很少发生的时候，这样可以省去了锁的开销，加大了系统的整个吞吐量。但如果经常产生冲突，上层应用会不断的进行retry，这样反倒是降低了性能，所以这种情况下用悲观锁就比较合适。

##### 102、日志打印的log4j的配置中%t表示什么？

答案：%t 输出产生该日志事件的线程名。

扩展：%M 是输出方法的名字、%m 是输出代码指定的日志信息。指定的打印信息的具体格式 ConversionPattern，具体参数：

● %m输出代码中指定的消息

● %p输出优先级，即 DEBUG，INFO，WARN，ERROR，FATAL

● %r输出自应用启动到输出该 log 信息耗费的毫秒数

● %c输出所属的类目，通常就是所在类的全名

● %t输出产生该日志事件的线程名

● %n输出一个回车换行符，Windows 平台为"rn”，Unix 平台为"n”

● %d 输出日志时间点的日期或时间，默认格式为 ISO8601，也可以在其后指定格式，比如：%d{yyyy MM dd HH:mm:ss，SSS}

● %l输出日志事件的发生位置，包括类目名、发生的线程，以及在代码中的行数

● %x输出和当前线程相关联的 NDC(嵌套诊断环境)，尤其用到像 java servlets 这样的多客户多线程的应用中。

● %%输出一个”%”字符

● %F输出日志消息产生时所在的文件名称

● %M输出执行方法

● %L输出代码中的行号

##### 103、Spring中什么时候引起NotWritablePropertyException和Could not open calss path resource[ApplicationContext.xml]

出现NotWritablePropertyException异常的原因一般是在ApplicationContext.xml中 property name的错误等相关问题。

##### 104、关于Web应用程序，下列说法错误的是（B）

A. WEB-INF目录存在于web应用的根目录下

B. WEB-INF目录与classes目录平行

C. web.xml在WEB-INF目录下

D. Web应用程序可以打包为war文件

##### 105、有关Servlet的生命周期说法正确的有（CD）

A. Servlet 的生命周期由 Servlet 实例控制

B. init()方法在创建完 Servlet 实例后对其进行初始化，传递的参数为实现 ServletContext 接口的对象

C. service()方法响应客户端发出的请求

D. destroy()方法释放Servlet实例

##### 106、有关会话跟踪技术描述正确的是（ABC）

A. Cookie是Web服务器发送给客户端的一小段信息，客户端请求时，可以读取该信息发送到服务器端

B. 关闭浏览器意味着会话 ID 丢失，但所有与原会话关联的会话数据仍保留在服务器上，直至会话过期

C. 在禁用 Cookie 时可以使用 URL 重写技术跟踪会话

D. 隐藏表单域将字段添加到 HTML 表单并在客户端浏览器中显示

##### 107、以下web.xml片断（D）正确地声明servlet上下文参数

A.

<init-param>

<param-name>MAX</param-name>

<param-value>100</param-value>

</init-param>

B.

<context-param>

<param name="MAX" value="100" />

<context-param>

C.

<context>

<param name="MAX" value="100" />

<context>

D.

<context-param>

<param-name>MAX</param-name>

<param-value>100</param-value>

<context-param>

##### 108、以下（A）可用于检索session属性userid的值

A. session. getAttribute (“userid”);

B. session. setAttribute (“userid”);

C. request. getParameter (“userid”);

D. request. getAttribute (“userid”);

##### 109、下列 JSP 代码，以下（CD）可放置在1处，不会发生编译错误

```java
<html>
<body>
<%
for(int i = 0; i < 10; i++) {
//1
}
%>
</body>
</html>
```

A. <%= i %>

B. <b>i</b>

C. %><%= i %><%

D. 不写任何内容

##### 110、考虑下面两个JSP文件代码片断：

**● test1.jsp:**

```java
<HTML>
<BODY>
<% pageContext.setAttribute(“ten”,new Integer(10));%>
//1
</BODY>
</HTML>
```

**● test2.jsp:**

```java
数字为：<%= pageContext.getAttribute(”ten”)%>
```

以下（C）放置在 test1.jsp 中的//1 处，当请求 test1.jsp 时正确输出 test2.jsp 中的内容。

A. <jsp:include page=”test2.jsp”  />

B. <jsp:forword page=”test2.jsp”  />

C. <%@ include file=”test2.jsp”  %>

D. pageContext对象的scope属性为page，所以test2.jsp不能访问 test1.jsp定义的属性

##### 111、有关JSP隐式对象，以下（ACD）描述正确。

A. 隐式对象是 WEB 容器加载的一组类的实例，可以直接在 JSP 页面使用

B. 不能通过config对象获取 ServletContext 对象

C. response对象通过sendRedirect方法实现重定向

D. 只有在出错处理页面才有 exception 对象

解释：jsp 的九大内置对象分别是：config、request、response、out、page、pageContext、session、exception、application。其中exception 是特殊的内置对象，只有当在 jsp 中添加 isErrorPage="true"属性时如下配置时才可以使用。该属性一般出现在设定的错误界面。

```java
<%@ page language="java" contentType="text/html; charset=ISO-8859-1" pageEncoding="ISO-8859-1" isErrorPage="true" %>
```

112、考虑下面 JSP 文件代码片断：

```java
<HTML>
<BODY>
<jsp:include page=”test2.jsp”>
    <jsp:param name=”username”  value=”zhangsan”/>
</jsp:include>
</BODY>
</HTML>
```

以下（C）代码片断放置在 test2.jsp 中不会导致错误。

A. <jsp:getParam name=”username”/>

B. <jsp:include param =”username”/>

C. <%=request.getParameter(“username”)%>

D. <%=request.getAttribute(“username”)%>

解释：<jsp:include page=”test2.jsp”>属于动态调用test2.jsp界面，相当于动态去请求test2.jsp 所生成的Servlet，在请求的同时携带了请求参数“username”，我们知道在Servlet中获取请求携带的参数就是通过request.getParameter(key)来获取的，因此C正确。

##### 113、以下是login.jsp文件的代码片断：

```java
<%@ page isELIgnored="false"%>
<html>
<body>
<FORM action="login.jsp" method="GET">
    <input type="text" name="name" value="${param['name']}">
    <input type="submit" value="提交">
</FORM>
<P>
    用户名为: ${param.name}
</body>
</html>
```



# 初级Java程序员面试题（114~130企业真题）

114、编写一个Filter，需要（B）

A. 继承 Filter 类

B. 实现 Filter 接口

C. 继承 HttpFilter 类

D. 实现 HttpFilter 接口

##### 115、有关MVC设计模式（B）描述不正确

A. 使用 Servlet 作为控制器

B. MVC 设计模式增大了维护难度

C. MVC 设计模式属于 Model 2

D. 模型对象向客户端显示应用程序界面

**● Model 1：**

Model 1的基础是JSP文件，它由一些相互独立的JSP文件，和其他一些Java Class 组成（不是必须的）。这些JSP从 HTTP Request中获得所需要的数据，处理业务逻辑，然后将结果通过 Response 返回前端浏览器。

**● Model 2：**

采用面向对象技术实现MVC模式从而扩展JSP/Servlet的模式被称为是Model 2模式。Apache Jakarta 项目中Struts是一个实现Model 2的很好的框架，它通过一些Custom Tag Lib处理表现层，用ActionFrom Bean表示数据，用自己提供的一个ActionServlet作为控制器实现页面的流转的控制功能。说的直白一些，model1即为单纯的 jsp+java，没有框架参与，通过response和request对象传送值域，而model2则使用较为流行的struts2框架（当然也可能是其他的MVC框架，例如SpringMVC）。

##### 116、在Linux中，可以使用命令（C）加挂计算机上的非Linux文件系统

A. cat /proc/filesystems

B. ln

C. mount

D. df

##### 117、下面关于Linux中shell的说法错误的是（D）

A. shell 是解释用户在终端键入的命令的一种中间程序

B. shell 可以读取并执行脚本文件中的命令

C. 用户可以使用参数将命令行的参数传递给 shell 脚本，从而实现在 Linux 中的交互式编程

D. 默认情况下，Linux 中创建的所有文件都具有执行权限

##### 118、在Oracle中，当需要使用显式游标更新或删除游标中的行时，UPDATE 或 DELETE 语句必须使用（C）子句

A. WHERE CURRENT OF

B. WHERE CURSOR OF

C. FOR UPDATE

D. FOR CURSOR OF

##### 119、在Oracle中，使用下列的语句CREATE PUBLIC SYNONYM parts FOR Scott.inventory;完成的任务是（D）

A. 将Scott.inventory 对象的访问权限赋予所有用户

B. 指定了新的对象权限

C. 指定了新的系统权限

D. 给Scott.inventory对象创建一个公用同义词 parts

##### 120、下列说法正确的有（C）

A. class中的constructor不可忽略

B. constructor可以作为普通方法被调用

C. constructor在一个对象new时被调用

D. 一个class只能定义一个 constructor

##### 121、下列运算符合法的是（D）

A、&&

B、<>

C、If

D、:=

##### 122、下列哪种说法不正确（ABC）

A、实例方法可以直接调用超类的实例方法

B、实例方法可以直接调用超类的类方法

C、实例方法可以直接调用其他类的实例方法

D、实例方法可以直接调用本类的类方法

##### 123、执行如下程序代码后，c 的值是（C）

```java
int a = 0;
int c = 0;
do {
    --c;
    a = a - 1;
} while (a > 0);
```

A、0

B、1

C、-1

D、死循环

##### 124、什么时候使用抽象类和接口

如果你拥有一些方法并且想让它们中的一些有默认实现，那么使用抽象类吧。

如果你想实现多重继承，那么你必须使用接口。由于 Java 不支持多继承，子类不能够继承多个类，但可以实现多个接口。因此你就可以使用接口来解决它。

如果基本功能在不断改变，那么就需要使用抽象类。如果不断改变基本功能并且使用接口，那么就需要改变所有实现了该接口的类。

多数情况下抽象类都是共同特征的抽象，而接口是共同行为的抽象。

##### 125、throws和throw有什么不同点？

throw和throws都是异常处理机制当中的关键字，throw是手动抛异常，throws是以声明的方式抛出异常，并且抛给调用者处理。

##### 126、请编写一个jdbc查询任意一个数据库（如 MySQL、Oracle 等），数据库名为test，表名为user，只有一个字段。

```java
class SqlConnectionUtil {
    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        String sql;
        String url = "jdbc:MySQL://localhost:3306/test?user=root&password=root&useUnicode=true&characterEncoding=UTF8";
        try {
            Class.forName("com.MySQL.jdbc.Driver");
            conn = DriverManager.getConnection(url);
            stmt = conn.createStatement();
            sql = "select * from user";
            rs = stmt.executeQuery(sql);
            while (rs.next()) {
                String name = rs.getString("name");
                System.out.println("姓名是：" + name);
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (rs != null) {
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

##### 127、写一个多线程程序，四个线程对一个int 变量，2 个加 1，2 个减1，输出。

```java
class TMain {
    int j = 1;

    public synchronized void inc() {
        j++;
        System.out.println(Thread.currentThread().getName() + "-inc:" + j);
    }

    class T1 implements Runnable {
        public void run() {
            inc();
        }
    }

    public synchronized void dec() {
        j--;
        System.out.println(Thread.currentThread().getName() + "-dec:" + j);
    }

    class T11 implements Runnable {
        public void run() {
            dec();
        }
    }

    public static void main(String[] args) {
        TMain t = new TMain();
        T1 t1 = t.new T1();
        T11 t11 = t.new T11();
        for (int i = 0; i < 2; i++) {
            Thread thread = new Thread(t1);
            thread.start();
            Thread thread1 = new Thread(t11);
            thread1.start();
        }
    }
}
```

##### 128、说出常用的10个linux操作命令，至少 5 个，并简述命令的作用。

● ls命令

－作用：显示目录内容，类似 DOS 下的 DIR

－格式：LS【options】【filename】

－常用参数：

\>-a:all，不隐藏任何以"."字符开始的文件

\>-l：使用较长的格式列出信息

\>-r:按照文件名的逆序打印输出

\>-F:加上文件类型的指示符ls -lF | grep / 过滤

● man ls 查询 ls 的帮助文件

● cat命令

－作用：显示文件内容，concatenate 的缩写，类似 dos 的 type 命令。

－格式：cat【options】【fielname】

－常用参数：

\>-n：显示文件内容的行号。

\>-b：类似-n，但是不对空白行进行编号。

\>-s：当遇到有连续两行以上的空白行时，就代换为一行的空白行。

● mv 命令

－作用：更改文件或者目录的名字。

－格式：mv[options]source destination

－常用参数：

\>-f：强制模式，覆盖文件不提示。

\>-i：交互模式，当要覆盖文件的时候给提示。

● rm 命令

－作用：删除文件命令，类似 dos 的 del 命令

－格式：rm【options】filenames

－常用参数：

\>-f：强制模式，不给提示。

\>-r，-R：删除目录，recursive

##### 129、说出常见的5个linux系统日志，至少3个并做简述日志的用途。

● access-log记录 HTTP/web 的传输

● acct/pacct记录用户命令

● aculog记录 MODEM 的活动

● btmp记录失败的纪录

● lastlog记录最近几次成功登录的事件和最后一次不成功的登录

● messages从 syslog 中记录信息（有的链接到 syslog 文件）

● sudolog记录使用 sudo 发出的命令

● sulog记录使用 su 命令的使用

● syslog从 syslog 中记录信息（通常链接到 messages 文件）

● utmp记录当前登录的每个用户

● wtmp一个用户每次登录进入和退出时间的永久纪录

● xferlog记录 FTP 会话

##### 130、创建一张员工表，表名EMPLOYEES，有四个字段，EMPLOYEE_ID: 员工表（ 主键）、DEPT_ID: 部门号、EMPLOYEE_NAME:员工姓名、EMPLOYEE_SALARY:员工工资。

● 写出建表语句：

CREATE TABLE EMPLOYEES(

EMPLOYEE_ID int not null primary key，

DEPT_ID int，

EMPLOYEE_NAME char(40)，

EMPLOYEE_SALARY double

);

● 检索出员工工资最高的员工姓名和工资

select * from user where employee_salary= (select max(employee_salary) from user);

● 检索出部门中员工最多的部门号和此部门员工数量

select dept_id，count(*) cno from user GROUP BY dept_id desc limit 1;

# Java常见面试题及答案（131~140企业真题）

131、j2ee中的应用服务器有哪些？（ACD）

A. Weblogic

B. Tomcat

C. JBoss

D. WebSphere

E. IIS

##### 132、EJB程序与普通的java程序区别有哪些？

EJB是sun的服务器端组件模型，最大的用处是部署分布式应用程序当然，还有许多方式可以实现分布式应用，类似微软的.net技术。

凭借 java 跨平台的优势，用EJB 技术部署的分布式系统可以不限于特定的平台。EJB(EnterpriseJavaBean)是J2EE的一部分，定义了一个用于开发基于组件的企业多重应用程序的标准。其特点包括网络服务支持和核心开发工具(SDK)。

在J2EE里，Enterprise Java Beans(EJB)称为Java企业Bean，是Java 的核心代码，分别是会话 Bean（Session Bean），实体 Bean（Entity Bean）和消息驱动 Bean（MessageDriven Bean）。

简单来讲：比如做一个工程就和盖房子，如果，你会 java，那么你就拥有了基本的技能，一步一步累砖，总能把房子盖好但是EJB就是一个框架，盖房子的时候，先有这个框架，然后你根据这个框架去累砖，房子就会盖的又快又好。java是基础，EJB 是在java上发展出来的模型，框架。

##### 133、请简述什么是集群？

服务器集群就是指将很多服务器集中起来一起进行同一种服务，在客户端看来就象是只有一个服务器。集群可以利用多个计算机进行并行计算从而获得很高的计算速度，也可以用多个计算机做备份，从而使得任何一个机器坏了整个系统还是能正常运行。一旦在服务器上安装并运行了群集服务，该服务器即可加入群集。群集化操作可以减少单点故障数量，并且实现了群集化资源的高可用性。

##### 134、字符串中有重复的内容去重 例如：abbccccaaddaggb-->abvadagb

```java
public class Test {
    public static void main(String[] args) {
        List<String> list = new ArrayList<String>();
        list.add("a");
        list.add("a");
        list.add("a");
        list.add("b");
        list.add("b");
        list.add("c");
        System.out.println("没有去重前的数据为>>>" + list.toString());
        for (int i = 0; i < list.size() - 1; i++) {
            for (int j = list.size() - 1; j > i; j--) {
                if (list.get(j).equals(list.get(i))) {
                    list.remove(j);
                }
            }
        }
        System.out.println("去重后的数据为>>>" + list.toString());
    }
}
```

135、利用 java 面向对象的思路设计正方形、长方形、圆的计算面积的类圆：

```java
class MianJi {
    float r;
    float pai = (float) 3.14;

    void gongShi() {
        Float s = pai * r * r;
        System.out.println("圆的面积为" + s);
    }

    void zhengFangXing(float bianChang) {
        System.out.println("正方形的面积为" + bianChang * bianChang);
    }

    void changFangXing(float chang, float kuan) {
        System.out.println("长方形的面积为" + chang * kuan);
    }

    public static void main(String[] arg) {
        MianJi c = new MianJi();
        System.out.println("请输入圆的半径：");
        Scanner sc = new Scanner(System.in);
        c.r = sc.nextFloat();
        c.gongShi();
        System.out.println("请输入正方形的边长：");
        float bian = sc.nextFloat();
        c.zhengFangXing(bian);
        System.out.println("请输入长方形的长和宽：");
        float chang = sc.nextFloat();
        float kuan = sc.nextFloat();
        c.changFangXing(chang, kuan);
    }
}
```

136、任何>=6的偶数都可以分解为两个质数之和，从键盘输入一个偶数，输出其分解的质数

```java
public class Test {
    public static void main(String[] args) {
        int num = inPut();
        outPut(num);
    }

    public static int inPut() {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入大于 6 的偶数：");
        int num = sc.nextInt();
        if (num % 2 != 0 || num <= 6) {
            System.out.println("输入错误，请重新输入大于 6 的偶数：");
            return inPut();
        }
        return num;
    }

    public static boolean isPrim(int num) {
        for (int i = 2; i <= Math.sqrt((double) num); i++) {
            if (num % i == 0) {
                return false;
            }
        }
        return true;
    }

    public static void outPut(int num) {
        for (int i = 2; i <= num / 2; i++) {
            if (isPrim(i) == true && isPrim(num - i) == true) {
                System.out.println(i + " " + (num - i));
            }
        }
    }
}
```

##### 137、什么叫对象？什么叫类？什么是面向对象（OOP）？

类的概念：类是具有相同属性和行为的一组对象的集合。它为属于该类的所有对象提供了统一的抽象描述，其内部包括属性和行为两个主要部分。在面向对象的编程语言中，类是一个独立的程序单位，它应该有一个类名并包括属性说明和服务说明两个主要部分。

对象的概念：对象是系统中用来描述客观事物的一个实体，它是构成系统的一个基本单位。一个对象由一组属性和对这组属性进行操作的一组服务组成。从更抽象的角度来说，对象是问题域或实现域中某些事物的一个抽象，它反映该事物在系统中需要保存的信息和发挥的作用；它是一组属性和有权对这些属性进行操作的一组服务的封装体。客观世界是由对象和对象之间的联系组成的。

类与对象的关系就如模具和铸件的关系，类的实例化结果就是对象，而对一类对象的抽象就是类。类描述了一组有相同特性（属性）和相同行为（方法）的对象。

上面大概就是它们的定义吧，也许你是刚接触面象对象的朋友，不要被概念的东西搞晕了， 给你举个列子吧，如果你去中关村想买几台组装的PC机，到了那里你第一步要干什么，是不是装机的工程师和你坐在一起，按你提供的信息和你一起完成一个装机的配置单呀，这个配置单就可以想像成是类，它就是一张纸，但是它上面记录了你要买的PC机的信息，如果用这个配置单买10台机器，那么这10台机子，都是按这个配置单组成的，所以说这10台机子是一个类型的，也可以说是一类的。那么什么是对象呢，类的实例化结果就是对象，用这个配置单配置出来（实例化出来）的机子就是对象，是我们可以操作的实体，10 台机子，10 个对象。每台机子都是独立的，只能说明他们是同一类的，对其中一个机（对象）做任何动作都不会影响其它9台机器，但是我对类修改，也就是在这个配置单上加一个或少一个配件，那么装出来的9个机子都改变了，这是类和对象的关系(类的实例化结果就是对象) 

##### 138、JAVA中使用final修饰符，对程序有哪些影响？

● 修饰类

当用final修饰一个类时，表明这个类不能被继承。也就是说，如果一个类你永远不会让他被继承，就可以用final进行修饰。final类中的成员变量可以根据需要设为final，但是要注意 final 类中的所有成员方法都会被隐式地指定为final 方法。在使用 final 修饰类的时候，要注意谨慎选择，除非这个类真的在以后不会用来继承或者出于安全的考虑，尽量不要将类设计为 final类。

● 修饰方法

被final修饰的方法将不能被子类覆盖，主要用于：

把方法锁定，以防任何继承类修改它的含义。

在早期的Java实现版本中，会将final方法转为内嵌调用，所以效率能够提升。

● 修饰变量

对于一个final变量，如果是基本数据类型的变量，则其数值一旦在初始化之后便不能更改；如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象。当用 final 作用于类的成员变量时，成员变量（注意是类的成员变量，局部变量只需要保证在使用之前被初始化赋值即可）必须在定义时或者构造器中进行初始化赋值，而且final变量一旦被初始化赋值之后，就不能再被赋值了。

##### 139、写出 5 个你在 JAVA 开发中常用的包含（全名），并简述其作用。

● java.lang.*

提供利用Java编程语言进行程序设计的基础类。最重要的类是Object（它是类层次结构的根）和Class（它的实例表示正在运行的应用程序中的类）。

● java.util.*

包含集合框架、遗留的Collection类、事件模型、日期和时间设施、国际化和各种实用工具类（字符串标记生成器、随机数生成器和位数组、日期 Date 类、堆栈 Stack 类、向量 Vector 类等）。集合类、时间处理模式、日期时间工具等各类常用工具包。

● java.io.*

Java 的核心库java.io提供了全面的IO接口。包括：文件读写、标准设备输出等。Java 中 IO是以流为基础进行输入输出的，所有数据被串行化写入输出流，或者从输入流读入。

● java.net.*

并非所有系统都支持 IPv6 协议，而当 Java 网络连接堆栈尝试检测它并在可用时透明地使用它时，还可以利用系统属性禁用它。在IPv6不可用或被显式禁用的情况下，Inet6Address  对大多数网络连接操作都不再是有效参数。虽然可以保证在查找主机名时 java.net.InetAddress.getByName之类的方法不返回Inet6Address，但仍然可能通过传递字面值来创建此类对象。在此情况下，大多数方法在使用 Inet6Address 调用时都将抛出异常。

● java.sql.*

提供使用JavaTM编程语言访问并处理存储在数据源（通常是一个关系数据库）中的数据的API。此API包括一个框架，凭借此框架可以动态地安装不同驱动程序来访问不同数据源。

##### 140、方法重载（overload）需要满足什么条件，方法覆盖/方法重写（override）需要满足什么条件？

重载需要满足的条件：在同一类中定义的方法，方法名必须相同，参数一定不同。

发生覆盖的条件：“三同一不低”，子类和父类的方法名称，参数列表，返回类型必须完全相同，而且子类方法的访问修饰符的权限不能比父类低。子类方法不能抛出比父类方法更多的异常。即子类方法所抛出的异常必须和父类方法所抛出的异常一致，或者是其子类，或者什么也不抛出；被覆盖的方法不能是final类型的。因为final修饰的方法是无法覆盖的。被覆盖的方法不能为private。否则在其子类中只是新定义了一个方法，并没有对其进行覆盖。被覆盖的方法不能为static。所以如果父类中的方法为静态的，而子类中的方法不是静态的，但是两个方法除了这一点外其他都满足覆盖条件，那么会发生编译错误。反之亦然。即使父类和子类中的方法都是静态的，并且满足覆盖条件，但是仍然不会发生覆盖，因为静态方法是在编译的时候把静态方法和类的引用类型进行匹配。

重写规则：重写方法不能比被重写方法限制有更严格的访问级别。（但是可以更广泛，比如父类方法是包访问权限，子类的重写方法是public访问权限）。比如：Object类有个toString()方法，开始重写这个方法的时候我们总容易忘记public修饰符，编译器当然不会放过任何教训我们的机会。出错的原因就是：没有加任何访问修饰符的方法具有包访问权限，包访问权限比 public当然要严格了，所以编译器会报错的。参数列表必须与被重写方法相同。重写有个孪生的弟弟叫重载，也就是后面要出场的。如果子类方法的参数与父类对应的方法不同，那么就是你认错人了，那是重载，不是重写。返回类型必须与被重写方法的返回类型相同。

父类方法A：void eat(){}子类方法B：int eat(){}两者虽然参数相同，可是返回类型不同，所以不是重写。

父类方法A：int eat(){} 子类方法 B：long eat(){} 返回类型虽然兼容父类，但是不同就是不同，所以不是重写。



##### Java经典面试题及答案（140~146企业真题）

141、继承（inheritance）的优缺点是什么？

● 优点：

新的实现很容易，因为大部分是继承而来的。很容易修改和扩展已有的实现。

● 缺点：

打破了封装，因为基类向子类暴露了实现细节，白盒重用，因为基类的内部细节通常对子类是可见的，当父类的实现改变时可能要相应的对子类做出改变，不能在运行时改变由父类继承来的实现。由此可见，组合比继承具有更大的灵活性和更稳定的结构，一般情况下应该优先考虑组合。只有当下列条件满足时才考虑使用继承：子类是一种特殊的类型，而不只是父类的一个角色，子类的实例不需要变成另一个类的对象子类扩展，而不是覆盖或者使父类的功能失效。

##### 142、为什么要使用接口和抽象类？

Java接口和Java抽象类代表的就是抽象类型，就是我们需要提出的抽象层的具体表现。OOP面向对象的编程，如果要提高程序的复用率，增加程序的可维护性，可扩展性，就必须是面向接口的编程，面向抽象的编程。

● Java接口和Java抽象类最大的一个区别，就在于Java抽象类可以提供某些方法的部分实现，而Java接口不可以，这大概就是Java抽象类唯一的优点吧，但这个优点非常有用。 如果向一个抽象类里加入一个新的具体方法时，那么它所有的子类都一下子得到了这个新方法，而Java接口做不到这一点，如果向一个Java接口里加入一个新方法，所有实现这个接口的类就无法成功通过编译了，因为你必须让每一个类都再实现这个方法才行。

● 一个抽象类的实现只能由这个抽象类的子类给出，也就是说，这个实现处在抽象类所定义出的继承的等级结构中，而由于Java语言的单继承性，所以抽象类作为类型定义工具的效能大打折扣。在这一点上，Java接口的优势就出来了，任何实现了一个Java接口所规定的方法的类都可以具有这个接口的类型，而一个类可以实现任意多个Java接口，从而这个类就有了多种类型。Java接口是定义混合类型的理想工具，混合类表明一个类不仅仅具有某个主类型的行为，而且具有其他的次要行为。

● 结合以上描述中抽象类和Java接口的各自优势，精典的设计模式就出来了：声明类型的工作仍然由Java接口承担，但是同时给出一个Java抽象类，且实现了这个接口，而其他同属于这个抽象类型的具体类可以选择实现这个Java接口，也可以选择继承这个抽象类，也就是说在层次结构中，Java接口在最上面，然后紧跟着抽象类。这下两个的最大优点都能发挥到极至了。这个模式就是“缺省适配模式”。在Java语言API中用了这种模式，而且全都遵循一定的命名规范：Abstract＋接口名。

Java接口和Java抽象类的存在就是为了用于具体类的实现和继承的，如果你准备写一个具体类去继承另一个具体类的话，那你的设计就有很大问题了。Java抽象类就是为了继承而存在的，它的抽象方法就是为了强制子类必须去实现的。

使用Java接口和抽象Java类进行变量的类型声明、参数的类型声明、方法的返回类型说明，以及数据类型的转换等。而不要用具体Java类进行变量的类型声明、参数类型声明、方法的返回类型说明，以及数据类型的转换等。如果你写的代码里面连一个接口和抽象类都没有的话，也许我可以说你根本没有用到任何设计模式，任何一个设计模式都是和抽象分不开的，而抽象与Java接口和抽象Java类又是分不开的。

接口的作用，就是标识类的类别。把不同类型的类归于不同的接口，可以更好的管理他们。把一组看如不相关的类归为一个接口去调用。可以用一个接口型的变量来引用一个对象。

##### 143、什么叫对象持久化（object persistence），为什么要进行对象持久化？

持久化的对象，是已经存储到数据库或保存到本地硬盘中的对象，我们称之为持久化对象。为了保存在内存中的各种对象的状态（也就是实例变量，不是方法），并且可以把保存的对象状态再读出来。我们可以使用Java提供的序列化机制。

简单说对象序列化是将对象状态转换为可保持或传输的格式的过程。什么情况下需要序列化：

● 当你想把的内存中的对象状态保存到一个文件中或者数据库中时候；

● 当你想用套接字在网络上传送对象的时候；

● 当你想通过RMI传输对象的时候；

对象要实现序列化，是非常简单的，只需要实现Serializable接口就可以了。

##### 144、JavaScript有哪些优缺点？

**● javascript的优点：**

javascript 减少网络传输：在javascript这样的用户端脚本语言出现之前，传统的数据提交和验证工作均由用户端浏览器通过网络传输到服务器开发上进行。如果数据量很大，这对于网络和服务器开发的资源来说实在是一种无形的浪费。而使用javascript就可以在客户端进行数据验证。

javascript方便操纵html对象：javascript可以方便地操纵各种页面中的对象，用户可以使用javascript来控制页面中各个元素的外观、状态甚至运行方式，javascript可以根据用户的需要“定制”浏览器，从而使网页更加友好。

javascript支持分布式应用运算：javascript可以使多种任务仅在用户端就可以完成，而不需要网络和服务器开发的参与，从而支持分布式应用的运算和处理。

**● javascript的局限性：**

各浏览器厂商对javascript支持程度不同：目前在互联网上有很多浏览器，如firefox、internet explorer、opera等，但每种浏览器支持javascript的程度是不一样的，不同的浏览器在浏览一个带有javascript脚本的主页时，由于对javascript的支持稍有不同，其效果会有一定的差距，有时甚至会显示不出来。

“web 安全性”对javascript一些功能牺牲：当把javascript的一个设计目标设定为“web安全性”时，就需要牺牲javascript的一些功能。因此，纯粹的javascript将不能打开、读写和保存用户计算机上的文件。其有权访问的唯一信息就是该javascript所嵌入开发的那个web主页中的信息，简言之，javascript将只存在于它自己的小小世界—web主页里。

##### 145、JSP有什么特点？

JSP(Java Server Pages)是由Sun Microsystems公司倡导、许多公司参与一起建立的一种动态网页技术标准

JSP 技术是用JAVA语言作为脚本语言的，JSP网页为整个服务器端的JAVA库单元提供了一个接口来服务于HTTP的应用程序。

在传统的网页HTML文件(*.htm，*.html)中加入Java程序片段(Scriptlet)和JSP标记(tag)，就构成了JSP网页(*.jsp)。Web服务器在遇到访问JSP网页的请求时，首先执行其中的程序片段，然后将执行结果以HTML格式返回给客户。程序片段可以操作数据库、重新定向网页以及发送email等等，这就是建立动态网站所需要的功能。所有程序操作都在服务器端执行，网络上传送给客户端的仅是得到的结果，对客户浏览器的要求最低，可以实现无Plugin，无ActiveX，无 Java Applet，甚至无Frame。

**● JSP 的优点：**

对于用户界面的更新，其实就是由Web Server进行的，所以给人的感觉更新很快。

所有的应用都是基于服务器的，所以它们可以时刻保持最新版本。

客户端的接口不是很繁琐，对于各种应用易于部署、维护和修改。

##### 146、什么叫脏数据，什么叫脏读（Dirty Read）？

脏数据在临时更新（脏读）中产生。事务A更新了某个数据项X，但是由于某种原因，事务A出现了问题，于是要把A回滚。但是在回滚之前，另一个事务B读取了数据项X的值(A 更新后)，A回滚了事务，数据项恢复了原值。事务B读取的就是数据项X的就是一个“临时”的值，就是脏数据。

脏读就是指当一个事务正在访问数据，并且对数据进行了修改，而这种修改还没有提交到数据库中，这时，另外一个事务也访问这个数据，然后使用了这个数据。因为这个数据是还没有提交的数据，那么另外一个事务读到的这个数据是脏数据，依据脏数据所做的操作可能是不正确的。

# Java基础逻辑面试题

##### 1、什么是BOS？

ERP系统是企业资源计划(Enterprise Resource Planning )的简称。BOS(Business & Operation Support )指的是业务运营支撑系统。BOS是ERP的集成与应用平台。BOS遵循面向服务的架构体系，是一个面向业务的可视化开发平台；是一个ERP和第三方应用集成的技术平台。它有效的解决了ERP应用的最主要矛盾：用户需求个性化和传统ERP软件标准化之间的矛盾。

##### 2、BOS与ERP是什么关系？

ERP是企业管理信息化的全面解决方案，ERP是基于BOS构建的。 ERP满足企业全面业务的标准应用；BOS确保了企业ERP应用中的个性化需求完美实现。基于BOS的ERP，可以为不同行业不同发展阶段的企业构建灵活的、可扩展的、全面集成的整体解决方案。

##### 3、Activity工作流的理解？

**● 什么是工作流**

现在大多数公司的请假流程是这样的：员工打电话（或网聊）向上级提出请假申请——上级口头同意——上级将请假记录下来——月底将请假记录上交公司——公司将请假录入电脑。采用工作流技术的公司的请假流程是这样的：员工使用账户登录系统——点击请假——上级登录系统点击允许。就这样，一个请假流程就结束了。有人会问，那上级不用向公司提交请假记录？公司不用将记录录入电脑？答案是，用的。但是这一切的工作都会在上级点击允许后自动运行！这就是工作流技术。

Georgakopoulos给出的工作流定义是：工作流是将一组任务组织起来以完成某个经营过程：定义了任务的触发顺序和触发条件，每个任务可以由一个或多个软件系统完成，也可以由一个或一组人完成，还可以由一个或多个人与软件系统协作完。

**● 工作流技术的优点**

从上面的例子，很容易看出，工作流系统实现了工作流程的自动化，提高了企业运营效率、改善企业资源利用、提高企业运作的灵活性和适应性、提高量化考核业务处理的效率、减少浪费（时间就是金钱）。而手工处理工作流程，一方面无法对整个流程状况进行有效跟踪、了解，另一方面难免会出现人为的失误和时间上的延时导致效率低下，特别是无法进行量化统计，不利于查询、报表及绩效评估。

**● 工作流生命周期**

除了我们自行启动（start）或者结束（finish）一个Activity，我们并不能直接控制一个  Activity的生命状态，我们只能通过实现Activity生命状态的表现——即回调方法来达到管理 Activity生命周期的变化。

# Javaweb面试题及答案

##### 1、说下原生JDBC操作数据库流程？

● 第一步：Class.forName()加载数据库连接驱动；

● 第二步：DriverManager.getConnection()获取数据连接对象;

● 第三步：根据SQL获取sql会话对象，有2种方式 Statement、PreparedStatement ;

● 第四步：执行SQL，执行SQL前如果有参数值就设置参数值setXXX();

● 第五步：处理结果集；

● 第六步：关闭结果集、关闭会话、关闭连接。

##### 2、为什么要使用PreparedStatement？

PreparedStatement接口继承Statement，PreparedStatement实例包含已编译的SQL语句，所以其执行速度要快于Statement对象。

作为Statement的子类， PreparedStatement 继承了Statement的所有功能。三种方法execute、 executeQuery和executeUpdate已被更改以使之不再需要参数。

● 在 JDBC 应用中，多数情况下使用PreparedStatement，原因如下：

代码的可读性和可维护性。Statement需要不断地拼接，而PreparedStatement不会。

PreparedStatement尽最大可能提高性能。DB有缓存机制，相同的预编译语句再次被调用不会再次需要编译。

最重要的一点是极大地提高了安全性。Statement容易被SQL注入，而PreparedStatement传入的内容不会和sql 语句发生任何匹配关系。

##### 3、http的长连接和短连接区别？

HTTP协议有HTTP/1.0版本和HTTP/1.1版本。HTTP1.1默认保持长连接（HTTP persistent connection，也翻译为持久连接），数据传输完成了保持TCP连接不断开（不发RST包、不四次握手），等待在同域名下继续用这个通道传输数据；相反的就是短连接。

在 HTTP/1.0 中，默认使用的是短连接。也就是说，浏览器和服务器每进行一次HTTP操作，就建立一次连接，任务结束就中断连接。从HTTP/1.1起，默认使用的是长连接，用以保持连接特性。

##### 4、HTTP/1.1与HTTP/1.0的区别？

● 可扩展性

● HTTP/1.1在消息中增加版本号，用于兼容性判断。

● HTTP/1.1增加了OPTIONS方法，它允许客户端获取一个服务器支持的方法列表。

● 为了与未来的协议规范兼容，HTTP/1.1在请求消息中包含了Upgrade头域，通过该头域，客户端可以让服务器知道它能够支持的其它备用通信协议，服务器可以据此进行协议切换，使用备用协议与客户端进行通信。

● 缓存

在HTTP/1.0中，使用Expire头域来判断资源的fresh或stale，并使用条件请求（conditional request）来判断资源是否仍有效。HTTP/1.1在1.0的基础上加入了一些cache的新特性，当缓存对象的Age超过Expire时变为stale对象，cache不需要直接抛弃stale对象，而是与源服务器进行重新激活（revalidation）。

● 带宽优化

HTTP/1.0中，存在一些浪费带宽的现象，例如客户端只是需要某个对象的一部分，而服务器却将整个对象送过来了。例如，客户端只需要显示一个文档的部分内容，又比如下载大文件时需要支持断点续传功能，而不是在发生断连后不得不重新下载完整的包。

HTTP/1.1中在请求消息中引入了range头域，它允许只请求资源的某个部分。在响应消息中Content-Range头域声明了返回的这部分对象的偏移值和长度。如果服务器相应地返回了对象所请求范围的内容，则响应码为206（Partial Content），它可以防止Cache将响应误以为是完整的一个对象。

另外一种情况是请求消息中如果包含比较大的实体内容，但不确定服务器是否能够接收该请求（如是否有权限）， 此时若贸然发出带实体的请求，如果被拒绝也会浪费带宽。HTTP/1.1 加入了一个新的状态码100（Continue）。客户端事先发送一个只带头域的请求，如果服务器因为权限拒绝了请求，就回送响应码401（Unauthorized）；如果服务器接收此请求就回送响应码100，客户端就可以继续发送带实体的完整请求了。

注意，HTTP/1.0 的客户端不支持100响应码。但可以让客户端在请求消息中加入Expect头域，并将它的值设置为100-continue。节省带宽资源的一个非常有效的做法就是压缩要传送的数据。Content-Encoding是对消息进行端到端（end-to-end）的编码，它可能是资源在服务器上保存的固有格式（如 jpeg 图片格式）；在请求消息中加入Accept-Encoding头域，它可以告诉服务器客户端能够解码的编码方式。

● 长连接

HTTP/1.0规定浏览器与服务器只保持短暂的连接，浏览器的每次请求都需要与服务器建立一个TCP连接，服务器完成请求处理后立即断开TCP连接，服务器不跟踪每个客户也不记录过去的请求。此外，由于大多数网页的流量都比较小，一次TCP连接很少能通过slow-start区，不利于提高带宽利用率。

HTTP 1.1支持长连接（PersistentConnection）和请求的流水线（Pipelining）处理，在一个 TCP连接上可以传送多个HTTP请求和响应，减少了建立和关闭连接的消耗和延迟。例如：一个包含有许多图像的网页文件的多个请求和应答可以在一个连接中传输，但每个单独的网页文件的请求和应答仍然需要使用各自的连接。

HTTP1.1还允许客户端不用等待上一次请求结果返回，就可以发出下一次请求，但服务器端必须按照接收到客户端请求的先后顺序依次回送响应结果，以保证客户端能够区分出每次请求的响应内容，这样也显著地减少了整个下载过程所需要的时间。

● 消息传递

HTTP消息中可以包含任意长度的实体，通常它们使用Content-Length来给出消息结束标志。但是，对于很多动态产生的响应，只能通过缓冲完整的消息来判断消息的大小，但这样做会加大延迟。如果不使用长连接，还可以通过连接关闭的信号来判定一个消息的结束。

HTTP/1.1中引入了Chunkedtransfer-coding来解决上面这个问题，发送方将消息分割成若干个任意大小的数据块，每个数据块在发送时都会附上块的长度，最后用一个零长度的块作为消息结束的标志。这种方法允许发送方只缓冲消息的一个片段，避免缓冲整个消息带来的过载。

在HTTP/1.0中，有一个Content-MD5的头域，要计算这个头域需要发送方缓冲完整个消息后才能进行。而HTTP/1.1中，采用chunked分块传递的消息在最后一个块（零长度）结束之后会再传递一个拖尾（trailer），它包含一个或多个头域，这些头域是发送方在传递完所有块之后再计算出值的。发送方会在消息中包含一个Trailer头域告诉接收方这个拖尾的存在。

● Host头域

在HTTP1.0中认为每台服务器都绑定一个唯一的IP地址，因此，请求消息中的URL并没有传递主机名（hostname）。但随着虚拟主机技术的发展，在一台物理服务器上可以存在多个虚拟主机（Multi-homed Web Servers），并且它们共享一个 IP 地址。HTTP1.1的请求消息和响应消息都应支持 Host 头域，且请求消息中如果没有 Host 头域会报告一个错误（400 Bad Request）。此外，服务器应该接受以绝对路径标记的资源请求。

● 错误提示

HTTP/1.0中只定义了16个状态响应码，对错误或警告的提示不够具体。HTTP/1.1引入了一个Warning头域， 增加对错误或警告信息的描述。此外，在HTTP/1.1中新增了24个状态响应码，如409（Conflict）表示请求的资源与资源的当前状态发生冲突；410（Gone）表示服务器上的某个资源被永久性的删除。

##### 5、http常见的状态码有哪些？

● 200 OK 客户端请求成功

● 301Moved Permanently（永久移除)，请求的URL已移走。Response中应该包含一个 Location URL，说明资源现在所处的位置

● 302found 重定向

● 400Bad Request 客户端请求有语法错误，不能被服务器所理解

● 401Unauthorized 请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用

● 403 Forbidden 服务器收到请求，但是拒绝提供服务

● 404 Not Found 请求资源不存在，eg：输入了错误的URL

● 500 Internal Server Error 服务器发生不可预期的错误

● 503 Server Unavailable 服务器当前不能处理客户端的请求，一段时间后可能恢复正常

##### 6、GET和POST的区别？

● GET请求的数据会附在URL之后（就是把数据放置在HTTP协议头中），以?分割URL和传输数据，参数之间以&相连，如：login.action?name=zhagnsan&password=123456。POST 把提交的数据则放置在是HTTP包的包体中。

● GET方式提交的数据最多只能是1024字节，理论上POST没有限制，可传较大量的数据。其实这样说是错误的，不准确的：“GET方式提交的数据最多只能是1024字节"，因为 GET 是通过URL提交数据，那么GET可提交的数据量就跟URL的长度有直接关系了。而实际上，URL不存在参数上限的问题，HTTP协议规范没有对URL长度进行限制。这个限制是特定的浏览器及服务器对它的限制。IE对URL长度的限制是2083字节(2K+35)。对于其他浏览器，如Netscape、FireFox等，理论上没有长度限制，其限制取决于操作系统的支持。

● POST的安全性要比GET的安全性高。注意：这里所说的安全性和上面 GET 提到的“安全”不是同个概念。上面“安全”的含义仅仅是不作数据修改，而这里安全的含义是真正的 Security的含义，比如：通过GET 提交数据，用户名和密码将明文出现在URL上，因为登录页面有可能被浏览器缓存，其他人查看浏览器的历史纪录，那么别人就可以拿到你的账号和密码了，除此之外，使用 GET 提交数据还可能会造成Cross-site request forgery攻击。

● Get 是向服务器发索取数据的一种请求，而Post是向服务器提交数据的一种请求，在FORM（表单）中，Method默认为"GET"，实质上GET和POST只是发送机制不同，并不是一个取一个发。

##### 7、http中重定向和请求转发的区别？

本质区别：转发是服务器行为，重定向是客户端行为。

重定向特点：两次请求，浏览器地址发生变化，可以访问自己web之外的资源，传输的数据会丢失。

请求转发特点：一次强求，浏览器地址不变，访问的是自己本身的web资源，传输的数据不会丢失。

##### 8、Cookie 和 Session 的区别？

Cookie是web服务器发送给浏览器的一块信息，浏览器会在本地一个文件中给每个web服务器存储Cookie。以后浏览器再给特定的web服务器发送请求时，同时会发送所有为该服务器存储的Cookie。

Session是存储在web服务器端的一块信息。Session对象存储特定用户会话所需的属性及配置信息。当用户在应用程序的Web页之间跳转时，存储在Session对象中的变量将不会丢失，而是在整个用户会话中一直存在下去。

**● Cookie和 Session的不同点：**

无论客户端做怎样的设置，Session都能够正常工作。当客户端禁用Cookie时将无法使用Cookie。

在存储的数据量方面：Session能够存储任意的java对象，Cookie只能存储String类型的对象。

##### 9、2.session共享怎么做的（分布式如何实现session共享）？

问题描述：一个用户在登录成功以后会把用户信息存储在session当中，这时session所在服务器为server1，那么用户在session失效之前如果再次使用app，那么可能会被路由到 server2，这时问题来了，server2没有该用户的session，所以需要用户重新登录，这时的用户体验会非常不好，所以我们想如何实现多台server之间共享session，让用户状态得以保存。

● 服务器实现的session复制或session共享，这类型的共享session是和服务器紧密相关的，比如webSphere或JBOSS在搭建集群时候可以配置实现session复制或session共享，但是这种方式有一个致命的缺点，就是不好扩展和移植，比如我们更换服务器，那么就要修改服务器配置。

● 利用成熟的技术session复制，比如12306使用的gemfire，比如常见的内存数据库如redis 或memorycache，这类方案虽然比较普适，但是严重依赖于第三方，这样当第三方服务器出现问题的时候，那么将是应用的灾难。

● 将session维护在客户端，很容易想到就是利用cookie，但是客户端存在风险，数据不安全，而且可以存放的数据量比较小，所以将session维护在客户端还要对session中的信息加密。我们实现的方案可以说是第二种方案和第三种方案的合体，可以利用gemfire实现session复制共享，还可以session维护在redis中实现session共享，同时可以将session维护在客户端的cookie中，但是前提是数据要加密。这三种方式可以迅速切换，而不影响应用正常执行。我们在实践中，首选gemfire或者redis作为session共享的载体，一旦session不稳定出现问题的时候，可以紧急切换cookie维护session作为备用，不影响应用提供服务。

这里主要讲解redis和cookie方案，gemfire比较复杂大家可以自行查看gemfire工作原理。利用redis做session共享，首先需要与业务逻辑代码解耦，不然session共享将没有意义，其次支持动态切换到客户端cookie模式。redis的方案是，重写服务器中的HttpSession和HttpServletRequest，首先实现HttpSession接口，重写session的所有方法，将session以hash值的方式存在redis中，一个session的key就是sessionID，setAtrribute重写之后就是更新redis中的数据，getAttribute重写之后就是获取redis中的数据，等等需要将HttpSession的接口一一实现。实现了HttpSesson，那么我们先将该session类叫做MySession（当然实践中不是这么命名的），当MySession出现之后问题才开始，怎么能在不影响业务逻辑代码的情况下，还能让原本的request.getSession()获取到的是MySession，而不是服务器原生的session。这里，我决定重写服务器的HttpServletRequet，这里先称为MyRequest，但是这可不是单纯的重写，我需要在原生的request基础上重写，于是我决定在filter中，实现request的偷梁换柱，我的思路是这样的，MyRequest的构建器，必须以request作为参数，于是我在filter中将服务器原生的request（也有可能是框架封装过的request），当做参数new出来一个MyRequest，并且MyRequest也实现了HttpServletRequest接口，其实就是对原生request的一个增强，这里主要重写了几个request的方法，但是最重要的是重写了request.getSession()，写到这里大家应该都明白为什么重写这个方法了吧，当然是为了获取MySession，于是这样就在filter中，偷偷的将原生的request换成MyRequest了，然后再将替换过的request传入chan.doFilter()，这样filter时候的代码都使用的是MyRequest了，同时对业务代码是透明的，业务代码获取session的方法仍然是request.getSession()，但其实获取到的已经是MySession了，这样对session的操作已经变成了对redis的操作。这样实现的好处有两个，第一开发人员不需要对session共享做任何关注，session共享对用户是透明的；第二，filter是可配置的，通过filter的方式可以将session共享做成一项可插拔的功能，没有任何侵入性。这个时候已经实现了一套可插拔的session共享的框架了，但是我们想到如果redis服务出了问题，这时我们该怎么办呢，于是我们延续redis的想法，想到可以将session维护在客户端内（加密的cookie），当然实现方法还是一样的，我们重写HttpSession接口，实现其所有方法，比如setAttribute就是写入cookie，getAttribute就是读取cookie，我们可以将重写的session称作MySession2，这时怎么让开发人员透明的获取到MySession2呢，实现方法还是在filter内偷梁换柱，在MyRequest加一个判断，读取sessionType配置，如果sessionType是redis的，那么getSession的时候获取到的是MySession，如果sessionType是coolie的，那么getSession的时候获取到的是MySession2，以此类推，用同样的方法就可以获取到MySession3，4，5，6等等。

这样两种方式都有了，那么我们怎实现两种session共享方式的快速切换呢，刚刚我提到一个sessionType，这是用来决定获取到session的类型的，只要变换sessionType就能实现两种session共享方式的切换，但是sessionType必须对所有的服务器都是一致的，如果不一致那将会出现比较严重的问题，我们目前是将sessionType维护在环境变量里，如果要切换sessionType就要重启每一台服务器，完成session共享的转换，但是当服务器太多的时候将是一种灾难。而且重启服务意味着服务的中断，所以这样的方式只适合服务器规模比较小，而且用户量比较少的情况，当服务器太多的时候，务必需要一种协调技术，能够让服务器能够及时获取切换的通知。基于这样的原因，我们选用zookeeper作为配置平台，每一台服务器都会订阅zookeeper上的配置，当我们切换sessionType之后，所有服务器都会订阅到修改之后的配置，那么切换就会立即生效，当然可能会有短暂的时间延迟，但这是可以接受的。

##### 10、在单点登录中，如果cookie被禁用了怎么办？****

单点登录的原理是后端生成一个sessionID，然后设置到cookie，后面的所有请求浏览器都会带上cookie，然后服务端从cookie里获取sessionID，再查询到用户信息。所以，保持登录的关键不是cookie，而是通过cookie保存和传输的sessionID，其本质是能获取用户信息的数据。除了cookie，还通常使用HTTP请求头来传输。但是这个请求头浏览器不会像cookie一样自动携带，需要手工处理。

# Java前端面试题及答案

##### 1、什么是jsp，什么是Servlet？jsp 和Servlet 有什么区别？

jsp本质上就是一个Servlet，它是Servlet的一种特殊形式（由SUN公司推出），每个jsp页面都是一个servlet实例。Servlet是由Java提供用于开发web服务器应用程序的一个组件，运行在服务端，由servlet容器管理，用来生成动态内容。一个servlet实例是实现了特殊接口Servlet的Java类，所有自定义的servlet均必须实现Servlet接口。

● 区别：

jsp是html页面中内嵌的Java代码，侧重页面显示；

Servlet是html代码和Java代码分离，侧重逻辑控制，mvc设计思想中jsp位于视图层，servlet位于控制层

jsp运行机制：如下图

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190517/1558056288@60312a9f40ca145501b4d69b0c7f0213.png)

JVM只能识别Java类，并不能识别jsp代码！web容器收到以.jsp为扩展名的url请求时，会将访问请求交给tomcat中jsp引擎处理，每个jsp页面第一次被访问时，jsp引擎将jsp代码解释为一个servlet源程序，接着编译servlet源程序生成.class文件，再有web容器servlet引擎去装载执行servlet程序，实现页面交互。

##### 2、jsp有哪些域对象和内置对象及他们的作用？

四大域对象：

● pageContext page域-指当前页面，在当前jsp页面有效，跳到其它页面失效。

● requestrequest域-指一次请求范围内有效，从http请求到服务器处理结束，返回响应的整个过程。在这个过程中使用forward（请求转发）方式跳转多个jsp，在这些页面里你都可以使用这个变量。

● sessionsession域-指当前会话有效范围，浏览器从打开到关闭过程中，转发、重定向均可以使用。

● applicationcontext域-指只能在同一个web中使用，服务器未关闭或者重启，数据就有效。

##### 3、什么是xml，使用xml的优缺点，xml的解析器有哪几种，分别有什么区别？

xml是一种可扩展性标记语言，支持自定义标签（使用前必须预定义）使用DTD和XMLSchema标准化XML结构。

优点：用于配置文件，格式统一，符合标准；用于在互不兼容的系统间交互数据，共享数据方便；

缺点：xml文件格式复杂，数据传输占流量，服务端和客户端解析xml文件占用大量资源且不易维护

xml常用解析器有2种，分别是：DOM和SAX。主要区别在于它们解析xml文档的方式不同。使用DOM解析，xml文档以DOM树形结构加载入内存，而SAX采用的是事件模型。

##### 4、谈谈你对ajax的认识？

Ajax是一种创建交互式网页应用的的网页开发技术;AsynchronousJavaScriptandXML的缩写。

Ajax的优势：通过异步模式，提升了用户体验。优化了浏览器和服务器之间的传输，减少不必要的数据往返，减少了带宽占用。Ajax引擎在客户端运行，承担了一部分本来由服务器承担的工作，从而减少了大用户量下的服务器负载。

Ajax的最大特点：可以实现局部刷新，在不更新整个页面的前提下维护数据，提升用户体验度。

##### 5、jsonp原理是什么？

JavaScript是一种在Web开发中经常使用的前端动态脚本技术。在JavaScript中，有一个很重要的安全性限制，被称为“Same-OriginPolicy”（同源策略）。这一策略对于JavaScript代码能够访问的页面内容做了很重要的限制，即JavaScript只能访问与包含它的文档在同一域下的内容。

JavaScript这个安全策略在进行多iframe或多窗口编程、以及Ajax编程时显得尤为重要。根据这个策略，在baidu.com下的页面中包含的JavaScript代码，不能访问在google.com域名下的页面内容；甚至不同的子域名之间的页面也不能通过JavaScript代码互相访问。对于Ajax的影响在于，通过XMLHttpRequest实现的Ajax请求，不能向不同的域提交请求，例如，在abc.example.com下的页面，不能向def.example.com提交Ajax请求，等等。然而，当进行一些比较深入的前端编程的时候，不可避免地需要进行跨域操作，这时候“同源策略”就显得过于苛刻。JSONP跨域GET请求是一个常用的解决方案，下面我们来看一下JSONP跨域是如何实现的，并且探讨下JSONP跨域的原理。jsonp的最基本的原理是：动态添加一个<script>标签，使用script标签的src属性没有跨域的限制特点。首先在客户端注册一个callback，然后把callback的名字传给服务器。此时，服务器先生成json数据。然后以javascript语法的方式，生成一个function，function名字就是传递上来的参数jsonp。最后将json数据直接以入参的方式，放置到function中，这样就生成了一段js语法的文档，返回给客户端。客户端浏览器，解析script标签，并执行返回的javascript文档，此时数据作为参数，传入到了客户端预先定义好的callback函数里。

# Java linux面试题及答案

##### 1、说一下常用的Linux命令？

● 列出文件列表：ls【参数 -a -l】

● 创建目录和移除目录：mkdir rmdir

● 用于显示文件后几行内容：tail打包：tar -xvf

● 打包并压缩：tar -zcvf

● 查找字符串：grep

● 显示当前所在目录：pwd创建空文件：touch

● 编辑器：vim vi

##### 2、Linux中如何查看日志？

动态打印日志信息：tail –f 日志文件

##### 3、Linux怎么关闭进程？

通常用ps查看进程PID，用kill命令终止进程。ps命令用于查看当前正在运行的进程。grep是搜索；-aux显示所有状态；

例如：

ps –ef | grep java表示查看所有进程里CMD是java的进程信息。

ps –aux | grep java

kill命令用于终止进程。例如：kill -9 [PID]  -9表示强迫进程立即停止。

# Java框架面试题及答案

##### 1、说一下EasyUI的认识？

EasyUI是一种基于jQuery的用户界面插件集合。easyui为创建现代化，互动，JavaScript应用程序，提供必要的功能。使用easyui你不需要写很多代码，你只需要通过编写一些简单HTML标记，就可以定义用户界面。优势：开源免费，页面也还说的过去。接下来看easyUI入门：

页面引入必要的js和css样式文件，文件引入顺序为：

```java
<!-- 引入 JQuery -->
<script type="text/javascript" src="../jquery-easyui-1.4.1/jquery.min.js"></script>
<!-- 引入 EasyUI -->
<script type="text/javascript" src="../jquery-easyui-1.4.1/jquery.easyui.min.js"></script>
<!-- 引入 EasyUI 的中文国际化 js，让 EasyUI 支持中文 -->
<script type="text/javascript" src="../jquery-easyui-1.4.1/locale/easyui-lang- zh_CN.js"></script>
<!-- 引入 EasyUI 的样式文件-->
<link rel="stylesheet" href="../jquery-easyui-1.4.1/themes/default/easyui.css" type="text/css"/>
<!-- 引入 EasyUI 的图标样式文件-->
<link rel="stylesheet" href="../jquery-easyui-1.4.1/themes/icon.css" type="text/css"/>
```

然后在页面写 easyUI 代码就行，easyUI 提供了很多样式：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190517/1558056622@677c9c633d9d5cc9947b2059a3824581.png)

示例如下：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190517/1558056649@9e7c15354b7dbd320b0e85c76f6d529f.png)

实现代码如下：

```java
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Basic Dialog - jQuery EasyUI Demo</title>
    <link rel="stylesheet" type="text/css" href="../../themes/default/easyui.css">
    <link rel="stylesheet" type="text/css" href="../../themes/icon.css">
    <link rel="stylesheet" type="text/css" href="../demo.css">
    <script type="text/javascript" src="../../jquery.min.js"></script>
    <script type="text/javascript" src="../../jquery.easyui.min.js"></script>
</head>
<body>
<h2>Basic Dialog</h2>
<p>Click below button to open or close dialog.</p>
<div style="margin:20px 0;">
    <a href="javascript:void(0)" class="easyui-linkbutton" onclick="$('#dlg').dialog('open')">Open</a>
    <a href="javascript:void(0)" class="easyui-linkbutton" onclick="$('#dlg').dialog('close')">Close</a>
</div>
<div id="dlg" class="easyui-dialog" title="Basic Dialog" data-options="iconCls:'icon-save'"
     style="width:400px;height:200px;padding:10px">
    The dialog content.
</div>
</body>
</html>
```

##### 2、说一下MiniUI的认识？

基于jquery的框架，开发的界面功能都很丰富。jQueryMiniUI-快速开发WebUI。它能缩短开发时间，减少代码量，使开发者更专注于业务和服务端，轻松实现界面开发，带来绝佳的用户体验。使用MiniUI，开发者可以快速创建Ajax无刷新、B/S快速录入数据、CRUD、Master-Detail、菜单工具栏、弹出面板、布局导航、数据验证、分页表格、树、树形表格等典型WEB应用系统界面。缺点：收费，没有源码，基于这个开发如果想对功能做扩展就需要找他们的团队进行升级！

● 提供以下几大类控件：

表格控件树形控件。

布局控件：标题面板、弹出面板、折叠分割器、布局器、表单布局器等导航控件：分页导航器、导航菜单、选项卡、菜单、工具栏等。

表单控件：多选输入框、弹出选择框、文本输入框、数字输入框、日期选择框、下拉选择框、下拉树形选择框、下拉表格选择框、文件上传控件、多选框、列表框、多选框组、单选框组、按钮等富文本编辑器。

图表控件：柱状图、饼图、线形图、双轴图等。

● 技术亮点：

快速开发：使用Html配置界面，减少80%界面代码量。易学易用：简单的API设计，可以独立、组合使用控件。

性能优化：内置数据懒加载、低内存开销、快速界面布局等机制。丰富控件：包含表格、树、数据验证、布局导航等超过50个控件。

超强表格：提供锁定列、多表头、分页排序、行过滤、数据汇总、单元格编辑、详细行、Excel导出等功能。

第三方兼容：与ExtJS、jQuery、YUI、Dojo等任意第三方控件无缝集成。浏览器兼容：支持IE6+、FireFox、Chrome等。

跨平台支持：支持Java、.NET、PHP等。

**● 示例如下：**

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190517/1558056721@fdab4546938a44f0fac69a00aa98a80d.png)

实现代码如下：

```java
<ul id="tree1" class="mini-tree" url="../data/tree.txt" style="width:200px;padding:5px;"
    showTreeIcon="true" textField="text" idField="id"
    allowDrag="true" allowDrop="true">
</ul>
```

##### 3、说一下jQueryUI的认识？

jQueryUI是一套jQuery的页面UI插件，包含很多种常用的页面空间，例如Tabs（如本站首页右上角部分）、拉帘效果（本站首页左上角）、对话框、拖放效果、日期选择、颜色选择、数据排序、窗体大小调整等等非常多的内容。

**● 技术亮点：**

简单易用：继承jQuery简易使用特性，提供高度抽象接口，短期改善网站易用性。

开源免费：采用MIT&GPL双协议授权，轻松满足自由产品至企业产品各种授权需求。

广泛兼容：兼容各主流桌面浏览器。包括IE6+、Firefox2+、Safari3+、Opera9+、Chrome1+。

轻便快捷：组件间相对独立，可按需加载，避免浪费带宽拖慢网页打开速度。

标准先进：支持WAI-ARIA，通过标准XHTML代码提供渐进增强，保证低端环境可访问性。

美观多变：提供近20种预设主题，并可自定义多达60项可配置样式规则，提供24种背景纹理选择。

##### 4、说一下Vue.js的认识？

Vue.js(读音/vjuː/，类似于view)是一套构建用户界面的渐进式框架。与其他重量级框架不同的是，Vue采用自底向上增量开发的设计。Vue的核心库只关注视图层，它不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与单文件组件和Vue生态系统支持的库结合使用时，Vue也完全能够为复杂的单页应用程序提供驱动。Vue.js起步：

引入相应文件：

```java
<script src="https://unpkg.com/vue"></script>
```

声明式渲染：Vue.js 的核心是一个允许采用简洁的模板语法来声明式的将数据渲染进 DOM 的系统：

```java
<!-- html 文件中 -->
<div id="app">
    {{ message }}
</div>
<!-- js 文件中 -->
var app = new Vue({
    el: '#app'，
    data: {
        message: 'Hello Vue!'
    }
})
```

通过浏览器查看效果图为：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190517/1558056837@9bfea47ade58d69d9a332357a345ebe0.png)

创建 vue 实例：每个 Vue 应用都是通过 Vue 函数创建一个新的 Vue 实例开始的，当创建一个 Vue 实例时，你可以传入一个选项对象。可以使用这些选项来创建你想要的行为。

```java
<!-- js 文件中 -->
var vm = new Vue({
    // 选项
})
```

据变化时更新 DOM  等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，给予用户机会在一些特定的场景下添加他们自己的代码。比如 created 钩子可以用来在一个实例被创建之后执行代码：

```java
<!-- js 文件中 -->
new Vue({
    data: {
        a: 1
    },
    created: function () {
        console.log('a is: ' + this.a)
    }
})
```

##### 5、说一下AngularJS的认识？

AngularJS是google开发者设计的一个前端开发框架，它是由是由JavaScript编写的一个JS框架。通常它是用来在静态网页构建动态应用不足而设计的。

**● AngularJS特点如下：**

数据绑定：AngularJS是数据双向绑定。

MVVM（Model-View-ViewModel）模式：Model简单数据对象，View视图（如HTML，JSP等）

ViewModel是用来提供数据和方法，和View进行交互。这种设计模式使得代码解耦合。

依赖注入：AngularJS支持注入方式把需要的对象，方法等注入到指定的对象中。

指令：AngularJS内部自带各种常用指令，同时也支持开发者自定义指令。

HTML模板和扩展HTML：AngularJS可以定义与HTML兼容的自定义模板。

**● AngularJS的Api：**

AngularJS提供了很多功能丰富的组件，处理核心的ng组件外，还扩展了很多常用的功能组件，如ngRoute(路由)，ngAnimate(动画)，ngTouch(移动端操作)等，只需要引入相应的头文件，并依赖注入你的工作模块，则可使用。ng(coremodule)：AngularJS的默认模块，包含AngularJS的所有核心组件。

# Java mysql面试题

##### 1、SQL中聚合函数有哪些？

聚合函数是对一组值进行计算并返回单一的值的函数，它经常与select语句中的group by子句一同使用。

● avg()：返回的是指定组中的平均值，空值被忽略。

● count()：返回的是指定组中的项目个数。

● max()：返回指定数据中的最大值。

● min()：返回指定数据中的最小值。

● sum()：返回指定数据的和，只能用于数字列，空值忽略。

##### 2、SQL之连接查询（左连接和右连接的区别）？

● 外连接：

● 左连接（左外连接）：以左表作为基准进行查询，左表数据会全部显示出来，右表如果和左表匹配的数据则显示相应字段的数据，如果不匹配则显示为null。

● 右连接（右外连接）：以右表作为基准进行查询，右表数据会全部显示出来，左表如果和右表匹配的数据则显示相应字段的数据，如果不匹配则显示为null。

● 全连接：先以左表进行左外连接，再以右表进行右外连接。

● 内连接：显示表之间有连接匹配的所有行。

##### 3、SQL之sql注入是什么？

通过在Web表单中输入（恶意）SQL语句得到一个存在安全漏洞的网站上的数据库，而不是按照设计者意图去执行SQL语句。举例：当执行的sql为select * from user where username = “admin” or “a” = “a”时，sql语句恒成立，参数username毫无意义。

**● 防止sql注入的方式：**

预编译语句：如，select * from user where username = ？，sql语句语义不会发生改变，sql语句中变量用?表示，即使传递参数时为“admin or ‘a’ = ‘a’”，也会把这整体当做一个字符创去查询。

Mybatis框架中的mapper方式中的#也能很大程度的防止sql注入（$无法防止sql注入）。

##### 4、MySQL性能优化有哪些？

● 当只要一行数据时使用limit 1

查询时如果已知会得到一条数据，这种情况下加上limit 1会增加性能。因为MySQL数据库引擎会在找到一条结果停止搜索，而不是继续查询下一条是否符合标准直到所有记录查询完毕。

● 选择正确的数据库引擎

MySQL中有两个引擎MyISAM和InnoDB，每个引擎有利有弊。MyISAM适用于一些大量查询的应用，但对于有大量写功能的应用不是很好。甚至你只需要update一个字段整个表都会被锁起来。而别的进程就算是读操作也不行要等到当前update操作完成之后才能继续进行。另外，MyISAM对于select count(*)这类操作是超级快的。InnoDB的趋势会是一个非常复杂的存储引擎，对于一些小的应用会比MyISAM还慢，但是支持“行锁”，所以在写操作比较多的时候会比较优秀。并且，它支持很多的高级应用，例如：事务。

● 用not exists代替not in

not exists用到了连接能够发挥已经建立好的索引的作用，not in不能使用索引。not in是最慢的方式要同每条记录比较，在数据量比较大的操作红不建议使用这种方式。

● 对操作符的优化，尽量不采用不利于索引的操作符

如：in、not in、is null、is not null 、<> 等某个字段总要拿来搜索，为其建立索引：MySQL中可以利用alter table语句来为表中的字段添加索引，语法为：alter table表名add index(字段名)

##### 5、MySQL 数据库架构图了解吗？

MyISAM和InnoDB是最常见的两种存储引擎，特点如下。

● MyISAM存储引擎

MyISAM是MySQL官方提供默认的存储引擎，其特点是不支持事务、表锁和全文索引，对于一些OLAP（联机分析处理）系统，操作速度快。

每个MyISAM在磁盘上存储成三个文件。文件名都和表名相同，扩展名分别是.frm（存储表定义）、.MYD(MYData，存储数据)、.MYI(MYIndex，存储索引)。这里特别要注意的是MyISAM不缓存数据文件，只缓存索引文件。

● InnoDB存储引擎

InnoDB存储引擎支持事务，主要面向OLTP（联机事务处理过程）方面的应用，其特点是行锁设置、支持外键，并支持类似于Oracle的非锁定读，即默认情况下读不产生锁。InnoDB将数据放在一个逻辑表空间中（类似Oracle）。

InnoDB通过多版本并发控制来获得高并发性，实现了ANSI标准的4种隔离级别，默认为Repeatable，使用一种被称为next-keylocking的策略避免幻读。

对于表中数据的存储，InnoDB采用类似Oracle索引组织表Clustered的方式进行存储。InnoDB存储引擎提供了具有提交、回滚和崩溃恢复能力的事务安全。但是对比myisam的存储引擎，InnoDB写的处理效率差一些并且会占用更多的磁盘空间以保留数据和索引。以下是InnoDB体系架构：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190517/1558057046@41e3b0a1560b08d041938020e7208fa4.png)

 

 

##### 6、MySQL架构器中各个模块都是什么？

● 连接管理与安全验证是什么

每个客户端都会建立一个与服务器连接的线程，服务器会有一个线程池来管理这些连接；如果客户端需要连接到MYSQL数据库还需要进行验证，包括用户名、密码、主机信息等。

● 解析器是什么

解析器的作用主要是分析查询语句，最终生成解析树；首先解析器会对查询语句的语法进行分析，分析语法是否有问题。还有解析器会查询缓存，如果在缓存中有对应的语句，就返回查询结果不进行接下来的优化执行操作。前提是缓存中的数据没有被修改，当然如果被修改了也会被清出缓存。

● 优化器怎么用

优化器的作用主要是对查询语句进行优化操作，包括选择合适的索引，数据的读取方式，包括获取查询的开销信息，统计信息等，这也是为什么图中会有优化器指向存储引擎的箭头。之前在别的文章没有看到优化器跟存储引擎之间的关系，在这里我个人的理解是因为优化器需要通过存储引擎获取查询的大致数据和统计信息。

● 执行器是什么

执行器包括执行查询语句，返回查询结果，生成执行计划包括与存储引擎的一些处理操作。

##### 7、MySQL存储引擎有哪些？

● InnoDB存储引擎

InnoDB是事务型数据库的首选引擎，支持事务安全表（ACID），支持行锁定和外键，InnoDB是默认的MySQL引擎。

● MyISAM存储引擎

MyISAM基于ISAM存储引擎，并对其进行扩展。它是在Web、数据仓储和其他应用环境下最常使用的存储引擎之一。MyISAM拥有较高的插入、查询速度，但不支持事务。

● MEMORY存储引擎

MEMORY存储引擎将表中的数据存储到内存中，未查询和引用其他表数据提供快速访问。

● NDB存储引擎

NDB存储引擎是一个集群存储引擎，类似于Oracle的RAC，但它是ShareNothing的架构，因此能提供更高级别的高可用性和可扩展性。NDB的特点是数据全部放在内存中，因此通过主键查找非常快。关于NDB，有一个问题需要注意，它的连接(join)操作是在MySQL数据库层完成，不是在存储引擎层完成，这意味着，复杂的join操作需要巨大的网络开销，查询速度会很慢。

● Memory(Heap)存储引擎

Memory存储引擎（之前称为Heap）将表中数据存放在内存中，如果数据库重启或崩溃，数据丢失，因此它非常适合存储临时数据。

● Archive存储引擎

正如其名称所示，Archive非常适合存储归档数据，如日志信息。它只支持INSERT和SELECT操作，其设计的主要目的是提供高速的插入和压缩功能。

● Federated存储引擎

Federated存储引擎不存放数据，它至少指向一台远程MySQL数据库服务器上的表，非常类似于Oracle的透明网关。

● Maria存储引擎

Maria存储引擎是新开发的引擎，其设计目标是用来取代原有的MyISAM存储引擎，从而成为MySQL默认的存储引擎。

上述引擎中，InnoDB是事务安全的存储引擎，设计上借鉴了很多Oracle的架构思想，一般而言，在OLTP应用中，InnoDB应该作为核心应用表的首先存储引擎。InnoDB是由第三方的InnobaseOy公司开发，现已被Oracle收购，创始人是HeikkiTuuri，芬兰赫尔辛基人，和著名的Linux创始人Linus是校友。

##### 8、MySQL事务介绍？

MySQL和其它的数据库产品有一个很大的不同就是事务由存储引擎所决定，例如MYISAM，MEMORY，ARCHIVE都不支持事务，事务就是为了解决一组查询要么全部执行成功，要么全部执行失败。MySQL事务默认是采取自动提交的模式，除非显示开始一个事务。

```java
SHOW VARIABLES LIKE 'AUTOCOMMIT';
```

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190517/1558057193@44836e47420e691a306f34321543ccba.png)

修改自动提交模式，0=OFF，1=ON，注意：修改自动提交对非事务类型的表是无效的，因为它们本身就没有提交和回滚的概念，还有一些命令是会强制自动提交的，比如DLL命令、locktables等。

```java
SET AUTOCOMMIT = 0;
```

或

```java
SET AUTOCOMMIT = OFF;
```

##### 9、事务的四大特征是什么？

数据库事务transanction正确执行的四个基本要素。ACID，原子性(Atomicity)、一致性(Correspondence)、隔离性(Isolation)、持久性(Durability)。

● 原子性：整个事务中的所有操作，要么全部完成，要么全部不完成，不可能停滞在中间某个环节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。

● 一致性：在事务开始之前和事务结束以后，数据库的完整性约束没有被破坏。

● 隔离性：隔离状态执行事务，使它们好像是系统在给定时间内执行的唯一操作。如果有两个事务，运行在相同的时间内，执行相同的功能，事务的隔离性将确保每一事务在系统中认为只有该事务在使用系统。这种属性有时称为串行化，为了防止事务操作间的混淆，必须串行化或序列化请求，使得在同一时间仅有一个请求用于同一数据。

● 持久性：在事务完成以后，该事务所对数据库所作的更改便持久的保存在数据库之中，并不会被回滚。

##### 10、MySQL中四种隔离级别分别是什么？

● 读未提交（READ UNCOMMITTED）：未提交读隔离级别也叫读脏，就是事务可以读取其它事务未提交的数据。

● 读已提交（READ COMMITTED）：在其它数据库系统比如SQL Server默认的隔离级别就是提交读，已提交读隔离级别就是在事务未提交之前所做的修改其它事务是不可见的。

● 可重复读（REPEATABLE READ）：保证同一个事务中的多次相同的查询的结果是一致的，比如一个事务一开始查询了一条记录然后过了几秒钟又执行了相同的查询，保证两次查询的结果是相同的，可重复读也是MySQL的默认隔离级别。

● 可串行化（SERIALIZABLE）：可串行化就是保证读取的范围内没有新的数据插入，比如事务第一次查询得到某个范围的数据，第二次查询也同样得到了相同范围的数据，中间没有新的数据插入到该范围中。

##### 11、MySQL怎么创建存储过程?

MySQL存储过程是从MySQL5.0开始增加的新功能。存储过程的优点有一箩筐。不过最主要的还是执行效率和SQL代码封装。特别是SQL代码封装功能，如果没有存储过程，在外部程序访问数据库时，要组织很多SQL语句。特别是业务逻辑复杂的时候，一大堆的SQL和条件夹杂在代码中，让人不寒而栗。现在有了MySQL存储过程，业务逻辑可以封装存储过程中，这样不仅容易维护，而且执行效率也高。

● 创建MySQL存储过程

下面代码创建了一个叫pr_add的MySQL存储过程，这个MySQL存储过程有两个int类型的输入参数“a”、“b”，返回这两个参数的和。

● drop procedure if exists pr_add;（备注：如果存在pr_add的存储过程，则先删掉）

● 计算两个数之和（备注：实现计算两个整数之和的功能）

```java
create procedure pr_add (a int,b int)
begin
declare c int;
if a is null then set a = 0;
end if;
if b is null then set b = 0;
end if;
set c = a + b;
select c as sum;
```

● 调用 MySQL 存储过程

```java
call pr_add(10， 20);
```

##### 12、MySQL触发器怎么写？

MySQL包含对触发器的支持。触发器是一种与表操作有关的数据库对象，当触发器所在表上出现指定事件时，将调用该对象，即表的操作事件触发表上的触发器的执行。

在MySQL中，创建触发器语法如下：

```java
CREATE TRIGGER trigger_name trigger_time
trigger_event ON tbl_name FOR EACH ROW
trigger_stmt
```

其中：

trigger_name：标识触发器名称，用户自行指定；

trigger_time：标识触发时机，取值为BEFORE或AFTER；

trigger_event：标识触发事件，取值为INSERT、UPDATE或DELETE；

tbl_name：标识建立触发器的表名，即在哪张表上建立触发器；

trigger_stmt：触发器程序体，可以是一句SQL语句，或者用BEGIN和END包含的多条语句。

由此可见，可以建立6种触发器，即：BEFOREINSERT、BEFOREUPDATE、BEFOREDELETE、AFTERINSERT、AFTERUPDATE、AFTERDELETE。

另外有一个限制是不能同时在一个表上建立2个相同类型的触发器，因此在一个表上最多建立6个触发器。假设系统中有两个表：

● 班级表class(班级号classID，班内学生数stuCount)

● 学生表student(学号stuID，所属班级号classID)

要创建触发器来使班级表中的班内学生数随着学生的添加自动更新，代码如下：

```java
create trigger tri_stuInsert after insert on student for each row
begin
declare c int;
set c = (select stuCount from class where classID=new.classID); 
update class set stuCount = c + 1 where classID = new.classID;
```

查看触发器：和查看数据库（showdatabases;）查看表格（showtables;）一样，查看触发器的语法如下：

```java
SHOW TRIGGERS [FROM schema_name];
```

其中，schema_name即Schema的名称，在MySQL中Schema和Database是一样的，也就是说，可以指定数据库名，这样就不必先“USE database_name;”了。

删除触发器：和删除数据库、删除表格一样，删除触发器的语法如下：

```java
DROP TRIGGER [IF EXISTS] [schema_name.]trigger_name
```

其中，schema_name即Schema的名称，在MySQL中Schema和Database是一样的，也就是说，可以指定数据库名，这样就不必先“USEdatabase_name;”了。

# Java面试题mysql语句优化部分

##### 1、where子句中可以对字段进行null值判断吗？

可以，比如 select id from t where num is null 这样的sql也是可以的。但是最好不要给数据库留NULL，尽可能的使用NOT NULL填充数据库。不要以为NULL不需要空间，比如：char(100) 型，在字段建立时，空间就固定了，不管是否插入值（NULL 也包含在内），都是占用100个字符的空间的，如果是varchar 这样的变长字段，null 不占用空间。可以在num上设置默认值0，确保表中num列没有null值，然后这样查询：select id from t where num= 0。

##### 2、select * from admin left join log on admin.admin_id = log.admin_id where log.admin_id>10 如何优化?

优 化 为 ： select * from (select * from admin where admin_id>10) T1 left join log on T1.admin_id =log.admin_id。使用 JOIN 时候，应该用小的结果驱动大的结果（left join 左边表结果尽量小如果有条件应该放到左边先处理， right join同理反向），同时尽量把牵涉到多表联合的查询拆分多个 query（多个连表查询效率低，容易到之后锁表和阻塞）。

##### 3、limit的基数比较大时使用between场景有那些?

例如

```java
select * from admin order by admin_id limit 100000,10
```

优化为

```java
select * from admin where admin_time> '2014-01-01′
```

# Java spring面试题及答案（1~11题）

##### 1、SpringMVC的工作原理？

● 用户向服务器发送请求，请求被springMVC前端控制器DispatchServlet捕获；

● DispatcherServle对请求URL进行解析，得到请求资源标识符（URL），然后根据该URL调用HandlerMapping将请求映射到处理器HandlerExcutionChain；

● DispatchServlet根据获得Handler选择一个合适的HandlerAdapter适配器处理；

● Handler对数据处理完成以后将返回一个ModelAndView对象给DisPatchServlet；

● Handler返回的ModelAndView只是一个逻辑视图并不是一个正式的视图，DispatcherSevlet通过ViewResolver试图解析器将逻辑视图转化为真正的视图View；

● DispatcherServle通过model解析出ModelAndView()中的参数进行解析最终展现出完整的view并返回给客户端；

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190517/1558058659@b9f9e39cca7dc9cda0035d133cc42946.png)

##### 2、SpringMVC常用注解都有哪些？

● @RequestMapping用于请求url映射。

● @RequestBody注解实现接收http请求的json数据，将json数据转换为java对象。

● @ResponseBody注解实现将controller方法返回对象转化为json响应给客户。

##### 3、如何开启注解处理器和适配器？

我们在项目中一般会在springmvc.xml中通过开启<mvc:annotation-driven>来实现注解处理器和适配器的开启。

##### 4、如何解决get和post乱码问题？

解决post请求乱码:我们可以在web.xml里边配置一个CharacterEncodingFilter过滤器。设置为utf-8.解决get请求的乱码:有两种方法。对于get请求中文参数出现乱码解决方法有两个：

● 修改tomcat配置文件添加编码与工程编码一致。

● 另 外 一 种 方 法 对 参 数 进 行 重 新 编 码 String userName = new String(request.getParameter(“userName”).getBytes(“ISO8859-1”)，“utf-8”);

##### 5、谈谈你对Spring的理解?

Spring是一个开源框架，为简化企业级应用开发而生。Spring可以是使简单的JavaBean实现以前只有EJB才能实现的功能。Spring是一个IOC和AOP容器框架。

**● Spring容器的主要核心是：**

控制反转（IOC），传统的java开发模式中，当需要一个对象时，我们会自己使用new或者getInstance等直接或者间接调用构造方法创建一个对象。而在spring开发模式中，spring容器使用了工厂模式为我们创建了所需要的对象，不需要我们自己创建了，直接调用spring提供的对象就可以了，这是控制反转的思想。

依赖注入（DI），spring使用JavaBean对象的set方法或者带参数的构造方法为我们在创建所需对象时将其属性自动设置所需要的值的过程，就是依赖注入的思想。

面向切面编程（AOP），在面向对象编程（oop）思想中，我们将事物纵向抽成一个个的对象。而在面向切面编程中，我们将一个个的对象某些类似的方面横向抽成一个切面，对这个切面进行一些如权限控制、事物管理，记录日志等公用操作处理的过程就是面向切面编程的思想。AOP底层是动态代理，如果是接口采用JDK动态代理，如果是类采用CGLIB方式实现动态代理

##### 6、Spring中的设计模式有哪些？

● 单例模式——spring中两种代理方式，若目标对象实现了若干接口，spring使用jdk的java.lang.reflect.Proxy类代理。若目标兑现没有实现任何接口，spring使用CGLIB库生成目标类的子类。单例模式——在spring的配置文件中设置bean默认为单例模式。

● 模板方式模式——用来解决代码重复的问题。比如：RestTemplate、JmsTemplate、JpaTemplate

● 前端控制器模式——spring提供了前端控制器DispatherServlet来对请求进行分发。

● 试图帮助（viewhelper）——spring提供了一系列的JSP标签，高效宏来帮助将分散的代码整合在试图中。

● 依赖注入——贯穿于BeanFactory/ApplacationContext接口的核心理念。

● 工厂模式——在工厂模式中，我们在创建对象时不会对客户端暴露创建逻辑，并且是通过使用同一个接口来指向新创建的对象。Spring中使用beanFactory来创建对象的实例。

##### 7、Spring的常用注解？

Spring在2.5版本以后开始支持注解的方式来配置依赖注入。可以用注解的方式来代替xml中bean的描述。注解注入将会被容器在XML注入之前被处理，所以后者会覆盖掉前者对于同一个属性的处理结果。

注解装配在spring中默认是关闭的。所以需要在spring的核心配置文件中配置一下才能使用基于注解的装配模式。配置方式如下：<context:annotation-config/>

**● 常用的注解：**

@Required：该注解应用于设值方法。

@Autowired：该注解应用于有值设值方法、非设值方法、构造方法和变量。

@Qualifier：该注解和@Autowired搭配使用，用于消除特定bean自动装配的歧义。

##### 8、简单介绍一下spring bean的生命周期?

● bean定义：在配置文件里面用<bean></bean>来进行定义。

● bean初始化：有两种方式初始化：

1、在配置文件中通过指定init-method属性来完成。

2、实现org.springframwork.beans.factory.InitializingBean接口。

● bean调用：有三种方式可以得到bean实例，并进行调用

● bean销毁：销毁有两种方式：

1、使用配置文件指定的destroy-method属性。

2、实现org.springframwork.bean.factory.DisposeableBean。

##### 9、Spring结构图了解吗？

● 核心容器：包括Core、Beans、Context、EL模块。

1、Core模块：封装了框架依赖的最底层部分，包括资源访问、类型转换及一些常用工具类。

2、Beans模块：提供了框架的基础部分，包括反转控制和依赖注入。其中BeanFactory是容器核心，本质是“工厂设计模式”的实现，而且无需编程实现“单例设计模式”，单例完全由容器控制，而且提倡面向接口编程，而非面向实现编程；所有应用程序对象及对象间关系由框架管理，从而真正把你从程序逻辑中把维护对象之间的依赖关系提取出来，所有这些依赖关系都由BeanFactory来维护。

3、Context模块：以Core和Beans为基础，集成Beans模块功能并添加资源绑定、数据验证、国际化、JavaEE支持、容器生命周期、事件传播等；核心接口是ApplicationContext。

4、EL模块：提供强大的表达式语言支持，支持访问和修改属性值，方法调用，支持访问及修改数组、容器和索引器，命名变量，支持算数和逻辑运算，支持从Spring容器获取Bean，它也支持列表投影、选择和一般的列表聚合等。

**● AOP、Aspects模块：**

1、AOP模块：SpringAOP模块提供了符合AOPAlliance规范的面向方面的编程（aspect-orientedprogramming）实现，提供比如日志记录、权限控制、性能统计等通用功能和业务逻辑分离的技术，并且能动态的把这些功能添加到需要的代码中；这样各专其职，降低业务逻辑和通用功能的耦合。

2、Aspects模块：提供了对AspectJ的集成，AspectJ提供了比SpringASP更强大的功能。数据访问/集成模块：该模块包括了JDBC、ORM、OXM、JMS和事务管理。

3、事务模块：该模块用于Spring管理事务，只要是Spring管理对象都能得到Spring管理事务的好处，无需在代码中进行事务控制了，而且支持编程和声明性的事务管理。

4、JDBC模块：提供了一个JBDC的样例模板，使用这些模板能消除传统冗长的JDBC编码还有必须的事务控制，而且能享受到Spring管理事务的好处。

5、ORM模块：提供与流行的“对象-关系”映射框架的无缝集成，包括Hibernate、JPA、MyBatis等。而且可以使用Spring事务管理，无需额外控制事务。

6、OXM模块：提供了一个对Object/XML映射实现，将java对象映射成XML数据，或者将XML数据映射成java对象，Object/XML映射实现包括JAXB、Castor、XMLBeans和XStream。

7、JMS模块：用于JMS(JavaMessagingService)，提供一套“消息生产者、消息消费者”模板用于更加简单的使用JMS，JMS用于用于在两个应用程序之间，或分布式系统中发送消息，进行异步通信。

8、Web/Remoting模块：Web/Remoting模块包含了Web、Web-Servlet、Web-Struts、Web-Porlet模块。

9、Web模块：提供了基础的web功能。例如多文件上传、集成IoC容器、远程过程访问（RMI、Hessian、Burlap）以及WebService支持，并提供一个RestTemplate类来提供方便的Restfulservices访问。

10、Web-Servlet模块：提供了一个SpringMVCWeb框架实现。SpringMVC框架提供了基于注解的请求资源注入、更简单的数据绑定、数据验证等及一套非常易用的JSP标签，完全无缝与Spring其他技术协作。

11、Web-Struts模块：提供了与Struts无缝集成，Struts1.x和Struts2.x都支持

12、Test模块：Spring支持Junit和TestNG测试框架，而且还额外提供了一些基于Spring的测试功能，比如在测试Web框架时，模拟Http请求的功能。

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190517/1558058832@b24702dd7fcc35cf7b8c8415e429cfc1.png)

##### 10、Spring能帮我们做什么？

● Spring能帮我们根据配置文件创建及组装对象之间的依赖关系。

● Spring根据配置文件来进行创建及组装对象间依赖关系，只需要改配置文件即可

● Spring面向切面编程能帮助我们无耦合的实现日志记录，性能统计，安全控制。

● Spring面向切面编程能提供一种更好的方式来完成，一般通过配置方式，而且不需要在现有代码中添加任何额外代码，现有代码专注业务逻辑。

● Spring能非常简单的帮我们管理数据库事务。

● 采用Spring，我们只需获取连接，执行SQL，其他事物相关的都交给Spring来管理了。

● Spring还能与第三方数据库访问框架（如Hibernate、JPA）无缝集成，而且自己也提供了一套JDBC访问模板，来方便数据库访问。

● Spring还能与第三方Web（如Struts、JSF）框架无缝集成，而且自己也提供了一套SpringMVC框架，来方便web层搭建。

● Spring能方便的与JavaEE（如JavaMail、任务调度）整合，与更多技术整合（比如缓存框架）。

##### 11、请描述一下Spring的事务？

声明式事务管理的定义：用在Spring配置文件中声明式的处理事务来代替代码式的处理事务。这样的好处是，事务管理不侵入开发的组件，具体来说，业务逻辑对象就不会意识到正在事务管理之中，事实上也应该如此，因为事务管理是属于系统层面的服务，而不是业务逻辑的一部分，如果想要改变事务管理策划的话，也只需要在定义文件中重新配置即可，这样维护起来极其方便。

基于TransactionInterceptor的声明式事务管理：两个次要的属性：transactionManager，用来指定一个事务治理器，并将具体事务相关的操作请托给它；其他一个是Properties类型的transactionAttributes属性，该属性的每一个键值对中，键指定的是方法名，方法名可以行使通配符，而值就是表现呼应方法的所运用的事务属性。

```java
<beans>
    ......
    <bean id="transactionInterceptor"
          class="org.springframework.transaction.interceptor.TransactionInterceptor">
        <property name="transactionManager" ref="transactionManager"/>
        <property name="transactionAttributes">
            <props>
                <prop key="transfer">PROPAGATION_REQUIRED</prop>
            </props>
        </property>
    </bean>
    <bean id="bankServiceTarget"
          class="footmark.spring.core.tx.declare.origin.BankServiceImpl">
        <property name="bankDao" ref="bankDao"/>
    </bean>
    <bean id="bankService"
          class="org.springframework.aop.framework.ProxyFactoryBean">
        <property name="target" ref="bankServiceTarget"/>
        <property name="interceptorNames">
            <list>
                <idref bean="transactionInterceptor"/>
            </list>
        </property>
    </bean>
</beans>
```

基于 TransactionProxyFactoryBean 的声明式事务管理：设置配置文件与先前比照简化了许多。我们把这类设置配置文件格式称为 Spring 经典的声明式事务治理。 

```java
<beans>
    ......
    <bean id="bankServiceTarget"
          class="footmark.spring.core.tx.declare.classic.BankServiceImpl">
        <property name="bankDao" ref="bankDao"/>
    </bean>
    <bean id="bankService"
          class="org.springframework.transaction.interceptor.TransactionProxyFactoryBean">
        <property name="target" ref="bankServiceTarget"/>
        <property name="transactionManager" ref="transactionManager"/>
        <property name="transactionAttributes">
            <props>
                <prop key="transfer">PROPAGATION_REQUIRED</prop>
            </props>
        </property>
    </bean>
</beans>
```

基于  <tx>  命名空间的声明式事务治理：在前两种方法的基础上，Spring 2.x  引入了  <tx>  命名空间， 连络行使 <aop> 命名空间，带给开发人员设置配备声明式事务的全新体验。

```java
<beans>
    ......
    <bean id="bankService"
          class="footmark.spring.core.tx.declare.namespace.BankServiceImpl">
        <property name="bankDao" ref="bankDao"/>
    </bean>
    <tx:advice id="bankAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="transfer" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>
    <aop:config>
        <aop:pointcut id="bankPointcut" expression="execution(* *.transfer(..))"/>
        <aop:advisor advice-ref="bankAdvice" pointcut-ref="bankPointcut"/>
    </aop:config>
    ......
</beans>
```

基于  @Transactional  的声明式事务管理：Spring  2.x  还引入了基于  Annotation  的体式格式，具体次要触及@Transactional  标注。@Transactional  可以浸染于接口、接口方法、类和类方法上。算作用于类上时，该类的一切public 方法将都具有该类型的事务属性。

```java
@Transactional(propagation = Propagation.REQUIRED)
public boolean transfer(Long fromId, Long toId, double amount) {
    return bankDao.transfer(fromId, toId, amount);
}
```

编程式事务管理的定义：在代码中显式挪用 beginTransaction()、commit()、rollback()等事务治理相关的方法， 这就是编程式事务管理。Spring 对事物的编程式管理有基于底层 API 的编程式管理和基于 TransactionTemplate 的编程式事务管理两种方式。

基于底层 API 的编程式管理：凭证 PlatformTransactionManager 、 TransactionDefinition 和TransactionStatus 三个焦点接口，来实现编程式事务管理。

```java
public class BankServiceImpl implements BankService {
    private BanckDao bankDao;
    private TransactionDefinition txDefinition;
    private PlatformTransactionManager txManager;

    public boolean transfer(Long fromId, Long toId, double amount) {
        TransactionStatus txStatus = txManager.getTransaction(txDefinition);
        boolean result = false;
        try {
            result = bankDao.transfer(fromId, toId, amount);
            txManager.commit(txStatus);
        } catch (Exception e) {
            result = false;
            txManager.rollback(txStatus);
            System.out.println("Transfer Error!");
        }
        return result;
    }
}
```

基于 TransactionTemplate  的编程式事务管理:为了不损坏代码原有的条理性，避免出现每一个方法中都包括相同的启动事物、提交、回滚事物样板代码的现象，spring 提供了 transactionTemplate 模板来实现编程式事务管理。

```java
public class BankServiceImpl implements BankService {
    private BankDao bankDao;
    private TransactionTemplate transactionTemplate;

    public boolean transfer(final Long fromId, final Long toId, final double amount) {
        return (Boolean) transactionTemplate.execute(new TransactionCallback() {
            public Object doInTransaction(TransactionStatus status) {
                Object result;
                try {
                    result = bankDao.transfer(fromId, toId, amount);
                } catch (Exception e) {
                    status.setRollbackOnly();
                    result = false;
                    System.out.println("Transfer Error!");
                }
                return result;
            }
        });
    }
}
```

编程式事务与声明式事务的区别：

编程式事务是自己写事务处理的类，然后调用。

声明式事务是在配置文件中配置，一般搭配在框架里面使用。

# Java spring面试题及答案（12~44题）

12、BeanFactory常用的实现类有哪些？

Bean工厂是工厂模式的一个实现，提供了控制反转功能，用来把应用的配置和依赖从正真的应用代码中分离。常用的BeanFactory实现有DefaultListableBeanFactory、XmlBeanFactory、ApplicationContext等。XMLBeanFactory，最常用的就是org.springframework.beans.factory.xml.XmlBeanFactory，它根据XML文件中的定义加载beans。该容器从XML文件读取配置元数据并用它去创建一个完全配置的系统或应用。

##### 13、解释SpringJDBC、SpringDAO和SpringORM?

Spring-DAO并非Spring的一个模块，它实际上是指示你写DAO操作、写好DAO操作的一些规范。因此，对于访问你的数据它既没有提供接口也没有提供实现更没有提供模板。在写一个DAO的时候，你应该使用@Repository对其进行注解，这样底层技术(JDBC，Hibernate，JPA，等等)的相关异常才能一致性地翻译为相应的DataAccessException子类。

Spring-JDBC提供了Jdbc模板类，它移除了连接代码以帮你专注于SQL查询和相关参数。Spring-JDBC还提供了一个JdbcDaoSupport，这样你可以对你的DAO进行扩展开发。它主要定义了两个属性：一个DataSource和一个JdbcTemplate，它们都可以用来实现DAO方法。JdbcDaoSupport还提供了一个将SQL异常转换为SpringDataAccessExceptions的异常翻译器。

Spring-ORM是一个囊括了很多持久层技术(JPA，JDO，Hibernate，iBatis)的总括模块。对于这些技术中的每一个，Spring都提供了集成类，这样每一种技术都能够在遵循Spring的配置原则下进行使用，并平稳地和Spring事务管理进行集成。

对于每一种技术，配置主要在于将一个DataSourcebean注入到某种SessionFactory或者EntityManagerFactory等bean中。纯JDBC不需要这样的一个集成类(JdbcTemplate除外)，因为JDBC仅依赖于一个DataSource。

如果你计划使用一种ORM技术，比如JPA或者Hibernate，那么你就不需要Spring-JDBC模块了，你需要的是这个Spring-ORM模块。

##### 14、简单介绍一下SpringWEB模块?

Spring的WEB模块是构建在applicationcontext模块基础之上，提供一个适合web应用的上下文。这个模块也包括支持多种面向web的任务，如透明地处理多个文件上传请求和程序级请求参数的绑定到你的业务对象。它也有对JakartaStruts的支持。

##### 15、Spring配置文件有什么作用？

Spring配置文件是个XML文件，这个文件包含了类信息，描述了如何配置它们，以及如何相互调用。

##### 16、什么是SpringIOC容器？

IOC控制反转：SpringIOC负责创建对象，管理对象。通过依赖注入（DI），装配对象，配置对象，并且管理这些对象的整个生命周期。

##### 17、IOC的优点是什么？

IOC或依赖注入把应用的代码量降到最低。它使应用容易测试，单元测试不再需要单例和JNDI查找机制。最小的代价和最小的侵入性使松散耦合得以实现。IOC容器支持加载服务时的饿汉式初始化和懒加载。

##### 18、ApplicationContext的实现类有哪些?

FileSystemXmlApplicationContext：此容器从一个XML文件中加载beans的定义，XMLBean配置文件的全路径名必须提供给它的构造函数。

ClassPathXmlApplicationContext：此容器也从一个XML文件中加载beans的定义，这里，你需要正确设置classpath因为这个容器将在classpath里找bean配置。

WebXmlApplicationContext：此容器加载一个XML文件，此文件定义了一个WEB应用的所有bean。

##### 19、BeanFactory与AppliacationContext有什么区别？

**● BeanFactory**

基础类型的IOC容器，提供完成的IOC服务支持。如果没有特殊指定，默认采用延迟初始化策略。相对来说，容器启动初期速度较快，所需资源有限。

**● ApplicationContext**

ApplicationContext是在BeanFactory的基础上构建，是相对比较高级的容器实现，除了BeanFactory的所有支持外，ApplicationContext还提供了事件发布、国际化支持等功能。ApplicationContext管理的对象，在容器启动后默认全部初始化并且绑定完成。

##### 20、什么是Spring的依赖注入？

平常的java开发中，程序员在某个类中需要依赖其它类的方法，则通常是new一个依赖类再调用类实例的方法，这种开发存在的问题是new的类实例不好统一管理，spring提出了依赖注入的思想，即依赖类不由程序员实例化，而是通过spring容器帮我们new指定实例并且将实例注入到需要该对象的类中。依赖注入的另一种说法是“控制反转”，通俗的理解是：平常我们new一个实例，这个实例的控制权是我们程序员，而控制反转是指new实例工作不由我们程序员来做而是交给spring容器来做。

##### 21、有哪些不同类型的IOC（依赖注入）方式？

Spring提供了多种依赖注入的方式。

● set注入

● 构造器注入

● 静态工厂的方法注入

● 实例工厂的方法注入

##### 22、什么是Springbeans?

Springbeans是那些形成Spring应用的主干的java对象。它们被SpringIOC容器初始化，装配，和管理。这些beans通过容器中配置的元数据创建。比如，以XML文件中<bean/>的形式定义。Spring框架定义的beans都是单例beans。

##### 23、一个SpringBeans的定义需要包含什么？

一个SpringBean的定义包含容器必知的所有配置元数据，包括如何创建一个bean，它的生命周期详情及它的依赖。

##### 24、你怎样定义类的作用域?

当定义一个<bean>在Spring里，我们还能给这个bean声明一个作用域。它可以通过bean定义中的scope属性来定义。如，当Spring要在需要的时候每次生产一个新的bean实例，bean的scope属性被指定为prototype。另一方面，一个bean每次使用的时候必须返回同一个实例，这个bean的scope属性必须设为singleton。

##### 25、Spring支持bean的作用域有几种？

Spring框架支持以下五种bean的作用域：

● singleton:bean在每个Springioc容器中只有一个实例。

● prototype：一个bean的定义可以有多个实例。

● request：每次http请求都会创建一个bean，该作用域仅在基于web的SpringApplicationContext情形下有效。

● session：在一个HTTPSession中，一个bean定义对应一个实例。该作用域仅在基于web的SpringApplicationContext情形下有效。

● global-session：在一个全局的HTTPSession中，一个bean定义对应一个实例。该作用域仅在基于web的SpringApplicationContext情形下有效。缺省的Springbean的作用域是Singleton。

##### 26、Spring框架中的单例bean是线程安全的吗?

Spring框架中的单例bean不是线程安全的。

##### 27、什么是Spring的内部bean？

当一个bean仅被用作另一个bean的属性时，它能被声明为一个内部bean，为了定义innerbean，在Spring的基于XML的配置元数据中，可以在<property/>或<constructor-arg/>元素内使用<bean/>元素，内部bean通常是匿名的，它们的Scope一般是prototype。

##### 28、在Spring中如何注入一个java集合？

Spring提供以下几种集合的配置元素：

● <list>类型用于注入一列值，允许有相同的值。

● <set>类型用于注入一组值，不允许有相同的值。

● <map>类型用于注入一组键值对，键和值都可以为任意类型。

● <props>类型用于注入一组键值对，键和值都只能为String类型。

##### 29、什么是bean的自动装配？

无须在Spring配置文件中描述javaBean之间的依赖关系（如配置<property>、<constructor-arg>）。IOC容器会自动建立javabean之间的关联关系。

##### 30、解释不同方式的自动装配？

有五种自动装配的方式，可以用来指导Spring容器用自动装配方式来进行依赖注入。

● no：默认的方式是不进行自动装配，通过显式设置ref属性来进行装配。

● byName：通过参数名自动装配，Spring容器在配置文件中发现bean的autowire属性被设置成byname，之后容器试图匹配、装配和该bean的属性具有相同名字的bean。

● byType:：通过参数类型自动装配，Spring容器在配置文件中发现bean的autowire属性被设置成byType，之后容器试图匹配、装配和该bean的属性具有相同类型的bean。如果有多个bean符合条件，则抛出错误。

● constructor：这个方式类似于byType，但是要提供给构造器参数，如果没有确定的带参数的构造器参数类型，将会抛出异常。

● autodetect：首先尝试使用constructor来自动装配，如果无法工作，则使用byType方式。

##### 31、什么是基于Java的Spring注解配置?

基于Java的配置，允许你在少量的Java注解的帮助下，进行你的大部分Spring配置而非通过XML文件。以@Configuration注解为例，它用来标记类可以当做一个bean的定义，被SpringIOC容器使用。另一个例子是@Bean注解，它表示此方法将要返回一个对象，作为一个bean注册进Spring应用上下文。

##### 32、什么是基于注解的容器配置?

相对于XML文件，注解型的配置依赖于通过字节码元数据装配组件，而非尖括号的声明。开发者通过在相应的类，方法或属性上使用注解的方式，直接组件类中进行配置，而不是使用xml表述bean的装配关系。

##### 33、怎样开启注解装配？

注解装配在默认情况下是不开启的，为了使用注解装配，我们必须在Spring配置文件中配置<context:annotation-config/>元素。

##### 34、在Spring框架中如何更有效地使用JDBC?

使用SpringJDBC框架，资源管理和错误处理的代价都会被减轻。所以开发者只需写statements和queries从数据存取数据，JDBC也可以在Spring框架提供的模板类的帮助下更有效地被使用，这个模板叫JdbcTemplate。JdbcTemplate类提供了很多便利的方法解决诸如把数据库数据转变成基本数据类型或对象，执行写好的或可调用的数据库操作语句，提供自定义的数据错误处理。

##### 35、使用Spring通过什么方式访问Hibernate？

在Spring中有两种方式访问Hibernate：

● 控制反转：HibernateTemplate和Callback。

● 继承HibernateDAOSupport提供一个AOP拦截器。

##### 36、Spring支持的ORM框架有哪些？

Spring支持以下ORM框架：

● Hibernate

● MyBatis

● JPA (Java Persistence API)

● TopLink

● JDO (Java Data Objects)

● OJB

##### 37、简单解释一下Spring的AOP？

AOP（Aspect Oriented Programming），即面向切面编程，可以说是OOP（Object Oriented Programming，面向对象编程）的补充和完善。OOP引入封装、继承、多态等概念来建立一种对象层次结构，用于模拟公共行为的一个集合。不过OOP允许开发者定义纵向的关系，但并不适合定义横向的关系，例如日志功能。日志代码往往横向地散布在所有对象层次中，而与它对应的对象的核心功能毫无关系。对于其他类型的代码，如安全性、异常处理和透明的持续性也都是如此。这种散布在各处的无关的代码被称为横切（cross cutting），在OOP设计中，它导致了大量代码的重复，而不利于各个模块的重用。

AOP技术恰恰相反，它利用一种称为“横切”的技术，剖解开封装的对象内部，并将那些影响了多个类的公共行为封装到一个可重用模块，并将其命名为“Aspect”，即切面。所谓“切面”，简单说就是那些与业务无关，却为业务模块所共同调用的逻辑或责任封装起来，便于减少系统的重复代码，降低模块之间的耦合度，并有利于未来的可操作性和可维护性。使用“横切”技术，AOP把软件系统分为两个部分：核心关注点和横切关注点。业务处理的主要流程是核心关注点，与之关系不大的部分是横切关注点。横切关注点的一个特点是，他们经常发生在核心关注点的多处，而各处基本相似，比如权限认证、日志、事物。AOP的作用在于分离系统中的各种关注点，将核心关注点和横切关注点分离开来。AOP核心就是切面，它将多个类的通用行为封装成可重用的模块，该模块含有一组API提供横切功能。比如，一个日志模块可以被称作日志的AOP切面。根据需求的不同，一个应用程序可以有若干切面。在Spring AOP中，切面通过带有@Aspect注解的类实现。

##### 38、在Spring AOP中，关注点和横切关注的区别是什么？

关注点是应用中一个模块的行为，一个关注点可能会被定义成一个我们想实现的一个功能。横切关注点是一个关注点，此关注点是整个应用都会使用的功能，并影响整个应用，比如日志，安全和数据传输，几乎应用的每个模块都需要的功能。因此这些都属于横切关注点。

##### 39、什么是连接点？

被拦截到的点，因为Spring只支持方法类型的连接点，所以在Spring中连接点指的就是被拦截到的方法，实际上连接点还可以是字段或者构造器。

##### 40、Spring的通知是什么？有哪几种类型？

通知是个在方法执行前或执行后要做的动作，实际上是程序执行时要通过SpringAOP框架触发的代码段。

Spring切面可以应用五种类型的通知：

● before：前置通知，在一个方法执行前被调用。

● after：在方法执行之后调用的通知，无论方法执行是否成功。

● after-returning：仅当方法成功完成后执行的通知。

● after-throwing：在方法抛出异常退出时执行的通知。

● around：在方法执行之前和之后调用的通知。

##### 41、什么是切入点？

切入点是一个或一组连接点，通知将在这些位置执行。可以通过表达式或匹配的方式指明切入点。

##### 42、什么是目标对象？

被一个或者多个切面所通知的对象。它通常是一个代理对象。也指被通知（advised）对象。

##### 43、什么是代理？

代理是通知目标对象后创建的对象。从客户端的角度看，代理对象和目标对象是一样的。

##### 44、什么是织入？什么是织入应用的不同点？****

把切面（aspect）连接到其它的应用程序类型或者对象上，并创建一个被通知（advised）的对象，这样的行为叫做织入。织入可以在编译时，加载时，或运行时完成。

# Java mybatis面试题及答案

##### 1、Mybatis中#和$的区别？

● #相当于对数据加上双引号，$相当于直接显示数据

● #将传入的数据都当成一个字符串，会对自动传入的数据加一个双引号。如：order by#user_id#，如果传入的值是111，那么解析成sql时的值为order by"111"，如果传入的值是id，则解析成的sql为order by "id".

● $将传入的数据直接显示生成在sql中。如：order by$user_id$，如果传入的值是111，那么解析成sql时的值为order by user_id，如果传入的值是id，则解析成的sql为order by id.

● #方式能够很大程度防止sql注入，$方式无法防止Sql注入。

● $方式一般用于传入数据库对象，例如传入表名.

##### 2、Mybatis的编程步骤是什么样的？

● 创建SqlSessionFactory

● 通过SqlSessionFactory创建SqlSession

● 通过sqlsession执行数据库操作

● 调用session.commit()提交事务

● 调用session.close()关闭会话

##### 3、JDBC编程有哪些不足之处，MyBatis是如何解决这些问题的？

● 数据库链接创建、释放频繁造成系统资源浪费从而影响系统性能，使用数据库链接池可解决此问题。解决：在SqlMapConfig.xml中配置数据链接池，使用连接池管理数据库链接。

● Sql语句写在代码中造成代码不易维护，实际应用sql变化的可能较大，sql变动需要改变java代码。解决：将Sql语句配置在XXXXmapper.xml文件中与java代码分离。

● 向sql语句传参数麻烦，因为sql语句的where条件不一定，可能多也可能少，占位符需要和参数一一对应。解决： Mybatis 自动将 java 对象映射至 sql 语句。

● 对结果集解析麻烦，sql 变化导致解析代码变化，且解析前需要遍历，如果能将数据库记录封装成 pojo 对象解析比较方便。解决：Mybatis 自动将 sql 执行结果映射至 java 对象。

##### 4、使用MyBatis的mapper接口调用时有哪些要求？

● Mapper接口方法名和mapper.xml中定义的每个sql的id相同

● Mapper接口方法的输入参数类型和mapper.xml中定义的每个sql的parameterType的类型相同

● Mapper接口方法的输出参数类型和mapper.xml中定义的每个sql的resultType的类型相同

● Mapper.xml文件中的namespace即是mapper接口的类路径。

##### 5、Mybatis中一级缓存与二级缓存？

● 一级缓存：基于PerpetualCache的HashMap本地缓存，其存储作用域为Session，当flush或close之后，该Session中的所有Cache就将清空。

● 二级缓存与一级缓存其机制相同，默认也是采用PerpetualCache的HashMap存储，不同在于其存储作用域为Mapper(namespace)，并且可自定义存储源，如Ehcache。作用域namespace是指对namespace所对应的配置文件中所有的select操作结果都缓存，这样不同线程之间就可以共用二级缓存。二级缓存可以设置返回的缓存对象策略：<cache readOnly="true">。当readOnly="true"时，表示二级缓存返回给所有调用者同一个缓存对象实例，调用者可以update获取的缓存实例，但是这样可能会造成其他调用者出现数据不一致的情况（因为所有调用者调用的是同一个实例）。当readOnly=“false”时，返回给调用者的是二级缓存总缓存对象的拷贝，即不同调用者获取的是缓存对象不同的实例，这样调用者对各自的缓存对象的修改不会影响到其他的调用者，即是安全的，所以默认是readOnly="false";

● 对于缓存数据更新机制，当某一个作用域(一级缓存Session/二级缓存Namespaces)进行了C/U/D操作后，默认该作用域下所有select中的缓存将被clear。

##### 6、MyBatis在insert插入操作时如何返回主键ID？

数据库为 MySql 时：

```java
<insert id="insert" parameterType="com.test.User" keyProperty="userId" useGeneratedKeys="true">
```

“keyProperty”表示返回的id要保存到对象的属性中，“useGeneratedKeys”表示主键id为自增长模式。

数据库为Oracle时：

```java
<insert id="insert" parameterType="com.test.User">
    <selectKey resultType="INTEGER" order="BEFORE" keyProperty="userId">
        SELECT SEQ_USER.NEXTVAL as userId from DUAL
    </selectKey>
    insert into user 
        (user_id, user_name, modified, state)
    values 
        (#{userId,jdbcType=INTEGER}, #{userName,jdbcType=VARCHAR}, #{modified,jdbcType=TIMESTAMP},#{state,jdbcType=INTEGER})
</insert>
```

由于Oracle没有自增长这一说法，只有序列这种自增的形式，所以不能再使用“useGeneratedKeys”属性。而是使用<selectKey>将ID获取并赋值到对象的属性中，insert插入操作时正常插入id。

# Java Redis面试题

##### 1、Redis的特点？

Redis是由意大利人Salvatore Sanfilippo（网名：antirez）开发的一款内存高速缓存数据库。Redis全称为：Remote Dictionary Server（远程数据服务），该软件使用C语言编写，典型的NoSQL数据库服务器。Redis是一个key-value存储系统，它支持丰富的数据类型，如：string、list、set、zset(sorted set)、hash。

Redis本质上是一个Key-Value类型的内存数据库，很像memcached，整个数据库统统加载在内存当中进行操作，定期通过异步操作把数据库数据flush到硬盘上进行保存。因为是纯内存操作，Redis的性能非常出色，每秒可以处理超过10万次读写操作，是已知性能最快的Key-Value DB。

Redis的出色之处不仅仅是性能，Redis最大的魅力是支持保存多种数据结构，此外单个value的最大限制是1GB，不像memcached只能保存1MB的数据，另外Redis也可以对存入的Key-Value设置expire时间。

Redis的主要缺点是数据库容量受到物理内存的限制，不能用作海量数据的高性能读写，因此Redis适合的场景主要局限在较小数据量的高性能操作和运算上。

##### 2、为什么redis需要把所有数据放到内存中？

Redis为了达到最快的读写速度将数据都读到内存中，并通过异步的方式将数据写入磁盘。所以redis具有快速和数据持久化的特征。如果不将数据放在内存中，磁盘I/O速度为严重影响redis的性能。在内存越来越便宜的今天，redis将会越来越受欢迎。如果设置了最大使用的内存，则数据已有记录数达到内存限值后不能继续插入新值。

##### 3、Redis常见的性能问题都有哪些？如何解决？

● Master写内存快照，save命令调度rdbSave函数，会阻塞主线程的工作，当快照比较大时对性能影响是非常大的，会间断性暂停服务，所以Master最好不要写内存快照。

● Master AOF持久化，如果不重写AOF文件，这个持久化方式对性能的影响是最小的，但是AOF文件会不断增大，AOF文件过大会影响Master重启的恢复速度。Master最好不要做任何持久化工作，包括内存快照和AOF日志文件，特别是不要启用内存快照做持久化，如果数据比较关键，某个Slave开启AOF备份数据，策略为每秒同步一次。

● Master调用BGREWRITEAOF重写AOF文件，AOF在重写的时候会占大量的CPU和内存资源，导致服务load过高，出现短暂服务暂停现象。

● Redis主从复制的性能问题，为了主从复制的速度和连接的稳定性，Slave和Master最好在同一个局域网内。

##### 4、Redis最适合的场景有哪些？

● 会话缓存（Session Cache）

● 全页缓存（FPC）

● 队列

● 排行榜/计数器

● 发布/订阅

##### 5、Memcache与Redis的区别都有哪些？

● 存储方式不同，Memcache是把数据全部存在内存中，数据不能超过内存的大小，断电后数据库会挂掉。Redis有部分存在硬盘上，这样能保证数据的持久性。

● 数据支持的类型不同memcahe对数据类型支持相对简单，redis有复杂的数据类型。

● 使用底层模型不同 它们之间底层实现方式以及与客户端之间通信的应用协议不一样。Redis直接自己构建了VM机制，因为一般的系统调用系统函数的话，会浪费一定的时间去移动和请求。

● 支持的value大小不一样redis最大可以达到1GB，而memcache只有1MB。

##### 6、Redis有哪几种数据结构？

● String——字符串

String数据结构是简单的key-value类型，value不仅可以是String，也可以是数字（当数字类型用Long可以表示的时候encoding就是整型，其他都存储在sdshdr当做字符串）。

● Hash——字典

在Memcached中，我们经常将一些结构化的信息打包成hashmap，在客户端序列化后存储为一个字符串的值（一般是JSON格式），比如用户的昵称、年龄、性别、积分等。

● List——列表

List说白了就是链表（redis使用双端链表实现的List）

● Set——集合

Set就是一个集合，集合的概念就是一堆不重复值的组合。利用Redis提供的Set数据结构，可以存储一些集合性的数据。

● Sorted Set——有序集合

和Set相比，Sorted Set是将Set中的元素增加了一个权重参数score，使得集合中的元素能够按score进行有序排列，

● 带有权重的元素，比如一个游戏的用户得分排行榜

● 比较复杂的数据结构，一般用到的场景不算太多

##### 7、Redis的优缺点？

**● 优点：**

性能极高–Redis能支持超过100K+每秒的读写频率。

丰富的数据类型–Redis支持二进制案例的Strings，Lists，Hashes，Sets及Ordered Sets数据类型操作。

原子–Redis的所有操作都是原子性的，同时Redis还支持对几个操作全并后的原子性执行。attention原子性定义：例如，A想要从自己的帐户中转1000块钱到B的帐户里。那个从A开始转帐，到转帐结束的这一个过程，称之为一个事务。如果在A的帐户已经减去了1000块钱的时候，忽然发生了意外，比如停电什么的，导致转帐事务意外终止了，而此时B的帐户里还没有增加1000块钱。那么，我们称这个操作失败了，要进行回滚。回滚就是回到事务开始之前的状态，也就是回到A的帐户还没减1000块的状态，B的帐户的原来的状态。此时A的帐户仍然有3000块，B的帐户仍然有2000块。我们把这种要么一起成功（A帐户成功减少1000，同时B帐户成功增加1000），要么一起失败（A帐户回到原来状态，B帐户也回到原来状态）的操作叫原子性操作。如果把一个事务可看作是一个程序，它要么完整的被执行，要么完全不执行，这种特性就叫原子性。

丰富的特性–Redis还支持publish/subscribe，通知key过期等等特性。

**● 缺点：**

由于是内存数据库，所以单台机器存储的数据量跟机器本身的内存大小不一样。虽然redis本身有key过期策略，但是还是需要提前预估和节约内存。如果内存增长过快，需要定期删除数据。

如果进行完整重同步，由于需要生成rdb文件，并进行传输，会占用主机的CPU，并会消耗现网的带宽。不过redis2.8版本，已经有部分重同步的功能，但是还是有可能有完整重同步的。比如：新上线的备机。

修改配置文件，进行重启，将硬盘中的数据加载进内存，时间比较久。在这个过程中，redis不能提供服务。

##### 8、Redis的持久化是什么？

RDB持久化：该机制可以在指定的时间间隔内生成数据集的时间点快照（point-in-time snapshot）。

AOF持久化：记录服务器执行的所有写操作命令，并在服务器启动时，通过重新执行这些命令来还原数据集。AOF文件中的命令全部以Redis协议的格式来保存，新命令会被追加到文件的末尾。Redis还可以在后台对AOF文件进行重写（rewrite），使得AOF文件的体积不会超出保存数据集状态所需的实际大小。

AOF和RDB的同时应用：当Redis重启时，它会优先使用AOF文件来还原数据集，因为AOF文件保存的数据集通常比RDB文件所保存的数据集更完整。

##### 9、RDB的优缺点？

优点：RDB是一个非常紧凑（compact）的文件，它保存了Redis在某个时间点上的数据集。这种文件非常适合用于进行备份：比如说，你可以在最近的24小时内，每小时备份一次RDB文件，并且在每个月的每一天，也备份一个RDB文件。这样的话，即使遇上问题，也可以随时将数据集还原到不同的版本。RDB非常适用于灾难恢复（disaster recovery）：它只有一个文件，并且内容都非常紧凑，可以（在加密后）将它传送到别的数据中心，或者亚马逊S3中。RDB可以最大化Redis的性能：父进程在保存RDB文件时唯一要做的就是fork出一个子进程，然后这个子进程就会处理接下来的所有保存工作，父进程无须执行任何磁盘I/O操作。RDB在恢复大数据集时的速度比AOF的恢复速度要快。

缺点：如果你需要尽量避免在服务器故障时丢失数据，那么RDB不适合你。虽然Redis允许你设置不同的保存点（save point）来控制保存RDB文件的频率，但是，因为RDB文件需要保存整个数据集的状态，所以它并不是一个轻松的操作。因此你可能会至少5分钟才保存一次RDB文件。在这种情况下，一旦发生故障停机，你就可能会丢失好几分钟的数据。每次保存RDB的时候，Redis都要fork()出一个子进程，并由子进程来进行实际的持久化工作。在数据集比较庞大时，fork()可能会非常耗时，造成服务器在某某毫秒内停止处理客户端；如果数据集非常巨大，并且CPU时间非常紧张的话，那么这种停止时间甚至可能会长达整整一秒。

##### 10、AOF的优缺点？

**● 优点：**

使用AOF持久化会让Redis变得非常耐久（much more durable）：你可以设置不同的fsync策略，比如无fsync，每秒钟一次fsync，或者每次执行写入命令时fsync。AOF的默认策略为每秒钟fsync一次，在这种配置下，Redis仍然可以保持良好的性能，并且就算发生故障停机，也最多只会丢失一秒钟的数据（fsync会在后台线程执行，所以主线程可以继续努力地处理命令请求）。AOF文件是一个只进行追加操作的日志文件（append onlylog），因此对AOF文件的写入不需要进行seek，即使日志因为某些原因而包含了未写入完整的命令（比如写入时磁盘已满，写入中途停机，等等），redis-check-aof工具也可以 轻易地修复这种问题。

Redis可以在AOF文件体积变得过大时，自动地在后台对AOF进行重写：重写后的新AOF文件包含了恢复当前数据集所需的最小命令集合。整个重写操作是绝对安全的，因为Redis在创建新AOF文件的过程中，会继续将命令追加到现有的AOF文件里面，即使重写过程中发生停机，现有的AOF文件也不会丢失。而一旦新AOF文件创建完毕，Redis就会从旧AOF文件切换到新AOF文件，并开始对新AOF文件进行追加操作。

**● 缺点：**

对于相同的数据集来说，AOF文件的体积通常要大于RDB文件的体积。根据所使用的fsync策略，AOF的速度可能会慢于RDB。在一般情况下，每秒fsync的性能依然非常高，而关闭fsync可以让AOF的速度和RDB一样快，即使在高负荷之下也是如此。不过在处理巨大的写入载入时，RDB可以提供更有保证的最大延迟时间（latency）。

AOF在过去曾经发生过这样的bug：因为个别命令的原因，导致AOF文件在重新载入时，无法将数据集恢复成保存时的原样。（举个例子，阻塞命令BRPOPLPUSH就曾经引起过这样的bug。）测试套件里为这种情况添加了测试：它们会自动生成随机的、复杂的数据集，并通过重新载入这些数据来确保一切正常。虽然这种bug在AOF文件中并不常见，但是对比来说，RDB几乎是不可能出现这种bug的。

# Java高并发面试题

1、如何测试并发量？

可以使用apache提供的ab工具测试。

##### 2、Nginx反向代理为什么能够提升服务器性能？

对于后端是动态服务来说，比如Java和PHP。这类服务器(如JBoss和PHP-FPM)的IO处理能力往往不高。Nginx有个好处是它会把Request在读取完整之前buffer住，这样交给后端的就是一个完整的HTTP请求，从而提高后端的效率，而不是断断续续的传递(互联网上连接速度一般比较慢)。同样，Nginx也可以把response给buffer住，同样也是减轻后端的压力。

##### 3、Nginx和Apache各有什么优缺点？

**● nginx相对apache的优点：**

轻量级，同样起web服务，比apache占用更少的内存及资源

抗并发，nginx处理请求是异步非阻塞的，而apache则是阻塞型的，在高并发下nginx能保持

低资源低消耗高性能

高度模块化的设计，编写模块相对简单

社区活跃，各种高性能模块出品迅速

**● Apache相对nginx的优点：**

Apache的rewrite比nginx的rewrite强大

Apache模块超多，基本想到的都可以找到

Apache的bug少，nginx的bug相对较多

Apache超稳定，一般来说，需要性能的web服务用nginx。如果不需要性能只求稳定，那就用apache

##### 4、Nginx多进程模型是如何实现高并发的？

进程数与并发数不存在很直接的关系。这取决取server采用的工作方式。如果一个server采用一个进程负责一个request的方式，那么进程数就是并发数。那么显而易见的，就是会有很多进程在等待中。等什么？最多的应该是等待网络传输。

Nginx的异步非阻塞工作方式正是利用了这点等待的时间。在需要等待的时候，这些进程就空闲出来待命了。因此表现为少数几个进程就解决了大量的并发问题。apache是如何利用的呢，简单来说：同样的4个进程，如果采用一个进程负责一个request的方式，那么，同时进来4个request之后，每个进程就负责其中一个，直至会话关闭。期间，如果有第5个request进来了。就无法及时反应了，因为4个进程都没干完活呢，因此，一般有个调度进程，每当新进来了一个request，就新开个进程来处理。nginx不这样，每进来一个request，会有一个worker进程去处理。但不是全程的处理，处理到什么程度呢？处理到可能发生阻塞的地方，比如向上游（后端）服务器转发request，并等待请求返回。那么，这个处理的worker不会这么傻等着，他会在发送完请求后，注册一个事件：“如果upstream返回了，告诉我一声，我再接着干”。于是他就休息去了。此时，如果再有request进来，他就可以很快再按这种方式处理。而一旦上游服务器返回了，就会触发这个事件，worker才会来接手，这个request才会接着往下走。由于web server的工作性质决定了每个request的大部份生命都是在网络传输中，实际上花费在server机器上的时间片不多。这是几个进程就解决高并发的秘密所在。webserver刚好属于网络io密集型应用，不算是计算密集型。异步，非阻塞，使用epoll，和大量细节处的优化。也正是nginx之所以然的技术基石。

##### 5、简单介绍一下zookeeper以及zookeeper的原理？

ZooKeeper是一个分布式的，开放源码的分布式应用程序协调服务，是Google的Chubby一个开源的实现，是Hadoop和Hbase的重要组件。它是一个为分布式应用提供一致性服务的软件，提供的功能包括：配置维护、域名服务、分布式同步、组服务等。

ZooKeeper的目标就是封装好复杂易出错的关键服务，将简单易用的接口和性能高效、功能稳定的系统提供给用户。ZooKeeper包含一个简单的原语集，提供Java和C的接口。ZooKeeper代码版本中，提供了分布式独享锁、选举、队列的接口，代码在zookeeper-3.4.3\src\recipes。其中分布锁和队列有Java和C两个版本，选举只有Java版本。

**● 原理：**

ZooKeeper是以Fast Paxos算法为基础的，Paxos算法存在活锁的问题，即当有多个proposer交错提交时，有可能互相排斥导致没有一个proposer能提交成功，而Fast Paxos作了一些优化，通过选举产生一个leader(领导者)，只有leader才能提交proposer，具体算法可见Fast Paxos。因此，要想弄懂ZooKeeper首先得对Fast Paxos有所了解。

**● ZooKeeper的基本运转流程：**

选举Leader。

同步数据。

选举Leader过程中算法有很多，但要达到的选举标准是一致的。

Leader要具有最高的执行ID，类似root权限。5、集群中大多数的机器得到响应并follow选出的Leader。

##### 6、简单介绍一下solr？

Solr是一个独立的企业级搜索应用服务器，它对外提供类似于Web-service的API接口。用户可以通过http请求，向搜索引擎服务器提交一定格式的XML文件，生成索引；也可以通过Http Get操作提出查找请求，并得到XML格式的返回结果。

**● 特点：**

Solr是一个高性能，采用Java5开发，基于Lucene的全文搜索服务器。同时对其进行了扩展，提供了比Lucene更为丰富的查询语言，同时实现了可配置、可扩展并对查询性能进行了优化，并且提供了一个完善的功能管理界面，是一款非常优秀的全文搜索引擎。

**● 工作方式：**

文档通过Http利用XML加到一个搜索集合中。查询该集合也是通过http收到一个XML/JSON响应来实现。它的主要特性包括：高效、灵活的缓存功能，垂直搜索功能，高亮显示搜索结果，通过索引复制来提高可用性，提供一套强大Data Schema来定义字段，类型和设置文本分析，提供基于Web的管理界面等。

##### 7、solr怎么设置搜索结果排名靠前？

可以设置文档中域的boost值，boost值越高，计算出来的相关度得分就越高，排名也就越靠前。此方法可以把热点商品或者推广商品的排名提高。

##### 8、solr中IK分词器原理是什么？

Ik分词器的分词原理本质上是词典分词。先在内存中初始化一个词典，然后在分词过程中挨个读取字符，和字典中的字符相匹配，把文档中的所有的词语拆分出来的过程。

##### 9、什么是webService？

WebService是一种跨编程语言和跨操作系统平台的远程调用技术。所谓跨编程语言和跨操作平台，就是说服务端程序采用java编写，客户端程序则可以采用其他编程语言编写.跨操作系统平台则是指服务端程序和客户端程序可以在不同的操作系统上。

##### 10、常见的远程调用技术？

RMI是java语言本身提供的远程通讯协议，稳定高效，是EJB的基础。但它只能用于JAVA程序之间的通讯。Hessian和Burlap是caucho公司提供的开源协议，基于HTTP传输，服务端不用开防火墙端口。协议的规范公开，可以用于任意语言。跨平台有点小问题。

Httpinvoker是SpringFramework提供的远程通讯协议，只能用于JAVA程序间的通讯，且服务端和客户端必须使用SpringFramework。Web service是连接异构系统或异构语言的首选协议，它使用SOAP形式通讯，可以用于任何语言，目前的许多开发工具对其的支持也很好。

效率相比：RMI > Httpinvoker >= Hessian > Burlap > web service。

##### 11、谈谈你对restful的理解以及在项目中的使用？

 

一种软件架构风格、设计风格，而不是标准，只是提供了一组设计原则和约束条件。它主要用于客户端和服务器交互类的软件。REST指的是一组架构约束条件和原则。满足这些约束条件和原则的应用程序或设计就是RESTful。它结构清晰、符合标准、易于理解、扩展方便，所以正得到越来越多网站的采用。

# 企业Java实战面试题

##### 1、在Java中，负责对字节代码解释执行的是()？

A.应用服务器

B.虚拟机

C.垃圾回收器

D.编译器

答案选：B

##### 2、一个栈的输入序列为1 2 3 4 5，则下列序列中不可能是栈输出的序列的是()？

A.5 4 1 3 2

B.2 3 4 1 5

C.1 5 4 3 2

D.2 3 1 4 5

答案选：B

##### 3、下列那一个选项按照顺序包括了OSI模型的7个层次()？

A.物理层数据链路层传输层网络层会话层表示层应用层

B.物理层数据链路层会话层网络层传输层表示层应用层

C.物理层数据链路层网络层传输层会话层表示层应用层

D.网络层传输层物理层数据链路层会话层表示层应用层

答案选：C

##### 4、当客户度关闭一个从连接池中获取的连接，会发生下面哪一种情况()？

A.连接不会关闭，只是简单地归还给连接池

B.连接被关闭，但又被重新打开并归还给连接池

C.连接永久性关闭

答案选：A

##### 5、以下哪些不是javaScript的全局函数()？

A.eval

B.escape

C.setTimeout

D.parseFloat

答案选：C

##### 6、你使用mkdir命令创建一个临时的文件夹/tmp/aaa，并将一些文件复制其中，使用完后要删除/mnt/tmp文件夹及其中的所有文件，应该使用命令（）？

A.rm/tmp/aaa

B.rm–r/tmp/aaa

C.rmdir–r/tmp/aaa

D.rmdir/tmp/aaa

答案选：B

##### 7、在UML提供的图中，()用于按数据顺序描述对象间的交互？

A.协作图

B.网络图

C.序列图

D.状态图

答案选：C

##### 8、下面有关系统并发访问数估算数据哪个最有效：（）？

A.高峰时段日处理业务量100000

B.高峰时段平均每秒请求数80

C.同时在线用户100

D.平均每秒用户请求50

答案选：B

##### 9、不同级别的用户对同一对象拥有不同的访问权利或某个客户端不能直接操作到某个对象，但有必须和那个对象有所互动，这种情况最好使用什么设计模式()？

A.Bridge模式

B.Facade模式

C.Adapter模式

D.Proxy模式

答案选：D

##### 10、下面哪个Set是排序的？()？

A.LinkedHashSet

B.HashSet

C.TreeSet

D.AbstractSet

答案选：C

##### 11、编程题：用1，2，2，3，4，5这6个数字，用Java写一个main函数，打印出所有不同的排列，如：512234，412345等，要求：“4”不能在第三位，“3”与”5”不能相连。

```java
package com.bjpowernode.test;
import java.util.Iterator;
import java.util.TreeSet;
public class NumberRandom {
    String[] stra = {"1", "2", "2", "3", "4", "5"
    };
    int n = stra.length;
    boolean[] visited = new boolean[n];
    String result = "";
    TreeSet<String> ts = new TreeSet<String>();
    int[][] a = new int[n][n];
    private void searchMap() {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (i == j) {
                    a[i][j] = 0;
                } else {
                    a[i][j] = 1;
                }
            }
        }
        //3和5不能相连
        a[3][5] = 0;
        a[5][3] = 0;
        //开始遍历
        for (int i = 0; i < n; i++) {
            search(i);
        }
        Iterator<String> it = ts.iterator();
        while (it.hasNext()) {
            String str = it.next();
        //4不能在第三位
            if (str.indexOf("4") != 2) {
                System.out.println(str);
            }
        }
    }

    private void search(int startIndex) {
        visited[startIndex] = true;
        result = result + stra[startIndex];
        if (result.length() == n) {
            ts.add(result);
        }
        for (int j = 0; j < n; j++) {
            if (a[startIndex][j] == 1 && visited[j] == false) {
                search(j);
            } else {
                continue;
            }
        }
        //一个result结束后踢掉最后一个，寻找别的可能性，若没有的话，则继续向前踢掉当前最后一个
        result = result.substring(0，result.length() - 1);
        visited[startIndex] = false;
    }
    public static void main(String[] args) {
        new NumberRandom().searchMap();
    }
}
```

##### 12、编程题：一个数如果恰好等于它的因子之和，这个数就称为”完数”.例如6=1+2+3。编程找出1000以内的所有完数。

```java
public class wsTest{
    public static void main(String[]args){
        for(int m=2;m<1000;m++){
            int s=0;
            for(int i=1;i<m;i++){
                if((m%i)==0)
                    s+=i;
            }
            if(s==m){
                System.out.print(m+"its factors are：");
                for(int j=1;j<m;j++)
                {
                    if((m%j)==0){
                        System.out.print(j);
                        System.out.print("");
                    }
                }
                System.out.println();
            }
        }
    }
}
```

结果：

```java
6 its factors are：1 2 3
28 its factors are：1 2 4 7 14
496 its factors are：1 2 4 8 16 31 62 124 248
```

 

# Java多线程和并发面试题（附答案）1~3题

##### 1、DeplayQueue延时无界阻塞队列

在谈到DelayQueue的使用和原理的时候，我们首先介绍一下DelayQueue，DelayQueue是一个无界阻塞队列，只有在延迟期满时才能从中提取元素。该队列的头部是延迟期满后保存时间最长的Delayed元素。

DelayQueue阻塞队列在我们系统开发中也常常会用到，例如：缓存系统的设计，缓存中的对象，超过了空闲时间，需要从缓存中移出；任务调度系统，能够准确的把握任务的执行时间。我们可能需要通过线程处理很多时间上要求很严格的数据，如果使用普通的线程，我们就需要遍历所有的对象，一个一个的检查看数据是否过期等，首先这样在执行上的效率不会太高，其次就是这种设计的风格也大大的影响了数据的精度。一个需要12：00点执行的任务可能12：01才执行，这样对数据要求很高的系统有更大的弊端。由此我们可以使用DelayQueue。

下面将会对DelayQueue做一个介绍，然后举个例子。并且提供一个Delayed接口的实现和Sample代码。DelayQueue是一个BlockingQueue，其特化的参数是Delayed。（不了解BlockingQueue的同学，先去了解BlockingQueue再看本文）Delayed扩展了Comparable接口，比较的基准为延时的时间值，Delayed接口的实现类getDelay的返回值应为固定值（final）。DelayQueue内部是使用PriorityQueue实现的。

DelayQueue=BlockingQueue+PriorityQueue+Delayed

DelayQueue的关键元素BlockingQueue、PriorityQueue、Delayed。可以这么说，DelayQueue是一个使用优先队列（PriorityQueue）实现的BlockingQueue，优先队列的比较基准值是时间。

他们的基本定义如下

```java
public interface Comparable<T> {
    public int compareTo(T o);
}
public interface Delayed extends Comparable<Delayed> {
    long getDelay(TimeUnit unit);
}
public class DelayQueue<E extends Delayed> implements BlockingQueue<E> {
    private final PriorityQueue<E> q = new PriorityQueue<E>();
}
```

DelayQueue 内部的实现使用了一个优先队列。当调用 DelayQueue 的 offer 方法时，把 Delayed 对象加入到优先队列 q 中。如下：

```java
public boolean offer(E e) {
    final ReentrantLock lock = this.lock;
    lock.lock();
    try {
        E first = q.peek();
        q.offer(e);
        if (first == null || e.compareTo(first) < 0)
            available.signalAll();
        return true;
    } finally {
        lock.unlock();
    }
}
```

DelayQueue 的 take 方法，把优先队列 q 的 first 拿出来（peek），如果没有达到延时阀值，则进行 await处理。如下：

```java
public E take() throws InterruptedException {
    final ReentrantLock lock = this.lock;
    lock.lockInterruptibly();
    try {
        for (; ; ) {
            E first = q.peek();
            if (first == null) {
                available.await();
            } else {
                long delay = first.getDelay(TimeUnit.NANOSECONDS);
                if (delay > 0) {
                    long tl = available.awaitNanos(delay);
                } else {
                    E x = q.poll();
                    assert x != null;
                    if (q.size() != 0)
                        available.signalAll(); //wake up other takers return x;
                }
            }
        }
    } finally {
        lock.unlock();
    }
}
```

**● DelayQueue 实例应用**

Ps：为了具有调用行为，存放到 DelayDeque 的元素必须继承 Delayed 接口。Delayed 接口使对象成为延迟对象，它使存放在 DelayQueue 类中的对象具有了激活日期。该接口强制执行下列两个方法。

一下将使用 Delay 做一个缓存的实现。其中共包括三个类Pair、DelayItem、Cache

**● Pair 类：**

```java
public class Pair<K, V> {
    public K first;
    public V second;
    public Pair() {
    }
    public Pair(K first, V second) {
        this.first = first;
        this.second = second;
    }
}
```

以下是对 Delay 接口的实现：

```java
import java.util.concurrent.Delayed;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.atomic.AtomicLong;
public class DelayItem<T> implements Delayed {
    /**
     * Base of nanosecond timings， to avoid wrapping
     */
    private static final long NANO_ORIGIN = System.nanoTime();
    /**
     * Returns nanosecond time offset by origin
     */
    final static long now() {
        return System.nanoTime() - NANO_ORIGIN;
    }
    /**
     * Sequence number to break scheduling ties， and in turn to guarantee FIFO order among tied
     * entries.
     */
    private static final AtomicLong sequencer = new AtomicLong(0);

    /**
     * Sequence number to break ties FIFO
     */
    private final long sequenceNumber;

    /**
     * The time the task is enabled to execute in nanoTime units
     */
    private final long time;
    private final T item;
    public DelayItem(T submit， long timeout) {
        this.time = now() + timeout;
        this.item = submit;
        this.sequenceNumber = sequencer.getAndIncrement();
    }
    public T getItem() {
        return this.item;
    }
    public long getDelay(TimeUnit unit) {
        long d = unit.convert(time - now()， TimeUnit.NANOSECONDS); return d;
    }
    public int compareTo(Delayed other) {
        if (other == this) // compare zero ONLY if same object return 0;
            if (other instanceof DelayItem) {
                DelayItem x = (DelayItem) other;
                long diff = time - x.time;
                if (diff < 0) return -1;
                else if (diff > 0) return 1;
                else if (sequenceNumber < x.sequenceNumber) return -1;
                else
                    return 1;
            }
        long d = (getDelay(TimeUnit.NANOSECONDS) - other.getDelay(TimeUnit.NANOSECONDS));
        return (d == 0) ?0 :((d < 0) ?-1 :1);
    }
}
```

以下是 Cache 的实现，包括了 put 和 get 方法

```java
import javafx.util.Pair;

import java.util.concurrent.ConcurrentHashMap;
import java.util.concurrent.ConcurrentMap;
import java.util.concurrent.DelayQueue;
import java.util.concurrent.TimeUnit;
import java.util.logging.Level;
import java.util.logging.Logger;

public class Cache<K, V> {
    private static final Logger LOG = Logger.getLogger(Cache.class.getName());
    private ConcurrentMap<K, V> cacheObjMap = new ConcurrentHashMap<K, V>();
    private DelayQueue<DelayItem<Pair<K, V>>> q = new DelayQueue<DelayItem<Pair<K, V>>>();
    private Thread daemonThread;

    public Cache() {

        Runnable daemonTask = new Runnable() {
            public void run() {
                daemonCheck();
            }
        };
        daemonThread = new Thread(daemonTask);
        daemonThread.setDaemon(true);
        daemonThread.setName("Cache Daemon");
        daemonThread.start();
    }

    private void daemonCheck() {
        if (LOG.isLoggable(Level.INFO)) LOG.info("cache service started.");
        for (; ; ) {
            try {
                DelayItem<Pair<K, V>> delayItem = q.take();
                if (delayItem != null) {
                    // 超时对象处理
                    Pair<K, V> pair = delayItem.getItem();
                    cacheObjMap.remove(pair.first, pair.second); // compare and remove
                }
            } catch (InterruptedException e) {
                if (LOG.isLoggable(Level.SEVERE)) LOG.log(Level.SEVERE, e.getMessage(), e);
                break;
            }
        }
        if (LOG.isLoggable(Level.INFO)) LOG.info("cache service stopped.");
    }

    // 添加缓存对象
    public void put(K key, V value, long time, TimeUnit unit) {
        V oldValue = cacheObjMap.put(key, value);
        if (oldValue != null) q.remove(key);
        long nanoTime = TimeUnit.NANOSECONDS.convert(time, unit);
        q.put(new DelayItem<Pair<K, V>>(new Pair<K, V>(key, value), nanoTime));
    }

    public V get(K key) {
        return cacheObjMap.get(key);
    }
}
```

测试 main 方法：

```java
// 测试入口函数
public static void main(String[] args) throws Exception {
    Cache<Integer, String> cache = new Cache<Integer, String>();
    cache.put(1, "aaaa", 3, TimeUnit.SECONDS);
    Thread.sleep(1000 * 2);
    {
        String str = cache.get(1);
        System.out.println(str);
    }
    Thread.sleep(1000 * 2);
    {
        String str = cache.get(1);
        System.out.println(str);
    }
}
```

输出结果为：

aaaa

null

我们看到上面的结果，如果超过延时的时间，那么缓存中数据就会自动丢失，获得就为 null。

##### 2、并发（Collection）队列-非阻塞队列

● 非阻塞队列

首先我们要简单的理解下什么是非阻塞队列：

与阻塞队列相反，非阻塞队列的执行并不会被阻塞，无论是消费者的出队，还是生产者的入队。在底层，非阻塞队列使用的是 CAS(compare and swap)来实现线程执行的非阻塞。

● 非阻塞队列简单操作

与阻塞队列相同，非阻塞队列中的常用方法，也是出队和入队。

● offer()：Queue 接口继承下来的方法，实现队列的入队操作，不会阻碍线程的执行，插入成功返回 true； 出队方法：

● poll()：移动头结点指针，返回头结点元素，并将头结点元素出队；队列为空，则返回 null；

● peek()：移动头结点指针，返回头结点元素，并不会将头结点元素出队；队列为空，则返回 null；

##### 3、非阻塞算法CAS

首先我们需要了解悲观锁和乐观锁

悲观锁：假定并发环境是悲观的，如果发生并发冲突，就会破坏一致性，所以要通过独占锁彻底禁止冲突发生。有一个经典比喻，“如果你不锁门，那么捣蛋鬼就回闯入并搞得一团糟”，所以“你只能一次打开门放进一个人，才能时刻盯紧他”。

乐观锁：假定并发环境是乐观的，即虽然会有并发冲突，但冲突可发现且不会造成损害，所以，可以不加任何保护，等发现并发冲突后再决定放弃操作还是重试。可类比的比喻为，“如果你不锁门，那么虽然捣蛋鬼会闯入，但他们一旦打算破坏你就能知道”，所以“你大可以放进所有人，等发现他们想破坏的时候再做决定”。通常认为乐观锁的性能比悲观所更高，特别是在某些复杂的场景。这主要由于悲观锁在加锁的同时，也会把某些不会造成破坏的操作保护起来；而乐观锁的竞争则只发生在最小的并发冲突处，如果用悲观锁来理解，就是“锁的粒度最小”。但乐观锁的设计往往比较复杂，因此，复杂场景下还是多用悲观锁。首先保证正确性，有必要的话，再去追求性能。

乐观锁的实现往往需要硬件的支持，多数处理器都都实现了一个CAS指令，实现“Compare And Swap”的语义（这里的swap是“换入”，也就是set），构成了基本的乐观锁。CAS包含3个操作数：

需要读写的内存位置V

进行比较的值A

拟写入的新值B

当且仅当位置V的值等于A时，CAS才会通过原子方式用新值B来更新位置V的值；否则不会执行任何操作。无论位置V的值是否等于A，都将返回V原有的值。一个有意思的事实是，“使用CAS控制并发”与“使用乐观锁”并不等价。CAS只是一种手段，既可以实现乐观锁，也可以实现悲观锁。乐观、悲观只是一种并发控制的策略。

# Java多线程和并发面试题（附答案）第4题

##### 4、ConcurrentLinkedQueue非阻塞无界链表队列

ConcurrentLinkedQueue是一个线程安全的队列，基于链表结构实现，是一个无界队列，理论上来说队列的长度可以无限扩大。与其他队列相同，ConcurrentLinkedQueue也采用的是先进先出（FIFO）入队规则，对元素进行排序。当我们向队列中添加元素时，新插入的元素会插入到队列的尾部；而当我们获取一个元素时，它会从队列的头部中取出。因为ConcurrentLinkedQueue是链表结构，所以当入队时，插入的元素依次向后延伸，形成链表；而出队时，则从链表的第一个元素开始获取，依次递增；

值得注意的是，在使用ConcurrentLinkedQueue时，如果涉及到队列是否为空的判断，切记不可使用size()==0的做法，因为在size()方法中，是通过遍历整个链表来实现的，在队列元素很多的时候，size()方法十分消耗性能和时间，只是单纯的判断队列为空使用isEmpty()即可。

```java
public class ConcurrentLinkedQueueTest {
    public static int threadCount = 10;
    public static ConcurrentLinkedQueue<String> queue = new ConcurrentLinkedQueue<String>();
    static class Offer implements Runnable {
        public void run() {
            //不建议使用 queue.size()==0，影响效率。可以使用!queue.isEmpty()
            if (queue.size() == 0) {
                String ele = new Random().nextInt(Integer.MAX_VALUE) + "";
                queue.offer(ele);
                System.out.println("入队元素为" + ele);
            }
        }
    }
    static class Poll implements Runnable {
        public void run() {
            if (!queue.isEmpty()) {
                String ele = queue.poll();
                System.out.println("出队元素为" + ele);
            }
        }
    }
    public static void main(String[] agrs) {
        ExecutorService executorService = Executors.newFixedThreadPool(4);
        for (int x = 0; x < threadCount; x++) {
            executorService.submit(new Offer());
            executorService.submit(new Poll());
        }
        executorService.shutdown();
    }
}
```

一种输出：

入队元素为313732926

出队元素为313732926

入队元素为812655435

出队元素为812655435

入队元素为1893079357

出队元素为1893079357

入队元素为1137820958

出队元素为1137820958

入队元素为1965962048

出队元素为1965962048

出队元素为685567162

入队元素为685567162

出队元素为1441081163

入队元素为1441081163

出队元素为1627184732

入队元素为1627184732

ConcurrentLinkedQuere类图

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558319848@fc5c0cca9c25baf601fca2a4f2c8ee95.png)

如图ConcurrentLinkedQueue中有两个volatile类型的Node节点分别用来存在列表的首尾节点，其中head节点存放链表第一个item为null的节点，tail则并不是总指向最后一个节点。Node节点内部则维护一个变量item用来存放节点的值，next用来存放下一个节点，从而链接为一个单向无界列表。

```java
public ConcurrentLinkedQueue(){
    head=tail=new Node<E>(null);
}
```

如上代码初始化时候会构建一个 item 为 NULL 的空节点作为链表的首尾节点。

Offer 操作offer 操作是在链表末尾添加一个元素，下面看看实现原理。

```java
public boolean offer(E e) {
    //e 为 null 则抛出空指针异常
    checkNotNull(e);
    //构造 Node 节点构造函数内部调用 unsafe.putObject，后面统一讲
    final Node<E> newNode = new Node<E>(e);
    //从尾节点插入
    for (Node<E> t = tail, p = t; ; ) {
        Node<E> q = p.next;
        //如果 q=null 说明 p 是尾节点则插入
        if (q == null) {
            //cas 插入（1）
            if (p.casNext(null, newNode)) {
                //cas 成功说明新增节点已经被放入链表，然后设置当前尾节点（包含 head，1，3，5.。。个节点为尾节点）
                if (p != t)// hop two nodes at a time
                    casTail(t, newNode); // Failure is OK. return true;
            }
            // Lost CAS race to another thread; re-read next
        } else if (p == q)//(2)
            //多线程操作时候，由于 poll 时候会把老的 head 变为自引用，然后 head 的 next 变为新 head，所以这里需要
            //重新找新的 head，因为新的 head 后面的节点才是激活的节点
            p = (t != (t = tail)) ? t : head;
        else
            // 寻找尾节点(3)
            p = (p != t && t != (t = tail)) ? t : q;
    }
}
```

从构造函数知道一开始有个item为null的哨兵节点，并且head和tail都是指向这个节点。

如图首先查找尾节点，q==null，p就是尾节点，所以执行p.casNext通过cas设置p的next为新增节点，这时候p==t所以不重新设置尾节点为当前新节点。由于多线程可以调用offer方法，所以可能两个线程同时执行到了（1）进行cas，那么只有一个会成功（假如线程1成功了），成功后的链表为：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558320190@f13d5648d098f76a280b5d6db6687ae8.png)

失败的线程会循环一次这时候指针为：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558320224@9fc759dbded616184ca624a813042212.png)

这时候会执行（3）所以 p=q，然后在循环后指针位置为：

 

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558320258@427e57fcd638fa4e2a3f0127b749ea3f.png)

所以没有其他线程干扰的情况下会执行（1）执行 cas 把新增节点插入到尾部，没有干扰的情况下线程 2 cas 会成功，然后去更新尾节点 tail，由于 p!=t 所以更新。这时候链表和指针为：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558320292@afec41d41b7c505885677c2fe55b24f4.png)

假如线程 2cas 时候线程 3 也在执行，那么线程 3 会失败，循环一次后，线程 3 的节点状态为：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558320321@8292d8ed7898718f5743f59e917d73e0.png)

这时候 p!=t ；并且 t 的原始值为 told，t 的新值为 tnew ，所以 told!=tnew，所以 p=tnew=tail

然后在循环一下后节点状态：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558320358@b8f2581677882a5cd813fc848e96824c.png)

q==null 所以执行（1）。

现在就差 p==q 这个分支还没有走，这个要在执行 poll 操作后才会出现这个情况。poll 后会存在下面的状态

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558320392@e2b2dcbc8ea536561eeae066a74acb5a.png)

这个时候添加元素时候指针分布为：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558320420@db8a4e68235164a689d9da41575b0303.png)

所以会执行（2）分支 结果 p=head，然后循环，循环后指针分布：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558320447@3a8fdd5ce361bc39cc9b78cc3f764d34.png)

所以执行(1)，然后 p!=t 所以设置 tail 节点。现在分布图：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558320473@a5a6e0ad7c6f84cb73881caf3b4bb7ba.png)

自引用的节点会被垃圾回收掉。

**● add 操作**

add操作是在链表末尾添加一个元素，下面看看实现原理。

其实内部调用的还是 offer

```java
public boolean add(E e) {
    return offer(e);
}
```

**● poll 操作**

poll 操作是在链表头部获取并且移除一个元素，下面看看实现原理。

```java
public E poll() {
    restartFromHead:
    // 死 循 环
    for (; ; ) {
        //死循环
        for (Node<E> h = head, p = h, q;
                ; ) {
            //保存当前节点值
            E item = p.item;
            //当前节点有值则 cas 变为 null（1）
            if (item != null && p.casItem(item， null)){
                //cas 成功标志当前节点以及从链表中移除
                if (p != h) // 类似 tail 间隔 2 设置一次头节点（2）
                    updateHead(h, ((q = p.next) != null) ? q : p);
                return item;
            }
            //当前队列为空则返回 null（3）
        else if ((q = p.next) == null) {
                updateHead(h, p);
                return null;
            }
            //自引用了，则重新找新的队列头节点（4）
            else if (p == q)
                continue restartFromHead;
            else//(5)
                p = q;
        }
    }
}
final void updateHead(Node<E> h,Node<E> p){
    if(h!=p&&casHead(h,p))
        h.lazySetNext(h);
}
```

**● 当队列为空时候：**

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558320564@74040df9149c5a6aba7cf03bbdab871d.png)

可知执行（3）这时候有两种情况，第一没有其他线程添加元素时候(3)结果为 true 然后因为 h!=p 为 false 所以直接返回 null。第二在执行 q=p.next 前，其他线程已经添加了一个元素到队列，这时候（3）返回 false，然后执行（5）p=q，然后循环后节点分布：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558320592@9d36515db39504e0643dc827fb4b4522.png)

这时候执行（1）分支，进行 cas 把当前节点值值为 null，同时只有一个线程会成功，cas 成功 标示该节点从队列中移除了，然后 p!=h，调用 updateHead 方法，参数为 h，p;h!=p 所以把 p 变为当前链表 head 节点，然后 h 节点的 next 指向自己。现在状态为：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558320612@b0f7a86c41ed9d72e9ff03fa959580a7.png)

cas 失败 后 会再次循环，这时候分布图为：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558320637@df0953085efbbe9564acf4b77b3589e0.png)

这时候执行（3）返回 null.

现在还有个分支（4）没有执行过，那么什么时候会执行那？

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558320665@fc5cac5e3db5d1ae6e76d379f6dd4cb7.png)

这时候执行（1）分支，进行 cas 把当前节点值值为 null，同时只有一个线程 A 会成功，cas 成功  标示该节点从队列中移除了，然后 p!=h，调用 updateHead 方法，假如执行 updateHead 前另外一个线程 B 开始 poll 这时候它 p 指向为原来的 head 节点，然后当前线程 A 执行 updateHead 这时候 B 线程链表状态为：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558320687@c183c203f37d437b023d9215167a4387.png)

所以会执行（4）重新跳到外层循环，获取当前 head，现在状态为：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558320711@b8845aa31eea675e353801d3efd29cf3.png)

**● peek 操作**

peek 操作是获取链表头部一个元素（只读取不移除），下面看看实现原理。

代码与 poll 类似，只是少了 castItem.并且 peek 操作会改变 head 指向，offer 后 head 指向哨兵节点，第一次 peek 后 head 会指向第一个真的节点元素。

```java
public E peek() {
    restartFromHead:
    for (; ; ) {
        for (Node<E> h = head, p = h, q; ; ) {
            E item = p.item;
            if (item != null || (q = p.next) == null) {
                updateHead(h, p);
                return item;
            } else if (p == q)
                continue restartFromHead;
            else
                p = q;
        }
    }
}
```

**● size 操作**

获取当前队列元素个数，在并发环境下不是很有用，因为使用 CAS 没有加锁所以从调用 size 函数到返回结果期间有可能增删元素，导致统计的元素个数不精确。

```java
public int size() {
    int count = 0;
    for (Node<E> p = first(); p != null; p = succ(p))
        if (p.item != null)
            // 最大返回 Integer.MAX_VALUE
            if (++count == Integer.MAX_VALUE) break;
    return count;
}

//获取第一个队列元素（哨兵元素不算），没有则为 null
Node<E> first() {
    restartFromHead:
    for (; ; ) {
        for (Node<E> h = head, p = h, q; ; ) {
            boolean hasItem = (p.item != null);
            if (hasItem || (q = p.next) == null) {
                updateHead(h, p);
                return hasItem ? p : null;
            } else if (p == q)
                continue restartFromHead;
            else
                p = q;
        }
    }
}
//获取当前节点的 next 元素，如果是自引入节点则返回真正头节点
final Node<E> succ(Node<E> p) {
    Node<E> next = p.next;
    return (p == next) ? head : next;
}
```

 **● remove 操作**

如果队列里面存在该元素则删除该元素，如果存在多个则删除第一个，并返回 true，否则返回 false

```java
ublic boolean remove(Object o){
    //查找元素为空，直接返回 false
    if(o==null)return false;
    Node<E> pred=null;
    for(Node<E> p=first();p!=null;p=succ(p)){
        E item=p.item;
        //相等则使用 cas 值 null，同时一个线程成功，失败的线程循环查找队列中其他元素是否有匹配的。
        if(item!=null&&o.equals(item)&&p.casItem(item,null)){
            //获取 next 元素
            Node<E> next=succ(p);
            //如果有前驱节点，并且 next 不为空则链接前驱节点到 next，
            if(pred!=null&&next!=null)
                pred.casNext(p,next);
            return true;
        }
        pred=p;
    }
    return false;
}
```

**● contains 操作**

判断队列里面是否含有指定对象，由于是遍历整个队列，所以类似 size 不是那么精确，有可能调用该方法时候元素还在队列里面，但是遍历过程中才把该元素删除了，那么就会返回 false.

```java
public boolean contains(Object o) {
    if (o == null) return false;
    for (Node<E> p = first(); p != null; p = succ(p)) {
        E item = p.item;
        if (item != null && o.equals(item)) return true;
    }
    return false;
}
```

关于ConcurrentLinkedQuere的offer方法有意思的问题：

offer 中有个 判断 t != (t = tail）假如 t=node1;tail=node2;并且 node1!=node2 那么这个判断是 true 还是 false 那，答案是 true，这个判断是看当前 t 是不是和 tail 相等，相等则返回 true 否者为 false，但是无论结果是啥执行后 t 的值都是 tail。

下面从字节码来分析下为啥。

举一个例子：

```java
public static void main(String[] args){
        int t=2;
        int tail=3;
        System.out.println(t!=(t=tail));
}
```

结果为：true

字节码文件：

C：\Users\Simple\Desktop\TeacherCode\Crm_Test\build\classes\com\bjpowernode\crm\util>javap-c Test001

警告：二进制文件Test001包含com.bjpowernode.crm.util.Test001 Compiled from"Test001.java"

```java
public class com.bjpowernode.crm.util.Test001{
    public class com.bjpowernode.crm.util.Test001();
        Code:
        0:aload_0
        1:invokespecial #8    // Method java/lang/Object."<init>"：()V
        4:return

    public static void main(java.lang.String[]){
        Code:
        0:iconst_2
        1:istore_1
        2:iconst_3
        3:istore_2
        4:getstatic    #16    // Field java/lang/System.out：Ljava/io/PrintStream;
        7:iload_1
        8:iload_2
        9:dup
        10:istore_1
        11:if_icmpeq 18
        14:iconst_1
        15:goto 19
        18:iconst_0
        19:invokevirtual #22    // Method java/io/PrintStream.println：(Z)V
        22:return
}
```

我们从上面main方法的字节码文件中分析

一开始栈为空：

栈

第0行指令作用是把值2入栈栈顶元素为2

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558321806@982b49ab0ec30878a426b0e7810b0d13.png)

栈

第1行指令作用是将栈顶int类型值保存到局部变量t中

栈

第2行指令作用是把值3入栈栈顶元素为3

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558321966@87f8e1860b8ca0ae2f5c4e80a509573e.png)

栈

第3行指令作用是将栈顶int类型值保存到局部变量tail中。

栈

第4调用打印命令

第7行指令作用是把变量t中的值入栈

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558322024@748fd01a1f45b3fa4eb059835cce6813.png)

栈

第8行指令作用是把变量tail中的值入栈

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558322051@b3fd07fece5b215d35a82ebf8ac042d3.png)

栈

现在栈里面的元素为3、2，并且3位于栈顶

第9行指令作用是当前栈顶元素入栈，所以现在栈内容3，3，2

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558322079@26cda51d94e39838fc2699dd8c0a06c3.png)

栈

第10行指令作用是把栈顶元素存放到t，现在栈内容3，2

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558322106@b10acb6922e616cd28366718af69389a.png)

栈

第11行指令作用是判断栈顶两个元素值，相等则跳转18。由于现在栈顶严肃为3，2不相等所以返回true.

第14行指令作用是把1入栈

然后回头分析下!=是双目运算符，应该是首先把左边的操作数入栈，然后在去计算了右侧操作数。

**● ConcurrentLinkedQuere总结**

ConcurrentLinkedQueue使用CAS非阻塞算法实现使用CAS解决了当前节点与next节点之间的安全链接和对当前节点值的赋值。由于使用CAS没有使用锁，所以获取size的时候有可能进行offer，poll或者remove操作，导致获取的元素个数不精确，所以在并发情况下size函数不是很有用。另外第一次peek或者first时候会把head指向第一个真正的队列元素。

下面总结下如何实现线程安全的，可知入队出队函数都是操作volatile变量：head，tail。所以要保证队列线程安全只需要保证对这两个Node操作的可见性和原子性，由于volatile本身保证可见性，所以只需要看下多线程下如果保证对着两个变量操作的原子性。

对于offer操作是在tail后面添加元素，也就是调用tail.casNext方法，而这个方法是使用的CAS操作，只有一个线程会成功，然后失败的线程会循环一下，重新获取tail，然后执行casNext方法。对于poll也是这样的。

# Java多线程和并发面试题（附答案）第5题

##### 5、ConcurrentHashMap非阻塞Hash集合?

ConcurrentHashMap是Java并发包中提供的一个线程安全且高效的HashMap实现，ConcurrentHashMap在并发编程的场景中使用频率非常之高，本文就来分析下ConcurrentHashMap的实现原理，并对其实现原理进行分析。

● ConcurrentLinkedQuere 类图

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558321385@9aa230e981e5a1ed55eac651aa1a622c.png)

ConcurrentHashMap是由Segment数组结构和HashEntry数组结构组成。Segment是一种可重入锁。

ReentrantLock在ConcurrentHashMap里扮演锁的角色，HashEntry则用于存储键值对数据。一个ConcurrentHashMap里包含一个Segment数组，Segment的结构和HashMap类似，是一种数组和链表结构，一个Segment里包含一个HashEntry数组，每个HashEntry是一个链表结构的元素，每个Segment守护者一个HashEntry数组里的元素，当对HashEntry数组的数据进行修改时，必须首先获得它对应的Segment锁。

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558321412@1e083e42388713d399e2fccdad96f312.png)

● ConcurrentLinkedQuere实现原理

众所周知，哈希表是非常高效的，复杂度为O(1)的数据结构，在Java开发中，我们最常见到最频繁使用的就是HashMap和HashTable，但是在线程竞争激烈的并发场景中使用都不够合理。

● HashMap：先说HashMap，HashMap是线程不安全的，在并发环境下，可能会形成环状链表（扩容时可能造成，具体原因自行百度google或查看源码分析），导致get操作时，cpu 空转，所以，在并发环境中使用HashMap是非常危险的。

● HashTable：HashTable和HashMap的实现原理几乎一样，差别无非是：

1、HashTable不允许key和value为null；

2、HashTable是线程安全的。但是HashTable线程安全的策略实现代价却太大了，简单粗暴，get/put所有相关操作都是synchronized的，这相当于给整个哈希表加了一把大锁，多线程访问时候，只要有一个线程访问或操作该对象，那其他线程只能阻塞，相当于将所有的操作串行化，在竞争激烈的并发场景中性能就会非常差。如下图：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558321450@97d6b48800fd03cea35829258796122d.png)

HashTable性能差主要是由于所有操作需要竞争同一把锁，而如果容器中有多把锁，每一把锁锁一段数据，这样在多线程访问时不同段的数据时，就不会存在锁竞争了，这样便可以有效地提高并发效率。这就是ConcurrentHashMap所采用的“分段锁”思想。

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558321473@2094cc6ee04ea8611e21ddb1c12615e7.png)

● ConcurrentLinkedQuere源码解析

ConcurrentHashMap采用了非常精妙的"分段锁"策略，ConcurrentHashMap的主干是个Segment数组。

```java
final Segment<K,V>[] segments;
```

 Segment继承了ReentrantLock，所以它就是一种可重入锁（ReentrantLock)。在ConcurrentHashMap中，一个Segment就是一个子哈希表，Segment里维护了一个HashEntry数组，并发环境下，对于不同Segment的数据进行的操作是不用考虑锁竞争的。（就按默认的ConcurrentLeve为16来讲，理论上就允许16个线程并发执行）所以，对于同一个Segment的操作才需考虑线程同步，不同的Segment则无需考虑。Segment类似于HashMap，一个Segment维护着一个HashEntry数组。

```java
transient volatile HashEntry<K，V>[] table;  
```

HashEntry是目前我们提到的最小的逻辑处理单元了。一个ConcurrentHashMap维护一个Segment数组，一个Segment维护一个HashEntry数组。

```java
static final class HashEntry<K, V> {
    final int hash;
    final K key;
    volatile V value;
    volatile HashEntry<K, V> next;
    //其他省略
}
```

我们说Segment类似哈希表，那么一些属性就跟我们之前提到的HashMap差不多，比如负载因子loadFactor，比如阈值threshold等等，看下Segment的构造方法。

```java
Segment(float lf, int threshold, HashEntry<K, V>[] tab) {
    this.loadFactor = lf;//负载因子
    this.threshold = threshold;//阈值
    this.table = tab;//主干数组即 HashEntry 数组
}
```

我们来看下ConcurrentHashMap的构造方法

```java
public ConcurrentHashMap(int initialCapacity, float loadFactor, int concurrencyLevel) {
    if (!(loadFactor > 0) || initialCapacity < 0 || concurrencyLevel <= 0) throw new IllegalArgumentException();
    //MAX_SEGMENTS 为 1<<16=65536，也就是最大并发数为 65536
    if (concurrencyLevel > MAX_SEGMENTS) concurrencyLevel = MAX_SEGMENTS;
    //2 的 sshif 次方等于 ssize，例:ssize=16，sshift=4;ssize=32，sshif=5
    int sshift = 0;
    //ssize 为 segments 数组长度，根据 concurrentLevel 计算得出
    int ssize = 1;
    while (ssize < concurrencyLevel) {
        ++sshift;
        ssize <<= 1;
    }
    //segmentShift 和 segmentMask 这两个变量在定位 segment 时会用到，后面会详细讲
    this.segmentShift = 32 - sshift;
    this.segmentMask = ssize - 1;
    if (initialCapacity > MAXIMUM_CAPACITY) initialCapacity = MAXIMUM_CAPACITY;
    //计算 cap 的大小，即 Segment 中 HashEntry 的数组长度，cap 也一定为 2 的 n 次方.
    int c = initialCapacity / ssize;
    if (c * ssize < initialCapacity)
        ++c;
    int cap = MIN_SEGMENT_TABLE_CAPACITY;
    while (cap < c) cap <<= 1;
    //创建 segments 数组并初始化第一个 Segment，其余的 Segment 延迟初始化
    Segment<K, V> s0 = new Segment<K, V>(loadFactor, (int) (cap * loadFactor), (HashEntry<K, V>[]) new HashEntry[cap]);
    Segment<K, V>[] ss = (Segment<K, V>[]) new Segment[ssize];
    UNSAFE.putOrderedObject(ss, SBASE, s0);
    this.segments = ss;
}
```

初始化方法有三个参数，如果用户不指定则会使用默认值，initialCapacity为16，loadFactor为0.75（负载因子，扩容时需要参考），concurrentLevel为16。

从上面的代码可以看出来，Segment数组的大小ssize是由concurrentLevel来决定的，但是却不一定等于concurrentLevel，ssize一定是大于或等于concurrentLevel的最小的2的次幂。比如：默认情况下concurrentLevel是16，则ssize为16；若concurrentLevel为14，ssize为16；若concurrentLevel为17，则ssize为32。为什么Segment的数组大小一定是2的次幂？其实主要是便于通过按位与的散列算法来定位Segment的index。其实，put方法对segment也会有所体现。

```java
public V put(K key, V value) {
    Segment<K, V> s;
    //concurrentHashMap 不允许 key/value 为空
    if (value == null)
        throw new NullPointerException();
    //hash 函数对 key 的 hashCode 重新散列，避免差劲的不合理的 hashcode，保证散列均匀
    int hash = hash(key);
    //返回的 hash 值无符号右移 segmentShift 位与段掩码进行位运算，定位 segment
    int j = (hash >>> segmentShift) & segmentMask;
    if ((s = (Segment<K, V>) UNSAFE.getObject    // nonvolatile; recheck
            (segments, (j << SSHIFT) + SBASE)) == null) // in ensureSegment
        s = ensureSegment(j);
    return s.put(key, hash, value, false);
}
```

**● 从源码看出，put的主要逻辑也就两步：**

定位segment并确保定位的Segment已初始化。

调用Segment的put方法。

Ps：关于segmentShift和segmentMask

segmentShift和segmentMask这两个全局变量的主要作用是用来定位Segment。

```java
int j = (hash >>> segmentShift) & segmentMask;
```

segmentMask：段掩码，假如segments数组长度为16，则段掩码为16-1=15；segments长度为32，段掩码为32-1=31。这样得到的所有bit位都为1，可以更好地保证散列的均匀性。

segmentShift：2的sshift次方等于ssize，segmentShift=32-sshift。若segments长度为16，segmentShift=32-4=28;若segments长度为32，segmentShift=32-5=27。而计算得出的hash值最大为32位，无符号右移segmentShift，则意味着只保留高几位（其余位是没用的），然后与段掩码segmentMask位运算来定位Segment。

ConcurrentLinkedQuere 方法。

**● Get 操作：**

```java
public V get(Object key) {
    Segment<K, V> s;
    HashEntry<K, V>[] tab;
    int h = hash(key);
    long u = (((h >>> segmentShift) & segmentMask) << SSHIFT) + SBASE;
    //先定位 Segment,再定位 HashEntry
    if ((s = (Segment<K, V>) UNSAFE.getObjectVolatile(segments, u)) != null && (tab = s.table) != null) {
        for (HashEntry<K, V> e = (HashEntry<K, V>) UNSAFE.getObjectVolatile(tab, ((long) (((tab.length - 1) & h)) << TSHIFT) + TBASE);
             e != null; e = e.next) {
            K k;
            if ((k = e.key) == key || (e.hash == h && key.equals(k))) return e.value;
        }
    }
    return null;
}
```

get方法无需加锁，由于其中涉及到的共享变量都使用volatile修饰，volatile可以保证内存可见性，所以不会读取到过期数据。

来看下concurrentHashMap代理到Segment上的put方法，Segment中的put方法是要加锁的。只不过是锁粒度细了而已。

**● Put 操作：**

```java
final V put(K key, int hash, V value, boolean onlyIfAbsent) {
    HashEntry<K, V> node = tryLock() ? null :
            scanAndLockForPut(key, hash, value);
    //tryLock 不成功时会遍历定位到的 HashEnry 位置的链表（遍历主要是为了使 CPU 缓存链表）,
    // 若找不到,则创建 HashEntry。tryLock 一定次数后（MAX_SCAN_RETRIES 变量决定）,则lock。
    // 若遍历过程中,由于其他线程的操作导致链表头结点变化,则需要重新遍历。
    V oldValue;
    try {
        HashEntry<K, V>[] tab = table;
        int index = (tab.length - 1) & hash;//定位 HashEntry,可以看到,这个 hash 值在定位 Segment
        //时和在 Segment 中定位 HashEntry 都会用到,只不过定位 Segment 时只用到高几位。
        HashEntry<K, V> first = entryAt(tab, index);
        for (HashEntry<K, V> e = first; ; ) {
            if (e != null) {
                K k;
                if ((k = e.key) == key ||
                        (e.hash == hash && key.equals(k))) {
                    oldValue = e.value;
                    if (!onlyIfAbsent) {
                        e.value = value;
                        ++modCount;
                    }
                    break;
                }
                e = e.next;
            } else {
                if (node != null) node.setNext(first);
                else
                    node = new HashEntry<K, V>(hash, key, value, first);
                int c = count + 1;
                //若 c 超出阈值 threshold,需要扩容并 rehash。扩容后的容量是当前容量的 2 倍。这样可以最大程
                //度避免之前散列好的 entry 重新散列,具体在另一篇文章中有详细分析,不赘述。扩容并 rehash 的这个过程是比较消耗资源的。
                if (c > threshold && tab.length < MAXIMUM_CAPACITY) rehash(node);
                else
                    setEntryAt(tab, index, node);
                ++modCount;
                count = c;
                oldValue = null;
                break;
            }
        }
    } finally {
        unlock();
    }
    return oldValue;
}
```

ConcurrentLinkedQuere 总结

ConcurrentHashMap作为一种线程安全且高效的哈希表的解决方案，尤其是其中的“分段锁”的方案，相比HashTable的全表锁在性能上的提升非常之大。本文对ConcurrentHashMap的实现原理进行了详细分析，并解读了部分源码，希望能帮助到有需要的童鞋。

# Java多线程和并发面试题（附答案）第6题

##### 6、ConcurrentSkipListMap非阻塞Hash跳表集合？

大家都是知道TreeMap，它是使用树形结构的方式进行存储数据的线程不安全的Map集合（有序的哈希表），并且可以对Map中的Key进行排序，Key中存储的数据需要实现Comparator接口或使用CompareAble接口的子类来实现排序。

ConcurrentSkipListMap也是和TreeMap，它们都是有序的哈希表。但是，它们是有区别的：

● 第一，它们的线程安全机制不同，TreeMap是非线程安全的，而ConcurrentSkipListMap是线程安全的。

● 第二，ConcurrentSkipListMap是通过跳表实现的，而TreeMap是通过红黑树实现的。

● 那现在我们需要知道什么是跳表？

Skip list(跳表）是一种可以代替平衡树的数据结构，默认是按照Key值升序的。Skip list让已排序的数据分布在多层链表中，以0-1随机数决定一个数据的向上攀升与否，通过“空间来换取时间”的一个算法，在每个节点中增加了向前的指针，在插入、删除、查找时可以忽略一些不可能涉及到的结点，从而提高了效率。

从概率上保持数据结构的平衡比显式的保持数据结构平衡要简单的多。对于大多数应用，用Skip list要比用树算法相对简单。由于Skip list比较简单，实现起来会比较容易，虽然和平衡树有着相同的时间复杂度O(log(n))，但是Skip list的常数项会相对小很多。Skip list在空间上也比较节省。一个节点平均只需要1.333个指针（甚至更少）。下图为Skip list结构图

（以7，14，21，32，37，71，85序列为例）。

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558322199@bb2fb299f25b3a94c5ddc8b2a931fce1.png)

● SkipList性质

● 由很多层结构组成，level是通过一定的概率随机产生的。

● 每一层都是一个有序的链表，默认是升序，也可以根据创建映射时所提供的Comparator进行排序，具体取决于使用的构造方法。

● 最底层（Level 1）的链表包含所有元素。

● 如果一个元素出现在Level i的链表中，则它在Level i之下的链表也都会出现。

● 每个节点包含两个指针，一个指向同一链表中的下一个元素，一个指向下面一层的元素。

● 什么是ConcurrentSkipListMap？

ConcurrentSkipListMap提供了一种线程安全的并发访问的排序映射表。内部是SkipList（跳表）结构实现，在理论上能够在O(log(n))时间内完成查找、插入、删除操作。

注意：调用ConcurrentSkipListMap的size时，由于多个线程可以同时对映射表进行操作，所以映射表需要遍历整个链表才能返回元素个数，这个操作是个O(log(n))的操作。

● ConcurrentSkipListMap 的数据结构，如下图所示：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558322224@ed3f3439c328ed296c9a74a27af5cbf1.png)

说明：可以看到ConcurrentSkipListMap的数据结构使用的是跳表，每一个HeadIndex、Index结点都会包含一个对Node的引用，同一垂直方向上的Index、HeadIndex结点都包含了最底层的Node结点的引用。并且层级越高，该层级的结点（HeadIndex和Index）数越少。Node结点之间使用单链表结构。

● ConcurrentSkipListMap源码分析

ConcurrentSkipListMap主要用到了Node和Index两种节点的存储方式，通过volatile关键字实现了并发的操作

```java
static final class Node<K, V> {
    final K key;
    volatile Object value;//value 值
    volatile Node<K, V> next;//next 引用
……
}

static class Index<K, V> {
    final Node<K, V> node;
    final Index<K, V> down;//downy 引用
    volatile Index<K, V> right;//右边引用
……
}
```

● ConcurrentSkipListMap查找操作

通过SkipList的方式进行查找操作：（下图以“查找91”进行说明：）

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558322307@2fc63efdd9e1165a0f01fec72c141a95.png)

红色虚线，表示查找的路径，蓝色向右箭头表示right引用；黑色向下箭头表示down引用；下面就是源码中的实现（get方法是通过doGet方法来时实现的）。

```java
private V doGet(Object okey) {
    Comparable<? super K> key = comparable(okey);
    for (; ; ) {
        // 找到“key对应的节点”
        Node<K, V> n = findNode(key);
        if (n == null)
            return null;
        Object v = n.value;
        if (v != null)
            return (V) v;
    }
}
```

● ConcurrentSkipListMap删除操作

通过SkipList的方式进行删除操作：（下图以“删除23”进行说明：）。

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558322360@d3580028d0fdfb6fe0e2113cf839d40b.png)

红色虚线，表示查找的路径，蓝色向右箭头表示right引用；黑色向下箭头表示down引用；下面就是源码中的实现（remove方法是通过doRemove方法来时实现的）。

```java
//remove 操作,通过 doRemove 实现,把所有 level 中出现关键字 key 的地方都 delete 掉
public V remove(Object key) {
    return doRemove(key, null);
}

final V doRemove(Object okey, Object value) {
    Comparable<? super K> key = comparable(okey);
    for (; ; ) {
        Node<K, V> b = findPredecessor(key);//得到 key 的前驱（就是比 key 小的最大节点）
        Node<K, V> n = b.next;//前驱节点的 next 引用
        for (; ; ) {//遍历
            if (n == null)//如果 next 引用为空,直接返回
                return null;
            Node<K, V> f = n.next;
            if (n != b.next)    // 如果两次获得的 b.next 不是相同的 Node,就跳转到第一层循环重新获得 b 和 n
                break;
            Object v = n.value;
            if (v == null) {    // 当 n 被其他线程 delete 的时候,其 value==null, 此时做辅助处理,并重新获取 b 和 n
                n.helpDelete(b, f);
                break;
            }
            if (v == n || b.value == null)    // 当其前驱被 delet 的时候直接跳出,重新获取 b 和 n break;
                int c = key.compareTo(n.key);
            if (c < 0)
                return null;
            if (c > 0) {//当 key 较大时就继续遍历
                b = n;
                n = f;
                continue;
            }
            if (value != null && !value.equals(v)) return null;
            if (!n.casValue(v, null)) break;
            if (!n.appendMarker(f) || !b.casNext(n, f))//casNext 方法就是通过比较和设置 b（前驱）的 next 节点的方式来实现删除操作
                findNode(key);    // 通过尝试 findNode 的方式继续 find
            else {
                findPredecessor(key);    // Clean index
                if (head.right == null)    //如果 head 的 right 引用为空,则表示不存在该 level
                    tryReduceLevel();
            }
            return (V) v;
        }
    }
}
```

● ConcurrentSkipListMap插入操作

通过SkipList的方式进行插入操作：（下图以“添加55”的两种情况，进行说明：）。

 

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558322417@474228bc5bf70fb3ceef9fdd1d377c67.png)

在level=2（该level存在）的情况下添加55的图示:只需在level<=2的合适位置插入55即可在level=4(该level不存在，图示level4是新建的)的情况下添加55的情况：首先新建level4，然后在level<=4的合适位置插入55。

下面就是源码中的实现（put方法是通过doPut方法来时实现的）。

```java
/put 操作,通过 doPut 实现
    public V put(K key, V value) {
        if (value == null)
            throw new NullPointerException();
        return doPut(key, value, false);
    }

    private V doPut(K kkey, V value, boolean onlyIfAbsent) {
        Comparable<? super K> key = comparable(kkey);
        for (; ; ) {
            Node<K, V> b = findPredecessor(key);//前驱
            Node<K, V> n = b.next;
            //定位的过程就是和 get 操作相似
            for (; ; ) {
                if (n != null) {
                    Node<K, V> f = n.next;
                    if (n != b.next) // 前后值不一致的情况下,跳转到第一层循环重新获得 b 和 n
                        break;
                    Object v = n.value;
                    if (v == null) {    // n 被 delete 的情况下
                        n.helpDelete(b, f);
                        break;
                    }
                    if (v == n || b.value == null) // b 被 delete 的情况,重新获取 b 和 n
                        break;
                    int c = key.compareTo(n.key);
                    if (c > 0) {
                        b = n;
                        n = f;
                        continue;
                    }
                    if (c == 0) {
                        if (onlyIfAbsent || n.casValue(v, value)) return (V) v;
                        else
                            break; // restart if lost race to replace value
                    }
                    // else c < 0; fall through
                }
                Node<K, V> z = new Node<K, V>(kkey, value, n);
                if (!b.casNext(n, z))
                    break;    // restart if lost race to append to b
                int level = randomLevel();//得到一个随机的 level 作为该 key-value 插入的最高 level
                if (level > 0)
                    insertIndex(z, level);//进行插入操作
                return null;
            }
        }
    }

    /**
     * 获得一个随机的 level 值
     */
    private int randomLevel() {

        int x = randomSeed;
        x ^= x << 13;
        x ^= x >>> 17;
        randomSeed = x ^= x << 5;
        if ((x & 0x8001) != 0) // test highest and lowest bits
            return 0;
        int level = 1;
        while (((x >>>= 1) & 1) != 0) ++level;
        return level;
    }

    //执行插入操作：如上图所示,有两种可能的情况：
//1.当 level 存在时,对 level<=n 都执行 insert 操作
//2.当 level 不存在（大于目前的最大 level）时,首先添加新的 level,然后在执行操作 1
    private void insertIndex(Node<K, V> z, int level) {
        HeadIndex<K, V> h = head;
        int max = h.level;
        if (level <= max) {//情况 1
            Index<K, V> idx = null;
            for (int i = 1; i <= level; ++i)//首先得到一个包含 1~level 个级别的 down 关系的链表, 最后的 inx 为最高 level
                idx = new Index<K, V>(z, idx, null);
            addIndex(idx, h, level);//把最高 level 的 idx 传给 addIndex 方法
        } else { // 情况 2 增加一个新的级别
            level = max + 1;
            Index<K, V>[] idxs = (Index<K, V>[]) new Index[level + 1];
            Index<K, V> idx = null;
            for (int i = 1; i <= level; ++i)//该步骤和情况 1 类似
                idxs[i] = idx = new Index<K, V>(z, idx, null);
            HeadIndex<K, V> oldh;
            int k;
            for (; ; ) {
                oldh = head;
                int oldLevel = oldh.level;
                if (level <= oldLevel) { // lost race to add level
                    k = level;
                    break;
                }
                HeadIndex<K, V> newh = oldh;
                Node<K, V> oldbase = oldh.node;
                for (int j = oldLevel + 1; j <= level; ++j)
                    newh = new HeadIndex<K, V>(oldbase, newh, idxs[j], j);//创建新的
                if (casHead(oldh, newh)) {
                    k = oldLevel;
                    break;
                }
            }
            addIndex(idxs[k], oldh, k);
        }

    }

    /**
     * 在 1~indexlevel 层中插入数据
     */
    private void addIndex(Index<K, V> idx, HeadIndex<K, V> h, int indexLevel) {
        // insertionLevel 代表要插入的 level,该值会在 indexLevel~1 间遍历一遍
        int insertionLevel = indexLevel;
        Comparable<? super K> key = comparable(idx.node.key);
        if (key == null) throw new NullPointerException();
        // 和 get 操作类似,不同的就是查找的同时在各个 level 上加入了对应的 key
        for (; ; ) {
            int j = h.level;
            Index<K, V> q = h;
            Index<K, V> r = q.right;
            Index<K, V> t = idx;
            for (; ; ) {
                if (r != null) {
                    Node<K, V> n = r.node;
                    // compare before deletion check avoids needing recheck
                    int c = key.compareTo(n.key);
                    if (n.value == null) {
                        if (!q.unlink(r))
                            break;
                        r = q.right;
                        continue;
                    }
                    if (c > 0) {
                        q = r;
                        r = r.right;
                        continue;
                    }
                }
                if (j == insertionLevel) {//在该层 level 中执行插入操作
                    // Don't insert index if node already deleted
                    if (t.indexesDeletedNode()) {
                        findNode(key); // cleans up
                        return;
                    }
                    if (!q.link(r, t))//执行 link 操作,其实就是 inset 的实现部分
                        break; // restart
                    if (--insertionLevel == 0) {
                        // need final deletion check before return
                        if (t.indexesDeletedNode())
                            findNode(key);
                        return;
                    }
                }
                if (--j >= insertionLevel && j < indexLevel)//key 移动到下一层 level
                    t = t.down;
                q = q.down;
                r = q.right;
            }
        }
    }
```

# Java多线程和并发面试题（附答案）第6题

● java.util.concurrent.atomic包

● AtomicBoolean原子性布尔

AtomicBoolean是java.util.concurrent.atomic包下的原子变量，这个包里面提供了一组原子类。其基本的特性就是在多线程环境下，当有多个线程同时执行这些类的实例包含的方法时，具有排它性，即当某个线程进入方法，执行其中的指令时，不会被其他线程打断，而别的线程就像自旋锁一样，一直等到该方法执行完成，才由JVM从等待队列中选择一个另一个线程进入，这只是一种逻辑上的理解。实际上是借助硬件的相关指令来实现的，不会阻塞线程（或者说只是在硬件级别上阻塞了）。

AtomicBoolean，在这个Boolean值的变化的时候不允许在之间插入，保持操作的原子性。下面将解释重点方法并举例：

```java
boolean compareAndSet(expectedValue, updateValue);
```

**● 这个方法主要有两个作用:**

比较AtomicBoolean和expect的值，如果一致，执行方法内的语句。其实就是一个if语句。

把AtomicBoolean的值设成update，比较最要的是这两件事是一气呵成的，这连个动作之间不会被打断，任何内部或者外部的语句都不可能在两个动作之间运行。为多线程的控制提供了解决的方案。

**● 下面我们从代码上解释：**

首先我们看下在不使用 AtomicBoolean 情况下，代码的运行情况：

```java
package com.bjpowernode;

import java.util.concurrent.TimeUnit;

public class BarWorker implements Runnable {
    //静态变量
    private static boolean exists = false;

    private String name;

    public BarWorker(String name) {
        this.name = name;
    }

    @Override
    public void run() {
        if (!exists) {
            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e1) {
                // do nothing
            }
            exists = true;
            System.out.println(name + " enter");
            try {
                System.out.println(name + " working");
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                // do nothing
            }
            System.out.println(name + " leave");
            exists = false;
        } else {
            System.out.println(name + " give up");
        }

    }
    
    public static void main(String[] args) {
        BarWorker bar1 = new BarWorker("bar1");
        BarWorker bar2 = new BarWorker("bar2");
        new Thread(bar1).start();
        new Thread(bar2).start();
    }
}
```

运行结果：

bar1 enter

bar2 enter

bar1 working

bar2 working

bar1 leave

bar2 leave

从上面的运行结果我们可看到，两个线程运行时，都对静态变量exists同时做操作，并没有保证exists静态变量的原子性，也就是一个线程在对静态变量exists进行操作到时候，其他线程必须等待或不作为。等待一个线程操作完后，才能对其进行操作。

下面我们将静态变量使用AtomicBoolean来进行操作。

```java
package com.bjpowernode;

import java.util.concurrent.TimeUnit;
import java.util.concurrent.atomic.AtomicBoolean;

public class BarWorker2 implements Runnable {
    //静态变量使用 AtomicBoolean 进行操作
    private static AtomicBoolean exists = new AtomicBoolean(false);

    private String name;

    public BarWorker2(String name) {
        this.name = name;
    }

    @Override
    public void run() {
        if (exists.compareAndSet(false, true)) {

            System.out.println(name + " enter");

            try {
                System.out.println(name + " working");
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                // do nothing
            }
            System.out.println(name + " leave");
            exists.set(false);
        } else {
            System.out.println(name + " give up");
        }
    }

    public static void main(String[] args) {
        BarWorker2 bar1 = new BarWorker2("bar1");
        BarWorker2 bar2 = new BarWorker2("bar2");
        new Thread(bar1).start();
        new Thread(bar2).start();
    }
}
```

运行结果：

bar1 enter

bar1 working

bar2 give up

bar1 leave

可以从上面的运行结果看出仅仅一个线程进行工作，因为exists.compareAndSet(false，true)提供了原子性操作，比较和赋值操作组成了一个原子操作，中间不会提供可乘之机。使得一个线程操作，其他线程等待或不作为。

下面我们简单介绍下AtomicBoolean的API

● 你可以这样创建一个AtomicBoolean：

```java
AtomicBoolean atomicBoolean = new AtomicBoolean();  
```

以上示例新建了一个默认值为false的AtomicBoolean。如果你想要为AtomicBoolean实例设置一个显式的初始值，那么你可以将初始值传给AtomicBoolean的构造子：

```java
AtomicBoolean atomicBoolean = new AtomicBoolean(true); 
```

● 获得AtomicBoolean的值：

你可以通过使用get()方法来获取一个AtomicBoolean的值。示例如下：

```java
AtomicBoolean atomicBoolean = new AtomicBoolean(true);
boolean value = atomicBoolean.get();
```

● 设置AtomicBoolean的值：

你可以通过使用set()方法来设置一个AtomicBoolean的值。示例如下：

```java
AtomicBoolean atomicBoolean = new AtomicBoolean(true);
atomicBoolean.set(false);
```

以上代码执行后AtomicBoolean的值为false。

● 交换AtomicBoolean的值：

你可以通过 getAndSet()方法来交换一个AtomicBoolean实例的值。getAndSet()方法将返回AtomicBoolean当前的值，并将为AtomicBoolean设置一个新值。示例如下：

```java
AtomicBoolean atomicBoolean = new AtomicBoolean(true);
boolean oldValue = atomicBoolean.getAndSet(false);
```

以上代码执行后oldValue变量的值为true，atomicBoolean实例将持有false值。代码成功将AtomicBoolean当前值ture交换为false。

● 比较并设置 AtomicBoolean 的值：

compareAndSet()方法允许你对AtomicBoolean的当前值与一个期望值进行比较，如果当前值等于期望值的话，将会对AtomicBoolean设定一个新值。compareAndSet()方法是原子性的，因此在同一时间之内有单个线程执行它。因此compareAndSet()方法可被用于一些类似于锁的同步的简单实现。以下是一个compareAndSet()示例：

```java
AtomicBoolean atomicBoolean = new AtomicBoolean(true);
boolean expectedValue = true; boolean newValue = false;
boolean wasNewValueSet = atomicBoolean.compareAndSet(expectedValue， newValue);
```

本示例对AtomicBoolean的当前值与true值进行比较，如果相等，将AtomicBoolean的值更新为false。

● AtomicInteger原子性整型

AtomicInteger，一个提供原子操作的Integer的类。在Java语言中，++i和i++操作并不是线程安全的，在使用的时候，不可避免的会用到synchronized关键字。而AtomicInteger则通过一种线程安全的加减操作接口。

我们先来看看AtomicInteger给我们提供了什么方法：

```java
ublic final int get() //获取当前的值
public final int getAndSet(int newValue)//获取当前的值，并设置新的值
public final int getAndIncrement()//获取当前的值，并自增
public final int getAndDecrement() //获取当前的值，并自减
public final int getAndAdd(int delta) //获取当前的值，并加上预期的值
```

下面通过两个简单的例子来看一下AtomicInteger的优势在哪？

● 普通线程同步:

```java
class Test2 {
    private volatile int count = 0;
    public synchronized void increment() {
        count++; //若要线程安全执行执行 count++，需要加锁
    }

    public int getCount() {
        return count;
    }
}
```

● 使用AtomicInteger:

```java
import java.util.concurrent.atomic.AtomicInteger;

class Test2 {
    private AtomicInteger count = new AtomicInteger();
    public void increment() {
        count.incrementAndGet();
    }
    //使用 AtomicInteger 之后，不需要加锁，也可以实现线程安全。
    public int getCount() {
        return count.get();
    }
}
```

从上面的例子中我们可以看出：使用AtomicInteger是非常安全的，而且因为AtomicInteger由硬件提供原子操作指令实现的，在非激烈竞争的情况下，开销更小，速度更快。AtomicInteger是使用非阻塞算法来实现并发控制的。AtomicInteger的关键域只有以下3个：

```java
// setup to use Unsafe.compareAndSwapInt for updates
private static final Unsafe unsafe = Unsafe.getUnsafe();
private static final long valueOffset;

static {
    try {
        valueOffset = unsafe.objectFieldOffset(AtomicInteger.class.getDeclaredField("value"));
    } catch (Exception ex) {
        throw new Error(ex);
    }
}

private volatile int value;
```

这里，unsafe是java提供的获得对对象内存地址访问的类，注释已经清楚的写出了，它的作用就是在更新操作时提供“比较并替换”的作用。实际上就是AtomicInteger中的一个工具。valueOffset是用来记录value本身在内存的偏移地址的，这个记录也主要是为了在更新操作在内存中找到value的位置，方便比较。

注意：value是用来存储整数的时间变量，这里被声明为volatile，就是为了保证在更新操作时，当前线程可以拿到value最新的值（并发环境下，value可能已经被其他线程更新了）。

优点:最大的好处就是可以避免多线程的优先级倒置和死锁情况的发生，提升在高并发处理下的性能。

● 下面我们简单介绍下 AtomicInteger 的 API

● 创建一个 AtomicInteger 示例如下：

```java
AtomicInteger atomicInteger = new AtomicInteger();
```

本示例将创建一个初始值为0的AtomicInteger。如果你想要创建一个给

定初始值的AtomicInteger，你可以这样：

```java
AtomicInteger atomicInteger = new AtomicInteger(123);
```

本示例将123作为参数传给AtomicInteger的构造子，它将设置AtomicInteger实例的初始值为123。

● 获得AtomicInteger的值

你可以使用get()方法获取AtomicInteger实例的值。示例如下：

```java
AtomicInteger atomicInteger = new AtomicInteger(123);
int theValue = atomicInteger.get();
```

● 设置AtomicInteger的值

你可以通过set()方法对AtomicInteger的值进行重新设置。以下是AtomicInteger.set()示例：

```java
AtomicInteger atomicInteger = new AtomicInteger(123);
atomicInteger.set(234);
```

以上示例创建了一个初始值为123的AtomicInteger，而在第二行将其值更新为234。

● 比较并设置AtomicInteger的值

AtomicInteger类也通过了一个原子性的compareAndSet()方法。这一方法将AtomicInteger实例的当前值与期望值进行比较，如果二者相等，为AtomicInteger实例设置一个新值。 AtomicInteger.compareAndSet()代码示例：

```java
AtomicInteger atomicInteger = new AtomicInteger(123);
int expectedValue = 123;
int newValue = 234;
atomicInteger.compareAndSet(expectedValue,newValue);
```

本示例首先新建一个初始值为123的AtomicInteger实例。然后将AtomicInteger与期望值123进行比较，如果相等，将AtomicInteger的值更新为234。

● 增加AtomicInteger的值

AtomicInteger类包含有一些方法，通过它们你可以增加AtomicInteger的值，并获取其值。这些方法如下：

```java
public final int addAndGet(int addValue)//在原来的数值上增加新的值，并返回新值
public final int getAndIncrement()//获取当前的值，并自增
public final int incrementAndget() //自减，并获得自减后的值
public final int getAndAdd(int delta) //获取当前的值，并加上预期的值
```

第一个addAndGet()方法给AtomicInteger增加了一个值，然后返回增加后的值。getAndAdd()方法为AtomicInteger增加了一个值，但返回的是增加以前的AtomicInteger的值。具体使用哪一个取决于你的应用场景。以下是这两种方法的示例：

```java
AtomicInteger atomicInteger = new AtomicInteger();
System.out.println(atomicInteger.getAndAdd(10));
System.out.println(atomicInteger.addAndGet(10));
```

本示例将打印出0和20。例子中，第二行拿到的是加10之前的AtomicInteger的值。加10之前的值是0。第三行将AtomicInteger的值再加10，并返回加操作之后的值。该值现在是为20。你当然也可以使用这俩方法为AtomicInteger添加负值。结果实际是一个减法操作。getAndIncrement()和incrementAndGet()方法类似于getAndAdd()和addAndGet()，但每次只将AtomicInteger的值加1。

● 减小AtomicInteger的值

AtomicInteger类还提供了一些减小AtomicInteger的值的原子性方法。这些方法是：

```java
public final int decrementAndGet()
public final int getAndDecrement()
```

decrementAndGet()将AtomicInteger的值减一，并返回减一后的值。getAndDecrement()也将AtomicInteger的值减一，但它返回的是减一之前的值。

● AtomicIntegerArray原子性整型数组

java.util.concurrent.atomic.AtomicIntegerArray类提供了可以以原子方式读取和写入的底层int数组的操作，还包含高级原子操作。AtomicIntegerArray支持对底层int数组变量的原子操作。它具有获取和设置方法，如在变量上的读取和写入。也就是说，一个集合与同一变量上的任何后续get相关联。原子compareAndSet方法也具有这些内存一致性功能。

AtomicIntegerArray本质上是对int[]类型的封装。使用Unsafe类通过CAS的方式控制int[]在多线程下的安全性。它提供了以下几个核心API：

```java
//获得数组第 i 个下标的元素
public final int get(int i)
//获得数组的长度
public final int length()
//将数组第 i 个下标设置为 newValue,并返回旧的值
public final int getAndSet(int i, int newValue)
//进行 CAS 操作,如果第 i 个下标的元素等于 expect,则设置为 update,设置成功返回 true 
public final boolean compareAndSet(int i, int expect, int update)
//将第 i 个下标的元素加 1
public final int getAndIncrement(int i)
//将第 i 个下标的元素减 1
public final int getAndDecrement(int i)
//将第 i 个下标的元素增加 delta（delta 可以是负数）
public final int getAndAdd(int i,int delta)
```

下面给出一个简单的示例，展示 AtomicIntegerArray 使用：

```java
public class AtomicIntegerArrayDemo {
    static AtomicIntegerArray arr = new AtomicIntegerArray(10);
    public static class AddThread implements Runnable {
        public void run() {
            for (int k = 0; k < 10000; k++) 
	     arr.getAndIncrement(k % arr.length());
        }
    }

    public static void main(String[] args) throws InterruptedException {
        Thread[] ts = new Thread[10];
        for (int k = 0; k < 10; k++) {
            ts[k] = new Thread(new AddThread());
        }
        for (int k = 0; k < 10; k++) {
            ts[k].start();
        }
        for (int k = 0; k < 10; k++) {
            ts[k].join();
        }
        System.out.println(arr);
    }
}
```

输出结果：

[10000，10000，10000，10000，10000，10000，10000，10000，10000，10000]

上述代码第2行，申明了一个内含10个元素的数组。第3行定义的线程对数组内10个元素进行累加操作，每个元素各加1000次。第11行，开启10个这样的线程。因此，可以预测，如果线程安全，数组内10个元素的值必然都是10000。反之，如果线程不安全，则部分或者全部数值会小于10000。

● AtomicLong、AtomicLongArray原子性整型数组

AtomicLong、AtomicLongArray的API跟AtomicInteger、AtomicIntegerArray在使用方法都是差不多的。区别在于用前者是使用原子方式更新的long值和long数组，后者是使用原子方式更新的Integer值和Integer数组。两者的相同处在于它们此类确实扩展了Number，允许那些处理基于数字类的工具和实用工具进行统一访问。在实际开发中，它们分别用于不同的场景。这个就具体情况具体分析了，下面将举例说明AtomicLong的使用场景（使用AtomicLong生成自增长ID）。

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashSet;
import java.util.List;
import java.util.Set;
import java.util.concurrent.atomic.AtomicLong;

public class AtomicLongTest {

    /**
     * @param args
     */
    public static void main(String[] args) {
        final AtomicLong orderIdGenerator = new AtomicLong(0);
        final List<Item> orders = Collections
                .synchronizedList(new ArrayList<Item>());
        for (int i = 0; i < 10; i++) {
            Thread orderCreationThread = new Thread(new Runnable() {
                public void run() {
                    for (int i = 0; i < 10; i++) {
                        long orderId = orderIdGenerator.incrementAndGet();
                        Item order = new Item(Thread.currentThread().getName(), orderId);
                        orders.add(order);
                    }
                }
            });
            orderCreationThread.setName("Order Creation Thread " + i);
            orderCreationThread.start();
        }
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        Set<Long> orderIds = new HashSet<Long>();
        for (Item order : orders) {
            orderIds.add(order.getID());
            System.out.println("Order name:" + order.getItemName()
                    + "----" + "Order id:" + order.getID());
        }
    }
}

class Item {
    String itemName;
    long id;

    Item(String n, long id) {
        this.itemName = n;
        this.id = id;
    }

    public String getItemName() {
        return itemName;
    }

    public long getID() {
        return id;
    }
}
```

输出：

Order name:Order Creation Thread 0----Order id:1

Order name:Order Creation Thread 1----Order id:2

Order name:Order Creation Thread 0----Order id:4

Order name:Order Creation Thread 1----Order id:5

Order name:Order Creation Thread 3----Order id:3

Order name:Order Creation Thread 0----Order id:7

Order name:Order Creation Thread 1----Order id:6

........

Order name:Order Creation Thread 2----Order id:100

从运行结果我们看到，不管是哪个线程。它们获得的ID是不会重复的，保证的ID生成的原子性，避免了线程安全上的问题。

# Java多线程和并发面试题（附答案）7~10题

##### 7、多线程面试题

**1.多线程的创建方式**

（1）继承Thread类：但Thread本质上也是实现了Runnable接口的一个实例，它代表一个线程的实例，并且，启动线程的唯一方法就是通过Thread类的start()实例方法。start()方法是一个native方法，它将启动一个新线程，并执行run()方法。这种方式实现多线程很简单，通过自己的类直接extend Thread，并复写run()方法，就可以启动新线程并执行自己定义的run()方法。例如：继承Thread类实现多线程，并在合适的地方启动线程。

```java
public class MyThread extends Thread {
    public void run() {
        System.out.println("MyThread.run()");
    }
}
    MyThread myThread1 = new MyThread();
    MyThread myThread2 = new MyThread(); 
    myThread1.start();
    myThread2.start();
```

（2）实现Runnable接口的方式实现多线程，并且实例化Thread，传入自己的Thread实例，调用run()方法。

```java
public class MyThread implements Runnable {
    public void run() {
        System.out.println("MyThread.run()");
    }

}
    MyThread myThread = new MyThread();
    Thread thread = new Thread(myThread);
    thread.start();
```

（3）使用ExecutorService、Callable、Future实现有返回结果的多线程：ExecutorService、Callable、Future这个对象实际上都是属于Executor框架中的功能类。返回结果的线程是在JDK1.5中引入的新特征，确实很实用，有了这种特征我就不需要再为了得到返回值而大费周折了，而且即便实现了也可能漏洞百出。可返回值的任务必须实现Callable接口，类似的，无返回值的任务必须实现Runnable接口。执行Callable任务后，可以获取一个Future的对象，在该对象上调用get就可以获取到Callable任务返回的Object了，再结合线程池接口ExecutorService就可以实现有返回结果的多线程了。下面提供了一个完整的有返回结果的多线程测试例子，在JDK1.5下验证过没问题可以直接使用。代码如下：

```java
import java.util.concurrent.*;
import java.util.Date;
import java.util.List;
import java.util.ArrayList;

public class Test {

    public static void main(String[] args) throws ExecutionException， InterruptedException{
        System.out.println("----程序开始运行----");
        Date date1 = new Date();

        int taskSize = 5;
        // 创建一个线程池
        ExecutorService pool = Executors.newFixedThreadPool(taskSize);
        // 创建多个有返回值的任务
        List<Future> list = new ArrayList<Future>();
        for (int i = 0; i < taskSize; i++) {
            Callable c = new MyCallable(i + " ");
            // 执行任务并获取 Future 对象
            Future f = pool.submit(c);
            // System.out.println(">>>" + f.get().toString());
            list.add(f);
        }
        // 关闭线程池
        pool.shutdown();

        // 获取所有并发任务的运行结果
        for (Future f : list) {
            // 从 Future 对象上获取任务的返回值，并输出到控制台
            System.out.println(">>>" + f.get().toString());
        }
        Date date2 = new Date();
        System.out.println("----程序结束运行----，程序运行时间【" + (date2.getTime() - date1.getTime()) + "毫秒】");
    }
}

class MyCallable implements Callable<Object> {
    private String taskNum;

    MyCallable(String taskNum) {
        this.taskNum = taskNum;
    }

    public Object call() throws Exception {
        System.out.println(">>>" + taskNum + "任务启动");
        Date dateTmp1 = new Date();
        Thread.sleep(1000);
        Date dateTmp2 = new Date();
        long time = dateTmp2.getTime() - dateTmp1.getTime();
        System.out.println(">>>" + taskNum + "任务终止");
        return taskNum + "任务返回运行结果，当前任务时间【" + time + "毫秒】";
    }

}
```

**2.在java中wait和sleep方法的不同？**

最大的不同是在等待时wait会释放锁，而sleep一直持有锁。wait通常被用于线程间交互，sleep通常被用于暂停执行。

**3.synchronized和volatile关键字的作用？**

一旦一个共享变量（类的成员变量、类的静态成员变量）被volatile修饰之后，那么就具备了两层语义：

● 保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。

● 禁止进行指令重排序。

● volatile本质是在告诉jvm当前变量在寄存器（工作内存）中的值是不确定的，需要从主存中读取；synchronized则是锁定当前变量，只有当前线程可以访问该变量，其他线程被阻塞住。

● volatile仅能使用在变量级别；synchronized则可以使用在变量、方法、和类级别的。

● volatile仅能实现变量的修改可见性，并不能保证原子性；synchronized则可以保证变量的修改可见性和原子性。

● volatile不会造成线程的阻塞；synchronized可能会造成线程的阻塞。

volatile标记的变量不会被编译器优化；synchronized标记的变量可以被编译器优化。

**4.分析线程并发访问代码解释原因？**

```java
public class Counter {
    private volatile int count = 0;
    public void inc() {
        try {
            Thread.sleep(3);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        count++;
    }
    @Override
    public String toString() {
        return "[count=" + count + "]";
    }
}

public class VolatileTest {
    public static void main(String[] args) {
        final Counter counter = new Counter();
        for (int i = 0; i < 1000; i++) {
            new Thread(new Runnable() {
                @Override
                public void run() {
                    counter.inc();
                }
            }).start();
        }
        System.out.println(counter);
    }
}
```

上面的代码执行完后输出的结果确定为1000吗？答案是不一定，或者不等于 1000。你知道这是为什么吗？

在java的内存模型中每一个线程运行时都有一个线程栈，线程栈保存了线程运行时候变量值信息。当线程访问某一个对象时候值的时候，首先通过对象的引用找到对应在堆内存的变量的值，然后把堆内存变量的具体值load到线程本地内存中，建立一个变量副本，之后线程就不再和对象在堆内存变量值有任何关系，而是直接修改副本变量的值，在修改完之后的某一个时刻（线程退出之前），自动把线程变量副本的值回写到对象在堆中变量。这样在堆中的对象的值就产生变化了。

也就是说上面主函数中开启了1000个子线程，每个线程都有一个变量副本，每个线程修改变量只是临时修改了自己的副本，当线程结束时再将修改的值写入在主内存中，这样就出现了线程安全问题。因此结果就不可能等于1000了，一般都会小于1000。

上面的解释用一张图表示如下：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558324405@7dc9f65f8739ada505662e2d876948f5.png)

**5.什么是线程池，如何使用？**

线程池就是事先将多个线程对象放到一个容器中，当使用的时候就不用new线程而是直接去池中拿线程即可，节省了开辟子线程的时间，提高的代码执行效率。在JDK的java.util.concurrent.Executors中提供了生成多种线程池的静态方法。

```java
ExecutorService newCachedThreadPool = Executors.newCachedThreadPool();
ExecutorService newFixedThreadPool = Executors.newFixedThreadPool(4);
ScheduledExecutorService newScheduledThreadPool = Executors.newScheduledThreadPool(4);
ExecutorService newSingleThreadExecutor = Executors.newSingleThreadExecutor();
```

然后调用他们的 execute 方法即可。

**6.常用的线程池有哪些？**

● newSingleThreadExecutor：创建一个单线程的线程池，此线程池保证所有任务的执行顺序按照任务的提交顺序执行。

● newFixedThreadPool：创建固定大小的线程池，每次提交一个任务就创建一个线程，直到线程达到线程池的最大大小。

● newCachedThreadPool：创建一个可缓存的线程池，此线程池不会对线程池大小做限制，线程池大小完全依赖于操作系统（或者说JVM）能够创建的最大线程大小。

● newScheduledThreadPool：创建一个大小无限的线程池，此线程池支持定时以及周期性执行任务的需求。

● newSingleThreadExecutor：创建一个单线程的线程池。此线程池支持定时以及周期性执行任务的需求。

**7. 请叙述一下您对线程池的理解？**

（如果问到了这样的问题，可以展开的说一下线程池如何用、线程池的好处、线程池的启动策略）合理利用线程池能够带来三个好处。

第一：降低资源消耗。通过重复利用已创建的线程降低线程创建和销毁造成的消耗。

第二：提高响应速度。当任务到达时，任务可以不需要等到线程创建就能立即执行。

第三：提高线程的可管理性。线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一的分配，调优和监控。

##### 8.线程池的启动策略？

1、线程池刚创建时，里面没有一个线程。任务队列是作为参数传进来的。不过，就算队列里面有任务，线程池也不会马上执行它们。

2、当调用execute()方法添加一个任务时，线程池会做如下判断：

（1）如果正在运行的线程数量小于corePoolSize，那么马上创建线程运行这个任务；

（2）如果正在运行的线程数量大于或等于corePoolSize，那么将这个任务放入队列；

（3）如果这时候队列满了，而且正在运行的线程数量小于maximumPoolSize，那么还是要创建线程运行这个任务；

（4）如果队列满了，而且正在运行的线程数量大于或等于maximumPoolSize，那么线程池会抛出异常，告诉调用者“我不能再接受任务了”。

（5）当一个线程完成任务时，它会从队列中取下一个任务来执行。

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558324629@43559555d0f9a2db495b87c81d8f3faf.png)

（6）当一个线程无事可做，超过一定的时间（keepAliveTime）时，线程池会判断，如果当前运行的线程数大于corePoolSize，那么这个线程就被停掉。所以线程池的所有任务完成后，它最终会收缩到corePoolSize的大小。

##### 9.如何控制某个方法允许并发访问线程的个数？

```java
1.package com.bjpowernode;
2.
3.import java.util.concurrent.Semaphore;
4. /**
5. *
6. * @author dujubin
7. *
8. */
9. public class SemaphoreTest {
10./*
11.* permits the initial number of permits available. This value may be negative，
12.in which case releases must occur before any acquires will be granted.
13.fair true if this semaphore will guarantee first-in first-out granting of
14.permits under contention， else false 
15. */
16.     static Semaphore semaphore = new Semaphore(5，true);
17.     public static void main(String[] args) {
18.         for (int i = 0; i < 100; i++) {
19.             new Thread(new Runnable()
20.             {
21.                 @Override
22.                 public void run () {
23.                     test();
24.                 }
25.             }).start();
26.         }
27.
28.     }
29.
30.     public static void test() {
31.         try {
32.         //申请一个请求
33.         semaphore.acquire();
34.         } catch (InterruptedException e1) {
35.             e1.printStackTrace();
36.         }
37.             System.out.println(Thread.currentThread().getName() + "进来了");
38.         try {
39.             Thread.sleep(1000);
40.         } catch (InterruptedException e) {
41.             e.printStackTrace();
42.         }
43.             System.out.println(Thread.currentThread().getName() + "走了");
44.         //释放一个请求
45.         semaphore.release();
46.     }
47.}
```

可以使用Semaphore控制，第16行的构造函数创建了一个Semaphore对象，并且初始化了5个信号。这样的效果是控件test方法最多只能有5个线程并发访问，对于5个线程时就排队等待，走一个来一下。第33行，请求一个信号（消费一个信号），如果信号被用完了则等待，第45行释放一个信号，释放的信号新的线程就可以使用了。

##### 10.三个线程a、b、c并发运行，b，c需要a线程的数据怎么实现?

根据问题的描述，我将问题用以下代码演示，ThreadA、ThreadB、ThreadC，ThreadA用于初始化数据num，只有当num初始化完成之后再让ThreadB和ThreadC获取到初始化后的变量num。分析过程如下：

考虑到多线程的不确定性，因此我们不能确保ThreadA就一定先于ThreadB和ThreadC前执行，就算ThreadA先执行了，我们也无法保证ThreadA什么时候才能将变量num给初始化完成。因此我们必须让ThreadB和ThreadC去等待ThreadA完成任何后发出的消息。

现在需要解决两个难题，一是让ThreadB和ThreadC等待ThreadA先执行完，二是ThreadA执行完之后给ThreadB和ThreadC发送消息。

解决上面的难题我能想到的两种方案，一是使用纯Java API的Semaphore类来控制线程的等待和释放，二是使用Android提供的Handler消息机制。

解决方案一：

```java
1. package com.bjpowernode;
2. /**
3. * 三个线程 a、b、c 并发运行，b，c 需要 a 线程的数据怎么实现
4. *
5. */
6.public class ThreadCommunication {
7.      private static int num;//定义一个变量作为数据8.
9.      public static void main(String[] args) {
10.
11.            Thread threadA = new Thread(new Runnable() {
12.
13.            @Override
14.            public void run() {
15.                 try {
16.                     //模拟耗时操作之后初始化变量 num
17.                     Thread.sleep(1000);
18.                     num = 1;
19.
20.                 } catch (InterruptedException e) {
21.                     e.printStackTrace();
22.                 }
23.            }
24.            });
25.            Thread threadB = new Thread(new Runnable() { 
26.
27.            @Override
28.            public void run() {
29.            System.out.println(Thread.currentThread().getName()+"获取到 num 的值为："+num);
30.            }
31.            });
32.            Thread threadC = new Thread(new Runnable() { 
33.
34.            @Override
35.            public void run() {
36.            System.out.println(Thread.currentThread().getName()+"获取到 num 的值为："+num);
37.            }
38.            });
39.             //同时开启 3 个线程
40.             threadA.start();
41.             threadB.start();
42.             threadC.start();
43.
44.       }
45. } 
46.
```

解决方案二：

```java
ublic class ThreadCommunication {
    private static int num;
    /**
     * 定义一个信号量，该类内部维持了多个线程锁，可以阻塞多个线程，释放多个线程，
     * 线程的阻塞和释放是通过 permit 概念来实现的
     * 线程通过 semaphore.acquire()方法获取 permit，如果当前 semaphore 有 permit 则分配给该线程，
     * 如果没有则阻塞该线程直到 semaphore
     * 调用 release（）方法释放 permit。
     * 构造函数中参数：permit（允许） 个数，
     */
    private static Semaphore semaphore = new Semaphore(0);

    public static void main(String[] args) {

        Thread threadA = new Thread(new Runnable() {

            @Override
            public void run() {
                try {
                    //模拟耗时操作之后初始化变量 num
                    Thread.sleep(1000);
                    num = 1;
                    //初始化完参数后释放两个 permit
                    semaphore.release(2);

                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
        Thread threadB = new Thread(new Runnable() {

            @Override
            public void run() {
                try {
                    //获取 permit，如果 semaphore 没有可用的 permit 则等待，如果有则消耗一个
                    semaphore.acquire();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName() + "获取到 num 的值为：" + num);
            }
        });
        Thread threadC = new Thread(new Runnable() {

            @Override
            public void run() {
                try {
                    //获取 permit，如果 semaphore 没有可用的 permit 则等待，如果有则消耗一个
                    semaphore.acquire();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName() + "获取到 num 的值为：" + num);
            }
        });
        //同时开启 3 个线程
        threadA.start();
        threadB.start();
        threadC.start();

    }
}
```

# Java多线程和并发面试题（附答案）11~16题

##### 11.同一个类中的2个方法都加了同步锁，多个线程能同时访问同一个类中的这两个方法吗？

这个问题需要考虑到Lock与synchronized两种实现锁的不同情形。因为这种情况下使用Lock和synchronized会有截然不同的结果。Lock可以让等待锁的线程响应中断，Lock获取锁，之后需要释放锁。如下代码，多个线程不可访问同一个类中的2个加了Lock锁的方法。

```java
package com.bjpowernode;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class qq {

    private int count = 0;
    private Lock lock = new ReentrantLock();//设置 lock 锁
    //方法 1
    public Runnable run1 = new Runnable() {
        public void run() {
            lock.lock(); //加锁
            while (count < 1000) {
                try {
                    //打印是否执行该方法
                    System.out.println(Thread.currentThread().getName() + " run1: " + count++);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
      lock.unlock();
    }

    //方法 2
    public Runnable run2 = new Runnable() {
        public void run() {
            lock.lock();
            while (count < 1000) {
                try {
                    System.out.println(Thread.currentThread().getName() +
                            " run2: " + count++);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            lock.unlock();
        }
    };

    public static void main(String[] args) throws InterruptedException {
        qq t = new qq();    //创建一个对象
        new Thread(t.run1).start();//获取该对象的方法 1

        new Thread(t.run2).start();//获取该对象的方法 2
    }
}
```

结果是：

Thread-0 run1: 0

Thread-0 run1: 1

Thread-0 run1: 2

Thread-0 run1: 3

Thread-0 run1: 4

Thread-0 run1: 5

Thread-0 run1: 6

........

而synchronized却不行，使用synchronized时，当我们访问同一个类对象的时候，是同一把锁，所以可以访问该对象的其他synchronized方法。代码如下：

```java
package com.bjpowernode;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
public class qq {
    private int count = 0;
    private Lock lock = new ReentrantLock();
    public Runnable run1 = new Runnable() {
        public void run() {
            synchronized (this) { //设置关键字 synchronized，以当前类为锁
                while (count < 1000) {
                    try {
                        //打印是否执行该方法
                        System.out.println(Thread.currentThread().getName() + " run1: " + count++);
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    };
    public Runnable run2 = new Runnable() {
        public void run() {
            synchronized (this) {
                while (count < 1000) {
                    try {
                        System.out.println(Thread.currentThread().getName()
                                + " run2: " + count++);
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    };

    public static void main(String[] args) throws InterruptedException {
        qq t = new qq(); //创建一个对象
        new Thread(t.run1).start(); //获取该对象的方法 1
        new Thread(t.run2).start(); //获取该对象的方法 2
    }
}
```

结果为：

Thread-1 run2: 0

Thread-1 run2: 1

Thread-1 run2: 2

Thread-0 run1: 0

Thread-0 run1: 4

Thread-0 run1: 5

Thread-0 run1: 6

......

##### 12.什么情况下导致线程死锁，遇到线程死锁该怎么解决？

● 死锁的定义：所谓死锁是指多个线程因竞争资源而造成的一种僵局（互相等待），若无外力作用，这些进程都将无法向前推进。

● 死锁产生的必要条件：

● 互斥条件：线程要求对所分配的资源（如打印机）进行排他性控制，即在一段时间内某资源仅为一个线程所占有。此时若有其他线程请求该资源，则请求线程只能等待。

● 不剥夺条件：线程所获得的资源在未使用完毕之前，不能被其他线程强行夺走，即只能由获得该资源的线程自己来释放（只能是主动释放)。

● 请求和保持条件：线程已经保持了至少一个资源，但又提出了新的资源请求，而该资源已被其他线程占有，此时请求进程被阻塞，但对自己已获得的资源保持不放。

● 循环等待条件：存在一种线程资源的循环等待链，链中每一个线程已获得的资源同时被链中下一个线程所请求。即存在一个处于等待状态的线程集合{Pl，P2，...，pn}，其中Pi等待的资源被P(i+1)占有（i=0，1，...，n-1)，Pn等待的资源被P0占有，如图2-15所示。

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558324989@060b7ec2603b089c0ecd500d5120547a.png)

产生死锁的一个例子：

```java
package com.bjpowernode;

/**
 * 一个简单的死锁类
 * 当 DeadLock 类的对象 flag==1 时（td1），先锁定 o1，睡眠 500 毫秒
 * 而 td1 在睡眠的时候另一个 flag==0 的对象（td2）线程启动，先锁定 o2，睡眠 500 毫秒
 * <p>
 * <p>
 * <p>
 * td1 睡眠结束后需要锁定 o2 才能继续执行，而此时 o2 已被 td2 锁定；
 * td2 睡眠结束后需要锁定 o1 才能继续执行，而此时 o1 已被 td1 锁定；
 * td1、td2 相互等待，都需要得到对方锁定的资源才能继续执行，从而死锁。
 */
public class DeadLock implements Runnable {
    public int flag = 1;
    //静态对象是类的所有对象共享的
    private static Object o1 = new Object(), o2 = new Object();

    public void run() {
        System.out.println("flag=" + flag);
        if (flag == 1) {
            synchronized (o1) {
                try {
                    Thread.sleep(500);
                } catch (Exception e) {
                    e.printStackTrace();
                }
                synchronized (o2) {
                    System.out.println("1");
                }
            }
        }
        if (flag == 0) {
            synchronized (o2) {
                try {
                    Thread.sleep(500);
                } catch (Exception e) {
                    e.printStackTrace();
                }
                synchronized (o1) {
                    System.out.println("0");
                }
            }
        }
    }
    public static void main(String[] args) {
        DeadLock td1 = new DeadLock();
        DeadLock td2 = new DeadLock();
        td1.flag = 1;
        td2.flag = 0;
        //td1，td2 都处于可执行状态，但 JVM 线程调度先执行哪个线程是不确定的。
        //td2 的 run()可能在 td1 的 run()之前运行
        new Thread(td1).start();
        new Thread(td2).start();
    }
}
```

● 如何避免死锁？

在有些情况下死锁是可以避免的。两种用于避免死锁的技术：

● 加锁顺序（线程按照一定的顺序加锁）

```java
package bjpowernode.com;

public class DeadLock {
    public int flag = 1;
    //静态对象是类的所有对象共享的
    private static Object o1 = new Object(), o2 = new Object();

    public void money(int flag) {
        this.flag = flag;
        if (flag == 1) {
            synchronized (o1) {
                try {
                    Thread.sleep(500);
                } catch (Exception e) {
                    e.printStackTrace();
                }
                synchronized (o2) {
                    System.out.println("当前的线程是" +
                            Thread.currentThread().getName() + " " + "flag 的值" + "1");
                }
            }
        }
        if (flag == 0) {
            synchronized (o2) {
                try {
                    Thread.sleep(500);
                } catch (Exception e) {
                    e.printStackTrace();
                }
                synchronized (o1) {
                    System.out.println("当前的线程是" +
                            Thread.currentThread().getName() + " " + "flag 的值" + "0");
                }
            }
        }
    }

    public static void main(String[] args) {
        final DeadLock td1 = new DeadLock();
        final DeadLock td2 = new DeadLock();
        td1.flag = 1;
        td2.flag = 0;
        //td1，td2 都处于可执行状态，但 JVM 线程调度先执行哪个线程是不确定的。
        //td2 的 run()可能在 td1 的 run()之前运行
        final Thread t1 = new Thread(new Runnable() {
            public void run() {
                td1.flag = 1;
                td1.money(1);
            }
        });
        t1.start();
        Thread t2 = new Thread(new Runnable() {
            public void run() {
                // TODO Auto-generated method stub
                try {
                    //让 t2 等待 t1 执行完
                    t1.join();//核心代码，让 t1 执行完后 t2 才会执行
                } catch (InterruptedException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
                td2.flag = 0;
                td1.money(0);
            }
        });
        t2.start();
    }
}
```

结果：

当前的线程是 Thread-0 flag 的值 1

当前的线程是 Thread-1 flag 的值 0

● 加锁时限（线程尝试获取锁的时候加上一定的时限，超过时限则放弃对该锁的请求，并释放自己占有的锁）。

```java
package com.bjpowernode;

import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class DeadLock {

    public int flag = 1;
    //静态对象是类的所有对象共享的
    private static Object o1 = new Object(), o2 = new Object();

    public void money(int flag) throws InterruptedException {
        this.flag = flag;
        if (flag == 1) {
            synchronized (o1) {
                Thread.sleep(500);
                synchronized (o2) {
                    System.out.println("当前的线程是" +
                            Thread.currentThread().getName() + " " + "flag 的值" + "1");
                }
            }
        }
        if (flag == 0) {
            synchronized (o2) {
                Thread.sleep(500);
                synchronized (o1) {
                    System.out.println("当前的线程是" +
                            Thread.currentThread().getName() + " " + "flag 的值" + "0");
                }
            }
        }
    }

    public static void main(String[] args) {
        final Lock lock = new ReentrantLock();
        final DeadLock td1 = new DeadLock();
        final DeadLock td2 = new DeadLock();
        td1.flag = 1;
        td2.flag = 0;
        //td1,td2 都处于可执行状态,但 JVM 线程调度先执行哪个线程是不确定的。
        //td2 的 run()可能在 td1 的 run()之前运行39．
        final Thread t1 = new Thread(new Runnable() {
            public void run() {
                // TODO Auto-generated method stub
                String tName = Thread.currentThread().getName();

                td1.flag = 1;
                try {
                    //获取不到锁,就等 5 秒,如果 5 秒后还是获取不到就返回 false
                    if (lock.tryLock(5000, TimeUnit.MILLISECONDS)) {

                        System.out.println(tName + "获取到锁！");
                    } else {
                        System.out.println(tName + "获取不到锁！");
                        return;
                    }
                } catch (Exception e) {
                    e.printStackTrace();
                }

                try {
                    td1.money(1);
                } catch (Exception e) {
                    System.out.println(tName + "出错了！！！");
                } finally {
                    System.out.println("当前的线程是" + Thread.currentThread().getName() + "释放锁！！");
                    lock.unlock();
                }
            }
        });
        t1.start();
        Thread t2 = new Thread(new Runnable() {
            public void run() {
                String tName = Thread.currentThread().getName();
                // TODO Auto-generated method stub
                td1.flag = 1;
                try {
                    //获取不到锁,就等 5 秒,如果 5 秒后还是获取不到就返回 false
                    if (lock.tryLock(5000, TimeUnit.MILLISECONDS)) {
                        System.out.println(tName + "获取到锁！");
                    } else {
                        System.out.println(tName + "获取不到锁！");
                        return;
                    }
                } catch (Exception e) {
                    e.printStackTrace();
                }
                try {
                    td2.money(0);
                } catch (Exception e) {
                    System.out.println(tName + "出错了！！！");
                } finally {
                    System.out.println("当前的线程是" + Thread.currentThread().getName() + "释放锁！！");
                    lock.unlock();
                }
            }
        });
        t2.start();
    }
}
```

打印结果：

Thread-0获取到锁！

当前的线程是Thread-0 flag的值1

当前的线程是Thread-0释放锁！！

Thread-1获取到锁！

当前的线程是Thread-1 flag的值0

当前的线程是Thread-1释放锁！！

##### 13.Java 中多线程间的通信怎么实现?

线程通信的方式：

● 共享变量

线程间通信可以通过发送信号，发送信号的一个简单方式是在共享对象的变量里设置信号值。线程A在一个同步块里设置boolean型成员变量hasDataToProcess为true，线程B也在同步块里读取hasDataToProcess这个成员变量。这个简单的例子使用了一个持有信号的对象，并提供了set和get方法：

```java
package com.bjpowernode;

public class MySignal {
    //共享的变量
    private boolean hasDataToProcess = false;

    //取值
    public boolean getHasDataToProcess() {
        return hasDataToProcess;
    }
    //存值
    public void setHasDataToProcess(boolean hasDataToProcess) {
        this.hasDataToProcess = hasDataToProcess;
    }

    public static void main(String[] args) {
        //同一个对象
        final MySignal my = new MySignal();
        //线程 1 设置 hasDataToProcess 值为 true
        final Thread t1 = new Thread(new Runnable() {
            public void run() {
                my.setHasDataToProcess(true);
            }
        });
        t1.start();
        //线程 2 取这个值 hasDataToProcess
        Thread t2 = new Thread(new Runnable() {
            public void run() {
                try {
                    //等待线程 1 完成然后取值
                    t1.join();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                my.getHasDataToProcess();
                System.out.println("t1 改变以后的值：" + my.isHasDataToProcess());
            }
        });
        t2.start();
    }
}
```

结果：

t1 改变以后的值：true

● wait/notify机制

以资源为例，生产者生产一个资源，通知消费者就消费掉一个资源，生产者继续生产资源，消费者消费资源，以此循环。代码如下：

```java
package com.bjpowernode;

//资源类
class Resource {
    private String name;
    private int count = 1;
    private boolean flag = false;

    public synchronized void set(String name) {
        //生产资源
        while (flag) {
            try {
                //线程等待。消费者消费资源
                wait();
            } catch (Exception e) {
            }
        }
        this.name = name + "---" + count++;
        System.out.println(Thread.currentThread().getName() + "...生产者..." + this.name);
        flag = true;
        //唤醒等待中的消费者
        this.notifyAll();
    }

    public synchronized void out() {
        //消费资源
        while (!flag) {
            //线程等待，生产者生产资源
            try {
                wait();
            } catch (Exception e) {
            }
        }
        System.out.println(Thread.currentThread().getName() + "...消费者..." + this.name);
        flag = false;
        //唤醒生产者，生产资源
        this.notifyAll();
    }
}

//生产者
class Producer implements Runnable {
    private Resource res;

    Producer(Resource res) {
        this.res = res;
    }

    //生产者生产资源
    public void run() {
        while (true) {
            res.set("商品");
        }
    }
}

//消费者消费资源
class Consumer implements Runnable {
    private Resource res;

    Consumer(Resource res) {
        this.res = res;
    }

    public void run() {
        while (true) {
            res.out();
        }
    }
}

public class ProducerConsumerDemo {
    public static void main(String[] args) {
        Resource r = new Resource();
        Producer pro = new Producer(r);
        Consumer con = new Consumer(r);
        Thread t1 = new Thread(pro);
        Thread t2 = new Thread(con);
        t1.start();
        t2.start();
    }
}
```

##### 14.线程和进程的区别?

● 进程：具有一定独立功能的程序关于某个数据集合上的一次运行活动，是操作系统进行资源分配和调度的一个独立单位。

● 线程：是进程的一个实体，是cpu调度和分派的基本单位，是比进程更小的可以独立运行的基本单位。

特点：线程的划分尺度小于进程，这使多线程程序拥有高并发性，进程在运行时各自内存单元相互独立，线程之间内存共享，这使多线程编程可以拥有更好的性能和用户体验。

注意：多线程编程对于其它程序是不友好的，占据大量cpu资源。

##### 15.请说出同步线程及线程调度相关的方法？

wait()：使一个线程处于等待（阻塞）状态，并且释放所持有的对象的锁；

sleep()：使一个正在运行的线程处于睡眠状态，是一个静态方法，调用此方法要处理InterruptedException异常；

notify()：唤醒一个处于等待状态的线程，当然在调用此方法的时候，并不能确切的唤醒某一个等待状态的线程，而是由JVM确定唤醒哪个线程，而且与优先级无关；

notityAll()：唤醒所有处于等待状态的线程，该方法并不是将对象的锁给所有线程，而是让它们竞争，只有获得锁的线程才能进入就绪状态；

注意：java 5通过Lock接口提供了显示的锁机制，Lock接口中定义了加锁（lock()方法）和解锁（unLock()方法），增强了多线程编程的灵活性及对线程的协调。

##### 16.启动一个线程是调用run()方法还是start()方法？

启动一个线程是调用 start()方法，使线程所代表的虚拟处理机处于可运行状态，这意味着它可以由 JVM 调度并执行，这并不意味着线程就会立即运行。

run()方法是线程启动后要进行回调（callback）的方法。

# Java反射面试题及答案

##### 1、静态嵌套类 (Static Nested Class) 和内部类(Inner Class)的不同？

● 静态嵌套类：Static Nested Class是被声明为静态（static）的内部类，它可以不依赖于外部类实例被实例化。

● 内部类：需要在外部类实例化后才能实例化，其语法看起来挺诡异的。

##### 2、下面的代码哪些地方会产生编译错误？

```java
class Outer {
    class Inner {
    }
    public static void foo() {
        new Inner();
    }
    public void bar() {
        new Inner();
    }
    public static void main(String[] args) {
        new Inner();
    }
}
```

注意：Java中非静态内部类对象的创建要依赖其外部类对象，上面的面试题中foo和main方法都是静态方法，静态方法中没有this，也就是说没有所谓的外部类对象，因此无法创建内部类对象，如果要在静态方法中创建内部类对象，可以这样做：

```java
new Outer().new Inner();  
```

**● Java中的反射**

说说你对 Java 中反射的理解

Java中的反射首先是能够获取到Java中要反射类的字节码，获取字节码有三种方法，

Class.forName(className)。

类名.class。

this.getClass()。然后将字节码中的方法，变量，构造函数等映射成相应的Method、Filed、Constructor等类，这些类提供了丰富的方法可以被我们所使用。

# Java动态代理面试题及答案

##### 1、写一个 ArrayList 的动态代理类（笔试题）

```java
final List<String> list = new ArrayList<String>();
List<String> proxyInstance =
        (List<String>) Proxy.newProxyInstance(list.getClass().getClassLoader(),
                list.getClass().getInterfaces(),
                new InvocationHandler() {
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        return method.invoke(list, args);
                    }
                });
    proxyInstance.add("你好");
    System.out.println(list);
```

##### 2、动静态代理的区别，什么场景使用？

● 静态代理通常只代理一个类，动态代理是代理一个接口下的多个实现类。

● 静态代理事先知道要代理的是什么，而动态代理不知道要代理什么东西，只有在运行时才知道。

动态代理是实现JDK里的InvocationHandler接口的invoke方法，但注意的是代理的是接口，也就是你的业务类必须要实现接口，通过Proxy里的newProxyInstance得到代理对象。还有一种动态代理CGLIB，代理的是类，不需要业务类继承接口，通过派生的子类来实现代理。通过在运行时，动态修改字节码达到修改类的目的。AOP编程就是基于动态代理实现的，比如著名的Spring框架、Hibernate框架等等都是动态代理的使用例子。

# Java设计模式面试题（1~9题）

##### 1、你所知道的设计模式有哪些

Java中一般认为有 23 种设计模式，我们不需要所有的都会，但是其中常用的几种设计模式应该去掌握。下面列出了所有的设计模式。需要掌握的设计模式已经单独列出来了，当然能掌握的越多越好。

**● 总体来说设计模式分为三大类：**

创建型模式，共五种：工厂方法模式、抽象工厂模式、单例模式、建造者模式、原型模式。

结构型模式，共七种：适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式。

行为型模式，共十一种：策略模式、模板方法模式、观察者模式、迭代子模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式。

##### 2、单例设计模式？

最好理解的一种设计模式，分为懒汉式和饿汉式。

饿汉式：

```java
public class Singleton {
   
    // 直接创建对象
    public static Singleton instance = new Singleton();
   
    // 私有化构造函数
    private Singleton() {
    }

    // 返回对象实例
    public static Singleton getInstance() {
        return instance;
  
  }
```

懒汉式：

```java
public class Singleton {
    // 声明变量
    private static volatile Singleton singleton = null;
    // 私有构造函数
    private Singleton() {
    }
    // 提供对外方法
    public static Singleton getInstance() {
        if (singleton == null) {
            synchronized (Singleton.class) {
                if (singleton == null) {
                    singleton = new Singleton();
                }
            }
        }
        return singleton;
    }
}
```

##### 3、工厂设计模式？

工厂模式分为工厂方法模式和抽象工厂模式。

工厂方法模式分为三种：

● 普通工厂模式，就是建立一个工厂类，对实现了同一接口的一些类进行实例的创建。多个工厂方法模式，是对普通工厂方法模式的改进，在普通工厂方法模式中，如果传递的字符串出错，则不能正确创建对象。

● 多个工厂方法模式，是提供多个工厂方法，分别创建对象。

● 静态工厂方法模式，将上面的多个工厂方法模式里的方法置为静态的，不需要创建实例，直接调用即可。

● 普通工厂模式

```java
public interface Sender {
    public void Send();
}
public class MailSender implements Sender {
    @Override
    public void Send() {
        System.out.println("this is mail sender!");
    }
}
public class SmsSender implements Sender {
    @Override
    public void Send() {
        System.out.println("this is sms sender!");
    }
}
public class SendFactory {
    public Sender produce(String type) {
        if ("mail".equals(type)) {
            return new MailSender();
        } else if ("sms".equals(type)) {
            return new SmsSender();
        } else {
            System.out.println("请输入正确的类型!");
            return null;
        }
    }
}
```

● 多个工厂方法模式

该模式是对普通工厂方法模式的改进，在普通工厂方法模式中，如果传递的字符串出错，则不能正确创建对象，而多个工厂方法模式是提供多个工厂方法，分别创建对象。

```java
public class SendFactory {
    public Sender produceMail() {
        return new MailSender();
    }
    public Sender produceSms() {
        return new SmsSender();
    }
}
public class FactoryTest {
    public static void main(String[] args) {
        SendFactory factory = new SendFactory();
        Sender sender = factory.produceMail();
        sender.send();
    }
}
```

● 静态工厂方法模式

将上面的多个工厂方法模式里的方法置为静态的，不需要创建实例，直接调用即可。

```java
public class SendFactory {
    public static Sender produceMail() {
        return new MailSender();
    }
    public static Sender produceSms() {
        return new SmsSender();
    }
}
public class FactoryTest {
    public static void main(String[] args) {
        Sender sender = SendFactory.produceMail();
        sender.send();
    }
}
```

● 抽象工厂模式

工厂方法模式有一个问题就是，类的创建依赖工厂类，也就是说，如果想要拓展程序，必须对工厂类进行修改，这违背了闭包原则，所以，从设计角度考虑，有一定的问题，如何解决？就用到抽象工厂模式，创建多个工厂类，这样一旦需要增加新的功能，直接增加新的工厂类就可以了，不需要修改之前的代码。

```java
public interface Provider {
    public Sender produce();
}
public interface Sender {
    public void send();
}
public class MailSender implements Sender {
    @Override
    public void send() {
        System.out.println("this is mail sender!");
    }
}
public class SmsSender implements Sender {
    @Override
    public void send() {
        System.out.println("this is sms sender!");
    }
}
public class SendSmsFactory implements Provider {
    @Override
    public Sender produce() {
        return new SmsSender();
    }
}
public class SendMailFactory implements Provider {
    @Override
    public Sender produce() {
        return new MailSender();
    }
}
public class Test {
    public static void main(String[] args) {
        Provider provider = new SendMailFactory();
        Sender sender = provider.produce();
        sender.send();
    }
}
```

##### 4、建造者模式（Builder）

工厂类模式提供的是创建单个类的模式，而建造者模式则是将各种产品集中起来进行管理，用来创建复合对象， 所谓复合对象就是指某个类具有不同的属性，其实建造者模式就是前面抽象工厂模式和最后的 Test 结合起来得到的。

```java
public class Builder {
    private List<Sender> list = new ArrayList<Sender>();
    public void produceMailSender(int count) {
        for (int i = 0; i < count; i++) {
            list.add(new MailSender());
        }
    }
    public void produceSmsSender(int count) {
        for (int i = 0; i < count; i++) {
            list.add(new SmsSender());
        }
    }
}
public class Builder {
    private List<Sender> list = new ArrayList<Sender>();
    public void produceMailSender(int count) {
        for (int i = 0; i < count; i++) {
            list.add(new MailSender());
        }
    }
    public void produceSmsSender(int count) {
        for (int i = 0; i < count; i++) {
            list.add(new SmsSender());
        }
    }
}
public class TestBuilder {
    public static void main(String[] args) {
        Builder builder = new Builder();
        builder.produceMailSender(10);
    }
}
```

##### 5、适配器设计模式

适配器模式将某个类的接口转换成客户端期望的另一个接口表示，目的是消除由于接口不匹配所造成的类的兼容性问题。主要分为三类：类的适配器模式、对象的适配器模式、接口的适配器模式。

● 类的适配器模式

```java
public class Source {
    public void method1() {
        System.out.println("this is original method!");
    }
}
public interface Targetable {
    /* 与原类中的方法相同 */
    public void method1();
    /* 新类的方法 */
    public void method2();
}
public class Adapter extends Source implements Targetable {
    @Override
    public void method2() {
        System.out.println("this is the targetable method!");
    }
}
public class AdapterTest {
    public static void main(String[] args) {
        Targetable target = new Adapter();
        target.method1();
        target.method2();
    }
}
```

● 对象的适配器模式

基本思路和类的适配器模式相同，只是将 Adapter 类作修改，这次不继承 Source 类，而是持有 Source 类的实例，以达到解决兼容性的问题。

```java
public class Wrapper implements Targetable {
    private Source source;
    public Wrapper(Source source) {
        super();
        this.source = source;
    }
    @Override
    public void method2() {
        System.out.println("this is the targetable method!");
    }
    @Override
    public void method1() {
        source.method1();
    }
}
public class AdapterTest {
    public static void main(String[] args) {
        Source source = new Source();
        Targetable target = new Wrapper(source);
        target.method1();
        target.method2();
    }
}
```

● 接口的适配器模式

接口的适配器是这样的：有时我们写的一个接口中有多个抽象方法，当我们写该接口的实现类时，必须实现该接口的所有方法，这明显有时比较浪费，因为并不是所有的方法都是我们需要的，有时只需要某一些，此处为了解决这个问题，我们引入了接口的适配器模式，借助于一个抽象类，该抽象类实现了该接口，实现了所有的方法，而我们不和原始的接口打交道，只和该抽象类取得联系，所以我们写一个类，继承该抽象类，重写我们需要的方法就行。

##### 6、装饰模式（Decorator）

顾名思义，装饰模式就是给一个对象增加一些新的功能，而且是动态的，要求装饰对象和被装饰对象实现同一个接口，装饰对象持有被装饰对象的实例。

```java
public interface Sourceable {
    public void method();
}
public class Source implements Sourceable {
    @Override
    public void method() {
        System.out.println("the original method!");
    }
}
public class Decorator implements Sourceable {
    private Sourceable source;
    public Decorator(Sourceable source) {
        super();
        this.source = source;
    }
    @Override
    public void method() {
        System.out.println("before decorator!");
        source.method();
        System.out.println("after decorator!");
    }
}

public class DecoratorTest {
    public static void main(String[] args) {
        Sourceable source = new Source();
        Sourceable obj = new Decorator(source);
        obj.method();
    }
}
```

##### 7、策略模式（strategy）

策略模式定义了一系列算法，并将每个算法封装起来，使他们可以相互替换，且算法的变化不会影响到使用算法的客户。需要设计一个接口，为一系列实现类提供统一的方法，多个实现类实现该接口，设计一个抽象类（可有可无，属于辅助类），提供辅助函数。策略模式的决定权在用户，系统本身提供不同算法的实现，新增或者删除算法，对各种算法做封装。因此，策略模式多用在算法决策系统中，外部用户只需要决定用哪个算法即可。

```java
public interface ICalculator {
    public int calculate(String exp);
}
public class Minus extends AbstractCalculator implements ICalculator {
    @Override
    public int calculate(String exp) {
        int arrayInt[] = split(exp， "-");
        return arrayInt[0] - arrayInt[1];
    }
}
public class Plus extends AbstractCalculator implements ICalculator {
    @Override
    public int calculate(String exp) {
        int arrayInt[] = split(exp， "\\+");
        return arrayInt[0] + arrayInt[1];
    }
}
public class AbstractCalculator {
    public int[] split(String exp， String opt) {
        String array[] = exp.split(opt);
        int arrayInt[] = new int[2];
        arrayInt[0] = Integer.parseInt(array[0]);
        arrayInt[1] = Integer.parseInt(array[1]);
        return arrayInt;
    }
}
public class StrategyTest {
    public static void main(String[] args) {
        String exp = "2+8";
        ICalculator cal = new Plus();
        int result = cal.calculate(exp);
        System.out.println(result);
    }
}
```

##### 8、观察者模式（Observer）

观察者模式很好理解，类似于邮件订阅和RSS订阅，当我们浏览一些博客或wiki时，经常会看到RSS图标，就这的意思是，当你订阅了该文章，如果后续有更新，会及时通知你。其实，简单来讲就一句话：当一个对象变化时，其它依赖该对象的对象都会收到通知，并且随着变化！对象之间是一种一对多的关系。

```java
public interface Observer {
    public void update();
}
public class Observer1 implements Observer {
    @Override
    public void update() {
        System.out.println("observer1 has received!");
    }
}
public class Observer2 implements Observer {
    @Override
    public void update() {
        System.out.println("observer2 has received!");
    }
}
public interface Subject {
    /*增加观察者*/
    public void add(Observer observer);
    /*删除观察者*/
    public void del(Observer observer);
    /*通知所有的观察者*/
    public void notifyObservers();
    /*自身的操作*/
    public void operation();
}
public abstract class AbstractSubject implements Subject {
    private Vector<Observer> vector = new Vector<Observer>();
    @Override
    public void add(Observer observer) {
        vector.add(observer);
    }
    @Override
    public void del(Observer observer) {
        vector.remove(observer);
    }
    @Override
    public void notifyObservers() {
        Enumeration<Observer> enumo = vector.elements();
        while (enumo.hasMoreElements()) {
            enumo.nextElement().update();
        }
    }
}
public class MySubject extends AbstractSubject {
    @Override
    public void operation() {
        System.out.println("update self!");
        notifyObservers();
    }
}
public class ObserverTest {
    public static void main(String[] args) {
        Subject sub = new MySubject();
        sub.add(new Observer1());
        sub.add(new Observer2());
        sub.operation();
    }
}
```

##### 9、JVM 垃圾回收机制和常见算法

理论上来讲Sun公司只定义了垃圾回收机制规则而不局限于其实现算法，因此不同厂商生产的虚拟机采用的算法也不尽相同。

GC（Garbage Collector）在回收对象前首先必须发现那些无用的对象，如何去发现定位这些无用的对象？常用的搜索算法如下：

● 引用计数器算法（废弃）

引用计数器算法是给每个对象设置一个计数器，当有地方引用这个对象的时候，计数器+1，当引用失效的时候，计数器-1，当计数器为0的时候，JVM就认为对象不再被使用，是“垃圾”了。引用计数器实现简单，效率高；但是不能解决循环引用问问题（A对象引用B对象，B对象又引用A对象，但是A，B对象已不被任何其他对象引用），同时每次计数器的增加和减少都带来了很多额外的开销，所以在JDK1.1之后，这个算法已经不再使用了。

● 根搜索算法（使用）

根搜索算法是通过一些“GC Roots”对象作为起点，从这些节点开始往下搜索，搜索通过的路径成为引用链（Reference Chain），当一个对象没有被GC Roots的引用链连接的时候，说明这个对象是不可用的。

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558326069@a60a48b24ef18897fea6dc4f4f49dece.png)

**● GC Roots对象包括：**

虚拟机栈（栈帧中的本地变量表）中的引用的对象。

方法区域中的类静态属性引用的对象。

方法区域中常量引用的对象。

本地方法栈中JNI（Native方法）的引用的对象。

通过上面的算法搜索到无用对象之后，就是回收过程，回收算法如下：

标记—清除算法（Mark-Sweep）（DVM使用的算法）

标记—清除算法包括两个阶段：“标记”和“清除”。在标记阶段，确定所有要回收的对象，并做标记。清除阶段紧随标记阶段，将标记阶段确定不可用的对象清除。标记—清除算法是基础的收集算法，标记和清除阶段的效率不高，而且清除后回产生大量的不连续空间，这样当程序需要分配大内存对象时，可能无法找到足够的连续空间。

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558326105@350879173762c65054bcb04d6a3a2203.png)

● 复制算法（Copying）

复制算法是把内存分成大小相等的两块，每次使用其中一块，当垃圾回收的时候，把存活的对象复制到另一块上，然后把这块内存整个清理掉。复制算法实现简单，运行效率高，但是由于每次只能使用其中的一半，造成内存的利用率不高。现在的JVM用复制方法收集新生代，由于新生代中大部分对象（98%）都是朝生夕死的，所以两块内存的比例不是1:1(大概是8:1)

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558326142@546f8c0be844d5ff4f51222d510fa5f9.png)

● 标记—整理算法（Mark-Compact）

标记—整理算法和标记—清除算法一样，但是标记—整理算法不是把存活对象复制到另一块内存，而是把存活对象往内存的一端移动，然后直接回收边界以外的内存。标记—整理算法提高了内存的利用率，并且它适合在收集对象存活时间较长的老年代。

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558326185@95e380a6a71c2b6f2a5e2fe4dad97f34.png)

● 分代收集（Generational Collection）

分代收集是根据对象的存活时间把内存分为新生代和老年代，根据各个代对象的存活特点，每个代采用不同的垃圾回收算法。新生代采用复制算法，老年代采用标记—整理算法。垃圾算法的实现涉及大量的程序细节，而且不同的虚拟机平台实现的方法也各不相同。

# Java设计模式笔试题（10~13题）

##### 10、谈谈 JVM 的内存结构和内存分配

● Java内存模型：Java虚拟机将其管辖的内存大致分三个逻辑部分：方法区(Method Area)、Java栈和Java堆。

● 方法区是静态分配的，编译器将变量绑定在某个存储位置上，而且这些绑定不会在运行时改变。常数池，源代码中的命名常量、String常量和static变量保存在方法区。

● Java Stack是一个逻辑概念，特点是后进先出。一个栈的空间可能是连续的，也可能是不连续的。最典型的Stack应用是方法的调用，Java虚拟机每调用一次方法就创建一个方法帧（frame），退出该方法则对应的 方法帧被弹出(pop)。栈中存储的数据也是运行时确定的。

● Java堆分配(heap allocation)意味着以随意的顺序，在运行时进行存储空间分配和收回的内存管理模型。堆中存储的数据常常是大小、数量和生命期在编译时无法确定的。Java对象的内存总是在heap中分配。

● java内存分配

● 基础数据类型直接在栈空间分配；

● 方法的形式参数，直接在栈空间分配，当方法调用完成后从栈空间回收；

● 引用数据类型，需要用new来创建，既在栈空间分配一个地址空间，又在堆空间分配对象的类变量；

● 方法的引用参数，在栈空间分配一个地址空间，并指向堆空间的对象区，当方法调用完后从栈空间回收；

● 局部变量new出来时，在栈空间和堆空间中分配空间，当局部变量生命周期结束后，栈空间立刻被回收，堆空间区域等待GC回收；

● 方法调用时传入的实际参数，先在栈空间分配，在方法调用完成后从栈空间释放;

● 字符串常量在DATA区域分配，this在堆空间分配；

● 数组既在栈空间分配数组名称，又在堆空间分配数组实际的大小！

##### 11、Java中引用类型都有哪些？（重要）

● Java中对象的引用分为四种级别，这四种级别由高到低依次为：强引用、软引用、弱引用和虚引用。

● 强引用（StrongReference）

如果一个对象被人拥有强引用，那么垃圾回收器绝不会回收它。当内存空间不足，Java虚拟机宁愿抛出OutOfMemoryError错误，使程序异常终止，也不会靠随意回收具有强引用的对象来解决内存不足问题。Java的对象是位于heap中的，heap中对象有强可及对象、软可及对象、弱可及对象、虚可及对象和不可到达对象。应用的强弱顺序是强、软、弱、和虚。对于对象是属于哪种可及的对象，由他的最强的引用决定。如下代码：

```java
String abc=new String("abc"); //1
SoftReference<String> softRef=new SoftReference<String>(abc); //2
WeakReference<String> weakRef = new WeakReference<String>(abc); //3
abc=null; //4
softRef.clear();//5
```

第一行在heap堆中创建内容为“abc”的对象，并建立abc到该对象的强引用，该对象是强可及的。

第二行和第三行分别建立对heap中对象的软引用和弱引用，此时heap中的abc对象已经有3个引用，显然此时abc对象仍是强可及的。

第四行之后heap中对象不再是强可及的，变成软可及的。

第五行执行之后变成弱可及的。

● 软引用（SoftReference）

如果一个对象只具有软引用，那么如果内存空间足够，垃圾回收器就不会回收它，如果内存空间不足了，就会回收这些对象的内存。只要垃圾回收器没有回收它，该对象就可以被程序使用。软引用可用来实现内存敏感的高速缓存。软引用可以和一个引用队列（ReferenceQueue）联合使用，如果软引用所引用的对象被垃圾回收，Java虚拟机就会把这个软引用加入到与之关联的引用队列中。软引用是主要用于内存敏感的高速缓存。在jvm报告内存不足之前会清除所有的软引用，这样以来gc就有可能收集软可及的对象，可能解决内存吃紧问题，避免内存溢出。什么时候会被收集取决于gc的算法和gc运行时可用内存的大小。当gc决定要收集软引用时执行以下过程，

以上面的softRef为例：

（1）首先将softRef的referent（abc）设置为null，不再引用heap中的new String("abc")对象。

（2）将heap中的new String("abc")对象设置为可结束的(finalizable)。

（3）当heap中的new String("abc")对象的finalize()方法被运行而且该对象占用的内存被释放，softRef被添加到它的ReferenceQueue(如果有的话)中。

注意:对ReferenceQueue软引用和弱引用可以有可无，但是虚引用必须有。

被Soft Reference指到的对象，即使没有任何Direct Reference，也不会被清除。一直要到JVM内存不足且没有Direct Reference时才会清除，SoftReference是用来设计object-cache之用的。如此一来SoftReference不但可以把对象cache起来，也不会造成内存不足的错误（OutOfMemoryError）。

● 弱引用（WeakReference）

如果一个对象只具有弱引用，那该类就是可有可无的对象，因为只要该对象被gc扫描到了随时都会把它干掉。弱引用与软引用的区别在于：只具有弱引用的对象拥有更短暂的生命周期。在垃圾回收器线程扫描它所管辖的内存区域的过程中，一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存。不过，由于垃圾回收器是一个优先级很低的线程，因此不一定会很快发现那些只具有弱引用的对象。弱引用可以和一个引用队列（ReferenceQueue）联合使用，如果弱引用所引用的对象被垃圾回收，Java虚拟机就会把这个弱引用加入到与之关联的引用队列中。

● 虚引用（PhantomReference）

"虚引用"顾名思义，就是形同虚设，与其他几种引用都不同，虚引用并不会决定对象的生命周期。如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收。虚引用主要用来跟踪对象被垃圾回收的活动。虚引用与软引用和弱引用的一个区别在于：虚引用必须和引用队列（ReferenceQueue）联合使用。当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在回收对象的内存之前，把这个虚引用加入到与之关联的引用队列中。程序可以通过判断引用队列中是否已经加入了虚引用，来了解被引用的对象是否将要被垃圾回收。程序如果发现某个虚引用已经被加入到引用队列，那么就可以在所引用的对象的内存被回收之前采取必要的行动。建立虚引用之后通过get方法返回结果始终为null，通过源代码你会发现，虚引用通向会把引用的对象写进referent，只是get方法返回结果为null。先看一下和gc交互的过程再说一下他的作用。

（1）不把referent设置为null，直接把heap中的new String("abc")对象设置为可结束的(finalizable)。

（2）与软引用和弱引用不同，先把PhantomRefrence对象添加到它的ReferenceQueue中.然后在释放虚可及的对象。

##### 12、heap和stack有什么区别？

● 从以下几个方面阐述堆（heap）和栈（stack）的区别。

● 申请方式

stack:由系统自动分配。例如，声明在函数中一个局部变量int b;系统自动在栈中为b开辟空间。

heap:需要程序员自己申请，并指明大小，在c中malloc函数，对于Java需要手动new Object()的形式开辟。

● 申请后系统的响应

stack：只要栈的剩余空间大于所申请空间，系统将为程序提供内存，否则将报异常提示栈溢出。

heap：首先应该知道操作系统有一个记录空闲内存地址的链表，当系统收到程序的申请时，

会遍历该链表，寻找第一个空间大于所申请空间的堆结点，然后将该结点从空闲结点链表中删除，并将该结点的空间分配给程序。另外，由于找到的堆结点的大小不一定正好等于申请的大小，系统会自动的将多余的那部分重新放入空闲链表中。

● 申请大小的限制

stack：栈是向低地址扩展的数据结构，是一块连续的内存的区域。这句话的意思是栈顶的地址和栈的最大容量是系统预先规定好的，在WINDOWS下，栈的大小是2M（也有的说是1M，总之是一个编译时就确定的常数），如果申请的空间超过栈的剩余空间时，将提示overflow。因此，能从栈获得的空间较小。

heap：堆是向高地址扩展的数据结构，是不连续的内存区域。这是由于系统是用链表来存储的空闲内存地址的，自然是不连续的，而链表的遍历方向是由低地址向高地址。堆的大小受限于计算机系统中有效的虚拟内存。由此可见，堆获得的空间比较灵活，也比较大。

● 申请效率的比较：

stack：由系统自动分配，速度较快。但程序员是无法控制的。

heap：由new分配的内存，一般速度比较慢，而且容易产生内存碎片，不过用起来最方便。

● heap和stack中的存储内容

stack：在函数调用时，第一个进栈的是主函数中后的下一条指令（函数调用语句的下一条可执行语句）的地址，然后是函数的各个参数，在大多数的C编译器中，参数是由右往左入栈的，然后是函数中的局部变量。注意静态变量是不入栈的。

当本次函数调用结束后，局部变量先出栈，然后是参数，最后栈顶指针指向最开始存的地址，也就是主函数中的下一条指令，程序由该点继续运行。

heap：一般是在堆的头部用一个字节存放堆的大小。堆中的具体内容有程序员安排。

● 数据结构层面的区别

数据结构方面的堆和栈，这些都是不同的概念。这里的堆实际上指的就是（满足堆性质的）优先队列的一种数据结构，第1个元素有最高的优先权；栈实际上就是满足先进后出的性质的数学或数据结构。虽然堆栈，堆栈的说法是连起来叫，但是他们还是有很大区别的，连着叫只是由于历史的原因。

● 拓展知识（Java中堆栈的应用）

（1）栈(stack)与堆(heap)都是Java用来在Ram中存放数据的地方。与C++不同，Java自动管理栈和堆，程序员不能直接地设置栈或堆。

（2）栈的优势是，存取速度比堆要快，仅次于直接位于CPU中的寄存器。但缺点是，存在栈中的数据大小与生存期必须是确定的，缺乏灵活性。另外，栈数据可以共享，详见第3点。堆的优势是可以动态地分配内存大小，生存期也不必事先告诉编译器，Java的垃圾回收器会自动收走这些不再使用的数据。但缺点是，由于要在运行时动态分配内存，存取速度较慢。

（3）Java中的数据类型有两种。

一种是基本类型(primitive types)，共有8种，即int，short，long，byte，float，double，boolean，char(注意，并没有string的基本类型)。这种类型的定义是通过诸如int a=3;long b=255L;的形式来定义的，称为自动变量（自动变量：只在定义它们的时候才创建，在定义它们的函数返回时系统回收变量所占存储空间。对这些变量存储空间的分配和回收是由系统自动完成的。）。值得注意的是，自动变量存的是字面值，不是类的实例，即不是类的引用，这里并没有类的存在。如int a=3;这里的a是一个指向int类型的引用，指向3这个字面值。这些字面值的数据，由于大小可知，生存期可知(这些字面值固定定义在某个程序块里面，程序块退出后，字段值就消失了)，出于追求速度的原因，就存在于栈中。另外，栈有一个很重要的特殊性，就是存在栈中的数据可以共享。

假设我们同时定义：

```java
int a=3;
int b=3;
```

编译器先处理int a=3；首先它会在栈中创建一个变量为a的引用，然后查找有没有字面值为3的地址，没找到，就开辟一个存放3这个字面值的地址，然后将a指向3的地址。接着处理int b=3；在创建完b的引用变量后，由于在栈中已经有3这个字面值，便将b直接指向3的地址。这样，就出现了a与b同时均指向3的情况。

特别注意的是，这种字面值的引用与类对象的引用不同。假定两个类对象的引用同时指向一个对象，如果一个对象引用变量修改了这个对象的内部状态，那么另一个对象引用变量也即刻反映出这个变化。相反，通过字面值的引用来修改其值，不会导致另一个指向此字面值的引用的值也跟着改变的情况。如上例，我们定义完a与b的值后，再令a=4；那么，b不会等于4，还是等于3。在编译器内部，遇到a=4；时，它就会重新搜索栈中是否有4的字面值，如果没有，重新开辟地址存放4的值；如果已经有了，则直接将a指向这个地址。因此a值的改变不会影响到b的值。

另一种是包装类数据，如Integer，String，Double等将相应的基本数据类型包装起来的类。这些类数据全部存在于堆中，Java用new()语句来显示地告诉编译器，在运行时才根据需要动态创建，因此比较灵活，但缺点是要占用更多的时间。

（4）每个JVM的线程都有自己的私有的栈空间，随线程创建而创建，java的stack存放的是frames，java的stack和c的不同，只是存放本地变量，返回值和调用方法，不允许直接push和pop frames，因为frames可能是有heap分配的，所以java的stack分配的内存不需要是连续的。java的heap是所有线程共享的，堆存放所有runtime data，里面是所有的对象实例和数组，heap是JVM启动时创建。

（5）String是一个特殊的包装类数据。即可以用String str=new String("abc");的形式来创建，也可以用String str="abc"；的形式来创建(作为对比，在JDK 5.0之前，你从未见过Integer i=3;的表达式，因为类与字面值是不能通用的，除了String。而在JDK 5.0中，这种表达式是可以的！因为编译器在后台进行Integer i=new Integer(3)的转换)。前者是规范的类的创建过程，即在Java中，一切都是对象，而对象是类的实例，全部通过new()的形式来创建。那为什么在String str="abc"；中，并没有通过new()来创建实例，是不是违反了上述原则？其实没有。

（5.1）关于String str="abc"的内部工作。Java内部将此语句转化为以下几个步骤：

(1)先定义一个名为str的对String类的对象引用变量：String str；

(2)在栈中查找有没有存放值为"abc"的地址，如果没有，则开辟一个存放字面值为"abc"的地址，接着创建一个新的String类的对象o，并将o的字符串值指向这个地址，而且在栈中这个地址旁边记下这个引用的对象o。如果已经有了值为"abc"的地址，则查找对象o，并返回o的地址。

(3)将str指向对象o的地址。值得注意的是，一般String类中字符串值都是直接存值的。但像String str="abc"；这种场合下，其字符串值却是保存了一个指向存在栈中数据的引用！

为了更好地说明这个问题，我们可以通过以下的几个代码进行验证。

```java
String str1 = "abc"; 
String str2 = "abc";
System.out.println(str1==str2);    //true
```

注意，我们这里并不用str1.equals(str2)；的方式，因为这将比较两个字符串的值是否相等。==号，根据JDK的说明，只有在两个引用都指向了同一个对象时才返回true值。而我们在这里要看的是，str1与str2是否都指向了同一个对象。

结果说明，JVM创建了两个引用str1和str2，但只创建了一个对象，而且两个引用都指向了这个对象。我们再来更进一步，将以上代码改成：

```java
String str1 = "abc"; 
String str2 = "abc";
str1 = "bcd";
System.out.println(str1 + "，" + str2); //bcd， abc System.out.println(str1==str2); //false
```

这就是说，赋值的变化导致了类对象引用的变化，str1指向了另外一个新对象！而str2仍旧指向原来的对象。上例中，当我们将str1的值改为"bcd"时，JVM发现在栈中没有存放该值的地址，便开辟了这个地址，并创建了一个新的对象，其字符串的值指向这个地址。

事实上，String类被设计成为不可改变(immutable)的类。如果你要改变其值，可以，但JVM在运行时根据新值悄悄创建了一个新对象，然后将这个对象的地址返回给原来类的引用。这个创建过程虽说是完全自动进行的，但它毕竟占用了更多的时间。在对时间要求比较敏感的环境中，会带有一定的不良影响。

再修改原来代码：

```java
String str1 = "abc"; 
String str2 = "abc"; 
str1 = "bcd"; 
String str3 = str1;
System.out.println(str3);  //bcd String str4 = "bcd";
System.out.println(str1 == str4);  //true
```

str3这个对象的引用直接指向str1所指向的对象(注意，str3并没有创建新对象)。当str1改完其值后，再创建一个String的引用str4，并指向因str1修改值而创建的新的对象。可以发现，这回str4也没有创建新的对象，从而再次实现栈中数据的共享。

我们再接着看以下的代码。

```java
String str1 = new String("abc"); 
String str2 = "abc";
System.out.println(str1==str2);    //false
```

创建了两个引用。创建了两个对象。两个引用分别指向不同的两个对象。

以上两段代码说明，只要是用new()来新建对象的，都会在堆中创建，而且其字符串是单独存值的，即使与栈中的数据相同，也不会与栈中的数据共享。

● 数据类型包装类的值不可修改。不仅仅是String类的值不可修改，所有的数据类型包装类都不能更改其内部的值。

● 结论与建议：

（1）我们在使用诸如String str="abc"；的格式定义类时，总是想当然地认为，我们创建了String类的对象str。担心陷阱！对象可能并没有被创建！唯一可以肯定的是，指向String类的引用被创建了。至于这个引用到底是否指向了一个新的对象，必须根据上下文来考虑，除非你通过new()方法来显要地创建一个新的对象。因此，更为准确的说法是，我们创建了一个指向String类的对象的引用变量str，这个对象引用变量指向了某个值为"abc"的String类。清醒地认识到这一点对排除程序中难以发现的bug是很有帮助的。

（2）使用String str="abc"；的方式，可以在一定程度上提高程序的运行速度，因为JVM会自动根据栈中数据的实际情况来决定是否有必要创建新对象。而对于String str=new String("abc")；的代码，则一概在堆中创建新对象，而不管其字符串值是否相等，是否有必要创建新对象，从而加重了程序的负担。这个思想应该是享元模式的思想，但JDK的内部在这里实现是否应用了这个模式，不得而知。

（3）当比较包装类里面的数值是否相等时，用equals()方法；当测试两个包装类的引用是否指向同一个对象时，用 ==。

（4）由于String类的immutable性质，当String变量需要经常变换其值时，应该考虑使用StringBuffer类，以提高程序效率。

如果java不能成功分配heap的空间，将抛出OutOfMemoryError。

##### 13、解释内存中的栈(stack)、堆(heap)和方法区(method area)的用法

通常我们定义一个基本数据类型的变量，一个对象的引用，还有就是函数调用的现场保存都使用JVM中的栈空间；而通过new关键字和构造器创建的对象则放在堆空间，堆是垃圾收集器管理的主要区域，由于现在的垃圾收集器都采用分代收集算法，所以堆空间还可以细分为新生代和老生代，再具体一点可以分为Eden、Survivor（又可分为From Survivor和To Survivor）、Tenured；方法区和堆都是各个线程共享的内存区域，用于存储已经被JVM加载的类信息、常量、静态变量、JIT编译器编译后的代码等数据；程序中的字面量（literal）如直接书写的100、"hello"和常量都是放在常量池中，常量池是方法区的一部分。栈空间操作起来最快但是栈很小，通常大量的对象都是放在堆空间，栈和堆的大小都可以通过JVM的启动参数来进行调整，栈空间用光了会引发StackOverflowError，而堆和常量池空间不足则会引发OutOfMemoryError。

```java
String str = new String("hello");
```

上面的语句中变量str放在栈上，用new创建出来的字符串对象放在堆上，而"hello"这个字面量是放在方法区的。

# Java类加载器面试题

##### 1、Java的类加载器的种类都有哪些？

● 根类加载器(Bootstrap)--C++写的，看不到源码；

● 扩展类加载器（Extension）--加载位置：jre\lib\ext中；

● 系统(应用)类加载器(System\App) --加载位置：classpath中；

● 自定义加载器(必须继承ClassLoader)。

##### 2、类什么时候被初始化？

● 创建类的实例，也就是new一个对象

● 访问某个类或接口的静态变量，或者对该静态变量赋值

● 调用类的静态方法

● 反射（Class.forName("com.lyj.load")）

● 初始化一个类的子类（会首先初始化子类的父类）

● JVM启动时标明的启动类，即文件名和类名相同的那个类只有这6中情况才会导致类的类的初始化。

**● 类的初始化步骤：**

如果这个类还没有被加载和链接，那先进行加载和链接；

假如这个类存在直接父类，并且这个类还没有被初始化（注意：在一个类加载器中，类只能初始化一次），那就初始化直接的父类（不适用于接口）；

加入类中存在初始化语句（如static变量和static块），那就依次执行这些初始化语句。

##### 3、Java类加载体系之ClassLoader双亲委托机制

java是一种类型安全的语言，它有四类称为安全沙箱机制的安全机制来保证语言的安全性，这四类安全沙箱分别是：

1.类加载体系

2.class文件检验器

3.内置于Java虚拟机（及语言）的安全特性

4.安全管理器及Java API

**● 主要讲解类的加载体系：**

java程序中的.java文件编译完会生成.class文件，而.class文件就是通过被称为类加载器的ClassLoader加载的，而ClassLoder在加载过程中会使用“双亲委派机制”来加载.class文件，图：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190520/1558327102@7324ea804022678b5911eaad54e39052.png)

BootStrapClassLoader：启动类加载器，该ClassLoader是jvm在启动时创建的，用于加载$JAVA_HOME$/jre/lib下面的类库（或者通过参数-Xbootclasspath指定）。由于启动类加载器涉及到虚拟机本地实现细节，开发者无法直接获取到启动类加载器的引用，所以不能直接通过引用进行操作。

ExtClassLoader：扩展类加载器，该ClassLoader是在sun.misc.Launcher里作为一个内部类ExtClassLoader定义的(即sun.misc.Launcher$ExtClassLoader)，ExtClassLoader会加载$JAVA_HOME/jre/lib/ext下的类库(或者通过参数-Djava.ext.dirs指定)。

AppClassLoader：应用程序类加载器，该ClassLoader同样是在sun.misc.Launcher里作为一个内部类，AppClassLoader定义的（即sun.misc.Launcher$AppClassLoader），AppClassLoader会加载java环境变量CLASSPATH所指定的路径下的类库，而CLASSPATH所指定的路径可以通过System.getProperty("java.class.path")获取；当然，该变量也可以覆盖，可以使用参数-cp，例如：java-cp路径（可以指定要执行的class目录）。

CustomClassLoader：自定义类加载器，该ClassLoader是指我们自定义的ClassLoader，比如tomcat的StandardClassLoader属于这一类；当然，大部分情况下使用AppClassLoader就足够了。

前面谈到了ClassLoader的几类加载器，而ClassLoader使用双亲委派机制来加载class文件的。ClassLoader的双亲委派机制是这样的（这里先忽略掉自定义类加载器CustomClassLoader）：

1.当AppClassLoader加载一个class时，它首先不会自己去尝试加载这个类，而是把类加载请求委派给父类加载器ExtClassLoader去完成。

2.当ExtClassLoader加载一个class时，它首先也不会自己去尝试加载这个类，而是把类加载请求委派给BootStrapClassLoader去完成。

3.如果BootStrapClassLoader加载失败（例如在$JAVA_HOME$/jre/lib里未查找到该class），会使用ExtClassLoader来尝试加载；

4.若ExtClassLoader也加载失败，则会使用AppClassLoader来加载，如果AppClassLoader也加载失败，则会报出异常ClassNotFoundException。

下面贴下ClassLoader的loadClass(String name，boolean resolve)的源码：

```java
protected synchronized Class<?> loadClass(String name，boolean resolve)throws ClassNotFoundException{
        // 首先找缓存是否有 class
        Class c=findLoadedClass(name);
        if(c==null){
        //没有判断有没有父类
        try{
        if(parent!=null){
        //有的话，用父类递归获取 class
        c=parent.loadClass(name，false);
	}else{
        //没有父类。通过这个方法来加载
        c=findBootstrapClassOrNull(name);}
        }catch(ClassNotFoundException e){
        // ClassNotFoundException thrown if class not found
        // from the non-null parent class loader
        }
        if(c==null){
        // 如果还是没有找到，调用 findClass(name)去找这个类
        c=findClass(name);}
        }
        if(resolve){
        resolveClass(c);}
        return c;
        }
```

代码很明朗：

首先找缓存（findLoadedClass），没有的话就判断有没有parent，有的话就用parent来递归的loadClass，然而ExtClassLoader并没有设置parent，则会通过findBootstrapClassOrNull来加载class，而findBootstrapClassOrNull则会通过JNI方法”private native Class findBootstrapClass(String name)”来使用BootStrapClassLoader来加载class。

如果parent未找到class，则会调用findClass来加载class，findClass是一个protected的空方法，可以覆盖它以便自定义class加载过程。另外，虽然ClassLoader加载类是使用loadClass方法，但是鼓励用ClassLoader的子类重写findClass(String)，而不是重写loadClass，这样就不会覆盖了类加载默认的双亲委派机制。

双亲委派托机制为什么安全：

举个例子，ClassLoader加载的class文件来源很多，比如编译器编译生成的class、或者网络下载的字节码。而一些来源的class文件是不可靠的，比如我可以自定义一个java.lang.Integer类来覆盖jdk中默认的Integer类，例如下面这样：

```java
package java.lang;
public class Integer {
    public Integer(int value) {
        System.exit(0);
    }
}
```

初始化这个Integer的构造器是会退出JVM，破坏应用程序的正常进行，如果使用双亲委派机制的话该Integer类永远不会被调用，以为委托BootStrapClassLoader加载后会加载JDK中的Integer类而不会加载自定义的这个，可以看下下面这测试个用例：

```java
public static void main(String...args){
        Integer i=new Integer(1);
        System.err.println(i);
}
```

执行时JVM并未在new Integer(1)时退出，说明未使用自定义的Integer，于是就保证了安全性。

##### 4、描述一下 JVM 加载 class?

JVM中类的装载是由类加载器（ClassLoader）和它的子类来实现的，Java中的类加载器是一个重要的Java运行时系统组件，它负责在运行时查找和装入类文件中的类。

1.由于Java的跨平台性，经过编译的Java源程序并不是一个可执行程序，而是一个或多个类文件。

2.当Java程序需要使用某个类时，JVM会确保这个类已经被加载、连接（验证、准备和解析）和初始化。类的加载是指把类的.class文件中的数据读入到内存中，通常是创建一个字节数组读入.class文件，然后产生与所加载类对应的Class对象。加载完成后，Class对象还不完整，所以此时的类还不可用。

3.当类被加载后就进入连接阶段，这一阶段包括验证、准备（为静态变量分配内存并设置默认的初始值）和解析（将符号引用替换为直接引用）三个步骤。最后JVM对类进行初始化，包括：如果类存在直接的父类并且这个类还没有被初始化，那么就先初始化父类；如果类中存在初始化语句，就依次执行这些初始化语句。

4.类的加载是由类加载器完成的，类加载器包括：根加载器（BootStrap）、扩展加载器（Extension）、系统加载器（System）和用户自定义类加载器（java.lang.ClassLoader的子类）。

5.从Java 2（JDK 1.2）开始，类加载过程采取了父亲委托机制（PDM）。PDM更好的保证了Java平台的安全性，在该机制中，JVM自带的Bootstrap是根加载器，其他的加载器都有且仅有一个父类加载器。类的加载首先请求父类加载器加载，父类加载器无能为力时才由其子类加载器自行加载。JVM不会向Java程序提供对Bootstrap的引用。

● 下面是关于几个类加载器的说明：

1.Bootstrap：一般用本地代码实现，负责加载JVM基础核心类库（rt.jar）；

2.Extension：从java.ext.dirs系统属性所指定的目录中加载类库，它的父加载器是Bootstrap；

3.System：又叫应用类加载器，其父类是Extension。它是应用最广泛的类加载器。它从环境变量classpath或者系统属性java.class.path所指定的目录中记载类，是用户自定义加载器的默认父加载器。

##### 5、获得一个类对象有哪些方式？

● 类型.class，例如：String.class;

● 对象.getClass()，例如：”hello”.getClass();

● Class.forName()，例如：Class.forName(“java.lang.String”);

# Java GC面试题及答案（1~5题）

##### 1、既然有GC机制，为什么还会有内存泄露的情况?

理论上Java因为有垃圾回收机制（GC）不会存在内存泄露问题（这也是Java被广泛使用于服务器端编程的一个重要原因）。然而在实际开发中，可能会存在无用但可达的对象，这些对象不能被GC回收，因此也会导致内存泄露的发生。

● 例如hibernate的Session（一级缓存）中的对象属于持久态，垃圾回收器是不会回收这些对象的，然而这些对象中可能存在无用的垃圾对象，如果不及时关闭（close）或清空（flush）一级缓存就可能导致内存泄露。

下面例子中的代码也会导致内存泄露。

```java
import java.util.Arrays;
import java.util.EmptyStackException;
public class MyStack<T> {
    private T[] elements;
    private int size = 0;
    private static final int INIT_CAPACITY = 16;
    public MyStack() {
        elements = (T[]) new Object[INIT_CAPACITY];
    }
    public void push(T elem) {
        ensureCapacity();
        elements[size++] = elem;
    }
    public T pop() {
        if (size == 0) throw new EmptyStackException();
        return elements[--size];
    }

    private void ensureCapacity() {
        if (elements.length == size) {
            elements = Arrays.copyOf(elements,2 * size + 1);
        }
    }
}
```

上面的代码实现了一个栈（先进后出（FILO））结构，乍看之下似乎没有什么明显的问题，它甚至可以通过你编写的各种单元测试。然而其中的pop方法却存在内存泄露的问题，当我们用pop方法弹出栈中的对象时，该对象不会被当作垃圾回收，即使使用栈的程序不再引用这些对象，因为栈内部维护着对这些对象的过期引用（obsolete reference）。在支持垃圾回收的语言中，内存泄露是很隐蔽的，这种内存泄露其实就是无意识的对象保持。如果一个对象引用被无意识的保留起来了，那么垃圾回收器不会处理这个对象，也不会处理该对象引用的其他对象，即使这样的对象只有少数几个，也可能会导致很多的对象被排除在垃圾回收之外，从而对性能造成重大影响，极端情况下会引发Disk Paging（物理内存与硬盘的虚拟内存交换数据），甚至造成OutOfMemoryError。

##### 2、Java中为什么会有GC机制呢？

安全性考虑；--for security.

减少内存泄露；--erase memory leak in some degree.

减少程序员工作量。--Programmers don't worry about memory releasing.

##### 3、对于Java的GC哪些内存需要回收？

内存运行时JVM会有一个运行时数据区来管理内存。

● 它主要包括5大部分：

1.程序计数器(Program CounterRegister)；

2.虚拟机栈(VM Stack)；

3.本地方法栈(Native Method Stack)；

4.方法区(Method Area)；

5.堆(Heap)。

而其中程序计数器、虚拟机栈、本地方法栈是每个线程私有的内存空间，随线程而生，随线程而亡。例如栈中每一个栈帧中分配多少内存基本上在类结构确定是哪个时就已知了，因此这3个区域的内存分配和回收都是确定的，无需考虑内存回收的问题。

但方法区和堆就不同了，一个接口的多个实现类需要的内存可能不一样，我们只有在程序运行期间才会知道会创建哪些对象，这部分内存的分配和回收都是动态的，GC主要关注的是这部分内存。总而言之，GC主要进行回收的内存是JVM中的方法区和堆。

##### 4、Java的GC什么时候回收垃圾？

在面试中经常会碰到这样一个问题（事实上笔者也碰到过）：如何判断一个对象已经死去？

很容易想到的一个答案是：对一个对象添加引用计数器。每当有地方引用它时，计数器值加1；当引用失效时，计数器值减1.而当计数器的值为0时这个对象就不会再被使用，判断为已死。是不是简单又直观。然而，很遗憾。这种做法是错误的！为什么是错的呢？事实上，用引用计数法确实在大部分情况下是一个不错的解决方案，而在实际的应用中也有不少案例，但它却无法解决对象之间的循环引用问题。比如对象A中有一个字段指向了对象B，而对象B中也有一个字段指向了对象A，而事实上他们俩都不再使用，但计数器的值永远都不可能为0，也就不会被回收，然后就发生了内存泄露。

● 正确的做法应该是怎样呢？

在Java，C#等语言中，比较主流的判定一个对象已死的方法是：可达性分析(Reachability Analysis).所有生成的对象都是一个称为"GC Roots"的根的子树。从GC Roots开始向下搜索，搜索所经过的路径称为引用链(Reference Chain)，当一个对象到GC Roots没有任何引用链可以到达时，就称这个对象是不可达的（不可引用的），也就是可以被GC回收了。

● 无论是引用计数器还是可达性分析，判定对象是否存活都与引用有关！那么，如何定义对象的引用呢？

我们希望给出这样一类描述：当内存空间还够时，能够保存在内存中；如果进行了垃圾回收之后内存空间仍旧非常紧张，则可以抛弃这些对象。所以根据不同的需求，给出如下四种引用，根据引用类型的不同，GC回收时也会有不同的操作：

● 强引用(Strong Reference):Object obj=new Object();只要强引用还存在，GC永远不会回收掉被引用的对象。

● 软引用(Soft Reference)：描述一些还有用但非必需的对象。在系统将会发生内存溢出之前，会把这些对象列入回收范围进行二次回收（即系统将会发生内存溢出了，才会对他们进行回收）

● 弱引用(Weak Reference):程度比软引用还要弱一些。这些对象只能生存到下次GC之前。当GC工作时，无论内存是否足够都会将其回收（即只要进行GC，就会对他们进行回收。）

● 虚引用(Phantom Reference):一个对象是否存在虚引用，完全不会对其生存时间构成影响。关于方法区中需要回收的是一些废弃的常量和无用的类。

1.废弃的常量的回收。这里看引用计数就可以了。没有对象引用该常量就可以放心的回收了。

2.无用的类的回收。什么是无用的类呢？

A.该类所有的实例都已经被回收。也就是Java堆中不存在该类的任何实例；

B加载该类的ClassLoader已经被回收；

C.该类对应的java.lang.Class对象没有任何地方被引用，无法在任何地方通过反射访问该类的方法。总而言之:对于堆中的对象，主要用可达性分析判断一个对象是否还存在引用，如果该对象没有任何引用就应该被回收。而根据我们实际对引用的不同需求，又分成了4种引用，每种引用的回收机制也是不同的。对于方法区中的常量和类，当一个常量没有任何对象引用它，它就可以被回收了。而对于类，如果可以判定它为无用类，就可以被回收了。

##### 5、通过10个示例来初步认识Java8中的lambda表达式

● 用lambda表达式实现Runnable

// Java 8 之前：

```java
new Thread(new Runnable(){
    @Override
    public void run(){
        System.out.println("Before Java8， too much code for too little to do");
    }}).start();
    //Java 8 方式：
    new Thread(()->System.out.println("In Java8， Lambda expression rocks !!")).start();
```

输出：

too much code，for too little to do

Lambda expression rocks!!

这个例子向我们展示了Java 8 lambda表达式的语法。你可以使用lambda写出如下代码：

```java
(params) -> expression (params) -> statement
(params) -> { statements }
```

例如，如果你的方法不对参数进行修改、重写，只是在控制台打印点东西的话，那么可以这样写：

```java
() -> System.out.println("Hello Lambda Expressions");
```

如果你的方法接收两个参数，那么可以写成如下这样：

```java
(int even， int odd) -> even + odd
```

顺便提一句，通常都会把lambda表达式内部变量的名字起得短一些。这样能使代码更简短，放在同一行。所以，在上述代码中，变量名选用a、b或者x、y会比even、odd要好。

● 使用Java 8 lambda表达式进行事件处理

如果你用过Swing API编程，你就会记得怎样写事件监听代码。这又是一个旧版本简单匿名类的经典用例，但现在可以不这样了。你可以用lambda表达式写出更好的事件监听代码，如下所示：

```java
// Java 8 之前：
JButton show = new JButton("Show"); show.addActionListener(new ActionListener() {
    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println("Event handling without lambda expression is boring");
    }
});
// Java 8 方式：
show.addActionListener((e) -> {
    System.out.println("Light， Camera， Action !! Lambda expressions Rocks");
});
```

● 使用Java 8 lambda表达式进行事件处理 使用lambda表达式对列表进行迭代

如果你使过几年Java，你就知道针对集合类，最常见的操作就是进行迭代，并将业务逻辑应用于各个元素，例如处理订单、交易和事件的列表。由于Java是命令式语言，Java 8之前的所有循环代码都是顺序的，即可以对其元素进行并行化处理。如果你想做并行过滤，就需要自己写代码，这并不是那么容易。通过引入lambda表达式和默认方法，将做什么和怎么做的问题分开了，这意味着Java集合现在知道怎样做迭代，并可以在API层面对集合元素进行并行处理。下面的例子里，我将介绍如何在使用lambda或不使用lambda表达式的情况下迭代列表。你可以看到列表现在有了一个forEach()方法，它可以迭代所有对象，并将你的lambda代码应用在其中。

```java
// Java 8 之前：
List features = Arrays.asList("Lambdas"， "Default Method"， "Stream API"，"Date and Time API");
for (String feature : features) {
    System.out.println(feature);
}
// Java 8 之后：
List features = Arrays.asList("Lambdas"， "Default Method"， "Stream API"，"Date and Time API");
features.forEach(n -> System.out.println(n));
// 使用 Java 8 的方法引用更方便，方法引用由::双冒号操作符标示，
// 看起来像 C++的作用域解析运算符
features.forEach(System.out::println);
```

输出：

Lambdas Default Method Stream API

Date and Time API

列表循环的最后一个例子展示了如何在Java 8中使用方法引用（method reference）。你可以看到C++里面的双冒号、范围解析操作符现在在Java 8中用来表示方法引用。

● 使用lambda表达式和函数式接口Predicate

除了在语言层面支持函数式编程风格，Java 8也添加了一个包，叫做java.util.function。它包含了很多类，用来支持Java的函数式编程。其中一个便是Predicate，使用java.util.function.Predicate函数式接口以及lambda表达式，可以向API方法添加逻辑，用更少的代码支持更多的动态行为。下面是Java 8 Predicate的例子，展示了过滤集合数据的多种常用方法。Predicate接口非常适用于做过滤。

```java
public static void main(String[]args){
    List languages=Arrays.asList("Java", "Scala","C++", "Haskell", "Lisp");
    System.out.println("Languages which starts with J :");
    filter(languages, (str)->str.startsWith("J"));
    System.out.println("Languages which ends with a ");
    filter(languages, (str)->str.endsWith("a"));
    System.out.println("Print all languages :");
    filter(languages, (str)->true);
    System.out.println("Print no language : ");
    filter(languages, (str)->false);
    System.out.println("Print language whose length greater than 4:");
    filter(languages, (str)->str.length()>4);
}
public static void filter(List names, Predicate condition){
    for(String name:names){
        if(condition.test(name)){
            System.out.println(name+" ");
        }
    }
}
// filter 更好的办法--filter 方法改进
public static void filter(List names, Predicate condition) {
    names.stream().filter((name)->(condition.test(name))).forEach((name)->
	{System.out.println(name + " ");
    });
}
```

可以看到，Stream API的过滤方法也接受一个Predicate，这意味着可以将我们定制的filter()方法替换成写在里面的内联代码，这就是lambda表达式的魔力。另外，Predicate接口也允许进行多重条件的测试。

# Java GC面试题及答案（第5题）

● 如何在lambda表达式中加入Predicate

上个例子说到，java.util.function.Predicate允许将两个或更多的Predicate合成一个。它提供类似于逻辑操作符AND和OR的方法，名字叫做and()、or()和xor()，用于将传入filter()方法的条件合并起来。例如，要得到所有以J开始，长度为四个字母的语言，可以定义两个独立的Predicate示例分别表示每一个条件，然后用Predicate.and()方法将它们合并起来。

如下所示：

```java
// 甚至可以用 and()、or()和 xor()逻辑函数来合并 Predicate，
// 例如要找到所有以 J 开始，长度为四个字母的名字，你可以合并两个 Predicate 并传入
Predicate<String> startsWithJ = (n) -> n.startsWith("J");
Predicate<String> fourLetterLong = (n) -> n.length() == 4; names.stream().filter(startsWithJ.and(fourLetterLong)).forEach((n) ->System.out.print("nName， which starts with 'J' and four letter long is:"+n));
```

类似地，也可以使用or()和xor()方法。本例着重介绍了如下要点：可按需要将Predicate作为单独条件然后将其合并起来使用。简而言之，你可以以传统Java命令方式使用Predicate接口，也可以充分利用lambda表达式达到事半功倍的效果。

● Java 8中使用lambda表达式的Map和Reduce示例

本例介绍最广为人知的函数式编程概念map。它允许你将对象进行转换。例如在本例中，我们将costBeforeTax列表的每个元素转换成为税后的值。我们将x->x*x lambda表达式传到map()方法，后者将其应用到流中的每一个元素。然后用forEach()将列表元素打印出来。使用流API的收集器类，可以得到所有含税的开销。有toList()这样的方法将map或任何其他操作的结果合并起来。由于收集器在流上做终端操作，因此之后便不能重用流了。你甚至可以用流API的reduce()方法将所有数字合成一个，下一个例子将会讲到。

```java
/ 不使用 lambda 表达式为每个订单加上 12%的税
List costBeforeTax = Arrays.asList(100， 200， 300， 400， 500);
for(Integer cost :costBeforeTax){
    double price = cost + .12 * cost;
    System.out.println(price);
}
// 使用 lambda 表达式
List costBeforeTax = Arrays.asList(100， 200， 300， 400， 500);
costBeforeTax.stream().map((cost) ->cost +.12*cost).forEach(System.out::println);
```

在上面例子中，可以看到 map 将集合类（例如列表）元素进行转换的。还有一个 reduce() 函数可以将所有值合并成一个。Map 和 Reduce 操作是函数式编程的核心操作，因为其功能，reduce 又被称为折叠操作。另外，reduce 并不是一个新的操作，你有可能已经在使用它。SQL 中类似 sum()、avg() 或者 count() 的聚集函数，实际上就是reduce 操作，因为它们接收多个值并返回一个值。流 API 定义的 reduceh() 函数可以接受 lambda 表达式，并对所有值进行合并。IntStream 这样的类有类似 average()、count()、sum() 的内建方法来做 reduce 操作，也有mapToLong()、mapToDouble() 方法来做转换。这并不会限制你，你可以用内建方法，也可以自己定义。在这个 Java 8 的 Map Reduce 示例里，我们首先对所有价格应用 12% 的 VAT，然后用 reduce() 方法计算总和。

```java
// 为每个订单加上 12%的税
// 老方法：
List costBeforeTax = Arrays.asList(100， 200， 300， 400， 500);
double total = 0;
for(Integer cost :costBeforeTax){
    double price = cost + .12 * cost;
    total = total + price;
}
System.out.println("Total : "+total);
// 新方法：
List costBeforeTax = Arrays.asList(100， 200， 300， 400， 500);
double bill = costBeforeTax.stream().map((cost) -> cost + .12 * cost).reduce((sum， cost) ->sum +cost).get();
System.out.println("Total : "+bill);
```

● 通过过滤创建一个String列表

过滤是Java开发者在大规模集合上的一个常用操作，而现在使用lambda表达式和流API过滤大规模数据集合是惊人的简单。流提供了一个filter()方法，接受一个Predicate对象，即可以传入一个lambda表达式作为过滤逻辑。下面的例子是用lambda表达式过滤Java集合，将帮助理解。

```java
/ 创建一个字符串列表，每个字符串长度大于 2
List costBeforeTax = Arrays.asList("abc"， "bcd"， "defg"， "jk");
List<String>filtered=strList.stream().filter(x->x.length() >2).collect(Collectors.toList());
System.out.printf("Original List : %s， filtered list : %s %n"，strList，filtered);
```

输出：

Original List:[abc，，bcd，，defg，jk]，filtered list:[abc，bcd，defg]

另外，关于filter()方法有个常见误解。在现实生活中，做过滤的时候，通常会丢弃部分，但使用filter()方法则是获得一个新的列表，且其每个元素符合过滤原则。

● 对列表的每个元素应用函数

我们通常需要对列表的每个元素使用某个函数，例如逐一乘以某个数、除以某个数或者做其它操作。这些操作都很适合用map()方法，可以将转换逻辑以lambda表达式的形式放在map()方法里，就可以对集合的各个元素进行转换了，如下所示。

```java
// 将字符串换成大写并用逗号链接起来
List<String> G7 = Arrays.asList("USA", "Japan", "France", "Germany", "Italy","U.K.", "Canada");
String G7Countries=G7.stream().map(x->x.toUpperCase()).collect(Collectors.joining(","));
System.out.println(G7Countries);
```

输出：

USA， JAPAN， FRANCE， GERMANY， ITALY， U.K.， CANADA

● 复制不同的值，创建一个子列表

本例展示了如何利用流的distinct()方法来对集合进行去重

```java
///用所有不同的数字创建一个正方形列表
List<Integer> numbers = Arrays.asList(9, 10, 3, 4, 7, 3, 4);
List<Integer> distinct = numbers.stream().map( i -> i*i).distinct().collect(Collectors.toList());
System.out.print("Original List : %s, Square Without duplicates : %s %n", numbers, distinct);
```

输出：

Original List:[9，10，3，4，7，3，4]，Square Without duplicates:[81，100，9，16，49]

● 计算集合元素的最大值、最小值、总和以及平均值

IntStream、LongStream和DoubleStream等流的类中，有个非常有用的方法叫做summaryStatistics()。可以返回IntSummaryStatistics、LongSummaryStatistics或者DoubleSummaryStatistic s，描述流中元素的各种摘要数据。在本例中，我们用这个方法来计算列表的最大值和最小值。它也有getSum()和getAverage()方法来获得列表的所有元素的总和及平均值。

```java
/获取数字的个数、最小值、最大值、总和以及平均值
List<Integer> primes = Arrays.asList(2, 3, 5, 7, 11, 13, 17, 19, 23, 29);
IntSummaryStatistics stats = primes.stream().mapToInt((x) -> x).summaryStatistics();
System.out.println("Highest prime number in List : "+stats.getMax());
System.out.println("Lowest prime number in List : "+stats.getMin());
System.out.println("Sum of all prime numbers : "+stats.getSum());
System.out.println("Average of all prime numbers : "+stats.getAverage());
```

输出：

Highest prime number in List:29

Lowest prime number in List:2

Sum of all prime numbers:129

Average of all prime numbers:12.9

Java 8的10个lambda表达式，这对于新手来说是个合适的任务量，你可能需要亲自运行示例程序以便掌握。试着修改要求创建自己的例子，达到快速学习的目的。

补充：从例子中我们可以可以看到，以前写的匿名内部类都用了lambda表达式代替了。

● 那么，我们简单谈谈“lambda表达式&匿名内部类”

● 两者不同：

1.关键字this

(1)匿名内部类中的this代表匿名类

(2)Lambda表达式中的this代表lambda表达式的类

2.编译方式不同

(1)匿名内部类中会编译成一个.class文件，文件命名方式为：主类+$+(1，2，3.......)

(2)Java编译器将lambda表达式编译成类的私有方法。使用了Java 7的invokedynamic字节码指令来动态绑定这个方法

**● Java8中的lambda表达式6要点**

要点1：lambda表达式的使用位置，预定义使用了@Functional注释的函数式接口，自带一个抽象函数的方法，或者SAM（Single Abstract Method单个抽象方法）类型。这些称为lambda表达式的目标类型，可以用作返回类型，或lambda目标代码的参数。例如，若一个方法接收Runnable、Comparable或者Callable接口，都有单个抽象方法，可以传入lambda表达式。类似的，如果一个方法接受声明于java.util.function包内的接口，例如Predicate、Function、Consumer或Supplier，那么可以向其传lambda表达式。

要点2：lambda表达式和方法引用，lambda表达式内可以使用方法引用，仅当该方法不修改lambda表达式提供的参数。本例中的lambda表达式可以换为方法引用，因为这仅是一个参数相同的简单方法调用。

```java
list.forEach(n -> System.out.println(n));
list.forEach(System.out::println); // 使用方法引用
```

然而，若对参数有任何修改，则不能使用方法引用，而需键入完整地 lambda 表达式，如下所示：

```java
list.forEach((String s) -> System.out.println("*" + s + "*"));
```

事实上，可以省略这里的 lambda 参数的类型声明，编译器可以从列表的类属性推测出来。

要点 3：lambda 表达式内部引用资源，lambda 内部可以使用静态、非静态和局部变量，这称为 lambda 内的变量捕获。

要点 4：lambda 表达式也成闭包，Lambda 表达式在 Java 中又称为闭包或匿名函数，所以如果有同事把它叫闭包的时候，不用惊讶。

要点 5：lambda 表达式的编译方式，Lambda 方法在编译器内部被翻译成私有方法，并派发  invokedynamic  字节码指令来进行调用。可以使用 JDK中的  javap  工具来反编译 class 文件。使用  javap -p  或  javap -c -v  命令来看一看 lambda 表达式生成的字节码。

```java
private static java.lang.Object lambda$0(java.lang.String);
```

要点 6：lambda 表达式的限制，lambda 表达式有个限制，那就是只能引用 final 或 final  局部变量，这就是说不能在 lambda 内部修改定义在域外的变量。

```java
List<Integer> primes = Arrays.asList(new Integer[]{2, 3,5,7});
int factor = 2;
primes.forEach(element -> { factor++; });
```

Error：

Compile time error:"local variables referenced from a lambda expression must be final or effectively final"

另外，只是访问它而不作修改是可以的，如下所示：

```java
List<Integer> primes = Arrays.asList(new Integer[]{2, 3,5,7});
int factor = 2;
primes.forEach(element -> { System.out.println(factor*element); });
```

输出：

4

6

10

14

因此，它看起来更像不可变闭包，类似于 Python。

# Java GC面试题及答案（第5题）

● Java8 中的 Optional 类的解析

身为一名Java程序员，大家可能都有这样的经历：调用一个方法得到了返回值却不能直接将返回值作为参数去调用别的方法。我们首先要判断这个返回值是否为null，只有在非空的前提下才能将其作为其他方法的参数。这正是一些类似Guava的外部API试图解决的问题。一些JVM编程语言比如Scala、Ceylon等已经将对在核心API中解决了这个问题。在之前一篇文章中，介绍了Scala是如何解决了这个问题。

新版本的Java，比如Java 8引入了一个新的Optional类。Optional类的Javadoc描述如下：

这是一个可以为null的容器对象。如果值存在则isPresent()方法会返回true，调用get()方法会返回该对象。

下面会逐个探讨Optional类包含的方法，并通过一两个示例展示如何使用。

方法1：Optional.of()

作用：为非null的值创建一个Optional。

说明：of方法通过工厂方法创建Optional类。需要注意的是，创建对象时传入的参数不能为null。如果传入参数为null，则抛出NullPointerException。

```java
/调用工厂方法创建 Optional 实例
Optional<String> name = Optional.of("Sanaulla");
//传入参数为 null,抛出 NullPointerException.
Optional<String> someNull = Optional.of(null);
```

方法2：Optional.ofNullable()

作用：为指定的值创建一个Optional，如果指定的值为null，则返回一个空的Optional。说明：ofNullable与of方法相似，唯一的区别是可以接受参数为null的情况。

```java
//下面创建了一个不包含任何值的 Optional 实例
//例如,值为'null'
Optional empty = Optional.ofNullable(null);
```

方法3：Optional.isPresent()

作用：判断预期值是否存在

说明：如果值存在返回true，否则返回false。

```java
//isPresent 方法用来检查 Optional 实例中是否包含值
Optional<String> name = Optional.of("Sanaulla");
if (name.isPresent()) {
    //在 Optional 实例内调用 get()返回已存在的值
    System.out.println(name.get());//输出 Sanaulla
}
```

方法4：Optional.get()

作用：如果Optional有值则将其返回，否则抛出NoSuchElementException。

说明：上面的示例中，get方法用来得到Optional实例中的值。下面我们看一个抛出NoSuchElementException的例子

```java
//执行下面的代码会输出：No value present
try {
    Optional empty = Optional.ofNullable(null);
    //在空的 Optional 实例上调用 get(),抛出 NoSuchElementException System.out.println(empty.get());
} catch (NoSuchElementException ex) {
    System.out.println(ex.getMessage());
}
```

方法5：Optional.ifPresent()

作用：如果Optional实例有值则为其调用consumer，否则不做处理

说明：要理解ifPresent方法，首先需要了解Consumer类。简答地说，Consumer类包含一个抽象方法。该抽象方法对传入的值进行处理，但没有返回值。Java8支持不用接口直接通过lambda表达式传入参数，如果Optional实例有值，调用ifPresent()可以接受接口段或lambda表达式

```java
//ifPresent 方法接受 lambda 表达式作为参数。
//lambda 表达式对 Optional 的值调用 consumer 进行处理。
Optional<String> name = Optional.of("Sanaulla");
name.ifPresent((value) -> {
    System.out.println("The length of the value is: " + value.length());
});
```

方法6：Optional.orElse()

作用：如果有值则将其返回，否则返回指定的其它值。

说明：如果Optional实例有值则将其返回，否则返回orElse方法传入的参数。示例如下：

```java
Optional<String> name = Optional.of("Sanaulla");
Optional<String> someNull = Optional.of(null);
//如果值不为 null,orElse 方法返回 Optional 实例的值。
//如果为 null,返回传入的消息。
//输出：There is no value present!
System.out.println(empty.orElse("There is no value present!"));
//输出：Sanaulla
System.out.println(name.orElse("There is some value!"));
```

方法7：Optional.orElseGet()

作用：如果有值则将其返回，否则返回指定的其它值。

说明：orElseGet与orElse方法类似，区别在于得到的默认值。orElse方法将传入的字符串作为默认值，orElseGet方法可以接受Supplier接口的实现用来生成默认值

```java
Optional<String> name = Optional.of("Sanaulla");
Optional<String> someNull = Optional.of(null);
//orElseGet 与 orElse 方法类似,区别在于 orElse 传入的是默认值,
//orElseGet 可以接受一个 lambda 表达式生成默认值。
//输出：Default Value
System.out.println(empty.orElseGet(() -> "Default Value"));
//输出：Sanaulla
System.out.println(name.orElseGet(() -> "Default Value"));
```

方法8：Optional.orElseThrow()

作用：如果有值则将其返回，否则抛出supplier接口创建的异常。

说明：在orElseGet方法中，我们传入一个Supplier接口。然而，在orElseThrow中我们可以传入一个lambda表达式或方法，如果值不存在来抛出异常

```java
try {
    Optional<String> empty = Optional.of(null);
    //orElseThrow 与 orElse 方法类似。与返回默认值不同,
    //orElseThrow 会抛出 lambda 表达式或方法生成的异常
    empty.orElseThrow(ValueAbsentException::new);
} catch (Throwable ex) {
//输出: No value present in the Optional instance System.out.println(ex.getMessage());
}
```

ValueAbsentException定义如下：

```java
class ValueAbsentException extends Throwable {
    public ValueAbsentException() {
        super();
    }
    public ValueAbsentException(String msg) {
        super(msg);
    }
    @Override
    public String getMessage() {
        return "No value present in the Optional instance";
    }
}
```

方法9：Optional.map()

作用：如果有值，则对其执行调用mapping函数得到返回值。如果返回值不为null，则创建包含mapping返回值的Optional作为map方法返回值，否则返回空Optional。

说明：map方法用来对Optional实例的值执行一系列操作。通过一组实现了Function接口的lambda表达式传入操作。

```java
Optional<String> name = Optional.of("Sanaulla");
//map 方法执行传入的 lambda 表达式参数对 Optional 实例的值进行修改。
//为 lambda 表达式的返回值创建新的 Optional 实例作为 map 方法的返回值。
Optional<String> upperName = name.map((value) -> value.toUpperCase());
System.out.println(upperName.orElse("No value found"));
```

方法10：Optional.flatMap()

作用：如果有值，为其执行mapping函数返回Optional类型返回值，否则返回空Optional。flatMap与map（Funtion）方法类似，区别在于flatMap中的mapper返回值必须是Optional。调用结束时，flatMap不会对结果用Optional封装。

说明：flatMap方法与map方法类似，区别在于mapping函数的返回值不同。map方法的mapping函数返回值可以是任何类型T，而flatMap方法的mapping函数必须是Optional。

```java
Optional<String> name = Optional.of("Sanaulla");
//flatMap 与 map（Function）非常类似,区别在于传入方法的 lambda 表达式的返回类型。
//map 方法中的 lambda 表达式返回值可以是任意类型,在 map 函数返回之前会包装为 Optional。
//但 flatMap 方法中的 lambda 表达式返回值必须是 Optionl 实例。
upperName = name.flatMap((value) -> Optional.of(value.toUpperCase()));
System.out.println(upperName.orElse("No value found"));//输出 SANAULLA
```

方法11：Optional.filter()

作用：如果有值并且满足断言条件返回包含该值的Optional，否则返回空Optional。

说明：filter个方法通过传入限定条件对Optional实例的值进行过滤。这里可以传入一个lambda表达式。对于filter函数我们应该传入实现了Predicate接口的lambda表达式。

```java
Optional<String> name=Optional.of("Sanaulla");
//filter 方法检查给定的 Option 值是否满足某些条件。
//如果满足则返回同一个 Option 实例,否则返回空 Optional。
Optional<String> longName=name.filter((value)->value.length()>6);
System.out.println(longName.orElse("The name is less than 6 characters"));
//输出 Sanaulla
```

另一个例子是Optional值不满足filter指定的条件。

```java
Optional<String> anotherName = Optional.of("Sana");
Optional<String> shortName = anotherName.filter((value) -> value.length() > 6);
//输出：name 长度不足 6 字符
System.out.println(shortName.orElse("The name is less than 6 characters"));
```

总结：Optional方法

以上，我们介绍了Optional类的各个方法。下面通过一个完整的示例对用法集中展示。

```java
public class OptionalDemo {
    public static void main(String[] args) {
        //创建 Optional 实例,也可以通过方法返回值得到。
        Optional<String> name = Optional.of("Sanaulla");
        //创建没有值的 Optional 实例,例如值为'null' Optional empty = Optional.ofNullable(null);
        //isPresent 方法用来检查 Optional 实例是否有值。
        if (name.isPresent()) {
            //调用 get()返回 Optional 值。
            System.out.println(name.get());
        }
        try {
            //在 Optional 实例上调用 get()抛出 NoSuchElementException。System.out.println(empty.get());
        } catch (NoSuchElementException ex) {
            System.out.println(ex.getMessage());
        }
        //ifPresent 方法接受 lambda 表达式参数。
        //如果 Optional 值不为空,lambda 表达式会处理并在其上执行操作。
        name.ifPresent((value) -> {
            System.out.println("The length of the value is: " +
                    value.length());
        });
        //如果有值 orElse 方法会返回 Optional 实例,否则返回传入的错误信息。
        System.out.println(empty.orElse("There is no value present!"));
        System.out.println(name.orElse("There is some value!"));
        //orElseGet 与 orElse 类似,区别在于传入的默认值。
        //orElseGet 接受 lambda 表达式生成默认值。
        System.out.println(empty.orElseGet(() -> "Default Value"));
        System.out.println(name.orElseGet(() -> "Default Value"));
        try {
            //orElseThrow 与 orElse 方法类似,区别在于返回值。
            //orElseThrow 抛出由传入的 lambda 表达式/方法生成异常。
            empty.orElseThrow(ValueAbsentException::new);
        } catch (Throwable ex) {
            System.out.println(ex.getMessage());
        }
        //map 方法通过传入的 lambda 表达式修改 Optonal 实例默认值。
        //lambda 表达式返回值会包装为 Optional 实例。
        Optional<String> upperName = name.map((value) -> value.toUpperCase());
        System.out.println(upperName.orElse("No value found"));
        //flatMap 与 map（Funtion）非常相似,区别在于 lambda 表达式的返回值。
        //map 方法的 lambda 表达式返回值可以是任何类型,但是返回值会包装成 Optional 实例。
        //但是 flatMap 方法的 lambda 返回值总是 Optional 类型。
        upperName = name.flatMap((value) -> Optional.of(value.toUpperCase()));
        System.out.println(upperName.orElse("No value found"));
        //filter 方法检查 Optiona 值是否满足给定条件。
        //如果满足返回 Optional 实例值,否则返回空 Optional。
        Optional<String> longName = name.filter((value) -> value.length()
                > 6);
        System.out.println(longName.orElse("The name is less than 6 characters"));
        //另一个示例,Optional 值不满足给定条件。
        Optional<String> anotherName = Optional.of("Sana");
        Optional<String> shortName = anotherName.filter((value) -> value.length() > 6);
        System.out.println(shortName.orElse("The name is less than 6 characters"));
    }
}
```

##### Java内存溢出面试题

引起内存溢出的原因有很多种，常见的有以下几种：

● 内存中加载的数据量过于庞大，如一次从数据库取出过多数据；

● 集合类中有对对象的引用，使用完后未清空，使得JVM不能回收；

● 代码中存在死循环或循环产生过多重复的对象实体；

● 使用的第三方软件中的BUG；

● 启动参数内存值设定的过小；

##### 内存溢出的解决方案：

● 第一步，修改JVM启动参数，直接增加内存。(-Xms，-Xmx参数一定不要忘记加。)

● 第二步，检查错误日志，查看“OutOfMemory”错误前是否有其它异常或错误。

● 第三步，对代码进行走查和分析，找出可能发生内存溢出的位置。重点排查以下几点：

1.检查对数据库查询中，是否有一次获得全部数据的查询。一般来说，如果一次取十万条记录到内存，就可能引起内存溢出。这个问题比较隐蔽，在上线前，数据库中数据较少，不容易出问题，上线后，数据库中数据多了，一次查询就有可能引起内存溢出。因此对于数据库查询尽量采用分页的方式查询。

2.检查代码中是否有死循环或递归调用。

3.检查是否有大循环重复产生新对象实体。

4.检查对数据库查询中，是否有一次获得全部数据的查询。一般来说，如果一次取十万条记录到内存，就可能引起内存溢出。这个问题比较隐蔽，在上线前，数据库中数据较少，不容易出问题，上线后，数据库中数据多了，一次查询就有可能引起内存溢出。因此对于数据库查询尽量采用分页的方式查询。

5.检查List、MAP等集合对象是否有使用完后，未清除的问题。List、MAP等集合对象会始终存有对对象的引用，使得这些对象不能被GC回收。

● 第四步，使用内存查看工具动态查看内存使用情况。

##### Java内存模型面试题

在并发编程中，多个线程之间采取什么机制进行通信（信息交换），什么机制进行数据的同步？

在Java语言中，采用的是共享内存模型来实现多线程之间的信息交换和数据同步的。

线程之间通过共享程序公共的状态，通过读-写内存中公共状态的方式来进行隐式的通信。同步指的是程序在控制多个线程之间执行程序的相对顺序的机制，在共享内存模型中，同步是显式的，程序员必须显式指定某个方法/代码块需要在多线程之间互斥执行。

在说Java内存模型之前，我们先说一下**Java的内存结构，也就是运行时的数据区域：**

Java虚拟机在执行Java程序的过程中，会把它管理的内存划分为几个不同的数据区域，这些区域都有各自的用途、创建时间、销毁时间。

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20191101/1572576811@6d2ec4681e8828b043dc1561c5aff319.png)

Java运行时数据区分为下面几个内存区域：

***\*PC寄存器/程序计数器：\****

严格来说是一个数据结构，用于保存当前正在执行的程序的内存地址，由于Java是支持多线程执行的，所以程序执行的轨迹不可能一直都是线性执行。当有多个线程交叉执行时，被中断的线程的程序当前执行到哪条内存地址必然要保存下来，以便用于被中断的线程恢复执行时再按照被中断时的指令地址继续执行下去。为了线程切换后能恢复到正确的执行位置，每个线程都需要有一个独立的程序计数器，各个线程之间计数器互不影响，独立存储，我们称这类内存区域为“线程私有”的内存,这在某种程度上有点类似于“ThreadLocal”，是线程安全的。

##### Java栈 Java Stack：

Java栈总是与线程关联在一起的，每当创建一个线程，JVM就会为该线程创建对应的Java栈，在这个Java栈中又会包含多个栈帧(Stack Frame)，这些栈帧是与每个方法关联起来的，每运行一个方法就创建一个栈帧，每个栈帧会含有一些局部变量、操作栈和方法返回值等信息。每当一个方法执行完成时，该栈帧就会弹出栈帧的元素作为这个方法的返回值，并且清除这个栈帧，Java栈的栈顶的栈帧就是当前正在执行的活动栈，也就是当前正在执行的方法，PC寄存器也会指向该地址。只有这个活动的栈帧的本地变量可以被操作栈使用，当在这个栈帧中调用另外一个方法时，与之对应的一个新的栈帧被创建，这个新创建的栈帧被放到Java栈的栈顶，变为当前的活动栈。同样现在只有这个栈的本地变量才能被使用，当这个栈帧中所有指令都完成时，这个栈帧被移除Java栈，刚才的那个栈帧变为活动栈帧，前面栈帧的返回值变为这个栈帧的操作栈的一个操作数。

由于Java栈是与线程对应起来的，Java栈数据不是线程共有的，所以不需要关心其数据一致性，也不会存在同步锁的问题。

在Java虚拟机规范中，对这个区域规定了两种异常状况：如果线程请求的栈深度大于虚拟机所允许的深度，将抛出StackOverflowError异常；如果虚拟机可以动态扩展，如果扩展时无法申请到足够的内存，就会抛出OutOfMemoryError异常。在Hot Spot虚拟机中，可以使用-Xss参数来设置栈的大小。栈的大小直接决定了函数调用的可达深度。

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20191101/1572576996@37e4029f0abae4cfd9d1c8b396009648.png)

堆 Heap:

堆是JVM所管理的内存中国最大的一块，是被所有Java线程锁共享的，不是线程安全的，在JVM启动时创建。堆是存储Java对象的地方，这一点Java虚拟机规范中描述是：所有的对象实例以及数组都要在堆上分配。Java堆是GC管理的主要区域，从内存回收的角度来看，由于现在GC基本都采用分代收集算法，所以Java堆还可以细分为：新生代和老年代；新生代再细致一点有Eden空间、From Survivor空间、To Survivor空间等。

##### 方法区Method Area:

方法区存放了要加载的类的信息（名称、修饰符等）、类中的静态常量、类中定义为final类型的常量、类中的Field信息、类中的方法信息，当在程序中通过Class对象的getName.isInterface等方法来获取信息时，这些数据都来源于方法区。方法区是被Java线程锁共享的，不像Java堆中其他部分一样会频繁被GC回收，它存储的信息相对比较稳定，在一定条件下会被GC，当方法区要使用的内存超过其允许的大小时，会抛出OutOfMemory的错误信息。方法区也是堆中的一部分，就是我们通常所说的Java堆中的永久区 Permanet Generation，大小可以通过参数来设置,可以通过-XX:PermSize指定初始值，-XX:MaxPermSize指定最大值。

##### 常量池Constant Pool:

常量池本身是方法区中的一个数据结构。常量池中存储了如字符串、final变量值、类名和方法名常量。常量池在编译期间就被确定，并保存在已编译的.class文件中。一般分为两类：字面量和应用量。字面量就是字符串、final变量等。类名和方法名属于引用量。引用量最常见的是在调用方法的时候，根据方法名找到方法的引用，并以此定为到函数体进行函数代码的执行。引用量包含：类和接口的权限定名、字段的名称和描述符，方法的名称和描述符。

##### 本地方法栈Native Method Stack:

本地方法栈和Java栈所发挥的作用非常相似，区别不过是Java栈为JVM执行Java方法服务，而本地方法栈为JVM执行Native方法服务。本地方法栈也会抛出StackOverflowError和OutOfMemoryError异常。

##### 主内存和工作内存：

Java内存模型的主要目标是定义程序中各个变量的访问规则，即在JVM中将变量存储到内存和从内存中取出变量这样的底层细节。此处的变量与Java编程里面的变量有所不同步，它包含了实例字段、静态字段和构成数组对象的元素，但不包含局部变量和方法参数，因为后者是线程私有的，不会共享，当然不存在数据竞争问题（如果局部变量是一个reference引用类型，它引用的对象在Java堆中可被各个线程共享，但是reference引用本身在Java栈的局部变量表中，是线程私有的）。为了获得较高的执行效能，Java内存模型并没有限制执行引起使用处理器的特定寄存器或者缓存来和主内存进行交互，也没有限制即时编译器进行调整代码执行顺序这类优化措施。

JMM规定了所有的变量都存储在主内存（Main Memory）中。每个线程还有自己的工作内存（Working Memory）,线程的工作内存中保存了该线程使用到的变量的主内存的副本拷贝，线程对变量的所有操作（读取、赋值等）都必须在工作内存中进行，而不能直接读写主内存中的变量（volatile变量仍然有工作内存的拷贝，但是由于它特殊的操作顺序性规定，所以看起来如同直接在主内存中读写访问一般）。不同的线程之间也无法直接访问对方工作内存中的变量，线程之间值的传递都需要通过主内存来完成。

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20191101/1572577037@759e744d6702950e57a5c046349f5534.png)

线程1和线程2要想进行数据的交换一般要经历下面的步骤：

1.线程1把工作内存1中的更新过的共享变量刷新到主内存中去。

2.线程2到主内存中去读取线程1刷新过的共享变量，然后copy一份到工作内存2中去。

Java内存模型是围绕着并发编程中原子性、可见性、有序性这三个特征来建立的，那我们依次看一下这三个特征：

***\*原子性（Atomicity）\****：一个操作不能被打断，要么全部执行完毕，要么不执行。在这点上有点类似于事务操作，要么全部执行成功，要么回退到执行该操作之前的状态。

基本类型数据的访问大都是原子操作，long 和double类型的变量是64位，但是在32位JVM中，32位的JVM会将64位数据的读写操作分为2次32位的读写操作来进行，这就导致了long、double类型的变量在32位虚拟机中是非原子操作，数据有可能会被破坏，也就意味着多个线程在并发访问的时候是线程非安全的。

下面我们来演示这个32位JVM下，对64位long类型的数据的访问的问题：

```java
public class NotAtomicity {
    //静态变量t
    public  static long t = 0;
    //静态变量t的get方法
    public  static long getT() {
        return t;
    }
    //静态变量t的set方法
    public  static void setT(long t) {
        NotAtomicity.t = t;
    }
    //改变变量t的线程
    public static class ChangeT implements Runnable{
        private long to;
        public ChangeT(long to) {
            this.to = to;
        }
        public void run() {
            //不断的将long变量设值到 t中
            while (true) {
                NotAtomicity.setT(to);
                //将当前线程的执行时间片段让出去，以便由线程调度机制重新决定哪个线程可以执行
                Thread.yield();
            }
        }
    }
    //读取变量t的线程，若读取的值和设置的值不一致，说明变量t的数据被破坏了，即线程不安全
    public static class ReadT implements Runnable{

        public void run() {
            //不断的读取NotAtomicity的t的值
            while (true) {
                long tmp = NotAtomicity.getT();
                //比较是否是自己设值的其中一个
                if (tmp != 100L && tmp != 200L && tmp != -300L && tmp != -400L) {
                    //程序若执行到这里，说明long类型变量t，其数据已经被破坏了
                    System.out.println(tmp);
                }
                ////将当前线程的执行时间片段让出去，以便由线程调度机制重新决定哪个线程可以执行
                Thread.yield();
            }
        }
    }
    public static void main(String[] args) {
        new Thread(new ChangeT(100L)).start();
        new Thread(new ChangeT(200L)).start();
        new Thread(new ChangeT(-300L)).start();
        new Thread(new ChangeT(-400L)).start();
        new Thread(new ReadT()).start();
    }
}
```

我们创建了4个线程来对long类型的变量t进行赋值，赋值分别为100,200，-300，-400，有一个线程负责读取变量t,如果正常的话，读取到的t的值应该是我们赋值中的一个，但是在32的JVM中，事情会出乎预料。如果程序正常的话，我们控制台不会有任何的输出，可实际上，程序一运行，控制台就输出了下面的信息：

-4294967096
4294966896
-4294967096
-4294967096
4294966896

之所以会出现上面的情况，是因为在32位JVM中，64位的long数据的读和写都不是原子操作，即不具有原子性，并发的时候相互干扰了。

32位的JVM中，要想保证对long、double类型数据的操作的原子性，可以对访问该数据的方法进行同步，就像下面的：

```
public class Atomicity {
    //静态变量t
    public  static long t = 0;
    //静态变量t的get方法,同步方法
    public synchronized static long getT() {
        return t;
    }
    //静态变量t的set方法，同步方法
    public synchronized static void setT(long t) {
        Atomicity.t = t;
    }
    //改变变量t的线程
    public static class ChangeT implements Runnable{
        private long to;
        public ChangeT(long to) {
            this.to = to;
        }
        public void run() {
            //不断的将long变量设值到 t中
            while (true) {
                Atomicity.setT(to);
                //将当前线程的执行时间片段让出去，以便由线程调度机制重新决定哪个线程可以执行
                Thread.yield();
            }
        }
    }
    //读取变量t的线程，若读取的值和设置的值不一致，说明变量t的数据被破坏了，即线程不安全
    public static class ReadT implements Runnable{

        public void run() {
            //不断的读取NotAtomicity的t的值
            while (true) {
                long tmp = Atomicity.getT();
                //比较是否是自己设值的其中一个
                if (tmp != 100L && tmp != 200L && tmp != -300L && tmp != -400L) {
                    //程序若执行到这里，说明long类型变量t，其数据已经被破坏了
                    System.out.println(tmp);
                }
                ////将当前线程的执行时间片段让出去，以便由线程调度机制重新决定哪个线程可以执行
                Thread.yield();
            }
        }
    }
    public static void main(String[] args) {
        new Thread(new ChangeT(100L)).start();
        new Thread(new ChangeT(200L)).start();
        new Thread(new ChangeT(-300L)).start();
        new Thread(new ChangeT(-400L)).start();
        new Thread(new ReadT()).start();
    }
}
```

这样做的话，可以保证对64位数据操作的原子性。

***\*可见性：\****一个线程对共享变量做了修改之后，其他的线程立即能够看到（感知到）该变量这种修改（变化）。

Java内存模型是通过将在工作内存中的变量修改后的值同步到主内存，在读取变量前从主内存刷新最新值到工作内存中，这种依赖主内存的方式来实现可见性的。

无论是普通变量还是volatile变量都是如此，区别在于：volatile的特殊规则保证了volatile变量值修改后的新值立刻同步到主内存，每次使用volatile变量前立即从主内存中刷新，因此volatile保证了多线程之间的操作变量的可见性，而普通变量则不能保证这一点。

除了volatile关键字能实现可见性之外，还有synchronized,Lock，final也是可以的。

使用synchronized关键字，在同步方法/同步块开始时（Monitor Enter）,使用共享变量时会从主内存中刷新变量值到工作内存中（即从主内存中读取最新值到线程私有的工作内存中），在同步方法/同步块结束时(Monitor Exit),会将工作内存中的变量值同步到主内存中去（即将线程私有的工作内存中的值写入到主内存进行同步）。

使用Lock接口的最常用的实现ReentrantLock(重入锁)来实现可见性：当我们在方法的开始位置执行lock.lock()方法，这和synchronized开始位置（Monitor Enter）有相同的语义，即使用共享变量时会从主内存中刷新变量值到工作内存中（即从主内存中读取最新值到线程私有的工作内存中），在方法的最后finally块里执行lock.unlock()方法，和synchronized结束位置（Monitor Exit）有相同的语义,即会将工作内存中的变量值同步到主内存中去（即将线程私有的工作内存中的值写入到主内存进行同步）。

final关键字的可见性是指：被final修饰的变量，在构造函数数一旦初始化完成，并且在构造函数中并没有把“this”的引用传递出去（“this”引用逃逸是很危险的，其他的线程很可能通过该引用访问到只“初始化一半”的对象），那么其他线程就可以看到final变量的值。

***\*有序性：\****对于一个线程的代码而言，我们总是以为代码的执行是从前往后的，依次执行的。这么说不能说完全不对，在单线程程序里，确实会这样执行；但是在多线程并发时，程序的执行就有可能出现乱序。用一句话可以总结为：在本线程内观察，操作都是有序的；如果在一个线程中观察另外一个线程，所有的操作都是无序的。前半句是指“线程内表现为串行语义（WithIn Thread As-if-Serial Semantics）”,后半句是指“指令重排”现象和“工作内存和主内存同步延迟”现象。

Java提供了两个关键字volatile和synchronized来保证多线程之间操作的有序性,volatile关键字本身通过加入内存屏障来禁止指令的重排序，而synchronized关键字通过一个变量在同一时间只允许有一个线程对其进行加锁的规则来实现，

在单线程程序中，不会发生“指令重排”和“工作内存和主内存同步延迟”现象，只在多线程程序中出现。

## happens-before原则：

Java内存模型中定义的两项操作之间的次序关系，如果说操作A先行发生于操作B，操作A产生的影响能被操作B观察到，“影响”包含了修改了内存中共享变量的值、发送了消息、调用了方法等。

下面是Java内存模型下一些”天然的“happens-before关系，这些happens-before关系无须任何同步器协助就已经存在，可以在编码中直接使用。如果两个操作之间的关系不在此列，并且无法从下列规则推导出来的话，它们就没有顺序性保障，虚拟机可以对它们进行随意地重排序。

a.程序次序规则(Pragram Order Rule)：在一个线程内，按照程序代码顺序，书写在前面的操作先行发生于书写在后面的操作。准确地说应该是控制流顺序而不是程序代码顺序，因为要考虑分支、循环结构。

b.管程锁定规则(Monitor Lock Rule)：一个unlock操作先行发生于后面对同一个锁的lock操作。这里必须强调的是同一个锁，而”后面“是指时间上的先后顺序。

c.volatile变量规则(Volatile Variable Rule)：对一个volatile变量的写操作先行发生于后面对这个变量的读取操作，这里的”后面“同样指时间上的先后顺序。

d.线程启动规则(Thread Start Rule)：Thread对象的start()方法先行发生于此线程的每一个动作。

e.线程终于规则(Thread Termination Rule)：线程中的所有操作都先行发生于对此线程的终止检测，我们可以通过Thread.join()方法结束，Thread.isAlive()的返回值等作段检测到线程已经终止执行。

f.线程中断规则(Thread Interruption Rule)：对线程interrupt()方法的调用先行发生于被中断线程的代码检测到中断事件的发生，可以通过Thread.interrupted()方法检测是否有中断发生。

g.对象终结规则(Finalizer Rule)：一个对象初始化完成(构造方法执行完成)先行发生于它的finalize()方法的开始。

h.传递性(Transitivity)：如果操作A先行发生于操作B，操作B先行发生于操作C，那就可以得出操作A先行发生于操作C的结论。

一个操作”时间上的先发生“不代表这个操作会是”先行发生“，那如果一个操作”先行发生“是否就能推导出这个操作必定是”时间上的先发生 “呢？也是不成立的，一个典型的例子就是指令重排序。所以时间上的先后顺序与happens-before原则之间基本没有什么关系，所以衡量并发安全问题一切必须以happens-before 原则为准。

# Java多线程笔试题

说明：答案要变成自己的话去描述，不要死记硬背。

## 多线程专题

**● 在实际开发中用过多线程吗，具体怎么用的，解决什么问题？**

我们通常在后台执行定时任务的时候会用到多线程，例如我们在P2P项目中给用户回款的时候，数据量比较大，收益需要在指定的时间之前全部返还，不然客户这边投诉的电话就来了，当时我们使用了JUC包下的Excutor线程池，启动多线程跑数据。这样问题就解决了。

**● 你对java的内存模型有了解吗？**

面试官问这个问题的时候，同学们要注意了，面试官要听的不是“JVM的内存结构”，而是Java的内存模型，简称JMM（Java Memory Model），参见另一个文件：Java内存模型JMM.docx

**● 线程之间怎么通信？**

另请参见：Java线程间的通信.docx

**● 什么是线程安全，怎么理解的？**

如果你的代码在多线程下执行和在单线程下执行永远都能获得一样的结果，那么你的代码就是线程安全的。

导致线程不安全的原因是：多线程同时对某个共享的数据进行修改操作，此时就会存在数据不一致问题。在Java中线程安全也是有几个级别的：

不可变

像String，其中String字符串在JVM中有字符串常量池的存在，字符串对象一旦创建不可变，由于这些对象在多线程并发的情况下不会被修改，所以不存在线程安全问题。

绝对线程安全

不管运行时环境如何，调用者都不需要额外的同步措施。要做到这一点通常需要付出许多额外的代价，Java中标注自己是线程安全的类，实际上绝大多数都不是线程安全的，不过绝对线程安全的类，Java中也有，比方说CopyOnWriteArrayList、CopyOnWriteArraySet。

相对线程安全

相对线程安全也就是我们通常意义上所说的线程安全，像Vector这种，add、remove方法都是原子操作，不会被打断，但也仅限于此，如果有个线程在遍历某个Vector、有个线程同时在add这个Vector，99%的情况下都会出现ConcurrentModificationException，也就是 fail-fast机制。

非线程安全

这个就没什么好说的了，ArrayList、LinkedList、HashMap等都是线程非安全的类。

**● 线程之间如何共享数据？**

（1）多个线程对共享数据的操作是相同的，那么创建一个Runnable的子类对象，将这个对象作为参数传递给Thread的构造方法，此时因为多个线程操作的是同一个Runnable的子类对象，所以他们操作的是同一个共享数据。比如：买票系统，所以的线程的操作都是对票数减一的操作。

（2）多个线程对共享数据的操作是不同的，将共享数据和操作共享数据的方法放在同一对象中，将这个对象作为参数传递给Runnable的子类，在子类中用该对象的方法对共享数据进行操作。比如实现生产者和消费者模型。

（3）多个线程对共享数据的操作是不同的， 用内部类的方式去实现，创建Runnable的子类作为内部类，将共享对象作为全局变量，在Runnable的子类中对共享数据进行操作。

**● 线程的start()和run()方法的区别？**

start()方法表示启动一个新的线程，在JVM内存中会开启一个新的栈空间。而run()方法只是普通方法调用，不会启动新的线程。

**● 实现线程的方式分别是什么？**

第一种：继承Thread

第二种：实现Runnable接口，这种方式使用较多，面向接口编程一直是被推崇的开发原则。

第三种：实现Callable接口用来实现返回结果的线程

**● 怎么获取线程的返回值？**

使用ExecutorService、Callable、Future实现有返回结果的线程。可返回值的任务必须实现Callable接口。执行Callable任务后，可以获取一个Future的对象，在该对象上调用get就可以获取到Callable任务返回的Object了。get方法是阻塞的，即：线程无返回结果，get方法会一直等待。再结合线程池接口ExecutorService就可以实现传说中有返回结果的多线程了。

**● volatile关键字的作用是什么？**

参见《java并发编程之volatile关键字解析.docx》

**● CyclicBarrier（篱栅）和CountDownLatch（倒计时锁存器）的区别是什么？**

都在java.util.concurrent下，都可以用来表示代码运行到某个点上，二者的区别在于：

（1）CyclicBarrier的某个线程运行到某个点上之后，该线程即停止运行，直到所有的线程都到达了这个点，所有线程才重新运行；

CountDownLatch则不是，某线程运行到某个点上之后，只是给某个数值-1而已，该线程继续运行。

（2）CyclicBarrier只能唤起一个任务，CountDownLatch可以唤起多个任务。

（3）CyclicBarrier可重用，CountDownLatch不可重用，计数值为0该CountDownLatch就不可再用了。

**● volatile（不稳定的）和synchronized（同步的）对比？**

读以下内容之前，参见：《java并发编程之volatile关键字解析.docx》
一旦一个共享变量（类的成员变量、类的静态成员变量）被volatile修饰之后，那么就具备了两层语义：

第一：保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。

第二：禁止进行指令重排序。

volatile本质是在告诉jvm当前变量在寄存器（工作内存）中的值是不确定的，需要从主存中读取；

（1）synchronized则是锁定当前变量，只有当前线程可以访问该变量，其他线程被阻塞住。

（2）volatile仅能使用在变量级别；synchronized则可以使用在变量、方法、和类级别的。

（3）volatile仅能实现变量的修改可见性，并不能保证原子性；synchronized则可以保证变量的修改可见性和原子性。

（4）volatile不会造成线程的阻塞；synchronized可能会造成线程的阻塞。

（5）volatile标记的变量不会被编译器优化；synchronized标记的变量可以被编译器优化。

**● 怎么唤醒一个阻塞的线程？**

如果线程是因为调用了wait()、sleep()或者join()方法而导致的阻塞，可以中断线程，并且通过抛出InterruptedException来唤醒它；如果线程遇到了IO阻塞，无能为力，因为IO是操作系统实现的，Java代码并没有办法直接接触到操作系统。

**● Java中如何获取到线程dump文件？**

当程序遇到死循环、死锁、阻塞、页面打开慢等问题，查看dump信息是最好的解决问题的途径。线程dump也就是线程堆栈信息。
获取到线程堆栈dump文件内容分两步：

（1）第一步：获取到线程的pid，Linux环境下可以使用ps -ef | grep java
（2）第二步：打印线程堆栈，可以通过使用jstack pid命令
具体实现步骤，参照：https://blog.csdn.net/u010271462/article/details/70171553

**● sleep方法和wait方法的相同点和不同点？**

相同点**：**

二者都可以让线程处于冻结状态。

不同点**：**

（1）首先应该明确sleep方法是Thread类中定义的方法，而wait方法是Object类中定义的方法。

（2）sleep方法必须人为地为其指定时间。wait方法既可以指定时间，也可以不指定时间。

（3）sleep方法时间到，线程处于临时阻塞状态或者运行状态。wait方法如果没有被设置时间，就必须要通过notify或者notifyAll来唤醒。

（4）sleep方法不一定非要定义在同步中。wait方法必须定义在同步中。

（5）当二者都定义在同步中时，线程执行到sleep，不会释放锁。线程执行到wait，会释放锁。

**● 生产者和消费者模型的作用是什么？**

（1）通过平衡生产者的生产能力和消费者的消费能力来提升整个系统的运行效率，这是生产者消费者模型最重要的作用

（2）解耦，这是生产者消费者模型附带的作用，解耦意味着生产者和消费者之间的联系少，联系越少越可以独自发展而不需要受到相互的制约。

**● ThreadLocal有什么作用？**

ThreadLocal用来解决多线程程序的并发问题。将数据放到ThreadLocal当中可以将该数据和当前线程绑定在一起，这样可以保证一个线程对应一个对象，这样对象就是线程安全的。

**● wait方法和notify/notifyAll方法在放弃对象监视器时有什么区别？**

wait()方法立即释放对象监视器；

notify()/notifyAll()方法则会等待线程剩余代码执行完毕才会放弃对象监视器。

**● Lock和synchronized对比？**

（1）Lock是一个接口，而synchronized是Java中的关键字，synchronized是内置的语言实现；

（2）synchronized在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生；而Lock在发生异常时，如果没有主动通过unLock()去释放锁，则很可能造成死锁现象，因此使用Lock时需要在finally块中释放锁；

（3）Lock可以让等待锁的线程响应中断，而synchronized却不行，使用synchronized时，等待的线程会一直等待下去，不能够响应中断；

（4）通过Lock可以知道有没有成功获取锁，而synchronized却无法办到。

（5）Lock可以提高多个线程进行读操作的效率。

（6）在JDK1.5中，synchronized是性能低效的。因为这是一个重量级操作，它对性能最大的影响是阻塞式的实现，挂起线程和恢复线程的操作都需要转入内核态中完成，这些操作给系统的并发性带来了很大的压力。相比之下使用Java提供的Lock对象，性能更高一些。但是，JDK1.6，发生了变化，对synchronize加入了很多优化措施，有自适应自旋，锁消除，锁粗化，轻量级锁，偏向锁等等。导致在JDK1.6上synchronize的性能并不比Lock差。因此。提倡优先考虑使用synchronized来进行同步。

**● ConcurrentHashMap的并发度是什么？**

ConcurrentHashMap的并发度就是segment的大小，默认为16，这意味着最多同时可以有16条线程操作ConcurrentHashMap，这也是ConcurrentHashMap对Hashtable的最大优势。

**● ReadWriteLock是什么？**

首先明确一下，不是说ReentrantLock不好，只是ReentrantLock某些时候有局限。如果使用ReentrantLock，可能本身是为了防止线程A在写数据、线程B在读数据造成的数据不一致，但这样，如果线程C在读数据、线程D也在读数据，读数据是不会改变数据的，没有必要加锁，但是还是加锁了，降低了程序的性能。因为这个，才诞生了读写锁ReadWriteLock。ReadWriteLock是一个读写锁接口，ReentrantReadWriteLock是ReadWriteLock接口的一个具体实现，实现了读写的分离， 读锁是共享的，写锁是独占的，读和读之间不会互斥，读和写、写和读、写和写之间才会互斥，提升了读写的性能。

**● 不可变对象对多线程有什么帮助**

不可变对象保证了对象的内存可见性，对不可变对象的读取不需要进行额外的同步手段，提升了代码执行效率。

**● FutureTask是什么？**

FutureTask表示一个异步运算的任务。FutureTask里面可以传入一个Callable的具体实现类，可以对这个异步运算的任务的结果进行等待获取、判断是否已经完成、取消任务等操作。当然，由于FutureTask也是Runnable接口的实现类，所以FutureTask也可以放入线程池中。

**● Java中用到的线程调度算法是什么？**

抢占式。一个线程用完CPU之后，操作系统会根据线程优先级、线程饥饿情况等数据算出一个总的优先级并分配下一个时间片给某个线程执行。

**● 怎么得到一个线程安全的单例模式？**

饿汉本来就是线程安全的，就不再多说了。懒汉单例本身就是非线程安全的，可以使用双检锁的方式实现线程安全的单例模式。双检锁就是volatile+synchronized实现。

**● 什么是乐观锁和悲观锁？**

**（1）乐观锁：**乐观锁(Optimistic Lock), 顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号等机制。乐观锁适用于多读的应用类型，这样可以提高吞吐量，像数据库如果提供类似于write_condition机制的其实都是提供的乐观锁。

**（2）悲观锁：**对于并发间操作产生的线程安全问题持悲观状态，悲观锁认为竞争总是会发生，因此每次对某资源进行操作时，都会持有一个独占的锁，就像synchronized，不管三七二十一，直接上了锁就操作资源了。

**● Java编写一个会导致死锁的程序？**

（1）两个线程里面分别持有两个Object对象：lock1和lock2。这两个lock作为同步代码块的锁；

（2）线程1的run()方法中同步代码块先获取lock1的对象锁，Thread.sleep(xxx)，时间不需要太多，50毫秒差不多了，然后接着获取lock2的对象锁。这么做主要是为了防止线程1启动一下子就连续获得了lock1和lock2两个对象的对象锁；

（3）线程2的run)(方法中同步代码块先获取lock2的对象锁，接着获取lock1的对象锁，当然这时lock1的对象锁已经被线程1锁持有，线程2肯定是要等待线程1释放lock1的对象锁的。

这样，线程1”睡觉”睡完，线程2已经获取了lock2的对象锁了，线程1此时尝试获取lock2的对象锁，便被阻塞，此时一个死锁就形成了。

**● 如何确保N个线程可以同时访问N个资源又不导致死锁？**

使用多线程的时候，一种非常简单的避免死锁的方式就是：指定获取锁的顺序，并强制线程按照指定的顺序获取锁。因此，如果所有的线程都是以同样的顺序加锁和释放锁，就不会出现死锁了。

**● Thread.sleep(0)的作用是什么?**

这个问题和上面那个问题是相关的，我就连在一起了。由于Java采用抢占式的线程调度算法，因此可能会出现某条线程常常获取到CPU控制权的情况，为了让某些优先级比较低的线程也能获取到CPU控制权，可以使用Thread.sleep(0)手动触发一次操作系统分配时间片的操作，这也是平衡CPU控制权的一种操作。

**● 高并发、任务执行时间短的业务怎样使用线程池？并发不高、任务执行时间长的业务怎样使用线程池？并发高、业务执行时间长的业务怎样使用线程池？**

（1）高并发、任务执行时间短的业务，线程池线程数可以设置为CPU核数+1，减少线程上下文的切换

（2）并发不高、任务执行时间长的业务要区分开看：
假如是业务时间长集中在IO操作上，也就是IO密集型的任务，因为IO操作并不占用CPU，所以不要让所有的CPU闲下来，可以加大线程池中的线程数目，让CPU处理更多的业务
假如是业务时间长集中在计算操作上，也就是计算密集型任务，这个就没办法了，和（1）一样吧，线程池中的线程数设置得少一些，减少线程上下文的切换

（3）并发高、业务执行时间长，解决这种类型任务的关键不在于线程池而在于整体架构的设计，看看这些业务里面某些数据是否能做缓存是第一步，增加服务器是第二步，至于线程池的设置，设置参考（2）。最后，业务执行时间长的问题，也可能需要分析一下，看看能不能使用中间件对任务进行拆分和解耦。

**● 同步方法和同步块，哪个是更好的选择，你怎么看？**

同步块，这意味着同步块之外的代码是异步执行的，这比同步整个方法更提升代码的效率。请知道一条原则：同步的范围越少越好。借着这一条，我额外提一点，虽说同步的范围越少越好，但是在Java虚拟机中还是存在着一种叫做 锁粗化 的优化方法，这种方法就是把同步范围变大。这是有用的，比方说StringBuffer，它是一个线程安全的类，自然最常用的append()方法是一个同步方法，我们写代码的时候会反复append字符串，这意味着要进行反复的加锁->解锁，这对性能不利，因为这意味着Java虚拟机在这条线程上要反复地在内核态和用户态之间进行切换，因此Java虚拟机会将多次append方法调用的代码进行一个锁粗化的操作，将多次的append的操作扩展到append方法的头尾，变成一个大的同步块，这样就减少了加锁–>解锁的次数，有效地提升了代码执行的效率。

**● 线程类的构造方法、静态块是被哪个线程调用的**

这是一个非常刁钻和狡猾的问题。请记住：线程类的构造方法、静态块是被new这个线程类所在的线程所调用的，而run方法里面的代码才是被线程自身所调用的。如果说上面的说法让你感到困惑，那么我举个例子， 假设Thread2中new了Thread1，main函数中new了Thread2，那么：

（1）Thread2的构造方法、静态块是main线程调用的，Thread2的run()方法是Thread2自己调用的

（2）Thread1的构造方法、静态块是Thread2调用的，Thread1的run()方法是Thread1自己调用的

**● Hashtable的size()方法中明明只有一条语句”return count”，为什么还要做同步？**

这是我之前的一个困惑，不知道大家有没有想过这个问题。某个方法中如果有多条语句，并且都在操作同一个类变量，那么在多线程环境下不加锁，势必会引发线程安全问题，这很好理解，但是size()方法明明只有一条语句，为什么还要加锁？

关于这个问题，在慢慢地工作、学习中，有了理解，主要原因有两点：
（1） 同一时间只能有一条线程执行固定类的同步方法，但是对于类的非同步方法，可以多条线程同时访问 。所以，这样就有问题了，可能线程A在执行Hashtable的put方法添加数据，线程B则可以正常调用size()方法读取Hashtable中当前元素的个数，那读取到的值可能不是最新的，可能线程A添加了完了数据，但是没有对size++，线程B就已经读取size了，那么对于线程B来说读取到的size一定是不准确的。而给size()方法加了同步之后，意味着线程B调用size()方法只有在线程A调用put方法完毕之后才可以调用，这样就保证了线程安全性

（2） CPU执行代码，执行的不是Java代码，这点很关键，一定得记住 。Java代码最终是被翻译成汇编代码执行的，汇编代码才是真正可以和硬件电路交互的代码。 即使你看到Java代码只有一行，甚至你看到Java代码编译之后生成的字节码也只有一行，也不意味着对于底层来说这句语句的操作只有一个 。一句”return count”假设被翻译成了三句汇编语句执行，完全可能执行完第一句，线程就切换了。

**● 多线程有什么用？**

**（1）发挥多核CPU的优势**

随着工业的进步，现在的笔记本、台式机乃至商用的应用服务器至少也都是双核的，4核、8核甚至16核的也都不少见，如果是单线程的程序，那么在双核CPU上就浪费了50%，在4核CPU上就浪费了75%。 单核CPU上所谓的”多线程”那是假的多线程，同一时间处理器只会处理一段逻辑，只不过线程之间切换得比较快，看着像多个线程”同时”运行罢了 。多核CPU上的多线程才是真正的多线程，它能让你的多段逻辑同时工作，多线程，可以真正发挥出多核CPU的优势来，达到充分利用CPU的目的。

**（2）防止阻塞**

从程序运行效率的角度来看，单核CPU不但不会发挥出多线程的优势，反而会因为在单核CPU上运行多线程导致线程上下文的切换，而降低程序整体的效率。但是单核CPU我们还是要应用多线程，就是为了防止阻塞。试想，如果单核CPU使用单线程，那么只要这个线程阻塞了，比方说远程读取某个数据吧，对端迟迟未返回又没有设置超时时间，那么你的整个程序在数据返回回来之前就停止运行了。多线程可以防止这个问题，多条线程同时运行，哪怕一条线程的代码执行读取数据阻塞，也不会影响其它任务的执行。

**● Semaphore有什么作用，信号量是什么？**

Semaphore就是一个信号量，它的作用是限制某段代码块的并发数。Semaphore有一个构造函数，可以传入一个int型整数n，表示某段代码最多只有n个线程可以访问，如果超出了n，那么请等待，等到某个线程执行完毕这段代码块，下一个线程再进入。由此可以看出如果Semaphore构造函数中传入的int型整数n=1，相当于变成了一个synchronized了。

**● 一个线程如果出现了运行时异常会怎么样？**

如果这个异常没有被捕获的话，这个线程就停止执行了。另外重要的一点是： 如果这个线程持有某个某个对象的监视器，那么这个对象监视器会被立即释放。

**● 为什么使用线程池？**

避免频繁地创建和销毁线程，达到线程对象的重用。另外，使用线程池还可以根据项目灵活地控制并发的数目。

**● synchronized和ReentrantLock的区别**

synchronized是和if、else、for、while一样的关键字，ReentrantLock是类，这是二者的本质区别。既然ReentrantLock是类，那么它就提供了比synchronized更多更灵活的特性，可以被继承、可以有方法、可以有各种各样的类变量，ReentrantLock比synchronized的扩展性体现在几点上：

（1）ReentrantLock可以对获取锁的等待时间进行设置，这样就避免了死锁

（2）ReentrantLock可以获取各种锁的信息

（3）ReentrantLock可以灵活地实现多路通知

另外，二者的锁机制其实也是不一样的。ReentrantLock底层调用的是Unsafe的park方法加锁，synchronized操作的是对象头中mark word。

**● 什么是多线程的上下文切换**

多线程的上下文切换是指CPU控制权由一个已经正在运行的线程切换到另外一个就绪并等待获取CPU执行权的线程的过程。

**● 如果你提交任务时，线程池队列已满，这时会发生什么**

如果你使用的LinkedBlockingQueue，也就是无界队列的话，没关系，继续添加任务到阻塞队列中等待执行，因为LinkedBlockingQueue可以近乎认为是一个无穷大的队列，可以无限存放任务；如果你使用的是有界队列比方说ArrayBlockingQueue的话，任务首先会被添加到ArrayBlockingQueue中，ArrayBlockingQueue满了，则会使用拒绝策略RejectedExecutionHandler处理满了的任务，默认是AbortPolicy。

**● 什么是CAS？**

CAS，全称为Compare and Set，即比较-设置。假设有三个操作数： 内存值V、旧的预期值A、要修改的值B，当且仅当预期值A和内存值V相同时，才会将内存值修改为B并返回true，否则什么都不做并返回false 。当然CAS一定要volatile变量配合，这样才能保证每次拿到的变量是主内存中最新的那个值，否则旧的预期值A对某条线程来说，永远是一个不会变的值A，只要某次CAS操作失败，永远都不可能成功。

**● 什么是AQS？**

简单说一下AQS，AQS全称为AbstractQueuedSychronizer，翻译过来应该是抽象队列同步器。

如果说java.util.concurrent的基础是CAS的话，那么AQS就是整个Java并发包的核心了，ReentrantLock、CountDownLatch、Semaphore等等都用到了它。AQS实际上以双向队列的形式连接所有的Entry，比方说ReentrantLock，所有等待的线程都被放在一个Entry中并连成双向队列，前面一个线程使用ReentrantLock好了，则双向队列实际上的第一个Entry开始运行。

AQS定义了对双向队列所有的操作，而只开放了tryLock和tryRelease方法给开发者使用，开发者可以根据自己的实现重写tryLock和tryRelease方法，以实现自己的并发功能。

**● notify和notifyAll的区别？**

notifyAll使所有原来在该对象上等待被唤醒的线程统统退出wait的状态，变成等待该对象上的锁，一旦该对象被解锁，他们就会去竞争。notify是通知其中一个线程，不会通知所有的线程。

**● 你是如何合理配置线程池大小的？**

首先，需要考虑到线程池所进行的工作的性质：IO密集型？CPU密集型？
简单的分析来看，如果是CPU密集型的任务，我们应该设置数目较小的线程数，比如CPU数目加1。如果是IO密集型的任务，则应该设置可能多的线程数，由于IO操作不占用CPU，所以，不能让CPU闲下来。当然，如果线程数目太多，那么线程切换所带来的开销又会对系统的响应时间带来影响。

在《linux多线程服务器端编程》中有一个思路，CPU计算和IO的阻抗匹配原则。如果线程池中的线程在执行任务时，密集计算所占的时间比重为P(0 下面验证一下边界条件的正确性：

假设C = 8，P = 1.0，线程池的任务完全是密集计算，那么T = 8。只要8个活动线程就能让8个CPU饱和，再多也没用了，因为CPU资源已经耗光了。

假设C = 8，P = 0.5，线程池的任务有一半是计算，有一半在等IO上，那么T = 16.考虑操作系统能灵活，合理调度sleeping/writing/running线程，那么大概16个“50%繁忙的线程”能让8个CPU忙个不停。启动更多的线程并不能提高吞吐量，反而因为增加上下文切换的开销而降低性能。

如果P < 0.2，这个公式就不适用了，T可以取一个固定值，比如 5*C。另外公式里的C不一定是CPU总数，可以是“分配给这项任务的CPU数目”，比如在8核机器上分出4个核来做一项任务，那么C=4
文章如何合理设置线程池大小里面提到了一个公式：

最佳线程数目 = （（线程等待时间+线程CPU时间）/线程CPU时间 ）* CPU数目

比如平均每个线程CPU运行时间为0.5s，而线程等待时间（非CPU运行时间，比如IO）为1.5s，CPU核心数为8，那么根据上面这个公式估算得到：((0.5+1.5)/0.5)*8=32。这个公式进一步转化为：

最佳线程数目 = （线程等待时间与线程CPU时间之比 + 1）* CPU数目
可以得出一个结论：

线程等待时间所占比例越高，需要越多线程。线程CPU时间所占比例越高，需要越少线程。

以上公式与之前的CPU和IO密集型任务设置线程数基本吻合。

**● 线程池具体配置参数？**

corePoolSize：核心线程数

queueCapacity：任务队列容量（阻塞队列）

maxPoolSize：最大线程数

keepAliveTime：线程空闲时间

allowCoreThreadTimeout：允许核心线程超时

rejectedExecutionHandler：任务拒绝处理器

**● 线程池按以下行为执行任务：**

（1）当线程数小于核心线程数时，创建线程。

（2）当线程数大于等于核心线程数，且任务队列未满时，将任务放入任务队列。

（3）当线程数大于等于核心线程数，且任务队列已满

若线程数小于最大线程数，创建线程

若线程数等于最大线程数，抛出异常，拒绝任务

# Java集合面试题

集合专题

**● 说一下你了解的Java集合？**

**● HashMap与Hashtable的区别？**

**● 哈希碰撞/哈希冲突你有了解吗，它是什么，怎么解决的？**

**● ArrayList和LinkedList的不同？**

**● ArrayList和Vector的区别？**

**● HashMap的数据存储和实现原理你了解多少？**

**● HashMap的长度为什么是2的幂次方？**

**● HashSet和HashMap的区别与联系？**

**● Hashtable和ConcurrentHashMap的区别？**

**● ConcurrentHashMap线程安全的具体实现原理？**

**● 怎么获取一个线程安全的ArrayList？**

**● TreeMap/TreeSet底层是什么数据结构，怎么实现的自动排序？**

**● HashMap集合的put和get方法的实现原理？**

**● Comparable和Comparator的区别？**

**● for循环和foreach遍历集合哪个更快?**

**● 如何快速的遍历map集合，哪种方式最快？**

**● HashSet、TreeSet、LinkedHashSet区别？**

**● 在什么场景下要重写equals()和hashCode()方法？**

**● HashMap和TreeMap有什么不同？**

**● Collections和Collection有哪些区别？**

**● 如何对一组对象进行排序？**

**● Collection接口的remove()方法和Iterator接口的remove()方法区别？**

**● Java集合中List、Set、Map的区别？**

**● Iterator和ListIterator的区别是什么？**

**● 快速失败(fail-fast)和安全失败(fail-safe)的区别是什么？**

**● 什么是Java优先级队列(Priority Queue)？**

PriorityQueue是一个基于优先级堆的无界队列，它的元素是按照自然顺序(natural order)排序的。在创建的时候，我们可以给它提供一个负责给元素排序的比较器。PriorityQueue不允许null值，因为他们没有自然顺序，或者说他们没有任何的相关联的比较器。最后，PriorityQueue不是线程安全的，入队和出队的时间复杂度是O(log(n))。

**● Enumeration接口和Iterator接口的区别有哪些？**

Enumeration速度是Iterator的2倍，同时占用更少的内存。但是，Iterator远远比Enumeration安全，因为其他线程不能够修改正在被Iterator遍历的集合里面的对象。同时，Iterator允许调用者删除底层集合里面的元素，这对Enumeration来说是不可能的。

**● 在迭代一个集合的时候，如何避免**ConcurrentModificationException？
在遍历一个集合的时候，我们可以使用并发集合类来避免ConcurrentModificationException，比如使用CopyOnWriteArrayList，而不是ArrayList。

**● 哪些集合类是线程安全的？**

Vector、Hashtable、Properties和Stack是同步类，所以它们是线程安全的，可以在多线程环境下使用。Java1.5并发包下包括一些集合类，允许迭代时修改，因为它们都工作在集的克隆上。所以它们在多线程环境中是安全的。

**● 并发集合类是什么？**

Java1.5并发包（java.util.concurrent）包含线程安全集合类，允许在迭代时修改集合。迭代器被设计为fail-fast的，会 抛出ConcurrentModificationException。一部分类为：CopyOnWriteArrayList、 ConcurrentHashMap、CopyOnWriteArraySet。

**● BlockingQueue是什么？**

java.util.concurrent.BlockingQueue是一个队列，在进行检索或移除一个元素的时候，它会等待队列变为非空；当在添加一个元素时，它会等待队列中的可用空间。BlockingQueue接口是Java集合框架的一部分，主要用于实现生产者-消费者模式。我们不需要担心等待生产者有可用的空间，或消费者有可用的对象，因为它都在BlockingQueue的实现类中被处理了。Java提供了集中BlockingQueue的实现，比如ArrayBlockingQueue、LinkedBlockingQueue、PriorityBlockingQueue、SynchronousQueue等。

**● 大写的O是什么，举几个例子？**

大写的O描述的是数据结构中一个算法的性能。Collection类就是实际的数据结构，我们通常基于时间、内存和性能，使用大写的O来选择集合实现。比如：例子1：ArrayList的get(index i)是一个常量时间操作，它不依赖list中元素的数量，所以它的性能是O(1)。例子2：一个对于数组或列表的线性搜索的性能是O(n)，因为我们需要 遍历所有的元素来查找需要的元素。

## 数据库专题/SQL优化专题

**● 你用过存储过程吗？**

之前用过存储过程，近期的项目没有使用存储过程，由于存储过程具有数据库特色，mysql有自己一套语法机制，oracle数据库则有另一套语法机制，虽然使用存储过程会提高执行速度，但使用了存储过程后，项目的数据库很难平滑的移植，例如项目A在开发的时候使用了mysql存储过程，假设将来将A项目的数据库更改为B项目则是很困难的，故我们开发的时候为了保证数据库的可移植性，就没有使用存储过程。

注意：游标、触发器、存储过程，都是存储过程相关的。

**● 千万级数据，你如何提高查询效率？**

**● 你对SQL优化有了解吗，可以描述一些你用过的优化策略吗？**

**● 索引的实现原理？**

**● 索引的分类？**

**● 索引在什么情况下失效？**

**● 内连接和外连接有什么区别？**

**● 数据库设计范式？**

第一范式：每个表都应该有主键，并且每个字段要求原子性不可再分。

第二范式：建立在第一范式基础之上，所有非主键字段必须完全依赖主键，不能产生部分依赖。

第三范式：建立在第二范式基础之上，所有非主键字段必须直接依赖主键，不能产生传递依赖。

设计范式的最终目的是：减少数据的冗余。但在实际的开发中，我们以满足客户的需求为目的，有的时候也会拿冗余来换取速度。（建议把这句话说上，体现工作经验）

**● MySQL默认的事务隔离级别？**

可重复读（Repeatable Read）

**● 事务的隔离级别有哪些？**

读未提交（Read UnCommitted）、读提交（Read Committed）、可重复读（Repeatable Read）、序列化（Serializable）

**● 在什么情况下使用having过滤？**

从效率方面考虑，建议优先使用where进行过滤，如果专门是对分组之后的数据进行过滤，才会使用having。

**● 事务的特性有哪些？**

原则性（A）、一致性（C）、隔离性（I）、持久性（D）。

原子性表示事务是最小的工作单元不可再分。

一致性表示事务必须同时成功或者同时失败。

隔离性表示事务A和事务B之间具有隔离。

持久性是事务最终结束的保障。

JVM专题

**● Java GC的工作原理你有了解吗？**

**● System.gc()和Runtime.gc()会做什么事情？**

这两个方法用来提示JVM要进行垃圾回收。但是，立即开始还是延迟进行垃圾回收是取决于JVM的。

**● 如果对象的引用被置为null，垃圾收集器是否会立即释放对象占用的内存？**
不会，在下一个垃圾回收周期中，这个对象将是可被回收的。

**● 你听说过哪些GC算法？**

**● 你知道哪些垃圾回收机制？**

**● 你对JVM调优有了解吗，可以说一下吗？**

**● JVM的内存结构你有了解吗（运行时数据区）？**

**● Java的堆结构是什么样子的？**

JVM的堆是运行时数据区，所有类的实例和数组都是在堆上分配内存。它在JVM启动的时候被创建。对象所占的堆内存是由自动内存管理系统也就是垃圾收集器回收。堆内存是由存活和死亡的对象组成的。存活的对象是应用可以访问的，不会被垃圾回收。死亡的对象是应用不可访问尚且还没有被垃圾收集器回收掉的对象。一直到垃圾收集器把这些对象回收掉之前，他们会一直占据堆内存空间。

**● 串行(serial)收集器和吞吐量(throughput)收集器的区别是什么？**

吞吐量收集器使用并行版本的新生代垃圾收集器，它用于中等规模和大规模数据的应用程序。而串行收集器对大多数的小应用(在现代处理器上需要大概100M左右的内存)就足够了。

**● JVM的永久代中会发生垃圾回收么？**

垃圾回收不会发生在永久代，如果永久代满了或者是超过了临界值，会触发完全垃圾回收(Full GC)。如果你仔细查看垃圾收集器的输出信息，就会发现永久代也是被回收的。这就是为什么正确的永久代大小对避免Full GC是非常重要的原因。

**● 什么是分布式垃圾回收(DGC)？它是如何工作的？**

DGC叫做分布式垃圾回收。RMI使用DGC来做自动垃圾回收。因为RMI包含了跨虚拟机的远程对象的引用，垃圾回收是很困难的。DGC使用引用计数算法来给远程对象提供自动内存管理。

## 设计模式专题

**● 你在实际项目中使用过哪些设计模式？**

在实际的开发中也用过很多设计模式，例如GoF的设计模式，JavaEE的设计模式，例如在项目中写过BaseController，所有的Controller都继承这个Controller，这个BaseController当中定义核心算法骨架，具体的实现延迟到子类中完成，这个就符合模板方法设计模式。

另外还使用了JavaEE设计模式DAO，持久层统一使用该模式。还有在分页查询的时候封装了PaginationVO，这里使用了JavaEE的VO模式。

在实际的项目中也使用了过滤器、拦截器等，这些都是符合GoF责任链设计模式的。还有使用Spring AOP控制事务，AOP底层使用了动态代理模式。

在P2P项目中，生成合同文件的时候，我们使用了两个设计模式，一个是门面模式，一个是策略模式。门面模式体现在我们提供了生成合同文件的统一接口，策略模式体现在合同文件可以最终生成到PC端，也可以生成到H5端等。

**● 你对设计模式有了解吗，什么是设计模式，常见设计模式你知道哪些？**

设计模式就是可以重复利用的解决方案。常见的设计模式有：单例模式、装饰器模式、适配器模式、责任链模式、代理模式、策略模式、观察者模式等。

**单例模式：**解决对象创建问题，属于创建型设计模式，保证创建的实例只有1个，用于节省内存的开销，符合单例模式的对象不会被垃圾回收器回收，所以在实际开发中通常使用单例模式来实现缓存，因为缓存需要避免被GC回收。

**装饰器模式：**IO流当中使用了大量的装饰器模式，装饰器模式属于结构型设计模式，例如关闭流的时候只需要关闭最外层流即可。

**适配器模式：**Servlet规范中的GenericServlet使用了适配器模式，适配器模式属于结构型设计模式，其中GenericServlet属于适配器，所有Servlet类继承GenericServlet，而不需要直接实现Servlet接口，这样代码会更加优雅（该词汇出自阎宏的《Java与模式》一书）。另外过滤器Filter、监听器Listener都符合适配器模式。

**责任链设计模式：**Filter过滤器符合责任链设计模式，包括SpringMVC中的拦截器也是符合该设计模式的，责任链设计模式属于行为型设计模式，再不改变java源代码的基础之上，可以实现方法的动态调用组合。

**代理模式：**Spring AOP就使用了动态代理机制。代理模式属于结构型设计模式。

**策略模式：**比较器Comparator就体现了策略模式，面向接口编程，更换比较器即可更换比较规则，策略模式属于行为型设计模式。

**观察者模式：**Servlet规范中的Listener监听器就实现了观察者模式，另外MVC架构模式也属于符合观察者设计模式，观察者设计模式属于行为型设计模式。

**● 你知道哪些开发原则？**

据我了解，软件的开发原则有六大原则。

开闭原则：开闭原则是面向对象的可复用设计的第一块基石，它是最重要的面向对象设计原则。开闭原则(Open-Closed Principle, OCP)：一个软件实体应当对扩展开放，对修改关闭。即软件实体应尽量在不修改原有代码的情况下进行扩展。

单一职责原则：单一职责原则是最简单的面向对象设计原则，它用于控制类的粒度大小。单一职责原则定义如下：单一职责原则(Single Responsibility Principle, SRP)：一个类只负责一个功能领域中的相应职责，或者可以定义为：就一个类而言，应该只有一个引起它变化的原因。单一职责原则是实现高内聚、低耦合的指导方针，它是最简单但又最难运用的原则，需要设计人员发现类的不同职责并将其分离，而发现类的多重职责需要设计人员具有较强的分析设计能力和相关实践经验。

接口隔离原则：接口隔离原则(Interface Segregation Principle, ISP)：使用多个专门的接口，而不使用单一的总接口，即客户端不应该依赖那些它不需要的接口。

里氏代换原则：里氏代换原则(Liskov Substitution Principle, LSP)：所有引用基类（父类）的地方必须能透明地使用其子类的对象。里氏代换原则告诉我们，在软件中将一个基类对象替换成它的子类对象，程序将不会产生任何错误和异常，反过来则不成立，如果一个软件实体使用的是一个子类对象的话，那么它不一定能够使用基类对象。例如：我喜欢动物，那我一定喜欢狗，因为狗是动物的子类；但是我喜欢狗，不能据此断定我喜欢动物，因为我并不喜欢老鼠，虽然它也是动物。

迪米特法则：一个软件实体应当尽可能少地与其他实体发生相互作用。迪米特法则还有几种定义形式，包括：不要和“陌生人”说话、只与你的直接朋友通信等。

依赖倒转原则：依赖倒转原则(Dependency Inversion Principle, DIP)：抽象不应该依赖于细节，细节应当依赖于抽象。换言之，要针对接口编程，而不是针对实现编程。依赖倒转原则要求我们在程序代码中传递参数时或在关联关系中，尽量引用层次高的抽象层类，即使用接口和抽象类进行变量类型声明、参数类型声明、方法返回类型声明，以及数据类型的转换等，而不要用具体类来做这些事情。为了确保该原则的应用，一个具体类应当只实现接口或抽象类中声明过的方法，而不要给出多余的方法，否则将无法调用到在子类中增加的新方法。

**● GoF设计模式分类？**

创建型、结构型、行为型。

创建型的有：单例模式、工厂模式等。

结构型的有：适配器、装饰器、代理模式等。

行为型的有：策略模式、观察者模式、责任链模式等。

**● JavaEE的设计模式知道那些？**

DAO、POJO、DTO、VO、BO等。

**● 怎么写一个线程安全的单例模式？**

```java
//饿汉（绝对线程安全，但是对象在未使用时初始化，占用内存）
public final class EagerSingleton {  
    private static EagerSingleton singObj = new EagerSingleton();  
    private EagerSingleton(){  
    }  
    public static EagerSingleton getSingleInstance(){  
       return singObj；
    }  
}
//懒汉（多线程并发时，存在线程安全问题）
public final class LazySingleton {  
    private static LazySingleton singObj = null;  
    private LazySingleton(){  
    }  
    public static LazySingleton getSingleInstance(){  
        if(null == singObj ) singObj = new LazySingleton();
          return singObj；
    }  
}
//懒汉加synchronized（线程安全的，但是同步的是整个方法，并发度差）
public final class ThreadSafeSingleton {  
    private static ThreadSafeSingleton singObj = null;  
    private ThreadSafeSingleton(){  
    }  
    public static Synchronized ThreadSafeSingleton getSingleInstance(){  
        if(null == singObj ) singObj = new ThreadSafeSingleton();
            return singObj；
    }  
}
//双重检查锁（如果发生指令重排序，则会出现线程不安全）
public final class DoubleCheckedSingleton {  
    private static DoubleCheckedSingletonsingObj = null;  
    private DoubleCheckedSingleton(){  
    }  
    public static DoubleCheckedSingleton getSingleInstance(){  
        if(null == singObj ) {
              Synchronized(DoubleCheckedSingleton.class){
                     if(null == singObj) // （如果发生指令重排序，则会出现线程不安全）
                           singObj = new DoubleCheckedSingleton();
              }
         }
       return singObj；
    }  
}
//双重检查锁（加volatile关键字）
public final class DoubleCheckedSingleton {  
    private static volatile DoubleCheckedSingletonsingObj = null;  
    private DoubleCheckedSingleton(){  
    }  
    public static DoubleCheckedSingleton getSingleInstance(){  
        if(null == singObj ) {
              Synchronized(DoubleCheckedSingleton.class){
                     if(null == singObj) 
                           singObj = new DoubleCheckedSingleton();
              }
         }
       return singObj；
    }  
}
// 静态内部类方式
public class SingletonPattern {
   private SingletonPattern() {
   }
   private static class SingletonPatternHolder {
       private static final SingletonPattern singletonPattern = new SingletonPattern();
   }
   public static SingletonPattern getInstance() {
       return SingletonPatternHolder.singletonPattern;
   }
}
```

**● 单例模式的优缺点及使用场景？**

**（1）优点：** 

在单例模式中，活动的单例只有一个实例，对单例类的所有实例化得到的都是相同的一个实例。这样就防止其它对象对自己的实例化，确保所有的对象都访问一个实例。 

提供了对唯一实例的受控访问。 

由于在系统内存中只存在一个对象，因此可以节约系统资源，当需要频繁创建和销毁的对象时单例模式无疑可以提高系统的性能。 

避免对共享资源的多重占用。

**（2）缺点：**

不适用于变化的对象，如果同一类型的对象总是要在不同的用例场景发生变化，单例就会引起数据的错误，不能保存彼此的状态。 

由于单利模式中没有抽象层，因此单例类的扩展有很大的困难。

单例类的职责过重，在一定程度上违背了“单一职责原则”。 

滥用单例将带来一些负面问题，如为了节省资源将数据库连接池对象设计为的单例类，可能会导致共享连接池对象的程序过多而出现连接池溢出；如果实例化的对象长时间不被利用，系统会认为是垃圾而被回收，这将导致对象状态的丢失。

**（3）使用注意事项：**

使用时不能用反射模式创建单例，否则会实例化一个新的对象。

使用懒单例模式时注意线程安全问题。

饿单例模式和懒单例模式构造方法都是私有的，因而是不能被继承的，有些单例模式可以被继承（如登记式模式）。

**（4）经典使用场景：**

网站的计数器，一般也是采用单例模式实现，否则难以同步。 

应用程序的日志应用，一般都何用单例模式实现，这一般是由于共享的日志文件一直处于打开状态，因为只能有一个实例去操作，否则内容不好追加。 

Web应用的配置对象的读取，一般也应用单例模式，这个是由于配置文件是共享的资源。 

数据库连接池的设计一般也是采用单例模式，因为数据库连接是一种数据库资源。数据库软件系统中使用数据库连接池，主要是节省打开或者关闭数据库连接所引起的效率损耗，这种效率上的损耗还是非常昂贵的，因为何用单例模式来维护，就可以大大降低这种损耗。 

多线程的线程池的设计一般也是采用单例模式，这是由于线程池要方便对池中的线程进行控制。

**● 什么是享元模式，哪里用了？**

享元模式通过共享对象来避免创建太多的对象。为了使用享元模式，你需要确保你的对象是不可变的，这样你才能安全的共享。JDK 中 String 池、Integer 池以及 Long 池都是很好的使用了享元模式的例子。

**● 描述一下接口和抽象类你是如何选用的？**

接口和抽象类其实表示事物与事物之间的联系的一种关系的体现。接口更多的体现的是like A的关系，而抽象类更多的是is A的关系。如果这两个类他们之间确实无形中体现出is A的关系，比如猫和狗都是动物的一种，则可以写抽象类。而如果这两个类它们之间的行为很像，则它们体现出了一种Like A的关系，如媒婆代理别人去相亲，那么本身就体现了一种方法，则体现出了接口的关系。也就是说接口更偏向于描述行为。

# Java mybatis面试题及答案

MyBatis专题

**● 谈一谈你对ORM的理解？**

对象关系映射（Object Relational Mapping，简称ORM）是一种为了解决面向对象与关系数据库存在的互不匹配的现象的技术。 简单的说，ORM是通过使用描述对象和数据库之间映射的元数据，将java程序中的对象自动持久化到关系数据库中。当然反过来也是可以的，例如将数据库表当中的记录查询出来，然后映射为Java程序中的Java对象。

**● 在MyBatis中$和#的区别？**

$进行sql语句的拼接，#是专门给sql语句传值的。$就是JDBC当中的Statement，#就是JDBC当中的PreparedStatement。$的这种方式使用时需要特别注意sql注入问题。

**● 你对MyBatis的一级缓存和二级缓存有了解吗，说一下？**

Mybatis对缓存提供支持，但是在没有配置的默认情况下，它只开启一级缓存，一级缓存只是相对于同一个SqlSession而言。所以在参数SQL完全一样的情况下，我们使用同一个SqlSession对象调用一个Mapper方法，往往只执行一次SQL，因为使用SqlSession第一次查询后，MyBatis会将其放在缓存中，以后再查询的时候，如果没有声明需要刷新，并且缓存没有超时的情况下，SqlSession都会取出当前缓存的数据，而不会再次发送SQL到数据库。

MyBatis的二级缓存是Application级别的缓存，它可以提高对数据库查询的效率，以提高应用的性能。SqlSessionFactory层面上的二级缓存默认是不开启的，二级缓存的开启需要进行配置，实现二级缓存的时候，MyBatis要求返回的POJO必须是可序列化的。 也就是要求实现Serializable接口，配置方法很简单，只需要在映射XML文件配置就可以开启缓存了。

由于我们在实际的开发中目前都会使用第三方的缓存技术，例如Redis，所以MyBatis这块的二级缓存没有太多的了解。

**● MyBatis一对多你是怎么实现的？**

有联合查询和嵌套查询。联合查询是几个表联合查询,只查询一次,通过在resultMap里面配 置collection节点配置一对多的类就可以完成; 嵌套查询是先查一个表,根据这个表里面的结果的外键id再去另外一个表里面查询数据,也是通过配置collection,但另外一个表的查询通过select节点配置。

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20191101/1572583149@4df74dc492eddc7a36df104c792ff3dc.png)

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20191101/1572583174@cd817ec35ffaf9503886d05ba48cf745.png)

**● MyBatis的parameterType怎么理解的？**

parameterType属性用来指定参数类型，parameterType属性是专门用来给sql语句占位符#{}传值的，底层原理使用了反射机制，#{}的大括号当中需要提供实体类的属性名，底层使用属性名拼接get方法来获取属性值，将属性值传递给sql语句。

**● MyBatis的resultType是怎么理解的？**

resultType用来指定结果集封装的数据类型，当一个select语句查询之后得到结果集，结果集的列名需要和java实体类的属性名一致，不一致的可以使用as关键字给列起别名，拿着列名拼接set方法，通过反射机制调用set方法给结果集对象的属性赋值。

**● MyBatis中resultMap用过吗，它是干什么的？**

在MyBatis当中，查询结果集被封装为Java对象，可以通过resultType，也可以通过resultMap，在resultMap当中描述了数据库表的列与Java对象的属性之间的对应关系。在映射关系中，还可以通过resultMap的typeHandler设置实现查询结果值的类型转换。另外，最重要的是通过resultMap的子标签比如、等，可以实现一对一、一对多等的映射。

**● MyBatis底层实现原理？**

MyBatis是一个持久层框架，实现了ORM思想，可以将查询的结果集自动转换成Java对象，也可以将Java对象转换成一条数据插入到数据库表当中。

那么，查询结果集是如何自动转换成Java对象的呢？实际上这里使用了反射机制，在配置文件中假设编写了一条select语句，查询之后，列名与属性名要一一对应（不对应的可以采用给列起别名），然后每个列名前添加“set”，通过反射机制获取set方法，然后再通过反射机制的method.invoke()来调用这个set方法，给Java对象的属性赋值。这样就完成了对象的封装。

另外，Java对象是如何转换成一条记录插入到数据库的呢？假设在配置文件中编写了一条insert语句，那么这条语句需要的值从哪里来呢，在mybatis的mapper配置中有parameterType属性，该属性是专门给sql语句占位符传值的，其实这里也是使用了反射机制，其中sql语句的占位符采用#{}，其中大括号当中需要提供java对象的属性名，该属性名和get进行拼接得到get方法名，然后通过反射机制获取该get方法，再通过method.invoke()来调用这个get方法，这样就可以获取到对应的属性值，然后传入了。

其实MyBatis设计最牛的地方当然是采用JDK动态代理的方式生成DAO接口的实现类了。其中DAO接口中的每一个方法名对应sql语句的id。DAO接口中的方法不允许重载，因为id是不允许重复的。以上大概就是我了解的MyBatis实现原理。

**● 谈谈MyBatis和Hibernate的区别？**

Hibernate属于全自动ORM映射框架，使用Hibernate查询关联对象或者关联集合对象时，可以根据对象关系模型直接获取，所以它是全自动的。而Mybatis在查询关联对象或关联集合对象时，需要手动编写sql来完成，所以，称之为半自动ORM映射工具。

也正因为MyBatis的sql语句由程序员自己编写，所以sql更容易优化，这也是目前互联网公司使用MyBatis较多的重要原因。

**● MyBatis中除了之外你还用过哪些标签？**

还有很多其他的标签，、、、、，加上动态sql的9个标签，trim|where|set|foreach|if|choose|when|otherwise|bind等，其中为sql片段标签，通过标签引入sql片段，为不支持自增的主键生成策略标签。

**● 在MyBatis当中，通常一个Mapper映射文件，都会写一个Dao接口与之对应，请问，这个Dao接口的工作原理是什么？**

Dao接口的工作原理是JDK动态代理，Mybatis运行时会使用JDK动态代理为Dao接口生成代理proxy对象，代理对象proxy会拦截接口方法，转而执行MappedStatement所代表的sql，然后将sql执行结果返回。

**● MyBatis注解用过吗，都有哪些？**

到目前为止，我们在项目中还没有使用过MyBatis的注解，因为MyBatis最主要是编写sql语句，sql语句涉及到后期优化，可能会频繁修改，所以我们一直都在使用配置文件的形式。

**● Mybatis动态sql是做什么的？都有哪些动态sql？能简述一下动态sql的执行原理吗？**

Mybatis动态sql可以让我们在Xml映射文件内以标签的形式编写动态sql，完成逻辑判断和动态拼接sql的功能，Mybatis提供了9种动态sql标签trim|where|set|foreach|if|choose|when|otherwise|bind。其执行原理为，使用OGNL从sql参数对象中计算表达式的值，根据表达式的值动态拼接sql，以此来完成动态sql的功能。

**● Mybatis是如何将sql执行结果封装为目标对象并返回的？**

第一种是使用resultMap，逐一定义列名和对象属性名之间的映射关系。 第二种是使用resultType，使用sql列的别名功能，将列别名书写为对象属性名，比如T_NAME AS NAME，对象属性名一般是name，小写，但是列名不区分大小写，Mybatis会忽略列名大小写，智能找到与之对应对象属性名，你甚至可以写成T_NAME AS NaMe，Mybatis一样可以正常工作。 有了列名与属性名的映射关系后，Mybatis通过反射创建对象，同时使用反射给对象的属性逐一赋值并返回，那些找不到映射关系的属性，是无法完成赋值的。

**● MyBatis接口Mapper中的方法能够重载吗？**

不能。MyBatis使用package+Mapper+method全限名作为key，去xml内寻找唯一sql来执行的。类似：key=x.y.UserMapper.getUserById，那么，重载方法时将导致矛盾。对于Mapper接口，Mybatis禁止方法重载（overload）。

**● 在MyBatis当中，给sql语句传值，你知道哪几种方式？**

通过POJO（Javabean）可以传值，但要求#{}的大括号当中提供POJO的属性名。如果没有合适的POJO，可以使用Map集合进行传值，但要求#{}的大括号当中提供Map集合的key。如果DAO接口的方法参数有多个，并且数量不多，而且每个都是简单类型，也可以通过#{arg0}、#{arg1}的方式传参。   

## Spring专题

**● 谈谈你对Spring框架的理解？**

**● Spring IoC的理解以及实现原理？**

**● Spring AOP的理解以及实现原理？**

**● Spring是如何进行事务管理的？**

事务就是对一系列的数据库操作（比如插入多条数据）进行统一的提交或回滚操作，如果插入成功，那么一起成功，如果中间有一条出现异常，那么回滚之前的所有操作。这样可以防止出现脏数据，防止数据库数据出现问题。开发中为了避免这种情况一般都会进行事务管理。 Spring的声明式事务通常是指在配置文件中对事务进行配置声明，其中包括了很多声明属性，它是通过Spring Proxy帮你做代理，自己不用额外的写代码，只要在Spring配置文件中声明即可；通常用在数据库的操作里面； 编程式事务就是指通过硬编码的方式做事务处理，这种处理方式需要写代码，事务中的逻辑可以自己定制；可以是数据库的东东，也可以是其他的操作。 Spring中也有自己的事务管理机制，一般是使用TransactionMananger进行管理，可以通过Spring的注入来使用此功能。

**● Spring事务的传播特性有了解吗？**

**● Spring常用注解说几个？**

**● @Autowired 与@Resource的区别？**

**● IoC和DI的关系？**

**● 解释Spring支持的几种bean的作用域，怎么配置？**

**● 依赖注入的实现方式包括哪些？**

**● 说一下Spring Bean的生命周期？**

**● 什么是Spring的自动装配机制，包括哪些？**

**● 在Spring中怎么启用注解？ 使用 配置。**

**● Spring支持的事务管理类型有哪些？**

**● Spring AOP中什么是切面？**

**● Spring AOP中什么是关注点？**

 **●Spring AOP中什么是通知，包括哪些？**

**● Spring AOP中什么是切入点？**

**● Spring AOP中什么是连接点？**

**● Spring AOP中什么是目标对象？**

**● Spring AOP中什么是代理对象？**

**● Spring AOP中什么是织入？**

**● 你在实际开发中使用Spring AOP干什么了？**

**● Spring框架中的单例bean是否是线程安全的？**

**● Spring框架中BeanFactory和ApplicationContext有什么区别？**  

## SpringMVC专题

**● 说一下SpringMVC的执行流程？**

（1）用户发送请求至前端控制器DispatcherServlet；

（2） DispatcherServlet收到请求后，调用HandlerMapping处理器映射器，请求获取Handle；

（3）处理器映射器根据请求url找到具体的处理器，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet；

（4）DispatcherServlet 调用 HandlerAdapter处理器适配器；

（5）HandlerAdapter 经过适配调用 具体处理器(Handler，也叫后端控制器)；

（6）Handler执行完成返回ModelAndView；

（7）HandlerAdapter将Handler执行结果ModelAndView返回给DispatcherServlet；

（8）DispatcherServlet将ModelAndView传给ViewResolver视图解析器进行解析；

（9）ViewResolver解析后返回具体View；

（10）DispatcherServlet对View进行渲染视图（即将模型数据填充至视图中）

（11）DispatcherServlet响应用户。  

**● 说一下SpringMVC框架的理解？**

Spring MVC是一个基于Java实现了MVC架构模式的请求驱动类型的轻量级Web框架，通过把Model，View，Controller分离，将web层职能分工（解耦合），把复杂的web应用分成逻辑清晰的几部分，简化开发，减少出错，方便组内开发人员之间的配合。

**● SpringMVC常用注解？**

@RequestMapping：用于处理请求地址映射，可以作用于类和方法上。

@RequestParam：用于获取传入参数的值 @RequestBody：将客户端请求过来的json转成java对象 @RestController：@Controller+ @ResponseBody @GetMapping@DeleteMapping@PostMapping@PutMapping

@PathViriable：用于定义路径参数值 @ResponseBody：作用于方法上，可以将整个返回结果以某种格式返回，如json或xml格式。

@ModelAttribute：用于把参数保存到model中，可以注解方法或参数，注解在方法上的时候，该方法将在处理器方法执行之前执行，然后把返回的对象存放在 session（前提时要有@SessionAttributes注解） 或模型属性中，@ModelAttribute(“attributeName”) 在标记方法的时候指定，若未指定，则使用返回类型的类名称（首字母小写）作为属性名称。

**● SpringMVC的控制器是不是单例,如果是,有什么问题,怎么解决？**

是单例,所以在多线程访问的时候有线程安全问题,不要用同步,会影响性能的,解决方案是在控制器里面不能写字段。

**● 谈谈你对RESTful的理解？**

一种软件架构风格、设计风格，而不是标准，只是提供了一组设计原则和约束条件。它主要用于客户端和服务器交互类的软件。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。 REST 指的是一组架构约束条件和原则。满足这些约束条件和原则的应用程序或设计就是 RESTful。 REST被翻译为“表现层状态转换”，具体来说，就是HTTP协议里面，四个表示操作方式的动词：GET、POST、PUT、DELETE。它们分别对应四种基本操作：GET用来获取资源，POST用来新建资源（也可以用于更新资源），PUT用来更新资源，DELETE用来删除资源。使用REST编程风格要求URL中不允许出现动词。

**● 如何解决POST请求中文乱码问题，GET的又如何处理呢？**

POST乱码：web.xml文件中添加以下配置 CharacterEncodingFilter org.springframework.web.filter.CharacterEncodingFilter encoding utf-8 CharacterEncodingFilter /* GET乱码，修改Tomcat服务器server.xml文件： 

## 并发编程专题

**● 你进行过压测吗，怎么进行的？**  

## Redis专题

**●Redis你在项目中哪里使用了，用来解决什么问题？**

**●Redis支持哪些数据类型？**

**● 什么是Redis持久化？Redis有哪几种持久化方式？优缺点是什么？**

**● 什么是缓存穿透，如何避免？什么是缓存雪崩，如何避免？**

**● 数据库和Redis缓存数据一致性问题，你是怎么解决的？**

**● Redis有哪几种数据淘汰策略？**

**● Redis有哪些适合的场景？**

（1）会话缓存（Session Cache）最常用的一种使用Redis的情景是会话缓存（session cache）。 用Redis缓存会话比其他存储（如Memcached）的优势在于：Redis提供持久化。当维护一个不是严格要求一致性的缓存时，如果用户的购物车信息全部丢失，大部分人都会不高兴的，现在，他们还会这样吗？幸运的是，随着 Redis 这些年的改进，很容易找到怎么恰当的使用Redis来缓存会话的文档。甚至广为人知的商业平台Magento也提供Redis的插件。

（2）全页缓存（FPC）除基本的会话token之外，Redis还提供很简便的FPC平台。 回到一致性问题，即使重启了Redis实例，因为有磁盘的持久化，用户也不会看到页面加载速度的下降，这是一个极大改进，类似PHP本地FPC。再次以Magento为例，Magento提供一个插件来使用Redis作为全页缓存后端。此外，对WordPress的用户来说，Pantheon有一个非常好的插件 wp-redis，这个插件能帮助你以最快速度加载你曾浏览过的页面。

（3）队列Reids在内存存储引擎领域的一大优点是提供 list 和 set 操作，这使得Redis能作为一个很好的消息队列平台来使用。 Redis作为队列使用的操作，就类似于本地程序语言（如Python）对 list 的 push/pop 操作。如果你快速的在Google中搜索“Redis queues”，你马上就能找到大量的开源项目，这些项目的目的就是利用Redis创建非常好的后端工具，以满足各种队列需求。例如，Celery有一个后台就是使用Redis作为broker，你可以从这里去查看。

（4）排行榜/计数器Redis在内存中对数字进行递增或递减的操作实现的非常好。 集合（Set）和有序集合（Sorted Set）也使得我们在执行这些操作的时候变的非常简单，Redis只是正好提供了这两种数据结构。所以，我们要从排序集合中获取到排名最靠前的10个用户–我们称之为“user_scores”，我们只需要像下面一样执行即可：当然，这是假定你是根据你用户的分数做递增的排序。如果你想返回用户及用户的分数，你需要这样执行：ZRANGE user_scores 0 10 WITHSCORESAgora Games就是一个很好的例子，用Ruby实现的，它的排行榜就是使用Redis来存储数据的，你可以在这里看到。

（5）发布/订阅最后（但肯定不是最不重要的）是Redis的发布/订阅功能。 发布/订阅的使用场景确实非常多。我已看见人们在社交网络连接中使用，还可作为基于发布/订阅的脚本触发器，甚至用Redis的发布/订阅功能来建立聊天系统！   

## Dubbo专题

**● 可以说一下你对Dubbo的理解吗，什么是Dubbo？**

Dubbo是阿里巴巴开源的基于 Java 的高性能 RPC 分布式服务框架，现已成为 Apache 基金会孵化项目。 因为是阿里开源项目，国内很多互联网公司都在用，已经经过很多线上考验。内部使用了 Netty、Zookeeper，保证了高性能高可用性。 使用 Dubbo 可以将核心业务抽取出来，作为独立的服务，逐渐形成稳定的服务中心，可用于提高业务复用灵活扩展，使前端应用能更快速的响应多变的市场需求。 最重要的一点是，分布式架构可以承受更大规模的并发流量。比如某个服务并发量很大的话，可以针对此服务部署多台服务器提高性能。

**● Dubbo的实现原理？**

**● 你对zookeeper是怎么理解的，为什么要用它？**

**● 据你了解，zookeeper都能干什么？**

**● Dubbo支持负载均衡吗，有哪些策略？**

**● Dubbo超时重试之后导致的数据重复怎么解决？**

对于核心的服务中心，去除dubbo超时重试机制，并重新评估设置超时时间。业务处理代码必须放在服务端，客户端只做参数验证和服务调用，不涉及业务流程处理。

# Java p2p项目面试题

P2P项目专题

**● 具体的你在P2P项目中都干什么了，你负责哪一块，每一块实现的时候用到了哪些技术？**

**● 你们项目的并发量设计了多少？**

**● P2P项目上线了吗？**

**● 你项目中的事务是怎么处理的？**

**● 在秒杀项目中使用消息队列ActiveMQ进行流量削峰，如果ActiveMQ接收消息失败了，怎么办？**

消息在接收后会被服务器删除，为了避免接收消息失败而消息又被服务器删除，此时我们可以关闭自动确认机制AUTO_ACKNOWLEDGE，采用手动消息确认机制，由程序进行消息的确认，接收消息发生异常，则不确认消息，以便于下次可以再次接收。

**● 在秒杀项目中使用消息队列ActiveMQ进行流量削峰，如果ActiveMQ挂掉怎么办？**

第一点，首先消息需要使用持久化消息，服务挂掉，重启服务后消息依然可以消费，不会丢失；

第二点，ActiveMQ采用主从模式搭建集群，比如搭建3台主从模式的ActiveMQ集群，提高服务的可用性；

**● 在交易中，如何控制超卖问题（卖出去的商品大于数据库实际商品库存）？**

解决超卖问题，一个是借助于锁机制实现，这里面有线程锁、数据库锁、分布式锁等，另一个是借助于某些中间件产品实现，比如Redis；

如果我们的服务只是部署了一台服务器，我们通过线程锁即可控制并发问题，控制了并发问题，也就不会产生减库存的冲突，即不会产生超卖问题，这种实现，效率不高，而且只能是单机部署，实际项目不会采用；

如果我们的服务是集群部署，线程锁就不行了，此时可以使用数据库锁或分布式锁控制并发问题，从而控制减库存的冲突，避免超卖问题，数据库锁可以采用乐观锁、悲观锁，其中乐观锁比悲观锁效率稍高，在实际项目中使用多一些，悲观锁使用较少，但由于数据库本身的性能和并发处理能力并不理想，在高并发项目中，使用数据库锁也是不合适的；

使用分布式锁解决超卖问题，在实际项目中有相关真实的案例，主要采用zookeeper实现分布式锁，分布式锁是将我们的线程锁扩展为多个jvm的锁，代码在多个jvm上执行时，分布式锁也能进行锁定，因为能锁定就能控制并发，控制了并发即能控制减库存的冲突，即可解决超卖问题；

使用一些中间件产品解决超卖问题被经常使用，最被常用的是Redis，在实际项目中被大量使用，由于Redis是单线程的，不管有多少个并发请求，Redis会将请求排队进行处理，即一个一个地有先后顺序地处理，这样即不会有并发问题，即不会产生减库存的冲突，从而解决减库存的超卖问题；

**● 在实际项目中，是否使用过多线程？**

在P2P项目中，比如在用户投资到期后需要给用户回款，此处我们使用了多线程，加快整个回款的速度，我们先从数据库获取所有待回款的数据，然后创建一个线程池，每个回款是一个线程，将这些回款线程提交到线程池中执行，从而充分利用服务器的CPU资源快速为用户回款；

再比如当每个投资用户生日时，我们会在用户生日当天给用户送一个生日红包，由于同一天有大量用户生日，我们也是通过多线程为用户送红包，先从数据库获取当天生日的用户，然后创建一个线程池，给每个用户发红包是一个线程，将这些发红包线程提交到线程池中执行，从而加速生日红包的发送任务；

**● P2P中的投资及收益金额采用Java中的什么数据类型进行存储？**

由于涉及到精度问题，我们项目中都采用BigDecimal类型，以避免在计算收益时，导致金额精确的损失。

**● 你们这个P2P项目上线后采用的是什么访问协议？**

为了数据传输的安全性，我们的p2p项目访问的时候，全部都https协议，https协议会将数据加密传输，提高安全性，我们当时公司的运维部门采购了https的安全证书，在服务器上搭建了https协议的访问方式，如果用户采用http访问，我们会自动跳转到https协议打开网页；

**● 你们P2P项目对金钱是怎么处理的？**

项目中涉及到的资金问题，有几处处理，一个是我们有一个后台对账系统，每天会根据第三方支付系统的结算清单，与我们这边的充值数据进行对账，保证我们与第三方的数据一致；

对于用户投资到期后，提现自己的本金和收益，我们后台系统有专门的提现审核功能，通过系统审核该投资用户的资金流是否有异常，无异常方可通过提现审核。

**● 你们使用定时任务是干什么的？**

我们使用定时任务主要是处理一些定时或延迟的工作，这些工作不需要马上处理，就配置好时间，让程序在指定的时间或者指定的频率去执行，比如我们在理财产品投资满标后，会生成收益计划，投资到期后给用户返回收益等，我们都是采用定时任务来完成的，Spring框架集成支持定时任务，我们采用的就是spring框架下的spring-task来实现定时任务；

**● 请你描述一下整个P2P的支付流程？**

支付模块，我们当时对接了两家，一家快钱支付，一家丰付支付，当时是由我开发的，我们的商务和对方谈好后并签订了支付协议，对方的技术给我们提供了相关的支付接口文档及demo，我主要是根据对方的接口文档进行开发，首先是调用对方提供的支付下单接口，把我这边准备好的参数传给对方，然后调用对方接口，根据对方的返回信息进行处理，如果对方返回成功，然后调用对方的获取短信验证码接口，此时将给用户的手机号发送一个支付验证码，用户输入支付短信验证码后，点击确定支付，然后我们提交支付请求，对方将返回支付的结果，支付结果对方会通过两种方式返回，一个是同步返回，一个是异步返回，我们接收对方的这两个返回结果，更新我们的数据库状态，完成整个支付流程；

**● 请介绍一下这个P2P项目的整体架构及你做了什么？**

整个P2P项目第一个版本，我们是采用普通的ssm框架进行开发的，是一种集中式的开发方式，上线一年之后，随着公司发展和业务需要，我们原来的ssm架构的项目，代码非常庞大和混乱，一个方法可能出现好几百行，里面很多逻辑，要新增一个功能，开发特别慢，由于修改这个功能，可能又导致另一个功能出现问题或者bug，后来我们对整个p2p项目进行了重构，采用分布式开发方式，当时选择了非常流行的Dubbo分布式开发框架，主要架构是spring mvc + spring + dubbo +mybatis的架构，另外还有一些中间件，dubbo注册中心zookeeper，缓存Redis、队列ActiveMQ、文件存储FastDFS，Session同步Sprign Session等；

在这个项目中，我参与了整个过程的开发，当时公司处于快速发展中，工作分工并不明确，前后台基本都会参与开发，我先是开发了p2p前端网站部分的理财产品展示、用户投资、用户充值三大功能模块，同时开发这些模块对应的Dubbo服务，其中的支付模块，是单独一个项目，主要对接第三方支付接口，快钱和丰付，这个项目全部是我完成的，对此我也比较熟悉。

**● 你们这个P2P项目的并发量大概多少，部署了几台tomcat？**

我们P2P项目有三端，一个是PC端，一个是H5端，一个的App端，其中H5端的流量比较小，并发量也很低，PC端和App端并发量高一些，其中App端并发量最大，因为App端的用户最多，App端的后端接口部署了5台tomcat，最大的并发是3500左右，这个3500是同时处理请求的能力，而不是一段时间或者多少秒的处理能力，QPS大约是30万左右，整个平台大约有200万左右的用户；

**● 你们P2P项目中分布式事务怎么解决和处理的？**

我们这个P2P项目，后端的服务，没有再拆分，就是一个dubbo服务，所以一个事务请求会提交到一个项目单元上执行，这样我们是避免了分布式事务的问题，因为对于一般的中小型项目，也建议不涉及到分布式事务的问题，也就是说能避免尽量避免，而在一些大型项目中无法避免需要分布式事务时，目前常用的解决方案是：

1、两阶段提交；

2、三阶段提交；

3、TCC补偿；

4、异步确保；

5、最大努力通知；

**● 为什么充值会发生掉单问题？怎么解决的？**

充值时候与第三方接口对接的过程中，涉及到多个系统，每个系统都无法保证整个充值过程中都是100%的高可用，还有网络等原因，就不可避免会出现某个系统成功了，另一个系统没有成功的问题，当出现这种情况的时候，这笔充值就会出现数据状态的不一致，也就是掉单，为了解决该问题，我们采用的是一种补偿机制，在发生掉单后，进行自动补偿，系统开启一个定时任务，对出现掉单的订单进行再次补偿，我们会先检索出掉单的订单，然后通过第三方支付系统的接口，查询该订单的充值状态，如果第三方已经成功了，我们这边系统就需要更新相关的数据；

**● 你们的P2P项目使用Redis主要做什么？**

在该项目中，最初没有使用Redis，是边运营边迭代升级的，在没有使用Redis的时候，我们的前端业务系统上的所有数据都是直接到达数据库获取，导致我们后端的数据库经常出现cpu及io压力很大，后续我们将前端业务系统上一些不需要实时更新的数据，一些频繁查询的热点数据，进行了Redis缓存存储，来提升系统的能力。

**● 生产环境（线上环境）中出现的问题，你们是怎么解决的？**

生产中的问题，发现后由该系统的技术负责人全力协调解决，如果是紧急影响较大的bug，会暂时下线该功能，快速对该问题进行修复，然后由测试团队进行严格测试，再上线，再次上线前将之前由于bug产生的数据问题进行修复。处理线上问题的步骤是先判断问题的严重程序、波及范围等，优先快速恢复服务，让用户可以继续使用，然后再解决bug，排查bug主要是根据线上日志、数据库数据等；

**● 你们P2P项目的利息是怎么计算的?**

利息的计算，公司的产品经理提供了计算公式，我们技术人员根据计算公式进行计算，由于涉及到精度问题，所有计算都是采用BigDecimal类型进行加减乘除；

**● 用户在你的P2P充值，如何防止你的请求被黑客拦截，给你返回一个假的充值成功结果？实际用户未支付，但你系统给用户充了值？**

这个问题，我们有签名验签机制就保证了，我们的请求中都有一个签名串，签名串黑客无法伪造，如果签名串验证无法通过，我们将不会给用户充值，提示签名失败；

**● 如何防止用户重复点击、重复提交充值？**

用户点击后我们会将用户的提交在redis中存放一个标志，如果用户重复提交，我们会检查redis的标志，来拒绝用户的第二次提交，值处理第一次提交请求；

**● 用户的钱的整个流转过程是怎么样的？**

用户通过第三方充值渠道，将用户银行卡的资金划入到P2P平台在第三方支付公司开通的账户中，然后我们会把用户的充值金额记录在我们的数据库用户资金记录表中，当用户投资某个产品，比如100元，我们会将用户资金记录表中的资金冻结掉投资金额100元，当投资到期后，会产生收益，比如收益是1元，那么我们就会将之前冻结的100元，再加上收益1元返回到用户的资金记录表中，最后用户提现101元，我们的后台结算系统审核用户的提现金额，审核通过后，通过第三方支付公司，从我们P2P平台在第三支付公司开通的账户中扣除101元，将101元打入用户提交的银行账号中；

**● 请介绍一下你做的这个P2P项目？**

该项目是一个基于互联网金融的网贷平台，有理财端和借款端，我主要是参与做的理财端，该项目主要包括PC站、M站、APP客户端（Android、iOS），由多个项目系统构成，包括前端业务系统PC端和H5端、数据接口系统、核心系统、结算系统、支付系统、定时任务系统、营销活动系统，红包系统，合同签章系统、实名认证接口系统、轮播图系统等。

整个项目采用分布式开发模式，Tomcat集群部署，采用Nginx实现负载均衡和动静分离，MySQL主从复制模式，Redis集群模式，ActiveMQ的异步处理、Zookeeper做为注册中心，Spring session共享，Redis集群缓存热点数据提升系统吞吐量，整个项目采用的技术架构主要是：Spring mvc + Spring + Dubbo +MyBatis, 采用Dubbo实现项目间的RPC调用，采用分布式文件系统FastDFS存储投资借款合同。

我在这个平台中参与过前端业务系统，Dubbo数据接口系统，支付接口系统、合同签章系统的开发，对这几个系统比较熟悉，通过这个项目，遇到过一些问题，也学到很多东西。比如最早的时候，对Dubbo开发框架不是很了解，通过该项目，现在比较得心应手。

互联网分布式专题

**● 你都知道哪些技术可以完成异构系统整合？**

httpclient、webservices

## HR专题

**● 自我介绍？**

面试官您好，我叫XX，非常高兴认识您。到目前为止我从事Java软件编码工作已经3年了，之前一直在石家庄的XX公司工作，他是一家外包公司，在这期间我接触了有六七个项目，其中包括政府部门的项目、银行相关的项目、还有互联网公司相关的项目，最近做的是一个P2P互联网金融项目。北京这边毕竟软件环境好一些，有一些朋友也来到北京发展情况挺好的，所以前两天把石家庄的工作辞掉了，想在北京闯一闯。这就是我的个人情况，谢谢。

说明：这只是一个参考的模板，自行修改，并且在进行自我介绍的时候不要死记硬背。要用描述性的语言说出来。

**● 说一个我们留下你的理由？**

首先，我个人非常喜欢热爱Java软件开发这份工作，我喜欢耐心的做一件事，实现一个一个客户的需求。

其次，我的技术能力完全可以胜任咱们这份工作，技术这块您放心，肯定不掉链子，按时按点完成领导交办的任务。

另外，我也喜欢咱们公司的企业文化以及工作氛围。希望贵公司能给一次机会。

**● 你还有什么要问的吗？**

基本上也没什么问题了，我就是想知道一下咱们团队开发使用的是哪些技术，如果可以的话，我回去抓紧时间熟悉一下。比如用的哪些框架，后台使用的什么技术，前端使用的什么框架等。

**● 你的期望薪资是多少？**

多种回答方式：第一种是直接说一个薪资值，例如：10K左右是可以接受的（带个左右，不要说区间值，千万别说10~15）。第二种方式可以这样说：我来之前也对北京这块的薪资结构有了一定的了解，3年工作经验在北京这块大概在12K以上，所以12K左右都可以接受，当然，薪资这块不是硬性要求，能够遇到一个好的团队，优秀的企业这是最主要的。

**● 说一下职业规划？**

其实具体的职业规划目前也没有刻意的去制定过，目前来说我还是希望能够把技术底层再好好的研究一下，比如像数据结构、算法、框架底层源代码等。我还是比较喜欢钻研技术。

**● 你平时除了开发还干什么？**

平时除了编码之外，主要还是为公司产品解决一些线上的问题，调一些bug。另外还会写日报周报、参与各种会议、和产品沟通、和测试沟通等等，闲暇时间会学习一些新的技术。

**● 项目经理通过什么方式分配任务？**

这个问题的答案不是固定的，因为每个团队的管理方式不同。比较正规的大的公司一般会使用专业的工具，例如project、Planisware工具，但这些工具使用复杂。对于一些规模比较小的公司来说一般excel就搞定了，在excel表格中描述任务并自动生成甘特图来跟踪任务的进展。（建议这样回答：我们项目组使用的excel进行任务分配的，在excel中标上一些特殊的颜色，来表示不同的进度。）

**● 项目经理怎么监控项目进度的？**

每一天我们都要按时提交日报，每一周都要按时提交周报，项目经理通过日报周报等形式监控项目进度。

**● 项目经理怎么监控代码质量的？**

不定期的进行代码走查（代码review），项目经理、组长、程序员一起到会议室，打开投影仪，把最近几天的代码一行一行的看一看，主要看看代码的注释、代码的格式、代码的命名规范、代码的冗余度等。

**● 怎么和测试沟通的？**

**● 你平时和谁沟通较多？**

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20191101/1572583991@55c92681a6a9114d39f6ec595e62eb7b.png)

通过上图可以看出，开发通常和产品、测试沟通较多。

**● 你们小组是怎么沟通的？**

**●** **你们平时都什么时候开会，都开什么会？**

一般都会有晨会（小组会议）。

每日晨会是敏捷开发过程中最为重要的一个环节。核心团队成员每天早上开一个非常短的碰头会，每人平均2到3分钟，介绍昨天做了什么，今天要做什么，有什么困难没有。 

主要目的是便于项目管理人员了解任务开发状态，发现潜在的隐患，督促团队成员勤勉工作，帮助解决困难。

一般采用站立式非正规会议，集中在一个小会议室或者是在稍微偏僻一点的走廊。 

当项目管理人员发现某个团队成员工作不力或者是遇到困难的时候，除在会议上作简短确认以及询问其它团队成员能否帮助外，还应在会后与当事人进行详细沟通。

有一些公司会有夕会，下班时候的一个碰头会（小组会议）。

每周都会有周例会（公司的所有成员会议）。

**● 为什么从上家公司离职？**

北京这边的软件环境好一些，趁年轻想来大城市闯一闯。

**● 你上家公司在哪，哪条路，哪个大厦？**

这个题目就需要随机应变了，比如：上家公司在北京大兴区，经济技术开发区，凉水河二街，大族企业湾，11号楼A座三层。

**● 你上家公司是做哪个行业的？**

这个题目需要大家查一下“之前工作的公司”，了解一下该公司主要做哪一方面的。如果上一家工作的公司是外包公司那是不错的。因为被外派出去的人员可能会接触不同行业的项目。

**● 对你上家公司进行一个评价？**

我相信面试官您也看到了，自从我大学毕业到现在一直在上家公司工作，这也充分证明了我对上家公司的认可。上家公司拥有良好的企业文化，为我们提供了良好的工作环境，我们团队也构建了一个良好的工作氛围，大家工作都很积极努力，团队成员也都很照顾我，如果还有机会回到石家庄工作的话，我希望能再回到这家公司。

**● 说一下你的离职流程？**

我离职的前1个月提出了申请，然后我们老大就开始招人，招到后，我就开始和新人进行工作交接，直到新人能够上手，我就离开了。

**● 你们多少人开发这个项目？**

**● 说一下你们项目组的组织架构/人员分配？**

**● 你们分组了吗，都是什么组，你在哪一组？**

**● 你和领导在一些想法上产生了分歧，你怎么做？**

在工作中，和领导打交道是一个技术活，让领导开心很重要，但也不能委屈了自己，当和领导产生分歧后，好尽量的去挽救双方之间的关系，毕竟自己使下属，在该让步的时候还是要学会让步。

第一：积极的沟通。也许是领导不了你的实际情况，没看到你的付出和努力，对你产生了误解，所以适当的沟通就很重要了。

第二：保持不卑不亢的态度。在产生分歧的时候，不要一味的退让或者太过于强势，凡是都是可以沟通的，让领导看到自己的工作态度很重要。

第三：显示出自己对领导的尊重。也许你的工作能力是比较出众的，但是一定要给足领导面子，让领导看出自己的态度，免得给领导留下了不好的态度。

第四：选择求同存异的处理手法。特别是解决工作上的问题，手段是有很多的，你考虑的方向和领导考虑的方向可能会有所出入，可以选择一种大家都认同的方法来处理。

第五：领导布置的任务要好好的完成。就算是和领导有分歧，但是领导布置的任务还是要好好的完成，这样可以显示出自己对工作一丝不苟的态度，也会的到领导的赞赏。

第六：在不改变自己原则的前提下妥协。先要表明自己的立场，让自己不做违背自己本心的事情，然后考虑妥协，对方是领导，很有可能是你处于被动的地步，所以适当的妥协是有必要的。

**● 你们项目组使用的是哪个项目管理软件？**

禅道。

**● 你们项目组使用的是哪个版本控制工具？**

最近两年的开发一直在使用Git，感觉Git比SVN好用。Git是基于分布式的。

**● 你之前交过社保吗？**

之前没有缴纳过社保，公司每个月有800元补助。（之前在北京工作过，并且在北京交过社保的同学，可以回答：交过社保。如果社保不是连续的，可以告诉面试官，社保中间断了，不想交社保了，公司每个月有800元补助。）

**● 程序员苦逼的一天？**

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20191101/1572584228@4f9b3e678ff323d00267f10e4a9728f3.png)![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20191101/1572584247@46dfb35ea85505559836235268450d63.png)![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20191101/1572584261@c87dc6086fa6d90b2e406bd91abf33ca.png)![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20191101/1572584277@7f7d0369210bbd71c7fe7c8c862a47cc.png)![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20191101/1572584292@aab2ec1bccfdc4413804267ed619bf66.png)![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20191101/1572584315@52de72be08f2ea75d04f3b94553ee578.png)

**其它**

**● 你们的测试环境是怎样的？**

我们的测试环境模拟的就是生产环境，只不过生产环境中服务器硬件要多一些，在测试环境中一个机器上会安装多个软件。

**● AJAX跨域你是怎么实现的？**

首先我先给您解释一下我理解的跨域，为什么会有跨域呢？这是因为浏览器的同源策略导致的，同源策略是一种约定，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，则浏览器的正常功能可能都会受到影响。可以说 Web 是构建在同源策略基础之上的，浏览器只是针对同源策略的一种实现。它的核心就在于它认为来自任何站点装载的信赖内容是不安全的。当被浏览器半信半疑的脚本运行在沙箱时，它们应该只被允许访问来自同一站点的资源，而不是那些来自其它站点可能怀有恶意的资源。所谓同源是指：域名、协议、端口相同。所以跨域限制主要的目的就是为了用户的上网安全。不过在实际的开发中有很多情况下需要我们实现跨域访问，因为同一个项目的不同服务可能部署在不同的服务器当中，服务器A调用服务器B当中的资源时，就涉及到了跨域问题，那么怎么解决AJAX跨域呢？

第一种方式：JSONP方式解决跨域问题。

jsonp解决跨域问题是一个比较古老的方案（实际中不推荐使用），实际项目中如果要使用JSONP，一般会使用jQuery对JSONP进行了封装的类库来进行ajax请求。

# Java volatile面试题

volatile这个关键字可能很多朋友都听说过，或许也都用过。在Java 5之前，它是一个备受争议的关键字，因为在程序中使用它往往会导致出人意料的结果。在Java 5之后，volatile关键字才得以重获生机。

volatile关键字虽然从字面上理解起来比较简单，但是要用好不是一件容易的事情。由于volatile关键字是与Java的内存模型有关的，因此在讲述volatile关键之前，我们先来了解一下与内存模型相关的概念和知识，然后分析了volatile关键字的实现原理，最后给出了几个使用volatile关键字的场景。

## 以下是本文的目录大纲：

一.内存模型的相关概念

二.并发编程中的三个概念

三.Java内存模型

四.深入剖析volatile关键字

五.使用volatile关键字的场景

## 内存模型的相关概念

大家都知道，计算机在执行程序时，每条指令都是在CPU中执行的，而执行指令过程中，势必涉及到数据的读取和写入。由于程序运行过程中的临时数据是存放在主存（物理内存）当中的，这时就存在一个问题，由于CPU执行速度很快，而从内存读取数据和向内存写入数据的过程跟CPU执行指令的速度比起来要慢的多，因此如果任何时候对数据的操作都要通过和内存的交互来进行，会大大降低指令执行的速度。因此在CPU里面就有了高速缓存。

也就是，当程序在运行过程中，会将运算需要的数据从主存复制一份到CPU的高速缓存当中，那么CPU进行计算时就可以直接从它的高速缓存读取数据和向其中写入数据，当运算结束之后，再将高速缓存中的数据刷新到主存当中。举个简单的例子，比如下面的这段代码：

i = i + 1;

当线程执行这个语句时，会先从主存当中读取i的值，然后复制一份到高速缓存当中，然后CPU执行指令对i进行加1操作，然后将数据写入高速缓存，最后将高速缓存中i最新的值刷新到主存当中。

这个代码在单线程中运行是没有任何问题的，但是在多线程中运行就会有问题了。在多核CPU中，每条线程可能运行于不同的CPU中，因此每个线程运行时有自己的高速缓存（对单核CPU来说，其实也会出现这种问题，只不过是以线程调度的形式来分别执行的）。本文我们以多核CPU为例。

比如同时有2个线程执行这段代码，假如初始时i的值为0，那么我们希望两个线程执行完之后i的值变为2。但是事实会是这样吗？

可能存在下面一种情况：初始时，两个线程分别读取i的值存入各自所在的CPU的高速缓存当中，然后线程1进行加1操作，然后把i的最新值1写入到内存。此时线程2的高速缓存当中i的值还是0，进行加1操作之后，i的值为1，然后线程2把i的值写入内存。

最终结果i的值是1，而不是2。这就是著名的缓存一致性问题。通常称这种被多个线程访问的变量为共享变量。

也就是说，如果一个变量在多个CPU中都存在缓存（一般在多线程编程时才会出现），那么就可能存在缓存不一致的问题。

为了解决缓存不一致性问题，通常来说有以下2种解决方法：

1）通过在总线加LOCK#锁的方式

2）通过缓存一致性协议

这2种方式都是硬件层面上提供的方式。

在早期的CPU当中，是通过在总线上加LOCK#锁的形式来解决缓存不一致的问题。因为CPU和其他部件进行通信都是通过总线来进行的，如果对总线加LOCK#锁的话，也就是说阻塞了其他CPU对其他部件访问（如内存），从而使得只能有一个CPU能使用这个变量的内存。比如上面例子中 如果一个线程在执行 i = i +1，如果在执行这段代码的过程中，在总线上发出了LCOK#锁的信号，那么只有等待这段代码完全执行完毕之后，其他CPU才能从变量i所在的内存读取变量，然后进行相应的操作。这样就解决了缓存不一致的问题。

但是上面的方式会有一个问题，由于在锁住总线期间，其他CPU无法访问内存，导致效率低下。所以就出现了缓存一致性协议。最出名的就是Intel 的MESI协议，MESI协议保证了每个缓存中使用的共享变量的副本是一致的。它核心的思想是：当CPU写数据时，如果发现操作的变量是共享变量，即在其他CPU中也存在该变量的副本，会发出信号通知其他CPU将该变量的缓存行置为无效状态，因此当其他CPU需要读取这个变量时，发现自己缓存中缓存该变量的缓存行是无效的，那么它就会从内存重新读取。

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20191101/1572585749@9bfa3031e0f60016ccce4ddd491cb5db.png)

并发编程中的三个概念

**● 原子性**

原子性：即一个操作或者多个操作 要么全部执行并且执行的过程不会被任何因素打断，要么就都不执行。

一个很经典的例子就是银行账户转账问题：

比如从账户A向账户B转1000元，那么必然包括2个操作：从账户A减去1000元，往账户B加上1000元。

试想一下，如果这2个操作不具备原子性，会造成什么样的后果。假如从账户A减去1000元之后，操作突然中止。然后又从B取出了500元，取出500元之后，再执行 往账户B加上1000元 的操作。这样就会导致账户A虽然减去了1000元，但是账户B没有收到这个转过来的1000元。

所以这2个操作必须要具备原子性才能保证不出现一些意外的问题。

同样地反映到并发编程中会出现什么结果呢？

举个最简单的例子，大家想一下假如为一个32位的变量赋值过程不具备原子性的话，会发生什么后果？

i = 9;

假若一个线程执行到这个语句时，我暂且假设为一个32位的变量赋值包括两个过程：为低16位赋值，为高16位赋值。

那么就可能发生一种情况：当将低16位数值写入之后，突然被中断，而此时又有一个线程去读取i的值，那么读取到的就是错误的数据。

**● 可见性**

可见性是指当多个线程访问同一个变量时，一个线程修改了这个变量的值，其他线程能够立即看得到修改的值。

举个简单的例子，看下面这段代码：

//线程1执行的代码

int i = 0;

i = 10;

//线程2执行的代码

j = i;

假若执行线程1的是CPU1，执行线程2的是CPU2。由上面的分析可知，当线程1执行 i =10这句时，会先把i的初始值加载到CPU1的高速缓存中，然后赋值为10，那么在CPU1的高速缓存当中i的值变为10了，却没有立即写入到主存当中。

此时线程2执行 j = i，它会先去主存读取i的值并加载到CPU2的缓存当中，注意此时内存当中i的值还是0，那么就会使得j的值为0，而不是10。
这就是可见性问题，线程1对变量i修改了之后，线程2没有立即看到线程1修改的值。

**● 有序性**

有序性：即程序执行的顺序按照代码的先后顺序执行。举个简单的例子，看下面这段代码：

int i = 0;     

boolean flag = false;

i = 1;         //语句1  

flag = true;      //语句2

上面代码定义了一个int型变量，定义了一个boolean类型变量，然后分别对两个变量进行赋值操作。从代码顺序上看，语句1是在语句2前面的，那么JVM在真正执行这段代码的时候会保证语句1一定会在语句2前面执行吗？不一定，为什么呢？这里可能会发生指令重排序（Instruction Reorder）。

下面解释一下什么是指令重排序，一般来说，处理器为了提高程序运行效率，可能会对输入代码进行优化，它不保证程序中各个语句的执行先后顺序同代码中的顺序一致，但是它会保证程序最终执行结果和代码顺序执行的结果是一致的。

比如上面的代码中，语句1和语句2谁先执行对最终的程序结果并没有影响，那么就有可能在执行过程中，语句2先执行而语句1后执行。

但是要注意，虽然处理器会对指令进行重排序，但是它会保证程序最终结果会和代码顺序执行结果相同，那么它靠什么保证的呢？再看下面一个例子：

int a = 10;   //语句1

int r = 2;   //语句2

a = a + 3;   //语句3

r = a*a;   //语句4

这段代码有4个语句，那么可能的一个执行顺序是：

![img](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20191101/1572585822@c401d743dd7bfc7a263c5b0505ab2247.png)

那么可不可能是这个执行顺序呢： 语句2  语句1   语句4  语句3
不可能，因为处理器在进行重排序时是会考虑指令之间的数据依赖性，如果一个指令Instruction 2必须用到Instruction 1的结果，那么处理器会保证Instruction 1会在Instruction 2之前执行。

虽然重排序不会影响单个线程内程序执行的结果，但是多线程呢？下面看一个例子：

```java
//线程1:
context = loadContext();   //语句1
inited = true;             //语句2
//线程2:
while(!inited ){
  			sleep()
}
doSomethingwithconfig(context);
```

上面代码中，由于语句1和语句2没有数据依赖性，因此可能会被重排序。假如发生了重排序，在线程1执行过程中先执行语句2，而此是线程2会以为初始化工作已经完成，那么就会跳出while循环，去执行doSomethingwithconfig(context)方法，而此时context并没有被初始化，就会导致程序出错。

从上面可以看出，指令重排序不会影响单个线程的执行，但是会影响到线程并发执行的正确性。

也就是说，要想并发程序正确地执行，必须要保证原子性、可见性以及有序性。只要有一个没有被保证，就有可能会导致程序运行不正确。

## Java内存模型

在前面谈到了一些关于内存模型以及并发编程中可能会出现的一些问题。下面我们来看一下Java内存模型，研究一下Java内存模型为我们提供了哪些保证以及在java中提供了哪些方法和机制来让我们在进行多线程编程时能够保证程序执行的正确性。

在Java虚拟机规范中试图定义一种Java内存模型（Java Memory Model，JMM）来屏蔽各个硬件平台和操作系统的内存访问差异，以实现让Java程序在各种平台下都能达到一致的内存访问效果。那么Java内存模型规定了哪些东西呢，它定义了程序中变量的访问规则，往大一点说是定义了程序执行的次序。注意，为了获得较好的执行性能，Java内存模型并没有限制执行引擎使用处理器的寄存器或者高速缓存来提升指令执行速度，也没有限制编译器对指令进行重排序。也就是说，在java内存模型中，也会存在缓存一致性问题和指令重排序的问题。

Java内存模型规定所有的变量都是存在主存当中（类似于前面说的物理内存），每个线程都有自己的工作内存（类似于前面的高速缓存）。线程对变量的所有操作都必须在工作内存中进行，而不能直接对主存进行操作。并且每个线程不能访问其他线程的工作内存。

举个简单的例子：在java中，执行下面这个语句：

i = 10;

执行线程必须先在自己的工作线程中对变量i所在的缓存行进行赋值操作，然后再写入主存当中。而不是直接将数值10写入主存当中。

那么Java语言 本身对 原子性、可见性以及有序性提供了哪些保证呢？

**1.原子性**

在Java中，对基本数据类型的变量的读取和赋值操作是原子性操作，即这些操作是不可被中断的，要么执行，要么不执行。

上面一句话虽然看起来简单，但是理解起来并不是那么容易。看下面一个例子i：

请分析以下哪些操作是原子性操作：

x = 10;     //语句1

y = x;     //语句2

x++;      //语句3

x = x + 1;   //语句4

咋一看，有些朋友可能会说上面的4个语句中的操作都是原子性操作。其实只有语句1是原子性操作，其他三个语句都不是原子性操作。

语句1是直接将数值10赋值给x，也就是说线程执行这个语句的会直接将数值10写入到工作内存中。

语句2实际上包含2个操作，它先要去读取x的值，再将x的值写入工作内存，虽然读取x的值以及 将x的值写入工作内存 这2个操作都是原子性操作，但是合起来就不是原子性操作了。

同样的，x++和 x = x+1包括3个操作：读取x的值，进行加1操作，写入新的值。

所以上面4个语句只有语句1的操作具备原子性。

也就是说，只有简单的读取、赋值（而且必须是将数字赋值给某个变量，变量之间的相互赋值不是原子操作）才是原子操作。

不过这里有一点需要注意：在32位平台下，对64位数据的读取和赋值是需要通过两个操作来完成的，不能保证其原子性。但是好像在最新的JDK中，JVM已经保证对64位数据的读取和赋值也是原子性操作了。

从上面可以看出，Java内存模型只保证了基本读取和赋值是原子性操作，如果要实现更大范围操作的原子性，可以通过synchronized和Lock来实现。

由于synchronized和Lock能够保证任一时刻只有一个线程执行该代码块，那么自然就不存在原子性问题了，从而保证了原子性。

**2.可见性**

对于可见性，Java提供了volatile关键字来保证可见性。

当一个共享变量被volatile修饰时，它会保证修改的值会立即被更新到主存，当有其他线程需要读取时，它会去内存中读取新值。

而普通的共享变量不能保证可见性，因为普通共享变量被修改之后，什么时候被写入主存是不确定的，当其他线程去读取时，此时内存中可能还是原来的旧值，因此无法保证可见性。

另外，通过synchronized和Lock也能够保证可见性，synchronized和Lock能保证同一时刻只有一个线程获取锁然后执行同步代码，并且在释放锁之前会将对变量的修改刷新到主存当中。因此可以保证可见性。

**3.有序性**

在Java内存模型中，允许编译器和处理器对指令进行重排序，但是重排序过程不会影响到单线程程序的执行，却会影响到多线程并发执行的正确性。

在Java里面，可以通过volatile关键字来保证一定的“有序性”（具体原理在下一节讲述）。另外可以通过synchronized和Lock来保证有序性，很显然，synchronized和Lock保证每个时刻是有一个线程执行同步代码，相当于是让线程顺序执行同步代码，自然就保证了有序性。

另外，Java内存模型具备一些先天的“有序性”，即不需要通过任何手段就能够得到保证的有序性，这个通常也称为 happens-before 原则。如果两个操作的执行次序无法从happens-before原则推导出来，那么它们就不能保证它们的有序性，虚拟机可以随意地对它们进行重排序。

下面就来具体介绍下happens-before原则（先行发生原则）：
程序次序规则：一个线程内，按照代码顺序，书写在前面的操作先行发生于书写在后面的操作

锁定规则：一个unLock操作先行发生于后面对同一个锁额lock操作
volatile变量规则：对一个变量的写操作先行发生于后面对这个变量的读操作

传递规则：如果操作A先行发生于操作B，而操作B又先行发生于操作C，则可以得出操作A先行发生于操作C

线程启动规则：Thread对象的start()方法先行发生于此线程的每个一个动作

线程中断规则：对线程interrupt()方法的调用先行发生于被中断线程的代码检测到中断事件的发生

线程终结规则：线程中所有的操作都先行发生于线程的终止检测，我们可以通过Thread.join()方法结束、Thread.isAlive()的返回值手段检测到线程已经终止执行

对象终结规则：一个对象的初始化完成先行发生于他的finalize()方法的开始

这8条原则摘自《深入理解Java虚拟机》。

这8条规则中，前4条规则是比较重要的，后4条规则都是显而易见的。

下面我们来解释一下前4条规则：

对于程序次序规则来说，我的理解就是一段程序代码的执行在单个线程中看起来是有序的。注意，虽然这条规则中提到“书写在前面的操作先行发生于书写在后面的操作”，这个应该是程序看起来执行的顺序是按照代码顺序执行的，因为虚拟机可能会对程序代码进行指令重排序。虽然进行重排序，但是最终执行的结果是与程序顺序执行的结果一致的，它只会对不存在数据依赖性的指令进行重排序。因此，在单个线程中，程序执行看起来是有序执行的，这一点要注意理解。事实上，这个规则是用来保证程序在单线程中执行结果的正确性，但无法保证程序在多线程中执行的正确性。

第二条规则也比较容易理解，也就是说无论在单线程中还是多线程中，同一个锁如果出于被锁定的状态，那么必须先对锁进行了释放操作，后面才能继续进行lock操作。

第三条规则是一条比较重要的规则，也是后文将要重点讲述的内容。直观地解释就是，如果一个线程先去写一个变量，然后一个线程去进行读取，那么写入操作肯定会先行发生于读操作。

第四条规则实际上就是体现happens-before原则具备传递性。

**4.深入剖析volatile关键字**

在前面讲述了很多东西，其实都是为讲述volatile关键字作铺垫，那么接下来我们就进入主题。

**（1）volatile关键字的两层语义**

一旦一个共享变量（类的成员变量、类的静态成员变量）被volatile修饰之后，那么就具备了两层语义：

1）保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。

2）禁止进行指令重排序。

先看一段代码，假如线程1先执行，线程2后执行：

```java
//线程1
boolean stop = false;
while(!stop){
    doSomething();
}
//线程2
stop = true;
```

这段代码是很典型的一段代码，很多人在中断线程时可能都会采用这种标记办法。但是事实上，这段代码会完全运行正确么？即一定会将线程中断么？不一定，也许在大多数时候，这个代码能够把线程中断，但是也有可能会导致无法中断线程（虽然这个可能性很小，但是只要一旦发生这种情况就会造成死循环了）。

下面解释一下这段代码为何有可能导致无法中断线程。在前面已经解释过，每个线程在运行过程中都有自己的工作内存，那么线程1在运行的时候，会将stop变量的值拷贝一份放在自己的工作内存当中。

那么当线程2更改了stop变量的值之后，但是还没来得及写入主存当中，线程2转去做其他事情了，那么线程1由于不知道线程2对stop变量的更改，因此还会一直循环下去。

但是用volatile修饰之后就变得不一样了：

第一：使用volatile关键字会强制将修改的值立即写入主存；

第二：使用volatile关键字的话，当线程2进行修改时，会导致线程1的工作内存中缓存变量stop的缓存行无效（反映到硬件层的话，就是CPU的L1或者L2缓存中对应的缓存行无效）；

第三：由于线程1的工作内存中缓存变量stop的缓存行无效，所以线程1再次读取变量stop的值时会去主存读取。

那么在线程2修改stop值时（当然这里包括2个操作，修改线程2工作内存中的值，然后将修改后的值写入内存），会使得线程1的工作内存中缓存变量stop的缓存行无效，然后线程1读取时，发现自己的缓存行无效，它会等待缓存行对应的主存地址被更新之后，然后去对应的主存读取最新的值。

那么线程1读取到的就是最新的正确的值。

**（2）volatile保证原子性吗？**

从上面知道volatile关键字保证了操作的可见性，但是volatile能保证对变量的操作是原子性吗？

下面看一个例子：

```java
public class Test {
    public volatile int inc = 0;
     
    public void increase() {
        inc++;
    }
     
    public static void main(String[] args) {
        final Test test = new Test();
        for(int i=0;i<10;i++){
            new Thread(){
                public void run() {
                    for(int j=0;j<1000;j++)
                        test.increase();
                };
            }.start();
        }
         
        while(Thread.activeCount()>1)  //保证前面的线程都执行完
            Thread.yield();
        System.out.println(test.inc);
    }
}
```

大家想一下这段程序的输出结果是多少？也许有些朋友认为是10000。但是事实上运行它会发现每次运行结果都不一致，都是一个小于10000的数字。

可能有的朋友就会有疑问，不对啊，上面是对变量inc进行自增操作，由于volatile保证了可见性，那么在每个线程中对inc自增完之后，在其他线程中都能看到修改后的值啊，所以有10个线程分别进行了1000次操作，那么最终inc的值应该是1000*10=10000。

这里面就有一个误区了，volatile关键字能保证可见性没有错，但是上面的程序错在没能保证原子性。可见性只能保证每次读取的是最新的值，但是volatile没办法保证对变量的操作的原子性。

在前面已经提到过，自增操作是不具备原子性的，它包括读取变量的原始值、进行加1操作、写入工作内存。那么就是说自增操作的三个子操作可能会分割开执行，就有可能导致下面这种情况出现：

假如某个时刻变量inc的值为10，

线程1对变量进行自增操作，线程1先读取了变量inc的原始值，然后线程1被阻塞了；

然后线程2对变量进行自增操作，线程2也去读取变量inc的原始值，由于线程1只是对变量inc进行读取操作，而没有对变量进行修改操作，所以不会导致线程2的工作内存中缓存变量inc的缓存行无效，所以线程2会直接去主存读取inc的值，发现inc的值时10，然后进行加1操作，并把11写入工作内存，最后写入主存。

然后线程1接着进行加1操作，由于已经读取了inc的值，注意此时在线程1的工作内存中inc的值仍然为10，所以线程1对inc进行加1操作后inc的值为11，然后将11写入工作内存，最后写入主存。

那么两个线程分别进行了一次自增操作后，inc只增加了1。

解释到这里，可能有朋友会有疑问，不对啊，前面不是保证一个变量在修改volatile变量时，会让缓存行无效吗？然后其他线程去读就会读到新的值，对，这个没错。这个就是上面的happens-before规则中的volatile变量规则，但是要注意，线程1对变量进行读取操作之后，被阻塞了的话，并没有对inc值进行修改。然后虽然volatile能保证线程2对变量inc的值读取是从内存中读取的，但是线程1没有进行修改，所以线程2根本就不会看到修改的值。

根源就在这里，自增操作不是原子性操作，而且volatile也无法保证对变量的任何操作都是原子性的。

把上面的代码改成以下任何一种都可以达到效果：

```java
采用synchronized：
public class Test {
    public  int inc = 0;
    
    public synchronized void increase() {
        inc++;
    }
    
    public static void main(String[] args) {
        final Test test = new Test();
        for(int i=0;i<10;i++){
            new Thread(){
                public void run() {
                    for(int j=0;j<1000;j++)
                        test.increase();
                };
            }.start();
        }
        
        while(Thread.activeCount()>1)  //保证前面的线程都执行完
            Thread.yield();
        System.out.println(test.inc);
    }
}
采用Lock：
public class Test {
    public  int inc = 0;
    Lock lock = new ReentrantLock();
    
    public  void increase() {
        lock.lock();
        try {
            inc++;
        } finally{
            lock.unlock();
        }
    }
    
    public static void main(String[] args) {
        final Test test = new Test();
        for(int i=0;i<10;i++){
            new Thread(){
                public void run() {
                    for(int j=0;j<1000;j++)
                        test.increase();
                };
            }.start();
        }
        
        while(Thread.activeCount()>1)  //保证前面的线程都执行完
            Thread.yield();
        System.out.println(test.inc);
    }
}
采用AtomicInteger：
public class Test {
    public  AtomicInteger inc = new AtomicInteger();
     
    public  void increase() {
        inc.getAndIncrement();
    }
    
    public static void main(String[] args) {
        final Test test = new Test();
        for(int i=0;i<10;i++){
            new Thread(){
                public void run() {
                    for(int j=0;j<1000;j++)
                        test.increase();
                };
            }.start();
        }
        
        while(Thread.activeCount()>1)  //保证前面的线程都执行完
            Thread.yield();
        System.out.println(test.inc);
    }
}
```

在java 1.5的java.util.concurrent.atomic包下提供了一些原子操作类，即对基本数据类型的 自增（加1操作），自减（减1操作）、以及加法操作（加一个数），减法操作（减一个数）进行了封装，保证这些操作是原子性操作。atomic是利用CAS来实现原子性操作的（Compare And Swap），CAS实际上是利用处理器提供的CMPXCHG指令实现的，而处理器执行CMPXCHG指令是一个原子性操作。

**（3）volatile能保证有序性吗？**

在前面提到volatile关键字能禁止指令重排序，所以volatile能在一定程度上保证有序性。

volatile关键字禁止指令重排序有两层意思：

1）当程序执行到volatile变量的读操作或者写操作时，在其前面的操作的更改肯定全部已经进行，且结果已经对后面的操作可见；在其后面的操作肯定还没有进行；

2）在进行指令优化时，不能将在对volatile变量访问的语句放在其后面执行，也不能把volatile变量后面的语句放到其前面执行。
可能上面说的比较绕，举个简单的例子：

//x、y为非volatile变量

//flag为volatile变量

x = 2;     //语句1

y = 0;     //语句2

flag = true;  //语句3

x = 4;     //语句4

y = -1;    //语句5

由于flag变量为volatile变量，那么在进行指令重排序的过程的时候，不会将语句3放到语句1、语句2前面，也不会讲语句3放到语句4、语句5后面。但是要注意语句1和语句2的顺序、语句4和语句5的顺序是不作任何保证的。

并且volatile关键字能保证，执行到语句3时，语句1和语句2必定是执行完毕了的，且语句1和语句2的执行结果对语句3、语句4、语句5是可见的。
那么我们回到前面举的一个例子：

```java
//线程1:
context = loadContext();   //语句1
inited = true;             //语句2
//线程2:
while(!inited ){
  sleep()
}
doSomethingwithconfig(context);
```

前面举这个例子的时候，提到有可能语句2会在语句1之前执行，那么久可能导致context还没被初始化，而线程2中就使用未初始化的context去进行操作，导致程序出错。

这里如果用volatile关键字对inited变量进行修饰，就不会出现这种问题了，因为当执行到语句2时，必定能保证context已经初始化完毕。

**（4）volatile的原理和实现机制**

前面讲述了源于volatile关键字的一些使用，下面我们来探讨一下volatile到底如何保证可见性和禁止指令重排序的。

下面这段话摘自《深入理解Java虚拟机》：

“观察加入volatile关键字和没有加入volatile关键字时所生成的汇编代码发现，加入volatile关键字时，会多出一个lock前缀指令”

lock前缀指令实际上相当于一个内存屏障（也成内存栅栏），内存屏障会提供3个功能：

1）它确保指令重排序时不会把其后面的指令排到内存屏障之前的位置，也不会把前面的指令排到内存屏障的后面；即在执行到内存屏障这句指令时，在它前面的操作已经全部完成；

2）它会强制将对缓存的修改操作立即写入主存；

3）如果是写操作，它会导致其他CPU中对应的缓存行无效。

**（5）使用volatile关键字的场景**

synchronized关键字是防止多个线程同时执行一段代码，那么就会很影响程序执行效率，而volatile关键字在某些情况下性能要优于synchronized，但是要注意volatile关键字是无法替代synchronized关键字的，因为volatile关键字无法保证操作的原子性。通常来说，使用volatile必须具备以下2个条件：

1）对变量的写操作不依赖于当前值

2）该变量没有包含在具有其他变量的不变式中
实际上，这些条件表明，可以被写入 volatile 变量的这些有效值独立于任何程序的状态，包括变量的当前状态。

事实上，我的理解就是上面的2个条件需要保证操作是原子性操作，才能保证使用volatile关键字的程序在并发时能够正确执行。

下面列举几个Java中使用volatile的几个场景。

1.状态标记量

```java
volatile boolean flag = false;
while(!flag){
    doSomething();
}
public void setFlag() {
    flag = true;
}
volatile boolean inited = false;
//线程1:
context = loadContext();  
inited = true;            
//线程2:
while(!inited ){
sleep()
}
doSomethingwithconfig(context);
2.double check
class Singleton{
    private volatile static Singleton instance = null;
    private Singleton() {
         
    }
    public static Singleton getInstance() {
        if(instance==null) {
            synchronized (Singleton.class) {
                if(instance==null)
                    instance = new Singleton();
            }
        }
        return instance;
    }
}
```

 

# Java线程面试题之线程间的通信方式

一，介绍

本总结我对于JAVA多线程中线程之间的通信方式的理解，主要以代码结合文字的方式来讨论线程间的通信，故摘抄了书中的一些示例代码。
 
二，线程间的通信方式

**①同步**

这里讲的同步是指多个线程通过synchronized关键字这种方式来实现线程间的通信。

参考示例：

```java
public class MyObject {

    synchronized public void methodA() {
        //do something....
    }

    synchronized public void methodB() {
        //do some other thing
    }
}

public class ThreadA extends Thread {

    private MyObject object;
//省略构造方法
    @Override
    public void run() {
        super.run();
        object.methodA();
    }
}

public class ThreadB extends Thread {

    private MyObject object;
//省略构造方法
    @Override
    public void run() {
        super.run();
        object.methodB();
    }
}

public class Run {
    public static void main(String[] args) {
        MyObject object = new MyObject();

        //线程A与线程B 持有的是同一个对象:object
        ThreadA a = new ThreadA(object);
        ThreadB b = new ThreadB(object);
        a.start();
        b.start();
    }
}
```

由于线程A和线程B持有同一个MyObject类的对象object，尽管这两个线程需要调用不同的方法，但是它们是同步执行的，比如：线程B需要等待线程A执行完了methodA()方法之后，它才能执行methodB()方法。这样，线程A和线程B就实现了 通信。

这种方式，本质上就是“共享内存”式的通信。多个线程需要访问同一个共享变量，谁拿到了锁（获得了访问权限），谁就可以执行。

**②while轮询的方式**

代码如下：

```java
import java.util.ArrayList;
import java.util.List;

public class MyList {

    private List list = new ArrayList();
    public void add() {
        list.add("elements");
    }
    public int size() {
        return list.size();
    }
}

import mylist.MyList;

public class ThreadA extends Thread {

    private MyList list;

    public ThreadA(MyList list) {
        super();
        this.list = list;
    }

    @Override
    public void run() {
        try {
            for (int i = 0; i < 10; i++) {
                list.add();
                System.out.println("添加了" + (i + 1) + "个元素");
                Thread.sleep(1000);
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

import mylist.MyList;

public class ThreadB extends Thread {

    private MyList list;

    public ThreadB(MyList list) {
        super();
        this.list = list;
    }

    @Override
    public void run() {
        try {
            while (true) {
                if (list.size() == 5) {
                    System.out.println("==5, 线程b准备退出了");
                    throw new InterruptedException();
                }
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

import mylist.MyList;
import extthread.ThreadA;
import extthread.ThreadB;

public class Test {

    public static void main(String[] args) {
        MyList service = new MyList();

        ThreadA a = new ThreadA(service);
        a.setName("A");
        a.start();

        ThreadB b = new ThreadB(service);
        b.setName("B");
        b.start();
    }
}
```

在这种方式下，线程A不断地改变条件，线程ThreadB不停地通过while语句检测这个条件(list.size()==5)是否成立 ，从而实现了线程间的通信。但是这种方式会浪费CPU资源。之所以说它浪费资源，是因为JVM调度器将CPU交给线程B执行时，它没做啥“有用”的工作，只是在不断地测试 某个条件是否成立。就类似于现实生活中，某个人一直看着手机屏幕是否有电话来了，而不是： 在干别的事情，当有电话来时，响铃通知TA电话来了。关于线程的轮询的影响，可参考：JAVA多线程之当一个线程在执行死循环时会影响另外一个线程吗？

这种方式还存在另外一个问题：

轮询的条件的可见性问题，关于内存可见性问题，可参考：JAVA多线程之volatile 与 synchronized 的比较中的第一点“一，volatile关键字的可见性”

线程都是先把变量读取到本地线程栈空间，然后再去再去修改的本地变量。因此，如果线程B每次都在取本地的 条件变量，那么尽管另外一个线程已经改变了轮询的条件，它也察觉不到，这样也会造成死循环。

**③wait/notify机制**

代码如下：

```java
import java.util.ArrayList;
import java.util.List;

public class MyList {

    private static List list = new ArrayList();

    public static void add() {
        list.add("anyString");
    }

    public static int size() {
        return list.size();
    }
}


public class ThreadA extends Thread {

    private Object lock;

    public ThreadA(Object lock) {
        super();
        this.lock = lock;
    }

    @Override
    public void run() {
        try {
            synchronized (lock) {
                if (MyList.size() != 5) {
                    System.out.println("wait begin "
                            + System.currentTimeMillis());
                    lock.wait();
                    System.out.println("wait end  "
                            + System.currentTimeMillis());
                }
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}


public class ThreadB extends Thread {
    private Object lock;

    public ThreadB(Object lock) {
        super();
        this.lock = lock;
    }

    @Override
    public void run() {
        try {
            synchronized (lock) {
                for (int i = 0; i < 10; i++) {
                    MyList.add();
                    if (MyList.size() == 5) {
                        lock.notify();
                        System.out.println("已经发出了通知");
                    }
                    System.out.println("添加了" + (i + 1) + "个元素!");
                    Thread.sleep(1000);
                }
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

public class Run {

    public static void main(String[] args) {

        try {
            Object lock = new Object();

            ThreadA a = new ThreadA(lock);
            a.start();

            Thread.sleep(50);

            ThreadB b = new ThreadB(lock);
            b.start();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

线程A要等待某个条件满足时(list.size()==5)，才执行操作。线程B则向list中添加元素，改变list 的size。

A,B之间如何通信的呢？也就是说，线程A如何知道 list.size() 已经为5了呢？

这里用到了Object类的 wait() 和 notify() 方法。

当条件未满足时(list.size() !=5)，线程A调用wait() 放弃CPU，并进入阻塞状态。---不像②while轮询那样占用CPU

当条件满足时，线程B调用 notify()通知 线程A，所谓通知线程A，就是唤醒线程A，并让它进入可运行状态。

这种方式的一个好处就是CPU的利用率提高了。

但是也有一些缺点：比如，线程B先执行，一下子添加了5个元素并调用了notify()发送了通知，而此时线程A还执行；当线程A执行并调用wait()时，那它永远就不可能被唤醒了。因为，线程B已经发了通知了，以后不再发通知了。这说明：通知过早，会打乱程序的执行逻辑。

**④管道通信就是使用java.io.PipedInputStream 和java.io.PipedOutputStream进行通信**

具体就不介绍了。分布式系统中说的两种通信机制：共享内存机制和消息通信机制。感觉前面的①中的synchronized关键字和②中的while轮询 “属于” 共享内存机制，由于是轮询的条件使用了volatile关键字修饰时，这就表示它们通过判断这个“共享的条件变量“是否改变了，来实现进程间的交流。

而管道通信，更像消息传递机制，也就是说：通过管道，将一个线程中的消息发送给另一个。