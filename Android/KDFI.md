# KDFI: Kernel Data Control Flow Integrity
  * Protects important kernel data structures
  * KNOX 2.6 ~

## Kernel object protection

* S6
* Protected Object type (name of its kmem_cache):
   cred_jar_ro : credential of processes
   tsec_jar: securitycontext
   vfsmnt_cache: struct vfsmount

## Bypasses - cred_jar_ro

1. Manipulate credentials and security context in kernel mode
2. Point current credential to init_cred
3. Call rkp_override_creds() to ask secure world to help us override credential with uid 0~1000

### Reusing Init's Credentials

current -> struct cred * -> init_cred (reusing init's credentials)

S7: Data Flow Integrity

### Kernel object protection
   cred_jar_ro -> rkp_override_creds()
   
#### Fix

* Unprivileged process(uid>1000) cannot override the credential with high privilege(uid 0~1000)

## Data Flow Integrity

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

## Bypasses
* [orderly_poweroff​ Bypass](KernelToUser.md#orderly_poweroff-bypass)

# References

[Defeating Samsung KNOX
with zero privilege](https://www.blackhat.com/docs/us-17/thursday/us-17-Shen-Defeating-Samsung-KNOX-With-Zero-Privilege-wp.pdf)
