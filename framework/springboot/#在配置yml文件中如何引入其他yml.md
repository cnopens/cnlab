#在配置yml文件中如何引入其他yml.md

*Note : 在开发过程中如果需要其他的配置文件，全部写到application.yml中感觉太臃肿，这时，我们可以将yml拆分成多个yml文件


```
在配置文件目录（如：resources）下新建application-xx开头的yml文件

application-test.yml
application-prod.yml
application-dev.yml

然后再在application.yml中添加配置

spring:
  profiles:
      include:
          test,api,jdbc

注意，不能换行，include:下面的test,api,jdbc，多个用英文逗号分隔

然后在程序中就同时可以访问test,prod,dev中的东西了
```
