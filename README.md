<div align="center">

# 筋斗云 JindouYun

**一个筋斗，十万八千里。**

面向 Linux VPS 的 VPNGate 节点管理与 HTTP / SOCKS5 代理网关（增强版）

[![License](https://img.shields.io/badge/License-GPL--3.0-334155?style=flat-square)](LICENSE)

</div>

## 这是什么

筋斗云把 [VPNGate](https://www.vpngate.net/) 的免费节点变成一个**能自己照顾自己的代理网关**：

- 自动拉取、检测全球 VPNGate 节点（OpenVPN），在 Web 面板里一键连接
- 对外提供带账号密码的 HTTP / SOCKS5 代理（默认端口 7928）
- 可按**国家 + IP 类型（住宅 / 机房）**锁定出口，节点失效自动切换到同地区的备用节点
- 每个节点带 **IP 纯净度评分**，自动切换时优先挑最干净的出口

适合：给自己的设备、脚本、AI 工具准备一个干净的住宅 IP 出口；注册海外服务时先看一眼 IP 干不干净。

> 本项目是 YouTube 频道 **《八戒带你玩科技》** 的配套开源项目，搭建和使用教程见频道视频。
>
> 频道主页：[youtube.com/@bajiedainikeji](https://www.youtube.com/@bajiedainikeji)
> TG 交流群：https://t.me/bajiewankeji
> 

## 相比原版的增强

本项目基于 [AimiliVPN](https://github.com/baoweise-bot/aimili-vpngate) v2.1.5 二次开发，以下功能为本项目新增 / 修复（详见 [CHANGES.md](CHANGES.md)）：

1. **节点批量检测** —— 列表页一键检测本页全部节点，不用一个个点
2. **国家筛选修复** —— 修复国家下拉被锁死、只能看到一个国家的问题
3. **面板内代理设置** —— 代理账号、密码、协议类型（HTTP+SOCKS5 / 仅 HTTP / 仅 SOCKS5）直接在设置里改
4. **IP 纯净度检测** —— 节点表新增纯净度列（数据源 proxycheck.io，ip-api.com 兜底，按 IP 缓存 7 天）
5. **纯净度优先的自动切换** —— 固定地区模式下节点失效时，先比纯净度、再比延迟；没测过的候选会在切换时现测

## 快速开始

环境：一台 Linux VPS（Ubuntu / Debian 等），root 权限，能访问 vpngate.net。

```bash
# 克隆本仓库后，用自带脚本一键源码部署
git clone https://github.com/cncabc-sketch/jindouyun.git
cd jindouyun
bash install.sh
```

部署完成后：

- Web 面板：`http://<服务器IP>:8787/<秘密路径>/`（路径在安装日志和服务状态里查看）
- 代理地址：`http://<账号>:<密码>@<服务器IP>:7928`（HTTP 与 SOCKS5 同端口）
- 服务管理：`systemctl status aimilivpn`

更完整的部署方式（Docker、二进制包等）见原版文档 [docs/UPSTREAM_README.md](docs/UPSTREAM_README.md)。

## 使用要点

- **固定地区模式**最省心：选好国家（如日本）+ 住宅 IP，节点断了会自动换同地区的，优先换纯净度高的
- VPNGate 节点是志愿者提供的家用电脑，上下线频繁是常态，**不要把它当付费机场用**
- 注册重要账号前，先在面板里点一下节点的「检测纯净度」，分数高再用

## 致谢与许可

- 本项目基于 **AimiliVPN**（作者 [baoweise-bot](https://github.com/baoweise-bot)）v2.1.5 二次开发，原项目文档保留在 [docs/UPSTREAM_README.md](docs/UPSTREAM_README.md)
- 节点数据来自 [VPNGate](https://www.vpngate.net/)（筑波大学学术研究项目）
- 本项目以 **GPL-3.0** 许可发布，与原项目保持一致；修改过的文件清单见 [CHANGES.md](CHANGES.md)

## 法律声明

本项目仅供网络技术学习与研究使用。使用者应遵守所在国家和地区的法律法规，因使用本项目产生的任何后果由使用者自行承担。
