# ClassIn

## 通信方式

iframe 通信

## 限制

1. 目前平台限制每秒钟只能发送 10 次请求；
2. 课件只能通过在课堂中打开 `XXX.edu` 文件预览。

## 接口说明

ClassIn 提供了 window 层接口，通过 `window.classInApp` 实现与客户端的相关交互。本次适配中，使用自定义事件 `onFileMessage` 接管所有自定义消息。

- 加载完毕事件： `loadFinish` (H5 派发给教室)；

- 获取 fileContent： `fileContent` (H5 派发给教室)；

- 消息发送: `sendPalette` (H5 派发给教室)；
    - **参数**

    ```js
    {
        // 消息数据
        status:string = '',
        // 是否需要反馈
        sendBack:boolean = false
    }
    ```

    - **调用示例**

    ```js
    window.classInApp.sendPalette(
        JSON.stringify({
            method: name,
            payload: obj,
        }),
        false,
    );
    ```

  > 注意:`status` 必须是字符串类型且长度不得超过 1M，如单次需要同步的内容比较多，请拆分多个 `palette`，`sendBack` 为 `bool` 类型，如需要反馈，客户端同步成功后将根据 `status` 回传：`status` 长度为 0，则客户端回传将调用 `onClearPalette`，否则调用 `onPalette` 回传 status。

- 消息接收：`onPalette`；
    - **调用示例**

    ```js
    window.classInApp.onPalette = (status) => {
        that.onMessage(status);
    };
    ```

## 示例

下面是 ClassIn 平台适配代码，代码路径为： `.\edu-editor\ui-component\stateSync\ClassIn.ts`。

```js
import { GameConfig } from 'GameConfig';
import { GameDB } from 'GameDB';
import { eduGame } from 'EduGame';
import { IStateSync, StateSyncEvent } from 'IStateSync';
/**
 * 在 iframe 可能发送或者收到的事件
 */
export enum IframeEvents {
    Init = 'Init',
    AttributesUpdate = 'AttributesUpdate',
    SetAttributes = 'SetAttributes',
    DispatchMagixEvent = 'DispatchMagixEvent',
    NextPage = 'NextPage',
    PrevPage = 'PrevPage',
    GetAttributes = 'GetAttributes',
    PageTo = 'onJumpPage',
    // 房间状态变更
    RoomStateChanged = 'room-pubmsg',
    // 设置页数
    SetPage = 'onPagenum',
    // 自定义事件
    onFileMessage = 'onFileMessage',
    // 用户列表变更事件
    onUserChange = 'onUserChange',
    // 获取当前页面
    RequestCurrentPage = 'RequestCurrentPage',
    // 教室状态变化
    onClassStateChange = 'onClassStateChange',
    // 请求当前页数
    GetCurrentPage = 'GetCurrentPage',
    // 得到 ”获取当前页面“ 回调
    ReciveCurrentPage = 'ReciveCurrentPage',
}
/**
 * 白板目前会用到的
 */
export enum ClassInIdentity {
    // 老师
    teacher = 'teacher',
    // 助教
    assistant = 'assistant',
    // 学生
    student = 'student',
    // 旁听生
    auditor = 'auditor',
}

/**
 * 声网同步相关
 * todo:需要判断当前是否是声网的环境，不是就不要和发送相关操作
 */
export class ClassIn extends cc.EventTarget implements IStateSync {
    // 页面加载完成
    _pageLoaded = false;
    // 用户信息
    _currentUserData = {
        type: '',
        users: '',
    };
    // 缓存课件信息
    classData = {};
    // 慢下来吧，少发几个消息
    slowDownTimeOut = null;
    // 当前页面索引
    _currentPageIndex = 0;

    constructor() {
        super();
        !CC_EDITOR && this.registerEvent();
    }
    onStateUpdate() {}
    init(): void {
        this.postMessage(IframeEvents.onFileMessage, { cmd: IframeEvents.Init });
    }
    getCurrentPage(): void {
        this.postMessage(IframeEvents.onFileMessage, {
            cmd: IframeEvents.GetCurrentPage,
            userType: this._currentUserData.type,
            event: IframeEvents.GetCurrentPage,
        });
    }
    responseGetCurrentPage(payload) {
        if (
            this._currentUserData.type === ClassInIdentity.teacher &&
            payload.userType !== ClassInIdentity.teacher
        ) {
            this.postMessage(IframeEvents.onFileMessage, {
                cmd: IframeEvents.ReciveCurrentPage,
                event: IframeEvents.ReciveCurrentPage,
                currentPage: this._currentPageIndex,
            });
        }
    }
    nextPage(): void {
        this.postMessage(IframeEvents.onFileMessage, { cmd: IframeEvents.NextPage });
    }
    prevPage(): void {
        this.postMessage(IframeEvents.onFileMessage, { cmd: IframeEvents.PrevPage });
    }
    pageTo(page: number): void {
        window.classInApp.sendPalette(
            JSON.stringify({
                method: IframeEvents.PageTo,
                payload: { cmd: IframeEvents.PageTo, toPage: page },
                totalPages: '',
                toPage: page,
            }),
            false,
        );
    }
    setTotalPages(page: number): void {
        this.postMessage(IframeEvents.SetPage, {
            cmd: IframeEvents.SetPage,
            totalPages: page,
        });
    }

    onLoadComplete(): void {
        window.classInApp.loadFinish();
    }

    async setAttributes(obj: any) {
        return new Promise((resolve, reject) => {
            this.once(StateSyncEvent.AttributesUpdate, (data) => {
                resolve(data);
            });
            obj.event = StateSyncEvent.AttributesUpdate;
            obj.cmd = IframeEvents.SetAttributes;
            this.postMessage(IframeEvents.onFileMessage, obj);
        });
    }
    async getAttributes() {
        let obj = {
            cmd: IframeEvents.GetAttributes,
            event: StateSyncEvent.GetAttributes,
        };
        return new Promise((resolve, reject) => {
            resolve(this.classData);
            this.postMessage(IframeEvents.onFileMessage, obj);
        });
    }
    compStateUpdate(compId: string) {
        this.postMessage(IframeEvents.onFileMessage, {
            cmd: IframeEvents.DispatchMagixEvent,
            event: StateSyncEvent.CompStateUpdate,
            payload: compId,
        });
    }
    setCurPage(page) {
        this.setAttributes({ curPage: page });
    }
    postMessage(name: string, obj?: any): void {
        if (this.slowDownTimeOut) {
            clearTimeout(this.slowDownTimeOut);
            this.slowDownTimeOut = null;
        }
        this.slowDownTimeOut = setTimeout(() => {
            window.classInApp.sendPalette(
                JSON.stringify({
                    method: name,
                    payload: obj,
                    totalPages: obj.totalPages,
                    toPage: obj.toPage,
                }),
                false,
            );
            this.slowDownTimeOut = null;
        }, 50);
    }
    private registerEvent() {
        // 监听 ClassIn 过来的事件
        let that = this;
        window.classInApp.onPalette = (status) => {
            that.onMessage(status);
        };
        // 注册自定义事件，组件状态更新
        this.postMessage(IframeEvents.onFileMessage, {
            cmd: IframeEvents.DispatchMagixEvent,
            event: StateSyncEvent.CompStateUpdate,
        });
        this.initPersonIdentity();
        this.onLoadComplete();
    }
    initPersonIdentity() {
        this._currentUserData.type = getQueryVariable('identity') + '';
        if (this._currentUserData.type === ClassInIdentity.teacher) {
            // 监听 eduGame 初始化事件
            eduGame && eduGame.addEventListener(eduGame.Event.OnInitialize, this.onInit.bind(this));
        } else {
            this.getCurrentPage();
        }
        // 监听 eduGame 的页面切换事件
        eduGame &&
            eduGame.addEventListener(eduGame.Event.OnPageSwitch, this.onPageChanged.bind(this));

        function getQueryVariable(variable) {
            var query = window.location.search.substring(1);
            var vars = query.split('&');
            for (var i = 0; i < vars.length; i++) {
                var pair = vars[i].split('=');
                if (pair[0] == variable) {
                    return pair[1];
                }
            }
            return false;
        }
    }

    onPageChanged(info) {
        if (!this._pageLoaded) {
            this._pageLoaded = true;
            return;
        }
        let [cur, total] = GameConfig.getPagePositionInfo(info.page.id);
        this._currentPageIndex = cur;
        this.setCurPage(info.page.id);
    }
    onInit() {
        this._initPage();
        this.on(StateSyncEvent.PageChange, this.handlePageChanged, this);
    }
    private _initPage(info?) {
        let pageId = info ? info.curPage : null;
        if (!pageId) {
            let pageInfo = GameConfig.getBeginPage();
            pageId = pageInfo.id;
            this.setCurPage(pageId);
        }
        let [cur, total] = GameConfig.getPagePositionInfo(pageId);
        this.setTotalPages(total);
        eduGame.goPageByID(pageId);
    }
    handlePageChanged(info) {
        this._currentPageIndex = info;
        let page = GameDB.curPageInfo;
        if (page.lesson) {
            let jumpPage = page.lesson.pages[info - 1];
            if (jumpPage && GameDB.curPageInfo.id !== jumpPage.id) {
                eduGame.goPageByID(jumpPage.id);
            }
        }
    }
    onReciveCurrentPage(payload) {
        this.handlePageChanged(payload.currentPage);
    }
    private onMessage(event) {
        let data = event;
        if (typeof data === 'string') {
            data = JSON.parse(data);
        }
        let payload = data.payload;

        if (data.method === IframeEvents.onFileMessage) {
            // 自定义事件
            switch (data.payload && data.payload.event) {
                case IframeEvents.Init:
                    break;
                case IframeEvents.RoomStateChanged:
                    break;
                case IframeEvents.AttributesUpdate:
                    this.classData = Object.assign(this.classData, payload.paload);
                    this.emit(StateSyncEvent.CompStateUpdate, Object.keys(payload.paload)[0]);
                    break;
                case IframeEvents.DispatchMagixEvent:
                    this.classData = Object.assign(this.classData, payload.paload);
                    this.emit(StateSyncEvent.CompStateUpdate, Object.keys(payload.paload)[0]);
                    break;
                case IframeEvents.GetAttributes:
                    this.emit(StateSyncEvent.GetAttributes, payload);
                    break;
                case IframeEvents.ReciveCurrentPage:
                    this.onReciveCurrentPage(payload);
                    break;
                case IframeEvents.GetCurrentPage:
                    this.responseGetCurrentPage(payload);
                    break;
                default:
                    break;
            }
        } else {
            // 内置事件
            switch (data.method) {
                case IframeEvents.PageTo:
                    this.handlePageChanged(data.toPage);
                    break;

                default:
                    break;
            }
        }
    }
}


```

### 函数说明

- `registerEvent`：初始化事件函数；
    - 实现 window.classInApp 的 `onPalette` 回调函数，实现消息监听；
    - 监听 `EduGame` 发出的消息，例如页面切换；
    - 主动调用 `window.classInApp.loadFinish()` 方法，通知教室课件加载完成（这一步至关重要）。
- `postMessage`：消息发送函数；
    - 参数必须为字符串。
- `onMessage`：消息接收函数，因为本次适配使用 `onFileMessage` 接管所有自定义事件，所以将自定义事件与公共事件（例如翻页事件）分开处理。

### 课程预览

与 ClassIn 平台约定，需要用户自己打包课程并部署完毕后，将课程链接配置在 `XXX.edu` 文件中使用，详细步骤如下：

- 打包发布课程，具体操作请参考 [**构建发布**](./../../build/index.md)；
- 部署课程，具体参考：
- 新建 `ClassInDemo.edu` 文件，文件内容如下：

  ```js
    {
        // 课程链接
        "url":"https://www.xxx.com",
        // 分辨率
        "size":"600x400,300x200",
        // 课程名称
        "fileContent":"test",
        // 是否开启消息同步
        "classinSysn":true
    }
  ```

- 将 `ClassInDemo.edu` 文件上传至 ClassIn 客户端的云盘中；
- 课程中从云盘打开 `ClassInDemo.edu` 文件。

这样您的课程就已成功在 ClassIn 平台运行了。
