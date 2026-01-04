# Hướng Dẫn Toàn Diện Về Spring Framework (Phần 1: Giới Thiệu và Khởi Đầu)

## 1. Spring Framework Là Gì?

**Spring Framework** là một framework mã nguồn mở phổ biến nhất cho Java, được thiết kế để đơn giản hóa việc phát triển ứng dụng doanh nghiệp. Nó cung cấp các công cụ và thư viện để xây dựng ứng dụng web, API RESTful, ứng dụng microservices, và hơn thế nữa. **Spring Boot**, một module của Spring, là cách tiếp cận hiện đại để phát triển nhanh ứng dụng Spring với cấu hình tối thiểu.

### 1.1. Các Đặc Điểm Chính

- **IoC (Inversion of Control)**: Spring quản lý các đối tượng (beans) thông qua Dependency Injection (DI), giúp mã dễ bảo trì và kiểm thử.
- **Modular**: Spring bao gồm nhiều module như Spring MVC, Spring Data, Spring Security, v.v., bạn chỉ cần sử dụng những gì cần thiết.
- **Hiệu suất cao**: Phù hợp với ứng dụng doanh nghiệp lớn, hỗ trợ cả đồng bộ và bất đồng bộ.
- **Cộng đồng lớn**: Được sử dụng bởi các công ty như Netflix, Amazon, và được hỗ trợ bởi cộng đồng rộng lớn.
- **Tích hợp dễ dàng**: Hỗ trợ tích hợp với các công nghệ như Hibernate, JPA, Redis, Kafka, v.v.

### 1.2. Spring Boot Là Gì?

Spring Boot là một dự án trong hệ sinh thái Spring, giúp:

- **Giảm cấu hình**: Tự động cấu hình dựa trên các thư viện có trong dự án.
- **Tích hợp server**: Nhúng Tomcat, Jetty, hoặc Undertow để chạy ứng dụng mà không cần server riêng.
- **Tạo ứng dụng nhanh**: Cung cấp công cụ như Spring Initializr để khởi tạo dự án dễ dàng.
- **Tài liệu API**: Tích hợp Swagger/OpenAPI để tạo tài liệu API.

### 1.3. So Sánh Với Các Framework Khác

- **Vs FastAPI**: Spring mạnh mẽ hơn trong các ứng dụng doanh nghiệp lớn, tích hợp tốt với hệ sinh thái Java, nhưng phức tạp hơn FastAPI (dành cho Python, nhẹ và nhanh cho microservices).
- **Vs Django**: Spring linh hoạt hơn với microservices, nhưng Django (Python) dễ học hơn cho người mới và tích hợp sẵn ORM mạnh mẽ.
- **Vs Express.js**: Spring cung cấp cấu trúc rõ ràng hơn và phù hợp với ứng dụng lớn, trong khi Express.js (Node.js) nhẹ và nhanh nhưng ít tính năng tích hợp sẵn.

## 2. Cài Đặt Môi Trường

Để bắt đầu với Spring Boot, bạn cần:

- **JDK (Java Development Kit)**: Phiên bản 17 trở lên (khuyến nghị).
- **IDE**: IntelliJ IDEA (khuyên dùng) hoặc Eclipse.
- **Maven hoặc Gradle**: Công cụ quản lý phụ thuộc (Maven phổ biến hơn).
- **Spring Initializr**: Công cụ trực tuyến để tạo dự án Spring Boot.

### 2.1. Cài Đặt JDK

1. Tải JDK từ [Adoptium](https://adoptium.net/) hoặc [Oracle](https://www.oracle.com/java/technologies/javase-downloads.html).
2. Cài đặt và cấu hình biến môi trường:
   - Thêm `JAVA_HOME` trỏ đến thư mục cài đặt JDK.
   - Thêm `%JAVA_HOME%\bin` vào `PATH`.
3. Kiểm tra:
   ```bash
   java -version
   ```

### 2.2. Cài Đặt IntelliJ IDEA

1. Tải IntelliJ IDEA Community từ [JetBrains](https://www.jetbrains.com/idea/download/).
2. Cài đặt và mở IDE.

### 2.3. Tạo Dự Án Spring Boot

Sử dụng **Spring Initializr**:

1. Truy cập [start.spring.io](https://start.spring.io/).
2. Cấu hình:
   - **Project**: Maven.
   - **Language**: Java.
   - **Spring Boot**: Phiên bản mới nhất (ví dụ: 3.2.x).
   - **Dependencies**: Thêm `Spring Web` (cho API REST) và `Spring Boot DevTools` (tự động reload khi phát triển).
3. Tải dự án và giải nén.
4. Mở dự án trong IntelliJ IDEA.

Cấu trúc dự án:

```
my-spring-app/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/example/demo/
│   │   │       └── DemoApplication.java
│   │   ├── resources/
│   │   │   └── application.properties
├── pom.xml
```

### 2.4. Cài Đặt Maven

Maven được tích hợp trong dự án Spring Boot. Kiểm tra:

```bash
mvn -version
```

Nếu chưa có, tải từ [Maven](https://maven.apache.org/download.cgi) và cấu hình `PATH`.

## 3. Ứng Dụng Spring Boot Đầu Tiên

Dưới đây là một ứng dụng Spring Boot đơn giản tạo API REST cơ bản.

### 3.1. Mã Code

Tạo file `HelloController.java` trong `src/main/java/com/example/demo/`:

```java
package com.example.demo;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/hello")
    public String sayHello() {
        return "Hello, Spring Boot!";
    }
}
```

### 3.2. Giải Thích

- **`@RestController`**: Đánh dấu lớp này là một controller xử lý các yêu cầu HTTP và trả về JSON.
- **`@GetMapping("/hello")`**: Xử lý yêu cầu GET tại `/hello`.
- **`DemoApplication.java`**: File chính (tạo sẵn bởi Spring Initializr) để khởi động ứng dụng:

  ```java
  package com.example.demo;

  import org.springframework.boot.SpringApplication;
  import org.springframework.boot.autoconfigure.SpringBootApplication;

  @SpringBootApplication
  public class DemoApplication {
      public static void main(String[] args) {
          SpringApplication.run(DemoApplication.class, args);
      }
  }
  ```

  - **`@SpringBootApplication`**: Kích hoạt tự động cấu hình, quét component, và quản lý bean.

### 3.3. Chạy Ứng Dụng

1. Trong IntelliJ, nhấn nút "Run" (hình tam giác xanh) bên cạnh `DemoApplication`.
2. Hoặc chạy lệnh:
   ```bash
   mvn spring-boot:run
   ```
3. Mở trình duyệt hoặc Postman, truy cập `http://localhost:8080/hello`. Kết quả:
   ```
   Hello, Spring Boot!
   ```

## 4. Các Khái Niệm Cốt Lõi

### 4.1. Inversion of Control (IoC) và Dependency Injection (DI)

- **IoC**: Spring quản lý việc tạo và liên kết các đối tượng (beans). Thay vì bạn tạo đối tượng thủ công, Spring "tiêm" chúng vào nơi cần.
- **DI**: Cách Spring cung cấp các phụ thuộc (dependencies) cho một lớp. Ví dụ:

  ```java
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.stereotype.Service;

  @Service
  public class MyService {
      public String getMessage() {
          return "Hello from Service!";
      }
  }

  @RestController
  public class MyController {
      private final MyService myService;

      @Autowired
      public MyController(MyService myService) {
          this.myService = myService;
      }

      @GetMapping("/message")
      public String getMessage() {
          return myService.getMessage();
      }
  }
  ```

  - **`@Service`**: Đánh dấu `MyService` là một bean.
  - **`@Autowired`**: Tiêm `MyService` vào `MyController`.

### 4.2. Spring Beans

- **Bean**: Một đối tượng được Spring quản lý.
- Được định nghĩa bằng các annotation như `@Component`, `@Service`, `@Repository`, hoặc trong file cấu hình XML/Java.

### 4.3. Spring MVC

- **Model-View-Controller**: Spring MVC là module xử lý yêu cầu HTTP.
  - **Model**: Dữ liệu (ví dụ: một đối tượng Java).
  - **View**: Giao diện (thường không dùng trong API REST).
  - **Controller**: Xử lý logic (như `HelloController`).

### 4.4. Application Properties

File `application.properties` trong `src/main/resources` dùng để cấu hình:

```properties
server.port=8080
spring.application.name=my-spring-app
```

## 5. Ưu Điểm Của Spring So Với Các Framework Khác

- **Hệ sinh thái mạnh mẽ**: Spring tích hợp với hầu hết các công nghệ Java (Hibernate, JPA, Kafka, v.v.), phù hợp cho ứng dụng doanh nghiệp.
- **Cấu trúc rõ ràng**: Phân tầng (controller, service, repository) giúp mã dễ bảo trì.
- **Hỗ trợ microservices**: Spring Boot và Spring Cloud lý tưởng cho kiến trúc microservices.
- **Cộng đồng lớn**: Tài liệu phong phú, hỗ trợ từ Pivotal và cộng đồng.
- **So với FastAPI**: Spring phức tạp hơn nhưng mạnh mẽ hơn trong các dự án lớn, tích hợp tốt với hệ thống doanh nghiệp.

## 6. Kết Luận Phần 1

Phần này đã giới thiệu Spring Framework, Spring Boot, cách cài đặt môi trường, và tạo một API REST cơ bản. Bạn đã học các khái niệm như IoC, DI, và Spring MVC. Phần tiếp theo sẽ đi sâu vào:

- Xây dựng API RESTful đầy đủ với CRUD.
- Tích hợp cơ sở dữ liệu với Spring Data JPA.
- Xử lý xác thực với Spring Security.

# Hướng Dẫn Toàn Diện Về Spring Framework (Phần 2: API RESTful, Spring Data JPA, và Giới Thiệu Spring Security)

## 1. Xây Dựng API RESTful Với CRUD

Trong phần này, chúng ta sẽ tạo một ứng dụng Spring Boot để quản lý danh sách **sản phẩm** (Product) với các thao tác CRUD. API sẽ:

- **GET** `/products`: Lấy danh sách sản phẩm.
- **GET** `/products/{id}`: Lấy thông tin một sản phẩm.
- **POST** `/products`: Tạo sản phẩm mới.
- **PUT** `/products/{id}`: Cập nhật sản phẩm.
- **DELETE** `/products/{id}`: Xóa sản phẩm.

### 1.1. Tạo Dự Án Mới

1. Truy cập [start.spring.io](https://start.spring.io/).
2. Cấu hình:
   - **Project**: Maven.
   - **Language**: Java.
   - **Spring Boot**: Phiên bản mới nhất (ví dụ: 3.2.x).
   - **Dependencies**: `Spring Web`, `Spring Data JPA`, `H2 Database` (cơ sở dữ liệu nhúng để thử nghiệm), `Spring Boot DevTools`.
3. Tải và mở dự án trong IntelliJ IDEA.

### 1.2. Định Nghĩa Model

Tạo class `Product` trong `src/main/java/com/example/demo/model/`:

```java
package com.example.demo.model;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;

@Entity
public class Product {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private double price;

    // Constructors
    public Product() {}

    public Product(String name, double price) {
        this.name = name;
        this.price = price;
    }

    // Getters and Setters
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }
}
```

**Giải thích**:

- **`@Entity`**: Đánh dấu `Product` là một thực thể JPA, ánh xạ với bảng trong cơ sở dữ liệu.
- **`@Id`**: Đánh dấu trường `id` là khóa chính.
- **`@GeneratedValue`**: Tự động tạo giá trị cho `id` (tăng dần).

### 1.3. Tạo Repository

Tạo interface `ProductRepository` trong `src/main/java/com/example/demo/repository/`:

```java
package com.example.demo.repository;

import com.example.demo.model.Product;
import org.springframework.data.jpa.repository.JpaRepository;

public interface ProductRepository extends JpaRepository<Product, Long> {
}
```

**Giải thích**:

- **`JpaRepository<Product, Long>`**: Cung cấp các phương thức CRUD sẵn có (findAll, findById, save, delete, v.v.).
- Không cần viết code triển khai, Spring Data JPA tự động tạo logic truy vấn.

### 1.4. Tạo Service

Tạo class `ProductService` trong `src/main/java/com/example/demo/service/`:

```java
package com.example.demo.service;

import com.example.demo.model.Product;
import com.example.demo.repository.ProductRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Optional;

@Service
public class ProductService {

    private final ProductRepository productRepository;

    @Autowired
    public ProductService(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    public List<Product> getAllProducts() {
        return productRepository.findAll();
    }

    public Optional<Product> getProductById(Long id) {
        return productRepository.findById(id);
    }

    public Product createProduct(Product product) {
        return productRepository.save(product);
    }

    public Optional<Product> updateProduct(Long id, Product productDetails) {
        Optional<Product> product = productRepository.findById(id);
        if (product.isPresent()) {
            Product updatedProduct = product.get();
            updatedProduct.setName(productDetails.getName());
            updatedProduct.setPrice(productDetails.getPrice());
            return Optional.of(productRepository.save(updatedProduct));
        }
        return Optional.empty();
    }

    public boolean deleteProduct(Long id) {
        if (productRepository.existsById(id)) {
            productRepository.deleteById(id);
            return true;
        }
        return false;
    }
}
```

**Giải thích**:

- **`@Service`**: Đánh dấu đây là một bean chứa logic nghiệp vụ.
- **`@Autowired`**: Tiêm `ProductRepository` vào `ProductService`.
- Các phương thức thực hiện CRUD, sử dụng `Optional` để xử lý trường hợp không tìm thấy dữ liệu.

### 1.5. Tạo Controller

Tạo class `ProductController` trong `src/main/java/com/example/demo/controller/`:

```java
package com.example.demo.controller;

import com.example.demo.model.Product;
import com.example.demo.service.ProductService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/products")
public class ProductController {

    private final ProductService productService;

    @Autowired
    public ProductController(ProductService productService) {
        this.productService = productService;
    }

    @GetMapping
    public List<Product> getAllProducts() {
        return productService.getAllProducts();
    }

    @GetMapping("/{id}")
    public ResponseEntity<Product> getProductById(@PathVariable Long id) {
        return productService.getProductById(id)
                .map(ResponseEntity::ok)
                .orElseGet(() -> ResponseEntity.notFound().build());
    }

    @PostMapping
    public Product createProduct(@RequestBody Product product) {
        return productService.createProduct(product);
    }

    @PutMapping("/{id}")
    public ResponseEntity<Product> updateProduct(@PathVariable Long id, @RequestBody Product product) {
        return productService.updateProduct(id, product)
                .map(ResponseEntity::ok)
                .orElseGet(() -> ResponseEntity.notFound().build());
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteProduct(@PathVariable Long id) {
        if (productService.deleteProduct(id)) {
            return ResponseEntity.noContent().build();
        }
        return ResponseEntity.notFound().build();
    }
}
```

**Giải thích**:

- **`@RestController`**: Xử lý yêu cầu HTTP và trả về JSON.
- **`@RequestMapping("/products")`**: Định nghĩa tiền tố URL cho tất cả endpoint.
- **`@GetMapping`, `@PostMapping`, v.v.**: Ánh xạ các phương thức HTTP.
- **`@PathVariable`**: Lấy tham số từ URL (ví dụ: `id`).
- **`@RequestBody`**: Lấy dữ liệu JSON từ body của yêu cầu.
- **`ResponseEntity`**: Kiểm soát mã trạng thái HTTP (200, 404, v.v.).

### 1.6. Cấu Hình Cơ Sở Dữ liệu

Thêm cấu hình vào `src/main/resources/application.properties`:

```properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.h2.console.enabled=true
```

**Giải thích**:

- Sử dụng **H2 Database** nhúng để thử nghiệm.
- `spring.h2.console.enabled=true`: Kích hoạt giao diện H2 tại `http://localhost:8080/h2-console`.

### 1.7. Chạy và Kiểm Tra

1. Chạy ứng dụng:
   ```bash
   mvn spring-boot:run
   ```
2. Thử các endpoint bằng Postman hoặc curl:

   - **Tạo sản phẩm**:
     ```bash
     curl -X POST http://localhost:8080/products -H "Content-Type: application/json" -d '{"name":"Laptop","price":999.99}'
     ```
   - **Lấy danh sách**:
     ```bash
     curl http://localhost:8080/products
     ```
   - **Lấy một sản phẩm**:
     ```bash
     curl http://localhost:8080/products/1
     ```
   - **Cập nhật**:
     ```bash
     curl -X PUT http://localhost:8080/products/1 -H "Content-Type: application/json" -d '{"name":"Laptop Pro","price":1299.99}'
     ```
   - **Xóa**:
     ```bash
     curl -X DELETE http://localhost:8080/products/1
     ```

3. Truy cập H2 Console tại `http://localhost:8080/h2-console` để kiểm tra dữ liệu.

## 2. Tích Hợp Spring Data JPA

**Spring Data JPA** là module của Spring để làm việc với cơ sở dữ liệu quan hệ, sử dụng JPA (Java Persistence API) và Hibernate làm nhà cung cấp mặc định.

### 2.1. Các Khái Niệm Cơ Bản

- **Entity**: Một class ánh xạ với bảng trong cơ sở dữ liệu (như `Product`).
- **Repository**: Interface cung cấp các phương thức CRUD và truy vấn tùy chỉnh.
- **JPA**: Tiêu chuẩn Java để quản lý dữ liệu quan hệ.

### 2.2. Truy Vấn Tùy Chỉnh

Thêm phương thức tìm kiếm theo tên vào `ProductRepository`:

```java
package com.example.demo.repository;

import com.example.demo.model.Product;
import org.springframework.data.jpa.repository.JpaRepository;

import java.util.List;

public interface ProductRepository extends JpaRepository<Product, Long> {
    List<Product> findByNameContainingIgnoreCase(String name);
}
```

Cập nhật `ProductService`:

```java
public List<Product> searchProductsByName(String name) {
    return productRepository.findByNameContainingIgnoreCase(name);
}
```

Cập nhật `ProductController`:

```java
@GetMapping("/search")
public List<Product> searchProducts(@RequestParam String name) {
    return productService.searchProductsByName(name);
}
```

**Kiểm tra**:

```bash
curl "http://localhost:8080/products/search?name=laptop"
```

**Giải thích**:

- **`findByNameContainingIgnoreCase`**: Spring Data JPA tự động tạo truy vấn SQL tìm kiếm không phân biệt hoa thường.
- **`@RequestParam`**: Lấy tham số query từ URL (ví dụ: `?name=laptop`).

## 3. Giới Thiệu Spring Security

**Spring Security** là module để bảo mật ứng dụng, hỗ trợ xác thực (authentication) và phân quyền (authorization). Ở đây, chúng ta sẽ thêm xác thực cơ bản bằng **HTTP Basic Authentication** để bảo vệ các endpoint.

### 3.1. Thêm Dependency

Cập nhật `pom.xml` để thêm Spring Security:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

### 3.2. Cấu Hình Security

Tạo class `SecurityConfig` trong `src/main/java/com/example/demo/config/`:

```java
package com.example.demo.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authorize -> authorize
                .requestMatchers("/h2-console/**").permitAll()
                .anyRequest().authenticated()
            )
            .httpBasic()
            .and()
            .csrf().disable()
            .headers().frameOptions().disable();
        return http.build();
    }

    @Bean
    public UserDetailsService userDetailsService() {
        var user = User.withDefaultPasswordEncoder()
                .username("admin")
                .password("password")
                .roles("USER")
                .build();
        return new InMemoryUserDetailsManager(user);
    }
}
```

**Giải thích**:

- **`@EnableWebSecurity`**: Kích hoạt Spring Security.
- **`SecurityFilterChain`**: Cấu hình quy tắc bảo mật:
  - Cho phép truy cập `/h2-console` mà không cần xác thực.
  - Yêu cầu xác thực cho tất cả các request khác.
  - Sử dụng HTTP Basic Authentication.
- **`UserDetailsService`**: Tạo người dùng trong bộ nhớ với tên `admin` và mật khẩu `password`.
- **`csrf().disable()`**: Tắt CSRF để thử nghiệm (không nên dùng trong production).
- **`headers().frameOptions().disable()`**: Cho phép H2 Console hoạt động.

### 3.3. Kiểm Tra

1. Chạy lại ứng dụng.
2. Thử truy cập `http://localhost:8080/products` bằng curl:
   ```bash
   curl -u admin:password http://localhost:8080/products
   ```
   - `-u admin:password`: Cung cấp thông tin xác thực.
3. Nếu không cung cấp thông tin xác thực, bạn sẽ nhận lỗi 401 Unauthorized.

## 4. So Sánh Với FastAPI

- **Tích hợp cơ sở dữ liệu**:
  - **Spring Data JPA**: Cung cấp repository mạnh mẽ, hỗ trợ truy vấn tùy chỉnh dễ dàng, phù hợp với ứng dụng lớn. Tuy nhiên, cần định nghĩa entity và cấu hình rõ ràng.
  - **FastAPI với SQLAlchemy**: Linh hoạt và nhẹ hơn, nhưng cần viết nhiều code hơn cho các truy vấn phức tạp.
- **Bảo mật**:
  - **Spring Security**: Cung cấp giải pháp toàn diện (OAuth2, JWT, LDAP, v.v.), nhưng phức tạp hơn cho người mới.
  - **FastAPI**: Dựa vào thư viện bên ngoài như `python-jose` cho JWT, dễ cấu hình hơn nhưng ít tích hợp sẵn.
- **Hiệu suất**:
  - FastAPI nhanh hơn trong các tác vụ bất đồng bộ (nhờ ASGI), nhưng Spring Boot tối ưu tốt cho ứng dụng đồng bộ và có thể mở rộng với Reactor cho bất đồng bộ.
- **Dễ học**:
  - FastAPI có cú pháp đơn giản, phù hợp cho người mới với Python.
  - Spring Boot yêu cầu hiểu về Java, annotations, và cấu trúc dự án, nhưng cung cấp hệ sinh thái mạnh mẽ hơn.

## 5. Kết Luận Phần 2

Phần này đã hướng dẫn bạn:

- Xây dựng API RESTful với CRUD sử dụng Spring Boot.
- Tích hợp cơ sở dữ liệu với Spring Data JPA và H2 Database.
- Thêm xác thực cơ bản với Spring Security.

Phần tiếp theo sẽ bao gồm:

- Xác thực nâng cao với **JWT** (JSON Web Token).
- Tích hợp **Swagger/OpenAPI** để tạo tài liệu API.
- Xử lý lỗi và validation.
- Hướng dẫn triển khai ứng dụng Spring Boot lên server.

# Hướng Dẫn Toàn Diện Về Spring Framework (Phần 3: JWT, Swagger, Xử Lý Lỗi, và Triển Khai)

## 1. Xác Thực Nâng Cao Với JWT

**JWT (JSON Web Token)** là một chuẩn mở để xác thực và truyền thông tin giữa client và server dưới dạng token. Trong phần này, chúng ta sẽ tích hợp JWT vào ứng dụng quản lý sản phẩm để bảo vệ các endpoint, thay thế cho HTTP Basic Authentication ở phần trước.

### 1.1. Thêm Dependency

Cập nhật `pom.xml` để thêm thư viện JWT:

```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.11.5</version>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.11.5</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId>
    <version>0.11.5</version>
    <scope>runtime</scope>
</dependency>
```

### 1.2. Cấu Hình JWT

Tạo class `JwtUtil` trong `src/main/java/com/example/demo/config Secondary Title` để xử lý tạo và xác thực JWT:

```java
package com.example.demo.util;

import io.jsonwebtoken.*;
import org.springframework.stereotype.Component;

import java.util.Date;

@Component
public class JwtUtil {

    private static final String SECRET_KEY = "your-256-bit-secret"; // Thay bằng key mạnh hơn trong production
    private static final long EXPIRATION_TIME = 86400000; // 1 ngày

    public String generateToken(String username) {
        return Jwts.builder()
                .setSubject(username)
                .setIssuedAt(new Date())
                .setExpiration(new Date(System.currentTimeMillis() + EXPIRATION_TIME))
                .signWith(SignatureAlgorithm.HS256, SECRET_KEY.getBytes())
                .compact();
    }

    public String extractUsername(String token) {
        return Jwts.parser()
                .setSigningKey(SECRET_KEY.getBytes())
                .parseClaimsJws(token)
                .getBody()
                .getSubject();
    }

    public boolean validateToken(String token) {
        try {
            Jwts.parser().setSigningKey(SECRET_KEY.getBytes()).parseClaimsJws(token);
            return true;
        } catch (JwtException e) {
            return false;
        }
    }
}
```

**Giải thích**:

- **`generateToken`**: Tạo JWT với thông tin người dùng (username) và thời hạn hết hạn.
- **`extractUsername`**: Trích xuất username từ token.
- **`validateToken`**: Kiểm tra tính hợp lệ của token.
- **`SECRET_KEY`**: Phải là một chuỗi bí mật mạnh, dài ít nhất 256 bit. Trong production, lưu key này trong biến môi trường hoặc file cấu hình.

### 1.3. Cập Nhật Security Configuration

Cập nhật `SecurityConfig` để sử dụng JWT thay vì HTTP Basic:

```java
package com.example.demo.config;

import com.example.demo.util.JwtUtil;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Autowired
    private JwtUtil jwtUtil;

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            .and()
            .authorizeHttpRequests(authorize -> authorize
                .requestMatchers("/h2-console/**", "/auth/login").permitAll()
                .anyRequest().authenticated()
            )
            .headers().frameOptions().disable()
            .and()
            .addFilterBefore(new JwtAuthenticationFilter(jwtUtil), UsernamePasswordAuthenticationFilter.class);
        return http.build();
    }

    @Bean
    public UserDetailsService userDetailsService() {
        var user = User.withDefaultPasswordEncoder()
                .username("admin")
                .password("password")
                .roles("USER")
                .build();
        return new InMemoryUserDetailsManager(user);
    }
}
```

**Giải thích**:

- **`SessionCreationPolicy.STATELESS`**: Không lưu session, phù hợp với JWT.
- **`JwtAuthenticationFilter`**: Bộ lọc kiểm tra JWT trong header của mỗi request (sẽ tạo dưới đây).
- Cho phép truy cập `/auth/login` và `/h2-console` mà không cần xác thực.

### 1.4. Tạo JWT Authentication Filter

Tạo class `JwtAuthenticationFilter` trong `src/main/java/com/example/demo/filter/`:

```java
package com.example.demo.filter;

import com.example.demo.util.JwtUtil;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
import org.springframework.stereotype.Component;
import org.springframework.web.filter.OncePerRequestFilter;

import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import java.io.IOException;

@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    @Autowired
    private JwtUtil jwtUtil;

    @Autowired
    private UserDetailsService userDetailsService;

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
            throws ServletException, IOException {
        String authHeader = request.getHeader("Authorization");
        String token = null;
        String username = null;

        if (authHeader != null && authHeader.startsWith("Bearer ")) {
            token = authHeader.substring(7);
            username = jwtUtil.extractUsername(token);
        }

        if (username != null && SecurityContextHolder.getContext().getAuthentication() == null) {
            UserDetails userDetails = userDetailsService.loadUserByUsername(username);
            if (jwtUtil.validateToken(token)) {
                UsernamePasswordAuthenticationToken authToken =
                        new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());
                authToken.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
                SecurityContextHolder.getContext().setAuthentication(authToken);
            }
        }
        filterChain.doFilter(request, response);
    }
}
```

**Giải thích**:

- Kiểm tra header `Authorization` có chứa token dạng `Bearer <token>`.
- Trích xuất username từ token và xác thực.
- Nếu hợp lệ, thiết lập thông tin xác thực trong `SecurityContext`.

### 1.5. Tạo Endpoint Login

Cập nhật `ProductController` hoặc tạo class mới `AuthController` trong `src/main/java/com/example/demo/controller/`:

```java
package com.example.demo.controller;

import com.example.demo.util.JwtUtil;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/auth")
public class AuthController {

    @Autowired
    private AuthenticationManager authenticationManager;

    @Autowired
    private UserDetailsService userDetailsService;

    @Autowired
    private JwtUtil jwtUtil;

    @PostMapping("/login")
    public ResponseEntity<?> login(@RequestBody AuthRequest authRequest) {
        try {
            authenticationManager.authenticate(
                    new UsernamePasswordAuthenticationToken(authRequest.getUsername(), authRequest.getPassword())
            );
        } catch (Exception e) {
            return ResponseEntity.status(401).body("Invalid credentials");
        }

        final UserDetails userDetails = userDetailsService.loadUserByUsername(authRequest.getUsername());
        final String jwt = jwtUtil.generateToken(userDetails.getUsername());

        return ResponseEntity.ok(new AuthResponse(jwt));
    }
}

class AuthRequest {
    private String username;
    private String password;

    // Getters and Setters
    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}

class AuthResponse {
    private final String jwt;

    public AuthResponse(String jwt) {
        this.jwt = jwt;
    }

    public String getJwt() {
        return jwt;
    }
}
```

**Giải thích**:

- Endpoint `/auth/login` nhận username và password, xác thực, và trả về JWT nếu thành công.
- **`AuthRequest` và `AuthResponse`**: Các class DTO (Data Transfer Object) để xử lý request/response.

### 1.6. Cấu Hình AuthenticationManager

Thêm bean `AuthenticationManager` vào `SecurityConfig`:

```java
@Bean
public AuthenticationManager authenticationManager(AuthenticationConfiguration config) throws Exception {
    return config.getAuthenticationManager();
}
```

Cập nhật import:

```java
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
```

### 1.7. Kiểm Tra

1. Chạy ứng dụng:
   ```bash
   mvn spring-boot:run
   ```
2. Đăng nhập để lấy JWT:
   ```bash
   curl -X POST http://localhost:8080/auth/login -H "Content-Type: application/json" -d '{"username":"admin","password":"password"}'
   ```
   Kết quả mẫu:
   ```json
   { "jwt": "eyJhbGciOiJIUzI1NiJ9..." }
   ```
3. Truy cập endpoint bảo mật:
   ```bash
   curl http://localhost:8080/products -H "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9..."
   ```

## 2. Tích Hợp Swagger/OpenAPI

**Swagger** (thông qua Springdoc OpenAPI) giúp tạo tài liệu API tự động, tương tự Swagger UI trong FastAPI.

### 2.1. Thêm Dependency

Cập nhật `pom.xml`:

```xml
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
    <version>2.2.0</version>
</dependency>
```

### 2.2. Cấu Hình

Thêm cấu hình vào `application.properties`:

```properties
springdoc.api-docs.path=/api-docs
springdoc.swagger-ui.path=/swagger-ui.html
```

### 2.3. Kiểm Tra

1. Chạy ứng dụng.
2. Truy cập `http://localhost:8080/swagger-ui.html` để xem tài liệu API tương tác.
3. API sẽ hiển thị các endpoint như `/products`, `/auth/login`, với mô tả và tùy chọn thử nghiệm.

### 2.4. Tùy Chỉnh Swagger

Thêm mô tả API vào `application.properties`:

```properties
springdoc.swagger-ui.display-request-duration=true
springdoc.info.title=Product Management API
springdoc.info.description=API for managing products with JWT authentication
springdoc.info.version=1.0.0
```

**Giải thích**:

- Springdoc tự động quét các controller và tạo tài liệu.
- Có thể thêm annotation như `@Operation` và `@ApiResponse` để mô tả chi tiết hơn.

## 3. Xử Lý Lỗi và Validation

### 3.1. Validation

Thêm validation cho `Product` để đảm bảo dữ liệu hợp lệ. Cập nhật `Product`:

```java
package com.example.demo.model;

import jakarta.persistence.*;
import jakarta.validation.constraints.*;

@Entity
public class Product {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @NotBlank(message = "Name is mandatory")
    @Size(min = 2, max = 100, message = "Name must be between 2 and 100 characters")
    private String name;

    @Positive(message = "Price must be positive")
    private double price;

    // Constructors, Getters, Setters (giữ nguyên)
}
```

Cập nhật `ProductController` để xử lý lỗi validation:

```java
import jakarta.validation.Valid;

@PostMapping
public Product createProduct(@Valid @RequestBody Product product) {
    return productService.createProduct(product);
}

@PutMapping("/{id}")
public ResponseEntity<Product> updateProduct(@PathVariable Long id, @Valid @RequestBody Product product) {
    return productService.updateProduct(id, product)
            .map(ResponseEntity::ok)
            .orElseGet(() -> ResponseEntity.notFound().build());
}
```

**Giải thích**:

- **`@NotBlank`, `@Size`, `@Positive`**: Các ràng buộc validation.
- **`@Valid`**: Kích hoạt validation cho request body.
- Nếu validation thất bại, Spring trả về lỗi 400 Bad Request với chi tiết.

### 3.2. Xử Lý Lỗi Toàn Cầu

Tạo class `GlobalExceptionHandler` trong `src/main/java/com/example/demo/exception/`:

```java
package com.example.demo.exception;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

import java.util.HashMap;
import java.util.Map;

@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<Map<String, String>> handleValidationExceptions(MethodArgumentNotValidException ex) {
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getFieldErrors().forEach(error ->
                errors.put(error.getField(), error.getDefaultMessage()));
        return new ResponseEntity<>(errors, HttpStatus.BAD_REQUEST);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleAllExceptions(Exception ex) {
        return new ResponseEntity<>("An error occurred: " + ex.getMessage(), HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

**Giải thích**:

- **`@RestControllerAdvice`**: Xử lý lỗi toàn cục.
- **`MethodArgumentNotValidException`**: Xử lý lỗi validation.
- Trả về JSON với chi tiết lỗi, ví dụ:
  ```json
  {
    "name": "Name is mandatory",
    "price": "Price must be positive"
  }
  ```

## 4. Triển Khai Ứng Dụng Spring Boot

### 4.1. Đóng Gói Ứng Dụng

Tạo file JAR chạy được:

```bash
mvn clean package
```

File JAR sẽ nằm trong `target/demo-0.0.1-SNAPSHOT.jar`.

### 4.2. Chạy JAR

Chạy file JAR:

```bash
java -jar target/demo-0.0.1-SNAPSHOT.jar
```

### 4.3. Triển Khai Lên Server

Ví dụ triển khai lên **Heroku**:

1. Cài đặt Heroku CLI.
2. Đăng nhập:
   ```bash
   heroku login
   ```
3. Tạo ứng dụng:
   ```bash
   heroku create my-spring-app
   ```
4. Thêm `Procfile` trong thư mục gốc dự án:
   ```
   web: java -Dserver.port=$PORT -jar target/demo-0.0.1-SNAPSHOT.jar
   ```
5. Đẩy code lên Heroku:
   ```bash
   git add .
   git commit -m "Deploy to Heroku"
   git push heroku main
   ```
6. Truy cập URL do Heroku cung cấp (ví dụ: `https://my-spring-app.herokuapp.com`).

### 4.4. Sử Dụng Docker

Tạo `Dockerfile` trong thư mục gốc:

```
FROM openjdk:17-jdk-slim
COPY target/demo-0.0.1-SNAPSHOT.jar app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
```

Xây dựng và chạy Docker:

```bash
docker build -t my-spring-app .
docker run -p 8080:8080 my-spring-app
```

## 5. So Sánh Với FastAPI

- **JWT Authentication**:
  - **Spring**: Spring Security cung cấp giải pháp toàn diện, tích hợp chặt chẽ với hệ sinh thái Java, nhưng cấu hình phức tạp hơn.
  - **FastAPI**: Dễ cấu hình hơn với thư viện `python-jose`, nhưng cần thêm logic để tích hợp sâu.
- **Tài liệu API**:
  - **Spring**: Springdoc OpenAPI tạo tài liệu tương tự Swagger UI/ReDoc của FastAPI, nhưng cần thêm dependency và cấu hình.
  - **FastAPI**: Tích hợp sẵn Swagger UI/ReDoc, không cần cấu hình thêm.
- **Validation**:
  - **Spring**: Sử dụng Jakarta Bean Validation, mạnh mẽ nhưng cần annotation và xử lý lỗi thủ công.
  - **FastAPI**: Pydantic tích hợp sẵn, tự động trả về lỗi chi tiết, đơn giản hơn.
- **Triển khai**:
  - **Spring**: JAR hoặc Docker, tích hợp tốt với các nền tảng như Heroku, AWS. Phù hợp với ứng dụng lớn.
  - **FastAPI**: Nhẹ hơn, dễ triển khai với Docker hoặc các nền tảng serverless, phù hợp với microservices.

## 6. Kết Luận Phần 3

Phần này đã hướng dẫn bạn:

- Tích hợp xác thực JWT với Spring Security.
- Tạo tài liệu API tự động với Swagger/OpenAPI.
- Xử lý lỗi và validation để đảm bảo dữ liệu hợp lệ.
- Triển khai ứng dụng lên server hoặc sử dụng Docker.

Phần tiếp theo sẽ bao gồm:

- Tích hợp với cơ sở dữ liệu thực tế (MySQL/PostgreSQL).
- Xử lý bất đồng bộ với Spring WebFlux.
- Tối ưu hóa hiệu suất và bảo mật.
- Các best practices và kiến trúc Clean Architecture.

# Hướng Dẫn Toàn Diện Về Spring Framework (Phần 4: MySQL/PostgreSQL, WebFlux, Tối Ưu Hóa, và Best Practices)

## 1. Tích Hợp Cơ Sở Dữ Liệu Thực Tế (MySQL/PostgreSQL)

Chúng ta sẽ chuyển từ H2 Database (nhúng) sang **MySQL** hoặc **PostgreSQL** để phù hợp với môi trường production.

### 1.1. Cài Đặt MySQL/PostgreSQL

- **MySQL**: Tải từ [mysql.com](https://dev.mysql.com/downloads/) và chạy server.
- **PostgreSQL**: Tải từ [postgresql.org](https://www.postgresql.org/download/) và chạy server.
- Tạo database:
  ```sql
  CREATE DATABASE product_db;
  ```

### 1.2. Cập Nhật Dependency

Thêm driver MySQL hoặc PostgreSQL vào `pom.xml` (chọn một):

```xml
<!-- MySQL -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.33</version>
</dependency>
<!-- PostgreSQL -->
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.7.3</version>
</dependency>
```

### 1.3. Cấu Hình Datasource

Cập nhật `application.properties`:

```properties
# MySQL
spring.datasource.url=jdbc:mysql://localhost:3306/product_db
spring.datasource.username=root
spring.datasource.password=your_password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# PostgreSQL (thay nếu dùng)
# spring.datasource.url=jdbc:postgresql://localhost:5432/product_db
# spring.datasource.username=postgres
# spring.datasource.password=your_password
# spring.datasource.driver-class-name=org.postgresql.Driver

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
# spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect (cho PostgreSQL)
```

**Giải thích**:

- **`ddl-auto=update`**: Tự động cập nhật schema dựa trên entity.
- **`show-sql=true`**: Hiển thị câu lệnh SQL trong console.
- **`hibernate.dialect`**: Chỉ định dialect phù hợp với database.

### 1.4. Kiểm Tra

- Giữ nguyên `Product`, `ProductRepository`, `ProductService`, và `ProductController` từ các phần trước.
- Chạy ứng dụng và kiểm tra endpoint `/products`. Dữ liệu sẽ lưu vào MySQL/PostgreSQL thay vì H2.

## 2. Xử Lý Bất Đồng Bộ Với Spring WebFlux

**Spring WebFlux** là module hỗ trợ lập trình phản ứng (reactive programming), phù hợp với các ứng dụng cần xử lý nhiều yêu cầu đồng thời (như microservices hoặc streaming).

### 2.1. Thêm Dependency

Cập nhật `pom.xml`:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-r2dbc</artifactId>
</dependency>
<dependency>
    <groupId>io.r2dbc</groupId>
    <artifactId>r2dbc-mysql</artifactId>
    <version>1.0.5</version>
</dependency>
```

### 2.2. Cập Nhật Model

Cập nhật `Product` để tương thích với R2DBC (reactive database):

```java
package com.example.demo.model;

import org.springframework.data.annotation.Id;
import org.springframework.data.relational.core.mapping.Table;

@Table("products")
public class Product {

    @Id
    private Long id;
    private String name;
    private double price;

    // Constructors, Getters, Setters (giữ nguyên)
}
```

### 2.3. Tạo Repository

Tạo `ProductReactiveRepository`:

```java
package com.example.demo.repository;

import com.example.demo.model.Product;
import org.springframework.data.repository.reactive.ReactiveCrudRepository;

public interface ProductReactiveRepository extends ReactiveCrudRepository<Product, Long> {
}
```

### 2.4. Tạo Service

Tạo `ProductReactiveService`:

```java
package com.example.demo.service;

import com.example.demo.model.Product;
import com.example.demo.repository.ProductReactiveRepository;
import org.springframework.stereotype.Service;
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

@Service
public class ProductReactiveService {

    private final ProductReactiveRepository repository;

    public ProductReactiveService(ProductReactiveRepository repository) {
        this.repository = repository;
    }

    public Flux<Product> getAllProducts() {
        return repository.findAll();
    }

    public Mono<Product> getProductById(Long id) {
        return repository.findById(id);
    }

    public Mono<Product> createProduct(Product product) {
        return repository.save(product);
    }
}
```

### 2.5. Tạo Controller

Tạo `ProductReactiveController`:

```java
package com.example.demo.controller;

import com.example.demo.model.Product;
import com.example.demo.service.ProductReactiveService;
import org.springframework.web.bind.annotation.*;
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

@RestController
@RequestMapping("/reactive/products")
public class ProductReactiveController {

    private final ProductReactiveService service;

    public ProductReactiveController(ProductReactiveService service) {
        this.service = service;
    }

    @GetMapping
    public Flux<Product> getAllProducts() {
        return service.getAllProducts();
    }

    @GetMapping("/{id}")
    public Mono<Product> getProductById(@PathVariable Long id) {
        return service.getProductById(id);
    }

    @PostMapping
    public Mono<Product> createProduct(@RequestBody Product product) {
        return service.createProduct(product);
    }
}
```

### 2.6. Cấu Hình R2DBC

Cập nhật `application.properties`:

```properties
spring.r2dbc.url=r2dbc:mysql://localhost:3306/product_db
spring.r2dbc.username=root
spring.r2dbc.password=your_password
```

### 2.7. Kiểm Tra

- Chạy ứng dụng và truy cập `/reactive/products`.
- WebFlux sử dụng **Flux** (danh sách) và **Mono** (một phần tử) để xử lý dữ liệu bất đồng bộ, cải thiện hiệu suất với lưu lượng lớn.

**So với FastAPI**:

- FastAPI sử dụng `async/await` với ASGI, đơn giản hơn cho bất đồng bộ.
- WebFlux mạnh mẽ hơn trong hệ sinh thái Java, nhưng phức tạp hơn do reactive programming.

## 3. Tối Ưu Hóa Hiệu Suất và Bảo Mật

### 3.1. Tối Ưu Hiệu Suất

- **Caching**: Sử dụng Spring Cache để giảm truy vấn database.

  ```java
  @Service
  @Cacheable("products")
  public class ProductService {
      public List<Product> getAllProducts() {
          return productRepository.findAll();
      }
  }
  ```

  Thêm dependency:

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-cache</artifactId>
  </dependency>
  ```

  Kích hoạt cache trong `DemoApplication`:

  ```java
  @EnableCaching
  @SpringBootApplication
  public class DemoApplication { ... }
  ```

- **Pagination**: Thêm phân trang vào `ProductRepository`:

  ```java
  Page<Product> findAll(Pageable pageable);
  ```

  Cập nhật `ProductService` và `ProductController`:

  ```java
  @GetMapping
  public Page<Product> getAllProducts(@PageableDefault(size = 10) Pageable pageable) {
      return productService.getAllProducts(pageable);
  }
  ```

- **Connection Pooling**: Sử dụng HikariCP (mặc định trong Spring Boot) và cấu hình:
  ```properties
  spring.datasource.hikari.maximum-pool-size=10
  spring.datasource.hikari.minimum-idle=5
  ```

### 3.2. Tối Ưu Bảo Mật

- **HTTPS**: Cấu hình SSL trong `application.properties`:
  ```properties
  server.ssl.key-store=classpath:keystore.p12
  server.ssl.key-store-password=your_password
  server.ssl.key-alias=alias
  server.ssl.enabled=true
  ```
- **CSRF**: Kích hoạt CSRF trong `SecurityConfig` (bị tắt ở phần trước):
  ```java
  http.csrf().csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse());
  ```
- **Input Sanitization**: Sử dụng `@SafeHtml` từ Hibernate Validator để ngăn XSS.
- **Rate Limiting**: Sử dụng thư viện như `Bucket4j` để giới hạn yêu cầu.

**So với FastAPI**:

- Spring có các công cụ bảo mật tích hợp (Spring Security), nhưng cấu hình phức tạp hơn.
- FastAPI yêu cầu thư viện bên ngoài (như `slowapi` cho rate limiting), nhưng dễ triển khai hơn.

## 4. Best Practices và Clean Architecture

### 4.1. Best Practices

- **Phân tầng rõ ràng**:
  - **Controller**: Xử lý HTTP request/response.
  - **Service**: Chứa logic nghiệp vụ.
  - **Repository**: Truy cập dữ liệu.
- **DTOs**: Sử dụng Data Transfer Objects để tách biệt API và entity:
  ```java
  public class ProductDTO {
      private String name;
      private double price;
      // Getters, Setters
  }
  ```
- **Logging**: Sử dụng SLF4J với Logback:

  ```java
  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;

  @Service
  public class ProductService {
      private static final Logger logger = LoggerFactory.getLogger(ProductService.class);
      public Product createProduct(Product product) {
          logger.info("Creating product: {}", product.getName());
          return productRepository.save(product);
      }
  }
  ```

- **Testing**: Viết unit test và integration test:

  ```java
  @SpringBootTest
  class ProductServiceTest {
      @Autowired
      private ProductService service;

      @Test
      void shouldCreateProduct() {
          Product product = new Product("Test", 99.99);
          Product saved = service.createProduct(product);
          assertNotNull(saved.getId());
      }
  }
  ```

### 4.2. Clean Architecture

Áp dụng Clean Architecture để đảm bảo mã dễ bảo trì:

- **Entities**: `Product` (tầng Domain, chứa logic nghiệp vụ cốt lõi).
- **Use Cases**: `ProductService` (tầng Application, điều phối logic).
- **Interfaces**: `ProductRepository` (tầng Infrastructure, giao tiếp với database).
- **Controllers**: `ProductController` (tầng Interface, xử lý HTTP).

Cấu trúc thư mục:

```
src/main/java/com/example/demo/
├── domain/
│   └── Product.java
├── application/
│   └── ProductService.java
├── infrastructure/
│   └── ProductRepository.java
├── interfaces/
│   └── ProductController.java
```

**So với FastAPI**:

- Spring hỗ trợ Clean Architecture tốt hơn nhờ hệ sinh thái Java và annotations.
- FastAPI thường sử dụng cấu trúc đơn giản hơn, nhưng có thể áp dụng Clean Architecture với effort lớn hơn (như trong dự án AI bạn hỏi trước đây).

## 5. Kết Luận

Phần này hoàn thành tài liệu Spring Boot với:

- Tích hợp MySQL/PostgreSQL.
- Xử lý bất đồng bộ với WebFlux.
- Tối ưu hóa hiệu suất (caching, pagination) và bảo mật (SSL, CSRF).
- Best practices và Clean Architecture.

**So sánh cuối với FastAPI**:

- **Spring Boot**: Phù hợp với ứng dụng doanh nghiệp lớn, tích hợp sâu với Java, nhưng phức tạp hơn. Lý tưởng cho dự án cần cấu trúc rõ ràng và hệ sinh thái mạnh.
- **FastAPI**: Nhẹ, nhanh, dễ học, lý tưởng cho microservices và API đơn giản. Tuy nhiên, ít tích hợp sẵn hơn so với Spring.

Tài liệu này cung cấp nền tảng để bạn phát triển ứng dụng Spring Boot chuyên nghiệp. Nếu cần thêm ví dụ hoặc giải thích, hãy yêu cầu cụ thể!
