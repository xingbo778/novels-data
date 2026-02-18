# Browser Use Cloud Cookies 持久化方案

## 问题
小红书发布需要登录，每次创建新的浏览器会话没有 cookies。

## 解决方案

### 方案 1: Sandbox 模式（推荐）✅

**特点**: 自动持久化 cookies 和 session

```python
from browser_use import Browser, sandbox
from browser_use.agent.service import Agent

@sandbox()  # 关键装饰器
async def xhs_publish(browser: Browser):
    agent = Agent(task="发布内容", browser=browser, llm=llm)
    await agent.run()

# 直接调用，框架自动管理持久化
asyncio.run(xhs_publish())
```

**优点**:
- 自动管理 cookies
- 自动管理 session
- 无需手动操作

### 方案 2: Chrome Profile 同步

**特点**: 复用本地 Chrome 的登录状态

```bash
# 同步本地 Chrome profile 到 Cloud
curl -fsSL https://browser-use.com/profile.sh | BROWSER_USE_API_KEY=xxx sh
```

**使用**:
```python
browser = Browser(use_cloud=True)  # 自动加载同步的 profile
```

**优点**:
- 可以复用现有登录
- 适合本地开发

**限制**:
- 需要在有 Chrome 的机器上执行同步
- 需要 GUI 环境

### 方案 3: 手动管理 Cookies

**不推荐** - Cloud 模式不支持直接获取/设置 cookies

## 最佳实践

### 小红书发布流程

```bash
# 1. 首次登录（手动完成验证）
python xhs_publisher.py --sandbox --login

# 2. 发布内容（cookies 已保存）
python xhs_publisher.py --sandbox --publish \
    --title "标题" \
    --body "正文"
```

### 注意事项

1. **Cloud 并发限制**: 最多同时运行 N 个会话（取决于套餐）
2. **Session 过期**: Sandbox 模式会保持 session 活跃
3. **验证码**: 某些网站仍需要手动处理验证码
4. **成本**: Cloud 模式按使用量计费

## 相关文件

| 文件 | 用途 |
|------|------|
| `scripts/xhs_publisher.py` | 小红书发布工具（支持 sandbox） |
| `scripts/sync_chrome_profile.sh` | Chrome profile 同步脚本 |
| `scripts/xhs_cookies.py` | Cookies 管理工具（备用） |

## 测试状态

- [ ] Sandbox 模式登录测试
- [ ] Sandbox 模式发布测试
- [ ] Chrome Profile 同步测试

## 下一步

1. 运行 `python xhs_publisher.py --sandbox --login` 完成登录
2. 测试发布功能
3. 记录成功案例
