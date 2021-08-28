# 构建发布

## 指定 bundle 打包

- 请使用 `edu.Bundle.build(bundles:IBundle[], options?)` 进行指定 bundle 的打包。
- `db://assets` 内部的文件夹无需手动勾选设置为 bundle ，编辑器会自动处理为 bundle 包进行打包。
- options 为构建的 options 参数，编辑器会默认带一份，如果不满足需求，可以选择覆盖部分参数，传入 options 中，仅需要填写需要覆盖的参数，无需全部填写，但是 platform 字段不可覆盖，否则会无法构建 bundle 。
- 可以指定打多个 bundle 包，bundle 会在 outputDir 的目录下生成指定以 bundle 的 name 的名字命名的文件夹存放 bundle 文件。

```js
/**
 * 压缩类型
 */
export enum BundleCompressType {
    none = 'none',
    default = 'default',
    mergeAllJson = 'merge_all_json',
}
/**
 * bundle 的配置
 */
export interface IBundle {
    root: string;//db 的路径
    name: string;//bundle 的名字
    outputDir: string;//输出目录
    priority: number;//优先级
    compressType: BundleCompressType//压缩类型
}
//打包示例
await edu.Bundle.build([
{
    "root": "db://assets/images",
    "name": "images",
    "outputDir": "/Users/wzm/Documents/edu/samples/test-build-bundle/",
    "priority": 1,
    "compressionType": "default"
}
])
```
