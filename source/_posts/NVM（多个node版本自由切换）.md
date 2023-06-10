---
title: NVM（多个node版本自由切换）
tages: [nvm]
categories: 工具
description: NVM（多个node版本自由切换）
comments: true
cover:  https://static-youdao-note.oss-cn-shenzhen.aliyuncs.com/images/202305270040290.webp?x-oss-process=style/webp
---

> node 版本管理工具 
>
> 1. 可以下载多个`不同的node`版本
> 2. 通过命令直接切换不同node版本的环境，方便快捷



### 安装

`注意：安装前要把之前的node先卸载`

##### 1. 下载

`github`: https://github.com/coreybutler/nvm-windows/releases

仓库地址：https://github.com/nvm-sh/nvm

![image-20230516001402862](https://static-youdao-note.oss-cn-shenzhen.aliyuncs.com/images/202306101135092.webp)

`gitee`:

```bash
git clone https://gitee.com/codeslive/nvm.git
```

##### 2. 安装

双击安装，直接使用软件`默认路径`

安装完成后：打开` cmd` 输入`nvm -v` 查看是否安装成功

常用命令：

```bash
nvm list available
```

![image-20230516001727419](https://static-youdao-note.oss-cn-shenzhen.aliyuncs.com/images/202306101135792.webp)

这里使用`nvm install xxx`命令安装node的时候，一定要`切换镜像源`，不然真的就是不动的

`nvm install 16` 如果不指定具体的版本号，那么将默认下载的是node 16的`最后一个稳定版本`

阿里云镜像：

```bash
nvm npm_mirror https://npmmirror.com/mirrors/npm/
nvm node_mirror https://npmmirror.com/mirrors/node/
```

腾讯云镜像：

```
nvm npm_mirror http://mirrors.cloud.tencent.com/npm/
nvm node_mirror http://mirrors.cloud.tencent.com/nodejs-release/
```

##### 3. 使用node

![image-20230516001905031](https://static-youdao-note.oss-cn-shenzhen.aliyuncs.com/images/202306101136693.webp)



[参考地址](https://nvm.uihtm.com/#:~:text=%E5%A6%82%E6%9E%9C%E4%B8%8B%E8%BD%BDnode%E8%BF%87%E6%85%A2%E6%88%96%E8%80%85%E5%AE%89%E8%A3%85%E5%A4%B1%E8%B4%A5%EF%BC%8C%E8%AF%B7%E6%9B%B4%E6%8D%A2%E5%9B%BD%E5%86%85%E9%95%9C%E5%83%8F%E6%BA%90%2C%20%E5%9C%A8,nvm%20%E7%9A%84%E5%AE%89%E8%A3%85%E8%B7%AF%E5%BE%84%E4%B8%8B%EF%BC%8C%E6%89%BE%E5%88%B0%20settings.txt%EF%BC%8C%E8%AE%BE%E7%BD%AEnode_mirro%E4%B8%8Enpm_mirror%E4%B8%BA%E5%9B%BD%E5%86%85%E9%95%9C%E5%83%8F%E5%9C%B0%E5%9D%80%E3%80%82)