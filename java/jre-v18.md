## Java JRE 运行环境 (AppSpec)

[Java JRE](https://oracle.com/) 是 SUN/Oracle 提供的 Java 运行环境.


## 构建


构建 java-jre AppSpec 非常简单，下载 jre-version-linux-x64.tar.gz 包，并在 AppSpec/Script Executors 配置必要的环境变量即可。

### 开始构建

<div class="alert alert-warning">
注: 开始构建前，请确保你已经安装了 inpack 工具, 参考文档 <a href="/gdoc/view/inpack/cli/index.md" target="_blank">inpack cli</a>
</div>

从 [http://www.oracle.com/technetwork/java/javase/downloads/index.html](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 下载 jre-version-linux-x64.tar.gz 包备用。


参考代码项目 [https://github.com/inpack/java](https://github.com/inpack/java) 开始打包 Java JRE


``` shell
git clone https://github.com/inpack/java.git
cd java
```

将上面的 jre tar.gz 包移动到 deps 目录，继续:

``` shell
inpack build --spec inpack-jre-v18.toml --dist linux

Building
  package: java-jre
  version: 1.8.0.151
  release: 1
  dist:    linux
  arch:    x64
  vendor:  oracle.com
  OK
    java-jre-1.8.0.151-1.linux.x64.txz
```

如果一切顺利，上传新构建的 inpack 原始包文件

``` shell
inpack push --repo <your-alias-name> --pack_path java-jre-1.8.0.151-1.linux.x64.txz 

PUSH /opt/src/inpack/java/java-jre-1.8.0.151-1.linux.x64.txz
  ok java-jre 100%
```

### 构建 java AppSpec 文件

AppSpec 通过登录 InnerStack inPanel 可视化 WebUI 管理 (https://ip-address:9530/in):

进入方式: Applications / AppSpec Center / New AppSpec / Advanced editing mode


将上面导出的 https://github.com/inpack/java.git 项目根目录 app-spec-jre-v18.toml 文件内容复制到提交框并提交, 如下图:

![app-new](java/assets/app-spec-edit-a.cmp.png)


> 注: 运行环境配置逻辑在 AppSpec/Script Executors 实现, 可参考实例文件细节


如上步骤提交 AppSpec 后，就可以开始部署了.


## 部署

### Step 1: New Instance

在 AppSpec List 找到 sysinner-java-jre18 , 点击 "New Instance"

![app-new](java/assets/app-new-name.cmp.png)

### Step 2: 设置 Pod 容器

如下图, 系统会提示两个选项:

1. 新建 Pod: 将这个 App 绑定到一个全新的 Pod 中运行
2. 绑定到已有的 Pod: 一般用于 DevOps 开发，测试，生产环境推荐一个 App 对应一个 Pod.


![app-new](java/assets/app-new-pod-select.cmp.png)


这里选择新建 Pod Instance, 提示选择 Pod 规格:

![app-new](java/assets/app-new-pod-spec.cmp.png)

确认 Pod 信息后继续下一步.


### Step 3: 创建完成

成功后，会自动跳转到 Pod 详情，可以在这个页面里观察 AppSpec 在这个 Pod 里面的执行情况, 请等待一段时间，没有异常的话，Pod 详情页会出现更多有关这个 AppSpec 的信息，如图:

![app-new](java/assets/pod-entry.cmp.png)


## Java JRE 后续操作

以上对于 InnerStack 的操作已经完成，确认部署的有效性可以参考 [远程访问Pod](https://www.sysinner.cn/gdoc/view/si/pod/ssh.md) 登录 Pod 查看状态:

``` shell
[action@sysinner ~]$ java -version
java version "1.8.0_151"
Java(TM) SE Runtime Environment (build 1.8.0_151-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.151-b12, mixed mode)
```

> 注: 更多的操作信息请参考 [https://www.oracle.com/](https://www.oracle.com/)


## 申明

* InnerStack/AppCenter 旨在助力企业构建自主 PaaS 平台，InnerStack 自身不提供任何云服务
* AppCenter 涉及的方案、文档、代码、数据旨在为企业构建业务系统过程中提供指导参考，这些 AppSpec 组件可能没有经过完整的功能配置、测试和安全审计, 请不要直接用于生产系统。
* AppCenter 包含的第三方项目源码统一开放在 [https://github.com/inpack](https://github.com/inpack)


