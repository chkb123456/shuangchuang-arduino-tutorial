# 第1节：第一个程序

什么是单片机？单片机是如何运行的？单片机运行时有什么效果？

![](.gitbook/assets/chap1\_img1\_baidubaike.png)

百度百科总能告诉我们一些正确但是无助于理解的东西，在这次实验中，我们将通过实际运行一个代码来理解单片机是什么。

## §1.1 电路连接

在第一个实验中，我们不需要使用面包板、杜邦线和各种模块，只需要连接Arduino和电脑。

* 将你手上的USB Mini连接线一端插入Arduino的接口，另一端连接到你的电脑。
* 稍等片刻，应该能听到系统的硬件接入提示音，同时，Arduino上的电源灯会亮起，用户灯可能会闪烁。此时Arduino的电源已经接通。

![](.gitbook/assets/chap1\_img2\_nanojiegou.png)

{% hint style="info" %}
如果灯不亮，请向我们反馈。
{% endhint %}

* 打开设备管理器（右键点击任务栏Windows徽标按钮），选择“设备管理器”。
* 你应该能在“端口 (COM 和 LPT)”一栏下看到名为“USB-SERIAL CH340 (COM3)”的设备，如图所示。此时你的Arduino已经正常连接。**根据接口不同，你看到的设备名称中“COM”后的数字序号可能不同，这没有关系，但是请记下此处的数字序号，我们将在后续步骤中用到它。**

![](.gitbook/assets/chap1\_img3\_shebeiguanliqi.png)

{% hint style="info" %}
如果你没有找到这个设备，或者设备的图标带有叹号标志，或者系统提示无法识别的USB设备等，请确认已按[【§0.2】](di-0-jie-zhun-bei.md#0.2-huan-jing-pei-zhi)的说明安装了驱动，如果问题仍无法解决，请向我们反馈。
{% endhint %}

## §1.2 第一个程序

下面给出我们将要运行的程序：[【src/chap1\_code1\_blink/chap1\_code1\_blink.ino】](https://www.jianguoyun.com/p/DQpVhxQQmcGwBxjsjpsE)

```arduino
// setup函数将在单片机通电时自动开始运行，仅运行一次
void setup() {
  // 设置LED_BUILTIN引脚（这个引脚连接至Arduino上的用户指示灯）为输出模式
  // 输出模式的意思是这个引脚用于输出电压（高电平5V或低电平0V），这样引脚就可以点亮LED或控制蜂鸣器发声等
  pinMode(LED_BUILTIN, OUTPUT);
}

// loop函数将在setup函数运行完成后不断循环运行
void loop() {
  digitalWrite(LED_BUILTIN, HIGH);   // 设置LED_BUILTIN引脚的输出为HIGH，即高电平5V，此时用户指示灯亮
  delay(1000);                       // 等待1秒
  digitalWrite(LED_BUILTIN, LOW);    // 设置LED_BUILTIN引脚的输出为LOW，即低电平0V，此时用户指示灯熄灭
  delay(1000);                       // 等待1秒
}
```

这个程序的功能非常简单，在运行时，它将会以2秒为周期闪烁Arduino板载的用户指示灯，1秒亮，1秒灭。

Arduino程序的语法和C或者C++非常相似，不一样的是Arduino程序通过setup函数和loop函数运行，而非main函数。在Arduino接通电源时，setup函数会被先执行一次，用来进行一些配置和内容的初始化，之后，loop函数会被不断循环运行。这里循环运行的意思是，loop函数运行完毕之后，程序会自动回到loop函数的开头重新开始函数的运行，这个过程永远不会终止。

你可能会好奇，为什么要让loop函数不断循环下去呢？比较简单的解释就是，大部分场合下，Arduino之类的单片机所负责的工作都是持续地对一些东西进行控制，这种控制通常不是只进行几次就可以结束的，而常常需要一直进行下去。这样，在这里使loop函数不断循环就会比较符合使用的环境。

程序中用到了几个Arduino为我们提供的库函数：

* pinMode(引脚名或引脚编号, 引脚模式)：pinMode函数用于**设置引脚的工作模式**，包括INPUT（输入），INPUT\_PULLUP（输入上拉），OUTPUT（输出）等。我们这里用到了**输出模式**，输出模式的意思是这个引脚用于输出电压（高电平或低电平），这样引脚就可以点亮LED或控制蜂鸣器发声等，其他的模式将在接下来的实验中用到。
* digitalWrite(引脚名或引脚编号, 电平)：digitalWrite函数用于**设置引脚输出的电平**，包括HIGH（高电平，5V），LOW（低电平，0V）。电平是电压的另外一种称呼。自然，只有在给LED通电（即提供电压）时，LED才会亮起，因此我们使用高电平点亮LED，低电平（即不提供电压）熄灭LED。为了使用这个函数，需要预先将引脚设置为**输出模式**。
* delay(毫秒数)：delay函数和引脚或者电平没什么关系，它的功能是让程序暂停指定的时间，这里时间以毫秒为单位（1秒等于1000毫秒）。

在上面你已经看到了几个Arduino提供的库函数，在接下来的实验中，我们将一步步认识和使用更多的库函数对Arduino进行控制。

## §1.3 上传和运行

和一般的软件程序不一样的是，Arduino的程序运行在Arduino上，而非你的电脑上。因此，为了让Arduino能够运行你写的程序，我们需要将程序通过一些方法写入到Arduino内部，这个过程就叫上传或烧录。在上传完成之后，Arduino就可以独立运行你写的程序，而无需电脑。

当然，我们还是需要将Arduino接在电脑上，这是为了给Arduino供电。不过，换成接在充电宝等能够供电的USB接口上也可以支持Arduino的运行。

在进行第一次上传之前，我们需要先认识一下Arduino IDE的界面。

![](.gitbook/assets/chap1\_img4\_idejiemian.png)

* 代码区是你写程序代码的地方。
* 编译信息区会显示你程序的编译和上传情况。
* 上方左侧的五个按钮从左至右依次是编译、上传、新建、打开和保存。
* 上方右侧的按钮是串口监视器，我们暂时还用不到它。

将上一部分提供的代码粘贴至代码区（或者你也可以直接打开提供的代码文件）。

Arduino包括一系列的多种型号，为了让IDE能编译出型号相符的程序，接下来需要进行一些配置。

![](.gitbook/assets/chap1\_img5\_idepeizhi.png)

配置完成后，点击“编译”。如果一切正常，你将看到编译完成的提示，表示你的程序没有语法错误。

![](.gitbook/assets/chap1\_img6\_idebianyi.png)

{% hint style="info" %}
如果出现编译错误，请检查代码是否正确地输入，比如大小写错误、中英文标点错误等都可能导致程序编译失败。如果你不能确定问题，请向我们反馈。
{% endhint %}

编译完成后，点击“上传”，如果之前的配置正确，你将看到上传成功的提示，表示程序已经成功上传到了Arduino中，并且已经开始运行。

![](.gitbook/assets/chap1\_img7\_ideshangchuan.png)

{% hint style="info" %}
如果没有成功，请检查自己在编译配置，部分Arduino Nano在选择处理器时可能需要选择“ATmega328P (Old Bootloader)”。如果你不能确定问题，请向我们反馈。
{% endhint %}

如果一切正常，Arduino板上的用户指示灯应该已经按之前所说开始闪烁，1秒亮，1秒灭。

事实上，在看似复杂的定义里，单片机不过也只是一个按你写好的程序执行的工具。尽管程序的写法上有所区别，输入输出的方式也有所不同，但和你在电脑上编写普通的程序没有本质区别。除了没有鼠标键盘显示器之外，你甚至可以把它看作是一个超小型的电脑。

## §1.4 _更进一步_

从这个实验开始，每个实验的最后一部分都包括了一个更进一步的尝试环节，在这个环节中，我们将在本次实验的基础上，给出一个进阶的目标。通过完成这个目标，你可以更好地掌握本次实验的相关内容。

任务：在上面程序的基础上进行修改，使用户指示灯按SOS摩尔斯电码（三短三长三短）的规律点亮。你可以假定短亮为500ms，长亮为1000ms，两次亮起的间隔为300ms，整段SOS完成之后等待1000ms。

{% tabs %}
{% tab title="防剧透页" %}
【剧透警告！！！】
{% endtab %}

{% tab title="参考代码" %}
参考代码：[【src/chap1\_code2\_sos/chap1\_code2\_sos.ino】](https://www.jianguoyun.com/p/DQpVhxQQmcGwBxjsjpsE)

{% code title="chap1_code2_sos.ino" %}
```arduino
void setup() {
  pinMode(LED_BUILTIN, OUTPUT);
}

void loop() {
  for (int i = 0; i < 3; ++i) {       // 三短亮
    digitalWrite(LED_BUILTIN, HIGH); 
    delay(500); 
    digitalWrite(LED_BUILTIN, LOW);
    delay(500);
  }
  
  for (int i = 0; i < 3; ++i) {       // 三长亮
    digitalWrite(LED_BUILTIN, HIGH); 
    delay(1000); 
    digitalWrite(LED_BUILTIN, LOW);
    delay(300);
  }
  
  for (int i = 0; i < 3; ++i) {       // 三短亮
    digitalWrite(LED_BUILTIN, HIGH); 
    delay(500); 
    digitalWrite(LED_BUILTIN, LOW);
    delay(300);
  }

  delay(1000);
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
