# 配置信息

## 配置类型

- `global` 全局配置信息，存放在 `home/.EduEditor/settings/edu-settings.json` 文件中。
- `editor` 编辑器配置信息，存放在编辑器目录的 `/builtin/edu-editor/.EduEditor/settings/edu-settings.json` 文件中。
- `project` 项目配置信息，存放在项目目录的 `/settings/edu-settings.json` 文件中。

## 目前支持的配置

- [服务端配置](server/index.md)
- [分辨率配置](resolution/index.md)
- [属性分组配置](properties-groups/index.md)

### 其他配置

```js
//项目路径，可以是绝对路径，也可以是相对于 Resources 的路径
"project-path": "../../abc_project",

// 素材库 panel 相关设置
'assets-store': {
    // 默认打开的面板
    default: 'edu-editor.assets-store',
    // 如果配置了这个，就不打开默认面板
    panel: 'edu-editor.assets-store',
},

// 配置一些内置的资源
internal: {
    'use-edu-external-component': true, // 是否加载编辑器内置的一些互动组件
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

// 一些接口的继承配置
implements: {
    // 课程广场 - IProject 的实现
    project: '',
    // project: 'packages://edu-editor/dist/edu/project/ProjectServer',
    // 课程广场 - IPage 的实现
    page: '',
    // page: 'packages://edu-editor/dist/edu/page/PageServer'
},

//todo:临时配置，因为 abcMouse 有个需求是希望能体验在线版本和离线版本，所以加个开关，只能为空或者 abcMouse 其余参数无效
channel:"abcMouse"

```
