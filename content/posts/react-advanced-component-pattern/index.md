---
title: "React Advanced Component Pattern"
description: "Design Pattern là những giải pháp, khuôn mẫu đã được tối ưu hóa, nhằm giải quyết các vấn đề thường gặp trong lập trình"
date: 2022-12-09T22:54:33+07:00
draft: true
categories:
  - react
  - "design pattern"
  - "react design pattern series"
tags:
  - react
  - "design pattern"
---

Đây là bài viết tìm hiểu về `React Component Pattern` nằm trong chuỗi series [React Design Pattern Series](/categories/react-design-pattern-series)

# Overview

Phạm vi series này sẽ chuyên về các `Design Pattern cho React Component`. Cũng như tìm hiểu tổng quan về `Design Pattern` trong các ngôn ngữ OOP như `C, Java, C#, PHP, ...`.

## Design Pattern là gì?

> Theo Reafactoring Guru:
>
> Design patterns are typical solutions to common problems in software design. Each pattern is like a blueprint that you can customize to solve a particular design problem in your code.

Nôm na, `Design Pattern` là những giải pháp, khuôn mẫu đã được tối ưu hóa, nhằm giải quyết các vấn đề thường gặp trong lập trình.

`Design Pattern` không phụ thuộc vào một ngôn ngữ cố định, nó chỉ là khuôn mẫu (blurprint) để chúng ta có thể dựa vào đó mà áp dụng vào code base.

Qua đó, code của chúng ta sẽ đảm bảo tính dễ đọc, dễ bảo trì cũng như bao hàm được các test case.

## Công dụng của Design Pattern

- `Đem lại giải pháp tối ưu`: với mỗi vấn đề ta sẽ có nhiều phương pháp khác nhau để giải quyết, áp dụng Design Pattern sẽ giúp chúng ta có được phương pháp tối ưu nhất, đảm bảo theo đúng best practice.

- `Tái sử dụng`: vì DP đã có sẵn khuôn mẫu, nên ta có thể áp dụng vào và dễ dàng mở rộng, bảo trì cũng như tái sử dụng ở nhiều nơi mà vẫn có thể đảm bảo được ảnh hưởng đến code base là nhỏ nhất.

- `Tăng tốc độ phát triển`: DP cung cấp giải pháp ở dạng tổng quát, tăng tốc độ phát triển phần mềm.

- `Bảo trì`: đọc code và fix bug sẽ dễ hơn nếu áp dụng DP.

_**Túm cái váy lại:** Ứng dụng là một cuốn bài tập toán, những gì Lập Trình Viên làm sẽ là giải hết các bài tập trong đó, và Design Patterns sẽ là phần hướng dẫn, hắn đưa ra các giải pháp để chúng ta có thể xem và ấp dụng vào bài giải, thế thôi._

## Lý do để tìm hiểu Design Pattern

Thật ra, chúng ta có thể làm việc bình thường mà chẳng cần quan tâm đến Design Pattern hay biết nó là cái gì. Vậy chúng ta biết nó để làm gì?

- Design Pattern là bộ công cụ đã được thử và kiểm tra (test) với các vấn đề thường gặp trong lập trình, nó sẽ giúp bạn hướng giải quyết các vấn đề sử dụng nguyên lí lập trình hướng đối tượng.

- Giúp ta giao tiếp và giải quyết trong team tốt hơn và đảm bảo rằng mọi người sẽ hiều, thay vì đi vào giải thích cách tiếp cận, test case, ... ta chỉ cần nói "Xời, dùng Singleton đi", và mọi người sẽ hiểu ý tưởng đằng sau nó.

## Phân Loại

Design Pattern có tổng cộng 23 loại (Gang of Four) và được phân loại thành 3 nhóm chính:

- Khởi Tạo (Creational Patterns): cách mà Object được tạo nên.
- Cấu Trúc (Structure Patterns): mối quan hệ | liên kết giữa các Object với nhau.
- Hành Vi (Behavior Patterns): cách mà Object giao tiếp với các Object khác.

## Nhược điểm

Mặc dù có nhiều công dụng, nhưng nhân vô thập toàn và Design Pattern cũng có các nhược điểm như:

- Design Pattern chỉ là các giải pháp tổng quát, một trong nhiều cách để giải quyết các vấn đề, vì vậy nên tỉnh táo và linh hoạt áp dụng các cách khác nhau.

- Khiến thiết kế trong code base chúng ta trở nên phức tạp và khó debug hơn.

## Tham Khảo

[Refactoring Guru](https://refactoring.guru/design-patterns)

[Pattern.dev](https://www.patterns.dev/posts/)

[React Pattern](https://reactpatterns.js.org/docs/)

[Design Pattern - Viblo](https://viblo.asia/p/design-patterns-phan-1-tong-quan-ve-design-pattern-LzD5dWpeljY)
