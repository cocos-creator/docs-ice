# 客户端接入服务端配置

配置地址参见 [配置信息](developer/configure/index.md) 中的 editor

```js
"assets-server": {
    "host": "192.168.19.255",
    "port": "8080",
    "protocol": "http"
}
```

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

提示：服务端布署可以借助 pm2 进程管理工具对 node 的进程进行管理
