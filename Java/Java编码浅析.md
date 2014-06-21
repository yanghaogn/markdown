#<center><element style="margin:0em 0px 12px; padding:0px; font-family:Arial;   color:rgb(32,136,178); line-height:32px">Java字符串编码</element></center>
##<element style="margin:0em 0px 12px; padding:0px; font-family:Arial;   color:rgb(32,136,178); line-height:32px">概念</element>
1. Java字符串采用的编码：unicode。Java的String永远都是unicode编码的。Java以UTF-16作为内存的字符存储格式。因为UTF-16用两个字节表示一个字符，非常方便，编码效率非常高。
2. JVM平台默认字符集：读取数据的默认编码方式，由JVM启动时确定，一般由语言环境和操作系统底层决定
3. 外部资源的编码：外部资源编码一般和JVM默认编码字符集不一样，读取时，需要指定外部资源编码
4. 解码&&解码：
>编码：字符串变成字节数组；String -->byte[];  （String类的方法）：str.getBytes(charsetName);；将该字符串按照指定编码表编码。
解码：字节数组变成字符串；byte[] -->String;  （String类的构造方法）：new String(byte[],charsetName);通过使用指定的charset解码指定的 byte 数组。
5. 强烈的不建议使用操作系统的默认编码，因为这样，你的应用程序的编码格式就和运行环境绑定起来了，在跨环境下很可能出现乱码问题。

##<element style="margin:0em 0px 12px; padding:0px; font-family:Arial;   color:rgb(32,136,178); line-height:32px">从外部读取字符串</element>
这跟外部资源采取的编码方式有关，我们需要使用外部资源采用的字符集来读取外部数据：
>InputStream is = new FileInputStream("res/input2.data");   
InputStreamReader streamReader = new InputStreamReader(is, "GB18030"); 

##<element style="margin:0em 0px 12px; padding:0px; font-family:Arial;   color:rgb(32,136,178); line-height:32px">字符串转换</element>
下面两个语句是等价的
>"string".getBytes(); 
"string".getBytes(Charset.defaultCharset());  

也就是说它根据JVM的默认编码（而不是你可能以为的unicode）把字符串转换成一个字节数组。
##<element style="margin:0em 0px 12px; padding:0px; font-family:Arial;   color:rgb(32,136,178); line-height:32px">举例</element>
>new String(input.getBytes("ISO-8859-1"), "GB18030")  

我们本应该用GB18030的编码来读取数据并解码成字符串，但结果却采用了ISO-8859-1的编码，导致生成一个错误的字符串。要恢复，就要先把字符串恢复成原始字节数组，然后通过正确的编码GB18030再次解码成字符串（即把以GB18030编码的数据转成unicode的字符串）。注意，字符串永远都是unicode编码的。
##<element style="margin:0em 0px 12px; padding:0px; font-family:Arial;   color:rgb(32,136,178); line-height:32px">几种编码方式比较</element>
1. 对中文字符后面四种编码格式都能处理，GB2312 与 GBK 编码规则类似，但是 GBK 范围更大，它能处理所有汉字字符，所以 GB2312 与 GBK 比较应该选择 GBK。
2. UTF-16 与 UTF-8 都是处理 Unicode 编码，它们的编码规则不太相同，相对来说 UTF-16 编码效率最高，字符到字节相互转换更简单，进行字符串操作也更好。它适合在本地磁盘和内存之间使用，可以进行字符和字节之间快速切换，如 Java 的内存编码就是采用 UTF-16 编码。但是它不适合在网络之间传输，因为网络传输容易损坏字节流，一旦字节流损坏将很难恢复
3. 想比较而言 UTF-8 更适合网络传输，对 ASCII 字符采用单字节存储，另外单个字符损坏也不会影响后面其它字符，在编码效率上介于 GBK 和 UTF-16 之间，所以 UTF-8 在编码效率上和编码安全性上做了平衡，是理想的中文编码方式。
