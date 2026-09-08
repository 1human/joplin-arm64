# Joplin Android ARM64 Builder

这是一个自动化构建工具，专门用于为 [Joplin](https://github.com/laurent22/joplin) 移动端生成 **ARM64 (arm64-v8a)** 架构的 Android 安装包。

## 为什么使用这个？
官方的 Joplin Android 版通常包含多个架构的库。本项目通过 GitHub Actions 专门针对 ARM64 架构进行编译，并开启了 R8 压缩，从而提供一个更轻量、更高效的版本。

## 功能特点
- **自动同步**：每天凌晨自动检测官方仓库的最新 Release。
- **极致精简**：强制过滤仅保留 `arm64-v8a` 架构，剔除冗余代码。
- **R8 优化**：默认开启 R8 代码混淆与压缩，进一步缩小 APK 体积。
- **自动发布**：构建完成后自动在本项目创建 Release 并上传带有 SHA256 校验的 APK。
- **灵活签名**：支持使用标准 Debug 密钥或用户自定义的正式密钥进行签名。

## 如何使用

### 1. 自动构建
工作流每天凌晨 02:00 (UTC) 自动运行。如果检测到官方有新版本且本项目尚未构建，则会自动开始。

### 2. 手动构建
1. 进入本仓库的 **Actions** 页面。
2. 选择 **Build Joplin ARM64 Release** 工作流。
3. 点击 **Run workflow**。
4. (可选) 输入具体的版本号（如 `v3.0.1`），留空则默认构建最新版。

## 配置自定义签名 (可选)
默认情况下，生成的 APK 使用 debug 密钥签名。若需使用自己的密钥：
1. 准备你的 `.jks` 或 `.keystore` 文件。
2. 将其转为 Base64 编码：
   - **Linux/macOS**: `base64 -i your_key.jks`
   - **Windows (PowerShell)**: `[Convert]::ToBase64String([IO.File]::ReadAllBytes("your_key.jks"))`
3. 在本仓库的 **Settings -> Secrets and variables -> Actions** 中添加以下 Secrets：
   - `SIGNING_KEY`: 密钥文件的 Base64 字符串。
   - `KEY_STORE_PASSWORD`: 密钥库密码。
   - `ALIAS`: 别名。
   - `KEY_PASSWORD`: 别名密码。

## 致谢
- 原项目：[Joplin](https://github.com/laurent22/joplin) by Laurent Cozic.

---
*声明：本项目是一个独立的构建工具，与 Joplin 官方团队无直接关联。*
