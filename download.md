# 赛题归档说明

本仓库是 **XAIU-NCTF 2026**（2026 年迎新赛）的赛后归档。

## 未包含的内容

| 项目 | 说明 |
|---|---|
| 逆向工具包 `re.tools.7z` | 体积超出 GitHub 单文件限制，**不随仓库发布** |


## 关于 `re.tools.7z`

原题「初识逆向」附带的工具包

如需复现该题，可自行准备：

```
gdb + pwndbg / peda
python3 + pwntools
ROPgadget / one_gadget / checksec
```

## 目录结构

```
challenges/
├── 由此开始/               5 题
├── Crypto 密码学/         11 题
├── Misc 取证与安全杂项/    16 题
├── PWN 二进制漏洞利用/     10 题
├── Reverse 逆向工程/      11 题
└── Web 安全与渗透测试/      9 题

<分类>/<题目名>/
├── config.toml      题目配置
├── env.toml         容器环境
├── hints.toml       提示
├── README.md        题面
├── WRITEUP.md       官方题解
├── static/          选手附件
├── mapped/          按队分发的附件
└── src/             源码
```

## 平台信息

原平台使用 [Ret2Shell](https://github.com/ret2shell/ret2shell) 搭建。

## 许可

赛题内容（题面、附件）采用 **CC BY-NC-SA 4.0**，脚本与代码采用 **MIT**。引用请注明出处。

