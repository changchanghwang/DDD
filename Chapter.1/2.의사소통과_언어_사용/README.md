# 의사소통과 언어 사용

## Ubiquitous Language (보편 언어)

내 생각에 DDD가 좋은 이유중 하나인 것 같다.

소프트웨어 개발자는 보통 혼자서 일하지 않는다. 도메인 전문가 또는 기획자, 디자이너 등 여러 직군의 사람들과 협업을 하게 된다.
개발자는 시스템을 서술적이고 기능적인 용어로 이해하고, 토론할 수 있어도 전문가들의 언어에 담긴 의미를 알수 없다.
반면에 도메인 전문가들은 해당 도메인의 전문용어는 매우 잘 알고 있지만 기술적인 용어를 이해하는데에는 한계가 있을 것이다.

언어적으로 어긋남에도 불구하고 원하는바를 자신의 언어로만 말한다면, 요구사항을 이해하는 개발자는 모호한 수준에 머문다. 
요구사항을 이해하는 개발자가 있다고 하더라도 다른 개발자에게 정보를 전달하는데에 어려움이 있을 수 있기 때문에 정보흐름의 병목지점이 되고 심지어 변역도 정확하지않을 수 있다.

이런 경우 의사소통을 점점 무디게 만들고 지식탐구를 빈약하게 만들어 프로젝트를 위험에 빠뜨릴 수 있다.

책에서는 그래서 아래와 같이 말한다.

> - *모델을 언어의 근간으로 사용하라.*
> - *팀 내 모든 의사소통과 코드에서 해당 언어를 끊임없이 적용하는 데 전념하라*
> - *다이어그램과 문서에서, 그리고 특히 말할 때 동일한 언어를 사용하라.*

> - *대안 모델을 반영하는 대안이 되는 표현을 시도해 봄으로써 어려움을 해소하라*
> - *그런 다음 새로운 모델에 맞게끔 클래스, 메서드, 모듈의 이름을 다시 지으면서 코드를 리팩터링하라.*
> - *일상적으로 쓰는 단어의 의미에 동의를 이끌어내는 것과 같은 방식으로 대화할 때 쓰는 용어의 혼란도 해결하라.*

> - *UBIQUITOUS LANGUAGE의 변화가 곧 모델의 변화라는 것을 인식하라.*

> - *도메인 전문가는 도메인을 이해하는 데 부자연스럽고 부정확한 용어나 구조에 대해 반대 의사를 표명해야 한다.*
> - *개발자는 설계를 어렵게 만드는 모호함과 불일치를 찾아내는 데 촉각을 곤두세워야한다.*

내가 중요하게 생각한 곳은 이것이다.
`같은 도메인에서 쓰이는 대체 어휘가 포함돼 있어서는 안된다.`

또한 여기서 문서의 중요성에 대해서 강조한다.
대표적으로 UML과 글로 쓴 설계문서를 들었는데 UML 다이어그램을 통해 아이디어 골자들을 나타내고 이런 보조적인 역할을 수행하는 다이어그램을 통해 핵심에만 주의를 기울일 수 있다.

