# SELinux on S7

## Bypass
* Overwrite global variable ss_initialized (writable) -> 0
   * All labels will reset to non except kernel domain
   * Can load customized policy and reinitiaze SELinux
   
* [Defeating Samsung KNOX
with zero privilege](https://www.blackhat.com/docs/us-17/thursday/us-17-Shen-Defeating-Samsung-KNOX-With-Zero-Privilege-wp.pdf)

