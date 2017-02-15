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
- recursively find all files ending with `.test.js` in your `src/` directory
- mocha execute found files

```bash
yarn test
```

### Conventions

- test filenames MUST end with `test.js` or `yarn test` will not be able to detect them
- test filenames SHOULD be named after the related component (e.g. create `Login.test.js` for
`Login.js` component)

### Basic example

To help you on your way RSK comes with the following
[basic test case](https://github.com/kriasoft/react-starter-kit/blob/master/src/components/Layout/Layout.test.js)
you can use as a starting point:

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

### React-intl exampleß

React-intl users MUST render/wrap components inside an IntlProvider like the example below:

The example below example is a drop-in test for the RSK `Header` component:

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

Please note that  NOT using IntlProvider will produce the following error:

> Invariant Violation: [React Intl] Could not find required `intl` object. <IntlProvider>
> needs to exist in the component ancestry.

### Linting

In order to check if your JavaScript and CSS code follows the suggested style guidelines run:

```bash
yarn run lint
```
