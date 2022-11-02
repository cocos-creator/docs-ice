# 工具栏配置

允许用户自定义顶部 **工具栏** 中按钮的组成、位置。

## 类型声明

### ICE 内置项

```ts
// 内置行为类型的配置
export interface ToolbarItemRole {
    // 标识内置的行为
    role: ToolbarRole;

    // 用于覆盖内置的默认文本
    label?: string;

    // 用于覆盖内置的默认图标
    // 支持类型：
    // 1. 内置 iconfont 图标
    // 2. 图片 URL (png、jpg、jpeg、gif、webp、svg)
    // 3. base64 字符串
    icon?: string;
}
```

### 用户自定义项

```ts
// 用户自定义按钮的配置
export interface ToolbarItemCustomized {
    // 用于标识按钮的 ID。影响绑定的事件名称、元素的标签
    id: string;

    // 展示的文本
    label: string;

    // 展示的图标
    icon: string;
}
```

### 每一项配置

```ts
// 每一个 toolbar 选项的配置
export type ToolbarItem = ToolbarItemCustomized | ToolbarItemBase;
```

### JSON 根配置

```ts
// json 中配置的类型
export interface ToolbarConfigs {
    left: ToolbarItem[];
    center: ToolbarItem[];
    right: ToolbarItem[];
}
```

### 内置行为

```ts
// 内置的行为类型
export enum ToolbarRole {
    /**
     * 分隔符
     */
    divider = 'divider',

    /**
     * 打开课件广场
     */
    openCoursePlaza = 'openCoursePlaza',

    /**
     * “显示”下拉菜单
     */
    showDisplayMenu = 'showDisplayMenu',

    /**
     * 保存
     */
    save = 'save',

    /**
     * 撤销
     */
    undo = 'undo',

    /**
     * 重做
     */
    redo = 'redo',

    /**
     * 插入文本
     */
    insertText = 'insertText',

    /**
     * 插入形状
     */
    insertShape = 'insertShape',

    /**
     * 插入图片
     */
    insertImage = 'insertImage',

    /**
     * 插入音频
     */
    insertAudio = 'insertAudio',

    /**
     * 插入动画
     */
    insertAnimation = 'insertAnimation',

    /**
     * 插入视频
     */
    insertVideo = 'insertVideo',

    /**
     * 打开素材库
     */
    openAssetStore = 'openAssetStore',

    /**
     * 插入游戏
     */
    insertGame = 'insertGame',

    /**
     * 插入组件
     */
    insertComponent = 'insertComponent',

    /**
     * 插入题型
     */
    insertInteraction = 'insertInteraction',

    /**
     * 预览
     */
    preview = 'preview',

    /**
     * 上传
     */
    upload = 'upload',

    /**
     * 发布
     */
    publish = 'publish',
}
```

## 自定义按钮的行为

```json
{
    "id": "custom1",
    "label": "课件",
    "icon": "courses"
}
```

对于以上这样一个按钮配置，ICE 提供以下接口供外部对接使用。

- 用户配置的 `id`，将以 `data attribute` 的形式附加到目标按钮元素上

    ```html
    <!-- 元素示例 -->
    <button data-ice-toolbar-id="custom1">...</button>
    ```

    ```js
    // 通过 `querySelector` 获取定制的按钮元素
    const buttons = document.querySelectorAll('[data-ice-toolbar-id=custom1]');
    ```

- 用户配置出现的按钮，在元素对应的 `click`、`mouseenter`、`mouseleave` 事件触发时，会发出指定的 IPC 事件广播。

    事件名称与用户定义的 `id` 相关，对应逻辑为 `toolbar-${id}-${eventName}`

    ```text
    <!-- id 为 custom1 时的事件名称示例 -->

    click: 'toolbar-custom1-click',

    mouseenter: 'toolbar-custom1-mouseenter',

    mouseleave: 'toolbar-custom1-mouseleave',
    ```

## 超出折叠

当前未支持

## 配置示例

```json
{
    "toolbar": {
        "left": [
            {
                "role": "openCoursePlaza"
            },

            {
                "role": "showDisplayMenu"
            },

            {
                "role": "divider"
            },

            {
                "role": "save"
            }
        ],

        "center": [
            {
                "id": "custom1",
                "label": "课件",
                "icon": "courses"
            },

            {
                "role": "save",
                "label": "快速保存",
                "icon": "https://www.cocos.com/wp-content/themes/cocos/image/logo.png"
            }
        ],

        "right": []
    }
}
```
