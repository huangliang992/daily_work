### websocket 编程实现聊天室
对于多个客户端，一个服务器端，要实现多个用户端之间信息的传送，同步，使用web socket十分合适，当一个客户端向服务器端发送消息后，服务器端可以向所有和它连接的客户端转发这个消息。

服务器端

	import java.text.SimpleDateFormat;
	import java.util.Date;
	
	import javax.websocket.OnClose;
	import javax.websocket.OnError;
	import javax.websocket.OnMessage;
	import javax.websocket.OnOpen;
	import javax.websocket.Session;
	import javax.websocket.server.ServerEndpoint;
	
	import net.sf.json.JSONObject;
	
	/**
	 * 聊天服务器类
	 * @author shiyanlou
	 *
	 */
	@ServerEndpoint("/websocket")
	public class ChatServer {
	    private static final SimpleDateFormat DATE_FORMAT = new SimpleDateFormat("yyyy-MM-dd HH:mm");    // 日期格式化
	
	    @OnOpen
	    public void open(Session session) {
	        // 添加初始化操作
	    }
	
	    /**
	     * 接受客户端的消息，并把消息发送给所有连接的会话
	     * @param message 客户端发来的消息
	     * @param session 客户端的会话
	     */
	    @OnMessage
	    public void getMessage(String message, Session session) {
	        // 把客户端的消息解析为JSON对象
	        JSONObject jsonObject = JSONObject.fromObject(message);
	        // 在消息中添加发送日期
	        jsonObject.put("date", DATE_FORMAT.format(new Date()));
	        // 把消息发送给所有连接的会话
	        for (Session openSession : session.getOpenSessions()) {
	            // 添加本条消息是否为当前会话本身发的标志
	            jsonObject.put("isSelf", openSession.equals(session));
	            // 发送JSON格式的消息
	            openSession.getAsyncRemote().sendText(jsonObject.toString());
	        }
	    }
	
	    @OnClose
	    public void close() {
	        // 添加关闭会话时的操作
	    }
	
	    @OnError
	    public void error(Throwable t) {
	        // 添加处理错误的操作
	    }
	}

客户端

	// 新建WebSocket对象，最后的/websocket对应服务器端的@ServerEndpoint("/websocket")
	var socket = new WebSocket('ws://${pageContext.request.getServerName()}:${pageContext.request.getServerPort()}${pageContext.request.contextPath}/websocket');
	            // 处理服务器端发送的数据
	            socket.onmessage = function(event) {
	                addMessage(event.data);
	            };
	            // 点击Send按钮时的操作
	            $('#send').on('click', function() {
	                var nickname = $('#nickname').val();
	                if (!um.hasContents()) {    // 判断消息输入框是否为空
	                    // 消息输入框获取焦点
	                    um.focus();
	                    // 添加抖动效果
	                    $('.edui-container').addClass('am-animation-shake');
	                    setTimeout("$('.edui-container').removeClass('am-animation-shake')", 1000);
	                } else if (nickname == '') {    // 判断昵称框是否为空
	                    //昵称框获取焦点
	                    $('#nickname')[0].focus();
	                    // 添加抖动效果
	                    $('#message-input-nickname').addClass('am-animation-shake');
	                    setTimeout("$('#message-input-nickname').removeClass('am-animation-shake')", 1000);
	                } else {
	                    // 发送消息
	                    socket.send(JSON.stringify({
	                        content : um.getContent(),
	                        nickname : nickname
	                    }));
	                    // 清空消息输入框
	                    um.setContent('');
	                    // 消息输入框获取焦点
	                    um.focus();
	                }
	            });
	
	            // 把消息添加到聊天内容中
	            function addMessage(message) {
	                message = JSON.parse(message);
	                var messageItem = '<li class="am-comment '
	                        + (message.isSelf ? 'am-comment-flip' : 'am-comment')
	                        + '">'
	                        + '<a href="javascript:void(0)" ><img src="assets/images/'
	                        + (message.isSelf ? 'self.png' : 'others.jpg')
	                        + '" alt="" class="am-comment-avatar" width="48" height="48"/></a>'
	                        + '<div class="am-comment-main"><header class="am-comment-hd"><div class="am-comment-meta">'
	                        + '<a href="javascript:void(0)" class="am-comment-author">'
	                        + message.nickname + '</a> <time>' + message.date
	                        + '</time></div></header>'
	                        + '<div class="am-comment-bd">' + message.content
	                        + '</div></div></li>';
	                $(messageItem).appendTo('#message-list');
	                // 把滚动条滚动到底部
	                $(".chat-content-container").scrollTop($(".chat-content-container")[0].scrollHeight);
	            }