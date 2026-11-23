# 筋斗云 JindouYun V1.0.3 正式版

V1.0.3 是个易用性小升级，基于 AimiliVPN V2.1.5（作者 baoweise-bot，GPL-3.0）二次开发。此前版本的功能（Scamalytics 欺诈分纯净度、列表按纯净度排序、纯净度优先自动切换、节点批量检测、面板内代理设置等）全部保留，本版只补一个洞：

## 本版改进

1. **Scamalytics 的 Key 可以直接在面板里填了** —— V1.0.2 换上 Scamalytics 欺诈分之后，Key 只能改服务器上的环境变量，会用的人不多。这版在「网关设置与自检」弹窗里加了「IP 纯净度数据源（Scamalytics）」区块：把欢迎邮件里的 user / key / 接口主机粘贴进去保存即可，立即生效、不用重启，弹窗里还能看到今日已用次数。配置存在服务器本地文件（权限 0600），面板回显只显示 Key 的末 4 位掩码，全文不会下发到浏览器。原来用环境变量注入的方式继续有效，面板保存优先；点「清除」可退回环境变量或兜底数据源。

## 说明

- 本项目是 YouTube 频道《八戒带你玩科技》的配套开源项目，搭建和使用教程见频道视频。
- 频道主页：https://www.youtube.com/@bajiedainikeji
- TG 交流群：https://t.me/bajiewankeji
- 原项目：https://github.com/baoweise-bot/aimili-vpngate （GPL-3.0，本项目按相同许可发布）
- VPNGate 节点是志愿者提供的家用电脑，上下线频繁是常态，不要把它当付费机场用。
