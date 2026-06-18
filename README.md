# pyqzan — 微信朋友圈模拟工具

基于 [uni-app](https://uniapp.dcloud.io/) 框架的微信朋友圈模拟工具，支持多平台编译运行。

## 技术栈

- **框架**: uni-app (Vue 2.6.x)
- **Node 版本**: v18.12.1
- **包管理器**: Yarn v1.22.22
- **HTTP 请求**: flyio
- **截图**: html2canvas
- **样式**: Sass
- **后端服务**: Express (weinode/)

## 功能模块

| 页面 | 路径 | 说明 |
|------|------|------|
| 首页 | `pages/index/index` | 朋友圈列表主页 |
| 发表 | `pages/edit/edit` | 发布/编辑朋友圈内容 |
| 详情 | `pages/article/article` | 朋友圈文章详情页 |
| 主页面 | `pages/main/main` | 主页面模式 |
| 自动文章 | `pages/autoArticle/autoArticle` | 自动生成文章内容 |
| 自动编辑 | `pages/autoEdit/autoEdit` | 自动编辑模式 |
| 自动主页 | `pages/autoMain/autoMain` | 自动主页模式 |

### 主要组件

- `navbar` — 导航栏（含电池、WiFi 等状态图标）
- `comment-module` / `comment-item` — 评论模块
- `emoticon-popup` — 表情选择器
- `edit-item` — 编辑面板
- `html2canvas` — 朋友圈截图导出
- `rule-popup` — 规则说明弹窗
- `upload` — 图片上传
- `userinfo-popup` — 用户信息弹窗
- `weiarticle-popup` — 微信文章弹窗
- `renderimg-popup` — 图片渲染弹窗

## 项目结构

```
pyqzan/
├── src/
│   ├── pages/          # 页面文件
│   ├── components/     # 公共组件
│   ├── config/         # 项目配置
│   ├── request/        # API 接口封装
│   ├── utils/          # 工具函数
│   └── static/         # 静态资源
├── weinode/            # Express 后端服务
│                       #   - 获取微信文章详情（封面图、标题）
│                       #   - 爬取头像
├── public/             # 公共静态资源
└── dist/               # 构建输出
```

## Project setup

```
yarn install
```

### 开发调试（H5）

```
yarn serve
```

### 生产构建

```
yarn build        # 等同于 yarn build:h5
yarn build:h5     # H5 平台
```

### 头像来源

头像来自 [我要个性网](https://www.woyaogexing.com/) 手动爬取。如在此工具中发现你正在使用的头像纯属巧合。

## 开源特别声明

- 本仓库发布的 `weipyq` 项目中涉及的任何内容，仅用于测试和学习研究，禁止用于商业用途，不能保证其合法性，准确性，完整性和有效性，请根据情况自行判断。
- 本项目内所有资源文件，禁止任何公众号、自媒体进行任何形式的转载、发布。
- `weipyq` 对任何商业问题概不负责，包括但不限于由任何人使用本项目导致的任何损失或损害。
- 请勿将 `weipyq` 项目的任何内容用于商业或非法目的，否则后果自负。
- 如果任何单位或个人认为该项目的脚本可能涉嫌侵犯其权利，则应及时通知并提供身份证明，所有权证明，我们将在收到认证文件后删除相关内容。
- 以任何方式查看此项目的人或直接或间接使用 `weipyq` 项目的使用者都应仔细阅读此声明。`zxb1655` 保留随时更改或补充此免责声明的权利。一旦使用并复制了任何相关代码或 `weipyq` 项目，则视为您已接受此免责声明。

> **您使用或者复制了本仓库且本人制作的任何代码或项目，则视为"已接受"此声明，请仔细阅读**
