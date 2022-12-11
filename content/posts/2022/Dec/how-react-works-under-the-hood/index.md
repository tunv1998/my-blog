---
title: "React hoạt động như thế nào?"
description: "Bạn tôi ơi, bạn vẫn chưa làm chủ được sức mạnh React của mình đâu."
date: 2022-12-10T21:45:42+07:00
draft: false
categories:
  - react
tags:
  - react
image: "intro.jpeg"
---

[React](https://beta.reactjs.org/) là một thư viện Javascript rất phổ biến, và đang phát triển rất mạnh. Nhưng mấy ai biết được cách mà React thực sự hoạt động (trong đó có mình T.T). Đa số các lập trình viên đều biết các khái niệm và làm việc với React (core concepts, API, hooks, ...), nhưng cách React làm việc (how and why) thì vẫn còn khá mù mờ.

## Vậy React làm công chiện gì?

Công việc chính của React là làm vườn, nó chăm sóc cho bạn 1 cái cây, không phải cây cảnh mà là cây DOM. Cây này giúp các bạn các phép tính toán trên nodes.

![DOM](https://www.w3schools.com/js/pic_htmltree.gif)

_DOM Nodes (src: w3schools)_

Nhưng bất ngờ chưa, cây này không phải là cây thật (Real DOM) mà là cây ảo (Virtual DOM). React không những là anh làm vườn, mà còn là một cây tỉa nến chuyên nghiệp =]]z.

Vậy tại sao lại có cây này, hiểu đơn giản thì _Real DOM_ giống như ngôi nhà đã được xây dựng vậy, còn _Virtual DOM_ thì giống như bản thiết kế (blueprint). Đoán xem thay đổi ở đâu nhanh và nhẹ hơn nào.

> Để có thể hiểu thêm quá trình Rendering trên trình duyệt thì bạn có thể [nghía qua post này](https://medium.com/jspoint/how-the-browser-renders-a-web-page-dom-cssom-and-rendering-df10531c9969)

## JSX

Làm việc với React hẳn rất quen thuộc với cú pháp này, nó gọi là [JSX](https://beta.reactjs.org/learn/writing-markup-with-jsx)

```jsx
const Tag = () => {
  return <div>Hello World!</div>;
};
```

JSX là một cú pháp đặc biệt, chẳng phải là `string`, `js` hay là `HTML`. Nó là cú pháp mở rộng cho `js` và cho phép bạn viết cú pháp giống như `HTML` trong file `js` để tạo ra một đối tượng cụ thể.

Về cơ bản những gì bạn làm sẽ là thế này:

```jsx
const Tag = () => {
  return React.createElement("div", {}, "Hello World!");
};

/**
 * createElement sẽ trông như thế này.
 * {
    $$typeof: Symbol(react.element),
    key: null,
    props: {children: "Hello World!"},
    ref: null,
    type: "div"
}
*/
```

<!-- Đây là cây _DOM ảo_ mà đã được nhắc đến ở trên. -->

Giờ bạn biết rồi đấy, khi app chạy, JSX được parsed và mọi phương thức React.createElement được gọi thì ta sẽ có một cục Object siêu to khổng lồ được lồng vào nhau.

## React Virtual DOM và Reconciliation

Ứng dụng React được tạo thành bởi các Components. Component bản chất là nhóm React Element được trả về bởi phương thức `render()` hoặc `return`.

Và bởi các Component được lồng vào nhau và được duyệt trong quá trình chạy, React Elements thiết lập liên kết cha-con tương tự như DOM, đây chính là _DOM ảo_ mà ta đã nói ở trên. Sau đó React sẽ đồng bộ _DOM ảo_ với _DOM thật_.

> React uses virtual DOM which is a lightweight version of the DOM. The only difference is the ability to write the screen like the real DOM, in fact, a new virtual DOM is created after every re-render. DOM stores the components of a website in a tree structure.
> [Diffing Algorithm](https://www.geeksforgeeks.org/what-is-diffing-algorithm/)

![How DOM updated in React](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20220131100246/Group-2-2.jpg)

_How DOM updated in react (src: geeksforgeeks)_

### Cách DOM cập nhật trên React

- Lần chạy đầu tiên, cả DOM ảo và DOM thật được khởi tạo.
- React hoạt động trên `observable pattern`, từ đây, bất kì thay đổi nào trên state, React sẽ cập nhật lại DOM ảo.
- Sau đó, React so sánh DOM ảo và DOM thật và cập nhật thay đổi, quá trình này gọi là `reconciliation` (hay giảng hòa).

Ract dùng thuật toán heuristic hay gọi là thuật toán Diffing để so sánh sự khác nhau.

React kiểm tra phần tử gỗc để thay đổi và cập nhật dựa trên `type` của element

React không thực hiện nhiều tính toán ở những phần diffing, React giả sử rằng nếu parent thay đổi, thì child chắn chắn sẽ thay đổi.

Ví dụ

```jsx
<div className="class">
  <p>I did not change</p>
</div>
```

Và nếu ta thay đổi `type` của thẻ `div`

```jsx
<p className="class">
  <p>I did not change</p>
</p>
```

Mặc dù ta không cần tạo lại thẻ `p`, nhưng React sẽ không biết khi duyệt cây từ trên xuống. Vì vậy, React quyết định hủy toàn bộ children và khởi tạo lại từ đầu.

Trong trường hợp phần tử có cùng type, React sẽ kiểm tra đến các thuộc tính. Sau đó mới cập nhật các nodes có thay đổi. Component sex được cập nhật vào lifecycle tiếp theo. Và đây cũng là lí do tại sao ta **bắt buộc** có thuộc tính `key` trong list render, để React có thể dễ dàng xác định các cập nhật trong các phần tử.

## Tổng kết

Hi vọng bài viết này đã đủ để nói về khái niệm về cách mà React đang hoạt động.

## Tham khảo

[React under the hood - FreeCodeCamp](https://www.freecodecamp.org/news/react-under-the-hood/)

[What Does React Actually Do?](https://betterprogramming.pub/what-does-react-actually-do-c9412c08bfe6)

[How React Works Under the Hood](https://javascript.plainenglish.io/how-react-works-under-the-hood-277356c95e3d)

[How does React JS actually work?](https://codewithsudeep.com/sudeep/javascript/reactjs/how-does-react-js-actually-work/)

[React Behind The Scene](https://medium.com/@mousumi.cse05/react-behind-the-scene-521dea44ed0e)

[What is Diffing Algorithm ?](https://www.geeksforgeeks.org/what-is-diffing-algorithm/)
