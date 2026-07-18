# 网腾无限AI表情包生成器

## 项目介绍

<div align="center">

# [网腾无限AI表情包生成器]

**[一个支持主角设定与四种特色画风的 AI 表情包制作工具，基于 Vue 3 + Vite + Vanilla CSS 构建]**

[如果有的话，放一张极具代表性的项目截图/GIF动图]

[Vue3] · [TypeScript] · [Vite] · [Node.js] · [开源协议如 MIT]

[![GitHub stars](https://img.shields.io/github/stars/WT-Agent/ai-bqb?style=social)](https://github.com/WT-Agent/ai-bqb)
[![GitHub license](https://img.shields.io/github/license/WT-Agent/ai-bqb)](https://github.com/WT-Agent/ai-bqb/blob/main/LICENSE)

[🚀 在线演示](#-在线演示) · [📖 使用文档](#-快速启动) · [💬 参与贡献](#-参与贡献) · [☕ 支持一下](#-支持一下)

</div>

## 关于我们

团队成员大多来自C9等顶尖学府并在字节、腾讯、阿里的校友，研发 of AI 产品。

全职创业做开源项目，想让所有人感受AI的魅力。

本项目模拟了创意表情包与插画的自动化设计，用户可挑选表情包的主角（包含马斯克等九大佬以及传统的经典熊猫头），选择画面风格（经典线条画、写实搞怪、3D粘土卡通、复古像素），并输入自定义搞笑配文，大模型（通义万相）将自动为您生成高保真的创意社交表情包。

**我们不搞概念，不卖课，只写能跑起来的代码。**

欢迎Star、Fork、提Issue，一起让这个项目变得更好用。

核心特性：
- **常驻浮动分享按钮**：页面右上角常驻设计了半透明毛玻璃质感的“分享”按钮，用户可随时呼出朋友圈分享引导。
- **丰富的角色选择**：内置 9 位顶级科技与思想巨擘角色，以及长盛不衰的传统熊猫头，充分满足玩梗需求。
- **四大特色表情包画风**：
  - **经典线条画**：经典黑白素描简笔线条，完美契合传统熊猫头恶搞风。
  - **写实搞怪**：夸张、滑稽的写实大头贴质感，让表情包更具冲击力。
  - **3D 粘土**：色彩绚丽的 3D 卡通公仔质感，微距镜头感极强。
  - **复古像素**：8 位像素艺术风格，带有红白机时代的怀旧与戏谑。
- **自定义文字融入**：用户可随意设计并融入表情包文案，契合任意社交对话语境。
- **极简交互设计**：提供毛玻璃质感的深色玻璃拟态自适应 Web 界面，高度适配移动端 H5 体验。
- **一键部署托管**：纯静态前端结构，支持零成本部署于 Vercel、GitHub Pages 或 CDN/OSS/COS 静态托管服务。
- **开发代理服务**：本地开发支持使用个人 API 密钥发起代理请求，密钥由 Vite 服务器中转，无需担心前端泄露。

## 快速启动

### 1. 克隆项目
```bash
git clone https://github.com/WT-Agent/ai-bqb.git
cd ai-bqb
```

### 2. 安装依赖
项目强制使用 pnpm 作为包管理器：
```bash
pnpm install
```

### 3. 配置本地开发环境变量
复制并修改环境变量配置文件：
```bash
cp .env.example .env
```
根据微应用的功能类型，在 `.env` 中配置您的开发者密钥：
- `DEEPSEEK_API_KEY`: 您的 DeepSeek 开发者 API 密钥（用于文本生成任务）
- `DASHSCOPE_API_KEY`: 您的通义千问/通义万相开发者 API 密钥（用于多模态与生图任务）

### 4. 启动本地开发服务
```bash
pnpm dev
```
启动成功后在浏览器访问控制台输出的地址即可。

### 5. 生产构建打包
```bash
pnpm build
```
打包后生成的 `dist` 目录即为纯静态网页资源，可直接上传部署。

### 6. 部署至 GitHub Pages

由于本项目是纯静态前端应用，您可以将其零成本部署在 GitHub Pages 上：

1. **配置基础路径 (Base Path)**：
   在打包部署到非根域名（例如 `https://<your-username>.github.io/ai-bqb/`）时，需确保 `vite.config.ts` 中的 `base` 路径配置正确。在 `vite.config.ts` 中可将 `base` 设定为 `process.env.NODE_ENV === 'production' ? '/ai-bqb/' : '/'` 或 `./`。
2. **构建静态文件**：
   运行以下命令生成打包文件：
   ```bash
   pnpm build
   ```
   打包产物将输出在 `dist` 目录下。
3. **推送到分支部署**：
   将 `dist` 目录下的内容推送到您仓库的 `gh-pages` 分支，或在仓库的 **Settings -> Pages** 中将源分支设置为 `gh-pages`。也可以使用第三方的部署工具（例如 `gh-pages` npm 包）：
   ```bash
   # 安装部署工具
   pnpm add -D gh-pages
   # 执行部署
   npx gh-pages -d dist
   ```
4. **使用 GitHub Actions 自动部署**（推荐）：
   在项目根目录创建 `.github/workflows/deploy.yml` 工作流文件，配置在每次向 `main` 分支推送代码时自动构建并部署至 GitHub Pages 分支。

## 联系方式

- GitHub Issues: [提交反馈](https://github.com/WT-Agent/ai-bqb/issues)
- 邮箱: your_email@example.com

## 打赏支持

如果本项目对您有帮助，欢迎请作者喝杯咖啡。您的支持是持续维护与更新的动力。

<div align="center">

**微信支付** | **支付宝**
:---:|:---:
<img src="./asset/tenpay.png" width="200" alt="微信支付"> | <img src="./asset/alipay.png" width="200" alt="支付宝">

</div>

- Buy Me a Coffee: https://buymeacoffee.com/your_profile

## 版权与许可

本项目基于 MIT License 开源协议。

Copyright (c) 2026. All rights reserved.
