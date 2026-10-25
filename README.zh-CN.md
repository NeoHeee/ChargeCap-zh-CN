# ChargeCap 简体中文版

ChargeCap 是 Nintendo Switch 的电池充电限制插件。它由 Atmosphère 常驻模块和悬浮菜单组成：达到设定电量后停止充电，电量下降后恢复。悬浮菜单可通过 Tesla Menu 或 Ultrahand 打开。

本仓库在原版 ChargeCap 上提供简体中文菜单与安装说明。原作者项目：[aoihera/ChargeCap](https://github.com/aoihera/ChargeCap)。

## 功能

- 充电上限可设为 50%–99%，默认 80%。
- 睡眠时限充默认开启：主机息屏充电时会短暂唤醒检查电量，达到上限后停止充电。
- 悬浮菜单显示充电状态、电量百分比、电量计原始百分比和电池电压。
- 关闭限充后，插件不再限制充电。

## 适用范围与注意事项

- 适用于已安装 Atmosphère 的 Nintendo Switch；需要 nx-ovlloader 以及 Tesla Menu 或 Ultrahand 来打开悬浮菜单。
- 限充由 Atmosphère 常驻模块执行。只安装 .ovl 悬浮菜单不会限制充电；安装包同时包含两者。
- 软件会调用 Horizon OS 电源管理服务并访问电源管理硬件接口，按原作者说明自行承担使用风险。
- 长期把电池维持在固定电量可能使系统电量计出现偏差。发现读数异常时，可关闭限充、充到 100% 后继续连接充电器约一小时，再使用至约 5%–10%。
- 关闭“睡眠时限充”后，如果主机在低于上限时进入深度睡眠，硬件仍可能继续充电；唤醒主机后才会重新检查。建议保留默认开启状态。

## 安装

1. 从仓库的 Actions → Build ChargeCap 下载最新成功构建的 ChargeCap.zip。也可以在本机按下方步骤编译。
2. 解压到 microSD 卡根目录，保留压缩包中的目录结构：
   - atmosphere/contents/42000000000000C0/：常驻限充模块
   - switch/.overlays/充电限制.ovl：悬浮菜单（Ultrahand 中显示为“充电限制”）
   - config/chargecap/lang/zh.json：简体中文菜单翻译
3. 将文件复制到正在运行的主机后，重启一次，使 Atmosphère 加载常驻模块。
4. 用 Tesla Menu 或 Ultrahand 打开 ChargeCap，设置上限并开启“启用”。

若菜单显示“限充模块未运行”，请检查 atmosphere/contents/42000000000000C0/ 是否完整、模块是否在 ovl-sysmodules 中启用，然后重启。

## 编译

需要 devkitPro、devkitA64、libnx，以及 switch-curl、switch-zlib、switch-mbedtls。从仓库根目录运行：

~~~sh
./scripts/vendor-libultrahand.sh
make zip
~~~

成功后生成 ChargeCap.zip。压缩包包含常驻模块、悬浮菜单和中文翻译文件。

## 设置文件

设置保存在 sdmc:/config/chargecap/config.ini：

~~~ini
[ChargeCap]
enabled=0
limit=80
sleep_limit=1
~~~

- enabled：0 关闭限充，1 开启。
- limit：充电停止上限，范围 50–99。
- sleep_limit：1 开启睡眠时定时检查，0 仅在主机运行时限充。

## 致谢与许可

ChargeCap 基于原作者 [aoihera/ChargeCap](https://github.com/aoihera/ChargeCap)。限充实现参考 Switch-OC-Suite，IPC 架构参考 sys-clk，悬浮菜单使用 libultrahand。原项目中各部分许可见 [LICENSE](LICENSE)。
