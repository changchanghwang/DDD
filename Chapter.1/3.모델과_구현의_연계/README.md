# 모델과 구현의 연계

## Model-Driven-Design

- 코드와 그것의 기반이 되는 모델이 긴밀하게 연결되면 코드에 의미가 부여되고 모델과 코드가 서로 대응하게된다.

> - *소프트웨어 시스템의 일부를 설계할 때는 도메인 모델을 있는 그대로 반영해서 설계와 모델의 대응을 분명하게 하라*

> - *모델로부터 설계와 기본적인 책임 할당에 사용한 용ㅇ어를 도출하라.*
> - *코드를 작성할 때 그러한 용어를 사용하면 코드가 모델을 표현한 것이 되고, 코드의 변경이 곧 모델의 변경으로 이어질 수 있다.*

> - *구현을 모델과 그대로 묶으려면 보통 객체지향 프로그래밍과 같은 모델링 패러다임을 지원하는 소프웨어 개발 도구와 언어가 필요하다.*

## 예제

PCB는 다수의 컴포넌트의 핀을 연결하는 전도체(네트)의 집합으로 볼 수 있다.
PCB 레이아웃 도구는 네트가 서로 교차하거나 방해하지 않게끔 모든 네트의 물리적인 배열을 찾아낸다.
문제는 각각 수천 개의 네트 각각이 고유의 레이아웃 규칙을 지니고 있다는 것이다.
PCB엔지니어들은 자연스럽게 특정 그룹에 속하는 여러 네트가 서로 동일한 규칙을 공유해야하는 것으로 본다. 예를 들어 어떤 네트들은 버스를 구성하기도 한다.

```ts
abstract class AbstractNet {
  private rules:LayoutRule[];

  assignRule(rule:LayoutRule):void {
    this.rules.push(rule);
  }
  getAssignedRules(){
    return this.rules;
  }
}

class Net extends AbstractNet {
  private bus:Bus;

  name: string;
  constructor(name:string){
    super();
    this.name = name;
  }

  assignedRules():LayoutRule[] {
    return this.getAssignedRules().concat(this.bus.getAssignedRules());
  }
}
```

어플리케이션은 아래와 같은 로직이 필요하다
| 서비스 | 책임 |
|--|--|
| 네트 목록 가져오기 | 네트 목록 파일을 읽고 각 항목에 대한 인스턴스를 생성한다.|
| 네트 규칙 내보내기 | 특정 네트 집합에 대해 첨부된 모든 규칙을 규칙 파일에 기록한다.|

그리고 유틸리티도 필요하다

| 클래스 | 책임 |
|--|--|
| 네트 리파지터리 | 이름을 기준으로 네트에 접근하게 해준다 |
| 추론된 버스 팩터리 | 특정 네트 집합에 대해 명명규칙을 이용해 버스를 추론하고 인스턴스를 생성한다.|
| 버스 리파지터리 | 이름을 기준으로 버스에 접근하게 해준다. |


```ts
const nets = await NetListImportService.read(aFile);
await NetRepository.save(nets);
const buses = InferredBusFactory.inferBuses(nets);
BusRepository.save(buses);
```

각 서비스, 리파지터리는 단위 테스트가 가능하다. 그리고 더 중요한 점은 핵심적인 도메인 로직을 테스트할 수 있다는 것이다.

```ts
test('bus rule assignment test',()=>{
  const net0 = new Net("net0");
  const net1 = new Net("net1");
  const bus = new Bus("bus");
  bus.addNet(net0);
  bus.addNet(net1);

  const minWidth4 = NetRule.create(MIN_WIDTH, 4);
  bus.assignRule(minWidth4);

  expect(net0.getRule(MIN_WIDTH)).toEqual(minWidth4);
  expect(net1.getRule(MIN_WIDTH)).toEqual(minWidth4);
})
```

대화식 사용자 인터페이스라면 버스의 목록이 보여주고 사용자가 각 규칙을 할당하게 하거나 하위 호환성을 위해 규칙이 담긴 파일에서 규칙을 읽어올 수도 있다.

- facade를 활용하면 어떤 인터페이스에서든 단순하게 접근할 수 있다.

```ts
public assignBusRule(busName:string, ruleType:String, parameter:number){
  const bus = busRepository.getBus(busName);
  bus.assignRule(NetRule.create(ruleType, parameter));
}
```

