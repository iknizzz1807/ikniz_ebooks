**Tài liệu React & Next.js "Khủng Khiếp" với TypeScript (Phần 1/5): Nền Tảng React - Cái Nhìn Từ Dân SvelteKit**

---

**Lời Mở Đầu:**

Chào mừng bạn, một chiến binh từ SvelteKit, đến với lãnh địa React! Như đã nói, React chỉ là thư viện UI, còn Next.js là framework xây dựng trên nó, tương tự như Svelte và SvelteKit. Điểm cộng lớn là cả hai hệ sinh thái đều hỗ trợ **TypeScript** mạnh mẽ, giúp code an toàn hơn, dễ bảo trì và tái cấu trúc hơn.

Phần này sẽ đặt nền móng vững chắc cho **React với TypeScript**, tập trung vào những khái niệm cốt lõi bạn cần biết, luôn đối chiếu với cách bạn làm trong SvelteKit (với `<script lang="ts">`).

**Điểm Khác Biệt Cốt Lõi (React/TS vs Svelte/TS):**

- **Svelte:** Compiler-first. Type checking chủ yếu diễn ra trong khối `<script lang="ts">`. Template ít được type-check trực tiếp hơn (mặc dù có cải thiện).
- **React:** Library-first (Virtual DOM). TypeScript tích hợp sâu vào JSX (gọi là **TSX**), cho phép type check cả props, state, event handlers ngay trong quá trình viết code component.

Bắt đầu nào!

---

**1. TSX (TypeScript XML) - Template An Toàn Kiểu**

Giống JSX, nhưng dành cho TypeScript. Nó cho phép bạn viết cấu trúc giống HTML trong file `.ts` hoặc `.tsx`, với sự hỗ trợ đầy đủ của trình kiểm tra kiểu TypeScript.

- **Trong SvelteKit (`.svelte` với `lang="ts"`):**

  ```svelte
  <!-- MyComponent.svelte -->
  <script lang="ts">
    let name: string = 'World';
    let className: string = 'greeting';
    let count: number = 0;

    function handleClick() {
      count++;
      // name = 123; // Lỗi TypeScript! Type 'number' is not assignable to type 'string'.
    }
  </script>

  <h1 class={className}>Hello {name}!</h1>
  <p>Clicked {count} times.</p>
  <button on:click={handleClick}>Click</button>
  ```

- **Trong React (`.tsx`):**

  ```typescript jsx
  // MyComponent.tsx
  import React, { useState } from "react"; // Import React và Hooks

  function MyComponent(): JSX.Element {
    // Khai báo kiểu trả về là JSX.Element (best practice)
    const name: string = "World";
    const className: string = "greeting";
    const [count, setCount] = useState<number>(0); // useState với kiểu number

    function handleClick() {
      setCount((prevCount) => prevCount + 1);
      // name = 123; // Lỗi TypeScript! Cannot assign to 'name' because it is a constant.
      // setCount("abc"); // Lỗi TypeScript! Argument of type 'string' is not assignable to parameter of type 'SetStateAction<number>'.
    }

    return (
      // Phải có một thẻ cha duy nhất hoặc <></> (Fragment)
      <>
        {/* Dùng {} cho biểu thức JS */}
        <h1 className={className}>Hello {name}!</h1> {/* class -> className */}
        <p>Clicked {count} times.</p>
        {/* Thuộc tính HTML -> camelCase, ví dụ: onclick -> onClick */}
        <button onClick={handleClick}>Click</button>
      </>
    );
  }

  export default MyComponent;
  ```

**Điểm quan trọng với TSX:**

- **`className`**, **`camelCase` attributes**, **thẻ cha duy nhất**: Giống JSX.
- **File `.tsx`**: Quy ước dùng đuôi file này cho các file chứa JSX/TSX.
- **Kiểu trả về `JSX.Element`**: Nên khai báo rõ ràng kiểu trả về của function component là `JSX.Element` (hoặc `React.ReactNode` nếu có thể trả về null, string, array...).
- **Type checking mạnh mẽ**: TypeScript kiểm tra kiểu của biến, state, tham số hàm, giá trị trả về... ngay trong component.

---

**2. Components & Props Với TypeScript**

Định nghĩa component và cách chúng nhận dữ liệu (props) một cách an toàn kiểu.

- **Trong SvelteKit:** Dùng `export let propName: type;`

  ```svelte
  <!-- Child.svelte -->
  <script lang="ts">
    export let message: string; // Prop bắt buộc
    export let count: number = 0; // Prop tùy chọn với giá trị mặc định
    export let user: { id: number; name: string } | null = null; // Prop phức tạp hơn
  </script>

  <p>{message} - Count: {count}</p>
  {#if user}
    <p>User: {user.name} (ID: {user.id})</p>
  {/if}
  ```

  ```svelte
  <!-- Parent.svelte -->
  <script lang="ts">
    import Child from './Child.svelte';
    let currentUser: { id: number; name: string } = { id: 1, name: 'Alice' };
  </script>

  <Child message="Chào từ Parent" />
  <Child message="Hello" count={10} user={currentUser} />
  <!-- <Child message={123} /> --> {/* Lỗi TypeScript! */}
  ```

- **Trong React:** Định nghĩa `interface` hoặc `type` cho props.

  ```typescript jsx
  // ChildComponent.tsx
  import React from "react";

  // Định nghĩa kiểu cho props bằng Interface (phổ biến và dễ mở rộng)
  interface ChildProps {
    message: string; // Prop bắt buộc
    count?: number; // Prop tùy chọn (dấu ?)
    user?: { id: number; name: string } | null; // Prop tùy chọn và có thể null
    // Có thể thêm các hàm callback làm props
    onUpdate?: (newCount: number) => void;
  }

  // Sử dụng React.FC (Functional Component) - Cách cũ hơn, ít được khuyên dùng hiện nay
  // const ChildComponent: React.FC<ChildProps> = ({ message, count = 0, user = null, onUpdate }) => {
  //   // React.FC tự động thêm kiểu cho `children`, đôi khi không mong muốn
  //   // ...
  // };

  // Cách hiện đại và được khuyên dùng: Định nghĩa kiểu trực tiếp cho tham số props
  function ChildComponent({
    message,
    count = 0,
    user = null,
    onUpdate,
  }: ChildProps): JSX.Element {
    // Giá trị mặc định được đặt trong destructuring (count = 0, user = null)

    const handleInternalUpdate = () => {
      const newCount = count + 1;
      // Gọi callback nếu nó được truyền vào
      onUpdate?.(newCount); // Dùng optional chaining (?.) để gọi an toàn
    };

    return (
      <div>
        <p>
          {message} - Count: {count}
        </p>
        {user && ( // Dùng conditional rendering (sẽ nói kỹ hơn)
          <p>
            User: {user.name} (ID: {user.id})
          </p>
        )}
        {/* Ví dụ sử dụng callback prop */}
        {onUpdate && (
          <button onClick={handleInternalUpdate}>
            Update Count Internally
          </button>
        )}
      </div>
    );
  }

  export default ChildComponent;
  ```

  ```typescript jsx
  // ParentComponent.tsx
  import React, { useState } from "react";
  import ChildComponent from "./ChildComponent";

  function ParentComponent(): JSX.Element {
    const [parentCount, setParentCount] = useState<number>(0);
    const currentUser: { id: number; name: string } = { id: 1, name: "Alice" };

    const handleChildUpdate = (newCount: number) => {
      console.log("Child updated count to:", newCount);
      setParentCount(newCount); // Cập nhật state của Parent dựa trên sự kiện từ Child
    };

    return (
      <div>
        <h2>Parent State Count: {parentCount}</h2>
        <ChildComponent message="Chào từ Parent" onUpdate={handleChildUpdate} />
        <ChildComponent message="Hello" count={10} user={currentUser} />
        {/* <ChildComponent message={123} /> */} {/* Lỗi TypeScript! */}
        {/* <ChildComponent message="Thiếu prop bắt buộc?" /> */}{" "}
        {/* Vẫn chạy, nhưng có thể có warning nếu linter cấu hình chặt */}
      </div>
    );
  }

  export default ParentComponent;
  ```

**So sánh Props & TypeScript:**

- **Svelte:** `export let propName: type;` ngắn gọn. Giá trị mặc định đặt ngay sau kiểu.
- **React:** Cần định nghĩa `interface` hoặc `type` riêng. Dùng `?` cho props tùy chọn. Giá trị mặc định đặt trong destructuring `{ prop = defaultValue }`. Cung cấp type-checking mạnh mẽ khi sử dụng component (`<ChildComponent ... />`). Truyền hàm callback làm props là rất phổ biến.

---

**3. State (`useState`) Với TypeScript**

Quản lý state nội tại component một cách an toàn kiểu.

- **Trong SvelteKit:** `let variable: type = initialValue;`

  ```svelte
  <script lang="ts">
    let count: number = 0;
    let name: string | null = null; // Union type: string hoặc null
    let user: { id: number; name: string } | undefined = undefined; // Hoặc undefined

    function increment() {
      count++;
    }
    function setName() {
      name = "Svelte";
    }
     function setUser() {
         user = { id: 1, name: "Admin" };
     }
  </script>
  <!-- ... -->
  ```

- **Trong React:** Dùng Hook `useState<Type>(initialValue)`.

  ```typescript jsx
  // StateDemo.tsx
  import React, { useState } from "react";

  interface User {
    id: number;
    name: string;
  }

  function StateDemo(): JSX.Element {
    // 1. Type Inference (Suy luận kiểu): TypeScript tự động biết `count` là number
    const [count, setCount] = useState(0);
    // setCount('abc'); // Lỗi!

    // 2. Khai báo kiểu rõ ràng (Explicit Type): Cần thiết cho union types, null, undefined, hoặc khi giá trị khởi tạo là null/undefined
    const [name, setName] = useState<string | null>(null);
    const [user, setUser] = useState<User | undefined>(undefined);
    const [items, setItems] = useState<string[]>([]); // Ví dụ với mảng

    function increment() {
      setCount((prev) => prev + 1); // Dùng callback để cập nhật an toàn
    }
    function assignName() {
      setName("React");
      // setName(123); // Lỗi!
    }
    function assignUser() {
      setUser({ id: 1, name: "Admin" });
      // setUser({ id: 2 }); // Lỗi! Thiếu thuộc tính 'name'
    }
    function addItem() {
      setItems((prevItems) => [...prevItems, `Item ${prevItems.length + 1}`]);
      // setItems(1); // Lỗi!
    }

    return (
      <div>
        <p>Count: {count}</p>
        <button onClick={increment}>Increment</button>
        <p>Name: {name ?? "Chưa có tên"}</p>{" "}
        {/* Dùng ?? (nullish coalescing) */}
        <button onClick={assignName}>Set Name</button>
        <p>User: {user ? `${user.name} (ID: ${user.id})` : "Chưa có user"}</p>
        <button onClick={assignUser}>Set User</button>
        <p>Items:</p>
        <ul>
          {items.map((item) => (
            <li key={item}>{item}</li>
          ))}{" "}
          {/* Render list (sẽ nói kỹ) */}
        </ul>
        <button onClick={addItem}>Add Item</button>
      </div>
    );
  }

  export default StateDemo;
  ```

**So sánh State & TypeScript:**

- **Svelte:** Khai báo biến với `let` và kiểu. Đơn giản.
- **React:** `useState<Type>(initial)`. Type inference hoạt động tốt cho các kiểu cơ bản. Cần chỉ định `<Type>` rõ ràng cho các trường hợp phức tạp hơn (union, object/array khởi tạo rỗng/null/undefined). Luôn dùng hàm `setter` để cập nhật state.

---

**4. Event Handling Với TypeScript**

Định nghĩa kiểu cho các hàm xử lý sự kiện.

- **Trong SvelteKit:** Type `event` trong handler.

  ```svelte
  <script lang="ts">
    let inputValue = '';

    // Hover để xem kiểu của event (thường là Event, MouseEvent, KeyboardEvent, etc.)
    function handleClick(event: MouseEvent) {
      console.log('Button clicked!', event.clientX, event.clientY);
      // event.target là EventTarget, cần kiểm tra kiểu nếu muốn truy cập thuộc tính cụ thể
       if (event.currentTarget instanceof HTMLButtonElement) {
          console.log(event.currentTarget.textContent);
       }
    }

    function handleInput(event: Event) {
        // event.target có thể là nhiều thứ, cần kiểm tra hoặc ép kiểu (type assertion)
        const target = event.target as HTMLInputElement;
        inputValue = target.value;
        console.log('Input value:', inputValue);
    }
  </script>

  <button on:click={handleClick}>Click Me</button>
  <input type="text" value={inputValue} on:input={handleInput} placeholder="Nhập gì đó..." />
  ```

- **Trong React:** Sử dụng các kiểu sự kiện cụ thể từ `React` (`React.MouseEvent`, `React.ChangeEvent`, `React.FormEvent`, etc.).

  ```typescript jsx
  // EventDemo.tsx
  import React, { useState, MouseEvent, ChangeEvent, FormEvent } from "react";

  function EventDemo(): JSX.Element {
    const [inputValue, setInputValue] = useState<string>("");
    const [submittedValue, setSubmittedValue] = useState<string>("");

    // Kiểu cho sự kiện click chuột vào button
    function handleClick(event: MouseEvent<HTMLButtonElement>) {
      console.log("Button clicked!", event.clientX, event.clientY);
      // event.currentTarget là phần tử gắn event listener (button) và có kiểu chính xác
      console.log("Button text:", event.currentTarget.textContent);
      // event.target có thể là phần tử con bên trong button nếu có
    }

    // Kiểu cho sự kiện thay đổi giá trị của input
    function handleInputChange(event: ChangeEvent<HTMLInputElement>) {
      // event.target có kiểu chính xác là HTMLInputElement
      const newValue = event.target.value;
      setInputValue(newValue);
      console.log("Input value:", newValue);
    }

    // Kiểu cho sự kiện submit form
    function handleSubmit(event: FormEvent<HTMLFormElement>) {
      event.preventDefault(); // Ngăn chặn hành vi mặc định của form (tải lại trang)
      console.log("Form submitted with value:", inputValue);
      setSubmittedValue(inputValue);
    }

    return (
      <form onSubmit={handleSubmit}>
        {" "}
        {/* Gắn onSubmit vào form */}
        <button type="button" onClick={handleClick}>
          {" "}
          {/* type="button" để không submit form */}
          Click Me
        </button>
        <input
          type="text"
          onChange={handleInputChange}
          value={inputValue} // Two-way binding "thủ công"
          placeholder="Nhập và Enter để submit..."
        />
        <button type="submit">Submit</button>{" "}
        {/* Button này sẽ trigger onSubmit của form */}
        {submittedValue && <p>Đã submit: {submittedValue}</p>}
      </form>
    );
  }

  export default EventDemo;
  ```

**So sánh Event Handling & TypeScript:**

- **Svelte:** Cung cấp kiểu cơ bản cho `event`. Thường cần tự kiểm tra/ép kiểu `event.target` hoặc `event.currentTarget`.
- **React:** Cung cấp các kiểu sự kiện **generic** rất chi tiết (`React.MouseEvent<ElementType>`, `React.ChangeEvent<ElementType>`). `event.currentTarget` thường có kiểu chính xác của phần tử bạn gắn listener vào, giúp truy cập thuộc tính an toàn hơn. Cần nhớ `preventDefault()` thủ công khi cần (ví dụ: form submit).

---

**5. Conditional Rendering & Rendering Lists Với TypeScript**

Cách dùng TypeScript với các cấu trúc điều khiển luồng hiển thị. Logic cơ bản giống JavaScript, nhưng được type-check.

- **Trong SvelteKit:**

  ```svelte
  <script lang="ts">
    let isLoggedIn: boolean = false;
    type UserRole = 'admin' | 'editor' | 'guest';
    let role: UserRole = 'guest';

    interface Product {
        id: string;
        name: string;
        price: number;
    }
    const products: Product[] = [
      { id: 'p1', name: 'Laptop', price: 1200 },
      { id: 'p2', name: 'Mouse', price: 25 },
    ];
  </script>

  {#if isLoggedIn} <p>Welcome!</p> {:else} <button>Login</button> {/if}

  {#if role === 'admin'} <p>Admin Panel</p> {:else if role === 'editor'} <p>Editor Tools</p> {:else} <p>Guest</p> {/if}

  <ul>
    {#each products as product (product.id)}
      <li>{product.name} - ${product.price}</li>
    {/each}
  </ul>
  ```

- **Trong React:**

  ```typescript jsx
  // ControlFlowDemo.tsx
  import React, { useState } from "react";

  type UserRole = "admin" | "editor" | "guest";

  interface Product {
    id: string;
    name: string;
    price: number;
  }

  function ControlFlowDemo(): JSX.Element {
    const [isLoggedIn, setIsLoggedIn] = useState<boolean>(false);
    const role: UserRole = "editor"; // Kiểu được kiểm tra
    // role = 'viewer'; // Lỗi TypeScript!

    const products: Product[] = [
      // Mảng có kiểu Product[]
      { id: "p1", name: "Laptop", price: 1200 },
      { id: "p2", name: "Mouse", price: 25 },
      // { id: 'p3', name: 'Keyboard' } // Lỗi! Thiếu price
    ];

    return (
      <div>
        {/* Conditional Rendering */}
        {isLoggedIn ? (
          <p>Welcome!</p>
        ) : (
          <button onClick={() => setIsLoggedIn(true)}>Login</button>
        )}

        {role === "admin" && <p>Admin Panel</p>}
        {role === "editor" && <p>Editor Tools</p>}
        {role === "guest" && <p>Guest</p>}

        {/* Rendering Lists */}
        <h2>Products</h2>
        <ul>
          {products.map(
            (
              product // `product` tự động có kiểu Product
            ) => (
              <li key={product.id}>
                {" "}
                {/* Key là bắt buộc! */}
                {product.name} - ${product.price}
                {/* {product.stock} */} {/* Lỗi! Property 'stock' does not exist on type 'Product'. */}
              </li>
            )
          )}
        </ul>
      </div>
    );
  }

  export default ControlFlowDemo;
  ```

**So sánh Control Flow & TypeScript:**

- Cả hai đều hưởng lợi từ type checking cho các biến điều kiện và dữ liệu trong mảng.
- **Svelte:** Cú pháp `{#if}`, `{#each}` chuyên dụng.
- **React:** Dùng toán tử logic (`&&`, `? :`) và `array.map()`. TypeScript giúp đảm bảo bạn đang truy cập đúng thuộc tính của `item` trong `map` và sử dụng đúng kiểu cho `key`.

---

**Tạm Kết Phần 1 (với TypeScript):**

Chúng ta đã đi qua các khối xây dựng cơ bản của React với sự trợ giúp đắc lực của TypeScript:

- **TSX:** Viết UI an toàn kiểu.
- **Functional Components:** Hàm trả về `JSX.Element`.
- **Props:** Định nghĩa bằng `interface` hoặc `type`, nhận qua destructuring với type checking.
- **State (`useState<Type>`):** Quản lý state với kiểu rõ ràng.
- **Event Handling (`React.SomeEvent<Element>`):** Xử lý sự kiện với kiểu cụ thể.
- **Conditional Rendering & Lists:** Tận dụng toán tử JS và `.map()` với type safety.

So với SvelteKit/TS, React/TS yêu cầu bạn phải "rõ ràng" hơn trong việc định nghĩa kiểu (đặc biệt là props và state phức tạp), nhưng bù lại cung cấp khả năng tích hợp type checking sâu hơn vào chính cấu trúc component và template.

---

**Tài liệu React & Next.js "Khủng Khiếp" với TypeScript (Phần 2/5): Hooks Nâng Cao & Vòng Đời Component**

---

**Giới thiệu:**

Ở Phần 1, chúng ta đã nắm vững `useState` để quản lý state cục bộ. Nhưng ứng dụng thực tế cần nhiều hơn thế: tương tác với API, quản lý state xuyên suốt ứng dụng, hay "chọc" vào DOM khi cần thiết. Đây là lúc các Hooks như `useEffect`, `useContext`, và `useRef` tỏa sáng. Chúng là công cụ mạnh mẽ để kiểm soát "vòng đời" và hành vi của component React.

---

**1. `useEffect`: Quản Lý Side Effects (Tác Vụ Phụ)**

Đây là một trong những Hook quan trọng và dễ gây nhầm lẫn nhất cho người mới. `useEffect` cho phép bạn thực hiện các _side effects_ trong functional components. Side effects là bất cứ thứ gì tương tác với "thế giới bên ngoài" component:

- Fetch dữ liệu từ API.
- Thiết lập và hủy bỏ các subscription (ví dụ: WebSocket, listeners).
- Thiết lập và hủy bỏ timers (`setTimeout`, `setInterval`).
- Thao tác trực tiếp với DOM (mặc dù nên hạn chế, thường dùng `useRef` tốt hơn).
- Ghi log, lưu vào Local Storage...

**Tư Duy:** Hãy nghĩ `useEffect` như một cách để đồng bộ component của bạn với một hệ thống bên ngoài.

- **Trong SvelteKit:** Bạn thường dùng `onMount` để chạy code khi component được gắn vào DOM, `onDestroy` để dọn dẹp, và các khối `$: { ... }` (reactive statements) để chạy code khi một hoặc nhiều biến phụ thuộc thay đổi. `useEffect` trong React bao hàm _tất cả_ các trường hợp này, tùy thuộc vào cách bạn cấu hình nó.

- **Trong React:**

  ```typescript jsx
  import React, { useState, useEffect } from "react";

  interface Post {
    userId: number;
    id: number;
    title: string;
    body: string;
  }

  function EffectDemo(): JSX.Element {
    const [count, setCount] = useState<number>(0);
    const [postId, setPostId] = useState<number>(1); // State để trigger fetch lại post khác
    const [post, setPost] = useState<Post | null>(null);
    const [isLoading, setIsLoading] = useState<boolean>(false);
    const [error, setError] = useState<string | null>(null);

    // --- Ví dụ 1: Chạy một lần sau khi component mount (Tương đương onMount) ---
    useEffect(() => {
      console.log("EffectDemo Component Mounted!");
      document.title = `You clicked ${count} times`; // Ví dụ side effect: Cập nhật title trang

      // Cleanup function: Chạy khi component unmount (Tương đương onDestroy)
      return () => {
        console.log("EffectDemo Component Unmounting! Cleanup...");
        // Ví dụ: Hủy bỏ subscription, timer... nếu có
      };
    }, []); // <-- Dependency array rỗng [], chỉ chạy 1 lần sau mount

    // --- Ví dụ 2: Chạy sau mỗi lần render (Ít dùng, cẩn thận vòng lặp vô hạn!) ---
    // useEffect(() => {
    //   console.log('Component Rendered (runs on every render)');
    // }); // <-- Không có dependency array

    // --- Ví dụ 3: Chạy khi `count` thay đổi (Tương đương $: if (count) { ... } ) ---
    useEffect(() => {
      console.log(`Count changed to: ${count}`);
      document.title = `You clicked ${count} times`; // Cập nhật title khi count thay đổi

      // Có thể có cleanup riêng cho effect này nếu cần
      // return () => { console.log('Cleanup for count effect'); };
    }, [count]); // <-- Dependency: [count]

    // --- Ví dụ 4: Fetch dữ liệu khi `postId` thay đổi ---
    useEffect(() => {
      console.log(`Fetching post with ID: ${postId}`);
      setIsLoading(true);
      setError(null);
      setPost(null); // Xóa post cũ khi bắt đầu fetch mới

      const controller = new AbortController(); // API để hủy fetch nếu component unmount hoặc postId thay đổi nhanh
      const signal = controller.signal;

      fetch(`https://jsonplaceholder.typicode.com/posts/${postId}`, { signal })
        .then((response) => {
          if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
          }
          return response.json();
        })
        .then((data: Post) => {
          setPost(data);
          setIsLoading(false);
        })
        .catch((err) => {
          if (err.name === "AbortError") {
            console.log("Fetch aborted");
          } else {
            console.error("Fetch error:", err);
            setError("Failed to fetch post.");
            setIsLoading(false);
          }
        });

      // Cleanup function: Hủy fetch nếu component unmount HOẶC postId thay đổi trước khi fetch xong
      return () => {
        console.log(`Cleanup fetch for postId: ${postId}`);
        controller.abort();
      };
    }, [postId]); // <-- Dependency: [postId]

    return (
      <div>
        <h2>useEffect Demo</h2>
        <p>You clicked {count} times</p>
        <button onClick={() => setCount((c) => c + 1)}>Click me</button>

        <hr />
        <h3>Fetch Post</h3>
        <button onClick={() => setPostId((id) => id + 1)} disabled={isLoading}>
          Fetch Next Post (ID: {postId + 1})
        </button>
        <button
          onClick={() => setPostId(1)}
          disabled={isLoading || postId === 1}
        >
          Fetch Post 1
        </button>

        {isLoading && <p>Loading post...</p>}
        {error && <p style={{ color: "red" }}>{error}</p>}
        {post && !isLoading && (
          <article>
            <h4>{post.title}</h4>
            <p>{post.body}</p>
          </article>
        )}
      </div>
    );
  }

  export default EffectDemo;
  ```

**Giải thích `useEffect(callback, dependencies)`:**

1.  **`callback`**: Hàm chứa logic side effect của bạn.
2.  **`dependencies` (mảng phụ thuộc - cực kỳ quan trọng!)**:
    - **`[]` (Mảng rỗng):** Callback chỉ chạy _một lần_ sau lần render đầu tiên (mount). Hàm cleanup (nếu có) sẽ chạy khi component bị hủy (unmount). _Tương đương `onMount` và `onDestroy` của Svelte._
    - **`[dep1, dep2, ...]` (Mảng có giá trị):** Callback chạy sau lần render đầu tiên _VÀ_ sau mỗi lần render tiếp theo _chỉ khi_ ít nhất một giá trị trong mảng `dependencies` thay đổi (so sánh bằng `Object.is`). Hàm cleanup chạy trước khi effect chạy lại lần tiếp theo (do dependency thay đổi) và khi component unmount. _Tương đương các reactive statements (`$:`) theo dõi `dep1`, `dep2`... và `onDestroy` trong Svelte._
    - **Không có mảng (bỏ qua tham số thứ 2):** Callback chạy sau _mỗi lần_ component render. **Rất nguy hiểm** nếu callback đó lại cập nhật state (gây render lại -> chạy effect -> cập nhật state -> ... vòng lặp vô hạn). Hạn chế tối đa việc không cung cấp mảng dependencies.

**Best Practices với `useEffect`:**

- **Luôn cung cấp mảng dependencies.** Nếu effect không phụ thuộc vào gì, dùng `[]`.
- **Chỉ đưa vào dependencies những giá trị (props, state, biến cục bộ) mà effect _thực sự_ sử dụng và có thể thay đổi.** ESLint plugin của React (`eslint-plugin-react-hooks`) sẽ giúp cảnh báo nếu bạn quên hoặc thêm thừa dependency.
- **Sử dụng hàm cleanup** để ngăn chặn memory leaks hoặc hành vi không mong muốn (hủy timer, abort fetch, gỡ bỏ event listener...).
- Nếu một hàm được gọi bên trong `useEffect` và hàm đó được định nghĩa bên ngoài `useEffect` (ví dụ: hàm từ props hoặc định nghĩa trong component), hãy đưa hàm đó vào dependency array hoặc sử dụng `useCallback` (sẽ nói ở phần sau) để ổn định tham chiếu của hàm.
- Đối với việc fetch dữ liệu, hãy xử lý trạng thái loading, error và cleanup (abort fetch).

**So sánh `useEffect` với Svelte Lifecycles/Reactivity:**

| React (`useEffect`)                           | Svelte Tương Đương                                             | Ghi Chú                                                               |
| :-------------------------------------------- | :------------------------------------------------------------- | :-------------------------------------------------------------------- |
| `useEffect(() => {...}, [])`                  | `onMount(() => {...})`                                         | Chạy 1 lần sau khi component gắn vào DOM.                             |
| `useEffect(() => { return cleanup; }, [])`    | `onMount(() => { return cleanup; })` hoặc `onDestroy(cleanup)` | Cleanup khi component bị hủy.                                         |
| `useEffect(() => {...}, [dep])`               | `$: if (dep) { ... }` hoặc `$: (... = dep, ...)`               | Chạy khi `dep` thay đổi.                                              |
| `useEffect(() => { return cleanup; }, [dep])` | Kết hợp `$: if (dep) { ... return cleanup; }` và `onDestroy`   | Cleanup trước lần chạy tiếp theo hoặc khi unmount.                    |
| (Không dependency array)                      | `afterUpdate(() => {...})` (gần giống)                         | Chạy sau mỗi lần render/update. Ít dùng và cần cẩn thận trong cả hai. |

`useEffect` linh hoạt hơn nhưng cũng yêu cầu bạn phải hiểu rõ về dependencies để tránh lỗi và tối ưu hiệu năng.

---

**2. `useContext`: Chia Sẻ State Xuyên Suốt Component (Global State)**

Khi ứng dụng lớn dần, việc truyền props qua nhiều cấp component ("prop drilling") trở nên phiền phức và khó bảo trì. `useContext` cung cấp một cách để chia sẻ dữ liệu (state, hàm, theme...) giữa các component mà không cần truyền props thủ công.

- **Trong SvelteKit:** Bạn thường dùng **Svelte Stores (`writable`, `readable`, `derived`)** để quản lý state toàn cục. Stores cực kỳ mạnh mẽ và dễ sử dụng. Svelte cũng có `getContext`/`setContext` hoạt động tương tự Context API của React nhưng ít phổ biến hơn Stores cho state động.

- **Trong React:** Context API bao gồm 3 bước:

  1.  **Tạo Context:** Dùng `React.createContext()` để tạo một đối tượng Context. Bạn nên cung cấp giá trị mặc định (có thể là `null` hoặc một giá trị khởi tạo) và định kiểu cho giá trị context.
  2.  **Cung cấp Context (Provider):** Bọc cây component cần truy cập context bằng `<MyContext.Provider value={yourValue}>`. `yourValue` là dữ liệu bạn muốn chia sẻ. Bất cứ khi nào `yourValue` thay đổi, tất cả các component con sử dụng context này sẽ được render lại.
  3.  **Sử dụng Context (Consumer):** Trong component con, dùng Hook `useContext(MyContext)` để lấy giá trị hiện tại của context.

  ```typescript jsx
  // contexts/ThemeContext.tsx
  import React, { createContext, useState, useContext, ReactNode } from "react";

  type Theme = "light" | "dark";

  // Định nghĩa kiểu cho giá trị Context
  interface ThemeContextProps {
    theme: Theme;
    toggleTheme: () => void;
  }

  // Tạo Context với giá trị mặc định (có thể là undefined hoặc null và kiểm tra trong Provider/Consumer)
  // Ở đây ta cung cấp giá trị mặc định hợp lệ nhưng sẽ bị ghi đè bởi Provider
  const ThemeContext = createContext<ThemeContextProps>({
    theme: "light",
    toggleTheme: () => console.warn("no theme provider"), // Hàm mặc định để tránh lỗi runtime
  });

  // Tạo Provider component tùy chỉnh (Custom Provider - Best Practice)
  interface ThemeProviderProps {
    children: ReactNode; // Kiểu cho prop children
  }

  export function ThemeProvider({ children }: ThemeProviderProps): JSX.Element {
    const [theme, setTheme] = useState<Theme>("light");

    const toggleTheme = () => {
      setTheme((prevTheme) => (prevTheme === "light" ? "dark" : "light"));
    };

    // Giá trị được cung cấp cho context
    const value = { theme, toggleTheme };

    return (
      <ThemeContext.Provider value={value}>{children}</ThemeContext.Provider>
    );
  }

  // Tạo Hook tùy chỉnh để sử dụng Context (Custom Hook - Best Practice)
  export function useTheme(): ThemeContextProps {
    const context = useContext(ThemeContext);
    if (context === undefined) {
      // Lỗi này xảy ra nếu useTheme() được gọi bên ngoài một ThemeProvider
      throw new Error("useTheme must be used within a ThemeProvider");
    }
    return context;
  }
  ```

  ```typescript jsx
  // components/ThemedButton.tsx
  import React from "react";
  import { useTheme } from "../contexts/ThemeContext"; // Import custom hook

  function ThemedButton(): JSX.Element {
    // Sử dụng custom hook để lấy theme và hàm toggle
    const { theme, toggleTheme } = useTheme();

    const buttonStyle: React.CSSProperties = {
      backgroundColor: theme === "light" ? "#eee" : "#333",
      color: theme === "light" ? "#000" : "#fff",
      padding: "10px 20px",
      border: "none",
      cursor: "pointer",
      marginTop: "10px",
    };

    return (
      <button style={buttonStyle} onClick={toggleTheme}>
        Toggle Theme (Current: {theme})
      </button>
    );
  }

  export default ThemedButton;
  ```

  ```typescript jsx
  // App.tsx (hoặc một component cha nào đó)
  import React from "react";
  import { ThemeProvider } from "./contexts/ThemeContext"; // Import Provider
  import ThemedButton from "./components/ThemedButton";
  import SomeOtherComponent from "./components/SomeOtherComponent"; // Component khác cũng có thể dùng useTheme()

  function App(): JSX.Element {
    return (
      // Bọc các component cần dùng theme bằng ThemeProvider
      <ThemeProvider>
        <div>
          <h1>My App with Theme Context</h1>
          <ThemedButton />
          <SomeOtherComponent />
          {/* Thêm các component khác ở đây */}
        </div>
      </ThemeProvider>
    );
  }

  // components/SomeOtherComponent.tsx (ví dụ)
  function SomeOtherComponent(): JSX.Element {
    const { theme } = useTheme(); // Cũng có thể truy cập theme
    return (
      <p style={{ color: theme === "dark" ? "cyan" : "blue" }}>
        Another component using the theme.
      </p>
    );
  }

  export default App;
  ```

**Best Practices với `useContext`:**

- **Tạo Custom Provider và Custom Hook:** Như ví dụ `ThemeProvider` và `useTheme()`. Điều này giúp đóng gói logic khởi tạo, quản lý state (nếu cần) và cung cấp API sử dụng context đơn giản, dễ hiểu hơn. Nó cũng giúp ẩn chi tiết về `createContext` và `useContext` khỏi người dùng context.
- **Định kiểu rõ ràng:** Dùng TypeScript `interface` hoặc `type` cho giá trị context.
- **Tách biệt Context:** Tạo các context khác nhau cho các mối quan tâm khác nhau (ví dụ: `ThemeContext`, `AuthContext`, `LocaleContext`) thay vì nhồi nhét mọi thứ vào một context lớn.
- **Tối ưu hiệu năng:** Context làm cho tất cả consumer bị render lại mỗi khi `value` của Provider thay đổi. Nếu `value` là một object hoặc array được tạo inline (`value={{ theme, toggleTheme }}`), nó sẽ tạo tham chiếu mới mỗi lần Provider render, gây render lại không cần thiết cho consumer.
  - Cách khắc phục: Đảm bảo `value` chỉ thay đổi khi cần thiết. Dùng `useState` trong Provider (như ví dụ) là cách phổ biến. Đối với các hàm (như `toggleTheme`), sử dụng `useCallback` (sẽ nói sau) để ổn định tham chiếu của hàm nếu cần. Hoặc sử dụng `useMemo` (sẽ nói sau) để ghi nhớ giá trị object/array.

**So sánh `useContext` với Svelte Stores:**

- **Mục đích:** Cả hai đều giải quyết vấn đề chia sẻ state/giá trị.
- **Độ phức tạp:** Svelte Stores thường được coi là đơn giản và trực quan hơn cho state động toàn cục. Context API của React đòi hỏi nhiều bước thiết lập hơn (create, provide, consume).
- **Tích hợp:** `useContext` là một Hook tích hợp sẵn của React, hoạt động tự nhiên với luồng render của React. Svelte Stores là một hệ thống riêng biệt nhưng tích hợp rất tốt với reactivity của Svelte (`$storeName`).
- **Trường hợp sử dụng:**
  - **`useContext`:** Thường dùng cho dữ liệu "bán tĩnh" (ít thay đổi) như theme, locale, thông tin user đăng nhập, hoặc để truyền các hàm điều khiển xuống sâu trong cây component. Cũng hay kết hợp với `useReducer` (sẽ nói sau) để quản lý state phức tạp hơn.
  - **Svelte Stores:** Rất linh hoạt cho cả state tĩnh và động, dễ dàng tạo derived stores, xử lý side effects liên quan đến store. Là lựa chọn hàng đầu cho global state trong Svelte.

---

**3. `useRef`: Truy Cập DOM & Lưu Trữ Giá Trị Không Gây Re-render**

`useRef` là một Hook đa năng với hai mục đích chính:

1.  **Truy cập trực tiếp các phần tử DOM:** Lấy tham chiếu đến một thẻ HTML (ví dụ: `<input>`, `<div>`, `<canvas>`) để thực hiện các thao tác như focus, đo kích thước, gọi các phương thức của DOM element.
2.  **Lưu trữ một giá trị _có thể thay đổi_ (mutable) mà _không_ gây ra re-render khi giá trị đó thay đổi.** Điều này khác biệt hoàn toàn với `useState`. Hữu ích để lưu ID của timer/interval, giá trị trước đó của state/props, hoặc bất kỳ dữ liệu nào bạn muốn giữ lại giữa các lần render mà không muốn việc thay đổi nó làm component render lại.

- **Trong SvelteKit:**

  - Để truy cập DOM, bạn dùng directive `bind:this={elementVariable}`.
  - Để lưu giá trị không gây re-render, bạn có thể khai báo biến `let` thông thường trong `<script>`. Tuy nhiên, nếu biến này được sử dụng trong template hoặc reactive statement (`$:`) thì việc thay đổi nó _vẫn có thể_ trigger update. `useRef` đảm bảo không bao giờ trigger re-render.

- **Trong React:**

  ```typescript jsx
  // RefDemo.tsx
  import React, { useState, useEffect, useRef } from "react";

  function RefDemo(): JSX.Element {
    const [inputValue, setInputValue] = useState<string>("");

    // --- Ví dụ 1: Truy cập DOM Element (focus input) ---
    // Khởi tạo ref với kiểu là HTMLInputElement hoặc null
    const inputRef = useRef<HTMLInputElement>(null);

    useEffect(() => {
      // Focus vào input khi component mount
      // inputRef.current có thể là null ban đầu, nên cần kiểm tra
      inputRef.current?.focus(); // Dùng optional chaining (?.)
    }, []); // Chạy 1 lần sau mount

    const handleFocusClick = () => {
      inputRef.current?.focus(); // Focus khi nhấn nút
    };

    // --- Ví dụ 2: Lưu giá trị không gây re-render (Interval ID) ---
    const intervalRef = useRef<number | null>(null); // Lưu ID của setInterval
    const [timerCount, setTimerCount] = useState<number>(0);

    useEffect(() => {
      // Bắt đầu interval khi mount
      intervalRef.current = window.setInterval(() => {
        // Lưu ý: Dùng callback trong setTimerCount để lấy giá trị mới nhất mà không cần đưa timerCount vào dependency array của useEffect này
        setTimerCount((prevCount) => prevCount + 1);
      }, 1000);
      console.log("Interval Started, ID:", intervalRef.current);

      // Cleanup: Dọn dẹp interval khi unmount
      return () => {
        if (intervalRef.current !== null) {
          console.log("Clearing Interval, ID:", intervalRef.current);
          clearInterval(intervalRef.current);
          intervalRef.current = null; // Reset ref
        }
      };
    }, []); // Chỉ chạy 1 lần

    // --- Ví dụ 3: Lưu giá trị trước đó của state ---
    const previousInputValue = useRef<string>("");

    useEffect(() => {
      // Cập nhật giá trị trước đó *sau* mỗi lần render (khi inputValue đã thay đổi)
      previousInputValue.current = inputValue;
      console.log("Updating previous value ref");
    }, [inputValue]); // Chạy mỗi khi inputValue thay đổi

    return (
      <div>
        <h2>useRef Demo</h2>

        {/* Ví dụ 1: Focus Input */}
        <input
          ref={inputRef} // Gắn ref vào input element
          type="text"
          value={inputValue}
          onChange={(e) => setInputValue(e.target.value)}
          placeholder="I will be focused on mount"
        />
        <button onClick={handleFocusClick}>Focus Input Manually</button>
        <p>Current Value: {inputValue}</p>
        <p>Previous Value (from ref): {previousInputValue.current}</p>

        <hr />

        {/* Ví dụ 2: Timer */}
        <p>Timer Count: {timerCount}</p>
        {/* Nút dừng timer (ví dụ) */}
        <button
          onClick={() => {
            if (intervalRef.current !== null) {
              clearInterval(intervalRef.current);
              console.log(
                "Stopped Interval Manually, ID:",
                intervalRef.current
              );
              intervalRef.current = null;
            }
          }}
        >
          Stop Timer
        </button>
      </div>
    );
  }

  export default RefDemo;
  ```

**Cách `useRef` hoạt động:**

- `useRef<Type>(initialValue)` trả về một **object** có dạng `{ current: initialValue }`.
- Bạn có thể đọc và **thay đổi** giá trị của `myRef.current` một cách trực tiếp.
- **Quan trọng:** Việc thay đổi `myRef.current` **KHÔNG** làm component re-render.
- Giá trị của `myRef.current` được giữ nguyên giữa các lần render (giống như state).
- Khi dùng với DOM element (`ref={inputRef}`), React sẽ tự động gán DOM node vào `inputRef.current` sau khi component mount và gán `null` khi unmount.

**So sánh `useRef` với Svelte:**

- **DOM Access:** `useRef` + `ref` attribute ≈ `bind:this={element}`. Cả hai đều cho phép truy cập trực tiếp DOM node.
- **Mutable Non-Reactive Value:** `myRef.current = newValue` ≈ `let myVar = newValue` (trong `<script>`). Điểm khác biệt chính là `useRef` đảm bảo không gây re-render, trong khi biến `let` của Svelte có thể gây re-render nếu nó được dùng ở những nơi "reactive". `useRef` rõ ràng hơn về ý định "lưu trữ giá trị không cần render lại".

---

**Tạm Kết Phần 2:**

Chúng ta đã tìm hiểu 3 Hooks cực kỳ quan trọng:

- **`useEffect`:** Xử lý side effects, tương đương với sự kết hợp của `onMount`, `onDestroy`, và reactive statements (`$:`) trong Svelte, dựa vào mảng dependencies. Cần hiểu rõ dependencies và cleanup.
- **`useContext`:** Chia sẻ state/giá trị toàn cục, tương tự Svelte Stores hoặc `getContext`/`setContext`, nhưng với cơ chế Provider/Consumer tích hợp vào React. Thường dùng cho dữ liệu bán tĩnh hoặc khi kết hợp với các state management phức tạp hơn.
- **`useRef`:** Truy cập DOM elements (giống `bind:this`) và lưu trữ giá trị mutable không gây re-render (rõ ràng hơn biến `let` thông thường trong Svelte).

Việc nắm vững các Hooks này là chìa khóa để xây dựng các component React phức tạp và hiệu quả. Bạn sẽ thấy chúng được sử dụng rất nhiều trong các ứng dụng thực tế.

---

**Tài liệu React & Next.js "Khủng Khiếp" với TypeScript (Phần 3/5): Tối Ưu Hóa, Forms & Giới Thiệu Routing SPA**

---

**Giới thiệu:**

Khi ứng dụng React của bạn phát triển về quy mô và độ phức tạp, hiệu năng trở thành một yếu tố quan trọng. React sử dụng Virtual DOM và cơ chế diffing khá hiệu quả, nhưng việc render lại không cần thiết vẫn có thể xảy ra, ảnh hưởng đến trải nghiệm người dùng. Phần này sẽ giới thiệu các công cụ React cung cấp để tối ưu hóa (`React.memo`, `useCallback`, `useMemo`), cách xử lý form hiệu quả, và cuối cùng là giới thiệu về routing trong mô hình Single Page Application (SPA) sử dụng thư viện phổ biến React Router.

---

**1. Tối Ưu Hóa Hiệu Năng Render**

- **Vấn đề:** Trong React, khi một component cha render lại (do state hoặc props của nó thay đổi), _tất cả_ các component con của nó cũng sẽ mặc định bị render lại, ngay cả khi props truyền cho chúng không hề thay đổi. Điều này có thể lãng phí tài nguyên, đặc biệt với các component phức tạp hoặc danh sách dài.
- **So sánh với Svelte:** Svelte có cách tiếp cận khác. Nhờ vào compiler, Svelte thường chỉ cập nhật chính xác những phần DOM bị ảnh hưởng bởi sự thay đổi state, thay vì render lại toàn bộ component con. Điều này làm cho Svelte thường có hiệu năng tốt "out-of-the-box" cho nhiều trường hợp mà không cần tối ưu hóa thủ công nhiều như React. Tuy nhiên, React cung cấp các công cụ để bạn kiểm soát việc này một cách tường minh.

**a) `React.memo()` - Ngăn Re-render Component Con Không Cần Thiết**

`React.memo()` là một Higher-Order Component (HOC) - một hàm nhận vào một component và trả về một component mới đã được tối ưu. Component được bọc bởi `memo()` sẽ chỉ re-render nếu props của nó thay đổi (so sánh nông - shallow comparison).

```typescript jsx
// ChildComponent.tsx
import React from "react";

interface ChildProps {
  message: string;
  // Nếu có prop là object hoặc function, xem xét useCallback/useMemo ở dưới
  userInfo: { name: string };
}

// Component con này sẽ re-render mỗi khi ParentComponent re-render, ngay cả khi props 'message' và 'userInfo' không đổi tham chiếu
function ChildComponent({ message, userInfo }: ChildProps): JSX.Element {
  console.log(
    `Rendering ChildComponent with message: ${message}, user: ${userInfo.name}`
  );
  return (
    <p>
      {message} - User: {userInfo.name}
    </p>
  );
}

// Component con được bọc bởi React.memo
// Nó chỉ re-render nếu 'message' thay đổi HOẶC tham chiếu của 'userInfo' thay đổi
const MemoizedChildComponent = React.memo(function ChildComponentMemo({
  message,
  userInfo,
}: ChildProps): JSX.Element {
  console.log(
    `Rendering MemoizedChildComponent with message: ${message}, user: ${userInfo.name}`
  );
  return (
    <p>
      (Memoized) {message} - User: {userInfo.name}
    </p>
  );
});
// Có thể viết gọn: export default React.memo(ChildComponent); nếu export trực tiếp

// ParentComponent.tsx
import React, { useState, useMemo } from "react";
// Giả sử đã import ChildComponent và MemoizedChildComponent ở trên

function ParentComponent(): JSX.Element {
  const [count, setCount] = useState(0);
  const [text, setText] = useState("");

  // !!! Vấn đề: userInfo này sẽ được tạo MỚI mỗi lần ParentComponent render
  // const userInfo = { name: 'Alice' };
  // Điều này sẽ làm MemoizedChildComponent re-render ngay cả khi name không đổi, vì tham chiếu object đã khác

  // --- Giải pháp: Dùng useMemo để memoize object userInfo ---
  const userInfo = useMemo(() => ({ name: "Alice" }), []); // Chỉ tạo object 1 lần

  console.log("Rendering ParentComponent");

  return (
    <div>
      <h2>Parent Component</h2>
      <button onClick={() => setCount((c) => c + 1)}>
        Increment Count ({count})
      </button>
      <input
        type="text"
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="Type something..."
      />

      <hr />
      <h3>Normal Child:</h3>
      {/* Child này luôn re-render khi Parent re-render (do count hoặc text thay đổi) */}
      <ChildComponent message="Static Message" userInfo={userInfo} />

      <h3>Memoized Child:</h3>
      {/* Child này CHỈ re-render khi `userInfo` thay đổi tham chiếu (nhờ useMemo nên nó ổn định) */}
      {/* Nó sẽ KHÔNG re-render khi chỉ có `count` hoặc `text` của Parent thay đổi */}
      <MemoizedChildComponent message="Static Message" userInfo={userInfo} />
    </div>
  );
}

export default ParentComponent;
```

- **Lưu ý:** `React.memo()` thực hiện so sánh nông (shallow comparison) các props. Nếu bạn truyền object hoặc array làm prop, nó chỉ kiểm tra xem tham chiếu có thay đổi hay không, chứ không đi sâu vào bên trong. Nếu object/array được tạo mới mỗi lần render cha (như ví dụ `userInfo` bị comment), `memo()` sẽ không hiệu quả. Đây là lúc `useMemo` và `useCallback` phát huy tác dụng.

**b) `useCallback()` - Ghi Nhớ (Memoize) Hàm Callback**

Khi bạn truyền một hàm callback từ component cha xuống component con (đặc biệt là con đã được `memo()`), nếu hàm đó được định nghĩa trực tiếp trong thân hàm cha, nó sẽ được tạo mới mỗi lần cha render. Điều này làm thay đổi prop callback và khiến `memo()` của con mất tác dụng. `useCallback()` dùng để giải quyết vấn đề này.

`useCallback(fn, dependencies)` trả về một phiên bản được ghi nhớ của hàm `fn`. Hàm này chỉ thay đổi nếu một trong các `dependencies` thay đổi.

```typescript jsx
// CallbackDemo.tsx
import React, { useState, useCallback } from "react";

interface ButtonProps {
  onClick: () => void; // Prop là một hàm
  label: string;
}

// Component Button được memoize
const MemoizedButton = React.memo(({ onClick, label }: ButtonProps) => {
  console.log(`Rendering Button: ${label}`);
  return <button onClick={onClick}>{label}</button>;
});

function CallbackDemo(): JSX.Element {
  const [count, setCount] = useState(0);
  const [otherState, setOtherState] = useState(0); // State khác để trigger re-render

  // Vấn đề: Hàm này được tạo mới mỗi lần CallbackDemo render
  // const handleIncrement = () => {
  //   setCount(count + 1); // Phụ thuộc vào 'count'
  // };

  // Giải pháp: Dùng useCallback
  const handleIncrement = useCallback(() => {
    // Cách 1: Đưa 'count' vào dependencies
    // setCount(count + 1); // Làm hàm này thay đổi mỗi khi count thay đổi
    // }, [count]);

    // Cách 2 (Tốt hơn): Dùng dạng callback của setter để không cần phụ thuộc vào 'count'
    setCount((prevCount) => prevCount + 1);
  }, []); // Dependencies rỗng, hàm này sẽ không bao giờ thay đổi tham chiếu

  // Hàm này không liên quan đến count, dùng useCallback cũng tốt nếu truyền xuống con memoized
  const handleOtherAction = useCallback(() => {
    console.log("Performing other action...");
    // Không cần dependencies nếu không dùng state/props nào
  }, []);

  console.log("Rendering CallbackDemo");

  return (
    <div>
      <h2>useCallback Demo</h2>
      <p>Count: {count}</p>
      <p>Other State: {otherState}</p>
      <button onClick={() => setOtherState((s) => s + 1)}>
        Update Other State (triggers re-render)
      </button>

      <hr />
      {/*
        Nếu dùng handleIncrement phiên bản không useCallback:
        Mỗi lần nhấn "Update Other State", CallbackDemo re-render, handleIncrement mới được tạo,
        và MemoizedButton('Increment') cũng sẽ re-render dù prop 'label' không đổi.

        Nếu dùng handleIncrement phiên bản useCallback với []:
        Nhấn "Update Other State", CallbackDemo re-render, nhưng handleIncrement được trả về từ useCallback
        vẫn giữ nguyên tham chiếu cũ. MemoizedButton('Increment') sẽ KHÔNG re-render.
      */}
      <MemoizedButton onClick={handleIncrement} label="Increment" />
      <MemoizedButton onClick={handleOtherAction} label="Other Action" />
    </div>
  );
}

export default CallbackDemo;
```

- **Khi nào dùng `useCallback`?** Chủ yếu khi bạn truyền hàm callback xuống component con đã được tối ưu bằng `React.memo()` và muốn tránh việc hàm callback đó gây re-render không cần thiết cho con.

**c) `useMemo()` - Ghi Nhớ (Memoize) Giá Trị Tính Toán**

`useMemo()` dùng để ghi nhớ kết quả của một phép tính tốn kém. Hàm tính toán chỉ được gọi lại khi một trong các dependencies thay đổi.

`useMemo(() => computeExpensiveValue(a, b), [a, b])`

```typescript jsx
import React, { useState, useMemo } from "react";

// Hàm giả lập tính toán tốn kém
function calculateExpensiveValue(num: number): number {
  console.log("Calculating expensive value...");
  let result = 0;
  for (let i = 0; i < num * 100000000; i++) {
    result += Math.random();
  }
  return result;
}

function MemoDemo(): JSX.Element {
  const [count, setCount] = useState(0);
  const [numberInput, setNumberInput] = useState(5); // Đầu vào cho phép tính

  // Vấn đề: calculateExpensiveValue sẽ chạy mỗi lần component re-render (kể cả khi chỉ 'count' thay đổi)
  // const expensiveValue = calculateExpensiveValue(numberInput);

  // Giải pháp: Dùng useMemo
  const expensiveValue = useMemo(() => {
    return calculateExpensiveValue(numberInput);
  }, [numberInput]); // Chỉ tính toán lại khi 'numberInput' thay đổi

  console.log("Rendering MemoDemo");

  return (
    <div>
      <h2>useMemo Demo</h2>
      <button onClick={() => setCount((c) => c + 1)}>
        Increment Count ({count})
      </button>
      <p>Count change should NOT re-trigger calculation.</p>

      <hr />
      <label>
        Input for expensive calculation:
        <input
          type="number"
          value={numberInput}
          onChange={(e) => setNumberInput(parseInt(e.target.value, 10) || 0)}
        />
      </label>
      <p>Expensive Value: {expensiveValue}</p>
      <p>Changing the input WILL re-trigger calculation.</p>
    </div>
  );
}

export default MemoDemo;
```

- **Khi nào dùng `useMemo`?**
  - Khi bạn có một phép tính tốn nhiều thời gian/tài nguyên và không muốn nó chạy lại mỗi lần render nếu các đầu vào không đổi.
  - Khi bạn cần tạo và truyền một object hoặc array xuống component con được `memo()` mà muốn đảm bảo tham chiếu của nó ổn định nếu dữ liệu bên trong không đổi (như ví dụ `userInfo` ở phần `React.memo`).
- **So sánh với Svelte:** `useMemo` có thể coi là tương đương với việc sử dụng derived stores (`derived(...)`) hoặc reactive statements (`$: derivedValue = compute(dependency)`) để chỉ tính toán lại khi dependency thay đổi.

**Cảnh báo:** Đừng lạm dụng `React.memo`, `useCallback`, `useMemo`. Việc ghi nhớ bản thân nó cũng tốn chi phí (bộ nhớ, so sánh dependencies). Chỉ sử dụng chúng khi bạn xác định được có vấn đề về hiệu năng và việc tối ưu hóa mang lại lợi ích rõ rệt. Đo lường trước và sau khi tối ưu!

---

**2. Xử lý Form trong React (với TypeScript)**

Xử lý form trong React khác biệt đáng kể so với sự tiện lợi của `bind:value` trong Svelte. Cách phổ biến nhất là sử dụng **Controlled Components**.

**Controlled Components:**

Ý tưởng là component React sẽ "kiểm soát" trạng thái của các phần tử form (input, textarea, select) thông qua state.

1.  Dùng `useState` để lưu trữ giá trị của từng input field.
2.  Gán giá trị state đó vào thuộc tính `value` (hoặc `checked` cho checkbox/radio) của input.
3.  Sử dụng sự kiện `onChange` để cập nhật state mỗi khi người dùng thay đổi giá trị input.

```typescript jsx
// ControlledForm.tsx
import React, { useState, ChangeEvent, FormEvent } from "react";

interface FormData {
  username: string;
  email: string;
  subscribe: boolean;
  gender: "male" | "female" | "other" | ""; // Union type cho radio
  country: string; // Cho select
}

function ControlledForm(): JSX.Element {
  const [formData, setFormData] = useState<FormData>({
    username: "",
    email: "",
    subscribe: true,
    gender: "",
    country: "vietnam", // Giá trị mặc định cho select
  });

  const [errors, setErrors] = useState<Partial<Record<keyof FormData, string>>>(
    {}
  );
  const [submittedData, setSubmittedData] = useState<FormData | null>(null);

  // --- Generic Input Handler (Xử lý nhiều input bằng 1 hàm) ---
  const handleInputChange = (
    event: ChangeEvent<
      HTMLInputElement | HTMLSelectElement | HTMLTextAreaElement
    >
  ) => {
    const { name, value, type } = event.target;

    // Xử lý riêng cho checkbox
    const isCheckbox = type === "checkbox";
    // Cần ép kiểu vì checked chỉ có trên HTMLInputElement
    const checked = isCheckbox
      ? (event.target as HTMLInputElement).checked
      : undefined;

    setFormData((prevData) => ({
      ...prevData,
      [name]: isCheckbox ? checked : value, // Dùng computed property name
    }));

    // Xóa lỗi khi người dùng bắt đầu nhập lại
    if (errors[name as keyof FormData]) {
      setErrors((prev) => ({ ...prev, [name]: undefined }));
    }
  };

  // --- Validation Logic ---
  const validateForm = (): boolean => {
    const newErrors: Partial<Record<keyof FormData, string>> = {};
    if (!formData.username.trim()) {
      newErrors.username = "Username is required";
    }
    if (!formData.email.includes("@")) {
      newErrors.email = "Invalid email format";
    }
    if (!formData.gender) {
      newErrors.gender = "Please select a gender";
    }
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0; // true nếu không có lỗi
  };

  // --- Form Submission Handler ---
  const handleSubmit = (event: FormEvent<HTMLFormElement>) => {
    event.preventDefault(); // Ngăn submit mặc định
    console.log("Form Data Submitted:", formData);

    if (validateForm()) {
      console.log("Form is valid, submitting...");
      setSubmittedData(formData);
      // Ở đây bạn có thể gửi formData lên server
      // Reset form (tùy chọn)
      // setFormData({ username: '', email: '', subscribe: true, gender: '', country: 'vietnam' });
    } else {
      console.log("Form has errors.");
      setSubmittedData(null);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <h2>Controlled Form Demo</h2>

      {/* Username Input */}
      <div>
        <label htmlFor="username">Username:</label>
        <input
          type="text"
          id="username"
          name="username" // Quan trọng: name phải khớp với key trong state
          value={formData.username} // Giá trị từ state
          onChange={handleInputChange} // Cập nhật state
        />
        {errors.username && <p style={{ color: "red" }}>{errors.username}</p>}
      </div>

      {/* Email Input */}
      <div>
        <label htmlFor="email">Email:</label>
        <input
          type="email"
          id="email"
          name="email"
          value={formData.email}
          onChange={handleInputChange}
        />
        {errors.email && <p style={{ color: "red" }}>{errors.email}</p>}
      </div>

      {/* Checkbox Input */}
      <div>
        <label>
          <input
            type="checkbox"
            name="subscribe"
            checked={formData.subscribe} // Dùng 'checked'
            onChange={handleInputChange}
          />
          Subscribe to newsletter
        </label>
      </div>

      {/* Radio Buttons */}
      <fieldset>
        {" "}
        {/* Nhóm các radio lại */}
        <legend>Gender:</legend>
        <label>
          <input
            type="radio"
            name="gender"
            value="male"
            checked={formData.gender === "male"} // Kiểm tra giá trị khớp
            onChange={handleInputChange}
          />{" "}
          Male
        </label>
        <label>
          <input
            type="radio"
            name="gender"
            value="female"
            checked={formData.gender === "female"}
            onChange={handleInputChange}
          />{" "}
          Female
        </label>
        <label>
          <input
            type="radio"
            name="gender"
            value="other"
            checked={formData.gender === "other"}
            onChange={handleInputChange}
          />{" "}
          Other
        </label>
        {errors.gender && <p style={{ color: "red" }}>{errors.gender}</p>}
      </fieldset>

      {/* Select Dropdown */}
      <div>
        <label htmlFor="country">Country:</label>
        <select
          id="country"
          name="country"
          value={formData.country} // value của select là value của option được chọn
          onChange={handleInputChange}
        >
          <option value="vietnam">Việt Nam</option>
          <option value="usa">USA</option>
          <option value="japan">Japan</option>
        </select>
      </div>

      <button type="submit">Submit</button>

      {/* Hiển thị dữ liệu đã submit */}
      {submittedData && (
        <div
          style={{
            marginTop: "20px",
            border: "1px solid green",
            padding: "10px",
          }}
        >
          <h3>Submitted Data:</h3>
          <pre>{JSON.stringify(submittedData, null, 2)}</pre>
        </div>
      )}
    </form>
  );
}

export default ControlledForm;
```

**Ưu điểm Controlled Components:**

- Dữ liệu form luôn đồng bộ với state của React.
- Dễ dàng thực hiện validation ngay khi người dùng nhập liệu.
- Dễ dàng reset hoặc thay đổi giá trị form theo logic ứng dụng.

**Nhược điểm:**

- Code có thể dài dòng hơn, đặc biệt với form nhiều field.
- Mỗi lần gõ phím sẽ trigger `onChange` và re-render (thường không phải vấn đề lớn về hiệu năng trừ khi logic trong `onChange` quá phức tạp).

**Uncontrolled Components (Ít phổ biến hơn):**

Sử dụng `useRef` để lấy giá trị từ DOM khi cần thiết (thường là lúc submit). State không "kiểm soát" giá trị input trực tiếp.

```typescript jsx
// UncontrolledForm.tsx (Ví dụ đơn giản)
import React, { useRef, FormEvent } from "react";

function UncontrolledForm(): JSX.Element {
  const usernameRef = useRef<HTMLInputElement>(null);
  const emailRef = useRef<HTMLInputElement>(null);

  const handleSubmit = (event: FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    // Truy cập giá trị trực tiếp từ DOM node thông qua ref.current
    const username = usernameRef.current?.value;
    const email = emailRef.current?.value;
    console.log("Uncontrolled Submit:", { username, email });
    // Validation và xử lý tiếp theo...
  };

  return (
    <form onSubmit={handleSubmit}>
      <h2>Uncontrolled Form Demo</h2>
      <div>
        <label htmlFor="uncontrolled-username">Username:</label>
        {/* Không cần value và onChange */}
        <input type="text" id="uncontrolled-username" ref={usernameRef} />
      </div>
      <div>
        <label htmlFor="uncontrolled-email">Email:</label>
        <input type="email" id="uncontrolled-email" ref={emailRef} />
      </div>
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Thư viện quản lý Form:**

Với các form phức tạp (nhiều field, validation phức tạp, quản lý trạng thái submit, errors...), việc tự code bằng Controlled Components có thể rất tốn công. Các thư viện như **React Hook Form** (khuyến nghị, hiệu năng tốt, dựa trên Hook) hoặc **Formik** giúp đơn giản hóa việc này rất nhiều. Chúng thường kết hợp cả hai cách tiếp cận (controlled/uncontrolled) và cung cấp API mạnh mẽ.

---

**3. Giới Thiệu Routing trong React SPA (React Router)**

Khác với SvelteKit (và Next.js sắp tới), React bản thân nó _không_ cung cấp cơ chế routing. Để xây dựng một Single Page Application (SPA) - ứng dụng chỉ tải HTML/JS/CSS một lần và sau đó tự quản lý việc thay đổi nội dung dựa trên URL mà không cần tải lại trang - bạn cần một thư viện routing phía client. **React Router** là thư viện phổ biến nhất.

**Các khái niệm chính:**

1.  **`BrowserRouter`**: Component gốc, bọc toàn bộ ứng dụng của bạn. Nó sử dụng History API của trình duyệt để đồng bộ UI với URL. (Trong Next.js, bạn không cần dùng cái này vì Next.js có router riêng).
2.  **`Routes`**: Component chứa danh sách các route (`Route`) của bạn. Nó sẽ tìm và render `Route` đầu tiên khớp với URL hiện tại.
3.  **`Route`**: Định nghĩa một ánh xạ giữa một đường dẫn URL (`path`) và component (`element`) sẽ được render khi URL đó khớp.
4.  **`Link`**: Component dùng để tạo link điều hướng giữa các trang trong SPA. Nó render ra thẻ `<a>` nhưng xử lý việc chuyển trang bằng JavaScript mà không tải lại toàn bộ trang.
5.  **`useNavigate`**: Hook để điều hướng chương trình (ví dụ: sau khi submit form thành công).
6.  **`useParams`**: Hook để lấy các tham số động từ URL (ví dụ: `/users/:userId`).

**Ví dụ cơ bản với React Router v6 (phiên bản hiện đại):**

```bash
# Cài đặt react-router-dom
npm install react-router-dom
# hoặc
yarn add react-router-dom
```

```typescript jsx
// main.tsx (hoặc index.tsx) - Điểm khởi đầu ứng dụng
import React from "react";
import ReactDOM from "react-dom/client";
import { BrowserRouter } from "react-router-dom"; // Import BrowserRouter
import App from "./App"; // Component App chính chứa Routes
import "./index.css";

ReactDOM.createRoot(document.getElementById("root") as HTMLElement).render(
  <React.StrictMode>
    {/* Bọc App bằng BrowserRouter */}
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
);
```

```typescript jsx
// App.tsx - Định nghĩa layout và routes
import React from "react";
import {
  Routes,
  Route,
  Link,
  Outlet,
  useParams,
  useNavigate,
} from "react-router-dom";

// --- Các Component Trang ---
function HomePage() {
  const navigate = useNavigate(); // Hook để điều hướng

  const goToAbout = () => {
    navigate("/about"); // Điều hướng tới trang About
  };

  return (
    <div>
      <h2>Home Page</h2>
      <p>Welcome to our simple SPA!</p>
      <button onClick={goToAbout}>Go to About Page Programmatically</button>
    </div>
  );
}

function AboutPage() {
  return <h2>About Page</h2>;
}

interface UserParams {
  userId: string; // Tên param phải khớp với path=":userId"
  [key: string]: string | undefined; // Cho phép các params khác nếu có
}

function UserProfilePage() {
  // Lấy userId từ URL: /users/123 -> params.userId = "123"
  const params = useParams<UserParams>();
  // Lưu ý: params.userId luôn là string, cần chuyển đổi nếu là số
  const userId = params.userId;

  return (
    <div>
      <h2>User Profile</h2>
      <p>Displaying profile for User ID: {userId}</p>
    </div>
  );
}

function NotFoundPage() {
  return <h2>404 - Page Not Found</h2>;
}

// --- Layout Component (Optional) ---
// Chứa các phần tử chung như header, nav và render nội dung trang con qua <Outlet />
function Layout() {
  return (
    <div>
      <nav>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>{" "}
          {/* Dùng Link thay vì <a> */}
          <li>
            <Link to="/about">About</Link>
          </li>
          <li>
            <Link to="/users/123">User 123</Link>
          </li>
          <li>
            <Link to="/users/abc">User ABC</Link>
          </li>
          <li>
            <Link to="/non-existent-page">Broken Link</Link>
          </li>
        </ul>
      </nav>
      <hr />
      {/* Outlet là nơi component con tương ứng với route khớp sẽ được render */}
      <main>
        <Outlet />
      </main>
      <footer>
        <p>My SPA Footer</p>
      </footer>
    </div>
  );
}

// --- Component App chính ---
function App(): JSX.Element {
  return (
    // Routes định nghĩa các đường dẫn và component tương ứng
    <Routes>
      {/* Sử dụng Layout cho các route bên trong nó */}
      <Route path="/" element={<Layout />}>
        {/* Route con tương đối với path của cha */}
        <Route index element={<HomePage />} /> {/* index=true: route mặc định cho "/" */}
        <Route path="about" element={<AboutPage />} />
        <Route path="users/:userId" element={<UserProfilePage />} /> {/* :userId là param động */}
        {/* Route bắt tất cả các path không khớp khác (404) */}
        <Route path="*" element={<NotFoundPage />} />
      </Route>
      {/* Có thể có các route không dùng Layout ở đây */}
      {/* <Route path="/login" element={<LoginPage />} /> */}
    </Routes>
  );
}

export default App;
```

**Tại sao cần biết React Router?**

Mặc dù Next.js có hệ thống routing riêng dựa trên file system (giống SvelteKit), hiểu cách React Router hoạt động giúp bạn:

- Hiểu được các khái niệm routing cơ bản trong SPA.
- Nắm được các thành phần như `Link`, `useNavigate`, `useParams` vì Next.js cũng cung cấp các thành phần/hook tương tự (`next/link`, `next/navigation`).
- Hiểu được sự khác biệt và lợi ích của routing tích hợp sẵn trong framework (như Next.js/SvelteKit) so với thư viện bên ngoài.

---

**Tạm Kết Phần 3:**

Chúng ta đã đi qua các kỹ thuật tối ưu hóa quan trọng (`React.memo`, `useCallback`, `useMemo`), cách xử lý form phổ biến trong React (Controlled Components), và có cái nhìn đầu tiên về cách routing hoạt động trong một SPA React thuần túy với React Router.

Những kiến thức này rất cần thiết:

- Tối ưu hóa giúp ứng dụng mượt mà hơn.
- Xử lý form là tác vụ cơ bản trong mọi web app.
- Hiểu routing SPA làm nền tảng để đánh giá cao hơn routing tích hợp của Next.js.

Trong Phần 4, chúng ta sẽ chính thức bước vào **Next.js**. Chúng ta sẽ tìm hiểu về:

- **Setup dự án Next.js với TypeScript.**
- **Routing dựa trên File System (Pages Router và App Router - sẽ tập trung vào App Router mới hơn).** So sánh trực tiếp với SvelteKit.
- **Các kiểu Rendering trong Next.js:** SSG (Static Site Generation), SSR (Server-Side Rendering), ISR (Incremental Static Regeneration), Client Components, Server Components. Đây là điểm mạnh cốt lõi của Next.js.
- **Data Fetching trong Next.js.**

Hãy chuẩn bị bước vào thế giới của một framework React đầy đủ tính năng!

---

**Tài liệu React & Next.js "Khủng Khiếp" với TypeScript (Phần 4/5): Bước Vào Next.js - Routing, Rendering & Data Fetching**

---

**Giới thiệu:**

Nếu React là động cơ (engine), thì Next.js là toàn bộ chiếc xe. Nó cung cấp các tính năng cấp framework mà React thuần không có, bao gồm:

- **Routing tích hợp:** Dựa trên hệ thống file, rất giống SvelteKit.
- **Các chiến lược Rendering đa dạng:** SSR, SSG, ISR, Client/Server Components. Đây là điểm cốt lõi và mạnh mẽ nhất của Next.js.
- **Tối ưu hóa tự động:** Tách code (code splitting), tối ưu ảnh (`next/image`), prefetching link...
- **API Routes:** Xây dựng backend API ngay trong project Next.js.
- **TypeScript hỗ trợ sẵn.**

Mục tiêu của Next.js là cung cấp trải nghiệm phát triển tốt nhất và hiệu năng ứng dụng cao nhất có thể khi xây dựng với React.

**Từ SvelteKit:** Bạn sẽ thấy nhiều khái niệm quen thuộc (file-based routing, SSR/SSG, `load` functions tương đương data fetching), nhưng cách triển khai và thuật ngữ trong Next.js (đặc biệt với App Router) có những điểm khác biệt quan trọng.

---

**1. Setup Dự Án Next.js với TypeScript (App Router)**

Cách dễ nhất là dùng `create-next-app`:

```bash
npx create-next-app@latest my-nextjs-app --typescript --eslint --tailwind --src-dir --app --import-alias "@/*"
# Hoặc dùng yarn:
# yarn create next-app my-nextjs-app --typescript --eslint --tailwind --src-dir --app --import-alias "@/*"

# Giải thích các flag quan trọng:
# --typescript: Khởi tạo dự án với TypeScript.
# --eslint: Tích hợp ESLint để kiểm tra code.
# --tailwind: (Tùy chọn) Cài đặt và cấu hình Tailwind CSS.
# --src-dir: Đặt code ứng dụng vào thư mục `src/` (giống cấu trúc SvelteKit thường thấy).
# --app: ***QUAN TRỌNG*** Sử dụng App Router (thay vì Pages Router cũ hơn).
# --import-alias "@/*": (Tùy chọn) Cấu hình alias cho import dễ dàng hơn (ví dụ: `@/components/Button` thay vì `../../components/Button`).
```

Sau khi chạy lệnh trên và trả lời các câu hỏi, bạn sẽ có cấu trúc thư mục cơ bản trong `src/`:

```
src/
├── app/              # ***Thư mục cốt lõi của App Router***
│   ├── favicon.ico
│   ├── globals.css   # CSS toàn cục
│   ├── layout.tsx    # Layout GỐC của toàn bộ ứng dụng (tương đương +layout.svelte gốc)
│   └── page.tsx      # Trang chủ "/" (tương đương +page.svelte ở gốc routes)
│
├── components/       # (Tự tạo) Thư mục chứa các component tái sử dụng
├── lib/              # (Tự tạo) Thư mục chứa các hàm tiện ích, config...
└── ... (các file config như next.config.js, tsconfig.json, ...)
```

**Điểm tương đồng SvelteKit:** Cấu trúc `src/` và ý tưởng dùng một thư mục (`app` trong Next.js, `routes` trong SvelteKit) để định nghĩa routing.

---

**2. File-System Based Routing (App Router)**

Đây là điểm cực kỳ giống SvelteKit. Next.js App Router sử dụng cấu trúc thư mục và tên file đặc biệt bên trong `src/app/` để định nghĩa các route.

**Các Quy Ước Tên File Đặc Biệt:**

- `page.tsx`: Định nghĩa UI chính cho một route. **Tương đương `+page.svelte`**.
- `layout.tsx`: Định nghĩa UI layout chung cho một segment (phân đoạn đường dẫn) và các segment con của nó. Layout cha sẽ bao bọc layout con. **Tương đương `+layout.svelte`**. Layout gốc `src/app/layout.tsx` là bắt buộc.
- `loading.tsx`: (Tùy chọn) Hiển thị UI tạm thời khi dữ liệu cho `page.tsx` hoặc layout con đang được load (sử dụng React Suspense). **Gần giống việc tự xử lý cờ loading trong `load` và component SvelteKit.**
- `error.tsx`: (Tùy chọn) Tự động bắt lỗi xảy ra trong segment và các segment con, hiển thị UI lỗi và cung cấp chức năng thử lại. **Tương đương `+error.svelte`**.
- `template.tsx`: (Tùy chọn) Tương tự `layout.tsx` nhưng _không_ giữ lại state khi điều hướng giữa các trang con. Ít dùng hơn `layout`.
- `route.ts` (hoặc `.js`): Định nghĩa API endpoint (không phải UI). **Tương đương `+server.ts` (hoặc `.js`) trong SvelteKit.** Sẽ tìm hiểu kỹ hơn ở phần sau.

**Ví dụ Cấu Trúc Route:**

```
src/app/
├── layout.tsx        # Layout gốc (ví dụ: thẻ <html>, <body>)
├── page.tsx          # Trang chủ (path: /)
│
├── about/
│   └── page.tsx      # Trang About (path: /about)
│
├── blog/
│   ├── layout.tsx    # Layout riêng cho phần blog (ví dụ: thêm sidebar)
│   ├── page.tsx      # Trang danh sách bài viết (path: /blog)
│   │
│   └── [slug]/       # *** Dynamic Segment (Tham số động) ***
│       ├── page.tsx  # Trang chi tiết bài viết (path: /blog/react-is-cool, /blog/nextjs-vs-sveltekit)
│       └── loading.tsx # UI loading khi fetch chi tiết bài viết
│
├── (marketing)/      # *** Route Group (Không ảnh hưởng URL) ***
│   │                 # Dùng để nhóm các route có chung layout hoặc mục đích
│   ├── contact/
│   │   └── page.tsx  # Path: /contact (không có /marketing/)
│   └── pricing/
│       └── page.tsx  # Path: /pricing
│
├── shop/
│   └── [[...filters]]/ # *** Optional Catch-all Segment ***
│       └── page.tsx    # Khớp /shop, /shop/a, /shop/a/b, ...
│
└── api/              # *** API Routes ***
    └── hello/
        └── route.ts  # Endpoint API tại /api/hello
```

**So Sánh Routing với SvelteKit:**

| Next.js App Router (`src/app`) | SvelteKit (`src/routes`)                        | Ghi Chú                                                              |
| :----------------------------- | :---------------------------------------------- | :------------------------------------------------------------------- |
| `page.tsx`                     | `+page.svelte`                                  | UI chính của route.                                                  |
| `layout.tsx`                   | `+layout.svelte`                                | Layout chung, lồng nhau.                                             |
| `loading.tsx`                  | (Tự xử lý loading)                              | UI loading tích hợp qua Suspense.                                    |
| `error.tsx`                    | `+error.svelte`                                 | Xử lý lỗi trong segment.                                             |
| `[slug]/page.tsx`              | `[slug]/+page.svelte`                           | Dynamic segment. Param lấy qua props `params`.                       |
| `[...slug]/page.tsx`           | `[...slug]/+page.svelte`                        | Catch-all segment. Param là mảng.                                    |
| `[[...slug]]/page.tsx`         | `[[optional]]/+page...` (không hoàn toàn giống) | Optional Catch-all. Khớp cả path gốc. SvelteKit dùng param tùy chọn. |
| `(group)/page.tsx`             | `(group)/+page.svelte`                          | Route group, không ảnh hưởng URL.                                    |
| `route.ts`                     | `+server.ts`                                    | Định nghĩa API endpoint.                                             |

Nhìn chung, cơ chế routing rất tương đồng về mặt khái niệm.

---

**3. Rendering Strategies & Server/Client Components (Trái Tim Của App Router)**

Đây là phần quan trọng và khác biệt nhất so với React SPA thuần và cả cách SvelteKit phân chia công việc. App Router giới thiệu mô hình **React Server Components (RSC)**.

**a) Server Components (Mặc Định):**

- **Là gì?** Component React được render **chỉ trên server**. Code của chúng **không bao giờ** được gửi xuống client.
- **Mặc định:** Tất cả component bên trong `src/app/` (trừ khi được đánh dấu `'use client'`) đều là Server Components. Điều này bao gồm `page.tsx`, `layout.tsx`, `loading.tsx`, `error.tsx`.
- **Ưu điểm:**
  - **Truy cập trực tiếp tài nguyên backend:** Có thể `await` fetch dữ liệu, truy cập database, đọc file hệ thống, sử dụng secret keys... ngay trong component mà không cần tạo API endpoint riêng.
  - **Giảm JavaScript phía client:** Code component không được gửi xuống trình duyệt, giúp trang tải nhanh hơn và nhẹ hơn.
  - **Bảo mật:** Secret keys, logic nhạy cảm được giữ an toàn trên server.
  - **Caching tốt:** Kết quả render có thể được cache mạnh mẽ trên server.
  - **SEO tốt:** Nội dung được render sẵn trên server.
- **Hạn chế:**
  - **Không thể sử dụng Hooks tương tác:** Không dùng được `useState`, `useEffect`, `useContext` (cho client-side context), `useReducer`... vì chúng cần chạy trên client.
  - **Không thể sử dụng Event Listeners:** Không dùng được `onClick`, `onChange`...
  - **Không thể sử dụng Browser APIs:** Không dùng được `window`, `document`, `localStorage`...
- **So sánh SvelteKit:** Server Components giống như phần logic bạn viết trong hàm `load` của `+page.server.ts` hoặc `+layout.server.ts`, nhưng nó được tích hợp _ngay trong_ cách bạn viết component UI. Bạn có thể `await` data fetching ngay trước `return` JSX.

**b) Client Components (`'use client'`):**

- **Là gì?** Component React được render trên server (để tạo HTML ban đầu - SSR/SSG) và sau đó "hydrate" (gắn event listener, chạy effect) và chạy trên client. Code của chúng **được** gửi xuống client.
- **Cách sử dụng:** Đặt directive `'use client';` ở **dòng đầu tiên** của file component.
- **Khi nào dùng?**
  - Khi cần sử dụng Hooks tương tác (`useState`, `useEffect`, `useReducer`...).
  - Khi cần gắn Event Listeners (`onClick`, `onChange`...).
  - Khi cần sử dụng Browser APIs (`window`, `document`...).
  - Khi sử dụng thư viện UI bên thứ ba mà chúng phụ thuộc vào state hoặc effect phía client.
  - Khi dùng Context API phía client.
- **Quan trọng:** Một khi bạn đánh dấu một component là `'use client'`, tất cả các component được _import và sử dụng bên trong_ nó cũng sẽ được coi là Client Components, ngay cả khi chúng không có directive `'use client'`. Do đó, **hãy đặt `'use client'` ở những component "lá" (leaf components) nhất có thể** trong cây component của bạn để giữ phần lớn ứng dụng là Server Components.
- **So sánh SvelteKit:** Client Components hoạt động giống như các component Svelte thông thường trong file `.svelte` của bạn – chúng có thể chứa state, xử lý sự kiện, chạy `onMount`, `onDestroy`... và code của chúng được bundle gửi xuống client.

**c) Mô hình hoạt động:**

Cây component trong App Router là sự kết hợp của Server và Client Components:

```
       Layout (Server)
           |
      Navbar (Client - vì có useState cho menu mobile)
       /         \
Logo (Server)   MenuToggle (Client - vì có onClick)
       |
      Page (Server - fetch data)
       |
   ProductList (Server - nhận data từ Page)
       |
   ProductCard (Server - hiển thị data)
       |
   AddToCartButton (Client - vì có onClick, useState)
```

- **Server Components có thể import và render Client Components.**
- **Client Components KHÔNG THỂ import trực tiếp Server Components.** (Bạn có thể truyền Server Components làm `children` prop cho Client Components).

**d) Các Kiểu Rendering Thực Tế:**

Cách Server Components được render (tại thời điểm build hay request) phụ thuộc vào cách bạn fetch dữ liệu và sử dụng các hàm động:

- **Static Site Generation (SSG - Mặc định):** Nếu một route (và layout của nó) chỉ chứa Server Components và sử dụng `fetch` với tùy chọn cache mặc định (hoặc `force-cache`), Next.js sẽ render route đó thành HTML tĩnh tại **thời điểm build**. Nhanh nhất, tốt nhất cho SEO, có thể deploy lên CDN.
  - **So sánh SvelteKit:** Tương đương `export const prerender = true;`.
- **Server-Side Rendering (SSR - Động):** Nếu một route Server Component sử dụng `fetch` với tùy chọn `{ cache: 'no-store' }` hoặc `{ next: { revalidate: 0 } }`, hoặc sử dụng các hàm động như `headers()`, `cookies()` từ `next/headers`, Next.js sẽ render route đó **tại mỗi request** đến server. Phù hợp cho dữ liệu thay đổi liên tục hoặc cần cá nhân hóa theo user.
  - **So sánh SvelteKit:** Tương đương hành vi mặc định (SSR) hoặc `export const ssr = true;`.
- **Incremental Static Regeneration (ISR):** Kết hợp SSG và SSR. Render trang tĩnh tại thời điểm build, nhưng định kỳ kiểm tra và render lại trang trên server nếu dữ liệu thay đổi, sau một khoảng thời gian (`revalidate`). Người dùng đầu tiên sau khoảng thời gian `revalidate` có thể thấy trang cũ một chút, nhưng server sẽ trigger re-render ngầm và cập nhật cache cho những người dùng tiếp theo.
  - Cách dùng: Sử dụng `fetch` với tùy chọn `{ next: { revalidate: number_of_seconds } }`.
  - **So sánh SvelteKit:** Không có cơ chế ISR tích hợp sẵn tiện lợi như Next.js. Bạn có thể mô phỏng bằng cách set cache headers trong `load` và cấu hình CDN/proxy.

**Client Components luôn được render trước trên server (pre-rendering) thành HTML tĩnh (cho SSR/SSG/ISR) rồi mới hydrate trên client.**

---

**4. Data Fetching trong App Router**

Với Server Components, việc fetch dữ liệu trở nên cực kỳ đơn giản và mạnh mẽ.

**a) Fetching trong Server Components (Khuyến Nghị):**

Bạn có thể dùng `async/await` với `fetch` trực tiếp trong Server Component (`page.tsx`, `layout.tsx`, hoặc component Server khác).

```typescript jsx
// src/app/blog/[slug]/page.tsx

import { notFound } from "next/navigation"; // Hàm để trả về 404

interface Post {
  userId: number;
  id: number;
  title: string;
  body: string;
}

interface BlogPostPageProps {
  params: {
    slug: string; // Lấy từ tên thư mục [slug]
  };
  // searchParams?: { [key: string]: string | string[] | undefined }; // Lấy query params nếu cần
}

// --- Data Fetching Function (có thể tách ra file riêng) ---
// Next.js tự động deduplicate các request fetch giống nhau trong cùng một cây render
// Tự động cache kết quả fetch (mặc định là force-cache -> SSG)
async function getPost(slug: string): Promise<Post | null> {
  try {
    // Ví dụ: slug là post ID
    const res = await fetch(
      `https://jsonplaceholder.typicode.com/posts/${slug}`,
      {
        // --- Tùy chỉnh Caching và Revalidation ---
        // cache: 'force-cache', // Mặc định (SSG, cache vĩnh viễn trừ khi build lại)
        // cache: 'no-store',   // SSR (Không cache, fetch mỗi request)
        next: {
          revalidate: 60, // ISR: Cache trong 60s, sau đó kiểm tra lại (stale-while-revalidate)
        },
      }
    );

    if (!res.ok) {
      // Nếu không tìm thấy post (ví dụ 404 từ API)
      if (res.status === 404) return null;
      // Lỗi khác
      throw new Error(`Failed to fetch post: ${res.statusText}`);
    }
    return res.json();
  } catch (error) {
    console.error("Error fetching post:", error);
    // Có thể throw lỗi để error.tsx bắt hoặc trả về null/thông báo lỗi
    // throw error; // Sẽ bị bắt bởi error.tsx gần nhất
    return null; // Hoặc xử lý nhẹ nhàng hơn
  }
}

// --- Page Component (là Server Component) ---
export default async function BlogPostPage({
  params,
}: BlogPostPageProps): Promise<JSX.Element> {
  const post = await getPost(params.slug); // Gọi hàm fetch trực tiếp

  // Xử lý trường hợp không tìm thấy post
  if (!post) {
    notFound(); // Trả về trang 404 được định nghĩa bởi Next.js hoặc not-found.tsx tùy chỉnh
  }

  // Nếu có post, render UI
  return (
    <div>
      {/* Có thể có layout hoặc component khác ở đây */}
      <article>
        <h1>{post.title}</h1>
        <p>
          Post ID: {post.id}, User ID: {post.userId}
        </p>
        <p>{post.body}</p>
      </article>
      {/* Có thể fetch thêm dữ liệu khác nếu cần */}
      {/* const comments = await getComments(params.slug); */}
    </div>
  );
}

// --- (Tùy chọn) Generate Static Params cho SSG ---
// Giúp Next.js biết trước các giá trị slug có thể có để prerender tại thời điểm build
export async function generateStaticParams() {
  // Ví dụ: fetch danh sách các slug hợp lệ
  const res = await fetch(
    "https://jsonplaceholder.typicode.com/posts?_limit=10"
  );
  const posts: Post[] = await res.json();

  return posts.map((post) => ({
    slug: post.id.toString(), // Param phải là string
  }));
  // Next.js sẽ gọi getPost và render trang cho các slug này khi build
  // Nếu người dùng truy cập slug không có trong list này:
  // - Mặc định (dynamicParams = true): Next.js sẽ cố render trang đó on-demand (SSR) lần đầu, rồi cache lại.
  // - Nếu dynamicParams = false (thêm export const dynamicParams = false;): Trả về 404.
}
```

- **So sánh SvelteKit:** `await fetch(...)` trong Server Component tương đương với việc fetch trong hàm `load` của `+page.server.ts` và trả về dữ liệu qua `return { props: { post } }`. Next.js tích hợp việc này trực tiếp hơn. `generateStaticParams` tương đương việc bạn cung cấp các entry trong `prerender.entries` của SvelteKit hoặc logic tạo trang tĩnh trong adapter.

**b) Fetching trong Client Components:**

Nếu bạn _cần_ fetch dữ liệu từ phía client (ví dụ: dữ liệu phụ thuộc vào tương tác người dùng mà không muốn điều hướng trang), bạn làm như cách truyền thống trong React SPA:

```typescript jsx
// components/ClientDataFetcher.tsx
"use client"; // Đánh dấu là Client Component

import React, { useState, useEffect } from "react";

interface User {
  id: number;
  name: string;
  email: string;
}

export default function ClientDataFetcher(): JSX.Element {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);
  const [userId, setUserId] = useState<number>(1); // State để trigger fetch lại

  useEffect(() => {
    setLoading(true);
    setError(null);
    fetch(`https://jsonplaceholder.typicode.com/users/${userId}`)
      .then((res) => {
        if (!res.ok) throw new Error("Failed to fetch user");
        return res.json();
      })
      .then((data: User) => {
        setUser(data);
        setLoading(false);
      })
      .catch((err) => {
        setError(err.message);
        setLoading(false);
      });
  }, [userId]); // Fetch lại khi userId thay đổi

  return (
    <div>
      <h3>Client-Side Data Fetching Demo</h3>
      <button onClick={() => setUserId((id) => id + 1)}>
        Fetch Next User (ID: {userId + 1})
      </button>
      {loading && <p>Loading user...</p>}
      {error && <p style={{ color: "red" }}>Error: {error}</p>}
      {user && (
        <div>
          <p>Name: {user.name}</p>
          <p>Email: {user.email}</p>
        </div>
      )}
    </div>
  );
}
```

- **Thư viện Data Fetching Client-Side:** Cho các tác vụ phức tạp hơn (caching phía client, revalidation tự động, xử lý mutation...), các thư viện như **SWR** (từ Vercel/Next.js team) hoặc **React Query (TanStack Query)** rất được khuyến nghị khi fetch ở Client Components.
- **So sánh SvelteKit:** Fetching trong `useEffect` tương đương fetching trong `onMount` hoặc dùng `{#await}` block trong Svelte. Fetching trong hàm `load` của `+page.ts` (chạy cả server và client) cũng là một lựa chọn trong SvelteKit.

**Ưu tiên fetching trong Server Components bất cứ khi nào có thể** để tận dụng lợi thế về hiệu năng, bảo mật và caching của Next.js.

---

**5. Các Thành Phần/Hooks Quan Trọng Khác (Giới Thiệu)**

- **`next/link`:** Component để tạo link điều hướng nội bộ. Nó tự động prefetch các trang liên kết (khi link xuất hiện trong viewport) ở chế độ production, giúp chuyển trang nhanh hơn. **Tương đương thẻ `<a>` với các thuộc tính data-sveltekit-... của SvelteKit, nhưng có prefetching tích hợp mạnh mẽ hơn.**

  ```typescript jsx
  import Link from "next/link";

  function MyNav() {
    return <Link href="/about">Go to About</Link>;
  }
  ```

- **`next/navigation`:** Cung cấp các hooks để tương tác với router từ Client Components:

  - `useRouter()`: Lấy đối tượng router để điều hướng chương trình (`router.push('/path')`, `router.back()`...). **Tương đương `goto` từ `$app/navigation` trong SvelteKit.**
  - `usePathname()`: Lấy đường dẫn URL hiện tại. **Tương đương `$page.url.pathname` từ `$app/stores` trong SvelteKit.**
  - `useSearchParams()`: Lấy đối tượng chứa query parameters của URL. **Tương đương `$page.url.searchParams` từ `$app/stores` trong SvelteKit.**

  ```typescript jsx
  // components/SearchButton.tsx
  "use client";
  import { useRouter, usePathname, useSearchParams } from "next/navigation";

  function SearchButton() {
    const router = useRouter();
    const pathname = usePathname(); // ví dụ: "/blog"
    const searchParams = useSearchParams(); // ví dụ: "?q=react"

    const handleSearch = () => {
      const currentQuery = searchParams.get("q") || "";
      // Ví dụ: điều hướng đến trang search
      router.push(`/search?query=${encodeURIComponent(currentQuery)}`);
    };
    return <button onClick={handleSearch}>Search</button>;
  }
  ```

---

**Tạm Kết Phần 4:**

Chúng ta đã đặt những viên gạch đầu tiên vào thế giới Next.js với App Router:

- **Setup dự án** với các cấu hình phổ biến.
- **File-based routing** cực kỳ giống SvelteKit.
- **Server Components (mặc định):** Chạy trên server, truy cập backend, không gửi JS client, không dùng hook tương tác.
- **Client Components (`'use client'`):** Chạy/hydrate client, dùng hook, event, browser API.
- **Rendering:** SSG (mặc định), SSR (khi dùng dynamic fetch/functions), ISR (dùng `revalidate`).
- **Data Fetching:** Ưu tiên `async/await fetch` trong Server Components, `useEffect` hoặc thư viện (SWR/React Query) trong Client Components.
- **`next/link` và `next/navigation`** cho routing phía client.

Bạn đã thấy Next.js cung cấp một mô hình mạnh mẽ để xây dựng ứng dụng React hiệu năng cao, kết hợp sức mạnh của server và client.

Trong **Phần 5 (cuối cùng)**, chúng ta sẽ khám phá các chủ đề nâng cao hơn:

- **API Routes (`route.ts`):** Xây dựng backend API.
- **Layouts Nâng Cao:** Nested layouts, route groups.
- **Metadata & SEO:** Quản lý thẻ `<head>` (`metadata` object).
- **State Management:** Các chiến lược quản lý state trong ứng dụng Next.js (Context, Zustand, Redux Toolkit...).
- **Deployment:** Các lựa chọn và tối ưu khi deploy ứng dụng Next.js (đặc biệt là Vercel).

Hãy chuẩn bị cho phần cuối cùng của hành trình "khủng khiếp" này!

---

**Tài liệu React & Next.js "Khủng Khiếp" với TypeScript (Phần 5/5): API Routes, Layouts Nâng Cao, Metadata, State & Deployment**

---

**Giới thiệu:**

Bạn đã đi một chặng đường dài, từ những viên gạch React cơ bản đến sức mạnh của routing, rendering và data fetching trong Next.js App Router. Trong phần cuối cùng này, chúng ta sẽ khám phá cách xây dựng backend API ngay trong Next.js, tối ưu cấu trúc layout, quản lý SEO và metadata, thảo luận về các chiến lược state management hiệu quả, và cuối cùng là cách đưa ứng dụng của bạn lên "sóng".

---

**1. API Routes (`route.ts`) - Xây Dựng Backend API**

Giống như SvelteKit có `+server.ts`, Next.js App Router cho phép bạn tạo các API endpoint bằng cách tạo file `route.ts` (hoặc `.js`) trong thư mục `app/`.

- **Cách hoạt động:** Export các hàm `async` được đặt tên theo các phương thức HTTP (viết hoa: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `HEAD`, `OPTIONS`). Các hàm này nhận vào `request: Request` (đối tượng Request chuẩn của Web API) và có thể nhận thêm một context object chứa `params` (cho dynamic routes).
- **Trả về:** Sử dụng `NextResponse` (từ `next/server`) để tạo response, cho phép bạn dễ dàng trả về JSON, đặt status code, và set headers.

**Ví dụ:** Tạo endpoint `/api/posts` để lấy danh sách posts (GET) và tạo post mới (POST).

```typescript jsx
// src/app/api/posts/route.ts

import { NextRequest, NextResponse } from "next/server";
import { z } from "zod"; // Thư viện validation mạnh mẽ (ví dụ)

// Giả lập database hoặc nguồn dữ liệu
let posts: { id: number; title: string; body: string }[] = [
  { id: 1, title: "Hello Next.js", body: "This is the first post." },
  { id: 2, title: "API Routes", body: "Learning about API routes." },
];
let nextId = 3;

// --- GET Handler ---
export async function GET(request: NextRequest) {
  // Lấy query params (ví dụ: /api/posts?limit=1)
  const searchParams = request.nextUrl.searchParams;
  const limit = searchParams.get("limit");

  let resultPosts = [...posts];
  if (limit) {
    const limitNum = parseInt(limit, 10);
    if (!isNaN(limitNum) && limitNum > 0) {
      resultPosts = resultPosts.slice(0, limitNum);
    }
  }

  // Trả về JSON với status 200 (mặc định)
  return NextResponse.json(resultPosts);
}

// --- POST Handler ---

// Định nghĩa schema validation cho body request (dùng Zod)
const createPostSchema = z.object({
  title: z.string().min(3, { message: "Title must be at least 3 characters" }),
  body: z.string().min(10, { message: "Body must be at least 10 characters" }),
});

export async function POST(request: NextRequest) {
  try {
    // Đọc và parse JSON body từ request
    const body = await request.json();

    // Validate body sử dụng Zod
    const validation = createPostSchema.safeParse(body);
    if (!validation.success) {
      // Nếu validation thất bại, trả về lỗi 400 Bad Request
      return NextResponse.json(
        { errors: validation.error.flatten().fieldErrors },
        { status: 400 }
      );
    }

    // Nếu validation thành công, tạo post mới
    const { title, body: validatedBody } = validation.data; // Đổi tên body để tránh trùng
    const newPost = { id: nextId++, title, body: validatedBody };
    posts.push(newPost);

    // Trả về post vừa tạo với status 201 Created
    return NextResponse.json(newPost, { status: 201 });
  } catch (error) {
    // Xử lý lỗi nếu parse JSON thất bại hoặc có lỗi khác
    console.error("Error in POST /api/posts:", error);
    return NextResponse.json(
      { message: "Internal Server Error" },
      { status: 500 }
    );
  }
}

// --- Ví dụ Dynamic Route API ---
// src/app/api/posts/[postId]/route.ts
interface RouteContext {
  params: {
    postId: string;
  };
}
export async function GET(request: NextRequest, { params }: RouteContext) {
  const postId = parseInt(params.postId, 10);
  if (isNaN(postId)) {
    return NextResponse.json({ message: "Invalid post ID" }, { status: 400 });
  }

  const post = posts.find((p) => p.id === postId);
  if (!post) {
    return NextResponse.json({ message: "Post not found" }, { status: 404 });
  }

  return NextResponse.json(post);
}

// Có thể thêm PUT, DELETE handlers ở đây...
// export async function DELETE(request: NextRequest, { params }: RouteContext) { ... }
```

**Gọi API Routes từ Client Components:**

Bạn có thể dùng `fetch` hoặc thư viện như SWR/React Query như cách gọi API thông thường.

```typescript jsx
// components/PostManager.tsx
"use client";
import React, { useState, useEffect } from "react";

// ... (interface Post giống ở route.ts)

function PostManager() {
  const [posts, setPosts] = useState<Post[]>([]);
  const [newTitle, setNewTitle] = useState("");
  const [newBody, setNewBody] = useState("");
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    fetch("/api/posts") // Gọi GET endpoint
      .then((res) => res.json())
      .then((data: Post[]) => {
        setPosts(data);
        setLoading(false);
      })
      .catch((err) => {
        setError("Failed to load posts");
        setLoading(false);
      });
  }, []);

  const handleAddPost = async (e: React.FormEvent) => {
    e.preventDefault();
    setError(null);
    try {
      const res = await fetch("/api/posts", {
        // Gọi POST endpoint
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ title: newTitle, body: newBody }),
      });

      if (!res.ok) {
        const errorData = await res.json();
        throw new Error(
          JSON.stringify(
            errorData.errors || errorData.message || "Failed to add post"
          )
        );
      }

      const newPost = await res.json();
      setPosts((prev) => [...prev, newPost]);
      setNewTitle("");
      setNewBody("");
    } catch (err: any) {
      setError(err.message);
    }
  };

  // ... JSX để hiển thị posts và form thêm post
  return (
    <div>
      <h2>Posts</h2>
      {loading && <p>Loading...</p>}
      {error && <p style={{ color: "red" }}>Error: {error}</p>}
      <ul>
        {posts.map((p) => (
          <li key={p.id}>{p.title}</li>
        ))}
      </ul>

      <form onSubmit={handleAddPost}>
        <h3>Add New Post</h3>
        <input
          type="text"
          value={newTitle}
          onChange={(e) => setNewTitle(e.target.value)}
          placeholder="Title"
          required
        />
        <textarea
          value={newBody}
          onChange={(e) => setNewBody(e.target.value)}
          placeholder="Body"
          required
        />
        <button type="submit">Add Post</button>
      </form>
    </div>
  );
}

export default PostManager;
```

**So sánh với SvelteKit:**

- `route.ts` trong Next.js tương đương `+server.ts` trong SvelteKit.
- Cách export hàm theo tên phương thức HTTP là giống hệt.
- Next.js dùng `Request` và `NextResponse` (dựa trên Web API chuẩn), SvelteKit dùng `RequestEvent` (chứa `request`, `params`, `url`, `cookies`...) và `Response` hoặc các helper như `json()`. Về cơ bản là rất tương đồng.

---

**2. Layouts Nâng Cao & Route Groups**

- **Nested Layouts:** Như đã thấy, `layout.tsx` trong một thư mục sẽ bao bọc `page.tsx` và các layout con bên trong thư mục đó. State của layout được giữ lại khi điều hướng giữa các trang con.
- **Route Groups `(folderName)`:** Dùng dấu ngoặc đơn `()` để tạo thư mục nhóm mà **không** ảnh hưởng đến đường dẫn URL. Mục đích chính:

  - **Tổ chức routes:** Gom các route liên quan vào cùng một chỗ.
  - **Áp dụng layout cụ thể:** Đặt một `layout.tsx` bên trong thư mục nhóm để áp dụng layout đó chỉ cho các route trong nhóm đó.

  ```
  src/app/
  ├── (marketing)/          # Nhóm không ảnh hưởng URL
  │   ├── layout.tsx        # Layout riêng cho trang marketing (ví dụ: header/footer khác)
  │   ├── about/
  │   │   └── page.tsx      # URL: /about (dùng layout của marketing)
  │   └── contact/
  │       └── page.tsx      # URL: /contact (dùng layout của marketing)
  │
  ├── (shop)/               # Nhóm khác
  │   ├── layout.tsx        # Layout cho shop (ví dụ: có sidebar sản phẩm)
  │   ├── page.tsx          # URL: / (trang chủ shop, dùng layout shop)
  │   └── products/
  │       └── [id]/
  │           └── page.tsx  # URL: /products/123 (dùng layout shop)
  │
  └── layout.tsx            # Layout gốc (áp dụng cho cả marketing và shop)
  └── page.tsx              # Trang chủ gốc (nếu không có (shop)/page.tsx, hoặc nếu cần trang gốc riêng)
  ```

- **Parallel Routes (Nâng cao):** Sử dụng ký hiệu `@folderName` để định nghĩa các "slot" độc lập trong cùng một layout. Bạn có thể render các trang hoàn toàn khác nhau vào các slot này dựa trên URL, hữu ích cho dashboard phức tạp hoặc modal. (Tìm hiểu thêm trong docs Next.js khi cần).

---

**3. Metadata & SEO (`<head>` Management)**

Next.js App Router cung cấp API `metadata` để dễ dàng quản lý các thẻ trong `<head>` (như `title`, `meta description`, Open Graph tags...).

- **Static Metadata:** Export một object `metadata` từ `layout.tsx` hoặc `page.tsx`. Metadata từ layout cha sẽ được merge với metadata từ layout/page con (con ghi đè cha nếu trùng key).

  ```typescript jsx
  // src/app/layout.tsx
  import type { Metadata } from "next"; // Import kiểu Metadata

  export const metadata: Metadata = {
    title: {
      template: "%s | My Awesome Site", // Template cho title, %s là title từ page con
      default: "My Awesome Site", // Title mặc định nếu page con không có
    },
    description: "The best site ever built with Next.js",
    openGraph: {
      // Ví dụ Open Graph tags
      title: "My Awesome Site",
      description: "The best site ever built...",
      images: ["/og-image.png"], // Đường dẫn đến ảnh trong public/
    },
  };

  export default function RootLayout({
    children,
  }: {
    children: React.ReactNode;
  }) {
    return (
      <html lang="en">
        <body>{children}</body>
      </html>
    );
  }
  ```

  ```typescript jsx
  // src/app/about/page.tsx
  import type { Metadata } from "next";

  // Metadata cụ thể cho trang About
  export const metadata: Metadata = {
    title: "About Us", // Sẽ thành "About Us | My Awesome Site" nhờ template ở layout
    description: "Learn more about our amazing team.",
  };

  export default function AboutPage() {
    return <h1>About Page</h1>;
  }
  ```

- **Dynamic Metadata:** Export một hàm `async` tên là `generateMetadata`. Hàm này nhận vào `params` và `searchParams` giống như page component và trả về một object `Metadata`. Hữu ích khi metadata phụ thuộc vào dữ liệu được fetch.

  ```typescript jsx
  // src/app/blog/[slug]/page.tsx
  import type { Metadata, ResolvingMetadata } from 'next';
  // import { getPost } from './data-fetching-logic'; // Hàm fetch post đã định nghĩa ở ví dụ trước

  interface BlogPostPageProps { params: { slug: string } }

  // Hàm generateMetadata
  export async function generateMetadata(
    { params }: BlogPostPageProps,
    parent: ResolvingMetadata // Có thể truy cập metadata đã được resolve từ layout cha
  ): Promise<Metadata> {
      const slug = params.slug;
      const post = await getPost(slug); // Fetch dữ liệu post

      if (!post) {
          // Trả về metadata mặc định hoặc cho trang 404 nếu muốn
          return { title: 'Post Not Found' };
      }

      // Lấy metadata từ cha (ví dụ: để thêm vào description)
      // const previousImages = (await parent).openGraph?.images || [];

      return {
          title: post.title, // Title động
          description: post.body.substring(0, 160), // Description động
          openGraph: {
              title: post.title,
              description: post.body.substring(0, 100),
              // images: ['/some-dynamic-image.png', ...previousImages],
          },
      };
  }

  // Page component vẫn fetch data như bình thường (Next.js sẽ deduplicate fetch)
  export default async function BlogPostPage({ params }: BlogPostPageProps) {
      const post = await getPost(params.slug);
      // ... render page ...
       if (!post) { /* ... */ }
       return (/* ... JSX ... */);
  }
  ```

- **So sánh SvelteKit:** API `metadata` của Next.js tập trung và có cấu trúc hơn so với việc dùng `<svelte:head>` rải rác trong các component Svelte. Việc generate dynamic metadata trong `generateMetadata` tương đương với việc return object chứa metadata từ hàm `load` trong SvelteKit, nhưng Next.js cung cấp type `Metadata` và cơ chế merge/resolve tích hợp sẵn.

---

**4. State Management Strategies**

Việc lựa chọn cách quản lý state phụ thuộc vào độ phức tạp và phạm vi của state đó.

- **Local State (`useState`, `useReducer`):** Hoàn hảo cho state chỉ thuộc về một component hoặc một vài component con gần nhau. **Chỉ dùng trong Client Components.**
- **Context API (`useContext`):** Tốt cho việc chia sẻ state "bán tĩnh" (ít thay đổi) hoặc các hàm xuyên suốt cây component mà không cần prop drilling (ví dụ: theme, locale, auth status). **Provider thường là Client Component, consumer cũng là Client Component.** Cẩn thận với vấn đề hiệu năng khi value thay đổi thường xuyên hoặc state lớn, vì tất cả consumer sẽ re-render.
- **Dedicated State Management Libraries (Thường dùng cho Global State phức tạp/thay đổi thường xuyên):** Các thư viện này cung cấp giải pháp tối ưu hơn Context API cho state toàn cục, dễ dàng xử lý logic phức tạp, và có devtools tốt. **Chúng được dùng chủ yếu bên trong Client Components.**

  - **Zustand:**

    - **Ưu điểm:** Cực kỳ đơn giản, nhẹ, linh hoạt, hook-based, ít boilerplate. Rất dễ học và tích hợp. Hiệu năng tốt (chỉ re-render component nào thực sự sử dụng phần state đã thay đổi).
    - **Cách dùng (cơ bản):** Tạo một "store" bằng `create` và sử dụng hook được trả về trong Client Component.

    ```typescript jsx
    // stores/counterStore.ts
    import { create } from "zustand";

    interface CounterState {
      count: number;
      increment: () => void;
      decrement: () => void;
    }

    export const useCounterStore = create<CounterState>((set) => ({
      count: 0,
      increment: () => set((state) => ({ count: state.count + 1 })),
      decrement: () => set((state) => ({ count: state.count - 1 })),
    }));
    ```

    ```typescript jsx
    // components/ZustandCounter.tsx
    "use client";
    import { useCounterStore } from "@/stores/counterStore"; // Import hook

    function ZustandCounter() {
      // Lấy state và actions từ store
      const count = useCounterStore((state) => state.count);
      const increment = useCounterStore((state) => state.increment);
      const decrement = useCounterStore((state) => state.decrement);

      // Hoặc lấy cả object store: const { count, increment, decrement } = useCounterStore();

      return (
        <div>
          <h2>Zustand Counter</h2>
          <p>Count: {count}</p>
          <button onClick={increment}>+</button>
          <button onClick={decrement}>-</button>
        </div>
      );
    }
    export default ZustandCounter;
    ```

  - **Redux Toolkit (RTK):**

    - **Ưu điểm:** Tiêu chuẩn công nghiệp, cấu trúc rõ ràng (reducers, actions, slices), cực kỳ mạnh mẽ cho state phức tạp, hệ sinh thái lớn, DevTools tuyệt vời.
    - **Nhược điểm:** Nhiều boilerplate hơn Zustand, courbe học dốc hơn một chút.
    - **Cách dùng:** Cần setup Store, tạo Slice (chứa reducer và actions), và dùng các hook (`useSelector`, `useDispatch`) trong Client Component. (Xem docs RTK để biết chi tiết setup).

  - **Các thư viện khác:** Jotai (atomic state), Valtio (proxy-based)...

- **Server Components & State:** Nhớ rằng Server Components không có state theo cách truyền thống. Chúng lấy dữ liệu qua props (từ layout/page cha) hoặc fetch trực tiếp. Dữ liệu "toàn cục" cho Server Components thường đến từ database, CMS, hoặc các nguồn dữ liệu server-side khác.

- **So sánh SvelteKit:** Svelte Stores (`writable`, `readable`, `derived`) cực kỳ đơn giản và hiệu quả cho state management trong SvelteKit, có thể coi là tương đương với Zustand về độ dễ dùng. RTK cung cấp cấu trúc chặt chẽ hơn cho các ứng dụng rất lớn. Việc lựa chọn trong React/Next.js phụ thuộc nhiều vào quy mô dự án và sở thích cá nhân/team.

---

**5. Deployment**

Đưa ứng dụng Next.js của bạn ra thế giới!

- **Vercel (Nền tảng được khuyến nghị):**

  - Được tạo bởi chính đội ngũ Next.js.
  - **Tích hợp hoàn hảo:** Hỗ trợ đầy đủ tất cả các tính năng của Next.js (SSR, ISR, Server Components, API Routes, Image Optimization, Edge Functions...) gần như không cần cấu hình.
  - **Deploy dễ dàng:** Kết nối repo Git (GitHub, GitLab, Bitbucket) và Vercel tự động build và deploy mỗi khi bạn push code.
  - **CDN Toàn cầu:** Phân phối các tài sản tĩnh (SSG pages, JS, CSS, images) nhanh chóng.
  - **Serverless Functions:** API Routes và SSR/ISR pages được deploy dưới dạng serverless function (hoặc Edge function tùy cấu hình).
  - **Miễn phí:** Gói Hobby rất hào phóng cho các dự án cá nhân và thử nghiệm.

- **Các Nền tảng Khác:**

  - **Netlify:** Hỗ trợ tốt Next.js, cần cài đặt adapter (`@netlify/plugin-nextjs`) để đảm bảo các tính năng như SSR/ISR hoạt động đúng.
  - **AWS Amplify, Google Cloud Run, Azure Static Web Apps:** Cung cấp các giải pháp để deploy ứng dụng Next.js, thường yêu cầu cấu hình phức tạp hơn Vercel.
  - **Node.js Server (Self-hosting):** Chạy `next build` rồi `next start`. Bạn cần một server Node.js để chạy ứng dụng. Phù hợp nếu bạn muốn toàn quyền kiểm soát hạ tầng. Cần tự quản lý server, scaling, caching...
  - **Docker:** Đóng gói ứng dụng Next.js (sau khi build) vào Docker image để deploy lên các nền tảng container như Kubernetes, AWS ECS...

- **Build Command:** `npm run build` hoặc `yarn build` (chạy `next build`). Lệnh này sẽ tạo thư mục `.next` chứa phiên bản tối ưu hóa của ứng dụng sẵn sàng để deploy.
- **Environment Variables:** Sử dụng file `.env.local` (không commit lên Git) cho các biến môi trường cục bộ, `.env.production` cho production build, `.env.development` cho development. Next.js tự động load các biến này. Các biến cần expose cho browser phải có tiền tố `NEXT_PUBLIC_`. Cấu hình biến môi trường trên nền tảng deployment (Vercel, Netlify...) cho môi trường production thực tế.

---

**Tạm Kết & Lời Khuyên Cuối Cùng:**

Chúc mừng bạn đã hoàn thành hành trình "khủng khiếp" khám phá React và Next.js! Chúng ta đã đi từ những khái niệm cơ bản nhất của React (JSX, Components, State, Props) đến sức mạnh của Next.js với App Router:

- Routing dựa trên file system linh hoạt.
- Mô hình Server Components và Client Components độc đáo, tối ưu hiệu năng và trải nghiệm dev.
- Các chiến lược rendering đa dạng (SSG, SSR, ISR).
- Data fetching tích hợp mạnh mẽ trong Server Components.
- API Routes để xây dựng backend.
- Quản lý Metadata hiệu quả.
- Các lựa chọn state management phù hợp.
- Triển khai dễ dàng (đặc biệt với Vercel).

**Lời khuyên:**

1.  **Thực hành, thực hành, thực hành:** Lý thuyết là cần thiết, nhưng cách tốt nhất để thực sự hiểu là xây dựng dự án. Bắt đầu với một dự án nhỏ (blog, portfolio, todo list...) sử dụng Next.js App Router.
2.  **Đọc kỹ Docs:** Tài liệu của Next.js (nextjs.org/docs) cực kỳ chi tiết và là nguồn tham khảo tốt nhất.
3.  **Hiểu Server vs. Client:** Nắm vững sự khác biệt và khi nào nên dùng Server Components, khi nào dùng Client Components là chìa khóa để tận dụng tối đa Next.js. Luôn cố gắng giữ component là Server Component nếu có thể.
4.  **So sánh với SvelteKit:** Việc liên tục đối chiếu giúp bạn củng cố kiến thức và hiểu rõ ưu nhược điểm của từng framework trong các tình huống khác nhau.
5.  **Tham gia Cộng đồng:** Stack Overflow, Discord, GitHub Discussions là nơi tuyệt vời để đặt câu hỏi và học hỏi từ người khác.

## Chuyển đổi từ SvelteKit sang React/Next.js có thể có những thách thức ban đầu (đặc biệt là với Hooks và sự tường minh của React), nhưng Next.js cung cấp một hệ sinh thái cực kỳ mạnh mẽ và trưởng thành để xây dựng các ứng dụng web hiện đại, hiệu năng cao.

**Tài liệu React & Next.js "Khủng Khiếp" với TypeScript (Phần 6/10): Nâng Cấp Backend - Database & ORM với Prisma**

---

**Giới thiệu:**

Ở Phần 5, chúng ta đã xây dựng các API route mạnh mẽ, nhưng chúng vẫn hoạt động với một mảng dữ liệu "giả lập" trong bộ nhớ (`let posts = [...]`). Điều này rất tốt cho việc học, nhưng trong thế giới thực, dữ liệu cần được lưu trữ bền vững. Khi server khởi động lại, dữ liệu của bạn phải còn đó. Đây là lúc **cơ sở dữ liệu (database)** và **ORM (Object-Relational Mapping)** vào cuộc.

Phần này sẽ hướng dẫn bạn cách tích hợp **Prisma**, ORM TypeScript-first hiện đại và cực kỳ phổ biến trong hệ sinh thái Next.js. Prisma giúp bạn định nghĩa schema dữ liệu, di chuyển (migrate) cấu trúc database, và thực hiện các truy vấn một cách an toàn kiểu (type-safe) mà không cần viết SQL thuần.

**So sánh với SvelteKit:** Khái niệm này hoàn toàn giống nhau. Trong SvelteKit, các file `+server.ts` của bạn cũng cần một cách để nói chuyện với database. Prisma là lựa chọn hàng đầu cho cả SvelteKit và Next.js vì khả năng tích hợp và type safety tuyệt vời của nó. Logic sử dụng Prisma trong API Route của Next.js gần như y hệt logic trong `+server.ts` của SvelteKit.

---

**1. Setup Prisma trong Dự Án Next.js**

Đầu tiên, chúng ta cần cài đặt Prisma CLI và Prisma Client.

```bash
npm install prisma --save-dev
npm install @prisma/client
```

Tiếp theo, khởi tạo Prisma trong dự án của bạn:

```bash
npx prisma init
```

Lệnh này sẽ làm hai việc chính:

1.  Tạo một thư mục `prisma/` chứa file `schema.prisma`. Đây là nơi bạn sẽ định nghĩa các model dữ liệu của mình.
2.  Tạo một file `.env` ở gốc dự án. File này chứa biến môi trường `DATABASE_URL` để kết nối đến database của bạn.

Mở file `.env` và cấu hình chuỗi kết nối. Để đơn giản cho tutorial, chúng ta sẽ dùng **SQLite**, một database dựa trên file không yêu cầu server riêng.

```env
# .env
# Connection string cho SQLite
DATABASE_URL="file:./dev.db"
```

Bây giờ, mở file `prisma/schema.prisma` và chỉnh sửa `datasource`:

```prisma
// prisma/schema.prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "sqlite" // Đổi từ "postgresql" (mặc định) sang "sqlite"
  url      = env("DATABASE_URL")
}

// Model dữ liệu sẽ được định nghĩa ở đây
```

---

**2. Định Nghĩa Schema (`schema.prisma`)**

Đây là nơi sức mạnh của Prisma bắt đầu tỏa sáng. Bạn định nghĩa các model của mình bằng một ngôn ngữ đơn giản, dễ đọc. Prisma sẽ dựa vào đây để tạo ra các migration và Prisma Client với đầy đủ type.

Hãy mở rộng ví dụ về `Post` và thêm `User` để thể hiện mối quan hệ.

```prisma
// prisma/schema.prisma

// ... (phần generator và datasource ở trên)

model User {
  id    Int     @id @default(autoincrement())
  email String  @unique
  name  String? // Dấu ? nghĩa là trường này không bắt buộc (optional)
  posts Post[]  // Một User có thể có nhiều Post (quan hệ một-nhiều)
}

model Post {
  id        Int      @id @default(autoincrement())
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  title     String
  body      String
  published Boolean  @default(false)

  // Khóa ngoại để liên kết với User
  authorId Int
  // Định nghĩa mối quan hệ: một Post thuộc về một User
  author   User     @relation(fields: [authorId], references: [id])
}
```

**Giải thích:**

- `model`: Định nghĩa một bảng trong database.
- `@id @default(autoincrement())`: Đánh dấu trường `id` là khóa chính và tự động tăng.
- `@unique`: Đảm bảo giá trị trong cột này là duy nhất.
- `@relation`: Định nghĩa mối quan hệ giữa hai model. Ở đây, `Post.authorId` là khóa ngoại liên kết đến `User.id`.

---

**3. Chạy Migration để Đồng Bộ Database**

Sau khi định nghĩa schema, chúng ta cần áp dụng những thay đổi này vào database thực tế. Prisma gọi quá trình này là **migration**.

Chạy lệnh sau trong terminal:

```bash
npx prisma migrate dev --name init
```

- `migrate dev`: Tạo và áp dụng migration trong môi trường development.
- `--name init`: Đặt tên cho migration này (ví dụ: `init` cho lần đầu tiên).

Prisma sẽ:

1.  Lưu một "snapshot" của `schema.prisma` của bạn.
2.  Tạo một file migration SQL trong thư mục `prisma/migrations/`.
3.  Áp dụng file SQL đó vào database của bạn (tạo ra file `dev.db` và các bảng `User`, `Post`).
4.  **Tự động chạy `prisma generate`** để cập nhật Prisma Client theo schema mới nhất.

---

**4. Tích Hợp Prisma Client vào Ứng Dụng (Best Practice)**

`PrismaClient` là class bạn dùng để gửi truy vấn đến database. Một best practice quan trọng trong môi trường serverless (như Vercel) và trong development với Next.js (do hot-reloading) là **tránh tạo quá nhiều instance của `PrismaClient`**.

Hãy tạo một file để khởi tạo và export một instance duy nhất (singleton).

```typescript
// src/lib/prisma.ts

import { PrismaClient } from "@prisma/client";

// Khai báo biến global để lưu trữ instance
declare global {
  var prisma: PrismaClient | undefined;
}

// Khởi tạo PrismaClient, gán vào biến global nếu ở dev để tránh tạo instance mới mỗi lần hot-reload
// Ở production, luôn tạo instance mới.
const client = globalThis.prisma || new PrismaClient();
if (process.env.NODE_ENV !== "production") globalThis.prisma = client;

export default client;
```

Bây giờ, bất cứ khi nào cần truy vấn database, bạn chỉ cần import `client` từ file này.

---

**5. Sử Dụng Prisma trong API Routes và Server Components**

Đây là lúc chúng ta gặt hái thành quả! Hãy refactor lại API route và một Server Component để sử dụng Prisma.

**a) Refactor API Route (`/api/posts/route.ts`)**

```typescript
// src/app/api/posts/route.ts

import { NextRequest, NextResponse } from "next/server";
import { z } from "zod";
import prisma from "@/lib/prisma"; // Import instance PrismaClient duy nhất

// --- GET Handler ---
export async function GET(request: NextRequest) {
  try {
    const posts = await prisma.post.findMany({
      // Lấy tất cả bài viết
      where: { published: true }, // Chỉ lấy bài viết đã published
      orderBy: { createdAt: "desc" }, // Sắp xếp theo ngày tạo mới nhất
      include: {
        // Bao gồm cả dữ liệu từ model liên quan
        author: {
          select: { name: true, email: true }, // Chỉ lấy tên và email của tác giả
        },
      },
    });
    return NextResponse.json(posts);
  } catch (error) {
    console.error("Error fetching posts:", error);
    return NextResponse.json(
      { message: "Internal Server Error" },
      { status: 500 }
    );
  }
}

// --- POST Handler ---
const createPostSchema = z.object({
  title: z.string().min(3),
  body: z.string().min(10),
  authorId: z.number(), // Cần authorId để tạo post
});

export async function POST(request: NextRequest) {
  try {
    const body = await request.json();
    const validation = createPostSchema.safeParse(body);

    if (!validation.success) {
      return NextResponse.json(
        { errors: validation.error.errors },
        { status: 400 }
      );
    }

    const { title, body: postBody, authorId } = validation.data;

    // TODO: Trong thực tế, authorId sẽ được lấy từ session của người dùng đã đăng nhập
    // Tạm thời, ta giả định user có id 1 tồn tại
    const newPost = await prisma.post.create({
      data: {
        title,
        body: postBody,
        authorId: authorId, // Liên kết post với user
      },
    });

    return NextResponse.json(newPost, { status: 201 });
  } catch (error) {
    console.error("Error creating post:", error);
    return NextResponse.json(
      { message: "Internal Server Error" },
      { status: 500 }
    );
  }
}
```

**b) Sử dụng Prisma trực tiếp trong Server Component**

Đây là một trong những điểm mạnh nhất của Next.js App Router. Bạn có thể fetch dữ liệu từ database ngay trong component render trên server mà không cần phải tạo một API route riêng.

```typescript jsx
// src/app/blog/[slug]/page.tsx
import { notFound } from "next/navigation";
import prisma from "@/lib/prisma"; // Import Prisma client

interface BlogPostPageProps {
  params: {
    slug: string; // Giả sử slug là post ID
  };
}

// Dữ liệu được fetch trên server tại thời điểm request (hoặc build time)
export default async function BlogPostPage({ params }: BlogPostPageProps) {
  const postId = parseInt(params.slug, 10);
  if (isNaN(postId)) {
    notFound();
  }

  const post = await prisma.post.findUnique({
    where: { id: postId },
    include: { author: true }, // Lấy cả thông tin tác giả
  });

  if (!post) {
    notFound(); // Trả về trang 404 nếu không tìm thấy post
  }

  return (
    <article>
      <h1>{post.title}</h1>
      <p>By {post.author.name || "Unknown Author"}</p>
      <div>{post.body}</div>
    </article>
  );
}
```

Bạn sẽ thấy **autocompletion** và **type-checking** đầy đủ khi viết `prisma.post.findUnique(...)`, đó chính là ma thuật của Prisma!

---

**Tạm Kết Phần 6:**

Chúng ta đã nâng cấp ứng dụng của mình từ một backend "giả lập" lên một hệ thống có cơ sở dữ liệu thực sự:

- **Setup Prisma:** Khởi tạo và cấu hình Prisma với SQLite.
- **Định nghĩa Schema:** Học cách tạo model và định nghĩa mối quan hệ.
- **Chạy Migrations:** Đồng bộ schema với database một cách an toàn.
- **Tích hợp Prisma Client:** Sử dụng best practice để quản lý kết nối database.
- **Truy vấn Dữ liệu:** Sử dụng Prisma Client với đầy đủ type-safety trong cả **API Routes** (cho client-side fetching) và **Server Components** (cho server-side rendering).

## Bây giờ chúng ta đã có user và post trong database, nhưng làm sao để biết user nào đang tạo post? Làm sao để bảo vệ endpoint chỉ cho người dùng đã đăng nhập? Phần 7 sẽ giải quyết vấn đề cốt lõi này: **Xác thực và Phân quyền (Authentication) với Auth.js.**

---

**Tài liệu React & Next.js "Khủng Khiếp" với TypeScript (Phần 7/10): Quản Lý Người Dùng - Authentication với Auth.js (NextAuth.js)**

---

**Giới thiệu:**

Ở Phần 6, chúng ta đã có một cơ sở dữ liệu với model `User` và `Post`. Chúng ta đã có thể tạo bài viết mới và liên kết nó với một `authorId`. Tuy nhiên, `authorId` này vẫn đang được nhập thủ công. Đây là một lỗ hổng bảo mật và trải nghiệm người dùng lớn:

1.  Làm sao ứng dụng biết **ai** đang gửi request?
2.  Làm sao để đảm bảo chỉ người dùng đã đăng nhập mới có thể tạo bài viết?
3.  Làm sao để tự động điền `authorId` của người dùng đang đăng nhập?

Phần này sẽ giải quyết tất cả những vấn đề trên bằng cách tích hợp **Auth.js (trước đây là NextAuth.js)**, thư viện xác thực "tiêu chuẩn" cho Next.js. Nó giúp đơn giản hóa việc thêm các luồng đăng nhập phức tạp (Email/Password, Google, GitHub, Facebook...) vào ứng dụng của bạn.

**So sánh với SvelteKit:** SvelteKit cũng có thể sử dụng Auth.js thông qua một adapter (`@auth/sveltekit`). Tuy nhiên, trong hệ sinh thái SvelteKit, việc sử dụng các dịch vụ bên thứ ba như Supabase Auth, Lucia, hoặc tự xây dựng luồng xác thực cũng khá phổ biến. Auth.js cung cấp một giải pháp "tất cả trong một" được tích hợp cực kỳ sâu và mượt mà với Next.js, giúp bạn tiết kiệm rất nhiều thời gian và công sức.

---

**1. Cài Đặt và Cấu Hình Cơ Bản**

Đầu tiên, cài đặt thư viện `next-auth`.

```bash
npm install next-auth
```

Auth.js hoạt động dựa trên một **catch-all API route**. Route này sẽ xử lý tất cả các request liên quan đến xác thực (ví dụ: `/api/auth/signin`, `/api/auth/signout`, `/api/auth/session`).

Tạo file sau:

```typescript
// src/app/api/auth/[...nextauth]/route.ts

import NextAuth from "next-auth";
// Chúng ta sẽ thêm các cấu hình providers ở đây
import { authOptions } from "@/lib/auth"; // Tách config ra file riêng cho gọn

const handler = NextAuth(authOptions);

export { handler as GET, handler as POST };
```

Tiếp theo, tạo file cấu hình cho Auth.js. Đây là nơi chứa toàn bộ "bộ não" của hệ thống xác thực.

```typescript
// src/lib/auth.ts
import { NextAuthOptions } from "next-auth";

export const authOptions: NextAuthOptions = {
  // Configure one or more authentication providers
  providers: [
    // Chúng ta sẽ thêm các providers ở đây, ví dụ: GitHub, Google, Credentials
  ],
  // Các cấu hình khác sẽ được thêm ở đây
};
```

Cuối cùng, bạn cần một `NEXTAUTH_SECRET`. Auth.js sử dụng secret này để mã hóa JWT và các dữ liệu nhạy cảm khác. Chạy lệnh sau trong terminal để tạo một secret ngẫu nhiên:

```bash
openssl rand -base64 32
```

Thêm secret này và `NEXTAUTH_URL` vào file `.env` của bạn.

```env
# .env

# ... DATABASE_URL từ phần trước

# Auth.js
NEXTAUTH_URL="http://localhost:3000" # URL của ứng dụng khi dev
NEXTAUTH_SECRET="your-generated-secret-goes-here"
```

---

**2. Thêm Provider - Đăng Nhập với GitHub (OAuth)**

OAuth là cách dễ nhất để bắt đầu. Người dùng sẽ đăng nhập thông qua một dịch vụ bên thứ ba mà họ đã tin tưởng.

1.  Truy cập GitHub > Settings > Developer settings > OAuth Apps > New OAuth App.
2.  Điền thông tin:
    - Application name: `My Next.js App`
    - Homepage URL: `http://localhost:3000`
    - **Authorization callback URL:** `http://localhost:3000/api/auth/callback/github`
3.  Sau khi tạo, bạn sẽ nhận được **Client ID** và tạo một **Client Secret**. Thêm chúng vào file `.env`.

```env
# .env
# ... các biến khác
GITHUB_ID="your-github-client-id"
GITHUB_SECRET="your-github-client-secret"
```

Bây giờ, cập nhật file cấu hình `authOptions`:

```typescript
// src/lib/auth.ts
import { NextAuthOptions } from "next-auth";
import GitHubProvider from "next-auth/providers/github";

export const authOptions: NextAuthOptions = {
  providers: [
    GitHubProvider({
      clientId: process.env.GITHUB_ID as string,
      clientSecret: process.env.GITHUB_SECRET as string,
    }),
    // Thêm các provider khác ở đây
  ],
};
```

Chỉ với vài bước như vậy, bạn đã có luồng đăng nhập bằng GitHub! Truy cập `http://localhost:3000/api/auth/signin` và bạn sẽ thấy nút "Sign in with GitHub".

---

**3. Thêm Provider - Đăng Nhập với Email/Password (Credentials)**

Đây là luồng phổ biến nhưng phức tạp hơn, vì chúng ta phải tự xử lý logic kiểm tra username và password.

Đầu tiên, cài đặt `bcrypt` để băm và so sánh mật khẩu một cách an toàn. **Không bao giờ lưu mật khẩu thuần trong database!**

```bash
npm install bcrypt
npm install @types/bcrypt --save-dev
```

Bây giờ, cập nhật `authOptions` để thêm `CredentialsProvider`.

```typescript
// src/lib/auth.ts
import { PrismaAdapter } from "@auth/prisma-adapter";
import { NextAuthOptions } from "next-auth";
import GitHubProvider from "next-auth/providers/github";
import CredentialsProvider from "next-auth/providers/credentials";
import prisma from "@/lib/prisma";
import bcrypt from "bcrypt";

export const authOptions: NextAuthOptions = {
  // Sử dụng PrismaAdapter để tự động lưu user và session vào database
  // Điều này rất hữu ích cho OAuth, khi user đăng nhập lần đầu, nó sẽ tự tạo record user trong DB
  adapter: PrismaAdapter(prisma),

  providers: [
    GitHubProvider({
      clientId: process.env.GITHUB_ID as string,
      clientSecret: process.env.GITHUB_SECRET as string,
    }),
    CredentialsProvider({
      name: "Credentials",
      credentials: {
        email: {
          label: "Email",
          type: "email",
          placeholder: "jsmith@example.com",
        },
        password: { label: "Password", type: "password" },
      },
      // Logic xác thực nằm ở đây
      async authorize(credentials, req) {
        if (!credentials?.email || !credentials.password) {
          return null;
        }

        // 1. Tìm user trong database bằng email
        const user = await prisma.user.findUnique({
          where: { email: credentials.email },
        });

        if (!user) {
          return null;
        }

        // Cần thêm cột password vào model User trong prisma.schema
        // Giả sử user.password là mật khẩu đã được băm
        if (!user.password) {
          // User đăng nhập bằng OAuth, không có password
          return null;
        }

        // 2. So sánh mật khẩu người dùng nhập với mật khẩu đã băm trong DB
        const isPasswordValid = await bcrypt.compare(
          credentials.password,
          user.password
        );

        if (!isPasswordValid) {
          return null;
        }

        // 3. Nếu mọi thứ hợp lệ, trả về object user
        // Auth.js sẽ tự động tạo session và JWT
        return {
          id: user.id.toString(), // id phải là string
          name: user.name,
          email: user.email,
        };
      },
    }),
  ],

  // Quan trọng: Chỉ định chiến lược session là "jwt" khi dùng CredentialsProvider
  session: {
    strategy: "jwt",
  },

  // (Tùy chọn) Thêm thông tin vào JWT và session
  callbacks: {
    async jwt({ token, user }) {
      if (user) {
        token.id = user.id;
      }
      return token;
    },
    async session({ session, token }) {
      if (session.user) {
        // TypeScript cần bạn định nghĩa lại kiểu Session để thêm thuộc tính `id`
        (session.user as any).id = token.id;
      }
      return session;
    },
  },

  secret: process.env.NEXTAUTH_SECRET,
};
```

**Lưu ý:** Bạn cần cập nhật `schema.prisma` để thêm trường `password` (optional) cho `User` và các model khác mà `PrismaAdapter` yêu cầu (`Account`, `Session`, `VerificationToken`), sau đó chạy `npx prisma migrate dev`. Auth.js docs có hướng dẫn chi tiết về schema này.

---

**4. Hiển Thị Trạng Thái Đăng Nhập ở Client**

Để các Client Component có thể truy cập thông tin session (`useSession`), chúng ta cần bọc ứng dụng trong một `SessionProvider`.

Tạo một component provider riêng:

```typescript jsx
// src/components/AuthProvider.tsx
"use client";

import { SessionProvider } from "next-auth/react";
import React from "react";

interface AuthProviderProps {
  children: React.ReactNode;
}

export default function AuthProvider({ children }: AuthProviderProps) {
  return <SessionProvider>{children}</SessionProvider>;
}
```

Sau đó, sử dụng nó trong `RootLayout`:

```typescript jsx
// src/app/layout.tsx
import AuthProvider from "@/components/AuthProvider";
// ... các import khác

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>
        <AuthProvider>
          {/* Các component khác như Header, Navbar... */}
          <main>{children}</main>
        </AuthProvider>
      </body>
    </html>
  );
}
```

Bây giờ, bạn có thể tạo một component để hiển thị nút Login/Logout:

```typescript jsx
// src/components/AuthButton.tsx
"use client";

import { useSession, signIn, signOut } from "next-auth/react";
import Link from "next/link";

export default function AuthButton() {
  const { data: session, status } = useSession();

  if (status === "loading") {
    return <p>Loading...</p>;
  }

  if (session) {
    return (
      <div>
        <span>Welcome, {session.user?.name}!</span>
        <button onClick={() => signOut()}>Sign Out</button>
        <Link href="/dashboard">Dashboard</Link>
      </div>
    );
  }

  return (
    <div>
      <span>You are not signed in.</span>
      <button onClick={() => signIn()}>Sign In</button> {/* Mở trang đăng nhập mặc định */}
    </div>
  );
}
```

---

**5. Bảo Vệ Routes và API**

Đây là mục đích cuối cùng của việc xác thực.

**a) Bảo vệ một trang (Server Component)**

Giả sử bạn có trang `/dashboard` chỉ dành cho người dùng đã đăng nhập.

```typescript jsx
// src/app/dashboard/page.tsx
import { getServerSession } from "next-auth";
import { authOptions } from "@/lib/auth";
import { redirect } from "next/navigation";

export default async function DashboardPage() {
  // Lấy session phía server
  const session = await getServerSession(authOptions);

  // Nếu không có session, chuyển hướng về trang chủ
  if (!session) {
    redirect("/api/auth/signin?callbackUrl=/dashboard");
  }

  return (
    <div>
      <h1>Welcome to your Dashboard, {session.user?.name}!</h1>
      <p>Your user ID is: {(session.user as any)?.id}</p>
      <p>This page is protected on the server.</p>
    </div>
  );
}
```

**b) Bảo vệ một API Route**

Hãy refactor lại API `POST /api/posts` để chỉ người dùng đã đăng nhập mới có thể tạo bài viết, và tự động lấy `authorId` từ session của họ.

```typescript
// src/app/api/posts/route.ts
// ... imports
import { getServerSession } from "next-auth";
import { authOptions } from "@/lib/auth";

export async function POST(request: NextRequest) {
  // 1. Lấy session từ request
  const session = await getServerSession(authOptions);

  // 2. Kiểm tra xem người dùng đã đăng nhập chưa
  if (!session || !session.user) {
    return NextResponse.json({ message: "Unauthorized" }, { status: 401 });
  }

  // 3. Lấy ID của người dùng từ session
  // (session.user as any).id là do chúng ta đã thêm nó vào trong callback của authOptions
  const userId = (session.user as any)?.id;

  if (!userId) {
    return NextResponse.json(
      { message: "User ID not found in session" },
      { status: 400 }
    );
  }

  // ... (phần code validation với Zod vẫn giữ nguyên, nhưng bỏ authorId ra)
  const createPostSchema = z.object({
    title: z.string().min(3),
    body: z.string().min(10),
  });

  try {
    const body = await request.json();
    const validation = createPostSchema.safeParse(body);
    // ...

    // 4. Sử dụng userId từ session để tạo post
    const newPost = await prisma.post.create({
      data: {
        title: validation.data.title,
        body: validation.data.body,
        authorId: userId, // Tự động điền ID của người dùng đang đăng nhập!
      },
    });

    return NextResponse.json(newPost, { status: 201 });
  } catch (error) {
    // ...
  }
}
```

---

**Tạm Kết Phần 7:**

Chúng ta đã triển khai một hệ thống xác thực hoàn chỉnh và chuyên nghiệp cho ứng dụng Next.js:

- **Setup Auth.js:** Cấu hình thư viện với `NEXTAUTH_SECRET` và route handler.
- **Thêm Providers:** Tích hợp cả luồng đăng nhập OAuth (GitHub) và luồng Credentials (Email/Password) truyền thống.
- **Băm mật khẩu:** Học cách sử dụng `bcrypt` để bảo vệ mật khẩu người dùng.
- **Quản lý Session:** Sử dụng `SessionProvider` và hook `useSession` để Client Components nhận biết trạng thái đăng nhập.
- **Truy cập Session trên Server:** Dùng `getServerSession` để lấy thông tin người dùng trong Server Components và API Routes.
- **Bảo vệ tài nguyên:** Triển khai logic bảo vệ cho cả trang (dùng `redirect`) và API endpoint (trả về lỗi 401).

## Với hệ thống database và authentication đã sẵn sàng, ứng dụng của bạn giờ đây đã có đủ "xương sống" của một ứng dụng full-stack thực thụ. Phần tiếp theo, chúng ta sẽ tập trung vào việc cải thiện trải nghiệm người dùng và các kỹ thuật nâng cao hơn, ví dụ như **Xử lý Form phức tạp và Upload File.**

---

**Tài liệu React & Next.js "Khủng Khiếp" với TypeScript (Phần 8/10): Tương Tác Nâng Cao - Forms, Validation & File Uploads**

---

**Giới thiệu:**

Ở Phần 3, chúng ta đã học về "Controlled Components" để xử lý form cơ bản. Tuy nhiên, khi ứng dụng lớn lên, việc quản lý trạng thái form (giá trị, lỗi, trạng thái submitting...) một cách thủ công có thể trở nên rất rườm rà và dễ phát sinh lỗi. Hơn nữa, việc cho phép người dùng tải lên file (ví dụ: ảnh đại diện, ảnh sản phẩm) là một yêu cầu cực kỳ phổ biến nhưng lại đòi hỏi xử lý cả ở client và server.

Phần này sẽ giới thiệu các công cụ và kỹ thuật hiện đại để giải quyết những vấn đề này một cách hiệu quả:

1.  **React Hook Form:** Một thư viện mạnh mẽ để quản lý form với hiệu năng cao và ít code hơn.
2.  **Zod:** Một thư viện validation TypeScript-first để định nghĩa và kiểm tra dữ liệu một cách nhất quán ở cả client và server.
3.  **File Uploads:** Xây dựng luồng tải file lên một dịch vụ lưu trữ đám mây (ví dụ: Cloudinary hoặc Vercel Blob) thông qua API route của Next.js.

**So sánh với SvelteKit:** SvelteKit có `use:enhance` và `actions` cung cấp một giải pháp tích hợp sẵn rất mạnh mẽ cho progressive enhancement form. Trong React, chúng ta thường dựa vào các thư viện chuyên dụng như React Hook Form để đạt được trải nghiệm tương tự. Việc kết hợp React Hook Form với Zod tạo ra một "bộ đôi hoàn hảo" cho việc quản lý form an toàn kiểu, tương tự như cách SvelteKit actions có thể dùng Zod để validate `FormData`.

---

**1. Quản Lý Form Nâng Cao với React Hook Form & Zod**

Hãy bắt đầu bằng cách cài đặt các thư viện cần thiết.

```bash
npm install react-hook-form zod @hookform/resolvers
```

- `react-hook-form`: Thư viện chính.
- `zod`: Để định nghĩa schema validation.
- `@hookform/resolvers`: "Cầu nối" để React Hook Form có thể sử dụng schema của Zod.

Bây giờ, hãy refactor lại form tạo bài viết (`POST /api/posts`) mà chúng ta đã làm.

**a) Định Nghĩa Schema Validation với Zod**

Chúng ta có thể tái sử dụng schema đã định nghĩa ở API route (Phần 6) để đảm bảo validation nhất quán.

```typescript
// src/lib/validations/post.ts
import { z } from "zod";

// Schema này có thể được import và sử dụng ở cả client và server
export const postFormSchema = z.object({
  title: z
    .string()
    .min(3, "Title must be at least 3 characters long.")
    .max(128, "Title must be no longer than 128 characters."),
  body: z.string().min(10, "Body must be at least 10 characters long."),
});

// TypeScript type được suy luận từ schema
export type PostFormData = z.infer<typeof postFormSchema>;
```

**b) Xây Dựng Form ở Client**

Tạo một component Client để chứa form tạo bài viết.

```typescript jsx
// src/components/PostCreateForm.tsx
"use client";

import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { postFormSchema, PostFormData } from "@/lib/validations/post";
import { useRouter } from "next/navigation";
import { useState } from "react";

export default function PostCreateForm() {
  const router = useRouter();
  const [serverError, setServerError] = useState<string | null>(null);

  const {
    register,
    handleSubmit,
    formState: { errors, isSubmitting },
  } = useForm<PostFormData>({
    resolver: zodResolver(postFormSchema),
    // (Tùy chọn) Giá trị mặc định
    // defaultValues: { title: '', body: '' }
  });

  const onSubmit = async (data: PostFormData) => {
    setServerError(null);
    try {
      const response = await fetch("/api/posts", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data),
      });

      if (!response.ok) {
        const errorData = await response.json();
        throw new Error(errorData.message || "Failed to create post.");
      }

      // Xử lý thành công
      console.log("Post created successfully!");
      router.push("/dashboard"); // Chuyển hướng đến trang dashboard
      router.refresh(); // Yêu cầu server re-fetch dữ liệu cho route hiện tại
    } catch (error: any) {
      setServerError(error.message);
    }
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)} className="space-y-4">
      <h2>Create a New Post</h2>

      <div>
        <label htmlFor="title" className="block text-sm font-medium">
          Title
        </label>
        <input
          id="title"
          type="text"
          {...register("title")} // Đăng ký input với React Hook Form
          className="mt-1 block w-full rounded-md border-gray-300 shadow-sm"
          disabled={isSubmitting}
        />
        {errors.title && (
          <p className="mt-1 text-sm text-red-600">{errors.title.message}</p>
        )}
      </div>

      <div>
        <label htmlFor="body" className="block text-sm font-medium">
          Body
        </label>
        <textarea
          id="body"
          {...register("body")}
          className="mt-1 block w-full rounded-md border-gray-300 shadow-sm"
          rows={5}
          disabled={isSubmitting}
        />
        {errors.body && (
          <p className="mt-1 text-sm text-red-600">{errors.body.message}</p>
        )}
      </div>

      {serverError && (
        <p className="text-sm text-red-600">Server Error: {serverError}</p>
      )}

      <button
        type="submit"
        disabled={isSubmitting}
        className="inline-flex justify-center rounded-md border border-transparent bg-indigo-600 py-2 px-4 text-sm font-medium text-white shadow-sm hover:bg-indigo-700 disabled:opacity-50"
      >
        {isSubmitting ? "Creating..." : "Create Post"}
      </button>
    </form>
  );
}
```

**Tại sao React Hook Form lại mạnh mẽ?**

- **Ít Re-render:** Nó quản lý state nội tại và chỉ re-render khi cần thiết, giúp form nhanh hơn.
- **Ít Code:** Bạn không cần `useState` cho mỗi trường input. Chỉ cần `register` là đủ.
- **Validation Dễ Dàng:** Tích hợp mượt mà với Zod để tự động hiển thị lỗi.
- **Quản Lý Trạng Thái:** Cung cấp sẵn các state hữu ích như `isSubmitting`, `isValid`, `isDirty`.

---

**2. Xử Lý File Uploads**

Đây là một quy trình nhiều bước, nhưng chúng ta sẽ chia nhỏ nó ra.

**Luồng hoạt động:**

1.  **Client:** Người dùng chọn file từ máy.
2.  **Client:** Gửi một request đến API route của Next.js, yêu cầu một "URL ký sẵn" (presigned URL) để upload file lên dịch vụ lưu trữ đám mây.
3.  **Server (API Route):** Nhận request, xác thực người dùng, tạo một URL ký sẵn có thời hạn từ dịch vụ lưu trữ (ví dụ: Cloudinary, S3, Vercel Blob) và trả về cho client.
4.  **Client:** Nhận URL ký sẵn và dùng nó để upload trực tiếp file lên dịch vụ lưu trữ. **File không đi qua server Next.js của bạn**, giúp giảm tải cho server.
5.  **Client:** Sau khi upload thành công, gửi một request khác đến server để lưu URL của file vừa upload vào database (ví dụ: cập nhật trường `imageUrl` của `Post` hoặc `User`).

Chúng ta sẽ dùng **Vercel Blob** vì nó tích hợp dễ dàng với Next.js và Vercel.

**a) Cài Đặt Vercel Blob**

```bash
npm install @vercel/blob
```

Kết nối project của bạn với Vercel Blob bằng cách chạy lệnh sau và làm theo hướng dẫn:

```bash
npx @vercel/blob link
```

Lệnh này sẽ tạo một kho lưu trữ Blob và thêm các biến môi trường cần thiết (`BLOB_READ_WRITE_TOKEN`) vào file `.env.local` và project Vercel của bạn.

**b) Tạo API Route để Upload**

File này sẽ nhận file từ client và upload nó lên Vercel Blob.

```typescript
// src/app/api/upload/route.ts
import { put } from "@vercel/blob";
import { NextResponse } from "next/server";
import { getServerSession } from "next-auth";
import { authOptions } from "@/lib/auth";

export async function POST(request: Request): Promise<NextResponse> {
  const session = await getServerSession(authOptions);
  if (!session) {
    return NextResponse.json({ error: "Unauthorized" }, { status: 401 });
  }

  const { searchParams } = new URL(request.url);
  const filename = searchParams.get("filename");

  if (!filename || !request.body) {
    return NextResponse.json(
      { error: "No filename or file body provided." },
      { status: 400 }
    );
  }

  // `put` sẽ upload file lên Vercel Blob
  // `request.body` là một ReadableStream chứa dữ liệu file
  const blob = await put(filename, request.body, {
    access: "public", // Cho phép truy cập công khai file sau khi upload
  });

  // Trả về thông tin của blob đã được upload, bao gồm cả URL công khai
  return NextResponse.json(blob);
}
```

**c) Xây Dựng Component Upload ở Client**

Component này sẽ xử lý việc chọn file và gọi API `/api/upload`.

```typescript jsx
// src/components/ImageUploader.tsx
"use client";

import { useState, useRef } from "react";
import type { PutBlobResult } from "@vercel/blob";

interface ImageUploaderProps {
  onUploadComplete: (blob: PutBlobResult) => void;
}

export default function ImageUploader({
  onUploadComplete,
}: ImageUploaderProps) {
  const inputFileRef = useRef<HTMLInputElement>(null);
  const [blob, setBlob] = useState<PutBlobResult | null>(null);
  const [uploading, setUploading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  const handleUpload = async (event: React.FormEvent) => {
    event.preventDefault();
    setError(null);

    if (!inputFileRef.current?.files) {
      throw new Error("No file selected");
    }

    const file = inputFileRef.current.files[0];
    setUploading(true);

    try {
      const response = await fetch(`/api/upload?filename=${file.name}`, {
        method: "POST",
        body: file, // Gửi trực tiếp đối tượng File
      });

      if (!response.ok) {
        const errorData = await response.json();
        throw new Error(errorData.error || "Failed to upload file.");
      }

      const newBlob = (await response.json()) as PutBlobResult;
      setBlob(newBlob);
      onUploadComplete(newBlob); // Gọi callback để component cha xử lý
    } catch (err: any) {
      setError(err.message);
    } finally {
      setUploading(false);
    }
  };

  return (
    <div>
      <form onSubmit={handleUpload}>
        <input name="file" ref={inputFileRef} type="file" required />
        <button type="submit" disabled={uploading}>
          {uploading ? "Uploading..." : "Upload"}
        </button>
      </form>

      {error && <p className="text-red-500">Error: {error}</p>}

      {blob && (
        <div>
          <p>Upload successful!</p>
          <img src={blob.url} alt="Uploaded image" width={200} />
          <a href={blob.url} target="_blank" rel="noopener noreferrer">
            View Image
          </a>
        </div>
      )}
    </div>
  );
}
```

**d) Tích Hợp Uploader vào Form Tạo Bài Viết**

Bây giờ chúng ta cần cập nhật model `Post` để có trường `imageUrl` và tích hợp `ImageUploader` vào `PostCreateForm`.

1.  **Cập nhật `schema.prisma`:**

    ```prisma
    model Post {
      // ... các trường khác
      imageUrl String? // Thêm trường lưu URL ảnh, optional
    }
    ```

    Chạy `npx prisma migrate dev --name add_post_image_url`

2.  **Cập nhật `PostCreateForm.tsx`:**

    ```typescript jsx
    // src/components/PostCreateForm.tsx
    // ... imports
    import ImageUploader from "./ImageUploader";
    import type { PutBlobResult } from "@vercel/blob";

    export default function PostCreateForm() {
      // ... (useRouter, useState...)
      const [uploadedImage, setUploadedImage] = useState<PutBlobResult | null>(
        null
      );

      // ... (useForm hook)

      const onUploadComplete = (blob: PutBlobResult) => {
        setUploadedImage(blob);
      };

      const onSubmit = async (data: PostFormData) => {
        // ... (try/catch block)
        const payload = {
          ...data,
          imageUrl: uploadedImage?.url, // Thêm imageUrl vào payload gửi đi
        };
        const response = await fetch("/api/posts", {
          // ...
          body: JSON.stringify(payload),
        });
        // ...
      };

      return (
        <form onSubmit={handleSubmit(onSubmit)} className="space-y-6">
          {/* ... các input title, body ... */}

          <div>
            <label className="block text-sm font-medium">Cover Image</label>
            <ImageUploader onUploadComplete={onUploadComplete} />
          </div>

          {/* ... nút submit ... */}
        </form>
      );
    }
    ```

3.  **Cập nhật API `/api/posts` (POST handler):**

    ```typescript
    // src/app/api/posts/route.ts
    // ...
    const createPostSchema = z.object({
      title: z.string().min(3),
      body: z.string().min(10),
      imageUrl: z.string().url().optional(), // Thêm imageUrl vào schema validation
    });

    // ... trong POST handler
    const newPost = await prisma.post.create({
      data: {
        title: validation.data.title,
        body: validation.data.body,
        imageUrl: validation.data.imageUrl, // Lưu URL vào database
        authorId: userId,
      },
    });
    ```

---

**Tạm Kết Phần 8:**

Chúng ta đã trang bị cho ứng dụng của mình những công cụ và kỹ thuật mạnh mẽ để xử lý các tương tác phức tạp từ người dùng:

- **React Hook Form & Zod:** Tạo ra một quy trình xử lý form hiệu quả, an toàn kiểu và ít code, giúp quản lý trạng thái và validation một cách chuyên nghiệp.
- **Luồng Upload File Hiện Đại:** Xây dựng một hệ thống upload file an toàn và hiệu quả bằng cách sử dụng "presigned URLs" (Vercel Blob tự động xử lý điều này), giảm tải cho server ứng dụng.
- **Tích hợp:** Kết hợp cả hai kỹ thuật để tạo ra một form hoàn chỉnh cho phép người dùng nhập dữ liệu văn bản và tải lên file ảnh.

## Với những kỹ năng này, bạn đã có thể xây dựng phần lớn các tính năng tương tác mà một ứng dụng web hiện đại yêu cầu. Phần tiếp theo, chúng ta sẽ khám phá các chủ đề về **State Management ở quy mô lớn và tối ưu hóa phía client,** để đảm bảo ứng dụng của bạn không chỉ mạnh mẽ mà còn nhanh và mượt.

---

**Tài liệu React & Next.js "Khủng Khiếp" với TypeScript (Phần 9/10): Quản Lý State Nâng Cao & Tối Ưu Phía Client**

---

**Giới thiệu:**

Ứng dụng của chúng ta giờ đã có database, xác thực, form phức tạp và upload file. Khi các tính năng này kết hợp với nhau, một câu hỏi lớn sẽ xuất hiện: **Làm thế nào để quản lý state một cách nhất quán và hiệu quả trên toàn bộ ứng dụng?**

- Làm sao để component A biết khi nào một hành động ở component B đã xảy ra (ví dụ: thêm sản phẩm vào giỏ hàng)?
- Làm sao để giữ cho dữ liệu hiển thị trên client luôn đồng bộ với dữ liệu trên server mà không cần refresh trang liên tục?
- Làm sao để tránh việc re-render toàn bộ ứng dụng mỗi khi một mẩu state nhỏ thay đổi?

Phần này sẽ giải quyết các vấn đề trên bằng cách phân biệt hai loại state chính và giới thiệu các công cụ hàng đầu cho từng loại:

1.  **Client State:** Trạng thái tồn tại hoàn toàn trên UI (ví dụ: sidebar đang mở hay đóng, theme sáng/tối, nội dung giỏ hàng). Chúng ta sẽ dùng **Zustand** để quản lý.
2.  **Server State:** Dữ liệu tồn tại trên server và chúng ta chỉ đang "cache" hoặc "đồng bộ hóa" một bản sao của nó trên client (ví dụ: danh sách bài viết, thông tin người dùng). Chúng ta sẽ dùng **SWR** để quản lý.

**So sánh với SvelteKit:** Svelte Stores (`writable`, `readable`) là một công cụ tuyệt vời và đơn giản để quản lý cả hai loại state này. Tuy nhiên, trong hệ sinh thái React, cộng đồng đã phát triển các công cụ chuyên biệt hơn. Zustand có thể coi là một phiên bản "Svelte Stores cho React" về độ đơn giản, trong khi SWR/React Query cung cấp các tính năng tự động mạnh mẽ (như revalidation, caching) dành riêng cho việc xử lý "Server State" mà Svelte Stores không có sẵn "out-of-the-box".

---

**1. Vượt Qua Giới Hạn của `useContext` - Tại Sao Cần Thư Viện State Management?**

Ở Phần 2, chúng ta đã học về `useContext`. Nó rất tốt để truyền dữ liệu "ít thay đổi" xuống sâu trong cây component. Nhưng nó có một nhược điểm lớn về hiệu năng khi dùng cho state thay đổi thường xuyên:

**Bất cứ khi nào giá trị của Context thay đổi, TẤT CẢ các component con sử dụng `useContext` đó sẽ bị re-render, ngay cả khi chúng không quan tâm đến phần dữ liệu đã thay đổi.**

Hãy tưởng tượng một `AppContext` chứa cả `theme` và `shoppingCart`. Khi bạn thêm một sản phẩm vào giỏ hàng, component chỉ quan tâm đến `theme` cũng sẽ bị re-render một cách không cần thiết. Đây là lúc các thư viện chuyên dụng tỏa sáng.

---

**2. Zustand - State Toàn Cục (Client State) Đơn Giản và Hiệu Quả**

Zustand là một thư viện nhỏ, nhanh và linh hoạt. Nó sử dụng hooks và cho phép bạn tạo các "store" độc lập. Điểm mạnh nhất của nó là các component có thể "subscribe" vào chính xác phần state mà chúng cần, tránh được các lần re-render không cần thiết.

**a) Cài đặt Zustand**

```bash
npm install zustand
```

**b) Tạo một Store**

Hãy tạo một store đơn giản để quản lý trạng thái của UI, ví dụ như một sidebar có thể đóng/mở.

```typescript
// src/stores/uiStore.ts
import { create } from "zustand";

interface UiState {
  isSidebarOpen: boolean;
  toggleSidebar: () => void;
  openSidebar: () => void;
  closeSidebar: () => void;
}

export const useUiStore = create<UiState>((set) => ({
  isSidebarOpen: true, // Giá trị khởi tạo
  toggleSidebar: () =>
    set((state) => ({ isSidebarOpen: !state.isSidebarOpen })),
  openSidebar: () => set({ isSidebarOpen: true }),
  closeSidebar: () => set({ isSidebarOpen: false }),
}));
```

**c) Sử dụng Store trong các Component**

Bây giờ, bất kỳ Client Component nào cũng có thể sử dụng store này.

```typescript jsx
// src/components/Sidebar.tsx
"use client";
import { useUiStore } from "@/stores/uiStore";

export default function Sidebar() {
  // Chỉ subscribe vào `isSidebarOpen`.
  // Component này sẽ CHỈ re-render khi `isSidebarOpen` thay đổi.
  const isSidebarOpen = useUiStore((state) => state.isSidebarOpen);

  if (!isSidebarOpen) {
    return null; // Hoặc render một sidebar thu gọn
  }

  return (
    <aside className="w-64 bg-gray-100 p-4">
      <h2 className="font-bold">Sidebar</h2>
      <ul>
        <li>Link 1</li>
        <li>Link 2</li>
      </ul>
    </aside>
  );
}
```

```typescript jsx
// src/components/Header.tsx
"use client";
import { useUiStore } from "@/stores/uiStore";

export default function Header() {
  // Chỉ subscribe vào action `toggleSidebar`.
  // Vì action này không bao giờ thay đổi, component này sẽ không bao giờ re-render
  // một cách không cần thiết do state trong store thay đổi.
  const toggleSidebar = useUiStore((state) => state.toggleSidebar);

  return (
    <header className="flex items-center justify-between bg-blue-600 p-4 text-white">
      <h1 className="text-xl font-bold">My App</h1>
      <button onClick={toggleSidebar}>Toggle Sidebar</button>
    </header>
  );
}
```

**Zustand là lựa chọn hoàn hảo cho:**

- Trạng thái UI toàn cục (modals, sidebars, notifications).
- Trạng thái giỏ hàng trong trang thương mại điện tử.
- Bất kỳ state nào cần được chia sẻ giữa các component không có quan hệ cha-con trực tiếp.

---

**3. Đồng Bộ Hóa Trạng Thái Server với SWR**

**Vấn đề:** Dữ liệu từ API của chúng ta (ví dụ: danh sách bài viết) là "Server State". Nó có thể bị thay đổi bởi người dùng khác, hoặc hết hạn. Làm thế nào để UI của chúng ta luôn "tươi mới"?

**SWR** (Stale-While-Revalidate) là một thư viện hook data fetching mạnh mẽ từ Vercel. Nó giúp việc fetching, caching, và re-fetching dữ liệu phía client trở nên cực kỳ đơn giản.

**a) Cài đặt SWR**

```bash
npm install swr
```

**b) Tạo một Custom Hook để Fetch Dữ Liệu**

Tạo một hook tái sử dụng để fetch danh sách bài viết từ API `/api/posts` của chúng ta.

```typescript
// src/hooks/usePosts.ts
"use client";
import useSWR from "swr";

// Định nghĩa kiểu Post cho nhất quán
export interface PostWithAuthor {
  id: number;
  title: string;
  body: string;
  imageUrl: string | null;
  author: {
    name: string | null;
    email: string | null;
  };
}

// Hàm fetcher: một hàm đơn giản nhận URL và trả về dữ liệu JSON
const fetcher = (url: string) => fetch(url).then((res) => res.json());

export function usePosts() {
  const { data, error, isLoading, mutate } = useSWR<PostWithAuthor[]>(
    "/api/posts",
    fetcher
  );

  return {
    posts: data,
    error,
    isLoading,
    mutate, // Hàm để trigger re-fetch thủ công
  };
}
```

**c) Sử dụng Hook trong Component và Thực Hiện Mutation**

Bây giờ, hãy tạo một component hiển thị danh sách bài viết và cho phép xóa một bài viết.

```typescript jsx
// src/components/PostList.tsx
"use client";

import { usePosts, PostWithAuthor } from "@/hooks/usePosts";

export default function PostList() {
  const { posts, error, isLoading, mutate } = usePosts();

  const handleDelete = async (postId: number) => {
    // Tùy chọn: Optimistic UI - Cập nhật UI trước khi request server hoàn tất
    // mutate(currentPosts => currentPosts?.filter(p => p.id !== postId), false);

    try {
      const response = await fetch(`/api/posts/${postId}`, {
        method: "DELETE",
      });
      if (!response.ok) {
        throw new Error("Failed to delete post.");
        // Nếu lỗi, SWR có thể rollback lại state (nếu dùng optimistic UI)
      }

      // Sau khi xóa thành công trên server, báo cho SWR re-fetch lại dữ liệu
      // để đảm bảo UI đồng bộ với server.
      mutate();
    } catch (err: any) {
      console.error(err.message);
      // Nếu có lỗi, cũng có thể trigger re-fetch để lấy lại state đúng từ server
      // mutate();
    }
  };

  if (isLoading) return <div>Loading posts...</div>;
  if (error) return <div>Failed to load posts.</div>;
  if (!posts || posts.length === 0) return <div>No posts found.</div>;

  return (
    <div className="space-y-4">
      {posts.map((post) => (
        <div key={post.id} className="border p-4 rounded-md">
          <h3 className="font-bold text-lg">{post.title}</h3>
          <p>By {post.author.name || "Unknown"}</p>
          <button
            onClick={() => handleDelete(post.id)}
            className="mt-2 text-sm text-red-600"
          >
            Delete
          </button>
        </div>
      ))}
    </div>
  );
}
```

_Lưu ý: Bạn cần tạo API Route cho `DELETE /api/posts/[postId]` để logic xóa hoạt động._

**Sức mạnh của SWR:**

- **Caching:** Tự động cache kết quả fetch.
- **Revalidation on Focus:** Khi người dùng chuyển tab và quay lại, SWR sẽ tự động re-fetch để đảm bảo dữ liệu mới nhất.
- **Revalidation on Interval:** Có thể cấu hình để tự động re-fetch sau một khoảng thời gian.
- **Mutation và UI Sync:** `mutate` là một công cụ cực kỳ mạnh mẽ để cập nhật UI sau khi thực hiện các hành động (POST, PUT, DELETE).
- **Optimistic UI:** Cập nhật giao diện người dùng ngay lập-tức, tạo cảm giác ứng dụng cực nhanh.

---

**4. Khi Nào Cần Đến "Hàng Khủng" - Redux Toolkit?**

Nếu Zustand là một chiếc xe tay ga nhanh nhẹn, thì Redux Toolkit (RTK) là một chiếc xe tải hạng nặng. Bạn chỉ nên dùng nó khi thực sự cần.

**Sử dụng Redux Toolkit khi:**

- Bạn có một state cực kỳ phức tạp với nhiều logic lồng vào nhau.
- Bạn cần một luồng dữ liệu một chiều (one-way data flow) cực kỳ chặt chẽ và dễ đoán.
- Bạn cần các tính năng nâng cao như time-travel debugging với Redux DevTools.
- Dự án của bạn rất lớn, có nhiều lập trình viên cùng làm việc và cần một cấu trúc state chuẩn hóa.

Đối với hầu hết các dự án, sự kết hợp giữa **Zustand (cho Client State)** và **SWR/React Query (cho Server State)** là một giải pháp hiện đại, hiệu quả và dễ bảo trì hơn.

---

**Tạm Kết Phần 9:**

Chúng ta đã học cách phân loại và quản lý state trong một ứng dụng React/Next.js hiện đại, vượt ra ngoài `useState` và `useContext`:

- **Phân biệt Client State và Server State:** Đây là chìa khóa để lựa chọn công cụ phù hợp.
- **Zustand:** Giải pháp đơn giản, hiệu quả cho state toàn cục phía client, giúp tối ưu hóa re-render.
- **SWR:** Công cụ mạnh mẽ để đồng bộ hóa, cache và mutate "Server State", giúp ứng dụng của bạn luôn "tươi mới" và phản hồi nhanh.

Bằng cách áp dụng những kỹ thuật này, bạn có thể xây dựng các ứng dụng phức tạp mà vẫn duy trì được hiệu năng cao và trải nghiệm người dùng mượt mà.

## Trong **phần cuối cùng (Phần 10)**, chúng ta sẽ hoàn thiện ứng dụng bằng cách tập trung vào **Kiểm Thử (Testing), CI/CD và các chiến lược Deployment nâng cao,** đảm bảo ứng dụng của bạn không chỉ chạy tốt trên máy local mà còn ổn định và đáng tin cậy trong môi trường production.

**Tài liệu React & Next.js "Khủng Khiếp" với TypeScript (Phần 10/11): Các Tiện Ích Cốt Lõi - Tối Ưu Hóa Assets & Navigation**

---

**Giới thiệu:**

Next.js không chỉ là một framework định tuyến và rendering; nó là một bộ công cụ (toolbox) được trang bị sẵn rất nhiều tiện ích mạnh mẽ để giải quyết các vấn đề phổ biến trong phát triển web. Việc sử dụng thành thạo các tiện ích này sẽ giúp ứng dụng của bạn không chỉ nhanh hơn, nhẹ hơn mà còn mang lại trải nghiệm người dùng tốt hơn và cải thiện điểm số SEO.

Phần này sẽ đi sâu vào các "helpers" và component được tích hợp sẵn, giúp bạn tự động hóa các tác vụ tối ưu hóa mà trước đây bạn có thể phải làm thủ công.

**So sánh với SvelteKit:** SvelteKit cũng có các cơ chế tối ưu hóa riêng, ví dụ như tự động xử lý preloading cho các link nội bộ. Tuy nhiên, Next.js cung cấp các component chuyên dụng và tường minh hơn như `<Image />` và `<Script />` để bạn có toàn quyền kiểm soát việc tối ưu hóa các loại assets khác nhau.

---

**1. `<Link>` - Điều Hướng Thông Minh và Nhanh Chóng (`next/link`)**

Bạn đã thấy `<Link>` trong các ví dụ trước, nhưng hãy tìm hiểu sâu hơn về "ma thuật" đằng sau nó.

**Vấn đề:** Thẻ `<a>` truyền thống khi được click sẽ gây ra một lần tải lại toàn bộ trang (full-page reload), làm mất state của ứng dụng và tạo ra trải nghiệm người dùng gián đoạn.

**Giải pháp của Next.js:** Component `<Link href="...">`

- **Client-Side Navigation:** Nó render ra một thẻ `<a>` trong HTML, nhưng xử lý sự kiện click bằng JavaScript để thay đổi URL và render component trang mới mà không cần tải lại trang.
- **Prefetching (Tải trước - Tính năng quan trọng nhất):** Ở môi trường production, khi một component `<Link>` xuất hiện trong tầm nhìn của người dùng (viewport), Next.js sẽ **tự động tải trước code JavaScript** của trang được liên kết trong nền. Khi người dùng click vào link, trang gần như được tải ngay lập tức. Đây là một trong những lý do chính khiến các ứng dụng Next.js cho cảm giác cực kỳ nhanh.

**Code Ví dụ:**

```typescript jsx
// src/components/Header.tsx
import Link from "next/link";
import { usePathname } from "next/navigation";

export default function Header() {
  const pathname = usePathname();

  return (
    <header>
      <nav>
        <ul className="flex gap-4">
          <li>
            {/* Link này sẽ được prefetch khi người dùng thấy nó */}
            <Link href="/" className={pathname === "/" ? "font-bold" : ""}>
              Home
            </Link>
          </li>
          <li>
            {/* Tắt prefetch nếu bạn không muốn (ví dụ: link đến trang rất nặng) */}
            <Link
              href="/blog"
              prefetch={false}
              className={pathname.startsWith("/blog") ? "font-bold" : ""}
            >
              Blog
            </Link>
          </li>
          <li>
            {/* `replace` sẽ thay thế lịch sử trình duyệt thay vì đẩy một entry mới */}
            <Link
              href="/about"
              replace
              className={pathname === "/about" ? "font-bold" : ""}
            >
              About
            </Link>
          </li>
        </ul>
      </nav>
    </header>
  );
}
```

---

**2. `<Image>` - Cuộc Cách Mạng Hình Ảnh (`next/image`)**

Đây là một trong những tiện ích mạnh mẽ và quan trọng nhất của Next.js. **Luôn sử dụng `<Image />` thay cho thẻ `<img>` thông thường.**

**Vấn đề của `<img>`:**

- **Kích thước file lớn:** Không tự động nén hoặc chuyển đổi sang định dạng hiện đại (như WebP).
- **Layout Shift (CLS):** Trình duyệt không biết kích thước ảnh cho đến khi nó được tải xong, gây ra hiện tượng nội dung trang "nhảy" lung tung khi ảnh xuất hiện.
- **Không lazy-load mặc định:** Tất cả ảnh trên trang đều được tải cùng lúc, ngay cả những ảnh nằm ngoài màn hình, làm chậm tốc độ tải ban đầu.

**Giải pháp của Next.js:** Component `<Image src="..." width="..." height="..." alt="..." />`

- **Tối ưu hóa tự động:** Tự động nén ảnh, thay đổi kích thước cho các thiết bị khác nhau và chuyển đổi sang các định dạng hiện đại như WebP (nếu trình duyệt hỗ trợ).
- **Chống Layout Shift:** Bắt buộc phải cung cấp `width` và `height`, giúp Next.js giữ đúng không gian cho ảnh trước khi nó được tải về.
- **Lazy Loading mặc định:** Ảnh sẽ chỉ được tải khi nó sắp cuộn vào tầm nhìn của người dùng.
- **Bảo vệ:** Tự động tối ưu hóa hình ảnh từ các nguồn bên ngoài, tránh việc hotlinking trực tiếp và tối ưu hóa băng thông.

**Code Ví dụ:**

**a) Hình ảnh Local (trong `src/` hoặc `public/`)**

```typescript jsx
import Image from "next/image";
import profilePic from "@/public/images/profile.png"; // Import ảnh

function ProfileCard() {
  return (
    <div>
      {/* Next.js sẽ tự động xử lý ảnh đã import */}
      <Image
        src={profilePic}
        alt="Profile Picture"
        width={200} // Bắt buộc
        height={200} // Bắt buộc
        priority // Dùng cho ảnh LCP (Largest Contentful Paint), ví dụ: banner. Ảnh này sẽ được tải trước.
        className="rounded-full"
      />
      <h2>John Doe</h2>
    </div>
  );
}
```

**b) Hình ảnh từ Nguồn Ngoài (Remote Images)**

Để sử dụng ảnh từ một URL bên ngoài, bạn cần phải cho Next.js biết domain đó là an toàn.

```javascript
// next.config.js
/** @type {import('next').NextConfig} */
const nextConfig = {
  images: {
    remotePatterns: [
      {
        protocol: "https",
        hostname: "images.unsplash.com", // Cho phép ảnh từ domain này
        port: "",
        pathname: "/**",
      },
      {
        protocol: "https",
        hostname: "cdn.example.com",
      },
    ],
  },
};

module.exports = nextConfig;
```

Sau đó, bạn có thể sử dụng URL trong component:

```typescript jsx
import Image from "next/image";

function BlogPost({ post }) {
  return (
    <article>
      <h1>{post.title}</h1>
      {/* Ảnh từ URL bên ngoài */}
      <Image
        src={post.imageUrl} // Ví dụ: "https://images.unsplash.com/..."
        alt={post.title}
        width={800}
        height={400}
        sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw" // Giúp Next.js chọn kích thước ảnh phù hợp
        className="object-cover"
      />
      <div>{post.body}</div>
    </article>
  );
}
```

---

**3. `next/font` - Tối Ưu Hóa Font Chữ Hoàn Hảo**

**Vấn đề:** Tải font từ các dịch vụ như Google Fonts có thể gây ra các yêu cầu mạng bổ sung và hiện tượng FOUT (Flash of Unstyled Text) hoặc FOIT (Flash of Invisible Text), ảnh hưởng đến CLS.

**Giải pháp của Next.js:** `next/font` cho phép bạn "self-host" (tự lưu trữ) font chữ một cách tối ưu.

- Tại thời điểm build, Next.js sẽ tải font về, tối ưu hóa và phục vụ nó cùng với các file static khác của bạn.
- Không có yêu cầu mạng bổ sung nào đến Google Fonts khi người dùng truy cập trang.
- Tự động tính toán các thuộc tính CSS `size-adjust` để loại bỏ hoàn toàn layout shift do font gây ra.

**Code Ví dụ:**

```typescript jsx
// src/app/layout.tsx
import type { Metadata } from "next";
import { Inter, Roboto_Mono } from "next/font/google"; // Import font từ Google
import "./globals.css";

// Cấu hình font
const inter = Inter({
  subsets: ["latin"],
  variable: "--font-inter", // Tạo CSS variable để sử dụng ở bất kỳ đâu
});

const robotoMono = Roboto_Mono({
  subsets: ["latin"],
  variable: "--font-roboto-mono",
  display: "swap",
});

export const metadata: Metadata = {
  title: "Create Next App",
  description: "Generated by create next app",
};

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    // Áp dụng các class của font vào thẻ html hoặc body
    <html lang="en" className={`${inter.variable} ${robotoMono.variable}`}>
      <body>{children}</body>
    </html>
  );
}
```

Sau đó trong file CSS của bạn (ví dụ `globals.css`):

```css
/* src/globals.css */
body {
  font-family: var(--font-inter), sans-serif;
}

code,
pre {
  font-family: var(--font-roboto-mono), monospace;
}
```

---

**4. `<Script>` - Quản Lý Script Bên Thứ Ba Một Cách An Toàn (`next/script`)**

**Vấn đề:** Các script từ bên thứ ba (Google Analytics, Facebook Pixel, Intercom chat widget...) thường là nguyên nhân chính làm chậm trang web vì chúng có thể chặn quá trình render.

**Giải pháp của Next.js:** Component `<Script>` cho phép bạn kiểm soát chính xác **khi nào** và **như thế nào** một script bên thứ ba được tải và thực thi.

**Các chiến lược (`strategy`) chính:**

- `strategy="beforeInteractive"`: Tải và thực thi trước khi trang có thể tương tác. Dùng cho các script cực kỳ quan trọng cần chạy sớm (ví dụ: cookie consent manager).
- `strategy="afterInteractive"` (mặc định): Tải và thực thi ngay sau khi trang có thể tương tác. Phù hợp cho hầu hết các script như tag managers, analytics.
- `strategy="lazyOnload"`: Tải khi trình duyệt không bận và sau khi tất cả tài nguyên khác đã được tải. Hoàn hảo cho các script không quan trọng như chat widgets, social media plugins.

**Code Ví dụ:**

```typescript jsx
// src/components/Analytics.tsx
import Script from "next/script";

export default function Analytics() {
  const GA_TRACKING_ID = "G-XXXXXXXXXX";

  return (
    <>
      {/* Tải script Google Analytics sau khi trang đã tương tác */}
      <Script
        strategy="afterInteractive"
        src={`https://www.googletagmanager.com/gtag/js?id=${GA_TRACKING_ID}`}
      />
      <Script
        id="gtag-init" // Cần đặt id khi viết script inline
        strategy="afterInteractive"
        dangerouslySetInnerHTML={{
          __html: `
            window.dataLayer = window.dataLayer || [];
            function gtag(){dataLayer.push(arguments);}
            gtag('js', new Date());
            gtag('config', '${GA_TRACKING_ID}', {
              page_path: window.location.pathname,
            });
          `,
        }}
      />
    </>
  );
}

// Sau đó, bạn có thể đặt <Analytics /> trong RootLayout.
```

---

**Tạm Kết Phần 10:**

Chúng ta đã khám phá 4 tiện ích cốt lõi giúp tối ưu hóa hiệu năng và trải nghiệm người dùng trong Next.js:

- **`<Link>`:** Giúp điều hướng nhanh như chớp nhờ prefetching.
- **`<Image>`:** Cách mạng hóa việc xử lý hình ảnh, giúp trang nhẹ hơn và không bị layout shift.
- **`next/font`:** Loại bỏ các vấn đề về hiệu năng và CLS liên quan đến font chữ.
- **`<Script>`:** Cung cấp toàn quyền kiểm soát việc tải các script bên thứ ba.

Việc nắm vững và áp dụng những tiện ích này là một bước tiến lớn trong việc xây dựng các ứng dụng Next.js chuyên nghiệp và hiệu năng cao.

## Trong **phần cuối cùng (Phần 11)**, chúng ta sẽ khám phá các tiện ích nâng cao hơn, mang tính kiến trúc hệ thống như **Middleware, Dynamic Imports, và các Server Helpers,** hoàn thiện bộ kỹ năng của bạn.

**Tài liệu React & Next.js "Khủng Khiếp" với TypeScript (Phần 11/11): Tiện Ích Nâng Cao - Middleware, Dynamic Imports & Server Functions**

---

**Giới thiệu:**

Bạn đã đi một chặng đường dài và nắm vững gần như toàn bộ hệ sinh thái của Next.js. Trong phần cuối cùng này, chúng ta sẽ khám phá những công cụ nâng cao cho phép bạn thực hiện các tác vụ phức tạp ở cấp độ kiến trúc hệ thống. Đây là những "con dao phẫu thuật" giúp bạn tinh chỉnh luồng request/response, tối ưu hóa JavaScript phía client một cách triệt để, và tương tác trực tiếp với các API của Next.js trên server.

Việc hiểu và sử dụng các tiện ích này sẽ giúp bạn giải quyết các bài toán như A/B testing, internationalization (i18n), bảo vệ route một cách linh hoạt, và đảm bảo ứng dụng của bạn chỉ tải những gì thực sự cần thiết.

**So sánh với SvelteKit:** Khái niệm `Middleware` trong Next.js rất giống với `hooks.server.ts` trong SvelteKit, cả hai đều cho phép bạn chặn và sửa đổi request trước khi nó đến được route handler. `Dynamic Imports` hoạt động tương tự ở cả hai framework. Các `Server Functions/Helpers` của Next.js cung cấp một cách tường minh để truy cập vào các thông tin request-time mà SvelteKit thường cung cấp qua các đối tượng như `event` hoặc store `$page`.

---

**1. Middleware - Chặn và Xử Lý Request ở "Cạnh" (Edge)**

**Middleware** là một đoạn code chạy **trước khi** một request được hoàn thành và đến được trang hoặc API route của bạn. Nó chạy trên **Edge Runtime**, một môi trường JavaScript nhẹ và siêu nhanh được phân phối toàn cầu, gần với người dùng hơn là server chính của bạn.

**Khi nào dùng Middleware?**

- **Xác thực:** Kiểm tra xem người dùng đã đăng nhập chưa và chuyển hướng họ nếu cần. (Đây là một cách khác để bảo vệ route, linh hoạt hơn việc kiểm tra trong từng layout/page).
- **A/B Testing:** Chuyển hướng người dùng đến các phiên bản khác nhau của một trang.
- **Internationalization (i18n) Routing:** Phát hiện ngôn ngữ của người dùng và chuyển hướng họ đến path phù hợp (ví dụ: `/en/about`, `/fr/about`).
- **Thêm header vào request/response.**
- **Theo dõi và phân tích (Analytics).**

**Cách tạo Middleware:**

Tạo một file `middleware.ts` (hoặc `.js`) trong thư mục `src/`.

```typescript
// src/middleware.ts
import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

// Hàm middleware
export function middleware(request: NextRequest) {
  // Ví dụ 1: Ghi log mọi request
  console.log(`[Middleware] Path: ${request.nextUrl.pathname}`);

  // Ví dụ 2: Thêm một header vào request
  const requestHeaders = new Headers(request.headers);
  requestHeaders.set("x-from-middleware", "true");

  // Ví dụ 3: Chuyển hướng nếu người dùng cố truy cập một trang bí mật
  if (request.nextUrl.pathname.startsWith("/secret-page")) {
    // Giả sử bạn có logic kiểm tra vai trò ở đây
    const isAdmin = false; // Logic thực tế sẽ kiểm tra token/cookie
    if (!isAdmin) {
      // Chuyển hướng về trang chủ
      return NextResponse.redirect(new URL("/", request.url));
    }
  }

  // Cho phép request tiếp tục
  return NextResponse.next({
    request: {
      headers: requestHeaders,
    },
  });
}

// Cấu hình matcher để chỉ định middleware chạy trên những path nào
export const config = {
  matcher: [
    /*
     * Khớp với tất cả các path, TRỪ những path bắt đầu bằng:
     * - api (API routes)
     * - _next/static (static files)
     * - _next/image (image optimization files)
     * - favicon.ico (favicon file)
     */
    "/((?!api|_next/static|_next/image|favicon.ico).*)",
  ],
};
```

**Quan trọng:** Middleware cho phép bạn thực thi logic chung cho một nhóm các route một cách tập trung và hiệu quả trước cả khi React bắt đầu render bất cứ thứ gì trên server.

---

**2. Dynamic Imports - Tối Ưu Hóa JavaScript Phía Client**

Mặc dù Next.js tự động tách code (code-splitting) cho từng trang, đôi khi bạn còn muốn tối ưu hóa sâu hơn nữa ngay trong một trang.

**Vấn đề:** Một trang có thể chứa các component rất nặng (ví dụ: một trình soạn thảo văn bản phức tạp, một thư viện biểu đồ lớn) nhưng không phải lúc nào người dùng cũng cần đến chúng ngay lập tức. Việc tải chúng cùng với bundle ban đầu sẽ làm tăng kích thước và thời gian tải trang.

**Giải pháp:** `next/dynamic` cho phép bạn "lazy-load" các component React. Component đó và các dependency của nó sẽ được tạo thành một chunk JavaScript riêng và chỉ được tải về khi nó sắp được render.

**Code Ví dụ:**

```typescript jsx
// src/app/dashboard/page.tsx
"use client"; // Trang này cần tương tác client để mở modal

import { useState } from "react";
import dynamic from "next/dynamic";

// --- Lazy-loading một component nặng ---
// Component này sẽ không được tải cho đến khi `showEditor` là true
const HeavyTextEditor = dynamic(() => import("@/components/HeavyTextEditor"), {
  loading: () => <p>Loading editor...</p>, // Hiển thị component loading
  ssr: false, // Tắt Server-Side Rendering cho component này nếu nó chỉ hoạt động trên client (ví dụ: dùng window)
});

// --- Component được import thông thường (để so sánh) ---
import SimpleComponent from "@/components/SimpleComponent";

export default function DashboardPage() {
  const [showEditor, setShowEditor] = useState(false);

  return (
    <div>
      <h1>My Dashboard</h1>
      <SimpleComponent />
      <hr className="my-4" />

      <button onClick={() => setShowEditor(true)}>Open Heavy Editor</button>

      {/* HeavyTextEditor chỉ được render (và tải JS) khi showEditor là true */}
      {showEditor && <HeavyTextEditor />}
    </div>
  );
}
```

Việc sử dụng `dynamic()` là một kỹ thuật cực kỳ hiệu quả để giảm kích thước bundle JavaScript ban đầu của các trang phức tạp, cải thiện đáng kể chỉ số First Contentful Paint (FCP) và Time to Interactive (TTI).

---

**3. Server-Only Functions & Helpers (`next/headers`, `next/cookies`)**

App Router cung cấp các hàm tiện ích cho phép bạn truy cập thông tin của request **bên trong Server Components** mà không cần truyền chúng xuống từ `page.tsx`.

**Vấn đề:** Trong Pages Router cũ, để lấy headers hoặc cookies, bạn phải dùng `getServerSideProps` và truyền chúng xuống làm props. Điều này khá cồng kềnh.

**Giải pháp trong App Router:** Sử dụng các hàm từ `next/headers` và `next/cookies`. Khi bạn gọi các hàm này, Next.js sẽ tự động chuyển route đó sang chế độ render động (SSR) tại thời điểm request, vì kết quả render sẽ phụ thuộc vào thông tin của từng request cụ thể.

**Code Ví dụ:**

Tạo một Server Component để hiển thị thông tin request.

```typescript jsx
// src/components/RequestInfo.tsx
import { headers, cookies } from "next/headers";

// Đây là một Server Component
export default function RequestInfo() {
  const headersList = headers();
  const cookiesList = cookies();

  // Lấy một header cụ thể
  const userAgent = headersList.get("user-agent");

  // Kiểm tra xem có cookie nào đó không
  const themeCookie = cookiesList.get("theme");

  return (
    <div className="p-4 border mt-4">
      <h3 className="font-bold">Request Information (from Server Component)</h3>
      <p>
        <strong>User-Agent:</strong> {userAgent}
      </p>
      <p>
        <strong>Theme Cookie:</strong>{" "}
        {themeCookie ? themeCookie.value : "Not set"}
      </p>

      {/* Ví dụ đặt một cookie từ Server Action (sẽ tìm hiểu ở phần bonus) */}
    </div>
  );
}

// Bạn có thể đặt <RequestInfo /> vào bất kỳ page.tsx hoặc layout.tsx nào.
// Trang đó sẽ tự động được render động (SSR).
```

**`server-only` package:**

Để đảm bảo một module chỉ được import và sử dụng trên server (ví dụ: một file chứa các hàm tương tác với database có secret keys), bạn có thể dùng package `server-only`.

```bash
npm install server-only
```

```typescript
// src/lib/server-utils.ts
import "server-only"; // Đặt ở dòng đầu tiên

import prisma from "./prisma";

export async function getSensitiveData() {
  // Hàm này chứa logic nhạy cảm
  // ...
  return { data: "some secret data" };
}
```

Nếu một Client Component (`'use client'`) cố gắng import module này, Next.js sẽ báo lỗi tại thời điểm build, giúp bạn tránh vô tình làm lộ code server ra client.

---

**4. `notFound()` và `redirect()` - Điều Hướng Chương Trình từ Server**

Trong Server Components, bạn không thể dùng hook `useRouter`. Thay vào đó, Next.js cung cấp các hàm từ `next/navigation` để xử lý các trường hợp này.

- `notFound()`: Khi được gọi, nó sẽ dừng việc render và hiển thị file `not-found.tsx` gần nhất trong cây thư mục.
- `redirect()`: Dừng việc render và chuyển hướng người dùng đến một URL khác.

**Code Ví dụ (chúng ta đã thấy ở các phần trước):**

```typescript jsx
// src/app/blog/[slug]/page.tsx
import { notFound, redirect } from "next/navigation";
import { getPost } from "@/lib/data";
import { getServerSession } from "next-auth";

export default async function BlogPostPage({ params }) {
  const post = await getPost(params.slug);

  // Nếu không tìm thấy bài viết -> Hiển thị trang 404
  if (!post) {
    notFound();
  }

  // Nếu bài viết này là bản nháp và người dùng chưa đăng nhập
  if (!post.published) {
    const session = await getServerSession();
    if (!session) {
      // Chuyển hướng đến trang đăng nhập
      redirect("/api/auth/signin");
    }
  }

  return <article>{/* ... render post ... */}</article>;
}
```

---

**Tổng Kết Toàn Bộ Series & Hướng Đi Tiếp Theo**

Chúc mừng bạn! Bạn đã hoàn thành một hành trình "khủng khiếp" và toàn diện, đi từ những viên gạch cơ bản của React đến việc làm chủ các khái niệm và công cụ nâng cao nhất của Next.js.

**Những gì bạn đã học được:**

1.  **Nền tảng React & TS**
2.  **React Hooks & Vòng đời**
3.  **Tối ưu hóa & Forms cơ bản**
4.  **Kiến trúc Next.js App Router (Routing, Rendering)**
5.  **Xây dựng API Routes**
6.  **Tích hợp Database với Prisma**
7.  **Xác thực với Auth.js**
8.  **Forms nâng cao & File Uploads**
9.  **State Management (Zustand & SWR)**
10. **Tối ưu Assets & Navigation (`<Link>`, `<Image>`, ...)**
11. **Tiện ích nâng cao (Middleware, Dynamic Imports, ...)**

Với bộ kiến thức này, bạn không chỉ có thể xây dựng một ứng dụng full-stack hoàn chỉnh, mà còn có thể xây dựng nó một cách hiệu quả, an toàn, có khả năng mở rộng và được tối ưu hóa cho hiệu năng.

**Hướng đi tiếp theo:**

1.  **Xây dựng dự án thực tế:** Đây là bước quan trọng nhất. Hãy áp dụng tất cả những gì đã học vào một dự án của riêng bạn.
2.  **Server Actions:** Tìm hiểu về Server Actions, một tính năng mới trong React và Next.js cho phép Client Components gọi trực tiếp các hàm chạy trên server mà không cần tạo API route. Đây là một mô hình rất mạnh mẽ để xử lý các mutation.
3.  **Testing:** Đi sâu vào việc viết Unit Test (Jest/Vitest), Component Test (React Testing Library) và E2E Test (Cypress/Playwright) cho ứng dụng Next.js của bạn.
4.  **CI/CD:** Thiết lập một quy trình tích hợp và triển khai liên tục (Continuous Integration/Continuous Deployment) với GitHub Actions để tự động hóa việc testing và deployment.
5.  **Khám phá hệ sinh thái:** Tìm hiểu thêm về các thư viện phổ biến khác như tRPC (để tạo API type-safe), các thư viện component UI (Shadcn/ui, MUI, Chakra UI)...

Hành trình học không bao giờ kết thúc, nhưng bạn đã có một nền tảng cực kỳ vững chắc. Chúc bạn thành công trên con đường trở thành một nhà phát triển Next.js chuyên nghiệp
