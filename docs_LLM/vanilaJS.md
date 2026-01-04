**Phần 1: Làm Chủ DOM Selection & Traversal (Chọn và Duyệt DOM)**

DOM (Document Object Model) là một giao diện lập trình cho các tài liệu HTML và XML. Nó biểu diễn cấu trúc của tài liệu dưới dạng một cây các đối tượng, nơi mỗi đối tượng (node) tương ứng với một phần của tài liệu (ví dụ: một phần tử, một thuộc tính, hoặc một đoạn văn bản). Thao tác DOM là cốt lõi của việc tạo ra các trang web động.

**1. Các Phương Thức Lựa Chọn Phần Tử Cơ Bản và Nâng Cao:**

- **`document.getElementById(id)`**:

  - Trả về một đối tượng `Element` duy nhất có `id` khớp. Nếu không tìm thấy, trả về `null`.
  - `id` là duy nhất trong tài liệu, nên đây là cách nhanh nhất để lấy một phần tử cụ thể.
  - Ví dụ:
    ```html
    <div id="main-container">Nội dung chính</div>
    <script>
      const mainContainer = document.getElementById("main-container");
      if (mainContainer) {
        mainContainer.style.color = "blue";
        console.log(mainContainer.textContent); // "Nội dung chính"
      }
    </script>
    ```

- **`document.getElementsByTagName(tagName)`**:

  - Trả về một `HTMLCollection` (một tập hợp _live_ - tự động cập nhật khi DOM thay đổi) của tất cả các phần tử có tên thẻ (tag name) được chỉ định.
  - Thứ tự trong collection theo thứ tự xuất hiện trong tài liệu.
  - Ví dụ:
    ```html
    <p>Đoạn 1</p>
    <div>Một div</div>
    <p>Đoạn 2</p>
    <script>
      const allParagraphs = document.getElementsByTagName("p");
      console.log(allParagraphs.length); // 2
      for (let i = 0; i < allParagraphs.length; i++) {
        allParagraphs[i].style.fontWeight = "bold";
      }
      // Thêm một <p> mới, allParagraphs sẽ tự động cập nhật
      const newP = document.createElement("p");
      newP.textContent = "Đoạn 3";
      document.body.appendChild(newP);
      console.log(allParagraphs.length); // 3 (HTMLCollection là live)
    </script>
    ```

- **`document.getElementsByClassName(classNames)`**:

  - Trả về một `HTMLCollection` (_live_) của tất cả các phần tử có chứa tất cả các class được chỉ định trong chuỗi `classNames` (cách nhau bởi dấu cách).
  - Ví dụ:
    ```html
    <div class="item active">Sản phẩm 1</div>
    <div class="item">Sản phẩm 2</div>
    <div class="item active featured">Sản phẩm 3</div>
    <script>
      const activeItems = document.getElementsByClassName("item active");
      console.log(activeItems.length); // 2 (Sản phẩm 1 và Sản phẩm 3)
      for (let item of activeItems) {
        item.style.border = "1px solid green";
      }
    </script>
    ```

- **`document.querySelector(selectors)`**:

  - Trả về phần tử đầu tiên trong tài liệu khớp với bộ chọn CSS (CSS selector) được chỉ định. Nếu không tìm thấy, trả về `null`.
  - Bộ chọn CSS rất mạnh mẽ, cho phép lựa chọn phức tạp.
  - Ví dụ:

    ```html
    <div id="app">
      <ul class="list">
        <li class="item">Mục 1</li>
        <li class="item special">Mục 2</li>
      </ul>
    </div>
    <button class="btn-primary">Gửi</button>
    <script>
      const firstItem = document.querySelector(".list .item"); // Lấy li.item đầu tiên trong ul.list
      if (firstItem) firstItem.style.color = "red";

      const specialItem = document.querySelector("#app .item.special"); // Lấy li.item.special trong #app
      if (specialItem) specialItem.style.backgroundColor = "yellow";

      const button = document.querySelector("button.btn-primary");
      console.log(button.textContent); // "Gửi"
    </script>
    ```

- **`document.querySelectorAll(selectors)`**:

  - Trả về một `NodeList` (_static_ - không tự động cập nhật khi DOM thay đổi) của tất cả các phần tử trong tài liệu khớp với bộ chọn CSS được chỉ định.
  - `NodeList` có thể được lặp qua bằng `forEach` hoặc vòng lặp `for...of`.
  - Ví dụ:

    ```html
    <article>
      <p class="highlight">Đoạn văn nổi bật 1</p>
      <p>Đoạn văn thường</p>
      <p class="highlight">Đoạn văn nổi bật 2</p>
    </article>
    <script>
      const highlightedParagraphs = document.querySelectorAll(
        "article p.highlight"
      );
      console.log(highlightedParagraphs.length); // 2
      highlightedParagraphs.forEach((p) => {
        p.style.fontStyle = "italic";
      });

      // Thêm một <p class="highlight"> mới, highlightedParagraphs sẽ KHÔNG tự động cập nhật
      const newHighlight = document.createElement("p");
      newHighlight.className = "highlight";
      newHighlight.textContent = "Đoạn văn nổi bật 3";
      document.querySelector("article").appendChild(newHighlight);
      console.log(highlightedParagraphs.length); // Vẫn là 2 (NodeList là static)
    </script>
    ```

**Sự khác biệt giữa `HTMLCollection` (live) và `NodeList` (static):**

- **Live (`HTMLCollection`):** Phản ánh trạng thái hiện tại của DOM. Nếu bạn thêm hoặc xóa các phần tử khớp với bộ chọn ban đầu, collection sẽ tự động cập nhật. Điều này có thể hữu ích nhưng cũng có thể gây ra lỗi không mong muốn nếu bạn đang lặp qua collection và sửa đổi DOM cùng lúc.
- **Static (`NodeList` từ `querySelectorAll`):** Là một bản snapshot của DOM tại thời điểm nó được tạo ra. Các thay đổi sau đó đối với DOM không ảnh hưởng đến NodeList này. Điều này thường an toàn hơn khi lặp và sửa đổi.

**2. Duyệt DOM (DOM Traversal): Di chuyển giữa các Node**

Sau khi chọn được một phần tử, bạn thường cần di chuyển đến các phần tử liên quan (cha, con, anh em).

- **`element.parentNode`**:

  - Trả về node cha của `element`. Nếu không có cha (ví dụ: `document.documentElement`), trả về `null`.
  - Lưu ý: `parentNode` có thể là một `Element` node hoặc `Document` node hoặc `DocumentFragment` node.
  - Ví dụ:
    ```html
    <div id="parent">
      <span id="child">Con</span>
    </div>
    <script>
      const childSpan = document.getElementById("child");
      const parentDiv = childSpan.parentNode;
      if (parentDiv) {
        parentDiv.style.border = "1px solid black"; // parentDiv là #parent
      }
    </script>
    ```

- **`element.parentElement`**:

  - Tương tự `parentNode`, nhưng chỉ trả về cha nếu cha đó là một `Element`. Nếu cha không phải là `Element` (ví dụ, `document`), nó trả về `null`. Thường thì đây là cái bạn muốn dùng khi chỉ quan tâm đến các phần tử HTML.
  - Ví dụ:
    ```html
    <div id="grandparent">
      <div id="parent-el">
        <span id="child-el">Con</span>
      </div>
    </div>
    <script>
      const childEl = document.getElementById("child-el");
      const parentElement = childEl.parentElement; // #parent-el
      if (parentElement) {
        parentElement.style.backgroundColor = "lightgray";
      }
      console.log(document.documentElement.parentNode); // #document
      console.log(document.documentElement.parentElement); // null
    </script>
    ```

- **`element.childNodes`**:

  - Trả về một `NodeList` (_live_) chứa tất cả các node con trực tiếp của `element`, bao gồm cả text nodes (khoảng trắng, xuống dòng cũng là text nodes) và comment nodes.
  - Ví dụ:
    ```html
    <div id="container">
      Văn bản đầu tiên
      <span>Span bên trong</span>
      <!-- Một comment -->
      Văn bản cuối cùng
    </div>
    <script>
      const container = document.getElementById("container");
      const childNodes = container.childNodes;
      console.log(childNodes.length); // Có thể là 7 hoặc nhiều hơn tùy vào khoảng trắng
      childNodes.forEach((node) => {
        console.log(node.nodeType, node.nodeName, node.textContent.trim());
        // Node Types: 1 (ELEMENT_NODE), 3 (TEXT_NODE), 8 (COMMENT_NODE)
      });
    </script>
    ```

- **`element.children`**:

  - Trả về một `HTMLCollection` (_live_) chỉ chứa các node con là `Element` của `element`. Bỏ qua text nodes và comment nodes. Đây thường là cái bạn muốn dùng khi chỉ làm việc với các phần tử HTML con.
  - Ví dụ:
    ```html
    <ul id="myList">
      <li>Mục 1</li>
      <li>Mục 2</li>
      <!-- comment -->
      <li>Mục 3</li>
    </ul>
    <script>
      const myList = document.getElementById("myList");
      const childrenElements = myList.children;
      console.log(childrenElements.length); // 3
      for (let child of childrenElements) {
        child.style.color = "purple";
      }
    </script>
    ```

- **`element.firstChild` / `element.lastChild`**:

  - Trả về node con đầu tiên / cuối cùng của `element` (có thể là text node, comment node, hoặc element node).
  - Trả về `null` nếu không có node con.
  - Ví dụ:
    ```html
    <div id="box">
      <!-- khoảng trắng ở đây là một text node -->
      <span>Đầu tiên</span>Cuối cùng
    </div>
    <script>
      const box = document.getElementById("box");
      console.log(box.firstChild.nodeName); // có thể là #text (do khoảng trắng)
      console.log(box.lastChild.nodeName); // có thể là #text
    </script>
    ```

- **`element.firstElementChild` / `element.lastElementChild`**:

  - Trả về phần tử con đầu tiên / cuối cùng của `element`. Bỏ qua text/comment nodes.
  - Trả về `null` nếu không có phần tử con.
  - Ví dụ:
    ```html
    <div id="box-el">
      <span>Đầu tiên</span>
      <em>Thứ hai</em>
    </div>
    <script>
      const boxEl = document.getElementById("box-el");
      if (boxEl.firstElementChild) {
        boxEl.firstElementChild.style.fontWeight = "bold"; // <span>
      }
      if (boxEl.lastElementChild) {
        boxEl.lastElementChild.style.fontStyle = "italic"; // <em>
      }
    </script>
    ```

- **`element.nextSibling` / `element.previousSibling`**:

  - Trả về node anh em kế tiếp / trước đó của `element` (cùng cấp, cùng cha). Có thể là text node, comment node, hoặc element node.
  - Trả về `null` nếu không có.
  - Ví dụ:
    ```html
    <ul>
      <li>Một</li>
      <!-- comment -->
      <li>Hai</li>
      <li>Ba</li>
    </ul>
    <script>
      const liTwo = document.querySelector("ul li:nth-child(2)"); // Chọn "Hai"
      if (liTwo) {
        console.log(liTwo.previousSibling.nodeName); // #comment
        console.log(liTwo.nextSibling.nodeName); // #text (khoảng trắng giữa Hai và Ba)
      }
    </script>
    ```

- **`element.nextElementSibling` / `element.previousElementSibling`**:
  - Trả về phần tử anh em kế tiếp / trước đó của `element`. Bỏ qua text/comment nodes.
  - Trả về `null` nếu không có.
  - Ví dụ:
    ```html
    <ul>
      <li id="item1">Một</li>
      <!-- comment -->
      <li id="item2">Hai</li>
      <li id="item3">Ba</li>
    </ul>
    <script>
      const itemTwo = document.getElementById("item2");
      if (itemTwo.previousElementSibling) {
        itemTwo.previousElementSibling.textContent += " (trước đó)"; // item1
      }
      if (itemTwo.nextElementSibling) {
        itemTwo.nextElementSibling.textContent += " (kế tiếp)"; // item3
      }
    </script>
    ```

**3. Các Phương Thức Duyệt DOM Nâng Cao (ES6+):**

- **`element.closest(selector)`**:

  - Duyệt ngược lên cây DOM từ `element` (bao gồm cả chính nó), trả về tổ tiên đầu tiên khớp với `selector` được chỉ định. Nếu không tìm thấy, trả về `null`.
  - Rất hữu ích trong việc xử lý sự kiện (event delegation).
  - Ví dụ:

    ```html
    <article id="post">
      <div class="content">
        <p>Văn bản <button class="btn-save">Lưu</button></p>
      </div>
    </article>
    <script>
      const saveButton = document.querySelector(".btn-save");
      const paragraph = saveButton.closest("p");
      if (paragraph) paragraph.style.border = "1px dashed blue";

      const contentDiv = saveButton.closest(".content");
      if (contentDiv) contentDiv.style.backgroundColor = "#f0f0f0";

      const article = saveButton.closest("article#post");
      if (article) article.style.padding = "10px";

      const form = saveButton.closest("form"); // null, vì không có form cha
      console.log(form);
    </script>
    ```

- **`element.matches(selector)`**:
  - Trả về `true` nếu `element` khớp với `selector` CSS được chỉ định, ngược lại trả về `false`.
  - Hữu ích để kiểm tra một phần tử có thuộc tính/class nhất định không.
  - Ví dụ:
    ```html
    <div id="myDiv" class="container active" data-id="123"></div>
    <script>
      const myDiv = document.getElementById("myDiv");
      console.log(myDiv.matches(".active")); // true
      console.log(myDiv.matches(".container.active")); // true
      console.log(myDiv.matches('div[data-id="123"]')); // true
      console.log(myDiv.matches("#myDiv")); // true
      console.log(myDiv.matches(".inactive")); // false
      console.log(myDiv.matches("span")); // false
    </script>
    ```

**4. Best Practices và Hiệu Suất khi Chọn Phần Tử:**

- **Ưu tiên `getElementById` và `querySelector` (cho một phần tử đơn lẻ):**
  - `getElementById` là nhanh nhất nếu bạn có ID.
  - `querySelector` rất linh hoạt và thường đủ nhanh cho hầu hết các trường hợp.
- **Cẩn thận với `getElementsByTagName` và `getElementsByClassName` trong vòng lặp:**

  - Vì chúng trả về `HTMLCollection` (live), việc sửa đổi DOM bên trong vòng lặp có thể dẫn đến hành vi không mong muốn hoặc vòng lặp vô hạn. Nếu cần, hãy chuyển đổi `HTMLCollection` thành mảng tĩnh trước khi lặp: `Array.from(collection)`.
  - ```javascript
    const divs = document.getElementsByTagName("div");
    // const divsArray = Array.from(divs); // Cách an toàn hơn nếu sửa đổi DOM
    for (let i = 0; i < divs.length; i++) {
      // Nếu bạn xóa div ở đây, divs.length sẽ thay đổi
      // console.log(divs[i]);
    }
    ```

  ```

  ```

- **Giới hạn phạm vi tìm kiếm:**
  - Thay vì `document.querySelector('.my-class')`, nếu bạn biết phần tử đó nằm trong một container cụ thể, hãy dùng `containerElement.querySelector('.my-class')`. Điều này giúp trình duyệt tìm kiếm nhanh hơn.
  - Ví dụ:
    ```html
    <header>
      <a class="nav-link">Trang chủ</a>
    </header>
    <main>
      <a class="nav-link">Bài viết</a>
    </main>
    <script>
      const header = document.querySelector("header");
      const headerLink = header.querySelector(".nav-link"); // Nhanh hơn document.querySelector('header .nav-link')
      if (headerLink) headerLink.style.color = "navy";
    </script>
    ```
- **Cache các phần tử thường dùng:**

  - Nếu bạn cần truy cập cùng một phần tử nhiều lần, hãy lưu nó vào một biến thay vì query DOM mỗi lần.
  - Ví dụ:

    ```javascript
    // Xấu
    function updateUI() {
      document.getElementById("user-name").textContent = "...";
      document.getElementById("user-name").style.display = "block";
    }

    // Tốt
    const userNameElement = document.getElementById("user-name");
    function updateUIGood() {
      userNameElement.textContent = "...";
      userNameElement.style.display = "block";
    }
    ```

- **Sử dụng `querySelectorAll` một cách khôn ngoan:**
  - Nó trả về `NodeList` tĩnh, an toàn hơn để lặp.
  - Tránh các bộ chọn quá chung chung và phức tạp không cần thiết (`*`, bộ chọn con cháu sâu) nếu có thể, vì chúng có thể chậm hơn.

Phần này đã cung cấp nền tảng vững chắc về cách chọn và duyệt qua các phần tử trong DOM. Nắm vững những kỹ thuật này là bước đầu tiên để bạn có thể "điều khiển" HTML theo ý muốn. Tiếp theo, chúng ta sẽ tìm hiểu cách tạo, sửa đổi và xóa các phần tử DOM.

**Phần 2: Tạo, Sửa đổi và Xóa Bỏ Phần Tử DOM**

Sau khi biết cách chọn và duyệt các phần tử, bước tiếp theo là học cách thay đổi cấu trúc và nội dung của trang web bằng cách tạo mới, sửa đổi thuộc tính/nội dung, và xóa bỏ các phần tử DOM.

**1. Tạo Phần Tử Mới (Creating Elements):**

- **`document.createElement(tagName)`**:

  - Tạo một phần tử HTML mới với tên thẻ (tag name) được chỉ định. Phần tử này ban đầu chỉ tồn tại trong bộ nhớ, chưa được hiển thị trên trang.
  - Ví dụ:

    ```javascript
    const newDiv = document.createElement("div");
    const newParagraph = document.createElement("p");
    const newImage = document.createElement("img");
    const newButton = document.createElement("button");

    // Thiết lập thuộc tính và nội dung cho các phần tử mới
    newDiv.id = "myNewDiv";
    newDiv.className = "container special";

    newParagraph.textContent = "Đây là một đoạn văn bản mới.";
    newParagraph.style.fontSize = "16px";

    newImage.src = "path/to/image.jpg";
    newImage.alt = "Mô tả ảnh";
    newImage.width = 100;

    newButton.textContent = "Click Me";
    newButton.onclick = function () {
      alert("Button clicked!");
    };

    // Lúc này, các phần tử này vẫn chưa nằm trong DOM
    console.log(newDiv);
    ```

- **`document.createTextNode(text)`**:

  - Tạo một text node mới. Text nodes được dùng để chứa nội dung văn bản cho các phần tử.
  - Ví dụ:

    ```javascript
    const textForDiv = document.createTextNode("Nội dung văn bản cho div.");
    const dynamicText = "Giá trị động: " + Math.random();
    const dynamicTextNode = document.createTextNode(dynamicText);

    // Để thêm text node vào một element, bạn dùng appendChild (sẽ nói ở dưới)
    // newDiv.appendChild(textForDiv);
    ```

- **`document.createDocumentFragment()`**:

  - Tạo một "mảnh tài liệu" (DocumentFragment) nhẹ, không có cha. Nó giống như một "kho chứa" tạm thời cho các node.
  - **Lợi ích chính**: Khi bạn `appendChild` một `DocumentFragment` vào DOM, chỉ có các node con của fragment được thêm vào, bản thân fragment thì không. Điều này rất hiệu quả khi bạn cần thêm nhiều phần tử vào DOM cùng một lúc, vì nó chỉ gây ra một lần reflow/repaint (tái cấu trúc/vẽ lại trang) thay vì nhiều lần.
  - Ví dụ:

    ```javascript
    const fragment = document.createDocumentFragment();
    const listItems = ["Táo", "Chuối", "Cam"];

    listItems.forEach((itemText) => {
      const li = document.createElement("li");
      li.textContent = itemText;
      fragment.appendChild(li); // Thêm li vào fragment, chưa vào DOM chính
    });

    // Giả sử có một <ul> với id="fruit-list" trong HTML
    // document.getElementById('fruit-list').appendChild(fragment);
    // Chỉ có 3 <li> được thêm vào #fruit-list, không phải fragment
    console.log(fragment.childNodes.length); // Vẫn là 3 sau khi append fragment vào DOM
    console.log(fragment.querySelector("li")); // có thể query bên trong fragment
    ```

**2. Thêm Phần Tử vào DOM (Adding Elements):**

Sau khi tạo, bạn cần thêm chúng vào cây DOM để hiển thị.

- **`parentNode.appendChild(childNode)`**:

  - Thêm `childNode` làm con cuối cùng của `parentNode`.
  - Nếu `childNode` đã tồn tại ở vị trí khác trong DOM, nó sẽ được di chuyển (không phải copy).
  - Trả về node đã được thêm (childNode).
  - Ví dụ:

    ```html
    <div id="main"></div>
    <script>
      const mainDiv = document.getElementById("main");
      const p1 = document.createElement("p");
      p1.textContent = "Đoạn 1";
      mainDiv.appendChild(p1); // <div id="main"><p>Đoạn 1</p></div>

      const p2 = document.createElement("p");
      p2.textContent = "Đoạn 2";
      mainDiv.appendChild(p2); // <div id="main"><p>Đoạn 1</p><p>Đoạn 2</p></div>
    </script>
    ```

- **`parentNode.insertBefore(newNode, referenceNode)`**:

  - Chèn `newNode` vào `parentNode` ngay trước `referenceNode`.
  - Nếu `referenceNode` là `null`, `newNode` sẽ được thêm vào cuối cùng (giống `appendChild`).
  - `referenceNode` phải là con trực tiếp của `parentNode`.
  - Trả về node đã được chèn (newNode).
  - Ví dụ:

    ```html
    <ul id="my-list">
      <li>Mục 2</li>
      <li>Mục 3</li>
    </ul>
    <script>
      const ul = document.getElementById("my-list");
      const newItem = document.createElement("li");
      newItem.textContent = "Mục 1";

      const secondItem = ul.children[0]; // "Mục 2"
      ul.insertBefore(newItem, secondItem);

      const lastItem = document.createElement("li");
      lastItem.textContent = "Mục cuối";
      ul.insertBefore(lastItem, null); // Tương đương ul.appendChild(lastItem)
    </script>
    ```

- **Các Phương Thức Hiện Đại (ES6+): Tiện lợi hơn**
  Những phương thức này cho phép chèn nhiều node hoặc chuỗi DOM (HTML string) cùng lúc.

  - **`element.append(...nodesOrDOMStrings)`**: Thêm các node hoặc chuỗi DOM vào cuối `element`. Chuỗi DOM sẽ được parse thành HTML.
  - **`element.prepend(...nodesOrDOMStrings)`**: Thêm các node hoặc chuỗi DOM vào đầu `element`.
  - Ví dụ:

    ```html
    <div id="box"><span>Nội dung gốc</span></div>
    <script>
      const box = document.getElementById("box");
      const pNew = document.createElement("p");
      pNew.textContent = "Đoạn mới";

      box.append(pNew, document.createElement("hr"), "Văn bản thêm cuối");
      // box giờ là: <span>Nội dung gốc</span><p>Đoạn mới</p><hr />Văn bản thêm cuối

      const strongStart = document.createElement("strong");
      strongStart.textContent = "Bắt đầu: ";
      box.prepend(strongStart, "<em>Văn bản đầu tiên</em> ");
      // box giờ là: <strong>Bắt đầu: </strong><em>Văn bản đầu tiên</em> <span>Nội dung gốc</span>...
    </script>
    ```

  - **`element.before(...nodesOrDOMStrings)`**: Chèn các node hoặc chuỗi DOM ngay trước `element` (anh em).
  - **`element.after(...nodesOrDOMStrings)`**: Chèn các node hoặc chuỗi DOM ngay sau `element` (anh em).
  - Ví dụ:

    ```html
    <p id="target-p">Đây là đoạn văn mục tiêu.</p>
    <script>
      const targetP = document.getElementById("target-p");
      const divBefore = document.createElement("div");
      divBefore.textContent = "Div trước P";

      targetP.before(divBefore, "--- Đường kẻ trước ---");
      // HTML: <div>Div trước P</div>--- Đường kẻ trước ---<p id="target-p">...</p>

      targetP.after(document.createElement("hr"), "Nội dung sau P");
      // HTML: ...<p id="target-p">...</p><hr />Nội dung sau P
    </script>
    ```

  - **`element.replaceWith(...nodesOrDOMStrings)`**: Thay thế `element` bằng các node hoặc chuỗi DOM được cung cấp.
    ```html
    <div id="old-content">Nội dung cũ cần thay thế</div>
    <script>
      const oldContent = document.getElementById("old-content");
      const newContent = document.createElement("h2");
      newContent.textContent = "Nội dung mới";
      oldContent.replaceWith(newContent, "<p>Và một đoạn văn nữa.</p>");
      // HTML giờ chỉ còn: <h2>Nội dung mới</h2><p>Và một đoạn văn nữa.</p>
      // oldContent không còn trong DOM
    </script>
    ```

- **`element.insertAdjacentHTML(position, text)`**:

  - Parse chuỗi `text` thành HTML và chèn các node kết quả vào cây DOM tại vị trí `position` tương đối với `element`.
  - `position` có thể là một trong các giá trị:
    - `'beforebegin'`: Trước bản thân `element`.
    - `'afterbegin'`: Ngay bên trong `element`, trước đứa con đầu tiên của nó.
    - `'beforeend'`: Ngay bên trong `element`, sau đứa con cuối cùng của nó.
    - `'afterend'`: Sau bản thân `element`.
  - **Cảnh báo**: Giống `innerHTML`, nếu `text` đến từ nguồn không đáng tin cậy, có thể gây ra lỗ hổng XSS.
  - Ví dụ:
    ```html
    <div id="target-div">Nội dung hiện tại</div>
    <script>
      const targetDiv = document.getElementById("target-div");
      targetDiv.insertAdjacentHTML("beforebegin", "<h2>Tiêu đề trước Div</h2>");
      targetDiv.insertAdjacentHTML("afterbegin", "<strong>Bắt đầu!</strong> ");
      targetDiv.insertAdjacentHTML("beforeend", " <em>Kết thúc!</em>");
      targetDiv.insertAdjacentHTML("afterend", "<hr><p>Nội dung sau Div</p>");
      /*
      Kết quả HTML:
      <h2>Tiêu đề trước Div</h2>
      <div id="target-div">
        <strong>Bắt đầu!</strong>
        Nội dung hiện tại
        <em>Kết thúc!</em>
      </div>
      <hr>
      <p>Nội dung sau Div</p>
      */
    </script>
    ```

- **`element.insertAdjacentElement(position, newElement)`**: Tương tự `insertAdjacentHTML` nhưng chèn một `newElement` (đối tượng Element) thay vì chuỗi HTML.
- **`element.insertAdjacentText(position, text)`**: Tương tự `insertAdjacentHTML` nhưng chèn một `text` node (văn bản thuần túy) thay vì parse HTML. An toàn hơn `insertAdjacentHTML` nếu bạn chỉ muốn chèn văn bản.

**3. Sửa đổi Phần Tử (Modifying Elements):**

- **Nội dung (Content):**

  - **`element.textContent`**:

    - Lấy hoặc thiết lập nội dung văn bản của một node và tất cả các con cháu của nó.
    - Khi thiết lập, nó sẽ loại bỏ tất cả các node con hiện tại của `element` và thay thế bằng một text node duy nhất với giá trị được cung cấp.
    - An toàn hơn `innerHTML` vì nó tự động escape các ký tự HTML (ví dụ, `<` trở thành `&lt;`).
    - Ví dụ:
      ```html
      <div id="info">
        <p>Văn bản <strong>cũ</strong></p>
      </div>
      <script>
        const infoDiv = document.getElementById("info");
        console.log(infoDiv.textContent); // "Văn bản cũ" (không có tag HTML)
        infoDiv.textContent =
          "Nội dung văn bản mới. Thẻ <b> sẽ hiển thị như text.";
        // infoDiv giờ là: <div id="info">Nội dung văn bản mới. Thẻ &lt;b&gt; sẽ hiển thị như text.</div>
      </script>
      ```

  - **`element.innerHTML`**:

    - Lấy hoặc thiết lập nội dung HTML bên trong một `element`.
    - Khi thiết lập, trình duyệt sẽ parse chuỗi HTML và tạo cấu trúc DOM tương ứng.
    - **Mạnh mẽ nhưng nguy hiểm**: Nếu chuỗi HTML đến từ người dùng hoặc nguồn không đáng tin cậy mà không được sanitize (làm sạch), có thể dẫn đến tấn công XSS (Cross-Site Scripting).
    - Thường chậm hơn `textContent` hoặc thao tác DOM trực tiếp khi chỉ thay đổi nhỏ.
    - Ví dụ:

      ```html
      <div id="content-area">Nội dung ban đầu</div>
      <script>
        const contentArea = document.getElementById("content-area");
        console.log(contentArea.innerHTML); // "Nội dung ban đầu"
        contentArea.innerHTML =
          "<h1>Tiêu đề mới</h1><p>Một đoạn văn với <strong>từ in đậm</strong>.</p>";
        // contentArea giờ chứa cấu trúc H1 và P mới

        // Nguy hiểm nếu userInput không được sanitize:
        // const userInput = '<img src="invalid" onerror="alert(\'XSS Attack!\')">';
        // contentArea.innerHTML = userInput; // Sẽ thực thi alert!
      </script>
      ```

  - **`element.innerText`**:
    - Tương tự `textContent` nhưng có một số khác biệt quan trọng:
      - `innerText` nhận biết được CSS (ví dụ: không trả về text của phần tử bị ẩn bởi `display: none`).
      - `innerText` chuẩn hóa khoảng trắng và ngắt dòng theo cách hiển thị trên trình duyệt.
      - `innerText` có thể kích hoạt reflow vì nó cần tính toán layout.
    - Ít được dùng hơn `textContent` do hành vi phụ thuộc vào CSS và hiệu suất kém hơn.
    - Ví dụ:
      ```html
      <div id="test-inner">
        Hello <span>World</span>
        <style>
          #test-inner span {
            display: none;
          }
        </style>
      </div>
      <script>
        const testInner = document.getElementById("test-inner");
        console.log(testInner.textContent); // "Hello   World  " (bao gồm cả text của span ẩn và khoảng trắng)
        console.log(testInner.innerText); // "Hello" (không có text của span ẩn, khoảng trắng chuẩn hóa)
      </script>
      ```

- **Thuộc tính (Attributes):**

  - **`element.getAttribute(attributeName)`**: Lấy giá trị của một thuộc tính.
  - **`element.setAttribute(attributeName, value)`**: Thiết lập giá trị cho một thuộc tính. Nếu thuộc tính đã tồn tại, giá trị sẽ được cập nhật; nếu không, thuộc tính mới sẽ được tạo.
  - **`element.hasAttribute(attributeName)`**: Kiểm tra xem phần tử có thuộc tính đó không (trả về `true`/`false`).
  - **`element.removeAttribute(attributeName)`**: Xóa một thuộc tính.
  - Ví dụ:

    ```html
    <img id="my-image" src="old.jpg" alt="Ảnh cũ" data-info="thông tin" />
    <a id="my-link" href="#">Link</a>
    <script>
      const img = document.getElementById("my-image");
      console.log(img.getAttribute("src")); // "old.jpg"
      img.setAttribute("src", "new.png");
      img.setAttribute("alt", "Ảnh mới đẹp hơn");
      img.setAttribute("width", "200"); // Thêm thuộc tính width

      console.log(img.hasAttribute("data-info")); // true
      img.removeAttribute("data-info");
      console.log(img.hasAttribute("data-info")); // false

      const link = document.getElementById("my-link");
      link.setAttribute("target", "_blank");
    </script>
    ```

  - **Thuộc tính DOM (DOM Properties):**

    - Nhiều thuộc tính HTML phổ biến có các thuộc tính DOM tương ứng trên đối tượng element, có thể truy cập trực tiếp.
    - Ví dụ: `element.id`, `element.className`, `img.src`, `a.href`, `input.value`, `input.checked`, `select.selectedIndex`, `option.selected`.
    - Thay đổi thuộc tính DOM thường đồng bộ với thuộc tính HTML (nhưng không phải lúc nào cũng vậy, ví dụ `input.value` sau khi người dùng nhập).
    - Thường nhanh hơn `getAttribute/setAttribute` cho các thuộc tính chuẩn.
    - Ví dụ:

      ```html
      <input type="text" id="my-input" value="Giá trị ban đầu" />
      <div id="my-div" class="box"></div>
      <script>
        const input = document.getElementById("my-input");
        console.log(input.value); // "Giá trị ban đầu"
        input.value = "Giá trị mới"; // Thay đổi giá trị hiển thị và thuộc tính DOM
        console.log(input.getAttribute("value")); // "Giá trị ban đầu" (thuộc tính HTML gốc không đổi)

        const div = document.getElementById("my-div");
        console.log(div.id); // "my-div"
        div.className = "box highlighted"; // Ghi đè toàn bộ class
      </script>
      ```

  - **`element.dataset` (cho thuộc tính `data-*`)**:

    - Cách tiện lợi để làm việc với các thuộc tính dữ liệu tùy chỉnh (`data-your-name`).
    - Tên thuộc tính trong `dataset` được chuyển thành camelCase (ví dụ: `data-user-id` -> `element.dataset.userId`).
    - Ví dụ:

      ```html
      <div
        id="user-card"
        data-user-id="123"
        data-user-name="Alice"
        data-is-active="true"
      >
        Thẻ người dùng
      </div>
      <script>
        const card = document.getElementById("user-card");
        console.log(card.dataset.userId); // "123"
        console.log(card.dataset.userName); // "Alice"
        console.log(card.dataset.isActive); // "true" (luôn là chuỗi)

        card.dataset.userRole = "admin"; // Thêm data-user-role="admin"
        delete card.dataset.isActive; // Xóa data-is-active
        console.log(card.outerHTML);
        // <div id="user-card" data-user-id="123" data-user-name="Alice" data-user-role="admin">Thẻ người dùng</div>
      </script>
      ```

- **Kiểu dáng CSS (CSS Styles):**

  - **`element.style.property`**:

    - Truy cập và thiết lập các thuộc tính CSS inline của phần tử.
    - Tên thuộc tính CSS có dấu gạch ngang (kebab-case) được chuyển thành camelCase (ví dụ: `background-color` -> `element.style.backgroundColor`).
    - Thiết lập theo cách này sẽ ghi đè các style từ stylesheet hoặc thẻ `<style>`.
    - Chỉ lấy được các style được đặt inline, không lấy được style từ CSS file.
    - Ví dụ:

      ```html
      <p id="styled-text">Văn bản cần style.</p>
      <script>
        const p = document.getElementById("styled-text");
        p.style.color = "blue";
        p.style.fontSize = "20px";
        p.style.backgroundColor = "#f0f0f0";
        p.style.padding = "10px";
        p.style.border = "1px solid green";
        // p.style.margin-left = '50px'; // (margin-left -> marginLeft)

        console.log(p.style.color); // "blue"
        // Nếu color được đặt trong CSS file, p.style.color sẽ là chuỗi rỗng.
      </script>
      ```

  - **`element.style.cssText`**:

    - Thiết lập hoặc lấy toàn bộ chuỗi khai báo style inline.
    - Khi thiết lập, nó sẽ ghi đè tất cả các style inline hiện có.
    - Ví dụ:
      ```javascript
      const el = document.createElement("div");
      el.style.cssText = "color: red; font-weight: bold; display: block;";
      // el.style.cssText += 'padding: 5px;'; // Thêm vào, nhưng cẩn thận vì nó parse lại
      console.log(el.style.cssText);
      ```

  - **`element.className`**:

    - Lấy hoặc thiết lập toàn bộ giá trị của thuộc tính `class` dưới dạng một chuỗi.
    - Khi thiết lập, nó sẽ ghi đè tất cả các class hiện có.
    - Ví dụ:
      ```html
      <div id="my-element" class="one two"></div>
      <script>
        const myElement = document.getElementById("my-element");
        console.log(myElement.className); // "one two"
        myElement.className = "active highlighted"; // Ghi đè, giờ chỉ có 'active highlighted'
        myElement.className += " new-class"; // Thêm class, cẩn thận khoảng trắng: 'active highlighted new-class'
      </script>
      ```

  - **`element.classList` (API danh sách lớp)**:

    - Cách hiện đại và được khuyến nghị để thao tác với các class. Cung cấp các phương thức tiện lợi:
      - `add(className1, className2, ...)`: Thêm một hoặc nhiều class.
      - `remove(className1, className2, ...)`: Xóa một hoặc nhiều class.
      - `toggle(className, force)`: Nếu class tồn tại, xóa nó; nếu không, thêm nó. Nếu `force` là `true`, luôn thêm; nếu `false`, luôn xóa.
      - `contains(className)`: Kiểm tra xem class có tồn tại không (trả về `true`/`false`).
      - `replace(oldClass, newClass)`: Thay thế một class cũ bằng một class mới.
    - Ví dụ:

      ```html
      <button id="action-button" class="btn primary">Hành động</button>
      <script>
        const btn = document.getElementById("action-button");
        btn.classList.add("large", "active"); // class="btn primary large active"
        console.log(btn.className);

        btn.classList.remove("primary"); // class="btn large active"
        console.log(btn.className);

        btn.classList.toggle("loading"); // Thêm 'loading'
        console.log(btn.classList.contains("loading")); // true
        btn.classList.toggle("loading"); // Xóa 'loading'

        btn.classList.toggle("disabled", true); // Luôn thêm 'disabled'
        console.log(btn.className);

        btn.classList.replace("active", "inactive"); // class="btn large inactive disabled"
        console.log(btn.className);
      </script>
      ```

  - **`window.getComputedStyle(element, [pseudoElt])`**:

    - Trả về một đối tượng `CSSStyleDeclaration` chứa tất cả các giá trị CSS _thực sự được áp dụng_ cho một phần tử (bao gồm từ stylesheet, style inline, style mặc định của trình duyệt).
    - Các giá trị trả về là "computed values" (giá trị đã tính toán), thường là giá trị tuyệt đối (ví dụ: `16px` thay vì `1em`).
    - Đối tượng này là _read-only_.
    - `pseudoElt` (tùy chọn): Chuỗi chỉ định pseudo-element để lấy style (ví dụ: `':before'`, `':after'`).
    - Ví dụ:

      ```html
      <style>
        #computed-div {
          width: 50%;
          padding: 10px;
          color: green;
          margin-left: 1em; /* Giả sử font-size gốc là 16px */
        }
      </style>
      <div id="computed-div" style="font-weight: bold;">Nội dung</div>
      <script>
        const div = document.getElementById("computed-div");
        const styles = window.getComputedStyle(div);

        console.log(styles.width); // Ví dụ: "500px" (nếu body width là 1000px)
        console.log(styles.paddingTop); // "10px"
        console.log(styles.color); // "rgb(0, 128, 0)" (green)
        console.log(styles.fontWeight); // "700" (bold)
        console.log(styles.marginLeft); // "16px" (nếu 1em = 16px)
        console.log(styles.display); // "block" (mặc định cho div)

        // Lấy style của pseudo-element
        // div.innerHTML = 'Nội dung ::before'; // Cần có content cho ::before
        // const beforeStyles = window.getComputedStyle(div, '::before');
        // console.log(beforeStyles.content);
      </script>
      ```

**4. Xóa Bỏ Phần Tử (Removing Elements):**

- **`parentNode.removeChild(childNode)`**:

  - Phương thức truyền thống. Xóa `childNode` khỏi `parentNode`.
  - `childNode` phải là con trực tiếp của `parentNode`.
  - Trả về node đã bị xóa (có thể dùng lại hoặc để garbage collector dọn dẹp).
  - Ví dụ:
    ```html
    <ul id="task-list">
      <li id="task1">Task 1</li>
      <li id="task2">Task 2</li>
      <li id="task3">Task 3</li>
    </ul>
    <script>
      const taskList = document.getElementById("task-list");
      const task2 = document.getElementById("task2");
      if (taskList && task2) {
        const removedNode = taskList.removeChild(task2);
        console.log(removedNode.textContent); // "Task 2"
        // task2 vẫn tồn tại trong bộ nhớ, nhưng không còn trong DOM
      }
    </script>
    ```

- **`element.remove()`**:
  - Phương thức hiện đại, đơn giản hơn. Xóa `element` khỏi cây DOM.
  - Không cần tham chiếu đến cha.
  - Không trả về gì.
  - Ví dụ:
    ```html
    <div id="to-remove">Phần tử này sẽ bị xóa.</div>
    <script>
      const elementToRemove = document.getElementById("to-remove");
      if (elementToRemove) {
        elementToRemove.remove();
      }
    </script>
    ```

**5. Thay Thế Phần Tử (Replacing Elements):**

- **`parentNode.replaceChild(newNode, oldNode)`**:

  - Thay thế `oldNode` (phải là con của `parentNode`) bằng `newNode`.
  - Trả về `oldNode` đã bị thay thế.
  - Ví dụ:

    ```html
    <div id="container-replace">
      <p id="old-paragraph">Đoạn văn cũ.</p>
    </div>
    <script>
      const container = document.getElementById("container-replace");
      const oldP = document.getElementById("old-paragraph");
      const newHeading = document.createElement("h3");
      newHeading.textContent = "Tiêu đề mới thay thế";

      if (container && oldP) {
        container.replaceChild(newHeading, oldP);
        // oldP không còn trong DOM
      }
    </script>
    ```

- **`oldElement.replaceWith(newElementOrString)`**: (Đã giới thiệu ở phần thêm)
  - Phương thức hiện đại. Thay thế `oldElement` bằng `newElement` hoặc một chuỗi HTML.
  - Ví dụ:
    ```html
    <span id="placeholder">Nội dung tạm</span>
    <script>
      const placeholder = document.getElementById("placeholder");
      const finalContent = document.createElement("strong");
      finalContent.textContent = "Nội dung cuối cùng!";
      if (placeholder) {
        placeholder.replaceWith(finalContent);
      }
    </script>
    ```

**6. Sao Chép Phần Tử (Cloning Elements):**

- **`element.cloneNode(deep)`**:

  - Tạo một bản sao của `element`.
  - `deep` (boolean):
    - `true`: Sao chép sâu. Sao chép `element` và tất cả các node con cháu của nó (bao gồm text nodes).
    - `false` (mặc định): Sao chép nông. Chỉ sao chép bản thân `element` (không có con).
  - **Quan trọng**:
    - Bản sao không có cha (`parentNode` là `null`). Bạn cần `appendChild` hoặc các phương thức tương tự để thêm nó vào DOM.
    - Thuộc tính `id` cũng được sao chép, nên nếu bạn thêm bản sao vào cùng tài liệu, hãy đảm bảo thay đổi `id` để tránh trùng lặp.
    - **Event listeners (sự kiện được gán bằng JavaScript như `element.onclick = ...` hoặc `element.addEventListener(...)`) KHÔNG được sao chép.** Bạn phải gán lại sự kiện cho node đã clone. Tuy nhiên, các trình xử lý sự kiện inline HTML (ví dụ: `<button onclick="myFunc()">`) thì được sao chép như một phần của chuỗi HTML thuộc tính.
  - Ví dụ:

    ```html
    <div id="original-item" class="item" data-value="100">
      <p>Văn bản gốc <span>Nested</span></p>
      <button onclick="alert('Clicked original')">Click Original</button>
    </div>
    <div id="clone-container"></div>
    <script>
      const original = document.getElementById("original-item");
      const container = document.getElementById("clone-container");

      // Sao chép nông (chỉ div, không có p hay button bên trong)
      const shallowClone = original.cloneNode(false);
      shallowClone.id = "shallow-clone";
      // container.appendChild(shallowClone);
      // console.log(shallowClone.innerHTML); // ""

      // Sao chép sâu (toàn bộ cấu trúc)
      const deepClone = original.cloneNode(true);
      deepClone.id = "deep-clone-item"; // Thay đổi ID để tránh trùng
      deepClone.querySelector("p").textContent += " (Bản sao)";

      // Gán lại event listener cho button trong bản sao (nếu cần)
      const clonedButton = deepClone.querySelector("button");
      if (clonedButton) {
        clonedButton.textContent = "Click Cloned";
        // Xóa onclick attribute cũ nếu có và không muốn hành vi đó
        clonedButton.removeAttribute("onclick");
        clonedButton.addEventListener("click", () => {
          alert("Clicked CLONED item!");
        });
      }

      container.appendChild(deepClone);
    </script>
    ```

**7. Best Practices và Hiệu Suất khi Thao Tác DOM:**

- **Giảm thiểu Reflow và Repaint:**
  - **Reflow (Layout/Relayout)**: Xảy ra khi trình duyệt phải tính toán lại vị trí và kích thước của các phần tử. Các thao tác như thay đổi kích thước cửa sổ, thay đổi font, thêm/xóa/thay đổi kích thước phần tử, đọc một số thuộc tính layout (`offsetTop`, `offsetWidth`) có thể gây ra reflow. Reflow rất tốn kém.
  - **Repaint (Redraw)**: Xảy ra khi trình duyệt phải vẽ lại các phần tử trên màn hình sau khi reflow, hoặc khi chỉ thay đổi các thuộc tính không ảnh hưởng đến layout (ví dụ: `background-color`, `color`, `visibility`). Repaint ít tốn kém hơn reflow.
- **Sử dụng `DocumentFragment` cho các thao tác hàng loạt:** Như đã đề cập, khi thêm nhiều phần tử, hãy `appendChild` chúng vào một `DocumentFragment` trước, sau đó mới `appendChild` fragment đó vào DOM. Điều này chỉ gây ra một reflow.
- **Thay đổi "ngoài DOM":** Nếu bạn cần thực hiện nhiều thay đổi (thuộc tính, style, con) trên một phần tử đã có trong DOM, một kỹ thuật là:
  1.  Lưu trữ tham chiếu đến phần tử.
  2.  Xóa nó khỏi DOM (ví dụ: `element.remove()`).
  3.  Thực hiện tất cả các thay đổi trên phần tử đó (lúc này nó đang "ngoài DOM").
  4.  Thêm lại nó vào DOM.
  - Cách này giảm thiểu số lần reflow/repaint. Tuy nhiên, việc sử dụng `DocumentFragment` thường ưu tiên hơn nếu chỉ là thêm mới. Cân nhắc nếu việc xóa và thêm lại gây ra nhấp nháy không mong muốn.
- **Cẩn thận với `innerHTML`:**
  - Nó tiện lợi để tạo cấu trúc HTML phức tạp từ chuỗi, nhưng có thể chậm hơn thao tác DOM trực tiếp cho các thay đổi nhỏ và tiềm ẩn nguy cơ XSS.
  - Khi gán cho `innerHTML`, trình duyệt sẽ parse lại toàn bộ nội dung HTML bên trong phần tử đó, xóa bỏ các con cũ và tạo các con mới. Các event listener trên các con cũ sẽ bị mất.
- **CSS `transform` và `opacity` cho Animation:**
  - Khi tạo animation, ưu tiên sử dụng các thuộc tính CSS `transform` (translate, scale, rotate) và `opacity`. Trình duyệt thường có thể tối ưu hóa các thay đổi này bằng cách sử dụng GPU acceleration và không gây ra reflow (chỉ repaint hoặc composite).
- **Debounce và Throttle Event Handlers:** Đối với các sự kiện kích hoạt thường xuyên (như `scroll`, `resize`, `mousemove`), nếu trình xử lý sự kiện thực hiện các thao tác DOM nặng, hãy sử dụng kỹ thuật debounce hoặc throttle để giới hạn tần suất thực thi.

Nắm vững các kỹ thuật này sẽ cho phép bạn xây dựng các giao diện người dùng động và tương tác cao hoàn toàn bằng Vanilla JavaScript.

**Phần 3: Làm Chủ Sự Kiện (Event Handling)**

Sự kiện là nền tảng của tính tương tác trên web. Chúng là những hành động hoặc sự xuất hiện được trình duyệt phát hiện, chẳng hạn như người dùng nhấp chuột, nhấn phím, trang tải xong, hoặc một phần tử được thay đổi. JavaScript cho phép bạn "lắng nghe" những sự kiện này và thực thi mã phản hồi lại.

**1. Cách Gán Trình Xử Lý Sự Kiện (Event Handlers):**

Có nhiều cách để gán một hàm (trình xử lý sự kiện) để chạy khi một sự kiện cụ thể xảy ra trên một phần tử.

- **Thuộc tính HTML on-event (Inline Event Handlers):**

  - Bạn có thể gán mã JavaScript trực tiếp vào các thuộc tính HTML như `onclick`, `onmouseover`, v.v.
  - **Không khuyến khích:** Trộn lẫn HTML và JavaScript làm mã khó đọc, khó bảo trì và vi phạm nguyên tắc tách biệt mối quan tâm (separation of concerns).
  - Ví dụ:
    ```html
    <button onclick="alert('Button clicked inline!'); console.log(this);">
      Click Me (Inline)
    </button>
    <div
      onmouseover="this.style.backgroundColor='yellow'"
      onmouseout="this.style.backgroundColor='transparent'"
    >
      Hover over me (Inline)
    </div>
    ```
    - Trong trình xử lý inline, `this` thường tham chiếu đến phần tử DOM mà sự kiện được gán.

- **Thuộc tính DOM on-event (DOM Element Properties):**

  - Các phần tử DOM có các thuộc tính tương ứng với các sự kiện (ví dụ: `element.onclick`, `element.onmouseover`). Bạn có thể gán một hàm cho các thuộc tính này.
  - **Hạn chế:** Chỉ có thể gán một trình xử lý sự kiện cho mỗi loại sự kiện trên một phần tử. Gán một hàm mới sẽ ghi đè hàm cũ.
  - `this` bên trong hàm xử lý sẽ tham chiếu đến phần tử DOM mà sự kiện được gán.
  - Ví dụ:

    ```html
    <button id="myButtonDomProp">Click Me (DOM Prop)</button>
    <div id="hoverDivDomProp">Hover me (DOM Prop)</div>
    <script>
      const myButton = document.getElementById("myButtonDomProp");
      const hoverDiv = document.getElementById("hoverDivDomProp");

      myButton.onclick = function () {
        alert("Button clicked via DOM property!");
        console.log("this in DOM prop onclick:", this); // 'this' là button
        this.textContent = "Clicked!";
      };

      // Ghi đè trình xử lý cũ nếu có
      // myButton.onclick = function() { alert('New handler!'); };

      hoverDiv.onmouseover = function () {
        this.style.border = "2px solid red";
      };
      hoverDiv.onmouseout = function () {
        this.style.border = "none";
      };

      // Để xóa trình xử lý:
      // myButton.onclick = null;
    </script>
    ```

- **`element.addEventListener(type, listener, optionsOrUseCapture)` (Khuyến khích nhất):**

  - Phương thức linh hoạt và mạnh mẽ nhất.
  - `type`: Chuỗi tên sự kiện (ví dụ: `'click'`, `'mouseover'`, `'keydown'`) không có tiền tố "on".
  - `listener`: Hàm sẽ được gọi khi sự kiện xảy ra. Hàm này nhận một đối tượng `Event` làm đối số đầu tiên.
  - `optionsOrUseCapture` (tùy chọn):
    - Một boolean:
      - `true`: Listener sẽ được kích hoạt trong giai đoạn _capturing_ (chụp).
      - `false` (mặc định): Listener sẽ được kích hoạt trong giai đoạn _bubbling_ (nổi bọt).
    - Một đối tượng `options`:
      - `capture`: (boolean) Giống như giá trị boolean ở trên.
      - `once`: (boolean) Nếu `true`, listener sẽ tự động bị xóa sau khi được kích hoạt lần đầu tiên.
      - `passive`: (boolean) Nếu `true`, cho trình duyệt biết rằng listener sẽ không gọi `event.preventDefault()`. Điều này có thể cải thiện hiệu suất cuộn trên các thiết bị cảm ứng đối với các sự kiện như `touchmove` hoặc `wheel`.
  - Cho phép gán nhiều trình xử lý cho cùng một sự kiện trên cùng một phần tử.
  - `this` bên trong `listener` (nếu là hàm thông thường) tham chiếu đến phần tử mà listener được gắn vào (`event.currentTarget`).
  - Ví dụ:

    ```html
    <button id="advButton">Advanced Click</button>
    <div id="eventOptionsDiv" style="padding: 10px; border: 1px solid black;">
      Test Options
    </div>
    <script>
      const advButton = document.getElementById("advButton");
      const eventOptionsDiv = document.getElementById("eventOptionsDiv");

      function handleClick(event) {
        console.log("Event type:", event.type);
        console.log("Target element:", event.target); // Phần tử kích hoạt sự kiện
        console.log("Current target:", event.currentTarget); // Phần tử listener được gắn vào
        console.log("this in addEventListener:", this); // 'this' là advButton
        this.innerHTML = `Clicked at ${event.timeStamp}`;
      }

      advButton.addEventListener("click", handleClick);

      // Thêm một listener khác cho cùng sự kiện 'click'
      advButton.addEventListener("click", function (e) {
        console.log("Second click listener on the same button.");
      });

      // Sử dụng options object
      function handleMouseOverOnce(event) {
        event.target.style.backgroundColor = "lightgreen";
        console.log("Mouse over (once). Listener will be removed.");
      }
      eventOptionsDiv.addEventListener("mouseover", handleMouseOverOnce, {
        once: true,
      });

      function handleScroll(event) {
        // Giả sử chúng ta không gọi preventDefault()
        console.log("Scrolling...");
      }
      // window.addEventListener('scroll', handleScroll, { passive: true });

      // Listener trong giai đoạn capturing
      document.body.addEventListener(
        "click",
        function (event) {
          console.log("Body capturing click on:", event.target.tagName);
        },
        { capture: true }
      );

      eventOptionsDiv.addEventListener(
        "click",
        function (event) {
          console.log("Div bubbling click");
        },
        { capture: false }
      ); // hoặc bỏ qua, vì false là mặc định
    </script>
    ```

**2. Đối Tượng `Event`:**

Khi một sự kiện xảy ra và một trình xử lý được gọi, trình duyệt sẽ tự động truyền một đối tượng `Event` (hoặc một đối tượng con của nó như `MouseEvent`, `KeyboardEvent`) làm đối số đầu tiên cho hàm xử lý. Đối tượng này chứa thông tin quan trọng về sự kiện.

- **Các Thuộc Tính Phổ Biến của `Event`:**

  - `event.type`: (string) Loại sự kiện (ví dụ: `'click'`, `'load'`).
  - `event.target`: (Node) Phần tử DOM _khởi phát_ sự kiện (phần tử sâu nhất trong DOM mà sự kiện xảy ra trên đó).
  - `event.currentTarget`: (Node) Phần tử DOM mà trình xử lý sự kiện _hiện tại_ được gắn vào. `this` bên trong hàm xử lý (nếu là hàm thường) thường bằng `event.currentTarget`.
  - `event.timeStamp`: (number) Thời gian (tính bằng millisecond từ khi tài liệu được tải) mà sự kiện được tạo ra.
  - `event.bubbles`: (boolean) Cho biết sự kiện có nổi bọt lên cây DOM hay không.
  - `event.cancelable`: (boolean) Cho biết hành động mặc định của sự kiện có thể bị hủy bỏ bằng `event.preventDefault()` hay không.

- **Các Phương Thức Phổ Biến của `Event`:**

  - `event.preventDefault()`:
    - Ngăn chặn hành động mặc định của trình duyệt đối với sự kiện đó.
    - Ví dụ: Ngăn form submit khi nhấp nút submit, ngăn link điều hướng khi nhấp vào thẻ `<a>`.
    - Chỉ có tác dụng nếu `event.cancelable` là `true`.
  - `event.stopPropagation()`:
    - Ngăn sự kiện lan truyền thêm trong các giai đoạn capturing hoặc bubbling. Sự kiện sẽ không được xử lý bởi các listener trên các phần tử cha (trong bubbling) hoặc các phần tử con sâu hơn (trong capturing đã xảy ra trước đó).
  - `event.stopImmediatePropagation()`:
    - Tương tự `stopPropagation()`, nhưng nó còn ngăn cả các listener _khác trên cùng một phần tử_ cho _cùng một loại sự kiện_ được thực thi sau listener hiện tại.

- **Thuộc Tính Đặc Thù của Các Loại Sự Kiện:**

  - **`MouseEvent`** (cho `click`, `mousemove`, etc.):
    - `event.clientX`, `event.clientY`: Tọa độ X, Y của chuột tương đối với viewport của trình duyệt.
    - `event.pageX`, `event.pageY`: Tọa độ X, Y của chuột tương đối với toàn bộ tài liệu (bao gồm cả phần đã cuộn).
    - `event.screenX`, `event.screenY`: Tọa độ X, Y của chuột tương đối với màn hình.
    - `event.button`: (number) Nút chuột nào được nhấn (0: trái, 1: giữa, 2: phải).
    - `event.altKey`, `event.ctrlKey`, `event.shiftKey`, `event.metaKey`: (boolean) Cho biết các phím модификатор có được nhấn khi sự kiện xảy ra không.
  - **`KeyboardEvent`** (cho `keydown`, `keyup`):
    - `event.key`: (string) Giá trị của phím được nhấn (ví dụ: `'a'`, `'Enter'`, `'ArrowUp'`).
    - `event.code`: (string) Mã vật lý của phím trên bàn phím (ví dụ: `'KeyA'`, `'Enter'`, `'ArrowUp'`). `key` thay đổi theo layout bàn phím, `code` thì không.
    - `event.altKey`, `event.ctrlKey`, `event.shiftKey`, `event.metaKey`.
  - **`TouchEvent`** (cho `touchstart`, `touchmove`, `touchend`):
    - `event.touches`: Danh sách các điểm chạm hiện tại trên màn hình.
    - `event.targetTouches`: Danh sách các điểm chạm hiện tại trên phần tử target.
    - `event.changedTouches`: Danh sách các điểm chạm có trạng thái đã thay đổi (ví dụ: chạm mới hoặc nhấc lên).

  Ví dụ sử dụng đối tượng `Event`:

  ```html
  <a id="myLink" href="https://example.com">Visit Example.com</a>
  <form id="myForm">
    <input type="text" name="username" placeholder="Username" />
    <button type="submit">Submit</button>
  </form>
  <div
    id="mouseBox"
    style="width:200px; height:100px; background:lightblue; margin-top:10px;"
  >
    Move mouse here
  </div>
  <input type="text" id="keyInput" placeholder="Type here" />

  <script>
    document
      .getElementById("myLink")
      .addEventListener("click", function (event) {
        event.preventDefault(); // Ngăn chuyển hướng
        console.log("Link click prevented. Target:", event.target.href);
      });

    document
      .getElementById("myForm")
      .addEventListener("submit", function (event) {
        event.preventDefault(); // Ngăn form submit theo cách truyền thống
        const username = event.target.elements.username.value;
        console.log("Form submitted via JS. Username:", username);
        // Gửi dữ liệu bằng AJAX ở đây
      });

    const mouseBox = document.getElementById("mouseBox");
    mouseBox.addEventListener("mousemove", function (event) {
      this.textContent = `X: ${event.clientX}, Y: ${event.clientY}`;
    });
    mouseBox.addEventListener("contextmenu", function (event) {
      event.preventDefault(); // Ngăn menu chuột phải mặc định
      alert("Custom context menu!");
    });

    document
      .getElementById("keyInput")
      .addEventListener("keydown", function (event) {
        console.log(`Key: ${event.key}, Code: ${event.code}`);
        if (event.key === "Enter") {
          alert("Enter pressed in input!");
        }
        if (event.ctrlKey && event.key === "s") {
          event.preventDefault(); // Ngăn lưu trang mặc định của trình duyệt
          alert("Ctrl+S pressed!");
        }
      });

    // stopPropagation example
    const parentDiv = document.createElement("div");
    parentDiv.style.padding = "20px";
    parentDiv.style.background = "lightgray";
    const childButton = document.createElement("button");
    childButton.textContent = "Click Me (Child)";
    parentDiv.appendChild(childButton);
    document.body.appendChild(parentDiv);

    parentDiv.addEventListener("click", function () {
      console.log("Parent div clicked (Bubbling)");
    });

    childButton.addEventListener("click", function (event) {
      console.log("Child button clicked");
      event.stopPropagation(); // Ngăn sự kiện click nổi bọt lên parentDiv
    });
  </script>
  ```

**3. Giai Đoạn Sự Kiện: Capturing và Bubbling**

Khi một sự kiện xảy ra trên một phần tử lồng nhau, nó sẽ trải qua ba giai đoạn:

1.  **Giai đoạn Capturing (Chụp):** Sự kiện di chuyển từ `window` xuống qua các phần tử cha cho đến phần tử `target` (phần tử khởi phát sự kiện). Các listener được đăng ký cho giai đoạn capturing sẽ được kích hoạt theo thứ tự này.
2.  **Giai đoạn Target (Mục tiêu):** Sự kiện đến phần tử `target`. Các listener được gắn trực tiếp vào `target` sẽ được kích hoạt.
3.  **Giai đoạn Bubbling (Nổi bọt):** Sự kiện di chuyển ngược từ `target` lên qua các phần tử cha cho đến `window`. Các listener được đăng ký cho giai đoạn bubbling (mặc định) sẽ được kích hoạt theo thứ tự này.

Hầu hết các sự kiện đều "nổi bọt" (ví dụ: `click`, `keydown`), nhưng một số thì không (ví dụ: `focus`, `blur`, `load`).
Mặc định, `addEventListener` đăng ký listener cho giai đoạn bubbling. Để đăng ký cho giai đoạn capturing, bạn đặt tùy chọn `capture: true` (hoặc đối số thứ ba là `true`).

```html
<div id="grandparent" style="background: #f0f0f0; padding: 30px;">
  Grandparent
  <div id="parent" style="background: #d0d0d0; padding: 20px;">
    Parent
    <button id="child" style="padding: 10px;">Child Button</button>
  </div>
</div>

<script>
  const gp = document.getElementById("grandparent");
  const p = document.getElementById("parent");
  const c = document.getElementById("child");

  // Capturing phase listeners
  gp.addEventListener(
    "click",
    function (e) {
      console.log("Grandparent Capturing - Target:", e.target.id);
    },
    true
  );
  p.addEventListener(
    "click",
    function (e) {
      console.log("Parent Capturing - Target:", e.target.id);
    },
    true
  );
  c.addEventListener(
    "click",
    function (e) {
      console.log(
        "Child (Target Phase during Capturing flow) - Target:",
        e.target.id
      );
    },
    true
  );

  // Bubbling phase listeners (mặc định, hoặc { capture: false })
  gp.addEventListener("click", function (e) {
    console.log("Grandparent Bubbling - Target:", e.target.id);
  });
  p.addEventListener("click", function (e) {
    console.log("Parent Bubbling - Target:", e.target.id);
  });
  c.addEventListener("click", function (e) {
    console.log(
      "Child (Target Phase during Bubbling flow) - Target:",
      e.target.id
    );
  });

  /*
  Khi nhấp vào Child Button, output sẽ là:
  1. Grandparent Capturing - Target: child
  2. Parent Capturing - Target: child
  3. Child (Target Phase during Capturing flow) - Target: child  (listener trên child, giai đoạn capturing)
  4. Child (Target Phase during Bubbling flow) - Target: child (listener trên child, giai đoạn bubbling)
  5. Parent Bubbling - Target: child
  6. Grandparent Bubbling - Target: child
  */
</script>
```

Giai đoạn capturing ít được sử dụng hơn bubbling, nhưng có thể hữu ích trong một số trường hợp cụ thể, ví dụ như chặn một sự kiện trước khi nó đến được phần tử target.

**4. Ủy Quyền Sự Kiện (Event Delegation)**

Event delegation là một pattern mạnh mẽ và hiệu quả: thay vì gắn nhiều listener cho nhiều phần tử con, bạn gắn một listener duy nhất cho một phần tử cha chung. Khi sự kiện xảy ra trên một phần tử con, nó sẽ nổi bọt lên cha. Trong listener của cha, bạn sử dụng `event.target` để xác định phần tử con nào đã thực sự khởi phát sự kiện và xử lý tương ứng.

- **Lợi ích:**

  - **Hiệu suất:** Giảm số lượng listener, đặc biệt hữu ích với danh sách dài hoặc bảng lớn.
  - **Nội dung động:** Tự động xử lý các phần tử con được thêm vào DOM _sau khi_ listener đã được gắn. Bạn không cần phải gắn listener mới cho từng phần tử mới.
  - **Đơn giản hóa mã:** Quản lý một listener dễ hơn nhiều listener.

- Cách thực hiện:
  1.  Chọn một phần tử cha bao chứa các phần tử bạn muốn theo dõi sự kiện.
  2.  Gắn một listener cho sự kiện mong muốn trên phần tử cha đó.
  3.  Trong hàm listener, kiểm tra `event.target` (hoặc `event.target.closest(selector)`) để xem nó có phải là phần tử con mà bạn quan tâm hay không.

```html
<ul id="itemList">
  <li>Item 1 <button class="delete-btn" data-id="1">Xóa</button></li>
  <li>Item 2 <button class="delete-btn" data-id="2">Xóa</button></li>
  <li>Item 3 <button class="delete-btn" data-id="3">Xóa</button></li>
</ul>
<button id="addItemBtn">Thêm Item</button>

<script>
  const itemList = document.getElementById("itemList");
  const addItemBtn = document.getElementById("addItemBtn");
  let itemCount = 3;

  // Event delegation cho các nút xóa
  itemList.addEventListener("click", function (event) {
    // Kiểm tra xem phần tử được click có class 'delete-btn' không
    const targetButton = event.target.closest(".delete-btn"); // an toàn hơn event.target nếu button có con

    if (targetButton) {
      // if (event.target.classList.contains('delete-btn')) { // Cách khác đơn giản hơn nếu button không có con
      const itemId = targetButton.dataset.id;
      console.log(`Xóa item có ID: ${itemId}`);
      // Tìm li cha để xóa
      const listItem = targetButton.closest("li");
      if (listItem) {
        listItem.remove();
      }
    }
  });

  // Thêm item mới (sẽ tự động được xử lý bởi listener ủy quyền)
  addItemBtn.addEventListener("click", function () {
    itemCount++;
    const newItem = document.createElement("li");
    newItem.innerHTML = `Item ${itemCount} <button class="delete-btn" data-id="${itemCount}">Xóa</button>`;
    itemList.appendChild(newItem);
  });
</script>
```

Trong ví dụ trên, chỉ có một listener `'click'` trên `ul#itemList`. Nó xử lý các click trên tất cả các nút `.delete-btn` hiện có và cả những nút sẽ được thêm vào sau này. `event.target.closest('.delete-btn')` được dùng để đảm bảo chúng ta lấy đúng nút bấm, ngay cả khi người dùng click vào một phần tử con bên trong nút (ví dụ: một `<span>` bên trong `<button>`).

**5. Một Số Loại Sự Kiện Phổ Biến:**

- **Mouse Events:**

  - `click`: Nhấp chuột trái.
  - `dblclick`: Nhấp đúp chuột trái.
  - `mousedown`: Nhấn nút chuột xuống.
  - `mouseup`: Nhả nút chuột.
  - `mousemove`: Di chuyển chuột trên một phần tử.
  - `mouseover`: Con trỏ chuột di chuyển vào một phần tử hoặc một trong các con của nó.
  - `mouseout`: Con trỏ chuột di chuyển ra khỏi một phần tử hoặc một trong các con của nó.
  - `mouseenter`: Con trỏ chuột di chuyển vào một phần tử (không nổi bọt, không kích hoạt khi vào/ra con).
  - `mouseleave`: Con trỏ chuột di chuyển ra khỏi một phần tử (không nổi bọt, không kích hoạt khi vào/ra con). `mouseenter`/`mouseleave` thường dễ làm việc hơn `mouseover`/`mouseout`.
  - `contextmenu`: Nhấp chuột phải (để hiển thị menu ngữ cảnh).

- **Keyboard Events:**

  - `keydown`: Nhấn một phím xuống. Kích hoạt liên tục nếu phím được giữ.
  - `keyup`: Nhả một phím.
  - `keypress` (cũ, nên tránh): Kích hoạt khi một phím tạo ra một ký tự. Không kích hoạt cho các phím như Shift, Ctrl, Alt.

- **Form Events:**

  - `submit`: Khi form được gửi (thường là nhấn nút `type="submit"` hoặc Enter trong trường input).
  - `change`: Khi giá trị của một phần tử form (`<input>`, `<select>`, `<textarea>`) thay đổi và phần tử đó mất focus. Đối với checkbox và radio, nó kích hoạt ngay khi trạng thái thay đổi.
  - `input`: Kích hoạt ngay lập tức khi giá trị của `<input>` hoặc `<textarea>` thay đổi. Hữu ích hơn `change` cho phản hồi tức thì.
  - `focus`: Một phần tử nhận focus.
  - `blur`: Một phần tử mất focus.
  - `reset`: Khi form được reset (nhấn nút `type="reset"`).

- **Document/Window Events:**

  - `DOMContentLoaded`: Khi tài liệu HTML ban đầu đã được tải và parse hoàn toàn, không cần đợi stylesheet, ảnh, và subframe tải xong. Đây là thời điểm tốt để chạy mã JavaScript thao tác DOM.
  - `load` (trên `window`): Khi toàn bộ trang đã tải xong, bao gồm tất cả tài nguyên (ảnh, CSS, script, iframe).
  - `unload` (trên `window`): Khi người dùng rời khỏi trang (ví dụ: đóng tab, điều hướng đi).
  - `beforeunload` (trên `window`): Kích hoạt ngay trước khi người dùng rời trang. Có thể dùng để hiển thị hộp thoại xác nhận (ví dụ: "Bạn có chắc muốn rời khỏi trang này không?").
  - `resize` (trên `window`): Khi cửa sổ trình duyệt được thay đổi kích thước.
  - `scroll` (trên `window` hoặc các phần tử có thể cuộn): Khi nội dung được cuộn.

- **Focus Events (ngoài `focus` và `blur` đã nêu):**

  - `focusin`: Tương tự `focus` nhưng nổi bọt.
  - `focusout`: Tương tự `blur` nhưng nổi bọt.

- **CSS Transition/Animation Events:**

  - `transitionend`: Khi một CSS transition hoàn thành.
  - `animationend`: Khi một CSS animation hoàn thành.

- **Custom Events:** Bạn có thể tạo và gửi các sự kiện tùy chỉnh của riêng mình.

  ```javascript
  const myElement = document.getElementById("someElement");

  // Tạo custom event
  const myCustomEvent = new CustomEvent("userLoggedIn", {
    detail: {
      // Dữ liệu tùy chỉnh bạn muốn truyền
      userId: 123,
      username: "coderPro",
    },
    bubbles: true, // Có nổi bọt không
    cancelable: true, // Có thể preventDefault không
  });

  // Lắng nghe custom event
  myElement.addEventListener("userLoggedIn", function (event) {
    console.log('Custom event "userLoggedIn" received!');
    console.log("User ID:", event.detail.userId);
    console.log("Username:", event.detail.username);
  });

  // Gửi (dispatch) custom event
  // myElement.dispatchEvent(myCustomEvent);
  ```

**6. Xóa Bỏ Trình Xử Lý Sự Kiện:**

Quan trọng là phải xóa các listener khi chúng không còn cần thiết để tránh rò rỉ bộ nhớ (memory leaks), đặc biệt trong các ứng dụng Single Page Application (SPA) nơi các phần tử DOM được thêm và xóa thường xuyên.

- **`element.removeEventListener(type, listener, optionsOrUseCapture)`:**

  - Các đối số `type`, `listener`, và `optionsOrUseCapture` (hoặc chỉ `useCapture` boolean) phải _chính xác giống_ như khi bạn gọi `addEventListener`.
  - **Lưu ý quan trọng:** Bạn không thể xóa một listener là hàm ẩn danh (anonymous function) trực tiếp, vì bạn không có tham chiếu đến nó. Bạn phải sử dụng một hàm được đặt tên hoặc lưu trữ hàm ẩn danh vào một biến.

  ```javascript
  const btn = document.getElementById("removableEventBtn");
  let counter = 0;

  function handleBtnClick() {
    counter++;
    console.log(`Button clicked ${counter} times.`);
    if (counter >= 3) {
      console.log("Removing click listener.");
      btn.removeEventListener("click", handleBtnClick); // Xóa listener thành công
    }
  }
  btn.addEventListener("click", handleBtnClick);

  // Không thể xóa listener ẩn danh như thế này:
  // btn.addEventListener('mouseover', function() { console.log('Mouse over'); });
  // btn.removeEventListener('mouseover', function() { console.log('Mouse over'); }); // SẼ KHÔNG HOẠT ĐỘNG
  // vì đây là hai hàm khác nhau.
  // Cách đúng cho hàm ẩn danh (lưu vào biến):
  const handleMouseOver = function () {
    console.log("Mouse over, will be removed.");
    btn.removeEventListener("mouseover", handleMouseOver); // Hoạt động
  };
  // btn.addEventListener('mouseover', handleMouseOver);
  ```

- **Xóa listener thuộc tính DOM:**

  ```javascript
  // Gán: myButton.onclick = myFunction;
  // Xóa: myButton.onclick = null;
  ```

- **Sử dụng `AbortController` để xóa nhiều listener (hiện đại):**
  Một `AbortController` có thể được sử dụng để hủy bỏ một hoặc nhiều hoạt động không đồng bộ, bao gồm cả event listener.

  ```javascript
  const controller = new AbortController();
  const signal = controller.signal;
  const abortableButton = document.getElementById("abortableButton");

  abortableButton.addEventListener(
    "click",
    () => {
      console.log("Abortable button clicked!");
    },
    { signal }
  ); // Truyền signal vào options

  abortableButton.addEventListener(
    "mouseover",
    () => {
      console.log("Abortable button mouseover!");
    },
    { signal }
  );

  // Sau một thời gian, hoặc khi một điều kiện nào đó xảy ra:
  // setTimeout(() => {
  //   console.log('Aborting listeners on abortableButton.');
  //   controller.abort(); // Tất cả listener với signal này sẽ bị xóa
  // }, 5000);
  ```

  Khi `controller.abort()` được gọi, tất cả các listener được đăng ký với `signal` tương ứng sẽ tự động bị xóa.

**7. `this` trong Trình Xử Lý Sự Kiện:**

Giá trị của `this` bên trong một trình xử lý sự kiện phụ thuộc vào cách hàm đó được định nghĩa và gọi:

- **Hàm thông thường (`function() {}`)**:
  - Khi được sử dụng với `element.addEventListener('event', function() { /* this is element */ })` hoặc `element.onevent = function() { /* this is element */ }`, `this` bên trong hàm sẽ tham chiếu đến phần tử DOM mà listener được gắn vào (`event.currentTarget`).
- **Hàm mũi tên (`() => {}`)**:

  - Hàm mũi tên không có `this` của riêng nó. `this` bên trong hàm mũi tên được kế thừa từ phạm vi (scope) bao quanh nó tại thời điểm hàm được định nghĩa.
  - Điều này có thể hữu ích, nhưng cũng có thể gây nhầm lẫn nếu bạn mong đợi `this` là phần tử DOM.

  ```html
  <button id="thisTestBtn">Test 'this'</button>
  <script>
    const thisTestBtn = document.getElementById("thisTestBtn");

    // Hàm thông thường
    thisTestBtn.addEventListener("click", function (event) {
      console.log("Regular function this:", this); // thisTestBtn
      console.log("Regular function event.currentTarget:", event.currentTarget); // thisTestBtn
      this.textContent = "Clicked (Regular)";
    });

    // Hàm mũi tên
    // Giả sử code này nằm trong global scope hoặc một object method
    // thisTestBtn.addEventListener('click', (event) => {
    //   console.log('Arrow function this:', this); // Sẽ là `window` (nếu ở global scope)
    //                                           // hoặc 'this' của object bao quanh
    //   console.log('Arrow function event.currentTarget:', event.currentTarget); // thisTestBtn
    //   // event.currentTarget.textContent = 'Clicked (Arrow)'; // Dùng event.currentTarget để truy cập phần tử
    // });

    class MyComponent {
      constructor(elementId) {
        this.element = document.getElementById(elementId);
        this.count = 0;

        // 'this' trong hàm thường là element, phải bind(this) để this.incrementCount hoạt động
        // this.element.addEventListener('mousedown', function() {
        //   console.log('Mousedown this (regular):', this); // this.element
        //   // this.incrementCount(); // Lỗi: this.incrementCount is not a function
        // }.bind(this)); // bind this của class MyComponent vào

        // Hàm mũi tên giữ 'this' của class MyComponent
        this.element.addEventListener("mouseup", (event) => {
          console.log("Mouseup this (arrow):", this); // MyComponent instance
          console.log("Mouseup event.target:", event.target); // this.element
          this.incrementCount();
          event.target.textContent = `Count: ${this.count}`;
        });
      }

      incrementCount() {
        this.count++;
        console.log("Count is now:", this.count);
      }
    }
    // const component = new MyComponent('thisTestBtn');
  </script>
  ```

**8. Best Practices cho Event Handling:**

- **Ưu tiên `addEventListener`:** Nó linh hoạt hơn, cho phép nhiều listener, và kiểm soát giai đoạn capturing/bubbling.
- **Sử dụng Event Delegation:** Cho danh sách động hoặc nhiều phần tử tương tự để cải thiện hiệu suất và đơn giản hóa mã.
- **Xóa Listener:** Luôn xóa listener khi chúng không còn cần thiết để tránh memory leaks, đặc biệt trong SPA. Sử dụng `AbortController` cho các trường hợp phức tạp hơn.
- **Hiểu rõ `this`:** Cẩn thận với giá trị của `this`, đặc biệt khi dùng hàm mũi tên hoặc khi truyền hàm callback qua nhiều lớp.
- **`passive: true` cho hiệu suất cuộn:** Với các listener `scroll`, `wheel`, `touchstart`, `touchmove`, nếu bạn không gọi `event.preventDefault()`, hãy thêm `{ passive: true }` để trình duyệt không phải đợi listener thực thi xong trước khi cuộn, giúp cuộn mượt hơn.
- **Debounce và Throttle:** Đối với các sự kiện kích hoạt liên tục (ví dụ `resize`, `scroll`, `mousemove`), nếu trình xử lý thực hiện các tác vụ nặng, hãy sử dụng kỹ thuật debounce (chỉ thực thi sau một khoảng thời gian không có sự kiện mới) hoặc throttle (giới hạn tần suất thực thi) để tránh làm chậm trình duyệt. (Sẽ nói kỹ hơn ở phần sau).
- **Accessibility (Khả năng tiếp cận):** Đảm bảo các tương tác có thể thực hiện được bằng cả chuột và bàn phím. Ví dụ, nếu bạn có một hành động kích hoạt bằng `click`, hãy đảm bảo nó cũng có thể kích hoạt bằng phím `Enter` hoặc `Space` khi phần tử đó được focus.

Nắm vững cách xử lý sự kiện là một bước cực kỳ quan trọng để tạo ra các trang web thực sự động và tương tác, mở đường cho việc xây dựng các thành phần UI phức tạp và thậm chí là framework của riêng bạn.

**Phần 4: Thao Tác Với Thuộc Tính, Class và Style Nâng Cao, Dimensions và Coordinates**

Phần này sẽ đi sâu hơn vào cách làm việc với các khía cạnh trực quan và cấu trúc của phần tử DOM, bao gồm quản lý class hiệu quả, thao tác style phức tạp, và lấy thông tin về kích thước, vị trí của phần tử trên trang.

**1. Quản Lý Thuộc Tính (Attributes) Nâng Cao:**

Chúng ta đã biết `getAttribute`, `setAttribute`, `hasAttribute`, `removeAttribute`. Giờ hãy xem xét một số điểm tinh tế hơn.

- **Sự khác biệt giữa Thuộc tính HTML (HTML Attributes) và Thuộc tính DOM (DOM Properties):**

  - **HTML Attributes:** Được định nghĩa trong mã HTML ban đầu (ví dụ: `<input type="text" value="hello">`). Chúng thường được dùng để khởi tạo trạng thái ban đầu. `getAttribute` và `setAttribute` làm việc với chúng.
  - **DOM Properties:** Là các thuộc tính của đối tượng JavaScript tương ứng với phần tử HTML. Ví dụ: `inputElement.value`, `inputElement.type`.
  - **Đồng bộ hóa:** Đối với nhiều thuộc tính chuẩn (như `id`, `src`, `href`), DOM property và HTML attribute được đồng bộ hóa: thay đổi một cái sẽ cập nhật cái kia.
  - **Không đồng bộ hóa:** Một số trường hợp quan trọng không đồng bộ:
    - `input.value`: Khi người dùng nhập liệu vào ô input, `input.value` (DOM property) sẽ thay đổi, nhưng `input.getAttribute('value')` (HTML attribute) vẫn giữ giá trị ban đầu. HTML attribute `value` phản ánh giá trị _mặc định_ hoặc _khởi tạo_.
    - `input.checked`: Tương tự cho checkbox, `input.checked` (boolean) là trạng thái hiện tại, còn `input.getAttribute('checked')` chỉ phản ánh sự tồn tại của thuộc tính `checked` trong HTML ban đầu.
  - **Ưu tiên DOM Properties:** Khi có thể, hãy sử dụng DOM properties vì chúng thường nhanh hơn và tiện lợi hơn (ví dụ, `input.checked` là boolean, trong khi `input.getAttribute('checked')` là chuỗi hoặc `null`). `setAttribute` luôn chuyển giá trị thành chuỗi.
  - Ví dụ minh họa `value`:
    ```html
    <input id="myInput" type="text" value="Initial Value" />
    <button onclick="showValues()">Show Values</button>
    <script>
      const myInput = document.getElementById("myInput");
      function showValues() {
        console.log("DOM Property (input.value):", myInput.value);
        console.log(
          "HTML Attribute (getAttribute):",
          myInput.getAttribute("value")
        );
      }
      // Sau khi người dùng thay đổi nội dung input và click button:
      // DOM Property (input.value): giá trị người dùng nhập
      // HTML Attribute (getAttribute): "Initial Value"
    </script>
    ```

- **Thuộc tính boolean:**

  - Các thuộc tính HTML như `checked`, `disabled`, `selected`, `readonly`, `multiple` là thuộc tính boolean. Sự có mặt của chúng nghĩa là `true`, vắng mặt nghĩa là `false`.
  - Khi dùng `setAttribute`: `element.setAttribute('disabled', '')` hoặc `element.setAttribute('disabled', 'disabled')` đều làm cho thuộc tính có hiệu lực.
  - Khi dùng DOM properties: `input.disabled = true;` hoặc `input.disabled = false;`. Đây là cách rõ ràng hơn.
  - `hasAttribute` kiểm tra sự tồn tại, còn DOM property trả về giá trị boolean thực sự.
  - Ví dụ:

    ```html
    <input id="chk" type="checkbox" />
    <button id="btnToggle" disabled>Toggle</button>
    <script>
      const chk = document.getElementById("chk");
      const btn = document.getElementById("btnToggle");

      console.log("Initial chk.checked (DOM prop):", chk.checked); // false
      console.log(
        'Initial chk.hasAttribute("checked"):',
        chk.hasAttribute("checked")
      ); // false

      chk.checked = true; // Đặt qua DOM property
      console.log(
        'After chk.checked=true, chk.hasAttribute("checked"):',
        chk.hasAttribute("checked")
      ); // false (không tự thêm attribute)
      // Để attribute được thêm: chk.setAttribute('checked', '');

      console.log("Initial btn.disabled (DOM prop):", btn.disabled); // true
      console.log(
        'Initial btn.hasAttribute("disabled"):',
        btn.hasAttribute("disabled")
      ); // true

      btn.disabled = false;
      console.log(
        'After btn.disabled=false, btn.hasAttribute("disabled"):',
        btn.hasAttribute("disabled")
      ); // false (attribute bị xóa)

      // Ngược lại
      // btn.removeAttribute('disabled'); // => btn.disabled sẽ là false
      // btn.setAttribute('disabled', ''); // => btn.disabled sẽ là true
    </script>
    ```

- **Thuộc tính `data-*` (Custom Data Attributes):**

  - Đã đề cập với `element.dataset`. Đây là cách chuẩn để lưu trữ dữ liệu tùy chỉnh trên các phần tử HTML, giúp JavaScript dễ dàng truy cập mà không làm rối các thuộc tính chuẩn hoặc `class`/`id`.
  - Nhớ rằng giá trị trong `dataset` luôn là chuỗi. Nếu bạn lưu số hoặc boolean, bạn cần chuyển đổi khi đọc:

    ```javascript
    // HTML: <div id="item" data-count="10" data-is-valid="true"></div>
    const item = document.getElementById("item");
    const count = parseInt(item.dataset.count, 10); // 10 (number)
    const isValid = item.dataset.isValid === "true"; // true (boolean)

    item.dataset.price = 99.99;
    const price = parseFloat(item.dataset.price);
    ```

**2. Làm Việc với `classList` Hiệu Quả:**

`element.classList` là API tốt nhất để thao tác class.

- **Thêm/Xóa nhiều class:**
  ```javascript
  const box = document.getElementById("myBox");
  box.classList.add("active", "visible", "highlighted");
  box.classList.remove("old-class", "temporary");
  ```
- **Toggle với điều kiện (tham số `force`):**
  ```javascript
  let isEnabled = false;
  // ... some logic to change isEnabled ...
  // box.classList.toggle('enabled-feature', isEnabled); // Thêm 'enabled-feature' nếu isEnabled là true, xóa nếu false.
  ```
- **Kiểm tra và thay thế:**
  ```javascript
  if (box.classList.contains("hidden")) {
    box.classList.replace("hidden", "shown");
  }
  ```
- **Lặp qua các class:**
  `element.classList` là một đối tượng `DOMTokenList`, có thể lặp qua:
  ```javascript
  const elementWithClasses = document.getElementById("styledDiv"); // <div id="styledDiv" class="foo bar baz">
  if (elementWithClasses) {
    for (const className of elementWithClasses.classList) {
      console.log(className); // foo, bar, baz
    }
    elementWithClasses.classList.forEach((className) => {
      console.log("forEach:", className);
    });
    console.log("Class at index 0:", elementWithClasses.classList[0]); // foo
    console.log("Number of classes:", elementWithClasses.classList.length); // 3
  }
  ```

**3. Thao Tác Style Nâng Cao:**

- **Thiết lập nhiều style cùng lúc với `element.style.cssText`:**

  ```javascript
  const panel = document.getElementById("myPanel");
  // Ghi đè tất cả style inline hiện có
  panel.style.cssText =
    "color: white; background-color: navy; padding: 15px; border-radius: 5px;";

  // Thêm vào style hiện có (cẩn thận, có thể không hiệu quả bằng set từng cái)
  // panel.style.cssText += 'font-size: 1.2em;';
  ```

  Lưu ý: `cssText` tiện lợi nhưng có thể kém hiệu quả hơn việc đặt từng thuộc tính `style.property` nếu chỉ thay đổi một vài thứ, và nó ghi đè toàn bộ.

- **Xóa một style inline:**
  Để xóa một style inline đã đặt qua `element.style.property = value;`, bạn gán cho nó một chuỗi rỗng.

  ```javascript
  const header = document.getElementById("mainHeader");
  header.style.color = "red"; // Đặt màu
  // ... sau đó ...
  header.style.color = ""; // Xóa style màu inline, trình duyệt sẽ dùng lại style từ CSS hoặc mặc định
  ```

  Bạn cũng có thể dùng `element.style.removeProperty('property-name-kebab-case')`:

  ```javascript
  header.style.setProperty("font-weight", "bold", "important"); // Đặt với !important
  // ...
  header.style.removeProperty("font-weight"); // Xóa font-weight
  ```

- **Làm việc với `!important`:**
  JavaScript có thể đặt và xóa các style `!important`.

  ```javascript
  const alertBox = document.getElementById("alertBox");
  // Đặt style với !important
  alertBox.style.setProperty("display", "block", "important");

  // Xem giá trị và độ ưu tiên
  console.log(alertBox.style.getPropertyValue("display")); // "block"
  console.log(alertBox.style.getPropertyPriority("display")); // "important" hoặc ""

  // Xóa: không có cách trực tiếp để chỉ xóa !important, bạn phải xóa toàn bộ thuộc tính
  // Hoặc đặt lại mà không có !important
  // alertBox.style.setProperty('display', 'none'); // Ghi đè
  // alertBox.style.removeProperty('display');
  ```

  Nói chung, nên hạn chế sử dụng `!important` cả trong CSS và JavaScript vì nó làm code khó quản lý.

- **Đọc giá trị CSS thực sự được áp dụng (`window.getComputedStyle`):**
  Như đã đề cập, `element.style` chỉ đọc được các style inline. `getComputedStyle` là công cụ để biết được style cuối cùng sau khi tất cả các rule CSS (từ file, thẻ `<style>`, inline) và các giá trị mặc định của trình duyệt đã được áp dụng.

  ```html
  <style>
    .my-element {
      width: 50%;
      padding: 1em;
      color: blue;
    }
  </style>
  <div id="computedExample" class="my-element" style="margin-left: 20px;">
    Test
  </div>
  <script>
    const el = document.getElementById("computedExample");
    const computedStyles = window.getComputedStyle(el);

    console.log("Width (từ CSS class):", computedStyles.width); // Sẽ là giá trị px, ví dụ "600px"
    console.log("Padding-top (từ CSS class, 1em):", computedStyles.paddingTop); // Sẽ là giá trị px, ví dụ "16px"
    console.log("Color (từ CSS class):", computedStyles.color); // "rgb(0, 0, 255)"
    console.log("Margin-left (từ style inline):", computedStyles.marginLeft); // "20px"
    console.log("Display (mặc định cho div):", computedStyles.display); // "block"
    console.log("Font-size (mặc định hoặc kế thừa):", computedStyles.fontSize);

    // Lấy style của pseudo-element
    // HTML: <div id="pseudoEl" class="my-element">Content</div>
    // CSS: .my-element::before { content: "Prefix: "; color: red; }
    // const pseudoEl = document.getElementById('pseudoEl');
    // const beforeStyles = window.getComputedStyle(pseudoEl, '::before');
    // if (beforeStyles) {
    //   console.log('Pseudo-element ::before content:', beforeStyles.content); // '"Prefix: "'
    //   console.log('Pseudo-element ::before color:', beforeStyles.color);   // "rgb(255, 0, 0)"
    // }
  </script>
  ```

  `getComputedStyle` trả về giá trị _computed value_, không phải _resolved value_. Ví dụ, nếu `width: auto`, `computedStyles.width` sẽ là giá trị pixel thực tế.

**4. Kích Thước và Vị Trí Phần Tử (Dimensions and Coordinates):**

Hiểu về kích thước và vị trí của các phần tử là rất quan trọng để tạo layout động, animation, tooltip, v.v.

- **Kích thước bao gồm padding và border (Offset Dimensions):**

  - `element.offsetWidth`: Chiều rộng đầy đủ của phần tử, bao gồm padding, border, và thanh cuộn dọc (nếu có). Không bao gồm margin.
  - `element.offsetHeight`: Chiều cao đầy đủ của phần tử, bao gồm padding, border, và thanh cuộn ngang (nếu có). Không bao gồm margin.
  - Đây là kích thước "bên ngoài" của phần tử, thường là cái bạn thấy trên màn hình.
  - Ví dụ:
    ```html
    <style>
      #offsetBox {
        width: 100px; /* content width */
        height: 50px; /* content height */
        padding: 10px;
        border: 5px solid black;
        margin: 20px;
        overflow: auto; /* Để có thể có scrollbar */
      }
    </style>
    <div id="offsetBox">Content</div>
    <script>
      const box = document.getElementById("offsetBox");
      // Giả sử không có thanh cuộn
      // offsetWidth = 100 (width) + 10*2 (padding) + 5*2 (border) = 130
      console.log("offsetWidth:", box.offsetWidth);
      // offsetHeight = 50 (height) + 10*2 (padding) + 5*2 (border) = 80
      console.log("offsetHeight:", box.offsetHeight);
    </script>
    ```

- **Kích thước vùng nội dung (Client Dimensions):**

  - `element.clientWidth`: Chiều rộng của vùng nội dung bên trong phần tử, bao gồm padding. Không bao gồm border, margin, hoặc thanh cuộn dọc.
  - `element.clientHeight`: Chiều cao của vùng nội dung bên trong phần tử, bao gồm padding. Không bao gồm border, margin, hoặc thanh cuộn ngang.
  - Nếu không có padding và thanh cuộn, `clientWidth/Height` bằng `width/height` trong CSS.
  - Ví dụ (tiếp theo ví dụ trên):
    ```javascript
    // clientWidth = 100 (width) + 10*2 (padding) = 120
    console.log("clientWidth:", box.clientWidth);
    // clientHeight = 50 (height) + 10*2 (padding) = 70
    console.log("clientHeight:", box.clientHeight);
    ```
  - `element.clientTop`: Độ dày của border trên (top border width).
  - `element.clientLeft`: Độ dày của border trái (left border width).

- **Kích thước đầy đủ của nội dung, bao gồm cả phần bị cuộn (Scroll Dimensions):**

  - `element.scrollWidth`: Chiều rộng đầy đủ của nội dung phần tử, kể cả phần không nhìn thấy (bị ẩn do overflow và cần cuộn để xem).
  - `element.scrollHeight`: Chiều cao đầy đủ của nội dung phần tử, kể cả phần không nhìn thấy.
  - Nếu không có cuộn, các giá trị này thường bằng `clientWidth/Height`.
  - Ví dụ:
    ```html
    <style>
      #scrollableDiv {
        width: 150px;
        height: 100px;
        overflow: scroll;
        border: 1px solid #ccc;
      }
      #scrollableDiv div {
        /* Nội dung bên trong lớn hơn container */
        width: 300px;
        height: 200px;
        background-color: lightblue;
      }
    </style>
    <div id="scrollableDiv"><div></div></div>
    <script>
      const scrollDiv = document.getElementById("scrollableDiv");
      console.log("scrollWidth:", scrollDiv.scrollWidth); // Khoảng 300 (hoặc hơn nếu có padding)
      console.log("scrollHeight:", scrollDiv.scrollHeight); // Khoảng 200 (hoặc hơn nếu có padding)
      console.log("clientWidth:", scrollDiv.clientWidth); // Khoảng 150 - scrollbar width
      console.log("clientHeight:", scrollDiv.clientHeight); // Khoảng 100 - scrollbar height
    </script>
    ```

- **Vị trí cuộn hiện tại (Scroll Position):**

  - `element.scrollLeft`: Số pixel mà nội dung của `element` đã được cuộn theo chiều ngang (sang trái). Giá trị từ 0 đến `element.scrollWidth - element.clientWidth`.
  - `element.scrollTop`: Số pixel mà nội dung của `element` đã được cuộn theo chiều dọc (lên trên). Giá trị từ 0 đến `element.scrollHeight - element.clientHeight`.
  - Các thuộc tính này có thể đọc và _ghi_ (để cuộn bằng JavaScript).
  - Ví dụ:

    ```javascript
    // scrollDiv từ ví dụ trên
    // Cuộn xuống dưới 50px
    // scrollDiv.scrollTop = 50;

    // Cuộn sang phải 30px
    // scrollDiv.scrollLeft = 30;

    // scrollDiv.addEventListener('scroll', function() {
    //   console.log(`Scrolled: Left=${this.scrollLeft}, Top=${this.scrollTop}`);
    //   if (this.scrollTop + this.clientHeight >= this.scrollHeight - 5) { // Gần cuối
    //     console.log('Reached the bottom of scrollable content!');
    //   }
    // });
    ```

    Đối với toàn bộ cửa sổ: `window.pageXOffset` (hoặc `window.scrollX`) và `window.pageYOffset` (hoặc `window.scrollY`).

- **Tọa độ tương đối với `offsetParent`:**

  - `element.offsetParent`: Trả về phần tử cha gần nhất có thuộc tính CSS `position` là `relative`, `absolute`, `fixed`, hoặc `sticky`. Nếu không có, hoặc nếu `element` có `position: fixed`, `offsetParent` thường là `<body>` hoặc `<html>` (trong một số trường hợp có thể là `null`).
  - `element.offsetLeft`: Khoảng cách từ mép ngoài bên trái của `element` đến mép trong bên trái của `offsetParent`.
  - `element.offsetTop`: Khoảng cách từ mép ngoài bên trên của `element` đến mép trong bên trên của `offsetParent`.
  - Các giá trị này chỉ đọc.
  - Ví dụ:
    ```html
    <div
      style="position: relative; margin: 50px; padding: 20px; border: 1px solid green;"
    >
      Offset Parent (relative)
      <div
        id="childOffset"
        style="position: absolute; top: 30px; left: 40px; width:50px; height:50px; background:orange;"
      >
        Child
      </div>
    </div>
    <script>
      const child = document.getElementById("childOffset");
      console.log("offsetParent:", child.offsetParent.tagName); // DIV (là div cha có position: relative)
      console.log("offsetLeft:", child.offsetLeft); // 40 (tính từ padding của cha, nếu box-sizing: content-box cho cha)
      // hoặc 40 (tính từ border của cha, nếu box-sizing: border-box cho cha)
      // Chính xác hơn: là giá trị left được đặt cho nó, vì nó là absolute
      console.log("offsetTop:", child.offsetTop); // 30
    </script>
    ```
    `offsetLeft/Top` tính toán phức tạp hơn khi `offsetParent` có border và `element` không phải là `position: absolute/fixed`.

- **Tọa độ tương đối với Viewport (Cửa sổ trình duyệt): `getBoundingClientRect()`**

  - `element.getBoundingClientRect()`: Phương thức quan trọng nhất để lấy kích thước và vị trí của một phần tử tương đối với viewport (khung nhìn của trình duyệt).
  - Trả về một đối tượng `DOMRect` với các thuộc tính:
    - `x`, `left`: Tọa độ X của mép trái phần tử so với viewport.
    - `y`, `top`: Tọa độ Y của mép trên phần tử so với viewport.
    - `right`: Tọa độ X của mép phải phần tử so với viewport (`left + width`).
    - `bottom`: Tọa độ Y của mép dưới phần tử so với viewport (`top + height`).
    - `width`: Chiều rộng của phần tử (tương đương `offsetWidth`).
    - `height`: Chiều cao của phần tử (tương đương `offsetHeight`).
  - Các giá trị này thay đổi khi cuộn trang.
  - Các giá trị có thể là số thập phân.
  - Ví dụ:
    ```html
    <div
      id="rectTarget"
      style="width: 200px; height: 100px; margin: 150px; background: lightcoral;"
    >
      Get Bounding Rect
    </div>
    <script>
      const rectTarget = document.getElementById("rectTarget");
      function logRect() {
        const rect = rectTarget.getBoundingClientRect();
        console.log("Bounding Rect:", rect);
        console.log(`Top: ${rect.top}, Left: ${rect.left}`);
        console.log(`Width: ${rect.width}, Height: ${rect.height}`);
        console.log(`Bottom: ${rect.bottom}, Right: ${rect.right}`);
      }
      logRect(); // Log vị trí ban đầu
      // window.addEventListener('scroll', logRect); // Log khi cuộn
    </script>
    ```

- **Lấy phần tử tại một tọa độ cụ thể: `document.elementFromPoint(x, y)`**

  - Trả về phần tử nằm trên cùng (topmost) tại tọa độ `(x, y)` trong viewport.
  - Nếu tọa độ nằm ngoài viewport, hoặc không có phần tử nào ở đó, trả về `null`.
  - `x`, `y` là tọa độ tương đối với viewport.
  - Ví dụ:
    ```javascript
    document.addEventListener("click", function (event) {
      const clickedElement = document.elementFromPoint(
        event.clientX,
        event.clientY
      );
      if (clickedElement) {
        console.log(
          "Clicked on:",
          clickedElement.tagName,
          clickedElement.id || ""
        );
        // clickedElement có thể khác event.target nếu có các lớp phủ (overlay) trong suốt
      }
    });
    ```

- **Tọa độ của Toàn bộ Document:**
  - Để lấy tọa độ của một phần tử so với toàn bộ tài liệu (không phải viewport), bạn cần kết hợp `getBoundingClientRect()` với vị trí cuộn của trang:
    ```javascript
    function getElementCoords(element) {
      const rect = element.getBoundingClientRect();
      return {
        top: rect.top + window.pageYOffset,
        left: rect.left + window.pageXOffset,
        bottom: rect.bottom + window.pageYOffset,
        right: rect.right + window.pageXOffset,
        width: rect.width,
        height: rect.height,
      };
    }
    // const coords = getElementCoords(document.getElementById('myElement'));
    // console.log('Coords relative to document:', coords);
    ```

**5. Các hàm cuộn (Scrolling):**

- **`element.scrollIntoView(alignToTopOrOptions)`:**

  - Cuộn container cha của `element` (hoặc toàn bộ cửa sổ) sao cho `element` trở nên nhìn thấy được trong viewport.
  - `alignToTopOrOptions`:
    - Boolean `true` (mặc định): Mép trên của `element` sẽ được căn chỉnh với mép trên của viewport.
    - Boolean `false`: Mép dưới của `element` sẽ được căn chỉnh với mép dưới của viewport.
    - Đối tượng `options`:
      - `behavior`: `'auto'` (mặc định, cuộn tức thì) hoặc `'smooth'` (cuộn mượt).
      - `block`: Vị trí căn chỉnh theo chiều dọc: `'start'` (giống `true`), `'center'`, `'end'` (giống `false`), `'nearest'`.
      - `inline`: Vị trí căn chỉnh theo chiều ngang: `'start'`, `'center'`, `'end'`, `'nearest'`.
  - Ví dụ:
    ```html
    <div style="height: 1500px;">Spacer</div>
    <button id="scrollToMeBtn">Click to see me</button>
    <div
      id="targetForScroll"
      style="margin-top: 1000px; background: lightblue; padding: 20px;"
    >
      Target Element
    </div>
    <script>
      const btn = document.getElementById("scrollToMeBtn");
      const target = document.getElementById("targetForScroll");
      btn.addEventListener("click", () => {
        // target.scrollIntoView(); // Mặc định alignToTop: true, behavior: 'auto'
        // target.scrollIntoView(false); // Align to bottom
        target.scrollIntoView({
          behavior: "smooth",
          block: "center", // Căn giữa theo chiều dọc
          inline: "nearest", // Căn ngang nếu cần
        });
      });
    </script>
    ```

- **`window.scrollTo(x, y)` hoặc `window.scrollTo(options)`:**
  - Cuộn cửa sổ đến một tọa độ tuyệt đối `(x, y)` trong tài liệu.
  - `options`: `{ top: y, left: x, behavior: 'smooth' }`
- **`window.scrollBy(x, y)` hoặc `window.scrollBy(options)`:**
  - Cuộn cửa sổ một khoảng tương đối `(x, y)` so với vị trí hiện tại.
  - `options`: `{ top: yOffset, left: xOffset, behavior: 'smooth' }`
- Tương tự cho các phần tử có thể cuộn: `element.scrollTo(...)` và `element.scrollBy(...)`.

**Best Practices:**

- **Caching `getBoundingClientRect()`:** Nếu bạn cần nhiều giá trị từ `getBoundingClientRect()` của cùng một phần tử trong cùng một luồng thực thi, hãy gọi nó một lần và lưu kết quả vào biến, vì mỗi lần gọi nó có thể kích hoạt reflow.
- **Box Sizing:** Nhớ rằng `box-sizing: border-box;` ảnh hưởng đến cách `width` và `height` trong CSS được diễn giải, và do đó ảnh hưởng đến `offsetWidth/Height` và `clientWidth/Height`. Với `border-box`, `width`/`height` trong CSS bao gồm cả padding và border.
- **Tránh Trigger Layout Thrashing:** "Layout thrashing" (hay "forced synchronous layout") xảy ra khi bạn liên tục đọc các thuộc tính layout (như `offsetWidth`, `getBoundingClientRect().top`) và sau đó ghi (thay đổi style, class, ...) trong một vòng lặp. Mỗi lần đọc sau một lần ghi có thể buộc trình duyệt phải tính toán lại layout.

  - **Cách tránh:** Thực hiện tất cả các thao tác đọc trước, lưu trữ giá trị, sau đó thực hiện tất cả các thao tác ghi.

  ```javascript
  // XẤU: Layout Thrashing
  // function updateElementsBad(elements) {
  //   for (let i = 0; i < elements.length; i++) {
  //     elements[i].style.width = elements[i].offsetWidth / 2 + 'px'; // Đọc rồi Ghi
  //   }
  // }

  // TỐT: Tránh Layout Thrashing
  function updateElementsGood(elements) {
    const widths = [];
    // 1. Đọc tất cả
    for (let i = 0; i < elements.length; i++) {
      widths.push(elements[i].offsetWidth);
    }
    // 2. Ghi tất cả
    for (let i = 0; i < elements.length; i++) {
      elements[i].style.width = widths[i] / 2 + "px";
    }
  }
  ```

Việc hiểu rõ và sử dụng thành thạo các API này cho phép bạn tạo ra các giao diện phức tạp, đáp ứng, và có các hiệu ứng động chính xác mà không cần đến thư viện bên ngoài.

**Phần 5: Templates, Shadow DOM và Khởi Đầu Với Web Components**

Để xây dựng các giao diện người dùng phức tạp và có tổ chức, đặc biệt là khi hướng tới việc tạo framework riêng, việc đóng gói và tái sử dụng các thành phần UI là cực kỳ quan trọng. Thẻ `<template>` và Shadow DOM là những công cụ mạnh mẽ của Vanilla JavaScript giúp bạn đạt được điều này, đặt nền móng cho Web Components.

**1. Thẻ `<template>`: Khai Báo Mảnh HTML Tái Sử Dụng**

Thẻ `<template>` cho phép bạn khai báo các mảnh cấu trúc HTML mà không được render ngay lập tức khi trang tải. Nội dung bên trong `<template>` là _inert_ (trơ):

- Nó không được hiển thị trên trang.
- Script bên trong nó không chạy.
- Hình ảnh không được tải.
- CSS không được áp dụng.
  Nó chỉ đơn giản là một "khuôn mẫu" HTML mà bạn có thể lấy ra và sử dụng khi cần bằng JavaScript.

- **Mục đích và Lợi ích:**

  - **Tái sử dụng HTML:** Định nghĩa một cấu trúc một lần và nhân bản nó nhiều lần.
  - **Hiệu suất:** Trình duyệt parse nội dung template một lần. Việc clone node từ template thường nhanh hơn việc tạo nhiều phần tử DOM từ đầu bằng `document.createElement` hoặc parse chuỗi HTML nhiều lần với `innerHTML`.
  - **Tổ chức code:** Giữ các mảnh HTML phức tạp tách biệt khỏi logic JavaScript chính.

- **Cách sử dụng:**

  1.  Định nghĩa thẻ `<template>` trong HTML của bạn, thường với một `id` để dễ dàng truy cập.
  2.  Trong JavaScript, lấy đối tượng template.
  3.  Truy cập thuộc tính `content` của template. Đây là một `DocumentFragment` chứa bản sao của nội dung template.
  4.  Sử dụng `cloneNode(true)` trên `template.content` để tạo một bản sao sâu của nội dung.
  5.  Thao tác với bản sao này (thêm dữ liệu, gắn sự kiện) rồi chèn vào DOM.

- **Ví dụ: Template cho một Card Sản Phẩm**

  ```html
  <template id="product-card-template">
    <div class="product-card">
      <img src="" alt="Product Image" class="product-image" />
      <h3 class="product-name">Tên Sản Phẩm</h3>
      <p class="product-price">Giá: <span class="price-value">0</span> VND</p>
      <button class="add-to-cart-btn">Thêm vào giỏ</button>
    </div>
  </template>

  <div id="product-list"></div>

  <script>
    const productListContainer = document.getElementById("product-list");
    const productTemplate = document.getElementById("product-card-template");

    const productsData = [
      {
        id: 1,
        name: "Laptop ABC",
        price: 25000000,
        imageUrl: "https://via.placeholder.com/150/FF0000/FFFFFF?Text=Laptop",
      },
      {
        id: 2,
        name: "Điện thoại XYZ",
        price: 15000000,
        imageUrl: "https://via.placeholder.com/150/00FF00/FFFFFF?Text=Phone",
      },
      {
        id: 3,
        name: "Máy tính bảng QRS",
        price: 8000000,
        imageUrl: "https://via.placeholder.com/150/0000FF/FFFFFF?Text=Tablet",
      },
    ];

    productsData.forEach((productData) => {
      // 1. Clone nội dung từ template
      const cardClone = productTemplate.content.cloneNode(true); // true để clone sâu

      // 2. Điền dữ liệu vào bản clone
      cardClone.querySelector(".product-image").src = productData.imageUrl;
      cardClone.querySelector(".product-image").alt = productData.name;
      cardClone.querySelector(".product-name").textContent = productData.name;
      cardClone.querySelector(".price-value").textContent =
        productData.price.toLocaleString();

      const addToCartBtn = cardClone.querySelector(".add-to-cart-btn");
      addToCartBtn.dataset.productId = productData.id; // Lưu ID sản phẩm
      addToCartBtn.addEventListener("click", function () {
        alert(
          `Đã thêm sản phẩm "${productData.name}" (ID: ${this.dataset.productId}) vào giỏ!`
        );
      });

      // 3. Thêm bản clone vào DOM
      productListContainer.appendChild(cardClone);
    });
  </script>
  <style>
    .product-card {
      border: 1px solid #ccc;
      padding: 10px;
      margin: 10px;
      width: 200px;
      display: inline-block;
      vertical-align: top;
    }
    .product-card img {
      max-width: 100%;
      height: auto;
      margin-bottom: 5px;
    }
    .product-card h3 {
      margin: 5px 0;
      font-size: 1.1em;
    }
    .product-card p {
      margin: 5px 0;
    }
  </style>
  ```

  Trong ví dụ này, cấu trúc HTML của card sản phẩm được định nghĩa một lần trong `<template>`. Sau đó, chúng ta lặp qua dữ liệu sản phẩm, clone template, điền dữ liệu và thêm vào DOM.

**2. Giới Thiệu Shadow DOM: Đóng Gói DOM và Style**

Shadow DOM là một công nghệ trình duyệt cho phép bạn tạo ra một cây DOM "ẩn" (shadow tree) được gắn vào một phần tử (shadow host) trong DOM chính (light DOM). Shadow DOM cho phép đóng gói hoàn toàn, nghĩa là:

- **CSS Scoping (Phạm vi CSS):** Style được định nghĩa bên trong Shadow DOM chỉ áp dụng cho các phần tử bên trong shadow tree đó và (thường) không bị ảnh hưởng bởi style từ bên ngoài. Ngược lại, style từ DOM chính (thường) không "xuyên thủng" vào Shadow DOM. Điều này giải quyết một trong những vấn đề lớn nhất của CSS: xung đột tên class và style toàn cục.
- **DOM Encapsulation (Đóng gói DOM):** Cấu trúc DOM bên trong shadow tree được ẩn đi khỏi các truy vấn DOM từ bên ngoài (ví dụ: `document.querySelector` từ DOM chính sẽ không tìm thấy các phần tử bên trong shadow tree của một component). Điều này tạo ra các component "hộp đen" thực sự, nơi chi tiết triển khai được che giấu.

- **Thuật ngữ:**

  - **Shadow Host:** Phần tử DOM thông thường mà Shadow DOM được gắn vào.
  - **Shadow Root:** Gốc của shadow tree. Nó là một `DocumentFragment` đặc biệt.
  - **Shadow Tree:** Cây DOM bên trong Shadow Root.
  - **Light DOM:** DOM thông thường bên ngoài Shadow DOM. Các con của Shadow Host trong Light DOM có thể được "chiếu" vào Shadow DOM thông qua slots.

- **Cách tạo Shadow Root:**
  Sử dụng phương thức `element.attachShadow({ mode: 'open' | 'closed' })` trên shadow host.

  - `mode: 'open'`: Shadow root có thể được truy cập từ JavaScript bên ngoài thông qua thuộc tính `element.shadowRoot`. Đây là chế độ phổ biến và được khuyến khích.
  - `mode: 'closed'`: Shadow root không thể truy cập từ JavaScript bên ngoài (`element.shadowRoot` sẽ là `null`). Điều này tạo ra sự đóng gói mạnh mẽ hơn nhưng cũng khó debug và ít linh hoạt hơn. Ít được sử dụng.

- **Thêm nội dung vào Shadow Root:**
  Sau khi có shadow root, bạn có thể thao tác với nó giống như một `DocumentFragment` thông thường:
  `shadowRoot.innerHTML = '...';`
  `shadowRoot.appendChild(someElement);`
  `shadowRoot.querySelector(...)` (tìm bên trong shadow tree).

- **Ví dụ: Tạo một "Hello World" Component với Shadow DOM**

  ```html
  <div id="shadow-host-element">
    Nội dung này là Light DOM, sẽ bị ẩn nếu Shadow DOM có nội dung.
  </div>

  <script>
    const host = document.getElementById("shadow-host-element");

    // 1. Gắn Shadow DOM vào host
    const shadow = host.attachShadow({ mode: "open" });

    // 2. Tạo và thêm nội dung vào Shadow Root
    const wrapper = document.createElement("div");
    wrapper.setAttribute("class", "shadow-content"); // Class này chỉ có ý nghĩa trong shadow DOM

    const heading = document.createElement("h2");
    heading.textContent = "Chào bạn từ Shadow DOM!";

    const paragraph = document.createElement("p");
    paragraph.textContent = "Style của tôi được đóng gói.";

    // 3. Tạo style riêng cho Shadow DOM
    const style = document.createElement("style");
    style.textContent = `
      /* Style này chỉ áp dụng bên trong Shadow DOM này */
      .shadow-content {
        padding: 20px;
        background-color: lightblue;
        border: 2px dashed navy;
        border-radius: 5px;
      }
      h2 {
        color: navy;
      }
      p {
        font-style: italic;
        color: #333;
      }
      /* Thử style một thẻ p bên ngoài để thấy sự khác biệt */
    `;

    // Thêm các phần tử vào Shadow Root
    shadow.appendChild(style);
    wrapper.appendChild(heading);
    wrapper.appendChild(paragraph);
    shadow.appendChild(wrapper);

    // Bây giờ, nếu bạn inspect #shadow-host-element, bạn sẽ thấy #shadow-root (open)
    // Nội dung "Nội dung này là Light DOM..." sẽ không hiển thị trừ khi Shadow DOM trống
    // hoặc chúng ta sử dụng <slot>
    console.log(host.shadowRoot); // Truy cập được shadow root
    // console.log(shadow.querySelector('h2').textContent); // "Chào bạn từ Shadow DOM!"
    // console.log(document.querySelector('h2')); // Sẽ không tìm thấy h2 trong shadow DOM (nếu có h2 khác bên ngoài)
  </script>

  <p>
    Đây là một thẻ p bên ngoài Shadow DOM, nó không bị ảnh hưởng bởi style trong
    shadow.
  </p>
  <style>
    /* Style toàn cục này sẽ không ảnh hưởng h2, p bên trong shadow DOM ở trên */
    h2 {
      color: red !important; /* Kể cả !important cũng thường không xuyên qua */
    }
    p {
      font-weight: bold;
    }
  </style>
  ```

  Trong ví dụ này, `div#shadow-host-element` là shadow host. Style bên trong `<style>` của shadow root chỉ áp dụng cho `h2` và `p` bên trong nó. Style `h2` và `p` toàn cục không ảnh hưởng đến các phần tử tương ứng trong shadow DOM.

**3. Slots trong Shadow DOM (`<slot>`): Chiếu Nội Dung Từ Light DOM**

Shadow DOM cho phép đóng gói, nhưng đôi khi bạn muốn component của mình linh hoạt hơn bằng cách cho phép người dùng truyền nội dung của riêng họ vào các vị trí cụ thể bên trong component. Đây là lúc `<slot>` phát huy tác dụng.
Thẻ `<slot>` hoạt động như một placeholder bên trong shadow tree, nơi nội dung từ light DOM (con của shadow host) có thể được "chiếu" (projected) vào.

- **Slot Mặc định (Unnamed Slot): `<slot></slot>`**

  - Một slot không có thuộc tính `name` sẽ hiển thị tất cả các node con trực tiếp của shadow host mà không được gán cho một named slot cụ thể.
  - Chỉ có thể có một slot mặc định trong một shadow tree.

- **Slot Có Tên (Named Slot): `<slot name="some-name"></slot>`**

  - Bạn có thể tạo các slot được đặt tên.
  - Để truyền nội dung vào một named slot, các phần tử con trong light DOM của shadow host cần có thuộc tính `slot="some-name"`.

- **Styling Nội dung Được Chiếu (`::slotted()` pseudo-element):**

  - Bạn có thể style các phần tử từ light DOM đã được chiếu vào slot bằng cách sử dụng `::slotted(selector)` bên trong style của shadow DOM.
  - Ví dụ: `slot[name="header"]::slotted(h1)` sẽ style các thẻ `<h1>` được chiếu vào slot có tên là "header".
  - Lưu ý: `::slotted()` chỉ có thể nhắm mục tiêu các phần tử con trực tiếp được chiếu vào slot. Nó không thể style sâu hơn vào cấu trúc của phần tử được chiếu.

- **Ví dụ: Component Card với Slots**

  ```html
  <!-- Định nghĩa template cho component card -->
  <template id="slotted-card-template">
    <style>
      /* :host là selector để style chính shadow host từ bên trong shadow DOM */
      :host {
        display: block; /* Custom elements mặc định là inline */
        border: 1px solid #ccc;
        border-radius: 8px;
        padding: 15px;
        margin: 10px;
        box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.1);
        font-family: sans-serif;
      }
      :host([theme="dark"]) {
        /* Styling host dựa trên thuộc tính của nó */
        background-color: #333;
        color: white;
        border-color: #555;
      }
      :host([hidden]) {
        /* :host có thể dùng pseudo-classes */
        display: none;
      }
      .card-header {
        border-bottom: 1px solid #eee;
        padding-bottom: 10px;
        margin-bottom: 10px;
      }
      .card-footer {
        border-top: 1px solid #eee;
        padding-top: 10px;
        margin-top: 10px;
        font-size: 0.9em;
        color: #777;
      }
      /* Styling slotted content */
      ::slotted(h2) {
        /* Style mọi h2 được chiếu vào bất kỳ slot nào */
        margin: 0;
        color: purple;
      }
      ::slotted([slot="footer-content"]) {
        /* Style phần tử được chiếu vào slot 'footer-content' */
        font-style: italic;
      }
      /* Slot fallback content: Nội dung mặc định nếu không có gì được chiếu vào slot */
      slot[name="card-title"]::before {
        content: "Tiêu đề mặc định: ";
        font-weight: normal;
        color: #999;
      }
      slot:not([name])::before {
        /* Fallback cho default slot */
        content: "Nội dung mặc định cho card...";
        color: #aaa;
        display: block; /* Để hiển thị nếu slot trống */
      }
    </style>
    <div class="card-header">
      <slot name="card-title">
        <!-- Fallback content: Sẽ hiển thị nếu không có gì được chiếu vào slot "card-title" -->
        <strong>Tiêu đề mặc định nếu không có gì slot vào</strong>
      </slot>
    </div>
    <div class="card-content">
      <slot>
        <!-- Fallback content cho slot mặc định -->
        <p>Đây là nội dung mặc định của card.</p>
      </slot>
    </div>
    <div class="card-footer">
      <slot name="footer-content">Thông tin chân trang mặc định.</slot>
    </div>
  </template>

  <!-- Sử dụng component card -->
  <div id="card1-host">
    <!-- Nội dung này sẽ được chiếu vào các slot -->
    <h2 slot="card-title">Thẻ Sản Phẩm A</h2>
    <p>Đây là mô tả chi tiết cho sản phẩm A. Rất tuyệt vời!</p>
    <small slot="footer-content">Cập nhật: 01/01/2024</small>
  </div>

  <div id="card2-host" theme="dark">
    <!-- Thử :host([theme="dark"]) -->
    <!-- Thẻ này chỉ sử dụng một vài slot, các slot khác sẽ dùng fallback content -->
    <h3 slot="card-title">Thẻ Tin Tức B</h3>
    <!-- Không có nội dung cho slot mặc định, sẽ dùng fallback -->
    <!-- Không có nội dung cho slot footer-content, sẽ dùng fallback -->
  </div>

  <div id="card3-host">
    <!-- Không có gì được chiếu vào, tất cả sẽ là fallback -->
  </div>

  <script>
    function createCard(hostElementId, templateId) {
      const host = document.getElementById(hostElementId);
      const template = document.getElementById(templateId);
      const shadowRoot = host.attachShadow({ mode: "open" });
      shadowRoot.appendChild(template.content.cloneNode(true));
    }

    createCard("card1-host", "slotted-card-template");
    createCard("card2-host", "slotted-card-template");
    createCard("card3-host", "slotted-card-template");

    // setTimeout(() => {
    //     document.getElementById('card2-host').setAttribute('hidden', ''); // Thử :host([hidden])
    // }, 3000);
  </script>
  ```

  Trong ví dụ này:

  - `template#slotted-card-template` định nghĩa cấu trúc của card với ba slots: `card-title`, một slot mặc định (không tên) cho nội dung chính, và `footer-content`.
  - `div#card1-host` cung cấp nội dung cho tất cả các slot. `h2` có `slot="card-title"` sẽ được chiếu vào `<slot name="card-title">`. `p` không có `slot` sẽ vào slot mặc định. `small` có `slot="footer-content"` sẽ vào slot tương ứng.
  - `div#card2-host` chỉ cung cấp `card-title`. Slot mặc định và `footer-content` sẽ hiển thị nội dung fallback của chúng (nội dung bên trong thẻ `<slot>` trong template).
  - `::slotted(h2)` và `::slotted([slot="footer-content"])` minh họa cách style các node được chiếu.
  - `:host` selector được dùng để style chính phần tử host từ bên trong shadow DOM của nó. Nó rất mạnh mẽ, có thể dùng với các thuộc tính (`:host([theme="dark"])`) hoặc pseudo-classes (`:host(:hover)`).

**4. Sơ Lược về Custom Elements (Giới thiệu Web Components)**

Templates và Shadow DOM là hai trong ba công nghệ chính tạo nên Web Components. Công nghệ thứ ba là **Custom Elements**.

- **Custom Elements** cho phép bạn định nghĩa các thẻ HTML của riêng mình, với tên thẻ tùy chỉnh (phải chứa dấu gạch ngang, ví dụ: `<my-component>`, `<user-profile-card>`).
- Bạn tạo một class JavaScript kế thừa từ `HTMLElement`, sau đó đăng ký class đó với một tên thẻ bằng `customElements.define('tag-name', YourClass)`.
- Bên trong class này, bạn có thể:

  - Sử dụng Shadow DOM để đóng gói style và markup (thường trong `constructor`).
  - Định nghĩa các phương thức và thuộc tính riêng.
  - Sử dụng các "lifecycle callbacks" (phương thức vòng đời) như `connectedCallback()` (khi phần tử được thêm vào DOM), `disconnectedCallback()` (khi bị xóa), `attributeChangedCallback()` (khi thuộc tính thay đổi).

- **Ví dụ rất cơ bản về Custom Element (sẽ đi sâu hơn ở phần sau):**

  ```html
  <!-- 1. Định nghĩa template (tùy chọn, nhưng thường dùng) -->
  <template id="my-element-template">
    <style>
      p {
        color: green;
        font-weight: bold;
      }
    </style>
    <p>Xin chào từ <slot name="element-name">My Custom Element</slot>!</p>
  </template>

  <!-- 2. Sử dụng custom element -->
  <my-custom-element id="el1">
    <span slot="element-name">Thế Giới JavaScript</span>
  </my-custom-element>
  <my-custom-element id="el2"></my-custom-element>

  <script>
    // 3. Định nghĩa class cho custom element
    class MyCustomElement extends HTMLElement {
      constructor() {
        super(); // Luôn gọi super() đầu tiên trong constructor

        // Gắn Shadow DOM
        this.attachShadow({ mode: "open" });

        // Lấy template
        const template = document.getElementById("my-element-template");
        if (template) {
          this.shadowRoot.appendChild(template.content.cloneNode(true));
        } else {
          this.shadowRoot.innerHTML = `<p>Lỗi: không tìm thấy template!</p>`;
        }
        console.log("MyCustomElement constructor called");
      }

      // Lifecycle callback: được gọi khi phần tử được kết nối vào DOM
      connectedCallback() {
        console.log(
          `Custom element <${this.tagName.toLowerCase()}> (ID: ${
            this.id
          }) added to page.`
        );
        // Có thể thực hiện các thiết lập DOM, gắn sự kiện ở đây
        // Ví dụ, lấy giá trị thuộc tính:
        // if (this.hasAttribute('user-name')) {
        //   this.shadowRoot.querySelector('p').textContent += ` Chào ${this.getAttribute('user-name')}!`;
        // }
      }

      // Các lifecycle callbacks khác: disconnectedCallback, attributeChangedCallback
    }

    // 4. Đăng ký custom element với trình duyệt
    customElements.define("my-custom-element", MyCustomElement);
  </script>
  ```

  Bây giờ, bạn có thể sử dụng `<my-custom-element>` trong HTML như một thẻ HTML thông thường. Mỗi instance sẽ có Shadow DOM riêng, đóng gói và có thể nhận nội dung qua slot.

**5. Best Practices khi sử dụng Templates và Shadow DOM:**

- **Sử dụng `<template>` cho các cấu trúc HTML lặp lại:** Nó hiệu quả và giữ code sạch sẽ.
- **Sử dụng Shadow DOM khi bạn cần đóng gói mạnh mẽ:** Đặc biệt hữu ích cho các component UI độc lập, thư viện component, hoặc khi tích hợp các phần widget vào ứng dụng lớn mà không muốn xung đột style.
- **Ưu tiên `mode: 'open'` cho Shadow DOM:** Nó giúp việc debug dễ dàng hơn và cho phép tương tác (nếu cần) từ bên ngoài. `mode: 'closed'` ít thực tế trong hầu hết các trường hợp.
- **Sử dụng `:host` để style phần tử host:** Cho phép component tự style chính nó.
- **Sử dụng `:host-context(selector)` để style host dựa trên tổ tiên:** Ví dụ, `:host-context(.dark-theme)` sẽ áp dụng style nếu host là con cháu của một phần tử có class `dark-theme`.
- **Thiết kế Slots cẩn thận:** Suy nghĩ về API của component: người dùng sẽ muốn tùy chỉnh những phần nào? Đặt tên slot rõ ràng. Cung cấp fallback content hợp lý cho các slot.
- **Kết hợp với Custom Elements:** Sức mạnh thực sự của Templates và Shadow DOM được phát huy tối đa khi chúng được sử dụng bên trong Custom Elements để tạo ra các Web Components hoàn chỉnh.

Phần này đã cung cấp các công cụ cơ bản để bạn bắt đầu xây dựng các thành phần UI có cấu trúc, đóng gói và tái sử dụng. Trong các phần tiếp theo, chúng ta sẽ đi sâu hơn vào Custom Elements và các khía cạnh khác của việc xây dựng các ứng dụng web phức tạp bằng Vanilla JavaScript.

**Phần 6: JavaScript Modules (ES Modules) và Tổ Chức Code**

Khi ứng dụng JavaScript của bạn lớn dần, việc giữ cho mã nguồn được tổ chức tốt, dễ bảo trì và dễ hiểu trở nên cực kỳ quan trọng. JavaScript Modules (thường được gọi là ES Modules hoặc ESM) là cơ chế chuẩn của JavaScript để chia nhỏ code thành các file (module) riêng biệt, nơi mỗi module có thể export (xuất ra) các biến, hàm, hoặc class, và import (nhập vào) những gì nó cần từ các module khác.

**1. Tại Sao Cần Modules?**

Trước khi có ES Modules, việc quản lý code JavaScript lớn thường dựa vào:

- **Global Scope Pollution:** Tất cả các script được tải vào trang đều chia sẻ cùng một global scope (`window`). Điều này dễ dẫn đến xung đột tên biến/hàm.
- **Thứ Tự Script Quan Trọng:** Phải đảm bảo các file script được tải theo đúng thứ tự phụ thuộc.
- **Khó Tái Sử Dụng và Bảo Trì:** Khó tách biệt các phần logic, làm code trở nên rối rắm.
- **Các Giải Pháp Tạm Thời:** Các patterns như IIFE (Immediately Invoked Function Expressions) để tạo scope riêng, hoặc các hệ thống module không chuẩn (AMD, CommonJS - dùng trong Node.js).

ES Modules giải quyết các vấn đề này bằng cách cung cấp một cách chuẩn hóa để:

- **Encapsulation (Đóng gói):** Mỗi module có scope riêng. Biến và hàm khai báo trong module không tự động trở thành global.
- **Explicit Dependencies (Phụ Thuộc Rõ Ràng):** Bạn phải `import` những gì bạn cần, làm cho các mối quan hệ giữa các phần của code trở nên rõ ràng.
- **Reusability (Tái sử dụng):** Dễ dàng sử dụng lại code từ các module khác.
- **Maintainability (Dễ bảo trì):** Code được chia thành các đơn vị nhỏ hơn, dễ quản lý hơn.
- **No Global Namespace Pollution:** Giảm thiểu xung đột tên.

**2. Cú Pháp Cơ Bản của ES Modules:**

- **`export` (Xuất):**

  - Để làm cho một biến, hàm, hoặc class từ một module có thể được sử dụng bởi các module khác, bạn cần `export` nó.
  - Có hai loại export chính:
    - **Named Exports (Xuất có tên):** Xuất nhiều mục từ một module, mỗi mục có tên riêng.
    - **Default Export (Xuất mặc định):** Mỗi module chỉ có thể có _một_ default export. Thường dùng cho "thứ chính" mà module đó cung cấp.

- **`import` (Nhập):**
  - Để sử dụng các mục đã được export từ một module khác, bạn dùng `import`.
  - Cú pháp import tương ứng với cách export.

**3. Named Exports và Imports:**

- **Exporting (trong `utils.js`):**

  ```javascript
  // utils.js

  // Cách 1: Export khi khai báo
  export const PI = 3.14159;

  export function add(a, b) {
    return a + b;
  }

  export class User {
    constructor(name) {
      this.name = name;
    }
    greet() {
      return `Hello, ${this.name}!`;
    }
  }

  // Cách 2: Export một danh sách ở cuối file
  const SECRET_KEY = "mysecret";
  function multiply(a, b) {
    return a * b;
  }

  // export { SECRET_KEY, multiply }; // Nếu muốn export thêm
  ```

  _Lưu ý:_ `SECRET_KEY` và `multiply` (phiên bản không có `export` ở đầu) nếu không được liệt kê trong `export { ... }` thì sẽ là private cho module `utils.js`.

- **Importing (trong `main.js`):**

  ```javascript
  // main.js
  // Cách 1: Import cụ thể các mục cần thiết
  import { PI, add, User } from "./utils.js"; // Đường dẫn tương đối hoặc tuyệt đối
  // .js là cần thiết trên trình duyệt
  // (có thể không cần trong một số bundler/Node.js)

  console.log("PI:", PI); // 3.14159
  console.log("Sum:", add(5, 3)); // 8

  const user = new User("Alice");
  console.log(user.greet()); // Hello, Alice!

  // Cách 2: Import tất cả các named exports vào một object (namespace import)
  // import * as Utils from './utils.js';
  // console.log('Utils.PI:', Utils.PI);
  // console.log('Utils.add(10, 2):', Utils.add(10, 2));
  // const anotherUser = new Utils.User('Bob');
  // console.log(anotherUser.greet());

  // Cách 3: Import với renaming (alias)
  // import { PI as MyPI, add as sumNumbers } from './utils.js';
  // console.log('My PI:', MyPI);
  // console.log('Sum:', sumNumbers(7, 7));
  ```

**4. Default Export và Import:**

- **Exporting (trong `logger.js`):**

  ```javascript
  // logger.js
  // Chỉ có một default export cho mỗi module
  export default function logMessage(message) {
    console.log(`[LOG]: ${message}`);
  }

  // Bạn vẫn có thể có named exports cùng với default export
  export const INFO_LEVEL = "INFO";
  // export default class MyClass { ... }
  // const myInstance = new MyClass(); export default myInstance;
  ```

- **Importing (trong `app.js`):**

  ```javascript
  // app.js
  // Import default export - bạn có thể đặt tên bất kỳ cho nó
  import myCustomLoggerName from "./logger.js";
  // import logger from './logger.js'; // Tên thường dùng là tên file hoặc ý nghĩa của nó

  myCustomLoggerName("Application started."); // [LOG]: Application started.

  // Nếu module logger.js có cả default và named exports:
  import loggerFunc, { INFO_LEVEL } from "./logger.js";
  // loggerFunc('Another message.');
  // console.log('Log Level:', INFO_LEVEL);
  ```

  - Khi import một default export, bạn không dùng dấu ngoặc nhọn `{}`.

**5. Kết Hợp Named và Default Exports/Imports:**

Như đã thấy ở ví dụ `logger.js` và `app.js` ở trên, một module có thể có cả default export và nhiều named exports.

```javascript
// module.js
export default function mainFunction() {
  /* ... */
}
export const helperA = () => {
  /* ... */
};
export let config = {
  /* ... */
};

// mainApp.js
import mainFunc, { helperA, config as appConfig } from "./module.js";
mainFunc();
helperA();
console.log(appConfig);
```

**6. Sử Dụng Modules trong Trình Duyệt:**

Để trình duyệt hiểu và xử lý các file JavaScript như là ES Modules, bạn cần thêm `type="module"` vào thẻ `<script>`:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>ES Modules Example</title>
    <!-- Cách 1: Script chính là module -->
    <script type="module" src="main.js"></script>

    <!-- Cách 2: Inline module script (ít phổ biến hơn cho file lớn) -->
    <!--
  <script type="module">
    import { someFunction } from './anotherModule.js';
    someFunction();
    console.log('Inline module executed.');
  </script>
  --></head>
  <body>
    <h1>Hello ES Modules!</h1>
  </body>
</html>
```

**Đặc điểm của `<script type="module">`:**

- **Strict Mode Mặc Định:** Code bên trong module luôn chạy ở strict mode, không cần `'use strict';`.
- **Scope Riêng:** Biến khai báo ở top-level của module không tự động trở thành global (không gắn vào `window`).
  ```javascript
  // main.js (type="module")
  var myGlobalVar = "I am not global"; // Không thể truy cập window.myGlobalVar
  let myModuleScopedVar = "Only in module";
  // window.myGlobalVar = "Now I am global"; // Phải gán tường minh nếu muốn
  ```
- **Deferred by Default:** Các module script được trì hoãn (defer) theo mặc định. Nghĩa là:
  - Chúng được tải không đồng bộ (song song với việc parse HTML).
  - Chúng được thực thi theo thứ tự xuất hiện trong HTML, _sau khi_ tài liệu HTML đã được parse xong (tương tự `defer` trên script thường).
  - Nếu bạn muốn script module chạy ngay sau khi tải xong và có thể chặn việc parse HTML (giống script thường không có `async` hay `defer`), bạn có thể dùng `async` với `type="module"`: `<script type="module" async src="app.js"></script>`. Script `async` không đảm bảo thứ tự thực thi.
- **CORS Required for Cross-Origin Modules:** Nếu bạn import module từ một domain khác, server đó phải có CORS headers phù hợp (ví dụ `Access-Control-Allow-Origin: *`).
- **Đường Dẫn Phải Tương Đối hoặc Tuyệt Đối:**
  - `import { x } from './utils.js';` (Tương đối, cùng thư mục)
  - `import { y } from '../lib/helpers.js';` (Tương đối, thư mục cha)
  - `import { z } from '/modules/core.js';` (Tuyệt đối từ gốc domain)
  - `import { w } from 'https://unpkg.com/lodash-es';` (URL đầy đủ)
  - "Bare" specifiers (ví dụ `import _ from 'lodash';`) không hoạt động trực tiếp trên trình duyệt. Chúng thường được xử lý bởi các bundler (Webpack, Rollup, Parcel) hoặc các cơ chế như Import Maps.
- **Module chỉ được thực thi một lần:** Ngay cả khi được import nhiều lần ở các nơi khác nhau, code của một module chỉ chạy một lần duy nhất. Các lần import sau sẽ sử dụng lại instance đã được tạo.

**7. Dynamic Imports (`import()`):**

Đôi khi bạn không muốn tải tất cả các module ngay từ đầu, ví dụ để tối ưu thời gian tải ban đầu (code splitting) hoặc chỉ tải module khi thực sự cần thiết (lazy loading). Dynamic import cho phép làm điều này:

`import('path/to/module.js')` trả về một **Promise**. Promise này resolve thành một đối tượng module (giống như `* as ModuleName` khi dùng static import).

```javascript
// main.js
const loadButton = document.getElementById("loadModuleBtn");

if (loadButton) {
  loadButton.addEventListener("click", async () => {
    try {
      // Dynamic import trả về một Promise
      const utilsModule = await import("./utils.js"); // Trả về namespace object
      // utilsModule sẽ giống như { PI: ..., add: function, User: class }

      console.log("PI from dynamic import:", utilsModule.PI);
      const sum = utilsModule.add(100, 200);
      console.log("Dynamic sum:", sum);

      const dynamicUser = new utilsModule.User("Dynamic User");
      console.log(dynamicUser.greet());

      // Nếu module có default export:
      // const loggerModule = await import('./logger.js');
      // const log = loggerModule.default; // Truy cập default export
      // log('Message from dynamically loaded logger.');
    } catch (error) {
      console.error("Failed to load the module:", error);
    }
  });
}

// HTML: <button id="loadModuleBtn">Load Utility Module</button>
```

**Ứng dụng của Dynamic Imports:**

- **Code Splitting:** Chia nhỏ bundle JavaScript thành các chunk nhỏ hơn, chỉ tải khi cần, cải thiện Initial Load Time.
- **Lazy Loading:** Tải các component hoặc tính năng chỉ khi người dùng tương tác hoặc điều hướng đến phần đó của ứng dụng.
- **Conditional Loading:** Tải module dựa trên một điều kiện nào đó (ví dụ: dựa trên loại thiết bị, quyền người dùng).
- **Giảm kích thước ban đầu của ứng dụng.**

**8. `export ... from ...` (Re-exporting):**

Bạn có thể import một thứ gì đó từ một module và export nó ngay lập tức từ module hiện tại. Điều này hữu ích để tạo các module "trung gian" hoặc "barrel" files (gom nhiều export từ các module khác vào một điểm).

```javascript
// formatter.js
export function formatDate(date) {
  /* ... */
}
export function formatCurrency(amount) {
  /* ... */
}

// validator.js
export function validateEmail(email) {
  /* ... */
}
export function validatePassword(password) {
  /* ... */
}

// index.js (barrel file - điểm truy cập chính cho thư mục services chẳng hạn)
export { formatDate, formatCurrency } from "./formatter.js";
export { validateEmail } from "./validator.js"; // Chỉ re-export validateEmail
export * as ValidatorUtils from "./validator.js"; // Re-export tất cả từ validator.js vào một namespace
export { default as CoolLogger } from "./logger.js"; // Re-export default export với tên mới

// mainApp.js
import {
  formatDate,
  validateEmail,
  ValidatorUtils,
  CoolLogger,
} from "./index.js";

console.log(formatDate(new Date()));
console.log(validateEmail("test@example.com"));
console.log(ValidatorUtils.validatePassword("pass"));
CoolLogger("Barrel export works!");
```

**9. Import Maps (Tính năng tương đối mới, cần kiểm tra hỗ trợ trình duyệt hoặc polyfill):**

Import Maps cho phép bạn kiểm soát cách trình duyệt phân giải "bare" module specifiers (ví dụ: `import 'lodash'`) mà không cần dùng bundler. Bạn định nghĩa một bản đồ trong HTML:

```html
<script type="importmap">
  {
    "imports": {
      "moment": "/node_modules/moment/src/moment.js",
      "lodash/": "https://unpkg.com/lodash-es/", // Hậu tố / cho phép import các file con
      "my-app/": "./src/", // Ánh xạ thư mục cục bộ
      "utils": "./src/utils/common-utils.js" // Ánh xạ một file cụ thể
    }
  }
</script>

<script type="module">
  import moment from "moment";
  import { throttle } from "lodash/throttle.js"; // Sẽ thành https://unpkg.com/lodash-es/throttle.js
  import { helper } from "my-app/helpers/ui-helper.js"; // Sẽ thành ./src/helpers/ui-helper.js
  import { PI } from "utils"; // Sẽ thành ./src/utils/common-utils.js

  console.log(moment().format());
  console.log(typeof throttle);
</script>
```

Import Maps giúp việc quản lý đường dẫn module gọn gàng hơn và có thể giúp chuyển đổi giữa môi trường development (dùng local paths) và production (dùng CDN paths) dễ dàng hơn.

**10. Best Practices cho Tổ Chức Code với Modules:**

- **Single Responsibility Principle (Nguyên tắc Đơn trách nhiệm):** Mỗi module nên tập trung vào một nhiệm vụ hoặc một tập hợp các chức năng liên quan chặt chẽ.
- **High Cohesion, Low Coupling (Tính gắn kết cao, Khớp nối thấp):**
  - **High Cohesion:** Các thành phần bên trong một module nên liên quan mật thiết với nhau.
  - **Low Coupling:** Các module nên ít phụ thuộc vào chi tiết triển khai của nhau. Giao tiếp qua các API (exports) rõ ràng.
- **Sử dụng Default Export cho "Thứ Chính":** Nếu một module chủ yếu cung cấp một class, một hàm, hoặc một đối tượng chính, hãy dùng default export cho nó.
- **Sử dụng Named Exports cho các Tiện Ích Phụ:** Dùng named exports cho các hàm tiện ích, hằng số, hoặc các class phụ trợ.
- **Tạo "Barrel" Files (`index.js`) cho các Thư Mục Lớn:** Để đơn giản hóa việc import từ một thư mục có nhiều module, tạo một file `index.js` trong thư mục đó để re-export các thành phần quan trọng.

  ```
  // my-feature/
  //   |- componentA.js
  //   |- componentB.js
  //   |- utils.js
  //   |- index.js  <-- re-exports từ componentA, componentB, utils

  // app.js
  // import { ComponentA, someUtil } from './my-feature'; // Thay vì nhiều import lẻ
  ```

- **Tránh Circular Dependencies (Phụ thuộc vòng):** Khi module A import module B, và module B lại import module A. Điều này có thể gây lỗi hoặc hành vi không mong muốn. Cố gắng cấu trúc code để tránh điều này.
- **Đường dẫn rõ ràng:** Sử dụng đường dẫn tương đối (`./`, `../`) cho các module cục bộ và đường dẫn đầy đủ hoặc đã được map (qua Import Maps hoặc bundler) cho các thư viện bên ngoài.
- **Tận dụng Dynamic Imports cho Code Splitting:** Cải thiện hiệu suất tải trang.
- **Giữ exports tối thiểu:** Chỉ export những gì thực sự cần thiết để các module khác sử dụng. Chi tiết triển khai nội bộ nên được giữ private trong module.

ES Modules là một phần không thể thiếu của phát triển JavaScript hiện đại. Nắm vững chúng giúp bạn viết code có tổ chức, dễ bảo trì, và sẵn sàng cho các dự án lớn, kể cả việc xây dựng framework của riêng mình.

**Phần 7: Giao Tiếp Mạng (Network Communication)**

Các ứng dụng web hiện đại thường xuyên cần tương tác với server để lấy dữ liệu, gửi thông tin, hoặc duy trì kết nối thời gian thực. JavaScript cung cấp nhiều API mạnh mẽ cho các tác vụ này.

**1. Tại Sao Cần Giao Tiếp Mạng Bất Đồng Bộ?**

Hầu hết các thao tác mạng (như gửi yêu cầu HTTP đến server) đều tốn thời gian. Nếu JavaScript đợi cho đến khi yêu cầu hoàn tất mới tiếp tục thực thi các tác vụ khác, giao diện người dùng (UI) của trang web sẽ bị "treo" (freeze), mang lại trải nghiệm tồi tệ cho người dùng. Do đó, các API giao tiếp mạng trong JavaScript được thiết kế để hoạt động _bất đồng bộ_ (asynchronous). Điều này có nghĩa là:

- Bạn gửi một yêu cầu mạng.
- JavaScript không đợi phản hồi mà tiếp tục thực thi các dòng code tiếp theo.
- Khi phản hồi từ server về, một hàm callback (hoặc một Promise) sẽ được thực thi để xử lý dữ liệu hoặc lỗi.

Chúng ta sẽ chủ yếu sử dụng Promises với Fetch API, là cách tiếp cận hiện đại và dễ quản lý hơn so với callbacks truyền thống.

**2. Fetch API: Gửi Yêu Cầu HTTP Hiện Đại**

Fetch API là một giao diện JavaScript hiện đại, mạnh mẽ và linh hoạt để thực hiện các yêu cầu HTTP. Nó dựa trên Promises, giúp viết code bất đồng bộ dễ đọc và quản lý hơn so với `XMLHttpRequest` (AJAX cũ).

- **Cú Pháp Cơ Bản (Yêu Cầu GET):**

  ```javascript
  const apiUrl = "https://jsonplaceholder.typicode.com/todos/1"; // API công khai để test

  fetch(apiUrl)
    .then((response) => {
      // Bước 1: Kiểm tra xem yêu cầu có thành công không (status 200-299)
      if (!response.ok) {
        // Nếu không thành công (ví dụ: 404, 500), throw một error để nhảy vào .catch()
        throw new Error(
          `HTTP error! Status: ${response.status} - ${response.statusText}`
        );
      }
      // Bước 2: Parse nội dung phản hồi. response.json() cũng trả về một Promise.
      return response.json(); // Hoặc response.text(), response.blob(), response.arrayBuffer() ...
    })
    .then((data) => {
      // Bước 3: Xử lý dữ liệu đã được parse
      console.log("Data received:", data);
      // Ví dụ: Hiển thị data.title lên trang
      // document.body.innerHTML = `<h1>${data.title}</h1><p>Completed: ${data.completed}</p>`;
    })
    .catch((error) => {
      // Xử lý lỗi mạng (ví dụ: không kết nối được server) hoặc lỗi từ throw new Error ở trên
      console.error("Fetch error:", error.message);
      // document.body.innerHTML = `<p style="color:red;">Error fetching data: ${error.message}</p>`;
    });
  ```

  **Giải thích:**

  - `fetch(url)`: Gửi một yêu cầu GET đến `url` và trả về một Promise.
  - `.then(response => ...)`: Khi Promise từ `fetch` được resolve, nó cung cấp một đối tượng `Response`.
  - `response.ok`: Boolean, `true` nếu mã trạng thái HTTP là trong khoảng 200-299 (thành công).
  - `response.status`: Mã trạng thái HTTP (ví dụ: 200, 404, 500).
  - `response.statusText`: Thông điệp trạng thái HTTP (ví dụ: "OK", "Not Found").
  - `response.json()`: Đọc nội dung của response và parse nó thành đối tượng JavaScript từ chuỗi JSON. Phương thức này cũng trả về một Promise. Các phương thức khác:
    - `response.text()`: Trả về nội dung dưới dạng text.
    - `response.blob()`: Trả về nội dung dưới dạng `Blob` (dữ liệu nhị phân, ví dụ: ảnh).
    - `response.formData()`: Trả về nội dung dưới dạng `FormData`.
    - `response.arrayBuffer()`: Trả về nội dung dưới dạng `ArrayBuffer` (dữ liệu nhị phân cấp thấp).
  - `.catch(error => ...)`: Bắt lỗi mạng (ví dụ: DNS không tìm thấy, server không phản hồi) hoặc các lỗi được `throw` từ các block `.then()` trước đó.

- **Tùy Chọn Yêu Cầu (Request Options):**
  `fetch` chấp nhận một đối số thứ hai là một đối tượng options để tùy chỉnh yêu cầu:

  ```javascript
  const postApiUrl = "https://jsonplaceholder.typicode.com/posts";
  const newData = {
    title: "foo bar",
    body: "baz qux",
    userId: 1,
  };

  fetch(postApiUrl, {
    method: "POST", // Các phương thức: GET, POST, PUT, DELETE, PATCH, HEAD, OPTIONS,...
    headers: {
      "Content-Type": "application/json; charset=UTF-8", // Cho server biết định dạng dữ liệu gửi lên
      Authorization: "Bearer YOUR_ACCESS_TOKEN", // Ví dụ gửi token xác thực
      // ... các header khác
    },
    body: JSON.stringify(newData), // Dữ liệu gửi đi, phải là chuỗi, Blob, BufferSource, FormData,...
    // Với Content-Type là application/json, body thường là JSON.stringify(object)
  })
    .then((response) => {
      if (!response.ok) {
        throw new Error(`HTTP error! Status: ${response.status}`);
      }
      return response.json(); // Server thường trả về đối tượng vừa tạo/cập nhật
    })
    .then((createdPost) => {
      console.log("Post created:", createdPost);
    })
    .catch((error) => {
      console.error("Error creating post:", error);
    });
  ```

  **Các `options` phổ biến:**

  - `method`: Chuỗi HTTP method (mặc định là 'GET').
  - `headers`: Một đối tượng `Headers` hoặc một object literal chứa các header của yêu cầu.
  - `body`: Nội dung của yêu cầu. Không dùng cho 'GET' hoặc 'HEAD'.
  - `mode`: Kiểm soát CORS. Các giá trị:
    - `'cors'` (mặc định): Cho phép yêu cầu cross-origin, tuân theo quy tắc CORS.
    - `'no-cors'`: Cho phép yêu cầu cross-origin nhưng bạn sẽ nhận được một "opaque" response, không thể truy cập nội dung hoặc status. Hữu ích để gửi request mà không cần đọc phản hồi (ví dụ: logging, gửi beacon).
    - `'same-origin'`: Chỉ cho phép yêu cầu đến cùng một origin. Nếu khác origin, sẽ báo lỗi.
  - `credentials`: `'include'` (gửi cookies, authorization headers), `'same-origin'` (mặc định - gửi cho same-origin, không gửi cho cross-origin), `'omit'` (không gửi).
  - `cache`: Kiểm soát cách request tương tác với HTTP cache: `'default'`, `'no-store'`, `'reload'`, `'no-cache'`, `'force-cache'`, `'only-if-cached'`.
  - `signal`: Một `AbortSignal` để hủy yêu cầu (xem bên dưới).

- **Đối Tượng `Headers`:**
  Bạn có thể tạo và quản lý headers một cách linh hoạt:

  ```javascript
  const myHeaders = new Headers();
  myHeaders.append("Content-Type", "application/json");
  myHeaders.append("X-Custom-Header", "MyValue");
  // myHeaders.set('Content-Type', 'text/plain'); // Ghi đè
  // myHeaders.has('Content-Type'); // true
  // myHeaders.get('Content-Type'); // 'application/json'
  // myHeaders.delete('X-Custom-Header');

  // fetch(url, { headers: myHeaders });
  ```

- **Đối Tượng `Request`:**
  Bạn có thể tạo một đối tượng `Request` trước rồi mới truyền vào `fetch`:

  ```javascript
  const req = new Request(apiUrl, {
    method: "GET",
    headers: { Accept: "application/json" },
    // ... các options khác
  });

  // fetch(req)
  //   .then(...)
  //   .catch(...);
  ```

  Điều này hữu ích nếu bạn muốn tái sử dụng hoặc sửa đổi đối tượng request.

- **Gửi `FormData` (ví dụ: upload file hoặc gửi dữ liệu form):**

  ```html
  <form id="myForm">
    <input type="text" name="username" value="JohnDoe" /><br />
    <input type="file" name="avatar" /><br />
    <button type="submit">Submit Form</button>
  </form>
  <script>
    document
      .getElementById("myForm")
      .addEventListener("submit", function (event) {
        event.preventDefault(); // Ngăn form submit theo cách truyền thống

        const formData = new FormData(this); // Lấy dữ liệu từ form
        // Hoặc tạo FormData thủ công:
        // const manualFormData = new FormData();
        // manualFormData.append('customField', 'customValue');
        // manualFormData.append('userFile', fileInput.files[0]); // fileInput là <input type="file">

        fetch("/submit-form-data", {
          method: "POST",
          body: formData, // Không cần set Content-Type, trình duyệt sẽ tự động làm đúng (multipart/form-data)
        })
          .then((response) => response.json())
          .then((data) => console.log("Form submitted:", data))
          .catch((error) => console.error("Form submission error:", error));
      });
  </script>
  ```

- **Hủy Yêu Cầu Fetch (Aborting Fetch):**
  Sử dụng `AbortController` và `AbortSignal`.

  ```javascript
  const controller = new AbortController();
  const signal = controller.signal;

  // Nút để hủy
  // <button id="abortBtn">Abort Fetch</button>
  // document.getElementById('abortBtn').addEventListener('click', () => controller.abort());

  const timeoutId = setTimeout(() => {
    console.log("Aborting fetch due to timeout...");
    controller.abort();
  }, 5000); // Hủy sau 5 giây

  fetch("https://jsonplaceholder.typicode.com/users", { signal }) // Truyền signal vào options
    .then((response) => {
      clearTimeout(timeoutId); // Xóa timeout nếu fetch thành công trước đó
      if (!response.ok) throw new Error("Network response was not ok.");
      return response.json();
    })
    .then((data) => console.log("Users:", data))
    .catch((error) => {
      clearTimeout(timeoutId);
      if (error.name === "AbortError") {
        console.log("Fetch aborted successfully!");
      } else {
        console.error("Fetch error:", error);
      }
    });
  ```

- **Sử dụng `async/await` với Fetch (cách viết gọn gàng hơn):**

  ```javascript
  async function fetchData(url) {
    try {
      const response = await fetch(url);
      if (!response.ok) {
        throw new Error(`HTTP error! Status: ${response.status}`);
      }
      const data = await response.json();
      console.log("Data (async/await):", data);
      return data;
    } catch (error) {
      console.error("Fetch error (async/await):", error);
      // throw error; // Ném lại lỗi nếu cần xử lý ở nơi gọi
    }
  }
  // fetchData(apiUrl);

  async function postData(url, bodyData) {
    try {
      const response = await fetch(url, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(bodyData),
      });
      if (!response.ok) {
        throw new Error(
          `HTTP error! ${response.status} - ${response.statusText}`
        );
      }
      const result = await response.json();
      console.log("Post result (async/await):", result);
      return result;
    } catch (error) {
      console.error("Post error (async/await):", error);
    }
  }
  // postData(postApiUrl, { title: 'New Async Post', body: 'Content here', userId: 2 });
  ```

**3. `XMLHttpRequest` (XHR - "AJAX Cũ"):**

Trước Fetch API, `XMLHttpRequest` (XHR) là cách chính để thực hiện AJAX (Asynchronous JavaScript and XML). Nó dựa trên events thay vì Promises. Bạn vẫn có thể gặp nó trong code cũ.

```javascript
// Ví dụ GET đơn giản với XHR
const xhr = new XMLHttpRequest();
const xhrUrl = "https://jsonplaceholder.typicode.com/comments?postId=1";

xhr.open("GET", xhrUrl, true); // true nghĩa là bất đồng bộ

// Event listener cho khi request hoàn tất
xhr.onload = function () {
  if (xhr.status >= 200 && xhr.status < 300) {
    // Thành công
    try {
      const data = JSON.parse(xhr.responseText); // xhr.responseText chứa dữ liệu dạng text
      console.log("XHR Data:", data);
    } catch (e) {
      console.error("XHR JSON Parse Error:", e);
    }
  } else {
    // Lỗi HTTP
    console.error("XHR Request failed:", xhr.status, xhr.statusText);
  }
};

// Event listener cho lỗi mạng
xhr.onerror = function () {
  console.error("XHR Network error.");
};

// Gửi request
xhr.send();
```

Fetch API được ưa chuộng hơn vì cú pháp Promise trực quan, API xử lý Response và Request mạnh mẽ hơn, và tích hợp tốt hơn với các tính năng JavaScript hiện đại.

**4. WebSockets: Giao Tiếp Hai Chiều Thời Gian Thực**

WebSockets cung cấp một kênh giao tiếp hai chiều, full-duplex, liên tục giữa client và server qua một kết nối TCP duy nhất. Nó lý tưởng cho các ứng dụng cần cập nhật dữ liệu thời gian thực.

- **Đặc điểm:**
  - **Kết nối liên tục:** Sau khi "bắt tay" (handshake) ban đầu qua HTTP(S), kết nối được nâng cấp lên WebSocket và giữ mở.
  - **Hai chiều:** Cả client và server đều có thể gửi dữ liệu bất cứ lúc nào.
  - **Overhead thấp:** Sau handshake, dữ liệu được gửi với overhead rất nhỏ so với HTTP request/response liên tục.
- **Trường hợp sử dụng:** Ứng dụng chat, game online nhiều người chơi, thông báo real-time, dashboards cập nhật trực tiếp, công cụ cộng tác.

- **Cách sử dụng phía Client:**

  ```javascript
  // Địa chỉ WebSocket server (ws:// cho không mã hóa, wss:// cho mã hóa - tương tự http/https)
  const socketUrl = "wss://echo.websocket.org"; // Server echo công khai để test
  let webSocket;

  function connectWebSocket() {
    webSocket = new WebSocket(socketUrl);

    // 1. Khi kết nối được mở thành công
    webSocket.onopen = function (event) {
      console.log("WebSocket connection opened:", event);
      setStatus("Connected to WebSocket server.");
      // Gửi tin nhắn ngay khi kết nối
      webSocket.send("Hello WebSocket Server from Client!");
    };

    // 2. Khi nhận được tin nhắn từ server
    webSocket.onmessage = function (event) {
      console.log("Message from server:", event.data);
      addMessageToLog(`Server: ${event.data}`);
    };

    // 3. Khi có lỗi xảy ra
    webSocket.onerror = function (event) {
      console.error("WebSocket error:", event);
      setStatus("WebSocket Error. See console.");
    };

    // 4. Khi kết nối bị đóng
    webSocket.onclose = function (event) {
      console.log(
        "WebSocket connection closed:",
        event.code,
        event.reason,
        event.wasClean
      );
      setStatus(
        `WebSocket closed. Code: ${event.code}, Clean: ${event.wasClean}`
      );
      webSocket = null; // Dọn dẹp
    };
  }

  function sendMessage(message) {
    if (webSocket && webSocket.readyState === WebSocket.OPEN) {
      webSocket.send(message);
      addMessageToLog(`Client: ${message}`);
    } else {
      console.warn(
        "WebSocket is not open. ReadyState:",
        webSocket ? webSocket.readyState : "null"
      );
      setStatus("WebSocket is not connected. Cannot send message.");
    }
  }

  function closeWebSocket() {
    if (webSocket) {
      webSocket.close(1000, "Client initiated close"); // Mã 1000 là Normal Closure
    }
  }

  // --- Helper UI functions (giả định có các element trong HTML) ---
  // <div id="status">Not Connected</div>
  // <input type="text" id="messageInput" placeholder="Enter message">
  // <button onclick="sendMessage(document.getElementById('messageInput').value)">Send</button>
  // <button onclick="connectWebSocket()">Connect</button>
  // <button onclick="closeWebSocket()">Disconnect</button>
  // <pre id="log"></pre>
  function setStatus(text) {
    // document.getElementById('status').textContent = text;
  }
  function addMessageToLog(message) {
    // const logEl = document.getElementById('log');
    // logEl.textContent += message + '\n';
  }

  // Tự động kết nối khi tải trang (tùy chọn)
  // connectWebSocket();
  ```

  **Các thuộc tính và phương thức quan trọng của `WebSocket` object:**

  - `readyState`: Trạng thái kết nối (0: CONNECTING, 1: OPEN, 2: CLOSING, 3: CLOSED).
  - `onopen`, `onmessage`, `onerror`, `onclose`: Các trình xử lý sự kiện.
  - `send(data)`: Gửi dữ liệu (chuỗi, `Blob`, `ArrayBuffer`, `ArrayBufferView`). Thường là chuỗi JSON.
  - `close(code, reason)`: Đóng kết nối. `code` và `reason` là tùy chọn.

**5. Server-Sent Events (SSE): Nhận Dữ Liệu Theo Luồng Từ Server**

Server-Sent Events là một công nghệ cho phép server đẩy dữ liệu đến client một cách tự động qua một kết nối HTTP duy nhất, kéo dài. Nó là một chiều (server -> client).

- **Đặc điểm:**
  - **Một chiều:** Dữ liệu chỉ chảy từ server đến client. Client không thể gửi dữ liệu qua kết nối SSE (trừ request ban đầu).
  - **Dựa trên HTTP:** Sử dụng giao thức HTTP thông thường.
  - **Tự động kết nối lại:** Nếu kết nối bị mất, client sẽ tự động cố gắng kết nối lại.
  - **Định dạng text đơn giản:** Dữ liệu được gửi dưới dạng text, thường có định dạng cụ thể.
- **Trường hợp sử dụng:** Cập nhật tin tức trực tiếp, thông báo, biểu đồ/dữ liệu thay đổi theo thời gian thực (khi client chỉ cần nhận).

- **Cách sử dụng phía Client:**

  ```javascript
  // URL của endpoint SSE trên server
  const sseUrl = "/sse-events"; // Endpoint này phải được server triển khai để gửi SSE
  // Ví dụ: một server Node.js/Express có thể dùng thư viện như `express-sse`

  let eventSource;

  function startListeningSSE() {
    if (typeof EventSource === "undefined") {
      console.error("Sorry, your browser does not support Server-Sent Events.");
      // setSseStatus("SSE not supported by your browser.");
      return;
    }

    // eventSource = new EventSource(sseUrl); // Server phải triển khai endpoint này
    // Vì không có server thực tế, ta sẽ mock một chút
    console.warn(
      "SSE example is illustrative; requires a server-side SSE endpoint."
    );
    // setSseStatus("Attempting to connect to SSE... (requires server)");

    // Giả sử có một server, đây là cách bạn làm:
    /*
    eventSource = new EventSource(sseUrl);
  
    // 1. Khi kết nối được mở
    eventSource.onopen = function(event) {
      console.log("SSE Connection opened.", event);
      // setSseStatus("SSE Connected. Waiting for events...");
    };
  
    // 2. Khi nhận được một message chung (không có event name cụ thể)
    eventSource.onmessage = function(event) {
      console.log("SSE Generic Message:", event.data);
      // addSseMessage(`Generic: ${event.data}`);
      // event.lastEventId: ID của event cuối cùng (nếu server gửi)
    };
  
    // 3. Lắng nghe một event có tên cụ thể (server phải gửi event với 'event: customEventName')
    eventSource.addEventListener('userUpdate', function(event) {
      console.log("SSE Custom Event 'userUpdate':", event.data);
      // const userData = JSON.parse(event.data);
      // addSseMessage(`User Update: Name - ${userData.name}, Status - ${userData.status}`);
    });
  
    eventSource.addEventListener('serverTime', function(event) {
      console.log("SSE Server Time:", event.data);
      // addSseMessage(`Server Time: ${event.data}`);
    });
  
  
    // 4. Khi có lỗi xảy ra
    eventSource.onerror = function(event) {
      console.error("SSE Error:", event);
      if (eventSource.readyState === EventSource.CLOSED) {
        // setSseStatus("SSE Connection closed by server or error.");
      } else if (eventSource.readyState === EventSource.CONNECTING) {
        // setSseStatus("SSE Reconnecting...");
      } else {
        // setSseStatus("SSE Error. See console.");
      }
    };
    */
  }

  function stopListeningSSE() {
    if (eventSource) {
      eventSource.close(); // Đóng kết nối SSE
      console.log("SSE Connection closed by client.");
      // setSseStatus("SSE Disconnected by client.");
      eventSource = null;
    }
  }

  // --- Helper UI functions (giả định) ---
  // <div id="sseStatus">SSE Not Connected</div>
  // <button onclick="startListeningSSE()">Start SSE</button>
  // <button onclick="stopListeningSSE()">Stop SSE</button>
  // <pre id="sseLog"></pre>
  function setSseStatus(text) {
    /* ... */
  }
  function addSseMessage(message) {
    /* ... */
  }

  // startListeningSSE();
  ```

  **Định dạng dữ liệu từ Server (ví dụ):**
  Server cần gửi dữ liệu text với định dạng sau:

  ```
  id: event-id-1
  event: userUpdate
  data: {"name": "Alice", "status": "online"}
  retry: 5000

  data: Đây là một message chung không có event name.

  event: serverTime
  data: 2024-07-27T10:30:00Z

  ```

  - `id`: ID của event, client sẽ gửi lại header `Last-Event-ID` khi kết nối lại.
  - `event`: Tên của event (tùy chọn). Nếu có, client có thể dùng `eventSource.addEventListener('eventName', ...)`
  - `data`: Nội dung của message. Có thể có nhiều dòng `data:`.
  - `retry`: Thời gian (ms) client nên đợi trước khi thử kết nối lại nếu kết nối bị mất (tùy chọn).
  - Mỗi event kết thúc bằng một dòng trống (`\n\n`).

**Khi nào chọn Fetch, WebSocket, hay SSE?**

- **Fetch API (hoặc XHR):**
  - Cho các tương tác client-server truyền thống: lấy dữ liệu, gửi form, cập nhật tài nguyên.
  - Mô hình request/response.
  - Phù hợp khi client chủ động yêu cầu dữ liệu.
- **WebSockets:**
  - Cần giao tiếp hai chiều, thời gian thực, độ trễ thấp.
  - Client và server đều có thể chủ động gửi dữ liệu.
  - Phức tạp hơn một chút để thiết lập và quản lý so với SSE.
- **Server-Sent Events (SSE):**
  - Cần server đẩy dữ liệu một chiều đến client theo thời gian thực.
  - Đơn giản hơn WebSockets nếu chỉ cần server push.
  - Tận dụng HTTP và có cơ chế tự động kết nối lại.
  - Giới hạn số lượng kết nối đồng thời trên một số trình duyệt (vì mỗi SSE là một kết nối HTTP).

Nắm vững các kỹ thuật giao tiếp mạng này là nền tảng để xây dựng các ứng dụng web động, tương tác và phong phú tính năng.

**Phần 8: Lưu Trữ Dữ Liệu Phía Client (Client-Side Storage)**

Trình duyệt cung cấp nhiều cơ chế để lưu trữ dữ liệu trực tiếp trên máy của người dùng. Việc này có thể cải thiện hiệu suất (bằng cách cache dữ liệu), cho phép ứng dụng hoạt động offline (ở một mức độ nào đó), và lưu trữ các tùy chọn hoặc trạng thái của người dùng.

**1. `localStorage` và `sessionStorage` (Web Storage API):**

Đây là hai API đơn giản nhất để lưu trữ dữ liệu dạng key-value (chuỗi).

- **Đặc điểm chung:**

  - Lưu trữ dữ liệu dưới dạng cặp `(key, value)`, cả key và value đều là **chuỗi**. Nếu bạn muốn lưu đối tượng hoặc mảng, bạn cần `JSON.stringify()` trước khi lưu và `JSON.parse()` khi đọc.
  - Hoạt động đồng bộ (synchronous), nghĩa là các thao tác đọc/ghi sẽ chặn luồng chính. Với lượng dữ liệu nhỏ, điều này không đáng kể, nhưng với dữ liệu lớn có thể gây ảnh hưởng hiệu suất.
  - Dữ liệu được giới hạn theo origin (protocol + domain + port). Nghĩa là, một trang từ `http://example.com` không thể truy cập Web Storage của `https://example.com` hoặc `http://another.com`.
  - Giới hạn dung lượng lưu trữ: thường khoảng 5-10MB mỗi origin, tùy thuộc vào trình duyệt.

- **`localStorage`:**

  - Dữ liệu lưu trữ trong `localStorage` **tồn tại vĩnh viễn** cho đến khi bị xóa bởi người dùng (qua cài đặt trình duyệt) hoặc bởi mã JavaScript.
  - Dữ liệu không tự động hết hạn và vẫn còn đó ngay cả khi cửa sổ/tab trình duyệt được đóng và mở lại.
  - Chia sẻ giữa tất cả các tab và cửa sổ từ cùng một origin.
  - **Trường hợp sử dụng:** Lưu trữ tùy chọn người dùng (ví dụ: theme sáng/tối), token xác thực (cần cẩn trọng về bảo mật), dữ liệu ứng dụng nhỏ không thường xuyên thay đổi, cache dữ liệu API đơn giản.

  **API:**

  - `localStorage.setItem(key, value)`: Lưu một cặp key-value.
    ```javascript
    localStorage.setItem("username", "AliceWonder");
    const userPrefs = { theme: "dark", notifications: true };
    localStorage.setItem("preferences", JSON.stringify(userPrefs));
    ```
  - `localStorage.getItem(key)`: Lấy giá trị của một key. Trả về `null` nếu key không tồn tại.
    ```javascript
    const username = localStorage.getItem("username"); // "AliceWonder"
    const storedPrefsString = localStorage.getItem("preferences");
    if (storedPrefsString) {
      const userPreferences = JSON.parse(storedPrefsString);
      console.log("Theme:", userPreferences.theme); // dark
    }
    const nonExistent = localStorage.getItem("ghostKey"); // null
    ```
  - `localStorage.removeItem(key)`: Xóa một cặp key-value.
    ```javascript
    localStorage.removeItem("username");
    ```
  - `localStorage.clear()`: Xóa tất cả các cặp key-value trong `localStorage` cho origin hiện tại.
    ```javascript
    // localStorage.clear(); // Hãy cẩn thận khi dùng!
    ```
  - `localStorage.length`: Số lượng cặp key-value được lưu trữ.
  - `localStorage.key(index)`: Lấy key tại vị trí `index` (thứ tự không được đảm bảo và có thể thay đổi).
    ```javascript
    for (let i = 0; i < localStorage.length; i++) {
      const key = localStorage.key(i);
      const value = localStorage.getItem(key);
      console.log(`Key: ${key}, Value: ${value}`);
    }
    ```
  - Cũng có thể truy cập như thuộc tính đối tượng (không khuyến khích vì có thể xung đột với tên phương thức của `Storage` object):
    ```javascript
    // localStorage.myKey = 'myValue'; // Tương đương setItem
    // console.log(localStorage.myKey);  // Tương đương getItem
    // delete localStorage.myKey;      // Tương đương removeItem
    ```

- **`sessionStorage`:**

  - Dữ liệu lưu trữ trong `sessionStorage` chỉ tồn tại trong suốt **phiên duyệt hiện tại của tab hoặc cửa sổ đó**.
  - Khi tab/cửa sổ bị đóng, dữ liệu `sessionStorage` cho tab/cửa sổ đó sẽ bị xóa.
  - Mỗi tab/cửa sổ có `sessionStorage` riêng, ngay cả khi chúng cùng origin. Dữ liệu không được chia sẻ giữa các tab.
  - Reload trang không làm mất `sessionStorage`. Mở lại tab đã đóng (restore tab) trên một số trình duyệt có thể khôi phục `sessionStorage`.
  - **Trường hợp sử dụng:** Lưu trữ trạng thái tạm thời của một form nhiều bước, trạng thái UI tạm thời cho một tab cụ thể, dữ liệu người dùng cho một phiên làm việc duy nhất mà không cần lưu lại sau khi đóng tab.

  **API:** Giống hệt `localStorage`, chỉ cần thay `localStorage` bằng `sessionStorage`.

  ```javascript
  sessionStorage.setItem("currentStep", "2");
  const step = sessionStorage.getItem("currentStep"); // "2"
  // sessionStorage.removeItem('currentStep');
  // sessionStorage.clear();
  ```

- **Sự kiện `storage`:**
  Khi dữ liệu trong `localStorage` (không phải `sessionStorage`) thay đổi ở một tab, các tab khác từ cùng một origin sẽ nhận được sự kiện `storage` trên đối tượng `window`.

  ```javascript
  window.addEventListener("storage", function (event) {
    console.log("Storage event detected!");
    console.log("Key changed:", event.key); // Key đã thay đổi (null nếu clear() được gọi)
    console.log("Old value:", event.oldValue); // Giá trị cũ (null nếu item mới hoặc clear())
    console.log("New value:", event.newValue); // Giá trị mới (null nếu item bị xóa hoặc clear())
    console.log("URL of page that made change:", event.url);
    console.log("StorageArea object:", event.storageArea); // localStorage object

    if (event.key === "user-status") {
      // Cập nhật UI dựa trên trạng thái người dùng mới
      // const newStatus = event.newValue;
      // updateStatusDisplay(newStatus);
    }
  });

  // Trong một tab khác (hoặc tab hiện tại sau khi thay đổi)
  // localStorage.setItem('user-status', 'online');
  // localStorage.setItem('user-status', 'offline');
  // localStorage.removeItem('user-status');
  ```

  Sự kiện này không kích hoạt trên chính tab đã thực hiện thay đổi.

**2. IndexedDB: Database NoSQL Phía Client**

IndexedDB là một API cấp thấp để lưu trữ lượng lớn dữ liệu có cấu trúc phía client, bao gồm cả file/blob. Nó là một hệ thống database dựa trên transaction, sử dụng các index để cho phép truy vấn hiệu suất cao.

- **Đặc điểm:**

  - **Bất đồng bộ (Asynchronous):** Tất cả các thao tác IndexedDB đều là bất đồng bộ, sử dụng request objects và events, không chặn luồng chính. Thường được bọc trong Promises để dễ sử dụng.
  - **Transactional:** Các thao tác dữ liệu được thực hiện trong các transaction, đảm bảo tính toàn vẹn dữ liệu (ACID properties ở mức độ nào đó).
  - **Object Stores:** Dữ liệu được lưu trong các "object stores" (tương tự như tables trong SQL DB hoặc collections trong MongoDB). Mỗi object store lưu trữ các đối tượng JavaScript.
  - **Indexes:** Bạn có thể tạo các index trên các thuộc tính của đối tượng trong object store để tìm kiếm và sắp xếp dữ liệu nhanh chóng.
  - **Lưu trữ được nhiều loại dữ liệu:** Không chỉ chuỗi, mà cả object, array, Date, RegExp, Blob, File, ArrayBuffer.
  - **Dung lượng lớn:** Cho phép lưu trữ nhiều dữ liệu hơn Web Storage (thường là hàng trăm MB hoặc GB, tùy thuộc vào trình duyệt và dung lượng đĩa trống của người dùng). Trình duyệt có thể hỏi quyền người dùng nếu ứng dụng yêu cầu nhiều dung lượng.
  - **Same-origin policy:** Giống Web Storage.

- **Cấu trúc cơ bản:**

  1.  **Database:** Một tập hợp các object stores. Mỗi origin có thể có nhiều database, mỗi database có tên riêng.
  2.  **Object Store:** Nơi lưu trữ các record (đối tượng JavaScript). Mỗi record có một key duy nhất. Key có thể được tạo tự động (auto-incrementing) hoặc được chỉ định từ một thuộc tính của đối tượng (key path).
  3.  **Index:** Cho phép truy vấn hiệu quả dựa trên các thuộc tính khác ngoài key chính.
  4.  **Transaction:** Tất cả các thao tác đọc/ghi phải diễn ra trong một transaction. Transaction có thể là `readonly` (chỉ đọc) hoặc `readwrite` (đọc và ghi).
  5.  **Request:** Hầu hết các thao tác IndexedDB (mở database, thêm record, đọc record) trả về một đối tượng `IDBRequest`. Kết quả của thao tác được thông báo qua các event `success` hoặc `error` trên request object này.
  6.  **Cursor:** Dùng để lặp qua các record trong object store hoặc index.

- **Quy trình làm việc cơ bản:**

  1.  **Mở Database:** `indexedDB.open(databaseName, version)`
      - `version` là số nguyên. Nếu database chưa tồn জি বা version nhỏ hơn, sự kiện `upgradeneeded` sẽ được kích hoạt.
  2.  **Xử lý `upgradeneeded`:** Đây là nơi duy nhất bạn có thể thay đổi cấu trúc database (tạo/xóa object stores, tạo/xóa indexes).
  3.  **Xử lý `success` (khi mở DB):** Database đã sẵn sàng để sử dụng.
  4.  **Tạo Transaction:** `db.transaction(storeNames, mode)`
  5.  **Lấy Object Store:** `transaction.objectStore(storeName)`
  6.  **Thực hiện thao tác (add, get, put, delete, openCursor):**
      - `objectStore.add(dataObject, [optionalKey])`
      - `objectStore.put(dataObject, [optionalKey])` (thêm mới hoặc cập nhật nếu key đã tồn tại)
      - `objectStore.get(key)`
      - `objectStore.delete(key)`
      - `objectStore.openCursor()` hoặc `index.openCursor()`
  7.  **Xử lý `success` / `error` trên request của thao tác.**
  8.  **Xử lý `complete` / `abort` / `error` trên transaction.**

- **Ví dụ đơn giản (sử dụng Promises để code dễ đọc hơn):**

  ```javascript
  const DB_NAME = "myAppDB";
  const DB_VERSION = 1;
  const STORE_NAME = "items";
  let db; // Biến lưu trữ đối tượng database

  // Hàm bọc Promise cho việc mở DB
  function openDB() {
    return new Promise((resolve, reject) => {
      const request = indexedDB.open(DB_NAME, DB_VERSION);

      request.onupgradeneeded = function (event) {
        console.log("Database upgrade needed.");
        const dbInstance = event.target.result;
        if (!dbInstance.objectStoreNames.contains(STORE_NAME)) {
          // Tạo object store nếu chưa có
          // 'id' là key path, autoIncrement để tự động tạo key
          const objectStore = dbInstance.createObjectStore(STORE_NAME, {
            keyPath: "id",
            autoIncrement: true,
          });
          // Tạo index trên thuộc tính 'name'
          objectStore.createIndex("nameIndex", "name", { unique: false });
          console.log(
            `Object store "${STORE_NAME}" created with index "nameIndex".`
          );
        }
      };

      request.onsuccess = function (event) {
        console.log("Database opened successfully.");
        db = event.target.result; // Lưu lại instance DB
        resolve(db);
      };

      request.onerror = function (event) {
        console.error("Error opening database:", event.target.error);
        reject(event.target.error);
      };
    });
  }

  // Hàm thêm item (trả về Promise)
  function addItem(itemData) {
    return new Promise((resolve, reject) => {
      if (!db) {
        reject("Database not open. Call openDB() first.");
        return;
      }
      // Bắt đầu transaction ở mode 'readwrite'
      const transaction = db.transaction([STORE_NAME], "readwrite");
      const objectStore = transaction.objectStore(STORE_NAME);

      // Thêm dữ liệu. itemData nên là object, ví dụ { name: '...', description: '...' }
      // 'id' sẽ tự động được gán nếu keyPath là autoIncrement
      const request = objectStore.add(itemData);

      request.onsuccess = function (event) {
        console.log("Item added successfully, ID:", event.target.result);
        resolve(event.target.result); // event.target.result là key của item vừa thêm
      };
      request.onerror = function (event) {
        console.error("Error adding item:", event.target.error);
        reject(event.target.error);
      };
      transaction.oncomplete = function () {
        console.log("Add item transaction completed.");
      };
      transaction.onerror = function (event) {
        console.error("Add item transaction error:", event.target.error);
        // reject(event.target.error); // Lỗi transaction cũng nên reject promise
      };
    });
  }

  // Hàm lấy item theo ID (trả về Promise)
  function getItem(itemId) {
    return new Promise((resolve, reject) => {
      if (!db) {
        reject("Database not open.");
        return;
      }
      const transaction = db.transaction([STORE_NAME], "readonly");
      const objectStore = transaction.objectStore(STORE_NAME);
      const request = objectStore.get(itemId); // itemId phải là kiểu của keyPath

      request.onsuccess = function (event) {
        console.log("Item retrieved:", event.target.result);
        resolve(event.target.result); // event.target.result là object item, hoặc undefined
      };
      request.onerror = function (event) {
        console.error("Error getting item:", event.target.error);
        reject(event.target.error);
      };
    });
  }

  // Hàm lấy tất cả items (trả về Promise chứa mảng items)
  function getAllItems() {
    return new Promise((resolve, reject) => {
      if (!db) {
        reject("Database not open.");
        return;
      }
      const transaction = db.transaction([STORE_NAME], "readonly");
      const objectStore = transaction.objectStore(STORE_NAME);
      const request = objectStore.getAll(); // API đơn giản để lấy tất cả

      request.onsuccess = function (event) {
        console.log("All items retrieved:", event.target.result);
        resolve(event.target.result); // Mảng các objects
      };
      request.onerror = function (event) {
        console.error("Error getting all items:", event.target.error);
        reject(event.target.error);
      };
    });
  }

  // Hàm cập nhật item (trả về Promise)
  function updateItem(itemDataWithId) {
    // itemDataWithId phải có thuộc tính keyPath (ví dụ: 'id')
    return new Promise((resolve, reject) => {
      if (!db) {
        reject("Database not open.");
        return;
      }
      const transaction = db.transaction([STORE_NAME], "readwrite");
      const objectStore = transaction.objectStore(STORE_NAME);
      // 'put' sẽ cập nhật nếu key tồn tại, hoặc thêm mới nếu không
      const request = objectStore.put(itemDataWithId);

      request.onsuccess = function (event) {
        console.log("Item updated/added, ID:", event.target.result);
        resolve(event.target.result);
      };
      request.onerror = function (event) {
        console.error("Error updating item:", event.target.error);
        reject(event.target.error);
      };
    });
  }

  // Hàm xóa item theo ID (trả về Promise)
  function deleteItem(itemId) {
    return new Promise((resolve, reject) => {
      if (!db) {
        reject("Database not open.");
        return;
      }
      const transaction = db.transaction([STORE_NAME], "readwrite");
      const objectStore = transaction.objectStore(STORE_NAME);
      const request = objectStore.delete(itemId);

      request.onsuccess = function (event) {
        console.log("Item deleted successfully, ID:", itemId);
        resolve(); // Không có giá trị trả về cụ thể khi xóa thành công
      };
      request.onerror = function (event) {
        console.error("Error deleting item:", event.target.error);
        reject(event.target.error);
      };
    });
  }

  // Hàm sử dụng cursor để lặp qua items dựa trên index
  function getItemsByName(nameValue) {
    return new Promise((resolve, reject) => {
      if (!db) {
        reject("Database not open.");
        return;
      }
      const transaction = db.transaction([STORE_NAME], "readonly");
      const objectStore = transaction.objectStore(STORE_NAME);
      const index = objectStore.index("nameIndex"); // Lấy index đã tạo
      const items = [];

      // Tạo một key range để chỉ tìm các item có 'name' bằng nameValue
      const keyRange = IDBKeyRange.only(nameValue);
      // Hoặc: IDBKeyRange.lowerBound(value), IDBKeyRange.upperBound(value), IDBKeyRange.bound(low, high)

      const cursorRequest = index.openCursor(keyRange); // Mở cursor trên index

      cursorRequest.onsuccess = function (event) {
        const cursor = event.target.result;
        if (cursor) {
          items.push(cursor.value); // cursor.value là object item
          // console.log('Cursor found:', cursor.key, cursor.primaryKey, cursor.value);
          cursor.continue(); // Di chuyển đến record tiếp theo khớp với range
        } else {
          // Không còn item nào nữa
          console.log(`Items with name "${nameValue}":`, items);
          resolve(items);
        }
      };
      cursorRequest.onerror = function (event) {
        console.error("Error using cursor:", event.target.error);
        reject(event.target.error);
      };
    });
  }

  // Sử dụng các hàm:
  async function main() {
    try {
      await openDB(); // Mở hoặc tạo DB

      // Thêm items
      const newItem1Id = await addItem({
        name: "Laptop Pro",
        description: "High-end laptop",
        price: 2500,
      });
      const newItem2Id = await addItem({
        name: "Wireless Mouse",
        description: "Ergonomic mouse",
        price: 50,
      });
      await addItem({
        name: "Keyboard Mechanical",
        description: "RGB Keyboard",
        price: 120,
      });
      await addItem({
        name: "Laptop Pro",
        description: "Another Pro Laptop",
        price: 2600,
      }); // Test index

      // Lấy một item
      const retrievedItem = await getItem(newItem1Id);
      if (retrievedItem) {
        console.log("Retrieved specifically:", retrievedItem.name);
      }

      // Cập nhật item
      if (retrievedItem) {
        await updateItem({
          ...retrievedItem,
          price: 2450,
          description: "Updated high-end laptop",
        });
        const updatedItem = await getItem(newItem1Id);
        console.log("After update:", updatedItem);
      }

      // Lấy tất cả items
      const allItems = await getAllItems();
      console.log("All current items in store:", allItems);

      // Tìm items theo tên (sử dụng index và cursor)
      const laptops = await getItemsByName("Laptop Pro");
      console.log("Laptops found by name:", laptops.length);

      // Xóa một item
      // await deleteItem(newItem2Id);
      // const allItemsAfterDelete = await getAllItems();
      // console.log('All items after delete:', allItemsAfterDelete);
    } catch (error) {
      console.error("Main app error:", error);
    } finally {
      // Đóng database khi không cần nữa (quan trọng)
      if (db) {
        db.close();
        console.log("Database closed.");
      }
    }
  }

  // main();
  ```

  **Lưu ý về IndexedDB:**

  - API gốc khá dài dòng và dựa trên event. Việc sử dụng các thư viện wrapper (như `idb` của Jake Archibald) hoặc tự viết các hàm bọc Promise như trên có thể giúp code dễ quản lý hơn rất nhiều.
  - Luôn đóng database khi bạn không dùng nữa (`db.close()`) để giải phóng tài nguyên.
  - Xử lý lỗi cẩn thận ở các cấp độ: request, transaction, và mở database.

**3. Cookies:**

Cookies là các mẩu dữ liệu nhỏ (chuỗi) mà server gửi đến trình duyệt. Trình duyệt lưu trữ chúng và gửi lại chúng cùng với mỗi yêu cầu HTTP tiếp theo đến cùng một server.

- **Đặc điểm:**

  - **Gửi tự động với HTTP requests:** Đây là đặc điểm chính.
  - **Kích thước nhỏ:** Giới hạn khoảng 4KB mỗi cookie, và tổng số cookie cho một domain cũng có giới hạn.
  - **Có thể được đặt bởi server (qua header `Set-Cookie`) hoặc JavaScript client-side (qua `document.cookie`).**
  - **Có thể có thời gian hết hạn (`Expires`, `Max-Age`), domain, path, và các cờ bảo mật (`Secure`, `HttpOnly`, `SameSite`).**
  - **Same-origin policy áp dụng (với các tùy chọn `Domain` và `Path`).**

- **Truy cập Cookies bằng JavaScript (`document.cookie`):**
  API `document.cookie` khá kỳ lạ. Nó hoạt động như một accessor (getter/setter) cho tất cả cookies của trang hiện tại (khớp với domain và path).

  - **Đọc cookies:** `document.cookie` trả về một chuỗi duy nhất chứa tất cả cookies (khả dụng cho client) dưới dạng `name1=value1; name2=value2; ...`. Bạn phải tự parse chuỗi này.
  - **Ghi (tạo/cập nhật) cookie:** Gán một chuỗi cho `document.cookie` với định dạng `name=value; [expires=UTCString;] [max-age=seconds;] [path=pathString;] [domain=domainString;] [secure;] [samesite=Strict|Lax|None]`.
    - Ghi một cookie mới không xóa các cookie hiện có, nó chỉ thêm hoặc cập nhật cookie có cùng tên.
    - Để xóa cookie, bạn đặt ngày hết hạn của nó trong quá khứ.

  ```javascript
  // Hàm helper để set cookie
  function setCookie(
    name,
    value,
    days,
    path = "/",
    domain,
    secure,
    sameSite = "Lax"
  ) {
    let expires = "";
    if (days) {
      const date = new Date();
      date.setTime(date.getTime() + days * 24 * 60 * 60 * 1000);
      expires = "; expires=" + date.toUTCString();
    }
    let cookieString = name + "=" + (value || "") + expires + "; path=" + path;
    if (domain) cookieString += "; domain=" + domain;
    if (secure) cookieString += "; secure";
    if (sameSite) cookieString += "; samesite=" + sameSite; // None, Lax, Strict
    // Nếu SameSite=None thì Secure flag phải được set

    document.cookie = cookieString;
    console.log(`Cookie set: ${cookieString}`);
  }

  // Hàm helper để get cookie
  function getCookie(name) {
    const nameEQ = name + "=";
    const ca = document.cookie.split(";");
    for (let i = 0; i < ca.length; i++) {
      let c = ca[i];
      while (c.charAt(0) === " ") c = c.substring(1, c.length);
      if (c.indexOf(nameEQ) === 0) return c.substring(nameEQ.length, c.length);
    }
    return null;
  }

  // Hàm helper để xóa cookie
  function eraseCookie(name, path = "/", domain) {
    let cookieString = name + "=; Max-Age=-99999999; path=" + path;
    if (domain) cookieString += "; domain=" + domain;
    document.cookie = cookieString;
    console.log(`Cookie erased: ${name}`);
  }

  // Sử dụng:
  setCookie("user_id", "12345", 7); // Lưu 7 ngày
  setCookie("session_token", "abcdef", 0.5 / 24); // Lưu 30 phút (0.5 giờ)
  setCookie("theme", "dark", 365, "/", undefined, true, "Lax"); // Secure, SameSite=Lax

  console.log("All cookies:", document.cookie);
  const userId = getCookie("user_id");
  console.log("User ID from cookie:", userId);

  // eraseCookie('session_token');
  // console.log('After erasing session_token:', document.cookie);
  ```

- **Cờ `HttpOnly`:** Nếu một cookie được server đặt với cờ `HttpOnly`, nó **không thể** được truy cập hoặc sửa đổi bởi JavaScript client-side (`document.cookie` sẽ không thấy nó). Điều này giúp bảo vệ cookie khỏi các cuộc tấn công XSS (ví dụ: cookie chứa session ID quan trọng).
- **Cờ `Secure`:** Cookie chỉ được gửi qua kết nối HTTPS.
- **Thuộc tính `SameSite`:** Kiểm soát việc cookie có được gửi cùng với các yêu cầu cross-site hay không.

  - `Strict`: Cookie chỉ được gửi nếu request đến từ chính site đã đặt cookie.
  - `Lax` (thường là mặc định của trình duyệt): Cookie được gửi với navigation top-level (ví dụ: click vào link) từ site khác đến site của bạn, nhưng không gửi với các subrequest (như load ảnh, iframe) hoặc form POST từ site khác.
  - `None`: Cookie được gửi với tất cả các request cross-site. **Yêu cầu cờ `Secure` phải được đặt.** Dùng cho các kịch bản như theo dõi hoặc iframe cần cookie.

- **Khi nào dùng Cookies:**
  - Chủ yếu cho việc quản lý session và xác thực, đặc biệt khi server cần đọc thông tin này từ mỗi request.
  - Lưu trữ các tùy chọn nhỏ mà server cũng cần biết.
  - **Tránh lưu trữ dữ liệu nhạy cảm trong cookie có thể truy cập bởi JavaScript nếu không cần thiết.** Ưu tiên `HttpOnly` cho các cookie session.
  - Đối với lưu trữ dữ liệu lớn hoặc phức tạp phía client, Web Storage hoặc IndexedDB là lựa chọn tốt hơn.

**Chọn Cơ Chế Lưu Trữ Nào?**

- **`localStorage`:**
  - **Ưu điểm:** Đơn giản, API dễ dùng, dữ liệu tồn tại lâu dài.
  - **Nhược điểm:** Đồng bộ, giới hạn dung lượng 5-10MB, chỉ lưu chuỗi, có thể ảnh hưởng hiệu suất nếu lạm dụng.
  - **Dùng cho:** Tùy chọn người dùng, cache nhỏ, token (cẩn thận).
- **`sessionStorage`:**
  - **Ưu điểm:** Đơn giản, API dễ dùng, dữ liệu tự xóa khi đóng tab.
  - **Nhược điểm:** Giống `localStorage` về tính đồng bộ và chỉ lưu chuỗi. Dữ liệu không chia sẻ giữa các tab.
  - **Dùng cho:** Trạng thái tạm thời của một tab/phiên làm việc.
- **`IndexedDB`:**
  - **Ưu điểm:** Bất đồng bộ, dung lượng lớn, lưu trữ được nhiều loại dữ liệu, có index để truy vấn hiệu quả, transactional.
  - **Nhược điểm:** API phức tạp hơn nhiều.
  - **Dùng cho:** Ứng dụng offline, PWA, lưu trữ lượng lớn dữ liệu có cấu trúc, cache dữ liệu phức tạp.
- **`Cookies`:**
  - **Ưu điểm:** Tự động gửi với HTTP requests, được server và client kiểm soát.
  - **Nhược điểm:** Kích thước nhỏ, làm tăng overhead cho mỗi request, API `document.cookie` khó dùng, vấn đề bảo mật nếu không cẩn thận.
  - **Dùng cho:** Quản lý session, xác thực, các thông tin nhỏ mà server cần.

Việc hiểu rõ từng cơ chế lưu trữ sẽ giúp bạn chọn đúng công cụ cho từng nhu cầu cụ thể, tối ưu hóa trải nghiệm người dùng và hiệu suất ứng dụng.

**Phần 9: Tương Tác Với Phần Cứng & Cảm Biến (Hardware & Sensor Interaction)**

Trình duyệt hiện đại không còn bị giới hạn trong việc hiển thị nội dung tĩnh. Chúng ngày càng có khả năng tương tác sâu hơn với phần cứng và các cảm biến của thiết bị người dùng, mở ra nhiều ứng dụng web phong phú và hấp dẫn.

**Quan Trọng về Quyền Riêng Tư và Bảo Mật:**
Hầu hết các API này đều yêu cầu **sự cho phép rõ ràng từ người dùng** trước khi có thể truy cập phần cứng hoặc dữ liệu cảm biến. Trình duyệt sẽ hiển thị một hộp thoại yêu cầu quyền. Luôn luôn:

- **Yêu cầu quyền khi cần thiết:** Chỉ hỏi khi người dùng thực hiện một hành động cho thấy họ muốn sử dụng tính năng đó.
- **Giải thích rõ ràng:** Cho người dùng biết tại sao bạn cần quyền đó và dữ liệu sẽ được sử dụng như thế nào.
- **Xử lý trường hợp từ chối quyền:** Cung cấp trải nghiệm thay thế hoặc thông báo rõ ràng nếu tính năng không thể hoạt động do thiếu quyền.
- **Luôn sử dụng HTTPS:** Nhiều API trong số này chỉ hoạt động trên các trang được phục vụ qua HTTPS để đảm bảo an toàn.

**1. Geolocation API: Lấy Vị Trí Người Dùng**

Geolocation API cho phép ứng dụng web lấy thông tin vị trí địa lý (kinh độ, vĩ độ, độ chính xác) của thiết bị người dùng.

- **Kiểm tra hỗ trợ:**

  ```javascript
  if ("geolocation" in navigator) {
    // Geolocation API được hỗ trợ
    console.log("Geolocation is available.");
  } else {
    console.log("Geolocation is NOT available.");
    // Thông báo cho người dùng hoặc cung cấp phương án thay thế
  }
  ```

- **Lấy vị trí hiện tại một lần (`getCurrentPosition`):**

  ```javascript
  function getLocation() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(
        showPosition, // Hàm callback khi thành công
        showError, // Hàm callback khi có lỗi
        {
          // Đối tượng options (tùy chọn)
          enableHighAccuracy: true, // Cố gắng lấy vị trí chính xác hơn (có thể tốn pin hơn)
          timeout: 10000, // Thời gian tối đa (ms) chờ đợi phản hồi vị trí
          maximumAge: 0, // Thời gian tối đa (ms) của vị trí cache được chấp nhận (0 = luôn lấy mới)
        }
      );
    } else {
      alert("Geolocation is not supported by this browser.");
    }
  }

  function showPosition(position) {
    const latitude = position.coords.latitude;
    const longitude = position.coords.longitude;
    const accuracy = position.coords.accuracy; // Độ chính xác (mét)
    const altitude = position.coords.altitude; // Độ cao (mét) so với mực nước biển (nếu có)
    const altitudeAccuracy = position.coords.altitudeAccuracy; // Độ chính xác độ cao (nếu có)
    const heading = position.coords.heading; // Hướng di chuyển (độ so với hướng Bắc) (nếu có)
    const speed = position.coords.speed; // Tốc độ (m/s) (nếu có)
    const timestamp = new Date(position.timestamp); // Thời gian lấy vị trí

    console.log(`Latitude: ${latitude}°`);
    console.log(`Longitude: ${longitude}°`);
    console.log(`Accuracy: ${accuracy} meters`);
    console.log(`Timestamp: ${timestamp.toLocaleString()}`);
    if (altitude) console.log(`Altitude: ${altitude} m`);

    // Ví dụ: Hiển thị trên bản đồ
    // const mapUrl = `https://www.openstreetmap.org/?mlat=${latitude}&mlon=${longitude}#map=15/${latitude}/${longitude}`;
    // window.open(mapUrl, '_blank');
    // document.getElementById('location-info').innerHTML = `Lat: ${latitude}, Lon: ${longitude}`;
  }

  function showError(error) {
    let errorMessage = "An unknown error occurred.";
    switch (error.code) {
      case error.PERMISSION_DENIED:
        errorMessage = "User denied the request for Geolocation.";
        break;
      case error.POSITION_UNAVAILABLE:
        errorMessage = "Location information is unavailable.";
        break;
      case error.TIMEOUT:
        errorMessage = "The request to get user location timed out.";
        break;
      case error.UNKNOWN_ERROR: // Thực ra không có error.UNKNOWN_ERROR, nên default case sẽ bắt
        errorMessage = "An unknown error occurred.";
        break;
      default:
        errorMessage = `Error code ${error.code}: ${error.message}`;
    }
    console.error("Geolocation Error:", errorMessage, error);
    // document.getElementById('location-info').innerHTML = `<p style="color:red;">Error: ${errorMessage}</p>`;
  }

  // <button onclick="getLocation()">Get My Location</button>
  // <div id="location-info"></div>
  ```

- **Theo dõi vị trí thay đổi (`watchPosition`):**
  `watchPosition` hoạt động tương tự `getCurrentPosition` nhưng sẽ gọi lại hàm `successCallback` mỗi khi vị trí của thiết bị thay đổi đáng kể (hoặc theo tần suất nhất định).
  Nó trả về một `watchId` (số nguyên) để bạn có thể dừng theo dõi sau này.

  ```javascript
  let watchId = null;

  function startWatchingLocation() {
    if (navigator.geolocation) {
      if (watchId) {
        console.log("Already watching location.");
        return;
      }
      console.log("Starting to watch location...");
      watchId = navigator.geolocation.watchPosition(
        showPosition, // Dùng lại hàm showPosition ở trên
        showError, // Dùng lại hàm showError ở trên
        {
          enableHighAccuracy: true,
          // timeout: 5000, // Ít hữu ích cho watchPosition
          maximumAge: 0,
        }
      );
      // document.getElementById('watch-status').textContent = 'Watching location...';
    } else {
      alert("Geolocation is not supported.");
    }
  }

  function stopWatchingLocation() {
    if (watchId !== null && navigator.geolocation) {
      navigator.geolocation.clearWatch(watchId);
      watchId = null;
      console.log("Stopped watching location.");
      // document.getElementById('watch-status').textContent = 'Not watching.';
      // document.getElementById('location-info').innerHTML = '';
    }
  }

  // <button onclick="startWatchingLocation()">Start Watching</button>
  // <button onclick="stopWatchingLocation()">Stop Watching</button>
  // <div id="watch-status">Not watching.</div>
  ```

**2. Device Orientation và Device Motion API: Phát Hiện Chuyển Động và Hướng Thiết Bị**

Các API này cho phép bạn truy cập dữ liệu từ gia tốc kế (accelerometer), con quay hồi chuyển (gyroscope) và từ kế (magnetometer) của thiết bị.

- **`DeviceOrientationEvent` (`deviceorientation` event):**

  - Cung cấp thông tin về hướng vật lý của thiết bị.
  - Dữ liệu trả về bao gồm:
    - `event.alpha`: Hướng la bàn (compass heading) - xoay quanh trục Z (0-360 độ). 0 là hướng Bắc.
    - `event.beta`: Nghiêng trước/sau - xoay quanh trục X (-180 đến 180 độ).
    - `event.gamma`: Nghiêng trái/phải - xoay quanh trục Y (-90 đến 90 độ).
    - `event.absolute`: Boolean, cho biết dữ liệu có dựa trên từ kế (true) hay chỉ là tương đối (false).
  - **Lưu ý:** `deviceorientationabsolute` là một event tương tự nhưng đảm bảo `alpha` luôn là la bàn thực sự.

  ```javascript
  function handleOrientation(event) {
    const alpha = event.alpha ? event.alpha.toFixed(2) : "N/A";
    const beta = event.beta ? event.beta.toFixed(2) : "N/A";
    const gamma = event.gamma ? event.gamma.toFixed(2) : "N/A";
    const absolute = event.absolute;

    // console.log(`Alpha: ${alpha}, Beta: ${beta}, Gamma: ${gamma}, Absolute: ${absolute}`);
    // document.getElementById('orientation-data').innerHTML =
    //   `Alpha (Z): ${alpha}°<br>Beta (X): ${beta}°<br>Gamma (Y): ${gamma}°<br>Absolute: ${absolute}`;

    // Ví dụ: Xoay một hình ảnh
    // const img = document.getElementById('rotating-image');
    // if (img && beta !== 'N/A' && gamma !== 'N/A') {
    //   img.style.transform = `rotateX(${beta}deg) rotateY(${gamma}deg)`;
    // }
  }

  function startOrientationListener() {
    if (window.DeviceOrientationEvent) {
      // Yêu cầu quyền (cho iOS 13+)
      if (typeof DeviceOrientationEvent.requestPermission === "function") {
        DeviceOrientationEvent.requestPermission()
          .then((permissionState) => {
            if (permissionState === "granted") {
              window.addEventListener("deviceorientation", handleOrientation);
              console.log("DeviceOrientation permission granted.");
            } else {
              console.warn("DeviceOrientation permission denied.");
              alert("Device orientation permission was denied.");
            }
          })
          .catch(console.error);
      } else {
        // Cho các trình duyệt/OS khác không cần requestPermission tường minh
        window.addEventListener("deviceorientation", handleOrientation);
        console.log(
          "Listening to deviceorientation (no explicit permission needed or already granted)."
        );
      }
    } else {
      alert("DeviceOrientationEvent not supported.");
    }
  }
  function stopOrientationListener() {
    window.removeEventListener("deviceorientation", handleOrientation);
    console.log("Stopped listening to deviceorientation.");
    // document.getElementById('orientation-data').innerHTML = 'Listener stopped.';
  }
  // <button onclick="startOrientationListener()">Start Orientation</button>
  // <button onclick="stopOrientationListener()">Stop Orientation</button>
  // <div id="orientation-data">No data yet.</div>
  // <img id="rotating-image" src="https://via.placeholder.com/100" alt="Rotating">
  ```

  Trên iOS 13+, bạn cần gọi `DeviceOrientationEvent.requestPermission()` như một phản hồi cho một hành động của người dùng (ví dụ: click nút).

- **`DeviceMotionEvent` (`devicemotion` event):**

  - Cung cấp thông tin về gia tốc của thiết bị.
  - Dữ liệu trả về bao gồm:
    - `event.acceleration`: Gia tốc của thiết bị không tính đến tác động của trọng lực (m/s²). Có các thuộc tính `x`, `y`, `z`.
    - `event.accelerationIncludingGravity`: Gia tốc của thiết bị có tính cả trọng lực (m/s²). Có các thuộc tính `x`, `y`, `z`. Khi thiết bị nằm yên, giá trị này sẽ phản ánh vector trọng lực.
    - `event.rotationRate`: Tốc độ xoay của thiết bị (độ/giây). Có các thuộc tính `alpha`, `beta`, `gamma`.
    - `event.interval`: Khoảng thời gian (ms) giữa các lần cập nhật dữ liệu (có thể không được hỗ trợ trên mọi thiết bị).

  ```javascript
  function handleMotion(event) {
    const acc = event.acceleration;
    const accG = event.accelerationIncludingGravity;
    const rotRate = event.rotationRate;

    // console.clear(); // Để dễ đọc console log
    // if (acc) console.log(`Acceleration: X=${acc.x?.toFixed(2)}, Y=${acc.y?.toFixed(2)}, Z=${acc.z?.toFixed(2)}`);
    // if (accG) console.log(`Acceleration w/ Gravity: X=${accG.x?.toFixed(2)}, Y=${accG.y?.toFixed(2)}, Z=${accG.z?.toFixed(2)}`);
    // if (rotRate) console.log(`Rotation Rate: Alpha=${rotRate.alpha?.toFixed(2)}, Beta=${rotRate.beta?.toFixed(2)}, Gamma=${rotRate.gamma?.toFixed(2)}`);
    // console.log(`Interval: ${event.interval}ms`);

    // Ví dụ: Phát hiện lắc điện thoại
    // if (accG && (Math.abs(accG.x) > 15 || Math.abs(accG.y) > 15 || Math.abs(accG.z) > 15) ) {
    //    console.log("Device shaken!");
    //    document.body.style.backgroundColor = document.body.style.backgroundColor === 'red' ? 'white' : 'red';
    // }

    // document.getElementById('motion-data').innerHTML =
    //   `Acc X: ${accG.x?.toFixed(2)}<br>Acc Y: ${accG.y?.toFixed(2)}<br>Acc Z: ${accG.z?.toFixed(2)}`;
  }

  function startMotionListener() {
    if (window.DeviceMotionEvent) {
      // Yêu cầu quyền (cho iOS 13+)
      if (typeof DeviceMotionEvent.requestPermission === "function") {
        DeviceMotionEvent.requestPermission()
          .then((permissionState) => {
            if (permissionState === "granted") {
              window.addEventListener("devicemotion", handleMotion);
              console.log("DeviceMotion permission granted.");
            } else {
              console.warn("DeviceMotion permission denied.");
              alert("Device motion permission was denied.");
            }
          })
          .catch(console.error);
      } else {
        window.addEventListener("devicemotion", handleMotion);
        console.log("Listening to devicemotion.");
      }
    } else {
      alert("DeviceMotionEvent not supported.");
    }
  }
  function stopMotionListener() {
    window.removeEventListener("devicemotion", handleMotion);
    console.log("Stopped listening to devicemotion.");
    // document.getElementById('motion-data').innerHTML = 'Listener stopped.';
  }

  // <button onclick="startMotionListener()">Start Motion</button>
  // <button onclick="stopMotionListener()">Stop Motion</button>
  // <div id="motion-data">No data yet.</div>
  ```

  Giống như `DeviceOrientationEvent`, `DeviceMotionEvent.requestPermission()` cần thiết cho iOS 13+.

**3. Các API Phần Cứng Khác (Web Bluetooth, WebUSB, WebMIDI, WebXR - Giới thiệu sơ lược):**

Đây là các API tiên tiến hơn, cho phép tương tác sâu hơn với các thiết bị ngoại vi và tạo ra trải nghiệm thực tế ảo/tăng cường. Chúng thường có yêu cầu bảo mật cao hơn và có thể chưa được hỗ trợ rộng rãi trên tất cả các trình duyệt.

- **Web Bluetooth API:**

  - Cho phép các trang web giao tiếp với các thiết bị Bluetooth Low Energy (BLE) gần đó.
  - Quy trình: Yêu cầu thiết bị (`navigator.bluetooth.requestDevice()`) -> Kết nối với GATT server của thiết bị -> Lấy services và characteristics -> Đọc/ghi/subscribe vào characteristics.
  - Yêu cầu sự tương tác của người dùng để chọn thiết bị từ danh sách.
  - **Trường hợp sử dụng:** Điều khiển thiết bị IoT, đọc dữ liệu từ cảm biến đeo tay, tương tác với đồ chơi thông minh.
  - Ví dụ (rất đơn giản, chỉ scan):
    ```javascript
    // async function scanBluetoothDevices() {
    //   if (!navigator.bluetooth) {
    //     alert('Web Bluetooth API not available.');
    //     return;
    //   }
    //   try {
    //     console.log('Requesting Bluetooth device...');
    //     // Bạn cần chỉ định service bạn muốn tương tác, hoặc chấp nhận tất cả
    //     const device = await navigator.bluetooth.requestDevice({
    //       // acceptAllDevices: true // Không nên dùng trong production, hãy lọc theo services
    //       filters: [{ services: ['heart_rate'] }] // Ví dụ: chỉ tìm thiết bị có service đo nhịp tim
    //     });
    //     console.log('Device found:', device.name || `ID: ${device.id}`);
    //     // Tiếp theo là kết nối device.gatt.connect() và làm việc với services/characteristics
    //   } catch (error) {
    //     console.error('Bluetooth Error:', error);
    //   }
    // }
    // <button onclick="scanBluetoothDevices()">Scan Bluetooth</button>
    ```

- **WebUSB API:**

  - Cho phép các trang web giao tiếp với các thiết bị USB.
  - Tương tự Web Bluetooth, cần người dùng chọn thiết bị từ danh sách do trình duyệt hiển thị.
  - Quy trình: Yêu cầu thiết bị (`navigator.usb.requestDevice()`) -> Mở thiết bị -> Chọn configuration -> Claim interface -> Thực hiện transfer (control, bulk, interrupt, isochronous).
  - **Trường hợp sử dụng:** Giao tiếp với bo mạch phát triển (Arduino, Raspberry Pi), máy in 3D, thiết bị lưu trữ đặc biệt.
  - Ví dụ (chỉ scan):
    ```javascript
    // async function scanUsbDevices() {
    //   if (!navigator.usb) {
    //     alert('WebUSB API not available.');
    //     return;
    //   }
    //   try {
    //     console.log('Requesting USB device...');
    //     const device = await navigator.usb.requestDevice({
    //       filters: [{ vendorId: 0x2341 }] // Ví dụ: Arduino vendor ID
    //     });
    //     console.log('USB Device found:', device.productName, device.manufacturerName);
    //     // Tiếp theo là device.open(), selectConfiguration(), claimInterface()...
    //   } catch (error) {
    //     console.error('WebUSB Error:', error);
    //   }
    // }
    // <button onclick="scanUsbDevices()">Scan USB</button>
    ```

- **Web MIDI API:**

  - Cho phép các trang web tương tác với các thiết bị MIDI (Musical Instrument Digital Interface) như keyboard, synthesizer.
  - Quy trình: Yêu cầu quyền truy cập MIDI (`navigator.requestMIDIAccess()`) -> Lấy danh sách input/output ports -> Gửi/nhận tin nhắn MIDI.
  - **Trường hợp sử dụng:** Tạo ứng dụng học nhạc, trình chỉnh sửa MIDI, điều khiển synthesizer ảo.

- **WebXR Device API (XR = Extended Reality: AR/VR):**
  - Cho phép tạo ra các trải nghiệm Thực tế ảo (VR) và Thực tế tăng cường (AR) ngay trong trình duyệt.
  - API phức tạp, thường được sử dụng cùng với các thư viện đồ họa 3D như Three.js hoặc Babylon.js.
  - Quy trình: Kiểm tra hỗ trợ XR -> Yêu cầu một XR session (`navigator.xr.requestSession('immersive-vr' | 'immersive-ar' | 'inline')`) -> Thiết lập render loop -> Lấy pose (vị trí và hướng) của người xem và các controller -> Render cảnh 3D cho mỗi mắt (trong VR) hoặc chồng lên camera feed (trong AR).
  - **Trường hợp sử dụng:** Game VR/AR, ứng dụng giáo dục tương tác, xem sản phẩm 3D, visualizations.

**4. Ambient Light Sensor API:**

API này cho phép ứng dụng web đọc mức độ ánh sáng xung quanh môi trường của thiết bị.

- **Trường hợp sử dụng:** Tự động điều chỉnh độ sáng/tối của giao diện ứng dụng (dark mode/light mode) dựa trên ánh sáng môi trường.
- **Cách sử dụng:**

  ```javascript
  // let ambientLightSensor;

  // function startAmbientLightSensor() {
  //   if ('AmbientLightSensor' in window) {
  //     try {
  //       ambientLightSensor = new AmbientLightSensor({ frequency: 1 }); // Đọc mỗi giây 1 lần

  //       ambientLightSensor.onreading = () => {
  //         const illuminance = ambientLightSensor.illuminance;
  //         console.log(`Current light level: ${illuminance} lux`);
  //         // document.getElementById('light-level').textContent = `Light: ${illuminance.toFixed(2)} lux`;
  //         // if (illuminance < 50) { // Ngưỡng ví dụ cho dark mode
  //         //   document.body.classList.add('dark-mode');
  //         //   document.body.classList.remove('light-mode');
  //         // } else {
  //         //   document.body.classList.add('light-mode');
  //         //   document.body.classList.remove('dark-mode');
  //         // }
  //       };

  //       ambientLightSensor.onerror = (event) => {
  //         console.error('AmbientLightSensor Error:', event.error.name, event.error.message);
  //         if (event.error.name === 'NotAllowedError') {
  //           alert('Permission to access ambient light sensor was denied.');
  //         }
  //       };
  //       ambientLightSensor.start();
  //       console.log('AmbientLightSensor started.');
  //     } catch (error) {
  //       console.error('Failed to construct AmbientLightSensor:', error);
  //       if (error.name === 'SecurityError') {
  //         alert('Access to AmbientLightSensor is not allowed from an insecure context (HTTP). Use HTTPS.');
  //       } else if (error.name === 'ReferenceError') {
  //         alert('AmbientLightSensor API not fully supported or enabled in this browser.');
  //       }
  //     }
  //   } else {
  //     alert('AmbientLightSensor API not supported by this browser.');
  //   }
  // }

  // function stopAmbientLightSensor() {
  //   if (ambientLightSensor) {
  //     ambientLightSensor.stop();
  //     console.log('AmbientLightSensor stopped.');
  //     // document.getElementById('light-level').textContent = 'Sensor stopped.';
  //   }
  // }

  // // CSS ví dụ
  // // .dark-mode { background-color: #333; color: white; }
  // // .light-mode { background-color: #fff; color: black; }

  // // <button onclick="startAmbientLightSensor()">Start Light Sensor</button>
  // // <button onclick="stopAmbientLightSensor()">Stop Light Sensor</button>
  // // <div id="light-level">No data yet.</div>
  ```

  - API này vẫn còn trong giai đoạn phát triển và có thể cần cờ (flag) để kích hoạt trên một số trình duyệt hoặc có thể thay đổi.
  - Yêu cầu quyền 'sensors' hoặc một quyền cụ thể hơn.

**Kết luận:**
Các API này mở ra những khả năng to lớn cho ứng dụng web, cho phép chúng vượt ra ngoài giới hạn của một tài liệu truyền thống và tương tác với thế giới vật lý xung quanh người dùng. Tuy nhiên, luôn phải đặt quyền riêng tư và sự đồng ý của người dùng lên hàng đầu khi làm việc với chúng. Hãy kiểm tra kỹ tài liệu MDN và `caniuse.com` để biết mức độ hỗ trợ hiện tại của từng API trên các trình duyệt.

**Phần 10: Media & Streaming (Âm Thanh và Video)**

JavaScript cung cấp các API mạnh mẽ để làm việc với âm thanh và video, từ việc phát các file media đơn giản, truy cập webcam/microphone, ghi âm/ghi hình, cho đến việc xử lý âm thanh phức tạp và streaming video hiệu quả.

**1. Phát Âm Thanh và Video Cơ Bản (`<audio>` và `<video>` elements):**

Cách đơn giản nhất để phát media là sử dụng các thẻ HTML `<audio>` và `<video>`. JavaScript có thể điều khiển các phần tử này.

- **HTML:**

  ```html
  <audio id="myAudio" src="audio.mp3" controls preload="auto"></audio>
  <video
    id="myVideo"
    src="video.mp4"
    width="640"
    height="360"
    controls
    poster="poster.jpg"
  ></video>

  <div>
    <button onclick="playAudio()">Play Audio</button>
    <button onclick="pauseAudio()">Pause Audio</button>
    <input
      type="range"
      id="volumeAudio"
      min="0"
      max="1"
      step="0.1"
      value="1"
      oninput="setAudioVolume(this.value)"
    />
  </div>
  <div>
    <button onclick="playVideo()">Play Video</button>
    <button onclick="pauseVideo()">Pause Video</button>
    <button onclick="toggleFullScreenVideo()">Toggle Fullscreen</button>
    <span id="videoTime">00:00 / 00:00</span>
  </div>
  ```

- **JavaScript Điều Khiển:**

  ```javascript
  const audioEl = document.getElementById("myAudio");
  const videoEl = document.getElementById("myVideo");
  const videoTimeDisplay = document.getElementById("videoTime");

  // --- Audio Controls ---
  function playAudio() {
    audioEl.play();
  }
  function pauseAudio() {
    audioEl.pause();
  }
  function setAudioVolume(value) {
    audioEl.volume = parseFloat(value);
  }

  // --- Video Controls ---
  function playVideo() {
    videoEl.play();
  }
  function pauseVideo() {
    videoEl.pause();
  }

  function toggleFullScreenVideo() {
    if (!document.fullscreenElement) {
      // Nếu chưa có gì fullscreen
      if (videoEl.requestFullscreen) {
        videoEl.requestFullscreen();
      } else if (videoEl.mozRequestFullScreen) {
        /* Firefox */
        videoEl.mozRequestFullScreen();
      } else if (videoEl.webkitRequestFullscreen) {
        /* Chrome, Safari & Opera */
        videoEl.webkitRequestFullscreen();
      } else if (videoEl.msRequestFullscreen) {
        /* IE/Edge */
        videoEl.msRequestFullscreen();
      }
    } else {
      if (document.exitFullscreen) {
        document.exitFullscreen();
      }
    }
  }

  // --- Media Events ---
  if (audioEl) {
    audioEl.onplay = () => console.log("Audio started playing.");
    audioEl.onpause = () => console.log("Audio paused.");
    audioEl.onended = () => console.log("Audio finished.");
    audioEl.onvolumechange = () =>
      console.log("Audio volume changed to:", audioEl.volume);
  }

  if (videoEl) {
    videoEl.onloadedmetadata = () => {
      // Khi metadata (duration, dimensions) đã tải
      console.log("Video metadata loaded. Duration:", videoEl.duration);
      updateVideoTime();
    };
    videoEl.ontimeupdate = () => {
      // Khi currentTime thay đổi
      updateVideoTime();
    };
    videoEl.onplay = () => console.log("Video playing");
    videoEl.onpause = () => console.log("Video paused");
    videoEl.onended = () => console.log("Video ended");
    // Các event khác: loadeddata, canplay, canplaythrough, seeking, seeked, waiting, error, etc.
  }

  function formatTime(seconds) {
    const minutes = Math.floor(seconds / 60);
    const secs = Math.floor(seconds % 60);
    return `${minutes.toString().padStart(2, "0")}:${secs
      .toString()
      .padStart(2, "0")}`;
  }

  function updateVideoTime() {
    if (videoEl && videoTimeDisplay) {
      const currentTime = formatTime(videoEl.currentTime);
      const duration = formatTime(videoEl.duration || 0);
      videoTimeDisplay.textContent = `${currentTime} / ${duration}`;
    }
  }

  // Thuộc tính quan trọng của HTMLMediaElement (audioEl, videoEl):
  // - src: URL của file media.
  // - currentTime: Thời gian phát hiện tại (giây). Có thể set để seek.
  // - duration: Tổng thời gian của media (giây, read-only).
  // - paused: Boolean, true nếu đang pause.
  // - ended: Boolean, true nếu đã phát xong.
  // - muted: Boolean, true nếu âm thanh bị tắt. Có thể set.
  // - volume: Âm lượng (0.0 đến 1.0). Có thể set.
  // - loop: Boolean, true nếu lặp lại.
  // - controls: Boolean, hiển thị/ẩn controls mặc định của trình duyệt.
  // - autoplay: Boolean, tự động phát (thường bị chặn bởi trình duyệt nếu không có tương tác người dùng).
  // - playbackRate: Tốc độ phát (ví dụ: 1.0 là bình thường, 0.5 là chậm, 2.0 là nhanh).
  // - buffered: Đối tượng TimeRanges, cho biết phần nào của media đã được buffer.
  // - seeking: Boolean, true nếu đang trong quá trình seek.
  // - networkState: Trạng thái mạng (NETWORK_EMPTY, NETWORK_IDLE, NETWORK_LOADING, NETWORK_NO_SOURCE).
  // - readyState: Trạng thái sẵn sàng của media (HAVE_NOTHING, HAVE_METADATA, HAVE_CURRENT_DATA, HAVE_FUTURE_DATA, HAVE_ENOUGH_DATA).
  ```

**2. `navigator.mediaDevices.getUserMedia()`: Truy Cập Webcam và Microphone**

API này cho phép bạn yêu cầu quyền truy cập vào camera và/hoặc microphone của người dùng để lấy luồng media (MediaStream).

- **Yêu cầu quyền và lấy luồng:**

  ```html
  <video
    id="userVideo"
    autoplay
    playsinline
    style="width:320px; height:240px; border:1px solid black;"
  ></video>
  <audio id="userAudio" autoplay style="display:none;"></audio>
  <!-- Có thể không cần thẻ audio nếu chỉ xử lý stream -->
  <button onclick="startWebcam()">Start Webcam & Mic</button>
  <button onclick="stopWebcam()">Stop Webcam & Mic</button>
  <div id="mediaError"></div>
  <script>
    let localStream = null;
    const userVideoEl = document.getElementById("userVideo");
    const errorDisplay = document.getElementById("mediaError");

    async function startWebcam() {
      if (!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia) {
        errorDisplay.textContent =
          "getUserMedia is not supported in this browser.";
        return;
      }

      // Constraints: Yêu cầu cụ thể về media
      const constraints = {
        audio: true, // Yêu cầu microphone
        // audio: { deviceId: 'specificMicrophoneId' }, // Chọn mic cụ thể nếu biết ID
        video: {
          // Yêu cầu camera
          width: { min: 640, ideal: 1280, max: 1920 },
          height: { min: 480, ideal: 720, max: 1080 },
          facingMode: "user", // "user" (front camera), "environment" (rear camera)
          // frameRate: { ideal: 30, max: 60 }
        },
      };

      try {
        // Yêu cầu quyền và lấy MediaStream
        const stream = await navigator.mediaDevices.getUserMedia(constraints);
        localStream = stream; // Lưu lại stream để có thể dừng sau
        errorDisplay.textContent = "";

        // Gán stream cho phần tử <video> để hiển thị
        userVideoEl.srcObject = stream;
        // userVideoEl.onloadedmetadata = () => { userVideoEl.play(); }; // autoplay thường đã đủ

        console.log("MediaStream acquired:", stream);
        // Lấy các tracks từ stream
        const videoTracks = stream.getVideoTracks();
        const audioTracks = stream.getAudioTracks();
        if (videoTracks.length > 0)
          console.log("Using video device:", videoTracks[0].label);
        if (audioTracks.length > 0)
          console.log("Using audio device:", audioTracks[0].label);
      } catch (err) {
        console.error("Error accessing media devices.", err);
        let message = `Error: ${err.name} - ${err.message}. `;
        if (
          err.name === "NotAllowedError" ||
          err.name === "PermissionDeniedError"
        ) {
          message += "Permission to access camera/microphone was denied.";
        } else if (
          err.name === "NotFoundError" ||
          err.name === "DevicesNotFoundError"
        ) {
          message += "No camera/microphone found.";
        } else if (
          err.name === "NotReadableError" ||
          err.name === "TrackStartError"
        ) {
          message +=
            "Camera/microphone is already in use or a hardware error occurred.";
        }
        errorDisplay.textContent = message;
      }
    }

    function stopWebcam() {
      if (localStream) {
        localStream.getTracks().forEach((track) => {
          track.stop(); // Dừng từng track
        });
        userVideoEl.srcObject = null; // Xóa stream khỏi video element
        localStream = null;
        console.log("MediaStream stopped.");
        errorDisplay.textContent = "Webcam and Mic stopped.";
      }
    }

    // Lấy danh sách thiết bị media (camera, mic, loa)
    async function listMediaDevices() {
      if (!navigator.mediaDevices || !navigator.mediaDevices.enumerateDevices) {
        console.log("enumerateDevices() not supported.");
        return;
      }
      try {
        const devices = await navigator.mediaDevices.enumerateDevices();
        devices.forEach((device) => {
          console.log(
            `${device.kind}: ${device.label || "no label"} id = ${
              device.deviceId
            }`
          );
          // device.kind là 'audioinput', 'audiooutput', 'videoinput'
        });
      } catch (err) {
        console.error("Error listing devices:", err);
      }
    }
    // listMediaDevices(); // Gọi để xem danh sách thiết bị
  </script>
  ```

- **`MediaStream` Object:** Đại diện cho một luồng dữ liệu media, chứa một hoặc nhiều `MediaStreamTrack` (ví dụ: một track video, một track audio).
- **Quan trọng:** Luôn dừng các tracks (`track.stop()`) khi không còn sử dụng `MediaStream` để giải phóng camera/microphone.

**3. `MediaRecorder` API: Ghi Âm Thanh và Video**

`MediaRecorder` API cho phép bạn ghi lại dữ liệu từ một `MediaStream` (ví dụ: từ `getUserMedia`) thành một file hoặc `Blob`.

- **Cách sử dụng:**

  ```html
  <video
    id="liveVideo"
    autoplay
    playsinline
    muted
    style="width:320px; height:240px; border:1px solid black;"
  ></video>
  <video
    id="recordedVideo"
    controls
    style="width:320px; height:240px; border:1px solid green; margin-top:10px;"
  ></video>
  <button id="startRecordBtn" onclick="startRecording()">
    Start Recording
  </button>
  <button id="stopRecordBtn" onclick="stopRecording()" disabled>
    Stop Recording
  </button>
  <a id="downloadLink" style="display:none;">Download Recorded Video</a>
  <div id="recordError"></div>

  <script>
    let mediaRecorder;
    let recordedChunks = [];
    let recordingStream = null; // Stream đang được ghi
    const liveVideoEl = document.getElementById("liveVideo");
    const recordedVideoEl = document.getElementById("recordedVideo");
    const startBtn = document.getElementById("startRecordBtn");
    const stopBtn = document.getElementById("stopRecordBtn");
    const downloadLink = document.getElementById("downloadLink");
    const recordErrorDisplay = document.getElementById("recordError");

    async function setupStreamForRecording() {
      try {
        const stream = await navigator.mediaDevices.getUserMedia({
          video: true,
          audio: true,
        });
        recordingStream = stream;
        liveVideoEl.srcObject = stream;
        return stream;
      } catch (err) {
        recordErrorDisplay.textContent = `Error setting up stream: ${err.message}`;
        console.error("Error accessing media for recording:", err);
        return null;
      }
    }

    async function startRecording() {
      recordErrorDisplay.textContent = "";
      recordedChunks = []; // Reset chunks
      downloadLink.style.display = "none";
      recordedVideoEl.src = ""; // Xóa video đã ghi trước đó

      if (!recordingStream) {
        const stream = await setupStreamForRecording();
        if (!stream) return; // Không lấy được stream
      }

      // Chọn MIME type (định dạng file)
      const options = { mimeType: "video/webm; codecs=vp9,opus" }; // VP9 cho video, Opus cho audio
      // const options = { mimeType: 'video/mp4' }; // Có thể không được hỗ trợ rộng rãi để ghi trực tiếp
      if (!MediaRecorder.isTypeSupported(options.mimeType)) {
        console.warn(`${options.mimeType} is not supported. Trying default.`);
        // Thử các loại khác hoặc không truyền options để trình duyệt tự chọn
        try {
          options.mimeType = "video/webm; codecs=vp8,opus";
        } catch (e) {}
        if (!MediaRecorder.isTypeSupported(options.mimeType)) {
          try {
            options.mimeType = "video/webm";
          } catch (e) {}
          if (!MediaRecorder.isTypeSupported(options.mimeType)) {
            recordErrorDisplay.textContent =
              "No supported mimeType found for MediaRecorder.";
            return;
          }
        }
      }
      console.log("Using mimeType:", options.mimeType);

      try {
        mediaRecorder = new MediaRecorder(recordingStream, options);

        mediaRecorder.onstart = (event) => {
          console.log("Recording started.");
          startBtn.disabled = true;
          stopBtn.disabled = false;
        };

        mediaRecorder.ondataavailable = (event) => {
          if (event.data && event.data.size > 0) {
            recordedChunks.push(event.data); // Thu thập các chunk dữ liệu
            console.log("Data chunk available, size:", event.data.size);
          }
        };

        mediaRecorder.onstop = (event) => {
          console.log("Recording stopped.");
          startBtn.disabled = false;
          stopBtn.disabled = true;

          // Tạo Blob từ các chunks đã thu thập
          const blob = new Blob(recordedChunks, { type: options.mimeType }); // Type quan trọng
          const recordedUrl = URL.createObjectURL(blob);

          recordedVideoEl.src = recordedUrl; // Hiển thị video đã ghi
          downloadLink.href = recordedUrl;
          downloadLink.download = `recorded_video.${
            options.mimeType.split("/")[1].split(";")[0]
          }`; // Ví dụ: recorded_video.webm
          downloadLink.style.display = "block";
          downloadLink.textContent = `Download ${downloadLink.download}`;

          // Dừng stream gốc nếu không cần nữa
          // if (recordingStream) {
          //   recordingStream.getTracks().forEach(track => track.stop());
          //   liveVideoEl.srcObject = null;
          //   recordingStream = null;
          // }
        };

        mediaRecorder.onerror = (event) => {
          console.error("MediaRecorder Error:", event.error);
          recordErrorDisplay.textContent = `Recorder Error: ${event.error.name} - ${event.error.message}`;
          startBtn.disabled = false;
          stopBtn.disabled = true;
        };

        mediaRecorder.start(1000); // Bắt đầu ghi, và tạo data chunk mỗi 1000ms (tùy chọn)
        // Nếu không có timeslice, ondataavailable chỉ kích hoạt khi stop()
      } catch (e) {
        recordErrorDisplay.textContent = `Error creating MediaRecorder: ${e.toString()}`;
        console.error("Error creating MediaRecorder:", e);
      }
    }

    function stopRecording() {
      if (mediaRecorder && mediaRecorder.state !== "inactive") {
        mediaRecorder.stop();
      }
    }

    // Gọi setupStreamForRecording khi tải trang nếu muốn webcam hiển thị ngay
    // (async () => await setupStreamForRecording())();
  </script>
  ```

- `MediaRecorder.isTypeSupported(mimeType)`: Kiểm tra xem trình duyệt có hỗ trợ ghi với MIME type đó không.
- `timeslice` trong `mediaRecorder.start(timeslice)`: Nếu được cung cấp, sự kiện `ondataavailable` sẽ được kích hoạt sau mỗi `timeslice` mili giây (hoặc khi buffer đầy), cho phép xử lý dữ liệu ghi theo từng phần (ví dụ: streaming lên server).

**4. Web Audio API: Xử Lý và Tổng Hợp Âm Thanh Nâng Cao**

Web Audio API là một API cấp cao và linh hoạt để xử lý và tổng hợp âm thanh trong trình duyệt. Nó cho phép bạn:

- Nạp và phát âm thanh từ nhiều nguồn (file, stream, oscillator).
- Áp dụng các hiệu ứng âm thanh (filter, delay, reverb, panner).
- Phân tích âm thanh (tần số, waveform).
- Tạo âm thanh từ đầu (synthesis).
- Xử lý không gian âm thanh 3D.

- **Khái niệm cốt lõi:**

  - **`AudioContext`**: Đối tượng trung tâm, quản lý tất cả các node âm thanh và đồ thị xử lý.
  - **Audio Nodes**: Các khối xử lý âm thanh. Có nhiều loại node:
    - **Source Nodes**: Nguồn phát âm thanh (ví dụ: `AudioBufferSourceNode` cho file, `MediaStreamAudioSourceNode` cho mic/webcam, `OscillatorNode` cho sóng âm cơ bản).
    - **Effect/Processing Nodes**: Thay đổi âm thanh (ví dụ: `GainNode` cho âm lượng, `BiquadFilterNode` cho filter, `DelayNode`, `ConvolverNode` cho reverb).
    - **Destination Node**: Điểm cuối của chuỗi xử lý, thường là loa của người dùng (`audioContext.destination`).
  - **Audio Graph**: Các node được kết nối với nhau (`node1.connect(node2)`) để tạo thành một đồ thị, nơi âm thanh chảy từ source qua các processing node đến destination.

- **Ví dụ: Phát file âm thanh với Gain (âm lượng) và Filter**

  ```html
  <input type="file" id="audioFile" accept="audio/*" />
  <button onclick="playSelectedAudio()">Play with Effects</button>
  <label
    >Volume:
    <input
      type="range"
      id="audioVolume"
      min="0"
      max="2"
      step="0.1"
      value="1"
      oninput="changeGain(this.value)"
  /></label>
  <label
    >Filter Freq:
    <input
      type="range"
      id="filterFreq"
      min="20"
      max="20000"
      step="10"
      value="20000"
      oninput="changeFilterFreq(this.value)"
  /></label>
  <div id="audioError"></div>

  <script>
    let audioContext;
    let sourceNode;
    let gainNode;
    let biquadFilter;
    let audioBuffer; // Nơi lưu trữ dữ liệu âm thanh đã decode

    const fileInput = document.getElementById("audioFile");
    const volumeSlider = document.getElementById("audioVolume");
    const filterSlider = document.getElementById("filterFreq");
    const audioErrorDisplay = document.getElementById("audioError");

    function initAudioContext() {
      try {
        audioContext = new (window.AudioContext || window.webkitAudioContext)();
        console.log("AudioContext created.");

        // Tạo các node chính
        gainNode = audioContext.createGain();
        biquadFilter = audioContext.createBiquadFilter();
        biquadFilter.type = "lowpass"; // Các loại khác: highpass, bandpass, lowshelf, highshelf, peaking, notch, allpass
        biquadFilter.frequency.setValueAtTime(
          parseFloat(filterSlider.value),
          audioContext.currentTime
        ); // Giá trị ban đầu

        // Kết nối các node: Source -> Filter -> Gain -> Destination (Loa)
        // Source sẽ được tạo khi file được load
        biquadFilter.connect(gainNode);
        gainNode.connect(audioContext.destination);

        // Thiết lập giá trị ban đầu từ slider
        gainNode.gain.setValueAtTime(
          parseFloat(volumeSlider.value),
          audioContext.currentTime
        );
      } catch (e) {
        audioErrorDisplay.textContent =
          "Web Audio API is not supported in this browser.";
        console.error("Error creating AudioContext:", e);
      }
    }

    fileInput.addEventListener("change", async function (event) {
      if (!audioContext) initAudioContext();
      if (!audioContext) return; // Không tạo được context

      const file = event.target.files[0];
      if (file) {
        audioErrorDisplay.textContent = "Loading audio...";
        try {
          const arrayBuffer = await file.arrayBuffer();
          // Decode dữ liệu âm thanh từ ArrayBuffer thành AudioBuffer
          audioContext.decodeAudioData(
            arrayBuffer,
            (buffer) => {
              audioBuffer = buffer; // Lưu lại buffer để có thể phát nhiều lần
              audioErrorDisplay.textContent = `Audio "${file.name}" loaded. Ready to play.`;
              console.log("Audio decoded and ready.");
            },
            (error) => {
              audioErrorDisplay.textContent = `Error decoding audio: ${error.message}`;
              console.error("Error decoding audio data:", error);
            }
          );
        } catch (e) {
          audioErrorDisplay.textContent = `Error reading file: ${e.message}`;
          console.error("Error reading file:", e);
        }
      }
    });

    function playSelectedAudio() {
      if (!audioContext) {
        audioErrorDisplay.textContent =
          "AudioContext not initialized. Select a file first.";
        return;
      }
      if (!audioBuffer) {
        audioErrorDisplay.textContent = "No audio loaded. Select a file first.";
        return;
      }

      // Dừng source cũ nếu đang phát
      if (sourceNode) {
        try {
          sourceNode.stop();
        } catch (e) {}
      }

      // Tạo AudioBufferSourceNode mới cho mỗi lần phát
      sourceNode = audioContext.createBufferSource();
      sourceNode.buffer = audioBuffer; // Gán AudioBuffer đã decode

      // Kết nối source mới vào graph
      sourceNode.connect(biquadFilter); // Source -> Filter (Filter đã connect tới Gain -> Dest)

      sourceNode.onended = () => {
        console.log("Audio playback finished.");
        // sourceNode.disconnect(); // Tự động disconnect khi stop, nhưng có thể làm tường minh
        // sourceNode = null;
      };

      // Đảm bảo AudioContext đang chạy (quan trọng sau tương tác người dùng)
      if (audioContext.state === "suspended") {
        audioContext.resume().then(() => {
          console.log("AudioContext resumed.");
          sourceNode.start(0); // Bắt đầu phát từ đầu (offset 0)
          audioErrorDisplay.textContent = "Playing...";
        });
      } else {
        sourceNode.start(0);
        audioErrorDisplay.textContent = "Playing...";
      }
    }

    function changeGain(value) {
      if (gainNode && audioContext) {
        // gain.value là AudioParam, dùng setValueAtTime để thay đổi mượt mà
        gainNode.gain.setValueAtTime(
          parseFloat(value),
          audioContext.currentTime
        );
      }
    }

    function changeFilterFreq(value) {
      if (biquadFilter && audioContext) {
        biquadFilter.frequency.setValueAtTime(
          parseFloat(value),
          audioContext.currentTime
        );
      }
    }

    // Khởi tạo context khi người dùng tương tác (ví dụ: click nút)
    // document.addEventListener('DOMContentLoaded', () => {
    //    // Không nên init context ngay lập tức, đợi user interaction
    // });
  </script>
  ```

- **Autoplay Policy:** Trình duyệt thường chặn `AudioContext` tự động bắt đầu phát âm thanh cho đến khi có tương tác của người dùng (click, key press). Bạn cần gọi `audioContext.resume()` sau một hành động của người dùng nếu `audioContext.state` là `'suspended'`.
- Web Audio API rất mạnh mẽ và có nhiều node, nhiều tham số. Tham khảo MDN là cực kỳ cần thiết.

**5. Media Source Extensions (MSE): Streaming Video Nâng Cao**

MSE là một API cho phép JavaScript tạo các luồng media để phát lại bằng thẻ `<audio>` và `<video>`, nhưng thay vì cung cấp một URL file trực tiếp, bạn cung cấp dữ liệu media theo từng đoạn (chunk) thông qua JavaScript.

- **Mục đích:**
  - **Adaptive Streaming:** Điều chỉnh chất lượng video/audio dựa trên băng thông mạng (giống YouTube, Netflix).
  - **Live Streaming:** Phát các sự kiện trực tiếp.
  - **DRM (Digital Rights Management):** Tích hợp với Encrypted Media Extensions (EME) để phát nội dung được bảo vệ.
  - **Tùy chỉnh buffer và preloading.**
- **Cách hoạt động (tổng quan):**
  1.  Tạo một đối tượng `MediaSource`.
  2.  Tạo một URL object từ `MediaSource` (`URL.createObjectURL(mediaSource)`) và gán nó cho `src` của thẻ `<video>` hoặc `<audio>`.
  3.  Khi `MediaSource` kích hoạt sự kiện `sourceopen`, bạn tạo một hoặc nhiều `SourceBuffer` (`mediaSource.addSourceBuffer(mimeType)`).
  4.  Fetch các đoạn (segment) media từ server (ví dụ: file .mp4, .webm, hoặc các định dạng segment như fMP4, MPEG-TS).
  5.  Nối (append) dữ liệu của các đoạn này vào `SourceBuffer` (`sourceBuffer.appendBuffer(arrayBuffer)`).
  6.  Trình duyệt sẽ tự động xử lý việc decode và phát các đoạn media đã được buffer.
- Đây là một API phức tạp, thường được sử dụng bởi các thư viện player video chuyên dụng (ví dụ: Shaka Player, HLS.js, Dash.js) hơn là triển khai trực tiếp.

  ```javascript
  // Ví dụ rất cơ bản và minh họa (cần file video .webm đã được segment hóa đúng cách)
  // const videoElement = document.getElementById('mseVideo'); // <video id="mseVideo" controls></video>
  // const mediaSource = new MediaSource();
  // videoElement.src = URL.createObjectURL(mediaSource);

  // mediaSource.addEventListener('sourceopen', () => {
  //   try {
  //     // MIME type phải khớp với định dạng của các segment video/audio
  //     const mimeCodec = 'video/webm; codecs="vp8, vorbis"';
  //     if (MediaSource.isTypeSupported(mimeCodec)) {
  //       const sourceBuffer = mediaSource.addSourceBuffer(mimeCodec);

  //       // Hàm fetch và append segment
  //       async function fetchAndAppendSegment(url) {
  //         const response = await fetch(url);
  //         const arrayBuffer = await response.arrayBuffer();
  //         // Đảm bảo sourceBuffer không đang update
  //         if (!sourceBuffer.updating) {
  //           sourceBuffer.appendBuffer(arrayBuffer);
  //         } else {
  //           // Đợi updateend rồi mới append
  //           sourceBuffer.addEventListener('updateend', () => {
  //             try { sourceBuffer.appendBuffer(arrayBuffer); } catch(e) {console.error(e)}
  //           }, { once: true });
  //         }
  //       }

  //       // SourceBuffer event listeners
  //       sourceBuffer.onupdateend = () => {
  //         console.log('SourceBuffer update ended.');
  //         // Có thể fetch segment tiếp theo ở đây nếu cần
  //         // Nếu đã append tất cả, và muốn báo kết thúc:
  //         // if (allSegmentsAppended && !sourceBuffer.updating && mediaSource.readyState === 'open') {
  //         //   mediaSource.endOfStream();
  //         // }
  //       };
  //       sourceBuffer.onerror = (e) => console.error('SourceBuffer error:', e);

  //       // Logic để fetch và append các segment tuần tự
  //       // Ví dụ:
  //       // fetchAndAppendSegment('segment1.webm').then(() => {
  //       //   return fetchAndAppendSegment('segment2.webm');
  //       // }).then(() => {
  //       //   if (mediaSource.readyState === 'open' && !sourceBuffer.updating) {
  //       //      mediaSource.endOfStream(); // Báo hiệu đã hết dữ liệu
  //       //   }
  //       // });

  //       // Để chạy ví dụ này, bạn cần các file segment thực sự
  //       console.log("SourceBuffer created. Ready to append segments.");

  //     } else {
  //       console.error(`MIME type ${mimeCodec} not supported for MediaSource.`);
  //     }
  //   } catch (e) {
  //     console.error("Error in sourceopen handler:", e);
  //   }
  // });

  // mediaSource.addEventListener('sourceended', () => console.log('MediaSource ended.'));
  // mediaSource.addEventListener('sourceclose', () => console.log('MediaSource closed.'));
  // mediaSource.addEventListener('error', (e) => console.error('MediaSource error:', e));
  ```

Các API media này cung cấp một bộ công cụ toàn diện để tạo ra các ứng dụng web đa phương tiện phong phú, từ phát lại đơn giản đến xử lý âm thanh phức tạp và streaming video thích ứng. Luôn nhớ kiểm tra khả năng tương thích của trình duyệt và xử lý các quyền của người dùng một cách cẩn thận.

**Phần 11: File System, Drag-n-Drop và Clipboard API**

Các API này cho phép ứng dụng web tương tác với file hệ thống của người dùng (ở các mức độ khác nhau), xử lý thao tác kéo thả, và tương tác với clipboard hệ thống.

**1. File API: Đọc Dữ Liệu File Phía Client**

File API cho phép JavaScript truy cập nội dung của các file do người dùng chọn thông qua phần tử `<input type="file">` hoặc từ thao tác kéo thả. Nó không cho phép tự ý truy cập file hệ thống.

- **HTML Input Element:**

  ```html
  <label for="fileInput">Chọn một hoặc nhiều file:</label>
  <input type="file" id="fileInput" multiple accept=".txt,image/*,.pdf" />
  <!-- `multiple` cho phép chọn nhiều file -->
  <!-- `accept` gợi ý loại file cho người dùng (không phải là validation chặt chẽ) -->

  <div id="fileInfo"></div>
  <pre id="fileContent"></pre>
  <img
    id="imagePreview"
    style="max-width: 300px; max-height: 300px; display: none;"
  />
  ```

- **JavaScript Xử Lý File:**

  ```javascript
  const fileInputElement = document.getElementById("fileInput");
  const fileInfoDisplay = document.getElementById("fileInfo");
  const fileContentDisplay = document.getElementById("fileContent");
  const imagePreview = document.getElementById("imagePreview");

  fileInputElement.addEventListener("change", handleFiles);

  function handleFiles(event) {
    // event.target.files là một đối tượng FileList (giống mảng)
    const files = event.target.files;
    if (!files || files.length === 0) {
      fileInfoDisplay.innerHTML = "Không có file nào được chọn.";
      fileContentDisplay.textContent = "";
      imagePreview.style.display = "none";
      return;
    }

    fileInfoDisplay.innerHTML = `<h3>Thông tin ${files.length} file đã chọn:</h3>`;
    fileContentDisplay.textContent = ""; // Xóa nội dung cũ
    imagePreview.style.display = "none"; // Ẩn preview cũ

    for (let i = 0; i < files.length; i++) {
      const file = files[i];
      fileInfoDisplay.innerHTML += `
        <p>
          <strong>Tên file:</strong> ${file.name}<br>
          <strong>Loại MIME:</strong> ${file.type || "Không xác định"}<br>
          <strong>Kích thước:</strong> ${(file.size / 1024).toFixed(2)} KB<br>
          <strong>Sửa đổi lần cuối:</strong> ${new Date(
            file.lastModified
          ).toLocaleDateString()}
        </p>
      `;

      // Sử dụng FileReader để đọc nội dung file
      const reader = new FileReader();

      // Xử lý dựa trên loại file
      if (file.type.startsWith("text/")) {
        reader.onload = function (e) {
          fileContentDisplay.textContent += `\n--- Nội dung ${file.name} ---\n`;
          fileContentDisplay.textContent += e.target.result; // e.target.result chứa nội dung file
        };
        reader.readAsText(file); // Đọc file dưới dạng text
      } else if (file.type.startsWith("image/")) {
        reader.onload = function (e) {
          if (i === 0) {
            // Chỉ preview ảnh đầu tiên cho đơn giản
            imagePreview.src = e.target.result; // e.target.result là Data URL
            imagePreview.style.display = "block";
          }
          fileContentDisplay.textContent += `\n--- ${file.name} là một ảnh (xem preview nếu là ảnh đầu tiên) ---`;
        };
        reader.readAsDataURL(file); // Đọc file dưới dạng Data URL (base64 encoded)
      } else {
        // Đối với các loại file khác, có thể đọc dưới dạng ArrayBuffer hoặc không đọc nội dung
        reader.onload = function (e) {
          fileContentDisplay.textContent += `\n--- ${file.name} (dạng nhị phân, kích thước: ${e.target.result.byteLength} bytes) ---`;
          // e.target.result là ArrayBuffer
        };
        reader.readAsArrayBuffer(file); // Đọc file dưới dạng ArrayBuffer (dữ liệu nhị phân)
        // Hoặc chỉ hiển thị thông tin, không đọc nội dung
        // fileContentDisplay.textContent += `\n--- ${file.name} (loại không hỗ trợ preview nội dung) ---`;
      }

      reader.onerror = function (e) {
        console.error("Lỗi đọc file:", e);
        fileContentDisplay.textContent += `\n--- Lỗi khi đọc ${file.name} ---`;
      };

      reader.onprogress = function (e) {
        if (e.lengthComputable) {
          const percentLoaded = Math.round((e.loaded / e.total) * 100);
          console.log(`Đang tải ${file.name}: ${percentLoaded}%`);
        }
      };
    }
  }
  ```

  **Đối tượng `File`:** (Kế thừa từ `Blob`)

  - `name`: Tên file.
  - `size`: Kích thước file (bytes).
  - `type`: Loại MIME của file (ví dụ: `'image/jpeg'`, `'text/plain'`).
  - `lastModified`: Timestamp (milliseconds từ epoch) của lần sửa đổi cuối cùng.
  - `lastModifiedDate`: (Cũ, không nên dùng) Đối tượng `Date` của lần sửa đổi cuối cùng.

  **Đối tượng `FileReader`:**

  - Dùng để đọc nội dung của `File` hoặc `Blob`.
  - Hoạt động bất đồng bộ, sử dụng events:
    - `onloadstart`: Bắt đầu đọc.
    - `onprogress`: Trong quá trình đọc (cung cấp thông tin `loaded` và `total`).
    - `onload`: Đọc thành công. `event.target.result` chứa nội dung.
    - `onerror`: Có lỗi khi đọc.
    - `onabort`: Việc đọc bị hủy (`reader.abort()`).
    - `onloadend`: Đọc hoàn tất (thành công hoặc thất bại).
  - Các phương thức đọc:
    - `readAsText(blob, [encoding])`: Đọc nội dung dưới dạng chuỗi text (mặc định UTF-8).
    - `readAsDataURL(blob)`: Đọc nội dung dưới dạng Data URL (chuỗi base64). Hữu ích để hiển thị ảnh hoặc nhúng dữ liệu trực tiếp.
    - `readAsArrayBuffer(blob)`: Đọc nội dung dưới dạng `ArrayBuffer` (dữ liệu nhị phân thô).
    - `readAsBinaryString(blob)`: (Cũ, không nên dùng) Đọc dưới dạng chuỗi nhị phân thô.
  - `abort()`: Hủy thao tác đọc đang diễn ra.

- **Tạo Object URL từ File/Blob:**
  `URL.createObjectURL(fileOrBlob)` tạo ra một URL đặc biệt (ví dụ: `blob:http://localhost:3000/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`) tham chiếu đến dữ liệu của `File` hoặc `Blob` trong bộ nhớ. URL này có thể được dùng làm `src` cho thẻ `<img>`, `<video>`, `<audio>`, hoặc `href` cho `<a>`.
  ```javascript
  // Trong hàm handleFiles, sau khi lấy được file:
  // if (file.type.startsWith('image/')) {
  //   const objectURL = URL.createObjectURL(file);
  //   imagePreview.src = objectURL;
  //   imagePreview.style.display = 'block';
  //   // Quan trọng: Giải phóng Object URL khi không cần nữa để tránh rò rỉ bộ nhớ
  //   imagePreview.onload = () => { // Hoặc khi component bị unmount
  //     URL.revokeObjectURL(objectURL);
  //     console.log('Object URL revoked:', objectURL);
  //   }
  // }
  ```
  **`URL.revokeObjectURL(objectURL)`:** Cần thiết để giải phóng tài nguyên mà Object URL đang giữ. Nếu không, trình duyệt sẽ giữ file trong bộ nhớ cho đến khi tài liệu bị đóng.

**2. Drag and Drop API:**

Cho phép người dùng kéo (drag) các phần tử (bao gồm cả file từ desktop) và thả (drop) chúng vào các vùng nhất định trên trang web.

- **Các Sự Kiện Chính:**

  - Trên **phần tử được kéo (draggable element):**
    - `dragstart`: Khi bắt đầu kéo.
    - `drag`: Trong khi đang kéo.
    - `dragend`: Khi kết thúc thao tác kéo (dù thành công hay không).
  - Trên **vùng thả (drop target):**
    - `dragenter`: Khi một phần tử được kéo vào vùng thả.
    - `dragover`: Khi một phần tử đang được kéo qua vùng thả (cần `event.preventDefault()` ở đây để cho phép thả).
    - `dragleave`: Khi một phần tử được kéo ra khỏi vùng thả.
    - `drop`: Khi một phần tử được thả vào vùng thả (cần `event.preventDefault()` ở đây để ngăn hành vi mặc định của trình duyệt, ví dụ mở file).

- **`DataTransfer` Object:**

  - Có sẵn trong đối tượng `event` của các sự kiện drag-n-drop (`event.dataTransfer`).
  - Dùng để chứa dữ liệu được kéo và kiểm soát giao diện kéo thả.
  - `dataTransfer.files`: Một `FileList` chứa các file được kéo từ desktop.
  - `dataTransfer.items`: Một `DataTransferItemList` cho phép truy cập dữ liệu kéo thả ở dạng tổng quát hơn (có thể là file, chuỗi, URL).
  - `dataTransfer.setData(format, data)`: Đặt dữ liệu với một định dạng MIME (ví dụ: `'text/plain'`, `'text/html'`).
  - `dataTransfer.getData(format)`: Lấy dữ liệu theo định dạng.
  - `dataTransfer.clearData([format])`: Xóa dữ liệu.
  - `dataTransfer.dropEffect`: Thiết lập hoặc lấy hiệu ứng sẽ xảy ra khi thả (ví dụ: `'copy'`, `'move'`, `'link'`, `'none'`). Thường được set trong `dragover`.
  - `dataTransfer.effectAllowed`: Các loại hiệu ứng được phép cho phần tử kéo (ví dụ: `'copyMove'`, `'all'`).

- **Ví dụ: Kéo thả file vào một `div`:**

  ```html
  <style>
    #dropZone {
      width: 300px;
      height: 200px;
      border: 2px dashed #ccc;
      text-align: center;
      line-height: 200px;
      margin-bottom: 10px;
    }
    #dropZone.dragover {
      border-color: #000;
      background-color: #eee;
    }
  </style>
  <div id="dropZone">Kéo file vào đây</div>
  <div id="droppedFileInfo"></div>
  <pre id="droppedFileContent"></pre>
  <img
    id="droppedImagePreview"
    style="max-width: 300px; max-height: 300px; display: none;"
  />

  <script>
    const dropZoneElement = document.getElementById("dropZone");
    const droppedFileInfoDisplay = document.getElementById("droppedFileInfo");
    const droppedFileContentDisplay =
      document.getElementById("droppedFileContent");
    const droppedImagePreview = document.getElementById("droppedImagePreview");

    dropZoneElement.addEventListener("dragenter", function (event) {
      event.preventDefault(); // Cần thiết
      this.classList.add("dragover");
      console.log("Drag Enter");
    });

    dropZoneElement.addEventListener("dragover", function (event) {
      event.preventDefault(); // Quan trọng: Phải gọi để cho phép drop
      event.dataTransfer.dropEffect = "copy"; // Cho người dùng thấy là sẽ copy
      // this.classList.add('dragover'); // Có thể không cần nếu đã add ở dragenter và remove ở dragleave/drop
      console.log("Drag Over");
    });

    dropZoneElement.addEventListener("dragleave", function (event) {
      event.preventDefault();
      this.classList.remove("dragover");
      console.log("Drag Leave");
    });

    dropZoneElement.addEventListener("drop", function (event) {
      event.preventDefault(); // Quan trọng: Ngăn trình duyệt mở file
      this.classList.remove("dragover");
      console.log("Drop");

      const files = event.dataTransfer.files; // Lấy danh sách file
      if (files.length > 0) {
        // Xử lý files tương tự như với <input type="file"> (dùng hàm handleFiles đã viết ở trên)
        // Ở đây ta copy một phần logic để minh họa:
        droppedFileInfoDisplay.innerHTML = `<h3>File đã thả:</h3>`;
        droppedFileContentDisplay.textContent = "";
        droppedImagePreview.style.display = "none";

        const file = files[0]; // Xử lý file đầu tiên
        droppedFileInfoDisplay.innerHTML += `<p><strong>Tên:</strong> ${file.name}, <strong>Loại:</strong> ${file.type}, <strong>Kích thước:</strong> ${file.size} bytes</p>`;

        const reader = new FileReader();
        if (file.type.startsWith("text/")) {
          reader.onload = (e) =>
            (droppedFileContentDisplay.textContent = e.target.result);
          reader.readAsText(file);
        } else if (file.type.startsWith("image/")) {
          reader.onload = (e) => {
            droppedImagePreview.src = e.target.result;
            droppedImagePreview.style.display = "block";
          };
          reader.readAsDataURL(file);
        } else {
          droppedFileContentDisplay.textContent =
            "Không thể preview loại file này.";
        }
      } else {
        // Xử lý trường hợp kéo thả dữ liệu không phải file (ví dụ text từ trang khác)
        const textData = event.dataTransfer.getData("text/plain");
        if (textData) {
          droppedFileInfoDisplay.innerHTML = "Dữ liệu text đã thả:";
          droppedFileContentDisplay.textContent = textData;
        } else {
          droppedFileInfoDisplay.innerHTML =
            "Không có file hoặc dữ liệu text nào được thả.";
        }
      }
    });

    // Làm cho một phần tử có thể kéo đi (draggable)
    // <div id="draggableElement" draggable="true">Kéo tôi đi</div>
    // const draggable = document.getElementById('draggableElement');
    // if (draggable) {
    //   draggable.addEventListener('dragstart', function(event) {
    //     event.dataTransfer.setData('text/plain', 'Đây là dữ liệu từ draggable element!');
    //     event.dataTransfer.effectAllowed = 'copy';
    //     console.log('Drag Start on draggable element');
    //   });
    //   draggable.addEventListener('dragend', function(event) {
    //     console.log('Drag End on draggable element. Drop effect:', event.dataTransfer.dropEffect);
    //   });
    // }
  </script>
  ```

**3. File System Access API (Khá mới, cần kiểm tra hỗ trợ và quyền):**

API này cho phép các ứng dụng web đọc và ghi file **trực tiếp** trên hệ thống file của người dùng, sau khi người dùng cấp quyền rõ ràng. Đây là một bước tiến lớn so với File API (chỉ đọc file do người dùng chọn).

- **Đặc điểm:**

  - **Yêu cầu HTTPS.**
  - **Yêu cầu tương tác người dùng mạnh mẽ để cấp quyền:** Người dùng phải chủ động chọn file/thư mục thông qua hộp thoại hệ thống.
  - **Quyền được ghi nhớ (persistent) cho một origin nếu người dùng đồng ý.**
  - Cung cấp các đối tượng `FileSystemFileHandle` và `FileSystemDirectoryHandle`.

- **Chức năng chính:**

  - **Mở file picker để người dùng chọn file đọc:** `window.showOpenFilePicker([options])`
    - `options`: có thể chỉ định `multiple: true/false`, `types` (loại file).
    - Trả về một Promise với một mảng các `FileSystemFileHandle`.
  - **Mở file picker để người dùng chọn nơi lưu file mới (hoặc ghi đè file cũ):** `window.showSaveFilePicker([options])`
    - Trả về một Promise với một `FileSystemFileHandle`.
  - **Mở folder picker để người dùng chọn thư mục:** `window.showDirectoryPicker()`
    - Trả về một Promise với một `FileSystemDirectoryHandle`.

- **Làm việc với `FileSystemFileHandle`:**

  - `handle.getFile()`: Trả về một đối tượng `File` (giống File API) để đọc metadata và nội dung.
  - `handle.createWritable()`: Trả về một `FileSystemWritableFileStream` để ghi dữ liệu vào file.
    - `writableStream.write(data)`: Ghi dữ liệu (chuỗi, Blob, ArrayBuffer, hoặc object có `type: 'write'` và `data`).
    - `writableStream.seek(position)`: Di chuyển con trỏ ghi.
    - `writableStream.truncate(size)`: Thay đổi kích thước file.
    - `writableStream.close()`: Đóng stream, đảm bảo dữ liệu được ghi.

- **Làm việc với `FileSystemDirectoryHandle`:**

  - `dirHandle.values()`: Trả về một async iterator để lặp qua các file và thư mục con (trả về `FileSystemFileHandle` hoặc `FileSystemDirectoryHandle`).
  - `dirHandle.getFileHandle(name, [options])`: Lấy handle của một file con. `options.create: true` để tạo nếu chưa có.
  - `dirHandle.getDirectoryHandle(name, [options])`: Lấy handle của một thư mục con. `options.create: true` để tạo nếu chưa có.
  - `dirHandle.removeEntry(name, [options])`: Xóa file hoặc thư mục con. `options.recursive: true` để xóa thư mục không rỗng.
  - `dirHandle.resolve(possibleDescendant)`: Trả về path tương đối từ thư mục này đến một handle con cháu.

- **Ví dụ: Đọc và Ghi file (đơn giản):**

  ```html
  <button onclick="openAndReadFile()">Mở và Đọc File</button>
  <button onclick="saveTextToFile()">Lưu Text vào File</button>
  <textarea
    id="fileEditor"
    rows="10"
    cols="50"
    placeholder="Nội dung file sẽ hiển thị hoặc được lưu từ đây..."
  ></textarea>
  <div id="fsApiStatus"></div>

  <script>
    const editor = document.getElementById("fileEditor");
    const fsStatus = document.getElementById("fsApiStatus");
    let currentFileHandle = null; // Lưu lại handle của file đang mở để có thể lưu lại

    async function openAndReadFile() {
      fsStatus.textContent = "";
      if (!window.showOpenFilePicker) {
        fsStatus.textContent = "File System Access API không được hỗ trợ.";
        return;
      }
      try {
        // Hiển thị hộp thoại chọn file
        const [fileHandle] = await window.showOpenFilePicker({
          types: [
            {
              description: "Text Files",
              accept: { "text/plain": [".txt", ".md"] },
            },
          ],
          multiple: false,
        });
        currentFileHandle = fileHandle; // Lưu lại handle

        // Lấy đối tượng File từ handle
        const file = await fileHandle.getFile();
        // Đọc nội dung file
        const contents = await file.text();
        editor.value = contents;
        fsStatus.textContent = `Đã mở file: ${file.name}`;
        console.log(
          `File "${file.name}" (handle kind: ${fileHandle.kind}) read successfully.`
        );
      } catch (err) {
        if (err.name === "AbortError") {
          fsStatus.textContent = "Người dùng đã hủy chọn file.";
        } else {
          fsStatus.textContent = `Lỗi mở file: ${err.message}`;
          console.error("Lỗi mở file:", err);
        }
      }
    }

    async function saveTextToFile() {
      fsStatus.textContent = "";
      const textToSave = editor.value;

      if (!window.showSaveFilePicker && !currentFileHandle) {
        fsStatus.textContent =
          "File System Access API không được hỗ trợ hoặc chưa có file nào được mở để lưu.";
        return;
      }

      try {
        let fileHandleToSave;
        if (currentFileHandle) {
          // Nếu đã có file handle (ví dụ từ lần mở trước), hỏi người dùng có muốn ghi đè không
          // Hoặc có thể trực tiếp ghi nếu logic ứng dụng cho phép
          console.log(
            "Attempting to save to existing handle:",
            currentFileHandle.name
          );
          // Kiểm tra quyền ghi trước khi cố gắng ghi
          if (await verifyPermission(currentFileHandle, true)) {
            fileHandleToSave = currentFileHandle;
          } else {
            fsStatus.textContent =
              "Không có quyền ghi vào file hiện tại. Hãy chọn 'Save As'.";
            // Buộc người dùng phải Save As
            currentFileHandle = null; // Xóa handle cũ
            fileHandleToSave = await window.showSaveFilePicker({
              suggestedName: currentFileHandle
                ? currentFileHandle.name
                : "untitled.txt",
              types: [
                {
                  description: "Text Files",
                  accept: { "text/plain": [".txt"] },
                },
              ],
            });
            currentFileHandle = fileHandleToSave; // Cập nhật handle mới
          }
        } else {
          // Hiển thị hộp thoại "Save As"
          fileHandleToSave = await window.showSaveFilePicker({
            suggestedName: "my-document.txt",
            types: [
              { description: "Text Files", accept: { "text/plain": [".txt"] } },
            ],
          });
          currentFileHandle = fileHandleToSave; // Lưu handle mới
        }

        // Tạo một stream có thể ghi
        const writableStream = await fileHandleToSave.createWritable();
        // Ghi nội dung
        await writableStream.write(textToSave);
        // Đóng file và ghi thay đổi ra đĩa
        await writableStream.close();

        fsStatus.textContent = `Đã lưu file: ${fileHandleToSave.name}`;
        console.log(`File "${fileHandleToSave.name}" saved successfully.`);
      } catch (err) {
        if (err.name === "AbortError") {
          fsStatus.textContent = "Người dùng đã hủy lưu file.";
        } else {
          fsStatus.textContent = `Lỗi lưu file: ${err.message}`;
          console.error("Lỗi lưu file:", err);
        }
      }
    }

    // Hàm kiểm tra và yêu cầu quyền (ví dụ cho FileSystemHandle)
    async function verifyPermission(fileHandle, withWrite = false) {
      const opts = {};
      if (withWrite) {
        opts.mode = "readwrite";
      }
      // Kiểm tra xem đã có quyền chưa
      if ((await fileHandle.queryPermission(opts)) === "granted") {
        return true;
      }
      // Nếu chưa, yêu cầu quyền
      if ((await fileHandle.requestPermission(opts)) === "granted") {
        return true;
      }
      return false;
    }
  </script>
  ```

  API này rất mạnh nhưng cũng phức tạp và cần xử lý quyền cẩn thận.

**4. Clipboard API (Asynchronous):**

API hiện đại để tương tác với clipboard hệ thống (copy/paste). Nó an toàn hơn và linh hoạt hơn `document.execCommand('copy'/'paste')` (cũ).

- **Yêu cầu HTTPS (trừ localhost).**
- **Cần quyền (`'clipboard-read'`, `'clipboard-write'`) thường được cấp tự động khi trang đang active và có tương tác người dùng, nhưng có thể cần hỏi tường minh trong một số trường hợp (đặc biệt là đọc clipboard).**

- **`navigator.clipboard.writeText(text)`:** Ghi text vào clipboard. Trả về Promise.

  ```javascript
  async function copyToClipboard(text) {
    if (!navigator.clipboard) {
      alert("Clipboard API không được hỗ trợ.");
      // Fallback to execCommand (ít an toàn hơn, có thể không hoạt động trong mọi ngữ cảnh)
      // try {
      //   const textArea = document.createElement("textarea");
      //   textArea.value = text;
      //   document.body.appendChild(textArea);
      //   textArea.focus(); textArea.select();
      //   document.execCommand('copy');
      //   document.body.removeChild(textArea);
      //   console.log('Fallback: Text copied to clipboard');
      // } catch (err) { console.error('Fallback copy failed:', err); }
      return;
    }
    try {
      await navigator.clipboard.writeText(text);
      console.log("Text copied to clipboard:", text);
      // alert('Đã sao chép vào clipboard!');
    } catch (err) {
      console.error("Không thể sao chép text: ", err);
      // alert('Lỗi khi sao chép!');
    }
  }
  // <button onclick="copyToClipboard('Xin chào từ JavaScript!')">Copy "Xin chào..."</button>
  ```

- **`navigator.clipboard.readText()`:** Đọc text từ clipboard. Trả về Promise. Thường yêu cầu trang phải đang active và có thể cần sự cho phép rõ ràng hơn.

  ```javascript
  async function pasteFromClipboard() {
    if (!navigator.clipboard || !navigator.clipboard.readText) {
      alert("Đọc clipboard không được hỗ trợ.");
      return null;
    }
    try {
      const text = await navigator.clipboard.readText();
      console.log("Text pasted from clipboard:", text);
      // document.getElementById('pastedText').value = text;
      return text;
    } catch (err) {
      console.error("Không thể đọc từ clipboard: ", err);
      // alert('Lỗi khi đọc clipboard! Bạn đã cấp quyền chưa?');
      // Có thể do người dùng chưa cấp quyền 'clipboard-read'
      // Hoặc trang không active, hoặc không phải ngữ cảnh an toàn.
      return null;
    }
  }
  // <button onclick="pasteFromClipboard().then(text => { if(text) document.getElementById('pastedText').value = text; })">Paste</button>
  // <textarea id="pastedText" placeholder="Nội dung paste sẽ ở đây"></textarea>
  ```

- **`navigator.clipboard.write(data)` và `navigator.clipboard.read()`:**
  Cho phép copy/paste các loại dữ liệu phức tạp hơn (ví dụ: ảnh) sử dụng đối tượng `ClipboardItem`.

  ```javascript
  // Ví dụ: Copy ảnh (cần Blob của ảnh)
  // async function copyImageToClipboard(imageUrl) {
  //   try {
  //     const response = await fetch(imageUrl);
  //     const blob = await response.blob(); // Lấy Blob của ảnh
  //     await navigator.clipboard.write([
  //       new ClipboardItem({
  //         [blob.type]: blob // Ví dụ: { 'image/png': blob }
  //       })
  //     ]);
  //     console.log('Image copied to clipboard.');
  //   } catch (err) {
  //     console.error('Failed to copy image: ', err);
  //   }
  // }

  // Ví dụ: Đọc dữ liệu từ clipboard (có thể là ảnh)
  // async function pasteAndDisplayImage() {
  //   try {
  //     const clipboardItems = await navigator.clipboard.read();
  //     for (const item of clipboardItems) {
  //       for (const type of item.types) {
  //         if (type.startsWith("image/")) {
  //           const blob = await item.getType(type);
  //           const imgURL = URL.createObjectURL(blob);
  //           document.getElementById('pastedImagePreview').src = imgURL;
  //           // URL.revokeObjectURL(imgURL) sau khi dùng xong
  //           console.log('Image pasted and displayed.');
  //           return;
  //         }
  //       }
  //     }
  //     console.log("No image found on clipboard.");
  //   } catch (err) {
  //     console.error('Failed to read from clipboard: ', err);
  //   }
  // }
  // <img id="pastedImagePreview" style="max-width:300px;">
  // <button onclick="pasteAndDisplayImage()">Paste Image</button>
  ```

Các API này mở rộng đáng kể khả năng tương tác của ứng dụng web với môi trường của người dùng, từ việc xử lý file đơn giản đến việc đọc/ghi trực tiếp vào hệ thống file và tương tác với clipboard. Luôn ưu tiên trải nghiệm người dùng và xử lý quyền một cách cẩn thận.

**Phần 12: Notifications, Vibration và Fullscreen API**

Các API này cho phép ứng dụng web gửi thông báo cho người dùng (ngay cả khi tab không active), sử dụng tính năng rung của thiết bị, và chuyển đổi hiển thị sang chế độ toàn màn hình, nâng cao trải nghiệm người dùng và khả năng tương tác.

**1. Notifications API: Gửi Thông Báo Desktop**

Notifications API cho phép ứng dụng web hiển thị thông báo cho người dùng, tương tự như các thông báo từ ứng dụng desktop hoặc mobile.

- **Yêu cầu quyền:**

  - Cần sự cho phép rõ ràng từ người dùng.
  - Chỉ hoạt động trên ngữ cảnh an toàn (HTTPS, trừ localhost).
  - Quyền có thể là `'granted'`, `'denied'`, hoặc `'default'` (người dùng chưa quyết định, tương đương denied).

  ```javascript
  function checkNotificationPermission() {
    if (!("Notification" in window)) {
      alert("Trình duyệt này không hỗ trợ thông báo desktop.");
      return "unsupported";
    } else if (Notification.permission === "granted") {
      console.log("Quyền thông báo đã được cấp.");
      return "granted";
    } else if (Notification.permission !== "denied") {
      // 'default' hoặc chưa hỏi
      return "default";
    } else {
      // 'denied'
      console.log("Quyền thông báo đã bị từ chối.");
      return "denied";
    }
  }

  async function requestNotificationPermission() {
    const permissionStatus = checkNotificationPermission();
    if (
      permissionStatus === "granted" ||
      permissionStatus === "unsupported" ||
      permissionStatus === "denied"
    ) {
      if (permissionStatus === "denied")
        alert(
          "Bạn đã từ chối quyền thông báo. Vui lòng thay đổi trong cài đặt trình duyệt."
        );
      return permissionStatus;
    }

    // Notification.requestPermission() trả về một Promise
    const permission = await Notification.requestPermission();
    if (permission === "granted") {
      console.log("Quyền thông báo được cấp sau khi hỏi.");
    } else {
      console.log("Người dùng không cấp quyền thông báo.");
    }
    return permission;
  }
  ```

- **Tạo và Hiển Thị Thông Báo:**

  ```javascript
  async function showSimpleNotification() {
    const permission = await requestNotificationPermission();
    if (permission !== "granted") {
      alert("Không thể hiển thị thông báo do chưa được cấp quyền.");
      return;
    }

    const title = "Thông Báo Đơn Giản!";
    const options = {
      body: "Đây là nội dung của thông báo đơn giản từ JavaScript.",
      icon: "https://via.placeholder.com/64/00FF00/FFFFFF?Text=Icon", // URL đến icon
      // image: "https://via.placeholder.com/300x100/0000FF/FFFFFF?Text=Image", // Ảnh lớn trong thông báo (không phải trình duyệt nào cũng hỗ trợ)
      badge: "https://via.placeholder.com/16/FF0000/FFFFFF?Text=B", // Icon nhỏ (ví dụ trên Android)
      tag: "simple-notification-tag", // ID cho thông báo, nếu gửi thông báo mới với cùng tag, nó sẽ thay thế cái cũ
      renotify: false, // Nếu true và tag đã tồn tại, sẽ thông báo lại (rung/âm thanh) dù nội dung không đổi (mặc định false)
      requireInteraction: false, // Nếu true, thông báo sẽ không tự đóng cho đến khi người dùng tương tác (mặc định false)
      silent: false, // Nếu true, không có âm thanh hoặc rung (mặc định false)
      // timestamp: Date.now() + 5000, // Thời gian hiển thị thông báo (ít dùng)
      // data: { someData: 'Đây là dữ liệu kèm theo', url: '/some-page' }, // Dữ liệu tùy chỉnh, có thể truy cập trong event handler
      // actions: [ // Các nút hành động (không phải trình duyệt nào cũng hỗ trợ tốt)
      //   { action: 'explore', title: 'Khám phá', icon: 'path/to/explore-icon.png' },
      //   { action: 'close', title: 'Đóng', icon: 'path/to/close-icon.png' }
      // ]
    };

    try {
      // Cách 1: Trực tiếp tạo Notification (nếu dùng trong Service Worker, phải dùng registration.showNotification)
      const notification = new Notification(title, options);

      // Event handlers cho đối tượng Notification
      notification.onclick = function (event) {
        console.log("Thông báo được click!", event);
        // Ví dụ: Mở một cửa sổ mới hoặc focus vào tab hiện tại
        // window.open(notification.data.url || 'https://example.com');
        parent.focus(); // Focus vào tab đã tạo thông báo
        window.focus(); // Thử focus vào cửa sổ hiện tại
        this.close(); // Đóng thông báo sau khi click
      };

      notification.onshow = function (event) {
        console.log("Thông báo đã hiển thị.", event);
        // Tự động đóng sau 5 giây (ví dụ)
        // setTimeout(() => this.close(), 5000);
      };

      notification.onclose = function (event) {
        console.log("Thông báo đã đóng.", event);
      };

      notification.onerror = function (event) {
        console.error("Lỗi hiển thị thông báo:", event);
      };
    } catch (err) {
      console.error("Lỗi khi tạo thông báo:", err);
      alert("Lỗi: " + err.message);
    }
  }

  // <button onclick="requestNotificationPermission()">Yêu Cầu Quyền Thông Báo</button>
  // <button onclick="showSimpleNotification()">Hiển Thị Thông Báo</button>
  ```

  **Các `options` quan trọng cho `new Notification(title, options)`:**

  - `body`: Nội dung chính của thông báo.
  - `icon`: URL đến một ảnh nhỏ hiển thị làm icon.
  - `image`: URL đến một ảnh lớn hơn hiển thị trong nội dung thông báo.
  - `badge`: Một icon nhỏ hơn thường dùng trên các nền tảng mobile.
  - `tag`: Một chuỗi định danh. Nếu một thông báo mới được tạo với `tag` đã tồn tại, thông báo cũ sẽ được thay thế. Hữu ích để tránh spam thông báo giống nhau.
  - `renotify`: (Boolean) Nếu `true` và `tag` đã tồn tại, người dùng sẽ được thông báo lại (ví dụ: rung, âm thanh) ngay cả khi nội dung không đổi. Mặc định là `false`.
  - `requireInteraction`: (Boolean) Nếu `true`, thông báo sẽ không tự động đóng cho đến khi người dùng tương tác với nó (click hoặc đóng). Mặc định là `false`.
  - `silent`: (Boolean) Nếu `true`, không có âm thanh hoặc rung khi thông báo hiển thị.
  - `data`: Một đối tượng chứa dữ liệu tùy chỉnh bạn muốn liên kết với thông báo. Dữ liệu này có thể được truy cập trong các event handler của thông báo (ví dụ: `notification.data`).
  - `actions`: (Mảng các đối tượng) Định nghĩa các nút hành động có thể hiển thị trên thông báo. Mỗi action có `action` (ID), `title`, và `icon` (URL). Hỗ trợ có thể khác nhau giữa các trình duyệt.

- **Sử dụng với Service Workers:**
  Để gửi thông báo khi trang web không active hoặc đã đóng (ví dụ: push notifications), bạn cần sử dụng Service Worker.
  Trong Service Worker, bạn dùng `self.registration.showNotification(title, options)`. Phương thức này cũng trả về một Promise.

  ```javascript
  // Trong service-worker.js
  // self.addEventListener('push', function(event) {
  //   const data = event.data ? event.data.json() : { title: 'Push Thông Báo', body: 'Có tin mới!', icon: '/icon.png' };
  //   event.waitUntil(
  //     self.registration.showNotification(data.title, {
  //       body: data.body,
  //       icon: data.icon,
  //       tag: data.tag || 'default-push-tag',
  //       data: data.urlToOpen // Ví dụ: để mở URL khi click
  //     })
  //   );
  // });

  // self.addEventListener('notificationclick', function(event) {
  //   console.log('Notification clicked in SW:', event.notification.tag, event.action);
  //   event.notification.close(); // Đóng thông báo

  //   const urlToOpen = event.notification.data || '/'; // Lấy URL từ data hoặc mặc định

  //   event.waitUntil(
  //     clients.matchAll({ type: 'window', includeUncontrolled: true }).then(windowClients => {
  //       // Kiểm tra xem có tab nào của trang web đang mở không
  //       for (let i = 0; i < windowClients.length; i++) {
  //         const client = windowClients[i];
  //         if (client.url === urlToOpen && 'focus' in client) { // Giả sử urlToOpen là trang chính
  //           return client.focus();
  //         }
  //       }
  //       // Nếu không có tab nào mở, hoặc URL khác, mở tab mới
  //       if (clients.openWindow) {
  //         return clients.openWindow(urlToOpen);
  //       }
  //     })
  //   );
  // });
  ```

**2. Vibration API: Sử Dụng Tính Năng Rung của Thiết Bị**

Vibration API cho phép ứng dụng web làm rung thiết bị (thường là điện thoại di động).

- **Kiểm tra hỗ trợ:**

  ```javascript
  if ("vibrate" in navigator) {
    console.log("Vibration API is supported.");
  } else {
    console.log("Vibration API is NOT supported.");
  }
  ```

- **Cách sử dụng `navigator.vibrate(pattern)`:**

  - `pattern`:
    - Một số nguyên: Rung `pattern` mili giây. `navigator.vibrate(200);` // Rung 200ms
    - Một mảng các số nguyên: Mô tả một chuỗi rung và tạm dừng. Phần tử chẵn là thời gian rung, phần tử lẻ là thời gian tạm dừng.
      `navigator.vibrate([100, 50, 200, 50, 100]);` // Rung 100ms, nghỉ 50ms, rung 200ms, nghỉ 50ms, rung 100ms.
  - Gọi `navigator.vibrate(0)` hoặc `navigator.vibrate([])` sẽ hủy bất kỳ rung nào đang diễn ra.
  - API này là đồng bộ, nhưng việc rung là bất đồng bộ. Lệnh gọi trả về ngay lập tức.

  ```html
  <button onclick="simpleVibrate()">Rung Đơn (200ms)</button>
  <button onclick="patternVibrate()">Rung Theo Mẫu</button>
  <button onclick="stopVibrate()">Dừng Rung</button>
  <div id="vibrationStatus"></div>
  <script>
    const vibrationStatus = document.getElementById("vibrationStatus");

    function simpleVibrate() {
      if ("vibrate" in navigator) {
        if (navigator.vibrate(200)) {
          vibrationStatus.textContent = "Đang rung đơn...";
          console.log("Vibrating for 200ms");
        } else {
          vibrationStatus.textContent =
            "Thiết bị không hỗ trợ rung hoặc bị chặn.";
          console.log("Vibration failed or not supported by device.");
        }
      } else {
        vibrationStatus.textContent = "Vibration API không được hỗ trợ.";
      }
    }

    function patternVibrate() {
      if ("vibrate" in navigator) {
        const pattern = [100, 100, 100, 100, 300, 100, 100, 100, 100]; // Ví dụ: SOS
        if (navigator.vibrate(pattern)) {
          vibrationStatus.textContent = "Đang rung theo mẫu...";
          console.log("Vibrating with pattern:", pattern);
        } else {
          vibrationStatus.textContent =
            "Thiết bị không hỗ trợ rung theo mẫu hoặc bị chặn.";
        }
      } else {
        vibrationStatus.textContent = "Vibration API không được hỗ trợ.";
      }
    }

    function stopVibrate() {
      if ("vibrate" in navigator) {
        navigator.vibrate(0); // hoặc navigator.vibrate([])
        vibrationStatus.textContent = "Đã dừng rung.";
        console.log("Vibration stopped.");
      } else {
        vibrationStatus.textContent = "Vibration API không được hỗ trợ.";
      }
    }
  </script>
  ```

- **Lưu ý:**
  - Hành vi của Vibration API có thể bị ảnh hưởng bởi cài đặt của người dùng trên thiết bị (ví dụ: chế độ im lặng, tiết kiệm pin).
  - Nên sử dụng một cách có chừng mực để không làm phiền người dùng.
  - Không có cách nào để kiểm tra xem thiết bị có thực sự rung hay không, chỉ có thể kiểm tra hỗ trợ API.

**3. Fullscreen API: Hiển Thị Toàn Màn Hình**

Fullscreen API cho phép một phần tử cụ thể hoặc toàn bộ trang web được hiển thị ở chế độ toàn màn hình.

- **Yêu cầu HTTPS.**
- **Cần được kích hoạt bởi một hành động của người dùng (ví dụ: click nút).** Trình duyệt thường không cho phép tự động vào fullscreen khi tải trang.

- **Các Phương Thức và Thuộc Tính:**

  - **`Element.requestFullscreen([options])`**: Yêu cầu hiển thị `Element` ở chế độ toàn màn hình. Trả về một Promise.
    - `options.navigationUI`: `'auto'` (mặc định), `'show'`, `'hide'`. Kiểm soát việc hiển thị UI điều hướng của trình duyệt (ví dụ: thanh địa chỉ). `'hide'` có thể không được hỗ trợ trên mọi trình duyệt/OS.
  - **`document.exitFullscreen()`**: Thoát khỏi chế độ toàn màn hình. Trả về một Promise.
  - **`document.fullscreenElement`**: Trả về phần tử đang ở chế độ toàn màn hình, hoặc `null` nếu không có. (Read-only)
  - **`document.fullscreenEnabled`**: Boolean, cho biết chế độ toàn màn hình có được phép hay không. (Read-only)

- **Sự Kiện:**

  - `fullscreenchange`: Kích hoạt trên `document` khi trạng thái toàn màn hình thay đổi (vào hoặc ra).
  - `fullscreenerror`: Kích hoạt trên `document` nếu có lỗi khi cố gắng vào/ra chế độ toàn màn hình.

- **Ví dụ:**

  ```html
  <style>
    #fullscreenTarget {
      background-color: lightcoral;
      padding: 20px;
      border: 2px solid darkred;
    }
    /* Style khi một phần tử con của #fullscreenTarget đang fullscreen */
    /* #fullscreenTarget:fullscreen video { width: 100%; height: 100%; } */
    /* Hoặc dùng pseudo-class :fullscreen trên chính phần tử đó */
    #fullscreenTarget:-webkit-full-screen {
      background: lightblue;
    }
    #fullscreenTarget:-moz-full-screen {
      background: lightblue;
    }
    #fullscreenTarget:-ms-fullscreen {
      background: lightblue;
    }
    #fullscreenTarget:fullscreen {
      background: lightblue;
    } /* Chuẩn */
  </style>
  <div id="fullscreenTarget">
    <h2>Nội dung trong Div</h2>
    <p>Đây là một div có thể vào fullscreen.</p>
    <video
      id="mySampleVideo"
      src="video.mp4"
      width="320"
      height="180"
      controls
    ></video
    ><br />
    <button
      onclick="toggleElemFullscreen(document.getElementById('fullscreenTarget'))"
    >
      Toggle Fullscreen Div
    </button>
    <button
      onclick="toggleElemFullscreen(document.getElementById('mySampleVideo'))"
    >
      Toggle Fullscreen Video
    </button>
  </div>
  <button onclick="togglePageFullscreen()">Toggle Fullscreen Page</button>
  <div id="fsStatus">Trạng thái Fullscreen: Bình thường</div>

  <script>
    const fsStatusDisplay = document.getElementById("fsStatus");

    function isFullscreen() {
      return !!(
        document.fullscreenElement ||
        document.webkitFullscreenElement ||
        document.mozFullScreenElement ||
        document.msFullscreenElement
      );
    }

    function getFullscreenElement() {
      return (
        document.fullscreenElement ||
        document.webkitFullscreenElement ||
        document.mozFullScreenElement ||
        document.msFullscreenElement
      );
    }

    async function toggleElemFullscreen(elem) {
      if (
        !document.fullscreenEnabled &&
        !(
          document.webkitFullscreenEnabled ||
          document.mozFullScreenEnabled ||
          document.msFullscreenEnabled
        )
      ) {
        fsStatusDisplay.textContent = "Fullscreen không được phép.";
        return;
      }

      if (!isFullscreen()) {
        // Nếu chưa fullscreen, thì vào fullscreen
        try {
          if (elem.requestFullscreen) {
            await elem.requestFullscreen({ navigationUI: "hide" });
          } else if (elem.webkitRequestFullscreen) {
            /* Safari, Chrome */
            await elem.webkitRequestFullscreen();
          } else if (elem.mozRequestFullScreen) {
            /* Firefox */
            await elem.mozRequestFullScreen();
          } else if (elem.msRequestFullscreen) {
            /* IE/Edge */
            await elem.msRequestFullscreen();
          }
          console.log("Vào fullscreen cho:", elem.id || elem.tagName);
        } catch (err) {
          console.error("Lỗi khi vào fullscreen:", err);
          fsStatusDisplay.textContent = `Lỗi Fullscreen: ${err.message}`;
        }
      } else {
        // Nếu đang fullscreen, thì thoát
        if (
          getFullscreenElement() === elem ||
          getFullscreenElement()?.contains(elem)
        ) {
          // Chỉ thoát nếu element này hoặc cha của nó đang fullscreen
          try {
            if (document.exitFullscreen) {
              await document.exitFullscreen();
            } else if (document.webkitExitFullscreen) {
              await document.webkitExitFullscreen();
            } else if (document.mozCancelFullScreen) {
              await document.mozCancelFullScreen();
            } else if (document.msExitFullscreen) {
              await document.msExitFullscreen();
            }
            console.log("Đã thoát fullscreen.");
          } catch (err) {
            console.error("Lỗi khi thoát fullscreen:", err);
            fsStatusDisplay.textContent = `Lỗi thoát Fullscreen: ${err.message}`;
          }
        } else {
          console.log("Một phần tử khác đang fullscreen, không thoát.");
          // Hoặc bạn có thể muốn thoát fullscreen bất kể phần tử nào
          // await document.exitFullscreen(); (cần kiểm tra method)
        }
      }
    }

    // Hàm tiện ích để vào fullscreen cho toàn bộ trang (document.documentElement)
    function togglePageFullscreen() {
      toggleElemFullscreen(document.documentElement);
    }

    // Lắng nghe sự kiện thay đổi fullscreen
    document.addEventListener("fullscreenchange", handleFullscreenChange);
    document.addEventListener("webkitfullscreenchange", handleFullscreenChange); // Safari, Chrome
    document.addEventListener("mozfullscreenchange", handleFullscreenChange); // Firefox
    document.addEventListener("MSFullscreenChange", handleFullscreenChange); // IE/Edge

    function handleFullscreenChange() {
      if (isFullscreen()) {
        const currentFsElement = getFullscreenElement();
        fsStatusDisplay.textContent = `Trạng thái Fullscreen: Đang hiển thị ${
          currentFsElement.id || currentFsElement.tagName
        } toàn màn hình.`;
        console.log("Đã vào chế độ fullscreen. Element:", currentFsElement);
      } else {
        fsStatusDisplay.textContent = "Trạng thái Fullscreen: Bình thường.";
        console.log("Đã thoát chế độ fullscreen.");
      }
    }
    document.addEventListener("fullscreenerror", (event) => {
      console.error("Fullscreen error event:", event);
      fsStatusDisplay.textContent = "Lỗi khi chuyển đổi Fullscreen.";
    });
  </script>
  ```

- **Tiền tố trình duyệt (Vendor Prefixes):** Fullscreen API đã từng có nhiều tiền tố khác nhau (`webkit`, `moz`, `ms`). Mặc dù phiên bản chuẩn (`fullscreenElement`, `requestFullscreen`, `exitFullscreen`) đã được hỗ trợ rộng rãi, việc kiểm tra các phiên bản có tiền tố vẫn là một thói quen tốt để đảm bảo tương thích ngược tối đa, như trong ví dụ trên.
- **Styling trong Fullscreen:**
  Sử dụng pseudo-class `:fullscreen` (và các phiên bản có tiền tố của nó) để áp dụng style đặc biệt cho phần tử khi nó (hoặc một phần tử con của nó) đang ở chế độ toàn màn hình.

Các API này, khi được sử dụng hợp lý, có thể làm tăng đáng kể sự tương tác và tiện ích của ứng dụng web của bạn. Hãy nhớ luôn kiểm tra sự hỗ trợ của trình duyệt và xử lý các yêu cầu quyền một cách thân thiện với người dùng.

**Phần 13: Web Workers và Shared Workers: Chạy JavaScript ở Luồng Riêng**

JavaScript trong trình duyệt theo truyền thống là đơn luồng (single-threaded). Điều này có nghĩa là các tác vụ JavaScript phức tạp, tốn thời gian (ví dụ: tính toán nặng, xử lý dữ liệu lớn, phân tích hình ảnh) có thể chặn luồng chính, làm cho giao diện người dùng (UI) bị "đóng băng", không phản hồi lại tương tác của người dùng. Web Workers giải quyết vấn đề này bằng cách cho phép bạn chạy các script JavaScript trong các luồng nền (background threads), tách biệt khỏi luồng chính.

**1. Tại Sao Cần Web Workers?**

- **Không chặn luồng UI:** Các tác vụ nặng được thực hiện trong worker không làm chậm hoặc đóng băng UI. Người dùng vẫn có thể tương tác với trang web một cách mượt mà.
- **Tận dụng đa lõi CPU:** Các trình duyệt hiện đại có thể chạy các worker trên các lõi CPU khác nhau, cho phép xử lý song song thực sự.
- **Cải thiện hiệu suất:** Cho các ứng dụng web cần xử lý tính toán phức tạp, phân tích dữ liệu, mã hóa/giải mã, render đồ họa, v.v.

**2. Dedicated Web Workers:**

Một Dedicated Worker (thường gọi tắt là Web Worker) được sở hữu bởi một script cụ thể đã tạo ra nó. Chỉ script đó mới có thể giao tiếp với worker này.

- **Tạo Worker:**

  - Tạo một file JavaScript riêng cho mã của worker (ví dụ: `my_worker.js`).
  - Trong script chính, tạo một instance của `Worker`:

    ```javascript
    // main_script.js
    if (window.Worker) {
      // Kiểm tra trình duyệt có hỗ trợ Worker không
      const myWorker = new Worker("my_worker.js"); // Đường dẫn đến file worker

      // Gửi dữ liệu đến worker
      myWorker.postMessage({
        command: "startCalculation",
        data: [1, 2, 3, 4, 5 /* ... nhiều số ... */],
      });
      myWorker.postMessage("Hello Worker!"); // Có thể gửi chuỗi, số, object, array, ArrayBuffer (transferable)

      // Nhận dữ liệu từ worker
      myWorker.onmessage = function (event) {
        console.log("Message received from worker:", event.data);
        // event.data chứa dữ liệu worker gửi về
        // Ví dụ: document.getElementById('result').textContent = `Kết quả: ${event.data.result}`;
        if (event.data.status === "completed") {
          console.log("Worker has finished its task.");
        }
      };

      // Xử lý lỗi từ worker
      myWorker.onerror = function (error) {
        console.error(
          "Error from worker:",
          error.message,
          "at",
          error.filename,
          "line",
          error.lineno
        );
        // error.message, error.filename, error.lineno
      };

      // Chấm dứt worker từ script chính (nếu cần)
      // setTimeout(() => {
      //   myWorker.terminate();
      //   console.log('Worker terminated by main script.');
      // }, 10000);
    } else {
      console.log("Your browser doesn't support Web Workers.");
      // Cung cấp giải pháp thay thế hoặc thông báo cho người dùng
    }
    ```

- **Mã Bên Trong Worker (`my_worker.js`):**

  - Worker chạy trong một global context riêng (không phải `window`). `self` tham chiếu đến global context của worker (tương tự `window` trong luồng chính).
  - **Không thể truy cập trực tiếp DOM:** Workers không thể thao tác DOM hoặc truy cập các đối tượng của luồng chính như `window`, `document`.
  - **Có thể sử dụng nhiều API của trình duyệt:** `XMLHttpRequest`, `fetch`, `WebSocket`, `IndexedDB`, `localStorage`, `setTimeout`, `setInterval`, `console`, `navigator` (một số thuộc tính).
  - Giao tiếp với luồng chính thông qua `postMessage()` và `onmessage`.

  ```javascript
  // my_worker.js
  console.log("Worker script started."); // Sẽ log trong console của worker (nếu devtools hỗ trợ)

  self.onmessage = function (event) {
    console.log("Message received in worker:", event.data);
    const receivedData = event.data;

    if (
      typeof receivedData === "object" &&
      receivedData.command === "startCalculation"
    ) {
      const numbers = receivedData.data;
      let sum = 0;
      for (let i = 0; i < numbers.length; i++) {
        sum += numbers[i];
        // Mô phỏng công việc nặng
        // for (let j = 0; j < 100000; j++); // Không nên dùng busy loop thế này trong thực tế
      }
      // Gửi kết quả về luồng chính
      self.postMessage({ result: sum, status: "completed" });
    } else if (typeof receivedData === "string") {
      const reply = `Worker received: "${receivedData}". Replying now!`;
      self.postMessage(reply);
    } else {
      self.postMessage({ error: "Unknown command or data type" });
    }

    // Worker tự chấm dứt sau khi hoàn thành (tùy chọn)
    // if (receivedData.command === 'startCalculation') {
    //    self.close(); // Tự đóng worker
    //    console.log('Worker closed itself.');
    // }
  };

  // Xử lý lỗi bên trong worker (ví dụ: lỗi không bắt được)
  // self.onerror = function(errorEvent) {
  //   // errorEvent là một ErrorEvent
  //   console.error('Unhandled error in worker:', errorEvent.message);
  //   // Bạn không thể ngăn lỗi này lan truyền ra ngoài (vẫn kích hoạt myWorker.onerror ở luồng chính)
  //   // nhưng có thể log hoặc dọn dẹp gì đó ở đây.
  // };

  // Worker có thể import script khác bằng importScripts() (đồng bộ)
  // importScripts('helper_functions.js', 'another_lib.js');
  // console.log(helperFunctionFromScript()); // Nếu helper_functions.js có hàm này

  // Hoặc sử dụng ES Modules nếu worker được tạo với type: 'module'
  // // main_script.js: const myModuleWorker = new Worker('my_module_worker.js', { type: 'module' });
  // // my_module_worker.js:
  // // import { someUtility } from './utils.js';
  // // self.onmessage = (event) => { /* ... use someUtility ... */ };
  ```

- **`postMessage(data, [transferables])`:**

  - `data`: Dữ liệu gửi đi. Nó được sao chép bằng thuật toán "structured clone" (có thể sao chép hầu hết các kiểu dữ liệu phức tạp, bao gồm cả `File`, `Blob`, `ImageData`, `Map`, `Set`, nhưng không phải `Function` hay `Error` object).
  - `transferables`: Một mảng các đối tượng "transferable" (ví dụ: `ArrayBuffer`, `MessagePort`, `ImageBitmap`, `OffscreenCanvas`). Khi một đối tượng được transfer, quyền sở hữu của nó được chuyển sang phía nhận, và nó không còn có thể truy cập được ở phía gửi nữa. Điều này giúp truyền dữ liệu lớn (như `ArrayBuffer`) rất nhanh vì không cần sao chép.

  ```javascript
  // Gửi ArrayBuffer (ví dụ: dữ liệu ảnh)
  // main_script.js
  // const largeBuffer = new ArrayBuffer(1024 * 1024 * 10); // 10MB
  // myWorker.postMessage({ buffer: largeBuffer, command: 'processBuffer' }, [largeBuffer]);
  // console.log(largeBuffer.byteLength); // Sẽ là 0 ở đây vì đã transfer

  // my_worker.js
  // self.onmessage = function(event) {
  //   if (event.data.command === 'processBuffer') {
  //     const receivedBuffer = event.data.buffer;
  //     console.log('Worker received buffer size:', receivedBuffer.byteLength); // 10MB
  //     // Xử lý buffer...
  //     // Gửi lại (có thể transfer nếu cần)
  //     self.postMessage({ processedBuffer: receivedBuffer, status: 'done' }, [receivedBuffer]);
  //   }
  // };
  ```

- **Chấm dứt Worker:**
  - Từ luồng chính: `worker.terminate()` (chấm dứt ngay lập tức, không có cơ hội dọn dẹp).
  - Từ bên trong worker: `self.close()`.

**3. Shared Web Workers:**

Một Shared Worker có thể được truy cập bởi nhiều script khác nhau (ví dụ: từ các tab, iframe, hoặc thậm chí các worker khác) miễn là chúng cùng origin. Tất cả các script kết nối đến cùng một Shared Worker (với cùng URL) sẽ chia sẻ cùng một instance của worker đó.

- **Tạo và Kết Nối (`main_script_A.js`, `main_script_B.js`):**

  ```javascript
  // main_script_A.js (hoặc B)
  if (window.SharedWorker) {
    const mySharedWorker = new SharedWorker("my_shared_worker.js", {
      name: "myUniqueSharedWorkerName",
    });
    // 'name' là tùy chọn, giúp debug và có thể dùng để phân biệt nếu có nhiều shared worker cùng URL

    // Port là kênh giao tiếp chính với Shared Worker
    const port = mySharedWorker.port;

    // Bắt đầu kênh giao tiếp (quan trọng!)
    port.start(); // Hoặc mySharedWorker.port.onmessage = ... sẽ tự động gọi start()

    // Gửi tin nhắn đến Shared Worker
    port.postMessage({ clientId: "ScriptA", message: "Hello from Script A!" });

    // Nhận tin nhắn từ Shared Worker
    port.onmessage = function (event) {
      console.log(`Script A received from SharedWorker:`, event.data);
      // document.getElementById('shared-worker-log-A').textContent += JSON.stringify(event.data) + '\n';
    };

    // Xử lý lỗi
    // mySharedWorker.onerror = function(error) { // Lỗi khi tạo worker
    //   console.error('SharedWorker construction error:', error);
    // };
    // port.onmessageerror = function(error) { // Lỗi khi deserializing message
    //   console.error('Error in message to/from SharedWorker port:', error);
    // }

    // Khi trang bị đóng, port sẽ tự động đóng.
    // window.addEventListener('beforeunload', () => {
    //    port.postMessage({ clientId: 'ScriptA', status: 'disconnecting' });
    //    port.close(); // Có thể không cần thiết, trình duyệt tự xử lý
    // });
  } else {
    console.log("Your browser doesn't support Shared Workers.");
  }
  ```

  Mỗi script kết nối đến Shared Worker sẽ nhận được một đối tượng `MessagePort` riêng (`mySharedWorker.port`) để giao tiếp.

- **Mã Bên Trong Shared Worker (`my_shared_worker.js`):**

  - Sự kiện `connect` được kích hoạt mỗi khi một script mới kết nối.
  - `event.ports[0]` trong handler `onconnect` là `MessagePort` cho kết nối đó.

  ```javascript
  // my_shared_worker.js
  console.log("SharedWorker script started.");
  const connectedPorts = []; // Mảng để lưu các port đang kết nối

  self.onconnect = function (event) {
    const port = event.ports[0]; // Lấy port của kết nối mới
    connectedPorts.push(port);
    console.log(
      `New connection to SharedWorker. Total connections: ${connectedPorts.length}`
    );

    port.onmessage = function (e) {
      const messageData = e.data;
      console.log("SharedWorker received:", messageData);

      // Ví dụ: Broadcast tin nhắn đến tất cả các client khác (trừ người gửi)
      // Hoặc xử lý logic chung và gửi lại cho client cụ thể
      const response = {
        originalSender: messageData.clientId,
        sharedWorkerReply: `Processed: "${messageData.message}"`,
        timestamp: new Date().toLocaleTimeString(),
      };

      // Gửi lại cho client đã gửi tin nhắn
      // port.postMessage({ type: 'ack', originalMessage: messageData.message });

      // Broadcast cho tất cả các port đang kết nối
      connectedPorts.forEach((p) => {
        // if (p !== port) { // Không gửi lại cho người gửi nếu logic yêu cầu
        try {
          p.postMessage(response);
        } catch (err) {
          console.error(
            "Error posting message to a port (port might be closed):",
            err
          );
          // Xóa port bị lỗi khỏi danh sách nếu cần
          const index = connectedPorts.indexOf(p);
          if (index > -1) {
            connectedPorts.splice(index, 1);
          }
        }
        // }
      });

      if (messageData.command === "GET_SHARED_DATA") {
        // Logic để lấy dữ liệu chung
        // port.postMessage({ type: 'sharedData', data: someSharedDataObject });
      }
    };

    // Bắt đầu kênh trên port này (không cần thiết nếu đã gán onmessage)
    // port.start();

    // Gửi tin nhắn chào mừng cho client vừa kết nối
    port.postMessage({
      type: "connection_established",
      workerName: self.name,
      message: "Welcome to the SharedWorker!",
    });

    // Không có self.close() trực tiếp cho SharedWorker vì nó được thiết kế để tồn tại
    // miễn là có ít nhất một client kết nối. Nó sẽ tự động dừng khi không còn client nào.
  };

  // SharedWorker cũng có thể import script khác bằng importScripts()
  // importScripts('shared_utils.js');
  ```

- **Trường hợp sử dụng Shared Workers:**
  - Quản lý trạng thái chung giữa nhiều tab/cửa sổ của cùng một ứng dụng (ví dụ: trạng thái đăng nhập, giỏ hàng).
  - Quản lý kết nối WebSocket chung và chia sẻ dữ liệu real-time cho nhiều tab.
  - Điều phối tác vụ giữa các tab.
  - Cache dữ liệu chung.

**4. Giới Hạn và Lưu Ý Khi Dùng Web Workers:**

- **Không truy cập DOM:** Đây là giới hạn quan trọng nhất. Mọi tương tác với UI phải được thực hiện ở luồng chính, dựa trên dữ liệu nhận được từ worker.
- **Overhead giao tiếp:** `postMessage` có overhead (do structured cloning). Tránh gửi tin nhắn quá thường xuyên với dữ liệu lớn nếu không cần thiết. Sử dụng `Transferable Objects` cho dữ liệu lớn.
- **Debug:** Debugging worker có thể phức tạp hơn. Hầu hết các devtools của trình duyệt đều có tab "Sources" cho phép bạn đặt breakpoint trong file worker. `console.log` từ worker thường xuất hiện trong console chính, nhưng đôi khi có thể có console riêng cho worker.
- **Đường dẫn file worker:** Đường dẫn đến file worker là tương đối với origin của trang, không phải là vị trí của script đang gọi `new Worker()`.
- **Worker lồng nhau (Nested Workers):** Một worker có thể tạo ra các worker con khác (subworkers).
- **Module Workers:** Như đã đề cập, bạn có thể tạo worker với `type: 'module'` để sử dụng cú pháp ES Modules (`import`/`export`) bên trong worker, giúp tổ chức code worker tốt hơn.
  ```javascript
  // main.js
  // const moduleWorker = new Worker('path/to/module_worker.js', { type: 'module' });
  ```

**5. Trường Hợp Sử Dụng Phù Hợp cho Web Workers:**

- **Tính toán toán học nặng:** Xử lý ma trận, thuật toán AI/ML nhỏ, mô phỏng vật lý.
- **Xử lý dữ liệu lớn:** Sắp xếp, lọc, phân tích bộ dữ liệu lớn mà không làm treo UI.
- **Mã hóa/Giải mã dữ liệu:** Cho các tác vụ bảo mật hoặc nén/giải nén.
- **Render đồ họa phức tạp:** Tạo ảnh, video frames, hoặc render 3D trong OffscreenCanvas rồi gửi kết quả về luồng chính.
- **Phân tích văn bản hoặc code:** Syntax highlighting, linting trong trình soạn thảo code nền web.
- **Polling dữ liệu từ server:** Một worker có thể định kỳ fetch dữ liệu mà không làm gián đoạn luồng chính.
- **Xây dựng game:** Xử lý logic game, AI của đối thủ trong luồng riêng.

Web Workers là một công cụ mạnh mẽ để cải thiện đáng kể hiệu suất và khả năng phản hồi của các ứng dụng web phức tạp. Việc hiểu rõ khi nào và làm thế nào để sử dụng chúng sẽ giúp bạn xây dựng các trải nghiệm người dùng tốt hơn.

**Phần 14: Performance Best Practices, Debugging và "Ma Thuật Đen" (Advanced Techniques & Gotchas)**

Để đạt đến trình độ "siêu thành thạo" JavaScript, không chỉ cần biết các API mà còn phải hiểu cách tối ưu hóa hiệu suất, gỡ lỗi hiệu quả và nhận biết những khía cạnh "kỳ lạ" hoặc ít được biết đến của ngôn ngữ.

**A. Performance Best Practices (Tối Ưu Hóa Hiệu Suất):**

Hiệu suất là yếu tố then chốt cho trải nghiệm người dùng tốt. Dưới đây là các lĩnh vực chính cần tập trung:

**1. Tối Ưu Hóa Thao Tác DOM:** (Đã đề cập nhiều ở Phần 1, 2, 4)

- **Giảm thiểu Reflow/Repaint:**
  - **Batch DOM updates:** Nhóm các thay đổi DOM lại với nhau. Sử dụng `DocumentFragment` khi thêm nhiều phần tử.
  - **Thay đổi class thay vì nhiều style inline:** `element.classList.add('active-style')` tốt hơn nhiều lệnh `element.style.property = ...`.
  - **Tránh Layout Thrashing:** Đọc tất cả các giá trị layout cần thiết trước, sau đó mới thực hiện các thay đổi DOM.
  - **Animation với `transform` và `opacity`:** Trình duyệt có thể tối ưu hóa chúng bằng GPU, ít gây reflow hơn thay đổi `top/left/width/height`.
- **Cache DOM Queries:** Lưu kết quả của `document.getElementById`, `querySelector` vào biến nếu dùng nhiều lần.
- **Event Delegation:** Giảm số lượng event listener.
- **Virtual DOM (Khi xây dựng framework):** Các framework như React, Vue sử dụng Virtual DOM để tính toán sự khác biệt tối thiểu cần áp dụng lên DOM thật, giảm thiểu thao tác trực tiếp. Nếu bạn tự xây framework, đây là một khái niệm đáng xem xét.

**2. Tối Ưu Hóa JavaScript Execution:**

- **Tránh Global Variables:** Truy cập biến global chậm hơn biến local. Global scope dễ bị "ô nhiễm".
- **Vòng lặp hiệu quả:**
  - Cache độ dài mảng: `for (let i = 0, len = arr.length; i < len; i++)`.
  - Sử dụng các phương thức mảng hiện đại (`forEach`, `map`, `filter`, `reduce`) khi tính dễ đọc quan trọng. Đối với các vòng lặp cực kỳ nhạy cảm về hiệu suất trên mảng rất lớn, vòng lặp `for` truyền thống có thể nhanh hơn một chút.
  - Tránh tính toán nặng hoặc thao tác DOM bên trong vòng lặp nếu có thể.
- **Function Call Optimization:**
  - Giảm số lượng lời gọi hàm không cần thiết.
  - Inline các hàm nhỏ, đơn giản (trình duyệt hiện đại thường tự làm điều này - JIT compiler).
- **Sử dụng `requestAnimationFrame` cho Animations:**

  - Đồng bộ hóa animation với chu kỳ refresh của màn hình, giúp animation mượt mà hơn và tiết kiệm pin hơn `setTimeout` hoặc `setInterval`.

  ```javascript
  // let startTime = null;
  // const elementToAnimate = document.getElementById('animatedBox');

  // function animate(timestamp) {
  //   if (!startTime) startTime = timestamp;
  //   const progress = timestamp - startTime;

  //   // Tính toán vị trí mới, ví dụ: di chuyển sang phải
  //   elementToAnimate.style.transform = `translateX(${Math.min(progress / 5, 200)}px)`; // Di chuyển tối đa 200px trong 1s

  //   if (progress < 1000) { // Tiếp tục animation trong 1 giây
  //     requestAnimationFrame(animate);
  //   } else {
  //     console.log("Animation finished");
  //   }
  // }
  // // requestAnimationFrame(animate); // Bắt đầu animation
  ```

- **Debounce và Throttle Event Handlers:**

  - **Debounce:** Chỉ thực thi hàm sau khi một khoảng thời gian không có sự kiện mới nào được kích hoạt. Hữu ích cho các sự kiện như gõ phím trong ô tìm kiếm (chỉ gửi request sau khi người dùng ngừng gõ).
  - **Throttle:** Đảm bảo hàm chỉ được thực thi tối đa một lần trong một khoảng thời gian nhất định. Hữu ích cho các sự kiện như `scroll` hoặc `resize` để tránh thực thi quá nhiều lần.

  ```javascript
  // Debounce Function
  function debounce(func, delay) {
    let timeoutId;
    return function (...args) {
      clearTimeout(timeoutId);
      timeoutId = setTimeout(() => {
        func.apply(this, args);
      }, delay);
    };
  }

  // Throttle Function
  function throttle(func, limit) {
    let inThrottle;
    let lastFunc;
    let lastRan;
    return function (...args) {
      const context = this;
      if (!inThrottle) {
        func.apply(context, args);
        lastRan = Date.now();
        inThrottle = true;
        setTimeout(function () {
          inThrottle = false;
          if (lastFunc) {
            lastFunc.apply(context, args); // Chạy lần cuối nếu có gọi trong lúc throttle
            lastFunc = null;
            lastRan = Date.now();
          }
        }, limit);
      } else {
        // Lưu lại lần gọi cuối để thực thi sau khi throttle kết thúc
        clearTimeout(lastFunc); // Xóa timeout của lần gọi trước (nếu có)
        lastFunc = setTimeout(function () {
          if (Date.now() - lastRan >= limit) {
            func.apply(context, args);
            lastRan = Date.now();
          }
        }, limit - (Date.now() - lastRan)); // Đảm bảo chạy đúng thời điểm
      }
    };
  }

  // Sử dụng:
  // const handleSearchInput = debounce(function(event) {
  //   console.log('Searching for:', event.target.value);
  //   // Gửi API request ở đây
  // }, 500);
  // searchInputElement.addEventListener('input', handleSearchInput);

  // const handleWindowScroll = throttle(function() {
  //   console.log('Window scrolled!', window.scrollY);
  //   // Thực hiện các tác vụ khi scroll
  // }, 200);
  // window.addEventListener('scroll', handleWindowScroll);
  ```

- **Web Workers cho tác vụ nặng:** (Như Phần 13).
- **Memory Management (Quản lý bộ nhớ):**
  - **Tránh Memory Leaks:** Xóa bỏ các tham chiếu đến đối tượng không còn dùng đến (đặc biệt là DOM elements và event listeners) để garbage collector có thể dọn dẹp.
    - `element.removeEventListener(type, handler)` khi component bị hủy.
    - Xóa các tham chiếu trong mảng, object.
    - Cẩn thận với closures giữ tham chiếu đến biến không cần thiết.
  - **Sử dụng `WeakMap` và `WeakSet`:** Khi key của `WeakMap` hoặc giá trị của `WeakSet` bị garbage collect, entry đó sẽ tự động bị xóa khỏi collection. Hữu ích để liên kết dữ liệu với đối tượng DOM mà không ngăn cản DOM element đó bị dọn dẹp.

**3. Tối Ưu Hóa Tải Trang và Tài Nguyên:**

- **Minify và Compress Code:** Giảm kích thước file JS, CSS, HTML.
- **Code Splitting:** Sử dụng dynamic `import()` để chỉ tải code cần thiết cho trang hiện tại.
- **Lazy Loading Images and Iframes:** Chỉ tải ảnh/iframe khi chúng sắp vào viewport. Sử dụng thuộc tính `loading="lazy"` cho `<img>` và `<iframe>`.
- **Sử dụng CDN (Content Delivery Network):** Phân phối tài nguyên tĩnh từ các server gần người dùng.
- **HTTP Caching:** Cấu hình caching headers đúng cách trên server.
- **Tree Shaking (với bundlers):** Loại bỏ code không sử dụng (dead code) khỏi bundle cuối cùng.
- **Preload, Prefetch, Preconnect:**
  - `<link rel="preload" href="critical.js" as="script">`: Tải tài nguyên ưu tiên cao.
  - `<link rel="prefetch" href="next-page.js" as="script">`: Tải tài nguyên cho navigation tiếp theo (ưu tiên thấp).
  - `<link rel="preconnect" href="https://api.example.com">`: Thực hiện handshake sớm với origin khác.

**B. Debugging Techniques (Kỹ Thuật Gỡ Lỗi):**

Gỡ lỗi hiệu quả là kỹ năng quan trọng.

- **Browser Developer Tools (Công cụ nhà phát triển của trình duyệt):**

  - **Console:**
    - `console.log()`, `console.warn()`, `console.error()`, `console.info()`.
    - `console.table(arrayOfObjects)`: Hiển thị dữ liệu dạng bảng.
    - `console.dir(object)`: Hiển thị chi tiết thuộc tính của object.
    - `console.group()`, `console.groupCollapsed()`, `console.groupEnd()`: Nhóm các log lại.
    - `console.trace()`: In ra call stack.
    - `console.time()`, `console.timeEnd()`: Đo thời gian thực thi.
    - `console.assert(condition, message)`: Log message nếu condition là false.
    - Sử dụng các filter và search trong console.
  - **Sources Panel:**
    - **Breakpoints:** Đặt điểm dừng để xem xét giá trị biến và luồng thực thi.
    - **Conditional Breakpoints:** Breakpoint chỉ kích hoạt khi một điều kiện đúng.
    - **Logpoints:** Thay vì `console.log` trong code, đặt logpoint trong devtools.
    - **Watch Expressions:** Theo dõi giá trị của các biểu thức.
    - **Call Stack:** Xem chuỗi các hàm đã gọi.
    - **Scope Pane:** Xem các biến trong các scope khác nhau (local, closure, global).
    - **Step Over, Step Into, Step Out:** Điều khiển từng bước thực thi.
    - **Blackboxing Scripts:** Bỏ qua các script thư viện khi stepping.
  - **Network Panel:**
    - Kiểm tra các request/response HTTP, headers, status codes, timing.
    - Lọc, tìm kiếm request.
    - Throttling network để giả lập kết nối chậm.
  - **Elements Panel:**
    - Kiểm tra và sửa đổi DOM, CSS.
    - Xem computed styles, event listeners, layout.
    - Break on DOM changes (subtree modifications, attribute modifications, node removal).
  - **Performance Panel:**
    - Record và phân tích chi tiết hiệu suất tải trang và runtime (CPU usage, JavaScript execution, rendering, layout, painting).
    - Tìm các "bottlenecks", long tasks.
  - **Memory Panel:**
    - Chụp heap snapshots để tìm memory leaks.
    - Phân tích allocation timeline.
  - **Application Panel:**
    - Kiểm tra `localStorage`, `sessionStorage`, `IndexedDB`, `cookies`, `Cache Storage` (Service Workers), etc.

- **`debugger;` Statement:**
  Chèn `debugger;` vào code JavaScript của bạn. Khi devtools đang mở, trình duyệt sẽ tự động dừng tại dòng đó, giống như một breakpoint.

- **Error Handling:**

  - Sử dụng `try...catch...finally` để bắt và xử lý lỗi một cách duyên dáng.
  - `window.onerror` và `window.onunhandledrejection`: Global error handlers để bắt các lỗi không được xử lý và các Promise rejection không được bắt. Hữu ích để log lỗi lên server.

  ```javascript
  // window.onerror = function(message, source, lineno, colno, error) {
  //   console.error("Global error caught:", message, "at", source, `L${lineno}:C${colno}`);
  //   // Gửi thông tin lỗi lên server
  //   // reportErrorToServer({ message, source, lineno, colno, stack: error ? error.stack : null });
  //   return true; // true để ngăn trình duyệt log lỗi mặc định ra console
  // };

  // window.addEventListener('unhandledrejection', function(event) {
  //   console.error("Unhandled Promise rejection:", event.reason);
  //   // reportErrorToServer({ type: 'unhandledrejection', reason: event.reason });
  // });
  ```

- **Linting and Static Analysis:**
  Sử dụng các công cụ như ESLint, JSHint để phát hiện lỗi cú pháp, style code không nhất quán và các "code smells" tiềm ẩn trước khi chạy code.

**C. "Ma Thuật Đen" (Advanced Techniques, Quirks & Gotchas):**

Đây là những khía cạnh ít phổ biến hơn, đôi khi gây ngạc nhiên hoặc cần hiểu biết sâu để sử dụng đúng cách.

- **Type Coercion (Ép kiểu ngầm định):**
  JavaScript tự động ép kiểu trong nhiều trường hợp, có thể dẫn đến kết quả bất ngờ.

  ```javascript
  console.log(1 + "2"); // "12" (number to string)
  console.log("5" - 2); // 3 (string to number)
  console.log("5" * 2); // 10
  console.log(true + 1); // 2 (true is 1, false is 0)
  console.log([] + {}); // "[object Object]"
  console.log({} + []); // 0 (trong console trình duyệt, do {} được coi là block rỗng)
  // ({} + []) trong nodejs hoặc (({}) + []) là "[object Object]"
  console.log(!+[] + !+[]); // 2 (WTFJS classic)
  console.log([] == ![]); // true
  ```

  **Best practice:** Luôn sử dụng toán tử so sánh nghiêm ngặt (`===`, `!==`) để tránh ép kiểu không mong muốn. Hiểu rõ cách ép kiểu hoạt động khi làm việc với các toán tử khác.

- **`this` Keyword Binding:** (Đã đề cập ở Phần 3 về Events)
  Giá trị của `this` phụ thuộc vào cách hàm được gọi, không phải nơi nó được định nghĩa.

  - Global context: `this` là `window` (trong browser, non-strict mode) hoặc `undefined` (strict mode).
  - Function call: `this` là `window` (non-strict) hoặc `undefined` (strict).
  - Method call (`object.method()`): `this` là `object`.
  - Constructor call (`new MyClass()`): `this` là instance mới được tạo.
  - Arrow functions: `this` được kế thừa từ lexical scope (phạm vi bao quanh lúc định nghĩa).
  - `call()`, `apply()`, `bind()`: Cho phép set `this` một cách tường minh.

- **Prototypes và Prototypal Inheritance:**
  JavaScript sử dụng kế thừa prototype. Mọi object đều có một prototype (có thể là `null`), và nó kế thừa thuộc tính/phương thức từ prototype đó. `__proto__` (cũ, không nên dùng trực tiếp) hoặc `Object.getPrototypeOf()` để truy cập. `Object.create()` để tạo object với prototype cụ thể. Class syntax (ES6) là "syntactic sugar" trên nền tảng prototype.

  ```javascript
  function Animal(name) {
    this.name = name;
  }
  Animal.prototype.speak = function () {
    console.log(`${this.name} makes a noise.`);
  };
  const dog = new Animal("Dog");
  // dog.speak(); // "Dog makes a noise."
  // console.log(Object.getPrototypeOf(dog) === Animal.prototype); // true
  ```

- **Closures:**
  Một closure là sự kết hợp của một hàm và môi trường lexical (phạm vi biến) mà hàm đó được khai báo. Nó cho phép hàm truy cập các biến từ scope bên ngoài, ngay cả sau khi scope bên ngoài đã thực thi xong.

  ```javascript
  function makeCounter() {
    let count = 0;
    return function () {
      count++;
      return count;
    };
  }
  const counter1 = makeCounter();
  // console.log(counter1()); // 1
  // console.log(counter1()); // 2
  // const counter2 = makeCounter();
  // console.log(counter2()); // 1 (count của counter2 độc lập)
  ```

  Closures rất mạnh mẽ (dùng cho data privacy, currying, function factories) nhưng cũng có thể gây memory leak nếu không cẩn thận (giữ tham chiếu đến biến không cần thiết).

- **Hoisting:**
  Khai báo biến (`var`) và khai báo hàm (function declarations, không phải function expressions) được "nâng" lên đầu scope của chúng trước khi code thực thi. Tuy nhiên, chỉ khai báo được hoist, phép gán thì không.

  ```javascript
  // console.log(x); // undefined (do var x được hoist)
  // var x = 5;
  // foo(); // "Hello" (function declaration được hoist)
  // function foo() { console.log("Hello"); }
  // bar(); // TypeError: bar is not a function (var bar được hoist, nhưng gán hàm thì chưa)
  // var bar = function() { console.log("World"); }
  ```

  **Best practice:** Luôn khai báo biến (`let`, `const`) ở đầu scope của chúng để tránh nhầm lẫn. `let` và `const` cũng được hoist nhưng có "Temporal Dead Zone" (TDZ) - không thể truy cập trước khi khai báo.

- **Event Loop và Asynchronous Nature:**
  JavaScript là đơn luồng nhưng có thể xử lý các tác vụ bất đồng bộ (I/O, timers, events) nhờ event loop, call stack, message queue (task queue), và microtask queue.

  - **Call Stack:** Nơi các function call được thêm vào và lấy ra.
  - **Web APIs/Node APIs:** Xử lý các tác vụ bất đồng bộ (ví dụ: `setTimeout`, `fetch`). Khi hoàn thành, callback/Promise handler được đưa vào queue.
  - **Message Queue (Task Queue):** Chứa các callback từ Web APIs (ví dụ: `setTimeout`, event handlers). Event loop lấy task từ đây khi call stack rỗng.
  - **Microtask Queue (Job Queue):** Chứa các callback từ Promises (`.then`, `.catch`, `.finally`), `queueMicrotask()`, `MutationObserver`. Microtasks được ưu tiên hơn tasks, thực thi sau khi script hiện tại kết thúc và trước khi event loop lấy task tiếp theo từ message queue.
    Hiểu rõ event loop giúp giải thích tại sao `setTimeout(fn, 0)` không thực thi ngay lập tức, và hành vi của Promises.

- **`NaN` (Not a Number):**

  - Là một giá trị number đặc biệt.
  - `typeof NaN` là `'number'`.
  - `NaN` không bằng bất cứ thứ gì, kể cả chính nó (`NaN === NaN` là `false`).
  - Sử dụng `Number.isNaN(value)` để kiểm tra `NaN` một cách đáng tin cậy (khác với `isNaN(value)` toàn cục, `isNaN('foo')` là `true` do ép kiểu).

- **Floating Point Precision (Độ chính xác số thực):**
  Do cách biểu diễn số thực nhị phân, một số phép tính có thể cho kết quả không mong đợi.

  ```javascript
  console.log(0.1 + 0.2); // 0.30000000000000004
  // console.log(0.1 + 0.2 === 0.3); // false
  ```

  **Giải pháp:** Sử dụng các thư viện cho số học chính xác (ví dụ: `Decimal.js`, `BigNumber.js`) nếu cần, hoặc làm tròn kết quả đến một số chữ số thập phân nhất định khi so sánh/hiển thị.

- **`arguments` Object (trong hàm non-arrow):**
  Là một đối tượng giống mảng (array-like) chứa tất cả các đối số được truyền vào hàm, ngay cả khi hàm không khai báo tham số.
  Không nên dùng trong code hiện đại, ưu tiên rest parameters (`...args`).

  ```javascript
  // function sumAll() {
  //   let sum = 0;
  //   for (let i = 0; i < arguments.length; i++) {
  //     sum += arguments[i];
  //   }
  //   return sum;
  // }
  // console.log(sumAll(1, 2, 3, 4)); // 10

  // Hiện đại hơn:
  // function sumAllModern(...args) {
  //   return args.reduce((acc, current) => acc + current, 0);
  // }
  ```

- **Comma Operator (Toán tử phẩy):**
  Đánh giá nhiều biểu thức từ trái sang phải và trả về giá trị của biểu thức cuối cùng. Ít dùng, nhưng có thể thấy trong một số code rút gọn.

  ```javascript
  // let x = (1, 2, 3); // x sẽ là 3
  // console.log(x);
  ```

- **Bitwise Operators (Toán tử bit):**
  `&`, `|`, `^`, `~`, `<<`, `>>`, `>>>`. Hoạt động trên biểu diễn nhị phân 32-bit của số. Hữu ích trong một số thuật toán cấp thấp, làm việc với cờ (flags), hoặc tối ưu hóa một số phép tính (ví dụ: `~~num` để làm tròn xuống nhanh hơn `Math.floor(num)` cho số dương, nhưng ít rõ ràng hơn).

- **`with` Statement (Tuyệt đối không nên dùng):**
  Mở rộng scope chain với một object cụ thể. Gây khó khăn cho việc hiểu code và tối ưu hóa, bị cấm trong strict mode.
  ```javascript
  // // KHÔNG DÙNG CÁI NÀY
  // const obj = { a: 1, b: 2 };
  // with (obj) {
  //   console.log(a); // 1
  //   console.log(b); // 2
  // }
  ```

Phần này bao gồm nhiều chủ đề rộng lớn. Mục tiêu là cung cấp cho bạn những điểm khởi đầu để tìm hiểu sâu hơn. "Ma thuật đen" thường là những kỹ thuật nên được sử dụng một cách cẩn trọng và có hiểu biết, vì chúng có thể làm code khó đọc hoặc gây ra lỗi khó tìm nếu lạm dụng. Ưu tiên code rõ ràng, dễ bảo trì.

**Phần 15: Xây Dựng Frontend Framework Riêng (Conceptual Overview & Key Components)**

Sau khi đã nắm vững các kiến thức từ cơ bản đến nâng cao về Vanilla JavaScript và các Web API, việc tự xây dựng một frontend framework nhỏ của riêng mình không còn là điều quá xa vời. Đây là một cách tuyệt vời để hiểu sâu hơn về cách các framework lớn hoạt động và củng cố kiến thức của bạn.

Phần này sẽ không cung cấp code hoàn chỉnh của một framework, mà sẽ tập trung vào các **khái niệm cốt lõi và các thành phần chính** bạn cần xem xét khi thiết kế.

**A. Tại Sao Lại Tự Xây Dựng Framework?**

- **Học hỏi sâu sắc:** Hiểu rõ "bên trong" của rendering, state management, component lifecycle.
- **Tùy chỉnh tối đa:** Tạo ra một giải pháp chính xác cho nhu cầu của bạn, không có "bloat" từ các tính năng không cần thiết.
- **Thử thách bản thân:** Một dự án thú vị để áp dụng và mở rộng kỹ năng.
- **Nâng cao khả năng thiết kế phần mềm:** Suy nghĩ về API design, modularity, reusability.

**Lưu ý:** Việc xây dựng một framework đủ mạnh để dùng trong production cho các dự án lớn là một công việc rất phức tạp và tốn thời gian. Mục tiêu ở đây chủ yếu là học hỏi và tạo ra các công cụ nhỏ, hữu ích cho các dự án cá nhân hoặc để hiểu rõ hơn các framework hiện có.

**B. Các Thành Phần Cốt Lõi của một Frontend Framework:**

Hầu hết các frontend framework hiện đại đều xoay quanh một số khái niệm và thành phần chính:

**1. Component-Based Architecture (Kiến Trúc Dựa Trên Component):**

- **Khái niệm:** Chia giao diện người dùng thành các phần nhỏ, độc lập, có thể tái sử dụng gọi là "components". Mỗi component quản lý HTML, CSS và JavaScript của riêng nó.
- **Triển khai:**
  - **Định nghĩa Component:**
    - Sử dụng class ES6 kế thừa từ một base class (ví dụ: `MyFramework.Component`) hoặc một hàm trả về cấu trúc component.
    - Sử dụng Custom Elements (Web Components) là một lựa chọn tự nhiên và chuẩn hóa.
  - **Template:**
    - Cách component định nghĩa cấu trúc HTML của nó. Có thể dùng:
      - Thẻ `<template>` của HTML.
      - Chuỗi HTML (cần cẩn thận với XSS nếu có nội dung động).
      - JSX-like syntax (nếu bạn tự viết parser hoặc dùng thư viện).
      - Tagged template literals.
  - **Props (Properties):** Dữ liệu truyền từ component cha xuống component con. Props thường là read-only đối với component con.
  - **State:** Dữ liệu nội bộ của component, có thể thay đổi và khi thay đổi sẽ kích hoạt re-render component.
  - **Lifecycle Methods:** Các hàm được gọi tự động tại các thời điểm khác nhau trong vòng đời của component (ví dụ: `constructor`, `connectedCallback` (cho Custom Elements), `render`, `updated`, `disconnectedCallback`).
  - **Event Handling:** Cách component phản hồi lại tương tác người dùng hoặc các sự kiện khác.

**2. Rendering Mechanism (Cơ Chế Kết Xuất):**

- **Khái niệm:** Quá trình chuyển đổi định nghĩa component (template và state/props) thành các node DOM thực tế trên trang.
- **Các cách tiếp cận:**
  - **Manual DOM Manipulation:** Component tự chịu trách nhiệm tạo, cập nhật, xóa các node DOM. Khá phức tạp và dễ lỗi.
  - **String-based Rendering (dùng `innerHTML`):**
    - Component tạo ra một chuỗi HTML hoàn chỉnh rồi gán vào `innerHTML` của một container.
    - Đơn giản nhưng có thể không hiệu quả cho các cập nhật nhỏ (vì re-render toàn bộ) và có thể làm mất state của các input field hoặc các event listener được gán thủ công.
  - **Declarative Rendering với Diffing (Virtual DOM-like):**
    - **Render Function:** Mỗi component có một hàm `render()` trả về một mô tả "ảo" của DOM mà nó muốn (ví dụ: một object tree mô tả cấu trúc, thuộc tính, và nội dung).
    - **Diffing Algorithm:** Khi state/props thay đổi, gọi lại `render()` để lấy mô tả DOM mới. So sánh (diff) mô tả mới với mô tả cũ để tìm ra những thay đổi tối thiểu.
    - **Patching:** Áp dụng những thay đổi tối thiểu đó lên DOM thật.
    - Đây là cách các framework lớn như React, Vue hoạt động. Xây dựng một thuật toán diffing hiệu quả là phần khó nhất.
    - **Đơn giản hóa Diffing (cho framework nhỏ):**
      - **Keyed Lists:** Khi render danh sách, sử dụng key duy nhất cho mỗi item để giúp thuật toán diffing xác định đúng item nào đã thay đổi, thêm, hoặc xóa.
      - **Targeted Updates:** Nếu biết chính xác phần nào của component cần cập nhật, có thể cập nhật trực tiếp DOM node đó thay vì re-render toàn bộ và diff.

**3. State Management (Quản Lý Trạng Thái):**

- **Khái niệm:** Cách ứng dụng lưu trữ, cập nhật và chia sẻ dữ liệu (state) giữa các component.
- **Các cấp độ:**
  - **Component-Local State:** Dữ liệu chỉ liên quan đến một component cụ thể. Thường được quản lý bên trong chính component đó.
  - **Global State / Shared State:** Dữ liệu cần được truy cập hoặc thay đổi bởi nhiều component khác nhau trong ứng dụng.
- **Triển khai (cho framework nhỏ):**

  - **Props Drilling:** Truyền state từ component cha xuống các component con qua props. Có thể trở nên rườm rà với nhiều cấp lồng nhau.
  - **Event Bus / PubSub Pattern:**
    - Tạo một đối tượng trung gian (event bus).
    - Components có thể "publish" (phát) các sự kiện khi state thay đổi.
    - Các components khác có thể "subscribe" (đăng ký) để lắng nghe các sự kiện đó và cập nhật state cục bộ của chúng.
    - Ví dụ đơn giản:
      ```javascript
      // EventBus.js
      // class EventBus {
      //   constructor() { this.listeners = {}; }
      //   on(event, callback) {
      //     if (!this.listeners[event]) this.listeners[event] = [];
      //     this.listeners[event].push(callback);
      //   }
      //   emit(event, data) {
      //     if (this.listeners[event]) {
      //       this.listeners[event].forEach(cb => cb(data));
      //     }
      //   }
      //   off(event, callback) { /* ... remove listener ... */ }
      // }
      // const globalEventBus = new EventBus();
      // export default globalEventBus;
      ```
  - **Simple Global Store (Object/Proxy):**

    - Tạo một object JavaScript toàn cục để lưu trữ state.
    - Khi state thay đổi, cần một cơ chế để thông báo cho các component liên quan để chúng re-render.
    - Sử dụng `Proxy` để tự động phát hiện thay đổi state và kích hoạt re-render (reactivity).

    ```javascript
    // Store.js (ví dụ rất cơ bản)
    // let _state = { count: 0, user: null };
    // const _subscribers = new Set();

    // function notifySubscribers() {
    //   _subscribers.forEach(callback => callback(_state));
    // }

    // export const store = {
    //   getState: () => ({ ..._state }), // Trả về bản sao để tránh sửa đổi trực tiếp
    //   subscribe: (callback) => {
    //     _subscribers.add(callback);
    //     return () => _subscribers.delete(callback); // Unsubscribe function
    //   },
    //   dispatch: (action) => { // Hoặc setState trực tiếp
    //     if (action.type === 'INCREMENT') _state.count++;
    //     if (action.type === 'SET_USER') _state.user = action.payload;
    //     // ... other actions
    //     notifySubscribers();
    //   }
    // };

    // // Hoặc dùng Proxy cho reactivity đơn giản
    // const reactiveState = new Proxy(_state, {
    //   set(target, property, value) {
    //     target[property] = value;
    //     notifySubscribers(); // Thông báo khi có thay đổi
    //     return true;
    //   }
    // });
    // export { reactiveState, store.subscribe }; // Component dùng reactiveState và subscribe
    ```

  - **Context API-like (Nếu dùng cây component):** Cho phép truyền dữ liệu xuống cây component mà không cần props drilling ở mỗi cấp.

**4. Routing (Điều Hướng Trang):**

- **Khái niệm:** Quản lý việc hiển thị các "trang" hoặc "views" khác nhau của ứng dụng dựa trên URL, mà không cần tải lại toàn bộ trang từ server (cho Single Page Applications - SPA).
- **Triển khai:**
  - **Lắng nghe thay đổi URL:**
    - Sự kiện `hashchange` (cho URL dạng `example.com/#/path`).
    - History API: `popstate` event và các phương thức `history.pushState()`, `history.replaceState()` để thay đổi URL mà không tải lại trang (cho URL dạng `example.com/path`).
  - **Định nghĩa Routes:** Một cấu hình ánh xạ giữa các mẫu URL (path) và các component/view tương ứng.
    ```javascript
    // const routes = [
    //   { path: '/', component: HomeComponent },
    //   { path: '/about', component: AboutComponent },
    //   { path: '/products/:id', component: ProductDetailComponent } // Route với tham số
    // ];
    ```
  - **Router Logic:**
    - Khi URL thay đổi, tìm route khớp.
    - Render component tương ứng của route đó.
    - Xử lý tham số từ URL (ví dụ: `id` trong `/products/:id`).
  - **Navigation Links:** Cung cấp một cách để tạo link điều hướng trong ứng dụng (ví dụ: một component `<RouterLink to="/about">About</RouterLink>`) mà không gây tải lại trang. Các link này sẽ gọi `history.pushState()`.

**5. Build Process & Tooling (Tùy Chọn, nhưng hữu ích):**

Mặc dù mục tiêu là "Vanilla JS", khi framework phức tạp hơn, bạn có thể cần:

- **Bundler (Webpack, Rollup, Parcel):** Để gộp nhiều file JS thành một (hoặc vài) file, xử lý ES Modules cho các trình duyệt cũ hơn, tối ưu hóa code.
- **Transpiler (Babel):** Để sử dụng các tính năng JavaScript mới nhất và đảm bảo tương thích với các trình duyệt cũ.
- **CSS Preprocessors (SASS, LESS):** Nếu bạn muốn sử dụng các tính năng CSS nâng cao.
- **Linter (ESLint):** Đảm bảo chất lượng code.

Tuy nhiên, cho một framework nhỏ tự học, bạn hoàn toàn có thể bắt đầu mà không cần build tools phức tạp, chỉ dùng ES Modules trực tiếp trên trình duyệt.

**C. Các Bước Xây Dựng Cơ Bản (Gợi Ý):**

1.  **Thiết Kế API Component Cốt Lõi:**
    - Quyết định cách định nghĩa component (class, function, Custom Element).
    - Xác định các lifecycle methods cơ bản (ví dụ: `constructor`, `render`, `onMount`, `onUpdate`, `onUnmount`).
    - Cách xử lý props và state.
2.  **Triển Khai Cơ Chế Rendering Đơn Giản:**
    - Bắt đầu với việc render chuỗi HTML vào `innerHTML`.
    - Hoặc, mỗi component tự tạo và quản lý các node DOM của nó.
3.  **Thêm Quản Lý State Cơ Bản:**
    - Cho phép component có state cục bộ.
    - Khi state thay đổi, gọi lại hàm `render` của component (và các con của nó nếu cần).
4.  **Triển Khai Props:**
    - Cho phép truyền dữ liệu từ cha xuống con.
    - Khi props thay đổi, component con cũng cần re-render.
5.  **Tạo Hệ Thống Routing Đơn Gi giản (nếu là SPA):**
    - Sử dụng `hashchange` hoặc History API.
    - Ánh xạ path với component.
6.  **Nâng Cao Rendering (Nếu có tham vọng):**
    - Nghiên cứu về Virtual DOM và thuật toán diffing.
    - Thử nghiệm với việc tạo một mô tả DOM ảo và cơ chế patch.
7.  **Đóng Gói và Tái Sử Dụng:**
    - Sử dụng ES Modules để tổ chức code framework thành các module riêng (core, components, router, store...).
    - Nghĩ về cách người dùng sẽ import và sử dụng framework của bạn.
8.  **Tài Liệu Hóa (Documentation):**
    Ngay cả với framework cá nhân, việc viết tài liệu về cách sử dụng API của bạn cũng rất hữu ích.

**D. Ví Dụ Ý Tưởng về Cấu Trúc Component Đơn Giản:**

```javascript
// BaseComponent.js (ví dụ dùng class)
// class BaseComponent {
//   constructor(props = {}) {
//     this.props = props;
//     this.state = this.initState ? this.initState() : {};
//     this._element = null; // Tham chiếu đến DOM element của component
//   }

//   setState(newState) {
//     const oldState = { ...this.state };
//     this.state = { ...this.state, ...newState };
//     // Trigger re-render (đơn giản hóa: re-render toàn bộ)
//     if (this._element && this._element.parentNode) {
//       const newElement = this.render(); // Giả sử render() trả về DOM element
//       this._element.parentNode.replaceChild(newElement, this._element);
//       this._element = newElement;
//       this._attachEvents(); // Gắn lại sự kiện nếu cần
//       if (this.componentDidUpdate) this.componentDidUpdate(oldState, this.props);
//     }
//   }

//   // Phương thức render này sẽ được override bởi component con
//   render() {
//     // Trả về một DOM element hoặc một chuỗi HTML
//     throw new Error("Render method must be implemented by subclasses.");
//   }

//   // Gắn vào DOM
//   mount(parentElement) {
//     this._element = this.render();
//     this._attachEvents();
//     parentElement.appendChild(this._element);
//     if (this.componentDidMount) this.componentDidMount();
//     return this._element;
//   }

//   // Gỡ khỏi DOM
//   unmount() {
//     if (this._element && this._element.parentNode) {
//       this._element.parentNode.removeChild(this._element);
//     }
//     if (this.componentWillUnmount) this.componentWillUnmount();
//     this._element = null;
//   }

//   // Phương thức để component con định nghĩa event listeners
//   _attachEvents() {
//     // Ví dụ:
//     // const button = this._element.querySelector('button');
//     // if (button && this.handleClick) {
//     //   button.addEventListener('click', this.handleClick.bind(this));
//     // }
//   }

//   // Lifecycle hooks (optional)
//   // initState() { return {}; }
//   // componentDidMount() {}
//   // componentDidUpdate(prevStat, prevProps) {}
//   // componentWillUnmount() {}
// }

// // MyCounterComponent.js
// class MyCounter extends BaseComponent {
//   initState() {
//     return { count: this.props.initialCount || 0 };
//   }

//   handleClick() {
//     this.setState({ count: this.state.count + 1 });
//   }

//   render() {
//     const div = document.createElement('div');
//     div.innerHTML = `
//       <p>Count: ${this.state.count}</p>
//       <button>Increment</button>
//     `;
//     // Gắn sự kiện trong render hoặc trong _attachEvents
//     const button = div.querySelector('button');
//     button.addEventListener('click', this.handleClick.bind(this));
//     return div;
//   }

//   componentDidMount() { console.log("Counter mounted!"); }
//   componentDidUpdate(prevState) { console.log("Counter updated from", prevState.count, "to", this.state.count); }
// }

// // Sử dụng:
// // const counterInstance = new MyCounter({ initialCount: 5 });
// // counterInstance.mount(document.getElementById('app'));
```

Đây chỉ là một ý tưởng rất sơ khai. Một framework thực tế sẽ phức tạp hơn nhiều, đặc biệt là ở phần rendering và state management.

**E. Thử Thách và Niềm Vui:**

Xây dựng framework riêng là một hành trình đầy thử thách nhưng cũng rất bổ ích. Bạn sẽ phải đối mặt với nhiều vấn đề thiết kế, tối ưu hóa và gỡ lỗi. Nhưng qua đó, bạn sẽ hiểu sâu sắc hơn về cách JavaScript và trình duyệt hoạt động, và có thể tự tin hơn khi làm việc với bất kỳ công nghệ frontend nào.

Hãy bắt đầu nhỏ, tập trung vào một vài tính năng cốt lõi, và từ từ mở rộng. Chúc bạn thành công trên con đường "code như thần thánh" với Vanilla JavaScript!
