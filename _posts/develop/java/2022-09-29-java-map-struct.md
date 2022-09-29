---
title: "[Java] Map Struct(Entity, DTO 변환)"
category: Java
---

# Map Struct
Entity ↔ DTO 변환을 용이하게 해주는 라이브러리

속도가 매우 빠르다.


## Dependency

```java
dependencies {
		implementation 'org.mapstruct:mapstruct:1.4.2.Final'
    annotationProcessor "org.mapstruct:mapstruct-processor:1.4.2.Final"
    annotationProcessor 'org.projectlombok:lombok-mapstruct-binding:0.2.0'

		implementation 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok:1.18.22'
}
```

## order

```java
@Getter
@Builder
public class Order {

    private Long id;

    private String name;

    private String product;

    private Integer price;

    private String address;

    private LocalDateTime orderedTime;

}
```

## orderDto

```java
@Builder
@Getter
class OrderDto {

    private String name;

    private String product;

    private Integer price;

    private String address;

    private String img;

    private LocalDateTime orderedTime;
}
```

## OrderMapper

```java
@Mapper(componentModel = "spring")
public interface OrderMapper {

    //  Instance가 OrderMapper를 상속받아서 OrderMapperImpl로 구현하게 됨.
    //  OrderMapper를 기반으로 메서드와 실제 로직들이 생성될 것이라고 명시하는 것
    OrderMapper INSTANCE = Mappers.getMapper(OrderMapper.class);

    // orderDto에는 id가 없다.
    @Mapping(target = "id", constant = "0L")
    Order orderDtoToEntity(OrderDto orderDto);

    // order에는 img가 없다.
    @Mapping(target = "img", expression = "java(order.getProduct() + \".jpg\")")
    OrderDto orderToDto(Order order);
}
```

## OrderService

```java
@Component
@RequiredArgsConstructor
public class OrderService {

    private final OrderMapper orderMapper;

    @EventListener(ApplicationStartedEvent.class)
    public List<OrderDto> getOrders() {
        System.out.println("===== MapStruct =====");

        List<Order> orders = Arrays.asList(new Order(1L, "test1", "콜라", 1000, "111-1", LocalDateTime.now()),
            new Order(2L, "test2", "빵", 1000, "111-1", LocalDateTime.now()),
            new Order(3L, "test3", "커피", 1000, "111-1", LocalDateTime.now()));

        List<OrderDto> orderDtos = orders.stream().map(orderMapper::orderToDto).toList();

        orderDtos.forEach(o -> System.out.println(o.getProduct()));
				
				// ===== MapStruct =====
				// 콜라
				// 빵
				// 커피

        return orderDtos;
    }

}
```

## Test

```java
class OrderMapperTest {

    @Test
    @DisplayName("DTO에서 Entity로 변환하는 테스트")
    void test_dto_to_entity() {
        //given
        final OrderDto orderDto = OrderDto.builder()
            .name("테스트")
            .product("음료수")
            .price(1000)
            .address("Seoul")
            .orderedTime(LocalDateTime.now())
            .build();

        // when
        final Order order = OrderMapper.INSTANCE.orderDtoToEntity(orderDto);

        // then
        assertNotNull(order);
        assertThat(order.getName()).isEqualTo("테스트");
    }

    @Test
    @DisplayName("Entity에서 DTO로 변환하는 테스트")
    void test_entity_to_dto() {
        //given
        final Order order = Order.builder()
            .id(1L)
            .name("테스트")
            .product("음료수")
            .price(1000)
            .address("Seoul")
            .orderedTime(LocalDateTime.now())
            .build();

        //when
        final OrderDto orderDto = OrderMapper.INSTANCE.orderToDto(order);

        //then
        assertNotNull(orderDto);
        assertThat(orderDto.getName()).isEqualTo("테스트");

    }
}
```