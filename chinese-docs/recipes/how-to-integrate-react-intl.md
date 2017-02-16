## 如何实现 [React Intl](https://github.com/yahoo/react-intl#react-intl)

 1. 通过 GIT 合并 `feature/react-intl` 分支.
    因为 react-intl 实现是基于 `feature/redux`, 你也会获得所有功能.

 2. 调整常量 `INTL_REQUIRE_DESCRIPTIONS` 于 `tools/webpack.config.js` 大约在 17 行:
    ```js
    const INTL_REQUIRE_DESCRIPTIONS = true;
    ```
    When this boolean is set to true, the build will only succeed if a `description` is set for every message descriptor.

 3. 调整 `locales` 设置于 `src/config.js`:
    ```js
    // default locale is the first one
    export const locales = ['en-GB', 'cs-CZ'];
    ```
    注意你需要遵循
    [BCP 47](https://tools.ietf.org/html/bcp47)
    ([RFC 5646](https://tools.ietf.org/html/rfc5646)).

 4. 在 `src/client.js` 添加本地化支持:
    ```js
    import en from 'react-intl/locale-data/en';
    import cs from 'react-intl/locale-data/cs';
    ...

    [en, cs].forEach(addLocaleData);
    ```

 5. 运行 `yarn run extractMessages` 或者 `yarn start` 删除邮件.
    在中创建消息文件 `src/messages` 目录中.

 6. 编辑 `src/messages/*.json` 文件, 只更改 `message` 属性.

 7. 执行 `yarn run build`,
    您的翻译应该被复制到 `build/messages/` 目录.


### 如何编写可本地化的组件

只需要引入适当的组件 [component](https://github.com/yahoo/react-intl/wiki#the-react-intl-module) 来自于 `react-intl`

- 可本地化文本使用
[`<FormattedMessage>`](https://github.com/yahoo/react-intl/wiki/Components#formattedmessage).
- 也可以使用 [`defineMessages()`](https://github.com/yahoo/react-intl/wiki/API#definemessages) 辅助方法.

- 日期和时间:
[`<FormattedDate>`](https://github.com/yahoo/react-intl/wiki/Components#formatteddate)
[`<FormattedTime>`](https://github.com/yahoo/react-intl/wiki/Components#formattedtime)
[`<FormattedRelative>`](https://github.com/yahoo/react-intl/wiki/Components#formattedrelative)

- 数字和货币:
[`<FormattedNumber>`](https://github.com/yahoo/react-intl/wiki/Components#formattednumber)
[`<FormattedPlural>`](https://github.com/yahoo/react-intl/wiki/Components#formattedplural)

- 如果有可能, 不要使用 `<FormattedHTMLMessage>`, 请参阅如何使用 *富文本 格式* 关于
[`<FormattedMessage>`](https://github.com/yahoo/react-intl/wiki/Components#formattedmessage)

- 党你需要引入一个格式化的 API, 使用 [`injectIntl`](https://github.com/yahoo/react-intl/wiki/API#injectintl) 高阶 Component.

#### 案例

```jsx
import { defineMessages, FormattedMessage, injectIntl, intlShape } from 'react-intl';

const messages = defineMessages({
  text: {
    id: 'example.text',
    defaultMessage: 'Example text',
    description: 'Hi Pavel',
  },
  textTemplate: {
    id: 'example.text.template',
    defaultMessage: 'Example text template',
    description: 'Hi {name}',
  },
});

function Example(props) {
  const text = props.intl.formatMessage(messages.textTemplate, { name: 'Pavel'});
  return (
    <div>
      <FormattedMessage
        id="example.text.inlineDefinition"
        defaultMessage="Hi Pavel"
        description="Example of usage without defineMessages"
      />
      <FormattedMessage {...messages.text} />
      <FormattedMessage
        {...messages.textTemplate}
        values={{
          name: <b>Pavel</b>
        }}
      />
    </div>
  );
}

Example.propTypes = {
  intl: intlShape,
}

export default injectIntl(Example);
```

### 更新 translations

当运行开发服务器时, 每个源文件都会被监视并解析更改的消息.

消息文件即时更新.
如果找到新的定义, 这个定义被添加到每个使用的结尾 `src/messages/xx-XX.json` 文件 提交时,
新的翻译将在文件的尾部.

当未翻译的消息被删除并且其消息字段也为空时,
该消息将从所有翻译文件中删除,
这就是为什么文件数组存在.

编辑翻译文件时, 它应该被复制到 `build/messages/` 目录.

### 其他参考文献

 * [`关于MDN的Intl文档`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Intl)
 * [express-request-language](https://github.com/tinganho/express-request-language#readme)
  – 关于初始语言协商如何工作的更多细节.
