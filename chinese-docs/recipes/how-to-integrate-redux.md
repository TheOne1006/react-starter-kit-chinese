## 如何引入 [Redux](http://redux.js.org/index.html)

通过 Git 合并 `feature/redux` 分支.
如果你也对 `feature/react-intl` 感兴趣,
合并该分支，因为它也包括Redux.

**如果你对 Redux 不了解,你可以 [先看看这里](http://redux.js.org/docs/basics/index.html).**


### 创建 Actions

 1. 在 `src/constants/index.js` 定义 action 名

 2. 在 `src/actions/` 目录中创建适合的文件名. 你可以用 `src/actions/runtime.js` 作为一个模板.

 3. 如果你需要 async 的 actions, 使用 [`redux-thunk`](https://github.com/gaearon/redux-thunk#readme).
    关于如何创建 async actions 可以参考
    [`setLocale`](https://github.com/kriasoft/react-starter-kit/blob/feature/react-intl/src/actions/intl.js)
    action 来自 `feature/react-intl`.
    查看这篇文章 [Async Flow](http://redux.js.org/docs/advanced/AsyncFlow.html).


### 创建 Reducer (aka Store)

 1. 在 [`src/reducers/`](https://github.com/kriasoft/react-starter-kit/tree/feature/redux/src/reducers) 创建相关文件.

    可以把 [`src/reducers/runtime.js`](https://github.com/kriasoft/react-starter-kit/tree/feature/redux/src/reducers/runtime.js) 做为一个模板.

    - 不要忘记任何时候都要返回 `state`.
    - 不要改变原有的 `state`.
      如果你改变状态，连接组件的渲染将不会被触发，因为 `===` 相等.
      如果执行状态更新，则始终返回新值.
      你可以使用这个结构: `{ ...state, updatedKey: action.payload.value, }`
    - 请记住, Store State *必须* 是可重放的 action.
      例如, 当你存储时间戳时, 传递到 *action 的 payload*.
      当你调用REST API, 在 action 中处理这件事. *不要在 reducer 处理!*

 2. 编辑 [`src/reducers/index.js`](https://github.com/kriasoft/react-starter-kit/tree/feature/redux/src/reducers/index.js), 导入 你的 reducer 添加到 root reducer 通过
 [`combineReducers`](http://redux.js.org/docs/api/combineReducers.html) 创建


### Connecting(连接) Components

你可以使用 [`connect()`](https://github.com/reactjs/react-redux/blob/master/docs/api.md#connectmapstatetoprops-mapdispatchtoprops-mergeprops-options) 创建高阶 Component 来自于 [`react-redux`](https://github.com/reactjs/react-redux#readme) package 包.

查看 [Usage With React](http://redux.js.org/docs/basics/UsageWithReact.html) redux.js.org.

查看案例
[`<LanguageSwitcher>`](https://github.com/kriasoft/react-starter-kit/blob/feature/react-intl/src/components/LanguageSwitcher/LanguageSwitcher.js)
component 在 `feature/react-intl` 分支中.它演示了订阅存储和 dispatching actions.


### Dispatching Actions On Server(在服务器端调度操作)

详情查看 `src/server.js`
