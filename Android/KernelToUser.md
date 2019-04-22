# Kernel to User-mode
## PXN
### ret2user

* The place where attackers have the most control over memory layout tends to be in userspace, so it has been natural to place malicious code in userspace and have the kernel redirection execution there.

[Exploit Methods/Userspace execution](https://kernsec.org/wiki/index.php/Exploit_Methods/Userspace_execution)

### PXN: Privilege Execute Never

* Make userspace non-executable for the kernel

### Supported Platforms - ARM

* ARM v7 (32-bit) LPAE (e.g. Cortex-A7, A15+)
   * hardware PXN (since Linux v3.19)

* v8.0+ (64-bit)
   * hardware PXN

[ARM Feature](http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.den0024a/BABCEADG.html)
   
### PXN: Privilege Execute Never

* Android release 5.x
   * Privileged eXecute Never (PXN) [3]: Disallow the kernel from executing userspace. Prevents ‘ret2user’ style attacks.

[The Android Platform Security Model](https://arxiv.org/pdf/1904.05572.pdf)

### call_usermodehelper bypass
* call_usermodehelper(path,argv,envp,wait)
   * via ptmx_fops->check_flags(flag)

* user-mode​ ​ helpe: run user mode commands from kernel
```
int​ call_usermodehelper​ (const​ ​ char​ ​ *​ ​ path,
    char​ ​ **​ ​ argv,
    char​ ​ **​ ​ envp,
    int​ ​ wait); 
```

### run_cmd
```
static​ int​ run_cmd​ (const​ ​ char​ ​ *cmd)
{
    ...​ ​ //​ ​ argv_split​ ​ and​ ​ call_usermodehelper 
} 
```

### call_usermodehelper bypass

p. 24 ~ 31
[New Reliable Android Kernel Root Exploitation Techniques](http://powerofcommunity.net/poc2016/x82.pdf) - 2016


### orderly_poweroff​

```
static​ int​ __orderly_poweroff​ (bool​ ​ force) 
{ 
    ...​ ​ //​ ​ argv_split​ and​ ​ call_usermodehelper​ on​ ​ poweroff_cmd 
} 
```

### orderly_poweroff() bypass
* Call orderly_poweroff()
   -> __orderly_poweroff -> call_usermodehelper -> char poweroff_cmd[] "/sbin/poweroff"
   * char poweroff_cmd[] is +W
