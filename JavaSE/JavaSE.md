# JavaSE篇

## 目录

* [基础部分](### Q&S基础部分)

### Q&S基础部分

**Q1.JDK 和 JRE 有什么区别？**

JDK是功能齐全的Java软件开发包。JRE 是 Java运行时环境。<br>
JDK拥有JRE所拥有的一切，还有编译器（javac）和工具（如javadoc和jdb），它能够创建和编译程序。<br>
JRE 是 Java程序运行所需的内容的集合，它包含了 Java虚拟机（JVM），Java类库，java命令和其他的一些基础构件。但是，它不能用于创建新程序，只运行程序。<br>

**Q2.Java的基本类型有哪些？它们各自相对应的封装类又是什么？请说明int和它的封装类之间的区别。**

_Java的基本类型有8种：_<br>
整数型：byte、short、int、long（对应位数：8,16,32,64）<br>
浮点型：float、double（对应位数：32,64）<br>
字符型：char（对应位数：16）<br>
布尔类型：boolean<br>
<br>
_对应的封装类：_<br>
整数型包装类：Byte，Short，Integer，Long<br>
浮点型包装类：Float，Double<br>
字符型包装类：Character<br>
布尔类型包装类：Boolean<br>
<br>
_Integer与int的区别：_<br>
int 的默认值为0，而 Integer 的默认值为 null，即 Integer 可以区分出未赋值和值为0的区别，int 则无法表达出未赋值的情况。例如，要想表达出没有参加考试和考试成绩为0的区别，则只能使用 Integer。<br>

**Q3.请说出作用域 public，private，protected，以及default的区别**

流传的面试题中default经常被写成friendly，这两者没有区别，但是Java中没有friendly关键字。<br>
public：共有的，表明该数据对所有人开放，可以直接调<br>
private：私有的，可以理解为自己的私有财产，仅自己可以使用。<br>
protected：受保护的，可以理解为有一群人组成一个社团，这个社团里的人可以使用，后代也可以使用。这个社团就相当于一个包，在同一个包中的类便可以访问，子类也可以访问。<br>
default：默认的，在同一个包中的类可以访问，同一个包中的子类也可以访问，但是当子类在其他包中，就不能访问。<br>
![Java权限访问符](https://github.com/Zhang-Yixuan/Java-Basic-Knowledge/blob/master/resource/Java%E6%9D%83%E9%99%90%E8%AE%BF%E9%97%AE%E7%AC%A6.png)

**Q4.一个".java"源文件中是否可以包括多个类（不是内部类）？有什么限制？**

可以包含多个类，但是只有一个类可以使用public来修饰，并且文件名称必须与public修饰的类名称相同。

**Q5.switch 语句能否作用在 byte 上，能否作用在 long 上，能否作用在 String 上?**
 
switch表达式中，只能是int类型或者Integer或者枚举类型。byte、short、char可以隐式转换成int类型，因此可以使用这三种类型的表达式，那么long、String类型就不能应用。

**Q6.short s1 = 1; s1 = s1 + 1;有什么错? short s1 = 1; s1 += 1;有什么错?**

前者中s1+1会自动进行类型转换，结果是int型的，s1是short类型，将整型赋值给short型会出错。而后者中+=语句Java编译时会自动识别类型，并进行特殊处理，因此后者没有错误。

**Q7.用最有效率的方法算出 2 乘以 8 等于几?用最有效的方法算出奇数和偶数 ？**

2\*8=16，我们可以得到2的二进制位10，而16的二进制数为10000，发现2的二进制数中的1向左移动三位就可以得到16的二进制数。因此我们可以使用位移运算来快速计算2\<\<3。<br>
奇数的二进制数最后一位总是1，而偶数的二进制数总是0，因此我们可以使用与运算来进行奇偶数的识别。例如这个数为n，if((n&1)= =1)时，此数就是奇数；if((n&1)= =0)时，此数为偶数。<br>

**Q8.什么是引用类型？**

在Java中类型可分为两大类：值类型与引用类型。值类型就是基本数据类型（如int ,double 等），而引用类型，是指除了基本的变量类型之外的所有类型（如通过 class 定义的类型）。常用引用类型为数组，接口，类（尤其String类，最常见，最长考）。所有的类型在内存中都会分配一定的存储空间(形参在使用的时候也会分配存储空间,方法调用完成之后,这块存储空间自动消失)，基本的变量类型只有一块存储空间(分配在stack中), 而引用类型有两块存储空间(一块在stack中,一块在heap中)。<br>
![引用类型](https://github.com/Zhang-Yixuan/Java-Basic-Knowledge/blob/master/resource/%E5%BC%95%E7%94%A8%E7%B1%BB%E5%9E%8B.png)<br>
1） 引用是一种数据类型（保存在stack中），保存对象在内存（heap，堆空间）中的地址，这种类型即不是我们平时所说的简单数据类型也不是类实例(对象)；<br>
2） 不同的引用可能指向同一个对象，换句话说，一个对象可以有多个引用，即该类类型的变量。<br>
引用其实就像是一个对象的名字或者别名 (alias)，一个对象在内存中会请求一块空间来保存数据，根据对象的大小，它可能需要占用的空间大小也不等。访问对象的时候，我们不会直接是访问对象在内存中的数据，而是通过引用去访问。引用也是一种数据类型，我们可以把它想象为类似 C++ 语言中指针的东西，它指示了对象在内存中的地址——只不过我们不能够观察到这个地址究竟是什么。<br>
如果我们定义了不止一个引用指向同一个对象，那么这些引用是不相同的，因为引用也是一种数据类型，需要一定的内存空间（stack，栈空间）来保存。但是它们的值是相同的，都指示同一个对象在内存（heap，堆空间）的中位置。<br>

**Q9.== 和 equals 的区别是什么？**

对于基本类型和引用类型 == 的作用效果是不同的，基本类型：比较的是值是否相同；引用类型：比较的是引用是否相同；<br>
equals <br>
equals 本质上就是 ==，只不过 String 和 Integer 等重写了 equals 方法，把它变成了值比较。String 重写了 Object 的 equals 方法，把引用比较改成了值比较。

**Q10.Java 中操作字符串都有哪些类？它们之间有什么区别？**

主要是String、StringBuffer、StringBuild类。<br>
String 类是 final 类型的，因此不可以继承这个类、不能修改这个类，底层源码中有针对String数据的修改方法，都是重新创建了一个String对象，而原来的String对象未曾改变。对于字符串常量，如果内容相同，Java 认为它们代表同 一个 String 对象。而用关键字new调用构造器，总是会创建一个新的对象，无论内容是否相同。字符串如果是变量相加，先开空间，在拼接。字符串如果是常量相加，是先加，然后在常量池找，如果有就直接返回，否则，就创建。<br>
<br>
但是为了提高效率节省空间并且可以更改对String类型数据直接更改，我们可使用用StringBuffer 类。StringBuffer线程安全，同步，效率低，开销大，因此可以改用StringBuilder。StringBuilder线程不安全，异步，效率高。<br>

**Q11.什么是同步和异步？什么是线程安全？**

同步：可以理解为在执行完一个函数或方法之后，一直等待系统返回值或消息，这时程序是出于阻塞的，只有接收到返回的值或消息后才往下执行其他的命令。如打电话，通信双方不能断（我们是同时进行，同步），你一句我一句，这样的好处是，对方想表达的信息我马上能收到，但是，我在打着电话，我无法做别的事情。
<br>
异步：执行完函数或方法后，不必阻塞性地等待返回值或消息，只需要向系统委托一个异步过程，那么当系统接收到返回值或消息时，系统会自动触发委托的异步过程，从而完成一个完整的流程。如收发收短信，对方不用保证此刻我一定在手机旁，同时，我也不用时刻留意手机有没有来短信。这样的话，我看着视频，然后来了短信，我就处理短信（也可以不处理），接着再看视频。
<br>
线程安全：多个线程访问同一个对象时，如果不用考虑这些线程在运行时环境下的调度和交替执行，也不需要进行额外的同步，或者在调用方进行任何其他操作，调用这个对象的行为都可以获得正确的结果，那么这个对象就是线程安全的。一个类或者程序所提供的接口对于线程来说是[原子操作](https://baike.baidu.com/item/%E5%8E%9F%E5%AD%90%E6%93%8D%E4%BD%9C)或者多个线程之间的切换不会导致该接口的执行结果存在二义性,也就是说我们不用考虑同步的问题。
<br>
线程安全问题大多是由[全局变量](https://baike.baidu.com/item/%E5%85%A8%E5%B1%80%E5%8F%98%E9%87%8F)及[静态变量](https://baike.baidu.com/item/%E9%9D%99%E6%80%81%E5%8F%98%E9%87%8F)引起的，局部变量逃逸也可能导致线程安全问题。
<br>
若每个线程中对全局变量、静态变量只有读操作，而无写操作，一般来说，这个全局变量是线程安全的；若有多个线程同时执行写操作，一般都需要考虑[线程同步](https://baike.baidu.com/item/%E7%BA%BF%E7%A8%8B%E5%90%8C%E6%AD%A5)，否则的话就可能影响线程安全。

**Q12.String str1="i"与 String str2=new String(“i”)一样吗？**

String str2 = new String(“i”)会创建2（1）个对象，String str1 = “i”创建1（0）个对象。 <br>
==注==:当字符串常量池中有对象hello时括号内成立！<br>
str1 ==str2 的判断为false;<br>
str1 .equals(str2 )为true<br>

**Q13.String 类的常用方法都有那些？**

1、求字符串长度<br>
**public int length()**//返回该字符串的长度<br>
2、求字符串某一位置字符<br>
**public char charAt(int index)**//返回字符串中指定位置的字符；注意字符串中第一个字符索引是0，最后一个是length()-1。<br>
3、提取子串<br>
用String类的substring方法可以提取字符串中的子串，该方法有两种常用参数:<br>
1)**public String substring(int beginIndex)**//该方法从beginIndex位置起，从当前字符串中取出剩余的字符作为一个新的字符串返回。<br>
2)**public String substring(int beginIndex, int en<br>dIndex)**//该方法从beginIndex位置起，从当前字符串中取出到endIndex-1位置的字符作为一个新的字符串返回。<br>
4、字符串比较<br>
1)**public int compareTo(String anotherString)**//该方法是对字符串内容按字典顺序进行大小比较，通过返回的整数值指明当前字符串与参数字符串的大小关系。若当前对象比参数大则返回正整数，反之返回负整数，相等返回0。<br>
2)**public int compareToIgnore(String anotherString)**//与compareTo方法相似，但忽略大小写。<br>
3)**public boolean equals(Object anotherObject)**//比较当前字符串和参数字符串，在两个字符串相等的时候返回true，否则返回false。<br>
4)**public boolean equalsIgnoreCase(String anotherString)**//与equals方法相似，但忽略大小写。<br>
5、字符串连接<br>
**public String concat(String str)**//将参数中的字符串str连接到当前字符串的后面，效果等价于"+"。<br>
6、字符串中单个字符查找<br>
1)**public int indexOf(int ch/String str)**//用于查找当前字符串中字符或子串，返回字符或子串在当前字符串中从左边起首次出现的位置，若没有出现则返回-1。<br>
2)**public int indexOf(int ch/String str, int fromIndex)**//改方法与第一种类似，区别在于该方法从fromIndex位置向后查找。<br>
3)**public int lastIndexOf(int ch/String str)**//该方法与第一种类似，区别在于该方法从字符串的末尾位置向前查找。<br>
4)**public int lastIndexOf(int ch/String str, int fromIndex)**//该方法与第二种方法类似，区别于该方法从fromIndex位置向前查找。<br>
7、字符串中字符的大小写转换<br>
1)**public String toLowerCase()**//返回将当前字符串中所有字符转换成小写后的新串<br>
2)**public String toUpperCase()**//返回将当前字符串中所有字符转换成大写后的新串<br>
8、字符串中字符的替换<br>
1)**public String replace(char oldChar, char newChar)**//用字符newChar替换当前字符串中所有的oldChar字符，并返回一个新的字符串。<br>
2)**public String replaceFirst(String regex, String replacement)**//该方法用字符replacement的内容替换当前字符串中遇到的第一个和字符串regex相匹配的子串，应将新的字符串返回。<br>
3)**public String replaceAll(String regex, String replacement)**//该方法用字符replacement的内容替换当前字符串中遇到的所有和字符串regex相匹配的子串，应将新的字符串返回。<br>
9、其他类方法<br>
1)**String trim()**//截去字符串两端的空格，但对于中间的空格不处理。<br>
2)**boolean statWith(String prefix)**或**boolean endWith(String suffix)**//用来比较当前字符串的起始字符或子字符串prefix和终止字符或子字符串suffix是否和当前字符串相同，重载方法中同时还可以指定比较的开始位置offset。<br>
3)**regionMatches(boolean b, int firstStart, String other, int otherStart, int length)**//从当前字符串的firstStart位置开始比较，取长度为length的一个子字符串，other字符串从otherStart位置开始，指定另外一个长度为length的字符串，两字符串比较，当b为true时字符串不区分大小写。<br>
4)**contains(String** **str)**//判断参数s是否被包含在字符串中，并返回一个布尔类型的值。<br>
10、字符串转换为基本类型<br>
java.lang包中有Byte、Short、Integer、Float、Double类的调用方法：<br>
1)**public static byte parseByte(String s)**<br>
2)**public static short parseShort(String s)**<br>
3)**public static short parseInt(String s)**<br>
4)**public static long parseLong(String s)**<br>
5)**public static float parseFloat(String s)**<br>
6)**public static double parseDouble(String s)**<br>
11、基本类型转换为字符串类型<br>
String类中提供了String valueOf()放法，用作基本类型转换为字符串类型。<br>
1)**static String valueOf(char data[])**<br>
2)**static String valueOf(char data[], int offset, int count)**<br>
3)**static String valueOf(boolean b)**<br>
4)**static String valueOf(char c)**<br>
5)**static String valueOf(int i)**<br>
6)**static String valueOf(long l)**<br>
7)**static String valueOf(float f)**<br>
8)**static String valueOf(double d)**<br>
12、进制转换<br>
使用Long类中的方法得到整数之间的各种进制转换的方法：<br>
Long.toBinaryString(long l)<br>
Long.toOctalString(long l)<br>
Long.toHexString(long l)<br>
Long.toString(long l, int p)//p作为任意进制<br>
<br>
**Q14.如何将字符串反转？**

```java
import java.util.Scanner;
/**
 * 使用Java中的StringBuffer完成字符串的翻转
 * @author xuanxuan
 *
 */
public class ReverseString {
	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		//如果使用next()方法读取字符串时，读到空格就会停止，而使用nextLine()方法会读取空格
		String s = in.nextLine();
		System.out.println(ReverseString(s));
		in.close();
	}
	
	public static String ReverseString(String str) {
		StringBuffer bu = new StringBuffer();
		bu.append(str);
		String str1 = bu.reverse().toString();
		return str1;
	}
}
```

**Q15.final 在 java 中有什么作用？**

final 修饰的类叫最终类，该类不能被继承。<br>
final 修饰的方法不能被重写。<br>
final 修饰的变量叫常量，常量必须初始化，初始化之后值就不能被修改。<br>
使用 final 关键字修饰一个变量时，是指引用变量不能变，引用变量所指向的对象中的内容 还是可以改变的。<br>

**Q16.java 中的 Math.round(-1.5) 等于多少？**

Ceil向上取整，floor向下取整。Round先对一个树+0.5，然后向下取整。因此这个表达式的值为-1<br>

**Q17.是否可以从一个 static 方法内部发出对非 static 方法的调用？**

不可以。因为非 static 方法是要与对象关联在一起的，必须创建一个对象后，才可以在该对 象上进行方法调用，而 static 方法调用时不需要创建对象，可以直接调用。也就是说，当一 个 static 方法被调用时，可能还没有创建任何实例对象，如果从一个 static 方法中发出对非 static 方法的调用，那个非 static 方法是关联到哪个对象上的呢？这个逻辑无法成立，所以， 一个 static 方法内部发出对非 static 方法的调用。/<br>

**Q18.Overload 和 Override 的区别。Overloaded 的方法是否可以改变返回值的类型?**

Overload是重载的意思，Override是覆盖的意思，也就是重写。<br>

重载 Overload表示同一个类中可以有多个名称相同的方法，但这些方法的参数列表各不相同（即参数个数或类型不同）。<br>

重写 Override 表示子类中的方法可以与父类中的某个方法的名称和参数完全相同，通过子类创建的实例对象调用这个方法时，将调用子类中的定义方法，这相当于把父类中定义的那个完全相同的方法给覆盖了，这也是面向对象编程的多态性的一种表现。<br>

子类覆盖父类的方法时，只能比父类抛出更少的异常，或者是抛出父类抛出的异常的子异常，因为子类可以解决父类的一些问题，不能比父类有更多的问题。<br>

子类方法的访问权限只能比父类的更大，不能更小。如果父类的方法是 private 类型，那么，子类则不存在覆盖的限制，相当于子类中增加了一个全新的方法。<br>

如果几个 Overloaded 的方法的参数列表不一样，它们的返回者类型当然也可以不一样。如果两个方法的参数列表完全一样，是否可以让它们的返回值不同来实现重载 Overload?<br>

这是不行的，我们可以用反证法来说明这个问题， 因为我们有时候调用一个方法时也可以不定义返回结果变量，即不要关心其返回结果，例如，我们调用 map.remove(key)方法时，虽然 remove 方法有返回值，但是我们通常都不会定义接收返回结果的变量，这时候假设该类中有两个名称和参数列表完全相同的方法，仅仅是返回类型不同,java 就无法确定编程者倒底是想调用哪个方法了，因为它无法通过返回结果类型来判断。 override 可以翻译为覆盖，从字面就可以知道，它是覆盖了一个方法并且对其重写，以求达到不同的作用。对我们来说最熟悉的覆盖就是对接口方法的实现，在接口中一般只是对方法 进行了声明，而我们在实现时，就需要实现接口声明的所有方法。除了这个典型的用法以外， 我们在继承中也可能会在子类覆盖父类中的方法。<br>

在覆盖要注意以下的几点：<br>

1、覆盖的方法的标志必须要和被覆盖的方法的标志完全匹配，才能达到覆盖的效果；<br>
2、覆盖的方法的返回值必须和被覆盖的方法的返回一致；<br>
3、覆盖的方法所抛出的异常必须和被覆盖方法的所抛出的异常一致，或者是其子类；<br>
4、被覆盖的方法不能为 private，否则在其子类中只是新定义了一个方法，并没有对其进行覆盖。<br>

overload 对我们来说可能比较熟悉，可以翻译为重载，它是指我们可以定义一些名称相同的方法，通过定义不同的输入参数来区分这些方法，然后再调用时，VM 就会根据不同的参数样式，来选择合适的方法执行。在使用重载要注意以下的几点：<br>

1、在使用重载时只能通过不同的参数样式。例如，不同的参数类型，不同的参数个数，不同的参数顺序（当然，同一方法内的几个参数类型必须不一样，例如可以是 fun(int,float)， 但是不能为 fun(int,int)）； <br>
2、不能通过访问权限、返回类型、抛出的异常进行重载；<br>
3、方法的异常类型和数目不会对重载造成影响；<br>
4、对于继承来说，如果某一方法在父类中是访问权限是 priavte，那么就不能在子类对其进 行重载，如果定义的话，也只是定义了一个新方法，而不会达到重载的效果。<br>

**Q19.构造器 Constructor 是否可被 override?**

构造器 Constructor 不能被继承，因此不能重写 Override，但可以被重载 Overload。

**Q20.abstract class 和 interface 有什么区别?**

Abstract：<br>
* 含有 abstract 修饰符的 class 即为抽象类，abstract 类不能创建的实例对象。<br>
* 含有 abstract 方法的类必须定义为abstract class，abstract class类中的方法不必是抽象的。abstract class类中定义抽象方法必须在具体(Concrete)子类中实现，所以，不能有抽象构造方法或抽象静态方法。<br>
* 如果的子类没有实现抽象父类中的所有抽象方法，那么子类也必须定义为 abstract 类型。<br>

接口（interface）可以说成是抽象类的一种特例，接口中的所有方法都必须是抽象的。<br>
接口中的方法定义默认为 public abstract 类型，接口中的成员变量类型默认为public static final。<br>

两者区别：<br>
1.抽象类可以有构造方法，接口中不能有构造方法。 <br>
2.抽象类中可以有普通成员变量，接口中没有普通成员变量 <br>
3.抽象类中可以包含非抽象的普通方法，接口中的所有方法必须都是抽象的，不能有非抽象 的普通方法。<br>
4.抽象类中的抽象方法的访问类型可以是 public，protected 和（默认类型,虽然 eclipse 下不报错，但应该也不行），但接口中的抽象方法只能是 public 类型的，并且默认即 为 public abstract 类型。 <br>
5.抽象类中可以包含静态方法，接口中不能包含静态方法 <br>
6.抽象类和接口中都可以包含静态成员变量，抽象类中的静态成员变量的访问类型可以任 意，但接口中定义的变量只能是 public static final 类型，并且默认即为 public static final 类型。<br>
7.一个类可以实现多个接口，但只能继承一个抽象类。<br>

**Q21.接口是否可继承接口?抽象类是否可实现(implements)接口?抽象类是否可 继承具体类(concrete class)?抽象类中是否可以有静态的 main 方法？**

接口可以继承接口。抽象类可以实现(implements)接口，抽象类可以继承具体类。抽象类中 可以有静态的 main 方法。抽象类与普通类的唯一区别：就是不能创建实例对象和允许有 abstract 方法。<br>

**Q22.Java 中实现多态的机制是什么？**

靠的是父类或接口定义的引用变量可以指向子类或具体实现类的实例对象，而程序调用的方 法在运行期才动态绑定，就是引用变量所指向的具体实例对象的方法，也就是内存里正在运 行的那个对象的方法，而不是引用变量的类型中定义的方法。<br>

**Q22.说出一些常用的类，包，接口，请各举 5 个？**

常用的类：BufferedReader BufferedWriter FileReader FileWirter String Integer java.util.Date，System，Class，List,HashMap<br>
常用的包：java.lang java.io java.util java.sql,javax.servlet,org.hibernate<br>
常用的接口：Remote List Map Document NodeList,Servlet,HttpServletRequest,HttpServletResponse,Transaction(Hibernate)、 Session(Hibernate),HttpSession<br>

### Q&S集合部分

### Q&S线程部分

### Q&S反射部分

### Q&S异常部分

### Q&S对象拷贝部分

