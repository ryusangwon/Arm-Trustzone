### Arm-Trustzone

Arm trustzone 정의

Arm TrustZone은 하나의 장치에서 **분리된 두 개의 환경**을 제공하며 보안이 필요한 정보를 격리된 환경에서 안전하게 보호하는 기술.

* 일반 영역인 REE(Rich Execution Environment)와 보안 영역인 TEE(Trusted Execution Environment)로 나뉜다
  * 높은 신뢰성을 요구하는 프로그램은 Secure World(TEE)에서 실행시키고 그 외의 프로그램이나 운영체제는 Normal World(REE)에서 실행시킨다.
 
* TrustZone은 CPU, memory 등 하드웨어 기반 자원을 분리한다.
  * TEE는 TEE와 REE자원 모두에 접근이 가능하고, REE는 오직 REE의 자원에만 접근이 가능하다.
  * REE에서 TEE에 접근할 때는 page fault가 발생한다. 따라서 TEE에서 실행 중인 애플리케이션을 REE에서 방해할 수 없다.
  * TEE 내부에서 실행되는 코드는 신뢰할 수 있는 코드만을 실행한다.

* 기존의 ARM의 CPU모드에는 사용자모드(User Mode)인 User, Privileged Mode인 Supervisor, FIQ(Fast IRQ), IRQ(Interrupt Request), Undefined, Abort, System 모드가 있다.
  * ARM TrustZone에서는 Monitor 모드가 추가되었다.
  * Monitor 모드는 Secure World에만 존재하며 world간의 전환을 위한 모드이다. Monitor 모드에서는 secure world에서 사용 가능한 명령어나 메모리 영역을 접근할 수 있다.

* Secure World에 접근하는 방법
  1. Kernel mode에서 SMC(Secure Monitor Call)을 통해 진입하는 방법.
  2. IRQ, FIQ 발생 시 진입하는 방법.
  * Normal World의 운영체제가 변조되어 Secure World에 대해 악의적인 동작을 시도할 경우 해당 SMC명령어를 Monitor 모드의 SMC handler에서 처리한다
 
