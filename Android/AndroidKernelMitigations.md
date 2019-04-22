# Android Mitigations

## PXN
### ret2user

* The place where attackers have the most control over memory layout tends to be in userspace, so it has been natural to place malicious code in userspace and have the kernel redirection execution there.

[Exploit Methods/Userspace execution](https://kernsec.org/wiki/index.php/Exploit_Methods/Userspace_execution)

### PXN: Privilege Execute Never

* Make userspace non-executable for the kernel

### Supported Platforms - ARM

* ARM v7 (32-bit) LPAE (e.g. Cortex-A7, A15+)
   * hardware PXN (since Linux v3.19)

* v8.0+ (64-bit)
   * hardware PXN

[ARM Feature](http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.den0024a/BABCEADG.html)
   
### PXN: Privilege Execute Never

* Android release 5.x
   * Privileged eXecute Never (PXN) [3]: Disallow the kernel from executing userspace. Prevents ‘ret2user’ style attacks.

[The Android Platform Security Model](https://arxiv.org/pdf/1904.05572.pdf)

## KCFI: Kernel Control Flow Integrity
  https://github.com/kcfi/docs/blob/master/kCFI_slides.pdf

  * S9
  일단 삼성이 먼저 기술을 도입하여 적용해보고 검증이 끝나면 그 이후에 구글이 차기 안드로이드 버전에 적용한다고 봐도 무방할 듯 하고요. 삼성에서 KNOX를 위해 구현했던 KCFI 완화 기술을 구글이 안드로이드 9부터 기본 적용했다는 것은 권한 상승 공격에 대응할 필요성을 느꼈기 때문이라 판단됩니다.
  
  * addr_limit 공격은 과거부터 주로 악용되었던 방법 -> Bypass
  
  * Bypass: 높은 권한(커널 쓰레드)의 자식 프로세스로 실행되는 수평적인 권한 상승 공격은 탐지를 실패는.. 간단한 아이디어(POC 2016)로 우회 된다는 점
          


## JOPP: Jump Oriented Programming Protection
  * 간단한 아이디어(복사 함수 역할을 하는 새로운 gadget)로 우회했다는 점
  
