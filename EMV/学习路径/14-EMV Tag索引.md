# 14 - EMV Tag 索引

这份索引按“常见度 + 工程使用频率 + 排错价值”整理 EMV 高频 Tag，适合作为参考书附录快速查阅。

> 说明：这里强调工程理解和常见用途，不替代正式标签字典。

## 一、应用选择与应用标识相关

| Tag | 名称 | 常见阶段 | 工程意义 |
| --- | --- | --- | --- |
| `4F` | AID | Application Selection | 标识候选或最终支付应用 |
| `50` | Application Label | Application Selection | 用于显示应用名 |
| `84` | DF Name | SELECT / Application Selection | 标识目录或应用名 |
| `87` | Application Priority Indicator | Application Selection | 多应用冲突时用于优先级排序 |
| `9F12` | Application Preferred Name | Application Selection | 更友好的应用显示名 |

## 二、GPO 与读记录相关

| Tag | 名称 | 常见阶段 | 工程意义 |
| --- | --- | --- | --- |
| `82` | AIP | GPO | 卡支持哪些能力 |
| `94` | AFL | GPO | 指导终端读哪些记录 |
| `9F38` | PDOL | GPO 前准备 | 告诉终端 GPO 前要提供哪些对象 |
| `8C` | CDOL1 | Read Record | 第一次 Generate AC 输入模板 |
| `8D` | CDOL2 | Read Record / Online 后 | 第二次 Generate AC 输入模板 |
| `8E` | CVM List | Read Record | 持卡人验证规则清单 |

## 三、卡片账户与有效期相关

| Tag | 名称 | 常见阶段 | 工程意义 |
| --- | --- | --- | --- |
| `5A` | PAN | Read Record / 联机 | 主账号 |
| `5F24` | Application Expiration Date | Processing Restrictions | 卡片有效期 |
| `5F25` | Application Effective Date | Processing Restrictions | 卡片生效日期 |
| `5F34` | PAN Sequence Number | 联机 / 主机匹配 | 区分同卡号不同序列 |
| `57` | Track 2 Equivalent Data | 联机 / 业务 | 常用于主机或回执相关处理 |

## 四、终端与交易输入相关

| Tag | 名称 | 常见阶段 | 工程意义 |
| --- | --- | --- | --- |
| `9F02` | Amount, Authorised | 全流程 / DOL / 联机 | 授权金额 |
| `9F03` | Amount, Other | 全流程 / DOL | 其他金额，如现金回吐等 |
| `9F1A` | Terminal Country Code | GPO / 联机 | 终端国家码 |
| `5F2A` | Transaction Currency Code | GPO / 联机 | 交易币种 |
| `9A` | Transaction Date | GPO / 联机 | 交易日期 |
| `9C` | Transaction Type | GPO / 联机 | 交易类型 |
| `9F37` | Unpredictable Number | GPO / AC / 联机 | 不可预测数，参与密码学输入 |
| `9F41` | Transaction Sequence Counter | 联机 / 日志 | 终端流水计数 |

## 五、风险、验证与动作分析相关

| Tag | 名称 | 常见阶段 | 工程意义 |
| --- | --- | --- | --- |
| `95` | TVR | 风险 / 动作分析 / 联机 | 终端发现的问题事实集 |
| `9B` | TSI | 风险 / 日志 | 记录执行过哪些关键步骤 |
| `9F34` | CVM Results | CVM / 联机 | 持卡人验证执行结果 |
| `9F33` | Terminal Capabilities | GPO / 联机 | 终端能力 |
| `9F35` | Terminal Type | GPO / 联机 | 终端类型 |
| `9F40` | Additional Terminal Capabilities | GPO / 联机 | 附加终端能力 |

## 六、Generate AC 与联机密码学相关

| Tag | 名称 | 常见阶段 | 工程意义 |
| --- | --- | --- | --- |
| `9F26` | Application Cryptogram | AC / 联机 | AC 值，可能是 ARQC / TC / AAC |
| `9F27` | CID | AC | 指示 AC 类型 |
| `9F36` | ATC | AC / 联机 | 卡片交易计数器 |
| `9F10` | IAD | AC / 联机 | 发卡行定义的应用数据 |
| `91` | Issuer Authentication Data | 联机后 / ARPC | 发卡行认证相关数据 |
| `89` | Authorization Code | 联机后 | 主机授权码 |
| `8A` | ARC | 联机后 / AC2 | 授权响应码 |

## 七、ODA 与证书链相关

| Tag | 名称 | 常见阶段 | 工程意义 |
| --- | --- | --- | --- |
| `8F` | CA Public Key Index | Read Record / ODA | 命中本地 CAPK |
| `90` | Issuer Public Key Certificate | ODA | 恢复发卡行公钥 |
| `92` | Issuer Public Key Remainder | ODA | 发卡行公钥补足数据 |
| `9F32` | Issuer Public Key Exponent | ODA | 发卡行公钥指数 |
| `9F46` | ICC Public Key Certificate | ODA | 恢复 ICC 公钥 |
| `9F47` | ICC Public Key Exponent | ODA | ICC 公钥指数 |
| `9F48` | ICC Public Key Remainder | ODA | ICC 公钥补足数据 |
| `93` | Signed Static Application Data | SDA | 静态签名数据 |
| `9F4B` | Signed Dynamic Application Data | DDA / CDA | 动态签名数据 |
| `9F4A` | SDA Tag List | SDA | 指示参与 SDA 的数据列表 |

## 八、脚本与联机后处理相关

| Tag | 名称 | 常见阶段 | 工程意义 |
| --- | --- | --- | --- |
| `71` | Issuer Script Template 1 | 联机后处理 | 常用于 AC2 前脚本 |
| `72` | Issuer Script Template 2 | 联机后处理 | 常用于 AC2 后脚本 |
| `86` | Issuer Script Command | Script | 脚本 APDU 命令 |
| `9F18` | Issuer Script Identifier | Script | 标识脚本批次或来源 |

## 九、非接常见对象

| Tag | 名称 | 常见阶段 | 工程意义 |
| --- | --- | --- | --- |
| `9F66` | TTQ | 非接 GPO 前 / 联机 | 非接终端交易能力 |
| `9F6C` | Card Transaction Qualifiers / Brand-specific object | 非接 | 常见于品牌私有解释 |
| `9F6E` | Form Factor / Third Party Data / Brand-specific object | 非接 | 常见于品牌私有解释 |

## 十、最值得优先记住的一组 Tag

如果你现在只想先记住最关键的一组，建议先掌握下面这些：

- `4F` AID
- `82` AIP
- `94` AFL
- `95` TVR
- `9B` TSI
- `9F34` CVM Results
- `9F26` Application Cryptogram
- `9F27` CID
- `9F36` ATC
- `9F10` IAD
- `9F37` UN

这组 Tag 足以支撑你读懂大部分交易主线和联机主线。

## 十一、按问题反查 Tag 的建议

### 1. GPO 失败

优先看：

- `9F38` PDOL
- `82` AIP
- `94` AFL
- `9F66` TTQ（非接）

### 2. ODA 失败

优先看：

- `8F`
- `90` / `92` / `9F32`
- `9F46` / `9F47` / `9F48`
- `93`
- `9F4B`
- `9F4A`

### 3. CVM 行为异常

优先看：

- `8E`
- `9F34`
- `82`
- `9F33`
- `9F40`

### 4. 主机说 ARQC 校验失败

优先看：

- `9F26`
- `9F27`
- `9F36`
- `9F37`
- `95`
- `9F10`
- `82`

## 十二、与其他文档的关系

- 想看这些 Tag 在什么阶段出现，请结合 [02-交易流程与核心对象](02-%E4%BA%A4%E6%98%93%E6%B5%81%E7%A8%8B%E4%B8%8E%E6%A0%B8%E5%BF%83%E5%AF%B9%E8%B1%A1.md)
- 想看它们如何用于联机和密码学，请结合 [03-密码学、联机与脚本](03-%E5%AF%86%E7%A0%81%E5%AD%A6%E3%80%81%E8%81%94%E6%9C%BA%E4%B8%8E%E8%84%9A%E6%9C%AC.md)
- 想看它们如何出现在日志和案例里，请结合 [06-真实交易日志逐行解读](06-%E7%9C%9F%E5%AE%9E%E4%BA%A4%E6%98%93%E6%97%A5%E5%BF%97%E9%80%90%E8%A1%8C%E8%A7%A3%E8%AF%BB.md) 和 [11-真实案例库](11-%E7%9C%9F%E5%AE%9E%E6%A1%88%E4%BE%8B%E5%BA%93.md)
