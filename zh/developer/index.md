# 自定义组件

## 数据格式

- 自定义组件的数据格式，有自定义的属性，也可以往里面添加，页面插入后会 `page-added` 广播消息，如果插入的是默认的页面，那么不带数据，如果是自定义页面模板，广播消息的时候会把整个数据（包含自定义数据）一起广播出去。

- 自定义组件请的描述文件请保存为 `desc.json` 并放入对应组件的文件夹。

```js
/**
 * 组件的类型
 */
export enum ComponentsType {
    component = 'component', // 组件
    page = 'page', // 页面模板
    game = 'game', // 游戏模板
    interactive = 'interactive', // 互动题型
    // subject = 'subject', // 题型
}

export enum TabType {
    base = 'base', // 通用、基础组件、题型
    '3d' = '3d', // 3D 题型、组件
    interactive = 'interactive', // 互动视频分类
}

/**
 * 组件定义的格式
 */
export interface ICustomComponent {
    prefab: string; // prefab的db的路径或者相对路径
    icon: string; // 显示的图片路径
    type: ComponentsType; // 类型
    typeOrder?: number; // type的排序，数字越大放越后面
    tab: TabType | string; // 页签（分组）
    tabOrder?: number; // tab 的排序，数字越大，放越后面
    group?: string; // 子分组
    groupOrder?: number; // group的排序，数字越大，放越后面
    tag?: string[]; // 这个组件的标签
    description?: string; // 对这个组件的描述
    author?: string; // 作者
    sort: number; // 自身排序,数字越大，放越后面
    [key: string]: any; // 其他自定义格式
}
```

## 扫描规则

自定义组件默认会扫描 `db://assets` 、`db://edu-editor-component` 、`db://edu-component-external` 、`自定义 db` 里面的所有 `desc.json` 文件并根据类型（type）进行分类，在组件的 UI 和页面模板的 UI 进行分类显示。
