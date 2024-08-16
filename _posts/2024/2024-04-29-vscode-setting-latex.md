---
title: VS Code 开发环境配置（三）
date: 2024-04-29 12:14:33 +0800
categories: [Programming, Development-Logs]
tags: [Visual Studio Code]
description: 简单记录一下如何在 VS Code 上配置 LaTeX
---



### LaTeX

如果在 VS Code 上使用 LaTeX，需要安装以下插件：

+ [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop)

同时，考虑到本地安装 [TeX Live](https://www.tug.org/texlive/) 之类的 LaTeX 环境需要的磁盘空间和下载问题，采用 LaTeX Workshop 插件所推荐的配合 Docker 容器使用的方式。因此，还需要安装或拉取：

+ [Docker Desktop](https://www.docker.com/products/docker-desktop/)
+ [TeX Live docker image](https://hub.docker.com/r/texlive/texlive)

#### 1. 安装 Docker 

首先自然是下载 Docker Desktop Installer 了。但是现在这个 Installer 会默认把 Docker Desktop 安装在 C 盘上，为了解决这一问题，参考 Stack Overflow 上的[这篇回答](https://stackoverflow.com/questions/75727062/how-to-install-docker-desktop-on-a-different-drive-location-on-windows)，需要进行以下操作：

+ 在 `Docker Desktop Installer.exe` 所在的文件夹中，以管理员权限打开 Windows 终端，输入：

  ``` shell
  start /w "" ".\Docker Desktop Installer.exe" install -accept-license  --installation-dir=D:\Docker --wsl-default-data-root=D:\Docker\images
  ```

  其中，`D:\Docker` 是 Docker Desktop 的安装位置，`D:\Docker\images` 是容器镜像的存储位置。

#### 2. 拉取镜像

打开命令行窗口，输入：

```shell
docker pull texlive/texlive
```

然后耐心等待即可。

#### 3. 设置 LaTeX Workshop 插件

+ 在 VS Code 中安装 LaTeX Workshop 插件；

+ 在设置中找到 `Latex-workshop > Docker: Enabled` 选项并勾选；

+ 在设置中找到 `Latex-workshop > Docker > Image: Latex` 选项，输入：

  ```text
  texlive/texlive
  ```


需要说明的是，由于这两项配置是针对所有使用 VS Code 编辑的 LaTeX 项目，即不需要在每个 LaTeX 项目文件夹中生成 `.vscode` 文件夹进行配置，因此无需通过 Workspace 进行工作区设置，而是直接使用 User 进行用户设置即可。



