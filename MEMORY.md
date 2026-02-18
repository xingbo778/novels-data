- [2026-02-18 工作记录](memory/2026-02-17-work.md) - 视频爬取、小说爬取、Google Drive配置
- [2026-02-17 Code Review 发现](memory/2026-02-17-code-review.md) - 严重 Bug、安全问题、改进建议
- [2026-02-17 浏览器自动化经验](memory/2026-02-17-browser-automation.md) - Browser Use Cloud、小红书发布、API 用法
- [2026-02-17 技术趋势分析](memory/2026-02-17-trends-analysis.md) - GitHub/HN/PH 趋势、小红书内容风格
- [2026-02-17 工作记录](memory/2026-02-17-log.md) - Code Review、Vico 测试、Railway 服务、提权方案
- [2026-02-16 工作记录](memory/2026-02-16.md) - 模型配置、Vico 测试、浏览器远程控制
- [Skills 复用笔记](memory/skills-notes.md) - 可复用的 API 调用和工具使用方法
- [Browser Use Cookies 持久化](memory/browser-use-cookies.md) - Sandbox 模式、Chrome Profile 同步

## 当前任务状态

### 🟡 进行中
- **wnacg 漫画爬虫** - Cloudflare ASN 封锁，部署代理中
- **Railway proxy-service** - 项目已创建，待完成服务部署

### ✅ 已完成
- **real_crawler.py 修复** - Bug修复、选择器更新、视频下载功能
- **技能文档优化** - xiaohongshu-auto, github-trending
- **内容生成器个性化** - 基于 xingbo778 风格
- **Fal.ai 图像生成工具** - 队列模式支持
- **Railway API 工具** - 项目管理
- **内容发布管道** - 整合所有工具
- **Cookies 持久化方案** - Sandbox 模式

## 技术栈清单

### 已配置 API
| 服务 | 用途 | 状态 |
|------|------|------|
| Browser Use Cloud | 浏览器自动化 | ✅ Sandbox 模式 |
| Fal.ai | 图像/视频生成 | ✅ 队列模式 |
| Railway | 服务部署 | ✅ GraphQL API |

### 已创建脚本
| 脚本 | 用途 | 状态 |
|------|------|------|
| xhs_publisher.py | 小红书发布（支持 Sandbox）| ✅ |
| content_pipeline.py | 完整发布管道 | ✅ |
| fal_image_gen.py | 图像生成 | ✅ |
| railway_tool.py | Railway 管理 | ✅ |
| xhs_content_generator.py | 内容生成 | ✅ |
| trends_monitor.py | 趋势监控 | ✅ |
| auto_publish.py | 自动发布 | ✅ |

## 关键发现

### Railway 部署
- Workspace ID: `acdcbd0c-d1f2-4314-9693-498071bcd70f`
- Railway Token: `8a1a4ddc-441d-43e2-9fb7-1f8b1056aa52`

### wnacg.com Cloudflare 封锁
- 错误 1005: ASN 14061 被屏蔽
- 解决方案: 部署 proxy-service 到 Railway 获取新 IP

### Browser Use Sandbox 模式

```python
@sandbox()  # 关键装饰器，自动持久化 cookies
async def task(browser: Browser):
    agent = Agent(task="...", browser=browser, llm=llm)
    await agent.run()
```

**优点**:
- 自动管理 session 和 cookies
- 无需手动处理持久化
- 支持 Cloud 隐身浏览器

### 用户偏好

- 用户建议使用子 agent 执行任务，chat agent 只负责聊天
- 用户提到可以和 @nico2_bot 聊天（Telegram）

## 下一步

1. 修复 real_crawler.py 的严重 Bug
2. 继续 Code Review 剩余文件
3. 等 Browser Use Cloud 会话释放后重试小红书发布
4. 添加 API 认证到 testmanus-backend