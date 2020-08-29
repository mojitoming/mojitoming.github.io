---
title: Docker Nexus3 Maven 私服搭建及使用
date: 2020-08-29 23:50:44
tags:
  - maven
  - docker
  - nexus
---

> 在 CentOS 上，使用 docker 安装 nexus3，并配置 maven 私服，完成对团队封装的 jar 包的管理。

## 1.  版本

- CentOS Linux release 7.8.2003 (Core)
- Docker version 19.03.9, build 9d988398e7
- Nexus version 3.26.1-02

## 2. Nexus 安装

此处需要注意的地方是，持久化数据所创建的目录的权限问题。如果不给读写权限，启动容器的时候，容器会不断的重启，docker logs nexus3 分析日志后发现是目录权限的问题。

```bash
## CentOS 7 docker nexus3
$ mkdir -p /usr/local/docker/nexus-data && chmod -R 777 /usr/local/docker/nexus-data
$ docker run -id --net="host" --privileged=true --name=nexus3 \
-e NEXUS_CONTEXT=nexus --restart=always -p 8081:8081 \
-v /usr/local/docker/nexus-data:/nexus-data sonatype/nexus3
```

容器启动后可以通过[`http://ip:8081/`](http://ip:8081/)访问 WEB Management UI。此处因为指定了 NEXUS_CONTEXT ，故访问地址为[`http://ip:8081/nexus/`](http://ip:8081/)，指定此参数的原因是为了做 Nginx 代理，如果不指定，会导致资源无法访问。

第一次打开 WEB UI，默认的登录名是 admin，密码根据提示从容器中`/nexus-data`或 宿主`/usr/local/docker/nexus-data`目录下的对应文件获取；然后根据提示设置自己的密码。

<!--more-->

## 3. Maven 私服搭建

1. 创建 Blob Stores

   Nexus 使用 Blob 格式存储 Repository，首先创建自己的 Blob Stores，用来存储自定义的 Repository。

   {% asset_img Untitled.png "This is an image." %}

   {% asset_img Untitled_1.png "This is an image." %}

   {% asset_img Untitled_2.png "This is an image." %}

   选择 File 类型，此处我定义 Blob Store 的名字为 example，路径为容器内的绝对路径，默认为`/nexus-data/blobs/example` ，此处不建议修改，因为映射到宿主机的路径为此路径。

2. 创建 hosted 类型的仓库

   hosted repository 的作用，是让用户可以把自己的组件上传至此，比如自己封装的 jar，供团队使用。一般同名仓库中会有两个 hosted 类型的仓库，一个为 releases 库，另一个为 snapshots 库。分别用来存储版本号为正式版的发布，和版本号带有 SNAPSHOT 的发布。

   1. 创建 release 发布库

      {% asset_img Untitled_3.png "This is an image." %}

      {% asset_img Untitled_4.png "This is an image." %}

      {% asset_img Untitled_5.png "This is an image." %}

      最终创建名称为 example-releases 库，配置如上图所示。需要注意的是不要勾选 Strict Content Type Validation，如果勾选了，会影响拉取，我碰到的 error 是即使正确配置了用户名/密码，在拉取依赖时会一直报 401 unauthorized。

   2. 创建 snapshot 快照库

      {% asset_img Untitled_6.png "This is an image." %}

      最终创建名称为 example-snapshots 库，配置如上图所示。

3. 创建 group 类型的仓库

   仓库组（Repository Group）的作用是将多个仓库（代理仓库和宿主仓库）聚合，对用户暴露统一的地址。当用户需要获取依赖时，请求的是仓库组的地址。

   {% asset_img Untitled_7.png "This is an image." %}

   {% asset_img Untitled_8.png "This is an image." %}

   最终创建名称为 example-public 库，配置如上图所示。

## 4. Maven 全局配置及项目 pom.xml 配置

1. 项目全局配置 setting.xml

   因为我的配置有多个私服，所以通过 profiles 配置，具体配置如下：

   在管理 UI 点击 copy 按钮获取 example-public 的 url，repository 的 id 与repository 的 name 对应。配置 profile 之后记住激活：`<activeProfile>example-public</activeProfile>`

   {% asset_img Untitled_9.png "This is an image." %}

   通过 servers 配置与 repository 对应的用户名/密码。其中 example-releases 和 example-snapshots 是用来 deploy 的，其 id 与 pom.xml 中的 id 相对应，这部分在下一小节会有详细说明。

   {% asset_img Untitled_10.png "This is an image." %}

2. 项目中 pom.xml 配置

   id 必须与 setting.xml 文件中 server 的 id 相同。

   {% asset_img Untitled_11.png "This is an image." %}

至此，可以使用 deploy 将自定义组件发布到 maven 私服，上传后可以在管理 UI 看到自己的组件。

{% asset_img Untitled_12.png "This is an image." %}