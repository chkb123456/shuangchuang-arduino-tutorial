# 第8节：初始传感器——麦克风的驱动

这个实验我们介绍一种新的外设——传感器，传感器和蜂鸣器这类外设不同，它的主要功能是从外部环境中获取数据，并将这些数据传到Arduino中供我们进行控制。

一些传感器比较简单，比如我们这次的麦克风，数据是通过简单的数字信号引脚或者模拟信号引脚输出的，我们直接读取引脚的信号就可以获得传感器的数据。但另一些传感器需要比较复杂的驱动手段，比如串口等，那就需要结合传感器的手册进行开发。

一旦掌握了一种传感器的驱动方法，我们就可以举一反三，通过其他传感器的文档，以及我们的经验和理解，快速掌握其他传感器的驱动方法。

## §8.1 电路连接

本次实验的电路如图：

![参考电路](.gitbook/assets/chap8\_img1\_microphone.png)

图上的模块就是麦克风模块，注意正反面，插反正反面会导致正负极短路或反接，可能会损坏电路。模块上的G引脚即我们之前提到的GND引脚，接电源负极或Arduino的GND引脚；+引脚是电源正极，接Ardunio上的3V3引脚。

这次的麦克风模块有两个输出引脚，分别是两侧的AO（模拟输出）和DO（数字输出），其中，AO以模拟形式输出当前声音的波形，DO以数字形式输出当前的声音是否大于一个特定的强度。本次实验中，我们只需要用到AO引脚。

模块上有一个蓝色的变阻器，变阻器上有一颗可以拧动的金色螺丝用于调节灵敏度，我们将在后面介绍调节方法。

电路连接完成后，将Arduino连接至电脑，模块右侧的电源灯应亮起。

## §8.2 编写并上传程序

本次实验的代码如下：[【src/chap8\_code1\_microphone/chap8\_code1\_microphone.ino】](https://www.jianguoyun.com/p/DQpVhxQQmcGwBxjsjpsE)

{% code title="chap8_code1_microphone.ino" %}
```arduino
int val;                  // 用变量保存从引脚读到的传感器数据

void setup() {
  Serial.begin(2000000);  // 开启串口，这里使用了串口支持的最高波特率2000000，从而达到最高的传输速度
  pinMode(A0, INPUT);     // 将A0引脚设为输入模式，只有A0~A7引脚可以用作模拟输入
  delay(500);
}

void loop() {
  // analogRead函数从所给的引脚中读取模拟信号，和analogWrite不同的是，
  // analogRead函数返回的模拟信号值范围为0~1023，将0~5V均分成了1024份
  val = analogRead(A0);   // 麦克风模块读取到的数据就是当前的声音波形
  Serial.println(val);    // 向串口输出当前读到的传感器数据
}
```
{% endcode %}

上面的程序仅用到了一个新函数：

> * `模拟信号值 = analogRead(引脚)`：函数从所给的引脚中读取模拟信号，和analogWrite不同的是，函数返回的模拟信号值范围为0\~1023，将0\~5V均分成了1024份。

这个程序中，我们将串口的波特率设置为2000000，波特率此处也决定了串口数据传输的速度，将波特率设置到最高是为了尽可能快速地获得传感器的数据。

编译并上传这个程序，打开串口监视器并设置波特率为2000000，我们能看到不断有数字从串口发来，这个数字就是我们从模块读到的数据，如图。

![串口收到的传感器数据](.gitbook/assets/chap8\_img2\_chuanganqishuju.png)

接下来我们将使用串口绘图器工具展示系统的波形。点击串口绘图器，打开串口绘图器界面，如图。

![打开串口绘图器](.gitbook/assets/chap8\_img3\_chuankouhuituqi.png)

设置波特率为2000000，可以看到，串口绘图器以折线图的方式展示了串口发来数据的波形，这个波形就是当前麦克风收到的声音的波形。

![串口绘图器界面](.gitbook/assets/chap8\_img4\_chuankouhuituqijiemian.png)

接下来我们对灵敏度进行调整。在一个较安静的场合，使用一字螺丝刀或薄而硬的卡片旋转螺丝，顺时针旋转时波形降低，逆时针旋转时波形升高，小心调整使波形的平均值稳定在300附近，灵敏度调节完成。

尝试在麦克风模块边说话或者制造声音，观察波形的变化，或者使用手机等播放音乐，观察波形随音乐节奏和响度变化的情况。

## §8.3 _更进一步_

* **任务**：关于麦克风本身，其实并没有更多复杂的用法了，在更多情况下，我们需要结合麦克风之类的传感器数据，进行相应的控制。本次任务要求你设计一个声控灯，当检测到声音时，点亮LED1秒钟。
* **提示**：结合之前的电路设计此次任务的电路，如果你按照上文300的水平调整了麦克风模块的灵敏度，此时可以使用360作为你判断声音大小的阈值。如果超过360，则点亮LED。

{% tabs %}
{% tab title="防剧透页" %}
**【剧透警告！！！】**
{% endtab %}

{% tab title="参考电路" %}
参考电路：

![参考电路](.gitbook/assets/chap8\_img5\_soundlight.png)
{% endtab %}

{% tab title="参考代码" %}
参考代码：[【src/chap8\_code2\_soundlight/chap8\_code2\_soundlight.ino】](https://www.jianguoyun.com/p/DQpVhxQQmcGwBxjsjpsE)

{% code title="chap8_code2_soundlight.ino" %}
```arduino
int val;                    // 用变量保存从引脚读到的传感器数据

void setup() {
  Serial.begin(2000000);    // 开启串口
  pinMode(7, OUTPUT);       // 将D7引脚设为输出模式，连接LED
  pinMode(A0, INPUT);       // 将A0引脚设为输入模式，连接麦克风模块
  digitalWrite(7, LOW);     // 初始状态下LED为熄灭状态
  delay(500);
}

void loop() {
  val = analogRead(A0);     // 读取当前的声音强度
  Serial.println(val);      // 向串口输出当前读到的传感器数据
  if (val >= 360) {         // 如果声音强度达到一定水平
    digitalWrite(7, HIGH);  // 点亮LED 1秒钟
    delay(1000);
    digitalWrite(7, LOW);
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
