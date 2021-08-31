# 扩展右键菜单

目前仅支持在 **主进程（Main Process）** 的代码中进行右键菜单的扩展。

## 调用入口

目前右键菜单相关方法的调用入口为 `edu.Menu.contextMenu`。

使用前需了解 Electron 内置 `Menu` 对象的使用方法及相关 API。以下为相关文档链接：

- <https://www.electronjs.org/docs/api/menu>

- <https://www.electronjs.org/docs/api/menu-item>

```ts
// ContextMenu 相关的类型声明
interface ContextMenuFunction {
  // 弹出右键菜单。
  // MenuItemConstructorOptions 的类型及使用方式请参考上述的 Electron MenuItem 文档
  popup: (
    template: MenuItemConstructorOptions[],
    options?: ContextMenuOptions
  ) => Promise<void>;

  // 用于扩展 ContextMenu 的方法。
  // 对于一个指定类型（type）的菜单，注册一个处理菜单选项的方法（MenuMiddlewareFn）
  // 目前仅支持在主进程代码中调用
  extend: (type: string, middlewareFn: MenuMiddlewareFn) => void;

  // 表示 menu 类型的字符串常量
  types: typeof menuTypes;

  // ...
}
```

```ts
// ContextMenuOptions
export interface ContextMenuOptions {
    extend?: string;
}
```

```ts
// MenuMiddlewareFn 的类型声明
// 接收一个输入的 MenuItemConstructorOptions[]，返回一个 MenuItemConstructorOptions[]
// 支持异步返回数据
export interface MenuMiddlewareFn {
  (options: MenuItemConstructorOptions[]):
    | MenuItemConstructorOptions[]
    | Promise<MenuItemConstructorOptions[]>;
}
```

```ts
// 菜单扩展点（即支持扩展的位置）。
// 具体数据可通过访问 edu.Menu.contextMenu.types 对象获取。
const menuTypes = {
    // ...
    page: {
        card: '...',
    },
};
```

## 调用示例

### Main Process

- 注册一个 `MiddlewareFn`，接收一个 template（`MenuItemConstructorOptions[]`），用户根据需求进行定制后，返回一个 `MenuItemConstructorOptions[]`。`MenuItemConstructorOptions` 的使用方式及类型定义见上述 Electron 文档。

- 注册到同一个 `type` 下的 `MiddlewareFn` 会依次被执行。
- 上一次执行完毕后返回的 `template`，会作为下一个 `MiddlewareFn` 的参数被传入。

```ts
// main process
edu.Menu.contextMenu.extend(edu.Menu.contextMenu.types.page.card, (template) => {
  return template.concat([
    {
      label: 'extended',
      click: () => {
        //   do something
      },
    },
  ]);
});
```

### Renderer Process

```ts
// renderer process 调用后，会弹出菜单
edu.Menu.contextMenu.popup(
  [
    {
      label: 'm1',
      click: () => {
        console.log('m1 click');
      },
    },
  ],
  {
    extend: edu.Menu.contextMenu.types.page.card,
  }
);
```

弹出的菜单中会先后包含 'm1', 'extended' 两个菜单项，点击后会执行相应的 `click` 方法。
