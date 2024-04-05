# 7장 객체 분해

### 하향식 시스템 설계의 문제점

- 메인 함수를 기준으로 계층화 되어 있다.
- 변경에 대비해서 분해를 했지만, 결국 변경이 자주 일어나게 된다.
- 초기에 실행 순서를 고정해서 유연성과 재사용성이 저하된다.
- 결국 상위 계층에 강한 결합도를 가지기 때문에 변경에 취약하다.

### 하향식 기능 분해로 인한 설계를 개선하는 방법

- **모듈**
    - 높은 응집도와 낮은 결합도를 유지하는 시스템 분해 방법
    - 감춰지는 데이터와 관련성이 높은 함수의 집합
    - **장점**
        - 인터페이스와 구현의 관심사를 분리한다.
        - 외부에 변경을 노출하지 않고 내부에만 영향을 미친다.
        - 전역 변수 및 함수를 제거하여 네임스페이스 오염을 방지한다.
    - **단점**
        - 인스턴스(instance) 의 개념을 제공하지 않는다.

- **추상 데이터 타입**
    - **객체기반 프로그래밍** 패러다임
    - 타입을 추상화 한 것으로, 실적으로 추상화 수준을 향상시킨다.
- **클래스**
    - **객체지향 프로그래밍** 패러다임 (상속과 다형성을 지원하기 때문)

```csharp
// 집합 모듈
public class Empolyee {
	public Array<String> m_empolyees = new Array<String>{ "A", "B", "C" }
	public Array<String> m_empolyeesName = new Array<String>{ "길동", "둘리", "도우너" }
}

// 개방-폐쇄 원칙을 준수하는 클래스를 사용한 분해
public interface Empolyee {
	public abstract static Empolyee new();
}
public class SalesEmpolyee : Empolyee {
	public static new() { ... }
}
public class MarketerEmpolyee : Empolyee {
	public static new() { ... }
}

SalesEmpolyee.new("A", "길동")
MarketerEmpolyee.new("B", "둘리")
```