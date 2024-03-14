## 다형성과 동적 바인딩
다형성은 어떤 객체의 특정 메서드를 call하지만, 실제로 어떤 메서드가 실행되는지는 모르고 call한 주체의 타입(클래스)에 따라 실행 방식이 달라짐을 말한다.
즉 runtime 시점에 호출한 클래스에 따라 의존성이 달라짐을 뜻하고, 이는 compile 시점의 의존성과 runtime 시점의 의존성 또한 달라짐을 의미한다.

다형적인 협력에 참여하는 모든 객체는 인터페이스가 동일해야 하며, 그 구현 방법이 각자 다르다.

이처럼 다형성을 제공할 수 있는 이유는 자바의 동적 바인딩 덕분이다. 동적 바인딩은 말그대로 프로그램 실행 시점에 메서드의 호출을 결정하는 과정이다.


## 다형성을 적극 활용한 객체 지향 설계

다형성을 위해서 인터페이스를 적극 활용할 수 있다.
<br>

- 다형적인 협력에 참여하기 위한 인터페이스
```java
public interface OrderReservationService {
    void sendNotification(String reservationId); // 주문예약 알림
    PaymentType getPaymentType(); // 주문예약시, 사용한 결제방식
}
```

- 카드결제인 경우
```java
@RequiredArgsConstructor
public class CashOrderReservationService implements OrderReservationService {

    private final OrderReservationRepository orderReservationRepository;

    @Override
    public void sendNotification(String reservationId) {
        String notificationMessage = String.format(CASH_RESERVATION_NOTIFICATION, reservationId);
        System.out.println(notificationMessage);
    }

    @Override
    public PaymentType getPaymentType() {
        return PaymentType.CASH;
    }
}
```

- 현금결제인 경우
```java
@RequiredArgsConstructor
public class CardOrderReservationService implements OrderReservationService {

    private final OrderReservationRepository orderReservationRepository;

    @Override
    public void sendNotification(String reservationId) {
        String notificationMessage = String.format(CARD_RESERVATION_NOTIFICATION, reservationId);
        System.out.println(notificationMessage);
    }

    @Override
    public PaymentType getPaymentType() {
        return PaymentType.CARD;
    }
}
```

- 동적 바인딩(Runtime 시점에 결제타입에 따라 다른 Service를 반환)을 제공하는 팩토리 클래스
```java
@Component
public class OrderReservationFactory {

    private final Map<PaymentType, OrderReservationService> orderReservationServiceMap;

    @Autowired
    public OrderReservationFactory(Map<String, OrderReservationService> orderReservationServiceMap) {
        this.orderReservationServiceMap = orderReservationServiceMap.values().stream()
            .collect(Collectors.toMap(OrderReservationService::getPaymentType, Function.identity()));
    }

    public OrderReservationService getOrderReservationService(PaymentType paymentType) {
        OrderReservationService orderReservationService = orderReservationServiceMap.get(paymentType);
        if (orderReservationService == null) {
            throw new InvalidPaymentTypeException("지원하지 않는 결제 타입: " + paymentType);
        }
        return orderReservationService;
    }
}
```

- 다형성을 통해, Runtime 시점에 사용하고 싶은 결제타입의 service를 선택한다.
```java
@Transactional
    public String reservation(SaveOrderReservationRequest saveOrderReservationRequest) {
        PaymentType paymentType = PaymentType.from(saveOrderReservationRequest.paymentType());
        OrderReservationService orderReservationService = orderReservationFactory.getOrderReservationService(paymentType);

        OrderReservation orderReservation = ...; // 주문예약 엔티티 생성
        orderReservationService.sendNotification(reservationId);
        return reservationId;
    }
```

_위 코드에서 계좌이체라는 방식이 추가되면?_
