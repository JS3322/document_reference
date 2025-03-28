데이터 중심 프로그래밍은 객체지향 프로그래밍의 아래 네 가지 문제를 해결하려는 목표를 가지고 있습니다.

코드와 데이터가 혼합됨 (Code and data are mixed)
객체는 가변적임 (Objects are mutable)
데이터는 객체 내의 멤버로 갇혀 있음 (Data is locked in objects as members)
코드는 클래스 안의 메서드로 갇혀 있음 (Code is locked into classes as methods)

따라서, 데이터 중심 프로그래밍은 데이터를 (불변의) 데이터로 표현하고, 이 데이터에 대한 독립적인 연산을 수행하는 비즈니스 로직을 구현하는 코드를 작성하는 것을 추천합니다. 이렇게 하면 데이터와 로직이 명확하게 분리되어, 프로그램의 구조가 더욱 명료해지며 유지보수도 용이해지도록 돕습니다.

DOP의 핵심은 바로 데이터를 일급 시민으로 취급하는 것입니다. 이는 프로그래머가 숫자나 문자열을 다루는 것처럼 단순하게 프로그램 내의 데이터를 조작할 수 있게 함을 의미합니다. 이를 가능하게 하는 네 가지 핵심 원칙은 다음과 같습니다.

원칙 #1 : 데이터에서 코드(동작)를 분리합니다.
원칙 #2 : 일반 데이터 구조로 데이터를 표현합니다.
원칙 #3 : 데이터를 불변으로 취급합니다.
원칙 #4 : 데이터 스키마와 데이터 표현을 분리합니다.

https://blog.klipse.tech/databook/2022/06/22/separate-code-from-data.html

---

## dop가 필요한 이유?

캐시(Cache)는 CPU와 메인 메모리 간의 속도 차이를 완화하기 위한 고속의 임시 저장소입니다. CPU는 캐시를 사용하여 반복적으로 접근하는 데이터나 명령어를 빠르게 읽어올 수 있습니다.  캐시 사용으로 인해 CPU는 더 높은 처리 속도와 성능을 발휘할 수 있습니다.

L1 캐시(Level 1 Cache): CPU에 가장 가까운 캐시로, 속도가 가장 빠릅니다. 하지만 용량이 작아, 자주 사용되는 데이터와 명령어만 저장할 수 있습니다. L1 캐시는 데이터 캐시와 명령어 캐시로 나뉘어져 있을 수 있습니다.
L2 캐시(Level 2 Cache): L1 캐시보다 용량이 크고 속도는 다소 느린 캐시입니다. L2 캐시는 L1 캐시에서 찾지 못한 데이터와 명령어를 저장하며, CPU와 메인 메모리 사이의 성능 차이를 줄입니다. L2 캐시는 CPU 내부에 있거나 외부에 위치할 수 있습니다.
L3 캐시(Level 3 Cache): L2 캐시보다 용량이 더 크고 속도는 더 느린 캐시입니다. L3 캐시는 여러 CPU 코어가 공유할 수 있으며, L2 캐시에서 찾지 못한 데이터와 명령어를 저장합니다. 이 캐시 계층은 시스템 성능 향상에 기여합니다.

예를 들어, 간단하게 설명하기 위해 L1 캐시 라인의 캐시 크기를 16바이트라고 가정합시다. 이는 명령에 대한 요청 주소에서 시작해 16바이트가 복사된다는 것을 의미합니다.

첫 번째 코드 예제에서는 프로그램이 다음 바이트에 작업을 시도하고, 초기 캐시 미스 이후 이미 캐시로 복사된 데이터를 사용해 원활하게 진행됩니다. 이는 다음 14바이트에 대해서도 동일합니다. 그러나 16바이트가 지난 후에는, 처음 캐시 미스 이후 루프가 다시 캐시 미스를 만나게 되고, CPU는 다음 16바이트를 캐시로 복사할 때까지 데이터를 기다리게 됩니다.

두 번째 코드 샘플에서는 루프가 한 번에 16바이트씩 건너뛰지만 하드웨어는 같은 방식으로 계속 작동합니다. 캐시 미스가 발생할 때마다 캐시는 16바이트를 복사하는데, 이는 루프가 각 반복마다 캐시 미스를 발생시키고 CPU가 매번 데이터를 기다리는 동안 대기하게 됨을 의미합니다.

데이터를 배열에 할당하는 것과 같은 간단한 작업이라도 성능을 고려하면 프로그래밍 방식에 대해 신중하게 고민해야 합니다. 이런 데이터의 성능을 중점으로 설계를 고민하기 시작했고, 그렇게 생겨난 설계 방식 중 하나가 바로 데이터지향설계(Data-Oriented Design, DOD)입니다.

---

데이터 지향 설계의 핵심 원칙은 다음과 같습니다:

데이터 구조화: 데이터를 구조화하고 관련 있는 데이터를 물리적으로 서로 가까운 위치에 저장하여 메모리 접근 속도를 높이고 캐시 효율성을 개선합니다.

데이터 변환: 프로그램을 데이터 변환 단계로 분해하여 각 단계에서 데이터를 처리하는 데 필요한 최소한의 정보만 사용하도록 합니다.

메모리 접근 최적화: 데이터를 연속적인 메모리 블록에 저장하여 캐시 지역성을 개선하고, 메모리 접근을 최소화하며, 메모리에 효율적으로 접근하는 알고리즘을 사용합니다.

병렬성 고려: 데이터를 독립적으로 처리할 수 있는 작은 단위로 분할하여 병렬 처리를 용이하게 하고, 다중 코어 및 다중 스레드 프로세서의 성능을 최대한 활용합니다.

데이터 지향 설계의 장점은 성능 최적화와 관련된 것이 주로 있습니다. 캐시 효율성을 높여 연산 속도를 빠르게 하고, 병렬 처리를 통해 다중 코어 및 다중 스레드 프로세서의 성능을 극대화할 수 있습니다. 이러한 이유로 데이터 지향 설계는 자원 제한이 있는 환경에서 선호되는 설계 방식이 되었습니다.