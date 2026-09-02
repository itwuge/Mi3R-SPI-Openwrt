# OpenWrt 自动编译模板

> 专为 **Xiaomi MiR3 硬改 SPI 版** 打造的 GitHub‑Actions 自动固件构建模板
>
> ### 📖 在线文档站
>
> 👉 **[📚 点击访问项目静态文档站](https://itwuge.github.io/Mi3R-SPI-Openwrt/)**

## 📋 概要

本仓库基于 GitHub‑Actions 实现 ImmortalWrt / OpenWrt 固件全自动编译。

- 🖱️ 支持网页手动一键触发编译
- 📦 编译产物自动保存为 Artifact（保留 30 天）
- 🚀 可选择自动发布到 GitHub Release，方便下载分享
- 📄 **内置 GitHub Pages 静态文档站**，项目文档直接在线浏览

## ✨ 功能特性

- ✅ **手动触发编译**：GitHub Actions 页面一键运行
- ✅ **分支 / Tag 编译**：推送 `v*` / `openwrt‑v*` 标签自动启动构建
- ✅ **目标设备**：小米路由器 3 硬改 SPI（MediaTek mt7620a）
- ✅ **Artifact 产物上传**：固件、日志自动归档，保存 30 天
- ✅ **Release 自动发布**：构建成功自动生成 Release，附带固件附件
- ✅ **Rust 编译支持**：内置 Rust 编译环境，支持 OpenClash 等 Rust 组件构建
- ✅ **GitHub Pages 静态文档站**：`docs/` 目录自动部署项目文档、刷机教程、FAQ

## 📟 支持设备

**Xiaomi Mi Router R3（小米路由器 3）硬改 SPI 版本 MediaTek mt7620a**

> 
> ⚠️ **仅硬改 SPI 版本可用，原版 NAND 请勿刷入，会直接变砖**

- 自定义网口配置脚本 `02_network`，修复网口颠倒
- 适配 SPI 闪存分区布局 `mt7620.mk`
- 定制设备树 `mt7620a_xiaomi_miwifi‑r3mi.dts`
  - LED 三色灯控制（蓝 / 黄 / 红）
  - Reset 复位按键
  - SPI‑NOR 完整分区表：u‑boot /env/firmware/crash/reserved/ Bdata
  - 以太网、USB、板载 WiFi、PCIE 无线完整硬件适配

## 🚀 快速开始

#### 1. Fork 仓库

点击仓库右上角 **Fork**，复制到你自己的 GitHub 账号。

#### 2. Secrets 配置

进入仓库：`Settings → Secrets and variables → Actions`

- 如需自动发布 Release：添加 Secret：`ACTIONS_TRIGGER_PAT`，填入你的 PAT‑classic token

> 
> 默认编译无需额外密钥，如需高级功能可在此添加环境变量。

#### 3. 开启 GitHub Pages

1. 仓库 → `Settings → Pages`
2. Source：选择 **GitHub Actions**
3. 保存，推送 `docs/` 下文档会自动部署静态网站

#### 4. 目录说明

```
仓库根目录
├── .github/workflows/
│   └── Mi3R SPI Build.yml    		 	# GitHub Actions 自动编译脚本
├── Mi3R SPI/                  			# 设备补丁、配置文件
│   ├── .config                			# OpenWrt 编译配置
│   ├── 02_network             			# 网口映射补丁
│   ├── mt7620.mk              			# image 生成配置
│   ├── mt7620a_xiaomi_miwifi‑r3mi.dts  # SPI 设备树
│   ├── dl/                    			# 预下载文件
│   ├── breed‑mt7620‑xiaomi‑mini.bin 	# Breed 引导
│   └── eeprom.bin             			# wifi eeprom 参数
├── docs/                     	 		# mkdocs 文档
└── README.md
```

#### 5. 自定义硬件配置

配置文件存放于 `Mi3R SPI` 目录，按需修改适配你的硬件：

- `02_network`：网口 LAN/WAN 顺序配置
- `mt7620.mk`：固件镜像编译规则、文件系统设置
- `mt7620a_xiaomi_miwifi‑r3mi.dts`：设备树，GPIO、LED、按键、SPI 闪存分区
- `Makefile`：Rust 编译器版本配置

#### 6. 触发编译

工作流支持三种触发方式：

1. **网页手动运行**：Actions → 选择工作流 → `Run workflow`
2. **Tag 推送触发**：推送标签如 `v1.0.0`、`openwrt‑v25.12`，自动构建
3. **Repository dispatch**：API 远程调用触发

#### 7. 完整构建流程

1. Checkout 本仓库配置文件
2. 初始化 Ubuntu 构建环境，设置时区 `Asia/Shanghai`
3. 拉取 ImmortalWrt 源码
4. 覆盖自定义文件：设备树、网口脚本、目标平台 Makefile
5. 更新 & 安装 feeds 软件源
6. 生成默认配置、预下载源码包
7. 编译固件（多线程优先，失败自动降级单线程输出完整日志）
8. 上传固件产物 Artifact（保留 30 天）
9. 构建成功自动创建 GitHub Release，打包固件附件

#### 8. 获取编译结果 & 文档访问

- **Actions 页面**：查看完整构建日志，排查编译报错
- **Artifacts**：下载临时编译产物，30 天后自动删除
- **Releases**：正式发布固件，永久保存，可对外分享下载
- **静态文档站**：`https://你的用户名.github.io/仓库名/`，查看完整项目文档

## 🛠️ 工作流信息

- 固件编译工作流：`.github/workflows/Mi3R SPI Build.yml`
- 静态站部署工作流：`.github/workflows/pages‑deploy.yml`
- 时区：`TZ: Asia/Shanghai`
- 构建运行环境：`ubuntu‑22.04`
- 默认编译版本：ImmortalWrt `v25.12.0`

## 📝 设备配置详解
#### `breed-mt7620-xiaomi-mini.bin`

`小米3mini breed固件`, 用编程器 刷入SPI 8 脚芯片 救砖。

#### `eeprom.bin`

`在 breed 里面刷入`，解决无 Mac地址

#### `02_network`

`/etc/board.d/02_network`，修正小米 R3 SPI 硬改后的 LAN/WAN 端口映射，解决网口颠倒问题。

#### `mt7620.mk`

目标平台镜像编译脚本，修改子目标、设备配置、文件系统类型。

#### `mt7620a_xiaomi_miwifi‑r3mi.dts`

设备树核心：定义 GPIO、LED、按键、SPI 闪存分区、以太网、USB、WiFi 硬件资源。

---

🙏 **致谢**


<img src="https://github.com/immortalwrt.png" width="50"/> [ImmortalWrt](https://github.com/immortalwrt/immortalwrt) 开源项目


<img src="https://github.com/yuos-bit.png" width="50"/> [yuos‑bit](https://github.com/yuos-bit/AutoBuild%E2%80%91OpenWrt) 自动编译模板


感谢各位大佬硬件硬改方案、固件开发与社区贡献。

---

## 📄 许可证

**MIT License**

> 
> 本项目仅供学习研究使用，请勿用于商业用途。
> 刷写固件有风险，硬件变砖自行承担风险。