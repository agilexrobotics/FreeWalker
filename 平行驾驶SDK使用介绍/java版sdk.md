- 获取接入代码和接入密码信息，请联系厂家。

- 下载[java版sdk](http://39.98.34.145/storage/2021/01-27/oaPgwK8JMfiRHvwueEz9Uyd0I0G06kANVolPkLlR.zip)，该sdk基于jdk1.8版本开发编译，***第三方jar依赖<u>netty</u>包，使用的版本为<u>4.1.54.Final</u>，使用时切记导入该依赖项***。

- 重要类和接口说明

  TcpClient: 该类是单例的，使用时用TcpClient.getClient()获取操作对象。该类主要提供几下几个接口：

  ```java
     /**
       * 设置接入信息
       * 
       * @param accessCode
       *            接入代码
       * @param accessPwd
       *            接入密码
       */
      public void setAccessInfo(String accessCode, String accessPwd)
  ```

  ```java
     /**
       * 连接主机
       * 
       * @throws SocketException
       *             接入异常信息
       */
      public void connectToHost() throws SocketException
  ```

  ```java
     /**
       * 连接主机
       * 
       * @param accessCode
       *            接入代码
       * @param accessPwd
       *            接入密码
       * @throws SocketException
       *             接入异常信息
       */
      public void connectToHost(String accessCode, String accessPwd) throws SocketException
  ```

  ```java
      /**
       * 发送控制指令
       * 
       * @param montion
       *            设置运动类型，前后还是左右
       * @param state
       *            设置运动状态，运动还是停止
       * @param value
       *            设置速度或转向值，取值范围均为[-100, 100]
       * @return
       */
      public void sendCtrlIns(Constants.CtrlState montion, Constants.CtrlState state, int value)
  ```

  ```java
      /**
       * 关闭连接
       */
      public void disconnect()
  ```

  Constants: 常量定义类，当前主要定义了小车控制相关枚举类型

  ```java
      /**
       * 控制状态
       */
      public enum CtrlState {
          FRONT_AND_BACK, // 前后
          LEFT_AND_RIGHT, // 左右
          MOTION, // 运动
          STOP // 停止
      }
  ```

  ClientListener接口：该接口可由使用者实现，实现类在TcpClient中注入，通过该接口可接收车体返回的实时数据，如电压、速度、转向、故障等信息。

  ```java
  public interface ClientListener {
      /**
       * 连接状态
       * 
       * @param connected
       *            是否连接
       * @param e
       *            连接失败后的异常信息
       */
      void connectStatus(boolean connected, SocketException e);
  
      /**
       * 获取实时系统状态
       * 
       * @param vol
       *            系统状态
       */
      void systemState(SystemStateVO vo);
  
      /**
       * 获取实时运动状态
       * 
       * @param vo
       *            运动状态
       */
      void motionState(MotionStateVO vo);
  }
  ```

  SocketException: sdk中定义的异常信息，用于包装通信相关异常，捕获后，可获取具体错误信息。

- [示例下载](http://39.98.34.145/storage/2021/01-27/91WxXGBb6vicKEyWmeLr4qYRRYh7yTbPmCM0FoH2.zip)，本示例基于maven，后台采用springboot2.4.0，前台界面基于element-ui2.13.1开发。

  导入过程这里不再赘述，启动工程后，打开访问地址：http://localhost:8080/service/main/index

  界面截图如下：


![size:800,1000](http://39.98.34.145/storage/2021/01-27/ZhlVRQtFnODgx9mBAoYI5YXWiqSouDSvLnQ1S0Y6.jpeg "接入信息截图")

  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;输入接入代码和接入密码后，点击“连接”按钮，弹出“连接成功”后，在“系统状态”和“运动反馈”可以观察到小车的实时运动信息。

  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;点击“保持前进”按钮，该功能点会保持小车一直处于向前行驶状态，点击“停止”按钮可关闭。

  &nbsp;&nbsp;&nbsp;&nbsp;“前进”、“后退”、“向左”、“向右”可观察小车前后左右运动控制，示例中仅仅是演示，每点击一次，小车运动一下之后立刻停止。所以需要小车持续运动，需要持续不停发送运动指令。

  重点关注示例代码：

  src/main/java/com/agx/freewalker/ctrl/demo/controller/CtrlController.java

  ```java
  @Slf4j
  @RestController
  @RequestMapping("/service/ctrl")
  public class CtrlController {
  
      private TcpClient client = TcpClient.getTcpClient();
  
      /**
       * 连接远程小车 注意，该连接无法确认连接是否成功，需要监控ClientListener中的connectStatus返回结果
       * 
       * @param accessCode
       * @param accessPwd
       * @return
       */
      @GetMapping("/connect")
      public Response connect(@RequestParam String accessCode, @RequestParam String accessPwd) {
          int status = -1;
          try {
              // 注册回调
              client.setClientListener(new DemoClientListener());
              client.connectToHost(accessCode, accessPwd);
              status = 1;
          } catch (SocketException e) {
              log.error("connect error", e);
          }
          return new Response(status);
      }
  
      /**
       * 关闭连接
       * 
       * @return
       */
      @GetMapping("/disconnect")
      public Response disconnect() {
          int status = -1;
          try {
              client.disconnect();
              status = 1;
          } catch (Exception e) {
              log.error("disconnect error", e);
          }
          return new Response(status);
      }
  
      /**
       * 控制小车运动
       * 
       * @param motion
       *            1控制前后移动 2控制左右转向移动
       * @param status
       *            1移动 2停止
       * @param value
       *            数值介于[-100, 100]，前后移动时：数值正表示前进，负表示后退；左右移动时：正表示左转，负表示右转
       * @return
       */
      @GetMapping("/move")
      public Response move(@RequestParam int motion, @RequestParam int state, @RequestParam int value) {
          int status = -1;
          CtrlState _motion = motion == 1 ? CtrlState.FRONT_AND_BACK : CtrlState.LEFT_AND_RIGHT;
          CtrlState _state = state == 1 ? CtrlState.MOTION : CtrlState.STOP;
          try {
              client.sendCtrlIns(_motion, _state, value);
              status = 1;
          } catch (Exception e) {
              log.error("发送控制命令失败", e);
          }
          return new Response(status);
      }
    }
  ```

  src/main/resources/static/pages/index.html

  ```javascript
  	    	   // 连接
  	    	   connect() {
  	    		   this.$refs['form'].validate((valid) => {
  	    			   if (valid) {
  	    			       $.get("/service/ctrl/connect", this.form, (data) => {
  	    			    	   LOG.info(data);
  	    			    	   if (data.status === 1) {
  	    			    		   alert('连接成功');
  	    			    	   } else {
  	    			    		   this.connected = false;
  	    			    		   alert('连接失败');
  	    			    	   }
  	    			       });
  	    			   }
     		               return false;
     		           });
  	    	   },
  	    	   // 关闭连接
  	    	   disconnect() {
      			   $.get('/service/ctrl/disconnect', this.form, (data) => {
      				   LOG.info(data);
                            if (data.status === 1) {
                                this.connected = false;
                                alert('断开连接成功');
                            } else {
                                alert('断开连接失败');
                            }
      			   });
  	    	   },
  	    	   // 向前
  	    	   front() {
  	    		   if (!this.connected) {
                         alert('请先连接');
                         return;
                     }
  	    		   const params = {
                        motion: 1,
                        state: 1,
                        value: 30
                     };
  	    		   this._move(params);
  	    	   },
  	    	   // 向后
  	    	   back() {
  	    		   if (!this.connected) {
                         alert('请先连接');
                         return;
                     }
  	    		   const params = {
                         motion: 1,
                         state: 1,
                         value: -30
                     };
                     this._move(params);
  	    	   },
  	    	   // 向左
  	    	   left() {
  	    		   if (!this.connected) {
                         alert('请先连接');
                         return;
                     }
  	    		   const params = {
                         motion: 2,
                         state: 1,
                         value: 30
                     };
                     this._move(params);
  	    	   },
  	    	   // 向右
  	    	   right() {
  	    		   if (!this.connected) {
                         alert('请先连接');
                         return;
                     }
  	    		   const params = {
                         motion: 2,
                         state: 1,
                         value: -30
                     };
                     this._move(params);
  	    	   },
  	    	   // 持续向前
  	    	   keepFront() {
  	    		   if (!this.connected) {
  	    			   alert('请先连接');
  	    			   return;
  	    		   }
  	    		   const params = {
                         motion: 1,
                         state: 1,
                         value: 30
                     };
  	    		   this.frontInterval = setInterval(() => {
  	    			   this._move(params);
  	    		   }, 50);
  	    	   },
  	    	   // 关闭持续向前
  	    	   stopFront() {
  	    		   clearInterval(this.frontInterval);
  	    	   },
  	    	   _move(params) {
                     $.get("/service/ctrl/move", params);
  	    	   },
  ```

  [java版sdk下载](http://39.98.34.145/storage/2021/01-27/oaPgwK8JMfiRHvwueEz9Uyd0I0G06kANVolPkLlR.zip)

  [示例下载](http://39.98.34.145/storage/2021/01-27/91WxXGBb6vicKEyWmeLr4qYRRYh7yTbPmCM0FoH2.zip)