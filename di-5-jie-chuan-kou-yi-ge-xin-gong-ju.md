# 第5节：串口——一个新工具

在上一个实验中，我们已经尝试了利用GPIO引脚作为单片机的输入输出，但是，电平高低的输入和输出只有两种状态，并不能满足我们对复杂输入输出的需求。比如，我们需要知道程序内某个变量当前的值，在电脑上编写C程序时，我们可以使用`printf`等函数输出它的值。而在Arduino上，是否存在这样的方式呢？答案是肯定的，这就是我们接下来要介绍的——串口。



## §5.1 串口介绍

> 串口全称串行接口，也称串行通信接口或串行通讯接口（通常指COM接口），是采用串行通信方式的扩展接口。串行接口 （Serial Interface）是指数据一位一位地顺序传送。其特点是通信线路简单，只要一对传输线就可以实现双向通信（可以直接利用电话线作为传输线），从而大大降低了成本，特别适用于远距离通信，但传送速度较慢。

又是百度百科，不过对我们来说，串口可以简单的理解成一个可以双向传输数据（通常是字符文本等）的通道，就像运行C语言程序时的控制台窗口，既可以输入，也可以显示输出，就像这样：

![C语言程序通过控制台的输入输出](.gitbook/assets/chap5\_img1\_cyuyanshurushuchu.png)

和上图中的情况差不多，串口也可以实现类似的输入输出功能，大概像这个样子：

![Arduino通过串口的输入输出](.gitbook/assets/chap5\_img2\_chuankoushurushuchu.png)

在下文中，我们将会介绍如何应用串口，让你的Arduino可以和你进行文字交流。在本章节的这两个实验中，你无需连接电路。



## §5.2 一个串口输出程序

下面给出我们将要运行的程序：[【src/chap5\_code1\_serialoutput/chap5\_code1\_serialoutput.ino】](https://www.jianguoyun.com/p/DQpVhxQQmcGwBxjsjpsE)

{% code title="chap5_code1_serialoutput.ino" %}
```arduino
void setup() {
  Serial.begin(9600);               // 开启串口
  delay(500);                       // 等待一段时间
}

void loop() {
  Serial.print("Hello, ");          // 输出一个字符串
  Serial.print(123);                // 输出一个数值
  Serial.println(" is a number.");  // 输出一个字符串并换行
  delay(500);
  Serial.print("Hahaha, ");         // 输出一个字符串
  Serial.println(456);              // 输出一个数值并换行
  delay(500);
}
```
{% endcode %}

这个程序运行时会不断在串口输出一些内容，先输出“`Hello, 123 is a number.`”，再换行输出“`Hahaha, 456`”。两行之间间隔500ms，如此反复。

程序中用到了几个Arduino的串口输出函数，利用这些函数，你就可以随意定制你想要输出在串口的内容：

> * `Serial.begin(波特率)`：开启串口，波特率是串口通信中的一个参数，为了便于理解，你可以把他理解为通讯频道，收发双方（即Arduino和电脑）的波特率需要一致才能通信。
> * `Serial.print(数值或字符串)`：在串口中输出一个数值或字符串。
> * `Serial.println(数值或字符串)`：在串口中输出一个数值或字符串，与`Serial.print`不同的是，`Serial.println`会在输出完之后添加一个换行。

上传程序之后，我们需要打开串口监视器才能看到Arduino通过串口输出的内容，如图，点击串口监视器按钮。

![Arduino IDE主界面](.gitbook/assets/chap5\_img3\_chuankoujianshiqi.png)

串口监视器的界面如图所示，打开串口监视器后，程序会自动开始执行，输出会显示在窗口的输出框中。

![串口监视器主界面](.gitbook/assets/chap5\_img4\_chuankoujianshiqijiemian.png)

{% hint style="info" %}
如果看不到输出或输出乱码等错误情况，请再次检查代码和相应配置是否和教程一致。如果你不能确定问题，请向我们反馈。
{% endhint %}



## §5.3 一个串口输入程序

仅仅能通过串口输入显然是不够的，接下来我们介绍如何通过串口向Arduino输入数据。

下面给出我们将要运行的程序：[【src/chap5\_code2\_serialinput/chap5\_code2\_serialinput.ino】](https://www.jianguoyun.com/p/DQpVhxQQmcGwBxjsjpsE)

{% code title="chap5_code2_serialinput.ino" %}
```arduino
int a, b;

void setup() {
  Serial.begin(9600);                     // 开启串口
  delay(500);                             // 等待一段时间
}

void loop() {
  Serial.print("Input two numbers: ");    // 输出提示信息
  while (!Serial.available());            // 等待直到串口收到数据
  a = Serial.parseInt();                  // 读取a和b的值
  b = Serial.parseInt();
  Serial.println();                       // 换行
  Serial.print("The sum of ");            // 输出a和b的和
  Serial.print(a);
  Serial.print(" and ");
  Serial.print(b);
  Serial.print(" is ");
  Serial.print(a + b);
  Serial.println(".");
  while (Serial.available())              // 清空剩下未读取的数据
    Serial.read();
}
```
{% endcode %}

这个程序运行时先在串口输出“`Input two numbers: `”，然后等待用户输入以空格分隔的两个数字。收到数字后，程序会换行输出两个数字的和“`The sum of xxx and xxx is xxx.`”。

程序中用到了几个Arduino的串口输入函数，结合之前的串口输出函数，你就可以完全发挥串口的交互功能：

> * `待处理的字符数量 = Serial.available()`：返回当前用户已经输入至Arduino，但还未读取的字符数量，可以用来判断当前串口是否已收到数据。
> * `数值 = Serial.parseInt()`：从串口中读取一个数值，返回读取的数值。
> * `字符 = Serial.read()`：从串口中读取一个字符，返回读取的字符（以ASCII码的形式）。

上传程序之后，打开串口监视器，在上方的输入框中输入两个数字，点击发送，就可以看到Arduino输出了数字和的结果，如图。

![Arduino通过串口的输入输出](.gitbook/assets/chap5\_img2\_chuankoushurushuchu.png)

{% hint style="info" %}
如果看不到输出或输出乱码等错误情况，请再次检查代码和相应配置是否和教程一致。如果你不能确定问题，请向我们反馈。
{% endhint %}



## §5.4 _更进一步_

任务：利用上述串口输出函数，实现一个按钮触发的输出装置，在按钮按下时输出一行文本“`Pong!`”。

提示：你可以参考[【§4.2】](di-4-jie-cong-shu-chu-dao-shu-ru.md#4.2-bian-xie-bing-shang-chuan-cheng-xu)中的程序，记得在按钮按下后的动作中添加延时以避免重复触发。

{% tabs %}
{% tab title="防剧透页" %}
【剧透警告！！！】
{% endtab %}

{% tab title="参考电路" %}
参考电路：

![参考电路](.gitbook/assets/chap5\_img5\_pong.png)
{% endtab %}

{% tab title="参考代码" %}
参考程序：[【src/chap5\_code3\_pong/chap5\_code3\_pong.ino】](https://www.jianguoyun.com/p/DQpVhxQQmcGwBxjsjpsE)

{% code title="chap5_code2_serialinput.ino" %}
```arduino
void setup() {
  pinMode(8, INPUT_PULLUP);           // 将D8引脚设置为输入上拉模式，以接受按钮输入
  Serial.begin(9600);                 // 开启串口
  delay(500);                         // 等待一段时间
}

void loop() {
  if (digitalRead(8) == LOW) {        // 如果按钮被按下
    Serial.println("Pong!");          // 输出一个字符串并换行
    delay(500);                       // 等待一会以防止重复触发
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
