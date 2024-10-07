# MSA ( Micro Service Architecture )

### MSA 는 작고 독립적인 서비스들의 집합으로 구성된 애플리케이션 구조이다.
> - 애플리케이션을 느슨하게 결합된 서비스의 모임으로 구조화하는 **서비스 지향 아키텍쳐 ( SOA ) 스타일의 일종인 소프트웨어 개발 기법**
> - 서비스 지향 아키텍쳐 ( Service Oriented Architecture )
>   - 애플리케이션 구성요소가 통신프로토콜을 통해 다른 구성요소에 서비스를 제공하는 아키텍처 접근방식
>   - 대규모 컴퓨터 시스템을 구축할 때의 개념으로 업무상에 일 처리에 해당하는 소프트웨어 기능을 서비스
> 로 판단하여 그 서비스를 네트워크 상에 연동하여 시스템 전체를 구축해 나가는 방법론
>   - 여기서 " 서비스 " 는 기능의 독립적 단위.


### 모놀리식 아키텍처 ( Monolithic Architecture )
> - 소프트웨어의 모든 구성요소가 한 프로젝트에 통합되어 있는 서비스.
> - 많은 회사들의 소프트웨어가 레거시 또는 필요로 인해 모놀리식 아키텍처 형태로 구현되어 있음.
> - 소규모의 프로젝트에서는 모놀리식 구현이 간단하며, 유지보수가 편하기 때문에 선호되는편
> - 그러나 일정 규모 이상을 넘거아면 Monolithic 는 많은 한계점에 봉착한다.

### 기존 Monolithic Architecture 의 한계
> - 부분 장애가 서비스 전체의 장애로 확대될 수 있음.
> - 전체 시스템 구조의 파악이 어려움
> - 서비스 변경이 어렵고, 수정 시 영향도( 사이드 이펙트 등 ) 파악이 힘듬
> - 빌드 시간 및 테스트, 배포 시간의 급증
> - 서비스의 특정 부분만 스케일 아웃 (Scale-out)하기 어려움



### MSA 특징
> - 기존 모놀리식의 한계점을 극복하기 위해 등장한 아키텍처 방식
> - MSA는 API를 통해서만 상호작용이 가능하다. 즉, 마이크로 서비스는 서비스의 end-point ( 접근점 )을 API 형태로 외부에 노출하고,
> 실질적인 세부 사항은 모두 추상화한다. 내부의 구현로직, 아키텍처와 프로그래밍 언어, 데이터베이스, 품질 유지 체계와 같은 기술적인 사항들은 서비스 API에 의해 철저하게 가려진다.