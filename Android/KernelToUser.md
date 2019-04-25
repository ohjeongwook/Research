# Kernel to User-mode
## ret2user

* The place where attackers have the most control over memory layout tends to be in userspace, so it has been natural to place malicious code in userspace and have the kernel redirection execution there.

[Exploit Methods/Userspace execution](https://kernsec.org/wiki/index.php/Exploit_Methods/Userspace_execution)

## PXN Bypasses

### ROP/JOP to bypass PXN (2015 Keen team & wooyun)

* CVE-2015-3636


### call_usermodehelper bypass

* ptmx_fops->check_flags(flag) --> call_usermodehelper(path,argv,envp,wait)

* user-mode​​ helper: run user mode commands from kernel

```
int​ call_usermodehelper​ (const​ ​ char​ ​ *​ ​ path,
    char​ ​ **​ ​ argv,
    char​ ​ **​ ​ envp,
    int​ ​ wait); 
```

* [Defeating Samsung KNOX
with zero privilege](https://www.blackhat.com/docs/us-17/thursday/us-17-Shen-Defeating-Samsung-KNOX-With-Zero-Privilege-wp.pdf)

* [New Reliable Android Kernel Root Exploitation Techniques](http://powerofcommunity.net/poc2016/x82.pdf) - 2016
   p. 24 ~ 31
   
### orderly_poweroff​ Bypass

This method was mentioned by the following papers.

* [Defeating Samsung KNOX
with zero privilege](https://www.blackhat.com/docs/us-17/thursday/us-17-Shen-Defeating-Samsung-KNOX-With-Zero-Privilege-wp.pdf)
* [Exploiting BlueBorne in Linux-based IoT devices](https://go.armis.com/hubfs/ExploitingBlueBorneLinuxBasedIoTDevices.pdf) - 2017
* [Exploiting BlueBorne in Linux-based IoT devices - WP](https://www.blackhat.com/docs/eu-17/materials/eu-17-Seri-BlueBorne-A-New-Class-Of-Airborne-Attacks-Compromising-Any-Bluetooth-Enabled-Linux-IoT-Device-wp.pdf) - 2017
* [New Reliable Android Kernel Root Exploitation Techniques](http://powerofcommunity.net/poc2016/x82.pdf) - 2016

#### run_cmd

```
static​ int​ run_cmd​ (const​ ​ char​ ​ *cmd)
{
    ...​ ​ //​ ​ argv_split​ ​ and​ ​ call_usermodehelper 
} 
```

#### orderly_poweroff​

```
static​ int​ __orderly_poweroff​ (bool​ ​ force) 
{ 
    ...​ ​ //​ ​ argv_split​ and​ ​ call_usermodehelper​ on​ ​ poweroff_cmd 
} 
```

#### orderly_poweroff() bypass

* If we can overwrite one of these pre-defined strings and then invoke its respective function, we should be able to execute an arbitrary command in user-mode, with existing code doing all the heavy lifting for us - [Exploiting BlueBorne in Linux-based IoT devices](https://go.armis.com/hubfs/ExploitingBlueBorneLinuxBasedIoTDevices.pdf) - 2017

* Call orderly_poweroff()
   -> __orderly_poweroff -> call_usermodehelper -> char poweroff_cmd[] "/sbin/poweroff"
   * char poweroff_cmd[] is +W ([KDFI](KDFI.md) bypass)
