# 상속과 중복 코드

### 상속을 이용해서 중복 코드 제거하기

상속의 기본 아이디어는 이미 존재하는 클래스와 유사한 클래스가 필요하다면 코드를 복사하지 말고, 상속을 이용해 코드를 재사용하라는 것이다. <br>
하지만 상속을 염두에 두고 설계되지 않은 클래스를 상속을 이용해 재사용하는 것은 쉽지 않다. <br>
이는 코드를 이해하기 어렵게 만들고 직관에도 어긋날 수 있다.

상속을 이용해 코드를 재사용하기 위해서는 부모 클래스의 개발자가 세웠던 가정이나 추론 과정을 정확하게 이해해야 한다. <br>
이것은 자식 클래스의 작성자가 부모 클래스의 구현 방법에 대한 정확한 지식을 가져야 한다는 것을 의미한다.

따라서 상속은 결합도를 높인다.










