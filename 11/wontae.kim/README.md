# 11장 합성과 유연한 설계

상속이 is-a 관계를 나타내는 것과 달리 합성은 has-a 관계를 나타낸다. 상속의 문제점은 다음과 같다.

- 불필요한 인터페이스 상속 문제
- 메서드 오버라이딩의 오작동 문제
    - 부모 메서드 (super 클래스 메서드) 에 의존하는 문제 포함
- 부모 및 자식 클래스의 동시적 수정 문제
    - 동일한 메서드 호출을 위임하는 메서드 포워딩 (method forwarding) 문제

이같은 문제들로 인해 상속의 남용으로 **클래스 폭발 혹은 조합의 폭발** 문제가 발생한다. 상속에 의존하게 되면 부모 클래스에 강하게 결합되어 용도에 따라 분리하기 위해 필요 이상으로 많은 클래스를 추가해야 한다.

이를 방지하기 위해서는 결국 자식 클래스가 부모 클래스의 구현으로 인해 많은 작업을 해야 하는 문제를 해결해야 한다. 그러기 위해서는 **부모 클래스의 퍼블릭 인터페이스만을 사용하게 하여 구현에 직접적으로 관여하지 말아야 한다.**

### 합성으로 변경하기

```csharp
//region 상속 구현 영역
public abstract class Item {
	public abstract Name { get; }
}

public class Weapon : Item {
	private readonly string name;
	public Name { get { return name; } }
	
	public Weapon(string name) {
		name = name;
	}
}

// 오라(Ora)를 가지는 무기 확장
public class OraWeapon : Weapon {
	...
}

// 효과를 추가로 가지는 오라 무기 확장
public class EffectOraWeapon : OraWeapon {
	...
	public void AreaEffect(List<Collider> colliders) {
		colliders.forEach((Collider collider) -> {
			// 오러가 발생하면 다른 객체를 물리적 힘을 가해 튕겨냄
			collider.rigidBody.AddForce((this.transform.dir - this.player.transform.dir) * Rigidbody.Force.Impulse)
		})	
	}
}
//endregion

//region 합성 구현 영역
public abstract PlayerState { ... }
public class SkillState : PlayerState {

	// 합성으로 Weapon 을 사용할 때와 달리,
	// 관심사를 분리하여 필요에 따라 퍼블릭 인터페이스로 조합하여 사용되는 합성 클래스 하나로 해결할 수 있게 되었다.
	private readonly OraWeapon m_oraWeapon;
	private readonly InstanceEffect m_effect;

	public SkillState(
		OraWeapon oraWeapon,
		InstanceEffect effect
	) {
		m_oraWeapon = oraWeapon;
		m_effect = effect;
	}

	// 상태 진입
	public void Enter(Player player) {
		effect.start(player);
	}
	
	// 주 스레드의 렌더링 루프에서 종료 시 까지 지속적으로 효과 실행
	public void Update(Player player) {
		if (effect.IsRunning()) {
			effect.update(player);
		}
	}
	
	public void Leave(Player player) {
		effect.stop(player);
	}
}
// endregion
```

### 믹스인(Mixin)

쌓을 수 있는 변경(stackable modification)이라는 특징을 지니는 **추상 서브클래스** 성격을 띄는 방법이다.

**Scala 와 Rust 의 trait 개념**을 믹스인의 대표적인 예시로 들 수 있다. 직접적으로 상속을 한다는 개념은 아니지만, 슈퍼클래스를 명시해야하는 점에서 상속과 문맥적으로는 비슷하다.

Scala 에서 trait 는 인스턴스화 될 때, 클래스와 trait 를 선형화해서 어떤 메서드를 호출할 지 결정한다. 이를 위해 클래스와 with 구문으로 trait 를 연결해서 순서를 정해서 호출하도록 한다.

믹스인을 사용하면 추가적인 기능을 기본 클래스의 자식 클래스처럼 만들어 변경에 유연한 설계를 할 수 있다.