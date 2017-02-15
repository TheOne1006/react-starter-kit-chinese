## 应用的测试

### 使用的库

RSK 附带以下库用于测试:

- [Mocha](https://mochajs.org/) - Node.js 和 browser 的测试环境
- [Chai](http://chaijs.com/) - 断言库
- [Enzyme](https://github.com/airbnb/enzyme) - 用于 React 测试工具

您还可以查看以下相关软件包:

- [jsdom](https://github.com/tmpvar/jsdom)
- [react-addons-test-utils](https://www.npmjs.com/package/react-addons-test-utils)

### 执行测试

要测试您的应用程序，只需运行
[`yarn test`](https://github.com/kriasoft/react-starter-kit/blob/b22b1810461cec9c53eedffe632a3ce70a6b29a3/package.json#L154)
命令即可:
- 递归查找所有在 `src/`目录中 文件名以 `.test.js` 结尾的文件
- mocha 执行这些找到的文件

```bash
yarn test
```

### 惯例

- 待测试文件 必须 以 `test.js` 结尾 否则 `yarn test` 将无法检测到它们
- 测试文件名应该在 相关 Component 之后
(例如. 创建 `Login.test.js` 基于 `Login.js` 组件)

### 简单基础案例


为了帮助您顺利完成RSK，您可以使用以下
[基本测试](https://github.com/kriasoft/react-starter-kit/blob/master/src/components/Layout/Layout.test.js)
用例作为起点

```js
import React from 'react';
import { expect } from 'chai';
import { shallow } from 'enzyme';
import App from '../App';
import Layout from './Layout';

describe('Layout', () => {

  it('renders children correctly', () => {
    const wrapper = shallow(
      <App context={{ insertCss: () => {} }}>
        <Layout>
          <div className="child" />
        </Layout>
      </App>
    );

    expect(wrapper.contains(<div className="child" />)).to.be.true;
  });

});
```

### React-intl 案例ß

React-intl 用户必须 渲染/包裹 Component 与 IntlProvider 中,
如下所示:

下面的示例是RSK `Header` 组件的插入测试：

```js
import React from 'react';
import Header from './Header';
import IntlProvider from 'react-intl';
import Navigation from '../../components/Navigation';

describe('A test suite for <Header />', () => {

  it('should contain a <Navigation/> component', () => {
    it('rendering', () => {
      const wrapper = renderIntoDocument(<IntlProvider locale="en"><Header /></IntlProvider>);
      expect(wrapper.find(Navigation)).to.have.length(1);
    });
  });

});
```

请注意, 如若未使用 IntlProvider 会产生以下错误:

> Invariant Violation: [React Intl] Could not find required `intl` object. <IntlProvider>
> needs to exist in the component ancestry.

### Linting

为了检查您的JavaScript和CSS代码是否遵循建议的样式指南，请运行:

```bash
yarn run lint
```
