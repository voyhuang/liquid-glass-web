# Liquid Glass Web

[English](README.md) · [在线案例](https://voyhuang.github.io/liquid-glass-web/) · [安装](#安装)

面向 Codex、符合 [Agent Skills 规范](https://agentskills.io/specification)的独立
Liquid Glass 网页技能。**v0.1.2** 提供双路径边缘折射、柔和倒影、局部色散，
玻璃中央不使用整体高斯模糊。不发布 npm 包或 Plugin，不依赖 CDN，没有构建步骤。
中英文说明保持对应，技术表述以英文为准。

## 效果预览

[打开四个案例](https://voyhuang.github.io/liquid-glass-web/)。
Pages 直接展示随 skill 一起安装的单文件 HTML 案例。

## v0.1.2 更新

- Chromium 使用原生背景光学路径，Safari 使用声明区域的视觉副本。
- 底层正文使用轻量材质，导航采用一个可拖动的共享玻璃选中层。
- 会遮盖文字或图形的大浮层使用 2× 光学贴图，紧凑表面保留 1×。
- 保留清晰中心与统一边缘光学；中性饱和度／亮度，边框 0.5px。
- 深色边框／顶部高光不透明度为 0.10／0.20，上层玻璃透明度固定 90%。
- 外观按钮展开明暗切换和 5%–95% 底色透明度滑块（默认 40%），不显示数值或说明句。

## 安装

以下安装命令固定使用 v0.1.2 预览版。

让 Codex 执行：

```text
Use $skill-installer to install https://github.com/voyhuang/liquid-glass-web/tree/v0.1.2/skills/liquid-glass-web
```

把 `v0.1.2` 换成 `main` 可跟随开发分支。技能未立即出现时重启 Codex，
使用 `$liquid-glass-web` 调用。

手动安装：

```sh
git clone --depth 1 --branch v0.1.2 https://github.com/voyhuang/liquid-glass-web.git
mkdir -p ~/.codex/skills
cp -R liquid-glass-web/skills/liquid-glass-web ~/.codex/skills/liquid-glass-web
```

复制命令假定目标目录不存在。替换已有安装前先备份。安装的是 skill 子目录，
不是仓库根目录。

## 快速使用

1. 将 [glass.css](skills/liquid-glass-web/assets/glass.css) 完整内嵌到样式块。
2. 浮动表面使用 `.lg`，底层正文使用 `.lg--lite`，通过 `data-lg-source` 声明 Safari 背景源。
3. 将 [refraction-snippet.html](skills/liquid-glass-web/assets/refraction-snippet.html)
   完整复制一次到 body 末尾、业务脚本之前。
4. 页面专属 CSS 放在 canonical 样式之后。

```html
<article class="lg lg--lite">
  <h1>A clear surface</h1>
  <button id="themeToggle" class="lg-chip" type="button"
          aria-label="Appearance">◐</button>
</article>
```

可选的 `themeToggle` 按钮会创建外观弹出面板，点击时不立即切换主题。
滑块只改变底色 alpha；用户调节值在主题切换后保留，刷新后不保留。
不添加按钮就不生成菜单。

## 类名与 token

| 接口 | 用途 |
|---|---|
| `.lg` | 底色、尺寸适配边缘光学、边缘光与光标高光 |
| `.lg--lite`、`.lg--overlay` | 轻量正文／固定 90% 透明度的选中层 |
| `data-lg-source` | Safari 声明背景源的选择器 |
| `data-lg-map-scale="2"` | 用于覆盖具体内容的大浮层的 2× 贴图 |
| `data-lg-nav` | 可选共享拖动导航，详见组件参考 |
| `--lg-border-width` | 0.5px 边框及同心内圆角缩进 |
| `.lg--thick`、`.lg--thin` | 层级与阴影，不是模糊强度 |
| `.lg-materialize` | 入场；通过 `--enter-delay` 错峰 |
| `.lg-chip`、`.lg-cta` | 非玻璃控件及强调色变体 |
| `.lg-backdrop` | 可选动画背景 |
| `--lg-radius` | 圆形圆角；默认 26px，胶囊用 999px |
| `--lg-tint`、`--lg-tint-a` | 底色通道与不透明度 |
| `--lg-sat`、`--lg-bright` | 饱和度与亮度 |
| `--lg-shadow`、`--lg-shadow-sm` | 表面阴影 |
| `--sheen-a`、`--spot-max` | 光泽与光标高光 |
| `--bg`、`--ink`、`--ink-dim`、`--accent` | 主题颜色 |
| `--lg-blur` | 兼容保留的旧 token，默认 0px，不影响材质 |

在资产之后覆盖 token，不重写材质规则。运行时 filter ID 与 `--lg-filter`
属于内部状态。圆形圆角与光学贴图配套；不要单独改成 squircle。

## 案例

| 案例 | 内容 | 在线 |
|---|---|---|
| [Quick Start](skills/liquid-glass-web/references/example-quick-start.html) | 最小导航栏与卡片 | [打开](https://voyhuang.github.io/liquid-glass-web/skills/liquid-glass-web/references/example-quick-start.html) |
| [Components](skills/liquid-glass-web/references/example-components.html) | 导航、卡片、chip、独立 CTA、dialog | [打开](https://voyhuang.github.io/liquid-glass-web/skills/liquid-glass-web/references/example-components.html) |
| [Music Player](skills/liquid-glass-web/references/example-music-player.html) | 播放演示与进度控件 | [打开](https://voyhuang.github.io/liquid-glass-web/skills/liquid-glass-web/references/example-music-player.html) |
| [Résumé / Portfolio](skills/liquid-glass-web/references/example-resume.html) | 吸顶导航、响应式布局、打印 | [打开](https://voyhuang.github.io/liquid-glass-web/skills/liquid-glass-web/references/example-resume.html) |

四个案例均逐字内嵌 canonical CSS 和 snippet，可离线打开。
调校期间使用的光学小样不作为第五个公开案例发布。

## 浏览器降级

渲染策略（2026-10-08）：已验证的桌面 Chromium 使用原生 SVG 背景滤镜；
Safari 对声明的 HTML 源建立限定区域、不可交互的视觉副本并使用普通 SVG 滤镜。
其他环境和未声明背景源的 Safari 表面保留清晰半透明材质。外观菜单使用原生 popover。
背景源约束、刷新事件和核验范围集中见
[组件参考](skills/liquid-glass-web/references/components.md#browser-paths-and-source-scope)。

降级阶梯为：**边缘折射 → 清晰半透明表面 → 无障碍实色表面**。
降低透明度、提高对比度、强制色彩和打印样式覆盖材质；媒体查询支持因浏览器而异。

## 无障碍与性能

大面积正文使用轻量材质，导航使用单个共享选中层，控制完整光学面板数量。保留可见焦点、控件名称和繁忙背景上的
文字对比度。菜单支持 Escape、点击外部关闭，滑块保留原生键盘操作。

贴图随面板尺寸变化更新，不在滚动或光标移动时重建。每个面板有独立滤镜，
隐藏 dialog 在显示后初始化。每侧光学范围最多占最短边的 22%，避免相向边缘
挤占中央。减少动态效果时停止装饰动画，打印时表面实色化。
2× 贴图的像素数量为四倍，面板显示尺寸保持不变。Safari 副本在内容、尺寸或主题变化时刷新，
滚动与拖动时同步位置。

## 常见故障

- **没有折射：** 检查浏览器门控、filter ID、面板尺寸和祖先元素。
- **入场后效果消失：** 去掉持续保留的最终 `filter`，保留 canonical
  动画的 `backwards` 填充方式。
- **圆角有缝：** 保持同心圆形圆角。
- **中央浑浊：** 移除额外整体模糊，检查底色透明度。
- **小控件效果过重：** 确保所有光学半径共同缩放。
- **滚动卡顿：** 减少面板和不必要的合成层。

## 验证与贡献

```sh
python3 ~/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/liquid-glass-web
```

检查相关浏览器、键盘、响应式、无障碍与打印行为。参见
[CONTRIBUTING.md](CONTRIBUTING.md)。资产变更需同步四个案例和两份 README。
仓库没有构建流水线。

## 许可与声明

[MIT](LICENSE)。独立非官方实现，与 Apple 无关联，未获其认可。
不包含 Apple 代码或专有美术资产。光学曲线是视觉近似，不是苹果 shader，
也不是物理精确模拟。
