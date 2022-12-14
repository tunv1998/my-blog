---
title: "Tìm hiểu về BFF"
description: ""
date: 2022-12-14T22:04:53+07:00
draft: true
categories:
  - "design patterns"
  - "Cloud Design Patterns"
tags:
  - bff
  - architecture
  - "cloud design patterns"
image: "intro.jpeg"
---

BFF là gì? kiếm nhanh trên Google thì ta thấy là:

![BFF đây nè, lol](bff.png)

Đùa thôi, hôm nay ta sẽ tìm hiểu về [BFF - Backends for Frontends](https://learn.microsoft.com/en-us/azure/architecture/patterns/backends-for-frontends). Một design pattern giúp ta tránh việc việc custom quá nhiều 1 API cho các response khác nhau.Tác giả của pattern này là [Sam Newman - Sam Người Mới](https://twitter.com/samnewman). Các bạn có thể đọc bài viết của tác giả [tại đây](https://samnewman.io/patterns/architectural/bff/).

## Vấn đề

Tiêu biểu, 1 hệ thống BE được phát triển song song nhằm cung cấp các tính năng cần thiết cho UI đó. Khi người dùng tăng lên, một ứng dụng di động được phát triển phải tương tác cùng với 1 BE. BE sẽ trở thành BE đa dụng, phục vụ cả desktop và mobile.

Nhưng khả năng của thiết bị di động khác biệt đáng kể so với trình duyệt trên máy tính để bàn, về kích thước màn hình, hiệu suất và giới hạn hiển thị. Do đó, các yêu cầu BE dành cho thiết bị di động khác với giao diện người dùng web trên máy tính để bàn.

Thông thường, các team khác nhau sẽ hoạt động trên các giao diện khác nhau (web và mobile). Sẽ gây ra hiện tượng ngẽn cổ chai cho team BE. Xung đột yêu cầu và giữ cho service hoạt động tốt cho cả 2 hệ máy. Gây ra tốn tài nguyên triển khai.

![src: https://learn.microsoft.com/en-us/azure/architecture/patterns/backends-for-frontends](https://learn.microsoft.com/en-us/azure/architecture/patterns/_images/backend-for-frontend.png)

## Giải pháp

Tạo ra mỗi BE cho mỗi UI, tinh chỉnh hành vi để phù hợp nhất với nhu cầu của mỗi môi trường mà không lo ảnh hưởng đến các trải nghiệm khác.

![src: learn.microsoft](https://learn.microsoft.com/en-us/azure/architecture/patterns/_images/backend-for-frontend-example.png)

Và bởi vì mỗi BE sẽ tương ứng với 1 UI, ta có thể tối ưu cho UI đó. Và kết quả là, nó sẽ nhỏ hơn, ít phức tạp hơn và nhanh hơn 1 BE đa dụng cho tất cả UIs.

## Các vấn đề cần lưu tâm

- Có bao nhiêu BE cần được deploy.
- Nếu UIs khác nhau mà vẫn cần cùng loại data hoặc cần cùng 1 request. Thì ta xem có cần để implement BFF không hay chỉ cần 1 là đủ.
- Code trùng lặp nhiều.
- Chỉ nên tập trung vào logic và hành vi. Business logic chung và vài features global nên quản lí ở nơi khác.
- Hãy suy nghĩ về cách mô hình này có thể được phản ánh trong trách nhiệm của một nhóm phát triển.
- Xem xét sẽ mất bao lâu để thực hiện mô hình này. Nỗ lực xây dựng các chương trình phụ trợ mới có phát sinh nợ kỹ thuật không, trong khi bạn tiếp tục hỗ trợ chương trình phụ trợ chung hiện có?

## Khi nào implement?

- BE phải được bảo trì với 1 chi phí đáng kể (thường được dùng trong sản phẩm product).
- Tối ưu BE cho các yêu cầu UI khác nhau.
- Tùy chỉnh cho BE cho nhiều UIs (if - else cho các màn hình).

Không nên dùng pattern này khi:

- Khi nhiều màn hình tạo cùng 1 request cho BE.
- Khi chỉ có 1 giao diện giao tiếp với BE
