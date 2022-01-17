- 获取接入代码和接入密码信息，请联系厂家。

- 下载[python版sdk](http://39.98.34.145/storage/2021/01-27/RYBhogXg5Lm2Crr2JPs9blCogARmUAhZ9dUCRoiR.zip)，该版本sdk基于64位python3.6.5，在window10平台编译。

- 安装python版sdk

  双击agx_freewalker_ctrl-1.0.0.win-amd64.exe安装包，出现如下界面：

  ![size:800,1000](http://39.98.34.145/storage/2021/01-27/Z03xmZStqbcA9Ktq35c9J7XQ1lkLiMr4BvEWdqT5.jpeg "python sdk安装界面1")

  点击“下一步”按钮。

  ![size:800,1000](http://39.98.34.145/storage/2021/01-27/pMd645gQqgjsYXO3DFMKehJQYUixENTGrYa6uIY7.jpeg "python sdk安装界面2")

  电脑可能安装了多个版本的python，这里选择接下来将要使用的版本，笔者使用的是3.6.5，点击“下一步”按钮。

  ![size:800,1000](http://39.98.34.145/storage/2021/01-27/JpKYmsZVtK1EDK0kDDNwUmqBVRjtVG9kINi02XgS.jpeg "python sdk安装界面3")

  点击“下一步”按钮，开始安装。

  ![size:800,1000](http://39.98.34.145/storage/2021/01-27/GEFqhjMzbKfBo5lIBMI6BcsUbR1XAI9UXwtYcJAJ.jpeg "python sdk安装界面4")

  点击“完成”，sdk安装完成。

- 重要的类和函数说明

  抽象类ClientListener，该类由使用者继承实现，用于获取车载端实时采集的数据，比如运行速度、电压、故障等信息。如下所示：

  ```python
  class ClientListener(metaclass=abc.ABCMeta):
    @abc.abstractmethod
    def connect_status(self, connected, err_message):
      """
      连接状态监听
      :param connected: 布尔型，true表示已连接，false未连接
      :param err_message: 字符串类型，连接失败后的错误提示信息
      """
      pass
  
    @abc.abstractmethod
    def system_state(self, state):
      """
      系统状态回馈
      :param state: 字典型，系统状态，格式为{
        'carState': '',   // 车体状态，字符串类型
        'mode': '',       // 控制模式，字符串类型
        'voltage': ,      // 电压，浮点型
        'fault_01': '',   // 错误1，字符串类型
        'fault_02': ''    // 错误2，字符串类型
      }
      """
      pass
  
    @abc.abstractmethod
    def motion_state(self, state):
      """
      运动状态回馈
      :param state: 字典型，运动状态，格式为 {
        'speed': ,       // 运动速度，单位m/s，浮点型
        'angle':         // 转向角度，单位rad，浮点型
      }
      """
      pass
  ```

  FreeWalkerCtrl类：该类用于连接远程车载端，注册回调等功能，主要函数如下：

  ```python
    def set_client_listener(self, client_listener):
      """
      设置监听
      :param client_listener: 监听信息，继承自ClientListener类
      """
  
    def connect_to_host(self, access_code, access_pwd):
      """
      连接主机
      :param access_code: 接入代码
      :param access_pwd: 接入密码
      """
      
    def send_ctrl_ins(self, montion, state, value):
      """
      发送控制命令
      :param montion: 枚举类型，设置运动类型，前后还是左右，参见CtrlState定义
      :param state: 枚举类型，设置运动状态，运动还是停止，参见CtrlState定义
      :param value: 数值型，速度或角度值，取值范围均为[-100, 100]
      """
      
  def disconnect(self):
      """
      关闭通信
      """
  ```

  控制状态CtrlState：

  ```python
  # 控制状态
  class CtrlState(Enum):
    FRONT_AND_BACK = 1
    LEFT_AND_RIGHT = 2
    MOTION = 3
    STOP = 4
  ```

- 使用

  [示例下载](http://39.98.34.145/storage/2021/01-27/n7CpqyKkCnoX5f5nMpHcg9lZhCZ2DQTEtAfiWa5l.zip)，本示例采用pyqt5编写，界面如下：

  ![size:800,1000](http://39.98.34.145/storage/2021/01-27/2soXDfOb0Ews4vMKZjcBYDF8o3bWw5EQLKPHCmgA.jpeg "python sdk示例")

  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;输入接入代码和接入密码后，点击“连接”按钮，弹出“连接成功”后，在“系统状态”和“运动反馈”可以观察到小车的实时运动信息。

  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;点击“保持前进”按钮，该功能点会保持小车一直处于向前行驶状态，点击“停止”按钮可关闭。

  &nbsp;&nbsp;&nbsp;&nbsp;“前进”、“后退”、“向左”、“向右”可观察小车前后左右运动控制，示例中仅仅是演示，每点击一次，小车运动一下之后立刻停止。所以需要小车持续运动，需要持续不停发送运动指令。

  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;核心代码如下：

  demo_listener.py - DemoClientListener：实现实时数据回调监听

  ```python
  from agx_freewalker.client_listener import ClientListener
  
  class DemoClientListener(ClientListener):
    __system_cnt = 0
    __motion_cnt = 0
    __ui = object()
    connected = False
    def connect_status(self, connected, err_message):
      """
      连接状态监听
      :param connected: 布尔型，true表示已连接，false未连接
      :param err_message: 字符串类型，连接失败后的错误提示信息
      """
      print(connected)
      self.connected = connected
      if (connected == False):
        print(err_message)
  
    def system_state(self, state):
      """
      系统状态回馈
      :param state: 字典型，系统状态，格式为{
        'carState': '',   // 车体状态，字符串类型
        'mode': '',       // 控制模式，字符串类型
        'voltage': ,      // 电压，浮点型
        'fault_01': '',   // 错误1，字符串类型
        'fault_02': ''    // 错误2，字符串类型
      }
      """
      self.__system_cnt += 1
      if (self.__system_cnt % 10 == 0):
        # print(state)
        self.__ui.carState.setText(state['carState'])
        self.__ui.mode.setText(state['mode'])
        self.__ui.voltage.setText(str(state['voltage']))
        self.__ui.fault_01.setText(state['fault_01'])
        self.__ui.fault_02.setText(state['fault_02'])
  
    def motion_state(self, state):
      """
      运动状态回馈
      :param state: 字典型，运动状态，格式为 {
        'speed': ,       // 运动速度，单位m/s，浮点型
        'angle':         // 转向角度，单位rad，浮点型
      }
      """
      self.__motion_cnt += 1
      if (self.__motion_cnt % 10 == 0):
        # print(state)
        self.__ui.speed.setText(str(state['speed']))
        self.__ui.angle.setText(str(state['angle']))
  
    def set_ui(self, ui):
      """
      设置可视组件操作句柄
      """
      self.__ui = ui
  ```

  demo.py：该文件实现车载端的远程连接和操控。

  ```python
  def connect(ui):
    """
    连接服务器
    """
    access_code = ui.accessCode.text()
    access_pwd = ui.accessPwd.text()
    if (access_code.isspace() or len(access_code) == 0 or access_pwd.isspace() or len(access_pwd) == 0):
      QtWidgets.QMessageBox.warning(None, "警告", "请输入接入代码和接入密码！", QtWidgets.QMessageBox.Yes)
      print('请输入接入代码和接入密码')
      return
    global freewalker_ctrl
    global demo_client_listener
    global connect_
    demo_client_listener = DemoClientListener()
    demo_client_listener.set_ui(ui)
    freewalker_ctrl = FreeWalkerCtrl()
    freewalker_ctrl.set_client_listener(demo_client_listener)
    freewalker_ctrl.connect_to_host(access_code, access_pwd)
    connect_ = True
      
  def disconnect():
    """
    关闭连接
    """
    global connect_
    if (connect_ == False or demo_client_listener.connected == False):
      QtWidgets.QMessageBox.warning(None, "警告", "请先连接！", QtWidgets.QMessageBox.Yes)
      return
    freewalker_ctrl.disconnect()
    connect_ = False
  
  def front():
    """
    前进
    """
    if (connect_ == False or demo_client_listener.connected == False):
      QtWidgets.QMessageBox.warning(None, "警告", "请先连接！", QtWidgets.QMessageBox.Yes)
      return
    freewalker_ctrl.send_ctrl_ins(CtrlState.FRONT_AND_BACK, CtrlState.MOTION, 30)
  
  def back():
    """
    后退
    """
    if (connect_ == False or demo_client_listener.connected == False):
      QtWidgets.QMessageBox.warning(None, "警告", "请先连接！", QtWidgets.QMessageBox.Yes)
      return
    freewalker_ctrl.send_ctrl_ins(CtrlState.FRONT_AND_BACK, CtrlState.MOTION, -30)
   
  def left():
    """
    向左
    """
    if (connect_ == False or demo_client_listener.connected == False):
      QtWidgets.QMessageBox.warning(None, "警告", "请先连接！", QtWidgets.QMessageBox.Yes)
      return
    freewalker_ctrl.send_ctrl_ins(CtrlState.LEFT_AND_RIGHT, CtrlState.MOTION, 30)
  
  def right():
    """
    向右
    """
    if (connect_ == False or demo_client_listener.connected == False):
      QtWidgets.QMessageBox.warning(None, "警告", "请先连接！", QtWidgets.QMessageBox.Yes)
      return
    freewalker_ctrl.send_ctrl_ins(CtrlState.LEFT_AND_RIGHT, CtrlState.MOTION, -30)
  
  def keep_fornt():
    """
    保持前行
    """
    if (connect_ == False or demo_client_listener.connected == False):
      QtWidgets.QMessageBox.warning(None, "警告", "请先连接！", QtWidgets.QMessageBox.Yes)
      return
    keep_front_timer(0.1)
  
  def stop_front():
    """
    停止前行
    """
    if (connect_ == False or demo_client_listener.connected == False):
      QtWidgets.QMessageBox.warning(None, "警告", "请先连接！", QtWidgets.QMessageBox.Yes)
      return
    timer.cancel()
  
  def keep_front_timer(inc):
    global timer
    front()
    timer = Timer(inc, keep_front_timer, (inc,))
    timer.start()
  ```

  [python版sdk下载](http://39.98.34.145/storage/2021/01-27/RYBhogXg5Lm2Crr2JPs9blCogARmUAhZ9dUCRoiR.zip)

  [示例下载](http://39.98.34.145/storage/2021/01-27/n7CpqyKkCnoX5f5nMpHcg9lZhCZ2DQTEtAfiWa5l.zip)