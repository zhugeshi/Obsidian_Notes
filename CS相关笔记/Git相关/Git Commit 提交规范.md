# 格式
Commit message 包括三个部分：Header，Body 和 Footer。可以用下方的格式表示它的结构。

```shell
<type>(<scope>): <subject>// 空一行<body>// 空一行<footer>
```

> [!NOTE]
> 其中，Header 是必需的，Body 和 Footer 可以省略 (默认忽略)，一般我们在 git commit 提交时指定的 -m 参数，就相当于默认指定 Header。 不管是哪一个部分，任何一行都不得超过 72 个字符（或 100 个字符）。这是为了避免自动换行影响美观。
# Type
以下格式用在git commit的开头,用于表示当前提交的主要功能

yiyi'xia一下yi'xia以下ge'shi格式yong'zai用在g'i't给i他git commit d的d的kai'tou开头,yong'yu用于bi'aao'shi表示dang's'qi'aan当前ti'jiao提交d的zhu'yao主要gong'neng功能

- feat：新功能（feature）
- fix：修补 bug
- docs：文档（documentation）
- style：格式（不影响代码运行的变动）
- refactor：重构（即不是新增功能，也不是修改 bug 的代码变动）
- test：增加测试
- chore：构建过程或辅助工具的变动