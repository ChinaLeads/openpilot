<div align="center" style="text-align: center;">

<h1>China 智驾系统</h1>

<p>
  <b>为长安欧尚 Z6 iDD 深度定制的驾驶辅助系统</b>
  <br>
  基于 openpilot（MIT License）构建，开箱即用，上车自动识别车型。
</p>

<h3>
  <a href="#-安装">安装地址</a> ·
  <a href="#-功能特性">功能特性</a> ·
  <a href="#-常见问题">常见问题</a> ·
  <a href="#-许可">许可</a>
</h3>

</div>

> **Chinapilot** 是专为中国市场车型打造的国内定制分支，可实现长安 Z6 iDD 等国产车辆的控车能力。项目针对国内复杂道路环境开展本地化适配与性能优化，同时持续跟进、同步 commaai 官方上游代码。
>
> **⚠️ 安全免责声明**
> 1. 本项目为开源 L2 级驾驶辅助软件，不具备自动驾驶能力，车辆控制权始终归驾驶员所有。
> 2. 使用期间驾驶员必须全程手握方向盘，保持注意力，随时准备接管车辆。
> 3. 使用者知悉并自行承担使用本项目带来的一切安全风险、事故责任及相关法律后果。
> 4. 本项目仅用于学术与技术研究目的，禁止在公共道路违规使用。

---

## 📲 安装

在 comma 设备的 **Settings → Software → Custom Software** 中输入：

```
installer.comma.ai/ChinaLeads/openpilot/China
```

- 必须包含分支后缀 `/China`，完整地址为 `installer.comma.ai/ChinaLeads/openpilot/China`
- 无需手动选择车型，开机自动锁定 `CHANGAN_Z6_IDD`
- 首次安装约需下载 1.8GB 源码，请保持网络稳定

## 🚗 适配车型

**长安欧尚 Z6 iDD**（插电混动）

| 项目 | 说明 |
|---|---|
| 车型指纹 | `CHANGAN_Z6_IDD`（`launch_env.sh` 固定，跳过识别流程） |
| 安全模型 | `changan`（`SAFETY_CHANGAN`，含 CRC 校验与滚动计数器） |
| 横向控制 | 角度控制（直接写入 EPS 转角指令） |
| 纵向控制 | openpilot 全速域纵向控制，支持 Stop-and-Go |
| 整备质量 | 1760 kg（+136 kg 标准载荷） |
| 轴距 / 转向比 | 2.795 m / 14.5 |
| 盲区监测 | 启用（数据来自原车 BSM） |
| 驾驶员监控 | 已启用 openpilot 自带 DMS 逻辑（建议保持注意力） |

## ✨ 功能特性

- **自动车型识别**：固定指纹 + 完整 changan 车型端口，上电即用
- **L2 级辅助驾驶**：车道居中 + 全速域 ACC + 自动跟停起步
- **panda 安全模型**：所有发送帧逐帧校验（CRC-8/J1850 + 4-bit 滚动计数），超差即阻断
- **China 界面**：开机向导 / 设置 / 提示 / 12 种语言翻译全面 China 化
- **自动同步上游**：CI 工作流自动合并官方 openpilot 更新，合并后自动校验 Z6 iDD 适配完整性，校验通过才发布

## ❓ 常见问题

**Q：安装地址输入后没反应？**
确认拼写区分大小写，且包含分支后缀：`installer.comma.ai/ChinaLeads/openpilot/China`；确认设备已联网。

**Q：怎么确认装的是 China 版？**
`设置 → 设备 → 关于`，版本信息显示 **China**；车型指纹应为 `CHANGAN_Z6_IDD`。

**Q：官方 openpilot 更新后会失效吗？**
不会。同步工作流在每次合并后校验车型适配（指纹、安全模型、平台注册等），校验失败则中止推送，不会把未经验证的版本发布给用户设备。
**Q：首次上路注意什么？**
横纵向参数为保守出厂值，请在空旷路段低速验证转向与加减速响应，  异常时立即接管。

## 📁 仓库结构

| 路径 | 内容 |
|---|---|
| `openpilot/` | 主系统（selfdrive / system / tools） |
| `opendbc_repo` | 车型适配子模块 → [ChinaLeads/opendbc](https://github.com/ChinaLeads/opendbc)（`opendbc/car/changan/` + `opendbc/safety/modes/changan.h`） |
| `panda` | CAN 通信固件（上游 commaai/panda，编译时自动引入 changan 安全模型） |

## 📜 许可

本项目基于 [openpilot](https://github.com/commaai/openpilot)（MIT License）构建，保留上游原始版权声明与许可文本。
本项目定制部分（长安欧尚 Z6 iDD 车型适配、China 品牌标识）版权归本仓库所有。

**免责声明**：驾驶辅助系统不能替代驾驶员，使用时请始终保持注意力集中、手握方向盘。本系统按"现状"提供，使用风险由用户自行承担。
