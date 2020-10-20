## Visual Studio Code 云端版 (coder/code-server)

[coder/code-server](https://coder.com/) 是 coder.com 基于微软 VS Code 开发的一个运行于服务端并使用浏览器接入的云端IDE。


* 微软 Visual Studio Code 介绍 [vs code](https://code.visualstudio.com/)
* Coder code-server 介绍 [code-server](https://github.com/cdr/code-server)

## 系统分析

此文档主要介绍如何在 InnerStack 上构建并部署 coder-code-server 服务:

### 构建 coder-code-server 原始 package 包

1. 主程序文件准备

coder-code-server 基于 nodejs 运行环境，可以从 [https://github.com/cdr/code-server](https://github.com/cdr/code-server) 下载 code-server-VERSION-linux-amd64.tar.gz 包备用。


2. 配置实现

coder-code-server 依赖配置文件 config.yaml 用于设置 http-server port 和 登录密码等基础信息, 并将配置文件中涉及密码的参数使用 InnerStack 内置的随机密码替换为形如 ```{{.cfg/coder-code-server/config_password}}```, 这些模板变量将和下面的 AppSpec/Script 配合使用，在容器启动阶段完成配置信息的自动同步。


<div class="alert alert-warning">
注: 开始构建前，请确保你已经安装了 inpack 工具, 参考文档 <a href="/gdoc/view/inpack/cli/index.md" target="_blank">inpack cli</a>
</div>


``` shell
git clone https://github.com/inpack/coder-code-server.git
cd coder-code-server
```

将上面的 code-server-VERSION-linux-amd64.tar.gz 包移动到 deps 目录，继续:

``` shell
inpack build --spec inpack.toml --dist linux --version 3.6.1

Building
  package: coder-code-server
  version: 3.6.1
  release: 1
  dist:    linux
  arch:    x64
  vendor:  coder.com
  OK
    coder-code-server-3.6.1-1.linux.x64.txz
```

如果一切顺利，上传新构建的 inpack 原始包文件

``` shell
inpack push --repo <your-alias-name> --pack_path coder-code-server-3.6.1-1.linux.x64.txz 

PUSH /opt/src/inpack/coder-code-server/coder-code-server-3.6.1-1.linux.x64.txz
  ok coder-code-server 100%
```

### 构建 coder-code-server AppSpec 文件

AppSpec 通过登录 InnerStack inPanel 可视化 WebUI 管理 (https://ip-address:9530/in):

进入方式: Applications / AppSpec Center / New AppSpec / Advanced editing mode


将上面导出的 https://github.com/inpack/coder-code-server.git 项目根目录 app-spec-x1.toml 文件内容复制到提交框并提交, 如下图:

![app-new](coder/assets/app-spec-edit-a.cmp.png)


> 注: 运行环境配置逻辑在 AppSpec/Script Executors 实现, 可参考实例文件细节


如上步骤提交 AppSpec 后，就可以开始部署了.



## 部署

### Step 1: New Instance

在 AppSpec List 找到 coder-code-server , 点击 "New Instance"

![app-new](coder/assets/app-new-name.cmp.png)

### Step 2: 设置 Pod 容器

如下图, 系统会提示两个选项:

1. 新建 Pod: 将这个 App 绑定到一个全新的 Pod 中运行
2. 绑定到已有的 Pod: 一般用于 DevOps 开发，测试，生产环境推荐一个 App 对应一个 Pod.


![app-new](coder/assets/app-new-pod-select.cmp.png)


这里选择新建 Pod Instance, 提示选择 Pod 规格:

![app-new](coder/assets/app-new-pod-spec.cmp.png)

确认 Pod 信息后继续下一步.

### Step 4: 配置

![app-new](coder/assets/app-new-cfg.cmp.png)

### Step 3: 创建完成

成功后，会自动跳转到 Pod 详情，可以在这个页面里观察 AppSpec 在这个 Pod 里面的执行情况, 请等待一段时间，没有异常的话，Pod 详情页会出现更多有关这个 AppSpec 的信息，如图:

![app-new](coder/assets/pod-entry.cmp.png)


## coder-code-server 后续操作

以上对于 InnerStack 的操作已经完成，对于 coder-code-server 本身而言，它以 web 方式提供用户UI，如上图黄色框标记所示，分别取得系统分配的 http url 地址和系统自动产生的登录密码，即可进入 code-server.

### 登录

![app-new](coder/assets/app-login.cmp.png)

### 默认首页

![app-new](coder/assets/app-homepage.cmp.png)


### 插件安装

![app-new](coder/assets/app-exts.cmp.png)


### 使用内置 Terminal 终端

![app-new](coder/assets/app-terminal.cmp.png)



> 注: 更多的操作信息请参考 [code.visualstudio.com](https://code.visualstudio.com/)


## 申明

* InnerStack/AppCenter 旨在助力企业构建自主 PaaS 平台，InnerStack 自身不提供任何云服务
* AppCenter 涉及的方案、文档、代码、数据旨在为企业构建业务系统过程中提供指导参考，这些 AppSpec 组件可能没有经过完整的功能配置、测试和安全审计, 请不要直接用于生产系统。
* AppCenter 包含的第三方项目源码统一开放在 [https://github.com/inpack](https://github.com/inpack)

