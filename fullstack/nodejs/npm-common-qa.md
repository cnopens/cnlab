# npm-common-qa.md

https://registry.npmjs.org

npm config

 设置npm的registry

1.原npm地址

npm config set registry http://registry.npmjs.org 

2.设置国内镜像

a.通过config命令

npm config set registry https://registry.npm.taobao.org 
npm info underscore （如果上面配置正确这个命令会有字符串response）

b.命令行指定

npm --registry https://registry.npm.taobao.org info underscore 

c.编辑 ~/.npmrc 加入下面内容

registry = https://registry.npm.taobao.org

3.使用nrm管理registry地址

a.下载nrm

npm install -g nrm

b.添加registry地址

nrm add npm http://registry.npmjs.org

nrm add taobao https://registry.npm.taobao.org

c.切换npm registry地址

nrm use taobao

nrm use npm

搜索镜像: https://npm.taobao.org

建立或使用镜像,参考: https://github.com/cnpm/cnpmjs.org

**********************************************************************************************************


1. 7 error Unexpected end of JSON input while parsing near '..."shasum":"0a6b3916a7d'?
 
 resolve：
（1） npm install --registry=https://registry.npm.taobao.org --loglevel=silly
（2） npm cache clean --force
（3） npm install

2. 为什么node_modules里面会有.staging这个东西?
 resolve:
 staging是在运行过程中出现的，当真正install所有包之后，node_modules里面的包就会显示正常，也就是，出现这个就是没有加载完

3. verbose stack FetchError: Response timeout while trying to fetch https://registry.npm.taobao.org/@types%2fnode (over 30000ms)?

4. npm WARN tar ENOENT: no such file or directory, lstat ' 一直失败？
resolve：
1：删除package-lock.josn
2：npm install

5. npm ERR! Verification failed while extracting echarts@^4.0.4？
resolve:(完美解决)
npm cache verify 
