# 4장 설계품질과 트레이드오프

설계는 변경을 위한 구조를 만들지만 변경은 어떻게든 비용이 발생하기 때문에, 합리적인 비용 안에서 변경을 수용할 수 있는 구조를 만드는 설계를 해야 한다.

### 데이터 중심 설계 vs 책임 중심 설계

데이터 중심 설계는 객체의 ‘상태’ 를 조작하기 위한 방법을 제공하는 것에 초점이 맞춰져 있다. 그리고 책임 중심 설계는 행동을 결정하는데 초점을 맞춘다. 상태의 변경에 초점이 맞춰지면 각각의 상태를 변경하기 위한 메서드가 지속적으로 늘어날 수 밖에 없다. 하지만 책임 중심 설계를 하게 되면 외부로부터 호출되는 행동만 정의하고, 행동을 위한 내부적인 상태에 대해서는 외부에서 신경을 쓰지 않기 때문에 더욱 유연하게 변경을 할 수 있다.

즉, 책임 중심 설계는 **높은 응집도**를 가진 행동을 정의하여 외부와의 **결합도를 낮추고** 변경을 쉽게 하는 것이 목표이며, 객체지향 설계의 이상적인 효과를 가져온다고 볼 수 있다.

### 데이터 중심 설계 예시 (C#)

```csharp
public class Player : MonoBehaviour {
	private readonly PlayerScript _script; // Player Object Component
	private readonly PushdownAutomata _automata; // Stack FSM

	// 접근자, 수정자 정의 (GET, SET Operator)
	public Script { get { return _script; } set { _script = value } }
	public Automata { get { return _automata; } set { _automata = value } }

	// 메인 스레드 렌더링 전 수행 (객체 constructor 역할)
	public Awake() {
		_script = gameObject.GetComponent<PlayerScript>();
		_automata = new PushdownAutomata();

		**/*
			아래 코드는 행동을 파악하기 어렵고 수정자들이 많아 조합에 취약하다.
		  "스테이지 진입시 일어날 일" 들을 추측해서 설계했기 때문에,
		  외부에 수정자를 과도하게 노출해서 응집도가 낮아지고 결과적으로 결합도가 높아진다.
		*/**
		SetState(PlayerState.Ground);
		SetPlayerPosition(new Vector3(5,10,0));
		SetPlayerSkill(PlayerSkill.Attack);
	}

	public SetState(PlayerState state) {
		Automata.PushState(state);
	}

	public SetPlayerPosition(Vector3 pos) {
		gameObject.transform.position = pos;
	}

	public SetPlayerSkill(PlayerSkill skill) {
		_script.SetSkill(skill);
	}
}
```

### 책임 중심 설계 예시 (C#)

```csharp
public class Player : MonoBehaviour {
	private readonly PlayerScript _script; // Player Object Component
	private readonly PushdownAutomata _automata; // Stack FSM

	// ~~접근자, 수정자 정의 (GET, SET Operator)~~
	// public Script { get { return _script; } set { _script = value } }
	// public Automata { get { return _automata; } set { _automata = value } }

	// 메인 스레드 렌더링 전 수행 (객체 constructor 역할)
	public Awake() {
		_script = gameObject.GetComponent<PlayerScript>();
		_automata = new PushdownAutomata();

		EnterStage(Stage.AngelIslandZone);
	}

	**/*
		플레이어 스테이지 진입 시 수행할 "행동"
	  행동 하나가 <높은 응집도> 를 가지고 있어 변경에 강하다.
		수정자에 의존하지 않음 으로써 캡슐화를 어느정도 잘 지키고 있다.
	*/**
	public EnterStage(Stage stage) {
		Automata.PushState(PlayerState.Ground);
		gameObject.transform.position = stage.StartPosition;
		_script.SetSkill(PlayerSkill.Attack);
	}

	// public SetState(PlayerState state) {
	// 		Automata.PushState(state);
	// 	}

	// 	public SetPlayerPosition(Vector3 pos) {
	// 		gameObject.transform.position = pos;
	// 	}

	// 	public SetPlayerSkill(PlayerSkill skill) {
	// 		_script.SetSkill(skill);
	// 	}
}

public abstract Stage {
	public abstract StartPosition { get; }
	
	public static Stage AngelIslandZone = new AngelIslandZone();
}

public class AngelIslandZone : Stage {
	public override StartPosition { get { return new Vector3(5, 10, 0); } }
}
```