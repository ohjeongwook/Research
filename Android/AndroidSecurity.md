## RKP
### KNOX 2.6
RKP call entry: rkp _call()

config KERNEL_TEXT_RDONLY
 - Data Section NX

### KNOX 2.8
 * Control Flow Protection
 
## Kernel data protection

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
