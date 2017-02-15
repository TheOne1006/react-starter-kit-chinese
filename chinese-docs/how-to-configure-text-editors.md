## 如何为React.js配置文本编辑器和IDE [![img](https://img.shields.io/badge/discussion-join-green.svg?style=flat-square)](https://github.com/kriasoft/react-starter-kit/issues/117)

> 关于如何配置您喜欢的文本编辑器或IDE使用 React.js/ES6+/JSX 的提示和技巧

### WebStorm

创建一个新的项目基于 **React Starter Kit template**

![react-project-template-in-webstorm](https://dl.dropboxusercontent.com/u/16006521/react-starter-kit/webstorm-new-project.png)

确保在项目中启用了 **JSX** 支持.
如果你创建一个基于React.js模板的新项目, 这是默认设置.

![jsx-support-in-webstorm](https://dl.dropboxusercontent.com/u/16006521/react-starter-kit/webstorm-jsx.png)

配置 **auto-complete** JavaScript 库

![javascript-libraries-in-webstorm](https://dl.dropboxusercontent.com/u/16006521/react-starter-kit/webstorm-libraries.png)

启用 **ESLint** 支持

![eslint-support-in-webstorm](https://dl.dropboxusercontent.com/u/16006521/react-starter-kit/webstorm-eslint.png)

启用 **CSSComb** 按照说明进行操作 [here](https://github.com/csscomb/jetbrains-csscomb).

**如果您在自动重新载入时遇到问题** 可以尝试禁用 "safe write" 通过 in `File > Settings > System Settings > Use "safe write" (首先将更改保存到临时文件)`

### Atom

安装atom包

* [linter](https://atom.io/packages/linter)
* [linter-eslint](https://atom.io/packages/linter-eslint)
* [linter-stylelint](https://atom.io/packages/linter-stylelint)
* [react](https://atom.io/packages/react)

```shell
apm install linter linter-eslint react linter-stylelint
```

安装本地npm软件包

* [eslint](https://www.npmjs.com/package/eslint)
* [babel-eslint](https://www.npmjs.com/package/babel-eslint)
* [eslint-plugin-react](https://www.npmjs.com/package/eslint-plugin-react)
* [stylelint](https://www.npmjs.com/package/stylelint)

```shell
yarn add --dev eslint babel-eslint eslint-plugin-react stylelint
```

*你可能需要重启 atom 已确保修改生效*

### SublimeText

安装 SublimeText 安装包
Easiest with [Package Control](https://packagecontrol.io/) and then "Package Control: Install Package" (Ctrl+Shift+P)  

* [Babel](https://packagecontrol.io/packages/Babel)
* [Sublime-linter](http://www.sublimelinter.com/en/latest/)
* [SublimeLinter-contrib-eslint](https://packagecontrol.io/packages/SublimeLinter-contrib-eslint)
* [SublimeLinter-contrib-stylelint](https://packagecontrol.io/packages/SublimeLinter-contrib-stylelint)

You can also use [SublimeLinter-contrib-eslint_d](https://packagecontrol.io/packages/SublimeLinter-contrib-eslint_d) for faster linting.

Set Babel as default syntax for a particular extension:

* Open a file with that extension,
* Select `View` from the menu,
* Then `Syntax` `->` `Open all with current extension as...` `->` `Babel` `->` `JavaScript (Babel)`.
* Repeat this for each extension (e.g.: .js and .jsx).

Install local npm packages

```
yarn add --dev eslint@latest
yarn add --dev babel-eslint@latest
yarn add --dev eslint-plugin-react
yarn add --dev stylelint
```
