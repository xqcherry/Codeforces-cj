# Codeforces Rating & Problems

Tampermonkey 用户脚本，用于在 Codeforces 页面上显示当前登录用户的 Rating，并从自建服务获取个性化题目推荐列表。脚本在页面右上方注入按钮与面板，支持一键打开题面、展示题目标签及通过人数等信息。

## 功能特性

- 固定悬浮按钮 `获取题目推荐`，点击即可拉取最新 Rating 与推荐列表。
- 高亮状态提示区，反馈数据获取进度与错误信息。
- 推荐题面板展示题目标题、难度颜色、算法标签、通过人数以及一键跳转按钮。
- 自动调用 `https://codeforces.com/api/problemset.problems` 缓存过题人数，提升后续渲染速度。
- 后端回传数据格式未固定，只要包含 `feedList` 数组即可被渲染。

## 环境依赖

- Chromium 或 Firefox 浏览器（支持 Tampermonkey / Violentmonkey）。
- Tampermonkey 插件（或等效的用户脚本管理器）。
- 可访问 Codeforces 官方 API。
- 可用的推荐服务接口，默认指向 `http://36.111.157.95:8010/HttpService/Echo`。

## 快速开始

1. 安装 Tampermonkey。
2. 克隆/下载本仓库或直接复制 `codeforces.js`。
3. 在 Tampermonkey 中创建新脚本，将源码完整粘贴并保存。
4. 登录 Codeforces，刷新页面。
5. 点击右上角 `获取题目推荐`：
   - 首先会通过 `user.info` 接口获取当前登录账号的 Rating。
   - Rating 与用户名 POST 至后端，后端返回 `feedList`（题目数组）。
   - 面板显示题目详情，可点击 `查看` 在新标签页打开题面。

## 目录结构

- `codeforces.js`：唯一的 Tampermonkey 用户脚本文件。

## 后端接口约定

脚本以 JSON 形式向后端发送：

```json
{
  "username": "tourist",
  "rating": 3800
}
```

后端返回数据只要包含 `feedList` 即可，例如：

```json
{
  "feedList": [
    {
      "title": "1580A - A+B Problem",
      "url": "https://codeforces.com/problemset/problem/1580/A",
      "rating": 1400,
      "tag": ["math", "implementation"]
    }
  ]
}
```

其他字段会被忽略。`feedList` 缺失或解析失败会在状态提示中显示错误信息。

## 自定义与二次开发

- **样式**：修改 `GM_addStyle` 中的 CSS，控制按钮、面板、tooltip 等视觉效果。
- **推荐来源**：调整 `sendToBackend` 中的 `url`，即可接入自有服务。
- **难度配色**：编辑 `getRatingColor` 函数，满足自定义评级分段。
- **状态提示**：`updateStatus` 负责信息展示，可根据需要调整动画或时长。

## 调试建议

- 浏览器开发者工具 Console 查看日志：脚本会输出加载缓存、后端返回或错误信息。
- 网络面板确认 Codeforces API 与自建服务请求是否成功。
- 若状态提示为 “未检测到登录用户”，请确认已登录且页面 DOM 中存在 `/profile/` 链接。

## 许可

未显式指定许可证，可根据实际使用场景补充合适的开源协议。

