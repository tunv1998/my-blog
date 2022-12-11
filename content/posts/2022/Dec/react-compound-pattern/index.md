---
title: "React Compound Pattern"
description: ""
date: 2022-12-11T22:44:16+07:00
draft: true
categories:
  - react
tags:
  - react
  - "react component pattern"
image: ""
---

> Quick Look
>
> Compound Component là một trong những React pattern nâng cao, sử dụng một các thú vị để giao tiếp giữa các component với nhau và chia sẻ các state ngầm bằng các tận dụng mối quan hệ cha-con.

## Định nghĩa

> Theo định nghĩa Tiếng anh thì:
>
> Compound components can be said to be a pattern that encloses the state and the behavior of a group of components but still gives the rendering control of its variable parts back to the external user.

Compound Pattern là một pattern mà đóng gói state và hành vi của một nhóm component, nhưng vẫn trao quyền kiểm soát render của các biến cho người dùng.

Định nghĩ là thế, giờ ta qua ví dụ để xem nó là thể nào. Giờ thì xem nó giống như là tag `select` và `option` trong HTML.

```html
<select>
  <option value="iphone">iPhone</option>
  <option value="nokia">Nokia</option>
  <option value="pixel">Pixel</option>
</select>
```

`select` và `option` sẽ hoạt động cùng với nhau,

- select sẽ quản lí trạng thái của UI.
- option thì đảm nhận cách mà dropdown list hoạt động.

=> Compound pattern sẽ giúp ta dựng `declarative UI` và tránh khỏi việc `Props Drilling`.

## Áp dụng

Compound pattern được áp dụng nhiều vào các UI packages, ví dụ như [Reach UI](https://reach.tech/).

```jsx
import {
  Menu,
  MenuList,
  MenuButton,
  MenuItem,
  MenuItems,
  MenuPopover,
  MenuLink,
} from "@reach/menu-button";
import "@reach/menu-button/styles.css";

function Example() {
  return (
    <Menu>
      <MenuButton>Actions</MenuButton>
      <MenuList>
        <MenuItem>Download</MenuItem>
        <MenuLink to="view">View</MenuLink>
      </MenuList>
    </Menu>
  );
}
```

**Notes** Compound Pattern có 2 cách để export Component, và 2 cách này hoàn toàn tự chọn dựa theo dự án bạn đang làm.

1. Tách bạch Child Component và Parent Component, như ví dụ trên.
2. Thêm Child Component vào phần export của Parent Component, ví dụ:

```jsx
function Example() {
  return (
    <Menu>
      <Menu.MenuButton>Actions</Menu.MenuButton>
    </Menu>
  );
}
```

## Vậy, khi nào nên sử dụng

Bạn sẽ cần Pattern này khi:

- Xử lí các vấn đề liên quan đến tái sử dụng UI.
- Phát triển các thành phần có tính liên kết cao với liên kết ít nhất.
- Đây là một các tốt để chia sẻ state giữa các component với nhau, thay vì phụ thuộc hoàn toàn vào `render props`.

## Ưu nhược điểm

### Ưu

1. Chia để trị (Separation of Concerns): nhờ vào việc chia UI logic ở parent component và ui ở child component, tạo ra sự phân chia nhiệm vụ rõ ràng.

2. Giảm độ phức tạp: vì props được truyền qua `ContextApi` nên tránh được việc `Props Drilling` qua đó giúp code dễ đọc và bảo trì.

3. Cấu trúc linh hoạt: Bởi vì các phần UI được chia nhỏ ra thành nhiều phần nên ta có thể linh hoạt thay đổi UI giữa chúng, tưởng tượng nó giống như một khối Lego.

### Nhược

1. UI quá linh hoạt: sẽ tạo ra vài trường hợp không mong muốn, ví dụ như code UI lẫn vào hoặc khó để implement với các part ngoài.

2. JSX nặng: vì chia nhỏ ra thành từng thành phần, mới đầu ta có thể thấy nó rất dễ đọc, nhưng nhìn toàn cảnh thì lại là một câu chuyện khác.

```jsx
function App() {
  return (
    <Counter>
      <Label label="Total Count" />
      <IncreaseBtn />
      <DecreaseBtn />
    </Counter>
  );
}

function App() {
  return (
    <Counter
      label="Total Count"
      count={count}
      onIncrease={onIncrease}
      onDecrease={onDecrease}
    />
  );
}
```

## Tổng kết

Tận dụng Compound components chúng ta còn có thể tạo ra những highly customized , reusable components và clean component APIs cho khi sử dụng. Giúp chúng ta có thể tách riêng được business logic và rendering logic.

## Code mẫu

Bạn có thể xem tại [Github](https://github.com/tunv1998/react-advanced-component-pattern/tree/feature/compound-pattern), hoặc [Codesandbox](https://codesandbox.io/s/github/tunv1998/react-advanced-component-pattern/tree/feature/compound-pattern)

## Tham khảo

[Compound Components In React](https://www.smashingmagazine.com/2021/08/compound-components-react/)

[React Pattern: Compound components](https://viblo.asia/p/react-pattern-compound-components-1VgZv4aY5Aw)

[Understanding React compound components](https://blog.logrocket.com/understanding-react-compound-components/)
