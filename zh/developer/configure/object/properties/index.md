# 通用配置

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

#### 示例

```js
import {eduClass} from "education"

@eduClasss({name:'图片', collapsible: true})
export default class EduImage extends EduElementAbstract{
}
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

## 多端控制配置

该配置适用于 1 对 1、小班课、大班课、双师课等区分老师端和学生端的课程，可以通过配置让老师轻松控制部分内容对学生不显示。

- **配置类型**：`editor` 编辑器配置信息。

- **JSON 配置示例**：

    ```
    'client-mode': {
        // 是否启用教师端可见功能
        enable: false,
    }
    ```

- 此时在 ICE 里面选中任意节点，就能看到仅教师端可见的开关，老师只要打开这个开关，就可以实现教师端可见的功能。

- 客户端需要在启动的时候告诉运行时，当前运行的是否是教师端，如下字段为 `true` 就是运行在教师端，此时，学生端会自动隐藏上面选中的节点

    ```
    edu.eduGame.clientMode.setCustomData({isTeacherClient:true});
    ```
