# Java 코드 컨벤션 (Code Convention)

Google Java Style Guide 및 Oracle 공식 규칙 기반 Java 코딩 규칙입니다.

---

## 1. 명명 규칙

| 대상 | 스타일 | 예시 |
|------|--------|------|
| 클래스/인터페이스 | PascalCase | `CustomerService`, `Runnable` |
| 메서드/변수 | camelCase | `getUserById()`, `userName` |
| 상수 | UPPER_SNAKE_CASE | `MAX_SIZE`, `DEFAULT_TIMEOUT` |
| 패키지 | lowercase | `com.example.myapp` |
| 제네릭 타입 | 단일 대문자 | `T`, `E`, `K`, `V` |

```java
package com.example.customer;

public class CustomerService {
    // 상수
    private static final int MAX_RETRY_COUNT = 3;
    
    // 인스턴스 변수
    private final CustomerRepository repository;
    private int connectionCount;
    
    // 생성자
    public CustomerService(CustomerRepository repository) {
        this.repository = repository;
    }
    
    // 메서드
    public Customer getCustomerById(int customerId) {
        return repository.findById(customerId);
    }
}
```

---

## 2. 들여쓰기 및 공백

- **들여쓰기**: 4 spaces (또는 2 spaces)
- **줄 길이**: 최대 100자
- **중괄호**: K&R 스타일 (같은 줄에 여는 중괄호)

```java
// ✅ Good: K&R 스타일
public void processOrder(Order order) {
    if (order.isValid()) {
        // 처리 로직
    } else {
        // 에러 처리
    }
}

// 단일 문장도 중괄호 사용
if (condition) {
    doSomething();
}
```

---

## 3. Import 규칙

```java
// 1. java.* 패키지
import java.util.List;
import java.util.Map;

// 2. javax.* 패키지
import javax.annotation.Nullable;

// 3. 서드파티 라이브러리
import org.springframework.stereotype.Service;
import com.google.common.collect.ImmutableList;

// 4. 프로젝트 내부
import com.example.myapp.model.Customer;

// ❌ 와일드카드 import 금지
import java.util.*;
```

---

## 4. 클래스 구조

```java
public class OrderService {
    // 1. 상수 (static final)
    private static final Logger LOGGER = LoggerFactory.getLogger(OrderService.class);
    
    // 2. static 필드
    private static int instanceCount = 0;
    
    // 3. 인스턴스 필드
    private final OrderRepository repository;
    private final NotificationService notificationService;
    
    // 4. 생성자
    public OrderService(OrderRepository repository, NotificationService notificationService) {
        this.repository = repository;
        this.notificationService = notificationService;
    }
    
    // 5. public 메서드
    public Order createOrder(OrderRequest request) {
        validateRequest(request);
        Order order = buildOrder(request);
        return repository.save(order);
    }
    
    // 6. private 메서드
    private void validateRequest(OrderRequest request) {
        if (request == null) {
            throw new IllegalArgumentException("Request cannot be null");
        }
    }
}
```

---

## 5. Javadoc 주석

```java
/**
 * 고객 정보를 관리하는 서비스 클래스.
 *
 * <p>이 클래스는 고객 생성, 조회, 수정 기능을 제공합니다.
 *
 * @author John Doe
 * @version 1.0
 * @since 2025-01-01
 */
public class CustomerService {
    
    /**
     * 고객 ID로 고객 정보를 조회합니다.
     *
     * @param customerId 조회할 고객의 고유 ID (양수여야 함)
     * @return 고객 정보 객체
     * @throws IllegalArgumentException customerId가 0 이하인 경우
     * @throws CustomerNotFoundException 고객을 찾을 수 없는 경우
     */
    public Customer getCustomerById(int customerId) {
        if (customerId <= 0) {
            throw new IllegalArgumentException("Customer ID must be positive");
        }
        return repository.findById(customerId)
            .orElseThrow(() -> new CustomerNotFoundException(customerId));
    }
}
```

---

## 6. 예외 처리

```java
// 구체적인 예외부터 처리
try {
    int value = Integer.parseInt(userInput);
    int result = 100 / value;
} catch (NumberFormatException e) {
    LOGGER.warn("Invalid number format: {}", userInput);
} catch (ArithmeticException e) {
    LOGGER.error("Division by zero");
} finally {
    cleanup();
}

// try-with-resources (권장)
try (BufferedReader reader = new BufferedReader(new FileReader(path))) {
    String line = reader.readLine();
} catch (IOException e) {
    LOGGER.error("File read error", e);
}

// 사용자 정의 예외
public class CustomerNotFoundException extends RuntimeException {
    private final int customerId;
    
    public CustomerNotFoundException(int customerId) {
        super("Customer not found: " + customerId);
        this.customerId = customerId;
    }
    
    public int getCustomerId() {
        return customerId;
    }
}
```

---

## 7. Optional 사용

```java
// ✅ Good: Optional 반환
public Optional<Customer> findById(int id) {
    return Optional.ofNullable(repository.get(id));
}

// Optional 사용
Optional<Customer> customer = findById(123);

customer.ifPresent(c -> System.out.println(c.getName()));

String name = customer
    .map(Customer::getName)
    .orElse("Unknown");

Customer c = customer.orElseThrow(() -> 
    new CustomerNotFoundException(123));
```

---

## 8. Stream API

```java
// 필터링 및 변환
List<String> activeNames = customers.stream()
    .filter(Customer::isActive)
    .map(Customer::getName)
    .sorted()
    .collect(Collectors.toList());

// 그룹화
Map<String, List<Customer>> byCity = customers.stream()
    .collect(Collectors.groupingBy(Customer::getCity));

// 통계
double average = orders.stream()
    .mapToDouble(Order::getAmount)
    .average()
    .orElse(0.0);
```

---

## 9. Record (Java 16+)

```java
// 불변 데이터 클래스
public record Customer(
    int id,
    String name,
    String email
) {
    // 검증 로직
    public Customer {
        if (name == null || name.isBlank()) {
            throw new IllegalArgumentException("Name is required");
        }
    }
}

// 사용
Customer customer = new Customer(1, "John", "john@example.com");
String name = customer.name();
```

---

## 10. 어노테이션

```java
@Service
public class CustomerService {
    
    @Autowired
    private CustomerRepository repository;
    
    @Override
    public String toString() {
        return "CustomerService";
    }
    
    @Nullable
    public Customer findByEmail(String email) {
        return repository.findByEmail(email);
    }
    
    @Deprecated(since = "2.0", forRemoval = true)
    public void oldMethod() { }
}
```

---

## 📚 참고 자료

- [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html)
- [Oracle Java Code Conventions](https://www.oracle.com/java/technologies/javase/codeconventions-contents.html)

---

*마지막 업데이트: 2025년 12월*
