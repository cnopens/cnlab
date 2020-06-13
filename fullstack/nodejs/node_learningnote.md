#Simple Basic Knowlegue


##Resources 

Nodejs advance function:
---
[https://github.com/azat-co/node-advanced]



#Practice 
---
<title>vue+node+mongodb</title>

[https://github.com/Aimeezp/vue-node-blog]


## Npm commands Practice

Switch npm source:

注册源
npm config set registry https://registry.npm.taobao.org;

查看源
npm config get registry:

official : http://www.npmjs.org/

查看所有Config信息
npm config list

---
安装cnpm
npm install -g cnpm --registry=https://registry.npm.taobao.org



later you may use cnpm replace npm..

---



## General Knowlegues
--- 
<pre>
npm -i 与npm install -s与-d的区别
npm i module_name  -S  = >  npm install module_name --save    写入到 dependencies 对象

npm i module_name  -D  => npm install module_name --save-dev   写入到 devDependencies 对象

npm i module_name  -g  全局安装
  i 是install 的简写

-S就是--save的简写
-D就是--save-dev 这样安装的包的名称及版本号就会存在package.json的devDependencies这个里面，而--save会将包的名称及版本号放在dependencies里面。

我们在使用npm install 安装模块或插件的时候，有两种命令把他们写入到 package.json 文件里面去，比如：

--save-dev

--save

在 package.json 文件里面提现出来的区别就是，使用 --save-dev 安装的 插件，被写入到 devDependencies 对象里面去，而使用 --save 安装的插件，责被写入到 dependencies 对象里面去。

那 package.json 文件里面的 devDependencies  和 dependencies 对象有什么区别呢？

devDependencies  里面的插件只用于开发环境，不用于生产环境，而 dependencies  是需要发布到生产环境的
</pre>
--- 

## Global pacakge module config
