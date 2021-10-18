# 服务端配置

## 客户端接入服务端配置

ICE 的课程广场、素材库等所在的服务端支持部署在公司自己的服务器，部署好的服务器可以在配置文件中配置相应信息，具体配置地址请参考 [配置信息](../index.md) 中的 `editor` 编辑器配置信息。配置代码示例如下：

```js
"assets-server": {
    "host": "192.168.19.255",
    "port": "8080",
    "protocol": "http"
}
```

## 服务端的环境配置

- `nodejs`, PC 端全局安装 [nodejs-10.0.0](https://nodejs.org/zh-cn/download/) 或以上版本
- 包管理工具 `npm` 或者 `yarn`
- PC 端全局安装 TypeScript 4.2.0 或以上版本

```js
    //npm
    npm i -g typescript@4.2.0

    //yarn
    yarn global add typescript@4.2.0
```

## 服务端的依赖安装

根据使用的包管理工具执行 `npm install` 或者 `yarn install` 安装依赖模块

## 服务端的脚本编译

在服务端根目录执行 tsc 命令

## 服务端配置

把服务端代码下载布署到服务器，更改服务端配置，服务端的配置文件路径为 `source/config.ts`。

```js
class Config implements IConfig {
    port = 8080;  // 配置服务端的访问端口，默认端口为 8080
    ...
}
```

服务端的静态资源存储目录为 `assets` <br/>
服务端的临时存储目录为 `temp`

## 服务端的启动配置和启动

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

>**提示**：由于直接使用 `npm start` 或者 `yarn start` 在退出服务端的连接后，会被关闭，所以我们推荐使用 pm2 等进程管理工具，对服务器进行管理和 log 的写入。
