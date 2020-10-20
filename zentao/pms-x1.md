## 关于 zentao 禅道

[zentao-pms 禅道](https://www.zentao.net) 是一个 php 语言开发的项目管理系统:

## 系统分析

此文档主要介绍如何在 InnerStack 上构建并部署 zentao-pms 12.4 服务(单机版)，先分析下 zentao-pms 部署特征:


### 数据库依赖 : AppSpec 引用

zentao-pms 依赖 MySQL, PHP, Nginx.


InnerStack 支持 AppSpec 依赖引用，所以这里直接引用 sysinner-mysql-x1, sysinner-php74, sysinner-nginx 这几个 AppSpec 来实现对运行环境的支持. 


<div class="alert alert-warning">
注:

* AppSpec/sysinner-mysql-x1 构建参考 <a href="/gdoc/view/app-guide/mysql/v57-x1.md" target="_blank">Build MySQL</a>
* AppSpec/sysinner-php74 构建参考 <a href="/gdoc/view/app-guide/php/v74.md" target="_blank">Build PHP</a>
* AppSpec/sysinner-nginx 构建参考 <a href="/gdoc/view/app-guide/nginx/v1.md" target="_blank">Build Nginx</a>
</div>

### 构建 zentao-pms 原始 package 包

1. 主程序文件准备

zentao-pms 以 PHP 语言开发，这里我们从官网下载源码打包:

2. 配置模板文件

zentao-pms 依赖数据库连接等配置信息，首此安装时其内置的安装向导(WebUI)需要录入和确认配置信息，我们将涉及的 php 源码文件复制出来，加入与 InnerStack + MySQL 自动配置的有关模板字段，并保存为模板，后面在 App/Pod 部署是调用 inagent 命令自动配置依赖参数，以上这些可以通过 [https://github.com/inpack/zentao-pms](https://github.com/inpack/zentao-pms) 取得。下面开始打包:


<div class="alert alert-warning">
注: 开始构建前，请确保你已经安装了 inpack 工具, 参考文档 <a href="/gdoc/view/inpack/cli/index.md" target="_blank">inpack cli</a>
</div>


``` shell
git clone https://github.com/inpack/zentao-pms.git
cd zentao-pms

inpack build --dist linux --arch src --version 12.4.2

Building
  package: zentao-pms
  version: 12.4.2
  release: 1
  dist:    linux
  arch:    src
  vendor:  zentao-pms.io
  OK
    zentao-pms-12.4.2-1.linux.src.txz
```

如果一切顺利，上传新构建的 inpack 原始包文件

``` shell
inpack push --repo <your-alias-name> --pack_path zentao-pms-12.4.2-1.linux.src.txz 

PUSH /opt/src/inpack/zentao-pms/zentao-pms-12.4.2-1.linux.src.txz
  ok zentao-pms 100%
```

### 构建 zentao-pms AppSpec 文件

AppSpec 通过登录 InnerStack inPanel 可视化 WebUI 管理 (https://ip-address:9530/in):

进入方式: Applications / AppSpec Center / New AppSpec / Advanced editing mode


将上面导出的 https://github.com/inpack/php.git 项目根目录 app-spec-x1.toml 文件内容复制到提交框并提交, 如下图:


![app-new](zentao-pms/assets/app-spec-edit-a.cmp.png)



如上步骤提交 AppSpec 后，就可以开始部署了.


## 部署

### Step 1: New Instance

在 AppSpec List 找到 zentao-pms-x1 , 点击 "New Instance", 为应用设置一个名字 

![app-new](zentao-pms/assets/app-new-name.cmp.png)


### Step 2: 设置 Pod 容器

如下图, 系统会提示两个选项:

1. 新建 Pod: 将这个 App 绑定到一个全新的 Pod 中运行
2. 绑定到已有的 Pod: 一般用于 DevOps 开发，测试，生产环境推荐一个 App 对应一个 Pod.


![app-new](zentao-pms/assets/app-new-pod-select.cmp.png)

> 注: 不符合硬件规格的 Pod 将不会出现在这个列表里面


这里选择新建 Pod Instance, 提示选择 Pod 规格:

![app-new](zentao-pms/assets/app-new-pod-spec.cmp.png)

确认 Pod 信息后继续下一步.


### Step 3: 配置 MySQL

因为引入依赖 AppSpec/sysinner-mysql-x1 ，这个时候进入 MySQL 配置确认页, 完成对 db_name, db_user, db_auth 等信息确认, 系统后台自动处理并同步配置信息到 Pod/App 运行实例中, 用户可不关注具体细节。点击 "Next" 继续下面操作.

![app-new](mysql/assets/app-new-cfg.cmp.png)


### Step 4: 配置 PHP

因为引入依赖 AppSpec/sysinner-php74 ，这个时候进入 PHP 配置确认页, 完成对 php modules 模块的配置和确认。点击 "Next" 继续下面操作.

![app-new](php/assets/app-new-cfg.cmp.png)


### Step 5: 创建完成

成功后，会自动跳转到 Pod 详情，可以在这个页面里观察 AppSpec 在这个 Pod 里面的执行情况, 请等待一段时间，没有异常的话，Pod 详情页会出现更多有关这个 AppSpec 的信息，如图:

![app-new](zentao-pms/assets/pod-entry.cmp.png)


## zentao-pms 后续操作

以上对于 InnerStack 的操作已经完成，对于 zentao-pms 本身而言，它以 web 方式提供用户UI，有自己独立的用户注册管理系统。

### zentao-pms web access

zentao-pms 在 Pod 里面提供 web 服务的端口是 8081, 通过部署以后，对外的端口映射为 http://49.233.36.31:19449 (在 Pod 详情里面查看), 进入这个地址:

### zentao-pms 1: 应用成功部署后的首页

![app-new](zentao-pms/assets/app-well.cmp.png)

### zentao-pms 2: 确认运行环境

![app-new](zentao-pms/assets/app-setup-env.cmp.png)


### zentao-pms 3: 确认配置

![app-new](zentao-pms/assets/app-setup-config.cmp.png)


### zentao-pms 4: 确认配置 2

![app-new](zentao-pms/assets/app-setup-config2.cmp.png)


### zentao-pms 5: 配置基础信息

![app-new](zentao-pms/assets/app-setup-name.cmp.png)


### zentao-pms 6: 登录新系统

![app-new](zentao-pms/assets/app-login.cmp.png)


### zentao-pms 7: 进入系统首页

![app-new](zentao-pms/assets/app-homepage.cmp.png)


> 注: 更多的操作信息请参考 [https://www.zentao.net](https://www.zentao.net)


## 申明

* InnerStack/AppCenter 旨在助力企业构建自主 PaaS 平台，InnerStack 自身不提供任何云服务
* AppCenter 涉及的方案、文档、代码、数据旨在为企业构建业务系统过程中提供指导参考，这些 AppSpec 组件可能没有经过完整的功能配置、测试和安全审计, 请不要直接用于生产系统。
* AppCenter 包含的第三方项目源码统一开放在 [https://github.com/inpack](https://github.com/inpack)


