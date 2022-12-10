---
title: "Imperative Programming và Declarative Programming"
description: "Imperative và Declarative Programming là gì? Hãy cùng tìm hiểu."
date: 2022-12-11T02:45:23+07:00
draft: false
tags:
  - javascript
  - react
categories:
  - programming
image: "intro.png"
---

Gần đây khi đọc vài tài liệu và bài viết mình có thấy lại khái niệm `Imperative Programming` và `Declarative Programming`, bỗng dưng thấy kiến thức này hơi hổng dù đã có nghía qua nó một lần. Nên mình vội vàng làm ngay một post để hâm lại kiến thức.

> Imperative vs Declarative Programming
>
> Imperative là cách bạn làm một thứ.
>
> Declarative là cái mà bạn muốn.

## Uhm, vậy nó là cái quái gì (!?)

Định nghĩa thì rắc rối, vậy nên mình đưa ra ví dụ cho dễ hình dung.

_Bạn có một cuộc hẹn cho buổi pạt ty cuối tuần, và bạn nhắn: "Tao tới CircleK rồi, làm sao tới chỗ mày đây?"_

**Imperative**: Đi thẳng 100m. Tới đường Nguyễn Văn A, rẽ trái. Đi thẳng đến đường Phan Văn B đi thêm 200m nữa thì rẽ vào hẻm 213, tới nhà có số 12.

**Declarative**: Nhà t ở 213/12 Phan Văn B, Google Maps đi con giai.

_Cả 2 cách đều chỉ bạn đến nơi cần đến. Khác biệt ở chỗ, một cách sẽ cho bạn chỉ dẫn để có thể đến nơi, cách còn lại thì mong chị Google không dẫn bạn đi quá xa._

Với cách tiếp cận `Imperative` thì bạn sẽ có được một chỉ dẫn cụ thể, làm cách nào mà bạn đến được địa chỉ đó (how).

Với `Declarative` thì sẽ cho bạn cái bạn muốn (what) - một địa chỉ và mặc định bạn sẽ biết cách kiếm ra được đường đi.

Ta có thể thấy được các ngôn ngữ đang chia theo cách tiếp cận nào:

**Imperative**

- C
- C++
- Java

**Declarative**

- SQL
- HTML

**Both**

- Javascript
- C#
- Python

## Các ví dụ

```SQL
SELECT * FROM Users WHERE Country='Viet Nam'
```

Lướt qua ví dụ bạn có thể thấy và hiểu ngay bạn đang cần _cái gì_ mà không cần quan tâm đến SQL lấy _bằng cách nào_ hay _bằng thuật toán nào._

> Bạn sẽ lấy được thông tin người dùng từ table `Users`, họ có quốc tịch `Viet Nam`

```js
function sum(a, b) {
  return a + b;
}
```

Rõ ràng, đây là cách tiếp cận `Imperative`, bạn khai báo một hàm, đặt tên cho nó, trong hàm có 2 tham số `a, b` và nó sẽ trả về cho bạn tổng của 2 số đó.

Giả sử mình có yêu cầu sau

_Viết 1 hàm tên là `add`, nhận vào một mảng số nguyên và trả về tổng của các phần tử trong đó._

Và vì `Javascript` là ngôn ngữ có cả `Imperative` và `Declarative` nên ta hoàn toàn có 2 cách tiếp cận nó như sau:

```js
// Imperative
function add(arr) {
  let result = 0;
  const len = arr.length;

  for (let i = 0; i < len; i++) {
    result += arr[i];
  }

  return result;
}
```

```js
// Declarative
function add(arr) {
  return arr.reduce((total, current) => (total += current), 0);
}
```

## Tổng kết

**Imperative**

- Quen thuộc
- Dễ học
- Có tính logic

Đây là cách tiếp cận chúng ta được được học từ những ngày đầu, mô tả cách thực hiện. Logic được thể hiện rõ ra bên ngoài.

**Declarative**

- Hạn chế sự thay đổi
- Giảm side-effects
- Code ngắn và dễ đọc hơn

Do không phải viết lại các bước logic, nên code sẽ rõ ràng hơn và dễ đọc hơn. Nhưng Logic lại bị ẩn đi (abstract).
