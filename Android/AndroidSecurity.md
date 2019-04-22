## Samsung KNOX Feature

* Realtime Kernel Protection (RKP) (v2.4 ~ )
* Secure Boot (v2.4 ~ )
* Kernel address space layout randomization (v2.6 ~ )
* Protection from JOP exploits (v2.7 ~ )
* Protection from ROP exploits (v2.8 ~ )

[Knox features on Android](https://www.samsungknox.com/en/knox-features/android)

## Realtime Kernel Protection (RKP) (v2.4 ~ )
  * RKP prevents running unauthorized privileged code on the system and kernel data from being directly accessed by user processes.  Also, it monitors some critical kernel data structures to verify that they are not exploited by attacks.

[Real-time Kernel Protection (RKP)](https://www.samsungknox.com/en/blog/real-time-kernel-protection-rkp)

## Realtime Kernel Protection (RKP)

* RKP runs in the virtualization protected environment rather than TrustZone Secure World

[Real-time Kernel Protection (RKP)](https://www.samsungknox.com/en/blog/real-time-kernel-protection-rkp)

## Realtime Kernel Protection (RKP)

![rkp_0](https://cdn.samsungknox.com/knoxportal/files/rkp_0.png "rkp_0")

## Realtime Kernel Protection (RKP)

* RKP ensures that translation tables cannot be modified by the Normal World through making them read-only to the Normal World kernel. Hence, the only way for the kernel to update the translation tables is to request these updates from RKP. As a result, RKP guarantees that this interception is non-bypassable.

[Real-time Kernel Protection (RKP)](https://www.samsungknox.com/en/blog/real-time-kernel-protection-rkp)
  
## Secure Boot (v2.4 ~ )
  * Secure Boot is a security mechanism that prevents unauthorized boot loaders and kernels from being loaded during the startup process.  Firmware images, such as operating systems and system components, cryptographically signed by known, trusted authorities, are considered authorized firmware.

## Kernel address space layout randomization (v2.6 ~ )
   * The Knox platform ensures that the memory address of kernel data structures and code are randomized from one device to another.

## Protection from JOP exploits (v2.7 ~ )
  * Knox now prevents Jump-Oriented Programming (JOP) attacks, such as those enabled by the Ping Pong APK. JOP attacks use jump instructions to control, or modify, the kernel flow and modify system-critical data.

## Protection from ROP exploits (v2.8 ~ )
  * The Knox platform prevents Return Oriented Programming (ROP) exploits. This enhancement restricts an attacker’s ability to hijack the control-flow of an OS kernel by encrypting return addresses before putting them on the stack.
  
## Android Mitigations

### KCFI: Kernel Control Flow Integrity
  https://github.com/kcfi/docs/blob/master/kCFI_slides.pdf

  * S9
  일단 삼성이 먼저 기술을 도입하여 적용해보고 검증이 끝나면 그 이후에 구글이 차기 안드로이드 버전에 적용한다고 봐도 무방할 듯 하고요. 삼성에서 KNOX를 위해 구현했던 KCFI 완화 기술을 구글이 안드로이드 9부터 기본 적용했다는 것은 권한 상승 공격에 대응할 필요성을 느꼈기 때문이라 판단됩니다.
  
  * addr_limit 공격은 과거부터 주로 악용되었던 방법 -> Bypass
  
  * Bypass: 높은 권한(커널 쓰레드)의 자식 프로세스로 실행되는 수평적인 권한 상승 공격은 탐지를 실패는.. 간단한 아이디어(POC 2016)로 우회 된다는 점
          
### KDFI: Kernel Data Control Flow Integrity
  * KNOX의 KDFI 적용으로 인한 보호 대상 범위를 구지 건들지 않더라도 권한 상승이 충분히 가능하다는 점

### JOPP: Jump Oriented Programming Protection
  * 간단한 아이디어(복사 함수 역할을 하는 새로운 gadget)로 우회했다는 점
  
### PXN
#### ret2user

* The place where attackers have the most control over memory layout tends to be in userspace, so it has been natural to place malicious code in userspace and have the kernel redirection execution there.

[Exploit Methods/Userspace execution](https://kernsec.org/wiki/index.php/Exploit_Methods/Userspace_execution)

#### PXN: Privilege Execute Never

* make userspace non-executable for the kernel

#### PXN: Privilege Execute Never

* ARM v7 (32-bit) LPAE (e.g. Cortex-A7, A15+)
   * hardware PXN (since Linux v3.19)

* v8.0+ (64-bit)
   * hardware PXN

[ARM Feature](http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.den0024a/BABCEADG.html)
   
#### PXN: Privilege Execute Never

* Android release 5.x
   * Privileged eXecute Never (PXN) [3]: Disallow the kernel from executing userspace. Prevents ‘ret2user’ style attacks.

[The Android Platform Security Model](https://arxiv.org/pdf/1904.05572.pdf)



[Defeating Samsung KNOX
with zero privilege](https://www.blackhat.com/docs/us-17/thursday/us-17-Shen-Defeating-Samsung-KNOX-With-Zero-Privilege-wp.pdf)

## RKP
### KNOX 2.6
RKP call entry: rkp _call()

config KERNEL_TEXT_RDONLY
 - Data Section NX

PXN - Privileged eXecute Never - mitigate ret2user

### KNOX 2.8
 * COntrol Flow Protection
 
## Kernel data protection

## Kernel object protection

* S6
* Protected Object type (name of its kmem_cache):
   cred_jar_ro : credential of processes
   tsec_jar: securitycontext
   vfsmnt_cache: struct vfsmount

## Kernel object protection
   cred_jar_ro -> rkp_override_creds()

## Fix

* Unprivileged process(uid>1000) cannot override thecredential with high privilege(uid 0~1000)

## Reusing Init's Credentials

current -> struct cred * -> init_cred (reusing init's credentials)

S7: Data Flow Integrity

## Data Flow Integrity
KNOX 2.6

* Linux Kernel: security_integrity_current()
   * Verifies process's credential in real-time
     * current struct cred{} and struct task_security_struct{} are allocated in RO page
     * cred->bp_task is owned by current process
     * task_security->bp_cred is owned by current cred
     * current mount namespace is not malformed
* Secure World: Integrity_checking()

## Data Flow Integrity
* struct task_security_struct{}
   * bp_cred: pointer to this context's owner cred


## Kernel to User-mode

### call_usermodehelper bypass

p. 24 ~ 31
[New Reliable Android Kernel Root Exploitation Techniques](http://powerofcommunity.net/poc2016/x82.pdf) - 2016

### call_usermodehelper bypass
* call_usermodehelper(path,argv,envp,wait)
   * via ptmx_fops->check_flags(flag)

* user-mode​ ​ helpe: run user mode commands from kernel
```
int​ call_usermodehelper​ (const​ ​ char​ ​ *​ ​ path,
    char​ ​ **​ ​ argv,
    char​ ​ **​ ​ envp,
    int​ ​ wait); 
```

### run_cmd
```
static​ int​ run_cmd​ (const​ ​ char​ ​ *cmd)
{
    ...​ ​ //​ ​ argv_split​ ​ and​ ​ call_usermodehelper 
} 
```

### orderly_poweroff​

```
static​ int​ __orderly_poweroff​ (bool​ ​ force) 
{ 
    ...​ ​ //​ ​ argv_split​ and​ ​ call_usermodehelper​ on​ ​ poweroff_cmd 
} 
```

### orderly_poweroff() bypass
* Call orderly_poweroff()
   -> __orderly_poweroff -> call_usermodehelper -> char poweroff_cmd[] "/sbin/poweroff"
   * char poweroff_cmd[] is +W

## Common LPE flow on Android
Arbitrary Kernel Memory Overwriting -> Overwrite ptmx_fops -> Overwrite address_limit -> Overwrite uid, security id and selinux_enforcing

## LPE flow on Galaxy S7 edge

Bypass KASLR -> Arbitrary Kernel Memory Overwriting -> Overwrite ptmx_fops -> Overwrite address_limit -> Bypass DFI -> Bypass SELinux for Samsung -> Gain Root privilege


### Bypass KASLR
 * Readable TIMA logs

``` 
/proc/tima_debug_log
/proc/tima_debug_rkp_log
/proc/tima_secure_rkp_log
```

[Defeating Samsung KNOX with zero privilege](https://www.blackhat.com/docs/us-17/thursday/us-17-Shen-Defeating-Samsung-KNOX-With-Zero-Privilege.pdf)

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


