# Android Kernel Mitigations History

| Target | Time | Name | Description |
| :----- | :--- | :--- | :---------- |
| Android 2.3 | Dec 6, 2010 | mmap_min_addr (zero-page restrict) | Linux mmap_min_addr to mitigate null pointer dereference privilege escalation (further enhanced in Android 4.1) |
| Android 4.1 | Jul 9, 2012 | [kptr_restrict](https://lwn.net/Articles/420403/) | Prevent kernel pointer leak |
| Android 4.1 | Jul 9, 2012 | dmesg_restrict | Prevent users from looking at dmesg output |
| Android 4.4 | Oct 31, 2013 | [PXN](https://android.googlesource.com/kernel/msm/+/8e620b0476696e9428442d3551f3dad47df0e28f) | Prevent kernel to change execution to user mode +X memory |
| Android 5.0 | Nov 12, 2014 | [SEAndroid](https://source.android.com/security/selinux) | Security Enhancements for Android, a security solution for Android that identifies and addresses critical gaps |
| Android 6.0 | Nov 7 2014 | [KERNEL_TEXT_RDONLY](https://android.googlesource.com/kernel/msm/+/c45a4e6e07478a8cc7e513cca5582f472c3cd0cb) | Enable this option to mark the kernel text pages as read only to trigger a fault if any code attempts to write to a page part of the kernel text section. |
| Android 6.0 | Oct 5, 2015 | [CONFIG_DEBUG_RODATA](https://android-developers.googleblog.com/2016/07/protecting-android-with-more-linux.html) | Kernel text and rodata will be made read-only. This is to help catch accidental or malicious attempts to change the kernel's executable code. |
| [KNOX 2.4.1](https://seap.samsung.com/content/whats-new-knox-241) | Jul 2015 | Secure Boot, RKP | It is believed RKP introduced with KNOX 2.4. RKP is a TZ-based security suite |
| [KNOX 2.6](https://www.samsungknox.com/en/blog/whats-new-in-knox-26) | Feb 2016 | Enhanced Real Time Kernel Protection (RKP), Kernel address space layout randomization | kASLR makes kernel exploit difficult by randomizing kernel memory layout |
| [KNOX 2.8](https://www.samsungknox.com/en/blog/whats-new-in-knox-28) | April 2017 | Control Flow Protection | kCFI will prevent exploiting memory corruption vulnerability challenging by eliminating usable pointer corruption points. |

* [Security Enhancements in Android 1.5 through 4.1](https://source.android.com/security/enhancements/enhancements41) |

## Kernel memory leak

/proc/(kallsyms, modules, slabinfo)
/sys

## Kernel +X Memory Overwrite
2012 [exynosabuse](https://golos.io/book/@mestn1k/gaining-root-on-a-booted-system) (alephzain): Overwrite setresuid syscall

Mitigation: TIMA PKM (Periodic Kernel Measurement): KK (32), KAP (64)

## Kernel Data Overwrite

* FPT(Function Pointer Table) overwrite
   * ptmx_fops->fsync(~ 2008)
   * dev_attr_ro->show (levitator, 2011 ~ )
   
Mitigation: TIMA RKP (2016 ~ )

## ret2user

* Run shellcode in userland memory - usually change current user's uid/guid (in PCB - task_struct) using shellcode

## thread_info->addr_limit Corruption (2014 geohot's towelroot)


# Android Kernel/TZ-based Mitigations
## [Android Kernel Mitigations](AndroidKernelMitigations.md)
## [KNOX Mitigations](KNOX.md)
