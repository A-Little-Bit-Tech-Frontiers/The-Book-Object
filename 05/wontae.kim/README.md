# 5장 책임 할당하기

객체지향 설계를 시작할 때는 먼저 행동을 결정하고, 협력의 문맥에서 책임을 할당해야 한다.

5장에서는 GRASP 패턴으로 보는 원칙을 통해 객체지향 설계의 원칙을 다시금 강조한다.

### GRASP

‘일반적인 책임 할당을 위한 소프트웨어 패턴’ 으로, 객체에 책임을 할당할 때 지켜야 하는 9가지 원칙이다.

- 정보 전문가 ( Information Expert )
    - 책임을 수행할 정보를 알고있는 객체에게 책임을 할당하는 패턴
- 제작자 ( Creator )
    - 생성되는 객체를 가장 잘 알고 있거나 긴밀하게 협력하여 결합될 객체를 생성하는 주체
- 컨트롤러 ( Controller )
- 간접 참조 ( indirection )
- **낮은 결합 ( Low coupling )**
- **높은 응집도 ( High cohesion )**
- **다형성 ( Polymorphism )**
- **보호된 변형 ( Protected variations )**
- 순수 제작 ( Pure fabrication )

### GRASP 원칙을 만족하는 Player 구현

```csharp
// PlayerState 를 가장 잘 아는 객체, 즉 **<Information Expert>** 는 Player 이다.
public class Player
{
	// PlayerState 는 내부에서 캡슐화 되어 ****외부 충돌, 피격 발생 등의 변경요인이 발생해도
	// 인터페이스를 통해( PushState() ) 내부에서 처리되므로 **<Protected variations>** 를 만족한다.
	private Stack<PlayerState> m_stateStack = new Stack<PlayerState>();

	// Player 가 PlayerState 를 사용하기 때문에 **<Creator>** 역할을 한다.
	public PlayerState PushState<T>(string state) {
		m_stateStack.push(MakeState<T>(state))
	}
}

// **<Polymorphism>** (다형성)을 구현하기 위한 추상 클래스 (인터페이스)
public abstract class PlayerState {
	...
}

public static T MakeState<T>(string name)
{
    return (T) new PlayerState(name);
}
```

위와 같이 원칙을 만족해도, 나중에는 비즈니스 요구사항에 따른 ‘리팩토링’ 으로 인한 코드의 변경이 일어난다. 하지만 위의 원칙을 지키려고 노력했다면 리팩토링으로 인한 내부 변경을 쉽게 수행할 수 있을 것이다.