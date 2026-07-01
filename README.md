# 蜂巢·免疫系统 AutoImmune Pro v2.0

> 通用自愈免疫引擎 | 5大引擎融合 | 15预置Check | LLM诊断 | Web面板

[![Python](https://img.shields.io/badge/Python-3.8+-blue)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)
[![Version](https://img.shields.io/badge/Version-2.0.0-orange)]()

AutoImmune是一个**通用自愈免疫引擎**，吸收5大顶级监控系统的精华，1300行纯Python，零外部依赖。自检测→自修复→自上报→修不了升级告警。

## 5大引擎融合

| 引擎 | 吸收自 | 实现 |
|------|--------|------|
| 时序数据库 | **Prometheus** | SQLite轻量存储 |
| 动态阈值 | **Datadog** | 中位数+3σ自适应 |
| 告警分级 | **PagerDuty** | P0/P1/P2三级 |
| 消防演练 | **Chaos Monkey** | 定期故障注入验证 |
| 资源监控 | **AWS** | 磁盘/内存自动清理 |

## 为什么选择AutoImmune？

| 对比 | AutoImmune Pro | Datadog | PagerDuty | OneUptime |
|------|:----------:|:-------:|:---------:|:---------:|
| 自动修复 | ✅ 核心能力 | ❌ 仅告警 | ❌ 仅告警 | ✅ PR生成 |
| 动态阈值 | ✅ 3σ自适应 | ✅ | ❌ | ✅ |
| 消防演练 | ✅ 独有 | ❌ | ❌ | ❌ |
| LLM诊断 | ✅ 可选 | ❌ | ❌ | ✅ |
| 告警关联降噪 | ✅ | ❌ | ❌ | ✅ |
| 部署 | **单文件·1秒** | Agent安装 | SaaS | Docker集群 |
| 定价 | **免费开源** | $15/月/主机 | $21/月 | 免费开源 |

## 快速开始

```bash
# 单次检查
python auto_immune_pro.py --once

# 守护模式(每60秒)
python auto_immune_pro.py --daemon 60

# Web仪表盘
python auto_immune_pro.py --web

# 演示
python auto_immune_pro.py --demo
```

## SDK用法

```python
from auto_immune_pro import AutoImmune, Check, BuiltinChecks

ai = AutoImmune('production', notify_channels=['dingtalk', 'stdout'])
bc = BuiltinChecks()

ai.add_checks(
    bc.cpu(90), bc.memory(85), bc.disk(max_pct=80),
    bc.http_endpoint('https://api.example.com/health'),
    bc.ssl_expiry('example.com', 30),
)
result = ai.run()
# → LLM诊断 + 告警关联 + 消防演练 + 升级检测 + 多渠道通知
```

## 15个预置Check

cpu | memory | disk | process_alive | network | http_endpoint | tcp_service | ssl_expiry | dns_resolve | file_age | log_errors | port_listening | response_time | command_output | env_var_set

## LLM诊断（可选）

```bash
# 配了Key = AI根因分析
set AUTOIMMUNE_LLM_KEY=sk-你的密钥
set AUTOIMMUNE_LLM_URL=https://api.deepseek.com/v1/chat/completions

# 不配Key = 规则引擎降级 → 零费用
```

## 定价

| 版本 | 价格 | 说明 |
|------|------|------|
| 社区版 | **免费** (MIT开源) | 所有功能 |
| 专业版 | ¥99 永久买断 | 商用授权·技术支持 |
| 企业版 | ¥299 永久买断 | 源码可用·定制Check |

## 反馈

- 🐛 [提交Issue](https://github.com/qiming3344/autoimmune/issues)
- 📧 weiweilbj@163.com

---

**蜂巢AI实验室** | 让系统学会自己修自己 | 蜂群智能·免疫自愈
