**Phần 1: Giới thiệu TypeScript - Tại sao và Những khái niệm Cốt lõi**

---

Chào mừng bạn đến với TypeScript!

**1. TypeScript là gì và tại sao lại cần nó?**

- **TypeScript (TS) là một ngôn ngữ lập trình mã nguồn mở được phát triển bởi Microsoft.** Nó là một **superset** (tập hợp cha) của JavaScript (JS), có nghĩa là bất kỳ mã JavaScript hợp lệ nào cũng là mã TypeScript hợp lệ.
- **Mục đích chính:** Thêm vào **hệ thống kiểu tĩnh tùy chọn (optional static typing)** và các tính năng lập trình hướng đối tượng dựa trên lớp (class-based object-oriented programming) cho JavaScript.
- **Trình biên dịch (Compiler):** Mã TypeScript không chạy trực tiếp trên trình duyệt hay Node.js. Nó cần được **biên dịch (compile)** thành mã JavaScript thuần túy bằng trình biên dịch `tsc` (TypeScript Compiler).

**Tác dụng của TypeScript trong các tình huống cụ thể (Real-world usage):**

- **Phát hiện lỗi sớm (Early Error Detection):**

  - **JS:** `let x = 5; x = "hello";` (Hoàn toàn hợp lệ, nhưng có thể gây lỗi logic ở runtime).
  - **TS:** `let x: number = 5; x = "hello";` (Trình biên dịch sẽ báo lỗi ngay: `Type 'string' is not assignable to type 'number'.`)
  - **Lợi ích:** Giảm thiểu lỗi runtime, tiết kiệm thời gian debug. Đặc biệt quan trọng trong các dự án lớn, phức tạp.

- **Cải thiện khả năng đọc hiểu và bảo trì code (Improved Readability & Maintainability):**

  - Khi đọc code TS, bạn biết rõ kiểu dữ liệu của biến, tham số hàm, giá trị trả về. Điều này giúp hiểu logic nhanh hơn.
  - Ví dụ: `function calculateTotal(price: number, quantity: number): number { ... }` rõ ràng hơn `function calculateTotal(price, quantity) { ... }`.
  - **Lợi ích:** Dễ dàng cho người mới tham gia dự án, dễ refactor code mà không sợ phá vỡ những thứ không liên quan.

- **Hỗ trợ tái cấu trúc (Safer Refactoring):**

  - Khi bạn đổi tên một thuộc tính trong một interface hoặc class, trình biên dịch TS sẽ báo lỗi ở tất cả những nơi sử dụng thuộc tính đó với tên cũ.
  - **Lợi ích:** Tự tin hơn khi thay đổi cấu trúc code.

- **Tăng cường trải nghiệm phát triển (Enhanced Developer Experience - DX):**

  - Các IDE (như VS Code) cung cấp khả năng tự động hoàn thành (autocompletion), gợi ý kiểu, và kiểm tra lỗi tức thì rất mạnh mẽ nhờ vào thông tin kiểu.
  - **Lợi ích:** Code nhanh hơn, ít lỗi hơn, và có cảm giác "an toàn" hơn.

- **Hợp tác nhóm hiệu quả hơn (Better Team Collaboration):**
  - Types hoạt động như một "hợp đồng" (contract) giữa các phần của code hoặc giữa các thành viên trong nhóm. Mọi người đều hiểu rõ cấu trúc dữ liệu được mong đợi.
  - **Lợi ích:** Giảm hiểu lầm, tăng năng suất khi làm việc nhóm.

**Các chuyên gia dùng TypeScript như thế nào?**

- **Trong các dự án lớn và phức tạp:** Angular (viết bằng TS), NestJS (framework backend Node.js viết bằng TS), Vue 3 (viết bằng TS), React (có thể dùng với TS và rất phổ biến).
- **Để xây dựng thư viện và framework:** Thông tin kiểu giúp người dùng thư viện dễ dàng hiểu API và sử dụng đúng cách.
- **Tích hợp với quy trình CI/CD:** Bước kiểm tra kiểu (type checking) được thêm vào pipeline để đảm bảo code không có lỗi kiểu trước khi deploy.
- **Tận dụng `strict` mode:** Các chuyên gia thường bật các cờ `strict` trong `tsconfig.json` (sẽ nói ở phần sau) để TS kiểm tra chặt chẽ nhất có thể.

**2. Các kiểu dữ liệu cơ bản (Basic Types) trong TypeScript**

Vì bạn đã biết JS, nhiều kiểu sẽ quen thuộc, TS chỉ thêm "chú thích kiểu" (type annotation).

- **`boolean`**: `let isDone: boolean = false;`
- **`number`**: `let decimal: number = 6; let hex: number = 0xf00d;` (bao gồm cả số nguyên và số thực)
- **`string`**: `let color: string = "blue"; color = 'red'; let fullName: string = \`Bob Bobbington\`;`
- **`array`**:
  - `let list: number[] = [1, 2, 3];`
  - `let listGeneric: Array<number> = [1, 2, 3];` (cú pháp generic, sẽ tìm hiểu sau)
- **`tuple`**: Mảng với số lượng phần tử cố định và kiểu dữ liệu cố định cho từng phần tử.
  - `let x: [string, number]; x = ["hello", 10]; // OK`
  - `// x = [10, "hello"]; // Error`
- **`enum`**: Một cách để đặt tên thân thiện cho một tập hợp các giá trị số.
  - `enum Color {Red, Green, Blue}`
  - `let c: Color = Color.Green; // c sẽ là 1 (mặc định bắt đầu từ 0)`
  - `enum ColorNamed {Red = 1, Green = 2, Blue = 4}`
  - `let colorName: string = ColorNamed[2]; // 'Green'`
- **`any`**: Dùng khi bạn không biết kiểu dữ liệu hoặc muốn bỏ qua kiểm tra kiểu. **Hạn chế sử dụng `any`** vì nó làm mất đi lợi ích của TS.
  - `let notSure: any = 4; notSure = "maybe a string instead"; notSure = false;`
- **`unknown`**: Tương tự `any` nhưng an toàn hơn. Bạn không thể làm gì với giá trị `unknown` cho đến khi bạn thực hiện kiểm tra kiểu (type narrowing) hoặc ép kiểu (type assertion).
  - `let unsure: unknown = 4;`
  - `// let num: number = unsure; // Error! Type 'unknown' is not assignable to type 'number'.`
  - `if (typeof unsure === 'number') { let num: number = unsure; /* OK */ }`
- **`void`**: Thường dùng cho kiểu trả về của hàm không trả về giá trị gì.
  - `function warnUser(): void { console.log("This is my warning message"); }`
- **`null` và `undefined`**:
  - Mặc định, `null` và `undefined` là kiểu con của tất cả các kiểu khác. Nghĩa là bạn có thể gán `null` hoặc `undefined` cho một biến `number`.
  - Tuy nhiên, khi bật cờ `strictNullChecks` (khuyến khích!), `null` và `undefined` chỉ có thể được gán cho `any`, `unknown` hoặc kiểu của chính chúng (hoặc cho kiểu `union` bao gồm chúng, sẽ nói sau).
  - `let u: undefined = undefined;`
  - `let n: null = null;`
- **`never`**: Đại diện cho kiểu của các giá trị không bao giờ xảy ra. Ví dụ, một hàm luôn throw lỗi hoặc một vòng lặp vô tận.
  - `function error(message: string): never { throw new Error(message); }`
- **`object`**: Đại diện cho kiểu không phải là primitive (number, string, boolean, symbol, null, or undefined).
  - `declare function create(o: object | null): void;`
  - `create({ prop: 0 }); // OK`
  - `create(null); // OK`
  - `// create(42); // Error`

**3. Chú thích kiểu (Type Annotations) và Suy luận kiểu (Type Inference)**

- **Chú thích kiểu (Type Annotations):**

  - Là cách bạn nói cho TS biết kiểu dữ liệu của một biến, tham số hàm, hoặc giá trị trả về.
  - Ví dụ: `let apples: number = 5;` `function greet(name: string): string { return "Hello " + name; }`

- **Suy luận kiểu (Type Inference):**
  - Nếu bạn không cung cấp chú thích kiểu, TS sẽ cố gắng suy luận kiểu dựa trên giá trị được gán.
  - Ví dụ: `let oranges = 5; // TS suy luận oranges là kiểu number`
  - `let greeting = "Hello"; // TS suy luận greeting là kiểu string`
  - **Các chuyên gia thường:** Ưu tiên suy luận kiểu khi nó rõ ràng và chỉ chú thích kiểu khi cần thiết (ví dụ: tham số hàm, biến chưa được khởi tạo, hoặc khi muốn ép kiểu cụ thể hơn).

**4. Cài đặt và sử dụng trình biên dịch TypeScript (`tsc`)**

- **Cài đặt:** `npm install -g typescript` (cài đặt toàn cục)
- **Biên dịch file:**
  1.  Tạo file `app.ts` (ví dụ: `let message: string = "Hello TypeScript"; console.log(message);`)
  2.  Chạy lệnh: `tsc app.ts`
  3.  Sẽ tạo ra file `app.js` chứa mã JavaScript tương ứng.
- **File cấu hình `tsconfig.json`:**
  - Khi làm việc với dự án, bạn sẽ dùng `tsconfig.json` để cấu hình các tùy chọn biên dịch.
  - Tạo file: `tsc --init`
  - Các tùy chọn quan trọng sẽ được đề cập ở các phần sau (ví dụ: `target`, `module`, `strict`, `outDir`, `rootDir`).

---

**Tạm kết Phần 1:**

Bạn đã hiểu được TypeScript là gì, tại sao nó hữu ích, đặc biệt là trong các dự án thực tế với các lợi ích như phát hiện lỗi sớm, cải thiện khả năng bảo trì và làm việc nhóm. Bạn cũng đã làm quen với các kiểu dữ liệu cơ bản và cách TS xử lý kiểu thông qua chú thích và suy luận.

Tuyệt vời! Chúng ta sẽ tiếp tục với phần quan trọng không kém để bạn có thể "định hình" dữ liệu trong TypeScript.

**Phần 2: Interfaces và Type Aliases - Định hình Dữ liệu và Hợp đồng**

---

Trong Phần 1, bạn đã làm quen với các kiểu dữ liệu cơ bản. Bây giờ, chúng ta sẽ khám phá cách TypeScript cho phép bạn định nghĩa các cấu trúc dữ liệu phức tạp hơn, tạo ra các "hợp đồng" (contracts) mà code của bạn phải tuân theo. Đây là lúc `Interfaces` và `Type Aliases` tỏa sáng.

**1. Interfaces - Định nghĩa "Hình dạng" của Đối tượng**

`Interface` là một cách mạnh mẽ để định nghĩa một "hợp đồng" cho hình dạng (shape) của một đối tượng. Nó xác định các thuộc tính và phương thức mà một đối tượng _phải_ có.

- **Khai báo cơ bản:**

  ```typescript
  interface UserProfile {
    id: number;
    username: string;
    email: string;
    isActive: boolean;
    bio?: string; // Thuộc tính tùy chọn (optional)
    readonly registrationDate: Date; // Thuộc tính chỉ đọc (readonly)
  }

  function displayUserProfile(user: UserProfile) {
    console.log(`Username: ${user.username}`);
    if (user.bio) {
      console.log(`Bio: ${user.bio}`);
    }
    // user.registrationDate = new Date(); // Lỗi! Cannot assign to 'registrationDate' because it is a read-only property.
  }

  let myUser: UserProfile = {
    id: 1,
    username: "coderPro",
    email: "coder@example.com",
    isActive: true,
    registrationDate: new Date(),
  };

  displayUserProfile(myUser);
  // displayUserProfile({ id: 2, username: "another" }); // Lỗi! Property 'email' is missing...
  ```

- **Tác dụng và Real-world usage:**

  - **Định nghĩa cấu trúc dữ liệu cho API responses/requests:** Khi làm việc với backend, bạn biết rõ dữ liệu trả về sẽ có dạng như thế nào.

    ```typescript
    interface Product {
      id: string;
      name: string;
      price: number;
      description?: string;
    }

    async function fetchProduct(productId: string): Promise<Product> {
      const response = await fetch(`/api/products/${productId}`);
      return await response.json();
    }
    ```

  - **Định nghĩa props cho components (ví dụ trong React):**

    ```typescript
    // Trong một file .tsx (React với TypeScript)
    interface GreetingProps {
      name: string;
      messageCount?: number;
    }

    function Greeting(props: GreetingProps) {
      return (
        <h1>
          Hello, {props.name}! You have {props.messageCount || 0} new messages.
        </h1>
      );
    }
    ```

  - **Đảm bảo tính nhất quán của đối tượng:** Khi nhiều phần của ứng dụng cần làm việc với cùng một loại đối tượng, interface đảm bảo chúng đều tuân theo một cấu trúc chung.

- **Thuộc tính tùy chọn (Optional Properties `?`):**

  - Không phải tất cả các thuộc tính của một interface đều bắt buộc. Bạn có thể đánh dấu chúng là tùy chọn bằng dấu `?` sau tên thuộc tính.
  - Ví dụ: `bio?: string;` trong `UserProfile`.

- **Thuộc tính chỉ đọc (Readonly Properties `readonly`):**

  - Bạn có thể chỉ định rằng một số thuộc tính của đối tượng không được thay đổi sau khi đối tượng được tạo.
  - Ví dụ: `readonly registrationDate: Date;` trong `UserProfile`.

- **Mở rộng Interfaces (`extends`):**

  - Interfaces có thể kế thừa từ các interfaces khác, giúp tái sử dụng và tạo ra các cấu trúc phức tạp hơn.
  - ```typescript
    interface Animal {
      name: string;
      eat(food: string): void;
    }
    ```

  interface Dog extends Animal {
  breed: string;
  bark(): void;
  }

  let myDog: Dog = {
  name: "Buddy",
  breed: "Golden Retriever",
  eat: (food) => console.log(`Eating ${food}`),
  bark: () => console.log("Woof woof!")
  };

  ```

  ```

- **Interfaces cho Hàm (Function Types):**

  - Interfaces có thể mô tả "hình dạng" của một hàm (tham số và kiểu trả về).
  - ```typescript
    interface SearchFunc {
      (source: string, subString: string): boolean;
    }
    ```

  let mySearch: SearchFunc;
  mySearch = function(src, sub) { // Tên tham số không cần giống, nhưng kiểu phải khớp
  let result = src.search(sub);
  return result > -1;
  }
  console.log(mySearch("hello world", "world")); // true

  ```

  ```

- **Interfaces cho Mảng (Indexable Types):**

  - Mô tả các kiểu có thể được "truy cập bằng chỉ mục" (index), như mảng.
  - ```typescript
    interface StringArray {
      [index: number]: string; // Chỉ mục là number, giá trị trả về là string
    }
    ```

  let myArray: StringArray;
  myArray = ["Bob", "Fred"];
  let myStr: string = myArray[0]; // "Bob"

  ````
  *   Bạn cũng có thể có chỉ mục kiểu `string`:
  ```typescript
  interface NumberDictionary {
      [index: string]: number;
      length: number;    // có thể có thuộc tính khác
      // name: string;      // Lỗi: Thuộc tính 'name' kiểu 'string' không thể gán cho kiểu chỉ mục 'number'
  }
  ````

- **Interfaces cho Lớp (Class Types):** Chúng ta sẽ nói kỹ hơn ở Phần 4 về Lớp, nhưng interface có thể định nghĩa "hợp đồng" mà một lớp phải `implements`.

**2. Type Aliases - Tạo Tên Thay Thế cho Kiểu**

`Type Alias` (bí danh kiểu) cho phép bạn tạo một tên mới cho một kiểu đã có. Nó không tạo ra một kiểu mới hoàn toàn mà chỉ là một tên gọi khác. Type Aliases có thể dùng cho các kiểu cơ bản, union, intersection, tuple, và bất kỳ kiểu nào khác bạn có thể viết ra.

- **Khai báo cơ bản:**

  ```typescript
  type Point = {
    // Định nghĩa một kiểu đối tượng
    x: number;
    y: number;
  };

  type ID = string | number; // Định nghĩa một union type

  type StringOrNumberArray = (string | number)[];

  function printCoord(pt: Point) {
    console.log("The coordinate's x value is " + pt.x);
    console.log("The coordinate's y value is " + pt.y);
  }

  printCoord({ x: 100, y: 200 });

  let userId: ID = "user-123";
  userId = 456;
  // userId = false; // Lỗi
  ```

- **Tác dụng và Real-world usage:**

  - **Làm cho code dễ đọc hơn:** Đặc biệt hữu ích với các union type hoặc tuple phức tạp.

    ```typescript
    type UserID = string;
    type ProductID = string;
    type Coordinates = [number, number, number?]; // Tuple với phần tử thứ 3 tùy chọn

    function getUser(id: UserID): UserProfile {
      /* ... */ return {} as UserProfile;
    }
    ```

  - **Tái sử dụng các kiểu phức tạp:** Nếu bạn có một union type hoặc một kiểu đối tượng được sử dụng ở nhiều nơi, type alias giúp bạn không phải lặp lại định nghĩa.
  - **Có thể được dùng với Generics (sẽ học ở Phần 3).**

**3. So sánh Interfaces và Type Aliases**

Cả hai đều có thể dùng để mô tả hình dạng của đối tượng hoặc hàm, nhưng có một vài khác biệt chính:

| Đặc điểm                                    | Interface                                                                  | Type Alias                                                                                                |
| :------------------------------------------ | :------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------- |
| **Mở rộng**                                 | Dùng `extends`                                                             | Dùng `&` (Intersection Types) để kết hợp                                                                  |
| **Khai báo hợp nhất (Declaration Merging)** | **Có.** Nếu bạn khai báo nhiều interface cùng tên, chúng sẽ được hợp nhất. | **Không.** Bạn không thể khai báo nhiều type alias cùng tên.                                              |
| **Sử dụng với primitives/unions/tuples**    | Hạn chế (chủ yếu cho object shapes)                                        | Rất linh hoạt, có thể đặt tên cho bất kỳ kiểu nào.                                                        |
| **`implements` trong class**                | Dùng với `implements`                                                      | Không thể dùng trực tiếp với `implements` (nhưng có thể tạo type alias cho một object type rồi implement) |

- **Declaration Merging (Ví dụ):**

  ```typescript
  interface Box {
    height: number;
    width: number;
  }
  interface Box {
    // Hợp nhất với Box ở trên
    scale: number;
  }
  let box: Box = { height: 5, width: 6, scale: 10 };
  ```

  Điều này không thể làm với `type alias`.

- **Khi nào dùng cái nào?**
  - **Khuyến nghị chung (và của nhiều chuyên gia):**
    - **Sử dụng `interface` khi bạn định nghĩa "hình dạng" của một đối tượng hoặc khi bạn muốn tận dụng declaration merging.** Đây là trường hợp phổ biến nhất khi làm việc với object-oriented programming hoặc định nghĩa cấu trúc dữ liệu.
    - **Sử dụng `type` alias khi bạn muốn đặt tên cho các kiểu union, intersection, tuple, hoặc các kiểu primitive.** Nó cũng hữu ích khi bạn muốn có tên ngắn gọn cho một kiểu phức tạp.
  - Nhiều trường hợp bạn có thể dùng cả hai. Sự lựa chọn thường phụ thuộc vào sở thích cá nhân và quy ước của đội nhóm. Tuy nhiên, hãy nhất quán trong dự án của bạn.

**4. Union Types (`|`) và Literal Types**

- **Union Types (`|`):**

  - Cho phép một biến hoặc tham số có thể là một trong nhiều kiểu.
  - Ví dụ: `type Status = "pending" | "approved" | "rejected";`
  - `let orderStatus: Status = "pending";`
  - `// orderStatus = "shipped"; // Lỗi! Type '"shipped"' is not assignable to type 'Status'.`
  - **Tác dụng:** Hạn chế các giá trị có thể được gán, làm cho API của bạn an toàn và rõ ràng hơn.
  - ```typescript
    function printId(id: number | string) {
      if (typeof id === "string") {
        // TypeScript biết id là string trong khối này
        console.log(id.toUpperCase());
      } else {
        // TypeScript biết id là number
        console.log(id);
      }
    }
    ```

  ```
  (Kỹ thuật kiểm tra kiểu như `typeof id === "string"` được gọi là **type narrowing**, sẽ nói rõ hơn ở phần sau).

  ```

- **Literal Types:**
  - Là các kiểu chỉ đại diện cho một giá trị cụ thể của string, number, hoặc boolean.
  - Ví dụ:
    - `type YesNo = "yes" | "no";` (Ở đây `"yes"` và `"no"` là các literal string types)
    - `type OneToThree = 1 | 2 | 3;`
    - `interface Options { allow: true | false; }`
  - **Tác dụng:** Rất hữu ích khi kết hợp với union types để tạo ra một tập hợp các giá trị cụ thể được phép.

**5. Intersection Types (`&`)**

- Kết hợp nhiều kiểu thành một kiểu duy nhất, mà kiểu mới này sẽ có _tất cả_ các thành viên của các kiểu gốc.
- ```typescript
  interface Colorful {
    color: string;
  }
  interface CircleShape {
    radius: number;
  }

  type ColorfulCircle = Colorful & CircleShape; // Kết hợp

  function draw(shape: ColorfulCircle) {
    console.log(`Color: ${shape.color}`);
    console.log(`Radius: ${shape.radius}`);
  }

  draw({ color: "blue", radius: 42 });
  // draw({ color: "red" }); // Lỗi: Property 'radius' is missing...
  ```

- **Tác dụng:** Dùng để tạo ra các kiểu phức tạp bằng cách gộp các phần nhỏ hơn lại. Thường được dùng với type aliases.

---

**Tạm kết Phần 2:**

Bạn đã nắm vững cách sử dụng `Interfaces` và `Type Aliases` để định nghĩa cấu trúc dữ liệu và hàm, tạo ra code rõ ràng, dễ bảo trì và ít lỗi hơn. Bạn cũng đã hiểu về `Union Types`, `Literal Types` và `Intersection Types` để tạo ra các kiểu linh hoạt và chính xác hơn.

**Trong Phần 3, chúng ta sẽ khám phá một trong những tính năng mạnh mẽ nhất của TypeScript: Generics. Generics cho phép bạn viết code có thể tái sử dụng và hoạt động với nhiều kiểu khác nhau mà vẫn đảm bảo an toàn kiểu.** Chúng ta cũng sẽ tìm hiểu sâu hơn về các kỹ thuật kiểm soát kiểu nâng cao.

OK, chúng ta cùng tiến tới một trong những khái niệm giúp TypeScript trở nên thực sự mạnh mẽ và linh hoạt: Generics.

**Phần 3: Generics và Các Kỹ thuật Kiểm soát Kiểu Nâng cao**

---

Generics là một công cụ thiết yếu trong việc xây dựng các thành phần và hàm có khả năng tái sử dụng cao mà vẫn duy trì được sự an toàn về kiểu. Chúng cho phép bạn viết code hoạt động với nhiều kiểu dữ liệu khác nhau mà không phải "hy sinh" thông tin kiểu.

**1. Generics - Viết Code Tái Sử Dụng và An Toàn Kiểu**

- **Vấn đề cần giải quyết:**

  - Hãy tưởng tượng bạn viết một hàm trả về phần tử đầu tiên của một mảng.
  - Nếu không có generics, bạn có thể viết:
    ```typescript
    function getFirstElementAny(arr: any[]): any {
      // Mất thông tin kiểu
      return arr[0];
    }
    let num = getFirstElementAny([1, 2, 3]); // num là any
    let str = getFirstElementAny(["a", "b"]); // str là any
    ```
    Vấn đề là kiểu trả về là `any`, chúng ta mất đi sự an toàn kiểu.
  - Hoặc bạn phải viết nhiều hàm cho từng kiểu:
    ```typescript
    function getFirstElementNumber(arr: number[]): number {
      return arr[0];
    }
    function getFirstElementString(arr: string[]): string {
      return arr[0];
    }
    ```
    Điều này dẫn đến lặp code.

- **Giải pháp với Generics:**

  - Generics cho phép bạn tạo một "biến kiểu" (type variable), thường được ký hiệu bằng một chữ cái viết hoa (ví dụ: `T`).
  - ```typescript
    function getFirstElement<T>(arr: T[]): T | undefined {
      // T là biến kiểu
      return arr.length > 0 ? arr[0] : undefined;
    }
    ```

  let firstNumber = getFirstElement<number>([10, 20, 30]); // firstNumber là number
  let firstString = getFirstElement<string>(["apple", "banana"]); // firstString là string
  let firstBool = getFirstElement([true, false]); // TypeScript cũng có thể suy luận kiểu T là boolean

  ```
  *   **Cách hoạt động:**
    *   Khi bạn gọi `getFirstElement<number>([...])`, `T` sẽ được thay thế bằng `number`.
    *   Tham số `arr` trở thành `number[]` và kiểu trả về trở thành `number | undefined`.
    *   TypeScript đảm bảo rằng kiểu được truyền vào và kiểu trả về là nhất quán.

  ```

- **Real-world usage của Generics:**

  - **Trong các hàm tiện ích (Utility Functions):** Như ví dụ `getFirstElement` ở trên, hoặc các hàm xử lý mảng, đối tượng.
    ```typescript
    // Hàm map một mảng sang kiểu khác
    function mapArray<Input, Output>(
      arr: Input[],
      mapper: (item: Input) => Output
    ): Output[] {
      return arr.map(mapper);
    }
    const numbers = [1, 2, 3];
    const stringifiedNumbers = mapArray(numbers, (n) => n.toString()); // stringifiedNumbers là string[]
    ```
  - **Trong Lớp (Generic Classes):**

    ```typescript
    class DataStorage<T> {
      private data: T[] = [];

      addItem(item: T): void {
        this.data.push(item);
      }

      getItem(index: number): T | undefined {
        return this.data[index];
      }
    }

    const stringStorage = new DataStorage<string>();
    stringStorage.addItem("Hello");
    // stringStorage.addItem(123); // Lỗi! Argument of type 'number' is not assignable to parameter of type 'string'.

    const numberStorage = new DataStorage<number>();
    numberStorage.addItem(100);
    ```

  - **Trong Interfaces (Generic Interfaces):**

    ```typescript
    interface APIResponse<TData> {
      // TData là kiểu của dữ liệu payload
      success: boolean;
      data: TData;
      error?: string;
    }

    interface User {
      id: number;
      name: string;
    }
    interface Product {
      id: string;
      name: string;
      price: number;
    }

    async function fetchUsers(): Promise<APIResponse<User[]>> {
      // ... logic fetch
      return { success: true, data: [{ id: 1, name: "Alice" }] };
    }

    async function fetchProductDetails(
      productId: string
    ): Promise<APIResponse<Product>> {
      // ... logic fetch
      return {
        success: true,
        data: { id: productId, name: "Laptop", price: 1200 },
      };
    }
    ```

  - **Với React Hooks và Components:** Nhiều hook như `useState`, `useRef` và các thư viện component sử dụng generics để cung cấp kiểu chính xác cho state, props.
    ```typescript
    // const [count, setCount] = useState<number>(0);
    // function MyGenericComponent<TProps extends { id: string }>(props: TProps) { /* ... */ }
    ```

- **Ràng buộc Generics (Generic Constraints):**

  - Đôi khi, bạn muốn giới hạn các kiểu mà `T` có thể là. Ví dụ, bạn muốn một hàm generic chỉ hoạt động với các đối tượng có thuộc tính `length`.
  - Sử dụng `extends` trong khai báo generic:

    ```typescript
    interface Lengthwise {
      length: number;
    }

    function logLength<T extends Lengthwise>(arg: T): void {
      console.log(arg.length); // OK, vì T chắc chắn có thuộc tính length
    }

    logLength("hello"); // string có length
    logLength([1, 2, 3]); // array có length
    logLength({ length: 10, value: 3 }); // object có length
    // logLength(123); // Lỗi! Argument of type 'number' is not assignable to parameter of type 'Lengthwise'.
    ```

  - **Tác dụng:** Cho phép bạn sử dụng các thuộc tính hoặc phương thức cụ thể của kiểu bị ràng buộc bên trong hàm generic một cách an toàn.

- **Sử dụng Type Parameters trong Generic Constraints:**

  - Bạn có thể khai báo một tham số kiểu bị ràng buộc bởi một tham số kiểu khác.
  - ```typescript
    function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
      return obj[key];
    }
    ```

  let x = { a: 1, b: "hello", c: true };
  let aValue = getProperty(x, "a"); // aValue là number
  let bValue = getProperty(x, "b"); // bValue là string
  // let dValue = getProperty(x, "d"); // Lỗi! Argument of type '"d"' is not assignable to parameter of type '"a" | "b" | "c"'.

  ```
  `keyof T` là một *type operator* trả về một union của các key (dưới dạng string literal) của `T`. `T[K]` là một *indexed access type* hay *lookup type*, trả về kiểu của thuộc tính `K` trong `T`.
  ```

**2. Type Narrowing (Thu hẹp kiểu)**

TypeScript không phải lúc nào cũng biết kiểu chính xác của một biến, đặc biệt là với union types. Type narrowing là quá trình TypeScript thu hẹp một kiểu rộng hơn (như `string | number`) thành một kiểu cụ thể hơn (như `string` hoặc `number`) trong một phạm vi code nhất định.

- **Các kỹ thuật Type Narrowing phổ biến:**

  - **`typeof` type guards:**
    ```typescript
    function printValue(value: string | number | boolean) {
      if (typeof value === "string") {
        console.log(value.toUpperCase()); // value là string ở đây
      } else if (typeof value === "number") {
        console.log(value.toFixed(2)); // value là number ở đây
      } else {
        console.log(value ? "True" : "False"); // value là boolean ở đây
      }
    }
    ```
  - **`instanceof` type guards:** Dùng để kiểm tra xem một đối tượng có phải là instance của một class cụ thể không.

    ```typescript
    class Fish {
      swim() {
        console.log("Swimming...");
      }
    }
    class Bird {
      fly() {
        console.log("Flying...");
      }
    }

    function move(pet: Fish | Bird) {
      if (pet instanceof Fish) {
        pet.swim(); // pet là Fish ở đây
      } else {
        pet.fly(); // pet là Bird ở đây
      }
    }
    ```

  - **Equality narrowing (`===`, `!==`, `==`, `!=`):**
    ```typescript
    function example(x: string | number, y: string | boolean) {
      if (x === y) {
        // x và y đều là string ở đây
        x.toUpperCase();
        y.toLowerCase();
      } else {
        console.log(x); // x: string | number
        console.log(y); // y: string | boolean
      }
    }
    ```
  - **Truthiness narrowing (Kiểm tra `null`, `undefined`, `0`, `""`, `false`):**
    ```typescript
    function printName(name?: string | null) {
      if (name) {
        // name không phải là null, undefined, "", 0, false
        console.log(name.toUpperCase()); // name là string ở đây (sau khi loại trừ null/undefined)
      } else {
        console.log("No name provided");
      }
    }
    ```
  - **`in` operator narrowing:** Kiểm tra xem một thuộc tính có tồn tại trong một đối tượng không.

    ```typescript
    interface Admin {
      name: string;
      privileges: string[];
    }
    interface Employee {
      name: string;
      startDate: Date;
    }
    type Staff = Admin | Employee;

    function logStaffDetails(staff: Staff) {
      console.log(`Name: ${staff.name}`);
      if ("privileges" in staff) {
        // staff là Admin ở đây
        console.log(`Privileges: ${staff.privileges.join(", ")}`);
      }
      if ("startDate" in staff) {
        // staff là Employee ở đây
        console.log(`Start Date: ${staff.startDate}`);
      }
    }
    ```

  - **User-Defined Type Guards (Hàm bảo vệ kiểu do người dùng định nghĩa):**

    - Đôi khi, các phương pháp trên không đủ. Bạn có thể tạo hàm riêng để kiểm tra kiểu, hàm này phải trả về một _type predicate_ có dạng `parameterName is Type`.
    - ```typescript
      interface Cat {
        meow(): void;
      }
      interface Dog {
        bark(): void;
      }
      ```

    // Type predicate: pet is Cat
    function isCat(pet: Cat | Dog): pet is Cat {
    return (pet as Cat).meow !== undefined; // Kiểm tra sự tồn tại của phương thức meow
    }

    function makeSound(pet: Cat | Dog) {
    if (isCat(pet)) {
    pet.meow(); // pet là Cat ở đây
    } else {
    (pet as Dog).bark(); // pet là Dog ở đây (cần ép kiểu vì else không tự động suy ra Dog)
    // Hoặc tạo thêm isDog()
    }
    }

    ```

    ```

**3. Ép kiểu (Type Assertions)**

Đôi khi, bạn biết rõ về kiểu của một giá trị hơn là TypeScript có thể suy luận. Trong những trường hợp này, bạn có thể sử dụng ép kiểu để "bảo" TypeScript rằng "tin tôi đi, tôi biết tôi đang làm gì."

- **Cú pháp:**

  - **`as` syntax (khuyến khích dùng trong file .tsx để tránh nhầm lẫn với JSX):**
    `let someValue: unknown = "this is a string";`
    `let strLength: number = (someValue as string).length;`
  - **Angle-bracket syntax (`<>`):**
    `let someValue: unknown = "this is a string";`
    `let strLength: number = (<string>someValue).length;`

- **Lưu ý quan trọng:**

  - Ép kiểu **không** thực hiện bất kỳ chuyển đổi kiểu nào ở runtime. Nó chỉ là một gợi ý cho trình biên dịch.
  - Nếu bạn ép kiểu sai, code của bạn có thể bị lỗi ở runtime mà TypeScript không cảnh báo.
  - **Chỉ sử dụng ép kiểu khi bạn chắc chắn 100% về kiểu.** Lạm dụng ép kiểu sẽ làm mất đi lợi ích của hệ thống kiểu.
  - Ưu tiên sử dụng type narrowing hơn là ép kiểu khi có thể.

- **Real-world usage (cẩn thận):**
  - Khi làm việc với DOM: `const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;` (Bạn biết chắc "main_canvas" là một canvas).
  - Khi dữ liệu từ API không hoàn toàn khớp với interface và bạn cần truy cập một thuộc tính mà TS không biết:
    ```typescript
    // Giả sử API trả về { name: "Product A", extraInfo: { code: "XYZ" } }
    // Và interface của bạn chỉ là interface Product { name: string; }
    // const productData: any = await fetchProduct(); // Lấy dữ liệu
    // const productCode = (productData.extraInfo as { code: string }).code; // Nguy hiểm nếu extraInfo không tồn tại hoặc không có code
    // Cách tốt hơn: định nghĩa interface đầy đủ hơn hoặc dùng type guard.
    ```

**4. Các Type Operators hữu ích khác**

- **`keyof` Type Operator:**

  - Đã thấy ở ví dụ `getProperty`. Trả về một union của các key (dưới dạng string literal hoặc number literal) của một kiểu.
  - ```typescript
    interface Person {
      name: string;
      age: number;
      address: string;
    }
    type PersonKeys = keyof Person; // "name" | "age" | "address"
    ```

  ```

  ```

- **`typeof` Type Operator (trong ngữ cảnh kiểu):**

  - Khác với `typeof` trong JavaScript (trả về string như "string", "number").
  - Trong TypeScript, `typeof` có thể được sử dụng trong một vị trí kiểu để lấy kiểu của một biến hoặc thuộc tính.
  - ```typescript
    let s = "hello";
    let n: typeof s; // n có kiểu string
    ```

  type Predicate = (x: unknown) => boolean;
  type K = ReturnType<Predicate>; // K sẽ là boolean (ReturnType là một Utility Type)

  const person = { name: "Alice", age: 30 };
  type PersonType = typeof person; // PersonType là { name: string, age: number }

  ```

  ```

- **Indexed Access Types (`T[K]` - Lookup Types):**

  - Cho phép bạn lấy kiểu của một thuộc tính trong một kiểu khác bằng cách sử dụng tên thuộc tính (key).
  - ```typescript
    interface Car {
      make: string;
      model: string;
      year: number;
    }
    type MakeType = Car["make"]; // string
    type ModelType = Car["model"]; // string
    type YearType = Car["year"]; // number
    // type WrongType = Car["color"]; // Lỗi: Property 'color' does not exist on type 'Car'.
    ```

  type PropertyTypes = Car[keyof Car]; // string | number (kiểu của tất cả các thuộc tính trong Car)

  ```

  ```

---

**Tạm kết Phần 3:**

Bạn đã được trang bị `Generics`, một công cụ cực kỳ mạnh mẽ để viết code linh hoạt và an toàn. Bạn cũng đã hiểu các kỹ thuật `Type Narrowing` để giúp TypeScript hiểu rõ hơn về code của bạn, và khi nào nên (và không nên) sử dụng `Type Assertions`. Các `Type Operators` như `keyof`, `typeof` (trong ngữ cảnh kiểu), và `Indexed Access Types` cũng giúp bạn làm việc với kiểu một cách tinh vi hơn.

**Trong Phần 4, chúng ta sẽ đào sâu vào Lập trình Hướng đối tượng (OOP) với TypeScript, bao gồm Lớp (Classes), kế thừa, modifiers (public, private, protected), abstract classes, và cách interfaces tương tác với classes.**

Tuyệt vời! Chúng ta sẽ tiếp tục hành trình làm chủ TypeScript với một chủ đề quen thuộc trong lập trình hiện đại: Lập trình Hướng đối tượng.

**Phần 4: Lập trình Hướng đối tượng (OOP) với TypeScript**

---

TypeScript hỗ trợ đầy đủ các khái niệm Lập trình Hướng đối tượng (OOP) mà bạn có thể đã quen thuộc từ các ngôn ngữ như Java hay C#. Nó xây dựng trên nền tảng prototype-based của JavaScript và thêm vào cú pháp class-based rõ ràng cùng với các tính năng như interfaces, modifiers, và abstract classes.

**1. Lớp (Classes)**

Lớp trong TypeScript là một bản thiết kế (blueprint) để tạo ra các đối tượng. Chúng đóng gói dữ liệu (thuộc tính) và các hàm (phương thức) hoạt động trên dữ liệu đó.

- **Khai báo Lớp cơ bản:**

  ```typescript
  class Greeter {
    // Thuộc tính (Properties)
    greeting: string;

    // Hàm khởi tạo (Constructor)
    constructor(message: string) {
      this.greeting = message;
    }

    // Phương thức (Methods)
    greet(): string {
      return "Hello, " + this.greeting;
    }
  }

  let greeterInstance = new Greeter("world");
  console.log(greeterInstance.greet()); // "Hello, world"
  ```

- **Thuộc tính (Properties):**

  - Là các biến được liên kết với một instance của lớp.
  - Bạn có thể khai báo kiểu cho thuộc tính.
  - Có thể có giá trị khởi tạo mặc định: `isActive: boolean = true;`

- **Hàm khởi tạo (Constructor):**

  - Một hàm đặc biệt được gọi khi một đối tượng mới của lớp được tạo ra (`new ClassName(...)`).
  - Thường dùng để khởi tạo các thuộc tính của lớp.
  - Một lớp chỉ có thể có một constructor.

- **Phương thức (Methods):**
  - Là các hàm được định nghĩa bên trong lớp và hoạt động trên dữ liệu của đối tượng (thông qua `this`).

**2. Kế thừa (Inheritance)**

TypeScript hỗ trợ kế thừa đơn, cho phép một lớp (lớp con - subclass/derived class) kế thừa các thuộc tính và phương thức từ một lớp khác (lớp cha - superclass/base class).

- **Từ khóa `extends`:**

  ```typescript
  class Animal {
    name: string;
    constructor(theName: string) {
      this.name = theName;
    }
    move(distanceInMeters: number = 0) {
      console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
  }

  class Snake extends Animal {
    constructor(name: string) {
      super(name); // Gọi constructor của lớp cha (Animal)
    }
    // Override phương thức move
    move(distanceInMeters = 5) {
      console.log("Slithering...");
      super.move(distanceInMeters); // Gọi phương thức move của lớp cha
    }
  }

  class Horse extends Animal {
    constructor(name: string) {
      super(name);
    }
    move(distanceInMeters = 45) {
      // Override
      console.log("Galloping...");
      super.move(distanceInMeters);
    }
  }

  let sam = new Snake("Sammy the Python");
  let tom: Animal = new Horse("Tommy the Palomino"); // tom là Animal nhưng thực chất là Horse

  sam.move();
  tom.move(34);
  ```

- **`super`:**

  - Trong constructor của lớp con, `super()` được dùng để gọi constructor của lớp cha. Nó phải được gọi trước khi truy cập `this` trong constructor của lớp con.
  - Trong phương thức của lớp con, `super.methodName()` được dùng để gọi phương thức tương ứng của lớp cha.

- **Override (Ghi đè phương thức):** Lớp con có thể cung cấp một cài đặt cụ thể cho một phương thức đã được định nghĩa ở lớp cha.

**3. Access Modifiers (Bộ điều chỉnh Truy cập)**

TypeScript cung cấp các access modifiers để kiểm soát khả năng truy cập vào các thành viên (thuộc tính và phương thức) của một lớp từ bên ngoài lớp đó.

- **`public` (Mặc định):**

  - Thành viên có thể được truy cập từ bất kỳ đâu.
  - Nếu không chỉ định modifier, mặc định là `public`.
  - ```typescript
    class MyClassPublic {
      public name: string; // Tường minh
      age: number; // Mặc định là public
      constructor(n: string, a: number) {
        this.name = n;
        this.age = a;
      }
    }
    let instPub = new MyClassPublic("Test", 30);
    console.log(instPub.name); // OK
    ```

  ```

  ```

- **`private`:**

  - Thành viên chỉ có thể được truy cập từ bên trong chính lớp đó.
  - Không thể truy cập từ instance bên ngoài hoặc từ lớp con.
  - ```typescript
    class MyClassPrivate {
      private secret: string;
      constructor(s: string) {
        this.secret = s;
      }
      revealSecret() {
        console.log(this.secret); // OK, bên trong lớp
      }
    }
    let instPriv = new MyClassPrivate("My Secret");
    // console.log(instPriv.secret); // Lỗi: Property 'secret' is private and only accessible within class 'MyClassPrivate'.
    instPriv.revealSecret(); // OK
    ```

  class SubClassPrivate extends MyClassPrivate {
  show() {
  // console.log(this.secret); // Lỗi: Property 'secret' is private...
  }
  }

  ```

  ```

- **`protected`:**

  - Thành viên có thể được truy cập từ bên trong chính lớp đó và từ các lớp con kế thừa nó.
  - Không thể truy cập từ instance bên ngoài.
  - ```typescript
    class PersonProtected {
      protected name: string;
      constructor(name: string) {
        this.name = name;
      }
    }
    ```

  class EmployeeProtected extends PersonProtected {
  private department: string;
  constructor(name: string, department: string) {
  super(name);
  this.department = department;
  }
  public getElevatorPitch() {
  return `Hello, my name is ${this.name} and I work in ${this.department}.`; // OK, 'name' is protected
  }
  }

  let howard = new EmployeeProtected("Howard", "Sales");
  console.log(howard.getElevatorPitch());
  // console.log(howard.name); // Lỗi: Property 'name' is protected and only accessible within class 'PersonProtected' and its subclasses.

  ```

  ```

- **So sánh JavaScript private fields (`#`):**
  - JavaScript gần đây đã giới thiệu cú pháp private fields với `#` (ví dụ: `#secretField`).
  - TypeScript cũng hỗ trợ cú pháp này và nó thực sự tạo ra "hard private" ở runtime (không thể truy cập từ bên ngoài bằng bất kỳ cách nào, kể cả JavaScript thuần).
  - `private` của TypeScript là một kiểm tra ở compile-time. Sau khi biên dịch thành JS, nó không còn là private nữa (có thể truy cập được).
  - **Khi nào dùng cái nào?**
    - Nếu bạn cần "true privacy" ở runtime, dùng `#`.
    - Nếu kiểm tra compile-time là đủ và bạn muốn cú pháp quen thuộc hơn, `private` của TS là lựa chọn tốt. Nhiều dự án lớn vẫn dùng `private` của TS.

**4. Readonly Modifier**

- Thuộc tính được đánh dấu `readonly` chỉ có thể được gán giá trị tại thời điểm khai báo hoặc bên trong constructor của cùng lớp.
- ```typescript
  class Octopus {
    readonly name: string;
    readonly numberOfLegs: number = 8;
    constructor(theName: string) {
      this.name = theName; // OK, trong constructor
    }
    changeName(newName: string) {
      // this.name = newName; // Lỗi! Cannot assign to 'name' because it is a read-only property.
    }
  }
  let dad = new Octopus("Man with the 8 legs");
  // dad.name = "Man with the 3-piece suit"; // Lỗi!
  ```

**5. Parameter Properties (Thuộc tính Tham số)**

Một cách viết tắt tiện lợi để khai báo và khởi tạo thuộc tính thành viên từ tham số của constructor.

- ```typescript
  class Student {
    // Thay vì:
    // public id: number;
    // public name: string;
    // constructor(id: number, name: string) {
    //     this.id = id;
    //     this.name = name;
    // }

    // Bạn có thể viết:
    constructor(
      public readonly id: number,
      public name: string,
      private age: number
    ) {
      // Không cần code gì ở đây nếu chỉ là gán giá trị
    }

    displayInfo() {
      console.log(`ID: ${this.id}, Name: ${this.name}, Age: ${this.age}`);
      // this.id = 2; // Lỗi, id là readonly
    }
  }
  let student = new Student(1, "Alice", 20);
  student.displayInfo();
  console.log(student.name); // Alice
  // console.log(student.age); // Lỗi, age là private
  ```

- Bằng cách thêm access modifier (`public`, `private`, `protected`) hoặc `readonly` vào trước tên tham số trong constructor, TypeScript sẽ tự động:
  1.  Khai báo một thuộc tính thành viên với tên và kiểu đó.
  2.  Gán giá trị của tham số cho thuộc tính đó.

**6. Getters và Setters (Accessors)**

Cho phép bạn kiểm soát việc truy cập và thay đổi giá trị của một thuộc tính, giống như các phương thức nhưng được sử dụng như thuộc tính.

- ```typescript
  let passcode = "secret passcode";

  class EmployeeAccess {
    private _fullName: string = ""; // Thường dùng _ để đặt tên cho thuộc tính private "thật"

    get fullName(): string {
      console.log("Getter for fullName called");
      if (this.hasAccess()) {
        return this._fullName;
      }
      return "Access Denied";
    }

    set fullName(newName: string) {
      console.log("Setter for fullName called");
      if (this.hasAccess() && newName.length > 0) {
        this._fullName = newName;
      } else {
        console.error("Invalid name or no access.");
      }
    }

    private hasAccess(): boolean {
      // Logic kiểm tra quyền, ví dụ:
      return passcode === "secret passcode";
    }
  }

  let employee = new EmployeeAccess();
  employee.fullName = "Bob Smith"; // Gọi setter
  if (employee.fullName !== "Access Denied") {
    // Gọi getter
    console.log(employee.fullName);
  }

  passcode = "wrong passcode";
  employee.fullName = "John Doe"; // Gọi setter, nhưng có thể bị từ chối
  console.log(employee.fullName); // Gọi getter, có thể trả về "Access Denied"
  ```

- **Lợi ích:**
  - Thêm logic kiểm tra (validation) khi gán giá trị.
  - Thực hiện tính toán hoặc lấy dữ liệu động khi truy cập.
  - Che giấu cách lưu trữ nội bộ của thuộc tính.

**7. Thuộc tính và Phương thức Tĩnh (Static Members)**

- Thuộc tính và phương thức tĩnh thuộc về chính lớp đó, chứ không phải một instance cụ thể của lớp.
- Truy cập thông qua tên lớp, không phải `this`.
- ```typescript
  class Grid {
    static origin = { x: 0, y: 0 }; // Thuộc tính tĩnh

    // Phương thức tĩnh
    static calculateDistance(
      point1: { x: number; y: number },
      point2: { x: number; y: number }
    ): number {
      let xDist = point2.x - point1.x;
      let yDist = point2.y - point1.y;
      return Math.sqrt(xDist * xDist + yDist * yDist);
    }

    scale: number;
    constructor(scale: number) {
      this.scale = scale;
    }
  }

  console.log(Grid.origin); // { x: 0, y: 0 }
  let dist = Grid.calculateDistance({ x: 3, y: 4 }, Grid.origin);
  console.log(dist); // 5

  let grid1 = new Grid(1.0);
  // console.log(grid1.origin); // Lỗi: Property 'origin' does not exist on type 'Grid'. It is a static member of 'Grid'.
  ```

- **Real-world usage:**
  - Các hằng số chung cho tất cả các instance (ví dụ: `Math.PI`).
  - Các hàm tiện ích liên quan đến lớp nhưng không cần một instance cụ thể (factory methods, helper functions).

**8. Lớp Trừu tượng (Abstract Classes)**

- Lớp trừu tượng không thể được khởi tạo trực tiếp (`new AbstractClass()`). Chúng dùng làm lớp cơ sở cho các lớp khác kế thừa.
- Có thể chứa các phương thức trừu tượng (không có phần thân cài đặt) và các phương thức đã cài đặt.
- Lớp con kế thừa từ lớp trừu tượng _phải_ cài đặt tất cả các phương thức trừu tượng của lớp cha.
- ```typescript
  abstract class Department {
    name: string;

    constructor(name: string) {
      this.name = name;
    }

    printName(): void {
      console.log("Department name: " + this.name);
    }

    abstract printMeeting(): void; // Phương thức trừu tượng, phải được cài đặt ở lớp con
  }

  class AccountingDepartment extends Department {
    constructor() {
      super("Accounting and Auditing"); // Bắt buộc gọi constructor lớp cha
    }

    printMeeting(): void {
      // Cài đặt phương thức trừu tượng
      console.log("The Accounting Department meets each Monday at 10am.");
    }

    generateReports(): void {
      console.log("Generating accounting reports...");
    }
  }

  // let department = new Department("HR"); // Lỗi! Cannot create an instance of an abstract class.
  let department: Department; // OK, có thể tạo tham chiếu kiểu Department
  department = new AccountingDepartment(); // OK
  department.printName();
  department.printMeeting();
  // department.generateReports(); // Lỗi: Property 'generateReports' does not exist on type 'Department'.
  // Vì department được khai báo là Department, không phải AccountingDepartment.
  // Cần ép kiểu hoặc khai báo department là AccountingDepartment để gọi.
  if (department instanceof AccountingDepartment) {
    department.generateReports(); // OK, sau khi type guard
  }
  ```

- **Tác dụng:**
  - Định nghĩa một cấu trúc chung và các hành vi bắt buộc cho các lớp con.
  - Cho phép đa hình (polymorphism) - đối xử với các đối tượng của lớp con như là kiểu của lớp cha trừu tượng.

**9. Interfaces và Classes (`implements`)**

Như đã đề cập ở Phần 2, một lớp có thể `implements` (triển khai) một hoặc nhiều interfaces. Điều này đảm bảo rằng lớp đó tuân thủ "hợp đồng" được định nghĩa bởi interface (tức là nó phải có các thuộc tính và phương thức mà interface yêu cầu).

- ```typescript
  interface ClockInterface {
    currentTime: Date;
    setTime(d: Date): void;
  }

  interface Alarm {
    setAlarm(time: Date): void;
    snooze(): void;
  }

  class DigitalClock implements ClockInterface, Alarm {
    // Có thể implements nhiều interfaces
    currentTime: Date = new Date();

    constructor(h: number, m: number) {
      /* ... */
    }

    setTime(d: Date) {
      this.currentTime = d;
    }

    setAlarm(time: Date) {
      console.log(`Alarm set for ${time}`);
    }

    snooze() {
      console.log("Snoozing...");
    }
  }
  ```

- Interface chỉ mô tả phần "public" của lớp. Nó không quan tâm đến chi tiết cài đặt private.
- Khác với `extends` (chỉ có thể kế thừa từ một lớp), một lớp có thể `implements` nhiều interfaces.

---

**Tạm kết Phần 4:**

Bạn đã khám phá sâu về cách TypeScript hỗ trợ Lập trình Hướng đối tượng, từ việc tạo `Classes`, sử dụng `Inheritance`, kiểm soát truy cập với `Access Modifiers` và `readonly`, cho đến các tính năng nâng cao như `Static Members`, `Abstract Classes`, và sự kết hợp mạnh mẽ giữa `Classes` và `Interfaces` thông qua `implements`. Những công cụ này giúp bạn xây dựng các ứng dụng lớn, có cấu trúc tốt, dễ bảo trì và mở rộng.

**Trong Phần 5 (phần cuối cùng), chúng ta sẽ tập trung vào các chủ đề nâng cao hơn, các công cụ và thực tiễn tốt nhất để "trở thành pro TypeScript": Modules, Namespaces, Decorators, Utility Types, cấu hình `tsconfig.json` chuyên sâu, và các mẹo để viết code TypeScript hiệu quả và dễ đọc.**

Tuyệt vời! Chúng ta đã đi một chặng đường dài và đây là phần cuối cùng để hoàn thiện kiến thức và giúp bạn thực sự "pro" với TypeScript.

**Phần 5: Modules, Namespaces, Decorators, Utility Types và Thực tiễn Tốt nhất**

---

Phần này sẽ tập trung vào cách tổ chức code trong các dự án lớn, các tính năng nâng cao của TypeScript và những thực tiễn giúp bạn viết code TypeScript chất lượng cao.

**1. Modules (Mô-đun)**

Trong JavaScript hiện đại (và do đó là TypeScript), modules là cách để tổ chức code thành các tệp riêng biệt. Mỗi tệp module có phạm vi (scope) riêng, và bạn có thể `export` (xuất) các biến, hàm, lớp, interfaces, v.v. từ một module để `import` (nhập) và sử dụng chúng trong các module khác.

- **Tại sao cần Modules?**

  - **Tổ chức code:** Chia nhỏ code thành các phần logic, dễ quản lý.
  - **Tái sử dụng:** Dễ dàng sử dụng lại code ở nhiều nơi.
  - **Phạm vi (Scoping):** Tránh xung đột tên biến/hàm giữa các phần khác nhau của ứng dụng (biến trong một module không tự động có sẵn ở global scope).
  - **Lazy Loading:** Các trình bundler hiện đại có thể sử dụng modules để tải code theo yêu cầu, cải thiện hiệu suất ban đầu.

- **Cú pháp ES Modules (Được TypeScript hỗ trợ đầy đủ):**

  - **Exporting:**

    ```typescript
    // ----- StringUtils.ts -----
    export const PI = 3.14159;

    export function toUpperCase(str: string): string {
      return str.toUpperCase();
    }

    export interface User {
      id: number;
      name: string;
    }

    // Export default (chỉ một default export cho mỗi module)
    export default class Calculator {
      add(x: number, y: number): number {
        return x + y;
      }
    }

    // Re-exporting
    // export { someFunction } from './anotherModule';
    // export * from './anotherModule'; // Export tất cả từ module khác
    ```

  - **Importing:**

    ```typescript
    // ----- App.ts -----
    import Calculator, {
      PI,
      toUpperCase,
      User as PersonUser,
    } from "./StringUtils";
    //             ^ Default import   ^{ Named imports, có thể đổi tên với 'as' }

    // Import tất cả các named export vào một object
    // import * as Utils from './StringUtils';

    console.log(PI); // 3.14159
    console.log(toUpperCase("hello")); // HELLO

    const calc = new Calculator();
    console.log(calc.add(5, 3)); // 8

    let user: PersonUser = { id: 1, name: "Alice" };
    console.log(user);
    ```

- **Cấu hình Module trong `tsconfig.json`:**

  - `"module"`: Xác định hệ thống module mà trình biên dịch sẽ tạo ra (ví dụ: `"commonjs"`, `"es2015"`, `"esnext"`).
    - `"commonjs"`: Thường dùng cho Node.js.
    - `"esnext"` hoặc `"es2015"` (hay `"es6"`): Giữ nguyên cú pháp ES Module, phù hợp cho các trình bundler như Webpack, Rollup, Parcel để chúng có thể thực hiện tree-shaking (loại bỏ code không dùng).
  - `"moduleResolution"`: Cách TypeScript tìm các file module (thường là `"node"` cho các dự án Node.js và web hiện đại).

- **Real-world usage:**
  - Hầu hết các dự án TypeScript hiện đại đều sử dụng ES Modules.
  - Chia nhỏ components, services, utility functions, type definitions thành các file riêng biệt.

**2. Namespaces (Không gian tên)**

Trước khi ES Modules trở nên phổ biến, TypeScript có `namespaces` (ban đầu gọi là `internal modules`) để giải quyết vấn đề xung đột tên trong global scope, đặc biệt hữu ích khi code chưa được tổ chức thành nhiều file.

- **Khai báo:**

  ```typescript
  namespace MyMath {
    const PI_INTERNAL = 3.14;
    export function calculateCircumference(diameter: number): number {
      return diameter * PI_INTERNAL;
    }
    export function calculateRectangleArea(
      width: number,
      height: number
    ): number {
      return width * height;
    }
  }

  // Sử dụng
  console.log(MyMath.calculateCircumference(10));
  // console.log(MyMath.PI_INTERNAL); // Lỗi, không được export
  ```

- **Phân tách Namespace qua nhiều file:**

  - Bạn có thể chia một namespace ra nhiều file, và TypeScript sẽ hợp nhất chúng lại khi biên dịch (nếu biên dịch tất cả các file cùng lúc hoặc sử dụng tùy chọn `--outFile`).
  - ```typescript
    // --- validation-rules.ts ---
    namespace Validation {
      export interface StringValidator {
        isAcceptable(s: string): boolean;
      }
    }
    ```

  // --- letters-only-validator.ts ---
  /// <reference path="validation-rules.ts" />
  namespace Validation { // Cùng namespace Validation
  const lettersRegexp = /^[A-Za-z]+$/;
  export class LettersOnlyValidator implements StringValidator {
  isAcceptable(s: string) {
  return lettersRegexp.test(s);
  }
  }
  }

  // --- main.ts ---
  /// <reference path="validation-rules.ts" />
  /// <reference path="letters-only-validator.ts" />
  let validator = new Validation.LettersOnlyValidator();
  console.log(validator.isAcceptable("HelloWorld")); // true

  ```
  *   Thẻ `/// <reference path="..." />` là một "triple-slash directive" để chỉ định sự phụ thuộc giữa các file.

  ```

- **Namespaces vs. Modules:**
  - **Khuyến nghị hiện tại: Ưu tiên sử dụng ES Modules thay vì Namespaces cho việc tổ chức code.** Modules tích hợp tốt hơn với hệ sinh thái JavaScript hiện đại (NPM, bundlers).
  - Namespaces vẫn có thể hữu ích trong một số trường hợp rất cụ thể, ví dụ: khi bạn cần tạo một file JS duy nhất từ nhiều file TS (sử dụng `--outFile`) và muốn tránh xung đột global, hoặc khi làm việc với code JavaScript cũ sử dụng global objects.

**3. Decorators (Trình trang trí)**

Decorators là một tính năng thử nghiệm (experimental) trong TypeScript (và là một đề xuất cho JavaScript). Chúng cung cấp một cách để thêm metadata hoặc thay đổi hành vi của các khai báo lớp, phương thức, thuộc tính, hoặc tham số bằng cú pháp đặc biệt `@expression`.

- **Bật Decorators:** Cần bật cờ `"experimentalDecorators": true` và `"emitDecoratorMetadata": true` (nếu dùng với thư viện như TypeORM, NestJS) trong `tsconfig.json`.

- **Các loại Decorators:**

  - **Class Decorators:** `@sealed class Greeter {}`
  - **Method Decorators:** `class C { @enumerable(false) greet() {} }`
  - **Accessor Decorators:** `class C { @configurable(false) get x() {} }`
  - **Property Decorators:** `class C { @format("Hello, %s") greeting: string; }`
  - **Parameter Decorators:** `class C { greet(@required name: string) {} }`

- **Ví dụ đơn giản (Method Decorator):**

  ```typescript
  // tsconfig.json: "experimentalDecorators": true
  function logMethod(
    target: any,
    propertyKey: string,
    descriptor: PropertyDescriptor
  ) {
    console.log(`Method Decorator called on: ${propertyKey}`);
    const originalMethod = descriptor.value; // Lưu lại phương thức gốc

    // Ghi đè phương thức gốc
    descriptor.value = function (...args: any[]) {
      console.log(
        `LOG: Calling ${propertyKey} with arguments: ${JSON.stringify(args)}`
      );
      const result = originalMethod.apply(this, args);
      console.log(`LOG: ${propertyKey} returned: ${JSON.stringify(result)}`);
      return result;
    };
  }

  class MyService {
    @logMethod
    add(a: number, b: number): number {
      return a + b;
    }
  }

  const service = new MyService();
  service.add(2, 3);
  // Output:
  // Method Decorator called on: add
  // LOG: Calling add with arguments: [2,3]
  // LOG: add returned: 5
  ```

- **Real-world usage (Thường thấy trong các frameworks):**
  - **Angular:** Dùng decorators rộng rãi (`@Component`, `@Injectable`, `@Input`, `@Output`).
  - **NestJS (Node.js framework):** Tương tự Angular (`@Controller`, `@Module`, `@Injectable`, `@Get`, `@Post`).
  - **TypeORM (ORM):** `@Entity`, `@Column`, `@PrimaryGeneratedColumn`.
  - **MobX (State management):** `@observable`, `@action`, `@computed`.
  - Mục đích là để giảm code boilerplate, thêm metadata một cách khai báo, hoặc thay đổi hành vi của code.

**4. Utility Types (Kiểu Tiện ích)**

TypeScript cung cấp một số kiểu tiện ích toàn cục để giúp thực hiện các phép biến đổi kiểu phổ biến. Chúng rất hữu ích để tạo ra các kiểu mới dựa trên các kiểu hiện có mà không cần viết nhiều code lặp đi lặp lại.

- **Một số Utility Types quan trọng:**

  - **`Partial<T>`:** Tạo một kiểu mới với tất cả các thuộc tính của `T` đều là tùy chọn (`optional`).
    ```typescript
    interface Todo {
      title: string;
      description: string;
      completed: boolean;
    }
    function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
      return { ...todo, ...fieldsToUpdate };
    }
    const myTodo: Todo = {
      title: "Learn TS",
      description: "...",
      completed: false,
    };
    const updatedTodo = updateTodo(myTodo, { description: "Finish all parts" });
    ```
  - **`Required<T>`:** Tạo một kiểu mới với tất cả các thuộc tính của `T` đều là bắt buộc (ngược lại với `Partial`).
  - **`Readonly<T>`:** Tạo một kiểu mới với tất cả các thuộc tính của `T` đều là `readonly`.
    ```typescript
    const readonlyTodo: Readonly<Todo> = {
      title: "Final Todo",
      description: "Cannot change",
      completed: true,
    };
    // readonlyTodo.title = "New title"; // Lỗi!
    ```
  - **`Pick<T, K>`:** Tạo một kiểu mới bằng cách chọn một tập hợp các thuộc tính `K` (một union của string literals hoặc keyof T) từ `T`.
    ```typescript
    interface UserProfile {
      id: number;
      username: string;
      email: string;
      bio?: string;
    }
    type UserPreview = Pick<UserProfile, "id" | "username">; // { id: number; username: string; }
    ```
  - **`Omit<T, K>`:** Tạo một kiểu mới bằng cách loại bỏ một tập hợp các thuộc tính `K` từ `T` (ngược lại với `Pick`).
    ```typescript
    type UserContactInfo = Omit<UserProfile, "id" | "bio">; // { username: string; email: string; }
    ```
  - **`Record<K, T>`:** Tạo một object type với tập hợp các key `K` (thường là `string | number | symbol` hoặc một union của string literals) và tất cả các value có kiểu `T`.
    ```typescript
    type PageInfo = { title: string; visited: boolean };
    type Pages = Record<string, PageInfo>;
    const sitePages: Pages = {
      home: { title: "Homepage", visited: true },
      about: { title: "About Us", visited: false },
    };
    ```
  - **`Exclude<T, U>`:** Loại bỏ các kiểu trong `U` khỏi `T` (khi `T` và `U` là union types).
    `type T0 = Exclude<"a" | "b" | "c", "a">; // "b" | "c"`
  - **`Extract<T, U>`:** Trích xuất các kiểu trong `U` mà cũng có trong `T` (khi `T` và `U` là union types).
    `type T1 = Extract<"a" | "b" | "c", "a" | "f">; // "a"`
  - **`NonNullable<T>`:** Loại bỏ `null` và `undefined` khỏi `T`.
    `type T2 = NonNullable<string | number | undefined | null>; // string | number`
  - **`ReturnType<T>`:** Lấy kiểu trả về của một hàm `T`.
    `type MyFunc = () => string; type MyFuncReturn = ReturnType<MyFunc>; // string`
  - **`Parameters<T>`:** Lấy kiểu của các tham số của một hàm `T` dưới dạng một tuple.
  - **`ConstructorParameters<T>`:** Lấy kiểu của các tham số của constructor của một lớp `T` dưới dạng một tuple.
  - **`Awaited<T>`:** Lấy kiểu "unwrapped" của một `Promise` (ví dụ: `Awaited<Promise<string>>` là `string`).

- **Các chuyên gia dùng Utility Types như thế nào?**
  - Để giảm thiểu việc lặp lại định nghĩa kiểu.
  - Tạo ra các API type-safe và linh hoạt hơn.
  - Khi làm việc với dữ liệu từ API, biến đổi props của component, v.v.

**5. `tsconfig.json` - Cấu hình Chuyên sâu và Thực tiễn Tốt nhất**

File `tsconfig.json` là trung tâm điều khiển dự án TypeScript của bạn. Nắm vững các tùy chọn quan trọng sẽ giúp bạn tối ưu hóa quy trình làm việc và chất lượng code.

- **Các tùy chọn quan trọng cần chú ý:**

  - **`compilerOptions`:**

    - **`target`**: Phiên bản JavaScript đầu ra (ví dụ: `"es5"`, `"es2017"`, `"esnext"`). Chọn target phù hợp với môi trường chạy code của bạn (trình duyệt cũ, Node.js phiên bản mới).
    - **`module`**: Như đã nói ở trên (ví dụ: `"commonjs"`, `"esnext"`).
    - **`lib`**: Các thư viện định nghĩa kiểu tích hợp sẵn mà trình biên dịch sẽ bao gồm (ví dụ: `"dom"`, `"es2017"`, `"scripthost"`).
    - **`outDir`**: Thư mục chứa các file `.js` đã biên dịch. (ví dụ: `"./dist"`)
    - **`rootDir`**: Thư mục gốc chứa các file `.ts` nguồn. (ví dụ: `"./src"`)
    - **`strict`**: Bật tất cả các tùy chọn kiểm tra kiểu nghiêm ngặt. **Rất khuyến khích bật (`true`) cho các dự án mới.** Bao gồm:
      - **`noImplicitAny`**: Báo lỗi nếu có biến hoặc tham số có kiểu `any` ngầm định.
      - **`strictNullChecks`**: Xử lý `null` và `undefined` một cách nghiêm ngặt hơn (không thể gán cho các kiểu khác trừ khi được khai báo rõ ràng).
      - **`strictFunctionTypes`**: Kiểm tra kiểu tham số hàm một cách chặt chẽ hơn trong các tình huống bivariant/contravariant.
      - **`strictPropertyInitialization`**: Đảm bảo các thuộc tính của lớp được khởi tạo trong constructor hoặc có giá trị mặc định (trừ khi có `!`).
      - **`noImplicitThis`**: Báo lỗi khi `this` có kiểu `any`.
      - **`alwaysStrict`**: Luôn parse ở strict mode và thêm `"use strict"` vào đầu mỗi file JavaScript output.
    - **`esModuleInterop`: `true`** (Khuyến khích): Cho phép tương tác tốt hơn giữa CommonJS modules và ES Modules. Giúp `import React from 'react'` hoạt động trơn tru.
    - **`allowSyntheticDefaultImports`: `true`** (Thường đi kèm `esModuleInterop`): Cho phép default import từ các module không có default export thực sự (trình bundler sẽ xử lý).
    - **`skipLibCheck`: `true`** (Có thể cần thiết): Bỏ qua kiểm tra kiểu của các file định nghĩa (`.d.ts`) trong `node_modules`. Giúp tăng tốc độ biên dịch, đặc biệt khi một số thư viện có định nghĩa kiểu không hoàn toàn chính xác.
    - **`forceConsistentCasingInFileNames`: `true`** (Khuyến khích): Đảm bảo tên file được tham chiếu nhất quán về chữ hoa/thường.
    - **`sourceMap`: `true`**: Tạo file `.map` để debug code TypeScript trực tiếp trên trình duyệt.
    - **`declaration`: `true`**: Tạo các file định nghĩa kiểu (`.d.ts`) tương ứng cho code của bạn. Quan trọng khi xây dựng thư viện.
    - **`declarationDir`**: Thư mục chứa các file `.d.ts` được tạo ra.
    - **`resolveJsonModule`: `true`**: Cho phép import file `.json` như một module.
    - **`baseUrl` và `paths`**: Dùng để cấu hình alias cho đường dẫn import, giúp import gọn gàng hơn (ví dụ: `@components/*` thay vì `../../../components/*`).
      ```json
      {
        "compilerOptions": {
          "baseUrl": "./src", // Thư mục gốc cho paths
          "paths": {
            "@components/*": ["components/*"],
            "@utils/*": ["utils/*"]
          }
        }
      }
      ```

  - **`include`**: Một mảng các glob pattern để chỉ định các file TypeScript cần biên dịch. (ví dụ: `["src/**/*"]`)
  - **`exclude`**: Một mảng các glob pattern để loại trừ các file khỏi quá trình biên dịch. (ví dụ: `["node_modules", "**/*.spec.ts"]`)
  - **`extends`**: Kế thừa cấu hình từ một file `tsconfig.json` khác. Hữu ích cho các monorepo.

- **Thực tiễn tốt nhất cho `tsconfig.json`:**
  - **Luôn bật `strict: true` cho dự án mới.** Ban đầu có thể hơi khó khăn, nhưng nó sẽ cứu bạn khỏi vô số lỗi tiềm ẩn.
  - Điều chỉnh `target` và `module` phù hợp với dự án.
  - Sử dụng `outDir` và `rootDir` để giữ cấu trúc dự án sạch sẽ.
  - Cân nhắc `esModuleInterop` và `allowSyntheticDefaultImports` để tương thích tốt hơn.
  - Tìm hiểu các tùy chọn khác khi cần, tài liệu của TypeScript rất chi tiết.

**6. Viết Code TypeScript "Pro" - Mẹo và Thực tiễn**

- **Ưu tiên sự rõ ràng và An toàn kiểu:**

  - **Tránh `any` càng nhiều càng tốt.** Hãy cố gắng định nghĩa kiểu cụ thể. Nếu thực sự không biết, `unknown` an toàn hơn `any` vì nó bắt bạn phải kiểm tra kiểu trước khi sử dụng.
  - Sử dụng `readonly` cho các thuộc tính không nên thay đổi sau khi khởi tạo.
  - Tận dụng `strictNullChecks`. Xử lý các giá trị có thể là `null` hoặc `undefined` một cách tường minh (dùng `?`, `!`, `if` checks).
  - Viết các hàm nhỏ, có mục đích rõ ràng với kiểu tham số và kiểu trả về được định nghĩa tốt.

- **Tận dụng Suy luận kiểu (Type Inference), nhưng chú thích khi cần:**

  - TypeScript rất giỏi trong việc suy luận kiểu. Không cần chú thích kiểu cho mọi biến nếu kiểu đã rõ ràng từ giá trị khởi tạo.
  - **Luôn chú thích kiểu cho tham số hàm và kiểu trả về của hàm (trừ khi rất ngắn gọn và rõ ràng).** Điều này làm cho API của hàm dễ hiểu và an toàn hơn.
  - Chú thích kiểu cho các biến chưa được khởi tạo hoặc khi bạn muốn một kiểu cụ thể hơn kiểu được suy luận.

- **Sử dụng Interfaces và Type Aliases một cách hợp lý:**

  - Dùng `interface` cho việc định hình đối tượng và khi cần declaration merging.
  - Dùng `type` cho union, intersection, tuple, và các kiểu phức tạp khác.
  - Đặt tên kiểu rõ ràng và có ý nghĩa.

- **Viết code dễ đọc và bảo trì:**

  - Định dạng code nhất quán (sử dụng Prettier cùng với ESLint).
  - Sử dụng generics để viết code tái sử dụng.
  - Chia code thành các modules nhỏ, dễ quản lý.
  - Viết comment khi logic phức tạp hoặc không rõ ràng ngay lập tức.

- **Hiểu về Hệ sinh thái:**

  - Làm quen với các thư viện định nghĩa kiểu trong `@types` (DefinitelyTyped). Hầu hết các thư viện JavaScript phổ biến đều có định nghĩa kiểu. Cài đặt: `npm install --save-dev @types/lodash`.
  - Sử dụng ESLint với các plugin TypeScript (`@typescript-eslint/eslint-plugin`, `@typescript-eslint/parser`) để kiểm tra code và tuân thủ các quy tắc tốt nhất.
  - Tích hợp TypeScript vào quy trình build và CI/CD của bạn.

- **Không ngừng học hỏi:**
  - TypeScript liên tục phát triển. Theo dõi blog của TypeScript, các bản phát hành mới.
  - Đọc code TypeScript của các dự án mã nguồn mở lớn để học hỏi.
  - Thực hành nhiều!

---

**Kết luận Toàn bộ 5 Phần:**

Chúc mừng bạn! Bạn đã đi qua một hành trình toàn diện về TypeScript, từ những khái niệm cơ bản nhất cho đến các tính năng nâng cao và thực tiễn tốt nhất.

1.  **Nền tảng:** Bạn hiểu TypeScript là gì, tại sao nó quan trọng, các kiểu dữ liệu cơ bản và cách trình biên dịch hoạt động.
2.  **Định hình Dữ liệu:** Bạn thành thạo `Interfaces` và `Type Aliases`, `Union`, `Literal`, `Intersection Types` để tạo ra các "hợp đồng" mạnh mẽ.
3.  **Linh hoạt và An toàn:** Bạn biết cách sử dụng `Generics` để viết code tái sử dụng, các kỹ thuật `Type Narrowing` và khi nào nên dùng `Type Assertions`.
4.  **Hướng đối tượng:** Bạn nắm vững `Classes`, `Inheritance`, `Access Modifiers`, `Abstract Classes` và cách chúng tương tác với `Interfaces`.
5.  **Chuyên sâu và Thực tiễn:** Bạn đã học về `Modules`, `Namespaces`, `Decorators`, `Utility Types`, cách cấu hình `tsconfig.json` chuyên sâu và các mẹo để viết code TypeScript như một chuyên gia.

Con đường để trở thành "pro" thực sự đòi hỏi thực hành liên tục và áp dụng kiến thức này vào các dự án thực tế. Với nền tảng JavaScript sẵn có và những gì bạn vừa học, bạn đã có đủ công cụ để bắt đầu xây dựng các ứng dụng TypeScript mạnh mẽ, dễ bảo trì và ít lỗi hơn.
