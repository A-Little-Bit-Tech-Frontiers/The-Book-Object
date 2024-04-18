## 상속의 양면성

### 데이터 관점의 상속

부모 클래스에서 정의한 모든 데이터를 자식 클래스의 인스턴스에 자동으로 포함시킬 수 있다라는 관점에서의 상속

### 행동 관점의 상속

데이터뿐만 아니라 부모 클래스에서 정의한 일부 메서드 역시 자동으로 자식 클래스에 포함시킬수 있다라는 관점에서의 상속

**상속의 목적은 코드 재사용이 아닌 타입 계층을 구축하기 위한 것**이다.

## 동적 메서드 탐색과 다형성

동적 메서드 탐색은 self가 가리키는 객체의 클래스에서 시작해서 상속 계층의 역방향으로 이뤄지며 메서드 탐색이 종료되는 순간 self 참조는 자동으로 소멸된다.

### 동적 메서드 탐색의 두 가지 원리

1. **자동적인 메시지 위임**
자식 클래스는 자신이 이해할 수 없는 메시지를 전송받은 경우 상속 계층의 역방향으로 처리를 위임한다.
2. **메서드 탐색을 위한 동적인 문맥 사용**
멧지를 수신했을 때 실제로 어떤 메서드를 실행할지를 결정하는 것은 런타임에 이뤄지며, 메서드를 탐색하는 경로는 self 참조를 이용해서 결정한다.
즉, 동일한 코드라도 self 참조가 가리키는 객체가 무엇인지에 따라 메서드 탐색을 위한 상속 계층의 범위가 동적으로 변한다.

정리하면, **self 참조의 전달은 결과적으로 자식 클래스의 인스턴스와 부모 클래스의 인스턴스 사이에 동일한 실행 문맥을 공유할 수 있게 해준다.**

상속은 동적으로 메서드를 탐색하기 위해 현재의 실행 문맥을 가지고 있는 self 참조를 전달한다. 그리고 이 객체들 사이에서 메시지를 자동으로 전달해준다. 즉, 상속 관계로 연결된 클래스 사이에는 자동적인 메시지 위임이 일어나는 것이다.

사실, 위임은 클래스를 이용한 상속 관계를 객체 사이의 합성 관계로 대체해서 다형성을 구현하는 것이다.
상속 관계 대신에 합성 관계로 대체해서 위임을 통한 다형성을 구현하는 예시를 살펴보자.

### **예시: 동물의 소리 만들기**

- 인터페이스 정의

```java
public interface SoundBehavior {
    void makeSound();
}
```

- 구체적인 행동 구현

```java
public class Bark implements SoundBehavior {
    @Override
    public void makeSound() {
        System.out.println("Woof! Woof!");
    }
}

public class Meow implements SoundBehavior {
    @Override
    public void makeSound() {
        System.out.println("Meow! Meow!");
    }
}

public class Roar implements SoundBehavior {
    @Override
    public void makeSound() {
        System.out.println("Roar!");
    }
}
```

- 동물 클래스에서 행동 클래스를 합성한다.

```java
public class Animal {
    private SoundBehavior soundBehavior;

    public Animal(SoundBehavior soundBehavior) {
        this.soundBehavior = soundBehavior;
    }

    public void performSound() {
        soundBehavior.makeSound();
    }

    public void setSoundBehavior(SoundBehavior soundBehavior) {
        this.soundBehavior = soundBehavior;
    }
}

```

- 다형성 활용

```java
public class Zoo {
    public static void main(String[] args) {
        Animal dog = new Animal(new Bark());
        dog.performSound(); // 출력: Woof! Woof!

        Animal cat = new Animal(new Meow());
        cat.performSound(); // 출력: Meow! Meow!

        Animal lion = new Animal(new Roar());
        lion.performSound(); // 출력: Roar!

        // 동적으로 소리 변경하기
        dog.setSoundBehavior(new Roar());
        dog.performSound(); // 출력: Roar!
    }
}
```

### 위 예시가 만약 상속 관계였다면?

- 인터페이스 혹은 추상 클래스 정의

```java
public abstract class Animal {
    public abstract void makeSound();
}
```

- 각 동물 클래스 구현

```java
public class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Woof! Woof!");
    }
}

public class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Meow! Meow!");
    }
}

public class Lion extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Roar!");
    }
}
```

- 다형성 활용

```java
public class Zoo {
    public static void main(String[] args) {
        Animal dog = new Dog();
        dog.makeSound(); // 출력: Woof! Woof!

        Animal cat = new Cat();
        cat.makeSound(); // 출력: Meow! Meow!

        Animal lion = new Lion();
        lion.makeSound(); // 출력: Roar!
    }
}
```

강한 결합으로 인해 유연성이 전혀 없어진 상속 관계의 모습이다.
위임할 객체를 적절히 할당하고, 그 객체의 인터페이스를 통해 다형성을 제공하는 방법이다.
