# 属性分组配置

## 是否显示分组

只需要在编辑配置 （edu-settings.json）配置如下，即可默认开始属性折叠功能，此时，属性面板就会按照脚本，把每条属性的折叠按钮显示出来。

```js
components: {
    collapsible: false,
},
```

## 自定义分组名字

### eduClass修饰器

| 属性 | 说明 | 默认值|
| :--- | :--- | :---: |
| name | 定义组件在教育编辑器属性面板的分组名 | 类名 |
| collapsible|组件是否在教育编辑器属性面板显示分组标题名|true|

### 示例：

```js
import {eduClass} from "education"

@eduClasss({name:'图片', collapsible: true})
export default class EduImage extends EduElementAbstract{
}
```
