# luci-app-adguardhome

LuCI support for AdGuardHome — 新版 LuCI **JS 客户端框架**版本。

本项目由 [sirpdboy/luci-app-adguardhome](https://github.com/sirpdboy/luci-app-adguardhome) 的 Lua 版本移植而来，将全部 Lua 前端（controller / CBI / view）转换为新版 LuCI 客户端 JS 框架，功能与界面与原 Lua 版本保持一致；后端（`init.d` procd 脚本 + `usr/share/AdGuardHome/*.sh`）原样保留。

## 说明

- 菜单：`root/usr/share/luci/menu.d/luci-app-adguardhome.json`
- 视图：`htdocs/luci-static/resources/view/AdGuardHome/{base,log,manual}.js`
- 权限：`root/usr/share/rpcd/acl.d/luci-app-adguardhome.json`（纯 `fs`/`uci` RPC，无自定义 Lua）
- 汉化：`po/zh_Hans/adguardhome.po`

## 特性

- 完全使用新版 LuCI 客户端 JS 框架，前端不含任何 Lua 代码
- 状态横幅（运行状态 + 打开 Web 界面）
- 三标签页设置（主要设置 / 内核设置 / 其它设置）
- 一键更新/强制更新核心，实时显示更新日志
- 网页内 bcrypt 修改管理密码
- 日志查看（正/逆序、本地时间、下载、清空）
- CodeMirror 在线 YAML 编辑器（保存前 `--check-config` 校验）

## 安装

将本仓库加入 OpenWrt 的 feeds（或将 `luci-app-adguardhome/` 目录放入 `package/`），然后：

```
make menuconfig  # 勾选 LuCI → Applications → luci-app-adguardhome
make package/luci-app-adguardhome/compile
```

## 致谢

基于 [sirpdboy/luci-app-adguardhome](https://github.com/sirpdboy/luci-app-adguardhome)（Apache-2.0）。
