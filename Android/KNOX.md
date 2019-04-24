# Samsung KNOX Feature

* Realtime Kernel Protection (RKP) (v2.4 ~ )
* Secure Boot (v2.4 ~ )
* Kernel address space layout randomization (v2.6 ~ )
* Protection from JOP exploits (v2.7 ~ )
* Protection from ROP exploits (v2.8 ~ )

 Reference: [Knox features on Android](https://www.samsungknox.com/en/knox-features/android)

## Realtime Kernel Protection (RKP) (v2.4 ~ )
  * RKP prevents running unauthorized privileged code on the system and kernel data from being directly accessed by user processes.  Also, it monitors some critical kernel data structures to verify that they are not exploited by attacks.

 Reference: [Real-time Kernel Protection (RKP)](https://www.samsungknox.com/en/blog/real-time-kernel-protection-rkp)

* RKP call entry: rkp_call()

* RKP runs in the virtualization protected environment rather than TrustZone Secure World

 Reference: [Real-time Kernel Protection (RKP)](https://www.samsungknox.com/en/blog/real-time-kernel-protection-rkp)

![rkp_0](https://cdn.samsungknox.com/knoxportal/files/rkp_0.png "rkp_0")

* RKP ensures that translation tables cannot be modified by the Normal World through making them read-only to the Normal World kernel. Hence, the only way for the kernel to update the translation tables is to request these updates from RKP. As a result, RKP guarantees that this interception is non-bypassable.

 Reference: [Real-time Kernel Protection (RKP)](https://www.samsungknox.com/en/blog/real-time-kernel-protection-rkp)

### KDFI

Details: [KDFI](KDFI.md)

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

## Protection from JOP exploits (v2.7 ~ )

* Knox now prevents Jump-Oriented Programming (JOP) attacks, such as those enabled by the Ping Pong APK. JOP attacks use jump instructions to control, or modify, the kernel flow and modify system-critical data.

* JOPP: Jump Oriented Programming Protection

## Protection from ROP exploits (v2.8 ~ )

* The Knox platform prevents Return Oriented Programming (ROP) exploits. This enhancement restricts an attacker’s ability to hijack the control-flow of an OS kernel by encrypting return addresses before putting them on the stack.
  
# Examples

## Common LPE flow on Android

Arbitrary Kernel Memory Overwriting -> Overwrite ptmx_fops -> Overwrite address_limit -> Overwrite uid, security id and selinux_enforcing

 Reference: [Defeating Samsung KNOX with zero privilege](https://www.blackhat.com/docs/us-17/thursday/us-17-Shen-Defeating-Samsung-KNOX-With-Zero-Privilege.pdf)

## LPE flow on Galaxy S7 Edge

<b>Bypass KASLR</b> -> Arbitrary Kernel Memory Overwriting -> Overwrite ptmx_fops -> Overwrite address_limit -> <b>Bypass DFI</b> -> <b>Bypass SELinux for Samsung</b> -> Gain Root privilege

 Reference: [Defeating Samsung KNOX with zero privilege](https://www.blackhat.com/docs/us-17/thursday/us-17-Shen-Defeating-Samsung-KNOX-With-Zero-Privilege.pdf)
