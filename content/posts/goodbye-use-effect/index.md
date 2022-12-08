---
title: "Goodbye useEffect"
date: 2022-12-07T15:14:59+07:00
draft: false
description: "Tại sao bạn chẳng cần useEffect nhiều đến thế? được chia sẻ bởi David Khourshid tại ReactNext 2022"
categories:
  - react
  - hooks
tags:
  - hooks
  - useEffect
  - react
image: "gb-intro.png"
---

<em>`useEffect` - một hook khá quen thuộc đối với anh em khi xây dựng ứng dụng với React, và trong đa số trường hợp. Chúng ta đang dùng nó sai cách, vậy sai như thế nào và sửa làm sao?</em>

## Vậy useEffect là gì?

Theo [document của React thì](https://beta.reactjs.org/apis/react/useEffect): `useEffect` là 1 React Hook **cho phép ta đồng bộ component với 1 hệ thống bên ngoài.**

```js
useEffect(setup, dependencies?)
```

<!-- hm, vậy đồng bộ cái gì vào Component? và hệ thống bên ngoài sẽ ra làm sao? -->

các bạn có thể vào document để xem các ví dụ cụ thể mà ta có thể đồng bộ được vào các hệ thống bên ngoài.

## useEffect có phải là 1 lifecycle hook ?

> Câu trả lời ngắn gọn: Không, đầy đủ: Không.

Lạ ha, thường thì ta sẽ nghĩ nó như thế này

```js
useEffect(() => {
  // componentDidMount?
}, [])

useEffect(() => {
  // componentDidUpdate?
}, [something, in, the, way])

useEffect(() => {
  return () => {
    // componentWillUnMount?
  }
}, [])
```

và tác giả [Dan Abramov](https://twitter.com/dan_abramov?ref_src=twsrc%5Egoogle%7Ctwcamp%5Eserp%7Ctwgr%5Eauthor) cũng đã xác nhận rằng useEffect không phải là một lifecycle hook, nó hoạt động một cách đồng bộ.

Vậy chính xác nó là cái gì?

## Nhìn lại useEffect

Đây là cấu trúc của 1 useEffect đơn giản

```js
useEffect(() => {
  doSomething(); // effect
}, [whenever, these, thing, change]); // dependencies array
```

Ta có 2 cách tiếp cận sau đây

- Imperative: cách mà mọi thứ vận hành, hay logic
- Declarative: cách mà mọi thứ trông ra sao, hay giao diện

| Imperative                   | Declarative                                                                                                                                                 |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Khi có thay đổi, chạy effect | Khi có thay đổi, thực hiện thay đổi trong state, và phụ thuộc vào state nào thay đổi (dependencies), effect sẽ được chạy. Nhưng chỉ khi thỏa mãn điều kiện. |

Rắc rối phải không, nếu là bạn thì bạn sẽ chọn cách nào. Rõ ràng là theo `Imperative`.

```js
// Ideal
useEffect(() => {
  doSomething();

  return () => {
    cleanup();
  };
}, [whenThisChange]);

// Reality
useEffect(() => {
  if ((foo && bar) || foo || bar) {
    doSomething();
  } else {
    doSomethingElse();
  }
}, [foo, bar]);
```

Khai thật đi, các bạn có thấy mình trong đó không nào. (Cả mình nữa =]]z)

Tóm lại: mục đích chúng ta dùng useEffect và muốn nó thay đổi **khi có thứ gì đó xày ra** chứ không phải là **khi có thứ gì đó thay đổi**.

`when things happend NOT when things changed`

### useEffect run twice

Quay lại vấn đề một chút, ta biết đối với Reactv18 ở môi trường development, mọi thứ sẽ chạy 2 lần - vì chúng ta có StrictMode

**Tại sao điều này lại xảy ra?**

Bởi vì trong Strictmode, đối với useEffect, React sẽ giả lập 1 lần `unMount` để chạy `cleanup` và sau đó `reMount` lại để chạy effect.

có thể tham khảo với sơ đồ sau

| Mount       | Unmount (giả lập) | Remount     |
| ----------- | ----------------- | ----------- |
| chạy effect | chạy cleanup      | chạy effect |

## Các ví dụ refactor code với useEffect

### 1. Biến đổi dữ liệu

`useEffect => useMemo`

```js
// before
const [items, setItems] = useState([]);
const [total, setTotal] = useState(0);

useEffect(() => {
  setTotal(() =>
    items.reduce((currentTotal, item) => currentTotal + item.price, 0)
  );
}, [items]);

// after
const totalMemo = useMemo(
  () => items.reduce((currentTotal, item) => currentTotal + item.price, 0),
  [items]
);
```

### 2. Giao tiếp với component cha

`useEffect => eventHandler`

```jsx
// before
const Modal = ({ onOpen, onClose }) => {
  const [isOpen, setIsOpen] = useState(false);

  useEffect(() => {
    if (isOpen) onOpen();
    else onClose();
  }, [isOpen]);

  return (
    <div>
      <button onClick={() => setIsOpen(!isOpen)}>Button</button>
    </div>
  );
};

// after
const Modal = ({ onOpen, onClose }) => {
  const [isOpen, setIsOpen] = useState(false);

  const toggleView = () => {
    const nextIsOpen = !isOpen;
    setIsOpen(!isOpen);

    if (nextIsOpen) onOpen();
    else onClose();
  };

  return (
    <div>
      <button onClick={toggleView}>Button</button>
    </div>
  );
};
```

### 3. Subscribing to external stores

Vì dịch sang Tiếng Việt khá chuối nên mình để nguyên bản Tiếng Anh.

`useEffect => useSyncExternalStore`

Về `useSyncExternalStore` thì mình sẽ có một bài sau để tìm hiểu nó. Các bạn có thể tìm hiểu thêm về [useSyncExternalStore](https://beta.reactjs.org/apis/react/useSyncExternalStore).

```jsx
// before
const Store = () => {
  const [isConnected, setIsConnected] = useState(false);

  useEffect(() => {
    const sub = storeApi.subscribe(({ status }) => {
      setIsConnected(status === "connected");
    });

    return () => {
      sub.unsubscribe();
    };
  }, []);

  return <></>;
};

// after
const Store = () => {
  const isConnected = useSyncExternalStore(
    // subscribe
    storeApi.subscribe,
    // get snapshot
    () => storeApi.getStatus() === "connected",
    // get server snapshot
    true
  );

  return <></>;
};
```

### 4. Khi fetch dữ liệu

`useEffect => renderAsYouFetch` (with Suspense)

Cái này thì quá quen thuộc rồi phải không =]]z

```jsx
const Component = () => {
  const [items, setItems] = useState([]);

  useEffect(() => {
    let isCanceled = false;

    try {
      getItems().then((res) => {
        if (isCanceled) return;
        setItems(res.items);
      });
    } catch (error) {
      /**
       * Handle error
       */
    }

    return () => {
      isCanceled = true;
    };
  }, []);

  return <></>;
};
```

Ta có thể đối chiếu với các frameworks khác như `Remix` có hook `useLoaderData()`, hoặc `NextJs` có `getServerSideProps()` để hạn chế việc dùng `useEffect()` hook cho việc fetch dữ liệu.

Đối với app React thuần, ta có thể xem xét các phương án thay thế như dùng các thư viện `caching` như `react-query`, `swr`, ...

Và theo như chia sẻ của tác giả, trong tương lai sẽ có một hook mới rất bá đạo, là `use()`.

### 5. Khởi tạo singletons pattern toàn cục

`useEffect => justCallIt`

Phần này mình vẫn còn khá mù mờ nên hẹn sau.

## Tóm lại

- `Effect` là state management.
- Đừng dùng `side-effect` cùng với nó. Thay vào đó, hãy quản lý `side-effect` bằng hook.

```jsx
const [state, event] = [nextState, effect];
```

- `useEffect` được dùng cho tác vụ đồng bộ.
- `state transition` sẽ kích hoạt `effects`
- `Effects` sẽ chạy trong `event handler`

## Tham khảo

[Video](https://www.youtube.com/watch?v=bGzanfKVFeU&ab_channel=BeJS)

[Slides dùng trong video](https://slides.com/davidkhourshid/goodbye-useeffect/fullscreen)

[You might not need an effect](https://beta.reactjs.org/learn/you-might-not-need-an-effect)
