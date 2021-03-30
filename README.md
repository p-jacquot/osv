# slightly modified OSv

This repository is a fork of OSv I made to test a simple modification on its source code.
I advise you to use the real OSv, and not my quickly modified version. You can find it at this link : [https://github.com/cloudius-systems/osv](https://github.com/cloudius-systems/osv).

# Original problem

While trying to execute an OpenMP application, I encountered this error :

```
page fault outside application, addr: 0x0000000000000000
[registers]
RIP: 0x0000000040477678 <__sched_cpucount+40>
RFL: 0x0000000000010246  CS:  0x0000000000000008  SS:  0x0000000000000010
RAX: 0xfffffffffffff958  RBX: 0x0000000000000000  RCX: 0x0000000000000000  RDX: 0x0000000000000000
RSI: 0x0000000000000000  RDI: 0x0000000000000008  RBP: 0x0000200000700830  R8:  0x0000000000000001
R9:  0x0000000000000000  R10: 0xffff800001fc7008  R11: 0x00001000000d5c70  R12: 0x0000000000000000
R13: 0x0000000000000000  R14: 0xffff800001fc7008  R15: 0x00001000000d5c70  RSP: 0x0000200000700810
Aborted

[backtrace]
0x000000004034084f <???+1077151823>
0x00000000403423d1 <mmu::vm_fault(unsigned long, exception_frame*)+385>
0x0000000040393243 <page_fault+147>
0x0000000040392096 <???+1077485718>
0x000000004046d080 <???+1078382720>
0x000000004046d1e1 <sched_setaffinity+33>
0x00000000403f3f19 <__syscall+2425>
```

# What did I do ?

I removed all the code from the `sched_setaffinity` syscall, and left only the `return 0`, so that the function is faking success.
The result of this removal is that the program I tried to execute doesn't crash anymore.
But, it also make the program run on only one thread.


This is what the program is now printing : 
```
Thread 0 is doing iteration 0.
Thread 0 is doing iteration 1.
Thread 0 is doing iteration 2.
[...]
Thread 0 is doing iteration 97
Thread 0 is doing iteration 98.
Thread 0 is doing iteration 99.
```

Because the thread doing iterations is always 0, I assume that there is no parallelism.
