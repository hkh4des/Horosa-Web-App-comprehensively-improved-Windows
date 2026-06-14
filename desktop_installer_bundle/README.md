# Horosa Windows Desktop Bundle

这个目录承载 Horosa v2.6.7 Beta Windows 桌面应用工程。

- 当前发布版本：`2.6.7 Beta`（安装器版本号为 `2.6.7`）
- 发布定位：Mac 端 v2.6.7 产品面同步（古典占星补全：古典参数 / 格局分析 / 围攻详断 + AI 古典挂载 + Info 速览 + 埃及历高纬度天狼偕日升修复）+ **后端 AI 代理与缓存加固**（流式工作线程池 + ParamHash 本地缓存治理）。后端 Java 改动 → 已重建 `astrostudyboot.jar`（324,257,816 B）；v2.6.6 及更早所有功能全部保留
- 用户入口：`release/Horosa-Setup-2.6.7.exe`
- 图标资产：`assets/horosa_setup.ico` 与 `assets/horosa_setup_badge.png`

## 目录职责

- `electron/`：Electron 主进程、预加载脚本、窗口管理、Python/Java/Web 服务编排、运行时健康检查、启动控制台与本地日志。
- `scripts/`：renderer 构建、runtime staging、项目路径解析、版本资产整理、安装器校验和干净机器冷/热启动检查。
- `assets/`：安装器图标、README 展示 icon、NSIS 定制资源与安装器图片。
- `release/`：本地安装器输出目录。

## 本地构建

1. 在主项目 `astrostudyui` 中执行 `npm run build:file`
2. 在本目录执行 `npm run build:desktop`
3. 需要完整安装器时执行 `npm run dist:win`

`npm run dist:win` 会重新构建 file-mode 前端、准备 Python/Java/Node/Web runtime、打包 runtime payload、生成 `win-unpacked`，再输出 NSIS 安装器。

## 开发验证

- `npm run dev`：重建 renderer、重新 stage runtime，再启动 Electron。
- `npm run dev:fast`：仅适合不涉及前端改动的快速调试。
- 如果改了 `astrostudyui`，不要跳过 `build:renderer`。
- `scripts/installer_custom_dir_smoke.py`：验证安装器自定义目录、卸载程序和快捷方式。
- `scripts/installed_desktop_smoke_check.py`：启动安装后的 App，跑桌面 runtime 和重点 UI 用例。
- `scripts/clean_machine_cold_warm_check.py`：用隔离 `LOCALAPPDATA`、`APPDATA`、`TEMP`、`TMP` 模拟干净机器冷/热启动。
- `scripts/verify_kentang_runtime_endpoints.py`：覆盖 kentang/kin 17 个 `/pan` 端点，并在之后回打普通 chart。

## 产物

- 安装器输出到 `desktop_installer_bundle/release/`
- Electron 会把完整运行时打进 `extraResources/app-runtime/`
- 默认安装路径：`%LocalAppData%\Programs\Horosa`
- 安装器支持用户选择安装目录；选择受限目录时可触发 Windows 提权，安装前会校验目录可创建、可写。
- 正常用户不需要额外安装 Python、Java、Node.js、Maven 或前端工具链。

## v2.6.7 Beta 发布口径

v2.6.7 Beta 对齐 Mac 端 v2.6.7 产品面（**古典占星补全**：本命盘新增完整古典 / 希腊占星参数与格局，分列「古典」「格局」两个标签——**古典参数**：出界（Out of Bounds）+ 偕日相（含偕日升/没）+ 喜乐宫 + 昼夜宗派 + 野逸 + 度数明暗空烟与阴阳度 + 特殊度数 + 二十八月站 + 远地点 + 单度主星/九分/Darijan；**格局分析**：古典格局（护卫/优势相位/度数围攻）+ 相位动态（入相出相/左右旋/传光/聚光/不合意/交点弯曲）+ 逐题主星 + 偶然尊贵 + 比尼/王者恒星 + 行星时 + 埃及历 + 巴比伦参照星 + 交食食分 + 全身部位 melothesia；**围攻详断（十六式）**：三围（火土凶/金木荣富/日月耀贵）+ 春秋势 + 宰执夏冬 + 协防截击 + 围魏救赵 + 日木互容制约 + 逆行，附断语；**AI 古典挂载**：AI 分析与导出快照新增「古典」段，导出/导出设置/挂载/储存四处一致，老用户分段设置自动并入；**Info 速览**：「信息」标签新增格局速览；**埃及历高纬度修复**：修天狼偕日升在高纬度下纪年与日期不一致）+ **后端 AI 代理与缓存层加固**：流式工作线程池（标记 `STREAM_WORKER_POOL`）+ ParamHash 本地缓存治理（标记 `paramhash.cache.local.maxmb`）。后端 Java 改动 → **已重建 `astrostudyboot.jar`（324,257,816 B）**；命盘其余计算与 v2.6.6 完全一致。v2.6.6（排盘计算修正批 + 主限法大升级 宿命点应星/年数 3000/`_wireRev` v12 + 修 Windows #23/#24/#25 + 全 UI 扫雷 + Windows 壳层加固 9 项）/ v2.6.5（合盘交互链重建 + 起课时间 13 技法）/ v2.6.4（恒星黄道 47 岁差全栈 + AI 四同步 + AI 报告 v1 + 启动健壮性 + #21）/ v2.6.3 / v2.6.2（#18 升级安装修复）/ v2.6.1 及更早所有功能全部保留。Windows 发布包必须保持自包含。

这一版重点包括：

- 太乙、金口诀、皇极经世、五兆、太玄、荆诀、神易数、Kin Astro、七政四余、奇门等后端接入
- 三式合一中奇门/太乙走 kentang，六壬继续走本地实现
- 管理命盘/事盘保留结构化数据、快照、标签、原始 payload 与 JSON 导入导出
- AI 导出分段读取结构化后端数据
- 设置、窗口尺寸和必要 UI 选项持久化
- 启动控制台、窗口恢复、明暗主题、紫微四化盘、八字细盘滚动与桌面 harness 加固

正式 GitHub Release 的标准资产是：

- `Horosa-Setup-2.6.7.exe`
- `Horosa-Setup-2.6.7.exe.blockmap`
- `latest.yml`
- `SHA256SUMS.txt`

公开下载入口是 `Horosa-Setup-2.6.7.exe`。`latest.yml`、`.blockmap` 与 `SHA256SUMS.txt` 用于更新和校验流程。

当前安装包按用户要求未做 Authenticode 签名。发布前仍需明确告知：这可能触发 Windows SmartScreen 提示，但不代表 Java/Python/接口不匹配。
