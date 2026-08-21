<div align="center">

# 🛡️ luci-app-adguardhome

**AdGuard Home 的 LuCI 管理界面 —— 基于新版客户端 JS 框架完整重写**

![Release](https://img.shields.io/github/v/release/MinimaxFlora/luci-app-adguardhome?style=flat-square&label=Release&color=2f81f7)
![Build](https://img.shields.io/github/actions/workflow/status/MinimaxFlora/luci-app-adguardhome/build.yml?style=flat-square&label=Build)
![Downloads](https://img.shields.io/github/downloads/MinimaxFlora/luci-app-adguardhome/total?style=flat-square&label=Downloads)
![License](https://img.shields.io/badge/License-Apache--2.0-blue?style=flat-square)
![Arch](https://img.shields.io/badge/Architecture-all-brightgreen?style=flat-square)

</div>

---

## 📖 项目简介

`luci-app-adguardhome` 为 [AdGuard Home](https://github.com/AdguardTeam/AdGuardHome) 提供完整的 LuCI 图形化管理界面。

本项目由 [sirpdboy/luci-app-adguardhome](https://github.com/sirpdboy/luci-app-adguardhome)（Lua 版）移植而来：

- **前端**（Lua controller / CBI / 模板视图）全部转换为新版 LuCI **客户端 JS 框架**（`form` / `view`），功能与界面与原版保持一致，且不再依赖任何 Lua 运行时；
- **后端**（procd 守护脚本、内核更新、防火墙联动等 Shell 工具链）原样保留。

> **适用版本**：OpenWrt 23.05+ / ImmortalWrt 24.10+（需要 LuCI ≥ 22.03 的客户端 JS 框架支持）
> **适用架构**：全部（`PKGARCH:=all`，一个安装包通吃所有平台）

---

## ✨ 功能特性

| 模块 | 说明 |
| --- | --- |
| 🖥️ **状态总览** | 运行状态横幅，一键直达 AdGuard Home Web 管理界面 |
| ⚙️ **三页签设置** | 基本设置 / 内核设置 / 其它设置，全量 UCI 化，无需手工编辑配置文件 |
| 🔄 **内核管理** | 一键更新 / 强制更新 AdGuard Home 核心，实时输出更新日志 |
| 🔑 **密码管理** | 网页内 bcrypt 修改管理员密码，无需登录 AGH 后台 |
| 📜 **日志查看** | 正序 / 逆序浏览、本地时间转换、日志下载与一键清空 |
| 📝 **YAML 编辑器** | 内置 CodeMirror，保存前自动执行 `--check-config` 校验，杜绝错误配置 |
| 🧩 **DNS 重定向** | 支持 dnsmasq-upstream 等多种重定向方式，一键切换 |
| 🌐 **简体中文** | 内置 `po/zh_Hans` 完整简体中文翻译 |

---

## 📥 安装

### 方式一：Release 直接安装（推荐）

从 [GitHub Releases](https://github.com/MinimaxFlora/luci-app-adguardhome/releases) 下载对应格式的安装包（每个 Release 同时提供 ipk 与 apk 两种格式）：

**ImmortalWrt 24.10.x / OpenWrt 23.05+（opkg / ipk）**

```bash
opkg install luci-app-adguardhome_*.ipk luci-i18n-adguardhome-zh-cn_*.ipk
```

**ImmortalWrt 25.12+（apk）**

```bash
apk add --allow-untrusted ./luci-app-adguardhome-*.apk ./luci-i18n-adguardhome-zh-cn-*.apk
```

> [!NOTE]
> `luci-i18n-adguardhome-zh-cn` 为简体中文翻译包，**必须与主包一并安装**，否则界面显示英文。

### 方式二：源码编译

将本仓库添加为 feed，或直接把 `luci-app-adguardhome/` 目录放入 OpenWrt 源码树的 `package/` 下：

```bash
./scripts/feeds update -a
./scripts/feeds install -a
make menuconfig        # 勾选 LuCI → Applications → luci-app-adguardhome
make package/luci-app-adguardhome/compile V=s
```

编译产物位于 `bin/packages/<arch>/` 下，包含 `luci-app-adguardhome` 与 `luci-i18n-adguardhome-zh-cn` 两个安装包。

---

## 🚀 快速开始

1. 安装完成后，进入 **服务 → AdGuard Home**；
2. 在「基本设置」页签中启用服务 —— 首次启用会自动从 [AdGuardTeam/AdGuardHome](https://github.com/AdguardTeam/AdGuardHome/releases) 下载对应架构的核心程序（需设备可访问 GitHub，依赖 `curl` 或 `wget-ssl`）；
3. 核心就绪后，点击状态横幅中的链接即可打开 AdGuard Home Web 管理界面（默认 `http://<路由器IP>:3000`，DNS 端口默认 `5333`）；
4. 在「手动配置」页签中可直接编辑 `AdGuardHome.yaml`，保存前自动校验配置合法性，避免因配置错误导致服务无法启动；
5. 如需修改管理密码，在「内核设置」页签中直接设置即可，无需登录 AGH 后台。

---

## 🗂️ 项目结构

```
luci-app-adguardhome/
├── Makefile                            # OpenWrt 包定义（PKGARCH=all）
├── htdocs/
│   └── luci-static/resources/view/AdGuardHome/
│       ├── base.js                     # 基本设置 / 状态总览
│       ├── log.js                      # 日志查看
│       └── manual.js                   # 手动配置（YAML 编辑器）
├── po/
│   ├── templates/adguardhome.pot       # 翻译模板
│   └── zh_Hans/adguardhome.po          # 简体中文翻译
└── root/
    ├── etc/
    │   ├── config/AdGuardHome          # UCI 配置文件
    │   ├── init.d/AdGuardHome          # procd 守护脚本
    │   └── uci-defaults/               # 首次安装初始化
    └── usr/share/AdGuardHome/          # 后端 Shell 工具链（内核更新 / 防火墙 / 日志）
```

---

## 🔨 自动构建

仓库内置 GitHub Actions 工作流（`.github/workflows/build.yml`）：推送版本 tag（如 `v1.1.1`）后，自动使用 ImmortalWrt 官方 SDK 交叉编译并发布到 GitHub Releases：

| SDK | 包格式 | 产物 |
| --- | --- | --- |
| ImmortalWrt 24.10.5 | ipk | `luci-app-adguardhome_*.ipk`、`luci-i18n-adguardhome-zh-cn_*.ipk` |
| ImmortalWrt 25.12.0 | apk | `luci-app-adguardhome-*.apk`、`luci-i18n-adguardhome-zh-cn-*.apk` |

---

## ❓ 常见问题

- **界面显示英文？** 未安装翻译包 —— 请一并安装 `luci-i18n-adguardhome-zh-cn`。
- **核心程序下载失败？** 首次启用需要从 GitHub 下载核心二进制，请确认设备可以访问 GitHub，并已安装 `curl` 或 `wget-ssl`。
- **从旧版（Lua 版）升级？** 新旧版本共用 `/etc/config/AdGuardHome` 与 `/etc/AdGuardHome.yaml`，配置自动延续；建议先卸载旧包再安装新包。

---

## 📄 致谢与许可

本项目基于 [sirpdboy/luci-app-adguardhome](https://github.com/sirpdboy/luci-app-adguardhome) 移植，遵循 [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0) 开源协议。

[![Stargazers](https://img.shields.io/github/stars/MinimaxFlora/luci-app-adguardhome?style=social)](https://github.com/MinimaxFlora/luci-app-adguardhome/stargazers)
[![Fork](https://img.shields.io/github/forks/MinimaxFlora/luci-app-adguardhome?style=social)](https://github.com/MinimaxFlora/luci-app-adguardhome/forks)
