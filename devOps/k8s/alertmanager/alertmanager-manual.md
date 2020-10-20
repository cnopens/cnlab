#alertmanager-manual.md

----
## alertmanager key config 

alertmanager会定义一个基于标签匹配规则的路由树，用以接收到报警后根据不同的标签匹配不同的路由，来将报警发送给不同的receiver。如果定义一个路由route则需要接收并处理所有的报警，如果需要区分不同的报警发送给不同的receiver，则需要配置多个子级route来处理不同的报警，而根route此时必须能够接收处理所有的报警。

根路由
route:
  #顶级路由配置的接收者（匹配不到子级路由，会使用根路由发送报警）
  receiver: 'default-receiver'
  #设置等待时间，在此等待时间内如果接收到多个报警，则会合并成一个通知发送给receiver
  group_wait: 30s
  #两次报警通知的时间间隔，如：5m，表示发送报警通知后，如果5分钟内再次接收到报警则不会发送通知
  group_interval: 5m
  #发送相同告警的时间间隔，如：4h，表示4小时内不会发送相同的报警
  repeat_interval: 4h

  #分组规则，如果满足group_by中包含的标签，则这些报警会合并为一个通知发给receiver (重要)
  group_by: [cluster, alertname]

  routes:
  #子路由的接收者
  - receiver: 'database-pager'
    group_wait: 10s
    #默认为false。false：配置到满足条件的子节点点后直接返回 true：匹配到子节点后还会继续遍历后续子节点
    continue:false
    #正则匹配，验证当前标签service的值是否满足当前正则的条件
    match_re:
      service: mysql|cassandra
  #子路由的接收者
  - receiver: 'frontend-pager'
    group_by: [product, environment]
    #字符串匹配，匹配当前标签team的值为frontend的报警
    match:
      team: frontend

#定义所有接收者
receivers:
#接收者名称
- name: 'default-receiver'
  #接收者为webhook类型
  webhook_configs:
  #webhook的接收地址
  - url: 'http://127.0.0.1:5001/'
- name: 'database-pager'
  webhook_configs:
  - url: 'http://127.0.0.1:5002/'
- name: 'frontend-pager'
  webhook_configs:
  - url: 'http://127.0.0.1:5003/'


## alertmanager webhook config example:
````
{
    "global":{
        "resolve_timeout":"30s",
        "smtp_smarthost":"localhost:25",
        "smtp_from":"alertmanager@example.org",
        "smtp_auth_username":"alertmanager",
        "smtp_auth_password":"password"
    },
    "route":{
        "group_by":[
            "dscNamespace",
            "dscStrategyName",
            "dscTargetName",
            "dscTargetType",
            "dscClusterID"
        ],
        "group_wait":"5s",
        "group_interval":"5m",
        "repeat_interval":"0s",
        "receiver":"default",
        "routes":[
            {
                "match_re":{
                    "alertname":".*"
                },
                "continue":true,
                "receiver":"five-minutes",
                "group_interval":"5m"
            },
            {
                "match_re":{
                    "alertname":".*"
                },
                "continue":true,
                "receiver":"ten-minutes",
                "group_interval":"10m"
            }
        ]
    },
    "receivers":[
        {
            "name":"default"
        },
        {
            "name":"five-minutes",
            "webhook_configs":[
                {
                    "url":"http://192.168.31.132:19001/spi/v2/alerts/notifications/intervals/300",
                    "send_resolved":false
                },
                {
                    "url":"http://192.168.31.132:19001/v1/alert/cluster/CID-01c8cfdf4b8e/notifications/intervals/300",
                    "send_resolved":false
                }
            ]
        },
        {
            "name":"ten-minutes",
            "webhook_configs":[
                {
                    "url":"http://192.168.31.132:19001/spi/v2/alerts/notifications/intervals/600",
                    "send_resolved":false
                }
            ]
        }
    ]
}
````
转的yml

```
global:
  resolve_timeout: 30s
  smtp_smarthost: localhost:25
  smtp_from: alertmanager@example.org
  smtp_auth_username: alertmanager
  smtp_auth_password: password
route:
  group_by:
  - dscNamespace
  - dscStrategyName
  - dscTargetName
  - dscTargetType
  - dscClusterID
  group_wait: 5s
  group_interval: 5m
  repeat_interval: 0s
  receiver: default
  routes:
  - match_re:
      alertname: ".*"
    continue: true
    receiver: five-minutes
    group_interval: 5m
  - match_re:
      alertname: ".*"
    continue: true
    receiver: ten-minutes
    group_interval: 10m
receivers:
- name: default
- name: five-minutes
  webhook_configs:
  - url: http://192.168.31.132:19001/spi/v2/alerts/notifications/intervals/300
    send_resolved: false
  - url: http://192.168.31.132:19001/v1/alert/cluster/CID-01c8cfdf4b8e/notifications/intervals/300
    send_resolved: false
- name: ten-minutes
  webhook_configs:
  - url: http://192.168.31.132:19001/spi/v2/alerts/notifications/intervals/600
    send_resolved: false
```

dev-环境alertmanager-config:
 
```
apiVersion: v1
data:
  alertmanager.json: '{"global":{"resolve_timeout":"30s","smtp_smarthost":"localhost:25","smtp_from":"alertmanager@example.org","smtp_auth_username":"alertmanager","smtp_auth_password":"password"},"route":{"group_by":["dscNamespace","dscStrategyName","dscTargetName","dscTargetType","dscClusterID"],"group_wait":"5s","group_interval":"5m","repeat_interval":"0s","receiver":"default","routes":[{"match_re":{"alertname":".*"},"continue":true,"receiver":"five-minutes","group_interval":"5m"},{"match_re":{"alertname":".*"},"continue":true,"receiver":"ten-minutes","group_interval":"10m"}]},"receivers":[{"name":"default"},{"name":"five-minutes","webhook_configs":[{"url":"http://192.168.31.132:19001/spi/v2/alerts/notifications/intervals/300","send_resolved":false},{"url":"http://192.168.31.138:9001/v1/alert/cluster/CID-01c8cfdf4b8e/notifications/intervals/300","send_resolved":true}]},{"name":"ten-minutes","webhook_configs":[{"url":"http://192.168.31.138:9001/spi/v2/alerts/notifications/intervals/600","send_resolved":false}]}]}'
kind: ConfigMap
metadata:
  creationTimestamp: "2020-07-23T10:49:34Z"
  name: alertmanager-config
  namespace: kube-system
  resourceVersion: "46839162"
  selfLink: /api/v1/namespaces/kube-system/configmaps/alertmanager-config
  uid: c215d065-df6c-45d9-b906-f9a6c1de6fcd
```
json格式化:

```
{
    "global":{
        "resolve_timeout":"30s",
        "smtp_smarthost":"localhost:25",
        "smtp_from":"alertmanager@example.org",
        "smtp_auth_username":"alertmanager",
        "smtp_auth_password":"password"
    },
    "route":{
        "group_by":[
            "dscNamespace",
            "dscStrategyName",
            "dscTargetName",
            "dscTargetType",
            "dscClusterID"
        ],
        "group_wait":"30s",
        "group_interval":"5m",
        "repeat_interval":"4h",
        "receiver":"default",
        "routes":[
            {
                "match_re":{
                    "alertname":".*"
                },
                "continue":true,
                "receiver":"five-minutes",
                "group_interval":"5m"
            },
            {
                "match_re":{
                    "alertname":".*"
                },
                "continue":true,
                "receiver":"ten-minutes",
                "group_interval":"10m"
            }
        ]
    },
    "receivers":[
        {
            "name":"default"
        },
        {
            "name":"five-minutes",
            "webhook_configs":[
                {
                    "url":"http://192.168.31.132:19001/spi/v2/alerts/notifications/intervals/300",
                    "send_resolved":false
                },
                {
                    "url":"http://192.168.31.138:9001/v1/alert/cluster/CID-01c8cfdf4b8e/notifications/intervals/300",
                    "send_resolved":true
                }
            ]
        },
        {
            "name":"ten-minutes",
            "webhook_configs":[
                {
                    "url":"http://192.168.31.138:9001/spi/v2/alerts/notifications/intervals/600",
                    "send_resolved":false
                }
            ]
        }
    ]
}

```


告警配置详解：

```

#根路由
route:
  #顶级路由配置的接收者（匹配不到子级路由，会使用根路由发送报警）
  receiver: 'default-receiver'
  #设置等待时间，在此等待时间内如果接收到多个报警，则会合并成一个通知发送给receiver
  group_wait: 30s
  #两次报警通知的时间间隔，如：5m，表示发送报警通知后，如果5分钟内再次接收到报警则不会发送通知
  group_interval: 5m
  #发送相同告警的时间间隔，如：4h，表示4小时内不会发送相同的报警
  repeat_interval: 4h
  #分组规则，如果满足group_by中包含的标签，则这些报警会合并为一个通知发给receiver
  group_by: [cluster, alertname]
  routes:
  #子路由的接收者
  - receiver: 'database-pager'
    group_wait: 10s
    #默认为false。false：配置到满足条件的子节点点后直接返回，true：匹配到子节点后还会继续遍历后续子节点
    continue:false
    #正则匹配，验证当前标签service的值是否满足当前正则的条件
    match_re:
      service: mysql|cassandra
  #子路由的接收者
  - receiver: 'frontend-pager'
    group_by: [product, environment]
    #字符串匹配，匹配当前标签team的值为frontend的报警
    match:
      team: frontend

#定义所有接收者
receivers:
#接收者名称
- name: 'default-receiver'
  #接收者为webhook类型
  webhook_configs:
  #webhook的接收地址
  - url: 'http://127.0.0.1:5001/'
- name: 'database-pager'
  webhook_configs:
  - url: 'http://127.0.0.1:5002/'
- name: 'frontend-pager'
  webhook_configs:
  - url: 'http://127.0.0.1:5003/'

```