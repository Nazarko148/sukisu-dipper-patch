# SukiSU-Ultra Kernel for Xiaomi Pad 7 Pro (muyu)

为小米平板 7 Pro 编译集成 SukiSU-Ultra 的 GKI 内核。

## 信息

| 项目 | 详情 |
|------|------|
| 设备 | Xiaomi Pad 7 Pro (muyu) |
| Android 版本 | Android U / HyperOS |
| 内核版本 | 6.1.68 |
| 内核源码 | [MiCode/Xiaomi_Kernel_OpenSource](https://github.com/MiCode/Xiaomi_Kernel_OpenSource) (分支: `muyu-v-oss`) |
| Root 方案 | [SukiSU-Ultra](https://github.com/SukiSU-Ultra/SukiSU-Ultra) |
| 编译器 | Kei Space Clang `r547379` |
| 配置 | `gki_defconfig` + 合并 `vendor/pineapple_GKI.config` / `vendor/muyu_GKI.config` |
| 编译方式 | GitHub Actions (云端) |

## 使用方法

### 1. Fork 本仓库

点击右上角 Fork 按钮，将仓库 Fork 到你自己的 GitHub 账号。

### 2. 运行编译

1. 进入你 Fork 后的仓库页面
2. 点击 **Actions** 标签页
3. 选择左侧的 **Build SukiSU-Ultra Kernel for Xiaomi Pad 7 Pro (muyu)**
4. 点击右侧 **Run workflow** 按钮
5. 按需填写 `sukisu_tag`（默认 `main`），然后启动工作流

### 3. 下载产物

编译完成后：
- 在 Actions 运行页面下载 `SukiSU-muyu-boot` artifact 压缩包
- 解压后取出其中的 `boot.zip`

### 4. 刷入设备

1. 将从 artifact 中解压出来的 `boot.zip` 传输到平板
2. 重启进入支持该设备的自定义 Recovery
3. 刷入 `boot.zip`
4. 重启系统
5. 安装 [SukiSU-Ultra Manager APK](https://github.com/SukiSU-Ultra/SukiSU-Ultra/releases) 检查是否生效

> ⚠️ **风险提示**：刷机有风险，请确保已备份原始 boot 镜像。

## 工作流做了什么

工作流会自动完成以下步骤：

1. 拉取 Xiaomi Pad 7 Pro 官方开源内核分支 `muyu-v-oss`
2. 安装 GKI 6.1 编译所需依赖
3. 集成指定版本的 SukiSU-Ultra
4. 先生成 `gki_defconfig`，再合并 `vendor/pineapple_GKI.config` 与 `vendor/muyu_GKI.config`
5. 启用 `CONFIG_KSU=y` 与 `CONFIG_KPM=y`
6. 编译内核并打包为 AnyKernel3 可刷入 zip
7. 使用固定提交的 AnyKernel3 打包，避免上游 `master` 变化导致结果漂移

## 自定义

修改 `.github/workflows/build.yml` 中的 `env` 或 `workflow_dispatch` 参数可以调整：

- `kernel_branch`: 内核源码分支（默认 `muyu-v-oss`）
- `base_defconfig`: 基础 GKI defconfig
- `device_configs`: 需要合并的设备配置片段
- `sukisu_tag`: SukiSU-Ultra 分支或标签

## 说明

这个仓库当前面向 GKI 设备流程，不再使用原先针对 `dipper` 4.9 内核的旧补丁构建逻辑。
