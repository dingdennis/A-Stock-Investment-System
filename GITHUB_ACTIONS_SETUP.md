# GitHub Actions 配置指南

## 快速配置（5 分钟）

### 1. 启用 Actions
1. 访问仓库：https://github.com/dingdennis/A-Stock-Investment-System
2. 点击 **Actions** 标签
3. 点击 **I understand my workflows, go ahead and enable them**

### 2. 配置 Secrets（必须）

进入 **Settings → Secrets and variables → Actions → New repository secret**

| Secret 名称 | 值 | 说明 |
|------------|-----|------|
| `OPENAI_BASE_URL` | `https://coding.dashscope.aliyuncs.com/v1` | 百炼 API 地址 |
| `OPENAI_API_KEY` | `sk-sp-66efd4ff5b494ca5b0eede6f2d2d3710` | 百炼 API Key |
| `OPENAI_MODEL` | `qwen3.5-plus` | 模型名称 |
| `STOCK_LIST` | `600418` | 自选股列表（逗号分隔） |

### 3. 配置 Variables（可选）

进入 **Settings → Secrets and variables → Actions → Variables → New repository variable**

| Variable 名称 | 值 | 说明 |
|--------------|-----|------|
| `REPORT_TYPE` | `full` | 报告类型（full/simple/brief） |
| `REPORT_LANGUAGE` | `zh` | 报告语言（zh/en） |

### 4. 手动触发测试

1. 进入 **Actions** 标签
2. 点击左侧 **每日股票分析** 工作流
3. 点击 **Run workflow** 按钮
4. 选择分支 `main`
5. 点击 **Run workflow**

### 5. 查看运行结果

- 运行中：显示黄色图标 ⏳
- 运行成功：显示绿色勾 ✅
- 运行失败：显示红色叉 ❌

点击运行记录可查看详细日志

---

## 定时任务说明

**默认时间：** 每个工作日 北京时间 18:00（UTC 10:00）

**修改时间：** 编辑 `.github/workflows/daily_analysis.yml`
```yaml
schedule:
  - cron: '0 10 * * 1-5'  # 修改这里
```

**Cron 格式：** `分 时 日 月 周`

**常用配置：**
| 时间 | Cron 表达式 |
|------|------------|
| 每天 9:00（北京时间） | `0 1 * * *` |
| 每天 18:00（北京时间） | `0 10 * * *` |
| 工作日 9:00 | `0 1 * * 1-5` |
| 工作日 18:00 | `0 10 * * 1-5` |

---

## 通知渠道配置（可选）

添加以下 Secrets 启用推送：

| Secret 名称 | 说明 |
|------------|------|
| `WECHAT_WEBHOOK_URL` | 企业微信机器人 |
| `DINGTALK_WEBHOOK_URL` | 钉钉机器人 |
| `FEISHU_WEBHOOK_URL` | 飞书机器人 |
| `TELEGRAM_BOT_TOKEN` | Telegram Bot |
| `TELEGRAM_CHAT_ID` | Telegram Chat ID |
| `EMAIL_SENDER` | 发件人邮箱 |
| `EMAIL_PASSWORD` | 邮箱授权码 |
| `EMAIL_RECEIVERS` | 收件人邮箱 |

---

## 故障排查

### Actions 不运行
1. 检查是否已启用 Actions
2. 检查 Secrets 配置是否正确
3. 查看 Actions 页面的运行日志

### API 调用失败
1. 检查 `OPENAI_API_KEY` 是否有效
2. 检查 `OPENAI_BASE_URL` 是否正确
3. 查看日志中的错误信息

### 股票分析失败
1. 检查 `STOCK_LIST` 格式（如 `600418,000858`）
2. 检查数据源 API 是否可用
3. 查看日志中的详细错误

---

## 成本估算

**每次分析：** 约 1000-3000 tokens per 股票

**百炼 qwen3.5-plus 价格：**
- 输入：¥0.002 / 1K tokens
- 输出：¥0.006 / 1K tokens

**每日成本（1 只股票）：** 约 ¥0.01-0.03
**每月成本（22 交易日）：** 约 ¥0.2-0.6

---

**配置完成后，系统将自动在每个工作日 18:00 分析股票并推送报告！**
