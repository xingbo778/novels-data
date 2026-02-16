# Skills 复用笔记

本文档记录工作中积累的可复用知识。

---

## 1. Vico 视频生成 API

多智能体 AI 视频生成服务。

**Base URL**: `https://web-production-ab409.up.railway.app`

**可用模型**: Seedance, Veo3, Veo3-Reference, MiniMax-Hailuo-02, Kling（推荐）

**快速调用**:
```bash
# 创建会话 + 发送请求（一步到位）
curl -X POST "https://web-production-ab409.up.railway.app/api/v1/generate-video" \
  -G --data-urlencode "prompt=视频描述" --data-urlencode "i2v_model=Kling"

# 发送消息
curl -X POST https://web-production-ab409.up.railway.app/api/v1/run \
  -H "Content-Type: application/json" \
  -d '{"session_id":"xxx","message":"生成视频","streaming":false}'

# 查看产物
curl "https://web-production-ab409.up.railway.app/api/v1/sessions/{id}/artifacts?user_id=xxx"
```

**GitHub**: `git@github.com:xingbo778/vico.git`

---

## 2. Browser Remote Control

浏览器远程控制和自动化工具。

**GitHub**: `git@github.com:xingbo778/browser-remote-control.git`

**功能**:
- CDP 实时画面流
- 远程点击/输入
- 验证码辅助
- Browser-Use Agent 自动化

**小红书登录脚本**: `automation-scripts/xhs_login.py`

---

## 3. Railway API 部署

**API Token 使用**:
```bash
curl -X POST "https://backboard.railway.app/graphql/v2" \
  -H "Authorization: Bearer {TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{"query": "{ me { id email workspaces { id name } } }"}'
```

**创建项目**:
```bash
curl -X POST "https://backboard.railway.app/graphql/v2" \
  -H "Authorization: Bearer {TOKEN}" \
  -d '{"query": "mutation { projectCreate(input: { name: \"xxx\", workspaceId: \"xxx\" }) { id name } }"}'
```

---

## 4. OpenRouter 模型配置

**API 调用**:
```bash
curl -X POST https://api.openrouter.ai/v1/chat/completions \
  -H "Authorization: Bearer {KEY}" \
  -H "Content-Type: application/json" \
  -d '{"model":"openrouter/glm5","messages":[{"role":"user","content":"hi"}]}'
```

**推荐模型**:
- `openrouter/glm5` - 中文好，速度快
- `openrouter/minimax-m2.5` - 中文好
- `openai/gpt-5-mini` - 平衡
- `openrouter/anthropic/claude-opus-4` - 长任务

---

## 5. 小红书 IP 风控

小红书会检测无头浏览器和 IP 风险。

**错误码 300012**: IP at risk

**解决方案**:
1. 使用有头浏览器 + Xvfb
2. 使用代理
3. 切换网络/IP
