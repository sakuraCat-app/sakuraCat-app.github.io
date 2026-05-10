---
layout: post
title: Clash 配置详解与最佳实践
date: 2024-05-02 09:15:00 +0800
tags: [Clash, 配置, 进阶]
author: SakuraCat
reading_time: 12
excerpt: 深入讲解 Clash 配置文件的各个部分，帮助您优化网络体验。
---

## Clash 配置文件结构

Clash 配置文件采用 YAML 格式，主要包含以下几个部分：

```yaml
# 基本配置
port: 7890
socks-port: 7891
redir-port: 7892
mixed-port: 7893

# DNS 配置
dns:
  enable: true
  listen: 0.0.0.0:53
  default-nameserver:
    - 8.8.8.8
    - 1.1.1.1

# 代理配置
proxies: []

# 代理组
proxy-groups: []

# 规则
rules: []
```

## 详细配置说明

### 1. 端口配置

```yaml
# HTTP 代理端口
port: 7890

# SOCKS5 代理端口
socks-port: 7891

# 重定向端口（Linux/macOS）
redir-port: 7892

# 混合端口（HTTP + SOCKS5）
mixed-port: 7893
```

### 2. DNS 配置

```yaml
dns:
  enable: true
  listen: 0.0.0.0:53
  
  # DNS 服务器
  nameserver:
    - 8.8.8.8
    - 1.1.1.1
    - 119.29.29.29
  
  # 备用 DNS
  fallback:
    - 8.8.4.4
    - 1.0.0.1
  
  # DNS 缓存
  cache-size: 4096
```

### 3. 代理配置

#### Shadowsocks 节点
```yaml
proxies:
  - name: "SS节点"
    type: ss
    server: 1.2.3.4
    port: 8388
    cipher: aes-256-gcm
    password: password123
```

#### Trojan 节点
```yaml
  - name: "Trojan节点"
    type: trojan
    server: 1.2.3.4
    port: 443
    password: password123
    sni: example.com
    skip-cert-verify: false
```

#### VMess 节点
```yaml
  - name: "VMess节点"
    type: vmess
    server: 1.2.3.4
    port: 443
    uuid: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    alterId: 0
    cipher: auto
    tls: true
    skip-cert-verify: false
    servername: example.com
```

### 4. 代理组配置

```yaml
proxy-groups:
  # 自动选择（根据延迟）
  - name: "自动选择"
    type: url-test
    proxies:
      - "SS节点"
      - "Trojan节点"
    url: 'http://www.gstatic.com/generate_204'
    interval: 300
    tolerance: 50

  # 手动选择
  - name: "手动选择"
    type: select
    proxies:
      - "SS节点"
      - "Trojan节点"
      - "自动选择"

  # 负载均衡
  - name: "负载均衡"
    type: load-balance
    proxies:
      - "SS节点"
      - "Trojan节点"
    url: 'http://www.gstatic.com/generate_204'
    interval: 300

  # 故障转移
  - name: "故障转移"
    type: fallback
    proxies:
      - "SS节点"
      - "Trojan节点"
    url: 'http://www.gstatic.com/generate_204'
    interval: 300
```

### 5. 规则配置

```yaml
rules:
  # 直连规则
  - DOMAIN-SUFFIX,cn,DIRECT
  - DOMAIN-SUFFIX,baidu.com,DIRECT
  - GEOIP,CN,DIRECT
  
  # 代理规则
  - DOMAIN-SUFFIX,google.com,代理
  - DOMAIN-SUFFIX,github.com,代理
  - DOMAIN-SUFFIX,youtube.com,代理
  
  # 匹配所有其他流量
  - MATCH,代理
```

## 规则类型详解

| 规则类型 | 说明 | 示例 |
|---------|------|------|
| DOMAIN | 精确匹配域名 | DOMAIN,example.com,PROXY |
| DOMAIN-SUFFIX | 域名后缀匹配 | DOMAIN-SUFFIX,google.com,PROXY |
| DOMAIN-KEYWORD | 域名关键字匹配 | DOMAIN-KEYWORD,google,PROXY |
| GEOIP | IP 地址地理位置 | GEOIP,CN,DIRECT |
| IP-CIDR | IP 段匹配 | IP-CIDR,192.168.0.0/16,DIRECT |
| MATCH | 匹配所有流量 | MATCH,PROXY |

## 性能优化建议

### 1. 减少规则数量
- 使用规则集而不是单个规则
- 定期清理过期规则
- 合并相似的规则

### 2. 优化 DNS 配置
```yaml
dns:
  enable: true
  # 使用国内 DNS 加速
  nameserver:
    - 119.29.29.29  # 腾讯 DNS
    - 223.5.5.5     # 阿里 DNS
  
  # 使用国外 DNS 用于代理
  fallback:
    - 8.8.8.8
    - 1.1.1.1
```

### 3. 调整并发连接
```yaml
# 增加并发连接数
tcp-concurrent: true
```

## 常见配置错误

❌ **错误示例**：
```yaml
# 端口冲突
port: 80  # 需要 root 权限

# DNS 配置错误
dns:
  nameserver: 8.8.8.8  # 应该是列表格式
```

✅ **正确示例**：
```yaml
# 使用高端口
port: 7890

# 正确的 DNS 配置
dns:
  nameserver:
    - 8.8.8.8
```

## 测试您的配置

### 1. 验证 YAML 语法
使用在线 YAML 验证工具检查配置文件

### 2. 测试连接
```bash
# 测试代理连接
curl -x http://127.0.0.1:7890 http://www.google.com
```

### 3. 检查日志
查看 Clash 日志文件，排查连接问题

## 高级配置

### 使用规则集
```yaml
rule-providers:
  china:
    type: http
    behavior: domain
    url: "https://example.com/china.yaml"
    path: ./ruleset/china.yaml
    interval: 86400

rules:
  - RULE-SET,china,DIRECT
  - MATCH,PROXY
```

### 使用代理集
```yaml
proxy-providers:
  provider1:
    type: http
    url: "https://example.com/proxies.yaml"
    path: ./proxies/provider1.yaml
    interval: 3600
    health-check:
      enable: true
      interval: 600
      url: http://www.gstatic.com/generate_204
```

## 相关资源

- [Clash 官方配置文档](https://github.com/Dreamacro/clash/wiki/Configuration)
- [YAML 语法指南](https://yaml.org/)
- [规则集合集](https://github.com/Loyalsoldier/clash-rules)

---

**提示**：配置完成后，重启 Clash 使设置生效。

**更新时间**：2024年5月2日
