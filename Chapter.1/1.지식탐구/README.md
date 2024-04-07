# 지식 탐구

앞에 내용들은 조금 PCB에 대한 상세한 내용이 있어서 정리해보자면

> 해당 도메인의 전문가와 다이어그램을 그리며 해당 도메인 지식에대해서 정리하는 시간을 갖고 하는것이 좋다

라는 뜻이었던 것 같다.

### 지속적인 학습

소프트웨어를 작성하기 시작할 때는 충분히 알지 못한 상태에서 시작하더라도 계속해서 학습하며 지식이 생기면서 더 나은 제품을 만든다.

### 예제

- 선박화물의 운송 예약을 위한 간단한 도메인 모델로 시작

```ts
public makeBooking(cargo:Cargo, voyage:Voyage){
  const confirmation = orderConfirmationSequence.next();
  voyage.addCargo(cargo, confirmation);
  return confirmation;
}
```

예약 어플리케이션의 책임이 각 화물을 하나의 운항과 연관관계를 맺고 기록, 관리하는 것이라고 했을 때 위와 같은 메서드가 만들어진다.

하지만 화물 운송서비스의 경우 항상 마지막에 취소하는 경우가 있기 때문에 최대치보다 예약을 더 받는 것이 관행이다.

그렇다면 아래와 같은 요구사항이 생긴다.

> 10% 초과 예약 허용.

그렇다면 아래처럼 조건이 추가된다.
```ts
public makeBooking(cargo:Cargo, voyage:Voyage):number{
  const maxBooking = voyage.capacity * 1.1;
  if((voyage.bookedCargoSize + cargo.size) > maxBooking){
    return -1
  }

  const confirmation = orderConfirmationSequence.next();
  voyage.addCargo(cargo, confirmation);
  return confirmation;
}
```

> 이렇게 되면 이제 업무 규칙이 메서드의 보호절로 감춰진다. (추후 4장 layered architecture에서 이 규칙을 도메인 객체로 옮길 것이다.)

하지만 현재 코드에서는 다음과 같은 문제가 있다.
1. 코드가 작성된 대로라면 개발자의 도움이 있더라도 업무 전문가가 이 코드를 읽고 규칙을 검증하지 못한다.
2. 해당 업무에 종사하지 않고 기술적인 측면만 담당한다면 코드와 요구사항을 결부시키기 어려울 것이다. (왜 maxBooking이 10%인지도 모르고, 저 코드가 무엇을 의미하는지 모를 수 있다.)

규칙이 복잡해지면 복잡해질 수록 힘들 것이다.

다음과 같이 설계를 변경하여 개선해볼 수 있다.
- 위에서 10%의 초과 예약을 허용하는것은 *정책*이다.

초과예약정책(OverbookingPolicy)이라는 객체를 만들어 이 객체가 이 정책을 관리하도록 하자.

```ts
public makeBooking(cargo:Cargo, voyage:Voyage):number{
  if(!overbookingPolicy.isAllowed(voyage, cargo)){
    return -1;
  }
  const confirmation = orderConfirmationSequence.next();
  voyage.addCargo(cargo, confirmation);
  return confirmation;
}

// OverbookingPolicy.ts
public isAllowed(cargo:Cargo, voyage:Voyage):boolean{
  const maxBooking = voyage.capacity * 1.1;
  return (voyage.bookedCargoSize + cargo.size) <= maxBooking;
}
```

이렇게하면 초과예약이 별개의 정책이라는 사실을 알게되고 명시적으로 구현이 드러나며 다른 구현과 분리된다.
