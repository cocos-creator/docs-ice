# 强制刷新页面缩略图

允许在不保存页面的情况下，刷新页面的缩略图。默认刷新当前打开页面。

可通过在任一窗口的 DevTools 中输入以下代码来查看效果：

```ts
// creator 2.x only
Editor.Ipc.sendToAll('edu-editor:reload-page-thumbnails');
```
