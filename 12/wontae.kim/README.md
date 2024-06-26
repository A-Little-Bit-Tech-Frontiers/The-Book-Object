# 12장 다형성

### 다형성

하나의 추상 인터페이스에 대해 정의하고 이에 대해 서로 다른 구현을 연결할 수 있는 능력

### 상속과 위임

- 상속
    - 코드 재사용을 위한 것이 아닌, 다형성을 가능하게 하는 타입 계층을 구축하기 위한 방법
- 위임
    - 자신이 수신한 메시지를 다른 객체에게 동일하게 전달하여 처리를 요청하는 것
- 데이터 관점 상속
    - 자식 클래스의 인스턴스 안에 부모 인스턴스 및 내부에서 정의된 모든 변수를 포함하는 것
- 행동 관점 상속
    - 자식 클래스의 인스턴스 안에 부모 인스턴스가 정의한 메서드를 포함하는 것
- **동적 바인딩**
    
    > 상속에서 동적 바인딩은 런타임에 호출할 메서드를 자신의 인스턴스를 탐색하여 결정하는 것이다. self(this) 객체의 인스턴스에 따라 호출할 메서드를 결정하는데, 현재 객체에 적절한 메서드가 없으면 상위 상속 계층으로 위임하여 호출할 메서드를 결정한다. 이런 객체를 **self 참조 객체**라고 한다.
    > 
    
    자식 클래스는 이해할 수 없는 메시지를 받은 경우 부모 클래스에 자동으로 메시지를 위임하여 처리를 맡긴다. 메시지의 위임을 위해 동적인 문맥을 사용해, 부모 클래스의 메서드를 탐색한다.