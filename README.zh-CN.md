# AlgorithmArt

纯 WebGL2 程序化着色艺术 —— 向 **Xor(@XorDev)** 等图形程序员的紧凑 GLSL 作品致敬。他们短小精悍的程序化着色草图，是这里每一项技术重建的蓝本。

每一个演示都是**单文件、零依赖**的 HTML：无构建步骤、无第三方库、无贴图、无模型。画面中的一切都由 GPU 逐像素、纯数学实时计算 —— 视线发射、波浪动力、体积光线步进、散射、调色与色调映射。

English version: [README.md](README.md)

## 快速开始

用支持 WebGL2 的浏览器（最新版 Chrome / Edge / Firefox）直接打开任意 `.html` 文件即可，无需任何构建或服务器。

- 移动鼠标可环绕视角（大屏幕生效）。
- 底部按钮可切换 自动 / 高 / 中 / 低 渲染分辨率。
- HUD 实时显示 FPS 与当前渲染比例。

## 两大系列

### 🌙 MoonSea — 海上生明月

夜景海面场景，从零开始分五步递进构建：

| 步骤 | 文件 | 核心内容 |
|------|------|----------|
| 1 | `MoonSea/moonsea_step1_camerafrustum.html` | 视线发射：罗德里格斯轴角旋转相机 + 呼吸式焦距；视锥以透视网格可视化呈现 |
| 2 | `MoonSea/moonsea_step2_starsmoon.html` | 程序星野：250 格哈希抖动星场、逐星色相/饱和度、大气消光；smoothstep 月盘 + 月冕 + 光晕 |
| 3 | `MoonSea/moonsea_step3_gerstnerwave.html` | 波浪动力：`exp(sin(x)-1)` 指数格斯特纳波，36 层谐波迭代 + 位置拖拽，灰度等高线可视化高度场拓扑 |
| 4 | `MoonSea/moonsea_step4_raymarchnormal.html` | 水面求交：在双平面（y = 0 / y = −水深）包围盒内光线步进，36 次差分求法线，按距离混合回平面法线 |
| 5 | `MoonSea/moonsea_step5_fresnelscatter.html` | 终局着色：Schlick 菲涅尔、月光高光路径、比尔-朗伯式深度散射、ACES 色调映射（`moonsea_step5_fresnelscatter_backup_v1.html` 为早期备份变体） |

### 🌅 Sunset — 落日云海

体积云晚霞场景，同样分五步递进：

| 步骤 | 文件 | 核心内容 |
|------|------|----------|
| 1 | `Sunset/sunset_step1_slab.html` | 视锥发射与大气对称平板（y = ±0.3），自适应步长光线步进 —— 稀疏区大步跳跃、密实区细致采样 |
| 2 | `Sunset/sunset_step2_turbulence.html` | 八阶正交置换湍流：`p += amp · sin(p·f − vel·t).yzx / f`，每阶倍频 ×1.8，YZX 轴向置换消除平移共线性 |
| 3 | `Sunset/sunset_step3_inscattering.html` | 指数级光子内散射（比尔-朗伯近似）：density = exp(s·10)/d，云体开始有了厚度 |
| 4 | `Sunset/sunset_step4_palette.html` | 空间相位差余弦晚霞调色盘：RGB 三通道相位相差 [0, 1, 2] 弧度，色彩全部由波动相位生成 |
| 5 | `Sunset/sunset_step5_tonemap.html` | 最终效果：100 次体积光线步进 × 八阶湍流 × 余弦调色 × tanh 色调映射 |

## 通用工程细节

- **WebGL2 + GLSL ES 3.00**：全屏三角形对 + 原生 WebGL API，不引入任何第三方库。
- **自适应分辨率**：自动模式持续测量帧耗时并动态缩放内部渲染目标，以稳定 ~60 FPS；也可手动锁定高/中/低。
- **相机**：桌面端鼠标环绕视角；小屏幕自动回退为固定镜头并缓慢摇摆。
- **递进式教学**：每个步骤只在前一步基础上增加一项技术，两个系列都可按顺序阅读着色器源码 —— 从第一条光线走到最终画面。

## 目录结构

```
AlgorithmArt/
├── MoonSea/    # 海上生明月系列（5 个步骤 + 1 个备份）
└── Sunset/     # 落日云海系列（5 个步骤）
```

## 致谢

向 **Xor(@XorDev)** 与整个 Demoscene / Shadertoy 社区致敬 —— 他们最擅长用尽可能少的 GLSL 行数，表达尽可能丰富的画面。
