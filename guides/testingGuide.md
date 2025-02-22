Как писать разные тесты?
1️⃣ Unit-тесты (быстрые, без БД, тестируют логику)
📌 Папка: test/java/com/example/project/unit/
📌 Фреймворк: JUnit, Mockito

Пример unit-теста для OrderService:

   ```
@ExtendWith(MockitoExtension.class)
class OrderServiceTest {

    @Mock private OrderRepository orderRepository;
    @InjectMocks private OrderService orderService;

    @Test
    void shouldCalculateTotalPrice() {
        Order order = new Order(1, 100, 2);
        when(orderRepository.findById(1)).thenReturn(Optional.of(order));

        int total = orderService.calculateTotalPrice(1);
        
        assertEquals(200, total);
    }
}
   ```

2️⃣ Интеграционные тесты (с БД, проверяют слой DAO/Service)
📌 Папка: test/java/com/example/project/integration/
📌 Фреймворк: Spring Boot Test, Testcontainers

Пример теста с H2/PostgreSQL (Spring Boot Test):

```
@SpringBootTest
@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
class OrderRepositoryTest {

    @Autowired private OrderRepository orderRepository;

    @Test
    void shouldSaveAndFindOrder() {
        Order order = new Order(1, 500, 1);
        orderRepository.save(order);
        
        Optional<Order> foundOrder = orderRepository.findById(1);
        assertTrue(foundOrder.isPresent());
        assertEquals(500, foundOrder.get().getAmount());
    }
}
```

3️⃣ API-тесты (RestAssured, WebTestClient, MockMvc)
📌 Папка: test/java/com/example/project/api/
📌 Фреймворк: RestAssured, MockMvc, WebTestClient

Пример API-теста контроллера:

@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@AutoConfigureMockMvc
class OrderControllerTest {

    @Autowired private MockMvc mockMvc;

    @Test
    void shouldCreateOrder() throws Exception {
        mockMvc.perform(post("/orders")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{ \"amount\": 500, \"quantity\": 1 }"))
                .andExpect(status().isCreated())
                .andExpect(jsonPath("$.amount").value(500));
    }
}


4️⃣ Contract-тесты (Spring Cloud Contract, Pact) (не знаю зачем они)
📌 Папка: test/java/com/example/project/contract/
📌 Фреймворк: Spring Cloud Contract

Пример контракт-теста (contract/order-service.groovy):

Contract.make {
    request {
        method 'POST'
        url '/orders'
        body([
            amount: 500,
            quantity: 1
        ])
    }
    response {
        status 201
        body([
            id: 1,
            amount: 500
        ])
    }
}
Этот контракт будет генерировать тесты для проверки API.

5️⃣ End-to-End (E2E) тесты (все сервисы) (это не моя забота)
📌 Папка: test/java/com/example/project/e2e/
📌 Фреймворк: Postman (Newman), Selenium, Cucumber

Пример теста с Postman + Newman:

{
  "info": { "name": "Order API Test" },
  "item": [
    {
      "name": "Create Order",
      "request": {
        "method": "POST",
        "url": "{{baseUrl}}/orders",
        "body": { "mode": "raw", "raw": "{ \"amount\": 500 }" }
      },
      "event": [
        {
          "listen": "test",
          "script": {
            "exec": [
              "pm.response.to.have.status(201);",
              "pm.expect(pm.response.json().amount).to.eql(500);"
            ]
          }
        }
      ]
    }
  ]
}