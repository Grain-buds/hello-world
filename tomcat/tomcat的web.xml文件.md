Tomcat web.xml详解  
https://blog.csdn.net/wangxiaotongfan/article/details/51318951

web.xml中配置session属性:  
https://blog.csdn.net/ch717828/article/details/48542815


tomcat配置httponly属性：  
httponly属性：  
如果在Cookie中设置了"HttpOnly"属性，那么通过程序(JS脚本、Applet等)将无法读取到Cookie信息，这样能有效的防止XSS攻击  
处理办法：
1、修改tomcat/conf/context.xml  
```
<Context useHttpOnly="true"></context>
```
2、修改tomcat/conf/web.xml  
```
<session-config>  
    <cookie-config>   
        <http-only>true</http-only>  
    </cookie-config> 
</session-config> 
```
3、修改tomcat/conf/server.xml
<Connector port="8080" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" secure="true" />  
https://liverpool.blog.csdn.net/article/details/45531665