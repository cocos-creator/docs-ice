# 页面配置

## 新建课程空白页配置

配置该项可以控制新建课程后是否默认生成一页空白页。

```js
'edu-blank-page': false, // 课程制作模式
'story-blank-page': false, // 互动视频制作模式
```

| 属性 | 说明 | 默认值|
| :--- | :--- | :---: |
| edu-blank-page|  课程制作模式 | false |
| story-blank-page|  互动视频制作模式  |false |

## 页面切换动效配置

- **配置类型**: `editor` 编辑器配置信息。

- **JSON 配置示例**

  ```json
  "pageTransition": "PagePushIn"
  ```

  >**注意**：如果配置的数据是无效的，运行时可能无法生效。
