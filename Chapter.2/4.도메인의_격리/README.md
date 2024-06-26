# 도메인의 격리

객체지향 프로그램에서는 종종 사용자 인터페이스와 데이터베이스, 기타 보조적인 성격의 코드를 비즈니스 객체 안에 직접 작성하기 하지만 이는 단기적으로 쉬운 방법이지 추후에 큰 문제를 야기할 수 있다.
DDD에서는 Layered Architecture를 소개하는데 기본적으로 아래의 네가지 계층으로 구성된다.
| 계층 | 설명 |
|--|--|
| 사용자 인터페이스 (표현계층) | 사용자에게 정보를 보여주고 사용자의 명령을 해석하는 일을 책임진다. 다른 컴퓨터 시스템이 외부 행위자가 되곧 한다. |
| 응용 계층 | 소프트웨어가 수행할 작업을 정의하고 표현력 있는 도메인 객체가 문제를 해결하게 한다. 이 계층은 업무상 중요하거나 다른 시스템의 응용 계층과 상호작용하는 데 필요하다. 이 계층은 매우 얇게 유지되는데, 업무 규칙이나 지식이 포함되지 않고 작업을 조정하고 아래에 위치한 계층에 포함된 도메인 객체의 협력자에게 작업을 위임한다. 응용 계층에서는 업무 상황을 반영하는 상태가 없지만 사용자나 프로그램의 작업에 대한 진행상황을 반영하는 상태를 가질 수는 있다.
| 도메인 계층 (모델 계층) | 업무 개념과 업무 상황에 관한 정보, 업무 규칙을 표현하는 일을 책임진다. 이 계층에서는 업무 상황을 반영하는 상태를 제어하고 사용하며, 그와 같은 상태 저장과 관련된 기술적인 세부사항은 인프라스트럭쳐에 위임한다. 이 계층은 업무용 소프트웨어의 핵심이다. |
| 인프라스트럭처 계층 | 상위 게층을 지원하는 일반화된 기술적 기능을 제공한다. 이러한 기능에는 애플리케이션에 대한 메시지 전송, 도메인 영속화, UI에 위젯을 그리는 것 등이 있다. 또한 인프라스트럭처 계층은 아텍처 프레임워크를 통해 네가지 계층에 대한 상호작용 패턴을 지원할 수 있다. |

위에서 말한 layer들로만 구성되어 있지 않을 수 있다. 다만, Model-Driven-Design을 가능케하는 것은 결정적으로 `도메인 계층을 분리하는 데 있다.`

계층 간의 관계를 설정할 때 각계층은 한 방향으로만 둬서 느슨하게 결합된다. 

보통 인프라스트럭처 계층에서는 도메인 계층이 활동하지 않게 한다. 인프라는 도메인 계층의 아래에 있으므로 인프라스트럭처 계층이 보조하는 도메인의 구체적인 지식을 가져서는 안된다.
