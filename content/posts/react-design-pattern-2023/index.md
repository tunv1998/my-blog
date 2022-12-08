---
title: "React Design Patterns trong năm 2023"
date: 2022-12-07T21:14:59+07:00
draft: true
description: "Các design patterns trong React và các trường hợp dùng chúng."
categories:
  - React
  - "Design Patterns"
tags:
  - react
  - "design patterns"
keywords:
  - "design patterns"
  - react
  - hooks
image: "react-bg.png"
---

_Được dịch từ bài viết [React Design Patterns: You should know in 2023](https://aglowiditsolutions.com/blog/react-design-patterns/)_

> TLDR;
> React Design Patterns là các công cụ hỗ trợ cốt lõi của thư viện React, giúp developers tạo ra ứng dụng có độ phức tạp theo cách dễ bảo trì, linh hoạt và cải thiện hiệu năng.

## React Design Patterns là cái chi?

**Design Patterns** thường được dùng để giảm thời gian phát triển và công sức bằng các áp dụng các patterns đã được phát triển và kiểm tra để giải quyết các vấn đề thường gặp.

**React Design Patterns** thường được dùng để đơn giản hóa các ứng dụng React lớn, giảm đáng kể stress và làm việc trên các components khác nhau và chia sẻ logic giữa chúng.

[Design Patterns - Wikipedia](https://en.wikipedia.org/wiki/Design_Patterns)

[Design Patterns - Topdev](https://topdev.vn/blog/design-pattern-la-gi/)

## Dùng Design Patterns thì được lợi cái chi?

Không chỉ giúp tăng tốc thời gian thực hiện dự án mà còn giúp dự án dễ bảo trì và dễ đọc.

### 1. Cấu trúc dự án

React được biết đến bởi tính linh hoạt trong phát triển vì nó không áp dụng bất kì bộ quy tắc nào, không giống như các frameworks khác. Mặc dù nó mở ra khả năng vô hạn cho dev để kết hợp các phương phát phát triển khác nhau nhưng nó cũng là hạn chế khi có nhiều người trong dự án.

Design Patterns cung cấp các cấu trúc cần thiết cho nhóm phát triển. Nó cung cấp thuật ngữ tiêu chuẩn và giải pháp cho các vấn đề phổ biến. Bằng cách này, ta có thể đảm bảo rằng mọi dev sẽ biết nên tuân theo phương pháp nào để giải quyết vấn đề, tránh xung đột trong mã nguồn và tối ưu hóa toàn bộ quy trình.

### 2. Đảm báo theo sát best practices

Các frameworks được phát hành design patterns sau khi nghiên cứu và thử nghiệm. Điều này cũng đúng với React. Do đó, design patterns cho React giúp giảm bớt quá trình phát triển và đảm bảo tuân theo best practices.

Design Patterns là một công cụ mạnh mẽ cho chúng ta có thể tạo ra các ứng dụng độc đáo, mạnh mẽ và có khả năng mở rộng.

### 3. Hiều về Cross-Cutting Concerns (CCC) trong React

## Top React Design Patterns

Có rất nhiều Design Patterns cho React, tất cả chúng đều giúp ta giải quyết các vấn đề. Tuy nhiên, sẽ là bất khả thi nếu ra có thể cover tất cả mọi design patterns. Do đó, ta sẽ liệt kê ra 6 design patterns mà bạn nên biết trong năm 2023.

- The HOC Pattern
- The Provider Pattern
- Presentational & Container Component Pattern
- React Hooks Design Pattern
- Compound Component Pattern
- React Conditional Rendering Design Pattern

### 1. The HOC (Higer Order Component) Design Pattern

Nếu bạn là một nhà phát triển React có kinh nghiệm, rất có thể bạn sẽ nhận ra nhu cầu sử dụng cùng một logic trong các tình huống khác nhau, chẳng hạn như:

- Components có sử dụng `Third Party Subscription Data`
- Card Views với cùng 1 thiết kế (shadow, border, ...)
- App Components yêu cầu đăng nhập để xem data
- Infinity scroll với các view khác nhau và dữ liệu khác nhau

Tất cả vấn đề CCC trên sẽ được giải quyết với HOC.

HOC là 1 React design pattern thông dụng thường được dùng để chia sẻ logic giữa các components khác nhau mà không cần phải viết lại. HOC được xem là 1 `pure function` vì nó không có `side effect`.

**Structure**

```
const higherOrderComponent = (DecoratedComponent) => {
    class HOC extends Component {
        render(){
            return <DecoratedComponent />
        }
    }
}
```

**Các HOC thông dụng**

| 3rd party    | identify                                               |
| ------------ | ------------------------------------------------------ |
| react-router | withRouter(UserPage)                                   |
| react-redux  | connect(mapStateToProps, mapDispatchToProps)(UserPage) |
| material-ui  | withStyles(styles)(UserPage)                           |

### 2. The Provider Pattern

The Provider Pattern nhằm mục đích chia sẻ các dữ liệu toàn cục xuyên suốt components. Đi kèm với `<Provider/>` component chứa dữ liệu toàn cục, có thể truyền dữ liệu xuống `component tree` bằng cách dùng `Consumer Component / custom hooks`.

Để có thể hiểu cách Provider Pattern hoạt động. Ta cần hiểu về `React Context API`.

**Structure**

```
ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>
)
```

### 3. Presentational & Container Component Pattern

> Hay còn được gọi là SmartComponent và DumpComponent

Toàn bộ ý tưởng đằng sau React design pattern là tìm cách dễ hơn để tái sử dụng lại component. `Presentational` và `Container Component` hoạt động dựa trên niềm tin rằng sẽ dễ dàng để quản lý component nếu như chúng ta chia nó ra thành 2 nhóm - `Presentational` và `Container Component`. Vậy chúng khác nhau như thế nào?

| Presentational                                                                                          | Container Component                                                                           |
| ------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| mọi thứ nhìn như thế nào                                                                                | mọi thứ hoạt động ra sao                                                                      |
| Có thể bao gồm `presentational` và `container component`, thông thường sẽ có DOM và styles của chính nó | Có thể bao gồm `presentational` và `container component`, không có DOM và styles của chính nó |
| Ví dụ: Sidebar, Page, List, UserInfo                                                                    | Ví dụ: StoryContainer, UserList, FollowerSidebar                                              |

### 4. React Hooks Design Pattern

Hooks được giới thiệu vào phiên bản React 16.8, và chúng đem lại một cuộc cách mạng để xây dựng React Component. Sử dụng hooks cho phép React Functional Component có thể truy xuất các tính năng như `context, states, props, lifecycle và ref`. Tăng tiềm năng của functional component vì giờ đây chúng có khả năng thực hiện tác vụ như `class component`.

### 5. Compound Component Pattern

Một trong những React pattern phổ biến, giúp ta thiết lập giao thức liền mạch giữa component cha và con thông qua API. Với Compound Pattern, component cha có thể ngầm tương tác và chia sẻ state với component con. Nó cung cấp một phương thức hiệu quả cho nhiều components để chia sẻ state và quản lí logic.

Compound pattern phù hợp nhất để xây dựng apps giao diện.

Compound pattern là pattern mà bao đóng state và hành vi của 1 nhóm component trong khi cung cấp các điều khiển render của các biến cho người dùng.

**Ví dụ: `<select>` và `<option>`**

```
<select>
  <option>USA</option>
  <option>Japan</option>
  <option>Vietnam</option>
</select>
```

## Tham khảo

[Cross-Cutting Concerns](https://stackoverflow.com/questions/23700540/cross-cutting-concern-example)
