## 如何实现路由和导航

让我们来看看100行代码下的自定义路由解决方案的方式。

首先, 你需要实现一个 **应用 routes 列表**
其中每个路由可以表示为具有路径 `path` (参数化的URL路径字符串) 属性的对象,
`action`(一个方法),
以及一个可选 `children` (子路由的列表，每个子路由是路由对象).

该 `action` 防范返回类型为 anything - 字符串, React Component. 等.
例如下面的案例

#### `src/routes/index.js`

```js
export default [
  {
    path: '/tasks',
    action() {
      const resp = await fetch('/api/tasks');
      const data = await resp.json();
      return data && {
        title: `To-do (${data.length})`,
        component: <TodoList {...data} />
      };
    }
  },
  {
    path: '/tasks/:id',
    action({ params }) {
      const resp = await fetch(`/api/tasks/${params.id}`);
      const data = await resp.json();
      return data && {
        title: data.title,
        component: <TodoItem {...data} />
      };
    }
  }
];
```

Next, implement a **URL Matcher** function that will be responsible for matching a parametrized
下一步, 实现一个 **URL 匹配** 的函数, 它负责将参数化的路径字符串与实际URL匹配.
例如: 当 `matchURI('/tasks/:id', '/tasks/123')` 返回 `{ id: '123' }` ,
当 `matchURI('/tasks/:id', '/foo')` 返回 `null`  

幸好, 这里可以使用一个很棒的库 [`path-to-regexp`](https://github.com/pillarjs/path-to-regexp)
用它实现起来非常轻松.
这里是一个URL匹配器功能的案例:

#### `src/core/router.js`

```js
import toRegExp from 'path-to-regexp';

function matchURI(path, uri) {
  const keys = [];
  const pattern = toRegExp(path, keys); // TODO: Use caching
  const match = pattern.exec(uri);
  if (!match) return null;
  const params = Object.create(null);
  for (let i = 1; i < match.length; i++) {
    params[keys[i - 1].name] =
      match[i] !== undefined ? match[i] : undefined;
  }
  return params;
}
```


最后, 实现一个 **Route Resolver** 方法 提供一个 routes 列表和 URL/上下文 应该找到匹配提供的URL字符串的第一个路由,
执行它的 action 方法,
如果 这个 action 方法返回 任何非 `null` 或者 `undefined` 返回给调用者.  

除此之外, 它应该继续迭代剩余的路由.
如果没有匹配到提供的URL字符串的路由，它应该抛出一个异常 (Not found).
例如下列这个函数:  

#### `src/core/router.js`

```js
import toRegExp from 'path-to-regexp';

function matchURI(path, uri) { ... } // See above

async function resolve(routes, context) {
  for (const route of routes) {
    const uri = context.error ? '/error' : context.pathname;
    const params = matchURI(route.path, uri);
    if (!params) continue;
    const result = await route.action({ ...context, params });
    if (result) return result;
  }
  const error = new Error('Not found');
  error.status = 404;
  throw error;
}

export default { resolve };
```

这里是个使用示例:

```js
import router from './core/router';
import routes from './routes';

router.resolve(routes, { pathname: '/tasks' }).then(result => {
  console.log(result);
  // => { title: 'To-do', component: <TodoList .../> }
});
```

虽然现在可以在服务器上使用它, 在浏览器环境中它必须结合客户端解决方案.
可以使用 npm 模块 [`history`](https://github.com/ReactTraining/history) 为你处理此事.
它和其他库一样在 React Router, 一种 [HTML5 History API](https://developer.mozilla.org/docs/Web/API/History_API)
的封装，用于处理与客户端导航相关的所有棘手的浏览器兼容性问题.

首先, 创建 `src/core/history.js` 文件 将初始化一个新的历史模块实例并导出为单例:

#### `src/core/history.js`

```js
import createHistory from 'history/lib/createBrowserHistory';
import useQueries from 'history/lib/useQueries';
export default useQueries(createHistory)();
```

Then plug it in, in your client-side bootstrap code as follows:

#### `src/client.js`

```js
import ReactDOM from 'react-dom';
import history from './core/history';
import router from './core/router';
import routes from './routes';

const container = document.getElementById('root');

function renderRouteOutput({ title, component }) {
  ReactDOM.render(component, container, () => {
    document.title = title;
  });
}

function render(location) {
  router.resolve(routes, location)
    .then(renderRouteOutput)
    .catch(error => router.resolve(routes, { ...location, error })
    .then(renderRouteOutput));
}

render(history.getCurrentLocation()); // render the current URL
history.listen(render);
```

每当一个新的 location 推入 `history` stack 中, 该 `render` 方法会被调用,
它本身调用 这个 router 的 `resolve()` 方法 并将从React组件返回到DOM中。

为了触发客户端导航而不导致整页刷新, 你需要使用 `history.push()` 方法,
例如:

```js
import React from 'react';
import history from '../core/history';

class App extends React.Component {
  transition = event => {
    event.preventDefault();
    history.push({
      pathname: event.currentTarget.pathname,
      search: event.currentTarget.search
    });
  };
  render() {
    return (
      <ul>
        <li><a href="/" onClick={this.transition}>Home</a></li>
        <li><a href="/one" onClick={this.transition}>One</a></li>
        <li><a href="/two" onClick={this.transition}>Two</a></li>
      </ul>
    );
  }
}
```

虽然，通常的做法是将转换功能提取为独立（`Link`）Component，可以如下使用：

```html
<Link to="/tasks/123">View Task #123</Link>
```

### React Starter Kit 中的路由 [Universal Router](https://github.com/kriasoft/universal-router)

React Starter Kit (RSK) 使用 [Universal Router](https://github.com/kriasoft/universal-router) 通用 Router 的 npm module
它是围绕着前面演示的相同概念构建的,
它支持嵌套路由的主要区别, 并为您提供了帮助
`Link` React Component.
它可以看作是一个轻量级更灵活的替代React路由。

- 它以最小的依赖简单的代码 (只有 `path-to-regexp` 和 `babel-runtime`)
- 它可以与任何JavaScript框架（如React，Vue.js等）一起使用
- 它使用在Express和Koa中使用的相同的中间件方法，使其易于学习
- 它使用完全相同的API和实现 在Node.js和浏览器环境中

The [Getting Started page](https://github.com/kriasoft/universal-router/blob/master/docs/getting-started.md)
has a few examples how to use it.

### Related Articles

- [You might not need React Router](https://medium.freecodecamp.com/you-might-not-need-react-router-38673620f3d) by Konstantin Tarkus

### Related Projects

- [`path-to-regexp`](https://github.com/pillarjs/path-to-regexp)
- [`history`](https://github.com/ReactTraining/history)
- [Universal Router](https://github.com/kriasoft/universal-router)

### Related Discussions

- [How to Implement Routing and Navigation](https://github.com/kriasoft/react-starter-kit/issues/748)
- [How to Add a Route to RSK?](https://github.com/kriasoft/react-starter-kit/issues/754)
