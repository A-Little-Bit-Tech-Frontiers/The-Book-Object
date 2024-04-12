## 컨텍스트 독립성

앞선 장들에서 컴파일타임의 의존성과 런타임 의존성이 달라야 하는 것을 인지하였다.
클래스는 자신과 협력할 객체의 구체적인 클래스에 대해 알아서는 안되고, 더 많은 정보를 알수록 강결합이 형성된다.
즉, 클래스가 특정한 문맥에 강하게 결합될수록 다른 문맥에서는 사용하기 어려워진다. 이를 컨텍스트 독립성이라고 한다.

예를 들어, 비용 할인 정책이 정해진 영화라고 가정을 한다면, 비용이 아닌 비율 할인 정책이 정해진 영화를 결합하기는 어려울 것이다.
반면 할인 정책이 정해진 영화로 가정한다면 비용 할인, 비율 할인 모두 적용이 가능하다.
이처럼 컨텍스트에 대한 정보가 적으면 적을수록 더 다양한 컨텍스트에서 재사용될 수 있기에 설계가 더 유연해 진다.

## 유연한 설계

컨텍스트에 독립적인 의존성을 가져야 하며, 특정한 컨텍스트에 강하게 결합되어 있지 않아야 한다.
즉, 느슨한 결합도를 가져야 한다.

### 추상화에 의존하라

<img width="1201" alt="image" src="https://github.com/A-Little-Bit-Tech-Frontiers/The-Book-Object/assets/67941526/a7a5423a-e109-40eb-a6c4-d1df9f3dbb16">