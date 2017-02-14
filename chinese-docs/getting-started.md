## 入门

### 要求

  * Mac OS X, Windows, or Linux
  * [Yarn](https://yarnpkg.com/) package + [Node.js](https://nodejs.org/) v6.5 或更新
  * Text editor or IDE pre-configured with React/JSX/Flow/ESlint ([learn more](./how-to-configure-text-editors.md))

### 布局目录

开始之前, 花一点时间了解文件结构:

```
.
├── /build/                     # 编译输出的文件夹
├── /docs/                      # 项目文档文件
├── /node_modules/              # 第三方库和工具
├── /public/                    # 静态文件,将会复制到 /build/public 目录
├── /src/                       # 应用源码
│   ├── /components/            # React Components
│   ├── /core/                  # 核心框架 和 utility 方法
│   ├── /data/                  # GraphQL 服务的 schema 和 data models
│   ├── /routes/                # 页面/screen 组件 以及路由信息
│   ├── /client.js              # 客户端启动脚本
│   ├── /config.js              # 全局应用程序设置
│   └── /server.js              # 服务器端脚本启动
├── /test/                      # 单元测试 以及 点对点测试
├── /tools/                     # 构建自动化脚本和实用程序
│   ├── /lib/                   # 实用程序片段的库
│   ├── /build.js               # 从源文件到输出（build）目录 构建项目
│   ├── /bundle.js              # 通过Webpack将Web资源捆绑到软件包中
│   ├── /clean.js               # 清空输出（build）目录
│   ├── /copy.js                # 将静态文件复制到输出（构建）目录
│   ├── /deploy.js              # 部署 Web 应用
│   ├── /run.js                 # 自动构建任务 的 Helper 方法
│   ├── /runServer.js           # 启动（或重新启动）Node.js服务器
│   ├── /start.js               # 启动开发Web服务器 with "live reload"
│   └── /webpack.config.js      # 客户端和服务器端软件包的配置
└── package.json                # 第三方 库和 utilities
```

**Note**: 当前 React Starter Kit (RSK) 版本不包含 Flux.
它可以轻松地与您选择的任何Flux库集成.
最常用的Flux库是
[Flux](http://facebook.github.io/flux/),
[Redux](http://redux.js.org/) ,
[Relay](http://facebook.github.io/relay/).

### 快速开始

#### 1. 获取最新版本

你可以开始 clone 最新版本的 React Starter Kit (RSK) 到本地:

```shell
$ git clone -o react-starter-kit -b master --single-branch \
      https://github.com/kriasoft/react-starter-kit.git MyApp
$ cd MyApp
```

或者, 你可以基于 RSK  创建一个新的项目 通过
[WebStorm IDE](https://www.jetbrains.com/webstorm/help/create-new-project-react-starter-kit.html),
或者使用
[Yeoman generator](https://www.npmjs.com/package/generator-react-fullstack).

#### 2. Run `yarn install`

这条命令将会安装所有 [package.json](../package.json) 中的项目依赖和开发工具列表.

#### 3. Run `yarn start`

这条命令将 通过 (`/src`) 里的所有源文件构建 应用 到 (`/build`)  目录.
一旦初始构建完成,
它将启动 Node.js 服务(`node build/server.js`) 以及
[Browsersync](https://browsersync.io/)
通过 [HMR](https://webpack.github.io/docs/hot-module-replacement).

> [http://localhost:3000/](http://localhost:3000/) — Node.js 服务 (`build/server.js`)<br>
> [http://localhost:3000/graphql](http://localhost:3000/graphql) — GraphQL 服务 and IDE<br>
> [http://localhost:3001/](http://localhost:3001/) — BrowserSync proxy with HMR, React Hot Transform<br>
> [http://localhost:3002/](http://localhost:3002/) — BrowserSync control panel (UI)

现在可以通过浏览器打开一个 web 应用, 每当修改 `/ src` 目录中的任何源文件时，
构建模块([Webpack](http://webpack.github.io/)) 将重新编译应用 然后很快的刷新所有浏览器

![browsersync](https://dl.dropboxusercontent.com/u/16006521/react-starter-kit/brwosersync.jpg)

注意! `yarn start` 命令在应用中是 `development` 模式,
在这种情况下不会优化和最小化编译的输出文件.
你可以使用 `--release` 命令参数 让 你的应用 使用 `production` 模式:

```shell
$ yarn start -- --release
```

*NOTE: double dashes are required*


### 如何构建，测试，部署

如果你只需要构建应用程序（没有运行dev服务器），只需运行：

```shell
$ yarn run build
```

或者, 构建一个 production 应用:

```shell
$ yarn run build -- --release
```

或者创建一个 docker 应用:

```shell
$ yarn run build -- --release --docker
```

*NOTE: double dashes are required*

在运行这些命令后, `/build` 目录将包含应用程序的编译版本.
例如, 你可以通过 `node build/server.js` 启动 Node.js 服务.

检查源代码的语法错误和潜在的问题运行:

```shell
$ yarn run lint
```

启动单元测试:

```shell
$ yarn run test          # Run unit tests with Mocha
$ yarn run test:watch    # Launch unit test runner and start watching for changes
```

默认情况下, [Mocha](https://mochajs.org/) 测试程序寻找匹配 `src/**/*.test.js` 正则的文件.

部署应用,执行以下命令:

```shell
$ yarn run deploy
```

The deployment script `tools/deploy.js` is configured to push the contents of
the `/build` folder to a remote server via Git. You can easily deploy your app
to [Azure Web Apps](https://azure.microsoft.com/en-us/services/app-service/web/),
or [Heroku](https://www.heroku.com/) this way. Both will execute `yarn install --production`
upon receiving new files from you. Note, you should only deploy the contents
of the `/build` folder to a remote server.

### How to Update

If you need to keep your project up to date with the recent changes made to RSK,
you can always fetch and merge them from [this repo](https://github.com/kriasoft/react-starter-kit)
back into your own project by running:

```shell
$ git checkout master
$ git fetch react-starter-kit
$ git merge react-starter-kit/master
$ yarn install
```
