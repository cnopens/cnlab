#npm-yarn-etc-常见问题.md

1. Browserslist: caniuse-lite is outdated. Please run next command `npm update`

今天打包vue项目，突然蹦出一个告警：

Browserslist: caniuse-lite is outdated. Please run next command `npm update`

    1

按照提示操作，运行npm update也没有解决。

于是我查询了一下npm手册，得知是不能直接运行npm update的，必须带上包名，所以应该这样写命令：

npm update caniuse-lite

    1

或者直接删了node_modules/caniuse-lite文件夹，然后重新安装：

npm i -g caniuse-lite

    1

这样问题就解决了！