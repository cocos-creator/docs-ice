# 配置信息

## 配置类型

- global 全局配置信息，放在 `home/.EduEditor/settings/edu-settings.json` ，用来存放全局信息。
- editor 编辑器配置信息，放在 编辑器 `/builtin/edu-editor/.EduEditor/settings/edu-settings.json` ，用来放置编辑器信息。
- project 项目配置信息，放在 项目 `/settings/edu-settings.json` 用来存放项目设置。

## 目前支持的配置

```js
//项目路径，可以是绝对路径，也可以是相对于 Resources 的路径
"project-path": "../../abc_project",

// 创建的页面的分辨率设置
"design-resolution": {
    "width": 1920,
    "height": 1080
},
// 素材库服务端相关设置
"assets-server": {
    "host": "192.168.19.255",
    "port": "8080",
    "protocol": "http"
},

// 素材库 panel 相关设置
'assets-store': {
    // 默认打开的面板
    default: 'edu-editor.assets-store',
    // 如果配置了这个，就不打开默认面板
    panel: 'edu-editor.assets-store',
},

// 预览相关
    // 预览的虚拟设备
    preview: { 
        'virtual-devices': [
            // {id: 1001, name: 'iPhone7'},
            // {id: 1002, name: '小米9'},
            // {id: 1003, name: 'iPad mini'},
        ],
    },
    // 是否启用默认分辨率进行预览
    'default-resolution-preview': true,

    upload: {
        // 功能开发中
        default: 'edu-editor.upload',
        // 如果配置了这个，就不打开默认面板
        panel: '',
    },
    // {path:'just-test',readonly:false,name?:'dbname'}
    // path packages://协议路径，
    // readonly 是否是只读
    // name 可以为空，填了就是 db 的名字
    db: [],

// 配置一些内置的资源
internal: {
    'use-edu-external-component': true, // 是否加载编辑器内置的一些互动组件
},

// 一些接口的继承配置
implements: {
    // 课程广场- IProject 的实现
    project: '',
    // project: 'packages://edu-editor/dist/edu/project/ProjectServer',
    // 课程广场- IPage 的实现
    page: '',
    // page: 'packages://edu-editor/dist/edu/page/PageServer'
},

//todo:临时配置，因为 abcMouse 有个需求是希望能体验在线版本和离线版本，所以加个开关，只能为空或者 abcMouse 其余参数无效
channel:"abcMouse",

// 配置默认的字体大小设置
fontSizes: [
  10, 12, 14, 16, 20, 24, 28, 32, 36, 48, 64, 72, 96, 128,
],

// 配置默认的字体下拉选项。支持两种格式
fontFamilies: [
  'Arial',
  { label: '楷体', value: 'kaiti' }
],

// 配置默认的颜色选项（调色盘）
presetColors: [
  '#000000',
  '#ffffff',
],

// 默认是否显示分组信息
components: {
    collapsible: false,
},

// 是否启用教师端模式
'client-mode': {
    enable: false,
}
```
