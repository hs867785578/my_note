1.  ros中缺少包，用apt安装时，提示找不到，这时候需要加上版本，比如
`apt-get install -q -y ros-noetic-cv-bridge`

2.  ros配置vscode环境
```bash
catkin_make -DCMAKE_EXPORT_COMPILE_COMMANDS=1
```

 c_cpp_properties.json
```bash
"compileCommands": "${workspaceFolder}/build/compile_commands.json"
```
