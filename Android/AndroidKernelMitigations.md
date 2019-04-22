# Android Mitigations

## PXN
Details: [PXN](KernelToUser.md#PXN)

## KCFI: Kernel Control Flow Integrity

* Control flow integrity (CFI) is a security mechanism that disallows changes to the original control flow graph of a compiled binary, making it significantly harder to perform such attacks. - [Kernel Control Flow Integrity
](https://source.android.com/devices/tech/debug/kcfi)

* Forward edge computation requires heuristics - [DROP THE ROP: Fine Grained Control-Flow integrity for The Linux Kernel](https://github.com/kcfi/docs/blob/master/kCFI_slides.pdf)

### Patches

[Version 4.9](https://android-review.googlesource.com/q/topic:android-4.9-cfi)
[Version 4.14](https://android-review.googlesource.com/q/topic:android-4.14-cfi)
  
