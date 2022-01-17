### <center>Freewaler使用帮助</center>
#### 一、软件使用
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;本软件出厂时均配置完毕，一般情况下不用修改软件的设置。为了防止后续误操作，建议备份config目录下的configsys.ini文件。打开本软件，双击“freewalker.exe “即可。<br>
![size:800,1000](http://39.98.34.145/storage/2021/02-25/KlUIHtLvgj0u99IJIOtDggIAGBoNUNWkdy6u2UmA.png)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;软件主界面如下，点击“开始连接”，便可进入小车控制界面<br>
![size:800,1000](http://39.98.34.145/storage/2021/02-25/3BP5xgPGHyt8el1OSAH3VjxaSc9GZxol76waImfH.png)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在如下界面点击“连接”按钮，使软件与小车连接起来，便可操控小车<br>
![size:800,1000](http://39.98.34.145/storage/2021/02-25/s3XkXa0zKVyxftN6piB2WUbOJZa0VRSCkW9FgQYR.png)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;连接成功后，界面如下，其中4、5、6、7按钮，需要选中某个视频画面后，方可操作，点击某个视频画面，其周边出现红色方框，即代表选中。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1：左摄像头画面<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2：右摄像头画面<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3：前置摄像头画面<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4：视频画面放大，目前只有1、2区域的摄像头支持调焦功能，所以选中1或2后，即可操作视频画面放大<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5：视频画面缩小，同理只能操作1、2区域的摄像头<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6：一键守望，当前设置的两个位置，一个是如下图所示的运动视角，小车行驶时可切换至按视角，还有一个观察视角，用于观察周边的情况，当然，您可以用圆盘任意操作画面的朝向。这里这是提供了运动视角、观察视角的快捷操作方式。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7：该圆盘用于控制摄像头上、下、左、右运动，只能控制1、2区域的摄像头<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8：远程控制<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1）键盘控制：有该段文字“TCP CLIENT CONNECTED SUCCESSFULLY”出现时，表示可以控制小车，这时可用键盘的W、A、S、D键控制，W代码向前，S代表向后，A代表向左，D代表向右。这里操作时注意中文输入法对键盘控制的影响（中文输入法会拦截键盘事件），如果键盘操控无反应，可尝试用鼠标点击8区域中的文字，然后再尝试。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2）手柄控制：有“CONTROLER LINK SUCCESS”时，表示连接成功，手柄左边的摇杆上下推动，可控制小车前后运动；手柄右边的摇杆左右推动，可控制小车左右运动。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3）方向盘控制：有“CONTROLER LINK SUCCESS”时，表示连接成功，转动方向盘可控制小车左右运动；脚刹，从左至右，第二个控制小车前进，第三个控制小车后退。<br>
![size:800,1000](http://39.98.34.145/storage/2021/02-25/BU2EhJ2xYz0ifJGBkPsgfnAP6WAKtu7aBnVuASgq.png)<br>
#### 二、配置介绍
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在主界面中点击“系统设置”按钮，进入该设置界面，如下所示：
![size:800,1000](http://39.98.34.145/storage/2021/02-25/KS85v4bJKSPzN6tmKc4sQwyq0fAKaWaZrim4OJCb.png)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;设置时只需要关注图中红色方框标识出来的即可。首先点击“已锁定”按钮，将配置解锁，然后进行配置。各标注点说明如下：<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1、VIDEO LEVEL 表示视频画面质量级别，HIGH高清画面，LOW画面质量低。在公网传输时，一般配置成LOW，虽然画面质量低一些，但画面会更加流畅。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2、WINDOW_ID 表示在视频显示区域的顺序，按从上到下，从左至右编号，一次为1、2、3、4，调整该参数，可以调整画面区域显示，以适应实际使用场景。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3、ENABLE 视频画面是否默认开启。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4、CONTROLER 设置远程控制方式，有三个选项，分别为：<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1）、Keyboard 键盘控制，W代码向前，S代表向后，A代表向左，D代表向右。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2）、PG 9083S 手柄控制，将手柄通过蓝牙连接到PC或平板后即可。手柄如下图所示：<br>
![size:800,1000](http://39.98.34.145/storage/2021/03-03/G36iQGgkgkyLJnVWRDAsOvGe8P8AAsU8F5UT2svO.jpeg)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;图中箭头表示对车体的控制，前、后、左、右。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;途中按钮标注1（X）：开启喷雾；标注2（Y）：开启旋转；标注3（A）：关闭喷雾和旋转；标注4（B）：关闭旋转。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3）、G29 方向盘控制，将方向盘接入PC即可。方向盘如下图所示：<br>
![size:800,600](http://39.98.34.145/storage/2021/02-25/YRsNOvD7UnPk7g05CiY8olVrJxiNNFFVdgNXbr3f.jpeg)
#### 三、Q&A
##### 1、电量低提示
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当hunter底盘电压低于22V或bunker电压低于47时，为了确保正常使用，请注意及时给小车充电，软件会有弹框提示，同时右下角的实时电压显示会变成红色，如下图所示：<br>
![size:800,1000](http://39.98.34.145/storage/2021/02-25/Jj4rELdAUrMf9fzgszIldlZkvkGFN97zBmLZ91FU.png)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;由于底层硬件数据采集时会产生误差，可能电量充足时，还会有该提示，关掉即可。真正当电压不足时，该提示会一直出现。<br>
##### 2、没有视频画面
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;由于该系统数据依赖公网传输，请检查小车端和freewalker客户端（平板或PC机）是否能正常联网。