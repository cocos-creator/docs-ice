# 多端控制配置

- **配置类型**：`editor` 编辑器配置信息。

- **JSON 配置示例**：

    ```
    'client-mode': {
    // 是否启用教师端可见功能
    enable: false,
    }
    ```

- 此时在 ICE 里面选中任意节点，就能看到仅教师端可见的开关，老师只要打开这个开关，就可以实现教师端可见的功能。

- 客户端需要在启动的时候告诉运行时，当前运行的是否是教师端，如下字段为 `true` 就是运行在教师端，此时，学生端会自动隐藏上面选中的节点

    ```
    edu.eduGame.clientMode.setCustomData({isTeacherClient:true});
    ```
