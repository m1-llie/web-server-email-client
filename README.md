# web-server-email-client
邮件客户端+简易WEB服务器的设计实践
## 01 简要介绍
这是自己实现的一个网页版邮件客户端和与之配套的简易WEB服务器。</br>
邮件客户端基于socket与其他商用成熟邮件服务进行smtp和pop3连接，支持收发信件，用到了smtp、pop3、SSL协议相关的socket网络编程的知识，在java mail的框架下实现；</br>
WEB服务器能够对http请求和邮箱API访问请求作出响应，使用bootstrap和jQuery进行编写。
## 02 开发环境
（1）	开发工具：IntelliJ IDEA Educational Edition2020.1.1，VScode1.46.0，Chrome83.0.4103.106</br>
（2）	开发语言：Java，jdk版本为12.0.2</br>
（3）	系统环境：Win10
## 03 涉及技术
* socket套接字
* HTTP协议
* SMTP协议
* POP3协议
* WEB服务器响应原理
* 此外，Bootstrap框架、jQuery...
## 04 总体设计
邮件客户端示意如下：</br>
![](https://github.com/m1-llie/web-server-email-client/blob/master/readmeIMG/1.png)</br>
搭配WEB服务器后的整体流程示意如下：</br>
![](https://github.com/m1-llie/web-server-email-client/blob/master/readmeIMG/2.png)</br>
## 05 代码工程文件目录结构
![](https://github.com/m1-llie/web-server-email-client/blob/master/readmeIMG/3.png)</br>
Src目录下为webserver的实现，webServer为WEB服务器主程序，监听8080端口对http请求进行响应；webThread为连接线程，当监听到socket连接时，创建一个webThread进行响应；WebService为一个抽象类，是所有的可以部署在这个web服务器上的服务的主程序的父类，通过动态加载，将服务加载到服务器上进行运行。</br>
Webpage目录下的index.html是服务器的默认首页，mailAgentWeb文件夹下是基于web的邮件客户端的前端页面，error.html是若访问到其他页面的错误页面之后的提示页面。最后，以Mail开头的三个类是与邮件客户端的后端服务相关的类，负责对请求的响应作出回应，牵涉到了smtp协议和pop3协议的编写，在此处我们再次熟悉了socket网络编程的方法。
## 06 详细设计与实现
当web服务器模块启动时，首先会寻找项目根目录文件夹下面的serverConfig.properties文件，将文件中对于服务器的相关的的配置项加载到一个map中，当我们在进行相关的配置值的使用时，首先判断是否在配置文件已经覆盖，然后再进行默认值的加载配置。然后会遍历webpage目录中结尾为service的类，并通过Class.forName()进行加载之后放到服务的列表中，然后通过循环调用所有的类的start()函数启动服务线程，然后整个服务器启动完成。不仅可以实现对于8080端口的访问请求，而且可以根据webPage中的Service类中自定义的内容添加对应的服务模块进行服务的部署工作。</br>
以mailAgentService为例，在服务器运行的时候该服务会被分配一个线程而启动，用于响应关于邮件方面的http请求，用户通过web服务器的mailAgentWeb/index.html链接进入到邮箱服务的前端界面中，该前端界面是由自己实现的web服务器进行驱动的，然后用户可以在前端界面上进行邮件的发送和查看操作，并将通过前端进行响应。
## 07 系统部分演示
![](https://github.com/m1-llie/web-server-email-client/blob/master/readmeIMG/4.png)
![](https://github.com/m1-llie/web-server-email-client/blob/master/readmeIMG/5.png)
