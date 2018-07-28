# cherryFly

这是一个基于云端语音识别的智能控制设备，你可以理解为类似于Amazon Echo或者天猫精灵的设备，与之不同的是它是基于单片机实现的。核心芯片为stm32f407vet6，wm8978，esp8266，这三者分别扮演主控，音频DA/ADC以及网络通信的角色。另外还需要SD卡来提供存储功能。

在软件层面本基于FreeRTOS实现。在硬件初始化完毕，FreeRTOS启动完毕后，wm8978开始收集环境中的音频信号。这里采用了一个基于短时过零率和声波能量的简易的VAD算法，用于实现自动录音。录音完毕后，将录制的语音文件发送至百度云的语音识别服务器进行识别，并接收识别的结果，然后根据结果执行相应的操作，比如播放一首音乐等。


## Description

- Audio目录下主要存放音频相关的代码，比如wm8978的驱动，解码，播放以及录制音频的功能。其中包含了一个第三方的mp3解码库“HelixMP3Decoder"。
- Fatfs目录下主要存放Fatfs文件系统的代码，它需要基于SD卡实现。
- File目录下主要存放一些基本的语音反馈文件，这些文件都是wav格式的。以及这个系统的原理图。
- FreeRTOS目录下存放的是FreeRTOS的代码。
- Led目录下存放的是一个根据识别的结果操作Led的实例，在播放音乐的时候，还会对音乐进行频谱分析，从而改变Led的颜色。
- Libraries目录下存放是是stm32f4系列的一些库文件。
- MDK为工程文件的目录。
- Network目录下存放的是与网络操作相关的代码，比如esp8266的串口驱动的封装，编解码，网络通信等等。
- Peripherals目录下存放的是stm32f4相关的外设的驱动代码，其中一些与FreeRTOS结合相当紧密，例如串口的驱动。
- Public目录下存放的是一些基础的功能函数，比如日志功能等。
- Shell目录下存放的是一个简单的人机交互的实现，正因为有了这些代码，我们可以借助Xshell等通过串口登陆stm32，并且可以实现一些简单的命令操作，就像操作Linux系统一样。而且，移植这个Shell非常简单，你只需要底层提供getchar,putchar以及puts这三个功能函数。

## Usage
为了连接WiFi,你需要在"network.c"文件中配置你的WIFI网络。
为了接入百度云的语音识别服务，你需要去注册百度语音的开发者账号，并且得到他的token,你需要在"netvoc.c"中配置这个token。

## More
这个项目是我大学期间课余时间制作的，从最开始的stm32fl03裸机到此版本stm32f407的FreeRTOS。大约跨越了两年的时间，最后我的毕设也是这个。
在此很感谢Li Zhang(此版本硬件的设计者)以及Chenlei Zhang提供的帮助。
一路走来愈发觉得EE是一个深坑，现已转行CS，因此本项目现在只对BUG进行维护，不再增加新功能。
