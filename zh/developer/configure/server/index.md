# 客户端接入服务端配置

配置地址参见 [配置信息](developer/configure/index.md) 中的 editor

```js
"assets-server": {
    "host": "192.168.19.255",
    "port": "8080",
    "protocol": "http"
}
```

# 服务端的环境要求

- `nodejs` ，版本 10 +
- 包管理工具 `npm` 或者 `yarn`
- `typescript` 4.2.0 + 建议全局安装

```js
    //npm
    npm i -g typescript@4.4.2

    //yarn
    yarn global add typescript@4.4.2
```

# 服务端的依赖安装

执行 npm install 或者 yarn install 进行依赖模块安装

# 服务端的脚本编译

在服务端根目录执行 tsc 命令

# 服务端配置

把服务端代码下载布署到服务器，更改服务端配置, 服务端的配置文件的路径 source/config.ts

```js
class Config implements IConfig {
    port = 8080;  //配置服务端的访问端口， 默认端口8080
    ...
}
```

服务端的静态资源存储目录 assets <br/>
服务端的临时存储目录 temp

# 服务端的启动配置和启动

服务端的启动配置文件 package.json

```js
"scripts": {
    "start": "node index",
    ...
}
```

在根目录下输入启动服务端命令如下:

```js
npm start    // 使用 npm 启动
yarn start   // 使用 yarn 启动
```

提示：由于直接使用 npm start 或者 yarn start 在退出服务端的链接后，会被关闭，所以我们推荐使用 pm2 等进程管理工具，对服务器进行管理和 log 的写入。
