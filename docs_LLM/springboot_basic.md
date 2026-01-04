**Lộ trình 10 phần của chúng ta sẽ như sau (dự kiến):**

1.  **Phần 1: Khởi tạo dự án và "Hello World" với Spring Boot.** (Phần này)
2.  **Phần 2: Thiết lập Database và làm việc với Entities (JPA).** (Product, Category, User)
3.  **Phần 3: Xây dựng Repository Layer với Spring Data JPA.** (CRUD cơ bản cho Product)
4.  **Phần 4: Xây dựng Service Layer và Business Logic.** (Tách biệt logic, dependency injection)
5.  **Phần 5: Xây dựng Controller Layer và hiển thị dữ liệu cơ bản với Thymeleaf.** (Hiển thị danh sách sản phẩm)
6.  **Phần 6: Quản lý người dùng và Bảo mật với Spring Security.** (Đăng ký, đăng nhập, phân quyền cơ bản)
7.  **Phần 7: Xây dựng tính năng Giỏ hàng (Shopping Cart).**
8.  **Phần 8: Xây dựng tính năng Đặt hàng (Order Processing).**
9.  **Phần 9: Validation, Exception Handling và Data Transfer Objects (DTOs).** (Làm cho ứng dụng "xịn" hơn)
10. **Phần 10: Xây dựng REST APIs cơ bản và tổng kết, gợi ý hướng phát triển.**

Chúng ta sẽ cố gắng sử dụng H2 Database (in-memory) cho giai đoạn đầu để dễ dàng phát triển, sau đó có thể chuyển sang MySQL hoặc PostgreSQL nếu bạn muốn.

---

**Phần 1: Khởi tạo dự án và "Hello World" với Spring Boot**

Chào mừng bạn đến với phần đầu tiên! Trong phần này, chúng ta sẽ cùng nhau:

1.  Tìm hiểu sơ lược về Spring Boot.
2.  Chuẩn bị môi trường.
3.  Sử dụng Spring Initializr để tạo dự án.
4.  Import dự án vào IntelliJ IDEA (hoặc IDE bạn chọn).
5.  Tìm hiểu cấu trúc thư mục cơ bản.
6.  Viết một REST Controller đơn giản để trả về "Hello, Spring Boot E-commerce!".
7.  Chạy thử ứng dụng.

**1. Spring Boot là gì?**

Spring Boot là một framework phát triển ứng dụng Java dựa trên Spring Framework. Nó giúp đơn giản hóa quá trình cấu hình và triển khai ứng dụng Spring bằng cách:

- **Auto-configuration:** Tự động cấu hình nhiều thành phần dựa trên các thư viện (dependencies) bạn thêm vào.
- **Standalone:** Tạo ra các ứng dụng có thể chạy độc lập (ví dụ, với embedded server như Tomcat, Jetty hoặc Undertow).
- **Opinionated:** Cung cấp một bộ cấu hình "có chính kiến" (mặc định) tốt, giúp bạn bắt đầu nhanh chóng, nhưng vẫn linh hoạt để tùy chỉnh.

Nói tóm lại, Spring Boot giúp bạn tập trung vào việc viết business logic thay vì lo lắng về cấu hình phức tạp.

**2. Chuẩn bị môi trường:**

- **JDK (Java Development Kit):** Phiên bản 17 trở lên là khuyến nghị (tuy nhiên 11 hoặc 8 vẫn phổ biến). Kiểm tra bằng cách gõ `java -version` trong terminal/command prompt. Nếu chưa có, hãy tải và cài đặt từ trang chủ Oracle hoặc AdoptOpenJDK (Temurin).
- **Maven hoặc Gradle:** Công cụ quản lý dependency và build dự án. Maven phổ biến hơn với người mới bắt đầu. Spring Boot hỗ trợ cả hai. Chúng ta sẽ dùng Maven. Thường thì Maven đi kèm với IDE.
- **IDE (Integrated Development Environment):** IntelliJ IDEA (Community Edition là đủ) được khuyến nghị mạnh mẽ cho phát triển Spring Boot. Eclipse hoặc VS Code với Java extensions cũng là lựa chọn tốt.

**3. Sử dụng Spring Initializr để tạo dự án:**

Spring Initializr là một công cụ web giúp bạn tạo nhanh cấu trúc dự án Spring Boot.

- Truy cập: [https://start.spring.io/](https://start.spring.io/)
- Cấu hình như sau:

  - **Project:** Maven Project
  - **Language:** Java
  - **Spring Boot:** Chọn phiên bản ổn định mới nhất (ví dụ: 3.x.x, không chọn SNAPSHOT hay M).
  - **Project Metadata:**
    - **Group:** `com.mycompany` (hoặc tên miền đảo ngược của bạn, ví dụ `vn.yourname.ecommerce`)
    - **Artifact:** `ecommerce-project` (tên dự án của bạn)
    - **Name:** `ecommerce-project` (thường giống Artifact)
    - **Description:** `Basic E-commerce project with Spring Boot`
    - **Package name:** `com.mycompany.ecommerceproject` (sẽ tự động sinh ra từ Group và Artifact)
    - **Packaging:** Jar (để tạo ứng dụng standalone với embedded server)
    - **Java:** Chọn phiên bản Java bạn đã cài (ví dụ: 17).
  - **Dependencies:** Nhấn nút "ADD DEPENDENCIES..."
    - Tìm và chọn `Spring Web`: Để xây dựng ứng dụng web, bao gồm RESTful applications, sử dụng Spring MVC. Nó cũng tích hợp sẵn Tomcat làm embedded server mặc định.
    - Tìm và chọn `Spring Boot DevTools`: Cung cấp các tính năng hữu ích khi phát triển như tự động restart ứng dụng khi code thay đổi, live reload. Rất tiện lợi!
    - _(Trong các phần sau, chúng ta sẽ thêm các dependencies khác như Spring Data JPA, Spring Security, Thymeleaf, H2 Database, v.v.)_

- Sau khi cấu hình xong, nhấn nút **"GENERATE"**. Một file `.zip` sẽ được tải về.

**4. Import dự án vào IntelliJ IDEA:**

- Giải nén file `.zip` vừa tải về vào một thư mục trên máy tính của bạn.
- Mở IntelliJ IDEA.
- Chọn **File > Open...** (hoặc "Open" từ màn hình chào mừng).
- Trỏ đến thư mục bạn vừa giải nén (thư mục chứa file `pom.xml`).
- IntelliJ IDEA sẽ nhận diện đây là một dự án Maven và tự động tải các dependencies cần thiết. Quá trình này có thể mất vài phút tùy thuộc vào tốc độ mạng.

**5. Tìm hiểu cấu trúc thư mục cơ bản:**

Sau khi import, bạn sẽ thấy cấu trúc thư mục chính như sau:

```
ecommerce-project
├── .mvn/
├── HELP.md
├── mvnw
├── mvnw.cmd
├── pom.xml               <-- File quản lý project và dependencies (Maven)
└── src
    ├── main
    │   ├── java
    │   │   └── com
    │   │       └── mycompany
    │   │           └── ecommerceproject
    │   │               └── EcommerceProjectApplication.java  <-- File chính để chạy ứng dụng
    │   └── resources
    │       ├── application.properties    <-- File cấu hình ứng dụng (port, database, etc.)
    │       ├── static                    <-- Chứa các tài nguyên tĩnh (CSS, JS, images)
    │       └── templates                 <-- Chứa các template views (ví dụ: Thymeleaf)
    └── test
        └── java
            └── com
                └── mycompany
                    └── ecommerceproject
                        └── EcommerceProjectApplicationTests.java <-- File test
```

- `pom.xml`: "Project Object Model" - file cấu hình của Maven. Nó định nghĩa thông tin dự án, các thư viện phụ thuộc (dependencies), cách build dự án, v.v.
- `src/main/java`: Chứa code Java của ứng dụng.
  - `EcommerceProjectApplication.java`: Đây là class chính, điểm khởi đầu của ứng dụng Spring Boot. Nó chứa annotation `@SpringBootApplication`.
- `src/main/resources`: Chứa các tài nguyên không phải code Java.
  - `application.properties` (hoặc `application.yml`): File cấu hình chính của ứng dụng. Bạn có thể định nghĩa port server, cấu hình database, logging, v.v.
  - `static`: Thư mục cho các file tĩnh như CSS, JavaScript, hình ảnh.
  - `templates`: Thư mục cho các template engine như Thymeleaf, FreeMarker (chúng ta sẽ dùng Thymeleaf sau).
- `src/test/java`: Chứa code Java cho việc test ứng dụng.

**6. Viết một REST Controller đơn giản:**

Chúng ta sẽ tạo một API đơn giản trả về một chuỗi "Hello".

- Trong `src/main/java/com/mycompany/ecommerceproject`, tạo một package mới tên là `controller`. (Chuột phải vào `ecommerceproject` -> New -> Package).
- Trong package `controller` vừa tạo, tạo một class Java mới tên là `HelloController`. (Chuột phải vào `controller` -> New -> Java Class).

Nội dung file `HelloController.java`:

```java
package com.mycompany.ecommerceproject.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController // Đánh dấu class này là một Controller xử lý các HTTP request và trả về dữ liệu (thường là JSON/XML)
public class HelloController {

    // Ánh xạ HTTP GET request tới "/hello" vào method này
    @GetMapping("/hello")
    public String sayHello() {
        return "Hello, Spring Boot E-commerce! Welcome to Part 1.";
    }

    @GetMapping("/") // Ánh xạ HTTP GET request tới root path "/"
    public String home() {
        return "Welcome to the E-commerce Project Homepage!";
    }
}
```

Giải thích:

- `@RestController`: Đây là một annotation tiện lợi, kết hợp `@Controller` và `@ResponseBody`.
  - `@Controller`: Đánh dấu class này là một Spring MVC Controller, nơi xử lý các request đến.
  - `@ResponseBody`: Chỉ ra rằng giá trị trả về của method sẽ được ghi trực tiếp vào HTTP response body (không phải tên của một view).
- `@GetMapping("/hello")`: Annotation này ánh xạ các HTTP GET request có đường dẫn `/hello` tới phương thức `sayHello()`. Tương tự với `@GetMapping("/")`.

**7. Chạy thử ứng dụng:**

Có nhiều cách để chạy ứng dụng Spring Boot:

- **Từ IntelliJ IDEA:**
  1.  Mở file `EcommerceProjectApplication.java`.
  2.  Bạn sẽ thấy một biểu tượng tam giác màu xanh lá cây (▶️) bên cạnh khai báo class hoặc phương thức `main`. Nhấn vào đó và chọn "Run 'EcommerceProjectApplication.main()'".
  3.  Hoặc, chuột phải vào file trong Project Explorer và chọn "Run 'EcommerceProjectApplication.main()'".
- **Sử dụng Maven (từ Terminal/Command Prompt):**
  1.  Mở Terminal (trong IntelliJ IDEA: View > Tool Windows > Terminal, hoặc dùng terminal của hệ điều hành).
  2.  Điều hướng đến thư mục gốc của dự án (thư mục chứa `pom.xml`).
  3.  Gõ lệnh: `./mvnw spring-boot:run` (trên Linux/macOS) hoặc `mvnw.cmd spring-boot:run` (trên Windows).

Khi ứng dụng khởi động thành công, bạn sẽ thấy output trong Console tương tự như sau:

```
... (nhiều log)
... .s.b.w.embedded.tomcat.TomcatWebServer : Tomcat started on port(s): 8080 (http) with context path ''
... .e.EcommerceProjectApplication         : Started EcommerceProjectApplication in X.XXX seconds (JVM running for Y.YYY)
```

Dòng `Tomcat started on port(s): 8080 (http)` cho biết server đã chạy trên cổng 8080.

**Kiểm tra:**

Mở trình duyệt web và truy cập các địa chỉ sau:

- `http://localhost:8080/hello`
  - Bạn sẽ thấy: `Hello, Spring Boot E-commerce! Welcome to Part 1.`
- `http://localhost:8080/`
  - Bạn sẽ thấy: `Welcome to the E-commerce Project Homepage!`

Nếu bạn thấy các thông điệp này, xin chúc mừng! Bạn đã khởi tạo và chạy thành công ứng dụng Spring Boot đầu tiên của mình.

**Lưu ý về Spring Boot DevTools:**
Nếu bạn đã thêm `spring-boot-devtools`, hãy thử thay đổi chuỗi trả về trong `HelloController.java` (ví dụ: `"Hello World updated!"`), lưu file lại. Bạn sẽ thấy ứng dụng tự động khởi động lại trong console, và khi bạn refresh trình duyệt, nội dung mới sẽ hiển thị mà không cần bạn phải restart thủ công. Rất tiện!

---

**Kết thúc Phần 1**

Trong phần này, chúng ta đã:

- Thiết lập một dự án Spring Boot cơ bản.
- Tìm hiểu về cấu trúc dự án.
- Tạo một REST controller đơn giản.
- Chạy ứng dụng và thấy kết quả trên trình duyệt.

Đây là nền tảng vững chắc để chúng ta xây dựng các tính năng phức tạp hơn. Hãy dành thời gian xem lại code, cấu trúc thư mục và các khái niệm đã được giới thiệu.

**Trong Phần 2, chúng ta sẽ bắt đầu làm việc với cơ sở dữ liệu: "Thiết lập Database và làm việc với Entities (JPA) cho Product, Category và User."**

Tuyệt vời! Chúng ta sẽ đi sâu vào việc thiết lập cơ sở dữ liệu và tạo các "khuôn mẫu" cho dữ liệu của mình bằng JPA.

---

**Phần 2: Thiết lập Database và làm việc với Entities (JPA)**

Trong phần này, chúng ta sẽ:

1.  **Tìm hiểu về JPA, Hibernate và ORM.**
2.  **Chọn và cấu hình H2 Database (In-memory).**
3.  **Thêm các dependencies cần thiết.**
4.  **Thiết kế và tạo các Entities: `Category`, `Product`, `User`.**
5.  **Định nghĩa mối quan hệ giữa các Entities.**
6.  **Cấu hình JPA và Hibernate properties.**
7.  **Chạy ứng dụng và kiểm tra schema trong H2 Console.**
8.  **Giới thiệu Lombok để giảm boilerplate code.**
9.  **Best Practices và Lưu ý quan trọng khi làm việc với Entities.**

---

**1. Tìm hiểu về JPA, Hibernate và ORM**

- **ORM (Object-Relational Mapping - Ánh xạ Đối tượng-Quan hệ):**

  - Đây là một kỹ thuật lập trình cho phép bạn tương tác với cơ sở dữ liệu quan hệ (như MySQL, PostgreSQL, H2) bằng cách sử dụng các đối tượng trong ngôn ngữ lập trình của bạn (ở đây là Java).
  - Thay vì viết các câu lệnh SQL phức tạp để CREATE, READ, UPDATE, DELETE (CRUD) dữ liệu, bạn thao tác trực tiếp trên các đối tượng Java. ORM framework sẽ tự động chuyển đổi các thao tác này thành các câu lệnh SQL tương ứng.
  - **Lợi ích:**
    - **Giảm code SQL thủ công:** Tập trung vào logic nghiệp vụ.
    - **Tính độc lập với Database:** Dễ dàng chuyển đổi giữa các hệ quản trị CSDL khác nhau với ít thay đổi code (trong nhiều trường hợp).
    - **Mã nguồn dễ đọc và bảo trì hơn:** Vì làm việc với đối tượng gần gũi hơn.
    - **Tận dụng các tính năng của OOP:** Kế thừa, đa hình có thể được ánh xạ.

- **JPA (Java Persistence API):**

  - JPA là một **đặc tả (specification)** của Java, định nghĩa một tập hợp các API và metadata (sử dụng annotations hoặc XML) để quản lý sự ổn định (persistence) của dữ liệu trong các ứng dụng Java.
  - Nó không phải là một implementation cụ thể, mà là một **tiêu chuẩn**. Điều này có nghĩa là bạn có thể viết code theo JPA, và sau đó chọn một ORM framework cụ thể (như Hibernate, EclipseLink, OpenJPA) để thực thi các API đó.
  - **Tại sao dùng JPA?**
    - **Tính chuẩn hóa:** Cung cấp một cách tiếp cận tiêu chuẩn cho ORM trong Java.
    - **Tính trừu tượng:** Che giấu sự phức tạp của việc tương tác trực tiếp với CSDL và ORM framework cụ thể.
    - **Tính linh hoạt:** Cho phép thay đổi ORM provider mà không cần thay đổi nhiều code nghiệp vụ.

- **Hibernate:**
  - Hibernate là một **implementation** phổ biến và mạnh mẽ của đặc tả JPA. Nó là một ORM framework cung cấp đầy đủ các tính năng để ánh xạ các đối tượng Java tới các bảng trong cơ sở dữ liệu quan hệ và ngược lại.
  - Spring Boot, khi bạn thêm `spring-boot-starter-data-jpa`, mặc định sẽ sử dụng Hibernate làm JPA provider.

**Tóm lại:** Bạn viết code sử dụng các API của **JPA** (ví dụ: các annotations như `@Entity`, `@Id`). **Spring Data JPA** (sẽ tìm hiểu ở phần sau) cung cấp một lớp trừu tượng tiện lợi hơn nữa trên JPA. **Hibernate** là "động cơ" thực thi các yêu cầu JPA của bạn, chuyển đổi chúng thành các lệnh SQL để tương tác với cơ sở dữ liệu.

**2. Chọn và cấu hình H2 Database (In-memory)**

- **H2 Database là gì?**
  - H2 là một hệ quản trị cơ sở dữ liệu quan hệ (RDBMS) được viết bằng Java.
  - Nó có thể được nhúng (embedded) vào ứng dụng Java hoặc chạy như một server riêng biệt.
  - Hỗ trợ chế độ **in-memory**, nghĩa là dữ liệu chỉ tồn tại trong bộ nhớ và sẽ mất khi ứng dụng tắt.
- **Tại sao dùng H2 cho development?**
  - **Dễ cài đặt:** Không cần cài đặt server CSDL riêng biệt, chỉ cần thêm dependency.
  - **Nhanh chóng:** Hoạt động trong bộ nhớ nên rất nhanh cho việc test và phát triển.
  - **Tự động reset:** Dữ liệu được làm mới mỗi khi ứng dụng khởi động (ở chế độ in-memory), rất tiện cho việc thử nghiệm mà không lo lắng về dữ liệu "rác".
  - **H2 Console:** Cung cấp giao diện web để xem và thao tác với dữ liệu trong DB.
- **Cấu hình H2 trong `application.properties`:**
  Mở file `src/main/resources/application.properties` và thêm các dòng sau:

  ```properties
  # H2 Database Configuration
  spring.datasource.url=jdbc:h2:mem:testdb  # URL kết nối: jdbc:h2:mem:[tên_database]
  spring.datasource.driverClassName=org.h2.Driver # Driver class của H2
  spring.datasource.username=sa                 # Username mặc định
  spring.datasource.password=                  # Password mặc định (để trống)

  # H2 Console (cho phép truy cập giao diện web của H2)
  spring.h2.console.enabled=true
  # spring.h2.console.path=/h2-console        # Đường dẫn để truy cập H2 console (mặc định là /h2-console)
  # spring.h2.console.settings.trace=false
  # spring.h2.console.settings.web-allow-others=false # Chỉ cho phép truy cập từ localhost
  ```

  - `spring.datasource.url=jdbc:h2:mem:testdb`:
    - `jdbc:h2:mem:`: Chỉ định sử dụng H2 ở chế độ in-memory.
    - `testdb`: Là tên của database bạn muốn tạo. Bạn có thể đặt tên khác.
  - `spring.h2.console.enabled=true`: Kích hoạt H2 Console. Sau khi ứng dụng chạy, bạn có thể truy cập `http://localhost:8080/h2-console` (nếu port mặc định là 8080).

**3. Thêm các Dependencies cần thiết**

Mở file `pom.xml` của bạn. Chúng ta cần thêm hai dependencies quan trọng:

- **Spring Data JPA:** Để làm việc với JPA một cách dễ dàng hơn.
- **H2 Database:** Driver cho H2.

Trong thẻ `<dependencies>` của `pom.xml`, thêm vào (nếu chưa có hoặc kiểm tra lại):

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope> <!-- Chỉ cần thiết lúc runtime, không cần lúc compile ứng dụng chính (trừ khi bạn viết code H2-specific) -->
</dependency>

<!-- Lombok (thêm luôn để dùng cho Entities) -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional> <!-- Lombok là compile-time dependency, không cần thiết lúc runtime -->
</dependency>
```

**Quan trọng:** Sau khi sửa `pom.xml`, IntelliJ IDEA thường sẽ có một thông báo nhỏ ở góc trên bên phải hoặc một biểu tượng Maven (chữ M) hiện ra. Bạn cần nhấn vào đó để "Load Maven Changes" (hoặc "Reload All Maven Projects") để IDE tải các dependencies mới về.

**Cài đặt Plugin Lombok cho IDE:**
Nếu bạn chưa có, hãy cài đặt plugin Lombok cho IntelliJ IDEA (hoặc IDE của bạn):

- IntelliJ: File > Settings > Plugins > Marketplace > tìm "Lombok" và cài đặt. Sau đó, khởi động lại IDE.
- Cũng trong IntelliJ: File > Settings > Build, Execution, Deployment > Compiler > Annotation Processors > **Enable annotation processing** (đảm bảo ô này được tick).

**4. Thiết kế và tạo các Entities: `Category`, `Product`, `User`**

**Entity là gì?**
Trong JPA, một Entity là một class Java đơn giản (POJO - Plain Old Java Object) được ánh xạ tới một bảng trong cơ sở dữ liệu. Mỗi instance của entity tương ứng với một dòng trong bảng đó.

**Annotations JPA cơ bản:**

- `@Entity`: Đánh dấu class này là một JPA entity.
- `@Table(name = "ten_bang")`: (Tùy chọn) Chỉ định tên bảng trong CSDL. Nếu không có, JPA thường lấy tên class làm tên bảng (có thể viết thường hoặc giữ nguyên tùy provider). **Best practice:** Luôn chỉ định rõ tên bảng và sử dụng `snake_case` (ví dụ: `product_categories`).
- `@Id`: Đánh dấu một trường là khóa chính (primary key) của bảng.
- `@GeneratedValue(strategy = GenerationType.IDENTITY)`: Cấu hình cách khóa chính được sinh tự động.
  - `GenerationType.AUTO`: JPA provider tự chọn chiến lược phù hợp (mặc định).
  - `GenerationType.IDENTITY`: Dựa vào cột auto-increment của CSDL (phổ biến cho MySQL, SQL Server, H2).
  - `GenerationType.SEQUENCE`: Dựa vào sequence của CSDL (phổ biến cho Oracle, PostgreSQL).
  - `GenerationType.TABLE`: Sử dụng một bảng riêng để quản lý giá trị khóa chính.
  - Chúng ta sẽ dùng `IDENTITY` cho H2.
- `@Column(name = "ten_cot", nullable = false, length = 255, unique = true)`: (Tùy chọn) Tùy chỉnh thuộc tính của cột tương ứng trong CSDL.
  - `name`: Tên cột trong CSDL. **Best practice:** Dùng `snake_case` (ví dụ: `product_name`).
  - `nullable`: `true` (mặc định) nếu cột cho phép giá trị NULL, `false` nếu không.
  - `length`: Độ dài tối đa cho cột kiểu chuỗi (VARCHAR).
  - `unique`: `true` nếu giá trị trong cột này phải là duy nhất.
  - `precision`, `scale`: Dùng cho kiểu số thập phân (ví dụ: `BigDecimal`). `precision` là tổng số chữ số, `scale` là số chữ số sau dấu phẩy.
- `@Temporal(TemporalType.TIMESTAMP)`: Dùng cho các trường kiểu `java.util.Date` hoặc `java.util.Calendar` để chỉ định kiểu dữ liệu ngày giờ trong CSDL (`DATE`, `TIME`, `TIMESTAMP`). Với `java.time.LocalDate`, `java.time.LocalDateTime` (Java 8+), annotation này thường không cần thiết vì JPA 2.2+ hỗ trợ chúng tự động.

---

**Tạo package `entity`:**
Trong `src/main/java/com/mycompany/ecommerceproject`, tạo package `entity`.

**4.1. Entity `Category`**

Tạo file `Category.java` trong package `entity`:

```java
package com.mycompany.ecommerceproject.entity;

import jakarta.persistence.*; // Quan trọng: dùng jakarta.persistence cho Spring Boot 3+
// import javax.persistence.*; // Dùng javax.persistence cho Spring Boot 2.x

import lombok.Getter;
import lombok.Setter;
import lombok.NoArgsConstructor;
import lombok.AllArgsConstructor;

import java.util.Set;

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Entity
@Table(name = "categories") // Tên bảng trong CSDL
public class Category {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) // Khóa chính tự tăng
    private Long id;

    @Column(name = "category_name", nullable = false, unique = true, length = 100)
    private String name;

    @Column(length = 255)
    private String description;

    // Mối quan hệ One-to-Many với Product
    // Một Category có thể có nhiều Product
    // 'mappedBy = "category"' chỉ ra rằng trường 'category' trong Product entity là chủ sở hữu của mối quan hệ này
    // FetchType.LAZY: Các product sẽ không được tải ngay khi tải Category, chỉ khi nào được gọi đến (ví dụ: category.getProducts())
    // CascadeType.ALL: Các thao tác (PERSIST, MERGE, REMOVE, REFRESH, DETACH) trên Category sẽ được áp dụng cho các Product liên quan.
    // Hãy cẩn thận với CascadeType.ALL, đặc biệt là REMOVE.
    @OneToMany(mappedBy = "category", cascade = CascadeType.ALL, fetch = FetchType.LAZY, orphanRemoval = true)
    private Set<Product> products;

    // equals và hashCode, toString sẽ được Lombok xử lý hoặc bạn có thể tự override.
    // Nên cẩn thận khi dùng Lombok @Data hoặc @EqualsAndHashCode với JPA entities có quan hệ hai chiều
    // để tránh vòng lặp vô hạn (stackoverflow). Thường chỉ dùng @Getter, @Setter, @NoArgsConstructor, @AllArgsConstructor.
    // Nếu tự viết equals/hashCode, thường dựa trên business key (ví dụ: name) hoặc id (nếu đã được set).
}
```

- **`jakarta.persistence.*` vs `javax.persistence.*`:**
  - Nếu bạn đang dùng Spring Boot 3.x (sử dụng Spring Framework 6.x), nó sẽ yêu cầu Jakarta EE 9+, do đó bạn phải dùng `jakarta.persistence.*`.
  - Nếu bạn đang dùng Spring Boot 2.x (sử dụng Spring Framework 5.x), nó sẽ dùng Java EE, do đó bạn phải dùng `javax.persistence.*`.
  - Spring Initializr sẽ tự động chọn đúng cho bạn dựa trên phiên bản Spring Boot bạn chọn.

**4.2. Entity `Product`**

Tạo file `Product.java` trong package `entity`:

```java
package com.mycompany.ecommerceproject.entity;

import jakarta.persistence.*;
import lombok.Getter;
import lombok.Setter;
import lombok.NoArgsConstructor;
import lombok.AllArgsConstructor;

import java.math.BigDecimal; // Quan trọng: Dùng BigDecimal cho tiền tệ
import java.time.LocalDateTime; // Dùng java.time cho ngày giờ hiện đại

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Entity
@Table(name = "products")
public class Product {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, length = 200)
    private String name;

    @Lob // Dùng cho các trường văn bản dài (TEXT, CLOB)
    @Column(columnDefinition = "TEXT") // Một số DB cần chỉ định rõ kiểu dữ liệu lớn
    private String description;

    @Column(nullable = false, precision = 10, scale = 2) // 10 chữ số, 2 chữ số sau dấu phẩy
    private BigDecimal price;

    @Column(name = "image_url", length = 255)
    private String imageUrl;

    @Column(name = "stock_quantity", nullable = false)
    private Integer stockQuantity;

    @Column(name = "created_at", updatable = false) // Chỉ được set khi tạo mới, không update
    private LocalDateTime createdAt;

    @Column(name = "updated_at")
    private LocalDateTime updatedAt;

    // Mối quan hệ Many-to-One với Category
    // Nhiều Product có thể thuộc về một Category
    // FetchType.EAGER (mặc định cho ManyToOne) hoặc LAZY. EAGER có nghĩa là Category sẽ được tải cùng lúc với Product.
    // Với ManyToOne, EAGER thường ổn, nhưng nếu Category có nhiều thông tin và không phải lúc nào cũng cần, LAZY sẽ tốt hơn.
    @ManyToOne(fetch = FetchType.LAZY) // Nên để LAZY để tránh tải thừa, trừ khi luôn cần category
    @JoinColumn(name = "category_id", nullable = false) // Khóa ngoại trong bảng products, trỏ tới id của bảng categories
    private Category category;


    @PrePersist // Hàm này sẽ được gọi trước khi entity được lưu vào DB (lần đầu)
    protected void onCreate() {
        createdAt = LocalDateTime.now();
        updatedAt = LocalDateTime.now();
    }

    @PreUpdate // Hàm này sẽ được gọi trước khi entity được cập nhật
    protected void onUpdate() {
        updatedAt = LocalDateTime.now();
    }

    // Lưu ý về equals/hashCode và toString như với Category
}
```

- **`BigDecimal` cho `price`:** **BEST PRACTICE.** Không bao giờ dùng `float` hoặc `double` cho tiền tệ vì chúng có vấn đề về độ chính xác khi tính toán. `BigDecimal` cung cấp độ chính xác cao.
- **`@Lob`:** Dùng cho các trường văn bản có thể rất dài (ví dụ: mô tả sản phẩm chi tiết). Nó sẽ được ánh xạ tới kiểu `CLOB` hoặc `TEXT` trong CSDL.
- **`java.time.LocalDateTime`:** Dùng cho các trường ngày giờ. Đây là API ngày giờ hiện đại của Java 8+, tốt hơn `java.util.Date`.
- **`@PrePersist` và `@PreUpdate`:** Đây là các JPA callback lifecycle methods. Chúng cho phép bạn thực thi một số logic ngay trước khi một entity được lưu (persist) hoặc cập nhật (update). Rất hữu ích để tự động set các trường như `createdAt` và `updatedAt`.

**4.3. Entity `User`**

Tạo file `User.java` trong package `entity` (tạm thời chúng ta sẽ đặt tên class là `UserAccount` để tránh trùng với class `User` của Spring Security sau này, hoặc bạn có thể dùng tên `User` và sau này chỉ định đầy đủ package khi cần). Chúng ta sẽ dùng `UserAccount` cho rõ ràng.

```java
package com.mycompany.ecommerceproject.entity;

import jakarta.persistence.*;
import lombok.Getter;
import lombok.Setter;
import lombok.NoArgsConstructor;
import lombok.AllArgsConstructor;

import java.time.LocalDateTime;

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Entity
@Table(name = "user_accounts") // Đặt tên bảng là user_accounts
public class UserAccount {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true, length = 50)
    private String username;

    @Column(nullable = false, length = 255) // Password sẽ được hash, nên cần đủ dài
    private String password;

    @Column(nullable = false, unique = true, length = 100)
    private String email;

    @Column(name = "first_name", length = 50)
    private String firstName;

    @Column(name = "last_name", length = 50)
    private String lastName;

    @Column(length = 20)
    private String role; // Ví dụ: "ROLE_USER", "ROLE_ADMIN". Sẽ cải thiện sau này bằng Enum hoặc bảng Role riêng.

    @Column(name = "created_at", updatable = false)
    private LocalDateTime createdAt;

    @Column(name = "updated_at")
    private LocalDateTime updatedAt;

    @PrePersist
    protected void onCreate() {
        createdAt = LocalDateTime.now();
        updatedAt = LocalDateTime.now();
        if (this.role == null) { // Gán vai trò mặc định nếu chưa có
            this.role = "ROLE_USER";
        }
    }

    @PreUpdate
    protected void onUpdate() {
        updatedAt = LocalDateTime.now();
    }
    // Lưu ý về equals/hashCode và toString
}
```

- **`role` (String):** Hiện tại, chúng ta dùng một trường String đơn giản. Trong một ứng dụng thực tế, bạn nên dùng `Enum` hoặc một bảng `Role` riêng và tạo mối quan hệ `Many-to-Many` giữa `User` và `Role` để quản lý quyền hạn linh hoạt hơn. Chúng ta sẽ cải thiện điều này sau.

---

**5. Định nghĩa mối quan hệ giữa các Entities**

Chúng ta đã bắt đầu định nghĩa mối quan hệ trong các entity ở trên. Giờ hãy xem kỹ hơn:

- **Các loại quan hệ chính:**

  - `@OneToOne`: Một-Một (ví dụ: `User` và `UserProfile`).
  - `@OneToMany`: Một-Nhiều (ví dụ: `Category` có nhiều `Product`).
  - `@ManyToOne`: Nhiều-Một (ví dụ: `Product` thuộc về một `Category`).
  - `@ManyToMany`: Nhiều-Nhiều (ví dụ: `Product` có thể thuộc nhiều `Tag`, và một `Tag` có thể gắn cho nhiều `Product`; hoặc `User` có nhiều `Role`).

- **Quan hệ `Product` và `Category`:**

  - **Từ `Product` đến `Category` (`@ManyToOne`):**

    ```java
    // Trong Product.java
    @ManyToOne(fetch = FetchType.LAZY) // Quan trọng: nên LAZY
    @JoinColumn(name = "category_id", nullable = false) // Tạo cột category_id trong bảng products
    private Category category;
    ```

    - `@ManyToOne`: Chỉ ra rằng nhiều `Product` có thể liên kết với một `Category`.
    - `fetch = FetchType.LAZY`: **Best practice.** Nghĩa là thông tin `Category` sẽ không được tải từ CSDL ngay khi một `Product` được tải. Nó chỉ được tải khi bạn gọi `product.getCategory()`. Điều này giúp tránh tải dữ liệu không cần thiết, cải thiện hiệu suất. `FetchType.EAGER` (mặc định cho `@ManyToOne`) sẽ tải `Category` cùng lúc, có thể hữu ích nếu bạn luôn cần thông tin `Category` mỗi khi làm việc với `Product`, nhưng thường thì `LAZY` an toàn hơn.
    - `@JoinColumn(name = "category_id", nullable = false)`: Định nghĩa khóa ngoại trong bảng `products`.
      - `name = "category_id"`: Tên của cột khóa ngoại trong bảng `products`. Cột này sẽ lưu `id` của `Category` tương ứng.
      - `nullable = false`: Mỗi `Product` phải thuộc về một `Category`.

  - **Từ `Category` đến `Product` (`@OneToMany`):**
    ```java
    // Trong Category.java
    @OneToMany(mappedBy = "category", cascade = CascadeType.ALL, fetch = FetchType.LAZY, orphanRemoval = true)
    private Set<Product> products; // Dùng Set để tránh trùng lặp product
    ```
    - `@OneToMany`: Chỉ ra rằng một `Category` có thể liên kết với nhiều `Product`.
    - `mappedBy = "category"`: **Rất quan trọng cho quan hệ hai chiều (bidirectional).** Nó chỉ ra rằng mối quan hệ này đã được "quản lý" (owned) bởi phía `Product`, cụ thể là bởi trường `category` trong class `Product`. JPA sẽ không tạo thêm cột khóa ngoại nào trong bảng `categories` cho mối quan hệ này; thay vào đó, nó sẽ sử dụng thông tin từ `category_id` trong bảng `products`. Nếu không có `mappedBy`, JPA sẽ mặc định tạo một bảng trung gian (join table) cho quan hệ One-to-Many, điều này thường không mong muốn cho trường hợp này.
    - `cascade = CascadeType.ALL`:
      - `CascadeType` định nghĩa các thao tác trên entity cha (ở đây là `Category`) sẽ được lan truyền (cascade) tới các entity con liên quan (ở đây là `Product`s).
      - `ALL` bao gồm: `PERSIST` (lưu), `MERGE` (cập nhật), `REMOVE` (xóa), `REFRESH` (làm mới từ DB), `DETACH` (tách khỏi persistence context).
      - **Thận trọng:** `CascadeType.REMOVE` (hoặc `ALL`) có nghĩa là nếu bạn xóa một `Category`, tất cả các `Product` thuộc `Category` đó cũng sẽ bị xóa. Điều này có thể đúng hoặc không tùy theo logic nghiệp vụ. Hãy cân nhắc kỹ. Đôi khi bạn chỉ muốn set `category_id` của các Product đó thành `null` (nếu `nullable=true` cho phép) hoặc gán chúng cho một category "Mặc định".
      - Trong trường hợp này, nếu một Category bị xóa, việc xóa các sản phẩm thuộc về nó có thể là hợp lý.
    - `fetch = FetchType.LAZY`: **BEST PRACTICE cho collections.** Nghĩa là danh sách `products` sẽ không được tải từ CSDL ngay khi một `Category` được tải. Nó chỉ được tải khi bạn gọi `category.getProducts()`. Điều này cực kỳ quan trọng để tránh "N+1 select problem" và tải một lượng lớn dữ liệu không cần thiết. `FetchType.EAGER` (mặc định cho `@OneToMany` là LAZY, nhưng `@ManyToMany` mặc định là LAZY) sẽ tải tất cả `Product`s cùng lúc, có thể gây ra vấn đề hiệu suất nghiêm trọng.
    - `orphanRemoval = true`: Nếu một `Product` bị xóa khỏi collection `products` của `Category` (ví dụ: `category.getProducts().remove(product)`) và `Product` đó không còn được tham chiếu bởi `Category` này nữa (trở thành "mồ côi"), thì `Product` đó cũng sẽ bị xóa khỏi CSDL. Điều này khác với `CascadeType.REMOVE` (chỉ kích hoạt khi `Category` bị xóa). Hữu ích khi bạn muốn đảm bảo rằng các entity con không tồn tại độc lập nếu không còn thuộc về cha.

- **Các mối quan hệ khác (sẽ xây dựng sau nếu cần):**
  - `UserAccount` và `Order`: Một `UserAccount` có thể có nhiều `Order` (`@OneToMany`), một `Order` thuộc về một `UserAccount` (`@ManyToOne`).
  - `Order` và `OrderItem` (hoặc `Product` thông qua bảng trung gian `OrderProduct`): Một `Order` có nhiều `OrderItem`. Một `OrderItem` chứa thông tin về một `Product` cụ thể trong đơn hàng đó (số lượng, giá tại thời điểm mua). Đây thường là quan hệ `@OneToMany` từ `Order` đến `OrderItem`, và `@ManyToOne` từ `OrderItem` đến `Product`.
  - Có thể có quan hệ `ManyToMany` giữa `Product` và `Tag` (nếu bạn muốn thêm tag cho sản phẩm).

---

**6. Cấu hình JPA và Hibernate properties**

Quay lại file `src/main/resources/application.properties` và thêm/cập nhật các cấu hình sau:

```properties
# ... (các cấu hình H2 ở trên) ...

# JPA / Hibernate Configuration
spring.jpa.hibernate.ddl-auto=update # Chiến lược tạo/cập nhật schema CSDL
# Các giá trị phổ biến cho ddl-auto:
# none: Không làm gì cả. DB schema phải tồn tại. Dùng cho production.
# validate: Kiểm tra schema với entities, báo lỗi nếu không khớp. An toàn cho production.
# update: Tự động cập nhật schema (thêm cột, bảng mới) nếu có thay đổi. KHÔNG xóa cột/bảng. Dùng cho development, cẩn thận.
# create: Xóa schema cũ và tạo mới mỗi lần ứng dụng khởi động. Dữ liệu cũ sẽ mất. Dùng cho development/test.
# create-drop: Tạo schema khi khởi động và xóa khi ứng dụng tắt. Dùng cho test.

spring.jpa.show-sql=true                     # Hiển thị câu lệnh SQL được Hibernate sinh ra trong console
spring.jpa.properties.hibernate.format_sql=true # Format câu lệnh SQL cho dễ đọc
spring.jpa.properties.hibernate.use_sql_comments=true # Thêm comment vào SQL để dễ debug (tùy chọn)

# (Tùy chọn) Để tương thích tốt hơn với các tên cột snake_case từ entity camelCase
# Spring Boot 3.x mặc định đã có chiến lược đặt tên tốt hơn (PhysicalNamingStrategy)
# spring.jpa.hibernate.naming.physical-strategy=org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
# Nếu bạn muốn snake_case một cách rõ ràng hơn cho mọi thứ (table và column):
# spring.jpa.hibernate.naming.physical-strategy=org.hibernate.boot.model.naming.CamelCaseToUnderscoresNamingStrategy (Hibernate 5)
# Đối với Hibernate 6 (Spring Boot 3+): org.hibernate.boot.model.naming.ImplicitNamingStrategyJpaCompliantImpl là mặc định
# và nó xử lý tên theo đặc tả JPA.
# Nếu bạn đã đặt tên rõ ràng bằng @Table(name="...") và @Column(name="..."), thì PhysicalNamingStrategy ít quan trọng hơn.

# (Tùy chọn cho H2) Để giữ dữ liệu giữa các lần khởi động khi dùng file-based H2 (thay vì in-memory)
# spring.datasource.url=jdbc:h2:file:./data/testdb_file # Lưu vào file
# spring.jpa.hibernate.ddl-auto=update # Phải là update hoặc validate, không phải create
```

- `spring.jpa.hibernate.ddl-auto=update`:
  - Trong giai đoạn phát triển với H2 in-memory, `create` hoặc `update` đều ổn. `create` đảm bảo schema luôn "sạch" mỗi lần chạy. `update` cố gắng cập nhật schema hiện có nếu có thay đổi trong entity (ví dụ: thêm cột mới).
  - **CẢNH BÁO:** **KHÔNG BAO GIỜ** sử dụng `create` hoặc `create-drop` trong môi trường **PRODUCTION** vì nó sẽ xóa toàn bộ dữ liệu của bạn! `update` cũng nên dùng cẩn thận trong production (tốt nhất là dùng các công cụ migration schema như Flyway hoặc Liquibase cho production). Trong production, `validate` hoặc `none` là an toàn nhất.
- `spring.jpa.show-sql=true`: Rất hữu ích khi phát triển để xem các câu lệnh SQL mà Hibernate sinh ra. Giúp bạn hiểu cách ORM hoạt động và debug các vấn đề liên quan đến query.
- `spring.jpa.properties.hibernate.format_sql=true`: Làm cho output SQL dễ đọc hơn.

---

**7. Chạy ứng dụng và kiểm tra schema trong H2 Console**

Bây giờ, hãy chạy ứng dụng Spring Boot của bạn (từ IntelliJ hoặc dùng Maven như Phần 1).

- **Kiểm tra Console Output:**
  Bạn sẽ thấy rất nhiều log từ Hibernate, bao gồm cả các câu lệnh SQL `CREATE TABLE` nếu `ddl-auto` được đặt là `create` hoặc `update` (và bảng chưa tồn tại/khác biệt). Ví dụ:

  ```sql
  Hibernate: create table categories (id bigint generated by default as identity, category_name varchar(100) not null, description varchar(255), primary key (id))
  Hibernate: alter table if exists categories add constraint UK_t8o6pivur7dstu40o4qjauqqe unique (category_name)
  Hibernate: create table products (category_id bigint not null, created_at timestamp(6), id bigint generated by default as identity, stock_quantity integer not null, updated_at timestamp(6), image_url varchar(255), name varchar(200) not null, price numeric(10,2) not null, description TEXT, primary key (id))
  Hibernate: create table user_accounts (created_at timestamp(6), id bigint generated by default as identity, updated_at timestamp(6), email varchar(100) not null, first_name varchar(50), last_name varchar(50), password varchar(255) not null, role varchar(20), username varchar(50) not null, primary key (id))
  Hibernate: alter table if exists user_accounts add constraint UK_hl02wv5h0s8t8n6x55k6u27k4 unique (email)
  Hibernate: alter table if exists user_accounts add constraint UK_r2Qk728m055p617ffm1th6s0b unique (username)
  Hibernate: alter table if exists products add constraint FKog2rp4qthbtt2lfyhfo32lsw9 foreign key (category_id) references categories
  ```

  Điều này xác nhận rằng Hibernate đã tạo các bảng dựa trên entities của bạn.

- **Truy cập H2 Console:**

  1.  Mở trình duyệt và đi đến `http://localhost:8080/h2-console` (hoặc đường dẫn bạn đã cấu hình cho `spring.h2.console.path`).
  2.  Bạn sẽ thấy một trang đăng nhập. **Rất quan trọng:**
      - **Saved Settings:** Chọn `Generic H2 (Embedded)` nếu có.
      - **JDBC URL:** Đảm bảo nó **CHÍNH XÁC** giống với giá trị bạn đã đặt trong `spring.datasource.url` của file `application.properties`. Trong trường hợp của chúng ta là `jdbc:h2:mem:testdb`. Nếu sai, bạn sẽ kết nối tới một database khác (hoặc tạo một DB mới trống rỗng) và không thấy bảng của mình.
      - **User Name:** `sa` (hoặc username bạn đã cấu hình).
      - **Password:** Để trống (hoặc password bạn đã cấu hình).
  3.  Nhấn **Connect**.

  Bên trái, bạn sẽ thấy danh sách các bảng đã được tạo: `CATEGORIES`, `PRODUCTS`, `USER_ACCOUNTS`. Bạn có thể click vào tên bảng để xem cấu trúc hoặc viết câu lệnh SQL (ví dụ: `SELECT * FROM PRODUCTS;`) để truy vấn. Hiện tại các bảng sẽ trống.

---

**8. Giới thiệu Lombok để giảm boilerplate code (đã tích hợp ở trên)**

Như bạn đã thấy, chúng ta đã sử dụng các annotation của Lombok: `@Getter`, `@Setter`, `@NoArgsConstructor`, `@AllArgsConstructor` trong các entity.

- **Lombok là gì?**
  Project Lombok là một thư viện Java giúp giảm thiểu code nhàm chán (boilerplate code) như getters, setters, constructors, `equals()`, `hashCode()`, `toString()` bằng cách sử dụng các annotations. Code của bạn sẽ trở nên gọn gàng hơn rất nhiều.
- **Các annotations phổ biến:**
  - `@Getter` / `@Setter`: Tự động tạo các phương thức getter/setter cho tất cả các trường không tĩnh.
  - `@NoArgsConstructor`: Tự động tạo một constructor không tham số. JPA yêu cầu entity phải có một constructor không tham số (public hoặc protected).
  - `@AllArgsConstructor`: Tự động tạo một constructor với tất cả các trường của class làm tham số.
  - `@ToString`: Tự động override phương thức `toString()`.
  - `@EqualsAndHashCode`: Tự động override `equals()` và `hashCode()`. **Cảnh báo:** Khi dùng với JPA entities có mối quan hệ (đặc biệt là hai chiều), việc sử dụng `@EqualsAndHashCode` mặc định của Lombok (dựa trên tất cả các trường) có thể gây ra `StackOverflowError` do vòng lặp vô hạn khi tính toán hashcode/equals qua các đối tượng liên kết, hoặc vấn đề với proxy của Hibernate.
    - **Best practice cho `equals()` và `hashCode()` trong Entities:**
      - Nếu entity có một "business key" (một trường hoặc tập hợp các trường tự nhiên định danh duy nhất cho entity, không phải là ID tự tăng), hãy dùng nó. Ví dụ: `@EqualsAndHashCode(onlyExplicitlyIncluded = true)` và đánh dấu trường business key với `@EqualsAndHashCode.Include`.
      - Nếu không có business key rõ ràng, việc implement `equals()`/`hashCode()` dựa trên `@Id` có thể phức tạp, vì `id` có thể là `null` trước khi entity được persist.
      - Một cách tiếp cận an toàn là không dựa vào `id` nếu nó được sinh tự động và có thể `null`. Hoặc, nếu `id` không `null`, thì dựa vào `id`.
      - Thường thì đối với các entity đơn giản, việc không override `equals/hashCode` (để nó dùng `Object.equals/hashCode` mặc định - so sánh tham chiếu) là chấp nhận được nếu bạn không đặt chúng vào `Set` hoặc dùng làm key trong `Map` _trước khi_ chúng được persist và có ID.
      - Đối với ví dụ này, chúng ta sẽ tạm thời không dùng `@EqualsAndHashCode` của Lombok để tránh phức tạp.
  - `@Data`: Là một tổ hợp của `@Getter`, `@Setter`, `@RequiredArgsConstructor`, `@ToString`, `@EqualsAndHashCode`. **Không nên dùng `@Data` trực tiếp cho JPA entities** vì lý do `equals/hashCode` và `toString` có thể gây vấn đề với lazy loading và các mối quan hệ.
  - `@Builder`: Cung cấp pattern Builder để tạo đối tượng một cách linh hoạt.

Chúng ta đã dùng `@Getter`, `@Setter`, `@NoArgsConstructor`, `@AllArgsConstructor`, điều này đã giúp các class entity gọn gàng hơn nhiều.

---

**9. Best Practices và Lưu ý quan trọng khi làm việc với Entities**

- **Đặt tên (Convention):**
  - **Class Entity:** `PascalCase` (ví dụ: `ProductCategory`).
  - **Trường (Fields) trong Entity:** `camelCase` (ví dụ: `productName`).
  - **Tên bảng trong CSDL (`@Table(name = ...)`):** `snake_case` và số nhiều (ví dụ: `product_categories`).
  - **Tên cột trong CSDL (`@Column(name = ...)`):** `snake_case` (ví dụ: `product_name`).
- **Khóa chính (`@Id`):**
  - Nên là kiểu `Long` (wrapper type) thay vì `long` (primitive type), vì `id` có thể là `null` trước khi entity được persist.
  - Nên để JPA/Hibernate quản lý việc sinh khóa chính (`@GeneratedValue`).
- **Kiểu dữ liệu:**
  - Dùng wrapper types (`Integer`, `Double`, `Boolean`) thay vì primitive types (`int`, `double`, `boolean`) cho các trường có thể `null` trong CSDL. Điều này giúp phân biệt rõ ràng giữa giá trị `0` (hoặc `false`) và `NULL`.
  - Dùng `BigDecimal` cho tiền tệ.
  - Dùng `java.time` (LocalDate, LocalDateTime, ZonedDateTime) cho ngày giờ.
- **Mối quan hệ:**
  - Luôn ưu tiên `FetchType.LAZY` cho các collection (`@OneToMany`, `@ManyToMany`).
  - Cân nhắc `FetchType.LAZY` cho `@ManyToOne` và `@OneToOne` nếu entity liên quan không phải lúc nào cũng cần thiết.
  - Hiểu rõ `CascadeType` và sử dụng cẩn thận, đặc biệt là `REMOVE` và `ALL`.
  - Sử dụng `mappedBy` chính xác cho mối quan hệ hai chiều để chỉ định "owning side".
  - Sử dụng `Set` thay vì `List` cho collection ở phía "Many" nếu thứ tự không quan trọng và bạn muốn tránh trùng lặp (ví dụ: `Set<Product>` trong `Category`). Nếu thứ tự quan trọng, dùng `List` và có thể cần `@OrderColumn`.
- **`equals()` và `hashCode()`:** Như đã thảo luận ở phần Lombok, implement cẩn thận. Nếu không chắc chắn, hãy bắt đầu bằng việc không override chúng hoặc chỉ dựa trên business key nếu có.
- **`toString()`:** Cẩn thận khi override `toString()`. Nếu bạn in ra các trường là collection được fetch LAZY, nó có thể kích hoạt việc tải dữ liệu từ CSDL, có thể không mong muốn. Lombok `@ToString(exclude = {"products"})` có thể giúp loại trừ các trường này.
- **Không đặt logic nghiệp vụ phức tạp trong Entities:** Entities nên chủ yếu là các cấu trúc dữ liệu (data holders). Logic nghiệp vụ nên được đặt trong Service Layer (sẽ học ở phần sau). Các phương thức helper đơn giản hoặc các callback `@PrePersist`, `@PreUpdate` thì chấp nhận được.
- **Immutability (Bất biến):** Cân nhắc làm cho một số trường của entity là final (và chỉ set qua constructor) nếu giá trị của chúng không bao giờ thay đổi sau khi tạo, mặc dù điều này có thể phức tạp hơn với cách JPA hoạt động (cần constructor không tham số). ID thường là bất biến sau khi được gán.
- **Validation:** JPA có hỗ trợ Bean Validation (`@NotNull`, `@Size`, `@Min`, `@Max`, `@Email`, etc.). Chúng ta sẽ tìm hiểu thêm về validation ở phần sau.

---

**Kết thúc Phần 2**

Wow, đây là một phần khá dài và chi tiết! Chúng ta đã:

- Hiểu về ORM, JPA và Hibernate.
- Cấu hình H2 Database và H2 Console.
- Thêm các dependencies cần thiết.
- Tạo các entity `Category`, `Product`, `UserAccount` với các annotation JPA cơ bản và mối quan hệ Many-to-One/One-to-Many.
- Cấu hình JPA properties.
- Chạy ứng dụng và thấy các bảng được tạo trong H2 Console.
- Sử dụng Lombok để làm gọn code.
- Thảo luận về các best practices quan trọng.

Hãy dành thời gian xem lại code, các annotations, và các khái niệm. Việc hiểu rõ Entities là nền tảng cực kỳ quan trọng cho các phần tiếp theo.

**Trong Phần 3, chúng ta sẽ xây dựng Repository Layer với Spring Data JPA để thực hiện các thao tác CRUD (Create, Read, Update, Delete) trên các entity này một cách dễ dàng.**

Tuyệt vời! Chúng ta sẽ tiếp tục với việc xây dựng lớp Repository, nơi mà "phép thuật" của Spring Data JPA bắt đầu tỏa sáng, giúp chúng ta tương tác với cơ sở dữ liệu một cách cực kỳ đơn giản.

---

**Phần 3: Xây dựng Repository Layer với Spring Data JPA**

Trong phần này, chúng ta sẽ:

1.  **Giới thiệu Spring Data JPA và vai trò của nó.**
2.  **Tạo các Repository interfaces cho `Category`, `Product`, và `UserAccount`.**
3.  **Tìm hiểu về các interface cơ sở: `CrudRepository`, `PagingAndSortingRepository`, `JpaRepository`.**
4.  **Sử dụng các phương thức CRUD có sẵn.**
5.  **Viết các Query Methods (Derived Query Methods) – để Spring Data JPA tự sinh query.**
6.  **Sử dụng `@Query` annotation để định nghĩa các query tùy chỉnh (JPQL và Native SQL).**
7.  **Giới thiệu về Pagination và Sorting.**
8.  **Tạo một `CommandLineRunner` để thử nghiệm Repository: Chèn và truy vấn dữ liệu mẫu.**
9.  **Best Practices cho Repository Layer.**

---

**1. Giới thiệu Spring Data JPA và vai trò của nó**

- **Spring Data JPA là gì?**
  - Spring Data JPA là một phần của dự án Spring Data lớn hơn, nhằm mục đích làm cho việc truy cập dữ liệu trở nên dễ dàng và nhất quán hơn, bất kể nguồn dữ liệu cơ bản là gì (quan hệ, NoSQL, v.v.).
  - Cụ thể, Spring Data JPA giúp giảm đáng kể lượng code boilerplate cần thiết để triển khai tầng Data Access Layer (DAL) cho các ứng dụng sử dụng JPA.
- **Vai trò và lợi ích:**
  - **Giảm code DAO (Data Access Object):** Bạn không cần phải viết các implementation class cho repository interfaces của mình trong hầu hết các trường hợp. Spring Data JPA sẽ tự động tạo ra các implementation tại thời điểm runtime.
  - **Cung cấp các phương thức CRUD tiêu chuẩn:** Các thao tác như `save()`, `findById()`, `findAll()`, `delete()` được cung cấp sẵn.
  - **Derived Query Methods:** Cho phép bạn định nghĩa các query chỉ bằng cách đặt tên phương thức theo một quy ước nhất định. Spring Data JPA sẽ phân tích tên phương thức và tự động tạo ra query tương ứng. (Ví dụ: `findByName(String name)`).
  - **Query tùy chỉnh với `@Query`:** Hỗ trợ viết các query phức tạp hơn bằng JPQL (Java Persistence Query Language) hoặc SQL native.
  - **Hỗ trợ Pagination và Sorting:** Dễ dàng triển khai phân trang và sắp xếp kết quả.
  - **Tích hợp tốt với Spring Framework:** Quản lý transaction, dependency injection.

**Nói tóm lại:** Spring Data JPA giúp bạn tập trung vào _cái gì_ bạn muốn truy vấn, thay vì _làm thế nào_ để truy vấn nó.

**2. Tạo các Repository Interfaces**

Chúng ta sẽ tạo các interface repository trong một package mới.

- Trong `src/main/java/com/mycompany/ecommerceproject`, tạo một package mới tên là `repository`.

**2.1. `CategoryRepository`**

Tạo file `CategoryRepository.java` trong package `repository`:

```java
package com.mycompany.ecommerceproject.repository;

import com.mycompany.ecommerceproject.entity.Category;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository; // (Tùy chọn nhưng nên có để rõ ràng và quét component)

import java.util.Optional;

@Repository // Đánh dấu đây là một Spring bean, và là một Data Repository
public interface CategoryRepository extends JpaRepository<Category, Long> {
    // JpaRepository<EntityType, IdType>

    // Ví dụ về một derived query method:
    // Spring Data JPA sẽ tự động tạo query để tìm Category theo tên
    // SELECT c FROM Category c WHERE c.name = ?1
    Optional<Category> findByName(String name);

    // Bạn có thể thêm các query method khác ở đây nếu cần
    // Ví dụ:
    // List<Category> findByNameContainingIgnoreCase(String keyword);
}
```

**2.2. `ProductRepository`**

Tạo file `ProductRepository.java` trong package `repository`:

```java
package com.mycompany.ecommerceproject.repository;

import com.mycompany.ecommerceproject.entity.Product;
import com.mycompany.ecommerceproject.entity.Category;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;
import org.springframework.stereotype.Repository;

import java.math.BigDecimal;
import java.util.List;

@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {

    // Tìm sản phẩm theo tên (chính xác)
    Optional<Product> findByName(String name);

    // Tìm sản phẩm theo tên, không phân biệt hoa thường
    List<Product> findByNameContainingIgnoreCase(String keyword);

    // Tìm sản phẩm thuộc một Category cụ thể
    List<Product> findByCategory(Category category);
    // hoặc
    List<Product> findByCategoryId(Long categoryId); // Tìm theo ID của Category

    // Tìm sản phẩm có giá trong một khoảng
    List<Product> findByPriceBetween(BigDecimal minPrice, BigDecimal maxPrice);

    // Tìm sản phẩm có số lượng tồn kho lớn hơn một giá trị nhất định
    List<Product> findByStockQuantityGreaterThan(Integer quantity);

    // Ví dụ sử dụng Pageable để phân trang và sắp xếp
    Page<Product> findByCategoryName(String categoryName, Pageable pageable);

    // Ví dụ sử dụng @Query với JPQL
    @Query("SELECT p FROM Product p WHERE p.name LIKE %:keyword% OR p.description LIKE %:keyword%")
    List<Product> searchProducts(@Param("keyword") String keyword);

    @Query("SELECT p FROM Product p JOIN p.category c WHERE c.name = :categoryName AND p.price < :maxPrice")
    List<Product> findByCategoryNameAndPriceLessThan(
            @Param("categoryName") String categoryName,
            @Param("maxPrice") BigDecimal maxPrice
    );

    // Ví dụ sử dụng @Query với Native SQL (ít dùng hơn JPQL, nhưng hữu ích cho các query rất đặc thù của CSDL)
    // @Query(value = "SELECT * FROM products p WHERE p.name = :productName LIMIT 1", nativeQuery = true)
    // Optional<Product> findByNameNative(@Param("productName") String productName);
}
```

**2.3. `UserAccountRepository`**

Tạo file `UserAccountRepository.java` trong package `repository`:

```java
package com.mycompany.ecommerceproject.repository;

import com.mycompany.ecommerceproject.entity.UserAccount;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import java.util.Optional;

@Repository
public interface UserAccountRepository extends JpaRepository<UserAccount, Long> {

    // Tìm UserAccount theo username
    Optional<UserAccount> findByUsername(String username);

    // Tìm UserAccount theo email
    Optional<UserAccount> findByEmail(String email);

    // Kiểm tra sự tồn tại của username
    boolean existsByUsername(String username);

    // Kiểm tra sự tồn tại của email
    boolean existsByEmail(String email);

    // Tìm UserAccount theo role
    List<UserAccount> findByRole(String role);
}
```

**Giải thích:**

- `@Repository`: Annotation này đánh dấu interface là một Spring Component (một bean được quản lý bởi Spring). Nó cũng giúp Spring xử lý các exception liên quan đến persistence và dịch chúng sang các exception của Spring DataAccessException. Mặc dù Spring Boot có thể tự động phát hiện các interface kế thừa từ `JpaRepository` mà không cần `@Repository` (nếu chúng nằm trong package được scan), việc thêm nó là một good practice để rõ ràng.
- `extends JpaRepository<EntityType, IdType>`:
  - `EntityType`: Class Entity mà repository này quản lý (ví dụ: `Product`).
  - `IdType`: Kiểu dữ liệu của khóa chính trong Entity đó (ví dụ: `Long`).

---

**3. Tìm hiểu về các interface cơ sở**

Spring Data JPA cung cấp một hệ thống các interface cơ sở, mỗi cái mở rộng thêm chức năng:

- **`Repository<T, ID>`:**

  - Interface đánh dấu (marker interface). Không có phương thức nào.
  - Mục đích chính là để Spring phát hiện ra các interface repository của bạn.
  - Bạn cần tự định nghĩa tất cả các phương thức truy vấn.

- **`CrudRepository<T, ID>`:**

  - Kế thừa từ `Repository`.
  - Cung cấp các phương thức CRUD cơ bản:
    - `S save(S entity)`: Lưu hoặc cập nhật một entity.
    - `Iterable<S> saveAll(Iterable<S> entities)`: Lưu hoặc cập nhật nhiều entities.
    - `Optional<T> findById(ID id)`: Tìm entity theo ID.
    - `boolean existsById(ID id)`: Kiểm tra entity có tồn tại theo ID.
    - `Iterable<T> findAll()`: Lấy tất cả entities.
    - `Iterable<T> findAllById(Iterable<ID> ids)`: Lấy tất cả entities theo danh sách ID.
    - `long count()`: Đếm số lượng entity.
    - `void deleteById(ID id)`: Xóa entity theo ID.
    - `void delete(T entity)`: Xóa một entity.
    - `void deleteAll(Iterable<? extends T> entities)`: Xóa nhiều entities.
    - `void deleteAll()`: Xóa tất cả entities.

- **`PagingAndSortingRepository<T, ID>`:**

  - Kế thừa từ `CrudRepository`.
  - Thêm các phương thức để hỗ trợ phân trang và sắp xếp:
    - `Iterable<T> findAll(Sort sort)`: Lấy tất cả entities và sắp xếp.
    - `Page<T> findAll(Pageable pageable)`: Lấy một "trang" (Page) dữ liệu, hỗ trợ cả phân trang và sắp xếp.

- **`JpaRepository<T, ID>`:**
  - Kế thừa từ `PagingAndSortingRepository`.
  - Cung cấp thêm các phương thức dành riêng cho JPA:
    - `List<T> findAll()`: Trả về `List` thay vì `Iterable` (tiện lợi hơn).
    - `List<T> findAll(Sort sort)`: Tương tự, trả về `List`.
    - `List<T> findAllById(Iterable<ID> ids)`: Trả về `List`.
    - `void flush()`: Đồng bộ hóa persistence context với database (ép ghi các thay đổi).
    - `S saveAndFlush(S entity)`: Lưu entity và flush ngay lập tức.
    - `void deleteInBatch(Iterable<T> entities)`: Xóa entities theo batch (hiệu quả hơn cho nhiều bản ghi).
    - `void deleteAllInBatch()`: Xóa tất cả entities theo batch.
    - `T getOne(ID id)` / `T getById(ID id)`: Lấy một tham chiếu "lazy" đến entity (có thể ném `EntityNotFoundException` nếu không tồn tại khi truy cập). `findById` thường được ưu tiên hơn vì nó trả về `Optional` và xử lý trường hợp không tìm thấy tốt hơn. `getById` trả về một proxy và sẽ chỉ query DB khi một thuộc tính của proxy được truy cập. (Lưu ý: `getOne` đã bị deprecated và thay bằng `getReferenceById`).
    - `List<T> findAll(Example<S> example)`: Hỗ trợ Query by Example (QBE).
    - `List<T> findAll(Example<S> example, Sort sort)`: QBE với sắp xếp.

**Tại sao chọn `JpaRepository`?**
Thường thì `JpaRepository` là lựa chọn tốt nhất vì nó cung cấp nhiều phương thức tiện lợi nhất, bao gồm cả CRUD, phân trang, sắp xếp và các tính năng JPA-specific.

---

**4. Sử dụng các phương thức CRUD có sẵn**

Như đã thấy, `JpaRepository` (và các interface cha của nó) cung cấp sẵn rất nhiều phương thức hữu ích. Bạn không cần phải viết bất kỳ dòng code nào để có được chúng.

Ví dụ, với `ProductRepository`:

- `productRepository.save(newProduct)`: Lưu một sản phẩm mới.
- `productRepository.findById(1L)`: Tìm sản phẩm có ID là 1.
- `productRepository.findAll()`: Lấy tất cả sản phẩm.
- `productRepository.deleteById(1L)`: Xóa sản phẩm có ID là 1.
- `productRepository.count()`: Đếm số lượng sản phẩm.

---

**5. Viết các Query Methods (Derived Query Methods)**

Đây là một trong những tính năng mạnh mẽ nhất của Spring Data JPA. Bạn chỉ cần định nghĩa một phương thức trong interface repository theo một quy ước đặt tên nhất định, và Spring Data JPA sẽ tự động "hiểu" và tạo ra query tương ứng.

**Quy ước:**
`find...By...`, `read...By...`, `query...By...`, `count...By...`, `get...By...`
Sau `By` là tên thuộc tính của Entity (có thể lồng nhau, ví dụ: `findByCategoryName(...)`).
Bạn có thể kết hợp các điều kiện bằng `And`, `Or`.
Có thể thêm các từ khóa như `Containing`, `IgnoreCase`, `StartingWith`, `EndingWith`, `Between`, `LessThan`, `GreaterThan`, `IsNull`, `IsNotNull`, `True`, `False`, `In`, `NotIn`, `OrderBy...Asc/Desc`.

**Ví dụ (đã có trong các repository ở trên):**

- Trong `CategoryRepository`:

  - `Optional<Category> findByName(String name);`
    - Sẽ sinh ra query JPQL tương tự: `SELECT c FROM Category c WHERE c.name = :name`

- Trong `ProductRepository`:

  - `Optional<Product> findByName(String name);`
  - `List<Product> findByNameContainingIgnoreCase(String keyword);`
    - Tương tự: `SELECT p FROM Product p WHERE upper(p.name) LIKE upper(concat('%', :keyword, '%'))`
  - `List<Product> findByCategory(Category category);`
  - `List<Product> findByCategoryId(Long categoryId);`
    - Tương tự: `SELECT p FROM Product p WHERE p.category.id = :categoryId`
  - `List<Product> findByPriceBetween(BigDecimal minPrice, BigDecimal maxPrice);`
    - Tương tự: `SELECT p FROM Product p WHERE p.price >= :minPrice AND p.price <= :maxPrice`

- Trong `UserAccountRepository`:
  - `Optional<UserAccount> findByUsername(String username);`
  - `boolean existsByEmail(String email);`
    - Tương tự: `SELECT CASE WHEN count(u) > 0 THEN true ELSE false END FROM UserAccount u WHERE u.email = :email`

**Kiểu trả về:**

- Nếu query có thể trả về nhiều kết quả: `List<Entity>`, `Set<Entity>`, `Collection<Entity>`.
- Nếu query dự kiến chỉ trả về một kết quả hoặc không có: `Optional<Entity>` (khuyến nghị) hoặc `Entity` (sẽ trả về `null` nếu không tìm thấy, hoặc ném `IncorrectResultSizeDataAccessException` nếu tìm thấy nhiều hơn 1 khi không phải là collection).
- Đối với query `count...By...`: `long` hoặc `int`.
- Đối với query `exists...By...`: `boolean`.

---

**6. Sử dụng `@Query` annotation**

Khi derived query methods không đủ linh hoạt hoặc query quá phức tạp, bạn có thể sử dụng `@Query` để tự định nghĩa query.

**6.1. JPQL (Java Persistence Query Language)**
JPQL tương tự SQL nhưng thao tác trên các Entity và thuộc tính của Entity, thay vì bảng và cột trong CSDL. Đây là cách ưa thích hơn native SQL vì nó độc lập với CSDL hơn.

```java
// Trong ProductRepository
@Query("SELECT p FROM Product p WHERE p.name LIKE %:keyword% OR p.description LIKE %:keyword%")
List<Product> searchProducts(@Param("keyword") String keyword);

@Query("SELECT p FROM Product p JOIN p.category c WHERE c.name = :categoryName AND p.price < :maxPrice")
List<Product> findByCategoryNameAndPriceLessThan(
        @Param("categoryName") String categoryName,
        @Param("maxPrice") BigDecimal maxPrice
);
```

- `:keyword` và `:categoryName`, `:maxPrice` là các named parameters.
- `@Param("keyword")` liên kết tham số của phương thức Java với named parameter trong query.
- `JOIN p.category c`: Đây là cách join trong JPQL, dựa trên mối quan hệ đã định nghĩa trong entity.

**6.2. Native SQL**
Bạn cũng có thể viết câu lệnh SQL thuần túy (native) nếu cần sử dụng các tính năng đặc thù của một CSDL cụ thể mà JPQL không hỗ trợ.

```java
// Trong ProductRepository (ví dụ minh họa, thường không cần thiết cho query đơn giản này)
// @Query(value = "SELECT * FROM products p WHERE p.name = :productName ORDER BY p.created_at DESC LIMIT 1", nativeQuery = true)
// Optional<Product> findLatestProductByNameNative(@Param("productName") String productName);
```

- `value = "..."`: Chứa câu lệnh SQL native.
- `nativeQuery = true`: Bắt buộc phải có để chỉ ra đây là native SQL.
- Tên bảng và cột phải khớp với CSDL.

**Khi nào dùng `@Query`?**

- Query phức tạp với nhiều `JOIN` hoặc subquery mà derived method không thể diễn tả.
- Cần sử dụng các hàm hoặc cú pháp đặc thù của CSDL (với `nativeQuery = true`).
- Muốn có toàn quyền kiểm soát câu lệnh query.
- Cần các DTO projection (sẽ nói sau).

---

**7. Giới thiệu về Pagination và Sorting**

Khi làm việc với lượng lớn dữ liệu, bạn không muốn tải tất cả về một lúc. Phân trang (Pagination) và Sắp xếp (Sorting) là rất cần thiết.
`PagingAndSortingRepository` (và do đó `JpaRepository`) hỗ trợ điều này thông qua interface `Pageable`.

- **`Pageable`:**

  - Là một interface đại diện cho thông tin phân trang (số trang, kích thước trang) và thông tin sắp xếp.
  - Thường được tạo bằng `PageRequest.of(pageNumber, pageSize, Sort.by(...))`.
    - `pageNumber`: Số trang (bắt đầu từ 0).
    - `pageSize`: Số lượng item trên mỗi trang.
    - `Sort.by(...)`: Đối tượng `Sort` để chỉ định cách sắp xếp.
      - `Sort.by("propertyName")`: Sắp xếp tăng dần theo `propertyName`.
      - `Sort.by("propertyName1", "propertyName2")`: Sắp xếp theo nhiều thuộc tính.
      - `Sort.by(Sort.Direction.DESC, "propertyName")`: Sắp xếp giảm dần.
      - `Sort.by(Sort.Order.asc("propertyName1"), Sort.Order.desc("propertyName2"))`: Kết hợp các hướng sắp xếp.

- **`Page<T>`:**
  - Là kiểu trả về của các phương thức repository có tham số `Pageable`.
  - `Page` object chứa:
    - `List<T> getContent()`: Danh sách các entity của trang hiện tại.
    - `int getNumber()`: Số trang hiện tại (zero-based).
    - `int getSize()`: Kích thước trang.
    - `int getTotalPages()`: Tổng số trang.
    - `long getTotalElements()`: Tổng số entity khớp với query.
    - `boolean hasNext()`, `boolean hasPrevious()`: Kiểm tra có trang tiếp theo/trước đó không.
    - `boolean isFirst()`, `boolean isLast()`: Kiểm tra có phải trang đầu/cuối không.
    - `Sort getSort()`: Thông tin sắp xếp đã áp dụng.

**Ví dụ sử dụng `Pageable`:**

```java
// Trong ProductRepository
Page<Product> findByCategoryName(String categoryName, Pageable pageable);

// Cách gọi trong Service hoặc Controller (sẽ học sau):
// import org.springframework.data.domain.Page;
// import org.springframework.data.domain.PageRequest;
// import org.springframework.data.domain.Pageable;
// import org.springframework.data.domain.Sort;

// ... inject ProductRepository productRepository;

// Pageable firstPageWithTwoElements = PageRequest.of(0, 2); // Trang đầu tiên, 2 phần tử
// Page<Product> productsPage1 = productRepository.findAll(firstPageWithTwoElements);

// Pageable secondPageWithFiveElementsSortedByNameDesc =
// PageRequest.of(1, 5, Sort.by("name").descending());
// Page<Product> productsPage2 = productRepository.findByCategoryName("Electronics", secondPageWithFiveElementsSortedByNameDesc);

// productsPage2.getContent().forEach(product -> System.out.println(product.getName()));
// System.out.println("Total elements: " + productsPage2.getTotalElements());
// System.out.println("Total pages: " + productsPage2.getTotalPages());
```

Chúng ta sẽ áp dụng `Pageable` khi xây dựng API và hiển thị danh sách sản phẩm.

---

**8. Tạo một `CommandLineRunner` để thử nghiệm Repository**

Để nhanh chóng thấy kết quả làm việc của Repository mà chưa cần xây dựng Service và Controller, chúng ta có thể tạo một `CommandLineRunner`. Bean này sẽ được thực thi một lần ngay sau khi Spring Boot ApplicationContext được tải xong.

- Trong package `com.mycompany.ecommerceproject`, tạo một class mới, ví dụ `DataInitializer.java`:

```java
package com.mycompany.ecommerceproject;

import com.mycompany.ecommerceproject.entity.Category;
import com.mycompany.ecommerceproject.entity.Product;
import com.mycompany.ecommerceproject.entity.UserAccount;
import com.mycompany.ecommerceproject.repository.CategoryRepository;
import com.mycompany.ecommerceproject.repository.ProductRepository;
import com.mycompany.ecommerceproject.repository.UserAccountRepository;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;
import org.springframework.transaction.annotation.Transactional; // Quan trọng khi thao tác nhiều repository

import java.math.BigDecimal;
import java.util.List;
import java.util.Optional;

@Component // Đánh dấu là một Spring bean
public class DataInitializer implements CommandLineRunner {

    private static final Logger logger = LoggerFactory.getLogger(DataInitializer.class);

    private final CategoryRepository categoryRepository;
    private final ProductRepository productRepository;
    private final UserAccountRepository userAccountRepository;

    // Constructor Injection - cách khuyến nghị để inject dependencies
    public DataInitializer(CategoryRepository categoryRepository,
                           ProductRepository productRepository,
                           UserAccountRepository userAccountRepository) {
        this.categoryRepository = categoryRepository;
        this.productRepository = productRepository;
        this.userAccountRepository = userAccountRepository;
    }

    @Override
    @Transactional // Bọc trong một transaction, đảm bảo tính nhất quán nếu có lỗi
    public void run(String... args) throws Exception {
        logger.info("Starting data initialization...");

        // === Tạo Categories ===
        Category electronics = new Category();
        electronics.setName("Electronics");
        electronics.setDescription("Gadgets, devices, and more");
        categoryRepository.save(electronics); // Lưu và Hibernate sẽ gán ID

        Category books = new Category();
        books.setName("Books");
        books.setDescription("Fiction, non-fiction, educational");
        categoryRepository.save(books);

        Category clothing = new Category();
        clothing.setName("Clothing");
        clothing.setDescription("Apparel for men and women");
        // Thử tìm category "Clothing", nếu chưa có thì tạo mới
        Optional<Category> existingClothingOpt = categoryRepository.findByName("Clothing");
        if (existingClothingOpt.isEmpty()) {
            categoryRepository.save(clothing);
        } else {
            clothing = existingClothingOpt.get(); // Sử dụng category đã có
        }


        logger.info("Saved categories. Count: {}", categoryRepository.count());
        categoryRepository.findAll().forEach(cat -> logger.info("Category: {}", cat.getName()));


        // === Tạo Products ===
        Product laptop = new Product();
        laptop.setName("Super Laptop Pro");
        laptop.setDescription("A very powerful laptop for professionals.");
        laptop.setPrice(new BigDecimal("1200.99"));
        laptop.setStockQuantity(50);
        laptop.setCategory(electronics); // Gán category
        productRepository.save(laptop);

        Product smartphone = new Product();
        smartphone.setName("SmartPhone X");
        smartphone.setDescription("Latest generation smartphone.");
        smartphone.setPrice(new BigDecimal("799.50"));
        smartphone.setStockQuantity(100);
        smartphone.setCategory(electronics);
        productRepository.save(smartphone);

        Product springBook = new Product();
        springBook.setName("Spring in Action");
        springBook.setDescription("Comprehensive guide to Spring Framework.");
        springBook.setPrice(new BigDecimal("45.00"));
        springBook.setStockQuantity(200);
        springBook.setCategory(books);
        productRepository.save(springBook);

        Product tShirt = new Product();
        tShirt.setName("Cool T-Shirt");
        tShirt.setDescription("100% cotton t-shirt.");
        tShirt.setPrice(new BigDecimal("19.99"));
        tShirt.setStockQuantity(150);
        tShirt.setCategory(clothing);
        productRepository.save(tShirt);

        logger.info("Saved products. Count: {}", productRepository.count());
        productRepository.findAll().forEach(p -> logger.info("Product: {} - Price: {} - Category: {}",
                p.getName(), p.getPrice(), p.getCategory().getName()));


        // === Tạo Users ===
        if (!userAccountRepository.existsByUsername("admin")) {
            UserAccount adminUser = new UserAccount();
            adminUser.setUsername("admin");
            adminUser.setPassword("hashed_admin_password"); // Trong thực tế, password phải được hash
            adminUser.setEmail("admin@example.com");
            adminUser.setFirstName("Admin");
            adminUser.setLastName("User");
            adminUser.setRole("ROLE_ADMIN");
            userAccountRepository.save(adminUser);
        }

        if (!userAccountRepository.existsByUsername("john.doe")) {
            UserAccount regularUser = new UserAccount();
            regularUser.setUsername("john.doe");
            regularUser.setPassword("hashed_user_password");
            regularUser.setEmail("john.doe@example.com");
            regularUser.setFirstName("John");
            regularUser.setLastName("Doe");
            regularUser.setRole("ROLE_USER"); // Mặc định role là ROLE_USER nếu không set (xem lại entity UserAccount)
            userAccountRepository.save(regularUser);
        }

        logger.info("Saved users. Count: {}", userAccountRepository.count());
        userAccountRepository.findAll().forEach(u -> logger.info("User: {} - Role: {}", u.getUsername(), u.getRole()));


        // === Thử nghiệm các query method ===
        logger.info("--- Testing Query Methods ---");

        // Tìm category theo tên
        Optional<Category> electronicsCat = categoryRepository.findByName("Electronics");
        electronicsCat.ifPresent(cat -> logger.info("Found Category by name 'Electronics': {}", cat.getDescription()));

        // Tìm sản phẩm theo tên category
        List<Product> electronicProducts = productRepository.findByCategoryName("Electronics", PageRequest.of(0, 10)).getContent();
        logger.info("Products in 'Electronics' category (found via derived query with Pageable):");
        electronicProducts.forEach(p -> logger.info("- {}", p.getName()));

        // Tìm sản phẩm có giá từ 500 đến 1000
        List<Product> midPriceProducts = productRepository.findByPriceBetween(new BigDecimal("500"), new BigDecimal("1000"));
        logger.info("Products with price between 500 and 1000:");
        midPriceProducts.forEach(p -> logger.info("- {} (Price: {})", p.getName(), p.getPrice()));

        // Tìm user theo username
        Optional<UserAccount> adminOpt = userAccountRepository.findByUsername("admin");
        adminOpt.ifPresent(admin -> logger.info("Found admin user: {} with email {}", admin.getUsername(), admin.getEmail()));

        // Tìm kiếm sản phẩm bằng @Query
        List<Product> searchResults = productRepository.searchProducts("laptop");
        logger.info("Search results for 'laptop':");
        searchResults.forEach(p -> logger.info("- {}", p.getName()));

        logger.info("Data initialization completed.");
    }
}
```

- **`@Component`:** Để Spring quét và tạo bean này.
- **`CommandLineRunner`:** Interface có một phương thức `run(String... args)`. Spring Boot sẽ tự động gọi phương thức này sau khi application context đã được load.
- **Constructor Injection:** Đây là cách khuyến nghị để inject dependencies (các repository) vào bean. Spring sẽ tự động cung cấp các instance của `CategoryRepository`, `ProductRepository`, `UserAccountRepository`.
- **`@Transactional`:** Rất quan trọng. Nếu không có `@Transactional` và bạn có nhiều thao tác `save()` hoặc thao tác trên nhiều repository, nếu một thao tác thất bại giữa chừng, các thao tác trước đó có thể đã được commit vào DB, gây ra trạng thái dữ liệu không nhất quán. `@Transactional` đảm bảo rằng tất cả các thao tác trong phương thức `run()` được thực hiện như một đơn vị công việc duy nhất: hoặc tất cả thành công, hoặc tất cả thất bại (rollback).
  - Lưu ý: Để `@Transactional` hoạt động đúng cách trên các phương thức public của một Spring bean, Spring cần tạo proxy cho bean đó.
- **Logging:** Sử dụng SLF4J (với Logback làm implementation mặc định trong Spring Boot) để ghi log. `LoggerFactory.getLogger(DataInitializer.class)`
- **Ví dụ về `save()`:** Sau khi `save()`, entity (ví dụ `electronics`) sẽ được cập nhật với ID do CSDL sinh ra (vì chúng ta dùng `GenerationType.IDENTITY`).
- **Gán quan hệ:** `laptop.setCategory(electronics);` thiết lập mối quan hệ Many-to-One. Khi `laptop` được lưu, khóa ngoại `category_id` sẽ được tự động điền.
- **Kiểm tra trước khi tạo:** `if (!userAccountRepository.existsByUsername("admin"))` để tránh tạo dữ liệu trùng lặp mỗi lần ứng dụng khởi động (quan trọng vì H2 in-memory sẽ xóa dữ liệu cũ, nhưng nếu bạn chuyển sang DB persistent, điều này cần thiết).
- **Mật khẩu:** Hiện tại chúng ta lưu mật khẩu dạng plain text. **ĐIỀU NÀY CỰC KỲ NGUYHIỂM TRONG THỰC TẾ.** Ở các phần sau về Spring Security, chúng ta sẽ học cách hash mật khẩu.

**Chạy ứng dụng:**
Bây giờ, khi bạn chạy ứng dụng Spring Boot, bạn sẽ thấy các log từ `DataInitializer` trong console, cho thấy dữ liệu đã được chèn và các query đã được thực thi.
Bạn cũng có thể vào H2 Console (`http://localhost:8080/h2-console`, JDBC URL: `jdbc:h2:mem:testdb`, user: `sa`, pass: trống) và thực hiện các câu `SELECT * FROM CATEGORIES;`, `SELECT * FROM PRODUCTS;`, `SELECT * FROM USER_ACCOUNTS;` để xem dữ liệu.

---

**9. Best Practices cho Repository Layer**

- **Giữ Repository Interfaces gọn gàng:** Chỉ nên chứa các phương thức truy vấn dữ liệu. Không đặt logic nghiệp vụ ở đây.
- **Ưu tiên Derived Query Methods:** Chúng dễ đọc, dễ bảo trì và ít lỗi hơn so với việc viết JPQL/SQL thủ công.
- **Sử dụng `@Query` khi cần thiết:** Cho các query phức tạp hoặc khi cần tối ưu hóa đặc biệt. Ưu tiên JPQL hơn Native SQL để giữ tính độc lập với CSDL.
- **Sử dụng `Optional<T>`:** Cho các phương thức query dự kiến trả về một hoặc không có kết quả. Điều này giúp xử lý trường hợp `null` một cách tường minh và tránh `NullPointerException`.
- **Hiểu rõ `FetchType` (LAZY vs EAGER):** Như đã thảo luận ở Phần 2, điều này rất quan trọng cho hiệu suất. Spring Data JPA không thay đổi hành vi fetch mặc định của JPA (ví dụ: `@ManyToOne` mặc định EAGER, `@OneToMany` mặc định LAZY).
- **Cẩn thận với `findAll()`:** Trên các bảng lớn, `findAll()` không có phân trang có thể tải một lượng lớn dữ liệu vào bộ nhớ, gây ra vấn đề về hiệu suất và bộ nhớ. Luôn sử dụng `Pageable` cho các tập kết quả lớn.
- **Transaction Management:**
  - Các phương thức trong repository được cung cấp bởi Spring Data JPA (như `save()`, `delete()`) thường đã được bọc trong transaction bởi Spring.
  - Nếu bạn gọi nhiều phương thức repository trong một phương thức service (sẽ học sau), thì phương thức service đó nên được đánh dấu `@Transactional` để đảm bảo tất cả các thao tác là một atomic operation. `DataInitializer` của chúng ta đã làm điều này.
- **Sử dụng Projection (nếu cần):** Đôi khi bạn chỉ cần một vài trường từ entity, không phải toàn bộ entity. Spring Data JPA hỗ trợ interface-based projections và DTO projections để chỉ lấy dữ liệu cần thiết, giúp cải thiện hiệu suất. (Chủ đề nâng cao hơn, có thể tìm hiểu thêm).
- **Testing:** Viết unit test cho các custom query trong repository của bạn sử dụng `@DataJpaTest`. Điều này giúp đảm bảo các query hoạt động đúng như mong đợi. `@DataJpaTest` sẽ cấu hình một môi trường test nhẹ với H2 in-memory (hoặc DB test khác) và chỉ load các component liên quan đến JPA.

---

**Kết thúc Phần 3**

Chúng ta đã đi một chặng đường dài trong việc xây dựng tầng truy cập dữ liệu:

- Hiểu và tạo các Repository interface với Spring Data JPA.
- Sử dụng các phương thức CRUD có sẵn, derived query methods, và `@Query`.
- Tìm hiểu về phân trang và sắp xếp.
- Tạo dữ liệu mẫu và thử nghiệm repository bằng `CommandLineRunner`.
- Thảo luận về các best practices.

Với tầng Repository đã sẵn sàng, chúng ta có thể truy vấn và thao tác dữ liệu một cách dễ dàng. Bước tiếp theo là xây dựng tầng Service để chứa business logic của ứng dụng.

**Trong Phần 4, chúng ta sẽ: "Xây dựng Service Layer và Business Logic."** (Tách biệt logic, dependency injection, xử lý nghiệp vụ cơ bản cho sản phẩm).

Tuyệt vời! Chúng ta sẽ tiếp tục xây dựng tầng Service, nơi "bộ não" của ứng dụng được đặt, xử lý các quy tắc nghiệp vụ và điều phối các hoạt động.

---

**Phần 4: Xây dựng Service Layer và Business Logic**

Trong phần này, chúng ta sẽ:

1.  **Tìm hiểu vai trò và lợi ích của Service Layer.**
2.  **Nguyên tắc thiết kế Service Layer: Single Responsibility, Dependency Injection.**
3.  **Tạo các Service Interfaces và Implementations cho `Category`, `Product`, và `UserAccount`.**
4.  **Triển khai các phương thức nghiệp vụ cơ bản (CRUD và các logic khác).**
5.  **Xử lý `Optional` và ném Exception tùy chỉnh khi cần thiết.**
6.  **Sử dụng `@Transactional` trong Service Layer.**
7.  **Giới thiệu sơ lược về Data Transfer Objects (DTOs) và tại sao chúng có thể cần thiết (sẽ đào sâu hơn ở phần sau).**
8.  **Cập nhật `DataInitializer` (nếu cần) để sử dụng Services thay vì Repositories trực tiếp (tùy chọn).**
9.  **Best Practices cho Service Layer.**

---

**1. Tìm hiểu vai trò và lợi ích của Service Layer**

- **Service Layer (Tầng Dịch Vụ) là gì?**

  - Là một tầng trung gian nằm giữa Controller Layer (hoặc presentation layer) và Repository Layer (data access layer).
  - Nó chứa đựng **business logic (logic nghiệp vụ)** của ứng dụng.
  - Nó điều phối các tương tác giữa các repository khác nhau và thực hiện các quy tắc nghiệp vụ trước khi dữ liệu được lưu hoặc sau khi dữ liệu được truy xuất.

- **Tại sao cần Service Layer?**
  - **Tách biệt mối quan tâm (Separation of Concerns):**
    - **Controller:** Chỉ chịu trách nhiệm nhận HTTP request, gọi service tương ứng, và trả về HTTP response. Không chứa business logic.
    - **Service:** Chứa business logic, điều phối các repository, quản lý transaction. Không biết về HTTP.
    - **Repository:** Chỉ chịu trách nhiệm tương tác với cơ sở dữ liệu. Không chứa business logic phức tạp.
  - **Tái sử dụng Logic:** Các business logic có thể được sử dụng bởi nhiều controller khác nhau (ví dụ: một Web Controller và một REST API Controller đều có thể gọi cùng một ProductService).
  - **Quản lý Transaction:** Service layer là nơi lý tưởng để định nghĩa ranh giới của các transaction. Một nghiệp vụ có thể liên quan đến nhiều thao tác trên CSDL (ví dụ: tạo Order cần cập nhật Product stock và lưu Order), tất cả phải nằm trong một transaction.
  - **Tăng khả năng Test:** Dễ dàng viết unit test cho business logic trong service mà không cần mock HTTP request/response hoặc tương tác CSDL phức tạp.
  - **Tính linh hoạt và Bảo trì:** Khi business logic thay đổi, bạn chỉ cần sửa đổi ở service layer mà không ảnh hưởng nhiều đến controller hay repository.
  - **Che giấu chi tiết triển khai của Data Access Layer:** Controller không cần biết dữ liệu đến từ đâu (JPA, JDBC, NoSQL). Nó chỉ tương tác với Service.

**Ví dụ về Business Logic trong Service:**

- Kiểm tra xem một sản phẩm có đủ số lượng tồn kho trước khi cho phép đặt hàng.
- Tính toán tổng giá trị đơn hàng, áp dụng mã giảm giá.
- Gửi email thông báo khi người dùng đăng ký thành công.
- Kiểm tra quyền của người dùng trước khi thực hiện một hành động.
- Kết hợp dữ liệu từ nhiều nguồn (nhiều repository) để tạo ra một kết quả tổng hợp.

---

**2. Nguyên tắc thiết kế Service Layer**

- **Single Responsibility Principle (SRP - Nguyên tắc Đơn trách nhiệm):**
  - Mỗi service class nên có một trách nhiệm nghiệp vụ rõ ràng và duy nhất. Ví dụ: `ProductService` quản lý các nghiệp vụ liên quan đến sản phẩm, `OrderService` quản lý các nghiệp vụ liên quan đến đơn hàng.
  - Tránh tạo ra các "God Services" làm quá nhiều thứ.
- **Dependency Injection (DI):**
  - Service layer sẽ phụ thuộc vào Repository layer (và có thể là các service khác).
  - Sử dụng Dependency Injection (thông qua constructor injection là tốt nhất) để Spring quản lý việc tạo và tiêm các dependency này. Điều này giúp code dễ test và linh hoạt hơn.
  - Ví dụ: `ProductService` sẽ inject `ProductRepository`.
- **Interface-based Programming (Lập trình dựa trên Interface - Tùy chọn nhưng khuyến khích):**
  - Định nghĩa một interface cho mỗi service (ví dụ: `ProductService`) và một class implement interface đó (ví dụ: `ProductServiceImpl`).
  - **Lợi ích:**
    - **Tính linh hoạt:** Dễ dàng thay đổi implementation (ví dụ: dùng một mock implementation cho testing) mà không ảnh hưởng đến client code (controller).
    - **Tách biệt rõ ràng:** Interface định nghĩa "hợp đồng" (contract) của service.
    - **Proxying:** Spring thường tạo proxy quanh các bean (ví dụ: cho transaction, security). Làm việc với interface giúp việc tạo proxy dễ dàng hơn.
  - Tuy nhiên, với các ứng dụng nhỏ hoặc khi bạn chắc chắn chỉ có một implementation, việc bỏ qua interface và chỉ có class service cũng được chấp nhận để giảm bớt code. Chúng ta sẽ theo hướng có interface cho bài thực hành này để làm quen với best practice.

---

**3. Tạo các Service Interfaces và Implementations**

- Trong `src/main/java/com/mycompany/ecommerceproject`, tạo một package mới tên là `service`.
- Bên trong `service`, tạo thêm một package con tên là `impl` để chứa các class implementation.

**3.1. `CategoryService`**

- **Interface:** `src/main/java/com/mycompany/ecommerceproject/service/CategoryService.java`

  ```java
  package com.mycompany.ecommerceproject.service;

  import com.mycompany.ecommerceproject.entity.Category;
  import java.util.List;
  import java.util.Optional;

  public interface CategoryService {
      List<Category> findAll();
      Optional<Category> findById(Long id);
      Category save(Category category);
      void deleteById(Long id);
      Optional<Category> findByName(String name);
      Category createOrUpdateCategory(Category category); // Ví dụ một nghiệp vụ phức tạp hơn
  }
  ```

- **Implementation:** `src/main/java/com/mycompany/ecommerceproject/service/impl/CategoryServiceImpl.java`

  ```java
  package com.mycompany.ecommerceproject.service.impl;

  import com.mycompany.ecommerceproject.entity.Category;
  import com.mycompany.ecommerceproject.repository.CategoryRepository;
  import com.mycompany.ecommerceproject.service.CategoryService;
  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;
  import org.springframework.stereotype.Service;
  import org.springframework.transaction.annotation.Transactional; // Quan trọng

  import java.util.List;
  import java.util.Optional;

  @Service // Đánh dấu đây là một Service bean, Spring sẽ quản lý nó
  public class CategoryServiceImpl implements CategoryService {

      private static final Logger logger = LoggerFactory.getLogger(CategoryServiceImpl.class);
      private final CategoryRepository categoryRepository;

      // Constructor Injection
      public CategoryServiceImpl(CategoryRepository categoryRepository) {
          this.categoryRepository = categoryRepository;
      }

      @Override
      @Transactional(readOnly = true) // Giao dịch chỉ đọc, có thể tối ưu hóa
      public List<Category> findAll() {
          logger.info("Fetching all categories");
          return categoryRepository.findAll();
      }

      @Override
      @Transactional(readOnly = true)
      public Optional<Category> findById(Long id) {
          logger.info("Fetching category with id: {}", id);
          return categoryRepository.findById(id);
      }

      @Override
      @Transactional // Giao dịch ghi (mặc định readOnly = false)
      public Category save(Category category) {
          logger.info("Saving category: {}", category.getName());
          // Có thể thêm logic kiểm tra dữ liệu đầu vào ở đây trước khi lưu
          // Ví dụ: kiểm tra tên category có hợp lệ, không trùng lặp (nếu DB không có unique constraint)
          return categoryRepository.save(category);
      }

      @Override
      @Transactional
      public void deleteById(Long id) {
          logger.info("Deleting category with id: {}", id);
          // Có thể thêm logic kiểm tra xem category có sản phẩm nào không trước khi xóa
          // Ví dụ: if (productRepository.countByCategory(id) > 0) throw new BusinessException("Cannot delete category with products");
          categoryRepository.deleteById(id);
      }

      @Override
      @Transactional(readOnly = true)
      public Optional<Category> findByName(String name) {
          logger.info("Fetching category by name: {}", name);
          return categoryRepository.findByName(name);
      }

      @Override
      @Transactional
      public Category createOrUpdateCategory(Category categoryDetails) {
          // Ví dụ: Nếu category đã tồn tại (dựa trên tên), thì cập nhật description.
          // Nếu chưa, tạo mới.
          Optional<Category> existingCategoryOpt = categoryRepository.findByName(categoryDetails.getName());
          if (existingCategoryOpt.isPresent()) {
              Category existingCategory = existingCategoryOpt.get();
              existingCategory.setDescription(categoryDetails.getDescription());
              // Chỉ cập nhật các trường cần thiết, không ghi đè ID hoặc các trường khác nếu không muốn
              logger.info("Updating existing category: {}", existingCategory.getName());
              return categoryRepository.save(existingCategory);
          } else {
              // Tạo mới
              logger.info("Creating new category: {}", categoryDetails.getName());
              return categoryRepository.save(categoryDetails);
          }
      }
  }
  ```

**3.2. `ProductService`**

- **Interface:** `src/main/java/com/mycompany/ecommerceproject/service/ProductService.java`

  ```java
  package com.mycompany.ecommerceproject.service;

  import com.mycompany.ecommerceproject.entity.Product;
  import org.springframework.data.domain.Page;
  import org.springframework.data.domain.Pageable;

  import java.math.BigDecimal;
  import java.util.List;
  import java.util.Optional;

  public interface ProductService {
      Page<Product> findAll(Pageable pageable);
      List<Product> findAllNonPaginated(); // Ví dụ: nếu cần lấy hết (cẩn thận với bảng lớn)
      Optional<Product> findById(Long id);
      Product save(Product product);
      void deleteById(Long id);
      Page<Product> findByCategoryId(Long categoryId, Pageable pageable);
      List<Product> findByNameContaining(String keyword);
      Page<Product> searchProducts(String keyword, Pageable pageable); // Sử dụng @Query trong Repo
      Product updateStock(Long productId, int quantityChange) throws Exception; // Ví dụ nghiệp vụ
  }
  ```

- **Implementation:** `src/main/java/com/mycompany/ecommerceproject/service/impl/ProductServiceImpl.java`

  ```java
  package com.mycompany.ecommerceproject.service.impl;

  import com.mycompany.ecommerceproject.entity.Category;
  import com.mycompany.ecommerceproject.entity.Product;
  import com.mycompany.ecommerceproject.repository.CategoryRepository;
  import com.mycompany.ecommerceproject.repository.ProductRepository;
  import com.mycompany.ecommerceproject.service.ProductService;
  // Import một custom exception (sẽ tạo ở bước 5)
  import com.mycompany.ecommerceproject.exception.ResourceNotFoundException;
  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;
  import org.springframework.data.domain.Page;
  import org.springframework.data.domain.Pageable;
  import org.springframework.stereotype.Service;
  import org.springframework.transaction.annotation.Transactional;

  import java.math.BigDecimal;
  import java.util.List;
  import java.util.Optional;

  @Service
  public class ProductServiceImpl implements ProductService {

      private static final Logger logger = LoggerFactory.getLogger(ProductServiceImpl.class);
      private final ProductRepository productRepository;
      private final CategoryRepository categoryRepository; // Có thể cần để validate category

      public ProductServiceImpl(ProductRepository productRepository, CategoryRepository categoryRepository) {
          this.productRepository = productRepository;
          this.categoryRepository = categoryRepository;
      }

      @Override
      @Transactional(readOnly = true)
      public Page<Product> findAll(Pageable pageable) {
          logger.info("Fetching all products with pagination: {}", pageable);
          return productRepository.findAll(pageable);
      }

      @Override
      @Transactional(readOnly = true)
      public List<Product> findAllNonPaginated() {
          logger.info("Fetching all products (non-paginated)");
          return productRepository.findAll();
      }

      @Override
      @Transactional(readOnly = true)
      public Optional<Product> findById(Long id) {
          logger.info("Fetching product with id: {}", id);
          return productRepository.findById(id);
      }

      @Override
      @Transactional
      public Product save(Product product) {
          logger.info("Saving product: {}", product.getName());
          // Kiểm tra Category có tồn tại không nếu product.getCategory() != null và product.getCategory().getId() != null
          if (product.getCategory() != null && product.getCategory().getId() != null) {
              Category category = categoryRepository.findById(product.getCategory().getId())
                      .orElseThrow(() -> new ResourceNotFoundException("Category not found with id: " + product.getCategory().getId()));
              product.setCategory(category); // Đảm bảo category là managed entity
          } else if (product.getCategory() != null && product.getCategory().getName() != null) {
              // Nếu client gửi tên category thay vì ID, có thể tìm hoặc tạo mới
              Category category = categoryRepository.findByName(product.getCategory().getName())
                      .orElseGet(() -> {
                          logger.info("Category {} not found, creating new one.", product.getCategory().getName());
                          return categoryRepository.save(new Category(null, product.getCategory().getName(), "Auto-created category", null));
                      });
              product.setCategory(category);
          } else {
               // Nếu không có category, có thể gán category mặc định hoặc ném lỗi tùy nghiệp vụ
               // throw new IllegalArgumentException("Product must have a category");
               // Hoặc cho phép null tùy thiết kế
          }
          return productRepository.save(product);
      }

      @Override
      @Transactional
      public void deleteById(Long id) {
          logger.info("Deleting product with id: {}", id);
          if (!productRepository.existsById(id)) {
              throw new ResourceNotFoundException("Product not found with id: " + id + " for deletion.");
          }
          productRepository.deleteById(id);
      }

      @Override
      @Transactional(readOnly = true)
      public Page<Product> findByCategoryId(Long categoryId, Pageable pageable) {
          logger.info("Fetching products for category id: {} with pagination: {}", categoryId, pageable);
          // Kiểm tra category có tồn tại không
          categoryRepository.findById(categoryId)
                  .orElseThrow(() -> new ResourceNotFoundException("Category not found with id: " + categoryId));
          return productRepository.findByCategoryId(categoryId, pageable);
      }

      @Override
      @Transactional(readOnly = true)
      public List<Product> findByNameContaining(String keyword) {
          logger.info("Fetching products with name containing: {}", keyword);
          return productRepository.findByNameContainingIgnoreCase(keyword);
      }

      @Override
      @Transactional(readOnly = true)
      public Page<Product> searchProducts(String keyword, Pageable pageable) {
          logger.info("Searching products with keyword: '{}' and pageable: {}", keyword, pageable);
          // Repository method searchProducts nên trả về Page<Product>
          // Nếu ProductRepository.searchProducts trả về List, bạn cần chuyển đổi sang Page.
          // Tạm thời giả sử ProductRepository.searchProducts đã được sửa để trả về Page<Product>
          // Nếu chưa, bạn có thể dùng query.getResultList() và new PageImpl<>(list, pageable, total)
          // Hoặc sửa productRepository.searchProducts
           return productRepository.searchProducts(keyword, pageable); // Giả sử repo đã hỗ trợ Pageable
      }


      @Override
      @Transactional // Giao dịch ghi
      public Product updateStock(Long productId, int quantityChange) throws Exception {
          logger.info("Updating stock for product id: {} by quantity: {}", productId, quantityChange);
          Product product = productRepository.findById(productId)
                  .orElseThrow(() -> new ResourceNotFoundException("Product not found with id: " + productId));

          int newStock = product.getStockQuantity() + quantityChange;
          if (newStock < 0) {
              throw new IllegalArgumentException("Stock quantity cannot be negative. Current stock: "
                      + product.getStockQuantity() + ", trying to change by: " + quantityChange);
          }
          product.setStockQuantity(newStock);
          return productRepository.save(product);
      }
  }
  ```

  **Lưu ý:** `ProductRepository.searchProducts` của chúng ta ở Phần 3 đang trả về `List<Product>`. Để hỗ trợ `Pageable` đúng cách, bạn cần sửa nó trong `ProductRepository.java`:

  ```java
  // Trong ProductRepository.java
  // @Query("SELECT p FROM Product p WHERE p.name LIKE %:keyword% OR p.description LIKE %:keyword%")
  // List<Product> searchProducts(@Param("keyword") String keyword);
  // Sửa thành:
  @Query(value = "SELECT p FROM Product p WHERE LOWER(p.name) LIKE LOWER(CONCAT('%', :keyword, '%')) OR LOWER(p.description) LIKE LOWER(CONCAT('%', :keyword, '%'))",
         countQuery = "SELECT count(p) FROM Product p WHERE LOWER(p.name) LIKE LOWER(CONCAT('%', :keyword, '%')) OR LOWER(p.description) LIKE LOWER(CONCAT('%', :keyword, '%'))")
  Page<Product> searchProducts(@Param("keyword") String keyword, Pageable pageable);
  ```

  - `CONCAT('%', :keyword, '%')`: Đảm bảo wildcard hoạt động đúng cách.
  - `LOWER(...)`: Để tìm kiếm không phân biệt hoa thường.
  - `countQuery`: Rất quan trọng cho phân trang. Spring Data JPA cần biết tổng số bản ghi khớp với điều kiện (không tính phân trang) để tính `totalPages`, `totalElements`. Nếu không có `countQuery`, Spring Data sẽ cố gắng tự tạo (có thể không tối ưu) hoặc bạn có thể gặp lỗi.

**3.3. `UserAccountService` (Tương tự, chúng ta sẽ làm đơn giản trước)**

- **Interface:** `src/main/java/com/mycompany/ecommerceproject/service/UserAccountService.java`

  ```java
  package com.mycompany.ecommerceproject.service;

  import com.mycompany.ecommerceproject.entity.UserAccount;
  import java.util.List;
  import java.util.Optional;

  public interface UserAccountService {
      UserAccount registerUser(UserAccount userAccount); // Logic đăng ký, bao gồm hash password
      Optional<UserAccount> findByUsername(String username);
      Optional<UserAccount> findByEmail(String email);
      List<UserAccount> findAll();
      Optional<UserAccount> findById(Long id);
      // Các nghiệp vụ khác: đổi mật khẩu, cập nhật thông tin user, ...
  }
  ```

- **Implementation:** `src/main/java/com/mycompany/ecommerceproject/service/impl/UserAccountServiceImpl.java`

  ```java
  package com.mycompany.ecommerceproject.service.impl;

  import com.mycompany.ecommerceproject.entity.UserAccount;
  import com.mycompany.ecommerceproject.repository.UserAccountRepository;
  import com.mycompany.ecommerceproject.service.UserAccountService;
  import com.mycompany.ecommerceproject.exception.DuplicateResourceException; // Tạo custom exception
  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;
  // import org.springframework.security.crypto.password.PasswordEncoder; // Sẽ dùng ở phần Spring Security
  import org.springframework.stereotype.Service;
  import org.springframework.transaction.annotation.Transactional;

  import java.util.List;
  import java.util.Optional;

  @Service
  public class UserAccountServiceImpl implements UserAccountService {

      private static final Logger logger = LoggerFactory.getLogger(UserAccountServiceImpl.class);
      private final UserAccountRepository userAccountRepository;
      // private final PasswordEncoder passwordEncoder; // Sẽ inject khi có Spring Security

      // public UserAccountServiceImpl(UserAccountRepository userAccountRepository, PasswordEncoder passwordEncoder) {
      public UserAccountServiceImpl(UserAccountRepository userAccountRepository) { // Tạm thời constructor này
          this.userAccountRepository = userAccountRepository;
          // this.passwordEncoder = passwordEncoder;
      }

      @Override
      @Transactional
      public UserAccount registerUser(UserAccount userAccount) {
          logger.info("Registering user: {}", userAccount.getUsername());
          // Kiểm tra username hoặc email đã tồn tại chưa
          if (userAccountRepository.existsByUsername(userAccount.getUsername())) {
              throw new DuplicateResourceException("Username " + userAccount.getUsername() + " already exists.");
          }
          if (userAccountRepository.existsByEmail(userAccount.getEmail())) {
              throw new DuplicateResourceException("Email " + userAccount.getEmail() + " already exists.");
          }

          // Hash password trước khi lưu (SẼ LÀM Ở PHẦN SPRING SECURITY)
          // String hashedPassword = passwordEncoder.encode(userAccount.getPassword());
          // userAccount.setPassword(hashedPassword);
          // Tạm thời giữ nguyên password
          logger.warn("SECURITY WARNING: Storing plain text password for user {}. Implement password hashing!", userAccount.getUsername());


          // Gán vai trò mặc định nếu chưa có (có thể đã làm trong entity @PrePersist)
          if (userAccount.getRole() == null || userAccount.getRole().isEmpty()) {
              userAccount.setRole("ROLE_USER");
          }

          return userAccountRepository.save(userAccount);
      }

      @Override
      @Transactional(readOnly = true)
      public Optional<UserAccount> findByUsername(String username) {
          logger.info("Fetching user by username: {}", username);
          return userAccountRepository.findByUsername(username);
      }

      @Override
      @Transactional(readOnly = true)
      public Optional<UserAccount> findByEmail(String email) {
          logger.info("Fetching user by email: {}", email);
          return userAccountRepository.findByEmail(email);
      }

      @Override
      @Transactional(readOnly = true)
      public List<UserAccount> findAll() {
          logger.info("Fetching all users");
          return userAccountRepository.findAll();
      }

      @Override
      @Transactional(readOnly = true)
      public Optional<UserAccount> findById(Long id) {
          logger.info("Fetching user by id: {}", id);
          return userAccountRepository.findById(id);
      }
  }
  ```

---

**4. Triển khai các phương thức nghiệp vụ cơ bản (Đã làm ở trên)**

Chúng ta đã triển khai các phương thức CRUD cơ bản (find, save, delete) và một vài logic nghiệp vụ đơn giản:

- `CategoryServiceImpl.createOrUpdateCategory()`: Ví dụ về logic tìm hoặc tạo.
- `ProductServiceImpl.save()`: Kiểm tra sự tồn tại của Category.
- `ProductServiceImpl.updateStock()`: Cập nhật số lượng tồn kho, kiểm tra không âm.
- `UserAccountServiceImpl.registerUser()`: Kiểm tra trùng lặp username/email (password hashing sẽ thêm sau).

---

**5. Xử lý `Optional` và ném Exception tùy chỉnh khi cần thiết**

- **Xử lý `Optional`:**

  - Các phương thức repository như `findById` trả về `Optional<T>`. Điều này rất tốt để tránh `NullPointerException`.
  - Trong service, bạn có thể:
    - Trả về `Optional<T>` cho controller (để controller quyết định cách xử lý).
    - Xử lý `Optional` ngay trong service:
      ```java
      // Ví dụ trong ProductServiceImpl.findById (nếu muốn trả về Product hoặc ném lỗi)
      // public Product findByIdOrThrow(Long id) {
      //     return productRepository.findById(id)
      //             .orElseThrow(() -> new ResourceNotFoundException("Product not found with id: " + id));
      // }
      ```
      `orElseThrow()` là một cách phổ biến.

- **Exception Tùy Chỉnh (Custom Exceptions):**

  - Thay vì ném các exception chung chung như `RuntimeException` hay `IllegalArgumentException` một cách không rõ ràng, việc tạo các exception tùy chỉnh giúp code dễ hiểu và dễ quản lý hơn ở tầng controller (để map sang HTTP status codes phù hợp).
  - Tạo package `exception` trong `com.mycompany.ecommerceproject`.
  - **Ví dụ `ResourceNotFoundException.java`:**

    ```java
    package com.mycompany.ecommerceproject.exception;

    import org.springframework.http.HttpStatus;
    import org.springframework.web.bind.annotation.ResponseStatus;

    @ResponseStatus(HttpStatus.NOT_FOUND) // Giúp Spring MVC tự động trả về 404 Not Found nếu exception này không được bắt
    public class ResourceNotFoundException extends RuntimeException {
        public ResourceNotFoundException(String message) {
            super(message);
        }

        public ResourceNotFoundException(String message, Throwable cause) {
            super(message, cause);
        }
    }
    ```

  - **Ví dụ `DuplicateResourceException.java`:**

    ```java
    package com.mycompany.ecommerceproject.exception;

    import org.springframework.http.HttpStatus;
    import org.springframework.web.bind.annotation.ResponseStatus;

    @ResponseStatus(HttpStatus.CONFLICT) // 409 Conflict
    public class DuplicateResourceException extends RuntimeException {
        public DuplicateResourceException(String message) {
            super(message);
        }

        public DuplicateResourceException(String message, Throwable cause) {
            super(message, cause);
        }
    }
    ```

  - **Ví dụ `BadRequestException.java` (cho các lỗi dữ liệu đầu vào không hợp lệ):**

    ```java
    package com.mycompany.ecommerceproject.exception;

    import org.springframework.http.HttpStatus;
    import org.springframework.web.bind.annotation.ResponseStatus;

    @ResponseStatus(HttpStatus.BAD_REQUEST) // 400 Bad Request
    public class BadRequestException extends RuntimeException {
        public BadRequestException(String message) {
            super(message);
        }

        public BadRequestException(String message, Throwable cause) {
            super(message, cause);
        }
    }
    ```

  - Chúng ta đã sử dụng `ResourceNotFoundException` và `DuplicateResourceException` trong các service implementations ở trên.

---

**6. Sử dụng `@Transactional` trong Service Layer**

- **Vai trò:** `@Transactional` đảm bảo rằng một loạt các thao tác CSDL (có thể qua nhiều repository) được thực hiện như một đơn vị công việc duy nhất (atomic). Nếu bất kỳ thao tác nào thất bại, tất cả các thay đổi trong transaction đó sẽ được rollback.
- **Vị trí đặt:** Thường được đặt ở **public methods** của **Service Layer**. Đây là nơi các business use cases được định nghĩa.
- **`readOnly = true`:**
  - Sử dụng cho các nghiệp vụ chỉ đọc dữ liệu (ví dụ: `findAll()`, `findById()`).
  - Có thể giúp JPA/Hibernate thực hiện một số tối ưu hóa (ví dụ: không cần flush, không cần dirty checking).
  - Ví dụ: `@Transactional(readOnly = true)`
- **Propagation (Lan truyền):**
  - `@Transactional` có thuộc tính `propagation` (mặc định là `Propagation.REQUIRED`).
  - `REQUIRED`: Nếu đã có một transaction đang chạy, tham gia vào transaction đó. Nếu không, tạo một transaction mới. Đây là lựa chọn phổ biến nhất.
  - Các loại propagation khác: `SUPPORTS`, `MANDATORY`, `REQUIRES_NEW`, `NOT_SUPPORTED`, `NEVER`, `NESTED`.
- **Rollback Rules:**
  - Mặc định, Spring sẽ rollback transaction cho các `RuntimeException` (unchecked exceptions) và `Error`, nhưng không rollback cho các `Exception` (checked exceptions).
  - Bạn có thể tùy chỉnh điều này bằng `rollbackFor` hoặc `noRollbackFor`.
  - Ví dụ: `@Transactional(rollbackFor = {SpecificCheckedException.class, AnotherException.class})`
- **Proxying:** Spring sử dụng AOP (Aspect-Oriented Programming) để tạo proxy xung quanh các bean có phương thức `@Transactional`. Khi bạn gọi một phương thức được đánh dấu `@Transactional` từ bên ngoài bean, bạn thực sự đang gọi qua proxy, proxy này sẽ quản lý việc bắt đầu, commit hoặc rollback transaction.
  - **Quan trọng:** Gọi một phương thức `@Transactional` từ một phương thức khác **bên trong cùng một class** (self-invocation) sẽ **không** đi qua proxy và do đó hành vi transaction có thể không như mong đợi (transaction mới sẽ không được tạo nếu propagation là REQUIRED).

Chúng ta đã sử dụng `@Transactional` trên các phương thức service ở trên.

---

**7. Giới thiệu sơ lược về Data Transfer Objects (DTOs)**

- **DTO là gì?**
  - Là các class Java đơn giản (POJO) được sử dụng để truyền dữ liệu giữa các tầng của ứng dụng, đặc biệt là giữa Service và Controller, hoặc giữa Controller và Client (API response/request).
  - Chúng giúp định hình dữ liệu theo đúng nhu cầu của client hoặc của tầng tiếp theo, thay vì lộ toàn bộ cấu trúc của Entity.
- **Tại sao cần DTOs?**
  - **Tách biệt Entity khỏi API:** Không nên expose trực tiếp JPA Entities ra API. Lý do:
    - **Bảo mật:** Entities có thể chứa các trường nhạy cảm (ví dụ: password hash trong `UserAccount`) mà bạn không muốn gửi ra client.
    - **Lazy Loading Issues:** Nếu Entity có các trường được fetch LAZY, khi Jackson (thư viện JSON serialization mặc định của Spring Boot) cố gắng serialize entity ra JSON, nó có thể cố gắng truy cập các trường LAZY này ngoài session của Hibernate, gây ra `LazyInitializationException`.
    - **Vòng lặp vô hạn (Circular Dependencies):** Với các mối quan hệ hai chiều (ví dụ `Product` có `Category`, `Category` có `Set<Product>`), việc serialize trực tiếp có thể gây vòng lặp vô hạn khi Jackson cố gắng serialize qua lại. `@JsonManagedReference` và `@JsonBackReference` có thể giải quyết phần nào, nhưng DTOs là giải pháp sạch sẽ hơn.
    - **API Contract:** API của bạn nên ổn định. Nếu bạn thay đổi cấu trúc Entity (ví dụ: thêm/bỏ trường), API sẽ bị ảnh hưởng nếu expose Entity trực tiếp. DTOs cho phép bạn giữ API contract ổn định ngay cả khi Entity thay đổi.
    - **Dữ liệu không cần thiết:** Client có thể chỉ cần một vài trường từ Entity, không phải tất cả. DTOs giúp gửi đúng lượng dữ liệu cần thiết.
    - **Kết hợp dữ liệu:** DTO có thể kết hợp dữ liệu từ nhiều Entity.
  - **Validation:** DTOs là nơi tốt để đặt các annotation validation (ví dụ: `@NotNull`, `@Size`) cho dữ liệu đầu vào từ client.
- **Ví dụ (rất cơ bản):**
  ```java
  // package com.mycompany.ecommerceproject.dto;
  // public class ProductDTO {
  //     private Long id;
  //     private String name;
  //     private BigDecimal price;
  //     private String categoryName; // Chỉ lấy tên category, không phải cả object Category
  //     // getters, setters, constructors
  // }
  ```
- **Mapping giữa Entity và DTO:**
  - Thủ công: Viết code để copy các trường.
  - Sử dụng thư viện mapping: ModelMapper, MapStruct. MapStruct thường được ưa chuộng hơn vì nó là compile-time (sinh code lúc biên dịch), hiệu năng tốt hơn ModelMapper (runtime reflection).

**Chúng ta sẽ đi sâu vào DTOs và mapping trong Phần 9 (Validation, Exception Handling và DTOs).** Hiện tại, các service của chúng ta vẫn trả về Entity để đơn giản hóa cho các bước đầu.

---

**8. Cập nhật `DataInitializer` (Tùy chọn)**

Bây giờ bạn đã có Service Layer, bạn có thể cân nhắc việc sửa `DataInitializer` để gọi các phương thức service thay vì gọi repository trực tiếp. Điều này giúp `DataInitializer` mô phỏng gần hơn cách ứng dụng thực tế hoạt động.

Ví dụ, thay vì:
`categoryRepository.save(electronics);`

Bạn sẽ inject `CategoryService` và gọi:
`categoryService.save(electronics);`

Điều này không bắt buộc cho `DataInitializer` nhưng là một thực hành tốt để giữ sự nhất quán. Nếu làm vậy, nhớ inject các service vào `DataInitializer` thay vì repository.

```java
// Trong DataInitializer.java
// ...
// private final CategoryService categoryService;
// private final ProductService productService;
// private final UserAccountService userAccountService;

// public DataInitializer(CategoryService categoryService,
//                        ProductService productService,
//                        UserAccountService userAccountService) {
//     this.categoryService = categoryService;
//     this.productService = productService;
//     this.userAccountService = userAccountService;
// }

// @Override
// @Transactional // Vẫn cần Transactional ở đây nếu DataInitializer gọi nhiều service method
// public void run(String... args) throws Exception {
//     // ...
//     categoryService.save(electronics);
//     // ...
//     Product laptop = new Product(...);
//     laptop.setCategory(electronics); // Cần đảm bảo 'electronics' là đối tượng đã được persist và có ID
//     productService.save(laptop);
//     // ...
// }
```

Nếu bạn quyết định cập nhật `DataInitializer`, hãy đảm bảo rằng khi bạn `save` một `Product` và `setCategory()`, `Category` đó phải là một entity đã được quản lý (tức là đã được lưu và có ID, hoặc là một đối tượng `Category` được trả về từ `categoryService.save()`).

---

**9. Best Practices cho Service Layer**

- **Luôn sử dụng `@Transactional`:** Cho các public method thực hiện nghiệp vụ (đặc biệt là các nghiệp vụ ghi dữ liệu).
- **Sử dụng `readOnly = true`:** Cho các nghiệp vụ chỉ đọc để tối ưu.
- **Dependency Injection:** Sử dụng constructor injection.
- **Interface-based Programming:** Cân nhắc sử dụng (khuyến khích).
- **Single Responsibility Principle:** Giữ cho service có trách nhiệm rõ ràng.
- **Xử lý Exception:** Ném các exception tùy chỉnh, rõ ràng để tầng Controller có thể xử lý phù hợp.
- **Không chứa logic liên quan đến HTTP:** Service không nên biết về `HttpServletRequest`, `HttpServletResponse` hay các chi tiết của web framework.
- **DTOs:** Sử dụng DTOs để truyền dữ liệu đến và từ Controller, đặc biệt khi xây dựng API.
- **Idempotency (Tính bất biến lặp lại):** Cố gắng thiết kế các service (đặc biệt là các service ghi dữ liệu) sao cho việc gọi chúng nhiều lần với cùng một tham số sẽ cho cùng một kết quả (hoặc không gây ra tác dụng phụ không mong muốn). Ví dụ: một service "tạo đơn hàng" không nên tạo 2 đơn hàng nếu được gọi 2 lần với cùng thông tin.
- **Testability:** Viết unit test cho các service method, mock các dependency (repository, service khác).

---

**Kết thúc Phần 4**

Chúng ta đã xây dựng thành công Service Layer, nơi chứa đựng logic nghiệp vụ của ứng dụng:

- Hiểu rõ vai trò và lợi ích của Service Layer.
- Tạo các interface và implementation cho `CategoryService`, `ProductService`, `UserAccountService`.
- Triển khai các nghiệp vụ CRUD và một số logic phức tạp hơn.
- Sử dụng `@Transactional` hiệu quả.
- Tạo và sử dụng các custom exception.
- Giới thiệu về DTOs.

Với tầng Service đã hoàn thiện, chúng ta đã có "bộ não" cho ứng dụng. Bây giờ, chúng ta cần một cách để người dùng (hoặc các client khác) tương tác với "bộ não" này.

**Trong Phần 5, chúng ta sẽ: "Xây dựng Controller Layer và hiển thị dữ liệu cơ bản với Thymeleaf."** (Xây dựng các trang web đơn giản để hiển thị danh sách sản phẩm, chi tiết sản phẩm sử dụng template engine Thymeleaf).

Tuyệt vời! Chúng ta sẽ tiến tới việc xây dựng Controller Layer, cầu nối giữa người dùng (thông qua trình duyệt web) và ứng dụng của chúng ta. Đồng thời, chúng ta sẽ sử dụng Thymeleaf để hiển thị dữ liệu một cách động trên các trang HTML.

---

**Phần 5: Xây dựng Controller Layer và hiển thị dữ liệu cơ bản với Thymeleaf**

Trong phần này, chúng ta sẽ:

1.  **Giới thiệu Controller Layer trong Spring MVC.**
2.  **Tìm hiểu về Thymeleaf - Template Engine.**
3.  **Thêm dependency Thymeleaf vào dự án.**
4.  **Cấu hình cơ bản cho Thymeleaf (thường tự động bởi Spring Boot).**
5.  **Tạo các HTML templates với Thymeleaf: layout, header, footer, danh sách sản phẩm, chi tiết sản phẩm.**
6.  **Xây dựng `ProductController` để xử lý các request liên quan đến sản phẩm.**
    - Hiển thị danh sách sản phẩm (có phân trang).
    - Hiển thị chi tiết một sản phẩm.
7.  **Xây dựng `HomeController` để hiển thị trang chủ.**
8.  **Xử lý form cơ bản (ví dụ: tìm kiếm sản phẩm - nếu có thời gian).**
9.  **Truyền dữ liệu từ Controller sang View (HTML template) sử dụng `Model`.**
10. **Best Practices cho Controller Layer.**

---

**1. Giới thiệu Controller Layer trong Spring MVC**

- **Controller Layer là gì?**
  - Trong kiến trúc Model-View-Controller (MVC), Controller là thành phần chịu trách nhiệm xử lý các yêu cầu (requests) từ người dùng (hoặc client), tương tác với Model (thường là thông qua Service Layer) để lấy hoặc cập nhật dữ liệu, và sau đó chọn một View thích hợp để hiển thị kết quả cho người dùng.
- **Vai trò trong Spring MVC:**
  - **Nhận HTTP Requests:** Các class được đánh dấu bằng `@Controller` (hoặc `@RestController` cho API) sẽ lắng nghe các request đến từ client.
  - **Ánh xạ Request (Request Mapping):** Sử dụng các annotation như `@RequestMapping`, `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping` để ánh xạ các URL và HTTP methods cụ thể tới các phương thức xử lý (handler methods) trong controller.
  - **Xử lý Request Parameters:** Trích xuất dữ liệu từ request (query parameters, path variables, request body, form data).
  - **Gọi Service Layer:** Ủy thác việc thực hiện business logic cho các service tương ứng.
  - **Chuẩn bị Model:** Tạo hoặc cập nhật một đối tượng `Model` (hoặc `ModelAndView`) chứa dữ liệu cần thiết để hiển thị trên View.
  - **Chọn View:** Trả về tên của View (ví dụ: tên file HTML template) để Spring MVC render. Đối với `@RestController`, phương thức sẽ trả về dữ liệu (thường là JSON/XML) trực tiếp trong response body.

---

**2. Tìm hiểu về Thymeleaf - Template Engine**

- **Template Engine là gì?**
  - Là một công cụ giúp tách biệt logic trình bày (presentation logic) khỏi logic nghiệp vụ (business logic).
  - Nó cho phép bạn tạo các file template (ví dụ: HTML) với các cú pháp đặc biệt để chèn dữ liệu động vào. Khi template được xử lý, các cú pháp này sẽ được thay thế bằng dữ liệu thực tế.
- **Thymeleaf là gì?**
  - Là một template engine phía server (server-side) hiện đại cho Java, rất phổ biến trong các ứng dụng Spring Boot.
  - **Đặc điểm nổi bật:**
    - **Natural Templates:** Các file template Thymeleaf vẫn là các file HTML hợp lệ, có thể xem trực tiếp trên trình duyệt (mặc dù dữ liệu động sẽ không hiển thị đúng). Điều này rất tốt cho việc thiết kế giao diện.
    - **Tích hợp tốt với Spring:** Spring Boot cung cấp auto-configuration tuyệt vời cho Thymeleaf.
    - **Mở rộng:** Có thể tùy chỉnh và mở rộng.
    - **Bảo mật:** Có các cơ chế bảo vệ chống lại XSS (Cross-Site Scripting) khi hiển thị dữ liệu.
    - **Hỗ trợ Layout Dialect:** Giúp dễ dàng tạo các layout (bố cục) chung cho trang web (header, footer, sidebar).
- **Cú pháp cơ bản của Thymeleaf (sử dụng các thuộc tính `th:*`):**
  - `th:text="'Hello World'"`: Hiển thị văn bản.
  - `th:utext="'<b>Hello</b>'"`: Hiển thị văn bản HTML không được escape (un-escaped HTML - cẩn thận XSS).
  - `th:object="${product}"`, `th:field="*{name}"`: Làm việc với đối tượng và các trường của nó (thường trong form).
  - `th:each="prod : ${products}"`: Lặp qua một collection.
  - `th:if="${condition}"`, `th:unless="${condition}"`: Điều kiện hiển thị.
  - `th:switch`, `th:case`: Cấu trúc switch-case.
  - `th:href="@{/products}"`: Tạo URL (tự động thêm context path nếu có).
  - `th:src="@{/images/logo.png}"`: Tương tự cho source của image.
  - `th:fragment`, `th:insert`, `th:replace`: Để tạo và sử dụng các mảnh template (fragments).

---

**3. Thêm dependency Thymeleaf vào dự án**

Mở file `pom.xml` và thêm dependency sau (nếu chưa có từ lúc khởi tạo dự án):

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>

<!-- (Tùy chọn) Thêm Thymeleaf Layout Dialect để quản lý layout dễ dàng hơn -->
<dependency>
    <groupId>nz.net.ultraq.thymeleaf</groupId>
    <artifactId>thymeleaf-layout-dialect</artifactId>
    <!-- Phiên bản có thể thay đổi, kiểm tra phiên bản mới nhất tương thích -->
</dependency>
```

- `spring-boot-starter-thymeleaf`: Starter này đã bao gồm thư viện Thymeleaf cốt lõi và cấu hình tự động cần thiết.
- `thymeleaf-layout-dialect`: Rất hữu ích để tạo các layout chung.

**Quan trọng:** Sau khi sửa `pom.xml`, nhớ "Load Maven Changes" trong IDE của bạn.

---

**4. Cấu hình cơ bản cho Thymeleaf**

Với Spring Boot, hầu hết cấu hình cho Thymeleaf đã được thực hiện tự động. Tuy nhiên, bạn có thể tùy chỉnh trong `application.properties`:

```properties
# src/main/resources/application.properties

# Thymeleaf settings (thường không cần thiết vì Spring Boot có default tốt)
spring.thymeleaf.prefix=classpath:/templates/  # Nơi tìm kiếm template (mặc định)
spring.thymeleaf.suffix=.html                 # Đuôi file template (mặc định)
spring.thymeleaf.mode=HTML                    # Chế độ template (mặc định là HTML)
spring.thymeleaf.encoding=UTF-8               # Encoding (mặc định)
spring.thymeleaf.cache=false                  # Tắt cache khi phát triển để thấy thay đổi ngay.
                                              # Đặt là true trong production để tăng hiệu suất.

# (Tùy chọn) Cấu hình cho Thymeleaf Layout Dialect
# spring.thymeleaf. चेक-template-location=true (mặc định)
# nz.net.ultraq.thymeleaf.layoutdialect.DECORATOR_DIALECT_PREFIX=layout (mặc định)
# nz.net.ultraq.thymeleaf.layoutdialect. sèche-décorateur-inclusion=true (mặc định)
```

- `spring.thymeleaf.prefix`: Thư mục chứa các file template. Mặc định là `classpath:/templates/`, nghĩa là các file template sẽ nằm trong `src/main/resources/templates/`.
- `spring.thymeleaf.suffix`: Đuôi file mặc định là `.html`.
- `spring.thymeleaf.cache=false`: **Rất quan trọng khi phát triển.** Nó cho phép bạn thay đổi file HTML template và thấy kết quả ngay khi refresh trình duyệt mà không cần khởi động lại ứng dụng. **Nhớ đặt lại thành `true` trong môi trường production.**

---

**5. Tạo các HTML templates với Thymeleaf**

Chúng ta sẽ tạo cấu trúc thư mục trong `src/main/resources/templates/`:

```
src/main/resources/
└── templates/
    ├── layouts/                     <-- Thư mục chứa layout chung
    │   └── default.html
    ├── fragments/                   <-- Thư mục chứa các mảnh template tái sử dụng
    │   ├── header.html
    │   └── footer.html
    ├── home.html                    <-- Trang chủ
    ├── products/                    <-- Thư mục chứa các template liên quan đến product
    │   ├── list.html                <-- Danh sách sản phẩm
    │   └── detail.html              <-- Chi tiết sản phẩm
    └── error.html                   <-- Trang lỗi chung (có thể tùy chỉnh sau)
    └── (các template khác nếu cần)
```

**5.1. Layout Chung (`layouts/default.html`)**
Sử dụng Thymeleaf Layout Dialect.

```html
<!DOCTYPE html>
<html
  xmlns:th="http://www.thymeleaf.org"
  xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title layout:title-pattern="$LAYOUT_TITLE - $CONTENT_TITLE">
      My E-commerce
    </title>
    <!-- Link CSS (ví dụ Bootstrap hoặc custom CSS) -->
    <link rel="stylesheet" th:href="@{/css/bootstrap.min.css}" />
    <!-- Giả sử có file bootstrap.min.css trong static/css -->
    <link rel="stylesheet" th:href="@{/css/style.css}" />
    <!-- Giả sử có file style.css trong static/css -->
    <!-- Thêm các thẻ meta, link khác nếu cần -->
  </head>
  <body>
    <div th:replace="~{fragments/header :: header}">
      <!-- Header content will be replaced here -->
    </div>

    <div class="container mt-4">
      <main layout:fragment="content">
        <!-- Content of specific pages will be inserted here -->
        <p>Default content if no specific content is provided.</p>
      </main>
    </div>

    <div th:replace="~{fragments/footer :: footer}">
      <!-- Footer content will be replaced here -->
    </div>

    <!-- Link JS (ví dụ Bootstrap JS hoặc custom JS) -->
    <script th:src="@{/js/bootstrap.bundle.min.js}"></script>
    <script th:src="@{/js/main.js}"></script>
    <th:block layout:fragment="scripts">
      <!-- Page specific scripts can be added here -->
    </th:block>
  </body>
</html>
```

- `xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"`: Khai báo namespace cho layout dialect.
- `layout:title-pattern="$LAYOUT_TITLE - $CONTENT_TITLE"`: Định dạng tiêu đề trang.
- `th:replace="~{fragments/header :: header}"`: Thay thế `<div>` này bằng fragment tên là `header` từ file `fragments/header.html`. Dấu `~{}` là cú pháp Thymeleaf để tham chiếu fragment.
- `layout:fragment="content"`: Đánh dấu khu vực này là nơi nội dung của các trang con sẽ được chèn vào.
- `th:block layout:fragment="scripts"`: Cho phép các trang con thêm JavaScript cụ thể.

**Lưu ý về CSS/JS:**
Bạn cần tạo thư mục `src/main/resources/static/css` và `src/main/resources/static/js` và đặt các file CSS (ví dụ `bootstrap.min.css`, `style.css`) và JS (ví dụ `bootstrap.bundle.min.js`, `main.js`) vào đó. Bạn có thể tải Bootstrap từ trang chủ của nó. File `style.css` và `main.js` có thể trống ban đầu.

**5.2. Header Fragment (`fragments/header.html`)**

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<body>
    <header th:fragment="header" class="bg-dark text-white p-3 mb-3">
        <div class="container">
            <nav class="navbar navbar-expand-lg navbar-dark">
                <a class="navbar-brand" th:href="@{/}">E-commerce Site</a>
                <button class.navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav"
                        aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                    <span class="navbar-toggler-icon"></span>
                </button>
                <div class="collapse navbar-collapse" id="navbarNav">
                    <ul class="navbar-nav ms-auto">
                        <li class="nav-item">
                            <a class="nav-link" th:href="@{/}">Home</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" th:href="@{/products}">Products</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" th:href="@{/cart}">Cart</a> <!-- Sẽ làm sau -->
                        </li>
                        <!-- Sẽ thêm link Login/Logout/Register sau khi có Spring Security -->
                        <li class="nav-item" th:if="${#authentication == null or #authentication.name == 'anonymousUser'}">
                             <a class="nav-link" th:href="@{/login}">Login</a> <!-- Giả định trang login -->
                        </li>
                         <li class="nav-item" th:if="${#authentication != null and #authentication.name != 'anonymousUser'}">
                            <span class="nav-link" th:text="'Hi, ' + ${#authentication.name}"></span>
                        </li>
                        <li class="nav-item" th:if="${#authentication != null and #authentication.name != 'anonymousUser'}">
                            <form th:action="@{/logout}" method="post" class="d-inline">
                                <button type="submit" class="btn btn-link nav-link">Logout</button>
                            </form>
                        </li>
                    </ul>
                </div>
            </nav>
        </div>
    </header>
</body>
</html>
```

- `th:fragment="header"`: Đánh dấu đây là một fragment tên "header".
- `th:href="@{/}"`: Tạo link đến trang chủ.
- `th:href="@{/products}"`: Tạo link đến trang danh sách sản phẩm.
- Các điều kiện `th:if` sử dụng `#authentication` là để chuẩn bị cho Spring Security sau này. Hiện tại, chúng có thể chưa hoạt động đúng nếu Spring Security chưa được cấu hình.

**5.3. Footer Fragment (`fragments/footer.html`)**

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <body>
    <footer
      th:fragment="footer"
      class="bg-light text-center text-lg-start mt-auto py-3"
    >
      <div class="container">
        <p class="text-center">
          &copy;
          <span th:text="${#dates.year(#dates.createNow())}">2023</span> My
          E-commerce Company. All rights reserved.
        </p>
      </div>
    </footer>
  </body>
</html>
```

- `th:text="${#dates.year(#dates.createNow())}"`: Sử dụng utility object `#dates` của Thymeleaf để lấy năm hiện tại.

**5.4. Trang Chủ (`home.html`)**

```html
<!DOCTYPE html>
<html
  xmlns:th="http://www.thymeleaf.org"
  xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
  layout:decorate="~{layouts/default}"
>
  <head>
    <title>Homepage</title>
  </head>
  <body>
    <section layout:fragment="content">
      <div class="px-4 py-5 my-5 text-center">
        <img
          class="d-block mx-auto mb-4"
          th:src="@{/images/logo.png}"
          alt="Logo"
          width="72"
          height="57"
        />
        <!-- Giả sử có logo.png trong static/images -->
        <h1 class="display-5 fw-bold">Welcome to Our E-commerce Store!</h1>
        <div class="col-lg-6 mx-auto">
          <p class="lead mb-4" th:text="${welcomeMessage}">
            Quickly design and customize responsive mobile-first sites with
            Bootstrap, the world’s most popular front-end open source toolkit.
          </p>
          <div class="d-grid gap-2 d-sm-flex justify-content-sm-center">
            <a th:href="@{/products}" class="btn btn-primary btn-lg px-4 gap-3"
              >Browse Products</a
            >
            <a th:href="@{/about}" class="btn btn-outline-secondary btn-lg px-4"
              >Learn More</a
            >
            <!-- Trang about chưa có -->
          </div>
        </div>
      </div>

      <!-- Có thể thêm các section khác như "Featured Products", "New Arrivals" ở đây -->
      <h2>Featured Products</h2>
      <div
        class="row"
        th:if="${featuredProducts != null and not #lists.isEmpty(featuredProducts)}"
      >
        <div class="col-md-4 mb-3" th:each="product : ${featuredProducts}">
          <div class="card">
            <a th:href="@{/products/{id}(id=${product.id})}">
              <img
                th:src="${product.imageUrl != null ? product.imageUrl : '/images/placeholder.png'}"
                class="card-img-top"
                alt="Product Image"
                style="height: 200px; object-fit: cover;"
              />
            </a>
            <div class="card-body">
              <h5 class="card-title" th:text="${product.name}">Product Name</h5>
              <p
                class="card-text"
                th:text="${#numbers.formatCurrency(product.price)}"
              >
                Price
              </p>
              <a
                th:href="@{/products/{id}(id=${product.id})}"
                class="btn btn-primary"
                >View Details</a
              >
              <!-- Nút Add to Cart sẽ làm sau -->
            </div>
          </div>
        </div>
      </div>
      <div
        th:if="${featuredProducts == null or #lists.isEmpty(featuredProducts)}"
      >
        <p>No featured products available at the moment.</p>
      </div>
    </section>
  </body>
</html>
```

- `layout:decorate="~{layouts/default}"`: Chỉ định rằng trang này sẽ sử dụng layout `default.html`.
- `<section layout:fragment="content"> ... </section>`: Nội dung bên trong `section` này sẽ được chèn vào vị trí `layout:fragment="content"` của `default.html`.
- `th:text="${welcomeMessage}"`: Hiển thị biến `welcomeMessage` được truyền từ controller.
- `th:if="${featuredProducts != null and not #lists.isEmpty(featuredProducts)}"`: Kiểm tra danh sách sản phẩm nổi bật.
- `th:each="product : ${featuredProducts}"`: Lặp qua danh sách sản phẩm.
- `th:href="@{/products/{id}(id=${product.id})}"`: Tạo URL động cho chi tiết sản phẩm, ví dụ `/products/1`.
- `th:src="${product.imageUrl != null ? product.imageUrl : '/images/placeholder.png'}"`: Hiển thị ảnh sản phẩm, nếu không có thì dùng ảnh placeholder (tạo `placeholder.png` trong `static/images`).
- `${#numbers.formatCurrency(product.price)}`: Định dạng giá tiền.

**5.5. Danh sách Sản phẩm (`products/list.html`)**

```html
<!DOCTYPE html>
<html
  xmlns:th="http://www.thymeleaf.org"
  xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
  layout:decorate="~{layouts/default}"
>
  <head>
    <title>Product List</title>
  </head>
  <body>
    <section layout:fragment="content">
      <div class="d-flex justify-content-between align-items-center mb-3">
        <h1>All Products</h1>
        <!-- Search form (sẽ làm sau nếu có thời gian) -->
        <!-- <form th:action="@{/products/search}" method="get" class="d-flex">
                <input class="form-control me-2" type="search" name="keyword" placeholder="Search products..." aria-label="Search">
                <button class="btn btn-outline-success" type="submit">Search</button>
            </form> -->
      </div>

      <div th:if="${productsPage == null or productsPage.empty}">
        <p>No products found.</p>
      </div>

      <div th:if="${productsPage != null and not productsPage.empty}">
        <div class="row">
          <div
            class="col-md-4 mb-4"
            th:each="product : ${productsPage.content}"
          >
            <div class="card h-100">
              <a th:href="@{/products/{id}(id=${product.id})}">
                <img
                  th:src="${product.imageUrl != null and product.imageUrl != '' ? product.imageUrl : '/images/placeholder.png'}"
                  class="card-img-top"
                  alt="Product Image"
                  style="height: 200px; object-fit: cover;"
                />
              </a>
              <div class="card-body d-flex flex-column">
                <h5 class="card-title">
                  <a
                    th:href="@{/products/{id}(id=${product.id})}"
                    th:text="${product.name}"
                    class="text-decoration-none"
                    >Product Name</a
                  >
                </h5>
                <p
                  class="card-text"
                  th:text="${product.category != null ? product.category.name : 'Uncategorized'}"
                >
                  Category
                </p>
                <p class="card-text mt-auto">
                  <strong th:text="${#numbers.formatCurrency(product.price)}"
                    >Price</strong
                  >
                </p>
                <div class="mt-2">
                  <a
                    th:href="@{/products/{id}(id=${product.id})}"
                    class="btn btn-sm btn-outline-primary"
                    >View Details</a
                  >
                  <!-- Nút Add to Cart sẽ làm sau -->
                  <!-- <a href="#" class="btn btn-sm btn-primary">Add to Cart</a> -->
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Pagination -->
        <nav
          aria-label="Page navigation"
          th:if="${productsPage.totalPages > 1}"
        >
          <ul class="pagination justify-content-center">
            <!-- Previous Page Link -->
            <li
              class="page-item"
              th:classappend="${productsPage.first ? 'disabled' : ''}"
            >
              <a
                class="page-link"
                th:href="@{/products(page=${productsPage.number - 1}, size=${productsPage.size})}"
                >Previous</a
              >
            </li>

            <!-- Page Numbers -->
            <li
              class="page-item"
              th:each="i : ${#numbers.sequence(0, productsPage.totalPages - 1)}"
              th:classappend="${i == productsPage.number ? 'active' : ''}"
            >
              <a
                class="page-link"
                th:href="@{/products(page=${i}, size=${productsPage.size})}"
                th:text="${i + 1}"
                >1</a
              >
            </li>

            <!-- Next Page Link -->
            <li
              class="page-item"
              th:classappend="${productsPage.last ? 'disabled' : ''}"
            >
              <a
                class="page-link"
                th:href="@{/products(page=${productsPage.number + 1}, size=${productsPage.size})}"
                >Next</a
              >
            </li>
          </ul>
        </nav>
      </div>
    </section>
  </body>
</html>
```

- `th:each="product : ${productsPage.content}"`: Lặp qua danh sách sản phẩm trong trang hiện tại (`productsPage` là đối tượng `Page<Product>`).
- **Pagination:**
  - `th:if="${productsPage.totalPages > 1}"`: Chỉ hiển thị phân trang nếu có nhiều hơn 1 trang.
  - `productsPage.first`, `productsPage.last`, `productsPage.number` (số trang hiện tại, 0-based), `productsPage.size`, `productsPage.totalPages`.
  - `#numbers.sequence(0, productsPage.totalPages - 1)`: Tạo một dãy số từ 0 đến `totalPages - 1` để lặp qua các số trang.
  - `th:href="@{/products(page=${...}, size=${...})}"`: Tạo link phân trang với query parameters `page` và `size`.

**5.6. Chi tiết Sản phẩm (`products/detail.html`)**

```html
<!DOCTYPE html>
<html
  xmlns:th="http://www.thymeleaf.org"
  xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
  layout:decorate="~{layouts/default}"
>
  <head>
    <title th:text="${product != null ? product.name : 'Product Not Found'}">
      Product Detail
    </title>
  </head>
  <body>
    <section layout:fragment="content">
      <div th:if="${product == null}">
        <div class="alert alert-danger" role="alert">Product not found!</div>
        <a th:href="@{/products}" class="btn btn-primary"
          >Back to Product List</a
        >
      </div>

      <div th:if="${product != null}">
        <div class="row">
          <div class="col-md-6">
            <img
              th:src="${product.imageUrl != null and product.imageUrl != '' ? product.imageUrl : '/images/placeholder.png'}"
              class="img-fluid rounded"
              alt="Product Image"
              style="max-height: 400px; object-fit: cover;"
            />
          </div>
          <div class="col-md-6">
            <h1 th:text="${product.name}">Product Name</h1>
            <p
              class="text-muted"
              th:text="${product.category != null ? product.category.name : 'Uncategorized'}"
            >
              Category
            </p>
            <h2>
              <strong th:text="${#numbers.formatCurrency(product.price)}"
                >Price</strong
              >
            </h2>
            <p
              th:if="${product.stockQuantity > 0 and product.stockQuantity <= 10}"
              class="text-warning"
            >
              Only <span th:text="${product.stockQuantity}"></span> items left
              in stock!
            </p>
            <p th:if="${product.stockQuantity > 10}" class="text-success">
              In Stock
            </p>
            <p th:if="${product.stockQuantity <= 0}" class="text-danger">
              Out of Stock
            </p>

            <hr />
            <h4>Description</h4>
            <p th:text="${product.description}">
              Product description goes here.
            </p>
            <hr />

            <!-- Add to Cart Form (sẽ làm sau) -->
            <form
              th:action="@{/cart/add}"
              method="post"
              th:if="${product.stockQuantity > 0}"
            >
              <input type="hidden" name="productId" th:value="${product.id}" />
              <div class="input-group mb-3" style="max-width: 150px;">
                <label for="quantity" class="visually-hidden">Quantity</label>
                <input
                  type="number"
                  name="quantity"
                  id="quantity"
                  class="form-control"
                  value="1"
                  min="1"
                  th:max="${product.stockQuantity}"
                />
              </div>
              <button type="submit" class="btn btn-primary btn-lg">
                Add to Cart
              </button>
            </form>
            <div th:if="${product.stockQuantity <= 0}">
              <button type="button" class="btn btn-secondary btn-lg" disabled>
                Out of Stock
              </button>
            </div>

            <div class="mt-3">
              <a th:href="@{/products}" class="btn btn-outline-secondary"
                >Back to Product List</a
              >
            </div>
          </div>
        </div>

        <!-- Related Products (Optional) -->
        <!--
            <div class="mt-5" th:if="${relatedProducts != null and not #lists.isEmpty(relatedProducts)}">
                <h3>Related Products</h3>
                <div class="row">
                    <div class="col-md-3 mb-3" th:each="relatedProd : ${relatedProducts}">
                        <div class="card h-100">
                            <a th:href="@{/products/{id}(id=${relatedProd.id})}">
                                <img th:src="${relatedProd.imageUrl != null ? relatedProd.imageUrl : '/images/placeholder.png'}"
                                     class="card-img-top" alt="Product Image" style="height: 150px; object-fit: cover;">
                            </a>
                            <div class="card-body">
                                <h6 class="card-title">
                                    <a th:href="@{/products/{id}(id=${relatedProd.id})}" th:text="${relatedProd.name}" class="text-decoration-none"></a>
                                </h6>
                                <p class="card-text"><strong th:text="${#numbers.formatCurrency(relatedProd.price)}"></strong></p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            -->
      </div>
    </section>
  </body>
</html>
```

- `th:text="${product != null ? product.name : 'Product Not Found'}"`: Đặt tiêu đề động.
- Kiểm tra `th:if="${product == null}"` để hiển thị thông báo nếu không tìm thấy sản phẩm.

---

**6. Xây dựng `ProductController`**

Tạo package `controller` trong `com.mycompany.ecommerceproject` (nếu chưa có từ Phần 1).
Tạo file `ProductController.java`:

```java
package com.mycompany.ecommerceproject.controller;

import com.mycompany.ecommerceproject.entity.Product;
import com.mycompany.ecommerceproject.service.ProductService;
import com.mycompany.ecommerceproject.exception.ResourceNotFoundException; // Import custom exception
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;
import org.springframework.data.domain.Sort;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;

import java.util.Optional;

@Controller
@RequestMapping("/products") // Tất cả các request đến /products sẽ được xử lý bởi controller này
public class ProductController {

    private static final Logger logger = LoggerFactory.getLogger(ProductController.class);
    private final ProductService productService;

    public ProductController(ProductService productService) {
        this.productService = productService;
    }

    @GetMapping
    public String listProducts(Model model,
                               @RequestParam(name = "page", defaultValue = "0") int page,
                               @RequestParam(name = "size", defaultValue = "9") int size, // Hiển thị 9 sản phẩm mỗi trang
                               @RequestParam(name = "sort", defaultValue = "name,asc") String[] sort) {
        logger.info("Request to list products - page: {}, size: {}, sort: {}", page, size, sort);

        String sortField = sort[0];
        Sort.Direction sortDirection = (sort.length > 1 && sort[1].equalsIgnoreCase("desc")) ?
                                       Sort.Direction.DESC : Sort.Direction.ASC;

        Pageable pageable = PageRequest.of(page, size, Sort.by(sortDirection, sortField));
        Page<Product> productsPage = productService.findAll(pageable);

        model.addAttribute("productsPage", productsPage);
        model.addAttribute("currentPage", page);
        model.addAttribute("pageSize", size);
        model.addAttribute("sortField", sortField);
        model.addAttribute("sortDir", sortDirection.name().toLowerCase());

        // Các thuộc tính để xây dựng link sắp xếp
        model.addAttribute("reverseSortDir", sortDirection == Sort.Direction.ASC ? "desc" : "asc");


        return "products/list"; // Trả về tên view: src/main/resources/templates/products/list.html
    }

    @GetMapping("/{id}")
    public String productDetail(@PathVariable("id") Long id, Model model) {
        logger.info("Request to view product detail for id: {}", id);
        try {
            Product product = productService.findById(id)
                    .orElseThrow(() -> new ResourceNotFoundException("Product not found with id: " + id));
            model.addAttribute("product", product);

            // (Tùy chọn) Lấy các sản phẩm liên quan (ví dụ: cùng category)
            // Pageable relatedProductsPageable = PageRequest.of(0, 4); // Lấy 4 sản phẩm liên quan
            // if (product.getCategory() != null) {
            //     Page<Product> relatedProducts = productService.findByCategoryIdAndNotProductId(
            //             product.getCategory().getId(), product.getId(), relatedProductsPageable);
            //     model.addAttribute("relatedProducts", relatedProducts.getContent());
            // }

            return "products/detail"; // Trả về view: src/main/resources/templates/products/detail.html
        } catch (ResourceNotFoundException ex) {
            logger.warn("Product not found for id {}: {}", id, ex.getMessage());
            // Có thể chuyển hướng đến trang lỗi tùy chỉnh hoặc hiển thị thông báo ngay trên trang list
            // model.addAttribute("errorMessage", ex.getMessage());
            // return "error/404"; // Giả sử có trang error/404.html
            // Hoặc để GlobalExceptionHandler xử lý (sẽ học sau)
            // Hiện tại, detail.html có xử lý trường hợp product là null
            model.addAttribute("product", null); // Truyền null để template biết không tìm thấy
            return "products/detail";
        }
    }

    // (Tùy chọn) Handler cho tìm kiếm sản phẩm
    // @GetMapping("/search")
    // public String searchProducts(@RequestParam("keyword") String keyword,
    //                              Model model,
    //                              @RequestParam(name = "page", defaultValue = "0") int page,
    //                              @RequestParam(name = "size", defaultValue = "9") int size) {
    //     logger.info("Request to search products with keyword: {}", keyword);
    //     Pageable pageable = PageRequest.of(page, size);
    //     Page<Product> productsPage = productService.searchProducts(keyword, pageable);
    //     model.addAttribute("productsPage", productsPage);
    //     model.addAttribute("keyword", keyword); // Để hiển thị lại keyword trên thanh search
    //     return "products/list"; // Tái sử dụng view list, có thể cần điều chỉnh view để hiển thị kết quả search
    // }
}
```

- `@Controller`: Đánh dấu class này là một Spring MVC Controller.
- `@RequestMapping("/products")`: Tất cả các request bắt đầu bằng `/products` sẽ được controller này xử lý.
- `@GetMapping`: Ánh xạ HTTP GET request.
  - Không có path: `/products` (cho danh sách).
  - `/{id}`: `/products/1`, `/products/2` (cho chi tiết). `id` là một path variable.
- `Model model`: Spring sẽ tự động inject một đối tượng `Model`. Bạn dùng nó để truyền dữ liệu từ controller sang view. `model.addAttribute("attributeName", attributeValue);`.
- `@RequestParam`: Để lấy giá trị từ query parameter.
  - `defaultValue`: Nếu không có parameter này trong URL, giá trị mặc định sẽ được dùng.
- `@PathVariable`: Để lấy giá trị từ path variable.
- `Pageable pageable = PageRequest.of(page, size, Sort.by(...))`: Tạo đối tượng `Pageable`.
- Trong `productDetail`, chúng ta bắt `ResourceNotFoundException` để xử lý trường hợp không tìm thấy sản phẩm.

---

**7. Xây dựng `HomeController`**

Tạo file `HomeController.java` trong package `controller`:

```java
package com.mycompany.ecommerceproject.controller;

import com.mycompany.ecommerceproject.entity.Product;
import com.mycompany.ecommerceproject.service.ProductService;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Sort;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HomeController {

    private static final Logger logger = LoggerFactory.getLogger(HomeController.class);
    private final ProductService productService;

    public HomeController(ProductService productService) {
        this.productService = productService;
    }

    @GetMapping("/") // Ánh xạ request đến root path
    public String home(Model model) {
        logger.info("Request to home page");
        model.addAttribute("welcomeMessage", "Discover a world of products at your fingertips!");

        // Lấy một vài sản phẩm nổi bật (ví dụ: 4 sản phẩm mới nhất hoặc bán chạy nhất)
        // Hiện tại, lấy 4 sản phẩm đầu tiên sắp xếp theo tên hoặc ngày tạo (nếu có)
        Page<Product> featuredProductsPage = productService.findAll(PageRequest.of(0, 4, Sort.by("createdAt").descending()));
        // Nếu không có createdAt, có thể dùng "id" hoặc "name"
        // Page<Product> featuredProductsPage = productService.findAll(PageRequest.of(0, 4, Sort.by("name").ascending()));

        model.addAttribute("featuredProducts", featuredProductsPage.getContent());

        return "home"; // Trả về view: src/main/resources/templates/home.html
    }

    // Có thể thêm các trang tĩnh khác như /about, /contact
    // @GetMapping("/about")
    // public String aboutPage() {
    //     return "about"; // src/main/resources/templates/about.html
    // }
}
```

---

**8. Xử lý form cơ bản (Đã comment out trong ví dụ)**

Phần tìm kiếm sản phẩm đã được comment out trong `ProductController` và `products/list.html`. Nếu bạn muốn kích hoạt:

1.  Bỏ comment phần form search trong `products/list.html`.
2.  Bỏ comment phương thức `searchProducts` trong `ProductController.java`.
3.  Đảm bảo `ProductService` có phương thức `searchProducts` và `ProductRepository` có query tương ứng (như đã sửa ở Phần 4).

---

**9. Truyền dữ liệu từ Controller sang View (Đã làm ở trên)**

Sử dụng `model.addAttribute("key", value);`. Trong template Thymeleaf, bạn có thể truy cập `value` bằng `${key}`.

---

**10. Best Practices cho Controller Layer**

- **Mỏng (Thin Controllers):** Controller chỉ nên làm nhiệm vụ điều phối, nhận request, gọi service, chuẩn bị model, và trả về view/response. **Không chứa business logic.**
- **Sử dụng `@RequestParam` và `@PathVariable`:** Để lấy dữ liệu từ request một cách rõ ràng.
- **Validation:** Validate dữ liệu đầu vào (request parameters, request body) ở tầng Controller (hoặc Service). Chúng ta sẽ tìm hiểu kỹ hơn về Bean Validation ở phần sau.
- **Không trả về Entities trực tiếp trong API (đối với `@RestController`):** Sử dụng DTOs. Đối với `@Controller` trả về view Thymeleaf, việc truyền Entities vào `Model` thường chấp nhận được vì Thymeleaf chạy trên server và có thể xử lý lazy loading (nếu transaction còn mở) hoặc bạn đã fetch EAGER các quan hệ cần thiết. Tuy nhiên, nếu Entity quá phức tạp hoặc có vòng lặp, việc map sang DTO trước khi đưa vào Model vẫn là lựa chọn tốt.
- **Xử lý Exception:**
  - Sử dụng `@ExceptionHandler` trong controller để xử lý các exception cụ thể của controller đó.
  - Sử dụng `@ControllerAdvice` để tạo Global Exception Handler, xử lý exception cho toàn bộ ứng dụng (sẽ học sau).
- **PRG Pattern (Post/Redirect/Get):** Khi xử lý form submission (HTTP POST), sau khi xử lý thành công, nên redirect đến một trang GET khác để tránh việc người dùng refresh trình duyệt và gửi lại form.
  - Ví dụ: `return "redirect:/products";`
- **Giữ URL nhất quán và RESTful (nếu có thể):**
  - Ví dụ: `/products` (GET - list), `/products/{id}` (GET - detail), `/products/new` (GET - form tạo mới), `/products` (POST - tạo mới).
- **Logging:** Ghi log các request quan trọng, các lỗi, hoặc các thông tin debug cần thiết.

---

**Chạy thử ứng dụng:**

1.  Chạy ứng dụng Spring Boot.
2.  Mở trình duyệt và truy cập:
    - `http://localhost:8080/` (Trang chủ)
    - `http://localhost:8080/products` (Danh sách sản phẩm)
    - Click vào một sản phẩm để xem chi tiết, ví dụ `http://localhost:8080/products/1` (nếu sản phẩm có ID=1 tồn tại).
    - Thử các link phân trang.

Bạn sẽ thấy các trang HTML được render với dữ liệu từ `DataInitializer` (nếu bạn còn giữ nó) hoặc từ CSDL.

**Kết thúc Phần 5**

Chúng ta đã xây dựng thành công Controller Layer và có thể hiển thị dữ liệu lên giao diện web bằng Thymeleaf:

- Hiểu về Controller và Thymeleaf.
- Tạo layout, fragment và các trang HTML.
- Xây dựng `ProductController` và `HomeController`.
- Truyền dữ liệu và xử lý phân trang.

Ứng dụng của chúng ta đã bắt đầu có hình hài! Người dùng đã có thể xem sản phẩm.

**Trong Phần 6, chúng ta sẽ đi vào một phần rất quan trọng: "Quản lý người dùng và Bảo mật với Spring Security."** (Đăng ký, đăng nhập, phân quyền cơ bản). Đây là một chủ đề lớn và khá phức tạp, nhưng rất cần thiết.

Tuyệt vời! Chúng ta sẽ bước vào một trong những phần quan trọng và cũng khá thử thách nhất của một ứng dụng web: Bảo mật và Quản lý người dùng. Spring Security là một framework mạnh mẽ và linh hoạt để giải quyết vấn đề này.

---

**Phần 6: Quản lý người dùng và Bảo mật với Spring Security**

Trong phần này, chúng ta sẽ:

1.  **Giới thiệu về Spring Security và các khái niệm cốt lõi.** (Authentication, Authorization, Principal, GrantedAuthority, SecurityContextHolder, Filters).
2.  **Thêm dependency Spring Security.**
3.  **Cấu hình Spring Security cơ bản:**
    - Tạo một class cấu hình kế thừa `WebSecurityConfigurerAdapter` (cho Spring Boot < 2.7) hoặc sử dụng `SecurityFilterChain` bean (khuyến nghị cho Spring Boot 2.7+ và 3.x). Chúng ta sẽ dùng cách mới với `SecurityFilterChain`.
    - Cấu hình `PasswordEncoder` (BCrypt).
    - Cấu hình `UserDetailsService` để tải thông tin người dùng từ database (sử dụng `UserAccountService` đã có).
4.  **Cập nhật `UserAccountService` để làm việc với `UserDetails` và `PasswordEncoder`.**
5.  **Tạo trang Đăng nhập (Login Page) tùy chỉnh bằng Thymeleaf.**
6.  **Xử lý đăng nhập và đăng xuất.**
7.  **Cấu hình quyền truy cập (Authorization):**
    - Cho phép tất cả truy cập một số trang (trang chủ, danh sách sản phẩm, chi tiết sản phẩm, trang đăng nhập, trang đăng ký).
    - Yêu cầu đăng nhập cho các trang khác (ví dụ: giỏ hàng, đặt hàng - sẽ làm sau).
    - Phân quyền dựa trên vai trò (ví dụ: chỉ ADMIN mới được truy cập trang quản trị - sẽ làm sau).
8.  **Tạo trang Đăng ký (Registration Page) và Controller xử lý đăng ký.**
9.  **Hiển thị thông tin người dùng đã đăng nhập trên giao diện (ví dụ: trong header).**
10. **Best Practices và Lưu ý khi làm việc với Spring Security.**

---

**1. Giới thiệu về Spring Security và các khái niệm cốt lõi**

- **Spring Security là gì?**

  - Là một framework mạnh mẽ và có khả năng tùy biến cao, cung cấp các dịch vụ xác thực (authentication) và ủy quyền (authorization) cho các ứng dụng Java, đặc biệt là các ứng dụng xây dựng trên Spring Framework.
  - Nó bảo vệ ứng dụng của bạn khỏi các mối đe dọa bảo mật phổ biến như CSRF (Cross-Site Request Forgery), Session Fixation, v.v.

- **Các khái niệm cốt lõi:**
  - **Authentication (Xác thực):**
    - Là quá trình xác minh danh tính của người dùng (hoặc một hệ thống khác). Ai là bạn?
    - Thường liên quan đến việc kiểm tra username và password, nhưng cũng có thể là token, certificate, v.v.
    - Kết quả của quá trình xác thực thành công là một đối tượng `Authentication` chứa thông tin về người dùng đã được xác thực (principal) và các quyền của họ (authorities).
  - **Authorization (Ủy quyền/Phân quyền):**
    - Là quá trình quyết định xem một người dùng đã được xác thực có được phép thực hiện một hành động cụ thể hoặc truy cập một tài nguyên cụ thể hay không. Bạn được phép làm gì?
    - Dựa trên các quyền (authorities/roles) của người dùng.
  - **Principal (Đối tượng định danh):**
    - Đại diện cho người dùng hoặc thực thể đã được xác thực. Có thể là một đối tượng `UserDetails` của Spring Security, hoặc một chuỗi (username).
  - **GrantedAuthority (Quyền hạn được cấp):**
    - Đại diện cho một quyền được cấp cho principal. Thường là các vai trò (roles, ví dụ: `ROLE_USER`, `ROLE_ADMIN`) hoặc các quyền chi tiết hơn (permissions, ví dụ: `product:read`, `product:write`).
    - Trong Spring Security, vai trò thường được tiền tố là `ROLE_`.
  - **SecurityContextHolder:**
    - Là nơi Spring Security lưu trữ thông tin về principal hiện tại đã được xác thực.
    - `SecurityContextHolder.getContext().getAuthentication()` trả về đối tượng `Authentication` của người dùng hiện tại.
  - **UserDetailsService:**
    - Một interface trong Spring Security mà bạn cần implement.
    - Nó có một phương thức duy nhất `loadUserByUsername(String username)` trả về một đối tượng `UserDetails`.
    - Spring Security sử dụng `UserDetailsService` để tải thông tin người dùng (bao gồm password đã hash và các quyền) trong quá trình xác thực.
  - **UserDetails:**
    - Một interface trong Spring Security đại diện cho thông tin cốt lõi của người dùng.
    - Nó chứa username, password (đã hash), danh sách các `GrantedAuthority`, và các trạng thái tài khoản (enabled, accountNonExpired, credentialsNonExpired, accountNonLocked).
  - **PasswordEncoder:**
    - Một interface để mã hóa và kiểm tra mật khẩu.
    - **Rất quan trọng:** Không bao giờ lưu mật khẩu dạng plain text. Luôn sử dụng một thuật toán hashing mạnh như BCrypt, SCrypt, hoặc Argon2. `BCryptPasswordEncoder` là một lựa chọn phổ biến và an toàn.
  - **Security Filters (Bộ lọc bảo mật):**
    - Spring Security hoạt động dựa trên một chuỗi các Servlet Filters. Mỗi filter có một trách nhiệm cụ thể (ví dụ: `UsernamePasswordAuthenticationFilter` xử lý xác thực form login, `FilterSecurityInterceptor` xử lý ủy quyền).

---

**2. Thêm dependency Spring Security**

Mở file `pom.xml` và thêm dependency sau:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>

<!-- Dependency này cần thiết để Spring Security tích hợp với Thymeleaf (ví dụ: sử dụng sec:authorize, #authentication) -->
<dependency>
    <groupId>org.thymeleaf.extras</groupId>
    <artifactId>thymeleaf-extras-springsecurity6</artifactId>
    <!-- Kiểm tra phiên bản mới nhất. Spring Boot 3.x dùng springsecurity6, 2.x dùng springsecurity5 -->
</dependency>
```

**Quan trọng:** Sau khi sửa `pom.xml`, nhớ "Load Maven Changes".

**Lưu ý:** Ngay sau khi bạn thêm `spring-boot-starter-security` và khởi động lại ứng dụng, Spring Boot sẽ tự động kích hoạt các cấu hình bảo mật mặc định, bao gồm:

- Yêu cầu xác thực cho tất cả các request.
- Tạo một trang đăng nhập mặc định (rất cơ bản).
- Tạo một người dùng mặc định tên là `user` với mật khẩu được sinh ngẫu nhiên và in ra console khi khởi động.
- Bật tính năng bảo vệ CSRF.

Chúng ta sẽ tùy chỉnh các hành vi này.

---

**3. Cấu hình Spring Security cơ bản**

Tạo một package mới `config` trong `com.mycompany.ecommerceproject`.
Bên trong package `config`, tạo class `SecurityConfig.java`.

```java
package com.mycompany.ecommerceproject.config;

import com.mycompany.ecommerceproject.service.UserAccountService; // UserAccountService của bạn
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.dao.DaoAuthenticationProvider;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.method.configuration.EnableMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.util.matcher.AntPathRequestMatcher;

@Configuration
@EnableWebSecurity // Kích hoạt hỗ trợ bảo mật web của Spring Security
@EnableMethodSecurity(prePostEnabled = true, securedEnabled = true, jsr250Enabled = true) // Kích hoạt bảo mật ở mức phương thức (tùy chọn)
public class SecurityConfig {

    private final UserAccountService userAccountService; // Sẽ được inject

    public SecurityConfig(UserAccountService userAccountService) {
        this.userAccountService = userAccountService;
    }

    // Bean để mã hóa mật khẩu
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    // Bean để cung cấp UserDetailsService cho Spring Security
    // UserAccountService của chúng ta cần implement UserDetailsService
    @Bean
    public UserDetailsService userDetailsService() {
        // Chúng ta sẽ sửa UserAccountServiceImpl để implement UserDetailsService
        // và UserAccount để implement UserDetails
        // Hoặc, tạo một UserDetailsServiceImpl riêng biệt
        return username -> userAccountService.loadUserByUsername(username);
    }


    // Bean để cấu hình AuthenticationProvider sử dụng UserDetailsService và PasswordEncoder
    @Bean
    public DaoAuthenticationProvider authenticationProvider() {
        DaoAuthenticationProvider authProvider = new DaoAuthenticationProvider();
        authProvider.setUserDetailsService(userDetailsService());
        authProvider.setPasswordEncoder(passwordEncoder());
        return authProvider;
    }

    // Bean để lấy AuthenticationManager
    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration authConfig) throws Exception {
        return authConfig.getAuthenticationManager();
    }


    // Bean để cấu hình chuỗi filter bảo mật (thay thế cho WebSecurityConfigurerAdapter)
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authorize -> authorize
                // Cho phép truy cập không cần xác thực vào các URL này
                .requestMatchers("/", "/home", "/css/**", "/js/**", "/images/**", "/webjars/**").permitAll()
                .requestMatchers("/products", "/products/{\\d+}", "/products/category/{\\d+}").permitAll() // {\\d+} để khớp ID là số
                .requestMatchers("/register", "/login").permitAll() // Trang đăng ký và đăng nhập
                // .requestMatchers("/admin/**").hasRole("ADMIN") // Ví dụ: Yêu cầu vai trò ADMIN cho /admin/**
                // .requestMatchers("/user/**").hasAnyRole("USER", "ADMIN") // Yêu cầu vai trò USER hoặc ADMIN
                .anyRequest().authenticated() // Tất cả các request khác đều yêu cầu xác thực
            )
            .formLogin(formLogin -> formLogin
                .loginPage("/login") // URL của trang đăng nhập tùy chỉnh
                .loginProcessingUrl("/perform_login") // URL mà Spring Security sẽ xử lý việc submit form login
                .defaultSuccessUrl("/", true) // Chuyển hướng đến trang chủ sau khi đăng nhập thành công
                .failureUrl("/login?error=true") // Chuyển hướng đến trang login với tham số error nếu đăng nhập thất bại
                .permitAll() // Cho phép tất cả truy cập URL của form login
            )
            .logout(logout -> logout
                .logoutRequestMatcher(new AntPathRequestMatcher("/logout")) // URL để thực hiện logout
                .logoutSuccessUrl("/login?logout=true") // Chuyển hướng sau khi logout thành công
                .invalidateHttpSession(true) // Hủy session
                .deleteCookies("JSESSIONID") // Xóa cookie session (tùy chọn)
                .permitAll()
            )
            // .csrf(csrf -> csrf.disable()) // Tạm thời tắt CSRF để test POST dễ hơn, SẼ BẬT LẠI VÀ XỬ LÝ SAU
            // .rememberMe(rememberMe -> rememberMe.key("uniqueAndSecret").tokenValiditySeconds(86400)) // Cấu hình Remember Me
            ;

        // Đăng ký AuthenticationProvider
        http.authenticationProvider(authenticationProvider());

        return http.build();
    }
}
```

**Giải thích `SecurityConfig`:**

- `@Configuration`, `@EnableWebSecurity`: Các annotation cần thiết.
- `@EnableMethodSecurity`: (Tùy chọn) Cho phép sử dụng các annotation như `@PreAuthorize`, `@PostAuthorize`, `@Secured` trên các phương thức service để kiểm soát quyền truy cập chi tiết hơn.
- `passwordEncoder()`: Tạo một bean `BCryptPasswordEncoder`. Đây là chuẩn để hash password.
- `userDetailsService()`: Cung cấp một `UserDetailsService`. Chúng ta sẽ cần điều chỉnh `UserAccountService` của mình để nó có thể hoạt động như một `UserDetailsService` hoặc tạo một service riêng cho việc này. Cách đơn giản là để `UserAccountService` có một phương thức `loadUserByUsername` trả về `UserDetails`.
- `authenticationProvider()`: Tạo một `DaoAuthenticationProvider`, là một `AuthenticationProvider` sử dụng `UserDetailsService` để lấy thông tin người dùng và `PasswordEncoder` để kiểm tra mật khẩu.
- `authenticationManager()`: Bean này cần thiết nếu bạn muốn tự thực hiện xác thực ở đâu đó (ví dụ trong một controller), nhưng với form login tiêu chuẩn, Spring Security sẽ tự dùng.
- `securityFilterChain(HttpSecurity http)`: Đây là nơi chính để cấu hình bảo mật.
  - `authorizeHttpRequests`: Cấu hình quyền truy cập cho các URL.
    - `requestMatchers(...).permitAll()`: Cho phép truy cập các URL này mà không cần đăng nhập. Chú ý `{\\d+}` là regex để khớp ID là số.
    - `requestMatchers(...).hasRole("ADMIN")`: Yêu cầu người dùng phải có vai trò `ADMIN`. Spring Security tự động thêm tiền tố `ROLE_` nên khi lưu trong DB, vai trò nên là `ROLE_ADMIN`, nhưng khi dùng `hasRole()`, bạn chỉ cần ghi `ADMIN`.
    - `anyRequest().authenticated()`: Mọi request khác chưa được định nghĩa ở trên đều yêu cầu người dùng phải đăng nhập.
  - `formLogin`: Cấu hình xác thực dựa trên form.
    - `.loginPage("/login")`: Chỉ định URL của trang đăng nhập tùy chỉnh của bạn.
    - `.loginProcessingUrl("/perform_login")`: URL mà form đăng nhập sẽ POST tới. Spring Security sẽ tự động xử lý request này.
    - `.defaultSuccessUrl("/", true)`: Trang chuyển hướng đến sau khi đăng nhập thành công. `true` để luôn chuyển hướng đến URL này.
    - `.failureUrl("/login?error=true")`: Trang chuyển hướng đến nếu đăng nhập thất bại.
  - `logout`: Cấu hình đăng xuất.
    - `.logoutRequestMatcher(new AntPathRequestMatcher("/logout"))`: URL để kích hoạt đăng xuất (thường là POST).
    - `.logoutSuccessUrl("/login?logout=true")`: Trang chuyển hướng sau khi đăng xuất.
  - **CSRF (Cross-Site Request Forgery) Protection:** Mặc định được bật. Nó yêu cầu một token CSRF phải được gửi kèm với các request thay đổi trạng thái (POST, PUT, DELETE). Khi làm việc với form Thymeleaf, Thymeleaf tự động thêm token này nếu bạn dùng `th:action` và form là POST.
    - Nếu bạn gặp lỗi 403 khi POST form, có thể do CSRF. `// .csrf(csrf -> csrf.disable())` để tạm tắt khi debug, nhưng **phải bật lại và xử lý đúng cách trong production.**

---

**4. Cập nhật `UserAccountService` và Entity `UserAccount`**

**4.1. Sửa Entity `UserAccount` để implement `UserDetails`**
Mở `UserAccount.java` và thay đổi:

```java
package com.mycompany.ecommerceproject.entity;

import jakarta.persistence.*;
import lombok.Getter;
import lombok.Setter;
import lombok.NoArgsConstructor;
import lombok.AllArgsConstructor;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails; // Import UserDetails

import java.time.LocalDateTime;
import java.util.Collection;
import java.util.Collections; // Dùng cho getAuthorities
import java.util.stream.Collectors;
import java.util.Arrays; // Dùng cho getAuthorities nếu role là chuỗi nhiều vai trò

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Entity
@Table(name = "user_accounts")
public class UserAccount implements UserDetails { // Implement UserDetails

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true, length = 50)
    private String username;

    @Column(nullable = false, length = 255)
    private String password;

    @Column(nullable = false, unique = true, length = 100)
    private String email;

    @Column(name = "first_name", length = 50)
    private String firstName;

    @Column(name = "last_name", length = 50)
    private String lastName;

    @Column(length = 100) // Tăng độ dài nếu lưu nhiều vai trò dạng "ROLE_USER,ROLE_EDITOR"
    private String role; // Ví dụ: "ROLE_USER", "ROLE_ADMIN", hoặc "ROLE_USER,ROLE_EDITOR"

    @Column(name = "created_at", updatable = false)
    private LocalDateTime createdAt;

    @Column(name = "updated_at")
    private LocalDateTime updatedAt;

    private boolean accountNonExpired = true;
    private boolean accountNonLocked = true;
    private boolean credentialsNonExpired = true;
    private boolean enabled = true;


    @PrePersist
    protected void onCreate() {
        createdAt = LocalDateTime.now();
        updatedAt = LocalDateTime.now();
        if (this.role == null || this.role.trim().isEmpty()) {
            this.role = "ROLE_USER"; // Gán vai trò mặc định
        }
    }

    @PreUpdate
    protected void onUpdate() {
        updatedAt = LocalDateTime.now();
    }

    // --- UserDetails methods ---
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        // Nếu 'role' chỉ lưu một vai trò, ví dụ: "ROLE_USER"
        // return Collections.singletonList(new SimpleGrantedAuthority(this.role));

        // Nếu 'role' có thể lưu nhiều vai trò, cách nhau bằng dấu phẩy, ví dụ: "ROLE_USER,ROLE_EDITOR"
        if (this.role == null || this.role.trim().isEmpty()) {
            return Collections.emptyList();
        }
        return Arrays.stream(this.role.split(","))
                .map(String::trim)
                .filter(roleName -> !roleName.isEmpty())
                .map(SimpleGrantedAuthority::new)
                .collect(Collectors.toList());
    }

    @Override
    public String getPassword() {
        return this.password;
    }

    @Override
    public String getUsername() {
        return this.username;
    }

    @Override
    public boolean isAccountNonExpired() {
        return this.accountNonExpired;
    }

    @Override
    public boolean isAccountNonLocked() {
        return this.accountNonLocked;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return this.credentialsNonExpired;
    }

    @Override
    public boolean isEnabled() {
        return this.enabled;
    }

    // Constructor tiện lợi (tùy chọn)
    public UserAccount(String username, String password, String email, String role) {
        this.username = username;
        this.password = password;
        this.email = email;
        this.role = role;
        this.accountNonExpired = true;
        this.accountNonLocked = true;
        this.credentialsNonExpired = true;
        this.enabled = true;
    }
}
```

- `implements UserDetails`: Class của chúng ta giờ đây là một `UserDetails`.
- Thêm các trường `accountNonExpired`, `accountNonLocked`, `credentialsNonExpired`, `enabled` với giá trị mặc định là `true`.
- Implement các phương thức của `UserDetails`:
  - `getAuthorities()`: Trả về một collection các `GrantedAuthority`. Chúng ta tạo `SimpleGrantedAuthority` từ trường `role`. Nếu bạn lưu nhiều vai trò cách nhau bằng dấu phẩy (ví dụ: "ROLE_USER,ROLE_STAFF"), bạn cần parse chuỗi đó.
  - `getPassword()`: Trả về password đã hash.
  - `getUsername()`: Trả về username.
  - Các phương thức `is...()` còn lại trả về trạng thái của tài khoản.

**4.2. Sửa `UserAccountServiceImpl` để inject `PasswordEncoder` và implement `UserDetailsService`**
Mở `UserAccountServiceImpl.java`:

```java
package com.mycompany.ecommerceproject.service.impl;

import com.mycompany.ecommerceproject.entity.UserAccount;
import com.mycompany.ecommerceproject.repository.UserAccountRepository;
import com.mycompany.ecommerceproject.service.UserAccountService;
import com.mycompany.ecommerceproject.exception.DuplicateResourceException;
import com.mycompany.ecommerceproject.exception.ResourceNotFoundException;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.security.core.userdetails.UserDetails; // Import UserDetails
import org.springframework.security.core.userdetails.UserDetailsService; // Import UserDetailsService
import org.springframework.security.core.userdetails.UsernameNotFoundException; // Import UsernameNotFoundException
import org.springframework.security.crypto.password.PasswordEncoder; // Import PasswordEncoder
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;
import java.util.Optional;

@Service
public class UserAccountServiceImpl implements UserAccountService, UserDetailsService { // Implement UserDetailsService

    private static final Logger logger = LoggerFactory.getLogger(UserAccountServiceImpl.class);
    private final UserAccountRepository userAccountRepository;
    private final PasswordEncoder passwordEncoder; // Inject PasswordEncoder

    // Constructor Injection
    public UserAccountServiceImpl(UserAccountRepository userAccountRepository, PasswordEncoder passwordEncoder) {
        this.userAccountRepository = userAccountRepository;
        this.passwordEncoder = passwordEncoder;
    }

    @Override
    @Transactional
    public UserAccount registerUser(UserAccount userAccount) {
        logger.info("Registering user: {}", userAccount.getUsername());
        if (userAccountRepository.existsByUsername(userAccount.getUsername())) {
            throw new DuplicateResourceException("Username " + userAccount.getUsername() + " already exists.");
        }
        if (userAccountRepository.existsByEmail(userAccount.getEmail())) {
            throw new DuplicateResourceException("Email " + userAccount.getEmail() + " already exists.");
        }

        // Hash password trước khi lưu
        userAccount.setPassword(passwordEncoder.encode(userAccount.getPassword()));

        if (userAccount.getRole() == null || userAccount.getRole().trim().isEmpty()) {
            userAccount.setRole("ROLE_USER");
        }
        // Đảm bảo các trường boolean của UserDetails được set (nếu không có giá trị mặc định trong entity)
        userAccount.setEnabled(true);
        userAccount.setAccountNonExpired(true);
        userAccount.setAccountNonLocked(true);
        userAccount.setCredentialsNonExpired(true);

        return userAccountRepository.save(userAccount);
    }

    @Override
    @Transactional(readOnly = true)
    public Optional<UserAccount> findByUsername(String username) {
        logger.debug("Fetching user by username: {}", username); // Đổi sang debug cho bớt log
        return userAccountRepository.findByUsername(username);
    }

    @Override
    @Transactional(readOnly = true)
    public Optional<UserAccount> findByEmail(String email) {
        logger.debug("Fetching user by email: {}", email);
        return userAccountRepository.findByEmail(email);
    }

    @Override
    @Transactional(readOnly = true)
    public List<UserAccount> findAll() {
        logger.info("Fetching all users");
        return userAccountRepository.findAll();
    }

    @Override
    @Transactional(readOnly = true)
    public Optional<UserAccount> findById(Long id) {
        logger.debug("Fetching user by id: {}", id);
        return userAccountRepository.findById(id);
    }

    // --- UserDetailsService method ---
    @Override
    @Transactional(readOnly = true)
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        logger.debug("Loading user by username for Spring Security: {}", username);
        return userAccountRepository.findByUsername(username)
                .orElseThrow(() ->
                        new UsernameNotFoundException("User not found with username: " + username));
    }
}
```

- `implements UserDetailsService`: Service của chúng ta giờ đây cũng là một `UserDetailsService`.
- Inject `PasswordEncoder` qua constructor.
- Trong `registerUser()`:
  - Sử dụng `passwordEncoder.encode()` để hash mật khẩu trước khi lưu.
- Implement `loadUserByUsername(String username)`:
  - Phương thức này được Spring Security gọi trong quá trình xác thực.
  - Nó tìm `UserAccount` trong CSDL bằng username.
  - Nếu không tìm thấy, ném `UsernameNotFoundException`.
  - Vì `UserAccount` đã implement `UserDetails`, chúng ta có thể trả về trực tiếp instance `UserAccount`.

**4.3. Cập nhật `DataInitializer` (Nếu có)**
Nếu bạn dùng `DataInitializer` để tạo user mẫu, hãy đảm bảo bạn hash password:

```java
// Trong DataInitializer.java
// ...
// private final PasswordEncoder passwordEncoder; // Inject PasswordEncoder

// public DataInitializer(..., PasswordEncoder passwordEncoder) {
//    ...
//    this.passwordEncoder = passwordEncoder;
// }

// ...
// adminUser.setPassword(passwordEncoder.encode("adminpass")); // Hash password
// regularUser.setPassword(passwordEncoder.encode("userpass"));
// ...
```

Bạn cần inject `PasswordEncoder` vào `DataInitializer` và cập nhật việc tạo user mẫu.
**Quan trọng:** Vì `DataInitializer` bây giờ phụ thuộc vào `PasswordEncoder` (từ `SecurityConfig`), và `SecurityConfig` lại phụ thuộc vào `UserAccountService` (mà bạn có thể inject vào `DataInitializer`), bạn có thể gặp **Circular Dependency** nếu `DataInitializer` inject `UserAccountService` và `UserAccountService` được inject vào `SecurityConfig`.

**Cách giải quyết Circular Dependency tiềm ẩn:**
Một cách là `DataInitializer` không inject `UserAccountService` mà inject trực tiếp `UserAccountRepository` và `PasswordEncoder` để tạo user. Hoặc, nếu `DataInitializer` chỉ tạo dữ liệu ban đầu và không cần logic phức tạp từ service, thì inject repository là hợp lý.
Nếu bạn để `DataInitializer` sử dụng `UserAccountService`, và `UserAccountService` được inject vào `SecurityConfig` (thông qua `userDetailsService()` bean), bạn cần đảm bảo thứ tự khởi tạo bean hoặc sử dụng `@Lazy` trên một trong các dependency.
Hiện tại, `SecurityConfig` của chúng ta đang lấy `UserAccountService` (mà `UserAccountService` đã implement `UserDetailsService`) một cách gián tiếp qua method `userDetailsService()`.
Method `userDetailsService()` trong `SecurityConfig` tạo một lambda `username -> userAccountService.loadUserByUsername(username)`. `userAccountService` này là bean đã được inject vào `SecurityConfig`.

**Hãy kiểm tra lại `DataInitializer`:**
Nếu `DataInitializer` của bạn inject `UserAccountService`:

```java
// DataInitializer.java
// ...
private final UserAccountService userAccountService; // Dùng service để tạo user
// ...
public DataInitializer(CategoryRepository categoryRepository,
                       ProductRepository productRepository,
                       UserAccountService userAccountService) { // Bỏ PasswordEncoder nếu service đã xử lý
    this.categoryRepository = categoryRepository;
    this.productRepository = productRepository;
    this.userAccountService = userAccountService;
}
// ... trong run()
// UserAccount adminUser = new UserAccount();
// adminUser.setUsername("admin");
// adminUser.setPassword("adminpass"); // Service sẽ hash
// adminUser.setEmail("admin@example.com");
// adminUser.setRole("ROLE_ADMIN");
// userAccountService.registerUser(adminUser);
```

Như vậy, `UserAccountService` sẽ chịu trách nhiệm hash password. Điều này ổn và không tạo circular dependency với `SecurityConfig` vì `SecurityConfig` chỉ sử dụng `UserAccountService` thông qua `UserDetailsService` interface.

---

**5. Tạo trang Đăng nhập (Login Page) tùy chỉnh bằng Thymeleaf**

Tạo file `login.html` trong `src/main/resources/templates/`:

```html
<!DOCTYPE html>
<html
  xmlns:th="http://www.thymeleaf.org"
  xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
  layout:decorate="~{layouts/default}"
>
  <head>
    <title>Login</title>
    <style>
      .login-container {
        max-width: 400px;
        margin: 50px auto;
        padding: 20px;
        border: 1px solid #ddd;
        border-radius: 5px;
        background-color: #f9f9f9;
      }
    </style>
  </head>
  <body>
    <section layout:fragment="content">
      <div class="login-container">
        <h2 class="text-center mb-4">Login</h2>

        <div th:if="${param.error}" class="alert alert-danger" role="alert">
          Invalid username or password.
        </div>
        <div th:if="${param.logout}" class="alert alert-success" role="alert">
          You have been logged out.
        </div>

        <form th:action="@{/perform_login}" method="post">
          <!-- CSRF Token (Thymeleaf tự động thêm nếu Spring Security được enable và form method là POST) -->
          <!-- <input type="hidden" th:name="${_csrf.parameterName}" th:value="${_csrf.token}" /> -->

          <div class="mb-3">
            <label for="username" class="form-label">Username:</label>
            <input
              type="text"
              id="username"
              name="username"
              class="form-control"
              required
              autofocus
            />
          </div>
          <div class="mb-3">
            <label for="password" class="form-label">Password:</label>
            <input
              type="password"
              id="password"
              name="password"
              class="form-control"
              required
            />
          </div>
          <!-- (Tùy chọn) Remember Me
                <div class="mb-3 form-check">
                    <input type="checkbox" class="form-check-input" id="remember-me" name="remember-me">
                    <label class="form-check-label" for="remember-me">Remember me</label>
                </div>
                -->
          <div class="d-grid">
            <button type="submit" class="btn btn-primary">Login</button>
          </div>
        </form>
        <p class="mt-3 text-center">
          Don't have an account? <a th:href="@{/register}">Register here</a>
        </p>
      </div>
    </section>
  </body>
</html>
```

- `th:if="${param.error}"`: Hiển thị thông báo lỗi nếu URL có query parameter `error=true` (do `failureUrl` trong `SecurityConfig`).
- `th:if="${param.logout}"`: Hiển thị thông báo logout nếu URL có `logout=true`.
- `th:action="@{/perform_login}" method="post"`: Form sẽ POST đến `/perform_login`.
- `name="username"` và `name="password"`: Đây là tên input field mặc định mà Spring Security tìm kiếm.
- **CSRF Token:** Thymeleaf (với `thymeleaf-extras-springsecurity`) sẽ tự động thêm một input ẩn chứa CSRF token vào form nếu CSRF protection được bật (mặc định là bật) và form method là POST. Bạn không cần thêm thủ công `_csrf` input.

---

**6. Xử lý đăng nhập và đăng xuất (Đã cấu hình trong `SecurityConfig`)**

Spring Security sẽ tự động xử lý:

- **Đăng nhập:** Khi form POST đến `/perform_login`, `UsernamePasswordAuthenticationFilter` sẽ chặn request, lấy username/password, gọi `AuthenticationManager` (sử dụng `DaoAuthenticationProvider` của chúng ta), nếu thành công thì lưu `Authentication` vào `SecurityContextHolder` và chuyển hướng.
- **Đăng xuất:** Khi request đến `/logout`, `LogoutFilter` sẽ xử lý, xóa `Authentication` khỏi `SecurityContextHolder`, hủy session, và chuyển hướng.

---

**7. Cấu hình quyền truy cập (Đã làm trong `SecurityConfig`)**

Phần `authorizeHttpRequests` trong `SecurityConfig` đã định nghĩa:

- Các URL công khai (`permitAll`).
- Các URL còn lại yêu cầu xác thực (`authenticated`).
- Bạn có thể thêm các rule phức tạp hơn với `hasRole()`, `hasAuthority()`, `access()`.

---

**8. Tạo trang Đăng ký (Registration Page) và Controller xử lý đăng ký**

**8.1. Template `register.html`**
Tạo file `register.html` trong `src/main/resources/templates/`:

```html
<!DOCTYPE html>
<html
  xmlns:th="http://www.thymeleaf.org"
  xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
  layout:decorate="~{layouts/default}"
>
  <head>
    <title>Register</title>
    <style>
      .register-container {
        max-width: 500px;
        margin: 50px auto;
        padding: 20px;
        border: 1px solid #ddd;
        border-radius: 5px;
        background-color: #f9f9f9;
      }
    </style>
  </head>
  <body>
    <section layout:fragment="content">
      <div class="register-container">
        <h2 class="text-center mb-4">Create Account</h2>

        <!-- Hiển thị thông báo lỗi chung (nếu có) -->
        <div
          th:if="${errorMessage}"
          class="alert alert-danger"
          role="alert"
          th:text="${errorMessage}"
        ></div>
        <!-- Hiển thị thông báo thành công -->
        <div
          th:if="${successMessage}"
          class="alert alert-success"
          role="alert"
          th:text="${successMessage}"
        ></div>

        <!-- Nếu dùng DTO và BindingResult cho validation -->
        <!-- <form th:action="@{/register}" th:object="${userRegistrationDto}" method="post"> -->
        <form th:action="@{/register}" method="post">
          <!-- CSRF Token -->

          <div class="mb-3">
            <label for="username" class="form-label">Username:</label>
            <input
              type="text"
              id="username"
              name="username"
              class="form-control"
              required
            />
            <!-- <span th:if="${#fields.hasErrors('username')}" th:errors="*{username}" class="text-danger"></span> -->
          </div>

          <div class="mb-3">
            <label for="email" class="form-label">Email:</label>
            <input
              type="email"
              id="email"
              name="email"
              class="form-control"
              required
            />
            <!-- <span th:if="${#fields.hasErrors('email')}" th:errors="*{email}" class="text-danger"></span> -->
          </div>

          <div class="mb-3">
            <label for="password" class="form-label">Password:</label>
            <input
              type="password"
              id="password"
              name="password"
              class="form-control"
              required
            />
            <!-- <span th:if="${#fields.hasErrors('password')}" th:errors="*{password}" class="text-danger"></span> -->
          </div>

          <div class="mb-3">
            <label for="firstName" class="form-label">First Name:</label>
            <input
              type="text"
              id="firstName"
              name="firstName"
              class="form-control"
            />
          </div>

          <div class="mb-3">
            <label for="lastName" class="form-label">Last Name:</label>
            <input
              type="text"
              id="lastName"
              name="lastName"
              class="form-control"
            />
          </div>

          <div class="d-grid">
            <button type="submit" class="btn btn-primary">Register</button>
          </div>
        </form>
        <p class="mt-3 text-center">
          Already have an account? <a th:href="@{/login}">Login here</a>
        </p>
      </div>
    </section>
  </body>
</html>
```

- Form này sẽ POST đến `/register`.
- Chúng ta sẽ tạo DTO và validation cho form này ở Phần 9. Hiện tại, lấy trực tiếp request parameters.

**8.2. `AuthController` (hoặc `UserRegistrationController`)**
Tạo một controller mới, ví dụ `AuthController.java` trong package `controller`:

```java
package com.mycompany.ecommerceproject.controller;

import com.mycompany.ecommerceproject.entity.UserAccount;
import com.mycompany.ecommerceproject.service.UserAccountService;
import com.mycompany.ecommerceproject.exception.DuplicateResourceException;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam; // Sử dụng RequestParam đơn giản trước
import org.springframework.web.servlet.mvc.support.RedirectAttributes; // Dùng để truyền message sau redirect

@Controller
public class AuthController {

    private static final Logger logger = LoggerFactory.getLogger(AuthController.class);
    private final UserAccountService userAccountService;

    public AuthController(UserAccountService userAccountService) {
        this.userAccountService = userAccountService;
    }

    @GetMapping("/login")
    public String loginPage(@RequestParam(value = "error", required = false) String error,
                            @RequestParam(value = "logout", required = false) String logout,
                            Model model) {
        if (error != null) {
            model.addAttribute("errorMessage", "Invalid username or password.");
        }
        if (logout != null) {
            model.addAttribute("successMessage", "You have been logged out successfully.");
        }
        logger.info("Displaying login page");
        return "login"; // Trả về view login.html
    }

    @GetMapping("/register")
    public String registrationPage(Model model) {
        // model.addAttribute("userRegistrationDto", new UserRegistrationDto()); // Sẽ dùng DTO sau
        logger.info("Displaying registration page");
        return "register"; // Trả về view register.html
    }

    @PostMapping("/register")
    public String processRegistration(
            // Thay vì dùng @ModelAttribute với DTO (sẽ làm ở Phần 9), dùng @RequestParam trước
            @RequestParam String username,
            @RequestParam String email,
            @RequestParam String password,
            @RequestParam(required = false) String firstName,
            @RequestParam(required = false) String lastName,
            Model model,
            RedirectAttributes redirectAttributes) { // Để gửi flash message

        logger.info("Processing registration for username: {}", username);
        try {
            UserAccount newUser = new UserAccount();
            newUser.setUsername(username);
            newUser.setEmail(email);
            newUser.setPassword(password); // Service sẽ hash
            newUser.setFirstName(firstName);
            newUser.setLastName(lastName);
            // Vai trò sẽ được gán mặc định trong service hoặc entity

            userAccountService.registerUser(newUser);
            logger.info("User {} registered successfully", username);
            // redirectAttributes.addFlashAttribute("globalMessage", "Registration successful! Please login.");
            redirectAttributes.addFlashAttribute("successMessage", "Registration successful! Please login.");
            return "redirect:/login"; // Chuyển hướng đến trang login sau khi đăng ký thành công

        } catch (DuplicateResourceException e) {
            logger.warn("Registration failed for {}: {}", username, e.getMessage());
            model.addAttribute("errorMessage", e.getMessage());
            // model.addAttribute("userRegistrationDto", filledDto); // Giữ lại dữ liệu form nếu dùng DTO
            return "register"; // Quay lại trang register với thông báo lỗi
        } catch (Exception e) {
            logger.error("Unexpected error during registration for " + username, e);
            model.addAttribute("errorMessage", "An unexpected error occurred. Please try again.");
            return "register";
        }
    }
}
```

- `@GetMapping("/login")`: Hiển thị trang login.
- `@GetMapping("/register")`: Hiển thị trang đăng ký.
- `@PostMapping("/register")`: Xử lý việc submit form đăng ký.
  - Tạo đối tượng `UserAccount`.
  - Gọi `userAccountService.registerUser()`.
  - Nếu thành công, redirect đến trang login với một flash message (sử dụng `RedirectAttributes`). Flash attributes là các attribute được lưu trữ tạm thời (thường trong session) và chỉ tồn tại cho request redirect tiếp theo.
  - Nếu có lỗi (ví dụ `DuplicateResourceException`), quay lại trang register và hiển thị lỗi.

---

**9. Hiển thị thông tin người dùng đã đăng nhập trên giao diện**

Spring Security Extras for Thymeleaf cung cấp các utility để làm việc này.
Trong `fragments/header.html`, chúng ta đã có:

```html
<li
  class="nav-item"
  th:if="${#authentication == null or #strings.equals(#authentication.name, 'anonymousUser')}"
>
  <a class="nav-link" th:href="@{/login}">Login</a>
</li>
<li
  class="nav-item"
  th:if="${#authentication != null and not #strings.equals(#authentication.name, 'anonymousUser')}"
>
  <span class="nav-link">Hi, <span sec:authentication="name">User</span></span>
</li>
<li
  class="nav-item"
  th:if="${#authentication != null and not #strings.equals(#authentication.name, 'anonymousUser')}"
>
  <!-- Logout form POST -->
  <form th:action="@{/logout}" method="post" class="d-inline">
    <button type="submit" class="btn btn-link nav-link p-0 align-baseline">
      Logout
    </button>
  </form>
</li>
<!-- Thêm link đăng ký nếu chưa đăng nhập -->
<li
  class="nav-item"
  th:if="${#authentication == null or #strings.equals(#authentication.name, 'anonymousUser')}"
>
  <a class="nav-link" th:href="@{/register}">Register</a>
</li>
```

- `#authentication`: Tham chiếu đến đối tượng `Authentication` trong `SecurityContext`.
- `#strings.equals(#authentication.name, 'anonymousUser')`: Kiểm tra xem người dùng có phải là anonymous không.
- `sec:authentication="name"`: Lấy tên của principal (username).
- `sec:authorize="hasRole('ADMIN')"`: Một cách khác để kiểm soát hiển thị dựa trên vai trò (cần `xmlns:sec="http://www.thymeleaf.org/extras/spring-security"` trong thẻ `<html>` của layout).
  ```html
  <!-- Ví dụ trong header.html hoặc layout.html -->
  <!-- <li class="nav-item" sec:authorize="hasRole('ADMIN')">
      <a class="nav-link" th:href="@{/admin/dashboard}">Admin Dashboard</a>
  </li> -->
  ```
  Để `sec:*` hoạt động, hãy thêm namespace vào thẻ `<html>` trong `layouts/default.html`:
  `xmlns:sec="http://www.thymeleaf.org/extras/spring-security"`

**Kiểm tra:**

1.  Chạy ứng dụng.
2.  Truy cập `http://localhost:8080/`. Bạn sẽ được chuyển hướng đến `/login`.
3.  Thử đăng ký một tài khoản mới.
4.  Đăng nhập bằng tài khoản vừa tạo.
5.  Kiểm tra xem header có hiển thị đúng thông tin người dùng và link logout.
6.  Thử logout.
7.  Thử truy cập một trang yêu cầu xác thực (ví dụ, bạn có thể tạo một trang `/profile` đơn giản chỉ để test, và cấu hình nó yêu cầu `authenticated()` trong `SecurityConfig`).

---

**10. Best Practices và Lưu ý khi làm việc với Spring Security**

- **Luôn hash mật khẩu:** Sử dụng `PasswordEncoder` mạnh (BCrypt).
- **Bảo vệ CSRF:** Hiểu và bật CSRF protection. Thymeleaf tự động hỗ trợ cho form POST. Nếu dùng JavaScript để gọi API POST/PUT/DELETE, bạn cần gửi CSRF token trong header.
- **HTTPS trong Production:** Luôn sử dụng HTTPS để mã hóa dữ liệu truyền tải.
- **Nguyên tắc Quyền Tối thiểu (Principle of Least Privilege):** Chỉ cấp cho người dùng những quyền thực sự cần thiết.
- **Session Management:** Cấu hình session timeout, protection chống session fixation.
- **Remember Me:** Nếu sử dụng, hãy dùng giải pháp an toàn (ví dụ: persistent token approach).
- **Không log thông tin nhạy cảm:** Tránh log mật khẩu, token, v.v.
- **Cập nhật thường xuyên:** Giữ Spring Security và các dependency khác ở phiên bản mới nhất để vá lỗi bảo mật.
- **Tách biệt cấu hình:** Giữ `SecurityConfig` tập trung vào việc cấu hình bảo mật.
- **Sử dụng DTOs cho Form:** Khi form đăng ký/đăng nhập phức tạp hơn hoặc cần validation, hãy dùng DTOs (sẽ học ở Phần 9).
- **Bảo mật ở nhiều lớp (Defense in Depth):** Không chỉ dựa vào Spring Security ở web layer. Cân nhắc kiểm tra quyền ở service layer (`@PreAuthorize`) nếu cần.
- **Kiểm thử bảo mật:** Thực hiện penetration testing cho ứng dụng của bạn.

---

**Kết thúc Phần 6**

Đây là một phần rất lớn và quan trọng. Chúng ta đã:

- Tích hợp Spring Security vào dự án.
- Cấu hình Password Encoding, UserDetailsService.
- Tạo trang đăng nhập, đăng ký tùy chỉnh.
- Xử lý đăng nhập, đăng xuất, và ủy quyền cơ bản.

Ứng dụng của bạn bây giờ đã có một lớp bảo vệ cơ bản. Người dùng có thể đăng ký và đăng nhập.

**Trong Phần 7, chúng ta sẽ xây dựng tính năng Giỏ hàng (Shopping Cart).** Tính năng này sẽ yêu cầu người dùng phải đăng nhập.

Tuyệt vời! Bây giờ chúng ta đã có người dùng và cơ chế bảo mật, hãy cùng nhau xây dựng một tính năng cốt lõi của bất kỳ trang thương mại điện tử nào: Giỏ hàng (Shopping Cart).

---

**Phần 7: Xây dựng tính năng Giỏ hàng (Shopping Cart)**

Trong phần này, chúng ta sẽ:

1.  **Thiết kế Entity cho Giỏ hàng và Chi tiết Giỏ hàng.**
    - `Cart` (Giỏ hàng): Liên kết với `UserAccount`.
    - `CartItem` (Mục trong giỏ hàng): Liên kết với `Cart` và `Product`.
2.  **Tạo Repositories cho `Cart` và `CartItem`.**
3.  **Tạo Service Layer cho Giỏ hàng (`CartService`).**
    - Lấy giỏ hàng của người dùng hiện tại.
    - Thêm sản phẩm vào giỏ.
    - Cập nhật số lượng sản phẩm trong giỏ.
    - Xóa sản phẩm khỏi giỏ.
    - Tính tổng tiền của giỏ hàng.
    - Xóa toàn bộ giỏ hàng.
4.  **Xây dựng `CartController` để xử lý các request liên quan đến giỏ hàng.**
    - Hiển thị trang giỏ hàng.
    - API (hoặc form POST) để thêm sản phẩm vào giỏ (ví dụ: từ trang chi tiết sản phẩm hoặc danh sách sản phẩm).
    - API (hoặc form POST) để cập nhật/xóa sản phẩm trong giỏ.
5.  **Tạo các trang Thymeleaf cho Giỏ hàng.**
    - Hiển thị các mục trong giỏ, số lượng, giá, tổng tiền.
    - Form để cập nhật số lượng, nút xóa.
6.  **Tích hợp nút "Add to Cart" vào trang danh sách và chi tiết sản phẩm.**
7.  **Hiển thị số lượng item trong giỏ hàng trên Header (nếu có).**
8.  **Xử lý các trường hợp đặc biệt (ví dụ: sản phẩm hết hàng khi thêm vào giỏ).**
9.  **Lưu ý về quản lý Session và Giỏ hàng cho người dùng chưa đăng nhập (Guest Cart - tùy chọn nâng cao, phần này sẽ tập trung vào giỏ hàng cho người dùng đã đăng nhập).**

---

**1. Thiết kế Entity cho Giỏ hàng và Chi tiết Giỏ hàng**

Chúng ta cần hai entity chính:

- **`Cart`**: Đại diện cho giỏ hàng của một người dùng. Mỗi người dùng (đã đăng nhập) sẽ có một giỏ hàng.
- **`CartItem`**: Đại diện cho một loại sản phẩm cụ thể trong một giỏ hàng, bao gồm số lượng.

**1.1. Entity `Cart`**
Tạo file `Cart.java` trong package `com.mycompany.ecommerceproject.entity`:

```java
package com.mycompany.ecommerceproject.entity;

import jakarta.persistence.*;
import lombok.Getter;
import lombok.Setter;
import lombok.NoArgsConstructor;
import lombok.AllArgsConstructor;

import java.math.BigDecimal;
import java.time.LocalDateTime;
import java.util.HashSet;
import java.util.Set;

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Entity
@Table(name = "carts")
public class Cart {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    // Mối quan hệ One-to-One với UserAccount. Một UserAccount có một Cart.
    // `optional = false` nghĩa là UserAccount phải tồn tại.
    // `fetch = FetchType.LAZY` vì thường không cần tải User khi chỉ xem Cart.
    @OneToOne(fetch = FetchType.LAZY, optional = false)
    @JoinColumn(name = "user_id", nullable = false, unique = true) // Khóa ngoại trỏ tới user_accounts.id
    private UserAccount userAccount;

    // Mối quan hệ One-to-Many với CartItem
    // `cascade = CascadeType.ALL`: Các thao tác trên Cart sẽ lan truyền tới CartItems (lưu, cập nhật, xóa).
    // `orphanRemoval = true`: Nếu một CartItem bị xóa khỏi collection `cartItems` này, nó cũng sẽ bị xóa khỏi DB.
    @OneToMany(mappedBy = "cart", cascade = CascadeType.ALL, fetch = FetchType.EAGER, orphanRemoval = true)
    private Set<CartItem> cartItems = new HashSet<>(); // Initialize để tránh NullPointerException

    @Column(name = "created_at", updatable = false)
    private LocalDateTime createdAt;

    @Column(name = "updated_at")
    private LocalDateTime updatedAt;

    @PrePersist
    protected void onCreate() {
        createdAt = LocalDateTime.now();
        updatedAt = LocalDateTime.now();
    }

    @PreUpdate
    protected void onUpdate() {
        updatedAt = LocalDateTime.now();
    }

    // Phương thức tiện ích để tính tổng giá trị giỏ hàng
    public BigDecimal getTotalPrice() {
        BigDecimal totalPrice = BigDecimal.ZERO;
        if (cartItems != null) {
            for (CartItem item : cartItems) {
                totalPrice = totalPrice.add(item.getSubtotal());
            }
        }
        return totalPrice;
    }

    // Phương thức tiện ích để thêm item
    public void addCartItem(CartItem item) {
        if (item != null) {
            cartItems.add(item);
            item.setCart(this); // Duy trì tính nhất quán của mối quan hệ hai chiều
        }
    }

    // Phương thức tiện ích để xóa item
    public void removeCartItem(CartItem item) {
        if (item != null) {
            cartItems.remove(item);
            item.setCart(null); // Ngắt kết nối
        }
    }
}
```

- `@OneToOne` với `UserAccount`: Mỗi giỏ hàng thuộc về một người dùng duy nhất. `unique = true` trên `user_id` đảm bảo điều này.
- `@OneToMany` với `CartItem`: Một giỏ hàng có thể chứa nhiều mục hàng.
  - `fetch = FetchType.EAGER` cho `cartItems`: Khi tải `Cart`, chúng ta thường muốn tải luôn các `CartItem` của nó để hiển thị. Tuy nhiên, nếu có rất nhiều item, `LAZY` có thể tốt hơn và bạn sẽ query riêng các item khi cần. Với giỏ hàng, EAGER thường chấp nhận được.
- `cartItems = new HashSet<>()`: Khởi tạo collection để tránh `NullPointerException` khi gọi `getCartItems()` trên một `Cart` mới.
- `getTotalPrice()`: Một phương thức tiện ích để tính tổng giá trị.
- `addCartItem()`, `removeCartItem()`: Các phương thức helper để quản lý `CartItem` và duy trì tính nhất quán của mối quan hệ hai chiều (rất quan trọng!).

**1.2. Entity `CartItem`**
Tạo file `CartItem.java` trong package `com.mycompany.ecommerceproject.entity`:

```java
package com.mycompany.ecommerceproject.entity;

import jakarta.persistence.*;
import lombok.Getter;
import lombok.Setter;
import lombok.NoArgsConstructor;
import lombok.AllArgsConstructor;

import java.math.BigDecimal;

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Entity
@Table(name = "cart_items")
public class CartItem {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    // Mối quan hệ Many-to-One với Cart
    // Một CartItem thuộc về một Cart
    @ManyToOne(fetch = FetchType.LAZY, optional = false)
    @JoinColumn(name = "cart_id", nullable = false)
    private Cart cart;

    // Mối quan hệ Many-to-One với Product
    // Một CartItem tương ứng với một Product
    // FetchType.EAGER vì khi xem CartItem, ta gần như luôn cần thông tin Product.
    @ManyToOne(fetch = FetchType.EAGER, optional = false)
    @JoinColumn(name = "product_id", nullable = false)
    private Product product;

    @Column(nullable = false)
    private Integer quantity;

    // Giá tại thời điểm thêm vào giỏ (có thể khác giá hiện tại của sản phẩm nếu giá sản phẩm thay đổi)
    // Hoặc chúng ta có thể luôn lấy giá từ Product. Tùy theo yêu cầu.
    // Hiện tại, chúng ta sẽ tính subtotal dựa trên giá hiện tại của Product.
    // Nếu muốn lưu giá cố định, thêm cột: @Column(name="price_at_purchase", nullable=false, precision=10, scale=2) private BigDecimal priceAtPurchase;


    // Phương thức tiện ích để tính tổng phụ (subtotal) cho mục này
    public BigDecimal getSubtotal() {
        if (product != null && product.getPrice() != null && quantity != null) {
            return product.getPrice().multiply(new BigDecimal(quantity));
        }
        return BigDecimal.ZERO;
    }

    // Cần equals và hashCode nếu bạn dùng Set<CartItem> và muốn kiểm tra sự tồn tại của item dựa trên product.
    // Thường dựa trên Product (nếu một product chỉ xuất hiện 1 lần trong cart với số lượng thay đổi)
    // hoặc dựa trên ID nếu mỗi lần thêm là một CartItem mới (ít phổ biến hơn cho giỏ hàng).
    // Chúng ta sẽ xử lý logic "nếu sản phẩm đã có trong giỏ thì tăng số lượng" ở service.
    // Lombok @EqualsAndHashCode(exclude = {"cart"}) có thể hữu ích nếu dựa trên product và quantity
    // Nhưng hãy cẩn thận với lazy loading.

    // equals và hashCode dựa trên sản phẩm để đảm bảo một sản phẩm chỉ có một mục trong giỏ
    // (sau đó sẽ điều chỉnh số lượng)
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        CartItem cartItem = (CartItem) o;
        // Quan trọng: chỉ so sánh dựa trên Product để xác định một sản phẩm đã có trong giỏ hay chưa
        // ID có thể null nếu là item mới chưa được persist
        return product != null ? product.getId().equals(cartItem.product.getId()) : cartItem.product == null;
    }

    @Override
    public int hashCode() {
        // Quan trọng: chỉ hash dựa trên Product
        return product != null ? product.getId().hashCode() : 0;
    }
}
```

- `@ManyToOne` với `Cart`: Mỗi mục hàng thuộc về một giỏ hàng.
- `@ManyToOne` với `Product`: Mỗi mục hàng liên quan đến một sản phẩm. `FetchType.EAGER` thường hợp lý ở đây.
- `quantity`: Số lượng của sản phẩm này trong giỏ.
- `getSubtotal()`: Tính tổng tiền cho mục hàng này.
- **`equals()` và `hashCode()`:** Rất quan trọng. Chúng ta override `equals` và `hashCode` chỉ dựa trên `product.id`. Điều này có nghĩa là nếu bạn cố gắng thêm cùng một sản phẩm vào `Set<CartItem>` nhiều lần, nó sẽ được coi là cùng một đối tượng (nếu `id` của `Product` giống nhau). Logic trong service sẽ xử lý việc tăng số lượng thay vì tạo `CartItem` mới trùng lặp.

**Quan trọng:** Sau khi tạo entity, chạy lại ứng dụng để Hibernate tạo bảng trong CSDL. Bạn có thể kiểm tra trong H2 Console.
`spring.jpa.hibernate.ddl-auto=update` trong `application.properties` sẽ giúp tự động tạo bảng.

---

**2. Tạo Repositories cho `Cart` và `CartItem`**

Trong package `com.mycompany.ecommerceproject.repository`:

**2.1. `CartRepository.java`**

```java
package com.mycompany.ecommerceproject.repository;

import com.mycompany.ecommerceproject.entity.Cart;
import com.mycompany.ecommerceproject.entity.UserAccount;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import java.util.Optional;

@Repository
public interface CartRepository extends JpaRepository<Cart, Long> {
    Optional<Cart> findByUserAccount(UserAccount userAccount);
    Optional<Cart> findByUserAccountId(Long userId); // Tiện lợi hơn
}
```

**2.2. `CartItemRepository.java`**

```java
package com.mycompany.ecommerceproject.repository;

import com.mycompany.ecommerceproject.entity.Cart;
import com.mycompany.ecommerceproject.entity.CartItem;
import com.mycompany.ecommerceproject.entity.Product;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import java.util.Optional;
import java.util.Set;

@Repository
public interface CartItemRepository extends JpaRepository<CartItem, Long> {
    Optional<CartItem> findByCartAndProduct(Cart cart, Product product);
    Set<CartItem> findByCart(Cart cart); // Lấy tất cả item của một cart
    // void deleteByCartAndProduct(Cart cart, Product product); // Có thể cần
}
```

---

**3. Tạo Service Layer cho Giỏ hàng (`CartService`)**

Tạo interface `CartService.java` trong `com.mycompany.ecommerceproject.service`:

```java
package com.mycompany.ecommerceproject.service;

import com.mycompany.ecommerceproject.entity.Cart;
import com.mycompany.ecommerceproject.entity.Product;
import com.mycompany.ecommerceproject.entity.UserAccount;

public interface CartService {
    Cart getCartByUser(UserAccount user);
    Cart addProductToCart(UserAccount user, Long productId, int quantity);
    Cart updateProductQuantityInCart(UserAccount user, Long productId, int quantity);
    Cart removeProductFromCart(UserAccount user, Long productId);
    void clearCart(UserAccount user);
}
```

Tạo implementation `CartServiceImpl.java` trong `com.mycompany.ecommerceproject.service.impl`:

```java
package com.mycompany.ecommerceproject.service.impl;

import com.mycompany.ecommerceproject.entity.Cart;
import com.mycompany.ecommerceproject.entity.CartItem;
import com.mycompany.ecommerceproject.entity.Product;
import com.mycompany.ecommerceproject.entity.UserAccount;
import com.mycompany.ecommerceproject.repository.CartRepository;
import com.mycompany.ecommerceproject.repository.CartItemRepository; // Cần thiết nếu thao tác trực tiếp CartItem
import com.mycompany.ecommerceproject.repository.ProductRepository;
import com.mycompany.ecommerceproject.service.CartService;
import com.mycompany.ecommerceproject.exception.ResourceNotFoundException;
import com.mycompany.ecommerceproject.exception.BadRequestException;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.Optional;

@Service
public class CartServiceImpl implements CartService {

    private static final Logger logger = LoggerFactory.getLogger(CartServiceImpl.class);

    private final CartRepository cartRepository;
    private final ProductRepository productRepository;
    private final CartItemRepository cartItemRepository; // Inject nếu cần xóa CartItem trực tiếp

    public CartServiceImpl(CartRepository cartRepository,
                           ProductRepository productRepository,
                           CartItemRepository cartItemRepository) {
        this.cartRepository = cartRepository;
        this.productRepository = productRepository;
        this.cartItemRepository = cartItemRepository;
    }

    @Override
    @Transactional(readOnly = true)
    public Cart getCartByUser(UserAccount user) {
        if (user == null) {
            throw new IllegalArgumentException("User cannot be null to get cart");
        }
        logger.debug("Fetching cart for user: {}", user.getUsername());
        // Tìm giỏ hàng của user, nếu chưa có thì tạo mới và lưu
        return cartRepository.findByUserAccount(user)
                .orElseGet(() -> {
                    logger.info("No cart found for user {}, creating a new one.", user.getUsername());
                    Cart newCart = new Cart();
                    newCart.setUserAccount(user);
                    return cartRepository.save(newCart);
                });
    }

    @Override
    @Transactional
    public Cart addProductToCart(UserAccount user, Long productId, int quantity) {
        if (quantity <= 0) {
            throw new BadRequestException("Quantity must be positive.");
        }
        Product product = productRepository.findById(productId)
                .orElseThrow(() -> new ResourceNotFoundException("Product not found with id: " + productId));

        if (product.getStockQuantity() < quantity) {
            throw new BadRequestException("Not enough stock for product: " + product.getName() +
                                          ". Available: " + product.getStockQuantity() +
                                          ", Requested: " + quantity);
        }

        Cart cart = getCartByUser(user); // Lấy hoặc tạo giỏ hàng

        // Kiểm tra xem sản phẩm đã có trong giỏ chưa
        Optional<CartItem> existingCartItemOpt = cart.getCartItems().stream()
                .filter(item -> item.getProduct().getId().equals(productId))
                .findFirst();

        if (existingCartItemOpt.isPresent()) {
            // Nếu có, cập nhật số lượng
            CartItem cartItem = existingCartItemOpt.get();
            int newQuantity = cartItem.getQuantity() + quantity;
             if (product.getStockQuantity() < newQuantity) {
                throw new BadRequestException("Not enough stock for product: " + product.getName() +
                                              ". Available: " + product.getStockQuantity() +
                                              ", Requested total: " + newQuantity);
            }
            cartItem.setQuantity(newQuantity);
            logger.info("Updated quantity for product {} in cart of user {}", product.getName(), user.getUsername());
        } else {
            // Nếu chưa, tạo CartItem mới
            CartItem newCartItem = new CartItem();
            newCartItem.setProduct(product);
            newCartItem.setQuantity(quantity);
            // newCartItem.setCart(cart); // Cart.addCartItem sẽ làm điều này
            cart.addCartItem(newCartItem); // Thêm vào set và set cart cho item
            logger.info("Added product {} to cart of user {}", product.getName(), user.getUsername());
        }
        return cartRepository.save(cart); // Lưu lại cart (cascade sẽ lưu cart items)
    }

    @Override
    @Transactional
    public Cart updateProductQuantityInCart(UserAccount user, Long productId, int quantity) {
        if (quantity <= 0) {
            // Nếu số lượng là 0 hoặc âm, coi như xóa sản phẩm
            return removeProductFromCart(user, productId);
            // throw new BadRequestException("Quantity must be positive.");
        }
        Product product = productRepository.findById(productId)
                .orElseThrow(() -> new ResourceNotFoundException("Product not found with id: " + productId));

        if (product.getStockQuantity() < quantity) {
             throw new BadRequestException("Not enough stock for product: " + product.getName() +
                                          ". Available: " + product.getStockQuantity() +
                                          ", Requested: " + quantity);
        }

        Cart cart = getCartByUser(user);
        CartItem cartItem = cart.getCartItems().stream()
                .filter(item -> item.getProduct().getId().equals(productId))
                .findFirst()
                .orElseThrow(() -> new ResourceNotFoundException("Product not found in cart. Product ID: " + productId));

        cartItem.setQuantity(quantity);
        logger.info("Updated quantity to {} for product {} in cart of user {}", quantity, product.getName(), user.getUsername());
        return cartRepository.save(cart);
    }

    @Override
    @Transactional
    public Cart removeProductFromCart(UserAccount user, Long productId) {
        Cart cart = getCartByUser(user);
        Product product = productRepository.findById(productId)
                .orElseThrow(() -> new ResourceNotFoundException("Product not found with id: " + productId));

        CartItem itemToRemove = cart.getCartItems().stream()
                .filter(item -> item.getProduct().getId().equals(productId))
                .findFirst().orElse(null);

        if (itemToRemove != null) {
            cart.removeCartItem(itemToRemove); // Xóa khỏi collection của Cart
            // cartItemRepository.delete(itemToRemove); // Nếu orphanRemoval=true trên Cart.cartItems thì không cần dòng này
                                                    // Hibernate sẽ tự xóa CartItem khi nó bị remove khỏi collection của Cart
            logger.info("Removed product {} from cart of user {}", product.getName(), user.getUsername());
            return cartRepository.save(cart);
        } else {
            logger.warn("Product {} not found in cart of user {} for removal.", product.getName(), user.getUsername());
            // Không cần ném lỗi nếu sản phẩm không có trong giỏ, chỉ trả về giỏ hàng hiện tại
            return cart;
        }
    }

    @Override
    @Transactional
    public void clearCart(UserAccount user) {
        Cart cart = getCartByUser(user);
        // Cách 1: Xóa từng item (nếu cần logic phức tạp cho từng item)
        // cart.getCartItems().clear(); // Điều này sẽ kích hoạt orphanRemoval

        // Cách 2: Lấy tất cả cart items và xóa chúng (nếu không dùng orphanRemoval)
        // Set<CartItem> itemsToRemove = new HashSet<>(cart.getCartItems());
        // for (CartItem item : itemsToRemove) {
        //     cart.removeCartItem(item); // Xóa khỏi collection
        //     cartItemRepository.delete(item); // Xóa khỏi DB
        // }

        // Cách 3: Xóa trực tiếp CartItems bằng repository (hiệu quả hơn nếu không cascade)
        // Set<CartItem> items = cartItemRepository.findByCart(cart);
        // cartItemRepository.deleteAll(items);
        // cart.getCartItems().clear(); // Clear collection trong bộ nhớ

        // Cách đơn giản nhất khi có CascadeType.ALL và orphanRemoval=true
        cart.getCartItems().clear();
        cartRepository.save(cart); // Lưu lại cart để cập nhật
        logger.info("Cleared cart for user {}", user.getUsername());
    }
}
```

- `getCartByUser()`: Lấy giỏ hàng của người dùng, nếu chưa có thì tạo mới.
- `addProductToCart()`:
  - Kiểm tra số lượng hợp lệ, sản phẩm tồn tại, và đủ hàng tồn kho.
  - Lấy giỏ hàng của người dùng.
  - Kiểm tra xem sản phẩm đã có trong `CartItem` của giỏ hàng chưa (dựa vào `product.id`).
    - Nếu có: Tăng số lượng.
    - Nếu chưa: Tạo `CartItem` mới và thêm vào `cart.getCartItems()`.
  - Sử dụng `cart.addCartItem(newCartItem)` để đảm bảo mối quan hệ hai chiều được thiết lập.
  - Lưu `Cart` (JPA sẽ cascade lưu các `CartItem` mới hoặc cập nhật).
- `updateProductQuantityInCart()`: Tương tự, nhưng cập nhật số lượng cho một `CartItem` đã có.
- `removeProductFromCart()`: Xóa `CartItem` khỏi `cart.getCartItems()`. Nếu `orphanRemoval=true` được đặt trên `@OneToMany` trong `Cart`, Hibernate sẽ tự động xóa `CartItem` này khỏi CSDL khi `Cart` được lưu.
- `clearCart()`: Xóa tất cả các `CartItem` khỏi giỏ hàng.

---

**4. Xây dựng `CartController`**

Tạo `CartController.java` trong package `com.mycompany.ecommerceproject.controller`:

```java
package com.mycompany.ecommerceproject.controller;

import com.mycompany.ecommerceproject.entity.Cart;
import com.mycompany.ecommerceproject.entity.UserAccount;
import com.mycompany.ecommerceproject.service.CartService;
import com.mycompany.ecommerceproject.service.UserAccountService;
import com.mycompany.ecommerceproject.exception.ResourceNotFoundException;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.security.core.annotation.AuthenticationPrincipal; // Để inject UserDetails (UserAccount)
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.servlet.mvc.support.RedirectAttributes;

@Controller
@RequestMapping("/cart")
public class CartController {

    private static final Logger logger = LoggerFactory.getLogger(CartController.class);

    private final CartService cartService;
    private final UserAccountService userAccountService; // Có thể không cần nếu @AuthenticationPrincipal hoạt động tốt

    public CartController(CartService cartService, UserAccountService userAccountService) {
        this.cartService = cartService;
        this.userAccountService = userAccountService;
    }

    // Lấy thông tin người dùng hiện tại
    private UserAccount getCurrentUser(Object principal) {
        if (principal instanceof UserAccount) {
            return (UserAccount) principal;
        } else if (principal instanceof org.springframework.security.core.userdetails.User) {
            // Trường hợp UserDetails mặc định của Spring Security (nếu không custom UserDetails)
            String username = ((org.springframework.security.core.userdetails.User) principal).getUsername();
            return userAccountService.findByUsername(username)
                    .orElseThrow(() -> new ResourceNotFoundException("User not found: " + username));
        }
        // Nếu không lấy được user từ principal, có thể ném lỗi hoặc xử lý khác
        // Trong trường hợp này, chúng ta đã cấu hình để principal là UserAccount
        throw new IllegalStateException("User not authenticated or principal type not supported.");
    }


    @GetMapping
    public String viewCart(Model model, @AuthenticationPrincipal UserAccount currentUser) {
        // @AuthenticationPrincipal sẽ inject UserAccount (vì UserAccount implement UserDetails)
        // Nếu currentUser là null, nghĩa là người dùng chưa đăng nhập, cần xử lý (ví dụ: redirect tới login)
        // SecurityConfig đã đảm bảo /cart yêu cầu authenticated()
        if (currentUser == null) {
            logger.warn("Attempt to access cart without authentication.");
            return "redirect:/login"; // Nên được xử lý bởi Spring Security rồi
        }
        logger.info("User {} viewing cart", currentUser.getUsername());
        Cart cart = cartService.getCartByUser(currentUser);
        model.addAttribute("cart", cart);
        return "cart/view"; // Tạo view cart/view.html
    }

    @PostMapping("/add")
    public String addToCart(@RequestParam("productId") Long productId,
                            @RequestParam(name = "quantity", defaultValue = "1") int quantity,
                            @AuthenticationPrincipal UserAccount currentUser,
                            RedirectAttributes redirectAttributes) {
        if (currentUser == null) {
            return "redirect:/login";
        }
        logger.info("User {} adding product {} to cart with quantity {}", currentUser.getUsername(), productId, quantity);
        try {
            cartService.addProductToCart(currentUser, productId, quantity);
            redirectAttributes.addFlashAttribute("successMessage", "Product added to cart successfully!");
        } catch (Exception e) {
            logger.error("Error adding product to cart for user {}: {}", currentUser.getUsername(), e.getMessage());
            redirectAttributes.addFlashAttribute("errorMessage", "Error adding product: " + e.getMessage());
        }
        // Chuyển hướng người dùng về trang trước đó hoặc trang sản phẩm
        // HttpServletRequest request; // inject request
        // String referer = request.getHeader("Referer");
        // return "redirect:" + (referer != null ? referer : "/products");
        return "redirect:/products/" + productId; // Hoặc redirect về trang chi tiết sản phẩm vừa thêm
        // Hoặc "redirect:/cart" để xem giỏ hàng
    }

    @PostMapping("/update")
    public String updateCartItem(@RequestParam("productId") Long productId,
                                 @RequestParam("quantity") int quantity,
                                 @AuthenticationPrincipal UserAccount currentUser,
                                 RedirectAttributes redirectAttributes) {
        if (currentUser == null) {
            return "redirect:/login";
        }
        logger.info("User {} updating product {} in cart with quantity {}", currentUser.getUsername(), productId, quantity);
        try {
            if (quantity > 0) {
                cartService.updateProductQuantityInCart(currentUser, productId, quantity);
                redirectAttributes.addFlashAttribute("successMessage", "Cart updated successfully!");
            } else {
                cartService.removeProductFromCart(currentUser, productId);
                 redirectAttributes.addFlashAttribute("successMessage", "Product removed from cart.");
            }
        } catch (Exception e) {
            logger.error("Error updating cart for user {}: {}", currentUser.getUsername(), e.getMessage());
            redirectAttributes.addFlashAttribute("errorMessage", "Error updating cart: " + e.getMessage());
        }
        return "redirect:/cart";
    }

    @PostMapping("/remove")
    public String removeFromCart(@RequestParam("productId") Long productId,
                                 @AuthenticationPrincipal UserAccount currentUser,
                                 RedirectAttributes redirectAttributes) {
        if (currentUser == null) {
            return "redirect:/login";
        }
        logger.info("User {} removing product {} from cart", currentUser.getUsername(), productId);
        try {
            cartService.removeProductFromCart(currentUser, productId);
            redirectAttributes.addFlashAttribute("successMessage", "Product removed from cart successfully!");
        } catch (Exception e) {
            logger.error("Error removing product from cart for user {}: {}", currentUser.getUsername(), e.getMessage());
            redirectAttributes.addFlashAttribute("errorMessage", "Error removing product: " + e.getMessage());
        }
        return "redirect:/cart";
    }

    @PostMapping("/clear")
    public String clearCart(@AuthenticationPrincipal UserAccount currentUser, RedirectAttributes redirectAttributes) {
        if (currentUser == null) {
            return "redirect:/login";
        }
        logger.info("User {} clearing cart", currentUser.getUsername());
        try {
            cartService.clearCart(currentUser);
            redirectAttributes.addFlashAttribute("successMessage", "Cart cleared successfully!");
        } catch (Exception e) {
            logger.error("Error clearing cart for user {}: {}", currentUser.getUsername(), e.getMessage());
            redirectAttributes.addFlashAttribute("errorMessage", "Error clearing cart: " + e.getMessage());
        }
        return "redirect:/cart";
    }
}
```

- `@AuthenticationPrincipal UserAccount currentUser`: Spring Security sẽ inject đối tượng `UserAccount` (vì nó implement `UserDetails`) của người dùng hiện tại đã đăng nhập.
- Các phương thức POST (`/add`, `/update`, `/remove`, `/clear`) sẽ nhận thông tin từ form và gọi `CartService`.
- `RedirectAttributes`: Dùng để gửi thông báo (flash messages) sau khi redirect.

---

**5. Tạo các trang Thymeleaf cho Giỏ hàng**

Tạo thư mục `cart` trong `src/main/resources/templates/`.
Tạo file `view.html` trong `src/main/resources/templates/cart/`:

```html
<!DOCTYPE html>
<html
  xmlns:th="http://www.thymeleaf.org"
  xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
  layout:decorate="~{layouts/default}"
>
  <head>
    <title>Shopping Cart</title>
  </head>
  <body>
    <section layout:fragment="content">
      <div class="container mt-4">
        <h1>Your Shopping Cart</h1>

        <div
          th:if="${successMessage}"
          class="alert alert-success"
          role="alert"
          th:text="${successMessage}"
        ></div>
        <div
          th:if="${errorMessage}"
          class="alert alert-danger"
          role="alert"
          th:text="${errorMessage}"
        ></div>

        <div th:if="${cart == null or cart.cartItems.isEmpty()}">
          <p class="lead">Your cart is currently empty.</p>
          <a th:href="@{/products}" class="btn btn-primary"
            >Continue Shopping</a
          >
        </div>

        <div th:if="${cart != null and not cart.cartItems.isEmpty()}">
          <table class="table table-hover">
            <thead>
              <tr>
                <th scope="col">Product</th>
                <th scope="col" class="text-center">Image</th>
                <th scope="col" class="text-end">Price</th>
                <th scope="col" class="text-center">Quantity</th>
                <th scope="col" class="text-end">Subtotal</th>
                <th scope="col" class="text-center">Actions</th>
              </tr>
            </thead>
            <tbody>
              <tr th:each="item : ${cart.cartItems}">
                <td th:text="${item.product.name}">Product Name</td>
                <td class="text-center">
                  <img
                    th:src="${item.product.imageUrl != null ? item.product.imageUrl : '/images/placeholder.png'}"
                    alt="Product Image"
                    style="max-width: 50px; max-height: 50px;"
                  />
                </td>
                <td
                  class="text-end"
                  th:text="${#numbers.formatCurrency(item.product.price)}"
                >
                  Price
                </td>
                <td class="text-center">
                  <form
                    th:action="@{/cart/update}"
                    method="post"
                    class="d-inline-flex align-items-center"
                  >
                    <input
                      type="hidden"
                      name="productId"
                      th:value="${item.product.id}"
                    />
                    <input
                      type="number"
                      name="quantity"
                      th:value="${item.quantity}"
                      min="1"
                      th:max="${item.product.stockQuantity}"
                      class="form-control form-control-sm"
                      style="width: 70px;"
                      onchange="this.form.submit()"
                    />
                    <!-- Tự động submit khi thay đổi số lượng -->
                    <!-- <button type="submit" class="btn btn-sm btn-outline-secondary ms-1">Update</button> -->
                  </form>
                </td>
                <td
                  class="text-end"
                  th:text="${#numbers.formatCurrency(item.getSubtotal())}"
                >
                  Subtotal
                </td>
                <td class="text-center">
                  <form
                    th:action="@{/cart/remove}"
                    method="post"
                    class="d-inline"
                  >
                    <input
                      type="hidden"
                      name="productId"
                      th:value="${item.product.id}"
                    />
                    <button type="submit" class="btn btn-sm btn-danger">
                      Remove
                    </button>
                  </form>
                </td>
              </tr>
            </tbody>
          </table>

          <div class="row mt-4">
            <div class="col-md-8">
              <a th:href="@{/products}" class="btn btn-outline-secondary"
                >Continue Shopping</a
              >
              <form
                th:action="@{/cart/clear}"
                method="post"
                class="d-inline ms-2"
              >
                <button type="submit" class="btn btn-warning">
                  Clear Cart
                </button>
              </form>
            </div>
            <div class="col-md-4 text-end">
              <h3>
                Total:
                <span th:text="${#numbers.formatCurrency(cart.getTotalPrice())}"
                  >$0.00</span
                >
              </h3>
              <a th:href="@{/checkout}" class="btn btn-success btn-lg mt-2"
                >Proceed to Checkout</a
              >
              <!-- Sẽ làm sau -->
            </div>
          </div>
        </div>
      </div>
    </section>

    <th:block layout:fragment="scripts">
      <script th:inline="javascript">
        // Có thể thêm JS để update cart bằng AJAX sau này
        // Ví dụ: khi số lượng thay đổi, gửi AJAX request thay vì submit form
      </script>
    </th:block>
  </body>
</html>
```

- Hiển thị các item trong giỏ, có form để cập nhật số lượng hoặc xóa.
- `onchange="this.form.submit()"`: Khi người dùng thay đổi số lượng trong input number, form sẽ tự động được submit để cập nhật giỏ hàng.
- Nút "Proceed to Checkout" sẽ được làm ở phần sau.

---

**6. Tích hợp nút "Add to Cart" vào trang danh sách và chi tiết sản phẩm**

**6.1. Trang chi tiết sản phẩm (`products/detail.html`)**
Sửa lại phần form "Add to Cart" trong `products/detail.html`:

```html
<!-- Trong products/detail.html -->
<!-- ... -->
<form
  th:action="@{/cart/add}"
  method="post"
  th:if="${product.stockQuantity > 0}"
>
  <input type="hidden" name="productId" th:value="${product.id}" />
  <div class="input-group mb-3" style="max-width: 200px;">
    <label for="quantity" class="input-group-text">Quantity</label>
    <input
      type="number"
      name="quantity"
      id="quantity"
      class="form-control"
      value="1"
      min="1"
      th:max="${product.stockQuantity}"
    />
    <button type="submit" class="btn btn-primary">Add to Cart</button>
  </div>
</form>
<div th:if="${product.stockQuantity <= 0}">
  <button type="button" class="btn btn-secondary btn-lg" disabled>
    Out of Stock
  </button>
</div>
<!-- ... -->
```

**6.2. Trang danh sách sản phẩm (`products/list.html`)**
Thêm nút Add to Cart đơn giản (thêm 1 sản phẩm):

```html
<!-- Trong products/list.html, bên trong vòng lặp th:each="product : ..." -->
<!-- ... -->
<div class="mt-2">
  <a
    th:href="@{/products/{id}(id=${product.id})}"
    class="btn btn-sm btn-outline-primary"
    >View Details</a
  >
  <form
    th:action="@{/cart/add}"
    method="post"
    class="d-inline ms-1"
    th:if="${product.stockQuantity > 0}"
  >
    <input type="hidden" name="productId" th:value="${product.id}" />
    <input type="hidden" name="quantity" value="1" />
    <!-- Mặc định thêm 1 sản phẩm -->
    <button type="submit" class="btn btn-sm btn-primary">Add to Cart</button>
  </form>
  <button
    type="button"
    class="btn btn-sm btn-secondary"
    disabled
    th:if="${product.stockQuantity <= 0}"
  >
    Out of Stock
  </button>
</div>
<!-- ... -->
```

---

**7. Hiển thị số lượng item trong giỏ hàng trên Header (nếu có)**

Để làm điều này, chúng ta cần truyền thông tin số lượng item trong giỏ hàng vào tất cả các trang. Có nhiều cách:

- **Trong mỗi controller method:** Thêm logic để lấy số lượng item và add vào Model. Cách này lặp lại code.
- **Sử dụng `@ControllerAdvice` và `@ModelAttribute`:** Tạo một method trong một class `@ControllerAdvice` được đánh dấu `@ModelAttribute` để tự động thêm thuộc tính này vào Model cho tất cả các request. Đây là cách tốt hơn.

**Tạo `GlobalControllerAdvice.java` (nếu chưa có):**

```java
package com.mycompany.ecommerceproject.controller; // hoặc trong package .advice

import com.mycompany.ecommerceproject.entity.Cart;
import com.mycompany.ecommerceproject.entity.UserAccount;
import com.mycompany.ecommerceproject.service.CartService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.annotation.AuthenticationPrincipal;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ModelAttribute;

@ControllerAdvice // Áp dụng cho tất cả các @Controller
public class GlobalControllerAdvice {

    private final CartService cartService;

    @Autowired // Constructor injection
    public GlobalControllerAdvice(CartService cartService) {
        this.cartService = cartService;
    }

    @ModelAttribute("cartItemCount") // Tên attribute sẽ có trong Model
    public int getCartItemCount(@AuthenticationPrincipal UserAccount currentUser) {
        if (currentUser != null) {
            Cart cart = cartService.getCartByUser(currentUser);
            if (cart != null && cart.getCartItems() != null) {
                // Đếm tổng số lượng sản phẩm, không phải số loại sản phẩm
                return cart.getCartItems().stream()
                           .mapToInt(item -> item.getQuantity() != null ? item.getQuantity() : 0)
                           .sum();
            }
        }
        return 0;
    }
}
```

**Sử dụng trong `fragments/header.html`:**

```html
<!-- Trong fragments/header.html -->
<!-- ... -->
<li class="nav-item">
  <a class="nav-link" th:href="@{/cart}">
    Cart
    <span
      th:if="${cartItemCount > 0}"
      class="badge bg-danger rounded-pill"
      th:text="${cartItemCount}"
      >0</span
    >
  </a>
</li>
<!-- ... -->
```

---

**8. Xử lý các trường hợp đặc biệt**

- **Sản phẩm hết hàng khi thêm vào giỏ:** `CartServiceImpl` đã có logic kiểm tra `product.getStockQuantity()`. Controller sẽ bắt exception và hiển thị thông báo cho người dùng qua `RedirectAttributes`.
- **Số lượng trong giỏ vượt quá tồn kho:** Cũng đã được xử lý.
- **Concurrency Issues (Vấn đề đồng thời):** Nếu nhiều người dùng cùng thêm một sản phẩm cuối cùng vào giỏ, có thể xảy ra race condition. Cần cơ chế locking hoặc optimistic locking trong `ProductService` khi cập nhật `stockQuantity` (phần nâng cao hơn, tạm thời bỏ qua).

---

**9. Lưu ý về quản lý Session và Giỏ hàng cho người dùng chưa đăng nhập (Guest Cart)**

Phần này chúng ta tập trung vào giỏ hàng cho người dùng đã đăng nhập (lưu trong DB).
Giỏ hàng cho khách (Guest Cart) thường được lưu trong HTTP Session. Khi khách đăng nhập, giỏ hàng trong session có thể được merge với giỏ hàng trong DB của họ. Đây là một tính năng phức tạp hơn:

- Cần một cách để lưu `Cart` object (hoặc một DTO đơn giản của nó) vào session.
- Khi người dùng thêm/sửa/xóa item, cập nhật `Cart` trong session.
- Khi người dùng đăng nhập, kiểm tra session có guest cart không. Nếu có, merge các item từ guest cart vào cart trong DB của user.
- `CartService` sẽ cần các method để xử lý cả cart trong session và cart trong DB, hoặc có một `SessionCartService` riêng.

Chúng ta sẽ không implement Guest Cart trong phạm vi phần này để giữ mọi thứ đơn giản, nhưng đây là một hướng phát triển quan trọng cho ứng dụng thực tế.

---

**Chạy thử ứng dụng:**

1.  Đăng nhập vào ứng dụng.
2.  Đi đến trang sản phẩm, thử thêm sản phẩm vào giỏ hàng.
3.  Kiểm tra trang giỏ hàng (`/cart`).
4.  Thử cập nhật số lượng, xóa sản phẩm.
5.  Kiểm tra số lượng item trên header.
6.  Thử thêm số lượng lớn hơn tồn kho.

**Kết thúc Phần 7**

Chúng ta đã xây dựng thành công tính năng giỏ hàng cho người dùng đã đăng nhập!

- Thiết kế và tạo Entities, Repositories, Services, Controllers cho Cart.
- Xây dựng giao diện Thymeleaf cho giỏ hàng.
- Tích hợp "Add to Cart" và hiển thị số lượng item.

Ứng dụng của chúng ta ngày càng hoàn thiện. Bước tiếp theo sẽ là cho phép người dùng đặt hàng từ giỏ hàng của họ.

**Trong Phần 8, chúng ta sẽ: "Xây dựng tính năng Đặt hàng (Order Processing)."** (Tạo Order từ Cart, cập nhật tồn kho, quản lý trạng thái Order).

Tuyệt vời! Sau khi người dùng đã chọn đầy đủ sản phẩm vào giỏ hàng, bước tiếp theo tự nhiên là tiến hành đặt hàng. Đây là một phần quan trọng, liên quan đến việc lưu trữ thông tin đơn hàng và cập nhật trạng thái của các sản phẩm.

---

**Phần 8: Xây dựng tính năng Đặt hàng (Order Processing)**

Trong phần này, chúng ta sẽ:

1.  **Thiết kế Entity cho Đơn hàng và Chi tiết Đơn hàng.**
    - `Order` (Đơn hàng): Liên kết với `UserAccount`, chứa thông tin giao hàng, tổng tiền, trạng thái.
    - `OrderItem` (Mục trong đơn hàng): Liên kết với `Order` và `Product`, chứa số lượng, giá tại thời điểm mua.
2.  **Tạo Repositories cho `Order` và `OrderItem`.**
3.  **Tạo Service Layer cho Đơn hàng (`OrderService`).**
    - Tạo đơn hàng từ giỏ hàng của người dùng.
    - Cập nhật tồn kho sản phẩm sau khi đặt hàng.
    - Lấy danh sách đơn hàng của người dùng.
    - Lấy chi tiết một đơn hàng.
    - (Tùy chọn) Cập nhật trạng thái đơn hàng (ví dụ: ADMIN cập nhật).
4.  **Xây dựng `OrderController` để xử lý các request liên quan đến đơn hàng.**
    - Hiển thị trang "Checkout" (xác nhận thông tin và đặt hàng).
    - Xử lý việc tạo đơn hàng.
    - Hiển thị trang xác nhận đơn hàng thành công.
    - Hiển thị danh sách đơn hàng của người dùng.
    - Hiển thị chi tiết một đơn hàng.
5.  **Tạo các trang Thymeleaf cho quy trình Đặt hàng.**
    - Trang Checkout.
    - Trang xác nhận đơn hàng.
    - Trang danh sách đơn hàng.
    - Trang chi tiết đơn hàng.
6.  **Tích hợp nút "Proceed to Checkout" từ trang Giỏ hàng.**
7.  **Một số lưu ý về Transaction Management và Idempotency.**

---

**1. Thiết kế Entity cho Đơn hàng và Chi tiết Đơn hàng**

**1.1. Entity `Order` (Đơn hàng)**
Tạo file `Order.java` trong package `com.mycompany.ecommerceproject.entity`:

```java
package com.mycompany.ecommerceproject.entity;

import jakarta.persistence.*;
import lombok.Getter;
import lombok.Setter;
import lombok.NoArgsConstructor;
import lombok.AllArgsConstructor;

import java.math.BigDecimal;
import java.time.LocalDateTime;
import java.util.HashSet;
import java.util.Set;

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Entity
@Table(name = "orders") // "order" là từ khóa trong SQL, nên dùng "orders"
public class Order {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    // Mối quan hệ Many-to-One với UserAccount
    // Một UserAccount có thể có nhiều Order
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "user_id", nullable = false)
    private UserAccount userAccount;

    @Column(name = "order_date", nullable = false, updatable = false)
    private LocalDateTime orderDate;

    @Enumerated(EnumType.STRING) // Lưu trữ enum dưới dạng String trong DB
    @Column(name = "order_status", nullable = false, length = 50)
    private OrderStatus status;

    @Column(name = "total_amount", nullable = false, precision = 12, scale = 2)
    private BigDecimal totalAmount;

    // Thông tin giao hàng
    @Column(name = "shipping_address_line1", nullable = false, length = 255)
    private String shippingAddressLine1;

    @Column(name = "shipping_address_line2", length = 255)
    private String shippingAddressLine2;

    @Column(name = "shipping_city", nullable = false, length = 100)
    private String shippingCity;

    @Column(name = "shipping_postal_code", nullable = false, length = 20)
    private String shippingPostalCode;

    @Column(name = "shipping_country", nullable = false, length = 100)
    private String shippingCountry;

    @Column(name = "customer_phone", length = 20)
    private String customerPhone; // Số điện thoại người nhận

    @Column(name = "customer_name", length = 100) // Tên người nhận
    private String customerName;


    // Mối quan hệ One-to-Many với OrderItem
    // `cascade = CascadeType.ALL`: Khi lưu Order, các OrderItem cũng được lưu.
    // `orphanRemoval = true` có thể không cần thiết ở đây nếu OrderItem không bao giờ bị xóa khỏi Order sau khi tạo.
    @OneToMany(mappedBy = "order", cascade = CascadeType.ALL, fetch = FetchType.EAGER, orphanRemoval = true)
    private Set<OrderItem> orderItems = new HashSet<>();

    @Column(name = "updated_at")
    private LocalDateTime updatedAt;

    @PrePersist
    protected void onCreate() {
        orderDate = LocalDateTime.now();
        updatedAt = LocalDateTime.now();
        if (status == null) {
            status = OrderStatus.PENDING; // Trạng thái mặc định khi tạo đơn
        }
    }

    @PreUpdate
    protected void onUpdate() {
        updatedAt = LocalDateTime.now();
    }

    // Phương thức tiện ích
    public void addOrderItem(OrderItem item) {
        if (item != null) {
            orderItems.add(item);
            item.setOrder(this);
        }
    }

    // Tính tổng tiền lại từ các orderItems (nếu cần, thường totalAmount được set khi tạo)
    public BigDecimal calculateTotalAmount() {
        BigDecimal calculatedTotal = BigDecimal.ZERO;
        if (orderItems != null) {
            for (OrderItem item : orderItems) {
                calculatedTotal = calculatedTotal.add(item.getSubtotal());
            }
        }
        this.totalAmount = calculatedTotal;
        return calculatedTotal;
    }
}
```

- **`OrderStatus` Enum:** Chúng ta cần tạo một Enum để quản lý các trạng thái của đơn hàng.
  Tạo file `OrderStatus.java` trong `com.mycompany.ecommerceproject.entity` (hoặc một package con `enums`):

  ```java
  package com.mycompany.ecommerceproject.entity; // hoặc .enums

  public enum OrderStatus {
      PENDING,        // Đang chờ xử lý
      PROCESSING,     // Đang xử lý
      SHIPPED,        // Đã giao hàng
      DELIVERED,      // Đã nhận hàng
      CANCELLED,      // Đã hủy
      RETURNED        // Đã trả hàng
  }
  ```

- Thông tin giao hàng được lưu trực tiếp trong `Order`. Trong một hệ thống phức tạp hơn, bạn có thể có entity `Address` riêng và `Order` sẽ liên kết đến một `Address`.
- `FetchType.EAGER` cho `orderItems`: Khi xem một đơn hàng, chúng ta thường muốn xem luôn các sản phẩm trong đó.

**1.2. Entity `OrderItem` (Chi tiết Đơn hàng)**
Tạo file `OrderItem.java` trong package `com.mycompany.ecommerceproject.entity`:

```java
package com.mycompany.ecommerceproject.entity;

import jakarta.persistence.*;
import lombok.Getter;
import lombok.Setter;
import lombok.NoArgsConstructor;
import lombok.AllArgsConstructor;

import java.math.BigDecimal;

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Entity
@Table(name = "order_items")
public class OrderItem {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    // Mối quan hệ Many-to-One với Order
    @ManyToOne(fetch = FetchType.LAZY, optional = false)
    @JoinColumn(name = "order_id", nullable = false)
    private Order order;

    // Mối quan hệ Many-to-One với Product
    // Lưu ý: Nên lưu một "snapshot" của Product tại thời điểm mua hàng,
    // hoặc ít nhất là tên sản phẩm và giá, vì thông tin Product có thể thay đổi.
    // Ở đây, để đơn giản, chúng ta liên kết trực tiếp.
    @ManyToOne(fetch = FetchType.EAGER, optional = false) // EAGER để dễ lấy thông tin sản phẩm khi hiển thị OrderItem
    @JoinColumn(name = "product_id", nullable = false)
    private Product product; // Liên kết đến Product ID

    @Column(name = "product_name", nullable = false, length = 200) // Lưu lại tên sản phẩm tại thời điểm mua
    private String productName;

    @Column(nullable = false)
    private Integer quantity;

    @Column(name = "price_at_purchase", nullable = false, precision = 10, scale = 2) // Giá một đơn vị tại thời điểm mua
    private BigDecimal priceAtPurchase;

    // Tổng phụ cho mục này
    public BigDecimal getSubtotal() {
        if (priceAtPurchase != null && quantity != null) {
            return priceAtPurchase.multiply(new BigDecimal(quantity));
        }
        return BigDecimal.ZERO;
    }
}
```

- **Quan trọng:** `OrderItem` nên lưu trữ `productName` và `priceAtPurchase`. Điều này đảm bảo rằng ngay cả khi thông tin sản phẩm gốc (tên, giá) trong bảng `products` thay đổi sau này, thông tin trong đơn hàng đã đặt vẫn không bị ảnh hưởng.
- `getSubtotal()`: Tính toán dựa trên `priceAtPurchase`.

**Chạy lại ứng dụng** để Hibernate tạo các bảng mới này.

---

**2. Tạo Repositories cho `Order` và `OrderItem`**

Trong package `com.mycompany.ecommerceproject.repository`:

**2.1. `OrderRepository.java`**

```java
package com.mycompany.ecommerceproject.repository;

import com.mycompany.ecommerceproject.entity.Order;
import com.mycompany.ecommerceproject.entity.UserAccount;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;


import java.util.List;

@Repository
public interface OrderRepository extends JpaRepository<Order, Long> {
    List<Order> findByUserAccountOrderByOrderDateDesc(UserAccount userAccount);
    Page<Order> findByUserAccountOrderByOrderDateDesc(UserAccount userAccount, Pageable pageable);
    // Có thể thêm các query tìm theo status, v.v.
}
```

**2.2. `OrderItemRepository.java`**

```java
package com.mycompany.ecommerceproject.repository;

import com.mycompany.ecommerceproject.entity.OrderItem;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface OrderItemRepository extends JpaRepository<OrderItem, Long> {
    // Thường không cần nhiều custom query ở đây vì OrderItem được quản lý thông qua Order.
}
```

---

**3. Tạo Service Layer cho Đơn hàng (`OrderService`)**

Tạo interface `OrderService.java` trong `com.mycompany.ecommerceproject.service`:

```java
package com.mycompany.ecommerceproject.service;

import com.mycompany.ecommerceproject.dto.CheckoutFormDto; // Sẽ tạo DTO này
import com.mycompany.ecommerceproject.entity.Order;
import com.mycompany.ecommerceproject.entity.UserAccount;
import com.mycompany.ecommerceproject.entity.OrderStatus;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;


import java.util.List;
import java.util.Optional;

public interface OrderService {
    Order createOrder(UserAccount user, CheckoutFormDto checkoutFormDto); // Sử dụng DTO cho thông tin checkout
    Optional<Order> getOrderByIdAndUser(Long orderId, UserAccount user);
    List<Order> getOrdersByUser(UserAccount user);
    Page<Order> getOrdersByUser(UserAccount user, Pageable pageable);
    Optional<Order> findById(Long orderId); // Cho admin
    Order updateOrderStatus(Long orderId, OrderStatus newStatus); // Cho admin
}
```

**DTO `CheckoutFormDto.java`:**
Tạo package `dto` trong `com.mycompany.ecommerceproject`.
Tạo `CheckoutFormDto.java`:

```java
package com.mycompany.ecommerceproject.dto;

import lombok.Data;
import jakarta.validation.constraints.NotEmpty; // Sẽ dùng ở Phần 9
import jakarta.validation.constraints.Size;   // Sẽ dùng ở Phần 9

@Data // Lombok: @Getter, @Setter, @ToString, @EqualsAndHashCode, @RequiredArgsConstructor
public class CheckoutFormDto {
    // @NotEmpty(message = "Customer name is required")
    private String customerName;

    // @NotEmpty(message = "Phone number is required")
    private String customerPhone;

    // @NotEmpty(message = "Shipping address line 1 is required")
    private String shippingAddressLine1;

    private String shippingAddressLine2;

    // @NotEmpty(message = "Shipping city is required")
    private String shippingCity;

    // @NotEmpty(message = "Shipping postal code is required")
    private String shippingPostalCode;

    // @NotEmpty(message = "Shipping country is required")
    private String shippingCountry;

    // Có thể thêm các trường khác như paymentMethod, notes, etc.
}
```

Chúng ta sẽ sử dụng validation với DTO này ở Phần 9.

Tạo implementation `OrderServiceImpl.java` trong `com.mycompany.ecommerceproject.service.impl`:

```java
package com.mycompany.ecommerceproject.service.impl;

import com.mycompany.ecommerceproject.dto.CheckoutFormDto;
import com.mycompany.ecommerceproject.entity.*;
import com.mycompany.ecommerceproject.repository.OrderRepository;
import com.mycompany.ecommerceproject.repository.ProductRepository; // Cần để cập nhật stock
import com.mycompany.ecommerceproject.service.CartService;
import com.mycompany.ecommerceproject.service.OrderService;
import com.mycompany.ecommerceproject.exception.ResourceNotFoundException;
import com.mycompany.ecommerceproject.exception.BadRequestException;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;


import java.math.BigDecimal;
import java.util.List;
import java.util.Optional;

@Service
public class OrderServiceImpl implements OrderService {

    private static final Logger logger = LoggerFactory.getLogger(OrderServiceImpl.class);

    private final OrderRepository orderRepository;
    private final CartService cartService;
    private final ProductRepository productRepository; // Để cập nhật stock

    public OrderServiceImpl(OrderRepository orderRepository,
                            CartService cartService,
                            ProductRepository productRepository) {
        this.orderRepository = orderRepository;
        this.cartService = cartService;
        this.productRepository = productRepository;
    }

    @Override
    @Transactional // Rất quan trọng: tạo order, cập nhật stock, xóa cart phải là một transaction
    public Order createOrder(UserAccount user, CheckoutFormDto checkoutFormDto) {
        Cart cart = cartService.getCartByUser(user);
        if (cart.getCartItems() == null || cart.getCartItems().isEmpty()) {
            throw new BadRequestException("Cannot create order from an empty cart.");
        }

        Order order = new Order();
        order.setUserAccount(user);
        order.setStatus(OrderStatus.PENDING); // Trạng thái ban đầu
        order.setTotalAmount(cart.getTotalPrice());

        // Sao chép thông tin giao hàng từ DTO
        order.setCustomerName(checkoutFormDto.getCustomerName());
        order.setCustomerPhone(checkoutFormDto.getCustomerPhone());
        order.setShippingAddressLine1(checkoutFormDto.getShippingAddressLine1());
        order.setShippingAddressLine2(checkoutFormDto.getShippingAddressLine2());
        order.setShippingCity(checkoutFormDto.getShippingCity());
        order.setShippingPostalCode(checkoutFormDto.getShippingPostalCode());
        order.setShippingCountry(checkoutFormDto.getShippingCountry());

        // Chuyển đổi CartItems thành OrderItems và cập nhật tồn kho
        for (CartItem cartItem : cart.getCartItems()) {
            Product product = cartItem.getProduct();
            int quantityOrdered = cartItem.getQuantity();

            // Kiểm tra lại tồn kho trước khi tạo OrderItem (defense in depth)
            if (product.getStockQuantity() < quantityOrdered) {
                throw new BadRequestException("Not enough stock for product: " + product.getName() +
                                              ". Available: " + product.getStockQuantity() +
                                              ", Requested: " + quantityOrdered);
            }

            OrderItem orderItem = new OrderItem();
            orderItem.setProduct(product);
            orderItem.setProductName(product.getName()); // Lưu tên SP tại thời điểm mua
            orderItem.setQuantity(quantityOrdered);
            orderItem.setPriceAtPurchase(product.getPrice()); // Lưu giá SP tại thời điểm mua
            // orderItem.setOrder(order); // Order.addOrderItem sẽ làm điều này
            order.addOrderItem(orderItem);

            // Cập nhật tồn kho sản phẩm
            product.setStockQuantity(product.getStockQuantity() - quantityOrdered);
            productRepository.save(product); // Lưu thay đổi tồn kho
        }

        Order savedOrder = orderRepository.save(order);
        logger.info("Order {} created successfully for user {}", savedOrder.getId(), user.getUsername());

        // Xóa giỏ hàng sau khi đã đặt hàng thành công
        cartService.clearCart(user);
        logger.info("Cart cleared for user {} after order creation", user.getUsername());

        return savedOrder;
    }

    @Override
    @Transactional(readOnly = true)
    public Optional<Order> getOrderByIdAndUser(Long orderId, UserAccount user) {
        logger.debug("Fetching order id {} for user {}", orderId, user.getUsername());
        return orderRepository.findById(orderId)
                .filter(order -> order.getUserAccount().getId().equals(user.getId()));
    }

    @Override
    @Transactional(readOnly = true)
    public List<Order> getOrdersByUser(UserAccount user) {
        logger.debug("Fetching all orders for user {}", user.getUsername());
        return orderRepository.findByUserAccountOrderByOrderDateDesc(user);
    }

    @Override
    @Transactional(readOnly = true)
    public Page<Order> getOrdersByUser(UserAccount user, Pageable pageable) {
        logger.debug("Fetching orders for user {} with pageable {}", user.getUsername(), pageable);
        return orderRepository.findByUserAccountOrderByOrderDateDesc(user, pageable);
    }

    @Override
    @Transactional(readOnly = true)
    public Optional<Order> findById(Long orderId) {
        logger.debug("Fetching order by id {} (admin access)", orderId);
        return orderRepository.findById(orderId);
    }

    @Override
    @Transactional
    public Order updateOrderStatus(Long orderId, OrderStatus newStatus) {
        // Logic này thường dành cho Admin
        Order order = orderRepository.findById(orderId)
                .orElseThrow(() -> new ResourceNotFoundException("Order not found with id: " + orderId));

        // Có thể thêm logic kiểm tra việc chuyển đổi trạng thái có hợp lệ không
        // Ví dụ: không thể chuyển từ DELIVERED về PENDING

        order.setStatus(newStatus);
        logger.info("Order {} status updated to {} by admin", orderId, newStatus);
        return orderRepository.save(order);
    }
}
```

- **`@Transactional` trên `createOrder()`:** Rất quan trọng. Việc tạo `Order`, `OrderItem`, cập nhật `Product` stock, và xóa `Cart` phải xảy ra như một đơn vị công việc duy nhất. Nếu bất kỳ bước nào lỗi, tất cả sẽ được rollback.
- Trong `createOrder()`:
  - Lấy `Cart` của người dùng.
  - Kiểm tra giỏ hàng không rỗng.
  - Tạo đối tượng `Order` mới, set thông tin từ `CheckoutFormDto`.
  - Lặp qua các `CartItem` trong `Cart`:
    - Tạo `OrderItem` tương ứng.
    - **Lưu tên sản phẩm và giá tại thời điểm mua.**
    - **Kiểm tra lại tồn kho và cập nhật (giảm) `stockQuantity` của `Product`.** Đây là bước quan trọng.
    - Thêm `OrderItem` vào `Order`.
  - Lưu `Order` (JPA sẽ cascade lưu các `OrderItem`).
  - Xóa giỏ hàng (`cartService.clearCart(user)`).

---

**4. Xây dựng `OrderController`**

Tạo `OrderController.java` trong package `com.mycompany.ecommerceproject.controller`:

```java
package com.mycompany.ecommerceproject.controller;

import com.mycompany.ecommerceproject.dto.CheckoutFormDto;
import com.mycompany.ecommerceproject.entity.Cart;
import com.mycompany.ecommerceproject.entity.Order;
import com.mycompany.ecommerceproject.entity.UserAccount;
import com.mycompany.ecommerceproject.service.CartService;
import com.mycompany.ecommerceproject.service.OrderService;
import com.mycompany.ecommerceproject.exception.BadRequestException;
import com.mycompany.ecommerceproject.exception.ResourceNotFoundException;
import jakarta.validation.Valid; // Sẽ dùng ở Phần 9
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;
import org.springframework.data.domain.Sort;
import org.springframework.security.core.annotation.AuthenticationPrincipal;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult; // Sẽ dùng ở Phần 9
import org.springframework.web.bind.annotation.*;
import org.springframework.web.servlet.mvc.support.RedirectAttributes;

@Controller
@RequestMapping("/orders")
public class OrderController {

    private static final Logger logger = LoggerFactory.getLogger(OrderController.class);

    private final OrderService orderService;
    private final CartService cartService;

    public OrderController(OrderService orderService, CartService cartService) {
        this.orderService = orderService;
        this.cartService = cartService;
    }

    @GetMapping("/checkout")
    public String checkoutPage(Model model, @AuthenticationPrincipal UserAccount currentUser) {
        if (currentUser == null) return "redirect:/login";

        Cart cart = cartService.getCartByUser(currentUser);
        if (cart.getCartItems() == null || cart.getCartItems().isEmpty()) {
            logger.warn("User {} attempted to checkout with an empty cart.", currentUser.getUsername());
            // Thông báo cho người dùng rằng giỏ hàng trống
            model.addAttribute("errorMessage", "Your cart is empty. Please add items to your cart before checking out.");
            return "cart/view"; // Quay lại trang giỏ hàng
        }

        model.addAttribute("cart", cart);
        model.addAttribute("checkoutFormDto", new CheckoutFormDto()); // DTO cho form checkout
        logger.info("User {} proceeding to checkout page.", currentUser.getUsername());
        return "order/checkout"; // Tạo view order/checkout.html
    }

    @PostMapping("/place")
    public String placeOrder(@ModelAttribute("checkoutFormDto") /*@Valid*/ CheckoutFormDto checkoutFormDto, // Bỏ @Valid tạm thời
                             // BindingResult bindingResult, // Bỏ tạm thời
                             @AuthenticationPrincipal UserAccount currentUser,
                             Model model, // Dùng Model thay vì RedirectAttributes nếu muốn quay lại form với lỗi
                             RedirectAttributes redirectAttributes) {
        if (currentUser == null) return "redirect:/login";
        logger.info("User {} attempting to place order.", currentUser.getUsername());

        // if (bindingResult.hasErrors()) { // Sẽ dùng ở Phần 9
        //     logger.warn("Checkout form validation errors for user {}", currentUser.getUsername());
        //     Cart cart = cartService.getCartByUser(currentUser);
        //     model.addAttribute("cart", cart);
        //     return "order/checkout"; // Quay lại trang checkout với lỗi validation
        // }

        try {
            Order order = orderService.createOrder(currentUser, checkoutFormDto);
            logger.info("Order {} placed successfully for user {}.", order.getId(), currentUser.getUsername());
            redirectAttributes.addFlashAttribute("successMessage", "Your order has been placed successfully! Order ID: " + order.getId());
            return "redirect:/orders/confirmation/" + order.getId();
        } catch (BadRequestException e) {
            logger.warn("Bad request during order placement for user {}: {}", currentUser.getUsername(), e.getMessage());
            Cart cart = cartService.getCartByUser(currentUser); // Lấy lại cart để hiển thị
            model.addAttribute("cart", cart);
            model.addAttribute("errorMessage", e.getMessage());
            // model.addAttribute("checkoutFormDto", checkoutFormDto); // Giữ lại dữ liệu đã nhập
            return "order/checkout"; // Quay lại trang checkout với thông báo lỗi
        } catch (Exception e) {
            logger.error("Unexpected error during order placement for user " + currentUser.getUsername(), e);
            Cart cart = cartService.getCartByUser(currentUser);
            model.addAttribute("cart", cart);
            model.addAttribute("errorMessage", "An unexpected error occurred while placing your order. Please try again.");
            // model.addAttribute("checkoutFormDto", checkoutFormDto);
            return "order/checkout";
        }
    }

    @GetMapping("/confirmation/{orderId}")
    public String orderConfirmationPage(@PathVariable Long orderId, Model model, @AuthenticationPrincipal UserAccount currentUser) {
        if (currentUser == null) return "redirect:/login";
        logger.info("User {} viewing order confirmation for orderId {}", currentUser.getUsername(), orderId);

        Order order = orderService.getOrderByIdAndUser(orderId, currentUser)
                .orElseThrow(() -> new ResourceNotFoundException("Order not found or access denied. Order ID: " + orderId));
        model.addAttribute("order", order);
        return "order/confirmation"; // Tạo view order/confirmation.html
    }

    @GetMapping
    public String listUserOrders(Model model, @AuthenticationPrincipal UserAccount currentUser,
                                 @RequestParam(name = "page", defaultValue = "0") int page,
                                 @RequestParam(name = "size", defaultValue = "5") int size) {
        if (currentUser == null) return "redirect:/login";
        logger.info("User {} viewing their orders. Page: {}, Size: {}", currentUser.getUsername(), page, size);

        Pageable pageable = PageRequest.of(page, size, Sort.by("orderDate").descending());
        Page<Order> ordersPage = orderService.getOrdersByUser(currentUser, pageable);

        model.addAttribute("ordersPage", ordersPage);
        return "order/list"; // Tạo view order/list.html
    }

    @GetMapping("/{orderId}")
    public String viewOrderDetail(@PathVariable Long orderId, Model model, @AuthenticationPrincipal UserAccount currentUser) {
        if (currentUser == null) return "redirect:/login";
        logger.info("User {} viewing detail for orderId {}", currentUser.getUsername(), orderId);

        Order order = orderService.getOrderByIdAndUser(orderId, currentUser)
                .orElseThrow(() -> new ResourceNotFoundException("Order not found or access denied. Order ID: " + orderId));

        model.addAttribute("order", order);
        return "order/detail"; // Tạo view order/detail.html
    }
}
```

- `/checkout` (GET): Hiển thị form checkout, truyền `Cart` và một `CheckoutFormDto` rỗng. Kiểm tra giỏ hàng không rỗng.
- `/place` (POST): Xử lý đặt hàng.
  - Nhận `CheckoutFormDto` từ form.
  - Gọi `orderService.createOrder()`.
  - Nếu thành công, redirect đến trang xác nhận.
  - Nếu lỗi (`BadRequestException` do hết hàng, giỏ trống,...), quay lại trang checkout với thông báo lỗi.
- `/confirmation/{orderId}` (GET): Hiển thị thông tin đơn hàng vừa tạo.
- `/` (GET, trong `/orders`): Hiển thị danh sách đơn hàng của người dùng (có phân trang).
- `/{orderId}` (GET, trong `/orders`): Hiển thị chi tiết một đơn hàng của người dùng.

---

**5. Tạo các trang Thymeleaf cho quy trình Đặt hàng**

Tạo thư mục `order` trong `src/main/resources/templates/`.

**5.1. `order/checkout.html`**

```html
<!DOCTYPE html>
<html
  xmlns:th="http://www.thymeleaf.org"
  xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
  layout:decorate="~{layouts/default}"
>
  <head>
    <title>Checkout</title>
  </head>
  <body>
    <section layout:fragment="content">
      <div class="container mt-4">
        <h1>Checkout</h1>

        <div
          th:if="${errorMessage}"
          class="alert alert-danger"
          role="alert"
          th:text="${errorMessage}"
        ></div>

        <div class="row">
          <!-- Order Summary -->
          <div class="col-md-5 order-md-2 mb-4">
            <h4 class="d-flex justify-content-between align-items-center mb-3">
              <span class="text-muted">Your Cart</span>
              <span
                class="badge bg-secondary rounded-pill"
                th:text="${cart.cartItems.size()}"
                >0</span
              >
            </h4>
            <ul class="list-group mb-3">
              <li
                class="list-group-item d-flex justify-content-between lh-condensed"
                th:each="item : ${cart.cartItems}"
              >
                <div>
                  <h6 class="my-0" th:text="${item.product.name}">
                    Product name
                  </h6>
                  <small class="text-muted" th:text="'Qty: ' + ${item.quantity}"
                    >Quantity</small
                  >
                </div>
                <span
                  class="text-muted"
                  th:text="${#numbers.formatCurrency(item.getSubtotal())}"
                  >$0.00</span
                >
              </li>
              <li class="list-group-item d-flex justify-content-between">
                <span>Total (USD)</span>
                <strong
                  th:text="${#numbers.formatCurrency(cart.getTotalPrice())}"
                  >$0.00</strong
                >
              </li>
            </ul>
            <a th:href="@{/cart}" class="btn btn-sm btn-outline-secondary"
              >Edit Cart</a
            >
          </div>

          <!-- Shipping Information Form -->
          <div class="col-md-7 order-md-1">
            <h4 class="mb-3">Shipping Address</h4>
            <form
              th:action="@{/orders/place}"
              th:object="${checkoutFormDto}"
              method="post"
            >
              <!-- CSRF Token -->
              <div class="row">
                <div class="col-md-12 mb-3">
                  <label for="customerName" class="form-label">Full Name</label>
                  <input
                    type="text"
                    class="form-control"
                    id="customerName"
                    th:field="*{customerName}"
                    required
                  />
                  <!-- <div th:if="${#fields.hasErrors('customerName')}" th:errors="*{customerName}" class="invalid-feedback d-block"></div> -->
                </div>
              </div>

              <div class="mb-3">
                <label for="customerPhone" class="form-label"
                  >Phone Number</label
                >
                <input
                  type="tel"
                  class="form-control"
                  id="customerPhone"
                  th:field="*{customerPhone}"
                  required
                />
                <!-- <div th:if="${#fields.hasErrors('customerPhone')}" th:errors="*{customerPhone}" class="invalid-feedback d-block"></div> -->
              </div>

              <div class="mb-3">
                <label for="shippingAddressLine1" class="form-label"
                  >Address Line 1</label
                >
                <input
                  type="text"
                  class="form-control"
                  id="shippingAddressLine1"
                  th:field="*{shippingAddressLine1}"
                  required
                />
                <!-- <div th:if="${#fields.hasErrors('shippingAddressLine1')}" th:errors="*{shippingAddressLine1}" class="invalid-feedback d-block"></div> -->
              </div>

              <div class="mb-3">
                <label for="shippingAddressLine2" class="form-label"
                  >Address Line 2
                  <span class="text-muted">(Optional)</span></label
                >
                <input
                  type="text"
                  class="form-control"
                  id="shippingAddressLine2"
                  th:field="*{shippingAddressLine2}"
                />
              </div>

              <div class="row">
                <div class="col-md-5 mb-3">
                  <label for="shippingCity" class="form-label">City</label>
                  <input
                    type="text"
                    class="form-control"
                    id="shippingCity"
                    th:field="*{shippingCity}"
                    required
                  />
                  <!-- <div th:if="${#fields.hasErrors('shippingCity')}" th:errors="*{shippingCity}" class="invalid-feedback d-block"></div> -->
                </div>
                <div class="col-md-4 mb-3">
                  <label for="shippingPostalCode" class="form-label"
                    >Postal Code</label
                  >
                  <input
                    type="text"
                    class="form-control"
                    id="shippingPostalCode"
                    th:field="*{shippingPostalCode}"
                    required
                  />
                  <!-- <div th:if="${#fields.hasErrors('shippingPostalCode')}" th:errors="*{shippingPostalCode}" class="invalid-feedback d-block"></div> -->
                </div>
                <div class="col-md-3 mb-3">
                  <label for="shippingCountry" class="form-label"
                    >Country</label
                  >
                  <input
                    type="text"
                    class="form-control"
                    id="shippingCountry"
                    th:field="*{shippingCountry}"
                    value="Vietnam"
                    required
                  />
                  <!-- Mặc định -->
                  <!-- <div th:if="${#fields.hasErrors('shippingCountry')}" th:errors="*{shippingCountry}" class="invalid-feedback d-block"></div> -->
                </div>
              </div>
              <hr class="mb-4" />
              <!-- Payment Information (sẽ không implement chi tiết phần thanh toán ở đây) -->
              <h4 class="mb-3">Payment</h4>
              <div class="my-3">
                <p>
                  Payment processing is not implemented in this demo. This order
                  will be marked as PENDING.
                </p>
                <p>You can choose "Cash on Delivery" conceptually.</p>
              </div>
              <hr class="mb-4" />
              <button
                class="btn btn-primary btn-lg btn-block w-100"
                type="submit"
              >
                Place Order
              </button>
            </form>
          </div>
        </div>
      </div>
    </section>
  </body>
</html>
```

- Hiển thị tóm tắt giỏ hàng.
- Form nhập thông tin giao hàng, sử dụng `th:object="${checkoutFormDto}"` và `th:field="*{...}"`.
- Các comment `<!-- <div th:if...` là để chuẩn bị cho validation ở Phần 9.

**5.2. `order/confirmation.html`**

```html
<!DOCTYPE html>
<html
  xmlns:th="http://www.thymeleaf.org"
  xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
  layout:decorate="~{layouts/default}"
>
  <head>
    <title>Order Confirmation</title>
  </head>
  <body>
    <section layout:fragment="content">
      <div class="container mt-5 text-center">
        <div
          th:if="${successMessage}"
          class="alert alert-success"
          role="alert"
          th:text="${successMessage}"
        ></div>

        <div th:if="${order != null}">
          <i
            class="bi bi-check-circle-fill text-success"
            style="font-size: 4rem;"
          ></i>
          <!-- Cần Bootstrap Icons -->
          <h1 class="mt-3">Thank You For Your Order!</h1>
          <p class="lead">
            Your order <strong th:text="'#' + ${order.id}">#12345</strong> has
            been placed successfully.
          </p>
          <p>
            Order Date:
            <span
              th:text="${#temporals.format(order.orderDate, 'dd-MM-yyyy HH:mm')}"
            ></span>
          </p>
          <p>
            Status:
            <span
              class="badge"
              th:classappend="${order.status.name() == 'PENDING' ? 'bg-warning text-dark' :
                                                  order.status.name() == 'PROCESSING' ? 'bg-info text-dark' :
                                                  order.status.name() == 'SHIPPED' ? 'bg-primary' :
                                                  order.status.name() == 'DELIVERED' ? 'bg-success' :
                                                  order.status.name() == 'CANCELLED' ? 'bg-danger' : 'bg-secondary'}"
              th:text="${order.status.name()}"
              >PENDING</span
            >
          </p>
          <p>
            Total Amount:
            <strong th:text="${#numbers.formatCurrency(order.totalAmount)}"
              >$0.00</strong
            >
          </p>

          <h4 class="mt-4">Shipping To:</h4>
          <p th:text="${order.customerName}"></p>
          <p th:text="${order.shippingAddressLine1}"></p>
          <p
            th:if="${order.shippingAddressLine2}"
            th:text="${order.shippingAddressLine2}"
          ></p>
          <p
            th:text="${order.shippingCity + ', ' + order.shippingPostalCode}"
          ></p>
          <p th:text="${order.shippingCountry}"></p>
          <p
            th:if="${order.customerPhone}"
            th:text="'Phone: ' + ${order.customerPhone}"
          ></p>

          <h4 class="mt-4">Order Items:</h4>
          <ul class="list-group mb-3">
            <li
              class="list-group-item d-flex justify-content-between lh-condensed"
              th:each="item : ${order.orderItems}"
            >
              <div>
                <h6 class="my-0" th:text="${item.productName}">Product name</h6>
                <small
                  class="text-muted"
                  th:text="'Qty: ' + ${item.quantity} + ' @ ' + ${#numbers.formatCurrency(item.priceAtPurchase)}"
                  >Quantity @ Price</small
                >
              </div>
              <span
                class="text-muted"
                th:text="${#numbers.formatCurrency(item.getSubtotal())}"
                >$0.00</span
              >
            </li>
          </ul>

          <div class="mt-4">
            <a th:href="@{/products}" class="btn btn-primary"
              >Continue Shopping</a
            >
            <a th:href="@{/orders}" class="btn btn-outline-secondary ms-2"
              >View My Orders</a
            >
          </div>
        </div>
        <div th:if="${order == null and !successMessage}">
          <p class="lead">
            Order not found or you do not have permission to view it.
          </p>
          <a th:href="@{/}" class="btn btn-primary">Go to Homepage</a>
        </div>
      </div>
    </section>
  </body>
</html>
```

- Hiển thị thông tin chi tiết của đơn hàng vừa tạo.
- Cần thêm thư viện Bootstrap Icons nếu muốn dùng `bi-check-circle-fill`.

**5.3. `order/list.html` (Danh sách đơn hàng của người dùng)**

```html
<!DOCTYPE html>
<html
  xmlns:th="http://www.thymeleaf.org"
  xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
  layout:decorate="~{layouts/default}"
>
  <head>
    <title>My Orders</title>
  </head>
  <body>
    <section layout:fragment="content">
      <div class="container mt-4">
        <h1>My Orders</h1>

        <div th:if="${ordersPage == null or ordersPage.empty}">
          <p>You have no orders yet.</p>
          <a th:href="@{/products}" class="btn btn-primary">Start Shopping</a>
        </div>

        <div th:if="${ordersPage != null and not ordersPage.empty}">
          <table class="table table-hover">
            <thead>
              <tr>
                <th scope="col">Order ID</th>
                <th scope="col">Date</th>
                <th scope="col">Status</th>
                <th scope="col" class="text-end">Total Amount</th>
                <th scope="col" class="text-center">Actions</th>
              </tr>
            </thead>
            <tbody>
              <tr th:each="order : ${ordersPage.content}">
                <td>
                  <a
                    th:href="@{/orders/{id}(id=${order.id})}"
                    th:text="'#' + ${order.id}"
                    >#12345</a
                  >
                </td>
                <td
                  th:text="${#temporals.format(order.orderDate, 'dd-MM-yyyy HH:mm')}"
                >
                  Date
                </td>
                <td>
                  <span
                    class="badge"
                    th:classappend="${order.status.name() == 'PENDING' ? 'bg-warning text-dark' :
                                                       order.status.name() == 'PROCESSING' ? 'bg-info text-dark' :
                                                       order.status.name() == 'SHIPPED' ? 'bg-primary' :
                                                       order.status.name() == 'DELIVERED' ? 'bg-success' :
                                                       order.status.name() == 'CANCELLED' ? 'bg-danger' : 'bg-secondary'}"
                    th:text="${order.status.name()}"
                    >PENDING</span
                  >
                </td>
                <td
                  class="text-end"
                  th:text="${#numbers.formatCurrency(order.totalAmount)}"
                >
                  $0.00
                </td>
                <td class="text-center">
                  <a
                    th:href="@{/orders/{id}(id=${order.id})}"
                    class="btn btn-sm btn-info"
                    >View Details</a
                  >
                </td>
              </tr>
            </tbody>
          </table>

          <!-- Pagination for Orders -->
          <nav
            aria-label="Order Page navigation"
            th:if="${ordersPage.totalPages > 1}"
          >
            <ul class="pagination justify-content-center">
              <li
                class="page-item"
                th:classappend="${ordersPage.first ? 'disabled' : ''}"
              >
                <a
                  class="page-link"
                  th:href="@{/orders(page=${ordersPage.number - 1}, size=${ordersPage.size})}"
                  >Previous</a
                >
              </li>
              <li
                class="page-item"
                th:each="i : ${#numbers.sequence(0, ordersPage.totalPages - 1)}"
                th:classappend="${i == ordersPage.number ? 'active' : ''}"
              >
                <a
                  class="page-link"
                  th:href="@{/orders(page=${i}, size=${ordersPage.size})}"
                  th:text="${i + 1}"
                  >1</a
                >
              </li>
              <li
                class="page-item"
                th:classappend="${ordersPage.last ? 'disabled' : ''}"
              >
                <a
                  class="page-link"
                  th:href="@{/orders(page=${ordersPage.number + 1}, size=${ordersPage.size})}"
                  >Next</a
                >
              </li>
            </ul>
          </nav>
        </div>
      </div>
    </section>
  </body>
</html>
```

**5.4. `order/detail.html` (Chi tiết một đơn hàng)**
Tương tự như `order/confirmation.html` nhưng có thể có một vài khác biệt nhỏ nếu cần. Thường thì có thể tái sử dụng nhiều phần.

```html
<!DOCTYPE html>
<html
  xmlns:th="http://www.thymeleaf.org"
  xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
  layout:decorate="~{layouts/default}"
>
  <head>
    <title th:text="'Order Details - #' + ${order != null ? order.id : ''}">
      Order Details
    </title>
  </head>
  <body>
    <section layout:fragment="content">
      <div class="container mt-4">
        <div th:if="${order == null}">
          <div class="alert alert-danger" role="alert">
            Order not found or you do not have permission to view it.
          </div>
          <a th:href="@{/orders}" class="btn btn-primary">Back to My Orders</a>
        </div>

        <div th:if="${order != null}">
          <div class="d-flex justify-content-between align-items-center mb-3">
            <h1 th:text="'Order Details - #' + ${order.id}">Order Details</h1>
            <a th:href="@{/orders}" class="btn btn-outline-secondary"
              >Back to My Orders</a
            >
          </div>

          <div class="card">
            <div class="card-header">Order Information</div>
            <div class="card-body">
              <div class="row">
                <div class="col-md-6">
                  <p>
                    <strong>Order ID:</strong>
                    <span th:text="${order.id}"></span>
                  </p>
                  <p>
                    <strong>Order Date:</strong>
                    <span
                      th:text="${#temporals.format(order.orderDate, 'dd-MM-yyyy HH:mm')}"
                    ></span>
                  </p>
                  <p>
                    <strong>Status:</strong>
                    <span
                      class="badge"
                      th:classappend="${order.status.name() == 'PENDING' ? 'bg-warning text-dark' :
                                                           order.status.name() == 'PROCESSING' ? 'bg-info text-dark' :
                                                           order.status.name() == 'SHIPPED' ? 'bg-primary' :
                                                           order.status.name() == 'DELIVERED' ? 'bg-success' :
                                                           order.status.name() == 'CANCELLED' ? 'bg-danger' : 'bg-secondary'}"
                      th:text="${order.status.name()}"
                      >PENDING</span
                    >
                  </p>
                  <p>
                    <strong>Total Amount:</strong>
                    <strong
                      th:text="${#numbers.formatCurrency(order.totalAmount)}"
                    ></strong>
                  </p>
                </div>
                <div class="col-md-6">
                  <h4>Shipping Address:</h4>
                  <p th:text="${order.customerName}"></p>
                  <p th:text="${order.shippingAddressLine1}"></p>
                  <p
                    th:if="${order.shippingAddressLine2}"
                    th:text="${order.shippingAddressLine2}"
                  ></p>
                  <p
                    th:text="${order.shippingCity + ', ' + order.shippingPostalCode}"
                  ></p>
                  <p th:text="${order.shippingCountry}"></p>
                  <p
                    th:if="${order.customerPhone}"
                    th:text="'Phone: ' + ${order.customerPhone}"
                  ></p>
                </div>
              </div>
            </div>
          </div>

          <h4 class="mt-4">Order Items:</h4>
          <table class="table table-striped">
            <thead>
              <tr>
                <th>Product</th>
                <th class="text-center">Quantity</th>
                <th class="text-end">Price at Purchase</th>
                <th class="text-end">Subtotal</th>
              </tr>
            </thead>
            <tbody>
              <tr th:each="item : ${order.orderItems}">
                <td>
                  <a
                    th:href="@{/products/{id}(id=${item.product.id})}"
                    th:text="${item.productName}"
                    >Product Name</a
                  >
                </td>
                <td class="text-center" th:text="${item.quantity}">1</td>
                <td
                  class="text-end"
                  th:text="${#numbers.formatCurrency(item.priceAtPurchase)}"
                >
                  $0.00
                </td>
                <td
                  class="text-end"
                  th:text="${#numbers.formatCurrency(item.getSubtotal())}"
                >
                  $0.00
                </td>
              </tr>
            </tbody>
          </table>
          <!-- (Tùy chọn cho Admin) Các nút quản lý đơn hàng -->
          <!-- <div sec:authorize="hasRole('ADMIN')" class="mt-4">
                    <h4>Admin Actions:</h4>
                    <form th:action="@{/admin/orders/{orderId}/update-status(orderId=${order.id})}" method="post">
                        <div class="input-group mb-3" style="max-width: 300px;">
                            <label class="input-group-text" for="newStatus">New Status:</label>
                            <select class="form-select" id="newStatus" name="newStatus">
                                <option th:each="st : ${T(com.mycompany.ecommerceproject.entity.OrderStatus).values()}"
                                        th:value="${st}" th:text="${st.name()}" th:selected="${st == order.status}"></option>
                            </select>
                            <button type="submit" class="btn btn-warning">Update Status</button>
                        </div>
                    </form>
                </div> -->
        </div>
      </div>
    </section>
  </body>
</html>
```

---

**6. Tích hợp nút "Proceed to Checkout" từ trang Giỏ hàng**

Trong `cart/view.html`, chúng ta đã có:

```html
<a th:href="@{/orders/checkout}" class="btn btn-success btn-lg mt-2"
  >Proceed to Checkout</a
>
```

Nút này sẽ dẫn người dùng đến trang checkout.

---

**7. Một số lưu ý về Transaction Management và Idempotency**

- **Transaction Management:** Như đã đề cập, phương thức `createOrder()` trong `OrderServiceImpl` được đánh dấu `@Transactional`. Điều này cực kỳ quan trọng để đảm bảo tính nhất quán của dữ liệu (tạo order, cập nhật stock, xóa cart).
- **Idempotency (Tính bất biến lặp lại):**
  - Một thao tác được gọi là idempotent nếu việc gọi nó nhiều lần với cùng một tham số cho kết quả giống như gọi một lần (và không gây ra tác dụng phụ không mong muốn sau lần gọi đầu tiên).
  - Việc đặt hàng (POST đến `/orders/place`) **không** phải là idempotent theo mặc định. Nếu người dùng refresh trang xác nhận (mà URL là POST) hoặc nhấn nút "Place Order" nhiều lần, họ có thể tạo ra nhiều đơn hàng trùng lặp.
  - **Giải pháp:**
    - **Post/Redirect/Get (PRG) Pattern:** Sau khi xử lý POST thành công (đặt hàng), redirect người dùng đến một trang GET (ví dụ: trang xác nhận đơn hàng). Chúng ta đã làm điều này: `return "redirect:/orders/confirmation/" + order.getId();`.
    - **Idempotency Key:** Phía client tạo một khóa duy nhất (idempotency key) cho mỗi yêu cầu đặt hàng. Server lưu trữ key này và chỉ xử lý yêu cầu nếu key chưa được xử lý trước đó. Phức tạp hơn để implement.
    - **Client-side disabling:** Vô hiệu hóa nút "Place Order" sau khi click lần đầu bằng JavaScript. Không hoàn toàn đáng tin cậy nhưng giúp giảm thiểu.
  - Hiện tại, PRG pattern đã giúp chúng ta tránh việc refresh gây tạo đơn hàng mới.

---

**Chạy thử ứng dụng:**

1.  Đăng nhập.
2.  Thêm sản phẩm vào giỏ hàng.
3.  Đi đến giỏ hàng, nhấn "Proceed to Checkout".
4.  Điền thông tin giao hàng và nhấn "Place Order".
5.  Bạn sẽ được chuyển đến trang xác nhận đơn hàng.
6.  Kiểm tra trong H2 Console:
    - Dữ liệu trong bảng `orders` và `order_items`.
    - Số lượng `stock_quantity` trong bảng `products` đã giảm.
    - Giỏ hàng (bảng `carts` và `cart_items`) của người dùng đó đã được xóa.
7.  Vào "My Orders" (tạo link trong header nếu cần) để xem danh sách đơn hàng.
    - Thêm link "My Orders" vào `fragments/header.html`:
    ```html
    <li
      class="nav-item"
      th:if="${#authentication != null and not #strings.equals(#authentication.name, 'anonymousUser')}"
    >
      <a class="nav-link" th:href="@{/orders}">My Orders</a>
    </li>
    ```

**Kết thúc Phần 8**

Chúng ta đã hoàn thành một phần rất quan trọng: xử lý đặt hàng.

- Thiết kế Entities, Repositories, Services, Controllers cho Orders.
- Tạo quy trình checkout và các trang Thymeleaf liên quan.
- Xử lý việc cập nhật tồn kho và xóa giỏ hàng sau khi đặt hàng.

Ứng dụng của chúng ta giờ đây đã có một luồng thương mại điện tử cơ bản hoàn chỉnh từ xem sản phẩm, thêm vào giỏ, đến đặt hàng.

**Trong Phần 9, chúng ta sẽ tập trung vào việc làm cho ứng dụng "xịn" hơn: "Validation, Exception Handling và Data Transfer Objects (DTOs)."** Chúng ta sẽ cải thiện việc xử lý form, báo lỗi, và cấu trúc dữ liệu truyền tải.

Tuyệt vời! Sau khi đã xây dựng được các tính năng cốt lõi, giờ là lúc chúng ta tinh chỉnh ứng dụng, làm cho nó trở nên mạnh mẽ, dễ bảo trì hơn và cung cấp trải nghiệm người dùng tốt hơn thông qua việc xử lý lỗi và xác thực dữ liệu đầu vào một cách bài bản.

---

**Phần 9: Validation, Exception Handling và Data Transfer Objects (DTOs)**

Trong phần này, chúng ta sẽ:

1.  **Giới thiệu về Data Transfer Objects (DTOs):**
    - Tại sao lại cần DTOs (lặp lại và đào sâu hơn từ các phần trước).
    - Phân biệt Entity và DTO.
    - Tạo các DTOs cho Product (tạo/cập nhật), UserRegistration, CheckoutForm (đã có).
2.  **Bean Validation với Jakarta Bean Validation (Hibernate Validator):**
    - Thêm các annotation validation (`@NotNull`, `@NotEmpty`, `@Size`, `@Email`, `@Min`, `@Max`, ...) vào DTOs.
    - Sử dụng `@Valid` trong Controller để kích hoạt validation.
    - Xử lý `BindingResult` để lấy thông tin lỗi validation và hiển thị trên View.
3.  **Custom Validators (Tùy chọn nâng cao):**
    - Khi các annotation có sẵn không đủ (ví dụ: kiểm tra password và confirm password phải giống nhau).
4.  **Global Exception Handling với `@ControllerAdvice` và `@ExceptionHandler`:**
    - Tạo một Global Exception Handler để xử lý các exception một cách tập trung.
    - Map các custom exception (ví dụ: `ResourceNotFoundException`, `DuplicateResourceException`) sang HTTP status codes phù hợp và trả về trang lỗi thân thiện.
    - Xử lý `MethodArgumentNotValidException` (ném ra khi validation DTO thất bại).
5.  **Sử dụng DTOs trong Service Layer và Controller Layer:**
    - Mapping giữa Entity và DTO (thủ công hoặc sử dụng thư viện như MapStruct). Chúng ta sẽ làm thủ công trước để hiểu rõ.
6.  **Cập nhật các Form Thymeleaf để hiển thị lỗi validation.**
7.  **Cải thiện thông báo cho người dùng.**

---

**1. Giới thiệu về Data Transfer Objects (DTOs)**

(Chúng ta đã nói sơ qua ở Phần 4, giờ sẽ đi sâu hơn)

- **Tại sao lại cần DTOs?**

  - **Tách biệt API/View Contract khỏi Domain Model (Entity):**
    - **API Stability:** API của bạn (nếu là REST API) hoặc cấu trúc dữ liệu cho View không nên thay đổi chỉ vì bạn thay đổi cấu trúc Entity (ví dụ: thêm một trường vào Entity mà client/view không cần). DTO đóng vai trò là một lớp trung gian, một "hợp đồng" ổn định.
    - **Tránh lộ Entity:** Entity là biểu diễn của cấu trúc CSDL, có thể chứa các trường nhạy cảm (password hash), các mối quan hệ phức tạp (lazy loading), hoặc các thông tin mà client/view không cần.
  - **Tối ưu hóa dữ liệu truyền tải:**
    - Chỉ gửi những dữ liệu cần thiết cho client/view.
    - Có thể "làm phẳng" (flatten) cấu trúc dữ liệu từ nhiều Entity liên quan vào một DTO duy nhất.
    - Ngược lại, client có thể gửi một DTO chứa dữ liệu để cập nhật nhiều Entity.
  - **Validation:** DTO là nơi lý tưởng để đặt các annotation validation cho dữ liệu đầu vào từ client (request body, form parameters). Validating trực tiếp trên Entity có thể gây ra vấn đề nếu một số trường Entity chỉ được set bởi hệ thống (ví dụ: `id`, `createdAt`).
  - **Xử lý Lazy Loading:** Khi serialize Entity ra JSON, nếu có các trường lazy-loaded chưa được khởi tạo và session Hibernate đã đóng, bạn sẽ gặp `LazyInitializationException`. DTOs giúp bạn chỉ chọn những dữ liệu đã được fetch hoặc cần thiết.
  - **Tránh Circular Dependencies khi Serialize:** Các mối quan hệ hai chiều trong Entities có thể gây vòng lặp vô hạn khi serialize (ví dụ: `Product` có `Category`, `Category` có `Set<Product>`). DTOs giúp phá vỡ vòng lặp này bằng cách chỉ chọn các trường cần thiết.

- **Phân biệt Entity và DTO:**

  - **Entity:**
    - Đại diện cho một đối tượng trong domain của bạn, thường được ánh xạ 1-1 với một bảng trong CSDL.
    - Chứa logic liên quan đến persistence (ví dụ: annotations `@Entity`, `@Id`, `@Column`, các mối quan hệ).
    - Thuộc về Persistence Layer.
  - **DTO:**
    - Là một đối tượng Java đơn giản (POJO) dùng để mang dữ liệu giữa các tầng (ví dụ: Controller <-> Service, Service <-> Client).
    - Không chứa logic nghiệp vụ hay persistence.
    - Chỉ chứa các trường dữ liệu, getters, setters, constructors.
    - Thường được dùng ở Presentation Layer (Controller) và đôi khi ở Service Layer (làm tham số hoặc kiểu trả về).

- **Tạo các DTOs:**
  Chúng ta đã có `CheckoutFormDto.java`. Bây giờ tạo thêm:

  **1.1. `ProductDto.java` (Dùng cho hiển thị và có thể là request tạo/cập nhật cơ bản)**
  Trong package `com.mycompany.ecommerceproject.dto`:

  ```java
  package com.mycompany.ecommerceproject.dto;

  import lombok.Data;
  import lombok.NoArgsConstructor;
  import lombok.AllArgsConstructor;
  import jakarta.validation.constraints.*; // Import các constraint
  import java.math.BigDecimal;

  @Data
  @NoArgsConstructor
  @AllArgsConstructor
  public class ProductDto {
      private Long id;

      @NotEmpty(message = "Product name cannot be empty")
      @Size(min = 3, max = 200, message = "Product name must be between 3 and 200 characters")
      private String name;

      private String description; // Có thể không cần validation phức tạp ở đây

      @NotNull(message = "Price cannot be null")
      @DecimalMin(value = "0.01", message = "Price must be greater than 0")
      private BigDecimal price;

      private String imageUrl;

      @NotNull(message = "Stock quantity cannot be null")
      @Min(value = 0, message = "Stock quantity cannot be negative")
      private Integer stockQuantity;

      @NotNull(message = "Category ID cannot be null for product creation/update")
      private Long categoryId; // Chỉ cần ID của category

      private String categoryName; // Dùng để hiển thị tên category
  }
  ```

  - DTO này có thể dùng để nhận dữ liệu khi tạo/cập nhật sản phẩm (Controller sẽ nhận `ProductDto` từ request).
  - Cũng có thể dùng để gửi dữ liệu sản phẩm ra view/client (Service trả về `ProductDto` cho Controller).
  - Đã thêm các annotation validation.

  **1.2. `UserRegistrationDto.java` (Dùng cho form đăng ký)**
  Trong package `com.mycompany.ecommerceproject.dto`:

  ```java
  package com.mycompany.ecommerceproject.dto;

  import lombok.Data;
  import jakarta.validation.constraints.Email;
  import jakarta.validation.constraints.NotEmpty;
  import jakarta.validation.constraints.Size;

  @Data
  public class UserRegistrationDto {
      @NotEmpty(message = "Username cannot be empty")
      @Size(min = 3, max = 50, message = "Username must be between 3 and 50 characters")
      private String username;

      @NotEmpty(message = "Email cannot be empty")
      @Email(message = "Email should be valid")
      private String email;

      @NotEmpty(message = "Password cannot be empty")
      @Size(min = 6, message = "Password must be at least 6 characters long")
      // Có thể thêm custom validator để kiểm tra độ mạnh password, hoặc confirm password
      private String password;

      // @NotEmpty(message = "Confirm password cannot be empty")
      // private String confirmPassword; // Sẽ cần custom validator để so sánh với password

      private String firstName;
      private String lastName;
  }
  ```

---

**2. Bean Validation với Jakarta Bean Validation (Hibernate Validator)**

Spring Boot tự động tích hợp Hibernate Validator (một implementation của Jakarta Bean Validation) khi bạn có `spring-boot-starter-web` hoặc `spring-boot-starter-validation`.

- **Thêm Annotations vào DTOs:** Chúng ta đã làm ở bước trên. Các annotations phổ biến:

  - `@NotNull`: Giá trị không được null.
  - `@NotEmpty`: Cho chuỗi, collection, map, array - không được null và không được rỗng.
  - `@NotBlank`: Cho chuỗi - không được null và phải chứa ít nhất một ký tự không phải khoảng trắng.
  - `@Size(min=, max=)`: Kiểm tra kích thước (độ dài chuỗi, số phần tử collection).
  - `@Min(value=)` / `@Max(value=)`: Cho số - giá trị nhỏ nhất/lớn nhất.
  - `@DecimalMin(value=, inclusive=)` / `@DecimalMax(value=, inclusive=)`: Cho `BigDecimal`, `BigInteger`.
  - `@Email`: Kiểm tra định dạng email.
  - `@Pattern(regexp=)`: Kiểm tra với một regular expression.
  - `@Future` / `@Past`: Cho Date/Time.
  - `@Valid`: Dùng để kích hoạt validation cho các object lồng nhau.

- **Sử dụng `@Valid` trong Controller:**
  Để Spring kích hoạt quá trình validation cho một DTO được truyền vào controller method (thường là từ request body hoặc form data), bạn cần đánh dấu tham số đó bằng `@Valid`.

  ```java
  // Ví dụ trong AuthController
  import jakarta.validation.Valid;
  import org.springframework.validation.BindingResult;

  // ...
  @PostMapping("/register")
  public String processRegistration(
          @Valid @ModelAttribute("userDto") UserRegistrationDto userDto, // Sử dụng DTO và @Valid
          BindingResult bindingResult, // Phải đặt ngay sau tham số @Valid
          Model model,
          RedirectAttributes redirectAttributes) {

      logger.info("Processing registration for DTO: {}", userDto);
      if (bindingResult.hasErrors()) {
          logger.warn("Registration form validation errors for username: {}", userDto.getUsername());
          // model.addAttribute("userDto", userDto); // Thymeleaf tự động add lại object bị lỗi
          return "register"; // Quay lại trang register, lỗi sẽ được hiển thị
      }
      // ... (logic đăng ký nếu không có lỗi)
  }
  ```

- **Xử lý `BindingResult`:**
  - `BindingResult` là một interface của Spring, chứa kết quả của quá trình validation. Nó **phải được đặt ngay sau tham số được đánh dấu `@Valid`** trong controller method.
  - `bindingResult.hasErrors()`: Kiểm tra xem có lỗi validation nào không.
  - Nếu có lỗi, bạn thường trả về lại view của form, và Thymeleaf có thể hiển thị các lỗi này. Spring tự động thêm `BindingResult` và đối tượng DTO bị lỗi vào Model, nên bạn có thể truy cập chúng trong template.

---

**3. Custom Validators (Tùy chọn nâng cao)**

Đôi khi bạn cần các logic validation phức tạp hơn mà các annotation có sẵn không đáp ứng được, ví dụ:

- So sánh hai trường (ví dụ: `password` và `confirmPassword`).
- Validation dựa trên logic nghiệp vụ phức tạp (ví dụ: kiểm tra username có duy nhất trong CSDL không - mặc dù việc này thường được xử lý ở service layer với exception).

Để tạo custom validator, bạn cần:

1.  Tạo một annotation (ví dụ: `@PasswordMatches`).
2.  Tạo một class implement `ConstraintValidator<YourAnnotation, FieldType>`.

Ví dụ (chúng ta sẽ không implement chi tiết ở đây để giữ độ dài, bạn có thể tìm hiểu thêm):
Annotation `@PasswordMatches`:

```java
// @Target({TYPE, ANNOTATION_TYPE})
// @Retention(RUNTIME)
// @Constraint(validatedBy = PasswordMatchesValidator.class)
// @Documented
// public @interface PasswordMatches {
//     String message() default "Passwords do not match";
//     Class<?>[] groups() default {};
//     Class<? extends Payload>[] payload() default {};
// }
```

Validator `PasswordMatchesValidator`:

```java
// public class PasswordMatchesValidator implements ConstraintValidator<PasswordMatches, Object> {
//    @Override
//    public void initialize(PasswordMatches constraintAnnotation) { }
//    @Override
//    public boolean isValid(Object obj, ConstraintValidatorContext context){
//       UserRegistrationDto user = (UserRegistrationDto) obj;
//       return user.getPassword().equals(user.getConfirmPassword());
//    }
// }
```

Sau đó, bạn có thể dùng `@PasswordMatches` trên class `UserRegistrationDto`.

---

**4. Global Exception Handling với `@ControllerAdvice` và `@ExceptionHandler`**

Thay vì đặt `try-catch` trong từng controller method, `@ControllerAdvice` cho phép bạn định nghĩa các xử lý exception tập trung cho toàn bộ ứng dụng (hoặc một nhóm controller).

- **Tạo GlobalExceptionHandler:**
  Trong package `com.mycompany.ecommerceproject.exception` (hoặc `config`, `advice`):

  ```java
  package com.mycompany.ecommerceproject.exception;

  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;
  import org.springframework.http.HttpStatus;
  import org.springframework.ui.Model;
  import org.springframework.web.bind.MethodArgumentNotValidException;
  import org.springframework.web.bind.annotation.ControllerAdvice;
  import org.springframework.web.bind.annotation.ExceptionHandler;
  import org.springframework.web.bind.annotation.ResponseStatus;
  import org.springframework.web.servlet.ModelAndView; // Dùng ModelAndView nếu muốn trả về view cụ thể

  @ControllerAdvice
  public class GlobalExceptionHandler {

      private static final Logger logger = LoggerFactory.getLogger(GlobalExceptionHandler.class);

      @ExceptionHandler(ResourceNotFoundException.class)
      @ResponseStatus(HttpStatus.NOT_FOUND) // Set HTTP status
      public ModelAndView handleResourceNotFoundException(ResourceNotFoundException ex, Model model) {
          logger.warn("Resource not found: {}", ex.getMessage());
          ModelAndView mav = new ModelAndView("error/404"); // Trả về view error/404.html
          mav.addObject("errorMessage", ex.getMessage());
          mav.addObject("exceptionType", ex.getClass().getSimpleName());
          return mav;
      }

      @ExceptionHandler(DuplicateResourceException.class)
      @ResponseStatus(HttpStatus.CONFLICT)
      public ModelAndView handleDuplicateResourceException(DuplicateResourceException ex, Model model) {
          logger.warn("Duplicate resource: {}", ex.getMessage());
          ModelAndView mav = new ModelAndView("error/409"); // Trả về view error/409.html
          mav.addObject("errorMessage", ex.getMessage());
          mav.addObject("exceptionType", ex.getClass().getSimpleName());
          return mav;
      }

      @ExceptionHandler(BadRequestException.class)
      @ResponseStatus(HttpStatus.BAD_REQUEST)
      public ModelAndView handleBadRequestException(BadRequestException ex, Model model) {
          logger.warn("Bad request: {}", ex.getMessage());
          ModelAndView mav = new ModelAndView("error/400"); // Trả về view error/400.html
          mav.addObject("errorMessage", ex.getMessage());
          mav.addObject("exceptionType", ex.getClass().getSimpleName());
          return mav;
      }

      // Xử lý lỗi validation từ @Valid
      @ExceptionHandler(MethodArgumentNotValidException.class)
      @ResponseStatus(HttpStatus.BAD_REQUEST)
      public ModelAndView handleValidationExceptions(MethodArgumentNotValidException ex) {
          logger.warn("Validation error: {}", ex.getMessage());
          ModelAndView mav = new ModelAndView("error/validation_error"); // View cho lỗi validation chung
          // Lấy các lỗi cụ thể
          // BindingResult result = ex.getBindingResult();
          // Map<String, String> errors = new HashMap<>();
          // result.getFieldErrors().forEach(error -> errors.put(error.getField(), error.getDefaultMessage()));
          // mav.addObject("errors", errors);
          mav.addObject("errorMessage", "Validation failed. Please check your input.");
           // Trong thực tế, bạn có thể muốn redirect lại form với lỗi cụ thể
           // nhưng ở đây làm trang lỗi chung trước.
          return mav;
      }


      @ExceptionHandler(Exception.class) // Bắt tất cả các exception khác
      @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
      public ModelAndView handleGenericException(Exception ex, Model model) {
          logger.error("An unexpected error occurred: ", ex); // Log stack trace
          ModelAndView mav = new ModelAndView("error/500"); // Trả về view error/500.html
          mav.addObject("errorMessage", "An unexpected error occurred. Please try again later.");
          mav.addObject("exceptionType", ex.getClass().getSimpleName());
          // Không nên truyền ex.getMessage() trực tiếp ra view cho lỗi 500 vì lý do bảo mật
          return mav;
      }
  }
  ```

- **Tạo các trang lỗi (ví dụ: `error/404.html`, `error/500.html`, etc.):**
  Trong `src/main/resources/templates/`, tạo thư mục `error`.
  Ví dụ `error/404.html`:
  ```html
  <!DOCTYPE html>
  <html
    xmlns:th="http://www.thymeleaf.org"
    xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
    layout:decorate="~{layouts/default}"
  >
    <head>
      <title>Page Not Found</title>
    </head>
    <body>
      <section layout:fragment="content">
        <div class="container text-center mt-5">
          <h1>404 - Page Not Found</h1>
          <p
            th:text="${errorMessage != null ? errorMessage : 'The page you are looking for does not exist.'}"
          >
            Error message here.
          </p>
          <p th:if="${exceptionType}">
            <small th:text="'Exception: ' + ${exceptionType}"></small>
          </p>
          <a th:href="@{/}" class="btn btn-primary mt-3">Go to Homepage</a>
        </div>
      </section>
    </body>
  </html>
  ```
  Tương tự cho `error/400.html`, `error/409.html`, `error/500.html`, `error/validation_error.html`.
- Bây giờ, khi một service ném `ResourceNotFoundException`, `GlobalExceptionHandler` sẽ bắt nó và hiển thị trang `error/404.html`.

---

**5. Sử dụng DTOs trong Service Layer và Controller Layer**

**5.1. Mapping Entity <-> DTO (Thủ công)**
Bạn cần các phương thức để chuyển đổi giữa Entity và DTO. Có thể đặt chúng trong service, hoặc tạo một class `Mapper` riêng.

**Ví dụ: `ProductMapper` (tạo class này trong package `dto` hoặc `mapper`)**

```java
package com.mycompany.ecommerceproject.mapper; // hoặc .dto

import com.mycompany.ecommerceproject.dto.ProductDto;
import com.mycompany.ecommerceproject.entity.Product;
import com.mycompany.ecommerceproject.entity.Category; // Cần để lấy categoryName

public class ProductMapper {

    public static ProductDto toProductDto(Product product) {
        if (product == null) {
            return null;
        }
        ProductDto dto = new ProductDto();
        dto.setId(product.getId());
        dto.setName(product.getName());
        dto.setDescription(product.getDescription());
        dto.setPrice(product.getPrice());
        dto.setImageUrl(product.getImageUrl());
        dto.setStockQuantity(product.getStockQuantity());
        if (product.getCategory() != null) {
            dto.setCategoryId(product.getCategory().getId());
            dto.setCategoryName(product.getCategory().getName());
        }
        return dto;
    }

    public static Product toProductEntity(ProductDto dto) {
        if (dto == null) {
            return null;
        }
        Product product = new Product();
        // Không set ID khi tạo mới từ DTO, ID sẽ được CSDL sinh ra
        // Nếu là update, ID sẽ được dùng để tìm entity gốc
        // product.setId(dto.getId()); // Cẩn thận khi dùng cho create

        product.setName(dto.getName());
        product.setDescription(dto.getDescription());
        product.setPrice(dto.getPrice());
        product.setImageUrl(dto.getImageUrl());
        product.setStockQuantity(dto.getStockQuantity());

        // Category sẽ được xử lý trong service (tìm Category bằng categoryId từ DTO)
        // Ở đây chỉ tạo đối tượng Category tạm với ID để service xử lý
        if (dto.getCategoryId() != null) {
            Category category = new Category();
            category.setId(dto.getCategoryId());
            // category.setName(dto.getCategoryName()); // Không cần set name ở đây nếu service tìm bằng ID
            product.setCategory(category);
        }
        return product;
    }
}
```

**Tương tự, tạo `UserMapper` cho `UserRegistrationDto` <-> `UserAccount`.**

```java
package com.mycompany.ecommerceproject.mapper;

import com.mycompany.ecommerceproject.dto.UserRegistrationDto;
import com.mycompany.ecommerceproject.entity.UserAccount;

public class UserMapper {
    public static UserAccount toUserAccount(UserRegistrationDto dto) {
        if (dto == null) {
            return null;
        }
        UserAccount user = new UserAccount();
        user.setUsername(dto.getUsername());
        user.setEmail(dto.getEmail());
        user.setPassword(dto.getPassword()); // Password sẽ được hash trong service
        user.setFirstName(dto.getFirstName());
        user.setLastName(dto.getLastName());
        // Role sẽ được gán mặc định trong service hoặc entity
        return user;
    }

    // Có thể không cần toUserRegistrationDto nếu không có use case hiển thị lại form đăng ký với dữ liệu user
}
```

**5.2. Cập nhật Service và Controller để dùng DTO và Mapper**

- **`ProductService` và `ProductServiceImpl`:**

  - Các phương thức `save`, `update` có thể nhận `ProductDto` làm tham số.
  - Các phương thức `findById`, `findAll` có thể trả về `ProductDto` hoặc `Page<ProductDto>`.

  Ví dụ sửa `ProductService`:

  ```java
  // ProductService.java
  // ...
  // Product save(Product product); // Cũ
  ProductDto createProduct(ProductDto productDto); // Mới: nhận DTO, trả về DTO
  Optional<ProductDto> findProductDtoById(Long id);
  Page<ProductDto> findAllProductDtos(Pageable pageable);
  ProductDto updateProduct(Long id, ProductDto productDto);
  // ...
  ```

  Trong `ProductServiceImpl`:

  ```java
  // ProductServiceImpl.java
  // ...
  // @Autowired private CategoryRepository categoryRepository; // Đã có

  @Override
  @Transactional
  public ProductDto createProduct(ProductDto productDto) {
      Product product = ProductMapper.toProductEntity(productDto);
      // Xử lý Category
      if (productDto.getCategoryId() != null) {
          Category category = categoryRepository.findById(productDto.getCategoryId())
                  .orElseThrow(() -> new ResourceNotFoundException("Category not found with id: " + productDto.getCategoryId()));
          product.setCategory(category);
      } else {
           throw new BadRequestException("Category ID is required to create a product.");
      }
      Product savedProduct = productRepository.save(product);
      return ProductMapper.toProductDto(savedProduct);
  }

  @Override
  @Transactional(readOnly = true)
  public Optional<ProductDto> findProductDtoById(Long id) {
      return productRepository.findById(id).map(ProductMapper::toProductDto);
  }

  @Override
  @Transactional(readOnly = true)
  public Page<ProductDto> findAllProductDtos(Pageable pageable) {
      return productRepository.findAll(pageable).map(ProductMapper::toProductDto);
  }

  @Override
  @Transactional
  public ProductDto updateProduct(Long id, ProductDto productDto) {
      Product existingProduct = productRepository.findById(id)
              .orElseThrow(() -> new ResourceNotFoundException("Product not found with id: " + id));

      // Cập nhật các trường từ DTO
      existingProduct.setName(productDto.getName());
      existingProduct.setDescription(productDto.getDescription());
      existingProduct.setPrice(productDto.getPrice());
      existingProduct.setImageUrl(productDto.getImageUrl());
      existingProduct.setStockQuantity(productDto.getStockQuantity());

      if (productDto.getCategoryId() != null &&
          (existingProduct.getCategory() == null || !existingProduct.getCategory().getId().equals(productDto.getCategoryId()))) {
          Category category = categoryRepository.findById(productDto.getCategoryId())
                  .orElseThrow(() -> new ResourceNotFoundException("Category not found with id: " + productDto.getCategoryId()));
          existingProduct.setCategory(category);
      }
      Product updatedProduct = productRepository.save(existingProduct);
      return ProductMapper.toProductDto(updatedProduct);
  }
  // ...
  ```

- **`ProductController`:**

  - Sửa các method để làm việc với `ProductDto`.
    Ví dụ `listProducts`:

  ```java
  // ProductController.java
  // ...
  @GetMapping
  public String listProducts(Model model,
                             @RequestParam(name = "page", defaultValue = "0") int page,
                             @RequestParam(name = "size", defaultValue = "9") int size,
                             @RequestParam(name = "sort", defaultValue = "name,asc") String[] sort) {
      // ... (tạo Pageable) ...
      Page<ProductDto> productsPageDto = productService.findAllProductDtos(pageable); // Gọi service trả về DTO
      model.addAttribute("productsPage", productsPageDto); // Truyền DTO Page ra view
      // ...
      return "products/list";
  }

  @GetMapping("/{id}")
  public String productDetail(@PathVariable("id") Long id, Model model) {
      ProductDto productDto = productService.findProductDtoById(id)
              .orElseThrow(() -> new ResourceNotFoundException("Product not found with id: " + id));
      model.addAttribute("product", productDto); // Truyền DTO ra view
      return "products/detail";
  }
  // ...
  ```

  Các template Thymeleaf (`products/list.html`, `products/detail.html`) giờ sẽ nhận `ProductDto` thay vì `Product` entity. Bạn cần cập nhật các biểu thức Thymeleaf nếu tên trường trong DTO khác Entity (ví dụ `product.categoryName` thay vì `product.category.name`). Trong trường hợp `ProductDto` của chúng ta, tên trường khá giống.

- **`AuthController` (cho Registration):**
  Sửa `processRegistration` để nhận `UserRegistrationDto` và dùng `@Valid`.

  ```java
  // AuthController.java
  // ...
  @GetMapping("/register")
  public String registrationPage(Model model) {
      if (!model.containsAttribute("userDto")) { // Chỉ tạo mới nếu chưa có (do redirect từ lỗi validation)
          model.addAttribute("userDto", new UserRegistrationDto());
      }
      return "register";
  }

  @PostMapping("/register")
  public String processRegistration(
          @Valid @ModelAttribute("userDto") UserRegistrationDto userDto,
          BindingResult bindingResult,
          // Model model, // Không cần Model nếu redirect
          RedirectAttributes redirectAttributes) {

      if (bindingResult.hasErrors()) {
          // Thêm lại DTO và BindingResult vào RedirectAttributes để hiển thị lỗi sau redirect
          // Tuy nhiên, cách này làm URL dài. Tốt hơn là trả về view trực tiếp.
          // redirectAttributes.addFlashAttribute("userDto", userDto);
          // redirectAttributes.addFlashAttribute("org.springframework.validation.BindingResult.userDto", bindingResult);
          // return "redirect:/register?error";
          // Hoặc, đơn giản là trả về view trực tiếp, không redirect
          return "register"; // Spring sẽ tự add userDto và bindingResult vào model
      }
      try {
          UserAccount newUser = UserMapper.toUserAccount(userDto); // Dùng Mapper
          userAccountService.registerUser(newUser); // Service đã xử lý hash password
          redirectAttributes.addFlashAttribute("successMessage", "Registration successful! Please login.");
          return "redirect:/login";
      } catch (DuplicateResourceException e) {
          // Thêm lỗi vào BindingResult để hiển thị trên form
          // bindingResult.rejectValue("username", "error.userDto", e.getMessage()); // Gán lỗi cho field cụ thể
          // Hoặc lỗi chung
          bindingResult.reject("error.global", e.getMessage()); // Lỗi chung cho form

          // redirectAttributes.addFlashAttribute("userDto", userDto);
          // redirectAttributes.addFlashAttribute("org.springframework.validation.BindingResult.userDto", bindingResult);
          // return "redirect:/register?error";
          return "register";
      }
      // ... (các catch khác)
  }
  // ...
  ```

- **`OrderController` (cho Checkout):**
  Sửa `placeOrder` để dùng `@Valid` với `CheckoutFormDto`.
  ```java
  // OrderController.java
  // ...
  @PostMapping("/place")
  public String placeOrder(@Valid @ModelAttribute("checkoutFormDto") CheckoutFormDto checkoutFormDto,
                           BindingResult bindingResult,
                           @AuthenticationPrincipal UserAccount currentUser,
                           Model model, // Dùng Model để trả lại form với lỗi
                           RedirectAttributes redirectAttributes) {
      // ...
      if (bindingResult.hasErrors()) {
          logger.warn("Checkout form validation errors for user {}", currentUser.getUsername());
          Cart cart = cartService.getCartByUser(currentUser);
          model.addAttribute("cart", cart);
          // model.addAttribute("checkoutFormDto", checkoutFormDto); // Spring tự thêm lại
          return "order/checkout"; // Quay lại trang checkout với lỗi validation
      }
      // ... (logic đặt hàng)
  }
  // ...
  ```

---

**6. Cập nhật các Form Thymeleaf để hiển thị lỗi validation**

Thymeleaf cung cấp các utility để dễ dàng hiển thị lỗi từ `BindingResult`.

- **Trong `register.html` (ví dụ cho username):**

  ```html
  <!-- register.html -->
  <form th:action="@{/register}" th:object="${userDto}" method="post">
    <!-- th:object trỏ đến DTO -->
    <!-- ... -->
    <div class="mb-3">
      <label for="username" class="form-label">Username:</label>
      <input
        type="text"
        id="username"
        th:field="*{username}"
        class="form-control"
        th:classappend="${#fields.hasErrors('username')} ? 'is-invalid' : ''"
      />
      <div
        th:if="${#fields.hasErrors('username')}"
        th:errors="*{username}"
        class="invalid-feedback"
      >
        Username error message
      </div>
    </div>

    <div class="mb-3">
      <label for="email" class="form-label">Email:</label>
      <input
        type="email"
        id="email"
        th:field="*{email}"
        class="form-control"
        th:classappend="${#fields.hasErrors('email')} ? 'is-invalid' : ''"
      />
      <div
        th:if="${#fields.hasErrors('email')}"
        th:errors="*{email}"
        class="invalid-feedback"
      >
        Email error message
      </div>
    </div>

    <div class="mb-3">
      <label for="password" class="form-label">Password:</label>
      <input
        type="password"
        id="password"
        th:field="*{password}"
        class="form-control"
        th:classappend="${#fields.hasErrors('password')} ? 'is-invalid' : ''"
      />
      <div
        th:if="${#fields.hasErrors('password')}"
        th:errors="*{password}"
        class="invalid-feedback"
      >
        Password error message
      </div>
    </div>
    <!-- Các trường khác tương tự -->

    <!-- Hiển thị lỗi global (không thuộc field nào cụ thể) -->
    <div th:if="${#fields.hasGlobalErrors()}" class="alert alert-danger mt-3">
      <p th:each="err : ${#fields.globalErrors()}" th:text="${err}">
        Global error message
      </p>
    </div>

    <div class="d-grid">
      <button type="submit" class="btn btn-primary">Register</button>
    </div>
  </form>
  ```

  - `th:object="${userDto}"`: Liên kết form với DTO trong model.
  - `th:field="*{username}"`: Liên kết input field với thuộc tính `username` của `userDto`. Nó tự động xử lý `name`, `id`, `value`.
  - `#fields.hasErrors('username')`: Kiểm tra xem trường `username` có lỗi không.
  - `th:errors="*{username}"`: Hiển thị thông báo lỗi của trường `username`.
  - `th:classappend="${#fields.hasErrors('username')} ? 'is-invalid' : ''"`: Thêm class `is-invalid` của Bootstrap nếu có lỗi để input field có viền đỏ.
  - `#fields.hasGlobalErrors()` và `#fields.globalErrors()`: Để hiển thị các lỗi không gắn với field cụ thể (ví dụ lỗi từ `bindingResult.reject("error.global", ...)`).

- **Tương tự, cập nhật `order/checkout.html`** để hiển thị lỗi cho `CheckoutFormDto`.

---

**7. Cải thiện thông báo cho người dùng**

- **Flash Attributes:** Sử dụng `RedirectAttributes.addFlashAttribute()` để hiển thị thông báo thành công/thất bại sau khi redirect. Chúng ta đã làm điều này ở các phần trước.
- **Hiển thị lỗi validation cụ thể trên form:** Như đã làm ở bước 6.
- **Trang lỗi thân thiện:** `GlobalExceptionHandler` giúp hiển thị các trang lỗi chung thân thiện thay vì stack trace mặc định của Spring.

**Chạy thử ứng dụng:**

1.  Thử đăng ký với dữ liệu không hợp lệ (ví dụ: username quá ngắn, email sai định dạng, password trống). Bạn sẽ thấy các thông báo lỗi trên form.
2.  Thử đăng ký với username/email đã tồn tại. Bạn sẽ thấy thông báo lỗi (từ `DuplicateResourceException` được `GlobalExceptionHandler` xử lý, hoặc từ `BindingResult.reject()` trong controller).
3.  Thử truy cập một sản phẩm không tồn tại (ví dụ: `/products/99999`). Bạn sẽ thấy trang lỗi 404.
4.  Thử checkout với thông tin không hợp lệ (sau khi đã implement validation cho `CheckoutFormDto` đầy đủ).

**Kết thúc Phần 9**

Chúng ta đã nâng cấp ứng dụng của mình đáng kể:

- Sử dụng DTOs để tách biệt dữ liệu truyền tải và validation.
- Implement Bean Validation để đảm bảo dữ liệu đầu vào hợp lệ.
- Xây dựng Global Exception Handler để xử lý lỗi một cách nhất quán và thân thiện.
- Cập nhật các form để hiển thị lỗi validation.

Ứng dụng giờ đây không chỉ hoạt động mà còn "thông minh" hơn trong việc xử lý dữ liệu và lỗi.

**Trong Phần 10 (phần cuối cùng của lộ trình chính), chúng ta sẽ: "Xây dựng REST APIs cơ bản và tổng kết, gợi ý hướng phát triển."** Chúng ta sẽ tạo một vài API đơn giản để minh họa cách Spring Boot có thể phục vụ cả web UI và API, đồng thời nhìn lại toàn bộ dự án và thảo luận về các bước tiếp theo.

Tuyệt vời! Chúng ta đã đi đến phần cuối cùng trong lộ trình 10 phần này. Sau khi xây dựng một ứng dụng web hoàn chỉnh với giao diện người dùng, việc cung cấp các REST APIs sẽ mở rộng khả năng tương tác của hệ thống, cho phép các client khác (ví dụ: ứng dụng di động, hệ thống của bên thứ ba) có thể truy cập và thao tác dữ liệu.

---

**Phần 10: Xây dựng REST APIs cơ bản và Tổng kết, Gợi ý hướng phát triển**

Trong phần này, chúng ta sẽ:

1.  **Giới thiệu về REST APIs và Spring MVC cho REST.**
    - Các nguyên tắc của REST (Representational State Transfer).
    - `@RestController`, `@ResponseBody`, `@RequestBody`.
    - HTTP Methods (GET, POST, PUT, DELETE).
    - HTTP Status Codes.
2.  **Thiết kế một vài REST API Endpoints cơ bản:**
    - Lấy danh sách sản phẩm (có phân trang, lọc).
    - Lấy chi tiết một sản phẩm.
    - Tạo một sản phẩm mới (yêu cầu quyền ADMIN).
    - Cập nhật một sản phẩm (yêu cầu quyền ADMIN).
    - Xóa một sản phẩm (yêu cầu quyền ADMIN).
3.  **Sử dụng DTOs cho Request và Response của API.**
4.  **Xử lý Authentication và Authorization cho API Endpoints (sử dụng Spring Security đã cấu hình).**
5.  **Xây dựng `ProductRestController`.**
6.  **Kiểm thử API sử dụng công cụ như Postman hoặc curl.**
7.  **Tổng kết dự án:**
    - Nhìn lại các kiến thức và kỹ năng đã học.
    - Đánh giá sản phẩm đã xây dựng.
8.  **Gợi ý hướng phát triển tiếp theo:**
    - Hoàn thiện các tính năng (thanh toán, quản lý kho nâng cao, khuyến mãi,...).
    - Tối ưu hóa (caching, database optimization).
    - Testing (Unit test, Integration test).
    - Deployment (Docker, Cloud).
    - Microservices.
    - Sử dụng thư viện mapping (MapStruct).
    - GraphQL.
    - Và nhiều hơn nữa...

---

**1. Giới thiệu về REST APIs và Spring MVC cho REST**

- **REST (Representational State Transfer):**
  - Là một kiểu kiến trúc phần mềm cho các hệ thống phân tán, đặc biệt là các ứng dụng web.
  - Không phải là một tiêu chuẩn hay giao thức, mà là một tập hợp các ràng buộc kiến trúc.
  - **Các nguyên tắc chính của REST:**
    1.  **Client-Server:** Tách biệt client và server.
    2.  **Stateless:** Mỗi request từ client đến server phải chứa tất cả thông tin cần thiết để server hiểu và xử lý request đó. Server không lưu trữ trạng thái (context) của client giữa các request.
    3.  **Cacheable:** Response từ server có thể được đánh dấu là cacheable hoặc non-cacheable.
    4.  **Uniform Interface (Giao diện đồng nhất):** Đây là ràng buộc quan trọng nhất, bao gồm:
        - **Resource Identification:** Các tài nguyên (resource) được định danh bằng URI (Uniform Resource Identifier), ví dụ: `/api/products`, `/api/products/1`.
        - **Resource Manipulation Through Representations:** Client tương tác với tài nguyên thông qua các biểu diễn (representation) của chúng (ví dụ: JSON, XML).
        - **Self-descriptive Messages:** Mỗi message (request/response) phải tự mô tả đủ để hiểu (ví dụ: sử dụng HTTP headers như `Content-Type`, `Accept`).
        - **Hypermedia as the Engine of Application State (HATEOAS):** (Nâng cao) Response nên chứa các liên kết (hyperlinks) để client có thể khám phá các hành động hoặc tài nguyên liên quan khác.
    5.  **Layered System:** Có thể có các tầng trung gian (proxy, gateway) giữa client và server.
- **Spring MVC cho REST:**
  - `@RestController`: Là một annotation tiện lợi, kết hợp `@Controller` và `@ResponseBody`. Nó chỉ ra rằng tất cả các handler method trong controller này sẽ trả về dữ liệu trực tiếp trong response body (thường là JSON hoặc XML), thay vì trả về tên view.
  - `@ResponseBody`: (Có thể dùng trên từng method nếu class là `@Controller`). Đánh dấu rằng giá trị trả về của method sẽ được ghi trực tiếp vào HTTP response body. Spring sử dụng các `HttpMessageConverter` (ví dụ: `MappingJackson2HttpMessageConverter` cho JSON) để chuyển đổi đối tượng Java sang định dạng mong muốn.
  - `@RequestBody`: Đánh dấu tham số của method để nhận dữ liệu từ HTTP request body (thường là JSON/XML) và chuyển đổi nó thành một đối tượng Java.
  - **HTTP Methods:**
    - `GET`: Lấy thông tin tài nguyên. (Idempotent, Safe)
    - `POST`: Tạo mới một tài nguyên. (Non-idempotent)
    - `PUT`: Cập nhật toàn bộ một tài nguyên đã tồn tại (hoặc tạo mới nếu chưa có và server cho phép). (Idempotent)
    - `PATCH`: Cập nhật một phần của tài nguyên đã tồn tại. (Non-idempotent, mặc dù có thể làm idempotent)
    - `DELETE`: Xóa một tài nguyên. (Idempotent)
  - **HTTP Status Codes:** Rất quan trọng để REST API truyền tải đúng ngữ nghĩa của response.
    - `200 OK`: Request thành công (cho GET, PUT, PATCH, DELETE).
    - `201 Created`: Tạo tài nguyên thành công (cho POST). Response thường chứa URI của tài nguyên mới trong header `Location`.
    - `204 No Content`: Request thành công nhưng không có nội dung trả về (cho DELETE, hoặc PUT/PATCH không trả về body).
    - `400 Bad Request`: Request không hợp lệ (sai cú pháp, thiếu tham số, validation DTO thất bại).
    - `401 Unauthorized`: Cần xác thực (chưa đăng nhập).
    - `403 Forbidden`: Đã xác thực nhưng không có quyền truy cập tài nguyên.
    - `404 Not Found`: Tài nguyên không tồn tại.
    - `409 Conflict`: Xung đột (ví dụ: cố gắng tạo tài nguyên đã tồn tại).
    - `500 Internal Server Error`: Lỗi không xác định phía server.

---

**2. Thiết kế một vài REST API Endpoints cơ bản cho Product**

Chúng ta sẽ tạo các API cho quản lý sản phẩm. Base path có thể là `/api/v1/products`.

| HTTP Method | URI              | Action                              | Request Body DTO | Response Body DTO/List<DTO> | Status Codes                 | Bảo mật      |
| :---------- | :--------------- | :---------------------------------- | :--------------- | :-------------------------- | :--------------------------- | :----------- |
| `GET`       | `/products`      | Lấy danh sách sản phẩm (phân trang) | -                | `Page<ProductDto>`          | `200 OK`                     | Public       |
| `GET`       | `/products/{id}` | Lấy chi tiết sản phẩm theo ID       | -                | `ProductDto`                | `200 OK`, `404 Not Found`    | Public       |
| `POST`      | `/products`      | Tạo sản phẩm mới                    | `ProductDto`     | `ProductDto` (đã tạo)       | `201 Created`, `400 Bad Req` | `ROLE_ADMIN` |
| `PUT`       | `/products/{id}` | Cập nhật sản phẩm theo ID           | `ProductDto`     | `ProductDto` (đã cập nhật)  | `200 OK`, `400`, `404`       | `ROLE_ADMIN` |
| `DELETE`    | `/products/{id}` | Xóa sản phẩm theo ID                | -                | -                           | `204 No Content`, `404`      | `ROLE_ADMIN` |

---

**3. Sử dụng DTOs cho Request và Response của API**

Chúng ta đã tạo `ProductDto` ở Phần 9. DTO này rất phù hợp để dùng cho cả request (khi tạo/cập nhật) và response của các API sản phẩm. Nó chứa các trường cần thiết và có thể có validation.

---

**4. Xử lý Authentication và Authorization cho API Endpoints**

Spring Security đã được cấu hình. Chúng ta có thể sử dụng:

- **Cấu hình trong `SecurityConfig`:**

  ```java
  // Trong SecurityConfig.java, phương thức securityFilterChain
  // ...
  .authorizeHttpRequests(authorize -> authorize
      // ... (các rule cho web UI)
      .requestMatchers(HttpMethod.GET, "/api/v1/products", "/api/v1/products/**").permitAll() // Ai cũng được xem sản phẩm
      .requestMatchers(HttpMethod.POST, "/api/v1/products").hasRole("ADMIN")
      .requestMatchers(HttpMethod.PUT, "/api/v1/products/**").hasRole("ADMIN")
      .requestMatchers(HttpMethod.DELETE, "/api/v1/products/**").hasRole("ADMIN")
      // ...
      .anyRequest().authenticated()
  )
  // ...
  // Đối với REST API, chúng ta thường không dùng formLogin mà dùng các cơ chế khác như Basic Auth, JWT, OAuth2
  // Tuy nhiên, để đơn giản cho phần này, nếu client API (như Postman) có thể quản lý session cookie
  // thì formLogin vẫn có thể hoạt động. Hoặc có thể dùng Basic Auth đơn giản.
  // Thêm .httpBasic(Customizer.withDefaults()) để bật Basic Authentication
  .httpBasic(Customizer.withDefaults()) // Cho phép Basic Auth
  // Quan trọng: Tắt CSRF cho API endpoints nếu client không phải là trình duyệt web (ví dụ: app di động)
  // hoặc client không thể dễ dàng gửi CSRF token.
  // .csrf(csrf -> csrf.ignoringRequestMatchers("/api/**")) // Tắt CSRF cho /api/**
  // HOẶC TẮT TOÀN BỘ (Không khuyến khích cho web UI, chỉ cho API nếu cần)
  .csrf(csrf -> csrf.disable()) // Tạm thời tắt cho dễ test API
  // ...
  ```

  - `HttpMethod.GET, "/api/v1/products", "/api/v1/products/**"`: Phân biệt rõ GET request đến các path này.
  - `httpBasic()`: Kích hoạt Basic Authentication. Client sẽ gửi header `Authorization: Basic <base64_encoded_username:password>`.
  - **CSRF và API:** Nếu API của bạn được gọi bởi các non-browser clients, CSRF protection có thể không cần thiết hoặc gây khó khăn. Bạn có thể tắt CSRF cho các API path cụ thể (`csrf.ignoringRequestMatchers("/api/**")`) hoặc tắt hoàn toàn (nếu ứng dụng chỉ là API server). **Nếu ứng dụng của bạn có cả Web UI (dùng session/cookie) và API, hãy cẩn thận khi tắt CSRF.** Web UI vẫn nên được bảo vệ.
  - **Lưu ý về Session:** Mặc định, Spring Security tạo session. Đối với REST API thuần túy (stateless), bạn nên cấu hình để không tạo session:
    ```java
    // .sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
    ```
    Tuy nhiên, nếu làm vậy, các cơ chế dựa trên session như formLogin sẽ không hoạt động. Cho demo này, chúng ta có thể giữ session mặc định và test API với Postman (Postman có thể gửi cookie).

- **Sử dụng Annotation `@PreAuthorize` trên method (nếu `@EnableMethodSecurity` được bật):**
  ```java
  // Trong ProductRestController
  // @PostMapping
  // @PreAuthorize("hasRole('ADMIN')")
  // public ResponseEntity<ProductDto> createProduct(...) { ... }
  ```
  Cách này linh hoạt hơn để bảo vệ từng method.

---

**5. Xây dựng `ProductRestController`**

Tạo file `ProductRestController.java` trong package `com.mycompany.ecommerceproject.controller` (hoặc một package con `api`):

```java
package com.mycompany.ecommerceproject.controller.api; // Đặt trong package con api

import com.mycompany.ecommerceproject.dto.ProductDto;
import com.mycompany.ecommerceproject.service.ProductService;
import com.mycompany.ecommerceproject.exception.ResourceNotFoundException; // Dùng exception đã có
import jakarta.validation.Valid;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;
import org.springframework.data.domain.Sort;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.security.access.prepost.PreAuthorize; // Cho method-level security
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.server.ResponseStatusException; // Cách khác để trả về lỗi HTTP
import org.springframework.web.util.UriComponentsBuilder;

import java.net.URI;
import java.util.HashMap;
import java.util.Map;
import java.util.stream.Collectors;


@RestController // Quan trọng: Đánh dấu đây là REST Controller
@RequestMapping("/api/v1/products") // Base path cho tất cả các API trong controller này
public class ProductRestController {

    private static final Logger logger = LoggerFactory.getLogger(ProductRestController.class);
    private final ProductService productService;

    public ProductRestController(ProductService productService) {
        this.productService = productService;
    }

    // GET /api/v1/products
    @GetMapping
    public ResponseEntity<Page<ProductDto>> getAllProducts(
            @RequestParam(name = "page", defaultValue = "0") int page,
            @RequestParam(name = "size", defaultValue = "10") int size,
            @RequestParam(name = "sort", defaultValue = "name,asc") String[] sort) {

        logger.info("API request to list products - page: {}, size: {}, sort: {}", page, size, sort);
        try {
            String sortField = sort[0];
            Sort.Direction sortDirection = (sort.length > 1 && sort[1].equalsIgnoreCase("desc")) ?
                                           Sort.Direction.DESC : Sort.Direction.ASC;
            Pageable pageable = PageRequest.of(page, size, Sort.by(sortDirection, sortField));
            Page<ProductDto> productsPage = productService.findAllProductDtos(pageable);
            return ResponseEntity.ok(productsPage); // 200 OK
        } catch (Exception e) {
            logger.error("Error fetching products via API", e);
            // Có thể trả về lỗi cụ thể hơn nếu cần
            throw new ResponseStatusException(HttpStatus.INTERNAL_SERVER_ERROR, "Error fetching products", e);
        }
    }

    // GET /api/v1/products/{id}
    @GetMapping("/{id}")
    public ResponseEntity<ProductDto> getProductById(@PathVariable Long id) {
        logger.info("API request to get product by id: {}", id);
        return productService.findProductDtoById(id)
                .map(ResponseEntity::ok) // Nếu tìm thấy, trả về 200 OK với ProductDto
                .orElseThrow(() -> new ResourceNotFoundException("Product not found with id: " + id)); // Nếu không, GlobalExceptionHandler sẽ bắt và trả về 404
    }

    // POST /api/v1/products
    @PostMapping
    @PreAuthorize("hasRole('ADMIN')") // Chỉ ADMIN mới được tạo sản phẩm
    public ResponseEntity<?> createProduct(@Valid @RequestBody ProductDto productDto, BindingResult bindingResult, UriComponentsBuilder ucb) {
        logger.info("API request to create product: {}", productDto.getName());
        if (bindingResult.hasErrors()) {
            // Trả về lỗi validation
            Map<String, String> errors = bindingResult.getFieldErrors().stream()
                    .collect(Collectors.toMap(
                            fieldError -> fieldError.getField(),
                            fieldError -> fieldError.getDefaultMessage() == null ? "Invalid value" : fieldError.getDefaultMessage()
                    ));
            return ResponseEntity.badRequest().body(errors); // 400 Bad Request với chi tiết lỗi
        }
        try {
            ProductDto createdProduct = productService.createProduct(productDto);
            // Tạo URI cho resource mới được tạo
            URI location = ucb.path("/api/v1/products/{id}")
                              .buildAndExpand(createdProduct.getId())
                              .toUri();
            return ResponseEntity.created(location).body(createdProduct); // 201 Created
        } catch (Exception e) { // Bắt các lỗi khác từ service (ví dụ DuplicateResourceException nếu chưa được xử lý bởi GlobalExceptionHandler cho API)
            logger.error("Error creating product via API: {}", e.getMessage());
            // Có thể map exception cụ thể sang HTTP status code phù hợp
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("Error creating product: " + e.getMessage());
        }
    }

    // PUT /api/v1/products/{id}
    @PutMapping("/{id}")
    @PreAuthorize("hasRole('ADMIN')")
    public ResponseEntity<?> updateProduct(@PathVariable Long id,
                                              @Valid @RequestBody ProductDto productDto, BindingResult bindingResult) {
        logger.info("API request to update product with id: {}", id);
         if (bindingResult.hasErrors()) {
            Map<String, String> errors = bindingResult.getFieldErrors().stream()
                    .collect(Collectors.toMap(fieldError -> fieldError.getField(), fieldError -> fieldError.getDefaultMessage()));
            return ResponseEntity.badRequest().body(errors);
        }
        try {
            ProductDto updatedProduct = productService.updateProduct(id, productDto);
            return ResponseEntity.ok(updatedProduct); // 200 OK
        } catch (ResourceNotFoundException ex) {
            throw ex; // Để GlobalExceptionHandler xử lý thành 404
        } catch (Exception e) {
            logger.error("Error updating product via API for id {}: {}", id, e.getMessage());
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("Error updating product: " + e.getMessage());
        }
    }

    // DELETE /api/v1/products/{id}
    @DeleteMapping("/{id}")
    @PreAuthorize("hasRole('ADMIN')")
    public ResponseEntity<Void> deleteProduct(@PathVariable Long id) {
        logger.info("API request to delete product with id: {}", id);
        try {
            // Kiểm tra sản phẩm có tồn tại không trước khi xóa
            productService.findProductDtoById(id)
                .orElseThrow(() -> new ResourceNotFoundException("Product not found with id: " + id + " for deletion."));

            productService.deleteById(id); // Service này cần được tạo/sửa để hoạt động đúng
                                           // (ProductService hiện tại chỉ có deleteById(Long id) không trả về gì)
            return ResponseEntity.noContent().build(); // 204 No Content
        } catch (ResourceNotFoundException ex) {
            throw ex; // Để GlobalExceptionHandler xử lý thành 404
        } catch (Exception e) { // Ví dụ: DataIntegrityViolationException nếu sản phẩm đang được tham chiếu
            logger.error("Error deleting product via API for id {}: {}", id, e.getMessage());
            // Có thể trả về 409 Conflict nếu không xóa được do ràng buộc CSDL
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(null); // Hoặc .build()
        }
    }
}
```

- `@RestController`: Tất cả các method sẽ trả về dữ liệu trong response body.
- `@RequestMapping("/api/v1/products")`: Base path.
- `ResponseEntity<T>`: Cho phép kiểm soát toàn bộ HTTP response, bao gồm status code, headers, và body.
- `@RequestBody ProductDto productDto`: Nhận JSON từ request body và chuyển thành `ProductDto`.
- `@Valid`: Kích hoạt validation cho `productDto`.
- `BindingResult`: Bắt lỗi validation. Đối với API, chúng ta thường trả về JSON chứa các lỗi thay vì redirect.
- `UriComponentsBuilder ucb`: Dùng để tạo URI cho resource mới (trong header `Location` của response 201).
- `@PreAuthorize("hasRole('ADMIN')")`: Đảm bảo chỉ người dùng có vai trò `ROLE_ADMIN` mới thực hiện được các thao tác tạo, sửa, xóa.

**Lưu ý:**

- `ProductService` cần có phương thức `deleteById(Long id)` (hiện đã có, nhưng cần đảm bảo nó hoạt động đúng).
- `GlobalExceptionHandler` đã tạo ở Phần 9 sẽ xử lý `ResourceNotFoundException` và trả về 404 cho API. Bạn có thể tùy chỉnh `GlobalExceptionHandler` để trả về JSON error response thay vì trang HTML cho các request API (ví dụ, bằng cách kiểm tra header `Accept: application/json`).
  - **Nâng cao GlobalExceptionHandler cho API:**
    ```java
    // Trong GlobalExceptionHandler
    // @ExceptionHandler(ResourceNotFoundException.class)
    // public ResponseEntity<Object> handleResourceNotFoundExceptionForApi(ResourceNotFoundException ex, WebRequest request) {
    //     if (request.getHeader("Accept") != null && request.getHeader("Accept").contains("application/json")) {
    //         Map<String, Object> body = new LinkedHashMap<>();
    //         body.put("timestamp", LocalDateTime.now());
    //         body.put("status", HttpStatus.NOT_FOUND.value());
    //         body.put("error", "Not Found");
    //         body.put("message", ex.getMessage());
    //         body.put("path", request.getDescription(false).replace("uri=", ""));
    //         return new ResponseEntity<>(body, HttpStatus.NOT_FOUND);
    //     } else {
    //         // Trả về ModelAndView cho web UI
    //         ModelAndView mav = new ModelAndView("error/404");
    //         mav.addObject("errorMessage", ex.getMessage());
    //         return mav; // Cần sửa kiểu trả về của method thành Object hoặc ResponseEntity<?>
    //     }
    // }
    ```
    Để làm điều này, bạn cần thay đổi kiểu trả về của các `@ExceptionHandler` method thành `ResponseEntity<Object>` hoặc `Object` và kiểm tra header `Accept`.

---

**6. Kiểm thử API sử dụng công cụ như Postman hoặc curl**

- **Khởi động ứng dụng Spring Boot.**
- **Sử dụng Postman:**

  1.  **GET All Products:**
      - Method: `GET`
      - URL: `http://localhost:8080/api/v1/products?page=0&size=5&sort=price,desc`
      - Không cần Body.
      - Nhấn Send. Bạn sẽ thấy JSON response chứa danh sách sản phẩm.
  2.  **GET Product by ID:**
      - Method: `GET`
      - URL: `http://localhost:8080/api/v1/products/1` (thay 1 bằng ID sản phẩm có thật)
      - Nhấn Send.
  3.  **POST Create Product (Cần ADMIN):**
      - Method: `POST`
      - URL: `http://localhost:8080/api/v1/products`
      - **Authorization Tab:** Chọn Type "Basic Auth". Nhập username (`admin`) và password (`adminpass` - hoặc password bạn đã đặt và hash cho admin).
      - **Headers Tab:** Thêm `Content-Type` với value `application/json`.
      - **Body Tab:** Chọn "raw" và "JSON". Nhập JSON cho `ProductDto`:
        ```json
        {
          "name": "API Test Product",
          "description": "Created via API",
          "price": 99.99,
          "stockQuantity": 10,
          "categoryId": 1 // ID của category đã tồn tại
        }
        ```
      - Nhấn Send. Nếu thành công, bạn sẽ nhận được status `201 Created` và sản phẩm vừa tạo trong body, cùng header `Location`.
  4.  **PUT Update Product (Cần ADMIN):**
      - Method: `PUT`
      - URL: `http://localhost:8080/api/v1/products/ID_CUA_SP_VUA_TAO`
      - **Authorization:** Basic Auth (admin/adminpass).
      - **Headers:** `Content-Type: application/json`.
      - **Body:** JSON `ProductDto` với thông tin cập nhật:
        ```json
        {
          "name": "API Test Product Updated",
          "description": "Updated via API",
          "price": 109.99,
          "stockQuantity": 5,
          "categoryId": 1
        }
        ```
      - Nhấn Send.
  5.  **DELETE Product (Cần ADMIN):**
      - Method: `DELETE`
      - URL: `http://localhost:8080/api/v1/products/ID_CUA_SP_VUA_TAO_HOAC_UPDATE`
      - **Authorization:** Basic Auth (admin/adminpass).
      - Nhấn Send. Bạn sẽ nhận được status `204 No Content`.

- **Sử dụng `curl` (từ terminal):**

  ```bash
  # GET All Products
  curl -X GET "http://localhost:8080/api/v1/products"

  # GET Product by ID
  curl -X GET "http://localhost:8080/api/v1/products/1"

  # POST Create Product (Cần ADMIN)
  curl -X POST "http://localhost:8080/api/v1/products" \
  -u admin:adminpass \
  -H "Content-Type: application/json" \
  -d '{
      "name": "Curl Test Product",
      "description": "Created via Curl API",
      "price": 79.99,
      "stockQuantity": 15,
      "categoryId": 2
  }'

  # DELETE Product (Cần ADMIN)
  curl -X DELETE "http://localhost:8080/api/v1/products/ID_CUA_SP_VUA_TAO" -u admin:adminpass
  ```

---

**7. Tổng kết dự án**

Qua 10 phần, chúng ta đã cùng nhau xây dựng một ứng dụng thương mại điện tử cơ bản nhưng đầy đủ các thành phần quan trọng của một dự án Spring Boot hiện đại:

- **Phần 1:** Khởi tạo dự án, "Hello World".
- **Phần 2:** Thiết lập Database (H2), Entities (JPA - Product, Category, UserAccount).
- **Phần 3:** Repository Layer (Spring Data JPA), CRUD, Query Methods.
- **Phần 4:** Service Layer, Business Logic, `@Transactional`.
- **Phần 5:** Controller Layer (Spring MVC), Thymeleaf templates, hiển thị dữ liệu.
- **Phần 6:** Spring Security (Authentication, Authorization), Custom Login/Registration.
- **Phần 7:** Tính năng Giỏ hàng (Shopping Cart).
- **Phần 8:** Tính năng Đặt hàng (Order Processing), cập nhật tồn kho.
- **Phần 9:** DTOs, Bean Validation, Global Exception Handling.
- **Phần 10:** REST APIs cơ bản, tổng kết.

**Kiến thức và kỹ năng đã học:**

- Hiểu và sử dụng Spring Boot, Spring MVC, Spring Data JPA, Spring Security.
- Thiết kế CSDL quan hệ, làm việc với JPA Entities và mối quan hệ.
- Xây dựng ứng dụng theo kiến trúc 3 lớp (Controller-Service-Repository).
- Sử dụng Thymeleaf để xây dựng giao diện web động.
- Bảo mật ứng dụng (xác thực, phân quyền).
- Xử lý form, validation, exception.
- Sử dụng DTOs.
- Xây dựng REST APIs cơ bản.
- Sử dụng Maven, IntelliJ IDEA (hoặc IDE khác).

**Đánh giá sản phẩm:**

- Ứng dụng hiện tại là một nền tảng tốt, có các chức năng cơ bản của một trang e-commerce.
- Code được tổ chức theo các lớp, dễ theo dõi.
- Đã áp dụng nhiều best practices của Spring Boot.
- Tuy nhiên, còn nhiều chỗ cần cải thiện và nhiều tính năng có thể thêm vào để trở thành một sản phẩm hoàn chỉnh cho thực tế.

---

**8. Gợi ý hướng phát triển tiếp theo**

Đây chỉ là điểm khởi đầu. Để nâng cao tay nghề và làm cho dự án hoàn thiện hơn, bạn có thể xem xét các hướng sau:

- **Hoàn thiện tính năng:**

  - **Thanh toán:** Tích hợp cổng thanh toán (PayPal, Stripe, MoMo, VNPay,...).
  - **Quản lý Sản phẩm (Admin UI):** Giao diện cho admin để thêm/sửa/xóa sản phẩm, category, quản lý user, xem đơn hàng.
  - **Quản lý Đơn hàng (Admin UI):** Admin cập nhật trạng thái đơn hàng.
  - **Tìm kiếm nâng cao:** Lọc sản phẩm theo nhiều tiêu chí, sắp xếp.
  - **Đánh giá sản phẩm (Product Reviews).**
  - **Khuyến mãi, Mã giảm giá (Promotions, Coupons).**
  - **Quản lý tồn kho nâng cao:** Thông báo khi sắp hết hàng, quản lý nhập hàng.
  - **User Profile Management:** Cho phép user cập nhật thông tin cá nhân, đổi mật khẩu, xem lịch sử đơn hàng.
  - **Email Notifications:** Gửi email xác nhận đăng ký, xác nhận đơn hàng, thông báo giao hàng. (Sử dụng `spring-boot-starter-mail`).
  - **Guest Checkout / Guest Cart:** Cho phép người dùng chưa đăng nhập vẫn mua hàng.

- **Kỹ thuật và Tối ưu hóa:**

  - **Database:** Chuyển sang CSDL thực tế như MySQL, PostgreSQL.
  - **Database Migration:** Sử dụng Flyway hoặc Liquibase để quản lý thay đổi schema CSDL một cách version-controlled.
  - **Caching:** Sử dụng Spring Cache (với EhCache, Redis, Caffeine) để tăng hiệu suất cho các dữ liệu ít thay đổi.
  - **Asynchronous Operations:** Sử dụng `@Async` cho các tác vụ tốn thời gian không cần kết quả ngay (ví dụ: gửi email).
  - **Phân trang và Tải lười (Lazy Loading) hiệu quả:** Tối ưu các query, tránh N+1 select problem.
  - **Sử dụng MapStruct:** Để tự động hóa việc mapping giữa Entity và DTO một cách hiệu quả (compile-time).
  - **Logging nâng cao:** Cấu hình logging chi tiết hơn, tập trung log (ví dụ: ELK stack).
  - **Monitoring:** Sử dụng Spring Boot Actuator, Prometheus, Grafana để theo dõi sức khỏe ứng dụng.

- **Testing:**

  - **Unit Tests:** Test từng class/method riêng lẻ (JUnit, Mockito). Test Service, Controller (dùng MockMvc).
  - **Integration Tests:** Test sự tương tác giữa các component (`@SpringBootTest`). Test Repository Layer (`@DataJpaTest`).
  - **End-to-End Tests:** Sử dụng Selenium, Cypress.

- **Deployment:**

  - **Build Executable Jar:** Spring Boot tạo ra file JAR có thể chạy độc lập.
  - **Docker:** Đóng gói ứng dụng vào Docker container.
  - **Cloud Platforms:** Triển khai lên AWS, Google Cloud, Azure, Heroku.
  - **CI/CD:** Thiết lập quy trình Continuous Integration/Continuous Deployment (Jenkins, GitLab CI, GitHub Actions).

- **Kiến trúc nâng cao:**

  - **Microservices:** Nếu ứng dụng phát triển lớn, có thể cân nhắc chia thành các microservices.
  - **Event-Driven Architecture:** Sử dụng message queues (Kafka, RabbitMQ) cho các tương tác bất đồng bộ.
  - **GraphQL API:** Cân nhắc sử dụng GraphQL thay thế hoặc bổ sung cho REST API để client có thể yêu cầu chính xác dữ liệu họ cần.

- **Giao diện người dùng (Frontend):**
  - Sử dụng các framework JavaScript hiện đại như React, Angular, Vue.js để xây dựng Single Page Application (SPA) tương tác với backend API.
  - Cải thiện CSS, UI/UX.

---

**Đề xuất Lộ trình các phần tiếp theo về REST API:**

- **Phần 11: Documenting REST APIs với OpenAPI (Swagger).** (Rất quan trọng cho việc làm việc nhóm và cho client sử dụng API)
- **Phần 12: Advanced REST API Techniques: Versioning, HATEOAS, Custom Error Responses.** (Làm cho API chuyên nghiệp và dễ sử dụng hơn)
- **Phần 13: Securing REST APIs với JWT (JSON Web Tokens).** (Cơ chế xác thực phổ biến cho stateless APIs)
- **(Tùy chọn) Phần 14: Testing REST APIs (Unit & Integration Tests).** (Đảm bảo chất lượng API)

Chúng ta sẽ bắt đầu với Phần 11.

---

**Phần 11: Documenting REST APIs với OpenAPI (Swagger)**

Trong phần này, chúng ta sẽ:

1.  **Tại sao cần Document API?**
2.  **Giới thiệu OpenAPI Specification và Swagger.**
    - OpenAPI Specification (OAS).
    - Swagger UI, Swagger Editor, Swagger Codegen.
3.  **Tích hợp Springdoc-openapi vào dự án Spring Boot.** (Thư viện phổ biến thay thế cho SpringFox đã cũ).
4.  **Cấu hình cơ bản và tùy chỉnh thông tin API (Info, Servers, Security Schemes).**
5.  **Sử dụng Annotations của Springdoc-openapi để mô tả API Endpoints, DTOs.**
    - `@Operation`, `@Parameter`, `@ApiResponse`, `@Schema`, `@Tag`.
6.  **Truy cập Swagger UI để xem và tương tác với API Documentation.**
7.  **Tạo tài liệu cho các `ProductRestController` endpoints đã có.**
8.  **Lưu ý và Best Practices khi viết tài liệu API.**

---

**1. Tại sao cần Document API?**

Khi bạn xây dựng REST API, việc có tài liệu rõ ràng và cập nhật là vô cùng quan trọng vì những lý do sau:

- **Cho Client Developers (Frontend, Mobile, Third-party):**
  - Họ cần biết API của bạn cung cấp những endpoint nào, cách gọi chúng (HTTP method, URL, headers, request parameters, request body), định dạng dữ liệu mong đợi và trả về, các mã lỗi có thể xảy ra.
  - Tài liệu tốt giúp họ tích hợp với API của bạn nhanh chóng và chính xác hơn, giảm thiểu lỗi và thời gian trao đổi.
- **Cho Team Members (Backend Developers):**
  - Giúp các thành viên trong nhóm (kể cả chính bạn trong tương lai) hiểu rõ về thiết kế và chức năng của API.
  - Là một "single source of truth" về API contract.
- **Testing và Debugging:**
  - Một số công cụ tài liệu API (như Swagger UI) cho phép bạn thử nghiệm trực tiếp các API endpoint từ trình duyệt, rất hữu ích cho việc testing và debugging.
- **Onboarding thành viên mới:** Giúp thành viên mới nhanh chóng nắm bắt được cách hệ thống API hoạt động.
- **Tự động hóa:** Từ một file đặc tả API (như OpenAPI), bạn có thể tự động sinh ra client SDKs, server stubs, tài liệu,...

Tóm lại, tài liệu API tốt giúp tăng năng suất, giảm lỗi, cải thiện sự hợp tác và làm cho API của bạn dễ sử dụng hơn.

---

**2. Giới thiệu OpenAPI Specification và Swagger**

- **OpenAPI Specification (OAS):**

  - Là một đặc tả (specification) tiêu chuẩn, độc lập ngôn ngữ, để mô tả, sản xuất, tiêu thụ và trực quan hóa RESTful web services.
  - Nó định nghĩa một cấu trúc (thường ở định dạng JSON hoặc YAML) để mô tả các API endpoints, tham số, kiểu dữ liệu request/response, cơ chế xác thực, thông tin metadata (phiên bản, mô tả API),...
  - Phiên bản hiện tại phổ biến là OpenAPI 3.x (OAS3).
  - Việc tuân theo một tiêu chuẩn giúp các công cụ khác nhau có thể hiểu và làm việc với API của bạn.

- **Swagger:**
  - Swagger là một bộ công cụ (suite of tools) mã nguồn mở được xây dựng xung quanh OpenAPI Specification.
  - Các công cụ Swagger chính:
    - **Swagger UI:** Tự động sinh ra một giao diện web tương tác đẹp mắt từ một file đặc tả OpenAPI. Người dùng có thể xem danh sách các API, chi tiết từng API, và thậm chí là "Try it out" (gửi request thử nghiệm) trực tiếp từ UI.
    - **Swagger Editor:** Một trình soạn thảo dựa trên trình duyệt để viết và chỉnh sửa file đặc tả OpenAPI (YAML/JSON).
    - **Swagger Codegen (OpenAPI Generator):** Một công cụ dòng lệnh có thể sinh ra client libraries (SDKs), server stubs, và tài liệu API từ một file đặc tả OpenAPI.

**Trong ngữ cảnh Spring Boot, chúng ta thường dùng các thư viện giúp tự động sinh ra file đặc tả OpenAPI từ code Java (annotations), và sau đó tích hợp Swagger UI để hiển thị tài liệu đó.**

---

**3. Tích hợp Springdoc-openapi vào dự án Spring Boot**

Trước đây, SpringFox là thư viện phổ biến cho việc này, nhưng nó đã không còn được bảo trì tích cực và có thể không tương thích tốt với các phiên bản Spring Boot mới.
**`springdoc-openapi`** là giải pháp hiện đại và được khuyến nghị, hỗ trợ tốt OpenAPI 3 và tích hợp mượt mà với Spring Boot 2.x và 3.x.

- **Thêm Dependency:**
  Mở file `pom.xml` và thêm dependency sau:

  ```xml
  <dependency>
      <groupId>org.springdoc</groupId>
      <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
      <version>2.3.0</version> <!-- Kiểm tra phiên bản mới nhất trên Maven Central -->
  </dependency>
  ```

  - `springdoc-openapi-starter-webmvc-ui`: Dependency này bao gồm cả phần core để sinh ra OpenAPI spec và phần UI (Swagger UI) cho các ứng dụng Spring WebMVC (như của chúng ta).
  - Nếu bạn chỉ cần sinh ra spec mà không cần UI, có thể dùng `springdoc-openapi-starter-webmvc-api`.

  **Quan trọng:** Sau khi sửa `pom.xml`, nhớ "Load Maven Changes".

- **Cách hoạt động:**
  Sau khi thêm dependency này và khởi động lại ứng dụng, `springdoc-openapi` sẽ tự động:
  1.  Quét các `@RestController` và các annotation mapping (`@GetMapping`, `@PostMapping`,...) trong ứng dụng của bạn.
  2.  Sinh ra file đặc tả OpenAPI 3 dưới dạng JSON. Theo mặc định, file này có thể được truy cập tại `/v3/api-docs`.
  3.  Tích hợp Swagger UI. Theo mặc định, Swagger UI có thể được truy cập tại `/swagger-ui.html`.

---

**4. Cấu hình cơ bản và tùy chỉnh thông tin API (Info, Servers, Security Schemes)**

Bạn có thể tùy chỉnh các thông tin chung của API (như tiêu đề, mô tả, phiên bản, thông tin liên hệ, license) bằng cách tạo một bean `@Bean` kiểu `OpenAPI`.

Tạo hoặc sửa file `SecurityConfig.java` (hoặc một class `@Configuration` khác) để thêm bean này:

```java
// Trong package config, ví dụ: OpenApiConfig.java hoặc thêm vào SecurityConfig.java

package com.mycompany.ecommerceproject.config;

import io.swagger.v3.oas.models.Components;
import io.swagger.v3.oas.models.OpenAPI;
import io.swagger.v3.oas.models.info.Contact;
import io.swagger.v3.oas.models.info.Info;
import io.swagger.v3.oas.models.info.License;
import io.swagger.v3.oas.models.security.SecurityRequirement;
import io.swagger.v3.oas.models.security.SecurityScheme;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class OpenApiConfig { // Tạo class riêng hoặc thêm vào SecurityConfig

    @Bean
    public OpenAPI customOpenAPI() {
        final String securitySchemeName = "bearerAuth"; // Hoặc "basicAuth" nếu dùng Basic

        return new OpenAPI()
                .info(new Info()
                        .title("E-commerce Project API")
                        .version("v1.0.0")
                        .description("REST API documentation for the E-commerce project.")
                        .termsOfService("http://swagger.io/terms/") // URL điều khoản dịch vụ (nếu có)
                        .contact(new Contact()
                                .name("Your Name/Company")
                                .url("https://your-website.com")
                                .email("your-email@example.com"))
                        .license(new License()
                                .name("Apache 2.0")
                                .url("http://springdoc.org")))
                // Cấu hình Servers (nếu API của bạn được deploy ở nhiều môi trường)
                // .addServersItem(new Server().url("http://localhost:8080").description("Development server"))
                // .addServersItem(new Server().url("https://api.yourdomain.com").description("Production server"))

                // Cấu hình Security Schemes (quan trọng để test API có bảo mật từ Swagger UI)
                .addSecurityItem(new SecurityRequirement().addList(securitySchemeName)) // Yêu cầu scheme này cho các API
                .components(new Components()
                        // Định nghĩa Basic Auth Scheme
                        .addSecuritySchemes("basicAuth", new SecurityScheme()
                                .type(SecurityScheme.Type.HTTP)
                                .scheme("basic")
                                .description("Basic Authentication with username and password"))
                        // Định nghĩa Bearer Auth Scheme (cho JWT - sẽ dùng ở Phần 13)
                        .addSecuritySchemes("bearerAuth", new SecurityScheme()
                                .type(SecurityScheme.Type.HTTP)
                                .scheme("bearer")
                                .bearerFormat("JWT")
                                .description("JWT Authentication with Bearer token"))
                );
    }
}
```

- `Info`: Chứa các thông tin chung về API.
- `Servers`: (Tùy chọn) Liệt kê các server mà API của bạn được host.
- `SecurityRequirement` và `SecurityScheme`: Rất quan trọng.
  - `addSecuritySchemes`: Định nghĩa các cơ chế bảo mật mà API của bạn sử dụng (ví dụ: "basicAuth" cho Basic Authentication, "bearerAuth" cho JWT Bearer token).
  - `addSecurityItem`: Áp dụng một security scheme (đã định nghĩa trong `components.securitySchemes`) cho tất cả các API một cách mặc định. Bạn cũng có thể áp dụng security cho từng API cụ thể bằng annotation `@Operation(security = ...)` .
  - Khi bạn định nghĩa `SecurityScheme`, Swagger UI sẽ hiển thị nút "Authorize" cho phép người dùng nhập credentials (username/password cho Basic Auth, token cho Bearer Auth) để có thể gửi request đến các API được bảo vệ.

**Để Basic Auth hoạt động với Swagger UI từ `OpenApiConfig` trên:**

1.  Trong `customOpenAPI()`, đổi `securitySchemeName` thành `"basicAuth"`.
2.  Trong `SecurityConfig.java`, đảm bảo `.httpBasic(Customizer.withDefaults())` được bật và CSRF được xử lý đúng cách (có thể cần tắt cho API path nếu client là Swagger UI không quản lý CSRF token tốt).

---

**5. Sử dụng Annotations của Springdoc-openapi để mô tả API Endpoints, DTOs**

`springdoc-openapi` cung cấp các annotation (từ package `io.swagger.v3.oas.annotations.*`) để bạn làm phong phú thêm tài liệu API được sinh ra.

- `@Tag(name = "...", description = "...")`: Nhóm các API vào các "tag" (thường tương ứng với một controller).
- `@Operation(summary = "...", description = "...", tags = {"..."}, security = {@SecurityRequirement(name = "...")})`: Mô tả một API endpoint (một method trong controller).
  - `summary`: Mô tả ngắn gọn.
  - `description`: Mô tả chi tiết hơn (có thể dùng Markdown).
  - `security`: Chỉ định security scheme cần thiết cho endpoint này.
- `@Parameter(description = "...", required = true, example = "...")`: Mô tả một tham số của API (path variable, query parameter, header).
- `@RequestBody(description = "...", required = true, content = @Content(schema = @Schema(implementation = YourDto.class)))`: Mô tả request body.
- `@ApiResponse(responseCode = "200", description = "...", content = @Content(schema = @Schema(implementation = YourDto.class)))`: Mô tả một HTTP response có thể có.
  - `@ApiResponses({@ApiResponse(...), @ApiResponse(...)})`: Dùng để định nghĩa nhiều response codes.
- `@Schema(description = "...", example = "...", requiredProperties = {"name", "price"})`: Mô tả một DTO hoặc một trường trong DTO.
  - Có thể đặt trên class DTO hoặc trên các trường của DTO.

**Ví dụ cập nhật `ProductRestController`:**

```java
package com.mycompany.ecommerceproject.controller.api;

import com.mycompany.ecommerceproject.dto.ProductDto;
// ... (các import khác) ...
import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.Parameter;
import io.swagger.v3.oas.annotations.media.Content;
import io.swagger.v3.oas.annotations.media.Schema;
import io.swagger.v3.oas.annotations.responses.ApiResponse;
import io.swagger.v3.oas.annotations.responses.ApiResponses;
import io.swagger.v3.oas.annotations.security.SecurityRequirement;
import io.swagger.v3.oas.annotations.tags.Tag; // Import Tag
import org.springframework.http.MediaType; // Import MediaType

// ...

@RestController
@RequestMapping("/api/v1/products")
@Tag(name = "Product API", description = "APIs for managing products") // Tag cho cả controller
public class ProductRestController {

    // ... (dependencies và constructor) ...

    @Operation(summary = "Get all products", description = "Retrieves a paginated list of products.")
    @ApiResponses(value = {
            @ApiResponse(responseCode = "200", description = "Successfully retrieved list",
                         content = @Content(mediaType = MediaType.APPLICATION_JSON_VALUE,
                                            schema = @Schema(implementation = Page.class))) // Page<ProductDto>
    })
    @GetMapping
    public ResponseEntity<Page<ProductDto>> getAllProducts(
            @Parameter(description = "Page number (0-indexed)", example = "0") @RequestParam(name = "page", defaultValue = "0") int page,
            @Parameter(description = "Number of items per page", example = "10") @RequestParam(name = "size", defaultValue = "10") int size,
            @Parameter(description = "Sort criteria (field,direction e.g., name,asc or price,desc)", example = "name,asc")
            @RequestParam(name = "sort", defaultValue = "name,asc") String[] sort) {
        // ... (implementation)
        logger.info("API request to list products - page: {}, size: {}, sort: {}", page, size, sort);
        try {
            String sortField = sort[0];
            Sort.Direction sortDirection = (sort.length > 1 && sort[1].equalsIgnoreCase("desc")) ?
                                           Sort.Direction.DESC : Sort.Direction.ASC;
            Pageable pageable = PageRequest.of(page, size, Sort.by(sortDirection, sortField));
            Page<ProductDto> productsPage = productService.findAllProductDtos(pageable);
            return ResponseEntity.ok(productsPage);
        } catch (Exception e) {
            logger.error("Error fetching products via API", e);
            throw new ResponseStatusException(HttpStatus.INTERNAL_SERVER_ERROR, "Error fetching products", e);
        }
    }

    @Operation(summary = "Get a product by its ID")
    @ApiResponses(value = {
            @ApiResponse(responseCode = "200", description = "Product found",
                         content = @Content(mediaType = MediaType.APPLICATION_JSON_VALUE,
                                            schema = @Schema(implementation = ProductDto.class))),
            @ApiResponse(responseCode = "404", description = "Product not found", content = @Content)
    })
    @GetMapping("/{id}")
    public ResponseEntity<ProductDto> getProductById(
            @Parameter(description = "ID of the product to be retrieved", required = true, example = "1") @PathVariable Long id) {
        // ... (implementation)
        logger.info("API request to get product by id: {}", id);
        return productService.findProductDtoById(id)
                .map(ResponseEntity::ok)
                .orElseThrow(() -> new ResourceNotFoundException("Product not found with id: " + id));
    }

    @Operation(summary = "Create a new product",
               description = "Creates a new product. Requires ADMIN role.",
               security = @SecurityRequirement(name = "basicAuth")) // Hoặc "bearerAuth" nếu dùng JWT
    @ApiResponses(value = {
            @ApiResponse(responseCode = "201", description = "Product created successfully",
                         content = @Content(mediaType = MediaType.APPLICATION_JSON_VALUE,
                                            schema = @Schema(implementation = ProductDto.class))),
            @ApiResponse(responseCode = "400", description = "Invalid input data", content = @Content),
            @ApiResponse(responseCode = "401", description = "Unauthorized", content = @Content),
            @ApiResponse(responseCode = "403", description = "Forbidden (User does not have ADMIN role)", content = @Content)
    })
    @PostMapping
    @PreAuthorize("hasRole('ADMIN')")
    public ResponseEntity<?> createProduct(
            @Parameter(description = "Product object to be created", required = true,
                       content = @Content(schema = @Schema(implementation = ProductDto.class)))
            @Valid @RequestBody ProductDto productDto, BindingResult bindingResult, UriComponentsBuilder ucb) {
        // ... (implementation)
         logger.info("API request to create product: {}", productDto.getName());
        if (bindingResult.hasErrors()) {
            Map<String, String> errors = bindingResult.getFieldErrors().stream()
                    .collect(Collectors.toMap(
                            fieldError -> fieldError.getField(),
                            fieldError -> fieldError.getDefaultMessage() == null ? "Invalid value" : fieldError.getDefaultMessage()
                    ));
            return ResponseEntity.badRequest().body(errors);
        }
        try {
            ProductDto createdProduct = productService.createProduct(productDto);
            URI location = ucb.path("/api/v1/products/{id}")
                              .buildAndExpand(createdProduct.getId())
                              .toUri();
            return ResponseEntity.created(location).body(createdProduct);
        } catch (Exception e) {
            logger.error("Error creating product via API: {}", e.getMessage());
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("Error creating product: " + e.getMessage());
        }
    }

    @Operation(summary = "Update an existing product",
               description = "Updates an existing product by ID. Requires ADMIN role.",
               security = @SecurityRequirement(name = "basicAuth"))
    @ApiResponses(value = {
        @ApiResponse(responseCode = "200", description = "Product updated successfully",
                     content = @Content(mediaType = MediaType.APPLICATION_JSON_VALUE,
                                        schema = @Schema(implementation = ProductDto.class))),
        @ApiResponse(responseCode = "400", description = "Invalid input data", content = @Content),
        @ApiResponse(responseCode = "401", description = "Unauthorized", content = @Content),
        @ApiResponse(responseCode = "403", description = "Forbidden", content = @Content),
        @ApiResponse(responseCode = "404", description = "Product not found", content = @Content)
    })
    @PutMapping("/{id}")
    @PreAuthorize("hasRole('ADMIN')")
    public ResponseEntity<?> updateProduct(
            @Parameter(description = "ID of the product to be updated", required = true, example = "1") @PathVariable Long id,
            @Parameter(description = "Updated product object", required = true,
                       content = @Content(schema = @Schema(implementation = ProductDto.class)))
            @Valid @RequestBody ProductDto productDto, BindingResult bindingResult) {
        // ... (implementation)
        logger.info("API request to update product with id: {}", id);
         if (bindingResult.hasErrors()) {
            Map<String, String> errors = bindingResult.getFieldErrors().stream()
                    .collect(Collectors.toMap(fieldError -> fieldError.getField(), fieldError -> fieldError.getDefaultMessage()));
            return ResponseEntity.badRequest().body(errors);
        }
        try {
            ProductDto updatedProduct = productService.updateProduct(id, productDto);
            return ResponseEntity.ok(updatedProduct);
        } catch (ResourceNotFoundException ex) {
            throw ex;
        } catch (Exception e) {
            logger.error("Error updating product via API for id {}: {}", id, e.getMessage());
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("Error updating product: " + e.getMessage());
        }
    }


    @Operation(summary = "Delete a product by its ID",
               description = "Deletes a product by ID. Requires ADMIN role.",
               security = @SecurityRequirement(name = "basicAuth"))
    @ApiResponses(value = {
        @ApiResponse(responseCode = "204", description = "Product deleted successfully", content = @Content),
        @ApiResponse(responseCode = "401", description = "Unauthorized", content = @Content),
        @ApiResponse(response_code = "403", description = "Forbidden", content = @Content),
        @ApiResponse(responseCode = "404", description = "Product not found", content = @Content)
    })
    @DeleteMapping("/{id}")
    @PreAuthorize("hasRole('ADMIN')")
    public ResponseEntity<Void> deleteProduct(
            @Parameter(description = "ID of the product to be deleted", required = true, example = "1") @PathVariable Long id) {
        // ... (implementation)
        logger.info("API request to delete product with id: {}", id);
        try {
            productService.findProductDtoById(id)
                .orElseThrow(() -> new ResourceNotFoundException("Product not found with id: " + id + " for deletion."));
            productService.deleteById(id);
            return ResponseEntity.noContent().build();
        } catch (ResourceNotFoundException ex) {
            throw ex;
        } catch (Exception e) {
            logger.error("Error deleting product via API for id {}: {}", id, e.getMessage());
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).build();
        }
    }
}
```

**Cập nhật DTO `ProductDto` với `@Schema` (tùy chọn, nhưng tốt cho tài liệu):**

```java
// ProductDto.java
package com.mycompany.ecommerceproject.dto;

import io.swagger.v3.oas.annotations.media.Schema; // Import Schema
// ... (các import khác) ...

@Data
@NoArgsConstructor
@AllArgsConstructor
@Schema(description = "Data Transfer Object for Product information") // Mô tả cho cả DTO
public class ProductDto {
    @Schema(description = "Unique identifier of the Product.", example = "1", accessMode = Schema.AccessMode.READ_ONLY)
    private Long id;

    @NotEmpty(message = "Product name cannot be empty")
    @Size(min = 3, max = 200, message = "Product name must be between 3 and 200 characters")
    @Schema(description = "Name of the product.", example = "Laptop Pro X", requiredMode = Schema.RequiredMode.REQUIRED)
    private String name;

    @Schema(description = "Detailed description of the product.", example = "A powerful laptop for professionals.")
    private String description;

    @NotNull(message = "Price cannot be null")
    @DecimalMin(value = "0.01", message = "Price must be greater than 0")
    @Schema(description = "Price of the product.", example = "1299.99", requiredMode = Schema.RequiredMode.REQUIRED)
    private BigDecimal price;

    @Schema(description = "URL of the product image.", example = "https://example.com/images/laptop.jpg")
    private String imageUrl;

    @NotNull(message = "Stock quantity cannot be null")
    @Min(value = 0, message = "Stock quantity cannot be negative")
    @Schema(description = "Available stock quantity.", example = "50", requiredMode = Schema.RequiredMode.REQUIRED)
    private Integer stockQuantity;

    @NotNull(message = "Category ID cannot be null for product creation/update")
    @Schema(description = "ID of the category this product belongs to.", example = "101", requiredMode = Schema.RequiredMode.REQUIRED)
    private Long categoryId;

    @Schema(description = "Name of the category this product belongs to.", example = "Electronics", accessMode = Schema.AccessMode.READ_ONLY)
    private String categoryName;
}
```

- `accessMode = Schema.AccessMode.READ_ONLY`: Chỉ ra rằng trường này chỉ dùng để đọc (ví dụ: `id`, `categoryName` thường được server sinh ra hoặc lấy từ DB, client không gửi lên khi tạo).
- `requiredMode = Schema.RequiredMode.REQUIRED`: Đánh dấu trường này là bắt buộc trong request/response schema (khác với validation `@NotNull` là kiểm tra lúc runtime).

---

**6. Truy cập Swagger UI để xem và tương tác với API Documentation**

1.  **Khởi động lại ứng dụng Spring Boot của bạn.**
2.  Mở trình duyệt và truy cập: `http://localhost:8080/swagger-ui.html` (hoặc port bạn đang dùng).
3.  Bạn sẽ thấy giao diện Swagger UI:
    - Thông tin chung về API (từ `OpenApiConfig`).
    - Danh sách các "Tag" (ví dụ: "Product API").
    - Click vào một tag để mở rộng các API endpoints thuộc tag đó.
    - Mỗi endpoint sẽ có mô tả (từ `@Operation`), tham số (`@Parameter`), các response có thể có (`@ApiResponse`).
    - Schema của DTOs (`ProductDto`) cũng sẽ được hiển thị.
    - **Nút "Authorize":** Click vào đây. Nếu bạn đã cấu hình `SecurityScheme` (ví dụ "basicAuth"), bạn sẽ thấy một dialog để nhập username/password (ví dụ: `admin`/`adminpass`). Sau khi authorize, Swagger UI sẽ tự động gửi header `Authorization` cho các request đến API được bảo vệ.
    - **Nút "Try it out":** Cho phép bạn điền các tham số, request body (nếu có) và gửi request thử nghiệm đến API. Bạn sẽ thấy response body, headers, và status code.

![Swagger UI Example](https://i.imgur.com/gYgXk2G.png) (Hình ảnh minh họa giao diện Swagger UI)

---

**7. Tạo tài liệu cho các `ProductRestController` endpoints (Đã làm ở bước 5)**

Chúng ta đã thêm các annotation `@Tag`, `@Operation`, `@Parameter`, `@ApiResponse`, `@Schema` vào `ProductRestController` và `ProductDto` để làm phong phú tài liệu.

---

**8. Lưu ý và Best Practices khi viết tài liệu API**

- **Luôn giữ tài liệu cập nhật:** Tài liệu API lỗi thời còn tệ hơn là không có tài liệu. Việc sinh tài liệu từ code (như `springdoc-openapi`) giúp giảm thiểu vấn đề này.
- **Rõ ràng và Ngắn gọn:** Mô tả phải dễ hiểu.
- **Cung cấp ví dụ (Examples):** Rất hữu ích cho người dùng API. `@Schema(example = "...")`, `@Parameter(example = "...")`.
- **Mô tả tất cả các Response Codes:** Bao gồm cả các mã lỗi phổ biến (400, 401, 403, 404, 500).
- **Mô tả chi tiết các trường trong DTOs:** Ý nghĩa, ràng buộc, định dạng.
- **Phiên bản hóa API (Versioning):** Nếu API của bạn thay đổi (breaking changes), hãy có chiến lược phiên bản hóa (ví dụ: `/api/v1/products`, `/api/v2/products`). Tài liệu cũng cần phản ánh phiên bản.
- **Bảo mật:** Chỉ rõ các yêu cầu bảo mật cho từng endpoint.
- **Rate Limiting:** Nếu có, hãy đề cập đến trong tài liệu.
- **Sử dụng Markdown:** Trong các trường `description` để định dạng văn bản tốt hơn.
- **Kiểm tra tài liệu được sinh ra:** Đảm bảo nó chính xác và đầy đủ.

---

**Kết thúc Phần 11**

Chúng ta đã tìm hiểu cách tài liệu hóa REST API của mình một cách chuyên nghiệp bằng OpenAPI và Swagger (thông qua `springdoc-openapi`):

- Hiểu tầm quan trọng của việc tài liệu hóa API.
- Tích hợp `springdoc-openapi`.
- Tùy chỉnh thông tin API chung.
- Sử dụng các annotation để mô tả chi tiết API endpoints và DTOs.
- Sử dụng Swagger UI để xem và tương tác với tài liệu.

Việc này giúp API của bạn dễ tiếp cận, dễ sử dụng và dễ bảo trì hơn rất nhiều.

**Trong Phần 12, chúng ta sẽ khám phá các kỹ thuật nâng cao hơn cho REST API: "Advanced REST API Techniques: Versioning, HATEOAS, Custom Error Responses."**

Tuyệt vời! Sau khi đã có tài liệu API rõ ràng, chúng ta sẽ tiếp tục nâng cao chất lượng và tính chuyên nghiệp của REST API bằng cách tìm hiểu về các kỹ thuật như phiên bản hóa, HATEOAS và tùy chỉnh cách trả về lỗi.

---

**Phần 12: Advanced REST API Techniques: Versioning, HATEOAS, Custom Error Responses**

Trong phần này, chúng ta sẽ:

1.  **API Versioning (Phiên bản hóa API):**
    - Tại sao cần phiên bản hóa?
    - Các chiến lược phiên bản hóa phổ biến (URI Path, Query Parameter, Custom Header, Accept Header).
    - Triển khai phiên bản hóa qua URI Path (ví dụ: `/api/v1/products`, `/api/v2/products`).
2.  **HATEOAS (Hypermedia as the Engine of Application State):**
    - Khái niệm và lợi ích của HATEOAS.
    - Giới thiệu Spring HATEOAS.
    - Thêm các liên kết (links) vào response của API để client có thể khám phá các hành động/tài nguyên liên quan.
    - Sử dụng `EntityModel`, `CollectionModel`, `RepresentationModelAssembler`.
3.  **Custom Error Responses for APIs:**
    - Tại sao cần response lỗi tùy chỉnh (thay vì response lỗi mặc định của Spring Boot hoặc HTML error pages).
    - Thiết kế một cấu trúc JSON chuẩn cho response lỗi.
    - Cập nhật `GlobalExceptionHandler` để trả về JSON error response cho các API request.
4.  **Thực hành áp dụng các kỹ thuật này vào `ProductRestController`.**

---

**1. API Versioning (Phiên bản hóa API)**

- **Tại sao cần phiên bản hóa?**
  Khi API của bạn phát triển, sẽ có những lúc bạn cần thay đổi cấu trúc (breaking changes) như:

  - Thay đổi định dạng của request/response body (thêm/bỏ/đổi tên trường).
  - Thay đổi URL của endpoint.
  - Thay đổi hành vi của endpoint.
    Những thay đổi này có thể làm hỏng các client đang sử dụng phiên bản API cũ. Phiên bản hóa cho phép bạn giới thiệu các thay đổi mới mà không làm ảnh hưởng đến các client hiện tại, cho họ thời gian để nâng cấp lên phiên bản mới.

- **Các chiến lược phiên bản hóa phổ biến:**

  1.  **URI Path Versioning (Phiên bản hóa qua đường dẫn URI):**

      - **Cách làm:** Thêm số phiên bản trực tiếp vào URI.
      - **Ví dụ:** `/api/v1/products`, `/api/v2/users`
      - **Ưu điểm:** Rất rõ ràng, dễ hiểu, dễ cache bởi các HTTP proxy.
      - **Nhược điểm:** Làm URI "bẩn" hơn một chút. Khi có nhiều phiên bản, có thể dẫn đến nhiều controller hoặc logic điều hướng phức tạp.
      - **Đây là cách phổ biến và dễ triển khai nhất.**

  2.  **Query Parameter Versioning (Phiên bản hóa qua tham số Query):**

      - **Cách làm:** Thêm tham số query để chỉ định phiên bản.
      - **Ví dụ:** `/api/products?version=1`, `/api/users?api-version=2.1`
      - **Ưu điểm:** URI gốc không thay đổi.
      - **Nhược điểm:** Ít rõ ràng hơn URI path. Việc caching có thể phức tạp hơn một chút.

  3.  **Custom Header Versioning (Phiên bản hóa qua Header tùy chỉnh):**

      - **Cách làm:** Client gửi một header HTTP tùy chỉnh để chỉ định phiên bản.
      - **Ví dụ:** `X-API-VERSION: 1` hoặc `API-Version: 2.0`
      - **Ưu điểm:** URI gốc hoàn toàn sạch sẽ.
      - **Nhược điểm:** Client phải biết và gửi header này. Ít trực quan khi gõ URL trên trình duyệt.

  4.  **Accept Header Versioning (Content Negotiation / Media Type Versioning):**
      - **Cách làm:** Sử dụng header `Accept` với một media type tùy chỉnh có chứa thông tin phiên bản.
      - **Ví dụ:** `Accept: application/vnd.mycompany.v1+json` hoặc `Accept: application/json; version=2.0`
      - **Ưu điểm:** Được coi là cách "RESTful" nhất vì nó sử dụng cơ chế content negotiation của HTTP. URI sạch.
      - **Nhược điểm:** Phức tạp nhất để triển khai và cho client sử dụng. Việc caching có thể cần cấu hình đặc biệt.

- **Triển khai phiên bản hóa qua URI Path:**
  Đây là cách đơn giản nhất để bắt đầu. Chúng ta đã ngầm sử dụng nó khi đặt base path là `/api/v1/products`.

  - Khi bạn muốn có `v2`, bạn có thể tạo một controller mới (ví dụ `ProductRestControllerV2`) với `@RequestMapping("/api/v2/products")` hoặc thêm các method mới vào controller hiện tại với path versioned.
  - Hoặc, bạn có thể có một controller cha và các controller con cho từng phiên bản.

  **Ví dụ cấu trúc với package:**

  ```
  com.mycompany.ecommerceproject.controller.api
  ├── v1
  │   └── ProductRestController.java  (@RequestMapping("/api/v1/products"))
  └── v2
      └── ProductRestController.java  (@RequestMapping("/api/v2/products"))
  ```

  **Hoặc trong cùng controller (ít khuyến khích nếu thay đổi lớn):**

  ```java
  // ProductRestController.java
  @GetMapping("/v1/products")
  public ResponseEntity<Page<ProductDtoV1>> getAllProductsV1(...) { /* ... */ }

  @GetMapping("/v2/products")
  public ResponseEntity<Page<ProductDtoV2>> getAllProductsV2(...) { /* ... */ }
  ```

  **Lưu ý:** Khi có nhiều phiên bản, bạn cũng có thể cần các DTO phiên bản khác nhau (ví dụ: `ProductDtoV1`, `ProductDtoV2`) và logic service tương ứng.

  **Hiện tại, chúng ta đã có `/api/v1/products`. Nếu bạn muốn tạo `v2`, bạn sẽ lặp lại các bước tương tự nhưng với base path `/api/v2` và có thể là DTOs/logic khác.**

---

**2. HATEOAS (Hypermedia as the Engine of Application State)**

- **Khái niệm:**
  - HATEOAS là một ràng buộc của kiến trúc REST. Nó nói rằng client nên tương tác với ứng dụng hoàn toàn thông qua các hypermedia (liên kết) được cung cấp động bởi server trong các response.
  - Thay vì client phải "hard-code" các URI để thực hiện các hành động tiếp theo, server sẽ cung cấp các URI này trong response.
  - Ví dụ: Khi lấy chi tiết một sản phẩm, response có thể chứa link để "thêm vào giỏ hàng", "xem các sản phẩm liên quan", "cập nhật sản phẩm này (nếu có quyền)".
- **Lợi ích:**
  - **Giảm sự phụ thuộc (Decoupling):** Client không cần biết trước cấu trúc URI của server. Nếu URI thay đổi, client vẫn hoạt động được miễn là nó theo các link được cung cấp.
  - **Khả năng khám phá (Discoverability):** Client có thể "khám phá" các hành động và tài nguyên có sẵn từ các link trong response.
  - **API dễ phát triển hơn:** Server có thể thay đổi URI mà không làm hỏng client (về mặt lý thuyết).
- **Giới thiệu Spring HATEOAS:**

  - Là một dự án của Spring giúp dễ dàng implement HATEOAS trong các ứng dụng Spring MVC.
  - Nó cung cấp các class như `EntityModel`, `CollectionModel`, `Link` và các `RepresentationModelAssembler` để giúp bạn xây dựng response chứa các link.
  - Spring HATEOAS thường trả về response theo định dạng HAL (Hypertext Application Language) hoặc các định dạng hypermedia khác.

- **Thêm Dependency Spring HATEOAS:**
  Mở `pom.xml`:

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-hateoas</artifactId>
  </dependency>
  ```

  Nhớ "Load Maven Changes".

- **Sử dụng `EntityModel`, `CollectionModel`, `RepresentationModelAssembler`:**

  1.  **`EntityModel<T>`:** Bọc một đối tượng DTO/Entity và thêm các link vào đó.
  2.  **`CollectionModel<EntityModel<T>>`:** Bọc một collection các `EntityModel` và có thể thêm link cho cả collection.
  3.  **`RepresentationModelAssembler<T, D extends RepresentationModel<?>>`:** Một interface giúp chuyển đổi một đối tượng domain (`T`) thành một `RepresentationModel` (thường là `EntityModel<T>`). Rất hữu ích để tránh lặp code tạo link.

- **Thực hành với `ProductRestController`:**

  **Sửa `ProductDto` để kế thừa `RepresentationModel` (tùy chọn, nhưng tiện lợi):**

  ```java
  // ProductDto.java
  package com.mycompany.ecommerceproject.dto;

  import org.springframework.hateoas.RepresentationModel; // Import
  // ... (các import khác) ...

  @Data
  @NoArgsConstructor
  @AllArgsConstructor
  @Schema(description = "Data Transfer Object for Product information")
  public class ProductDto extends RepresentationModel<ProductDto> { // Kế thừa
      // ... (các trường như cũ) ...
  }
  ```

  **Cập nhật `ProductRestController`:**

  ```java
  package com.mycompany.ecommerceproject.controller.api;

  // ... (các import đã có) ...
  import org.springframework.hateoas.EntityModel;
  import org.springframework.hateoas.CollectionModel;
  import org.springframework.hateoas.Link;
  import org.springframework.hateoas.server.mvc.WebMvcLinkBuilder; // Quan trọng
  import static org.springframework.hateoas.server.mvc.WebMvcLinkBuilder.*; // Cho linkTo và methodOn

  // ...

  @RestController
  @RequestMapping("/api/v1/products")
  @Tag(name = "Product API", description = "APIs for managing products")
  public class ProductRestController {
      // ... (service, logger) ...

      @Operation(summary = "Get a product by its ID with HATEOAS links")
      @ApiResponses(value = {
              @ApiResponse(responseCode = "200", description = "Product found",
                           content = @Content(mediaType = MediaType.APPLICATION_JSON_VALUE, // Hoặc application/hal+json
                                              schema = @Schema(implementation = EntityModel.class))), // EntityModel<ProductDto>
              @ApiResponse(responseCode = "404", description = "Product not found", content = @Content)
      })
      @GetMapping("/{id}")
      public ResponseEntity<EntityModel<ProductDto>> getProductById(@PathVariable Long id) {
          logger.info("API HATEOAS request to get product by id: {}", id);
          ProductDto productDto = productService.findProductDtoById(id)
                  .orElseThrow(() -> new ResourceNotFoundException("Product not found with id: " + id));

          // Tạo EntityModel để bọc ProductDto và thêm links
          EntityModel<ProductDto> productModel = EntityModel.of(productDto);

          // Thêm link "self"
          productModel.add(linkTo(methodOn(ProductRestController.class).getProductById(id)).withSelfRel());

          // Thêm link "all-products"
          productModel.add(linkTo(methodOn(ProductRestController.class).getAllProducts(0, 10, new String[]{"name", "asc"})).withRel("all-products"));

          // (Tùy chọn) Thêm link "add-to-cart" nếu có API cho việc đó
          // Giả sử có CartRestController.addToCartApi(Long productId, int quantity)
          // productModel.add(linkTo(methodOn(CartRestController.class).addToCartApi(id, 1)).withRel("add-to-cart"));

          return ResponseEntity.ok(productModel);
      }


      @Operation(summary = "Get all products with HATEOAS links")
      @GetMapping("/list-hateoas") // Tạo endpoint riêng để không ảnh hưởng cái cũ
      public ResponseEntity<CollectionModel<EntityModel<ProductDto>>> getAllProductsWithHateoas(
              @RequestParam(name = "page", defaultValue = "0") int page,
              @RequestParam(name = "size", defaultValue = "10") int size,
              @RequestParam(name = "sort", defaultValue = "name,asc") String[] sort) {

          Pageable pageable = PageRequest.of(page, size, Sort.by(Sort.Direction.fromString(sort[1]), sort[0]));
          Page<ProductDto> productsPage = productService.findAllProductDtos(pageable);

          List<EntityModel<ProductDto>> productModels = productsPage.getContent().stream()
                  .map(dto -> EntityModel.of(dto,
                          linkTo(methodOn(ProductRestController.class).getProductById(dto.getId())).withSelfRel()))
                  .collect(Collectors.toList());

          CollectionModel<EntityModel<ProductDto>> collectionModel = CollectionModel.of(productModels,
                  linkTo(methodOn(ProductRestController.class).getAllProductsWithHateoas(page, size, sort)).withSelfRel());

          // Thêm link phân trang (next, prev, first, last) nếu cần
          if (productsPage.hasNext()) {
              collectionModel.add(linkTo(methodOn(ProductRestController.class).getAllProductsWithHateoas(page + 1, size, sort)).withRel("next"));
          }
          if (productsPage.hasPrevious()) {
              collectionModel.add(linkTo(methodOn(ProductRestController.class).getAllProductsWithHateoas(page - 1, size, sort)).withRel("prev"));
          }
          // ... first, last ...

          return ResponseEntity.ok(collectionModel);
      }
      // ... (các method POST, PUT, DELETE có thể chưa cần HATEOAS nhiều) ...
  }
  ```

  - `linkTo(methodOn(ControllerClass.class).methodName(params...)).withSelfRel()`: Cách tạo link an toàn, tự động lấy path từ controller method.
    - `withSelfRel()`: Tạo link với `rel="self"`.
    - `withRel("relation-name")`: Tạo link với tên quan hệ tùy chỉnh.
  - Khi bạn gọi API `/api/v1/products/{id}` (ví dụ `/api/v1/products/1`), response JSON sẽ có thêm một trường `_links`, ví dụ:
    ```json
    {
      "id": 1,
      "name": "Super Laptop Pro",
      // ... other fields ...
      "_links": {
        "self": {
          "href": "http://localhost:8080/api/v1/products/1"
        },
        "all-products": {
          "href": "http://localhost:8080/api/v1/products?page=0&size=10&sort=name,asc"
        }
      }
    }
    ```
  - **`RepresentationModelAssembler`:** Để tránh lặp code tạo link, bạn có thể tạo một assembler:

    ```java
    // ProductModelAssembler.java (trong package mapper hoặc assembler)
    // import org.springframework.hateoas.server.RepresentationModelAssembler;
    // import org.springframework.stereotype.Component;

    // @Component
    // public class ProductModelAssembler implements RepresentationModelAssembler<ProductDto, EntityModel<ProductDto>> {
    //     @Override
    //     public EntityModel<ProductDto> toModel(ProductDto productDto) {
    //         return EntityModel.of(productDto,
    //                 linkTo(methodOn(ProductRestController.class).getProductById(productDto.getId())).withSelfRel(),
    //                 linkTo(methodOn(ProductRestController.class).getAllProducts(0,10,new String[]{"name","asc"})).withRel("all-products")
    //         );
    //     }
    // }
    ```

    Sau đó inject `ProductModelAssembler` vào controller và dùng `assembler.toModel(productDto)`.

  Spring HATEOAS sẽ tự động thay đổi `Content-Type` của response thành `application/hal+json` (hoặc một media type hypermedia khác) nếu client yêu cầu qua header `Accept`.

---

**3. Custom Error Responses for APIs**

Khi API gặp lỗi (validation, resource not found, server error), việc trả về một cấu trúc JSON lỗi nhất quán và chứa thông tin hữu ích là rất quan trọng.

- **Tại sao cần response lỗi tùy chỉnh?**

  - Response lỗi HTML mặc định của Spring Boot (Whitelabel Error Page) không phù hợp cho API client.
  - Cần cung cấp thông tin lỗi rõ ràng cho client để họ có thể xử lý (ví dụ: mã lỗi nội bộ, thông điệp chi tiết, các trường bị lỗi validation).
  - Nhất quán trong cách báo lỗi giúp client dễ dàng parse và xử lý.

- **Thiết kế một cấu trúc JSON chuẩn cho response lỗi:**
  Một cấu trúc phổ biến có thể bao gồm:

  ```json
  {
    "timestamp": "2023-10-27T10:30:00.123Z",
    "status": 404, // HTTP Status Code
    "error": "Not Found", // HTTP Status Phrase
    "message": "Product not found with id: 999", // Thông điệp lỗi chi tiết
    "path": "/api/v1/products/999", // Path gây ra lỗi
    "details": {
      // (Tùy chọn) Chi tiết lỗi, ví dụ cho validation
      "fieldName1": "Error message for field 1",
      "fieldName2": "Error message for field 2"
    }
  }
  ```

- **Cập nhật `GlobalExceptionHandler` để trả về JSON error response cho API request:**
  Chúng ta sẽ sửa `GlobalExceptionHandler` đã tạo ở Phần 9. Cách tốt nhất là kiểm tra header `Accept` của request. Nếu client yêu cầu `application/json`, trả về JSON, ngược lại trả về trang HTML.

  ```java
  package com.mycompany.ecommerceproject.exception;

  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;
  import org.springframework.http.HttpHeaders;
  import org.springframework.http.HttpStatus;
  import org.springframework.http.MediaType;
  import org.springframework.http.ResponseEntity; // Import ResponseEntity
  import org.springframework.validation.FieldError;
  import org.springframework.web.bind.MethodArgumentNotValidException;
  import org.springframework.web.bind.annotation.ControllerAdvice;
  import org.springframework.web.bind.annotation.ExceptionHandler;
  // import org.springframework.web.bind.annotation.ResponseStatus; // Không cần @ResponseStatus nữa nếu dùng ResponseEntity
  import org.springframework.web.context.request.ServletWebRequest; // Để lấy path
  import org.springframework.web.context.request.WebRequest;
  import org.springframework.web.servlet.ModelAndView;

  import java.time.LocalDateTime;
  import java.util.HashMap;
  import java.util.LinkedHashMap;
  import java.util.Map;
  import java.util.stream.Collectors;

  @ControllerAdvice
  public class GlobalExceptionHandler {

      private static final Logger logger = LoggerFactory.getLogger(GlobalExceptionHandler.class);

      // Helper để tạo error DTO
      private Map<String, Object> createErrorBody(HttpStatus status, String message, String path, Map<String, String> details) {
          Map<String, Object> body = new LinkedHashMap<>();
          body.put("timestamp", LocalDateTime.now().toString());
          body.put("status", status.value());
          body.put("error", status.getReasonPhrase());
          body.put("message", message);
          body.put("path", path);
          if (details != null && !details.isEmpty()) {
              body.put("details", details);
          }
          return body;
      }

      private String getRequestPath(WebRequest request) {
          if (request instanceof ServletWebRequest) {
              return ((ServletWebRequest) request).getRequest().getRequestURI();
          }
          return "N/A";
      }

      private boolean prefersJson(WebRequest request) {
          String acceptHeader = request.getHeader(HttpHeaders.ACCEPT);
          return acceptHeader != null && acceptHeader.contains(MediaType.APPLICATION_JSON_VALUE);
      }

      @ExceptionHandler(ResourceNotFoundException.class)
      public ResponseEntity<Object> handleResourceNotFoundException(ResourceNotFoundException ex, WebRequest request) {
          logger.warn("Resource not found: {}", ex.getMessage());
          if (prefersJson(request)) {
              Map<String, Object> body = createErrorBody(HttpStatus.NOT_FOUND, ex.getMessage(), getRequestPath(request), null);
              return new ResponseEntity<>(body, HttpStatus.NOT_FOUND);
          } else {
              ModelAndView mav = new ModelAndView("error/404");
              mav.addObject("errorMessage", ex.getMessage());
              mav.addObject("exceptionType", ex.getClass().getSimpleName());
              // Trả về ResponseEntity để Spring không cố gắng tìm view resolver khác
              return new ResponseEntity<>(mav.getModelMap(), HttpStatus.NOT_FOUND); // Hoặc trả về mav trực tiếp nếu method là ModelAndView
          }
      }

      @ExceptionHandler(DuplicateResourceException.class)
      public ResponseEntity<Object> handleDuplicateResourceException(DuplicateResourceException ex, WebRequest request) {
          logger.warn("Duplicate resource: {}", ex.getMessage());
          if (prefersJson(request)) {
               Map<String, Object> body = createErrorBody(HttpStatus.CONFLICT, ex.getMessage(), getRequestPath(request), null);
              return new ResponseEntity<>(body, HttpStatus.CONFLICT);
          } else {
              ModelAndView mav = new ModelAndView("error/409");
              mav.addObject("errorMessage", ex.getMessage());
              // ... (tương tự)
              return new ResponseEntity<>(mav.getModelMap(), HttpStatus.CONFLICT);
          }
      }

      @ExceptionHandler(BadRequestException.class)
      public ResponseEntity<Object> handleBadRequestException(BadRequestException ex, WebRequest request) {
           logger.warn("Bad request: {}", ex.getMessage());
          if (prefersJson(request)) {
              Map<String, Object> body = createErrorBody(HttpStatus.BAD_REQUEST, ex.getMessage(), getRequestPath(request), null);
              return new ResponseEntity<>(body, HttpStatus.BAD_REQUEST);
          } else {
               ModelAndView mav = new ModelAndView("error/400");
              mav.addObject("errorMessage", ex.getMessage());
              // ...
              return new ResponseEntity<>(mav.getModelMap(), HttpStatus.BAD_REQUEST);
          }
      }


      @ExceptionHandler(MethodArgumentNotValidException.class)
      public ResponseEntity<Object> handleValidationExceptions(MethodArgumentNotValidException ex, WebRequest request) {
          logger.warn("Validation error: {}", ex.getBindingResult().getAllErrors().get(0).getDefaultMessage());
           Map<String, String> errors = ex.getBindingResult().getFieldErrors().stream()
              .collect(Collectors.toMap(FieldError::getField,
                                        fe -> fe.getDefaultMessage() == null ? "Invalid value" : fe.getDefaultMessage()));

          if (prefersJson(request)) {
              Map<String, Object> body = createErrorBody(HttpStatus.BAD_REQUEST, "Validation failed", getRequestPath(request), errors);
              return new ResponseEntity<>(body, HttpStatus.BAD_REQUEST);
          } else {
              // Đối với web UI, thường là redirect lại form hoặc hiển thị lỗi trên trang lỗi chung.
              // Nếu redirect lại form, controller method cần xử lý BindingResult.
              // Ở đây ta trả về trang lỗi validation chung.
              ModelAndView mav = new ModelAndView("error/validation_error"); // Hoặc một trang lỗi 400 chung
              mav.addObject("errorMessage", "Validation failed. Please check your input.");
              mav.addObject("errors", errors); // Truyền chi tiết lỗi ra view
              return new ResponseEntity<>(mav.getModelMap(), HttpStatus.BAD_REQUEST);
          }
      }

      @ExceptionHandler(Exception.class)
      public ResponseEntity<Object> handleGenericException(Exception ex, WebRequest request) {
          logger.error("An unexpected error occurred: ", ex);
          String message = "An unexpected error occurred. Please try again later.";
           if (prefersJson(request)) {
              Map<String, Object> body = createErrorBody(HttpStatus.INTERNAL_SERVER_ERROR, message, getRequestPath(request), null);
              return new ResponseEntity<>(body, HttpStatus.INTERNAL_SERVER_ERROR);
          } else {
              ModelAndView mav = new ModelAndView("error/500");
              mav.addObject("errorMessage", message);
              // ...
              return new ResponseEntity<>(mav.getModelMap(), HttpStatus.INTERNAL_SERVER_ERROR);
          }
      }
  }
  ```

  - Mỗi method `@ExceptionHandler` giờ đây trả về `ResponseEntity<Object>`.
  - `prefersJson(request)`: Kiểm tra header `Accept` để quyết định định dạng response.
  - `createErrorBody()`: Helper method để tạo cấu trúc JSON lỗi.
  - Đối với `MethodArgumentNotValidException`, chúng ta trích xuất các lỗi field và đưa vào `details`.
  - **Lưu ý:** Khi trả về `ModelAndView` trong `ResponseEntity`, bạn nên truyền `mav.getModelMap()` làm body và status code tương ứng, hoặc sửa kiểu trả về của method thành `ModelAndView` và Spring sẽ tự xử lý. Cách dùng `ResponseEntity` cho cả hai trường hợp giúp nhất quán hơn.

---

**4. Thực hành áp dụng các kỹ thuật này vào `ProductRestController`**

Chúng ta đã:

- **Versioning:** Sử dụng `/api/v1/products` làm base path.
- **HATEOAS:** Cập nhật `getProductById` và thêm `getAllProductsWithHateoas` trong `ProductRestController` để trả về response có links.
- **Custom Error Responses:** `GlobalExceptionHandler` đã được cập nhật để trả về JSON error response khi client yêu cầu `application/json`.

**Kiểm thử lại với Postman:**

1.  **Test HATEOAS:**
    - Gọi `GET http://localhost:8080/api/v1/products/1`. Đặt header `Accept: application/hal+json` (hoặc `application/json`). Bạn sẽ thấy trường `_links`.
    - Gọi `GET http://localhost:8080/api/v1/products/list-hateoas`.
2.  **Test Custom Error Responses:**
    - Gọi `GET http://localhost:8080/api/v1/products/9999` (ID không tồn tại). Đặt header `Accept: application/json`. Bạn sẽ thấy JSON error response 404.
    - Thử `POST /api/v1/products` với body JSON không hợp lệ (ví dụ: `name` trống, `price` âm). Đặt header `Accept: application/json`. Bạn sẽ thấy JSON error 400 với chi tiết lỗi validation.
    - Thử `POST /api/v1/products` mà không có Basic Auth (admin). Bạn sẽ thấy lỗi 401 hoặc 403 (tùy vào cấu hình).

---

**Kết thúc Phần 12**

Chúng ta đã khám phá các kỹ thuật nâng cao giúp REST API trở nên mạnh mẽ và chuyên nghiệp hơn:

- Hiểu và biết cách tiếp cận **API Versioning**.
- Triển khai **HATEOAS** với Spring HATEOAS để tăng tính khám phá cho API.
- Tùy chỉnh **Error Responses** để cung cấp thông tin lỗi hữu ích cho API client dưới dạng JSON.

Những kỹ thuật này giúp API của bạn dễ tích hợp, dễ mở rộng và dễ bảo trì hơn.

**Trong Phần 13, chúng ta sẽ tập trung vào một khía cạnh bảo mật rất quan trọng cho stateless REST APIs: "Securing REST APIs với JWT (JSON Web Tokens)."**

Tuyệt vời! Sau khi đã làm cho API của chúng ta "thông minh" hơn với versioning, HATEOAS và custom error responses, bước tiếp theo là củng cố bảo mật cho chúng, đặc biệt là khi hướng tới các kiến trúc stateless. JWT là một lựa chọn phổ biến và hiệu quả cho việc này.

---

**Phần 13: Securing REST APIs với JWT (JSON Web Tokens)**

Trong phần này, chúng ta sẽ:

1.  **Giới thiệu JWT (JSON Web Tokens):**
    - JWT là gì? Cấu trúc của JWT (Header, Payload, Signature).
    - Ưu điểm và nhược điểm của JWT.
    - Luồng hoạt động của xác thực dựa trên JWT.
2.  **Thêm Dependencies cần thiết cho JWT.** (ví dụ: `jjwt-api`, `jjwt-impl`, `jjwt-jackson`).
3.  **Tạo các Utility Classes cho JWT:**
    - `JwtUtil` (hoặc `JwtTokenProvider`): Class để tạo (generate), phân giải (parse), và xác thực (validate) JWT.
    - Lưu trữ secret key một cách an toàn (trong `application.properties` hoặc biến môi trường).
4.  **Xây dựng Endpoint Xác thực (`/api/v1/auth/login`) để cấp JWT:**
    - Controller nhận username/password.
    - Sử dụng `AuthenticationManager` của Spring Security để xác thực.
    - Nếu xác thực thành công, tạo JWT và trả về cho client.
5.  **Tạo một `JwtAuthenticationFilter`:**
    - Filter này sẽ chặn các request đến API được bảo vệ.
    - Trích xuất JWT từ header `Authorization: Bearer <token>`.
    - Validate JWT.
    - Nếu JWT hợp lệ, tạo đối tượng `Authentication` và đặt vào `SecurityContextHolder`.
6.  **Cập nhật `SecurityConfig`:**
    - Vô hiệu hóa session creation (`STATELESS`).
    - Thêm `JwtAuthenticationFilter` vào chuỗi filter của Spring Security (trước `UsernamePasswordAuthenticationFilter` hoặc filter tương ứng).
    - Cấu hình các API endpoint nào yêu cầu JWT.
7.  **Kiểm thử luồng xác thực JWT với Postman:**
    - Gọi API login để lấy token.
    - Sử dụng token đó để gọi các API được bảo vệ.
8.  **Lưu ý về Refresh Tokens (Tùy chọn nâng cao).**
9.  **Best Practices khi sử dụng JWT.**

---

**1. Giới thiệu JWT (JSON Web Tokens)**

- **JWT là gì?**
  - JWT (phát âm là "jot") là một tiêu chuẩn mở (RFC 7519) định nghĩa một cách nhỏ gọn và tự chứa (self-contained) để truyền thông tin giữa các bên một cách an toàn dưới dạng một đối tượng JSON.
  - Thông tin này có thể được xác minh và tin cậy vì nó được ký điện tử (digitally signed).
  - JWT thường được sử dụng cho:
    - **Authorization:** Sau khi người dùng đăng nhập, server sẽ tạo một JWT và gửi lại cho client. Client sẽ gửi kèm JWT này trong mỗi request tiếp theo đến các tài nguyên được bảo vệ.
    - **Information Exchange:** Truyền thông tin an toàn giữa các bên.
- **Cấu trúc của JWT:**
  Một JWT bao gồm ba phần, được ngăn cách bởi dấu chấm (`.`): `Header.Payload.Signature`

  1.  **Header (Tiêu đề):**
      - Thường bao gồm hai phần: loại token (`typ`, thường là "JWT") và thuật toán ký được sử dụng (`alg`, ví dụ: HMAC SHA256 hoặc RSA).
      - Ví dụ: `{"alg": "HS256", "typ": "JWT"}`
      - Header này được mã hóa Base64Url để tạo phần đầu tiên của JWT.
  2.  **Payload (Tải trọng):**
      - Chứa các "claims" (tuyên bố). Claims là các phát biểu về một thực thể (thường là người dùng) và dữ liệu bổ sung.
      - Có ba loại claims:
        - **Registered claims:** Các claim được định nghĩa trước, không bắt buộc nhưng được khuyến nghị để cung cấp một tập hợp các claim hữu ích, có thể tương tác được. Ví dụ: `iss` (issuer), `exp` (expiration time), `sub` (subject), `aud` (audience).
        - **Public claims:** Các claim có thể được định nghĩa theo ý muốn của người sử dụng JWT. Để tránh xung đột, chúng nên được định nghĩa trong IANA JSON Web Token Registry hoặc được đặt tên với URI có khả năng chống xung đột.
        - **Private claims:** Các claim tùy chỉnh được tạo ra để chia sẻ thông tin giữa các bên đã đồng ý sử dụng chúng và không phải là registered hay public claims.
      - Ví dụ: `{"sub": "user123", "name": "John Doe", "admin": true, "exp": 1678886400}`
      - Payload này cũng được mã hóa Base64Url để tạo phần thứ hai của JWT.
  3.  **Signature (Chữ ký):**
      - Để tạo phần chữ ký, bạn phải lấy header đã mã hóa, payload đã mã hóa, một secret (nếu dùng thuật toán HMAC) hoặc một cặp private/public key (nếu dùng RSA hoặc ECDSA), và ký chúng bằng thuật toán đã chỉ định trong header.
      - Ví dụ, nếu dùng HMAC SHA256, chữ ký được tạo bằng cách:
        `HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)`
      - Chữ ký được sử dụng để xác minh rằng người gửi JWT là ai (tính xác thực) và đảm bảo rằng message không bị thay đổi trên đường truyền (tính toàn vẹn).

- **Ưu điểm của JWT:**

  - **Stateless:** Server không cần lưu trữ thông tin session của người dùng. Thông tin người dùng và quyền hạn có thể được chứa trong payload của JWT. Điều này rất tốt cho khả năng mở rộng (scalability).
  - **Self-contained:** Tất cả thông tin cần thiết để xác minh người dùng đều nằm trong token.
  - **Compact:** Kích thước nhỏ, có thể truyền qua URL, POST parameter, hoặc HTTP header.
  - **Widely adopted:** Được nhiều framework và ngôn ngữ hỗ trợ.
  - **Decoupling:** Cho phép tách biệt service xác thực khỏi các service nghiệp vụ.

- **Nhược điểm của JWT:**

  - **Không thể thu hồi (revoke) một cách dễ dàng trước khi hết hạn:** Một khi JWT đã được cấp, nó sẽ hợp lệ cho đến khi hết hạn. Để thu hồi, cần các giải pháp phức tạp hơn (ví dụ: blacklist token).
  - **Kích thước:** Nếu payload chứa quá nhiều thông tin, JWT có thể trở nên lớn.
  - **Bảo mật secret/private key:** Việc giữ an toàn cho secret key (cho HMAC) hoặc private key (cho RSA) là cực kỳ quan trọng. Nếu bị lộ, kẻ tấn công có thể tạo ra các token hợp lệ.
  - **Không mã hóa payload mặc định:** Payload được mã hóa Base64Url, không phải mã hóa mật mã. Bất kỳ ai cũng có thể giải mã và đọc payload. **Không bao giờ đặt thông tin nhạy cảm (như mật khẩu) vào payload JWT nếu không mã hóa thêm (JWE).**

- **Luồng hoạt động của xác thực dựa trên JWT:**
  1.  Client gửi yêu cầu đăng nhập (username/password) đến server.
  2.  Server xác thực credentials.
  3.  Nếu hợp lệ, server tạo một JWT (chứa thông tin người dùng, quyền, thời gian hết hạn) và ký nó bằng secret key.
  4.  Server gửi JWT này lại cho client.
  5.  Client lưu trữ JWT (thường trong Local Storage, Session Storage, hoặc HTTP Cookie - có cờ HttpOnly).
  6.  Đối với mỗi request tiếp theo đến các API được bảo vệ, client gửi JWT trong header `Authorization` với scheme `Bearer`.
      Ví dụ: `Authorization: Bearer <your_jwt_token>`
  7.  Server nhận request, trích xuất JWT từ header.
  8.  Server xác minh chữ ký của JWT bằng secret key đã lưu.
  9.  Nếu JWT hợp lệ và chưa hết hạn, server xử lý request.
  10. Nếu JWT không hợp lệ hoặc hết hạn, server từ chối request (thường với lỗi 401 Unauthorized).

---

**2. Thêm Dependencies cần thiết cho JWT**

Chúng ta sẽ sử dụng thư viện `jjwt` (Java JWT) của Auth0, một thư viện phổ biến và mạnh mẽ.
Mở `pom.xml` và thêm các dependencies sau:

```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.12.5</version> <!-- Kiểm tra phiên bản mới nhất -->
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.12.5</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId> <!-- Hoặc jjwt-gson nếu bạn thích Gson -->
    <version>0.12.5</version>
    <scope>runtime</scope>
</dependency>
```

- `jjwt-api`: Chứa các interface API.
- `jjwt-impl`: Chứa implementation mặc định.
- `jjwt-jackson`: Cung cấp serializer/deserializer JSON sử dụng Jackson (mà Spring Boot cũng dùng).
  Nhớ "Load Maven Changes".

---

**3. Tạo các Utility Classes cho JWT**

**3.1. `JwtUtil.java`**
Tạo class `JwtUtil` (ví dụ trong package `com.mycompany.ecommerceproject.security.jwt` hoặc `util`):

```java
package com.mycompany.ecommerceproject.security.jwt; // Tạo package này

import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import io.jsonwebtoken.security.Keys; // Cho việc tạo key an toàn
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Value; // Để đọc từ application.properties
import org.springframework.security.core.Authentication;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails; // UserDetails của Spring
import org.springframework.stereotype.Component;

import jakarta.annotation.PostConstruct; // Dùng để khởi tạo key
import java.security.Key;
import java.util.Date;
import java.util.List;
import java.util.stream.Collectors;
import java.util.function.Function;


@Component
public class JwtUtil {

    private static final Logger logger = LoggerFactory.getLogger(JwtUtil.class);

    @Value("${app.jwt.secret}") // Lấy từ application.properties
    private String jwtSecretString;

    @Value("${app.jwt.expirationMs}") // Lấy từ application.properties
    private int jwtExpirationMs;

    private Key jwtSecretKey;

    @PostConstruct // Được gọi sau khi dependency injection hoàn tất
    public void init() {
        // Chuyển đổi secret string thành Key object.
        // Nên sử dụng secret đủ dài và phức tạp.
        // Keys.hmacShaKeyFor yêu cầu secret phải đủ mạnh cho thuật toán đã chọn (ví dụ HS256 cần ít nhất 256 bit).
        // Nếu jwtSecretString của bạn là Base64 encoded của một byte array đủ mạnh thì có thể decode rồi dùng.
        // Cách đơn giản hơn cho demo là dùng trực tiếp byte array của string, nhưng đảm bảo nó đủ dài.
        byte[] keyBytes = jwtSecretString.getBytes();
        if (keyBytes.length < 32 && Jwts.SIG.HS256.getId().equals(SignatureAlgorithm.HS256.getValue())) {
             logger.warn("JWT Secret key for HS256 should be at least 32 bytes long. Current length: {}", keyBytes.length);
             // Trong production, bạn nên ném lỗi hoặc dùng key mạnh hơn.
             // For demo, we might proceed, but this is a security risk.
        }
        this.jwtSecretKey = Keys.hmacShaKeyFor(keyBytes);
        // Hoặc nếu bạn có một secret base64 mạnh:
        // this.jwtSecretKey = Keys.hmacShaKeyFor(Decoders.BASE64.decode(jwtSecretString));
    }

    public String generateJwtToken(Authentication authentication) {
        UserDetails userPrincipal = (UserDetails) authentication.getPrincipal();
        List<String> roles = userPrincipal.getAuthorities().stream()
                                .map(GrantedAuthority::getAuthority)
                                .collect(Collectors.toList());

        return Jwts.builder()
                .subject(userPrincipal.getUsername())
                .claim("roles", roles) // Thêm roles vào claims
                .issuedAt(new Date())
                .expiration(new Date((new Date()).getTime() + jwtExpirationMs))
                .signWith(jwtSecretKey, Jwts.SIG.HS256) // Sử dụng thuật toán HS256
                .compact();
    }

    // Hoặc một method generate token chỉ từ username và roles
    public String generateTokenFromUsername(String username, List<String> roles) {
        return Jwts.builder()
                .subject(username)
                .claim("roles", roles)
                .issuedAt(new Date())
                .expiration(new Date((new Date()).getTime() + jwtExpirationMs))
                .signWith(jwtSecretKey, Jwts.SIG.HS256)
                .compact();
    }


    public String getUsernameFromJwtToken(String token) {
        return getClaimFromToken(token, Claims::getSubject);
    }

    public List<String> getRolesFromJwtToken(String token) {
        Claims claims = getAllClaimsFromToken(token);
        return claims.get("roles", List.class);
    }

    public <T> T getClaimFromToken(String token, Function<Claims, T> claimsResolver) {
        final Claims claims = getAllClaimsFromToken(token);
        return claimsResolver.apply(claims);
    }

    private Claims getAllClaimsFromToken(String token) {
        return Jwts.parser()
                    .verifyWith(jwtSecretKey) // Xác minh bằng key
                    .build()
                    .parseSignedClaims(token) // Parse token đã ký
                    .getPayload();
    }

    public boolean validateJwtToken(String authToken) {
        try {
            Jwts.parser().verifyWith(jwtSecretKey).build().parseSignedClaims(authToken);
            return true;
        } catch (io.jsonwebtoken.security.SignatureException e) {
            logger.error("Invalid JWT signature: {}", e.getMessage());
        } catch (io.jsonwebtoken.MalformedJwtException e) {
            logger.error("Invalid JWT token: {}", e.getMessage());
        } catch (io.jsonwebtoken.ExpiredJwtException e) {
            logger.error("JWT token is expired: {}", e.getMessage());
        } catch (io.jsonwebtoken.UnsupportedJwtException e) {
            logger.error("JWT token is unsupported: {}", e.getMessage());
        } catch (IllegalArgumentException e) {
            logger.error("JWT claims string is empty: {}", e.getMessage());
        }
        return false;
    }
}
```

- `@Value`: Inject giá trị từ `application.properties`.
- `@PostConstruct`: Đảm bảo `jwtSecretKey` được khởi tạo sau khi `jwtSecretString` được inject.
- `generateJwtToken()`: Tạo token từ đối tượng `Authentication`. Lấy username làm `subject` và thêm `roles` vào claims.
- `getUsernameFromJwtToken()`: Trích xuất username từ token.
- `getRolesFromJwtToken()`: Trích xuất roles từ token.
- `validateJwtToken()`: Kiểm tra tính hợp lệ của token (chữ ký, thời gian hết hạn).

**3.2. Cấu hình Secret Key và Expiration Time**
Trong `src/main/resources/application.properties`:

```properties
# JWT Settings
# QUAN TRỌNG: Trong production, secret này phải rất mạnh, dài, và được quản lý an toàn (ví dụ: biến môi trường).
# Không commit secret key thực tế vào source control.
# Cho HS256, secret nên là một chuỗi ngẫu nhiên dài ít nhất 32 bytes (256 bits).
app.jwt.secret=MySuperSecretKeyForJwtGenerationWhichIsVeryLongAndSecureEnoughForHS256MaybeChangeIt
app.jwt.expirationMs=3600000 # 1 giờ (1 * 60 * 60 * 1000)
# Ví dụ thời gian ngắn hơn để test: 60000 (1 phút)
```

**CẢNH BÁO AN NINH:** Secret key ở trên chỉ là ví dụ. **KHÔNG BAO GIỜ** sử dụng secret key yếu hoặc dễ đoán trong môi trường production. Secret key nên được tạo ngẫu nhiên, có độ dài phù hợp với thuật toán (ví dụ, ít nhất 256 bit cho HMAC-SHA256), và được lưu trữ an toàn (ví dụ, thông qua biến môi trường, vault, hoặc các dịch vụ quản lý secret của cloud provider).

---

**4. Xây dựng Endpoint Xác thực (`/api/v1/auth/login`) để cấp JWT**

Chúng ta sẽ tạo một `AuthController` mới cho API (hoặc thêm vào `AuthController` đã có nếu muốn, nhưng tách ra cho rõ ràng).
Tạo `AuthRestController.java` trong package `com.mycompany.ecommerceproject.controller.api`:

```java
package com.mycompany.ecommerceproject.controller.api;

import com.mycompany.ecommerceproject.dto.LoginRequestDto; // Tạo DTO này
import com.mycompany.ecommerceproject.dto.JwtResponseDto; // Tạo DTO này
import com.mycompany.ecommerceproject.security.jwt.JwtUtil;
import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.media.Content;
import io.swagger.v3.oas.annotations.media.Schema;
import io.swagger.v3.oas.annotations.responses.ApiResponse;
import io.swagger.v3.oas.annotations.tags.Tag;
import jakarta.validation.Valid;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;
import java.util.stream.Collectors;

@RestController
@RequestMapping("/api/v1/auth")
@Tag(name = "Authentication API", description = "APIs for user authentication and JWT generation")
public class AuthRestController {

    private static final Logger logger = LoggerFactory.getLogger(AuthRestController.class);

    private final AuthenticationManager authenticationManager;
    private final JwtUtil jwtUtil;

    public AuthRestController(AuthenticationManager authenticationManager, JwtUtil jwtUtil) {
        this.authenticationManager = authenticationManager;
        this.jwtUtil = jwtUtil;
    }

    @Operation(summary = "User Login", description = "Authenticates a user and returns a JWT token upon successful login.")
    @ApiResponse(responseCode = "200", description = "Login successful, JWT token returned",
                 content = @Content(mediaType = MediaType.APPLICATION_JSON_VALUE,
                                    schema = @Schema(implementation = JwtResponseDto.class)))
    @ApiResponse(responseCode = "400", description = "Invalid login request data", content = @Content)
    @ApiResponse(responseCode = "401", description = "Invalid credentials", content = @Content)
    @PostMapping("/login")
    public ResponseEntity<?> authenticateUser(@Valid @RequestBody LoginRequestDto loginRequest) {
        logger.info("API login attempt for user: {}", loginRequest.getUsername());
        try {
            Authentication authentication = authenticationManager.authenticate(
                    new UsernamePasswordAuthenticationToken(loginRequest.getUsername(), loginRequest.getPassword()));

            SecurityContextHolder.getContext().setAuthentication(authentication);
            // UserDetails userDetails = (UserDetails) authentication.getPrincipal(); // Hoặc UserAccount
            // String jwt = jwtUtil.generateJwtToken(authentication);

            // Lấy thông tin UserDetails từ Authentication để tạo token
            // Điều này đảm bảo chúng ta dùng chính xác principal đã được xác thực
            UserDetails userDetails = (UserDetails) authentication.getPrincipal();
            List<String> roles = userDetails.getAuthorities().stream()
                                     .map(GrantedAuthority::getAuthority)
                                     .collect(Collectors.toList());

            String jwt = jwtUtil.generateTokenFromUsername(userDetails.getUsername(), roles);

            logger.info("User {} logged in successfully. JWT generated.", userDetails.getUsername());

            return ResponseEntity.ok(new JwtResponseDto(jwt, userDetails.getUsername(), roles));

        } catch (org.springframework.security.core.AuthenticationException e) {
            logger.warn("Login failed for user {}: {}", loginRequest.getUsername(), e.getMessage());
            // Trả về lỗi 401 rõ ràng hơn
            return ResponseEntity.status(401).body("Error: Invalid username or password");
        }
    }
}
```

**Tạo DTOs `LoginRequestDto.java` và `JwtResponseDto.java` trong package `dto`:**

```java
// LoginRequestDto.java
package com.mycompany.ecommerceproject.dto;

import jakarta.validation.constraints.NotEmpty;
import lombok.Data;

@Data
public class LoginRequestDto {
    @NotEmpty(message = "Username cannot be empty")
    private String username;

    @NotEmpty(message = "Password cannot be empty")
    private String password;
}

// JwtResponseDto.java
package com.mycompany.ecommerceproject.dto;

import lombok.Data;
import lombok.AllArgsConstructor;
import java.util.List;

@Data
@AllArgsConstructor
public class JwtResponseDto {
    private String token;
    private String type = "Bearer"; // Loại token, thường là Bearer
    private String username;
    private List<String> roles;

    public JwtResponseDto(String accessToken, String username, List<String> roles) {
        this.token = accessToken;
        this.username = username;
        this.roles = roles;
    }
}
```

- Trong `AuthRestController`, chúng ta inject `AuthenticationManager` (đã được tạo bean trong `SecurityConfig`).
- `authenticationManager.authenticate()`: Thực hiện xác thực username/password. Nếu thất bại, nó sẽ ném `AuthenticationException`.
- Nếu thành công, tạo JWT bằng `jwtUtil.generateJwtToken()` và trả về cho client.

---

**5. Tạo một `JwtAuthenticationFilter`**

Filter này sẽ xử lý JWT trong mỗi request đến API được bảo vệ.
Tạo `JwtAuthenticationFilter.java` trong package `com.mycompany.ecommerceproject.security.jwt`:

```java
package com.mycompany.ecommerceproject.security.jwt;

import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService; // UserDetailsService của Spring
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
import org.springframework.stereotype.Component;
import org.springframework.util.StringUtils;
import org.springframework.web.filter.OncePerRequestFilter; // Để đảm bảo filter chỉ chạy 1 lần mỗi request

import java.io.IOException;

@Component // Đánh dấu là một Spring bean
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    private static final Logger logger = LoggerFactory.getLogger(JwtAuthenticationFilter.class);

    @Autowired
    private JwtUtil jwtUtil;

    @Autowired
    private UserDetailsService userDetailsService; // UserDetailsService của Spring (UserAccountServiceImpl)

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain) throws ServletException, IOException {
        try {
            String jwt = parseJwt(request);
            if (jwt != null && jwtUtil.validateJwtToken(jwt)) {
                String username = jwtUtil.getUsernameFromJwtToken(jwt);

                // Tải UserDetails từ DB (thông qua UserDetailsService)
                UserDetails userDetails = userDetailsService.loadUserByUsername(username);
                // List<String> roles = jwtUtil.getRolesFromJwtToken(jwt); // Có thể lấy roles từ token
                // Collection<? extends GrantedAuthority> authorities = roles.stream()
                //        .map(SimpleGrantedAuthority::new)
                //        .collect(Collectors.toList());

                // Tạo đối tượng Authentication
                // UsernamePasswordAuthenticationToken authentication = new UsernamePasswordAuthenticationToken(
                //         userDetails, null, authorities);
                // Hoặc đơn giản hơn nếu UserDetails đã có authorities đúng:
                UsernamePasswordAuthenticationToken authentication = new UsernamePasswordAuthenticationToken(
                        userDetails, null, userDetails.getAuthorities());


                authentication.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));

                // Đặt Authentication vào SecurityContext
                SecurityContextHolder.getContext().setAuthentication(authentication);
                logger.debug("User '{}' authenticated with JWT. Authorities: {}", username, userDetails.getAuthorities());
            }
        } catch (Exception e) {
            logger.error("Cannot set user authentication: {}", e.getMessage());
        }

        filterChain.doFilter(request, response); // Chuyển request cho filter tiếp theo
    }

    private String parseJwt(HttpServletRequest request) {
        String headerAuth = request.getHeader("Authorization");

        if (StringUtils.hasText(headerAuth) && headerAuth.startsWith("Bearer ")) {
            return headerAuth.substring(7); // Bỏ "Bearer "
        }
        return null;
    }
}
```

- Kế thừa `OncePerRequestFilter` để đảm bảo filter chỉ được thực thi một lần cho mỗi request.
- `parseJwt()`: Trích xuất token từ header `Authorization`.
- Trong `doFilterInternal()`:
  - Lấy JWT.
  - Validate JWT.
  - Nếu hợp lệ, lấy username từ JWT.
  - Tải `UserDetails` từ `UserDetailsService` (để đảm bảo người dùng vẫn tồn tại và có thông tin mới nhất, thay vì chỉ dựa vào thông tin trong token đã cũ).
  - Tạo `UsernamePasswordAuthenticationToken`.
  - Đặt `Authentication` vào `SecurityContextHolder`.

---

**6. Cập nhật `SecurityConfig`**

```java
package com.mycompany.ecommerceproject.config;

// ... (các import đã có) ...
import com.mycompany.ecommerceproject.security.jwt.JwtAuthenticationFilter; // Import filter
import org.springframework.security.config.http.SessionCreationPolicy; // Import SessionCreationPolicy
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter; // Import UsernamePasswordAuthenticationFilter
import org.springframework.web.cors.CorsConfiguration; // Cho CORS
import org.springframework.web.cors.UrlBasedCorsConfigurationSource; // Cho CORS
import org.springframework.web.filter.CorsFilter; // Cho CORS
import org.springframework.context.annotation.Bean; // Đảm bảo @Bean được import
import org.springframework.http.HttpMethod; // Import HttpMethod
import org.springframework.security.config.Customizer; // Cho .httpBasic(Customizer.withDefaults())

@Configuration
@EnableWebSecurity
@EnableMethodSecurity(prePostEnabled = true, securedEnabled = true, jsr250Enabled = true)
public class SecurityConfig {

    private final UserAccountService userAccountService; // Hoặc UserDetailsService
    private final JwtAuthenticationFilter jwtAuthenticationFilter; // Inject JWT Filter

    public SecurityConfig(UserAccountService userAccountService, JwtAuthenticationFilter jwtAuthenticationFilter) {
        this.userAccountService = userAccountService;
        this.jwtAuthenticationFilter = jwtAuthenticationFilter;
    }

    // ... (passwordEncoder, userDetailsService, authenticationProvider, authenticationManager beans như cũ) ...
    // PasswordEncoder bean
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    // UserDetailsService bean (sử dụng UserAccountService đã implement UserDetailsService)
    @Bean
    public UserDetailsService userDetailsService() {
        return userAccountService; // UserAccountServiceImpl đã implement UserDetailsService
    }

    // DaoAuthenticationProvider bean
    @Bean
    public DaoAuthenticationProvider authenticationProvider() {
        DaoAuthenticationProvider authProvider = new DaoAuthenticationProvider();
        authProvider.setUserDetailsService(userDetailsService());
        authProvider.setPasswordEncoder(passwordEncoder());
        return authProvider;
    }

    // AuthenticationManager bean
    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration authConfig) throws Exception {
        return authConfig.getAuthenticationManager();
    }


    // (Tùy chọn) Cấu hình CORS nếu API được gọi từ domain khác
    @Bean
    public CorsFilter corsFilter() {
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        CorsConfiguration config = new CorsConfiguration();
        config.setAllowCredentials(true); // Cho phép cookie, authorization headers
        config.addAllowedOriginPattern("*"); // Hoặc chỉ định domain cụ thể: "http://localhost:3000"
        config.addAllowedHeader("*"); // Cho phép tất cả headers
        config.addAllowedMethod("*"); // Cho phép tất cả methods (GET, POST, etc.)
        source.registerCorsConfiguration("/**", config); // Áp dụng cho tất cả paths
        return new CorsFilter(source);
    }


    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            // Thêm CORS filter (nếu có)
            .addFilterBefore(corsFilter(), UsernamePasswordAuthenticationFilter.class) // Hoặc trước JwtAuthenticationFilter

            // Tắt CSRF (phổ biến cho API, đặc biệt nếu client không phải trình duyệt)
            // Nếu có cả web UI và API, cân nhắc .csrf(csrf -> csrf.ignoringRequestMatchers("/api/**"))
            .csrf(csrf -> csrf.disable())

            // Cấu hình Session Management là STATELESS vì chúng ta dùng JWT
            .sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))

            .authorizeHttpRequests(authorize -> authorize
                // Các endpoints công khai (không cần xác thực)
                .requestMatchers("/", "/home", "/css/**", "/js/**", "/images/**", "/webjars/**").permitAll()
                .requestMatchers("/products", "/products/{\\d+}").permitAll() // Web UI
                .requestMatchers("/register", "/login").permitAll() // Web UI login/register
                .requestMatchers("/swagger-ui.html", "/swagger-ui/**", "/v3/api-docs/**").permitAll() // Cho Swagger
                .requestMatchers("/api/v1/auth/login").permitAll() // API Login để lấy JWT

                // Các API GET sản phẩm công khai
                .requestMatchers(HttpMethod.GET, "/api/v1/products", "/api/v1/products/{\\d+}").permitAll()

                // Các API khác yêu cầu xác thực (sẽ được JwtAuthenticationFilter xử lý)
                // .requestMatchers("/api/v1/products/**").hasRole("ADMIN") // Đã dùng @PreAuthorize rồi
                // .requestMatchers("/api/v1/orders/**").authenticated() // Ví dụ
                // .requestMatchers("/cart/**", "/orders/**").authenticated() // Web UI

                .anyRequest().authenticated() // Mọi request khác đều cần xác thực
            )
            // Không cần formLogin và logout cho API nếu chỉ dùng JWT và STATELESS
            // Nếu vẫn muốn có Web UI login, bạn cần cấu hình phức tạp hơn hoặc tách SecurityConfig
            // Tạm thời comment out formLogin và logout của Web UI để tập trung vào JWT
            /*
            .formLogin(formLogin -> formLogin
                .loginPage("/login")
                .loginProcessingUrl("/perform_login")
                .defaultSuccessUrl("/", true)
                .failureUrl("/login?error=true")
                .permitAll()
            )
            .logout(logout -> logout
                .logoutRequestMatcher(new AntPathRequestMatcher("/logout"))
                .logoutSuccessUrl("/login?logout=true")
                .invalidateHttpSession(true)
                .deleteCookies("JSESSIONID")
                .permitAll()
            )
            */
            // Bỏ httpBasic nếu chỉ dựa vào JWT filter
            // .httpBasic(Customizer.withDefaults())
        ;

        // Đăng ký AuthenticationProvider
        http.authenticationProvider(authenticationProvider());

        // Thêm JWT filter vào trước UsernamePasswordAuthenticationFilter
        http.addFilterBefore(jwtAuthenticationFilter, UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }
}
```

- **`corsFilter()`:** (Tùy chọn) Nếu API của bạn được gọi từ một frontend chạy trên domain khác (ví dụ: React app trên `localhost:3000` gọi API trên `localhost:8080`), bạn cần cấu hình CORS.
- **`csrf(csrf -> csrf.disable())`:** Tắt CSRF. Đối với API stateless dùng token, CSRF protection thường không cần thiết như với ứng dụng web truyền thống dùng session cookie. Nếu bạn có cả web UI và API, cần cân nhắc kỹ.
- **`sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))`:** **Rất quan trọng.** Yêu cầu Spring Security không tạo hoặc sử dụng HTTP session. Mỗi request phải tự chứa thông tin xác thực (JWT).
- Cập nhật `authorizeHttpRequests`:
  - Cho phép `/api/v1/auth/login` công khai.
  - Các API GET sản phẩm vẫn công khai.
  - Các API khác (POST, PUT, DELETE sản phẩm) sẽ được bảo vệ bởi `JwtAuthenticationFilter` và `@PreAuthorize`.
- **Loại bỏ `formLogin`, `logout`, `httpBasic` (tạm thời):** Vì chúng ta đang chuyển sang xác thực JWT hoàn toàn cho API. Nếu bạn muốn cả Web UI (với session) và API (với JWT) cùng hoạt động, cấu hình sẽ phức tạp hơn, có thể cần nhiều `SecurityFilterChain` beans với các `securityMatcher`. Để đơn giản, phần này tập trung vào JWT cho API.
- **`http.addFilterBefore(jwtAuthenticationFilter, UsernamePasswordAuthenticationFilter.class)`:** Thêm `JwtAuthenticationFilter` của chúng ta vào chuỗi filter, trước filter xử lý username/password (mặc dù filter này có thể không được dùng nếu `formLogin` bị tắt).

---

**7. Kiểm thử luồng xác thực JWT với Postman**

1.  **Khởi động lại ứng dụng.**
2.  **Bước 1: Lấy JWT Token**

    - Mở Postman.
    - Method: `POST`
    - URL: `http://localhost:8080/api/v1/auth/login`
    - **Headers:** `Content-Type: application/json`
    - **Body:** Chọn "raw" và "JSON". Nhập:
      ```json
      {
        "username": "admin",
        "password": "adminpass" // Hoặc password bạn đã đặt cho admin
      }
      ```
    - Nhấn Send.
    - Response sẽ là JSON chứa `token`, `username`, `roles`. Copy giá trị của `token`.
      ```json
      {
        "token": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsInJvbGVzIjpbIlJPTEVfQURNSU4iLCJST0xFX1VTRVIiXSwiaWF0IjoxNzA...",
        "type": "Bearer",
        "username": "admin",
        "roles": ["ROLE_ADMIN", "ROLE_USER"]
      }
      ```

3.  **Bước 2: Gọi API được bảo vệ bằng JWT**
    - Thử gọi API tạo sản phẩm (yêu cầu `ROLE_ADMIN`):
      - Method: `POST`
      - URL: `http://localhost:8080/api/v1/products`
      - **Headers:**
        - `Content-Type: application/json`
        - `Authorization: Bearer <PASTE_YOUR_JWT_TOKEN_HERE>` (Thay `<PASTE_YOUR_JWT_TOKEN_HERE>` bằng token bạn vừa copy).
      - **Body:**
        ```json
        {
          "name": "JWT Secured Product",
          "description": "Created with JWT auth",
          "price": 199.99,
          "stockQuantity": 20,
          "categoryId": 1
        }
        ```
      - Nhấn Send. Nếu JWT hợp lệ và user `admin` có quyền, bạn sẽ nhận được response `201 Created`.
    - **Thử không gửi token hoặc gửi token sai/hết hạn:** Bạn sẽ nhận được lỗi `401 Unauthorized` hoặc `403 Forbidden`.
    - **Thử đăng nhập bằng user thường (`john.doe`) lấy token, rồi dùng token đó gọi API tạo sản phẩm:** Bạn sẽ nhận được lỗi `403 Forbidden` vì user thường không có `ROLE_ADMIN`.

---

**8. Lưu ý về Refresh Tokens (Tùy chọn nâng cao)**

- JWT access token thường có thời gian sống ngắn (ví dụ: 15 phút - 1 giờ) để giảm thiểu rủi ro nếu token bị lộ.
- Khi access token hết hạn, client cần một cách để lấy access token mới mà không yêu cầu người dùng đăng nhập lại. Đây là lúc cần **Refresh Token**.
- **Luồng Refresh Token:**
  1.  Khi đăng nhập thành công, server cấp cả **Access Token** (ngắn hạn) và **Refresh Token** (dài hạn hơn, ví dụ: vài ngày, vài tuần).
  2.  Refresh Token được lưu trữ an toàn hơn phía client (ví dụ: HTTP-only cookie).
  3.  Khi Access Token hết hạn, client gửi Refresh Token đến một endpoint đặc biệt (ví dụ: `/api/v1/auth/refresh-token`).
  4.  Server xác minh Refresh Token (kiểm tra trong DB xem có hợp lệ và chưa bị thu hồi không).
  5.  Nếu hợp lệ, server cấp một cặp Access Token mới và (có thể) Refresh Token mới.
- **Lợi ích:** Cải thiện bảo mật (access token ngắn hạn) và trải nghiệm người dùng (không phải đăng nhập lại thường xuyên).
- **Triển khai:**
  - Lưu trữ Refresh Token trong CSDL, liên kết với user.
  - Tạo endpoint `/refresh-token`.
  - Thêm logic để thu hồi Refresh Token (ví dụ: khi đổi mật khẩu, logout).
  - Đây là một tính năng nâng cao, cần cân nhắc kỹ về bảo mật.

---

**9. Best Practices khi sử dụng JWT**

- **HTTPS:** Luôn truyền JWT qua HTTPS để tránh bị nghe lén.
- **Secret Key mạnh và an toàn:** Không hard-code, sử dụng biến môi trường hoặc vault.
- **Thuật toán ký mạnh:** Ưu tiên RSA hoặc ECDSA nếu cần xác minh chữ ký bởi nhiều bên mà không chia sẻ private key. HMAC-SHA256 là đủ nếu chỉ server tạo và xác minh token.
- **Thời gian hết hạn (Expiration Time - `exp` claim):** Đặt thời gian hết hạn hợp lý cho access token (ngắn) và refresh token (dài hơn).
- **Không lưu trữ thông tin nhạy cảm trong Payload:** Payload có thể bị giải mã. Chỉ lưu các thông tin cần thiết cho việc xác định người dùng và quyền hạn.
- **Validate các claim quan trọng:** Ngoài chữ ký và `exp`, kiểm tra `iss` (issuer), `aud` (audience) nếu bạn dùng chúng.
- **Xử lý việc thu hồi token (Revocation):** Nếu cần thu hồi token trước khi hết hạn (ví dụ: user đổi mật khẩu, logout), bạn cần một cơ chế (blacklist, versioning token).
- **Lưu trữ JWT phía Client:**
  - **Local Storage/Session Storage:** Dễ bị tấn công XSS (Cross-Site Scripting).
  - **HTTP-only Cookie:** An toàn hơn trước XSS, nhưng cần cẩn thận với CSRF (mặc dù JWT thường được dùng cho API stateless, nơi CSRF ít là vấn đề hơn nếu không dùng cookie cho session).
- **Tránh JWT quá lớn:** Giữ payload nhỏ gọn.
- **Sử dụng Refresh Tokens:** Cho trải nghiệm người dùng tốt hơn và bảo mật hơn.

---

**Kết thúc Phần 13**

Chúng ta đã triển khai thành công cơ chế xác thực dựa trên JWT cho REST API của mình:

- Hiểu về JWT, cấu trúc và luồng hoạt động.
- Tạo JWT utility, endpoint login để cấp token.
- Xây dựng `JwtAuthenticationFilter` để xác thực token trong mỗi request.
- Cấu hình Spring Security để sử dụng JWT và trở thành stateless.

API của bạn giờ đây đã được bảo vệ bằng một cơ chế hiện đại và linh hoạt, sẵn sàng cho các client khác nhau.

**Trong Phần 14 (phần tùy chọn, nhưng rất nên làm), chúng ta sẽ tìm hiểu về: "Testing REST APIs (Unit & Integration Tests).**" Điều này đảm bảo rằng API của bạn hoạt động đúng như mong đợi và duy trì chất lượng khi có thay đổi.

Tuyệt vời! Sau khi đã xây dựng và bảo mật REST API, việc đảm bảo chúng hoạt động đúng đắn và ổn định qua thời gian là cực kỳ quan trọng. Testing là một phần không thể thiếu trong quy trình phát triển phần mềm chuyên nghiệp.

---

**Phần 14: Testing REST APIs (Unit & Integration Tests)**

Trong phần này, chúng ta sẽ:

1.  **Tại sao cần Test API? Các loại Test.**
    - Unit Tests, Integration Tests, End-to-End (E2E) Tests.
    - Tập trung vào Unit Tests cho Controller và Integration Tests cho API endpoints.
2.  **Công cụ và Frameworks:**
    - JUnit 5 (Jupiter).
    - Mockito (cho mocking dependencies trong unit tests).
    - Spring Boot Test (`@SpringBootTest`, `@WebMvcTest`, `@DataJpaTest`).
    - RestAssured hoặc `MockMvc` (cho testing API endpoints).
3.  **Unit Testing Controllers (Sử dụng `@WebMvcTest` và `MockMvc`):**
    - Mock Service Layer dependencies.
    - Kiểm tra request mapping, request parameters, request body.
    - Kiểm tra response status, response headers, response body.
    - Kiểm tra việc gọi đúng các method của service.
    - Kiểm tra xử lý validation và exception.
4.  **Integration Testing API Endpoints (Sử dụng `@SpringBootTest` và `TestRestTemplate` hoặc `MockMvc` với full context):**
    - Test luồng hoàn chỉnh từ request đến response, bao gồm cả tương tác với database (có thể dùng H2 in-memory cho test).
    - Kiểm tra authentication và authorization.
5.  **Viết Test Cases cho `ProductRestController`:**
    - Test GET all products, GET product by ID.
    - Test POST create product (happy path, validation errors, authorization).
    - Test PUT update product.
    - Test DELETE product.
6.  **Lưu ý về Test Data và Môi trường Test.**
7.  **Best Practices cho việc viết Test API.**

---

**1. Tại sao cần Test API? Các loại Test.**

- **Tại sao cần Test API?**

  - **Đảm bảo tính đúng đắn (Correctness):** API trả về đúng dữ liệu, đúng status code, và xử lý đúng các trường hợp đầu vào khác nhau.
  - **Phát hiện lỗi sớm (Early Bug Detection):** Tìm ra lỗi ngay trong quá trình phát triển, giảm chi phí sửa lỗi sau này.
  - **Hỗ trợ Refactoring:** Khi bạn có bộ test tốt, bạn có thể tự tin refactor code mà không sợ làm hỏng chức năng hiện có. Test sẽ báo hiệu nếu có gì đó sai.
  - **Tài liệu sống (Living Documentation):** Test cases có thể đóng vai trò như một dạng tài liệu, mô tả cách API nên hoạt động.
  - **Tự động hóa (Automation):** Test có thể được chạy tự động trong quy trình CI/CD.
  - **Tăng độ tin cậy (Reliability):** API được test kỹ càng sẽ ổn định và đáng tin cậy hơn.

- **Các loại Test (liên quan đến API):**

  1.  **Unit Tests (Kiểm thử đơn vị):**
      - Test từng đơn vị code nhỏ nhất một cách cô lập (ví dụ: một method trong service, một method trong controller).
      - Các dependency bên ngoài (service khác, database) thường được **mock** (giả lập).
      - Mục tiêu: Kiểm tra logic của đơn vị đó.
      - Nhanh, dễ viết, dễ chạy.
  2.  **Integration Tests (Kiểm thử tích hợp):**
      - Test sự tương tác giữa các component khác nhau của ứng dụng.
      - Ví dụ: Test luồng từ Controller -> Service -> Repository -> Database (có thể là DB thật hoặc in-memory).
      - Mục tiêu: Đảm bảo các component làm việc đúng với nhau.
      - Chậm hơn unit test, phức tạp hơn để thiết lập.
  3.  **End-to-End (E2E) Tests (Kiểm thử đầu cuối):**
      - Test toàn bộ luồng ứng dụng từ phía client (ví dụ: giao diện người dùng hoặc một API client) đến backend và database.
      - Mô phỏng hành vi thực tế của người dùng.
      - Phức tạp nhất, chậm nhất, và "mong manh" (brittle) nhất.
  4.  **Contract Tests:** Đảm bảo rằng API consumer (client) và API provider (server) tuân thủ một "hợp đồng" (contract) đã định nghĩa về cấu trúc request/response. (Ví dụ: Pact).

  **Trong phần này, chúng ta sẽ tập trung vào:**

  - **Unit testing Controllers:** Cách ly controller và mock các service.
  - **Integration testing API endpoints:** Test toàn bộ stack của một API endpoint (từ HTTP request đến DB và HTTP response).

---

**2. Công cụ và Frameworks**

Spring Boot cung cấp hỗ trợ tuyệt vời cho việc testing:

- **JUnit 5 (Jupiter):** Framework testing tiêu chuẩn cho Java. Spring Boot Test tích hợp sẵn với JUnit 5.
  - Annotations: `@Test`, `@BeforeEach`, `@AfterEach`, `@DisplayName`, `@Nested`, `@ParameterizedTest`, ...
- **Mockito:** Framework mocking phổ biến nhất cho Java. Dùng để tạo các đối tượng giả (mocks) cho các dependency.
  - `@Mock`, `@InjectMocks`, `when(...).thenReturn(...)`, `verify(...)`.
- **Spring Boot Test:** Cung cấp các tiện ích và annotation để test ứng dụng Spring Boot.
  - `@SpringBootTest`: Load toàn bộ ApplicationContext của Spring Boot. Dùng cho integration tests.
    - `webEnvironment = WebEnvironment.MOCK`: Tạo `MockMvc` bean.
    - `webEnvironment = WebEnvironment.RANDOM_PORT` hoặc `DEFINED_PORT`: Khởi động server web thật trên một port ngẫu nhiên hoặc xác định. Dùng với `TestRestTemplate`.
  - `@WebMvcTest(controllers = YourController.class)`: Chỉ test Web Layer (Controllers, Filters, Resolvers, etc.). Không load toàn bộ context, chỉ load các bean liên quan đến MVC. Các service, repository dependencies cần được `@MockBean`. Rất phù hợp cho unit testing controllers.
  - `@DataJpaTest`: Chỉ test JPA Layer (Repositories, Entities). Sử dụng H2 in-memory database mặc định.
  - `@MockBean`: Tạo một mock của một Spring bean và đưa nó vào ApplicationContext.
  - `TestRestTemplate`: Một HTTP client tiện lợi để gọi các API endpoint khi server đang chạy (dùng với `@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)`).
- **`MockMvc`:** Cho phép bạn thực hiện các HTTP request đến dispatcher servlet của Spring MVC mà không cần khởi động server web thực sự. Rất mạnh mẽ để test controllers.
  - `mockMvc.perform(get("/path")...).andExpect(status().isOk())...`
- **AssertJ:** Thư viện assertion linh hoạt và dễ đọc (ví dụ: `assertThat(actual).isEqualTo(expected)`). Spring Boot Starter Test bao gồm AssertJ.
- **JSONAssert:** (Tùy chọn) Để so sánh JSON một cách linh hoạt (bỏ qua thứ tự trường,...)
- **RestAssured:** (Tùy chọn) Một thư viện Java DSL mạnh mẽ để test RESTful services. Cung cấp cú pháp fluent và dễ đọc.

**Dependencies (thường đã có trong `spring-boot-starter-test`):**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
<!-- Nếu cần RestAssured (tùy chọn) -->
<!--
<dependency>
    <groupId>io.rest-assured</groupId>
    <artifactId>rest-assured</artifactId>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>io.rest-assured</groupId>
    <artifactId>spring-mock-mvc</artifactId>
    <scope>test</scope>
</dependency>
-->
```

---

**3. Unit Testing Controllers (Sử dụng `@WebMvcTest` và `MockMvc`)**

Chúng ta sẽ unit test `ProductRestController`. Mục tiêu là kiểm tra logic của controller một cách cô lập, giả lập (mock) `ProductService`.

Tạo file test trong `src/test/java/com/mycompany/ecommerceproject/controller/api/ProductRestControllerTest.java`:
(IDE của bạn thường có tính năng "Go to Test" hoặc "Create Test" khi bạn ở trong class `ProductRestController`).

```java
package com.mycompany.ecommerceproject.controller.api;

import com.fasterxml.jackson.databind.ObjectMapper; // Để chuyển object thành JSON string
import com.mycompany.ecommerceproject.dto.ProductDto;
import com.mycompany.ecommerceproject.service.ProductService;
import com.mycompany.ecommerceproject.exception.ResourceNotFoundException; // Để test exception
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageImpl;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;
import org.springframework.data.domain.Sort;
import org.springframework.http.MediaType;
import org.springframework.security.test.context.support.WithMockUser; // Cho test bảo mật
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.ResultActions;


import java.math.BigDecimal;
import java.util.Arrays;
import java.util.List;
import java.util.Optional;

import static org.mockito.ArgumentMatchers.any; // Cho any(Class.class)
import static org.mockito.ArgumentMatchers.anyLong;
import static org.mockito.BDDMockito.given; // BDD style mocking
import static org.mockito.Mockito.doNothing;
import static org.mockito.Mockito.doThrow;
import static org.springframework.security.test.web.servlet.request.SecurityMockMvcRequestPostProcessors.csrf; // Cho CSRF
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;
import static org.hamcrest.Matchers.is; // Cho jsonPath
import static org.hamcrest.Matchers.hasSize; // Cho jsonPath

@WebMvcTest(ProductRestController.class) // Chỉ test ProductRestController, không load toàn bộ context
public class ProductRestControllerTest {

    @Autowired
    private MockMvc mockMvc; // Để thực hiện HTTP request giả

    @MockBean // Tạo mock cho ProductService và inject vào context của @WebMvcTest
    private ProductService productService;

    @Autowired
    private ObjectMapper objectMapper; // Spring Boot tự động cấu hình bean này

    private ProductDto productDto1;
    private ProductDto productDto2;

    @BeforeEach // Chạy trước mỗi @Test method
    void setUp() {
        productDto1 = new ProductDto(1L, "Laptop X", "Powerful laptop", new BigDecimal("1200.00"), "img.jpg", 50, 1L, "Electronics");
        productDto2 = new ProductDto(2L, "Mouse Y", "Ergonomic mouse", new BigDecimal("25.00"), "mouse.jpg", 100, 1L, "Electronics");
    }

    @Test
    void getAllProducts_shouldReturnPageOfProducts() throws Exception {
        // Given
        List<ProductDto> productList = Arrays.asList(productDto1, productDto2);
        Pageable pageable = PageRequest.of(0, 10, Sort.by("name").ascending());
        Page<ProductDto> productPage = new PageImpl<>(productList, pageable, productList.size());

        given(productService.findAllProductDtos(any(Pageable.class))).willReturn(productPage);

        // When
        ResultActions response = mockMvc.perform(get("/api/v1/products")
                .param("page", "0")
                .param("size", "10")
                .param("sort", "name,asc")
                .contentType(MediaType.APPLICATION_JSON));

        // Then
        response.andExpect(status().isOk())
                .andExpect(jsonPath("$.content", hasSize(2)))
                .andExpect(jsonPath("$.content[0].name", is(productDto1.getName())))
                .andExpect(jsonPath("$.totalElements", is(2)));
    }

    @Test
    void getProductById_whenProductExists_shouldReturnProduct() throws Exception {
        // Given
        given(productService.findProductDtoById(1L)).willReturn(Optional.of(productDto1));

        // When
        ResultActions response = mockMvc.perform(get("/api/v1/products/1")
                .contentType(MediaType.APPLICATION_JSON));

        // Then
        response.andExpect(status().isOk())
                .andExpect(jsonPath("$.name", is(productDto1.getName())))
                .andExpect(jsonPath("$.id", is(1)));
    }

    @Test
    void getProductById_whenProductNotExists_shouldReturnNotFound() throws Exception {
        // Given
        given(productService.findProductDtoById(99L)).willReturn(Optional.empty());
        // Hoặc nếu service ném exception:
        // given(productService.findProductDtoById(99L)).willThrow(new ResourceNotFoundException("Product not found"));
        // Thì GlobalExceptionHandler của bạn cần được cấu hình để bắt ResourceNotFoundException và trả về 404
        // @WebMvcTest không load GlobalExceptionHandler mặc định, bạn có thể cần test riêng hoặc dùng @SpringBootTest

        // When
        ResultActions response = mockMvc.perform(get("/api/v1/products/99")
                .contentType(MediaType.APPLICATION_JSON));

        // Then
        // Nếu service trả về Optional.empty và controller ném ResourceNotFoundException
        // Và GlobalExceptionHandler được load (thường không với @WebMvcTest trừ khi bạn import)
        // thì bạn có thể expect 404.
        // Nếu không có GlobalExceptionHandler, controller sẽ ném exception và test sẽ fail.
        // Trong ProductRestController, chúng ta có .orElseThrow(() -> new ResourceNotFoundException(...))
        // @WebMvcTest không tự động áp dụng @ControllerAdvice.
        // Để test exception handling này, bạn cần @SpringBootTest hoặc cấu hình @ControllerAdvice vào @WebMvcTest context.
        // Tạm thời, chúng ta sẽ kiểm tra xem service có được gọi không và controller có ném lỗi không.
        // Để test đúng 404, cần cấu hình GlobalExceptionHandler vào test context hoặc dùng cách khác.
        // Một cách đơn giản là controller trả về ResponseEntity.notFound() nếu Optional rỗng.
        // Sửa ProductRestController.getProductById:
        // return productService.findProductDtoById(id)
        //        .map(ResponseEntity::ok)
        //        .orElse(ResponseEntity.notFound().build()); // Thay vì ném Exception
        // Với cách sửa trên, test này sẽ là:
        response.andExpect(status().isNotFound()); // Giả sử controller đã được sửa
        // Nếu bạn giữ nguyên controller ném Exception, và muốn test 404 từ GlobalExceptionHandler,
        // bạn cần import GlobalExceptionHandler vào @WebMvcTest:
        // @WebMvcTest(controllers = ProductRestController.class,
        //            includeFilters = @ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, classes = GlobalExceptionHandler.class))
        // Hoặc dùng @SpringBootTest.
    }

    @Test
    @WithMockUser(username = "admin", roles = {"ADMIN"}) // Giả lập user admin đã đăng nhập
    void createProduct_whenAdminAndValidInput_shouldReturnCreated() throws Exception {
        // Given
        given(productService.createProduct(any(ProductDto.class))).willReturn(productDto1); // Giả sử productDto1 là kết quả sau khi tạo

        // When
        ResultActions response = mockMvc.perform(post("/api/v1/products")
                .with(csrf()) // Thêm CSRF token nếu CSRF enabled cho API (chúng ta đã disable trong SecurityConfig cho API)
                               // Nếu CSRF đã disable hoàn toàn thì không cần .with(csrf())
                .contentType(MediaType.APPLICATION_JSON)
                .content(objectMapper.writeValueAsString(productDto1))); // Gửi DTO làm body

        // Then
        response.andExpect(status().isCreated()) // 201
                .andExpect(jsonPath("$.name", is(productDto1.getName())))
                .andExpect(header().exists("Location")); // Kiểm tra header Location
    }

    @Test
    @WithMockUser(username = "admin", roles = {"ADMIN"})
    void createProduct_whenInvalidInput_shouldReturnBadRequest() throws Exception {
        // Given
        ProductDto invalidProductDto = new ProductDto(); // Thiếu các trường required
        invalidProductDto.setPrice(new BigDecimal("-10")); // Giá trị không hợp lệ

        // productService.createProduct sẽ không được gọi nếu validation fail
        // given(productService.createProduct(any(ProductDto.class))).willReturn(productDto1);


        // When
        ResultActions response = mockMvc.perform(post("/api/v1/products")
                .contentType(MediaType.APPLICATION_JSON)
                .content(objectMapper.writeValueAsString(invalidProductDto)));

        // Then
        response.andExpect(status().isBadRequest())
                .andExpect(jsonPath("$.name", is("Product name cannot be empty"))) // Lỗi từ validation
                .andExpect(jsonPath("$.price", is("Price must be greater than 0")))
                .andExpect(jsonPath("$.stockQuantity", is("Stock quantity cannot be null")))
                .andExpect(jsonPath("$.categoryId", is("Category ID cannot be null for product creation/update")));
    }


    @Test
    @WithMockUser(username = "user", roles = {"USER"}) // User thường
    void createProduct_whenNotAdmin_shouldReturnForbidden() throws Exception {
        // When
        ResultActions response = mockMvc.perform(post("/api/v1/products")
                .contentType(MediaType.APPLICATION_JSON)
                .content(objectMapper.writeValueAsString(productDto1)));

        // Then
        response.andExpect(status().isForbidden()); // 403
    }

    @Test
    void createProduct_whenUnauthenticated_shouldReturnUnauthorized() throws Exception {
         // When
        ResultActions response = mockMvc.perform(post("/api/v1/products")
                .contentType(MediaType.APPLICATION_JSON)
                .content(objectMapper.writeValueAsString(productDto1)));

        // Then
        // Nếu không có @WithMockUser, Spring Security mặc định là anonymous.
        // Nếu API yêu cầu authenticated() hoặc role cụ thể, nó sẽ trả về 401 hoặc 403.
        // Với @PreAuthorize("hasRole('ADMIN')"), user anonymous sẽ không qua được.
        // Behavior chính xác phụ thuộc vào SecurityConfig và @PreAuthorize.
        // Vì POST /api/v1/products yêu cầu ADMIN, user anonymous sẽ bị từ chối.
        // Nếu đã bật .httpBasic() hoặc các cơ chế khác, có thể là 401.
        // Nếu không có cơ chế xác thực nào được kích hoạt cho request này và anonymous bị deny, có thể là 403.
        // Với cấu hình JWT, nếu không có token, JwtAuthenticationFilter sẽ không set Authentication.
        // Sau đó @PreAuthorize sẽ kiểm tra và ném AccessDeniedException (dẫn đến 403).
        // Nếu bạn muốn 401 khi không có token, bạn cần cấu hình AuthenticationEntryPoint.
        response.andExpect(status().isUnauthorized()); // Hoặc isForbidden() tùy cấu hình entry point
        // Hiện tại, Spring Security mặc định trả về 401 nếu không xác thực và tài nguyên yêu cầu xác thực.
        // Tuy nhiên, @PreAuthorize có thể ném AccessDeniedException (403) nếu SecurityContext rỗng.
        // Hãy kiểm tra kỹ response thực tế hoặc cấu hình AuthenticationEntryPoint cho API.
        // Giả sử với config JWT, nếu không có token, filter không làm gì, @PreAuthorize sẽ thấy không có auth -> 403
        // response.andExpect(status().isForbidden()); // Sửa lại nếu thấy 403
    }


    @Test
    @WithMockUser(username = "admin", roles = {"ADMIN"})
    void deleteProduct_whenAdminAndProductExists_shouldReturnNoContent() throws Exception {
        // Given
        Long productId = 1L;
        given(productService.findProductDtoById(productId)).willReturn(Optional.of(productDto1)); // Giả lập sản phẩm tồn tại
        doNothing().when(productService).deleteById(productId); // Mock việc xóa

        // When
        ResultActions response = mockMvc.perform(delete("/api/v1/products/" + productId));

        // Then
        response.andExpect(status().isNoContent()); // 204
    }

    @Test
    @WithMockUser(username = "admin", roles = {"ADMIN"})
    void deleteProduct_whenAdminAndProductNotExists_shouldReturnNotFound() throws Exception {
        // Given
        Long productId = 99L;
        // Giả lập service ném ResourceNotFoundException khi findById
        given(productService.findProductDtoById(productId)).willReturn(Optional.empty());
        // Hoặc, nếu controller gọi findById trước, rồi mới gọi deleteById
        // given(productService.findProductDtoById(productId)).willThrow(new ResourceNotFoundException("Product not found"));

        // Trong ProductRestController.deleteProduct, nó gọi findProductDtoById trước.
        // Nếu Optional rỗng, nó ném ResourceNotFoundException.

        // When
        ResultActions response = mockMvc.perform(delete("/api/v1/products/" + productId));

        // Then
        response.andExpect(status().isNotFound());
    }
}
```

- `@WebMvcTest(ProductRestController.class)`: Chỉ load các bean cần thiết cho MVC và `ProductRestController`. `ProductService` sẽ không được load, nên ta dùng `@MockBean`.
- `@Autowired private MockMvc mockMvc`: Dùng để gửi HTTP request giả.
- `@MockBean private ProductService productService`: Tạo một mock của `ProductService`. Spring sẽ inject mock này vào `ProductRestController`.
- `@Autowired private ObjectMapper objectMapper`: Dùng để chuyển đổi đối tượng Java sang chuỗi JSON cho request body.
- `@BeforeEach`: Thiết lập dữ liệu test mẫu.
- `given(productService.someMethod(...)).willReturn(...)`: Cấu hình hành vi của mock service (Mockito BDD style).
- `mockMvc.perform(get/post/put/delete(...))`: Thực hiện request.
  - `.contentType(MediaType.APPLICATION_JSON)`: Set header `Content-Type`.
  - `.content(objectMapper.writeValueAsString(dto))`: Set request body.
  - `.param("name", "value")`: Set query parameter.
  - `.with(csrf())`: Thêm CSRF token (nếu CSRF được bật cho API path đó trong `SecurityConfig`). Với cấu hình hiện tại của chúng ta (CSRF disable cho API), dòng này không cần thiết.
- `.andExpect(status().isOk()/isCreated()/isNotFound()/...)`: Kiểm tra HTTP status code.
- `.andExpect(jsonPath("$.fieldName", is(value)))`: Kiểm tra nội dung JSON response (sử dụng Jayway JsonPath).
- `.andExpect(header().exists("Location"))`: Kiểm tra sự tồn tại của header.
- `@WithMockUser(username = "admin", roles = {"ADMIN"})`: Rất hữu ích từ `spring-security-test`. Nó giả lập một người dùng đã được xác thực với username và roles cho context của test method đó. Điều này cho phép test các endpoint được bảo vệ bởi `@PreAuthorize` hoặc các rule trong `SecurityConfig`.

**Lưu ý về `GlobalExceptionHandler` và `@WebMvcTest`:**
`@WebMvcTest` mặc định không load các bean `@ControllerAdvice`. Do đó, nếu `ProductRestController` của bạn ném một exception (ví dụ `ResourceNotFoundException`) và bạn mong đợi `GlobalExceptionHandler` xử lý nó để trả về 404, thì test này có thể không hoạt động như mong đợi (test có thể thấy exception không được bắt).
Có vài cách giải quyết:

1.  **Controller tự xử lý và trả về `ResponseEntity` phù hợp:** Ví dụ, thay vì ném exception, controller có thể trả về `ResponseEntity.notFound().build()`.
2.  **Import `@ControllerAdvice` vào test context:**
    ```java
    // import org.springframework.context.annotation.FilterType;
    // import org.springframework.context.annotation.ComponentScan;
    // @WebMvcTest(controllers = ProductRestController.class,
    //            includeFilters = @ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, classes = GlobalExceptionHandler.class))
    ```
3.  **Sử dụng `@SpringBootTest`:** Nó sẽ load toàn bộ context, bao gồm cả `@ControllerAdvice`.

Trong ví dụ trên, tôi đã giả định rằng controller sẽ ném exception và `GlobalExceptionHandler` sẽ xử lý. Nếu bạn chọn cách 1, bạn sẽ thay đổi expectation của test.

---

**4. Integration Testing API Endpoints (Sử dụng `@SpringBootTest`)**

Integration test sẽ kiểm tra toàn bộ luồng, từ HTTP request đến database.
Tạo file `ProductRestControllerIntegrationTest.java` trong `src/test/java/...`:

```java
package com.mycompany.ecommerceproject.controller.api;

import com.mycompany.ecommerceproject.dto.ProductDto;
import com.mycompany.ecommerceproject.entity.Category;
import com.mycompany.ecommerceproject.entity.Product;
import com.mycompany.ecommerceproject.repository.CategoryRepository;
import com.mycompany.ecommerceproject.repository.ProductRepository;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.web.client.TestRestTemplate; // Hoặc dùng MockMvc với full context
// import org.springframework.boot.test.web.server.LocalServerPort; // Nếu dùng TestRestTemplate
import org.springframework.http.*;
import org.springframework.security.crypto.password.PasswordEncoder; // Để tạo user test
import com.mycompany.ecommerceproject.entity.UserAccount; // Để tạo user test
import com.mycompany.ecommerceproject.repository.UserAccountRepository; // Để tạo user test
import org.springframework.test.context.ActiveProfiles; // Nếu có profile test riêng
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.web.context.WebApplicationContext;


import java.math.BigDecimal;
import java.util.Arrays;

import static org.assertj.core.api.Assertions.assertThat;
import static org.springframework.security.test.web.servlet.setup.SecurityMockMvcConfigurers.springSecurity;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;


@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT) // Khởi động server thật trên port ngẫu nhiên
// Hoặc @SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.MOCK) để dùng MockMvc với full context
@ActiveProfiles("test") // (Tùy chọn) Nếu bạn có application-test.properties
public class ProductRestControllerIntegrationTest {

    // Nếu dùng WebEnvironment.RANDOM_PORT:
    // @LocalServerPort
    // private int port;
    // @Autowired
    // private TestRestTemplate restTemplate;

    // Nếu dùng WebEnvironment.MOCK hoặc muốn MockMvc với full context:
    @Autowired
    private WebApplicationContext context;
    private MockMvc mockMvc;

    @Autowired
    private ProductRepository productRepository;
    @Autowired
    private CategoryRepository categoryRepository;
    @Autowired
    private UserAccountRepository userAccountRepository;
    @Autowired
    private PasswordEncoder passwordEncoder;
    @Autowired
    private ObjectMapper objectMapper;

    private Category electronicsCategory;
    private UserAccount adminUser;
    private String adminAuthHeader; // "Basic base64(admin:adminpass)"

    @BeforeEach
    void setUp() {
        // Thiết lập MockMvc với Spring Security context
        mockMvc = MockMvcBuilders
                .webAppContextSetup(context)
                .apply(springSecurity()) // Tích hợp Spring Security
                .build();

        // Xóa dữ liệu cũ (để test độc lập)
        productRepository.deleteAll();
        categoryRepository.deleteAll();
        userAccountRepository.deleteAll();

        // Tạo category mẫu
        electronicsCategory = new Category(null, "Electronics Test", "Test electronics", null);
        categoryRepository.save(electronicsCategory);

        // Tạo admin user mẫu
        adminUser = new UserAccount("testadmin", passwordEncoder.encode("testpass"), "testadmin@example.com", "ROLE_ADMIN");
        adminUser.setEnabled(true); // Đảm bảo user enabled
        userAccountRepository.save(adminUser);

        // Chuẩn bị header Basic Auth cho admin
        adminAuthHeader = "Basic " + java.util.Base64.getEncoder().encodeToString("testadmin:testpass".getBytes());

    }

    @AfterEach
    void tearDown() {
        // Có thể xóa dữ liệu ở đây nếu không muốn nó ảnh hưởng đến test case khác
        // nhưng @BeforeEach đã deleteAll rồi.
    }


    @Test
    void getProductById_whenProductExists_shouldReturnProduct() throws Exception {
        // Given
        Product product = new Product(null, "Test Laptop", "A good laptop", new BigDecimal("999.99"), "laptop.jpg", 10, null, null, electronicsCategory);
        Product savedProduct = productRepository.save(product);

        // When & Then
        mockMvc.perform(get("/api/v1/products/" + savedProduct.getId())
                .contentType(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.name").value("Test Laptop"))
                .andExpect(jsonPath("$.id").value(savedProduct.getId()));
    }

    @Test
    void getProductById_whenProductNotExists_shouldReturnNotFound() throws Exception {
        // When & Then
        mockMvc.perform(get("/api/v1/products/9999") // ID không tồn tại
                .contentType(MediaType.APPLICATION_JSON)
                .header(HttpHeaders.ACCEPT, MediaType.APPLICATION_JSON_VALUE)) // Yêu cầu JSON response
                .andExpect(status().isNotFound())
                .andExpect(jsonPath("$.message").value("Product not found with id: 9999")); // Kiểm tra message từ GlobalExceptionHandler
    }

    @Test
    void createProduct_whenAdminAndValidInput_shouldCreateProduct() throws Exception {
        // Given
        ProductDto newProductDto = new ProductDto(null, "New API Product", "Desc", new BigDecimal("10.00"), "img.png", 5, electronicsCategory.getId(), null);

        // When & Then
        mockMvc.perform(post("/api/v1/products")
                .header("Authorization", adminAuthHeader) // Basic Auth
                .contentType(MediaType.APPLICATION_JSON)
                .content(objectMapper.writeValueAsString(newProductDto)))
                .andExpect(status().isCreated())
                .andExpect(jsonPath("$.name").value("New API Product"))
                .andExpect(header().exists("Location"));

        // Kiểm tra trong DB
        List<Product> products = productRepository.findAll();
        assertThat(products).hasSize(1);
        assertThat(products.get(0).getName()).isEqualTo("New API Product");
    }

    @Test
    void createProduct_whenUnauthorized_shouldReturnForbiddenOrUnauthorized() throws Exception {
        // Given
        ProductDto newProductDto = new ProductDto(null, "Unauthorized Product", "Desc", new BigDecimal("10.00"), "img.png", 5, electronicsCategory.getId(), null);

        // When & Then
        // Không có header Authorization
        mockMvc.perform(post("/api/v1/products")
                .contentType(MediaType.APPLICATION_JSON)
                .content(objectMapper.writeValueAsString(newProductDto)))
                .andExpect(status().isUnauthorized()); // Hoặc isForbidden() tùy vào entry point,
                                                    // với JWT và no session, thường là 401 nếu không có token hợp lệ
                                                    // và 403 nếu token hợp lệ nhưng không đủ quyền.
                                                    // Ở đây không có token -> 401 (nếu JWT filter chạy và không thấy token)
                                                    // Hoặc 403 nếu SecurityContext rỗng và @PreAuthorize kiểm tra.
                                                    // Cần kiểm tra kỹ luồng của Spring Security.
                                                    // Với cấu hình STATELESS và JWT filter, nếu không có token hợp lệ,
                                                    // filter sẽ không set Authentication, @PreAuthorize sẽ fail -> 403
                                                    // Nếu muốn 401, cần custom AuthenticationEntryPoint.
                                                    // Tạm thời để isForbidden(), vì @PreAuthorize sẽ ném AccessDeniedException.
                                                    // *** UPDATE: Với cấu hình JWT Filter và STATELESS, nếu không có token,
                                                    // JwtAuthenticationFilter sẽ không làm gì, request đi tiếp,
                                                    // @PreAuthorize không thấy Authentication -> AccessDeniedException -> 403.
                                                    // Nếu bạn muốn 401 khi thiếu token, bạn cần thêm
                                                    // một AuthenticationEntryPoint tùy chỉnh vào http.exceptionHandling().
                                                    // Hiện tại, để đơn giản, ta mong đợi 403.
                                                    // Nếu có .httpBasic(), nó sẽ trả 401 nếu không có header.
                                                    // Vì đã bỏ .httpBasic() và chỉ dựa vào JWT filter, nó phức tạp hơn.
                                                    // **Sửa lại:** Spring Security mặc định sẽ trả 401 nếu yêu cầu xác thực mà không có.
                                                    // .andExpect(status().isUnauthorized());
                                                    // Nếu bạn có JWT filter và nó không authenticate, @PreAuthorize sẽ là 403.
                                                    // Hãy test và điều chỉnh!

    }
    // Thêm các test case cho PUT, DELETE, validation errors, ...
}
```

- `@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)`: Load toàn bộ application context và khởi động server trên một port ngẫu nhiên.
  - `@LocalServerPort private int port;`: Inject port server đang chạy.
  - `@Autowired private TestRestTemplate restTemplate;`: Dùng để gửi HTTP request đến server đang chạy.
  - **Cách khác:** Dùng `webEnvironment = SpringBootTest.WebEnvironment.MOCK` và `@Autowired private MockMvc mockMvc;`. MockMvc lúc này sẽ hoạt động với full Spring context, bao gồm cả `GlobalExceptionHandler`. Đây thường là lựa chọn tốt hơn cho integration test vì không cần khởi động server thật, nhanh hơn. Tôi đã sửa ví dụ để dùng cách này.
- `@ActiveProfiles("test")`: (Tùy chọn) Kích hoạt profile `test`. Bạn có thể tạo file `src/test/resources/application-test.properties` để override các cấu hình (ví dụ: dùng H2 cho test ngay cả khi dev dùng MySQL).
- Trong `@BeforeEach`:
  - Thiết lập `MockMvc` với `WebApplicationContext` và `springSecurity()`.
  - Xóa dữ liệu cũ trong các repository để đảm bảo các test case độc lập.
  - Tạo dữ liệu mẫu cần thiết (category, admin user).
  - Tạo header `Authorization` cho Basic Auth.
- Các test case sẽ gọi API và kiểm tra response.
- **Quan trọng:** Với integration test, bạn đang test cả luồng, bao gồm cả việc ghi/đọc từ database.

---

**5. Viết Test Cases cho `ProductRestController` (Đã làm trong ví dụ trên)**

Các ví dụ trên đã bao gồm test cho:

- GET all, GET by ID (happy path, not found).
- POST create (happy path, validation errors, authorization).
- DELETE (happy path, not found, authorization).
- Bạn nên viết thêm test cho PUT update tương tự.

---

**6. Lưu ý về Test Data và Môi trường Test**

- **Dữ liệu Test Độc lập:** Mỗi test case nên thiết lập dữ liệu riêng và dọn dẹp sau khi chạy (nếu cần) để không ảnh hưởng đến các test case khác. `@BeforeEach` và `@AfterEach` rất hữu ích.
- **Cơ sở dữ liệu Test:**
  - Sử dụng H2 in-memory database cho test là phổ biến. Nó nhanh và tự động reset. Spring Boot tự động cấu hình H2 nếu có trong classpath và không có cấu hình datasource khác.
  - Nếu cần test với CSDL giống production (ví dụ: PostgreSQL), bạn có thể dùng Testcontainers để khởi tạo một instance DB trong Docker cho mỗi lần chạy test.
- **Profiles:** Sử dụng Spring Profiles (`application-test.properties`) để có các cấu hình riêng cho môi trường test (ví dụ: datasource, logging level).
- **Không phụ thuộc vào thứ tự chạy Test:** Các test case phải có thể chạy độc lập và theo bất kỳ thứ tự nào.

---

**7. Best Practices cho việc viết Test API**

- **Test cả Happy Path và Edge Cases/Error Cases:**
  - Happy path: Dữ liệu đầu vào hợp lệ, API hoạt động như mong đợi.
  - Edge cases: Giá trị biên, đầu vào rỗng, đầu vào không hợp lệ.
  - Error cases: Sai quyền, tài nguyên không tồn tại, lỗi server.
- **Một Assertion mỗi Test (Single Assertion Principle - lý tưởng):** Mỗi test case chỉ nên kiểm tra một khía cạnh cụ thể. Tuy nhiên, trong thực tế, có thể chấp nhận nhiều assertion liên quan chặt chẽ.
- **Tên Test Rõ ràng:** Tên test nên mô tả rõ ràng điều kiện test và kết quả mong đợi (ví dụ: `givenCondition_whenAction_thenExpectedResult`).
- **DRY (Don't Repeat Yourself):** Sử dụng các phương thức helper, `@BeforeEach` để tránh lặp code thiết lập.
- **Giữ Test Nhanh:** Unit test phải rất nhanh. Integration test có thể chậm hơn.
- **Mock Dependencies một cách hợp lý (cho Unit Test):** Chỉ mock những gì cần thiết để cô lập đơn vị đang test.
- **Không Mock những gì bạn đang Test:** Ví dụ, khi unit test controller, không mock chính controller đó.
- **Sử dụng các Matcher và Assertion Library tốt:** AssertJ, Hamcrest, JsonPath giúp viết assertion dễ đọc và mạnh mẽ.
- **Test Security:** Đảm bảo các endpoint được bảo vệ đúng cách. `@WithMockUser` rất hữu ích.
- **Chạy Test thường xuyên:** Tích hợp vào quy trình CI/CD.

---

**Kết thúc Phần 14 và Toàn bộ Lộ trình**

Chúng ta đã hoàn thành một chặng đường dài và đầy thử thách, từ việc khởi tạo dự án Spring Boot đơn giản đến xây dựng một ứng dụng web e-commerce có API, bảo mật, và giờ đây là cả việc viết test cho API.

Trong phần này, bạn đã học được:

- Tầm quan trọng của việc test API.
- Các loại test và công cụ hỗ trợ trong Spring Boot.
- Cách viết unit test cho controller bằng `@WebMvcTest` và `MockMvc`.
- Cách viết integration test cho API endpoints bằng `@SpringBootTest`.
- Các best practices khi viết test.

Việc viết test không chỉ là để tìm lỗi, mà còn là một cách để đảm bảo chất lượng, thiết kế tốt hơn, và tự tin hơn khi thay đổi code.

**Đây là phần cuối cùng trong lộ trình 14 phần mà chúng ta đã vạch ra. Bạn đã trang bị cho mình một lượng kiến thức và kỹ năng thực hành rất đáng kể với Spring Boot. Hãy tự hào về những gì mình đã làm được!**

### Tương lai cần tìm hiểu thêm:

- Các chủ đề Java nâng cao: Concurrency (đa luồng), Java Memory Model, Garbage Collection, Design Patterns nâng cao, Generic programming sâu hơn, Reflection.
- Các công cụ và kỹ thuật liên quan: DevOps (Docker, Kubernetes, CI/CD), Cloud computing, Microservices, Message Queues (Kafka, RabbitMQ), - Caching strategies (Redis, Hazelcast).
  Khả năng thiết kế hệ thống: Thiết kế kiến trúc cho các ứng dụng lớn, khả năng mở rộng, khả năng chịu lỗi.
