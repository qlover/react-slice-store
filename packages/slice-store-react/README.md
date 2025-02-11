# slice-store-react
## 简介

slice-store 可以帮助您编写行为一致、在不同环境（客户端、服务器和本机）中运行且易于测试的 JavaScript 应用程序.

## 特性

- 简单易用的 API
- 支持复杂数据结构
- 高效的状态更新
- 支持选择器功能
- 完善的类型支持

## 安装

```bash
npm install @qlover/slice-store-react
# or use yarn
yarn add @qlover/slice-store-react
```

## 使用示例

### 基本使用

```tsx
import { SliceStore } from '@qlover/slice-store';
import { useSliceStore } from '@qlover/slice-store-react';
import './App.css';

type Value = {
  count: number;
};
class AppStore extends SliceStore<Value> {
  constructor() {
    super(() => ({ count: 1 }));
  }

  inc = () => {
    this.emit({ count: this.state.count + 1 });
  };
}

const appStore = new AppStore();

function App() {
  const { count } = useSliceStore(appStore);

  return (
    <>
      <h1>React Slice Store</h1>
      <div className="card">
        <button onClick={appStore.inc}>count is {count}</button>
      </div>
    </>
  );
}

export default App;
```

### 使用选择器

```tsx
import { SliceStore } from '@qlover/slice-store';
import { useSliceStore } from '@qlover/slice-store-react';

type User = {
  id: number;
  name: string;
  age: number;
};

class UserStore extends SliceStore<User> {
  constructor() {
    super(() => ({ id: 1, name: 'John Doe', age: 30 }));
  }

  updateAge = (newAge: number) => {
    this.emit({ ...this.state, age: newAge });
  };
}

const userStore = new UserStore();

function UserAge() {
  const age = useSliceStore(userStore, state => state.age);

  return <div>User age: {age}</div>;
}

function UserInfo() {
  const { name, age } = useSliceStore(userStore);

  return (
    <div>
      <div>Name: {name}</div>
      <div>Age: {age}</div>
      <button onClick={() => userStore.updateAge(age + 1)}>Increment Age</button>
    </div>
  );
}
```

### 多个组件监听同一个状态

```tsx
import { SliceStore } from '@qlover/slice-store';
import { useSliceStore } from '@qlover/slice-store-react';

type CounterState = { count: number };

class CounterStore extends SliceStore<CounterState> {
  constructor() {
    super(() => ({ count: 0 }));
  }

  increment = () => {
    this.emit({ count: this.state.count + 1 });
  };
}

const counterStore = new CounterStore();

function CounterDisplay() {
  const { count } = useSliceStore(counterStore);
  return <div>Count: {count}</div>;
}

function IncrementButton() {
  useSliceStore(counterStore); // 监听状态变化以触发重新渲染
  return <button onClick={counterStore.increment}>Increment</button>;
}

function App() {
  return (
    <div>
      <CounterDisplay />
      <CounterDisplay /> {/* 两个组件同时显示并更新计数 */}
      <IncrementButton />
    </div>
  );
}
```

这些示例展示了 @qlover/slice-store-react 的灵活性和强大功能，包括基本用法、选择器的使用，以及多个组件如何共享和响应同一个状态。

## 测试

我们的测试套件涵盖了以下方面:

- 基本功能测试
- 复杂数据结构的处理
- 多组件同时监听状态变化
- 选择器功能的使用
- 边缘情况处理(空数组、未定义值等)

## 贡献

欢迎提交 issues 和 pull requests 来帮助改进这个项目。

## 许可证

ISC