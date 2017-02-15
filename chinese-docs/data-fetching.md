## 使用 WHATWG Fetch 获取数据

同构的 `core/fetch` 模块可以在客户端和服务器端代码中使用相同的方式,
如下所示：

```jsx
import fetch from '../core/fetch';

export const path = '/products';
export const action = async () => {
  const response = await fetch('/graphql?query={products{id,name}}');
  const data = await response.json();
  return <Layout><Products {...data} /></Layout>;
};
```

当这个代码在客户端上执行时,
Ajax请求将通过GitHub的[fetch](https://github.com/github/fetch)库（whatwg-fetch）发送请求，
它本身在场景后面使用XHMLHttpRequest，除非用户的浏览器本地支持 `fetch` .

每当在服务器上执行相同的代码,
它使用[node-fetch](https://github.com/bitinn/node-fetch) 模块本身是通过 NODE.js 的
`http` 模块发送的 HTTP 请求.
它还将相对 URL 转换为 绝对路径(详情 `./core/fetch/fetch.server.js`).

`whatwg-fetch` 和 `node-fetch` 模块具有几乎相同的API.
如果您是这个API的新手
, 下面的文章可能给你一个很好的介绍:

https://jakearchibald.com/2015/thats-so-fetch/
