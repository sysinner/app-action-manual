## 关于 tensorflow

* [tensorflow](https://tensorflow.org/) 是 google 发布的一个机器学习框架
* [jupyter notebook](https://jupyter.org/) 是一个支持交互科学计算的在线开发编辑器


## 构建 AppSpec

此文档主要介绍如何在 InnerStack 上构建并部署 tensorflow + jupyter 服务(CPU单机版)，这里直接引用 sysinner-tf2-jupyter 中的 AppSpec 模板来快速部署: 


``` shell
git clone https://github.com/inpack/tf.git
```

上面代码拉取成功后，在根目录中找到 app-spec-tf-2-jupyter.toml 这个模板文件。登录 InnerStack inPanel 可视化 WebUI 管理 (https://ip-address:9530/in):

进入方式: Applications / AppSpec Center / New AppSpec


一个完整的定义实例如下图:
![app-new](tf/assets/app-spec-edit-v.cmp.png)


更简单的方式: 在上面导出的 app-spec-tf-2-jupyter.toml 文件原始文本通过 WebUI/AppSpec 编辑界面的 "Advanced editing mode" 模式下直接提交该文件定义, 如下图:

![app-new](tf/assets/app-spec-edit-a.cmp.png)


jupyter notebook 基于 web 提供 UI，这里需要加入一个密钥配置项，可在 inPanel/Applications/AppSpec 列表的 Config 列点击进入,如下:


![app-new](tf/assets/app-spec-edit-cfg.cmp.png)


如上步骤提交 AppSpec 后，就可以开始部署了.


## 部署

### Step 1: New Instance

在 AppSpec List 找到 sysinner-tf2-jupyter , 点击 "New Instance", 为应用设置一个名字 

![app-new](tf/assets/app-new-n1.cmp.png)


### Step 2: 设置 Pod 容器

如下图, 系统会提示两个选项:

1. 新建 Pod: 将这个 App 绑定到一个全新的 Pod 中运行
2. 绑定到已有的 Pod: 一般用于 DevOps 开发，测试，生产环境推荐一个 App 对应一个 Pod.


![app-new](tf/assets/app-new-n2.cmp.png)

> 注: 不符合硬件规格的 Pod 将不会出现在这个列表里面


这里选择新建 Pod Instance, 提示选择 Pod 规格:

![app-new](tf/assets/app-new-n2.2.cmp.png)

确认 Pod 信息后继续下一步.


### Step 3: 配置信息确认


![app-new](tf/assets/app-new-n3.cmp.png)

配置向导会自动产生一个随机密码用于 jupyter web 访问认真，这里直接点击进入下一步。

### Step 4: 创建完成

成功后，会自动跳转到 Pod 详情，可以在这个页面里观察 AppSpec 在这个 Pod 里面的执行情况, 请等待一段时间，没有异常的话，Pod 详情页会出现更多有关这个 AppSpec 的信息，如图:

![app-new](tf/assets/app-new-n4.cmp.png)


## 后续操作

以上对于 InnerStack 的操作已经完成，对于 tensorflow/jupyter 本身而言，它以 web 方式提供用户UI, 在 Pod 里面提供 web 服务的端口是 8888, 通过部署以后，对外的端口映射为 http://81.70.44.120:15009 (在 Pod 详情里面查看), 进入这个地址:

### tensorflow/jupyter 1: 应用成功部署后的首页

![app-new](tf/assets/app-n1.cmp.png)

### tensorflow/jupyter 2: Hello World

![app-new](tf/assets/app-n2.cmp.png)


> 注: 更多的操作信息请参考 [https://jupyter.org](https://jupyter.org)


## 申明

* InnerStack/AppCenter 旨在助力企业构建自主 PaaS 平台，InnerStack 自身不提供任何云服务
* AppCenter 涉及的方案、文档、代码、数据旨在为企业构建业务系统过程中提供指导参考，这些 AppSpec 组件可能没有经过完整的功能配置、测试和安全审计, 请不要直接用于生产系统。
* AppCenter 包含的第三方项目源码统一开放在 [https://github.com/inpack](https://github.com/inpack)

