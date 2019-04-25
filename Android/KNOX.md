# Samsung KNOX Feature

| Version | Time | Name | Description |
| :----- | :--- | :--- | :---------- |
| [v2.4.1](https://seap.samsung.com/content/whats-new-knox-241) | Jul 2015 | Secure Boot, RKP(?) | It is believed RKP introduced with KNOX 2.4. RKP is a TZ-based security suite |
| [v2.6](https://www.samsungknox.com/en/blog/whats-new-in-knox-26) | Feb 2016 | Enhanced Real Time Kernel Protection (RKP), Kernel address space layout randomization | kASLR makes kernel exploit difficult by randomizing kernel memory layout |
| [v2.7](https://seap.samsung.com/content/whats-new-knox-271) | | JOPP (Jump Oriented Programming Protection)  | Protection from JOP exploits |
| [v2.8](https://www.samsungknox.com/en/blog/whats-new-in-knox-28) | April 2017 | Control Flow Protection, Protection from ROP exploits | kCFI will prevent exploiting memory corruption vulnerability challenging by eliminating usable pointer corruption points. |

* Reference: [Knox features on Android](https://www.samsungknox.com/en/knox-features/android)

## Realtime Kernel Protection (RKP) (v2.4 ~ )
  * RKP prevents running unauthorized privileged code on the system and kernel data from being directly accessed by user processes.  Also, it monitors some critical kernel data structures to verify that they are not exploited by attacks.

 Reference: [Real-time Kernel Protection (RKP)](https://www.samsungknox.com/en/blog/real-time-kernel-protection-rkp)

* RKP call entry: rkp_call()

* RKP runs in the virtualization protected environment rather than TrustZone Secure World

 Reference: [Real-time Kernel Protection (RKP)](https://www.samsungknox.com/en/blog/real-time-kernel-protection-rkp)

![rkp_0](https://cdn.samsungknox.com/knoxportal/files/rkp_0.png "rkp_0")

* RKP ensures that translation tables cannot be modified by the Normal World through making them read-only to the Normal World kernel. Hence, the only way for the kernel to update the translation tables is to request these updates from RKP. As a result, RKP guarantees that this interception is non-bypassable.

Reference: [Real-time Kernel Protection (RKP)](https://www.samsungknox.com/en/blog/real-time-kernel-protection-rkp)

* [KDFI](KDFI.md)

### Kernel Data Overwrite

* FPT(Function Pointer Table) overwrite
   * ptmx_fops->fsync(~ 2008)
   * dev_attr_ro->show (levitator, 2011 ~ )

## thread_info->addr_limit Corruption 

* [The Stack is Back](https://jon.oberheide.org/files/infiltrate12-thestackisback.pdf) - 2012
* [geohot's towelroot](https://towelroot.com/) - 2014
   * Rooting for Galaxy S5

* From [Exploiting the Futex Bug and uncovering Towelroot](https://tinyhack.com/2014/07/07/exploiting-the-futex-bug-and-uncovering-towelroot/) - Jul 2014

```
The addr_limit is a nice target for overwrite. The default value for ARM is 3204448256 (0xbf000000), this value is checked in every operation in kernel that copy values from and to user space. 

If we can somehow increase this limit, we can then read and modify kernel structures (using read/write syscall). Using a list manipulation, we can overwrite an address, and the location of addr_limit the one we want to overwrite.
```
   
* Mitigation: RKP (2016 ~ )

## Secure Boot (v2.4 ~ )
* Secure Boot is a security mechanism that prevents unauthorized boot loaders and kernels from being loaded during the startup process.  Firmware images, such as operating systems and system components, cryptographically signed by known, trusted authorities, are considered authorized firmware.

## Kernel address space layout randomization (v2.6 ~ )
* The Knox platform ensures that the memory address of kernel data structures and code are randomized from one device to another.

### Bypass KASLR

* Readable TIMA logs (S7)
   * Kernel pointers leaked in global readable file /proc/tima_secure_rkp_log. At 0x13B80 of this file, it leaked the actual address of init_user_ns. “init_user_ns” is a global variable in kernel’s data section

``` 
/proc/tima_debug_log
/proc/tima_debug_rkp_log
/proc/tima_secure_rkp_log
```

 Reference: [Defeating Samsung KNOX with zero privilege](https://www.blackhat.com/docs/us-17/thursday/us-17-Shen-Defeating-Samsung-KNOX-With-Zero-Privilege.pdf)

## JOPP (Jump Oriented Programming Protection) (v2.7 ~ )

* Knox now prevents Jump-Oriented Programming (JOP) attacks, such as those enabled by the Ping Pong APK. JOP attacks use jump instructions to control, or modify, the kernel flow and modify system-critical data.


## Protection from ROP exploits (v2.8 ~ )

* The Knox platform prevents Return Oriented Programming (ROP) exploits. This enhancement restricts an attacker’s ability to hijack the control-flow of an OS kernel by encrypting return addresses before putting them on the stack.
  
# Examples

## Common LPE flow on Android

Arbitrary Kernel Memory Overwriting -> Overwrite ptmx_fops -> Overwrite address_limit -> Overwrite uid, security id and selinux_enforcing

 Reference: [Defeating Samsung KNOX with zero privilege](https://www.blackhat.com/docs/us-17/thursday/us-17-Shen-Defeating-Samsung-KNOX-With-Zero-Privilege.pdf)

## LPE flow on Galaxy S7 Edge

<b>Bypass KASLR</b> -> Arbitrary Kernel Memory Overwriting -> Overwrite ptmx_fops -> Overwrite address_limit -> <b>Bypass DFI</b> -> <b>Bypass SELinux for Samsung</b> -> Gain Root privilege

 Reference: [Defeating Samsung KNOX with zero privilege](https://www.blackhat.com/docs/us-17/thursday/us-17-Shen-Defeating-Samsung-KNOX-With-Zero-Privilege.pdf)
