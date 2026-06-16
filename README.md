## 🛠️ Meta-Rules-Patch

### 🧐 简介

这是一个**个人维护**的规则补充仓库，用于弥补我在日常使用 [MetaCubeX/meta-rules-dat](https://github.com/MetaCubeX/meta-rules-dat/tree/meta) 时发现的**未覆盖规则和遗漏域名**。

它本质上是我**自用的查漏补缺列表**，旨在提高个人网络配置的准确性，确保流量被正确地代理或直连。

---

### ✨ 核心内容

本仓库位于 `/Meta-Rules-Patch` 目录下，主要文件结构如下：

| 文件名     | 描述                           |
| :--------- | :----------------------------- |
| `/geoip`   | 标准的 **IP列表** 规则文件。   |
| `/geosite` | 标准的 **域名列表** 规则文件。 |

本仓库位于 `/Meta-Rules-Patch/geoip` 目录下，主要文件结构如下：

| 文件名   | 描述                               |
| :------- | :--------------------------------- |
| `*.list` | 标准的 **IP列表** 规则文本文件。   |
| `*.mrs`  | 标准的 **IP列表** 规则二进制文件。 |

本仓库位于 `/Meta-Rules-Patch/geosite` 目录下，主要文件结构如下：

| 文件名   | 描述                                 |
| :------- | :----------------------------------- |
| `*.list` | 标准的 **域名列表** 规则文本文件。   |
| `*.mrs`  | 标准的 **域名列表** 规则二进制文件。 |

---

### ⚙️ 如何集成到您的配置

如果您也使用了原版的 Meta 规则集，可以将本仓库的规则作为**补充包**导入。

> **关键原则：** **本仓库的规则应在 Meta 规则之后加载**

#### 1. 规则文件链接示例

您可以直接引用任何一个 `.mrs` 文件，例如 `xxx.mrs` 的 Raw 链接如下：

```
https://raw.githubusercontent.com/ZheLaVie/Meta-Rules-Patch/main/geosite/xxx.mrs
```

或任意`.list` 文件，例如 `xxx.list` 的 Raw 链接如下：

```
https://raw.githubusercontent.com/ZheLaVie/Meta-Rules-Patch/main/geoip/xxx.list
```

#### 2. 配置片段示例 (以 Clash/Mihomo 配置为例)

您需要在您的配置文件中引用这些规则，并将它们导向正确的策略组（例如 `DIRECT` 或 `PROXY`）。

##### 1. 设置规则匹配

```yaml
rules:
  - RULE-SET,xxx_domain,DIRECT
  - RULE-SET,xxx_domain,PROXY
  - RULE-SET,xxx_ip,DIRECT,no-resolve
  - RULE-SET,xxx_ip,PROXY,no-resolve
...
```

##### 2. 设置规则集

```yaml
#设立锚点
rule-anchor:
  ip: &ip { type: http, interval: 86400, behavior: ipcidr, format: mrs }
  text-ip:
    &text-ip { type: http, interval: 86400, behavior: ipcidr, format: text }
  domain: &domain { type: http, interval: 86400, behavior: domain, format: mrs }
  text-domain:
    &text-domain { type: http, interval: 86400, behavior: domain, format: text }
#规则集
rule-providers:
  xxx_ip:
    {
      <<: *ip,
      url: "https://raw.githubusercontent.com/ZheLaVie/Meta-Rules-Patch/main/geoip/xxx.mrs",
      interval: 86400,
    }
  xxx_ip:
    {
      <<: *text-ip,
      url: "https://raw.githubusercontent.com/ZheLaVie/Meta-Rules-Patch/main/geoip/xxx.list",
      interval: 86400,
    }
  xxx_domain:
    {
      <<: *domain,
      url: "https://raw.githubusercontent.com/ZheLaVie/Meta-Rules-Patch/main/geosite/xxx.mrs",
    }
  xxx_domain:
    {
      <<: *text-domain,
      url: "https://raw.githubusercontent.com/ZheLaVie/Meta-Rules-Patch/main/geosite/xxx.list",
    }
...
```
