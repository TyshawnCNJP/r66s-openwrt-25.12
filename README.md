# OpenWrt 25.12 极简旁路由固件 (Lunzn FastRhino R66S)

基于 OpenWrt `25.12` 官方分支（Linux 6.12 LTS 内核 + `apk` 包管理器）与 GitHub Actions 自动化编译的 **Lunzn FastRhino R66S (RK3568)** 专属旁路由固件。

---

## ✨ 核心特性

* **极简纯净架构**：移除冗余后台插件，保留原生 `dnsmasq-full`（支持 `nftset` 硬件级分流）与多核数据包转向（`Packet Steering`），轻松跑满 2.5G 物理网口带宽。
* **旁路由开箱即用**：
  * **管理地址**：`http://192.168.50.2`（默认无密码 / `root`）
  * **默认网关 / DNS**：`192.168.50.1`（无缝对接华硕主路由）
  * **双网口盲插**：`eth0` 与 `eth1` 已合并至 `br-lan` 网桥，网线插 WAN 或 LAN 丝印口均可直接通信。
  * **单臂防回环**：默认关闭 DHCP 与 IPv6 通告，开启 LAN 区域 IP 动态伪装（`Masquerading`），避免异步路由丢包。
* **完整汉化与原生指示灯**：
  * 预置简体中文界面（LuCI、软件包管理、终端）。
  * 绑定 `r8169` 驱动 LED 触发器，网口连接常亮、收发数据闪烁。
* **集成组件**：
  * `ttyd` 网页终端
  * `Argon` 现代主题
  * `parted` / `losetup` / `resize2fs`（支持 EXT4 分区无损在线扩容）

---

## ⏰ 自动化编译机制

* **定时构建**：每月 **1 号** 与 **15 号** 北京时间凌晨 **04:00**（UTC `20:00`）自动拉取 OpenWrt `25.12` 最新源码编译。
* **手动构建**：随时进入仓库 **Actions** -> 选择 `Build OpenWrt 25.12 for Lunzn FastRhino R66S` -> 点击 **Run workflow** 触发。
* **产物下载**：构建完成后，在对应运行记录底部的 **Artifacts** 中下载 `R66S-OpenWrt-25.12-Firmware`，解压后使用 `*-ext4-sysupgrade.img.gz`。

---

## 🚀 刷机与升级规范

### 1. 首次刷写 TF 卡
使用 **balenaEtcher** 或 **Rufus** 将 `*-ext4-sysupgrade.img.gz` 直接写入 TF 卡，插入 R66S 通电启动即可。

### 2. 后台在线升级（Sysupgrade）
登录 `http://192.168.50.2` -> **系统** -> **备份/升级** -> 上传 `*-ext4-sysupgrade.img.gz`：
* **日常版本小升级**（仅更新内核与软件版本）：**保持勾选「保留配置」**，升级后节点订阅与设置无缝保留。
* **初始化脚本变更升级**（修改了 `99-custom-network` 里的网口、IP 或 LED 规则）：**务必取消勾选「保留配置」**，确保 `uci-defaults` 初始化脚本在首次启动时重新执行。

---
