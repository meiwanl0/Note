下面按“个人成长为二进制安全研究员/研究岗”来整理一套完整学习路线；如果是团队或研究院培养，也可以把各阶段作为课程模块。

> **合规提示**：以下内容只用于学习、CTF、授权测试和漏洞研究；不要对未授权目标进行任何攻击。

---

## 一、整体路线概览

```text
编程与系统基础
  → 计算机体系结构与汇编
  → 操作系统、链接与二进制格式
  → 逆向工程
  → 漏洞原理与利用
  → 内核/移动/IoT/固件等方向深入
  → 漏洞挖掘与模糊测试
  → 实战与研究输出
```

建议时间分配：

- 0–6 个月：基础、C/C++、汇编、操作系统
- 6–12 个月：逆向工具、静态/动态分析、常见漏洞利用
- 12–24 个月：内核、移动、Windows/Linux 漏洞、Fuzzing
- 2 年以上：专精方向、漏洞挖掘、CVE 复现、研究输出

---

## 二、分阶段学习路线与资源

### 阶段 0：基础工具与开发环境

目标：熟练使用 Linux、命令行、Git、Python，能搭建实验环境。

资源：

- 《鸟哥的 Linux 私房菜》
- MIT Missing Semester：`https://missing.csail.mit.edu`
- 《Python 编程：从入门到实践》
- Docker 快速入门，后续漏洞环境会大量使用

实践：

- 安装 Ubuntu / Kali / Arch 任选一个
- 学会使用 SSH、tmux、gcc、make、gdb 基础命令
- 用 Docker 搭建 `pwnable` 环境

---

### 阶段 1：C/C++ 与数据结构基础

二进制安全研究非常依赖 C/C++，尤其是内存布局、指针、结构体、虚函数、回调、类型转换等。

目标：能读懂常见 C/C++ 漏洞代码，能写调试用的小程序。

资源：

- 《C Primer Plus》
- 《C 和指针》
- 《C 陷阱与缺陷》
- 《C++ Primer》
- 《数据结构与算法分析：C 语言描述》

实践：

- 自己实现链表、栈、堆、哈希表
- 用 C 写一个简单的网络 TCP 回显程序
- 用 C++ 写一个带虚函数的类，观察虚表结构

---

### 阶段 2：计算机体系结构与汇编

二进制安全的核心是理解机器如何执行代码。

目标：掌握 x86-64 汇编，理解栈帧、调用约定、寄存器、中断、缓存、内存模型；了解 ARM。

资源：

- **《深入理解计算机系统》**，即 CSAPP，必读
- 《计算机组成与设计：硬件/软件接口》
- 《x86 汇编语言：从实模式到保护模式》
- 《ARM System Developer's Guide》
- Intel 64 and IA-32 Architectures Software Developer Manuals
- ARM Architecture Reference Manual

课程：

- CMU 15-213：CSAPP 配套课程
- MIT 6.004：计算机组成原理
- OpenSecurityTraining2：`Architecture 1001`

实践：

- 用 `gcc -S` 查看 C 代码对应汇编
- 用 GDB 单步跟踪函数调用，观察 `rbp/rsp` 变化
- 写一段汇编实现简单函数调用

---

### 阶段 3：操作系统、链接与二进制格式

目标：理解进程/线程、虚拟内存、系统调用、文件系统、动态链接、ELF/PE 结构。

资源：

- **《操作系统导论》**，即 OSTEP，强烈推荐
- **《程序员的自我修养：链接、装载与库》**，国内二进制基础神书
- 《链接器与加载器》
- 《Linux 内核设计与实现》
- 《Windows Internals》第 7 版

课程：

- MIT 6.S081：xv6 操作系统实验
- 南京大学操作系统课程：蒋炎岩老师
- 哈工大操作系统课程

实践：

- 阅读 ELF 文件头：`readelf -h/-l/-S`
- 阅读 PE 文件头：`dumpbin` / `CFF Explorer`
- 理解 GOT、PLT、重定位
- 用 `strace` 观察程序系统调用

---

### 阶段 4：逆向工程

目标：熟练使用 IDA/Ghidra、GDB/x64dbg/WinDbg，能分析无源码二进制。

资源：

- **《加密与解密》**，看雪经典
- **《IDA Pro 权威指南》**
- **《The Ghidra Book》**
- **《Practical Reverse Engineering》**
- **《Reversing: Secrets of Reverse Engineering》**
- 《C++ 反汇编与逆向分析技术揭秘》
- 《软件调试》

课程：

- OpenSecurityTraining2：`Introduction to Reverse Engineering`
- OpenSecurityTraining2：`Reverse Engineering 1001/1002`
- TCM Security / Pluralsight 逆向课程

工具：

- 静态分析：IDA Pro、Ghidra、Binary Ninja、radare2/Cutter
- 动态分析：GDB + pwndbg/gef、x64dbg、WinDbg、LLDB
- 辅助：`objdump`、`readelf`、`nm`、`strings`、`ltrace`、`strace`

练习：

- Crackmes.one
- reversing.kr
- Flare-On 历年题目
- 看雪 CTF / 52 破解论坛题目

---

### 阶段 5：漏洞原理与利用基础

目标：理解内存破坏漏洞，能编写基础利用；理解缓解机制与绕过思路。

核心内容：

- 栈溢出、堆溢出、整数溢出、格式化字符串、越界读写、UAF、类型混淆、未初始化变量
- 缓解机制：ASLR、NX/DEP、Canary、PIE、RELRO、CFG/CFI
- 利用技术：shellcode、ret2libc、ROP、JOP、栈迁移、GOT 劫持、堆风水

资源：

- **《0day 安全：软件漏洞分析技术》**
- **《漏洞战争：软件漏洞分析精要》**
- **《The Shellcoder's Handbook》**
- **《Hacking: The Art of Exploitation》**
- CTF Wiki：`https://ctf-wiki.org`

在线练习：

- pwn.college：`https://pwn.college`
- ROP Emporium：`https://ropemporium.com`
- Exploit Education：Protostar、Phoenix
- pwnable.kr / pwnable.tw
- OverTheWire Narnia
- HackTheBox Pwn 模块

工具：

- pwntools
- ROPgadget、Ropper、one_gadget
- checksec、pwninit

建议：每学一个漏洞类型，写一个最小 PoC，并画栈图/堆图，做到真正理解原因。

---

### 阶段 6：内核与驱动安全

目标：掌握 Linux/Windows 内核漏洞原理与利用，理解内核缓解机制。

Linux 方向：

- 《Linux 内核设计与实现》
- 《深入理解 Linux 内核》
- 《Linux 内核安全与漏洞利用》
- Linux Kernel CTF、HackSys Extreme Vulnerable Driver

Windows 方向：

- 《Windows Internals》
- 《Windows 内核安全与驱动开发》
- 《内核漏洞的利用与防范》
- WinDbg + VirtualKD 调试内核

课程：

- Linux 内核分析与编程课程
- Syzkaller 内核 fuzzing 官方文档

练习：

- 编译调试 Linux 内核，加载自定义内核模块
- 分析 HackSys Extreme Vulnerable Driver
- 复现公开 Linux 内核 CVE

---

### 阶段 7：移动 / IoT / 固件安全

目标：将二进制安全能力迁移到 ARM、Android、iOS、嵌入式设备。

资源：

- **《Android 软件安全与逆向分析》**
- **《Android 安全攻防权威指南》**
- **《iOS 应用逆向工程》**
- OWASP Mobile Security Testing Guide
- ARM 汇编与 TrustZone 文档

工具：

- Frida、jadx、apktool、Hopper、IDA、Ghidra
- QEMU 用户态/系统态仿真
- binwalk、firmware-mod-kit、JTAG/UART 调试

练习：

- Microcorruption：`https://microcorruption.com`
- ARM 平台 pwn 题
- 路由器固件解包与漏洞分析
- 复现公开 IoT CVE

---

### 阶段 8：漏洞挖掘与模糊测试

目标：掌握自动化漏洞挖掘方法，理解程序分析、符号执行、动态污点。

资源：

- **《Fuzzing: Brute Force Vulnerability Discovery》**
- **《Practical Binary Analysis》**
- **《The Art of Software Security Assessment》**
- **《A Bug Hunter's Diary》**
- Fuzzing 101：`https://github.com/antonio-morales/Fuzzing101`

工具：

- 模糊测试：AFL++、libFuzzer、honggfuzz、syzkaller
- 符号执行：angr、KLEE、Triton、Manticore
- 动态插桩：DynamoRIO、Intel PIN、Frida
- 静态分析：CodeQL、Ghidra Script、IDA Python

实践：

- 用 AFL++ 对开源项目进行 fuzz
- 参与 OSS-Fuzz
- 使用 angr 解决 CTF 题目
- 分析 Google Project Zero、ZDI 的漏洞报告

---

### 阶段 9：实战与研究输出

目标：从学习者过渡到独立研究员。

方式：

- 长期打 CTF：关注 CTFtime，重点 pwn、reverse、kernel、fuzzing
- 复现公开 CVE：Exploit-DB、NVD、CVE Details
- 读安全论文：IEEE S&P、USENIX Security、CCS、NDSS、ACSAC
- 跟踪前沿博客：
  - Google Project Zero
  - Zero Day Initiative
  - Microsoft Security Response Center
  - 看雪、先知、安全客、Seebug Paper
- 写分析文章、技术博客
- 尝试漏洞挖掘，提交 CVE 或参加漏洞赏金

社区与论坛：

- 看雪论坛：`https://bbs.kanxue.com`
- 52 破解：`https://www.52pojie.cn`
- 先知社区：`https://xz.aliyun.com`
- 安全客：`https://www.anquanke.com`
- GitHub Awesome 系列：
  - `awesome-reverse-engineering`
  - `awesome-binary-security`
  - `awesome-fuzzing`
  - `awesome-ctf`

---

## 三、资源速查清单

### 必读书籍

| 方向 | 书名 |
|------|------|
| 系统基础 | 《深入理解计算机系统》 |
| 系统基础 | 《操作系统导论》 |
| 链接/格式 | 《程序员的自我修养》 |
| 逆向 | 《加密与解密》 |
| 逆向 | 《IDA Pro 权威指南》 |
| 逆向 | 《Practical Reverse Engineering》 |
| 漏洞利用 | 《0day 安全：软件漏洞分析技术》 |
| 漏洞利用 | 《The Shellcoder's Handbook》 |
| 漏洞利用 | 《Hacking: The Art of Exploitation》 |
| 内核 | 《Linux 内核设计与实现》 |
| 内核 | 《Windows Internals》 |
| 漏洞挖掘 | 《Fuzzing: Brute Force Vulnerability Discovery》 |
| 漏洞挖掘 | 《Practical Binary Analysis》 |
| 移动安全 | 《Android 软件安全与逆向分析》 |

### 在线练习平台

- pwn.college
- ROP Emporium
- Exploit Education
- pwnable.kr / pwnable.tw
- Crackmes.one / reversing.kr
- Microcorruption
- HackTheBox / TryHackMe
- CTFtime

### 核心工具链

- 逆向：IDA Pro、Ghidra、Binary Ninja、radare2/Cutter
- 调试：GDB + pwndbg/gef、x64dbg、WinDbg、LLDB
- 利用开发：pwntools、ROPgadget、Ropper、one_gadget
- 程序分析：angr、Unicorn、Capstone、Keystone、Triton
- 模糊测试：AFL++、libFuzzer、honggfuzz、syzkaller
- 移动：Frida、jadx、apktool、Hopper

### 课程与视频

- CMU 15-213
- MIT 6.S081
- OpenSecurityTraining2
- pwn.college
- RPISEC Modern Binary Exploitation
- 南京大学操作系统课程

---

## 四、最后建议

1. **不要只刷 CTF**，要补操作系统、编译原理、计算机体系结构，否则容易遇到瓶颈。
2. **多做最小复现**，每个漏洞类型都自己写 PoC、画内存图。
3. **保持英文阅读能力**，一流资料和漏洞报告基本是英文。
4. **选择一个方向专精**：内核、浏览器、移动、IoT、虚拟化、Fuzzing、恶意样本分析等。
5. **长期输出**：写博客、做分享、复现 CVE、提交漏洞，这些是成为“研究员”的关键路径。

如果你想，我也可以进一步帮你制定一个 6 个月或 12 个月的详细周计划。
