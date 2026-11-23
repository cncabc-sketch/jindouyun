# 修改记录（相对于 AimiliVPN v2.1.5）

本项目（筋斗云 JindouYun）fork 自 [baoweise-bot/aimili-vpngate](https://github.com/baoweise-bot/aimili-vpngate) v2.1.5，
按 GPL-3.0 要求在此列明本项目对原文件的全部修改。功能修改涉及两个文件：
`vpngate_manager.py`、`proxy_server.py`；另有第 6 项更名改动涉及
`install.sh`、`compose.yaml`（仅仓库地址与展示文案）。其余文件与原版
v2.1.5 一致（原版 README 移至 `docs/UPSTREAM_README.md` 保存）。

## 2026-11-21

1. **节点批量检测**（`vpngate_manager.py`）
   节点列表工具栏新增「批量检测本页」按钮，一次检测当前页的 5 个节点。

2. **国家筛选修复**（`vpngate_manager.py`）
   修复国家筛选被锁定为单一国家的问题：国家下拉恢复为按当前节点列表
   动态生成，并新增「恢复全部国家」重置入口。

3. **面板内代理设置**（`vpngate_manager.py`、`proxy_server.py`）
   代理设置窗口新增代理账号、密码（留空保持不变）与协议类型
   （HTTP+SOCKS5 / 仅 HTTP / 仅 SOCKS5）设置；协议类型经
   `LOCAL_PROXY_MODE` 环境变量传给代理进程，修改后自动重启服务生效。

4. **IP 纯净度检测**（`vpngate_manager.py`）
   新增 `/api/check_purity` 接口与节点表「纯净度」列。数据源为
   proxycheck.io（免 Key，含 risk 分与代理标记），异常时以 ip-api.com
   的 proxy/hosting 标记兜底；结果按 IP 缓存 7 天
   （`vpngate_data/purity_cache.json`）。工具栏新增「检测纯净度」批量按钮。

## 2026-11-23

5. **纯净度优先的自动切换**（`vpngate_manager.py`）
   新增 `sort_candidates_purity_first()`，`auto_switch_node()` 在
   固定地区 / 自动模式下改用该排序：先按纯净度从高到低（取 7 天缓存），
   同分再按延迟从低到高；无缓存的候选在切换时按延迟取前 5 个现测
   （`PURITY_SWITCH_LIVE_CHECK_LIMIT`，受数据源每日额度限制），
   现测结果回写缓存；纯净度仍未知的候选排在最后。切换日志会打印
   首选节点的纯净度与延迟。固定 IP 模式行为不变。

6. **更名为「筋斗云 JindouYun」并迁移仓库**（`vpngate_manager.py`、
   `proxy_server.py`、`install.sh`、`compose.yaml`）
   项目更名发布至 `cncabc-sketch/jindouyun`：更新检查仓库
   （`GITHUB_REPOSITORY`）、面板页脚链接、`install.sh` 默认仓库与
   `compose.yaml` 镜像地址改指新仓库；登录页 / 面板标题、代理认证
   realm、安装脚本文案中的产品名改为「筋斗云 JindouYun」。
   VPNGate 节点列表镜像地址（`baoweise-bot.github.io`）为上游维护的
   数据源，保持原样。

7. **测试同步**（`tests/test_manager_logic.py`）
   第 4 项新增纯净度列后，节点表为 8 列，上游测试中空状态行
   `colspan="7"` 的断言已过时，改为 `colspan="8"`；另更新版本检查
   测试中的期望仓库地址为新仓库。
