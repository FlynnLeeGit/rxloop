# 多状态与单一状态树

```javascript
// 添加 user 模型
app.model({
  name: 'user',
  state: {
    name: 'wxnet',
    email: 'test@gmail.com',
  },
});

// 每一个模型会对应一个状态树，比如之前创建的 counter 模型。
// counter 状态树
const counter$ = app.stream('counter');
// user 状态树
const user$ = app.stream('user');

// 可以在不同的组件中，自由订阅模型状态的变更
counter$.subscribe(
  (state) => {
    // this.setState(state);
  },
  (error) => {},
);

user$.subscribe(
  (state) => {
    // this.setState(state);
  },
  (error) => {},
);

// 还可以使用 RxJS 的 combineLatest 工厂方法将多个模型合并为单一状态树。
const singleState = combineLatest(
  user$,
  counter$,
)
.pipe(
  map(([user, counter]) => {
    return {
      user,
      counter,
    };
  }),
);

singleState.subscribe(
  (state) => {
    // this.setState(state);
  },
  (error) => {},
);
```