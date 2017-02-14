## React 风格规范



> 本风格规范来自于[Airbnb React/JSX Guide](https://github.com/airbnb/javascript/tree/master/react).

> 随意修改它，以适应您的项目的需要.

### 目录

* [每个UI组件使用单独的文件夹](#separate-folder-per-ui-component)
* [纯函数组件优先](#prefer-using-functional-components)
* [使用CSS模块](#use-css-modules)
* [使用高阶组件](#use-higher-order-components)

### 每个UI组件使用单独的文件夹

* 将每个主要UI组件以及相关资源放在单独的文件夹中
  这将使它更容易找到所需要的 UI 元素(CSS, images, unit 测试, 本地化文件 等.).
  重构过程中删除相关组件也变得容易
* 避免 CSS, image 以及其他资源文件在多个 Component 中共享
  这将使你的代码更具有可维护性,更容易重构
* Add `package.json` file into each component's folder.<br>
  This will allow to easily reference such components from other places in
  your code.<br>
* 将 `package.json` 文件添加到每个 组件的目录中
  使用 `import Nav from '../Navigation'` 替代 `import Nav from '../Navigation/Navigation.js'`

```
/components/Navigation/icon.svg
/components/Navigation/Navigation.css
/components/Navigation/Navigation.js
/components/Navigation/Navigation.test.js
/components/Navigation/Navigation.ru-RU.css
/components/Navigation/package.json
```

```
// components/Navigation/package.json
{
  "name:": "Navigation",
  "main": "./Navigation.js"
}
```

更多详情请自行 谷歌 [component-based UI development](https://google.com/search?q=component-based+ui+development).

### 纯函数组件优先

* 尽可能使用无状态的纯函数组件
  不需要使用状态的组件,最好使用 纯函数组件.

```jsx
// Bad
class Navigation extends Component {
  static propTypes = { items: PropTypes.array.isRequired };
  render() {
    return <nav><ul>{this.props.items.map(x => <li>{x.text}</li>)}</ul></nav>;
  }
}

// Better
function Navigation({ items }) {
  return (
    <nav><ul>{items.map(x => <li>{x.text}</li>)}</ul></nav>;
  );
}
Navigation.propTypes = { items: PropTypes.array.isRequired };
```

### Use CSS Modules

* Use CSS Modules<br>
  This will allow using short CSS class names and at the same time avoid conflicts.
* Keep CSS simple and declarative. Avoid loops, mixins etc.
* Feel free to use variables in CSS via [precss](https://github.com/jonathantneal/precss) plugin for [PostCSS](https://github.com/postcss/postcss)
* Prefer CSS class selectors instead of element and `id` selectors (see [BEM](https://bem.info/))
* Avoid nested CSS selectors (see [BEM](https://bem.info/))
* When in doubt, use `.root { }` class name for the root elements of your components

```scss
// Navigation.scss
@import '../variables.scss';

.root {
  width: 300px;
}

.items {
  margin: 0;
  padding: 0;
  list-style-type: none;
  text-align: center;
}

.item {
  display: inline-block;
  vertical-align: top;
}

.link {
  display: block;
  padding: 0 25px;
  outline: 0;
  border: 0;
  color: $default-color;
  text-decoration: none;
  line-height: 25px;
  transition: background-color .3s ease;

  &,
  .items:hover & {
    background: $default-bg-color;
  }

  .selected,
  .items:hover &:hover {
    background: $active-bg-color;
  }
}
```

```jsx
// Navigation.js
import React, { PropTypes } from 'react';
import withStyles from 'isomorphic-style-loader/lib/withStyles';
import s from './Navigation.scss';

function Navigation() {
  return (
    <nav className={`${s.root} ${this.props.className}`}>
      <ul className={s.items}>
        <li className={`${s.item} ${s.selected}`}>
          <a className={s.link} href="/products">Products</a>
        </li>
        <li className={s.item}>
          <a className={s.link} href="/services">Services</a>
        </li>
      </ul>
    </nav>
  );
}

Navigation.propTypes = { className: PropTypes.string };

export default withStyles(Navigation, s);
```

### Use higher-order components

* Use higher-order components (HOC) to extend existing React components.<br>
  Here is an example:

```js
// withViewport.js
import React, { Component } from 'react';
import { canUseDOM } from 'fbjs/lib/ExecutionEnvironment';

function withViewport(ComposedComponent) {
  return class WithViewport extends Component {

    state = {
      viewport: canUseDOM ?
        {width: window.innerWidth, height: window.innerHeight} :
        {width: 1366, height: 768} // Default size for server-side rendering
    };

    componentDidMount() {
      window.addEventListener('resize', this.handleResize);
      window.addEventListener('orientationchange', this.handleResize);
    }

    componentWillUnmount() {
      window.removeEventListener('resize', this.handleResize);
      window.removeEventListener('orientationchange', this.handleResize);
    }

    handleResize = () => {
      let viewport = {width: window.innerWidth, height: window.innerHeight};
      if (this.state.viewport.width !== viewport.width ||
        this.state.viewport.height !== viewport.height) {
        this.setState({ viewport });
      }
    };

    render() {
      return <ComposedComponent {...this.props} viewport={this.state.viewport}/>;
    }

  };
};

export default withViewport;
```

```js
// MyComponent.js
import React from 'react';
import withViewport from './withViewport';

class MyComponent {
  render() {
    let { width, height } = this.props.viewport;
    return <div>{`Viewport: ${width}x${height}`}</div>;
  }
}

export default withViewport(MyComponent);
```

**[⬆ back to top](#table-of-contents)**
