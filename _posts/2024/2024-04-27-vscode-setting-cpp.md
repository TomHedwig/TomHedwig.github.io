---
title: VS Code 开发环境配置（一）
date: 2024-04-27 09:54:04 +0800
categories: [Programming, Development-Logs]
tags: [Visual Studio Code]
description: 简单记录一下如何在 VS Code 上配置 C/C++
---



### 写在前面

近来笔者更换了笔记本电脑，对着新电脑欣喜之余，突然发觉旧电脑上的一些开发环境（以 [VS Code](https://code.visualstudio.com/) 为主）都需要重新配置，因此免不了一番折腾，尤其是在 [CSDN](https://www.csdn.net/) 上屎里淘金的过程更是让人拍案叫绝。为了避免以后再被中文互联网上的垃圾恶心到，故综合各种教程帖子资料，撰此文以详细记录一下 Visual Studio Code 开发环境配置（Windows 10，x86_64 平台）的过程。

### Visual Studio Code 的安装

前往 [VS Code 官网](https://code.visualstudio.com/download#) 下载安装即可。不过值得一提的是，笔者这次下载的是能以管理员权限安装的 System Installer 版本，以免安装后每次启动都要右键选择 “以管理员身份运行”。

### C/C++

#### 1. 安装 c/c++ 编译器

+ 下载并安装 [MSYS2](https://www.msys2.org/)

+ 在安装完成后的 MSYS2 命令行窗口界面中更新组件包：

  ```shell
  pacman -Syu
  pacman -Su
  ```

+ 安装 gcc：

  ```shell
  pacman -S mingw-w64-x86_64-gcc
  ```

+ 安装 gdb：

  ```shell
  pacman -S mingw-w64-x86_64-gdb
  ```

+ 在 MSYS2 安装文件夹中找到 `MSYS2 Install Folder/mingw64/bin` 文件夹，将其添加到系统环境变量；

+ 打开系统命令行窗口，验证安装：

  ```shell
  gcc --version
  g++ --version
  gdb --version
  ```

  如果一切顺利，将分别出现以下内容：

  > gcc (Rev1, Built by MSYS2 project) 14.2.0
  
  > g++ (Rev1, Built by MSYS2 project) 14.2.0
  
  > GNU gdb (GDB) 15.1

#### 2. 配置 VS Code

+ 在想要保存代码的文件夹（不应含有中文字符）中打开命令行窗口，输入：

  ```shell
  code .
  ```

+ 在 VS Code 中搜索并安装以下扩展插件：

  + [C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
  + [Code Runner](https://marketplace.visualstudio.com/items?itemName=formulahendry.code-runner)

+ 在工作目录中新建 `.vscode` 文件夹，并在其中新建 `c_cpp_properties.json`、`launch.json`、`settings.json` 和 `tasks.json` 四个文件：

  + `c_cpp_properties.json`：

    ```json
    {
        "configurations": [
          {
            "name": "Win64",
            "includePath": ["${workspaceFolder}/**"],
            "defines": ["_DEBUG", "UNICODE", "_UNICODE"],
            "windowsSdkVersion": "10.0.18362.0",
            "compilerPath": "g++.exe location, divided by '/', end with executable file",
            "cStandard": "c17",
            "cppStandard": "c++17",
            "intelliSenseMode": "gcc-x64"
          }
        ],
        "version": 4
      }
    ```

    其中， `complierPath` 的值为先前安装的 g++ 可执行文件路径：`MSYS2 Install Folder/msys64/mingw64/bin/g++.exe`

  + `launch.json`：

    ```json
    {
        "version": "0.2.0",
        "configurations": [
            {
                "name": "(gdb) Launch",
                "type": "cppdbg",
                "request": "launch",
                "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",
                "args": [],
                "stopAtEntry": false,
                "cwd": "${workspaceRoot}",
                "environment": [],
                "externalConsole": true,
                "MIMode": "gdb",
                "miDebuggerPath": "gdb.exe location, divided by '\\', end with executable file",
                "preLaunchTask": "g++",
                "setupCommands": [
                    {
                        "description": "Enable pretty-printing for gdb",
                        "text": "-enable-pretty-printing",
                        "ignoreFailures": true
                    }
                ]
            },
            {
                "name": "C/C++: g++.exe build and debug active file",
                "type": "cppdbg",
                "request": "launch",
                "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",
                "args": [],
                "stopAtEntry": false,
                "cwd": "path of bin folder in mingw, divided by '/', end without executable file",
                "environment": [],
                "externalConsole": false,
                "MIMode": "gdb",
                "miDebuggerPath": "gdb.exe location, divided by '\\', end with executable file",
                "setupCommands": [
                    {
                        "description": "Enable pretty-printing for gdb",
                        "text": "-enable-pretty-printing",
                        "ignoreFailures": true
                    },
                    {
                        "description": "Set Disassembly Flavor to Intel",
                        "text": "-gdb-set disassembly-flavor intel",
                        "ignoreFailures": true
                    }
                ],
                "preLaunchTask": "C/C++: g++.exe build active file"
            }
        ]
    }
    ```

    其中，有几处值需要根据 c/c++ 编译器的实际安装情况做出调整：

    （1）`miDebuggerPath`：为  c/c++ 编译器中 gdb.exe 的位置：`MSYS2 Install Folder\\mingw64\\bin\\gdb.exe`

    （2）`cwd`：为 c/c++ 编译器中 bin 文件夹的路径：`MSYS2 Install Folder/mingw64/bin`

  + `settings.json`：

    ```json
    {
        "files.associations": {
          "*.py": "python",
          "iostream": "cpp",
          "*.tcc": "cpp",
          "string": "cpp",
          "unordered_map": "cpp",
          "vector": "cpp",
          "ostream": "cpp",
          "new": "cpp",
          "typeinfo": "cpp",
          "deque": "cpp",
          "initializer_list": "cpp",
          "iosfwd": "cpp",
          "fstream": "cpp",
          "sstream": "cpp",
          "map": "c",
          "stdio.h": "c",
          "algorithm": "cpp",
          "atomic": "cpp",
          "bit": "cpp",
          "cctype": "cpp",
          "clocale": "cpp",
          "cmath": "cpp",
          "compare": "cpp",
          "concepts": "cpp",
          "cstddef": "cpp",
          "cstdint": "cpp",
          "cstdio": "cpp",
          "cstdlib": "cpp",
          "cstring": "cpp",
          "ctime": "cpp",
          "cwchar": "cpp",
          "exception": "cpp",
          "ios": "cpp",
          "istream": "cpp",
          "iterator": "cpp",
          "limits": "cpp",
          "memory": "cpp",
          "random": "cpp",
          "set": "cpp",
          "stack": "cpp",
          "stdexcept": "cpp",
          "streambuf": "cpp",
          "system_error": "cpp",
          "tuple": "cpp",
          "type_traits": "cpp",
          "utility": "cpp",
          "xfacet": "cpp",
          "xiosbase": "cpp",
          "xlocale": "cpp",
          "xlocinfo": "cpp",
          "xlocnum": "cpp",
          "xmemory": "cpp",
          "xstddef": "cpp",
          "xstring": "cpp",
          "xtr1common": "cpp",
          "xtree": "cpp",
          "xutility": "cpp",
          "stdlib.h": "c",
          "string.h": "c"
        },
        "editor.suggest.snippetsPreventQuickSuggestions": false,
        "aiXcoder.showTrayIcon": true
      }
    ```

  + `tasks.json`：

    ```json
    {
        "version": "2.0.0",
        "tasks": [
            {
                "label": "g++",
                "command": "g++",
                "args": [
                    "-g",
                    "${file}",
                    "-o",
                    "${fileDirname}/${fileBasenameNoExtension}.exe"
                ],
                "problemMatcher": {
                    "owner": "cpp",
                    "fileLocation": [
                        "relative",
                        "${workspaceRoot}"
                    ],
                    "pattern": {
                        "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
                        "file": 1,
                        "line": 2,
                        "column": 3,
                        "severity": 4,
                        "message": 5
                    }
                },
                "group": "build"
            },
            {
                "type": "cppbuild",
                "label": "C/C++: g++.exe build active file",
                "command": "g++.exe location, divided by '/', end with executable file",
                "args": [
                    "-fdiagnostics-color=always",
                    "-g",
                    "${file}",
                    "-o",
                    "${fileDirname}\\${fileBasenameNoExtension}.exe"
                ],
                "options": {
                    "cwd": "path of bin folder in mingw, divided by '/', end without executable file"
                },
                "problemMatcher": [
                    "$gcc"
                ],
                "group": {
                    "kind": "build",
                    "isDefault": true
                },
                "detail": "Task generated by Debugger."
            }
        ]
    }
    ```

    其中，有几处值需要根据 c/c++ 编译器的实际安装情况做出调整：

    （1）command：为  c/c++ 编译器中 g++.exe 的位置：`MSYS2 Install Folder/mingw64/bin/g++.exe`

    （2）`cwd`：为 c/c++ 编译器中 bin 文件夹的路径：`MSYS2 Install Folder/mingw64/bin`

