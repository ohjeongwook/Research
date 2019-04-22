## RKP
### KNOX 2.6
RKP call entry: rkp _call()

config KERNEL_TEXT_RDONLY
 - Data Section NX

PXN - Privileged eXecute Never - mitigate ret2user

### KNOX 2.8
 * Control Flow Protection
 
## Kernel data protection

## Common LPE flow on Android
Arbitrary Kernel Memory Overwriting -> Overwrite ptmx_fops -> Overwrite address_limit -> Overwrite uid, security id and selinux_enforcing

## LPE flow on Galaxy S7 edge
Bypass KASLR -> Arbitrary Kernel Memory Overwriting -> Overwrite ptmx_fops -> Overwrite address_limit -> Bypass DFI -> Bypass SELinux for Samsung -> Gain Root privilege


### Arbitrary Kernel Memory Overwriting 
* CVE-2016-6787

### Overwrite ptmx_fops 
### Overwrite address_limit 
### Bypass DFI 
### Bypass SELinux for Samsung 
### Gain Root privilege

## SELinux on S7
global variable ss_initialized (writable) -> 0
    All labels will reset to non except kernel domain
Can load customized policy and reinitiaze SELinux

