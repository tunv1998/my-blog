---
title: "React Control Props Pattern"
description: ""
date: 2022-12-12T03:15:21+07:00
draft: true
categories:
  - react
tags:
  - react
  - "react component pattern"
---

> Quick Look
>
> finish when done article

## Định nghĩa

> [Epic React](https://advanced-react-patterns.netlify.app/6)
>
> One liner: The Control Props pattern allows users to completely control state values within your component. This differs from the state reducer pattern in the fact that you can not only change the state changes based on actions dispatched but you also can trigger state changes from outside the component or hook as well.

`Control Props` là một pattern giúp chúng ta tạo ra một [controlled component](https://reactjs.org/docs/forms.html#controlled-components), cho phép ta thêm logic vào để sửa đổi các hành vi mặc định của component.

ví dụ

```jsx
function MyCapitalizedInput() {
  const [capitalizedValue, setCapitalizedValue] = React.useState("");

  return (
    <input
      value={capitalizedValue}
      onChange={(e) => setCapitalizedValue(e.target.value.toUpperCase())}
    />
  );
}
```

## Áp dụng

Tương tự như `input` trong React, ta cần có 2 props chính là `values` và `onChange`.

Code demo bạn có thể xem ở [Github]() hoặc [Codesandbox]()

## Ưu nhược điểm

### Ưu

- Cho nhiều quyền kiểm soát: vì ta có quyền kiểm soát state chính, nên ta có thể điều khiển về hành vi của state đó.

### Nhược

- Áp dụng phức tạp: trước đó, với [Compound Pattern](/posts/2022/dec/react-compound-pattern/), ta chỉ cần áp dụng vào 1 nơi, nhưng với cách implement này thì ta lại phân bổ nó ra 3 nơi (JSX, state, methods)
