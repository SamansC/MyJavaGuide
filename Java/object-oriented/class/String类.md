Java 没有内置的字符串类型，而是在标准 Java 类库中提供了一个预定义类，叫做 String 类。String 被声明为 final，因此它不可被继承。

在 Java 8 中，String 内部使用 char 数组存储数据。

```java
public final class String
  implements java.io.Serializable, Comparable<String>, CharSequence {
  /** The value is used for character storage. */
  private final char value[];
}
```

在 Java 9 之后，String 类的实现改用 byte 数组存储字符串，同时使用 `coder` 来标识使用了哪种编码。

```java
public final class String
  implements java.io.Serializable, Comparable<String>, CharSequence {
  /** The value is used for character storage. */
  private final byte[] value;

  /** The identifier of the encoding used to encode the bytes in {@code value}. */
  private final byte coder;
}
```

value 数组被声明为 final，这意味着 value 数组初始化之后就不能再引用其它数组。并且 String 内部没有改变 value 数组的方法，因此可以保证 String 不可变。

----

## 创建字符串的方式

1. String str1 = “Hello World!”;
2. String str2 = new String();
3. String str3 = new String(“Hello World!”)

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gd0h5a750wj30ww0nigpp.jpg" alt="newname2020-03-2016.22.59" style="zoom:50%;" />

----

## 常用 API

**示例1：遍历字符串的每个字符**

String 类底层使用 char value[] 字符数组来存储字符串中的每个字符。通过 charAt(index) 返回字符串指定位置的字符。 length() 返回字符串中字符的数量， 数组有一个 length 属性返回数组元素的数量，String 类中使用 length() 方法。通过 for 循环遍历字符串的每个字符。如：

```java
String text = “helloworld”;
for( int i = 0 ; i < text.length(); i++ ){
  System.out.print( text。charAt(i) );
}
System.out.println();
```



**示例2：比较字符串大小。**

String 类实现了 Comparable 接口， 重写了 compareTo() 方法，可以比较两个字符串大小。Java 把比较两个对象大小的功能封装在 Comparable 接口中，Comparable 接口定义如下：

```java
public interface Comparable {
  public int compareTo(T o);
}
```

Comparable 接口后面的< T >是泛型， 泛型就是把数据类型作为参数传递，当某个类实现 Comparable 接口时，通过泛型指定比较元素的数据类型，如 String 类实现 Comparable 接口时，通过泛型传递了 String 类型，指定了比较元素的数据类型为 String。 String 类重写了 compareTo(String) 方法，把两个字符串逐个字符进行比较，遇到第一个不相同的字符，码值相减。如果第一个字符串比参数字符串大返回正数， 两个字符串一样返回 0 ，参数字符串大返回负数。如：

```java
System.out.println( "hello".compareTo("hehe"));       //4，正数说明第一个字符串大
System.out.println("hello".compareTo("helloworld"));  //-5， 负数说明第二个字符串大
System.out.println("lisi".compareTo("lisi"));  //0
```

​    在比较字符串大小时，还可以忽略大小写，如：

```java
  System.out.println("hello".compareToIgnoreCase("HELLO"));  //0
```

 

**示例3：取子串substring()**

调用 charAt(int) 方法可以返回指定索引值的字符，调用 substring() 方法可以从字符串中提取子串，substring 方法有两个方法重载：

1. **substring( from )** 返回当前字符串从 from 开始一直到最后的子串
2. **substring( from,to)** 返回当前字符串 [from,to) 范围内的子串，注意 from 起始位置的字符可以取到，to 终止位置的字符取不到

在取子串时，经常结合 indexOf()、lastIndexOf() 方法一起使用。 indexOf() 返回字符串或字符在当前字符串中第一次出现的索引值； lastIndexOf() 返回字符串或字符在当前字符串中最后一次出现的索引值。 如把一个路径中的文件夹，文件名与扩展名分别取出来：     

```java
path="D:\\course\\03-JavaSE\\code\\day06 \\src\\string\\Test02.java";
//需要确定\\最后一次出现的索引值，
int lastSlash = path.lastIndexOf("\\");
//需要确定小点的位置
int dot = path.indexOf('。');
//调用方法取子串
String folder = path.substring(0,lastSlash);         //文件夹
String filename = path.substring(lastSlash + 1,dot); //文件名
String suffix = path.substring(dot + 1 );       		 //扩展名
```



**示例4：字符串与字节数组之间的转换**

​     String 类提供的 getBytes() 返回字符串在当前默认编码下对应的字节数组，注意，在不同编码中字符串对应的字节数组可能不一样。如

```java
text = "hello我的世界";
//返回text字符串在默认UTF8编码下对应的字节数组，在UTF8编码中，一个英文占1字节，一个汉字占3字节
byte[] bytes = text.getBytes();   
System.out.println(Arrays.toString(bytes));
String s = new String(bytes);
```

​    也可以把字符串转换为指定编码的字节数组，如返回 text 字符串在 GBK 编码下对应的字节数组，在 GBK 编码中一个英文字符占 1 字节， 一个汉字占 2 字节。

```java
bytes = text.getBytes("GBK");    //调用getBytes(String)方法时，需要对getBytes()方法的受检异常进行预处理
System.out.println(Arrays.toString(bytes)); 
//把bytes数组中的字节以GBK编码转换为字符串
s = new String(bytes， "GBK");  
System。out。println(s);        // hello动力节点
```



**示例5： 字符串与字符数组的相互转换**

String 字符串可以调用 toCharArray() 方法把当前字符串转换为字符数组

```java
char[] charArray = text.toCharArray();
```

可以调用 String 构造方法把字符数组转换为字符串

```java
s = new String(charArray);   
```

也可以调用 String.valueOf(char[]) 方法把字符数组转换为字符串

```java
s = String.valueOf(charArray);   
```

 

**示例6： 字符串与基本类型的相互转换**

​     String 类提供了一组 valueOf() 方法的重载，可以把 int， double， boolean，char，char[] 等类型的数据转换为字符串

```java
int num = 456;
s = String.valueOf(num);    //可以把整数转换为字符串
```

​    可以调用 Integer.parseInt(text)， Double.parseDouble(text) 等方法可以把数值字符串转换为 int 整数，double 小数

```java
s = "258";
num = Integer.parseInt(s);   //把字符串转换为整数
```

 

**示例7： 字符串替换与分隔**

String类提供一组与正则表达式相关的方法 ： matches()， replaceAll()， split()。正则表达式就是一个模式串， 用于验证字符串是否匹配指定的模式，常用的模式符有：

- [abc]    匹配a，b，c三个字符中的一个
- .        任意字符
- \d     数字
- \s     空白符
- \w     单词字符[a-z A-Z 0-9_]
- X?     匹配 X ，0 次或 1 次
- X*     匹配 X 任意次
- X+     匹配 X 至少 1 次
- X{n}    匹配 X 正好 n 次
- X{n,}    匹配 X 至少 n 次
- X{n,m}   匹配X至少n次，最多m次

使用 matches(pattern) 判断字符串是否匹配指定的模式，如验证字符串是否是匹配规范的邮箱格式， 先定义邮箱的模式串：

```java
String pattern = "\\w{6,20}@\\w{2,}\\.(com|cn|net)";       
String email = "hehe@qq.com";
System.out.println(email.matches(pattern));
```

调用 replaceAll( 模式串， str ) 方法可以把当前字符串中符合指定模式的字符串使用 str 替换，如把字符串中的数字替换为星号*：

```java
String text = "hello1234world";      
text = text.replaceAll("\\d", "*");
System.out.println(text);       //hello****world
```

调用 split(regex) 方法可以把当前字符串使用 regex 作为分割符拆分为若干小的字符串，如把 text 字符串中的每个单词给拆分出来， 字符串中单词与单词使用空格或者逗号分隔

```java
text = "A small step forward,A big step of civilization";
String[] words = text.split("[ ,]");
for(String str : words ){
  System.out.println(str);
}
```

----

## 不可变性

String 的对象一旦被创建，则不能修改，是不可变的，所谓的修改其实是创建了新的对象，所指向的内存空间不变。不可变字符串有一个优点：编译器可以让字符串**共享**。

#### 不可变的好处

**1. 可以缓存 hash 值**

因为 String 的 hash 值经常被使用，例如 String 用做 HashMap 的 key。不可变的特性可以使得 hash 值也不可变，因此只需要进行一次计算。

**2. String Pool 的需要**

如果一个 String 对象已经被创建过了，那么就会从 String Pool 中取得引用。只有 String 是不可变的，才可能使用 String Pool。

**3. 安全性**

String 经常作为参数，String 不可变性可以保证参数不可变。例如在作为网络连接参数的情况下如果 String 是可变的，那么在网络连接过程中，String 被改变，改变 String 的那一方以为现在连接的是其它主机，而实际情况却不一定是。

**4. 线程安全**

String 不可变性天生具备线程安全，可以在多个线程中安全地使用。

----

## String Pool

字符串常量池（String Pool）保存着所有字符串字面量（literal strings），这些字面量在编译时期就确定。不仅如此，还可以使用 String 的 intern() 方法在运行过程将字符串添加到 String Pool 中。

在 Java 7 之前，String Pool 被放在运行时常量池中，它属于永久代。而在 Java 7，String Pool 被移到堆中。这是因为永久代的空间有限，在大量使用字符串的场景下会导致 OutOfMemoryError 错误。

----

## StringBuffer 和 StringBuilder

Sting 是典型的不可变（Immutable）类，被声明为 final class，所有属性也都是 final 的。也由于它的不可变性，类似拼接、裁剪字符串等工作，都会产生新的 String 对象。由于字符串操作的普遍性，所以相关操作的效率往往对应用性能有明显的影响。

StringBuffer 是为了解决拼接产生太多中间对象的问题而提供的一个类，可以使用它的 append() 或 add() 方法，将字符串添加到已有序列的末尾或指定位置。它是线程安全的（通过把各种修改数据的方法都加上 synchronized 关键字实现），随之也带来了额外的性能开销。

StringBuilder 是 Java 1.5 中新增的，能力上与 StringBuffer 没有本质区别，但是它去掉性能安全的部分，减小了额外的开销，也是绝大部分情况进行字符串拼接的首选。

> 为了实现修改字符序列的目的，StringBuffer 和 StringBuilder 底层都是利用可以修改的数组（char，JDK 9 之后是 byte），二者都继承了 AbstractStringBuilder，区别仅在于最终的方法是否加了 sychronized。
>
> 构建时，内部数组大小为：**初始字符串长度 +16**，如果没有构建对象时，内部数组大小初始值为：**16**。如果确定会拼接多次且大小是可以预计的，最好指定初始化容量，可以避免多次扩容。

**对于三者使用的总结：**

1. 操作少量的数据: 适用 String
2. 单线程操作字符串缓冲区下操作大量数据: 适用 StringBuilder
3. 多线程操作字符串缓冲区下操作大量数据: 适用 StringBuffer