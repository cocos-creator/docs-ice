# 自定义组件

## 数据格式

- 自定义组件的数据格式，可以定义自己需要的数据（字段）。
- 页面插入后会 `page-added` 广播消息，如果插入的是默认的页面，那么不带数据，如果是自定义页面模板，广播消息的时候会把整个数据（包含自定义数据）一起广播出去。
- 自定义组件的描述文件请保存为 `desc.json` 并放入对应组件的文件夹。

```
/**
 * 组件的类型
 */
export enum ComponentsType {
    middleware = 'middleware', // 中间件（即插件），不在列表中展示
    component = 'component', // 组件
    page = 'page', // 页面模板
    game = 'game', // 游戏模板
    interactive = 'interactive', // 互动题型
    // subject = 'subject', // 题型
    animation = 'animation', // 动画类型
    subjectTools = 'subjectTools', // 学科工具
}

export enum TabType {
    base = 'base', // 通用、基础组件、题型
    '3d' = '3d', // 3D 题型、组件
    interactive = 'interactive', // 互动视频分类
    solidGeometry = 'solidGeometry', // 学科工具-立体几何
}

/**
 * 组件定义的格式
 */
export interface ICustomComponent {
    componentID: string; // 组件的 ID 标识，以 Prefab 对应的 UUID 为 ID
    name: string; // 组件展示的名称
    version: string; // 组件的版本（格式: '0.0.0'）
    prefab: string; // Prefab 的 DB 的路径或者相对路径
    icon: string; // 显示的图片路径
    type: ComponentsType; // 类型
    tab: TabType | string; // 页签（分组）
    sort: number; // 自身排序,数字越大，放越后面
    dependModules: {[key: string]: string} // 组件的依赖模块
    typeOrder?: number; // type 的排序，数字越大放越后面
    tabOrder?: number; // tab 的排序，数字越大，放越后面
    group?: string; // 子分组
    groupOrder?: number; // group的排序，数字越大，放越后面
    tag?: string[]; // 这个组件的标签
    description?: string; // 对这个组件的描述
    author?: string; // 作者
    [key: string]: any; // 其他自定义格式
}
```

其中 `dependModules: {[key: string]: string}` 的 `dependModules` 主要描述的是该组件对外依赖的组件，引用的组件须是 ICE 通用组件格式，即具备 `desc.json` 描述文件，`key` 对应的是组件的 `desc.json` 的 `ComponentID`，`value` 对应的是 `(name)^(version)`。案例如下：

```
{
    "f6d95e66-f48c-4f2e-aafa-269e1dc65443": "alert^0.0.1",
    "6a15c155-1426-4390-91c3-d1af0e05c6b1": "填空题-乌鸦喝水^0.0.1"
}
```

## 组件展示规则

默认 ICE 中读取的是所配置的服务端上的组件，如果需要更改读取本地配置需要添加配置

```
'depend-local-resource': true, // 默认是 `false`
```

## 自定义组件服务端布署

在服务端 `assets/components` 路径新建一个文件夹，把自定义组件包打成的 `.zip` 压缩文件，对应的 `desc.json` 文件、组件封面文件复制到该文件夹下，然后重启服务器，即可完成自定义组件的服务端部署。示例结构如下：

```
components
 └── 3DModel
     ├── 3DModel.zip
     ├── desc.json
     └── effectPic.jpg // 与 desc.json 中的 icon 名一致

```

后续版本将支持直接通过调用接口或 API 快速批量上传自定义组件到服务器，并且在组件的 UI 和页面模板的 UI 进行数据请求和展示。

用户完成本地的组件编写后，需要上传到服务器，调用 ICE 的上传面板，上传组件到服务器， 并且在组件的 UI 和页面模板的 UI 进行数据请求和展示

## 自定义组件的本地扫描规则

自定义组件默认会扫描 `db://assets`、`db://edu-editor-component`、`db://edu-component-external`、`自定义 db`，里面所有的 `desc.json` 文件会根据类型（type）进行分类，并在组件的 UI 和页面模板的 UI 进行分类显示。

## 研发自定义组件

研发自定义组件时，可以自定义组件在属性面板上显示的内容，也可以自定义要展示给老师的节点。

- [自定义属性](develop-properties/index.md)

- [节点设置](node-setting/index.md)
