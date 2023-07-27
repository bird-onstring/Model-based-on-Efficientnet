基于EfficientNet的云服务垃圾分类信息系统项目方案

上海工程技术大学
参赛队员：袁文鹏 林创栩


目  录

摘   要

当前，随着人民日渐提升的环保意识，垃圾分类已经成为了日常生活里一项重要事务。由于起步较晚，垃圾分类的知识并没有在人民中广泛普及，这导致了民众虽然响应国家号召进行垃圾分类，但是由于上述知识的匮乏，许多垃圾的分类仍存在大量的错误，以至于垃圾被运输到处理终端时，还需要二次处理。

神经网络的出现使得上述问题的解决成为可能，基于人工标注的数据集，在此基础上通过在GPU上的训练,使得训练网络可以在无人干预下识别实际场景中的物体，配合机械装置，让人可以在缺少相关知识的情况下，实现对于垃圾的正确分类。

本作品的设计思路是以树莓派为主体平台，依托云服务接口，最终在树莓派上实现完整的系统。该系统由现场图片采集端和云服务提供端，以及微机驱动单元组成。本地端以树莓派作为其主控核心通过HDMI显示器进行显示，使用PWM信号发生器PCA9685作为舵机控制器，控制软件采用Python语言设计，图片采集器使用E12摄像头。远程服务端使用EfficientNet模型为本地端提供服务，可根据本地端设备的数量和需求，动态规划云服务算力。远程服务端会对树莓派发送的图片进行判断，并将对应的垃圾分类结果返回给树莓派，此后树莓派会通过PCA9685控制舵机打开对应的垃圾桶。经测试验证，该系统实现了本地端与远程端的交互、图像识别、图片获取、垃圾桶自动开合等相关操作，满足垃圾分类相关需求。因此，该作品可作为产品原型，具有良好的市场应用前景。

关键词：垃圾分类，树莓派，PWM，EfficientNet，云服务


1.作品难点与创新点
1.1作品难点
在老旧城区，巷道，此类地区由于历史和地理原因，人口众多，同时也是生活垃圾的主要产生地，垃圾种类繁多，数量大，迫切需要一套行之有效的垃圾分类系统。但在传统的垃圾分类中，还是依靠人使用传统先验知识来分类，此类方法十分依靠人力，在人力资本高昂的现代社会，效率十分低下，已经在被逐步淘汰。后续发展的依据传统图像算法的机械垃圾桶，需要针对每种特定的垃圾来设定独特的算子，针对现代社会垃圾种类日益丰富的现状，在可预计的将来是无法满足需求的。本系统难点旨在将垃圾分类集成化、智能化的检测，并配以适当的传动装置完成整套垃圾分类流程。

可持续发展作为我国科学发展的一种经济促进形式，一直是我国现代社会发展最重要的关键。在现代科学技术背景下，始终是一个严肃的问题，而不仅仅是满足现代人对科学技术的强烈需求，不能因为一时之快，不注重资源保护而牺牲子孙后代的利益的问题。随着人工智能时代的来领，如何将人工智能投入于提升人民的生活质量提高是一个迫切需要解决的问题。为了智慧生活改造的需求，各种智能技术不断涌现。但是产品的使用对象需要的操作依旧较多。如何采取低人力资本消耗和快速迭代新垃圾种类的是本系统需要解决的难题。

1.2作品创新点
一．使用树莓派作为信息交互的主体，依托GPIO拓展板实现快速硬件部署、迭代。同时实现简洁直观的前端界面，完成一键识别物体、开关对应分类垃圾桶等操作，实现垃圾分类集成化、智能化检测，以传动装置完成整套垃圾分类流程。

二．使用EfficientNet模型作为分类动作的backbone，进行较高精度下更快速的目标识别。

三．利用云服务，减少本地设备的复杂程度，降低维护成本，同时可以享受到云服务商的高可用硬件，在产品试运行阶段，可快速扩容落地，实现一对多的服务。


2.方案论证与设计
2.1EfficientNet
122.12.1.1EfficientNet的诞生
EfficientNets是谷歌大脑的工程师谭明星和首席科学家QuocV.Le在论文
《EfficientNet:RethinkingModelScalingforConvolutionalNeuralNetworks》中提出。

该模型的基础网络架构是通过使用神经网络架构搜索（Neural Architecture Search）设计得到。卷积神经网络模型通常是在已知硬件资源的条件下，进行训练的。当你拥有更好的硬件资源时，可以通过放大网络模型以获得更好的训练结果。为系统的研究模型缩放，谷歌大脑的研究人员针对EfficientNets的基础网络模型提出了一种全新的模型缩放方法，该方法使用简单而高效的复合系数来权衡网络深度、宽度和输入图片分辨率。尺度模型如下图2-1所示。


图2-1EfficientNets的模型尺度

2.1.2EfficientNet的进步
EfficientNet的主要创新点并不是结构，不像ResNet、SENet发明了shortcut或attention机制，EfficientNet的base结构是利用结构搜索出来的，然后使用compoundscaling规则放缩，得到一系列表现优异的网络：B0~B7.下面图2-2分别是ImageNet的Top-1Accuracy随参数量和flops变化关系图，可以看到EfficientNet饱和值高，并且到达速度快。

ImageNet的Top-1Accuracy随参数量和flops变化关系图如图2-2所示。


图2-2ImageNet的Top-1Accuracy随参数量和flops变化关系图

增加网络参数可以获得更好的精度（有足够的数据，不过拟合的条件下），例如ResNet可以加深从ResNet-18到ResNet-200，GPipe将baseline模型放大四倍在ImageNet数据集上获得了84.3%的top-1精度。增加网络参数的方式有三种：深度、宽度和分辨率。

深度是指网络的层数，宽度指网络中卷积的channel数（例如wideresnet中通过增加channel数获得精度收益），分辨率是指通过网络输入大小（例如从112x112到224x224）

直观上来讲，这三种缩放方式并不独立。对于分辨率高的图像，应该用更深的网络，因为需要更大的感受野，同时也应该增加网络宽度来获得更细粒度的特征。
之前增加网络参数都是单独放大这三种方式中的一种，并没有同时调整，也没有调整方式的研究。EfficientNet使用了compoundscaling方法，统一缩放网络深度、宽度和分辨率。


2.2树莓派技术
2.22.2.1树莓派起源
树莓派由注册于英国的慈善组织“RaspberryPi基金会”开发，埃·厄普顿为项目带头人。2012年3月，英国剑桥大学埃本·阿普顿（EbenEpton）正式发售世界上最小的台式机，又称卡片式电脑，外形只有信用卡大小，却具有电脑的所有基本功能，这就是RaspberryPi电脑板，中文译名"树莓派"。这一基金会以提升学校计算机科学及相关学科的教育，让计算机变得有趣为宗旨。基金会期望这一款电脑无论是在发展中国家还是在发达国家，会有更多的其它应用不断被开发出来，并应用到更多领域。在2006年树莓派早期概念是基于Atmel的ATmega644单片机，树莓派样板如下图2-3


图2-3树莓派4B

它是一款基于ARM的微型电脑主板，以SD/MicroSD卡为内存硬盘，卡片主板周围有1/24个USB接口和一个10/100以太网接口（A型没有网口），可连接键盘、鼠标和网线，同时拥有视频模拟信号的电视输出接口和HDMI高清视频输出接口，以上部件全部整合在一张仅比信用卡稍大的主板上，具备所有PC的基本功能只需接通电视机和键盘，就能执行如电子表格、文字处理、玩游戏、播放高清视频等诸多功能。RaspberryPiB款只提供电脑板，无内存、电源、键盘、机箱或连线。

2.2.2树莓派引脚
树莓派的可扩展性很大一部分依赖于树莓派的一组GPIO接口来实现，但是树莓派4b的GPIO接口较多，接线的时候如果没有参照很难完成。

下图2-4为是树莓派4b的树莓派接口对照表。接线的时候可以参照该GPIO接口对照表来确定接线的位置。

图2-4树莓派4B树莓派接口对照表

2.3Flask技术
22.22.32.3.1Flask起源
Flask是一个用Python编写的Web应用程序框架。ArminRonacher带领一个名为Pocco的国际Python爱好者团队开发了Flask。Flask基于WerkzeugWSGI工具包和Jinja2模板引擎。两者都是Pocco项目。

2.3.2Flask的便捷性
Flask是一个轻量级的可定制框架，使用Python语言编写，较其他同类型框架更为灵活、轻便、安全且容易上手。它可以很好地结合MVC模式进行开发，开发人员分工合作，小型团队在短时间内就可以完成功能丰富的中小型网站或Web服务的实现。另外，Flask还有很强的定制性，用户可以根据自己的需求来添加相应的功能，在保持核心功能简单的同时实现功能的丰富与扩展，其强大的插件库可以让用户实现个性化的网站定制，开发出功能强大的网站。

Flask也被称为“microframework”，因为它使用简单的核心，用extension增加其他功能。Flask没有默认使用的数据库、窗体验证工具。

2.4VUE技术
22.12.22.32.42.4.1VUE的介绍
VUE是一套用于构建用户界面的渐进式框架。与其它大型框架不同的是，VUE被设计为可以自底向上逐层应用。VUE的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库结合使用时，VUE也完全能够为复杂的单页应用提供驱动。响应式系统绑定结构如下图所示。


图2-5响应式系统绑定结构

2.4.2VUE的特点
完整的网页是由DOM组合与嵌套形成最基本的视图结构，再加上CSS样式的修饰，使用JavaScript接受用户的交互请求，并通过事件机制来响应用户交互操作而形成的。把最基本的视图结构（也就是HTMLDOM）拿出来，称为视图层。这个被称为视图层的部分就是Vue核心库关注的部分。

Vue.js关注视图层。因为一些页面元素非常多。结构庞大的网页如果使用传统开发方式，数据和视图会全部混合在HTML中，处理起来十分不易，并且结构之间还存在依赖或依存关系，代码上就会出现更多问题。有过前端开发基础的使用者都应当了解过jQuery，jQuery给予简洁的语法和跨平台的兼容性，极大地简化了JavaScript开发人员遍历HTML文档、操作DOM、事件处理等操作。

jQuery的使用经验，可知，开始页面元素不多，有时会需要一层层地不断向上寻找父辈元素，如$('#xxx').parent().parent()，但后期修改页面结构，代码可能就会变得臃肿，如$('#xxx').parent().parent().parent()，随着产品升级的速度越来越快，修改变得越来越多，页面中相似的关联和嵌套DOM元素多得数不清，而jQuery选择器及DOM操作本身也存在性能缺失问题，想要修改无从下手。总之，原本轻巧简洁的jQuery代码，在产品需求面前变得啰嗦冗长。但是Vue.js解决了这些问题，这些问题将在VUE中消失。

2.5云服务技术
2.5.1云服务的介绍
云中的资源在使用者看来是可以无限扩展的，并且可以随时获取，按需使用，按使用付费。云是一个计算资源池，包括计算服务器、存储服务器、带宽资源等等。

云服务商将所有的计算资源集中起来，通过网络提供给用户。这使得应用提供者无需为繁琐的细节而烦恼，能够更加专注于自己的业务，有利于创新和降低成本。

云服务器通常运用到的技术有分布式存储、资源调度和虚拟化。其中虚拟化技术又包括服务器虚拟化、存储虚拟化、内存虚拟化和网络虚拟化。

虚拟化技术：
1.利用服务器虚拟化，将服务器的CPU、内存、磁盘等硬件集中管理，通过集中式动态按需分配，提升资源利用效率。

2.存储虚拟化是将存储资源的逻辑视图和物理存储分离，为系统提供无缝的资源管理。但存储标准化程度低，不同云服务商的技术要考虑兼容性。

3.内存虚拟化是计算机内存系统对内存的管理，系统使上层应用具有连续可用的内存，并在物理层上分割多个碎片，以满足内存的分配及必要的数据交换。

4.网络虚拟化利用软件从物理网络元素中分离网络力量，与其他形式的虚
拟化有共同之处。


3原理分析与硬件电路图
3.1功能设计
本文旨在设计一个基于EfficientNet的智能垃圾分类系统，以树莓派为核心，辅以PWM信号发生器、HDMI显示器、摄像头、远程计单元构建等硬件平台，设备详情为USB鼠标、E12免驱摄像头、GPIO拓展板、跳线、PCA9685、DELL显示器、树莓派。通过图像采集端采集图片，经由前端中介传输到远程计算单元，从而控制舵机，打开对应的垃圾桶。

主要功能如下：
1.在现场打开程序，USB蓝灯亮起，放入待拍摄物体到载物台，按下前台拍摄按钮。并通过树莓派，将图片上传至云服务端。

2.图片经云服务端判断完成后，传回必要的参数，树莓派接受参数。

3.树莓派接受完参数后打开对应的分类舵机。

4.在舵机在打开后，睡眠8秒后，再次驱动舵机，关闭垃圾箱。

3.2架构设计
本系统的架构设计如图3-1所示。具体设备以树莓派4B、USB鼠标、E12免驱摄像头、GPIO拓展板、跳线、PCA9685、SG90舵机、DELL显示器、树莓派等组成构建系统，设计和实现基于Efficientnet的智能垃圾分类系统。


图3-1赋能垃圾分类真正落地见实效的信息系统的架构图

系统由以下组成：
1.现场图片采集端：现场图片采集终端以E12摄像头为核心。

2.树莓派中继传输端：树莓派收到E12的图片数据后经由http协议传输至远程算力单元。

3.远程图片处理端：远程算力单元收到图片后，传输至云服务端等待判断网络输出结果后送回树莓派。

4.舵机驱动端：树莓派收到参数后经由驱动程序判断之后，打开对应的舵机。

3.3现场图片采集端
3.3.1硬件选型
由于树莓派对摄像头有原生的支持，但是仅可使用原生排线摄像头。并且如果需要使用USB摄像头需要挑选过，本系统挑选了E12摄像头，免驱，需要配合fswebcam。

输入命令：apt-getinstallfswebcam，即可一键安装，图3-2为E12摄像头。


图3-2E12摄像头

3.4树莓派中继传输端
3.4.1硬件选型
核心控制板选用树莓派，树莓派4B支持很多种操作系统，原厂系统与linux类似，可以快速上手，且对硬件的支持十分完善。

表3-3为树莓派4B具体参数。

表3-3树莓派4B具体参数
核心：
CPU：BroadcomBCM2711，1.5GHz，64-bit，4核心，ARMCortex-A72架构，1MBshared
L2cacheRAM：1、2、4GBLPDDR4-3200RAM(sharedwithGPU)
网络：
以太网：10/100/1000Mbit/s无线网：b/g/n/ac双频2.4/5GHz蓝牙：5.0
多媒体：
GPU：BroadcomVideoCoreVI@500MHzHDMI：micro-HDMIDSI：板载排线
外围设备：
17×GPIOplusthesamespecificfunctions,HAT,andanadditional4×UART,4×SPI,and4×I2Cconnectors

下图3-3为树莓派4B各部分详情。


图3-3为树莓派4B各部分详情

3.5远程图片处理端
3.5.1硬件选型
本系统的算力由远程算力单元提供型号为GTX1070，由于EfficientNet在训练时对于计算资源有一定要求，所以需要选择一张合适的GPU。


图3-41070核心

3.6舵机驱动端
1.2.3.3.1.3.2.3.3.3.4.3.5.3.6.3.6.1.硬件选型
一、重要器件、模块和外设的选型
本次设计选用的舵机为SG90，树莓派需要与GPIO拓展板连接，舵机控制板选用PCA9685（PWM信号发生器）。

二、PCA9685控制原理
通过之前对舵机的观察会发现，控制一个舵机需要占用三个引脚，其中一个为单片机的I/O脚。当所需使用的舵机数量较多时，十分占用引脚资源。此时PCA9685模块便可以发挥作用。

PCA9685芯片设计之初是用来控制多路LED灯，后来发现在控制多路舵机上也可发挥很大作用。

1、PCA9685设备地址
地址分配是通过模块右上方的短接焊盘来确定的，从A0-A5表示地址的最低位到最高位，也就是最多可级联2^5=32个模块，地址为：1+A5+A4+A3+A2+A1+A0+rw。如果不用短接的话Ax=0；短接的话Ax=1；rw为写的话rw=0；rw为读的话rw=1；所以写入数据不做短接则地址应该为10000000=0x80。

2、IIC通信
SCL接树莓派SCL0，SDA接树莓派SDA0，VCC接3.3V，GND接树莓派
GND，V+单纯只是供电。


图3-5树莓派引脚连接图
重启树莓派之后进行测试。
执行命令安装i2c-tools：sudoapt-getinstalli2c-tools
再运行：sudoi2cdetect-y


图3-6引脚显示图

如下图3-7所示：一个脉冲周期为20ms，高电平为脉冲宽度，这个脉冲宽度决定舵机旋转角度，假设180°舵机旋转位于中间位置（90°）的脉冲宽度为1.5ms，则0°位置为1ms，180°位置为2ms，以此类推，得出一下公式：
舵机旋转1°=（最大脉冲宽度-最小脉冲宽度）/最大角度（最大位置）


图3-7控制电平示意图

SG90舵机一共三根线，VCC接5Ｖ电源，GND接地，还有一根数据控制线，该线接在GPIO上。


图3-8SG90连接树莓派示意图

由于连接了PCA9685所以此时的接线需要改变，需要依照把舵机对应的VCC、GND数据传输线接到对应的接到PCA9685接口上。


图3-10pca9685整体


4软件设计与流程
4.1树莓派端传输设计及实现
4.1.1设计思路
树莓派端上需要分写三个程序，app_server.py、motor.py、all.sh。


图4-1文件层级结构

程序一需要实现通过opencv获取实时视频流，从而实现对摄像有的调用，同时使用，flask的路由传播技术实现对特定信息，传到到对应的路由页面。

主要使用了flask-router技术，现代Web框架使用路由技术来帮助用户记住
应用程序URL。可以直接访问所需的页面，而无需从主页导航。

Flask中的route()装饰器用于将URL绑定到函数。例如：表4-1

表4-1flask-router的简单实现


在这里，URL'/hello'规则绑定到hello_world()函数。因此，如果用户访问http：//localhost：5000/helloURL，hello_world()函数的输出将在浏览器中呈现。

application对象的add_url_rule()函数也可用于将URL与函数绑定，如上例所示，使用route()。

装饰器的目的也由以下表示：

表4-2flask-router的绑定URL实现


程序二需要可以收到信号后打开对应的舵机，实现时直接调用Adafruit_PCA9685。

程序三需要实现linux后台启动功能，否则每运行一次，需要开启一个新终端。

4.1.2实现
motor.py程序会响应前端程序的命令。当发出识别指令后，后台程序会通过opencv获取实时视频流，并转化为jpg格式，然后上传至后端服务接口进行判断。motor.py程序会控制四个舵机的运转，其会调用PCA9685的驱动，对控制板发出舵机旋转命令已达到垃圾桶开关的效果。其主要代码及参数配置如下图所示：


图4-2舵机控制主要代码及参数

舵机旋转到servo_max时及垃圾桶打开，当旋转到servo_min时垃圾桶关闭。垃圾桶一次开关会停顿8秒。以下展示其余关键代码：

表4-3树莓派端传输关键代码


4.2VUE的使用
4.24.2.1设计思路
为了要避免使用VUE手动建立起electron应用程序。electron-vue在本项目中被充分利用。vue-cli作为脚手架工具，加上拥有vue-loader的webpackelectron-packager或是electron-builder，以及一些最常用的插件如vue-router、vuex等。

electron-vue官方推荐yarn作为软件包管理器，因为它可以更好地处理依赖关系，并可以使用yarnclean帮助减少最后构建文件的大小。

表4-4electron-vue起步


为了帮助消除开发过程中的冗余任务，请注意一些可用的NPM脚本。以下命令应该运行在项目的根目录下。当然，你可以使用yarnrun<command>的方式运行下列任何命令。

表4-5NPM脚本


4.3远程算力提供端设计及实现
4.3.1设计思路
远程算力提供端口需要完成图片接收，图片处理，图像分类，返回分类结果等四个步骤，具体操作流程如下图4-3所示。


图4-3后端服务流程图

程序使用get、post方法实现图片的上传和分类结果的返回。在各个流程中，程序都设置有异常处理，包括图片类型错误、识别错误、分类结果无法输出等情况。在遇到异常时，服务器的控制台可看到异常的结果，便于管理员维护。

服务器接口采用http协议中较为常用的get，post方法，可扩展性强，其他客户端可以轻松地调用该接口并实现各种功能。

4.34.3.14.3.2实现
主要接口代码如图4-4所示。


图4-4接口代码


5系统测试与误差分析
5.1测试环境的搭建
5.1.1硬件连接
一个赋能垃圾分类真正落地见实效的信息系统，以树莓派为核心，辅以PWM信号发生器、HDMI显示器、摄像头、远程计单元构建等硬件平台，设备详情为USB鼠标、E12免驱摄像头、GPIO拓展板、跳线、PCA9685、SG90舵机、DELL显示器、树莓派。具体接线，如图5-1所示

图5-1现场硬件连接

GPIO拓展板与树莓派通过直线连接，实现直观的IO口观察，如图5-2


图5-2	GPIO拓展板与树莓派连接

GPIO拓展板与PCA968实现进行硬件连接，如图5-3所示


图5-3GPIO拓展板与PCA9685进行连接

5.1.2控制树莓派配置
使用VNCViewer软件，远程控制树莓派。


图5-4VNCViewer图标
然后再此界面填入正确的user以及pwd，即可连接进入PI。

图5-5VNCViewer连接

之后就可登录PI的GUI界面


图5-6VNCViewer连接成功显示页面


5.2EfficientNet训练
5.2.1EfficientNet原理
EfficientNets是谷歌大脑的工程师谭明星和首席科学家QuocV.Le提出。该模型的基础网络架构是通过使用神经网络架构搜索（neuralarchitecturesearch）设计得到。卷积神经网络模型通常是在已知硬件资源的条件下，进行训练的。当你拥有更好的硬件资源时，可以通过放大网络模型以获得更好的训练结果。为系统的研究模型缩放，谷歌大脑的研究人员针对EfficientNets的基础网络模型提出了一种全新的模型缩放方法，该方法使用简单而高效的复合系数来权衡网络深度、宽度和输入图片分辨率。

通过放大EfficientNets基础模型，获得了一系列EfficientNets模型。该系列模型在效率和准确性上战胜了之前所有的卷积神经网络模型。尤其是EfficientNet-B7在ImageNet数据集上得到了top-1准确率84.4%和top-5准确率97.1%的结果。且它和当时准确率最高的其它模型对比，大小缩小了8.4倍，效率提高了6.1倍。且通过迁移学习，EfficientNets在多个知名数据集上均达到了当时最先进的水平。



图5-7EfficientNets与较优网络在ImageNet上的比较

5.2.2训练数据集
本次训练的数据集来自于华为云人工智能大赛，垃圾分类的相关数据集。数据集中包括4个大类，40个小类。大类包括其他垃圾、厨余垃圾、可回收物、有害垃圾，小类包括一次性快餐盒、牙签、大骨头等。其中每一个小类都会对应一个大类。具体的类别及标签如下表所示。

表5-1训练类别

标签	对应种类
0
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16	其他垃圾/一次性快餐盒其他垃圾/污损塑料
其他垃圾/烟蒂其他垃圾/牙签
其他垃圾/破碎花盆及碟碗其他垃圾/竹筷
厨余垃圾/剩饭剩菜厨余垃圾/大骨头厨余垃圾/水果果皮厨余垃圾/水果果肉厨余垃圾/茶叶渣厨余垃圾/菜叶菜根厨余垃圾/蛋壳
厨余垃圾/鱼骨
可回收物/充电宝


17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
34
36
37
38
39
40
41
42
43
44	可回收物/包
可回收物/化妆品瓶可回收物/塑料玩具可回收物/塑料碗盆可回收物/塑料衣架可回收物/快递纸袋可回收物/插头电线可回收物/旧衣服可回收物/易拉罐可回收物/枕头
可回收物/毛绒玩具可回收物/洗发水瓶可回收物/玻璃杯可回收物/皮鞋
可回收物/砧板可回收物/纸板箱可回收物/调料瓶可回收物/酒瓶
可回收物/金属食品罐可回收物/锅
可回收物/食用油桶可回收物/饮料瓶有害垃圾/干电池有害垃圾/软膏
有害垃圾/过期药物
数据集的图片格式为jpg，一共有19735张，每一张图片对应一个txt文件，用以记录图片的类别。

本次训练我们将测试集和训练集分为7:3，即4405张测试集和10278张训练集，并重新记录相关的路径及标签用以制作自己的dataset。
4.5.6.7.8.8.1.8.2.8.2.1.8.2.2.8.2.3.模型训练硬件支持
CPU：i7-8750HGPU：Gtx1070
内存：16GB
训练环境：python3.7+pytorch1.7.1
集成开发工具：pycharm
8.2.4.模型训练技巧
为了满足速度快，准确率高的需求，本文采用efficientnet-b0做为分类模型。如果我们从零开始训练一个EfficientNets网络，这个代价是及其昂贵的。在ImageNet上预训练模型参数，让网络适应图像信息普适特征，然后针对扩展数据集进行第二次训练，这里可以让垃圾分类数据集已经有不错的特征提取能力，最后在训练集上进行微调训练。

本方案将对imagenet上的预训练efficientnet-b0进行训练。替换掉该模型的最后一个全连接层，训练20个epoch。由于设备显示，本次训练设置的batchsize为60。具体的超参数设置如下表所示。

表5-2训练超参数表

超参数	变量
Img	224*224
Batchsize	60
Epoch	20
Optimizer	Adam
Lossfunction	Crossentropy
Learningrate	0.0001

本次模型采用pytorch并使用GPU进行训练，使用tensorboardx记录相关测试数据。在训练前会对图片进行处理，剪裁至模型规定的224*224像素。在训练过程中，程序会在每一个step下记录一次loss，每一个epoch记录一次训练集准确率和测试集准确率，具体的结果如图5-8、图5-9、图5-10所示

图5-8训练loss



图5-9训练集准确率

图5-10测试集准确率

由上述三图可以看出在20个epoch之后，模型已经完全收敛，训练结束。模型最后的测试集准确率为87.26%，性能基本符合方案总体需求。

5.3实际分类功能测试
5.3.1测试前端
测试树莓派上的交互页面能否正常打开，如下图显示

图5-11树莓派交互页面


5.3.2舵机功能测试
测试舵机能否被树莓派控制

图5-12测试舵机

5.3.3整体联动测试
完成一整套控制链，观察整体控制流程是否完成，效果下图所示

图5-13进入前端界面

点击基于EfficientNet的云服务垃圾分类系统，进入识别界面，如下图所示。



图5-14进入识别界面并点击拍照识别

放入待拍摄物体到载物台，如放入苹果，点击拍照识别，进入结果界面，如
下图所示。

图5-15进入结果界面

识别出苹果为厨余垃圾，此时厨余垃圾桶自动打开，8秒后自动关闭。具体实现流程见附件演示视频。


6.总结
本作品的设计思路是以树莓派为主体平台，依靠网络传输技术，实现远程算力调度，最终在树莓派上实现完整的系统。该系统由现场图片采集端和远程算力提供单元，微机驱动单元组成，以树莓派作为其主控核心，远程算力单元为GTX1070使用HDMI显示器作为现场端显示设备，使用PWM信号发生器PCA9685作为舵机控制器，控制软件采用Python语言设计，图片采集器使用E12摄像头。实现了由E12摄像头完成采集垃圾图片后，经由树莓派发送图片至远程算力单元，使用训练完成的神经网络EfficientNet，经过判断输出对应的垃圾类别，此后树莓派发送信号到PCA9685驱动舵机打开对应的垃圾桶。经测试验证，该系统实现了现场端与远程端的交互，以通过E12摄像头拍摄垃圾图片上传至远程算力单元完成垃圾分类的识别并打开对应的舵机。

本设计最终实现了上述所有功能，通过http通信协议实现模块间的数据传输，分别使用VUE和python。经测试验证，基赋能垃圾分类真正落地见实效的信息系统可以正常识别分类垃圾，并且打开对应的舵机。

本方案采用了人机交互前端与后台分离的方式进行部署，在遇到需要舵机控制和交互界面分别在两台计算机上运行的情况时，本方案依然可以灵活适用。

本设计使用前后端分离的方案。后端服务器进行图片的识别分类并提供接口，前端的树莓派，展示交互界面，拍摄图片并调用后端服务器接口，最后根据返回的结果控制垃圾桶的开合。因此，一个服务器可以为多个前端提供服务。为了保证服务接口能够满足并发任务，保证接口的快速有序。

该课题可作为符合企业需求的产品原型，拥有十分出色的市场应用前景。

最后，由于本次设计的调研时间问题和成本问题，并未完全在硬件层面实现
故障闭环控制，只在后台实现了故障报错。考虑到硬件的可靠性已经基本实现需求，会在日后的调研学习中不断优化与改进。


7.参考文献
[1]孙卓廷.科技发展与环境保护[J].ft东农业工程学院学报,2002,(05).

[2]李寿.绿色科技与环境保护[J].福建环境,1996,(14).

[3]TanM,LeQV.EfficientNet:RethinkingModelScalingforConvolutionalNeuralNetworks[J].2019.

[4]赵凯旋,刘晓航,姬江涛.基于EfficientNet与点云凸包特征的奶牛体况自动评分[J].农业机械学报,2021,52(05):192-201+73.

[5]苏祥林,陈文艺,闫洒洒.基于树莓派的物联网开放平台[J].电子科技,2015,28(9):35.

[6]吴波涛,徐正锋,孙金卫,等.基于树莓派的智能温湿度监控终端:,CN207688931U[P].2018.

[7]邢永恒、杨振南、熊定胜.基于VUE的学生管理系统设计[J].信息与电脑(理论版),2020,v.32;No.459(17):104-106.

[8]陶飞,张霖,郭华,等.云制造特征及云服务组合关键问题研究[J].计算机集成制造系统,2011,17(03):477-486.

[9]张亮,赵飞跃.基于STM32-PCA9685的四足机器人控制系统设计[J].南方农机,2020,51(14):117-119.