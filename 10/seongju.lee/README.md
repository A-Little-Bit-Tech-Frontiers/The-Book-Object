## DRY 원칙

Don’t Repeat Yourself(반복하지 마라)
모든 지식은 시스템 내에서 단일하고, 애매하지 않고, 정말로 믿을만한 표현양식을 가져야 한다.

프로그램의 본질은 비즈니스와 관련된 지식을 코드로 변환하는 것이다. 중복 코드는 이 변경에 필요한 노력을 몇 배로 증가시킨다.

**중복 여부를 판단하는 기준은 변경**이다. 요구사항이 변경됐을 때 두 코드를 함께 수정해야 한다면 이 코드는 중복이다.

즉, 단순히 코드가 중복된다고 해서 그것을 중복이라고 보기는 어렵고, 변경이 같이 된다면 그것이 중복이 아닌 것.

## 상속을 사용할 때, 부모와 강결합이 되어 있다는 주의신호

- 자식 클래스의 메서드 안에서 super 참조를 이용해 부모 클래스의 메서드를 직접 호출할 경우 두 클래스는 강하게 결합된다. super 호출을 제거할 수 있는 방법을 찾아 결합도를 제거하라
- 상속받은 부모 클래스의 메서드가 자식 클래스의 내부 구조에 대한 규칙을 깨트린다. (e.g. Vector를 상속한 Stack)
- 자식 클래스가 부모 클래스의 메서드를 오버라이딩할 경우 부모 클래스가 자신의 메서드를 사용하는 방법에 자식 클래스가 결합될 수 있다.

클래스를 상속하면 결합도로 인해 자식 클래스와 부모 클래스의 구현을 영원히 변경하지 않거나, 자식 클래스와 부모 클래스를 동시에 변경하거나 둘 중 하나를 선택할 수밖에 없다.
