#k8s-learning-note.md


***k8s中文文档***：
--------------
http://www.coderdocument.com/docs/kubernetes/v1.14/tasks/access_applications_in_cluster/list_all_container_images_running_in_cluster.html

##kubernetes 查看pod 的容器日志

1.pod若处于运行状态，则通过kubectl logs 即可

- 查看指定pod的日志
kubectl logs <pod_name>
kubectl logs -f <pod_name> #类似tail -f的方式查看(tail -f 实时查看日志文件 tail -f 日志文件log)

- 查看指定pod中指定容器的日志
kubectl logs <pod_name> -c <container_name>

kubectl logs pod_name -c container_name -n namespace (一次性查看)
kubectl logs -f <pod_name> -n namespace (tail -f方式实时查看)

2.若pod处于init状态，则需要通过docker ps查看

-获取对应的pod name
kubectl get pods -n  namespace -o wide (STATUS是init的pod_name)

-通过docker ps 获取该pod的中的CONTAINER ID
docker ps | grep pod_name

-通过docker log获取对应的日志信息
docker logs CONTAINER_ID




##Kubernetes中pod和容器的关系

pod可以想象成一个篮子，而容器则是篮子里的鸡蛋，他们之间的关系主要变现为以下几点：

    一个pod里的容器能有多少资源也取决于这个篮子的大小。
    label也是贴在篮子上的。
    IP分配给篮子而不是容器，篮子里面的所有容器共享这个IP。（pod是IP等网络资源的分配的基本单位，这个IP及其对应的network namespace是由pod里的容器共享的；）
    哪怕只有一个鸡蛋（容器），Kubernetes仍然会给它分配一个篮子。

pod里的容器共享network namespace，并通过volume机制共享一部分存储。



## 常用命令
kubectl get pod -n kube-system

kubectl  delete pod -n kube-system alertmanager-78567898bc-2j54p

进入pod

进入container


## basic usage

*kubectl 创建对象

    $ kubectl create -f ./my-manifest.yaml     #创建资源

    $ kubectl create -f ./my1.yaml -f ./my2.yaml    #使用多个文件创建资源

    $ kubectl create -f ./dir    #使用目录下的所有清单文件（yaml）来创建资源

    $ kubectl create -f https://git.io/vPieo    #使用url创建资源

    $ kubectl run nginx --image=nginx    #启动一个nginx实例

    $ kubectl explain pods    #获取pod和svc的文档

*kubectl 显示和查找资源

    以下命令查找资源时可能查不到的原因是需要指定namespace，通过 -n <namespace>指定即可，或者all

    $ kubectl get pods --all-namespaces    #列出所有namespace中的pod，也可以是services、deployment等

    $ kubectl get pods -o wide    #列出pod并显示详细信息

    $ kubectl get deployment my-dep    #列出指定daployment

    $ kubectl get pods --include-uninitialized    #列出该namespace中的所有pod，包括未初始化的

    使用详细输出来描述命令

    $ kubectl describe nodes <my-node IP or name>    #查看node节点信息

    $ kubectl describe pods <my-pod>    #查看pod详细信息

    $ kubectl get services --sort-by=.metadata.name --all-namespaces    #l列出所有service并按名称排序

    根据重启次数排序列出pod

    $ kubectl get pods --sort-by='.status.containerStatuses[0].restartCount' --all-namespaces

    获取所有具有app=cassandra的pod中的version标签

    $ kubectl get pods --selector=app=cassandra rc -o jsonpath='{.items[*].metadata.labels.version}'

    获取所有节点的ExternalIP

    $ kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="ExternlIP")].address}'

*kubectl编辑资源

    $ kubectl -n codeus edit svc/c    #编辑codeus命名空间下名称为c的service

*kubectl Scale 资源

    扩展pod下容器数量

    $ kubectl scale --replicas=3 rs/foo    #扩展名称为foo的资源到3个，是否使用rs取决于yaml中的编写

    例如yaml中kind: Deployment ，则应通过下面方法扩展

    $ kubectl scale --replicas=3 deployment/foo

    或者直接通过创建资源的yaml文件扩展

    $ kubectl scale --replicas=3 -f foo.yaml

    根据判断条件扩展

    例如条件是：如果mysql的数量是2，则扩展到3

    $ kubectl scale --current-replicas=2 --replicas=3 deployment/mysql

    同时扩展多个资源

    $ kubectl scale --replicas=5 rc/foo rc/bar rc/baz

*kubectl 删除资源

    $ kubectl delete deployment <name>     #删除指定deployment，此方法还可以删除service等

    $ kubectl delete -f xxx.yaml    #通过创建此pod的yaml文件删除pod

*kubectl 与运行中的pod交互

    $ kubectl -n <namespaces> logs my-podname    #查看pod日志， -f 持续查看

    $ kubectl port-forward my-podname 5000:6000    #转发pod中的6000端口到本地的5000端口

    $ kubectl exec my-podname -- ls /    #在已存在的容器中执行命令




查看容器配置：
kubectl  get cm -n kube-system  -o yaml  prometheus-config


查看所有容器配置：
kubectl get cm -n kube-system


