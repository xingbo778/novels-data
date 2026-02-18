# 浏览器自动化经验总结

## Browser Use Cloud 关键发现

### 1. 正确的 API 用法
```python
from browser_use import Agent, Browser, ChatOpenAI
import os

os.environ["BROWSER_USE_API_KEY"] = "bu_xxx"

llm = ChatOpenAI(model="gpt-4.1-mini")  # 不是 langchain_openai
browser = Browser(use_cloud=True)  # 关键：use_cloud=True

agent = Agent(task=task, llm=llm, browser=browser)
history = await agent.run()
print(history.final_result())

# 关闭浏览器（正确方法）
try:
    if hasattr(browser, 'close'):
        await browser.close()
except:
    pass
```

### 2. 小红书自动化难点
- **登录需要人工辅助**：滑块验证、协议勾选
- **Cloud 版本绕过 IP 风控**：use_cloud=True
- **发布内容需要登录态**：新会话没有 cookies
- **建议**：保存 cookies 避免重复登录

### 3. 小红书发布按钮
- 首页有"发布"按钮 → 点击会跳转到 creator.xiaohongshu.com
- 需要在创作者平台登录
- 发布流程：点击发布 → 编辑器 → 填写内容 → 点击发布

### 4. 常见错误
- `AttributeError: 'BrowserSession' object has no attribute 'close'`
  - 解决：用 try/except 包裹，或检查 hasattr
- `ImportError: cannot import name 'ChatOpenAI' from langchain_openai`
  - 解决：`from browser_use import ChatOpenAI`

## 创建的技能文件

| 文件 | 用途 |
|------|------|
| `skills/xiaohongshu-auto/SKILL.md` | 小红书自动化指南 |
| `skills/github-trending/SKILL.md` | GitHub 趋势监控 |
| `scripts/trends_monitor.py` | 多平台趋势聚合脚本 |

## API Keys（已在环境变量中）

- **Browser Use Cloud**: `bu_PdIFdp2gH7pFblF2SSKnQVPSD_baIGhmpla3hxyE3ow`
- **Fal.ai**: `6bfa23ed-14f7-47aa-ad11-a90577eee3a2:03b732759c2ed675ab42dd601d346e42`
- **Railway**: `8a1a4ddc-441d-43e2-9fb7-1f8b1056aa52`

## Live URL 格式

Browser Use Cloud 会话实时观看链接：
```
https://live.browser-use.com?wss=https://{browser-id}.cdp1.browser-use.com
```

## 下次实践建议

1. **保存登录态**：登录后保存 cookies 到文件
2. **复用浏览器会话**：避免每次创建新实例
3. **人工辅助点**：标记需要人工介入的步骤（验证码等）
4. **错误重试**：添加重试逻辑处理网络问题
