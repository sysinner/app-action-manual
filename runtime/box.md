## 容器基础运行环境

InnerStack 当前默认支持 g3, g2 两种容器规格:

* g3 (General v3) : 基于 CentOS-8 x86_64 系统，与 RHEL-8 兼容 
* g2 (General v2) : 基于 CentOS-7 x86_64 系统，与 RHEL-7 兼容 

注: 企业可以部署私有 docker-registry 服务，并将服务地址在 inPanel 后台 Zone 管理中加入，可针对业务特点定制容器镜像。

当前 g3, g2 内置支持的运行环境和版本清单如下:


## g3 运行环境 (推荐)


| 名称 | 版本 |
|----|----|
| gcc | 8.3.1 |
| clang, llvm | 9.0.1 |
| cmake | 3.11.4 |
| perl | 5.26.3 |
| python | 3.6.8 |
| ruby | 2.5.5 |
| nodejs | 10.21.0 |
| git | 2.18 |



## g2 运行环境


| 名称 | 版本 |
|----|----|
| gcc | 4.8.5 |
| clang, llvm | 3.4.2 |
| perl | 5.16.3 |
| python | 2.7.5 |
| git | 2.24 |



## 基于 AppSpec 的运行环境支持

* Java [文档详情](java/jre-v18.md)
* Go [文档详情](go/v1.md)
* PHP [文档详情](php/v74.md)

