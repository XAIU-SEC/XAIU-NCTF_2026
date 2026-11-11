Alice 和 Bob 用 Diffie-Hellman 协商了一个共享密钥，把 key 取成 `shared_secret % 256`，

再逐字节异或加密了 flag。运维把调试信息一起打进了输出，私钥也在里面。恢复 flag。