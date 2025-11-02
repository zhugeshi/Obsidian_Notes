# 环境搭建
为了方便ida python脚本等的学习,先在vscode中搭建一下ida python环境方便我们编写程序.

- 首先在在vscode中安装插件IDACode

![image.png](https://cloud-map-bed-1351541725.cos.ap-nanjing.myqcloud.com/pic/20251031134716.png)

- 然后下载ida插件 [https://github.com/ioncodes/idacode](https://github.com/ioncodes/idacode) 根据官方文档中的教程把相关的内容拖到ida的plugin目录中即可

这里可能需要修改一下一部分的内容,在idacode_utils中找到setting.json文件,修改其中的 PYTHON = "你的idapython程序路径"
比如我的就是`PYTHON = "D:/IDA_Pro_v8.3_Portable/python311/python.exe"`

- 配置vscode

在setting.json中配置一下ida python sdk的路径用于导入模块.

```json
    "python.autoComplete.extraPaths": [
        "D:\\IDA Professional 9.0\\python"
    ],
    "python.analysis.extraPaths": [
        "D:\\IDA Professional 9.0\\python"
    ],
    "IDACode.saveOnExecute": false,
    "IDACode.executeOnSave": false,
```
# 测试使用
先在ida中打开IDACode插件,再在vscodd中打开 Ctrl-shift p 打开控制面板选择 `connect to ida` ,如果ida中没有报错说明就没有问题了,接下来我们就可以尝试编写一些脚本测试了.

```python
print("Hello IDA")
```

然后呼出命令面板选择execute in ida

```shell
[IDACode] Executing d:\My Code\idapython\test.py
Hello IDA
```
# IDA API学习
参考连接: [Getting Started | Hex-Rays Docs](https://docs.hex-rays.com/developer-guide/idapython/idapython-getting-started?_gl=1*1cji2b*_ga*Njg1MjI3MzM1LjE3NjE4ODE0NDI.*_ga_Y2G1VBHRDB*czE3NjE4ODE0NDIkbzEkZzEkdDE3NjE4ODI1MjAkajU3JGwwJGgw)
## 地址和名称
```python
import idc, idautils, ida_name

# 获取当前光标位置处的地址
ea = idc.here()
print(f"current addr is: {ea:x}")
print("current addr is: " + hex(idc.get_screen_ea()))

# 设置当前光标的地址 
jump_addr = 0x40136d
idc.jumpto(jump_addr)

# 获取所有的指令地址并附加上所有的名称信息
with open("D:\\My Code\\out.txt", "wt") as file:
    for ea in idautils.Heads():
        name = ida_name.get_name(ea)

        try:
            file.write(hex(ea) + f" Option name is: {name}\n")
        except Exception as e:
            print(f"Error is: {e}")

# 获取和给定名称关联的地址
print(hex(ida_name.get_name_ea(0, "printf")))
```
## 读取和写入数据
```python

```
