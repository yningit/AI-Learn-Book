# 17 - AID / RID / CAPK 参考表

这份附录用于把 AID、RID、CAPK、Index 这些在应用选择与 ODA 中高频出现、但初学者又最容易混淆的对象放到同一张参考表里。

它的目标是帮助你回答：

- AID、RID、CAPK 到底是什么关系
- 终端如何从 AID 走到 CAPK
- 为什么 ODA 问题经常要同时看 AID、RID、Index、CAPK

> 说明：本文强调对象关系、工程理解和治理建议，不提供完整官方品牌注册表。

## 一、先建立关系图

可以把这几个对象理解成下面这条链：

```text
AID
  -> 前 5 字节通常包含 RID
      -> RID + CA Public Key Index
          -> 命中终端本地 CAPK
              -> 用于恢复 Issuer / ICC 公钥链
```

## 二、AID 是什么

AID（Application Identifier）是应用标识符，用于在 Application Selection 阶段确定交易由哪个应用接管。

### 1. AID 的工程作用

- 决定最终应用
- 决定品牌 profile / scheme profile
- 在很多系统里，AID 还是参数配置和联机策略的入口键

### 2. 工程上要记录的 AID 信息

建议至少记录：

- AID 本身
- Label / Preferred Name
- 优先级
- 是否支持部分匹配
- 接触 / 非接支持情况
- 所属 scheme profile

## 三、RID 是什么

RID（Registered Application Provider Identifier）通常是 AID 前 5 字节中的注册应用提供方标识。

### 1. RID 的工程作用

- 标识所属卡组织 / 应用提供方
- 帮助终端定位 CAPK 集合
- 在很多系统里，也是路由到品牌 profile 的辅助键

### 2. 一个最重要的理解

不要把 RID 当作完整应用标识。它更像是：

- “这个应用大体属于哪个体系”

而不是：

- “这次交易最终一定是哪一个具体应用”

## 四、CA Public Key Index 是什么

CA Public Key Index 通常来自卡片返回对象，用来告诉终端：

- 在 RID 对应的 CAPK 集合中，应选择哪一把 CA 公钥

所以工程上通常会按：

- `RID + Index`

来命中某一条 CAPK 记录。

## 五、CAPK 是什么

CAPK（Certification Authority Public Key）是终端本地预装的 CA 公钥。

### 1. CAPK 的工程作用

- 它是 ODA 信任链的起点
- 它不是卡片公钥
- 它通常是静态下发、版本管理和更新的参数对象

### 2. CAPK 常见管理字段

建议至少包含：

- RID
- Index
- Key modulus
- Exponent
- 有效期
- 来源与版本号
- 环境标识（测试 / 生产）

## 六、AID、RID、CAPK 的关系表

| 对象 | 作用 | 常在哪一步用到 |
| --- | --- | --- |
| AID | 选择应用、进入正确交易 profile | Application Selection |
| RID | 标识体系 / 帮助命中公钥集合 | ODA / 参数路由 |
| Index | 在 RID 对应集合里定位具体 CAPK | ODA |
| CAPK | ODA 信任根 | ODA |

## 七、常见卡组织与 RID 参考理解

> 这里给出的是“工程理解上的常见映射思路”，不是官方完整列表。

| 卡组织 / 体系 | 工程上常见理解 |
| --- | --- |
| Visa | 常见国际品牌 profile，需要独立 AID、host mapping 和测试路径 |
| Mastercard | 常见国际品牌 profile，尤其要注意非接行为和 outcome |
| UnionPay | 常见本地化受理 profile，参数和主机协同常更关键 |
| Amex | 品牌私有对象和联机路径需独立 profile |
| Discover / Diners | 路由、host mapping 和受理网络兼容性需特别注意 |
| RuPay | 本地化规则、参数和 host 协同常是重点 |
| Pure | 特定地区或项目 profile，需独立管理，不宜硬套其他国际品牌逻辑 |
| JCB | 品牌 profile、主机字段和参数配合需单独验证 |

## 八、为什么 ODA 排错经常要同时看这 4 个对象

### 1. 如果 AID 进错 profile

可能会导致：

- 错误的品牌参数
- 错误的 CAPK 集合
- 错误的 host mapping

### 2. 如果 RID 解释错

可能会导致：

- 去错误的 CAPK 库里查找
- ODA 路径天然失败

### 3. 如果 Index 不匹配

可能会导致：

- 虽然 RID 正确，但命中不到正确 CAPK
- ODA 无法恢复 Issuer Public Key

### 4. 如果 CAPK 版本旧

可能会导致：

- 某些卡在某些终端正常、在另一些终端失败
- 测试环境正常、生产环境失败

## 九、一个常见排查顺序

遇到 ODA 或品牌 profile 问题时，建议按下面顺序检查：

1. 最终 AID 是什么
2. AID 对应的 scheme profile 是什么
3. AID 前缀 / RID 是否与预期一致
4. 卡返回的 CA Index 是什么
5. 本地是否存在对应 `RID + Index` 的 CAPK
6. CAPK 版本、有效期和环境是否正确

## 十、一个常见问题对照表

| 现象 | 优先检查 |
| --- | --- |
| 选卡正常，但 ODA 失败 | RID、Index、CAPK |
| 同品牌部分卡通过、部分卡失败 | CAPK 版本、Index 命中 |
| 同卡不同终端表现不同 | 参数版本、CAPK 表、AID profile |
| 非接路由看起来正常，但联机 / ODA 不一致 | AID profile、scheme mapping、CAPK 集合 |

## 十一、工程治理建议

### 1. AID 治理建议

- 按品牌 / 终端能力 / 接口类型分层管理
- 带版本号和审批记录
- 支持灰度与回滚

### 2. CAPK 治理建议

- 按 `RID + Index` 管理
- 明确测试和生产隔离
- 提供版本追踪和有效期检查

### 3. 日志建议

日志里最好能打印：

- 最终 AID
- 命中的 scheme profile
- RID
- CA Index
- 命中的 CAPK 标识和版本

这样现场排错时，很多“为什么 ODA 不过”的问题会明显更快定位。

## 十二、初学者最容易误解的地方

1. 把 CAPK 当成卡片公钥。  
2. 把 RID 当成完整 AID。  
3. 忽略了 Index 在 CAPK 命中中的作用。  
4. 认为 ODA 失败只和密码学实现有关，不看参数治理。  

## 十三、与其他文档的关系

- 想看 ODA 和证书链背景，请结合 [03-密码学、联机与脚本](03-%E5%AF%86%E7%A0%81%E5%AD%A6%E3%80%81%E8%81%94%E6%9C%BA%E4%B8%8E%E8%84%9A%E6%9C%AC.md)
- 想看参数治理，请结合 [09-参数与配置治理](09-%E5%8F%82%E6%95%B0%E4%B8%8E%E9%85%8D%E7%BD%AE%E6%B2%BB%E7%90%86.md)
- 想看多卡组织实现，请结合 [12-卡组织差异全景](12-%E5%8D%A1%E7%BB%84%E7%BB%87%E5%B7%AE%E5%BC%82%E5%85%A8%E6%99%AF.md)
