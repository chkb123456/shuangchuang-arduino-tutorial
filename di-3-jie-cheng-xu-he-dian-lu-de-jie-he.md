# 第3节：程序和电路的结合

在上一个实验中，我们已经掌握了如何连接一个电路，接下来，我们需要将Arduino的程序和我们的电路结合到一起，让我们能一窥程序控制是如何与真实世界的电路交互的。

## §3.1 电路连接

我们要连接的电路如下图，请按和上一节实验中一样的方法连接电路。

![电路示意图](.gitbook/assets/chap3\_img1\_blink.png)

{% hint style="info" %}
**再次强调：在连接电路时，务必断开Arduino到电脑的连接，以免在通电的情况下错误连接导线烧毁元件。**
{% endhint %}

可以看到，这个电路和上个实验的电路相似，唯一的区别是，原本一端接在Arduino上侧3V3引脚的红色导线，现在接到了Arduino下侧的D7引脚。D7引脚和其他的D2\~D13引脚一样，属于Arduino的GPIO接口（通用输入输出接口），通俗地说，就是Arduino可以通过程序控制的接口。

连接好电路之后，将Arduino连接至电脑，准备开始编写并上传程序。

## §3.2 编写并上传程序

下面给出我们将要运行的程序：[【src/chap3\_code1\_blink/chap3\_code1\_blink.ino】](https://www.jianguoyun.com/p/DQpVhxQQmcGwBxjsjpsE)

{% code title="chap3_code1_blink.ino" %}
```arduino
// setup函数将在单片机通电时自动开始运行，仅运行一次
void setup() {
  // 设置7号引脚（也就是D7引脚）为输出模式
  // 输出模式的意思是这个引脚用于输出电压（高电平5V或低电平0V），这样引脚就可以点亮LED或控制蜂鸣器发声等
  pinMode(7, OUTPUT);
}

// loop函数将在setup函数运行完成后不断循环运行
void loop() {
  digitalWrite(7, HIGH);  // 设置D7引脚的输出为HIGH，即高电平5V，此时LED亮
  delay(1000);            // 等待1秒
  digitalWrite(7, LOW);   // 设置D7引脚的输出为LOW，即低电平0V，此时LED熄灭
  delay(1000);            // 等待1秒
}
```
{% endcode %}

这个程序的功能和第一节的实验相似，不同的是，在运行时，它将会闪烁面包板上连接的LED。

程序的代码和第一节的代码相比，唯一的区别是，调用`pinMode`和`digitalWrite`函数的时候，传入的参数不再是`LED_BUILTIN`，而是7。7代表7号引脚，也就是Ardunio上印着D7的引脚。这样的引脚都可以通过`pinMode`指定输入输出模式，并使用`digitalWrite`设置其输出的电平高低。

接下来，按照第一节实验中同样的过程，将代码上传到Arduino。如果一切正常，你将会看到之前接在面包板上的LED开始闪烁。

{% hint style="info" %}
如果没能成功，请再次检查接线、引脚以及代码是否和教程一致。如果你不能确定问题，请向我们反馈。
{% endhint %}

## §3.3 _更进一步_

* **任务**：设计一个红绿灯电路，并为其编写程序，你可以假定绿灯、黄灯、红灯交替亮起，各亮一秒。
* **提示**：在这个电路中，你需要用到红黄绿三色的LED，以及3个电阻。你需要为3个LED分配不同的GPIO引脚，D或A开头的引脚都可以用。

{% tabs %}
{% tab title="防剧透页" %}
**【剧透警告！！！】**
{% endtab %}

{% tab title="参考电路" %}
参考电路：

![参考电路](.gitbook/assets/chap3\_img2\_traffic.png)
{% endtab %}

{% tab title="参考代码" %}
参考代码：[【src/chap3\_code2\_traffic/chap3\_code2\_traffic.ino】](https://www.jianguoyun.com/p/DQpVhxQQmcGwBxjsjpsE)

{% code title="chap3_code2_traffic.ino" %}
```arduino
void setup() {
  pinMode(7, OUTPUT);     // D7引脚连接红色LED
  pinMode(8, OUTPUT);     // D8引脚连接黄色LED
  pinMode(9, OUTPUT);     // D9引脚连接绿色LED
  digitalWrite(7, LOW);   // 红灯灭
  digitalWrite(8, LOW);   // 黄灯灭
  digitalWrite(9, LOW);   // 绿灯灭
}

void loop() {
  digitalWrite(9, HIGH);  // 绿灯亮
  digitalWrite(7, LOW);   // 红灯灭
  delay(1000);            // 等待1秒

  digitalWrite(8, HIGH);  // 黄灯亮
  digitalWrite(9, LOW);   // 绿灯灭
  delay(1000);            // 等待1秒
  
  digitalWrite(7, HIGH);  // 红灯亮
  digitalWrite(8, LOW);   // 黄灯灭
  delay(1000);            // 等待1秒
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
