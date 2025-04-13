---
title: TexLive 安装步骤
date: 2025-03-16 15:32:10 +0800
categories: [Programming, Development-Logs]
tags: [LaTeX]
description: 安装 LaTeX 发行版 TeX Live 的简要步骤
---

### 1. Ubuntu 下 TeX Live 的安装

为避免在线安装时漫长的下载过程，使用离线方式安装 TeX Live：

+ 从[镜像站](https://www.tug.org/texlive/acquire-iso.html)下载最新的 `texlive.iso` 文件至 Ubuntu 系统；

+ 在下载路径内运行终端，挂载安装文件：

    ```shell
    sudo mount -t iso9660 -o loop <Location of texlive.iso> <Mount Location>
    ```

    其中，`<Location of texlive.iso>` 为 `texlive.iso` 文件的下载位置；`<Mount Location>` 为挂载安装文件的位置（需确保该位置存在），例如：

    ```shell
    sudo mount -t iso9660 -o loop ~/Downloads/texlive2024-20240312.iso ~/Temp/TexLive
    ```

+ 进入挂载位置，运行安装脚本：

    ```shell
    cd <Mount Location>
    sudo ./install-tl -no-gui
    ```

+ 根据提示进行安装选项的设置：

    ```shell
    ======================> TeX Live installation procedure <=====================
    
    ======>   Letters/digits in <angle brackets> indicate   <=======
    ======>   menu items for actions or customizations      <=======
    = help>   https://tug.org/texlive/doc/install-tl.html   <=======
    
     Detected platform: GNU/Linux on x86_64
     
     <B> set binary platforms: 1 out of 15
    
     <S> set installation scheme: scheme-full
    
     <C> set installation collections:
         40 collections out of 41, disk space required: 8315 MB (free: 836873 MB)
    
     <D> set directories:
       TEXDIR (the main TeX directory):
         /usr/local/texlive/2024
       TEXMFLOCAL (directory for site-wide local files):
         /usr/local/texlive/texmf-local
       TEXMFSYSVAR (directory for variable and automatically generated data):
         /usr/local/texlive/2024/texmf-var
       TEXMFSYSCONFIG (directory for local config):
         /usr/local/texlive/2024/texmf-config
       TEXMFVAR (personal directory for variable and automatically generated data):
         ~/.texlive2024/texmf-var
       TEXMFCONFIG (personal directory for local config):
         ~/.texlive2024/texmf-config
       TEXMFHOME (directory for user-specific files):
         ~/texmf
    
     <O> options:
       [ ] use letter size instead of A4 by default
       [X] allow execution of restricted list of programs via \write18
       [X] create all format files
       [X] install macro/font doc tree
       [X] install macro/font source tree
       [ ] create symlinks to standard directories
       [X] after install, set CTAN as source for package updates
    
     <V> set up for portable installation
    
    Actions:
     <I> start installation to hard disk
     <P> save installation profile to 'texlive.profile' and exit
     <Q> quit
    
    Enter command: 
    
    ```

    如指定安装位置为 `/opt/texlive/2024`，依次输入：

    ```shell
    D
    1
    /opt/texlive/2024
    ```

    设置完成后输入：

    ```shell
    R
    I
    ```

    即可开始安装；

+ 安装完成后取消安装文件的挂载：

    ```shell
    sudo umount <Mount Location>
    ```

+ 设置环境变量：

    + 修改 `/home/.bashrc` 文件：

        ```shell
        gedit ~/.bashrc
        ```

    + 在文件末尾添加：

        ```shell
        export PATH="$PATH:<TexLive Install Location>/bin/x86_64-linux"
        ```

        其中 `<TexLive Install Location>` 为实际安装路径，这里是 `/opt/texlive/2024`；之后保存并退出

    + 刷新使配置生效：

        ```shell
        source ~/.bashrc
        ```

### 2. Windows 下 TeX Live 的安装

TexLive 在Ubuntu 和 Windows 下的镜像文件都是同一个，因此在 Windows 中直接双击 `texlive.iso` 挂载即可；之后运行 `install-tl-windows.bat` 即可打开图形化的安装界面

除此之外，为了配合 VS Code 中的 `LaTeX Workshop`  插件，也可以通过 Docker 拉取：

```cmd
docker pull texlive/texlive:latest
```

之后在设置中找到 `Latex-workshop > Docker: Enabled` 选项并勾选；在设置中找到 `Latex-workshop > Docker > Image: Latex` 选项，输入：

```
texlive/texlive
```
