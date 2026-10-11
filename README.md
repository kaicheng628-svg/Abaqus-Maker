# Abaqus Maker

用于 **Codex 的 Abaqus 有限元 Skill**：从参数整理、建模和求解，到结果验证、软件原生曲线和固定视角动画，形成可复用的工作流程。

**不绑定具体模型、尺寸或材料。** 根据当前项目选择分析方法；稳态释放、反向切换和抗扰动搜索按需启用。

[技能入口](SKILL.md) · [稳态与抗扰动验证](references/stability.md) · [曲线与视频输出](references/native-output.md) · [任务配置示例](examples/task.example.json)

## 功能

- 核对几何、材料、边界条件和文献依据，区分原文参数、换算值与假设。
- 指导参数化建模、连接与接触、网格、分析步和输出设置。
- 检查作业是否真正完成，以及力平衡、能量、接触与异常变形。
- 按需开展参数扫描、网格对照和数值控制敏感性检查。
- 按需执行加载—释放—保持、反向切换与指定方向的脱稳力搜索。
- 从真实 ODB 提取数据，由 **Abaqus CAE/Viewer 直接生成曲线**，保留图像和数据报告。
- 按需录制软件中的完整结果回放，保持固定视角，明确色标、单位与阶段。

## 运行环境

- **Codex**：用于读取 Skill、组织任务、生成项目脚本与执行工作流。
- **Abaqus 及有效许可证**：实际建模与求解在安装了 Abaqus 的电脑上执行。
- **Abaqus 自带 Python**：用于调用 Abaqus API；具体版本与参数按当前安装核对。
- **可用的 computer-use 能力**：仅在需要操作软件界面或录屏时使用。
- **视频编码工具**：按交付需要配置，不是所有分析的必需项。

工作流来自 Windows 上的实际 Abaqus 建模、求解和后处理过程。其他版本、系统与新模型仍需检查软件接口和分析设置。

GitHub 用来保存和分享 Skill，**不会在 GitHub 上自动运行有限元计算**。本仓库是指导型技能；项目建模脚本由 Codex 根据当前模型编写，不包含适用于任意几何的一键求解器，也不需要安装额外的 Python 依赖包来读取技能。

## 获取与安装

### 在 Codex 中安装

把下面这句话发送给 Codex：

> 请使用 skill-installer，从 https://github.com/kaicheng628-svg/Abaqus-Maker 安装技能。技能入口位于仓库根目录，安装名称为 abaqus-maker。

已有本机安装无需重复安装；更新时先保留本地修改。

### 手动安装

点击 **Code → Download ZIP**，解压后把包含 `SKILL.md`、`agents`、`references` 和 `examples` 的文件夹重命名为 `abaqus-maker`，放入 Codex 的技能目录：

| 环境 | 默认位置 |
|---|---|
| Windows | `%USERPROFILE%\.codex\skills\abaqus-maker\` |
| macOS / Linux | `~/.codex/skills/abaqus-maker/` |
| 设置了 `CODEX_HOME` | `<CODEX_HOME>/skills/abaqus-maker/` |

也可在 PowerShell 中克隆到默认位置：

```powershell
git clone https://github.com/kaicheng628-svg/Abaqus-Maker.git "$env:USERPROFILE/.codex/skills/abaqus-maker"
```

如果配置了 `CODEX_HOME`，请将目标路径换成其下的 `skills/abaqus-maker`。目标目录应不存在；不要覆盖已有技能。确认 `SKILL.md` 直接位于技能目录内，没有多嵌套一层仓库文件夹。安装后在下一轮对话调用。

## 使用方法

### 1. 提供当前项目

提供模型文件、图纸、论文或参数，以及要回答的问题。不要求一次提供所有信息；技能会先核对已有内容，再指出影响分析的缺失项。

可参考 [任务配置示例](examples/task.example.json) 整理需求。该 JSON 是任务记录模板，**不是直接提交给 Abaqus 的输入文件**；其中的 `null` 表示尚未确定，不能直接当作模型参数。

### 2. 调用 Skill

```text
$abaqus-maker 根据当前项目准备有限元建模方案，先明确材料、边界条件、网格和输出，暂不运行求解。
```

```text
$abaqus-maker 按已确定的方案建立模型并运行，保存可编辑模型、输入文件、求解日志和结果。
```

```text
$abaqus-maker 检查这次计算，并在 Abaqus 中直接导出力—位移和弹性应变能曲线。
```

```text
$abaqus-maker 验证卸载后能否保持目标构型，再搜索指定方向的脱稳力区间；每次从同一个已释放状态开始。
```

```text
$abaqus-maker 录制 Abaqus 软件里的加载、释放和保持过程，始终保持固定视角，显示完整模型与色标。
```

### 3. 检查与交付

一般流程为：**核对输入 → 确定分析方案 → 建模与检查 → 求解 → 验证 → 导出与解释**。

按任务需要交付：

| 内容 | 典型文件或结果 |
|---|---|
| 可继续编辑的工程 | `.cae`、参数、建模脚本 |
| 可追溯的求解 | `.inp`、`.odb`、关键日志，必要的重启动文件 |
| 软件原生曲线 | 曲线图、`.rpt`、XYData 或重建图表脚本 |
| 对照结果 | 各阶段峰值、整体峰值、位移、能量及判定依据 |
| 动画与录像 | 来自真实 ODB 的软件回放或按需录制的视频 |
| 结论 | 通俗说明已验证结果、假设、限制和未完成事项 |

仅要求方案时不启动新计算；仅要求重录时，若现有结果已满足要求，不为录制而重复求解。

## 验证与边界

技能入口和配置已通过格式检查；仓库根目录安装方式已完成实际安装验证。这里的检查确认技能能被安装和读取，**不代表未来任意模型都已经验证**。

使用时保留以下判断：

- 静力收敛、负刚度段或约束下停留，不能单独证明释放后的稳态。
- `ALLSE` 是弹性应变能，不自动等于总势能或能垒。
- 静力步时间和动画播放时间，不自动代表实际加载秒数。
- 求解失败不能直接算作物理脱稳；脱稳力按已验证的保持/脱稳区间报告。
- 材料名称和临时弹性参数不能代替真实材料标定或强度验证。
- 抗扰动结果限定于所验证的方向、夹具和加载协议。

详细判据见 [稳态与抗扰动验证](references/stability.md)，图表和视频要求见 [原生输出](references/native-output.md)。

## 文件结构

```text
SKILL.md                      Codex 技能入口与主要工作流
agents/openai.yaml            技能名称与默认调用提示
references/stability.md       稳态、切换、抗扰动与能量判据
references/native-output.md   原生曲线、动画、录屏与追溯要求
examples/task.example.json    通用任务记录模板
README.md                     安装与使用说明
.gitignore                    排除本地缓存和仿真产物
```

不随仓库发布任何具体项目的模型、材料参数、仿真结果或视频。
