**Phần 1: Thiết Lập Môi Trường & Nền Tảng C++ Căn Bản ("The C++ Way")**

**1. Môi Trường Phát Triển**

- **Compiler:**
  - **GCC (g++):** Phổ biến trên Linux/macOS. Lệnh: `g++ source.cpp -o program -std=c++17`
  - **Clang:** Tương tự GCC, nổi tiếng với thông báo lỗi rõ ràng. Lệnh: `clang++ source.cpp -o program -std=c++17`
  - **MSVC (Microsoft Visual C++):** Tích hợp trong Visual Studio trên Windows.
  - Chọn phiên bản C++ (flag `-std=`): Nên dùng `c++17` hoặc `c++20` trở lên cho các tính năng hiện đại.
- **IDE (Integrated Development Environment):**
  - **Visual Studio Code (VS Code):** Nhẹ, đa nền tảng, mạnh mẽ với extension C/C++ của Microsoft.
  - **Visual Studio:** IDE đầy đủ tính năng cho Windows, hỗ trợ C++ mạnh mẽ.
  - **CLion (JetBrains):** IDE C++ trả phí, đa nền tảng, nhiều tính năng thông minh.
- **Build Systems:**
  - **Mục đích:** Tự động hóa quá trình biên dịch và liên kết, đặc biệt cho dự án lớn có nhiều tệp.
  - **Make:** Cổ điển, dùng `Makefile`. Phù hợp dự án nhỏ hoặc khi cần kiểm soát chi tiết.
  - **CMake:** Đa nền tảng, tạo file build cho nhiều môi trường (Makefiles, Visual Studio projects, etc.). Tiêu chuẩn công nghiệp.
    - Ví dụ `CMakeLists.txt` đơn giản:
      ```cmake
      cmake_minimum_required(VERSION 3.10)
      project(MyProject)
      set(CMAKE_CXX_STANDARD 17)
      add_executable(MyProgram main.cpp utils.cpp)
      ```

**2. Quy Trình Biên Dịch và Liên Kết (Compilation & Linking)**

1.  **Source Code (`.cpp`, `.h`):** Mã nguồn bạn viết.
2.  **Preprocessing:** Xử lý các chỉ thị tiền xử lý (`#include`, `#define`, `#ifdef`). Kết quả là một "translation unit".
    - `#include <iostream>`: Chèn nội dung của file header `iostream`.
    - `#define PI 3.14159`: Thay thế `PI` bằng `3.14159`.
3.  **Compilation:** Trình biên dịch dịch mã C++ (đã qua tiền xử lý) thành mã assembly, sau đó thành mã máy (object code, file `.o` hoặc `.obj`). Mỗi file `.cpp` thường tạo ra một file object.
4.  **Linking:** Trình liên kết (linker) kết hợp các file object đã biên dịch và các thư viện cần thiết (static hoặc dynamic) thành một file thực thi duy nhất (executable, `.exe` trên Windows, không có đuôi trên Linux/macOS). Nó giải quyết các tham chiếu đến hàm/biến ở các file object khác nhau.

**3. Biến, Kiểu Dữ Liệu Cơ Bản và Ép Kiểu**

- **Kiểu dữ liệu cơ bản (Primitive Types):**
  - Số nguyên: `int`, `short int`, `long int`, `long long int`.
  - Số thực: `float`, `double`, `long double`.
  - Ký tự: `char` (thường là 1 byte, có thể `signed` hoặc `unsigned`). `wchar_t` (cho wide characters).
  - Logic: `bool` (`true`, `false`).
  - Không kiểu: `void` (dùng cho hàm không trả về giá trị hoặc con trỏ generic).
- **Modifiers:** `signed`, `unsigned` (cho kiểu số nguyên).
- **Khai báo và Khởi tạo:**
  ```cpp
  int age = 30;                   // C-style initialization
  double pi {3.14159};            // Uniform initialization (C++11) - ưa dùng
  bool isValid = true;
  char initial {'J'};
  ```
  - **Best Practice:** Luôn khởi tạo biến. Uniform initialization `{}` an toàn hơn (ngăn chặn narrowing conversions).
- **Hằng số (`const`, `constexpr`):**

  ```cpp
  const int MAX_USERS = 100;      // Giá trị không đổi sau khi khởi tạo
  // MAX_USERS = 200;             // Lỗi biên dịch

  constexpr double G_ACCEL = 9.8; // Tính toán tại thời điểm biên dịch (nếu có thể)
  ```

  - **Best Practice:** Dùng `const` cho mọi thứ có thể không thay đổi. Dùng `constexpr` cho các giá trị có thể tính toán tại compile-time để tối ưu.

- **`auto` (Type Deduction):** Trình biên dịch tự suy ra kiểu dữ liệu từ giá trị khởi tạo.

  ```cpp
  auto count = 10;              // count là int
  auto price = 29.99;           // price là double (mặc định)
  auto name = "Alice";          // name là const char*
  auto greeting = std::string{"Hello"}; // greeting là std::string (nếu #include <string>)

  const auto MAX_RETRIES = 3;   // const int
  auto& ref_count = count;      // int&
  const auto& cref_count = count; // const int&
  ```

  - **Best Practice:** Dùng `auto` khi kiểu dữ liệu rõ ràng từ phía phải hoặc quá dài dòng, giúp code dễ đọc hơn. Không lạm dụng khi làm giảm tính rõ ràng.

- **`decltype` (Declare Type):** Lấy kiểu của một biểu thức.
  ```cpp
  int x = 5;
  decltype(x) y = 10;           // y là int
  double z = 3.0;
  decltype(x * z) product;      // product là double (vì x*z là double)
  ```
  - Hữu ích trong generic programming và khi kiểu phụ thuộc vào template parameters.
- **Ép Kiểu (Type Casting):**
  - **C-style cast (tránh dùng):** `(new_type)expression`
    ```cpp
    // int i = 10;
    // double d = (double)i / 3; // Tránh
    ```
  - **C++ style casts (ưu tiên):**
    - `static_cast<new_type>(expression)`: Cho các chuyển đổi an toàn, tương đối rõ ràng (ví dụ: `int` sang `double`, `void*` sang con trỏ cụ thể nếu bạn chắc chắn).
      ```cpp
      int i = 10;
      double d_static = static_cast<double>(i) / 3; // d_static = 3.333...
      char c = 'A';
      int ascii_val = static_cast<int>(c);          // ascii_val = 65
      ```
    - `dynamic_cast<new_type>(expression)`: Dùng cho downcasting an toàn trong kế thừa đa hình (sẽ học sau). Trả về `nullptr` nếu cast thất bại cho con trỏ, ném `std::bad_cast` cho tham chiếu.
    - `reinterpret_cast<new_type>(expression)`: Ép kiểu cấp thấp, coi bit pattern của một kiểu như là của kiểu khác. Rất nguy hiểm, chỉ dùng khi thực sự hiểu rõ.
      ```cpp
      // Ví dụ (cẩn thận):
      // long addr = 0x12345678;
      // int* ptr = reinterpret_cast<int*>(addr); // Coi địa chỉ là con trỏ int
      ```
    - `const_cast<new_type>(expression)`: Bỏ thuộc tính `const` hoặc `volatile`. Thường dùng để gọi hàm non-const trên đối tượng const (nếu bạn biết hàm đó không thực sự thay đổi đối tượng logic). Nguy hiểm nếu dùng sai.
      ```cpp
      // const int val = 10;
      // int* non_const_ptr = const_cast<int*>(&val);
      // *non_const_ptr = 20; // Undefined Behavior nếu val thực sự được lưu ở vùng nhớ read-only
      ```
  - **Best Practice:** Ưu tiên C++ style casts. `static_cast` là phổ biến nhất cho các chuyển đổi an toàn. Hạn chế tối đa `reinterpret_cast` và `const_cast`.

**4. Toán Tử (Operators) và Độ Ưu Tiên**

- **Số học:** `+`, `-`, `*`, `/`, `%` (chia lấy dư).
- **Gán:** `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`.
- **Quan hệ:** `==`, `!=`, `>`, `<`, `>=`, `<=`.
- **Logic:** `&&` (AND), `||` (OR), `!` (NOT).
- **Bitwise:** `&` (AND), `|` (OR), `^` (XOR), `~` (NOT), `<<` (dịch trái), `>>` (dịch phải).
- **Tăng/Giảm:** `++` (tiền tố/hậu tố), `--` (tiền tố/hậu tố).
  ```cpp
  int a = 5;
  int b = ++a; // a = 6, b = 6 (tiền tố)
  int c = a++; // c = 6, a = 7 (hậu tố)
  ```
- **Điều kiện (Ternary):** `condition ? expr_if_true : expr_if_false;`
  ```cpp
  int x = 10, y = 20;
  int max_val = (x > y) ? x : y; // max_val = 20
  ```
- **`sizeof(type_or_expression)`:** Trả về kích thước (byte) của kiểu hoặc biểu thức.
- **Độ ưu tiên:** Toán tử có độ ưu tiên khác nhau (ví dụ `*`, `/` cao hơn `+`, `-`).
  - **Best Practice:** Dùng dấu ngoặc đơn `()` để làm rõ thứ tự thực hiện, ngay cả khi không bắt buộc, giúp code dễ đọc và tránh lỗi.
  ```cpp
  int result = (a + b) * c; // Rõ ràng hơn a + b * c nếu ý đồ là (a+b) trước
  ```

**5. Cấu Trúc Điều Khiển (Control Flow)**

- **`if-else if-else`:**

  ```cpp
  int score = 85;
  if (score >= 90) {
      std::cout << "Grade: A\n";
  } else if (score >= 80) {
      std::cout << "Grade: B\n"; // Output: Grade: B
  } else {
      std::cout << "Grade: C or lower\n";
  }

  // if với initializer (C++17)
  if (int val = some_function(); val > 10) {
      // val chỉ tồn tại trong scope của if/else
      std::cout << "Value is " << val << std::endl;
  }
  ```

- **`switch`:** Dùng cho lựa chọn dựa trên giá trị của một biểu thức nguyên hoặc enum.
  ```cpp
  char grade = 'B';
  switch (grade) {
      case 'A':
          std::cout << "Excellent!\n";
          break; // Quan trọng: thoát khỏi switch
      case 'B':
          std::cout << "Good job!\n"; // Output
          // Fall-through nếu không có break
      case 'C': // Fall-through từ 'B' nếu 'B' không có break
          std::cout << "Passed (B or C).\n"; // Output nếu grade là B (do fall-through) hoặc C
          break;
      default:
          std::cout << "Invalid grade.\n";
  }
  ```
  - **Best Practice:** Luôn có `break` sau mỗi `case` trừ khi bạn cố ý muốn "fall-through". Luôn có `default` case.
  - **C++17 `switch` with initializer:**
    ```cpp
    // switch (DataType data = get_data(); data.type) { ... }
    ```
- **Vòng lặp `while`:** Lặp khi điều kiện đúng.
  ```cpp
  int i = 0;
  while (i < 5) {
      std::cout << i << " "; // Output: 0 1 2 3 4
      i++;
  }
  std::cout << std::endl;
  ```
- **Vòng lặp `do-while`:** Thực thi thân vòng lặp ít nhất một lần, sau đó kiểm tra điều kiện.
  ```cpp
  int j = 0;
  do {
      std::cout << j << " "; // Output: 0 (nếu j khởi tạo là 5, vẫn output 5 một lần)
      j++;
  } while (j < 0); // Điều kiện sai ngay từ đầu, nhưng thân lặp vẫn chạy 1 lần
  std::cout << std::endl;
  ```
- **Vòng lặp `for`:** Cấu trúc lặp cổ điển.
  ```cpp
  for (int k = 0; k < 5; k++) {
      std::cout << k << " "; // Output: 0 1 2 3 4
  }
  std::cout << std::endl;
  ```
- **Range-based `for` loop (C++11):** Duyệt qua các phần tử của một collection (mảng, `std::vector`, `std::string`, etc.).

  ```cpp
  #include <vector>
  #include <string>
  // ... (trong hàm main hoặc một hàm khác)
  int numbers[] = {1, 2, 3, 4, 5};
  for (int num : numbers) {
      std::cout << num << " "; // Output: 1 2 3 4 5
  }
  std::cout << std::endl;

  std::vector<std::string> names = {"Alice", "Bob", "Charlie"};
  for (const auto& name : names) { // Dùng const auto& để tránh copy và cho phép thay đổi (nếu không const)
      std::cout << name << " ";    // Output: Alice Bob Charlie
  }
  std::cout << std::endl;
  ```

  - **Best Practice:** Ưu tiên range-based for khi duyệt toàn bộ collection, code gọn gàng hơn. Dùng `const auto&` cho phần tử để tránh copy không cần thiết và cho phép đọc, hoặc `auto&` nếu cần sửa đổi phần tử.

**6. Best Practices & Lưu Ý Chung (Phần 1)**

- **Đặt tên (Naming Conventions):**
  - Không có chuẩn tuyệt đối, nhưng nhất quán trong dự án là quan trọng.
  - Biến, hàm: `snake_case` (ví dụ: `user_count`) hoặc `camelCase` (ví dụ: `userCount`).
  - Lớp, struct: `PascalCase` (ví dụ: `UserProfile`).
  - Hằng số: `UPPER_SNAKE_CASE` (ví dụ: `MAX_CONNECTIONS`).
- **`const` Correctness:** Sử dụng `const` rộng rãi cho biến, tham số hàm, hàm thành viên (sẽ học sau) không thay đổi trạng thái. Giúp trình biên dịch tối ưu và tăng tính an toàn.
- **Tránh Magic Numbers:** Dùng hằng số có tên thay vì các giá trị số trực tiếp trong code.

  ```cpp
  // Bad
  // if (status == 2) { /* ... */ }

  // Good
  const int STATUS_ACTIVE = 2;
  if (status == STATUS_ACTIVE) { /* ... */ }
  ```

- **Khởi tạo biến:** Luôn khởi tạo biến trước khi sử dụng để tránh hành vi không xác định (undefined behavior) do giá trị rác. Ưu tiên uniform initialization `{}`.
- **Sử dụng C++ Casts:** Tránh C-style cast.
- **Includes:** Chỉ `#include` những gì cần thiết. Dùng include guards trong file header (sẽ chi tiết hơn).
- **Comment:** Viết comment giải thích những đoạn code phức tạp hoặc "tại sao" chứ không phải "làm gì" (nếu code đã rõ ràng).

OK. Tiếp tục với chiều sâu và chi tiết bạn yêu cầu.

**Phần 2: Hàm (Functions) & Tổ Chức Mã Nguồn**

**1. Khai Báo và Định Nghĩa Hàm (Function Declaration and Definition)**

- **Khai báo (Declaration/Prototype):** Cung cấp cho trình biên dịch thông tin về tên hàm, kiểu giá trị trả về, và kiểu của các tham số. Không chứa thân hàm.
  - Mục đích: Cho phép gọi hàm trước khi nó được định nghĩa đầy đủ (ví dụ, hàm được định nghĩa ở file khác hoặc ở cuối file hiện tại).
  - Cú pháp: `return_type function_name(parameter_type1 param_name1, parameter_type2 param_name2, ...);`
    ```cpp
    // Khai báo hàm (prototypes)
    int add(int a, int b); // Tên tham số (a, b) là tùy chọn trong khai báo
    void printMessage(const char* message);
    double calculateAverage(double x, double y);
    ```
- **Định nghĩa (Definition):** Cung cấp phần thực thi (thân hàm) của hàm. Bao gồm cả khai báo. Một hàm chỉ có thể được định nghĩa một lần trong toàn bộ chương trình (One Definition Rule - ODR).

  - Cú pháp:
    ```cpp
    return_type function_name(parameter_type1 param_name1, ...) {
        // Thân hàm - các câu lệnh
        // return value; (nếu return_type không phải void)
    }
    ```
  - Ví dụ định nghĩa:

    ```cpp
    #include <iostream> // Cho std::cout

    // Định nghĩa hàm add
    int add(int a, int b) {
        return a + b;
    }

    // Định nghĩa hàm printMessage
    void printMessage(const char* message) {
        if (message) { // Kiểm tra con trỏ khác null
            std::cout << message << std::endl;
        }
    }

    // Định nghĩa hàm tính trung bình
    double calculateAverage(double x, double y) {
        return (x + y) / 2.0; // Dùng 2.0 để đảm bảo phép chia số thực
    }

    int main() {
        int sum = add(5, 3); // Gọi hàm add
        printMessage("Hello from function!"); // Gọi hàm printMessage
        double avg = calculateAverage(10.5, 20.5);
        std::cout << "Sum: " << sum << ", Average: " << avg << std::endl;
        return 0;
    }
    ```

- **Function Signature:** Phần của khai báo hàm bao gồm tên hàm và danh sách kiểu tham số (và `const`/`volatile` qualifiers của hàm thành viên, sẽ học sau). Kiểu trả về không phải là một phần của signature cho mục đích nạp chồng hàm.

**2. Truyền Tham Số (Parameter Passing)**

- **Truyền bằng giá trị (Pass-by-Value):**

  - Một bản sao của đối số được tạo ra và truyền vào hàm. Mọi thay đổi đối với tham số bên trong hàm không ảnh hưởng đến đối số gốc.
  - Mặc định cho các kiểu cơ bản và đối tượng nếu không có `&` hoặc `*`.
  - **Ưu điểm:** An toàn, đối số gốc không bị thay đổi ngoài ý muốn.
  - **Nhược điểm:** Tốn kém chi phí copy nếu đối tượng lớn.
  - Ví dụ:

    ```cpp
    #include <iostream>
    #include <string>

    void modifyValue(int val, std::string str) {
        val = 100; // Thay đổi bản sao của 'x'
        str = "Modified"; // Thay đổi bản sao của 's'
        std::cout << "Inside modifyValue: val = " << val << ", str = " << str << std::endl;
    }

    int main() {
        int x = 10;
        std::string s = "Original";
        std::cout << "Before call: x = " << x << ", s = " << s << std::endl;
        modifyValue(x, s);
        std::cout << "After call: x = " << x << ", s = " << s << std::endl; // x và s không đổi
        return 0;
    }
    // Output:
    // Before call: x = 10, s = Original
    // Inside modifyValue: val = 100, str = Modified
    // After call: x = 10, s = Original
    ```

- **Truyền bằng tham chiếu (Pass-by-Reference - dùng `&`):**

  - Tham số trở thành một _bí danh_ (alias) của đối số gốc. Mọi thay đổi đối với tham số bên trong hàm sẽ ảnh hưởng trực tiếp đến đối số gốc.
  - Hiệu quả hơn pass-by-value cho các đối tượng lớn vì không có sự sao chép.
  - Cú pháp: `return_type function_name(type& param_name, ...)`
  - **`const` reference (`const type&`):** Tham chiếu hằng. Cho phép hàm truy cập đối số gốc để đọc nhưng không cho phép thay đổi. Rất phổ biến để truyền các đối tượng lớn một cách hiệu quả mà không sợ bị thay đổi.

    ```cpp
    #include <iostream>
    #include <string>
    #include <vector>

    void modifyReference(int& ref_val, std::string& ref_str) {
        ref_val = 200;    // Thay đổi 'y' gốc
        ref_str += " World"; // Thay đổi 'msg' gốc
    }

    void printVector(const std::vector<int>& vec) { // Tham chiếu hằng, không copy, không sửa
        // vec.push_back(10); // Lỗi: không thể sửa đổi qua const reference
        std::cout << "Vector elements: ";
        for (int val : vec) {
            std::cout << val << " ";
        }
        std::cout << std::endl;
    }

    int main() {
        int y = 20;
        std::string msg = "Hello";
        std::cout << "Before modifyReference: y = " << y << ", msg = " << msg << std::endl;
        modifyReference(y, msg);
        std::cout << "After modifyReference: y = " << y << ", msg = " << msg << std::endl;

        std::vector<int> data = {1, 2, 3};
        printVector(data); // Truyền hiệu quả, data không bị thay đổi
        return 0;
    }
    // Output:
    // Before modifyReference: y = 20, msg = Hello
    // After modifyReference: y = 200, msg = Hello World
    // Vector elements: 1 2 3
    ```

  - **Bad Practice:** Trả về tham chiếu đến biến cục bộ (xem mục Giá Trị Trả Về).

- **Truyền bằng con trỏ (Pass-by-Pointer - dùng `*`):**

  - Địa chỉ của đối số được truyền vào hàm. Hàm có thể thay đổi giá trị tại địa chỉ đó (dereference con trỏ).
  - Cú pháp: `return_type function_name(type* ptr_name, ...)`
  - Cần kiểm tra `nullptr` trước khi dereference.
  - Có thể truyền `nullptr` để biểu thị "không có giá trị" hoặc "tùy chọn".

    ```cpp
    #include <iostream>

    void modifyPointer(int* ptr_val) {
        if (ptr_val != nullptr) { // Luôn kiểm tra con trỏ
            *ptr_val = 300;   // Thay đổi 'z' gốc
        }
    }

    // Hàm có thể nhận con trỏ null (tham số tùy chọn)
    void processOptionalData(const int* data, int default_value) {
        if (data != nullptr) {
            std::cout << "Processing data: " << *data << std::endl;
        } else {
            std::cout << "Processing with default: " << default_value << std::endl;
        }
    }

    int main() {
        int z = 30;
        std::cout << "Before modifyPointer: z = " << z << std::endl;
        modifyPointer(&z); // Truyền địa chỉ của z
        std::cout << "After modifyPointer: z = " << z << std::endl;

        int value = 42;
        processOptionalData(&value, 0);
        processOptionalData(nullptr, 0); // Truyền nullptr
        return 0;
    }
    // Output:
    // Before modifyPointer: z = 30
    // After modifyPointer: z = 300
    // Processing data: 42
    // Processing with default: 0
    ```

  - **So sánh tham chiếu và con trỏ:**
    - Tham chiếu phải được khởi tạo khi khai báo và không thể "re-seat" (trỏ đến đối tượng khác). Con trỏ có thể không khởi tạo (nguy hiểm!), có thể là `nullptr`, và có thể trỏ đến các đối tượng khác nhau.
    - Tham chiếu dùng cú pháp truy cập thành viên như đối tượng thường (`ref.member`), con trỏ dùng `->` (`ptr->member`) hoặc `(*ptr).member`.
    - **Best Practice:** Ưu tiên tham chiếu hơn con trỏ khi có thể, đặc biệt là `const&` để truyền đối tượng lớn mà không cần sửa đổi. Dùng con trỏ khi cần khả năng `nullptr` hoặc khi cần "re-seat".

**3. Giá Trị Trả Về (Return Values)**

- **Trả về bằng giá trị (Return-by-Value):**

  - Một bản sao của giá trị được tạo và trả về cho nơi gọi.
  - **RVO (Return Value Optimization) / NRVO (Named Return Value Optimization):** Các trình biên dịch hiện đại thường tối ưu hóa việc copy này, có thể tạo đối tượng trực tiếp tại vị trí của biến nhận kết quả (elision of copy).

    ```cpp
    std::string createGreeting(const std::string& name) {
        std::string greeting = "Hello, " + name + "!";
        return greeting; // NRVO có thể xảy ra ở đây
    }

    int main() {
        std::string my_greeting = createGreeting("Alice"); // greeting có thể được xây dựng trực tiếp vào my_greeting
        std::cout << my_greeting << std::endl;
        return 0;
    }
    ```

- **Trả về bằng tham chiếu (Return-by-Reference - `type&`):**

  - Hàm trả về một bí danh đến một đối tượng đã tồn tại.
  - **CỰC KỲ NGUY HIỂM nếu trả về tham chiếu đến biến cục bộ của hàm** vì biến đó sẽ bị hủy khi hàm kết thúc, tham chiếu trở thành "dangling".

    ```cpp
    // BAD PRACTICE - DANGLING REFERENCE
    // int& getLocalValue() {
    //     int local_var = 10;
    //     return local_var; // LỖI: local_var bị hủy, tham chiếu trả về là vô nghĩa
    // }

    // Ví dụ hợp lệ: trả về thành viên của đối tượng có lifetime dài hơn, hoặc static local
    int global_data = 42;
    int& getGlobalDataRef() {
        return global_data; // OK, global_data tồn tại
    }

    class NumberWrapper {
    public:
        int value;
        NumberWrapper(int v) : value(v) {}
        int& getValueRef() { return value; } // OK, trả về tham chiếu đến thành viên
    };

    int main() {
        int& g_ref = getGlobalDataRef();
        g_ref = 100; // Thay đổi global_data
        std::cout << "Global data: " << global_data << std::endl; // Output: 100

        NumberWrapper nw(50);
        int& val_ref = nw.getValueRef();
        val_ref = 500; // nw.value giờ là 500
        std::cout << "NW value: " << nw.value << std::endl; // Output: 500
        return 0;
    }
    ```

- **Trả về bằng con trỏ (Return-by-Pointer - `type*`):**

  - Hàm trả về địa chỉ của một đối tượng.
  - Cũng **NGUY HIỂM** nếu trả về con trỏ đến biến cục bộ.
  - Thường dùng khi đối tượng được cấp phát động (`new`) bên trong hàm và quyền sở hữu (trách nhiệm `delete`) được chuyển cho nơi gọi.

    ```cpp
    // BAD PRACTICE - DANGLING POINTER
    // int* getLocalPointer() {
    //     int local_var = 20;
    //     return &local_var; // LỖI: local_var bị hủy, con trỏ trả về là vô nghĩa
    // }

    // Ví dụ: trả về đối tượng cấp phát động
    int* createDynamicInt(int val) {
        int* ptr = new int(val);
        return ptr; // Nơi gọi chịu trách nhiệm delete
    }

    int main() {
        int* my_dynamic_int = createDynamicInt(77);
        if (my_dynamic_int) {
            std::cout << "Dynamic int: " << *my_dynamic_int << std::endl;
            delete my_dynamic_int; // QUAN TRỌNG: giải phóng bộ nhớ
            my_dynamic_int = nullptr; // Good practice: gán null sau delete
        }
        return 0;
    }
    ```

  - **Best Practice:** Ưu tiên trả về bằng giá trị (RVO/NRVO giúp hiệu quả). Nếu cần trả về đối tượng lớn được cấp phát động, hãy cân nhắc sử dụng Smart Pointers (sẽ học sau) để quản lý bộ nhớ tự động.

- **Hàm `void`:** Hàm không trả về giá trị nào.
  ```cpp
  void logMessage(const std::string& msg) {
      std::cerr << "[LOG]: " << msg << std::endl; // Ghi ra standard error
      // Không có lệnh return value
  }
  ```

**4. Nạp Chồng Hàm (Function Overloading)**

- Cho phép định nghĩa nhiều hàm cùng tên nhưng khác nhau về danh sách tham số (số lượng, kiểu, hoặc thứ tự của tham số).
- Trình biên dịch chọn hàm phù hợp nhất dựa trên các đối số được truyền khi gọi hàm (quá trình này gọi là "overload resolution").
- Kiểu trả về _không_ được dùng để phân biệt các hàm nạp chồng.

  ```cpp
  #include <iostream>
  #include <string>

  void display(int i) {
      std::cout << "Displaying int: " << i << std::endl;
  }

  void display(double d) {
      std::cout << "Displaying double: " << d << std::endl;
  }

  void display(const std::string& s) {
      std::cout << "Displaying string: " << s << std::endl;
  }

  void display(int i, double d) {
      std::cout << "Displaying int: " << i << " and double: " << d << std::endl;
  }

  // LỖI: không thể nạp chồng chỉ dựa trên kiểu trả về
  // std::string display(const std::string& s) { /* ... */ }

  int main() {
      display(10);                     // Gọi display(int)
      display(3.14);                   // Gọi display(double)
      display("Hello");                // Gọi display(const std::string&) - "Hello" được ngầm chuyển thành std::string
      display(5, 2.71);                // Gọi display(int, double)
      return 0;
  }
  ```

- **Best Practice:** Sử dụng nạp chồng khi các hàm thực hiện logic tương tự trên các kiểu dữ liệu khác nhau. Tránh nạp chồng gây mơ hồ (ambiguity) cho trình biên dịch.

**5. Tham Số Mặc Định (Default Arguments)**

- Cho phép gán giá trị mặc định cho một hoặc nhiều tham số cuối cùng trong danh sách tham số của hàm.
- Nếu đối số không được truyền cho các tham số này khi gọi hàm, giá trị mặc định sẽ được sử dụng.
- Các tham số có giá trị mặc định phải được đặt ở cuối danh sách tham số.
- Giá trị mặc định chỉ cần khai báo một lần, thường trong khai báo hàm (prototype) ở file header.

  ```cpp
  #include <iostream>
  #include <string>

  // Khai báo với tham số mặc định
  void setupConnection(const std::string& host, int port = 8080, int timeout_ms = 5000);

  // Định nghĩa
  void setupConnection(const std::string& host, int port, int timeout_ms) {
      std::cout << "Connecting to " << host
                << " on port " << port
                << " with timeout " << timeout_ms << "ms." << std::endl;
  }

  int main() {
      setupConnection("example.com"); // port=8080, timeout_ms=5000
      setupConnection("api.example.com", 443); // timeout_ms=5000
      setupConnection("test.server", 1234, 1000); // Tất cả đối số được cung cấp
      return 0;
  }
  // Output:
  // Connecting to example.com on port 8080 with timeout 5000ms.
  // Connecting to api.example.com on port 443 with timeout 5000ms.
  // Connecting to test.server on port 1234 with timeout 1000ms.
  ```

- **Best Practice:** Sử dụng cho các tham số thực sự tùy chọn và có giá trị mặc định hợp lý, phổ biến.

**6. Hàm `inline`**

- Từ khóa `inline` là một _gợi ý_ cho trình biên dịch rằng hàm này nên được "inline expansion" (thay thế lời gọi hàm bằng chính mã của hàm đó) tại điểm gọi.
- **Mục đích:** Giảm chi phí của một lời gọi hàm (function call overhead), đặc biệt hữu ích cho các hàm rất nhỏ và được gọi thường xuyên.
- **Không phải là một mệnh lệnh:** Trình biên dịch có thể bỏ qua gợi ý `inline` (ví dụ, nếu hàm quá lớn, đệ quy phức tạp). Trình biên dịch cũng có thể tự động inline các hàm không được đánh dấu `inline` nếu thấy phù hợp.
- **Định nghĩa hàm `inline`:** Thường được đặt trong file header. Nếu một hàm `inline` được định nghĩa trong file header và header đó được `#include` vào nhiều file `.cpp`, trình biên dịch và linker cần xử lý để không vi phạm One Definition Rule (ODR). Các hàm thành viên của lớp được định nghĩa bên trong khai báo lớp là `inline` ngầm định.

  ```cpp
  // Trong file "utils.h"
  #ifndef UTILS_H
  #define UTILS_H

  inline int max_inline(int a, int b) { // Gợi ý inline
      return (a > b) ? a : b;
  }

  class MyMath {
  public:
      // Hàm thành viên định nghĩa trong class là inline ngầm định
      int min_val(int a, int b) { return (a < b) ? a : b; }
  };

  #endif // UTILS_H

  // Trong file "main.cpp"
  // #include "utils.h"
  // #include <iostream>
  // int main() {
  //     std::cout << "Max: " << max_inline(10, 20) << std::endl; // Có thể được inline
  //     MyMath m;
  //     std::cout << "Min: " << m.min_val(5, 3) << std::endl; // Có thể được inline
  //     return 0;
  // }
  ```

- **Nhược điểm:** Nếu lạm dụng cho các hàm lớn, có thể làm tăng kích thước mã thực thi (code bloat), ảnh hưởng đến cache hiệu năng.
- **Best Practice:** Chỉ dùng cho các hàm thực sự nhỏ, thường là một vài dòng code. Tin tưởng vào khả năng tối ưu của trình biên dịch hiện đại.

**7. Namespaces**

- **Mục đích:** Cung cấp một phạm vi (scope) để tránh xung đột tên giữa các đoạn mã từ các thư viện khác nhau hoặc các phần khác nhau của một dự án lớn.
- Cú pháp:

  ```cpp
  namespace MyCustomLibrary {
      void func() { /* ... */ }
      class Widget { /* ... */ };
      int count = 0;
  }

  namespace AnotherLibrary {
      void func() { /* ... */ } // Không xung đột với MyCustomLibrary::func
      const double PI = 3.14159;
  }
  ```

- **Truy cập thành viên namespace:**

  - **Toán tử phân giải phạm vi `::`**:
    ```cpp
    MyCustomLibrary::func();
    MyCustomLibrary::Widget w;
    AnotherLibrary::func();
    double val = AnotherLibrary::PI;
    ```
  - **Khai báo `using` (Using Declaration):** Mang một tên cụ thể từ namespace vào scope hiện tại.

    ```cpp
    #include <iostream>
    namespace Graphics {
        void drawCircle() { std::cout << "Drawing circle\n"; }
    }

    int main() {
        using Graphics::drawCircle; // Mang drawCircle vào scope của main
        drawCircle(); // Có thể gọi trực tiếp
        // Graphics::drawSquare(); // Lỗi: drawSquare chưa được mang vào
        return 0;
    }
    ```

    - **Best Practice:** An toàn để sử dụng trong file `.cpp` hoặc trong scope cục bộ của hàm.

  - **Chỉ thị `using namespace` (Using Directive):** Mang tất cả các tên từ một namespace vào scope hiện tại.

    ```cpp
    #include <iostream> // std::cout, std::endl
    // using namespace std; // BAD PRACTICE ở global scope, đặc biệt trong header

    namespace MyStuff {
        int value = 10;
        void printValue() { std::cout << value << std::endl; }
    }

    void some_function() {
        using namespace MyStuff; // OK trong scope hàm
        std::cout << value << std::endl; // value là MyStuff::value
        printValue();
    }

    int main() {
        // MyStuff::printValue(); // Cách tốt nếu không có using directive
        some_function();
        return 0;
    }
    ```

    - **BAD PRACTICE:** Tránh `using namespace SomeNamespace;` ở global scope trong file header. Nó "pollute" global namespace cho tất cả các file `#include` header đó, có thể gây xung đột tên khó lường.

- **Nested Namespaces:**

  ```cpp
  namespace Outer {
      namespace Inner {
          void foo() { /* ... */ }
      }
  }
  // Gọi: Outer::Inner::foo();

  // C++17 nested namespace definition:
  namespace Outer::Inner::EvenDeeper {
      void bar() { /* ... */ }
  }
  // Gọi: Outer::Inner::EvenDeeper::bar();
  ```

- **Unnamed/Anonymous Namespaces:**

  - Cung cấp internal linkage cho các khai báo bên trong nó. Tương tự như dùng `static` cho biến/hàm toàn cục trong C.
  - Mỗi translation unit (file `.cpp`) có anonymous namespace riêng.

  ```cpp
  // Trong file helper.cpp
  namespace { // Anonymous namespace
      int internal_counter = 0; // Chỉ truy cập được trong helper.cpp
      void helper_internal_func() { internal_counter++; }
  }

  void public_helper_func() {
      helper_internal_func();
      // ...
  }
  ```

- **Namespace `std`:** Tất cả các thành phần của Thư viện Chuẩn C++ (STL) đều nằm trong namespace `std` (ví dụ: `std::cout`, `std::vector`, `std::string`).

**8. Tệp Header (`.h`, `.hpp`) và Tệp Nguồn (`.cpp`), Include Guards**

- **Mục đích phân chia:**
  - **Tệp Header (`.h`, `.hpp`):** Chứa các _khai báo_.
    - Khai báo hàm (prototypes).
    - Định nghĩa lớp (class definitions).
    - Khai báo `extern` cho biến toàn cục (biến được định nghĩa ở file `.cpp` khác).
    - Định nghĩa hàm `inline`.
    - Định nghĩa templates (sẽ học sau).
    - Khai báo `enum`, `struct` (mà không phải định nghĩa hàm thành viên).
    - `#define` macros (cẩn thận khi dùng).
  - **Tệp Nguồn (`.cpp`, `.cxx`, `.cc`):** Chứa các _định nghĩa_.
    - Định nghĩa các hàm non-inline.
    - Định nghĩa các hàm thành viên của lớp (nếu không inline trong header).
    - Định nghĩa biến toàn cục (không `extern`).
- **`#include`:** Chỉ thị tiền xử lý để chèn nội dung của một file (thường là header) vào file hiện tại.
  - `#include <filename>`: Tìm file trong các thư mục include chuẩn của trình biên dịch. Dùng cho thư viện hệ thống/chuẩn.
  - `#include "filename"`: Tìm file trong thư mục hiện tại (hoặc thư mục do người dùng chỉ định), sau đó mới tìm trong thư mục include chuẩn. Dùng cho header của dự án.
- **Include Guards (Header Guards):** Cơ chế để ngăn chặn nội dung của một file header bị include nhiều lần vào cùng một translation unit (file `.cpp`), điều này có thể gây lỗi biên dịch do định nghĩa lại.

  - Cú pháp phổ biến:

    ```cpp
    // Trong file "my_header.h"
    #ifndef MY_HEADER_H  // Nếu MY_HEADER_H chưa được định nghĩa
    #define MY_HEADER_H  // Thì định nghĩa MY_HEADER_H

    // Nội dung của header file ở đây
    // class MyClass { ... };
    // void myFunction(int);

    #endif // MY_HEADER_H
    ```

    - `MY_HEADER_H` là một macro duy nhất, thường dựa trên tên file hoặc namespace để tránh xung đột.

  - **`#pragma once`:** Một chỉ thị không chuẩn nhưng được hỗ trợ rộng rãi bởi nhiều trình biên dịch. Có tác dụng tương tự include guards nhưng đơn giản hơn.

    ```cpp
    // Trong file "another_header.h"
    #pragma once

    // Nội dung của header file ở đây
    ```

    - **Ưu điểm:** Ngắn gọn. Có thể hiệu quả hơn về thời gian biên dịch trong một số trường hợp vì compiler không cần mở lại file để kiểm tra macro.
    - **Nhược điểm:** Không phải là một phần của chuẩn C++, tính portable có thể là vấn đề (dù hiếm gặp với compiler hiện đại).

  - **Best Practice:** Sử dụng include guards truyền thống (`#ifndef/#define/#endif`) để đảm bảo tính tương thích tối đa. Có thể dùng `#pragma once` nếu dự án chỉ nhắm đến các compiler hỗ trợ nó.

- **Best Practices cho Header:**

  - **Include What You Use (IWYU):** Mỗi file (cả header và source) chỉ nên `#include` những header mà nó trực tiếp sử dụng các khai báo từ đó.
  - **Minimize Includes in Headers:** Cố gắng giảm số lượng `#include` trong file header. Nếu chỉ cần biết sự tồn tại của một kiểu (ví dụ, làm tham số con trỏ hoặc tham chiếu cho hàm), hãy dùng **Forward Declaration** thay vì `#include` toàn bộ header định nghĩa kiểu đó.

    ```cpp
    // Trong "user_profile.h"
    #ifndef USER_PROFILE_H
    #define USER_PROFILE_H

    // #include "address.h" // Tránh nếu có thể, dùng forward declaration
    class Address; // Forward declaration của class Address

    class UserProfile {
    public:
        void setPrimaryAddress(Address* addr); // OK với forward declaration
        // Address getAddressCopy(); // Cần #include "address.h" vì trả về đối tượng Address
    private:
        Address* primaryAddress; // OK với forward declaration
        // Address homeAddress;    // Cần #include "address.h" vì là member object
    };
    #endif
    ```

    Điều này giúp giảm thời gian biên dịch và giảm sự phụ thuộc giữa các module.

**9. Ví dụ & Best Practices Tổng Hợp (Phần 2)**

- **Hàm nên ngắn gọn, dễ hiểu và thực hiện một nhiệm vụ duy nhất (Single Responsibility Principle - SRP).**
- **Tên hàm và tham số rõ ràng, thể hiện mục đích.**
- **Sử dụng `const` triệt để:**
  - `const type& param` cho tham số đầu vào là đối tượng lớn, không cần sửa đổi.
  - `const type* param` cho tham số con trỏ đầu vào, không sửa đổi dữ liệu trỏ tới.
  - `return_type function_name(...) const;` cho hàm thành viên không thay đổi trạng thái đối tượng (sẽ học ở phần OOP).
- **Cẩn thận khi trả về tham chiếu hoặc con trỏ:** Đảm bảo chúng không trỏ đến đối tượng cục bộ sắp bị hủy.
- **Ưu tiên truyền bằng `const&` cho các đối tượng phức tạp để tránh copy không cần thiết.** Truyền bằng giá trị khi bạn thực sự cần một bản sao hoặc đối tượng nhỏ.
- **Sử dụng namespaces để tổ chức code và tránh xung đột tên.** Không `using namespace std;` ở global scope trong header.
- **Tổ chức code vào các file header (.h/.hpp) và source (.cpp) một cách hợp lý.** Header chứa khai báo, source chứa định nghĩa.
- **Luôn sử dụng include guards hoặc `#pragma once` trong các file header.**
- **Cân nhắc sử dụng tham số mặc định cho các tùy chọn thực sự không bắt buộc.**
- **`inline` chỉ nên là gợi ý cho các hàm rất nhỏ, trình biên dịch hiện đại tối ưu rất tốt.**

Hết Phần 2.

OK, tiếp tục với Phần 3, tập trung vào con trỏ và quản lý bộ nhớ kiểu C. Đây là một phần nền tảng quan trọng, nhưng cũng là nguồn gốc của nhiều lỗi nếu không cẩn thận.

**Phần 3: Con Trỏ (Pointers) và Quản Lý Bộ Nhớ (C-style)**

**1. Khái Niệm Con Trỏ, Địa Chỉ Bộ Nhớ**

- **Bộ nhớ (Memory):** Có thể hình dung như một dãy các ô nhớ liên tiếp, mỗi ô có một địa chỉ duy nhất.
- **Biến (Variable):** Một tên gọi cho một vùng nhớ chứa dữ liệu.
- **Con trỏ (Pointer):** Là một biến đặc biệt, giá trị của nó _không phải là dữ liệu_ mà là _địa chỉ của một ô nhớ khác_. Con trỏ "trỏ tới" một vị trí trong bộ nhớ.
- Kích thước của con trỏ phụ thuộc vào kiến trúc hệ thống (ví dụ, 4 bytes trên hệ thống 32-bit, 8 bytes trên hệ thống 64-bit), không phụ thuộc vào kiểu dữ liệu mà nó trỏ tới.
- **Kiểu của con trỏ:** Quan trọng. Một con trỏ kiểu `int*` chỉ nên trỏ tới một vùng nhớ chứa dữ liệu kiểu `int`. Trình biên dịch sử dụng kiểu này để biết cách diễn giải dữ liệu tại địa chỉ đó và để thực hiện pointer arithmetic.

**2. Toán Tử `*` (Dereference) và `&` (Address-of)**

- **Toán tử địa chỉ `&` (Address-of Operator):**

  - Khi đặt trước một biến, `&variable_name` trả về địa chỉ bộ nhớ của biến đó.
  - Kết quả của `&` có thể gán cho một con trỏ có kiểu tương ứng.

  ```cpp
  #include <iostream>

  int main() {
      int score = 100;
      double price = 29.99;

      // Lấy địa chỉ của biến
      std::cout << "Address of score: " << &score << std::endl;   // In ra địa chỉ dạng hexa
      std::cout << "Address of price: " << &price << std::endl;

      // Khai báo con trỏ và gán địa chỉ
      int* ptr_score;         // Khai báo con trỏ kiểu int* (trỏ tới int)
      ptr_score = &score;     // ptr_score bây giờ chứa địa chỉ của score

      double* ptr_price = &price; // Khai báo và khởi tạo con trỏ trong một dòng

      std::cout << "Value of ptr_score (address it holds): " << ptr_score << std::endl;
      std::cout << "Value of ptr_price (address it holds): " << ptr_price << std::endl;

      return 0;
  }
  ```

- **Toán tử dereference (truy xuất nội dung) `*` (Indirection/Dereference Operator):**

  - Khi đặt trước một biến con trỏ (đã được khởi tạo hợp lệ và không phải `nullptr`), `*pointer_name` truy cập vào _giá trị_ được lưu trữ tại địa chỉ mà con trỏ đó đang trỏ tới.
  - Có thể dùng để đọc hoặc ghi giá trị tại địa chỉ đó.

  ```cpp
  #include <iostream>

  int main() {
      int value = 42;
      int* ptr_value = &value; // ptr_value trỏ tới value

      // Đọc giá trị thông qua con trỏ
      std::cout << "Value at address " << ptr_value << " is " << *ptr_value << std::endl; // Output: 42

      // Thay đổi giá trị thông qua con trỏ
      *ptr_value = 99; // Giá trị của 'value' sẽ bị thay đổi
      std::cout << "New value of 'value': " << value << std::endl; // Output: 99
      std::cout << "New value via *ptr_value: " << *ptr_value << std::endl; // Output: 99

      // Con trỏ tới con trỏ (pointer to pointer)
      int** ptr_to_ptr_value = &ptr_value; // ptr_to_ptr_value trỏ tới ptr_value
      std::cout << "Address of ptr_value: " << &ptr_value << std::endl;
      std::cout << "Value of ptr_to_ptr_value (address of ptr_value): " << ptr_to_ptr_value << std::endl;
      std::cout << "Value *ptr_to_ptr_value (address held by ptr_value): " << *ptr_to_ptr_value << std::endl;
      std::cout << "Value **ptr_to_ptr_value (value of 'value'): " << **ptr_to_ptr_value << std::endl; // Output: 99

      return 0;
  }
  ```

  - **Bad Practice:** Dereference một con trỏ chưa được khởi tạo hoặc một con trỏ `nullptr` sẽ dẫn đến **Undefined Behavior (UB)**, thường là crash chương trình (segmentation fault).

**3. Con Trỏ và Mảng (Pointers and Arrays)**

- Trong C++, tên của một mảng (khi không đi kèm toán tử `[]` hoặc `sizeof`) thường phân rã (decays) thành một con trỏ trỏ tới phần tử đầu tiên của mảng đó.

  ```cpp
  #include <iostream>

  int main() {
      int numbers[] = {10, 20, 30, 40, 50};

      // Tên mảng 'numbers' phân rã thành con trỏ tới numbers[0]
      int* ptr_numbers = numbers; // Tương đương với int* ptr_numbers = &numbers[0];

      std::cout << "Address of numbers[0]: " << &numbers[0] << std::endl;
      std::cout << "Value of ptr_numbers: " << ptr_numbers << std::endl;
      std::cout << "Value at *ptr_numbers (numbers[0]): " << *ptr_numbers << std::endl; // Output: 10

      // Truy cập các phần tử khác bằng pointer arithmetic
      std::cout << "Value at *(ptr_numbers + 1) (numbers[1]): " << *(ptr_numbers + 1) << std::endl; // Output: 20
      std::cout << "Value at *(ptr_numbers + 2) (numbers[2]): " << *(ptr_numbers + 2) << std::endl; // Output: 30

      // Con trỏ cũng có thể dùng cú pháp mảng
      std::cout << "Value ptr_numbers[1] (numbers[1]): " << ptr_numbers[1] << std::endl; // Output: 20
      // ptr_numbers[i] tương đương với *(ptr_numbers + i)

      // Thay đổi giá trị qua con trỏ
      *(ptr_numbers + 3) = 45; // numbers[3] = 45
      ptr_numbers[0] = 15;     // numbers[0] = 15

      std::cout << "Modified array: ";
      for (int i = 0; i < 5; ++i) {
          std::cout << numbers[i] << " "; // Output: 15 20 30 45 50
      }
      std::cout << std::endl;

      return 0;
  }
  ```

- **Pointer Arithmetic:**
  - Khi cộng một số nguyên `n` vào một con trỏ `ptr`, địa chỉ mới sẽ là `địa_chỉ_hiện_tại + n * sizeof(*ptr)`. Tức là con trỏ di chuyển `n` _phần tử_, không phải `n` bytes (trừ khi `sizeof(*ptr)` là 1).
  - Các phép toán hợp lệ: `ptr + n`, `ptr - n`, `ptr++`, `ptr--`, `ptr1 - ptr2` (cho kết quả là số phần tử giữa hai con trỏ cùng kiểu trỏ vào cùng một mảng).
  - **Bad Practice:** Pointer arithmetic vượt ra ngoài biên của mảng (hoặc vùng nhớ được cấp phát hợp lệ) là Undefined Behavior.

**4. Con Trỏ Hàm (Function Pointers)**

- Con trỏ hàm lưu trữ địa chỉ của một hàm. Cho phép truyền hàm như một đối số cho hàm khác, trả về hàm từ hàm khác, hoặc lưu trữ hàm trong cấu trúc dữ liệu.
- **Khai báo:** `return_type (*pointer_name)(parameter_type_list);`

  - Dấu `(*pointer_name)` là quan trọng để phân biệt với khai báo hàm trả về con trỏ.

  ```cpp
  #include <iostream>

  void sayHello(const char* name) {
      std::cout << "Hello, " << name << "!" << std::endl;
  }

  int add(int a, int b) {
      return a + b;
  }

  int subtract(int a, int b) {
      return a - b;
  }

  // Hàm nhận con trỏ hàm làm tham số
  void greet(void (*greeting_func)(const char*), const char* person_name) {
      greeting_func(person_name); // Gọi hàm thông qua con trỏ
  }

  // Hàm nhận con trỏ hàm và trả về kết quả
  int calculate(int x, int y, int (*operation_func)(int, int)) {
      return operation_func(x, y);
  }

  int main() {
      // Khai báo và khởi tạo con trỏ hàm
      void (*ptr_say_hello)(const char*);
      ptr_say_hello = &sayHello; // Lấy địa chỉ hàm sayHello (dấu & là tùy chọn)
      // ptr_say_hello = sayHello; // Cũng hợp lệ, tên hàm phân rã thành con trỏ hàm

      // Gọi hàm thông qua con trỏ
      (*ptr_say_hello)("Alice"); // Cú pháp truyền thống
      ptr_say_hello("Bob");      // Cú pháp đơn giản hơn, cũng hợp lệ

      // Sử dụng với các hàm khác
      greet(sayHello, "Charlie");

      int (*ptr_op)(int, int);

      ptr_op = add;
      std::cout << "5 + 3 = " << calculate(5, 3, ptr_op) << std::endl; // Output: 8

      ptr_op = subtract;
      std::cout << "7 - 2 = " << calculate(7, 2, ptr_op) << std::endl; // Output: 5

      // Mảng các con trỏ hàm (yêu cầu các hàm có cùng signature)
      int (*operations[2])(int, int) = {add, subtract};
      std::cout << "10 + 20 (via array): " << operations[0](10, 20) << std::endl; // Output: 30
      std::cout << "100 - 25 (via array): " << operations[1](100, 25) << std::endl; // Output: 75

      return 0;
  }
  ```

- `std::function` (C++11 trở đi, trong `<functional>`) là một cách hiện đại và linh hoạt hơn để làm việc với các đối tượng có thể gọi được (callables), bao gồm con trỏ hàm, lambda, functors.

**5. Cấp Phát Động Bộ Nhớ (Dynamic Memory Allocation - C-style)**

- **Stack Memory vs. Heap Memory:**
  - **Stack:** Vùng nhớ được quản lý tự động. Biến cục bộ, tham số hàm được cấp phát trên stack. Bộ nhớ được cấp phát và giải phóng theo thứ tự LIFO (Last-In, First-Out) khi hàm được gọi và trả về. Nhanh nhưng giới hạn về kích thước.
  - **Heap (Free Store):** Vùng nhớ lớn hơn, không được quản lý tự động. Lập trình viên phải yêu cầu cấp phát và chịu trách nhiệm giải phóng bộ nhớ một cách tường minh. Linh hoạt về kích thước và thời gian sống của đối tượng.
- **Toán tử `new`:** Dùng để cấp phát bộ nhớ trên heap.

  - `new type`: Cấp phát đủ bộ nhớ cho một đối tượng kiểu `type` và trả về con trỏ tới vùng nhớ đó.
  - `new type(initializer)`: Cấp phát và khởi tạo đối tượng.
  - `new type[size]`: Cấp phát đủ bộ nhớ cho một mảng `size` phần tử kiểu `type` và trả về con trỏ tới phần tử đầu tiên.
  - Nếu `new` thất bại (không đủ bộ nhớ), nó sẽ ném một exception `std::bad_alloc` (hành vi chuẩn). Trong các trình biên dịch cũ hoặc với `(std::nothrow)`, nó có thể trả về `nullptr`.

    ```cpp
    int* ptr_int = new int;          // Cấp phát một int, chưa khởi tạo (giá trị rác)
    *ptr_int = 123;

    int* ptr_int_initialized = new int(456); // Cấp phát và khởi tạo

    double* ptr_double_array = new double[10]; // Cấp phát mảng 10 double
    // ptr_double_array[0] = 1.1; ...

    // Ví dụ kiểm tra nothrow (ít dùng hơn, thường để exception xử lý)
    // #include <new> // Cho std::nothrow
    // int* ptr_safe_int = new (std::nothrow) int;
    // if (ptr_safe_int == nullptr) {
    //     // Xử lý lỗi cấp phát
    //     std::cerr << "Memory allocation failed!" << std::endl;
    // } else {
    //     *ptr_safe_int = 789;
    //     // ... sử dụng ptr_safe_int
    //     delete ptr_safe_int; // Nhớ giải phóng
    // }
    ```

- **Toán tử `delete`:** Dùng để giải phóng bộ nhớ đã được cấp phát bằng `new`.

  - `delete pointer`: Giải phóng bộ nhớ được cấp phát cho một đối tượng đơn.
  - `delete[] pointer`: Giải phóng bộ nhớ được cấp phát cho một mảng đối tượng.
  - **QUAN TRỌNG:**

    - Chỉ `delete` con trỏ nhận được từ `new`.
    - Chỉ `delete` một con trỏ một lần duy nhất.
    - Phải dùng `delete[]` cho bộ nhớ cấp phát bằng `new[]`, và `delete` cho bộ nhớ cấp phát bằng `new` (không có `[]`). Dùng sai sẽ dẫn đến UB.
    - Sau khi `delete`, gán con trỏ về `nullptr` là một thói quen tốt để tránh "dangling pointer".

    ```cpp
    // Tiếp tục từ ví dụ trên
    delete ptr_int;
    ptr_int = nullptr; // Good practice

    delete ptr_int_initialized;
    ptr_int_initialized = nullptr;

    delete[] ptr_double_array; // QUAN TRỌNG: dùng delete[] cho mảng
    ptr_double_array = nullptr;
    ```

**6. Các Lỗi Thường Gặp với Con Trỏ và Quản Lý Bộ Nhớ**

- **Dangling Pointers (Con trỏ lơ lửng):**

  - Xảy ra khi một con trỏ vẫn trỏ đến một vùng nhớ đã được giải phóng (`delete`) hoặc một biến cục bộ đã ra khỏi scope.
  - Truy cập qua dangling pointer là UB.
  - **Ví dụ:**

    ```cpp
    int* ptr = new int(10);
    // ... sử dụng ptr ...
    delete ptr;
    // ptr bây giờ là dangling pointer
    // *ptr = 20; // UNDEFINED BEHAVIOR!

    int* createDangling() {
        int local_var = 5;
        return &local_var; // Trả về địa chỉ biến cục bộ
    }
    // int* d_ptr = createDangling();
    // *d_ptr = 50; // UNDEFINED BEHAVIOR! local_var đã bị hủy
    ```

  - **Cách tránh:** Gán `nullptr` cho con trỏ ngay sau khi `delete`. Không trả về con trỏ/tham chiếu tới biến cục bộ.

- **Memory Leaks (Rò rỉ bộ nhớ):**
  - Xảy ra khi bộ nhớ được cấp phát động (`new`) không được giải phóng (`delete`) khi không còn cần thiết.
  - Chương trình sẽ chiếm dụng ngày càng nhiều bộ nhớ, có thể dẫn đến cạn kiệt bộ nhớ và crash.
  - **Ví dụ:**
    ```cpp
    void causeLeak() {
        int* leaky_ptr = new int[1000];
        // ... làm gì đó với leaky_ptr ...
        // QUÊN delete[] leaky_ptr; -> Memory leak khi hàm kết thúc
    }
    // for (int i = 0; i < 10000; ++i) {
    //    causeLeak(); // Sẽ gây rò rỉ bộ nhớ lớn
    // }
    ```
  - **Cách tránh:** Luôn đảm bảo mọi `new`/`new[]` đều có một `delete`/`delete[]` tương ứng. Sử dụng Smart Pointers (Phần 12) để tự động hóa việc này (RAII).
- **Double Free:**
  - Xảy ra khi giải phóng cùng một vùng nhớ hai lần. Dẫn đến UB, thường là crash.
  - **Ví dụ:**
    ```cpp
    int* ptr_double = new int(5);
    delete ptr_double;
    // ... code khác ...
    // delete ptr_double; // UNDEFINED BEHAVIOR - Double Free!
    ```
  - **Cách tránh:** Gán `nullptr` sau khi `delete`. `delete nullptr;` là an toàn và không làm gì cả.
- **Dereferencing Null Pointer:**
  - Truy cập giá trị qua một con trỏ `nullptr` là UB, thường gây crash.
  - **Ví dụ:**
    ```cpp
    int* null_p = nullptr;
    // *null_p = 10; // UNDEFINED BEHAVIOR!
    // if (null_p) { *null_p = 10; } // Kiểm tra trước là an toàn
    ```
  - **Cách tránh:** Luôn kiểm tra con trỏ trước khi dereference nếu có khả năng nó là `nullptr`.
- **Buffer Overflow/Underflow (khi dùng pointer arithmetic với mảng):**
  - Truy cập ngoài vùng nhớ hợp lệ của mảng.
  - **Ví dụ:**
    ```cpp
    int arr[5];
    int* p_arr = arr;
    // *(p_arr + 5) = 10; // Buffer overflow (chỉ số hợp lệ là 0-4)
    // *(p_arr - 1) = 20; // Buffer underflow
    ```
  - **Cách tránh:** Cẩn thận với chỉ số và pointer arithmetic.

**7. `nullptr` (C++11)**

- Trước C++11, `NULL` (thường là `#define NULL 0` hoặc `#define NULL ((void*)0)`) được dùng để biểu thị con trỏ rỗng. Điều này có thể gây mơ hồ vì `0` cũng là một `int`.
- `nullptr` là một từ khóa mới trong C++11, là một hằng con trỏ rỗng có kiểu đặc biệt `std::nullptr_t`.
- `nullptr` an toàn hơn về kiểu và giải quyết sự mơ hồ của `NULL`.

  ```cpp
  #include <cstddef> // Cho NULL (nếu cần so sánh) và std::nullptr_t

  void func(int i) { std::cout << "func(int)\n"; }
  void func(char* s) { std::cout << "func(char*)\n"; }
  void func(std::nullptr_t) { std::cout << "func(nullptr_t)\n"; }

  int main() {
      int* p1 = nullptr;    // Ưu tiên
      // int* p2 = NULL;    // Vẫn hoạt động nhưng nên tránh
      // int* p3 = 0;       // Vẫn hoạt động nhưng nên tránh

      if (p1 == nullptr) {
          std::cout << "p1 is null\n";
      }

      // func(NULL); // Có thể mơ hồ, compiler có thể chọn func(int) hoặc lỗi
                    // Phụ thuộc vào định nghĩa của NULL
      func(nullptr); // Rõ ràng gọi func(std::nullptr_t) hoặc func(char*) (nếu có chuyển đổi ngầm)
                     // Trong ví dụ này, nó sẽ gọi func(std::nullptr_t)
      return 0;
  }
  ```

- **Best Practice:** Luôn sử dụng `nullptr` thay vì `NULL` hoặc `0` cho con trỏ rỗng trong C++ hiện đại.

**8. Ví dụ & Best/Bad Practices Tổng Hợp (Phần 3)**

- **Best Practices:**

  - **Khởi tạo con trỏ:** Luôn khởi tạo con trỏ khi khai báo, hoặc là với địa chỉ của một biến hợp lệ, hoặc với `nullptr`, hoặc với kết quả của `new`.
    ```cpp
    int x;
    int* p_x = &x;
    int* p_null = nullptr;
    int* p_dyn = new int(10);
    // delete p_dyn; p_dyn = nullptr; // Nhớ giải phóng và gán null
    ```
  - **Kiểm tra `nullptr`:** Trước khi dereference một con trỏ có khả năng là null, hãy kiểm tra nó.
  - **`delete` tương ứng với `new`:** `delete` cho `new`, `delete[]` cho `new[]`.
  - **Gán `nullptr` sau `delete`:** Giúp tránh dangling pointers.
  - **Sử dụng `const` với con trỏ:**

    - `const int* ptr;` hoặc `int const* ptr;`: Con trỏ tới hằng `int` (giá trị `*ptr` không thể thay đổi, nhưng `ptr` có thể trỏ tới nơi khác).
    - `int* const ptr;`: Con trỏ hằng tới `int` (giá trị `*ptr` có thể thay đổi, nhưng `ptr` không thể trỏ tới nơi khác, phải khởi tạo khi khai báo).
    - `const int* const ptr;` hoặc `int const* const ptr;`: Con trỏ hằng tới hằng `int` (cả `ptr` và `*ptr` đều không thể thay đổi, phải khởi tạo khi khai báo).

      ```cpp
      int a = 10, b = 20;
      const int val_const = 100;

      const int* ptr_to_const = &a;
      // *ptr_to_const = 15; // Lỗi: không thể thay đổi giá trị qua con trỏ
      ptr_to_const = &b;    // OK: con trỏ có thể trỏ tới nơi khác
      ptr_to_const = &val_const; // OK

      int* const const_ptr = &a;
      *const_ptr = 15;      // OK: có thể thay đổi giá trị
      // const_ptr = &b;    // Lỗi: con trỏ không thể trỏ tới nơi khác

      const int* const const_ptr_to_const = &a;
      // *const_ptr_to_const = 25; // Lỗi
      // const_ptr_to_const = &b; // Lỗi
      ```

  - **Ưu tiên Smart Pointers (sẽ học ở Phần 12):** Đây là cách hiện đại và an toàn nhất để quản lý bộ nhớ động trong C++, giúp tự động hóa việc giải phóng bộ nhớ và tránh nhiều lỗi phổ biến. Khi học về smart pointers, bạn sẽ thấy việc dùng `new`/`delete` trực tiếp trở nên ít cần thiết hơn nhiều.

- **Bad Practices (Tóm tắt lại):**
  - Sử dụng con trỏ chưa khởi tạo.
  - Dereference `nullptr`.
  - Quên `delete`/`delete[]` (memory leak).
  - Sử dụng `delete` cho bộ nhớ cấp phát bằng `new[]` hoặc ngược lại.
  - Double free.
  - Truy cập dangling pointer.
  - Pointer arithmetic ra ngoài biên.
  - Trả về con trỏ/tham chiếu tới biến cục bộ.

Quản lý bộ nhớ thủ công với con trỏ kiểu C là một kỹ năng nền tảng nhưng đòi hỏi sự cẩn trọng cao. Hiểu rõ các khái niệm này sẽ giúp bạn đánh giá cao lợi ích của các cơ chế quản lý bộ nhớ an toàn hơn trong C++ hiện đại.

Hết Phần 3.

OK. Tiếp tục với Phần 4, tập trung vào Mảng, Chuỗi ký tự kiểu C và giới thiệu `std::string`.

**Phần 4: Mảng, Chuỗi Ký Tự (C-style) và `std::string`**

**1. Mảng (Arrays)**

- **Định nghĩa:** Một tập hợp các phần tử có cùng kiểu dữ liệu, được lưu trữ liên tiếp trong bộ nhớ.
- **Khai báo:** `type array_name[size];`
  - `size` phải là một hằng số tại thời điểm biên dịch (compile-time constant) đối với mảng tĩnh (static arrays) và mảng trên stack. Variable Length Arrays (VLAs) là một phần mở rộng của C99 và một số compiler C++, nhưng không phải là C++ chuẩn và nên tránh.
- **Khởi tạo:**

  ```cpp
  #include <iostream>

  int main() {
      // Khai báo và khởi tạo mảng
      int scores[5] = {90, 85, 77, 92, 88}; // Khởi tạo đầy đủ
      double prices[3] = {1.99, 2.50};      // prices[2] sẽ được khởi tạo là 0.0 (zero-initialization)
      char message[10] = {'H', 'e', 'l', 'l', 'o'}; // Các phần tử còn lại là '\0' (null character)

      // Nếu không cung cấp kích thước, trình biên dịch sẽ tự suy ra từ list khởi tạo
      int data[] = {1, 2, 3, 4, 5, 6}; // data có kích thước là 6

      // Khởi tạo tất cả phần tử bằng 0 (hoặc giá trị mặc định cho kiểu)
      int zeros[100] = {0}; // Tất cả 100 phần tử là 0
      // Hoặc chỉ cần:
      int more_zeros[50] = {}; // Tất cả 50 phần tử được zero-initialized (C++11)

      // Truy cập phần tử
      std::cout << "First score: " << scores[0] << std::endl; // Output: 90
      scores[1] = 87; // Thay đổi giá trị
      std::cout << "Second score (modified): " << scores[1] << std::endl; // Output: 87

      // Kích thước mảng
      // sizeof(array_name) trả về tổng số byte của mảng
      // sizeof(array_name[0]) trả về số byte của một phần tử
      int num_elements_scores = sizeof(scores) / sizeof(scores[0]);
      std::cout << "Number of elements in scores: " << num_elements_scores << std::endl; // Output: 5

      // Vòng lặp duyệt mảng
      std::cout << "All scores: ";
      for (int i = 0; i < num_elements_scores; ++i) {
          std::cout << scores[i] << " ";
      }
      std::cout << std::endl;

      // Range-based for (C++11)
      std::cout << "Prices (range-based for): ";
      for (double price : prices) {
          std::cout << price << " "; // Output: 1.99 2.5 0
      }
      std::cout << std::endl;

      return 0;
  }
  ```

- **Mảng đa chiều (Multidimensional Arrays):** Mảng của các mảng.

  - Thường gặp nhất là mảng 2 chiều (ma trận).
  - Được lưu trữ theo hàng (row-major order) trong bộ nhớ.

  ```cpp
  #include <iostream>

  int main() {
      // Mảng 2 chiều: 3 hàng, 4 cột
      int matrix[3][4] = {
          {1, 2, 3, 4},    // Hàng 0
          {5, 6, 7, 8},    // Hàng 1
          {9, 10, 11, 12}  // Hàng 2
      };

      // Truy cập phần tử
      std::cout << "Element at matrix[1][2]: " << matrix[1][2] << std::endl; // Output: 7
      matrix[0][0] = 100;

      // Duyệt mảng 2 chiều
      std::cout << "Matrix elements:" << std::endl;
      for (int i = 0; i < 3; ++i) { // Duyệt qua các hàng
          for (int j = 0; j < 4; ++j) { // Duyệt qua các cột trong mỗi hàng
              std::cout << matrix[i][j] << "\t";
          }
          std::cout << std::endl;
      }
      return 0;
  }
  ```

- **Mảng và Con Trỏ:** Như đã đề cập ở Phần 3, tên mảng thường phân rã thành con trỏ tới phần tử đầu tiên.
- **Truyền mảng vào hàm:**

  - Khi truyền mảng vào hàm, nó thực sự được truyền dưới dạng con trỏ tới phần tử đầu tiên. Thông tin kích thước bị mất.
  - Do đó, cần truyền kích thước của mảng như một tham số riêng biệt.

  ```cpp
  #include <iostream>

  // Cách 1: Dùng con trỏ và kích thước
  void printArray(int* arr, int size) {
      // sizeof(arr) ở đây sẽ trả về kích thước của con trỏ (4 hoặc 8 bytes), KHÔNG phải kích thước mảng
      for (int i = 0; i < size; ++i) {
          std::cout << arr[i] << " "; // arr[i] tương đương *(arr + i)
      }
      std::cout << std::endl;
  }

  // Cách 2 (C-style): Dùng cú pháp mảng (vẫn là con trỏ ngầm)
  void modifyArray(int arr[], int size) { // Tương đương void modifyArray(int* arr, int size)
      for (int i = 0; i < size; ++i) {
          arr[i] *= 2;
      }
  }

  // Cách 3 (C++ Template - tốt hơn, giữ được kích thước nếu là mảng tĩnh)
  template <size_t N> // N là kích thước mảng, được suy ra tại compile time
  void printArrayModern(const int (&arr)[N]) { // Truyền bằng tham chiếu tới mảng
      std::cout << "Modern print (size " << N << "): ";
      for (int i = 0; i < N; ++i) {
          std::cout << arr[i] << " ";
      }
      std::cout << std::endl;
  }

  int main() {
      int my_array[] = {10, 20, 30, 40, 50};
      int size = sizeof(my_array) / sizeof(my_array[0]);

      printArray(my_array, size); // Output: 10 20 30 40 50

      modifyArray(my_array, size);
      printArray(my_array, size); // Output: 20 40 60 80 100

      printArrayModern(my_array); // Output: Modern print (size 5): 20 40 60 80 100

      int another_array[] = {1,2,3};
      printArrayModern(another_array); // Output: Modern print (size 3): 1 2 3

      return 0;
  }
  ```

  - **Best Practice:** Khi có thể, sử dụng `std::vector` hoặc `std::array` (sẽ học sau) thay vì mảng C-style thô, vì chúng quản lý kích thước và cung cấp nhiều tiện ích hơn. Nếu phải dùng mảng C-style, cách truyền tham chiếu tới mảng (như `printArrayModern`) là an toàn nhất để giữ thông tin kích thước cho mảng tĩnh.

**2. Chuỗi Ký Tự C-style (Null-Terminated Char Arrays)**

- Trong C và C++, chuỗi ký tự kiểu C là một mảng các ký tự (`char`) kết thúc bằng một ký tự null đặc biệt (`\0`).
- Ký tự null (`\0`) đánh dấu sự kết thúc của chuỗi.
- **Khai báo và Khởi tạo:**

  ```cpp
  #include <iostream>
  #include <cstring> // Cho các hàm xử lý chuỗi C-style như strlen, strcpy

  int main() {
      // Cách 1: Mảng ký tự với khởi tạo từng ký tự và null terminator
      char str1[6] = {'H', 'e', 'l', 'l', 'o', '\0'};
      std::cout << "str1: " << str1 << std::endl; // Output: Hello

      // Cách 2: Dùng chuỗi ký tự hằng (string literal)
      // Trình biên dịch tự động thêm '\0' vào cuối
      char str2[] = "World"; // Kích thước tự động là 6 (W,o,r,l,d,\0)
      std::cout << "str2: " << str2 << std::endl; // Output: World

      // Kích thước phải đủ lớn để chứa chuỗi và '\0'
      char str3[20] = "C-style string";
      std::cout << "str3: " << str3 << std::endl;

      // Con trỏ tới chuỗi ký tự hằng (string literal)
      // String literals thường được lưu ở vùng nhớ read-only.
      const char* ptr_str_literal = "This is a literal.";
      std::cout << "ptr_str_literal: " << ptr_str_literal << std::endl;
      // ptr_str_literal[0] = 't'; // LỖI BIÊN DỊCH hoặc CRASH (Undefined Behavior) nếu cố sửa đổi

      // Mảng char trên stack có thể sửa đổi
      char modifiable_str[] = "Change me";
      modifiable_str[0] = 'c';
      std::cout << "Modified str: " << modifiable_str << std::endl; // Output: change me

      return 0;
  }
  ```

- **Thư viện `<cstring>` (hoặc `<string.h>` trong C):** Cung cấp các hàm để thao tác với chuỗi C-style.

  - `strlen(const char* str)`: Trả về độ dài chuỗi (không tính `\0`).
  - `strcpy(char* dest, const char* src)`: Sao chép chuỗi `src` vào `dest`. **RẤT NGUY HIỂM** nếu `dest` không đủ lớn (buffer overflow).
  - `strncpy(char* dest, const char* src, size_t n)`: Sao chép tối đa `n` ký tự. An toàn hơn `strcpy` nhưng cần cẩn thận đảm bảo `dest` được kết thúc bằng `\0` nếu `strlen(src) >= n`.
  - `strcat(char* dest, const char* src)`: Nối chuỗi `src` vào cuối `dest`. **RẤT NGUY HIỂM** (buffer overflow).
  - `strncat(char* dest, const char* src, size_t n)`: Nối tối đa `n` ký tự.
  - `strcmp(const char* s1, const char* s2)`: So sánh hai chuỗi. Trả về 0 nếu bằng nhau, <0 nếu `s1` < `s2`, >0 nếu `s1` > `s2` (so sánh theo thứ tự từ điển).
  - `strchr(const char* str, int c)`: Tìm ký tự `c` đầu tiên trong `str`.
  - `strstr(const char* haystack, const char* needle)`: Tìm chuỗi con `needle` đầu tiên trong `haystack`.

  ```cpp
  #include <iostream>
  #include <cstring> // Quan trọng

  int main() {
      char greeting[50] = "Hello";
      const char* name = " Alice"; // Thêm dấu cách ở đầu để nối đẹp

      // strlen
      std::cout << "Length of greeting: " << strlen(greeting) << std::endl; // Output: 5

      // strcat (NGUY HIỂM NẾU KHÔNG CẨN THẬN VỀ KÍCH THƯỚC)
      // Giả sử greeting đủ lớn
      strcat(greeting, name); // greeting giờ là "Hello Alice"
      std::cout << "After strcat: " << greeting << std::endl;
      std::cout << "New length: " << strlen(greeting) << std::endl; // Output: 11

      // strcpy (NGUY HIỂM)
      char destination[20];
      const char* source = "Short string";
      strcpy(destination, source); // OK nếu destination đủ lớn
      std::cout << "Copied string: " << destination << std::endl;

      // strncpy (an toàn hơn, nhưng cần lưu ý)
      char safe_dest[10];
      const char* long_source = "This is too long for safe_dest";
      strncpy(safe_dest, long_source, sizeof(safe_dest) - 1); // Sao chép tối đa 9 ký tự
      safe_dest[sizeof(safe_dest) - 1] = '\0'; // LUÔN đảm bảo null-termination
      std::cout << "strncpy result: " << safe_dest << std::endl; // Output: This is t

      // strcmp
      const char* s1 = "apple";
      const char* s2 = "apply";
      const char* s3 = "apple";
      if (strcmp(s1, s2) < 0) {
          std::cout << s1 << " comes before " << s2 << std::endl; // Output
      }
      if (strcmp(s1, s3) == 0) {
          std::cout << s1 << " is equal to " << s3 << std::endl; // Output
      }

      return 0;
  }
  ```

- **Vấn đề An Toàn (Buffer Overflow):**
  - Các hàm như `strcpy`, `strcat`, `sprintf`, `gets` (đã bị loại bỏ khỏi C++11 và không nên dùng) rất dễ gây lỗi buffer overflow nếu buffer đích không đủ lớn để chứa kết quả.
  - Buffer overflow là một lỗ hổng bảo mật nghiêm trọng, có thể bị khai thác để thực thi mã độc.
  - **Best Practice:** Hết sức cẩn thận khi dùng các hàm này. Luôn đảm bảo buffer đủ lớn. Ưu tiên các phiên bản an toàn hơn (như `strncpy`, `snprintf`) và kiểm tra kích thước cẩn thận, hoặc tốt nhất là dùng `std::string`.

**3. Giới Thiệu `std::string` (Thư viện `<string>`)**

- `std::string` là một lớp trong Thư viện Chuẩn C++ (STL) được thiết kế để làm việc với chuỗi ký tự một cách an toàn và tiện lợi hơn nhiều so với chuỗi C-style.
- **Ưu điểm so với chuỗi C-style:**
  - **Quản lý bộ nhớ tự động:** Tự động cấp phát và giải phóng bộ nhớ khi cần, không lo memory leak hoặc buffer overflow do cấp phát sai.
  - **Biết kích thước:** Luôn theo dõi độ dài của chuỗi (`length()` hoặc `size()`).
  - **Nhiều hàm thành viên tiện ích:** Nối chuỗi (`+`, `+=`), so sánh (`==`, `!=`, `<`, `>`), tìm kiếm (`find`), trích xuất chuỗi con (`substr`), v.v.
  - **Tích hợp tốt với các thuật toán STL.**
- **Khai báo và Sử dụng cơ bản:**

  ```cpp
  #include <iostream>
  #include <string> // Quan trọng để dùng std::string

  int main() {
      // Khởi tạo std::string
      std::string s1;                         // Chuỗi rỗng
      std::string s2 = "Hello";               // Từ C-style string literal
      std::string s3("World");                // Constructor
      std::string s4(s2);                     // Copy constructor (s4 là bản sao của s2)
      std::string s5(10, 'A');                // Chuỗi gồm 10 ký tự 'A': "AAAAAAAAAA"

      std::cout << "s1: " << s1 << std::endl;
      std::cout << "s2: " << s2 << std::endl;
      std::cout << "s3: " << s3 << std::endl;
      std::cout << "s4: " << s4 << std::endl;
      std::cout << "s5: " << s5 << std::endl;

      // Nối chuỗi
      std::string greeting = s2 + " " + s3 + "!"; // Dùng toán tử +
      std::cout << "Greeting: " << greeting << std::endl; // Output: Hello World!

      s2 += " C++"; // Dùng toán tử +=
      std::cout << "s2 modified: " << s2 << std::endl; // Output: Hello C++

      // Độ dài chuỗi
      std::cout << "Length of greeting: " << greeting.length() << std::endl; // hoặc .size()

      // Truy cập ký tự (an toàn hơn với .at())
      std::cout << "First char of greeting: " << greeting[0] << std::endl; // 'H'
      // greeting[100]; // Truy cập ngoài biên, có thể là UB với [], nhưng với at() sẽ ném exception

      try {
          char c = greeting.at(5); // Ký tự tại vị trí 5 (dấu cách)
          std::cout << "Char at index 5: " << c << std::endl;
          // char C_out_of_bounds = greeting.at(100); // Sẽ ném std::out_of_range
      } catch (const std::out_of_range& e) {
          std::cerr << "Exception: " << e.what() << std::endl;
      }

      // So sánh chuỗi
      std::string name1 = "Alice";
      std::string name2 = "Bob";
      if (name1 < name2) {
          std::cout << name1 << " comes before " << name2 << std::endl; // Output
      }
      if (name1 == "Alice") {
          std::cout << name1 << " is Alice." << std::endl; // Output
      }

      // Tìm kiếm
      std::string sentence = "The quick brown fox jumps over the lazy dog.";
      size_t pos = sentence.find("fox"); // size_t là kiểu không dấu
      if (pos != std::string::npos) { // std::string::npos là giá trị đặc biệt báo không tìm thấy
          std::cout << "\"fox\" found at position: " << pos << std::endl; // Output: 16
      } else {
          std::cout << "\"fox\" not found." << std::endl;
      }

      // Chuỗi con
      std::string sub = sentence.substr(10, 5); // Bắt đầu từ vị trí 10, lấy 5 ký tự
      std::cout << "Substring: " << sub << std::endl; // Output: brown

      // Lấy C-style string (const char*) từ std::string
      const char* c_str_representation = greeting.c_str();
      std::cout << "C-style string from std::string: " << c_str_representation << std::endl;
      // Quan trọng: Con trỏ trả về bởi c_str() chỉ hợp lệ chừng nào đối tượng std::string gốc còn tồn tại
      // và không bị sửa đổi theo cách làm thay đổi bộ đệm bên trong.

      // Đọc input vào std::string
      std::string user_input;
      std::cout << "Enter a line: ";
      // std::cin >> user_input; // Chỉ đọc đến khoảng trắng đầu tiên
      std::getline(std::cin, user_input); // Đọc cả dòng, bao gồm khoảng trắng
      std::cout << "You entered: " << user_input << std::endl;

      return 0;
  }
  ```

**4. Ví dụ & Best Practices Tổng Hợp (Phần 4)**

- **Mảng C-style:**
  - **Bad Practice:** Khai báo mảng với kích thước là biến không hằng (VLA) vì không chuẩn C++.
  - **Bad Practice:** Truy cập ngoài biên mảng (array out-of-bounds) là UB.
  - **Best Practice:** Khi truyền mảng C-style vào hàm, truyền kích thước riêng. Cân nhắc dùng tham chiếu tới mảng (template) để giữ thông tin kích thước cho mảng tĩnh.
  - **Best Practice:** Cân nhắc `std::array` (cho mảng kích thước cố định tại compile-time) hoặc `std::vector` (cho mảng động) thay thế mảng C-style.
- **Chuỗi C-style:**
  - **BAD PRACTICE:** Sử dụng các hàm như `strcpy`, `strcat`, `sprintf` mà không kiểm soát chặt chẽ kích thước buffer đích. Đây là nguồn gốc chính của buffer overflow.
  - **Best Practice:** Nếu phải dùng chuỗi C-style, hãy dùng các phiên bản an toàn hơn như `strncpy`, `strncat`, `snprintf` và luôn đảm bảo null-termination.
  - **Best Practice:** **ƯU TIÊN TUYỆT ĐỐI `std::string`** so với chuỗi C-style trong hầu hết mọi trường hợp trong C++.
- **`std::string`:**
  - **Best Practice:** Sử dụng `std::string` cho tất cả các nhu cầu xử lý chuỗi ký tự trong C++ hiện đại. Nó an toàn, tiện lợi và mạnh mẽ.
  - **Best Practice:** Dùng `at()` thay vì `[]` để truy cập ký tự nếu bạn muốn kiểm tra biên và nhận exception khi truy cập sai. Dùng `[]` nếu bạn chắc chắn chỉ số hợp lệ và cần hiệu năng tối đa (dù sự khác biệt thường không đáng kể).
  - **Best Practice:** Khi truyền `std::string` vào hàm:
    - Nếu hàm chỉ đọc chuỗi: `const std::string&` (tránh copy, không cho sửa).
    - Nếu hàm cần sửa chuỗi gốc: `std::string&`.
    - Nếu hàm cần một bản sao riêng hoặc sẽ sửa đổi nhiều: `std::string` (pass-by-value, tận dụng move semantics nếu có thể, sẽ học sau).
  - **Best Practice:** Cẩn thận khi dùng `c_str()`. Con trỏ trả về chỉ hợp lệ khi `std::string` gốc không bị hủy hoặc thay đổi bộ đệm. Nếu cần một bản sao C-style string tồn tại độc lập, hãy cấp phát bộ nhớ mới và `strcpy`.

Sự ra đời của `std::string` là một cải tiến lớn, giúp loại bỏ rất nhiều vấn đề cố hữu của chuỗi C-style.

Hết Phần 4.

OK. Tiếp tục với Phần 5, bắt đầu khám phá thế giới Lập Trình Hướng Đối Tượng (OOP) trong C++.

**Phần 5: Lập Trình Hướng Đối Tượng (OOP) - Cơ Sở**

Lập trình hướng đối tượng (OOP) là một mô hình lập trình dựa trên khái niệm "đối tượng", có thể chứa dữ liệu dưới dạng các trường (thường được gọi là thuộc tính hoặc thành viên dữ liệu) và mã dưới dạng các thủ tục (thường được gọi là phương thức hoặc hàm thành viên). OOP giúp tổ chức mã nguồn một cách logic, dễ quản lý, tái sử dụng và mở rộng.

Các trụ cột chính của OOP thường bao gồm:

1.  **Encapsulation (Tính đóng gói)**
2.  **Abstraction (Tính trừu tượng)** - Thường đi kèm với Encapsulation.
3.  **Inheritance (Tính kế thừa)** - Sẽ học ở Phần 6.
4.  **Polymorphism (Tính đa hình)** - Sẽ học ở Phần 6.

**1. `struct` vs `class`**

Trong C++, cả `struct` và `class` đều có thể được sử dụng để định nghĩa các kiểu dữ liệu do người dùng tự định nghĩa, có thể chứa cả dữ liệu và hàm.
Sự khác biệt chính duy nhất giữa chúng là **khả năng truy cập mặc định (default access specifier):**

- **`struct`:** Các thành viên (dữ liệu và hàm) mặc định là `public`.
- **`class`:** Các thành viên mặc định là `private`.

```cpp
#include <iostream>
#include <string>

// Sử dụng struct
struct PointStruct {
    // Mặc định là public
    int x;
    int y;

    void display() {
        std::cout << "Struct Point: (" << x << ", " << y << ")" << std::endl;
    }
};

// Sử dụng class
class PointClass {
    // Mặc định là private nếu không chỉ định
    int x; // Private
    int y; // Private

public: // Phải khai báo public để truy cập từ bên ngoài
    // Constructor (sẽ giải thích sau)
    PointClass(int x_val, int y_val) : x(x_val), y(y_val) {}

    void setX(int new_x) { x = new_x; }
    void setY(int new_y) { y = new_y; }

    int getX() const { return x; } // const sau hàm (sẽ giải thích sau)
    int getY() const { return y; }

    void display() const { // const sau hàm
        std::cout << "Class Point: (" << x << ", " << y << ")" << std::endl;
    }
};

int main() {
    // Struct
    PointStruct ps;
    ps.x = 10; // Truy cập trực tiếp vì mặc định public
    ps.y = 20;
    ps.display(); // Output: Struct Point: (10, 20)

    // Class
    PointClass pc(30, 40); // Khởi tạo qua constructor
    // pc.x = 50; // LỖI: 'x' is private
    pc.setX(50); // Sử dụng hàm public để thay đổi
    pc.display(); // Output: Class Point: (50, 40)
    std::cout << "X from class: " << pc.getX() << std::endl; // Output: 50

    return 0;
}
```

- **Quy ước (Convention):**
  - `struct` thường được dùng cho các cấu trúc dữ liệu đơn giản, nơi các thành viên dữ liệu chủ yếu là `public` và không có nhiều logic phức tạp (Plain Old Data - POD, hoặc các kiểu gần POD).
  - `class` thường được dùng để định nghĩa các đối tượng phức tạp hơn, nơi tính đóng gói (encapsulation) quan trọng, với các thành viên dữ liệu thường là `private` và được truy cập/thay đổi thông qua các hàm thành viên `public` (getters/setters).
- Về mặt kỹ thuật, bạn có thể làm mọi thứ với `struct` mà `class` làm được và ngược lại (bằng cách chỉ định rõ access specifiers).

**2. Encapsulation (Tính đóng gói) và Access Specifiers (`public`, `private`, `protected`)**

- **Encapsulation:** Là việc gói gọn dữ liệu (thuộc tính) và các phương thức (hành vi) hoạt động trên dữ liệu đó vào bên trong một đối tượng. Nó cũng bao gồm việc che giấu chi tiết triển khai bên trong của đối tượng khỏi thế giới bên ngoài.
- **Access Specifiers:** Điều khiển khả năng truy cập đến các thành viên (dữ liệu và hàm) của một lớp từ bên ngoài lớp đó.

  - **`public`:**
    - Các thành viên `public` có thể được truy cập từ bất kỳ đâu, cả bên trong lớp và từ bên ngoài lớp (thông qua một đối tượng của lớp đó).
    - Thường dùng cho interface của lớp (các hàm mà người dùng lớp có thể gọi).
  - **`private`:**
    - Các thành viên `private` chỉ có thể được truy cập bởi các hàm thành viên (member functions) và các hàm bạn (friend functions) của chính lớp đó.
    - Không thể truy cập từ bên ngoài lớp, kể cả từ các lớp con (derived classes - sẽ học ở phần kế thừa).
    - Thường dùng để che giấu dữ liệu nội bộ và chi tiết triển khai, đảm bảo tính toàn vẹn của dữ liệu.
  - **`protected`:**
    - Các thành viên `protected` tương tự như `private`, nhưng chúng có thể được truy cập bởi các hàm thành viên của các lớp con (derived classes).
    - Sẽ quan trọng khi học về kế thừa.

  ```cpp
  class BankAccount {
  private: // Dữ liệu nội bộ, không nên cho phép truy cập trực tiếp từ bên ngoài
      std::string accountNumber;
      double balance;

      // Hàm private helper, chỉ dùng nội bộ
      bool isValidTransaction(double amount) const {
          return amount > 0;
      }

      void logTransaction(const std::string& type, double amount) const {
          std::cout << "Log: " << type << " of " << amount << " on account " << accountNumber << std::endl;
      }

  public: // Interface công khai của lớp
      BankAccount(const std::string& accNum, double initialBalance)
          : accountNumber(accNum), balance(initialBalance) {
          if (initialBalance < 0) {
              balance = 0; // Đảm bảo số dư không âm khi khởi tạo
          }
          std::cout << "Account " << accountNumber << " created with balance " << balance << std::endl;
      }

      void deposit(double amount) {
          if (isValidTransaction(amount)) {
              balance += amount;
              logTransaction("Deposit", amount);
          } else {
              std::cout << "Invalid deposit amount." << std::endl;
          }
      }

      bool withdraw(double amount) {
          if (isValidTransaction(amount) && balance >= amount) {
              balance -= amount;
              logTransaction("Withdrawal", amount);
              return true;
          } else {
              std::cout << "Withdrawal failed: insufficient funds or invalid amount." << std::endl;
              return false;
          }
      }

      double getBalance() const { // Getter để đọc số dư
          return balance;
      }

      std::string getAccountNumber() const { // Getter
          return accountNumber;
      }
  };

  int main() {
      BankAccount acc1("ACC123", 1000.0);
      // acc1.balance = 5000.0; // LỖI: 'balance' is private
      // acc1.logTransaction("Manual", 10); // LỖI: 'logTransaction' is private

      acc1.deposit(500.0); // OK, deposit là public
      acc1.withdraw(200.0); // OK
      acc1.withdraw(2000.0); // Sẽ báo lỗi không đủ tiền

      std::cout << "Account: " << acc1.getAccountNumber() << ", Current Balance: " << acc1.getBalance() << std::endl;

      return 0;
  }
  ```

- **Best Practice:** Mặc định, hãy đặt các thành viên dữ liệu là `private`. Cung cấp các hàm thành viên `public` (getters/setters, hoặc các hàm thực hiện hành vi cụ thể) để tương tác với dữ liệu đó một cách có kiểm soát. Điều này giúp:
  - **Bảo vệ dữ liệu:** Ngăn chặn thay đổi trực tiếp không mong muốn.
  - **Che giấu chi tiết triển khai:** Bạn có thể thay đổi cách lưu trữ dữ liệu bên trong mà không ảnh hưởng đến code sử dụng lớp (miễn là interface public không đổi).
  - **Tăng tính module hóa và dễ bảo trì.**

**3. Thành Viên Dữ Liệu (Data Members) và Hàm Thành Viên (Member Functions)**

- **Thành viên dữ liệu (Data Members/Attributes/Fields):** Là các biến được khai báo bên trong một lớp/struct. Chúng lưu trữ trạng thái của một đối tượng.
  - Ví dụ: `x`, `y` trong `PointClass`; `accountNumber`, `balance` trong `BankAccount`.
- **Hàm thành viên (Member Functions/Methods):** Là các hàm được khai báo bên trong một lớp/struct. Chúng định nghĩa hành vi của đối tượng, cách đối tượng tương tác hoặc cách dữ liệu của nó được xử lý.
  - Ví dụ: `display()`, `setX()`, `getX()` trong `PointClass`; `deposit()`, `withdraw()`, `getBalance()` trong `BankAccount`.
- Hàm thành viên có quyền truy cập trực tiếp đến tất cả các thành viên (dữ liệu và hàm khác) của cùng một lớp, bất kể access specifier của chúng.

**4. Constructors**

- **Mục đích:** Là một hàm thành viên đặc biệt, tự động được gọi khi một đối tượng của lớp được tạo ra. Nhiệm vụ chính của constructor là **khởi tạo** các thành viên dữ liệu của đối tượng, đảm bảo đối tượng ở trạng thái hợp lệ ngay từ khi sinh ra.
- **Đặc điểm:**
  - Tên của constructor **trùng với tên lớp**.
  - **Không có kiểu trả về** (kể cả `void`).
  - Có thể được nạp chồng (overloaded) - tức là một lớp có thể có nhiều constructor với các danh sách tham số khác nhau.
- **Các loại Constructor phổ biến:**

  - **Default Constructor (Constructor mặc định):**

    - Constructor không có tham số.
    - Nếu bạn không định nghĩa bất kỳ constructor nào, trình biên dịch sẽ tự động tạo ra một default constructor _rỗng_ (không làm gì cả) cho bạn.
    - Nếu bạn đã định nghĩa bất kỳ constructor nào (ví dụ, constructor có tham số), trình biên dịch sẽ **không** tự động tạo default constructor nữa. Nếu bạn vẫn muốn có default constructor, bạn phải tự định nghĩa nó.
    - C++11: Bạn có thể yêu cầu trình biên dịch tạo default constructor bằng cách dùng `= default;`

      ```cpp
      class Box {
      public:
          double length;
          double width;
          double height;

          // 1. Default constructor do người dùng định nghĩa
          // Box() {
          //     length = 1.0;
          //     width = 1.0;
          //     height = 1.0;
          //     std::cout << "Default Box constructor called." << std::endl;
          // }

          // 2. Yêu cầu compiler tạo default constructor (C++11)
          Box() = default; // Các thành viên sẽ được khởi tạo mặc định (0 cho kiểu số)

          // Constructor có tham số
          Box(double l, double w, double h) : length(l), width(w), height(h) {
              std::cout << "Parameterized Box constructor called." << std::endl;
          }
      };

      // Box b1; // Sẽ lỗi nếu không có default constructor nào (tự định nghĩa hoặc =default)
               // và đã có constructor có tham số.
      // Box b2(10,20,30);
      ```

  - **Parameterized Constructor (Constructor có tham số):**
    - Constructor nhận một hoặc nhiều tham số để khởi tạo đối tượng.
    - Ví dụ: `PointClass(int x_val, int y_val)` và `BankAccount(const std::string& accNum, double initialBalance)`.
  - **Copy Constructor (Constructor sao chép):**

    - Tạo một đối tượng mới là bản sao của một đối tượng đã tồn tại cùng kiểu.
    - Signature điển hình: `ClassName(const ClassName& other_object)`
    - Nếu bạn không định nghĩa copy constructor, trình biên dịch sẽ tự động tạo một copy constructor thực hiện "member-wise copy" (sao chép từng thành viên dữ liệu). Điều này ổn cho các lớp đơn giản, nhưng có thể gây vấn đề nếu lớp chứa con trỏ quản lý bộ nhớ động (shallow copy vs. deep copy - sẽ bàn kỹ hơn khi nói về Rule of Three/Five).
    - C++11: Bạn có thể yêu cầu compiler tạo copy constructor mặc định: `ClassName(const ClassName&) = default;` hoặc cấm copy: `ClassName(const ClassName&) = delete;`

      ```cpp
      class MyString {
      private:
          char* str_data;
          size_t len;
      public:
          MyString(const char* s = "") { // Constructor có tham số (và default)
              len = strlen(s);
              str_data = new char[len + 1];
              strcpy(str_data, s);
              std::cout << "MyString created: " << str_data << std::endl;
          }

          // Copy Constructor (Deep Copy)
          MyString(const MyString& other) {
              len = other.len;
              str_data = new char[len + 1]; // Cấp phát bộ nhớ mới
              strcpy(str_data, other.str_data); // Sao chép nội dung
              std::cout << "MyString copied (deep): " << str_data << std::endl;
          }

          ~MyString(); // Destructor (sẽ giải thích sau)

          void display() const { std::cout << (str_data ? str_data : "null") << std::endl; }
      };
      // (Destructor sẽ ở mục sau)
      // MyString s1("Hello");
      // MyString s2 = s1; // Gọi copy constructor
      // MyString s3(s1);  // Cũng gọi copy constructor
      ```

- **Initializer List (Danh sách khởi tạo thành viên):**

  - Cách tốt nhất để khởi tạo các thành viên dữ liệu trong constructor. Được đặt sau dấu hai chấm `:` và trước thân constructor.
  - Cú pháp: `ConstructorName(params) : member1(value1), member2(value2), ... { /* thân constructor */ }`
  - **Tại sao nên dùng Initializer List?**

    1.  **Hiệu quả:** Đối với các thành viên là đối tượng của lớp khác (member objects) hoặc các thành viên `const` hoặc tham chiếu, initializer list thực hiện _khởi tạo trực tiếp_. Nếu gán trong thân constructor, đó là _gán sau khi đã khởi tạo mặc định_ (có thể tốn kém hơn).
    2.  **Bắt buộc:** Các thành viên `const` và thành viên tham chiếu (`&`) **phải** được khởi tạo trong initializer list. Chúng không thể được gán giá trị.
    3.  **Thứ tự khởi tạo:** Các thành viên được khởi tạo theo thứ tự chúng được _khai báo_ trong lớp, không phải thứ tự trong initializer list.

        ```cpp
        class Example {
        private:
            const int ID;
            std::string name;
            int& counter_ref;
            int value;

        public:
            Example(int id_val, const std::string& n, int& counter)
                : ID(id_val),         // Khởi tạo const member
                  name(n),            // Khởi tạo member object (hiệu quả hơn gán)
                  counter_ref(counter), // Khởi tạo reference member
                  value(0)            // Khởi tạo member kiểu cơ bản
            {
                // value = 0; // Có thể gán ở đây, nhưng khởi tạo trong list tốt hơn
                std::cout << "Example created. ID: " << ID << ", Name: " << name << std::endl;
                counter_ref++;
            }

            void display() const {
                std::cout << "ID: " << ID << ", Name: " << name << ", Value: " << value
                          << ", Counter: " << counter_ref << std::endl;
            }
        };

        // int main() {
        //     int global_counter = 0;
        //     Example ex1(1, "First", global_counter); // counter_ref trỏ tới global_counter
        //     ex1.display(); // Counter: 1
        //     Example ex2(2, "Second", global_counter);
        //     ex2.display(); // Counter: 2
        //     std::cout << "Global counter: " << global_counter << std::endl; // Output: 2
        //     return 0;
        // }
        ```

**5. Destructors (Hàm hủy)**

- **Mục đích:** Là một hàm thành viên đặc biệt, tự động được gọi khi một đối tượng sắp bị hủy (ví dụ, khi đối tượng ra khỏi scope, hoặc khi `delete` được gọi trên con trỏ tới đối tượng được cấp phát động).
- Nhiệm vụ chính của destructor là **dọn dẹp tài nguyên** mà đối tượng đã chiếm giữ trong suốt vòng đời của nó, ví dụ: giải phóng bộ nhớ động, đóng file, giải phóng kết nối mạng, v.v.
- **Đặc điểm:**

  - Tên của destructor là `~` theo sau là tên lớp (ví dụ: `~ClassName()`).
  - **Không có kiểu trả về** và **không có tham số**.
  - Một lớp chỉ có thể có **một destructor duy nhất**. Không thể nạp chồng destructor.
  - Nếu bạn không định nghĩa destructor, trình biên dịch sẽ tự động tạo một destructor rỗng. Điều này ổn nếu lớp không quản lý tài nguyên nào cần dọn dẹp thủ công.
  - Destructor được gọi theo thứ tự ngược lại với constructor (trong trường hợp các đối tượng lồng nhau hoặc mảng đối tượng).

  ```cpp
  // Tiếp tục với class MyString ở trên
  MyString::~MyString() {
      std::cout << "MyString destroyed: " << (str_data ? str_data : "null") << std::endl;
      delete[] str_data; // Giải phóng bộ nhớ đã cấp phát cho chuỗi
      str_data = nullptr; // Good practice
  }

  // Trong main, khi s1, s2, s3 ra khỏi scope, destructor của chúng sẽ được gọi.
  // int main() {
  //     MyString s1("Hello");
  //     MyString s2 = s1;
  //     {
  //         MyString s3("Temporary");
  //         s3.display();
  //     } // s3 is destroyed here, ~MyString() for s3 is called
  //
  //     MyString* s_ptr = new MyString("Dynamic");
  //     s_ptr->display();
  //     delete s_ptr; // QUAN TRỌNG: gọi delete sẽ kích hoạt ~MyString() cho đối tượng được trỏ bởi s_ptr
  //
  //     return 0; // s2 is destroyed, then s1 is destroyed
  // }
  ```

- **Rule of Three/Five/Zero (Quy tắc Ba/Năm/Không):**
  - **Rule of Three (C++98/03):** Nếu một lớp cần một trong ba hàm sau:
    1.  Destructor (tự định nghĩa)
    2.  Copy Constructor (tự định nghĩa)
    3.  Copy Assignment Operator (sẽ học sau) (tự định nghĩa)
        thì rất có thể nó cần cả ba. Thường là do lớp quản lý tài nguyên (như con trỏ tới bộ nhớ động).
  - **Rule of Five (C++11):** Mở rộng Rule of Three với hai hàm nữa liên quan đến move semantics: 4. Move Constructor (sẽ học sau) 5. Move Assignment Operator (sẽ học sau)
    Nếu bạn định nghĩa một trong năm hàm này, bạn nên xem xét định nghĩa (hoặc `=default` / `=delete`) tất cả năm hàm.
  - **Rule of Zero:** Cố gắng thiết kế các lớp sao cho chúng không cần tự định nghĩa bất kỳ hàm nào trong số năm hàm trên (copy/move constructors, copy/move assignment operators, destructor). Thay vào đó, hãy sử dụng các lớp quản lý tài nguyên có sẵn (như smart pointers: `std::unique_ptr`, `std::shared_ptr`; containers STL: `std::vector`, `std::string`) để chúng tự động xử lý việc sao chép, di chuyển và giải phóng tài nguyên. Đây là cách tiếp cận hiện đại và an toàn hơn.

**6. Con Trỏ `this`**

- Bên trong một hàm thành viên non-static của một lớp, `this` là một con trỏ ngầm định, trỏ đến chính đối tượng mà hàm thành viên đó đang được gọi trên nó.
- Kiểu của `this` là `ClassName* const` (một con trỏ hằng trỏ tới đối tượng kiểu `ClassName`). Trong hàm thành viên `const`, kiểu của `this` là `const ClassName* const`.
- **Sử dụng:**

  - Phân biệt thành viên dữ liệu của lớp với tham số hàm hoặc biến cục bộ có cùng tên.
  - Trả về tham chiếu hoặc con trỏ tới đối tượng hiện tại từ một hàm thành viên (ví dụ, để cho phép chaining method calls).

  ```cpp
  class Data {
  public:
      int value;

      // Constructor dùng this để phân biệt
      Data(int value) {
          this->value = value; // this->value là thành viên dữ liệu, value là tham số
          // Hoặc đặt tên tham số khác: Data(int val) : value(val) {}
      }

      void setValue(int value) {
          this->value = value;
      }

      int getValue() const {
          return this->value; // this-> là tùy chọn ở đây vì không có xung đột tên
                              // return value; cũng được
      }

      Data& increment() {
          this->value++;
          return *this; // Trả về tham chiếu tới đối tượng hiện tại
      }

      Data* getAddress() {
          return this; // Trả về con trỏ tới đối tượng hiện tại
      }

      void printAddress() const {
          std::cout << "Object address: " << this << std::endl;
      }
  };

  int main() {
      Data d1(10);
      d1.printAddress();
      std::cout << "Value: " << d1.getValue() << std::endl;

      d1.setValue(20);
      std::cout << "New Value: " << d1.getValue() << std::endl;

      // Chaining method calls
      d1.increment().increment().increment(); // d1.value giờ là 23
      std::cout << "After increments: " << d1.getValue() << std::endl;

      Data* d_ptr = d1.getAddress();
      d_ptr->printAddress(); // Sẽ giống địa chỉ của d1

      return 0;
  }
  ```

**7. Hàm Thành Viên `const` (Const Member Functions)**

- Một hàm thành viên được khai báo với từ khóa `const` ở cuối signature (trước dấu `;` hoặc trước thân hàm `{`).
- `return_type functionName(params) const;`
- **Ý nghĩa:** Hàm này **hứa** rằng nó sẽ **không thay đổi** bất kỳ thành viên dữ liệu non-static nào của đối tượng (trừ khi thành viên đó được đánh dấu là `mutable`).
- **Tại sao quan trọng?**

  1.  **An toàn:** Giúp trình biên dịch bắt lỗi nếu bạn vô tình cố gắng thay đổi trạng thái của đối tượng bên trong một hàm `const`.
  2.  **Cho phép gọi trên đối tượng `const`:** Bạn chỉ có thể gọi các hàm thành viên `const` trên các đối tượng hằng.

      ```cpp
      class Rectangle {
      private:
          double width;
          double height;
          mutable int access_count = 0; // Thành viên mutable có thể bị thay đổi trong hàm const

      public:
          Rectangle(double w, double h) : width(w), height(h) {}

          double getWidth() const {
              access_count++; // OK vì access_count là mutable
              return width;
          }

          double getHeight() const {
              access_count++;
              return height;
          }

          double getArea() const { // Hàm này không sửa đổi width, height
              access_count++;
              return width * height;
          }

          void setDimensions(double w, double h) { // Hàm non-const, vì nó thay đổi width, height
              width = w;
              height = h;
          }

          int getAccessCount() const {
              return access_count;
          }
      };

      void printRectangleInfo(const Rectangle& rect) { // rect là đối tượng const
          std::cout << "Width: " << rect.getWidth() << std::endl;
          std::cout << "Height: " << rect.getHeight() << std::endl;
          std::cout << "Area: " << rect.getArea() << std::endl;
          // rect.setDimensions(10, 5); // LỖI: không thể gọi hàm non-const trên đối tượng const
      }

      int main() {
          Rectangle r1(5.0, 3.0);
          r1.setDimensions(6.0, 4.0); // OK
          printRectangleInfo(r1);

          const Rectangle r_const(10.0, 2.0);
          std::cout << "Const rect area: " << r_const.getArea() << std::endl; // OK
          // r_const.setDimensions(1,1); // LỖI
          std::cout << "Access count for r1: " << r1.getAccessCount() << std::endl;
          std::cout << "Access count for r_const: " << r_const.getAccessCount() << std::endl;
          return 0;
      }
      ```

- **Best Practice:** Đánh dấu `const` cho tất cả các hàm thành viên không làm thay đổi trạng thái logic của đối tượng. Điều này làm tăng tính đúng đắn và cho phép code linh hoạt hơn (ví dụ, sử dụng đối tượng với `const&`).
- **Thành viên `mutable`:** Dùng từ khóa `mutable` cho các thành viên dữ liệu mà bạn muốn có thể thay đổi được ngay cả bên trong một hàm thành viên `const`. Dùng cẩn thận, thường cho các mục đích như caching, logging, đếm số lần truy cập mà không ảnh hưởng đến trạng thái logic "const" của đối tượng.

**8. Thành Viên `static` (Dữ Liệu và Hàm)**

- **Thành viên dữ liệu `static` (Static Data Members):**
  - Là thành viên dữ liệu **thuộc về lớp**, không phải của một đối tượng cụ thể nào.
  - Chỉ có **một bản sao duy nhất** của thành viên dữ liệu `static` tồn tại, chia sẻ cho tất cả các đối tượng của lớp đó (và ngay cả khi không có đối tượng nào được tạo).
  - Phải được **định nghĩa (và có thể khởi tạo) bên ngoài lớp**, thường trong file `.cpp`.
  - Truy cập bằng tên lớp và toán tử `::` (ví dụ: `ClassName::static_member`) hoặc thông qua một đối tượng (ví dụ: `object.static_member`, nhưng cách đầu tiên rõ ràng hơn).
- **Hàm thành viên `static` (Static Member Functions):**

  - Là hàm thành viên **thuộc về lớp**, không gắn với một đối tượng cụ thể.
  - **Không có con trỏ `this`**. Do đó, chúng chỉ có thể truy cập các thành viên `static` khác (dữ liệu hoặc hàm) của lớp đó trực tiếp. Chúng không thể truy cập các thành viên non-static (trừ khi được truyền một đối tượng cụ thể làm tham số).
  - Có thể được gọi mà không cần tạo đối tượng của lớp, dùng `ClassName::static_function()`.

  ```cpp
  #include <iostream>
  #include <string>

  class Entity {
  public:
      std::string name;
      int id;

      // Thành viên dữ liệu static
      static int entityCount; // Khai báo
      static const int MAX_ENTITIES = 100; // Static const int có thể khởi tạo trong lớp (C++17 cho inline static)

      Entity(const std::string& n) : name(n) {
          id = entityCount++; // Sử dụng và tăng biến static
          std::cout << "Entity created: " << name << " with ID " << id << std::endl;
      }

      ~Entity() {
          std::cout << "Entity destroyed: " << name << " with ID " << id << std::endl;
          // entityCount--; // Cẩn thận nếu destructor có thể được gọi nhiều lần hoặc không theo thứ tự
      }

      void display() const {
          std::cout << "ID: " << id << ", Name: " << name << std::endl;
      }

      // Hàm thành viên static
      static int getEntityCount() {
          // std::cout << name; // LỖI: không thể truy cập thành viên non-static 'name'
          // this->id; // LỖI: không có 'this' trong hàm static
          return entityCount;
      }

      static void printMaxEntities() {
          std::cout << "Max entities allowed: " << MAX_ENTITIES << std::endl;
      }
  };

  // Định nghĩa và khởi tạo thành viên dữ liệu static (thường trong file .cpp)
  int Entity::entityCount = 0;
  // const int Entity::MAX_ENTITIES; // Không cần nếu đã khởi tạo trong lớp như trên (C++17 inline static)

  int main() {
      std::cout << "Initial entity count: " << Entity::getEntityCount() << std::endl; // Output: 0
      Entity::printMaxEntities(); // Output: 100

      Entity e1("Player");
      Entity e2("Enemy");

      std::cout << "Current entity count (via class): " << Entity::entityCount << std::endl; // Output: 2
      std::cout << "Current entity count (via static func): " << Entity::getEntityCount() << std::endl; // Output: 2
      // std::cout << "Current entity count (via object e1): " << e1.entityCount << std::endl; // Cũng hoạt động

      e1.display();
      e2.display();

      if (Entity::getEntityCount() < Entity::MAX_ENTITIES) {
          Entity e3("NPC");
          std::cout << "Count after NPC: " << Entity::getEntityCount() << std::endl; // Output: 3
      }

      return 0;
  }
  ```

- **Sử dụng phổ biến:**
  - Biến `static` để đếm số lượng đối tượng được tạo.
  - Hằng số `static const` chung cho tất cả đối tượng (ví dụ: `MAX_ENTITIES`).
  - Hàm `static` như "utility functions" hoặc "factory methods" (sẽ học sau) không cần trạng thái của một đối tượng cụ thể.

Phần này đã đặt nền móng cho OOP. Các khái niệm như Kế thừa và Đa hình sẽ được xây dựng dựa trên những cơ sở này.

Hết Phần 5.

OK. Tiếp tục với Phần 6, đi sâu hơn vào các khía cạnh mạnh mẽ của OOP: Kế thừa và Đa hình.

**Phần 6: Lập Trình Hướng Đối Tượng (OOP) - Kế Thừa & Đa Hình**

**1. Kế Thừa (Inheritance)**

- **Khái niệm:** Kế thừa là một cơ chế cho phép một lớp (gọi là **lớp con - derived class / subclass**) thừa hưởng các thuộc tính (thành viên dữ liệu) và phương thức (hàm thành viên) từ một lớp khác (gọi là **lớp cha - base class / superclass**).
- **Mục đích:**
  - **Tái sử dụng mã (Code Reusability):** Lớp con không cần định nghĩa lại các thành viên đã có ở lớp cha.
  - **Tạo mối quan hệ "is-a" (là một):** Ví dụ, `Dog` "is an" `Animal`. Điều này giúp mô hình hóa thế giới thực một cách tự nhiên.
  - **Mở rộng chức năng:** Lớp con có thể thêm các thành viên dữ liệu và hàm thành viên mới, hoặc thay đổi (ghi đè) hành vi của các hàm được kế thừa từ lớp cha.
  - Nền tảng cho **Tính đa hình**.
- **Cú pháp kế thừa đơn (Single Inheritance):**
  `class DerivedClassName : access_specifier BaseClassName { ... };`

  - `access_specifier` (loại kế thừa): `public`, `protected`, hoặc `private`. Quyết định cách các thành viên `public` và `protected` của lớp cha được truy cập trong lớp con và từ bên ngoài lớp con.
    - **`public` inheritance (phổ biến nhất):**
      - Thành viên `public` của lớp cha trở thành `public` trong lớp con.
      - Thành viên `protected` của lớp cha trở thành `protected` trong lớp con.
      - Thành viên `private` của lớp cha **không thể truy cập trực tiếp** từ lớp con (chúng vẫn tồn tại trong đối tượng lớp con nhưng không thể truy cập).
      - Thể hiện rõ nhất mối quan hệ "is-a". Đối tượng của lớp con có thể được sử dụng ở bất kỳ đâu mong đợi đối tượng của lớp cha.
    - **`protected` inheritance:**
      - Thành viên `public` và `protected` của lớp cha đều trở thành `protected` trong lớp con.
      - Thành viên `private` của lớp cha không thể truy cập trực tiếp.
      - Ít phổ biến hơn.
    - **`private` inheritance:**
      - Thành viên `public` và `protected` của lớp cha đều trở thành `private` trong lớp con.
      - Thành viên `private` của lớp cha không thể truy cập trực tiếp.
      - Thể hiện mối quan hệ "is-implemented-in-terms-of" (được triển khai dựa trên). Không phải là "is-a". Đối tượng lớp con không thể thay thế đối tượng lớp cha một cách tự nhiên.
      - **Best Practice:** Ưu tiên composition (chứa một đối tượng của lớp khác làm thành viên) hơn `private` inheritance nếu chỉ muốn tái sử dụng code mà không có mối quan hệ "is-a".

  ```cpp
  #include <iostream>
  #include <string>

  // Lớp cha (Base Class)
  class Animal {
  protected: // Dùng protected để lớp con có thể truy cập trực tiếp
      std::string name;
      int age;

  public:
      Animal(const std::string& n, int a) : name(n), age(a) {
          std::cout << "Animal constructor called for " << name << std::endl;
      }

      ~Animal() {
          std::cout << "Animal destructor called for " << name << std::endl;
      }

      void eat() const {
          std::cout << name << " is eating." << std::endl;
      }

      void sleep() const {
          std::cout << name << " is sleeping." << std::endl;
      }

      std::string getName() const { return name; }
      int getAge() const { return age; }
  };

  // Lớp con (Derived Class) kế thừa public từ Animal
  class Dog : public Animal {
  private:
      std::string breed;

  public:
      // Constructor của lớp con gọi constructor của lớp cha
      Dog(const std::string& n, int a, const std::string& b)
          : Animal(n, a), breed(b) { // Gọi constructor Animal(n,a)
          // this->name = n; // Có thể truy cập name vì nó là protected trong Animal
          // Animal::name = n; // Cũng có thể truy cập như vậy
          std::cout << "Dog constructor called for " << name << std::endl;
      }

      ~Dog() {
          std::cout << "Dog destructor called for " << name << std::endl;
      }

      void bark() const {
          std::cout << name << " (a " << breed << ") is barking: Woof woof!" << std::endl;
      }

      // Ghi đè phương thức (Method Overriding) - sẽ nói kỹ hơn
      void eat() const { // Hàm này có cùng signature với Animal::eat()
          std::cout << name << " (Dog) is eating dog food." << std::endl;
          // Animal::eat(); // Nếu muốn gọi cả phiên bản của lớp cha
      }

      std::string getBreed() const { return breed; }
  };

  class Cat : public Animal {
  public:
      Cat(const std::string& n, int a) : Animal(n, a) {
          std::cout << "Cat constructor called for " << name << std::endl;
      }
      ~Cat() {
          std::cout << "Cat destructor called for " << name << std::endl;
      }
      void meow() const {
          std::cout << name << " is meowing: Meooow!" << std::endl;
      }
  };

  int main() {
      std::cout << "--- Creating Dog object ---" << std::endl;
      Dog myDog("Buddy", 3, "Golden Retriever");
      myDog.eat();    // Gọi Dog::eat() (phiên bản ghi đè)
      myDog.sleep();  // Gọi Animal::sleep() (kế thừa)
      myDog.bark();   // Hàm riêng của Dog
      std::cout << myDog.getName() << " is a " << myDog.getBreed() << std::endl;

      std::cout << "\n--- Creating Cat object ---" << std::endl;
      Cat myCat("Whiskers", 2);
      myCat.eat();    // Gọi Animal::eat() (không có ghi đè trong Cat)
      myCat.meow();

      std::cout << "\n--- Destructor calls ---" << std::endl;
      // Khi myCat và myDog ra khỏi scope, destructor của chúng được gọi.
      // Thứ tự: ~Dog -> ~Animal (cho myDog), ~Cat -> ~Animal (cho myCat)
      return 0;
  }
  // Thứ tự gọi Constructor: Base -> Derived
  // Thứ tự gọi Destructor: Derived -> Base (quan trọng!)
  ```

- **Gọi Constructor Lớp Cha:** Lớp con **phải** gọi constructor của lớp cha. Nếu không chỉ định rõ trong initializer list của constructor lớp con, trình biên dịch sẽ cố gắng gọi default constructor (`BaseClassName()`) của lớp cha. Nếu lớp cha không có default constructor, hoặc bạn muốn gọi một constructor cụ thể của lớp cha, bạn phải làm điều đó tường minh.
- **Access Control và Kế Thừa:**
  | Loại Kế Thừa | `public` member của Cha | `protected` member của Cha | `private` member của Cha |
  | :------------ | :----------------------- | :-------------------------- | :------------------------ |
  | `public` | `public` trong Con | `protected` trong Con | Không truy cập được |
  | `protected` | `protected` trong Con | `protected` trong Con | Không truy cập được |
  | `private` | `private` trong Con | `private` trong Con | Không truy cập được |

**2. Ghi Đè Phương Thức (Method Overriding)**

- Khi một lớp con cung cấp một định nghĩa cụ thể cho một hàm thành viên đã được định nghĩa ở lớp cha, với **cùng tên, cùng danh sách tham số, và cùng `const`/`volatile` qualifiers (nếu có)**, đó là ghi đè phương thức.
- **Mục đích:** Cho phép lớp con cung cấp hành vi chuyên biệt hơn cho một phương thức được kế thừa.
- Ví dụ: `Dog::eat()` ghi đè `Animal::eat()`.
- Để việc ghi đè hoạt động như mong đợi trong ngữ cảnh đa hình (polymorphism), hàm ở lớp cha thường được khai báo là `virtual`.

**3. Hàm Ảo (`virtual functions`) và Đa Hình (Polymorphism) lúc Runtime**

- **Đa hình (Polymorphism):** Nghĩa là "nhiều hình dạng". Trong OOP, đa hình cho phép đối xử với các đối tượng của các lớp con khác nhau thông qua một con trỏ hoặc tham chiếu kiểu lớp cha, và hành vi thực thi sẽ là của lớp con thực sự tại thời điểm chạy (runtime).
- **Hàm ảo (`virtual`):**

  - Từ khóa `virtual` được đặt trước khai báo của một hàm thành viên trong lớp cha.
  - Khi một hàm được khai báo là `virtual` trong lớp cha, và một lớp con ghi đè hàm đó, thì việc gọi hàm đó thông qua một con trỏ hoặc tham chiếu kiểu lớp cha sẽ dẫn đến việc thực thi phiên bản hàm của lớp con (nếu đối tượng thực sự là của lớp con). Đây gọi là **late binding** hoặc **dynamic dispatch**.
  - Nếu hàm không phải `virtual` (static binding), việc gọi qua con trỏ/tham chiếu lớp cha sẽ luôn gọi phiên bản của lớp cha, bất kể kiểu đối tượng thực sự.

  ```cpp
  #include <iostream>
  #include <string>
  #include <vector>

  class Shape {
  public:
      // Constructor
      Shape(const std::string& n = "Shape") : name(n) {}

      // Hàm ảo, để các lớp con có thể ghi đè
      virtual void draw() const {
          std::cout << "Drawing a generic " << name << "." << std::endl;
      }

      // Destructor ảo - RẤT QUAN TRỌNG khi dùng đa hình với cấp phát động
      virtual ~Shape() {
          std::cout << "Shape destructor for " << name << std::endl;
      }
      // Nếu không có virtual destructor, khi delete một đối tượng lớp con qua con trỏ lớp cha,
      // chỉ destructor của lớp cha được gọi -> memory leak/undefined behavior
      // nếu lớp con có tài nguyên cần giải phóng.

      std::string getName() const { return name; }
  protected:
      std::string name;
  };

  class Circle : public Shape {
  private:
      double radius;
  public:
      Circle(double r, const std::string& n = "Circle") : Shape(n), radius(r) {}

      // Ghi đè hàm draw (từ khóa 'override' là C++11, giúp kiểm tra)
      void draw() const override { // 'override' là tùy chọn nhưng rất nên dùng
          std::cout << "Drawing a " << name << " with radius " << radius << "." << std::endl;
      }

      ~Circle() override { // Ghi đè destructor
          std::cout << "Circle destructor for " << name << std::endl;
      }
  };

  class Rectangle : public Shape {
  private:
      double width, height;
  public:
      Rectangle(double w, double h, const std::string& n = "Rectangle") : Shape(n), width(w), height(h) {}

      void draw() const override {
          std::cout << "Drawing a " << name << " with width " << width << " and height " << height << "." << std::endl;
      }
      ~Rectangle() override {
          std::cout << "Rectangle destructor for " << name << std::endl;
      }
  };

  class Triangle : public Shape {
  public:
      Triangle(const std::string& n = "Triangle") : Shape(n) {}
      // Không ghi đè draw(), sẽ dùng phiên bản của Shape
      ~Triangle() override {
          std::cout << "Triangle destructor for " << name << std::endl;
      }
  };

  // Hàm nhận con trỏ tới Shape (có thể là Shape, Circle, Rectangle,...)
  void renderShape(const Shape* shapePtr) {
      if (shapePtr) {
          shapePtr->draw(); // Đây là dynamic dispatch nếu draw() là virtual
      }
  }

  void renderShapeRef(const Shape& shapeRef) {
      shapeRef.draw();
  }

  int main() {
      Shape s("Generic Shape");
      Circle c(5.0, "MyCircle");
      Rectangle r(4.0, 6.0, "MyRectangle");
      Triangle t("MyTriangle");

      std::cout << "\n--- Calling draw directly ---" << std::endl;
      s.draw(); // Shape::draw()
      c.draw(); // Circle::draw()
      r.draw(); // Rectangle::draw()
      t.draw(); // Shape::draw() (kế thừa)

      std::cout << "\n--- Polymorphism with pointers ---" << std::endl;
      Shape* shape_ptr;

      shape_ptr = &s;
      shape_ptr->draw(); // Shape::draw()

      shape_ptr = &c;
      shape_ptr->draw(); // Circle::draw() (vì draw là virtual)

      shape_ptr = &r;
      shape_ptr->draw(); // Rectangle::draw()

      shape_ptr = &t;
      shape_ptr->draw(); // Shape::draw()

      std::cout << "\n--- Polymorphism with references ---" << std::endl;
      renderShapeRef(s);
      renderShapeRef(c);
      renderShapeRef(r);
      renderShapeRef(t);

      std::cout << "\n--- Polymorphism with dynamic allocation and virtual destructor ---" << std::endl;
      std::vector<Shape*> shapes;
      shapes.push_back(new Circle(1.0, "Small Circle"));
      shapes.push_back(new Rectangle(2.0, 3.0, "Small Rectangle"));
      shapes.push_back(new Triangle("Small Triangle"));
      shapes.push_back(new Shape("Generic Dynamic Shape"));


      for (const Shape* shp : shapes) {
          shp->draw(); // Dynamic dispatch
      }

      std::cout << "\n--- Deleting dynamic shapes ---" << std::endl;
      for (Shape* shp : shapes) {
          delete shp; // QUAN TRỌNG: Nhờ virtual ~Shape(), destructor của lớp con thực sự sẽ được gọi
                      // sau đó là destructor của lớp cha.
                      // Nếu ~Shape() không virtual, chỉ ~Shape() được gọi -> memory leak cho Circle, Rectangle
      }
      shapes.clear();

      return 0;
  }
  ```

- **Từ khóa `override` (C++11):**
  - Đặt sau khai báo hàm ở lớp con.
  - Báo cho trình biên dịch biết rằng bạn có ý định ghi đè một hàm `virtual` từ lớp cha.
  - Nếu hàm ở lớp con không thực sự ghi đè một hàm `virtual` nào ở lớp cha (ví dụ, sai tên, sai tham số, hoặc hàm cha không `virtual`), trình biên dịch sẽ báo lỗi. Rất hữu ích để tránh lỗi đánh máy.
- **Từ khóa `final` (C++11):**
  - Khi áp dụng cho một hàm `virtual`: `virtual void func() const final;`
    - Hàm này không thể bị ghi đè thêm ở các lớp con của lớp hiện tại.
  - Khi áp dụng cho một lớp: `class MyClass final : public Base { ... };`
    - Lớp này không thể được dùng làm lớp cha (không thể có lớp nào kế thừa từ nó).

**4. Bảng Hàm Ảo (vtable - Virtual Table)**

- Để thực hiện dynamic dispatch, trình biên dịch thường sử dụng một cơ chế gọi là "virtual table" (vtable).
- Mỗi lớp có ít nhất một hàm `virtual` sẽ có một vtable riêng. vtable là một mảng các con trỏ hàm, trỏ đến các phiên bản `virtual` function đúng của lớp đó.
- Mỗi đối tượng của một lớp có hàm `virtual` sẽ chứa một con trỏ ẩn (thường gọi là **vptr - virtual pointer**) trỏ đến vtable của lớp đó.
- Khi gọi một hàm `virtual` thông qua con trỏ/tham chiếu lớp cha:
  1.  Truy cập vptr của đối tượng.
  2.  Sử dụng vptr để tìm vtable của lớp thực sự của đối tượng.
  3.  Trong vtable, tìm con trỏ hàm tương ứng với hàm được gọi.
  4.  Gọi hàm thông qua con trỏ đó.
- Điều này thêm một chút chi phí nhỏ (một vài dereference) cho mỗi lần gọi hàm `virtual` so với non-virtual, nhưng cho phép đa hình mạnh mẽ.

**5. Lớp Trừu Tượng (Abstract Classes) và Hàm Thuần Ảo (Pure Virtual Functions)**

- **Hàm Thuần Ảo (Pure Virtual Function):**
  - Là một hàm `virtual` trong lớp cha mà không có định nghĩa (implementation) và được gán `= 0;`.
  - Cú pháp: `virtual return_type functionName(params) const = 0;`
  - Bắt buộc các lớp con cụ thể (concrete derived classes) phải cung cấp định nghĩa cho hàm này.
- **Lớp Trừu Tượng (Abstract Class):**

  - Là một lớp chứa ít nhất một hàm thuần ảo.
  - **Không thể tạo đối tượng (instance) trực tiếp** từ một lớp trừu tượng.
  - Mục đích: Định nghĩa một interface chung (một "hợp đồng") mà các lớp con phải tuân theo. Lớp trừu tượng chỉ định "cái gì" cần làm, còn các lớp con cụ thể hóa "làm như thế nào".

  ```cpp
  #include <iostream>
  #include <string>
  #include <cmath> // Cho M_PI (có thể cần -D_USE_MATH_DEFINES trên Windows với MSVC)

  #ifndef M_PI // Định nghĩa nếu chưa có
  #define M_PI 3.14159265358979323846
  #endif

  // Lớp trừu tượng: Drawable (định nghĩa một interface)
  class Drawable {
  public:
      virtual void draw() const = 0; // Hàm thuần ảo
      virtual double getArea() const = 0; // Hàm thuần ảo
      virtual std::string getDescription() const = 0; // Hàm thuần ảo
      virtual ~Drawable() { std::cout << "Drawable destructor." << std::endl; } // Destructor ảo vẫn cần thiết
  };

  // Lớp con cụ thể: Circle
  class ConcreteCircle : public Drawable {
  private:
      double radius;
  public:
      ConcreteCircle(double r) : radius(r) {}
      void draw() const override {
          std::cout << "Drawing a circle with radius " << radius << "." << std::endl;
      }
      double getArea() const override {
          return M_PI * radius * radius;
      }
      std::string getDescription() const override {
          return "A circle with radius " + std::to_string(radius);
      }
      ~ConcreteCircle() override { std::cout << "ConcreteCircle destructor." << std::endl; }
  };

  // Lớp con cụ thể: ConcreteSquare
  class ConcreteSquare : public Drawable {
  private:
      double side;
  public:
      ConcreteSquare(double s) : side(s) {}
      void draw() const override {
          std::cout << "Drawing a square with side " << side << "." << std::endl;
      }
      double getArea() const override {
          return side * side;
      }
      std::string getDescription() const override {
          return "A square with side " + std::to_string(side);
      }
      ~ConcreteSquare() override { std::cout << "ConcreteSquare destructor." << std::endl; }
  };

  void displayItem(const Drawable& item) {
      std::cout << "--- Item Info ---" << std::endl;
      item.draw();
      std::cout << "Description: " << item.getDescription() << std::endl;
      std::cout << "Area: " << item.getArea() << std::endl;
      std::cout << "-----------------" << std::endl;
  }

  int main() {
      // Drawable d; // LỖI: không thể tạo đối tượng của lớp trừu tượng Drawable

      ConcreteCircle c1(5.0);
      ConcreteSquare s1(4.0);

      displayItem(c1);
      displayItem(s1);

      Drawable* p_drawable1 = new ConcreteCircle(2.5);
      Drawable* p_drawable2 = new ConcreteSquare(3.0);

      displayItem(*p_drawable1);
      displayItem(*p_drawable2);

      delete p_drawable1;
      delete p_drawable2;

      return 0;
  }
  ```

**6. `virtual destructor`**

- **Vấn đề:** Nếu bạn `delete` một đối tượng lớp con thông qua một con trỏ kiểu lớp cha, và lớp cha _không_ có destructor ảo (`virtual ~BaseClassName()`), thì chỉ destructor của lớp cha được gọi. Destructor của lớp con sẽ không được gọi. Điều này dẫn đến **Undefined Behavior**, thường là memory leak nếu lớp con quản lý tài nguyên (ví dụ, cấp phát động bộ nhớ).
- **Giải pháp:** Khai báo destructor của lớp cha là `virtual`.
  `virtual ~BaseClassName() { /* ... */ }`
- **Quy tắc:** Nếu một lớp có bất kỳ hàm `virtual` nào (hoặc dự định được dùng làm lớp cha trong kế thừa đa hình), nó **nên có một `virtual destructor`**. Ngay cả khi destructor đó không làm gì (`= default;` hoặc `{}`). Điều này đảm bảo dọn dẹp đúng cách khi đối tượng lớp con bị xóa thông qua con trỏ lớp cha.

**7. `dynamic_cast`, `static_cast` (trong ngữ cảnh kế thừa)**

- `static_cast`:
  - Có thể dùng để **upcast** (chuyển từ con trỏ/tham chiếu lớp con sang con trỏ/tham chiếu lớp cha) một cách an toàn. Thường thì upcast là ngầm định và không cần `static_cast`.
  - Có thể dùng để **downcast** (chuyển từ con trỏ/tham chiếu lớp cha sang con trỏ/tham chiếu lớp con). Tuy nhiên, `static_cast` cho downcast **không an toàn** vì nó không kiểm tra kiểu thực sự của đối tượng tại runtime. Nếu cast sai, sẽ dẫn đến UB. Chỉ dùng khi bạn _chắc chắn 100%_ rằng con trỏ lớp cha thực sự trỏ tới một đối tượng của lớp con đó.
- `dynamic_cast`:

  - Được thiết kế riêng cho downcasting **an toàn** trong hệ thống phân cấp kế thừa có hàm `virtual` (polymorphic base class).
  - Nó kiểm tra kiểu thực sự của đối tượng tại runtime.
  - Cú pháp: `dynamic_cast<DerivedType*>(base_ptr)` hoặc `dynamic_cast<DerivedType&>(base_ref)`
  - **Với con trỏ:** Nếu cast thành công (base_ptr thực sự trỏ tới một DerivedType hoặc lớp con của DerivedType), nó trả về con trỏ tới đối tượng DerivedType. Nếu thất bại, nó trả về `nullptr`.
  - **Với tham chiếu:** Nếu cast thành công, nó trả về tham chiếu tới đối tượng DerivedType. Nếu thất bại, nó ném một exception `std::bad_cast`.
  - Yêu cầu lớp cha phải có ít nhất một hàm `virtual` (để có vtable cho RTTI - Run-Time Type Information).

  ```cpp
  class Base { public: virtual ~Base() = default; virtual void identify() { std::cout << "I am Base\n"; } };
  class Derived1 : public Base { public: void identify() override { std::cout << "I am Derived1\n"; } void derived1_func() { std::cout << "Derived1 specific func\n"; } };
  class Derived2 : public Base { public: void identify() override { std::cout << "I am Derived2\n"; } void derived2_func() { std::cout << "Derived2 specific func\n"; } };

  void processBasePointer(Base* b_ptr) {
      if (!b_ptr) return;
      b_ptr->identify(); // Polymorphic call

      // Thử dynamic_cast
      Derived1* d1_ptr = dynamic_cast<Derived1*>(b_ptr);
      if (d1_ptr) { // Nếu b_ptr thực sự là Derived1
          d1_ptr->derived1_func();
      } else {
          std::cout << "b_ptr is not a Derived1 object.\n";
      }

      Derived2* d2_ptr = dynamic_cast<Derived2*>(b_ptr);
      if (d2_ptr) { // Nếu b_ptr thực sự là Derived2
          d2_ptr->derived2_func();
      } else {
          std::cout << "b_ptr is not a Derived2 object.\n";
      }
      std::cout << "-----\n";
  }

  int main() {
      Base* bp1 = new Derived1();
      Base* bp2 = new Derived2();
      Base* bp_base = new Base();

      processBasePointer(bp1);
      processBasePointer(bp2);
      processBasePointer(bp_base);
      processBasePointer(nullptr);

      // Ví dụ với tham chiếu và std::bad_cast
      Derived1 d1_obj;
      Base& base_ref = d1_obj;
      try {
          Derived1& ref_d1 = dynamic_cast<Derived1&>(base_ref); // OK
          ref_d1.derived1_func();

          Derived2& ref_d2 = dynamic_cast<Derived2&>(base_ref); // Sẽ ném std::bad_cast
          ref_d2.derived2_func(); // Không tới được đây
      } catch (const std::bad_cast& e) {
          std::cerr << "Exception: " << e.what() << std::endl;
      }

      delete bp1;
      delete bp2;
      delete bp_base;
      return 0;
  }
  ```

  - **Best Practice:** `dynamic_cast` có chi phí runtime. Nếu có thể thiết kế lại để tránh cần downcast (ví dụ, bằng cách thêm hàm `virtual` phù hợp vào lớp cha), đó thường là giải pháp tốt hơn. Tuy nhiên, `dynamic_cast` hữu ích trong một số tình huống khi cần biết kiểu cụ thể.

**8. Vấn Đề Cắt Đối Tượng (Object Slicing)**

- Xảy ra khi một đối tượng của lớp con được gán hoặc truyền bằng giá trị (pass-by-value) cho một đối tượng của lớp cha.
- Chỉ phần "lớp cha" của đối tượng lớp con được sao chép. Các thành viên dữ liệu và hành vi riêng của lớp con bị "cắt" mất.
- Hành vi đa hình cũng bị mất vì đối tượng giờ thực sự là kiểu lớp cha.

  ```cpp
  #include <iostream>
  #include <vector>

  class Parent {
  public:
      int parent_data;
      Parent(int d = 0) : parent_data(d) {}
      virtual void show() const {
          std::cout << "Parent data: " << parent_data << std::endl;
      }
      virtual ~Parent() = default;
  };

  class Child : public Parent {
  public:
      int child_data;
      Child(int pd, int cd) : Parent(pd), child_data(cd) {}
      void show() const override {
          std::cout << "Parent data: " << parent_data << ", Child data: " << child_data << std::endl;
      }
  };

  void processByValue(Parent p_val) { // NHẬN BẰNG GIÁ TRỊ -> OBJECT SLICING
      std::cout << "Inside processByValue: ";
      p_val.show(); // Sẽ luôn gọi Parent::show(), ngay cả khi truyền Child
  }

  void processByReference(const Parent& p_ref) { // OK, không slicing
      std::cout << "Inside processByReference: ";
      p_ref.show(); // Đa hình hoạt động
  }

  int main() {
      Parent p_obj(10);
      Child c_obj(20, 30);

      std::cout << "--- Direct calls ---" << std::endl;
      p_obj.show(); // Parent
      c_obj.show(); // Child

      std::cout << "\n--- Slicing example ---" << std::endl;
      Parent p_sliced = c_obj; // GÁN: c_obj bị "cắt" thành Parent, child_data mất
      p_sliced.show();      // Gọi Parent::show(), parent_data là 20

      std::cout << "\n--- Pass by value (slicing) ---" << std::endl;
      processByValue(p_obj);   // Truyền Parent, OK
      processByValue(c_obj);   // Truyền Child, c_obj bị cắt thành Parent trong hàm

      std::cout << "\n--- Pass by reference (no slicing) ---" << std::endl;
      processByReference(p_obj); // Truyền Parent, OK
      processByReference(c_obj); // Truyền Child, đa hình OK

      // Slicing cũng xảy ra với container chứa đối tượng (không phải con trỏ/tham chiếu)
      // std::vector<Parent> vec_parents;
      // vec_parents.push_back(p_obj);
      // vec_parents.push_back(c_obj); // c_obj bị sliced khi copy vào vector
      // for (const auto& item : vec_parents) {
      //     item.show(); // Luôn là Parent::show()
      // }

      return 0;
  }
  ```

- **Cách tránh Object Slicing:** Luôn làm việc với các đối tượng đa hình thông qua **con trỏ** hoặc **tham chiếu** kiểu lớp cha, không bao giờ bằng giá trị.

**9. Best Practices & Lưu Ý Chung (Phần 6)**

- **Ưu tiên `public` inheritance** để thể hiện mối quan hệ "is-a" và tận dụng đa hình.
- **Sử dụng `virtual destructor` trong lớp cha** nếu lớp đó có bất kỳ hàm `virtual` nào hoặc được dùng đa hình với cấp phát động.
- **Sử dụng `override` (C++11)** khi ghi đè hàm `virtual` để bắt lỗi sớm.
- **Sử dụng `final` (C++11)** để ngăn ghi đè thêm hoặc kế thừa thêm khi cần.
- **Lớp trừu tượng (Abstract Classes) với hàm thuần ảo** là cách tuyệt vời để định nghĩa interface.
- **Tránh Object Slicing** bằng cách dùng con trỏ hoặc tham chiếu khi làm việc với hệ thống phân cấp kế thừa đa hình.
- **Liskov Substitution Principle (LSP):** Các đối tượng của lớp con phải có thể thay thế các đối tượng của lớp cha mà không làm thay đổi tính đúng đắn của chương trình. `public` inheritance nên tuân thủ LSP.
- **Composition over Inheritance:** Cân nhắc sử dụng composition (một lớp chứa đối tượng của lớp khác làm thành viên) thay vì kế thừa nếu mối quan hệ không phải là "is-a" mà là "has-a" (có một) hoặc "is-implemented-in-terms-of". Composition thường linh hoạt và ít ràng buộc hơn.
  - Ví dụ: `Car` "has an" `Engine`. Thay vì `Car` kế thừa từ `Engine` (vô lý), `Car` nên có một thành viên dữ liệu kiểu `Engine`.

Kế thừa và Đa hình là những công cụ rất mạnh, nhưng cần được sử dụng một cách có chủ đích và hiểu biết để tránh các thiết kế phức tạp hoặc dễ lỗi.

Hết Phần 6.

OK. Tiếp tục với Phần 7, nơi chúng ta sẽ khám phá các khía cạnh nâng cao hơn của OOP trong C++ và một số idioms quan trọng.

**Phần 7: Lập Trình Hướng Đối Tượng (OOP) - Nâng Cao & Idioms**

**1. Nạp Chồng Toán Tử (Operator Overloading)**

- **Khái niệm:** C++ cho phép bạn định nghĩa lại (nạp chồng) hành vi của hầu hết các toán tử chuẩn (như `+`, `-`, `*`, `/`, `[]`, `()`, `=`, `<<`, `>>`, `==`, `!=`, etc.) cho các kiểu dữ liệu do người dùng định nghĩa (lớp hoặc struct).
- **Mục đích:** Làm cho việc sử dụng các đối tượng của lớp bạn trở nên tự nhiên và trực quan hơn, giống như làm việc với các kiểu dữ liệu cơ bản.
- **Cú pháp:**
  - **Là hàm thành viên (member function):**
    `return_type operatorSYMBOL(parameters);`
    - Toán tử một ngôi (unary, ví dụ `operator-()` cho phủ định, `operator++()`): Không có tham số nếu là tiền tố, có một tham số `int` giả (dummy) nếu là hậu tố (`operator++(int)`).
    - Toán tử hai ngôi (binary, ví dụ `operator+()`): Có một tham số (đối tượng bên phải của toán tử). Đối tượng bên trái là `*this`.
  - **Là hàm không phải thành viên (non-member function, thường là `friend`):**
    `return_type operatorSYMBOL(parameter1, parameter2);`
    - Toán tử một ngôi: Có một tham số.
    - Toán tử hai ngôi: Có hai tham số (toán hạng trái và phải).
- **Các toán tử không thể nạp chồng:** `.` (truy cập thành viên), `.*` (truy cập thành viên qua con trỏ), `::` (phân giải phạm vi), `?:` (điều kiện tam ngôi), `sizeof`, `typeid`, các `static_cast`, `dynamic_cast`, `const_cast`, `reinterpret_cast`.
- **Lưu ý quan trọng:**
  - Không thể thay đổi độ ưu tiên (precedence) hoặc tính kết hợp (associativity) của toán tử.
  - Không thể tạo ra toán tử mới.
  - Ít nhất một trong các toán hạng phải là kiểu do người dùng định nghĩa (để tránh nạp chồng cho các kiểu cơ bản).
  - Nên giữ cho hành vi của toán tử nạp chồng gần giống với ý nghĩa tự nhiên của nó để tránh gây nhầm lẫn.

**Ví dụ: Nạp chồng toán tử cho lớp `Complex` (số phức)**

```cpp
#include <iostream>

class Complex {
private:
    double real;
    double imag;

public:
    Complex(double r = 0.0, double i = 0.0) : real(r), imag(i) {}

    // Getter
    double getReal() const { return real; }
    double getImag() const { return imag; }

    // 1. Nạp chồng toán tử + (là hàm thành viên)
    // c1 + c2  =>  c1.operator+(c2)
    Complex operator+(const Complex& other) const {
        Complex result;
        result.real = this->real + other.real;
        result.imag = this->imag + other.imag;
        return result;
    }

    // 2. Nạp chồng toán tử - (là hàm thành viên, cho c1 - c2)
    Complex operator-(const Complex& other) const {
        return Complex(this->real - other.real, this->imag - other.imag);
    }

    // 3. Nạp chồng toán tử * (là hàm thành viên, cho c1 * c2)
    // (a+bi)*(c+di) = (ac-bd) + (ad+bc)i
    Complex operator*(const Complex& other) const {
        double res_real = (this->real * other.real) - (this->imag * other.imag);
        double res_imag = (this->real * other.imag) + (this->imag * other.real);
        return Complex(res_real, res_imag);
    }

    // 4. Nạp chồng toán tử == (là hàm thành viên)
    bool operator==(const Complex& other) const {
        return (this->real == other.real) && (this->imag == other.imag);
    }

    // 5. Nạp chồng toán tử != (có thể dựa trên ==)
    bool operator!=(const Complex& other) const {
        return !(*this == other); // Gọi toán tử == đã nạp chồng
    }

    // 6. Nạp chồng toán tử += (là hàm thành viên, thường trả về *this)
    Complex& operator+=(const Complex& other) {
        this->real += other.real;
        this->imag += other.imag;
        return *this; // Trả về tham chiếu tới đối tượng hiện tại để cho phép chaining
    }

    // 7. Nạp chồng toán tử - (unary, phủ định, là hàm thành viên)
    // -c1 => c1.operator-()
    Complex operator-() const {
        return Complex(-this->real, -this->imag);
    }

    // 8. Nạp chồng toán tử ++ (tiền tố, là hàm thành viên)
    // ++c1 => c1.operator++()
    Complex& operator++() {
        this->real++;
        this->imag++; // Hoặc chỉ real++ tùy ý nghĩa
        return *this;
    }

    // 9. Nạp chồng toán tử ++ (hậu tố, là hàm thành viên)
    // c1++ => c1.operator++(0)  (tham số int giả)
    // Thường trả về bản sao của giá trị trước khi tăng
    Complex operator++(int) {
        Complex temp = *this; // Lưu trạng thái hiện tại
        ++(*this);            // Gọi toán tử ++ tiền tố để thực hiện tăng
        return temp;          // Trả về trạng thái cũ
    }

    // 10. Nạp chồng toán tử << (output stream, phải là non-member, thường là friend)
    // cout << c1
    // Nếu là non-member: operator<<(cout, c1)
    // Để truy cập private members 'real', 'imag', cần là friend
    friend std::ostream& operator<<(std::ostream& os, const Complex& c);

    // 11. Nạp chồng toán tử >> (input stream, phải là non-member, thường là friend)
    // cin >> c1
    friend std::istream& operator>>(std::istream& is, Complex& c);
};

// Định nghĩa non-member friend functions
std::ostream& operator<<(std::ostream& os, const Complex& c) {
    os << "(" << c.real << (c.imag >= 0 ? "+" : "") << c.imag << "i)";
    return os;
}

std::istream& operator>>(std::istream& is, Complex& c) {
    char sign, i_char;
    // Giả sử format là (real_val+imag_val i) hoặc (real_val-imag_val i)
    // Đây là một parser đơn giản, thực tế có thể phức tạp hơn
    // is >> ch_open >> c.real >> sign >> c.imag >> i_char >> ch_close;
    std::cout << "Enter real part: ";
    is >> c.real;
    std::cout << "Enter imaginary part: ";
    is >> c.imag;
    return is;
}

// Nạp chồng toán tử + cho trường hợp double + Complex
// d + c1 => operator+(d, c1)
// Phải là non-member vì toán hạng trái (double) không phải là class type
Complex operator+(double d, const Complex& c) {
    return Complex(d + c.getReal(), c.getImag());
}
// Tương tự cho Complex + double (nếu không muốn dùng hàm thành viên)
// Complex operator+(const Complex& c, double d) {
//     return Complex(c.getReal() + d, c.getImag());
// }
// Tuy nhiên, Complex::operator+(const Complex&) có thể xử lý trường hợp này nếu double
// có thể ngầm chuyển đổi thành Complex (ví dụ, Complex có constructor Complex(double r)).

int main() {
    Complex c1(2.0, 3.0);
    Complex c2(1.0, -1.0);

    std::cout << "c1 = " << c1 << std::endl; // (2+3i)
    std::cout << "c2 = " << c2 << std::endl; // (1-1i)

    Complex sum = c1 + c2; // c1.operator+(c2)
    std::cout << "c1 + c2 = " << sum << std::endl; // (3+2i)

    Complex sum_double_first = 5.0 + c1; // operator+(5.0, c1)
    std::cout << "5.0 + c1 = " << sum_double_first << std::endl; // (7+3i)

    Complex diff = c1 - c2;
    std::cout << "c1 - c2 = " << diff << std::endl; // (1+4i)

    Complex prod = c1 * c2;
    std::cout << "c1 * c2 = " << prod << std::endl; // (2 - (-3)) + (-2 + 3)i = (5+1i)

    if (c1 == Complex(2.0, 3.0)) {
        std::cout << "c1 is equal to (2+3i)." << std::endl;
    }

    c1 += c2;
    std::cout << "c1 after += c2: " << c1 << std::endl; // (3+2i)

    std::cout << "-c1 = " << -c1 << std::endl; // (-3-2i)

    std::cout << "++c1 (prefix): " << ++c1 << std::endl; // (4+3i), c1 is now (4+3i)
    std::cout << "c1 after prefix ++: " << c1 << std::endl; // (4+3i)

    std::cout << "c1++ (postfix): " << c1++ << std::endl; // (4+3i), c1 is now (5+4i)
    std::cout << "c1 after postfix ++: " << c1 << std::endl; // (5+4i)

    Complex c3;
    std::cout << "Enter a complex number for c3: ";
    std::cin >> c3;
    std::cout << "You entered c3 = " << c3 << std::endl;

    return 0;
}
```

- **Khi nào dùng member vs. non-member (friend)?**
  - **Member function:**
    - Khi toán tử thay đổi trạng thái của đối tượng (ví dụ: `+=`, `-=`, `++`, `--`, `=`).
    - Khi toán tử yêu cầu truy cập `private`/`protected` members và bạn không muốn dùng `friend`.
    - Toán tử `[]` (subscript), `()` (function call), `->` (member access through pointer), và toán tử gán `=` **phải** là member functions.
  - **Non-member (thường là `friend` nếu cần truy cập private/protected):**
    - Khi toán tử hai ngôi có toán hạng trái không phải là đối tượng của lớp (ví dụ: `double d + Complex c;` thì `operator+` không thể là member của `Complex`).
    - Các toán tử đối xứng (ví dụ: `a + b` nên giống `b + a`). Nếu `operator+` là member của `A` (cho `a+b`), thì `b+a` sẽ không tự động hoạt động nếu `b` không có hàm `operator+` tương ứng hoặc không có chuyển đổi ngầm. Non-member function cho phép cả hai toán hạng được đối xử bình đẳng.
    - Toán tử `<<` (output stream) và `>>` (input stream) là ví dụ điển hình phải là non-member vì toán hạng trái là `std::ostream&` hoặc `std::istream&`.

**2. Hàm Bạn (Friend Functions) và Lớp Bạn (Friend Classes)**

- **`friend`:** Là một cơ chế cho phép một hàm hoặc một lớp khác truy cập vào các thành viên `private` và `protected` của lớp khai báo nó là bạn.
- Tình bạn **không có tính bắc cầu** (A là bạn của B, B là bạn của C thì không suy ra A là bạn của C).
- Tình bạn **không được kế thừa**.
- **Hàm bạn (Friend Function):**
  - Một hàm không phải là thành viên của lớp nhưng được phép truy cập các thành viên private/protected của lớp đó.
  - Khai báo bên trong lớp với từ khóa `friend`.
  - Định nghĩa bên ngoài lớp (như một hàm thông thường).
  - Ví dụ: `operator<<` và `operator>>` cho lớp `Complex` ở trên.
- **Lớp bạn (Friend Class):**

  - Tất cả các hàm thành viên của lớp bạn đều có thể truy cập các thành viên `private` và `protected` của lớp khai báo tình bạn.
  - Khai báo: `friend class OtherClassName;`

  ```cpp
  class B; // Forward declaration

  class A {
  private:
      int data_A = 10;
      friend void showDataA(const A& obj_a); // Hàm bạn
      friend class B; // Lớp B là bạn của A
  public:
      A() {}
  };

  class B {
  public:
      void accessA(const A& obj_a) {
          // Lớp B có thể truy cập private member của A
          std::cout << "Class B accessing A's private data: " << obj_a.data_A << std::endl;
          // obj_a.data_A = 20; // Có thể sửa nếu obj_a không const
      }
  };

  void showDataA(const A& obj_a) {
      // Hàm showDataA có thể truy cập private member của A
      std::cout << "Friend function showDataA accessing A's data: " << obj_a.data_A << std::endl;
  }

  int main_friend() { // Đổi tên để không trùng main
      A a_obj;
      B b_obj;

      showDataA(a_obj);
      b_obj.accessA(a_obj);

      return 0;
  }
  ```

- **Best Practice:** Sử dụng `friend` một cách hạn chế. Nó phá vỡ tính đóng gói ở một mức độ nào đó. Chỉ dùng khi thực sự cần thiết và không có giải pháp thay thế tốt hơn (ví dụ, `operator<<` cho stream output là một trường hợp điển hình).

**3. Multiple Inheritance (Đa Kế Thừa) và Vấn Đề "Diamond Problem"**

- **Đa kế thừa:** Cho phép một lớp con kế thừa từ nhiều hơn một lớp cha trực tiếp.
  `class Derived : public Base1, public Base2, private Base3 { ... };`
- **Ưu điểm:** Có thể kết hợp các chức năng từ nhiều nguồn khác nhau.
- **Nhược điểm và Vấn đề:**
  - **Tăng độ phức tạp:** Khó quản lý, dễ gây nhầm lẫn.
  - **Xung đột tên (Name Ambiguity):** Nếu nhiều lớp cha có thành viên cùng tên, lớp con phải chỉ định rõ muốn dùng phiên bản nào (ví dụ: `Base1::member_name`).
  - **Diamond Problem (Vấn đề Kim Cương):**
    - Xảy ra khi một lớp (`D`) kế thừa từ hai lớp (`B` và `C`), mà cả `B` và `C` lại cùng kế thừa từ một lớp cha chung (`A`).
    - Khi đó, đối tượng của lớp `D` sẽ có _hai bản sao_ của các thành viên từ `A` (một qua `B`, một qua `C`). Điều này gây mơ hồ và lãng phí bộ nhớ.
    - Sơ đồ:
      ```
            A
           / \
          B   C
           \ /
            D
      ```
- **Giải quyết Diamond Problem bằng `virtual` Inheritance (Kế thừa ảo):**

  - Khi `B` và `C` kế thừa từ `A` sử dụng `virtual public A` (hoặc `virtual protected/private A`), thì lớp `D` sẽ chỉ chứa một bản sao duy nhất của các thành viên từ `A`.
  - Lớp cha chung (`A` trong ví dụ) được gọi là "virtual base class".
  - Việc khởi tạo virtual base class được thực hiện bởi lớp con "xa nhất" trong cây kế thừa (lớp `D` trong ví dụ).

  ```cpp
  #include <iostream>

  class PoweredDevice { // Lớp cha chung nhất
  public:
      bool power_status = false;
      PoweredDevice() { std::cout << "PoweredDevice constructor\n"; }
      virtual ~PoweredDevice() = default;
      void powerOn() { power_status = true; std::cout << "Device powered ON.\n"; }
      void powerOff() { power_status = false; std::cout << "Device powered OFF.\n"; }
  };

  // Scanner kế thừa ảo từ PoweredDevice
  class Scanner : virtual public PoweredDevice {
  public:
      Scanner() { std::cout << "Scanner constructor\n"; }
      void scan() {
          if (power_status) std::cout << "Scanning document...\n";
          else std::cout << "Scanner is off. Cannot scan.\n";
      }
  };

  // Printer kế thừa ảo từ PoweredDevice
  class Printer : virtual public PoweredDevice {
  public:
      Printer() { std::cout << "Printer constructor\n"; }
      void print() {
          if (power_status) std::cout << "Printing document...\n";
          else std::cout << "Printer is off. Cannot print.\n";
      }
  };

  // Copier kế thừa từ Scanner và Printer
  class Copier : public Scanner, public Printer {
  public:
      Copier() { std::cout << "Copier constructor\n"; }
      void copy() {
          if (power_status) { // Chỉ có một power_status nhờ virtual inheritance
              std::cout << "Copying document...\n";
              scan();  // Gọi Scanner::scan
              print(); // Gọi Printer::print
          } else {
              std::cout << "Copier is off. Cannot copy.\n";
          }
      }
  };

  int main_diamond() {
      Copier myCopier;
      // Thứ tự constructor: PoweredDevice -> Scanner -> Printer -> Copier
      // (Virtual base được khởi tạo trước)

      myCopier.powerOn(); // Chỉ có 1 hàm powerOn() được gọi trên 1 power_status
      myCopier.copy();
      myCopier.powerOff();

      // Nếu không dùng virtual inheritance với Scanner và Printer từ PoweredDevice:
      // myCopier.Scanner::powerOn(); // Sẽ có 2 powerOn khác nhau
      // myCopier.Printer::powerOn();
      // và sẽ có 2 thành viên power_status.

      return 0;
  }
  ```

- **Best Practice:** Cân nhắc kỹ trước khi dùng đa kế thừa. Nó có thể làm phức tạp thiết kế. Thường thì composition hoặc kế thừa từ các lớp interface (lớp trừu tượng chỉ chứa hàm thuần ảo) là lựa chọn tốt hơn, ít rủi ro hơn. Nếu dùng đa kế thừa, hiểu rõ virtual inheritance khi gặp diamond problem.

**4. RAII (Resource Acquisition Is Initialization) - Idiom Quan Trọng**

- **Khái niệm:** RAII là một idiom (mẫu thiết kế) lập trình trong C++ (và các ngôn ngữ khác có destructor) để quản lý tài nguyên (bộ nhớ, file, mutex, kết nối mạng, etc.) một cách an toàn và tự động.
- **Nguyên tắc:**
  1.  **Resource Acquisition (Thu thập tài nguyên):** Tài nguyên được thu thập (cấp phát, mở, khóa) trong constructor của một đối tượng.
  2.  **Initialization (Khởi tạo):** Đối tượng được khởi tạo và chịu trách nhiệm quản lý tài nguyên đó.
  3.  **Resource Release (Giải phóng tài nguyên):** Tài nguyên được giải phóng tự động trong destructor của đối tượng đó khi đối tượng ra khỏi scope (bất kể ra khỏi scope bình thường hay do exception).
- **Lợi ích:**
  - **An toàn với Exception (Exception Safety):** Đảm bảo tài nguyên luôn được giải phóng ngay cả khi có exception xảy ra, vì destructor luôn được gọi trong quá trình stack unwinding.
  - **Tự động hóa:** Giảm thiểu lỗi do quên giải phóng tài nguyên (memory leaks, file handles không đóng, mutex không unlock).
  - **Code sạch hơn:** Logic quản lý tài nguyên được gói gọn trong lớp quản lý.
- **Ví dụ kinh điển:** Smart pointers (`std::unique_ptr`, `std::shared_ptr`), `std::lock_guard`, `std::fstream`.

  ```cpp
  #include <iostream>
  #include <fstream> // Cho std::fstream
  #include <vector>
  #include <mutex>   // Cho std::mutex, std::lock_guard
  #include <stdexcept> // Cho std::runtime_error

  // Ví dụ 1: Quản lý file đơn giản bằng RAII
  class FileHandler {
  private:
      std::fstream file_stream;
      std::string filename;
  public:
      FileHandler(const std::string& fname, std::ios_base::openmode mode)
          : filename(fname) {
          file_stream.open(filename, mode); // Thu thập tài nguyên (mở file)
          if (!file_stream.is_open()) {
              throw std::runtime_error("Failed to open file: " + filename);
          }
          std::cout << "File opened: " << filename << std::endl;
      }

      ~FileHandler() { // Giải phóng tài nguyên
          if (file_stream.is_open()) {
              file_stream.close();
              std::cout << "File closed: " << filename << std::endl;
          }
      }

      // Các hàm làm việc với file...
      void write(const std::string& data) {
          if (file_stream.is_open()) {
              file_stream << data << std::endl;
          }
      }
      // ...
  };

  // Ví dụ 2: std::lock_guard là một ví dụ RAII cho mutex
  std::mutex mtx;
  int shared_resource = 0;

  void critical_section_raii(int id) {
      // Thu thập tài nguyên (lock mutex) khi lock được tạo
      std::lock_guard<std::mutex> lock(mtx); // RAII: lock() trong constructor, unlock() trong destructor

      // Critical section
      shared_resource++;
      std::cout << "Thread " << id << " accessed shared_resource, new value: " << shared_resource << std::endl;
      if (id == 1 && shared_resource > 1) {
           // Giả sử có exception
           // throw std::runtime_error("Something went wrong in critical section from thread 1");
           // Dù có exception, destructor của lock_guard vẫn được gọi, mutex được unlock
      }
      // Mutex tự động được unlock khi 'lock' ra khỏi scope (destructor của lock_guard được gọi)
  }


  int main_raii() {
      try {
          FileHandler fh("my_raii_file.txt", std::ios::out | std::ios::app);
          fh.write("Hello from RAII FileHandler!");
          fh.write("Another line.");
          // Khi fh ra khỏi scope (cuối try block), destructor được gọi, file tự đóng
          // Nếu constructor ném exception, destructor sẽ không được gọi (vì đối tượng chưa hoàn thành)
          // nhưng file cũng chưa được mở thành công.
      } catch (const std::exception& e) {
          std::cerr << "Exception caught: " << e.what() << std::endl;
      }

      // std::thread t1(critical_section_raii, 1);
      // std::thread t2(critical_section_raii, 2);
      // t1.join();
      // t2.join();
      // Bạn sẽ thấy mutex được quản lý đúng cách.

      return 0;
  }
  ```

- **Best Practice:** **LUÔN LUÔN** sử dụng RAII để quản lý tài nguyên trong C++. Đây là một trong những idiom mạnh mẽ và quan trọng nhất của ngôn ngữ. Khi có thể, hãy dùng các lớp RAII có sẵn của thư viện chuẩn. Nếu không, hãy tự viết lớp RAII của riêng bạn.

**5. Một Số Idioms C++ Khác (Giới Thiệu Sơ Lược)**

- **Pimpl (Pointer to Implementation) / Compilation Firewall / Cheshire Cat:**
  - **Mục đích:** Giảm sự phụ thuộc biên dịch (compile-time dependencies) và che giấu hoàn toàn chi tiết triển khai private của một lớp.
  - **Cách làm:** Lớp public (`Widget`) chỉ chứa một con trỏ (`std::unique_ptr<Impl> pimpl;`) đến một lớp `Impl` được định nghĩa riêng (thường trong file `.cpp`). Tất cả các thành viên private và logic triển khai được đặt trong lớp `Impl`.
  - **Lợi ích:**
    - Thay đổi triển khai trong `Impl` không yêu cầu biên dịch lại các file sử dụng `Widget` (miễn là interface public của `Widget` không đổi).
    - Giấu hoàn toàn các header private, chỉ cần forward declaration cho `Impl` trong header của `Widget`.
  - Sẽ chi tiết hơn nếu cần.
- **CRTP (Curiously Recurring Template Pattern):**
  - **Mục đích:** Đạt được "static polymorphism" (đa hình tại thời điểm biên dịch), tránh chi phí của virtual functions.
  - **Cách làm:** Một lớp cha template (`Base<Derived>`) nhận lớp con (`Derived`) làm tham số template. Lớp con kế thừa từ `Base<Derived>`. Lớp cha có thể `static_cast` con trỏ `this` của nó thành `Derived*` để gọi các hàm của lớp con.
  - **Lợi ích:** Hiệu năng cao hơn vtable, nhưng ít linh hoạt hơn (kiểu phải biết tại compile time).
  - Thường dùng để thêm chức năng chung cho các lớp con (ví dụ: counter, comparison operators).
  - Sẽ chi tiết hơn nếu cần.

Phần này đã chạm đến một số kỹ thuật và mẫu thiết kế nâng cao hơn trong C++ OOP. Chúng giúp viết code mạnh mẽ, an toàn và dễ bảo trì hơn.

Hết Phần 7.

OK. Tiếp tục với Phần 8, chúng ta sẽ khám phá Templates và Lập trình Generic trong C++.

**Phần 8: Templates và Lập Trình Generic**

Lập trình generic (Generic Programming) là một mô hình lập trình cho phép viết các hàm và lớp hoạt động với nhiều kiểu dữ liệu khác nhau mà không cần viết lại mã cho từng kiểu. Trong C++, templates là cơ chế chính để đạt được điều này.

**1. Function Templates (Khuôn Mẫu Hàm)**

- **Khái niệm:** Cho phép định nghĩa một "khuôn mẫu" cho một hàm, nơi một hoặc nhiều kiểu dữ liệu không được xác định trước (gọi là template parameters). Trình biên dịch sẽ tự động tạo ra các phiên bản cụ thể của hàm đó khi nó được gọi với các kiểu dữ liệu khác nhau.
- **Cú pháp:**
  ```cpp
  template <typename T> // hoặc template <class T> - typename và class ở đây là tương đương
  return_type function_name(parameters_list) {
      // Thân hàm, sử dụng T như một kiểu dữ liệu
  }
  ```
  - `T` là một **template type parameter** (tham số kiểu của khuôn mẫu). Bạn có thể đặt tên khác (ví dụ: `U`, `MyType`).
  - Có thể có nhiều tham số kiểu: `template <typename T1, typename T2, ...>`
- **Ví dụ:**

  ```cpp
  #include <iostream>
  #include <string>
  #include <vector>

  // 1. Khuôn mẫu hàm tìm giá trị lớn nhất
  template <typename T>
  T getMax(T a, T b) {
      return (a > b) ? a : b;
  }

  // 2. Khuôn mẫu hàm in một giá trị
  template <typename T>
  void printValue(T val) {
      std::cout << "Value: " << val << std::endl;
  }

  // 3. Khuôn mẫu hàm với nhiều tham số kiểu
  template <typename T1, typename T2>
  void printPair(T1 first, T2 second) {
      std::cout << "Pair: (" << first << ", " << second << ")" << std::endl;
  }

  // 4. Khuôn mẫu hàm trả về kiểu khác với tham số
  template <typename T, typename U>
  U castAndAdd(T val1, T val2) {
      return static_cast<U>(val1) + static_cast<U>(val2);
  }


  // Ví dụ với một lớp tự định nghĩa
  class Point {
  public:
      int x, y;
      Point(int _x, int _y) : x(_x), y(_y) {}

      // Cần nạp chồng toán tử > để getMax hoạt động với Point
      bool operator>(const Point& other) const {
          // Giả sử so sánh dựa trên tổng x+y
          return (x + y) > (other.x + other.y);
      }
      // Cần nạp chồng toán tử << để printValue hoạt động với Point
      friend std::ostream& operator<<(std::ostream& os, const Point& p);
  };
  std::ostream& operator<<(std::ostream& os, const Point& p) {
      os << "(" << p.x << "," << p.y << ")";
      return os;
  }


  int main() {
      // Sử dụng getMax
      std::cout << "Max of 5, 10 is " << getMax(5, 10) << std::endl;         // T suy ra là int
      std::cout << "Max of 3.14, 2.71 is " << getMax(3.14, 2.71) << std::endl; // T suy ra là double
      std::cout << "Max of 'a', 'z' is " << getMax('a', 'z') << std::endl;   // T suy ra là char
      std::cout << "Max of strings: " << getMax(std::string("hello"), std::string("world")) << std::endl; // T suy ra là std::string

      Point p1(1,2), p2(3,0);
      Point p_max = getMax(p1, p2); // T suy ra là Point (cần operator>)
      std::cout << "Max Point: " << p_max << std::endl;

      // Nếu kiểu không thể suy ra hoặc muốn ép kiểu tường minh
      std::cout << "Max of 5, 10.5 (as double): " << getMax<double>(5, 10.5) << std::endl;

      // Sử dụng printValue
      printValue(100);                 // T là int
      printValue(3.14159);             // T là double
      printValue("Hello Template!");   // T là const char*
      printValue(std::string("C++")); // T là std::string
      printValue(p1);                  // T là Point (cần operator<<)

      // Sử dụng printPair
      printPair(123, "Text");         // T1 là int, T2 là const char*
      printPair(2.5, Point(5,5));   // T1 là double, T2 là Point

      // Sử dụng castAndAdd
      double sum_double = castAndAdd<int, double>(5, 6); // T là int, U là double. Kết quả: 11.0
      std::cout << "Cast and add (int to double): " << sum_double << std::endl;
      int sum_int = castAndAdd<double, int>(5.5, 6.7); // T là double, U là int. Kết quả: 5 + 6 = 11 (do ép kiểu)
      std::cout << "Cast and add (double to int): " << sum_int << std::endl;

      return 0;
  }
  ```

- **Template Argument Deduction (Suy diễn đối số khuôn mẫu):** Trình biên dịch cố gắng tự động suy ra kiểu `T` (hoặc các tham số kiểu khác) từ các đối số được truyền cho hàm.
- **Explicit Template Arguments (Chỉ định đối số khuôn mẫu tường minh):** Đôi khi cần chỉ định rõ kiểu cho tham số khuôn mẫu, ví dụ `getMax<double>(5, 10.5);` hoặc khi kiểu trả về là một tham số khuôn mẫu mà không thể suy ra từ các đối số hàm.

**2. Class Templates (Khuôn Mẫu Lớp)**

- **Khái niệm:** Cho phép định nghĩa một "khuôn mẫu" cho một lớp, nơi một hoặc nhiều kiểu dữ liệu (hoặc giá trị hằng) là tham số của khuôn mẫu. Trình biên dịch sẽ tạo ra các phiên bản lớp cụ thể khi bạn sử dụng khuôn mẫu lớp với các đối số khuôn mẫu cụ thể.
- **Cú pháp:**

  ```cpp
  template <typename T> // hoặc template <class T>
  class ClassName {
  private:
      T member_data;
      // ...
  public:
      ClassName(T data);
      T getData() const;
      void setData(T data);
      // ... các hàm thành viên khác sử dụng T
  };

  // Định nghĩa hàm thành viên bên ngoài khuôn mẫu lớp
  template <typename T>
  ClassName<T>::ClassName(T data) : member_data(data) {
      // ...
  }

  template <typename T>
  T ClassName<T>::getData() const {
      return member_data;
  }
  // ...
  ```

- **Ví dụ: Khuôn mẫu lớp `Stack`**

  ```cpp
  #include <iostream>
  #include <vector>
  #include <string>
  #include <stdexcept> // Cho std::out_of_range, std::logic_error

  template <typename T, size_t MaxSize = 100> // T là type param, MaxSize là non-type param
  class Stack {
  private:
      std::vector<T> elements; // Sử dụng std::vector để lưu trữ đơn giản hơn
                               // Hoặc có thể dùng mảng T data[MaxSize];

  public:
      Stack() {
          elements.reserve(MaxSize); // Có thể pre-allocate nếu dùng vector
      }

      bool isEmpty() const {
          return elements.empty();
      }

      bool isFull() const {
          // Nếu dùng std::vector, isFull ít có ý nghĩa trừ khi có giới hạn logic
          // return elements.size() >= MaxSize; // Ví dụ nếu dùng mảng cố định hoặc giới hạn
          return false; // Giả sử vector không có giới hạn cứng ngoài MaxSize cho reserve
      }

      void push(const T& item) {
          if (elements.size() < MaxSize) { // Kiểm tra nếu có giới hạn cứng
              elements.push_back(item);
          } else {
              throw std::overflow_error("Stack overflow");
          }
      }

      T pop() {
          if (isEmpty()) {
              throw std::underflow_error("Stack underflow: cannot pop from empty stack");
          }
          T top_item = elements.back();
          elements.pop_back();
          return top_item;
      }

      const T& top() const {
          if (isEmpty()) {
              throw std::logic_error("Stack is empty: cannot peek top");
          }
          return elements.back();
      }

      size_t size() const {
          return elements.size();
      }
  };


  int main_class_template() {
      // Tạo Stack cho kiểu int với MaxSize mặc định (100)
      Stack<int> intStack;
      intStack.push(10);
      intStack.push(20);
      intStack.push(30);

      std::cout << "Top of intStack: " << intStack.top() << std::endl; // 30
      std::cout << "Popped from intStack: " << intStack.pop() << std::endl; // 30
      std::cout << "New top of intStack: " << intStack.top() << std::endl; // 20
      std::cout << "Size of intStack: " << intStack.size() << std::endl; // 2

      // Tạo Stack cho kiểu std::string với MaxSize là 5
      Stack<std::string, 5> stringStack;
      stringStack.push("Hello");
      stringStack.push("World");
      stringStack.push("C++");

      while (!stringStack.isEmpty()) {
          std::cout << "Popped from stringStack: " << stringStack.pop() << std::endl;
      }

      try {
          stringStack.pop(); // Sẽ ném std::underflow_error
      } catch (const std::exception& e) {
          std::cerr << "Exception caught: " << e.what() << std::endl;
      }

      // Stack<Point> pointStack; // Sẽ hoạt động nếu Point có default constructor, copy constructor...
      // pointStack.push(Point(1,2));

      return 0;
  }
  ```

- **Instantiation (Khởi tạo khuôn mẫu):** Khi bạn sử dụng một khuôn mẫu lớp với các đối số cụ thể (ví dụ: `Stack<int>`), trình biên dịch sẽ tạo ra một phiên bản lớp thực sự từ khuôn mẫu đó.
- **Định nghĩa hàm thành viên template:** Nếu định nghĩa hàm thành viên bên ngoài khai báo lớp, bạn phải lặp lại `template <...>` và sử dụng `ClassName<T>::`
- **File Header và File Source:** Do cơ chế hoạt động của template (cần source code đầy đủ để khởi tạo tại điểm sử dụng), **định nghĩa của khuôn mẫu hàm và khuôn mẫu lớp (bao gồm cả định nghĩa các hàm thành viên của nó) thường được đặt hoàn toàn trong file header (`.h` hoặc `.hpp`)**. Nếu chia ra file `.cpp`, trình biên dịch ở các file khác include header sẽ không thấy định nghĩa và gây lỗi linker. Có một số cách giải quyết (explicit instantiation, export keyword - đã bị deprecated) nhưng cách phổ biến nhất là đặt tất cả trong header.

**3. Non-Type Template Parameters (Tham Số Khuôn Mẫu Không Phải Kiểu)**

- Ngoài tham số kiểu (`typename T`), template cũng có thể nhận các tham số không phải kiểu, ví dụ: `int`, `size_t`, con trỏ, tham chiếu tới đối tượng/hàm có external linkage, hoặc enum.
- Giá trị của non-type parameter phải là một **hằng số tại thời điểm biên dịch (compile-time constant)**.
- Ví dụ: `MaxSize` trong `Stack<T, size_t MaxSize>` ở trên. `std::array<T, N>` là một ví dụ điển hình trong STL.

  ```cpp
  template <typename T, int Size>
  class FixedArray {
  private:
      T arr[Size]; // Kích thước mảng được xác định tại compile-time
      int current_size = 0;
  public:
      void add(const T& val) {
          if (current_size < Size) arr[current_size++] = val;
      }
      void print() const {
          for (int i = 0; i < current_size; ++i) std::cout << arr[i] << " ";
          std::cout << std::endl;
      }
      // ...
  };

  // FixedArray<int, 10> myIntArray; // Mảng 10 int
  // FixedArray<double, 5> myDoubleArray; // Mảng 5 double
  ```

**4. Template Specialization (Chuyên Biệt Hóa Khuôn Mẫu)**

- Đôi khi, bạn muốn cung cấp một triển khai hoàn toàn khác cho một khuôn mẫu khi nó được sử dụng với một (hoặc một tập hợp) kiểu cụ thể. Đây là lúc dùng template specialization.
- **Full Template Specialization (Chuyên biệt hóa hoàn toàn):** Cung cấp một định nghĩa riêng cho một tập hợp cụ thể tất cả các tham số khuôn mẫu.

  - **Function Template Specialization:**

    ```cpp
    template <typename T>
    void print(T value) { // Khuôn mẫu chung
        std::cout << "Generic print: " << value << std::endl;
    }

    // Chuyên biệt hóa hoàn toàn cho const char*
    template <> // Danh sách tham số khuôn mẫu rỗng
    void print<const char*>(const char* value) {
        std::cout << "Specialized print for const char*: \"" << value << "\"" << std::endl;
    }

    // Chuyên biệt hóa hoàn toàn cho bool
    template <>
    void print<bool>(bool value) {
        std::cout << "Specialized print for bool: " << (value ? "true" : "false") << std::endl;
    }

    // main() {
    //     print(10);         // Gọi generic
    //     print("hello");    // Gọi specialized for const char*
    //     print(true);       // Gọi specialized for bool
    // }
    ```

  - **Class Template Specialization:**

    ```cpp
    template <typename T>
    class Container {
    public:
        Container(T val) { std::cout << "Generic Container for T" << std::endl; }
    };

    // Chuyên biệt hóa hoàn toàn cho Container<int>
    template <>
    class Container<int> {
    public:
        Container(int val) { std::cout << "Specialized Container for int, value: " << val << std::endl; }
    };

    // main() {
    //    Container<double> cd(3.14); // Dùng generic
    //    Container<int> ci(100);    // Dùng specialized
    // }
    ```

- **Partial Template Specialization (Chuyên biệt hóa một phần - chỉ cho Class Templates):**

  - Cho phép chuyên biệt hóa một khuôn mẫu lớp bằng cách cố định một số tham số khuôn mẫu hoặc áp đặt các ràng buộc lên chúng (ví dụ, chuyên biệt hóa cho tất cả các kiểu con trỏ).
  - Không áp dụng cho function templates (thay vào đó, dùng function overloading).

  ```cpp
  template <typename T, typename U>
  class Pair {
  public:
      Pair() { std::cout << "Generic Pair<T, U>\n"; }
  };

  // Chuyên biệt hóa một phần: cả hai kiểu giống nhau
  template <typename T>
  class Pair<T, T> {
  public:
      Pair() { std::cout << "Partial specialization Pair<T, T> (same types)\n"; }
  };

  // Chuyên biệt hóa một phần: kiểu thứ hai là int
  template <typename T>
  class Pair<T, int> {
  public:
      Pair() { std::cout << "Partial specialization Pair<T, int>\n"; }
  };

  // Chuyên biệt hóa một phần: cả hai kiểu là con trỏ
  template <typename T, typename U>
  class Pair<T*, U*> {
  public:
      Pair() { std::cout << "Partial specialization Pair<T*, U*> (both pointers)\n"; }
  };

  // Chuyên biệt hóa hoàn toàn (dựa trên một chuyên biệt hóa một phần ở trên)
  template <>
  class Pair<int, int> { // Đây là chuyên biệt hóa hoàn toàn của Pair<T,T> với T=int
  public:                // hoặc của Pair<T,int> với T=int
      Pair() { std::cout << "Full specialization Pair<int, int>\n"; }
  };

  // main() {
  //     Pair<double, char> p1; // Generic Pair<T, U>
  //     Pair<double, double> p2; // Partial Pair<T, T>
  //     Pair<char, int> p3;    // Partial Pair<T, int>
  //     Pair<int*, double*> p4; // Partial Pair<T*, U*>
  //     Pair<int, int> p5;     // Full Pair<int, int>
  // }
  ```

**5. Variadic Templates (Khuôn Mẫu Đa Tham Số - C++11)**

- Cho phép một khuôn mẫu (hàm hoặc lớp) nhận một số lượng **biến đổi** (variable number) các đối số khuôn mẫu.
- Sử dụng `...` (ellipsis) để biểu thị một "parameter pack".
- Thường được xử lý bằng đệ quy (recursive template instantiation) hoặc folding expressions (C++17).
- Rất hữu ích cho các hàm như `printf`-style functions, tuple implementation, `std::make_unique`, `std::make_shared`.

  ```cpp
  #include <iostream>
  #include <string>

  // 1. Base case cho đệ quy (khi không còn tham số)
  void printVariadic() {
      std::cout << std::endl; // Kết thúc bằng xuống dòng
  }

  // 2. Khuôn mẫu hàm variadic (đệ quy)
  template <typename T, typename... Args> // T là tham số đầu, Args là parameter pack còn lại
  void printVariadic(T first, Args... rest) {
      std::cout << first;
      if (sizeof...(rest) > 0) { // Kiểm tra nếu còn tham số
          std::cout << ", ";
      }
      printVariadic(rest...); // Gọi đệ quy với các tham số còn lại (pack expansion)
  }

  // Ví dụ với folding expression (C++17)
  template <typename... Args>
  void printFold(Args... args) {
      // ((std::cout << args << " "), ...); // Unary right fold
      // (... , (std::cout << args << " ")); // Unary left fold (cẩn thận thứ tự)
      // Hoặc in từng cái một với dấu phẩy (cần phức tạp hơn một chút để xử lý dấu phẩy cuối)
      auto printWithComma = [&](const auto& arg) {
           std::cout << arg;
      };
      const char* sep = "";
      ( (std::cout << sep, printWithComma(args), sep = ", "), ... );
      std::cout << std::endl;
  }

  template<typename... Args>
  auto sumVariadic(Args... args) {
      return (args + ... + 0); // Fold expression: (arg1 + (arg2 + (... + 0)))
                               // Hoặc (args + ... ) nếu chắc chắn có ít nhất 1 arg và + được định nghĩa
  }


  int main_variadic() {
      printVariadic("Values:", 1, 2.5, "hello", 'c');
      // Output: Values:, 1, 2.5, hello, c

      printVariadic(100); // Output: 100
      printVariadic();    // Output: (xuống dòng)

      std::cout << "Using fold expression:" << std::endl;
      printFold("Fold:", 10, 3.14, std::string("world"));

      std::cout << "Sum: " << sumVariadic(1, 2, 3, 4, 5) << std::endl; // Output: 15
      std::cout << "Sum: " << sumVariadic(1.1, 2.2, 3.3) << std::endl; // Output: 6.6

      return 0;
  }
  ```

**6. Template Metaprogramming (TMP - Lập Trình Siêu Khuôn Mẫu - Giới Thiệu Cơ Bản)**

- **Khái niệm:** Là một kỹ thuật lập trình trong đó các khuôn mẫu được sử dụng để thực hiện các phép tính **tại thời điểm biên dịch (compile-time)**. Mã khuôn mẫu được trình biên dịch "thực thi" để tạo ra mã hoặc tính toán các giá trị.
- **Đặc điểm:**
  - Không có vòng lặp, biến thay đổi trạng thái như lập trình runtime.
  - Tính toán thường dựa trên đệ quy khuôn mẫu và chuyên biệt hóa.
- **Ví dụ: Tính giai thừa tại compile-time**

  ```cpp
  #include <iostream>

  // Khuôn mẫu chung (đệ quy)
  template <int N>
  struct Factorial {
      static const unsigned long long value = N * Factorial<N - 1>::value;
      // enum { value = N * Factorial<N - 1>::value }; // Cách cũ hơn
  };

  // Chuyên biệt hóa khuôn mẫu cho trường hợp cơ sở (N=0)
  template <>
  struct Factorial<0> {
      static const unsigned long long value = 1;
      // enum { value = 1 };
  };

  int main_tmp() {
      // Các giá trị này được tính toán hoàn toàn tại thời điểm biên dịch
      const unsigned long long fact5 = Factorial<5>::value; // 120
      const unsigned long long fact10 = Factorial<10>::value; // 3628800
      // const unsigned long long fact0 = Factorial<0>::value; // 1 (dùng chuyên biệt hóa)

      std::cout << "Factorial of 5: " << fact5 << std::endl;
      std::cout << "Factorial of 10: " << fact10 << std::endl;
      // std::cout << "Factorial of 20: " << Factorial<20>::value << std::endl; // Có thể tràn số nếu không dùng kiểu lớn

      // Trình biên dịch có thể thay thế Factorial<5>::value trực tiếp bằng 120 trong mã máy
      return 0;
  }
  ```

- TMP có thể rất mạnh mẽ (tối ưu hóa, kiểm tra tĩnh, tạo mã) nhưng cũng có thể làm code khó đọc và tăng thời gian biên dịch. Thư viện `<type_traits>` của C++11 sử dụng rất nhiều TMP.

**7. Best Practices & Lưu Ý Chung (Phần 8)**

- **Đặt định nghĩa template trong file header.**
- **Sử dụng `typename` thay vì `class`** cho tham số kiểu trong khai báo template (ví dụ: `template <typename T>`) để rõ ràng hơn, mặc dù `class` vẫn hợp lệ. `typename` là bắt buộc khi bạn muốn chỉ định một tên phụ thuộc (dependent name) là một kiểu.
  ```cpp
  template <typename C>
  void processContainer(const C& container) {
      // typename C::value_type item; // Cần 'typename' vì C::value_type là dependent type
      // ...
  }
  ```
- **Cẩn thận với "code bloat":** Mỗi lần một khuôn mẫu được khởi tạo với một tập hợp đối số khuôn mẫu mới, trình biên dịch sẽ tạo ra một bản sao mã. Nếu dùng với quá nhiều kiểu khác nhau, kích thước mã thực thi có thể tăng lên.
- **Cung cấp thông báo lỗi rõ ràng:** Lỗi biên dịch liên quan đến template đôi khi rất dài và khó hiểu. Thiết kế template tốt và sử dụng `static_assert` (C++11) có thể giúp.
- **`Concepts` (C++20):** Là một cải tiến lớn cho template, cho phép định nghĩa các ràng buộc (constraints) trên các tham số khuôn mẫu một cách rõ ràng. Điều này giúp cải thiện thông báo lỗi, tăng tính dễ đọc và cho phép nạp chồng dựa trên concepts. (Sẽ giới thiệu ở phần C++ Hiện Đại).
- **Sử dụng template khi bạn thực sự cần tính generic.** Đừng lạm dụng nếu một giải pháp non-template đơn giản hơn.

Templates là một trong những tính năng mạnh mẽ nhất và phức tạp nhất của C++. Chúng là nền tảng của Thư viện Template Chuẩn (STL), mà chúng ta sẽ khám phá ở các phần tiếp theo.

Hết Phần 8.
OK. Tiếp tục với Phần 9, chúng ta sẽ bắt đầu khám phá Thư Viện Template Chuẩn (STL), tập trung vào các Containers.

**Phần 9: Thư Viện Template Chuẩn (STL) - Containers**

Thư viện Template Chuẩn (Standard Template Library - STL) là một tập hợp các khuôn mẫu lớp (class templates) và khuôn mẫu hàm (function templates) mạnh mẽ trong C++, cung cấp các cấu trúc dữ liệu và thuật toán phổ biến, hiệu quả và đã được kiểm thử kỹ lưỡng.

STL có thể được chia thành các thành phần chính:

1.  **Containers (Lưu trữ):** Các khuôn mẫu lớp dùng để lưu trữ tập hợp các đối tượng.
2.  **Iterators (Bộ duyệt):** Các đối tượng hoạt động giống như con trỏ, dùng để duyệt qua các phần tử trong container.
3.  **Algorithms (Thuật toán):** Các khuôn mẫu hàm thực hiện các thao tác trên các dãy phần tử (thường được định nghĩa bởi iterators), ví dụ: sắp xếp, tìm kiếm, sao chép.
4.  **Function Objects (Functors/Callables):** Các đối tượng có thể được gọi như hàm (nạp chồng `operator()`).
5.  **Allocators (Bộ cấp phát - ít dùng trực tiếp):** Quản lý việc cấp phát và giải phóng bộ nhớ cho containers.

**1. Tổng Quan về Containers trong STL**

Containers STL được thiết kế để quản lý việc lưu trữ các phần tử một cách hiệu quả. Chúng tự động quản lý bộ nhớ và cung cấp các interface nhất quán.

Có ba loại container chính:

- **Sequence Containers (Lưu trữ tuần tự):** Duy trì thứ tự của các phần tử theo cách chúng được chèn vào.
  - `std::vector`
  - `std::deque` (double-ended queue)
  - `std::list` (doubly-linked list)
  - `std::forward_list` (singly-linked list - C++11)
  - `std::array` (fixed-size array - C++11)
- **Associative Containers (Lưu trữ kết hợp):** Lưu trữ các phần tử đã được sắp xếp theo một tiêu chí (thường là khóa), cho phép tìm kiếm nhanh.
  - `std::set` (tập hợp các khóa duy nhất, đã sắp xếp)
  - `std::map` (ánh xạ khóa-giá trị, khóa duy nhất, đã sắp xếp)
  - `std::multiset` (tập hợp các khóa, có thể trùng lặp, đã sắp xếp)
  - `std::multimap` (ánh xạ khóa-giá trị, khóa có thể trùng lặp, đã sắp xếp)
- **Unordered Associative Containers (Lưu trữ kết hợp không có thứ tự - C++11):** Lưu trữ các phần tử sử dụng bảng băm (hash tables), cho phép tìm kiếm, chèn, xóa với độ phức tạp trung bình O(1). Thứ tự phần tử không được đảm bảo.
  - `std::unordered_set`
  - `std::unordered_map`
  - `std::unordered_multiset`
  - `std::unordered_multimap`
- **Container Adapters (Bộ điều hợp lưu trữ):** Cung cấp một interface khác (thường là hạn chế hơn) cho một container tuần tự cơ bản.
  - `std::stack` (LIFO - Last In First Out)
  - `std::queue` (FIFO - First In First Out)
  - `std::priority_queue` (hàng đợi ưu tiên - phần tử lớn nhất/nhỏ nhất luôn ở đầu)

**2. Sequence Containers (Lưu trữ Tuần tự)**

- **`std::vector<T>` (Header: `<vector>`)**

  - **Mô tả:** Mảng động (dynamic array). Lưu trữ các phần tử liên tiếp trong bộ nhớ, cho phép truy cập ngẫu nhiên nhanh (O(1)) bằng `operator[]` hoặc `at()`.
  - **Tự động thay đổi kích thước:** Khi vector đầy và cần chèn thêm phần tử, nó sẽ cấp phát một vùng nhớ mới lớn hơn, sao chép các phần tử cũ sang, rồi giải phóng vùng nhớ cũ.
  - **Chèn/Xóa:**
    - `push_back(value)`: Chèn vào cuối (O(1) trung bình, O(N) tệ nhất nếu cần reallocate).
    - `pop_back()`: Xóa ở cuối (O(1)).
    - `insert(iterator_pos, value)`: Chèn vào vị trí `pos` (O(N) vì các phần tử sau phải dịch chuyển).
    - `erase(iterator_pos)` hoặc `erase(iterator_first, iterator_last)`: Xóa (O(N)).
  - **Truy cập:** `[]`, `at()`, `front()`, `back()`.
  - **Kích thước:** `size()`, `capacity()`, `empty()`, `resize()`, `reserve()`.
  - **Khi nào dùng:** Lựa chọn mặc định cho sequence container nếu không có lý do đặc biệt để dùng loại khác. Tốt cho truy cập ngẫu nhiên và chèn/xóa ở cuối.

  ```cpp
  #include <iostream>
  #include <vector>
  #include <string>
  #include <algorithm> // Cho std::sort, std::find

  int main_vector() {
      // Khởi tạo vector
      std::vector<int> v1;                             // Vector rỗng
      std::vector<int> v2 = {1, 2, 3, 4, 5};           // Initializer list (C++11)
      std::vector<std::string> v3(5, "hello");       // 5 chuỗi "hello"
      std::vector<int> v4(v2);                         // Copy constructor

      // Thêm phần tử
      v1.push_back(10);
      v1.push_back(20);
      v1.push_back(30); // v1: {10, 20, 30}

      // Truy cập phần tử
      std::cout << "v1[0]: " << v1[0] << std::endl;         // Không kiểm tra biên
      std::cout << "v1.at(1): " << v1.at(1) << std::endl;   // Kiểm tra biên, ném std::out_of_range nếu sai
      std::cout << "v2 front: " << v2.front() << ", back: " << v2.back() << std::endl;

      // Kích thước và dung lượng
      std::cout << "v1 size: " << v1.size() << ", capacity: " << v1.capacity() << std::endl;
      v1.reserve(100); // Yêu cầu dung lượng ít nhất 100
      std::cout << "v1 after reserve: size: " << v1.size() << ", capacity: " << v1.capacity() << std::endl;

      // Duyệt vector
      std::cout << "v2 elements (index-based for): ";
      for (size_t i = 0; i < v2.size(); ++i) {
          std::cout << v2[i] << " ";
      }
      std::cout << std::endl;

      std::cout << "v2 elements (range-based for): ";
      for (int val : v2) {
          std::cout << val << " ";
      }
      std::cout << std::endl;

      std::cout << "v3 elements (iterator-based for): ";
      for (std::vector<std::string>::iterator it = v3.begin(); it != v3.end(); ++it) {
          std::cout << *it << " ";
      }
      std::cout << std::endl;

      // Chèn và xóa
      v2.insert(v2.begin() + 2, 99); // v2: {1, 2, 99, 3, 4, 5}
      std::cout << "v2 after insert: "; for(int x:v2) std::cout << x << " "; std::cout << std::endl;

      v2.erase(v2.begin() + 3);      // Xóa phần tử tại vị trí 3 (số 3) -> v2: {1, 2, 99, 4, 5}
      std::cout << "v2 after erase: "; for(int x:v2) std::cout << x << " "; std::cout << std::endl;

      v1.pop_back(); // Xóa 30. v1: {10, 20}
      std::cout << "v1 after pop_back: "; for(int x:v1) std::cout << x << " "; std::cout << std::endl;

      v1.clear(); // Xóa tất cả phần tử
      std::cout << "v1 is empty: " << (v1.empty() ? "true" : "false") << std::endl;

      // Sắp xếp vector (sử dụng thuật toán STL)
      std::vector<int> unsorted_v = {5, 1, 4, 2, 8};
      std::sort(unsorted_v.begin(), unsorted_v.end()); // Sắp xếp tăng dần
      std::cout << "Sorted vector: "; for(int x:unsorted_v) std::cout << x << " "; std::cout << std::endl;

      return 0;
  }
  ```

- **`std::deque<T>` (Double-Ended Queue, Header: `<deque>`)**

  - **Mô tả:** Hàng đợi hai đầu. Tương tự `vector` nhưng hỗ trợ chèn/xóa hiệu quả ở cả **đầu và cuối** (O(1) trung bình).
  - Không lưu trữ các phần tử hoàn toàn liên tiếp trong một khối bộ nhớ duy nhất (thường là một tập các khối nhỏ liên kết với nhau). Điều này làm cho việc chèn/xóa ở giữa vẫn O(N) nhưng có thể nhanh hơn `vector` một chút do không cần sao chép toàn bộ.
  - Truy cập ngẫu nhiên `operator[]` và `at()` vẫn O(1) nhưng chậm hơn `vector` một chút.
  - **Thêm/Xóa:** `push_front()`, `pop_front()`, `push_back()`, `pop_back()`.
  - **Khi nào dùng:** Khi cần chèn/xóa thường xuyên ở cả đầu và cuối.

- **`std::list<T>` (Doubly-Linked List, Header: `<list>`)**

  - **Mô tả:** Danh sách liên kết đôi. Mỗi phần tử lưu trữ con trỏ tới phần tử trước và sau nó.
  - **Chèn/Xóa:** Rất hiệu quả ở bất kỳ vị trí nào nếu bạn đã có iterator trỏ tới vị trí đó (O(1)). Việc tìm iterator đến vị trí đó có thể là O(N).
  - **Truy cập:** Không hỗ trợ truy cập ngẫu nhiên nhanh (`operator[]` hoặc `at()` không có). Chỉ có thể duyệt tuần tự (O(N) để đến phần tử thứ i).
  - **Iterators:** Iterators của `list` không bị vô hiệu hóa (invalidate) khi chèn/xóa các phần tử khác (trừ iterator trỏ tới chính phần tử bị xóa). Điều này khác với `vector` và `deque`.
  - **Hàm thành viên đặc biệt:** `splice()`, `sort()`, `merge()`, `remove()`, `unique()`.
  - **Khi nào dùng:** Khi cần chèn/xóa thường xuyên ở giữa danh sách và không cần truy cập ngẫu nhiên. Khi tính ổn định của iterator quan trọng.

- **`std::forward_list<T>` (Singly-Linked List, Header: `<forward_list>`, C++11)**

  - **Mô tả:** Danh sách liên kết đơn. Mỗi phần tử chỉ lưu con trỏ tới phần tử tiếp theo.
  - **Hiệu quả bộ nhớ hơn `std::list`** một chút.
  - Chỉ hỗ trợ duyệt tiến. Chèn/xóa hiệu quả sau một phần tử đã biết (O(1)).
  - Interface hạn chế hơn `std::list` (ví dụ, không có `push_back()`, `pop_back()`, `size()`). Có `push_front()`, `pop_front()`, `insert_after()`, `erase_after()`.
  - **Khi nào dùng:** Khi cần danh sách liên kết và hiệu quả bộ nhớ là tối quan trọng, và chỉ cần duyệt tiến.

- **`std::array<T, N>` (Fixed-size array, Header: `<array>`, C++11)**

  - **Mô tả:** Mảng tĩnh có kích thước cố định `N` được xác định tại thời điểm biên dịch. `N` là non-type template parameter.
  - Kết hợp sự hiệu quả của mảng C-style (lưu trữ trên stack nếu khai báo cục bộ, các phần tử liên tiếp) với interface của container STL (hàm `size()`, iterators, `at()`, `front()`, `back()`).
  - Không thể thay đổi kích thước sau khi tạo.
  - **Khi nào dùng:** Khi bạn biết kích thước của mảng tại thời điểm biên dịch và không cần thay đổi kích thước. An toàn hơn mảng C-style.

  ```cpp
  #include <array>
  #include <iostream>

  int main_array_stl() {
      std::array<int, 5> arr = {11, 22, 33, 44, 55};

      std::cout << "arr size: " << arr.size() << std::endl;
      std::cout << "arr[1]: " << arr[1] << std::endl;
      std::cout << "arr.at(2): " << arr.at(2) << std::endl;

      for (int val : arr) {
          std::cout << val << " ";
      }
      std::cout << std::endl;

      // arr.push_back(66); // LỖI: std::array không có push_back
      return 0;
  }
  ```

**3. Associative Containers (Lưu trữ Kết hợp - Đã Sắp Xếp)**

- Lưu trữ các phần tử theo một thứ tự được sắp xếp dựa trên khóa.
- Thường được triển khai bằng cây đỏ-đen (Red-Black trees) hoặc cấu trúc cây cân bằng tương tự.
- **Độ phức tạp chèn, xóa, tìm kiếm:** O(log N).
- Yêu cầu kiểu khóa phải có `operator<` được định nghĩa (hoặc bạn cung cấp một functor so sánh tùy chỉnh).

- **`std::set<Key>` (Header: `<set>`)**

  - **Mô tả:** Lưu trữ một tập hợp các khóa **duy nhất**, được sắp xếp.
  - **Hàm chính:** `insert()`, `erase()`, `find()`, `count()`, `lower_bound()`, `upper_bound()`.

  ```cpp
  #include <iostream>
  #include <set>
  #include <string>

  int main_set() {
      std::set<int> s;
      s.insert(30);
      s.insert(10);
      s.insert(50);
      s.insert(20);
      s.insert(10); // 10 đã có, không chèn thêm

      std::cout << "Set elements: "; // Sẽ in ra theo thứ tự: 10 20 30 50
      for (int val : s) {
          std::cout << val << " ";
      }
      std::cout << std::endl;

      auto it_find = s.find(20);
      if (it_find != s.end()) {
          std::cout << "Found 20 in set." << std::endl;
      }

      s.erase(30); // Xóa giá trị 30
      std::cout << "Set after erasing 30: ";
      for (int val : s) std::cout << val << " ";
      std::cout << std::endl;

      std::cout << "Count of 10: " << s.count(10) << std::endl; // 1 (vì duy nhất)
      std::cout << "Count of 100: " << s.count(100) << std::endl; // 0

      return 0;
  }
  ```

- **`std::map<Key, Value>` (Header: `<map>`)**

  - **Mô tả:** Lưu trữ các cặp khóa-giá trị (key-value pairs). Các khóa là **duy nhất** và được sắp xếp.
  - Mỗi phần tử là một `std::pair<const Key, Value>`.
  - **Truy cập/Chèn:**
    - `operator[](key)`: Nếu `key` tồn tại, trả về tham chiếu tới giá trị tương ứng. Nếu `key` không tồn tại, nó sẽ **chèn một phần tử mới** với `key` đó và giá trị được khởi tạo mặc định (value-initialized), sau đó trả về tham chiếu tới giá trị mới này. Cẩn thận khi dùng để đọc nếu không muốn chèn.
    - `at(key)`: Tương tự `[]` nhưng nếu `key` không tồn tại, ném `std::out_of_range`. An toàn hơn cho việc đọc.
    - `insert(std::make_pair(key, value))` hoặc `insert({key, value})` (C++11).

  ```cpp
  #include <iostream>
  #include <map>
  #include <string>

  int main_map() {
      std::map<std::string, int> student_scores;

      // Chèn
      student_scores["Alice"] = 90; // Sử dụng operator[]
      student_scores.insert(std::make_pair("Bob", 85));
      student_scores.insert({"Charlie", 92}); // C++11 initializer list for pair

      student_scores["Alice"] = 95; // Cập nhật giá trị của Alice

      std::cout << "Score of Alice: " << student_scores["Alice"] << std::endl;
      try {
          std::cout << "Score of David (using at): " << student_scores.at("David") << std::endl;
      } catch (const std::out_of_range& e) {
          std::cerr << "David not found: " << e.what() << std::endl;
      }
      // std::cout << student_scores["David"]; // Sẽ chèn David với score = 0 nếu không có

      std::cout << "\nAll student scores (sorted by name):" << std::endl;
      for (const auto& pair : student_scores) { // auto là std::pair<const std::string, int>
          std::cout << pair.first << ": " << pair.second << std::endl;
      }

      auto it = student_scores.find("Bob");
      if (it != student_scores.end()) {
          std::cout << "\nFound Bob, score: " << it->second << std::endl;
          student_scores.erase(it); // Xóa Bob
      }

      std::cout << "\nScores after erasing Bob:" << std::endl;
      for (const auto& pair : student_scores) {
          std::cout << pair.first << ": " << pair.second << std::endl;
      }
      return 0;
  }
  ```

- **`std::multiset<Key>` (Header: `<set>`)**

  - Tương tự `std::set`, nhưng cho phép lưu trữ các khóa **trùng lặp**.
  - `count(key)` có thể trả về giá trị > 1.
  - `find(key)` trả về iterator tới một trong các phần tử có khóa đó (không đảm bảo là cái nào nếu có nhiều).
  - `equal_range(key)` trả về một cặp iterator (first, last) đánh dấu phạm vi của tất cả các phần tử có khóa đó.

- **`std::multimap<Key, Value>` (Header: `<map>`)**
  - Tương tự `std::map`, nhưng cho phép các khóa **trùng lặp**.
  - `operator[]` và `at()` không có trong `multimap` vì một khóa có thể tương ứng với nhiều giá trị.
  - Phải dùng `insert()` để thêm.
  - Dùng `equal_range(key)` để lấy tất cả các giá trị ứng với một khóa.

**4. Unordered Associative Containers (Lưu trữ Kết hợp Không Sắp Xếp - C++11)**

- Lưu trữ các phần tử sử dụng bảng băm (hash tables).
- **Độ phức tạp trung bình cho chèn, xóa, tìm kiếm là O(1).** Trường hợp tệ nhất (do xung đột hash nhiều) là O(N).
- Yêu cầu kiểu khóa phải có:
  1.  Một hàm băm (hash function) `std::hash<Key>` được định nghĩa (STL cung cấp cho các kiểu cơ bản và `std::string`).
  2.  Một toán tử so sánh bằng `operator==` (hoặc functor so sánh bằng tùy chỉnh).
- Thứ tự các phần tử không được đảm bảo và có thể thay đổi khi container được sửa đổi.

- **`std::unordered_set<Key>` (Header: `<unordered_set>`)**: Tương tự `std::set` nhưng không sắp xếp, hiệu năng trung bình tốt hơn.
- **`std::unordered_map<Key, Value>` (Header: `<unordered_map>`)**: Tương tự `std::map` nhưng không sắp xếp, hiệu năng trung bình tốt hơn. `operator[]` và `at()` hoạt động tương tự `std::map`.
- **`std::unordered_multiset<Key>` (Header: `<unordered_set>`)**: Tương tự `std::multiset` nhưng không sắp xếp.
- **`std::unordered_multimap<Key, Value>` (Header: `<unordered_map>`)**: Tương tự `std::multimap` nhưng không sắp xếp.

  ```cpp
  #include <iostream>
  #include <unordered_map>
  #include <string>

  // Ví dụ với custom hash cho một struct (nếu muốn dùng struct làm key)
  struct Person {
      std::string name;
      int age;

      bool operator==(const Person& other) const { // Cần operator==
          return name == other.name && age == other.age;
      }
  };

  // Custom hash function for Person
  namespace std { // Phải chuyên biệt hóa std::hash trong namespace std
      template <>
      struct hash<Person> {
          size_t operator()(const Person& p) const {
              // Kết hợp hash của các thành viên
              size_t h1 = std::hash<std::string>()(p.name);
              size_t h2 = std::hash<int>()(p.age);
              return h1 ^ (h2 << 1); // Hoặc một cách kết hợp hash khác
          }
      };
  }

  int main_unordered_map() {
      std::unordered_map<std::string, double> product_prices;
      product_prices["Laptop"] = 1200.50;
      product_prices["Mouse"] = 25.99;
      product_prices["Keyboard"] = 75.00;

      std::cout << "Price of Mouse: " << product_prices["Mouse"] << std::endl;

      std::cout << "\nAll products (order not guaranteed):" << std::endl;
      for (const auto& pair : product_prices) {
          std::cout << pair.first << ": " << pair.second << std::endl;
      }

      // Với custom key
      std::unordered_map<Person, std::string> person_role;
      Person p1 = {"Alice", 30};
      Person p2 = {"Bob", 25};
      person_role[p1] = "Manager";
      person_role[p2] = "Developer";

      std::cout << "\nRole of Alice: " << person_role[p1] << std::endl;

      return 0;
  }
  ```

**5. Container Adapters (Bộ Điều Hợp Lưu Trữ)**

- Không phải là container thực sự mà là một lớp bao bọc (wrapper) một container tuần tự cơ sở (mặc định là `std::deque`, nhưng có thể thay đổi), cung cấp một interface cụ thể.

- **`std::stack<T, Container = std::deque<T>>` (Header: `<stack>`)**

  - **LIFO (Last-In, First-Out).**
  - **Operations:** `push()` (thêm vào đỉnh), `pop()` (xóa khỏi đỉnh), `top()` (xem đỉnh), `empty()`, `size()`.
  - Không có iterators, không thể duyệt.

- **`std::queue<T, Container = std::deque<T>>` (Header: `<queue>`)**

  - **FIFO (First-In, First-Out).**
  - **Operations:** `push()` (thêm vào cuối), `pop()` (xóa khỏi đầu), `front()` (xem phần tử đầu), `back()` (xem phần tử cuối), `empty()`, `size()`.
  - Không có iterators.

- **`std::priority_queue<T, Container = std::vector<T>, Compare = std::less<T>>` (Header: `<queue>`)**

  - Hàng đợi ưu tiên. Phần tử có "ưu tiên cao nhất" luôn ở đỉnh.
  - Mặc định, `std::less<T>` được dùng, nghĩa là phần tử **lớn nhất** có ưu tiên cao nhất (max-heap).
  - Để có min-heap (phần tử nhỏ nhất ở đỉnh), dùng `std::greater<T>` cho `Compare`.
  - **Operations:** `push()`, `pop()` (xóa phần tử ưu tiên nhất), `top()` (xem phần tử ưu tiên nhất), `empty()`, `size()`.
  - Không có iterators.

  ```cpp
  #include <iostream>
  #include <stack>
  #include <queue> // Cho cả queue và priority_queue

  int main_adapters() {
      // Stack
      std::stack<int> s;
      s.push(1); s.push(2); s.push(3); // Stack: 1 <- 2 <- 3 (top)
      std::cout << "Stack top: " << s.top() << std::endl; // 3
      s.pop(); // Stack: 1 <- 2 (top)
      std::cout << "Stack top after pop: " << s.top() << std::endl; // 2

      // Queue
      std::queue<std::string> q;
      q.push("First"); q.push("Second"); q.push("Third"); // Queue: First (front) -> Second -> Third (back)
      std::cout << "\nQueue front: " << q.front() << std::endl; // First
      q.pop(); // Queue: Second (front) -> Third (back)
      std::cout << "Queue front after pop: " << q.front() << std::endl; // Second

      // Priority Queue (max-heap by default)
      std::priority_queue<int> pq_max;
      pq_max.push(30); pq_max.push(100); pq_max.push(20); pq_max.push(50);
      std::cout << "\nPriority Queue (max-heap) top: " << pq_max.top() << std::endl; // 100
      pq_max.pop();
      std::cout << "Priority Queue top after pop: " << pq_max.top() << std::endl; // 50

      // Priority Queue (min-heap)
      std::priority_queue<int, std::vector<int>, std::greater<int>> pq_min;
      pq_min.push(30); pq_min.push(100); pq_min.push(20); pq_min.push(50);
      std::cout << "\nPriority Queue (min-heap) top: " << pq_min.top() << std::endl; // 20
      pq_min.pop();
      std::cout << "Priority Queue top after pop: " << pq_min.top() << std::endl; // 30

      return 0;
  }
  ```

**6. Best Practices & Chọn Container Phù Hợp**

- **Mặc định, cân nhắc `std::vector`** nếu không có lý do cụ thể để chọn loại khác. Nó hiệu quả và linh hoạt.
- **Cần chèn/xóa nhanh ở cả đầu và cuối?** Dùng `std::deque`.
- **Cần chèn/xóa nhanh ở giữa và không cần truy cập ngẫu nhiên?** Dùng `std::list`.
- **Biết kích thước tại compile-time và không đổi?** Dùng `std::array`.
- **Cần lưu trữ các phần tử duy nhất và tìm kiếm nhanh (đã sắp xếp)?** Dùng `std::set`.
- **Cần ánh xạ khóa-giá trị (đã sắp xếp, khóa duy nhất)?** Dùng `std::map`.
- **Cần hiệu năng tìm kiếm/chèn/xóa O(1) trung bình và không quan tâm thứ tự?** Dùng các phiên bản `unordered_` (ví dụ, `std::unordered_map`, `std::unordered_set`).
- **Cần LIFO?** `std::stack`. **FIFO?** `std::queue`. **Lấy phần tử ưu tiên nhất?** `std::priority_queue`.
- **Hiểu độ phức tạp (Big O) của các thao tác** trên từng container để đưa ra lựa chọn tối ưu cho hiệu năng.
- **Sử dụng iterators và thuật toán STL** để thao tác với container một cách hiệu quả và an toàn (sẽ học ở phần sau).

STL Containers là một phần cực kỳ quan trọng và hữu ích của C++. Việc nắm vững chúng sẽ giúp bạn viết code hiệu quả và dễ bảo trì hơn rất nhiều.

Hết Phần 9.
OK. Tiếp tục với Phần 10, chúng ta sẽ tìm hiểu về Iterators và Algorithms trong Thư Viện Template Chuẩn (STL).

**Phần 10: Thư Viện Template Chuẩn (STL) - Iterators và Algorithms**

Iterators và Algorithms là hai thành phần cốt lõi khác của STL, hoạt động mật thiết với Containers để cung cấp một cách thức mạnh mẽ và linh hoạt để xử lý dữ liệu.

**1. Iterators (Bộ Duyệt)**

- **Khái niệm:** Iterator là một đối tượng hoạt động giống như một con trỏ, cung cấp một cách để truy cập tuần tự các phần tử trong một container (hoặc một dãy dữ liệu khác).
- **Mục đích:**
  - **Trừu tượng hóa truy cập:** Cung cấp một interface chung để duyệt qua các loại container khác nhau mà không cần biết chi tiết triển khai bên trong của container đó.
  - **Kết nối Containers và Algorithms:** Algorithms trong STL hoạt động trên các _dãy_ được xác định bởi các cặp iterator (thường là `begin` và `end`), chứ không phải trực tiếp trên container. Điều này cho phép một thuật toán có thể hoạt động trên nhiều loại container khác nhau.
- **Các thao tác cơ bản với iterator:**
  - `*it`: Dereference (truy cập giá trị phần tử mà iterator đang trỏ tới).
  - `it++` / `++it`: Di chuyển iterator tới phần tử tiếp theo.
  - `it--` / `--it`: Di chuyển iterator tới phần tử trước (chỉ cho Bidirectional và Random Access Iterators).
  - `it1 == it2`, `it1 != it2`: So sánh hai iterator.
  - `it + n`, `it - n`, `it[n]`, `it1 - it2`: Các phép toán số học (chỉ cho Random Access Iterators).
- **Lấy iterator từ container:**
  - `container.begin()`: Trả về iterator trỏ tới phần tử đầu tiên của container.
  - `container.end()`: Trả về iterator trỏ tới vị trí "sau phần tử cuối cùng" (past-the-end). Đây là một vị trí hợp lệ nhưng **không thể dereference**. Dùng để đánh dấu kết thúc của dãy.
  - `container.rbegin()`, `container.rend()`: Cho reverse iterators (duyệt ngược).
  - `container.cbegin()`, `container.cend()`, `container.crbegin()`, `container.crend()` (C++11): Cho const iterators (không cho phép sửa đổi phần tử qua iterator).

**Các Loại Iterator (Iterator Categories):**

STL định nghĩa 5 loại iterator, mỗi loại có một tập hợp các thao tác được hỗ trợ khác nhau. Một loại "cao hơn" sẽ hỗ trợ tất cả các thao tác của loại "thấp hơn".

1.  **Input Iterator:**
    - Chỉ có thể duyệt **tiến** (forward), từng phần tử một.
    - Chỉ có thể **đọc** giá trị (`*it`) một lần cho mỗi vị trí (sau khi đọc, không đảm bảo giá trị vẫn còn hoặc iterator vẫn hợp lệ để đọc lại).
    - Hỗ trợ: `++`, `*` (đọc), `==`, `!=`.
    - Ví dụ: Đọc từ `std::istream` (`std::istream_iterator`).
2.  **Output Iterator:**
    - Chỉ có thể duyệt **tiến**, từng phần tử một.
    - Chỉ có thể **ghi** giá trị (`*it = value`) một lần cho mỗi vị trí.
    - Hỗ trợ: `++`, `*` (ghi).
    - Ví dụ: Ghi ra `std::ostream` (`std::ostream_iterator`).
3.  **Forward Iterator:**
    - Kết hợp tính năng của Input và Output Iterator.
    - Có thể duyệt **tiến** nhiều lần qua cùng một dãy.
    - Có thể **đọc và/hoặc ghi** nhiều lần vào cùng một vị trí (nếu phần tử đó cho phép).
    - Hỗ trợ: `++`, `*` (đọc/ghi), `==`, `!=`.
    - Ví dụ: Iterators của `std::forward_list`. `std::unordered_map/set` cũng cung cấp ít nhất là Forward Iterator.
4.  **Bidirectional Iterator:**
    - Thêm khả năng duyệt **lùi** (`--`).
    - Hỗ trợ: Tất cả của Forward Iterator + `--`.
    - Ví dụ: Iterators của `std::list`, `std::set`, `std::map`.
5.  **Random Access Iterator:**
    - Mạnh mẽ nhất. Hỗ trợ tất cả các thao tác của Bidirectional Iterator.
    - Thêm khả năng **truy cập ngẫu nhiên** (nhảy tới bất kỳ phần tử nào trong O(1)) bằng các phép toán số học.
    - Hỗ trợ: `it + n`, `it - n`, `it += n`, `it -= n`, `it1 - it2` (tính khoảng cách), `it[n]`.
    - Ví dụ: Iterators của `std::vector`, `std::deque`, `std::array`, con trỏ C-style vào mảng.

**Ví dụ sử dụng iterators:**

```cpp
#include <iostream>
#include <vector>
#include <list>
#include <map>
#include <string>

int main_iterators() {
    std::vector<int> vec = {10, 20, 30, 40, 50};

    // Duyệt vector bằng iterator
    std::cout << "Vector elements: ";
    for (std::vector<int>::iterator it = vec.begin(); it != vec.end(); ++it) {
        std::cout << *it << " "; // Dereference để lấy giá trị
        // *it = *it * 2; // Có thể sửa đổi nếu iterator không phải const
    }
    std::cout << std::endl;

    // Duyệt bằng const_iterator (C++03 style, hoặc dùng cbegin/cend C++11)
    std::cout << "Vector elements (const_iterator): ";
    for (std::vector<int>::const_iterator cit = vec.cbegin(); cit != vec.cend(); ++cit) {
        std::cout << *cit << " ";
        // *cit = 100; // LỖI: không thể sửa đổi qua const_iterator
    }
    std::cout << std::endl;

    // Duyệt ngược vector bằng reverse_iterator
    std::cout << "Vector elements (reversed): ";
    for (std::vector<int>::reverse_iterator rit = vec.rbegin(); rit != vec.rend(); ++rit) {
        std::cout << *rit << " "; // Output: 50 40 30 20 10
    }
    std::cout << std::endl;


    std::list<std::string> str_list = {"alpha", "beta", "gamma"};
    std::cout << "List elements: ";
    for (const auto& s : str_list) { // Range-based for sử dụng iterators ngầm
        std::cout << s << " ";
    }
    std::cout << std::endl;


    std::map<char, int> char_counts = {{'a', 3}, {'b', 1}, {'c', 5}};
    std::cout << "Map elements: " << std::endl;
    for (std::map<char, int>::iterator map_it = char_counts.begin(); map_it != char_counts.end(); ++map_it) {
        // *map_it là một std::pair<const char, int>
        std::cout << "Key: " << map_it->first << ", Value: " << map_it->second << std::endl;
        // map_it->first = 'd'; // LỖI: key của map là const
        map_it->second = map_it->second + 10; // OK: value có thể sửa đổi
    }
    return 0;
}
```

**2. Algorithms (Thuật Toán)**

- Header: `<algorithm>` (chủ yếu), `<numeric>` (cho các thuật toán số học như `std::accumulate`, `std::iota`), `<memory>` (cho các thuật toán liên quan đến bộ nhớ).
- Là các khuôn mẫu hàm độc lập, không phải là thành viên của các lớp container.
- Chúng hoạt động trên các dãy được xác định bởi các cặp iterator. Điều này làm cho chúng rất linh hoạt.
- **Phân loại (không chính thức):**
  - **Non-modifying sequence operations (Không thay đổi dãy):**
    - `std::for_each`: Áp dụng một hàm cho mỗi phần tử.
    - `std::find`, `std::find_if`, `std::find_if_not`: Tìm phần tử.
    - `std::count`, `std::count_if`: Đếm số lần xuất hiện.
    - `std::mismatch`: Tìm vị trí khác nhau đầu tiên giữa hai dãy.
    - `std::equal`: Kiểm tra hai dãy có bằng nhau không.
    - `std::search`: Tìm một dãy con.
    - `std::all_of`, `std::any_of`, `std::none_of` (C++11): Kiểm tra predicate trên tất cả/bất kỳ/không phần tử nào.
  - **Modifying sequence operations (Thay đổi dãy):**
    - `std::copy`, `std::copy_if`, `std::copy_n`, `std::copy_backward`: Sao chép phần tử.
    - `std::move`, `std::move_backward` (C++11): Di chuyển phần tử.
    - `std::transform`: Áp dụng một phép biến đổi cho mỗi phần tử và lưu kết quả.
    - `std::replace`, `std::replace_if`: Thay thế các phần tử.
    - `std::fill`, `std::fill_n`: Điền giá trị vào dãy.
    - `std::generate`, `std::generate_n`: Tạo phần tử bằng một hàm generator.
    - `std::remove`, `std::remove_if`: "Xóa" các phần tử (thực ra là di chuyển các phần tử không bị xóa lên đầu, trả về iterator tới vị trí cuối mới của dãy hợp lệ. Cần kết hợp với `container.erase()` để thực sự xóa).
    - `std::unique`: Loại bỏ các phần tử trùng lặp liên tiếp (dãy cần được sắp xếp trước).
    - `std::reverse`: Đảo ngược dãy.
    - `std::rotate`: Xoay các phần tử.
    - `std::shuffle` (C++11), `std::random_shuffle` (deprecated): Xáo trộn ngẫu nhiên.
  - **Partitioning operations (Phân hoạch):**
    - `std::partition`: Phân hoạch dãy thành hai nhóm dựa trên predicate.
    - `std::stable_partition`: Tương tự `partition` nhưng giữ thứ tự tương đối.
  - **Sorting operations (Sắp xếp):**
    - `std::sort`: Sắp xếp dãy (thường dùng IntroSort - kết hợp QuickSort, HeapSort, InsertionSort).
    - `std::stable_sort`: Sắp xếp ổn định (giữ thứ tự tương đối của các phần tử bằng nhau).
    - `std::partial_sort`: Sắp xếp một phần của dãy.
    - `std::nth_element`: Đặt phần tử thứ n vào đúng vị trí nếu dãy được sắp xếp, các phần tử khác không đảm bảo thứ tự.
  - **Binary search operations (Tìm kiếm nhị phân - yêu cầu dãy đã được sắp xếp):**
    - `std::lower_bound`: Tìm vị trí đầu tiên không nhỏ hơn giá trị cho trước.
    - `std::upper_bound`: Tìm vị trí đầu tiên lớn hơn giá trị cho trước.
    - `std::equal_range`: Trả về cặp `lower_bound` và `upper_bound`.
    - `std::binary_search`: Kiểm tra một giá trị có tồn tại trong dãy không.
  - **Merge operations (Trộn - yêu cầu các dãy đã được sắp xếp):**
    - `std::merge`: Trộn hai dãy đã sắp xếp thành một dãy mới.
    - `std::inplace_merge`: Trộn hai phần đã sắp xếp liền kề của cùng một dãy.
  - **Heap operations (Thao tác với heap):**
    - `std::make_heap`, `std::push_heap`, `std::pop_heap`, `std::sort_heap`.
  - **Min/max operations:**
    - `std::min`, `std::max`, `std::minmax` (C++11), `std::min_element`, `std::max_element`, `std::minmax_element` (C++11).
  - **Numeric operations (Header `<numeric>`):**
    - `std::accumulate`: Tính tổng (hoặc phép toán hai ngôi khác) các phần tử.
    - `std::inner_product`: Tính tích vô hướng.
    - `std::partial_sum`: Tính tổng tiền tố.
    - `std::iota` (C++11): Điền dãy với các giá trị tăng dần.

**Ví dụ sử dụng algorithms:**

```cpp
#include <iostream>
#include <vector>
#include <list>
#include <string>
#include <algorithm> // Cho hầu hết các thuật toán
#include <numeric>   // Cho std::accumulate, std::iota
#include <functional> // Cho std::greater, std::bind, etc. (sẽ dùng ở phần sau)

bool isOdd(int n) { return n % 2 != 0; }
void printDouble(int n) { std::cout << n * 2 << " "; }

int main_algorithms() {
    std::vector<int> v = {5, 1, 8, 2, 8, 3, 4, 8, 6};

    // std::for_each
    std::cout << "Doubled values: ";
    std::for_each(v.begin(), v.end(), printDouble); // Output: 10 2 16 4 16 6 8 16 12
    std::cout << std::endl;

    // std::find
    auto it_find = std::find(v.begin(), v.end(), 3);
    if (it_find != v.end()) {
        std::cout << "Found 3 at index: " << std::distance(v.begin(), it_find) << std::endl;
    }

    // std::count_if
    int odd_count = std::count_if(v.begin(), v.end(), isOdd);
    std::cout << "Number of odd elements: " << odd_count << std::endl; // 4 (5,1,3)

    // std::sort
    std::vector<int> v_sorted = v;
    std::sort(v_sorted.begin(), v_sorted.end()); // Sắp xếp tăng dần
    std::cout << "Sorted v_sorted: ";
    for (int x : v_sorted) std::cout << x << " "; // Output: 1 2 3 4 5 6 8 8 8
    std::cout << std::endl;

    // std::sort với custom comparator (sắp xếp giảm dần)
    std::sort(v_sorted.begin(), v_sorted.end(), std::greater<int>());
    std::cout << "Sorted v_sorted (descending): ";
    for (int x : v_sorted) std::cout << x << " "; // Output: 8 8 8 6 5 4 3 2 1
    std::cout << std::endl;

    // std::remove (kết hợp erase) - "erase-remove idiom"
    std::vector<int> v_remove = v; // {5, 1, 8, 2, 8, 3, 4, 8, 6}
    // Di chuyển tất cả các số 8 về cuối và trả về iterator tới số 8 đầu tiên
    auto new_end = std::remove(v_remove.begin(), v_remove.end(), 8);
    // v_remove bây giờ có thể là: {5, 1, 2, 3, 4, 6, ?, ?, ?} (phần tử hợp lệ đến new_end)
    std::cout << "v_remove after std::remove(8) (before erase): ";
    for(auto it = v_remove.begin(); it != new_end; ++it) std::cout << *it << " ";
    std::cout << std::endl;

    v_remove.erase(new_end, v_remove.end()); // Thực sự xóa các phần tử từ new_end
    std::cout << "v_remove after erase: ";
    for (int x : v_remove) std::cout << x << " "; // Output: 5 1 2 3 4 6
    std::cout << std::endl;

    // std::transform
    std::vector<int> v_transformed;
    v_transformed.resize(v.size()); // Đảm bảo đủ chỗ
    std::transform(v.begin(), v.end(), v_transformed.begin(), [](int n){ return n * n; }); // Bình phương
    std::cout << "v_transformed (squares): ";
    for (int x : v_transformed) std::cout << x << " ";
    std::cout << std::endl;

    // std::accumulate (trong <numeric>)
    int sum = std::accumulate(v.begin(), v.end(), 0); // Tính tổng, giá trị khởi tạo là 0
    std::cout << "Sum of v: " << sum << std::endl; // 45

    long long product = std::accumulate(v.begin(), v.end(), 1LL, std::multiplies<long long>());
    std::cout << "Product of v: " << product << std::endl;

    // std::iota (trong <numeric>) - C++11
    std::vector<int> v_iota(5);
    std::iota(v_iota.begin(), v_iota.end(), 100); // Fills with 100, 101, 102, 103, 104
    std::cout << "v_iota: ";
    for (int x : v_iota) std::cout << x << " ";
    std::cout << std::endl;

    return 0;
}
```

**3. Function Objects (Functors) và Lambdas (C++11)**

Nhiều thuật toán STL nhận một hàm (hoặc đối tượng có thể gọi được) làm tham số để tùy chỉnh hành vi của chúng (ví dụ: `std::sort` nhận một hàm so sánh, `std::count_if` nhận một predicate).

- **Function Objects (Functors):**

  - Là các đối tượng của một lớp nạp chồng `operator()`. Điều này cho phép đối tượng được gọi như một hàm.
  - Có thể lưu trữ trạng thái (thành viên dữ liệu).

  ```cpp
  struct IsGreaterThan {
      int limit;
      IsGreaterThan(int l) : limit(l) {}
      bool operator()(int value) const { // Nạp chồng operator()
          return value > limit;
      }
  };

  // int main() {
  //     std::vector<int> numbers = {10, 25, 5, 30, 15};
  //     int count_gt_20 = std::count_if(numbers.begin(), numbers.end(), IsGreaterThan(20));
  //     std::cout << "Numbers greater than 20: " << count_gt_20 << std::endl; // Output: 2
  // }
  ```

  - STL cung cấp sẵn nhiều functor trong `<functional>` (ví dụ: `std::plus<T>`, `std::less<T>`, `std::equal_to<T>`).

- **Lambda Expressions (C++11):**

  - Cung cấp một cú pháp ngắn gọn để tạo các function object ẩn danh (anonymous) ngay tại chỗ.
  - Cú pháp cơ bản: `[capture_list](parameters) -> return_type { body }`
    - `capture_list`: Xác định các biến từ scope xung quanh được "bắt" (capture) và cách bắt (by value `=` hoặc by reference `&`).
      - `[]`: Không bắt gì.
      - `[=]`: Bắt tất cả các biến đã dùng bằng giá trị.
      - `[&]`: Bắt tất cả các biến đã dùng bằng tham chiếu.
      - `[x, &y]`: Bắt `x` bằng giá trị, `y` bằng tham chiếu.
      - `[this]`: Bắt con trỏ `this` (trong hàm thành viên).
    - `parameters`: Danh sách tham số (giống hàm thông thường).
    - `-> return_type`: Kiểu trả về (tùy chọn, trình biên dịch thường tự suy ra).
    - `body`: Thân của lambda.

  ```cpp
  #include <iostream>
  #include <vector>
  #include <algorithm>
  #include <string>

  int main_lambda() {
      std::vector<int> nums = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

      // Đếm số chẵn dùng lambda
      int even_count = std::count_if(nums.begin(), nums.end(),
                                  [](int n) -> bool { return n % 2 == 0; }
                                 );
      std::cout << "Even numbers: " << even_count << std::endl;

      // In các số lớn hơn một giới hạn (bắt biến limit)
      int limit = 5;
      std::cout << "Numbers greater than " << limit << ": ";
      std::for_each(nums.begin(), nums.end(),
                    [limit](int n) { // Bắt 'limit' bằng giá trị
                        if (n > limit) {
                            std::cout << n << " ";
                        }
                    }
                   );
      std::cout << std::endl;

      // Sắp xếp một vector các chuỗi theo độ dài
      std::vector<std::string> words = {"apple", "banana", "kiwi", "orange", "grape"};
      std::sort(words.begin(), words.end(),
                [](const std::string& a, const std::string& b) {
                    return a.length() < b.length();
                }
               );
      std::cout << "Words sorted by length: ";
      for (const auto& w : words) std::cout << w << " "; // kiwi apple grape banana orange
      std::cout << std::endl;


      // Lambda có thể sửa đổi biến bắt bằng tham chiếu
      int sum = 0;
      std::for_each(nums.begin(), nums.end(),
                    [&sum](int n) { // Bắt 'sum' bằng tham chiếu
                        sum += n;
                    }
                   );
      std::cout << "Sum (via lambda with ref capture): " << sum << std::endl;


      // Lambda với mutable (cho phép sửa đổi biến bắt bằng giá trị bên trong lambda)
      int counter = 0;
      auto mutable_lambda = [counter]() mutable {
          counter++; // 'counter' ở đây là bản sao, thay đổi không ảnh hưởng counter gốc
          return counter;
      };
      std::cout << "Mutable lambda call 1: " << mutable_lambda() << std::endl; // 1
      std::cout << "Mutable lambda call 2: " << mutable_lambda() << std::endl; // 2
      std::cout << "Original counter: " << counter << std::endl; // 0

      return 0;
  }
  ```

  - **Best Practice:** Ưu tiên sử dụng lambdas cho các tác vụ đơn giản và cục bộ thay vì viết functor riêng, giúp code ngắn gọn và dễ đọc hơn.

**4. Best Practices & Lưu Ý Chung (Phần 10)**

- **Sử dụng thuật toán STL thay vì tự viết vòng lặp:** Thuật toán STL đã được tối ưu và kiểm thử kỹ, giúp code ngắn gọn, dễ đọc, ít lỗi hơn.
- **Hiểu rõ các loại iterator** và thuật toán nào yêu cầu loại iterator nào.
- **Erase-Remove Idiom:** Khi muốn xóa các phần tử khỏi container (như `vector`, `deque`, `list`) dựa trên một điều kiện, hãy dùng `std::remove` hoặc `std::remove_if` sau đó là `container.erase()`.
- **Const Correctness với Iterators:** Dùng `cbegin()`, `cend()` để lấy const iterators khi bạn chỉ cần đọc dữ liệu, giúp đảm bảo không vô tình sửa đổi.
- **Iterator Invalidation:** Cẩn thận với việc các thao tác trên container (chèn, xóa) có thể làm vô hiệu hóa (invalidate) các iterator hiện có.
  - `std::vector`, `std::deque`: Chèn/xóa có thể làm vô hiệu hóa tất cả iterator (hoặc những iterator từ điểm chèn/xóa trở về sau). `reserve()` trước có thể giúp.
  - `std::list`, `std::forward_list`: Chỉ iterator trỏ tới phần tử bị xóa mới bị vô hiệu hóa. Các iterator khác vẫn hợp lệ.
  - Associative containers (`map`, `set`, `unordered_map`, `unordered_set`): Xóa một phần tử chỉ làm vô hiệu hóa iterator trỏ tới phần tử đó. Chèn không làm vô hiệu hóa iterator (trừ `unordered_` containers có thể rehash).
- **Lambda là một công cụ mạnh mẽ:** Sử dụng capture list một cách cẩn thận, đặc biệt là bắt bằng tham chiếu (đảm bảo đối tượng tham chiếu còn sống khi lambda được gọi).

Iterators và Algorithms là trái tim của STL, cho phép bạn viết mã C++ generic, hiệu quả và biểu cảm.

Hết Phần 10.

Tuyệt vời! Dựa trên 10 phần bạn đã cung cấp, đây là Phần 11. Chúng ta sẽ đi vào một khía cạnh rất quan trọng của việc viết mã C++ mạnh mẽ: Xử Lý Ngoại Lệ.

---

**Phần 11: Xử Lý Ngoại Lệ (Exception Handling)**

Trong quá trình thực thi chương trình, có thể xảy ra các tình huống lỗi không lường trước hoặc các điều kiện bất thường mà chương trình không thể xử lý theo luồng thông thường. Xử lý ngoại lệ là một cơ chế trong C++ cho phép chương trình đối phó với những tình huống này một cách có cấu trúc, thay vì bị crash đột ngột hoặc trả về các mã lỗi khó quản lý.

**1. Khái Niệm Cơ Bản: `try`, `catch`, `throw`**

- **`throw` (Ném ngoại lệ):**
  - Khi một tình huống lỗi xảy ra trong một đoạn mã, bạn có thể "ném" một đối tượng ngoại lệ bằng từ khóa `throw`.
  - Đối tượng được ném có thể là bất kỳ kiểu dữ liệu nào (kiểu cơ bản, đối tượng lớp, con trỏ), nhưng thường là một đối tượng của một lớp được thiết kế riêng cho ngoại lệ (thường kế thừa từ `std::exception`).
  - Khi một ngoại lệ được ném, luồng thực thi bình thường của chương trình bị dừng lại, và trình biên dịch bắt đầu tìm kiếm một khối `catch` phù hợp.
- **`try` (Khối thử):**
  - Đoạn mã mà bạn dự đoán có thể phát sinh ngoại lệ được đặt trong một khối `try`.
  - Cú pháp: `try { /* code có thể ném ngoại lệ */ }`
- **`catch` (Khối bắt):**
  - Theo sau một khối `try` là một hoặc nhiều khối `catch`. Mỗi khối `catch` được thiết kế để "bắt" và xử lý một loại ngoại lệ cụ thể.
  - Cú pháp: `catch (type_of_exception& e) { /* code xử lý ngoại lệ */ }`
  - Trình biên dịch sẽ khớp kiểu của đối tượng ngoại lệ được ném với kiểu được khai báo trong các khối `catch` theo thứ tự chúng xuất hiện. Khối `catch` đầu tiên khớp kiểu sẽ được thực thi.

**Ví dụ cơ bản:**

```cpp
#include <iostream>
#include <string>
#include <stdexcept> // Cho các lớp ngoại lệ chuẩn như std::runtime_error

double divide(int numerator, int denominator) {
    if (denominator == 0) {
        throw std::runtime_error("Division by zero error!"); // Ném một đối tượng std::runtime_error
    }
    if (denominator < 0 ) {
        throw "Denominator cannot be negative (custom string exception)"; // Ném một chuỗi C-style
    }
    return static_cast<double>(numerator) / denominator;
}

int main() {
    int a = 10, b = 0, c = 2, d = -1;

    try {
        std::cout << "Attempting 10 / 0..." << std::endl;
        double result1 = divide(a, b); // Sẽ ném ngoại lệ
        std::cout << "Result 1: " << result1 << std::endl; // Dòng này không được thực thi
    } catch (const std::runtime_error& e) { // Bắt std::runtime_error bằng tham chiếu hằng
        std::cerr << "Caught a runtime_error: " << e.what() << std::endl; // e.what() trả về thông điệp lỗi
    } catch (const char* msg) { // Bắt ngoại lệ kiểu const char*
        std::cerr << "Caught a C-string exception: " << msg << std::endl;
    }

    std::cout << "\nAttempting 10 / 2..." << std::endl;
    try {
        double result2 = divide(a, c);
        std::cout << "Result 2: " << result2 << std::endl; // Output: 5
    } catch (const std::runtime_error& e) {
        std::cerr << "Caught a runtime_error: " << e.what() << std::endl;
    }
    // Không có catch cho const char* ở đây

    std::cout << "\nAttempting 10 / -1..." << std::endl;
    try {
        double result3 = divide(a, d); // Sẽ ném const char*
        std::cout << "Result 3: " << result3 << std::endl;
    } catch (const std::runtime_error& e) {
         std::cerr << "Caught a runtime_error: " << e.what() << std::endl;
    }
    // Nếu không có catch(const char*) phù hợp ở đây, và không có ở các scope cao hơn,
    // std::terminate sẽ được gọi.

    std::cout << "\nProgram continues after handling exceptions (if any)." << std::endl;
    return 0;
}
```

**2. Các Lớp Ngoại Lệ Chuẩn (Standard Exceptions)**

- Header: `<stdexcept>`
- Thư viện chuẩn C++ cung cấp một hệ thống phân cấp các lớp ngoại lệ, tất cả đều kế thừa từ lớp cơ sở `std::exception`.
- `std::exception`:
  - Lớp cơ sở cho tất cả các ngoại lệ chuẩn.
  - Có một hàm thành viên ảo `virtual const char* what() const noexcept;` trả về một chuỗi C-style mô tả ngoại lệ.
- Một số lớp ngoại lệ chuẩn phổ biến kế thừa từ `std::exception`:
  - **Các lớp kế thừa trực tiếp từ `std::exception`:**
    - `std::bad_alloc`: Được ném bởi `new` khi không thể cấp phát bộ nhớ. (Header `<new>`)
    - `std::bad_cast`: Được ném bởi `dynamic_cast` khi cast tham chiếu thất bại. (Header `<typeinfo>`)
    - `std::bad_typeid`: Được ném bởi `typeid` trên một con trỏ null. (Header `<typeinfo>`)
    - `std::bad_exception`: Có thể được ném trong một số tình huống liên quan đến xử lý ngoại lệ (ít gặp).
  - **`std::logic_error` (Lỗi logic trong chương trình):**
    - `std::invalid_argument`: Đối số không hợp lệ cho hàm.
    - `std::domain_error`: Giá trị nằm ngoài miền xác định (ví dụ, `sqrt(-1)`).
    - `std::length_error`: Cố gắng tạo đối tượng có độ dài vượt quá giới hạn cho phép (ví dụ, `std::string` quá dài).
    - `std::out_of_range`: Truy cập phần tử ngoài phạm vi hợp lệ (ví dụ, `vector::at()`).
  - **`std::runtime_error` (Lỗi chỉ có thể phát hiện tại thời điểm chạy):**
    - `std::range_error`: Kết quả tính toán không thể biểu diễn được (ví dụ, tràn số).
    - `std::overflow_error`: Tràn số số học.
    - `std::underflow_error`: Tràn số dưới (số học).
    - `std::system_error` (C++11, Header `<system_error>`): Báo cáo lỗi từ hệ điều hành hoặc các API cấp thấp.

**3. Bắt Ngoại Lệ**

- **Bắt theo kiểu cụ thể:** `catch (const std::out_of_range& e)`
- **Bắt theo kiểu lớp cha (polymorphic catch):** Nếu bạn bắt một ngoại lệ bằng tham chiếu tới lớp cha (ví dụ `catch (const std::exception& e)`), nó có thể bắt được tất cả các đối tượng ngoại lệ của các lớp con kế thừa từ lớp cha đó.
- **Thứ tự các khối `catch`:** Rất quan trọng. Các khối `catch` được kiểm tra theo thứ tự chúng xuất hiện. Nếu một khối `catch` cho lớp cha đứng trước khối `catch` cho lớp con, thì khối `catch` cho lớp con sẽ không bao giờ được thực thi cho ngoại lệ đó.
  ```cpp
  try {
      // ... code có thể ném std::out_of_range
  } catch (const std::exception& e) { // Bắt tất cả std::exception và con của nó
      std::cerr << "Caught std::exception: " << e.what() << std::endl;
  } catch (const std::out_of_range& e) { // KHỐI NÀY SẼ KHÔNG BAO GIỜ ĐƯỢC THỰC THI
                                         // vì std::out_of_range đã bị bắt bởi catch(std::exception&)
      std::cerr << "Caught std::out_of_range: " << e.what() << std::endl;
  }
  // Nên đặt catch cho lớp con trước catch cho lớp cha.
  ```
- **Catch-all (Bắt tất cả):** `catch (...)`
  - Khối này sẽ bắt bất kỳ loại ngoại lệ nào chưa được bắt bởi các khối `catch` trước đó.
  - Bên trong `catch (...)`, bạn không có thông tin về kiểu hoặc giá trị của ngoại lệ.
  - Thường được dùng để thực hiện dọn dẹp cuối cùng hoặc log lỗi trước khi có thể ném lại ngoại lệ hoặc kết thúc chương trình.
- **Ném lại ngoại lệ (Rethrowing):**
  - Bên trong một khối `catch`, bạn có thể ném lại chính ngoại lệ vừa bắt được bằng cách sử dụng `throw;` (không có toán hạng).
  - Điều này hữu ích khi một hàm chỉ có thể xử lý một phần ngoại lệ và muốn báo cho scope gọi hàm xử lý tiếp.
  ```cpp
  void processFile(const std::string& filename) {
      try {
          // ... mở file, đọc file ...
          if (/* lỗi đọc file cụ thể */) {
              throw std::runtime_error("Specific file read error");
          }
      } catch (const std::runtime_error& e) {
          std::cerr << "Error in processFile: " << e.what() << std::endl;
          // Dọn dẹp cục bộ nếu có
          throw; // Ném lại ngoại lệ gốc để scope gọi hàm xử lý
      } catch (...) {
          std::cerr << "Unknown error in processFile." << std::endl;
          throw; // Ném lại
      }
  }
  ```

**4. Stack Unwinding (Xả Ngăn Xếp)**

- Khi một ngoại lệ được ném và không được bắt trong scope hiện tại, C++ sẽ "xả ngăn xếp" (stack unwinding).
- Quá trình này bao gồm:
  1.  Thoát khỏi scope hiện tại.
  2.  **Destructor của tất cả các đối tượng cục bộ (local objects) được tạo trên stack trong scope đó sẽ được tự động gọi.** Đây là lý do tại sao RAII (Resource Acquisition Is Initialization) rất quan trọng cho exception safety.
  3.  Chương trình tìm kiếm một khối `catch` phù hợp trong scope gọi hàm.
  4.  Nếu không tìm thấy, quá trình tiếp tục xả ngăn xếp lên scope cao hơn.
  5.  Nếu ngoại lệ không được bắt ở bất kỳ đâu cho đến khi thoát khỏi hàm `main()`, chương trình sẽ gọi `std::terminate()`, thường dẫn đến việc dừng chương trình.

**5. `noexcept` Specifier (C++11)**

- Từ khóa `noexcept` được dùng để chỉ định rằng một hàm **không được phép** ném bất kỳ ngoại lệ nào.
  - `void func() noexcept;` (tương đương `noexcept(true)`)
  - `void func() noexcept(expression);` (hàm sẽ là `noexcept` nếu `expression` ước lượng thành `true` tại compile-time).
- **Hậu quả:** Nếu một hàm được đánh dấu `noexcept` lại ném một ngoại lệ, chương trình sẽ gọi `std::terminate()` ngay lập tức, không có stack unwinding từ điểm đó.
- **Tại sao dùng `noexcept`?**
  - **Tối ưu hóa:** Trình biên dịch có thể tạo mã hiệu quả hơn cho các hàm `noexcept` vì không cần chuẩn bị cho stack unwinding.
  - **Đảm bảo:** Cung cấp một đảm bảo mạnh mẽ về hành vi của hàm, quan trọng cho các hàm cơ bản (ví dụ: move constructors, move assignment operators, swap, destructors).
  - Move constructors/assignment operators của các container STL và smart pointers thường chỉ được gọi nếu các thao tác move của kiểu phần tử là `noexcept`, để đảm bảo strong exception safety.
  - **Destructors:** Mặc định là `noexcept` trong C++11 trở đi (nếu tất cả các thành viên và lớp cha có destructor `noexcept`). Rất không nên để destructor ném ngoại lệ. Nếu destructor ném ngoại lệ trong quá trình stack unwinding (do một ngoại lệ khác đã được ném), `std::terminate()` sẽ được gọi.

**6. Exception Safety Guarantees (Các Đảm Bảo An Toàn Ngoại Lệ)**

Khi viết code có thể ném ngoại lệ, cần xem xét các mức độ đảm bảo an toàn mà hàm/lớp của bạn cung cấp:

1.  **Basic Guarantee (Đảm bảo cơ bản):**
    - Nếu một ngoại lệ được ném, chương trình vẫn ở trạng thái hợp lệ.
    - Không có tài nguyên bị rò rỉ (resource leaks).
    - Các invariants (bất biến) của đối tượng vẫn được duy trì.
    - Tuy nhiên, trạng thái của đối tượng có thể đã thay đổi so với trước khi thao tác.
    - Đây là mức đảm bảo tối thiểu mà hầu hết các hàm nên cố gắng đạt được. RAII là chìa khóa.
2.  **Strong Guarantee (Đảm bảo mạnh):**
    - Nếu một thao tác ném ngoại lệ, trạng thái của chương trình/đối tượng được khôi phục hoàn toàn về trạng thái như trước khi thao tác đó bắt đầu (commit-or-rollback semantics).
    - Khó đạt được hơn Basic Guarantee. Một kỹ thuật phổ biến là "copy-and-swap idiom".
3.  **Nothrow (No-throw) Guarantee (Đảm bảo không ném):**
    - Hàm được đảm bảo không bao giờ ném ngoại lệ.
    - Được chỉ định bằng `noexcept`.
    - Đây là đảm bảo mạnh nhất, thường cho các thao tác rất cơ bản hoặc các hàm đã xử lý tất cả các lỗi có thể.

**7. Thiết Kế Lớp Ngoại Lệ Tùy Chỉnh (Custom Exception Classes)**

- Nên kế thừa từ `std::exception` hoặc một trong các lớp con của nó.
- Cung cấp thông tin chi tiết hơn về lỗi.
- Ghi đè hàm `what()` để trả về thông điệp lỗi.

```cpp
#include <stdexcept>
#include <string>

class MyFileError : public std::runtime_error {
private:
    std::string filename;
    int error_code;
public:
    MyFileError(const std::string& msg, const std::string& fname, int err_code)
        : std::runtime_error(msg + " (File: " + fname + ", Code: " + std::to_string(err_code) + ")"),
          filename(fname), error_code(err_code) {}

    // Không cần ghi đè what() nếu constructor của lớp cha đã tạo thông điệp đủ tốt.
    // const char* what() const noexcept override {
    //    // có thể tạo thông điệp phức tạp hơn ở đây nếu cần
    //    // và lưu vào một std::string thành viên, rồi trả về c_str()
    //    return std::runtime_error::what(); // Hoặc gọi phiên bản của cha
    // }

    std::string getFilename() const { return filename; }
    int getErrorCode() const { return error_code; }
};

void readFile(const std::string& fname) {
    if (fname.empty()) {
        throw MyFileError("Filename cannot be empty", fname, 101);
    }
    // ... code mở và đọc file ...
    bool read_failed_simulated = true; // Giả sử lỗi
    if (read_failed_simulated) {
        throw MyFileError("Simulated read failure", fname, 202);
    }
}

// int main() {
//     try {
//         readFile("my_document.txt");
//     } catch (const MyFileError& e) {
//         std::cerr << "MyFileError caught: " << e.what() << std::endl;
//         std::cerr << "Affected file: " << e.getFilename() << ", Error code: " << e.getErrorCode() << std::endl;
//     } catch (const std::exception& e) { // Bắt các ngoại lệ std khác nếu có
//         std::cerr << "Standard exception caught: " << e.what() << std::endl;
//     }
//     return 0;
// }
```

**8. Best Practices & Lưu Ý Chung (Phần 11)**

- **Ném theo giá trị, bắt theo tham chiếu (thường là `const&`):**
  - `throw MyException("error");`
  - `catch (const MyException& e)`
  - Điều này tránh object slicing khi bắt ngoại lệ đa hình và tránh chi phí copy không cần thiết.
- **Sử dụng ngoại lệ cho các tình huống thực sự "ngoại lệ", không phải cho luồng điều khiển thông thường.** Nếu một điều kiện có thể kiểm tra và xử lý cục bộ, thường không nên dùng exception.
- **RAII là bạn đồng hành tốt nhất của exception safety.** Đảm bảo tài nguyên được giải phóng tự động.
- **Destructors nên là `noexcept` (hoặc không ném ngoại lệ).** Nếu một destructor ném ngoại lệ trong quá trình stack unwinding của một ngoại lệ khác, `std::terminate()` sẽ được gọi.
- **Cung cấp thông tin hữu ích trong thông điệp ngoại lệ.**
- **Tài liệu hóa các ngoại lệ mà một hàm có thể ném.**
- **Cân nhắc mức độ đảm bảo an toàn ngoại lệ (basic, strong, nothrow) mà code của bạn cung cấp.**
- **Không bắt ngoại lệ trừ khi bạn biết cách xử lý nó.** Nếu không, hãy để nó lan truyền lên scope cao hơn.
- **Tránh `catch(...)` trừ khi bạn có lý do rất cụ thể** (ví dụ, ở lớp cao nhất của một thread để log lỗi và kết thúc thread một cách an toàn, hoặc để ném lại).

Xử lý ngoại lệ là một công cụ mạnh mẽ để viết phần mềm C++ đáng tin cậy. Việc hiểu và sử dụng đúng cách là rất quan trọng.

Hết Phần 11.

Tuyệt vời! Chúng ta sẽ tiếp tục với Phần 12, tập trung vào một trong những cải tiến quan trọng nhất của C++ hiện đại để quản lý bộ nhớ an toàn: **Smart Pointers (Con Trỏ Thông Minh)**.

---

**Phần 12: Smart Pointers (Con Trỏ Thông Minh)**

Như đã đề cập ở Phần 3 (Con Trỏ và Quản Lý Bộ Nhớ C-style), việc quản lý bộ nhớ thủ công bằng `new` và `delete` rất dễ gây ra lỗi như memory leaks (rò rỉ bộ nhớ) và dangling pointers (con trỏ lơ lửng). Smart Pointers là các lớp template trong C++ (chủ yếu từ C++11 trở đi, trong header `<memory>`) được thiết kế để tự động hóa việc quản lý vòng đời của các đối tượng được cấp phát động trên heap, áp dụng mạnh mẽ idiom RAII (Resource Acquisition Is Initialization).

Mục tiêu chính của smart pointers là đảm bảo rằng bộ nhớ được giải phóng một cách chính xác khi đối tượng không còn được sử dụng, ngay cả khi có ngoại lệ xảy ra.

Có ba loại smart pointer chính trong C++ hiện đại:

1.  **`std::unique_ptr<T>`**: Quản lý sở hữu độc quyền (exclusive ownership).
2.  **`std::shared_ptr<T>`**: Quản lý sở hữu chia sẻ (shared ownership) thông qua đếm tham chiếu.
3.  **`std::weak_ptr<T>`**: Tham chiếu không sở hữu (non-owning reference) tới một đối tượng được quản lý bởi `std::shared_ptr`, dùng để phá vỡ chu trình tham chiếu vòng (circular references).

**1. `std::unique_ptr<T>` (Sở Hữu Độc Quyền)**

- **Khái niệm:** `std::unique_ptr` là một smart pointer nắm giữ **quyền sở hữu duy nhất** đối với đối tượng mà nó trỏ tới. Khi `unique_ptr` bị hủy (ví dụ, ra khỏi scope), đối tượng mà nó quản lý sẽ tự động được giải phóng bằng `delete` (hoặc một deleter tùy chỉnh nếu được cung cấp).
- **Không thể sao chép (Non-copyable):** Bạn không thể tạo một bản sao của `std::unique_ptr` (không có copy constructor hoặc copy assignment operator). Điều này đảm bảo rằng chỉ có một `unique_ptr` sở hữu đối tượng tại một thời điểm.
- **Có thể di chuyển (Movable):** Quyền sở hữu có thể được **chuyển giao** từ một `unique_ptr` này sang một `unique_ptr` khác bằng cách sử dụng `std::move()`. Sau khi di chuyển, `unique_ptr` gốc sẽ trở thành `nullptr`.
- **Header:** `<memory>`
- **Tạo `std::unique_ptr`:**
  - **Cách ưu tiên (C++14 trở đi): `std::make_unique<T>(args...)`**
    - An toàn hơn với exception (tránh memory leak trong một số trường hợp phức tạp với `new`).
    - Ngắn gọn hơn.
  - Cách C++11: `std::unique_ptr<T>(new T(args...))`
- **Toán tử:**
  - `operator*()` và `operator->()`: Để truy cập đối tượng được quản lý (giống con trỏ thô).
  - `get()`: Trả về con trỏ thô (raw pointer) tới đối tượng được quản lý (chỉ dùng khi cần tương tác với code C-style hoặc code không nhận smart pointer, cẩn thận không `delete` con trỏ này).
  - `release()`: Giải phóng quyền sở hữu đối tượng (trả về con trỏ thô và `unique_ptr` trở thành `nullptr`). Người gọi chịu trách nhiệm `delete` con trỏ này.
  - `reset(ptr = nullptr)`: Hủy đối tượng hiện tại (nếu có) và nhận quyền sở hữu `ptr` mới (hoặc trở thành `nullptr`).

**Ví dụ `std::unique_ptr`:**

```cpp
#include <iostream>
#include <memory> // Cho smart pointers
#include <vector>
#include <string>

struct MyResource {
    int id;
    std::string data;

    MyResource(int i, const std::string& d) : id(i), data(d) {
        std::cout << "MyResource " << id << " created (" << data << ")" << std::endl;
    }
    ~MyResource() {
        std::cout << "MyResource " << id << " destroyed (" << data << ")" << std::endl;
    }
    void print() const {
        std::cout << "Resource ID: " << id << ", Data: " << data << std::endl;
    }
};

// Hàm nhận unique_ptr bằng giá trị (quyền sở hữu được chuyển vào)
void processResource(std::unique_ptr<MyResource> res_ptr) {
    if (res_ptr) {
        std::cout << "Processing resource inside function: ";
        res_ptr->print();
    } else {
        std::cout << "Resource pointer is null inside function." << std::endl;
    }
    // Khi res_ptr ra khỏi scope của hàm này, MyResource sẽ bị hủy
}

// Hàm trả về unique_ptr (quyền sở hữu được chuyển ra)
std::unique_ptr<MyResource> createResource(int id, const std::string& data) {
    // return std::unique_ptr<MyResource>(new MyResource(id, data)); // C++11 way
    return std::make_unique<MyResource>(id, data); // C++14+ way (preferred)
}

int main_unique_ptr() {
    std::cout << "--- Basic unique_ptr usage ---" << std::endl;
    {
        // Tạo unique_ptr sử dụng std::make_unique (C++14+)
        std::unique_ptr<MyResource> ptr1 = std::make_unique<MyResource>(1, "Data A");

        if (ptr1) { // Kiểm tra xem có trỏ tới đối tượng không (như con trỏ thô)
            ptr1->print();
            (*ptr1).data = "Modified Data A"; // Truy cập qua *
            ptr1->print();
        }

        // std::unique_ptr<MyResource> ptr2 = ptr1; // LỖI BIÊN DỊCH: unique_ptr không thể copy

        // Chuyển quyền sở hữu
        std::unique_ptr<MyResource> ptr3 = std::move(ptr1);
        std::cout << "ptr1 is " << (ptr1 ? "not null" : "null") << " after move." << std::endl; // ptr1 is null

        if (ptr3) {
            ptr3->print();
        }
        // ptr3 ra khỏi scope, MyResource(1) bị hủy
    }
    std::cout << "ptr1 and ptr3 have gone out of scope." << std::endl;


    std::cout << "\n--- unique_ptr with functions ---" << std::endl;
    std::unique_ptr<MyResource> p_func = createResource(2, "Data B");
    processResource(std::move(p_func)); // Chuyển quyền sở hữu vào hàm
    // p_func bây giờ là null, MyResource(2) đã bị hủy trong processResource


    std::cout << "\n--- unique_ptr for arrays ---" << std::endl;
    // unique_ptr cũng có thể quản lý mảng động
    // std::unique_ptr<int[]> arr_ptr(new int[5]); // C++11
    std::unique_ptr<int[]> arr_ptr = std::make_unique<int[]>(5); // C++14+ (cho mảng)
                                                                // C++20: std::make_unique_for_overwrite<int[]>(5) cho uninitialized memory
    for (int i = 0; i < 5; ++i) {
        arr_ptr[i] = i * 10; // Sử dụng operator[]
    }
    for (int i = 0; i < 5; ++i) {
        std::cout << "arr_ptr[" << i << "] = " << arr_ptr[i] << std::endl;
    }
    // Khi arr_ptr ra khỏi scope, delete[] sẽ được gọi tự động.


    std::cout << "\n--- unique_ptr with custom deleter ---" << std::endl;
    // Deleter là một function object hoặc lambda tùy chỉnh để giải phóng tài nguyên
    auto custom_file_deleter = [](FILE* fp) {
        if (fp) {
            std::cout << "Custom deleter: Closing file." << std::endl;
            fclose(fp);
        }
    };

    // Mở file (ví dụ, không phải cách tốt nhất để xử lý file, chỉ để minh họa deleter)
    FILE* file_handle = fopen("temp_unique.txt", "w");
    if (file_handle) {
        std::unique_ptr<FILE, decltype(custom_file_deleter)> file_ptr(file_handle, custom_file_deleter);
        // Hoặc dùng: using FileUniquePtr = std::unique_ptr<FILE, void(*)(FILE*)>;
        // FileUniquePtr file_ptr(file_handle, [](FILE* fp){ fclose(fp); });

        fprintf(file_ptr.get(), "Hello from unique_ptr with custom deleter.\n");
        // Khi file_ptr ra khỏi scope, custom_file_deleter sẽ được gọi
    }


    std::cout << "\n--- unique_ptr::reset() and release() ---" << std::endl;
    std::unique_ptr<MyResource> p_reset = std::make_unique<MyResource>(3, "Data C");
    p_reset->print();

    // p_reset.reset(); // Hủy MyResource(3), p_reset trở thành nullptr
    p_reset.reset(new MyResource(4, "Data D")); // Hủy MyResource(3), p_reset quản lý MyResource(4)
    std::cout << "After reset with new resource: ";
    p_reset->print();

    MyResource* raw_ptr = p_reset.release(); // p_reset từ bỏ quyền sở hữu, raw_ptr trỏ tới MyResource(4)
    std::cout << "p_reset is " << (p_reset ? "not null" : "null") << " after release." << std::endl;
    if (raw_ptr) {
        std::cout << "Raw pointer after release: ";
        raw_ptr->print();
        delete raw_ptr; // QUAN TRỌNG: Phải tự delete vì unique_ptr đã release
        raw_ptr = nullptr;
    }

    std::cout << "End of main_unique_ptr." << std::endl;
    return 0;
}
```

- **Khi nào dùng `std::unique_ptr`:**
  - Khi bạn muốn một con trỏ duy nhất chịu trách nhiệm quản lý vòng đời của một đối tượng động.
  - Làm thành viên dữ liệu của lớp để quản lý tài nguyên mà lớp đó sở hữu độc quyền.
  - Trả về đối tượng được cấp phát động từ một hàm factory.
  - Lưu trữ trong các container STL (ví dụ, `std::vector<std::unique_ptr<MyClass>>`) nếu các đối tượng là đa hình hoặc lớn, và bạn muốn tránh chi phí copy và object slicing. (Lưu ý: khi thêm vào container, cần dùng `std::move`).

**2. `std::shared_ptr<T>` (Sở Hữu Chia Sẻ)**

- **Khái niệm:** `std::shared_ptr` cho phép **nhiều smart pointer cùng sở hữu** một đối tượng động. Nó sử dụng kỹ thuật **đếm tham chiếu (reference counting)**.
- **Đếm tham chiếu:** Một khối điều khiển (control block) được cấp phát cùng với đối tượng (hoặc khi `shared_ptr` đầu tiên được tạo cho một con trỏ thô). Khối điều khiển này chứa:
  - Số lượng `shared_ptr` đang cùng trỏ tới đối tượng (use count / strong reference count).
  - Số lượng `weak_ptr` đang trỏ tới đối tượng (weak count).
  - Có thể cả deleter tùy chỉnh và allocator.
- Khi một `shared_ptr` mới được tạo bằng cách copy một `shared_ptr` hiện có, hoặc gán một `shared_ptr` cho một `shared_ptr` khác, use count sẽ tăng lên.
- Khi một `shared_ptr` bị hủy (ra khỏi scope, hoặc được gán lại), use count sẽ giảm đi.
- **Đối tượng được giải phóng (bằng `delete`) khi use count trở về 0.**
- **Có thể sao chép (Copyable):** Khác với `unique_ptr`.
- **Header:** `<memory>`
- **Tạo `std::shared_ptr`:**
  - **Cách ưu tiên (an toàn và hiệu quả hơn): `std::make_shared<T>(args...)`**
    - Cấp phát đối tượng `T` và khối điều khiển trong một lần cấp phát bộ nhớ duy nhất (thường hiệu quả hơn).
    - An toàn hơn với exception.
  - Cách khác: `std::shared_ptr<T>(new T(args...))` (sẽ có hai lần cấp phát: một cho `T`, một cho khối điều khiển).
- **Toán tử:** Tương tự `unique_ptr` (`*`, `->`, `get()`, `reset()`).
  - `use_count()`: Trả về số lượng `shared_ptr` đang cùng sở hữu đối tượng.

**Ví dụ `std::shared_ptr`:**

```cpp
#include <iostream>
#include <memory>
#include <string>
#include <vector>

// Sử dụng lại struct MyResource từ ví dụ unique_ptr

void observeResource(std::shared_ptr<MyResource> sp_observe, const std::string& observer_name) {
    std::cout << observer_name << " observing. Use count: " << sp_observe.use_count() << ". ";
    if (sp_observe) {
        sp_observe->print();
    } else {
        std::cout << "Resource is null for " << observer_name << std::endl;
    }
    // Khi sp_observe ra khỏi scope, use_count giảm
}

std::shared_ptr<MyResource> createSharedResource(int id, const std::string& data) {
    // return std::shared_ptr<MyResource>(new MyResource(id, data)); // Cách cũ, 2 allocations
    return std::make_shared<MyResource>(id, data); // Ưu tiên, 1 allocation
}


int main_shared_ptr() {
    std::cout << "--- Basic shared_ptr usage ---" << std::endl;
    std::shared_ptr<MyResource> sp1; // sp1 là nullptr, use_count là 0

    {
        std::shared_ptr<MyResource> sp2 = std::make_shared<MyResource>(10, "Shared Data X");
        std::cout << "sp2 use count: " << sp2.use_count() << std::endl; // 1

        sp1 = sp2; // sp1 và sp2 cùng trỏ tới MyResource(10). Copy tăng use_count.
        std::cout << "sp1 use count after assignment: " << sp1.use_count() << std::endl; // 2
        std::cout << "sp2 use count after assignment: " << sp2.use_count() << std::endl; // 2

        std::shared_ptr<MyResource> sp3 = sp2; // Copy constructor, sp3 cũng trỏ tới MyResource(10)
        std::cout << "sp3 use count: " << sp3.use_count() << std::endl; // 3

        if (sp1) {
            sp1->data = "Modified Shared Data X";
            sp3->print(); // Sẽ thấy dữ liệu đã thay đổi vì cùng trỏ tới 1 đối tượng
        }
        // Khi sp3 ra khỏi scope, use_count giảm còn 2
    }
    std::cout << "sp3 has gone out of scope." << std::endl;
    std::cout << "sp1 use count: " << (sp1 ? sp1.use_count() : 0) << std::endl; // 2 (nếu sp1 còn trỏ)
    // Khi sp2 (trong scope của nó, nếu có) ra khỏi scope, use_count giảm còn 1 (nếu sp1 vẫn còn)

    std::cout << "\n--- shared_ptr with functions ---" << std::endl;
    std::shared_ptr<MyResource> sp_func = createSharedResource(20, "Shared Data Y");
    std::cout << "Initial use count for sp_func: " << sp_func.use_count() << std::endl; // 1

    observeResource(sp_func, "Observer 1"); // Truyền bằng giá trị (copy), use_count tăng lên 2 trong hàm, rồi giảm khi hàm kết thúc
    std::cout << "Use count for sp_func after Observer 1: " << sp_func.use_count() << std::endl; // 1

    std::vector<std::shared_ptr<MyResource>> sp_vector;
    sp_vector.push_back(sp_func); // Copy vào vector, use_count tăng
    std::cout << "Use count for sp_func after push_back to vector: " << sp_func.use_count() << std::endl; // 2
    std::cout << "Use count for vector element: " << sp_vector[0].use_count() << std::endl; // 2

    sp_func.reset(); // sp_func từ bỏ quyền sở hữu, use_count của đối tượng giảm
                     // Nếu sp_vector[0] là shared_ptr cuối cùng, đối tượng sẽ bị hủy
    std::cout << "sp_func is " << (sp_func ? "not null" : "null") << " after reset." << std::endl;
    std::cout << "Use count for vector element after sp_func.reset(): "
              << (sp_vector[0] ? sp_vector[0].use_count() : 0) << std::endl; // 1

    sp_vector.clear(); // Các shared_ptr trong vector bị hủy, use_count của đối tượng giảm về 0
                       // MyResource(20) sẽ bị hủy ở đây (nếu chưa bị hủy bởi sp_func.reset())
    std::cout << "Vector cleared." << std::endl;


    std::cout << "\n--- shared_ptr from raw pointer (cẩn thận) ---" << std::endl;
    MyResource* raw_res = new MyResource(30, "Raw for Shared");
    // std::shared_ptr<MyResource> sp_raw1(raw_res);
    // std::shared_ptr<MyResource> sp_raw2(raw_res); // LỖI LOGIC NGHIÊM TRỌNG!
                                                 // Điều này tạo 2 khối điều khiển riêng biệt cho cùng 1 con trỏ thô.
                                                 // Khi sp_raw1 bị hủy, raw_res bị delete.
                                                 // Khi sp_raw2 bị hủy, nó sẽ cố delete raw_res một lần nữa (double free).

    // Cách đúng khi cần tạo shared_ptr từ con trỏ thô đã tồn tại (và bạn biết bạn đang làm gì):
    // Chỉ tạo shared_ptr MỘT LẦN từ con trỏ thô, sau đó chỉ copy shared_ptr đó.
    std::shared_ptr<MyResource> sp_from_raw_correct(raw_res);
    std::cout << "Use count for sp_from_raw_correct: " << sp_from_raw_correct.use_count() << std::endl; // 1
    // std::shared_ptr<MyResource> sp_another_from_raw = sp_from_raw_correct; // OK, đây là copy shared_ptr

    // TỐT NHẤT: Tránh dùng `new` trực tiếp. Hãy dùng `std::make_shared`.

    std::cout << "End of main_shared_ptr." << std::endl;
    // sp1, sp_from_raw_correct ra khỏi scope, các đối tượng tương ứng được hủy nếu use_count về 0.
    return 0;
}
```

- **Khi nào dùng `std::shared_ptr`:**
  - Khi bạn cần nhiều con trỏ cùng tham chiếu và chia sẻ quyền sở hữu một đối tượng động.
  - Khi vòng đời của đối tượng không rõ ràng hoặc phụ thuộc vào nhiều phần của chương trình.
  - Lưu trữ trong các container STL khi cần chia sẻ đối tượng giữa các phần tử hoặc các container khác nhau.
- **Nhược điểm:**
  - Chi phí nhỏ cho việc đếm tham chiếu (atomic operations cho thread-safety của việc tăng/giảm count).
  - Khối điều khiển chiếm thêm bộ nhớ.
  - **Nguy cơ tham chiếu vòng (Circular References):** Nếu hai hoặc nhiều đối tượng `shared_ptr` trỏ tới nhau tạo thành một chu trình, use count của chúng sẽ không bao giờ về 0, dẫn đến memory leak. `std::weak_ptr` được dùng để giải quyết vấn đề này.

**3. `std::weak_ptr<T>` (Tham Chiếu Yếu)**

- **Khái niệm:** `std::weak_ptr` là một smart pointer **không sở hữu** (non-owning) trỏ tới một đối tượng đang được quản lý bởi một hoặc nhiều `std::shared_ptr`.
- Nó **không ảnh hưởng đến use count** của đối tượng. Do đó, nó không ngăn cản đối tượng bị hủy khi tất cả các `shared_ptr` sở hữu nó bị hủy.
- **Mục đích chính:** Phá vỡ các chu trình tham chiếu vòng (circular references) giữa các đối tượng `shared_ptr`.
- **Không thể dereference trực tiếp:** Bạn không thể truy cập đối tượng trực tiếp qua `weak_ptr` bằng `*` hoặc `->`.
- **Cách sử dụng:**
  - `expired()`: Kiểm tra xem đối tượng được tham chiếu có còn tồn tại không (use count > 0).
  - `lock()`: Trả về một `std::shared_ptr` tới đối tượng.
    - Nếu đối tượng còn tồn tại, `lock()` trả về một `shared_ptr` hợp lệ (và tạm thời tăng use count).
    - Nếu đối tượng đã bị hủy, `lock()` trả về một `shared_ptr` rỗng (`nullptr`).
- **Header:** `<memory>`
- **Tạo `std::weak_ptr`:** Thường được tạo từ một `std::shared_ptr`.

**Ví dụ `std::weak_ptr` và phá vỡ tham chiếu vòng:**

```cpp
#include <iostream>
#include <memory>
#include <string>

struct Son; // Forward declaration
struct Daughter;

struct Parent {
    std::string name;
    // std::shared_ptr<Son> son; // Nếu dùng shared_ptr ở đây -> tham chiếu vòng
    // std::shared_ptr<Daughter> daughter;
    std::weak_ptr<Son> son; // Sử dụng weak_ptr để phá vỡ chu trình
    std::weak_ptr<Daughter> daughter;

    Parent(const std::string& n) : name(n) {
        std::cout << "Parent " << name << " created." << std::endl;
    }
    ~Parent() {
        std::cout << "Parent " << name << " destroyed." << std::endl;
    }
    void showChildren() {
        if (auto s = son.lock()) { // Cố gắng lấy shared_ptr từ weak_ptr
            std::cout << name << "'s son: " << s->name << std::endl;
        } else {
            std::cout << name << " has no son or son is gone." << std::endl;
        }
        if (auto d = daughter.lock()) {
            std::cout << name << "'s daughter: " << d->name << std::endl;
        } else {
            std::cout << name << " has no daughter or daughter is gone." << std::endl;
        }
    }
};

struct Son {
    std::string name;
    std::weak_ptr<Parent> mother; // Hoặc father
    std::weak_ptr<Daughter> sister;

    Son(const std::string& n) : name(n) {
        std::cout << "Son " << name << " created." << std::endl;
    }
    ~Son() {
        std::cout << "Son " << name << " destroyed." << std::endl;
    }
};

struct Daughter {
    std::string name;
    std::weak_ptr<Parent> mother; // Hoặc father
    std::weak_ptr<Son> brother;

    Daughter(const std::string& n) : name(n) {
        std::cout << "Daughter " << name << " created." << std::endl;
    }
    ~Daughter() {
        std::cout << "Daughter " << name << " destroyed." << std::endl;
    }
};


int main_weak_ptr() {
    std::cout << "--- weak_ptr example (breaking circular references) ---" << std::endl;
    {
        std::shared_ptr<Parent> mom = std::make_shared<Parent>("Jane");
        std::shared_ptr<Son> son_obj = std::make_shared<Son>("John");
        std::shared_ptr<Daughter> daughter_obj = std::make_shared<Daughter>("Jill");

        // Thiết lập mối quan hệ
        // Nếu tất cả đều là shared_ptr: mom->son, son->mother, mom->daughter, daughter->mother
        // sẽ tạo tham chiếu vòng, không ai bị hủy.

        mom->son = son_obj;         // Parent giữ weak_ptr tới Son
        mom->daughter = daughter_obj;

        son_obj->mother = mom;      // Son giữ weak_ptr tới Parent
        son_obj->sister = daughter_obj;

        daughter_obj->mother = mom; // Daughter giữ weak_ptr tới Parent
        daughter_obj->brother = son_obj;


        std::cout << "Mom's son use_count (from mom.son weak_ptr perspective, only if locked): "
                  << mom->son.lock().use_count() << std::endl; // Sẽ là 2 (son_obj gốc và cái tạm thời từ lock())
        std::cout << "Son_obj original use_count: " << son_obj.use_count() << std::endl; // 1


        mom->showChildren();

        // Thử truy cập parent từ son
        if (std::shared_ptr<Parent> p = son_obj->mother.lock()) {
            std::cout << son_obj->name << "'s mother is " << p->name << std::endl;
        }

        // Khi mom, son_obj, daughter_obj ra khỏi scope, use_count của shared_ptr gốc giảm.
        // Vì các tham chiếu qua lại là weak_ptr, chúng không giữ use_count > 0.
        // Do đó, tất cả các đối tượng sẽ được hủy đúng cách.
    }
    std::cout << "All family members have gone out of scope." << std::endl;


    std::cout << "\n--- weak_ptr expired example ---" << std::endl;
    std::weak_ptr<MyResource> wp_res;
    {
        std::shared_ptr<MyResource> sp_res_temp = std::make_shared<MyResource>(40, "Temporary Resource");
        wp_res = sp_res_temp; // wp_res trỏ tới đối tượng được quản lý bởi sp_res_temp

        std::cout << "Before sp_res_temp goes out of scope:" << std::endl;
        std::cout << "wp_res.expired(): " << std::boolalpha << wp_res.expired() << std::endl; // false
        if (auto locked_sp = wp_res.lock()) {
            std::cout << "Locked resource: ";
            locked_sp->print();
            std::cout << "Use count via locked_sp: " << locked_sp.use_count() << std::endl; // 2 (sp_res_temp và locked_sp)
        }
        // sp_res_temp ra khỏi scope, MyResource(40) bị hủy
    }
    std::cout << "After sp_res_temp has gone out of scope:" << std::endl;
    std::cout << "wp_res.expired(): " << std::boolalpha << wp_res.expired() << std::endl; // true
    if (auto locked_sp = wp_res.lock()) { // Sẽ không vào đây vì đối tượng đã bị hủy
        std::cout << "This should not print. Locked resource: ";
        locked_sp->print();
    } else {
        std::cout << "Cannot lock wp_res, object has been destroyed." << std::endl;
    }

    std::cout << "End of main_weak_ptr." << std::endl;
    return 0;
}
```

- **Khi nào dùng `std::weak_ptr`:**
  - Để quan sát một đối tượng mà không ảnh hưởng đến vòng đời của nó.
  - Để phá vỡ các chu trình tham chiếu vòng khi sử dụng `std::shared_ptr`.
  - Trong các cấu trúc dữ liệu cache, nơi bạn muốn giữ tham chiếu đến đối tượng nhưng cho phép nó bị hủy nếu không còn ai khác sử dụng.

**4. `std::enable_shared_from_this<T>`**

- **Vấn đề:** Đôi khi, bên trong một hàm thành viên của một lớp `T`, bạn muốn lấy một `std::shared_ptr<T>` trỏ đến chính đối tượng `*this` đang được quản lý bởi `shared_ptr`. Nếu bạn chỉ đơn giản là `return std::shared_ptr<T>(this);`, bạn sẽ tạo ra một khối điều khiển mới cho cùng một con trỏ thô `this`, dẫn đến double free (tương tự như ví dụ `sp_raw1`, `sp_raw2` ở trên).
- **Giải pháp:** Kế thừa lớp của bạn từ `std::enable_shared_from_this<T>`.
  - Nó cung cấp một hàm thành viên `shared_from_this()` trả về một `std::shared_ptr<T>` hợp lệ (chia sẻ quyền sở hữu với các `shared_ptr` khác đang quản lý đối tượng).
  - **Quan trọng:** `shared_from_this()` chỉ có thể được gọi trên một đối tượng đã được quản lý bởi một `std::shared_ptr` (tức là, ít nhất một `shared_ptr` đã được tạo và trỏ tới đối tượng đó). Gọi trên đối tượng chưa được `shared_ptr` quản lý sẽ dẫn đến undefined behavior (thường là ném `std::bad_weak_ptr`).

**Ví dụ `std::enable_shared_from_this`:**

```cpp
#include <iostream>
#include <memory>
#include <vector>

// Lớp kế thừa từ std::enable_shared_from_this
class GameObject : public std::enable_shared_from_this<GameObject> {
public:
    std::string name;
    std::vector<std::shared_ptr<GameObject>> children; // Ví dụ, một cây đối tượng game

    GameObject(const std::string& n) : name(n) {
        std::cout << "GameObject " << name << " created." << std::endl;
    }
    ~GameObject() {
        std::cout << "GameObject " << name << " destroyed." << std::endl;
    }

    void addChild(std::shared_ptr<GameObject> child) {
        children.push_back(child);
        // Giả sử child cần một tham chiếu ngược lại tới parent
        // Nếu child có một thành viên weak_ptr<GameObject> parent_ptr;
        // child->parent_ptr = shared_from_this(); // Lấy shared_ptr tới *this
    }

    // Hàm trả về shared_ptr tới chính nó
    std::shared_ptr<GameObject> getSelfSharedPtr() {
        try {
            return shared_from_this();
        } catch (const std::bad_weak_ptr& e) {
            std::cerr << "Error: shared_from_this() called on object not managed by shared_ptr: "
                      << e.what() << std::endl;
            return nullptr;
        }
    }

    void printName() const {
        std::cout << "My name is " << name << std::endl;
    }
};

int main_enable_shared() {
    // Tạo đối tượng GameObject và quản lý bằng shared_ptr
    std::shared_ptr<GameObject> root = std::make_shared<GameObject>("RootNode");

    // Gọi getSelfSharedPtr() là an toàn vì root đang được quản lý bởi shared_ptr
    std::shared_ptr<GameObject> self_ptr_from_root = root->getSelfSharedPtr();
    if (self_ptr_from_root) {
        std::cout << "Got self_ptr_from_root. Use count of root: " << root.use_count() << std::endl; // Sẽ là 2
        self_ptr_from_root->printName();
    }

    std::shared_ptr<GameObject> child1 = std::make_shared<GameObject>("Child1");
    root->addChild(child1); // Giả sử addChild sử dụng shared_from_this() của root để gán parent cho child1

    // Ví dụ về trường hợp gọi shared_from_this() không an toàn:
    // GameObject raw_obj("RawObject");
    // std::shared_ptr<GameObject> bad_ptr = raw_obj.getSelfSharedPtr(); // Sẽ ném std::bad_weak_ptr
                                                                      // vì raw_obj không được shared_ptr quản lý

    std::cout << "End of main_enable_shared." << std::endl;
    return 0;
}
```

**5. Best Practices & Lưu Ý Chung (Phần 12)**

- **Ưu tiên `std::make_unique` và `std::make_shared`** để tạo smart pointers thay vì dùng `new` trực tiếp. Chúng an toàn hơn và thường hiệu quả hơn.
- **Không trộn lẫn con trỏ thô và smart pointers một cách tùy tiện.** Một khi một đối tượng được quản lý bởi smart pointer, hãy để smart pointer quản lý vòng đời của nó.
- **Không tạo nhiều `shared_ptr` độc lập từ cùng một con trỏ thô.** Điều này sẽ dẫn đến nhiều khối điều khiển và double free.
- **Sử dụng `std::unique_ptr` làm lựa chọn mặc định** khi bạn không cần chia sẻ quyền sở hữu. Nó nhẹ hơn và đơn giản hơn `shared_ptr`.
- **Chuyển `std::unique_ptr` bằng `std::move()`** khi cần chuyển quyền sở hữu (ví dụ, truyền vào hàm hoặc trả về từ hàm).
- **Sử dụng `std::shared_ptr` khi cần chia sẻ quyền sở hữu.**
- **Sử dụng `std::weak_ptr` để phá vỡ tham chiếu vòng** hoặc để quan sát đối tượng mà không ảnh hưởng đến vòng đời của nó.
- **Luôn `lock()` một `weak_ptr` để lấy `shared_ptr` trước khi sử dụng.** Kiểm tra xem `shared_ptr` kết quả có rỗng không.
- **Nếu một lớp cần cung cấp `shared_ptr` tới chính nó từ bên trong hàm thành viên, hãy kế thừa từ `std::enable_shared_from_this`.**
- **Cẩn thận khi dùng `get()`** để lấy con trỏ thô. Đảm bảo không `delete` con trỏ đó và nó không tồn tại lâu hơn smart pointer quản lý nó.
- **Smart pointers không phải là thuốc chữa bách bệnh cho tất cả các vấn đề quản lý bộ nhớ,** nhưng chúng giải quyết một cách hiệu quả các trường hợp phổ biến nhất liên quan đến cấp phát động trên heap.

Smart pointers là một bước tiến lớn trong C++ hiện đại, giúp viết code an toàn hơn, dễ quản lý hơn và ít bị lỗi liên quan đến bộ nhớ hơn rất nhiều.

Hết Phần 12.

Tuyệt vời! Tiếp tục với Phần 13, chúng ta sẽ khám phá các tính năng về **Move Semantics (Ngữ Nghĩa Di Chuyển)** và **Rvalue References (Tham Chiếu Giá Trị Phải)**, những cải tiến quan trọng trong C++11 giúp tối ưu hóa hiệu năng bằng cách tránh các sao chép không cần thiết.

---

**Phần 13: Move Semantics và Rvalue References (C++11)**

Trước C++11, việc sao chép các đối tượng lớn (như `std::vector` chứa nhiều phần tử, hoặc `std::string` dài) có thể rất tốn kém về hiệu năng, đặc biệt khi các đối tượng này là tạm thời hoặc sắp bị hủy. Move semantics cung cấp một cơ chế để "đánh cắp" tài nguyên (như con trỏ tới bộ nhớ động) từ một đối tượng (thường là đối tượng tạm thời - rvalue) sang một đối tượng khác, thay vì thực hiện một bản sao sâu (deep copy) tốn kém.

**1. Lvalues và Rvalues (Giá Trị Trái và Giá Trị Phải)**

Để hiểu move semantics, trước tiên cần phân biệt lvalues và rvalues:

- **Lvalue (Left-value):**
  - Là một biểu thức tham chiếu đến một **vị trí bộ nhớ có thể xác định (có địa chỉ)**.
  - Lvalues thường xuất hiện ở **bên trái** của một phép gán.
  - Bạn có thể lấy địa chỉ của một lvalue bằng toán tử `&`.
  - Ví dụ: tên biến ( `int x; x` là lvalue), phần tử mảng (`arr[0]`), kết quả dereference con trỏ (`*ptr`), một tham chiếu lvalue (`int& ref = x; ref` là lvalue).
- **Rvalue (Right-value):**
  - Là một biểu thức tham chiếu đến một **giá trị tạm thời** hoặc một **literal (hằng)** mà **không có vị trí bộ nhớ cố định** có thể truy cập trực tiếp từ bên ngoài biểu thức đó.
  - Rvalues thường xuất hiện ở **bên phải** của một phép gán.
  - Bạn **không thể** lấy địa chỉ của một rvalue bằng toán tử `&` (thường là vậy, có một số ngoại lệ với string literals).
  - Ví dụ: hằng số (`10`, `3.14`, `"hello"`), kết quả của một hàm trả về bằng giá trị (`getVal()`), các đối tượng tạm thời được tạo ra trong một biểu thức (`x + y`, `MyClass()`).

**2. Rvalue References (`&&`)**

- C++11 giới thiệu một loại tham chiếu mới gọi là **rvalue reference**, được ký hiệu bằng `&&`.
- Một rvalue reference có thể **liên kết (bind) với một rvalue** (đối tượng tạm thời). Nó không thể liên kết trực tiếp với một lvalue.
- Mục đích chính của rvalue references là cho phép xác định một đối tượng là tạm thời và có thể "đánh cắp" tài nguyên từ nó một cách an toàn.
- Cú pháp: `Type&& rvalue_ref = rvalue_expression;`

```cpp
#include <iostream>
#include <string>
#include <vector>

std::string getName() {
    return "Temporary Name"; // Trả về rvalue (một std::string tạm thời)
}

int main_lvalue_rvalue() {
    int x = 10;        // x là lvalue
    int y = 20;        // y là lvalue
    int& lref_x = x;   // lref_x là lvalue reference, liên kết với lvalue x

    // int&& rref_x = x; // LỖI: không thể liên kết rvalue reference với lvalue x
    int&& rref_literal = 5; // OK: 5 là rvalue (literal)
    int&& rref_sum = x + y; // OK: x + y tạo ra một rvalue (int tạm thời)
    std::string&& rref_str_temp = getName(); // OK: getName() trả về rvalue

    std::cout << "rref_literal: " << rref_literal << std::endl;
    std::cout << "rref_sum: " << rref_sum << std::endl;
    std::cout << "rref_str_temp: " << rref_str_temp << std::endl;

    // Có thể sửa đổi qua rvalue reference (vì nó vẫn là một tham chiếu)
    rref_literal = 50;
    std::cout << "Modified rref_literal: " << rref_literal << std::endl; // Output: 50

    // std::string s = "Hello";
    // std::string&& rref_s = s; // LỖI

    return 0;
}
```

**3. Move Constructor (Hàm Dựng Di Chuyển)**

- **Mục đích:** Cho phép một đối tượng mới "đánh cắp" tài nguyên (như con trỏ tới bộ nhớ động, file handles, etc.) từ một đối tượng nguồn (thường là rvalue) thay vì sao chép chúng. Đối tượng nguồn sau đó được để lại ở một trạng thái hợp lệ nhưng không xác định (thường là rỗng hoặc mặc định), sẵn sàng để bị hủy.
- **Signature điển hình:** `ClassName(ClassName&& other) noexcept;`
  - Tham số là một rvalue reference (`ClassName&&`).
  - Thường được đánh dấu `noexcept` vì move constructor không nên ném ngoại lệ. Nếu nó ném ngoại lệ, có thể khó đảm bảo strong exception safety cho các thuật toán STL (ví dụ, khi `std::vector` reallocate).
- **Hoạt động:**
  1.  Sao chép nông (shallow copy) các con trỏ tài nguyên từ `other` sang đối tượng hiện tại (`*this`).
  2.  Đặt các con trỏ tài nguyên trong `other` thành `nullptr` (hoặc trạng thái rỗng khác) để `other` không còn sở hữu tài nguyên đó nữa và destructor của `other` sẽ không giải phóng tài nguyên đã được chuyển đi.

**4. Move Assignment Operator (Toán Tử Gán Di Chuyển)**

- **Mục đích:** Tương tự move constructor, nhưng cho phép gán. "Đánh cắp" tài nguyên từ đối tượng nguồn (thường là rvalue) và gán cho đối tượng đích.
- **Signature điển hình:** `ClassName& operator=(ClassName&& other) noexcept;`
  - Tham số là một rvalue reference.
  - Trả về tham chiếu tới `*this` (`ClassName&`).
  - Thường được đánh dấu `noexcept`.
- **Hoạt động:**
  1.  Giải phóng tài nguyên hiện tại của `*this` (nếu có).
  2.  Sao chép nông các con trỏ tài nguyên từ `other` sang `*this`.
  3.  Đặt các con trỏ tài nguyên trong `other` thành `nullptr`.

**Ví dụ với lớp `MyBuffer` có Move Constructor và Move Assignment:**

```cpp
#include <iostream>
#include <utility> // Cho std::move, std::swap
#include <cstring> // Cho strlen, strcpy
#include <algorithm> // Cho std::copy (thay thế strcpy nếu cần an toàn hơn)

class MyBuffer {
private:
    char* data_ = nullptr;
    size_t size_ = 0;

    void log(const char* msg) const {
        std::cout << "[" << (data_ ? data_ : "null") << ", size=" << size_ << "] " << msg << std::endl;
    }

public:
    // Constructor mặc định
    MyBuffer() : data_(nullptr), size_(0) {
        log("Default constructor");
    }

    // Constructor với kích thước và dữ liệu
    MyBuffer(const char* str) {
        log("Parameterized constructor");
        if (str) {
            size_ = strlen(str);
            data_ = new char[size_ + 1];
            strcpy(data_, str); // Hoặc std::copy(str, str + size_ + 1, data_);
        } else {
            data_ = nullptr;
            size_ = 0;
        }
    }

    // Destructor
    ~MyBuffer() {
        log("Destructor");
        delete[] data_; // An toàn khi delete nullptr
    }

    // Copy Constructor (Deep Copy)
    MyBuffer(const MyBuffer& other) {
        log("Copy constructor from");
        other.log(""); // Log trạng thái của 'other'
        if (other.data_) {
            size_ = other.size_;
            data_ = new char[size_ + 1];
            strcpy(data_, other.data_);
        } else {
            data_ = nullptr;
            size_ = 0;
        }
    }

    // Copy Assignment Operator (Copy-and-Swap Idiom)
    MyBuffer& operator=(const MyBuffer& other) {
        log("Copy assignment operator from");
        other.log("");
        if (this != &other) { // Tránh tự gán
            MyBuffer temp(other); // Gọi copy constructor (tạo bản sao sâu)
            swap(temp);          // Hoán đổi tài nguyên với bản sao tạm thời
        }
        return *this;
        // temp sẽ bị hủy khi ra khỏi scope, giải phóng tài nguyên cũ của *this (nếu có)
    }

    // === MOVE SEMANTICS ===
    // Move Constructor
    MyBuffer(MyBuffer&& other) noexcept // Quan trọng: noexcept
        : data_(other.data_), size_(other.size_) {
        log("Move constructor from");
        other.log(""); // Log trạng thái của 'other' trước khi bị move

        // Để lại 'other' ở trạng thái hợp lệ nhưng rỗng
        other.data_ = nullptr;
        other.size_ = 0;
        // Quan trọng: không delete[] other.data_ ở đây!
    }

    // Move Assignment Operator
    MyBuffer& operator=(MyBuffer&& other) noexcept // Quan trọng: noexcept
    {
        log("Move assignment operator from");
        other.log(""); // Log trạng thái của 'other' trước khi bị move

        if (this != &other) { // Tránh tự di chuyển
            // 1. Giải phóng tài nguyên hiện tại của *this
            delete[] data_;

            // 2. "Đánh cắp" tài nguyên từ 'other'
            data_ = other.data_;
            size_ = other.size_;

            // 3. Để lại 'other' ở trạng thái hợp lệ rỗng
            other.data_ = nullptr;
            other.size_ = 0;
        }
        return *this;
    }

    // Hàm swap để hỗ trợ copy-and-swap và các thao tác khác
    void swap(MyBuffer& other) noexcept {
        log("swap() with");
        other.log("");
        std::swap(data_, other.data_);
        std::swap(size_, other.size_);
    }

    const char* getData() const { return data_; }
    size_t getSize() const { return size_; }

    void print() const {
        std::cout << "Buffer content: " << (data_ ? data_ : "null")
                  << ", size: " << size_ << std::endl;
    }
};

MyBuffer createBuffer(const char* name) {
    MyBuffer b(name); // Parameterized constructor
    return b; // Trả về bằng giá trị.
              // Nếu NRVO không xảy ra, move constructor SẼ được gọi (nếu có)
              // để di chuyển từ 'b' vào đối tượng tạm thời trả về.
              // Nếu NRVO xảy ra, 'b' có thể được xây dựng trực tiếp tại vị trí của đối tượng nhận.
}

int main_move_semantics() {
    std::cout << "--- Test 1: Initialization and Copy ---" << std::endl;
    MyBuffer b1("Hello");         // Parameterized constructor
    MyBuffer b2 = b1;             // Copy constructor
    MyBuffer b3;                  // Default constructor
    b3 = b1;                      // Copy assignment operator

    b1.print(); b2.print(); b3.print();

    std::cout << "\n--- Test 2: Move Construction ---" << std::endl;
    MyBuffer b4 = MyBuffer("World"); // Tạo đối tượng tạm thời MyBuffer("World") (rvalue)
                                    // Move constructor được gọi để khởi tạo b4 từ rvalue này.
    b4.print();

    MyBuffer b5 = createBuffer("From Function"); // createBuffer trả về rvalue
                                                 // Move constructor (hoặc NRVO)
    b5.print();

    std::cout << "\n--- Test 3: Explicit std::move ---" << std::endl;
    MyBuffer b6("Original");
    b6.print();
    // MyBuffer b7 = b6; // Sẽ gọi copy constructor
    MyBuffer b7 = std::move(b6); // Ép b6 (lvalue) thành rvalue, GỌI MOVE CONSTRUCTOR
                                 // b6 bây giờ ở trạng thái "moved-from" (rỗng)
    std::cout << "After std::move(b6) to b7:" << std::endl;
    b6.print(); // In ra trạng thái của b6 sau khi bị di chuyển
    b7.print();

    std::cout << "\n--- Test 4: Move Assignment ---" << std::endl;
    MyBuffer b8("ToBeMoved");
    MyBuffer b9;
    b9.print();
    b8.print();
    b9 = std::move(b8); // Ép b8 thành rvalue, GỌI MOVE ASSIGNMENT OPERATOR
                        // b8 bây giờ ở trạng thái "moved-from"
    std::cout << "After b9 = std::move(b8):" << std::endl;
    b8.print();
    b9.print();

    std::cout << "\n--- Test 5: Move with std::vector (ví dụ) ---" << std::endl;
    std::vector<MyBuffer> vec_buffers;
    // Khi vector reallocate, nếu MyBuffer có move constructor noexcept,
    // các phần tử sẽ được di chuyển thay vì sao chép, hiệu quả hơn.
    std::cout << "Pushing b1 (copy) to vector:" << std::endl;
    vec_buffers.push_back(b1); // b1 là lvalue, copy constructor được gọi
    b1.print(); // b1 vẫn còn nguyên vẹn

    std::cout << "\nPushing MyBuffer(\"Temp1\") (move) to vector:" << std::endl;
    vec_buffers.push_back(MyBuffer("Temp1")); // MyBuffer("Temp1") là rvalue, move constructor được gọi

    std::cout << "\nPushing std::move(b7) (move) to vector:" << std::endl;
    b7.print();
    vec_buffers.push_back(std::move(b7)); // Ép b7 thành rvalue, move constructor được gọi
    b7.print(); // b7 bây giờ rỗng

    std::cout << "\nContents of vector:" << std::endl;
    for (const auto& buf : vec_buffers) {
        buf.print();
    }

    std::cout << "\n--- End of Tests ---" << std::endl;
    // Destructors sẽ được gọi cho tất cả các đối tượng khi ra khỏi scope
    return 0;
}
```

**5. `std::move()`**

- `std::move` (trong `<utility>`) **không thực sự di chuyển bất cứ thứ gì.**
- Nó là một **phép ép kiểu (cast)**, chuyển một biểu thức (thường là lvalue) thành một **xvalue** (eXpiring value - một loại rvalue đặc biệt).
- Khi bạn dùng `std::move(some_lvalue)`, bạn đang nói với trình biên dịch: "Hãy coi `some_lvalue` như một rvalue. Tôi không cần nội dung của nó nữa, bạn có thể 'đánh cắp' tài nguyên từ nó nếu hàm được gọi có phiên bản nhận rvalue reference."
- Sau khi `std::move` được áp dụng cho một đối tượng, đối tượng đó được coi là ở trạng thái "moved-from". Nó vẫn hợp lệ (destructor có thể được gọi) nhưng nội dung của nó không còn được đảm bảo. **Không nên sử dụng lại giá trị của một đối tượng đã bị `std::move` trừ khi gán lại giá trị mới cho nó.**

**6. Quy Tắc Năm (Rule of Five)**

Mở rộng từ Quy Tắc Ba (Rule of Three) đã đề cập ở Phần 5 (OOP Cơ Sở):

Nếu một lớp cần định nghĩa một trong năm hàm đặc biệt sau (thường là do quản lý tài nguyên thủ công như con trỏ thô):

1.  Destructor
2.  Copy Constructor
3.  Copy Assignment Operator
4.  **Move Constructor**
5.  **Move Assignment Operator**

thì rất có thể nó cần định nghĩa (hoặc `=default` / `=delete`) tất cả năm hàm đó.

- **Nếu bạn định nghĩa copy constructor/assignment mà không định nghĩa move constructor/assignment:** Trình biên dịch sẽ không tự động tạo các phiên bản move. Khi cần di chuyển, các phiên bản copy sẽ được gọi (nếu có thể), làm mất đi lợi ích của move semantics.
- **Nếu bạn định nghĩa move constructor/assignment mà không định nghĩa copy constructor/assignment:** Trình biên dịch sẽ ngầm định nghĩa các phiên bản copy là bị xóa (`=delete`). Điều này có nghĩa là đối tượng sẽ chỉ có thể di chuyển, không thể sao chép.
- **`= default`:** Yêu cầu trình biên dịch tạo phiên bản mặc định của hàm đặc biệt (nếu hợp lệ).
- **`= delete`:** Cấm trình biên dịch tạo hoặc sử dụng hàm đặc biệt đó.

**Quy Tắc Không (Rule of Zero):**
Cách tiếp cận hiện đại và an toàn nhất là cố gắng thiết kế các lớp sao cho chúng **không cần** tự định nghĩa bất kỳ hàm nào trong số năm hàm trên. Thay vào đó, hãy dựa vào các lớp quản lý tài nguyên có sẵn (như smart pointers, các container STL) để chúng tự động xử lý việc sao chép, di chuyển và giải phóng.

**7. Perfect Forwarding (Chuyển Tiếp Hoàn Hảo)**

- **Vấn đề:** Khi viết một hàm template nhận tham số và muốn chuyển tiếp (forward) tham số đó cho một hàm khác, làm thế nào để giữ nguyên "value category" (lvalue/rvalue) và `const`/`volatile` qualifiers của tham số gốc?
- **Giải pháp:** Sử dụng **forwarding references** (còn gọi là universal references) kết hợp với `std::forward`.
- **Forwarding Reference:** Một rvalue reference tới một _kiểu được suy diễn từ template parameter_ ( `T&&` trong `template<typename T> void wrapper(T&& arg)`).
  - Nếu `arg` truyền vào là lvalue kiểu `U`, thì `T` được suy diễn là `U&`, và `T&&` trở thành `U& &&` -> được rút gọn (reference collapsing) thành `U&` (tham chiếu lvalue).
  - Nếu `arg` truyền vào là rvalue kiểu `U`, thì `T` được suy diễn là `U`, và `T&&` trở thành `U&&` (tham chiếu rvalue).
- **`std::forward<T>(arg)`:**
  - Ép kiểu `arg` thành `T&&` (rvalue reference) chỉ khi `arg` ban đầu được truyền vào là một rvalue. Nếu `arg` ban đầu là lvalue, nó sẽ vẫn là lvalue.
  - Điều này đảm bảo rằng giá trị được chuyển tiếp đúng cách.

**Ví dụ Perfect Forwarding:**

```cpp
#include <iostream>
#include <utility> // Cho std::forward, std::move
#include <string>

void real_function(int& x) {
    std::cout << "real_function(int&): lvalue received, x = " << x << std::endl;
    x = 100;
}

void real_function(int&& x) {
    std::cout << "real_function(int&&): rvalue received, x = " << x << std::endl;
    // x = 200; // Có thể sửa đổi rvalue reference
}

void real_function(const std::string& s) {
    std::cout << "real_function(const std::string&): lvalue const ref received, s = " << s << std::endl;
}

void real_function(std::string&& s) {
    std::cout << "real_function(std::string&&): rvalue ref received, s = " << s << std::endl;
    // s = "moved string"; // Có thể sửa đổi
}

// Hàm wrapper sử dụng perfect forwarding
template <typename F, typename... Args> // Variadic template cho các đối số
decltype(auto) wrapper(F&& func, Args&&... args) {
    std::cout << "Wrapper called. Forwarding arguments..." << std::endl;
    // Gọi hàm func, chuyển tiếp hoàn hảo các đối số args
    return std::forward<F>(func)(std::forward<Args>(args)...);
    // decltype(auto) (C++14) để suy diễn kiểu trả về một cách chính xác,
    // bao gồm cả việc nó là tham chiếu hay không.
}

int main_perfect_forwarding() {
    int a = 10;
    std::string str = "Hello";
    const std::string cstr = "Const Hello";

    std::cout << "--- Calling real_function directly ---" << std::endl;
    real_function(a);             // Calls real_function(int&)
    real_function(5);             // Calls real_function(int&&)
    real_function(str);           // Calls real_function(const std::string&)
    real_function(std::move(str)); // Calls real_function(std::string&&)
    real_function(cstr);          // Calls real_function(const std::string&)
    real_function("World");       // Calls real_function(std::string&&) (string literal -> temp std::string)


    std::cout << "\n--- Calling via wrapper ---" << std::endl;
    wrapper(real_function, a);              // Forwards a as lvalue (int&)
    std::cout << "a after wrapper: " << a << std::endl; // a có thể bị thay đổi bởi real_function(int&)

    wrapper(real_function, 50);             // Forwards 50 as rvalue (int&&)

    std::string str_wrap = "Wrapper Test";
    wrapper(real_function, str_wrap);       // Forwards str_wrap as lvalue (const std::string&)

    wrapper(real_function, std::move(str_wrap)); // Forwards str_wrap (now rvalue) as (std::string&&)
    std::cout << "str_wrap after move through wrapper: " << str_wrap << std::endl; // Trạng thái moved-from

    wrapper(real_function, std::string("Temporary via wrapper")); // Forwards temporary as (std::string&&)

    return 0;
}
```

- **Khi nào dùng Perfect Forwarding:** Chủ yếu trong việc viết các hàm template generic (như `std::make_unique`, `std::make_shared`, `std::vector::emplace_back`, `std::bind`, `std::thread` constructor) cần nhận các đối số và chuyển chúng một cách hoàn hảo đến một hàm hoặc constructor khác.

**8. Best Practices & Lưu Ý Chung (Phần 13)**

- **Tuân thủ Quy Tắc Năm (hoặc tốt hơn là Quy Tắc Không).** Nếu lớp của bạn quản lý tài nguyên, hãy cung cấp cả move constructor và move assignment operator.
- **Đánh dấu các hàm move là `noexcept`** nếu chúng thực sự không ném ngoại lệ. Điều này quan trọng cho hiệu năng và tính đúng đắn với các thuật toán STL.
- **Sử dụng `std::move` để chỉ định rõ ràng rằng bạn muốn di chuyển từ một lvalue.** Cẩn thận sau khi di chuyển, đối tượng gốc ở trạng thái không xác định.
- **Move semantics rất hữu ích cho các đối tượng "sink"** (hàm nhận quyền sở hữu đối số) và các hàm "factory" (trả về đối tượng mới).
- **Hiểu rõ sự khác biệt giữa `std::move` và `std::forward`.** `std::move` luôn ép thành rvalue. `std::forward` ép thành rvalue chỉ khi đối số gốc là rvalue (dùng với forwarding references).
- **Rvalue references và move semantics là nền tảng cho nhiều tính năng tối ưu hóa và API hiện đại trong C++.**

Move semantics đã cách mạng hóa cách C++ xử lý việc truyền và trả về các đối tượng, giúp cải thiện đáng kể hiệu năng trong nhiều tình huống.

Hết Phần 13.

OK, chúng ta sẽ tiếp tục với Phần 14, tập trung vào các tính năng và cải tiến quan trọng của **C++ Hiện Đại (Modern C++)**, chủ yếu là từ C++11 trở đi, bao gồm cả một số điểm nổi bật từ C++14, C++17 và C++20. Phần này sẽ không đi sâu vào từng chi tiết của mọi tính năng mà sẽ làm nổi bật những thay đổi quan trọng nhất ảnh hưởng đến cách chúng ta viết code C++.

---

**Phần 14: C++ Hiện Đại (Modern C++ - C++11 và Sau Này)**

C++11 (trước đây gọi là C++0x) là một bản cập nhật lớn và quan trọng cho ngôn ngữ C++, mang lại vô số tính năng mới và cải tiến giúp ngôn ngữ trở nên an toàn hơn, hiệu quả hơn, và dễ sử dụng hơn. Các phiên bản sau (C++14, C++17, C++20, C++23) tiếp tục xây dựng và hoàn thiện trên nền tảng này.

Phần này sẽ điểm qua một số tính năng nổi bật nhất, nhiều trong số đó bạn có thể đã thấy rải rác trong các phần trước.

**1. `auto` và Type Deduction (Suy Diễn Kiểu)**

- **`auto` (C++11):**

  - Cho phép trình biên dịch tự động suy diễn kiểu của một biến từ biểu thức khởi tạo của nó.
  - Giúp code ngắn gọn hơn, dễ đọc hơn (đặc biệt với các kiểu phức tạp như kiểu của iterator hoặc lambda) và giảm thiểu lỗi do gõ sai kiểu.
  - Ví dụ:
    ```cpp
    auto i = 10; // i là int
    auto d = 3.14; // d là double
    auto s = std::string("hello"); // s là std::string
    std::vector<int> vec = {1,2,3};
    auto it = vec.begin(); // it là std::vector<int>::iterator
    auto lambda_func = [](int x){ return x * x; }; // lambda_func có kiểu closure độc nhất
    ```
  - `auto` có thể được dùng với `const`, `&`, `*`:
    ```cpp
    const auto MAX_VAL = 100; // const int
    auto& ref_i = i;          // int&
    const auto& cref_s = s;    // const std::string&
    auto* ptr_i = &i;         // int*
    ```
  - **Lưu ý:** `auto` loại bỏ `const`, `volatile` và tham chiếu ở cấp cao nhất trừ khi được chỉ định rõ.

    ```cpp
    const int ci = 5;
    auto ai = ci; // ai là int (const bị loại bỏ)
    const auto cai = ci; // cai là const int

    int& ri = i;
    auto ar = ri; // ar là int (tham chiếu bị loại bỏ)
    auto& arr = ri; // arr là int&
    ```

- **`decltype` (C++11):**
  - Suy diễn kiểu của một biểu thức mà không cần thực thi biểu thức đó.
  - Hữu ích trong generic programming và khi kiểu phụ thuộc vào template parameters.
  - `decltype(expression)` trả về kiểu của `expression`.
  - Ví dụ:
    ```cpp
    int x = 5;
    decltype(x) y = 10; // y là int
    decltype(x + 3.0) z; // z là double
    const int& getRef();
    decltype(getRef()) ref_val = getRef(); // ref_val là const int&
    ```
  - `decltype(auto)` (C++14): Kết hợp `auto` và `decltype`. Kiểu được suy diễn bằng `decltype(initializer)`, giữ lại `const`/`volatile` và tham chiếu. Thường dùng cho kiểu trả về của hàm template.

**2. Range-based `for` loop (Vòng Lặp Dựa Trên Dãy - C++11)**

- Cung cấp cú pháp đơn giản và an toàn hơn để duyệt qua các phần tử của một container (hoặc bất kỳ đối tượng nào hỗ trợ `begin()` và `end()`, hoặc có `std::begin` và `std::end` được định nghĩa cho nó).
- Ví dụ:

  ```cpp
  std::vector<int> numbers = {1, 2, 3, 4, 5};
  for (int num : numbers) { // num là bản sao của mỗi phần tử
      std::cout << num << " ";
  }
  std::cout << std::endl;

  for (const auto& str : std::vector<std::string>{"a", "b", "c"}) { // Duyệt qua rvalue container
      std::cout << str << " ";
  }
  std::cout << std::endl;

  std::map<std::string, int> scores = {{"Alice", 90}, {"Bob", 85}};
  for (const auto& pair : scores) { // pair là const std::pair<const std::string, int>&
      std::cout << pair.first << ": " << pair.second << std::endl;
  }
  ```

**3. `nullptr` (C++11)**

- Từ khóa đại diện cho con trỏ rỗng, an toàn hơn về kiểu so với `NULL` hoặc `0`. (Đã đề cập ở Phần 3).
- Kiểu của `nullptr` là `std::nullptr_t`.

**4. Lambda Expressions (Biểu Thức Lambda - C++11)**

- Cung cấp cú pháp ngắn gọn để tạo các function object ẩn danh. (Đã đề cập ở Phần 10).
- Rất hữu ích khi dùng với các thuật toán STL.
- **Generic Lambdas (C++14):** Cho phép sử dụng `auto` cho tham số của lambda.
  ```cpp
  auto generic_add = [](auto a, auto b) { // a và b có thể là các kiểu khác nhau
      return a + b;
  };
  std::cout << generic_add(5, 3) << std::endl;      // 8 (int)
  std::cout << generic_add(2.5, 1.1) << std::endl;  // 3.6 (double)
  std::cout << generic_add(std::string("hello"), std::string(" world")) << std::endl; // "hello world"
  ```
- **Lambda Capture `*this` (C++17) và `[=, this]` (C++20):** Cải tiến cách lambda bắt `this` trong hàm thành viên.
- **Lambda Templates (C++20):** Cho phép lambda có template parameters rõ ràng.
  ```cpp
  auto templated_lambda = []<typename T>(T val) {
      return val * val;
  };
  std::cout << templated_lambda(5) << std::endl; // 25
  std::cout << templated_lambda(2.5) << std::endl; // 6.25
  ```

**5. Uniform Initialization (Khởi Tạo Đồng Nhất) và `std::initializer_list` (C++11)**

- **Uniform Initialization:** Sử dụng dấu ngoặc nhọn `{}` để khởi tạo các đối tượng.

  - Mục đích: Cung cấp một cú pháp khởi tạo nhất quán cho tất cả các loại (biến cơ bản, mảng, struct, class, container).
  - Giúp tránh "most vexing parse" (một sự mơ hồ cú pháp của C++ cũ).
  - **Ngăn chặn narrowing conversions (chuyển đổi thu hẹp)**: Nếu giá trị khởi tạo không thể biểu diễn chính xác bằng kiểu của biến mà không mất thông tin, trình biên dịch sẽ báo lỗi (ví dụ, `int x {3.14};` // lỗi).

  ```cpp
  int a = 1;    int b(2);    int c{3};    // Tất cả đều OK
  std::string s1("hello"); std::string s2{"world"};
  std::vector<int> v1 = {1, 2, 3}; std::vector<int> v2{4, 5, 6};
  MyClass obj1(arg1, arg2); MyClass obj2{arg1, arg2};

  // int narrow_err {7.0}; // Lỗi: narrowing conversion
  double d = 5;
  // int no_narrow_err {d}; // Lỗi: narrowing (double to int)
  int no_narrow_ok (d); // OK, nhưng có thể mất thông tin (d = 5.0 -> no_narrow_ok = 5)
  ```

- **`std::initializer_list<T>` (Header `<initializer_list>`):**
  - Một lớp template nhẹ, cho phép các hàm và constructor nhận một danh sách các giá trị được bao trong `{}` (ví dụ: `{1, 2, 3}`).
  - Các container STL (như `std::vector`, `std::map`) có constructor nhận `std::initializer_list`.
  ```cpp
  void print_list(std::initializer_list<int> il) {
      for (int val : il) {
          std::cout << val << " ";
      }
      std::cout << std::endl;
  }
  // print_list({10, 20, 30, 40}); // Output: 10 20 30 40
  ```

**6. Rvalue References và Move Semantics (C++11)**

- Đã được thảo luận chi tiết ở Phần 13.
- Cho phép di chuyển tài nguyên thay vì sao chép, cải thiện đáng kể hiệu năng.
- Bao gồm Move Constructor, Move Assignment Operator, `std::move`, `std::forward`.

**7. Smart Pointers (C++11)**

- `std::unique_ptr`, `std::shared_ptr`, `std::weak_ptr`. (Đã thảo luận chi tiết ở Phần 12).
- Cung cấp quản lý bộ nhớ tự động và an toàn.

**8. `constexpr` - Expressions and Functions (Biểu Thức và Hàm Hằng - C++11, Mở Rộng C++14, C++17)**

- **`constexpr` variable:** Giá trị của biến phải được biết tại thời điểm biên dịch.
- **`constexpr` function:** Một hàm có thể được thực thi tại thời điểm biên dịch nếu tất cả các đối số của nó là hằng số compile-time.
  - C++11: `constexpr` function rất hạn chế (chỉ có một lệnh `return`).
  - C++14, C++17: Nới lỏng các hạn chế, cho phép vòng lặp, `if`, biến cục bộ (miễn là chúng không vi phạm ngữ nghĩa compile-time).
- Mục đích: Cho phép tính toán phức tạp hơn tại compile-time, tối ưu hóa, sử dụng trong các ngữ cảnh yêu cầu hằng số compile-time (ví dụ: kích thước mảng, non-type template arguments).

```cpp
#include <iostream>
#include <array>

// C++11 constexpr function (khá hạn chế)
constexpr int factorial_cpp11(int n) {
    return (n <= 1) ? 1 : (n * factorial_cpp11(n - 1));
}

// C++14+ constexpr function (linh hoạt hơn)
constexpr int sum_up_to_cpp14(int n) {
    int sum = 0;
    for (int i = 1; i <= n; ++i) {
        sum += i;
    }
    return sum;
}

int main_constexpr() {
    constexpr int N = 5;
    const int fact_n = factorial_cpp11(N); // Tính tại compile-time
    std::array<int, fact_n / 10> my_array; // Kích thước mảng phải là hằng compile-time
                                            // fact_n/10 = 120/10 = 12

    std::cout << "Factorial of " << N << " is " << fact_n << std::endl;
    std::cout << "Size of my_array: " << my_array.size() << std::endl;

    constexpr int sum_10 = sum_up_to_cpp14(10); // Tính tại compile-time (55)
    std::cout << "Sum up to 10: " << sum_10 << std::endl;

    int runtime_val = 6;
    // int runtime_fact = factorial_cpp11(runtime_val); // OK, hàm constexpr có thể gọi tại runtime
                                                     // nếu đối số không phải hằng compile-time.
    // std::array<int, factorial_cpp11(runtime_val)> arr2; // LỖI: kích thước mảng phải là const_expr

    return 0;
}
```

**9. Threading Library (Thư Viện Luồng - C++11, Header `<thread>`, `<mutex>`, `<condition_variable>`, `<future>`, `<atomic>`)**

- Cung cấp hỗ trợ chuẩn cho lập trình đa luồng.
  - `std::thread`: Tạo và quản lý luồng.
  - `std::mutex`, `std::lock_guard`, `std::unique_lock`: Đồng bộ hóa truy cập tài nguyên chia sẻ.
  - `std::condition_variable`: Cho phép các luồng chờ một điều kiện nào đó.
  - `std::future`, `std::promise`, `std::async`: Cho lập trình bất đồng bộ và lấy kết quả từ các tác vụ chạy song song.
  - `std::atomic`: Cung cấp các kiểu dữ liệu và thao tác nguyên tử cho lập trình không khóa (lock-free programming).
- Sẽ được thảo luận chi tiết hơn ở một phần riêng.

**10. Tuple (`std::tuple` - C++11, Header `<tuple>`)**

- Một khuôn mẫu lớp cho phép gom một tập hợp các giá trị có kiểu khác nhau thành một đối tượng duy nhất.
- Giống như `std::pair` nhưng cho số lượng phần tử tùy ý.
- Truy cập phần tử bằng `std::get<index>(my_tuple)` hoặc `std::get<Type>(my_tuple)` (nếu kiểu là duy nhất).

```cpp
#include <iostream>
#include <tuple>
#include <string>

int main_tuple() {
    std::tuple<int, std::string, double> t1(10, "Hello", 3.14);

    std::cout << "Int value: " << std::get<0>(t1) << std::endl;
    std::cout << "String value: " << std::get<1>(t1) << std::endl;
    std::cout << "Double value: " << std::get<2>(t1) << std::endl;
    // std::cout << "String value (by type): " << std::get<std::string>(t1) << std::endl;

    std::get<1>(t1) = "World"; // Sửa đổi phần tử

    // Unpacking tuple (C++17 structured bindings)
    auto [id, name, value] = t1; // id=int, name=std::string, value=double
    std::cout << "Unpacked: id=" << id << ", name=" << name << ", value=" << value << std::endl;

    // Trước C++17, dùng std::tie
    int my_id;
    std::string my_name;
    double my_value;
    std::tie(my_id, my_name, my_value) = t1;
    // std::tie(std::ignore, my_name, std::ignore) = t1; // Bỏ qua các phần tử không cần

    auto t2 = std::make_tuple(20, "C++ Tuple", 2.71); // Tạo tuple bằng make_tuple

    return 0;
}
```

**11. Regular Expressions (Biểu Thức Chính Quy - C++11, Header `<regex>`)**

- Cung cấp hỗ trợ cho việc tìm kiếm, khớp và thao tác chuỗi dựa trên các mẫu biểu thức chính quy.
- Bao gồm các lớp như `std::regex`, `std::smatch` (cho `std::string`), `std::cmatch` (cho C-style strings), và các hàm như `std::regex_match`, `std::regex_search`, `std::regex_replace`.

**12. Type Traits (Đặc Tính Kiểu - C++11, Header `<type_traits>`)**

- Cung cấp một tập hợp các khuôn mẫu lớp và hàm để truy vấn các thuộc tính của kiểu tại thời điểm biên dịch (ví dụ: `std::is_integral<T>::value`, `std::is_pointer<T>::value`, `std::is_same<T, U>::value`, `std::remove_reference<T>::type`).
- Rất hữu ích trong template metaprogramming và để viết code generic an toàn hơn (ví dụ, sử dụng `std::enable_if` để bật/tắt các overload của hàm template dựa trên thuộc tính của kiểu).

**13. `std::function` (C++11, Header `<functional>`)**

- Một trình bao bọc (wrapper) kiểu đa hình cho bất kỳ đối tượng có thể gọi được (callable object) nào có một signature cụ thể (ví dụ: con trỏ hàm, lambda, function object).
- Cho phép lưu trữ và truyền các callable object khác nhau một cách đồng nhất.

```cpp
#include <iostream>
#include <functional> // Cho std::function
#include <vector>
#include <string>

void print_num(int i) { std::cout << "Number: " << i << std::endl; }
struct MyFunctor { void operator()(const std::string& s) { std::cout << "Functor says: " << s << std::endl; } };

int main_std_function() {
    std::function<void(int)> f_print_num = print_num;
    f_print_num(10); // Gọi hàm print_num

    std::function<void(const std::string&)> f_functor = MyFunctor();
    f_functor("Hello"); // Gọi MyFunctor::operator()

    std::function<int(int, int)> f_lambda = [](int a, int b) { return a + b; };
    std::cout << "Lambda sum: " << f_lambda(5, 3) << std::endl;

    // Có thể lưu trữ trong container
    std::vector<std::function<void()>> tasks;
    tasks.push_back([](){ std::cout << "Task 1 executed\n"; });
    tasks.push_back([](){ std::cout << "Task 2 executed\n"; });

    for(const auto& task : tasks) {
        task(); // Gọi các lambda
    }
    return 0;
}
```

**14. Các Cải Tiến Từ C++14, C++17, C++20 (Sơ Lược)**

- **C++14:**
  - **Generic lambdas** (đã đề cập).
  - **Return type deduction cho hàm (kể cả non-template):** `auto func() { return 10; }`
  - **`decltype(auto)`** (đã đề cập).
  - **Binary literals:** `0b101010`
  - **Digit separators:** `int large_num = 1'000'000;`
  - **`std::make_unique`** (đã đề cập).
- **C++17:**
  - **Structured Bindings (Ràng Buộc Cấu Trúc):** `auto [x, y, z] = my_tuple;` (đã đề cập).
  - **`if` và `switch` với initializer:** `if (int val = getVal(); val > 0) { ... }`
  - **`inline` variables:** Cho phép định nghĩa biến static trong header.
  - **`std::optional<T>` (Header `<optional>`):** Biểu diễn một giá trị có thể có hoặc không (giống `Nullable<T>` trong các ngôn ngữ khác).
  - **`std::variant<Types...>` (Header `<variant>`):** Một kiểu union an toàn về kiểu (type-safe union), có thể giữ một trong nhiều kiểu khác nhau tại một thời điểm.
  - **`std::any` (Header `<any>`):** Có thể giữ một giá trị của bất kỳ kiểu nào (nhưng cần biết kiểu khi lấy ra).
  - **`std::string_view` (Header `<string_view>`):** Một tham chiếu không sở hữu (non-owning) tới một chuỗi (một phần của `std::string` hoặc C-style string). Hiệu quả để truyền chuỗi chỉ đọc mà không cần copy.
  - **Parallel STL algorithms (Thuật toán STL song song):** Nhiều thuật toán STL có thêm execution policy (ví dụ `std::execution::par`) để chạy song song.
  - **Filesystem library (Thư viện hệ thống tệp, Header `<filesystem>`)**: Thao tác với đường dẫn, thư mục, tệp.
  - **Folding expressions for variadic templates** (đã đề cập).
- **C++20:**

  - **Concepts (Khái Niệm):** Ràng buộc các tham số khuôn mẫu, cải thiện thông báo lỗi template.

    ```cpp
    template<typename T>
    concept Integral = std::is_integral_v<T>; // Định nghĩa concept Integral

    template<Integral T> // Ràng buộc T phải là kiểu nguyên
    void print_integral(T val) { std::cout << val << std::endl; }
    ```

  - **Modules (Mô-đun):** Một cách mới để tổ chức mã nguồn, thay thế một phần cho `#include` truyền thống, cải thiện thời gian biên dịch và che giấu chi tiết triển khai.
  - **Coroutines (Luồng Đồng Tác):** Cho phép viết code bất đồng bộ một cách tuần tự hơn.
  - **Ranges library (Thư viện Dãy):** Cung cấp một cách mới để làm việc với các dãy dữ liệu, bao gồm "views" và các thuật toán thân thiện với range.
    ```cpp
    #include <ranges> // Cần compiler hỗ trợ C++20 và ranges
    // std::vector<int> nums = {1,2,3,4,5,6};
    // auto even_squares = nums
    //    | std::views::filter([](int n){ return n % 2 == 0; })
    //    | std::views::transform([](int n){ return n * n; });
    // for(int x : even_squares) std::cout << x << " "; // 4 16 36
    ```
  - **Three-way comparison operator (`<=>`, "spaceship operator"):** Đơn giản hóa việc viết các toán tử so sánh (`<`, `>`, `==`, etc.).
  - **`consteval` và `constinit`:** Kiểm soát chặt chẽ hơn việc khởi tạo tại compile-time.
  - **Formatted output (`std::format`, Header `<format>`):** Một cách an toàn và linh hoạt hơn `printf` để định dạng chuỗi.

**15. Best Practices & Tư Duy Hiện Đại (Phần 14)**

- **Luôn sử dụng phiên bản C++ mới nhất mà dự án của bạn cho phép.** Các tính năng mới thường giúp code an toàn, rõ ràng và hiệu quả hơn.
- **Ưu tiên các tính năng của thư viện chuẩn** (smart pointers, containers, algorithms) thay vì tự viết lại từ đầu.
- **Viết code "exception-safe"** sử dụng RAII.
- **Tận dụng `auto` và range-based for** để code ngắn gọn và dễ đọc.
- **Sử dụng `nullptr` thay vì `NULL`.**
- **Sử dụng uniform initialization `{}`** khi có thể.
- **Hiểu và sử dụng move semantics** để tối ưu hóa.
- **`constexpr` cho các tính toán compile-time.**
- **Lambdas cho các function object đơn giản.**
- **Khi làm việc với template, cân nhắc Concepts (C++20)** nếu có thể.
- **Tư duy về quản lý tài nguyên và sở hữu.** Ai sở hữu tài nguyên? Vòng đời của nó là gì? Smart pointers giúp trả lời những câu hỏi này.

C++ hiện đại đã phát triển rất nhiều, trở thành một ngôn ngữ mạnh mẽ và linh hoạt hơn bao giờ hết. Việc nắm bắt các tính năng mới này là rất quan trọng để viết code C++ hiệu quả và theo kịp xu hướng.

Hết Phần 14.

Tuyệt vời! Chúng ta sẽ kết thúc loạt bài này với Phần 15, tập trung vào **Lập Trình Đa Luồng (Multithreading) và Đồng Bộ Hóa (Synchronization)** trong C++, sử dụng các tính năng được giới thiệu từ C++11.

---

**Phần 15: Lập Trình Đa Luồng và Đồng Bộ Hóa (C++11 trở đi)**

Lập trình đa luồng cho phép một chương trình thực hiện nhiều tác vụ (luồng thực thi) đồng thời, tận dụng các bộ xử lý đa nhân hiện đại để cải thiện hiệu năng và độ phản hồi của ứng dụng. Tuy nhiên, nó cũng đi kèm với những thách thức về việc quản lý tài nguyên chia sẻ và tránh các vấn_đề như data races (tranh chấp dữ liệu) và deadlocks (khóa chết).

Thư viện chuẩn C++ (từ C++11) cung cấp các công cụ mạnh mẽ để tạo và quản lý luồng, cũng như đồng bộ hóa truy cập vào dữ liệu chia sẻ.

**Header chính:**

- `<thread>`: Cho lớp `std::thread` và các hàm liên quan.
- `<mutex>`: Cho các loại mutex (`std::mutex`, `std::recursive_mutex`, `std::timed_mutex`, etc.) và các công cụ khóa (`std::lock_guard`, `std::unique_lock`).
- `<condition_variable>`: Cho `std::condition_variable` để các luồng chờ đợi điều kiện.
- `<atomic>`: Cho các kiểu dữ liệu và thao tác nguyên tử.
- `<future>` và `<promise>`: Cho lập trình bất đồng bộ và truyền kết quả giữa các luồng.

**1. Tạo và Quản Lý Luồng (`std::thread`)**

- **`std::thread` (Lớp):** Đại diện cho một luồng thực thi riêng lẻ.
- **Tạo luồng:** Một đối tượng `std::thread` được khởi tạo với một callable object (hàm, lambda, function object) mà luồng mới sẽ thực thi.
  - `std::thread my_thread(callable_object, args...);`
- **`join()` (Hàm thành viên):** Chờ cho đến khi luồng kết thúc thực thi. Một luồng **phải** được `join()` hoặc `detach()` trước khi đối tượng `std::thread` quản lý nó bị hủy, nếu không chương trình sẽ gọi `std::terminate()`.
- **`detach()` (Hàm thành viên):** Tách luồng ra khỏi đối tượng `std::thread`. Luồng sẽ tiếp tục chạy độc lập trong nền. Sau khi `detach()`, bạn không còn cách nào để `join()` với luồng đó nữa, và phải tự đảm bảo luồng kết thúc một cách an toàn. Thường ít được khuyến khích hơn `join()`.
- **`get_id()` (Hàm thành viên):** Trả về ID của luồng.
- **`std::this_thread` (Namespace):** Cung cấp các hàm cho luồng hiện tại.
  - `std::this_thread::get_id()`: Lấy ID của luồng đang chạy.
  - `std::this_thread::sleep_for(duration)`: Dừng luồng hiện tại trong một khoảng thời gian.
  - `std::this_thread::sleep_until(time_point)`: Dừng luồng hiện tại cho đến một thời điểm cụ thể.
  - `std::this_thread::yield()`: Gợi ý cho bộ lập lịch nhường thời gian xử lý cho các luồng khác.

**Ví dụ tạo và join luồng:**

```cpp
#include <iostream>
#include <thread>   // Cho std::thread
#include <chrono>   // Cho std::chrono::milliseconds
#include <vector>
#include <string>

void worker_function_no_args() {
    std::cout << "Worker thread (no args) ID: " << std::this_thread::get_id() << " started." << std::endl;
    std::this_thread::sleep_for(std::chrono::milliseconds(1000)); // Giả lập công việc
    std::cout << "Worker thread (no args) ID: " << std::this_thread::get_id() << " finished." << std::endl;
}

void worker_function_with_args(int id, const std::string& message) {
    std::cout << "Worker thread " << id << " (ID: " << std::this_thread::get_id() << ") started with message: " << message << std::endl;
    std::this_thread::sleep_for(std::chrono::milliseconds(500 * id));
    std::cout << "Worker thread " << id << " (ID: " << std::this_thread::get_id() << ") finished." << std::endl;
}

class WorkerClass {
public:
    void operator()(int n) { // Function object
        std::cout << "WorkerClass thread (ID: " << std::this_thread::get_id() << ") processing: " << n << std::endl;
        std::this_thread::sleep_for(std::chrono::milliseconds(300));
        std::cout << "WorkerClass thread (ID: " << std::this_thread::get_id() << ") finished processing: " << n << std::endl;
    }
};

int main_thread_basic() {
    std::cout << "Main thread ID: " << std::this_thread::get_id() << std::endl;

    // 1. Tạo luồng từ hàm không có đối số
    std::thread t1(worker_function_no_args);

    // 2. Tạo luồng từ hàm có đối số
    // Các đối số được sao chép/di chuyển vào bộ nhớ trong của luồng mới
    // Nếu truyền tham chiếu, cần dùng std::ref() hoặc std::cref()
    std::string msg = "Hello Thread";
    std::thread t2(worker_function_with_args, 1, msg); // msg được copy
    std::thread t3(worker_function_with_args, 2, std::cref(msg)); // msg được truyền bằng const reference

    // 3. Tạo luồng từ lambda
    std::thread t4([](int x) {
        std::cout << "Lambda thread (ID: " << std::this_thread::get_id() << ") processing: " << x << std::endl;
        std::this_thread::sleep_for(std::chrono::milliseconds(600));
        std::cout << "Lambda thread (ID: " << std::this_thread::get_id() << ") finished." << std::endl;
    }, 100);

    // 4. Tạo luồng từ function object
    WorkerClass wc_obj;
    std::thread t5(wc_obj, 200); // Copy của wc_obj sẽ được dùng
    // std::thread t5(std::ref(wc_obj), 200); // Nếu muốn dùng chính wc_obj (cẩn thận vòng đời)


    std::cout << "Main thread: All threads launched." << std::endl;

    // Chờ các luồng hoàn thành
    // Nếu không join hoặc detach, std::terminate sẽ được gọi khi t1, t2,... bị hủy
    if (t1.joinable()) t1.join();
    if (t2.joinable()) t2.join();
    if (t3.joinable()) t3.join();
    if (t4.joinable()) t4.join();
    if (t5.joinable()) t5.join();

    std::cout << "Main thread: All threads finished." << std::endl;

    // Ví dụ với detach (ít dùng hơn)
    // std::thread t_detached([](){
    //     std::this_thread::sleep_for(std::chrono::seconds(2));
    //     std::cout << "Detached thread finished execution." << std::endl;
    // });
    // t_detached.detach(); // Luồng sẽ chạy ngầm, main có thể kết thúc trước
    // std::cout << "Main thread continues after detaching a thread." << std::endl;
    // Cần cẩn thận với tài nguyên mà detached thread sử dụng, đảm bảo chúng còn tồn tại.

    return 0;
}
```

**2. Bảo Vệ Dữ Liệu Chia Sẻ (Protecting Shared Data)**

Khi nhiều luồng cùng truy cập và sửa đổi dữ liệu chia sẻ, có thể xảy ra **data race** (tranh chấp dữ liệu), dẫn đến hành vi không xác định. Cần sử dụng các cơ chế đồng bộ hóa để bảo vệ dữ liệu.

- **`std::mutex` (Mutual Exclusion):**

  - Là một đối tượng khóa cơ bản. Một luồng có thể `lock()` mutex để có quyền truy cập độc quyền vào một đoạn mã (critical section). Các luồng khác cố gắng `lock()` cùng mutex đó sẽ bị block cho đến khi luồng đầu tiên `unlock()` nó.
  - **Operations:** `lock()`, `unlock()`, `try_lock()` (thử khóa, trả về ngay lập tức).
  - **QUAN TRỌNG:** Luôn đảm bảo `unlock()` mutex, ngay cả khi có exception. Đây là lý do `std::lock_guard` và `std::unique_lock` được ưa dùng.

- **`std::lock_guard<Mutex>` (RAII wrapper cho Mutex):**

  - Một lớp template RAII. Constructor của nó gọi `mutex.lock()`, destructor gọi `mutex.unlock()`.
  - Đảm bảo mutex luôn được giải phóng khi `lock_guard` ra khỏi scope (an toàn với exception).
  - Không thể `lock()` hay `unlock()` thủ công.

- **`std::unique_lock<Mutex>` (RAII wrapper linh hoạt hơn):**
  - Tương tự `lock_guard` nhưng linh hoạt hơn.
  - Cho phép `lock()`, `unlock()` thủ công.
  - Hỗ trợ `try_lock()`, `try_lock_for()`, `try_lock_until()`.
  - Có thể chuyển quyền sở hữu khóa (movable).
  - Quan trọng khi dùng với `std::condition_variable`.

**Ví dụ sử dụng Mutex và Lock Guard:**

```cpp
#include <iostream>
#include <thread>
#include <mutex>   // Cho std::mutex, std::lock_guard
#include <vector>
#include <numeric> // Cho std::accumulate

long long shared_counter = 0;
std::mutex counter_mutex; // Mutex để bảo vệ shared_counter

void increment_counter(int iterations) {
    for (int i = 0; i < iterations; ++i) {
        // Cách 1: Dùng lock() và unlock() trực tiếp (dễ quên unlock)
        // counter_mutex.lock();
        // shared_counter++;
        // counter_mutex.unlock();

        // Cách 2: Dùng std::lock_guard (ưu tiên)
        std::lock_guard<std::mutex> lock(counter_mutex); // Lock mutex, unlock tự động khi ra khỏi scope
        shared_counter++;
        // Nếu có exception ở đây, mutex vẫn được unlock
    }
}

int main_mutex() {
    const int num_threads = 10;
    const int iterations_per_thread = 100000;
    std::vector<std::thread> threads;

    std::cout << "Initial shared_counter: " << shared_counter << std::endl;

    for (int i = 0; i < num_threads; ++i) {
        threads.emplace_back(increment_counter, iterations_per_thread);
        // emplace_back xây dựng thread trực tiếp trong vector
    }

    for (auto& t : threads) {
        if (t.joinable()) {
            t.join();
        }
    }

    std::cout << "Final shared_counter: " << shared_counter << std::endl;
    std::cout << "Expected value: " << num_threads * iterations_per_thread << std::endl;
    // Nếu không có mutex, final shared_counter sẽ thường nhỏ hơn expected do data race.

    return 0;
}
```

**Các loại Mutex khác:**

- `std::recursive_mutex`: Cho phép cùng một luồng `lock()` nhiều lần (cần `unlock()` tương ứng số lần `lock()`). Hữu ích trong các hàm đệ quy cần khóa.
- `std::timed_mutex` và `std::recursive_timed_mutex`: Hỗ trợ `try_lock_for()` và `try_lock_until()` (thử khóa trong một khoảng thời gian hoặc cho đến một thời điểm).
- `std::shared_mutex` (C++17, header `<shared_mutex>`): Cho phép nhiều luồng đọc đồng thời (shared lock) hoặc một luồng ghi độc quyền (exclusive lock). Còn gọi là read-write lock. Dùng `std::shared_lock` cho shared access và `std::unique_lock` (hoặc `std::lock_guard`) cho exclusive access.

**3. Deadlocks (Khóa Chết)**

- Xảy ra khi hai hoặc nhiều luồng bị block vĩnh viễn, mỗi luồng chờ một tài nguyên đang bị giữ bởi luồng khác trong nhóm đó.
- Ví dụ: Luồng A khóa mutex M1 rồi cố khóa M2. Luồng B khóa M2 rồi cố khóa M1.
- **Cách tránh Deadlock:**

  - **Thứ tự khóa (Lock Ordering):** Luôn khóa nhiều mutex theo một thứ tự nhất quán toàn cục.
  - **`std::lock(mutex1, mutex2, ...)`:** Một hàm tiện ích để khóa nhiều mutex cùng lúc một cách an toàn (tránh deadlock bằng cách sử dụng thuật toán try-lock-then-lock).
  - **`std::scoped_lock<MutexTypes...>` (C++17):** Một RAII wrapper cho nhiều mutex, sử dụng `std::lock` bên trong.

  ```cpp
  std::mutex mtx1, mtx2;
  void process_deadlock_prone() {
      // std::lock_guard<std::mutex> lock1(mtx1);
      // std::this_thread::sleep_for(std::chrono::milliseconds(10)); // Tăng khả năng deadlock
      // std::lock_guard<std::mutex> lock2(mtx2);
      // ...
  } // Giả sử có luồng khác khóa mtx2 rồi mtx1

  void process_deadlock_safe() {
      // Cách 1: std::lock và unique_lock với std::adopt_lock
      std::unique_lock<std::mutex> lock1(mtx1, std::defer_lock); // Không khóa ngay
      std::unique_lock<std::mutex> lock2(mtx2, std::defer_lock);
      std::lock(lock1, lock2); // Khóa cả hai một cách an toàn
      // ... làm việc với tài nguyên được bảo vệ bởi mtx1, mtx2 ...
      // unique_lock sẽ tự unlock khi ra khỏi scope
  }

  void process_deadlock_safe_cpp17() {
      std::scoped_lock locks(mtx1, mtx2); // C++17, tự động khóa và giải phóng
      // ... làm việc với tài nguyên ...
  }
  ```

**4. Biến Điều Kiện (`std::condition_variable`)**

- Cho phép một hoặc nhiều luồng chờ (block) cho đến khi một điều kiện nào đó được thỏa mãn và một luồng khác thông báo (notify) về sự thay đổi đó.
- Luôn được sử dụng cùng với một `std::mutex` để bảo vệ điều kiện và dữ liệu liên quan.
- **Operations:**
  - `wait(std::unique_lock<std::mutex>& lock, Predicate pred)`: Giải phóng `lock` và block luồng hiện tại cho đến khi `pred` trả về `true` và được notify. Khi được đánh thức, nó sẽ tự động khóa lại `lock` trước khi `pred` được kiểm tra lại (để tránh spurious wakeups - đánh thức giả).
  - `wait(std::unique_lock<std::mutex>& lock)`: Tương tự nhưng không có predicate, cần vòng lặp `while` để kiểm tra điều kiện.
  - `notify_one()`: Đánh thức một luồng đang chờ.
  - `notify_all()`: Đánh thức tất cả các luồng đang chờ.

**Ví dụ Producer-Consumer với Condition Variable:**

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <queue>
#include <chrono>

std::queue<int> data_queue;
std::mutex queue_mutex;
std::condition_variable data_cond;
const int MAX_QUEUE_SIZE = 5;
bool生产_finished = false; // Biến đánh dấu kết thúc sản xuất

void producer(int num_items) {
    for (int i = 0; i < num_items; ++i) {
        std::this_thread::sleep_for(std::chrono::milliseconds(100 * (i % 5 + 1))); // Giả lập thời gian sản xuất
        {
            std::unique_lock<std::mutex> lock(queue_mutex);
            // Chờ nếu queue đầy (dùng lambda làm predicate cho wait)
            data_cond.wait(lock, []{ return data_queue.size() < MAX_QUEUE_SIZE; });

            int item = i * 10;
            data_queue.push(item);
            std::cout << "Producer (ID: " << std::this_thread::get_id() << ") produced: " << item
                      << ", Queue size: " << data_queue.size() << std::endl;
        } // lock được giải phóng ở đây
        data_cond.notify_one(); // Thông báo cho một consumer
    }
    // Đánh dấu kết thúc sản xuất và thông báo cho tất cả consumer
    {
        std::unique_lock<std::mutex> lock(queue_mutex);
        生产_finished = true;
    }
    data_cond.notify_all(); // Đảm bảo tất cả consumer đang chờ đều được đánh thức
    std::cout << "Producer finished." << std::endl;
}

void consumer(int id) {
    while (true) {
        int item;
        {
            std::unique_lock<std::mutex> lock(queue_mutex);
            // Chờ nếu queue rỗng VÀ sản xuất chưa kết thúc
            data_cond.wait(lock, []{ return !data_queue.empty() || 生产_finished; });

            if (data_queue.empty() && 生产_finished) {
                // Queue rỗng và producer đã xong, consumer có thể thoát
                break;
            }

            if (!data_queue.empty()) { // Kiểm tra lại lần nữa sau khi được đánh thức
                item = data_queue.front();
                data_queue.pop();
                std::cout << "Consumer " << id << " (ID: " << std::this_thread::get_id() << ") consumed: " << item
                          << ", Queue size: " << data_queue.size() << std::endl;
            }
        } // lock được giải phóng
        data_cond.notify_one(); // Thông báo cho producer (nếu đang chờ queue bớt đầy)
        std::this_thread::sleep_for(std::chrono::milliseconds(200 * (id % 3 + 1))); // Giả lập thời gian tiêu thụ
    }
    std::cout << "Consumer " << id << " finished." << std::endl;
}


int main_cond_var() {
    std::thread prod_thread(producer, 20); // Sản xuất 20 item

    std::vector<std::thread> consumer_threads;
    for (int i = 0; i < 3; ++i) { // 3 consumer
        consumer_threads.emplace_back(consumer, i + 1);
    }

    if (prod_thread.joinable()) {
        prod_thread.join();
    }
    for (auto& ct : consumer_threads) {
        if (ct.joinable()) {
            ct.join();
        }
    }

    std::cout << "All threads finished." << std::endl;
    return 0;
}
```

**5. Lập Trình Bất Đồng Bộ (`std::async`, `std::future`, `std::promise`)**

- Cho phép thực thi các tác vụ một cách bất đồng bộ (có thể trên một luồng khác) và lấy kết quả của tác vụ đó sau này.
- **`std::async(LaunchPolicy policy, Function&& f, Args&&... args)`:**
  - Chạy hàm `f` với các đối số `args` một cách bất đồng bộ.
  - `policy`:
    - `std::launch::async`: Đảm bảo `f` chạy trên một luồng mới.
    - `std::launch::deferred`: `f` sẽ không chạy cho đến khi `future::get()` hoặc `future::wait()` được gọi (chạy trên luồng gọi `get`/`wait`).
    - Mặc định (`std::launch::async | std::launch::deferred`): Tùy trình biên dịch quyết định.
  - Trả về một `std::future<ResultType>` để lấy kết quả.
- **`std::future<T>`:**
  - Đại diện cho kết quả của một tác vụ bất đồng bộ (sẽ có trong tương lai).
  - `get()`: Chờ cho đến khi tác vụ hoàn thành và trả về kết quả (hoặc ném exception nếu tác vụ ném). Chỉ có thể gọi `get()` một lần.
  - `wait()`: Chờ tác vụ hoàn thành nhưng không lấy kết quả.
  - `wait_for()`, `wait_until()`: Chờ trong một khoảng thời gian hoặc đến một thời điểm.
  - `valid()`: Kiểm tra `future` có hợp lệ không (ví dụ, chưa gọi `get()`).
- **`std::promise<T>`:**
  - Một cách để một luồng cung cấp kết quả (hoặc exception) cho một luồng khác đang chờ trên `std::future` tương ứng.
  - `get_future()`: Lấy `std::future` liên kết với `promise`.
  - `set_value(value)`: Đặt giá trị kết quả.
  - `set_exception(exception_ptr)`: Đặt exception.

**Ví dụ `std::async` và `std::future`:**

```cpp
#include <iostream>
#include <thread>
#include <future>  // Cho std::async, std::future, std::promise
#include <chrono>
#include <stdexcept>

long long calculate_sum(int start, int end) {
    std::cout << "Thread (ID: " << std::this_thread::get_id() << ") calculating sum from " << start << " to " << end << std::endl;
    long long sum = 0;
    for (int i = start; i <= end; ++i) {
        sum += i;
        if (i == start + 500 && start == 1) { // Giả lập exception
            // throw std::runtime_error("Simulated error in calculate_sum");
        }
        std::this_thread::sleep_for(std::chrono::microseconds(1)); // Giả lập tính toán
    }
    std::cout << "Thread (ID: " << std::this_thread::get_id() << ") finished sum: " << sum << std::endl;
    return sum;
}

void set_promise_value(std::promise<std::string> prms) {
    try {
        std::this_thread::sleep_for(std::chrono::seconds(2));
        // Giả sử tính toán thành công
        prms.set_value("Data from promise thread!");
        // Hoặc nếu có lỗi:
        // throw std::runtime_error("Error in promise task");
    } catch (...) {
        prms.set_exception(std::current_exception()); // Truyền exception qua future
    }
}

int main_async_future() {
    std::cout << "Main thread ID: " << std::this_thread::get_id() << std::endl;

    // 1. Sử dụng std::async
    std::cout << "Launching calculate_sum with std::async..." << std::endl;
    std::future<long long> future_sum1 = std::async(std::launch::async, calculate_sum, 1, 10000);
    std::future<long long> future_sum2 = std::async(std::launch::async, calculate_sum, 10001, 20000);

    // Làm việc khác trong main thread
    std::cout << "Main thread doing other work..." << std::endl;
    std::this_thread::sleep_for(std::chrono::seconds(1));

    try {
        long long sum1 = future_sum1.get(); // Block cho đến khi kết quả sẵn sàng, hoặc ném exception
        std::cout << "Sum1 from future: " << sum1 << std::endl;

        long long sum2 = future_sum2.get();
        std::cout << "Sum2 from future: " << sum2 << std::endl;

        std::cout << "Total sum: " << sum1 + sum2 << std::endl;
    } catch (const std::exception& e) {
        std::cerr << "Exception caught from async task: " << e.what() << std::endl;
    }


    // 2. Sử dụng std::promise và std::future
    std::cout << "\n--- std::promise example ---" << std::endl;
    std::promise<std::string> data_promise;
    std::future<std::string> data_future = data_promise.get_future();

    std::thread promise_thread(set_promise_value, std::move(data_promise)); // Chuyển promise vào luồng

    std::cout << "Main thread waiting for data from promise..." << std::endl;
    try {
        std::string result_from_promise = data_future.get(); // Block và chờ
        std::cout << "Result from promise: " << result_from_promise << std::endl;
    } catch (const std::exception& e) {
        std::cerr << "Exception from promise: " << e.what() << std::endl;
    }

    if (promise_thread.joinable()) {
        promise_thread.join();
    }

    std::cout << "All async/promise tasks finished." << std::endl;
    return 0;
}
```

**6. Kiểu Nguyên Tử (`std::atomic<T>`)**

- Header: `<atomic>`
- Cung cấp các kiểu dữ liệu (ví dụ `std::atomic<bool>`, `std::atomic<int>`, `std::atomic<MyStruct*`) mà các thao tác trên chúng (đọc, ghi, tăng, giảm, so sánh và hoán đổi) được thực hiện một cách **nguyên tử (atomic)**.
- Nguyên tử có nghĩa là thao tác đó được thực hiện hoàn toàn và không thể bị ngắt quãng bởi các luồng khác. Điều này giúp tránh data race mà không cần dùng mutex cho các thao tác đơn giản.
- Hỗ trợ các memory orderings khác nhau để kiểm soát việc sắp xếp lại lệnh của trình biên dịch và CPU, quan trọng cho lập trình không khóa (lock-free programming) nâng cao. Mặc định là `std::memory_order_seq_cst` (tuần tự nhất quán - an toàn nhất nhưng có thể chậm nhất).
- Ví dụ:

  ```cpp
  #include <iostream>
  #include <thread>
  #include <vector>
  #include <atomic> // Cho std::atomic

  std::atomic<int> atomic_counter(0); // Khởi tạo atomic int là 0
  // std::atomic_flag atomic_flag_lock = ATOMIC_FLAG_INIT; // Một cờ nguyên tử cơ bản

  void atomic_increment(int iterations) {
      for (int i = 0; i < iterations; ++i) {
          atomic_counter++; // Thao tác tăng nguyên tử (fetch-add)
          // Hoặc atomic_counter.fetch_add(1, std::memory_order_relaxed);
      }
  }

  int main_atomic() {
      const int num_threads = 10;
      const int iterations_per_thread = 100000;
      std::vector<std::thread> threads;

      std::cout << "Initial atomic_counter: " << atomic_counter.load() << std::endl; // Đọc nguyên tử

      for (int i = 0; i < num_threads; ++i) {
          threads.emplace_back(atomic_increment, iterations_per_thread);
      }

      for (auto& t : threads) {
          if (t.joinable()) t.join();
      }

      std::cout << "Final atomic_counter: " << atomic_counter.load() << std::endl;
      std::cout << "Expected value: " << num_threads * iterations_per_thread << std::endl;
      // Kết quả sẽ đúng mà không cần mutex cho thao tác counter đơn giản này.
      return 0;
  }
  ```

**7. Best Practices & Lưu Ý Chung (Phần 15)**

- **Luôn `join()` hoặc `detach()` các luồng `std::thread`** trước khi đối tượng `thread` bị hủy. Ưu tiên `join()` nếu bạn cần chờ kết quả hoặc đảm bảo luồng hoàn thành.
- **Bảo vệ tất cả dữ liệu chia sẻ** bằng mutex hoặc các cơ chế đồng bộ hóa khác để tránh data race.
- **Sử dụng RAII (ví dụ `std::lock_guard`, `std::unique_lock`)** để quản lý mutex, đảm bảo chúng luôn được giải phóng.
- **Cẩn thận với deadlock.** Sử dụng thứ tự khóa nhất quán hoặc `std::lock`/`std::scoped_lock`.
- **Sử dụng `std::condition_variable` để chờ các điều kiện,** tránh các vòng lặp bận (busy-waiting) tốn CPU. Luôn kiểm tra lại điều kiện trong một vòng lặp sau khi `wait()` trả về (spurious wakeups).
- **`std::async` là một cách tiện lợi để chạy tác vụ bất đồng bộ và lấy kết quả.** Cân nhắc launch policy.
- **`std::atomic` hữu ích cho các thao tác đơn giản trên dữ liệu chia sẻ** mà không cần khóa nặng nề, nhưng cần hiểu rõ về memory ordering cho các kịch bản phức tạp.
- **Truyền đối số cho `std::thread`:** Mặc định là copy/move. Nếu cần truyền tham chiếu, dùng `std::ref()` hoặc `std::cref()`. Cẩn thận với vòng đời của các đối tượng được tham chiếu.
- **Xử lý ngoại lệ trong luồng:** Nếu một luồng ném ngoại lệ mà không được bắt bên trong luồng đó, chương trình sẽ gọi `std::terminate()`. Nếu dùng `std::async`, exception có thể được truyền qua `std::future`.
- **Thiết kế cẩn thận:** Lập trình đa luồng phức tạp. Hãy chia nhỏ vấn đề, xác định rõ các phần dữ liệu chia sẻ và các điểm cần đồng bộ hóa.

Lập trình đa luồng mở ra nhiều khả năng tối ưu hóa và cải thiện trải nghiệm người dùng, nhưng đòi hỏi sự cẩn trọng và hiểu biết sâu sắc về các cơ chế đồng bộ hóa để tránh các lỗi khó gỡ. Thư viện chuẩn C++ cung cấp một nền tảng vững chắc cho việc này.

Hết Phần 15. Chúc bạn thành công trên con đường chinh phục C++!
