<p align="center">
  <img src="logo.ico" alt="SubsPlayer Logo" width="82" height="82">&nbsp;&nbsp;<img src="sub_readme.png" alt="Subtitle'CAT" height="80">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows-blue.svg?style=for-the-badge&logo=windows" alt="Platform">
  <img src="https://img.shields.io/badge/Python-3.10%2B-brightgreen.svg?style=for-the-badge&logo=python" alt="Python">
  <img src="https://img.shields.io/badge/UI-PySide6-orange.svg?style=for-the-badge" alt="UI">
</p>

<p align="center">
  <a href="README.md">English</a> | 中文
</p>

一款支持视频字幕 **提取 → 对齐 → 翻译 → 编辑 → OCR硬字幕** 的Windows 桌面播放+字幕编辑软件，为制作双语字幕工作流而开发。

SubsPlayer 整合了视频字幕轨探测与批量提取、音频对齐、SRT 清洗、和多 Key 轮换AI翻译、断点续翻、硬字幕OCR翻译、双语 ASS 字幕生产的全部核心能力，并提供了一个基于 **mpv** 播放内核的流畅、直观、美观的桌面播放器模式。

---

## 🖥 界面预览

![SubsPlayer 主界面](Screenshot/2026-07-08_171211.jpg)

> 主界面（编辑器在右布局）：左侧为 mpv 播放器实时渲染双语 ASS 字幕，右侧为可视化编辑器 —— 播放时自动高亮并滚动到当前字幕行，底部可对当前行进行时间戳数位微调、文本编辑、OCR 识别与特效生成。

---

## ✨ 核心特性

*   **字幕批量提取**

*   **自动化时间轴对齐**

*   **AI 智能翻译引擎**
    *   使用 Gemini 官方 REST API 驱动，提供极速响应。
    *   **多 API Key 自动轮换与负载均衡**：支持配置多个 API Key，智能限制 RPM（每分钟请求数）和 RPD（每日请求数）规避限流。
    *   **智能容错机制**：二分重试（Bisect-retry）算法应对漏译或长句截断；当翻译中断时，支持**断点续翻**（自动加载缓存进度）。
    *   **字幕预清洗**：去除句首角色名（如 `JOHN: `）、过滤括号及花括号注释，使翻译文本更加纯净。
    *   **自动术语表**：翻译前 AI 自动提取本片的人名、地名、组织名等专有名词并生成固定译法。
    *   **双语 ASS 输出**：支持生成专业的双语 ASS 字幕样式，可自由选择保留单语/双语以及正译文的上下排序。
    
*   **PotPlayer 风格的高清可视化编辑器**
    *   **音视频同步渲染**：基于 `libmpv` 实现的播放器，完美按照 ASS 规则渲染实时字幕样式。
    *   **时间戳微调滚轮操作**：点击开始/结束时间的**任意数位**（时/分/秒/毫秒），滚动鼠标滚轮或使用上下方向键即可直接递增/递减，视频画面实时定位跟随。
    *   **播放实时高亮**：视频播放时，自动在右侧/左侧编辑器中高亮显示当前正在播放的字幕行并自动滚动居中。
    *   **多种界面布局**：支持“编辑器在右”、“编辑器在左”、“视频在上”以及“**双窗口独立分离**”等四种布局（支持双显示器，窗口靠近自动贴合）。
    
*   **智能视频 OCR与预置基本特效**
    *   在视频播放区域直接用鼠标框选画面，使用 Gemini 视觉多模态能力识别硬字幕原文，并自动翻译，填入当前的编辑行中。
    *   预置常用的基本ass特效
    
*   **国际化界面 (i18n)**
    *   全面支持 **中、英、日、韩、越、印尼、葡、西、法** 九国语言界面，立即生效。

---

## 📦 环境要求与依赖安装

> [!IMPORTANT]
> **二进制依赖说明（重要）：**
> 由于以下文件体积过大（合计约 300MB+），**项目仓库中默认不包含这些二进制文件**。您需要从各软件的原始官方项目下载，并放入本项目的对应文件夹中：
> *   `ffmpeg.exe` / `ffprobe.exe` / `ffplay.exe` (FFmpeg 组件，每个约 100MB) -> 放入 `ffmpeg/bin/` 目录
> *   `alass-cli.exe` (字幕对齐核心，约 3.5MB) -> 放入 `ffmpeg/bin/` 目录（或根目录）
> *   `libmpv-2.dll` (播放器渲染核心，约 120MB) -> 放入项目根目录

### 1. 软件环境
*   **Python 3.10+** (安装时请确保勾选了 `Add Python to PATH`)

### 2. 外部依赖下载与配置步骤

为保证全部功能可用，请点击链接前往下载并放置到指定目录：

1.  **FFmpeg 组件 (`ffmpeg.exe`, `ffprobe.exe`, `ffplay.exe`)** (必须是完整版，精简版可能无法提取音频):
    *   **下载地址**：前往 [Gyan.dev FFmpeg Builds](https://www.gyan.dev/ffmpeg/builds/) 下载 `ffmpeg-release-essentials.zip`。
    *   **放置路径**：解压后，将 `bin\` 目录内的 `ffmpeg.exe`、`ffprobe.exe` 和 `ffplay.exe` 复制到 SubsPlayer 目录下的 `ffmpeg\bin\` 文件夹中（或将其所在路径加入系统 PATH）。
2.  **alass-cli.exe** (时间轴对齐引擎):
    *   **下载地址**：前往 [alass GitHub Release](https://github.com/kaegi/alass) 下载对应 Windows 版本的 `alass-cli.exe`。
    *   **放置路径**：放入 SubsPlayer 目录下的 `ffmpeg\bin\` 文件夹中（或项目根目录，并在软件设置中指定其路径）。
3.  **libmpv-2.dll** (视频播放与 ASS 渲染核心):
    *   **下载地址**：前往 [SourceForge mpv-player-windows](https://sourceforge.net/projects/mpv-player-windows/files/libmpv/)，下载最新的 `mpv-dev-x86_64-*.7z` 并解压。
    *   **放置路径**：将其中的 `libmpv-2.dll` (旧版本通常名为 `mpv-2.dll`) 直接复制到 SubsPlayer 项目根目录下即可。

---

## 🚀 安装与使用


### 推荐使用流程
1.  **导入视频**：点击“打开视频”或直接将视频文件拖入软件窗口。SubsPlayer 会记住您的上次使用目录，并自动根据匹配算法加载同名或前缀匹配的字幕文件。
2.  **字幕提取**：在左侧面板的视频列表中选择视频，点击“提取字幕”即可看到该视频包含的音轨和字幕轨。批量配置应用后可一键将内封字幕提取为外部 SRT 格式。
3.  **时间对齐**：通常视频内置的字幕不需要对齐。若字幕与视频音轨错位，点击“对齐字幕”，选择以“当前视频音轨”或已确定时间轴准确的srt字幕文件作为参考源，一键对齐。
4.  **AI 智能翻译**：选中字幕文件后，点击“AI 翻译”，配置您本次翻译的目标语言、是否导出双语、清洗规则等。翻译任务将多线程执行，如果您的 API Key 较多，可以在设置中填入多个，软件会自动轮换。
5.  **可视化微调**：翻译完毕后，字幕将自动加载进编辑器并实时在视频上渲染。你可以一边播放视频，一边在编辑器中校对译文和微调时间轴，OCR翻译硬字幕等。

---

## 🎮 编辑器快捷键与操作指南

### 编辑器操作表
| 交互动作 | 说明 |
| :------- | :------- |
| **播放同步** | 视频播放时，右侧/左侧编辑器会自动滚动，使当前播放的时间点对应的字幕行居中高亮显示。 |
| **时间轴数位微调** | 点击开始/结束时间的**任意数位**（例如时、分、秒或毫秒），**向上滚动鼠标滚轮**或按 `↑` 键可加时间，**向下滚动**或按 `↓` 键减时间。视频播放器会自动跳转到该时间点方便校对。 |
| **双击字幕行** | 视频播放器会自动跳转到该行的开始时间并继续播放。 |
| **插入新行** | 在当前时间点前后 1 秒插入一条空白字幕，插入时视频会自动暂停。 |
| **删除字幕行** | 删除当前选中的字幕行，删除时视频会自动暂停。 |
| **文本换行** | 在单元格编辑框内直接回车输入换行，将以 `\N` 格式保存至 ASS 文件中（最多支持 3 行）。 |
| **Ctrl + S** | 立即保存。另外，编辑器包含自动保存机制（无输入 0.7 秒后自动保存并重新加载渲染字幕）。 |
| **布局切换** | 支持四种视窗布局循环切换：编辑在右 → 编辑在左 → 视频在上 → **双窗口分离模式**。 |


### 播放器快捷键
*(点击视频画面使其获取焦点后生效)*
*   `空格键` / `鼠标单击视频区域` ： 暂停 / 播放
*   `←` / `→` ： 向后 / 向前快进（默认 3 秒，按住 `Shift` 键为 1 秒）
*   `↑` / `↓` ： 增加 / 降低播放速度（步长 0.25x）
*   `R` ： 恢复为原始 1.0x 播放速度
*   `鼠标滚轮`（在视频窗口的任意位置）：向前 / 向后快速Seek
*   `音量滑条` / `亮度滑条` ：可随时拖动或使用鼠标轮微调音量与亮度
*   `音轨切换` & ` 内封字幕切换` ：在控制栏可以直接切换多音轨或开启/关闭内封字幕图层

---

## ⚙ 常见问题与排查 (FAQ)

1.  **启动时提示找不到 `libmpv-2.dll` 或加载失败？**
    *   请确保你已经将对应的 `libmpv-2.dll` 放置在了 SubsPlayer 根目录下。对于 64 位的 Windows 系统，必须使用 64 位的 DLL 文件。
    
2.  **提取字幕/对齐字幕时报错，提示 `ffmpeg` / `ffprobe` 执行失败？**
    *   检查 `config.json` 中配置的路径是否正确。如果是自动探测的，请尝试在主界面“设置 → 工具路径”中手动浏览并指定完整的 `ffmpeg.exe` 和 `ffprobe.exe` 路径。
    
3.  **Gemini 翻译速度慢或总是报错？**
    *   国内用户需要确保网络环境能顺利访问 `generativelanguage.googleapis.com`。你可以在设置中增加 API Key 以提高轮换并发上限，同时必须配置科学上网的代理。

---

## 🙏 鸣谢 (Acknowledgements)

SubsPlayer 站在这些优秀开源项目与服务的肩膀上，向所有作者与维护者致谢：

| 组件 | 作者 / 团队 | 主页 |
| :--- | :--------- | :--- |
| **FFmpeg** | FFmpeg team | https://ffmpeg.org |
| **FFmpeg Windows 构建** | Gyan Doshi | https://www.gyan.dev/ffmpeg/ |
| **mpv / libmpv** | mpv-player team | https://mpv.io · https://github.com/mpv-player/mpv |
| **alass** | kaegi (Alana Sanchez) | https://github.com/kaegi/alass |
| **Qt / PySide6 (Qt for Python)** | The Qt Company | https://doc.qt.io/qtforpython/ |
| **python-mpv** | jaseg | https://github.com/jaseg/python-mpv |
| **pysubs2** | Tomáš Karabela | https://github.com/tkarabela/pysubs2 |
| **Requests** | Kenneth Reitz & contributors | https://requests.readthedocs.io |
| **Google Gemini API** | Google | https://ai.google.dev |
| **Claude** (辅助开发) | Anthropic | https://claude.com |

---

# All rights reserved by SubsPlayer. 

    *  Commercial use is strictly prohibited.
    *  Please comply with local laws and regulations.
    *  Technology is innocent. Long live open source!
