#Istio-learningnote.md

---

微服务治理

灵活的微服务框架：基于 Istio 微服务框架提供可视化的微服务治理功能，将 Kubernetes 的服务进行更细粒度的拆分
    
完善的治理功能：支持熔断、灰度发布、流量管控、限流、链路追踪、智能路由等完善的微服务治理功能，同时，支持代码无侵入的微服务治理



DevOps

开箱即用的 DevOps：基于 Jenkins 的可视化 CI / CD 流水线编辑，无需对 Jenkins 进行配置，同时内置丰富的 CI/CD 流水线插件

CI/CD 图形化流水线提供邮件通知功能，新增多个执行条件为流水线、s2i、b2i 提供代码依赖缓存支持端到端的流水线设置：支持从仓库 (Git/ SVN / BitBucket)、代码编译、镜像制作、镜像安全、推送仓库、版本发布、到定时构建的端到端流水线设置

安全管理：支持代码静态分析扫描以对 DevOps 工程中代码质量进行安全管理
日志：日志完整记录 CI / CD 流水线运行全过程
args: "--insecure-registry harbor.devops.kubesphere.local"