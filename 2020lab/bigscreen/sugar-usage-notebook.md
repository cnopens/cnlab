#sugar-usage-notebook.md
---
***Docker pull***

docker pull hub.baidubce.com/sugarbi/sugar:2.3.2
docker tag hub.baidubce.com/sugarbi/sugar:2.3.2 sugarbi/sugar:2.3.2
// 查看刚刚拉取的镜像
docker images

***env文件***

love_sugar=true
sugar_can_connect_local_ip=true

# MySQL数据库相关
sugar_db_username=root
sugar_db_password=123.com
sugar_db_database=sugarbi
#填写您安装MySQL机器的IP，即使是本机也不能使用localhost和127.0.0.1
sugar_db_host=labroom 
#sugar-mysql=
sugar_db_port=3306

# redis配置
# 如果您需要「定时邮件」功能，需要配置这块，否侧不需要配置
# 如果您私有部署只是单机运行（即只运行一个sugar的实例），sugar已经内置，这块不需要配置
# 否则需要您自己搭建redis（2.6版本以上，最好是5的版本，可以使用类似上面mysql的docker方式来启动一个redis）
sugar_redis_host=
sugar_redis_port=
sugar_redis_password=

# license相关
sugar_company=personal
sugar_license=kPfszKHz31qvd1win942PM27PsiEDyAceSIVPVV5NBUJZONRywFZ7H58XQY1mJw0f/2mNOmd6zlofeh82i5CY8dilrWmuqGPUfarlR9SOldD8xDFf1zkGEpDaDUdOg0yl3BSobM4OGoW3haquPwBUCXq83msJtx6IVdnu/CHB7mHaH3Kk/tQ8plryH4+DrdramdOj+rJ1wgKV0ksZeUfvnjJE7BEZeLXQMD2waq/cs00pyDjSuXQ+Q2nAGyvxINR

# 登陆形式，支持 demo、email、password、oauth
# 如果是最初步的测试，可以使用demo方式，后面的其他登陆方式的配置都可以不用填写
sugar_login_type=demo

# email登陆模式的配置，以及「定时邮件」功能也需要这块的配置，host 是 smtp 服务器
sugar_email_host=
# port 一般是 25 或 465
sugar_email_port=
sugar_email_username=
sugar_email_password=
sugar_email_from=
# 如果 smtp 端口是 465 则需要去掉下面的注释
# sugar_email_tls=true

# oauth登陆模式的配置
sugar_oauth_authorize_url=
sugar_oauth_token_url=
sugar_oauth_client_id=
sugar_oauth_client_secret=
sugar_oauth_scope=
sugar_oauth_info_url=
sugar_oauth_data_username=
sugar_oauth_data_useremail=
sugar_oauth_email_suffix=
sugar_oauth_logout_url=

# 是否HTTPS，只有您部署的sugar并且设置了HTTPS证书时才需要配置
sugar_is_https=
# 是否忽略SQL语句的安全检查（准许SQL中出现drop、delete等关键词），默认是都检查，填写true时将不检查，1.9.1版本之后才支持
sugar_ignore_sql_security_check=false
# 私有部署下是否不显示home首页，直接跳到login登录页面
sugar_home_redirect_login=false

在env文件所在目录执行

docker run --ulimit nofile=65100:65100 --restart unless-stopped -d -p 8000:8580 --name sugar -v ~/dashuo/sugar-log:/sugar-app/log --env-file env sugarbi/sugar:2.3.2


有时您的 docker 网络环境可能配置有问题，会导致 MySQL 数据库连接不上，Sugar 还有另外一种方式 link 的方式启动。如果您的 MySQL 和 Sugar 运行在同一台机器上，并且 MySQL 是使用上面的 docker 方式启动的，您可以在env文件中将sugar_db_host填写为sugar-mysql，然后使用下面的命令来启动：

docker run --link sugar-mysql --ulimit nofile=65100:65100 --restart unless-stopped -d -p 8000:8580 --name sugar -v ~/sugar-log:/sugar-app/log --env-file env sugarbi/sugar:2.3.2




## sugar knowlegue

### 部署doc
https://cloud.baidu.com/doc/SUGAR/s/Gjyth5q2n


## sugar sourcecode analyse

***javakit***


***supervisord***

进程管理工具


***dumb-init***
A minimal init system for Linux containers

resource: https://github.com/Yelp/dumb-init

example:
-------
/usr/bin/dumb-init -- /bin/sh -c /usr/local/bin/supervisord -c /etc/supervisord.conf; bash /sugar-app/addhostname.sh; pm2-runtime --max-memory-restart 1G app.js

/bin/sh -c /usr/local/bin/supervisord -c /etc/supervisord.conf; bash /sugar-app/addhostname.sh; pm2-runtime --max-memory-restart 1G app.js

/usr/bin/python /usr/local/bin/supervisord -c /etc/supervisord.conf

 node /usr/local/bin/pm2-runtime --max-memory-restart 1G app.js

 node /sugar-app/app.js

 java -Xmx6g -DconfigurationFile=log.xml -Dvertx.options.workerPoolSize=40 -cp libs/*:javakit.jar com.baidu.sugar.Main

 /usr/bin/redis-server *:6379

***EXASOL***
This guide will assist you during your initial steps with EXASOL such as setting up the database, loading data from various sources or connecting certain BI tools.

Directly importing data from databases project:
https://github.com/cnopens/database-migration

