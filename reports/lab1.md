# chapter3 报告

> 陈毓椿 计36 2023010452

## 功能实现

- **`syscall_times`**: 在 `TaskControlBlock`(`os/src/task/task.rs`) 结构中添加了
 `syscall_times` 数组，用于对不同 id 系统调用的计数。                           
    - `MAX_SYSCALL_NUM` 用于表示支持的系统调用 ID 范围，参考目前实现的最大 ID 为
 sys_trace 的 410，实验中设置 512 恰好覆盖并预留部分空间                            
    - 在 `os/src/task/mod.rs` 完成初始化

- **`record_syscall` & `get_syscall_times`**: 在 `os/src/task/mod.rs` 实现上述两
个功能函数，`record_syscall` 用于单次调用时计数，`get_syscall_times` 用于从给定 id 读取调用次数。在每次 `sys_call` 的开头，会首先执行一次 `record_syscall` 记录。                                                                              
- **`sys_trace`**: 根据实验文档实现 `sys_trace` 系统调用，调用 `get_syscall_times` 完成 id 为 2 时的调用次数返回。                                              
## 问答题

1. 运行下述脚本，得到 `TEST=2` 时的 3 个 bad 测例结果如下：

```bash
make run TEST=2 BASE=1

# 使用的 SBI：RustSBI
# SBI 版本：ch1 提供的 rustsbi-qemu.bin

...
[kernel] PageFault in application, bad addr = 0x0, bad instruction = 0x804003a4, kernel killed it.
[kernel] IllegalInstruction in application, kernel killed it.
[kernel] IllegalInstruction in application, kernel killed it.
...
```
对应的测例行为及其解释如下：

- ch2b_bad_address - 触发非法内存访问异常，符合输出 `[kernel] PageFault in application, bad addr = 0x0, bad instruction = 0x804003a4, kernel killed it.`

- ch2b_bad_instructions - 尝试执行S态特权指令 sret，应该报错，符合输出 `[kernel] IllegalInstruction in application, kernel killed it.`

- ch2b_bad_register - 尝试读取S态特权寄存器 sstatus，应该报错，符合输出 `[kernel] IllegalInstruction in application, kernel killed it.`

2. 深入理解 `trap.S` 中两个函数 `__alltraps` 和 `__restore` 的作用
    1. 


## 荣誉准则

1. 在完成本次实验的过程（含此前学习的过程）中，我曾分别与 以下各位 就（与本次实验相关的）以下方面做过交流，还在代码中对应的位置以注释形式记录了具体的交流对象及内容：

> 《你交流的对象说明》

2. 此外，我也参考了 以下资料 ，还在代码中对应的位置以注释形式记录了具体的参考来源及内容：

> 《你参考的资料说明》

3. 我独立完成了本次实验除以上方面之外的所有工作，包括代码与文档。 我清楚地知道，从以上方面获得的信息在一定程度上降低了实验难度，可能会影响起评分。

4. 我从未使用过他人的代码，不管是原封不动地复制，还是经过了某些等价转换。 我未曾也不会向他人（含此后各届同学）复制或公开我的实验代码，我有义务妥善保管好它们。我提交至本实验的评测系统的代码，均无意于破坏或妨碍任何计算机系统的正常运转。
