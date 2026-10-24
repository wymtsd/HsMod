# HsMod

[English](ReadMe.md) | **简体中文**

## 本 Fork：自动同步与 DLL 发布

本仓库是 [Pik-4/HsMod](https://github.com/Pik-4/HsMod) 的个人 Fork，提供 **非官方自动构建**。下方为项目说明的中文版本。

- **下载整合包**：[最新 Release](https://github.com/wymtsd/HsMod/releases/latest) 的 Assets 中选择 `HsMod-Windows-x64.zip`。关闭游戏，把包内全部内容直接解压到 `Hearthstone.exe` 所在目录，再通过战网启动；无需另装 BepInEx 或手动修改 Doorstop 配置。已有 Mod 时请先备份。
- **整合内容**：官方 BepInEx 5 最新稳定版 Windows x64、`HsMod.dll`、上游 Windows `unstripped_corlib` DLL、预设相对路径的 Doorstop 配置、安装说明和许可证。仅适用于 Windows x64；安装路径避免中文等非拉丁字符。已做结构检查，尚未实际启动游戏验证。
- **隐私范围**：在 GitHub 临时构建环境中打包，仅使用公开上游文件与自动编译的 DLL，不读取用户电脑、个人游戏配置、日志或账号令牌；不上传 PDB。公开仓库及提交仍显示正常 GitHub 作者信息。
- **下载 DLL**：[最新 Release](https://github.com/wymtsd/HsMod/releases/latest)，在 Assets 中下载 `HsMod.dll`；`SHA256SUMS.txt` 用于校验文件。
- **检查频率**：每小时整点计划检查上游 `bepinex5` 分支及官方 BepInEx 5 稳定版；GitHub 调度可能延迟。
- **同步与发布**：合并上游新提交，保留本 Fork 的 `.github/workflows` 配置；任一上游版本更新时自动编译、检查并发布整合 ZIP 和单独 DLL。相同组合已发布时跳过构建。
- **手动检查**：打开 [Sync Build Release](https://github.com/wymtsd/HsMod/actions/workflows/sync-release.yml)，点击 **Run workflow**，选择 `bepinex5` 并运行。手动运行也会跳过已发布版本。
- **版本追溯**：整合包 Release 标签为 `bundle-v1-<上游完整提交 SHA>-<BepInEx 版本>-<发行文件 ID>`，发布说明包含上游提交、实际构建提交和运行日志链接。
- **失败处理**：合并冲突或编译失败会停止流程，不发布新版本；查看 Actions 中失败步骤的日志。上游工作流变更需单独检查后更新本 Fork。
- **定时任务维护**：公开仓库连续 60 天没有仓库活动时，GitHub 可能自动停用定时任务；届时在 Actions 中重新启用。参见 [GitHub 官方说明](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#schedule)。

自动构建通过只代表编译成功，不代表已验证游戏内兼容性。单独安装 DLL 时仍需按下文配置 BepInEx 及相关依赖。源码与发布遵循原项目 AGPL-3.0 许可。


---

HsMod 是基于 BepInEx 的炉石传说插件，原项目源码位于 [Pik-4/HsMod](https://github.com/Pik-4/HsMod)。上游声明插件不收集用户信息，项目采用 AGPL-3.0，仅用于学习和研究。

[![License](https://img.shields.io/badge/license-AGPL--3.0-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)
[![BepInEx](https://img.shields.io/badge/BepInEx-5.x-green.svg)](https://github.com/BepInEx/BepInEx/releases)
[![.NET](https://img.shields.io/badge/.NET%20SDK-8.x-512BD4.svg)](https://dotnet.microsoft.com/download)
[![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20macOS%20%7C%20Linux-lightgrey.svg)](#安装)

> **注意：**中国大陆炉石客户端默认启用反作弊 SDK。插件会尝试阻止相关功能，但不能保证账号安全。

插件主要使用简体中文，英文翻译可能存在错误，部分内容尚未翻译。可修改 `HsMod/Languages` 中的 JSON 文件并重新编译，以支持其他语言。

[上游 Wiki](https://github.com/Pik-4/HsMod/wiki) 提供安装、设置、快捷键和故障排查说明。

## 快速开始（Windows）

整合包安装见顶部说明。单独 DLL 的手动安装步骤：

1. 从 [Releases](https://github.com/wymtsd/HsMod/releases/latest) 下载 `HsMod.dll`，或[从源码编译](#从源码编译)。
2. 将 [BepInEx 5 Windows x64](https://github.com/BepInEx/BepInEx/releases) 解压到炉石目录。
3. 将源码 `HsMod/UnstrippedCorlib` 内的所有 DLL 复制到 `Hearthstone\BepInEx\unstripped_corlib\`。
4. 在 `doorstop_config.ini` 设置 `dll_search_path_override = BepInEx\unstripped_corlib`。
5. 将 `HsMod.dll` 放入 `Hearthstone\BepInEx\plugins`，启动游戏。

游戏中按 Esc，在菜单中打开 Mod Settings（Mod 设置）。详见[上游安装说明](https://github.com/Pik-4/HsMod/wiki/Installation)。

## 目录

- [功能](#功能)
- [安装](#安装)
- [从源码编译](#从源码编译)
- [版本号规则](#版本号规则)
- [补充说明](#补充说明)
- [client.config](#clientconfig)
- [开发计划](#开发计划)
- [参与贡献](#参与贡献)
- [参考资料](#参考资料)

## 功能

以下为上游描述，实际可用性取决于游戏版本。

### 酒馆战棋

- 在排行榜玩家名称下显示对手分数，支持单人与双人模式，开局后台加载。
- 将每场排位的名次、分数变化和英雄记录至 CSV；图表支持今天、7 天、一个月、全部及单双人筛选。
- 商店阶段支持刷新、冻结、升级酒馆、英雄技能和双人队友面板快捷键，可重新绑定。
- 开局自动屏蔽所有对手表情，默认关闭。
- 快速战斗跳过部分动画，施法时屏蔽终结特效；也可用于佣兵 PvE 酒馆，但最终死亡结算仍可能延迟。

### 界面与操作便利

- 游戏菜单提供 Mod 设置入口，支持分类、搜索和快捷键录入；可隐藏入口，也可使用网页设置。
- 变速档位最高 8 倍，可配置至 32 倍。
- 显示并修改游戏帧率。
- 解除窗口焦点与窗口大小限制。
- 屏蔽无法匹配等弹窗。
- 屏蔽中文提示、削弱补丁提示、广告及天梯结算奖励提示。
- 屏蔽对局结束推广与结算提示。
- 屏蔽战令、成就和其他奖励提示。
- 收藏中显示大于 9 的实际卡牌数量。
- 右键收藏、英雄、卡背、特效、酒馆面板等项目时显示 DbId。
- 快捷键一键清除“新”标记，可能需要重进收藏；重启后的佣兵模式可能无效。
- 在 0 胜 0 负且没有套牌时放弃对决。
- 快捷键静音炉石。

### 自动化与挂机

- 使用 VerifyWebCredentials 登录，支持命令行启动，无需战网。
- 禁用无操作掉线。
- 自动领取竞技场、对决、佣兵等模式奖励并打开包裹。
- 空格快速开包，一次五包；可自动分解全额返尘卡牌。
- 快捷键一键分解全额返尘套牌。
- 自动举报对手；启用后可自动生成游戏日志。
- 屏蔽向暴雪发送错误报告，或在允许报告时自动退出。
- 自动轮换套牌，观战好友时自动观看双方。
- 接收指定分辨率等启动参数。

### 表情与对手信息

- 自动屏蔽表情或限制表情数量；屏蔽思考表情、鲍勃语音、匹配英雄介绍。
- 表情快捷键及无冷却表情，最短间隔 1.5 秒。
- 单独屏蔽对手卡牌特效。
- 显示完整战网昵称，在匹配过程中添加对手。
- 强制关闭主播模式，并屏蔽误触 Ctrl+Shift+S 导致名称变成英雄名。
- 点击酒馆玩家头像读取昵称。
- 显示传说之前的对手天梯排名。
- 标记对手已知卡牌。

### 皮肤与外观

- 修改匹配英雄、酒馆英雄、终结特效、匹配面板、酒馆面板、幸运币等；通过 `HsSkins.cfg` 或设置界面配置。在对局中更新时按 F4 保存后模拟断线。
- 修改卡背，在游戏内自动生效。
- 自动金卡、钻石卡。
- 佣兵随机皮肤、强制钻石等。
- 屏蔽佣兵宝藏、天梯奖励等弹窗。
- 屏蔽佣兵匹配界面缩放。

### 模拟与开发功能

- 访问炉石开发者模式。
- 快捷键模拟炉边聚会定位、拔线或断线。
- 模拟开包，可固定或随机结果，并指定类型、数量、稀有度与品质。
- 模拟 iOS、Android 等设备以领取卡背，可能需要完成一场匹配。
- 使用金币购买纳克萨玛斯、黑石山、探险者协会等冒险；卡拉赞不能进行序章。
- 强制开启卡拉赞，不能进行序章，也不能跳过尚未通关的关卡。
- Showinfo 信息展示默认使用 HTTP 58744 端口，可查看佣兵进度、开包记录等。
- 设置中可启用 `/shell` Webshell，中文显示可能乱码。
- 网页读取本地静态文件功能仍在开发，根目录为 `Hearthstone/BepInEx/HsMod`。
- 尝试禁用反作弊。

<details>
<summary>已被暴雪修复或移除</summary>

- ~~解除套牌识别限制。~~ 已修复。
- ~~零元购。~~ 已修复。

</details>

## 安装

使用 BepInEx 5；上游当前尚未适配 BepInEx 6。上游说明炉石于 2026-07-01 转为 64 位，请使用 x64 版本。安装路径避免中文等非拉丁字符。

### Windows

整合包安装见顶部说明；单独 DLL 的手动安装见[快速开始](#快速开始windows)。

BepInEx 5.4.23.2 及以上使用：

```ini
dll_search_path_override = BepInEx\unstripped_corlib
```

旧版使用：

```ini
dllSearchPathOverride=BepInEx\unstripped_corlib
```

### macOS

1. 将 [BepInEx 5 的 BepInEx_macos_universal](https://github.com/BepInEx/BepInEx/releases) 解压到炉石目录。
2. 将 `HsMod/UnstrippedCorlibUnix` 的 DLL 复制到 `/Applications/Hearthstone/BepInEx/unstripped_corlib/`。Mono 与 Unity 版本须匹配游戏。
3. 在 `run_bepinex.sh` 设置 `dll_search_path_override="BepInEx/unstripped_corlib"` 和 `executable_name="Hearthstone.app"`。
4. 将 `export ARCHPREFERENCE="arm64,x86_64"` 改为 `export ARCHPREFERENCE="x86_64"`。上游说明游戏仍使用 x86_64；命令行运行可参考[上游 #308](https://github.com/Pik-4/HsMod/issues/308)。
5. 执行 `chmod u+x run_bepinex.sh`。
6. 从战网登录 URL 获取令牌，截取 `http://localhost:0/?ST=` 之后、`&accountId=` 之前的部分，参照 [client.config](#clientconfig)。
7. 可创建 `client.config` 保存令牌。
8. 将 HsMod DLL 放入 `Hearthstone/BepInEx/plugins`。
9. 使用 `./run_bepinex.sh TOKEN` 启动；存在 client.config 时可直接使用 `./run_bepinex.sh`。

### Linux

1. 编译或下载 HsMod DLL。
2. 通过 [0xf4b1/hearthstone-linux](https://github.com/0xf4b1/hearthstone-linux) 安装炉石，通常会自动配置 client.config。
3. 将 [BepInEx 5 Unix 版](https://github.com/BepInEx/BepInEx/releases)解压到炉石目录。
4. 将 `HsMod/UnstrippedCorlibUnix` 中的 DLL 复制到 `hearthstone/BepInEx/unstripped_corlib/`。上游 UniTask 来源为 OpenMod.UniTask.2021.2.4.1 的 net48 目录。
5. 在 run_bepinex.sh 设置 `DOORSTOP_CORLIB_OVERRIDE_PATH="$BASEDIR/BepInEx/unstripped_corlib"` 和 `executable_name="Bin/Hearthstone.x86_64"`；按上游示例规范换行：`sed -i "s/\r/ /g" ./run_bepinex.sh`。
6. 未配置 client.config 时参照 macOS 步骤。
7. 将 HsMod DLL 放入 `hearthstone/BepInEx/plugins`，不存在则创建。
8. 执行 `chmod u+x run_bepinex.sh`，然后运行 `./run_bepinex.sh`。

Windows 整合 ZIP 不适用于 macOS 或 Linux。

## 从源码编译

需要 .NET SDK 8.x。输出为 `HsMod/Release/HsMod.dll`。

```bash
git clone --depth 1 --branch bepinex5 https://github.com/Pik-4/HsMod
cd HsMod
dotnet build --configuration Release --no-restore
```

编译后的 install.bat 会在默认游戏目录已配置 BepInEx 时复制 DLL；本 Fork 自动构建会跳过这个本机安装步骤。

## 版本号规则

以 `3.0.0.0` 为例：

- 第一位表示对应炉石大版本；上游举例 3 对应 26。
- 第二位表示该大版本下炉石更新的适配次数，不等同于炉石小版本号。未修改 Assembly-CSharp.dll 等文件的更新通常不增加该位。
- 第三位在 HsMod 新增功能时递增，第二位变化时归零。
- 第四位为编译版本，主要用于修复第三位版本中的问题。

游戏更新不一定导致插件失效；若仍正常工作，无需每次重新下载。功能变化可查看提交记录。

## 补充说明

1. 游戏及插件路径避免非拉丁字符。
2. 修改 Assembly-CSharp.dll 的 Mod 可能改变 IL 偏移并导致补丁失效。Mercenaries、MixMod 等 BepInEx 插件也可能修补相同方法而冲突；插件不会检测原方法是否已修改。
3. 皮肤配置首次运行生成于 `Hearthstone\BepInEx\config\HsSkins.cfg`。
4. F4 会将部分游戏信息导出至 BepInEx 目录、更新皮肤配置并重启网页服务；其他快捷键可修改。
5. 大部分功能默认需要手动开启，部分设置仅在补丁源码中说明。
6. Showinfo 默认端口为 58744，通常监听所有本地 IP；云服务器注意防火墙设置。
7. 战绩 CSV 日志默认位于 `BepInEx\HsMatch.log`，可在设置中修改。
8. 出现问题时可先备份并删除相关 cfg，再重新配置。持续出现时可向上游反馈并附配置，分享前请检查并移除私人信息。
9. GetHsLib.py 更新游戏运行库；install.bat 复制 DLL 至默认游戏目录。
10. 皮肤异常时检查 HsSkins.cfg，并可尝试备份后删除 HsMod.cfg 重新配置。
11. 设置不保存时检查其他炉石插件是否造成冲突。
12. 使用 BepInEx 5；当前尚未适配 BepInEx 6。

## client.config

可选的 client.config 支持绕过战网启动，放在 Hearthstone.exe 同目录。**整合包通过战网正常启动不需要此文件。不要公开真实登录令牌。**

```ini
[Config]
Version = 3
[Aurora]
VerifyWebCredentials = "TOKEN"
ClientCheck = 0
Env.Override = 1
Env = us.actual.battle.net
```

根据地区选择登录入口：

```text
https://us.battle.net/login/en/?app=wtcg
https://eu.battle.net/login/en/?app=wtcg
https://tw.battle.net/login/zh/?app=wtcg
https://kr.battle.net/login/zh/?app=wtcg
https://account.battlenet.com.cn/login/zh-cn/?app=wtcg
```

国服 Env 使用 `cn.actual.battlenet.com.cn`，其他地区对应令牌前两个字符。启用插件后也支持 `./Hearthstone.exe VerifyWebCredentials`，不强制要求 client.config。令牌失效后需更新。

## 开发计划

以下为上游计划：

- 整理 README、Wiki、配置与补丁关系，完善多语言。
- 对局内一键换肤，目前可能需要模拟断线。
- 重构 Showinfo 网页。
- 适配 macOS。
- 修复佣兵相关功能。

## 参与贡献

原插件开发在 [Pik-4/HsMod](https://github.com/Pik-4/HsMod) 进行。功能问题与建议可提交至[上游 Issues](https://github.com/Pik-4/HsMod/issues)。翻译文件位于 HsMod/Languages，可通过 PR 贡献修改。涉及本 Fork 自动打包或发布的问题请提交至[本仓库 Issues](https://github.com/wymtsd/HsMod/issues)。

社区：[Telegram @HearthstoneMod](https://t.me/HearthstoneMod) · [Discord](https://discord.gg/BCnhxtXHVq)

## 参考资料

1. [MixMod（4pda）](https://4pda.to/forum/index.php?showtopic=870696&st=4780#entry114865283)
2. [MixMod（GitHub）](https://github.com/DeNcHiK3713/MixMod)
3. [Hearthstone Apollo Mod](https://hearthmod.com/)
4. [从零制作 Unity 游戏的 BepInEx 插件](https://mod.3dmgame.com/read/3)
5. [BepInEx 文档](https://docs.bepinex.dev/)
6. [Harmony 文档](https://harmony.pardeike.net/articles/intro.html)
7. [CIL 指令列表](https://en.wikipedia.org/wiki/List_of_CIL_instructions)
8. [hearthstone-linux](https://github.com/0xf4b1/hearthstone-linux)
