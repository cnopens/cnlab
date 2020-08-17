# prometheus-manual.md
---
告警规则：expression ->
完整警报配置：
 - alert: container_cpu_usage_seconds_total_gt_2100_4ec8b6091db2ae894148893de8c9198a
        expr: (ceil((1 - (avg by (instance) (irate(node_cpu{instance="master",mode="idle"}[5m]))))* 100 * 100)/100) > 21
        for: 5m --延时5分钟
        labels:
          dscClusterID: CID-01c8cfdf4b8e
          dscNamespace: admin
          dscStrategyID: STRAID-TmWyQXm8dBMt
          dscStrategyName: caixf-alert-test
          dscTargetName: master
          dscTargetType: node
        annotations:
          condition: CPU利用率 > 21%
          createTime: "2020-07-29T14:53:18+08:00"
          currentValue: CPU利用率 {{ $value }}%
          dscMetricType: cpu/usage_rate
          operator: '>'
          tokenMD5: ""

prometheus配置刷新：

curl -XPOST http://192.168.31.134:59090/-/reload


配置文件设置好后，prometheus-opeartor自动重新读取配置。
如果二次修改comfigmap 内容只需要apply

kubectl apply -f prometheus-k8s-rules.yaml


如何修改抓取间隔？

在项目里全局搜索interval这个词，所有有爬取间隔的配置位置就可以搜到了

 

在这里可以查看prometheus设置的各组件的爬取间隔：

http://192.168.31.134:59090/config


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
