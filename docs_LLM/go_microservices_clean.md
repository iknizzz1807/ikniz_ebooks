**Ý tưởng Dự án: "Helios - Hệ thống Giám sát và Phân tích Bất thường Đô thị Thông minh"**

- **Mô tả:** Helios là một hệ thống thu thập, xử lý và phân tích dữ liệu từ các "cảm biến" (có thể giả lập) trong một thành phố thông minh để phát hiện các sự kiện bất thường. Ví dụ:
  - **Giao thông:** Phát hiện ùn tắc đột ngột, tai nạn dựa trên dữ liệu luồng xe, tốc độ trung bình giảm mạnh.
  - **An ninh:** Phát hiện tiếng động lớn bất thường (tiếng súng, kính vỡ giả lập) vào ban đêm ở khu vực vắng vẻ.
  - **Môi trường:** Phát hiện nồng độ ô nhiễm không khí (PM2.5, CO2 giả lập) tăng đột biến.
  - **Năng lượng:** Phát hiện mức tiêu thụ điện bất thường ở một khu vực.
- **Tại sao "wow" và "độc lạ" (ở một mức độ):**
  - Không phải là e-commerce hay blog đơn thuần.
  - Kết hợp yếu tố "smart city" (thành phố thông minh), một chủ đề nóng và có nhiều tiềm năng.
  - Cho phép mô phỏng dữ liệu đa dạng, tạo ra các kịch bản thú vị.
  - Có thể mở rộng với các module phân tích phức tạp hơn (ví dụ: dự đoán, gợi ý giải pháp).
- **Mức độ phức tạp:**
  - **Vừa phải:** Đủ để áp dụng microservices, clean architecture, DI, caching.
  - **Không quá tải:** Chúng ta sẽ tập trung vào kiến trúc và core logic, dữ liệu cảm biến có thể giả lập một cách thông minh.
- **Các microservices tiềm năng:**
  1.  `SensorDataIngestor`: Tiếp nhận dữ liệu từ các cảm biến (qua HTTP/gRPC).
  2.  `AnomalyDetectionEngine`: Xử lý dữ liệu, áp dụng quy tắc/thuật toán (đơn giản ban đầu) để phát hiện bất thường.
  3.  `NotificationService`: Gửi cảnh báo (ví dụ: qua email, webhook, hoặc log ra console).
  4.  `DashboardAPI`: Cung cấp API cho một giao diện (frontend không cần làm) để truy vấn trạng thái, lịch sử bất thường.
- **Công nghệ áp dụng:** Go, Clean Architecture, DI, REST/gRPC, Redis (cho caching, rate limiting, hoặc lưu trữ tạm thời), cơ sở dữ liệu (ví dụ: PostgreSQL) cho dữ liệu lâu dài.

Nếu bạn thấy ý tưởng dự án này phù hợp, chúng ta sẽ bắt đầu với Phần 1.

---

**PHẦN 1: NỀN TẢNG GO VỮNG CHẮC – TƯ DUY VÀ CÔNG CỤ CHO KỸ SƯ HIỆN ĐẠI**

- **Tên phần học:** Nền tảng Go vững chắc – Tư duy và Công cụ cho Kỹ sư Hiện đại
- **Mục tiêu học phần:**

  - Hiểu rõ triết lý thiết kế của Go và tại sao nó phù hợp với microservices và các hệ thống hiện đại.
  - Nắm vững các khái niệm cốt lõi của Go không chỉ ở mức cú pháp mà còn ở cách chúng hỗ trợ xây dựng phần mềm tốt: structs, interfaces, error handling, và concurrency primitives.
  - Xây dựng tư duy "Go " (Go idiomatic) ngay từ đầu, làm tiền đề cho việc viết code sạch và kiến trúc tốt.
  - Hiểu cách Go quản lý dependencies và tổ chức project.

- **Giải thích lý thuyết kỹ càng:**

  1.  **Triết lý Thiết kế của Go và Sự Phù hợp với Microservices:**

      - **Simplicity (Đơn giản):** Go có cú pháp nhỏ gọn, ít từ khóa, và tránh các tính năng phức tạp không cần thiết (như kế thừa class, generics phức tạp trước 1.18, exceptions).
        - _Tại sao quan trọng?_ Trong microservices, bạn có nhiều dịch vụ nhỏ. Sự đơn giản giúp dễ dàng phát triển, bảo trì, và onboarding thành viên mới cho từng service. Code dễ đọc, dễ hiểu giảm thiểu lỗi và tăng tốc độ phát triển.
      - **Concurrency (Đồng thời):** Goroutines và channels là công cụ hạng nhất (first-class citizen) trong Go. Chúng cho phép xử lý đồng thời một cách nhẹ nhàng và hiệu quả.
        - _Tại sao quan trọng?_ Microservices thường xuyên phải xử lý nhiều request cùng lúc, giao tiếp với các service khác, hoặc thực hiện các tác vụ I/O bound. Goroutines nhẹ hơn threads truyền thống, cho phép tạo hàng ngàn, thậm chí hàng triệu goroutines mà không tốn nhiều tài nguyên. Channels cung cấp cơ chế giao tiếp an toàn giữa các goroutines, tránh data races.
      - **Fast Compilation (Biên dịch nhanh):** Go biên dịch rất nhanh, giúp chu trình feedback của lập trình viên ngắn lại.
        - _Tại sao quan trọng?_ Trong phát triển microservices, việc build và deploy thường xuyên diễn ra. Tốc độ biên dịch nhanh cải thiện năng suất đáng kể.
      - **Statically Typed & Garbage Collected (Kiểu tĩnh & Thu gom rác tự động):** Kiểu tĩnh giúp phát hiện lỗi sớm ở giai đoạn biên dịch. Thu gom rác tự động giảm gánh nặng quản lý bộ nhớ.
        - _Tại sao quan trọng?_ An toàn kiểu và quản lý bộ nhớ tự động giúp xây dựng các service ổn định, ít lỗi runtime liên quan đến kiểu hay memory leaks.
      - **Excellent Standard Library (Thư viện chuẩn mạnh mẽ):** Go cung cấp thư viện chuẩn phong phú, đặc biệt cho networking (HTTP, RPC), I/O, encoding/decoding (JSON, XML).
        - _Tại sao quan trọng?_ Giảm sự phụ thuộc vào thư viện bên thứ ba cho các tác vụ cơ bản, giúp các microservices nhỏ gọn và ít "điểm hỏng" tiềm năng hơn.
      - **Single Binary Deployment (Triển khai bằng một file thực thi duy nhất):** Go biên dịch ra một file thực thi tĩnh, không cần runtime hay dependencies bên ngoài (trừ khi dùng CGO).
        - _Tại sao quan trọng?_ Cực kỳ tiện lợi cho việc đóng gói vào container (Docker) và triển khai microservices.

  2.  **Structs – Khối Xây dựng Dữ liệu:**

      - Trong Go, `struct` là một kiểu dữ liệu phức hợp cho phép nhóm các trường dữ liệu khác nhau lại với nhau. Nó tương tự như `class` không có phương thức trong các ngôn ngữ OOP khác, hoặc `record` / `struct` trong C/C++.
      - _Cách hoạt động low-level:_ Struct là một vùng nhớ liên tục chứa các trường của nó theo thứ tự khai báo (có thể có padding để tối ưu alignment).
      - _Khái niệm thiết kế cấp cao:_ Structs dùng để định nghĩa các thực thể (entities) và đối tượng truyền dữ liệu (DTOs) trong ứng dụng của bạn. Chúng là xương sống của mô hình domain.
        ```go
        // Ví dụ: Định nghĩa một cảm biến trong hệ thống Helios
        type SensorReading struct {
            SensorID  string    `json:"sensor_id"` // Struct tags cho metadata (ví dụ: JSON marshalling)
            Timestamp time.Time `json:"timestamp"`
            Value     float64   `json:"value"`
            Unit      string    `json:"unit"`
        }
        ```
      - _Tư duy:_ Hãy nghĩ về `struct` như là một bản thiết kế cho dữ liệu. Khi bạn muốn biểu diễn một "thứ gì đó" có các thuộc tính, `struct` là lựa chọn hàng đầu.

  3.  **Interfaces – Hợp đồng và Sự Linh hoạt:**

      - `Interface` trong Go định nghĩa một tập hợp các phương thức (method signatures). Bất kỳ kiểu nào (type) triển khai tất cả các phương thức của một interface thì được coi là "thỏa mãn" (satisfies) interface đó, một cách ngầm định (implicitly).
      - _Cách hoạt động low-level:_ Một biến interface thực chất là một cặp con trỏ: một con trỏ tới thông tin về kiểu cụ thể (concrete type) của giá trị được lưu trữ, và một con trỏ tới chính giá trị đó (hoặc con trỏ tới dữ liệu nếu giá trị lớn). Khi một phương thức được gọi trên biến interface, Go runtime sẽ tìm đến bảng phương thức (method table) của kiểu cụ thể để thực thi.
      - _Khái niệm thiết kế cấp cao:_ Interfaces là công cụ mạnh mẽ nhất của Go để đạt được **Polymorphism (Đa hình)** và **Decoupling (Giảm sự phụ thuộc/Khớp nối lỏng)**. Chúng định nghĩa hành vi (behavior) thay vì dữ liệu. Đây là nền tảng của Dependency Injection.

        ```go
        // Ví dụ: Interface cho một service có khả năng lưu trữ dữ liệu đọc từ cảm biến
        type SensorDataStore interface {
            Save(ctx context.Context, reading SensorReading) error
            GetBySensorID(ctx context.Context, sensorID string, limit int) ([]SensorReading, error)
        }

        // Một struct triển khai interface này (ví dụ, lưu vào memory)
        type InMemorySensorStore struct {
            data map[string][]SensorReading
            mu   sync.RWMutex
        }

        func NewInMemorySensorStore() *InMemorySensorStore {
            return &InMemorySensorStore{data: make(map[string][]SensorReading)}
        }

        func (s *InMemorySensorStore) Save(ctx context.Context, reading SensorReading) error {
            s.mu.Lock()
            defer s.mu.Unlock()
            s.data[reading.SensorID] = append(s.data[reading.SensorID], reading)
            fmt.Printf("Saved: %+v\n", reading)
            return nil
        }

        func (s *InMemorySensorStore) GetBySensorID(ctx context.Context, sensorID string, limit int) ([]SensorReading, error) {
            s.mu.RLock()
            defer s.mu.RUnlock()
            readings, ok := s.data[sensorID]
            if !ok {
                return nil, fmt.Errorf("sensor %s not found", sensorID)
            }
            if len(readings) > limit {
                return readings[len(readings)-limit:], nil
            }
            return readings, nil
        }
        ```

        Trong ví dụ trên, `InMemorySensorStore` ngầm định thỏa mãn `SensorDataStore` vì nó có các phương thức `Save` và `GetBySensorID` với đúng chữ ký. Chúng ta có thể có `PostgresSensorStore` hoặc `RedisSensorStore` cũng triển khai interface này.

      - _Tư duy:_ "Accept interfaces, return structs" (Chấp nhận interface, trả về struct) là một câu nói phổ biến. Khi hàm/phương thức của bạn cần một dependency, hãy yêu cầu một interface thay vì một kiểu cụ thể. Điều này giúp code linh hoạt, dễ test và dễ thay thế các thành phần.

  4.  **Error Handling – Xử lý Lỗi một cách Tường minh:**

      - Go không dùng `try-catch` exceptions. Thay vào đó, các hàm có thể trả về lỗi (error) thường là giá trị cuối cùng trong danh sách các giá trị trả về. Kiểu `error` là một interface tích hợp sẵn:
        ```go
        type error interface {
            Error() string
        }
        ```
      - Cách xử lý phổ biến là kiểm tra `if err != nil`.
      - _Tại sao không dùng exceptions?_ Go ưu tiên sự tường minh. Việc trả về lỗi buộc lập trình viên phải suy nghĩ về các trường hợp lỗi và xử lý chúng một cách chủ động. Exceptions có thể làm luồng kiểm soát chương trình trở nên khó theo dõi.
      - _Khái niệm thiết kế cấp cao:_ Xử lý lỗi là một phần quan trọng của logic nghiệp vụ. Bằng cách làm cho nó tường minh, Go khuyến khích xây dựng phần mềm mạnh mẽ (robust).
        ```go
        readings, err := store.GetBySensorID(context.Background(), "temp-sensor-01", 10)
        if err != nil {
            // Xử lý lỗi: log, trả về lỗi cho upstream, hoặc thực hiện hành động khắc phục
            log.Printf("Failed to get sensor readings: %v", err)
            // return err // Nếu hàm này cũng có thể trả về lỗi
        }
        // Tiếp tục xử lý readings nếu không có lỗi
        ```
      - _Tư duy:_ Luôn kiểm tra lỗi. Đừng bỏ qua chúng. Cung cấp ngữ cảnh cho lỗi (wrapping errors) để dễ dàng debug.

  5.  **Concurrency: Goroutines và Channels – Sức mạnh của Sự Đơn giản:**

      - **Goroutines:** Là các hàm hoặc phương thức có thể chạy đồng thời với các hàm/phương thức khác. Chúng cực kỳ nhẹ (khởi tạo với vài KB stack, có thể tăng giảm khi cần) và được quản lý bởi Go runtime, không phải OS threads trực tiếp (mặc dù Go runtime sử dụng OS threads). Tạo goroutine rất đơn giản: `go myFunction()`.
      - **Channels:** Là các "ống" (pipes) đã được định kiểu (typed) mà bạn có thể dùng để gửi và nhận giá trị giữa các goroutines, đảm bảo an toàn trong môi trường đồng thời. "Do not communicate by sharing memory; instead, share memory by communicating." (Đừng giao tiếp bằng cách chia sẻ bộ nhớ; thay vào đó, hãy chia sẻ bộ nhớ bằng cách giao tiếp.)
      - _Cách hoạt động low-level (sơ lược):_ Go runtime có một scheduler để phân phối các goroutines lên một số lượng nhỏ OS threads (M:N scheduling). Channels thường được triển khai với locks và hàng đợi nội bộ để đảm bảo đồng bộ hóa.
      - _Khái niệm thiết kế cấp cao:_ Goroutines và channels cung cấp một mô hình lập trình đồng thời dễ hiểu và mạnh mẽ, phù hợp với các tác vụ I/O-bound (như gọi API, đọc/ghi file/DB) và CPU-bound (nếu biết cách chia nhỏ công việc).

        ```go
        func processSensorData(data SensorReading, resultChan chan<- string) {
            // Giả lập xử lý tốn thời gian
            time.Sleep(100 * time.Millisecond)
            result := fmt.Sprintf("Processed sensor %s with value %.2f", data.SensorID, data.Value)
            resultChan <- result // Gửi kết quả vào channel
        }

        func main_concurrency_example() {
            readings := []SensorReading{
                {SensorID: "A", Value: 10.5},
                {SensorID: "B", Value: 12.3},
                {SensorID: "C", Value: 9.8},
            }
            resultChan := make(chan string, len(readings)) // Buffered channel

            for _, r := range readings {
                go processSensorData(r, resultChan) // Khởi chạy goroutine cho mỗi reading
            }

            // Thu thập kết quả
            for i := 0; i < len(readings); i++ {
                fmt.Println(<-resultChan)
            }
            close(resultChan)
        }
        ```

      - _Tư duy:_ Sử dụng goroutines để thực hiện các tác vụ độc lập song song. Sử dụng channels để điều phối và giao tiếp giữa chúng một cách an toàn. Đây sẽ là nền tảng khi xây dựng các microservices có khả năng chịu tải cao.

  6.  **Packages và Modules – Tổ chức Code:**
      - **Packages:** Là cách Go tổ chức code. Mỗi thư mục chứa các file Go thường là một package. Tên package được khai báo ở đầu mỗi file (`package mypackage`). Các định danh (biến, hằng, kiểu, hàm) viết hoa chữ cái đầu sẽ được export (public), còn viết thường là unexported (private với package).
      - **Modules:** Kể từ Go 1.11, modules là cơ chế quản lý dependencies. Một module là một tập hợp các package được phiên bản hóa cùng nhau. File `go.mod` ở thư mục gốc của project định nghĩa module path, phiên bản Go, và các dependencies.
      - _Tại sao quan trọng?_ Giúp tái sử dụng code, quản lý không gian tên, và kiểm soát "visibility" của code. Modules giải quyết vấn đề quản lý phiên bản dependency một cách hiệu quả.
      - _Tư duy:_ Tổ chức code thành các package có mục đích rõ ràng. Đặt tên package ngắn gọn, dễ hiểu. Sử dụng modules để quản lý project và các thư viện bên ngoài.

- **Code minh họa / sơ đồ:**

  - Các ví dụ code đã được lồng vào phần giải thích ở trên.
  - **Sơ đồ minh họa Interface (Conceptual):**

    ```
    +-------------------+      implements      +-----------------------+
    | Interface:        | <------------------ | Struct:               |
    | SensorDataStore   |                     | InMemorySensorStore   |
    |-------------------|                     |-----------------------|
    | Save() error      |                     | data: map[...]        |
    | GetByID() ([]D,e) |                     | mu: sync.RWMutex      |
    +-------------------+                     +-----------------------+
                                              | Save() error          |
                                              | GetByID() ([]D,e)     |
                                              +-----------------------+

    +-------------------+      implements      +-----------------------+
    | Interface:        | <------------------ | Struct:               |
    | SensorDataStore   |                     | PostgresSensorStore   |
    |-------------------|                     |-----------------------|
    | Save() error      |                     | db: *sql.DB           |
    | GetByID() ([]D,e) |                     +-----------------------+
    +-------------------+                     | Save() error          |
                                              | GetByID() ([]D,e)     |
                                              +-----------------------+

    Application Code:
    var store SensorDataStore // Can hold InMemorySensorStore OR PostgresSensorStore
    store = NewInMemorySensorStore()
    // or
    // store = NewPostgresSensorStore(dbConn)
    store.Save(...)
    ```

    Sơ đồ này minh họa cách `SensorDataStore` là một "hợp đồng". Bất kỳ ai triển khai nó (ví dụ: `InMemorySensorStore`, `PostgresSensorStore`) đều có thể được sử dụng thay thế cho nhau ở những nơi cần `SensorDataStore`.

- **Best practices:**

  1.  **Small Interfaces (Nguyên lý Phân tách Interface - ISP):** Ưu tiên các interface nhỏ, có mục đích cụ thể. Một kiểu có thể triển khai nhiều interface nhỏ. Ví dụ, thay vì một `FileHandler` có `Read`, `Write`, `Seek`, `Close`, có thể tách thành `io.Reader`, `io.Writer`, `io.Seeker`, `io.Closer`.
      - _Tại sao?_ Tăng tính linh hoạt và khả năng tái sử dụng. Client chỉ cần phụ thuộc vào những gì nó thực sự cần.
  2.  **Explicit Error Handling:** Luôn kiểm tra lỗi trả về. Cung cấp ngữ cảnh cho lỗi (ví dụ, sử dụng `fmt.Errorf("verb with args: %w", err)` với Go 1.13+ để wrapping errors).
      - _Tại sao?_ Giúp gỡ lỗi dễ dàng hơn và làm cho chương trình mạnh mẽ hơn.
  3.  **Favor Composition over Inheritance:** Go không có kế thừa class. Thay vào đó, sử dụng struct embedding để "vay mượn" hành vi, hoặc dùng interfaces để đạt được đa hình.
      - _Tại sao?_ Composition thường linh hoạt hơn và tránh được các vấn đề của hệ thống phân cấp class phức tạp (ví dụ: "fragile base class problem").
  4.  **Idiomatic Go Naming Conventions:**
      - Package names: ngắn gọn, chữ thường, single-word (ví dụ: `http`, `json`, `iot`).
      - Biến, hàm, kiểu export: `MixedCaps` hoặc `mixedCaps` (CamelCase). Chữ cái đầu viết hoa để export.
      - Interface names: thường kết thúc bằng `er` nếu chỉ có một phương thức (ví dụ: `Reader`, `Writer`). Hoặc mô tả rõ ràng vai trò (ví dụ: `SensorDataStore`).
      - _Tại sao?_ Tính nhất quán giúp code dễ đọc và dễ hiểu hơn trong cộng đồng Go.
  5.  **Clear Package Structure:** Tổ chức code thành các package có trách nhiệm rõ ràng. Tránh circular dependencies.
      - _Tại sao?_ Dễ bảo trì, dễ hiểu, và dễ mở rộng.

- **Anti-patterns / lỗi phổ biến:**

  1.  **Ignoring Errors:** `value, _ := someFunction()` mà không kiểm tra `err`.
      - _Hậu quả:_ Chương trình có thể tiếp tục chạy với trạng thái không hợp lệ, dẫn đến panic hoặc hành vi sai lệch khó gỡ lỗi.
  2.  **Fat Interfaces:** Tạo ra các interface quá lớn với nhiều phương thức không liên quan.
      - _Hậu quả:_ Buộc các implementer phải triển khai các phương thức mà chúng không cần, vi phạm ISP, giảm tính linh hoạt.
  3.  **Overuse of Global Variables:** Đặc biệt là các biến có thể thay đổi trạng thái.
      - _Hậu quả:_ Gây ra coupling ngầm, khó test, và tiềm ẩn nguy cơ data race trong môi trường đồng thời nếu không được bảo vệ đúng cách.
  4.  **Unnecessary Goroutine Spawning / Leaking Goroutines:** Tạo goroutine mà không có cơ chế quản lý vòng đời của nó (ví dụ, không có cách để dừng hoặc không chờ nó hoàn thành).
      - _Hậu quả:_ Lãng phí tài nguyên, có thể dẫn đến cạn kiệt bộ nhớ hoặc CPU.
  5.  **Misunderstanding Channel Behavior:**
      - Gửi vào channel đã đóng (gây panic).
      - Đọc từ channel đã đóng và rỗng (trả về zero value của kiểu, không block).
      - Sử dụng unbuffered channel và gây deadlock nếu không có goroutine sẵn sàng nhận/gửi.
      - _Hậu quả:_ Deadlocks, panics, hoặc hành vi không mong muốn.

- **So sánh các lựa chọn / cách tiếp cận (nếu có):**

  1.  **Error Handling: Go `error` vs. Exceptions (Java, Python, C#):**
      - **Go `error`:**
        - _Ưu điểm:_ Tường minh, luồng kiểm soát rõ ràng, lỗi là giá trị thông thường. Khuyến khích xử lý lỗi tại chỗ.
        - _Nhược điểm:_ Code có thể dài dòng hơn với các khối `if err != nil` lặp đi lặp lại.
      - **Exceptions:**
        - _Ưu điểm:_ Code có thể gọn hơn cho "happy path", lỗi có thể "bubble up" qua nhiều tầng gọi hàm.
        - _Nhược điểm:_ Luồng kiểm soát có thể khó theo dõi. Dễ bỏ qua việc xử lý exception. Performance overhead.
      - _Tại sao Go chọn `error`?_ Phù hợp với triết lý đơn giản và tường minh. Buộc lập trình viên phải đối mặt với lỗi.
  2.  **Concurrency: Goroutines/Channels vs. Threads/Locks (Java, C++):**
      - **Goroutines/Channels (Go):**
        - _Ưu điểm:_ Nhẹ hơn threads, dễ tạo và quản lý hàng ngàn/triệu goroutines. Channels cung cấp mô hình giao tiếp an toàn, dễ lý giải hơn so với locks phức tạp. Giảm nguy cơ deadlock/race condition nếu dùng đúng.
        - _Nhược điểm:_ Cần hiểu rõ mô hình CSP (Communicating Sequential Processes). Có thể có overhead nếu lạm dụng channels cho các tác vụ quá nhỏ.
      - **Threads/Locks:**
        - _Ưu điểm:_ Mô hình quen thuộc với nhiều lập trình viên. OS hỗ trợ trực tiếp.
        - _Nhược điểm:_ Threads nặng hơn, tạo nhiều threads tốn tài nguyên. Quản lý locks, mutexes, semaphores phức tạp, dễ gây deadlock, race condition.
      - _Tại sao Go chọn Goroutines/Channels?_ Cung cấp một cách tiếp cận đồng thời ở mức độ trừu tượng cao hơn, an toàn hơn và phù hợp với các hệ thống mạng hiện đại.

- **Gợi ý mở rộng kiến thức:**
  - **Sách:**
    - "The Go Programming Language" (Donovan & Kernighan) - Chương 1-6 là nền tảng tuyệt vời.
    - "Effective Go" - Tài liệu chính thức từ Golang team về cách viết code Go idiomatic. (https://go.dev/doc/effective_go)
    - "Go in Practice" (Butcher & Farina) - Nhiều ví dụ thực tế.
  - **Tài liệu trực tuyến:**
    - A Tour of Go (https://go.dev/tour/) - Thực hành tương tác.
    - Go by Example (https://gobyexample.com/) - Các ví dụ ngắn gọn.
  - **Chủ đề nâng cao liên quan (sẽ học sau, nhưng nên biết):**
    - Context package: Quản lý cancellation, deadlines, và request-scoped values. Rất quan trọng trong microservices.
    - Reflection (`reflect` package): Khả năng kiểm tra và sửa đổi các biến tại runtime. Dùng cẩn thận.
    - CGO: Gọi code C từ Go và ngược lại.

---

Phần 1 này tập trung vào việc tái khẳng định và đào sâu các khái niệm cơ bản của Go, nhưng dưới góc độ "tại sao" chúng lại quan trọng và làm thế nào chúng hỗ trợ các mục tiêu lớn hơn của chúng ta (microservices, clean architecture). Khi bạn đã thấy thoải mái với nội dung này, chúng ta sẽ chuyển sang Phần 2, tập trung vào Clean Architecture.

Tuyệt vời, chúng ta sẽ tiếp tục với **PHẦN 2: CLEAN ARCHITECTURE – XÂY DỰNG NỀN MÓNG CHO HỆ THỐNG BỀN VỮNG VÀ LINH HOẠT.**

- **Tên phần học:** Clean Architecture – Xây dựng Nền móng cho Hệ thống Bền vững và Linh hoạt
- **Mục tiêu học phần:**

  - Hiểu sâu sắc triết lý và các thành phần cốt lõi của Clean Architecture (CA).
  - Nắm vững "The Dependency Rule" (Quy tắc Phụ thuộc) và cách nó giúp tạo ra các hệ thống dễ bảo trì, dễ kiểm thử và độc lập với các yếu tố bên ngoài (framework, DB, UI).
  - Biết cách cấu trúc một ứng dụng Go theo Clean Architecture, phân chia trách nhiệm rõ ràng giữa các lớp.
  - Nhận diện được khi nào và tại sao nên áp dụng Clean Architecture cho dự án microservices.
  - Hiểu cách dữ liệu và control flow di chuyển giữa các lớp.

- **Giải thích lý thuyết kỹ càng:**

  1.  **Giới thiệu Clean Architecture (CA):**

      - Clean Architecture là một mẫu thiết kế kiến trúc phần mềm được đề xuất bởi Robert C. Martin (Uncle Bob). Mục tiêu chính của nó là **phân tách các mối quan tâm (Separation of Concerns)** để tạo ra các hệ thống:
        - **Độc lập với Framework (Independent of Frameworks):** Kiến trúc không phụ thuộc vào sự tồn tại của một thư viện/framework nào đó. Điều này cho phép bạn sử dụng framework như một công cụ, thay vì để hệ thống bị "nhốt" vào nó.
        - **Dễ kiểm thử (Testable):** Các quy tắc nghiệp vụ (business rules) có thể được kiểm thử mà không cần UI, Database, Web Server, hay bất kỳ yếu tố bên ngoài nào khác.
        - **Độc lập với UI (Independent of UI):** UI có thể thay đổi dễ dàng (web, console, mobile app) mà không làm thay đổi phần còn lại của hệ thống.
        - **Độc lập với Database (Independent of Database):** Bạn có thể hoán đổi Oracle hay SQL Server, MongoDB hay CouchDB, mà không ảnh hưởng đến các quy tắc nghiệp vụ.
        - **Độc lập với bất kỳ tác nhân bên ngoài nào (Independent of any external agency):** Thực tế, các quy tắc nghiệp vụ của bạn đơn giản là không biết gì về thế giới bên ngoài.
      - _Tại sao CA quan trọng cho microservices?_ Mỗi microservice là một ứng dụng nhỏ. Việc áp dụng CA giúp từng service có cấu trúc rõ ràng, dễ phát triển độc lập, dễ thay thế các thành phần công nghệ (ví dụ: đổi DB, đổi message broker) mà không ảnh hưởng đến logic cốt lõi. Điều này đặc biệt quan trọng khi bạn có nhiều services do các team khác nhau phát triển.

  2.  **Các Layer (Vòng tròn đồng tâm):**
      Clean Architecture thường được minh họa bằng các vòng tròn đồng tâm. Mỗi vòng tròn đại diện cho một khu vực khác nhau của phần mềm.

      - **Sơ đồ (Conceptual):**
        ```
                 ----------------------------------------------------
                |                 Frameworks & Drivers             |  <-- Lớp ngoài cùng
                |  (Web, UI, DB, Devices, External Interfaces)     |      (Chi tiết cụ thể)
                |----------------------------------------------------|
                |           Interface Adapters                     |
                |  (Controllers, Presenters, Gateways/Repositories)|
                |----------------------------------------------------|
                |             Use Cases (Interactors)              |
                |  (Application-specific Business Rules)           |
                |----------------------------------------------------|
                |                  Entities                        |  <-- Lớp trong cùng
                |  (Enterprise-wide Business Rules)                |      (Trừu tượng nhất)
                 ----------------------------------------------------
        ```
      - **Entities (Thực thể):**
        - Đây là lớp trong cùng. Chúng đóng gói các **quy tắc nghiệp vụ cốt lõi của toàn doanh nghiệp (enterprise-wide business rules)**. Một Entity có thể là một đối tượng với các phương thức, hoặc một tập hợp các cấu trúc dữ liệu và hàm.
        - Chúng ít thay đổi nhất khi có thay đổi ở các lớp ngoài. Ví dụ, nếu bạn thay đổi trang web hoặc cơ sở dữ liệu, các Entity không nên bị ảnh hưởng.
        - _Ví dụ cho Helios:_ `SensorType` (định nghĩa loại cảm biến, ngưỡng bình thường), `AnomalyRule` (quy tắc phát hiện bất thường), `DetectedEvent` (một sự kiện bất thường đã được phát hiện).
      - **Use Cases (Trường hợp sử dụng / Interactors):**
        - Lớp này chứa **quy tắc nghiệp vụ cụ thể của ứng dụng (application-specific business rules)**. Chúng điều phối dòng dữ liệu đến và đi từ Entities, và chỉ đạo Entities sử dụng các quy tắc nghiệp vụ cốt lõi để đạt được mục tiêu của use case.
        - Những thay đổi ở lớp này không nên ảnh hưởng đến Entities. Tuy nhiên, những thay đổi đối với hoạt động của ứng dụng sẽ ảnh hưởng đến Use Cases.
        - _Ví dụ cho Helios:_ `IngestSensorDataUseCase` (tiếp nhận dữ liệu cảm biến và lưu trữ), `DetectAnomalyUseCase` (phân tích dữ liệu dựa trên `AnomalyRule` để tạo ra `DetectedEvent`), `NotifySubscribersUseCase` (thông báo khi có `DetectedEvent`).
      - **Interface Adapters (Bộ điều hợp Giao diện):**
        - Lớp này là một tập hợp các bộ điều hợp (adapters) chuyển đổi dữ liệu từ định dạng thuận tiện nhất cho Use Cases và Entities, sang định dạng thuận tiện nhất cho một số tác nhân bên ngoài như Database hoặc Web. Ngược lại, dữ liệu từ các tác nhân bên ngoài cũng được chuyển đổi thành dạng mà Use Cases và Entities có thể sử dụng.
        - Ví dụ:
          - **Controllers/Handlers:** Nhận input từ bên ngoài (ví dụ: HTTP request từ Web Framework), chuyển đổi nó thành một request model phù hợp cho Use Case, và gọi Use Case.
          - **Presenters/Views:** Nhận output (response model) từ Use Case, chuyển đổi nó thành định dạng phù hợp để hiển thị (ví dụ: JSON cho HTTP response, HTML).
          - **Gateways (Thường là Repositories):** Đây là một điểm rất quan trọng. Use Cases cần truy cập dữ liệu (ví dụ: từ DB, từ một service khác). Tuy nhiên, Use Cases không được biết về DB cụ thể. Thay vào đó, Use Cases sẽ định nghĩa một _interface_ (ví dụ: `SensorDataRepository` với các phương thức `Save`, `FindBySensorID`). Implementation của interface này (ví dụ: `PostgresSensorDataRepository`) sẽ nằm ở lớp ngoài (Frameworks & Drivers) và được "tiêm" (inject) vào Use Case.
      - **Frameworks & Drivers:**
        - Lớp ngoài cùng nhất. Thường bao gồm các framework và công cụ như Database (PostgreSQL, MongoDB), Web Framework (Gin, Echo), UI, thiết bị, v.v.
        - Nơi này chứa các chi tiết cụ thể, "keo dán" (glue code) để kết nối các lớp lại với nhau.
        - Thông thường, bạn không viết nhiều code ở lớp này ngoài code để giao tiếp với các vòng tròn bên trong.

  3.  **The Dependency Rule (Quy tắc Phụ thuộc):**

      - Đây là quy tắc quan trọng nhất, là trái tim của Clean Architecture.
      - **"Source code dependencies can only point inwards."** (Các phụ thuộc mã nguồn chỉ có thể trỏ vào trong.)
      - Điều này có nghĩa là:
        - Không có gì ở một vòng tròn bên trong có thể biết bất cứ điều gì về một vòng tròn bên ngoài.
        - Cụ thể hơn, tên của các biến, hàm, lớp, hoặc bất kỳ thực thể nào được khai báo ở một vòng tròn bên ngoài không được đề cập bởi code ở một vòng tròn bên trong.
        - Tương tự, các cấu trúc dữ liệu được sử dụng ở một vòng tròn bên ngoài không nên được truyền qua ranh giới vào các vòng tròn bên trong.
      - _Tại sao quy tắc này quan trọng?_ Nó đảm bảo rằng logic nghiệp vụ cốt lõi (Entities, Use Cases) không bị "ô nhiễm" bởi các chi tiết của công nghệ cụ thể (DB, Web Framework). Điều này làm cho logic nghiệp vụ trở nên độc lập, dễ kiểm thử và dễ dàng thích ứng với sự thay đổi công nghệ.

  4.  **Crossing Boundaries (Vượt qua Ranh giới):**

      - Khi dữ liệu cần di chuyển qua các ranh giới của các vòng tròn, ví dụ từ Controller vào Use Case, hoặc từ Use Case ra Presenter.
      - **Dependency Inversion Principle (DIP):** Đây là cơ chế chính để tuân thủ The Dependency Rule khi giao tiếp giữa các lớp.
        - Thay vì lớp trong (ví dụ: Use Case) phụ thuộc trực tiếp vào lớp ngoài (ví dụ: một class cụ thể để truy cập DB), lớp trong sẽ định nghĩa một _interface_ (hợp đồng).
        - Lớp ngoài sẽ _triển khai (implement)_ interface đó.
        - Trong quá trình chạy, một instance của lớp triển khai (từ lớp ngoài) sẽ được truyền (inject) vào lớp trong.
        - Sơ đồ: `Use Case --> <interface> <-- Concrete DB Implementation`
      - **Data Transfer Objects (DTOs):** Khi dữ liệu đi qua các ranh giới, thường nên sử dụng các cấu trúc dữ liệu đơn giản, không có hành vi (plain data structures). Chúng được gọi là Data Transfer Objects.
        - _Tại sao?_ Tránh việc truyền các đối tượng "nặng" hoặc các đối tượng gắn liền với một framework cụ thể (ví dụ: không truyền đối tượng `http.Request` của Go vào sâu trong Use Case). DTOs giúp tách biệt rõ ràng. Ví dụ: Controller nhận `http.Request`, chuyển đổi thành `CreateSensorReadingRequestDTO`, rồi truyền DTO này cho Use Case. Use Case trả về `SensorReadingResponseDTO`, Presenter chuyển DTO này thành JSON response.

  5.  **Lợi ích của Clean Architecture (Tóm tắt lại):**

      - **Testability:** Các Use Cases và Entities có thể được unit test mà không cần đến cơ sở dữ liệu, web server, hay bất kỳ thành phần ngoại vi nào. Bạn chỉ cần mock các interface mà chúng phụ thuộc.
      - **Maintainability:** Logic nghiệp vụ được tách biệt và bảo vệ khỏi những thay đổi ở các lớp ngoài. Dễ dàng tìm và sửa lỗi.
      - **Flexibility:** Dễ dàng thay đổi hoặc nâng cấp các thành phần công nghệ (web framework, DB, UI) mà không làm ảnh hưởng đến cốt lõi của ứng dụng.
      - **Scalability (về mặt phát triển):** Các team khác nhau có thể làm việc trên các lớp khác nhau một cách độc lập hơn.

  6.  **Áp dụng vào dự án Helios (Sơ bộ):**
      - **Entities:**
        - `Sensor`: Định nghĩa thông tin về một cảm biến (ID, Tên, Loại, Đơn vị).
        - `SensorReading`: Dữ liệu đọc từ cảm biến (SensorID, Timestamp, Value).
        - `AnomalyRule`: Quy tắc để xác định bất thường (ví dụ: Loại cảm biến, Ngưỡng trên, Ngưỡng dưới, Khoảng thời gian xem xét).
        - `DetectedAnomaly`: Một sự kiện bất thường đã được phát hiện (RuleID, SensorID, Timestamp, Giá trị gây bất thường, Mức độ nghiêm trọng).
      - **Use Cases:**
        - `RegisterSensorUseCase(input RegisterSensorInput) (SensorOutput, error)`
        - `IngestSensorReadingUseCase(input IngestReadingInput) error`
        - `ProcessDataAndDetectAnomaliesUseCase(sensorID string, windowTime time.Duration) ([]DetectedAnomalyOutput, error)`
        - `GetAnomaliesUseCase(filter GetAnomaliesFilter) ([]DetectedAnomalyOutput, error)`
      - **Interface Adapters:**
        - **Controllers (ví dụ: HTTP Handlers trong `SensorDataIngestor` service):**
          - `SensorReadingHTTPHandler`: Nhận POST request chứa dữ liệu cảm biến, gọi `IngestSensorReadingUseCase`.
          - `AnomalyQueryHTTPHandler`: Nhận GET request để truy vấn các bất thường, gọi `GetAnomaliesUseCase`.
        - **Gateways (Interfaces được định nghĩa bởi Use Cases):**
          - `SensorRepository`: `SaveSensor(Sensor) error`, `GetSensorByID(string) (Sensor, error)`
          - `SensorReadingRepository`: `SaveReading(SensorReading) error`, `GetReadingsBySensorIDInRange(string, time.Time, time.Time) ([]SensorReading, error)`
          - `AnomalyRuleRepository`: `GetActiveRulesBySensorType(string) ([]AnomalyRule, error)`
          - `DetectedAnomalyRepository`: `SaveDetectedAnomaly(DetectedAnomaly) error`, `FindAnomalies(Filter) ([]DetectedAnomaly, error)`
        - **Presenters:** (Có thể đơn giản là việc Controller định dạng dữ liệu trả về từ Use Case thành JSON/XML).
      - **Frameworks & Drivers:**
        - Web Framework: Gin, Echo, hoặc `net/http` chuẩn.
        - Database Drivers: `pgx` cho PostgreSQL, `go-redis` cho Redis.
        - Messaging Client (nếu `NotificationService` dùng message queue): Kafka client, RabbitMQ client.

- **Code minh họa / sơ đồ:**

  **Cấu trúc thư mục dự kiến cho một microservice (ví dụ: `AnomalyDetectionEngine`):**

  ```
  anomaly_detection_engine/
  ├── cmd/
  │   └── server/
  │       └── main.go          // Khởi tạo dependencies, start server
  ├── internal/
  │   ├── entity/
  │   │   ├── anomaly.go
  │   │   └── sensor.go
  │   ├── usecase/
  │   │   ├── detect_anomaly.go
  │   │   ├── dto.go             // DTOs cho use cases
  │   │   └── interfaces.go      // Interfaces cho Repositories (Gateways)
  │   ├── adapter/
  │   │   ├── handler/           // HTTP Handlers (Controllers)
  │   │   │   └── anomaly_handler.go
  │   │   ├── repository/        // Implementations của Repository Interfaces
  │   │   │   ├── postgres_anomaly_repo.go
  │   │   │   └── redis_sensor_cache_repo.go // Có thể là decorator cho 1 repo khác
  │   │   └── presenter/
  │   │       └── json_presenter.go
  │   ├── infrastructure/
  │   │   ├── database/
  │   │   │   └── postgres.go    // Kết nối, migration DB
  │   │   ├── cache/
  │   │   │   └── redis.go       // Kết nối Redis
  │   │   └── router/
  │   │       └── router.go      // Định nghĩa routes cho web framework
  ├── go.mod
  └── go.sum
  ```

  _Lưu ý:_ `internal/` là một quy ước của Go, các package trong `internal/` chỉ có thể được import bởi code bên trong cùng một module gốc (ở đây là `anomaly_detection_engine`). Điều này giúp củng cố sự tách biệt.

  **Ví dụ về Interface (Gateway) và Use Case:**

  ```go
  // internal/entity/anomaly.go
  package entity

  import "time"

  type DetectedAnomaly struct {
      ID          string
      SensorID    string
      RuleID      string
      Timestamp   time.Time
      ActualValue float64
      Details     string
  }

  // internal/usecase/interfaces.go
  package usecase

  import (
      "context"
      "time"
      "project-helios/internal/entity" // Quan trọng: Use case phụ thuộc vào Entity
  )

  // DetectedAnomalyRepository là một Gateway Interface
  type DetectedAnomalyRepository interface {
      Save(ctx context.Context, anomaly *entity.DetectedAnomaly) error
      FindBySensorIDAndTimestampRange(ctx context.Context, sensorID string, start, end time.Time) ([]*entity.DetectedAnomaly, error)
  }

  // SensorReadingRepository là một Gateway Interface
  type SensorReadingRepository interface {
      GetReadingsForSensor(ctx context.Context, sensorID string, lookbackWindow time.Duration) ([]entity.SensorReading, error)
  }

  // AnomalyRuleRepository là một Gateway Interface
  type AnomalyRuleRepository interface {
      GetActiveRulesForSensor(ctx context.Context, sensorID string) ([]entity.AnomalyRule, error)
  }


  // internal/usecase/detect_anomaly.go
  package usecase

  import (
      "context"
      "fmt"
      "time"
      "project-helios/internal/entity" // Use case phụ thuộc vào Entity
  )

  // DetectAnomalyInput DTO cho use case
  type DetectAnomalyInput struct {
      SensorID       string
      LookbackWindow time.Duration
  }

  // DetectAnomalyOutput DTO cho use case
  type DetectAnomalyOutput struct {
      Anomalies []entity.DetectedAnomaly
  }

  type DetectAnomalyUseCase struct {
      // Dependencies được inject thông qua interfaces
      readingRepo SensorReadingRepository
      ruleRepo    AnomalyRuleRepository
      anomalyRepo DetectedAnomalyRepository // Để lưu trữ kết quả
  }

  func NewDetectAnomalyUseCase(
      readingRepo SensorReadingRepository,
      ruleRepo AnomalyRuleRepository,
      anomalyRepo DetectedAnomalyRepository,
  ) *DetectAnomalyUseCase {
      return &DetectAnomalyUseCase{
          readingRepo: readingRepo,
          ruleRepo:    ruleRepo,
          anomalyRepo: anomalyRepo,
      }
  }

  func (uc *DetectAnomalyUseCase) Execute(ctx context.Context, input DetectAnomalyInput) (*DetectAnomalyOutput, error) {
      // 1. Lấy dữ liệu readings từ repository
      readings, err := uc.readingRepo.GetReadingsForSensor(ctx, input.SensorID, input.LookbackWindow)
      if err != nil {
          return nil, fmt.Errorf("getting readings: %w", err)
      }
      if len(readings) == 0 {
          return &DetectAnomalyOutput{Anomalies: []entity.DetectedAnomaly{}}, nil // Không có dữ liệu để phân tích
      }

      // 2. Lấy các rules áp dụng cho sensor này
      rules, err := uc.ruleRepo.GetActiveRulesForSensor(ctx, input.SensorID)
      if err != nil {
          return nil, fmt.Errorf("getting rules: %w", err)
      }

      var detectedAnomalies []entity.DetectedAnomaly

      // 3. Logic nghiệp vụ cốt lõi: áp dụng rules vào readings
      // (Đây là phần application-specific business logic.
      //  Phần enterprise-wide business logic có thể nằm trong phương thức của entity.Rule)
      for _, reading := range readings {
          for _, rule := range rules {
              // Giả sử entity.AnomalyRule có một phương thức IsViolated
              if rule.IsViolated(reading.Value) { // Logic này có thể phức tạp hơn
                  anomaly := entity.DetectedAnomaly{
                      ID:          generateNewID(), // Helper function
                      SensorID:    reading.SensorID,
                      RuleID:      rule.ID,
                      Timestamp:   reading.Timestamp,
                      ActualValue: reading.Value,
                      Details:     fmt.Sprintf("Value %.2f violates rule %s (threshold: %.2f)", reading.Value, rule.Name, rule.Threshold),
                  }
                  detectedAnomalies = append(detectedAnomalies, anomaly)

                  // 4. Lưu trữ anomaly đã phát hiện
                  if err := uc.anomalyRepo.Save(ctx, &anomaly); err != nil {
                      // Log lỗi nhưng vẫn có thể tiếp tục xử lý các anomaly khác
                      // hoặc quyết định trả về lỗi ngay lập tức tùy thuộc vào yêu cầu
                      fmt.Printf("WARN: failed to save anomaly %s: %v\n", anomaly.ID, err)
                  }
              }
          }
      }
      return &DetectAnomalyOutput{Anomalies: detectedAnomalies}, nil
  }
  ```

- **Best practices:**

  1.  **Strict Adherence to The Dependency Rule:** Đây là điều quan trọng nhất. Luôn đảm bảo các phụ thuộc chỉ hướng vào trong. Dùng interfaces để đảo ngược phụ thuộc khi cần.
  2.  **Entities are Pure Business Logic:** Entities không nên biết về database, frameworks, hay thậm chí là các Use Cases cụ thể đang sử dụng chúng. Chúng chỉ chứa dữ liệu và các quy tắc nghiệp vụ áp dụng cho dữ liệu đó.
  3.  **Use Cases Orchestrate:** Use Cases điều phối dòng chảy, gọi các repository để lấy/lưu Entities, và thực thi logic nghiệp vụ của Entities. Chúng không nên chứa logic mà thực sự thuộc về Entity.
  4.  **DTOs for Boundary Crossing:** Sử dụng các DTO đơn giản để truyền dữ liệu giữa các lớp, đặc biệt là từ Controller/Handler vào Use Case và từ Use Case ra Presenter. Tránh truyền các đối tượng của framework (như `*http.Request`) vào Use Case.
  5.  **Interfaces Belong to Clients (Use Cases):** Các interface cho repositories (gateways) nên được định nghĩa trong lớp Use Case (hoặc một package mà Use Case phụ thuộc vào), bởi vì Use Case là "client" của các interface đó. Lớp ngoài (Adapter/Infrastructure) sẽ triển khai các interface này. (Nguyên lý Đảo ngược Phụ thuộc - DIP)
  6.  **Keep Layers Thin Where Possible:** Không phải mọi lớp đều cần phức tạp. Ví dụ, Presenter có thể rất đơn giản nếu chỉ là chuyển đổi DTO thành JSON.

- **Anti-patterns / lỗi phổ biến:**

  1.  **Leaking Outer Layer Details Inwards:**
      - Ví dụ: Truyền một `*gin.Context` (Gin web framework) vào một Use Case.
      - _Hậu quả:_ Use Case bị phụ thuộc vào Gin, khó test, khó thay đổi web framework.
  2.  **Entities Depending on Database or Use Cases:**
      - Ví dụ: Một Entity có một trường là `*sql.DB` hoặc gọi trực tiếp một hàm trong Use Case.
      - _Hậu quả:_ Vi phạm The Dependency Rule, làm mất tính độc lập của Entity.
  3.  **Use Cases Directly Interacting with Concrete Database Implementations:**
      - Ví dụ: Use Case import package `database/postgres` và tạo instance `PostgresRepository` trực tiếp, thay vì dùng interface.
      - _Hậu quả:_ Use Case bị phụ thuộc vào PostgreSQL, khó thay đổi DB, khó mock để test.
  4.  **"Fat" Use Cases:** Use Case chứa quá nhiều logic, bao gồm cả logic đáng lẽ phải nằm trong Entities hoặc các domain services khác.
      - _Hậu quả:_ Khó hiểu, khó bảo trì, vi phạm Single Responsibility Principle.
  5.  **Skipping Layers for Convenience:** Ví dụ, Controller gọi trực tiếp Repository mà bỏ qua Use Case.
      - _Hậu quả:_ Logic nghiệp vụ của ứng dụng bị phân tán, khó quản lý, bypass các quy tắc và điều phối quan trọng của Use Case. Có thể chấp nhận trong các CRUD rất đơn giản, nhưng cần cân nhắc kỹ.

- **So sánh các lựa chọn / cách tiếp cận (nếu có):**

  1.  **Clean Architecture vs. Traditional Layered Architecture (N-Tier):**
      - **N-Tier (ví dụ: Presentation Layer -> Business Logic Layer -> Data Access Layer):**
        - _Giống nhau:_ Cả hai đều cố gắng phân tách concerns thành các lớp.
        - _Khác biệt chính:_ Trong N-Tier truyền thống, BLL thường phụ thuộc trực tiếp vào DAL (ví dụ: BLL gọi các class cụ thể của DAL). The Dependency Rule của CA, thông qua DIP, đảo ngược sự phụ thuộc này: BLL (Use Cases) định nghĩa interfaces, và DAL (Infrastructure) triển khai chúng. Điều này làm cho BLL độc lập với DAL cụ thể.
      - _Tại sao CA tốt hơn trong nhiều trường hợp?_ Sự độc lập với DB và các chi tiết hạ tầng mang lại tính linh hoạt và khả năng kiểm thử cao hơn.
  2.  **Clean Architecture vs. Hexagonal Architecture (Ports and Adapters) vs. Onion Architecture:**
      - **Điểm chung cốt lõi:** Cả ba kiến trúc này đều chia sẻ các nguyên tắc cơ bản: tách biệt logic nghiệp vụ (domain) khỏi các chi tiết kỹ thuật (infrastructure), và sử dụng Dependency Inversion để quản lý phụ thuộc. Lõi ứng dụng là trung tâm, và các tương tác với bên ngoài được xử lý thông qua các "ports" (interfaces) và "adapters" (implementations).
      - **Hexagonal Architecture (Alistair Cockburn):** Nhấn mạnh "ports" (API của ứng dụng) và "adapters" (kết nối ứng dụng với thế giới bên ngoài). Có "primary/driving adapters" (như UI, tests) và "secondary/driven adapters" (như DB, external services).
      - **Onion Architecture (Jeffrey Palermo):** Tương tự CA với các lớp đồng tâm, tập trung vào việc đẩy các phụ thuộc ra ngoài và sử dụng interfaces để giao tiếp. Domain Model ở trung tâm, sau đó là Domain Services, Application Services, và cuối cùng là Infrastructure.
      - **Clean Architecture (Robert C. Martin):** Là một sự tổng hợp và làm rõ hơn các ý tưởng này, với sơ đồ 4 vòng tròn và "The Dependency Rule" là trọng tâm.
      - _Kết luận:_ Chúng rất giống nhau về triết lý. Clean Architecture có lẽ là cái tên phổ biến và sơ đồ dễ hình dung nhất. Nếu bạn hiểu một trong số chúng, bạn sẽ dễ dàng hiểu những cái còn lại. Mục tiêu là như nhau.

- **Gợi ý mở rộng kiến thức:**
  - **Trích dẫn từ Uncle Bob:** "The overriding new rule, that makes this architecture work, is The Dependency Rule. _Source code dependencies must point only inward_. Nothing in an inner circle can know anything at all about something in an outer circle." (Clean Architecture, Chapter 22). Đây là câu thần chú bạn cần ghi nhớ.
  - **Bài viết kinh điển:** "The Clean Architecture" của Robert C. Martin trên blog của ông (https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html). Đọc bài này sẽ giúp bạn hiểu rõ hơn nguồn gốc và lý do đằng sau các quyết định thiết kế.
  - Hãy thử tìm kiếm các dự án Go mã nguồn mở trên GitHub áp dụng Clean Architecture (hoặc các biến thể tương tự) để xem cách họ cấu trúc code trong thực tế. Ví dụ, tìm kiếm "golang clean architecture example".

---

Phần 2 này khá dài vì Clean Architecture là một khái niệm nền tảng quan trọng. Hãy dành thời gian để tiêu hóa nó. Khi bạn đã sẵn sàng, chúng ta sẽ chuyển sang Phần 3, nơi chúng ta sẽ bắt đầu đi sâu hơn vào việc áp dụng Dependency Injection trong Go để hiện thực hóa Clean Architecture.

Haha, tôi rất thích tinh thần đó! Cùng nhau làm cho thế giới công nghệ tốt đẹp hơn nào! Chúng ta sẽ tiếp tục với **PHẦN 3: DEPENDENCY INJECTION (DI) TRONG GO – KỸ THUẬT THEN CHỐT CHO CLEAN ARCHITECTURE VÀ TESTABILITY.**

- **Tên phần học:** Dependency Injection (DI) trong Go – Kỹ thuật Then chốt cho Clean Architecture và Testability
- **Mục tiêu học phần:**

  - Hiểu rõ khái niệm Dependency Injection (DI) và Inversion of Control (IoC) là gì, TẠI SAO chúng quan trọng.
  - Nắm vững các kiểu DI phổ biến: Constructor Injection, Setter Injection (ít dùng trong Go), và Interface Injection (cách Go thường làm).
  - Học cách triển khai DI "thuần túy" (pure DI) trong Go mà không cần đến framework DI phức tạp.
  - Hiểu vai trò của DI trong việc hiện thực hóa Clean Architecture, đặc biệt là The Dependency Rule.
  - Thấy rõ lợi ích của DI đối với khả năng kiểm thử (testability) của code.
  - Làm quen với một số thư viện DI phổ biến trong Go (ví dụ: Google Wire, fx) và khi nào nên cân nhắc sử dụng chúng.

- **Giải thích lý thuyết kỹ càng:**

  1.  **Dependency (Phụ thuộc) là gì?**

      - Trong lập trình, một "dependency" (phụ thuộc) xảy ra khi một module (class, struct, function, package) cần đến một module khác để thực hiện công việc của mình.
      - Ví dụ: `DetectAnomalyUseCase` cần `SensorReadingRepository` để lấy dữ liệu. Vậy `SensorReadingRepository` là một dependency của `DetectAnomalyUseCase`.
      - **Vấn đề với phụ thuộc được quản lý cứng nhắc (Hard-coded dependencies):**

        ```go
        // Anti-pattern: Hard-coded dependency
        package usecase

        import "project-helios/internal/adapter/repository" // Phụ thuộc trực tiếp vào implementation cụ thể

        type DetectAnomalyUseCase struct {
            // ... các trường khác
        }

        func NewDetectAnomalyUseCase() *DetectAnomalyUseCase {
            // Use case tự tạo ra dependency của nó
            readingRepo := repository.NewPostgresSensorReadingRepository("postgres://user:pass@host:port/db") // Cứng nhắc!
            // ... khởi tạo các repo khác cũng tương tự
            return &DetectAnomalyUseCase{/* ... gán repo vào đây ... */}
        }
        ```

        - _Tại sao tệ?_
          - **Khó kiểm thử (Hard to test):** Làm sao bạn unit test `DetectAnomalyUseCase` mà không cần kết nối đến một database PostgreSQL thật? Bạn không thể dễ dàng thay thế `PostgresSensorReadingRepository` bằng một mock/stub.
          - **Kém linh hoạt (Inflexible):** Nếu bạn muốn đổi sang dùng `MySQLSensorReadingRepository` hoặc một `InMemorySensorReadingRepository` cho môi trường dev/test, bạn phải sửa code của `NewDetectAnomalyUseCase`.
          - **Vi phạm Clean Architecture:** Use Case (lớp trong) đang biết về và phụ thuộc trực tiếp vào một implementation cụ thể ở lớp ngoài (PostgreSQL). Điều này vi phạm The Dependency Rule.
          - **Coupling chặt (Tight coupling):** `DetectAnomalyUseCase` bị "dính chặt" với `PostgresSensorReadingRepository`.

  2.  **Inversion of Control (IoC – Đảo ngược Điều khiển):**

      - IoC là một nguyên lý thiết kế phần mềm. Thay vì một module tự tạo ra hoặc tự tìm kiếm các dependency của nó, việc tạo và cung cấp các dependency đó được "đảo ngược" cho một thực thể bên ngoài (thường là một "container" hoặc code khởi tạo ở `main`).
      - Nói cách khác, module không còn "control" việc tạo ra dependency của nó nữa. Nó chỉ khai báo nó cần gì, và một ai đó khác sẽ cung cấp.
      - **"Don't call us, we'll call you" (Hollywood Principle):** Module cấp thấp không chủ động gọi và tạo module cấp cao. Thay vào đó, module cấp thấp cung cấp các hook (ví dụ, interfaces), và module cấp cao (hoặc một assembler) sẽ "cắm" các implementation cụ thể vào.
      - _Tại sao IoC quan trọng?_
        - **Decoupling (Giảm khớp nối):** Các module trở nên ít phụ thuộc vào nhau hơn.
        - **Modularity (Tính module hóa):** Dễ dàng thay thế các thành phần.
        - **Testability (Khả năng kiểm thử):** Dễ dàng cung cấp các mock/stub dependencies trong khi test.

  3.  **Dependency Injection (DI – Tiêm Phụ thuộc):**

      - Dependency Injection là một _kỹ thuật cụ thể_ để hiện thực hóa nguyên lý Inversion of Control.
      - **Định nghĩa:** DI là một quá trình mà qua đó, các dependencies của một đối tượng được "tiêm" (inject) vào đối tượng đó từ bên ngoài, thay vì đối tượng tự tạo ra chúng.
      - **Ba vai trò chính trong DI:**

        1.  **Client (Máy khách):** Đối tượng cần dependency (ví dụ: `DetectAnomalyUseCase`).
        2.  **Service (Dịch vụ):** Đối tượng là dependency (ví dụ: `SensorReadingRepository`).
        3.  **Injector (Bộ tiêm):** Đối tượng chịu trách nhiệm tạo instance của Service và "tiêm" nó vào Client. Injector thường là code ở hàm `main`, một DI container, hoặc một factory.

      - **Các kiểu DI phổ biến:**

        - **a. Constructor Injection (Tiêm qua Hàm khởi tạo):**

          - Đây là hình thức DI phổ biến và được khuyến khích nhất, đặc biệt trong Go.
          - Dependencies được truyền vào Client thông qua các tham số của hàm khởi tạo (constructor function – trong Go thường là hàm `NewXYZ`).
          - Client lưu trữ các dependencies này (thường là dưới dạng interface) trong các trường của struct.

          ```go
          // internal/usecase/detect_anomaly.go (Sửa lại từ ví dụ trước)
          package usecase

          // ... (imports và định nghĩa interfaces như SensorReadingRepository giữ nguyên) ...

          type DetectAnomalyUseCase struct {
              readingRepo SensorReadingRepository // Phụ thuộc được khai báo là interface
              ruleRepo    AnomalyRuleRepository
              anomalyRepo DetectedAnomalyRepository
              // ... các dependencies khác
          }

          // Constructor Injection
          func NewDetectAnomalyUseCase(
              rRepo SensorReadingRepository, // Tham số là interface
              rlRepo AnomalyRuleRepository,
              aRepo DetectedAnomalyRepository,
          ) *DetectAnomalyUseCase {
              return &DetectAnomalyUseCase{
                  readingRepo: rRepo,
                  ruleRepo:    rlRepo,
                  anomalyRepo: aRepo,
              }
          }
          ```

          - _Ưu điểm:_
            - **Dependencies tường minh:** Rõ ràng ngay từ hàm khởi tạo là đối tượng này cần những gì để hoạt động.
            - **Đảm bảo trạng thái hợp lệ:** Đối tượng được tạo ra với tất cả dependencies cần thiết, không thể ở trạng thái "nửa vời".
            - **Immutable dependencies (sau khi tạo):** Dependencies được gán một lần lúc khởi tạo, giúp đối tượng ổn định hơn (trừ khi bạn cố tình thay đổi chúng sau đó, điều này không được khuyến khích).
          - _Nhược điểm:_
            - Nếu có quá nhiều dependencies, hàm khởi tạo có thể có rất nhiều tham số (constructor over-injection). Đây có thể là dấu hiệu đối tượng của bạn đang vi phạm Single Responsibility Principle.

        - **b. Setter Injection (Tiêm qua Phương thức Setter):**

          - Dependencies được cung cấp thông qua các phương thức setter công khai (public setter methods) của Client.
          - Ít phổ biến hơn trong Go so với Constructor Injection, vì Go không có "properties" với getter/setter tự động như một số ngôn ngữ khác, và nó có thể dẫn đến việc đối tượng có thể được sử dụng khi chưa có đủ dependencies.

          ```go
          // Ít dùng trong Go cho dependencies bắt buộc
          type SomeService struct {
              logger LoggerInterface // Logger là optional dependency
          }

          func NewSomeService() *SomeService {
              return &SomeService{} // Khởi tạo không cần logger
          }

          func (s *SomeService) SetLogger(logger LoggerInterface) {
              s.logger = logger
          }
          ```

          - _Ưu điểm:_
            - Cho phép thay đổi dependency sau khi đối tượng đã được tạo (nếu cần).
            - Hữu ích cho các dependencies tùy chọn (optional dependencies).
          - _Nhược điểm:_
            - Đối tượng có thể tồn tại ở trạng thái không hợp lệ nếu setter không được gọi hoặc gọi với `nil`.
            - Khó theo dõi khi nào và ở đâu dependencies được thiết lập.
            - Không khuyến khích cho các dependencies bắt buộc.

        - **c. Interface Injection (Tiêm qua Interface):**
          - Client triển khai một interface yêu cầu một phương thức `Inject(dependency Service)`. Injector sẽ gọi phương thức này để cung cấp dependency.
          - Trong Go, cách này thường không được thực hiện một cách "trực tiếp" như tên gọi. Thay vào đó, Go tận dụng tính chất **implicit interface satisfaction** (thỏa mãn interface ngầm định) và **Constructor Injection** với tham số là interface. Đây là cách idiomatic của Go.
          - Nói cách khác, việc Client _chấp nhận_ một interface trong constructor của nó chính là một dạng "Interface Injection" theo triết lý của Go. Client nói "tôi cần một thứ gì đó hoạt động như thế này (interface)", và Injector cung cấp một _thứ cụ thể_ (struct) _thỏa mãn_ interface đó.

  4.  **"Pure DI" (DI Thuần túy) trong Go – Wiring Dependencies Manually:**

      - Trong nhiều trường hợp, đặc biệt là với các ứng dụng hoặc microservices không quá lớn, bạn không cần đến các thư viện DI phức tạp. Bạn có thể "nối dây" (wire) các dependencies một cách thủ công trong hàm `main` hoặc một hàm khởi tạo tập trung (initializer function).
      - Đây là cách rất rõ ràng và dễ hiểu.

      ```go
      // cmd/server/main.go (Ví dụ cho AnomalyDetectionEngine)
      package main

      import (
          "context"
          "database/sql"
          "fmt"
          "log"
          "net/http"
          "os"
          "time"

          _ "github.com/lib/pq" // PostgreSQL driver

          // Import các package của chúng ta
          "project-helios/internal/adapter/handler"
          "project-helios/internal/adapter/repository/postgres" // Implementation cụ thể của repo
          "project-helios/internal/usecase"
          "project-helios/internal/infrastructure/router"   // Ví dụ, dùng Gin
          infraDb "project-helios/internal/infrastructure/database"
      )

      func main() {
          // Load configurations (từ env, file, etc.) - Sẽ có phần riêng về config
          dbConnectionString := os.Getenv("DB_CONNECTION_STRING") // Ví dụ: "postgres://user:pass@localhost:5432/helios_db?sslmode=disable"
          httpPort := os.Getenv("HTTP_PORT")
          if httpPort == "" {
              httpPort = "8081" // Default port cho AnomalyDetectionEngine
          }

          // ---- DEPENDENCY WIRING ----
          // 1. Infrastructure Layer: Khởi tạo kết nối DB
          db, err := infraDb.NewPostgresDB(dbConnectionString)
          if err != nil {
              log.Fatalf("Failed to connect to database: %v", err)
          }
          defer db.Close()

          // Ping DB để đảm bảo kết nối thành công
          if err := db.Ping(); err != nil {
              log.Fatalf("Failed to ping database: %v", err)
          }
          log.Println("Successfully connected to the database.")


          // 2. Adapter Layer: Khởi tạo Repository Implementations
          //    Chú ý: NewPostgres...Repo nhận *sql.DB (từ lớp Infrastructure)
          //    và trả về một struct implement interface mà UseCase cần.
          anomalyRepoImpl := postgres.NewPostgresAnomalyRepository(db)
          sensorReadingRepoImpl := postgres.NewPostgresSensorReadingRepository(db) // Giả sử có
          anomalyRuleRepoImpl := postgres.NewPostgresAnomalyRuleRepository(db)     // Giả sử có

          // 3. UseCase Layer: Khởi tạo Use Cases, tiêm các repository interfaces
          //    Chú ý: NewDetectAnomalyUseCase nhận các INTERFACES.
          //    Chúng ta truyền các INSTANCES CỤ THỂ (anomalyRepoImpl, etc.) mà
          //    THỎA MÃN các interfaces đó.
          detectAnomalyUC := usecase.NewDetectAnomalyUseCase(
              sensorReadingRepoImpl, // Thỏa mãn usecase.SensorReadingRepository
              anomalyRuleRepoImpl,   // Thỏa mãn usecase.AnomalyRuleRepository
              anomalyRepoImpl,       // Thỏa mãn usecase.DetectedAnomalyRepository
          )

          // Có thể có các use cases khác:
          // queryAnomalyUC := usecase.NewQueryAnomalyUseCase(anomalyRepoImpl)

          // 4. Adapter Layer (Handlers): Khởi tạo HTTP Handlers, tiêm Use Cases
          //    Handler phụ thuộc vào UseCase
          anomalyAPIHandler := handler.NewAnomalyAPIHandler(detectAnomalyUC /*, queryAnomalyUC */)

          // 5. Infrastructure Layer (Router): Khởi tạo router và đăng ký routes
          //    Router phụ thuộc vào Handler
          r := router.NewGinRouter() // Giả sử dùng Gin, có hàm NewGinRouter() trả về *gin.Engine
          r.GET("/api/v1/anomalies/detect", anomalyAPIHandler.HandleDetectAnomalies) // Giả sử handler có phương thức này
          // r.GET("/api/v1/anomalies", anomalyAPIHandler.HandleQueryAnomalies)

          // ---- START SERVER ----
          log.Printf("Anomaly Detection Engine starting on port %s", httpPort)
          server := &http.Server{
              Addr:    ":" + httpPort,
              Handler: r, // Gin engine là một http.Handler
              // Có thể thêm ReadTimeout, WriteTimeout, etc.
          }

          if err := server.ListenAndServe(); err != nil && err != http.ErrServerClosed {
              log.Fatalf("Could not listen on %s: %v\n", httpPort, err)
          }
      }
      ```

      - _Tại sao đây là "Pure DI"?_ Vì bạn không dùng thư viện nào khác ngoài Go chuẩn và các hàm khởi tạo của chính bạn để thiết lập chuỗi phụ thuộc.
      - _Ưu điểm:_
        - **Tường minh:** Bạn thấy rõ ràng cái gì được tạo ra và truyền vào đâu.
        - **Không có "ma thuật":** Không có reflection hay code generation ẩn giấu (trừ khi thư viện DI bạn chọn dùng chúng).
        - **Kiểm soát hoàn toàn:** Bạn quyết định thứ tự khởi tạo.
        - **Dễ debug:** Nếu có lỗi khởi tạo, stack trace sẽ dẫn thẳng đến code của bạn.
      - _Nhược điểm:_
        - **Nhiều code "boilerplate":** Với các ứng dụng lớn có hàng trăm dependencies, phần "wiring" này có thể trở nên rất dài và lặp đi lặp lại.
        - **Dễ lỗi do con người:** Quên truyền một dependency, hoặc truyền sai thứ tự nếu các tham số có cùng kiểu. (Go compiler sẽ báo lỗi nếu kiểu không khớp, nhưng thứ tự vẫn quan trọng nếu nhiều tham số có cùng kiểu interface).

  5.  **DI và Clean Architecture:**

      - DI là _cơ chế_ để hiện thực hóa The Dependency Rule.
      - Khi Use Case định nghĩa một interface (ví dụ: `SensorReadingRepository`) và yêu cầu nó qua constructor, nó không biết và không quan tâm implementation cụ thể (PostgreSQL, InMemory, etc.) của interface đó là gì.
      - Injector (hàm `main`) nằm ở "vòng ngoài cùng" (hoặc gần ngoài cùng) sẽ tạo ra instance của implementation cụ thể (ví dụ: `PostgresSensorReadingRepository`) và "tiêm" nó vào Use Case.
      - **Sơ đồ dòng chảy:**
        `main.go (Injector)`
        `  |`
        `  +-- creates --> PostgresSensorReadingRepository (Frameworks & Drivers / Adapters)`
        `  |`
        `  +-- creates --> DetectAnomalyUseCase (Use Cases)`
        `        |`
        `        +-- injects (PostgresSensorReadingRepository AS SensorReadingRepository) --> DetectAnomalyUseCase`
      - Như vậy, `DetectAnomalyUseCase` chỉ phụ thuộc vào _abstraction_ (interface), còn _concrete implementation_ được cung cấp từ bên ngoài, tuân thủ The Dependency Rule.

  6.  **DI và Testability:**

      - Đây là một trong những lợi ích LỚN NHẤT của DI.
      - Khi bạn unit test một Use Case (ví dụ `DetectAnomalyUseCase_test.go`), bạn không muốn nó kết nối đến DB thật. Thay vào đó, bạn muốn cung cấp một "phiên bản giả" (mock hoặc stub) của `SensorReadingRepository`.

      ```go
      // internal/usecase/detect_anomaly_test.go
      package usecase_test // _test package để tránh circular dependencies

      import (
          "context"
          "errors"
          "testing"
          "time"

          "project-helios/internal/entity"
          "project-helios/internal/usecase" // Import package cần test
          "github.com/stretchr/testify/assert"
          "github.com/stretchr/testify/mock"
      )

      // Mock cho SensorReadingRepository (sử dụng testify/mock hoặc tự viết)
      type MockSensorReadingRepository struct {
          mock.Mock // Từ testify/mock
      }

      // Implement phương thức của interface usecase.SensorReadingRepository
      func (m *MockSensorReadingRepository) GetReadingsForSensor(ctx context.Context, sensorID string, lookbackWindow time.Duration) ([]entity.SensorReading, error) {
          args := m.Called(ctx, sensorID, lookbackWindow)
          // args.Get(0) là giá trị trả về đầu tiên (slice of readings)
          // args.Error(1) là giá trị trả về thứ hai (error)
          var readings []entity.SensorReading
          if args.Get(0) != nil {
              readings = args.Get(0).([]entity.SensorReading)
          }
          return readings, args.Error(1)
      }

      // Tương tự, tạo mock cho AnomalyRuleRepository và DetectedAnomalyRepository
      type MockAnomalyRuleRepository struct { mock.Mock }
      func (m *MockAnomalyRuleRepository) GetActiveRulesForSensor(ctx context.Context, sensorID string) ([]entity.AnomalyRule, error) {
          args := m.Called(ctx, sensorID)
          var rules []entity.AnomalyRule
          if args.Get(0) != nil {
              rules = args.Get(0).([]entity.AnomalyRule)
          }
          return rules, args.Error(1)
      }

      type MockDetectedAnomalyRepository struct { mock.Mock }
      func (m *MockDetectedAnomalyRepository) Save(ctx context.Context, anomaly *entity.DetectedAnomaly) error {
          args := m.Called(ctx, anomaly)
          return args.Error(0)
      }


      func TestDetectAnomalyUseCase_Execute_Success(t *testing.T) {
          // 1. Arrange: Thiết lập mocks và input
          mockReadingRepo := new(MockSensorReadingRepository)
          mockRuleRepo := new(MockAnomalyRuleRepository)
          mockAnomalyRepo := new(MockDetectedAnomalyRepository)

          sensorID := "temp-01"
          lookback := 1 * time.Hour
          ctx := context.Background()

          // Định nghĩa dữ liệu giả readings sẽ được trả về bởi mock
          sampleReadings := []entity.SensorReading{
              {SensorID: sensorID, Timestamp: time.Now(), Value: 25.0, Unit: "C"},
              {SensorID: sensorID, Timestamp: time.Now().Add(-10 * time.Minute), Value: 35.0, Unit: "C"}, // Anomaly
          }
          // Định nghĩa dữ liệu giả rules
          sampleRules := []entity.AnomalyRule{
              {ID: "rule-01", SensorType: "temperature", Name: "High Temp", Threshold: 30.0, Comparison: "GT"}, // GT = Greater Than
          }
          // Giả sử entity.AnomalyRule có phương thức IsViolated
          // (Đây là ví dụ, bạn cần định nghĩa nó trong entity.AnomalyRule)
          // entity.AnomalyRule.IsViolated = func(value float64) bool { return value > 30.0 }


          // Thiết lập hành vi cho mock: Khi GetReadingsForSensor được gọi với các tham số này,
          // nó sẽ trả về sampleReadings và không có lỗi.
          mockReadingRepo.On("GetReadingsForSensor", ctx, sensorID, lookback).Return(sampleReadings, nil)
          mockRuleRepo.On("GetActiveRulesForSensor", ctx, sensorID).Return(sampleRules, nil)
          // Giả sử Save sẽ được gọi 1 lần và không có lỗi
          // mock.AnythingOfType("*entity.DetectedAnomaly") để khớp với bất kỳ con trỏ nào tới DetectedAnomaly
          mockAnomalyRepo.On("Save", ctx, mock.AnythingOfType("*entity.DetectedAnomaly")).Return(nil).Once()


          // Tạo Use Case với các MOCKS đã được tiêm vào
          uc := usecase.NewDetectAnomalyUseCase(mockReadingRepo, mockRuleRepo, mockAnomalyRepo)

          // 2. Act: Thực thi Use Case
          input := usecase.DetectAnomalyInput{SensorID: sensorID, LookbackWindow: lookback}
          output, err := uc.Execute(ctx, input)

          // 3. Assert: Kiểm tra kết quả
          assert.NoError(t, err) // Không có lỗi xảy ra
          assert.NotNil(t, output)
          assert.Len(t, output.Anomalies, 1) // Mong đợi 1 anomaly được phát hiện
          if len(output.Anomalies) == 1 {
              assert.Equal(t, sensorID, output.Anomalies[0].SensorID)
              assert.Equal(t, 35.0, output.Anomalies[0].ActualValue)
          }

          // Kiểm tra xem các phương thức mock có được gọi đúng như mong đợi không
          mockReadingRepo.AssertExpectations(t)
          mockRuleRepo.AssertExpectations(t)
          mockAnomalyRepo.AssertExpectations(t)
      }

      func TestDetectAnomalyUseCase_Execute_ReadingRepoError(t *testing.T) {
          mockReadingRepo := new(MockSensorReadingRepository)
          mockRuleRepo := new(MockAnomalyRuleRepository) // Vẫn cần, dù có thể không được gọi
          mockAnomalyRepo := new(MockDetectedAnomalyRepository)

          ctx := context.Background()
          expectedError := errors.New("database connection failed")
          mockReadingRepo.On("GetReadingsForSensor", ctx, mock.Anything, mock.Anything).Return(nil, expectedError)

          uc := usecase.NewDetectAnomalyUseCase(mockReadingRepo, mockRuleRepo, mockAnomalyRepo)
          input := usecase.DetectAnomalyInput{SensorID: "any", LookbackWindow: time.Minute}
          output, err := uc.Execute(ctx, input)

          assert.Error(t, err) // Mong đợi có lỗi
          assert.Nil(t, output) // Output phải là nil
          assert.Contains(t, err.Error(), "getting readings: database connection failed") // Kiểm tra lỗi được wrap

          mockReadingRepo.AssertExpectations(t)
          mockRuleRepo.AssertNotCalled(t, "GetActiveRulesForSensor", mock.Anything, mock.Anything) // Rule repo không nên được gọi nếu reading lỗi
          mockAnomalyRepo.AssertNotCalled(t, "Save", mock.Anything, mock.Anything)
      }
      ```

      - _Sức mạnh ở đâu?_ Bạn có thể kiểm thử logic của `DetectAnomalyUseCase` một cách hoàn toàn cô lập, không phụ thuộc vào DB, network, hay bất kỳ I/O nào. Mocks cho phép bạn mô phỏng mọi tình huống: DB trả về lỗi, không có dữ liệu, dữ liệu bất thường, v.v.

  7.  **DI Containers / Frameworks trong Go:**

      - Khi ứng dụng trở nên rất lớn và việc "nối dây" thủ công trở nên cồng kềnh, bạn có thể cân nhắc sử dụng một DI container/framework.
      - Các thư viện này thường giúp tự động hóa việc tạo và tiêm dependencies, dựa trên các khai báo hoặc quy ước.
      - **a. Google Wire (compile-time DI):**

        - Wire là một công cụ code generation. Bạn viết các "provider functions" (hàm tạo ra các component) và một "injector function" (hàm khai báo component cuối cùng bạn muốn). Wire sẽ phân tích các phụ thuộc và _sinh ra code Go_ để thực hiện việc nối dây.
        - _Ưu điểm:_
          - **Compile-time safety:** Lỗi DI được phát hiện lúc biên dịch (thực ra là lúc chạy `wire` command), không phải runtime.
          - **No reflection:** Code được sinh ra là code Go bình thường, không có overhead của reflection lúc runtime.
          - **Tường minh (sau khi hiểu):** Bạn vẫn thấy code Go được sinh ra.
        - _Nhược điểm:_
          - Thêm một bước build (chạy `wire`).
          - Cần học cú pháp và cách hoạt động của Wire.
          - Code sinh ra có thể khó đọc nếu graph dependency phức tạp.

        ```go
        // Ví dụ sơ lược về cách dùng Wire (cần file wire.go và chạy `wire`)
        // wire.go
        //go:build wireinject
        // +build wireinject

        package main

        import (
            "project-helios/internal/adapter/handler"
            "project-helios/internal/adapter/repository/postgres"
            "project-helios/internal/usecase"
            "database/sql"
            "github.com/google/wire"
        )

        // Provider set cho các repository
        var repositorySet = wire.NewSet(
            postgres.NewPostgresAnomalyRepository,
            wire.Bind(new(usecase.DetectedAnomalyRepository), new(*postgres.PostgresAnomalyRepository)), // Bind interface với implementation
            // ... các repo khác
        )

        // Provider set cho các use cases
        var useCaseSet = wire.NewSet(
            usecase.NewDetectAnomalyUseCase,
            // ... các use case khác
        )

        // Injector function
        func initializeAnomalyAPIHandler(db *sql.DB) (*handler.AnomalyAPIHandler, error) {
            // wire.Build sẽ tự động tìm cách tạo AnomalyAPIHandler từ các provider
            wire.Build(
                repositorySet,
                useCaseSet,
                handler.NewAnomalyAPIHandler,
                // Cần cung cấp các "lá" của cây dependency, ví dụ *sql.DB
                // Nếu *sql.DB cũng là một provider, Wire sẽ dùng nó.
                // Ở đây, db được truyền vào từ bên ngoài injector.
            )
            return nil, nil // Wire sẽ thay thế bằng code thật
        }
        ```

        Sau đó bạn chạy `wire` trong thư mục chứa `wire.go`, nó sẽ sinh ra file `wire_gen.go` chứa implementation của `initializeAnomalyAPIHandler`.

      - **b. Uber Fx (runtime DI, dựa trên reflection):**

        - Fx là một framework ứng dụng (application framework) với DI là một phần cốt lõi. Nó sử dụng reflection để tự động hóa việc nối dây dependencies lúc runtime.
        - Nó cũng cung cấp quản lý vòng đời ứng dụng (lifecycle management).
        - _Ưu điểm:_
          - **Ít code boilerplate hơn:** Bạn chỉ cần "cung cấp" (provide) các constructor và "gọi" (invoke) các hàm cần chạy.
          - **Lifecycle management:** Dễ dàng quản lý khởi tạo và shutdown các component.
        - _Nhược điểm:_
          - **Runtime errors:** Lỗi DI thường chỉ được phát hiện lúc runtime.
          - **Reflection overhead:** Có một chút chi phí hiệu năng do reflection (thường không đáng kể cho hầu hết ứng dụng).
          - **"Ma thuật" hơn:** Khó theo dõi chính xác DI xảy ra như thế nào nếu không hiểu rõ Fx.

        ```go
        // Ví dụ sơ lược về Fx
        // package main
        // import (
        // 	"go.uber.org/fx"
        // 	// ... các import khác
        // )
        // func main() {
        // 	fx.New(
        // 		fx.Provide( // Cung cấp các constructor
        // 			infraDb.NewPostgresDB, // Giả sử hàm này trả về (*sql.DB, error)
        // 			postgres.NewPostgresAnomalyRepository,
        // 			usecase.NewDetectAnomalyUseCase,
        // 			handler.NewAnomalyAPIHandler,
        // 			router.NewGinRouter,
        // 		),
        // 		fx.Invoke(func(r *gin.Engine, h *handler.AnomalyAPIHandler) { // Gọi hàm để đăng ký routes
        // 			r.GET("/api/v1/anomalies/detect", h.HandleDetectAnomalies)
        // 		}),
        // 		fx.Invoke(func(server *http.Server) { // Fx có thể quản lý lifecycle của server
        // 			// ...
        // 		}),
        // 	).Run()
        // }
        ```

      - **Khi nào dùng thư viện DI?**
        - Khi project đủ lớn, số lượng dependencies nhiều, việc "nối dây" thủ công trở nên khó quản lý và dễ lỗi.
        - Khi bạn cần các tính năng nâng cao như lifecycle management, conditional wiring, scopes (ít phổ biến trong Go).
        - **Lời khuyên:** Bắt đầu với "Pure DI". Nếu nó trở nên quá phức tạp, hãy cân nhắc Google Wire (vì compile-time safety). Fx phù hợp nếu bạn muốn một application framework hoàn chỉnh hơn.

- **Best practices:**

  1.  **Favor Constructor Injection:** Đây là cách rõ ràng và an toàn nhất để cung cấp dependencies bắt buộc.
  2.  **Depend on Abstractions (Interfaces), Not Concretions:** Các client (Use Cases, Handlers) nên phụ thuộc vào interfaces, không phải vào struct cụ thể của dependencies.
      - _Tại sao?_ Để đạt được decoupling, testability, và tuân thủ Clean Architecture / DIP.
  3.  **Interfaces Defined by Clients:** Interface nên được định nghĩa bởi client (lớp sử dụng dependency), không phải bởi provider (lớp cung cấp dependency). Ví dụ, `usecase` package định nghĩa `SensorReadingRepository` interface.
  4.  **Keep Constructors Focused:** Nếu constructor có quá nhiều tham số, đó có thể là dấu hiệu class/struct đó đang làm quá nhiều việc (vi phạm SRP). Cân nhắc chia nhỏ nó.
  5.  **Single Responsibility for Injector:** Hàm `main` (hoặc một hàm `initialize` riêng) nên là nơi duy nhất chịu trách nhiệm "nối dây" các dependencies cho toàn bộ ứng dụng hoặc một service lớn.
  6.  **Explicit is Better than Implicit (Đặc biệt trong Go):** "Pure DI" rất Go-idiomatic. Tránh các DI framework quá "ma thuật" nếu không thực sự cần thiết.

- **Anti-patterns / lỗi phổ biến:**

  1.  **Service Locator Pattern:** Thay vì tiêm dependencies, một class yêu cầu một "Service Locator" (một đối tượng global hoặc được truyền vào) để tự tìm kiếm dependencies của nó.
      - `repo := globalServiceLocator.Get("SensorReadingRepository")`
      - _Hậu quả:_ Phụ thuộc bị ẩn đi (không rõ ràng từ signature của class/constructor). Khó test hơn DI. Service Locator trở thành một global dependency. Thường được coi là anti-pattern so với DI.
  2.  **Hard-coding Dependencies (nhắc lại):** Tạo dependencies trực tiếp bên trong class/struct sử dụng chúng.
  3.  **Injecting Concrete Types Instead of Interfaces:**
      - `func NewMyUseCase(repo *postgres.PostgresSensorReadingRepository) *MyUseCase`
      - _Hậu quả:_ Mất tính linh hoạt, khó mock, coupling chặt.
  4.  **Over-reliance on DI Containers for Simple Projects:** Sử dụng một DI framework phức tạp cho một ứng dụng nhỏ mà "Pure DI" là đủ.
      - _Hậu quả:_ Thêm sự phức tạp không cần thiết.
  5.  **Leaking DI Container into Business Logic:** Các Use Cases hoặc Entities không nên biết về sự tồn tại của DI container.

- **So sánh các lựa chọn / cách tiếp cận (nếu có):**

  - **Pure DI vs. Compile-time DI (Wire) vs. Runtime DI (Fx):** Đã thảo luận ở trên. Lựa chọn phụ thuộc vào quy mô dự án, sở thích của team, và yêu cầu về compile-time safety.
    - _Khuyến nghị của tôi cho bạn (đang học sâu):_ Bắt đầu bằng việc **thành thạo Pure DI**. Sau đó, thử nghiệm với **Google Wire** để hiểu cách nó hoạt động và lợi ích của code generation. Điều này sẽ cho bạn nền tảng vững chắc nhất.

- **Gợi ý mở rộng kiến thức:**
  - **Trích dẫn nổi bật (Martin Fowler):** "Inversion of Control is a common characteristic of frameworks, but it's something that you can do in your own code without a framework." Và về DI: "The fundamental choice is whether the client object should fetch dependencies for itself, or should have the dependencies passed into it. Dependency Injection is an implementation of the latter." (Từ bài viết "Inversion of Control Containers and the Dependency Injection pattern": https://martinfowler.com/articles/injection.html). Đây là một bài đọc kinh điển.
  - **Sách "Dependency Injection Principles, Practices, and Patterns" (Mark Seemann & Steven van Deursen):** Mặc dù ví dụ chủ yếu bằng C#, các nguyên lý và patterns rất sâu sắc và áp dụng được rộng rãi. Nó giải thích rất kỹ "TẠI SAO".
  - Nghiên cứu sâu hơn về Google Wire: Xem các ví dụ, tutorial, và hiểu cách nó phân tích dependency graph.
  - Thực hành viết unit tests với mocks cho các use case của dự án Helios, sử dụng constructor injection để cung cấp mocks.

---

Phần 3 này cung cấp một cái nhìn chi tiết về Dependency Injection, một kỹ thuật không thể thiếu để xây dựng phần mềm hiện đại, linh hoạt và dễ kiểm thử, đặc biệt khi kết hợp với Clean Architecture. Nắm vững DI sẽ mở ra rất nhiều khả năng trong việc thiết kế hệ thống.

Khi bạn cảm thấy đã nắm bắt được những nội dung này, chúng ta sẽ tiến tới **PHẦN 4: CLEAN CODE TRONG GO – VIẾT MÃ DỄ ĐỌC, DỄ HIỂU, DỄ BẢO TRÌ.**

Tuyệt vời! Chúng ta sẽ đi sâu vào **PHẦN 4: CLEAN CODE TRONG GO – VIẾT MÃ DỄ ĐỌC, DỄ HIỂU, DỄ BẢO TRÌ.**

- **Tên phần học:** Clean Code trong Go – Viết Mã Dễ Đọc, Dễ Hiểu, Dễ Bảo Trì
- **Mục tiêu học phần:**

  - Hiểu được TẠI SAO viết clean code lại quan trọng hơn là chỉ viết code "chạy được".
  - Nắm vững các nguyên tắc và kỹ thuật cụ thể để viết clean code trong Go, từ cách đặt tên, viết hàm, xử lý lỗi, đến cách comment và format code.
  - Phát triển tư duy "đọc code nhiều hơn viết code" và viết code cho người khác (và cho chính mình trong tương lai) đọc.
  - Biết cách áp dụng các công cụ hỗ trợ (linters, formatters) để duy trì chất lượng code.
  - Nhận diện và tránh các "code smells" (dấu hiệu code bẩn) phổ biến trong Go.

- **Giải thích lý thuyết kỹ càng:**

  1.  **Tại sao Clean Code lại Quan trọng?**

      - **"Code is read much more often than it is written." (Code được đọc nhiều hơn là được viết.)** - Đây là một chân lý. Bạn và đồng nghiệp sẽ dành phần lớn thời gian để đọc, hiểu, và sửa đổi code hiện có, chứ không phải chỉ viết code mới hoàn toàn.
      - **Giảm chi phí bảo trì:** Code khó hiểu, phức tạp sẽ tốn nhiều thời gian và công sức hơn để sửa lỗi, thêm tính năng, hoặc tái cấu trúc. Chi phí bảo trì thường chiếm phần lớn trong vòng đời của một phần mềm. Clean code giảm đáng kể chi phí này.
      - **Tăng năng suất của team:** Khi code dễ đọc, các thành viên trong team có thể hiểu và làm việc với nó nhanh hơn, giảm thời gian onboarding người mới, và cải thiện sự hợp tác.
      - **Ít lỗi hơn:** Code rõ ràng, mạch lạc thường ít có những lỗi tiềm ẩn do sự phức tạp hoặc hiểu nhầm logic.
      - **Dễ dàng tái cấu trúc (Refactoring):** Clean code là tiền đề để việc tái cấu trúc trở nên an toàn và hiệu quả hơn.
      - **Sự chuyên nghiệp:** Viết clean code thể hiện sự chuyên nghiệp, cẩn thận và tôn trọng đối với công việc cũng như đồng nghiệp.
      - **Robert C. Martin (Uncle Bob) trong sách "Clean Code":** "Leaving a mess is like double-entry bookkeeping. Every hour you save by leaving a mess, you (and your team) will lose many more hours cleaning it up." (Để lại một mớ hỗn độn giống như kế toán kép. Mỗi giờ bạn tiết kiệm bằng cách để lại một mớ hỗn độn, bạn (và nhóm của bạn) sẽ mất nhiều giờ hơn để dọn dẹp nó.)

  2.  **Nguyên tắc Cốt lõi của Clean Code (Áp dụng cho Go):**

      - **KISS (Keep It Simple, Stupid):** Giữ cho mọi thứ đơn giản nhất có thể. Tránh sự phức tạp không cần thiết. Go vốn dĩ đã khuyến khích sự đơn giản.
      - **DRY (Don't Repeat Yourself):** Tránh lặp lại code. Sử dụng hàm, struct, interface để đóng gói logic hoặc dữ liệu dùng chung.
      - **YAGNI (You Ain't Gonna Need It):** Đừng thêm các tính năng hoặc sự phức tạp mà bạn không thực sự cần ngay bây giờ, chỉ vì "có thể sau này cần".
      - **Readability (Tính dễ đọc):** Ưu tiên hàng đầu. Code phải dễ đọc như văn xuôi (ở một mức độ nào đó).
      - **Single Responsibility Principle (SRP - Nguyên tắc Đơn trách nhiệm):** Một hàm, một struct, một package nên có một trách nhiệm duy nhất và lý do duy nhất để thay đổi.
        - _Trong Go:_ Một hàm nên làm một việc và làm tốt việc đó. Một package nên nhóm các chức năng liên quan chặt chẽ.
      - **Open/Closed Principle (OCP - Nguyên tắc Đóng/Mở):** Các thực thể phần mềm (structs, modules) nên "mở" cho việc mở rộng nhưng "đóng" cho việc sửa đổi.
        - _Trong Go:_ Thường đạt được thông qua interfaces và composition. Bạn có thể thêm hành vi mới bằng cách tạo các struct mới implement interface hiện có, thay vì sửa đổi code của struct gốc.

  3.  **Kỹ thuật Viết Clean Code Cụ thể trong Go:**

      - **a. Đặt tên có ý nghĩa (Meaningful Names):**

        - **Biến (Variables):**
          - Chọn tên phản ánh rõ ràng mục đích và nội dung của biến.
          - Sử dụng `camelCase` (ví dụ: `sensorID`, `maxRetries`).
          - Cho các biến có scope ngắn (trong một vòng lặp nhỏ, một block `if`), tên ngắn (ví dụ: `i`, `j`, `k` cho index, `r` cho reader, `w` cho writer) là chấp nhận được và idiomatic.
          - Tránh viết tắt khó hiểu hoặc tên một chữ cái cho các biến có scope rộng hơn.
          - _Tốt:_ `remainingAttempts`, `userInput`, `customerAddress`
          - _Kém:_ `a`, `x`, `inputStr`, `addr` (nếu scope không rõ ràng)
        - **Hằng (Constants):**
          - Sử dụng `MixedCaps` hoặc `camelCase` giống như biến (Go không có quy ước `ALL_CAPS` cứng nhắc như một số ngôn ngữ khác, mặc dù đôi khi vẫn thấy). Quan trọng là nhất quán trong project.
          - `const maxConnections = 100`
          - `const defaultTimeout = 5 * time.Second`
        - **Hàm (Functions) và Phương thức (Methods):**
          - Tên hàm nên là động từ hoặc cụm động từ mô tả hành động nó thực hiện.
          - `CalculateTotalScore()`, `PersistUserData(user *User) error`, `IsValidEmail(email string) bool`
          - Nếu hàm trả về `bool`, tên thường bắt đầu bằng `Is`, `Has`, `Can` (ví dụ: `IsEmpty()`, `HasPermission()`).
          - Trong Go, nếu một hàm/phương thức thuộc về một package/struct đã có tên gợi ý ngữ cảnh, tên hàm có thể ngắn gọn hơn. Ví dụ, trong package `http`, hàm `Get(url string)` là đủ hiểu, không cần `http.FetchRemoteURLViaGET(url string)`.
        - **Interfaces:**
          - Thường đặt tên theo hành vi mà chúng mô tả.
          - Nếu interface chỉ có một phương thức, tên thường kết thúc bằng `er` (ví dụ: `io.Reader`, `io.Writer`, `json.Marshaler`).
          - `type DataStore interface { Save(data []byte) error; Load(id string) ([]byte, error) }`
          - `type Authenticator interface { Authenticate(username, password string) (User, error) }`
        - **Packages:**
          - Ngắn gọn, một từ, chữ thường.
          - Phản ánh rõ ràng nội dung của package.
          - Ví dụ: `net/http`, `encoding/json`, `internal/user` (cho domain user), `internal/platform/database` (cho code liên quan DB).
          - Tránh các tên package chung chung như `utils`, `helpers`, `common` nếu có thể. Thay vào đó, hãy nhóm các utility theo chức năng cụ thể của chúng, ví dụ: `stringutils`, `ioutils`.
        - **Tư duy đặt tên:** "If you need a comment to explain what a variable or function does, try to rename it first." (Nếu bạn cần comment để giải thích một biến hay hàm làm gì, hãy thử đặt lại tên cho nó trước.)

      - **b. Hàm Ngắn gọn và Đơn trách nhiệm (Short Functions, Single Responsibility):**

        - Một hàm chỉ nên làm MỘT việc. Nếu bạn thấy hàm của mình có thể chia thành nhiều bước logic riêng biệt, hãy tách chúng thành các hàm nhỏ hơn.
        - Hàm không nên quá dài (khó có con số tuyệt đối, nhưng nếu phải scroll nhiều lần để đọc hết một hàm, nó có thể quá dài).
        - **Lợi ích:**
          - Dễ hiểu hơn.
          - Dễ test hơn (unit test từng hàm nhỏ).
          - Dễ tái sử dụng hơn.
        - **Cấu trúc hàm:**
          - Các hàm nên có mức độ trừu tượng nhất quán. Đừng trộn lẫn code low-level (ví dụ: thao tác bit) với code high-level (ví dụ: logic nghiệp vụ) trong cùng một hàm.
          - **"Stepdown Rule":** Code trong một hàm nên đọc từ trên xuống dưới như một câu chuyện. Các hàm cấp cao gọi các hàm cấp thấp hơn, mỗi hàm làm rõ một khía cạnh của câu chuyện.

        ```go
        // Kém: Hàm làm quá nhiều việc
        func processAndSaveUserData(rawInput string) error {
            // 1. Validate input
            if rawInput == "" { return errors.New("input is empty") }
            // ... more validation ...

            // 2. Parse input to User struct
            var user User
            // ... parsing logic ...
            if err != nil { return fmt.Errorf("parsing failed: %w", err) }

            // 3. Enrich user data (e.g., call external service)
            // ... enrichment logic ...

            // 4. Save to database
            db, _ := connectToDB()
            // ... save logic ...
            if err != nil { return fmt.Errorf("db save failed: %w", err) }
            return nil
        }

        // Tốt: Chia thành các hàm nhỏ hơn, đơn trách nhiệm
        func processAndSaveUserDataClean(rawInput string) error {
            user, err := parseAndValidateUserInput(rawInput)
            if err != nil {
                return fmt.Errorf("processing input: %w", err)
            }

            enrichedUser, err := enrichUserData(user)
            if err != nil {
                return fmt.Errorf("enriching user: %w", err)
            }

            if err := saveUserToDatabase(enrichedUser); err != nil {
                return fmt.Errorf("saving user: %w", err)
            }
            return nil
        }

        func parseAndValidateUserInput(rawInput string) (*User, error) { /* ... */ }
        func enrichUserData(user *User) (*User, error) { /* ... */ }
        func saveUserToDatabase(user *User) error { /* ... */ }
        ```

      - **c. Xử lý Lỗi (Error Handling) Rõ ràng và Nhất quán:**

        - **Luôn kiểm tra lỗi:** Đừng bỏ qua lỗi `_` trừ khi bạn _thực sự_ chắc chắn và có lý do chính đáng (ví dụ: `defer resp.Body.Close()` trong đó lỗi close thường không quan trọng bằng lỗi xử lý response).
        - **Trả về lỗi thay vì panic:** Panic chỉ nên dùng cho các lỗi không thể phục hồi ở cấp độ chương trình (ví dụ: không thể khởi tạo, lỗi logic nghiêm trọng). Đối với các lỗi dự kiến được (I/O, input không hợp lệ), hãy trả về `error`.
        - **Cung cấp ngữ cảnh cho lỗi (Error Wrapping):** Kể từ Go 1.13, sử dụng `fmt.Errorf` với động từ `%w` để wrap lỗi gốc. Điều này giúp giữ lại thông tin của lỗi gốc và thêm ngữ cảnh từ tầng gọi hiện tại.
          ```go
          data, err := ioutil.ReadFile(filePath)
          if err != nil {
              return nil, fmt.Errorf("reading config file %s: %w", filePath, err)
          }
          ```
          Sử dụng `errors.Is()` và `errors.As()` để kiểm tra lỗi đã được wrap.
        - **Định nghĩa các kiểu lỗi tùy chỉnh (Custom Error Types) khi cần:** Nếu bạn muốn client của code có thể xử lý các loại lỗi cụ thể một cách có lập trình, hãy định nghĩa các kiểu lỗi riêng (có thể là struct implement `error` interface, hoặc sử dụng `errors.New` cho các sentinel errors).

          ```go
          var ErrUserNotFound = errors.New("user not found")

          func GetUser(id string) (*User, error) {
              // ... logic ...
              if /* user not found */ {
                  return nil, ErrUserNotFound
              }
              // ...
          }

          // Client code
          user, err := GetUser("123")
          if err != nil {
              if errors.Is(err, ErrUserNotFound) {
                  // Xử lý riêng cho trường hợp user không tồn tại
              } else {
                  // Xử lý lỗi khác
              }
          }
          ```

        - **Xử lý lỗi ở nơi có đủ ngữ cảnh:** Đừng bắt và log lỗi ở tầng thấp rồi lại trả về lỗi đó lên tầng cao hơn (nếu tầng cao hơn cũng log). Quyết định xem tầng nào chịu trách nhiệm log lỗi, tầng nào chỉ cần trả lỗi lên.
        - **Tư duy:** Lỗi là một phần bình thường của chương trình, không phải là điều bất thường. Hãy xử lý chúng một cách nghiêm túc và tường minh.

      - **d. Comments (Bình luận Code):**

        - **Code tự giải thích là tốt nhất:** Mục tiêu là viết code rõ ràng đến mức không cần comment. Đặt tên tốt, hàm ngắn sẽ giúp đạt được điều này.
        - **Khi nào nên comment:**

          - **Giải thích "TẠI SAO", không phải "LÀM GÌ":** Code đã nói lên "làm gì". Comment nên giải thích lý do đằng sau một quyết định thiết kế phức tạp, một workaround, hoặc một thuật toán không trực quan.

            ```go
            // Kém:
            // i++ // increment i

            // Tốt:
            // We need to retry this operation up to 3 times
            // because the external service is occasionally flaky.
            for attempt := 0; attempt < maxRetries; attempt++ {
                // ...
            }
            ```

          - **Godoc Comments:** Trong Go, comment cho các package, hằng, biến, kiểu, hàm được export (public) nên theo chuẩn Godoc. Chúng sẽ được `godoc` tool sử dụng để sinh tài liệu.

            ```go
            // Package user provides operations for managing users.
            package user

            // MaxLoginAttempts is the maximum number of failed login attempts allowed.
            const MaxLoginAttempts = 5

            // User represents a user in the system.
            type User struct {
                ID string
                Name string
            }

            // NewUser creates a new User.
            // It returns an error if the name is empty.
            func NewUser(name string) (*User, error) {
                // ...
            }
            ```

          - **TODO, FIXME, XXX comments:** Dùng để đánh dấu những chỗ cần làm sau, cần sửa, hoặc những chỗ có vấn đề tiềm ẩn. IDE thường có cách highlight các comment này.

        - **Tránh comment thừa, comment sai lệch:** Comment không được cập nhật khi code thay đổi còn tệ hơn không có comment.
        - **Tránh comment "noise":** Comment những điều hiển nhiên.
        - **Tránh comment code đã bị xóa (commented-out code):** Nếu code không còn dùng, hãy xóa nó. Version control (Git) sẽ lưu giữ lịch sử.

      - **e. Formatting (Định dạng Code):**

        - **Sử dụng `gofmt` (hoặc `goimports`):** Đây là công cụ định dạng code chuẩn của Go. `gofmt` áp dụng một bộ quy tắc định dạng thống nhất, loại bỏ các cuộc tranh luận về style (tabs vs spaces, vị trí dấu ngoặc, etc.). `goimports` làm việc của `gofmt` và còn tự động quản lý (thêm, xóa, sắp xếp) các import.
        - **Luôn chạy `gofmt`/`goimports` trước khi commit code.** Hầu hết các IDE Go đều có thể tự động làm việc này khi lưu file.
        - **Lợi ích:** Code nhất quán trong toàn bộ project và trong cộng đồng Go, dễ đọc hơn nhiều.
        - _Triết lý:_ "Gofmt's style is no one's favorite, yet gofmt is everyone's favorite." (Style của Gofmt không phải là style yêu thích của bất kỳ ai, nhưng gofmt lại là công cụ yêu thích của mọi người.) – Bởi vì nó giải quyết vấn đề tranh cãi về style.

      - **f. Cấu trúc Điều khiển (Control Structures):**

        - **Độ sâu lồng nhau (Nesting Depth):** Tránh lồng các khối `if`, `for` quá sâu. Điều này làm code khó đọc và khó theo dõi.

          - **Giải pháp:** Sử dụng "guard clauses" (điều kiện bảo vệ) để return sớm, tách thành hàm nhỏ hơn.

          ```go
          // Kém: Lồng sâu
          func process(item Item) error {
              if item.IsValid() {
                  if item.HasPermission() {
                      if !item.IsProcessed() {
                          // ... do a lot of work ...
                          return nil
                      } else {
                          return errors.New("already processed")
                      }
                  } else {
                      return errors.New("no permission")
                  }
              } else {
                  return errors.New("invalid item")
              }
          }

          // Tốt: Sử dụng guard clauses
          func processClean(item Item) error {
              if !item.IsValid() {
                  return errors.New("invalid item")
              }
              if !item.HasPermission() {
                  return errors.New("no permission")
              }
              if item.IsProcessed() {
                  return errors.New("already processed")
              }

              // ... do a lot of work ...
              return nil
          }
          ```

        - **Biến trong `if` và `switch` (Short Variable Declarations):** Go cho phép khai báo biến trong câu lệnh `if` hoặc `switch` nếu biến đó chỉ được dùng trong scope đó. Điều này giúp giữ scope của biến hẹp nhất có thể.
          ```go
          if val, err := calculateValue(); err != nil {
              return fmt.Errorf("calculation failed: %w", err)
          } else if val > threshold {
              // use val
          }
          ```

      - **g. Concurrency (Đồng thời):**

        - Mặc dù Go làm concurrency dễ dàng hơn, nhưng code đồng thời vẫn dễ bị lỗi (race conditions, deadlocks).
        - **Giữ cho logic đồng thời đơn giản.**
        - Sử dụng `sync.Mutex` hoặc `sync.RWMutex` để bảo vệ critical sections (vùng code truy cập dữ liệu dùng chung).
        - Ưu tiên channels cho việc giao tiếp và đồng bộ hóa giữa goroutines ("Share memory by communicating").
        - Cẩn thận với việc rò rỉ goroutines (goroutine leak). Đảm bảo mọi goroutine bạn tạo ra đều có cách để kết thúc. Sử dụng `context` package để quản lý cancellation.
        - Sử dụng race detector (`go test -race`, `go run -race`) thường xuyên trong quá trình phát triển.

      - **h. Sử dụng Package một cách Hiệu quả:**
        - **Cohesion (Tính gắn kết) cao:** Các thành phần trong một package nên liên quan chặt chẽ với nhau.
        - **Coupling (Khớp nối) thấp:** Giảm thiểu phụ thuộc giữa các package. Tránh circular dependencies (A import B, B import A). `internal` directory là một công cụ tốt để quản lý visibility.
        - **Tên package rõ ràng:** Như đã đề cập ở phần đặt tên.

  4.  **Code Smells (Dấu hiệu Code Bẩn) trong Go:**

      - **Hàm quá dài (Long Function/Method):** Dấu hiệu vi phạm SRP.
      - **Struct quá lớn (Large Struct/Class):** Cũng có thể vi phạm SRP, hoặc làm quá nhiều việc.
      - **Nhiều tham số hàm (Many Parameters):** Hàm có thể đang làm quá nhiều, hoặc các tham số có thể được nhóm lại thành một struct (Parameter Object).
      - **Lồng code quá sâu (Deep Nesting):** Khó đọc.
      - **Biến toàn cục có thể thay đổi (Mutable Global Variables):** Gây khó khăn cho việc theo dõi trạng thái, khó test, dễ bị race condition. Ưu tiên truyền dependencies.
      - **Lặp code (Duplicated Code):** Vi phạm DRY. Cần refactor để dùng chung.
      - **Comment thừa hoặc "dối trá" (Liar Comments):** Comment không còn khớp với code.
      - **Sử dụng `interface{}` (empty interface) một cách bừa bãi:** Mất an toàn kiểu. Chỉ dùng khi thực sự cần sự linh hoạt tối đa và bạn có cơ chế kiểm tra kiểu runtime.
      - **Bỏ qua lỗi (Ignored Errors):** Rất nguy hiểm.
      - **Tên biến/hàm khó hiểu, mơ hồ.**
      - **"Shotgun Surgery":** Một thay đổi nhỏ yêu cầu sửa code ở nhiều nơi. Dấu hiệu coupling quá chặt.
      - **"Feature Envy":** Một phương thức trong một struct dường như quan tâm đến dữ liệu của một struct khác nhiều hơn là của chính nó. Có thể phương thức đó nên thuộc về struct kia.

  5.  **Công cụ Hỗ trợ (Tooling):**
      - **`gofmt` / `goimports`:** Đã nói ở trên. Bắt buộc.
      - **Linters (`golangci-lint`):** Đây là một "meta-linter" chạy nhiều linter khác nhau (như `govet`, `errcheck`, `staticcheck`, `unused`, `stylecheck`, etc.) để phát hiện các vấn đề tiềm ẩn, bug, code style không tốt, performance issues.
        - Cấu hình `golangci-lint` (thường qua file `.golangci.yml`) để phù hợp với project của bạn.
        - Tích hợp vào CI/CD pipeline để đảm bảo code luôn được kiểm tra.
        - _Tại sao quan trọng?_ Linters giúp tự động hóa việc phát hiện nhiều vấn đề mà con người có thể bỏ sót, duy trì chất lượng code một cách nhất quán.
      - **IDE (VS Code, GoLand):** Hầu hết các IDE Go hiện đại đều tích hợp sẵn `gofmt`/`goimports` và có thể tích hợp với `golangci-lint` để hiển thị cảnh báo trực tiếp khi bạn code.

- **Code minh họa / sơ đồ:**

  - Các ví dụ code đã được lồng vào phần giải thích ở trên.
  - Không có sơ đồ cụ thể cho clean code, vì nó là tập hợp các nguyên tắc và thực hành.

- **Best practices (Tóm tắt lại):**

  1.  **Ưu tiên sự rõ ràng và đơn giản.**
  2.  **Đặt tên có ý nghĩa.**
  3.  **Hàm ngắn, đơn trách nhiệm.**
  4.  **Xử lý lỗi cẩn thận và tường minh.**
  5.  **Comment "tại sao", không phải "làm gì". Sử dụng Godoc.**
  6.  **Luôn dùng `gofmt`/`goimports`.**
  7.  **Sử dụng Linters (`golangci-lint`).**
  8.  **Viết unit tests (sẽ có phần riêng).** Test giúp đảm bảo code hoạt động đúng và là một dạng tài liệu sống.
  9.  **Refactor thường xuyên:** Khi bạn thấy code smell, đừng ngại refactor để cải thiện.

- **Anti-patterns / lỗi phổ biến (Tóm tắt lại):**

  1.  Bỏ qua lỗi.
  2.  Code lồng quá sâu.
  3.  Hàm/Struct quá lớn.
  4.  Tên không rõ ràng.
  5.  Lặp code.
  6.  Sử dụng `interface{}` không cần thiết.
  7.  Comment thừa hoặc sai.
  8.  Không format code.

- **So sánh các lựa chọn / cách tiếp cận (nếu có):**

  - **Clean Code là một triết lý, không phải là một framework cứng nhắc.** Có thể có những tranh luận nhỏ về một số quy tắc cụ thể (ví dụ: độ dài dòng tối đa), nhưng các nguyên tắc cốt lõi (đặt tên, SRP, DRY, xử lý lỗi) được chấp nhận rộng rãi.
  - **Sự cân bằng:** Đôi khi, việc tuân thủ một quy tắc một cách máy móc có thể làm code phức tạp hơn. Ví dụ, tách một hàm 2 dòng thành một hàm riêng có thể không cần thiết. Luôn sử dụng phán đoán của một engineer. "Clean code is not about following rules blindly, but about applying principles thoughtfully."

- **Gợi ý mở rộng kiến thức:**
  - **Sách "Clean Code: A Handbook of Agile Software Craftsmanship" (Robert C. Martin):** Cuốn sách kinh điển về chủ đề này. Mặc dù ví dụ bằng Java, các nguyên tắc là phổ quát. Đọc nó sẽ thay đổi cách bạn nghĩ về việc viết code.
    - _Trích dẫn đáng nhớ:_ "Indeed, the ratio of time spent reading versus writing is well over 10 to 1. We are constantly reading old code as part of the effort to write new code. ...[Therefore,] making it easy to read makes it easier to write."
  - **Sách "The Pragmatic Programmer" (Andrew Hunt, David Thomas):** Một cuốn sách kinh điển khác, chứa đựng nhiều lời khuyên thực tế về việc trở thành một lập trình viên hiệu quả và chuyên nghiệp, bao gồm nhiều khía cạnh của clean code.
  - **"Effective Go" (https://go.dev/doc/effective_go):** Tài liệu chính thức của Golang team, chứa nhiều hướng dẫn về cách viết code Go idiomatic, mà phần lớn cũng là clean code.
  - **Các bài blog và talk về Go best practices:** Tìm kiếm các bài viết từ các Go developers có kinh nghiệm (ví dụ: Dave Cheney, Bill Kennedy).
  - **Review code của người khác (và nhờ người khác review code của mình):** Đây là một trong những cách tốt nhất để học và cải thiện kỹ năng viết clean code.

---

Phần 4 này nhấn mạnh rằng việc viết code không chỉ là làm cho máy tính hiểu, mà quan trọng hơn là làm cho con người hiểu. Clean code là nền tảng để xây dựng các hệ thống phần mềm chất lượng cao, dễ bảo trì và phát triển bền vững.

Khi bạn đã sẵn sàng, chúng ta sẽ chuyển sang **PHẦN 5: UNIT TESTING VÀ TEST-DRIVEN DEVELOPMENT (TDD) TRONG GO – ĐẢM BẢO CHẤT LƯỢNG VÀ THIẾT KẾ TỐT.**
Chắc chắn rồi, chúng ta sẽ tiếp tục với một phần cực kỳ quan trọng để trở thành một kỹ sư toàn diện: **PHẦN 5: UNIT TESTING VÀ TEST-DRIVEN DEVELOPMENT (TDD) TRONG GO – ĐẢM BẢO CHẤT LƯỢNG VÀ THIẾT KẾ TỐT.**

- **Tên phần học:** Unit Testing và Test-Driven Development (TDD) trong Go – Đảm bảo Chất lượng và Thiết kế Tốt
- **Mục tiêu học phần:**

  - Hiểu rõ vai trò và lợi ích của Unit Testing trong chu trình phát triển phần mềm.
  - Nắm vững cách viết Unit Test hiệu quả trong Go bằng package `testing` chuẩn và các thư viện hỗ trợ phổ biến (ví dụ: `testify`).
  - Học cách sử dụng mocks và stubs để cô lập unit-under-test, đặc biệt khi làm việc với dependencies (đã được giới thiệu trong phần DI).
  - Hiểu khái niệm Test-Driven Development (TDD), chu trình Red-Green-Refactor, và TẠI SAO TDD có thể dẫn đến thiết kế code tốt hơn.
  - Thực hành viết test cho các thành phần trong dự án Helios, đặc biệt là Entities và Use Cases.
  - Biết cách đo lường độ bao phủ của test (test coverage) và ý nghĩa của nó.

- **Giải thích lý thuyết kỹ càng:**

  1.  **Unit Testing là gì?**

      - **Định nghĩa:** Unit Test là một đoạn code tự động kiểm tra một "đơn vị" (unit) nhỏ nhất có thể kiểm thử của mã nguồn (thường là một hàm, một phương thức, hoặc đôi khi là một struct/class nhỏ) một cách cô lập.
      - **Mục tiêu:** Xác minh rằng mỗi đơn vị hoạt động đúng như mong đợi theo các kịch bản (test cases) khác nhau.
      - **Đặc điểm của một Unit Test tốt (FIRST principles):**

        - **F (Fast - Nhanh):** Unit tests nên chạy rất nhanh. Hàng trăm, hàng ngàn unit tests nên hoàn thành trong vài giây. Điều này khuyến khích việc chạy test thường xuyên.
        - **I (Independent/Isolated - Độc lập/Cô lập):** Các test case không nên phụ thuộc vào nhau. Thứ tự chạy test không ảnh hưởng đến kết quả. Mỗi test nên tự thiết lập (setup) và dọn dẹp (teardown) môi trường riêng nếu cần. Quan trọng nhất, unit-under-test phải được cô lập khỏi các dependencies bên ngoài (DB, network, file system) bằng cách sử dụng mocks/stubs.
        - **R (Repeatable - Lặp lại được):** Test phải cho ra kết quả nhất quán mỗi khi chạy, bất kể môi trường (miễn là code không đổi). Tránh phụ thuộc vào các yếu tố ngẫu nhiên hoặc trạng thái bên ngoài không kiểm soát được.
        - **S (Self-Validating - Tự xác minh):** Test phải có khả năng tự quyết định pass hay fail mà không cần sự can thiệp của con người (ví dụ: không cần nhìn vào log để biết kết quả). Nó phải có assertions (khẳng định) rõ ràng.
        - **T (Timely/Thorough - Kịp thời/Toàn diện):**
          - _Timely:_ Unit tests nên được viết _cùng lúc_ hoặc _ngay sau khi_ viết code (hoặc _trước khi_ viết code nếu theo TDD).
          - _Thorough:_ Test nên bao phủ các khía cạnh quan trọng của unit: happy paths (đường đi đúng), edge cases (trường hợp biên), error conditions (điều kiện lỗi).

      - **Tại sao Unit Testing quan trọng?**
        - **Phát hiện lỗi sớm:** Lỗi được tìm thấy ở giai đoạn unit test thường dễ sửa và ít tốn kém hơn nhiều so với lỗi tìm thấy ở các giai đoạn sau (integration, UAT, production).
        - **Tăng sự tự tin khi Refactor:** Khi bạn có một bộ unit tests tốt, bạn có thể tự tin thay đổi, tái cấu trúc code mà không sợ làm hỏng các chức năng hiện có. Tests sẽ là "mạng lưới an toàn" của bạn.
        - **Cải thiện thiết kế:** Việc phải suy nghĩ làm sao để code có thể test được (testable) thường dẫn đến thiết kế module hóa hơn, ít coupling hơn, và tuân thủ các nguyên tắc như SRP và DIP tốt hơn. Nếu một đoạn code khó test, đó thường là dấu hiệu thiết kế có vấn đề.
        - **Tài liệu sống (Living Documentation):** Unit tests mô tả cách các đơn vị code được mong đợi hoạt động. Chúng là một dạng tài liệu luôn được cập nhật cùng với code.
        - **Giảm thời gian debug:** Khi một test fail, bạn biết chính xác unit nào có vấn đề, giúp khoanh vùng lỗi nhanh hơn.

  2.  **Unit Testing trong Go với package `testing`:**

      - Go có hỗ trợ first-class cho testing thông qua package `testing` chuẩn và command `go test`.
      - **Quy ước:**
        - File test phải có tên kết thúc bằng `_test.go` (ví dụ: `user_service_test.go` cho `user_service.go`).
        - File test phải nằm trong cùng package với code được test (hoặc trong một package riêng có tên `packagename_test` để test black-box và tránh circular dependencies khi cần mock các thành phần trong cùng package).
        - Hàm test phải có dạng `func TestXxx(t *testing.T)`, trong đó `Xxx` bắt đầu bằng chữ hoa và mô tả test case.
        - `*testing.T` là một "test runner" cung cấp các phương thức để báo cáo lỗi (`t.Error`, `t.Errorf`, `t.Fatal`, `t.Fatalf`), log (`t.Log`, `t.Logf`), bỏ qua test (`t.Skip`), v.v.
      - **Các phương thức phổ biến của `*testing.T`:**
        - `t.Log(args...)`, `t.Logf(format string, args...)`: Ghi log, chỉ hiển thị nếu test fail hoặc chạy với cờ `-v`.
        - `t.Error(args...)`, `t.Errorf(format string, args...)`: Báo cáo lỗi nhưng test vẫn tiếp tục chạy các assertions khác trong cùng hàm test.
        - `t.Fatal(args...)`, `t.Fatalf(format string, args...)`: Báo cáo lỗi và dừng ngay lập tức hàm test hiện tại (nhưng các hàm test khác vẫn chạy).
        - `t.Skip(args...)`, `t.Skipf(format string, args...)`: Bỏ qua test hiện tại.
        - `t.Run(name string, f func(t *testing.T))`: Chạy một sub-test (test con). Rất hữu ích để nhóm các test case liên quan hoặc sử dụng table-driven tests.
        - `t.Parallel()`: Đánh dấu test này có thể chạy song song với các test khác cũng được đánh dấu `Parallel`.
        - `t.Cleanup(f func())`: Đăng ký một hàm sẽ được chạy sau khi test (hoặc sub-test) hoàn thành, bất kể pass hay fail. Dùng để dọn dẹp tài nguyên.

      ```go
      // mymath/mymath.go
      package mymath

      func Add(a, b int) int {
          return a + b
      }

      // mymath/mymath_test.go
      package mymath // Cùng package

      import "testing"

      func TestAdd(t *testing.T) {
          result := Add(2, 3)
          expected := 5
          if result != expected {
              t.Errorf("Add(2, 3) = %d; want %d", result, expected)
          }
      }

      func TestAdd_NegativeNumbers(t *testing.T) {
          result := Add(-1, -5)
          expected := -6
          if result != expected {
              t.Errorf("Add(-1, -5) = %d; want %d", result, expected)
          }
      }
      ```

      Chạy test: `go test ./mymath` hoặc `go test` (nếu đang ở trong thư mục `mymath`).

  3.  **Table-Driven Tests (Test dựa trên Bảng):**

      - Một pattern phổ biến trong Go để kiểm thử nhiều bộ input/output cho cùng một hàm. Giúp code test gọn gàng và dễ mở rộng.

      ```go
      // mymath/mymath_test.go
      package mymath

      import "testing"

      func TestAddTableDriven(t *testing.T) {
          // Định nghĩa một struct chứa các trường của một test case
          testCases := []struct {
              name     string // Tên của test case (cho reporting)
              a, b     int    // Input
              expected int    // Output mong đợi
          }{
              {"PositiveNumbers", 2, 3, 5},
              {"NegativeNumbers", -1, -5, -6},
              {"ZeroSum", 5, -5, 0},
              {"WithZero", 0, 10, 10},
          }

          for _, tc := range testCases {
              // Sử dụng t.Run để tạo một sub-test cho mỗi test case
              // Điều này giúp báo cáo lỗi rõ ràng hơn và có thể chạy song song (nếu tc.Parallel() được gọi)
              t.Run(tc.name, func(st *testing.T) {
                  // st.Parallel() // Nếu các test case độc lập và có thể chạy song song
                  result := Add(tc.a, tc.b)
                  if result != tc.expected {
                      st.Errorf("Add(%d, %d) = %d; want %d", tc.a, tc.b, result, tc.expected)
                  }
              })
          }
      }
      ```

  4.  **Thư viện Hỗ trợ Test: `testify`**

      - Package `testing` chuẩn của Go khá tối giản. `testify` là một thư viện phổ biến cung cấp nhiều tiện ích, đặc biệt là:
        - **`testify/assert`:** Cung cấp các hàm assertion (khẳng định) phong phú và dễ đọc hơn (ví dụ: `assert.Equal(t, expected, actual)`, `assert.NoError(t, err)`, `assert.True(t, condition)`).
        - **`testify/require`:** Tương tự `assert`, nhưng nếu assertion fail, nó sẽ gọi `t.Fatal` ngay lập tức (dừng test case đó). Hữu ích khi các bước sau phụ thuộc vào kết quả của assertion trước.
        - **`testify/mock`:** Một framework mạnh mẽ để tạo mock objects (đã xem ở Phần 3).
        - **`testify/suite`:** Cho phép nhóm các test thành các "test suites" (bộ test) với các phương thức setup/teardown dùng chung.

      ```go
      // mymath/mymath_test.go (sử dụng testify/assert)
      package mymath

      import (
          "testing"
          "github.com/stretchr/testify/assert" // Import testify/assert
          "github.com/stretchr/testify/require" // Import testify/require
      )

      func TestAddWithAssert(t *testing.T) {
          result := Add(2, 3)
          expected := 5
          assert.Equal(t, expected, result, "Add(2,3) should be 5") // assert.Equal so sánh và báo lỗi nếu không bằng

          // Ví dụ với require
          // connection, err := ConnectToDatabase()
          // require.NoError(t, err, "Failed to connect to DB, cannot proceed with test")
          // // Code tiếp theo chỉ chạy nếu err là nil
      }
      ```

      - _Tại sao dùng `testify`?_ Giúp viết test ngắn gọn hơn, dễ đọc hơn, và cung cấp các công cụ mạnh mẽ như mocking.

  5.  **Mocks và Stubs (Ôn lại và Mở rộng):**

      - **Mục tiêu:** Cô lập "unit under test" (UUT - đơn vị đang được kiểm thử) khỏi các dependencies của nó.
      - **Stub:** Một đối tượng giả cung cấp các câu trả lời được định trước cho các lời gọi trong quá trình test. Nó thường không quan tâm đến việc nó được gọi như thế nào, chỉ trả về dữ liệu giả.
        - _Ví dụ:_ Một stub `UserRepository` luôn trả về một `User` cố định khi `FindByID` được gọi.
      - **Mock:** Một đối tượng giả tinh vi hơn. Nó không chỉ cung cấp câu trả lời giả mà còn _ghi nhận_ lại các tương tác (lời gọi phương thức, tham số truyền vào) và có thể _xác minh_ rằng UUT đã tương tác với nó đúng như mong đợi (ví dụ: gọi phương thức A đúng 1 lần với tham số X).
        - Thư viện `testify/mock` giúp tạo mock objects dễ dàng.
      - **Khi nào dùng?** Khi UUT của bạn (ví dụ: một Use Case) phụ thuộc vào một interface (ví dụ: một Repository), bạn sẽ truyền một mock/stub implementation của interface đó vào UUT trong unit test.
      - **Ví dụ với `testify/mock` (đã có ở Phần 3, ví dụ `MockSensorReadingRepository`):**

        ```go
        // Nhắc lại từ Phần 3
        type MockSensorReadingRepository struct {
            mock.Mock
        }

        func (m *MockSensorReadingRepository) GetReadingsForSensor(ctx context.Context, sensorID string, lookbackWindow time.Duration) ([]entity.SensorReading, error) {
            args := m.Called(ctx, sensorID, lookbackWindow)
            // ...
            return readings, args.Error(1)
        }

        // Trong test:
        mockRepo := new(MockSensorReadingRepository)
        // Thiết lập hành vi:
        mockRepo.On("GetReadingsForSensor", mock.Anything, "sensor-1", 1*time.Hour).Return(sampleReadings, nil)
        // ... inject mockRepo vào UseCase ...
        // ... chạy UseCase ...
        // Xác minh:
        mockRepo.AssertExpectations(t) // Kiểm tra xem On() có được gọi đúng như mong đợi không
        mockRepo.AssertCalled(t, "GetReadingsForSensor", mock.Anything, "sensor-1", 1*time.Hour) // Hoặc kiểm tra cụ thể hơn
        ```

  6.  **Test-Driven Development (TDD):**

      - **Định nghĩa:** TDD là một quy trình phát triển phần mềm trong đó bạn viết unit tests _trước khi_ bạn viết code production thực sự.
      - **Chu trình TDD (Red-Green-Refactor):**
        1.  **Red (Đỏ):** Viết một unit test _nhỏ_ cho một phần chức năng nhỏ mà bạn sắp triển khai. Chạy test này. Nó phải **FAIL** (đỏ), bởi vì bạn chưa viết code cho chức năng đó. Nếu nó PASS, test của bạn có thể bị sai hoặc bạn đang test một thứ đã tồn tại.
        2.  **Green (Xanh):** Viết lượng code production _tối thiểu nhất_ cần thiết để làm cho test vừa viết PASS (xanh). Đừng lo lắng về việc code có "đẹp" hay "tối ưu" ở bước này, chỉ cần làm cho nó chạy đúng.
        3.  **Refactor (Tái cấu trúc):** Bây giờ test đã PASS, bạn có thể tự tin dọn dẹp, cải thiện code production vừa viết (ví dụ: loại bỏ trùng lặp, cải thiện tên biến/hàm, tối ưu hóa nếu cần). Chạy lại tất cả các tests sau mỗi thay đổi nhỏ để đảm bảo bạn không làm hỏng gì. Code test cũng có thể cần refactor.
            Lặp lại chu trình này cho mỗi phần chức năng nhỏ.
      - **TẠI SAO TDD? Lợi ích của TDD:**
        - **Đảm bảo Test Coverage:** Bạn luôn có test cho mọi dòng code production bạn viết (vì code production chỉ được viết để làm cho test pass).
        - **Thiết kế Tốt hơn (Emergent Design):**
          - **Tập trung vào Interface:** Khi viết test trước, bạn phải suy nghĩ về cách client (test code) sẽ sử dụng unit-under-test. Điều này buộc bạn phải thiết kế API (interface) của unit một cách rõ ràng và dễ sử dụng.
          - **Decoupling & SRP:** Để làm cho code dễ test (và viết test trước), bạn thường phải chia nhỏ vấn đề thành các unit nhỏ, độc lập, có trách nhiệm rõ ràng. Điều này tự nhiên dẫn đến code tuân thủ SRP và có coupling thấp.
          - **YAGNI:** Bạn chỉ viết code thực sự cần thiết để làm cho test hiện tại pass. Tránh viết thừa chức năng.
        - **Tự tin và Giảm Stress:** Bộ test toàn diện hoạt động như một mạng lưới an toàn, cho phép bạn refactor và thêm tính năng mới mà ít sợ làm hỏng hệ thống.
        - **Tài liệu Rõ ràng:** Tests là tài liệu chính xác nhất về cách code nên hoạt động.
        - **Phản hồi Nhanh:** Chu trình ngắn giúp bạn nhận phản hồi về thiết kế và tính đúng đắn của code rất nhanh.
      - **Thách thức của TDD:**
        - Cần thời gian để làm quen và thành thạo. Ban đầu có thể cảm thấy chậm hơn.
        - Khó áp dụng cho một số loại vấn đề (ví dụ: UI, một số thuật toán phức tạp mà kết quả khó dự đoán trước).
        - Đòi hỏi kỷ luật.
      - **Áp dụng TDD cho dự án Helios (ví dụ cho một Entity):**

        - **Yêu cầu:** Entity `SensorReading` cần một phương thức `IsValueAnomalous(rule AnomalyRule) bool`.
        - **Red:**

          ```go
          // internal/entity/sensor_reading_test.go
          package entity_test

          import (
              "project-helios/internal/entity"
              "testing"
              "github.com/stretchr/testify/assert"
          )

          func TestSensorReading_IsValueAnomalous(t *testing.T) {
              reading := entity.SensorReading{Value: 35.0}
              // Giả sử AnomalyRule có các trường Threshold và ComparisonType ("GT", "LT")
              // GT = Greater Than, LT = Less Than
              ruleHigh := entity.AnomalyRule{Threshold: 30.0, ComparisonType: "GT"}
              ruleLow := entity.AnomalyRule{Threshold: 10.0, ComparisonType: "LT"}
              ruleNormal := entity.AnomalyRule{Threshold: 40.0, ComparisonType: "GT"}


              assert.True(t, reading.IsValueAnomalous(ruleHigh), "Value 35 should be anomalous for >30 rule")
              assert.False(t, reading.IsValueAnomalous(ruleLow), "Value 35 should NOT be anomalous for <10 rule")
              assert.False(t, reading.IsValueAnomalous(ruleNormal), "Value 35 should NOT be anomalous for >40 rule")
          }
          ```

          Chạy test này sẽ fail vì `IsValueAnomalous` chưa tồn tại hoặc chưa có logic.

        - **Green:**

          ```go
          // internal/entity/sensor_reading.go
          package entity

          // ... (SensorReading struct) ...

          // IsValueAnomalous checks if the reading's value violates a given rule.
          // Tối thiểu để pass test trên
          func (sr *SensorReading) IsValueAnomalous(rule AnomalyRule) bool {
              switch rule.ComparisonType {
              case "GT":
                  return sr.Value > rule.Threshold
              case "LT":
                  return sr.Value < rule.Threshold
              // TODO: Add more comparison types like GTE, LTE, EQ, NEQ
              default:
                  return false // Or handle unknown comparison type
              }
          }
          ```

          Chạy lại test. Bây giờ nó nên PASS.

        - **Refactor:**
          - Code hiện tại khá đơn giản, có thể chưa cần refactor nhiều.
          - Có thể nghĩ đến việc thêm các hằng số cho `ComparisonType` thay vì dùng string.
          - Xem xét thêm test cases (ví dụ: giá trị bằng ngưỡng, các `ComparisonType` khác). Mỗi test case mới lại bắt đầu từ Red.

  7.  **Đo lường Độ bao phủ của Test (Test Coverage):**
      - **Định nghĩa:** Test coverage đo lường tỷ lệ phần trăm code production được thực thi bởi bộ unit tests của bạn.
      - **Lệnh trong Go:** `go test -cover` (hiển thị % coverage), `go test -coverprofile=coverage.out` (tạo file profile), `go tool cover -html=coverage.out` (tạo báo cáo HTML chi tiết).
      - **Ý nghĩa:**
        - Coverage cao (ví dụ: >80-90%) cho thấy phần lớn code của bạn đã được "chạm tới" bởi tests.
        - **NHƯNG:** Coverage 100% KHÔNG đảm bảo code không có lỗi. Bạn có thể "chạm tới" một dòng code nhưng không test hết các logic path hoặc edge cases của nó.
        - Coverage thấp là một dấu hiệu xấu, cho thấy nhiều phần code chưa được test.
      - **Mục tiêu:** Không nên ám ảnh bởi con số 100%. Hãy tập trung vào việc viết các test _có ý nghĩa_, bao phủ các kịch bản quan trọng, happy paths, edge cases, và error conditions. Một con số coverage hợp lý (ví dụ 70-90%) thường là mục tiêu tốt, tùy thuộc vào bản chất của project.
      - Sử dụng báo cáo coverage HTML để xem những dòng code nào chưa được test và quyết định xem có cần viết thêm test cho chúng không.

- **Code minh họa / sơ đồ:**

  - Các ví dụ code đã được lồng vào phần giải thích.
  - **Sơ đồ chu trình TDD:**
    ```
          +------------------Refactor------------------+
          |                                           |
          |  (Improve code & tests, run all tests)    |
          |                                           |
          V                                           A
    +-----------+        Write Code        +-----------+
    |   RED     | -----------------------> |  GREEN    |
    | (Test Fails)|     (Minimal code to   | (Test Passes)|
    +-----------+       make test pass)  +-----------+
          ^                                           |
          |                                           |
          |           Write a new Test                |
          +-------------------------------------------+
    ```

- **Best practices:**

  1.  **Test One Thing at a Time:** Mỗi hàm test nên tập trung vào một khía cạnh cụ thể hoặc một kịch bản của unit-under-test.
  2.  **AAA Pattern (Arrange, Act, Assert):**
      - **Arrange:** Thiết lập môi trường, tạo mock/stub, chuẩn bị input.
      - **Act:** Thực thi unit-under-test (gọi hàm/phương thức cần test).
      - **Assert:** Kiểm tra xem kết quả (output, trạng thái) có đúng như mong đợi không.
  3.  **Write Readable Tests:** Test code cũng là code. Nó cần dễ đọc và dễ hiểu. Sử dụng tên test case, tên biến rõ ràng.
  4.  **Test Happy Paths, Edge Cases, and Error Conditions.**
  5.  **Keep Tests Fast and Independent.**
  6.  **Sử dụng Mocks/Stubs hiệu quả để cô lập UUT.**
  7.  **Cân nhắc TDD:** Đặc biệt cho logic nghiệp vụ quan trọng. Nó có thể không phù hợp cho mọi thứ, nhưng lợi ích về thiết kế và sự tự tin là rất lớn.
  8.  **Chạy test thường xuyên:** Tích hợp vào CI/CD pipeline.

- **Anti-patterns / lỗi phổ biến:**

  1.  **Tests quá lớn, test nhiều thứ cùng lúc (Integration Test in Disguise):** Khó debug khi fail, chậm.
  2.  **Tests phụ thuộc vào nhau hoặc vào trạng thái bên ngoài (Flickering Tests):** Kết quả không ổn định.
  3.  **Tests không có Assertions hoặc Assertions yếu:** Test pass nhưng không thực sự kiểm tra gì.
  4.  **Test logic phức tạp hơn cả code production:** Test nên đơn giản và trực tiếp.
  5.  **Bỏ qua việc test các trường hợp lỗi (Error Cases).**
  6.  **Mocking quá nhiều thứ (Over-mocking):** Test có thể trở nên quá gắn chặt với implementation hiện tại, làm cho refactor khó khăn. Chỉ mock các dependencies trực tiếp mà UUT tương tác.
  7.  **Chỉ tập trung vào Coverage mà không quan tâm chất lượng Test.**

- **So sánh các lựa chọn / cách tiếp cận (nếu có):**

  - **Unit Test vs. Integration Test vs. End-to-End (E2E) Test:**

    - **Unit Test:** Kiểm tra đơn vị nhỏ nhất, cô lập. Nhanh, nhiều.
    - **Integration Test:** Kiểm tra sự tương tác giữa các unit/component (ví dụ: Use Case với Database thật, Service A gọi Service B). Chậm hơn, ít hơn unit test.
    - **E2E Test:** Kiểm tra toàn bộ luồng của ứng dụng từ đầu đến cuối (ví dụ: từ UI request đến response, qua nhiều services). Chậm nhất, ít nhất.
    - **Test Pyramid (Kim tự tháp Test):**
      ```
            / \
           /E2E\
          /-----\
         / Integr\
        /---------\
       / Unit Tests\
      /-------------\
      ```
      Nền tảng là rất nhiều Unit Tests nhanh. Lớp giữa là ít Integration Tests hơn. Đỉnh là rất ít E2E Tests.
      _Tại sao?_ Unit tests rẻ, nhanh, và cho phản hồi sớm. E2E tests đắt, chậm, và khó debug.

  - **Testing private functions/methods:**
    - Trong Go, "private" (unexported) là ở cấp độ package. Bạn vẫn có thể test các hàm unexported nếu file test nằm trong cùng package.
    - **Tuy nhiên:** Thường thì bạn nên test hành vi public của package/struct. Nếu một hàm unexported chứa logic phức tạp đáng để test riêng, có thể nó nên được đưa ra một unit riêng (có thể là một struct/interface mới) hoặc logic của nó nên được test gián tiếp thông qua các hàm public gọi nó. "Test the interface, not the implementation."

- **Gợi ý mở rộng kiến thức:**
  - **Sách "Test Driven Development: By Example" (Kent Beck):** Cuốn sách kinh điển khai sinh ra TDD.
  - **Sách "Working Effectively with Legacy Code" (Michael Feathers):** Cung cấp nhiều kỹ thuật để đưa code cũ (không có test) vào trạng thái có thể test được, rất hữu ích.
  - **Blog của Martin Fowler về Testing:** Nhiều bài viết sâu sắc về các khía cạnh khác nhau của testing.
  - **Go's `testing` package documentation:** (https://pkg.go.dev/testing)
  - **Testify documentation:** (https://github.com/stretchr/testify)
  - **Trích dẫn đáng nhớ (Kent Beck):** "I'm not a great programmer; I'm just a good programmer with great habits." (TDD là một trong những thói quen đó).
  - Thực hành viết nhiều unit test cho dự án Helios. Thử áp dụng TDD cho một Use Case mới.

---

Phần 5 này là một bước tiến quan trọng. Unit testing và TDD không chỉ là về việc tìm lỗi, mà còn là về việc xây dựng phần mềm chất lượng cao với thiết kế tốt và sự tự tin khi thay đổi.

Khi bạn đã sẵn sàng, chúng ta sẽ đến với **PHẦN 6: QUẢN LÝ CONFIGURATION TRONG ỨNG DỤNG GO – LINH HOẠT VÀ AN TOÀN.**

Tuyệt vời! Chúng ta cùng khám phá **PHẦN 6: QUẢN LÝ CONFIGURATION TRONG ỨNG DỤNG GO – LINH HOẠT VÀ AN TOÀN.**

- **Tên phần học:** Quản lý Configuration trong Ứng dụng Go – Linh hoạt và An toàn
- **Mục tiêu học phần:**

  - Hiểu TẠI SAO việc quản lý cấu hình (configuration) một cách tách biệt và linh hoạt lại quan trọng cho các ứng dụng, đặc biệt là microservices.
  - Nắm vững các nguồn phổ biến để nạp cấu hình: biến môi trường (environment variables), file cấu hình (JSON, YAML, TOML, .env), và command-line flags.
  - Học cách sử dụng các thư viện Go phổ biến để quản lý cấu hình (ví dụ: `spf13/viper`, `joho/godotenv`).
  - Hiểu các best practices về việc cấu trúc dữ liệu cấu hình, xử lý các kiểu dữ liệu khác nhau, và cung cấp giá trị mặc định.
  - Biết cách tích hợp cấu hình vào ứng dụng Go một cách clean, đặc biệt là cách truyền cấu hình vào các components thông qua Dependency Injection.
  - Nhận thức được các vấn đề về bảo mật liên quan đến cấu hình (ví dụ: secrets management) và các giải pháp sơ bộ.

- **Giải thích lý thuyết kỹ càng:**

  1.  **Configuration là gì và Tại sao nó Quan trọng?**

      - **Định nghĩa:** Configuration (cấu hình) là tập hợp các tham số điều chỉnh hành vi của một ứng dụng mà không cần thay đổi mã nguồn của nó. Ví dụ:
        - Chuỗi kết nối cơ sở dữ liệu (DB connection string)
        - Địa chỉ và port của các service khác mà ứng dụng cần gọi
        - API keys cho các dịch vụ bên thứ ba
        - Cấp độ log (log level: DEBUG, INFO, ERROR)
        - Số lượng worker threads
        - Tên môi trường (development, staging, production)
      - **Tại sao quản lý configuration riêng biệt lại quan trọng (The Twelve-Factor App - III. Config):**
        - **Linh hoạt giữa các môi trường:** Cùng một codebase có thể chạy ở nhiều môi trường (dev, test, staging, prod) chỉ bằng cách thay đổi cấu hình. Code không nên chứa các giá trị hard-code cho từng môi trường.
        - **Bảo mật:** Các thông tin nhạy cảm (secrets) như mật khẩu DB, API keys không nên được commit vào version control. Chúng nên được cung cấp qua các kênh an toàn hơn (ví dụ: biến môi trường, secret management systems).
        - **Dễ dàng vận hành (Operability):** System administrators hoặc DevOps engineers có thể thay đổi hành vi của ứng dụng mà không cần lập trình viên can thiệp hay deploy lại code.
        - **Khả năng mở rộng (Scalability):** Khi triển khai nhiều instance của một service, mỗi instance có thể có cấu hình hơi khác nhau (ví dụ: kết nối đến một DB replica khác nhau) mà không cần build lại.
        - **Clean Code:** Tách biệt cấu hình khỏi logic nghiệp vụ giúp code sạch sẽ và tập trung hơn vào nhiệm vụ chính.

  2.  **Các Nguồn Cấu hình Phổ biến:**

      - **a. Biến Môi trường (Environment Variables):**

        - Đây là một cách rất phổ biến và được khuyến nghị bởi The Twelve-Factor App.
        - Các biến được thiết lập trong môi trường mà process chạy (ví dụ: trong Dockerfile, Kubernetes deployment YAML, hoặc thiết lập trực tiếp trên server).
        - Go đọc biến môi trường qua package `os`: `os.Getenv("MY_VARIABLE")`.
        - _Ưu điểm:_
          - Độc lập ngôn ngữ và OS.
          - Rất tốt cho việc triển khai trong container và các môi trường PaaS/cloud.
          - Thường được coi là an toàn hơn cho secrets so với việc lưu trong file (nếu môi trường được bảo vệ tốt).
        - _Nhược điểm:_
          - Khó quản lý khi có quá nhiều biến.
          - Chỉ hỗ trợ kiểu chuỗi (string), cần phải parse sang các kiểu khác (int, bool).
          - Không tiện cho local development nếu phải set thủ công nhiều biến.

        ```go
        package main

        import (
            "fmt"
            "os"
            "strconv"
        )

        func main_env_vars() {
            dbHost := os.Getenv("DB_HOST")
            if dbHost == "" {
                dbHost = "localhost" // Default value
            }

            dbPortStr := os.Getenv("DB_PORT")
            dbPort, err := strconv.Atoi(dbPortStr)
            if err != nil {
                dbPort = 5432 // Default value if parsing fails or not set
            }

            fmt.Printf("DB Host: %s, DB Port: %d\n", dbHost, dbPort)
        }
        ```

      - **b. File Cấu hình (Configuration Files):**

        - Lưu trữ cấu hình trong các file như JSON, YAML, TOML, INI, hoặc `.env`.
        - _Ưu điểm:_
          - Có cấu trúc, dễ đọc và quản lý các cấu hình phức tạp, lồng nhau.
          - Hỗ trợ nhiều kiểu dữ liệu tự nhiên.
          - Tốt cho local development (có thể commit file cấu hình mẫu, file cấu hình thực tế được ignore).
        - _Nhược điểm:_
          - Cần cẩn thận không commit file chứa secrets vào version control.
          - Việc load và parse file thêm một chút overhead.
          - Cần cơ chế để ghi đè giá trị từ file bằng biến môi trường cho các môi trường khác nhau.
        - **Định dạng phổ biến:**

          - **JSON (JavaScript Object Notation):** Rất phổ biến, dễ parse, nhưng không hỗ trợ comment.
            ```json
            // config.json
            {
              "server": {
                "port": 8080,
                "timeoutSeconds": 30
              },
              "database": {
                "host": "localhost",
                "port": 5432,
                "user": "appuser"
              }
            }
            ```
          - **YAML (YAML Ain't Markup Language):** Dễ đọc hơn JSON, hỗ trợ comment, cấu trúc thụt lề.
            ```yaml
            # config.yaml
            server:
              port: 8080
              timeoutSeconds: 30
            database:
              host: localhost
              port: 5432
              user: appuser
              # password: "SHOULD_BE_FROM_ENV_OR_SECRET_MANAGER"
            ```
          - **TOML (Tom's Obvious, Minimal Language):** Dễ đọc, được thiết kế để dễ parse.

            ```toml
            # config.toml
            [server]
            port = 8080
            timeoutSeconds = 30

            [database]
            host = "localhost"
            port = 5432
            user = "appuser"
            ```

          - **.env files:** Các file text đơn giản chứa các cặp `KEY=VALUE`, thường được dùng cho local development để mô phỏng biến môi trường. Thư viện `joho/godotenv` giúp load các file này.
            ```
            # .env
            DB_HOST=localhost
            DB_PORT=5432
            API_KEY=your_local_api_key
            ```

      - **c. Command-Line Flags (Cờ Dòng lệnh):**

        - Các tham số được truyền vào khi chạy ứng dụng từ command line.
        - Go có package `flag` chuẩn để xử lý.
        - _Ưu điểm:_
          - Tiện lợi cho việc ghi đè nhanh một vài giá trị cấu hình hoặc cho các tool CLI.
        - _Nhược điểm:_
          - Không phù hợp cho số lượng lớn cấu hình hoặc cấu hình phức tạp.
          - Khó quản lý trong môi trường production tự động hóa.

        ```go
        package main

        import (
            "flag"
            "fmt"
        )

        func main_flags() {
            port := flag.Int("port", 8080, "HTTP port to listen on")
            logLevel := flag.String("loglevel", "info", "Log level (debug, info, warn, error)")
            // Cần gọi flag.Parse() để các flag được đọc
            flag.Parse()

            fmt.Printf("Port: %d, LogLevel: %s\n", *port, *logLevel)
            // Truy cập các flag khác không được định nghĩa: flag.Args()
        }
        ```

  3.  **Thư viện Quản lý Cấu hình: `spf13/viper`**

      - Viper là một thư viện rất mạnh mẽ và phổ biến trong Go để xử lý configuration.
      - **Các tính năng chính của Viper:**
        - **Hỗ trợ nhiều nguồn:** Đọc từ file (JSON, YAML, TOML, HCL, .env, Java properties), biến môi trường, command-line flags, remote K/V stores (Etcd, Consul, Firestore - cần extension).
        - **Ưu tiên nguồn (Precedence):** Định nghĩa thứ tự ưu tiên (ví dụ: flag > env var > config file > default).
        - **Giá trị mặc định (Default Values):** Dễ dàng thiết lập giá trị mặc định.
        - **Binding vào Struct:** Tự động unmarshal (giải mã) cấu hình vào một struct Go.
        - **Watch Config File:** Tự động theo dõi thay đổi trong file cấu hình và reload (nếu được kích hoạt).
        - **Aliasing:** Đặt tên alias cho các key cấu hình.
      - **Cách sử dụng cơ bản với Viper:**

        ```go
        // config/config.go
        package config

        import (
            "fmt"
            "strings"
            "time"

            "github.com/spf13/viper"
        )

        // AppConfig là struct chứa toàn bộ cấu hình ứng dụng
        type AppConfig struct {
            Server   ServerConfig   `mapstructure:"server"`
            Database DatabaseConfig `mapstructure:"database"`
            Redis    RedisConfig    `mapstructure:"redis"`
            LogLevel string         `mapstructure:"logLevel"`
        }

        type ServerConfig struct {
            Port           int           `mapstructure:"port"`
            ReadTimeout    time.Duration `mapstructure:"readTimeout"`
            WriteTimeout   time.Duration `mapstructure:"writeTimeout"`
            MaxHeaderBytes int           `mapstructure:"maxHeaderBytes"`
        }

        type DatabaseConfig struct {
            Host     string `mapstructure:"host"`
            Port     int    `mapstructure:"port"`
            User     string `mapstructure:"user"`
            Password string `mapstructure:"password"` // Sẽ nói về secrets sau
            DBName   string `mapstructure:"dbName"`
            SSLMode  string `mapstructure:"sslMode"`
        }

        type RedisConfig struct {
            Address  string `mapstructure:"address"`
            Password string `mapstructure:"password"`
            DB       int    `mapstructure:"db"`
        }

        var Cfg AppConfig // Biến toàn cục để chứa config (sẽ cải tiến bằng DI)

        // LoadConfig tải cấu hình từ file và biến môi trường
        func LoadConfig(configPath string) (*AppConfig, error) {
            v := viper.New()

            // 1. Đặt tên file cấu hình (không có đuôi)
            v.SetConfigName("config") // Ví dụ: config.yaml, config.json
            // 2. Đặt loại file cấu hình
            v.SetConfigType("yaml") // Hoặc "json", "toml"
            // 3. Đặt đường dẫn đến file cấu hình
            if configPath != "" {
                v.AddConfigPath(configPath) // Đường dẫn cụ thể
            }
            v.AddConfigPath(".")          // Thư mục hiện tại
            v.AddConfigPath("./config")   // Thư mục config/
            v.AddConfigPath("/etc/app/")  // Đường dẫn cho production
            v.AddConfigPath("$HOME/.app") // Đường dẫn user-specific

            // 4. Đọc file cấu hình (nếu tìm thấy)
            if err := v.ReadInConfig(); err != nil {
                if _, ok := err.(viper.ConfigFileNotFoundError); ok {
                    // File không tìm thấy, không sao nếu có env vars hoặc defaults
                    fmt.Println("Config file not found, relying on defaults and env vars.")
                } else {
                    // Lỗi đọc file cấu hình
                    return nil, fmt.Errorf("failed to read config file: %w", err)
                }
            }

            // 5. Thiết lập đọc từ biến môi trường
            v.AutomaticEnv()                               // Tự động đọc các biến môi trường khớp key
            v.SetEnvKeyReplacer(strings.NewReplacer(".", "_")) // Thay thế dấu "." bằng "_" cho env var keys (SERVER.PORT -> SERVER_PORT)
            v.SetEnvPrefix("HELIOS")                       // Ví dụ: HELIOS_SERVER_PORT

            // 6. Thiết lập giá trị mặc định
            // Server defaults
            v.SetDefault("server.port", 8080)
            v.SetDefault("server.readTimeout", "15s") // Có thể parse duration string
            v.SetDefault("server.writeTimeout", "15s")
            v.SetDefault("server.maxHeaderBytes", 1<<20) // 1 MB

            // Database defaults (một số nên để trống và yêu cầu qua env)
            v.SetDefault("database.host", "localhost")
            v.SetDefault("database.port", 5432)
            v.SetDefault("database.sslMode", "disable")
            // v.SetDefault("database.user", "appuser") // Nên yêu cầu qua env
            // v.SetDefault("database.dbName", "helios_db") // Nên yêu cầu qua env

            // Redis defaults
            v.SetDefault("redis.address", "localhost:6379")
            v.SetDefault("redis.db", 0)

            v.SetDefault("logLevel", "info")

            // 7. Unmarshal cấu hình vào struct
            var cfg AppConfig
            if err := v.Unmarshal(&cfg); err != nil {
                return nil, fmt.Errorf("failed to unmarshal config: %w", err)
            }

            // 8. (Optional) Validate cấu hình
            // Ví dụ: kiểm tra các trường bắt buộc
            if cfg.Database.User == "" {
                return nil, fmt.Errorf("database user (HELIOS_DATABASE_USER) is required")
            }
            if cfg.Database.Password == "" {
                // Cảnh báo nếu đang ở môi trường không phải dev
                // Hoặc yêu cầu bắt buộc tùy theo chính sách
                fmt.Println("WARN: Database password (HELIOS_DATABASE_PASSWORD) is not set.")
            }


            Cfg = cfg // Gán vào biến toàn cục (sẽ thay bằng DI)
            return &cfg, nil
        }
        ```

        - **Thứ tự ưu tiên của Viper (mặc định, có thể cấu hình):**
          1.  Explicit `Set()` call
          2.  Command-line flags
          3.  Environment variables
          4.  Configuration file
          5.  Key/Value store (remote)
          6.  Default values

      - **Thư viện `joho/godotenv`:**

        - Đơn giản hơn Viper, chỉ tập trung vào việc load các biến từ file `.env` vào môi trường (để `os.Getenv` có thể đọc được).
        - Thường dùng cho local development.

        ```go
        import "github.com/joho/godotenv"
        import "os"
        import "log"

        func main_dotenv() {
            // Load .env file (nếu có)
            // Load sẽ không ghi đè các biến môi trường đã tồn tại
            // err := godotenv.Load()
            // Overload sẽ ghi đè
            err := godotenv.Overload(".env.local", ".env") // Load nhiều file, file sau ghi đè file trước
            if err != nil {
                log.Println("No .env file found or error loading, relying on system env vars")
            }

            apiKey := os.Getenv("API_KEY")
            dbHost := os.Getenv("DB_HOST")
            fmt.Printf("API Key: %s, DB Host: %s\n", apiKey, dbHost)
        }
        ```

        - Có thể kết hợp `godotenv` (cho local dev) với Viper (để Viper đọc từ env vars đã được `godotenv` nạp).

  4.  **Cấu trúc Dữ liệu Cấu hình và Tích hợp vào Ứng dụng:**

      - **Sử dụng Struct:** Định nghĩa một struct (ví dụ: `AppConfig` ở trên) để giữ toàn bộ cấu hình. Các trường của struct nên được export (viết hoa chữ cái đầu) và có `mapstructure` tags để Viper có thể unmarshal.
      - **Truyền Cấu hình qua Dependency Injection:**

        - Thay vì dùng biến config toàn cục (`config.Cfg`), hãy inject (tiêm) struct `AppConfig` (hoặc các phần con của nó như `DatabaseConfig`, `ServerConfig`) vào các components cần chúng thông qua constructor.

        ```go
        // cmd/server/main.go (Sửa đổi từ Phần 3)
        package main

        import (
            // ...
            "project-helios/config" // Import package config của chúng ta
            "project-helios/internal/adapter/repository/postgres"
            "project-helios/internal/usecase"
            infraDb "project-helios/internal/infrastructure/database"
            // ...
        )

        func main() {
            // 1. Load Configuration
            // Đường dẫn file config có thể lấy từ flag hoặc env var
            cfg, err := config.LoadConfig("./config") // Hoặc "" để tự tìm
            if err != nil {
                log.Fatalf("Failed to load configuration: %v", err)
            }

            // 2. Infrastructure Layer: Khởi tạo DB connection với config
            // Truyền DatabaseConfig vào hàm khởi tạo DB
            dbConnString := fmt.Sprintf("host=%s port=%d user=%s password=%s dbname=%s sslmode=%s",
                cfg.Database.Host, cfg.Database.Port, cfg.Database.User, cfg.Database.Password, cfg.Database.DBName, cfg.Database.SSLMode)

            db, err := infraDb.NewPostgresDB(dbConnString) // Hoặc NewPostgresDB có thể nhận DatabaseConfig
            if err != nil {
                log.Fatalf("Failed to connect to database: %v", err)
            }
            defer db.Close()
            log.Println("Successfully connected to the database.")

            // 3. Adapter Layer: Repository Implementations
            anomalyRepoImpl := postgres.NewPostgresAnomalyRepository(db)
            // ... các repo khác ...

            // 4. UseCase Layer: Use Cases
            detectAnomalyUC := usecase.NewDetectAnomalyUseCase(
                /* sensorReadingRepoImpl */ nil, // Sẽ thêm sau
                /* anomalyRuleRepoImpl */ nil,   // Sẽ thêm sau
                anomalyRepoImpl,
            )

            // 5. Adapter Layer (Handlers)
            // anomalyAPIHandler := handler.NewAnomalyAPIHandler(detectAnomalyUC, cfg.Server) // Handler có thể cần ServerConfig

            // 6. Infrastructure Layer (Router & Server)
            // r := router.NewGinRouter()
            // server := &http.Server{
            //     Addr:           fmt.Sprintf(":%d", cfg.Server.Port),
            //     Handler:        r,
            //     ReadTimeout:    cfg.Server.ReadTimeout,
            //     WriteTimeout:   cfg.Server.WriteTimeout,
            //     MaxHeaderBytes: cfg.Server.MaxHeaderBytes,
            // }

            log.Printf("Log level set to: %s", cfg.LogLevel)
            // ... (start server)
        }
        ```

        - _Lợi ích của DI cho config:_
          - **Rõ ràng Dependencies:** Component nào cần config gì sẽ được khai báo rõ trong constructor.
          - **Testability:** Dễ dàng cung cấp config giả (mock config) trong unit tests.
          - **Tránh biến toàn cục:** Giảm coupling ngầm.

  5.  **Bảo mật Cấu hình (Secrets Management):**
      - **Không bao giờ commit secrets vào Git:** Mật khẩu, API keys, private keys là secrets.
      - **Các phương pháp quản lý Secrets:**
        - **Environment Variables (trong môi trường được bảo vệ):** Cách đơn giản nhất. Đảm bảo rằng chỉ những người/process được ủy quyền mới có quyền truy cập vào các biến môi trường này trên server/container.
        - **Secret Management Systems:**
          - **HashiCorp Vault:** Một công cụ mã nguồn mở rất mạnh mẽ để quản lý secrets. Ứng dụng sẽ xác thực với Vault và lấy secrets lúc runtime.
          - **Cloud Provider Secrets Managers:** AWS Secrets Manager, Google Cloud Secret Manager, Azure Key Vault. Tích hợp tốt với các dịch vụ cloud khác.
          - **Kubernetes Secrets:** Lưu trữ secrets trong Kubernetes cluster.
        - **Encrypted Config Files:** Mã hóa file cấu hình chứa secrets, và giải mã nó lúc runtime bằng một key được cung cấp an toàn (ví dụ qua env var hoặc một secret manager khác).
      - **Nguyên tắc Least Privilege:** Mỗi ứng dụng/service chỉ nên có quyền truy cập vào những secrets mà nó thực sự cần.
      - **Rotation:** Secrets (đặc biệt là password, API keys) nên được xoay vòng (thay đổi) định kỳ.
      - _Cho dự án Helios (mức độ học tập):_ Chúng ta sẽ bắt đầu bằng việc đọc secrets từ biến môi trường (ví dụ: `HELIOS_DATABASE_PASSWORD`). Việc tích hợp với Vault hay Cloud Secret Manager là một chủ đề nâng cao hơn.

- **Code minh họa / sơ đồ:**

  - Các ví dụ code đã được lồng vào phần giải thích.
  - **Sơ đồ luồng tải cấu hình với Viper:**
    ```
    Defaults <--- Config File (YAML/JSON/TOML) <--- Env Vars <--- Flags <--- Explicit Set
    (Ưu tiên thấp nhất)                                           (Ưu tiên cao nhất)
         |
         V
    Viper Instance
         |
         V (Unmarshal)
    AppConfig Struct (trong code Go)
         |
         V (Dependency Injection)
    Các Component của Ứng dụng (Use Cases, Repositories, Handlers)
    ```

- **Best practices:**

  1.  **Tuân thủ The Twelve-Factor App (III. Config):** Lưu trữ config trong môi trường.
  2.  **Sử dụng thư viện như Viper:** Để quản lý sự phức tạp của việc load từ nhiều nguồn và unmarshal.
  3.  **Định nghĩa một Struct trung tâm cho cấu hình:** Giúp tổ chức và type-safety.
  4.  **Sử dụng Dependency Injection để cung cấp config:** Tránh biến toàn cục.
  5.  **Cung cấp giá trị mặc định hợp lý:** Giúp ứng dụng chạy được với cấu hình tối thiểu, đặc biệt cho local dev.
  6.  **Validate cấu hình khi khởi động:** Fail-fast nếu thiếu các config quan trọng hoặc giá trị không hợp lệ.
  7.  **Quản lý Secrets một cách An toàn:** Không hard-code, không commit vào Git. Sử dụng env vars (trong môi trường an toàn) hoặc secret management systems.
  8.  **Tách biệt cấu hình cho từng môi trường:** Sử dụng các file config khác nhau (ví dụ: `config.dev.yaml`, `config.prod.yaml` - được chọn bởi một env var) hoặc chủ yếu dựa vào env vars để ghi đè.

- **Anti-patterns / lỗi phổ biến:**

  1.  **Hard-coding configuration values:** Đặc biệt là secrets hoặc các giá trị thay đổi giữa các môi trường.
  2.  **Committing secrets vào version control.**
  3.  **Không có giá trị mặc định hoặc xử lý thiếu cấu hình kém:** Dẫn đến crash ứng dụng khó hiểu.
  4.  **Sử dụng biến config toàn cục một cách tràn lan:** Gây coupling ngầm, khó test.
  5.  **Cấu trúc config quá phẳng hoặc quá phức tạp, khó hiểu.**
  6.  **Không phân biệt rõ ràng giữa config của ứng dụng và config của OS/runtime.**

- **So sánh các lựa chọn / cách tiếp cận (nếu có):**

  - **Viper vs. `flag` + `os.Getenv` + manual JSON/YAML parsing:**
    - Với các ứng dụng đơn giản, cách thủ công có thể đủ.
    - Viper mang lại sự tiện lợi, quản lý thứ tự ưu tiên, unmarshalling tự động, và các tính năng nâng cao khi ứng dụng phức tạp hơn và cần đọc từ nhiều nguồn.
  - **Viper vs. Các thư viện config khác (ví dụ: `kelseyhightower/envconfig`, `Netflix/go-env`):**
    - `envconfig` và `go-env` tập trung chủ yếu vào việc load từ biến môi trường vào struct.
    - Viper toàn diện hơn, hỗ trợ nhiều nguồn hơn. Lựa chọn tùy thuộc vào mức độ phức tạp cần thiết.

- **Gợi ý mở rộng kiến thức:**
  - **The Twelve-Factor App - Config:** (https://12factor.net/config) - Đọc kỹ để hiểu triết lý.
  - **Viper Documentation:** (https://github.com/spf13/viper) - Rất chi tiết.
  - **HashiCorp Vault Documentation:** (https://www.vaultproject.io/docs) - Để tìm hiểu về một hệ thống quản lý secrets chuyên nghiệp.
  - Nghiên cứu về cách các nền tảng (Kubernetes, Docker Swarm, Cloud Providers) quản lý configuration và secrets cho ứng dụng.
  - **Trích dẫn (Kelsey Hightower):** "Stop hardcoding, start configuring." (Ngừng hardcode, hãy bắt đầu cấu hình.)

---

Phần 6 này đã trang bị cho bạn kiến thức để quản lý cấu hình ứng dụng Go một cách chuyên nghiệp, linh hoạt và an toàn. Đây là một kỹ năng thiết yếu cho bất kỳ kỹ sư nào xây dựng các hệ thống đáng tin cậy.

Tiếp theo, chúng ta sẽ đi vào **PHẦN 7: LOGGING HIỆU QUẢ TRONG GO – GHI NHẬN THÔNG TIN VÀ GỠ LỖI.**

OK, chúng ta sẽ tiếp tục với **PHẦN 7: LOGGING HIỆU QUẢ TRONG GO – GHI NHẬN THÔNG TIN VÀ GỠ LỖI.**

- **Tên phần học:** Logging Hiệu quả trong Go – Ghi nhận Thông tin và Gỡ lỗi
- **Mục tiêu học phần:**

  - Hiểu TẠI SAO logging lại cực kỳ quan trọng trong các ứng dụng, đặc biệt là microservices phân tán.
  - Nắm vững các cấp độ log (log levels) phổ biến và khi nào nên sử dụng chúng.
  - Học cách sử dụng package `log` chuẩn của Go và các thư viện logging có cấu trúc (structured logging) phổ biến (ví dụ: `uber-go/zap`, `rs/zerolog`).
  - Hiểu lợi ích của structured logging so với plain text logging.
  - Biết cách thêm ngữ cảnh (contextual information) vào log messages để gỡ lỗi hiệu quả hơn.
  - Học cách tích hợp logging với hệ thống quản lý cấu hình (để điều chỉnh log level, output format).
  - Nhận thức được các best practices về việc log gì, log ở đâu, và tránh các lỗi logging phổ biến.

- **Giải thích lý thuyết kỹ càng:**

  1.  **Tại sao Logging Quan trọng?**

      - **Khả năng quan sát (Observability):** Logs là một trong ba trụ cột của observability (cùng với metrics và traces). Chúng cung cấp một bản ghi chi tiết về các sự kiện và trạng thái của ứng dụng theo thời gian.
      - **Gỡ lỗi (Debugging):** Khi lỗi xảy ra, đặc biệt là ở môi trường production mà bạn không thể attach debugger, logs thường là công cụ chính để hiểu chuyện gì đã xảy ra, dòng chảy của request, và giá trị của các biến quan trọng.
      - **Giám sát (Monitoring):** Logs có thể được thu thập, phân tích để phát hiện các mẫu bất thường, lỗi lặp lại, hoặc các vấn đề về hiệu năng.
      - **Audit (Kiểm toán):** Ghi lại các hành động quan trọng của người dùng hoặc hệ thống cho mục đích bảo mật và tuân thủ.
      - **Hiểu hành vi ứng dụng:** Phân tích logs giúp hiểu cách người dùng tương tác với hệ thống, các tính năng nào được sử dụng nhiều, và các bottleneck tiềm ẩn.
      - **Trong Microservices:** Khi một request đi qua nhiều services, việc có logs nhất quán và có thể tương quan (correlated) giữa các services là tối quan trọng để theo dõi và gỡ lỗi toàn bộ giao dịch. (Sẽ nói thêm về distributed tracing sau).

  2.  **Các Cấp độ Log (Log Levels):**

      - Log levels giúp phân loại mức độ quan trọng của một thông điệp log, cho phép bạn lọc và tập trung vào những gì cần thiết.
      - Các cấp độ phổ biến (thứ tự từ ít nghiêm trọng đến nghiêm trọng nhất):
        - **TRACE (hoặc DEBUG siêu chi tiết):** Thông tin rất chi tiết, thường chỉ dùng cho việc gỡ lỗi sâu bởi lập trình viên. Ví dụ: giá trị của mọi biến, mỗi bước nhỏ trong một thuật toán. _Không nên bật ở production trừ khi đang troubleshoot một vấn đề cụ thể._
        - **DEBUG:** Thông tin chi tiết hữu ích cho việc gỡ lỗi. Ví dụ: đầu vào/đầu ra của hàm quan trọng, các quyết định logic. _Có thể bật ở dev/staging, hoặc tạm thời ở production khi cần._
        - **INFO:** Các sự kiện thông thường, mang tính thông tin trong quá trình hoạt động bình thường của ứng dụng. Ví dụ: server đã khởi động, request được xử lý thành công, một tác vụ định kỳ hoàn thành. _Thường được bật ở production._
        - **WARN (Warning):** Một điều gì đó bất thường hoặc không mong muốn đã xảy ra, nhưng ứng dụng vẫn có thể tiếp tục hoạt động. Nó có thể là dấu hiệu của một vấn đề tiềm ẩn. Ví dụ: một service phụ thuộc trả về lỗi có thể retry, sử dụng giá trị mặc định do cấu hình thiếu.
        - **ERROR:** Một lỗi đã xảy ra khiến một phần hoạt động cụ thể không thành công, nhưng ứng dụng tổng thể vẫn có thể chạy (ví dụ, một request cụ thể fail nhưng server vẫn nhận các request khác). Ví dụ: không thể kết nối DB, không thể ghi file, một request không hợp lệ.
        - **FATAL (hoặc CRITICAL):** Một lỗi nghiêm trọng khiến ứng dụng không thể tiếp tục hoạt động và phải dừng lại. Ví dụ: không thể load cấu hình bắt buộc, không thể bind vào port. Sau khi log FATAL, ứng dụng thường sẽ exit.
      - **Tư duy:** Chọn đúng log level rất quan trọng. Log quá nhiều ở level INFO có thể làm "ngập lụt" hệ thống log và tốn chi phí lưu trữ. Log quá ít có thể khiến bạn thiếu thông tin khi cần gỡ lỗi.

  3.  **Logging trong Go:**

      - **a. Package `log` chuẩn của Go:**

        - Cung cấp các chức năng logging cơ bản.
        - Mặc định log ra `os.Stderr`.
        - Có thể cấu hình output (ghi ra file), prefix, và flags (ngày giờ, file/line).

        ```go
        package main

        import (
            "log"
            "os"
        )

        func main_std_log() {
            // Log ra stdout
            log.Println("Đây là một thông điệp INFO bình thường.")

            // Cấu hình logger tùy chỉnh
            file, err := os.OpenFile("app.log", os.O_CREATE|os.O_WRONLY|os.O_APPEND, 0666)
            if err != nil {
                log.Fatal("Không thể mở file log:", err)
            }
            defer file.Close()

            // Logger ghi ra file, có prefix và hiển thị ngày giờ, file/line ngắn
            fileLogger := log.New(file, "MYAPP: ", log.Ldate|log.Ltime|log.Lshortfile)
            fileLogger.Println("Thông điệp này sẽ vào file app.log")
            fileLogger.Printf("Giá trị là %d", 123)

            // Nếu có lỗi nghiêm trọng
            // log.Fatal("Lỗi nghiêm trọng, ứng dụng dừng.") // Sẽ gọi os.Exit(1)
            // log.Panic("Lỗi panic, có thể recover.") // Sẽ gọi panic()
        }
        ```

        - _Nhược điểm của `log` chuẩn:_
          - Không hỗ trợ log levels một cách tự nhiên (phải tự implement nếu muốn).
          - Không hỗ trợ structured logging.
          - Logger mặc định (`log.Printf`, `log.Println`) là global, có thể gây race condition nếu nhiều goroutine cùng thay đổi output hoặc flags của nó (mặc dù các hàm Print của nó là an toàn đồng thời).

      - **b. Structured Logging (Logging có Cấu trúc):**

        - **Khái niệm:** Thay vì log ra các dòng text tự do, structured logging ghi log messages dưới dạng các cặp key-value có cấu trúc (thường là JSON hoặc logfmt).
          - _Plain text log:_ `2023-10-27T10:30:00Z INFO Request processed successfully for user 123, duration 55ms`
          - _Structured log (JSON):_
            ```json
            {
              "level": "info",
              "timestamp": "2023-10-27T10:30:00Z",
              "message": "Request processed successfully",
              "user_id": "123",
              "duration_ms": 55,
              "service": "order-service",
              "request_id": "xyz-789"
            }
            ```
        - **TẠI SAO Structured Logging? Lợi ích:**
          - **Dễ dàng Parse và Phân tích:** Máy móc (log aggregation systems như Elasticsearch, Splunk, Datadog) có thể dễ dàng parse, index, và truy vấn logs. Bạn có thể tìm kiếm, lọc, và tạo dashboard dựa trên các trường cụ thể (ví dụ: tất cả logs ERROR của `user_id="123"`).
          - **Nhất quán:** Định dạng log nhất quán giữa các message và các service.
          - **Giàu ngữ cảnh:** Dễ dàng thêm nhiều trường ngữ cảnh mà không làm message trở nên khó đọc đối với con người (vì con người sẽ đọc qua UI của log aggregator).
        - **Các thư viện Structured Logging phổ biến trong Go:**
          - **`uber-go/zap`:** Rất nhanh, API phong phú, được thiết kế cho hiệu năng cao. Có hai API chính: "sugared" (giống `Printf`) và "structured" (strongly-typed fields).
          - **`rs/zerolog`:** Cũng rất nhanh, tập trung vào zero allocation (không cấp phát bộ nhớ) trong một số trường hợp. API đơn giản và mạnh mẽ.
          - `sirupsen/logrus`: Một trong những thư viện đầu tiên, API giống `log` chuẩn nhưng có levels và structured logging. Tuy nhiên, `zap` và `zerolog` thường được ưa chuộng hơn về hiệu năng gần đây.

      - **c. Ví dụ với `uber-go/zap`:**

        ```go
        package main

        import (
            "time"
            "go.uber.org/zap"
            "go.uber.org/zap/zapcore"
        )

        var sugar *zap.SugaredLogger // SugaredLogger cho API tiện lợi
        var logger *zap.Logger       // Logger cho API có cấu trúc, hiệu năng cao hơn

        func InitZapLogger(logLevelStr string, isProduction bool) {
            var level zapcore.Level
            err := level.UnmarshalText([]byte(logLevelStr))
            if err != nil {
                level = zapcore.InfoLevel // Default
                log.Printf("Invalid log level '%s', defaulting to 'info'", logLevelStr)
            }

            var cfg zap.Config
            if isProduction {
                cfg = zap.NewProductionConfig()
                cfg.EncoderConfig.EncodeTime = zapcore.ISO8601TimeEncoder // Hoặc EpochTimeEncoder
                cfg.Level = zap.NewAtomicLevelAt(level)
            } else {
                cfg = zap.NewDevelopmentConfig()
                cfg.EncoderConfig.EncodeLevel = zapcore.CapitalColorLevelEncoder // Màu mè cho dev
                cfg.Level = zap.NewAtomicLevelAt(level)
            }


            tempLogger, err := cfg.Build()
            if err != nil {
                log.Fatalf("can't initialize zap logger: %v", err)
            }
            defer tempLogger.Sync() // Flushes buffer, if any

            logger = tempLogger
            sugar = logger.Sugar()
        }


        func main_zap() {
            // Giả sử cfg.LogLevel lấy từ config (ví dụ: "debug", "info")
            // Giả sử cfg.IsProduction là bool
            InitZapLogger("debug", false) // Khởi tạo logger

            // Sử dụng SugaredLogger (giống Printf)
            sugar.Info("Sugared Logger: Server đã khởi động thành công.")
            sugar.Infof("Sugared Logger: Xử lý request cho user_id: %s, mất %dms", "user-abc", 55)
            sugar.Warnw("Sugared Logger: Phát hiện timeout khi gọi service B",
                "service", "service-B",
                "timeout_ms", 2000,
            )

            // Sử dụng Logger (strongly-typed fields, hiệu năng tốt hơn)
            logger.Info("Structured Logger: Server đã khởi động thành công.",
                zap.String("component", "server-bootstrap"),
                zap.Time("startup_time", time.Now()),
            )
            userID := "user-xyz"
            duration := 75 * time.Millisecond
            logger.Info("Structured Logger: Request processed",
                zap.String("user_id", userID),
                zap.Duration("duration", duration),
                zap.Int("status_code", 200),
                zap.String("path", "/api/items"),
            )

            // Log lỗi với stacktrace (nếu cần)
            // err := someOperation()
            // if err != nil {
            //    logger.Error("Operation failed", zap.Error(err), zap.Stack("stacktrace"))
            // }
        }
        ```

        - **Tư duy:** Trong `zap`, `SugaredLogger` tiện lợi hơn cho các trường hợp log đơn giản. `Logger` với các trường `zap.String`, `zap.Int`, `zap.Time`, etc. cho hiệu năng tốt nhất và cấu trúc rõ ràng nhất.

      - **d. Ví dụ với `rs/zerolog`:**

        ```go
        package main

        import (
            "os"
            "time"
            "github.com/rs/zerolog"
            "github.com/rs/zerolog/log" // global logger của zerolog
        )

        func InitZeroLogger(logLevelStr string, isProduction bool) {
            level, err := zerolog.ParseLevel(logLevelStr)
            if err != nil {
                level = zerolog.InfoLevel
                // Dùng log chuẩn của Go để báo lỗi khởi tạo logger
                // import stdlog "log"
                // stdlog.Printf("Invalid log level '%s', defaulting to 'info'", logLevelStr)
            }
            zerolog.SetGlobalLevel(level)

            if isProduction {
                // Mặc định zerolog ghi ra os.Stderr dạng JSON
                // Không cần làm gì thêm nếu muốn output JSON
                // Có thể custom writer nếu muốn
            } else {
                // Output đẹp hơn cho console ở dev
                log.Logger = log.Output(zerolog.ConsoleWriter{Out: os.Stderr, TimeFormat: time.RFC3339})
            }

            // Thêm các trường cố định vào tất cả log messages
            log.Logger = log.With().
                Str("service", "helios-anomaly-engine"). // Tên service
                Timestamp(). // Thêm trường timestamp tự động
                Logger()
        }

        func main_zerolog() {
            InitZeroLogger("info", false) // Khởi tạo logger

            log.Info().Msg("Server đã khởi động thành công.")
            log.Info().
                Str("user_id", "user-123").
                Int("duration_ms", 55).
                Msg("Request processed successfully") // .Msg() hoặc .Send() để ghi log

            log.Warn().
                Str("component", "payment_gateway").
                Err(errors.New("connection timeout")). // Thêm field lỗi
                Msg("Failed to connect to payment gateway, retrying...")

            // Log với sub-logger có thêm context
            requestID := "req-abc-123"
            reqLogger := log.With().Str("request_id", requestID).Logger()
            reqLogger.Info().Str("path", "/orders").Msg("Received new order request")
            // ...
            reqLogger.Info().Str("status", "completed").Msg("Order processing finished")
        }
        ```

  4.  **Thêm Ngữ cảnh (Context) vào Logs:**

      - Logs sẽ hữu ích hơn nhiều nếu chúng chứa ngữ cảnh về request hoặc tác vụ đang được thực hiện.
      - **Request ID:** Một ID duy nhất cho mỗi request, được truyền qua các services (trong distributed tracing) và log ở mỗi service. Giúp theo dõi một request cụ thể qua toàn bộ hệ thống.
      - **User ID / Tenant ID:** Biết ai hoặc tổ chức nào gây ra sự kiện.
      - **Tên Service / Component:** Biết log này đến từ đâu.
      - **Các tham số quan trọng:** Ví dụ: ID của entity đang được xử lý.
      - **Cách thực hiện:**
        - Các thư viện structured logging cho phép tạo "sub-loggers" hoặc "contextual loggers" có các trường được định sẵn.
        - Trong Go, `context.Context` thường được sử dụng để truyền các giá trị request-scoped như Request ID. Bạn có thể viết một helper để trích xuất các giá trị này từ `context` và thêm vào logger.

      ```go
      // Ví dụ: Logger với context (sử dụng zerolog làm ví dụ)
      import (
          "context"
          "github.com/rs/zerolog"
          "github.com/google/uuid" // Cho request ID
      )

      type ctxKeyLogger int
      const loggerKey ctxKeyLogger = iota

      // Middleware để inject logger vào context
      func LoggerContextMiddleware(next http.Handler) http.Handler {
          return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
              requestID := r.Header.Get("X-Request-ID")
              if requestID == "" {
                  requestID = uuid.NewString()
              }
              // Tạo logger mới cho request này, với request_id
              reqLogger := log.With().Str("request_id", requestID).Logger()
              // Gắn logger vào context của request
              ctx := context.WithValue(r.Context(), loggerKey, &reqLogger)
              next.ServeHTTP(w, r.WithContext(ctx))
          })
      }

      // Helper để lấy logger từ context
      func LoggerFromContext(ctx context.Context) *zerolog.Logger {
          if l, ok := ctx.Value(loggerKey).(*zerolog.Logger); ok {
              return l
          }
          // Trả về global logger nếu không tìm thấy trong context
          return &log.Logger
      }

      // Trong handler:
      func MyHandler(w http.ResponseWriter, r *http.Request) {
          ctx := r.Context()
          logger := LoggerFromContext(ctx) // Lấy logger có request_id

          logger.Info().Msg("Processing request in MyHandler")
          // ...
      }
      ```

  5.  **Tích hợp với Configuration:**
      - Log level, output format (JSON/text), output destination (stdout, file) nên được cấu hình.
      - Sử dụng thư viện config (như Viper) để đọc các cài đặt này và khởi tạo logger tương ứng (như trong `InitZapLogger` và `InitZeroLogger` ở trên).
      - Một số thư viện logging (như Zap) cho phép thay đổi log level lúc runtime thông qua `AtomicLevel`, hữu ích để bật DEBUG log tạm thời ở production mà không cần restart.

- **Best practices:**

  1.  **Sử dụng Structured Logging (JSON hoặc logfmt):** Đặc biệt quan trọng cho các hệ thống production và microservices.
  2.  **Chọn Thư viện Logging Phù hợp:** `zap` hoặc `zerolog` là lựa chọn tốt cho hiệu năng và tính năng.
  3.  **Log ở Cấp độ (Level) Phù hợp:** Đừng log mọi thứ ở INFO. Dùng DEBUG/TRACE cho gỡ lỗi chi tiết.
  4.  **Bao gồm Ngữ cảnh Quan trọng:** Request ID, User ID, Service Name, và các dữ liệu liên quan đến sự kiện.
  5.  **Log Lỗi một cách Hữu ích:** Log message nên rõ ràng. Nếu có `error` object, hãy log nó (thường thư viện sẽ tự động trích xuất `err.Error()`). Cân nhắc log stack trace cho các lỗi không mong muốn.
      - **Uncle Bob (Clean Code):** "Error messages should be precise and informative."
  6.  **Đừng Log Thông tin Nhạy cảm (Secrets):** Mật khẩu, API keys, số thẻ tín dụng không bao giờ nên xuất hiện trong logs, kể cả ở DEBUG level. Cần có cơ chế để sanitize (lọc) chúng.
  7.  **Log ở Đầu và Cuối các Thao tác Quan trọng:** Ví dụ: khi nhận request, khi bắt đầu xử lý, khi kết thúc xử lý (thành công/thất bại).
  8.  **Không Log Quá Nhiều (Be Mindful of Volume):** Log nhiều có thể ảnh hưởng hiệu năng và tốn chi phí lưu trữ/xử lý. Cấu hình log level động là một giải pháp.
  9.  **Định dạng Log nhất quán:** Giữa các service và trong cùng một service.
  10. **Tích hợp với Hệ thống Tập trung Log (Log Aggregation):** ELK Stack (Elasticsearch, Logstash, Kibana), Splunk, Datadog, Grafana Loki.

- **Anti-patterns / lỗi phổ biến:**

  1.  **Logging quá ít hoặc không đủ thông tin:** Khiến việc gỡ lỗi trở thành ác mộng.
  2.  **Logging quá nhiều (Noise):** Làm loãng thông tin quan trọng, tốn tài nguyên.
  3.  **Logging thông tin nhạy cảm.**
  4.  **Log messages không rõ ràng, khó hiểu.**
  5.  **Sử dụng `fmt.Println` cho logging trong production:** Thiếu levels, thiếu cấu trúc, khó quản lý output.
  6.  **Nuốt lỗi mà không log (Swallowing errors):** `if err != nil { /* do nothing */ }`
  7.  **Log cùng một lỗi ở nhiều tầng (Multiple logging of the same error):** Gây nhiễu. Thường thì lỗi nên được log ở tầng cao nhất có đủ ngữ cảnh để xử lý hoặc báo cáo nó.
  8.  **Log messages chứa dữ liệu do người dùng nhập trực tiếp mà không sanitize:** Có thể dẫn đến log injection (mặc dù ít nghiêm trọng hơn SQL injection, nhưng vẫn có thể làm hỏng định dạng log hoặc gây hiểu nhầm).

- **So sánh các lựa chọn / cách tiếp cận (nếu có):**

  - **Standard `log` vs. `zap`/`zerolog`:**
    - `log` chuẩn: Đủ cho các script nhỏ, tool đơn giản, hoặc khi không muốn thêm dependency.
    - `zap`/`zerolog`: Bắt buộc cho các ứng dụng/service production cần hiệu năng, structured logging, và các tính năng nâng cao. `zap` có API phức tạp hơn một chút nhưng rất linh hoạt. `zerolog` có API đơn giản hơn và tập trung vào zero-allocation. Cả hai đều là lựa chọn xuất sắc.
  - **JSON vs. logfmt:**
    - JSON: Được hỗ trợ rộng rãi bởi các tool, dễ parse. Có thể hơi dài dòng.
    - logfmt: `level=info msg="request processed" user_id=123 duration_ms=55`. Ngắn gọn hơn JSON, vẫn dễ parse. Một số tool cũng hỗ trợ tốt. Lựa chọn thường phụ thuộc vào hệ thống log aggregation bạn sử dụng.

- **Gợi ý mở rộng kiến thức:**
  - **The Twelve-Factor App - XI. Logs:** (https://12factor.net/logs) - Coi logs như một stream sự kiện.
  - **`uber-go/zap` documentation:** (https://github.com/uber-go/zap)
  - **`rs/zerolog` documentation:** (https://github.com/rs/zerolog)
  - Tìm hiểu về các hệ thống log aggregation như ELK stack, Grafana Loki.
  - Nghiên cứu về Distributed Tracing (ví dụ: OpenTelemetry, Jaeger, Zipkin) và cách nó bổ sung cho logging trong việc quan sát microservices. (Sẽ là một phần sau).
  - **Trích dẫn:** "The trouble with programmers is that you can never tell what a programmer is doing until it’s too late." – Seymour Cray. (Logs giúp chúng ta biết họ đang làm gì!)

---

Logging là một kỹ năng nền tảng. Một hệ thống logging tốt sẽ là người bạn đồng hành đắc lực của bạn trong suốt vòng đời của ứng dụng.

Tiếp theo, chúng ta sẽ bàn về một chủ đề quan trọng khác liên quan đến dữ liệu: **PHẦN 8: LÀM VIỆC VỚI DATABASE TRONG GO – SQL VÀ ORM/QUERY BUILDERS.**

Chắc chắn rồi, chúng ta sẽ đi sâu vào **PHẦN 8: LÀM VIỆC VỚI DATABASE TRONG GO – SQL, QUERY BUILDERS, VÀ NGUYÊN TẮC KHI TƯƠNG TÁC VỚI DATA STORE.**

- **Tên phần học:** Làm việc với Database trong Go – SQL, Query Builders, và Nguyên tắc khi Tương tác với Data Store
- **Mục tiêu học phần:**

  - Hiểu cách Go tương tác với cơ sở dữ liệu SQL thông qua package `database/sql` chuẩn.
  - Nắm vững các khái niệm cốt lõi: `sql.DB` (connection pool), `sql.Tx` (transactions), `sql.Stmt` (prepared statements), và cách xử lý `sql.Rows` và `sql.Row`.
  - Học cách thực thi các câu lệnh CRUD (Create, Read, Update, Delete) một cách an toàn, tránh SQL Injection.
  - Tìm hiểu về các thư viện Query Builder và "ORM-light" phổ biến trong Go (ví dụ: `sqlc`, `squirrel`, `sqlx`, `gorm` ở mức độ cơ bản) và khi nào nên sử dụng chúng.
  - Hiểu các best practices khi thiết kế schema, viết truy vấn, quản lý kết nối, và xử lý lỗi liên quan đến database.
  - Áp dụng các kiến thức này để xây dựng lớp Repository cho dự án Helios, tương tác với PostgreSQL.

- **Giải thích lý thuyết kỹ càng:**

  1.  **Package `database/sql` chuẩn của Go:**

      - Package `database/sql` cung cấp một giao diện chung (generic interface) để làm việc với các cơ sở dữ liệu SQL (hoặc các hệ thống tương tự SQL).
      - Nó **không phải là một driver cụ thể** cho MySQL, PostgreSQL, SQLite, v.v. Thay vào đó, bạn cần import một package driver cụ thể bên cạnh `database/sql`. Driver này sẽ đăng ký chính nó với `database/sql`.
        ```go
        import (
            "database/sql"
            _ "github.com/lib/pq" // PostgreSQL driver. Dấu "_" nghĩa là import chỉ để thực thi hàm init() của nó (đăng ký driver).
            // _ "github.com/go-sql-driver/mysql" // MySQL driver
        )
        ```
      - **Các Thành phần Cốt lõi:**

        - **`sql.DB`:**

          - Đại diện cho một **connection pool** (bể kết nối) đến cơ sở dữ liệu, không phải một kết nối đơn lẻ. Nó quản lý việc mở và đóng kết nối một cách an toàn và hiệu quả.
          - Nên được tạo một lần khi ứng dụng khởi động và được chia sẻ (an toàn cho việc sử dụng đồng thời bởi nhiều goroutines).
          - Không nên đóng `sql.DB` sau mỗi thao tác, mà chỉ đóng khi ứng dụng shutdown.

          ```go
          // Khởi tạo sql.DB (ví dụ cho PostgreSQL)
          // dbConnString từ config (đã học ở Phần 6)
          db, err := sql.Open("postgres", dbConnString)
          if err != nil {
              log.Fatalf("Failed to open database connection: %v", err)
          }
          // Quan trọng: sql.Open không thực sự tạo kết nối ngay lập tức hay kiểm tra kết nối.
          // Nó chỉ chuẩn bị đối tượng DB.
          // Hãy Ping để kiểm tra kết nối.
          if err := db.Ping(); err != nil {
              db.Close() // Đóng nếu ping lỗi
              log.Fatalf("Failed to ping database: %v", err)
          }

          // Thiết lập các tham số cho connection pool (quan trọng cho performance)
          db.SetMaxOpenConns(25) // Số lượng kết nối mở tối đa
          db.SetMaxIdleConns(25) // Số lượng kết nối nhàn rỗi tối đa
          db.SetConnMaxLifetime(5 * time.Minute) // Thời gian sống tối đa của một kết nối
          db.SetConnMaxIdleTime(1 * time.Minute) // Thời gian nhàn rỗi tối đa của một kết nối trước khi bị đóng

          // defer db.Close() // Sẽ được gọi khi hàm main kết thúc (hoặc khi service shutdown)
          ```

        - **`sql.Tx` (Transactions - Giao dịch):**

          - Đại diện cho một giao dịch cơ sở dữ liệu. Một giao dịch là một chuỗi các thao tác được coi là một đơn vị công việc duy nhất: hoặc tất cả thành công (commit), hoặc tất cả thất bại và được rollback về trạng thái trước đó.
          - Sử dụng `db.Begin()` hoặc `db.BeginTx(ctx, opts)` để bắt đầu một transaction.
          - Phải gọi `tx.Commit()` để xác nhận các thay đổi hoặc `tx.Rollback()` để hủy bỏ. **Luôn `defer tx.Rollback()`** ngay sau khi `db.Begin()` thành công, phòng trường hợp có panic hoặc return sớm mà quên commit/rollback. Nếu `tx.Commit()` được gọi thành công, `tx.Rollback()` sau đó sẽ không làm gì cả (no-op).

          ```go
          tx, err := db.Begin()
          if err != nil {
              // log error
              return err
          }
          defer tx.Rollback() // Quan trọng!

          // ... thực hiện các thao tác trong transaction với tx.ExecContext, tx.QueryContext, tx.PrepareContext ...

          if err = tx.Commit(); err != nil {
              // log error
              return err
          }
          // Giao dịch thành công
          ```

        - **`sql.Stmt` (Prepared Statements - Câu lệnh Chuẩn bị sẵn):**

          - Một câu lệnh SQL đã được "chuẩn bị" (parsed, compiled, optimized) bởi server cơ sở dữ liệu và có thể được thực thi nhiều lần với các tham số khác nhau.
          - **Lợi ích:**
            - **An toàn (Chống SQL Injection):** Tham số được truyền riêng biệt với câu lệnh SQL, server DB sẽ xử lý chúng một cách an toàn.
            - **Hiệu năng:** Server không cần parse lại câu lệnh mỗi lần thực thi, có thể nhanh hơn cho các truy vấn được gọi lặp đi lặp lại.
          - Sử dụng `db.PrepareContext(ctx, query)` hoặc `tx.PrepareContext(ctx, query)`.
          - Phải `stmt.Close()` khi không dùng nữa (thường `defer stmt.Close()`).

          ```go
          stmt, err := db.PrepareContext(ctx, "INSERT INTO users(name, email) VALUES($1, $2) RETURNING id") // $1, $2 là placeholders cho PostgreSQL
          // (MySQL dùng "?")
          if err != nil {
              return err
          }
          defer stmt.Close()

          var userID int64
          err = stmt.QueryRowContext(ctx, "Alice", "alice@example.com").Scan(&userID)
          // ...
          ```

        - **Thực thi Truy vấn:**
          - **`db.ExecContext(ctx, query, args...)` / `tx.ExecContext(...)`:**
            - Dùng cho các câu lệnh không trả về hàng dữ liệu (INSERT, UPDATE, DELETE).
            - Trả về `sql.Result` (chứa thông tin `LastInsertId()` và `RowsAffected()`) và `error`.
          - **`db.QueryContext(ctx, query, args...)` / `tx.QueryContext(...)`:**
            - Dùng cho các câu lệnh SELECT trả về nhiều hàng.
            - Trả về `*sql.Rows` và `error`.
          - **`db.QueryRowContext(ctx, query, args...)` / `tx.QueryRowContext(...)`:**
            - Dùng cho các câu lệnh SELECT được mong đợi trả về tối đa một hàng.
            - Trả về `*sql.Row` (không trả về `error` trực tiếp từ đây, lỗi được trì hoãn cho đến khi gọi `Scan()`).
        - **Xử lý Kết quả:**

          - **`*sql.Rows`:**
            - Cần lặp qua bằng `rows.Next()`.
            - Sau vòng lặp, **luôn kiểm tra `rows.Err()`** để bắt lỗi xảy ra trong quá trình lặp.
            - **Luôn `defer rows.Close()`** để giải phóng kết nối về pool.
            - Đọc dữ liệu từ hàng hiện tại bằng `rows.Scan(&dest1, &dest2, ...)`.

          ```go
          rows, err := db.QueryContext(ctx, "SELECT id, name FROM products WHERE category = $1", "electronics")
          if err != nil {
              return err
          }
          defer rows.Close() // Quan trọng!

          var products []Product
          for rows.Next() {
              var p Product
              if err := rows.Scan(&p.ID, &p.Name); err != nil {
                  // log error, có thể là lỗi scan hoặc lỗi từ DB trong quá trình lặp
                  return err
              }
              products = append(products, p)
          }
          // Kiểm tra lỗi sau khi lặp xong
          if err = rows.Err(); err != nil {
              return err
          }
          // products chứa kết quả
          ```

          - **`*sql.Row`:**
            - Gọi `row.Scan(&dest1, &dest2, ...)` để đọc dữ liệu. `Scan` sẽ trả về `sql.ErrNoRows` nếu truy vấn không trả về hàng nào.

          ```go
          var userName string
          var userEmail string
          err := db.QueryRowContext(ctx, "SELECT name, email FROM users WHERE id = $1", userID).Scan(&userName, &userEmail)
          if err != nil {
              if errors.Is(err, sql.ErrNoRows) {
                  // Xử lý trường hợp không tìm thấy user
                  return fmt.Errorf("user with id %d not found", userID)
              }
              // Lỗi khác
              return fmt.Errorf("querying user: %w", err)
          }
          // userName, userEmail chứa dữ liệu
          ```

        - **Context Propagation:** Luôn truyền `context.Context` vào các phương thức của `database/sql` (ví dụ: `QueryContext`, `ExecContext`, `BeginTx`). Điều này cho phép cancellation (hủy bỏ) hoặc timeout lan truyền xuống driver và DB.

  2.  **Chống SQL Injection:**

      - **SQL Injection là gì?** Một lỗ hổng bảo mật khi kẻ tấn công có thể chèn các đoạn mã SQL độc hại vào input của ứng dụng, làm thay đổi logic của câu lệnh SQL gốc, có thể dẫn đến truy cập trái phép, sửa đổi hoặc xóa dữ liệu.
      - **Cách `database/sql` giúp phòng chống:**
        - **Sử dụng placeholders và truyền tham số riêng biệt:** Đây là cách hiệu quả nhất. Driver sẽ xử lý việc escape các ký tự đặc biệt trong tham số một cách an toàn.
          - _Đúng:_ `db.QueryContext(ctx, "SELECT * FROM users WHERE name = $1", userNameInput)`
          - _SAI (Rất nguy hiểm!):_ `query := fmt.Sprintf("SELECT * FROM users WHERE name = '%s'", userNameInput); db.QueryContext(ctx, query)`
        - **Prepared Statements:** Vốn dĩ đã tách biệt câu lệnh và tham số.
      - **Không bao giờ** tự xây dựng câu lệnh SQL bằng cách nối chuỗi trực tiếp với dữ liệu từ người dùng hoặc nguồn không đáng tin cậy.

  3.  **Query Builders và "ORM-light" trong Go:**

      - **Tại sao cần chúng?**
        - Viết SQL thuần túy có thể dài dòng và dễ lỗi (ví dụ: quên dấu phẩy, lỗi cú pháp).
        - Xây dựng các câu lệnh SQL động (ví dụ: thêm điều kiện `WHERE` tùy chọn) có thể phức tạp và dễ bị SQL injection nếu không cẩn thận.
        - Mapping kết quả từ `sql.Rows` vào struct thủ công có thể lặp đi lặp lại.
      - Go có xu hướng ưa chuộng các giải pháp "mỏng" hơn so với các ORM (Object-Relational Mapper) "nặng" như Hibernate (Java) hay SQLAlchemy (Python). Cộng đồng Go thường thích sự rõ ràng và kiểm soát mà SQL thuần mang lại, nhưng cũng cần các công cụ hỗ trợ.

      - **a. `sqlx` (`jmoiron/sqlx`):**

        - Là một tập hợp các extension cho `database/sql` chuẩn.
        - **Tính năng chính:**
          - `Get()` và `Select()` để tự động scan kết quả vào struct (sử dụng struct tags như `db:"column_name"`).
          - Hỗ trợ `NamedExec()` và `NamedQuery()` cho các truy vấn với tham số đặt tên (ví dụ: `WHERE name = :name`).
          - Giữ nguyên API của `database/sql` và tương thích hoàn toàn.

        ```go
        // import "github.com/jmoiron/sqlx"
        // var db *sqlx.DB // Khởi tạo tương tự sql.DB

        type User struct {
            ID    int64  `db:"id"`
            Name  string `db:"name"`
            Email string `db:"email"`
        }

        // Get một user
        var user User
        err := db.GetContext(ctx, &user, "SELECT id, name, email FROM users WHERE id = $1", 1)
        if err != nil { /* ... */ }

        // Select nhiều users
        var users []User
        err = db.SelectContext(ctx, &users, "SELECT id, name, email FROM users WHERE status = $1", "active")
        if err != nil { /* ... */ }

        // Named execution
        _, err = db.NamedExecContext(ctx, `INSERT INTO users (name, email) VALUES (:name, :email)`,
            map[string]interface{}{"name": "Bob", "email": "bob@example.com"})
        // Hoặc truyền struct User (nếu các trường khớp với tên placeholder)
        // newUser := User{Name: "Carol", Email: "carol@example.com"}
        // _, err = db.NamedExecContext(ctx, `INSERT INTO users (name, email) VALUES (:name, :email)`, newUser)
        ```

        - _Khi nào dùng?_ Khi bạn vẫn muốn viết SQL nhưng cần sự tiện lợi trong việc scan dữ liệu vào struct và xử lý named parameters. Rất phổ biến.

      - **b. `Squirrel` (`Masterminds/squirrel`):**

        - Là một Query Builder thuần túy. Nó giúp bạn xây dựng các câu lệnh SQL một cách có lập trình bằng Go code.
        - Nó **không thực thi** truy vấn, mà chỉ sinh ra chuỗi SQL và các `args` tương ứng, sau đó bạn dùng `database/sql` (hoặc `sqlx`) để thực thi.

        ```go
        // import "github.com/Masterminds/squirrel"

        psql := squirrel.StatementBuilder.PlaceholderFormat(squirrel.Dollar) // Cho PostgreSQL ($1, $2)
                                                                        // (squirrel.Question cho MySQL)
        sql, args, err := psql.Select("id", "name").
            From("users").
            Where(squirrel.Eq{"status": "active"}). // WHERE status = 'active'
            Where(squirrel.Gt{"age": 30}).          // AND age > 30
            OrderBy("created_at DESC").
            Limit(10).
            Offset(5).
            ToSql() // Sinh ra chuỗi SQL và slice các arguments

        if err != nil { /* ... */ }

        // Dùng sqlx để thực thi và scan
        // var users []User
        // err = db.SelectContext(ctx, &users, sql, args...)

        // Ví dụ INSERT
        sql, args, err = psql.Insert("users").
            Columns("name", "email").
            Values("Charlie", "charlie@example.com").
            Values("David", "david@example.com").
            Suffix("RETURNING id"). // Cho PostgreSQL
            ToSql()
        ```

        - _Khi nào dùng?_ Khi bạn cần xây dựng các câu lệnh SQL động một cách an toàn và dễ đọc hơn là nối chuỗi. Hoạt động tốt với `sqlx`.

      - **c. `sqlc` (`kyleconroy/sqlc`):**

        - Một cách tiếp cận khác: bạn viết các câu lệnh SQL thuần túy trong các file `.sql`, sau đó `sqlc` sẽ **sinh ra code Go type-safe** để thực thi các truy vấn đó và scan kết quả vào struct.
        - Nó không phải là query builder lúc runtime, mà là công cụ code generation lúc build-time.
        - File `schema.sql` (DDL) -> File `queries.sql` (DML với tên comment đặc biệt) -> `sqlc generate` -> Code Go (Models, Querier interface, và implementation).

        ```sql
        -- schema.sql
        CREATE TABLE authors (
          id   BIGSERIAL PRIMARY KEY,
          name TEXT NOT NULL,
          bio  TEXT
        );

        -- queries.sql
        -- name: GetAuthor :one
        SELECT * FROM authors
        WHERE id = $1 LIMIT 1;

        -- name: ListAuthors :many
        SELECT * FROM authors
        ORDER BY name;

        -- name: CreateAuthor :one
        INSERT INTO authors (name, bio)
        VALUES ($1, $2)
        RETURNING *;
        ```

        Sau khi chạy `sqlc generate`, bạn sẽ có code Go:

        ```go
        // db/querier.go (sinh ra)
        // type Querier interface {
        //     CreateAuthor(ctx context.Context, arg CreateAuthorParams) (Author, error)
        //     GetAuthor(ctx context.Context, id int64) (Author, error)
        //     ListAuthors(ctx context.Context) ([]Author, error)
        // }
        // type Author struct { /* ... */ }
        // type CreateAuthorParams struct { /* ... */ }

        // Sử dụng trong code của bạn:
        // queries := db.New(sqlDB) // sqlDB là *sql.DB hoặc *sqlx.DB
        // author, err := queries.GetAuthor(ctx, 123)
        ```

        - _Khi nào dùng?_ Nếu bạn thích viết SQL thuần, muốn type-safety, và muốn giảm code boilerplate cho việc thực thi/scan. Rất phù hợp với Clean Architecture (Querier interface là gateway).

      - **d. `GORM` (`go-gorm/gorm`):**

        - Là một ORM đầy đủ tính năng hơn, gần giống với các ORM ở ngôn ngữ khác.
        - Cung cấp API "fluent" để xây dựng truy vấn, tự động migrations, hooks, soft deletes, preloading associations (quan hệ).

        ```go
        // import "gorm.io/gorm"
        // import "gorm.io/driver/postgres"

        // type Product struct {
        //  gorm.Model // Bao gồm ID, CreatedAt, UpdatedAt, DeletedAt
        //  Code  string
        //  Price uint
        // }
        // dsn := "host=localhost user=gorm password=gorm dbname=gorm port=9920 sslmode=disable TimeZone=Asia/Shanghai"
        // db, err := gorm.Open(postgres.Open(dsn), &gorm.Config{})

        // db.AutoMigrate(&Product{}) // Tạo bảng
        // db.Create(&Product{Code: "D42", Price: 100})

        // var product Product
        // db.First(&product, 1) // Tìm product với id 1
        // db.First(&product, "code = ?", "D42") // Tìm product với code D42

        // db.Model(&product).Update("Price", 200)
        ```

        - _Khi nào dùng?_ Khi bạn cần các tính năng ORM mạnh mẽ và muốn giảm thiểu việc viết SQL. Tuy nhiên, nó có thể "che giấu" SQL bên dưới, đôi khi gây khó khăn trong việc tối ưu hoặc hiểu chính xác truy vấn. Cần học kỹ để dùng hiệu quả.
        - **Tư duy cộng đồng Go:** Nhiều Go developers cẩn trọng với các ORM "nặng" vì chúng có thể làm mất đi sự đơn giản và kiểm soát mà Go mang lại. `sqlx`, `squirrel`, `sqlc` thường được ưa chuộng hơn cho các dự án cần hiệu năng và sự rõ ràng của SQL. Tuy nhiên, GORM vẫn là lựa chọn tốt cho các dự án CRUD nhanh hoặc khi team đã quen với ORM.

  4.  **Thiết kế Repository Layer (cho Dự án Helios):**

      - Trong Clean Architecture, Repository là một interface (định nghĩa bởi Use Case) và implementation của nó nằm ở lớp Adapter.
      - Implementation sẽ sử dụng `database/sql` (có thể với `sqlx` hoặc code sinh bởi `sqlc`) để tương tác với DB.

      ```go
      // internal/usecase/interfaces.go (đã có từ Phần 2)
      package usecase

      import (
          "context"
          "project-helios/internal/entity"
      )

      type AnomalyRepository interface {
          Save(ctx context.Context, anomaly *entity.DetectedAnomaly) error
          FindByID(ctx context.Context, id string) (*entity.DetectedAnomaly, error)
          // ... các phương thức khác
      }

      // internal/adapter/repository/postgres/anomaly_postgres_repo.go
      package postgres

      import (
          "context"
          "database/sql" // Hoặc "github.com/jmoiron/sqlx"
          "errors"
          "fmt"
          "project-helios/internal/entity"
          // "github.com/jmoiron/sqlx" // Nếu dùng sqlx
      )

      type PostgresAnomalyRepository struct {
          // db *sqlx.DB // Nếu dùng sqlx
          db *sql.DB
      }

      // NewPostgresAnomalyRepository là constructor, nhận DB connection
      func NewPostgresAnomalyRepository(db *sql.DB /* hoặc *sqlx.DB */) *PostgresAnomalyRepository {
          return &PostgresAnomalyRepository{db: db}
      }

      func (r *PostgresAnomalyRepository) Save(ctx context.Context, anomaly *entity.DetectedAnomaly) error {
          query := `INSERT INTO detected_anomalies (id, sensor_id, rule_id, timestamp, actual_value, details)
                    VALUES ($1, $2, $3, $4, $5, $6)`
          _, err := r.db.ExecContext(ctx, query,
              anomaly.ID, anomaly.SensorID, anomaly.RuleID,
              anomaly.Timestamp, anomaly.ActualValue, anomaly.Details,
          )
          if err != nil {
              // Có thể wrap lỗi ở đây để thêm ngữ cảnh
              return fmt.Errorf("saving anomaly to postgres: %w", err)
          }
          return nil
      }

      func (r *PostgresAnomalyRepository) FindByID(ctx context.Context, id string) (*entity.DetectedAnomaly, error) {
          query := `SELECT id, sensor_id, rule_id, timestamp, actual_value, details
                    FROM detected_anomalies WHERE id = $1`
          var anomaly entity.DetectedAnomaly
          // Nếu dùng sqlx:
          // err := r.db.GetContext(ctx, &anomaly, query, id)
          // Nếu dùng database/sql thuần:
          row := r.db.QueryRowContext(ctx, query, id)
          err := row.Scan(
              &anomaly.ID, &anomaly.SensorID, &anomaly.RuleID,
              &anomaly.Timestamp, &anomaly.ActualValue, &anomaly.Details,
          )

          if err != nil {
              if errors.Is(err, sql.ErrNoRows) {
                  return nil, entity.ErrAnomalyNotFound // Định nghĩa lỗi này trong package entity hoặc usecase
              }
              return nil, fmt.Errorf("finding anomaly by id %s: %w", id, err)
          }
          return &anomaly, nil
      }
      ```

- **Best practices:**

  1.  **Luôn sử dụng Prepared Statements hoặc Query Builders an toàn:** Để chống SQL Injection.
  2.  **Quản lý Connection Pool hiệu quả:** Thiết lập `MaxOpenConns`, `MaxIdleConns`, `ConnMaxLifetime` phù hợp.
  3.  **Sử dụng Transactions cho các thao tác gồm nhiều bước cần tính nhất quán (atomicity).** Luôn `defer tx.Rollback()`.
  4.  **Luôn `defer rows.Close()` và kiểm tra `rows.Err()` sau khi lặp `sql.Rows`.**
  5.  **Truyền `context.Context` vào tất cả các lời gọi DB:** Để hỗ trợ cancellation và timeout.
  6.  **Xử lý lỗi `sql.ErrNoRows` một cách tường minh:** Đây không phải lúc nào cũng là lỗi thực sự, mà có thể là trường hợp "không tìm thấy".
  7.  **Tách biệt code truy cập DB vào Repository Layer:** Tuân thủ Clean Architecture/DIP.
  8.  **Cân nhắc sử dụng `sqlx` để giảm boilerplate khi scan, hoặc `sqlc` để sinh code type-safe nếu bạn thích SQL thuần.**
  9.  **Viết Unit Tests cho Repository Layer:** Sử dụng thư viện như `DATA-DOG/go-sqlmock` để mock DB interactions, hoặc test với DB thật trong container (integration test).
  10. **Tối ưu hóa Truy vấn và Schema:** Sử dụng EXPLAIN, tạo index hợp lý. Đây là một chủ đề lớn riêng.
  11. **Handle retries for transient errors:** Với một số lỗi DB (ví dụ: deadlock, connection timeout tạm thời), có thể cần retry.

- **Anti-patterns / lỗi phổ biến:**

  1.  **SQL Injection do nối chuỗi.**
  2.  **Không đóng `sql.Rows` hoặc `sql.Stmt`:** Gây rò rỉ connection, cạn kiệt pool.
  3.  **Không kiểm tra `rows.Err()` sau vòng lặp `rows.Next()`.**
  4.  **Bỏ qua lỗi `sql.ErrNoRows` hoặc xử lý nó như một lỗi nghiêm trọng ở mọi nơi.**
  5.  **Mở và đóng `sql.DB` cho mỗi truy vấn:** Rất kém hiệu quả, không tận dụng connection pool.
  6.  **Không sử dụng transactions khi cần thiết.**
  7.  **Fetch quá nhiều dữ liệu không cần thiết (`SELECT *` khi chỉ cần vài cột, hoặc không có `LIMIT`).**
  8.  **N+1 Query Problem:** Khi lấy một danh sách các đối tượng, rồi lại thực hiện một truy vấn DB riêng cho mỗi đối tượng trong danh sách đó để lấy dữ liệu liên quan. Cần giải quyết bằng JOIN hoặc preloading.

- **So sánh các lựa chọn / cách tiếp cận (nếu có):**

  - **SQL thuần vs. Query Builder vs. ORM:** Đã thảo luận. Lựa chọn phụ thuộc vào sở thích, yêu cầu dự án, và kinh nghiệm của team. Go có xu hướng nghiêng về các giải pháp "nhẹ" hơn.
    - **Lời khuyên của tôi:** Bắt đầu bằng việc **hiểu rõ `database/sql` và `sqlx`**. Sau đó, khám phá `sqlc` nếu bạn muốn code generation từ SQL. `Squirrel` hữu ích cho các truy vấn động phức tạp. Cân nhắc GORM kỹ lưỡng hơn.

- **Gợi ý mở rộng kiến thức:**
  - **"Effective Go - database/sql"**: (https://go.dev/doc/effective_go#database_sql) - Hướng dẫn từ Golang team.
  - **Alex Edwards - "Let's Go" book / "Let's Go Further" book:** Có các chương rất hay về làm việc với `database/sql` và PostgreSQL.
  - **`sqlx` documentation:** (https://jmoiron.github.io/sqlx/)
  - **`sqlc` documentation:** (https://docs.sqlc.dev/)
  - **`DATA-DOG/go-sqlmock`:** Thư viện để mock `database/sql` cho unit testing.
  - **Bài viết về connection pool tuning.**
  - Nghiên cứu về tối ưu hóa truy vấn SQL và thiết kế database schema (indexing, normalization).

---

Phần 8 này cung cấp nền tảng vững chắc để bạn tương tác với cơ sở dữ liệu một cách an toàn và hiệu quả trong Go. Đây là một kỹ năng cốt lõi cho hầu hết các ứng dụng backend.

Khi bạn sẵn sàng, chúng ta sẽ chuyển sang **PHẦN 9: XÂY DỰNG API VỚI HTTP VÀ GRPC TRONG GO.**

Tuyệt vời! Chúng ta sẽ cùng nhau khám phá **PHẦN 9: XÂY DỰNG API VỚI HTTP VÀ GRPC TRONG GO.**

- **Tên phần học:** Xây dựng API với HTTP và gRPC trong Go
- **Mục tiêu học phần:**

  - Hiểu rõ cách xây dựng API dựa trên HTTP (RESTful hoặc các kiểu khác) trong Go, sử dụng package `net/http` chuẩn và các framework/router phổ biến (ví dụ: Gin, Echo, Chi).
  - Nắm vững các khái niệm HTTP cốt lõi: methods (GET, POST, PUT, DELETE), status codes, headers, request body, response body.
  - Học cách xử lý request, validation input, serialization/deserialization (JSON, XML), và trả về response.
  - Hiểu về gRPC: khái niệm, Protocol Buffers (Protobuf), cách định nghĩa services và messages, và cách sinh code client/server.
  - So sánh ưu nhược điểm giữa HTTP/JSON và gRPC/Protobuf, và khi nào nên chọn loại nào cho microservices.
  - Áp dụng kiến thức để xây dựng các API endpoints cho dự án Helios (ví dụ: `SensorDataIngestor` API nhận dữ liệu qua HTTP, và có thể giao tiếp nội bộ giữa các services bằng gRPC).

- **Giải thích lý thuyết kỹ càng:**

  1.  **Xây dựng API với HTTP trong Go:**

      - **a. Package `net/http` chuẩn:**

        - Go có hỗ trợ mạnh mẽ cho việc xây dựng HTTP server và client ngay trong thư viện chuẩn.
        - **`http.ListenAndServe(addr string, handler http.Handler)`:** Hàm chính để khởi động một HTTP server. `handler` là một interface, thường là một router/mux. Nếu `nil`, `http.DefaultServeMux` sẽ được dùng.
        - **`http.Handler` interface:**
          ```go
          type Handler interface {
              ServeHTTP(ResponseWriter, *Request)
          }
          ```
          Bất kỳ kiểu nào implement phương thức này đều có thể xử lý HTTP requests.
        - **`http.HandleFunc(pattern string, handler func(ResponseWriter, *Request))`:** Đăng ký một hàm để xử lý các request đến một path cụ thể.
        - **`http.ResponseWriter`:** Interface được dùng để xây dựng HTTP response (ghi headers, status code, body).
        - **`http.Request`:** Struct chứa thông tin về HTTP request đến (method, URL, headers, body).

        ```go
        // cmd/http_server_std/main.go
        package main

        import (
            "encoding/json"
            "fmt"
            "log"
            "net/http"
            "time"
        )

        type SimpleMessage struct {
            Text string `json:"text"`
        }

        func helloHandler(w http.ResponseWriter, r *http.Request) {
            if r.Method != http.MethodGet {
                http.Error(w, "Method not allowed", http.StatusMethodNotAllowed)
                return
            }
            fmt.Fprintf(w, "Hello, World from net/http!")
        }

        func echoHandler(w http.ResponseWriter, r *http.Request) {
            if r.Method != http.MethodPost {
                http.Error(w, "Method not allowed", http.StatusMethodNotAllowed)
                return
            }
            var msg SimpleMessage
            // Decode JSON từ request body
            err := json.NewDecoder(r.Body).Decode(&msg)
            if err != nil {
                http.Error(w, err.Error(), http.StatusBadRequest)
                return
            }
            defer r.Body.Close()

            // Encode JSON và gửi lại response
            w.Header().Set("Content-Type", "application/json")
            w.WriteHeader(http.StatusOK) // Có thể không cần nếu chỉ ghi body và mặc định là 200 OK
            json.NewEncoder(w).Encode(msg)
        }

        func main() {
            // Sử dụng DefaultServeMux
            http.HandleFunc("/hello", helloHandler)
            http.HandleFunc("/echo", echoHandler)

            // Custom server để có thể cấu hình thêm
            server := &http.Server{
                Addr:         ":8080",
                // Handler: http.DefaultServeMux, // Nếu không set, DefaultServeMux được dùng
                ReadTimeout:  10 * time.Second,
                WriteTimeout: 10 * time.Second,
                IdleTimeout:  120 * time.Second,
            }

            log.Println("Starting HTTP server on :8080")
            if err := server.ListenAndServe(); err != nil && err != http.ErrServerClosed {
                log.Fatalf("Could not listen on :8080: %v\n", err)
            }
        }
        ```

        - _Ưu điểm `net/http`:_ Không cần dependency bên ngoài, hiểu rõ cách HTTP hoạt động ở mức thấp hơn.
        - _Nhược điểm `net/http`:_ `DefaultServeMux` khá cơ bản (không có routing với parameters, middleware phức tạp). Cần tự làm nhiều thứ.

      - **b. HTTP Routers/Frameworks Phổ biến:**

        - Giải quyết các nhược điểm của `DefaultServeMux`, cung cấp routing mạnh mẽ, middleware, data binding/validation, rendering.
        - **Gin (`gin-gonic/gin`):** Rất phổ biến, hiệu năng cao, API giống Martini nhưng tốt hơn. Nhiều middleware.
        - **Echo (`labstack/echo`):** Cũng rất phổ biến, hiệu năng cao, API tối giản, nhiều middleware tích hợp, template rendering.
        - **Chi (`go-chi/chi`):** Nhẹ, idiomatic, composable. Thiết kế tốt, tập trung vào `net/http` compatibility.
        - **Gorilla Mux (`gorilla/mux`):** Một trong những router lâu đời và đáng tin cậy, mạnh mẽ nhưng có thể hơi dài dòng hơn các framework mới.
        - **Lựa chọn:**
          - **Gin/Echo:** Lựa chọn tốt nếu cần một framework đầy đủ tính năng, hiệu năng cao.
          - **Chi:** Nếu thích sự tối giản, idiomatic, và muốn xây dựng mọi thứ trên nền tảng `net/http` một cách linh hoạt.
          - _Cho dự án Helios:_ Chúng ta có thể chọn Gin hoặc Echo cho các service cần HTTP API (ví dụ: `SensorDataIngestor`, `DashboardAPI`).

        ```go
        // Ví dụ với Gin (cmd/http_server_gin/main.go)
        package main

        import (
            "net/http"
            "project-helios/config" // Giả sử có config
            "project-helios/internal/adapter/handler/http_h" // HTTP Handlers của chúng ta
            "project-helios/internal/usecase"
            // ... (khởi tạo các dependencies khác như repo)
            "github.com/gin-gonic/gin"
            "log"
        )

        func main() {
            // cfg, _ := config.LoadConfig("") // Load config
            // Khởi tạo dependencies (repo, usecase)
            // var anomalyRepo usecase.DetectedAnomalyRepository = ...
            // ingestUseCase := usecase.NewIngestSensorReadingUseCase(anomalyRepo /*, other deps */)
            // queryUseCase := usecase.NewGetAnomaliesUseCase(anomalyRepo)

            // sensorHandler := http_h.NewSensorDataHandler(ingestUseCase)
            // anomalyHandler := http_h.NewAnomalyQueryHandler(queryUseCase)


            // Khởi tạo Gin engine
            // gin.SetMode(gin.ReleaseMode) // Cho production
            router := gin.Default() // Default() đi kèm với Logger và Recovery middleware

            // Middleware (ví dụ)
            router.Use(CORSMiddleware()) // Custom CORS middleware
            // router.Use(authMiddleware) // Middleware xác thực

            // Định nghĩa Routes
            v1 := router.Group("/api/v1")
            {
                // SensorDataIngestor service endpoints
                // sensors := v1.Group("/sensors")
                // {
                //  sensors.POST("/readings", sensorHandler.HandleIngestReading)
                // }

                // AnomalyDetectionEngine/DashboardAPI endpoints
                // anomalies := v1.Group("/anomalies")
                // {
                //  anomalies.GET("", anomalyHandler.HandleGetAnomalies)
                //  anomalies.GET("/:id", anomalyHandler.HandleGetAnomalyByID) // Path parameter
                // }
                v1.GET("/ping", func(c *gin.Context) {
                    c.JSON(http.StatusOK, gin.H{"message": "pong"})
                })
            }

            // serverPort := fmt.Sprintf(":%d", cfg.Server.Port)
            serverPort := ":8080"
            log.Printf("Starting Gin HTTP server on %s", serverPort)
            if err := router.Run(serverPort); err != nil {
                log.Fatalf("Failed to run Gin server: %v", err)
            }
        }

        func CORSMiddleware() gin.HandlerFunc { // Ví dụ CORS middleware
            return func(c *gin.Context) {
                c.Writer.Header().Set("Access-Control-Allow-Origin", "*")
                c.Writer.Header().Set("Access-Control-Allow-Credentials", "true")
                c.Writer.Header().Set("Access-Control-Allow-Headers", "Content-Type, Content-Length, Accept-Encoding, X-CSRF-Token, Authorization, accept, origin, Cache-Control, X-Requested-With")
                c.Writer.Header().Set("Access-Control-Allow-Methods", "POST, OPTIONS, GET, PUT, DELETE")
                if c.Request.Method == "OPTIONS" {
                    c.AbortWithStatus(204)
                    return
                }
                c.Next()
            }
        }
        ```

      - **c. Xử lý Request và Response:**
        - **Path Parameters:** `c.Param("id")` (Gin), `e.Param("id")` (Echo).
        - **Query Parameters:** `c.Query("status")` (Gin), `e.QueryParam("status")` (Echo).
        - **Request Body:**
          - **Binding/Unmarshalling:** Các framework thường có helper để bind JSON/XML/Form data vào struct Go.
            `if err := c.ShouldBindJSON(&inputStruct); err != nil { /* handle error */ }` (Gin)
          - **Validation:** Sử dụng các thư viện validation (ví dụ: `go-playground/validator`) tích hợp với framework hoặc tự validate.
        - **Response:**
          - Trả về JSON: `c.JSON(http.StatusOK, data)` (Gin), `e.JSON(http.StatusOK, data)` (Echo).
          - Trả về status code và message: `c.String(http.StatusInternalServerError, "error message")`.
          - Custom headers: `c.Header("X-My-Header", "value")`.
        - **Middleware:** Các hàm được thực thi trước hoặc sau handler chính. Dùng cho logging, auth, CORS, rate limiting, recovery từ panic.

  2.  **Xây dựng API với gRPC trong Go:**

      - **a. gRPC là gì?**

        - gRPC (gRPC Remote Procedure Calls) là một framework RPC (Remote Procedure Call) mã nguồn mở, hiệu năng cao, được phát triển bởi Google.
        - Nó cho phép một ứng dụng client gọi trực tiếp các phương thức trên một ứng dụng server ở một máy khác như thể đó là một đối tượng cục bộ.
        - **Đặc điểm chính:**
          - **Contract-first API development (IDL):** Sử dụng **Protocol Buffers (Protobuf)** làm Ngôn ngữ Định nghĩa Giao diện (Interface Definition Language - IDL) để mô tả cấu trúc dữ liệu (messages) và các phương thức dịch vụ (services).
          - **Hiệu năng cao:** Sử dụng HTTP/2 làm tầng vận chuyển, cho phép multiplexing (nhiều request/response trên một kết nối TCP), server push, header compression. Protobuf serialization cũng rất hiệu quả (nhỏ gọn và nhanh).
          - **Đa ngôn ngữ:** Hỗ trợ nhiều ngôn ngữ lập trình, code client và server có thể được sinh ra tự động từ file `.proto`.
          - **Streaming:** Hỗ trợ streaming hai chiều (bidirectional streaming), client streaming, server streaming.
          - **Strongly-typed:** Protobuf đảm bảo kiểu dữ liệu mạnh mẽ.

      - **b. Protocol Buffers (Protobuf):**

        - Là một cơ chế serialization dữ liệu có cấu trúc, trung lập về ngôn ngữ và nền tảng, có thể mở rộng (giống XML nhưng nhỏ hơn, nhanh hơn, đơn giản hơn).
        - Bạn định nghĩa cấu trúc dữ liệu một lần trong file `.proto`, sau đó dùng `protoc` compiler (với plugin cho Go) để sinh ra code Go (structs, methods để serialize/deserialize).

        ```protobuf
        // protos/anomaly_service.proto
        syntax = "proto3"; // Phiên bản Protobuf

        package helios.anomaly.v1; // Namespace cho Protobuf messages

        option go_package = "project-helios/gen/go/helios/anomaly/v1;anomaly_v1"; // Package Go sẽ được sinh ra

        // Định nghĩa Service
        service AnomalyService {
          rpc ReportAnomaly (ReportAnomalyRequest) returns (ReportAnomalyResponse);
          rpc GetAnomalyDetails (GetAnomalyDetailsRequest) returns (GetAnomalyDetailsResponse);
          // Có thể có streaming RPCs
          // rpc SubscribeToAnomalies (SubscriptionRequest) returns (stream AnomalyEvent);
        }

        // Định nghĩa Messages (cấu trúc dữ liệu)
        message Anomaly {
          string id = 1;
          string sensor_id = 2;
          string rule_id = 3;
          int64 timestamp_unix_ms = 4; // Dùng kiểu số cho timestamp
          double actual_value = 5;
          string details = 6;
        }

        message ReportAnomalyRequest {
          Anomaly anomaly_data = 1;
        }

        message ReportAnomalyResponse {
          string anomaly_id_received = 1;
          bool success = 2;
        }

        message GetAnomalyDetailsRequest {
          string anomaly_id = 1;
        }

        message GetAnomalyDetailsResponse {
          Anomaly anomaly_details = 1;
        }
        ```

      - **c. Sinh Code Go từ `.proto`:**

        - Cần cài đặt:
          - `protoc` (Protobuf compiler).
          - `protoc-gen-go` (plugin cho Go).
          - `protoc-gen-go-grpc` (plugin cho gRPC Go).
        - Lệnh (ví dụ):
          ```bash
          protoc --proto_path=protos \
                 --go_out=gen/go --go_opt=paths=source_relative \
                 --go-grpc_out=gen/go --go-grpc_opt=paths=source_relative \
                 protos/anomaly_service.proto
          ```
          Điều này sẽ tạo ra các file `.pb.go` (chứa struct và code serialization) và `_grpc.pb.go` (chứa client stub và server interface/registration) trong thư mục `gen/go`.

      - **d. Triển khai Server gRPC:**

        ```go
        // cmd/grpc_server/main.go (cho AnomalyService)
        package main

        import (
            "context"
            "fmt"
            "log"
            "net"
            pb "project-helios/gen/go/helios/anomaly/v1" // Import code đã sinh
            // ... (usecase, config, etc.)
            "google.golang.org/grpc"
            "google.golang.org/grpc/reflection" // Cho phép gRPC CLI/GUI khám phá service
        )

        // anomalyServer implement interface pb.AnomalyServiceServer (sinh ra từ .proto)
        type anomalyServer struct {
            pb.UnimplementedAnomalyServiceServer // Bắt buộc để tương thích về sau
            // Thêm các dependencies, ví dụ use cases
            // reportAnomalyUseCase usecase.ReportAnomalyUseCase
        }

        func (s *anomalyServer) ReportAnomaly(ctx context.Context, req *pb.ReportAnomalyRequest) (*pb.ReportAnomalyResponse, error) {
            log.Printf("Received ReportAnomaly request: %v", req.GetAnomalyData())
            // Gọi Use Case tương ứng để xử lý
            // anomalyEntity := convertProtoToEntity(req.GetAnomalyData())
            // err := s.reportAnomalyUseCase.Execute(ctx, anomalyEntity)
            // if err != nil {
            //    return nil, status.Errorf(codes.Internal, "failed to process anomaly: %v", err)
            // }

            return &pb.ReportAnomalyResponse{
                AnomalyIdReceived: req.GetAnomalyData().GetId(),
                Success:           true,
            }, nil
        }

        func (s *anomalyServer) GetAnomalyDetails(ctx context.Context, req *pb.GetAnomalyDetailsRequest) (*pb.GetAnomalyDetailsResponse, error) {
            log.Printf("Received GetAnomalyDetails request for ID: %s", req.GetAnomalyId())
            // Gọi Use Case để lấy chi tiết
            // anomalyEntity, err := s.getAnomalyUseCase.Execute(ctx, req.GetAnomalyId())
            // if err != nil { /* ... */ }
            // protoAnomaly := convertEntityToProto(anomalyEntity)
            return &pb.GetAnomalyDetailsResponse{
                // AnomalyDetails: protoAnomaly,
            }, nil
        }


        func main() {
            port := ":50051"
            lis, err := net.Listen("tcp", port)
            if err != nil {
                log.Fatalf("Failed to listen: %v", err)
            }

            s := grpc.NewServer(
                // grpc.UnaryInterceptor(loggingInterceptor), // Thêm interceptors nếu cần
            )

            // Khởi tạo server implementation
            anomalySrv := &anomalyServer{
                // reportAnomalyUseCase: ... ,
            }
            pb.RegisterAnomalyServiceServer(s, anomalySrv) // Đăng ký service

            // Cho phép reflection để các tool như grpcurl có thể khám phá service
            reflection.Register(s)

            log.Printf("gRPC server listening on %s", port)
            if err := s.Serve(lis); err != nil {
                log.Fatalf("Failed to serve gRPC: %v", err)
            }
        }
        ```

      - **e. Tạo Client gRPC:**

        ```go
        // cmd/grpc_client/main.go
        package main

        import (
            "context"
            "log"
            "time"

            pb "project-helios/gen/go/helios/anomaly/v1"
            "google.golang.org/grpc"
            "google.golang.org/grpc/credentials/insecure" // Cho ví dụ, không dùng cho production
        )

        func main() {
            serverAddr := "localhost:50051"
            // Thiết lập kết nối (không có credentials cho ví dụ này)
            conn, err := grpc.Dial(serverAddr, grpc.WithTransportCredentials(insecure.NewCredentials()))
            if err != nil {
                log.Fatalf("Did not connect: %v", err)
            }
            defer conn.Close()

            client := pb.NewAnomalyServiceClient(conn) // Tạo client stub

            ctx, cancel := context.WithTimeout(context.Background(), time.Second*5)
            defer cancel()

            // Gọi RPC
            anomalyData := &pb.Anomaly{
                Id:               "anomaly-001",
                SensorId:         "sensor-temp-A",
                RuleId:           "rule-high-temp",
                TimestampUnixMs: time.Now().UnixMilli(),
                ActualValue:      35.5,
                Details:          "Temperature exceeds threshold.",
            }
            resp, err := client.ReportAnomaly(ctx, &pb.ReportAnomalyRequest{AnomalyData: anomalyData})
            if err != nil {
                log.Fatalf("Could not report anomaly: %v", err)
            }
            log.Printf("ReportAnomaly Response: ID_Received=%s, Success=%t", resp.GetAnomalyIdReceived(), resp.GetSuccess())
        }
        ```

  3.  **So sánh HTTP/JSON và gRPC/Protobuf:**

      | Đặc điểm                | HTTP/JSON (Thường là REST)                                  | gRPC/Protobuf                                                         |
      | :---------------------- | :---------------------------------------------------------- | :-------------------------------------------------------------------- |
      | **IDL**                 | Không có chuẩn IDL chính thức (OpenAPI/Swagger là phổ biến) | Protocol Buffers (.proto files)                                       |
      | **Tầng Vận chuyển**     | HTTP/1.1 (phổ biến), HTTP/2                                 | HTTP/2 (bắt buộc)                                                     |
      | **Serialization**       | JSON (text-based, dễ đọc bởi người)                         | Protocol Buffers (binary, nhỏ gọn, nhanh)                             |
      | **Hiệu năng**           | Thấp hơn (JSON parsing, HTTP/1.1 overhead)                  | Cao hơn (Protobuf, HTTP/2 multiplexing, compression)                  |
      | **Code Generation**     | Có thể có (từ OpenAPI) nhưng không phải là cốt lõi          | Cốt lõi (client/server stubs từ .proto)                               |
      | **Streaming**           | Hạn chế (long polling, WebSockets, HTTP/2 streaming)        | Hỗ trợ mạnh mẽ (server, client, bidirectional)                        |
      | **Trình duyệt Hỗ trợ**  | Trực tiếp                                                   | Cần gRPC-Web proxy (hoặc dùng HTTP/JSON gateway)                      |
      | **Firewall/Proxy**      | Dễ dàng đi qua (port 80/443)                                | Có thể cần cấu hình đặc biệt nếu không dùng HTTP/2 chuẩn              |
      | **Độ phức tạp ban đầu** | Thấp hơn                                                    | Cao hơn một chút (cần toolchain Protobuf)                             |
      | **Tính dễ đọc (Debug)** | JSON dễ đọc                                                 | Binary, cần tool để xem                                               |
      | **Use Cases**           | Public APIs, browser clients, simple services               | Internal microservice communication, high-performance APIs, streaming |

      - **Khi nào chọn HTTP/JSON?**
        - API công khai (public-facing APIs) mà client là trình duyệt hoặc các bên thứ ba.
        - Cần sự đơn giản và dễ debug bằng các công cụ HTTP thông thường.
        - Khi hiệu năng không phải là yếu tố quan trọng nhất.
      - **Khi nào chọn gRPC?**
        - Giao tiếp **nội bộ giữa các microservices** trong cùng một hệ thống (service-to-service communication).
        - Yêu cầu hiệu năng cao, độ trễ thấp.
        - Cần các tính năng streaming mạnh mẽ.
        - Khi làm việc trong một môi trường đa ngôn ngữ và muốn có contract API rõ ràng, code generation.
      - **Hybrid Approach:** Hoàn toàn có thể sử dụng cả hai. Ví dụ:
        - `SensorDataIngestor` có thể expose một HTTP/JSON API để các cảm biến (hoặc gateway của cảm biến) gửi dữ liệu.
        - `SensorDataIngestor` sau đó có thể gọi `AnomalyDetectionEngine` qua gRPC để xử lý.
        - `DashboardAPI` có thể expose HTTP/JSON API cho frontend, và gọi các service nội bộ khác qua gRPC.
        - **gRPC Gateway (`grpc-ecosystem/grpc-gateway`):** Một công cụ cho phép bạn tự động sinh ra một RESTful HTTP proxy từ định nghĩa gRPC service của bạn. Điều này cho phép bạn có một service gRPC và expose nó ra bên ngoài như một API HTTP/JSON mà không cần viết code HTTP handler riêng.

- **Code minh họa / sơ đồ:**

  - Các ví dụ code đã được lồng vào phần giải thích.
  - **Sơ đồ luồng gRPC:**
    ```
    +-------------+      .proto      +-------------+
    | gRPC Client | <-------------- | gRPC Server |
    +-------------+   (Service Def) +-------------+
          |                                |
    (Generated Stub)                (Generated Service Impl Base)
          |                                |
    (Calls Method)                  (Implements Method)
          |                                |
    (Protobuf Msg) --- HTTP/2 ---> (Protobuf Msg)
          |                                |
    (Network)                       (Network)
    ```

- **Best practices:**

  - **HTTP API:**
    - Thiết kế theo RESTful principles nếu phù hợp (sử dụng đúng HTTP methods, status codes, tài nguyên rõ ràng).
    - Versioning API (ví dụ: `/api/v1/...`, `/api/v2/...`).
    - Sử dụng middleware cho các tác vụ chung (auth, logging, CORS, recovery).
    - Validate input một cách cẩn thận.
    - Trả về lỗi với cấu trúc rõ ràng (ví dụ: JSON error object).
  - **gRPC API:**
    - Thiết kế file `.proto` cẩn thận, nghĩ về khả năng tương thích ngược (backward/forward compatibility) khi thay đổi message. (Không thay đổi số thứ tự field, chỉ thêm field mới optional).
    - Sử dụng các kiểu dữ liệu chuẩn của Protobuf (ví dụ: `google.protobuf.Timestamp`, `google.protobuf.Empty`).
    - Sử dụng `status` package của gRPC để trả về lỗi với mã lỗi gRPC chuẩn và thông điệp.
    - Sử dụng Interceptors (Unary và Stream) cho các tác vụ AOP (Aspect-Oriented Programming) như logging, metrics, auth, retry.
    - Cân nhắc Deadlines và Cancellation.
  - **Chung:**
    - Sử dụng HTTPS/TLS cho HTTP và gRPC trong production để đảm bảo an toàn.
    - Quản lý lỗi và trả về mã lỗi phù hợp.

- **Anti-patterns / lỗi phổ biến:**

  - **HTTP API:**
    - Sử dụng GET cho các thao tác làm thay đổi dữ liệu.
    - Trả về status 200 OK cho cả lỗi.
    - API không có versioning.
    - Thiếu validation input.
  - **gRPC API:**
    - Thay đổi `.proto` một cách không tương thích làm hỏng client cũ.
    - Không xử lý lỗi gRPC đúng cách ở client.
    - Blocking calls quá lâu mà không có timeout/deadline.
  - **Chung:**
    - Không bảo mật API (thiếu authentication/authorization).
    - API trả về quá nhiều dữ liệu không cần thiết.

- **So sánh các lựa chọn / cách tiếp cận (nếu có):**

  - HTTP/1.1 vs HTTP/2: gRPC yêu cầu HTTP/2. Hầu hết các HTTP framework hiện đại của Go đều hỗ trợ HTTP/2 cho server.
  - REST vs GraphQL vs gRPC: Đây là ba phong cách API phổ biến.
    - GraphQL: Cho phép client yêu cầu chính xác dữ liệu nó cần. Phù hợp cho frontend phức tạp, giảm over-fetching/under-fetching.
    - REST: Dễ hiểu, phổ biến, stateless.
    - gRPC: Hiệu năng cao, contract-first, cho internal communication.

- **Gợi ý mở rộng kiến thức:**
  - **gRPC Documentation:** (https://grpc.io/docs/)
  - **Protocol Buffers Documentation:** (https://developers.google.com/protocol-buffers)
  - **OpenAPI Specification:** (https://swagger.io/specification/) - Cho thiết kế HTTP/JSON API.
  - **`grpc-gateway`:** (https://github.com/grpc-ecosystem/grpc-gateway)
  - Các bài viết so sánh REST, GraphQL, gRPC.
  - Nghiên cứu về API security (OAuth2, JWT, API Keys).
  - **Trích dẫn (Phil Karlton):** "There are only two hard things in Computer Science: cache invalidation and naming things." (Và thiết kế API tốt cũng là một trong số đó!)

---

Phần 9 này đã mở ra cánh cửa để bạn xây dựng các "cổng giao tiếp" cho microservices của mình, cả với thế giới bên ngoài và giữa các service với nhau. Việc lựa chọn và triển khai API đúng cách là yếu tố then chốt cho sự thành công của hệ thống.

Tiếp theo, chúng ta sẽ tập trung vào một khía cạnh quan trọng để tăng tốc độ và giảm tải cho hệ thống: **PHẦN 10: CACHING VỚI REDIS TRONG GO – TĂNG TỐC VÀ GIẢM TẢI.**

Tuyệt vời! Chúng ta sẽ đi sâu vào **PHẦN 10: CACHING VỚI REDIS TRONG GO – TĂNG TỐC VÀ GIẢM TẢI.**

- **Tên phần học:** Caching với Redis trong Go – Tăng tốc và Giảm tải
- **Mục tiêu học phần:**

  - Hiểu rõ khái niệm caching, các chiến lược caching phổ biến (Cache-Aside, Read-Through, Write-Through, Write-Back, Write-Around), và TẠI SAO caching lại quan trọng cho hiệu năng và khả năng mở rộng.
  - Nắm vững Redis: là gì, các kiểu dữ liệu chính (Strings, Hashes, Lists, Sets, Sorted Sets, Streams), và các trường hợp sử dụng phổ biến của Redis (caching, session store, message broker, rate limiter, leaderboard).
  - Học cách tương tác với Redis từ ứng dụng Go sử dụng các thư viện client phổ biến (ví dụ: `go-redis/redis`).
  - Biết cách serialize/deserialize dữ liệu Go (structs) để lưu trữ và truy xuất từ Redis.
  - Hiểu các vấn đề cần cân nhắc khi caching: cache eviction policies (chính sách loại bỏ cache), cache invalidation (làm mất hiệu lực cache), cache stampede (thundering herd), và cache penetration (xuyên thủng cache).
  - Áp dụng caching với Redis vào dự án Helios để tăng tốc độ truy vấn dữ liệu thường xuyên truy cập (ví dụ: cache kết quả phân tích bất thường, cache thông tin cảm biến).

- **Giải thích lý thuyết kỹ càng:**

  1.  **Caching là gì và Tại sao nó Quan trọng?**

      - **Định nghĩa:** Caching là một kỹ thuật lưu trữ tạm thời các bản sao của dữ liệu hoặc kết quả tính toán thường xuyên được truy cập ở một nơi có tốc độ truy cập nhanh hơn (bộ nhớ cache) so với nguồn gốc của nó (ví dụ: database, external API).
      - **Mục tiêu chính:**
        - **Giảm Độ trễ (Reduce Latency):** Truy cập dữ liệu từ cache (thường là in-memory) nhanh hơn nhiều so với việc truy cập từ disk-based database hoặc gọi qua network.
        - **Giảm Tải cho Hệ thống Nguồn (Reduce Load on Backend Systems):** Bằng cách phục vụ các request từ cache, số lượng request đến database hoặc các service khác giảm đi, giúp chúng hoạt động ổn định hơn và giảm chi phí.
        - **Tăng Thông lượng (Increase Throughput):** Hệ thống có thể xử lý nhiều request hơn trong một đơn vị thời gian.
        - **Cải thiện Khả năng Mở rộng (Improve Scalability):** Giúp hệ thống chịu được tải cao hơn.
      - **Khi nào nên dùng Caching?**
        - Dữ liệu được đọc thường xuyên nhưng ít khi thay đổi.
        - Kết quả của các phép tính toán tốn kém.
        - Dữ liệu từ các nguồn có độ trễ cao.

  2.  **Redis là gì?**

      - **REmote DIctionary Server (Redis)** là một kho lưu trữ cấu trúc dữ liệu trong bộ nhớ (in-memory data structure store) mã nguồn mở, hiệu năng cao. Nó có thể được sử dụng như một database, cache, và message broker.
      - **Đặc điểm chính:**
        - **In-memory:** Dữ liệu chủ yếu được lưu trong RAM, giúp tốc độ truy cập cực nhanh.
        - **Persistence (Bền bỉ - tùy chọn):** Redis có thể lưu dữ liệu xuống disk (RDB snapshots, AOF logs) để tránh mất dữ liệu khi restart.
        - **Nhiều kiểu dữ liệu phong phú:** Không chỉ là key-value đơn giản.
        - **Atomic operations:** Hỗ trợ các thao tác nguyên tử trên các kiểu dữ liệu.
        - **Replication & Clustering:** Hỗ trợ sao chép (master-slave replication) và phân cụm (Redis Cluster) để tăng tính sẵn sàng và khả năng mở rộng.
        - **Lua scripting:** Cho phép thực thi logic phức tạp trên server.
        - **Pub/Sub:** Hỗ trợ mô hình publish/subscribe cho messaging.
        - **Streams:** Kiểu dữ liệu giống như append-only log, hữu ích cho event sourcing, message queues.
      - **Các Kiểu dữ liệu Chính của Redis:**
        - **Strings:** Kiểu cơ bản nhất. Có thể lưu trữ text, số, hoặc dữ liệu nhị phân (lên đến 512MB). Thường dùng cho caching các đối tượng đã được serialize.
          - Lệnh: `SET key value [EX seconds] [NX|XX]`, `GET key`, `INCR key`, `DECR key`
        - **Hashes:** Lưu trữ các map (object) với các cặp field-value. Hữu ích để lưu trữ các thuộc tính của một đối tượng mà không cần serialize/deserialize toàn bộ.
          - Lệnh: `HSET key field value`, `HGET key field`, `HMGET key field1 field2`, `HGETALL key`
        - **Lists:** Danh sách các chuỗi được sắp xếp theo thứ tự chèn. Có thể dùng như queue hoặc stack.
          - Lệnh: `LPUSH key value`, `RPUSH key value`, `LPOP key`, `RPOP key`, `LRANGE key start stop`
        - **Sets:** Tập hợp các chuỗi không có thứ tự và không trùng lặp.
          - Lệnh: `SADD key member`, `SREM key member`, `SMEMBERS key`, `SISMEMBER key member`, `SUNION key1 key2`
        - **Sorted Sets (ZSETs):** Giống Sets nhưng mỗi member được liên kết với một `score` (điểm số). Các member được sắp xếp theo score. Rất hữu ích cho leaderboards, priority queues.
          - Lệnh: `ZADD key score member`, `ZREM key member`, `ZRANGE key start stop [WITHSCORES]`, `ZRANK key member`
        - **Streams (từ Redis 5.0):** Kiểu dữ liệu append-only log. Mỗi entry có ID duy nhất và một tập hợp các field-value. Hỗ trợ consumer groups.
          - Lệnh: `XADD key * field1 value1 ...`, `XRANGE key start end`, `XREAD [COUNT count] [BLOCK milliseconds] STREAMS key1 key2 ... id1 id2 ...`
      - **Trường hợp sử dụng phổ biến của Redis:**
        - **Caching:** Phổ biến nhất.
        - **Session Store:** Lưu trữ session của người dùng cho web applications.
        - **Message Broker (Pub/Sub, Lists, Streams):** Cho giao tiếp bất đồng bộ.
        - **Rate Limiting:** Theo dõi số lượng request từ một IP hoặc user.
        - **Leaderboards / Real-time rankings:** Sử dụng Sorted Sets.
        - **Distributed Locks:** Đảm bảo chỉ một process thực thi một critical section.
        - **Counting / Analytics:** Đếm số lượt xem, số unique visitors.

  3.  **Tương tác với Redis từ Go: `go-redis/redis`**

      - `go-redis/redis` là một thư viện client Redis phổ biến và mạnh mẽ cho Go.
      - Hỗ trợ Redis Sentinel và Redis Cluster.
      - Cung cấp API fluent, dễ sử dụng.

      ```go
      // internal/infrastructure/cache/redis.go
      package cache

      import (
          "context"
          "fmt"
          "time"

          "project-helios/config" // Giả sử có RedisConfig
          "github.com/go-redis/redis/v8" // Hoặc v9 tùy phiên bản
      )

      // RedisClient là một wrapper quanh *redis.Client hoặc *redis.ClusterClient
      type RedisClient struct {
          Client *redis.Client
          // Hoặc ClusterClient *redis.ClusterClient
          // Hoặc Ring *redis.Ring
      }

      // NewRedisClient khởi tạo kết nối đến Redis
      func NewRedisClient(cfg config.RedisConfig) (*RedisClient, error) {
          // Đối với single instance Redis
          client := redis.NewClient(&redis.Options{
              Addr:     cfg.Address,  // ví dụ: "localhost:6379"
              Password: cfg.Password, // "" nếu không có password
              DB:       cfg.DB,       // 0 là default DB
              // PoolSize: 10, // Số lượng socket connections tối đa
              // MinIdleConns: 5, // Số lượng idle connections tối thiểu
              // ReadTimeout: 3 * time.Second,
              // WriteTimeout: 3 * time.Second,
          })

          // Ping để kiểm tra kết nối
          ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
          defer cancel()

          if _, err := client.Ping(ctx).Result(); err != nil {
              client.Close()
              return nil, fmt.Errorf("failed to ping redis: %w", err)
          }

          return &RedisClient{Client: client}, nil
      }

      // Close đóng kết nối Redis
      func (rc *RedisClient) Close() error {
          return rc.Client.Close()
      }

      // ----- Các phương thức thao tác với Redis -----

      // SetString lưu một chuỗi vào Redis với thời gian hết hạn (TTL)
      func (rc *RedisClient) SetString(ctx context.Context, key string, value string, ttl time.Duration) error {
          return rc.Client.Set(ctx, key, value, ttl).Err()
      }

      // GetString lấy một chuỗi từ Redis
      func (rc *RedisClient) GetString(ctx context.Context, key string) (string, error) {
          val, err := rc.Client.Get(ctx, key).Result()
          if err == redis.Nil { // Key không tồn tại
              return "", nil // Hoặc trả về một lỗi tùy chỉnh entity.ErrCacheMiss
          } else if err != nil {
              return "", fmt.Errorf("getting string from redis for key %s: %w", key, err)
          }
          return val, nil
      }

      // Delete xóa một key khỏi Redis
      func (rc *RedisClient) Delete(ctx context.Context, key string) error {
          return rc.Client.Del(ctx, key).Err()
      }

      // SetJSON lưu một đối tượng Go (được serialize thành JSON) vào Redis
      func (rc *RedisClient) SetJSON(ctx context.Context, key string, value interface{}, ttl time.Duration) error {
          jsonData, err := json.Marshal(value)
          if err != nil {
              return fmt.Errorf("marshalling value to JSON for key %s: %w", key, err)
          }
          return rc.Client.Set(ctx, key, jsonData, ttl).Err()
      }

      // GetJSON lấy một đối tượng JSON từ Redis và unmarshal vào một struct Go
      func (rc *RedisClient) GetJSON(ctx context.Context, key string, dest interface{}) error {
          jsonData, err := rc.Client.Get(ctx, key).Result()
          if err == redis.Nil {
              return redis.Nil // Hoặc entity.ErrCacheMiss
          } else if err != nil {
              return fmt.Errorf("getting JSON from redis for key %s: %w", key, err)
          }

          if err := json.Unmarshal([]byte(jsonData), dest); err != nil {
              return fmt.Errorf("unmarshalling JSON for key %s: %w", key, err)
          }
          return nil
      }
      ```

      - **Context:** Luôn truyền `context.Context` vào các lệnh của `go-redis`.

  4.  **Chiến lược Caching (Caching Strategies):**

      - **a. Cache-Aside (Lazy Loading):**

        - Đây là chiến lược phổ biến nhất.
        - **Luồng hoạt động:**
          1.  Ứng dụng cần dữ liệu, đầu tiên kiểm tra cache.
          2.  **Cache Hit:** Nếu dữ liệu có trong cache và còn hợp lệ, trả về dữ liệu từ cache.
          3.  **Cache Miss:** Nếu dữ liệu không có trong cache hoặc đã hết hạn:
              a. Ứng dụng đọc dữ liệu từ nguồn gốc (database).
              b. Ứng dụng lưu dữ liệu đó vào cache.
              c. Ứng dụng trả về dữ liệu cho client.
        - _Sơ đồ:_
          ```
          Application  --1. Request data--> Cache
              ^  |
          5. Return data |  +--2. Cache Hit --> Return data to App
              |  |
              |  +--2. Cache Miss
              |      |
              |      V
              +--3a. Request data --> Database
                  |      ^
          3b. Store in Cache |      | 4. Return data
                  +------+
          ```
        - _Ưu điểm:_
          - Khá đơn giản để triển khai.
          - Chỉ cache dữ liệu thực sự được yêu cầu.
          - Khả năng phục hồi tốt nếu cache bị lỗi (ứng dụng vẫn có thể đọc từ DB).
        - _Nhược điểm:_
          - Độ trễ cao hơn cho request đầu tiên (cache miss).
          - Vấn đề về tính nhất quán dữ liệu (data consistency) nếu dữ liệu trong DB thay đổi mà cache chưa được cập nhật/invalidate.

      - **b. Read-Through:**

        - Ứng dụng luôn đọc dữ liệu thông qua cache provider. Cache provider chịu trách nhiệm load dữ liệu từ DB nếu cache miss.
        - Cache hoạt động như một proxy đến DB.
        - _Ưu điểm:_ Logic load dữ liệu được tập trung ở cache provider.
        - _Nhược điểm:_ Ít phổ biến hơn Cache-Aside vì cần cache provider hỗ trợ tính năng này (Redis không có sẵn, cần tự implement).

      - **c. Write-Through:**

        - Khi ứng dụng ghi dữ liệu:
          1.  Ghi vào cache.
          2.  Ghi vào database.
              _Thao tác ghi chỉ được coi là thành công khi cả hai đều thành công._
        - _Ưu điểm:_
          - Dữ liệu trong cache và DB thường nhất quán hơn (sau khi ghi).
          - Đọc luôn từ cache (nếu có) sẽ nhanh.
        - _Nhược điểm:_
          - Độ trễ ghi cao hơn vì phải ghi vào cả hai nơi.
          - Cache có thể chứa nhiều dữ liệu ít được đọc.

      - **d. Write-Back (Write-Behind):**

        - Khi ứng dụng ghi dữ liệu:
          1.  Chỉ ghi vào cache.
          2.  Sau một khoảng thời gian hoặc khi có một số lượng nhất định các bản ghi, cache sẽ ghi (batch) dữ liệu xuống database một cách bất đồng bộ.
        - _Ưu điểm:_
          - Độ trễ ghi rất thấp (chỉ ghi vào cache in-memory).
          - Tăng thông lượng ghi.
        - _Nhược điểm:_
          - **Nguy cơ mất dữ liệu** nếu cache bị lỗi trước khi dữ liệu được ghi xuống DB.
          - Phức tạp hơn để triển khai.
          - Dữ liệu có thể không nhất quán giữa cache và DB trong một khoảng thời gian.

      - **e. Write-Around:**

        - Khi ứng dụng ghi dữ liệu, ghi trực tiếp vào database, bỏ qua cache.
        - Dữ liệu chỉ được load vào cache khi có cache miss lúc đọc (giống Cache-Aside).
        - _Ưu điểm:_ Tránh việc cache bị "ngập" bởi dữ liệu được ghi nhiều nhưng ít được đọc.
        - _Nhược điểm:_ Độ trễ đọc cao hơn cho dữ liệu vừa được ghi (vì phải miss cache trước).

      - **Lựa chọn chiến lược nào?**
        - **Cache-Aside** là điểm khởi đầu tốt cho hầu hết các trường hợp.
        - **Write-Through** tốt nếu cần tính nhất quán cao và chấp nhận độ trễ ghi.
        - **Write-Back** cho các ứng dụng cần hiệu năng ghi cực cao và chấp nhận rủi ro mất dữ liệu.
        - **Read-Through** và **Write-Around** ít phổ biến hơn.

  5.  **Các Vấn đề Cần Cân nhắc khi Caching:**

      - **a. Cache Eviction Policies (Chính sách Loại bỏ Cache):**

        - Khi cache đầy, cần có chính sách để quyết định entry nào sẽ bị loại bỏ để nhường chỗ cho entry mới.
        - Redis hỗ trợ nhiều policy:
          - `noeviction`: Không loại bỏ, trả lỗi khi đầy (mặc định).
          - `allkeys-lru`: Loại bỏ key ít được sử dụng gần đây nhất (Least Recently Used) trong toàn bộ keyspace.
          - `volatile-lru`: LRU chỉ trên các key có đặt TTL (expire).
          - `allkeys-random`: Loại bỏ key ngẫu nhiên.
          - `volatile-random`: Random trên các key có TTL.
          - `volatile-ttl`: Loại bỏ key có TTL gần hết hạn nhất.
          - `allkeys-lfu` (từ Redis 4.0): Loại bỏ key ít được sử dụng thường xuyên nhất (Least Frequently Used).
          - `volatile-lfu`: LFU trên các key có TTL.
        - Lựa chọn policy phụ thuộc vào mẫu truy cập dữ liệu của bạn. LRU và LFU là phổ biến.

      - **b. Cache Invalidation (Làm Mất hiệu lực Cache):**

        - Khi dữ liệu gốc (trong DB) thay đổi, làm thế nào để đảm bảo cache cũng được cập nhật hoặc xóa đi để tránh phục vụ dữ liệu cũ (stale data)?
        - **Cách tiếp cận:**
          - **TTL (Time-To-Live):** Đặt thời gian hết hạn cho mỗi cache entry. Đơn giản nhưng dữ liệu có thể cũ trong khoảng TTL.
          - **Write-Through:** Dữ liệu trong cache được cập nhật đồng thời với DB.
          - **Explicit Invalidation:** Khi dữ liệu trong DB thay đổi (sau khi UPDATE/DELETE), ứng dụng chủ động gửi lệnh xóa (DELETE) cache entry tương ứng. Cần cẩn thận để xử lý lỗi khi xóa cache.
          - **Event-Driven Invalidation:** Sử dụng message queue hoặc CDC (Change Data Capture) từ DB để phát đi sự kiện thay đổi dữ liệu, một service khác sẽ lắng nghe và invalidate cache. Phức tạp hơn nhưng mạnh mẽ.
        - **"There are only two hard things in Computer Science: cache invalidation and naming things."** - Cache invalidation rất khó làm đúng.

      - **c. Cache Stampede (Thundering Herd):**

        - Xảy ra khi nhiều request cùng lúc bị cache miss cho cùng một key (ví dụ: key vừa hết hạn), và tất cả chúng đều cố gắng load dữ liệu từ DB và ghi lại vào cache. Điều này có thể làm quá tải DB.
        - **Giải pháp:**
          - **Probabilistic Early Expiration / Pre-computation:** Làm mới cache trước khi nó thực sự hết hạn (ví dụ: một background job).
          - **Locks (Mutexes):** Chỉ cho phép một request load dữ liệu từ DB và cập nhật cache, các request khác chờ hoặc nhận dữ liệu cũ (nếu chấp nhận được). Redis có thể dùng cho distributed locks (ví dụ: Redlock).
          - Sử dụng các thư viện cache client có sẵn cơ chế chống stampede.

      - **d. Cache Penetration (Xuyên thủng Cache):**
        - Xảy ra khi kẻ tấn công (hoặc lỗi logic) liên tục request các key không tồn tại cả trong cache và DB. Mỗi request này sẽ gây ra một cache miss và một truy vấn xuống DB, làm quá tải DB.
        - **Giải pháp:**
          - **Bloom Filters:** Một cấu trúc dữ liệu xác suất để kiểm tra nhanh xem một item có "khả năng" tồn tại trong một tập hợp hay không, trước khi query DB. Nếu Bloom filter nói "không", chắc chắn không có. Nếu nói "có", có thể có (false positive) hoặc thực sự có.
          - **Cache Nulls / Negative Caching:** Nếu một key được xác định là không tồn tại trong DB, hãy cache một giá trị đặc biệt (ví dụ: "NULL") cho key đó với một TTL ngắn. Request tiếp theo cho key này sẽ hit cache "NULL" và không query DB.

  6.  **Áp dụng Caching vào Dự án Helios:**
      - **Cache-Aside cho `AnomalyDetectionEngine` / `DashboardAPI`:**
        - Khi `DashboardAPI` cần lấy danh sách các `DetectedAnomaly` theo một bộ lọc nào đó.
        - Key cache có thể là `anomalies:<sensor_id>:<start_time>:<end_time>:<limit>`.
        - Value là danh sách `DetectedAnomaly` đã được serialize (JSON).
        - Đặt TTL hợp lý (ví dụ: 1-5 phút, tùy thuộc vào tần suất dữ liệu mới được tạo ra).
      - **Cache thông tin `Sensor` (nếu ít thay đổi):**
        - Key: `sensor:<sensor_id>`
        - Value: `Sensor` object serialized.
      - **Invalidation:**
        - Khi một `AnomalyRule` được cập nhật, có thể cần invalidate các cache liên quan đến kết quả phân tích cũ (phức tạp).
        - Khi một `DetectedAnomaly` mới được `AnomalyDetectionEngine` lưu vào DB, có thể không cần invalidate ngay lập tức các cache của `DashboardAPI` nếu chấp nhận dữ liệu hơi trễ một chút (dựa vào TTL). Hoặc có thể sử dụng một cơ chế event-driven đơn giản để báo cho `DashboardAPI` biết là có dữ liệu mới.
      - **Serialization:** Sử dụng `json.Marshal` và `json.Unmarshal` để lưu struct vào Redis strings.

- **Code minh họa / sơ đồ:**

  - Ví dụ code `RedisClient` ở trên.
  - **Sơ đồ Cache-Aside (Lặp lại cho rõ):**
    ```
    Client App -->| 1. Query Cache for Key 'X' |
                  V
            +-----------+
            |   Cache   |
            +-----------+
              |        ^
    (Hit) 2a. Return   | (Miss) 2b. Key 'X' not found
          Data 'X'     |
              |        |
              V        V
    Client App <------| 3. Query Database for Key 'X'
                          |
                          V
                    +-----------+
                    |  Database |
                    +-----------+
                          | 4. Return Data 'X'
                          V
                  | 5. Store Data 'X' in Cache | ----> Cache
                          |
                          V
                  | 6. Return Data 'X' to Client App |
    ```

- **Best practices:**

  1.  **Xác định rõ dữ liệu nào cần cache:** Không phải mọi thứ đều nên cache.
  2.  **Chọn chiến lược caching phù hợp.** Cache-Aside là điểm khởi đầu tốt.
  3.  **Sử dụng thư viện client Redis tốt (`go-redis/redis`).**
  4.  **Quản lý TTL cẩn thận.**
  5.  **Có chiến lược Invalidation rõ ràng.**
  6.  **Xử lý Cache Miss một cách duyên dáng.**
  7.  **Serialize/Deserialize hiệu quả (JSON, Gob, hoặc Protobuf nếu dùng chung với gRPC).**
  8.  **Theo dõi (Monitor) Cache:** Hit rate, miss rate, memory usage, latency.
  9.  **Bảo mật Redis Instance:** Đặt password, giới hạn network access.
  10. **Cân nhắc các vấn đề như Cache Stampede, Penetration.**

- **Anti-patterns / lỗi phổ biến:**

  1.  **Caching dữ liệu thay đổi quá thường xuyên mà không có Invalidation tốt:** Dẫn đến stale data.
  2.  **Không đặt TTL hoặc TTL quá dài:** Cache chứa dữ liệu cũ, tốn bộ nhớ.
  3.  **Key cache không đủ cụ thể:** Gây xung đột hoặc trả về sai dữ liệu.
  4.  **Không xử lý lỗi khi thao tác với cache (ví dụ: Redis down):** Ứng dụng nên có thể fallback về nguồn gốc.
  5.  **Cố gắng cache quá nhiều thứ, làm phức tạp hệ thống không cần thiết.**
  6.  **Bỏ qua việc serialize/deserialize errors.**

- **So sánh các lựa chọn / cách tiếp cận (nếu có):**

  - **In-process Cache vs. Distributed Cache (Redis, Memcached):**
    - **In-process Cache (ví dụ: `patrickmn/go-cache`, hoặc map với mutex):**
      - _Ưu điểm:_ Rất nhanh (không có network overhead). Đơn giản cho các ứng dụng đơn instance.
      - _Nhược điểm:_ Cache không được chia sẻ giữa các instance của service. Bộ nhớ cache bị giới hạn bởi RAM của từng instance. Cache bị mất khi instance restart.
    - **Distributed Cache (Redis):**
      - _Ưu điểm:_ Cache được chia sẻ giữa nhiều instance. Khả năng mở rộng tốt hơn. Có thể bền bỉ.
      - _Nhược điểm:_ Có network latency (nhưng vẫn nhanh hơn DB). Cần quản lý một service riêng (Redis).
    - **Lựa chọn:** Cho microservices, **Distributed Cache (Redis)** thường là lựa chọn tốt hơn.
  - **Redis vs. Memcached:**
    - Memcached: Đơn giản hơn Redis, chủ yếu là key-value store in-memory, đa luồng (multi-threaded).
    - Redis: Nhiều kiểu dữ liệu hơn, persistence, single-threaded (nhưng non-blocking I/O), có các tính năng nâng cao (Lua, Pub/Sub, Streams, Cluster).
    - **Lựa chọn:** Redis thường được ưa chuộng hơn hiện nay do bộ tính năng phong phú.

- **Gợi ý mở rộng kiến thức:**
  - **Redis Documentation:** (https://redis.io/documentation)
  - **`go-redis/redis` documentation:** (https://redis.uptrace.dev/)
  - Các bài viết về caching patterns và anti-patterns.
  - Tìm hiểu về các giải pháp chống Cache Stampede (ví dụ: using `singleflight` package trong Go).
  - Nghiên cứu về Bloom Filters.
  - **Trích dẫn (Tim Bray):** "The Caching Axiom: If you have a performance problem, and you look closely enough, you’ll find that the data you need is far away."

---

Caching là một công cụ mạnh mẽ để cải thiện hiệu năng. Phần 10 này đã cung cấp cho bạn kiến thức nền tảng để sử dụng Redis một cách hiệu quả trong ứng dụng Go.

Tiếp theo, chúng ta sẽ khám phá một khía cạnh khác của việc xây dựng microservices mạnh mẽ: **PHẦN 11: XỬ LÝ TÁC VỤ BẤT ĐỒNG BỘ VÀ MESSAGE QUEUES.**

Chắc chắn rồi, chúng ta sẽ tiếp tục với **PHẦN 11: XỬ LÝ TÁC VỤ BẤT ĐỒNG BỘ VÀ MESSAGE QUEUES (VỚI NATS LÀM VÍ DỤ).**

- **Tên phần học:** Xử lý Tác vụ Bất đồng bộ và Message Queues (với NATS làm ví dụ)
- **Mục tiêu học phần:**

  - Hiểu TẠI SAO xử lý bất đồng bộ lại quan trọng trong microservices để cải thiện khả năng đáp ứng, khả năng phục hồi và khả năng mở rộng.
  - Nắm vững khái niệm Message Queue (MQ), các thành phần cơ bản (Producer, Consumer, Broker, Queue/Topic), và các mô hình messaging phổ biến (Point-to-Point, Publish/Subscribe).
  - Tìm hiểu về NATS: là gì, kiến trúc, các tính năng chính (Core NATS, NATS JetStream), và tại sao nó là một lựa chọn tốt cho Go.
  - Học cách sử dụng NATS client trong Go để publish và subscribe messages, cả với Core NATS và JetStream (để có persistence và guaranteed delivery).
  - Hiểu các vấn đề cần cân nhắc khi làm việc với MQ: message durability (độ bền), delivery guarantees (đảm bảo giao nhận), idempotency (tính lũy đẳng), error handling, và dead-letter queues.
  - Áp dụng NATS vào dự án Helios, ví dụ: `AnomalyDetectionEngine` publish sự kiện bất thường lên một topic, và `NotificationService` subscribe topic đó để gửi thông báo.

- **Giải thích lý thuyết kỹ càng:**

  1.  **Tại sao Xử lý Bất đồng bộ lại Quan trọng?**

      - **Cải thiện Khả năng Đáp ứng (Responsiveness):** Trong một request đồng bộ, client phải chờ cho đến khi server hoàn thành toàn bộ tác vụ (có thể bao gồm nhiều bước, gọi các service khác). Với xử lý bất đồng bộ, server có thể nhanh chóng xác nhận đã nhận request, đưa tác vụ vào một hàng đợi (queue), và trả lời client ngay. Tác vụ sẽ được xử lý sau đó bởi một worker khác. Điều này làm giảm đáng kể thời gian chờ của client.
        - _Ví dụ:_ Khi người dùng đặt hàng, thay vì chờ toàn bộ quá trình (kiểm tra kho, xử lý thanh toán, gửi email xác nhận) hoàn tất, API có thể chỉ cần ghi nhận đơn hàng, đưa vào queue, và báo thành công cho người dùng.
      - **Tăng Khả năng Phục hồi (Resilience) và Độ tin cậy (Reliability):**
        - Nếu một service phụ thuộc (downstream service) tạm thời không khả dụng, thay vì làm request của client fail, tác vụ có thể được lưu trong queue và được xử lý lại khi service đó hoạt động trở lại.
        - Message queues thường có cơ chế retry và dead-letter queues (DLQ) để xử lý các message không thể xử lý thành công.
      - **Tăng Khả năng Mở rộng (Scalability):**
        - Các consumer (workers xử lý message từ queue) có thể được scale độc lập với các producer (services tạo ra message). Nếu có nhiều message trong queue, bạn có thể tăng số lượng consumer.
        - Giúp "san phẳng" các đỉnh tải (load leveling/peak shaving). Thay vì tất cả request dồn dập vào một thời điểm, chúng được xếp hàng và xử lý từ từ.
      - **Decoupling (Giảm Khớp nối):**
        - Producer không cần biết về consumer, và ngược lại. Chúng chỉ cần biết về message queue và định dạng message. Điều này cho phép các service phát triển và deploy độc lập.
        - Dễ dàng thêm consumer mới cho cùng một loại message mà không ảnh hưởng đến producer hoặc các consumer hiện có (đặc biệt với Publish/Subscribe).

  2.  **Message Queue (MQ) là gì?**

      - Message Queue là một thành phần phần mềm trung gian (middleware) cho phép các ứng dụng/components giao tiếp với nhau một cách bất đồng bộ bằng cách gửi và nhận các "message" (thông điệp).
      - **Các thành phần chính:**
        - **Producer (Publisher):** Ứng dụng/component gửi message đến queue/topic.
        - **Consumer (Subscriber):** Ứng dụng/component nhận và xử lý message từ queue/topic.
        - **Broker (Server):** Hệ thống MQ trung tâm chịu trách nhiệm lưu trữ, định tuyến, và quản lý message.
        - **Queue (Hàng đợi) / Topic (Chủ đề):**
          - **Queue (Point-to-Point):** Một message được gửi đến một queue cụ thể. Thường chỉ có một consumer xử lý message đó (mặc dù có thể có nhiều consumer cạnh tranh để lấy message từ cùng một queue – competing consumers pattern).
          - **Topic (Publish/Subscribe - Pub/Sub):** Một message được publish lên một topic. Tất cả các subscriber đã đăng ký topic đó sẽ nhận được một bản sao của message.
      - **Message:** Đơn vị dữ liệu được truyền qua MQ. Thường chứa payload (dữ liệu thực tế, ví dụ JSON, Protobuf, XML) và metadata (headers, properties).

  3.  **NATS là gì?**

      - NATS (Neural Autonomic Transport System) là một hệ thống messaging mã nguồn mở, hiệu năng rất cao, đơn giản, và an toàn, được viết bằng Go.
      - **Kiến trúc và Triết lý:**
        - **Đơn giản và Hiệu năng cao:** NATS tập trung vào việc là một "ống dẫn" (dial tone) message cực nhanh và nhẹ.
        - **"Fire-and-Forget" (Core NATS):** Mô hình cơ bản của NATS là "at-most-once" delivery. Nếu subscriber không online hoặc không theo kịp, message có thể bị mất. Điều này làm cho Core NATS cực kỳ nhanh nhưng cần client tự xử lý độ tin cậy nếu cần.
        - **NATS JetStream (từ NATS 2.2.0):** Một lớp persistence và streaming được xây dựng trên Core NATS, cung cấp các tính năng như:
          - **Message Persistence (Lưu trữ bền bỉ):** Message được lưu trữ trên disk.
          - **Delivery Guarantees:** "At-least-once" và "exactly-once" (trong một số điều kiện).
          - **Message Replay:** Khả năng đọc lại message cũ.
          - **Consumer Acknowledgements (ACK/NAK):** Consumer phải xác nhận đã xử lý message.
          - **Streams và Consumers:** Khái niệm tương tự Kafka topics và consumer groups.
        - **Clustering:** NATS server có thể được cluster lại với nhau để tăng tính sẵn sàng và khả năng mở rộng.
      - **Tại sao NATS là lựa chọn tốt cho Go?**
        - Viết bằng Go, tích hợp tự nhiên.
        - Client Go rất tốt và được duy trì tích cực.
        - Rất nhẹ và dễ triển khai, dễ sử dụng.
        - Hiệu năng xuất sắc.
        - JetStream cung cấp các tính năng "enterprise" cần thiết cho nhiều ứng dụng.

  4.  **Sử dụng NATS Client trong Go (`nats-io/nats.go`):**

      - **a. Core NATS (Publish/Subscribe cơ bản):**

        ```go
        // nats_core_example/main.go
        package main

        import (
            "log"
            "runtime"
            "time"

            "github.com/nats-io/nats.go"
        )

        func main() {
            // Kết nối đến NATS server (mặc định là nats://localhost:4222)
            nc, err := nats.Connect(nats.DefaultURL, nats.Name("My Go Publisher/Subscriber"))
            if err != nil {
                log.Fatalf("Error connecting to NATS: %v", err)
            }
            defer nc.Close() // Đóng kết nối khi xong
            log.Println("Connected to NATS server!")

            subject := "helios.events.anomalies" // Tên subject (topic)

            // Subscriber (Async)
            // Queue group "workers" cho phép nhiều instance của subscriber này
            // cùng chia sẻ việc xử lý message trên subject (load balancing).
            // Nếu không có queue group, mỗi subscriber sẽ nhận 1 bản sao.
            sub, err := nc.QueueSubscribe(subject, "anomaly_processing_workers", func(msg *nats.Msg) {
                log.Printf("[WORKER] Received on subject '%s' (queue: 'anomaly_processing_workers'): %s", msg.Subject, string(msg.Data))
                // Xử lý message ở đây
                // msg.Respond([]byte("Acknowledged by worker")) // Nếu cần gửi reply (Request-Reply)
            })
            if err != nil {
                log.Fatalf("Error subscribing: %v", err)
            }
            // sub.Unsubscribe() // Để hủy đăng ký

            // Subscriber khác (không trong queue group)
            nc.Subscribe(subject, func(msg *nats.Msg) {
                log.Printf("[AUDITOR] Received on subject '%s': %s", msg.Subject, string(msg.Data))
            })


            // Publisher
            go func() {
                for i := 0; i < 10; i++ {
                    message := fmt.Sprintf("Anomaly detected - ID: %d, Severity: High", 1000+i)
                    // Publish message
                    if err := nc.Publish(subject, []byte(message)); err != nil {
                        log.Printf("Error publishing message %d: %v", i, err)
                    } else {
                        log.Printf("Published message %d: %s", i, message)
                    }
                    time.Sleep(500 * time.Millisecond)
                }
            }()

            // Request-Reply Pattern (Publisher gửi request và chờ reply)
            go func() {
                time.Sleep(1 * time.Second) // Chờ subscriber sẵn sàng
                payload := "Requesting processing for urgent anomaly XYZ"
                // Timeout cho request là 2 giây
                replyMsg, err := nc.Request(subject, []byte(payload), 2*time.Second)
                if err != nil {
                    log.Printf("Error making request: %v", err)
                } else {
                    log.Printf("Received reply for '%s': %s", payload, string(replyMsg.Data))
                }
            }()


            // Giữ chương trình chạy để subscriber có thể nhận message
            // Trong ứng dụng thực tế, đây sẽ là một service chạy dài hạn
            log.Println("Listening for messages... Press CTRL+C to exit.")
            runtime.Goexit() // Hoặc select{}
        }
        ```

      - **b. NATS JetStream (Persistence và Guaranteed Delivery):**

        - Cần NATS server có bật JetStream.
        - **Khái niệm JetStream:**
          - **Stream:** Một log message có tên, được lưu trữ bền bỉ. Tương tự Kafka topic. Stream định nghĩa retention policy (giữ message bao lâu, bao nhiêu message), storage type (file, memory).
          - **Consumer:** Một "view" vào stream, cho phép đọc message. Consumer có thể là:
            - **Pull Consumer:** Client chủ động `Fetch()` message.
            - **Push Consumer (Durable / Ephemeral):** JetStream đẩy message đến một subject mà client subscribe.
              - _Durable Consumer:_ Trạng thái của consumer (message nào đã được ACK) được lưu trữ. Nếu consumer offline rồi online lại, nó sẽ tiếp tục từ chỗ đã dừng.
              - _Ephemeral Consumer:_ Không có trạng thái bền bỉ, mất khi subscriber disconnect.
          - **Message Acknowledgement:** Consumer phải `msg.Ack()` để báo cho JetStream biết message đã được xử lý thành công. Nếu không ACK (hoặc `msg.Nak()`, `msg.Term()`), JetStream sẽ cố gắng giao lại message (redeliver).

        ```go
        // nats_jetstream_example/main.go
        package main

        import (
            "context"
            "log"
            "runtime"
            "time"
            "fmt"

            "github.com/nats-io/nats.go"
        )

        const (
            streamName     = "HELIOS_ANOMALIES"
            streamSubjects = "helios.jetstream.anomalies.>" // Subject pattern cho stream
            publishSubject = "helios.jetstream.anomalies.detected"
            consumerName   = "AnomalyNotifier"
        )

        func main() {
            nc, err := nats.Connect(nats.DefaultURL, nats.Name("My JetStream App"))
            if err != nil {
                log.Fatalf("Error connecting to NATS: %v", err)
            }
            defer nc.Close()
            log.Println("Connected to NATS server!")

            // 1. Lấy JetStream context
            js, err := nc.JetStream(nats.PublishAsyncMaxPending(256)) // Context để tương tác với JetStream
            if err != nil {
                log.Fatalf("Error getting JetStream context: %v", err)
            }

            // 2. Tạo hoặc lấy Stream (idempotent)
            // Stream sẽ lưu trữ tất cả message publish đến "helios.jetstream.anomalies.>"
            _, err = js.AddStream(&nats.StreamConfig{
                Name:      streamName,
                Subjects:  []string{streamSubjects},
                Storage:   nats.FileStorage, // Lưu trữ trên file
                Retention: nats.LimitsPolicy,  // Giữ lại theo giới hạn (số lượng, kích thước, tuổi)
                MaxAge:    24 * time.Hour * 7, // Giữ message trong 7 ngày
            })
            if err != nil {
                // Kiểm tra xem lỗi có phải là stream đã tồn tại không
                // if !errors.Is(err, nats.ErrStreamNameAlreadyInUse) { // Cách kiểm tra lỗi cụ thể của NATS
                log.Printf("Error adding stream (may already exist): %v", err)
                // }
            } else {
                log.Printf("Stream '%s' created/updated.", streamName)
            }


            // 3. Subscriber (Push Consumer - Durable)
            // Consumer này sẽ nhận message từ stream HELIOS_ANOMALIES
            // DurableName giúp NATS nhớ vị trí của consumer này ngay cả khi nó offline
            // AckPolicyAll: Phải ACK tất cả message trong batch (nếu có batching)
            // MaxDeliver: Số lần tối đa JetStream cố gắng giao lại một message
            sub, err := js.Subscribe(publishSubject, func(msg *nats.Msg) {
                log.Printf("[NOTIFIER SUB] Received JetStream msg on subject '%s', seq: %d, data: %s",
                    msg.Subject, msg.Metadata.Sequence.Stream, string(msg.Data))

                // Xử lý message
                // Ví dụ: gửi email, push notification
                processingTime := time.Duration(rand.Intn(500)+100) * time.Millisecond
                time.Sleep(processingTime) // Giả lập xử lý

                // Quan trọng: Phải ACK message để JetStream biết đã xử lý thành công
                if err := msg.Ack(); err != nil {
                    log.Printf("Error ACK'ing message (seq: %d): %v", msg.Metadata.Sequence.Stream, err)
                    // Nếu ACK lỗi, JetStream có thể sẽ redeliver
                } else {
                    log.Printf("ACKed message (seq: %d)", msg.Metadata.Sequence.Stream)
                }

                // msg.Nak() // Báo không xử lý được, JetStream redeliver ngay (nếu còn retry)
                // msg.Term() // Báo không xử lý được và không muốn redeliver (coi như DLQ)
                // msg.InProgress() // Báo đang xử lý, reset redelivery timer (dùng cho tác vụ lâu)

            }, nats.Durable(consumerName), nats.AckWait(30*time.Second), nats.MaxDeliver(5))
            if err != nil {
                log.Fatalf("Error subscribing to JetStream: %v", err)
            }
            defer sub.Unsubscribe() // Hoặc sub.Drain() để xử lý hết message đang chờ

            // 4. Publisher
            go func() {
                for i := 0; i < 20; i++ {
                    payload := fmt.Sprintf(`{"id": "anomaly-%03d", "sensor": "temp-01", "value": %.1f}`, i, 30.0+float64(i)*0.5)
                    // Publish message đến subject mà Stream đang lắng nghe
                    // Trả về PubAck (publish acknowledgement)
                    pubAck, err := js.Publish(publishSubject, []byte(payload))
                    if err != nil {
                        log.Printf("Error publishing JetStream message %d: %v", i, err)
                    } else {
                        log.Printf("Published JetStream message %d, stream: %s, seq: %d", i, pubAck.Stream, pubAck.Sequence)
                    }
                    time.Sleep(200 * time.Millisecond)
                }
            }()

            log.Println("JetStream example running... Press CTRL+C to exit.")
            runtime.Goexit()
        }
        ```

  5.  **Các Vấn đề Cần Cân nhắc khi làm việc với MQ:**

      - **Message Durability (Độ bền của Message):**
        - Message có được lưu trữ bền bỉ (trên disk) để không bị mất nếu broker restart không?
        - Core NATS là in-memory. NATS JetStream cung cấp persistence. Các MQ khác (Kafka, RabbitMQ) cũng có.
      - **Delivery Guarantees (Đảm bảo Giao nhận):**
        - **At-most-once:** Message được giao tối đa một lần. Có thể mất nếu subscriber offline hoặc broker lỗi. (Ví dụ: Core NATS).
        - **At-least-once:** Message được giao ít nhất một lần. Có thể bị giao lặp lại nếu ACK bị lỗi hoặc consumer crash trước khi ACK. Consumer cần xử lý message một cách idempotent. (Ví dụ: NATS JetStream với ACK).
        - **Exactly-once:** Message được giao đúng một lần. Rất khó đạt được trong hệ thống phân tán, thường yêu cầu sự phối hợp giữa MQ và logic của consumer (ví dụ: transactional processing, deduplication).
      - **Idempotency (Tính Lũy đẳng của Consumer):**
        - Consumer nên được thiết kế sao cho việc xử lý cùng một message nhiều lần không gây ra tác dụng phụ không mong muốn (ví dụ: không tạo nhiều đơn hàng cho cùng một request).
        - Cách thực hiện: Kiểm tra ID của message (hoặc một ID nghiệp vụ trong payload) xem đã được xử lý trước đó chưa (có thể lưu trạng thái vào DB/cache).
      - **Error Handling và Retries:**
        - Consumer nên xử lý lỗi một cách cẩn thận.
        - Với các lỗi tạm thời (transient errors, ví dụ: network glitch khi gọi service khác), có thể `Nak()` message để MQ redeliver sau một khoảng thời gian.
        - Với các lỗi không thể phục hồi (permanent errors, ví dụ: message bị hỏng, logic nghiệp vụ không cho phép), nên `Term()` message hoặc chuyển nó vào một Dead-Letter Queue (DLQ).
      - **Dead-Letter Queue (DLQ):**
        - Một queue đặc biệt để chứa các message không thể xử lý thành công sau một số lần retry.
        - Giúp tách biệt message lỗi khỏi luồng chính, cho phép developer điều tra và xử lý thủ công sau đó.
        - NATS JetStream hỗ trợ cấu hình redelivery policy, có thể dùng `msg.Term()` để mô phỏng việc đưa vào DLQ (bằng cách không ACK và để nó hết MaxDeliver).
      - **Ordering (Thứ tự Message):**
        - Một số MQ đảm bảo thứ tự message trong một partition/queue (ví dụ: Kafka, NATS JetStream trong một stream).
        - Tuy nhiên, nếu có nhiều consumer xử lý song song, thứ tự xử lý cuối cùng có thể không được đảm bảo trừ khi có cơ chế đồng bộ hóa.
      - **Message Serialization:** Chọn định dạng (JSON, Protobuf, Avro) phù hợp cho payload. Protobuf hiệu quả cho performance và schema evolution.

  6.  **Áp dụng NATS vào Dự án Helios:**
      - **Use Case:** Khi `AnomalyDetectionEngine` phát hiện một bất thường, nó sẽ publish một message chứa thông tin về bất thường đó lên một NATS JetStream Stream (ví dụ: `helios.anomalies.detected`).
      - **Producer (`AnomalyDetectionEngine`):**
        - Sau khi lưu `DetectedAnomaly` vào DB, sẽ tạo một message (ví dụ: JSON hoặc Protobuf) chứa các thông tin cần thiết của `DetectedAnomaly`.
        - Publish message này lên NATS JetStream.
      - **Consumer (`NotificationService`):**
        - Subscribe vào stream `helios.anomalies.detected` với một durable consumer.
        - Khi nhận được message, deserialize nó.
        - Dựa vào thông tin bất thường và các quy tắc thông báo, gửi thông báo (email, SMS, webhook – sẽ có phần riêng về notification).
        - ACK message sau khi gửi thông báo thành công (hoặc xử lý lỗi tương ứng).
      - _Lợi ích:_
        - `AnomalyDetectionEngine` không cần biết `NotificationService` tồn tại hay cách nó hoạt động.
        - Nếu `NotificationService` tạm thời offline, các message bất thường vẫn được lưu trong JetStream và sẽ được xử lý khi nó online trở lại.
        - Có thể dễ dàng thêm các service khác cũng quan tâm đến sự kiện bất thường (ví dụ: một service ghi log audit đặc biệt, một service cập nhật dashboard real-time) bằng cách cho chúng subscribe cùng một stream.

- **Code minh họa / sơ đồ:**

  - Các ví dụ code đã được lồng vào phần giải thích.
  - **Sơ đồ Helios với NATS JetStream:**
    ```
    +------------------------+     (DB Save)
    | AnomalyDetectionEngine | ---------------> +----------+
    +------------------------+                  | Database |
              |                                 +----------+
              | 1. Publish Anomaly Event (JSON/Protobuf)
              V
    +------------------------+
    | NATS JetStream         |
    | (Stream: HELIOS_ANOMALIES) |
    +------------------------+
              | 2. Push Message (At-least-once)
              V
    +------------------------+
    | NotificationService    | --(ACK)--> NATS JetStream
    | (Durable Consumer)     |
    +------------------------+
              | 3. Deserialize Message
              | 4. Send Notification (Email, SMS, etc.)
              V
         (External World)
    ```

- **Best practices:**

  1.  **Chọn đúng loại MQ và cấu hình phù hợp với yêu cầu về durability và delivery guarantees.** (Core NATS cho fire-and-forget, JetStream cho reliability).
  2.  **Thiết kế message payload rõ ràng, có versioning nếu cần.** Protobuf là lựa chọn tốt.
  3.  **Làm cho consumer idempotent.**
  4.  **Xử lý ACK/NAK/Term một cách cẩn thận.**
  5.  **Sử dụng Dead-Letter Queues (hoặc cơ chế tương đương) cho message lỗi.**
  6.  **Monitor Message Queues:** Độ dài queue, số lượng message chưa ACK, tốc độ xử lý.
  7.  **Bảo mật NATS server và connections.**
  8.  **Cân nhắc về thứ tự message nếu nó quan trọng cho nghiệp vụ.**

- **Anti-patterns / lỗi phổ biến:**

  1.  **Sử dụng MQ cho các thao tác RPC đồng bộ khi không cần thiết:** Làm tăng độ phức tạp.
  2.  **Không ACK message hoặc ACK quá sớm/quá muộn:** Dẫn đến mất message hoặc xử lý lặp không cần thiết.
  3.  **Consumer không idempotent, gây lỗi khi message được redeliver.**
  4.  **Queue bị đầy do producer nhanh hơn consumer mà không có cơ chế backpressure hoặc scaling consumer.**
  5.  **Không xử lý message trong DLQ.**
  6.  **Payload quá lớn:** MQ không được thiết kế để truyền file lớn. Nên truyền tham chiếu đến file (ví dụ: S3 URL).

- **So sánh các lựa chọn / cách tiếp cận (nếu có):**

  - **NATS vs. Kafka vs. RabbitMQ:**
    - **Kafka:** Rất mạnh mẽ cho high-throughput event streaming, log aggregation. Phức tạp hơn để vận hành. Đảm bảo thứ tự trong partition.
    - **RabbitMQ:** MQ truyền thống, linh hoạt với nhiều protocol (AMQP, MQTT, STOMP). Nhiều tính năng routing phức tạp.
    - **NATS:** Đơn giản, hiệu năng cực cao. Core NATS cho messaging nhanh, JetStream cho streaming và persistence. Dễ vận hành hơn Kafka.
    - **Lựa chọn:**
      - NATS là lựa chọn tuyệt vời cho microservices cần sự đơn giản, hiệu năng, và các tính năng streaming cơ bản đến nâng cao với JetStream.
      - Kafka phù hợp cho các hệ thống "big data" streaming, event sourcing quy mô lớn.
      - RabbitMQ là một lựa chọn linh hoạt, "all-around" tốt.
    - _Cho dự án Helios (mục tiêu học tập):_ NATS (đặc biệt là JetStream) là một lựa chọn rất phù hợp vì sự đơn giản, hiệu năng, và cộng đồng Go mạnh mẽ.

- **Gợi ý mở rộng kiến thức:**
  - **NATS Documentation:** (https://docs.nats.io/) - Rất chi tiết và dễ hiểu.
  - **NATS JetStream Documentation:** Cụ thể hơn về JetStream.
  - **Sách "Designing Data-Intensive Applications" (Martin Kleppmann):** Có các chương rất hay về message queues và event streaming.
  - Tìm hiểu về các pattern như "Saga" để quản lý distributed transactions sử dụng message queues.
  - Nghiên cứu về backpressure mechanisms.
  - **Trích dẫn:** "The great thing about distributed systems is that you have so many more ways for them to fail." (Message queues giúp quản lý một số dạng failure đó!)

---

Xử lý bất đồng bộ với message queues là một kỹ thuật then chốt để xây dựng các hệ thống microservices hiện đại, có khả năng phục hồi và mở rộng tốt. Phần 11 này đã trang bị cho bạn kiến thức để bắt đầu với NATS.

Tiếp theo, chúng ta sẽ thảo luận về một chủ đề quan trọng để đảm bảo sự ổn định và an toàn của API: **PHẦN 12: AUTHENTICATION VÀ AUTHORIZATION TRONG MICROSERVICES.**

Chắc chắn rồi, chúng ta sẽ đi vào một khía cạnh cực kỳ quan trọng đối với bất kỳ hệ thống nào: **PHẦN 12: AUTHENTICATION VÀ AUTHORIZATION TRONG MICROSERVICES.**

- **Tên phần học:** Authentication và Authorization trong Microservices
- **Mục tiêu học phần:**

  - Hiểu rõ sự khác biệt giữa Authentication (Xác thực - Ai là bạn?) và Authorization (Ủy quyền - Bạn được làm gì?).
  - Nắm vững các phương pháp Authentication phổ biến trong microservices: API Keys, Basic Auth (ít dùng), Token-based (JWT, PASETO), OAuth 2.0.
  - Học cách triển khai Token-based Authentication (cụ thể là JWT) trong Go: tạo token, validate token, và truyền token giữa các service.
  - Hiểu các chiến lược Authorization: Role-Based Access Control (RBAC), Attribute-Based Access Control (ABAC), và cách triển khai chúng.
  - Tìm hiểu về các giải pháp tập trung cho AuthN/AuthZ như API Gateway, Identity Provider (IdP), và các service chuyên dụng (ví dụ: Keycloak, Auth0, Open Policy Agent - OPA).
  - Áp dụng các khái niệm này vào dự án Helios: bảo vệ các API endpoints, đảm bảo chỉ người dùng/service được phép mới có thể thực hiện các hành động nhất định.

- **Giải thích lý thuyết kỹ càng:**

  1.  **Authentication (Xác thực) vs. Authorization (Ủy quyền):**

      - **Authentication (AuthN):** Là quá trình xác minh danh tính của một chủ thể (user, service, device). Trả lời câu hỏi: "Ai là bạn?".
        - Ví dụ: Người dùng cung cấp username/password, hệ thống kiểm tra và xác nhận đó đúng là người dùng A. Một service cung cấp API key, hệ thống kiểm tra và xác nhận đó là service B.
      - **Authorization (AuthZ) (còn gọi là Access Control):** Là quá trình xác định xem một chủ thể đã được xác thực (authenticated principal) có quyền thực hiện một hành động cụ thể trên một tài nguyên cụ thể hay không. Trả lời câu hỏi: "Bạn được phép làm gì?".
        - Ví dụ: Sau khi người dùng A được xác thực, hệ thống kiểm tra xem người dùng A có quyền "đọc" (read) tài liệu "X" hay không, hoặc có quyền "xóa" (delete) cảm biến "Y" hay không. Service B có được phép "ghi" (write) dữ liệu vào service C hay không.
      - **Quan hệ:** Authorization luôn diễn ra _sau khi_ Authentication thành công. Bạn không thể ủy quyền cho một người mà bạn không biết là ai.

  2.  **Các Phương pháp Authentication Phổ biến trong Microservices:**

      - **a. API Keys:**

        - Một chuỗi bí mật duy nhất được gán cho mỗi client (thường là service hoặc ứng dụng bên thứ ba).
        - Client gửi API key trong header HTTP (ví dụ: `Authorization: ApiKey <your_api_key>` hoặc `X-API-Key: <your_api_key>`).
        - Server kiểm tra key trong database hoặc một store an toàn.
        - _Ưu điểm:_ Đơn giản để triển khai và sử dụng, đặc biệt cho service-to-service communication hoặc các API công khai đơn giản.
        - _Nhược điểm:_
          - API key là một secret tĩnh, nếu bị lộ thì kẻ tấn công có toàn quyền của client đó.
          - Khó thu hồi một cách chi tiết (thường là thu hồi toàn bộ key).
          - Không phù hợp cho xác thực người dùng cuối (end-user authentication) trực tiếp.
          - Không có thông tin về danh tính người dùng, chỉ là danh tính của ứng dụng/service gọi.

      - **b. Basic Authentication (HTTP Basic Auth):**

        - Client gửi username và password được encode Base64 trong header `Authorization: Basic <base64_encoded_username:password>`.
        - _Ưu điểm:_ Rất đơn giản, được hỗ trợ rộng rãi.
        - _Nhược điểm:_
          - **Cực kỳ không an toàn nếu không dùng HTTPS,** vì username/password dễ dàng bị giải mã.
          - Password được gửi đi trong mỗi request.
          - Không phù hợp cho microservices hiện đại, chỉ nên dùng trong các trường hợp rất hạn chế và luôn qua HTTPS.

      - **c. Token-based Authentication (Xác thực dựa trên Token):**

        - Đây là phương pháp phổ biến nhất cho microservices và web applications hiện đại.
        - **Luồng hoạt động chung:**
          1.  Client (user/service) xác thực với một **Identity Provider (IdP)** hoặc một **Authentication Service** bằng credentials (username/password, API key, client credentials).
          2.  IdP/Auth Service cấp một **access token** (thường có thời hạn ngắn) và có thể cả một **refresh token** (thường có thời hạn dài hơn).
          3.  Client gửi access token này trong header `Authorization: Bearer <access_token>` của mỗi request đến các Resource Server (các microservice cần bảo vệ).
          4.  Resource Server (microservice) **validate token** (kiểm tra chữ ký, thời hạn, issuer, audience).
          5.  Nếu token hợp lệ, Resource Server xử lý request.
          6.  Nếu access token hết hạn, client có thể dùng refresh token để xin access token mới từ IdP/Auth Service mà không cần người dùng nhập lại credentials.
        - **Các loại Token phổ biến:**
          - **JWT (JSON Web Token - RFC 7519):**
            - Một chuẩn mở (RFC 7519) định nghĩa một cách nhỏ gọn và khép kín (self-contained) để truyền thông tin giữa các bên dưới dạng một đối tượng JSON.
            - Token được ký điện tử (digitally signed) bằng HMAC hoặc RSA/ECDSA. Chữ ký đảm bảo token không bị sửa đổi. Token cũng có thể được mã hóa (encrypted) nếu cần bảo vệ nội dung.
            - **Cấu trúc JWT:** Gồm 3 phần, phân cách bởi dấu chấm (`.`):
              1.  **Header:** Chứa metadata về token (loại token - `typ`, thuật toán ký - `alg`). Base64Url encoded.
              2.  **Payload (Claims):** Chứa các "claims" (thông tin) về chủ thể (ví dụ: user ID, username, roles, permissions) và các metadata khác (issuer - `iss`, expiration time - `exp`, issued at - `iat`, audience - `aud`, subject - `sub`). Base64Url encoded. **Payload không được mã hóa mặc định, không nên chứa thông tin nhạy cảm trừ khi toàn bộ JWT được mã hóa.**
              3.  **Signature:** Được tạo bằng cách ký (Header + `.` + Payload) với một secret (HMAC) hoặc private key (RSA/ECDSA). Dùng để xác minh tính toàn vẹn của token.
            - _Ưu điểm JWT:_ Stateless (server không cần lưu trữ session state cho token, thông tin người dùng nằm trong token). Self-contained. Được hỗ trợ rộng rãi.
            - _Nhược điểm JWT:_ Khó thu hồi token trước khi nó hết hạn (vì stateless). Nếu token bị lộ, nó có thể được sử dụng cho đến khi hết hạn. Payload lớn có thể làm tăng kích thước request.
          - **PASETO (Platform-Agnostic SEcurity TOkens):**
            - Một chuẩn token mới hơn, nhằm giải quyết một số vấn đề về sự phức tạp và các lỗ hổng tiềm ẩn của JWT (ví dụ: `alg: none` attack).
            - Tập trung vào sự an toàn theo mặc định (secure-by-default).
            - Cung cấp các phiên bản cho cả signed tokens (public) và encrypted tokens (local).
            - Ít phổ biến hơn JWT nhưng đang dần được chú ý.
          - **Opaque Tokens (Reference Tokens):**
            - Là các chuỗi token ngẫu nhiên, không chứa thông tin gì về người dùng.
            - Resource Server phải gọi lại IdP/Auth Service để validate token và lấy thông tin người dùng.
            - _Ưu điểm:_ Dễ thu hồi. An toàn hơn nếu token bị lộ vì nó vô nghĩa nếu không có IdP.
            - _Nhược điểm:_ Stateful (IdP phải lưu trữ mapping token -> user). Thêm một network hop để validate token.

      - **d. OAuth 2.0 và OpenID Connect (OIDC):**
        - **OAuth 2.0:** Là một **framework ủy quyền (authorization framework)**, không phải là một protocol xác thực. Nó cho phép một ứng dụng bên thứ ba (client application) truy cập vào các tài nguyên được bảo vệ của người dùng trên một HTTP service (resource server) mà không cần lộ credentials của người dùng cho client application.
          - Thường sử dụng access tokens.
          - Có nhiều "grant types" (luồng ủy quyền) khác nhau (Authorization Code, Implicit, Client Credentials, Resource Owner Password Credentials).
        - **OpenID Connect (OIDC):** Là một **lớp định danh (identity layer)** được xây dựng trên nền tảng OAuth 2.0. Nó cho phép client xác minh danh tính của người dùng cuối dựa trên quá trình xác thực được thực hiện bởi một Authorization Server, cũng như lấy thông tin profile cơ bản về người dùng cuối một cách có thể tương tác và giống REST.
          - OIDC giới thiệu **ID Token** (một JWT) chứa thông tin xác thực về người dùng.
        - _Khi nào dùng?_
          - Khi bạn cần cho phép các ứng dụng bên thứ ba truy cập API của bạn thay mặt người dùng.
          - Khi bạn muốn sử dụng các Identity Provider (IdP) bên ngoài như Google, Facebook, GitHub, Okta, Auth0 để xác thực người dùng (SSO - Single Sign-On).
          - Cho service-to-service auth với grant type `client_credentials`.
        - OAuth 2.0 và OIDC khá phức tạp, thường bạn sẽ sử dụng các thư viện hoặc IdP hiện có thay vì tự implement toàn bộ.

  3.  **Triển khai Token-based Authentication (JWT) trong Go:**

      - **a. Thư viện JWT cho Go:**
        - `golang-jwt/jwt` (trước đây là `dgrijalva/jwt-go`): Thư viện phổ biến nhất.
        - `lestrrat-go/jwx`: Một thư viện toàn diện hơn cho JOSE (JWT, JWS, JWE, JWK, JWA).
      - **b. Tạo (Issuing) JWT:**

        ```go
        // auth/jwt_service.go
        package auth

        import (
            "fmt"
            "time"
            "github.com/golang-jwt/jwt/v4" // Hoặc v5 tùy phiên bản
        )

        // Secret key (phải được bảo vệ, không hardcode trong production, đọc từ config/env)
        var jwtSecretKey = []byte("your-very-secret-key-that-is-long-and-random") // VÍ DỤ THÔI!

        type UserClaims struct {
            UserID string   `json:"user_id"`
            Roles  []string `json:"roles"`
            jwt.RegisteredClaims // Nhúng các claims chuẩn: Issuer, Subject, Audience, ExpiresAt, NotBefore, IssuedAt, ID
        }

        func GenerateJWT(userID string, roles []string, expirationTime time.Duration) (string, error) {
            claims := UserClaims{
                UserID: userID,
                Roles:  roles,
                RegisteredClaims: jwt.RegisteredClaims{
                    ExpiresAt: jwt.NewNumericDate(time.Now().Add(expirationTime)),
                    IssuedAt:  jwt.NewNumericDate(time.Now()),
                    NotBefore: jwt.NewNumericDate(time.Now()),
                    Issuer:    "helios-auth-service", // Tên service phát hành token
                    Subject:   userID,               // Chủ thể của token
                    // Audience:  []string{"helios-anomaly-service", "helios-dashboard-api"}, // Các service được phép dùng token này
                },
            }

            // Chọn thuật toán ký (HS256 là HMAC với SHA256)
            token := jwt.NewWithClaims(jwt.SigningMethodHS256, claims)

            // Ký token với secret key
            signedToken, err := token.SignedString(jwtSecretKey)
            if err != nil {
                return "", fmt.Errorf("failed to sign token: %w", err)
            }
            return signedToken, nil
        }
        ```

      - **c. Validate (Verifying) JWT và Middleware:**

        - Middleware được đặt trước các HTTP handlers cần bảo vệ.
        - Middleware sẽ trích xuất token từ header `Authorization: Bearer <token>`.
        - Parse và validate token: kiểm tra chữ ký, thời hạn, issuer, audience.
        - Nếu hợp lệ, có thể thêm thông tin user (claims) vào `context.Context` của request để các handler sau có thể sử dụng.

        ```go
        // auth/jwt_middleware.go
        package auth

        import (
            "context"
            "fmt"
            "net/http"
            "strings"
            "github.com/gin-gonic/gin" // Ví dụ với Gin
            "github.com/golang-jwt/jwt/v4"
        )

        type contextKey string
        const UserClaimsKey contextKey = "userClaims"

        func JWTMiddleware() gin.HandlerFunc {
            return func(c *gin.Context) {
                authHeader := c.GetHeader("Authorization")
                if authHeader == "" {
                    c.AbortWithStatusJSON(http.StatusUnauthorized, gin.H{"error": "Authorization header required"})
                    return
                }

                parts := strings.Split(authHeader, " ")
                if len(parts) != 2 || strings.ToLower(parts[0]) != "bearer" {
                    c.AbortWithStatusJSON(http.StatusUnauthorized, gin.H{"error": "Authorization header format must be Bearer {token}"})
                    return
                }
                tokenString := parts[1]

                claims := &UserClaims{}
                token, err := jwt.ParseWithClaims(tokenString, claims, func(token *jwt.Token) (interface{}, error) {
                    // Đảm bảo thuật toán ký là cái bạn mong đợi (tránh alg:none attack)
                    if _, ok := token.Method.(*jwt.SigningMethodHMAC); !ok {
                        return nil, fmt.Errorf("unexpected signing method: %v", token.Header["alg"])
                    }
                    return jwtSecretKey, nil
                })

                if err != nil {
                    if ve, ok := err.(*jwt.ValidationError); ok {
                        if ve.Errors&jwt.ValidationErrorMalformed != 0 {
                            c.AbortWithStatusJSON(http.StatusUnauthorized, gin.H{"error": "Malformed token"})
                        } else if ve.Errors&(jwt.ValidationErrorExpired|jwt.ValidationErrorNotValidYet) != 0 {
                            c.AbortWithStatusJSON(http.StatusUnauthorized, gin.H{"error": "Token is either expired or not active yet"})
                        } else {
                            c.AbortWithStatusJSON(http.StatusUnauthorized, gin.H{"error": "Couldn't handle this token: " + err.Error()})
                        }
                    } else {
                        c.AbortWithStatusJSON(http.StatusUnauthorized, gin.H{"error": "Couldn't handle this token: " + err.Error()})
                    }
                    return
                }

                if !token.Valid {
                    c.AbortWithStatusJSON(http.StatusUnauthorized, gin.H{"error": "Invalid token"})
                    return
                }

                // Token hợp lệ, lưu claims vào context của Gin (hoặc context.Context chuẩn)
                c.Set(string(UserClaimsKey), claims) // Gin context
                // Hoặc với context.Context chuẩn:
                // ctx := context.WithValue(c.Request.Context(), UserClaimsKey, claims)
                // c.Request = c.Request.WithContext(ctx)

                c.Next() // Chuyển sang handler tiếp theo
            }
        }

        // Helper để lấy claims từ context trong handler
        func GetUserClaimsFromContext(ctx context.Context) (*UserClaims, bool) {
            // Nếu dùng Gin context:
            // if ginCtx, ok := ctx.(*gin.Context); ok {
            //     claims, exists := ginCtx.Get(string(UserClaimsKey))
            //     if !exists { return nil, false }
            //     userClaims, ok := claims.(*UserClaims)
            //     return userClaims, ok
            // }

            // Nếu dùng context.Context chuẩn:
            claims, ok := ctx.Value(UserClaimsKey).(*UserClaims)
            return claims, ok
        }
        ```

      - **d. Truyền Token giữa các Microservices:**
        - Khi Service A (đã xác thực user) gọi Service B, Service A có thể:
          1.  **Forward Token:** Truyền token của user gốc sang Service B. Service B sẽ validate lại token đó.
          2.  **Service Account Token:** Service A xác thực với Service B bằng một "service account" riêng (ví dụ: client credentials grant type của OAuth2.0, hoặc một JWT riêng cho service-to-service). Service B tin tưởng Service A và thông tin user ID (nếu cần) có thể được truyền trong header hoặc payload của request từ A sang B.
        - Lựa chọn phụ thuộc vào mức độ tin cậy và yêu cầu bảo mật. Forwarding token gốc đơn giản hơn nhưng có thể lộ token ra nhiều nơi.

  4.  **Chiến lược Authorization (Ủy quyền):**

      - **a. Role-Based Access Control (RBAC):**

        - Người dùng được gán một hoặc nhiều "roles" (vai trò, ví dụ: `admin`, `editor`, `viewer`).
        - Mỗi role được liên kết với một tập hợp các "permissions" (quyền, ví dụ: `read_anomaly`, `delete_sensor`, `configure_rules`).
        - Hệ thống kiểm tra xem role của người dùng có chứa permission cần thiết để thực hiện hành động hay không.
        - _Triển khai:_
          - Thông tin roles có thể được lưu trong claims của JWT.
          - Cần một mapping (trong code hoặc DB) từ roles sang permissions.
          - Middleware hoặc logic trong handler sẽ kiểm tra.

        ```go
        // Ví dụ RBAC check trong Gin handler
        func DeleteSensorHandler(c *gin.Context) {
            claimsInterface, _ := c.Get(string(UserClaimsKey))
            claims, ok := claimsInterface.(*auth.UserClaims)
            if !ok {
                c.JSON(http.StatusInternalServerError, gin.H{"error": "Failed to get user claims"})
                return
            }

            // Giả sử có hàm HasPermission(roles []string, requiredPermission string) bool
            if !auth.HasPermission(claims.Roles, "delete_sensor_data") {
                c.JSON(http.StatusForbidden, gin.H{"error": "You do not have permission to delete sensors"})
                return
            }
            // ... (logic xóa sensor)
        }
        ```

      - **b. Attribute-Based Access Control (ABAC):**

        - Quyết định ủy quyền dựa trên các "attributes" (thuộc tính) của chủ thể (user), tài nguyên (resource), hành động (action), và môi trường (environment).
        - Linh hoạt hơn RBAC, cho phép các quy tắc phức tạp hơn. Ví dụ: "Một user thuộc phòng ban X chỉ được phép xem dữ liệu của các cảm biến thuộc cùng phòng ban X trong giờ làm việc."
        - _Triển khai:_ Phức tạp hơn, thường cần một "policy engine" (công cụ quyết định chính sách) như Open Policy Agent (OPA).

      - **c. Other models:** ACLs (Access Control Lists), Policy-based.

  5.  **Giải pháp Tập trung cho AuthN/AuthZ:**

      - **API Gateway:**
        - Có thể là điểm đầu cuối (entry point) cho tất cả các request.
        - Gateway có thể thực hiện Authentication (validate token) và một số Authorization cơ bản trước khi forward request đến các microservice nội bộ.
        - Các service nội bộ có thể tin tưởng vào thông tin user đã được Gateway xác thực (ví dụ: truyền user ID qua header).
      - **Identity Provider (IdP) / Authorization Server:**
        - Một service chuyên dụng chịu trách nhiệm xác thực người dùng, phát hành token.
        - Ví dụ: Keycloak (mã nguồn mở), Auth0, Okta, Firebase Auth, AWS Cognito.
        - Thường hỗ trợ OAuth 2.0 và OIDC.
        - Giúp tập trung logic AuthN, SSO.
      - **Open Policy Agent (OPA):**
        - Một policy engine mã nguồn mở, đa năng.
        - Bạn định nghĩa các policy bằng ngôn ngữ Rego.
        - Ứng dụng của bạn query OPA (cung cấp input là attributes của user, resource, action) để nhận quyết định (allow/deny).
        - Rất mạnh mẽ cho ABAC và các kịch bản authorization phức tạp. Có thể chạy như một sidecar hoặc tích hợp thư viện.

  6.  **Áp dụng vào Dự án Helios:**
      - **Authentication:**
        - **API của `SensorDataIngestor`:** Có thể dùng API Key cho các "cảm biến" (hoặc gateway của cảm biến) gửi dữ liệu. Hoặc nếu có một "device management service", nó có thể cấp JWT cho từng cảm biến.
        - **API của `DashboardAPI` (cho người dùng quản trị):** Sử dụng JWT. Cần một "Auth Service" (có thể đơn giản ban đầu) để user login và nhận JWT.
        - **Service-to-service communication:** Nếu `SensorDataIngestor` gọi `AnomalyDetectionEngine` qua gRPC, có thể dùng mTLS (Mutual TLS) để xác thực lẫn nhau, hoặc Service A dùng một JWT (client credentials grant) để gọi Service B.
      - **Authorization:**
        - Sử dụng RBAC cho `DashboardAPI`. Ví dụ:
          - Role `admin`: Có thể cấu hình rules, xem mọi anomalies, quản lý sensors.
          - Role `viewer`: Chỉ có thể xem anomalies.
        - Claims `roles` trong JWT sẽ được dùng để kiểm tra.

- **Code minh họa / sơ đồ:**

  - Các ví dụ code JWT đã được cung cấp.
  - **Sơ đồ Luồng JWT AuthN:**
    ```
    User Client ---1. Credentials (user/pass)--> Auth Service (IdP)
        ^                                           |
        |                                           | 2. Validate Credentials
        |                                           V
        |  <--8. Access Resource-- (Token Invalid)  Auth Service
        |  |         ^      (Token Valid) |          (Issues JWT)
        |  |         |                    V
    User Client <--7. Access Resource (Protected)  <--3. JWT--- User Client
        ^                                           |
        |                                           | 4. Store JWT
        |  (Each Subsequent Request)                V
        +---5. Request + JWT (Authorization: Bearer) --> Resource Server (Microservice)
                                                        |
                                                        | 6. Validate JWT (Signature, Expiry)
                                                        |    Extract Claims
                                                        V
                                                    (Process Request if Valid)
    ```

- **Best practices:**

  1.  **Luôn sử dụng HTTPS/TLS cho tất cả các giao tiếp truyền token hoặc credentials.**
  2.  **Giữ secret key (cho JWT HMAC) và private key (cho RSA/ECDSA) an toàn tuyệt đối.** Đọc từ config, không hardcode.
  3.  **Sử dụng thuật toán ký mạnh cho JWT (HS256/512, RS256/512, ES256/512).** Tránh `alg: none`.
  4.  **Đặt thời hạn (expiration) ngắn cho access tokens (ví dụ: 5-60 phút).** Sử dụng refresh tokens (có thời hạn dài hơn, được lưu trữ an toàn hơn) để lấy access token mới.
  5.  **Validate đầy đủ các claims của JWT:** `exp`, `iat`, `nbf`, `iss`, `aud`.
  6.  **Không lưu trữ thông tin quá nhạy cảm trong payload của JWT (trừ khi JWT được mã hóa).**
  7.  **Cân nhắc việc thu hồi JWT:** Nếu cần, sử dụng opaque tokens hoặc duy trì một "blacklist" các JTI (JWT ID) đã bị thu hồi (thêm statefulness).
  8.  **Thực hiện Authorization ở mỗi service cần bảo vệ tài nguyên.** Không chỉ dựa vào Gateway.
  9.  **Ghi log các sự kiện AuthN/AuthZ quan trọng (thành công, thất bại).**
  10. **Sử dụng các thư viện đã được kiểm chứng cho JWT và OAuth 2.0/OIDC.**

- **Anti-patterns / lỗi phổ biến:**

  1.  **Không dùng HTTPS khi truyền credentials hoặc tokens.**
  2.  **Hardcoding secret keys.**
  3.  **Không validate thuật toán ký của JWT (`alg` header).**
  4.  **Thời hạn token quá dài, không có cơ chế refresh.**
  5.  **Lưu trữ thông tin nhạy cảm trong JWT payload không được mã hóa.**
  6.  **Chỉ thực hiện Authentication ở API Gateway và bỏ qua Authorization ở các service nội bộ.**
  7.  **Triển khai OAuth 2.0/OIDC từ đầu mà không hiểu rõ (rất dễ sai).**

- **So sánh các lựa chọn / cách tiếp cận (nếu có):**

  - **JWT vs. Opaque Tokens:** Đã thảo luận. JWT stateless, Opaque stateful.
  - **Tự xây dựng Auth Service vs. Sử dụng IdP bên ngoài:**
    - Tự xây dựng: Toàn quyền kiểm soát, nhưng phức tạp và dễ mắc lỗi bảo mật.
    - IdP bên ngoài (Keycloak, Auth0, etc.): Giảm gánh nặng phát triển, thường an toàn hơn, nhiều tính năng (MFA, SSO). Có thể tốn chi phí hoặc cần học cách tích hợp.

- **Gợi ý mở rộng kiến thức:**
  - **JWT Handbook:** (https://jwt.io/introduction)
  - **OAuth 2.0 RFC 6749:** (https://tools.ietf.org/html/rfc6749)
  - **OpenID Connect Core 1.0:** (https://openid.net/specs/openid-connect-core-1_0.html)
  - **Keycloak Documentation:** (https://www.keycloak.org/documentation)
  - **Open Policy Agent Documentation:** (https://www.openpolicyagent.org/docs/latest/)
  - Các bài viết về "JWT best practices" và "microservice security patterns".
  - **Trích dẫn (Bruce Schneier):** "Security is a process, not a product." (Bảo mật là một quá trình, không phải là một sản phẩm.)

---

AuthN và AuthZ là những nền tảng không thể thiếu để xây dựng các microservices an toàn và đáng tin cậy. Phần 12 này đã cung cấp một cái nhìn tổng quan và các kỹ thuật cụ thể để bạn bắt đầu.
