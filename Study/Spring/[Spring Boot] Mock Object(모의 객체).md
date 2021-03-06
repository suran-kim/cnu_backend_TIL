>_본 포스팅은 프로그래머스 미니 데브 코스를 공부하며 
학습을 기록하기 위한 목적으로 작성된 글입니다._

![](https://velog.velcdn.com/images/suran-kim/post/995de739-392c-4e2f-ba86-ef2b2c8db694/image.png)


# _**test double**_
test double은
SUT와 의존관계에 있는 객체와 상호작용할 수 없을 때
SUT와 상호작용하기 위해 만든 객체이다.
test double에는 `Mock`과 `Stub` 두 가지 종류가 있다.



## Mock

mock, spy이다.
호출에 대한 명세를 한다.
Mock은 어떻게 동작할지 기술이 된 객체이다.

Mock을 이용한 검증에서는 
특정 메소드가 호출되는지 또는 특정 동작이 수행되는지를 확인하면서 검증한다.

**행위**에 집중 하고, **행위 검증**을 사용한다.
(_Mock은 가짜 객체는 아니다._)


### _행위 검증 _

- 메소드의 리턴값으로 판단할 수 없을 경우 특정 동작(특정 메소드가 호출 등)을 수행하는지 확인하는 검증법이다. 


## Stub
가짜 객체, dummy, fake이다.
stub은 실제 동작하는 것처럼 보이게 만드는 객체이다.
예를 들어 SUT와 의존관계에 있는 인터페이스 B가 있다면
인터페이스 B를 구현하는 Stub을 만들어서 제공하는 것이다.

Stub을 이용한 검증에서는
어떤 내용이 반환되게 구현해놓고 stub을 사용한 뒤 
검증하고자 하는 SUT에 대해 **상태**를 확인하면서 검증한다.

### _상태 검증 _
- 메소드가 수행된 뒤, **객체의 상태**를 확인하고 올바르게 동작했는지를 확인하는 검증법이다.