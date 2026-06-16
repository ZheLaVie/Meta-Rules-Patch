## 🛠️ Meta-Rules-Patch

### 🧐 简介

这是一个**个人维护**的规则补充仓库，用于弥补我在日常使用 [MetaCubeX/meta-rules-dat](https://github.com/MetaCubeX/meta-rules-dat/tree/meta) 时发现的**未覆盖规则和遗漏域名**。

它本质上是我**自用的查漏补缺列表**，旨在提高个人网络配置的准确性，确保流量被正确地代理或直连。

---

### ✨ 核心内容

本仓库位于 `/Rules-dat` 目录下，主要文件结构如下：

| 文件名   | 描述                                                           |
| :------- | :------------------------------------------------------------- |
| `*.list` | 标准的 **域名列表** 规则文件。                                 |
| `*.mrs`  | 针对 **Mihomo/Clash** 的 **RULE-SET** 规则集文件，可直接引用。 |

---

### ⚙️ 如何集成到您的配置

如果您也使用了原版的 Meta 规则集，可以将本仓库的规则作为**补充包**导入。

> **关键原则：** **本仓库的规则应在 Meta 规则之后加载**

#### 1. 规则文件链接示例

您可以直接引用任何一个 `.mrs` 文件，例如 `usmart.mrs` 的 Raw 链接如下：

```
https://raw.githubusercontent.com/ZheLaVie/Meta-Rules-Patch/main/geosite/usmart.mrs
```

或任意`.list` 文件，例如 `direct.list` 的 Raw 链接如下：

```
https://raw.githubusercontent.com/ZheLaVie/Meta-Rules-Patch/main/geosite/direct.list
```

#### 2. 配置片段示例 (以 Clash/Mihomo 配置为例)

您需要在您的配置文件中引用这些规则，并将它们导向正确的策略组（例如 `DIRECT` 或 `PROXY`）。

##### 1. 设置规则匹配

```yaml
rules:
  - RULE-SET,usmart_domain,DIRECT
  - RULE-SET,xxx_domain,PROXY
...
```

##### 2. 设置规则集

```yaml
#设立锚点
rule-anchor:
  domain: &domain { type: http, interval: 86400, behavior: domain, format: mrs }
  text-domain:
    &text-domain { type: http, interval: 86400, behavior: domain, format: text }
#规则集
rule-providers:
  usmart_domain:
    {
      <<: *domain,
      url: "https://raw.githubusercontent.com/ZheLaVie/Meta-Rules-Patch/main/geosite/usmart.mrs",
    }
  direct_domain:
    {
      <<: *text-domain,
      url: "https://raw.githubusercontent.com/ZheLaVie/Meta-Rules-Patch/main/geosite/direct.list",
    }
...
```
