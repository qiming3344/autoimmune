# 蜂巢·免疫系统 Pro v1.0 (AutoImmune)

> 自愈系统免疫引擎 | 自检测→自修复→自上报 | 插件架构 | 纯Python

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Version](https://img.shields.io/badge/Version-1.0.0-orange.svg)]()

AutoImmune是一个**自愈系统免疫引擎**，吸收5大顶级监控系统的精华：
- **Prometheus** — SQLite时间序列趋势数据库
- **Datadog** — 动态阈值调整（非固定阈值）
- **PagerDuty** — P0/P1/P2分级告警升级
- **Chaos Monkey** — 受控故障注入消防演练
- **AWS** — 磁盘/内存自动清理

商业版以**插件架构**独立运行，不绑定蜂巢系统，可接入任何项目。

## 🎯 为什么需要免疫系统？

```
传统监控: 告警 → 等人工处理 → 修复 (平均15分钟+)
AutoImmune: 检测 → 自动修复 → 修复失败则升级上报 (秒级)
```

| 对比 | AutoImmune Pro | Datadog | PagerDuty | Prometheus |
|------|---------------|---------|-----------|------------|
| 自动修复 | ✅ 核心能力 | ❌ 仅告警 | ❌ 仅告警 | ❌ 仅采集 |
| 动态阈值 | ✅ | ✅ | ❌ | ❌ 固定阈值 |
| 消防演练 | ✅ Chaos模式 | ❌ | ❌ | ❌ |
| 插件架构 | ✅ | ❌ | ❌ | ❌ |
| 定价 | ¥99买断 | $15/月+$5/主机 | $21/月 | 免费但需自己修 |
| 数据所有权 | ✅ 本地 | ❌ 云 | ❌ 云 | ✅ 本地 |

## 🚀 快速开始

```bash
# 安装
git clone https://github.com/qiming3344/autoimmune.git
cd autoimmune

# 一次性检查
python auto_immune_pro.py --once

# 守护模式 (每60秒)
python auto_immune_pro.py --daemon

# 添加自定义插件
python auto_immune_pro.py --add-plugin my_check.py
```

## 🛡️ 五大核心引擎

### 1. 📊 趋势数据库 (Prometheus精华)
```python
# SQLite时间序列存储
# 追踪: API响应时间、错误率、内存使用、磁盘空间
# 自动计算: 平均/最大/最小/P95/标准差
```

### 2. 🎚️ 动态阈值 (Datadog精华)
```python
# 不是写死"CPU>80%告警"
# 而是: 学习历史基线 → 偏离2个标准差 → 告警
# 自动适应流量变化
```

### 3. 🚨 分级升级 (PagerDuty精华)
```
P2 (轻微) → 自动修复 + 日志记录
P1 (严重) → 自动修复 + 通知管理员
P0 (致命) → 尝试修复 + 立即告警 + 熔断
```

### 4. 🔥 消防演练 (Chaos Monkey精华)
```bash
python auto_immune_pro.py --drill
# 安全地注入故障 → 验证自动修复能力 → 生成演练报告
```

### 5. 💾 资源守护 (AWS精华)
```python
# 磁盘>80% → 自动清理旧日志
# 内存>90% → 重启泄漏进程
# 文件句柄不足 → 自动释放
```

## 🔌 插件架构

```python
# my_check.py — 自定义检查插件
def check():
    """返回 (status, message)"""
    import requests
    try:
        r = requests.get("https://my-api.com/health", timeout=5)
        return ("ok", f"API正常 {r.elapsed.total_seconds()}s") if r.status_code == 200 else ("error", f"状态码{r.status_code}")
    except Exception as e:
        return ("error", str(e))

def repair():
    """自动修复逻辑（可选）"""
    import subprocess
    subprocess.run(["systemctl", "restart", "my-api"])
    return ("ok", "已重启my-api服务")
```

```bash
# 加载自定义插件
python auto_immune_pro.py --add-plugin my_check.py
# 查看所有已加载插件
python auto_immune_pro.py --list-plugins
```

## 📊 输出示例

```
$ python auto_immune_pro.py --once

═══════════════════════════════════
  AutoImmune Pro v1.0 体检报告
  ️ 2026-07-01 10:00:00
═══════════════════════════════════

[PASS] 磁盘空间: 45% (234GB/500GB)
[PASS] 内存使用: 62% (9.9GB/16GB)
[PASS] API响应: 234ms (基线: 250ms)
[WARN] 错误率: 2.3% (阈值: 2.0%)
       → 自动修复: 重启worker进程...
       → 修复完成, 错误率恢复至0.8%
[PASS] 备份完整性: D+E双盘一致

结果: 4通过, 1自动修复成功
```

## 🏗️ 架构

```
AutoImmune守护进程
├── 采集层: 每N秒采集指标
├── 分析层: 动态阈值 + 趋势检测
├── 决策层: P0/P1/P2分级
├── 修复层: 自动修复动作
├── 告警层: 钉钉/邮件/Webhook
└── 插件层: 用户自定义检查
```

## 💰 定价

| 版本 | 价格 | 适用场景 |
|------|------|----------|
| 社区版 | 免费开源 | 个人项目，基础检查 |
| 专业版 | ¥99买断 | 小团队，5个自定义插件 |
| 企业版 | ¥299买断 | 无限插件+钉钉/邮件告警 |

## 🗺️ 路线图

- [x] 5大引擎融合
- [x] 插件架构
- [x] SQLite趋势数据库
- [x] 动态阈值
- [x] 消防演练模式
- [ ] Web仪表盘 (计划中)
- [ ] 分布式集群监控 (计划中)
- [ ] 机器学习异常检测 (计划中)

## 📄 许可证

MIT License — 详见 [LICENSE](LICENSE)

---

**蜂巢AI实验室** | 让系统学会自己修自己
