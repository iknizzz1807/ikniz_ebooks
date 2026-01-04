Dưới đây là **PHẦN 1** của lộ trình học, tập trung vào việc xây dựng nền tảng TypeScript vững chắc.

---

### **PHẦN 1: Nắm vững TypeScript Cơ bản - Nền tảng cho Backend Development**

1.  **Tên phần học:** Nắm vững TypeScript Cơ bản - Nền tảng cho Backend Development.

2.  **Mục tiêu học phần:**

    - Hiểu rõ **tại sao** TypeScript ra đời và lợi ích cốt lõi mà nó mang lại so với JavaScript thuần.
    - Nắm vững các khái niệm cơ bản của TypeScript: kiểu dữ liệu (primitive, object, array, tuple, enum, any, unknown, void, never), type inference, type assertion.
    - Sử dụng thành thạo cách định nghĩa kiểu cho hàm (tham số, kiểu trả về).
    - Hiểu và biết cách sử dụng `interface` để định nghĩa cấu trúc dữ liệu (object shapes).
    - Làm quen với `class` trong TypeScript ở mức độ cơ bản (khai báo, thuộc tính, phương thức).
    - Biết cách thiết lập một dự án TypeScript đơn giản và quy trình biên dịch (transpilation) sang JavaScript.
    - Trả lời được:
      - TypeScript là gì và tại sao nó quan trọng cho các dự án lớn, đặc biệt là backend?
      - Sự khác biệt giữa static typing và dynamic typing là gì?
      - Khi nào nên sử dụng `interface`?
      - Làm thế nào để đảm bảo an toàn kiểu trong code TypeScript?

3.  **Kiến thức tiên quyết:**

    - **JavaScript (ES6+):** Hiểu biết vững chắc về JavaScript hiện đại là bắt buộc (variables, scope, closures, functions, `this` keyword, array methods, promises, async/await).
    - **Khái niệm lập trình cơ bản:** Biến, kiểu dữ liệu, vòng lặp, câu lệnh điều kiện, hàm.
    - **Node.js và npm/yarn:** Kinh nghiệm cơ bản về cài đặt package và chạy script.

4.  **Giải thích lý thuyết chuyên sâu:**

    - **Tại sao TypeScript ra đời? Vấn đề của JavaScript trong các dự án lớn:**

      - JavaScript là ngôn ngữ **dynamic typing** (kiểu động). Kiểu của biến được xác định tại thời điểm chạy (runtime).
        - **Ưu điểm:** Linh hoạt, dễ viết code nhanh ban đầu.
        - **Nhược điểm:** Dễ phát sinh lỗi runtime do sai kiểu (ví dụ: `undefined is not a function`, `cannot read property 'x' of undefined`), khó refactor code, khó khăn cho IDE trong việc cung cấp gợi ý code (intellisense) chính xác, khó làm việc nhóm trên codebase lớn.
      - **TypeScript là gì?** TypeScript là một **superset** (tập hợp cha) của JavaScript, được phát triển bởi Microsoft. Nó bổ sung **static typing** (kiểu tĩnh) tùy chọn và các tính năng hướng đối tượng dựa trên class vào JavaScript.
        - **Static typing:** Kiểu của biến, tham số hàm, giá trị trả về được kiểm tra tại thời điểm biên dịch (compile-time/transpile-time). Điều này giúp phát hiện lỗi sớm hơn nhiều.
        - **"TẠI SAO" TypeScript lại quan trọng?**
          1.  **Phát hiện lỗi sớm (Early Error Detection):** Lỗi kiểu được bắt ngay trong quá trình code hoặc biên dịch, trước khi deploy, giảm thiểu bug runtime.
          2.  **Tăng cường khảibility đọc và bảo trì (Readability & Maintainability):** Kiểu rõ ràng làm code dễ hiểu hơn, dễ dàng cho người mới tham gia dự án hoặc khi quay lại code cũ.
          3.  **Cải thiện trải nghiệm phát triển (Developer Experience - DX):** IDE (VS Code, WebStorm) cung cấp gợi ý code, tự động hoàn thành, và refactoring mạnh mẽ hơn nhờ thông tin kiểu.
          4.  **Hỗ trợ tốt hơn cho các dự án lớn và làm việc nhóm:** Giảm thiểu hiểu lầm về cấu trúc dữ liệu và API contracts giữa các thành viên.
          5.  **Tích hợp dần dần:** Có thể áp dụng TypeScript từ từ vào một dự án JavaScript hiện có.
      - **Lịch sử ngắn gọn:** TypeScript được công bố lần đầu vào tháng 10 năm 2012. Sự phát triển của nó được thúc đẩy bởi nhu cầu xây dựng các ứng dụng JavaScript quy mô lớn, nơi các vấn đề của dynamic typing trở nên rõ rệt. Anders Hejlsberg, kiến trúc sư trưởng của C# và người tạo ra Delphi và Turbo Pascal, là người dẫn dắt đội ngũ phát triển TypeScript.

    - **Hệ thống kiểu (Type System) của TypeScript:**

      - **Structural Typing (Kiểu cấu trúc):** Đây là một điểm rất quan trọng. TypeScript kiểm tra tính tương thích của kiểu dựa trên **cấu trúc** (hình dạng) của chúng, chứ không phải tên khai báo (nominal typing như trong Java hay C#). Nếu hai kiểu có cùng cấu trúc thành viên, chúng được coi là tương thích. Điều này còn được gọi là "duck typing" ("If it walks like a duck and quacks like a duck, then it must be a duck").
        - **"TẠI SAO" structural typing?** Nó phù hợp với bản chất linh hoạt của JavaScript và cho phép tích hợp dễ dàng hơn với các thư viện JavaScript hiện có.
      - **Type Inference (Suy luận kiểu):** TypeScript đủ thông minh để tự suy luận kiểu của biến nếu bạn không khai báo tường minh, dựa trên giá trị khởi tạo.
        - `let name = "Alice"; // TypeScript tự suy luận name là string`
        - **"KHI NÀO" nên để TS tự suy luận, "KHI NÀO" nên khai báo tường minh?**
          - Nên để tự suy luận cho các biến cục bộ đơn giản, nơi kiểu rõ ràng từ giá trị gán.
          - Nên khai báo tường minh cho tham số hàm, kiểu trả về của hàm, và các thuộc tính của object/class để làm rõ "contract" và giúp người đọc hiểu ý đồ.

    - **Các kiểu dữ liệu cơ bản (Primitive Types):**

      - `boolean`: `true` hoặc `false`.
      - `number`: Tất cả các số (nguyên, thực, `NaN`, `Infinity`). Không có `int`, `float` riêng biệt như các ngôn ngữ khác.
      - `string`: Chuỗi ký tự, có thể dùng dấu nháy đơn (`'`) hoặc kép (`"`), hoặc template literals (`` ` ``).
      - `null`: Đại diện cho việc một biến cố ý không có giá trị object.
      - `undefined`: Đại diện cho việc một biến đã được khai báo nhưng chưa được gán giá trị.
      - `symbol` (ES6): Kiểu dữ liệu để tạo các định danh duy nhất.
      - `bigint`: Cho các số nguyên lớn hơn `2^53 - 1`.

    - **Các kiểu dữ liệu phức tạp hơn:**

      - **`array`**: Mảng các phần tử cùng kiểu. Khai báo: `let list: number[] = [1, 2, 3];` hoặc `let list: Array<number> = [1, 2, 3];`
      - **`tuple`**: Mảng có số lượng phần tử cố định và kiểu của từng phần tử đã biết.
        - `let person: [string, number] = ["Alice", 30];`
        - **"TẠI SAO" dùng tuple?** Khi bạn muốn một mảng có cấu trúc cố định, ví dụ trả về nhiều giá trị từ hàm với kiểu cụ thể cho từng vị trí.
      - **`enum` (Enumeration):** Cách thân thiện để đặt tên cho một tập hợp các hằng số.
        - `enum Color { Red, Green, Blue }; let c: Color = Color.Green;`
        - Mặc định, enum gán giá trị số từ 0. Có thể tùy chỉnh giá trị.
        - **"TẠI SAO" dùng enum?** Tăng tính đọc hiểu cho code thay vì dùng magic numbers/strings.
      - **`any`**: "Kiểu thoát hiểm". Biến có kiểu `any` sẽ bỏ qua mọi kiểm tra kiểu của TypeScript.
        - **"KHI NÀO" dùng `any` (và tại sao nên hạn chế)?** Khi làm việc với code JavaScript cũ, thư viện bên thứ ba không có type definitions, hoặc khi bạn thực sự không biết kiểu dữ liệu là gì. Tuy nhiên, lạm dụng `any` làm mất đi lợi ích của TypeScript. Nó như "tắt" TypeScript cho biến đó.
      - **`unknown`**: Một giải pháp an toàn hơn cho `any`. Biến kiểu `unknown` có thể nhận bất kỳ giá trị nào, nhưng bạn không thể thực hiện thao tác gì trên nó (truy cập thuộc tính, gọi hàm) cho đến khi bạn thực hiện kiểm tra kiểu (type checking) hoặc ép kiểu (type assertion) một cách tường minh.
        - **"TẠI SAO" `unknown` an toàn hơn `any`?** Nó buộc bạn phải xử lý khả năng kiểu không xác định, thay vì bỏ qua như `any`.
      - **`void`**: Thường dùng cho kiểu trả về của hàm không trả về giá trị nào.
        - `function warnUser(): void { console.log("This is a warning"); }`
      - **`never`**: Đại diện cho kiểu của các giá trị không bao giờ xảy ra. Ví dụ: hàm luôn throw lỗi, hoặc vòng lặp vô hạn.
        - `function error(message: string): never { throw new Error(message); }`
        - **"TẠI SAO" `never`?** Hữu ích trong việc kiểm tra tính đầy đủ (exhaustiveness checking) trong các câu lệnh `switch` hoặc `if/else if` với union types.

    - **Type Assertion (Ép kiểu):**

      - Cách để báo cho trình biên dịch TypeScript biết "tin tôi đi, tôi biết kiểu của biến này là gì".
      - Cú pháp: `as` (ví dụ: `value as string`) hoặc angle-bracket (ví dụ: `<string>value`). Nên dùng `as` vì nó tương thích tốt hơn với JSX.
      - **"KHI NÀO" dùng?** Khi bạn có thông tin về kiểu mà TypeScript không thể tự suy luận, ví dụ khi làm việc với DOM elements hoặc dữ liệu từ API.
      - **Cảnh báo:** Type assertion không thực hiện bất kỳ kiểm tra hay chuyển đổi dữ liệu nào ở runtime. Nếu ép kiểu sai, lỗi vẫn xảy ra ở runtime. Nó chỉ là một chỉ dẫn cho trình biên dịch.

    - **Functions trong TypeScript:**

      - Khai báo kiểu cho tham số và kiểu trả về.
        `function add(x: number, y: number): number { return x + y; }`
      - Optional parameters (`?`): `function greet(name: string, title?: string): string { ... }`
      - Default parameters: `function greet(name: string, title: string = "Mr."): string { ... }`
      - Rest parameters: `function sum(...numbers: number[]): number { ... }`

    - **Interfaces:**

      - Một cách mạnh mẽ để định nghĩa "hợp đồng" (contract) cho cấu trúc của một object. Nó chỉ định các thuộc tính và phương thức mà một object phải có.
      - `interface User { id: number; name: string; email?: string; // Thuộc tính tùy chọn greet?(): void; // Phương thức tùy chọn }`
      - **"TẠI SAO" dùng interface?**
        1.  Định nghĩa hình dạng của object một cách rõ ràng.
        2.  Tạo ra các API contract giữa các phần của ứng dụng hoặc giữa client và server.
        3.  Tận dụng structural typing: Bất kỳ object nào có các thuộc tính và phương thức khớp với interface đều được coi là tương thích.
      - Có thể mở rộng (extend) interface khác.
      - Readonly properties: `readonly id: number;`

    - **Classes (Giới thiệu cơ bản):**

      - TypeScript hỗ trợ đầy đủ lập trình hướng đối tượng (OOP) với class, kế thừa, modifiers (public, private, protected), etc. Phần này chỉ giới thiệu cơ bản.
      - `class Person { name: string; age: number; constructor(name: string, age: number) { this.name = name; this.age = age; } greet(): void { console.log(`Hello, my name is ${this.name}`); } }`
      - `const john = new Person("John Doe", 30);`
      - Class cũng tạo ra một kiểu (type) trong TypeScript. `john` ở trên có kiểu là `Person`.

    - **Thiết lập dự án TypeScript và Transpilation:**
      - **Cài đặt TypeScript:** `npm install -g typescript` (global) hoặc `npm install --save-dev typescript` (project-local, khuyến khích).
      - **File `tsconfig.json`:** File cấu hình cho trình biên dịch TypeScript. Nó cho phép bạn tùy chỉnh nhiều tùy chọn biên dịch (target JavaScript version, module system, strict mode, source maps, etc.).
        - **"TẠI SAO" `tsconfig.json`?** Để quản lý nhất quán các tùy chọn biên dịch cho dự án, thay vì truyền tham số dòng lệnh mỗi lần.
        - Một số tùy chọn quan trọng ban đầu:
          - `target`: Phiên bản JavaScript output (e.g., "ES2015", "ES2020", "ESNext").
          - `module`: Hệ thống module sử dụng (e.g., "commonjs" cho Node.js, "es2015" cho frontend).
          - `outDir`: Thư mục chứa file JavaScript đã biên dịch.
          - `rootDir`: Thư mục chứa file TypeScript gốc.
          - `strict`: `true` - Bật tất cả các tùy chọn kiểm tra kiểu nghiêm ngặt. Rất khuyến khích!
      - **Biên dịch (Transpilation):** Lệnh `tsc`.
        - `tsc filename.ts` -> tạo `filename.js`.
        - `tsc` (không có tên file) -> biên dịch tất cả file trong project theo `tsconfig.json`.
        - `tsc -w` hoặc `tsc --watch`: Chạy ở chế độ watch, tự động biên dịch lại khi có thay đổi file.

5.  **Minh họa trực quan và Code mẫu:**

    - **Sơ đồ: Quy trình TypeScript sang JavaScript**

      ```
      +-------------------+     tsc (TypeScript     +-------------------+     Node.js / Browser     +-----------------+
      |  your-code.ts     | -----------------------> |  your-code.js     | ------------------------> |   Application   |
      | (Static Typing,   |      Compiler)         | (Plain JavaScript)  |      Execution          |   (Runtime)     |
      | ESNext features)  |                        |                     |                         |                 |
      +-------------------+                        +-------------------+                         +-----------------+
      ```

    - **Code mẫu: Thiết lập dự án cơ bản**

      1.  Tạo thư mục dự án: `mkdir ts-backend-intro && cd ts-backend-intro`
      2.  Khởi tạo `package.json`: `npm init -y`
      3.  Cài TypeScript: `npm install --save-dev typescript @types/node`
          - `@types/node`: Cung cấp type definitions cho Node.js built-in modules.
      4.  Tạo file `tsconfig.json`: `npx tsc --init` (hoặc tạo thủ công)
          Nội dung `tsconfig.json` tối thiểu (ví dụ cho backend Node.js):
          ```json
          {
            "compilerOptions": {
              "target": "ES2020", // Target modern Node.js versions
              "module": "commonjs", // Module system for Node.js
              "rootDir": "./src", // Source files directory
              "outDir": "./dist", // Output directory for compiled JS
              "esModuleInterop": true, // Allows default imports from CommonJS modules
              "strict": true, // Enable all strict type-checking options
              "skipLibCheck": true, // Skip type checking of declaration files
              "forceConsistentCasingInFileNames": true
            },
            "include": ["src/**/*"], // Which files to compile
            "exclude": ["node_modules", "**/*.spec.ts"] // Which files to exclude
          }
          ```
      5.  Tạo thư mục `src` và file `src/index.ts`:

          ```typescript
          // src/index.ts
          interface UserProfile {
            username: string;
            email: string;
            isActive: boolean;
            lastLogin?: Date; // Optional property
          }

          function createUser(profile: UserProfile): void {
            console.log(`Creating user: ${profile.username}`);
            if (profile.email) {
              console.log(`Email: ${profile.email}`);
            }
            // Giả sử ở đây có logic lưu user vào database
          }

          const newUser: UserProfile = {
            username: "coderPro",
            email: "coder.pro@example.com",
            isActive: true,
          };

          createUser(newUser);

          // Ví dụ về type inference
          let message = "Hello, TypeScript Backend!"; // TypeScript infers 'message' as string
          // message = 123; // Error: Type 'number' is not assignable to type 'string'.

          // Ví dụ về type assertion (sử dụng cẩn thận)
          let someValue: unknown = "this is a string";
          // let strLength: number = someValue.length; // Error: 'someValue' is of type 'unknown'.
          let strLength: number = (someValue as string).length;
          console.log(`Length of someValue: ${strLength}`);

          function processInput(input: string | number) {
            if (typeof input === "string") {
              console.log(`Input is a string: ${input.toUpperCase()}`);
            } else {
              console.log(`Input is a number: ${input.toFixed(2)}`);
            }
          }

          processInput("test");
          processInput(123.456);
          ```

      6.  Thêm script vào `package.json`:
          ```json
          "scripts": {
            "build": "tsc",
            "start": "node dist/index.js",
            "dev": "tsc -w & nodemon dist/index.js" // Cần cài nodemon: npm i -D nodemon
          },
          ```
      7.  Biên dịch: `npm run build`
      8.  Chạy: `npm start`

6.  **Best Practices và Quy ước (Conventions):**

    - **Bật `strict` mode trong `tsconfig.json`:** Đây là điều quan trọng nhất (`"strict": true`). Nó bao gồm các cờ như `noImplicitAny`, `strictNullChecks`, `strictFunctionTypes`, `strictPropertyInitialization`...
    - **Tránh `any` hết mức có thể.** Sử dụng `unknown` nếu kiểu thực sự không xác định và thực hiện kiểm tra kiểu.
    - **Khai báo kiểu tường minh cho tham số hàm và kiểu trả về:** Tăng tính rõ ràng và giúp bắt lỗi.
    - **Sử dụng `interface` để định nghĩa hình dạng của object và `type` alias cho các kiểu phức tạp khác (sẽ học ở phần sau).**
    - **Đặt tên (Naming Conventions):**
      - `PascalCase` cho Types, Interfaces, Classes, Enums (ví dụ: `UserProfile`, `ColorPalette`).
      - `camelCase` cho Variables, Functions, Properties (ví dụ: `userName`, `calculateTotal`).
    - **Tổ chức code:** Chia code thành các module nhỏ, dễ quản lý.
    - **Luôn biên dịch và kiểm tra trước khi commit/deploy.**

7.  **Anti-patterns và Lỗi thường gặp (Common Pitfalls):**

    - **Lạm dụng `any`:** Mất đi lợi ích của TypeScript. Dấu hiệu: Code có quá nhiều `any`.
    - **Hiểu lầm về Type Assertion:** Coi nó như một phép "ép kiểu" thực sự ở runtime. Nhớ rằng nó chỉ là gợi ý cho compiler, không thay đổi giá trị hay kiểm tra ở runtime.
      - `const myValue: any = "123"; const num = myValue as number; console.log(num * 2); // Runtime error: num là string "123", "123" * 2 = NaN`
    - **Quên biên dịch code TypeScript sang JavaScript:** Trình duyệt hoặc Node.js không hiểu trực tiếp file `.ts`.
    - **Không cấu hình `tsconfig.json` đúng cách:** Dẫn đến hành vi biên dịch không mong muốn.
    - **Nhầm lẫn `null` và `undefined`:** Dù JavaScript có sự khác biệt, trong TypeScript với `strictNullChecks`, cả hai đều cần được xử lý cẩn thận.
    - **Sử dụng `Object` hoặc `{}` làm kiểu:** Chúng quá lỏng lẻo. `Object` cho phép bất kỳ giá trị nào trừ `null` và `undefined`. `{}` cho phép bất kỳ giá trị nào không phải `null` hoặc `undefined` (nhưng không thể truy cập thuộc tính nào mà không có type assertion). Hãy dùng `Record<string, unknown>` hoặc định nghĩa `interface` cụ thể.

8.  **So sánh, Đánh giá và Lựa chọn:**

    - **TypeScript vs. JavaScript:**
      - **JS:** Dynamic typing, linh hoạt, dễ bắt đầu, cộng đồng lớn, chạy mọi nơi.
      - **TS:** Static typing (tùy chọn), an toàn hơn, công cụ tốt hơn, phù hợp dự án lớn, cần bước biên dịch.
      - **Lựa chọn:** Với backend và microservices, nơi sự ổn định, khả năng bảo trì và làm việc nhóm quan trọng, TypeScript thường là lựa chọn tốt hơn JavaScript thuần.
    - **`interface` vs. `type` (alias):**
      - Cả hai đều có thể dùng để định nghĩa hình dạng object.
      - **`interface`:** Có thể được `implements` bởi class, có thể được `extends` bởi interface khác. Có thể "declaration merging" (khai báo cùng tên nhiều lần sẽ được gộp lại).
      - **`type` alias:** Linh hoạt hơn, có thể định nghĩa union types, intersection types, tuples, mapped types, conditional types... Không thể declaration merging.
      - **Khi nào dùng cái nào?**
        - Dùng `interface` khi định nghĩa cấu trúc của object hoặc khi muốn tận dụng declaration merging, hoặc khi muốn class implement. Đây là quy ước phổ biến.
        - Dùng `type` cho các trường hợp phức tạp hơn mà `interface` không hỗ trợ (ví dụ: union, intersection).
        - Chúng ta sẽ tìm hiểu sâu hơn về `type` ở phần sau.
    - **`any` vs. `unknown`:**
      - **`any`**: Bỏ qua kiểm tra kiểu hoàn toàn. "Tôi không quan tâm kiểu là gì".
      - **`unknown`**: An toàn hơn. "Tôi không biết kiểu là gì, hãy kiểm tra trước khi dùng".
      - **Lựa chọn:** Luôn ưu tiên `unknown` hơn `any` khi kiểu không xác định.

9.  **Bài tập thực hành và Gợi ý dự án nhỏ:**

    - **Câu hỏi lý thuyết:**
      1.  Giải thích sự khác biệt chính giữa TypeScript và JavaScript. Lợi ích của việc sử dụng TypeScript trong một dự án backend là gì?
      2.  Structural typing là gì? Cho ví dụ.
      3.  Tại sao `strict: true` trong `tsconfig.json` lại quan trọng?
      4.  Phân biệt `any`, `unknown`, `void`, và `never`. Cho ví dụ sử dụng của từng loại.
      5.  Khi nào bạn nên sử dụng type assertion? Những rủi ro tiềm ẩn là gì?
    - **Bài tập gỡ lỗi (Debugging exercises):**
      Cung cấp một đoạn code TypeScript có lỗi kiểu, yêu cầu tìm và sửa:

      ```typescript
      // Gỡ lỗi đoạn code sau:
      function processData(data: string | number[]): string {
        if (typeof data === "string") {
          return data.toUpperCase; // Lỗi 1: toUpperCase là hàm
        } else {
          return data.join(", ").length; // Lỗi 2: length là number, hàm yêu cầu trả về string
        }
      }

      let myUser = { name: "Alice", age: "30" }; // Lỗi 3: age nên là number nếu muốn tính toán
      function printUserInfo(user: { name: string; age: number }) {
        console.log(`${user.name} is ${user.age} years old.`);
        // console.log(user.email); // Lỗi 4: email không tồn tại trong kiểu user
      }
      // printUserInfo(myUser);
      ```

    - **Thử thách code (Coding challenges):**
      1.  Viết một hàm `calculateTotalPrice` nhận vào một mảng các `Product` (mỗi `Product` có `name: string`, `price: number`, `quantity: number`) và trả về tổng giá trị của tất cả sản phẩm.
      2.  Định nghĩa một `interface` tên là `Vehicle` với các thuộc tính: `make: string`, `model: string`, `year: number`, và một phương thức tùy chọn `startEngine(): void`. Tạo một object `myCar` tuân theo `interface` này.
      3.  Viết một hàm `formatDate` nhận vào một `Date` object hoặc một string (định dạng "YYYY-MM-DD") và trả về một string đã được định dạng là "DD/MM/YYYY". Sử dụng type guards để xử lý các kiểu đầu vào khác nhau.
    - **Gợi ý dự án nhỏ:**
      - **Quản lý công việc cơ bản (CLI):** Tạo một ứng dụng dòng lệnh đơn giản để thêm, xem, và đánh dấu hoàn thành công việc. Sử dụng `interface` để định nghĩa `Task` object (e.g., `id: number`, `title: string`, `completed: boolean`, `dueDate?: Date`).

10. **Câu hỏi phỏng vấn thường gặp:**
    - "What are the main advantages of using TypeScript over JavaScript for backend development?"
    - "Explain the concept of static typing and how it helps in large-scale applications."
    - "What is `any` type in TypeScript and why should it be avoided if possible? What's a safer alternative?"
    - "Can you describe what an `interface` is in TypeScript and provide a use case?"
    - "What does the `tsconfig.json` file do? Mention a few important compiler options."
    - "How does TypeScript's structural typing (or 'duck typing') work?"
    - "What is type inference? When should you rely on it, and when should you provide explicit types?"

---

Hãy dành thời gian nghiền ngẫm và thực hành kỹ lưỡng phần này. Nền tảng vững chắc là chìa khóa để làm chủ các khái niệm phức tạp hơn sau này. Khi bạn đã sẵn sàng, chúng ta sẽ chuyển sang Phần 2.

Chắc chắn rồi! Việc thực hành với nhiều ví dụ và nhận diện lỗi thường gặp là cực kỳ quan trọng để hiểu sâu TypeScript. Chúng ta sẽ tiếp tục với Phần 2, tập trung vào các kiểu nâng cao hơn, nơi sự phức tạp và sức mạnh của TypeScript thực sự tỏa sáng.

---

### **PHẦN 2: TypeScript Nâng Cao - Làm Chủ Hệ Thống Kiểu Phức Tạp**

1.  **Tên phần học:** TypeScript Nâng Cao - Làm Chủ Hệ Thống Kiểu Phức Tạp.

2.  **Mục tiêu học phần:**

    - Nắm vững và sử dụng thành thạo `Type Aliases` cho các kiểu phức tạp và tái sử dụng.
    - Hiểu và áp dụng `Union Types` để cho phép một biến có thể thuộc một trong nhiều kiểu.
    - Hiểu và áp dụng `Intersection Types` để kết hợp nhiều kiểu thành một.
    - Làm chủ `Literal Types` để giới hạn giá trị của biến vào một tập hợp cụ thể.
    - Sử dụng các `Utility Types` tích hợp sẵn của TypeScript để biến đổi kiểu một cách hiệu quả (ví dụ: `Partial`, `Readonly`, `Pick`, `Omit`, `Record`).
    - Hiểu khái niệm `Generics` và cách sử dụng chúng để viết code tái sử dụng và an toàn kiểu cho hàm, class, và interface.
    - Biết cách sử dụng `keyof` và `typeof` để làm việc với kiểu một cách linh hoạt hơn.
    - Làm quen với `Mapped Types` để tạo kiểu mới dựa trên cấu trúc của kiểu hiện có.
    - Hiểu sơ lược về `Conditional Types` (sẽ đào sâu hơn ở các phần nâng cao hơn nữa).
    - Trả lời được:
      - Khi nào nên dùng `type` alias thay vì `interface` và ngược lại?
      - Làm thế nào để mô hình hóa dữ liệu có thể có nhiều hình dạng khác nhau bằng Union Types?
      - Generics giải quyết vấn đề gì và làm thế nào để tạo một hàm generic?
      - Các Utility Types phổ biến dùng để làm gì và khi nào nên dùng chúng?
      - Sự khác biệt giữa `keyof T` và `keyof typeof T`?

3.  **Kiến thức tiên quyết:**

    - Hoàn thành Phần 1: Nắm vững TypeScript Cơ bản (kiểu dữ liệu, interface, class cơ bản, hàm).
    - Hiểu biết về JavaScript ES6+ (đặc biệt là arrow functions, destructuring).

4.  **Giải thích lý thuyết chuyên sâu:**

    - **Type Aliases (Bí danh kiểu):**

      - Dùng từ khóa `type` để tạo tên mới (bí danh) cho một kiểu.
      - **"TẠI SAO" dùng Type Aliases?**
        1.  **Tái sử dụng:** Định nghĩa một kiểu phức tạp một lần và sử dụng lại ở nhiều nơi.
        2.  **Tăng tính đọc hiểu:** Đặt tên có ý nghĩa cho các kiểu phức tạp (ví dụ: `type UserID = string | number;`).
        3.  **Định nghĩa Union, Intersection, Tuple, và các kiểu phức tạp khác** mà `interface` không thể hoặc khó diễn đạt.
      - **Ví dụ:**

        ```typescript
        type Point = {
          x: number;
          y: number;
        };

        type ID = string | number; // Union type

        function printCoord(pt: Point) {
          console.log("The coordinate's x value is " + pt.x);
          console.log("The coordinate's y value is " + pt.y);
        }
        printCoord({ x: 100, y: 200 });

        let userId: ID = "user-123";
        userId = 456; // Hợp lệ
        // userId = true; // Lỗi: Type 'boolean' is not assignable to type 'ID'.
        ```

      - **So sánh `type` và `interface` (chi tiết hơn):**
        | Tính năng | `interface` | `type` alias |
        |-----------------------|----------------------------------------------|---------------------------------------------------------|
        | Định nghĩa object | Có (chính) | Có |
        | Định nghĩa primitives | Không | Có (ví dụ: `type MyString = string;`) |
        | Định nghĩa union | Không trực tiếp (cần dùng class implements) | Có |
        | Định nghĩa intersection| Có (dùng `extends` hoặc `&` với type khác) | Có (dùng `&`) |
        | Định nghĩa tuple | Có (nhưng `type` thường rõ ràng hơn) | Có |
        | Extends/Implements | `extends` (interface), `implements` (class) | Không `extends` trực tiếp, dùng intersection (`&`) |
        | Declaration Merging | Có | Không (phải là duy nhất) |
        - **Declaration Merging (với interface):** Nếu bạn khai báo cùng một interface nhiều lần, TypeScript sẽ gộp các khai báo đó lại.
          ```typescript
          interface Box {
            height: number;
            width: number;
          }
          interface Box {
            // Merged
            scale: number;
          }
          let box: Box = { height: 5, width: 6, scale: 10 };
          ```
        - **"KHI NÀO" chọn cái nào?**
          - **Ưu tiên `interface` khi định nghĩa hình dạng của object hoặc khi bạn muốn khả năng declaration merging.** Đây là quy ước phổ biến trong cộng đồng.
          - **Sử dụng `type` khi bạn cần union, intersection, tuple, hoặc muốn đặt bí danh cho primitive types, hoặc các kiểu phức tạp hơn mà `interface` không dễ diễn tả.**

    - **Union Types (Kiểu hợp `|`):**

      - Cho phép một giá trị có thể là một trong nhiều kiểu khác nhau.
      - **"TẠI SAO" Union Types?** Để mô hình hóa dữ liệu có thể có nhiều biến thể hợp lệ. Ví dụ: ID có thể là số hoặc chuỗi, tham số hàm có thể nhận nhiều kiểu đầu vào.
      - **Ví dụ:**
        ```typescript
        function printId(id: number | string) {
          // console.log(id.toUpperCase()); // Lỗi: Property 'toUpperCase' does not exist on type 'string | number'. Property 'toUpperCase' does not exist on type 'number'.
          if (typeof id === "string") {
            // Bây giờ TypeScript biết id là string trong block này
            console.log(id.toUpperCase());
          } else {
            // Bây giờ TypeScript biết id là number
            console.log(id.toFixed(2));
          }
        }
        printId(101); // Output: 101.00
        printId("202-abc"); // Output: 202-ABC
        // printId({ message: "Hello" }); // Lỗi: Argument of type '{ message: string; }' is not assignable to parameter of type 'string | number'.
        ```
      - **Type Guards (Bộ bảo vệ kiểu):** Để làm việc an toàn với union types, bạn cần sử dụng type guards như `typeof`, `instanceof`, `in`, hoặc custom type guards để thu hẹp (narrow down) kiểu trong một phạm vi nhất định.

    - **Intersection Types (Kiểu giao `&`):**

      - Kết hợp nhiều kiểu thành một kiểu mới có tất cả các thành viên của các kiểu gốc.
      - **"TẠI SAO" Intersection Types?** Để tạo ra các kiểu phức tạp bằng cách gộp các tính năng từ nhiều kiểu khác nhau (mixins, composition).
      - **Ví dụ:**

        ```typescript
        interface Draggable {
          drag: () => void;
        }

        interface Resizable {
          resize: () => void;
        }

        type UIElement = Draggable & Resizable; // UIElement phải có cả drag và resize

        let element: UIElement = {
          drag: () => console.log("Dragging..."),
          resize: () => console.log("Resizing..."),
        };
        element.drag();
        element.resize();

        // Lỗi nếu thiếu
        // let incompleteElement: UIElement = {
        //   drag: () => console.log("Dragging...") // Lỗi: Property 'resize' is missing in type '{ drag: () => void; }' but required in type 'Resizable'.
        // };
        ```

      - **Lưu ý với kiểu primitive:** `string & number` sẽ trở thành `never` vì không có giá trị nào vừa là string vừa là number.

    - **Literal Types (Kiểu chữ):**

      - Cho phép bạn chỉ định các giá trị cụ thể mà một biến có thể có.
      - Thường được sử dụng kết hợp với union types.
      - **"TẠI SAO" Literal Types?** Để ràng buộc giá trị một cách chặt chẽ hơn, ví dụ: các trạng thái, các lựa chọn cố định.
      - **Ví dụ:**

        ```typescript
        type Alignment = "left" | "right" | "center";
        let textAlign: Alignment = "left";
        // textAlign = "justify"; // Lỗi: Type '"justify"' is not assignable to type 'Alignment'.

        type DiceRoll = 1 | 2 | 3 | 4 | 5 | 6;
        let roll: DiceRoll = 3;
        // roll = 7; // Lỗi

        function setAlignment(align: Alignment): void {
          console.log(`Setting alignment to: ${align}`);
        }
        setAlignment("center");
        ```

    - **Generics (Kiểu tổng quát):**

      - Cho phép viết code có thể làm việc với nhiều kiểu dữ liệu khác nhau mà vẫn đảm bảo an toàn kiểu. Hoạt động như "biến" cho kiểu.
      - **"TẠI SAO" Generics?**
        1.  **Tái sử dụng code:** Viết một hàm/class/interface một lần và dùng cho nhiều kiểu.
        2.  **An toàn kiểu:** Giữ lại thông tin kiểu, tránh dùng `any` làm mất an toàn.
      - **Ví dụ hàm generic:**

        ```typescript
        // Hàm không generic, dùng any (không an toàn)
        function identityAny(arg: any): any {
          return arg;
        }
        let outputAny = identityAny("myString"); // outputAny là 'any'
        // let num: number = outputAny; // Không có lỗi compile time, nhưng có thể sai logic

        // Hàm generic
        function identity<T>(arg: T): T {
          // T là một type parameter (tham số kiểu)
          return arg;
        }

        let outputString = identity<string>("myString"); // T được gán là string
        let outputNumber = identity<number>(123); // T được gán là number
        // Hoặc TypeScript tự suy luận kiểu (type argument inference)
        let outputBool = identity(true); // T được suy luận là boolean

        // outputString.toFixed(); // Lỗi: Property 'toFixed' does not exist on type 'string'.
        // outputNumber.toUpperCase(); // Lỗi: Property 'toUpperCase' does not exist on type 'number'.
        ```

      - **Ví dụ interface generic:**
        ```typescript
        interface Box<T> {
          contents: T;
        }
        let stringBox: Box<string> = { contents: "hello" };
        let numberBox: Box<number> = { contents: 100 };
        ```
      - **Ví dụ class generic:**

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
        const stringStore = new DataStorage<string>();
        stringStore.addItem("Apple");
        // stringStore.addItem(123); // Lỗi: Argument of type 'number' is not assignable to parameter of type 'string'.
        console.log(stringStore.getItem(0)?.toUpperCase()); // APPLE

        const numberStore = new DataStorage<number>();
        numberStore.addItem(10);
        console.log(numberStore.getItem(0)?.toFixed(2)); // 10.00
        ```

      - **Generic Constraints (Ràng buộc Generic):**
        Đôi khi bạn muốn giới hạn các kiểu mà `T` có thể nhận.

        ```typescript
        interface Lengthwise {
          length: number;
        }

        function loggingIdentity<T extends Lengthwise>(arg: T): T {
          console.log(arg.length); // Giờ đây an toàn để truy cập .length
          return arg;
        }
        loggingIdentity({ length: 10, value: 3 }); // OK
        loggingIdentity("hello"); // OK, string có thuộc tính length
        // loggingIdentity(3); // Lỗi: Argument of type 'number' is not assignable to parameter of type 'Lengthwise'.
        ```

    - **Utility Types (Kiểu tiện ích):**

      - TypeScript cung cấp sẵn một số kiểu tiện ích để thực hiện các phép biến đổi kiểu phổ biến.
      - **`Partial<T>`:** Tạo một kiểu mới với tất cả các thuộc tính của `T` đều là tùy chọn (`optional`).

        ```typescript
        interface Todo {
          title: string;
          description: string;
          completed: boolean;
        }

        function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>): Todo {
          return { ...todo, ...fieldsToUpdate };
        }

        const myTodo: Todo = {
          title: "Learn TS",
          description: "Deep dive",
          completed: false,
        };
        const updatedTodo = updateTodo(myTodo, {
          description: "Master TS Advanced Types",
        });
        // const invalidUpdate = updateTodo(myTodo, { nonExistentField: 1 }); // Lỗi
        console.log(updatedTodo);
        // { title: 'Learn TS', description: 'Master TS Advanced Types', completed: false }
        ```

      - **`Readonly<T>`:** Tạo một kiểu mới với tất cả các thuộc tính của `T` đều là chỉ đọc (`readonly`).
        ```typescript
        interface UserConfig {
          apiKey: string;
          theme: string;
        }
        const config: Readonly<UserConfig> = {
          apiKey: "xyz123",
          theme: "dark",
        };
        // config.apiKey = "abc456"; // Lỗi: Cannot assign to 'apiKey' because it is a read-only property.
        ```
      - **`Pick<T, K>`:** Tạo một kiểu mới bằng cách chọn một tập hợp các thuộc tính `K` từ kiểu `T`.
        ```typescript
        interface Product {
          id: string;
          name: string;
          price: number;
          inStock: boolean;
        }
        type ProductPreview = Pick<Product, "id" | "name" | "price">;
        const preview: ProductPreview = {
          id: "p1",
          name: "Laptop",
          price: 1200,
        };
        // const invalidPreview: ProductPreview = { id: "p1", name: "Laptop", price: 1200, inStock: true }; // Lỗi: Object literal may only specify known properties, and 'inStock' does not exist in type 'ProductPreview'.
        ```
      - **`Omit<T, K>`:** Tạo một kiểu mới bằng cách loại bỏ một tập hợp các thuộc tính `K` khỏi kiểu `T`.
        ```typescript
        interface UserDetails {
          id: number;
          username: string;
          passwordHash: string;
          email: string;
          createdAt: Date;
        }
        // Thông tin user có thể public, bỏ passwordHash
        type PublicUser = Omit<UserDetails, "passwordHash" | "createdAt">;
        const publicProfile: PublicUser = {
          id: 1,
          username: "testuser",
          email: "test@example.com",
        };
        ```
      - **`Record<K, T>`:** Tạo một kiểu object với tập hợp các key `K` và mỗi key có giá trị kiểu `T`. `K` thường là `string | number | symbol`.

        ```typescript
        type FeatureFlags = Record<string, boolean>;
        const flags: FeatureFlags = {
          darkMode: true,
          newEditor: false,
          // experimentalFeature: "enabled" // Lỗi: Type 'string' is not assignable to type 'boolean'.
        };

        interface PageInfo {
          title: string;
          analyticsName: string;
        }
        type Pages = "home" | "about" | "contact";
        const navigationInfo: Record<Pages, PageInfo> = {
          home: { title: "Home Page", analyticsName: "home_view" },
          about: { title: "About Us", analyticsName: "about_view" },
          contact: { title: "Contact", analyticsName: "contact_view" },
        };
        ```

      - Còn nhiều Utility Types khác như `Exclude<T, U>`, `Extract<T, U>`, `NonNullable<T>`, `ReturnType<T>`, `Parameters<T>`, `Required<T>`... chúng ta sẽ khám phá dần.

    - **`typeof` Type Operator:**

      - Lấy kiểu của một biến hoặc một thuộc tính.
      - **"KHI NÀO" dùng `typeof` (trong context kiểu)?** Khi bạn muốn tạo một kiểu mới dựa trên kiểu của một giá trị đã tồn tại mà không cần định nghĩa lại.
      - ```typescript
        let s = "hello";
        let n: typeof s; // n có kiểu string
        // n = 100; // Lỗi

        const person = { name: "Alice", age: 30 };
        type PersonType = typeof person; // PersonType là { name: string; age: number; }
        const anotherPerson: PersonType = { name: "Bob", age: 40 };

        function greet(message: typeof person.name) {
          // message phải là string
          console.log(`Greeting: ${message}`);
        }
        greet("Hello there");
        // greet(123); // Lỗi
        ```

    - **`keyof` Type Operator:**

      - Lấy ra một union type của các key (tên thuộc tính) của một object type.
      - **"KHI NÀO" dùng `keyof`?** Khi bạn muốn làm việc với tên các thuộc tính của một kiểu một cách an toàn, ví dụ như trong các hàm generic thao tác trên thuộc tính của object.
      - ```typescript
        interface Point3D {
          x: number;
          y: number;
          z: number;
        }
        type PointKeys = keyof Point3D; // PointKeys là "x" | "y" | "z"
        let key: PointKeys = "x";
        // key = "a"; // Lỗi: Type '"a"' is not assignable to type 'keyof Point3D'.

        function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
          return obj[key];
        }
        const myPoint: Point3D = { x: 1, y: 2, z: 3 };
        const xValue = getProperty(myPoint, "x"); // xValue có kiểu number
        // const wValue = getProperty(myPoint, "w"); // Lỗi: Argument of type '"w"' is not assignable to parameter of type '"x" | "y" | "z"'.
        ```

      - **Indexed Access Types (Truy cập kiểu theo chỉ mục `T[K]`):** `T[K]` trong ví dụ trên là một indexed access type, nó lấy kiểu của thuộc tính `K` trong kiểu `T`.

    - **Mapped Types:**

      - Tạo kiểu mới bằng cách lặp qua các thuộc tính của một kiểu hiện có và biến đổi chúng.
      - Cú pháp tương tự như `for...in` cho object.
      - **"TẠI SAO" Mapped Types?** Để tạo các biến thể của một kiểu hiện có một cách linh hoạt (ví dụ: làm tất cả thuộc tính thành readonly, optional, hoặc thay đổi kiểu của chúng). `Partial`, `Readonly`, `Pick`, `Record` thực ra được triển khai bằng Mapped Types.
      - **Ví dụ cơ bản:**

        ```typescript
        type MyReadonly<T> = {
          readonly [P in keyof T]: T[P];
        };
        interface Config {
          host: string;
          port: number;
        }
        type ReadonlyConfig = MyReadonly<Config>;
        // ReadonlyConfig tương đương với: { readonly host: string; readonly port: number; }

        type MyPartial<T> = {
          [P in keyof T]?: T[P];
        };
        type PartialConfig = MyPartial<Config>;
        // PartialConfig tương đương với: { host?: string; port?: number; }

        // Biến đổi kiểu của thuộc tính
        type StringifyProperties<T> = {
          [P in keyof T]: string;
        };
        type StringifiedConfig = StringifyProperties<Config>;
        // StringifiedConfig là: { host: string; port: string; }
        const stringConfig: StringifiedConfig = {
          host: "localhost",
          port: "8080",
        };
        ```

    - **Conditional Types (Sơ lược):**

      - Cho phép chọn một trong hai kiểu dựa trên một điều kiện kiểu.
      - Cú pháp: `SomeType extends OtherType ? TrueType : FalseType;`
      - **"TẠI SAO" Conditional Types?** Để tạo ra các kiểu rất linh hoạt, phụ thuộc vào các mối quan hệ giữa các kiểu khác. Đây là một công cụ rất mạnh mẽ, thường dùng trong các thư viện hoặc typing nâng cao.
      - **Ví dụ đơn giản:**

        ```typescript
        type IsString<T> = T extends string
          ? "Yes, it's a string"
          : "No, not a string";

        type Result1 = IsString<string>; // "Yes, it's a string"
        type Result2 = IsString<number>; // "No, not a string"
        ```

        Chúng ta sẽ đào sâu hơn về Conditional Types và các ứng dụng phức tạp của nó (như `infer` keyword) ở các phần sau.

5.  **Minh họa trực quan và Code mẫu (Đã tích hợp trong phần Giải thích lý thuyết)**

    - **Sơ đồ: Mối quan hệ giữa các kiểu nâng cao**

      ```mermaid
      graph TD
          A[Base Type / Interface] -->|Union `T | U`| B(Can be T or U)
          A -->|Intersection `T & U`| C(Must be T and U)
          A -->|`keyof A`| D(Union of Keys of A)
          A -->|Generic `MyType<A>`| E(Type parameterized by A)
          A -->|Mapped Type `{[P in keyof A]: ...}`| F(New type based on A's props)
          A -->|Utility `Partial<A>`| G(All props of A optional)

          B --> X{Type Guard e.g. `typeof`}
          X -- True --> B_T(T specific logic)
          X -- False --> B_U(U specific logic)

          D --> K[Specific Key]
          A & K --> T_K(Type of A[K] - Indexed Access)

          subgraph AdvancedTypeConstructs
              B
              C
              D
              E
              F
              G
          end
      ```

6.  **Best Practices và Quy ước (Conventions):**

    - **Ưu tiên `interface` cho việc định nghĩa hình dạng object, `type` cho union, intersection, và các alias phức tạp.**
    - **Sử dụng Generics để tăng tính tái sử dụng và an toàn kiểu, tránh `any`.**
    - **Đặt tên rõ ràng cho Type Parameters trong Generics (thường là `T`, `U`, `K`, `V`, `P` hoặc các tên mô tả hơn như `TItem`, `TKey`).**
    - **Tận dụng Utility Types để tránh viết lại logic biến đổi kiểu phổ biến.**
    - **Khi làm việc với Union Types, luôn sử dụng Type Guards để thu hẹp kiểu trước khi truy cập các thuộc tính/phương thức đặc thù của một kiểu con.**
    - **Sử dụng `as const` (const assertions) với literal types để TypeScript suy luận kiểu hẹp nhất có thể.**

      ```typescript
      // Không có as const
      let httpMethod = "GET"; // Type is string
      // Có as const
      let httpMethodConst = "GET" as const; // Type is "GET" (literal type)

      const req = { url: "/api/data", method: "POST" } as const;
      // req.method bây giờ là "POST" (literal) chứ không phải string
      // req.method = "GET"; // Lỗi: Cannot assign to 'method' because it is a read-only property.
      // type: { readonly url: "/api/data"; readonly method: "POST"; }
      ```

7.  **Anti-patterns và Lỗi thường gặp (Common Pitfalls):**

    - **Lạm dụng Union Types mà không có Type Guards:** Dẫn đến lỗi runtime hoặc code phức tạp khi cố gắng truy cập thuộc tính không tồn tại trên tất cả các thành viên của union.
      ```typescript
      type Shape =
        | { kind: "circle"; radius: number }
        | { kind: "square"; sideLength: number };
      function getArea(shape: Shape) {
        // return Math.PI * shape.radius * shape.radius; // Lỗi: Property 'radius' does not exist on type 'Shape'. Property 'radius' does not exist on type '{ kind: "square"; sideLength: number; }'.
        if (shape.kind === "circle") {
          return Math.PI * shape.radius * shape.radius; // OK
        }
        // TypeScript biết nếu không phải circle thì là square
        // return shape.sideLength * shape.sideLength; // OK (nhưng cần else hoặc return sau if)
      }
      ```
    - **Nhầm lẫn Intersection Types với Union Types:** `A & B` nghĩa là _phải có cả A và B_, trong khi `A | B` nghĩa là _có thể là A hoặc B_.
    - **Sử dụng `any` trong Generics:** Làm mất đi mục đích của Generics.
      ```typescript
      // Anti-pattern
      function badGeneric<T>(item: any): T {
        return item as T; // Không an toàn, không có kiểm tra thực sự
      }
      // let myNum: number = badGeneric<number>("this is a string"); // Không lỗi compile, nhưng runtime sẽ sai.
      ```
    - **Quên ràng buộc (constraints) cho Generics khi cần:** Dẫn đến lỗi khi cố truy cập thuộc tính không tồn tại trên type parameter.
      ```typescript
      function getLength<T>(arr: T): number {
        // return arr.length; // Lỗi: Property 'length' does not exist on type 'T'.
        return 0;
      }
      // Cần: function getLength<T extends { length: number }>(arr: T): number
      ```
    - **Quá phức tạp hóa kiểu:** Đôi khi một kiểu đơn giản hơn cũng đủ. Cố gắng tạo ra các kiểu "hoàn hảo" có thể dẫn đến code khó đọc và khó bảo trì. Cần cân bằng.
    - **Không hiểu rõ `keyof typeof`:**
      ```typescript
      const AppConfig = {
        API_URL: "https://api.example.com",
        TIMEOUT: 5000,
      };
      // type ConfigKeys = keyof AppConfig; // Lỗi: 'AppConfig' refers to a value, but is being used as a type here. Did you mean 'typeof AppConfig'?
      type ConfigKeys = keyof typeof AppConfig; // Đúng: "API_URL" | "TIMEOUT"
      ```
      `AppConfig` là một giá trị (object). `typeof AppConfig` là kiểu của object đó. `keyof` hoạt động trên kiểu.

8.  **So sánh, Đánh giá và Lựa chọn:**

    - **`Pick` vs. `Omit`:**
      - `Pick<T, K>`: Chọn những gì bạn muốn. Tốt khi bạn chỉ cần một vài thuộc tính.
      - `Omit<T, K>`: Loại bỏ những gì bạn không muốn. Tốt khi bạn muốn hầu hết các thuộc tính, trừ một vài.
      - Lựa chọn dựa trên sự rõ ràng và số lượng thuộc tính cần xử lý.
    - **Generics với Type Parameters vs. Union Types:**
      - **Generics:** Khi bạn muốn một hàm/class/interface hoạt động với _nhiều kiểu khác nhau_ nhưng vẫn giữ được _mối quan hệ kiểu_ giữa đầu vào và đầu ra, hoặc giữa các thành viên. Ví dụ: `identity<T>(arg: T): T` đảm bảo kiểu trả về giống kiểu đầu vào.
      - **Union Types:** Khi một giá trị có thể là _một trong một tập hợp các kiểu cố định_. Ví dụ: `string | number`.
      - Không phải lúc nào cũng thay thế cho nhau. `function process(input: T): T` khác với `function process(input: string | number): string | number`.

9.  **Bài tập thực hành và Gợi ý dự án nhỏ:**

    - **Câu hỏi lý thuyết:**
      1.  Giải thích sự khác biệt giữa `type` alias và `interface`. Khi nào bạn chọn sử dụng cái nào? Cho ví dụ.
      2.  Generics là gì? Chúng giải quyết vấn đề gì trong TypeScript? Cho một ví dụ về hàm generic bạn tự viết.
      3.  Sự khác biệt giữa `Pick<T, K>` và `Omit<T, K>` là gì? Cho ví dụ.
      4.  Literal types là gì và tại sao chúng hữu ích?
      5.  `keyof T` trả về gì? Nó thường được sử dụng kết hợp với toán tử nào khác để truy cập kiểu của thuộc tính?
    - **Bài tập gỡ lỗi (Debugging exercises):**

      ```typescript
      // Bài 1: Sửa lỗi để hàm hoạt động đúng với cả string và number array
      /*
      function processCollection<T>(collection: T): void {
        if (typeof collection === "string") {
          console.log(collection.toUpperCase());
        } else if (Array.isArray(collection)) {
          // Giả sử collection là mảng các number nếu là array
          console.log("Sum:", collection.reduce((sum, item) => sum + item, 0)); // Lỗi: 'item' is implicitly 'any' because its type cannot be inferred.
                                                                               // Lỗi: Operator '+' cannot be applied to types 'any' and 'any'.
        }
      }
      processCollection("hello");
      processCollection([1,2,3]);
      */

      // Bài 2: Sửa lỗi kiểu trong Mapped Type
      /*
      interface UserProfile {
        id: number;
        username: string;
        email: string;
        isAdmin: boolean;
      }
      // Yêu cầu: Tạo một mapped type OptionalUserProfile làm tất cả thuộc tính thành optional và giữ nguyên kiểu
      type OptionalUserProfile<T> = {
        [P in keyof T]?: P; // Lỗi: Type 'P' is not assignable to type 'T[P]'.
      };
      type TestOptional = OptionalUserProfile<UserProfile>;
      // const partialUser: TestOptional = { username: "test" }; // Mong muốn: username là string, không phải "username"
      */
      ```

    - **Thử thách code (Coding challenges):**
      1.  Viết một `type` alias `ApiResponse<T>` đại diện cho cấu trúc phản hồi API. Nó nên là một object có:
          - `data: T` (dữ liệu chính, kiểu generic)
          - `status: "success" | "error"`
          - `message?: string` (tùy chọn)
          - Nếu `status` là "error", thì `data` có thể là `null` và phải có `errorCode: number`. (Gợi ý: sử dụng discriminated union).
      2.  Viết một hàm generic `mergeObjects<T, U>(obj1: T, obj2: U): T & U` nhận vào hai object và trả về một object mới là sự kết hợp của cả hai.
      3.  Sử dụng `Pick` và `Omit` để tạo kiểu `UserSummary` từ `interface User { id: number; name: string; email: string; passwordHash: string; lastLogin: Date; }`. `UserSummary` chỉ nên chứa `id`, `name`, `email`.
      4.  Viết một Mapped Type `Getters<T>` để tạo ra một kiểu mới từ `T`. Với mỗi thuộc tính `P` trong `T`, kiểu mới sẽ có một phương thức `getP(): T[P]`. Ví dụ, nếu `T` là `{ name: string; age: number; }`, thì `Getters<T>` sẽ là `{ getName: () => string; getAge: () => number; }`.
    - **Gợi ý dự án nhỏ (tiếp tục dự án Quản lý công việc CLI):**
      - Mở rộng `Task` interface/type:
        - Thêm `priority: "low" | "medium" | "high"`.
        - Thêm `tags?: string[]`.
      - Viết hàm `updateTask(taskId: number, updates: Partial<Omit<Task, "id">>): Task | undefined` để cập nhật một task. Hàm này chỉ cho phép cập nhật các trường ngoại trừ `id`.
      - Viết hàm `filterTasks<K extends keyof Task>(tasks: Task[], criteriaKey: K, criteriaValue: Task[K]): Task[]` để lọc task dựa trên một thuộc tính và giá trị cụ thể.

10. **Câu hỏi phỏng vấn thường gặp:**
    - "What are Generics in TypeScript and why are they useful? Provide an example."
    - "Explain the difference between Union types and Intersection types."
    - "What are some common Utility Types in TypeScript (e.g., `Partial`, `Pick`, `Readonly`) and what do they do?"
    - "How would you define a type that can be either a string, a number, or a specific object structure?"
    - "What is a Mapped Type? Can you give an example of how you might use one?"
    - "Explain `keyof` and `typeof` operators in TypeScript."
    - "How do you ensure type safety when working with Union Types?" (Hỏi về Type Guards).
    - "When would you prefer a `type` alias over an `interface`?"
    - "Can you create a type that makes all properties of an existing type optional but also adds a new required property?" (Kết hợp Mapped Types/Utility Types và Intersection).

---

Phần này khá nặng về lý thuyết kiểu, hãy chắc chắn bạn thực hành nhiều với các ví dụ code. Việc hiểu rõ cách các kiểu này hoạt động và tương tác với nhau là nền tảng để viết code TypeScript hiệu quả và an toàn cho backend. Sẵn sàng cho phần tiếp theo khi bạn cảm thấy thoải mái nhé!

Tuyệt vời! Chúng ta đã đi qua những phần khá "khô khan" nhưng cực kỳ quan trọng của hệ thống kiểu TypeScript. Bây giờ, hãy chuyển sang một chủ đề quen thuộc hơn với bạn với tư cách là một fullstack developer, nhưng nhìn nó dưới lăng kính của TypeScript: **Lập trình hướng đối tượng (OOP)**.

---

### **PHẦN 3: Lập Trình Hướng Đối Tượng (OOP) với TypeScript - Thiết Kế Backend Linh Hoạt**

1.  **Tên phần học:** Lập Trình Hướng Đối Tượng (OOP) với TypeScript - Thiết Kế Backend Linh Hoạt.

2.  **Mục tiêu học phần:**

    - Nắm vững cách khai báo và sử dụng `class` trong TypeScript, bao gồm `constructor`, thuộc tính (properties), và phương thức (methods).
    - Hiểu và áp dụng các access modifiers: `public`, `private`, `protected`.
    - Sử dụng thành thạo `readonly` modifier cho thuộc tính.
    - Triển khai kế thừa (inheritance) giữa các class (`extends`).
    - Hiểu và sử dụng `super()` để gọi constructor và phương thức của class cha.
    - Ghi đè phương thức (method overriding) và khái niệm đa hình (polymorphism) cơ bản.
    - Làm quen với `static` members (properties và methods).
    - Hiểu và áp dụng `abstract class` và `abstract method` để định nghĩa khuôn mẫu cho các class con.
    - Biết cách class `implements` một `interface` để đảm bảo tuân thủ contract.
    - Hiểu rõ cách TypeScript xử lý `this` trong class và các hàm callback.
    - Trả lời được:
      - Bốn trụ cột của OOP là gì và chúng được thể hiện như thế nào trong TypeScript?
      - Sự khác biệt giữa `public`, `private`, và `protected` là gì?
      - Khi nào nên sử dụng `abstract class` thay vì `interface`?
      - `static` members dùng để làm gì?
      - Làm thế nào để class tuân thủ một `interface` đã định nghĩa?

3.  **Kiến thức tiên quyết:**

    - Hoàn thành Phần 1 và Phần 2 (đặc biệt là `interface` và các khái niệm kiểu cơ bản).
    - Kiến thức cơ bản về OOP từ các ngôn ngữ khác (nếu có) sẽ là một lợi thế, nhưng không bắt buộc.
    - Hiểu biết về JavaScript `class` (ES6).

4.  **Giải thích lý thuyết chuyên sâu:**

    - **Ôn lại Bốn Trụ Cột của OOP:**

      1.  **Encapsulation (Tính đóng gói):** Gói gọn dữ liệu (thuộc tính) và các phương thức xử lý dữ liệu đó vào trong một đối tượng (class). Che giấu chi tiết triển khai bên trong và chỉ lộ ra giao diện (public methods) cần thiết.
          - **"TẠI SAO"?** Giảm sự phức tạp, tăng tính bảo mật (không cho phép thay đổi trực tiếp trạng thái bên trong một cách tùy tiện), dễ quản lý và bảo trì hơn vì thay đổi bên trong một class ít ảnh hưởng đến phần còn lại của hệ thống miễn là public interface không đổi.
      2.  **Abstraction (Tính trừu tượng):** Chỉ hiển thị các thông tin cần thiết của đối tượng ra bên ngoài và ẩn đi các chi tiết triển khai phức tạp. Tập trung vào "cái gì" đối tượng làm, thay vì "làm thế nào" nó làm.
          - **"TẠI SAO"?** Đơn giản hóa việc sử dụng đối tượng, cho phép tập trung vào mục đích chính thay vì lạc vào chi tiết.
      3.  **Inheritance (Tính kế thừa):** Cho phép một class (class con - subclass/derived class) thừa hưởng các thuộc tính và phương thức từ một class khác (class cha - superclass/base class).
          - **"TẠI SAO"?** Tái sử dụng code, tạo ra một hệ thống phân cấp class, dễ mở rộng và bảo trì.
      4.  **Polymorphism (Tính đa hình):** "Nhiều hình dạng". Cho phép các đối tượng thuộc các class khác nhau có thể được xử lý thông qua một interface chung (thường là class cha hoặc interface). Một hành động (phương thức) có thể có các cách thực thi khác nhau tùy thuộc vào đối tượng gọi nó.
          - **"TẠI SAO"?** Tăng tính linh hoạt và khả năng mở rộng. Code có thể làm việc với các đối tượng mà không cần biết kiểu cụ thể của chúng tại thời điểm viết code.

    - **Classes trong TypeScript:**

      - Cú pháp tương tự ES6 class, nhưng có thêm các tính năng về kiểu và access modifiers.
      - **Constructor:** Phương thức đặc biệt để tạo và khởi tạo object.

        ```typescript
        class Greeter {
          greeting: string; // Khai báo thuộc tính và kiểu của nó

          constructor(message: string) {
            this.greeting = message;
            console.log("Greeter instance created!");
          }

          greet(): string {
            return "Hello, " + this.greeting;
          }
        }

        let greeter = new Greeter("world");
        console.log(greeter.greet()); // Output: Hello, world
        ```

      - **Properties (Thuộc tính):** Biến thành viên của class. Cần khai báo kiểu.
      - **Methods (Phương thức):** Hàm thành viên của class.

    - **Access Modifiers (Bộ điều chỉnh truy cập):**

      - **`public` (Mặc định):** Thành viên có thể được truy cập từ bất cứ đâu (bên trong class, class con, instance bên ngoài). Nếu không chỉ định modifier, nó mặc định là `public`.
      - **`private`:** Thành viên chỉ có thể được truy cập từ bên trong class định nghĩa nó. Class con hoặc instance bên ngoài không thể truy cập.
        - **"TẠI SAO" `private`?** Để thực hiện tính đóng gói, che giấu chi tiết triển khai, ngăn chặn việc thay đổi trạng thái nội bộ một cách không kiểm soát.
      - **`protected`:** Thành viên có thể được truy cập từ bên trong class định nghĩa nó và từ các class con kế thừa nó. Instance bên ngoài không thể truy cập.
        - **"TẠI SAO" `protected`?** Cho phép class con tùy chỉnh hoặc sử dụng một phần logic của class cha mà không cần lộ ra ngoài public.
      - **Ví dụ:**

        ```typescript
        class Animal {
          public name: string;
          private age: number; // Chỉ truy cập được trong Animal
          protected sound: string; // Truy cập được trong Animal và lớp con như Dog

          constructor(name: string, age: number, sound: string) {
            this.name = name;
            this.age = age;
            this.sound = sound;
          }

          public makeSound(): void {
            console.log(`${this.name} says ${this.getSoundDescription()}`);
          }

          private getAgeInDogYears(): number {
            // Phương thức private
            return this.age * 7; // Chỉ dùng trong Animal
          }

          protected getSoundDescription(): string {
            return this.sound;
          }

          public displayAge(): void {
            console.log(
              `${this.name} is ${
                this.age
              } years old. In dog years: ${this.getAgeInDogYears()}`
            );
          }
        }

        class Dog extends Animal {
          constructor(name: string, age: number) {
            super(name, age, "Woof");
          }

          public fetch(): void {
            console.log(`${this.name} is fetching a ball.`);
            // console.log(this.age); // Lỗi: Property 'age' is private and only accessible within class 'Animal'.
            console.log(`It makes a sound: ${this.sound}`); // OK, sound is protected
          }

          // Override phương thức protected
          protected getSoundDescription(): string {
            return `a loud ${this.sound}`;
          }
        }

        let myDog = new Dog("Buddy", 3);
        myDog.makeSound(); // Output: Buddy says a loud Woof
        myDog.displayAge(); // Output: Buddy is 3 years old. In dog years: 21
        myDog.fetch();
        // console.log(myDog.age); // Lỗi: Property 'age' is private...
        // console.log(myDog.sound); // Lỗi: Property 'sound' is protected...
        console.log(myDog.name); // OK, name is public
        ```

      - **Lưu ý về `private` trong TypeScript vs JavaScript:**
        - Trong TypeScript, `private` và `protected` là các construct của trình biên dịch. Chúng được kiểm tra tại compile-time. Sau khi biên dịch sang JavaScript, các thuộc tính này vẫn tồn tại trên object và có thể truy cập được (nếu cố tình).
        - JavaScript hiện đại có hỗ trợ "private class fields" bằng cách sử dụng `#` prefix (ví dụ: `#privateField`). TypeScript cũng hỗ trợ cú pháp này và nó đảm bảo tính private thực sự ở runtime.
          ```typescript
          class HardPrivate {
            #trulyPrivateField: string;
            constructor(secret: string) {
              this.#trulyPrivateField = secret;
            }
            getSecret() {
              return this.#trulyPrivateField;
            }
          }
          const hp = new HardPrivate("super secret");
          console.log(hp.getSecret());
          // console.log(hp.#trulyPrivateField); // Lỗi: Property '#trulyPrivateField' is not accessible outside class 'HardPrivate' because it has a private identifier.
          ```
        - **"KHI NÀO" dùng `#`?** Khi bạn cần tính private được đảm bảo ở runtime. Tuy nhiên, cú pháp `private` của TypeScript vẫn rất hữu ích cho việc kiểm tra tĩnh và rõ ràng ý đồ.

    - **`readonly` Modifier:**

      - Thuộc tính `readonly` chỉ có thể được gán giá trị một lần, thường là trong constructor hoặc tại thời điểm khai báo.
      - **"TẠI SAO" `readonly`?** Để tạo các thuộc tính bất biến (immutable) sau khi object được khởi tạo, tăng tính an toàn và dễ dự đoán.
      - ```typescript
        class Octopus {
          readonly name: string;
          readonly numberOfLegs: number = 8; // Gán tại điểm khai báo

          constructor(theName: string) {
            this.name = theName; // Gán trong constructor
          }

          setName(newName: string) {
            // this.name = newName; // Lỗi: Cannot assign to 'name' because it is a read-only property.
          }
        }
        let dad = new Octopus("Man with the 8 legs");
        // dad.name = "Man with the 3-piece suit"; // Lỗi
        ```

      - **Parameter Properties (Thuộc tính tham số):** Một cách viết tắt để khai báo và khởi tạo thuộc tính thành viên từ tham số constructor.

        ```typescript
        class Person {
          // Khai báo và gán thủ công
          // public name: string;
          // private age: number;
          // constructor(name: string, age: number) {
          //   this.name = name;
          //   this.age = age;
          // }

          // Sử dụng parameter properties
          constructor(
            public readonly name: string,
            private age: number,
            protected city: string = "Unknown"
          ) {}

          public getDetails(): string {
            return `${this.name} is ${this.age} years old and lives in ${this.city}.`;
          }
        }
        let adam = new Person("Adam", 30);
        console.log(adam.name); // Adam
        // console.log(adam.age); // Lỗi: Property 'age' is private...
        console.log(adam.getDetails()); // Adam is 30 years old and lives in Unknown.
        ```

    - **Inheritance (Kế thừa `extends`):**

      - Class con kế thừa các thuộc tính và phương thức (không phải `private`) từ class cha.
      - Sử dụng từ khóa `extends`.
      - **`super()`:**
        - Trong constructor của class con, `super()` phải được gọi _trước khi_ truy cập `this` hoặc gán giá trị cho thuộc tính của class con. Nó gọi constructor của class cha.
        - `super.methodName()` có thể được dùng để gọi phương thức của class cha từ class con.
      - **Method Overriding (Ghi đè phương thức):** Class con có thể cung cấp một triển khai cụ thể cho một phương thức đã được định nghĩa ở class cha.

        ```typescript
        class Vehicle {
          constructor(public brand: string) {}

          startEngine(): void {
            console.log(`${this.brand} engine started.`);
          }

          honk(): void {
            console.log("Beep beep!");
          }
        }

        class Car extends Vehicle {
          constructor(brand: string, public model: string) {
            super(brand); // Gọi constructor của Vehicle
            console.log(`Car instance created: ${this.brand} ${this.model}`);
          }

          // Ghi đè phương thức honk
          honk(): void {
            super.honk(); // Gọi phương thức honk của class cha (tùy chọn)
            console.log(`${this.brand} ${this.model} says: Honk Honk!`);
          }

          drive(): void {
            console.log(`${this.brand} ${this.model} is driving.`);
          }
        }

        let myCar = new Car("Toyota", "Camry");
        myCar.startEngine(); // Toyota engine started.
        myCar.honk(); // Beep beep! Toyota Camry says: Honk Honk!
        myCar.drive(); // Toyota Camry is driving.
        ```

      - **Polymorphism (Đa hình) với kế thừa:**

        ```typescript
        function operateVehicle(vehicle: Vehicle): void {
          vehicle.startEngine();
          vehicle.honk(); // Sẽ gọi phương thức honk() của Car nếu vehicle là Car instance
          // vehicle.drive(); // Lỗi: Property 'drive' does not exist on type 'Vehicle'.
          if (vehicle instanceof Car) {
            vehicle.drive(); // OK, sau khi kiểm tra kiểu (type guard)
          }
        }

        operateVehicle(new Vehicle("Generic Brand"));
        // Output:
        // Generic Brand engine started.
        // Beep beep!

        operateVehicle(myCar); // myCar là một instance của Car, cũng là một Vehicle
        // Output:
        // Car instance created: Toyota Camry
        // Toyota engine started.
        // Beep beep!
        // Toyota Camry says: Honk Honk!
        // Toyota Camry is driving.
        ```

    - **Static Members (Thành viên tĩnh):**

      - Thuộc tính và phương thức `static` thuộc về class, chứ không phải instance của class.
      - Truy cập thông qua tên class (ví dụ: `ClassName.staticMember`).
      - **"TẠI SAO" `static`?**
        1.  Tạo các hàm tiện ích hoặc hằng số liên quan đến class mà không cần tạo instance.
        2.  Quản lý trạng thái chung cho tất cả các instance của class (ví dụ: bộ đếm instance).
        3.  Triển khai Factory Pattern.
      - ```typescript
        class MathHelper {
          static readonly PI: number = 3.14159;
          private static instanceCount: number = 0;

          constructor() {
            MathHelper.instanceCount++;
          }

          static circleArea(radius: number): number {
            return MathHelper.PI * radius * radius;
          }

          static getInstanceCount(): number {
            return MathHelper.instanceCount;
          }
        }

        console.log(MathHelper.PI); // 3.14159
        console.log(MathHelper.circleArea(5)); // 78.53975
        // let helper = new MathHelper();
        // console.log(helper.PI); // Lỗi: Property 'PI' does not exist on type 'MathHelper'. It is a static member of 'MathHelper'.
        new MathHelper();
        new MathHelper();
        console.log(MathHelper.getInstanceCount()); // 2
        ```

    - **Abstract Classes and Methods (Lớp và Phương thức trừu tượng):**

      - `abstract class` là class không thể tạo instance trực tiếp. Nó dùng làm class cha cho các class khác.
      - `abstract method` là phương thức được khai báo trong abstract class nhưng không có triển khai. Class con _bắt buộc_ phải triển khai (override) các abstract method này.
      - **"TẠI SAO" `abstract class`?**
        1.  Để định nghĩa một khuôn mẫu chung (common template) và một số hành vi mặc định cho một nhóm các class liên quan.
        2.  Buộc các class con phải cung cấp triển khai cụ thể cho một số phương thức nhất định.
        3.  Khác với `interface`, `abstract class` có thể chứa các phương thức đã được triển khai và thuộc tính.
      - ```typescript
        abstract class Shape {
          constructor(public color: string) {}

          abstract getArea(): number; // Phương thức trừu tượng, không có body
          abstract getPerimeter(): number;

          displayInfo(): void {
            // Phương thức đã triển khai
            console.log(`This is a ${this.color} shape.`);
            console.log(`Area: ${this.getArea()}`);
            console.log(`Perimeter: ${this.getPerimeter()}`);
          }
        }

        // const myShape = new Shape("red"); // Lỗi: Cannot create an instance of an abstract class.

        class Circle extends Shape {
          constructor(color: string, public radius: number) {
            super(color);
          }

          getArea(): number {
            // Bắt buộc phải triển khai
            return Math.PI * this.radius * this.radius;
          }

          getPerimeter(): number {
            // Bắt buộc phải triển khai
            return 2 * Math.PI * this.radius;
          }
        }

        class Rectangle extends Shape {
          constructor(
            color: string,
            public width: number,
            public height: number
          ) {
            super(color);
          }

          getArea(): number {
            return this.width * this.height;
          }

          getPerimeter(): number {
            return 2 * (this.width + this.height);
          }
        }

        const redCircle = new Circle("red", 5);
        redCircle.displayInfo();
        // Output:
        // This is a red shape.
        // Area: 78.53981633974483
        // Perimeter: 31.41592653589793

        const blueRectangle = new Rectangle("blue", 4, 6);
        blueRectangle.displayInfo();
        // Output:
        // This is a blue shape.
        // Area: 24
        // Perimeter: 20
        ```

      - **`abstract class` vs. `interface`:**
        | Đặc điểm | `abstract class` | `interface` |
        |---------------------|---------------------------------------------------|------------------------------------------------------|
        | Tạo instance | Không thể | Không thể |
        | Chứa triển khai | Có thể (cho non-abstract methods) | Không (chỉ định nghĩa signature) |
        | Chứa thuộc tính | Có thể | Có thể (chỉ định nghĩa tên và kiểu) |
        | Constructor | Có | Không |
        | Access modifiers | Có thể dùng cho methods/properties | Mặc định là `public` cho tất cả members (không ghi) |
        | Kế thừa | Class con `extends` (chỉ một abstract class) | Class `implements` (nhiều interfaces), interface `extends` (nhiều interfaces) |
        | Mục đích chính | Chia sẻ code, định nghĩa khuôn mẫu với một số triển khai sẵn | Định nghĩa contract, hình dạng của object |
        - **"KHI NÀO" chọn cái nào?**
          - Dùng `abstract class` khi bạn muốn chia sẻ code (triển khai phương thức, thuộc tính) giữa các class liên quan chặt chẽ, và muốn định nghĩa một "khung sườn" mà các class con phải tuân theo.
          - Dùng `interface` khi bạn muốn định nghĩa một "hợp đồng" (contract) mà các class (có thể không liên quan) phải tuân thủ, hoặc khi bạn muốn mô tả hình dạng của object mà không quan tâm đến việc triển khai. Một class có thể implement nhiều interface.

    - **Class `implements` Interface:**

      - Một class có thể khai báo rằng nó tuân thủ một hoặc nhiều `interface` bằng từ khóa `implements`.
      - Class đó phải cung cấp triển khai cho tất cả các thành viên được định nghĩa trong interface.
      - **"TẠI SAO" `implements`?** Để đảm bảo class có đúng "hình dạng" hoặc "hành vi" mà interface yêu cầu, tăng tính nhất quán và cho phép đa hình dựa trên interface.
      - ```typescript
        interface Loggable {
          log(message: string): void;
        }

        interface Serializable {
          serialize(): string;
        }

        class Product implements Loggable, Serializable {
          constructor(public id: string, public name: string) {}

          log(message: string): void {
            console.log(`[Product ${this.id} Log]: ${message}`);
          }

          serialize(): string {
            return JSON.stringify({ id: this.id, name: this.name });
          }
        }

        class OrderService implements Loggable {
          log(message: string): void {
            console.log(`[OrderService Log]: ${message}`);
          }
          processOrder() {
            this.log("Processing new order...");
          }
        }

        function processAndLog(item: Loggable & Serializable) {
          item.log("Starting process...");
          const serializedData = item.serialize();
          console.log("Serialized:", serializedData);
          item.log("Process complete.");
        }

        const myProduct = new Product("p123", "Laptop Pro");
        processAndLog(myProduct);
        // Output:
        // [Product p123 Log]: Starting process...
        // Serialized: {"id":"p123","name":"Laptop Pro"}
        // [Product p123 Log]: Process complete.

        // const orderService = new OrderService();
        // processAndLog(orderService); // Lỗi: Property 'serialize' is missing in type 'OrderService' but required in type 'Serializable'.
        ```

    - **`this` trong TypeScript Classes:**

      - Trong các phương thức của class, `this` thường tham chiếu đến instance hiện tại của class đó.
      - **Vấn đề với `this` trong callbacks:** Khi một phương thức của class được truyền làm callback cho một hàm khác (ví dụ: `setTimeout`, event handlers), `this` bên trong callback đó có thể không còn trỏ đến instance của class nữa, mà có thể là `window` (trong browser), `undefined` (trong strict mode), hoặc một object khác.
      - **Giải pháp:**

        1.  **Arrow Functions cho phương thức:** Arrow functions không có `this` của riêng nó, nó "mượn" `this` từ lexical scope (phạm vi bao quanh nó khi nó được định nghĩa). Đây là cách phổ biến nhất.

            ```typescript
            class MyClass {
              message: string = "Hello";

              // Phương thức thường, 'this' có thể bị mất
              regularMethod() {
                // console.log(this.message); // 'this' có thể là undefined nếu gọi không đúng cách
              }

              // Arrow function làm phương thức, 'this' được giữ lại
              arrowMethod = () => {
                console.log(this.message); // 'this' luôn trỏ tới instance của MyClass
              };
            }

            const instance = new MyClass();
            // instance.regularMethod(); // OK

            setTimeout(instance.arrowMethod, 100); // Output: Hello (sau 100ms)

            const regularCallback = instance.regularMethod;
            // setTimeout(regularCallback, 200); // Lỗi runtime: Cannot read properties of undefined (reading 'message')
            // hoặc 'this' là đối tượng Timeout tùy môi trường

            // Giải pháp cho regularMethod: dùng .bind() hoặc wrapper
            setTimeout(instance.regularMethod.bind(instance), 300);
            setTimeout(() => instance.regularMethod(), 400);
            ```

        2.  **Sử dụng `bind`:** `myInstance.method.bind(myInstance)`.
        3.  **Sử dụng wrapper function:** `() => myInstance.method()`.

5.  **Minh họa trực quan và Code mẫu (Đã tích hợp trong phần Giải thích lý thuyết)**

    - **Sơ đồ UML Class Diagram cơ bản:**

      ```mermaid
      classDiagram
          direction LR
          class Animal {
              +name: string
              -age: number
              #sound: string
              +Animal(name, age, sound)
              +makeSound()
              -getAgeInDogYears()
              #getSoundDescription()
              +displayAge()
          }
          class Dog {
              +Dog(name, age)
              +fetch()
              #getSoundDescription()
          }
          Animal <|-- Dog  // Kế thừa

          class Shape {
              <<abstract>>
              +color: string
              +Shape(color)
              {abstract} +getArea(): number
              {abstract} +getPerimeter(): number
              +displayInfo()
          }
          class Circle {
              +radius: number
              +Circle(color, radius)
              +getArea(): number
              +getPerimeter(): number
          }
          class Rectangle {
              +width: number
              +height: number
              +Rectangle(color, width, height)
              +getArea(): number
              +getPerimeter(): number
          }
          Shape <|-- Circle
          Shape <|-- Rectangle

          interface Loggable {
              <<interface>>
              +log(message: string): void
          }
          class Product {
              +id: string
              +name: string
              +Product(id, name)
              +log(message: string): void
              +serialize(): string
          }
          Loggable <|.. Product : implements
          Serializable <|.. Product : implements
          note for Product "implements Serializable"

          class MathHelper {
              {static} +PI: number
              {static} -instanceCount: number
              {static} +circleArea(radius): number
              {static} +getInstanceCount(): number
          }
      ```

6.  **Best Practices và Quy ước (Conventions):**

    - **Nguyên tắc SOLID:** Khi thiết kế class, hãy cố gắng tuân thủ các nguyên tắc SOLID (chúng ta sẽ có một phần riêng cho Design Patterns và SOLID).
      - **S**ingle Responsibility Principle (SRP)
      - **O**pen/Closed Principle (OCP)
      - **L**iskov Substitution Principle (LSP)
      - **I**nterface Segregation Principle (ISP)
      - **D**ependency Inversion Principle (DIP)
    - **Ưu tiên composition over inheritance (Ưu tiên cấu thành hơn kế thừa):** Kế thừa tạo ra mối quan hệ chặt chẽ. Cân nhắc việc "có một" (has-a relationship - composition) thay vì "là một" (is-a relationship - inheritance) nếu nó giúp code linh hoạt hơn.
    - **Sử dụng `private` hoặc `#` để đóng gói:** Che giấu chi tiết triển khai.
    - **Sử dụng `readonly` cho các thuộc tính không nên thay đổi sau khi khởi tạo.**
    - **Sử dụng `abstract class` khi bạn cần chia sẻ code và định nghĩa một khuôn mẫu chung mà các class con phải tuân theo.**
    - **Sử dụng `interface` để định nghĩa contract, tách rời việc định nghĩa và triển khai.**
    - **Sử dụng arrow functions cho các phương thức sẽ được dùng làm callbacks để giữ `this`.**
    - **Tránh kế thừa quá sâu (deep inheritance hierarchies):** Có thể làm code khó hiểu và khó bảo trì.

7.  **Anti-patterns và Lỗi thường gặp (Common Pitfalls):**

    - **God Object / God Class:** Một class biết hoặc làm quá nhiều thứ. Vi phạm SRP.
    - **Lạm dụng kế thừa:** Sử dụng kế thừa khi composition phù hợp hơn.
    - **Quên gọi `super()` trong constructor của class con, hoặc gọi sau khi dùng `this`.**
      ```typescript
      class Base {
        constructor(public x: number) {}
      }
      class Derived extends Base {
        y: number;
        constructor(x: number, y: number) {
          // this.y = y; // Lỗi: 'super' must be called before accessing 'this' in the constructor of a derived class.
          super(x);
          this.y = y; // OK
        }
      }
      ```
    - **Mất `this` trong callbacks:** Không sử dụng arrow function hoặc `bind`.
    - **Thay đổi `public` API của class một cách tùy tiện:** Phá vỡ code của những nơi đang sử dụng class đó.
    - **Nhầm lẫn `static` members với instance members.**
      ```typescript
      class Counter {
        static count: number = 0;
        instanceCount: number = 0;
        constructor() {
          Counter.count++; // Tăng static count
          this.instanceCount++; // Tăng instance count
        }
      }
      const c1 = new Counter();
      const c2 = new Counter();
      console.log(Counter.count); // 2
      console.log(c1.instanceCount); // 1
      console.log(c2.instanceCount); // 1
      // console.log(c1.count); // Lỗi: Property 'count' does not exist on type 'Counter'. It is a static member of 'Counter'.
      ```
    - **Không triển khai đầy đủ các abstract methods hoặc interface members.** TypeScript sẽ báo lỗi compile-time.

8.  **So sánh, Đánh giá và Lựa chọn (Đã đề cập trong phần Abstract Class vs Interface)**

9.  **Bài tập thực hành và Gợi ý dự án nhỏ:**

    - **Câu hỏi lý thuyết:**
      1.  Kể tên và giải thích ngắn gọn 4 trụ cột của OOP.
      2.  Sự khác biệt giữa `private`, `protected`, và `public` access modifiers trong TypeScript là gì?
      3.  Khi nào bạn nên sử dụng một `abstract class`? Khi nào bạn nên sử dụng một `interface`?
      4.  `static` methods và properties trong class dùng để làm gì? Cho ví dụ.
      5.  Vấn đề "mất `this`" trong JavaScript/TypeScript là gì và làm thế nào để giải quyết nó trong class methods?
    - **Bài tập gỡ lỗi (Debugging exercises):**

      ```typescript
      // Bài 1: Sửa lỗi 'this'
      /*
      class NotificationService {
          private message: string = "Default notification";
      
          constructor(msg?: string) {
              if (msg) this.message = msg;
          }
      
          showMessage() {
              console.log(this.message); // this bị sai ngữ cảnh
          }
      
          triggerNotification() {
              setTimeout(this.showMessage, 1000); // Lỗi: `this` trong showMessage sẽ là global hoặc undefined
          }
      }
      const notifier = new NotificationService("Custom message!");
      notifier.triggerNotification();
      */

      // Bài 2: Sửa lỗi kế thừa và abstract
      /*
      abstract class DataProcessor {
          rawData: any[];
          constructor(data: any[]) {
              this.rawData = data;
          }
          // process(): any[]; // Thiếu abstract keyword, và class con không triển khai
          commonUtil() { console.log("Common utility used."); }
      }
      
      class TextProcessor extends DataProcessor {
          constructor(textData: string[]) {
              super(textData);
          }
          // Thiếu triển khai phương thức process()
          // process() { return this.rawData.map(txt => txt.toUpperCase()); }
      }
      // const tp = new TextProcessor(["a", "b"]);
      // console.log(tp.process());
      */
      ```

    - **Thử thách code (Coding challenges):**
      1.  Thiết kế một hệ thống class cho một thư viện:
          - `Item` (abstract class): có `id (readonly, string)`, `title (string)`. Có một `abstract displayDetails(): void`.
          - `Book` (extends `Item`): thêm `author (string)`, `isbn (string)`. Triển khai `displayDetails`.
          - `DVD` (extends `Item`): thêm `director (string)`, `durationInMinutes (number)`. Triển khai `displayDetails`.
          - `Library` class: có một mảng các `Item`. Có phương thức `addItem(item: Item)`, `findItemById(id: string): Item | undefined`, `listAllItems(): void` (gọi `displayDetails` của từng item).
      2.  Tạo một `interface` `Shape2D` với phương thức `calculateArea(): number` và `calculatePerimeter(): number`.
          - Tạo các class `CircleShape`, `RectangleShape`, `TriangleShape` implement `Shape2D`.
          - Viết một hàm `printShapeInfo(shape: Shape2D)` nhận vào một đối tượng tuân thủ `Shape2D` và in ra diện tích, chu vi của nó.
      3.  Tạo một class `Logger` sử dụng Singleton pattern (chỉ có một instance duy nhất trong toàn bộ ứng dụng).
          - Có một phương thức `static getInstance(): Logger`.
          - Có một phương thức `log(level: "INFO" | "WARN" | "ERROR", message: string): void`.
          - Ngăn không cho tạo instance bằng `new Logger()`.
    - **Gợi ý dự án nhỏ (tiếp tục dự án Quản lý công việc CLI):**
      - Refactor lại code để sử dụng class. Ví dụ, tạo class `TaskManager` để quản lý danh sách các `Task` (có thể là class `Task` hoặc interface `Task`).
      - Class `Task` có thể có các phương thức như `markComplete()`, `updateDetails(updates: Partial<TaskData>)`, `getFormattedDueDate(): string`.
      - Sử dụng access modifiers để bảo vệ dữ liệu nội bộ của `TaskManager` và `Task`.

10. **Câu hỏi phỏng vấn thường gặp:**
    - "What are the core principles of OOP? Explain them with TypeScript examples."
    - "Describe the differences between `public`, `private`, and `protected` access modifiers in TypeScript."
    - "When would you use an `abstract class` versus an `interface`?"
    - "What are `static` members in a class? Give a use case."
    - "How does inheritance work in TypeScript? Explain `super()` and method overriding."
    - "What is polymorphism? How can it be achieved in TypeScript?"
    - "Explain the `this` keyword in TypeScript classes and how to handle its context in callbacks."
    - "What are parameter properties in TypeScript constructors?"
    - "Can a class implement multiple interfaces? Can a class extend multiple classes?" (Trả lời: Có, Không (TypeScript/JS chỉ hỗ trợ đơn kế thừa cho class, nhưng có thể dùng mixins hoặc composition để đạt được kết quả tương tự đa kế thừa))

---

Lập trình hướng đối tượng là một paradigma mạnh mẽ. Hiểu rõ cách TypeScript hỗ trợ OOP sẽ giúp bạn xây dựng các hệ thống backend có cấu trúc tốt, dễ bảo trì và mở rộng. Hãy thực hành tạo các class, kế thừa, và sử dụng interface. Khi bạn đã vững, chúng ta sẽ tiếp tục!

Chắc chắn rồi. Việc chia nhỏ các chủ đề phức tạp thành nhiều phần sẽ giúp việc tiếp thu kiến thức sâu hơn và có hệ thống hơn. Lộ trình 15 phần là một khung sườn, và chúng ta có thể linh hoạt điều chỉnh độ sâu và số lượng phần cho từng chủ đề nếu cần thiết.

Tiếp theo, chúng ta sẽ đi sâu vào một khía cạnh cực kỳ quan trọng trong phát triển phần mềm hiện đại, đặc biệt là khi làm việc với backend và microservices: **Modules, Namespaces và cách tổ chức code trong TypeScript**.

---

### **PHẦN 4: Modules, Namespaces và Tổ Chức Code - Xây Dựng Backend Dễ Bảo Trì**

1.  **Tên phần học:** Modules, Namespaces và Tổ Chức Code - Xây Dựng Backend Dễ Bảo Trì.

2.  **Mục tiêu học phần:**

    - Hiểu rõ sự khác biệt và mục đích sử dụng của **Modules (ES Modules)** và **Namespaces** trong TypeScript.
    - Nắm vững cú pháp `export` và `import` của ES Modules để chia sẻ và sử dụng code giữa các file.
    - Tìm hiểu các kiểu export: named exports, default export, re-exporting.
    - Hiểu cách TypeScript xử lý module resolution (tìm kiếm module).
    - Biết cách sử dụng `namespaces` để tổ chức code trong các ứng dụng client-side cũ hoặc khi cần tránh xung đột tên trong scope global (ít dùng hơn trong backend Node.js hiện đại).
    - Tìm hiểu về **Declaration Files (`.d.ts`)** và vai trò của chúng trong việc cung cấp thông tin kiểu cho các thư viện JavaScript hoặc các module TypeScript khác.
    - Nắm được các chiến lược tổ chức thư mục và file cho một dự án backend TypeScript quy mô vừa và lớn.
    - Hiểu khái niệm **Barrel files** (`index.ts`) và cách chúng giúp đơn giản hóa import.
    - Trả lời được:
      - Sự khác biệt giữa module và namespace là gì? Khi nào nên dùng cái nào?
      - Làm thế nào để export và import một class, function, variable từ một module?
      - Default export và named export khác nhau như thế nào?
      - Declaration files (`.d.ts`) dùng để làm gì?
      - Một số cách phổ biến để cấu trúc thư mục trong dự án backend TypeScript là gì?

3.  **Kiến thức tiên quyết:**

    - Hoàn thành Phần 1, 2, 3 (đặc biệt là cách khai báo biến, hàm, class, interface).
    - Kiến thức cơ bản về cách Node.js xử lý modules (CommonJS - `require`/`module.exports`) sẽ hữu ích để so sánh.
    - Đã làm quen với `tsconfig.json` và các tùy chọn `module`, `moduleResolution`.

4.  **Giải thích lý thuyết chuyên sâu:**

    - **Sự cần thiết của việc tổ chức code:**

      - Khi dự án phát triển, việc giữ toàn bộ code trong một file duy nhất trở nên không khả thi: khó đọc, khó bảo trì, khó quản lý, dễ xung đột tên.
      - **"TẠI SAO" cần tổ chức code?**
        1.  **Maintainability (Khả năng bảo trì):** Code được chia thành các phần nhỏ, độc lập hơn, dễ hiểu và sửa đổi hơn.
        2.  **Reusability (Khả năng tái sử dụng):** Các thành phần (hàm, class) có thể được sử dụng lại ở nhiều nơi khác nhau trong dự án hoặc thậm chí trong các dự án khác.
        3.  **Collaboration (Hợp tác):** Nhiều người có thể làm việc đồng thời trên các phần khác nhau của codebase mà ít xung đột hơn.
        4.  **Namespace Management (Quản lý không gian tên):** Tránh xung đột tên giữa các biến, hàm, class được định nghĩa ở các phần khác nhau.

    - **Modules trong TypeScript (ES Modules):**

      - TypeScript sử dụng chuẩn ES Modules (giống JavaScript hiện đại). Mỗi file `.ts` được coi là một module riêng.
      - Modules có scope riêng: Biến, hàm, class, interface... được khai báo trong một module mặc định là **private** đối với module đó (không thể truy cập từ module khác) trừ khi chúng được **`export`** một cách tường minh.
      - **"TẠI SAO" ES Modules là lựa chọn hàng đầu (đặc biệt cho backend Node.js hiện đại)?**
        1.  **Tiêu chuẩn:** Là tiêu chuẩn của JavaScript, được hỗ trợ ngày càng rộng rãi bởi Node.js (từ v13.2.0 với flag, và mặc định trong các phiên bản mới hơn khi dùng `type: "module"` trong `package.json` hoặc file `.mjs`) và trình duyệt.
        2.  **Static Analysis:** Cấu trúc `import`/`export` tĩnh cho phép các công cụ (như bundlers, linters, TypeScript compiler) phân tích dependencies tại compile-time, giúp tối ưu hóa (tree-shaking) và phát hiện lỗi sớm.
        3.  **Encapsulation mạnh mẽ:** Mặc định mọi thứ là private.
      - **Cấu hình trong `tsconfig.json`:**

        - `"module": "ESNext"` (hoặc "ES2015", "ES2020", "ES2022"): Để TypeScript phát ra code JavaScript sử dụng cú pháp ES Module.
        - `"module": "CommonJS"`: Để TypeScript phát ra code JavaScript sử dụng cú pháp CommonJS (thường dùng cho các dự án Node.js cũ hơn hoặc khi cần tương thích).
        - `"moduleResolution": "NodeNext"` (hoặc "Node16", "Node"): Cách TypeScript tìm kiếm các module (ví dụ: từ `node_modules` hoặc đường dẫn tương đối).
        - Để Node.js thực thi ES Modules trực tiếp, bạn cần thêm `"type": "module"` vào `package.json` hoặc đổi tên file thành `.mts` (cho TypeScript) và `.mjs` (cho JavaScript output).
          ```json
          // package.json
          {
            "name": "my-ts-backend",
            "version": "1.0.0",
            "type": "module" // Quan trọng cho Node.js ES Modules
            // ...
          }
          ```
          ```json
          // tsconfig.json
          {
            "compilerOptions": {
              "module": "ESNext", // Hoặc ES2020, ES2022
              "moduleResolution": "NodeNext", // Hoặc Node16
              "outDir": "./dist",
              "rootDir": "./src"
              // ... other options
            },
            "include": ["src/**/*"]
          }
          ```

      - **`export` statement:**

        - **Named Exports (Xuất theo tên):** Export nhiều thành phần từ một module.

          ```typescript
          // src/utils/math.ts
          export const PI: number = 3.14159;

          export function add(x: number, y: number): number {
            return x + y;
          }

          export class Calculator {
            multiply(a: number, b: number): number {
              return a * b;
            }
          }

          const SECRET_KEY = "abc"; // Private to this module
          ```

        - **`import` với Named Exports:**
          ```typescript
          // src/app.ts
          import { PI, add, Calculator } from "./utils/math.js"; // .js extension thường cần thiết khi type: "module"
          // hoặc cấu hình moduleResolution phù hợp
          console.log(PI);
          console.log(add(5, 3));
          const calc = new Calculator();
          console.log(calc.multiply(4, 6));
          // import { SECRET_KEY } from './utils/math.js'; // Lỗi: Module ... has no exported member 'SECRET_KEY'.
          ```
        - **Importing as (Aliasing imports):**
          ```typescript
          // src/app.ts
          import { PI as MyPI, add as sumNumbers } from "./utils/math.js";
          console.log(MyPI);
          console.log(sumNumbers(10, 20));
          ```
        - **Importing all named exports as an object:**
          ```typescript
          // src/app.ts
          import * as MathUtils from "./utils/math.js";
          console.log(MathUtils.PI);
          console.log(MathUtils.add(1, 2));
          const newCalc = new MathUtils.Calculator();
          console.log(newCalc.multiply(3, 7));
          ```
        - **Default Export (Xuất mặc định):** Mỗi module chỉ có thể có một default export.

          - **"TẠI SAO" default export?** Khi một module chủ yếu chỉ cung cấp một thứ chính (ví dụ: một class, một hàm).

          ```typescript
          // src/services/logger.ts
          class LoggerService {
            log(message: string) {
              console.log(`[LOG]: ${message}`);
            }
          }
          export default LoggerService; // Default export

          // Cũng có thể export biểu thức mặc định
          // export default function(message: string) { console.log(message); };
          // export default "Some Default Value";
          ```

        - **`import` với Default Export:** Bạn có thể đặt tên bất kỳ cho import.

          ```typescript
          // src/main.ts
          import MyCustomLogger from "./services/logger.js"; // Tên MyCustomLogger là tùy ý
          // import AnotherLogger from './services/logger.js'; // Cũng OK

          const logger = new MyCustomLogger();
          logger.log("Application started.");
          ```

        - **Kết hợp Named và Default Exports:**

          ```typescript
          // src/utils/strings.ts
          export function toUpperCase(str: string): string {
            return str.toUpperCase();
          }
          export function toLowerCase(str: string): string {
            return str.toLowerCase();
          }
          const VERSION = "1.0";
          export default VERSION; // Default export là VERSION
          ```

          ```typescript
          // src/index.ts
          import StringVersion, {
            toUpperCase,
            toLowerCase as lower,
          } from "./utils/strings.js";
          // StringVersion là default import (VERSION)
          // toUpperCase và lower (là toLowerCase) là named imports

          console.log(`String Utils Version: ${StringVersion}`);
          console.log(toUpperCase("hello"));
          console.log(lower("WORLD"));
          ```

        - **Re-exporting:** Export lại các thành phần từ module khác.

          - **"TẠI SAO" re-exporting?** Để tạo một "điểm vào" (entry point) duy nhất cho một nhóm các module, hoặc để cấu trúc lại cách các module được expose ra bên ngoài. Thường dùng trong "barrel files".

          ```typescript
          // src/utils/math.ts (như trên)
          // src/utils/strings.ts (như trên)

          // src/utils/index.ts (Barrel file)
          export { PI, add, Calculator } from "./math.js"; // Re-export named
          export * from "./strings.js"; // Re-export tất cả named exports từ strings.ts
          export { default as StringUtilVersion } from "./strings.js"; // Re-export default export với tên mới
          // export { default } from './logger.js'; // Re-export default (không đổi tên)
          ```

          ```typescript
          // src/app.ts
          import {
            PI,
            add,
            toUpperCase,
            StringUtilVersion,
          } from "./utils/index.js"; // Import từ barrel file
          console.log(PI);
          console.log(StringUtilVersion);
          ```

    - **Namespaces (Không gian tên):**

      - Một cách cũ hơn để tổ chức code và tránh xung đột tên trong scope global, trước khi ES Modules trở nên phổ biến.
      - Sử dụng từ khóa `namespace`. Các thành phần bên trong namespace cần `export` để có thể truy cập từ bên ngoài namespace.
      - **"TẠI SAO" namespaces tồn tại?** Ban đầu (khi TypeScript mới ra đời và JavaScript chưa có module chuẩn), namespaces là cách TypeScript cung cấp để module hóa code, tương tự như namespaces trong C# hay Java.
      - **"KHI NÀO" (có thể) dùng namespaces ngày nay?**
        1.  Làm việc với code client-side cũ không sử dụng module bundler, nơi các script được load trực tiếp vào global scope.
        2.  Để cấu trúc các file khai báo `.d.ts` lớn cho các thư viện global (thư viện không có module).
        3.  **Trong backend Node.js hiện đại, nên ưu tiên ES Modules hơn Namespaces.** Namespaces không cung cấp sự cô lập file-level như modules.
      - **Ví dụ:**

        ```typescript
        // src/validation.ts
        namespace Validation {
          export interface StringValidator {
            isAcceptable(s: string): boolean;
          }

          const lettersRegexp = /^[A-Za-z]+$/;
          const numberRegexp = /^[0-9]+$/;

          export class LettersOnlyValidator implements StringValidator {
            isAcceptable(s: string) {
              return lettersRegexp.test(s);
            }
          }

          export class ZipCodeValidator implements StringValidator {
            isAcceptable(s: string) {
              return s.length === 5 && numberRegexp.test(s);
            }
          }
        }

        // Sử dụng trong cùng file hoặc file khác (nếu compile thành một file JS duy nhất hoặc dùng /// <reference>)
        let validators: { [s: string]: Validation.StringValidator } = {};
        validators["ZIP code"] = new Validation.ZipCodeValidator();
        validators["Letters only"] = new Validation.LettersOnlyValidator();

        // console.log(Validation.lettersRegexp); // Lỗi: Property 'lettersRegexp' does not exist on type 'typeof Validation'.
        ```

      - **File nhiều namespace hoặc namespace lồng nhau:**
        ```typescript
        namespace MyCompany.MyProject.MyFeature {
          export class CoolThing {}
        }
        let thing = new MyCompany.MyProject.MyFeature.CoolThing();
        ```
      - **Biên dịch Namespaces:**
        - Nếu compile nhiều file có namespace vào một file JS duy nhất (`outFile` option trong `tsconfig.json`), các namespace sẽ được gộp lại.
        - Nếu compile từng file riêng lẻ (mặc định), bạn cần đảm bảo các file được load đúng thứ tự hoặc sử dụng `/// <reference path="..."/>` directives (cách cũ).
      - **Modules vs. Namespaces Tóm tắt:**
        | Đặc điểm | ES Modules | Namespaces |
        |-----------------------|-------------------------------------------------|----------------------------------------------------------|
        | Scope | File-level | Global (cần tên namespace để truy cập) |
        | Dependencies | `import`/`export` | `/// <reference path="..."/>` (cũ) hoặc compile gộp file |
        | Organization | Dựa trên file và thư mục | Logical grouping trong code |
        | Encapsulation | Mặc định private, export tường minh | Mặc định private trong namespace, export tường minh |
        | Tooling Support | Rất tốt (bundlers, tree-shaking) | Hạn chế hơn, chủ yếu là compiler |
        | Recommendation | **Ưu tiên cho hầu hết các dự án, đặc biệt backend** | Ít dùng, cho kịch bản cụ thể (global scripts, .d.ts lớn) |

    - **Declaration Files (`.d.ts`):**

      - Các file khai báo kiểu. Chúng **chỉ chứa thông tin kiểu**, không chứa code triển khai (JavaScript).
      - **"TẠI SAO" `.d.ts` files?**
        1.  **Để sử dụng thư viện JavaScript thuần trong TypeScript:** Nếu bạn dùng một thư viện JS (ví dụ: Lodash, jQuery cũ) không được viết bằng TS, bạn cần một file `.d.ts` để TypeScript hiểu được các API, kiểu dữ liệu của thư viện đó.
        2.  **Để mô tả hình dạng của các module không phải TS:** Ví dụ: module CSS, ảnh... khi import vào TS (cần cấu hình bundler).
        3.  **Để tách biệt phần khai báo kiểu và phần triển khai của một module TypeScript:** Đôi khi, bạn muốn publish chỉ thông tin kiểu mà không phải toàn bộ source code.
      - **Nguồn gốc `.d.ts` files:**
        - **Đi kèm thư viện:** Nhiều thư viện hiện đại được viết bằng TypeScript hoặc cung cấp sẵn file `.d.ts`.
        - **DefinitelyTyped (`@types`):** Một repository khổng lồ chứa các file `.d.ts` do cộng đồng đóng góp cho hàng ngàn thư viện JavaScript. Bạn cài chúng qua npm: `npm install --save-dev @types/lodash`.
        - **Tự viết:** Nếu không có sẵn, bạn có thể tự tạo file `.d.ts`.
        - **Compiler tự tạo:** Khi compile project TS với option `"declaration": true` trong `tsconfig.json`, trình biên dịch sẽ tự tạo ra các file `.d.ts` tương ứng cho các file `.ts` của bạn.
      - **Cấu trúc cơ bản của một file `.d.ts`:**

        ```typescript
        // Ví dụ: my-js-library.d.ts
        // Mô tả một thư viện global
        declare var myGlobalLibrary: {
          version: string;
          doSomething: (input: string) => number;
        };

        // Mô tả một module
        declare module "my-module-name" {
          export function greet(name: string): string;
          export interface Options {
            verbose?: boolean;
          }
        }

        // Mô tả một module với default export
        declare module "another-module" {
          const thing: { name: string };
          export default thing;
        }

        // Khai báo cho phép import file không phải code
        declare module "*.svg" {
          const content: any; // Hoặc một kiểu cụ thể hơn nếu biết
          export default content;
        }
        ```

      - **Ambient Declarations (Khai báo môi trường):** Các khai báo dùng `declare` không tạo ra code JS, chúng chỉ thông báo cho TS về sự tồn tại của các biến/hàm/module đó ở đâu đó (ví dụ: từ một file JS khác hoặc từ môi trường runtime).

    - **Tổ chức thư mục và file:**

      - Không có một "cách đúng duy nhất", phụ thuộc vào quy mô và loại dự án.
      - **Nguyên tắc chung:**
        - **Cohesion (Tính gắn kết):** Các file liên quan đến nhau nên ở gần nhau.
        - **Low Coupling (Ít phụ thuộc chéo):** Giảm sự phụ thuộc giữa các module/thư mục.
        - **Clear Naming (Đặt tên rõ ràng):** Tên file và thư mục nên phản ánh nội dung bên trong.
      - **Cấu trúc phổ biến cho backend (ví dụ: Node.js/Express):**
        ```
        my-ts-project/
        ├── dist/                     # Thư mục code JS đã biên dịch (từ tsconfig.outDir)
        ├── node_modules/
        ├── src/                      # Thư mục code TypeScript (từ tsconfig.rootDir)
        │   ├── app.ts                # Điểm khởi tạo chính của ứng dụng (Express app, server setup)
        │   ├── server.ts             # Khởi động server HTTP
        │   ├── config/               # Cấu hình (database, env variables, etc.)
        │   │   ├── index.ts          # Barrel file cho config
        │   │   └── database.ts
        │   │   └── environment.ts
        │   ├── api/ (hoặc routes/)   # Định nghĩa các API endpoints, controllers
        │   │   ├── index.ts          # Barrel file cho routes
        │   │   ├── users/
        │   │   │   ├── user.routes.ts
        │   │   │   ├── user.controller.ts
        │   │   │   ├── user.service.ts
        │   │   │   ├── user.model.ts     # (Nếu dùng ORM)
        │   │   │   └── user.validation.ts
        │   │   └── products/
        │   │       └── ...
        │   ├── services/             # Logic nghiệp vụ, tương tác với database/external services
        │   │   ├── index.ts
        │   │   └── auth.service.ts
        │   │   └── payment.service.ts
        │   ├── models/ (hoặc entities/) # Định nghĩa cấu trúc dữ liệu, ORM models
        │   │   ├── index.ts
        │   │   └── user.entity.ts
        │   │   └── product.entity.ts
        │   ├── middlewares/            # Express middlewares (authentication, logging, error handling)
        │   │   ├── index.ts
        │   │   └── errorHandler.ts
        │   │   └── auth.middleware.ts
        │   ├── utils/                  # Các hàm tiện ích chung
        │   │   ├── index.ts
        │   │   └── logger.ts
        │   │   └── helpers.ts
        │   ├── interfaces/ (hoặc types/) # Global interfaces, types (nếu không đặt cùng feature)
        │   │   ├── index.ts
        │   │   └── common.types.ts
        │   └── @types/                 # Thư mục chứa các file .d.ts tùy chỉnh (nếu cần)
        │       └── express/
        │           └── index.d.ts      # Ví dụ: mở rộng Request object của Express
        ├── tests/                    # Tests (unit, integration)
        │   ├── **/*.spec.ts
        ├── .env
        ├── .gitignore
        ├── package.json
        ├── tsconfig.json
        └── README.md
        ```
      - **Phân chia theo Feature (Feature-based):** Trong `api/` (hoặc `modules/`, `features/`), nhóm tất cả code liên quan đến một feature (route, controller, service, model) vào cùng một thư mục. Ví dụ `src/features/users/`. Điều này giúp tăng tính gắn kết.
      - **Phân chia theo Layer (Layer-based):** Như ví dụ trên, có các thư mục `controllers/`, `services/`, `models/` ở cấp cao.

    - **Barrel Files (`index.ts`):**
      - Là một file `index.ts` trong một thư mục, có nhiệm vụ re-export các module quan trọng từ thư mục đó hoặc các thư mục con.
      - **"TẠI SAO" Barrel files?**
        1.  **Đơn giản hóa import:** Thay vì import từ nhiều đường dẫn dài `../../feature/component`, bạn có thể import từ đường dẫn thư mục `../feature`.
        2.  **Tạo ra một public API cho một thư mục/module lớn:** Chỉ export những gì cần thiết ra bên ngoài, ẩn đi các chi tiết triển khai.
      - **Ví dụ (trong `src/services/index.ts`):**
        ```typescript
        export * from "./auth.service";
        export * from "./payment.service";
        export { default as NotificationService } from "./notification.service";
        ```
      - **Khi import:**
        ```typescript
        import {
          AuthService,
          PaymentService,
          NotificationService,
        } from "../services"; // Không cần ../services/index.ts
        ```
      - **Cẩn trọng với Barrel files:**
        - Có thể gây ra circular dependencies nếu không cẩn thận.
        - Trong các dự án rất lớn, barrel file ở thư mục gốc có thể làm chậm quá trình build hoặc intellisense do phải load quá nhiều module. Cân nhắc dùng barrel ở các cấp độ sâu hơn.

5.  **Minh họa trực quan và Code mẫu (Đã tích hợp trong phần Giải thích lý thuyết)**

    - **Sơ đồ Module Interaction:**

      ```mermaid
      graph TD
          subgraph ModuleA ["src/moduleA.ts"]
              A_Component1["export const A_Comp1"]
              A_Component2["export class A_Comp2"]
              A_Internal["internalFunc()"]
          end

          subgraph ModuleB ["src/moduleB.ts"]
              B_Component1["export default class B_Comp1"]
              B_Helper["export function B_Help()"]
          end

          subgraph BarrelFile ["src/utils/index.ts"]
              ReExport_A["export * from '../moduleA.js'"]
              ReExport_B_Default["export { default as B_Main } from '../moduleB.js'"]
              ReExport_B_Named["export { B_Help } from '../moduleB.js'"]
          end

          subgraph AppFile ["src/app.ts"]
              Import_A["import { A_Comp1, A_Comp2 } from './utils/index.js'"]
              Import_B["import { B_Main, B_Help } from './utils/index.js'"]
              Usage_A["A_Comp1, new A_Comp2()"]
              Usage_B["new B_Main(), B_Help()"]
          end

          ModuleA --> ReExport_A
          ModuleB --> ReExport_B_Default
          ModuleB --> ReExport_B_Named

          BarrelFile --> Import_A
          BarrelFile --> Import_B

          Import_A --> Usage_A
          Import_B --> Usage_B
      ```

6.  **Best Practices và Quy ước (Conventions):**

    - **Ưu tiên ES Modules cho các dự án mới, đặc biệt là backend Node.js.**
    - **Sử dụng named exports khi một module cung cấp nhiều thành phần liên quan.**
    - **Sử dụng default export khi một module chủ yếu cung cấp một class hoặc function chính.** Cẩn thận vì tên import có thể tùy ý, có thể gây nhầm lẫn nếu không nhất quán. Nhiều người thích chỉ dùng named exports để rõ ràng.
    - **Giữ đường dẫn import ngắn gọn và dễ hiểu. Sử dụng barrel files một cách hợp lý.**
    - **Tổ chức thư mục theo feature hoặc layer một cách nhất quán.**
    - **Cài đặt `@types/*` cho các thư viện JavaScript bạn sử dụng.**
    - **Bật `"declaration": true` và `"declarationMap": true` trong `tsconfig.json` nếu bạn đang xây dựng thư viện để publish.**
    - **Sử dụng đường dẫn tương đối (`./`, `../`) cho các module trong cùng project.**
    - **Sử dụng đường dẫn non-relative (ví dụ: `express`, `lodash`) cho các module từ `node_modules`.**
    - **Khi dùng ES Modules với Node.js (`"type": "module"`), nhớ thêm extension `.js` (hoặc `.mjs`, `.mts`) vào cuối đường dẫn import cho các file local.**
      ```typescript
      import { myFunc } from "./myModule.js"; // NOT './myModule'
      // Nếu không muốn thêm .js, bạn có thể sử dụng bundler hoặc cấu hình moduleResolution và custom loader.
      // Với `"moduleResolution": "NodeNext"` hoặc `"Node16"`, TypeScript thường hiểu mà không cần extension trong source code,
      // nhưng file JS output vẫn cần chúng để Node.js chạy đúng.
      ```

7.  **Anti-patterns và Lỗi thường gặp (Common Pitfalls):**

    - **Circular Dependencies (Phụ thuộc vòng):** Module A imports Module B, và Module B imports Module A.
      - **Vấn đề:** Có thể dẫn đến `undefined` khi import, hoặc lỗi runtime khó gỡ.
      - **Cách tránh/sửa:** Refactor code để phá vỡ vòng lặp (ví dụ: tách logic chung ra module thứ ba, dùng dependency injection, hoặc trì hoãn import bằng dynamic `import()`). Barrel files có thể vô tình tạo ra điều này.
    - **Quá nhiều re-exports trong một barrel file lớn:** Làm chậm build, khó tree-shake.
    - **Trộn lẫn `require` (CommonJS) và `import` (ESM) không đúng cách:** Dẫn đến lỗi hoặc hành vi không mong muốn. Nên chọn một hệ thống module chính cho dự án.
    - **Quên file `.d.ts` cho thư viện JS:** TypeScript báo lỗi không tìm thấy module hoặc kiểu.
    - **Sai cấu hình `module` hoặc `moduleResolution` trong `tsconfig.json`.**
    - **Path Aliases (Bí danh đường dẫn):** Dùng `paths` trong `tsconfig.json` để tạo alias (ví dụ: `@/components/*` trỏ tới `src/components/*`).
      - **Lợi ích:** Import đẹp hơn.
      - **Cạm bẫy:** Cần cấu hình thêm cho runtime (ví dụ: `tsconfig-paths` cho Node.js) và cho các tool khác (Jest, Webpack).
      ```json
      // tsconfig.json
      {
        "compilerOptions": {
          "baseUrl": "./", // Cần thiết cho paths
          "paths": {
            "@app/*": ["src/*"],
            "@services/*": ["src/services/*"]
          }
        }
      }
      // import { AuthService } from '@services/auth.service.js';
      ```
    - **Sử dụng namespace cho việc mà module làm tốt hơn:** Trong các dự án Node.js hiện đại, gần như luôn luôn nên dùng module.

8.  **So sánh, Đánh giá và Lựa chọn (Đã đề cập trong Modules vs. Namespaces)**

9.  **Bài tập thực hành và Gợi ý dự án nhỏ:**

    - **Câu hỏi lý thuyết:**
      1.  Phân biệt rõ ràng giữa ES Modules và Namespaces trong TypeScript. Khi nào bạn sẽ cân nhắc sử dụng Namespaces?
      2.  Giải thích sự khác biệt giữa `export default` và `export` (named export). Một module có thể có bao nhiêu default export?
      3.  Barrel files (`index.ts`) là gì và lợi ích của chúng? Có nhược điểm nào không?
      4.  File `.d.ts` dùng để làm gì? Bạn lấy chúng từ đâu hoặc làm thế nào để tạo ra chúng?
      5.  Tại sao việc tổ chức cấu trúc thư mục lại quan trọng trong một dự án backend?
    - **Bài tập gỡ lỗi (Debugging exercises):**

      ```typescript
      // src/models/user.model.ts
      // export class User { name: string; constructor(name: string) { this.name = name; } } // Giả sử quên export

      // src/services/user.service.ts
      // import { User } from '../models/user.model.js'; // Sẽ báo lỗi nếu User không được export
      /*
      export class UserService {
          // createUser(name: string): User {
          // return new User(name);
          // }
      }
      */

      // src/app.ts
      // import { UserService } from "./services/user.service.js"; // Giả sử quên .js
      /*
      const userService = new UserService();
      const newUser = userService.createUser("Alice");
      console.log(newUser.name);
      */

      // Bài 2: Circular dependency (khó mô phỏng đơn giản, thường là logic phức tạp hơn)
      // moduleA.ts: import { B_func } from './moduleB.js'; export function A_func() { B_func(); }
      // moduleB.ts: import { A_func } from './moduleA.js'; export function B_func() { /* A_func(); */ }
      // Yêu cầu: Nhận diện và đề xuất cách giải quyết.
      ```

    - **Thử thách code (Coding challenges):**
      1.  **Tạo cấu trúc module cho một ứng dụng blog nhỏ:**
          - `src/entities/post.entity.ts`: Định nghĩa `interface Post { id: string; title: string; content: string; }`. Export nó.
          - `src/services/post.service.ts`:
            - Có một mảng private `posts: Post[]`.
            - `createPost(title: string, content: string): Post` (tạo id ngẫu nhiên).
            - `getPostById(id: string): Post | undefined`.
            - `getAllPosts(): Post[]`.
            - Export `PostService` (class) hoặc các hàm này.
          - `src/utils/slugify.ts`: Viết hàm `slugify(text: string): string` để tạo slug từ title (ví dụ: "Hello World" -> "hello-world"). Export hàm này.
          - `src/main.ts`: Import các thành phần trên, tạo một vài post, lấy post, sử dụng `slugify`.
          - Tạo các barrel file (`index.ts`) cho `entities` và `services`.
      2.  **Viết file `.d.ts` cho một thư viện JavaScript đơn giản:**
          Giả sử có file `external-lib.js`:
          ```javascript
          // external-lib.js
          globalThis.SuperCalculator = {
            version: "1.0.0",
            add: function (a, b) {
              return a + b;
            },
            subtract: function (a, b) {
              return a - b;
            },
          };
          ```
          Viết file `external-lib.d.ts` để TypeScript có thể sử dụng `SuperCalculator` một cách an toàn kiểu.
    - **Gợi ý dự án nhỏ (tiếp tục dự án Quản lý công việc CLI):**
      - Tổ chức lại toàn bộ code của dự án CLI thành các module và thư mục có cấu trúc rõ ràng (ví dụ: `src/commands/`, `src/services/`, `src/models/`, `src/utils/`).
      - Sử dụng barrel files để đơn giản hóa các import.
      - Nếu có các hàm tiện ích chung, đưa chúng vào `src/utils/`.

10. **Câu hỏi phỏng vấn thường gặp:**
    - "What's the difference between modules and namespaces in TypeScript?"
    - "How do you export and import components (classes, functions, etc.) in TypeScript using ES Modules?"
    - "Explain default vs. named exports. What are the pros and cons of each?"
    - "What is a `.d.ts` file, and why would you need one?"
    - "How does module resolution work in TypeScript/Node.js? What's the role of `tsconfig.json` here?"
    - "What are barrel files (`index.ts`) and how can they help in organizing code?"
    - "How do you handle or avoid circular dependencies between modules?"
    - "Describe a common directory structure for a TypeScript backend project."
    - "If you are using a third-party JavaScript library that doesn't have TypeScript typings, what would you do?"

---

Tổ chức code tốt là một kỹ năng quan trọng, nó ảnh hưởng trực tiếp đến khả năng bảo trì và mở rộng của dự án. Hãy thực hành việc chia nhỏ code thành các module, sử dụng `import`/`export` và suy nghĩ về cấu trúc thư mục cho các dự án của bạn. Khi bạn đã sẵn sàng, chúng ta sẽ chuyển sang các chủ đề tiếp theo, có thể là đi sâu hơn vào Generics nâng cao, Decorators, hoặc bắt đầu với một framework backend cụ thể.

Tuyệt vời! Sau khi đã nắm vững cách tổ chức code với modules và namespaces, chúng ta sẽ khám phá một tính năng rất mạnh mẽ và đặc trưng của TypeScript (và các ngôn ngữ hướng đối tượng hiện đại khác) đó là **Decorators**. Decorators thường được sử dụng nhiều trong các framework backend như NestJS để giảm boilerplate và thêm metadata vào class, method.

---

### **PHẦN 5: Decorators trong TypeScript - Metaprogramming và Mở Rộng Hành Vi**

1.  **Tên phần học:** Decorators trong TypeScript - Metaprogramming và Mở Rộng Hành Vi.

2.  **Mục tiêu học phần:**

    - Hiểu rõ **Decorators là gì** và mục đích sử dụng của chúng trong TypeScript.
    - Nắm vững cú pháp khai báo và áp dụng các loại Decorators: Class Decorators, Method Decorators, Accessor Decorators, Property Decorators, và Parameter Decorators.
    - Hiểu các tham số mà mỗi loại decorator nhận được và giá trị (nếu có) mà chúng có thể trả về.
    - Biết cách **decorator factories** (hàm trả về decorator) được sử dụng để truyền tham số tùy chỉnh vào decorator.
    - Hiểu thứ tự thực thi của nhiều decorator khi áp dụng cho cùng một khai báo.
    - Tìm hiểu về **metadata reflection** sử dụng `Reflect.metadata` API (thường kết hợp với thư viện `reflect-metadata`) để đính kèm và truy xuất metadata thông qua decorators.
    - Nhận biết các trường hợp sử dụng phổ biến của decorators trong thực tế (ví dụ: logging, validation, dependency injection, routing trong frameworks).
    - Trả lời được:
      - Decorator trong TypeScript là gì? Nó có phải là một Design Pattern không?
      - Có những loại decorator nào và chúng được áp dụng ở đâu?
      - Làm thế nào để truyền tham số cho một decorator?
      - Decorators được thực thi khi nào và theo thứ tự nào?
      - `reflect-metadata` là gì và tại sao nó thường được dùng cùng với decorators?

3.  **Kiến thức tiên quyết:**

    - Hoàn thành Phần 1-4 (đặc biệt là Classes, Functions, Modules).
    - Hiểu biết về JavaScript (ES6+), đặc biệt là hàm bậc cao (higher-order functions) và closures.
    - Khái niệm về metaprogramming (lập trình meta - code viết code hoặc thay đổi hành vi của code khác) là một lợi thế.

4.  **Giải thích lý thuyết chuyên sâu:**

    - **Decorators là gì?**

      - Decorators là một tính năng **experimental** (thử nghiệm) trong TypeScript (dựa trên một proposal của JavaScript đang ở Stage 3). Để sử dụng, bạn cần bật cờ `"experimentalDecorators": true` trong `tsconfig.json`.
      - Về bản chất, **decorator là một hàm đặc biệt** được gọi tại thời điểm **định nghĩa class (class definition time)**, không phải tại thời điểm runtime khi class được instantiate.
      - Chúng có thể được sử dụng để quan sát, sửa đổi hoặc thay thế định nghĩa của class, method, accessor, property, hoặc parameter.
      - Cú pháp: Sử dụng ký tự `@` theo sau là tên của decorator, đặt ngay phía trước khai báo mà nó "trang trí".
        ```typescript
        // tsconfig.json
        // {
        //   "compilerOptions": {
        //     "experimentalDecorators": true,
        //     "emitDecoratorMetadata": true, // Thường dùng cùng reflect-metadata
        //     // ...
        //   }
        // }
        ```
      - **"TẠI SAO" Decorators? (Mục đích)**
        1.  **Metaprogramming:** Cho phép thêm metadata (siêu dữ liệu) vào code một cách khai báo (declarative).
        2.  **Aspect-Oriented Programming (AOP) like features:** Cho phép tách biệt các "cross-cutting concerns" (những mối quan tâm xuyên suốt nhiều module, ví dụ: logging, validation, caching, authorization) ra khỏi logic nghiệp vụ chính.
        3.  **Code Generation/Modification:** Có thể thay đổi hoặc mở rộng hành vi của class/method mà không cần sửa đổi trực tiếp code gốc của chúng (tuân thủ Open/Closed Principle ở một mức độ).
        4.  **Giảm Boilerplate Code:** Thường thấy trong các framework (NestJS, Angular) để tự động hóa các tác vụ lặp đi lặp lại.
      - **Decorator Pattern vs. Decorator Feature:**
        - **Decorator Design Pattern:** Một mẫu thiết kế cấu trúc cho phép thêm hành vi mới vào một đối tượng một cách linh hoạt bằng cách "gói" đối tượng đó trong một đối tượng decorator khác có cùng interface. Nó hoạt động ở runtime trên các instance.
        - **Decorator Feature (trong TS/JS):** Một tính năng ngôn ngữ hoạt động tại thời điểm định nghĩa class. Chúng không trực tiếp triển khai Decorator Design Pattern, nhưng có thể được sử dụng để đạt được các mục tiêu tương tự một cách khai báo hơn.

    - **Các loại Decorators:**
      TypeScript hỗ trợ 5 loại decorator:

      1.  **Class Decorator (`@ClassDecorator`)**

          - Áp dụng cho khai báo class.
          - Nhận một tham số: constructor của class được trang trí.
          - Nếu class decorator trả về một giá trị, nó sẽ thay thế định nghĩa class gốc bằng constructor mới (hoặc class mới).
          - **Signature:** `function classDecorator<T extends Function>(constructor: T): T | void { ... }`
          - **Ví dụ:**

            ```typescript
            function sealed(constructor: Function) {
              Object.seal(constructor); // Ngăn chặn việc thêm thuộc tính mới vào constructor
              Object.seal(constructor.prototype); // Ngăn chặn việc thêm thuộc tính mới vào prototype
              console.log(`Class ${constructor.name} has been sealed.`);
            }

            @sealed
            class BugReport {
              type = "report";
              title: string;

              constructor(t: string) {
                this.title = t;
              }
            }
            // Output: Class BugReport has been sealed.

            // const bug = new BugReport("Memory Leak");
            // (BugReport as any).newProperty = "test"; // Lỗi ở runtime nếu seal hoạt động (TS không bắt được ở compile time)
            // bug.newProp = 123; // Lỗi runtime
            ```

            ```typescript
            // Class decorator thay thế constructor
            function WithExtraProperty<T extends { new (...args: any[]): {} }>(
              originalConstructor: T
            ) {
              return class extends originalConstructor {
                // Trả về class mới kế thừa class gốc
                extraProperty = "I am an extra property!";
                constructor(...args: any[]) {
                  super(...args);
                  console.log(
                    `Extra property added to ${originalConstructor.name}`
                  );
                }
              };
            }

            @WithExtraProperty
            class MyOriginalClass {
              name: string;
              constructor(name: string) {
                this.name = name;
                console.log(`MyOriginalClass constructed with name: ${name}`);
              }
            }

            const instance = new MyOriginalClass("Test");
            // Output:
            // MyOriginalClass constructed with name: Test
            // Extra property added to MyOriginalClass
            console.log((instance as any).extraProperty); // I am an extra property!
            ```

      2.  **Method Decorator (`@MethodDecorator`)**

          - Áp dụng cho khai báo method trong class.
          - Nhận ba tham số:
            1.  `target`: Hoặc constructor của class (cho static method) hoặc prototype của class (cho instance method).
            2.  `propertyKey`: Tên của method (string hoặc symbol).
            3.  `descriptor`: Property Descriptor của method (`PropertyDescriptor`).
                - `PropertyDescriptor` có các thuộc tính: `value` (chứa hàm gốc), `writable`, `enumerable`, `configurable`.
          - Nếu method decorator trả về một giá trị, nó sẽ được sử dụng làm Property Descriptor mới cho method.
          - **Signature:** `function methodDecorator(target: any, propertyKey: string | symbol, descriptor: PropertyDescriptor): PropertyDescriptor | void { ... }`
          - **Ví dụ: Logging method calls**

            ```typescript
            function LogMethodCall(
              target: any,
              propertyKey: string,
              descriptor: PropertyDescriptor
            ) {
              const originalMethod = descriptor.value; // Lưu lại hàm gốc

              descriptor.value = function (...args: any[]) {
                // Ghi đè hàm gốc
                console.log(
                  `Calling ${propertyKey} with arguments: ${JSON.stringify(
                    args
                  )}`
                );
                const result = originalMethod.apply(this, args); // Gọi hàm gốc với 'this' và arguments đúng
                console.log(
                  `Method ${propertyKey} returned: ${JSON.stringify(result)}`
                );
                return result;
              };

              return descriptor; // Trả về descriptor đã sửa đổi
            }

            class CalculatorService {
              @LogMethodCall
              add(a: number, b: number): number {
                return a + b;
              }

              @LogMethodCall
              subtract(a: number, b: number): number {
                return a - b;
              }
            }

            const calcService = new CalculatorService();
            calcService.add(5, 3);
            // Output:
            // Calling add with arguments: [5,3]
            // Method add returned: 8
            calcService.subtract(10, 4);
            // Output:
            // Calling subtract with arguments: [10,4]
            // Method subtract returned: 6
            ```

      3.  **Accessor Decorator (`@AccessorDecorator`)**

          - Áp dụng cho khai báo accessor (getter/setter).
          - Nhận ba tham số giống như Method Decorator: `target`, `propertyKey`, `descriptor`.
          - Nếu accessor decorator trả về một giá trị, nó sẽ được sử dụng làm Property Descriptor mới cho accessor.
          - **Lưu ý:** Bạn không thể trang trí cả getter và setter cho cùng một thành viên. Nếu có cả hai, decorator chỉ được áp dụng cho thành viên đầu tiên được khai báo trong source code (thường là getter).
          - **Signature:** `function accessorDecorator(target: any, propertyKey: string | symbol, descriptor: PropertyDescriptor): PropertyDescriptor | void { ... }`
          - **Ví dụ: Enumerable accessor**

            ```typescript
            function Enumerable(value: boolean) {
              return function (
                target: any,
                propertyKey: string,
                descriptor: PropertyDescriptor
              ) {
                descriptor.enumerable = value;
              };
            }

            class Point {
              private _x: number;
              private _y: number;

              constructor(x: number, y: number) {
                this._x = x;
                this._y = y;
              }

              @Enumerable(true) // Làm cho x có thể liệt kê được
              get x() {
                return this._x;
              }

              // @Enumerable(false) // Không thể áp dụng cho setter nếu getter đã có
              set x(val: number) {
                this._x = val;
              }

              @Enumerable(false) // y không liệt kê được
              get y() {
                return this._y;
              }
            }

            const p = new Point(1, 2);
            for (const key in p) {
              console.log(key); // Sẽ chỉ in ra "x" (và có thể các thuộc tính từ prototype nếu có)
            }
            ```

      4.  **Property Decorator (`@PropertyDecorator`)**

          - Áp dụng cho khai báo property trong class.
          - Nhận hai tham số:
            1.  `target`: Hoặc constructor của class (cho static property) hoặc prototype của class (cho instance property).
            2.  `propertyKey`: Tên của property (string hoặc symbol).
          - **Lưu ý quan trọng:** Property decorator **không nhận `PropertyDescriptor`** làm tham số và **giá trị trả về của nó bị bỏ qua**. Do đó, property decorator không thể trực tiếp thay đổi giá trị khởi tạo hoặc hành vi của property. Chúng chủ yếu dùng để ghi nhận metadata.
          - **Signature:** `function propertyDecorator(target: any, propertyKey: string | symbol): void { ... }`
          - **Ví dụ: Ghi nhận metadata (sẽ kết hợp với `reflect-metadata` sau)**

            ```typescript
            function Format(formatString: string) {
              return function (target: any, propertyKey: string) {
                // Lưu trữ metadata ở đâu đó (ví dụ, sử dụng Reflect.defineMetadata)
                console.log(
                  `Property ${propertyKey} in class ${target.constructor.name} should be formatted as ${formatString}`
                );
                // Reflect.defineMetadata("format", formatString, target, propertyKey);
              };
            }

            class UserProfile {
              @Format("email")
              email: string;

              @Format("uppercase")
              username: string;

              constructor(email: string, username: string) {
                this.email = email;
                this.username = username;
              }
            }
            // Output:
            // Property email in class UserProfile should be formatted as email
            // Property username in class UserProfile should be formatted as uppercase
            ```

      5.  **Parameter Decorator (`@ParameterDecorator`)**

          - Áp dụng cho tham số của constructor hoặc method.
          - Nhận ba tham số:
            1.  `target`: Hoặc constructor của class (cho tham số của static method hoặc constructor) hoặc prototype của class (cho tham số của instance method).
            2.  `propertyKey`: Tên của method (string hoặc symbol) chứa tham số đó (là `undefined` nếu trang trí tham số của constructor).
            3.  `parameterIndex`: Chỉ số (vị trí) của tham số trong danh sách tham số của hàm (bắt đầu từ 0).
          - **Lưu ý quan trọng:** Giống như Property Decorator, giá trị trả về của Parameter Decorator bị bỏ qua. Chúng chủ yếu dùng để ghi nhận metadata về tham số.
          - **Signature:** `function parameterDecorator(target: any, propertyKey: string | symbol | undefined, parameterIndex: number): void { ... }`
          - **Ví dụ: Ghi nhận tham số cần validate**

            ```typescript
            function Required(
              target: any,
              propertyKey: string | undefined,
              parameterIndex: number
            ) {
              console.log(
                `Parameter at index ${parameterIndex} of method ${
                  String(propertyKey) || "constructor"
                } in ${target.constructor.name} is required.`
              );
              // Reflect.defineMetadata(`required_${parameterIndex}`, true, target, propertyKey);
            }

            class OrderService {
              createOrder(
                @Required userIdentifier: string,
                @Required productID: string,
                quantity: number
              ) {
                console.log(
                  `Order created for ${userIdentifier}, product ${productID}, quantity ${quantity}`
                );
              }

              updateStatus(orderId: string, @Required newStatus: string) {
                console.log(`Order ${orderId} status updated to ${newStatus}`);
              }
            }
            // Output:
            // Parameter at index 1 of method createOrder in OrderService is required.
            // Parameter at index 0 of method createOrder in OrderService is required.
            // Parameter at index 1 of method updateStatus in OrderService is required.
            // (Lưu ý thứ tự output có thể ngược do decorator được áp dụng từ tham số cuối đến đầu)
            ```

    - **Decorator Factories (Hàm tạo Decorator):**

      - Là một hàm trả về một biểu thức sẽ được gọi làm decorator tại runtime.
      - Cho phép truyền tham số tùy chỉnh vào decorator.
      - **"TẠI SAO" Decorator Factories?** Để làm cho decorator linh hoạt hơn, có thể cấu hình được.
      - **Ví dụ:** `@LogMethodCall` ở trên có thể được viết lại thành factory để cho phép bật/tắt logging.

        ```typescript
        function LogExecution(enabled: boolean = true) {
          // Đây là decorator factory
          return function (
            target: any,
            propertyKey: string,
            descriptor: PropertyDescriptor
          ) {
            // Đây là decorator thực sự
            if (!enabled) return;

            const originalMethod = descriptor.value;
            descriptor.value = function (...args: any[]) {
              console.log(`LOG_FACTORY: Calling ${propertyKey}`);
              const result = originalMethod.apply(this, args);
              console.log(`LOG_FACTORY: ${propertyKey} returned.`);
              return result;
            };
          };
        }

        class TaskRunner {
          @LogExecution(true) // Truyền tham số cho factory
          runImportantTask() {
            console.log("Important task running...");
          }

          @LogExecution(false) // Tắt logging cho method này
          runSilentTask() {
            console.log("Silent task running...");
          }
        }
        const runner = new TaskRunner();
        runner.runImportantTask();
        // Output:
        // LOG_FACTORY: Calling runImportantTask
        // Important task running...
        // LOG_FACTORY: runImportantTask returned.
        runner.runSilentTask();
        // Output:
        // Silent task running...
        ```

    - **Thứ tự thực thi Decorator (Decorator Evaluation/Execution Order):**

      - **Đánh giá (Evaluation):** Các biểu thức decorator (nếu là factory) được đánh giá từ trên xuống dưới (top-to-bottom).
      - **Thực thi (Execution):** Các hàm decorator thực sự được gọi từ dưới lên trên (bottom-to-up).
      - **Thứ tự trong một khai báo:**
        1.  Parameter Decorators, sau đó Method/Accessor/Property Decorators (cho từng thành viên).
        2.  Các decorator cho các thành viên instance được áp dụng trước các decorator cho thành viên static.
        3.  Các decorator cho constructor được áp dụng sau cùng.
        4.  Nếu nhiều decorator áp dụng cho cùng một khai báo, chúng được đánh giá từ trái sang phải (hoặc trên xuống dưới), và thực thi theo thứ tự ngược lại (như "hành tây" được bóc từ ngoài vào trong).
      - **Ví dụ:**

        ```typescript
        function First(): ClassDecorator {
          console.log("First(): factory evaluated");
          return function (constructor: Function) {
            console.log("First(): decorator executed");
          };
        }

        function Second(): ClassDecorator {
          console.log("Second(): factory evaluated");
          return function (constructor: Function) {
            console.log("Second(): decorator executed");
          };
        }

        @First()
        @Second()
        class MyExampleClass {}
        // Output:
        // First(): factory evaluated
        // Second(): factory evaluated
        // Second(): decorator executed  (Thực thi trước)
        // First(): decorator executed   (Thực thi sau)
        ```

    - **Metadata Reflection với `reflect-metadata`:**

      - Decorators rất mạnh khi kết hợp với khả năng thêm và đọc metadata.
      - Thư viện `reflect-metadata` (cài đặt: `npm install reflect-metadata`) cung cấp một Polyfill cho API `Reflect.metadata` (một proposal khác của JavaScript).
      - Bạn cần import nó một lần ở đầu ứng dụng (thường là file `main.ts` hoặc `app.ts`): `import "reflect-metadata";`
      - Bật `"emitDecoratorMetadata": true` trong `tsconfig.json`. Khi cờ này được bật, TypeScript sẽ tự động phát ra một số metadata về kiểu cho các thành viên được trang trí (ví dụ: kiểu của thuộc tính, kiểu tham số của method) nếu có thể.
      - **API chính:**
        - `Reflect.defineMetadata(metadataKey: any, metadataValue: any, target: Object, propertyKey?: string | symbol): void;`
        - `Reflect.getMetadata(metadataKey: any, target: Object, propertyKey?: string | symbol): any;`
        - `Reflect.hasMetadata(metadataKey: any, target: Object, propertyKey?: string | symbol): boolean;`
        - `Reflect.getOwnMetadata(...)`, `Reflect.deleteMetadata(...)`...
      - **"TẠI SAO" `reflect-metadata`?**
        1.  Cho phép decorator **đính kèm thông tin tùy chỉnh** vào class/method/property mà không làm thay đổi cấu trúc hay hành vi của chúng một cách trực tiếp.
        2.  Thông tin này sau đó có thể được **truy xuất và sử dụng bởi các decorator khác, hoặc bởi logic của framework** để thực hiện các tác vụ như dependency injection, validation, serialization, routing.
      - **Ví dụ: Validation đơn giản**

        ```typescript
        import "reflect-metadata"; // Import một lần ở đầu

        const requiredMetadataKey = Symbol("required");

        function RequiredParam(
          target: Object,
          propertyKey: string | symbol | undefined,
          parameterIndex: number
        ) {
          let existingRequiredParameters: number[] =
            Reflect.getOwnMetadata(requiredMetadataKey, target, propertyKey!) ||
            [];
          existingRequiredParameters.push(parameterIndex);
          Reflect.defineMetadata(
            requiredMetadataKey,
            existingRequiredParameters,
            target,
            propertyKey!
          );
          console.log(
            `Metadata set for: ${target.constructor.name}.${String(
              propertyKey
            )}[${parameterIndex}]`
          );
        }

        function Validate(
          target: any,
          propertyName: string,
          descriptor: TypedPropertyDescriptor<Function>
        ) {
          let method = descriptor.value!;

          descriptor.value = function (...args: any[]) {
            let requiredParameters: number[] = Reflect.getOwnMetadata(
              requiredMetadataKey,
              target,
              propertyName
            );
            if (requiredParameters) {
              for (let parameterIndex of requiredParameters) {
                if (
                  parameterIndex >= args.length ||
                  args[parameterIndex] === undefined ||
                  args[parameterIndex] === null ||
                  args[parameterIndex] === ""
                ) {
                  throw new Error(
                    `Missing required argument at index ${parameterIndex} for ${propertyName}.`
                  );
                }
              }
            }
            return method.apply(this, args);
          };
        }

        class GreeterService {
          greeting: string;

          constructor(message: string) {
            this.greeting = message;
          }

          @Validate // Áp dụng decorator validate
          greet(@RequiredParam name: string, @RequiredParam title: string) {
            // name là index 0, title là index 1
            return `Hello ${title} ${name}, ${this.greeting}`;
          }

          sayGoodbye(name: string) {
            // Không có @Validate hoặc @RequiredParam
            return `Goodbye ${name}`;
          }
        }

        const greeter = new GreeterService("welcome!");
        console.log(greeter.greet("Alice", "Ms.")); // OK
        // Output (từ decorator):
        // Metadata set for: GreeterService.greet[1]
        // Metadata set for: GreeterService.greet[0]
        // (Log từ greet): Hello Ms. Alice, welcome!

        try {
          // console.log(greeter.greet("Bob", undefined)); // Lỗi: Missing required argument at index 1 for greet.
          console.log(greeter.greet(undefined as any, "Mr.")); // Lỗi: Missing required argument at index 0 for greet.
        } catch (e: any) {
          console.error(e.message);
        }
        ```

      - **Metadata tự động từ TypeScript (với `emitDecoratorMetadata`):**
        Khi `emitDecoratorMetadata` là `true`, TypeScript sẽ tự động thêm metadata về kiểu bằng các key `design:type`, `design:paramtypes`, `design:returntype`.

        ```typescript
        import "reflect-metadata";

        function showMetadata(target: any, propertyKey: string) {
          const t = Reflect.getMetadata("design:type", target, propertyKey);
          console.log(`${propertyKey} type: ${t.name}`);
        }

        function showParamMetadata(target: any, propertyKey: string) {
          const params = Reflect.getMetadata(
            "design:paramtypes",
            target,
            propertyKey
          );
          const paramNames = params.map((p: any) => p.name).join(", ");
          console.log(`${propertyKey} param types: ${paramNames}`);
        }

        class DemoClass {
          @showMetadata
          myProperty: string = "test";

          @showParamMetadata
          myMethod(param1: number, param2: boolean): string {
            return `${param1} ${param2}`;
          }
        }
        // Output:
        // myProperty type: String
        // myMethod param types: Number, Boolean
        ```

        **Lưu ý:** Metadata kiểu này có giới hạn. Ví dụ, nó không thể suy luận kiểu generic, hoặc kiểu là `interface` (sẽ là `Object`). Nó hoạt động tốt nhất với các class và primitive types.

5.  **Minh họa trực quan và Code mẫu (Đã tích hợp trong phần Giải thích lý thuyết)**

    - **Sơ đồ: Decorator và Metadata**

      ```mermaid
      graph TD
          A["Class/Method/Property Definition"] -- "@Decorator" --> B(Decorator Function);
          B -- "Executes at Definition Time" --> C{"Modifies Definition OR Attaches Metadata"};
          C -- "Defines Metadata" --> D["Metadata Store (e.g., Reflect.metadata)"];

          subgraph Runtime
              E["Framework / Other Code"] -- "Reads Metadata" --> D;
              E -- "Uses Metadata to Alter Behavior" --> F["Instance Behavior (Validation, DI, Routing etc.)"];
              A_Instance["Class Instance"] --> F;
          end

          X["Decorator Factory (Optional)"] -- "Returns" --> B;
          Y["Parameters for Factory"] --> X;
      ```

6.  **Best Practices và Quy ước (Conventions):**

    - **Bật `experimentalDecorators` và `emitDecoratorMetadata` trong `tsconfig.json` khi sử dụng decorators, đặc biệt nếu dùng với `reflect-metadata`.**
    - **Import `reflect-metadata` một lần ở entry point của ứng dụng.**
    - **Giữ decorator đơn giản và tập trung vào một mục đích.** Nếu decorator quá phức tạp, hãy xem xét tách nó thành nhiều decorator nhỏ hơn hoặc sử dụng các kỹ thuật khác.
    - **Decorator nên ít gây side effects không mong muốn.** Mục tiêu chính là thay đổi định nghĩa hoặc đính kèm metadata.
    - **Cẩn thận với thứ tự thực thi của decorators, đặc biệt khi nhiều decorator được áp dụng.**
    - **Tài liệu hóa rõ ràng decorator làm gì, nhận tham số nào, và metadata nào nó thêm/đọc.**
    - **Sử dụng decorator factories để làm cho decorator của bạn có thể cấu hình được.**
    - **Hiểu rõ sự khác biệt giữa các loại decorator và những gì chúng có thể/không thể làm.** (Ví dụ: Property/Parameter decorator không thể thay đổi giá trị trực tiếp).

7.  **Anti-patterns và Lỗi thường gặp (Common Pitfalls):**

    - **Quên bật `experimentalDecorators` hoặc `emitDecoratorMetadata`:** Decorator không hoạt động hoặc metadata không được phát ra.
    - **Quên import `reflect-metadata`:** Các hàm `Reflect.getMetadata` trả về `undefined`.
    - **Lạm dụng decorators:** Dùng decorator cho mọi thứ có thể làm code khó hiểu và khó debug. Đôi khi một hàm bậc cao hoặc composition đơn giản là đủ.
    - **Hiểu lầm về thời điểm decorator chạy:** Chúng chạy khi class được định nghĩa, không phải khi instance được tạo.
    - **Cố gắng thay đổi giá trị property từ property decorator:** Điều này không được hỗ trợ trực tiếp. Bạn có thể cần sử dụng accessor decorator hoặc kết hợp với logic trong constructor/methods.
    - **Thứ tự decorator không như mong đợi:** Dẫn đến hành vi sai.
    - **Circular dependencies liên quan đến metadata:** Module A dùng decorator đọc metadata từ module B, module B lại dùng decorator đọc metadata từ module A.

8.  **So sánh, Đánh giá và Lựa chọn:**

    - **Decorators vs. Higher-Order Functions (HOF):**
      - Cả hai đều có thể dùng để mở rộng hoặc thay đổi hành vi của hàm/class.
      - **Decorators:** Cú pháp khai báo (`@`), tích hợp sâu hơn với class. Tốt cho việc thêm metadata và các cross-cutting concerns ở mức class/member.
      - **HOF:** Cách tiếp cận truyền thống hơn trong JavaScript, linh hoạt hơn cho các hàm độc lập.
      - Lựa chọn phụ thuộc vào ngữ cảnh và sở thích. Decorators thường gọn gàng hơn cho các tác vụ liên quan đến class trong OOP.
    - **Decorators vs. Mixins (hoặc Traits):**
      - **Mixins:** Một cách để "trộn" các hành vi từ nhiều nguồn vào một class. TypeScript có một số pattern để implement mixins.
      - **Decorators:** Có thể được dùng để _áp dụng_ một số khía cạnh của mixin, nhưng chúng không phải là cơ chế mixin đầy đủ.
      - Thường bổ sung cho nhau hơn là thay thế.

9.  **Bài tập thực hành và Gợi ý dự án nhỏ:**

    - **Câu hỏi lý thuyết:**
      1.  Decorator trong TypeScript là gì? Giải thích ngắn gọn mục đích của chúng.
      2.  Liệt kê các loại decorator trong TypeScript và mô tả ngắn gọn chúng được áp dụng ở đâu.
      3.  Làm thế nào bạn có thể truyền tham số tùy chỉnh vào một decorator?
      4.  `reflect-metadata` là gì và tại sao nó thường được sử dụng cùng với decorators?
      5.  Thứ tự các decorator được đánh giá và thực thi như thế nào?
    - **Bài tập gỡ lỗi (Debugging exercises):**

      ```typescript
      // import "reflect-metadata"; // Giả sử quên import dòng này

      // function Deprecated(reason: string) {
      //   return function(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
      //     // descriptor.value = function(...args: any[]) { // Lỗi: nếu target là property thì không có descriptor.value
      //     //   console.warn(`Method ${propertyKey} is deprecated: ${reason}`);
      //     //   return target[propertyKey].apply(this, args);
      //     // };
      //     console.warn(`Member ${propertyKey} is deprecated: ${reason}`);
      //   }
      // }
      /*
      class OldApiService {
          // @Deprecated("Use newMethod instead") // Áp dụng cho property thay vì method
          oldProperty: string = "old data";
      
          // @Deprecated("Use anotherMethod instead")
          oldMethod() {
              console.log("Executing oldMethod with " + this.oldProperty);
          }
      }
      // new OldApiService().oldMethod();
      */
      ```

    - **Thử thách code (Coding challenges):**
      1.  **Tạo một method decorator `@CacheResult(duration: number)`:**
          - Decorator này sẽ cache kết quả của method được trang trí trong `duration` (miliseconds).
          - Nếu method được gọi lại trong khoảng thời gian cache với cùng tham số, nó sẽ trả về kết quả đã cache thay vì thực thi lại method.
          - Gợi ý: Sử dụng `Map` để lưu cache, key có thể là `JSON.stringify(args)`.
      2.  **Tạo một property decorator `@DefaultValue(value: any)`:**
          - Decorator này sẽ gán giá trị `value` cho property nếu property đó là `undefined` khi instance được tạo.
          - Gợi ý: Bạn không thể thay đổi giá trị trực tiếp trong property decorator. Hãy xem xét việc sửa đổi constructor của class (bằng class decorator) hoặc sử dụng accessor decorator. Cách đơn giản hơn là dùng class decorator để duyệt qua các property đã được đánh dấu metadata và gán giá trị trong constructor.
      3.  **Tạo một parameter decorator `@ValidateType(expectedType: Function)`:**
          - Sử dụng `reflect-metadata` để lưu trữ `expectedType`.
          - Tạo một method decorator `@ValidateParams` để kiểm tra kiểu của các tham số đã được đánh dấu `@ValidateType` trước khi method thực thi. Nếu kiểu không khớp, throw error.
    - **Gợi ý dự án nhỏ (tiếp tục dự án Quản lý công việc CLI):**
      - Tạo decorator `@ConfirmExecution(message: string)` cho các command nguy hiểm (ví dụ: delete all tasks). Decorator này sẽ hiển thị `message` và yêu cầu người dùng xác nhận (y/n) trước khi thực thi command.
      - Sử dụng property decorator và `reflect-metadata` để đánh dấu các thuộc tính của `Task` cần được hiển thị trong bảng output, và một hàm tiện ích sẽ đọc metadata này để format output.

10. **Câu hỏi phỏng vấn thường gặp:**
    - "What are decorators in TypeScript? How do they work?"
    - "Can you list the different types of decorators and their use cases?"
    - "How do you pass arguments to a decorator?"
    - "Explain the execution order of decorators."
    - "What is `reflect-metadata` and why is it often used with decorators? Give an example."
    - "How would you implement a simple logging decorator for a method?"
    - "What are some limitations or pitfalls of using decorators?"
    - "Are decorators a JavaScript feature or a TypeScript-specific feature?" (Trả lời: Là proposal của JS, TS triển khai trước. Hiện tại (2023-2024) JS Decorators đã ở Stage 3).

---

Decorators là một công cụ mạnh mẽ, nhưng cần được sử dụng một cách cẩn thận và có hiểu biết. Chúng thực sự tỏa sáng trong các framework để giảm thiểu code lặp lại và cho phép cấu hình theo kiểu khai báo. Hãy thử nghiệm và làm quen với chúng. Khi bạn đã sẵn sàng, chúng ta sẽ khám phá các khía cạnh nâng cao khác hoặc bắt đầu áp dụng kiến thức vào xây dựng backend thực tế.

Tuyệt vời! Chúng ta đã đi qua Decorators, một công cụ mạnh mẽ cho metaprogramming. Bây giờ, hãy đào sâu hơn vào một khía cạnh khác của hệ thống kiểu TypeScript mà chúng ta đã chạm tới ở Phần 2, nhưng còn rất nhiều điều thú vị để khám phá: **Generics Nâng Cao, Conditional Types và Mapped Types chi tiết.** Đây là những công cụ giúp bạn viết code TypeScript cực kỳ linh hoạt và an toàn kiểu, đặc biệt hữu ích khi xây dựng các thư viện, tiện ích hoặc các thành phần backend phức tạp.

---

### **PHẦN 6: Generics Nâng Cao, Conditional Types và Mapped Types - Xây Dựng Kiểu Linh Hoạt và Tái Sử Dụng Tối Đa**

1.  **Tên phần học:** Generics Nâng Cao, Conditional Types và Mapped Types - Xây Dựng Kiểu Linh Hoạt và Tái Sử Dụng Tối Đa.

2.  **Mục tiêu học phần:**

    - Nắm vững các kỹ thuật nâng cao với **Generics**:
      - Sử dụng Generic Constraints (ràng buộc generic) một cách hiệu quả.
      - Sử dụng Type Parameters trong Generic Constraints.
      - Hiểu và áp dụng Generic Defaults.
    - Làm chủ **Mapped Types**:
      - Hiểu sâu hơn về cú pháp `[P in keyof T]`.
      - Sử dụng Key Remapping thông qua `as`.
      - Kết hợp Mapped Types với Conditional Types để tạo ra các biến đổi kiểu phức tạp.
      - Hiểu các Modifiers trong Mapped Types (`+` hoặc `-` cho `readonly` và `?`).
    - Làm chủ **Conditional Types**:
      - Hiểu rõ cú pháp `T extends U ? X : Y`.
      - Sử dụng từ khóa `infer` để suy luận kiểu trong Conditional Types.
      - Ứng dụng Conditional Types để tạo các kiểu phụ thuộc, phân phối trên union types (Distributive Conditional Types).
      - Xây dựng các Utility Types tùy chỉnh phức tạp.
    - Kết hợp các khái niệm trên để giải quyết các bài toán về kiểu phức tạp trong thực tế.
    - Trả lời được:
      - Làm thế nào để ràng buộc một type parameter phải có một thuộc tính cụ thể hoặc kế thừa từ một kiểu khác?
      - Mapped types có thể làm gì ngoài việc làm cho các thuộc tính thành `readonly` hoặc `optional`?
      - Từ khóa `infer` trong conditional types dùng để làm gì? Cho ví dụ.
      - Distributive conditional types hoạt động như thế nào?
      - Làm thế nào để tạo một utility type tùy chỉnh như `ExtractPropertiesOfType<T, U>` (trích xuất các thuộc tính từ `T` có kiểu là `U`)?

3.  **Kiến thức tiên quyết:**

    - Hoàn thành Phần 1, 2 (đặc biệt là Generics cơ bản, Union/Intersection Types, `keyof`, `typeof`).
    - Hiểu biết về các Utility Types cơ bản (`Partial`, `Readonly`, `Pick`, `Omit`, `Record`).
    - Sự thoải mái với logic và tư duy trừu tượng.

4.  **Giải thích lý thuyết chuyên sâu:**

    - **Generics Nâng Cao:**

      - **Generic Constraints (Đã giới thiệu ở Phần 2, đào sâu hơn):**

        - Đảm bảo type parameter `T` có những thuộc tính hoặc phương thức nhất định.

        ```typescript
        interface WithLogging {
          log: (message: string) => void;
        }

        function processAndLog<T extends WithLogging>(item: T): void {
          item.log("Processing item...");
          // Do something with item
          item.log("Item processed.");
        }

        class ProductItem implements WithLogging {
          constructor(public name: string) {}
          log(message: string) {
            console.log(`[Product: ${this.name}] ${message}`);
          }
        }
        class UserItem {
          // Không implement WithLogging
          constructor(public username: string) {}
        }

        processAndLog(new ProductItem("Laptop"));
        // processAndLog(new UserItem("Alice")); // Lỗi: Argument of type 'UserItem' is not assignable to parameter of type 'WithLogging'. Property 'log' is missing in type 'UserItem'.
        ```

      - **Sử dụng Type Parameters trong Generic Constraints:**
        Một type parameter có thể bị ràng buộc bởi một type parameter khác.
        ```typescript
        // Lấy giá trị của một thuộc tính từ một object, đảm bảo key tồn tại trong object
        function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
          return obj[key];
        }
        let user = { name: "Alice", age: 30 };
        let userName = getProperty(user, "name"); // userName là string
        let userAge = getProperty(user, "age"); // userAge là number
        // let userAddress = getProperty(user, "address"); // Lỗi: Argument of type '"address"' is not assignable to parameter of type '"name" | "age"'.
        ```
        **"TẠI SAO" quan trọng?** Đảm bảo type safety khi làm việc với các thuộc tính động của object.
      - **Generic Defaults (Giá trị mặc định cho Type Parameter):**
        Cho phép bạn chỉ định một kiểu mặc định cho type parameter nếu nó không được cung cấp khi sử dụng generic.

        ```typescript
        interface Container<T = string> {
          // T mặc định là string
          value: T;
        }

        let stringContainer: Container = { value: "Hello" }; // T là string (mặc định)
        console.log(stringContainer.value.toUpperCase());

        let numberContainer: Container<number> = { value: 123 }; // T được cung cấp là number
        console.log(numberContainer.value.toFixed(2));

        // let booleanContainer: Container = { value: true }; // Lỗi: Type 'boolean' is not assignable to type 'string'.
        // Vì T mặc định là string, không phải boolean.
        // Phải ghi rõ: let booleanContainer: Container<boolean> = { value: true };
        ```

        **"KHI NÀO" dùng Generic Defaults?** Khi bạn muốn cung cấp một trường hợp sử dụng phổ biến cho generic của mình, giảm thiểu sự dài dòng cho người dùng trong trường hợp đó.

    - **Mapped Types (Chi tiết hơn):**

      - Cho phép tạo kiểu mới dựa trên các thuộc tính của một kiểu hiện có.
      - Cú pháp: `{ [P in K]: NewType }`
        - `K`: Thường là `keyof T` (một union của các key của `T`).
        - `P`: Type parameter, lặp qua từng key trong `K`.
        - `NewType`: Kiểu mới cho thuộc tính `P`. Có thể phụ thuộc vào `T[P]` (kiểu gốc của thuộc tính).
      - **Mapping Modifiers (`readonly` và `?`):**

        - Có thể thêm hoặc bớt `readonly` và `?` (optional) cho các thuộc tính trong mapped type.
        - Sử dụng `+` (mặc định) hoặc `-` để kiểm soát.

        ```typescript
        interface User {
          id: number;
          name: string;
          email?: string;
          readonly role: string;
        }

        // Làm tất cả thuộc tính thành optional
        type MyPartial<T> = {
          [P in keyof T]?: T[P]; // Tương đương Partial<T>
        };
        type PartialUser = MyPartial<User>; // { id?: number; name?: string; email?: string; role?: string; }

        // Làm tất cả thuộc tính thành required (bỏ optional)
        type MyRequired<T> = {
          [P in keyof T]-?: T[P]; // Tương đương Required<T>
        };
        type RequiredUser = MyRequired<User>; // { id: number; name: string; email: string; role: string; } (email không còn optional)

        // Làm tất cả thuộc tính thành readonly
        type MyReadonly<T> = {
          readonly [P in keyof T]: T[P]; // Tương đương Readonly<T>
        };

        // Bỏ readonly khỏi tất cả thuộc tính
        type Mutable<T> = {
          -readonly [P in keyof T]: T[P];
        };
        type MutableUser = Mutable<User>; // role không còn readonly
        // type: { id: number; name: string; email?: string | undefined; role: string; }
        ```

      - **Key Remapping via `as` (Đổi tên Key):**

        - Cho phép tạo thuộc tính mới với tên khác dựa trên các key gốc.
        - **"TẠI SAO" Key Remapping?** Để tạo ra các API hoặc cấu trúc dữ liệu mới từ các kiểu hiện có, ví dụ như thêm prefix/suffix cho key, hoặc tạo các "getter" methods.

        ```typescript
        interface Person {
          name: string;
          age: number;
        }

        // Tạo kiểu với các key là getter methods
        type Getters<T> = {
          [P in keyof T as `get${Capitalize<string & P>}`]: () => T[P];
        };
        // Capitalize<string & P>: `string & P` để đảm bảo P là string (vì keyof T có thể là symbol, number)
        // Capitalize là một built-in string manipulation type (cùng với Uppercase, Lowercase, Uncapitalize)

        type PersonGetters = Getters<Person>;
        // type PersonGetters = {
        //   getName: () => string;
        //   getAge: () => number;
        // }

        const personInstance: Person = { name: "Alice", age: 30 };
        const personGetters: PersonGetters = {
          getName: () => personInstance.name,
          getAge: () => personInstance.age,
        };
        console.log(personGetters.getName()); // Alice

        // Lọc key: Chỉ giữ lại các key là string và tạo event handler names
        type EventHandlers<T> = {
          [K in keyof T as K extends string
            ? `on${Capitalize<K>}Change`
            : never]: (value: T[K]) => void;
        };
        interface Settings {
          theme: "dark" | "light";
          fontSize: number;
          showNotifications: boolean;
          // [Symbol.iterator]?: any; // Key không phải string sẽ bị loại bởi 'never'
        }
        type SettingsEventHandlers = EventHandlers<Settings>;
        // type SettingsEventHandlers = {
        //     onThemeChange: (value: "dark" | "light") => void;
        //     onFontSizeChange: (value: number) => void;
        //     onShowNotificationsChange: (value: boolean) => void;
        // }
        ```

        Nếu `as` clause trả về `never`, thuộc tính đó sẽ bị loại bỏ khỏi mapped type.

    - **Conditional Types (Kiểu điều kiện):**

      - Cho phép chọn một trong hai kiểu dựa trên một điều kiện kiểm tra kiểu.
      - Cú pháp: `SomeType extends OtherType ? TrueType : FalseType;`
      - **"TẠI SAO" Conditional Types?** Để tạo các kiểu phụ thuộc vào mối quanah giữa các kiểu khác, cho phép logic kiểu phức tạp, overload functions dựa trên kiểu, và xây dựng utility types mạnh mẽ.
      - **Ví dụ cơ bản:**
        ```typescript
        type IsString<T> = T extends string ? true : false;
        type A = IsString<string>; // true
        type B = IsString<number>; // false
        ```
      - **Từ khóa `infer`:**

        - Sử dụng bên trong `extends` clause của một conditional type để **suy luận (infer) kiểu** từ một phần của kiểu đang được kiểm tra.
        - Kiểu được suy luận có thể được sử dụng trong nhánh `TrueType` của conditional type.
        - **"TẠI SAO" `infer`?** Rất mạnh mẽ để "bóc tách" các phần của một kiểu phức tạp (như kiểu trả về của hàm, kiểu phần tử của mảng, kiểu của Promise...).
        - **Ví dụ: Lấy kiểu trả về của hàm (`ReturnType` utility type):**

          ```typescript
          // Cách triển khai ReturnType
          type MyReturnType<T extends (...args: any) => any> = T extends (
            ...args: any
          ) => infer R
            ? R
            : any;
          // Nếu T là một hàm (...args: any) => infer R (suy luận kiểu trả về là R)
          // Thì kết quả là R
          // Ngược lại là any (hoặc never)

          function getString(): string {
            return "hello";
          }
          function getNumber(): number {
            return 123;
          }
          function getVoid(): void {}

          type StrReturn = MyReturnType<typeof getString>; // string
          type NumReturn = MyReturnType<typeof getNumber>; // number
          type VoidReturn = MyReturnType<typeof getVoid>; // void
          // type NotAFunction = MyReturnType<string>; // Lỗi: Type 'string' does not satisfy the constraint '(...args: any) => any'.
          ```

        - **Ví dụ: Lấy kiểu phần tử của mảng:**

          ```typescript
          type ElementTypeOfArray<T> = T extends (infer E)[] ? E : never;
          type ElementTypeOfPromise<T> = T extends Promise<infer V> ? V : never;

          type ItemType1 = ElementTypeOfArray<string[]>; // string
          type ItemType2 = ElementTypeOfArray<number[]>; // number
          type ItemType3 = ElementTypeOfArray<{ id: number }[]>; // { id: number }
          type ItemType4 = ElementTypeOfArray<string>; // never (string không phải là array)

          type PromiseValue1 = ElementTypeOfPromise<Promise<boolean>>; // boolean
          type PromiseValue2 = ElementTypeOfPromise<Promise<Date>>; // Date
          ```

      - **Distributive Conditional Types (Kiểu điều kiện phân phối):**

        - Khi một conditional type có dạng `NakedTypeParameter extends U ? X : Y` (trong đó `NakedTypeParameter` là một type parameter không bị "gói" trong kiểu khác như `T[]` hay `Promise<T>`), và bạn truyền vào một **union type** cho `NakedTypeParameter`, conditional type đó sẽ được **áp dụng riêng lẻ cho từng thành viên của union type**.
        - **"TẠI SAO" Distributive Conditional Types?** Để thực hiện các phép biến đổi hoặc lọc trên từng thành viên của một union.
        - **Ví dụ: `Exclude<T, U>` và `Extract<T, U>` built-in utility types:**

          ```typescript
          // Cách triển khai Exclude
          type MyExclude<T, U> = T extends U ? never : T;
          // Nếu T (một thành viên của union T ban đầu) là con của U, loại bỏ nó (never)
          // Ngược lại, giữ lại T

          type AllStatus = "pending" | "processing" | "success" | "failed";
          type NonTerminalStatus = MyExclude<AllStatus, "success" | "failed">;
          // NonTerminalStatus là "pending" | "processing"
          // Cách hoạt động:
          // "pending" extends "success"|"failed" ? never : "pending" -> "pending"
          // "processing" extends "success"|"failed" ? never : "processing" -> "processing"
          // "success" extends "success"|"failed" ? never : "success" -> never
          // "failed" extends "success"|"failed" ? never : "failed" -> never
          // Kết quả: "pending" | "processing"

          // Cách triển khai Extract
          type MyExtract<T, U> = T extends U ? T : never;
          type TerminalStatus = MyExtract<
            AllStatus,
            "success" | "failed" | "archived"
          >;
          // TerminalStatus là "success" | "failed"
          ```

        - **Ngăn chặn Distributive Behavior:** Nếu bạn không muốn conditional type phân phối trên union, hãy "gói" type parameter trong `[]`:

          ```typescript
          type ToArray<T> = T extends any ? T[] : never; // Phân phối
          type R1 = ToArray<string | number>; // string[] | number[]

          type ToArrayNonDist<T> = [T] extends [any] ? T[] : never; // Không phân phối
          type R2 = ToArrayNonDist<string | number>; // (string | number)[]
          ```

    - **Kết hợp các khái niệm để xây dựng Utility Types tùy chỉnh phức tạp:**

      - **Ví dụ 1: `FunctionPropertyNames<T>` - Lấy tên các thuộc tính là hàm của kiểu `T`**

        ```typescript
        type FunctionPropertyNames<T> = {
          [K in keyof T]: T[K] extends Function ? K : never;
        }[keyof T]; // Lấy các giá trị không phải 'never' từ mapped type

        interface MyService {
          id: number;
          getName(): string;
          process(data: any): void;
          config: object;
        }
        type ServiceFunctionNames = FunctionPropertyNames<MyService>;
        // ServiceFunctionNames là "getName" | "process"
        ```

        **Giải thích:**

        1.  `[K in keyof T]: T[K] extends Function ? K : never;` tạo ra một mapped type:
            ```
            {
              id: never;          // MyService["id"] (number) không extends Function
              getName: "getName"; // MyService["getName"] (function) extends Function
              process: "process"; // MyService["process"] (function) extends Function
              config: never;        // MyService["config"] (object) không extends Function
            }
            ```
        2.  `[keyof T]` ở cuối (Indexed Access Type) sẽ lấy union của các giá trị của object type trên: `never | "getName" | "process" | never`.
        3.  Union với `never` sẽ loại bỏ `never`, kết quả là `"getName" | "process"`.

      - **Ví dụ 2: `Diff<T, U>` - Lấy các thuộc tính có trong `T` mà không có trong `U`**

        ```typescript
        type Diff<T, U> = Omit<T, keyof U & keyof T>;
        // Hoặc cách khác phức tạp hơn dùng Mapped + Conditional
        // type Diff<T, U> = {
        //   [P in Exclude<keyof T, keyof U>]: T[P];
        // };

        interface PersonDetails {
          id: string;
          name: string;
          age: number;
          email: string;
        }
        interface ContactInfo {
          email: string;
          phone?: string;
        }
        type PersonOnlyDetails = Diff<PersonDetails, ContactInfo>;
        // PersonOnlyDetails sẽ là: { id: string; name: string; age: number; }
        // vì 'email' có trong cả hai nên bị Omit
        ```

      - **Ví dụ 3: `UnwrapPromiseArray<T>` - Nếu T là `Promise<Array<U>>` thì trả về `U`, ngược lại là `T`**

        ```typescript
        type UnwrapPromiseArray<T> = T extends Promise<Array<infer U>> ? U : T;

        type Test1 = UnwrapPromiseArray<Promise<string[]>>; // string
        type Test2 = UnwrapPromiseArray<Promise<number[]>>; // number
        type Test3 = UnwrapPromiseArray<string[]>; // string[] (không phải Promise)
        type Test4 = UnwrapPromiseArray<Promise<string>>; // Promise<string> (Promise nhưng không phải array)
        ```

5.  **Minh họa trực quan và Code mẫu (Đã tích hợp trong phần Giải thích lý thuyết)**

    - **Sơ đồ luồng tư duy khi xây dựng kiểu phức tạp:**

      ```mermaid
      graph TD
          Problem["Bài toán về kiểu cần giải quyết"] --> DefineInput["Xác định kiểu đầu vào (T, U, ...)"];
          DefineInput --> ChooseTool{"Chọn công cụ phù hợp"};

          ChooseTool -- "Cần ràng buộc T?" --> GenericsConstraints["Generics + Constraints"];
          ChooseTool -- "Tạo kiểu mới từ props của T?" --> MappedTypes["Mapped Types (`in keyof T`)"];
          ChooseTool -- "Quyết định kiểu dựa trên điều kiện?" --> ConditionalTypes["Conditional Types (`extends ? :`)"];
          ChooseTool -- "Cần tách/suy luận phần của kiểu?" --> InferKeyword["`infer` trong Conditional"];
          ChooseTool -- "Áp dụng cho từng phần tử union?" --> Distributive["Distributive Conditional"];

          GenericsConstraints --> Combine["Kết hợp các công cụ"];
          MappedTypes --> Combine;
          ConditionalTypes --> Combine;
          InferKeyword --> ConditionalTypes;
          Distributive --> ConditionalTypes;

          Combine --> SolutionType["Kiểu giải pháp"];
          SolutionType --> Test["Kiểm tra với các trường hợp biên"];
      ```

6.  **Best Practices và Quy ước (Conventions):**

    - **Đặt tên rõ ràng cho Type Parameters (`T`, `U`, `K`, `V`, `P` là phổ biến, nhưng tên mô tả hơn như `TItem`, `TKey`, `TReturn` cũng tốt).**
    - **Sử dụng Generic Constraints để làm cho generic types an toàn và dễ hiểu hơn.**
    - **Khi tạo Mapped Types, hãy xem xét các mapping modifiers (`readonly`, `?`) và key remapping (`as`) để đạt được kiểu mong muốn.**
    - **Conditional Types rất mạnh nhưng có thể khó đọc. Chia nhỏ logic phức tạp, comment rõ ràng.**
    - **Tận dụng `infer` để "bóc tách" kiểu một cách hiệu quả.**
    - **Luôn kiểm tra các kiểu phức tạp của bạn với nhiều trường hợp biên (edge cases) để đảm bảo chúng hoạt động đúng như mong đợi.**
    - **Nếu một utility type trở nên quá phức tạp, hãy cân nhắc xem có cách tiếp cận đơn giản hơn không hoặc có thể chia thành nhiều utility type nhỏ hơn.**
    - **Tham khảo các built-in Utility Types của TypeScript (`Partial`, `Readonly`, `Pick`, `Omit`, `Exclude`, `Extract`, `Record`, `ReturnType`, `Parameters`, `NonNullable`...) trước khi tự viết. Chúng thường được tối ưu và kiểm thử kỹ lưỡng.**

7.  **Anti-patterns và Lỗi thường gặp (Common Pitfalls):**

    - **Quá phức tạp hóa kiểu (Over-engineering types):** Cố gắng tạo ra một hệ thống kiểu "hoàn hảo" có thể dẫn đến code rất khó hiểu và bảo trì. Tìm sự cân bằng.
    - **Hiểu sai về Distributive Conditional Types:** Không nhận ra khi nào nó xảy ra hoặc làm thế nào để ngăn chặn nó.
    - **Sử dụng `any` để "thoát" khỏi các vấn đề về kiểu phức tạp:** Làm mất đi lợi ích của TypeScript. Cố gắng giải quyết bằng các công cụ kiểu mạnh hơn.
    - **Lỗi logic trong Conditional Types:** Điều kiện `extends` không đúng hoặc nhánh `TrueType`/`FalseType` không chính xác.
    - **Ràng buộc Generic quá chặt hoặc quá lỏng:** Dẫn đến việc generic không hữu ích hoặc không an toàn.
    - **Quên `keyof T` khi lặp qua các key trong Mapped Types, hoặc `T[P]` khi truy cập kiểu của thuộc tính.**

8.  **So sánh, Đánh giá và Lựa chọn:**

    - **Tự viết Utility Type vs. Sử dụng thư viện (ví dụ: `type-fest`):**
      - **Tự viết:** Giúp hiểu sâu hơn về hệ thống kiểu, không cần thêm dependency.
      - **Thư viện:** Cung cấp nhiều utility type đã được kiểm thử, tiết kiệm thời gian, nhưng thêm dependency.
      - Lựa chọn: Đối với các utility phổ biến và phức tạp, thư viện có thể là lựa chọn tốt. Đối với các nhu cầu cụ thể của dự án, tự viết có thể phù hợp hơn.

9.  **Bài tập thực hành và Gợi ý dự án nhỏ:**

    - **Câu hỏi lý thuyết:**
      1.  Key remapping (`as` clause) trong Mapped Types dùng để làm gì? Cho ví dụ.
      2.  Từ khóa `infer` được sử dụng như thế nào trong Conditional Types? Mục đích chính của nó là gì?
      3.  Giải thích cách Distributive Conditional Types hoạt động. Làm thế nào để ngăn chặn hành vi này?
      4.  Sự khác biệt giữa `[P in keyof T]+?: T[P]` và `[P in keyof T]-?: T[P]` là gì?
      5.  Làm thế nào bạn có thể tạo một kiểu generic `EnsureArray<T>` mà nếu `T` là một mảng thì giữ nguyên, nếu không thì biến `T` thành `T[]`?
    - **Thử thách code (Coding challenges):**
      1.  **`OptionalKeys<T>`:** Viết một utility type lấy ra union của các key trong `T` mà là optional.
          Gợi ý: Một thuộc tính `P` trong `T` là optional nếu `{ [K in P]: T[K] }` có thể gán cho `{ [K in P]?: T[K] }` hoặc `Pick<T,P>` có thể gán cho `Partial<Pick<T,P>>`. Hoặc `{} extends Pick<T,P>`.
          ```typescript
          // type OptionalKeys<T> = { [K in keyof T]-?: {} extends Pick<T, K> ? K : never }[keyof T];
          interface Example {
            a: string;
            b?: number;
            c?: boolean | undefined;
            d: null;
          }
          type OptKeys = OptionalKeys<Example>; // "b" | "c"
          ```
      2.  **`Promisify<F>`:** Viết một utility type nhận vào một hàm `F` và trả về một hàm mới có cùng tham số nhưng kiểu trả về là `Promise` của kiểu trả về gốc của `F`.
          Ví dụ: Nếu `F` là `(a: string, b: number) => boolean`, thì `Promisify<F>` là `(a: string, b: number) => Promise<boolean>`.
      3.  **`MutableDeep<T>`:** Viết một utility type làm cho tất cả các thuộc tính của `T` (và các object lồng nhau) trở thành `mutable` (bỏ `readonly`).
      4.  **`PathValue<T, P>`:** Viết một utility type `PathValue<T, P>` để lấy kiểu của giá trị tại một đường dẫn `P` (ví dụ: `P = 'user.address.street'`) bên trong object `T`. Nếu đường dẫn không hợp lệ, trả về `never`.
          Đây là một bài tập rất khó, có thể cần dùng recursive conditional/mapped types.
    - **Gợi ý dự án nhỏ (tiếp tục dự án Quản lý công việc CLI):**
      - Tạo một utility type `FilterableTaskKeys` để lấy ra các key của `Task` mà có thể dùng để lọc (ví dụ: chỉ các key có kiểu là `string` hoặc `boolean` hoặc `Date`).
      - Xây dựng một hàm generic `createUpdater<T>(initialState: T)` trả về một object có phương thức `update(changes: DeepPartial<T>)` và `getState(): T`. `DeepPartial<T>` là một utility type làm tất cả các thuộc tính của `T` (và các object lồng nhau) thành optional.

10. **Câu hỏi phỏng vấn thường gặp:**
    - "Explain what Mapped Types are in TypeScript and provide an example of how to create a type where all properties are strings."
    - "What are Conditional Types? How does the `infer` keyword work within them?"
    - "Can you explain Distributive Conditional Types? How would you write a type like `ToStringArray<T>` that converts `string | number` to `string[] | number[]`?"
    - "How would you implement a utility type `DeepReadonly<T>` that makes all properties of `T` and its nested objects readonly?"
    - "What are some advanced use cases for combining Generics, Mapped Types, and Conditional Types?"
    - "Given an object type, how would you create a new type that only includes properties of a certain type (e.g., only string properties)?"
    - "How can you use `as` for key remapping in Mapped Types?"

---

Đây là một phần rất nâng cao và đòi hỏi nhiều thực hành để làm chủ. Tuy nhiên, việc hiểu rõ các công cụ này sẽ mở ra khả năng viết code TypeScript cực kỳ biểu cảm và an toàn. Hãy dành thời gian thử nghiệm, xây dựng các utility type của riêng bạn. Khi bạn cảm thấy vững vàng, chúng ta sẽ tiếp tục khám phá các chủ đề khác, có thể là các design patterns trong TypeScript hoặc bắt đầu với một framework backend.

Chắc chắn rồi! Sau khi đã nắm vững các công cụ kiểu nâng cao của TypeScript, giờ là lúc chúng ta chuyển sang cách áp dụng chúng và các nguyên tắc thiết kế phần mềm vào việc xây dựng các ứng dụng backend có cấu trúc tốt. **Design Patterns (Mẫu Thiết Kế)** và các **Nguyên tắc SOLID** là nền tảng cho việc này.

---

### **PHẦN 7: Design Patterns và Nguyên tắc SOLID trong TypeScript Backend**

1.  **Tên phần học:** Design Patterns và Nguyên tắc SOLID trong TypeScript Backend.

2.  **Mục tiêu học phần:**

    - Hiểu rõ khái niệm **Design Patterns** là gì, tại sao chúng quan trọng và lợi ích của việc sử dụng chúng.
    - Nắm vững và biết cách triển khai một số **Creational Patterns (Mẫu Tạo Dựng)** phổ biến trong TypeScript:
      - Singleton
      - Factory Method
      - Abstract Factory
      - Builder
    - Nắm vững và biết cách triển khai một số **Structural Patterns (Mẫu Cấu Trúc)** phổ biến trong TypeScript:
      - Adapter
      - Decorator (Design Pattern, phân biệt với Decorator feature của TS)
      - Facade
      - Proxy
    - Nắm vững và biết cách triển khai một số **Behavioral Patterns (Mẫu Hành Vi)** phổ biến trong TypeScript:
      - Observer
      - Strategy
      - Command
      - Chain of Responsibility
      - Template Method
    - Hiểu sâu và biết cách áp dụng 5 **Nguyên tắc SOLID** trong thiết kế hướng đối tượng với TypeScript:
      - **S**ingle Responsibility Principle (SRP)
      - **O**pen/Closed Principle (OCP)
      - **L**iskov Substitution Principle (LSP)
      - **I**nterface Segregation Principle (ISP)
      - **D**ependency Inversion Principle (DIP)
    - Nhận biết khi nào nên và không nên áp dụng một Design Pattern cụ thể.
    - Liên kết các Design Patterns với các Nguyên tắc SOLID.
    - Trả lời được:
      - Design Pattern là gì? Tại sao nên sử dụng chúng?
      - Trình bày và cho ví dụ về cách triển khai một số Design Pattern cụ thể (ví dụ: Factory, Observer, Strategy).
      - Năm nguyên tắc SOLID là gì? Giải thích từng nguyên tắc và cho ví dụ minh họa bằng TypeScript.
      - Làm thế nào để áp dụng các nguyên tắc SOLID khi thiết kế class và module trong backend?
      - Mối quan hệ giữa Design Patterns và SOLID là gì?

3.  **Kiến thức tiên quyết:**

    - Hoàn thành Phần 1-6 (đặc biệt là OOP với TypeScript, Modules, Generics).
    - Kinh nghiệm thực tế xây dựng ứng dụng (bất kỳ ngôn ngữ nào) sẽ giúp dễ hình dung bối cảnh áp dụng.

4.  **Giải thích lý thuyết chuyên sâu:**

    - **Design Patterns (Mẫu Thiết Kế):**

      - **Là gì?** Design Patterns là các giải pháp đã được thử nghiệm, tái sử dụng được cho các vấn đề thường gặp trong thiết kế phần mềm trong một ngữ cảnh nhất định. Chúng không phải là code cụ thể, mà là các mô tả hoặc khuôn mẫu về cách các class và object tương tác với nhau để giải quyết một vấn đề. Chúng được đúc kết từ kinh nghiệm của nhiều nhà phát triển. Cuốn sách kinh điển là "Design Patterns: Elements of Reusable Object-Oriented Software" của "Gang of Four" (GoF).
      - **"TẠI SAO" Design Patterns quan trọng?**
        1.  **Giải pháp đã được chứng minh:** Cung cấp các giải pháp đã được kiểm chứng cho các vấn đề phổ biến, giúp tránh "phát minh lại bánh xe".
        2.  **Ngôn ngữ chung:** Cung cấp một vốn từ vựng chung cho các nhà phát triển để thảo luận về các giải pháp thiết kế.
        3.  **Cải thiện thiết kế:** Giúp tạo ra các hệ thống linh hoạt hơn, dễ bảo trì, dễ mở rộng và dễ hiểu hơn.
        4.  **Tăng tốc độ phát triển:** Giúp giải quyết vấn đề nhanh hơn bằng cách áp dụng một mẫu đã biết.
      - **Phân loại Design Patterns (theo GoF):**
        1.  **Creational Patterns (Mẫu Tạo Dựng):** Liên quan đến quá trình khởi tạo đối tượng. Giúp tạo đối tượng một cách linh hoạt, ẩn đi logic tạo đối tượng phức tạp.
        2.  **Structural Patterns (Mẫu Cấu Trúc):** Liên quan đến cách các class và object được kết hợp để tạo thành các cấu trúc lớn hơn, linh hoạt hơn.
        3.  **Behavioral Patterns (Mẫu Hành Vi):** Liên quan đến thuật toán và việc gán trách nhiệm giữa các đối tượng, tập trung vào cách các đối tượng giao tiếp và hợp tác.

    - **Nguyên tắc SOLID:**

      - Là năm nguyên tắc thiết kế cơ bản trong lập trình hướng đối tượng, nhằm mục đích làm cho thiết kế phần mềm dễ hiểu hơn, linh hoạt hơn và dễ bảo trì hơn. Được giới thiệu bởi Robert C. Martin ("Uncle Bob").
      - **"TẠI SAO" SOLID quan trọng?**
        - Hướng dẫn việc tạo ra các hệ thống phần mềm chất lượng cao.
        - Giảm sự phụ thuộc chặt chẽ (tight coupling), tăng tính gắn kết (high cohesion).
        - Dễ dàng hơn cho việc refactor, mở rộng và kiểm thử.

      1.  **S - Single Responsibility Principle (SRP - Nguyên tắc Đơn trách nhiệm):**

          - **Phát biểu:** Một class chỉ nên có một lý do duy nhất để thay đổi, nghĩa là nó chỉ nên có một trách nhiệm (một công việc) duy nhất.
          - **"TẠI SAO"?**
            - Giảm sự phức tạp của class.
            - Tăng tính dễ hiểu và dễ bảo trì.
            - Thay đổi một trách nhiệm không ảnh hưởng đến các trách nhiệm khác.
            - Tăng khả năng tái sử dụng của class.
          - **Ví dụ (TypeScript):**

            ```typescript
            // Anti-pattern: Class User làm quá nhiều việc
            class UserBAD {
              constructor(public name: string, public email: string) {}

              saveToDatabase() {
                /* ... logic lưu vào DB ... */ console.log(
                  `User ${this.name} saved.`
                );
              }
              sendWelcomeEmail() {
                /* ... logic gửi email ... */ console.log(
                  `Welcome email sent to ${this.email}.`
                );
              }
              validateEmail(): boolean {
                /* ... logic validate ... */ return this.email.includes("@");
              }
            }

            // SRP: Tách thành các class riêng
            class User {
              constructor(public name: string, public email: string) {}
            }

            class UserRepository {
              save(user: User) {
                /* ... */ console.log(
                  `User ${user.name} saved by UserRepository.`
                );
              }
            }

            class EmailService {
              sendWelcome(email: string) {
                /* ... */ console.log(
                  `Welcome email sent to ${email} by EmailService.`
                );
              }
            }

            class UserValidator {
              isValidEmail(email: string): boolean {
                return email.includes("@");
              }
            }

            const user = new User("Alice", "alice@example.com");
            new UserRepository().save(user);
            new EmailService().sendWelcome(user.email);
            ```

      2.  **O - Open/Closed Principle (OCP - Nguyên tắc Đóng/Mở):**

          - **Phát biểu:** Các thực thể phần mềm (class, module, function, etc.) nên **mở cho việc mở rộng (open for extension)**, nhưng **đóng cho việc sửa đổi (closed for modification)**.
          - **"TẠI SAO"?**
            - Cho phép thêm chức năng mới mà không cần thay đổi code hiện có đã được kiểm thử kỹ lưỡng.
            - Giảm rủi ro gây ra lỗi trong code cũ.
            - Tăng tính ổn định và khả năng bảo trì.
          - Thường đạt được thông qua kế thừa (abstract classes/methods) và đa hình (interfaces).
          - **Ví dụ (TypeScript):**

            ```typescript
            // Anti-pattern: Phải sửa đổi hàm khi có loại hình mới
            // class ShapeBAD { constructor(public type: 'circle' | 'square') {} }
            // function calculateAreaBAD(shapes: ShapeBAD[]): number {
            //   let totalArea = 0;
            //   for (const shape of shapes) {
            //     if (shape.type === 'circle') { /* calc circle area */ }
            //     else if (shape.type === 'square') { /* calc square area */ }
            //     // Nếu thêm Triangle, phải sửa hàm này
            //   }
            //   return totalArea;
            // }

            // OCP: Sử dụng interface và đa hình
            interface Shape {
              getArea(): number;
            }

            class Circle implements Shape {
              constructor(public radius: number) {}
              getArea(): number {
                return Math.PI * this.radius * this.radius;
              }
            }

            class Square implements Shape {
              constructor(public side: number) {}
              getArea(): number {
                return this.side * this.side;
              }
            }

            // Hàm này đóng với việc sửa đổi, mở với việc thêm Shape mới
            function calculateTotalArea(shapes: Shape[]): number {
              let totalArea = 0;
              for (const shape of shapes) {
                totalArea += shape.getArea(); // Không cần biết loại Shape cụ thể
              }
              return totalArea;
            }

            // Thêm Triangle mà không cần sửa calculateTotalArea
            class Triangle implements Shape {
              constructor(public base: number, public height: number) {}
              getArea(): number {
                return 0.5 * this.base * this.height;
              }
            }

            const shapes: Shape[] = [
              new Circle(5),
              new Square(4),
              new Triangle(3, 6),
            ];
            console.log("Total Area (OCP):", calculateTotalArea(shapes));
            ```

      3.  **L - Liskov Substitution Principle (LSP - Nguyên tắc Thay thế Liskov):**

          - **Phát biểu:** Các đối tượng của một class cha (superclass) phải có thể được thay thế bằng các đối tượng của class con (subclass) mà không làm thay đổi tính đúng đắn của chương trình. Nói cách khác, class con phải hoàn toàn có thể thay thế class cha.
          - **"TẠI SAO"?**
            - Đảm bảo tính nhất quán và đúng đắn của hệ thống phân cấp kế thừa.
            - Nếu LSP bị vi phạm, đa hình sẽ không hoạt động đúng, và bạn sẽ phải dùng `instanceof` để kiểm tra kiểu cụ thể, phá vỡ OCP.
          - Class con không nên:
            - Làm yếu đi các điều kiện tiên quyết (preconditions) của phương thức cha.
            - Làm mạnh hơn các điều kiện hậu quyết (postconditions) của phương thức cha.
            - Throw các exception mới mà class cha không throw.
          - **Ví dụ (TypeScript):**

            ```typescript
            class Bird {
              fly(): void {
                console.log("Bird is flying");
              }
            }

            class Sparrow extends Bird {
              // OK, Sparrow có thể bay
            }

            class Penguin extends Bird {
              // Vi phạm LSP nếu Penguin không thể bay thực sự
              fly(): void {
                // throw new Error("Penguins can't fly!"); // Hoặc không làm gì cả
                console.log("Penguin attempts to fly but waddles.");
              }
            }

            function makeBirdFly(bird: Bird): void {
              try {
                bird.fly();
              } catch (e: any) {
                console.error(e.message);
              }
            }

            makeBirdFly(new Sparrow()); // Bird is flying
            makeBirdFly(new Penguin()); // Penguin attempts to fly but waddles. (Nếu throw error thì sẽ bị bắt)

            // Vấn đề của LSP ở đây là `Penguin` "nói dối" rằng nó là một `Bird` có thể `fly` theo cách thông thường.
            // Giải pháp tốt hơn có thể là không cho Penguin kế thừa Bird nếu "fly" là hành vi cốt lõi của Bird.
            // Hoặc tách interface:
            interface Flyer {
              fly(): void;
            }
            interface Swimmer {
              swim(): void;
            }

            class GenericBird {} // Có thể có các thuộc tính chung của chim
            class FlyingBird extends GenericBird implements Flyer {
              fly() {
                console.log("Flying bird is soaring");
              }
            }
            class PenguinLSP extends GenericBird implements Swimmer {
              swim() {
                console.log("Penguin is swimming gracefully");
              }
              // Không implement Flyer
            }
            ```

      4.  **I - Interface Segregation Principle (ISP - Nguyên tắc Phân tách Interface):**

          - **Phát biểu:** Client không nên bị buộc phải phụ thuộc vào các phương thức (trong interface) mà chúng không sử dụng. Nên tạo ra các interface nhỏ, cụ thể cho từng client thay vì một interface lớn, chung chung.
          - **"TẠI SAO"?**
            - Giảm sự phụ thuộc không cần thiết.
            - Tăng tính gắn kết của interface.
            - Dễ dàng hơn cho client để implement chỉ những gì chúng cần.
          - **Ví dụ (TypeScript):**

            ```typescript
            // Anti-pattern: Interface lớn
            interface WorkerBAD {
              work(): void;
              eat(): void;
              sleep(): void;
            }

            class HumanWorkerBAD implements WorkerBAD {
              work() {
                console.log("Human working...");
              }
              eat() {
                console.log("Human eating...");
              }
              sleep() {
                console.log("Human sleeping...");
              }
            }

            class RobotWorkerBAD implements WorkerBAD {
              work() {
                console.log("Robot working...");
              }
              eat() {
                /* Robot không ăn! Buộc phải implement rỗng hoặc throw error */
              }
              sleep() {
                /* Robot không ngủ! */
              }
            }

            // ISP: Tách thành các interface nhỏ hơn
            interface Workable {
              work(): void;
            }
            interface Eatable {
              eat(): void;
            }
            interface Sleepable {
              sleep(): void;
            }

            class HumanWorker implements Workable, Eatable, Sleepable {
              work() {
                console.log("Human working (ISP)...");
              }
              eat() {
                console.log("Human eating (ISP)...");
              }
              sleep() {
                console.log("Human sleeping (ISP)...");
              }
            }

            class RobotWorker implements Workable {
              // Chỉ implement những gì cần
              work() {
                console.log("Robot working (ISP)...");
              }
            }

            function manageWorker(worker: Workable) {
              worker.work();
            }
            manageWorker(new HumanWorker());
            manageWorker(new RobotWorker());
            ```

      5.  **D - Dependency Inversion Principle (DIP - Nguyên tắc Đảo ngược Phụ thuộc):**

          - **Phát biểu:**
            1.  Các module cấp cao không nên phụ thuộc vào các module cấp thấp. Cả hai nên phụ thuộc vào **trừu tượng (abstractions)** (ví dụ: interfaces).
            2.  Trừu tượng không nên phụ thuộc vào chi tiết. Chi tiết nên phụ thuộc vào trừu tượng.
          - **"TẠI SAO"?**
            - Giảm sự phụ thuộc chặt chẽ giữa các module.
            - Tăng tính linh hoạt, dễ dàng thay thế các module cấp thấp mà không ảnh hưởng đến module cấp cao.
            - Tạo điều kiện cho Dependency Injection (DI).
          - **Ví dụ (TypeScript):**

            ```typescript
            // Anti-pattern: Module cấp cao phụ thuộc trực tiếp vào module cấp thấp
            class MySQLDatabaseBAD {
              connect() {
                console.log("Connecting to MySQL...");
              }
              query(sql: string) {
                console.log(`MySQL query: ${sql}`);
                return [{ id: 1 }];
              }
            }
            // class PostgreSQLDatabaseBAD { ... }

            class ReportGeneratorBAD {
              private db: MySQLDatabaseBAD; // Phụ thuộc trực tiếp
              constructor() {
                this.db = new MySQLDatabaseBAD(); // Khởi tạo cụ thể
              }
              generateReport() {
                this.db.connect();
                const data = this.db.query("SELECT * FROM sales");
                console.log("Generating report with MySQL data:", data);
              }
            }
            // new ReportGeneratorBAD().generateReport(); // Khó thay đổi DB

            // DIP: Sử dụng interface (trừu tượng) và Dependency Injection
            interface IDatabase {
              // Trừu tượng
              connect(): void;
              query(sql: string): any[];
            }

            class MySQLDatabase implements IDatabase {
              connect() {
                console.log("Connecting to MySQL (DIP)...");
              }
              query(sql: string) {
                console.log(`MySQL query (DIP): ${sql}`);
                return [{ source: "mysql" }];
              }
            }

            class PostgreSQLDatabase implements IDatabase {
              connect() {
                console.log("Connecting to PostgreSQL (DIP)...");
              }
              query(sql: string) {
                console.log(`PostgreSQL query (DIP): ${sql}`);
                return [{ source: "postgres" }];
              }
            }

            // Module cấp cao ReportGenerator phụ thuộc vào IDatabase (trừu tượng)
            class ReportGenerator {
              private db: IDatabase;

              constructor(database: IDatabase) {
                // Dependency Injection qua constructor
                this.db = database;
              }

              generateReport() {
                this.db.connect();
                const data = this.db.query("SELECT * FROM sales_summary");
                console.log("Generating report with data:", data);
              }
            }

            const mysqlReport = new ReportGenerator(new MySQLDatabase());
            mysqlReport.generateReport();

            const postgresReport = new ReportGenerator(
              new PostgreSQLDatabase()
            );
            postgresReport.generateReport();
            ```

          - **Dependency Injection (DI):** Là một kỹ thuật mà một object nhận các dependencies (đối tượng mà nó phụ thuộc) từ bên ngoài, thay vì tự tạo chúng. Có thể qua constructor (constructor injection), setter method (setter injection), hoặc property (property injection - thường dùng với DI frameworks).

    - **Một số Design Patterns phổ biến và cách triển khai trong TypeScript:**

      - _(Lưu ý: Đây là giới thiệu ngắn gọn. Mỗi pattern xứng đáng có một phần nghiên cứu sâu hơn nếu cần. Code ví dụ sẽ tập trung vào ý tưởng chính.)_

      1.  **Singleton (Creational):**

          - **Mục đích:** Đảm bảo một class chỉ có một instance duy nhất và cung cấp một điểm truy cập global đến instance đó.
          - **Khi nào dùng:** Khi bạn cần chính xác một object để điều phối các hành động trong hệ thống (ví dụ: quản lý cấu hình, logger, kết nối database).
          - **Triển khai:**

            ```typescript
            class AppSettings {
              private static instance: AppSettings;
              public readonly theme: string;
              private constructor() {
                // Constructor private
                console.log("AppSettings instance created.");
                this.theme = "dark"; // Giả sử đọc từ file config
              }

              public static getInstance(): AppSettings {
                if (!AppSettings.instance) {
                  AppSettings.instance = new AppSettings();
                }
                return AppSettings.instance;
              }

              public displaySettings(): void {
                console.log(`Current theme: ${this.theme}`);
              }
            }

            // const settings1 = new AppSettings(); // Lỗi: Constructor of class 'AppSettings' is private.
            const settings1 = AppSettings.getInstance();
            const settings2 = AppSettings.getInstance();
            settings1.displaySettings(); // Current theme: dark
            console.log(settings1 === settings2); // true
            ```

          - **Cẩn trọng:** Singleton có thể bị coi là anti-pattern nếu lạm dụng, vì nó tạo ra global state, khó test, và vi phạm DIP.

      2.  **Factory Method (Creational):**

          - **Mục đích:** Định nghĩa một interface để tạo object, nhưng để các class con quyết định class cụ thể nào sẽ được tạo.
          - **Khi nào dùng:** Khi một class không thể biết trước loại object nó cần tạo, hoặc muốn ủy quyền việc tạo object cho các class con.
          - **Triển khai:**

            ```typescript
            interface ILogger {
              log(message: string): void;
            }
            class ConsoleLogger implements ILogger {
              log(message: string) {
                console.log(`[CONSOLE]: ${message}`);
              }
            }
            class FileLogger implements ILogger {
              log(message: string) {
                console.log(`[FILE]: Writing to file: ${message}`);
              }
            }

            abstract class LoggerFactory {
              // Factory Method
              public abstract createLogger(): ILogger;

              public someOperationThatNeedsLogging(): void {
                const logger = this.createLogger(); // Sử dụng factory method
                logger.log("Operation in progress...");
              }
            }

            class DevelopmentLoggerFactory extends LoggerFactory {
              public createLogger(): ILogger {
                return new ConsoleLogger(); // Class con quyết định loại logger
              }
            }

            class ProductionLoggerFactory extends LoggerFactory {
              public createLogger(): ILogger {
                return new FileLogger(); // Class con quyết định loại logger
              }
            }

            const devFactory = new DevelopmentLoggerFactory();
            devFactory.someOperationThatNeedsLogging(); // [CONSOLE]: Operation in progress...

            const prodFactory = new ProductionLoggerFactory();
            prodFactory.someOperationThatNeedsLogging(); // [FILE]: Writing to file: Operation in progress...
            ```

      3.  **Abstract Factory (Creational):**

          - **Mục đích:** Cung cấp một interface để tạo các họ (families) object liên quan hoặc phụ thuộc nhau mà không cần chỉ định class cụ thể của chúng.
          - **Khi nào dùng:** Khi hệ thống cần độc lập với cách các product của nó được tạo, cấu thành và biểu diễn; hoặc khi bạn có nhiều họ product và muốn đảm bảo client sử dụng các product từ cùng một họ.
          - **Triển khai:**

            ```typescript
            interface IButton {
              render(): void;
            }
            interface ICheckbox {
              paint(): void;
            }

            // Abstract Factory
            interface IGUIFactory {
              createButton(): IButton;
              createCheckbox(): ICheckbox;
            }

            // Concrete Products for Windows
            class WindowsButton implements IButton {
              render() {
                console.log("Rendering Windows button");
              }
            }
            class WindowsCheckbox implements ICheckbox {
              paint() {
                console.log("Painting Windows checkbox");
              }
            }
            // Concrete Factory for Windows
            class WindowsFactory implements IGUIFactory {
              createButton(): IButton {
                return new WindowsButton();
              }
              createCheckbox(): ICheckbox {
                return new WindowsCheckbox();
              }
            }

            // Concrete Products for MacOS
            class MacOSButton implements IButton {
              render() {
                console.log("Rendering MacOS button");
              }
            }
            class MacOSCheckbox implements ICheckbox {
              paint() {
                console.log("Painting MacOS checkbox");
              }
            }
            // Concrete Factory for MacOS
            class MacOSFactory implements IGUIFactory {
              createButton(): IButton {
                return new MacOSButton();
              }
              createCheckbox(): ICheckbox {
                return new MacOSCheckbox();
              }
            }

            class Application {
              private button: IButton;
              private checkbox: ICheckbox;
              constructor(factory: IGUIFactory) {
                // Nhận factory
                this.button = factory.createButton();
                this.checkbox = factory.createCheckbox();
              }
              paintUI() {
                this.button.render();
                this.checkbox.paint();
              }
            }

            // Client code quyết định dùng factory nào
            let os: "windows" | "macos" = "macos"; // Giả sử đọc từ config
            let factory: IGUIFactory;
            if (os === "windows") {
              factory = new WindowsFactory();
            } else {
              factory = new MacOSFactory();
            }
            const app = new Application(factory);
            app.paintUI();
            // Nếu os là macos:
            // Rendering MacOS button
            // Painting MacOS checkbox
            ```

      4.  **Observer (Behavioral):**

          - **Mục đích:** Định nghĩa một mối quan hệ một-nhiều (one-to-many) giữa các object, sao cho khi một object (Subject) thay đổi trạng thái, tất cả các object phụ thuộc (Observers) của nó sẽ được thông báo và cập nhật tự động.
          - **Khi nào dùng:** Khi sự thay đổi của một object cần được phản ánh đến các object khác mà không cần chúng phải biết cụ thể về nhau (loose coupling).
          - **Triển khai:**

            ```typescript
            interface IObserver {
              update(data: any): void;
            }

            interface ISubject {
              registerObserver(observer: IObserver): void;
              removeObserver(observer: IObserver): void;
              notifyObservers(data: any): void;
            }

            class WeatherStation implements ISubject {
              private observers: IObserver[] = [];
              private temperature: number = 0;

              registerObserver(observer: IObserver): void {
                this.observers.push(observer);
              }
              removeObserver(observer: IObserver): void {
                const index = this.observers.indexOf(observer);
                if (index > -1) this.observers.splice(index, 1);
              }
              notifyObservers(data: any): void {
                for (const observer of this.observers) {
                  observer.update(data);
                }
              }
              setTemperature(temp: number): void {
                this.temperature = temp;
                console.log(`WeatherStation: New temperature ${temp}°C`);
                this.notifyObservers({ temperature: this.temperature });
              }
            }

            class PhoneDisplay implements IObserver {
              constructor(private name: string) {}
              update(data: any): void {
                if (data.temperature !== undefined) {
                  console.log(
                    `${this.name} Display: Temperature is now ${data.temperature}°C`
                  );
                }
              }
            }

            class FanController implements IObserver {
              update(data: any): void {
                if (data.temperature > 25) {
                  console.log(
                    "FanController: Temperature high! Turning on fan."
                  );
                } else {
                  console.log("FanController: Temperature normal. Fan off.");
                }
              }
            }

            const station = new WeatherStation();
            const phone1 = new PhoneDisplay("MyPhone");
            const phone2 = new PhoneDisplay("OfficeScreen");
            const fan = new FanController();

            station.registerObserver(phone1);
            station.registerObserver(phone2);
            station.registerObserver(fan);

            station.setTemperature(20);
            // WeatherStation: New temperature 20°C
            // MyPhone Display: Temperature is now 20°C
            // OfficeScreen Display: Temperature is now 20°C
            // FanController: Temperature normal. Fan off.

            station.setTemperature(28);
            // WeatherStation: New temperature 28°C
            // MyPhone Display: Temperature is now 28°C
            // OfficeScreen Display: Temperature is now 28°C
            // FanController: Temperature high! Turning on fan.

            station.removeObserver(phone2);
            station.setTemperature(22);
            // WeatherStation: New temperature 22°C
            // MyPhone Display: Temperature is now 22°C
            // FanController: Temperature normal. Fan off. (OfficeScreen không còn nhận)
            ```

      5.  **Strategy (Behavioral):**

          - **Mục đích:** Định nghĩa một họ các thuật toán, đóng gói từng thuật toán lại, và làm cho chúng có thể thay thế lẫn nhau. Strategy cho phép thuật toán thay đổi độc lập với client sử dụng nó.
          - **Khi nào dùng:** Khi bạn có nhiều biến thể của một thuật toán và muốn client có thể chọn thuật toán nào sẽ sử dụng tại runtime.
          - **Triển khai:**

            ```typescript
            interface ISortStrategy {
              sort<T>(data: T[]): T[];
            }

            class BubbleSortStrategy implements ISortStrategy {
              sort<T>(data: T[]): T[] {
                console.log("Sorting using Bubble Sort");
                // ... actual bubble sort logic ...
                return data.slice().sort((a: any, b: any) => (a > b ? 1 : -1)); // Simplified
              }
            }

            class QuickSortStrategy implements ISortStrategy {
              sort<T>(data: T[]): T[] {
                console.log("Sorting using Quick Sort");
                // ... actual quick sort logic ...
                return data.slice().sort((a: any, b: any) => a - b); // Simplified
              }
            }

            class Sorter<T> {
              private strategy: ISortStrategy;
              constructor(strategy: ISortStrategy) {
                this.strategy = strategy;
              }
              setStrategy(strategy: ISortStrategy) {
                this.strategy = strategy;
              }
              performSort(data: T[]): T[] {
                return this.strategy.sort(data);
              }
            }

            const numbers = [5, 1, 4, 2, 8];
            const sorter = new Sorter<number>(new BubbleSortStrategy());
            console.log(sorter.performSort(numbers));
            // Sorting using Bubble Sort
            // [ 1, 2, 4, 5, 8 ]

            sorter.setStrategy(new QuickSortStrategy());
            console.log(sorter.performSort(numbers));
            // Sorting using Quick Sort
            // [ 1, 2, 4, 5, 8 ]
            ```

      - **Các patterns khác đáng tìm hiểu cho backend:**
        - **Builder (Creational):** Tách biệt việc xây dựng một object phức tạp khỏi biểu diễn của nó, cho phép cùng một quy trình xây dựng có thể tạo ra các biểu diễn khác nhau.
        - **Adapter (Structural):** Cho phép các interface không tương thích có thể làm việc cùng nhau.
        - **Decorator (Structural Pattern):** Gắn thêm trách nhiệm cho một object một cách linh hoạt, là một giải pháp thay thế cho kế thừa để mở rộng chức năng. (Khác với TS Decorator feature).
        - **Facade (Structural):** Cung cấp một interface đơn giản, thống nhất cho một tập hợp các interface phức tạp trong một subsystem.
        - **Proxy (Structural):** Cung cấp một đối tượng thay thế (surrogate or placeholder) cho một object khác để kiểm soát việc truy cập đến nó.
        - **Command (Behavioral):** Đóng gói một yêu cầu như một object, qua đó cho phép bạn tham số hóa client với các yêu cầu khác nhau, đưa yêu cầu vào hàng đợi hoặc log, và hỗ trợ các thao tác có thể hoàn tác.
        - **Chain of Responsibility (Behavioral):** Tránh việc kết nối trực tiếp người gửi yêu cầu với người nhận yêu cầu bằng cách cho nhiều object có cơ hội xử lý yêu cầu. Các object nhận được liên kết thành một chuỗi và yêu cầu được truyền dọc theo chuỗi cho đến khi một object xử lý nó.
        - **Template Method (Behavioral):** Định nghĩa bộ khung của một thuật toán trong một operation, trì hoãn một số bước cho các class con. Template Method cho phép các class con định nghĩa lại một số bước của thuật toán mà không thay đổi cấu trúc của thuật toán.

5.  **Minh họa trực quan và Code mẫu (Đã tích hợp trong phần Giải thích lý thuyết)**

6.  **Best Practices và Quy ước (Conventions):**

    - **Hiểu rõ vấn đề trước khi áp dụng pattern:** Đừng cố ép buộc sử dụng pattern nếu không cần thiết. Over-engineering là một vấn đề.
    - **Bắt đầu đơn giản:** Chỉ áp dụng pattern khi sự phức tạp của vấn đề thực sự đòi hỏi.
    - **SOLID là kim chỉ nam:** Luôn cố gắng tuân thủ các nguyên tắc SOLID khi thiết kế và triển khai patterns. Nhiều patterns giúp bạn đạt được SOLID.
    - **Tên gọi nhất quán:** Sử dụng tên pattern trong tên class/interface của bạn nếu phù hợp (ví dụ: `UserFactory`, `NotificationObserver`) để dễ nhận biết.
    - **Ưu tiên composition over inheritance:** Nhiều patterns (Strategy, Decorator) khuyến khích điều này.
    - **Sử dụng interfaces để định nghĩa contracts:** Giúp tăng tính linh hoạt và tuân thủ DIP.
    - **Tài liệu hóa lý do chọn pattern:** Giúp người khác (và chính bạn trong tương lai) hiểu được quyết định thiết kế.

7.  **Anti-patterns và Lỗi thường gặp (Common Pitfalls):**

    - **Pattern Hammer / Silver Bullet Syndrome:** Coi một pattern yêu thích là giải pháp cho mọi vấn đề.
    - **Over-engineering:** Áp dụng quá nhiều pattern hoặc các pattern phức tạp cho các vấn đề đơn giản, làm tăng độ phức tạp không cần thiết.
    - **Hiểu sai mục đích của pattern:** Dẫn đến việc triển khai sai hoặc không hiệu quả.
    - **Không tuân thủ SOLID khi triển khai pattern:** Ví dụ, một Factory Method vi phạm SRP nếu nó làm quá nhiều việc.
    - **Singleton hóa mọi thứ:** Dẫn đến global state, khó test và giảm tính linh hoạt.
    - **Kế thừa quá sâu (Deep inheritance hierarchies) khi cố gắng áp dụng một số pattern.**

8.  **So sánh, Đánh giá và Lựa chọn:**

    - **Khi nào KHÔNG nên dùng Design Pattern?**
      - Khi vấn đề đơn giản và một giải pháp trực tiếp, dễ hiểu là đủ.
      - Khi việc áp dụng pattern làm tăng độ phức tạp hơn là lợi ích nó mang lại.
      - Khi bạn chưa thực sự hiểu rõ pattern hoặc vấn đề.
    - **Nhiều pattern có mục đích tương tự nhưng giải quyết ở các khía cạnh khác nhau.** Ví dụ: Factory Method, Abstract Factory, Builder đều liên quan đến việc tạo object nhưng có ngữ cảnh sử dụng khác nhau.

9.  **Bài tập thực hành và Gợi ý dự án nhỏ:**

    - **Câu hỏi lý thuyết:**
      1.  Giải thích nguyên tắc Open/Closed. Làm thế nào để đạt được nó trong TypeScript?
      2.  Sự khác biệt giữa Dependency Inversion Principle (DIP) và Dependency Injection (DI) là gì?
      3.  Mô tả một tình huống bạn sẽ sử dụng Strategy pattern.
      4.  Singleton pattern có những ưu và nhược điểm gì?
      5.  Làm thế nào Observer pattern giúp giảm sự phụ thuộc giữa các object?
    - **Thử thách code (Coding challenges):**
      1.  **Triển khai Builder Pattern:** Tạo một `QueryBuilder` để xây dựng các câu lệnh SQL (SELECT) một cách linh hoạt.
          Ví dụ: `new QueryBuilder().select("name", "email").from("users").where("age > 18").orderBy("name").build()`
      2.  **Triển khai Adapter Pattern:** Giả sử bạn có một `OldPaymentGateway` với API `processPayment(amount: number, cardNumber: string)` và một `NewPaymentProcessor` với API `submitTransaction({ total: number, creditCard: { number: string, expiry: string } })`. Tạo một Adapter để `NewPaymentProcessor` có thể được sử dụng thông qua interface của `OldPaymentGateway` (hoặc ngược lại).
      3.  **Triển khai Command Pattern:** Cho một ứng dụng chỉnh sửa văn bản đơn giản, tạo các command như `BoldCommand`, `ItalicCommand`, `UndoCommand`. Client có thể thực thi các command này.
    - **Gợi ý dự án nhỏ (tiếp tục dự án Quản lý công việc CLI):**
      - **Refactor bằng SOLID:** Xem lại code hiện tại và xác định các vị trí có thể vi phạm SOLID. Cố gắng refactor để tuân thủ tốt hơn.
      - **Áp dụng Strategy Pattern:** Cho phép người dùng chọn các chiến lược sắp xếp task khác nhau (theo ngày tạo, theo độ ưu tiên, theo deadline).
      - **Áp dụng Command Pattern:** Mỗi hành động của CLI (add task, list tasks, complete task) có thể được đóng gói thành một Command object. Điều này có thể giúp cho việc thêm tính năng undo/redo sau này.
      - **Áp dụng Observer Pattern:** Khi một task được cập nhật (ví dụ: đánh dấu hoàn thành), thông báo cho các "subscriber" khác (ví dụ: một module ghi log, một module gửi thông báo).

10. **Câu hỏi phỏng vấn thường gặp:**
    - "What are design patterns? Why are they important?"
    - "Explain the SOLID principles. Give an example for each."
    - "Describe the Factory Method pattern and when you would use it."
    - "What is the Singleton pattern? What are its potential drawbacks?"
    - "How does the Observer pattern work? Give a real-world example."
    - "Explain the Strategy pattern and how it promotes the Open/Closed Principle."
    - "What is Dependency Injection and how does it relate to the DependencyInversion Principle?"
    - "How would you refactor a class that violates the Single Responsibility Principle?"
    - "Can you give an example of the Adapter pattern?"

---

Design Patterns và SOLID là những chủ đề rộng lớn và cần thời gian để thẩm thấu và áp dụng thành thạo. Đừng cố gắng học thuộc lòng, mà hãy hiểu "tại sao" và "khi nào" sử dụng chúng. Thực hành qua các ví dụ và cố gắng nhận diện chúng trong các framework hoặc thư viện bạn sử dụng sẽ rất hữu ích. Khi bạn sẵn sàng, chúng ta sẽ chuyển sang việc áp dụng kiến thức này vào xây dựng API thực tế.

---

### **PHẦN 8: Xây Dựng RESTful APIs với Express.js và TypeScript**

1.  **Tên phần học:** Xây Dựng RESTful APIs với Express.js và TypeScript.

2.  **Mục tiêu học phần:**

    - Hiểu các khái niệm cơ bản của **REST (Representational State Transfer)** và các nguyên tắc thiết kế RESTful API.
    - Nắm vững cách cài đặt và cấu hình một dự án **Express.js** với TypeScript.
    - Biết cách định nghĩa routes (đường dẫn) và xử lý các HTTP methods (GET, POST, PUT, DELETE, PATCH) trong Express.
    - Sử dụng thành thạo các đối tượng `Request` và `Response` của Express để xử lý dữ liệu đầu vào (params, query, body) và gửi phản hồi (JSON, status codes).
    - Hiểu và triển khai **Middleware** trong Express cho các tác vụ như logging, authentication, validation, error handling.
    - Biết cách tổ chức cấu trúc route và controller một cách khoa học.
    - Xử lý lỗi (Error Handling) một cách nhất quán trong ứng dụng Express.
    - Sử dụng các thư viện phổ biến đi kèm Express như `body-parser` (nay đã tích hợp sẵn trong Express), `cors`, `helmet`.
    - Áp dụng kiến thức TypeScript (interfaces, classes, types) để xây dựng API an toàn kiểu.
    - Trả lời được:
      - RESTful API là gì? Các ràng buộc chính của REST là gì?
      - Làm thế nào để tạo một server Express đơn giản với TypeScript?
      - Cách xử lý các loại request parameters (route params, query params, request body) trong Express?
      - Middleware trong Express là gì và có những loại nào? Cho ví dụ.
      - Làm thế nào để thiết kế một error handling middleware hiệu quả?
      - Sự khác biệt giữa `PUT` và `PATCH` là gì?

3.  **Kiến thức tiên quyết:**

    - Hoàn thành Phần 1-7 (đặc biệt là Modules, OOP, TypeScript cơ bản và nâng cao).
    - Hiểu biết cơ bản về HTTP (methods, status codes, headers).
    - Kiến thức về JavaScript bất đồng bộ (Promises, async/await).
    - Node.js và npm/yarn cơ bản.

4.  **Giải thích lý thuyết chuyên sâu:**

    - **REST (Representational State Transfer):**

      - Là một kiểu kiến trúc (architectural style) để thiết kế các ứng dụng mạng phân tán, phổ biến nhất là cho web services. Không phải là một tiêu chuẩn hay protocol cụ thể, mà là một tập hợp các ràng buộc.
      - **"TẠI SAO" REST?**
        - **Scalability (Khả năng mở rộng):** Do tính stateless.
        - **Simplicity (Đơn giản):** Sử dụng các chuẩn HTTP quen thuộc.
        - **Performance (Hiệu năng):** Hỗ trợ caching.
        - **Modifiability (Khả năng sửa đổi):** Các thành phần có thể được cập nhật độc lập.
        - **Portability (Tính khả chuyển):** Dữ liệu có thể được truy cập từ nhiều client khác nhau.
      - **Các ràng buộc chính của REST:**
        1.  **Client-Server:** Tách biệt giữa client (giao diện người dùng) và server (lưu trữ dữ liệu). Client không cần biết về logic lưu trữ, server không cần biết về UI.
        2.  **Stateless (Phi trạng thái):** Mỗi request từ client đến server phải chứa tất cả thông tin cần thiết để server hiểu và xử lý request đó. Server không lưu trữ bất kỳ context (trạng thái) nào của client giữa các request.
            - **"TẠI SAO" Stateless?** Giúp đơn giản hóa thiết kế server, tăng khả năng mở rộng (dễ dàng scale server instances), và tăng tính tin cậy (nếu một server instance lỗi, request có thể được chuyển sang instance khác).
        3.  **Cacheable (Có thể cache):** Phản hồi từ server phải có thể được đánh dấu là cacheable hoặc non-cacheable. Client hoặc các proxy trung gian có thể cache các phản hồi cacheable để cải thiện hiệu năng.
        4.  **Uniform Interface (Giao diện đồng nhất):** Đây là ràng buộc cốt lõi, đơn giản hóa và tách rời kiến trúc. Bao gồm:
            - **Identification of resources (Định danh tài nguyên):** Mọi thứ là một tài nguyên (resource) và được định danh duy nhất bằng URI (Uniform Resource Identifier), ví dụ: `/users`, `/users/123`.
            - **Manipulation of resources through representations (Thao tác tài nguyên qua các biểu diễn):** Client tương tác với tài nguyên thông qua các biểu diễn của tài nguyên đó (ví dụ: JSON, XML). Client có thể thay đổi trạng thái tài nguyên bằng cách gửi biểu diễn mới của tài nguyên đó lên server.
            - **Self-descriptive messages (Thông điệp tự mô tả):** Mỗi thông điệp (request/response) phải chứa đủ thông tin để người nhận hiểu nó (ví dụ: `Content-Type` header để chỉ định định dạng dữ liệu, HTTP methods để chỉ định hành động).
            - **Hypermedia as the Engine of Application State (HATEOAS):** Phản hồi từ server nên chứa các liên kết (hyperlinks) đến các hành động hoặc tài nguyên liên quan khác mà client có thể thực hiện. Client điều hướng ứng dụng bằng cách theo các liên kết này. Đây là ràng buộc ít được tuân thủ nghiêm ngặt nhất trong thực tế.
        5.  **Layered System (Hệ thống phân lớp):** Client không nhất thiết phải biết nó đang kết nối trực tiếp đến server cuối cùng hay qua các server trung gian (proxy, load balancer, cache server).
        6.  **Code on Demand (Tùy chọn):** Server có thể tạm thời mở rộng hoặc tùy chỉnh chức năng của client bằng cách truyền mã thực thi (ví dụ: JavaScript). Ít phổ biến cho API.

    - **Express.js:**

      - Là một framework web tối giản và linh hoạt cho Node.js, được thiết kế để xây dựng các ứng dụng web và API một cách nhanh chóng và dễ dàng.
      - Không áp đặt cấu trúc cứng nhắc, cho phép nhà phát triển tự do lựa chọn công cụ và cấu trúc.
      - Dựa trên cơ chế middleware.
      - **"TẠI SAO" Express.js?**
        - **Phổ biến:** Cộng đồng lớn, nhiều tài liệu và thư viện hỗ trợ.
        - **Tối giản (Minimalist):** Cung cấp các tính năng cốt lõi, không cồng kềnh.
        - **Linh hoạt (Flexible):** Dễ dàng tích hợp với các thư viện và công cụ khác.
        - **Hiệu năng tốt:** Cho các ứng dụng I/O-bound.

    - **Cài đặt và cấu hình dự án Express.js với TypeScript:**

      1.  **Khởi tạo dự án Node.js:** `npm init -y`
      2.  **Cài đặt dependencies:**
          ```bash
          npm install express
          npm install --save-dev typescript @types/express @types/node ts-node nodemon
          ```
          - `express`: Framework Express.js.
          - `typescript`: Trình biên dịch TypeScript.
          - `@types/express`: File khai báo kiểu cho Express.
          - `@types/node`: File khai báo kiểu cho Node.js.
          - `ts-node`: Chạy trực tiếp file TypeScript mà không cần biên dịch trước (cho development).
          - `nodemon`: Tự động khởi động lại server khi có thay đổi code (cho development).
      3.  **Tạo `tsconfig.json`:** `npx tsc --init`
          Cấu hình quan trọng (tham khảo Phần 4):
          ```json
          {
            "compilerOptions": {
              "target": "ES2020", // Hoặc mới hơn
              "module": "CommonJS", // Hoặc "ESNext" nếu dùng "type": "module" trong package.json
              "outDir": "./dist",
              "rootDir": "./src",
              "strict": true,
              "esModuleInterop": true,
              "skipLibCheck": true,
              "forceConsistentCasingInFileNames": true,
              "experimentalDecorators": true, // Nếu dùng decorators
              "emitDecoratorMetadata": true // Nếu dùng decorators với reflect-metadata
            },
            "include": ["src/**/*"],
            "exclude": ["node_modules"]
          }
          ```
      4.  **Thêm scripts vào `package.json`:**
          ```json
          "scripts": {
            "build": "tsc",
            "start": "node dist/server.js", // Chạy file JS đã build
            "dev": "nodemon src/server.ts"    // Chạy file TS với ts-node
          },
          ```
      5.  **Tạo file `src/server.ts` (entry point):**

          ```typescript
          // src/server.ts
          import express, {
            Express,
            Request,
            Response,
            NextFunction,
          } from "express";
          import dotenv from "dotenv"; // Nếu dùng .env file

          dotenv.config(); // Load .env variables

          const app: Express = express();
          const port = process.env.PORT || 3000;

          // Middleware cơ bản
          app.use(express.json()); // Để parse JSON request body
          app.use(express.urlencoded({ extended: true })); // Để parse URL-encoded request body

          // Một route đơn giản
          app.get("/", (req: Request, res: Response) => {
            res.send("Hello from Express + TypeScript Server!");
          });

          // Khởi động server
          app.listen(port, () => {
            console.log(
              `[server]: Server is running at http://localhost:${port}`
            );
          });
          ```

    - **Routes và HTTP Methods:**

      - Express cung cấp các phương thức tương ứng với HTTP methods để định nghĩa routes: `app.get()`, `app.post()`, `app.put()`, `app.delete()`, `app.patch()`, `app.all()`, `app.use()`.
      - Cú pháp: `app.METHOD(PATH, HANDLER_FUNCTION)`
        - `PATH`: Chuỗi hoặc RegExp mô tả đường dẫn.
        - `HANDLER_FUNCTION`: Hàm callback được gọi khi route khớp. Có thể có một hoặc nhiều handler functions (middleware).
      - **Ví dụ:**

        ```typescript
        // src/app.ts (hoặc nơi bạn định nghĩa routes)
        // ... (khởi tạo app) ...

        interface User {
          id: number;
          name: string;
        }
        let users: User[] = [
          { id: 1, name: "Alice" },
          { id: 2, name: "Bob" },
        ];
        let nextUserId = 3;

        // GET /users - Lấy danh sách users
        app.get("/users", (req: Request, res: Response) => {
          res.status(200).json(users);
        });

        // POST /users - Tạo user mới
        app.post("/users", (req: Request, res: Response) => {
          const { name } = req.body; // Giả sử body có dạng { "name": "Charlie" }
          if (!name) {
            return res.status(400).json({ message: "Name is required" });
          }
          const newUser: User = { id: nextUserId++, name };
          users.push(newUser);
          res.status(201).json(newUser); // 201 Created
        });

        // GET /users/:id - Lấy user theo ID (Route Parameter)
        app.get("/users/:id", (req: Request, res: Response) => {
          const userId = parseInt(req.params.id, 10); // req.params.id là string
          const user = users.find((u) => u.id === userId);
          if (user) {
            res.status(200).json(user);
          } else {
            res.status(404).json({ message: "User not found" }); // 404 Not Found
          }
        });

        // PUT /users/:id - Cập nhật toàn bộ user (thay thế)
        app.put("/users/:id", (req: Request, res: Response) => {
          const userId = parseInt(req.params.id, 10);
          const { name } = req.body;
          if (!name) {
            return res
              .status(400)
              .json({ message: "Name is required for update" });
          }
          const userIndex = users.findIndex((u) => u.id === userId);
          if (userIndex > -1) {
            users[userIndex] = { id: userId, name }; // Thay thế toàn bộ
            res.status(200).json(users[userIndex]);
          } else {
            // Hoặc có thể tạo mới nếu không tìm thấy (upsert), tùy logic
            res.status(404).json({ message: "User not found for PUT" });
          }
        });

        // PATCH /users/:id - Cập nhật một phần user
        app.patch("/users/:id", (req: Request, res: Response) => {
          const userId = parseInt(req.params.id, 10);
          const updates = req.body; // { "name": "Alicia" } hoặc { "email": "new@example.com" }
          const userIndex = users.findIndex((u) => u.id === userId);

          if (userIndex > -1) {
            // Cập nhật chỉ các trường được cung cấp
            users[userIndex] = { ...users[userIndex], ...updates };
            res.status(200).json(users[userIndex]);
          } else {
            res.status(404).json({ message: "User not found for PATCH" });
          }
        });

        // DELETE /users/:id - Xóa user
        app.delete("/users/:id", (req: Request, res: Response) => {
          const userId = parseInt(req.params.id, 10);
          const initialLength = users.length;
          users = users.filter((u) => u.id !== userId);
          if (users.length < initialLength) {
            res.status(204).send(); // 204 No Content
          } else {
            res.status(404).json({ message: "User not found for DELETE" });
          }
        });
        ```

    - **Đối tượng `Request` và `Response`:**

      - **`req` (Request):**
        - `req.params`: Object chứa các route parameters (ví dụ: `/users/:id`, `req.params.id`).
        - `req.query`: Object chứa các query string parameters (ví dụ: `/search?q=typescript`, `req.query.q`).
        - `req.body`: Object chứa request body đã được parse (cần middleware như `express.json()`).
        - `req.headers`: Object chứa các HTTP headers của request.
        - `req.method`: HTTP method của request.
        - `req.path`: Đường dẫn của request.
      - **`res` (Response):**
        - `res.send(body)`: Gửi phản hồi với body (có thể là string, buffer, object, array). Tự động set `Content-Type`.
        - `res.json(body)`: Gửi phản hồi JSON. Tự động set `Content-Type: application/json`.
        - `res.status(statusCode)`: Set HTTP status code. Nên gọi trước `send` hoặc `json`.
        - `res.sendStatus(statusCode)`: Set status code và gửi mô tả text mặc định của status code đó làm body.
        - `res.set(headerName, value)` hoặc `res.header(headerName, value)`: Set một HTTP response header.
        - `res.redirect([status], path)`: Chuyển hướng request.
        - `res.end()`: Kết thúc response process mà không gửi dữ liệu (thường dùng khi response đã được stream).

    - **Middleware:**

      - Là các hàm có quyền truy cập vào đối tượng `request` (req), `response` (res), và hàm `next` trong chu kỳ request-response của ứng dụng.
      - Hàm `next()`: Chuyển quyền kiểm soát cho middleware tiếp theo trong stack. Nếu không gọi `next()`, request sẽ bị "treo" (trừ khi middleware đó kết thúc chu kỳ bằng cách gửi response).
      - **"TẠI SAO" Middleware?**
        - Thực thi code bất kỳ.
        - Thay đổi đối tượng `req` và `res`.
        - Kết thúc chu kỳ request-response.
        - Gọi middleware tiếp theo.
      - **Các loại Middleware:**
        1.  **Application-level middleware:** Gắn vào instance `app` bằng `app.use()` hoặc `app.METHOD()`.
        2.  **Router-level middleware:** Gắn vào instance `express.Router()`.
        3.  **Error-handling middleware:** Có 4 tham số `(err, req, res, next)`. Phải được định nghĩa sau cùng.
        4.  **Built-in middleware:** Ví dụ: `express.json()`, `express.urlencoded()`, `express.static()`.
        5.  **Third-party middleware:** Ví dụ: `cors`, `helmet`, `morgan` (logging).
      - **Ví dụ: Custom Logger Middleware**

        ```typescript
        // src/middlewares/logger.middleware.ts
        import { Request, Response, NextFunction } from "express";

        export const requestLogger = (
          req: Request,
          res: Response,
          next: NextFunction
        ) => {
          console.log(
            `${new Date().toISOString()} - ${req.method} ${req.originalUrl}`
          );
          // Log request body (cẩn thận với dữ liệu nhạy cảm)
          // if (Object.keys(req.body).length > 0) {
          //   console.log('Request Body:', req.body);
          // }
          next(); // Quan trọng: chuyển sang middleware/handler tiếp theo
        };

        // Sử dụng trong src/server.ts hoặc src/app.ts
        // import { requestLogger } from './middlewares/logger.middleware';
        // app.use(requestLogger); // Áp dụng cho tất cả routes sau nó
        ```

      - **Ví dụ: Authentication Middleware (đơn giản)**

        ```typescript
        // src/middlewares/auth.middleware.ts
        export const apiKeyAuth = (
          req: Request,
          res: Response,
          next: NextFunction
        ) => {
          const apiKey = req.headers["x-api-key"];
          if (apiKey && apiKey === process.env.API_KEY) {
            // Giả sử API_KEY trong .env
            next();
          } else {
            res.status(401).json({ message: "Unauthorized: Invalid API Key" });
          }
        };

        // Sử dụng:
        // app.use('/admin', apiKeyAuth, adminRoutes); // Chỉ áp dụng cho /admin
        // app.post('/sensitive-data', apiKeyAuth, (req, res) => { ... });
        ```

    - **Tổ chức Routes và Controllers (Router-level middleware):**

      - Khi ứng dụng lớn, không nên đặt tất cả routes trong một file.
      - Sử dụng `express.Router()` để nhóm các routes liên quan.
      - **Controller Pattern:** Tách logic xử lý request (controller) ra khỏi định nghĩa route.
      - **Ví dụ cấu trúc:**

        ```
        src/
        ├── api/
        │   ├── index.ts            # Main router, gộp các feature routers
        │   └── users/
        │       ├── user.routes.ts    # Định nghĩa routes cho user
        │       ├── user.controller.ts # Logic xử lý cho user
        │       └── user.validation.ts # (Tùy chọn) Validation schemas
        ├── app.ts                  # Khởi tạo Express app, gắn main router
        └── server.ts               # Khởi động server
        ```

        ```typescript
        // src/api/users/user.controller.ts
        import { Request, Response } from "express";
        // Giả sử có UserService
        // import UserService from '../../services/user.service';

        export class UserController {
          // private userService = new UserService(); // Hoặc inject

          public static getUsers(req: Request, res: Response): void {
            // const users = this.userService.findAll();
            res.status(200).json([{ id: 1, name: "From Controller" }]);
          }

          public static createUser(req: Request, res: Response): void {
            const { name } = req.body;
            if (!name) {
              res
                .status(400)
                .json({ message: "Name is required in controller" });
              return;
            }
            // const newUser = this.userService.create({ name });
            res.status(201).json({ id: Date.now(), name });
          }
        }

        // src/api/users/user.routes.ts
        import { Router } from "express";
        import { UserController } from "./user.controller";
        // import { validateCreateUser } from './user.validation'; // Middleware validation

        const router = Router();
        router.get("/", UserController.getUsers);
        router.post("/", /* validateCreateUser, */ UserController.createUser); // Có thể thêm middleware validate

        export default router; // Export router

        // src/api/index.ts
        import { Router } from "express";
        import userRoutes from "./users/user.routes";
        // import productRoutes from './products/product.routes';

        const mainRouter = Router();
        mainRouter.use("/users", userRoutes);
        // mainRouter.use('/products', productRoutes);

        export default mainRouter;

        // src/app.ts
        // ...
        // import mainApiRouter from './api/index';
        // app.use('/api/v1', mainApiRouter); // Gắn main router vào app với prefix
        // ...
        ```

    - **Error Handling Middleware:**

      - Middleware đặc biệt có 4 tham số: `(err, req, res, next)`.
      - Phải được định nghĩa **sau tất cả các `app.use()` và routes khác.**
      - Khi `next(err)` được gọi từ một route handler hoặc middleware khác, Express sẽ bỏ qua các middleware/handler thông thường và nhảy đến error handling middleware đầu tiên nó tìm thấy.
      - **"TẠI SAO" quan trọng?** Để xử lý lỗi một cách nhất quán, tránh crash server, và gửi phản hồi lỗi thân thiện cho client.
      - **Ví dụ:**

        ```typescript
        // src/middlewares/errorHandler.middleware.ts
        import { Request, Response, NextFunction } from "express";

        interface AppError extends Error {
          statusCode?: number;
          isOperational?: boolean; // Lỗi do người dùng hoặc lỗi dự kiến
        }

        export const globalErrorHandler = (
          err: AppError,
          req: Request,
          res: Response,
          next: NextFunction // Mặc dù không dùng next ở đây, nó vẫn cần để Express nhận diện là error handler
        ) => {
          err.statusCode = err.statusCode || 500;
          err.message = err.message || "Internal Server Error";

          // Log lỗi chi tiết cho dev (không gửi cho client nếu là lỗi server không lường trước)
          console.error("ERROR 💥:", err);

          // Chỉ gửi thông tin lỗi chi tiết nếu là lỗi dự kiến (operational)
          // Hoặc trong môi trường development
          if (process.env.NODE_ENV === "development" || err.isOperational) {
            res.status(err.statusCode).json({
              status: err.statusCode >= 500 ? "error" : "fail",
              message: err.message,
              // stack: process.env.NODE_ENV === 'development' ? err.stack : undefined
            });
          } else {
            // Lỗi server không xác định, không lộ chi tiết
            res.status(500).json({
              status: "error",
              message: "Something went very wrong!",
            });
          }
        };

        // Sử dụng trong src/app.ts (SAU CÙNG)
        // import { globalErrorHandler } from './middlewares/errorHandler.middleware';
        // app.use(globalErrorHandler);

        // Trong route handler, nếu có lỗi:
        // app.get('/some-route', (req, res, next) => {
        //   try {
        //     // ... some logic that might throw ...
        //     if (someConditionFails) {
        //       const error = new Error('Specific error message') as AppError;
        //       error.statusCode = 400;
        //       error.isOperational = true;
        //       return next(error); // Chuyển lỗi cho error handler
        //     }
        //     res.send("Success");
        //   } catch (e) {
        //     next(e); // Bắt lỗi đồng bộ và chuyển đi
        //   }
        // });

        // Đối với lỗi bất đồng bộ trong Promises, nếu không dùng try/catch với await,
        // bạn cần .catch(next)
        // someAsyncFunction().then(data => res.json(data)).catch(next);
        // Hoặc dùng thư viện như 'express-async-errors' để tự động bắt lỗi async
        ```

    - **Các thư viện phổ biến khác:**
      - **`cors`:** Middleware để kích hoạt Cross-Origin Resource Sharing (CORS).
        ```bash
        npm install cors
        npm install --save-dev @types/cors
        ```
        ```typescript
        // import cors from 'cors';
        // app.use(cors()); // Cho phép tất cả origin
        // Hoặc cấu hình cụ thể:
        // app.use(cors({
        //   origin: 'https://your-frontend-domain.com',
        //   methods: ['GET', 'POST', 'PUT'],
        //   allowedHeaders: ['Content-Type', 'Authorization']
        // }));
        ```
      - **`helmet`:** Giúp bảo vệ ứng dụng Express bằng cách thiết lập các HTTP headers bảo mật khác nhau.
        ```bash
        npm install helmet
        ```
        ```typescript
        // import helmet from 'helmet';
        // app.use(helmet());
        ```
      - **`morgan`:** HTTP request logger middleware.
        ```bash
        npm install morgan
        npm install --save-dev @types/morgan
        ```
        ```typescript
        // import morgan from 'morgan';
        // app.use(morgan('dev')); // 'combined', 'short', 'tiny', etc.
        ```
      - **`dotenv`:** Load biến môi trường từ file `.env`. (Đã dùng ở ví dụ đầu).
      - **`express-async-errors`:** Tự động bắt lỗi trong các route handler bất đồng bộ và chuyển cho error handling middleware.
        ```bash
        npm install express-async-errors
        ```
        ```typescript
        // Trong src/app.ts hoặc src/server.ts, import một lần ở đầu:
        // import 'express-async-errors';
        // Sau đó, không cần try/catch hoặc .catch(next) trong các route handler async nữa.
        ```

5.  **Minh họa trực quan và Code mẫu (Đã tích hợp trong phần Giải thích lý thuyết)**

    - **Sơ đồ luồng Request-Response trong Express với Middleware:**

      ```mermaid
      graph LR
          ClientRequest["Client HTTP Request"] --> A[Express App];
          A -- "Matches Route?" --> M1["Middleware 1 (e.g., Logger)"];
          M1 -- "next()" --> M2["Middleware 2 (e.g., Auth)"];
          M2 -- "next()" --> RH["Route Handler (Controller)"];
          RH -- "Processes Request" --> ResponseObject["Response Object (res)"];
          ResponseObject -- "Sends HTTP Response" --> ClientResponse["Client Receives Response"];

          M2 -- "Auth Fails, res.status(401).send()" ---> ClientResponse;
          RH -- "Error Occurs, next(err)" --> EHM["Error Handling Middleware"];
          EHM -- "Sends Error Response" --> ClientResponse;

          subgraph MiddlewareChain
              M1
              M2
              RH
          end
      ```

6.  **Best Practices và Quy ước (Conventions):**

    - **Tuân thủ các nguyên tắc RESTful:** Sử dụng đúng HTTP methods, status codes, URI hợp lý.
    - **Statelessness:** Thiết kế API sao cho server không cần lưu trạng thái của client.
    - **Sử dụng JSON làm định dạng dữ liệu chính.**
    - **Versioning API (ví dụ: `/api/v1/users`):** Giúp quản lý các thay đổi không tương thích ngược.
    - **Tổ chức routes và controllers rõ ràng.**
    - **Sử dụng middleware cho các cross-cutting concerns.**
    - **Triển khai error handling nhất quán.**
    - **Validate dữ liệu đầu vào (request body, query params, route params).** (Sẽ có phần riêng về Validation).
    - **Sử dụng biến môi trường (`.env`) cho các cấu hình nhạy cảm (database credentials, API keys).**
    - **Viết API documentation (ví dụ: Swagger/OpenAPI).** (Sẽ có phần riêng).
    - **Bảo mật API (sử dụng `helmet`, rate limiting, input sanitization).**

7.  **Anti-patterns và Lỗi thường gặp (Common Pitfalls):**

    - **Sử dụng GET cho các thao tác làm thay đổi dữ liệu:** GET phải luôn an toàn (idempotent và không có side effect thay đổi state).
    - **Không sử dụng đúng HTTP status codes:** Ví dụ, trả về `200 OK` cho cả lỗi.
    - **Lộ thông tin lỗi chi tiết (stack traces) ra client trong production.**
    - **Thiếu validation cho dữ liệu đầu vào:** Dẫn đến lỗi hoặc lỗ hổng bảo mật.
    - **Quên gọi `next()` trong middleware (nếu không phải là middleware cuối cùng kết thúc response).**
    - **Blocking the event loop:** Thực hiện các tác vụ đồng bộ, tốn thời gian dài trong route handlers hoặc middleware. Sử dụng async operations.
    - **Không xử lý lỗi trong các hàm bất đồng bộ đúng cách (nếu không dùng `express-async-errors`).**

8.  **So sánh, Đánh giá và Lựa chọn:**

    - **Express.js vs. các framework khác (ví dụ: NestJS, Fastify):**
      - **Express:** Tối giản, linh hoạt, cộng đồng lớn. Phù hợp cho người muốn tự do cấu hình.
      - **NestJS:** Framework opinionated, xây dựng trên Express (hoặc Fastify), cung cấp cấu trúc sẵn có (modules, controllers, services, DI), rất mạnh cho các ứng dụng lớn, phức tạp. Sử dụng nhiều Decorators.
      - **Fastify:** Tập trung vào hiệu năng cao, overhead thấp.
      - Lựa chọn phụ thuộc vào quy mô dự án, kinh nghiệm đội nhóm, và sở thích. Express là điểm khởi đầu tốt để hiểu các khái niệm cơ bản.

9.  **Bài tập thực hành và Gợi ý dự án nhỏ:**

    - **Câu hỏi lý thuyết:**
      1.  Nêu 5 ràng buộc chính của kiến trúc REST. Giải thích ngắn gọn ý nghĩa của "Stateless".
      2.  Middleware trong Express là gì? Cho ví dụ về một application-level middleware và một error-handling middleware.
      3.  Làm thế nào để bạn lấy dữ liệu từ `req.params`, `req.query`, và `req.body` trong một Express route handler?
      4.  Sự khác biệt giữa `app.put()` và `app.patch()` là gì trong ngữ cảnh RESTful API?
      5.  Tại sao việc sử dụng đúng HTTP status codes lại quan trọng? Cho ví dụ về 3 status codes thường dùng và ý nghĩa của chúng.
    - **Thử thách code (Coding challenges):**
      1.  **Xây dựng API CRUD đơn giản cho "Products":**
          - `GET /products`: Lấy danh sách sản phẩm.
          - `POST /products`: Tạo sản phẩm mới (yêu cầu `name`, `price`).
          - `GET /products/:id`: Lấy sản phẩm theo ID.
          - `PUT /products/:id`: Cập nhật toàn bộ sản phẩm.
          - `DELETE /products/:id`: Xóa sản phẩm.
          - Sử dụng mảng trong bộ nhớ để lưu trữ dữ liệu.
          - Thêm một logging middleware để ghi lại mỗi request.
          - Thêm một error handling middleware cơ bản.
      2.  **Tạo một middleware `validateApiKey`:** Middleware này kiểm tra xem request header `Authorization` có chứa một API key hợp lệ hay không (ví dụ: `Bearer YOUR_STATIC_API_KEY`). Nếu không, trả về lỗi 401.
    - **Gợi ý dự án nhỏ (tiếp tục dự án Quản lý công việc CLI, nhưng giờ là API):**
      - Bắt đầu xây dựng một RESTful API cho ứng dụng quản lý công việc.
      - Các endpoints có thể là:
        - `GET /tasks`
        - `POST /tasks` (body: `{ title: string, description?: string, dueDate?: string }`)
        - `GET /tasks/:id`
        - `PUT /tasks/:id` (cập nhật task)
        - `PATCH /tasks/:id` (ví dụ: chỉ cập nhật `completed: true`)
        - `DELETE /tasks/:id`
      - Sử dụng cấu trúc thư mục với `routes` và `controllers`.
      - Áp dụng TypeScript cho request/response DTOs (Data Transfer Objects).

10. **Câu hỏi phỏng vấn thường gặp:**
    - "What is REST, and what are its main principles?"
    - "Explain how middleware works in Express.js. Give an example."
    - "How do you handle errors in an Express application?"
    - "What's the difference between `req.params`, `req.query`, and `req.body`?"
    - "How would you implement authentication middleware in Express?"
    - "What are some common HTTP status codes and when would you use them?" (e.g., 200, 201, 204, 400, 401, 403, 404, 500)
    - "How do you structure routes in a large Express application?"
    - "What is CORS and why is it needed?"

---

Phần này đánh dấu một bước quan trọng: áp dụng TypeScript vào xây dựng một ứng dụng backend thực tế. Hãy thực hành nhiều với Express, thử nghiệm các loại middleware, và làm quen với luồng request-response. Khi bạn đã thoải mái, chúng ta sẽ tiếp tục với các chủ đề như làm việc với database, validation, và authentication/authorization chi tiết hơn.

Tuyệt vời! Sau khi đã xây dựng được các API cơ bản với Express.js và TypeScript, bước tiếp theo không thể thiếu trong hầu hết các ứng dụng backend là làm việc với **cơ sở dữ liệu (Database)**. Chúng ta sẽ tập trung vào việc tương tác với cơ sở dữ liệu quan hệ (SQL) sử dụng một **ORM (Object-Relational Mapper)** phổ biến là **TypeORM**.

---

### **PHẦN 9: Làm việc với Cơ sở dữ liệu SQL và TypeORM trong Backend TypeScript**

1.  **Tên phần học:** Làm việc với Cơ sở dữ liệu SQL và TypeORM trong Backend TypeScript.

2.  **Mục tiêu học phần:**

    - Hiểu khái niệm **ORM** là gì, lợi ích và nhược điểm của việc sử dụng ORM.
    - Nắm vững cách cài đặt và cấu hình **TypeORM** trong một dự án TypeScript Express.js.
    - Biết cách định nghĩa **Entities** (thực thể) trong TypeORM, tương ứng với các bảng trong cơ sở dữ liệu, sử dụng Decorators.
    - Thực hiện các thao tác **CRUD (Create, Read, Update, Delete)** cơ bản với TypeORM thông qua Repositories hoặc EntityManager.
    - Hiểu và triển khai các loại **Relations (quan hệ)** giữa các Entities: One-to-One, One-to-Many, Many-to-One, Many-to-Many.
    - Làm quen với **QueryBuilder** của TypeORM để xây dựng các truy vấn phức tạp hơn khi cần.
    - Hiểu về **Migrations** trong TypeORM để quản lý sự thay đổi schema của cơ sở dữ liệu một cách có phiên bản.
    - Biết cách xử lý **Transactions (giao dịch)** để đảm bảo tính toàn vẹn dữ liệu.
    - Tích hợp TypeORM vào các service layer trong kiến trúc ứng dụng.
    - Trả lời được:
      - ORM là gì? Tại sao nên (hoặc không nên) sử dụng ORM?
      - Làm thế nào để định nghĩa một Entity và các cột của nó trong TypeORM?
      - Cách thực hiện các truy vấn cơ bản (tìm kiếm, thêm, sửa, xóa) với TypeORM?
      - Các loại quan hệ trong TypeORM được định nghĩa và sử dụng như thế nào?
      - Migrations trong TypeORM dùng để làm gì và quy trình cơ bản là gì?
      - Khi nào nên dùng QueryBuilder thay vì các phương thức repository cơ bản?

3.  **Kiến thức tiên quyết:**

    - Hoàn thành Phần 8 (Xây dựng RESTful APIs với Express.js và TypeScript).
    - Kiến thức cơ bản về cơ sở dữ liệu quan hệ (SQL): bảng, cột, khóa chính, khóa ngoại, các câu lệnh SQL cơ bản (SELECT, INSERT, UPDATE, DELETE).
    - Hiểu biết về Promises và async/await trong JavaScript/TypeScript.
    - Có một cơ sở dữ liệu SQL đã cài đặt (ví dụ: PostgreSQL, MySQL, SQLite) để thực hành. (Chúng ta sẽ dùng PostgreSQL hoặc SQLite cho ví dụ).

4.  **Giải thích lý thuyết chuyên sâu:**

    - **ORM (Object-Relational Mapper):**

      - **Là gì?** ORM là một kỹ thuật lập trình chuyển đổi dữ liệu giữa các hệ thống không tương thích kiểu (incompatible type systems) trong lập trình hướng đối tượng. Cụ thể trong ngữ cảnh này, nó là một thư viện giúp "ánh xạ" các object trong code (ví dụ: class instances) với các bản ghi (rows) trong bảng của cơ sở dữ liệu quan hệ, và các thuộc tính của object với các cột (columns) của bảng.
      - **"TẠI SAO" sử dụng ORM? (Lợi ích)**
        1.  **Giảm code SQL lặp đi lặp lại:** ORM tự động tạo ra các câu lệnh SQL cho các thao tác CRUD cơ bản.
        2.  **Tăng năng suất:** Lập trình viên có thể làm việc với object và method quen thuộc thay vì viết SQL thuần.
        3.  **Database Agnostic (Ở một mức độ):** Nhiều ORM hỗ trợ nhiều loại CSDL khác nhau, giúp việc chuyển đổi CSDL dễ dàng hơn (mặc dù không phải lúc nào cũng hoàn hảo).
        4.  **An toàn kiểu (Type Safety):** Với TypeScript, ORM như TypeORM giúp đảm bảo kiểu dữ liệu khi tương tác với DB.
        5.  **Quản lý quan hệ dễ dàng hơn:** ORM cung cấp cách trực quan để định nghĩa và làm việc với các mối quan hệ giữa các bảng/entities.
        6.  **Tính năng nâng cao:** Hỗ trợ transactions, migrations, caching, eager/lazy loading.
      - **Nhược điểm của ORM:**
        1.  **Overhead về hiệu năng:** Lớp trừu tượng của ORM có thể gây ra một chút overhead so với SQL thuần tối ưu.
        2.  **Đường cong học tập (Learning Curve):** Cần thời gian để học cách sử dụng ORM và các API của nó.
        3.  **SQL phức tạp khó diễn đạt:** Đối với các truy vấn rất phức tạp, việc viết bằng API của ORM có thể khó khăn hoặc không hiệu quả bằng SQL thuần. (Nhiều ORM cho phép chạy raw SQL).
        4.  **"Leaky Abstraction":** Đôi khi vẫn cần hiểu về SQL và cách CSDL hoạt động để tối ưu hoặc debug.

    - **TypeORM:**

      - Là một ORM mạnh mẽ cho TypeScript và JavaScript (ES5, ES6, ES7, ES8).
      - Hỗ trợ nhiều CSDL: MySQL, PostgreSQL, MariaDB, SQLite, MS SQL Server, Oracle, SAP Hana, CockroachDB, etc.
      - Có thể chạy trong Node.js, trình duyệt, Ionic, Cordova, React Native, NativeScript, Expo, Electron.
      - Sử dụng nhiều Decorators để định nghĩa entities và relations.
      - Hỗ trợ cả Active Record và Data Mapper patterns (chúng ta sẽ tập trung vào Data Mapper qua Repositories).

    - **Cài đặt và Cấu hình TypeORM:**

      1.  **Cài đặt dependencies:**
          ```bash
          npm install typeorm reflect-metadata pg # Hoặc mysql, sqlite3, etc. tùy CSDL
          # pg là driver cho PostgreSQL
          ```
          - `reflect-metadata`: Cần thiết cho TypeORM để hoạt động với decorators và metadata. (Đã giới thiệu ở Phần 5).
      2.  **Import `reflect-metadata`:** Đảm bảo dòng này được import một lần ở đầu ứng dụng (ví dụ: `src/server.ts` hoặc `src/app.ts`):
          ```typescript
          import "reflect-metadata";
          ```
      3.  **Cấu hình `tsconfig.json`:** Đảm bảo có:
          ```json
          "emitDecoratorMetadata": true,
          "experimentalDecorators": true,
          "esModuleInterop": true, // Thường cần thiết
          ```
      4.  **Tạo file cấu hình TypeORM (`ormconfig.ts` hoặc cấu hình trong code):**
          TypeORM hỗ trợ nhiều cách cấu hình (file `ormconfig.json/yaml/js/ts`, biến môi trường, hoặc truyền object cấu hình khi tạo connection). Chúng ta sẽ dùng cách tạo object cấu hình trong code cho đơn giản ban đầu.

          ```typescript
          // src/config/database.ts (Ví dụ cấu hình)
          import { DataSource, DataSourceOptions } from 'typeorm';
          import { User } from '../api/users/user.entity'; // Import entity của bạn
          import { Product } from '../api/products/product.entity'; // Import entity khác

          // Đọc biến môi trường (ví dụ từ .env)
          import dotenv from 'dotenv';
          dotenv.config();

          export const dataSourceOptions: DataSourceOptions = {
            type: 'postgres', // Hoặc 'mysql', 'sqlite', etc.
            host: process.env.DB_HOST || 'localhost',
            port: parseInt(process.env.DB_PORT || '5432', 10),
            username: process.env.DB_USERNAME || 'your_db_user',
            password: process.env.DB_PASSWORD || 'your_db_password',
            database: process.env.DB_NAME || 'your_db_name',
            synchronize: process.env.NODE_ENV === 'development', // true: tự động tạo schema DB dựa trên entities (CHỈ DÙNG CHO DEVELOPMENT)
                                                              // false: trong production, dùng migrations
            logging: process.env.NODE_ENV === 'development' ? ['query', 'error'] : ['error'], // Log các câu query và lỗi
            entities: [User, Product /*, __dirname + '/../**/*.entity{.ts,.js}'*/], // Danh sách các entities
                                                                                      // Hoặc đường dẫn tới các file entity
            migrations: [__dirname + '/../migrations/*{.ts,.js}'], // Đường dẫn tới các file migration
            subscribers: [],
            // ssl: process.env.DB_SSL === 'true' ? { rejectUnauthorized: false } : false, // Cho Heroku Postgres, Render, etc.
          };

          const AppDataSource = new DataSource(dataSourceOptions);

          export default AppDataSource;
          ```

          **Lưu ý về `synchronize: true`:** Rất tiện cho development vì nó tự động cập nhật schema CSDL mỗi khi ứng dụng khởi động và entity thay đổi. **TUYỆT ĐỐI KHÔNG DÙNG `synchronize: true` TRONG PRODUCTION** vì nó có thể xóa dữ liệu. Trong production, phải dùng **Migrations**.

      5.  **Khởi tạo kết nối Database khi ứng dụng khởi động:**

          ```typescript
          // src/server.ts
          import "reflect-metadata";
          import express from "express";
          import AppDataSource from "./config/database"; // Import DataSource đã cấu hình
          // ... các import khác ...

          const app = express();
          const port = process.env.PORT || 3000;

          // ... middleware ...

          // Khởi tạo kết nối DB
          AppDataSource.initialize()
            .then(() => {
              console.log("[database]: Data Source has been initialized!");

              // Chỉ khởi động server Express SAU KHI DB đã kết nối thành công
              app.listen(port, () => {
                console.log(
                  `[server]: Server is running at http://localhost:${port}`
                );
              });
            })
            .catch((err) => {
              console.error(
                "[database]: Error during Data Source initialization:",
                err
              );
              process.exit(1); // Thoát ứng dụng nếu không kết nối được DB
            });

          // ... routes ...
          // ... error handler ...
          ```

    - **Định nghĩa Entities:**

      - Entity là một class được đánh dấu bằng decorator `@Entity()` và ánh xạ tới một bảng trong CSDL.
      - Các thuộc tính của class được ánh xạ tới các cột bằng các column decorators (`@PrimaryGeneratedColumn`, `@Column`, `@CreateDateColumn`, `@UpdateDateColumn`, etc.).
      - **Ví dụ: `User` Entity**

        ```typescript
        // src/api/users/user.entity.ts
        import {
          Entity,
          PrimaryGeneratedColumn,
          Column,
          CreateDateColumn,
          UpdateDateColumn,
          OneToMany,
        } from "typeorm";
        import { Post } from "../posts/post.entity"; // Giả sử có Post entity

        @Entity("users") // Tên bảng trong DB sẽ là 'users' (mặc định là tên class viết thường)
        export class User {
          @PrimaryGeneratedColumn("uuid") // Khóa chính, tự động tăng, kiểu UUID
          // @PrimaryGeneratedColumn() // Hoặc kiểu số tự tăng (INT)
          id: string; // Hoặc number nếu không dùng 'uuid'

          @Column({ type: "varchar", length: 100, unique: true })
          username: string;

          @Column({ type: "varchar", length: 255, unique: true })
          email: string;

          @Column({ type: "varchar", nullable: true }) // Cho phép NULL
          firstName?: string;

          @Column({ type: "varchar", nullable: true })
          lastName?: string;

          @Column({ default: true }) // Giá trị mặc định
          isActive: boolean;

          // Quan hệ One-to-Many với Post
          // Một User có nhiều Post
          @OneToMany(() => Post, (post) => post.author)
          posts: Post[];

          @CreateDateColumn({ type: "timestamp with time zone" }) // Tự động set khi tạo
          createdAt: Date;

          @UpdateDateColumn({ type: "timestamp with time zone" }) // Tự động set khi cập nhật
          updatedAt: Date;
        }
        ```

      - **Các Column Types phổ biến:** `varchar`, `text`, `int`, `bigint`, `float`, `double`, `decimal`, `boolean`, `date`, `time`, `timestamp`, `timestamp with time zone`, `json`, `jsonb` (cho PostgreSQL), `enum`, `uuid`, etc.
      - **Column Options:** `nullable`, `unique`, `default`, `length`, `precision`, `scale`, `enum`, `select` (false để không lấy ra khi select mặc định), `insert` (false để không cho insert), `update` (false để không cho update).

    - **Thực hiện CRUD với Repositories:**

      - TypeORM cung cấp `Repository` pattern (một dạng Data Mapper) để làm việc với entities.
      - Mỗi entity có một repository tương ứng.
      - Lấy repository: `AppDataSource.getRepository(EntityClass)`
      - **Ví dụ trong một User Service:**

        ```typescript
        // src/api/users/user.service.ts
        import { Repository } from "typeorm";
        import AppDataSource from "../../config/database";
        import { User } from "./user.entity";
        import { CreateUserDto, UpdateUserDto } from "./user.dto"; // Data Transfer Objects

        export class UserService {
          private userRepository: Repository<User>;

          constructor() {
            this.userRepository = AppDataSource.getRepository(User);
          }

          async createUser(userData: CreateUserDto): Promise<User> {
            const newUser = this.userRepository.create(userData); // Tạo instance entity, chưa lưu DB
            return this.userRepository.save(newUser); // Lưu vào DB
          }

          async findAllUsers(): Promise<User[]> {
            return this.userRepository.find({
              // relations: ['posts'], // Eager load posts (nếu cần)
              order: { createdAt: "DESC" },
            });
          }

          async findUserById(id: string): Promise<User | null> {
            return this.userRepository.findOne({
              where: { id },
              // relations: ['posts']
            });
          }

          async updateUser(
            id: string,
            updateData: UpdateUserDto
          ): Promise<User | null> {
            // Cách 1: findOne sau đó save (chạy 2 query, nhưng kích hoạt subscribers/listeners)
            // const user = await this.userRepository.findOneBy({ id });
            // if (!user) return null;
            // Object.assign(user, updateData);
            // return this.userRepository.save(user);

            // Cách 2: Dùng update (chạy 1 query, hiệu quả hơn, không load entity)
            const result = await this.userRepository.update(id, updateData);
            if (result.affected === 0) {
              // Số dòng bị ảnh hưởng
              return null;
            }
            return this.userRepository.findOneBy({ id }); // Lấy lại user đã cập nhật
          }

          async deleteUser(id: string): Promise<boolean> {
            const result = await this.userRepository.delete(id);
            return result.affected !== undefined && result.affected > 0;
          }

          async findByEmail(email: string): Promise<User | null> {
            return this.userRepository.findOneBy({ email });
          }
        }
        ```

        - **`create(entityLikeObject)`:** Tạo một instance entity mới với dữ liệu, nhưng chưa lưu vào DB.
        - **`save(entityOrEntities)`:** Lưu một hoặc nhiều entity vào DB. Nếu entity đã có ID (và tồn tại trong DB), nó sẽ update. Nếu chưa, nó sẽ insert.
        - **`find(options?)`:** Tìm nhiều entities. Options: `where`, `relations` (eager load), `order`, `skip`, `take`.
        - **`findOne(options?)`:** Tìm một entity.
        - **`findOneBy(where)`:** Tìm một entity theo điều kiện cụ thể (ví dụ: `findOneBy({ id: 1, name: "Alice" })`).
        - **`update(criteria, partialEntity)`:** Cập nhật entities khớp `criteria` với `partialEntity` mà không cần load chúng trước. Trả về `UpdateResult`.
        - **`delete(criteria)`:** Xóa entities khớp `criteria`. Trả về `DeleteResult`.
        - **`count(options?)`:** Đếm số lượng entities.

    - **Định nghĩa Relations (Quan hệ):**

      - TypeORM sử dụng decorators để định nghĩa quan hệ: `@OneToOne`, `@OneToMany`, `@ManyToOne`, `@ManyToMany`.
      - Cần chỉ định entity đối diện và (thường là) trường quan hệ nghịch đảo (inverse side).

      1.  **`@OneToOne`:**

          ```typescript
          // src/api/users/user.entity.ts
          // ...
          // import { Profile } from '../profiles/profile.entity';
          // @OneToOne(() => Profile, (profile) => profile.user, { cascade: true, onDelete: 'CASCADE' })
          // @JoinColumn() // Chỉ định class này sở hữu foreign key
          // profile: Profile;

          // src/api/profiles/profile.entity.ts
          // @Entity('profiles')
          // export class Profile {
          //   @PrimaryGeneratedColumn() id: number;
          //   @Column() bio: string;
          //   @OneToOne(() => User, (user) => user.profile)
          //   user: User;
          // }
          ```

          - `@JoinColumn()`: Đặt ở phía "sở hữu" của quan hệ (bên có foreign key).
          - `cascade: true`: Khi lưu/xóa User, Profile liên quan cũng được lưu/xóa. Dùng cẩn thận.
          - `onDelete: 'CASCADE'`: DB-level cascade delete.

      2.  **`@ManyToOne` (phía "nhiều") / `@OneToMany` (phía "một"):**
          Phổ biến nhất. Ví dụ: một User có nhiều Post, một Post thuộc về một User.

          ```typescript
          // src/api/posts/post.entity.ts
          import {
            Entity,
            PrimaryGeneratedColumn,
            Column,
            ManyToOne,
            JoinColumn,
          } from "typeorm";
          import { User } from "../users/user.entity";

          @Entity("posts")
          export class Post {
            @PrimaryGeneratedColumn()
            id: number;

            @Column()
            title: string;

            @Column("text")
            content: string;

            // Nhiều Post thuộc về một User
            @ManyToOne(() => User, (user) => user.posts, {
              onDelete: "SET NULL",
              nullable: true,
            })
            @JoinColumn({ name: "authorId" }) // Foreign key column sẽ là 'authorId' trong bảng 'posts'
            author: User; // Hoặc author: User | null;

            @Column({ nullable: true }) // Nếu author có thể null
            authorId: string; // Nên có FK id column để truy vấn dễ hơn
          }

          // src/api/users/user.entity.ts (đã có ở trên)
          // @OneToMany(() => Post, (post) => post.author)
          // posts: Post[];
          ```

      3.  **`@ManyToMany`:**

          - Cần một bảng trung gian (join table / pivot table). TypeORM có thể tự tạo bảng này.
          - Sử dụng `@JoinTable()` ở một trong hai phía (thường là phía "sở hữu" logic).

          ```typescript
          // src/api/tags/tag.entity.ts
          // @Entity('tags')
          // export class Tag {
          //   @PrimaryGeneratedColumn() id: number;
          //   @Column({ unique: true }) name: string;
          //   @ManyToMany(() => Post, (post) => post.tags)
          //   posts: Post[];
          // }

          // src/api/posts/post.entity.ts
          // ...
          // import { Tag } from '../tags/tag.entity';
          // @ManyToMany(() => Tag, (tag) => tag.posts, { cascade: ['insert'] })
          // @JoinTable({ // Cấu hình bảng trung gian
          //   name: 'post_tags_tag', // Tên bảng join
          //   joinColumn: { name: 'postId', referencedColumnName: 'id' },
          //   inverseJoinColumn: { name: 'tagId', referencedColumnName: 'id' }
          // })
          // tags: Tag[];
          ```

      - **Eager vs. Lazy Loading:**
        - **Lazy Loading:** Quan hệ không được load tự động khi query entity chính. Chúng chỉ được load khi bạn truy cập thuộc tính quan hệ đó (trả về Promise). Mặc định.
          `const user = await userRepository.findOneBy({id: 1}); const posts = await user.posts; // posts là Promise<Post[]>`
        - **Eager Loading:** Quan hệ được load tự động cùng với entity chính. Thêm `{ eager: true }` vào decorator quan hệ.
          `@OneToMany(() => Post, (post) => post.author, { eager: true })`
          Hoặc dùng `relations: ['posts']` trong `find` options.
        - **"KHI NÀO" dùng?** Eager loading tiện lợi nhưng có thể gây N+1 query problem nếu không cẩn thận. Lazy loading cho kiểm soát tốt hơn.

    - **QueryBuilder:**

      - Khi các phương thức repository (`find`, `findOneBy`) không đủ linh hoạt cho các truy vấn phức tạp (JOINs phức tạp, subqueries, aggregations).
      - Cú pháp gần giống SQL nhưng an toàn kiểu và có thể xây dựng động.
      - ```typescript
        async findUsersWithPostCount(minPostCount: number): Promise<any[]> {
          return this.userRepository.createQueryBuilder('user') // 'user' là alias cho bảng User
            .leftJoinAndSelect('user.posts', 'post') // JOIN bảng posts với alias 'post'
            .select([
                'user.id AS userId', // Chọn cột và đặt alias
                'user.username AS username',
                'COUNT(post.id) AS postCount'
            ])
            .groupBy('user.id, user.username') // Phải group by các cột không aggregate
            .having('COUNT(post.id) >= :minCount', { minCount: minPostCount }) // Điều kiện sau GROUP BY
            .orderBy('postCount', 'DESC')
            .limit(10)
            // .getRawMany(); // Trả về dữ liệu thô (array of objects)
            .getMany(); // Trả về array of User entities (nếu select entity chính)
                           // Ở đây getRawMany() phù hợp hơn vì select custom fields
        }
        // const activeUsers = await userService.findUsersWithPostCount(5);
        ```

    - **Migrations:**

      - Là cách để quản lý các thay đổi schema CSDL một cách có phiên bản, an toàn, đặc biệt trong môi trường production (nơi `synchronize: true` không được dùng).
      - Mỗi migration là một file TypeScript chứa logic `up()` (áp dụng thay đổi) và `down()` (hoàn tác thay đổi).
      - **TypeORM CLI:** Dùng để tạo, chạy, và hoàn tác migrations.
        1.  **Cài đặt CLI (nếu chưa có trong `package.json`):**
            `npm install -g typeorm` (global) hoặc thêm vào devDependencies.
            Tốt hơn là định nghĩa script trong `package.json` để dùng bản local:
            ```json
            "scripts": {
              // ...
              "typeorm": "ts-node ./node_modules/typeorm/cli.js -d ./src/config/database.ts",
              // Hoặc nếu đã build: "typeorm": "typeorm-ts-node-commonjs -d ./dist/config/database.js"
              "migration:generate": "npm run typeorm -- migration:generate ./src/migrations/$npm_config_name",
              "migration:run": "npm run typeorm -- migration:run",
              "migration:revert": "npm run typeorm -- migration:revert"
            }
            ```
            - `-d ./src/config/database.ts` (hoặc `dataSource` trong file ormconfig) chỉ định file chứa `DataSource` export.
        2.  **Tạo migration:**
            `npm run migration:generate --name=CreateUserTable` (hoặc `AddEmailToUserTable`)
            Thao tác này sẽ so sánh entities hiện tại của bạn với schema CSDL (nếu có thể kết nối) hoặc với các migration đã chạy trước đó, và tự động tạo ra một file migration mới trong thư mục `migrations` với các câu lệnh SQL (hoặc QueryRunner API) cần thiết. Bạn nên review file này.
        3.  **Chạy migrations:**
            `npm run migration:run`
            TypeORM sẽ chạy tất cả các migration mới chưa được áp dụng.
        4.  **Hoàn tác migration cuối cùng:**
            `npm run migration:revert`
      - **Ví dụ file migration (tự động tạo hoặc viết tay):**

        ```typescript
        // src/migrations/xxxxxxxxxxxxxx-CreateUserTable.ts
        import { MigrationInterface, QueryRunner, Table } from "typeorm";

        export class CreateUserTablexxxxxxxxxxxxxx
          implements MigrationInterface
        {
          public async up(queryRunner: QueryRunner): Promise<void> {
            await queryRunner.createTable(
              new Table({
                name: "users", // Tên bảng
                columns: [
                  {
                    name: "id",
                    type: "uuid",
                    isPrimary: true,
                    default: "uuid_generate_v4()",
                  },
                  {
                    name: "username",
                    type: "varchar",
                    length: "100",
                    isUnique: true,
                  },
                  { name: "email", type: "varchar", isUnique: true },
                  {
                    name: "createdAt",
                    type: "timestamp with time zone",
                    default: "now()",
                  },
                  {
                    name: "updatedAt",
                    type: "timestamp with time zone",
                    default: "now()",
                  },
                ],
              }),
              true
            ); // true để tạo foreign keys nếu có
            // await queryRunner.query(`CREATE EXTENSION IF NOT EXISTS "uuid-ossp";`); // Nếu dùng uuid_generate_v4()
          }

          public async down(queryRunner: QueryRunner): Promise<void> {
            await queryRunner.dropTable("users");
          }
        }
        ```

    - **Transactions:**

      - Đảm bảo một loạt các thao tác CSDL được thực hiện như một đơn vị công việc duy nhất: hoặc tất cả thành công, hoặc tất cả thất bại (rollback).
      - Sử dụng `AppDataSource.transaction(async (transactionalEntityManager) => { ... })` hoặc `queryRunner.startTransaction()`, `commitTransaction()`, `rollbackTransaction()`.
      - ```typescript
        async transferFunds(fromAccountId: string, toAccountId: string, amount: number): Promise<void> {
          await AppDataSource.transaction(async (entityManager) => {
            // entityManager là một instance của EntityManager, hoạt động trong transaction này
            const fromAccount = await entityManager.findOneBy(Account, { id: fromAccountId });
            const toAccount = await entityManager.findOneBy(Account, { id: toAccountId });

            if (!fromAccount || !toAccount) throw new Error("Account not found");
            if (fromAccount.balance < amount) throw new Error("Insufficient funds");

            fromAccount.balance -= amount;
            toAccount.balance += amount;

            await entityManager.save(Account, fromAccount); // Sử dụng Account entity class
            await entityManager.save(Account, toAccount);

            // Nếu có lỗi ở đây, tất cả thay đổi sẽ được rollback
            console.log(`Funds transferred successfully: ${amount} from ${fromAccountId} to ${toAccountId}`);
          });
        }
        ```

5.  **Minh họa trực quan và Code mẫu (Đã tích hợp trong phần Giải thích lý thuyết)**

    - **Sơ đồ tương tác TypeORM:**

      ```mermaid
      graph TD
          App["Application Code (Service/Controller)"] -- "Uses" --> Repo["Repository<Entity> / EntityManager"];
          Repo -- "Interacts with" --> DM["DataSource (Connection Pool)"];
          DM -- "Sends SQL / Receives Data" --> DB["SQL Database (PostgreSQL, MySQL, etc.)"];

          subgraph TypeORM_Layer
              DefineEntity["@Entity() Class"] --> Repo;
              DefineRelations["@OneToMany(), @ManyToOne(), etc."] --> Repo;
              QB["QueryBuilder API"] --> Repo;
              Migrations["Migration Files"] -- "Modifies Schema" --> DB;
              Migrations -- "Managed by TypeORM CLI" --> App;
          end
          App -- "Defines" --> DefineEntity
          App -- "Defines" --> DefineRelations
      ```

6.  **Best Practices và Quy ước (Conventions):**

    - **Sử dụng Migrations trong production, `synchronize: true` chỉ cho development.**
    - **Tách biệt Entities, Repositories (hoặc Services sử dụng Repositories) và Controllers.** Tuân thủ SRP.
    - **Sử dụng DTOs (Data Transfer Objects) cho dữ liệu đầu vào API, không nên truyền trực tiếp Entity instance từ request body.** Giúp validation và tách biệt.
    - **Xử lý lỗi CSDL một cách cẩn thận** (ví dụ: unique constraint violations, connection errors) và trả về status codes phù hợp.
    - **Cẩn thận với N+1 query problem khi load relations.** Sử dụng `relations` trong find options, `joinAndSelect` trong QueryBuilder, hoặc Dataloader pattern (nâng cao).
    - **Đóng kết nối CSDL khi ứng dụng thoát (graceful shutdown).**
    - **Sử dụng biến môi trường cho cấu hình CSDL.**
    - **Viết unit/integration tests cho service layer tương tác với DB (có thể dùng DB test riêng hoặc mocking/stubbing).**

7.  **Anti-patterns và Lỗi thường gặp (Common Pitfalls):**

    - **Dùng `synchronize: true` trong production.**
    - **Fat entities / Anemic domain model:** Entities chứa quá nhiều logic nghiệp vụ, hoặc ngược lại, chỉ là các túi dữ liệu không có hành vi.
    - **Lạm dụng eager loading:** Gây N+1 query.
    - **Không xử lý transaction cho các thao tác cần tính toàn vẹn.**
    - **Viết logic nghiệp vụ phức tạp trực tiếp trong QueryBuilder trong controller.** Nên đưa vào service layer.
    - **Quên định nghĩa inverse side của relation hoặc `@JoinColumn`/`@JoinTable`.**
    - **Lỗi kết nối CSDL không được xử lý đúng cách làm crash app.**

8.  **So sánh, Đánh giá và Lựa chọn:**

    - **TypeORM vs. Prisma vs. Sequelize vs. Knex.js:**
      - **TypeORM:** Rất mạnh về TypeScript, nhiều tính năng, hỗ trợ cả Active Record và Data Mapper. Đường cong học tập có thể hơi dốc.
      - **Prisma:** Cách tiếp cận hiện đại, schema-first (định nghĩa schema, Prisma Client được tạo tự động), an toàn kiểu rất tốt. Không phải là ORM truyền thống.
      - **Sequelize:** ORM lâu đời cho Node.js, nhiều tính năng, cộng đồng lớn. API có thể hơi cũ hơn.
      - **Knex.js:** Query Builder, không phải ORM đầy đủ. Linh hoạt hơn để viết SQL, nhưng ít trừu tượng hơn.
      - Lựa chọn phụ thuộc vào sở thích, yêu cầu dự án. TypeORM và Prisma là những lựa chọn phổ biến hàng đầu cho TypeScript.

9.  **Bài tập thực hành và Gợi ý dự án nhỏ:**

    - **Câu hỏi lý thuyết:**
      1.  ORM là gì? Nêu 2 lợi ích và 1 nhược điểm của việc sử dụng ORM.
      2.  Sự khác biệt giữa `repository.save()` và `repository.update()` trong TypeORM là gì?
      3.  Giải thích sự khác biệt giữa eager loading và lazy loading relations. Khi nào nên dùng mỗi loại?
      4.  Tại sao migrations lại quan trọng và tại sao không nên dùng `synchronize: true` trong production?
      5.  Mô tả cách bạn sẽ định nghĩa một quan hệ Many-to-Many giữa `Student` và `Course` trong TypeORM.
    - **Thử thách code (Coding challenges):**
      1.  **Mở rộng API Products (từ Phần 8):**
          - Tạo `Product` entity (id, name, description, price, stockQuantity, createdAt, updatedAt).
          - Refactor các route handlers của Product API để sử dụng TypeORM và ProductService.
          - Thêm một `Category` entity và tạo quan hệ Many-to-One từ `Product` đến `Category` (một Product thuộc một Category, một Category có nhiều Product).
          - Thêm endpoint `GET /categories` và `GET /categories/:id/products`.
      2.  **Viết một migration:** Sau khi tạo entity `Product` và `Category`, sử dụng TypeORM CLI để tạo một migration tự động. Review và chỉnh sửa migration nếu cần, sau đó chạy nó.
      3.  **Sử dụng QueryBuilder:** Viết một phương thức trong `ProductService` để tìm các sản phẩm có giá trong một khoảng nhất định (`minPrice`, `maxPrice`) và thuộc một danh sách các `categoryIds`, sắp xếp theo tên.
    - **Gợi ý dự án nhỏ (tiếp tục API Quản lý công việc):**
      - Tạo các entities `Task` và `User` (nếu muốn gán task cho user).
      - Định nghĩa quan hệ (ví dụ: một User có nhiều Task).
      - Refactor toàn bộ API để sử dụng TypeORM để lưu trữ và truy xuất dữ liệu tasks.
      - Triển khai transactions cho các thao tác phức tạp (nếu có).

10. **Câu hỏi phỏng vấn thường gặp:**
    - "What is an ORM? What are the pros and cons of using one?"
    - "How do you define an entity and its columns in TypeORM?"
    - "Explain how to perform basic CRUD operations using TypeORM repositories."
    - "How do you define different types of relationships (One-to-Many, Many-to-Many) in TypeORM?"
    - "What are database migrations and why are they important? How does TypeORM handle them?"
    - "When would you use TypeORM's QueryBuilder instead of simple repository methods?"
    - "What is the N+1 query problem, and how can you mitigate it with TypeORM?"
    - "How do you handle database transactions in TypeORM?"
    - "What does `synchronize: true` do in TypeORM configuration, and why is it dangerous in production?"

---

Làm việc với cơ sở dữ liệu là một phần không thể thiếu của backend. TypeORM cung cấp một cách mạnh mẽ và an toàn kiểu để tương tác với CSDL trong môi trường TypeScript. Hãy thực hành nhiều với việc định nghĩa entities, relations, và thực hiện các truy vấn. Khi bạn đã tự tin, chúng ta sẽ tiếp tục với các chủ đề quan trọng khác như validation và authentication.

Chắc chắn rồi! Sau khi đã có thể lưu trữ và truy xuất dữ liệu với TypeORM, một bước cực kỳ quan trọng để đảm bảo chất lượng và tính toàn vẹn của dữ liệu đầu vào cho API của chúng ta là **Validation (Kiểm tra tính hợp lệ của dữ liệu)**. Chúng ta sẽ tìm hiểu cách thực hiện validation hiệu quả trong ứng dụng Express.js/TypeScript, thường sử dụng các thư viện chuyên dụng.

---

### **PHẦN 10: Validation Dữ Liệu Đầu Vào và DTOs trong Backend TypeScript**

1.  **Tên phần học:** Validation Dữ Liệu Đầu Vào và DTOs trong Backend TypeScript.

2.  **Mục tiêu học phần:**

    - Hiểu tầm quan trọng của việc **validation dữ liệu đầu vào** từ client (request body, query params, route params).
    - Nắm vững khái niệm **DTO (Data Transfer Object)** và vai trò của nó trong việc định nghĩa cấu trúc dữ liệu cho request và response.
    - Sử dụng các thư viện validation phổ biến như **`class-validator`** kết hợp với **`class-transformer`** để thực hiện validation dựa trên decorator cho DTOs.
    - Biết cách tạo các **custom validation rules** (quy tắc kiểm tra tùy chỉnh).
    - Tích hợp logic validation vào Express.js thông qua **middleware**.
    - Xử lý và trả về các **thông báo lỗi validation** một cách thân thiện cho client.
    - Hiểu sự khác biệt giữa validation ở tầng controller/API và validation ở tầng CSDL (ví dụ: constraints của TypeORM).
    - Áp dụng các kỹ thuật này để xây dựng API mạnh mẽ và an toàn hơn.
    - Trả lời được:
      - Tại sao validation dữ liệu đầu vào lại quan trọng?
      - DTO là gì và tại sao nên sử dụng DTO thay vì truyền trực tiếp `req.body` vào service?
      - Làm thế nào để sử dụng `class-validator` để định nghĩa các quy tắc validation cho một DTO?
      - Cách tạo một validation middleware cho Express để tự động kiểm tra DTO?
      - Làm thế nào để tùy chỉnh thông báo lỗi từ `class-validator`?
      - Sự khác biệt giữa validation ở API layer và database constraints là gì?

3.  **Kiến thức tiên quyết:**

    - Hoàn thành Phần 8 và 9 (Express.js, TypeORM).
    - Hiểu biết về Decorators trong TypeScript (Phần 5).
    - Kiến thức về HTTP request/response.

4.  **Giải thích lý thuyết chuyên sâu:**

    - **Tầm quan trọng của Validation Dữ liệu Đầu Vào:**

      - **"TẠI SAO" phải validate?**
        1.  **Bảo mật (Security):** Ngăn chặn các tấn công như SQL Injection, XSS (Cross-Site Scripting), NoSQL Injection, etc., bằng cách đảm bảo dữ liệu đầu vào có định dạng và kiểu mong đợi, loại bỏ các ký tự hoặc cấu trúc độc hại.
        2.  **Tính toàn vẹn dữ liệu (Data Integrity):** Đảm bảo dữ liệu lưu trữ trong CSDL là đúng đắn, nhất quán và tuân thủ các quy tắc nghiệp vụ.
        3.  **Trải nghiệm người dùng (User Experience):** Cung cấp phản hồi lỗi rõ ràng và sớm cho người dùng nếu họ nhập liệu sai, giúp họ sửa lỗi dễ dàng hơn.
        4.  **Độ tin cậy của hệ thống (System Reliability):** Ngăn chặn các lỗi không mong muốn hoặc crash server do dữ liệu không hợp lệ gây ra.
        5.  **Giảm lỗi ở các tầng sau:** Validate sớm ở API gateway/controller giúp giảm tải cho service layer và database layer.
      - **Nguyên tắc "Never trust user input":** Luôn giả định rằng dữ liệu từ client (hoặc bất kỳ nguồn bên ngoài nào) có thể không hợp lệ, không đầy đủ, hoặc thậm chí là độc hại.

    - **DTO (Data Transfer Object):**

      - **Là gì?** DTO là một object đơn giản được sử dụng để truyền dữ liệu giữa các layer của một ứng dụng, đặc biệt là giữa client và server, hoặc giữa các service. DTO thường chỉ chứa dữ liệu và không có logic nghiệp vụ.
      - Trong ngữ cảnh API, DTOs thường được định nghĩa bằng class hoặc interface để mô tả cấu trúc (shape) của request body, query parameters, hoặc response body.
      - **"TẠI SAO" sử dụng DTOs?**
        1.  **Định nghĩa "contract" rõ ràng:** Xác định rõ ràng dữ liệu nào được mong đợi từ client và dữ liệu nào sẽ được trả về.
        2.  **An toàn kiểu (Type Safety):** Với TypeScript, DTOs (thường là class) cung cấp type checking tại compile-time.
        3.  **Validation:** DTOs là nơi lý tưởng để đính kèm các quy tắc validation (sử dụng decorators với `class-validator`).
        4.  **Tách biệt (Decoupling):** Tách biệt cấu trúc dữ liệu của API khỏi cấu trúc entity của CSDL. API có thể thay đổi DTO mà không nhất thiết phải thay đổi entity và ngược lại. Ví dụ: DTO có thể bỏ qua một số trường nhạy cảm của entity.
        5.  **Transformation (Biến đổi dữ liệu):** Dễ dàng biến đổi dữ liệu từ DTO sang Entity và ngược lại (sử dụng `class-transformer`).
      - **Ví dụ DTO cho việc tạo User:**

        ```typescript
        // src/api/users/dtos/create-user.dto.ts
        import {
          IsEmail,
          IsString,
          MinLength,
          IsNotEmpty,
          IsOptional,
          IsBoolean,
        } from "class-validator";

        export class CreateUserDto {
          @IsNotEmpty({ message: "Username không được để trống" })
          @IsString()
          @MinLength(3, { message: "Username phải có ít nhất 3 ký tự" })
          username: string;

          @IsNotEmpty({ message: "Email không được để trống" })
          @IsEmail({}, { message: "Email không hợp lệ" })
          email: string;

          @IsNotEmpty({ message: "Mật khẩu không được để trống" })
          @MinLength(6, { message: "Mật khẩu phải có ít nhất 6 ký tự" })
          password: string;

          @IsOptional() // Thuộc tính này có thể có hoặc không
          @IsString()
          firstName?: string;

          @IsOptional()
          @IsString()
          lastName?: string;

          @IsOptional()
          @IsBoolean()
          isActive?: boolean;
        }
        ```

    - **`class-validator` và `class-transformer`:**

      - **`class-validator`:** Một thư viện TypeScript/JavaScript cho phép sử dụng decorators để định nghĩa các quy tắc validation cho class.
      - **`class-transformer`:** Một thư viện giúp biến đổi plain object (ví dụ: từ `req.body`) thành instance của class (DTO) và ngược lại, cũng như thực hiện các phép biến đổi dữ liệu khác (ví dụ: `string` sang `number`, `string` sang `Date`). Nó thường được dùng cùng `class-validator` vì `class-validator` cần một instance của class để hoạt động.
      - **Cài đặt:**
        ```bash
        npm install class-validator class-transformer
        ```
        Đảm bảo `experimentalDecorators` và `emitDecoratorMetadata` được bật trong `tsconfig.json`.
      - **Một số Decorators phổ biến của `class-validator`:**
        - **Common:** `IsDefined`, `IsOptional`, `Equals`, `NotEquals`, `IsEmpty`, `IsNotEmpty`
        - **Type checks:** `IsBoolean`, `IsDate`, `IsNumber`, `IsString`, `IsArray`, `IsEnum`, `IsInt`, `IsObject`
        - **Number checks:** `Min`, `Max`, `IsPositive`, `IsNegative`
        - **String checks:** `MinLength`, `MaxLength`, `Contains`, `NotContains`, `IsAlpha`, `IsAlphanumeric`, `IsAscii`, `IsBase64`, `IsCreditCard`, `IsEmail`, `IsFQDN`, `IsHexColor`, `IsIn` (enum-like), `IsISBN`, `IsJSON`, `IsLowercase`, `IsMongoId`, `IsPassportNumber`, `IsUrl`, `IsUUID`, `IsUppercase`, `Matches` (regex)
        - **Date checks:** `MinDate`, `MaxDate`
        - **Array checks:** `ArrayContains`, `ArrayNotContains`, `ArrayNotEmpty`, `ArrayMinSize`, `ArrayMaxSize`, `ArrayUnique`
        - **Object checks:** `IsInstance`
        - **Nested validation:** `@ValidateNested({ each: true })` cho mảng các object, `@Type(() => NestedDto)` từ `class-transformer` để chỉ định kiểu của object lồng nhau.
        - Mỗi decorator có thể nhận một `ValidationOptions` object để tùy chỉnh thông báo lỗi (`message`) hoặc `groups` (cho validation theo group).

    - **Tích hợp Validation vào Express.js (Validation Middleware):**

      - Cách tốt nhất là tạo một middleware chung để tự động validate DTOs.
      - Middleware này sẽ:
        1.  Nhận vào kiểu DTO cần validate.
        2.  Sử dụng `class-transformer` để chuyển đổi `req.body` (hoặc `req.query`, `req.params`) thành một instance của DTO đó.
        3.  Sử dụng `class-validator` để validate instance DTO.
        4.  Nếu có lỗi, trả về response lỗi 400 (Bad Request) với chi tiết lỗi.
        5.  Nếu không có lỗi, gọi `next()` để chuyển sang route handler.
      - **Ví dụ Validation Middleware:**

        ```typescript
        // src/middlewares/validation.middleware.ts
        import {
          Request,
          Response,
          NextFunction,
          RequestHandler,
        } from "express";
        import { validate, ValidationError } from "class-validator";
        import { plainToInstance } from "class-transformer"; // Sử dụng plainToInstance thay vì plainToClass
        import { ClassConstructor } from "class-transformer/types/interfaces"; // Để gõ kiểu cho DTO class

        // Hàm tiện ích để format lỗi (tùy chọn)
        const formatValidationErrors = (errors: ValidationError[]): any => {
          const formattedErrors: { [key: string]: string[] } = {};
          errors.forEach((error) => {
            if (error.constraints) {
              formattedErrors[error.property] = Object.values(
                error.constraints
              );
            }
            // Xử lý lỗi lồng nhau (nested errors) nếu có
            if (error.children && error.children.length > 0) {
              const nestedErrors = formatValidationErrors(error.children);
              for (const key in nestedErrors) {
                formattedErrors[`${error.property}.${key}`] = nestedErrors[key];
              }
            }
          });
          return formattedErrors;
        };

        // Type cho DTO class, sử dụng ClassConstructor từ class-transformer
        export function validationMiddleware<T extends object>(
          dtoClass: ClassConstructor<T>,
          value: "body" | "query" | "params" = "body", // Mặc định validate body
          skipMissingProperties = false, // Tùy chọn của class-validator
          whitelist = true, // Loại bỏ các thuộc tính không có trong DTO
          forbidNonWhitelisted = true // Báo lỗi nếu có thuộc tính không có trong DTO
        ): RequestHandler {
          return async (req: Request, res: Response, next: NextFunction) => {
            // Chuyển đổi plain object (req.body, req.query, req.params) thành instance của DTO class
            const dtoInstance = plainToInstance(dtoClass, req[value]);

            // Validate instance
            const errors: ValidationError[] = await validate(dtoInstance, {
              skipMissingProperties,
              whitelist,
              forbidNonWhitelisted,
            });

            if (errors.length > 0) {
              // res.status(400).json({
              //   message: 'Validation failed',
              //   errors: formatValidationErrors(errors), // Format lỗi cho dễ đọc
              // });
              // Hoặc đơn giản hơn:
              res.status(400).json({
                errors: errors.map((err) => ({
                  property: err.property,
                  constraints: err.constraints,
                  children: err.children, // Để client tự xử lý lỗi nested nếu cần
                })),
              });
            } else {
              // Gán DTO đã được validate và transform vào req để handler sau có thể dùng
              // Điều này rất quan trọng, vì req.body gốc là plain object,
              // còn dtoInstance là instance của class, có thể có methods hoặc default values.
              req[value] = dtoInstance;
              next();
            }
          };
        }
        ```

      - **Sử dụng Validation Middleware trong Routes:**

        ```typescript
        // src/api/users/user.routes.ts
        import { Router } from "express";
        import { UserController } from "./user.controller";
        import { CreateUserDto } from "./dtos/create-user.dto";
        import { UpdateUserDto } from "./dtos/update-user.dto";
        import { validationMiddleware } from "../../middlewares/validation.middleware";
        import { FindUserParamsDto } from "./dtos/find-user-params.dto";

        const router = Router();

        router.post(
          "/",
          validationMiddleware(CreateUserDto), // Validate req.body với CreateUserDto
          UserController.createUser
        );

        router.get(
          "/:id",
          validationMiddleware(FindUserParamsDto, "params"), // Validate req.params
          UserController.getUserById
        );

        router.put(
          "/:id",
          validationMiddleware(FindUserParamsDto, "params"), // Validate ID trước
          validationMiddleware(UpdateUserDto, "body", true), // true: skipMissingProperties cho phép partial update
          UserController.updateUser
        );

        // ... các routes khác
        export default router;
        ```

        Và trong `user.controller.ts`, bạn có thể dùng `req.body` đã được validate và transform:

        ```typescript
        // src/api/users/user.controller.ts
        import { Request, Response, NextFunction } from "express";
        import { CreateUserDto } from "./dtos/create-user.dto";
        // ...

        export class UserController {
          public static async createUser(
            req: Request,
            res: Response,
            next: NextFunction
          ): Promise<void> {
            try {
              const createUserDto = req.body as CreateUserDto; // req.body giờ là instance của CreateUserDto
              // const userService = new UserService();
              // const newUser = await userService.create(createUserDto);
              // res.status(201).json(newUser);
              res.status(201).json({
                message: "User created (simulated)",
                data: createUserDto,
              });
            } catch (error) {
              next(error);
            }
          }
          // ...
        }
        ```

    - **Custom Validation Rules:**

      - `class-validator` cho phép bạn tạo các decorator validation tùy chỉnh.
      - Bạn cần tạo một class implement `ValidatorConstraintInterface`.
      - Sử dụng decorator `@ValidatorConstraint` để đăng ký.
      - **Ví dụ: Kiểm tra username không được là "admin"**

        ```typescript
        // src/validators/is-not-admin.validator.ts
        import {
          ValidatorConstraint,
          ValidatorConstraintInterface,
          ValidationArguments,
          registerDecorator,
          ValidationOptions,
        } from "class-validator";

        @ValidatorConstraint({ name: "isNotAdmin", async: false })
        export class IsNotAdminConstraint
          implements ValidatorConstraintInterface
        {
          validate(username: string, args: ValidationArguments) {
            return username.toLowerCase() !== "admin";
          }

          defaultMessage(args: ValidationArguments) {
            return 'Username cannot be "admin".';
          }
        }

        // Decorator function để sử dụng trong DTO
        export function IsNotAdmin(validationOptions?: ValidationOptions) {
          return function (object: Object, propertyName: string) {
            registerDecorator({
              target: object.constructor,
              propertyName: propertyName,
              options: validationOptions,
              constraints: [],
              validator: IsNotAdminConstraint, // Sử dụng constraint đã tạo
            });
          };
        }

        // Sử dụng trong DTO:
        // import { IsNotAdmin } from '../../validators/is-not-admin.validator';
        // class RegisterDto {
        //   @IsNotAdmin({ message: 'Tên đăng nhập không được là admin.'})
        //   @IsString()
        //   username: string;
        //   // ...
        // }
        ```

    - **Validation ở các tầng khác nhau:**
      - **API Layer (Controller/Middleware):**
        - Kiểm tra định dạng, kiểu dữ liệu, sự tồn tại của các trường bắt buộc, độ dài, giá trị hợp lệ.
        - Mục tiêu: Trả về lỗi sớm cho client, bảo vệ service layer khỏi dữ liệu rác.
        - Thường dùng `class-validator`.
      - **Service Layer (Business Logic):**
        - Kiểm tra các quy tắc nghiệp vụ phức tạp hơn.
        - Ví dụ: Kiểm tra xem email đã tồn tại chưa (cần truy vấn DB), một sản phẩm có đủ số lượng trong kho không.
        - Có thể throw custom business exceptions.
      - **Database Layer (Entity/ORM Constraints):**
        - Các ràng buộc ở CSDL (`NOT NULL`, `UNIQUE`, `FOREIGN KEY`, `CHECK` constraints).
        - Mục tiêu: Đảm bảo tính toàn vẹn dữ liệu ở mức cuối cùng, ngay cả khi các tầng trên có lỗi.
        - TypeORM entities có thể định nghĩa một số constraint (ví dụ: `@Column({ unique: true })`).
      - **Sự phối hợp:** Các tầng validation nên bổ sung cho nhau, không thay thế hoàn toàn. Validate sớm nhất có thể (API layer) để có trải nghiệm người dùng tốt nhất và giảm tải cho hệ thống.

5.  **Minh họa trực quan và Code mẫu (Đã tích hợp trong phần Giải thích lý thuyết)**

    - **Luồng Validation Dữ liệu:**
      ```mermaid
      graph LR
          ClientRequest["Client Request (Body/Query/Params)"] --> A[Express Route];
          A --> VM["Validation Middleware (using class-validator & class-transformer)"];
          VM -- "Invalid Data" --> ErrorResponse["400 Bad Request (Validation Errors)"];
          ErrorResponse --> ClientResponse["Client Receives Error"];
          VM -- "Valid Data (DTO instance)" --> RH["Route Handler / Controller"];
          RH --> SL["Service Layer (Business Logic Validation)"];
          SL -- "Business Rule Violation" --> CustomErrorResponse["Custom Error Response (e.g., 409 Conflict)"];
          CustomErrorResponse --> ClientResponse;
          SL -- "Valid for Business Logic" --> DB["Database Interaction (TypeORM - DB Constraints)"];
          DB -- "DB Constraint Violation" --> DBErrorResponse["500 Server Error / Specific DB Error"];
          DBErrorResponse --> ClientResponse;
          DB -- "Success" --> SuccessResponse["2xx Success Response"];
          SuccessResponse --> ClientResponse;
      ```

6.  **Best Practices và Quy ước (Conventions):**

    - **Luôn validate tất cả dữ liệu đầu vào từ các nguồn không đáng tin cậy.**
    - **Sử dụng DTOs để định nghĩa cấu trúc dữ liệu cho API request/response.**
    - **Đặt các decorator validation trực tiếp trên DTO class.**
    - **Sử dụng middleware validation để giữ cho controller logic sạch sẽ.**
    - **Cung cấp thông báo lỗi validation rõ ràng, cụ thể và (nếu có thể) đa ngôn ngữ.**
    - **Phân biệt rõ ràng giữa validation lỗi (400 Bad Request) và lỗi nghiệp vụ (ví dụ: 409 Conflict, 404 Not Found).**
    - **`whitelist: true` và `forbidNonWhitelisted: true` trong `validate` options của `class-validator` là các thiết lập tốt để tăng tính bảo mật và chặt chẽ.**
    - **Không nên tin tưởng validation ở client-side. Luôn validate ở server-side.**
    - **Sử dụng các message key thay vì hardcode message trong DTO để hỗ trợ i18n (quốc tế hóa).**

7.  **Anti-patterns và Lỗi thường gặp (Common Pitfalls):**

    - **Bỏ qua validation hoặc validate không đầy đủ.**
    - **Validation logic bị trộn lẫn trong controller hoặc service, thay vì tập trung ở DTO hoặc middleware.**
    - **Trả về thông báo lỗi không rõ ràng hoặc quá chung chung.**
    - **Lộ thông tin nhạy cảm trong thông báo lỗi validation (ví dụ: tên biến nội bộ).**
    - **Không sử dụng `class-transformer` để chuyển đổi plain object sang DTO instance trước khi validate:** Decorator của `class-validator` có thể không hoạt động đúng nếu không có instance của class.
    - **Quên xử lý lỗi từ `validate()` (là một Promise).**
    - **Nhầm lẫn giữa việc dùng `IsOptional` và việc cho phép trường đó là `null` hoặc `undefined`.** `IsOptional` nghĩa là trường đó có thể không có mặt trong request. Nếu có mặt, nó vẫn phải tuân theo các rule khác (trừ khi rule đó cũng có `if` condition).

8.  **So sánh, Đánh giá và Lựa chọn:**

    - **`class-validator` vs. `Joi` vs. `Zod` vs. `Yup`:**
      - **`class-validator`:** Tích hợp tốt với class và decorators, phù hợp với OOP và các framework như NestJS. Cần `class-transformer`.
      - **`Joi`:** Thư viện validation mạnh mẽ, schema-based, API linh hoạt. Phổ biến trong hệ sinh thái Hapi.js.
      - **`Zod`:** Thư viện schema declaration và validation hiện đại, an toàn kiểu rất tốt với TypeScript (suy luận kiểu từ schema). Không cần decorators. Rất được ưa chuộng gần đây.
      - **`Yup`:** Tương tự Joi, schema-based, thường dùng với Formik trong React.
      - Lựa chọn:
        - Nếu đã dùng nhiều class và decorators (ví dụ với TypeORM, NestJS), `class-validator` là lựa chọn tự nhiên.
        - Nếu thích schema-first và type inference mạnh mẽ, `Zod` rất đáng cân nhắc.
        - `Joi` và `Yup` cũng là những lựa chọn tốt, tùy thuộc vào hệ sinh thái và sở thích.

9.  **Bài tập thực hành và Gợi ý dự án nhỏ:**

    - **Câu hỏi lý thuyết:**
      1.  Tại sao DTO lại hữu ích khi xây dựng API, đặc biệt là với TypeScript?
      2.  Sự khác biệt giữa `IsOptional()` và `nullable: true` (trong TypeORM entity) là gì?
      3.  Làm thế nào `class-transformer` hỗ trợ `class-validator`?
      4.  Nêu một ví dụ về custom validation rule bạn có thể cần tạo.
      5.  Trong luồng request, validation middleware nên được đặt ở đâu?
    - **Thử thách code (Coding challenges):**
      1.  **Tạo DTO và Validation cho Product API (tiếp tục từ Phần 9):**
          - `CreateProductDto`: `name (string, required, minLength:3)`, `description (string, optional, maxLength:500)`, `price (number, required, positive)`, `stockQuantity (number, optional, integer, min:0)`.
          - `UpdateProductDto`: Tương tự `CreateProductDto` nhưng tất cả các trường đều optional (sử dụng `PartialType` từ `@nestjs/mapped-types` nếu dùng NestJS, hoặc tự định nghĩa).
          - Tích hợp validation middleware cho các endpoint tạo và cập nhật product.
      2.  **Tạo DTO cho Query Parameters:**
          - `GetProductsQueryDto`: `limit (number, optional, IsInt, Min:1, Max:100, default:10)`, `offset (number, optional, IsInt, Min:0, default:0)`, `sortBy (string, optional, IsIn:['name', 'price', 'createdAt'])`, `order (string, optional, IsIn:['ASC', 'DESC'], default:'ASC')`.
          - Sử dụng `validationMiddleware` để validate `req.query`.
          - Sử dụng `class-transformer` decorators như `@Type(() => Number)` để tự động chuyển đổi string từ query params sang number.
    - **Gợi ý dự án nhỏ (tiếp tục API Quản lý công việc):**
      - Định nghĩa các DTOs (ví dụ: `CreateTaskDto`, `UpdateTaskDto`, `TaskQueryDto`) với các validation rules phù hợp cho các thuộc tính của Task.
      - Tích hợp validation middleware vào tất cả các route của Task API.
      - Đảm bảo trả về lỗi validation rõ ràng cho client.

10. **Câu hỏi phỏng vấn thường gặp:**
    - "Why is input validation crucial in backend applications?"
    - "What are DTOs (Data Transfer Objects) and how do they help in API development?"
    - "How would you implement request validation in an Express.js application using TypeScript? Mention any libraries you'd use."
    - "Explain how to use `class-validator` decorators to define validation rules."
    - "How can you create a custom validation decorator with `class-validator`?"
    - "What's the role of `class-transformer` when working with `class-validator` and DTOs?"
    - "How should validation errors be communicated back to the client?"
    - "Where should different types of validation (e.g., format, business rules, database constraints) occur in an application stack?"

---

Validation là một lớp phòng vệ quan trọng cho ứng dụng của bạn. Việc sử dụng DTOs kết hợp với các thư viện validation mạnh mẽ như `class-validator` sẽ giúp code của bạn sạch sẽ, an toàn và dễ bảo trì hơn rất nhiều. Hãy thực hành tạo DTOs và áp dụng các rule validation khác nhau. Khi bạn sẵn sàng, chúng ta sẽ đến với một chủ đề quan trọng không kém: Authentication và Authorization.

Chắc chắn rồi! Sau khi đã đảm bảo dữ liệu đầu vào API được validate cẩn thận, bước tiếp theo để bảo vệ tài nguyên và chức năng của ứng dụng là triển khai **Authentication (Xác thực)** và **Authorization (Phân quyền)**. Đây là những khía cạnh cực kỳ quan trọng của bất kỳ hệ thống backend nào.

---

### **PHẦN 11: Authentication và Authorization trong Backend TypeScript**

1.  **Tên phần học:** Authentication và Authorization trong Backend TypeScript.

2.  **Mục tiêu học phần:**

    - Hiểu rõ sự khác biệt giữa **Authentication (AuthN)** và **Authorization (AuthZ)**.
    - Nắm vững các cơ chế Authentication phổ biến:
      - **Session-based Authentication** (truyền thống).
      - **Token-based Authentication** (ví dụ: JWT - JSON Web Tokens).
    - Triển khai **JWT-based Authentication** trong ứng dụng Express.js/TypeScript:
      - Tạo (sign) JWT khi người dùng đăng nhập thành công.
      - Lưu trữ JWT ở client (ví dụ: HTTP-only cookie, Local Storage - thảo luận ưu nhược điểm).
      - Xác minh (verify) JWT trong các request tiếp theo thông qua middleware.
      - Xử lý refresh tokens để duy trì phiên đăng nhập.
    - Hiểu và triển khai **Password Hashing** (ví dụ: sử dụng `bcrypt`) để lưu trữ mật khẩu an toàn.
    - Triển khai các chiến lược **Authorization** cơ bản:
      - Role-Based Access Control (RBAC).
      - Permission-based Access Control (hoặc attribute-based).
    - Sử dụng **Passport.js** (một thư viện authentication middleware phổ biến cho Node.js) để đơn giản hóa việc triển khai các chiến lược AuthN khác nhau (local, OAuth, etc.).
    - Tích hợp AuthN/AuthZ middleware vào các routes Express.
    - Hiểu các vấn đề bảo mật liên quan đến AuthN/AuthZ và cách giảm thiểu chúng.
    - Trả lời được:
      - Phân biệt Authentication và Authorization.
      - JWT là gì? Cấu trúc của một JWT? Ưu và nhược điểm của JWT?
      - Tại sao cần hash mật khẩu và bcrypt hoạt động như thế nào?
      - Refresh token dùng để làm gì và luồng hoạt động của nó?
      - RBAC là gì và làm thế nào để triển khai nó trong Express?
      - Passport.js giúp ích gì trong việc triển khai authentication?

3.  **Kiến thức tiên quyết:**

    - Hoàn thành Phần 8, 9, 10 (Express.js, TypeORM, Validation, DTOs).
    - Hiểu biết về Middleware trong Express.
    - Kiến thức cơ bản về mật mã học (hashing, signing) là một lợi thế.
    - HTTP Headers, Cookies.

4.  **Giải thích lý thuyết chuyên sâu:**

    - **Authentication (Xác thực - AuthN) vs. Authorization (Phân quyền - AuthZ):**

      - **Authentication (Ai là bạn?):** Là quá trình xác minh danh tính của một người dùng, thiết bị hoặc hệ thống. Trả lời câu hỏi "Bạn là ai?".
        - Ví dụ: Người dùng cung cấp username và password để đăng nhập. Server kiểm tra thông tin này với CSDL.
      - **Authorization (Bạn được phép làm gì?):** Là quá trình xác định xem một người dùng (đã được xác thực) có quyền truy cập vào một tài nguyên cụ thể hoặc thực hiện một hành động cụ thể hay không. Trả lời câu hỏi "Bạn được phép làm gì?".
        - Ví dụ: Sau khi đăng nhập, một người dùng "editor" có thể sửa bài viết, nhưng người dùng "viewer" chỉ có thể xem.
      - **Thứ tự:** Authentication luôn diễn ra trước Authorization. Bạn không thể cấp quyền cho một người mà bạn không biết họ là ai.

    - **Các Cơ chế Authentication Phổ biến:**

      1.  **Session-based Authentication:**

          - **Luồng hoạt động:**
            1.  Client gửi credentials (username/password) đến server.
            2.  Server xác thực credentials.
            3.  Nếu hợp lệ, server tạo một session ID duy nhất, lưu trữ thông tin session (ví dụ: user ID) trên server (trong bộ nhớ, DB, hoặc cache như Redis).
            4.  Server gửi session ID về cho client, thường qua HTTP cookie (ví dụ: `connect.sid`).
            5.  Trong các request tiếp theo, client tự động gửi cookie chứa session ID.
            6.  Server dùng session ID để tra cứu thông tin session và xác định người dùng.
          - **Ưu điểm:** Đơn giản để triển khai với nhiều framework, thông tin session được quản lý tập trung ở server.
          - **Nhược điểm:**
            - **Stateful:** Server phải lưu trữ trạng thái session, gây khó khăn cho việc scaling ngang (horizontal scaling) nếu không có giải pháp chia sẻ session (ví dụ: sticky sessions, session store tập trung).
            - **CORS:** Có thể phức tạp hơn với Cross-Origin Resource Sharing nếu client và server ở các domain khác nhau (cần cấu hình cookie đúng cách).
            - **CSRF (Cross-Site Request Forgery):** Cần các biện pháp phòng chống CSRF.

      2.  **Token-based Authentication (Ví dụ: JWT):**
          - **Luồng hoạt động (với JWT):**
            1.  Client gửi credentials đến server.
            2.  Server xác thực credentials.
            3.  Nếu hợp lệ, server tạo một JSON Web Token (JWT) chứa thông tin người dùng (payload) và ký (sign) nó bằng một secret key (hoặc private key).
            4.  Server gửi JWT về cho client.
            5.  Client lưu trữ JWT (ví dụ: Local Storage, Session Storage, HTTP-only cookie) và gửi nó trong header `Authorization` (thường là `Bearer <token>`) của các request tiếp theo.
            6.  Server nhận request, trích xuất JWT từ header, xác minh chữ ký (verify signature) và tính hợp lệ (ví dụ: thời gian hết hạn) của token. Nếu hợp lệ, server tin tưởng thông tin trong payload của token để xác định người dùng.
          - **JWT (JSON Web Token):**
            - Một tiêu chuẩn mở (RFC 7519) định nghĩa một cách nhỏ gọn và tự chứa (self-contained) để truyền thông tin giữa các bên dưới dạng một JSON object.
            - Thông tin này có thể được xác minh và tin cậy vì nó được ký điện tử.
            - **Cấu trúc JWT (3 phần, được phân cách bởi dấu chấm `.`, base64url encoded):**
              1.  **Header:** Chứa thông tin về thuật toán ký (ví dụ: HS256, RS256) và loại token (thường là "JWT").
                  `{"alg": "HS256", "typ": "JWT"}`
              2.  **Payload (Claims):** Chứa các thông tin (claims) về người dùng hoặc các dữ liệu khác. Có 3 loại claims:
                  - **Registered claims:** Các claim được định nghĩa sẵn (ví dụ: `iss` - issuer, `exp` - expiration time, `sub` - subject, `aud` - audience).
                  - **Public claims:** Các claim tùy chỉnh nhưng nên được định nghĩa trong IANA JSON Web Token Registry hoặc là URI để tránh xung đột.
                  - **Private claims:** Các claim tùy chỉnh, được thỏa thuận giữa các bên sử dụng token.
                    `{"sub": "user123", "name": "John Doe", "admin": true, "iat": 1516239022, "exp": 1516242622}`
                    **Lưu ý:** Payload được encode, không được mã hóa (encrypt). **Không lưu trữ thông tin nhạy cảm trong payload JWT nếu không mã hóa token.**
              3.  **Signature (Chữ ký):** Được tạo bằng cách ký header đã encode, payload đã encode, và một secret (nếu dùng thuật toán đối xứng như HS256) hoặc private key (nếu dùng thuật toán bất đối xứng như RS256).
                  `HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)`
            - **Ưu điểm của JWT:**
              - **Stateless:** Server không cần lưu trữ thông tin token (trừ secret key). Dễ dàng scaling.
              - **Self-contained:** Token chứa thông tin người dùng, giảm thiểu truy vấn DB để lấy thông tin user.
              - **CORS-friendly:** Dễ dàng sử dụng qua các domain khác nhau (gửi qua header).
              - **Mobile-friendly:** Thích hợp cho các ứng dụng di động.
            - **Nhược điểm của JWT:**
              - **Kích thước token:** Có thể lớn hơn session ID nếu chứa nhiều thông tin trong payload.
              - **Không thể thu hồi (revoke) token một cách dễ dàng trước khi hết hạn:** Một khi token đã được cấp, nó hợp lệ cho đến khi hết hạn. Để thu hồi, cần các giải pháp phức tạp hơn (ví dụ: blacklist token). Đây là một nhược điểm lớn.
              - **Bảo mật secret key:** Nếu secret key bị lộ, kẻ tấn công có thể tạo token giả mạo.
              - **XSS nếu lưu trong Local Storage:** Nếu token được lưu trong Local Storage, nó có thể bị đánh cắp qua tấn công XSS. HTTP-only cookie an toàn hơn về mặt này.

    - **Password Hashing:**

      - **"TẠI SAO" hash mật khẩu?** **TUYỆT ĐỐI KHÔNG BAO GIỜ lưu trữ mật khẩu dưới dạng plain text (văn bản thuần túy) trong CSDL.** Nếu CSDL bị xâm phạm, tất cả mật khẩu sẽ bị lộ.
      - Hashing là quá trình chuyển đổi một chuỗi (mật khẩu) thành một chuỗi có độ dài cố định khác (hash) bằng một thuật toán một chiều. Không thể dễ dàng đảo ngược hash để lấy lại mật khẩu gốc.
      - **Salt:** Một chuỗi ngẫu nhiên được thêm vào mật khẩu trước khi hash. Mục đích là để ngay cả khi hai người dùng có cùng mật khẩu, hash của họ sẽ khác nhau (do salt khác nhau). Giúp chống lại rainbow table attacks.
      - **Iteration Count (Cost Factor/Rounds):** Số lần thuật toán hash được lặp lại. Số lần lặp càng cao, việc tính toán hash càng tốn thời gian, làm cho brute-force attacks khó khăn hơn.
      - **`bcrypt`:** Một thuật toán hashing mật khẩu phổ biến, được thiết kế để chậm và tốn tài nguyên, rất tốt cho việc hash mật khẩu. Nó đã tích hợp sẵn salt.

        - Cài đặt: `npm install bcrypt` và `npm install --save-dev @types/bcrypt`
        - **Sử dụng `bcrypt`:**

          ```typescript
          import bcrypt from "bcrypt";

          const saltRounds = 10; // Số vòng lặp, càng cao càng an toàn nhưng càng chậm

          // Hash mật khẩu khi đăng ký user
          export async function hashPassword(
            plainPassword: string
          ): Promise<string> {
            const salt = await bcrypt.genSalt(saltRounds);
            const hashedPassword = await bcrypt.hash(plainPassword, salt);
            // Hoặc ngắn gọn: const hashedPassword = await bcrypt.hash(plainPassword, saltRounds);
            return hashedPassword;
          }

          // So sánh mật khẩu khi đăng nhập
          export async function comparePassword(
            plainPassword: string,
            hashedPassword: string
          ): Promise<boolean> {
            return bcrypt.compare(plainPassword, hashedPassword);
          }

          // Ví dụ sử dụng
          // async function registerUser(username: string, pass: string) {
          //   const hashed = await hashPassword(pass);
          //   // Lưu username và hashed vào DB
          // }
          // async function loginUser(username: string, pass: string) {
          //   // Lấy hashedPass từ DB dựa trên username
          //   const hashedPassFromDB = "...."; //
          //   const isMatch = await comparePassword(pass, hashedPassFromDB);
          //   if (isMatch) { /* Đăng nhập thành công */ }
          // }
          ```

    - **Triển khai JWT-based Authentication trong Express:**

      1.  **Cài đặt `jsonwebtoken`:**
          ```bash
          npm install jsonwebtoken
          npm install --save-dev @types/jsonwebtoken
          ```
      2.  **Tạo JWT (Sign):** Thường trong route đăng nhập.

          ```typescript
          // src/api/auth/auth.controller.ts
          import jwt from "jsonwebtoken";
          import { User } from "../users/user.entity"; // Giả sử
          // ... (userService, comparePassword)

          // Trong hàm login
          // if (user && await comparePassword(password, user.password)) {
          //   const payload = {
          //     userId: user.id,
          //     username: user.username,
          //     roles: user.roles // Giả sử user có mảng roles
          //   };
          //   const accessToken = jwt.sign(
          //     payload,
          //     process.env.JWT_ACCESS_SECRET!, // Secret key từ biến môi trường
          //     { expiresIn: process.env.JWT_ACCESS_EXPIRES_IN || '15m' } // Thời gian hết hạn (ví dụ: 15 phút)
          //   );
          //   const refreshToken = jwt.sign( // Refresh token có thời gian hết hạn dài hơn
          //     { userId: user.id },
          //     process.env.JWT_REFRESH_SECRET!,
          //     { expiresIn: process.env.JWT_REFRESH_EXPIRES_IN || '7d' }
          //   );
          //   // Lưu refreshToken vào DB, gắn với user, và có thể có trạng thái (valid/revoked)
          //   // Gửi accessToken (và có thể cả refreshToken) về cho client
          //   // res.cookie('refreshToken', refreshToken, { httpOnly: true, secure: process.env.NODE_ENV === 'production', sameSite: 'strict', maxAge: 7 * 24 * 60 * 60 * 1000 }); // Lưu refresh token trong httpOnly cookie
          //   // res.json({ accessToken });
          // }
          ```

          **Lưu ý về Secret Keys:** `JWT_ACCESS_SECRET` và `JWT_REFRESH_SECRET` phải là các chuỗi ngẫu nhiên, mạnh và được giữ bí mật tuyệt đối, thường lưu trong biến môi trường.

      3.  **Xác minh JWT (Verify) - Authentication Middleware:**

          ```typescript
          // src/middlewares/auth.middleware.ts (ví dụ đã có ở phần 8, mở rộng)
          import { Request, Response, NextFunction } from "express";
          import jwt, { JwtPayload } from "jsonwebtoken";

          // Mở rộng interface Request của Express để thêm thuộc tính user
          declare global {
            namespace Express {
              interface Request {
                user?: JwtPayload | { id: string; [key: string]: any }; // Hoặc kiểu User entity đã được fetch
              }
            }
          }

          export const authenticateToken = (
            req: Request,
            res: Response,
            next: NextFunction
          ) => {
            const authHeader = req.headers["authorization"];
            const token = authHeader && authHeader.split(" ")[1]; // Bearer TOKEN

            if (!token) {
              return res
                .status(401)
                .json({ message: "Unauthorized: No token provided" }); // 401 Unauthorized
            }

            jwt.verify(
              token,
              process.env.JWT_ACCESS_SECRET!,
              (err: any, decodedPayload: any) => {
                if (err) {
                  if (err.name === "TokenExpiredError") {
                    return res
                      .status(401)
                      .json({ message: "Unauthorized: Token expired" });
                  }
                  return res
                    .status(403)
                    .json({ message: "Forbidden: Invalid token" }); // 403 Forbidden
                }
                req.user = decodedPayload as JwtPayload; // Gắn payload đã giải mã vào req.user
                next();
              }
            );
          };

          // Sử dụng:
          // import { authenticateToken } from './middlewares/auth.middleware';
          // app.get('/protected-route', authenticateToken, (req, res) => {
          //   // req.user sẽ có thông tin từ token payload
          //   res.json({ message: "Welcome to protected route!", user: req.user });
          // });
          ```

      4.  **Refresh Tokens:**
          - **Mục đích:** Access token thường có thời gian sống ngắn để giảm thiểu rủi ro nếu bị lộ. Refresh token có thời gian sống dài hơn, được dùng để lấy access token mới khi access token cũ hết hạn mà không cần người dùng đăng nhập lại.
          - **Luồng hoạt động:**
            1.  Khi access token hết hạn, client gửi refresh token (thường được lưu an toàn hơn, ví dụ HTTP-only cookie) đến một endpoint đặc biệt (ví dụ: `/auth/refresh-token`).
            2.  Server xác minh refresh token (kiểm tra trong DB xem nó có hợp lệ và chưa bị thu hồi không).
            3.  Nếu hợp lệ, server cấp một access token mới (và có thể cả một refresh token mới - refresh token rotation).
          - **Lưu trữ Refresh Token:** Nên lưu trữ an toàn ở server (ví dụ: bảng `refresh_tokens` trong DB, có `userId`, `token_hash`, `expiresAt`, `isRevoked`).
          - **Thu hồi Refresh Token:** Khi người dùng đăng xuất, hoặc nghi ngờ bị lộ, refresh token tương ứng phải được đánh dấu là đã thu hồi (revoked).

    - **Triển khai Authorization (Phân quyền):**
      Sau khi người dùng đã được xác thực (có `req.user`), cần kiểm tra xem họ có quyền thực hiện hành động hay không.

      1.  **Role-Based Access Control (RBAC):**

          - Gán vai trò (roles) cho người dùng (ví dụ: 'admin', 'editor', 'viewer').
          - Định nghĩa quyền hạn cho từng vai trò.
          - Kiểm tra vai trò của người dùng để cấp/từ chối quyền.
          - **Middleware cho RBAC:**

            ```typescript
            // src/middlewares/authorization.middleware.ts
            export const authorizeRoles = (...allowedRoles: string[]) => {
              return (req: Request, res: Response, next: NextFunction) => {
                if (!req.user || !req.user.roles) {
                  // Giả sử req.user.roles là mảng string
                  return res
                    .status(403)
                    .json({ message: "Forbidden: Roles not available" });
                }
                const userRoles = Array.isArray(req.user.roles)
                  ? req.user.roles
                  : [req.user.roles];
                const hasPermission = userRoles.some((role) =>
                  allowedRoles.includes(role)
                );

                if (hasPermission) {
                  next();
                } else {
                  res.status(403).json({
                    message:
                      "Forbidden: You do not have permission to access this resource",
                  });
                }
              };
            };

            // Sử dụng:
            // import { authenticateToken } from './auth.middleware';
            // import { authorizeRoles } from './authorization.middleware';
            // app.get('/admin/dashboard',
            //   authenticateToken,
            //   authorizeRoles('admin', 'superadmin'), // Chỉ admin hoặc superadmin được vào
            //   adminController.getDashboard
            // );
            // app.post('/articles',
            //   authenticateToken,
            //   authorizeRoles('admin', 'editor'), // Admin hoặc editor được tạo bài
            //   articleController.createArticle
            // );
            ```

      2.  **Permission-based (or Attribute-based) Access Control (PBAC/ABAC):**
          - Linh hoạt hơn RBAC. Quyền được định nghĩa dựa trên các "permissions" (ví dụ: `create:article`, `edit:user`, `delete:comment`) hoặc các thuộc tính (attributes) của người dùng, tài nguyên, và môi trường.
          - Có thể phức tạp hơn để quản lý nhưng cho phép kiểm soát chi tiết hơn.
          - Thường dùng các thư viện như `casl.js`.

    - **Passport.js:**

      - Một thư viện authentication middleware cực kỳ phổ biến và linh hoạt cho Node.js.
      - Không tự mình triển khai logic authentication, mà cung cấp một framework để sử dụng các "strategies" (chiến lược) authentication khác nhau.
      - **Strategies:** Các module riêng biệt để xử lý các cơ chế authentication cụ thể (ví dụ: `passport-local` cho username/password, `passport-jwt` cho JWT, `passport-google-oauth20` cho Google OAuth).
      - **"TẠI SAO" Passport.js?**
        - Đơn giản hóa việc tích hợp nhiều cơ chế authentication.
        - Tách biệt logic authentication khỏi route handlers.
        - Cộng đồng lớn, nhiều strategies có sẵn.
      - **Cài đặt (ví dụ cho local và jwt):**
        ```bash
        npm install passport passport-local passport-jwt
        npm install --save-dev @types/passport @types/passport-local @types/passport-jwt
        ```
      - **Cách hoạt động cơ bản:**
        1.  **Cấu hình Strategy:** Định nghĩa cách Passport xác thực người dùng cho một strategy cụ thể (ví dụ: kiểm tra username/password với DB cho `passport-local`).
        2.  **Serialize/Deserialize User:** (Thường dùng cho session-based) Định nghĩa cách lưu trữ thông tin user vào session và cách lấy lại từ session. Với JWT, điều này ít quan trọng hơn vì token đã self-contained.
        3.  **Sử dụng Middleware:** Áp dụng middleware `passport.authenticate('strategy-name', options)` vào các route cần bảo vệ.
      - **Ví dụ: `passport-jwt` Strategy**

        ```typescript
        // src/config/passport.config.ts
        import passport from "passport";
        import {
          Strategy as JwtStrategy,
          ExtractJwt,
          StrategyOptions,
        } from "passport-jwt";
        import { User } from "../api/users/user.entity"; // Your User entity
        import AppDataSource from "./database"; // Your TypeORM DataSource

        const opts: StrategyOptions = {
          jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(), // Trích xuất token từ header 'Authorization: Bearer <token>'
          secretOrKey: process.env.JWT_ACCESS_SECRET!, // Secret key để verify token
          // issuer: 'your-app.com', // (Tùy chọn)
          // audience: 'your-app.com', // (Tùy chọn)
        };

        passport.use(
          new JwtStrategy(opts, async (jwt_payload, done) => {
            try {
              // jwt_payload là object đã được giải mã từ token
              const userRepository = AppDataSource.getRepository(User);
              const user = await userRepository.findOneBy({
                id: jwt_payload.userId,
              });

              if (user) {
                return done(null, user); // Thành công, trả về user object (sẽ được gán vào req.user)
              } else {
                return done(null, false); // Không tìm thấy user, xác thực thất bại
              }
            } catch (error) {
              return done(error, false); // Lỗi trong quá trình xác thực
            }
          })
        );

        export default passport;

        // Trong src/app.ts
        // import passportConfig from './config/passport.config';
        // app.use(passportConfig.initialize()); // Khởi tạo Passport

        // Sử dụng trong route:
        // app.get('/profile',
        //   passportConfig.authenticate('jwt', { session: false }), // {session: false} vì dùng token, không dùng session
        //   (req, res) => {
        //     res.json({ user: req.user }); // req.user là user object từ JwtStrategy
        //   }
        // );
        ```

5.  **Minh họa trực quan và Code mẫu (Đã tích hợp trong phần Giải thích lý thuyết)**

    - **Luồng JWT Authentication & Authorization:**

      ```mermaid
      sequenceDiagram
          participant Client
          participant Server
          participant DB as Database

          Client->>Server: POST /login (username, password)
          Server->>DB: Find user by username
          DB-->>Server: User data (incl. hashedPassword)
          Server->>Server: Compare(password, hashedPassword)
          alt Credentials Valid
              Server->>Server: Generate AccessToken (payload, secret, short_exp)
              Server->>Server: Generate RefreshToken (userId, secret, long_exp)
              Server->>DB: Store RefreshToken (hashed or encrypted)
              DB-->>Server: Store Success
              Server-->>Client: { accessToken, (optional) refreshTokenInHttpOnlyCookie }
          else Credentials Invalid
              Server-->>Client: 401 Unauthorized
          end

          Client->>Server: GET /protected-resource (Header: Authorization: Bearer accessToken)
          Server->>Server: Middleware: Verify AccessToken
          alt Token Valid & Not Expired
              Server->>Server: Middleware: req.user = decodedPayload
              Server->>Server: Middleware: Check Roles/Permissions (Authorization)
              alt Authorized
                  Server->>Server: Process request (e.g., access DB)
                  Server-->>Client: 200 OK (Protected Data)
              else Not Authorized
                  Server-->>Client: 403 Forbidden
              end
          else Token Invalid or Expired
              Server-->>Client: 401 Unauthorized (or 403 Forbidden)
          end

          Note over Client,Server: If AccessToken expired, Client might use RefreshToken...
          Client->>Server: POST /refresh-token (RefreshToken in body or cookie)
          Server->>DB: Verify RefreshToken (check if valid & not revoked)
          DB-->>Server: RefreshToken status
          alt RefreshToken Valid
              Server->>Server: Generate new AccessToken
              Server-->>Client: { newAccessToken }
          else RefreshToken Invalid
              Server-->>Client: 401 Unauthorized (force re-login)
          end
      ```

6.  **Best Practices và Quy ước (Conventions):**

    - **Luôn hash mật khẩu sử dụng thuật toán mạnh (bcrypt, Argon2, scrypt) với salt và cost factor đủ lớn.**
    - **Sử dụng HTTPS cho tất cả các giao tiếp** để bảo vệ token và dữ liệu khi truyền đi.
    - **Giữ access token có thời gian sống ngắn.**
    - **Sử dụng refresh tokens để cải thiện UX và bảo mật.**
    - **Lưu trữ refresh tokens an toàn ở server và có cơ chế thu hồi.**
    - **Không lưu trữ thông tin quá nhạy cảm trong payload của JWT (vì nó chỉ được encode, không encrypt).**
    - **Sử dụng HTTP-only, Secure, SameSite cookies để lưu trữ refresh token (hoặc access token nếu không dùng Local Storage) để giảm thiểu XSS và CSRF.**
    - **Thực hiện rate limiting trên các endpoint đăng nhập và refresh token để chống brute-force.**
    - **Có cơ chế thu hồi token (ví dụ: blacklist) cho các trường hợp khẩn cấp.**
    - **Triển khai cơ chế "đăng xuất" hợp lý (thu hồi refresh token ở server, xóa token ở client).**
    - **Thường xuyên cập nhật thư viện (jsonwebtoken, bcrypt, passport) để vá lỗi bảo mật.**
    - **Sử dụng các secret key mạnh, ngẫu nhiên và quản lý chúng an toàn (biến môi trường, secret manager).**

7.  **Anti-patterns và Lỗi thường gặp (Common Pitfalls):**

    - **Lưu trữ mật khẩu plain text.**
    - **Sử dụng thuật toán hash yếu hoặc không có salt (MD5, SHA1).**
    - **Thời gian sống của access token quá dài.**
    - **Không có cơ chế refresh token, bắt người dùng đăng nhập lại thường xuyên.**
    - **Lưu trữ JWT trong Local Storage mà không có biện pháp chống XSS đầy đủ.**
    - **Secret key yếu hoặc bị hardcode trong code.**
    - **Không validate hoặc sanitize input cho các trường liên quan đến auth (ví dụ: username khi query DB).**
    - **Logic authorization quá phức tạp hoặc bị rò rỉ trong nhiều nơi.**
    - **Quên xử lý các trường hợp lỗi của `jwt.verify()` (ví dụ: `TokenExpiredError`, `JsonWebTokenError`).**

8.  **So sánh, Đánh giá và Lựa chọn:**

    - **JWT vs. PASETO (Platform-Agnostic SEcurity TOkens):**
      - PASETO là một giải pháp thay thế hiện đại hơn cho JWT, nhằm giải quyết một số vấn đề bảo mật của JWT (ví dụ: `alg:none` attack, lựa chọn thuật toán). PASETO có các phiên bản, và phiên bản an toàn nhất (v2, v4) mặc định sử dụng các thuật toán mạnh và không cho phép "không thuật toán". Ít phổ biến hơn JWT.
    - **Tự xây dựng Auth vs. Sử dụng dịch vụ Auth bên thứ ba (Auth0, Firebase Auth, Okta):**
      - **Tự xây dựng:** Kiểm soát hoàn toàn, nhưng đòi hỏi kiến thức sâu về bảo mật và tốn thời gian.
      - **Dịch vụ bên thứ ba:** Nhanh chóng, nhiều tính năng (MFA, social login), thường được cập nhật bảo mật. Nhưng có chi phí và phụ thuộc vào nhà cung cấp.
      - Lựa chọn: Cho các dự án nhỏ hoặc cần ra mắt nhanh, dịch vụ bên thứ ba có thể tốt. Cho các hệ thống lớn, tùy chỉnh cao, tự xây dựng có thể cần thiết.

9.  **Bài tập thực hành và Gợi ý dự án nhỏ:**

    - **Câu hỏi lý thuyết:**
      1.  JWT là gì? Nêu 3 phần chính của một JWT và giải thích ý nghĩa của chúng.
      2.  Tại sao việc sử dụng refresh token lại được khuyến nghị khi dùng JWT?
      3.  Sự khác biệt giữa `bcrypt.hash()` và `bcrypt.compare()` là gì?
      4.  Passport.js giải quyết vấn đề gì trong authentication? "Strategy" trong Passport.js là gì?
      5.  Mô tả một cách bạn có thể triển khai Role-Based Access Control (RBAC) cho một API endpoint.
    - **Thử thách code (Coding challenges):**
      1.  **Triển khai đầy đủ luồng đăng ký và đăng nhập JWT:**
          - Endpoint `/auth/register` (POST): Nhận `username`, `email`, `password`. Hash password, lưu user vào DB (dùng TypeORM).
          - Endpoint `/auth/login` (POST): Nhận `email`, `password`. So sánh password, nếu thành công, tạo access token và refresh token. Gửi access token trong response body, refresh token trong HTTP-only cookie.
          - Endpoint `/auth/refresh-token` (POST): Nhận refresh token từ cookie. Xác minh nó (có thể cần tra cứu trong DB). Nếu hợp lệ, cấp access token mới.
          - Tạo middleware `authenticateToken` để bảo vệ các route khác.
      2.  **Thêm RBAC vào Product API (từ Phần 10):**
          - Thêm trường `roles: string[]` (ví dụ: `['user', 'admin']`) vào `User` entity và JWT payload.
          - Tạo middleware `authorizeRoles` (như ví dụ ở trên).
          - Yêu cầu role 'admin' để có thể POST, PUT, DELETE products. Role 'user' (hoặc bất kỳ ai đã xác thực) có thể GET products.
    - **Gợi ý dự án nhỏ (tiếp tục API Quản lý công việc):**
      - Triển khai authentication đầy đủ cho API Task. Chỉ người dùng đã đăng nhập mới có thể tạo, xem, sửa, xóa task của chính họ.
      - (Nâng cao) Thêm vai trò 'admin' có thể xem/quản lý tất cả các task.

10. **Câu hỏi phỏng vấn thường gặp:**
    - "What is the difference between authentication and authorization?"
    - "Explain how JWT-based authentication works. What are its pros and cons?"
    - "Why is password hashing important? What hashing algorithm would you recommend and why?"
    - "What is the purpose of a refresh token?"
    - "How would you implement role-based access control (RBAC) in an Express.js API?"
    - "What are some common security vulnerabilities related to authentication and how would you prevent them?"
    - "Have you used Passport.js? How does it help with authentication?"
    - "Where would you store JWTs on the client-side and why (Local Storage vs. Cookies)?"
    - "How do you handle token revocation with JWTs?"

---

Authentication và Authorization là những phần phức tạp và cực kỳ quan trọng. Hãy dành thời gian để hiểu rõ các khái niệm và thực hành cẩn thận. An toàn bảo mật phải luôn là ưu tiên hàng đầu. Khi bạn đã tự tin với phần này, chúng ta sẽ khám phá cách viết test cho ứng dụng backend.

Tuyệt vời! Sau khi đã xây dựng các tính năng cốt lõi như API, làm việc với CSDL, validation, và authentication/authorization, một phần không thể thiếu để đảm bảo chất lượng và sự ổn định của ứng dụng backend là **Testing (Kiểm thử)**. Chúng ta sẽ tìm hiểu các loại test khác nhau và cách triển khai chúng trong dự án TypeScript.

---

### **PHẦN 12: Testing trong Backend TypeScript - Đảm Bảo Chất Lượng và Sự Ổn Định**

1.  **Tên phần học:** Testing trong Backend TypeScript - Đảm Bảo Chất Lượng và Sự Ổn Định.

2.  **Mục tiêu học phần:**

    - Hiểu tầm quan trọng của việc viết test cho ứng dụng backend.
    - Phân biệt các loại test phổ biến: **Unit Tests, Integration Tests, End-to-End (E2E) Tests**.
    - Nắm vững cách viết **Unit Tests** cho các function, class, service trong TypeScript sử dụng một testing framework như **Jest**.
      - Sử dụng matchers, `describe`, `it`/`test`, `beforeEach`, `afterEach`, etc.
      - Hiểu và áp dụng **Mocking** và **Spying** để cô lập unit đang test.
    - Hiểu cách viết **Integration Tests** để kiểm thử sự tương tác giữa các module/component (ví dụ: service và database, controller và service).
    - Làm quen với việc viết **E2E Tests** cho API endpoints sử dụng **Supertest** cùng với Jest.
    - Hiểu khái niệm **Test Coverage** và cách đo lường nó.
    - Biết cách cấu hình môi trường testing và chạy test trong dự án TypeScript.
    - Áp dụng các nguyên tắc để viết test tốt: FIRST (Fast, Independent, Repeatable, Self-validating, Timely).
    - Trả lời được:
      - Tại sao testing lại quan trọng trong phát triển phần mềm?
      - Sự khác biệt giữa Unit, Integration, và E2E test là gì?
      - Làm thế nào để mock một dependency trong Jest?
      - Supertest dùng để làm gì khi test API?
      - Test coverage là gì và tại sao nó hữu ích (nhưng không phải là tất cả)?
      - Một test "tốt" cần có những đặc điểm gì?

3.  **Kiến thức tiên quyết:**

    - Hoàn thành các phần trước, đặc biệt là kiến thức về Express.js, TypeORM, và cấu trúc dự án.
    - Kiến thức cơ bản về JavaScript/TypeScript.
    - Sẵn sàng học một testing framework (Jest).

4.  **Giải thích lý thuyết chuyên sâu:**

    - **Tầm quan trọng của Testing:**

      - **"TẠI SAO" viết test?**
        1.  **Đảm bảo chất lượng (Quality Assurance):** Phát hiện lỗi sớm trong quá trình phát triển, trước khi release.
        2.  **Tăng sự tự tin khi refactor và thay đổi code:** Test đóng vai trò như một "mạng lưới an toàn", đảm bảo các thay đổi không phá vỡ chức năng hiện có.
        3.  **Tài liệu sống (Living Documentation):** Test case mô tả cách code nên hoạt động và cách sử dụng các API/module.
        4.  **Cải thiện thiết kế code:** Viết code dễ test thường dẫn đến thiết kế module hóa tốt hơn, ít phụ thuộc hơn (tuân thủ SOLID).
        5.  **Giảm chi phí sửa lỗi:** Lỗi được phát hiện sớm thường tốn ít chi phí và công sức để sửa hơn lỗi được phát hiện muộn (ví dụ: sau khi đã deploy).
        6.  **Hỗ trợ CI/CD (Continuous Integration/Continuous Deployment):** Test tự động là một phần quan trọng của quy trình CI/CD.

    - **Các Loại Test Phổ Biến (Kim tự tháp Testing - Testing Pyramid):**

      - Kim tự tháp testing minh họa tỷ lệ và chi phí của các loại test:
        ```
            /\         <-- E2E Tests (Ít, Chậm, Đắt)
           /  \
          /____\       <-- Integration Tests (Vừa phải)
         /______\
        /________\     <-- Unit Tests (Nhiều, Nhanh, Rẻ)
        ```

      1.  **Unit Tests (Kiểm thử Đơn vị):**

          - **Mục tiêu:** Kiểm tra các đơn vị code nhỏ nhất một cách độc lập (ví dụ: một hàm, một method của class, một component nhỏ).
          - **Đặc điểm:** Nhanh, cô lập, dễ viết và debug.
          - **Cách thực hiện:** Mock tất cả các dependencies bên ngoài (ví dụ: database, external APIs, file system) để chỉ test logic của unit đó.
          - **"TẠI SAO"?** Phát hiện lỗi logic cụ thể trong từng phần nhỏ của code.

      2.  **Integration Tests (Kiểm thử Tích hợp):**

          - **Mục tiêu:** Kiểm tra sự tương tác giữa các module hoặc component đã được unit test.
          - **Đặc điểm:** Chậm hơn unit test, phức tạp hơn để thiết lập.
          - **Cách thực hiện:** Có thể sử dụng các dependency thực (ví dụ: một test database) hoặc mock một số dependency không phải là trọng tâm của test.
          - Ví dụ: Test xem service layer có gọi đúng phương thức của repository và xử lý kết quả đúng không; test xem controller có gọi đúng service và trả về response API đúng không.
          - **"TẠI SAO"?** Phát hiện lỗi ở các điểm giao tiếp, đảm bảo các phần của hệ thống hoạt động cùng nhau đúng cách.

      3.  **End-to-End (E2E) Tests (Kiểm thử Đầu cuối) / UI Tests / API Tests:**
          - **Mục tiêu:** Kiểm tra toàn bộ luồng hoạt động của ứng dụng từ góc độ người dùng (hoặc client API).
          - **Đặc điểm:** Chậm nhất, đắt nhất để viết và duy trì, dễ bị "flaky" (kết quả không ổn định).
          - **Cách thực hiện:** Mô phỏng hành vi người dùng thực sự, tương tác với UI (nếu là frontend) hoặc gửi HTTP request đến API endpoints thực sự, và kiểm tra response/trạng thái cuối cùng. Thường chạy trên một môi trường giống production nhất có thể (staging).
          - **"TẠI SAO"?** Đảm bảo toàn bộ hệ thống hoạt động đúng như mong đợi từ đầu đến cuối. Bắt được các lỗi mà unit/integration test có thể bỏ sót.

    - **Jest - Testing Framework:**

      - Là một testing framework JavaScript phổ biến, được phát triển bởi Facebook.
      - **"TẠI SAO" Jest?**
        - **"Zero configuration" (Gần như):** Dễ dàng thiết lập.
        - **Tích hợp sẵn:** Bao gồm test runner, assertion library (expect), mocking library.
        - **Snapshot Testing:** Lưu lại "ảnh chụp" của UI component hoặc data structure và so sánh khi test chạy lại.
        - **Parallel Test Execution:** Chạy test song song để tăng tốc.
        - **Code Coverage Reports:** Tích hợp sẵn.
        - Hoạt động tốt với TypeScript (cần `ts-jest`).
      - **Cài đặt Jest và các dependencies liên quan:**
        ```bash
        npm install --save-dev jest ts-jest @types/jest
        # jest: Core Jest library
        # ts-jest: TypeScript preprocessor cho Jest (để Jest hiểu code TS)
        # @types/jest: Type definitions cho Jest
        ```
      - **Cấu hình Jest (`jest.config.js` hoặc trong `package.json`):**
        Tạo file `jest.config.js` ở root project:
        ```javascript
        // jest.config.js
        module.exports = {
          preset: "ts-jest", // Sử dụng ts-jest preset
          testEnvironment: "node", // Môi trường test cho backend Node.js
          roots: ["<rootDir>/src", "<rootDir>/tests"], // Nơi Jest tìm file test và source
          testMatch: [
            // Pattern để tìm file test
            "**/tests/**/*.test.ts", // Ví dụ: tests/users/user.service.test.ts
            "**/__tests__/**/*.ts", // Cách đặt tên file test khác
            "**/?(*.)+(spec|test).ts",
          ],
          moduleFileExtensions: ["ts", "tsx", "js", "jsx", "json", "node"],
          collectCoverage: true, // Bật thu thập coverage
          coverageReporters: ["json", "lcov", "text", "clover", "html"], // Các định dạng report
          coverageDirectory: "coverage", // Thư mục chứa report
          // moduleNameMapper: { // Nếu dùng path aliases trong tsconfig.json
          //   '^@app/(.*)$': '<rootDir>/src/$1',
          // },
          // setupFilesAfterEnv: ['<rootDir>/tests/setup.ts'], // File chạy trước mỗi test suite (ví dụ: setup DB test)
        };
        ```
      - **Thêm script vào `package.json`:**
        ```json
        "scripts": {
          // ...
          "test": "jest",
          "test:watch": "jest --watchAll",
          "test:cov": "jest --coverage"
        },
        ```
      - **Cú pháp Jest cơ bản:**
        - **`describe(name, fn)`:** Nhóm các test case liên quan lại (test suite).
        - **`it(name, fn)` hoặc `test(name, fn)`:** Định nghĩa một test case riêng lẻ.
        - **`expect(value).matcher(expectedValue)`:** Assertion - kiểm tra xem `value` có khớp với `expectedValue` bằng một `matcher` hay không.
          - Phổ biến matchers: `toBe()`, `toEqual()`, `toBeTruthy()`, `toBeFalsy()`, `toBeNull()`, `toBeUndefined()`, `toContain()`, `toHaveLength()`, `toThrow()`, `toMatchObject()`, etc.
        - **Setup/Teardown Hooks:**
          - `beforeAll(fn)`: Chạy một lần trước tất cả test trong `describe` block.
          - `afterAll(fn)`: Chạy một lần sau tất cả test trong `describe` block.
          - `beforeEach(fn)`: Chạy trước mỗi test case (`it`/`test`).
          - `afterEach(fn)`: Chạy sau mỗi test case.

    - **Unit Testing trong TypeScript với Jest:**

      - **Ví dụ: Test một hàm tiện ích đơn giản**

        ```typescript
        // src/utils/math.ts
        export function sum(a: number, b: number): number {
          return a + b;
        }

        export function multiply(a: number, b: number): number {
          if (typeof a !== "number" || typeof b !== "number") {
            throw new Error("Inputs must be numbers");
          }
          return a * b;
        }

        // tests/utils/math.test.ts
        import { sum, multiply } from "../../src/utils/math"; // Đường dẫn tới file cần test

        describe("Math Utils", () => {
          describe("sum function", () => {
            it("should return the sum of two positive numbers", () => {
              expect(sum(2, 3)).toBe(5);
            });

            it("should return the sum with a negative number", () => {
              expect(sum(5, -2)).toBe(3);
            });

            it("should return zero when summing zero and zero", () => {
              expect(sum(0, 0)).toBe(0);
            });
          });

          describe("multiply function", () => {
            it("should return the product of two numbers", () => {
              expect(multiply(3, 4)).toBe(12);
            });

            it("should throw an error if inputs are not numbers", () => {
              // expect(multiply('a' as any, 3)).toThrow(); // Kiểm tra có throw lỗi bất kỳ
              expect(() => multiply("a" as any, 3)).toThrow(
                "Inputs must be numbers"
              ); // Kiểm tra message lỗi
            });
          });
        });
        ```

      - **Mocking và Spying:**

        - **Mocking:** Thay thế một dependency (module, function, class) bằng một "phiên bản giả" (mock implementation) để cô lập unit đang test và kiểm soát hành vi của dependency đó.
        - **Spying:** Theo dõi (spy on) các lời gọi đến một function/method mà không thay đổi hành vi gốc của nó (trừ khi bạn muốn). Hữu ích để kiểm tra xem một hàm có được gọi với đúng tham số, bao nhiêu lần, etc.
        - **Jest Mock Functions (`jest.fn()`, `jest.spyOn()`):**
          - `jest.fn()`: Tạo một mock function.
          - `jest.spyOn(object, methodName)`: Tạo một spy trên một method của object.
        - **Ví dụ: Test một Service sử dụng Repository (mock Repository)**

          ````typescript
          // src/api/users/user.entity.ts (Giả sử đã có)
          // src/api/users/user.service.ts (Giống ví dụ ở Phần 9)
          import { Repository } from 'typeorm';
          import { User } from './user.entity';
          import AppDataSource from '../../config/database'; // Chỉ để ví dụ, thực tế nên inject DataSource

              export class UserService {
                private userRepository: Repository<User>;
                constructor() {
                  // Trong test, chúng ta sẽ mock AppDataSource.getRepository
                  // Hoặc tốt hơn là inject userRepository qua constructor
                  this.userRepository = AppDataSource.getRepository(User);
                }
                async findUserById(id: string): Promise<User | null> {
                  console.log('UserService: findUserById called with id:', id); // Để thấy spy hoạt động
                  return this.userRepository.findOneBy({ id });
                }
                async createUser(userData: Partial<User>): Promise<User> {
                  const newUser = this.userRepository.create(userData);
                  return this.userRepository.save(newUser);
                }
              }

              // tests/users/user.service.test.ts
              import { UserService } from '../../src/api/users/user.service';
              import { User } from '../../src/api/users/user.entity';
              import { Repository } from 'typeorm';
              // import AppDataSource from '../../src/config/database'; // Không import trực tiếp ở đây

              // Mock TypeORM Repository
              // Cách 1: Mock toàn bộ module 'typeorm' (phức tạp hơn nếu chỉ cần mock một phần)
              // jest.mock('typeorm', () => {
              //   const originalModule = jest.requireActual('typeorm');
              //   return {
              //     ...originalModule,
              //     getRepository: jest.fn(), // Hoặc mock cụ thể hơn
              //   };
              // });

              // Cách 2: Mock cụ thể getRepository hoặc inject mock repository
              // Đây là cách đơn giản hơn nếu UserService nhận repository qua constructor
              // Giả sử UserService được refactor để nhận repository:
              // export class UserService {
              //   constructor(private userRepository: Repository<User>) {}
              //   // ...
              // }

              // Giả sử chúng ta mock AppDataSource.getRepository cho ví dụ này
              // (Đây không phải là cách tốt nhất cho DI, nhưng để minh họa mock)
              jest.mock('../../src/config/database', () => ({
                getRepository: jest.fn().mockReturnValue({ // Mock implementation của getRepository
                  findOneBy: jest.fn(),
                  create: jest.fn(),
                  save: jest.fn(),
                }),
                // Cần mock cả initialize nếu service cố gắng gọi nó, hoặc đảm bảo nó không được gọi trong unit test
                initialize: jest.fn().mockResolvedValue(undefined),
                isInitialized: true // Giả sử đã initialize
              }));
              // Phải import AppDataSource SAU KHI jest.mock
              const MockAppDataSource = require('../../src/config/database');


              describe('UserService', () => {
                let userService: UserService;
                let mockUserRepository: jest.Mocked<Repository<User>>; // Type cho mock repository

                beforeEach(() => {
                  // Reset mocks trước mỗi test
                  // (AppDataSource.getRepository as jest.Mock).mockClear();
                  // (AppDataSource.getRepository().findOneBy as jest.Mock).mockClear();

                  // Lấy mock repository (đã được mock ở trên)
                  mockUserRepository = MockAppDataSource.getRepository() as jest.Mocked<Repository<User>>;

                  // Khởi tạo UserService, nó sẽ dùng mock repository
                  userService = new UserService();
                });

                describe('findUserById', () => {
                  it('should call userRepository.findOneBy with the correct id and return the user', async () => {
                    const mockUser: User = { id: '1', username: 'testuser', email: 'test@example.com' } as User;
                    mockUserRepository.findOneBy.mockResolvedValue(mockUser); // Setup mock return value

                    const userId = '1';
                    const result = await userService.findUserById(userId);

                    expect(mockUserRepository.findOneBy).toHaveBeenCalledTimes(1);
                    expect(mockUserRepository.findOneBy).toHaveBeenCalledWith({ id: userId });
                    expect(result).toEqual(mockUser);
                  });

                  it('should return null if user is not found', async () => {
                    mockUserRepository.findOneBy.mockResolvedValue(null);

                    const result = await userService.findUserById('non-existent-id');

                    expect(result).toBeNull();
                  });
                });

                describe('createUser', () => {
                  it('should call userRepository.create and userRepository.save with user data', async () => {
                      const userData = { username: 'newuser', email: 'new@example.com' };
                      const createdUserInstance = { ...userData, id: 'temp-id' } as User; // Giả lập kết quả của create
                      const savedUser = { ...createdUserInstance, id: 'final-id' } as User; // Giả lập kết quả của save

                      mockUserRepository.create.mockReturnValue(createdUserInstance);
                      mockUserRepository.save.mockResolvedValue(savedUser);

                      const result = await userService.createUser(userData);

                      expect(mockUserRepository.create).toHaveBeenCalledWith(userData);
                      expect(mockUserRepository.save).toHaveBeenCalledWith(createdUserInstance);
                      expect(result).toEqual(savedUser);
                  });
                });
              });
              ```

          **Lưu ý về Dependency Injection (DI):** Cách tốt nhất để làm cho code dễ test là sử dụng DI. Thay vì `UserService` tự tạo `UserRepository` (qua `AppDataSource.getRepository(User)`), `UserRepository` nên được truyền vào `UserService` qua constructor. Điều này giúp việc inject mock repository trong test dễ dàng hơn nhiều. (Chúng ta sẽ nói về DI kỹ hơn khi đến NestJS hoặc các pattern DI).
          ````

    - **Integration Testing:**

      - **Mục tiêu:** Test sự tương tác giữa UserService và một CSDL thực (test database).
      - **Thiết lập:**
        - Cần một CSDL riêng cho testing (ví dụ: `test_database`).
        - `beforeAll` có thể dùng để khởi tạo kết nối DB, chạy migrations.
        - `afterAll` để đóng kết nối DB.
        - `beforeEach` hoặc `afterEach` để dọn dẹp dữ liệu test (ví dụ: xóa tất cả bản ghi trong các bảng liên quan).
      - **Ví dụ (ý tưởng, cần setup DB test):**

        ```typescript
        // tests/integration/user.service.integration.test.ts
        // import AppDataSource from '../../src/config/database'; // DataSource thật
        // import { UserService } from '../../src/api/users/user.service';
        // import { User } from '../../src/api/users/user.entity';

        // describe('UserService - Integration', () => {
        //   let userService: UserService;

        //   beforeAll(async () => {
        //     // Cấu hình AppDataSource để trỏ đến TEST DATABASE
        //     // Ví dụ: process.env.DB_NAME = 'test_db_name';
        //     // await AppDataSource.initialize(); // Kết nối DB test
        //     // await AppDataSource.runMigrations(); // Chạy migrations trên DB test
        //     userService = new UserService(); // Giả sử nó dùng AppDataSource thật
        //   });

        //   afterAll(async () => {
        //     // await AppDataSource.destroy(); // Đóng kết nối
        //   });

        //   beforeEach(async () => {
        //     // Dọn dẹp dữ liệu test
        //     // const userRepository = AppDataSource.getRepository(User);
        //     // await userRepository.clear(); // Xóa tất cả user
        //   });

        //   it('should create a user and then find it by id', async () => {
        //     const userData = { username: 'integ_user', email: 'integ@example.com', password: 'password123' };
        //     const createdUser = await userService.createUser(userData); // Sử dụng service thật

        //     expect(createdUser).toBeDefined();
        //     expect(createdUser.username).toBe(userData.username);

        //     const foundUser = await userService.findUserById(createdUser.id);
        //     expect(foundUser).toBeDefined();
        //     expect(foundUser!.id).toBe(createdUser.id);
        //   });
        // });
        ```

      - Thư viện như `testcontainers` có thể giúp tạo Docker container cho DB khi chạy test, rất tiện lợi.

    - **E2E Testing cho API (với Supertest):**

      - **Supertest:** Một thư viện giúp test các HTTP server một cách dễ dàng. Nó "gói" server Express của bạn và cho phép gửi request đến nó mà không cần thực sự lắng nghe trên một port mạng (nhanh hơn).
      - **Cài đặt:** `npm install --save-dev supertest @types/supertest`
      - **Ví dụ: Test API User**

        ```typescript
        // tests/e2e/user.api.e2e.test.ts
        import request from "supertest"; // Supertest
        import express, { Express } from "express";
        // import AppDataSource from '../../src/config/database'; // DataSource thật
        // import mainApiRouter from '../../src/api'; // Main router của bạn
        // import { globalErrorHandler } from '../../src/middlewares/errorHandler.middleware';
        // import { User } from '../../src/api/users/user.entity';

        // let app: Express;

        // beforeAll(async () => {
        //   // Cấu hình AppDataSource cho TEST DATABASE
        //   // await AppDataSource.initialize();
        //   // await AppDataSource.runMigrations(); // Or clear data

        //   app = express();
        //   app.use(express.json());
        //   app.use('/api/v1', mainApiRouter); // Gắn router thật
        //   app.use(globalErrorHandler); // Gắn error handler thật
        // });

        // afterAll(async () => {
        //   // await AppDataSource.destroy();
        // });

        // beforeEach(async () => {
        //   // Dọn dẹp DB nếu cần
        //   // const userRepository = AppDataSource.getRepository(User);
        //   // await userRepository.delete({}); // Xóa tất cả users
        // });

        describe("User API - E2E", () => {
          // Mock app cho ví dụ đơn giản không cần DB thật
          let mockApp: Express;
          beforeAll(() => {
            mockApp = express();
            mockApp.use(express.json());
            // Mock route đơn giản
            mockApp.post("/api/v1/users", (req, res) => {
              if (!req.body.username || !req.body.email) {
                return res
                  .status(400)
                  .json({ message: "Username and email are required" });
              }
              res.status(201).json({ id: "mock-id", ...req.body });
            });
            mockApp.get("/api/v1/users/:id", (req, res) => {
              if (req.params.id === "mock-id") {
                return res.status(200).json({
                  id: "mock-id",
                  username: "mockuser",
                  email: "mock@example.com",
                });
              }
              res.status(404).json({ message: "User not found" });
            });
          });

          describe("POST /api/v1/users", () => {
            it("should create a new user and return 201", async () => {
              const userData = {
                username: "e2euser",
                email: "e2e@example.com",
                password: "password",
              };
              const response = await request(mockApp) // Sử dụng app thật (hoặc mockApp)
                .post("/api/v1/users")
                .send(userData)
                .expect("Content-Type", /json/) // Kiểm tra header
                .expect(201); // Kiểm tra status code

              expect(response.body).toBeInstanceOf(Object);
              expect(response.body.id).toBeDefined();
              expect(response.body.username).toBe(userData.username);
            });

            it("should return 400 if required fields are missing", async () => {
              await request(mockApp)
                .post("/api/v1/users")
                .send({ username: "onlyuser" })
                .expect(400);
            });
          });

          describe("GET /api/v1/users/:id", () => {
            it("should return a user if found and 200", async () => {
              // Giả sử đã có user với id 'mock-id' (hoặc tạo trước trong beforeEach)
              const response = await request(mockApp)
                .get("/api/v1/users/mock-id")
                .expect(200);
              expect(response.body.id).toBe("mock-id");
            });

            it("should return 404 if user not found", async () => {
              await request(mockApp)
                .get("/api/v1/users/non-existent-id")
                .expect(404);
            });
          });
          // ... thêm các test cho PUT, DELETE, GET all, AuthN/AuthZ
        });
        ```

    - **Test Coverage:**

      - Là một metric đo lường tỷ lệ phần trăm code (dòng, nhánh, hàm) được thực thi bởi các test case.
      - Jest có thể tạo report coverage: `jest --coverage`.
      - **"TẠI SAO" hữu ích?** Giúp xác định các phần code chưa được test.
      - **Cảnh báo:** 100% coverage không đảm bảo code không có lỗi. Nó chỉ có nghĩa là tất cả các dòng code đã được chạy qua ít nhất một lần, nhưng không đảm bảo tất cả các kịch bản logic và trường hợp biên đã được kiểm tra. Chất lượng của test case quan trọng hơn là chỉ số coverage.

    - **Nguyên tắc viết Test Tốt (FIRST):**
      - **F - Fast (Nhanh):** Test nên chạy nhanh để không làm chậm quá trình phát triển. Unit test phải cực nhanh.
      - **I - Independent/Isolated (Độc lập/Cô lập):** Các test case không nên phụ thuộc lẫn nhau. Kết quả của một test không ảnh hưởng đến test khác. Mỗi test nên tự setup và teardown môi trường của nó.
      - **R - Repeatable (Lặp lại được):** Test phải cho cùng một kết quả mỗi khi chạy, bất kể môi trường (miễn là code không đổi). Tránh phụ thuộc vào yếu tố bên ngoài không kiểm soát được (ví dụ: thời gian hiện tại, dữ liệu ngẫu nhiên không seed).
      - **S - Self-validating (Tự kiểm tra):** Test phải tự quyết định pass hay fail mà không cần sự can thiệp thủ công. Assertion phải rõ ràng.
      - **T - Timely/Thorough (Kịp thời/Toàn diện):** Viết test sớm (ví dụ: TDD - Test Driven Development) và test nên bao phủ các khía cạnh quan trọng, các trường hợp biên.

5.  **Minh họa trực quan và Code mẫu (Đã tích hợp trong phần Giải thích lý thuyết)**

6.  **Best Practices và Quy ước (Conventions):**

    - **Đặt tên file test rõ ràng:** `*.test.ts` hoặc `*.spec.ts` (ví dụ: `user.service.test.ts`).
    - **Sử dụng `describe` để nhóm các test liên quan.**
    - **Mỗi `it`/`test` chỉ nên kiểm tra một khía cạnh duy nhất.**
    - **Viết mô tả test (`it` description) rõ ràng, mô tả hành vi mong đợi.** (Ví dụ: "should return null when user is not found").
    - **Sử dụng AAA pattern (Arrange, Act, Assert):**
      - **Arrange:** Chuẩn bị dữ liệu, mock dependencies.
      - **Act:** Thực thi code cần test.
      - **Assert:** Kiểm tra kết quả.
    - **Mock dependencies một cách hiệu quả để cô lập unit test.**
    - **Dọn dẹp sau test (ví dụ: reset mocks, xóa dữ liệu DB test).**
    - **Chạy test thường xuyên, đặc biệt là trước khi commit code.**
    - **Tích hợp test vào CI/CD pipeline.**
    - **Không test code của thư viện bên thứ ba.** Chỉ test cách bạn sử dụng chúng.

7.  **Anti-patterns và Lỗi thường gặp (Common Pitfalls):**

    - **Test quá nhiều thứ trong một test case:** Khó debug, khó hiểu.
    - **Test phụ thuộc vào nhau:** Test A phải chạy trước test B.
    - **Test "flaky" (không ổn định):** Lúc pass lúc fail mà không rõ lý do (thường do race conditions, phụ thuộc thời gian, hoặc môi trường không được reset đúng).
    - **Mock quá nhiều hoặc mock không đúng cách:** Làm cho test không còn phản ánh đúng thực tế.
    - **Bỏ qua test cho các trường hợp lỗi hoặc trường hợp biên.**
    - **Chỉ tập trung vào coverage mà không quan tâm chất lượng test.**
    - **Không cập nhật test khi code thay đổi.**

8.  **So sánh, Đánh giá và Lựa chọn:**

    - **Jest vs. Mocha vs. Jasmine:**
      - **Jest:** All-in-one, phổ biến, dễ dùng.
      - **Mocha:** Test runner linh hoạt, cần kết hợp với assertion library (Chai) và mocking library (Sinon).
      - **Jasmine:** Tương tự Jest về việc cung cấp nhiều thứ sẵn có, là nền tảng của Angular testing.
      - Lựa chọn: Jest thường là lựa chọn tốt cho các dự án Node.js/TypeScript mới vì sự tiện lợi và tính năng đầy đủ.

9.  **Bài tập thực hành và Gợi ý dự án nhỏ:**

    - **Câu hỏi lý thuyết:**
      1.  Kim tự tháp testing là gì và nó gợi ý điều gì về chiến lược testing?
      2.  Sự khác biệt chính giữa `jest.fn()` và `jest.spyOn()` là gì?
      3.  Tại sao việc cô lập (isolation) lại quan trọng trong unit testing?
      4.  Nêu 3 đặc điểm của một test case tốt theo nguyên tắc FIRST.
      5.  Làm thế nào bạn có thể kiểm tra một hàm ném ra một exception cụ thể trong Jest?
    - **Thử thách code (Coding challenges):**
      1.  **Viết Unit Tests cho `AuthService` (từ Phần 11):**
          - Mock `UserRepository`, `bcrypt`, `jsonwebtoken`.
          - Test hàm `registerUser` (kiểm tra `hashPassword` được gọi, `userRepository.create/save` được gọi).
          - Test hàm `loginUser` (kiểm tra `comparePassword` được gọi, `jwt.sign` được gọi với đúng payload và options).
      2.  **Viết E2E Tests cho các endpoint AuthN/AuthZ:**
          - Sử dụng Supertest để test `/auth/register`, `/auth/login`.
          - Test một endpoint được bảo vệ, kiểm tra xem có trả về 401 nếu không có token, 403 nếu token không hợp lệ, và 200 nếu token hợp lệ.
          - Test một endpoint yêu cầu vai trò cụ thể.
      3.  **Thiết lập Integration Test cho một Service:**
          - Chọn một service đã có (ví dụ: `ProductService`).
          - Cấu hình một test database (ví dụ: SQLite in-memory cho đơn giản, hoặc PostgreSQL test).
          - Viết integration test để kiểm tra các thao tác CRUD của service đó với DB thật.
    - **Gợi ý dự án nhỏ (tiếp tục API Quản lý công việc):**
      - Viết unit tests cho tất cả các service (TaskService, UserService, AuthService).
      - Viết integration tests cho sự tương tác giữa các service và TypeORM repositories.
      - Viết E2E tests cho tất cả các API endpoints của Task.
      - Cấu hình Jest để hiển thị test coverage report.

10. **Câu hỏi phỏng vấn thường gặp:**
    - "Why is testing important in software development?"
    - "Can you explain the differences between unit, integration, and E2E tests?"
    - "What testing framework have you used for Node.js/TypeScript applications? Why did you choose it?"
    - "How do you mock dependencies in Jest? What's the difference between `jest.fn()` and `jest.spyOn()`?"
    - "What is test coverage, and how do you interpret it?"
    - "Describe the Arrange-Act-Assert (AAA) pattern in testing."
    - "How would you test an API endpoint that requires authentication?"
    - "What are some challenges you've faced when writing tests, and how did you overcome them?"
    - "How do you ensure your tests are reliable and not flaky?"

---

Testing là một kỹ năng thiết yếu. Việc đầu tư thời gian vào viết test sẽ mang lại lợi ích lâu dài cho dự án của bạn. Hãy bắt đầu với unit test, sau đó mở rộng sang integration và E2E test. Đừng ngại thử nghiệm với mocking và các kỹ thuật khác. Khi bạn đã sẵn sàng, chúng ta sẽ đi vào các chủ đề về tối ưu hóa, triển khai và microservices.

Tuyệt vời! Sau khi đã xây dựng và kiểm thử ứng dụng backend, bước tiếp theo trong vòng đời phát triển là **Deployment (Triển khai)** và một số cân nhắc về **Logging (Ghi log)** và **Monitoring (Giám sát)** để đảm bảo ứng dụng hoạt động ổn định trong môi trường production. Chúng ta cũng sẽ chạm đến khái niệm **DevOps cơ bản** liên quan đến quy trình này.

---

### **PHẦN 13: Logging, Monitoring, Deployment và DevOps Cơ bản cho Backend TypeScript**

1.  **Tên phần học:** Logging, Monitoring, Deployment và DevOps Cơ bản cho Backend TypeScript.

2.  **Mục tiêu học phần:**

    - Hiểu tầm quan trọng của **Logging** trong ứng dụng backend.
      - Biết các cấp độ log (log levels: DEBUG, INFO, WARN, ERROR, FATAL).
      - Sử dụng thư viện logging phổ biến (ví dụ: **Winston**, **Pino**) để ghi log có cấu trúc (structured logging).
      - Định dạng log và các nơi lưu trữ log (console, file, log management systems).
    - Hiểu khái niệm **Monitoring** và các loại metrics quan trọng cần theo dõi.
      - Application Performance Monitoring (APM).
      - Health checks.
    - Nắm vững các bước cơ bản để **Deployment** một ứng dụng Node.js/TypeScript lên một nền tảng cloud (ví dụ: Heroku, AWS EC2/Elastic Beanstalk, Google Cloud Run - tập trung vào khái niệm).
      - Build ứng dụng cho production.
      - Quản lý biến môi trường (environment variables).
      - Sử dụng **Docker** để đóng gói ứng dụng (khái niệm cơ bản và Dockerfile).
    - Làm quen với các khái niệm **DevOps cơ bản**:
      - **CI/CD (Continuous Integration/Continuous Deployment)**.
      - Infrastructure as Code (IaC) (giới thiệu sơ lược).
    - Hiểu cách kết hợp các yếu tố trên để vận hành ứng dụng hiệu quả.
    - Trả lời được:
      - Tại sao logging lại cần thiết và các cấp độ log có ý nghĩa gì?
      - Structured logging là gì và lợi ích của nó?
      - Monitoring giúp giải quyết vấn đề gì? Một số metrics quan trọng cần theo dõi là gì?
      - Các bước chính để deploy một ứng dụng Node.js lên cloud là gì?
      - Docker là gì và tại sao nó hữu ích cho việc deployment?
      - CI/CD là gì và lợi ích của nó trong quy trình phát triển?

3.  **Kiến thức tiên quyết:**

    - Hoàn thành các phần trước, đặc biệt là xây dựng và test ứng dụng Express.js.
    - Kiến thức cơ bản về command line.
    - Hiểu biết sơ lược về các dịch vụ cloud là một lợi thế.

4.  **Giải thích lý thuyết chuyên sâu:**

    - **Logging (Ghi Log):**

      - **Là gì?** Là quá trình ghi lại các sự kiện, thông tin hoạt động, lỗi xảy ra trong quá trình ứng dụng chạy.
      - **"TẠI SAO" Logging quan trọng?**
        1.  **Debugging và Troubleshooting:** Log là công cụ hàng đầu để chẩn đoán lỗi và vấn đề trong môi trường production (nơi không thể debug trực tiếp).
        2.  **Monitoring và Alerting:** Log có thể được phân tích để theo dõi tình trạng hệ thống và kích hoạt cảnh báo khi có sự cố.
        3.  **Auditing và Security:** Ghi lại các hành động quan trọng của người dùng hoặc hệ thống để phục vụ mục đích kiểm toán hoặc điều tra bảo mật.
        4.  **Performance Analysis:** Log có thể chứa thông tin về thời gian xử lý request, giúp xác định các điểm nghẽn.
        5.  **Understanding System Behavior:** Giúp hiểu rõ hơn về cách ứng dụng hoạt động trong thực tế.
      - **Log Levels (Cấp độ Log):** Thứ tự từ ít nghiêm trọng đến nghiêm trọng nhất (có thể khác nhau tùy thư viện/quy ước).
        - **`TRACE` (hoặc `VERBOSE`, `SILLY`):** Thông tin rất chi tiết, thường chỉ dùng cho debugging sâu.
        - **`DEBUG`:** Thông tin chi tiết hữu ích cho việc gỡ lỗi trong quá trình phát triển.
        - **`INFO`:** Thông tin chung về các sự kiện quan trọng, trạng thái hoạt động bình thường của ứng dụng (ví dụ: server started, user logged in, request processed successfully).
        - **`WARN` (Warning):** Các sự kiện không mong muốn nhưng chưa phải là lỗi nghiêm trọng, ứng dụng vẫn có thể tiếp tục hoạt động (ví dụ: một API phụ thuộc trả về lỗi tạm thời, sử dụng giá trị mặc định).
        - **`ERROR`:** Lỗi xảy ra trong quá trình xử lý một request hoặc một tác vụ cụ thể, nhưng ứng dụng tổng thể vẫn có thể hoạt động (ví dụ: không thể kết nối CSDL, lỗi validation không lường trước).
        - **`FATAL` (hoặc `CRITICAL`):** Lỗi nghiêm trọng khiến ứng dụng không thể tiếp tục hoạt động và cần phải dừng lại (ví dụ: không thể đọc file cấu hình quan trọng khi khởi động).
      - **Structured Logging (Ghi Log có cấu trúc):**
        - Thay vì ghi log dưới dạng chuỗi text tự do, structured logging ghi log dưới dạng dữ liệu có cấu trúc (thường là JSON).
        - **"TẠI SAO" Structured Logging?**
          - **Dễ dàng phân tích và truy vấn:** Các hệ thống quản lý log (Log Management Systems) có thể dễ dàng parse, index, và tìm kiếm log JSON.
          - **Thống nhất:** Giúp log từ các service khác nhau có cùng định dạng.
          - **Thêm ngữ cảnh:** Dễ dàng đính kèm các metadata (request ID, user ID, session ID, etc.) vào mỗi dòng log.
        - **Ví dụ (JSON log):**
          ```json
          {"level": "info", "timestamp": "2023-04-01T10:30:15.123Z", "message": "User logged in", "userId": "user-123", "requestId": "req-abc"}
          {"level": "error", "timestamp": "2023-04-01T10:35:02.456Z", "message": "Failed to process payment", "error": {"code": "PAYMENT_FAILED", "details": "Insufficient funds"}, "orderId": "order-789", "userId": "user-456"}
          ```
      - **Thư viện Logging cho Node.js/TypeScript:**

        - **Winston:** Rất phổ biến, linh hoạt, hỗ trợ nhiều "transports" (nơi ghi log: console, file, HTTP, database, etc.) và formatting.
        - **Pino:** Tập trung vào hiệu năng cao, structured logging (JSON) là mặc định.
        - **Bunyan:** Một thư viện structured logging khác.
        - **Ví dụ với Winston:**

          ```bash
          npm install winston
          ```

          ```typescript
          // src/config/logger.ts
          import winston from "winston";

          const { combine, timestamp, printf, colorize, json, errors } =
            winston.format;

          // Custom format cho console (dễ đọc hơn khi dev)
          const consoleFormat = printf(
            ({ level, message, timestamp, stack }) => {
              return `${timestamp} ${level}: ${stack || message}`;
            }
          );

          const logger = winston.createLogger({
            level: process.env.LOG_LEVEL || "info", // Đọc từ biến môi trường hoặc mặc định 'info'
            format: combine(
              timestamp({ format: "YYYY-MM-DD HH:mm:ss" }),
              errors({ stack: true }), // Log stack trace cho lỗi
              json() // Format chính là JSON cho các transport khác
            ),
            // defaultMeta: { service: 'user-service' }, // Metadata mặc định cho tất cả log
            transports: [
              // Ghi ra file (cho production)
              // new winston.transports.File({ filename: 'error.log', level: 'error' }),
              // new winston.transports.File({ filename: 'combined.log' }),
            ],
            // Không ghi ra console nếu không phải development để tránh làm chậm I/O
            // Hoặc chỉ ghi lỗi ra console trong production
          });

          // Nếu không phải môi trường production, thêm transport console với format dễ đọc
          if (process.env.NODE_ENV !== "production") {
            logger.add(
              new winston.transports.Console({
                format: combine(
                  colorize(), // Thêm màu cho log level
                  timestamp({ format: "YYYY-MM-DD HH:mm:ss" }),
                  consoleFormat
                ),
              })
            );
          } else {
            // Trong production, có thể chỉ muốn console log JSON nếu cần
            // Hoặc không console log gì cả nếu log đã được chuyển đi nơi khác
            logger.add(
              new winston.transports.Console({
                format: winston.format.json(), // Console log JSON trong production
              })
            );
          }

          export default logger;

          // Sử dụng logger trong ứng dụng:
          // import logger from './config/logger';
          // logger.info('User logged in', { userId: '123', ip: '192.168.1.1' });
          // logger.error('Database connection failed', { error: new Error('Timeout') });
          // try { /* ... */ } catch (err) { logger.error('An error occurred', { error: err }); }
          ```

      - **Log Management Systems:** Splunk, ELK Stack (Elasticsearch, Logstash, Kibana), Datadog, Sentry (tập trung vào error tracking).

    - **Monitoring (Giám sát):**

      - **Là gì?** Là quá trình thu thập, xử lý, phân tích và hiển thị dữ liệu về hiệu năng và tình trạng hoạt động của hệ thống theo thời gian.
      - **"TẠI SAO" Monitoring?**
        1.  **Phát hiện sự cố sớm:** Nhận biết vấn đề trước khi người dùng bị ảnh hưởng.
        2.  **Hiểu hiệu năng hệ thống:** Xác định bottlenecks, tối ưu tài nguyên.
        3.  **Đảm bảo tính sẵn sàng (Availability) và độ tin cậy (Reliability).**
        4.  **Lập kế hoạch dung lượng (Capacity Planning).**
        5.  **Hỗ trợ ra quyết định dựa trên dữ liệu.**
      - **Các loại Metrics quan trọng cần theo dõi (The Four Golden Signals - Google SRE):**
        1.  **Latency (Độ trễ):** Thời gian để phục vụ một request. Quan trọng là theo dõi phân phối độ trễ (percentiles: p50, p90, p95, p99) chứ không chỉ trung bình.
        2.  **Traffic (Lưu lượng):** Số lượng request mà hệ thống đang xử lý (ví dụ: requests per second - RPS).
        3.  **Errors (Tỷ lệ lỗi):** Tỷ lệ request bị lỗi (ví dụ: HTTP 5xx, 4xx).
        4.  **Saturation (Độ bão hòa):** Mức độ "đầy" của hệ thống, tài nguyên nào đang bị giới hạn nhất (ví dụ: CPU utilization, memory usage, disk I/O, network bandwidth).
      - **Health Checks:**
        - Là một endpoint đặc biệt (ví dụ: `/healthz`, `/status`) mà hệ thống monitoring (hoặc load balancer) có thể gọi để kiểm tra xem ứng dụng có đang hoạt động bình thường không.
        - Phản hồi thường là `200 OK` nếu ổn, và `503 Service Unavailable` (hoặc status code khác) nếu có vấn đề.
        - Có thể kiểm tra các dependency quan trọng (ví dụ: kết nối CSDL).
        ```typescript
        // src/app.ts
        // app.get('/healthz', async (req: Request, res: Response) => {
        //   try {
        //     // Kiểm tra kết nối DB (ví dụ)
        //     await AppDataSource.query('SELECT 1'); // Query đơn giản
        //     res.status(200).json({ status: 'UP', message: 'Application is healthy' });
        //   } catch (error) {
        //     logger.error('Health check failed', { error });
        //     res.status(503).json({ status: 'DOWN', message: 'Application is unhealthy', error: (error as Error).message });
        //   }
        // });
        ```
      - **Application Performance Monitoring (APM) Tools:** Datadog, New Relic, Dynatrace, Prometheus & Grafana.
        - Cung cấp khả năng theo dõi chi tiết (distributed tracing, code-level performance, error tracking).

    - **Deployment (Triển khai):**

      - Là quá trình đưa ứng dụng từ môi trường phát triển lên môi trường production để người dùng có thể truy cập.
      - **Các bước chính:**
        1.  **Build ứng dụng cho production:**
            - Biên dịch TypeScript sang JavaScript: `npm run build` (sử dụng `tsc`).
            - Cài đặt chỉ `dependencies` (không phải `devDependencies`): `npm install --production`.
        2.  **Quản lý biến môi trường:**
            - Không hardcode các cấu hình nhạy cảm (DB credentials, API keys, JWT secrets) vào code.
            - Sử dụng biến môi trường. Các nền tảng cloud thường có cách để set biến môi trường.
            - File `.env` chỉ dùng cho local development, **không commit file `.env` chứa secret lên Git.**
        3.  **Chọn nền tảng triển khai:**
            - **PaaS (Platform as a Service):** Heroku, Google App Engine, AWS Elastic Beanstalk, Render, Vercel (cho Node.js). Đơn giản hóa việc triển khai, quản lý hạ tầng được trừu tượng hóa.
            - **IaaS (Infrastructure as a Service):** AWS EC2, Google Compute Engine, Azure VMs. Kiểm soát hoàn toàn server, nhưng cần tự quản lý nhiều hơn.
            - **Containers (CaaS - Container as a Service / FaaS - Functions as a Service):** Docker + Kubernetes, AWS ECS/EKS/Fargate, Google Kubernetes Engine (GKE)/Cloud Run, AWS Lambda.
        4.  **Chuyển code lên server/nền tảng.**
        5.  **Cài đặt dependencies trên server.**
        6.  **Chạy migrations CSDL (nếu có).**
        7.  **Khởi động ứng dụng (sử dụng process manager như PM2).**
            - **PM2:** Một process manager cho Node.js, giúp quản lý ứng dụng (khởi động lại khi crash, clustering để tận dụng multi-core CPU, logging, monitoring).
              ```bash
              npm install pm2 -g # Cài global
              pm2 start dist/server.js --name "my-app" --watch # Khởi động và theo dõi
              pm2 list # Xem danh sách process
              pm2 logs my-app # Xem log
              pm2 restart my-app
              pm2 stop my-app
              ```
      - **Docker - Đóng gói ứng dụng:**

        - **Là gì?** Docker là một nền tảng để phát triển, vận chuyển và chạy ứng dụng trong các **containers (bộ chứa)**. Container đóng gói ứng dụng và tất cả các dependencies của nó (thư viện, runtime, system tools) thành một đơn vị độc lập.
        - **"TẠI SAO" Docker?**
          1.  **Consistency (Tính nhất quán):** "Works on my machine" không còn là vấn đề. Môi trường chạy ứng dụng nhất quán từ development đến production.
          2.  **Portability (Tính khả chuyển):** Container có thể chạy trên bất kỳ máy nào có Docker Engine.
          3.  **Isolation (Tính cô lập):** Các container chạy độc lập, không ảnh hưởng lẫn nhau.
          4.  **Scalability (Khả năng mở rộng):** Dễ dàng scale số lượng container.
          5.  **Efficiency (Hiệu quả):** Nhẹ hơn máy ảo truyền thống.
          6.  **DevOps:** Phù hợp với quy trình CI/CD.
        - **Dockerfile:** Một file text chứa các chỉ dẫn để build một Docker image.

          ```dockerfile
          # Dockerfile for Node.js/TypeScript app

          # === Build Stage ===
          FROM node:18-alpine AS builder
          # Sử dụng Node 18 phiên bản Alpine (nhẹ) làm base image cho build stage

          WORKDIR /usr/src/app
          # Thiết lập thư mục làm việc trong container

          COPY package*.json ./
          # Sao chép package.json và package-lock.json

          RUN npm install --only=production --ignore-scripts
          # Cài đặt chỉ production dependencies, bỏ qua script (ví dụ postinstall) để build nhanh hơn

          COPY . .
          # Sao chép toàn bộ source code

          RUN npm run build
          # Chạy lệnh build TypeScript (tsc)

          # === Production Stage ===
          FROM node:18-alpine AS production
          # Base image cho production stage, cũng là Alpine để giữ image nhẹ

          ARG NODE_ENV=production
          ENV NODE_ENV=${NODE_ENV}
          # Set biến môi trường NODE_ENV

          WORKDIR /usr/src/app

          COPY package*.json ./
          # Sao chép lại package files

          RUN npm install --only=production --ignore-scripts
          # Cài đặt lại production dependencies (cần thiết vì các stage là độc lập về file system, trừ khi dùng multi-stage build copy)
          # Hoặc tốt hơn: COPY --from=builder /usr/src/app/node_modules ./node_modules (nếu node_modules không phụ thuộc OS/arch)

          COPY --from=builder /usr/src/app/dist ./dist
          # Sao chép thư mục 'dist' (code JS đã build) từ builder stage

          # COPY .env.example .env # Không nên copy .env vào image, dùng config của platform
          # COPY ormconfig.prod.js ./ormconfig.js # Hoặc cấu hình ORM qua env vars

          EXPOSE ${PORT:-3000}
          # Expose port mà ứng dụng sẽ lắng nghe (đọc từ env PORT hoặc mặc định 3000)

          # USER node # Chạy ứng dụng với user không phải root để tăng bảo mật (tùy chọn)

          CMD ["node", "dist/server.js"]
          # Lệnh để khởi chạy ứng dụng khi container bắt đầu
          # Hoặc dùng PM2: CMD ["pm2-runtime", "dist/server.js"]
          ```

        - **Build Docker Image:** `docker build -t my-ts-app .`
        - **Run Docker Container:** `docker run -p 3000:3000 -e "DB_HOST=..." -e "JWT_SECRET=..." my-ts-app`

    - **DevOps Cơ bản:**
      - **DevOps:** Một tập hợp các thực hành kết hợp phát triển phần mềm (Dev) và vận hành công nghệ thông tin (Ops) nhằm rút ngắn vòng đời phát triển hệ thống và cung cấp việc phân phối liên tục với chất lượng phần mềm cao.
      - **CI/CD (Continuous Integration/Continuous Deployment or Delivery):**
        - **Continuous Integration (CI - Tích hợp Liên tục):** Thực hành tự động build và test code mỗi khi có thay đổi được commit lên version control (ví dụ: Git).
          - **Lợi ích:** Phát hiện lỗi tích hợp sớm, đảm bảo code luôn ở trạng thái có thể build được.
          - **Công cụ:** GitHub Actions, GitLab CI/CD, Jenkins, CircleCI.
          - **Quy trình CI cơ bản:**
            1.  Developer push code lên Git.
            2.  CI server tự động trigger.
            3.  Checkout code.
            4.  Cài đặt dependencies.
            5.  Chạy linters, code style checks.
            6.  Chạy unit tests, integration tests.
            7.  Build artifact (ví dụ: Docker image).
            8.  (Tùy chọn) Push artifact lên registry (ví dụ: Docker Hub, AWS ECR).
        - **Continuous Deployment (CD - Triển khai Liên tục):** Tự động deploy artifact đã qua CI lên môi trường production (hoặc staging).
        - **Continuous Delivery (CD - Phân phối Liên tục):** Tự động deploy artifact lên môi trường staging, nhưng cần sự phê duyệt thủ công để deploy lên production.
          - **Lợi ích:** Giảm thiểu rủi ro khi deploy, tăng tốc độ release, phản hồi nhanh hơn.
      - **Infrastructure as Code (IaC):**
        - Quản lý và cung cấp hạ tầng (servers, networks, databases, load balancers) thông qua code và các công cụ tự động hóa, thay vì cấu hình thủ công.
        - **Công cụ:** Terraform, AWS CloudFormation, Ansible, Pulumi.
        - **Lợi ích:** Tính nhất quán, lặp lại được, có phiên bản, tự động hóa.

5.  **Minh họa trực quan và Code mẫu (Đã tích hợp trong phần Giải thích lý thuyết)**

    - **Sơ đồ CI/CD cơ bản:**

      ```mermaid
      graph LR
          Dev["Developer"] -- "1. Push Code" --> Git["Git Repository (GitHub, GitLab)"];
          Git -- "2. Webhook Trigger" --> CI_Server["CI Server (GitHub Actions, Jenkins)"];
          subgraph CI_Pipeline ["CI Pipeline"]
              direction LR
              CI_Server --> P1["Checkout Code"];
              P1 --> P2["Install Dependencies"];
              P2 --> P3["Run Linters & Tests"];
              P3 --> P4["Build Application (e.g., Docker Image)"];
              P4 --> P5["Push Artifact (e.g., to Docker Registry)"];
          end
          P3 -- "Tests Fail" --> Notify_Fail["Notify Developer (Fail)"];
          P5 -- "Build Success" --> CD_Server["CD Server / Process"];

          subgraph CD_Pipeline ["CD Pipeline (Optional Automatic)"]
              direction LR
              CD_Server --> D1["Deploy to Staging"];
              D1 -- "Run E2E/Smoke Tests" --> D2["Approval (Manual/Auto)"];
              D2 -- "Approved" --> D3["Deploy to Production"];
          end
          D3 -- "Deployment Success" --> Notify_Success["Notify Team (Success)"];
      ```

6.  **Best Practices và Quy ước (Conventions):**

    - **Sử dụng structured logging (JSON) trong production.**
    - **Cấu hình log levels khác nhau cho các môi trường (dev, staging, prod).**
    - **Không log thông tin nhạy cảm (mật khẩu, API keys, PII) dưới dạng plain text.**
    - **Triển khai health check endpoint.**
    - **Sử dụng Docker để đóng gói ứng dụng cho deployment.**
    - **Tự động hóa quy trình build và deployment với CI/CD.**
    - **Quản lý cấu hình bằng biến môi trường.**
    - **Sử dụng process manager (PM2) cho ứng dụng Node.js trong production (nếu không dùng container orchestrator).**
    - **Theo dõi các metrics quan trọng và thiết lập cảnh báo.**
    - **Thực hiện "graceful shutdown" cho ứng dụng (đóng kết nối DB, chờ request hoàn thành trước khi thoát).**
      ```typescript
      // src/server.ts (ví dụ graceful shutdown)
      // const server = app.listen(...);
      // process.on('SIGTERM', () => { // Hoặc SIGINT
      //   logger.info('SIGTERM signal received: closing HTTP server');
      //   server.close(async () => {
      //     logger.info('HTTP server closed');
      //     if (AppDataSource.isInitialized) {
      //       await AppDataSource.destroy();
      //       logger.info('Database connection closed');
      //     }
      //     process.exit(0);
      //   });
      // });
      ```

7.  **Anti-patterns và Lỗi thường gặp (Common Pitfalls):**

    - **Không có logging hoặc logging không đủ thông tin.**
    - **Logging quá nhiều thông tin không cần thiết (đặc biệt ở level INFO) làm tốn tài nguyên.**
    - **Hardcode cấu hình trong code.**
    - **Deploy thủ công, dễ xảy ra lỗi.**
    - **Không có monitoring, không biết khi nào hệ thống có vấn đề.**
    - **Dockerfile không được tối ưu (image quá lớn, build chậm).**
    - **Không xử lý graceful shutdown, làm mất request hoặc dữ liệu.**
    - **Biến môi trường không được quản lý an toàn.**

8.  **So sánh, Đánh giá và Lựa chọn:**

    - **Các nền tảng PaaS:**
      - **Heroku:** Rất dễ sử dụng, phù hợp cho startup và dự án nhỏ/vừa.
      - **Render:** Tương tự Heroku, hỗ trợ nhiều loại service.
      - **AWS Elastic Beanstalk / Google App Engine:** Mạnh mẽ hơn, nhiều tùy chọn cấu hình, tích hợp tốt với hệ sinh thái cloud tương ứng.
    - **Container Orchestration (cho các hệ thống lớn, phức tạp):**
      - **Kubernetes (K8s):** Tiêu chuẩn ngành, rất mạnh mẽ nhưng phức tạp.
      - **AWS ECS/Fargate, Google Cloud Run:** Dịch vụ quản lý container đơn giản hơn K8s.
    - Lựa chọn phụ thuộc vào quy mô, độ phức tạp, ngân sách, và kinh nghiệm của đội ngũ.

9.  **Bài tập thực hành và Gợi ý dự án nhỏ:**

    - **Câu hỏi lý thuyết:**
      1.  Sự khác biệt giữa log level `INFO` và `DEBUG` là gì? Khi nào bạn dùng mỗi loại?
      2.  Tại sao structured logging lại được ưa chuộng hơn logging dạng text tự do?
      3.  Health check endpoint có mục đích gì?
      4.  Nêu 3 lợi ích của việc sử dụng Docker để deploy ứng dụng.
      5.  CI (Continuous Integration) là gì và nó giúp giải quyết vấn đề gì?
    - **Thử thách code (Coding challenges):**
      1.  **Tích hợp Winston (hoặc Pino) vào dự án Express:**
          - Cấu hình logger với các transport khác nhau (console, file).
          - Sử dụng logger trong các controller và service để ghi log các sự kiện quan trọng và lỗi.
          - Tạo một middleware để log mỗi request (method, URL, status code, duration).
      2.  **Tạo một Dockerfile cơ bản cho ứng dụng:**
          - Viết Dockerfile để build và chạy ứng dụng Express/TypeORM của bạn.
          - Build image và chạy container trên máy local.
      3.  **(Tùy chọn nâng cao) Setup một CI pipeline đơn giản với GitHub Actions:**
          - Tạo một workflow YAML để tự động chạy `npm install`, `npm run build`, `npm test` mỗi khi push code lên GitHub.
    - **Gợi ý dự án nhỏ (tiếp tục API Quản lý công việc):**
      - Tích hợp logger vào tất cả các phần của ứng dụng.
      - Thêm một health check endpoint kiểm tra kết nối CSDL.
      - Viết Dockerfile cho ứng dụng.
      - (Thử thách) Deploy ứng dụng lên một nền tảng PaaS miễn phí (Heroku, Render).

10. **Câu hỏi phỏng vấn thường gặp:**
    - "Why is logging important in a production application? What kind of information would you log?"
    - "What are log levels, and how do they help?"
    - "What is structured logging, and what are its benefits?"
    - "What is application monitoring? What key metrics would you track for a backend API?"
    - "Can you describe the basic steps to deploy a Node.js application?"
    - "What is Docker, and how does it simplify deployment?"
    - "What are the main components of a Dockerfile for a Node.js application?"
    - "Explain CI/CD. What are the benefits of implementing it?"
    - "How do you manage environment variables in different deployment environments?"
    - "What is PM2 and what problem does it solve?"

---

Phần này bao gồm nhiều khái niệm quan trọng liên quan đến việc vận hành ứng dụng. Mặc dù bạn là developer, việc hiểu biết về deployment, logging, và monitoring sẽ giúp bạn xây dựng các ứng dụng tốt hơn và phối hợp hiệu quả hơn với đội ngũ Ops (nếu có). Khi bạn đã sẵn sàng, chúng ta sẽ bắt đầu đi sâu vào kiến trúc Microservices.

Tuyệt vời! Sau khi đã có một nền tảng vững chắc về xây dựng, kiểm thử, và triển khai một ứng dụng backend monolith (đơn khối), giờ là lúc chúng ta tiến vào một chủ đề phức tạp và thú vị hơn, rất quan trọng trong các hệ thống lớn hiện đại: **Kiến trúc Microservices**.

---

### **PHẦN 14: Giới thiệu về Kiến trúc Microservices và Các Nguyên tắc Thiết kế**

1.  **Tên phần học:** Giới thiệu về Kiến trúc Microservices và Các Nguyên tắc Thiết kế.

2.  **Mục tiêu học phần:**

    - Hiểu rõ **Kiến trúc Microservices là gì**, phân biệt với kiến trúc Monolith.
    - Nắm vững các **lợi ích** chính của việc áp dụng Microservices (ví dụ: khả năng mở rộng độc lập, lựa chọn công nghệ linh hoạt, phát triển song song).
    - Nhận biết các **thách thức và nhược điểm** của Microservices (ví dụ: độ phức tạp vận hành, giao tiếp liên service, quản lý dữ liệu phân tán).
    - Hiểu các **đặc điểm cốt lõi** của một kiến trúc Microservices tốt (ví dụ: single responsibility, decentralized governance, design for failure).
    - Làm quen với các **nguyên tắc thiết kế** quan trọng cho Microservices:
      - Single Responsibility Principle (áp dụng cho service).
      - Bounded Context (từ Domain-Driven Design).
      - Decentralized Data Management (mỗi service sở hữu dữ liệu riêng).
      - Design for Failure (thiết kế cho khả năng chịu lỗi).
      - Evolutionary Design (thiết kế có khả năng tiến hóa).
    - Hiểu khi nào nên (và không nên) cân nhắc sử dụng kiến trúc Microservices.
    - Phân biệt Microservices với SOA (Service-Oriented Architecture) ở mức độ cơ bản.
    - Trả lời được:
      - Microservices là gì? So sánh ưu nhược điểm với Monolith.
      - Một số đặc điểm chính của một microservice tốt là gì?
      - Bounded Context là gì và nó liên quan đến Microservices như thế nào?
      - Tại sao "mỗi service sở hữu dữ liệu riêng" lại quan trọng trong Microservices?
      - "Design for Failure" có ý nghĩa gì trong bối cảnh Microservices?
      - Khi nào thì một dự án nên chuyển từ Monolith sang Microservices?

3.  **Kiến thức tiên quyết:**

    - Hoàn thành các phần trước, đặc biệt là kiến thức về xây dựng API, làm việc với CSDL, và các khái niệm DevOps cơ bản.
    - Hiểu biết về các nguyên tắc SOLID.
    - Kinh nghiệm làm việc với các hệ thống phần mềm (dù là monolith) sẽ giúp hình dung rõ hơn.

4.  **Giải thích lý thuyết chuyên sâu:**

    - **Kiến trúc Monolith (Đơn khối):**

      - **Là gì?** Một ứng dụng được xây dựng như một khối duy nhất, không thể chia tách. Tất cả các module, component, chức năng được đóng gói và triển khai chung trong một đơn vị (ví dụ: một file WAR, một thư mục Node.js lớn).
      - **Ví dụ:** Một ứng dụng web thương mại điện tử có module quản lý user, module sản phẩm, module đơn hàng, module thanh toán... tất cả nằm trong cùng một codebase và chạy trên cùng một process (hoặc nhiều instance của cùng một process).
      - **Ưu điểm:**
        1.  **Đơn giản để phát triển ban đầu:** Tất cả code ở một nơi, dễ dàng chia sẻ code, dễ debug.
        2.  **Đơn giản để triển khai ban đầu:** Chỉ cần deploy một đơn vị.
        3.  **Hiệu năng (cho các tương tác nội bộ):** Giao tiếp giữa các module trong monolith thường nhanh (gọi hàm trực tiếp).
      - **Nhược điểm (khi ứng dụng lớn và phức tạp dần):**
        1.  **Khó mở rộng (Scalability):** Phải scale toàn bộ ứng dụng, ngay cả khi chỉ một phần nhỏ bị quá tải. Lãng phí tài nguyên.
        2.  **Khó bảo trì và phát triển:** Codebase lớn trở nên phức tạp, khó hiểu, thay đổi một phần có thể ảnh hưởng đến các phần khác (tight coupling). Thời gian build và test tăng lên.
        3.  **Ràng buộc công nghệ:** Khó áp dụng các công nghệ mới cho từng phần riêng lẻ. Toàn bộ ứng dụng phải dùng chung một stack công nghệ.
        4.  **Tính sẵn sàng (Availability) thấp:** Lỗi ở một module có thể làm sập toàn bộ ứng dụng.
        5.  **Khó khăn cho các team lớn:** Nhiều người cùng làm việc trên một codebase lớn dễ gây xung đột.

    - **Kiến trúc Microservices:**

      - **Là gì?** Một phương pháp phát triển phần mềm mà ứng dụng được cấu thành từ một tập hợp các **service nhỏ, độc lập, có thể triển khai riêng lẻ**, mỗi service chạy trong process riêng của nó và giao tiếp với nhau thông qua các cơ chế nhẹ (thường là HTTP/REST API, gRPC, hoặc message queues).
      - Mỗi microservice được thiết kế để thực hiện một **chức năng nghiệp vụ (business capability)** cụ thể và có thể được phát triển, triển khai, và scale độc lập với các service khác.
      - **"TẠI SAO" Microservices?** Để giải quyết các nhược điểm của monolith khi hệ thống trở nên lớn mạnh và phức tạp.
      - **Ví dụ (ứng dụng thương mại điện tử với Microservices):**
        - UserService (quản lý user, authentication)
        - ProductService (quản lý thông tin sản phẩm, tồn kho)
        - OrderService (xử lý đặt hàng, theo dõi đơn hàng)
        - PaymentService (xử lý thanh toán)
        - NotificationService (gửi email, SMS)
          Mỗi service này có thể được viết bằng ngôn ngữ/framework khác nhau, có CSDL riêng, và được deploy riêng.

    - **Lợi ích của Microservices:**

      1.  **Khả năng mở rộng độc lập (Independent Scalability):** Có thể scale từng service riêng lẻ dựa trên nhu cầu của service đó. Ví dụ, ProductService có thể cần nhiều instance hơn OrderService trong mùa cao điểm.
      2.  **Lựa chọn công nghệ linh hoạt (Technology Diversity / Polyglot):** Mỗi service có thể chọn stack công nghệ (ngôn ngữ, framework, CSDL) phù hợp nhất cho chức năng của nó.
      3.  **Phát triển và triển khai độc lập (Independent Development & Deployment):** Các team nhỏ có thể làm việc song song trên các service khác nhau. Một service có thể được deploy mà không ảnh hưởng đến các service khác (giảm rủi ro khi release).
      4.  **Khả năng chịu lỗi tốt hơn (Improved Fault Isolation / Resilience):** Nếu một service lỗi, nó không nhất thiết làm sập toàn bộ hệ thống (nếu các service khác được thiết kế để xử lý lỗi này).
      5.  **Codebase nhỏ hơn, dễ quản lý hơn (Smaller, More Manageable Codebases):** Mỗi service có codebase riêng, dễ hiểu, dễ bảo trì hơn.
      6.  **Tổ chức team tốt hơn:** Các team có thể được tổ chức quanh các business capability (tương ứng với các microservice).
      7.  **Dễ dàng áp dụng công nghệ mới:** Có thể thử nghiệm công nghệ mới trên một service nhỏ trước khi áp dụng rộng rãi.

    - **Thách thức và Nhược điểm của Microservices:**

      1.  **Độ phức tạp vận hành (Operational Complexity / Overhead):**
          - Cần quản lý nhiều process, nhiều server/container hơn.
          - Yêu cầu các công cụ và quy trình phức tạp hơn cho deployment, monitoring, logging (ví dụ: container orchestration như Kubernetes, service discovery, distributed tracing).
      2.  **Giao tiếp liên service (Inter-service Communication):**
          - Giao tiếp qua mạng (HTTP, gRPC, message queue) chậm hơn và kém tin cậy hơn gọi hàm trực tiếp trong monolith.
          - Cần xử lý lỗi mạng, latency, retry mechanisms.
      3.  **Quản lý dữ liệu phân tán (Distributed Data Management):**
          - Mỗi service thường có CSDL riêng. Việc đảm bảo tính nhất quán dữ liệu (consistency) giữa các service trở nên phức tạp (ví dụ: two-phase commit (khó), sagas, eventual consistency).
          - Join dữ liệu giữa các service khó khăn hơn.
      4.  **Testing phức tạp hơn:** Cần cả unit test cho từng service, integration test giữa các service, và E2E test cho toàn bộ hệ thống. Mocking dependencies giữa các service có thể phức tạp.
      5.  **Yêu cầu kỹ năng DevOps mạnh mẽ:** Team cần có kinh nghiệm về tự động hóa, CI/CD, containerization, cloud.
      6.  **Distributed Tracing và Debugging:** Khó theo dõi một request đi qua nhiều service và debug lỗi. Cần các công cụ như Jaeger, Zipkin.
      7.  **Service Discovery:** Các service cần tìm thấy địa chỉ của nhau để giao tiếp.
      8.  **Configuration Management:** Quản lý cấu hình cho nhiều service.
      9.  **Latency:** Tổng độ trễ có thể tăng do nhiều hop mạng.
      10. **Chi phí ban đầu cao hơn:** Cần đầu tư vào hạ tầng và công cụ ngay từ đầu.

    - **Đặc điểm cốt lõi của một Kiến trúc Microservices tốt:**

      - **Componentization via Services (Thành phần hóa qua Dịch vụ):** Các service được thiết kế như các component có thể thay thế và nâng cấp độc lập.
      - **Organized around Business Capabilities (Tổ chức xoay quanh Năng lực Nghiệp vụ):** Mỗi service tập trung vào một lĩnh vực nghiệp vụ cụ thể (ví dụ: "quản lý đơn hàng" thay vì "lớp truy cập dữ liệu đơn hàng").
      - **Products not Projects (Sản phẩm chứ không phải Dự án):** Team sở hữu service từ lúc thiết kế, phát triển, triển khai, vận hành và bảo trì ("you build it, you run it").
      - **Smart Endpoints and Dumb Pipes (Điểm cuối Thông minh và Đường ống Ngớ ngẩn):** Các service tự chứa logic nghiệp vụ, còn cơ chế giao tiếp giữa chúng (HTTP, message queue) nên đơn giản và không chứa logic phức tạp. Tránh Enterprise Service Bus (ESB) phức tạp.
      - **Decentralized Governance (Quản trị Phi tập trung):** Các team có quyền tự chủ trong việc lựa chọn công nghệ và quy trình phát triển cho service của mình (trong một số giới hạn chung).
      - **Decentralized Data Management (Quản lý Dữ liệu Phi tập trung):** Mỗi service nên sở hữu và quản lý dữ liệu riêng của nó. Các service khác không được truy cập trực tiếp vào CSDL của service này mà phải qua API của nó.
      - **Infrastructure Automation (Tự động hóa Hạ tầng):** CI/CD, automated testing, automated deployment là rất quan trọng.
      - **Design for Failure (Thiết kế cho Khả năng Chịu lỗi):** Hệ thống phải được thiết kế để xử lý lỗi của các service phụ thuộc một cách linh hoạt (ví dụ: circuit breakers, retries, fallbacks).
      - **Evolutionary Design (Thiết kế có Khả năng Tiến hóa):** Kiến trúc phải cho phép các service thay đổi và phát triển độc lập theo thời gian.

    - **Các Nguyên tắc Thiết kế quan trọng cho Microservices:**

      1.  **Single Responsibility Principle (SRP) ở cấp độ Service:**
          - Mỗi microservice chỉ nên chịu trách nhiệm cho một phần nhỏ, cụ thể của chức năng nghiệp vụ. Nó nên có một lý do duy nhất để thay đổi.
          - Giúp service nhỏ gọn, dễ hiểu, dễ bảo trì.
          - **Ví dụ:** UserService chỉ quản lý thông tin user, không làm thêm việc gửi email thông báo (nên tách ra NotificationService).
      2.  **Bounded Context (Ngữ cảnh Giới hạn - từ Domain-Driven Design - DDD):**
          - **Là gì?** Một Bounded Context định nghĩa ranh giới rõ ràng của một mô hình nghiệp vụ (domain model) cụ thể và ngôn ngữ chung (ubiquitous language) được sử dụng trong ngữ cảnh đó. Bên trong một bounded context, mỗi thuật ngữ nghiệp vụ có một ý nghĩa duy nhất, rõ ràng.
          - **Liên quan đến Microservices:** Mỗi microservice thường tương ứng với một Bounded Context (hoặc một phần của Bounded Context). Điều này giúp xác định ranh giới và trách nhiệm của từng service một cách tự nhiên dựa trên nghiệp vụ.
          - **Ví dụ:** Trong context "Sales", một "Product" có thể có giá, mô tả bán hàng. Trong context "Warehouse", một "Product" có thể có vị trí lưu trữ, số lượng tồn. Đây là hai "Product" khác nhau trong các bounded context khác nhau, có thể được quản lý bởi các microservice khác nhau.
          - **"TẠI SAO"?** Giúp tránh sự mơ hồ về ý nghĩa của các thuật ngữ nghiệp vụ, giảm sự phụ thuộc không cần thiết giữa các phần của hệ thống, và cho phép các mô hình nghiệp vụ phát triển độc lập.
      3.  **Decentralized Data Management (Mỗi Service sở hữu Database riêng):**
          - Mỗi microservice nên có CSDL riêng, không chia sẻ CSDL với các service khác.
          - Nếu service A cần dữ liệu từ service B, nó phải gọi API của service B, không được truy cập trực tiếp vào CSDL của B.
          - **"TẠI SAO"?**
            - **Đảm bảo sự độc lập (autonomy) của service:** Service có thể thay đổi schema CSDL của mình mà không ảnh hưởng đến các service khác.
            - **Lựa chọn CSDL phù hợp:** Mỗi service có thể chọn loại CSDL (SQL, NoSQL) phù hợp nhất cho nhu cầu của nó.
            - **Tránh tight coupling ở tầng dữ liệu.**
          - **Thách thức:** Tính nhất quán dữ liệu (eventual consistency, sagas), join dữ liệu.
      4.  **Design for Failure (Thiết kế cho Khả năng Chịu lỗi):**
          - Trong một hệ thống phân tán, lỗi mạng và lỗi của các service phụ thuộc là điều không thể tránh khỏi.
          - Ứng dụng phải được thiết kế để xử lý các lỗi này một cách linh hoạt và không làm sập toàn bộ hệ thống.
          - **Các kỹ thuật:**
            - **Timeouts:** Đặt thời gian chờ khi gọi service khác.
            - **Retries:** Thử gọi lại service bị lỗi sau một khoảng thời gian (với exponential backoff).
            - **Circuit Breaker Pattern:** Ngăn chặn việc gọi liên tục đến một service đang bị lỗi. Sau một số lần lỗi, "mạch" sẽ "ngắt", các request sau đó sẽ fail fast (thất bại nhanh) mà không cần gọi service đó. Sau một thời gian, mạch sẽ thử "đóng" lại. (Ví dụ thư viện: `opossum`).
            - **Bulkheads:** Cô lập tài nguyên cho các service khác nhau để lỗi ở một service không ảnh hưởng đến tài nguyên của service khác.
            - **Fallbacks:** Cung cấp một hành vi dự phòng khi một service lỗi (ví dụ: trả về dữ liệu cache, dữ liệu mặc định, hoặc thông báo lỗi thân thiện).
      5.  **Evolutionary Design (Thiết kế có Khả năng Tiến hóa):**
          - Kiến trúc microservices cho phép hệ thống thay đổi và phát triển theo thời gian dễ dàng hơn monolith.
          - Có thể thêm service mới, thay thế service cũ, hoặc thay đổi công nghệ của một service mà ít ảnh hưởng đến phần còn lại.
          - Cần có các quy trình và công cụ tốt để quản lý sự thay đổi này (versioning API, CI/CD).

    - **Khi nào nên (và không nên) sử dụng Microservices?**

      - **NÊN cân nhắc Microservices khi:**
        - Ứng dụng rất lớn và phức tạp, khó quản lý như một monolith.
        - Cần scale các phần của ứng dụng một cách độc lập.
        - Có nhiều team phát triển muốn làm việc song song.
        - Muốn sử dụng các công nghệ khác nhau cho các phần khác nhau của ứng dụng.
        - Yêu cầu cao về tính sẵn sàng và khả năng chịu lỗi.
        - Đội ngũ có đủ kỹ năng về DevOps và kiến trúc phân tán.
      - **KHÔNG NÊN (hoặc cần cẩn trọng) khi:**
        - Ứng dụng nhỏ, đơn giản. Monolith có thể là lựa chọn tốt hơn.
        - Team nhỏ, thiếu kinh nghiệm về hệ thống phân tán và DevOps.
        - Thời gian ra mắt sản phẩm (time-to-market) rất gấp và chưa có kinh nghiệm với microservices (có thể làm chậm ban đầu).
        - Chưa xác định rõ ranh giới nghiệp vụ (bounded contexts).
        - **"You must be this tall to use microservices" (Martin Fowler):** Cần có một mức độ trưởng thành nhất định về kỹ thuật và tổ chức.
      - **Bắt đầu với monolith rồi tách dần (Monolith First / Strangler Fig Pattern):** Một chiến lược phổ biến là bắt đầu xây dựng ứng dụng như một monolith để ra mắt nhanh, sau đó khi ứng dụng phát triển và các vấn đề của monolith trở nên rõ ràng, từ từ "bóp nghẹt" (strangle) monolith bằng cách tách các chức năng ra thành các microservice mới.

    - **Microservices vs. SOA (Service-Oriented Architecture):**
      - Cả hai đều là kiến trúc hướng dịch vụ.
      - **SOA (Thường là cũ hơn):**
        - Thường có các service lớn hơn, phạm vi rộng hơn (enterprise-level services).
        - Thường sử dụng các cơ chế giao tiếp nặng nề hơn (ví dụ: SOAP, ESB - Enterprise Service Bus). ESB là một điểm tập trung logic, có thể gây bottleneck.
        - Chia sẻ schema dữ liệu hoặc CSDL chung có thể phổ biến hơn.
        - Quản trị tập trung hơn.
      - **Microservices:**
        - Service nhỏ hơn, tập trung vào một business capability hẹp.
        - Giao tiếp nhẹ nhàng (HTTP/REST, gRPC, message queues). "Smart endpoints and dumb pipes."
        - Mỗi service sở hữu dữ liệu riêng.
        - Quản trị phi tập trung.
      - Có thể coi Microservices là một cách triển khai cụ thể, hiện đại hơn của SOA, tập trung vào sự độc lập và linh hoạt cao hơn.

5.  **Minh họa trực quan và Code mẫu (Chủ yếu là sơ đồ và khái niệm ở phần này)**

    - **Sơ đồ Monolith vs. Microservices:**

      ```mermaid
      graph TD
          subgraph Monolith Architecture
              direction LR
              M_UI["User Interface"] --> M_App["Monolithic Application (User, Product, Order Logic + DB Access)"];
              M_App --> M_DB["Single Database"];
          end

          subgraph Microservice Architecture
              direction LR
              MS_UI["User Interface / API Gateway"] --> MS_User["UserService (User DB)"];
              MS_UI --> MS_Product["ProductService (Product DB)"];
              MS_UI --> MS_Order["OrderService (Order DB)"];

              MS_User --> DB_User["User Database"];
              MS_Product --> DB_Product["Product Database"];
              MS_Order --> DB_Order["Order Database"];

              MS_Order -- "Calls API" --> MS_User;
              MS_Order -- "Calls API" --> MS_Product;
              MS_User -- "Events?" --> MS_Order;
          end
          note right of M_App : All components tightly coupled, single deployment unit.
          note right of MS_Order : Services loosely coupled, independent deployment, own data.
      ```

6.  **Best Practices và Quy ước (Conventions) khi bắt đầu với Microservices:**

    - **Bắt đầu nhỏ (nếu có thể):** Đừng cố gắng xây dựng quá nhiều service cùng lúc.
    - **Xác định Bounded Contexts cẩn thận:** Đây là chìa khóa để phân chia service đúng.
    - **API First Design:** Thiết kế API contract (ví dụ: OpenAPI/Swagger) cho các service trước khi triển khai.
    - **Tự động hóa mọi thứ:** CI/CD, testing, deployment.
    - **Đầu tư vào Logging và Monitoring tập trung ngay từ đầu.**
    - **Thiết kế cho khả năng chịu lỗi.**
    - **Giữ các service nhỏ và tập trung.**
    - **Mỗi service nên có một team sở hữu (nếu có thể).**

7.  **Anti-patterns và Lỗi thường gặp (Common Pitfalls) khi chuyển sang Microservices:**

    - **Distributed Monolith:** Các "microservice" nhưng lại phụ thuộc chặt chẽ vào nhau, deploy cùng lúc, hoặc chia sẻ CSDL. Đây là trường hợp tệ nhất của cả hai thế giới.
    - **Service quá nhỏ (Nanoservices):** Tạo ra quá nhiều service rất nhỏ, làm tăng overhead giao tiếp và quản lý mà không mang lại nhiều lợi ích nghiệp vụ.
    - **Chia sẻ CSDL giữa các service.**
    - **Không có chiến lược cho tính nhất quán dữ liệu phân tán.**
    - **Bỏ qua độ phức tạp của hệ thống phân tán.**
    - **Thiếu tự động hóa và công cụ DevOps phù hợp.**
    - **Giao tiếp đồng bộ quá nhiều:** Gây ra cascading failures. Cân nhắc giao tiếp bất đồng bộ (message queues).

8.  **So sánh, Đánh giá và Lựa chọn (Đã đề cập trong "Khi nào nên dùng")**

9.  **Bài tập thực hành và Gợi ý dự án nhỏ:**

    - **Câu hỏi lý thuyết:**
      1.  Nêu 3 lợi ích chính và 3 thách thức chính của kiến trúc Microservices so với Monolith.
      2.  Bounded Context là gì và tại sao nó quan trọng khi thiết kế Microservices?
      3.  Giải thích nguyên tắc "Mỗi service sở hữu CSDL riêng". Những vấn đề nào có thể phát sinh từ nguyên tắc này?
      4.  Circuit Breaker pattern hoạt động như thế nào và nó giúp giải quyết vấn đề gì?
      5.  Một công ty có một ứng dụng monolith đang gặp vấn đề về scaling và tốc độ phát triển. Bạn sẽ tư vấn cho họ những yếu tố nào cần cân nhắc trước khi quyết định chuyển sang Microservices?
    - **Thử thách (mang tính lý thuyết/thiết kế):**
      1.  **Phân rã Monolith:** Lấy ví dụ ứng dụng Quản lý công việc (Task Management) mà chúng ta đã xây dựng. Nếu muốn chuyển nó thành kiến trúc microservices, bạn sẽ chia nó thành các service nào? Mỗi service sẽ chịu trách nhiệm gì? Chúng sẽ giao tiếp với nhau như thế nào? Dữ liệu sẽ được quản lý ra sao?
      2.  **Thiết kế API cho một Service:** Chọn một service từ bài tập trên (ví dụ: TaskService) và phác thảo các API endpoint mà nó sẽ cung cấp (HTTP method, path, request/response body).
    - **Gợi ý dự án nhỏ (nếu muốn bắt đầu code):**
      - Tạo hai service Node.js/TypeScript/Express rất đơn giản (ví dụ: `UserService` và `SimpleLogService`).
      - `UserService` có endpoint `POST /users` để tạo user (lưu vào mảng trong bộ nhớ).
      - Khi user được tạo, `UserService` sẽ gọi một endpoint `POST /logs` của `SimpleLogService` để ghi lại sự kiện (giao tiếp qua HTTP request).
      - (Đây là bước đầu để cảm nhận về giao tiếp liên service).

10. **Câu hỏi phỏng vấn thường gặp:**
    - "What are microservices, and how do they differ from a monolithic architecture?"
    - "What are the main advantages and disadvantages of using microservices?"
    - "Can you explain the Single Responsibility Principle in the context of microservices?"
    - "What is a Bounded Context, and how does it relate to microservice design?"
    - "Why is it generally recommended for each microservice to have its own database?"
    - "What does 'Design for Failure' mean in a microservices architecture? Give some examples of patterns that support this."
    - "When would you advise a team to adopt microservices, and when might it be a bad idea?"
    - "What are some common challenges in managing a microservices ecosystem?"
    - "How do microservices communicate with each other? Discuss synchronous vs. asynchronous communication."
    - "What is eventual consistency, and why is it often relevant in microservices?"

---

Đây là phần giới thiệu tổng quan về kiến trúc Microservices. Để thực sự làm chủ nó, bạn cần đi sâu hơn vào từng khía cạnh như giao tiếp liên service, quản lý dữ liệu, patterns cụ thể, và các công cụ hỗ trợ. Trong phần tiếp theo, chúng ta sẽ khám phá một số pattern và công nghệ cụ thể thường dùng trong hệ sinh thái microservices.

Chắc chắn rồi! Sau khi đã có cái nhìn tổng quan về kiến trúc Microservices, chúng ta sẽ đi sâu hơn vào các **Patterns và Công nghệ hỗ trợ** cụ thể thường được sử dụng để xây dựng và vận hành các hệ thống Microservices một cách hiệu quả. Phần này sẽ tập trung vào các giải pháp cho những thách thức đã được đề cập.

---

### **PHẦN 15: Patterns và Công nghệ Hỗ trợ cho Kiến trúc Microservices**

1.  **Tên phần học:** Patterns và Công nghệ Hỗ trợ cho Kiến trúc Microservices.

2.  **Mục tiêu học phần:**

    - Hiểu và biết cách áp dụng các **Communication Patterns (Mẫu Giao tiếp)** giữa các microservices:
      - **Synchronous Communication:** REST APIs, gRPC.
      - **Asynchronous Communication / Event-Driven:** Message Queues (ví dụ: RabbitMQ, Kafka), Event Sourcing, CQRS (giới thiệu).
    - Nắm vững các **Service Discovery Patterns (Mẫu Khám phá Dịch vụ)**:
      - Client-Side Discovery.
      - Server-Side Discovery (ví dụ: sử dụng API Gateway, Service Registry như Consul, Eureka).
    - Hiểu và áp dụng **Resiliency Patterns (Mẫu Tăng cường Khả năng Chịu lỗi)**:
      - Circuit Breaker.
      - Retry with Exponential Backoff.
      - Timeout.
      - Bulkhead.
    - Làm quen với **API Gateway Pattern**: Vai trò, lợi ích và cách triển khai cơ bản.
    - Hiểu các chiến lược **Distributed Data Management Patterns (Mẫu Quản lý Dữ liệu Phân tán)**:
      - Database per Service.
      - Saga Pattern (cho distributed transactions).
      - Eventual Consistency.
    - Tìm hiểu về **Distributed Tracing và Logging Tập trung**:
      - Sử dụng Correlation IDs.
      - Công cụ (ví dụ: Jaeger, Zipkin cho tracing; ELK Stack, Splunk cho logging).
    - Làm quen với **Containerization (Docker)** và **Orchestration (Kubernetes - K8s)** ở mức độ khái niệm và vai trò của chúng trong Microservices.
    - Giới thiệu về **Serverless (FaaS - Functions as a Service)** như một lựa chọn cho việc xây dựng microservices.
    - Trả lời được:
      - So sánh ưu nhược điểm của giao tiếp đồng bộ (REST, gRPC) và bất đồng bộ (message queues) giữa các service.
      - API Gateway là gì và nó giải quyết vấn_đề gì?
      - Circuit Breaker pattern hoạt động như thế nào?
      - Saga pattern dùng để làm gì trong quản lý dữ liệu phân tán?
      - Tại sao Kubernetes lại quan trọng cho việc vận hành microservices?
      - Event Sourcing và CQRS là gì (ở mức độ cơ bản)?

3.  **Kiến thức tiên quyết:**

    - Hoàn thành Phần 14 (Giới thiệu Microservices).
    - Kiến thức về REST APIs, HTTP.
    - Hiểu biết cơ bản về Docker (Phần 13).

4.  **Giải thích lý thuyết chuyên sâu:**

    - **Communication Patterns (Mẫu Giao tiếp):**

      1.  **Synchronous Communication (Giao tiếp Đồng bộ):**

          - Client gửi request và đợi response từ service khác trước khi tiếp tục.
          - **REST APIs (HTTP/HTTPS):** Phổ biến nhất. Sử dụng các HTTP verbs (GET, POST, PUT, DELETE) và định dạng dữ liệu như JSON.
            - **Ưu điểm:** Đơn giản, quen thuộc, nhiều công cụ hỗ trợ, dễ debug ban đầu.
            - **Nhược điểm:** Client bị block chờ response, dễ gây cascading failures nếu service được gọi bị chậm hoặc lỗi, coupling về mặt thời gian.
          - **gRPC (Google Remote Procedure Call):**
            - Một framework RPC hiệu năng cao, mã nguồn mở, được phát triển bởi Google.
            - Sử dụng **Protocol Buffers (Protobuf)** làm Interface Definition Language (IDL) để định nghĩa schema của service và messages. Protobuf hiệu quả hơn JSON về kích thước và tốc độ parse.
            - Sử dụng **HTTP/2** làm transport, hỗ trợ streaming (unary, client-streaming, server-streaming, bidirectional-streaming).
            - **Ưu điểm:** Hiệu năng cao (do binary serialization và HTTP/2), strong typing với Protobuf, hỗ trợ code generation cho nhiều ngôn ngữ, streaming.
            - **Nhược điểm:** Ít thân thiện với trình duyệt trực tiếp hơn REST (cần proxy như gRPC-Web), hệ sinh thái công cụ có thể ít hơn REST cho một số tác vụ.
            - **"KHI NÀO" dùng gRPC?** Cho giao tiếp nội bộ giữa các microservice yêu cầu hiệu năng cao và low latency, đặc biệt khi có nhiều service cùng stack công nghệ (dễ generate code).
          - **Thách thức chung của Synchronous:**
            - **Service Availability:** Nếu service B mà service A gọi bị down, service A cũng có thể bị ảnh hưởng.
            - **Latency:** Tổng độ trễ là tổng của các hop.

      2.  **Asynchronous Communication / Event-Driven Architecture (EDA) (Giao tiếp Bất đồng bộ / Kiến trúc Hướng sự kiện):**
          - Client gửi message/event và không cần đợi response ngay. Service xử lý message/event khi có thể.
          - **Message Queues (Hàng đợi Tin nhắn):**
            - Các service giao tiếp bằng cách gửi và nhận message qua một message broker (ví dụ: RabbitMQ, Apache Kafka, Redis Streams, AWS SQS, Google Pub/Sub).
            - Producer (bên gửi) đẩy message vào queue/topic. Consumer (bên nhận) lắng nghe và xử lý message từ queue/topic.
            - **"TẠI SAO" Message Queues?**
              - **Decoupling (Giảm phụ thuộc):** Producer và Consumer không cần biết về nhau, chỉ cần biết về message broker và định dạng message.
              - **Resilience (Khả năng chịu lỗi):** Nếu consumer bị lỗi, message vẫn còn trong queue và có thể được xử lý lại sau. Broker cũng có thể buffer message nếu consumer tạm thời không sẵn sàng.
              - **Scalability:** Dễ dàng scale số lượng consumer để xử lý nhiều message hơn.
              - **Load Leveling:** Giúp san bằng tải cho các service xử lý nặng.
            - **Ví dụ thư viện:** `amqplib` cho RabbitMQ, `kafkajs` cho Kafka.
            - **RabbitMQ:** Message broker truyền thống, hỗ trợ nhiều protocol (AMQP, MQTT, STOMP), routing linh hoạt (direct, fanout, topic, headers exchanges). Tốt cho các kịch bản task queues, RPC qua message.
            - **Apache Kafka:** Nền tảng streaming sự kiện phân tán, hiệu năng cao, khả năng chịu lỗi tốt, lưu trữ message lâu dài. Tốt cho event sourcing, log aggregation, stream processing.
          - **Event Sourcing:**
            - Một pattern lưu trữ trạng thái của một thực thể không phải là trạng thái hiện tại, mà là một **chuỗi các sự kiện (events)** đã xảy ra với thực thể đó theo thứ tự thời gian.
            - Trạng thái hiện tại của thực thể có thể được tái tạo bằng cách "phát lại" (replay) các sự kiện.
            - **"TẠI SAO"?** Cung cấp audit log đầy đủ, dễ debug, cho phép phân tích lịch sử, dễ dàng xây dựng các "views" khác nhau của dữ liệu.
            - Thường đi kèm với CQRS.
          - **CQRS (Command Query Responsibility Segregation - Tách biệt Trách nhiệm Lệnh và Truy vấn):**
            - Một pattern tách biệt phần xử lý thay đổi trạng thái (Commands - ghi) khỏi phần đọc trạng thái (Queries - đọc).
            - Có thể có các model dữ liệu khác nhau, thậm chí CSDL khác nhau cho việc ghi và đọc.
            - **"TẠI SAO"?** Tối ưu hóa riêng cho việc ghi và đọc, tăng khả năng mở rộng và hiệu năng.
            - Ví dụ: Model ghi có thể được chuẩn hóa cao, model đọc có thể là các view đã được denormalize để truy vấn nhanh.
            - Thường dùng chung với Event Sourcing: Event Sourcing cho phần Command, các "projection" của events được lưu vào các read model tối ưu cho Query.

    - **Service Discovery Patterns (Mẫu Khám phá Dịch vụ):**

      - Trong môi trường microservices, các instance của service có thể được tạo và hủy động, địa chỉ IP và port có thể thay đổi. Các service cần một cách để tìm thấy nhau.
      - **Client-Side Discovery:**
        - Client chịu trách nhiệm tìm địa chỉ của service cần gọi.
        - Client query một **Service Registry** (nơi các service đăng ký thông tin khi khởi động và hủy đăng ký khi dừng).
        - Client sau đó thực hiện load balancing (ví dụ: round-robin) để chọn một instance.
        - **Ví dụ thư viện:** Netflix Eureka (client library), Consul (client library).
        - **Ưu điểm:** Client có toàn quyền kiểm soát logic load balancing.
        - **Nhược điểm:** Logic discovery và load balancing phải được implement ở mỗi client (hoặc trong thư viện client dùng chung), làm tăng độ phức tạp của client.
      - **Server-Side Discovery:**
        - Client gửi request đến một router/load balancer (thường là một phần của **API Gateway** hoặc một dedicated load balancer).
        - Router này query Service Registry và chuyển tiếp request đến một instance service phù hợp.
        - Client không cần biết về Service Registry.
        - **Ưu điểm:** Đơn giản hóa client, logic discovery tập trung.
        - **Nhược điểm:** Thêm một hop mạng, router/load balancer trở thành một component quan trọng cần quản lý.
      - **Service Registry:** Một CSDL lưu trữ thông tin về các instance service đang chạy (tên service, địa chỉ IP, port, health status). Các service tự đăng ký khi khởi động và gửi heartbeat để báo hiệu còn sống. Ví dụ: Consul, etcd, Apache Zookeeper, Netflix Eureka. Kubernetes cũng có cơ chế service discovery tích hợp.

    - **Resiliency Patterns (Mẫu Tăng cường Khả năng Chịu lỗi):** (Đã giới thiệu ở Phần 14)

      - **Circuit Breaker:** (Thư viện: `opossum` cho Node.js)
        - Ngăn chặn cascading failures bằng cách "ngắt mạch" khi một service phụ thuộc liên tục bị lỗi.
        - Trạng thái: Closed (cho phép request), Open (fail fast, không gọi service lỗi), Half-Open (thử một vài request để xem service đã phục hồi chưa).
      - **Retry with Exponential Backoff:**
        - Tự động thử lại request bị lỗi sau một khoảng thời gian tăng dần (ví dụ: 1s, 2s, 4s, 8s...) để tránh làm quá tải service đang gặp sự cố.
      - **Timeout:**
        - Đặt thời gian chờ tối đa cho một request. Nếu không có response trong khoảng thời gian đó, request sẽ bị hủy và trả về lỗi timeout.
      - **Bulkhead:**
        - Cô lập tài nguyên (ví dụ: connection pools, thread pools) cho các lời gọi đến các service khác nhau. Lỗi ở một "khoang" (bulkhead) không làm ảnh hưởng đến các khoang khác.

    - **API Gateway Pattern:**

      - **Là gì?** Một server duy nhất đóng vai trò là **điểm vào (single entry point)** cho tất cả các client request đến hệ thống microservices.
      - **Nhiệm vụ của API Gateway:**
        1.  **Request Routing:** Định tuyến request đến microservice phù hợp.
        2.  **API Composition/Aggregation:** Gộp kết quả từ nhiều microservice thành một response duy nhất cho client.
        3.  **Protocol Translation:** Ví dụ: Client dùng REST, các service nội bộ dùng gRPC.
        4.  **Authentication & Authorization:** Xử lý xác thực và phân quyền tập trung trước khi request đến các service.
        5.  **Rate Limiting & Throttling:** Giới hạn số lượng request từ client.
        6.  **Caching:** Cache các response thường xuyên được yêu cầu.
        7.  **SSL Termination:** Xử lý mã hóa/giải mã SSL.
        8.  **Logging & Monitoring (tập trung).**
        9.  **Service Discovery (nếu dùng Server-Side Discovery).**
      - **"TẠI SAO" API Gateway?**
        - Đơn giản hóa client (client chỉ cần biết một địa chỉ API Gateway).
        - Giảm số lượng round trips giữa client và backend.
        - Cung cấp một lớp bảo vệ và quản lý chung cho các microservice.
        - Ẩn đi cấu trúc microservice bên trong khỏi client.
      - **Thách thức:** API Gateway có thể trở thành một điểm bottleneck hoặc single point of failure nếu không được thiết kế và scale đúng cách.
      - **Các giải pháp API Gateway:** AWS API Gateway, Google Cloud API Gateway, Apigee, Kong, NGINX Plus, Express Gateway, Ocelot (.NET). Hoặc tự xây dựng (ví dụ: dùng Express.js với các middleware cần thiết).
      - **Backend for Frontend (BFF) Pattern:** Một biến thể của API Gateway, nơi mỗi loại client (ví dụ: mobile app, web app, third-party app) có một API Gateway riêng được tối ưu cho nhu cầu cụ thể của client đó.

    - **Distributed Data Management Patterns:**

      - **Database per Service:** (Đã nói ở Phần 14) Mỗi service quản lý CSDL riêng.
      - **Saga Pattern (cho Distributed Transactions):**
        - Khi một giao dịch nghiệp vụ trải dài qua nhiều microservice (mỗi service có DB riêng), không thể dùng transaction ACID truyền thống.
        - Saga là một chuỗi các **local transactions** (giao dịch cục bộ trong từng service). Mỗi local transaction cập nhật dữ liệu trong service của nó và publish một event (hoặc gửi command) để trigger local transaction tiếp theo trong saga.
        - Nếu một local transaction thất bại, saga phải thực thi các **compensating transactions** (giao dịch bù trừ) để hoàn tác các thay đổi đã thực hiện ở các bước trước đó, nhằm duy trì tính nhất quán dữ liệu (thường là eventual consistency).
        - **Hai cách điều phối Saga:**
          1.  **Choreography-based Saga:** Mỗi service publish event, các service khác lắng nghe và phản ứng. Không có điểm điều phối trung tâm.
          2.  **Orchestration-based Saga:** Một Saga Orchestrator (một service hoặc component riêng) điều phối toàn bộ saga, gửi command cho từng service và lắng nghe event từ chúng.
        - **Thách thức:** Phức tạp để thiết kế và debug, đặc biệt là logic bù trừ.
      - **Eventual Consistency (Tính nhất quán sau cùng):**
        - Trong hệ thống phân tán, thường khó đạt được strong consistency (dữ liệu nhất quán ngay lập tức trên tất cả các node/service).
        - Eventual consistency chấp nhận rằng dữ liệu có thể không nhất quán trong một khoảng thời gian ngắn, nhưng cuối cùng sẽ trở nên nhất quán.
        - Thường đạt được thông qua giao tiếp bất đồng bộ và xử lý sự kiện.

    - **Distributed Tracing và Logging Tập trung:**

      - **Correlation IDs (Request IDs):**
        - Gán một ID duy nhất cho mỗi external request khi nó vào hệ thống (thường ở API Gateway).
        - ID này được truyền qua tất cả các microservice mà request đó đi qua (thường qua HTTP headers hoặc message headers).
        - Tất cả các log liên quan đến request đó đều chứa correlation ID này.
        - Giúp dễ dàng theo dõi và xâu chuỗi log của một request cụ thể qua nhiều service.
      - **Distributed Tracing:**
        - Theo dõi một request khi nó đi qua các service khác nhau, ghi lại thời gian ở mỗi service và mối quan hệ gọi giữa chúng (spans, traces).
        - **Công cụ:** Jaeger, Zipkin, OpenTelemetry (tiêu chuẩn mở).
        - Giúp xác định bottlenecks, hiểu luồng request, debug lỗi phân tán.
      - **Logging Tập trung:**
        - Thu thập log từ tất cả các microservice về một nơi trung tâm để dễ dàng tìm kiếm, phân tích, và cảnh báo.
        - **Công cụ:** ELK Stack (Elasticsearch, Logstash, Kibana), Graylog, Splunk, Datadog Logs.

    - **Containerization (Docker) và Orchestration (Kubernetes - K8s):**

      - **Docker:** (Đã nói ở Phần 13) Đóng gói mỗi microservice và dependencies của nó vào một container image.
      - **Kubernetes (K8s):**
        - Một nền tảng mã nguồn mở để **tự động hóa việc triển khai, scaling, và quản lý các ứng dụng containerized**.
        - **"TẠI SAO" Kubernetes cho Microservices?**
          1.  **Service Discovery & Load Balancing:** Tự động khám phá và cân bằng tải cho các instance service.
          2.  **Automated Rollouts & Rollbacks:** Triển khai phiên bản mới và hoàn tác một cách tự động.
          3.  **Self-healing:** Tự động khởi động lại container bị lỗi, thay thế container không phản hồi.
          4.  **Storage Orchestration:** Quản lý storage cho các stateful application.
          5.  **Secret & Configuration Management:** Quản lý an toàn các thông tin nhạy cảm và cấu hình.
          6.  **Horizontal Scaling:** Dễ dàng scale số lượng instance của một service.
        - **Khái niệm chính:** Pods, Services, Deployments, ReplicaSets, ConfigMaps, Secrets, Ingress.
        - **Đường cong học tập dốc.** Các dịch vụ cloud như AWS EKS, Google GKE, Azure AKS cung cấp Kubernetes được quản lý, giảm bớt gánh nặng vận hành.

    - **Serverless (FaaS - Functions as a Service):**
      - Một mô hình cloud computing nơi nhà cung cấp cloud quản lý việc thực thi code bằng cách tự động cấp phát và quản lý server.
      - Bạn chỉ viết code dưới dạng các **functions** nhỏ, độc lập, được trigger bởi các sự kiện (HTTP request, message queue, DB event).
      - **"Pay-as-you-go":** Chỉ trả tiền cho thời gian code thực sự chạy.
      - **Ví dụ:** AWS Lambda, Google Cloud Functions, Azure Functions.
      - **"TẠI SAO" Serverless cho Microservices?**
        - Mỗi function có thể được coi là một microservice rất nhỏ (nanoservice).
        - Tự động scaling.
        - Không cần quản lý server.
        - Phù hợp cho các tác vụ xử lý sự kiện, API backend đơn giản.
      - **Thách thức:** "Cold starts" (độ trễ khi function được gọi lần đầu sau một thời gian không hoạt động), giới hạn thời gian thực thi, state management, debugging và testing có thể phức tạp hơn.

5.  **Minh họa trực quan và Code mẫu (Chủ yếu là sơ đồ và khái niệm ở phần này)**

    - **Sơ đồ tổng thể một hệ thống Microservices điển hình:**

      ```mermaid
      graph TD
          Client["Client (Web/Mobile)"] --> AG["API Gateway"];

          subgraph Microservices_Cluster ["Microservices Cluster (e.g., Kubernetes)"]
              direction LR
              AG --> SD["Service Discovery (e.g., Consul, K8s Service)"];

              SD --> S1["UserService"];
              SD --> S2["ProductService"];
              SD --> S3["OrderService"];
              SD --> S4["NotificationService (Async via MQ)"];

              S1 --> DB1["User DB"];
              S2 --> DB2["Product DB"];
              S3 --> DB3["Order DB"];

              S3 -- "HTTP/gRPC Call (Sync)" --> S1;
              S3 -- "HTTP/gRPC Call (Sync)" --> S2;
              S3 -- "Publishes Event" --> MQ["Message Queue (RabbitMQ/Kafka)"];
              MQ -- "Consumes Event" --> S4;
          end

          subgraph Observability_Stack ["Observability Stack"]
              direction TB
              S1 --> LogAgg["Logging Aggregator (ELK/Splunk)"];
              S2 --> LogAgg;
              S3 --> LogAgg;
              S4 --> LogAgg;

              S1 --> Tracing["Distributed Tracing (Jaeger/Zipkin)"];
              S2 --> Tracing;
              S3 --> Tracing;

              S1 --> Monitoring["Metrics & Monitoring (Prometheus/Grafana/Datadog)"];
              S2 --> Monitoring;
              S3 --> Monitoring;
          end

          AG -- "Authentication, Rate Limiting, Routing" --> S1;
          AG -- "Routing" --> S2;
          AG -- "Routing" --> S3;

          note right of MQ : Asynchronous Communication.
          note right of S3 : Saga pattern might be used for order processing.
          note left of AG : Handles cross-cutting concerns.
      ```

6.  **Best Practices và Quy ước (Conventions):**

    - **Chọn đúng công cụ cho từng việc:** REST cho API public, gRPC cho internal M2M (machine-to-machine) communication, Message Queues cho async.
    - **Thiết kế API contract rõ ràng (OpenAPI, Protobuf).**
    - **Đầu tư vào Observability (Logging, Tracing, Monitoring) ngay từ đầu.**
    - **Tự động hóa CI/CD và Infrastructure (IaC).**
    - **Áp dụng resiliency patterns.**
    - **Sử dụng API Gateway.**
    - **Bắt đầu với số lượng service nhỏ và tăng dần khi cần.**
    - **Security là ưu tiên hàng đầu ở mọi lớp.**

7.  **Anti-patterns và Lỗi thường gặp (Đã đề cập nhiều ở Phần 14, bổ sung):**

    - **Chatty I/O:** Quá nhiều lời gọi đồng bộ nhỏ giữa các service, làm tăng latency. Cân nhắc API Composition ở Gateway hoặc dùng async.
    - **Hardcoding service locations:** Luôn dùng Service Discovery.
    - **Không có chiến lược retry hoặc timeout phù hợp.**
    - **Bỏ qua việc quản lý transaction phân tán (nếu cần thiết).**
    - **Monolithic releases:** Deploy tất cả service cùng lúc dù chỉ một service thay đổi.
    - **Thiếu sự nhất quán trong logging và monitoring.**

8.  **So sánh, Đánh giá và Lựa chọn:**

    - **REST vs. gRPC vs. Message Queues:**
      - **REST:** Dễ hiểu, phổ biến, nhiều công cụ, tốt cho API public.
      - **gRPC:** Hiệu năng cao, strong typing, streaming, tốt cho internal M2M.
      - **Message Queues:** Decoupling, resilience, scalability, tốt cho async, event-driven.
      - Thường một hệ thống microservices sẽ sử dụng kết hợp các phương thức này.
    - **Kubernetes vs. Serverless (FaaS):**
      - **Kubernetes:** Kiểm soát hoàn toàn, linh hoạt, phù hợp cho các ứng dụng phức tạp, stateful. Đòi hỏi nhiều kiến thức vận hành.
      - **Serverless:** Đơn giản, tự động scale, pay-as-you-go, phù hợp cho các function nhỏ, stateless, event-driven. Ít kiểm soát hơn.
      - Có thể kết hợp cả hai.

9.  **Bài tập thực hành và Gợi ý dự án nhỏ:**

    - **Câu hỏi lý thuyết:**
      1.  Sự khác biệt chính giữa giao tiếp đồng bộ và bất đồng bộ trong microservices là gì? Cho ví dụ về công nghệ cho mỗi loại.
      2.  API Gateway đóng vai trò gì trong kiến trúc microservices?
      3.  Saga pattern được sử dụng để giải quyết vấn đề gì? Mô tả ngắn gọn một cách điều phối Saga.
      4.  Tại sao Correlation ID lại quan trọng cho việc debugging trong hệ thống microservices?
      5.  So sánh Kubernetes và Serverless (FaaS) dưới góc độ xây dựng microservices.
    - **Thử thách (mang tính thiết kế/nghiên cứu):**
      1.  **Thiết kế Hệ thống Đặt vé xem phim (Microservices):**
          - Xác định các service chính (ví dụ: MovieService, ShowtimeService, BookingService, UserService, PaymentService, NotificationService).
          - Mô tả trách nhiệm của mỗi service.
          - Chọn phương thức giao tiếp (sync/async) giữa các service cho một luồng đặt vé.
          - Đề xuất cách xử lý transaction đặt vé (liên quan đến nhiều service).
          - Cần những resiliency pattern nào?
      2.  **Nghiên cứu một Message Broker:** Chọn RabbitMQ hoặc Kafka. Tìm hiểu cách cài đặt (local), cách publish và consume message cơ bản bằng một thư viện Node.js.
      3.  **Nghiên cứu gRPC:** Tìm hiểu cách định nghĩa một service bằng Protobuf và tạo client/server gRPC đơn giản trong Node.js/TypeScript.
    - **Gợi ý dự án nhỏ (có thể bắt đầu xây dựng từng service một):**
      - Bắt đầu với 2-3 service từ ý tưởng "Hệ thống Đặt vé xem phim" hoặc "Ứng dụng Thương mại Điện tử" (User, Product, Order).
      - Sử dụng Express.js/TypeScript cho mỗi service.
      - Cho các service giao tiếp với nhau qua REST API.
      - (Nâng cao) Thêm API Gateway (ví dụ: tự xây bằng Express, hoặc dùng Kong/Ocelot nếu có kinh nghiệm).
      - (Nâng cao hơn) Thử nghiệm giao tiếp bất đồng bộ với RabbitMQ cho một tính năng (ví dụ: gửi email thông báo sau khi đặt hàng thành công).

10. **Câu hỏi phỏng vấn thường gặp:**
    - "How do microservices typically communicate with each other? Discuss synchronous vs. asynchronous patterns."
    - "What is an API Gateway, and what are its responsibilities?"
    - "Explain the Circuit Breaker pattern and why it's used."
    - "How do you handle distributed transactions in a microservices architecture? (e.g., Saga pattern)"
    - "What is service discovery, and why is it needed in microservices?"
    - "What are some common resiliency patterns you would implement in a microservices system?"
    - "How does Kubernetes help in managing microservices?"
    - "What is the role of distributed tracing and centralized logging in a microservices environment?"
    - "Compare and contrast REST APIs with gRPC for inter-service communication."
    - "What is eventual consistency, and how does it apply to microservices?"

---

Kiến trúc Microservices là một hành trình dài và đầy thử thách nhưng cũng mang lại nhiều lợi ích to lớn cho các hệ thống phức tạp. Việc hiểu rõ các patterns và công nghệ hỗ trợ sẽ giúp bạn thiết kế và xây dựng các hệ thống microservices mạnh mẽ, linh hoạt và dễ bảo trì hơn.
