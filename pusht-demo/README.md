# 🎮 PushT 推方块 · 在线体验 Demo

> 一个**零依赖、浏览器直接玩**的 2D 推方块演示，用来直观理解 LeRobot 背后的「**模仿学习 / 行为克隆（Behavior Cloning）**」。

## 🚀 立即体验

**👉 在线演示：<https://rio888414.github.io/lerobot-test/pusht-demo/>**

（无需安装、无需 Python、电脑 / 手机浏览器都能打开）

本地打开：直接双击本目录下的 `index.html` 用浏览器打开即可。

## 这是什么

它是 [Hugging Face LeRobot](https://github.com/huggingface/lerobot) 里 `pusht` 环境（`gym_pusht/PushT-v0`，Diffusion Policy 论文的经典任务）的浏览器简化复刻：

- 拖动白色**圆盘**，把灰色 **T 块**推到绿色虚线**目标**上（位置 + 朝向都对齐才算成功）。
- 三种模式对应 LeRobot 真实链路的三个环节：

| 模式 | 含义 | 对应 LeRobot 命令 |
|---|---|---|
| 🖱 手动玩 | 人类遥操作采集**演示数据** | `lerobot-record --env.type=pusht` |
| ✨ 自动演示 | 训练后策略的**部署效果**（此处为手写规则，非真模型） | `lerobot-eval --policy.path=...` |
| 🎲 随机动作 | **未训练**的随机策略（baseline 下界） | 随机初始化权重直接 rollout |

## 核心原理：模仿学习（行为克隆）

```
人类演示 D = {(观测 o_t, 动作 a_t)}  →  训练策略 π(a|o)  →  运行时 o_t 输入，输出 a_t 执行
```

- **观测**：圆盘坐标 `agent_pos`（`STATE`，shape `(2,)`）+ 画面 `pixels`（`VISUAL`）——对应 LeRobot 的 `obs_type="pixels_agent_pos"`。
- **动作**：圆盘每步位移 `(dx, dy)`（`ACTION`，shape `(2,)`）。
- 策略要学的就是「看到状态 → 该往哪推」这个映射。

## 与 LeRobot 的关系

| 本 Demo | LeRobot 仓库 |
|---|---|
| 手写「圆 vs 旋转矩形」碰撞 | `gym_pusht` + pymunk 2D 物理引擎 |
| 手动 / 随机 / 手写规则 | Diffusion Policy / ACT / VQ-BeT / Gaussian 等真模型 |
| Canvas 画面 + 坐标 | 渲染图像 + 状态，可训 CNN |
| 距离 + 角度阈值判定 | T 块覆盖目标的重叠率（IoU） |

真实跑通该任务：`lerobot-record → lerobot-train → lerobot-eval`（详见主仓库文档）。
