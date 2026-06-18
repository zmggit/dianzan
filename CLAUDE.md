# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 常用命令

```bash
yarn install          # 安装依赖
yarn serve            # 开发调试（H5 模式，默认端口 8080）
yarn build            # 生产构建（H5）
yarn build:h5         # 显式 H5 构建
```

`yarn serve` 等同于 `npm run dev:h5`，使用 `cross-env NODE_ENV=development UNI_PLATFORM=h5` 启动 vue-cli-service uni-serve。

## 项目架构

这是一个基于 **uni-app (Vue 2.6)** 的微信朋友圈模拟工具，支持 H5 和多个小程序平台编译。

### 入口与应用挂载

- `src/main.js` — Vue 实例化入口，挂载两个全局属性：
  - `Vue.prototype.$api` — API 请求模块（`src/request/api.js`）
  - `Vue.prototype.$util` — 工具函数（`src/utils/index.js`），包含 `randomName()`、`randomImg()`、`emoticonReplace()`
- `src/App.vue` — 引入全局样式 `static/iconfont.css` 和 `static/css/index.scss`
- `src/pages.json` — uni-app 页面路由配置，`navigationStyle: "custom"` 表示使用自定义导航栏

### 页面流

```
edit（编辑页）
 ├─ type=0,1 → article（文章详情页）
 └─ type=2  → main（带评论的朋友圈主页）
```

autoArticle / autoEdit / autoMain 是自动生成内容模式的平行页面流。

数据通过 uni-app 的 `uni.navigateTo` 以 JSON 字符串通过 URL query 参数 `data` 在页面间传递。核心数据结构 `pageData` 定义在 edit.vue 中，包含：
- `type`: 0=普通图片, 1=公众号链接, 2=带评论模式
- `navbar`: 是否显示顶部导航栏（含电量、WiFi 图标、时间）
- `article`: 用户名、头像、文字内容、图片列表、日期时间
- `linkInfo`: 公众号链接的封面图和描述
- `comment`: 点赞数(`goodUserAvatarList`)、评论列表(`commentUserList`)

### HTTP 请求层

- `src/request/http.js` — 基于 flyio（微信小程序兼容的 HTTP 库）封装，baseURL 来自环境配置
- `src/request/api.js` — API 类，目前有两个方法：`getArticle(parms)` 获取微信文章信息、`getEmoJson()` 获取表情 JSON
- `src/config/index.config.js` — 环境配置，开发环境 `baseUrl: "http://localhost:3000"`，生产环境 `baseUrl: "https://wei.jzzz66.cn"`

### weinode 后端

Express 服务器（端口 3000），提供三个 API：
- `GET /getWeiDate?url=xxx` — 抓取微信公众号文章的 og:title 和 og:image
- `POST /uploadWorkPicture` — 上传图片到 `/public/image/`
- `GET /deleteWorkPicture?imagePath=xxx` — 删除指定图片

启动时自动爬取头像（从 woyaogexing.com 爬取 40 页头像到 `/public/avatar/`）。

### 关键组件

- `html2canvas` — 朋友圈截图生成，通过 slot 包裹要渲染的内容，调用 `createImg()` 方法触发生成
- `navbar` — 自定义顶部导航栏，包含电池电量、WiFi 信号、时间显示
- `comment-module` — 评论区，支持点赞（自赞）、评论列表展示和长按选择（删除/回复/预览图片）
- `article-module` — 文章内容卡片，包含头像、用户名、正文、图片、时间戳、点赞/评论操作栏
- `upload` — 图片展示组件，`show` 属性控制只读展示模式，`@upLoader` 事件接收上传文件
- `edit-item` — 编辑页通用配置项，支持 slider、switch、picker 三种类型
- `weiarticle-popup` — 输入公众号链接获取文章信息
- `userinfo-popup` — 发布前设置用户信息（昵称、头像、日期）
- `rule-popup` — 规则说明弹窗
- `emoticon-popup` — 表情选择弹窗
- `renderimg-popup` — 截图预览弹窗

### 静态资源结构

- `/static/avatar/avatar-{1..500}.jpeg` — 500 个头像图片，`randomImg()` 从中随机选取
- `/static/iconfont.css` — 图标字体
- `/static/css/index.scss` — 全局样式
- `public/emoticon.json` — 表情数据

### 开发注意事项

- 桌面端（≥1024px）页面宽度固定 550px、居中显示、带阴影；移动端（≤768px）全宽显示
- `var(--green)` 和 `var(--grayLight)` 等 CSS 变量定义在全局样式中
- 截图前需将 `isFixed` 设为 `false`，使 input 组件脱离固定定位，避免截图异常
