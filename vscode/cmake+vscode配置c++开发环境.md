# mac+vscode+cmake 配置强大的c++开发环境

​		长期做c++服务器开发，以前一直使用Windows，习惯了vs操作，刚刚转到ma开发对于xcode用的各种不爽，打算配置vscode作为开发环境，折腾了一阵，记录一下.

环境配置过过程

## 安装c/c++插件

![image-20190726225005136](/Users/hushanjun/Library/Application Support/typora-user-images/image-20190726225005136.png)

## 配置环境

一. lunch.json 调试环境

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(lldb) Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/build/BirkServer.out",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": true,
            "MIMode": "lldb",
            "preLaunchTask": "Compile"　
        }
    ]
}
```

二. c_cpp_properties.json c++ 智能提示，搜索路径

```
{
    "configurations": [
        {
            "name": "Mac",
            "includePath": [
                "${workspaceFolder}/**",
                "/usr/local/include/**"
            ],
            "defines": [],
            "macFrameworkPath": [
                "/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.14.sdk/System/Library/Frameworks"
            ],
            "compilerPath": "/usr/bin/clang",
            "cStandard": "c11",
            "cppStandard": "c++11",
            "intelliSenseMode": "clang-x64"
        }
    ],
    "version": 4
}
```

三.  tasks.json 配置编译任务命令

```
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "label": "Compile",
            "type": "shell",
            "command": "cd ./build && cmake .. && make",
            "args": [
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            },
        }
    ]
}
```













 