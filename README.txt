
**************第一题****************
1.1 分别启动两个服务端zdy_rpc_provider
	在application.properties中:
	zookeeper配置：
		#zookeeper.address=119.28.226.31:2181
		zookeeper.address=127.0.0.1:2181
		zookeeper.timeout=5000
		zookeeper.basePath=/rpc_provider
	netty服务配置：
		netty.address=127.0.0.1
		netty.port=8990
	server端口号:
		server.port=8080
		
	运行RpcProviderApplication main方法启动
	
	修改配置  server端口号改为8081
			  netty服务端口号改为8991
	再次运行RpcProviderApplication main方法启动
	
1.2 zookeeper终端 可使用：ls2 /rpc_provider 查看

1.3 zdy_rpc_consumer客户端运行ClientBootStrap main方法启动netty客户端，会建立netty连接

1.4 停止其中一个服务端，客户端会监听到并neety断连，zookeeper中临时节点会消失
	启动一个服务端，客户端会监听到并建立连接，zookeeper中会创建临时节点

**************第二题****************
2.1 修改服务端application.properties 的server端口号  和  netty端口号  启动两个服务

2.2 zdy_rpc_consumer客户端运行ClientBootStrap main方法启动netty客户端

2.3 客户端会根据服务端响应时间，向响应时间小的服务端发送请求：
		客户端请求前会记录当前时间，响应后根据返回的requestId获取到服务器信息，并更新响应时间(响应的当前时间-请求时系统时间)
		如果某个服务端5秒内没有请求信息，ServerCostInfo中的isActive()方法结果为false


**************第三题****************
application.properties配置信息：

# MySQL数据库连接配置
mysql.datasource.url=jdbc:mysql://localhost:3306/springbootdata?useUnicode=true&characterEncoding=utf-8&allowMultiQueries=true&useSSL=false&serverTimezone=Asia/Shanghai
mysql.datasource.username=root
mysql.datasource.password=root
mysql.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# zookeeper config
zookeeper.address=127.0.0.1:2181
zookeeper.timeout=5000
zookeeper.datasource.config=/mysql_config

3.1.服务启动，会创建zkClient会话，并创建数据源bean对象，且将数据源信息保存到zookeeper中

3.2.zookeeper终端可使用命令  get /mysql_config 查看

3.3.终端修改数据库配置信息：set /mysql_config {"driverClassName":"com.mysql.cj.jdbc.Driver","password":"root","url":"jdbc:mysql://localhost:3306/bank?serverTimezone=UTC","username":"root"}

3.4.服务端监听到配置变化，获取配置，关闭数据源   并将新的配置set到数据源中并加载到spring中：
	SpringUtils.registerDataSourceBean("dataSource", sourceEntity);






