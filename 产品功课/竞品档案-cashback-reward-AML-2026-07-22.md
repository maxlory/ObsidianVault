# 竞品档案：Cashback 结算 · XAUT Staking Reward · 出金白名单 + 入金 AML（纵向深挖 + 实现路径反推）

> **生成方式**：本文档由 **competitor-profiling skill** 生成
> **日期**：2026-07-22（所有外部事实截至本日；费率/利率/冷静期类数字时效性强，随时会变）
> **输入**：**基于已抓取的 78 个 raw 文件综合**（Nexo 31 / Crypto.com 26 / Binance 9 / Kraken 8 / Falcon 4），无联网重抓；叠加复用两份既有成果——
> - 需求权威来源：`/Users/aa00158/xaue-project/新需求-staking-reward与AML/三条新需求—需求总参考.md`
> - 横向对比 + 决策简报（本档案与之差异化，不重复其内容）：`/Users/aa00158/Documents/xaue-产品功课/竞品简报-cashback-reward-AML-2026-07-22.md`
> - 公司层基本面（Nexo/Crypto.com 牌照/口碑/规模，直接引用不重写）：`/Users/aa00158/Documents/xaue-产品功课/竞品档案-Nexo-CryptoCom-2026-07-22.md`
> - Raw 归档根目录：`/Users/aa00158/Documents/xaue-产品功课/competitor-profiles-raw/`
> **我方产品**：XAUE 质押信用卡——质押 XAUE（锚定 XAUT 的链上黄金代币）入 Collateral Vault，LTV 60% 授信，RedotPay 发卡；三条新需求 = cashback 年消费阶梯返现 / XAUT staking reward 独立 APY / 出金白名单 + 入金 AML。

---

## 本档案与横向简报的分工（先看这个）

| | 横向简报（已交付） | 本纵向档案（本文件） |
|---|---|---|
| 视角 | 跨竞品横切：Strong/Weak 矩阵 + Differentiate/Parity 决策 | 单竞品纵深：一家一份，从**页面文案逐字**深挖 |
| 核心产出 | "该抄谁、该差异化" 的决策表 | **从交互与公开约定反推后端实现路径**（mentor 点名功课） |
| 证据粒度 | 结论级（带来源链接） | **原文逐字级**（带 raw 文件名，供取证/复核） |

> 本档案的价值锚点 = 每家末尾的 **【实现路径反推】**：把"页面上看得到的交互/约定"翻译成"后端大概率长这样"，直接喂给 PRD §5/§7/§8/§11/§13 修订与原型设计。

**5 家竞品在三条需求上的对标定位**（决定每家哪块写实、哪块标「不适用」）：

| 竞品 | Cashback | XAUT/资产收益 | 出金白名单/AML | 对标价值 |
|---|---|---|---|---|
| **Nexo Card** | ✅ 核心 | ✅ 核心（且有信用卡质押品计息的独家线索） | ✅ 核心（多段冷静期状态机） | **头号对标**——唯一三条全覆盖的同模式竞品 |
| **Crypto.com** | ✅ 核心（即时发放模型，与 Nexo 相反） | ✅ 质押换等级 + 解押状态机 | ✅ 核心（24h lock 非对称设计） | 结算模型与冷静期的反面参照 |
| **Kraken** | ❌ 不适用 | ✅ 核心（XAUT Auto-Earn + ledger 记账） | ✅ 核心（GSL 全局锁 + 地址生命周期） | 收益记账 + 敏感操作锁的最佳范本 |
| **Binance** | ❌ 不适用 | ❌ 不适用 | ✅ 核心（白名单 + 提现风控状态机全景） | 出金风控 + Travel Rule 的实现全景 |
| **Falcon Finance** | ❌ 不适用 | ✅ 核心（XAUT vault，但为合规反面教材） | ❌ 不适用（DeFi 无中心化出金） | 收益宣传的合规红线（怎么写会翻车） |

---
---

# 档案一：Nexo Card

**URL**: https://nexo.com/card ｜ **模式**：抵押借贷 + 信用卡双模式（与 XAUE 同模式，**头号对标**）
**公司层基本面**（复用《竞品档案-Nexo-CryptoCom-2026-07-22.md》，不重写）：2018 年运营，瑞士 Zug，AUM $7B+，Mastercard/DiPocket 发卡，仅 EEA+UK，Trustpilot ≈4.4/5，MiCA CASP 申请中。
**Raw**：`competitor-profiles-raw/nexo-card/2026-07-22/scrapes/`（31 文件）

## 1 · Cashback

### 1.1 档位/比例/cap 与呈现方式

**四档表**（`[support-card-crypto-rewards.md]`，营销主页 FAQ 逐字重复 `[nexo-card-主页.md]`）：

| Tier | NEXO 返现 | BTC 返现 | 月度 cap |
|---|---|---|---|
| Platinum | 2% | 0.5% | $200 |
| Gold | 1% | 0.3% | $150 |
| Silver | 0.7% | 0.2% | $100 |
| Base | 0.5% | 0.1% | $50 |

- **返现资产**：NEXO Token 或 BTC 二选一，"the option to change your cashback currency at any time"，切换 "applied instantaneously"（`[support-card-crypto-rewards.md]`）。
- **呈现三层**（对我方文档架构直接可抄）：营销页只给 "get **up to** 2% cashback" 锚点数字 → 页面底 FAQ 给完整四档表 → help 专文给四档表 + cap 表 + 改币种步骤 + ineligible MCC 跳转（`[nexo-card-主页.md]` + `[support-card-crypto-rewards.md]`）。
- **最小返现门槛**："The minimum crypto cashback payout is $0.01. Amounts less than $0.01 cannot be processed."（`[support-card-crypto-rewards.md]`）。
- **tier 由 NEXO 持仓占比决定**（非余额档）：公式 `[NEXO Value / (Total Portfolio - NEXO Value)] × 100`，Base 0% / Silver ≥1% / Gold ≥5% / Platinum ≥10%（`[support-loyalty-program-explained.md]`）；叠加绝对门槛：组合须 ≥$5,000 才进 Loyalty（否则无返现无计息），掉线给 7 天宽限（`[support-loyalty-program-explained.md]`）。

### 1.2 何时入账（结算约定，逐字——本节最关键）

> 🔑 **返现只在卡交易 completed 后发放**（settle 触发，非授权即发）："This principle also applies to crypto cashback on purchases, which is **credited only after the card transaction is completed**."（`[support-card-credit-mode.md]`）

pending 期全链条（`[support-card-credit-mode.md]`）：
- "the merchant has **up to 45 days to claim the funds**, though this process usually takes only a few days."
- pending 期间 "will not be added to your Outstanding Loan"、"the amount will not be available for repayment yet"。
- pending 期间**照计利息**，completed 时一次性结转："The loan generated from the purchase will accrue interest while the purchase is pending. The total accrued interest will be added to your Outstanding Loan once the purchase is completed."
- **天然免 clawback**："If the merchant does not claim the funds within the 45-day period, the transaction will be **rejected automatically**, and no loan interest will be charged."
- pending 会阻塞切币种："If you have any pending purchases... the option to switch your crypto cashback preference will be unavailable until they are completed."

**未收到返现的 4 个原因**（反推发放前的 gate）：组合 <$5,000 / 返现 <$0.01 / MCC ineligible / 已达当月 tier cap（`[support-card-credit-mode.md]`）。

### 1.3 以什么资产返、账单里怎么展示

- 返现资产见 1.1（NEXO/BTC）。
- **账单展示**：raw 无"返现如何在账单条目里展示"的专门文案（缺口）；能确认返现 completed 后 credited 到账户（`[support-card-credit-mode.md]`）。交易本身展示三行：Card Currency / Local Currency / Credit Charge（`[support-card-credit-mode.md]`）。
- 只有 **Credit Mode 有返现**，Debit Mode 无（`[support-card-crypto-rewards.md]`）；EEA 的 USDT/USDP/DAI/PAXG swap 也无返现（`[support-confirmations-processing-time.md]`）。

### 1.4 ineligible MCC 文案

完整清单（`[support-cashback-ineligible-mcc.md]`）：4829 Money Transfer / 6011 ATM / 6012 Investment Platforms / 6211 Brokers（明确点名 cryptocurrency）/ 6513 Real Estate / 6532 / 6540 / 7995 Gambling / 8999 Professional Services（点名"Charging your PayPal or Curve account"）/ 9399 / 9402。另有**个别商户级排除**（不只按 MCC）："Some individual merchants are ineligible"；UK 居民全域无返现。

## 2 · XAUT / 资产收益

### 2.1 APY 怎么标

> 🔑 XAUT 官方口径 = **"up to 6.25%"**（上限式，非固定、非区间；SSR 硬编码 H1 实锤）："Buy Tether Gold and earn **up to 6.25%** annual interest."（`[earn-xaut-买币页-SSR提取.md]`）

- Earn 收益页是 asset-agnostic 模板，H1 "Earn up to [空] interest on your [空]"，数字由前端 JS 从 API 注入（`[earn-xaut-收益页-模板文案与免责.md]`）。
- **PAXG 无独立 APY 数字**，且两处均为受限语境：EEA "USDT, USDP, DAI, and PAXG pairs are in **sell-only mode**"（`[support-confirmations-processing-time.md]`）。
- 第三方交叉（低置信）：Bitcompare "XAUT... 5.50% APY on Nexo"；推测 6.25% = Platinum + 收 NEXO +2% bonus 顶配，5.5% ≈ in-kind 口径（`[earn-xaut-收益页-模板文案与免责.md]`）。

### 2.2 计息与发放周期

- **Flexible Savings**：日复利日发放，"Daily compound interest with no lock-ups"、"start accruing... automatically **after 24 hours**"（起息延迟 24h）；利息 "automatically paid into your Savings Wallet so that the next day you begin earning interest on it too"（`[earn-xaut-收益页-模板文案与免责.md]` + `[earn-利息计息与发放-nexohow镜像.md]`）。
- **Fixed-term**：到期一次性全额发放，"paid out in full at the term's end"（`[earn-xaut-收益页-模板文案与免责.md]`）。
- 🔑 **发放币种决定复利性质**：in-kind = 复利；收 NEXO Token = "**not compounding but a simple one**"（单利）但换 "up to 2% bonus on interest"（`[earn-利息计息与发放-nexohow镜像.md]`）。

### 2.3 合规免责文案（逐字，行业最稳范本）

- 全站页脚标准句："**Rates are subject to change and may vary by region, loyalty tier, and other applicable factors.**"（`[earn-xaut-收益页-模板文案与免责.md]`、`[nexo-card-主页.md]`、`[nexo-borrow-信用额度页.md]`）。
- "up to/from" 专门免责："When terms such as 'up to' or 'from' are used to denote limits, achieving these maximum or minimum thresholds may be conditional on additional actions..."（`[earn-xaut-收益页-模板文案与免责.md]`）。
- 辖区限制："The Nexo Earn Interest Product is not available for citizens or residents of certain jurisdictions... such as Bulgaria, Estonia and the USA."（`[earn-利息计息与发放-nexohow镜像.md]`）。
- 🔑 **MiCA 产品拆分声明**（合规架构关键）："Custody, trading and futures are provided by Tangany and DLT Finance under their MiCA and MiFID authorisations. **Earn rewards and crypto-backed loans are separate products, offered under their own terms and outside the scope of those authorisations.**"（`[earn-xaut-买币页-SSR提取.md]`）——把 Earn/loan 明确划到牌照范围外另立条款。

### 2.4 ⚠️ 信用卡质押品是否计息（对 XAUE 需求 6 直接对标——双轨矛盾，重点）

Nexo 对"抵押中的资产是否计息"给出**两套自相矛盾的口径**，raw 不足以下无歧义结论：

- **营销/FAQ 层：说抵押的 NEXO 计息**——Loyalty 页 FAQ "Do collateral NEXO Tokens earn interest? A: **Yes**, if account balance exceeds $5,000."（`[nexo-loyalty-会员页.md]`）。
- **机制层：只有留在 Savings Wallet（未作抵押）的资产计息，划入 Credit Wallet 作抵押的部分不计息**——
  - "These assets continue to earn... interest per year... **until they are spent on a purchase**."（`[support-card-benefits.md]`）
  - "You'll also be earning interest on **assets not used as collateral**."（`[nexo-card-主页.md]`）
  - 消费时 "automatically transfer enough assets from your Savings Wallet to your Credit Wallet to serve as collateral"（`[support-card-credit-mode.md]`）——一旦划入 Credit Wallet 就脱离"在 Savings Wallet 才计息"的条件。

> 🤔 **对 XAUE 的读法**：Nexo 的 collateral 计息只对 **NEXO Token 这一特例**成立（因 NEXO 是 tier 计算核心 + 平台币激励），并非泛指所有抵押品。这恰好印证了我方需求 6 的定位——**给信用卡质押品单设独立 APY 在行业是空位**（Nexo 对普通抵押品的答案是"不计息"）。若要引 Nexo 佐证，必须写清"仅其自家平台币例外"，别误引成"Nexo 给所有质押品计息"。

## 3 · 出金白名单 / AML

### 3.1 登记交互（address book vs whitelisting 两层，逐字）

- **两层区分**（`[whitelist-addressbook支持文章全文.md]`）：Address book = 通讯录，本身不限提币、无数量上限（"There's no limit to the number of addresses"）；Whitelisting = "limiting withdrawals only to wallets listed in your Address book"，开启后只能提到簿内地址，**全组合级**（"applies to the entire portfolio and all supported crypto assets"）。
- **加地址随 Whitelisting 状态分两条路径**（`[whitelist-addressbook支持文章全文.md]`）：
  - 关闭时 / 或激活后 4h cool-off 窗口内：填地址 → Save → 2FA（即时生效）。
  - 开启时：填地址 → Save → 2FA → **收 "Address whitelisting request email" → 邮件点 "Add to whitelist"** → 新地址 "**greyed out** and become available for withdrawals only after the Extra Stability period passes"。
- **地址不可编辑只能删重建**（"you cannot edit the address itself... remove the current one and add a new"）；Whitelisting 开启时 Memo/Tag 也不可改（`[whitelist-addressbook支持文章全文.md]`）。

### 3.2 冷静期 / 安全验证（逐字——反推状态机的关键）

> 🔑 **Whitelisting 多段冷静期**（`[whitelist-addressbook支持文章全文.md]`）：Extra Stability Level 可选 **None / 24h / 72h / Custom（5 小时到 10,000 小时）**；激活后有 "**4-hour cool-off window**" 可自由加地址或关白名单；过后 "you have to wait for the preset Extra Stability Level delay to pass before any changes... take effect"。关白名单要 "**within 10 minutes**" 邮件确认否则过期；若过了 4h cool-off，关白名单显示 "Turning off" 直到 Extra Stability 倒计时结束，期间白名单仍生效。

- **提币 24h cool-off**（账户安全变更触发，`[support-withdraw-crypto.md]`）：触发条件二 = 改 email/phone/password/2FA，"begins when the respective change is finalized. Subsequent changes... will **reset its duration**"；期间 "Nexo Card purchases and FIATx withdrawals will also be unavailable"；目的原话点破意图——"provides a delay in situations where your Nexo account may be compromised... help clients avoid rushed or emotionally driven decisions..."。
- **提币 2FA + 邮件双确认 + 30 分钟 TTL**："Withdrawal confirmation emails are valid for **30 minutes**. Your withdrawal will be rejected automatically if you do not confirm..."（`[support-withdraw-crypto.md]`）。

### 3.3 入金未到账 / 确认数 / 命中风险提示（逐字）

- 🔑 **两段式 push 通知**（同一措辞在 3 文件反复）："When your incoming crypto transfer is **detected on-chain**, you'll receive a push notification letting you know it's on its way. At this stage, the funds aren't yet available... Once confirmation is complete, you'll receive a **separate notification** letting you know your top-up has been credited..."（`[support-top-up-crypto.md]`、`[support-confirmations-processing-time.md]`、`[support-missing-crypto-topups.md]`）。
- **确认数配置**：XAUT 属 ERC-20，走 ETH "**Finalized**"（≈70 confirmations ≈25min）（`[support-confirmations-processing-time.md]`）。（注：nexohow 镜像旧值 BTC 3/ETH 10 与官方全表 BTC 4/ETH Finalized 不一致，以官方 support 全表为准 `[help-入金确认数-nexohow镜像.md]`。）
- **命中风险/合规审查延迟提示**："Due to the processes that ensure Nexo's adherence to the applicable regulatory standards, your transfer may be **delayed for up to 24 hours**."（`[support-withdraw-crypto.md]`）；loan 侧 "due to KYC, AML... up to 1 business day"（`[support-loans-faq.md]`）。
- **入金未到账 7 类原因**含一条直接对标我方归集架构的："转到了 **withdrawal wallet（hot wallet）** 而非 top-up 地址... it will be one of our hot wallets servicing withdrawals"，需人工介入 + 可能录视频验证（`[support-missing-crypto-topups.md]`）。
- **抵押中资产不能直接提**："Assets cannot be withdrawn... while they are located in the Credit Wallet. You must first transfer them to the Savings Wallet..."（`[support-withdraw-crypto.md]`）。

## 4 · 【实现路径反推】Nexo

| 观察到的交互/约定（逐字来源） | 反推的后端实现 |
|---|---|
| 返现 "credited only after the card transaction is **completed**" + pending 期不入 Outstanding Loan `[support-card-credit-mode.md]` | 卡交易有 **pending→Approved/completed 状态机**；返现挂在 completed 事件上的**批处理 job**，非授权时刻发放 |
| merchant "up to **45 days** to claim"，不 claim 则 "rejected automatically" `[support-card-credit-mode.md]` | 每笔 pending 授权带 **45 天 TTL + 过期扫描 job**；这是"只认清算终态天然免 clawback"的实现基础 |
| pending 期计息，completed 时 "total accrued interest... added to Outstanding Loan" `[support-card-credit-mode.md]` | pending 授权有**影子计息**，settle 事件触发利息合并入本金 |
| 贷款利息 "added... daily at **00:00 UTC**"，"minimum... 0.01 xUSD" `[support-loans-faq.md]` | 全平台统一时点的**每日定时计息 job**，复利靠把利息并入本金实现 |
| 两段式 push（detected on-chain → credited）`[support-top-up-crypto.md]` | **链上/mempool 监听器**（触发第一条通知）+ **确认数计数器达标入账队列**（触发第二条 + 记账） |
| ERC-20 走 "Finalized"，每币种独立确认阈值全表 `[support-confirmations-processing-time.md]` | asset×network → required_confirmations **配置表**驱动入账门限；ETH 按区块 finality 而非固定确认数 |
| Automatic Collateral Transfer 默认开 + 分级回补（Flexible → 取消 Limit Order → Fixed-term unlock）`[support-automatic-collateral-transfer.md]` | LTV 监控触发器 → **分级资产回补引擎**（多来源优先级串联） |
| margin call 邮件档 71.4/74.1/76.9%，自动还款档 83.33%，oracle 驱动、卖最小量 `[support-loan-repayments-margin-call.md]` | oracle 喂价 → LTV 计算 → **多档阈值状态机**（通知档 vs 强平档）→ 卖最小量抵押的自动还款执行器（经交易所 API 市价单，0.26% flat 费预算入报价） |
| Whitelisting：4h cool-off → Extra Stability(None/24h/72h/custom 5–10,000h) → 新地址 greyed out；关白名单 10 分钟邮件确认 `[whitelist-addressbook支持文章全文.md]` | 🔑 **地址 pending→active 生命周期 + 每操作独立倒计时 + 邮件确认 token（10 分钟 TTL）**——最强状态机信号 |
| 提币确认邮件 30 分钟 TTL、未确认 auto-reject `[support-withdraw-crypto.md]` | 提币请求进 pending，带 **30 分钟过期定时器 + 邮件确认回调** |
| 安全变更触发 24h cool-off，期间冻结卡/FIATx 提现，"再变更 reset duration" `[support-withdraw-crypto.md]` | credential/2FA 变更事件触发**可重置的 24h 安全窗口状态**，窗口内对提现/卡消费加限制 |
| 转错到 hot wallet 需人工介入 + 录视频 `[support-missing-crypto-topups.md]` | 归集/热钱包地址与充值地址分离；错转进**人工 recovery SOP 队列**（含身份复核） |
| xUSD 汇率 "obtained from fixer.io **at the time of purchase** and remains the same regardless of when merchant completes" `[support-card-credit-mode.md]` | 授权时刻锁定折算率并落库，settle 沿用授权时汇率（不 settle 时重新取价） |

## 5 · 【对 XAUE 的启示】Nexo

1. **cashback 结算模型直接对齐**：Nexo "completed 后发放 + 45 天 claim 窗口 auto-reject" = 我方 B-3/B-4「递延出账 + 只认已清算流水」的最贴切同构物，且共享同一"天然免 clawback"红利。我方按开卡年递延到账单出账更进一步（Nexo 是逐笔 completed 触发），但底层"只认终态"逻辑一致，**放心落地**。
2. **收益免责文案照抄 Nexo**：全站页脚三句（"subject to change / may vary by region, loyalty tier" + "up to/from" 专项免责 + "not... guarantee of future results"）是行业最稳范本，直接移植到 staking reward 展示页。
3. **MiCA 产品拆分声明值得学**：Earn/loan 明确划到牌照范围外另立条款——我方 staking reward 作为"独立奖励"若涉证券属性顾虑，可借鉴这种"独立产品、独立条款、独立牌照边界"的切割写法。
4. **信用卡质押品计息 = 差异化被反向印证**：Nexo 普通抵押品不计息（仅自家 NEXO 例外），我方需求 6 的"独立 APY"是真空位；引 Nexo 佐证时务必写清"仅其平台币例外"。
5. **白名单冷静期抄 Nexo 的可配 Extra Stability**：None/24h/72h/Custom 四档 + 4h cool-off 缓冲窗 + 关白名单 10 分钟邮件确认，是比 Coinbase 固定 48h 更灵活的模型，直接补我方 D-4 待定项。
6. **两段式 push + 确认数配置表**直接进我方 §13 通知 + §11 钱包架构（入账成功通知的前置增强 + asset×network 确认阈值表）。

---
---

# 档案二：Crypto.com Visa Card

**URL**: https://crypto.com/cards ｜ **模式**：预付卡 + CRO 锁仓换等级（**结算模型与 Nexo 相反的关键参照**）
**公司层基本面**（复用既有档案）：2016 年创立，新加坡，1.4 亿注册用户，持 EU MiCA/UK EMI/MAS MPI/Dubai VARA，Trustpilot 1.6/5（费用不透明是头号差评）。
**Raw**：`competitor-profiles-raw/crypto-com-card/2026-07-22/scrapes/`（26 文件）

## 1 · Cashback / CRO rewards

### 1.1 层级/比例/cap（Level Up 新制 + 旧等级，逐字）

**Level Up（2024-11-06 起）有 active lockup 时的返现**（分 Year 1 / thereafter，2025-09-02 生效，`[help-prepaid-card-rewards与降级规则.md]`）：

| Plan（旧名） | CRO 返现 | 月消费 cap |
|---|---|---|
| Plus (Ruby) | 2% Year 1; 2% thereafter | $1,250 |
| Pro (Jade/Indigo) | 3.5% Year 1; 3% thereafter | $2,500 |
| Private-$50K (Icy/Rose) | 5% Year 1; 4% thereafter | Unlimited |
| Private-$500K (Obsidian) | 6.5% Year 1; 5% thereafter | Unlimited |

- **旧等级营销页**（`[cashback-us-prepaidcard-营销页-层级与入账文案.md]`）：Midnight 0% / Ruby 2%(cap $25) / Indigo 3%(cap $75) / Rose 4%(无限) / Obsidian 5%(无限)，锁仓门槛 $500→$500,000。
- **可选返 BTC**（`[cashback-help-BTC-rewards与refund-clawback.md]`）：Plus 1%(cap $1,250) / Pro 1.5%(cap $2,500) / Private 2%(无限) / Obsidian&Prime 3%(无限)；🔑 **cap 跨币种共享**："The monthly spending cap is **shared between BTC and CRO rewards**, and resets at **00:00:00 UTC on the first day of each calendar month**."

### 1.2 何时入账（结算约定，逐字——与 Nexo 相反）

> 🔑 **挂钩 purchase/transaction 而非 settlement，即时发放**：
> - help："Spending rewards will be credited to your Token Wallet **shortly after you make an eligible purchase**."（`[cashback-help-rewards入账时点-补充抓取.md]`）
> - 营销页："Spending rewards are paid in CRO and deposited into your Crypto Wallet... **after every eligible transaction**."（`[cashback-us-prepaidcard-营销页-层级与入账文案.md]`）

- ⚠️ **关键缺口 + 关键发现**：用户预设的 "up to two billing cycles" / "confirmed" / "posted" / "settled" / "pending" **在 26 个文件中全部未出现**；且卡侧官方明确 "**no billing cycle, no revolving credit limit**"（`[help-USD卡拒付原因与debit-credit定性.md]`）。→ **Crypto.com 走的是"授权即发 + 事后 clawback 冲回"模型，不是异步账单确认模型**，与 Nexo/我方相反，是反面参照而非对标先例。
- 订阅返还固定月初发放："credited to your Crypto Wallet **on the first day of each month**" + "maximum one reimbursement per merchant monthly"（`[cashback-us-prepaidcard-营销页-层级与入账文案.md]`）。

### 1.3 退款 clawback（逐字）

> 🔑 **按原返 token 数量扣回**（token 维度记账）："If a transaction is reversed or refunded, the corresponding reward will be **deducted in the same token you originally received**."（`[cashback-help-BTC-rewards与refund-clawback.md]`）
> 官方示例："June 返 CRO、7 月切 BTC，退款仍扣 CRO"——追溯原始发放批次的币种。

- 缺口：用户想找的 "negative CRO balance" / "initial CRO amount" **未出现**；实际口径是"按原返币种扣回"，非允许负余额或按初始法币数量。

### 1.4 excluded MCC / 受限市场（逐字）

- MCC 黑名单（`[cashback-help-excluded-MCC与受限市场.md]`）：4829 / 4900 Utilities / 5734 Software / 5947 / 6012 / 6051 / 6211 / 6300 Insurance / 6513 / 6540 / 9222 / 9311。自留裁量："Crypto.com **reserves its right to exclude any other merchant categories or channels**... at its sole discretion."
- 受限市场：40+ 国家，按卡版本（US/CA/EU/SG/BR/AU/BH）二维差异化。

### 1.5 怎么展示/发放

- 入账目标：help 说 "Token Wallet"、营销页说 "Crypto Wallet in the Crypto.com App"（`[cashback-help-rewards入账时点-补充抓取.md]`）。
- 缺口："未见 transaction history 内 rewards 展示样式的具体描述"。

## 2 · CRO staking / 资产收益 + 解押

### 2.1 锁仓换等级 + 锁仓期

- **365 天 holding period**，升级重置："365-day holding period will **restart upon upgrading**"（`[help-level-up-加入规则.md]`）；只能在 365 天期满后降级（`[help-level-up-加入规则.md]`）。
- 升级按**补差额**（非全额重锁）："lock up... the difference between... current... and... required"（`[help-prepaid-card-rewards与降级规则.md]`）。
- tier **按锁入时点 USD 档位定档、不随市值波动降级**（官方未见按市值降级条款，`[help-level-up-加入规则.md]`）；降级只由主动 unstake / 订阅到期触发。

### 2.2 unstake / unbonding（逐字——三条线按产品分叉，别混用）

| 产品线 | unbonding 周期 | 来源 |
|---|---|---|
| Cardholder CRO Staking（卡质押 legacy） | **36 天**（"imposed by the underlying protocol"） | `[help-新USD卡与BRL卡FAQ.md]` |
| Level Up Program | **36 天**（"estimated unbonding period of 36 days"） | `[help-instant-unstaking与CRO-unbonding汇总.md]` |
| CRO Earn / Onchain（Cronos POS） | **28 天**（"enforced by the Cronos POS Chain"） | `[help-CRO-onchain-staking与soft-lockup.md]` |

- **unbonding 期间无收益**："During and after the unbonding period, your CROs will not be eligible for any... rewards."（`[help-instant-unstaking与CRO-unbonding汇总.md]`）。
- **Instant Unstaking 不含 CRO**：覆盖 29 个 POS token（每 token 日额度 quota），但 "does not mention CRO or Level Up staking products as eligible"（`[help-instant-unstaking与CRO-unbonding汇总.md]`）。
- **Soft Lockup 日快照计息**："Daily rewards calculated using snapshots at **22:00:00 UTC** each day, distributed within an hour after **00:00:00 UTC**"（`[help-CRO-onchain-staking与soft-lockup.md]`）；Exchange CRO Rewards "receive rewards at **01:00 UTC daily**"（`[help-CRO-rewards-exchange.md]`）。

## 3 · 出金白名单 / AML

### 3.1 登记交互（逐字）

- Exchange 路径：profile → Wallet → Deposit & withdrawal → Withdrawal whitelist → Add withdrawal address；地址类型 **Standard（单 token）/ Universal（多 token）**，字段 Coin/Network/Address/Wallet source/Label；"Enter your OTP and 2FA code... then click Confirm"（`[whitelist-help-whitelisting-Exchange交互.md]`）。
- 🔑 **价值主张话术**（可直接借鉴的用户教育文案）："Whitelisting protects your funds by only allowing transfers to approved wallets. **Even if a malicious actor gains access to your account, they cannot withdraw your assets to a non-whitelisted address.**"（`[whitelist-help-whitelisting-Exchange交互.md]`）。

### 3.2 24h withdrawal lock（逐字——非对称设计是重点）

- 定义："disabling withdrawals for **24 hours** to addresses that have been **newly-whitelisted**"（`[whitelist-help-24h-lock-新增地址-Exchange.md]`）。
- **新增地址触发**（多文件印证）："There's a mandatory 24-hour withdrawal lock on newly added addresses if that feature is enabled."（`[help-crypto-withdrawal-总说明.md]`）。
- ⚠️ **关闭非对称**：开启即时确认无额外验证；**关闭要重验证 + 24h 延迟生效**——"Toggle off → 输 SMS OTP + authenticator 2FA → 'Yes, I am sure'"，且 "**Important: The change will take effect in 24 hours.**"（`[whitelist-help-24h-lock-新增地址-Exchange.md]`）。
- **关闭的威慑代价**："you will **not be eligible for the Account Protection Programme** (if applicable) while this feature is disabled"（`[whitelist-help-24h-withdrawal-lock-开关交互.md]`）。

### 3.3 提币复审 / 拒付 / 限额（逐字）

- 提币处理 "almost instant... no longer than **2-3 hours**"；24h 滚动上限 "BTC 10 (or equivalent)"（搜索摘要，`[help-crypto-withdrawal-总说明.md]`）。
- USD 卡拒付：只查 USD 余额 "**no mention of automatic crypto-to-USD conversion**"；卡定性 "prepaid card that operates in credit mode only"、"**no billing cycle, no revolving credit limit, and no option to pay in installments**"（`[help-USD卡拒付原因与debit-credit定性.md]`）。

## 4 · 【实现路径反推】Crypto.com

| 观察（逐字来源） | 反推的后端实现 |
|---|---|
| 返现 "shortly after you make an eligible **purchase**" / "after every eligible **transaction**"，卡侧 "no billing cycle" `[cashback-help-rewards入账时点-补充抓取.md]` `[help-USD卡拒付原因与debit-credit定性.md]` | 返现在**授权(auth)事件即触发发放**，非 T+N 清算；对账/退款靠独立 clawback 补偿链路（与 Nexo 相反） |
| clawback "deducted in the **same token** you originally received"，June 返 CRO/7 月切 BTC 仍扣 CRO `[cashback-help-BTC-rewards与refund-clawback.md]` | reward ledger 按**发放批次记录 token+数量**；退款 job 追溯原始批次逐笔按币种冲回（token 维度记账，非法币折算） |
| cap "shared between BTC and CRO"，"resets at 00:00:00 UTC on the first day" `[cashback-help-BTC-rewards与refund-clawback.md]` | 统一**月度消费额度计数器**（USD equivalent），双返现路径共用同一 counter + UTC 月初 reset job |
| "3.5% Year 1; 3% thereafter"、legacy "up till September 9, 2026" `[help-prepaid-card-rewards与降级规则.md]` `[help-level-up-权益与legacy利率.md]` | 费率表带**加入日期维度 + effective-dated 分段配置**，到期自动切换（时间窗口状态机） |
| 365 天 holding period "restart upon upgrading" `[help-level-up-加入规则.md]` | tier 记录**holding period 起算时间戳**，升级重置；降级门禁校验 now ≥ 起算+365d |
| tier 不随市值降级、只由 unstake/退订触发 `[help-level-up-加入规则.md]` | tier 判定以**锁仓时刻 USD 快照**固化入库不重估；无价格监控降级 job |
| unbonding 36/36/28 天按产品分叉、跟随链上 `[help-instant-unstaking与CRO-unbonding汇总.md]` | 解押锁定状态机时长**按 product 类型配置**，直接跟随链上 unbonding；有独立 "unbonding" 中间态（计息停但资产未可取） |
| 新增白名单地址 24h lock `[whitelist-help-24h-lock-新增地址-Exchange.md]` | 地址表带 `whitelisted_at` + 提现前校验 now ≥ whitelisted_at+24h 的**风控延迟闸门** |
| 🔑 关闭 lock 也延迟 24h 生效 + 强验证 `[whitelist-help-24h-lock-新增地址-Exchange.md]` | 安全降级操作走**延迟生效 + 强验证双闸门**；后端需"pending security-setting change"延迟执行队列——攻击者夺号后关保护也要等 24h |
| 关闭 lock 联动 Account Protection 资格 `[whitelist-help-24h-withdrawal-lock-开关交互.md]` | 保障计划资格与安全开关**实时联动**（feature flag 影响理赔判定） |
| crypto 充值 "rate held 15 seconds" 后成交入法币卡 `[help-top-up-US.md]` | 前端锁价 + 后端成交的**15 秒短时对冲窗口**；卡余额纯法币记账 |
| 费率/限额/受限市场按 region + 卡版本二维拆分 `[help-费用与限额-US.md]` `[cashback-help-excluded-MCC与受限市场.md]` | 多 region 配置表（region key 索引）+ MCC 黑名单表（授权时匹配决定是否计返，可动态增补） |

## 5 · 【对 XAUE 的启示】Crypto.com

1. **结算模型：Crypto.com 是"别这么做"的参照**——授权即发返现导致必须建复杂 clawback 冲回链路（退款按原 token 追溯）。我方递延到账单出账、只认已清算流水，**天然规避这套复杂度**，本档案再次确认维持设计。
2. **但 token 维度 clawback 记账值得记**：即便我方免了 clawback，若未来 staking reward 涉及回撤，Crypto.com "按发放批次记录币种+数量、追溯冲回"是成熟的账本设计参考。
3. 🔑 **白名单"关闭也延迟生效"必须抄**：这是 Crypto.com 比 Nexo 更严的一点——攻击者夺号后关掉白名单保护也要等 24h，堵住 disable→instant-withdraw 攻击窗口。我方 D-4 应把"关闭/删除白名单对称冷静期"写进去。
4. **cap 跨币种共享 + UTC 月初 reset** 是干净的额度计数器设计；我方 cashback 虽是年 cap，但"统一 counter + 定时 reset job"的模式可复用到年度 cap 归零逻辑。
5. **effective-dated 费率表**（Year 1 vs thereafter、法币切换日）提示我方：cashback 档位比例若未来调整，配置应带生效日期分段，别硬编码。
6. **unbonding 独立中间态**（计息停、资产未可取）提示我方 staking reward：若质押品有解押流程，需设"解押中不计收益"的状态，别让解押期继续累积 reward。

---
---

# 档案三：Kraken（XAUT Auto-Earn）

**URL**: https://www.kraken.com/features/auto-earn/tether-gold ｜ **模式**：中心化交易所 Earn + 出金安全（**收益记账 + 敏感操作锁的最佳范本**）
**Raw**：`competitor-profiles-raw/kraken-xaut-earn/2026-07-22/scrapes/`（8 文件）

## 1 · Cashback

**不适用**。8 文件无任何信用卡返现产品——Kraken 无信用卡产品线（`[全部 8 文件]`）。

## 2 · XAUT Auto-Earn / opt-in rewards（核心）

### 2.1 APY 怎么标（逐字）

> 🔑 **"up to 0.1% APY"**（上限式）：headline "Earn **up to 0.1% APY** on Tether Gold (XAUT)"（`[kraken-features-auto-earn-tether-gold.md]`）。

- support 程序页用 **APR** 且是示例值："Flexible... Other Assets（含 XAUT）**0.10%**"，"Each asset will earn rewards at its own Annual Percentage Rate (APR)... **Rates are variable and subject to change**."（`[support-overview-of-opt-in-rewards.md]`）——APY/APR 两口径并存。
- 展示统一"抽佣前估计值"："The APYs shown are an estimate... **before our commission**."（`[support-overview-of-auto-earn.md]`）。
- 🔑 **结构事实**：XAUT 归 "**Other Assets**" 类，"仅通过 Auto Earn 参与，不支持手动 Fixed/Bonded allocation"——即 XAUT 不能选期限档（`[support-overview-of-opt-in-rewards.md]`）。

### 2.2 计息与发放（逐字）

- "Rewards **accrue daily** and all your earnings are **paid out on a weekly basis**."（`[kraken-features-auto-earn-tether-gold.md]`）；"Opt-In rewards are paid out **once per week**."（`[support-overview-of-opt-in-rewards.md]`）。
- **prorated 近实时起息**："start earning prorated rewards... **as soon as it is processed**（within minutes）"、"**no minimum allocation time**"（`[support-overview-of-opt-in-rewards.md]`）。
- **发放币种 in-kind（原币 XAUT）**："rewards will be paid out **in kind**"（`[support-overview-of-opt-in-rewards.md]`）；XAUT 无自动复投（走 Flexible 非 Fixed）。
- **地域**："**Not available in the US** and other geographic restrictions apply"；逐国：UK 不可用、US 除 Accredited Investor 外不可用、Australia 不可用、**EEA 只开放 BTC**（即 XAUT 在 EEA 也不可用）（`[support-opt-in-rewards-eligibility.md]`）。

### 2.3 合规免责（逐字，行业最稳之一）

- "**Reward rates are subject to change** and compliance with Kraken's terms and conditions."（`[kraken-features-auto-earn-tether-gold.md]`）。
- 🔑 全大写风险声明（XAUT 直接适用）："**KRAKEN IS NOT A BANK... YOUR KRAKEN ACCOUNT IS NOT A DEPOSIT ACCOUNT... NEITHER YOUR KRAKEN ACCOUNT NOR OPTED IN ASSETS ARE COVERED BY INSURANCE... (FDIC) OR (SIPC) PROTECTIONS...**"（`[support-overview-of-opt-in-rewards.md]`）。
- "We do **not guarantee** that you will earn any reward."（`[support-overview-of-auto-earn.md]`）。
- **opt-in 默认关闭**，各端 Settings 里逐程序 toggle（`[support-overview-of-auto-earn.md]`）。

## 3 · 出金白名单 / AML

### 3.1 登记交互 + 地址生命周期（逐字）

- **地址标签全账户唯一**（"you cannot use the same label even if it is for two different cryptocurrencies"）（`[support-adding-withdrawal-address-kraken-pro.md]`）。
- **邮件确认 1 小时 TTL**："The withdrawal approval link... valid for **one hour**... the link will expire."；邮件内含**账户锁定链接**（防攻击者加地址盗币）（`[support-adding-withdrawal-address-kraken-pro.md]`）。
- **受信设备豁免**："If you are adding the withdrawal address from a **trusted device**, your new address will be **approved automatically**."（`[support-adding-withdrawal-address-kraken-pro.md]`）。
- **改密码触发新地址 12h hold**："withdrawals to new withdrawal addresses will be **held for 12 hours**... addresses already added won't be affected."（`[support-adding-withdrawal-address-kraken-pro.md]`）。

### 3.2 Global Settings Lock（GSL，Kraken 特有，逐字——重点）

> 🔑 **GSL = 账户级安全锁全局状态机**："prevents changes being made to your Kraken account, **including by Kraken Support**"（连客服也改不了）；锁定清单含 "Adding withdrawal address (crypto)"（= allowlist-only 效果）、改密码/邮箱/2FA/API key 等（`[support-global-settings-lock.md]`）。
> **解锁等待期 "A minimum of 24 hours (or up to 30 days)"，天数由用户开启时设定**；Master Key 可即时解锁；"You will also be **notified via email of any attempted unlock**."（`[support-global-settings-lock.md]`）。

### 3.3 ledger 收益记账（逐字——对我方需求 7 最直接）

> 🔑 **rewards 走独立 "earn" type**：ledger 行字段 `txid, time, type, subtype, aclass, asset, amount, fee, balance`；type 取值含 "earn"，"relates to all onchain staking and **Opt-In Rewards** activities, including allocation, deallocation, **rewards**..."（`[support-ledger-history-fields.md]`）。
> earn 下 subtype 区分动作：**Allocation / Deallocation / Reward**（"payouts generated from... Opt-In Rewards programs. **Typically issued weekly or bi-weekly**"）+ autoallocate/migration 等（`[support-ledger-history-fields.md]`）。
> 估值口径："Earn rewards are considered **deposits and valued at the time of reward**."（`[support-ledger-history-fields.md]`）。

- **小额精度截断**："Rewards below **five decimal places** will only display on exported ledger history."（网页/App 截断）（`[support-overview-of-opt-in-rewards.md]`）。

## 4 · 【实现路径反推】Kraken

| 观察（逐字来源） | 反推的后端实现 |
|---|---|
| Opt-In Rewards 按国家逐一排除 + US Accredited 例外 + EEA 只 BTC `[support-opt-in-rewards-eligibility.md]` | per-user **地域 flag + 投资者资质 flag + asset×region 可用性矩阵**（EEA 只 BTC 说明是组合判定非单开关） |
| Auto Earn 默认关、三程序独立 toggle `[support-overview-of-auto-earn.md]` | 每用户每程序一个 **enrollment 状态位**；计息 job 只结算"已开启+资产>阈值+地域合规"的组合 |
| "accrue daily" + "paid out weekly" `[kraken-features-auto-earn-tether-gold.md]` | **两段式定时任务**：每日计提 accrual + 每周批量 payout，两者解耦 |
| "prorated... as soon as processed (within minutes)"、"no minimum allocation time" `[support-overview-of-opt-in-rewards.md]` | 计息按持仓时长比例、以 **allocation 事件时间戳**为起点，非整日批处理边界 |
| 🔑 ledger `type=earn` + subtype `reward/allocation/deallocation` `[support-ledger-history-fields.md]` | 后端把收益/划拨各设**独立 ledger entry type**，reward 是可单独查询/导出/计税的会计科目——**直接对标我方需求 7"记账流水形式"** |
| "valued at the time of reward" `[support-ledger-history-fields.md]` | 每笔 reward 入账时打**价格快照**存库（成本基础/计税用） |
| reward < 5 位小数只在导出显示 + "greater than smallest decimal precision" 才入账 `[support-overview-of-opt-in-rewards.md]` | **dust threshold**（最小可发放精度阈值），低于不入账；展示层与账本层精度分离 |
| Staking 抽 30%、Opt-In/Stablecoin 当前 0，均标 "before commission" `[support-overview-of-auto-earn.md]` | 佣金 **per-program 可配参数**；展示层在毛收益率上实时扣佣算净值 |
| "stake only a portion... remainder held for liquidity"、Flexible "up to 50%" `[support-overview-of-auto-earn.md]` | 维护"实际上链质押比例/储备比例"参数；收益按实际质押部分计（"up to 0.1%" 的 up to 可能源于此） |
| 🔑 GSL 一开则所有敏感变更冻结，解锁 24h–30 天可配，Master Key 即时解锁，解锁尝试即发邮件 `[support-global-settings-lock.md]` | **账户级安全锁全局状态机**：locked / unlock-requested(倒计时) / unlocked；锁定期拦截所有 sensitive mutation；倒计时时长开锁时持久化 |
| 新地址 pending(1h TTL)→confirmed/expired，受信设备跳过 pending `[support-adding-withdrawal-address-kraken-pro.md]` | 地址表带**状态字段 + TTL 过期 job + 设备信任标记**（trusted-device 表） |
| 改密码 → 仅新地址 hold 12h、老地址不受影响 `[support-adding-withdrawal-address-kraken-pro.md]` | 提现风控按"**地址新增时间 vs 最近敏感事件时间**"做差判定；hold 是加在 to-new-address 路径上的定时闸门，非全账户冻结 |

## 5 · 【对 XAUE 的启示】Kraken

1. 🔑 **收益记账直接照 Kraken 的 ledger 模型**——我方需求 7 要"记账流水形式提示收益更新"，Kraken 的 `type=earn` + subtype `reward` 是现成会计科目设计：新增独立"质押奖励"流水类型 + 每笔打价格快照 + 设 dust threshold（低于最小精度不入账，避免脏数据）。这是本档案对需求 7 最有价值的落地件。
2. **APY 文案抄 "up to X% + subject to change + before commission"**——比 Nexo 更进一步的是显式标注"抽佣前估计值"，若我方 staking reward 有平台抽成，这个口径纪律值得学。
3. **GSL 式全局安全锁是高净值黄金卡的加分项**——我方 D-4 白名单之外，可评估一个"账户级安全锁（锁定期内禁所有敏感变更、解锁需可配等待期）"，契合黄金卡稳健定位；连客服也改不了 + 解锁尝试即通知，是防社工的强设计。
4. **敏感事件定向 hold（改密码→仅新地址 hold 12h）**比 Nexo 的全账户 24h cool-off 更精细——只锁"新增于风险事件之后的地址"，老地址不受影响，用户体验更好，我方可借鉴这种"按地址新增时间 vs 事件时间做差"的定向闸门。
5. **asset×region 可用性矩阵**提示我方：staking reward 若分地区开放，别用单一开关，要做"资产×地区"组合判定（Kraken EEA 只 BTC 是先例）。

---
---

# 档案四：Binance（白名单 + 提现风控 + Travel Rule）

**URL**: binance.com（FAQ/Academy/DevDocs） ｜ **模式**：中心化交易所出金风控全景（**出金白名单 + AML 的实现全景最佳来源**）
**Raw**：`competitor-profiles-raw/binance-whitelist/2026-07-22/scrapes/`（9 文件）
**可信度**：`[文件名]` = 官方页（高）；`search-summary-*` = 搜索摘要（低，文中显式标注）。

## 1 · Cashback / 2 · 资产收益

**均不适用**。9 文件 100% 聚焦提币安全 + 入金合规，无信用卡返现、无黄金收益（`[全部 9 文件]`）。

## 3 · 出金白名单 / 入金 AML（核心）

### 3.1 白名单开关 + 200 上限（逐字）

- 开启后 "you can only withdraw to addresses on your whitelist"（`[binance-academy-withdrawal-address-whitelist.md]`）。
- 地址上限："up to **200 withdrawal addresses**"（双源确认，`[binance-academy-...]` + `[binance-faq-withdrawal-settings-whitelist.md]`）。
- Address Origin 三选项：Exchange / Wallet / Other（`[binance-faq-withdrawal-settings-whitelist.md]`）。

### 3.2 冷静期机制（逐字——两个概念要分清）

- **新增地址冷静期 = Whitelist Withdrawal Limit**："**suspends withdrawals to newly added whitelist addresses** for a selected time period. ... prevents unauthorized withdrawals if a hacker adds a new address."，可选 **24/48/72h 用户自选**（`[binance-faq-withdrawal-settings-whitelist.md]`）。
- **修改白名单触发暂停**："Modifying the withdrawal whitelist may lead to temporary withdrawal suspension for a specific time limit (**24, 48, or 72 hours**)."（`[binance-academy-...]`）。
- 🔑 **关闭白名单也触发同档暂停**（对称冷静期）："if you've enabled the withdrawal whitelist limit previously, your withdrawal will be **temporarily suspended for a certain time limit (24, 48, or 72 hours)**"（`[binance-faq-withdrawal-settings-whitelist.md]`）。

### 3.3 提现风控状态机（逐字）

- **Reactivate**：触发 = 改密码等安全操作 / 系统检测异常；分级 "Risk Level 1 → try again in **24 hours**"、"Level 2 → **48 hours**"、欺诈模式 → 不定长；🔑 **不可提前解锁**："the suspension **cannot be lifted before the suspension period ends**" + "automatically reactivated after the suspension period"（`[binance-faq-reactivate-withdrawal-function.md]`）。
- **Anti-Scam / Nova AI**：提币前实时评分 + probing questions，"risk... **low** 才放行"，high 引导 self-report；提币时检测"正在通话"→ in-app warning "Binance is not calling you"；反复向 blacklisted address → cool-down "up to a maximum of **24 hours**"（`[binance-faq-withdrawal-anti-scam-measures.md]`）。
- 🔑 **产品承诺**："**Binance never permanently freezes or blocks user funds**"（仅防诈场景，不覆盖 compliance/AML review）（`[binance-faq-withdrawal-anti-scam-measures.md]`）。
- **One-Step Withdrawal**：向白名单提小额免 2FA，但需设 quota（`[binance-academy-...]`）。

### 3.4 Travel Rule 问卷（逐字字段——动态表单）

- 覆盖 9 辖区；通用字段：`isAddressOwner`(1 自己/2 他人)、`bnfType`(个人/实体)、`sendTo`(**1 Wallet / 2 VASP**)、`vasp`、`vaspName`（vasp=others 时必填）（`[binance-devdocs-travel-rule-withdraw-questionnaire.md]`）。
- 区域特有：Japan/Kazakhstan 有 `txnPurpose` 枚举；NZ/EU/South Africa/Australia 需 `Declaration`(BOOLEAN)；Australia/EU 需 city/region（`[binance-devdocs-...]`）。
- 阈值（搜索摘要，低置信）：EU "deposits 超 €1,000 需 sender 详情；所有提币不论金额需 beneficiary details"；不提供 → "transactions may be delayed or canceled"（`[search-summary-travel-rule-and-deposit-aml.md]`）。

### 3.5 入金未到账 + 入金 AML

- **deposit-not-credited 官方页 5 类原因均为技术性、非 AML**：确认数不足（BTC ≥2）/ 不支持网络 / 地址或 MEMO 错 / 低于最小额 / 钱包维护（`[binance-faq-deposit-not-credited.md]`）。
- ⚠️ **入金 AML 冻结无官方正文**（官方 FAQ 明确不含）；以下全来自搜索摘要（低置信）："under review" = 人工 compliance 审查、可拖数月；链上分析 "if a deposit passed within a few hops of a mixer, darknet market, or hacked wallet, the score can **quarantine an entire account**"（`[search-summary-travel-rule-and-deposit-aml.md]`）。
- **无 deposit address retirement 素材**：9 文件全无（`[binance-faq-deposit-not-credited.md]`）——⚠️ 与横向简报引用的"Binance 地址退役先例"是不同来源，本批 raw 未覆盖，我方"废址换新"若需 Binance 佐证要另抓。

## 4 · 【实现路径反推】Binance

| 观察（逐字来源） | 反推的后端实现 |
|---|---|
| 新增白名单地址 24/48/72h 冷静期，"prevents... if a hacker adds a new address" `[binance-faq-withdrawal-settings-whitelist.md]` | **地址级风控延迟队列**：新地址进可配时长挂起窗口，窗口内提币被拒 |
| 🔑 关闭白名单也触发同档暂停（前提曾启用 limit）`[binance-faq-withdrawal-settings-whitelist.md]` | **对称冷静期**：状态机对"关闭白名单"这个动作本身施加等长冷却，堵 disable→instant-withdraw 攻击窗口 |
| 修改白名单（增/删/改）→ 提币暂停 `[binance-academy-...]` | 白名单是**受保护配置对象**，任何写操作触发账户级冷却 |
| suspended 后不可提前解锁、期满自动恢复、分 Risk Level `[binance-faq-reactivate-withdrawal-function.md]` | **账户风险状态机**：进 suspended 态只能靠计时器自然退出，无人工提前放行（申诉走另一条线） |
| Nova AI 实时评分 + probing questions，low 才放行 `[binance-faq-withdrawal-anti-scam-measures.md]` | 提币链路嵌**同步风控决策节点**，按分值分流（放行/追问/拦截）——KYT 式 |
| 检测"正在通话"→ in-app warning `[binance-faq-withdrawal-anti-scam-measures.md]` | **客户端环境信号采集**（设备侧行为）接入风控 |
| 多信号画像：profiles/patterns/devices/environmental `[binance-faq-withdrawal-anti-scam-measures.md]` | 聚合多源信号的**评分模型**（非单一规则） |
| Travel Rule 按辖区 + sendTo(Wallet/VASP) 动态渲染字段 `[binance-devdocs-...]` | **规则引擎驱动的动态合规表单**：counterparty+jurisdiction 分支决定字段集 |
| 金额阈值触发采集（入金 >€1,000 采集 sender；出金零门槛全采集）`[search-summary-...]`（低置信） | 金额阈值触发的合规采集卡点，入金/出金阈值不对称 |
| 入金链上评分命中 mixer/darknet 近邻 hop → quarantine 整账户 `[search-summary-...]`（低置信） | 入金侧 **KYT 链上溯源打分 + quarantine 挂起队列**；"under review" 人工态与自动锁并存 |
| 申诉入口二态（显示 Appeal 按钮 vs 只显示 countdown）`[search-summary-...]` | 前端按后端锁类型渲染 UI；后端对锁有**类型标记（自动 vs 需人工）** |

## 5 · 【对 XAUE 的启示】Binance

1. 🔑 **对称冷静期是白名单设计的必抄项**——Binance（关白名单同档暂停）+ Crypto.com（关 lock 延迟 24h）双证：**关闭/删除白名单必须与新增对称加冷却**，否则攻击者夺号后先关保护再提币。我方 D-4 白名单细则务必写死这条。
2. **提现风控状态机分层**给我方入金 AML 处置提供架构模板：自动锁（计时器自然退出、封顶 24h）vs 人工审查（可长、走申诉通道）两条线并存，前端按锁类型渲染不同 UI（有无 Appeal 按钮）。我方"命中禁归集 + 隔离 + 人工复核 + 申诉"应照此分两态实现。
3. **Travel Rule 动态表单**直接对标我方出金合规采集：按 jurisdiction + 收款方类型（个人钱包 vs VASP）动态渲染字段（`sendTo`/`isAddressOwner`/`bnfType`/`vasp`）——我方出金白名单登记 + 付款环节若涉 Travel Rule，这是现成字段模型。
4. **"永不永久冻结"是可借鉴的产品承诺，但要分场景**——Binance 只对防诈类承诺临时（封顶 24h），对 AML review 不承诺。我方对用户端文案应同样区分"风控临时限制"与"合规审查"，避免过度承诺。
5. ⚠️ **注意 raw 缺口**：入金 AML 全链条 + 地址退役在本批 Binance raw 里无官方正文，我方"废址换新"和"入金 quarantine"若要外部佐证需另抓（横向简报的 Binance 地址退役先例来自不同抓取）。

---
---

# 档案五：Falcon Finance（XAUT Staking Vault）

**URL**: https://falcon.finance ｜ **模式**：DeFi synthetic dollar 协议 + staking vault（**收益宣传的合规反面教材**）
**Raw**：`competitor-profiles-raw/falcon-finance-xaut/2026-07-22/scrapes/`（4 文件）

## 1 · Cashback

**不适用**。DeFi 协议无信用卡返现（`[全部 4 文件]`）。（注：docs 侧边栏有 "Falcon Card" 导航项，但本批未抓该页，仅记录产品线存在。）

## 2 · XAUT staking vault / yield（核心）

### 2.1 APY 怎么标（逐字——反面教材）

> ⚠️ **写死数字进 H1，非 "up to"**："Falcon Finance Launched Tether Gold Vault with **3-5% APR**"（H1）、"earn an estimated **3-5% APR**"（`[news-xaut-staking-vault-dec2025.md]`）。
> docs 侧更甚："Yield is distributed in USDf at a **fixed APR**. The vault does not mint USDf from user staking."（`[docs-staking-vaults.md]`）——一处"estimated 区间"、一处"fixed"，措辞自相矛盾，且全程无 "subject to change"。

### 2.2 收益来源 / 锁定 / 发放（逐字）

- 收益来源："**Proprietary Yield Generation** — Falcon Finance generates yield through **propriety trading**"（原文拼写保留）；9 大策略几乎全是 CEX delta-neutral 套利（funding rate arbitrage / spot-perp / cross-exchange / options / stat-arb）（`[docs-yield-generation.md]`）。
- 主动澄清不铸币付息："**The vault does not mint USDf from user staking.**"（`[docs-staking-vaults.md]`）。
- **锁定 180 天 + 3 天 cooldown**："for a **180-day lockup**"（`[news-...]`）；"A **3-day cooldown period** applies to allow strategies to unwind"（`[docs-staking-vaults.md]`）。
- **周付 + 发自家 USDf**："paid out **every 7 days in USDf**"（`[news-...]`）——质押 XAUt、拿 USDf（自家 synthetic dollar），本金与收益币种解耦。

### 2.3 合规文案（逐字——系统性反面教材，重点）

- **明说 no KYC**："...secure lockups and **no KYC required**"、"without selling their... tokens or completing KYC"（`[docs-staking-vaults.md]`）。
- **近乎保证收益**："This approach **ensures consistent yields regardless of market conditions**."（`[docs-yield-generation.md]`）——"ensures + regardless of market conditions" 是收益广告高危词。
- **确定性包装**："earn **predictable** USDf rewards"（`[news-...]`）、"creating **sustainable** rewards"（`[docs-staking-vaults.md]`）。
- **类比固收弱化风险**："yield mechanics that **behave more like traditional fixed-income products**"（`[news-...]`）。
- **风险披露近乎为零且与营销页物理隔离**：staking/yield 页只把风险轻描淡写成 "follow Falcon internal risk processes" / "strict risk controls"（强调"我们控得住"而非"你要承担"）；真正的 Risk Management / Terms of Use 页藏在侧边栏另开（`[docs-staking-vaults.md]`、`[docs-yield-generation.md]`）。

### 2.4 XAUt 作 collateral（`[news-xaut-collateral-oct2025.md]`）

- "integrated Tether Gold (XAUt)... as **collateral for minting USDf**"；双用途 "hold and trade... and to use it as a **yield-bearing asset**"；USDf 供应 "$2.1 billion... backed by more than $2.3 billion in reserves"。两条线分开：10 月 collateral（走 mint/redeem）→ 12 月 staking vault（锁仓拿息，明确**不**铸 USDf）。

## 3 · 出金白名单 / AML

**不适用**。DeFi 无中心化出金白名单（`[全部 4 文件]`）。反向记录：staking vault 明确 "no KYC required"；docs 侧边栏另有 KYC 页（本批未抓），说明协议其他环节可能有 KYC。

## 4 · 【实现路径反推】Falcon

| 观察（逐字来源） | 反推的后端实现 |
|---|---|
| 180 天锁 + 3 天 cooldown `[docs-staking-vaults.md]` | **时间锁 vault 合约**：deposit 触发 lockup timestamp，到期 unstake 再进 3 天 unwind 窗口 |
| 周付 USDf 而非发黄金/外部币 `[news-...]` | 付息来源 = 协议内 synthetic dollar，**收益币种与本金币种解耦** |
| "does not mint USDf from user staking" `[docs-staking-vaults.md]` | 主动撇清"庞氏铸币付息"，反推付息 USDf **另有来源**（金库/交易利润铸造）——恰恰暴露"USDf 从哪来"是敏感点 |
| "aggregate TVL... deploy into strategies" + 9 大 CEX 套利 `[docs-yield-generation.md]` | vault 合约聚合 TVL 后由**链下策略账户接管**（资金离链进交易所 = 托管/对手方/穿仓风险，页面零披露） |
| "fixed APR"(docs) vs "estimated 3-5% APR"(news) 打架 `[docs vs news]` | APR 大概率非合约硬编码，而是**运营方可调目标值**，"fixed" 是营销话术 |
| XAUt 是第 4 个 vault 资产（前 3 ESPORTS/VELVET/FF）`[news-...]` | vault 是**通用多资产模板合约**，新资产按同一 180 天/USDf 模板套上线 |
| no KYC + onchain `[docs-staking-vaults.md]` | 纯钱包交互、无账户体系、无出金白名单——与 XAUE 信用卡走传统出金白名单是**根本架构差异** |

## 5 · 【对 XAUE 的启示】Falcon

1. ⚠️ **Falcon 是合规文案的"照妖镜"——逐条对照避坑**：我方 staking reward 展示页**绝不能**出现 Falcon 的这些写法：写死 APR 进标题（应 "up to X%"）、"fixed APR"（应 "variable / subject to change"）、"ensures consistent yields regardless of market conditions"（近乎保证收益，红线）、"predictable/sustainable"（不确定收益包装成确定）、类比固收。我方自家拆解报告已把 "earns yield from day one / 4% APY" 列为证券属性危险信号，方向一致。
2. **收益币种解耦是反面警示**：Falcon 用自家 USDf 付息 = 脱锚风险敞口 + "USDf 从哪来"的可持续性质疑。我方需求 D-1 建议随 cashback 统一 USDT 记账（外部稳定币），避免 Falcon 式"自家币付息"的信任问题。
3. **收益来源必须可外部验证**：Falcon "proprietary trading + internal risk processes" 无法外部核验 + "fixed APR" 与套利本质矛盾 = 潜在补贴/庞氏信号。我方 staking reward 若是"独立奖励比例"，对内要说清资金成本来源（避免变成无源补贴），对外文案守 "may receive distributions" 口径。
4. **风险披露不能与营销页物理隔离**——Falcon 把风险页藏侧边栏、营销页只讲好处是可被监管挑战的做法；我方应在收益展示页**内嵌**风险提示（哪怕一行 + 链接到完整条款）。

---
---

# 跨竞品对比总结：三条需求「该抄谁、怎么抄」

> 本节回答 mentor 方法论 checklist ①（竞品怎么做、能否从交互反推实现路径）的收口结论。与横向简报 §6 的 Differentiate/Parity 决策表**互补不重复**：简报给"做不做"，此处给"抄谁的实现"。

## 需求一 · Cashback 结算

| 维度 | 该抄谁 | 怎么抄 |
|---|---|---|
| **结算模型** | **Nexo**（completed 后发放 + 45 天 claim auto-reject） | 直接对齐我方 B-3/B-4；Crypto.com "授权即发" 是**反面参照**（逼出复杂 clawback），别学 |
| **免 clawback 的实现** | **Nexo** | 每笔 pending 授权带 45 天 TTL + 过期扫描 job；只认清算终态 → 天然无追回场景 |
| **若必须做 clawback 的记账** | **Crypto.com**（备选） | 按发放批次记 token+数量、退款追溯原批次逐笔冲回（token 维度而非法币折算） |
| **cap 计数器** | **Crypto.com**（跨币种共享 counter + UTC 定时 reset） | 我方年 cap 复用"统一 counter + 定时归零 job"模式 |
| **费率配置** | **Crypto.com**（effective-dated Year1/thereafter 分段） | 档位比例带生效日期分段，别硬编码，便于未来调档 |
| **档位呈现三层** | **Nexo**（营销锚点 → FAQ 表 → App 权威） | 我方账单页返现明细（本期 + 年度累计/cap）+ help 专文 |

**一句话**：cashback 机制骨架抄 Nexo（结算 + 免 clawback），工程细节（额度计数器/费率配置/记账兜底）抄 Crypto.com。

## 需求二 · XAUT Staking Reward（独立 APY）

| 维度 | 该抄谁 | 怎么抄 |
|---|---|---|
| **收益记账（需求 7 核心）** | 🔑 **Kraken**（ledger `type=earn`+subtype`reward`） | 新增独立"质押奖励"流水类型 + 每笔价格快照 + dust threshold（低于最小精度不入账） |
| **APY 文案口径** | **Kraken > Nexo**（"up to X% + subject to change + **before commission**"） | 我方守 "up to/variable"；若抽成则标"抽佣前估计" |
| **免责结构** | **Nexo**（页脚三句 + MiCA 产品拆分声明） | 移植"subject to change/may vary by region,tier" + "独立产品独立条款独立牌照边界" |
| **计息节奏** | **Kraken**（accrue daily / pay weekly 两段解耦 + prorated 近实时起息） | 每日计提 + 定期批量发放两 job 解耦；按 allocation 时间戳 prorated |
| **发放币种** | **Nexo/Kraken 的 in-kind 逻辑**，但我方选 **USDT** | 避免 Falcon 自家币付息脱锚风险；随 cashback 统一 USDT |
| **合规红线（避坑）** | ⚠️ **Falcon 反面** | 禁写死 APR/禁"fixed"/禁"ensures consistent yields"/禁类比固收/风险提示必须内嵌营销页 |
| **定位** | 空白位（Nexo 普通抵押品不计息，仅自家币例外） | "信用卡质押品也生息"是差异化卖点，引 Nexo 佐证须注明"仅其平台币例外" |

**一句话**：记账抄 Kraken（最成熟的 reward 会计科目），文案抄 Kraken/Nexo 的稳健口径，然后拿 Falcon 当反面清单逐条自查。

## 需求三 · 出金白名单 + 入金 AML

| 维度 | 该抄谁 | 怎么抄 |
|---|---|---|
| **新地址冷静期** | **Nexo（可配 None/24h/72h/Custom）+ Binance（24/48/72h）** | 我方 D-4 设可配冷静期 + Nexo 式 4h cool-off 缓冲窗 + 新地址 greyed-out 状态 |
| 🔑 **关闭/删除对称冷静期** | **Binance + Crypto.com 双证** | 关闭白名单/删地址必须与新增对称加冷却（堵 disable→instant-withdraw），务必写死 |
| **登记验证** | **Bybit 验证前移思路 + Crypto.com Standard/Universal scope** | 登记时过 WaaS AML（安全成本前置），地址带单币/多币 scope + 强二次验证 |
| **账户级安全锁（加分项）** | **Kraken GSL** | 可选"锁定期禁所有敏感变更 + 可配解锁等待期 + 连客服改不了 + 解锁尝试即通知" |
| **敏感事件定向 hold** | **Kraken**（改密码→仅新地址 hold 12h） | 按"地址新增时间 vs 事件时间做差"定向锁，优于全账户 cool-off |
| **入金命中处置** | **Binance 双态**（自动锁 vs 人工审查）+ **我方前置** | 命中禁归集 + 隔离；分自动锁/人工复核两态，前端按锁类型渲染 UI（有无申诉入口） |
| **误伤申诉** | **Binance**（Appeal 按钮）+ 横向简报 dYdX 教训 | 必配 Manual Review 档 + 用户端申诉入口 |
| **两段式入账通知** | **Nexo**（detected → credited） | 直接进我方 §13 通知 |
| **Travel Rule 采集** | **Binance**（jurisdiction×counterparty 动态表单） | 出金付款环节按辖区+收款方类型动态渲染合规字段 |
| **用户教育文案** | **Crypto.com**（"malicious actor... cannot withdraw to non-whitelisted"） | 直接借鉴其白名单价值主张话术 |

**一句话**：白名单交互抄 Nexo（可配冷静期）+ Binance/Crypto.com（对称关闭防绕过），入金 AML 处置抄 Binance 的双态状态机 + 保留我方"前置禁归集"更严设计并补申诉通道，账户锁可选加 Kraken GSL。

---

## 全档案最关键的一条跨竞品洞察

> 🔑 **三条需求的实现难度天差地别，且难度由"结算/风控是同步还是异步"决定——这是 XAUE 落地排期的核心判据：**
>
> - **cashback 和 AML 处置的复杂度，全押在"事件的终态确定性"上**。Nexo/我方的 cashback 因为**递延到清算终态**（completed / 已清算流水），把"取消/退款/追回"这些不确定态挡在计算之外，天然免掉 clawback 冲回链路——这是 Crypto.com "授权即发" 模型要额外建一整套 token 维度退款追溯的复杂度来源。**同一个"只认终态"原则，同时简化了 cashback（免追回）和入金 AML（禁归集=挡在账本外，而非上账后再冻结）。**
> - **反过来，staking reward 的复杂度不在结算而在"记账可感知"**（需求 7），Kraken 的 `type=earn`+subtype`reward`+价格快照+dust threshold 已经把这件事做成标准会计科目，直接抄即可，是三条需求里工程风险最低的。
> - **落地排期建议**：staking reward（抄 Kraken ledger，风险最低）可先行；cashback 守住"只认已清算流水"这条红线就能复用 Nexo 骨架；出金白名单/入金 AML 最重（多段冷静期状态机 + 对称关闭 + 双态处置 + 申诉 + Travel Rule），且强依赖 WaaS 能力对接（D-2），应最后做且预留人工复核兜底。
>
> **一句话**：**"只认终态"是 cashback 与入金 AML 的共同简化器，"独立记账科目"是 staking reward 的共同复杂度收敛器——抓住这两个杠杆，三条需求的实现路径就都清晰了。**

---

## Raw Data Sources

- Nexo Card：`competitor-profiles-raw/nexo-card/2026-07-22/scrapes/`（31 文件）
- Crypto.com：`competitor-profiles-raw/crypto-com-card/2026-07-22/scrapes/`（26 文件）
- Binance：`competitor-profiles-raw/binance-whitelist/2026-07-22/scrapes/`（9 文件）
- Kraken：`competitor-profiles-raw/kraken-xaut-earn/2026-07-22/scrapes/`（8 文件）
- Falcon Finance：`competitor-profiles-raw/falcon-finance-xaut/2026-07-22/scrapes/`（4 文件）
- 抓取方式：既有 raw（WebFetch/WebSearch，Firecrawl/DataForSEO 未配置）；本次为纯综合，无联网重抓
- 复用成果：横向简报 `竞品简报-cashback-reward-AML-2026-07-22.md`、公司层档案 `竞品档案-Nexo-CryptoCom-2026-07-22.md`

## 已知信息缺口（如实标注）

| # | 缺口 | 影响 |
|---|---|---|
| 1 | Nexo 返现在账单条目内的展示样式无专门文案 | 我方账单返现明细样式无直接竞品可抄，按 B-5 自行设计 |
| 2 | Nexo 信用卡质押品是否计息 = 营销层 vs 机制层双轨矛盾，仅平台币 NEXO 例外明确 | 引 Nexo 佐证需注明"仅其平台币例外"；建议向 Nexo 实测确认 |
| 3 | Crypto.com 结算措辞（settled/posted/两个账单周期）26 文件全无，卡侧明确 no billing cycle | Crypto.com 非异步确认对标先例，是"授权即发"反面参照 |
| 4 | Binance 入金 AML 全链条 + 地址退役无官方正文（仅搜索摘要，低置信） | 我方"废址换新""入金 quarantine"若需 Binance 佐证要另抓 EDD/Source of Wealth 页 |
| 5 | Kraken XAUT：APY(0.1%) vs APR(0.10%) 两口径并存；流动性口径 Auto Earn 概述 vs Opt-In 明细冲突（以明细为准） | 引用数值/流动性时以 support 明细为准 |
| 6 | Falcon 收益来源官方从未量化披露、"fixed APR" 疑为可调目标值 | 仅作合规反面教材，不采其数值 |
| 7 | Nexo 每币种最小提币额全表 = App 内动态渲染，网页未公开 | 已在既有公司层档案标注 |

> 本档案由 AI 依已抓取的 78 个 raw 文件综合编制；推测性结论已标「推测/反推」，未公开/未查到项已如实标注；费率/利率/冷静期类数字时效性强，落决策前建议对缺口 2/4/5 做一次 app 内人工核对。
