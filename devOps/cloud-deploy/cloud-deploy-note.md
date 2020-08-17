#cloud-deploy-note.md
---
Note: wanwan
---

产物仓库

项目仓库

设施调度引擎

持续交付

增值服务中心

智能运维监控管理中心


cloud12345

192.168.103.114  ：部署运营实施中心

192.168.103.115  master  :节点

192.168.103.116  node :节点  
cloud.base.yss.njenk=jenkins-master
cloud.base.yss.nsona=sonarqube
cloud.base.yss.nnex=nexus


当前模式下，持续交付
中⼼可以不规划性能
测试所需资源，可以选
取⾄少⼀台集群节点
来创建持续交付中⼼
对应的标签（即持续交
付中⼼的 master、slave
节点及静态检查端等
都会在同⼀个主机节
点中运⾏）nexus 建议
⾄少 4 核 CPU、4GB 内
存，需要通过标签固
定到单独节点主机。



192.168.103.117  node :节点  nexus


cloud.base.yss.ngit=gitlab
数据需要落地存 储到
节点主机磁 盘，应⽤
需要占⽤⾄ 少 1 核
CPU、4GB 内存


extendnal Ip: 118,119



手动添加标签：
手动给 node 创建标签：手动查看 node 标签：Kubectl label nodes node2 nrmq=nrmqkubectl get nodes --show-label

只能给 worker 运⾏所在的物理节点打标签,不包含 worker 运⾏的物理节点，不可打标签。

192.168.103.114


赢时胜云部署之持续集成部署安排结构

持续交付							
114	运营中心						
115	master						
116	node	jenkins 		master	"116
cloud.base.yss.njenk=jenkins
对应设施：

117

对应设施：ok
cloud.base.yss.ngit=gitlab
cloud.base.yss.njenk=jenkins-performance
118
cloud.base.yss.nnex=nexus
cloud.base.yss.nsona=sonarqube
cloud.base.yss.njenk=jenkins-master
对应设施："		
117	node	gitlab  jenkins-performance	与jenkins配置问题	slave			
118	node	nexus sonarqube jenkins-master					
119	node						
							


## QA: 													
1. "准备 slave 端的集群权限?
2. 【运营设施中心】运维管理员?
3. 创建分组区域？
4. master/slave怎么定义的？



							
							
				
## kins Location
http://192.168.100.47:31000/