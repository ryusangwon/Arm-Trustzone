### [Arm-Trustzone]

* Arm trustzone 정의
[Arm TrustZone은 하나의 장치에서 **분리된 두 개의 환경**을 제공하며 보안이 필요한 정보를 격리된 환경에서 안전하게 보호하는 기술.]

![image](https://user-images.githubusercontent.com/78716763/118459760-0337b980-b737-11eb-9624-54f21dd4c618.png)

- 기존의 보안법은 운영체제(O/S)가 신뢰할 수 있다는 가정하에 응용프로그램(Application)을 보안해야 하는데, 최근 운영체제를 공격 및 수정하는 다양한 방법이 생김에 따라 그 밑의 하드웨어적으로 보안을 하는 방법인 TrustZone방법이 고안되었다..

---

* 일반 영역인 REE(Rich Execution Environment)와 보안 영역인 TEE(Trusted Execution Environment)로 나뉜다
  * 높은 신뢰성을 요구하는 프로그램은 Secure World(TEE)에서 실행시키고 그 외의 프로그램이나 운영체제는 Normal World(REE)에서 실행시킨다.

![image](https://user-images.githubusercontent.com/78716763/118459691-f3b87080-b736-11eb-8985-cd621c876626.png)
 
---
 
* TrustZone은 CPU, memory 등 하드웨어 기반 자원을 분리한다.
  * TEE는 TEE와 REE자원 모두에 접근이 가능하고, REE는 오직 REE의 자원에만 접근이 가능하다.
  * REE에서 TEE에 접근할 때는 page fault가 발생한다. 따라서 TEE에서 실행 중인 애플리케이션을 REE에서 방해할 수 없다.
  * TEE 내부에서 실행되는 코드는 신뢰할 수 있는 코드만을 실행한다.

---

* 기존의 ARM의 CPU모드에는 사용자모드(User Mode)인 User, Privileged Mode인 Supervisor, FIQ(Fast IRQ), IRQ(Interrupt Request), Undefined, Abort, System 모드가 있다.
  * ARM TrustZone에서는 Monitor 모드가 추가되었다.
  * Monitor 모드는 Secure World에만 존재하며 world간의 전환을 위한 모드이다. Monitor 모드에서는 secure world에서 사용 가능한 명령어나 메모리 영역을 접근할 수 있다.

![image](https://user-images.githubusercontent.com/78716763/118459536-cbc90d00-b736-11eb-82af-f9d6e4fefd4e.png)

---

* Secure World에 접근하는 방법
  1. Kernel mode에서 SMC(Secure Monitor Call)을 통해 진입하는 방법.
  2. IRQ, FIQ 발생 시 진입하는 방법.
  * Normal World의 운영체제가 변조되어 Secure World에 대해 악의적인 동작을 시도할 경우 해당 SMC명령어를 Monitor 모드의 SMC handler에서 처리한다
 
 ![image](https://user-images.githubusercontent.com/78716763/118459561-d2578480-b736-11eb-9342-6cccd9a80314.png)

 ---
 
* TrustZone 장치 부팅하는 법(2회 부팅)
  1. TEE를 먼저 부팅하고 TrustZone 관련 접근 권한을 설정한다.
  2. REE를 그 이후 부팅한다.
  * REE의 악의적인 소프트웨어가 TEE의 소프트웨어에 작동하는 것을 미리 파악하고 대비할 수 있기 때문
  
![image](https://user-images.githubusercontent.com/78716763/118459581-d7b4cf00-b736-11eb-84f3-922b552cc0eb.png)

---

* REE에서 TEE에 통신하는법
  1. REE의 CA(Client Application)에서 메시지를 보낸다.
  2. 해당 TEE Client API를 이용해 TEE로 전달한다.
  3. TEE의 TA(Trusted Application)가 메시지를 받으면 TA가 REE의 특정 Shared Memory 영역을 동기화해서 이용한다.
  4. 모든 송/수신은 GlobalPlatform API를 사용하는 보안통신을 한다. (REE가 TEE 애플리케이션 별 고유 UUID를 알아야 통신이 가능하다.)
  
![image](https://user-images.githubusercontent.com/78716763/118459646-e7341800-b736-11eb-8d78-5e5a5a8615ad.png)
