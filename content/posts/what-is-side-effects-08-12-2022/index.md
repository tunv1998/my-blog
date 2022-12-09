---
title: "Pure Functions và Side Effects"
description: "Pure Functions và Side Effects được biết đến như các khái niệm trong lập trình chức năng (functional programming)"
date: 2022-12-08T23:34:03+07:00
draft: false
categories:
  - React
  - javascript
tags:
  - react
  - "pure function"
  - "side effect"
  - js
image: "intro.png"
---

<em>
Pure Functions và Side Effects được biết đến như các khái niệm trong lập trình chức năng (functional programming), và thường được sử dụng rộng rãi trong Javascript. Vậy nó là gì, hãy cùng tìm hiểu trong bài viết này.
</em>

## Javascript Functions

Một `function` là một tập hợp các câu lệnh để thực hiện một tác vụ hoặc tính toán giá trị.

Trong `Javascript`, `function` là `first-class citizens`.

> Một ngôn ngữ lập trình có khả năng xem các `function` như là một giá trị, có thể truyền `function` như là một tham số vào `function` khác. Hoặc trả về một `function`. Thì ngôn ngữ đó có `First-Class Function` và `function` được gọi là `First-Class Citizens`.

`function` có thể có giá trị trả về hoặc không, trong trường hợp không có giá trị trả về, nó sẽ trả về `undefined`

```js
function sum(a, b) {
  return a + b; // return a number
}

function returnNothing(str) {
  console.log(str); // log to the console, and return undefined
}
```

## Pure Function

Là một lập trình viên (LTV, dev, ...), ta viết code để tạo đầu ra (output) dựa trên giá trị đầu vào (input) (ví dụ: hàm `sum` ở trên). Ta cần đảm bảo rằng:

- `Có thể dự đoán được kết quả đầu ra (Predictable)`: với cùng một giá trị đầu vào, luôn trả về cùng một giá trị đầu ra.
- `Dễ đọc (Readable)`: bất kì ai đọc hàm thì hiểu được mục đích và chức năng của nó.
- `Tái sử dụng (Reusable)`: có thể tái sử dụng ở nhiều nơi trong source code và không ảnh hưởng hoặc thay đổi hành vi của nó.
- `Có thể test (Testable)`: ta có thể kiểm thử nó như một đơn vị độc lập.

Một `Pure Function` có tất cả đặc điểm trên, nó là một `function` trả về cùng một giá trị với cùng một đầu vào. `Pure Function` không nên có bất kì `Side Effect` nào để thay đổi đầu ra dự kiến.

Đoán xem, đây có phải là một `Pure Function` không? và tại sao?

```js
function getFullName(firstName, lastName) {
  return firstName + lastName;
}
```

Đúng rầu, hắn là một `Pure Function`

Giờ thì qua ví dụ này

```js
let greeting = "";

function sayGreeting(name) {
  return greeting + name;
}
```

Well, trong trường hợp này hắn không còn là một `Pure Function` nữa, vì giờ chúng ta có một biến phụ thuộc **nằm bên ngoài scope function**.

```js
greeting = "Hola ";
sayGreeting("Tèo"); // Hola Tèo

greeting = "Hello ";
sayGreeting("Tèo"); // Hello Tèo
```

`sayGreeting` bây giờ đã không còn khả năng đoán được kết quả trả về, nếu ai đó thay đổi giá trị của `greeting` thì nó sễ thay đổi luôn giá trị đầu ra mặc dù chúng ta có cùng một giá trị đầu vào.

Vì vậy chúng ta có thể thấy `Side-Effect` qua ví dụ trên, phụ thuộc vào giá trị bên ngoài, có thể thay đổi mà trong `function` không nhận biết được.

## Side Effects

`Side Effect` là mọi thứ năm ngoài phạm vi của hàm (scope of function) mà có ảnh hưởng đến hàm đang được thực thi. Bất kì hoạt động nào không liên qua trực tiếp đến giá trị đầu ra của hàm được gọi là `Side Effect`.

Một vài trường hợp về `Side Effect`:

- Thực hiện request API.
- Tương tác với browsers API (`document` hoặc `window`).
- sử dụng `setTimeOut` hoặc `setInterval`.

Xem xét đến ví dụ sau

```js
/**
 * Find username in list of users
 * then update the DOM
 *
 * @param {Array<User>} users list of users
 * @param {string} name name of user that need to find
 *
 * @return undefined
 */
function findUser(users, name) {
  const reversedUsers = users.reverse();
  const found = reversedUsers.find((user) => user === name);

  document.getElementById("user-found").innerText = found;
}
```

`findUser` nhận vào 2 tham số, danh sách người dùng `users` và tên của người dùng cần kiếm `name`. Tìm kiếm từ cuối danh sách bằng `reverse`. Khi kiếm được thì nó cập nhật vào phần tử HTML sử dụng `DOM Api`.

Ở đây chúng ta đã phá vỡ 2 nguyên tắc thiết yếu của `Pure Function`:

1. Thay đổi giá trị đầu vào (users).
2. Truy vấn và thay đổi DOM.

Vậy các vấn đề có thể thấy được ở đây là gì? Hãy thử gọi hàm theo cách sau.

```js
let users = ["Tí", "Sửu", "Dần", "Mão"];
findUser(users, "Tí");
```

Tại thời điểm này, liệu người khác dùng hàm này có biết được hàm đang thực hiện thao tác với DOM trừ khi vào hàm `findUser` và đọc code ở trong đó. Vì vậy, tính dễ đọc không được đảm bảo.

Ngoài ra, chúng ta đã thay đổi luôn giá trị đầu vào. Lí tưởng nhất là ta nên clone giá trị đó và thực hiện thay đổi trong hàm.

```js
function findUser(users, name) {
  // cloned array of users
  const clonedUsers = [...users];

  // mutated (reverse) it
  const reversedUsers = clonedUsers.reverse();

  // find the element in new aray
  const found = reversedUsers.find((user) => user === name);

  // return found element
  return found;
}
```

Bây giờ thì `findUser` là một `Pure Function`. Ta đã loại bỏ được `Side Effect` và nó đã trả về đầu ra dự định. Đảm bảo dễ đọc, có thể kiểm thử, tái sử dụng nhiều nơi và đầu ra có thể dự đoán được.

## Tóm lại

Các khái niệm như `Pure Function` giúp giảm thiểu các `Side Effect` sẽ giúp chúng ta code tốt hơn, dễ quản lí và bảo trì. Qua đó giảm thiểu bugs, nhanh chóng tìm ra vấn đề, tăng khả năng tái sử dụng và kiểm thử.

## Tham khảo

[Pure Function và Side Effect với ví dụ](https://blog.greenroots.info/what-are-pure-functions-and-side-effects-in-javascript#heading-pure-functions-and-side-effects-with-examples)

[Định nghĩa về hàm - Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions)

[First-Class Function Mozilla](https://developer.mozilla.org/en-US/docs/Glossary/First-class_Function)

[First-Class Citizen GeeksForGeeks](https://www.geeksforgeeks.org/what-is-first-class-citizen-in-javascript)
