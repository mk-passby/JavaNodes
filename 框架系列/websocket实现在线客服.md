---
title: springMVC使用websocket实现在线客服
date: 2017-10-17 20:01:00
tags: springmvc
---

# 1.环境

springMVC+spring+mybatis（spring4.0以上）

注意需要导入spring-websocket和websocket-api包。其余架包正常ssm即可，可自行百度

```xml
<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-websocket</artifactId>
      <version>4.0.5.RELEASE</version> 
    </dependency>
<dependency> 
    	<groupId>javax.websocket</groupId>
    	<artifactId>javax.websocket-api</artifactId>
    	<version>1.1</version>
    	<scope>provided</scope>
	</dependency>
```

# 2.后台代码

        用Map存放当前登录的账户及其对应的session

提出几点解释：

1.  @ServerEndpoint：把当前类变成websocket服务类
2.  @OnOpen： 连接时执行
3.  @OnClose：关闭时执行
4.  @OnMessage：收到消息时执行
5.  @OnError：连接错误时执行

```java

import java.io.IOException;
import java.util.HashMap;
import java.util.Map;
import java.util.Set;
import javax.websocket.OnClose;
import javax.websocket.OnError;
import javax.websocket.OnMessage;
import javax.websocket.OnOpen;
import javax.websocket.Session;
import javax.websocket.server.PathParam;
import javax.websocket.server.ServerEndpoint;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
/**
 * @Description: 简单websocket demo *
 */
@ServerEndpoint(value = "/websocketTest/{userId}")//@ServerEndpoint把当前类变成websocket服务类
public class WebsocketTest {
	private Logger logger = LoggerFactory.getLogger(WebsocketTest.class);
	private static String userId;
	// 连接的用户
	private static Map<String, Session> onlines = new HashMap<String, Session>();

	// 连接时执行
	@OnOpen
	public void onOpen(@PathParam("userId") String userId, Session session) throws IOException {
		this.userId = userId;
		onlines.put(userId, session);
		System.out.println("新连接：" + userId);

	}

	// 关闭时执行
	@OnClose
	public void onClose(@PathParam("userId") String userId) {
		if (onlines.containsKey(userId)) {
			onlines.remove(userId);
		}
		System.out.println("连接close：" + this.userId + " 关闭");
	}

	// 收到消息时执行
	@OnMessage
	public void onMessage(String message, Session session, @PathParam("userId") String userId) throws IOException {
		System.out.println(message);
		if (message.contains("&")) {
			String[] params = message.split("&");
			if (params[1].equals("all")) {
				System.out.println("广播");
				sendMessageToAll(userId + "发送广播消息:" + params[0]);
			} else {
				System.out.println("toUser");
				sendMessageToUser(userId, params[1], params[0]);
				;
			}
		}else
			 session.getBasicRemote().sendText(userId+"发送消息： " + message); 
		System.out.println(onlines.toString());
	}

	


	// 连接错误时执行
	@OnError
	public void onError(Session session, Throwable error) {
		System.out.println("用户id为：" + this.userId + "的连接发送错误");
		error.printStackTrace();
	}

	/**
	 * 广播消息给所有人
	 * **/
		private void sendMessageToAll(String message) {
			Set<String> users = onlines.keySet();
			for (String user : users) {
				try {
					if (onlines.get(user).isOpen()) {
						((Session) onlines.get(user)).getBasicRemote().sendText(message);
					}
				} catch (IOException e) {
					e.printStackTrace();
					break;
				}
			}

		}
	/******
	 *  给某个用户发送消息 
	 * @Param userName 发消息的name
	 * **/
		private void sendMessageToUser(String userName, String receiveName, String message) {
			Set<String> users = onlines.keySet();
			for (String user : users) {
				if (user.equals(receiveName)) {
					try {
						if (onlines.get(user).isOpen()) {
							System.out.println("user---" + user);
							((Session) onlines.get(user)).getBasicRemote().sendText(userName + "给你发了消息：" + message);
						}
					} catch (IOException e) {
						e.printStackTrace();
					}
					break;
				}
			}
		}
}

```

# 3.html

## 一、操作类html

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title></title>
</head>
<body>
	广播消息---- admin
	<br />
	<input id="text" type="text" />
	<button onclick="send()">Send</button>
	<select id="toUser" >
	<option id="toUser" value="all">ALL</option>
	<option id="toUser" value="user001">user001</option>
	<option id="toUser" value="user002">user002</option>
	</select>
	<button onclick="closeWebSocket()">Close</button>
	<div id="message"></div>
	<script type="text/javascript">
		//判断当前浏览器是否支持WebSocket
		if ('WebSocket' in window) {
			websocket = new WebSocket(
					"ws://localhost:8088/IM/websocketTest/admin");
			console.log("link success")
		} else {
			alert('Not support websocket')
		}
		//连接发生错误的回调方法
		websocket.onerror = function() {
			setMessageInnerHTML("error");
		};
		//连接成功建立的回调方法
		websocket.onopen = function(event) {
			setMessageInnerHTML("open");
		}
		console.log("-----")
		//接收到消息的回调方法
		websocket.onmessage = function(event) {
			setMessageInnerHTML(event.data);
		}
		//连接关闭的回调方法
		websocket.onclose = function() {
			setMessageInnerHTML("close");
		}
		//监听窗口关闭事件，当窗口关闭时，主动去关闭websocket连接，防止连接还没断开就关闭窗口，server端会抛异常。 
		window.onbeforeunload = function() {
			websocket.close();
		}
		//将消息显示在网页上
		function setMessageInnerHTML(innerHTML) {
			document.getElementById('message').innerHTML += innerHTML + '<br/>';
		}
		//关闭连接
		function closeWebSocket() {
			websocket.close();
		}
		//发送消息
		function send() {
			var message = document.getElementById('text').value;
			var user = document.getElementById('toUser').value;
			message+='&'+user;
			websocket.send(message);
		}
	</script>
</body>
</html>

```

## 二、接收类1

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title></title>
</head>
<body>
	websocket Demo---- user001
	<br />
	<input id="text" type="text" />
	<button onclick="send()">Send</button>
	<button onclick="closeWebSocket()">Close</button>
	<div id="message"></div>
	<script type="text/javascript">
		//判断当前浏览器是否支持WebSocket
		if ('WebSocket' in window) {
			websocket = new WebSocket(
					"ws://localhost:8088/IM/websocketTest/user001");
			console.log("link success")
		} else {
			alert('Not support websocket')
		}
		//连接发生错误的回调方法
		websocket.onerror = function() {
			setMessageInnerHTML("error");
		};
		//连接成功建立的回调方法
		websocket.onopen = function(event) {
			setMessageInnerHTML("open");
		}
		console.log("-----")
		//接收到消息的回调方法
		websocket.onmessage = function(event) {
			setMessageInnerHTML(event.data);
		}
		//连接关闭的回调方法
		websocket.onclose = function() {
			setMessageInnerHTML("close");
		}
		//监听窗口关闭事件，当窗口关闭时，主动去关闭websocket连接，防止连接还没断开就关闭窗口，server端会抛异常。 
		window.onbeforeunload = function() {
			websocket.close();
		}
		//将消息显示在网页上
		function setMessageInnerHTML(innerHTML) {
			document.getElementById('message').innerHTML += innerHTML + '<br/>';
		}
		//关闭连接
		function closeWebSocket() {
			websocket.close();
		}
		//发送消息
		function send() {
			var message = document.getElementById('text').value;
			websocket.send(message);
		}
	</script>
</body>
</html>

```

## 三、接收类2

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title></title>
</head>
<body>
	websocket Demo---- user002
	<br />
	<input id="text" type="text" />
	<button onclick="send()">Send</button>
	<button onclick="closeWebSocket()">Close</button>
	<div id="message"></div>
	<script type="text/javascript">
		//判断当前浏览器是否支持WebSocket
		if ('WebSocket' in window) {
			websocket = new WebSocket(
					"ws://localhost:8088/IM/websocketTest/user002");
			console.log("link success")
		} else {
			alert('Not support websocket')
		}
		//连接发生错误的回调方法
		websocket.onerror = function() {
			setMessageInnerHTML("error");
		};
		//连接成功建立的回调方法
		websocket.onopen = function(event) {
			setMessageInnerHTML("open");
		}
		console.log("-----")
		//接收到消息的回调方法
		websocket.onmessage = function(event) {
			setMessageInnerHTML(event.data);
		}
		//连接关闭的回调方法
		websocket.onclose = function() {
			setMessageInnerHTML("close");
		}
		//监听窗口关闭事件，当窗口关闭时，主动去关闭websocket连接，防止连接还没断开就关闭窗口，server端会抛异常。 
		window.onbeforeunload = function() {
			websocket.close();
		}
		//将消息显示在网页上
		function setMessageInnerHTML(innerHTML) {
			document.getElementById('message').innerHTML += innerHTML + '<br/>';
		}
		//关闭连接
		function closeWebSocket() {
			websocket.close();
		}
		//发送消息
		function send() {
			var message = document.getElementById('text').value;
			websocket.send(message);
		}
	</script>
</body>
</html>

```

4.具体效果

截图如下，实现简单web聊天功能。

