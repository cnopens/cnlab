# java-产生ssl证书.md

一个Java程序需要访问一个https的网站的时候，可能需要涉及证书的安装，卸载等操作。

一、证书的下载

打开浏览器输入https://的网站，如果没有相关证书，可以根据提示从浏览器中下载下来，一般保存为*.cer文件。

二、证书的安装

1. 将步骤一的*.cer文件拷贝到"%JAVA_HOME%\jre\lib\security\cacerts"路径下
2. keytool -import -keystore cacerts -storepass changeit -keypass changeit -alias my-cas -file *.cer证书的绝对位置 

三、证书的查看

keytool -list -v -alias my-cas -keystore "%JAVA_HOME%\jre\lib\security\cacerts" -storepass changeit -keypass changeit

四、证书的删除

keytool -delete -v -alias my-cas -keystore "%JAVA_HOME%\jre\lib\security\cacerts" -storepass changeit -keypass changeit





keytool -import -alias baidu -keystore cacerts -file /home/cnopens/test.cer -trustcacerts