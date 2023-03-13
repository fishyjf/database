Server:
	Service:容器，可以配置多个容器
		Connector:接收用户请求，可以配置多个，默认使用BIO Connector传统阻塞式IO，创建Request和Response对象用于和请求端交换数据；然后分配线程让Engine来处理这个请求，并把产生的Request和Response对象传给Engine。
			（1）通过配置第1个Connector，客户端可以通过8080端口号使用http协议访问Tomcat。其中，protocol属性规定了请求的协议，port规定了请求的端口号，redirectPort表示当强制要求https而请求是http时，重定向至端口号为8443的Connector，connectionTimeout表示连接的超时时间。在这个例子中，Tomcat监听HTTP请求，使用的是8080端口，而不是正式的80端口；实际上，在正式的生产环境中，Tomcat也常常监听8080端口，而不是80端口。这是因为在生产环境中，很少将Tomcat直接对外开放接收请求，而是在Tomcat和客户端之间加一层代理服务器(如nginx)，用于请求的转发、负载均衡、处理静态文件等；通过代理服务器访问Tomcat时，是在局域网中，因此一般仍使用8080端口。
			（2）通过配置第2个Connector，客户端可以通过8009端口号使用AJP协议访问Tomcat。AJP协议负责和其他的HTTP服务器(如Apache)建立连接；在把Tomcat与其他HTTP服务器集成时，就需要用到这个连接器。之所以使用Tomcat和其他服务器集成，是因为Tomcat可以用作Servlet/JSP容器，但是对静态资源的处理速度较慢，不如Apache和IIS等HTTP服务器；因此常常将Tomcat与Apache等集成，前者作Servlet容器，后者处理静态资源，而AJP协议便负责Tomcat和Apache的连接。
			Protocol:
				Connector使用哪种protocol，可以通过<connector>元素中的protocol属性进行指定，也可以使用默认值。指定的protocol取值及对应的协议如下：
					-   HTTP/1.1：默认值，使用的协议与Tomcat版本有关
					-   org.apache.coyote.http11.Http11Protocol：BIO
					-   org.apache.coyote.http11.Http11NioProtocol：NIO
					-   org.apache.coyote.http11.Http11Nio2Protocol：NIO2
					-   org.apache.coyote.http11.Http11AprProtocol：APR
					如果没有指定protocol，则使用默认值HTTP/1.1，其含义如下：在Tomcat7中，自动选取使用BIO或APR（如果找到APR需要的本地库，则使用APR，否则使用BIO）；在Tomcat8中，自动选取使用NIO或APR（如果找到APR需要的本地库，则使用APR，否则使用NIO）。
		Engine:处理Connector接收到的用户请求、执行java代码，唯一
			Host:虚拟主机，可以配置多个。Host虚拟主机的作用，是运行多个Web应用（一个Context代表一个Web应用），并负责安装、展开、启动和结束每个Web应用。Host组件代表的虚拟主机，对应了服务器中一个网络名实体(如”www.test.com”，或IP地址”116.25.25.25”)；为了使用户可以通过网络名连接Tomcat服务器，这个名字应该在DNS服务器上注册。客户端通常使用主机名来标识它们希望连接的服务器；该主机名也会包含在HTTP请求头中。Tomcat从HTTP头中提取出主机名，寻找名称匹配的主机。如果没有匹配，请求将发送至默认主机。因此默认主机不需要是在DNS服务器中注册的网络名，因为任何与所有Host名称不匹配的请求，都会路由至默认主机。
			Host虚拟主机的作用，是运行多个Web应用（一个Context代表一个Web应用），并负责安装、展开、启动和结束每个Web应用。Host组件代表的虚拟主机，对应了服务器中一个网络名实体(如”www.test.com”，或IP地址”116.25.25.25”)；为了使用户可以通过网络名连接Tomcat服务器，这个名字应该在DNS服务器上注册。客户端通常使用主机名来标识它们希望连接的服务器；该主机名也会包含在HTTP请求头中。Tomcat从HTTP头中提取出主机名，寻找名称匹配的主机。如果没有匹配，请求将发送至默认主机。因此默认主机不需要是在DNS服务器中注册的网络名，因为任何与所有Host名称不匹配的请求，都会路由至默认主机。
				Context:Web应用，Context元素最重要的属性是docBase和path，此外reloadable属性也比较常用。
				docBase指定了该Web应用使用的WAR包路径，或应用目录。需要注意的是，**在自动部署场景下****(****配置文件位于xmlBase****中)****，docBase****不在appBase****目录中，才需要指定；如果docBase****指定的WAR****包或应用目录就在docBase****中，则不需要指定**，因为Tomcat会自动扫描appBase中的WAR包和应用目录，指定了反而会造成问题。
				path指定了访问该Web应用的上下文路径，当请求到来时，Tomcat根据Web应用的 path属性与URI的匹配程度来选择Web应用处理相应请求。例如，Web应用app1的path属性是”/app1”，Web应用app2的path属性是”/app2”，那么请求/app1/index.html会交由app1来处理；而请求/app2/index.html会交由app2来处理。如果一个Context元素的path属性为””，那么这个Context是虚拟主机的默认Web应用；当请求的uri与所有的path都不匹配时，使用该默认Web应用来处理。但是，需要注意的是，**在自动部署场景下****(****配置文件位于xmlBase****中)****，不能指定path****属性，path****属性由配置文件的文件名、WAR****文件的文件名或应用目录的名称自动推导出来**。
				reloadable属性指示tomcat是否在运行时监控在WEB-INF/classes和WEB-INF/lib目录下class文件的改动。如果值为true，那么当class文件改动时，会触发Web应用的重新加载。在开发环境下，reloadable设置为true便于调试；但是在生产环境中设置为true会给服务器带来性能压力，因此reloadable参数的默认值为false。
				Valve:
					AccessLogValve:作用是通过日志记录其所在的容器中处理的所有请求
						pattern：指定记录日志的格式，本例中各项的含义如下：
								-   %h：远程主机名或IP地址；如果有nginx等反向代理服务器进行请求分发，该主机名/IP地址代表的是nginx，否则代表的是客户端。后面远程的含义与之类似，不再解释。
								-   %l：远程逻辑用户名，一律是”-”，可以忽略。
								-   %u：授权的远程用户名，如果没有，则是”-”。
								-   %t：访问的时间。
								-   %r：请求的第一行，即请求方法(get/post等)、uri、及协议。
								-   %s：响应状态，200,404等等。
								-   %b：响应的数据量，不包括请求头，如果为0，则是””-。
								-   **%D，含义是请求处理的时间(单位是毫秒)，对于统计分析请求的处理速度帮助很大。**

tomcat的连接数与线程池
https://www.cnblogs.com/kismetv/p/7806063.html