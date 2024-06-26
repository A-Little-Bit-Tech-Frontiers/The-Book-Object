## 01. 구현 상속의 문제점

### 불필요한 인터페이스 상속문제

자식 클래스에게는 부적합한 부모 클래스의 오퍼레이션이 상속되기 때문에 자식 클래스 인스턴스의 상태가 불안정해지는 문제이다.

### 메서드 오버라이딩의 오작용 문제

자식 클래스가 부모 클래스의 메서드를 오버라이딩할 때 자식 클래스가 부모 클래스의 메서드 호출 방법에 영향을 받는 문제이다.

### 부모 클래스와 자식 클래스의 동시 수정 문제

부모 클래스와 자식 클래스 사이의 개념적인 결합으로 인해 부모 클래스를 변경할 때 자식 클래스도 함께 변경해야 하는 문제이다.

상속의 근본적인 문제는 자식 클래스가 부모 클래스의 구현에 강하게 결합되도록 강요하는 것이다.
컴파일타임에 결정된 부모-자식 관계는 실행 시점까지 그대로 가져가기 때문에, 유연하지 못하다.

만약, 부모-자식 클래스의 다양한 조합이 필요한 상황이라면 해결할 수 있는 방법이라곤 조합의 수만큼 새로운 클래스를 추가하는 것이다. 클래스 폭발이다.

## 02. 객체 합성이 클래스 구현 상속보다 더 좋은 이유

상속이 구현을 재사용하는 데 비해 합성은 객체의 인터페이스를 재사용한다.

상속에서 다양한 조합을 위해서, 새로운 클래스를 만들어야 했다.
합성에서는 원하는 클래스를 주입해주기만 하면 조합이 끝난다. 훨씬 예측 가능하고 일관성 있는 방식이다.

상속에서 새로운 클래스가 추가되거나 수정된다면 부모 클래스의 강결합으로 인해 동시에 변경되어야 했다.
합성은 부모의 퍼블릭 인터페이스만 재사용하기에, 구현 클래스만 추가해서 원하는 방식으로 조합해주면 된다. 즉, 계층구조가 없는 타입 프레임워크를 만들 수 있다.

## 03. 믹스인

객체를 생성할 때, 코드 일부를 클래스 안에 섞어 넣어 재사용하는 기법이다. 컴파일 시점에 필요한 코드조각을 조합하는 재사용 방법이다.

근데, 이 재사용이라는게 좀 다른 느낌이다.

이펙티브 자바 - 아이템 20 내용 발췌

“*믹스인이란 클래스가 구현할 수 있는 타입으로, 믹스인을 구현한 클래스에 원래의 ‘주된 타입’ 외에도 특정 선택적 행위를 제공한다고 선언하는 효과를 준다. 예컨대 Comparable은 자신을 구현한 클래스의 인스턴스끼리는 순서를 정할 수 있다고 선언하는 믹스인 인터페이스이다. 이처럼 대상 타입의 주된 기능에 선택적 기능을 혼합한다고 해서 믹스인이라 부른다.*”

위 내용을 빌려 설명을 이어가자면 아래와 같다.
Comparable 인터페이스에 제공하는 메서드는 다른 여러 클래스 안에 섞여(e.g. 기본 자료형 래퍼 클래스, BigDecimal 등) 자연스러운 순서로 정렬해준다고 선언하는 것이다.
