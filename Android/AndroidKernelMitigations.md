# Android Kernel Mitigations

## PXN (Privilege eXecute Never)

* Make userspace non-executable for the kernel

### Supported Platforms - ARM

* ARM v7 (32-bit) LPAE (e.g. Cortex-A7, A15+)
   * hardware PXN (since Linux v3.19)

* v8.0+ (64-bit)
   * hardware PXN

[ARM Feature](http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.den0024a/BABCEADG.html)
   
### Supported Versions

* Android release 5.x
   * Privileged eXecute Never (PXN) [3]: Disallow the kernel from executing userspace. Prevents ‘ret2user’ style attacks.

[The Android Platform Security Model](https://arxiv.org/pdf/1904.05572.pdf)

### Bypasses

Known bypasses are documented here: [Kernel To User](KernelToUser.md)

## KCFI: Kernel Control Flow Integrity

* Control flow integrity (CFI) is a security mechanism that disallows changes to the original control flow graph of a compiled binary, making it significantly harder to perform such attacks. - [Kernel Control Flow Integrity
](https://source.android.com/devices/tech/debug/kcfi)

* Forward edge computation requires heuristics - [DROP THE ROP: Fine Grained Control-Flow integrity for The Linux Kernel](https://github.com/kcfi/docs/blob/master/kCFI_slides.pdf)

### Patches

* [Version 4.9](https://android-review.googlesource.com/q/topic:android-4.9-cfi)
* [Version 4.14](https://android-review.googlesource.com/q/topic:android-4.14-cfi)

  
