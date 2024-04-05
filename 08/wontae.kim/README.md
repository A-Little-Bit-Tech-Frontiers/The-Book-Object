# 8장 의존성 관리하기

의존성을 최소로 유지하기 위해서는 **컨텍스트가 독립적**이어야 한다. 컨텍스트가 특정 객체에 의존하게 되고 그것을 또 다른 객체가 의존하게 될 때, **의존성 전이**가 일어나게 된다.

- **컴파일타임 의존성**
    - 소스 코드가 컴파일 될 때 필요한 의존성. 개발 과정 중에 필요하며 특히 프로그램 빌드시에 필요하다.
- **런타임 의존성**
    - 프로그램의 실행 시점에 의존한다. 어노테이션을 통해 런타임 타겟으로 의존하게 하는 경우 런타임 의존성에 해당한다.

### 강한 결합도를 가진 예제 (직접 의존)

```csharp
public class Skill {
	**// Player 에게 강하게 의존하고 있다.**
	private readonly Player m_player;
	private IEnumerator m_coroutine
	
	**// 스킬은 플레이어, 몬스터 모두 공통적으로 루틴이 적용되어야 하기 때문에 재사용성이 중요하다.**
	public Skill(Player player) {
		m_player = player
	}
	
	public void Enter() { 
		m_coroutine = m_player.StartCoroutine(this.UpdateCoroutine, () => this.Leave())
	}

	public void Leave() { ... }
	
	public void IEnumerater UpdateCoroutine() {
		GameObject playerObject = m_player.gameObject;
		
		// ...스킬 발동 상태 중 프레임 업데이트 로직...
	}
}
```

### 약한(느슨한) 결합도를 가진 예제 (간접 의존)

```csharp
public class Skill {
	**// 행동자로 추상화해서 상속받도록 유도한다.**
	private readonly BehaviourObject m_behaviourObject;
	private IEnumerator m_coroutine;
	
	**// 이제 모든 행동자가 Skill 을 확장해서 사용할 수 있다.**
	public Skill(BehaviourObject behaviourObject) {
		m_behaviourObject = behaviourObject
	}
	
	public void Enter() { 
		m_coroutine = m_behaviourObject.StartCoroutine(this.UpdateCoroutine, () => this.Leave())
	}

	public void Leave() { ... }
	
	public void IEnumerater UpdateCoroutine() {
		GameObject behaviourGameObject = m_behaviourObject.gameObject;
		
		// ...스킬 발동 상태 중 프레임 업데이트 로직...
	}
}

public class Player : BehaviourObject {
	...
}

public class SPlayerAttack : Skill {
	...
}

Player player = new Player();
SPlayerAttack playerAttack = new SPlayerAttack(player);
```