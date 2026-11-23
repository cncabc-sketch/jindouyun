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

8. **后台纯净度自动检测**（`vpngate_manager.py`）
   新增 `auto_check_purity()`，在每轮周期维护（约 21 分钟）结束时
   自动执行：对尚无新鲜缓存分数的「可用」节点按延迟从低到高补测
   纯净度，每轮最多 8 个（`PURITY_AUTO_CHECK_LIMIT`），避免耗尽
   proxycheck.io 每日 100 次免费额度；额度用完时本轮自动停止并
   记录日志。此前纯净度仅靠面板手动检测与切换时现测（第 5 项），
   现在面板分数会随后台周期自动补齐，切换排序也不必再等现测。

9. **面板细节去上游化**（`vpngate_manager.py`、`tests/test_manager_logic.py`）
   顶部 Telegram 按钮原指向原作者的 `t.me/arestemple`，改为指向
   YouTube 频道《八戒带你玩科技》主页，图标与配色换成 YouTube 风格；
   版本标签由「V2.1.5 正式版」改为「V2.1.5 筋斗云版」
   （`APP_VERSION_LABEL`、页面兜底文案与测试断言同步）。
   另：更新检查改指本仓库后，仓库尚无 Release 时接口返回 404、
   面板报红，已在本仓库发布 v2.1.5 Release 解决。

10. **发布工作流适配**（`.github/workflows/release.yml`、`RELEASE_NOTES.md`）
    首次打 v2.1.5 标签时发布流水线失败：GitHub 最新运行镜像
    （Ubuntu 26.04）已下架 Python 3.9，`setup-python` 在测试开始前
    就报错，整条流水线被跳过。测试矩阵据此移除 3.9，保留 3.11 /
    3.13（本项目实际运行环境为 Ubuntu 24.04 的 Python 3.12）。
    同时流水线里写死的 Release 标题由「AimiliVPN …」改为
    「筋斗云 JindouYun …」，`RELEASE_NOTES.md` 换成本项目的
    发布说明（上游原版说明可从原仓库查看）；测试中对该标题的
    一致性断言同步更新。

11. **版本号重启为 1.0.0**（`VERSION`、`vpngate_manager.py`、`install.sh`、发布流水线与测试）
    筋斗云是独立项目，版本号不沿用上游的 2.1.5，首个正式版定为
    V1.0.0（标签「V1.0.0 筋斗云版」）。文中表示上游底座的
    「基于 AimiliVPN V2.1.5」等表述保持不变。GitHub 上先前发布的
    v2.1.5 Release 与同名标签随之删除，改发 v1.0.0。

12. **面板新增 TG 交流群按钮**（`vpngate_manager.py`、`tests/test_manager_logic.py`）
    顶部在 YouTube 频道按钮旁新增「TG 交流群」按钮，指向频道官方群
    `https://t.me/bajiewankeji`，配 Telegram 蓝与纸飞机图标；测试同步
    断言该链接出现在面板 HTML 中。

13. **移除 VPS 购买推荐**（`vpngate_manager.py`、`tests/test_manager_logic.py`）
    删除上游遗留的右侧「VPS购买推荐」浮标及其弹窗：弹窗内含上游的
    RackNerd / 搬瓦工推广链接、第三方论坛链接与原作者的捐赠地址，
    均与本项目无关，一并移除（含相关 CSS 与 `openVpsModal` /
    `closeVpsModal` 脚本）；测试断言改为确认浮标与弹窗均不存在。

14. **纯净度主数据源换 Scamalytics + 列表按纯净度排序**（`vpngate_manager.py`、`tests/test_manager_logic.py`）
    proxycheck.io 的查询带 `seen=1&days=7&port=1`，问的是「这 IP 最近
    当过代理没有」——VPNGate 节点几乎个个当过，220 个缓存 IP 里
    172 个被齐刷刷扣成 30 分封顶，毫无区分度（同 IP 在 ping0.cc 上
    风控值仅 1%）。新版新增 `check_ip_purity_scamalytics()`：配置
    环境变量 `SCAMALYTICS_API_USER` / `SCAMALYTICS_API_KEY`（可选
    `SCAMALYTICS_API_HOST`）后以 Scamalytics v3 欺诈分为主数据源
    （纯净度 = 100 − 欺诈分），实测真脏的 IP 两边都是最低分
    （1↔0）、被扣死的中间段能分开（30 → 63）；API 另会识别挂在
    商业住宅代理网络里的 IP 并扣分（网页版头条分不含此项）。proxycheck.io 退为
    兜底并去掉 seen/port 参数，ip-api.com 仍是最后兜底；Scamalytics
    失败或本地日额度（150 次/天，免费档 5,000 次/月）用完时静默
    回退。新增 `purity_entry_fresh()` 统一四处缓存新鲜度判断：配置
    Key 后旧数据源的缓存条目视为过期，自动重新检测。API Key 在
    请求 URL 里，HTTP 错误一律脱敏重抛，绝不把 URL 带进面板可见的
    报错。前端 `stableSortNodes()` 改为先按纯净度从高到低排
    （未检测的排最后），再按节点质量分排。版本号升至 1.0.2，
    新增 `PuritySourceTests` 共 10 个测试覆盖打分映射、类型映射、
    错误脱敏、额度、回退链、新鲜度与排序断言。
