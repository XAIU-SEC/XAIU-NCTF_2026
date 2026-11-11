# web-hello-source


### 1. 浏览器拿到的是什么

你在地址栏敲一个地址，服务器回给你的是一份**文本**。浏览器读这份文本，按 HTML 规则把标签翻译成画面。

`<!-- 内容 -->` 是 HTML 注释。渲染时整段跳过，页面上看不到它。但它仍然是文件的一部分，和正文一起从服务器发出来。**能拿到文件本身，就能拿到注释。**

### 2. 三条路

**路一 · `Ctrl+U`**

在页面上按 `Ctrl+U`（macOS 是 `Command+Option+U`），浏览器新开一个标签，显示服务器发来的原始文本。

**路二 · F12 → Elements**

按 `F12` 打开开发者工具，切到 `Elements`（元素）。这里显示的是**解析后的 DOM 树**，不是原始文件：浏览器会补全缺失的闭合标签、规范化属性引号、把「浏览器理解的结构」显示出来；注释以 `<!-- -->` 的形式保留在树里。

注意：

| 面板 | 看到的内容 | 适合找什么 |
|---|---|---|
| View Source（`Ctrl+U`） | 服务器发来的**原文** | 注释、隐藏字段、被 JS 改写前的原始结构 |
| Elements（F12） | 浏览器**解析后**的 DOM | 运行时被 JS 插入/修改的节点、当前生效的样式 |

**路三 · `curl`**

命令行直接取文件，不渲染、不执行 JS、不带 Cookie：

```bash
curl -s http://127.0.0.1:8123/index.html
```

Windows 的 PowerShell 5.1 里 `curl` 是 `Invoke-WebRequest` 的别名，敲 `curl -s URL` 会报「请为以下参数提供值: Uri」。用 `curl.exe`（Windows 10 1803+ 自带）：

```powershell
curl.exe -s http://127.0.0.1:8123/index.html
```

只挑出注释行：

```powershell
curl.exe -s http://127.0.0.1:8123/index.html | Select-String '<!--'
```

```bash
curl -s http://127.0.0.1:8123/index.html | grep -n '<!--'
```

### 4. 

这份页面里留了三行注释，其中一行是 `debug-marker: flag{REDACTED}`。用上面任意一条路把源码取出来，找到那一行。
在附件中有这道题的 flag ，把它找出来吧！