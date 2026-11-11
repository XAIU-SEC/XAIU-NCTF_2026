## 本题可能需要 Linux 环境

## 题目描述

```bash
chmod +x hello_gdb
./hello_gdb
```

```
main() never calls win(). Prove it is still reachable.
ok
```

程序里存在一个 `win()`，它打印 flag。通读 `main` 会发现没有任何一处调用它。
二进制未 strip，符号表里能查到 `win`。

目标：让 `win()` 执行一次。

## 原理

**gdb 的 `call` 在做什么**。停在被调试进程里时，gdb 可以读写它的寄存器和内存。
`call win()` 的实现是：在当前栈顶压一个"假返回地址"（指向 gdb 自己的 trap 指令），
把 `rip` 改成 `win` 的地址，继续执行；`win` 执行到 `ret` 时弹回那个假地址，gdb 收回控制权。
对进程来说，这就是一次普通的函数调用，所以不需要 `main` 里存在调用点。

**为什么必须先 `run`**。没启动进程时没有寄存器、没有栈、没有加载好的 libc，`call` 无从执行。

**为什么符号在就能调**。`call win()` 里的 `win` 由 gdb 通过符号表解析成地址。
`strip` 过的二进制没有符号表，就只能写 `call ((void(*)())0x401196)()` 这种形式。

**ret2win 的栈算术**。函数入口 `push rbp; mov rbp,rsp; sub rsp,N` 之后，局部变量在 `[rbp-N, rbp)`。
`leave` 是 `mov rsp,rbp; pop rbp`，`ret` 从 `[rbp+8]` 取返回地址。
所以从缓冲区首字节到返回地址的距离固定是 `缓冲区大小 + 8`（中间有对齐空洞时要按实际反汇编算）。
本题 `buf` 在 `rbp-0x40`，距离正好 `0x40 + 8 = 72`。

**栈对齐**。ABI 要求进入函数时 `rsp % 16 == 8`。`call` 会压 8 字节返回地址，天然满足；
用 `ret` 跳进函数则不满足，函数里若有 SSE 指令（`movaps`）就会 `SIGSEGV`。
本题的 `win` 带 `__attribute__((force_align_arg_pointer))`，入口自己 `and rsp,-16`，两种进入方式