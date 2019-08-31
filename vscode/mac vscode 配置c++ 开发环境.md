#  mac vscode 配置c++ 开发环境 



1. 配置编译人物 task

   ```json
   {
       // See https://go.microsoft.com/fwlink/?LinkId=733558
       // for the documentation about the tasks.json format
       "version": "2.0.0",
       "tasks": [
           {
               "label": "Compile",
               "type": "shell",
               "command": "g++",
               "args": [
                   "Main.cpp",
                   "-o",
                   "${workspaceFolder}/a.out",
                   "-g",
                   "-Wall"
               ],
               "group": {
                   "kind": "build",
                   "isDefault": true
               }
           }
       ]
   }
   ```

   参数的简单说明
   “label”：任务的名称
   “type”：任务的类型，有两种（shell/process），shell的意思相当于先打开shell再输入命令，process是直接运行命令
   “command”：实际执行的命令
   “args”：在这里可以设置一些需要的参数，比如说这里我设置的四个参数中， 
   “-o”，表示指定输出文件名，如果不加该参数则默认Windows下输出a.exe，Linux/MAC下默认a.out，紧接着的一行即为指定的输出文件名 

2. 配置调试信息 launch.json

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
               "program": " ${workspaceFolder}/a.out",
               "args": [],
               "stopAtEntry": false,
               "cwd": "${workspaceFolder}",
               "environment": [],
               "externalConsole": true,
               "targetArchitecture": "x86_64",
               "MIMode": "lldb"
           },
       ]
   }
   ```

   参数的简单说明
   “name”：配置名称，将会在启动配置的下拉菜单中显示
   “type”：配置类型
   “request”：请求配置类型，可以为launch（启动）或attach（附加）
   “program”：进行调试的程序的路径
   “stopAtEntry”：若设置为true时程序将暂停在程序入口处
   “cwd”：当前调试所在目录

   “externalConsole”：调试时是否显示控制台窗口，设置为true即是显示控制台

3. 配置提示信息  c_cpp-propertries

   ```json
   {
       "configurations": [
           {
               "name": "Linux",
               "includePath": [
                   "${workspaceFolder}/**"
               ],
               "defines": [],
               "compilerPath": "/usr/bin/gcc",
               "cStandard": "c11",
               "cppStandard": "c++17",
               "intelliSenseMode": "clang-x64"
           }
       ],
       "version": 4
   }
   ```
