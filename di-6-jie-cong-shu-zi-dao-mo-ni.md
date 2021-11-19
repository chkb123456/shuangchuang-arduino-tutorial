# 第6节：从数字到模拟

你可能会好奇，为什么对引脚电平进行读取和设置的函数名字叫“`digitalRead`”和“`digitalWrite`”呢？

函数名中“`digital`”是数字的意思，这个数字并非我们常说的数字，而是表示它的信号以数字形式产生和传输，其中最常见的数字形式就是二进制的高低电平，高电平5V表示1，低电平0V表示0，这样就能将信息编码成一连串的二进制0或1，输出或者输入。

然而，电压并非一个非0即5的值，在0V和5V之间，我们可以选取任意的电压，并让不同电压的大小表示一定的信息。假设我们用0V表示灯熄灭，5V表示灯完全亮起，则2.5V就可以表示灯发出弱光。

这样用连续电压代表信息的表示方式，就是模拟形式“`analog`”。当然，上面只是对模拟信号的一个不精确的比喻，不过已经足够你理解本节的内容。

## §6.1 电路连接

本次实验的电路和[【§3.1】](di-3-jie-cheng-xu-he-dian-lu-de-jie-he.md#3.1-dian-lu-lian-jie)的电路基本相同，注意这次的电路使用了D9引脚：

![](.gitbook/assets/chap6\_img1\_fading.png)

这里使用D9引脚的原因是，只有D3、D5、D6、D9、D10、D11可以用作模拟输出，你也可以将D9换成这6个引脚中的任何一个。

## §6.2 编写并上传程序

下面给出这次实验的程序：[【src/chap6\_code1\_fading/chap6\_code1\_fading.ino】](https://www.jianguoyun.com/p/DQpVhxQQmcGwBxjsjpsE)

{% code title="chap6_code1_fading.ino" %}
```arduino
void setup() {
  pinMode(9, OUTPUT);                   // 将D9引脚设为输出模式，只有D3、D5、D6、D9、D10、D11可以用作模拟输出
}

void loop() {
  // analogWrite将0V到5V的电压均分为256级，用0到255表示，0表示输出电压为0V，255表示输出电压为5V
  for (int i = 0; i <= 255; i += 5) {   // 这个循环的自变量i表示亮度，从0V到5V均匀变亮
    analogWrite(9, i);                  // 将亮度以模拟形式写入D9引脚
    delay(30);                          // 等待一会以让变量的过程可以被看到
  }
  for (int i = 255; i >= 0; i -= 5) {   // 这个循环的自变量i表示亮度，从5V到0V均匀熄灭
    analogWrite(9, i);                  // 将亮度以模拟形式写入D9引脚
    delay(30);                          // 等待一会以让变量的过程可以被看到
  }
}
```
{% endcode %}

## §6.3 _更进一步_

任务：让红色和绿色的两个LED交替亮灭，此处的亮灭需和上文实验一样具有呼吸效果。

提示：两个LED需要被分配到上文中提到的几个引脚中，才能实现呼吸效果。若其中一个的当前亮度是`i`，另一个的亮度可以是`255 - i`。

{% tabs %}
{% tab title="防剧透页" %}
【剧透警告！！！】
{% endtab %}

{% tab title="参考电路" %}
参考电路：

![](.gitbook/assets/chap6\_img2\_2fading.png)
{% endtab %}

{% tab title="参考代码" %}
参考代码：[【src/chap6\_code2\_2fading/chap6\_code2\_2fading.ino】](https://www.jianguoyun.com/p/DQpVhxQQmcGwBxjsjpsE)

{% code title="chap6_code2_2fading.ino" %}
```arduino
void setup() {
  pinMode(5, OUTPUT);                   // 将D5、D6引脚设为输出模式
  pinMode(6, OUTPUT);
}

void loop() {
  for (int i = 0; i <= 255; i += 5) {
    analogWrite(5, i);                  // D5从暗变亮
    analogWrite(6, 255 - i);            // D6从亮变暗
    delay(30);
  }
  for (int i = 255; i >= 0; i -= 5) {
    analogWrite(5, i);                  // D5从亮变暗
    analogWrite(6, 255 - i);            // D6从暗变亮
    delay(30);
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
