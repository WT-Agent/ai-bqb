<div align="center">

# 网腾无限 AI - 网腾无限AI表情包生成器

**[基于 Vue 3 + Vite + Vanilla CSS 构建的 网腾无限AI表情包生成器 智能实战微应用，具备深色玻璃拟态自适应交互与微信端 H5 体验]**

[Vue 3] · [TypeScript] · [Vite] · [Vanilla CSS] · [开源协议 MIT]

[![GitHub stars](https://img.shields.io/github/stars/WT-Agent/ai-bqb?style=social)](https://github.com/WT-Agent/ai-bqb)
[![GitHub license](https://img.shields.io/github/license/WT-Agent/ai-bqb)](https://github.com/WT-Agent/ai-bqb/blob/main/LICENSE)
[![Online Demo](https://img.shields.io/badge/Online_Demo-bqb.wuxian.xyz-indigo?style=flat-square)](https://bqb.wuxian.xyz)

[在线演示](https://bqb.wuxian.xyz) · [快速启动](#快速启动) · [核心特性](#核心特性) · [脚手架集成](#脚手架集成说明) · [支持一下](#联系我们与打赏支持)

</div>

---

## 团队与产品简介

团队成员均来自 C9 等顶尖学府，由字节、腾讯、阿里的资深工程师组成，全职创业研发开源 AI 微应用矩阵产品，旨在让所有人都能零门槛感受 AIGC 的生产力魅力。

**网腾无限AI表情包生成器** 专注于“**你是一个创意表情包与搞笑插画设计大师。用户在前端输入了表情包配文，并选择了人物角色（如马斯克、比尔盖茨、扎克伯格、贝索斯、乔布斯、柏拉图、爱因斯坦、特斯拉、秦始皇或传统熊猫头）与画面风格（经典线条画、写实搞怪、3D粘土卡通、复古8位像素）。你需要为大模型生成一张高保真、趣味性极强的表情包图片。要求生成的图像必须突出选定的人物角色，且极富表情包恶搞神韵，将用户输入的配文合理融入图像主题氛围中。**”。我们剔除了冗余概念，不搞虚假宣传，只提供极致优雅、即调即用的高完成度微应用前端与边缘网关接口。

**我们不搞概念，不卖课，只写能跑起来的代码。**

欢迎 Star、Fork、提 Issue，一起让这个开源 AI 工具生态变得更好用。

---

## 核心特性

- **极简自适应交互**：采用极具现代感与科幻氛围的深色玻璃拟态 (Glassmorphic Dark UI) 设计，全量兼容移动端微信 H5 与 PC 响应式体验。
- **纯静态零成本部署**：架构保持 100% 静态化，无额外 Server 依赖，支持一键托管至 Cloudflare Pages、Vercel、GitHub Pages 或 CDN/OSS 静态存储。
- **安全代理与双模型网关**：内置安全开发代理中转层，支持无缝接入 DeepSeek-V3/R1 文本大模型及通义千问/通义万相多模态生图 API。
- **多维度评分与案例展示**：集成 AI 共识多指标看板、动态用户活跃跑马灯 ticker、精彩场景 Preset 案例以及生成卡片截图分享功能。
- **支付打赏与通道联系**：内置微信支付与支付宝赞赏二维码组件，支持灵活的裂变锁屏与额度留存管理。

---

## 核心功能与使用场景

1. **智能 Prompt 场景引擎**：针对 **你是一个创意表情包与搞笑插画设计大师。用户在前端输入了表情包配文，并选择了人物角色（如马斯克、比尔盖茨、扎克伯格、贝索斯、乔布斯、柏拉图、爱因斯坦、特斯拉、秦始皇或传统熊猫头）与画面风格（经典线条画、写实搞怪、3D粘土卡通、复古8位像素）。你需要为大模型生成一张高保真、趣味性极强的表情包图片。要求生成的图像必须突出选定的人物角色，且极富表情包恶搞神韵，将用户输入的配文合理融入图像主题氛围中。** 领域进行了深度提示词工程优化与共识打分约束。
2. **多风格预设切换**：提供专业干练、高情商说辞、幽默风趣、严谨学术（或写真照片、卡通动漫等多模态）风格的一键切换。
3. **一键复制与卡片分享**：支持生成内容的快速复制，以及渲染结果的截图分享导出。
4. **统一 SSO 额度管理**：接入 wuxian.xyz 共享登录凭证，支持每日免费额度计数与登录解锁。

---

## 快速启动

### 1. 克隆项目
```bash
git clone https://github.com/WT-Agent/ai-bqb.git
cd ai-bqb
```

### 2. 安装依赖
项目推荐使用 `pnpm` 作为包管理器：
```bash
pnpm install
```

### 3. 配置环境变量
复制并配置本地开发环境变量：
```bash
cp .env.example .env
```
在 `.env` 中填入您的 API Key：
- `DEEPSEEK_API_KEY`: 您的 DeepSeek 开发者 API Key（用于文本类微应用）
- `DASHSCOPE_API_KEY`: 阿里 DashScope API Key（用于多模态生图微应用）

### 4. 启动本地开发
```bash
pnpm dev
```
启动后在浏览器打开控制台提示的本地开发地址即可进行调试。

---

## 脚手架集成说明

本微应用由私有总控仓库 `ai.wuxian.xyz` 中的运维脚手架统一管理，支持通过 CLI 进行批量更新与配置维护：

```bash
# 自动化发版与发布
node bin/cli.js publish ai-bqb

# 查看当前微应用配置
node bin/cli.js get ai-bqb

# 动态热更新提示词或模型映射
node bin/cli.js set ai-bqb prompt "你是一个创意表情包与搞笑插画设计大师。用户在前端输入了表情包配文，并选择了人物角色（如马斯克、比尔盖茨、扎克伯格、贝索斯、乔布斯、柏拉图、爱因斯坦、特斯拉、秦始皇或传统熊猫头）与画面风格（经典线条画、写实搞怪、3D粘土卡通、复古8位像素）。你需要为大模型生成一张高保真、趣味性极强的表情包图片。要求生成的图像必须突出选定的人物角色，且极富表情包恶搞神韵，将用户输入的配文合理融入图像主题氛围中。"
node bin/cli.js set ai-bqb model deepseek-chat
```

---

## 联系我们与打赏支持

如果本项目对您的工作或学习有所帮助，欢迎扫码请团队喝杯咖啡，支持我们的开源维护！

<div align="center">

**微信支付** | **支付宝**
:---:|:---:
<img src="./asset/tenpay.png" width="180" alt="微信支付"> | <img src="./asset/alipay.png" width="180" alt="支付宝">

</div>

---

- **官方网站**: [https://bqb.wuxian.xyz](https://bqb.wuxian.xyz)
- **GitHub Issues**: [提交反馈](https://github.com/WT-Agent/ai-bqb/issues)
- **反馈邮箱**: us@wuxian.xyz
- **官方主页**: [ai.wuxian.xyz](https://ai.wuxian.xyz)

---

## 版权与许可

本项目基于 **MIT License** 开源协议。

Copyright (c) 2026 [WangTeng.Tech](https://ai.wuxian.xyz). All rights reserved.
