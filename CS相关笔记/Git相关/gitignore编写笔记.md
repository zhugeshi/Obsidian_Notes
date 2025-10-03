## 基本语法

在 `.gitignore` 文件中，每一行定义一条忽略规则：
- `*.log` → 忽略所有 `.log` 文件
- `node_modules/` → 忽略整个 `node_modules` 文件夹
- `*.tmp` → 忽略所有 `.tmp` 文件
- `/config.json` → 忽略项目根目录下的 `config.json`（但不会忽略子文件夹里的同名文件）

## 特殊符号规则
- `#` ：注释
- `!` ：取反（即不要忽略某个文件/目录）
```shell
*.log 
!important.log   # 忽略所有 log 文件，但保留 important.log
```
- `/` ：表示项目根目录
- `**/` ：匹配多层目录
```shell
	**/temp/   # 忽略所有目录下的 temp 文件夹
```