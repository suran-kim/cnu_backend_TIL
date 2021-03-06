
>_본 포스팅은 프로그래머스 미니 데브 코스를 공부하며 
학습을 기록하기 위한 목적으로 작성된 글입니다._

## 1. RDBMS란?
### 💿DBMS부터 알아야겠다.
일반적으로 컴퓨터 시스템에 저장되는 구조화된 정보, 조직화된 데이터의 모음을 `데이터베이스`라고 한다. 보통 데이터베이스와 데이터베이스를 제어하는 데이터베이스 관리시스템, 연결된 애플리케이션을 하나로 묶어 `데이터베이스 시스템(DBMS)` 이라고 부르거나 단축해서  `데이터베이스` 라고 부른다.


데이터베이스의 유형은 `관계형 데이터베이스(RDBMS)`, `NoSQL데이터베이스(비관계형 데이터베이스)`, `데이터 웨어하우스` 등 매우 다양하다.  

(출처: <https://www.oracle.com/kr/database/what-is-database/>)

<br/>

### 💿데이터베이스가 필요한 이유
모든 IT 서비스는 계속 데이터를 만들어내고 그 데이터를 어딘가에 저장해야 한다.
> _예) 카카오톡이라면…_ 
사용자가 회원가입 시 입력한 `사용자 정보 데이터(ID, 패스워드, 전화번호)`
사용자의 친구 리스트를 관리하기 위해 필요한 `친구들의 ID`
사용자의 `채팅기록`

>_예) 쿠팡이라면…_
이용자가 회원가입 시 입력한 `회원 정보 데이터(ID, 패스워드, 전화번호, 주소)`
이용자의 `구매이력`, `상품 검색기록`, `클릭했던 상품 정보`


모든 데이터를 영구적으로 저장할 필요는 없지만
서비스 품질을 높이고 효율적인 운영을 지원할 수 있는 데이터는 계속 기록되어야 한다.

예를 들어, 
어떤 서비스의 데이터베이스에서 
지속적으로 사용자의 `구매이력` 데이터를 저장했다면 
그 서비스에서는 `구매이력` 데이터를 활용한 머신러닝을 통해 
개인에게 특화된 이용자별 추천 서비스를 제공해줄 수도 있을 것이다.
	
    
<br/>


### 💿RDBMS와 데이터 웨어하우스
관계형 데이터베이스의 종류 중 `프로덕션 관계형 데이터베이스(RDBMS)`과 `데이터 웨어하우스`에 대해 알아보자.

- `프로덕션 관계형 데이터베이스(RDBMS)` 
`중요한 것:` 빠른 처리속도
`저장데이터:` 서비스 운영에 필요한 데이터를 저장
`이렇게도 부른다:` OLTP

- `데이터 웨어하우스` 
`중요한 것:` 데이터 처리의 크기
`저장데이터:`사이트 방문 트래픽과 외부데이터(이메일, 마케팅 등) 모든 회사 관련 데이터를 저장
`이렇게도 부른다:` OLSP
<BR/>

> `프로덕션 관계형 데이터베이스(RDBMS)` 는 데이터를 구조화된 테이블들의 집합으로 구성하여 저장하고 관리한다. 프로덕션 관계형 데이터베이스는 보통 `Star schema` 모델을 사용한다. 

> `데이터 웨어하우스` 는 하나의 데이터베이스에 여러 데이터를 저장한다. 데이터 웨어하우스처럼 회사에 필요한 모든 데이터를 한 군데에 저장해두면 큰 이점이 생긴다. 데이터가 분산되어있다면 데이터 수집을 위한 JOIN에 인력과 시간이 소모되고 실수가 발생하기 쉬워진다. 그러나 데이터 웨어하우스는 하나의 데이터베이스에 여러 데이터를 저장하여 그런 수고로움과 실수를 줄인다. 데이터 웨어하우스에 저장된 데이터를 분석해 비즈니스 의사결정과 서비스 최적화에 활용할 수 있다. 데이터 웨어하우스는 보통 `Denormalized schema` 모델을 사용한다.

<BR/>

🎇`추가 : 데이터 웨어하우스 내부의 프로덕션 데이터베이스 복사본`
데이터 웨어하우스에서 내부 데이터를 분석할 때는 보통 프로덕션 데이터베이스를 복사해서 데이터 웨어하우스에 저장한다. 그렇게 해야 프로덕션 데이터베이스에 영향을 주지 않으면서 프로덕션 데이터베이스를 대상으로 데이터를 분석할 수 있기 때문이다. 만약 프로덕션 데이터베이스에 직접 질의를 보내며 내부 데이터를 분석하던 중 실수로 너무 큰 질의를 발생시킨다면 그로 인해 서비스 이용자들의 체감속도에 큰 영향을 줄 수도 있을 것이다. 따라서 내부 데이터 분석을 위한 쿼리는 보통 프로덕션 데이터베이스로 보내지 않고 데이터 웨어하우스로 보내는 것이 기본적인 룰이다.
<BR/>

---



## 2. 백엔드에서의 DBMS

### 💿시스템 구성의 변화

> **_2tier_**
**데스크탑 응용프로그램**에서 사용되는 아키텍쳐
두 개의 컴포넌트(tier)로 이루어진 시스템으로, 
`클라이언트`가 사용자가 사용하는 UI가 된다(Front-end). 
2tier에서는 프론트 엔드인 클라이언트가 직접 비즈니스 로직을 수행한다.
`서버단`은 데이터베이스가 된다(Back-end).

>** _3tier_**
**웹 서비스, 모바일 앱**에서 사용되는 아키텍쳐
세 개의 컴포넌트로 이루어진 시스템으로, 
`프레젠테이션 티어`가 사용자에게 UI를 보여준다. HTML, CSS 등을 사용한 웹페이지를 통해 사용자가 서비스를 사용할 수 있도록 한다(Front-end).
`애플리케이션 티어`라는 미들 티어가 존재한다. 3tier에서는 애플리케이션 티어가 서비스에 맞는 비즈니스 로직을 수행한다(Back-end). _예) 사용자로부터 구매요청이 들어오면 애플리케이션 티어에서 재고가 있는지 확인하고, 재고가 있으면 transaction. 재고가 없으면 프레젠테이션 티어에 재고가 없다는 오류를 송출한다._
`데이터 티어` 는 사용자들의 데이터를 저장하기 위한 데이터 서버이다(Back-end).

<br/>
어떤 구조이건 사용자 데이터를 꼭 저장해야하고, 그 용도로 쓰이는 게 RDBMS이다. 
데이터베이스는 어떤 구조에서든 꼭 필요한 컴포넌트이다.

<BR/>
<BR/>

---

## 3. RDBMS의 구조
### 💿관계형 데이터베이스의 구조
관계형 데이터베이스는 2단계(테이블과 스키마)로 구성된다. RDBMS는 디렉토리, 혹은 폴더와 같은 컨셉을 가진다. `테이블`들을 `데이터베이스(스키마)`라는 폴더 밑에 구성해 테이블 속성에 맞춰 스키마를 만들면 조금 더 편하게 테이블을 관리할 수 있다. 
_예) 개인정보 테이블을 개인정보가 들어간 데이터베이스에 구성해 강력한 접근 제어를 실행한다_
<BR/>

### 💿데이터 모델
`데이터 모델`은 서비스를 운영하는데 필요한 데이터를 어떤 테이블의 집합으로 저장할 것인가를 결정한다.
`Star scheme 모델` 과 `Denormalized schema모델`에 대해 알아보자.

> **_Star scheme 모델_**
Star scheme 모델은 한 엔티티의 정보를 그 테이블에 유지한다. 예를 들어 `매장 테이블`, `기간 테이블`, `제품 테이블`, `영업직원 테이블`이 존재하고 있다면 매출정보를 가져오기 위해 그들을 JOIN하여 `매출정보 테이블`을 만든다. 정보를 중복해서 반복하는 것을 막는 형태이다.
_**장점**_
스토리지 낭비가 덜하다.
업데이트가 쉽다 : 매장정보가 바뀌면 `매장 테이블`만 수정하면 된다.
_**단점**_
JOIN으로 인해 많은 리소스 투입과 속도 저하가 발생한다.

프로덕션 관계형 데이터베이스는 보통 컴퓨터 한 대의 데이터베이스이다. 컴퓨터 한 대가 가질 수 있는 메모리나 디스크 크기에는 한계가 있기 때문에 저장공간을 효율적으로 사용해야할 필요가 있다. 따라서 프로덕션 관계형 데이터베이스에서는 기본적으로 `Star scheme 모델`을 사용한다.




> **_Denormalized schema모델_**
Denormalized schema 모델은 정보를 단위 테이블로 나누어 저장하지 않는다. 별도의 조인이 필요없는 형태로 `매출 테이블` 자체에 직원정보, 제품 정보, 기간 정보 등을 모두 집어넣는다.
_**장점**_
JOIN이 필요 없으므로 빠른 계산이 가능하다.
_**단점**_
업데이트가 상대적으로 복잡하다.

데이터 웨어하우스는 다수의 컴퓨터로 구성되는 데이터베이스이다. 컴퓨터 여러 대로 구성되는 클러스터 형태이기 때문에 프로덕션 관계형 데이터베이스보다 공간적 제약이 덜하고 저장공간을 아끼지 않아도 된다. 따라서 데이터 웨어하우스에서는 `Denormalized schema모델` 을 사용한다. 그러나 `Star scheme모델`을 쓸 수도 있다.

<br/>

---

## 4. SQL이란?
### 💿SQL에 대해서
SQL은 관계형 데이터베이스를 조작하는 프로그래밍 언어이다.
단계적으로 진행하는 다른 프로그래밍 언어와 달리 SQL은 선언하는 형태이다.
SQL은 구조화된 데이터를 다루는 검증된 언어로, 직군에 관계없이 배우면 큰 도움이 되는 기술이다.

<BR/>

### 💿SQL의 장점과 단점
> **_ 장점_**
 구조화된 데이터를 다루는데 최적화 되어있다.<BR/><BR/>
_** 단점**_
비구조화된 데이터를 다루는 것은 제약이 심하다.
_(그래서 보통 RDBMS를 보완해줄 수 있는 또 다른 분산 컴퓨팅 환경을 사용한다.)_

<BR/>

---
## 5. 정리

오늘은 RDBMS의 개념과 구조, 백엔드에서의 DBMS의 중요성, RDBMS의 구조를 결정하는 데이터 모델과 RDBMS에서 쓰이는 언어인 SQL의 특징에 대해 학습해보았다. 
<BR/>

#### 다음 학습 예고

>> _본격적으로 RDBMS를 다루는 SQL 문법에 대해 알아본다.
다음 시간에 계속…_