## 功能实现
修改 TaskControlBlock 数据结构，添加 start_time 和 syscall_counter 两个成员分别代表第一次调度时间和系统调用计数。
修改 TaskManager 的成员函数，使其能够获取当前进程的第一次调度时间，系统调用计数，状态，并能更新系统调用计数。
修改 syscall 函数，每次进入 syscall 函数时，更新系统调用计数。
修改 sys_task_info 函数，使用 TaskManager 的成员函数赋值给 _ti.

## 问答
1 [kernel] PageFault in application, bad addr = 0x0, bad instruction = 0x804003c4, kernel killed it.
[kernel] IllegalInstruction in application, kernel killed it.
[kernel] IllegalInstruction in application, kernel killed it.

2.1 分配 Trap 上下文之后的内核栈栈顶

2.2 sstatus：处理器状态寄存器
    sepc：记录 Trap 返回地址。
    sscratch：用户栈地址。

2.3 x2 和 x4 在 __alltraps 时未保存，所以退出 trap 时也不需要读取。

2.4 sp：用户栈
    sscratch：内核栈

2.5 sret：CPU 会将当前的特权级按照 sstatus 的 SPP 字段设置为 U 或者 S ；CPU 会跳转到 sepc 寄存器指向的那条指令，然后继续执行。

2.6 sp：内核栈
    sscratch：用户栈

2.7 ecall 发生 trap CPU 会跳转到 stvec 所设置的 Trap 处理入口地址，并将当前特权级设置为 S ，然后从Trap 处理入口地址处开始执行。