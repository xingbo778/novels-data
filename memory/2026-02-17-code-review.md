# Code Review 发现 - 2026-02-17

## 🔴 严重 Bug

### real_crawler.py
- **问题**: `crawl_hsex()` 函数中 `return videos` 在 `for` 循环内，导致只返回第一个结果
- **位置**: 第 32 行
- **修复**: 将 `return videos` 移到循环外

```python
# 错误
for item in soup.find_all('div', class_='video-item')[:10]:
    ...
    return videos  # ← 提前返回！

# 正确
for item in soup.find_all('div', class_='video-item')[:10]:
    ...
return videos  # ← 循环外
```

## ⚠️ 安全问题

### testmanus-backend/app.py
- **问题**: 无 API 认证，任何人都可以调用
- **建议**: 添加 `@require_auth` 装饰器
- **其他**: 无输入验证、无速率限制

### grok_image_gen.js
- **问题**: 敏感内容硬编码在源码中
- **建议**: 从环境变量或命令行参数读取

## 📊 代码质量评分

| 文件 | 评分 | 主要问题 |
|------|------|---------|
| novel_scraper/scraper.py | ⭐⭐⭐⭐ | 无重试机制、无并发 |
| real_crawler.py | ⭐⭐ | 严重 Bug、裸露 except |
| testmanus-backend/app.py | ⭐⭐⭐ | 无认证、无日志 |
| grok_image_gen.js | ⭐⭐ | 无错误处理、硬编码 |

## 💡 改进建议

### 通用改进
1. 所有爬虫添加重试机制 (`tenacity` 库)
2. 添加速率限制避免被封
3. 使用 `logging` 替代 `print`
4. 配置外部化（环境变量/配置文件）

### novel_scraper 特定
- 使用 `asyncio + aiohttp` 并发下载
- 黑名单从外部文件加载
- 增加更多内容选择器

### testmanus-backend 特定
- 添加 API Key 认证
- 添加请求速率限制
- 添加输入长度验证
- 添加结构化日志

## 📁 已 Review 文件列表

1. `/home/node/.openclaw/workspace/novel_scraper/scraper.py`
2. `/home/node/.openclaw/workspace/real_crawler.py`
3. `/home/node/.openclaw/workspace/testmanus-backend/app.py`
4. `/home/node/.openclaw/workspace/grok_image_gen.js`

## 待 Review 文件

- `grok_image_gen_v2.js`
- `grok_image_final.js`
- `scrape_novels.js`
- `scrape_with_browser.js`
- `twitter_post_greenhat.js`
- `text_to_long_image.js`
- `real_crawler.js`
- `hsex_greenhat_crawler.js`
