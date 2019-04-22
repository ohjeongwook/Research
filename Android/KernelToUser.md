## Kernel to User-mode

### call_usermodehelper bypass

p. 24 ~ 31
[New Reliable Android Kernel Root Exploitation Techniques](http://powerofcommunity.net/poc2016/x82.pdf) - 2016

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
