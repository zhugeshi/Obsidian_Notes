# 在Windows中构建Makefile
大致流程:

```shell
mkdir build
cd build

cmake -G "MinGW Makefiles" ..
mingw32-make
```

二编: 现在似乎不是这样了

```shell

```

在CMakeLists.txt中添加以下指令,会在编译的目录中自动生成clangd所需要的compile_commands.json.用于clangd读取完成自动补全和代码提示.

```cpp
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
```
# 在Windows中构建VS项目
