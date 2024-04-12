# 10장 상속과 코드 재사용

프로그래머들은 중복 코드를 제거하고 재사용성이 높은 코드를 만들기 위해, DRY 원칙을 지키는데 집중한다.

### 중복 코드를 관리하는 방법과 문제점

1. 전통적으로 코드를 재사용하려면 코드를 복사하여 일부를 수정하여 사용하는 것이다.
2. 타입 변수 사용하기
    1. 낮은 응집도와 높은 결합도를 가지는 문제를 안고 있다.
3. **기반이 되는 클래스를 상속하기**
    1. 기반 클래스를 그대로 상속받기 때문에 **불필요한 인터페이스를 그대로 상속받는다.**
    2. 자식 클래스에서 부모 클래스의 메서드를 호출하게 되면 강하게 결합된다.
    3. 상속을 받아 사용하기 때문에 **캡슐화 또한 깨지게 된다.**
    4. 메서드 오버라이딩시, 내부에서 부모 클래스의 메서드를 사용하게 되면 예상치 못한 결과를 초래한다.

### 결국에는 추상화가 중요하다

기반 클래스 상속은 문제점을 많이 잠재하고 있기 때문에, **차이에 따른 프로그래밍**을 강조하게 된다.

하나의 클래스가 여러 사례로 분리될 경우, 그 차이점을 나타내는 메서드를 추상 클래스, 즉 인터페이스로 끌어올려서 의존하도록 해야 한다.
```tsx
// MMORPG Player 구현 예제

// 플레이어 별도 구현
class Player {
	constructor(Vector3 pos) {
		this.pos = pos
	}
}

class OtherPlayer {
	constructor(Vector3 pos) {
		this.pos = pos
		
		// MMO 에서 플레이어 시야에서 벗어난 다른 플레이어는 렌더링 최적화를 위해 흐릿하게 보일 수 있다.
		// LOD(Level of Detail) 기술 적용을 위한 속성
		this.visibility = false
		this.lodLevel = 0
	}	
}

// 플레이어 상속을 통한 기반 클래스 구현
class Player { ... }
// 중복 코드는 줄었지만, 부모 메서드에 강하게 결합된다.
class OtherPlayer extends Player {
	constructor(Vector3 pos) {
		super(pos)
		this.visibility = false
		this.lodLevel = 0
	}
}

**// 공통적인 부분을 추상화하여 결합도를 낮추기**
abstract class Actor {
	protected pos: Vector3 = null
	protected setPos(Vector3 pos) { this.pos = pos; }
}

class Player extends Actor {
	constructor(Vector3 pos) {
		this.setPos(pos)
	}
}
class OtherPlayer extends Actor {
	private visibility: boolean = false
	private lodLevel: number = 0

	constructor(Vector3 pos) {
		this.setPos(pos)
	}
}

// 인터페이스를 사용할 수도 있다.
interface Actor { ... }
class Player implements Actor { ... }
class OtherPlayer implements Actor { ... }
```