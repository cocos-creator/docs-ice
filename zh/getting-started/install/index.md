# 下载和安装

## 1. 下载 Cocos ICE

- 如果您的公司尚未使用过 Cocos ICE，您可以 **联系官方人员** 获取最新版本的 Cocos ICE 安装包试用（联系方式见下图）。

    ![联系方式](../img/we_chat.png)

- 如果您的公司已购买 Cocos ICE，您可以 **联系公司相关负责人** 获取 Cocos ICE 安装包。

## 2. 安装与启动

### 操作系统要求

Cocos ICE 所支持的系统环境是：
- macOS 所支持的最低版本是 OS X 10.9。
- Windows 所支持的最低版本是 Windows 10 64 位。

### Windows 安装说明

Windows 版的安装程序是一个 `.zip` 压缩包，通常命名会是 `Cocos ICE_vX.X.X_20XXXXXX_win.zip`，其中 `vX.X.X` 是 Cocos ICE 的版本号，如 v1.1.1，后面的一串数字是版本日期编号。

双击或右键解压压缩包，然后双击打开解压后的文件夹内的 `Cocos ICE.exe` 文件，就可以开始使用了。

> **注意**：Cocos Creator 将会占据系统盘中大约 1.85 GB 的空间，请在安装前整理您的系统盘空间。

### Mac 安装说明

Mac 版 Cocos ICE 的安装程序是一个 `.zip` 压缩包，通常命名会是 `Cocos ICE_vX.X.X_20XXXXXX_mac.zip`，其中 `vX.X.X` 是 Cocos ICE 的版本号，如 v1.1.1，后面的一串数字是版本日期编号。

双击或右键解压压缩包，然后将解压后的文件夹内的 `Cocos ICE.app` 拖拽到您的应用程序文件夹，或任意其他位置。然后双击拖拽出来的 `Cocos ICE.app` 就可以开始使用了。

> **注意**：由于 Mac 的限制，从 zip 解压安装的应用必须移动到任意其他文件夹。解压后若不移动文件位置，系统会识别它仍然在临时文件夹，导致运行失败。

## 3. 定制安装包

各公司可以对 ICE 编辑器进行个性化定制。

### 可定制内容

- 接入公司内部素材库、课程管理相关系统的服务端。
- 配置公司专属组件库、模板库。
- 配置工程位置、分辨率、字体、字号预设、颜色预设、多端显示控制、属性分组等。
- 扩展部分右键菜单；自定义面板和部分功能。

### 定制方法

#### 1. 打开编辑器文件夹

- Windows 打开 `Cocos ICE.exe` 所在的文件夹。
- Mac 右键 `Cocos ICE.app` ，点击 **显示包内容**。

#### 2. 配置信息

- global 全局配置信息：打开 `home/.EduEditor/settings/edu-settings.json` ，配置全局信息。
- editor 编辑器配置信息：打开 `编辑器/builtin/edu-editor/.EduEditor/settings/edu-settings.json` ，配置编辑器信息。
- project 项目配置信息：打开 `项目/settings/edu-settings.json` 配置项目设置。

详细配置说明见 [**配置信息**](../../developer/configure/index.md) 。

#### 3. 二次打包分发

配置完成后，研发可以将 Windows 的编辑器文件夹或 Mac的 `Cocos ICE.app` 压缩为 `.zip` 压缩包，统一分发给公司使用 ICE 编辑器的人员。
