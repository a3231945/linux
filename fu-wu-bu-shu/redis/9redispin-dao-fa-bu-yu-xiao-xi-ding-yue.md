###redis频道发布与消息订阅

	发布一个频道：publish news 'today is sunshine'
	
	订阅消息：	  subscribe news
	 
	模糊订阅消息： psubscribe new*
	
	适合群聊在线聊天室，一个消息推送，都能收到消息
