# 第0节：准备

不打无准备之仗。在开始实验之前，需要整理好手上的物资，并配置好实验环境。

{% hint style="info" %}
为了方便物资的存放和回收，请将实验物资装在防静电自封袋内，并小心保管。
{% endhint %}

## §0.1 整理装备

你的手上应该有这些装备：

* 一个Arduino Nano：这玩意颜值不大行，名字看上去也相当怪异，但它是今天的主角，至少在本实验中，它就是我们的单片机。另外，我们把那一根根针叫做引脚。
  * _如果你感兴趣的话，Arduino是意大利语，来自1000年前意大利国王Arduin的名字；Nano表示小，和它的兄弟【Arduino Uno】相比，确实是个小个子，对不_。

![](.gitbook/assets/chap0\_img1\_arduinonano.jpg)

* 一根USB Mini数据线：用来连接Arduino和你的电脑。
  * _移动互联网时代的一级文物，小心保存这玩意，要是弄丢了就只能到你家的旧手机堆里去另找一根了。_

![](.gitbook/assets/chap0\_img2\_usbxian.jpg)

***

* 一块面包板：用来插接你的Arduino和各类模块，以及后面会提到的杜邦线，小心别把水弄进去了。
  * _这个名字完美地诠释了如何按东西的形状取名并让人完全无法想到它的真实样子。_

![](.gitbook/assets/chap0\_img3\_mianbaoban.jpg)

***

* 一排公对公杜邦线（40根）：两端的针可以插进面包板的孔里，用来连接接在面包板上的模块，公对公的意思是两边都是针脚而非插孔。
  * _所以这玩意也叫面包线。_

![](.gitbook/assets/chap0\_img4\_dubangxian.jpg)

***

* 6个LED（2红2黄2绿）：发光二极管。
  * _知道它为什么叫发光二极管吗，因为它会发光。_

{% hint style="info" %}
提醒：在我告诉你怎么接LED之前，小心对待这玩意。
{% endhint %}

![](.gitbook/assets/chap0\_img5\_led.jpg)

***

* 6个220欧电阻：就是普通的电阻，不过他能救你LED的小命。
  * _在这里，它的学名叫限流电阻，是为了防止太大的电流烧坏你的LED。_

![](.gitbook/assets/chap0\_img6\_dianzu.jpg)

***

* 3个轻触按钮开关：和你在一些电子设备上看到的按钮没什么区别。
  * _明明要用力按下去的按钮，为什么叫轻触开关呢？_

![](.gitbook/assets/chap0\_img7\_qingchukaiguan.jpg)

***

* 一个无源蜂鸣器：能发出滴滴的蜂鸣声。
  * _为了人身安全，别在你的室友睡觉时摆弄这玩意。_

![](.gitbook/assets/chap0\_img8\_fengmingqi.jpeg)

***

* 一个麦克风传感器：可以感受声音的大小，并以电信号的方式输出。
  * _聪明的人已经在考虑怎么把它和蜂鸣器结合在一起用了。_

![](.gitbook/assets/chap0\_img9\_maikefeng.jpeg)

***

* 一个SG90舵机：类似玩具车里的电机，不过这东西可以控制旋转的角度。
  * _加上个指针，或许可以用来做一个声音强度指示器？_

![](.gitbook/assets/chap0\_img10\_duoji.jpg)

###

## §0.2 环境配置

好消息是，在常见的各种单片机中，你需要为Arduino安装的东西是最少的；

坏消息是，仍然要装一个驱动和一个软件，而这个驱动有一定几率安装失败。

[【CH341SER.exe】](https://www.jianguoyun.com/p/DQpVhxQQmcGwBxjsjpsE)

* 下载上面的程序，双击运行，如果弹出“你要允许此应用对你的设备进行更改吗？”的窗口，选择“是”。

![](.gitbook/assets/chap0\_img11\_qudonganzhuang1.png)

* 在出现的界面中，点击“安装”，如果提示“驱动预安装成功”，恭喜，你的驱动已经装好了。

![](.gitbook/assets/chap0\_img12\_qudonganzhuang2.png)

![](.gitbook/assets/chap0\_img13\_qudonganzhuang3.png)

{% hint style="info" %}
如果出现失败的情况，请向我们反馈，我们会给出相应的解决方案。
{% endhint %}



接下来安装Arduino IDE。

[【arduino-1.8.16-windows.exe】](https://www.jianguoyun.com/p/DQpVhxQQmcGwBxjsjpsE)

* 下载上面的程序，双击运行，如果弹出“你要允许此应用对你的设备进行更改吗？”的窗口，选择“是”。

![](.gitbook/assets/chap0\_img14\_ideanzhuang1.png)

* 在出现的界面中，依次点击“I Agree”，“Next”和“Install”。

![](.gitbook/assets/chap0\_img15\_ideanzhuang2.png)

![](.gitbook/assets/chap0\_img16\_ideanzhuang3.png)

![](.gitbook/assets/chap0\_img17\_ideanzhuang4.png)

* 如显示“Completed”，说明安装已经完成，点击“Close”结束安装程序。

![](.gitbook/assets/chap0\_img18\_ideanzhuang5.png)

至此，环境配置完成，接下来，我们将进入真正的实验流程。
