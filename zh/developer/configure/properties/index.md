# 属性配置

## 属性分组配置

### 是否显示分组

只需要在配置文件 `edu-settings.json` 文件中进行如下配置，即可开启属性折叠功能。此时，属性面板就会按照脚本，把每条属性的折叠按钮显示出来。

```js
components: {
    collapsible: false,
},
```

### 自定义分组名字

#### eduClass 修饰器

| 属性 | 说明 | 默认值|
| :--- | :--- | :---: |
| name       | 定义组件在教育编辑器属性面板的分组名 | 类名 |
| collapsible|组件是否在教育编辑器属性面板显示分组标题名|true|

#### 示例：

```js
import {eduClass} from "education"

@eduClasss({name:'图片', collapsible: true})
export default class EduImage extends EduElementAbstract{
}
```

## 文字属性配置

### 字号配置

- **配置类型**：`editor` 编辑器配置信息。

- **类型定义**：

    ```ts
    type FontSizes = number[];
    ```

- **JSON 配置示例**：

    ```json
    "fontSizes": [12, 24, 32, 48]
    ```

## 颜色预设配置

- **配置类型**：`editor` 编辑器配置信息。

- **类型定义**：

    ```ts
    type presetColors = string[];
    ```

- **JSON 配置示例**：

    ```json
    "presetColors": ["#000000", "#ffffff"]
    ```
  
## 视频组件背景色配置

- **配置类型**：`editor` 编辑器配置信息。

- **JSON配置示例**
  ```json
  components:{
    "EduMovie":{
        "background-color":"#ffffff"
    }
  }
  ```
  
## 页面默认切换动效

- **配置类型**: `editor` 编辑器配置信息。

- **JSON 配置示例**
  ```json
  pageTransition: "PagePushIn"
  ```
  如果配置的数据是无效的，运行时可能无法生效。

## 互动组件结束互动时的默认动作配置

![](../../../img/interactive-finish-action.png)

示例代码：
```ts
import { Action }, interactiveFinishAction  from 'InteractiveFinishAction'
interactiveFinishAction.register(
    new Action(
        '测试选项', // 选项的名字
        'test-key', // 选项的唯一标识名，不允许重复，否则会提示注册失败
        ()=>{
            // 选项的具体执行逻辑
            console.log('i am test ...')
        }
    )
)

```
如上所示，就能在该下拉选项中增加一个 **测试选项**，当用户配置了这个 **测试选项** 后，互动组件在结束互动时，就会执行对应的选项回调。
