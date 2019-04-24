# Android Kernel Mitigations History

| Target | Time | Name |
| :----- | :--- | :--- |
| Android 2.3 | Dec 6, 2010 | mmap_min_addr (zero-page restrict) | Linux mmap_min_addr to mitigate null pointer dereference privilege escalation (further enhanced in Android 4.1) |
| Android 4.1 | Jul 9, 2012 | kptr_restrict, dmesg_restrict |
| Android 4.4 | Oct 31, 2013 | [PXN](https://android.googlesource.com/kernel/msm/+/8e620b0476696e9428442d3551f3dad47df0e28f) |
| Android 5.0 | Nov 12, 2014 | [SEAndroid](https://source.android.com/security/selinux) |
| Android 6.0 | Nov 7 2014 | [KERNEL_TEXT_RDONLY](https://android.googlesource.com/kernel/msm/+/c45a4e6e07478a8cc7e513cca5582f472c3cd0cb) |
| Android 6.0 | Oct 5, 2015 | [CONFIG_DEBUG_RODATA](https://android-developers.googleblog.com/2016/07/protecting-android-with-more-linux.html) |
| [KNOX 2.4.1](https://seap.samsung.com/content/whats-new-knox-241) | Jul 2015 | RKP(?) |
| [KNOX 2.6](https://www.samsungknox.com/en/blog/whats-new-in-knox-26) | Feb 2016 | Enhanced Real Time Kernel Protection (RKP), Kernel address space layout randomization |
| [KNOX 2.8](https://www.samsungknox.com/en/blog/whats-new-in-knox-28) | April 2017 | Control Flow Protection | 

* [Security Enhancements in Android 1.5 through 4.1](https://source.android.com/security/enhancements/enhancements41) |


# Android Kernel/TZ-based Mitigations
## [AndroidKernelMitigations](AndroidKernelMitigations.md)
## [KNOX Mitigations](KNOX.md)
