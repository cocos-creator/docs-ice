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

服务端的静态资源存储目录 assets
服务端的临时存储目录 temp
