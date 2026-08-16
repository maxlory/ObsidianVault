# 竞品简报：XAUE 质押信用卡三条新需求（Cashback 结算 · XAUT Staking Reward · 出金白名单 + 入金 AML）

> **生成方式**：本文档由 competitive-brief skill 生成
> **日期**：2026-07-22（全部外部事实截至本日抓取；费率/利率类信息时效性强，随时会变）
> **输入文档**：
> - 需求权威来源：`/Users/aa00158/xaue-project/新需求-staking-reward与AML/三条新需求—需求总参考.md`
> - 行业数据起点（本简报已逐条核实更新，其"复用已有收益"结论已作废）：`/Users/aa00158/xaue-project/新需求-staking-reward与AML/需求解读-XAUT-staking-reward.md`
> - 竞品公司层基本面复用（Nexo/Crypto.com 牌照、口碑、规模，不重查）：`/Users/aa00158/Documents/xaue-产品功课/竞品简报-充提还规则-2026-07-22.md`
> **我方产品**：XAUE 质押信用卡——质押 XAUE（锚定 XAUT 的链上黄金代币）入 Collateral Vault，LTV 60% 授信，RedotPay 发卡，充值/还款走 USDT/XAUT（ERC-20 + TRC-20）
> **方法**：3 路独立 web 调研（官方条款/help center 优先，共 87 次工具核查）；查不到的标「未公开/未查到」，不做推测填充

---

## TL;DR

三条需求的行业对标结论：**① Cashback**——我方「递延到账单出账、按已清算流水计算、账单内抵扣」与传统卡正统惯例（Chase/Citi 的 statement-close 模式）完全同构，机制层面照 B 层决定落地即可；2%/2.5%/3% 阶梯处于行业上沿水位（超过绝大多数无条件返现卡），「按年消费分档 + 年 cap」在竞品中无先例、属差异化。**② Staking reward**——全行业没有任何平台给信用卡/信贷质押品单设收益档（Nexo 明确质押品不产息），我方定位是空白位置；Binance Flexible Loan「质押品保留基础息、剥离补贴档」是"低于 stake 产品的独立 APY"定价逻辑的最佳先例；黄金代币真实收益极低（0.1%–0.65%），借贷模式 3%–5.5%，文案必须 "up to/variable" 不写死。**③ 白名单 + AML**——我方「登记制白名单 + 登记时过 AML」比零售所惯例更严，但缺行业标配的「新地址冷静期（24–72h）」；入金「命中禁归集 + 隔离 + 废址换新」比行业通行的"上账后冻结"更前置，Binance 充值地址退役机制提供了可行性先例，需补人工复核/申诉通道防误伤（dYdX–Tornado Cash 教训）。

---

## 0 · mentor 点名题直答：Visa/Master 平台的 cashback 怎么结算、周期多少

> mentor 原话（A3-1）："竞品调研: 在其他的 visa 和 master 平台是怎么进行结算的, 结算周期是多少"

**四句话答案**（证据见 §3.1）：

1. **计算基础**：全行业（传统卡 + crypto 卡）cashback 一律基于 **clearing/settlement 完成后的 posted 交易**计算，authorization/pending 状态的交易不计返现。Discover 官方原话："You earn cash back only when they're processed"；Nexo 官方原话："Once your transaction with the Nexo Card is **settled**, you'll receive crypto cashback"。
2. **交易结算周期**：卡组织轨道上 authorization 即时 → clearing 当天/隔夜批量 → settlement 通常 **1–3 个工作日**（Visa/Mastercard 网络层只提供这条轨道，cashback 本身是发卡行/program owner 的 rewards program，网络层无返现机制）。
3. **返现发放周期**：无统一标准，主流两种——**账单周期结束（statement close）统一计入**（Chase、Citi、Amex）或**交易 post 后滚动入账**（Capital One 10 天内可见、Discover processed 后即计）；crypto 卡两极：Nexo/Wirex settle 后近即时，Crypto.com 美国信用卡最长 2 个账单周期才确认。
4. **退款处理**：行业通例是从 rewards 余额自动追回等额返现（可扣成负余额）；Chase 条款按 "purchases minus any returns or refunds" 计算，Crypto.com 条款明文 "will be deducted… may result in a negative CRO balance"。

**对我方设计的直接含义**：B-3「递延到账单出账时统一计算」+ B-4「只计已清算流水」= 传统卡 statement-close 正统模式，且因为基数只认清算终态，天然免掉了竞品都要做的 clawback 追回机制——**对齐惯例且更简洁，维持设计**。

---

## 1 · 我方三条需求快照（B 层已确认口径）

来源：`三条新需求—需求总参考.md` B 层

| 需求 | XAUE 设计（2026-07-22 确认） |
|---|---|
| Cashback | 按开卡年年消费阶梯：<$120k → 2.00%（cap 500）/ ≥$120k → 2.50%（cap 1,000）/ ≥$360k → 3.00%（cap 1,500）；达档全量适用；USDT 记账、账单内抵扣；递延到账单出账时按已清算流水统一计算 |
| XAUT staking reward | 给信用卡质押品一个独立 APY（低于自家 stake 产品收益率，stake 产品宣传口径 4%）；以记账流水形式发放、给用户明确感知；APY 数值/计息基数/发放周期币种待定（D-1） |
| 白名单 + AML | 接入 WaaS 的 AML 服务（不自建 Chainalysis）；入金命中黑灰 → 禁用地址、禁止归集上账、用户端+管理后台风险提示（推导层：隔离记录 + HD 换新地址）；出金 = 客户登记白名单地址、平台只向白名单付款、登记时过 AML；黑灰标准以 WaaS 风控阈值为准 |

---

## 2 · 竞品概览

公司层基本面（成立、牌照、规模、口碑）已在《竞品简报-充提还规则-2026-07-22.md》§2 查证，此处只复用结论不重查：

- **Nexo Card**（最直接竞品，crypto 质押授信卡）：2018 年成立，瑞士 Zug；$7B+ AUM（官网口径，与 2025-04 PR $11B 矛盾未决）；截至 2026 年中未持 MiCA CASP 牌照（申请中）；Mastercard 网络、欧洲区可用；Trustpilot ≈4.4/5。2026-02 经 Bakkt 重返美国（含 yield 产品，卡未随行）。
- **Crypto.com Visa**（直接竞品，主线 prepaid + 美国另有真信用卡）：2016 年创立，新加坡；1.4 亿注册用户；持 EU MiCA CASP、UK EMI、MAS MPI、Dubai VARA；Trustpilot 仅 1.6/5，费用不透明是头号差评主题。美国 2025 夏推 Visa Signature 真信用卡（Comenity Capital Bank 发卡）——本简报 cashback 节的核心官方证据源。
- **本简报新增的惯例锚点**（非卡竞品，只取单点惯例）：传统发卡行 Chase/Citi/Amex/Capital One/Discover（cashback posting 惯例）；Kraken/Falcon Finance/OKX/YouHodler（黄金代币收益）；Binance/Kraken/Coinbase/OKX/Bybit（白名单交互）；Fireblocks/Cobo（WaaS AML 能力）。

---

## 3 · Cashback 消费返现

### 3.1 结算机制证据明细（点名题展开）

**传统卡（美国主流发卡行，官方条款级证据）**：

| 发卡行 | 计算基础 | 发放时点 | 退款处理 | 来源 |
|---|---|---|---|---|
| Chase | billing cycle 内 posted purchases − returns/refunds | statement close 时计入，下一周期初可用；周期末交易可能顺延 1 个周期 | 直接从当期计算中扣减 | 官方 [Freedom Flex Rewards Agreement](https://www.chase.com/freedomflex/rewardsagreement)、[退款教育页](https://www.chase.com/personal/credit-cards/education/basics/how-refunds-and-returns-work-on-credit-cards) |
| Citi Double Cash | 两段式：1% 购买入账时 + 1% 还款时（至少还 minimum due） | 第 1 个 1% 随消费入账周期，第 2 个 1% 随还款产生 | 通例扣回 | 官方 [citi.com](https://www.citi.com/credit-cards/credit-card-rewards/citi-double-cash-credit-card-benefits) |
| Amex | eligible purchases（− returns and other credits） | 官方口径 "allow 8-12 weeks"；实际多在 statement close 后出现（第三方口径） | returns 从基数中剔除 | 官方 [Rewards FAQ](https://www.americanexpress.com/en-us/benefits/rewards/rewards-information/)；[WalletHub（第三方）](https://wallethub.com/answers/cc/amex-blue-cash-preferred-cash-back-post-date-2140640903/) |
| Capital One | net purchases | 10 天内线上可见，最长 2 个 statement cycle 正式入账 | net 口径自动扣 | 官方 [Help Center](https://www.capitalone.com/help-center/credit-cards/manage-your-rewards/) |
| Discover | 仅 processed 交易（"may be after the transaction date"） | processed 后计入 | 官方页未明说（通例适用，[CreditCards.com 第三方](https://www.creditcards.com/education/credit-card-rewards-after-returning-purchases/)） | 官方 [Cashback Bonus 页](https://www.discover.com/credit-cards/cash-back/cashback-bonus.html) |

auth→clearing→settlement 时序佐证：[Stripe](https://stripe.com/resources/more/payment-settlement-explained-how-it-works-and-how-long-it-takes)、[Marqeta](https://www.marqeta.com/blog/card-program-clearing-and-settlement-how-issuer-processors-manage-fund-flow)（均为支付基础设施方口径）。卡组织角色自证：Crypto.com 卡条款 "issued by Comenity Capital Bank, pursuant to a license from Visa Inc."——rewards 规则完全由 program owner + 发卡行定义。

**Crypto 卡**：

- **Nexo Card**（官方 [nexo.com/crypto-card](https://nexo.com/crypto-card)、[官方博客](https://nexo.com/blog/switch-to-shopping-mode-and-get-crypto-cashback)、KB 镜像 [nexo.how](https://nexo.how/kb/what-crypto-rewards-will-i-get-with-my-nexo-card/)）：双币档——NEXO 币 0.5%/0.7%/1%/2%（Base/Silver/Gold/Platinum），BTC 档 0.1%–0.5%；tier 由 NEXO 持仓占组合比例决定；仅 Credit Mode 且账户余额 ≥$5,000 有返现；**月 cap $50/$100/$150/$200**；发放时点 = 交易 **settled 后**；pending 交易处理与退款 clawback 条文**未查到**（仅有 7 类 ineligible MCC 清单）。
- **Crypto.com 美国 Visa Signature 信用卡**（官方 [Program Terms PDF](https://crypto.com/document/us_credit_card)）：CRO 档 1.5%（免费）/3.5%/4.5%/5%/6%（订阅 $4.99–$29.99 月费或 $500–$500,000 CRO lock-up），CRO 档无 cap；BTC 奖励可选、有月度消费 cap（$250–$10,000，超出降档）；发放 "**may take up to two billing cycles** for CRO to be confirmed"，明确锚定 posted 交易；退款条款明文追回且**按初始 CRO 数量**（币价波动风险归用户）。
- 顺手锚点：Coinbase One Card——奖励在交易 "**posts** to your account" 时发放（官方 [T&C](https://www.coinbase.com/legal/creditcard/rewards)）；Wirex Cryptoback——购买后几分钟内发 WXT，近即时（官方 [help](https://help.wirexapp.com/article/wirexs-cryptobacktm-rewards-explained-1346)）。

### 3.2 档位与 cap 水位对比

| 产品 | 比例 | cap | 分档维度 |
|---|---|---|---|
| **XAUE（设计）** | **2% / 2.5% / 3%** | **年 cap 500/1,000/1,500 USDT** | **开卡年年消费额** |
| Citi Double Cash | 2% 平铺 | 无 cap | 无分档 |
| BofA Unlimited Cash + Preferred Rewards | 1.5%→2.625% | 无 cap | 存款/投资余额 ≥$100k（[TPG 第三方](https://thepointsguy.com/credit-cards/bank-of-america-preferred-rewards-program/)） |
| US Bank Smartly | 2%→最高 4% | bonus 部分限每 cycle 前 $10,000 消费 | 存款/投资余额三档（[官方 IR](https://ir.usbank.com/news-events/news/news-details/2024/U.S.-Bank-unveils-industry-leading-card-savings-combination/default.aspx)） |
| Robinhood Gold Card | 3% 平铺 | 无 cap | 订阅 Gold $5/月（[官方](https://robinhood.com/creditcard)） |
| Amex BCP / Chase Freedom Flex | 6% 超市 / 5% 轮换类别 | 年 $6,000 / 季 $1,500 类别 cap | 消费类别 |
| Nexo Card | 0.5%–2% | 月 cap $50–$200 | NEXO 持仓比例 |
| Crypto.com | 1.5%–6% | CRO 无 cap / BTC 月消费 cap | 订阅或 CRO 锁仓 |

**水位判读**：XAUE 的 2% 起步 = 无条件返现卡的天花板水平（Citi Double Cash 级），2.5%/3% 已超过 BofA 顶档 2.625% 的门槛线、比肩 Robinhood 3%；但年 cap 意味着实际返现率有上限——按 cap 拉满反推，$120k 年消费拿满 500 USDT 实际约 0.42%、$360k 拿满 1,500 USDT 实际约 0.42%——**名义比例是行业上沿，cap 后的有效比例远低于名义值**，这是与竞品沟通口径上要预防的攻击点（竞品月 cap 同样存在此效应：Nexo Platinum 2% 月 cap $200 = 月消费 $10k 即触顶）。

### 3.3 功能对比矩阵（Strong / Adequate / Weak / Absent）

> 评级口径：面向「返现机制的规则完整度与用户价值」；XAUE 列基于 PRD 设计值（未上线）；传统卡列取 Chase/Citi/Amex 惯例聚合。

| 能力项 | XAUE（设计） | Nexo Card | Crypto.com Visa | 传统卡惯例 |
|---|---|---|---|---|
| 结算基础（只认清算后交易） | **Strong**：只计已清算流水，pending/取消/退款不入基数 | **Strong**：settled 后发放（官方明文） | **Strong**：posted 交易 + 最长 2 周期确认 | **Strong**（惯例定义者）：posted − returns |
| 发放时点确定性 | **Strong**：账单出账时统一计算，时点与账单绑定、可预期 | **Adequate**：settle 后近即时（体验快，但无对账节奏） | **Weak**：官方口径"最长 2 个账单周期"，确认时点不可预期 | **Adequate**：statement close 惯例，Amex 官方口径可到 8-12 周 |
| 退款/追回机制复杂度 | **Strong**：基数只认清算终态 → 天然无追回场景，规则最简 | **Weak**：clawback 条文未查到，规则不透明 | **Adequate**：有明文追回但按初始 CRO 数量，波动风险归用户 | **Strong**：net purchases 口径 + 负余额机制成熟 |
| 返现币种价值稳定性 | **Strong**：USDT 记账，无币价波动，直接抵扣账单减还款 | **Adequate**：NEXO/BTC，有波动；NEXO 档与平台币深度绑定 | **Weak**：CRO 平台币，波动 + 追回按数量双重暴露 | **Strong**：法币/账单抵扣 |
| 名义返现率水位 | **Strong**：2%–3%，行业上沿 | **Weak**：0.5%–2%，且要求持 NEXO + 余额 ≥$5,000 | **Strong**：1.5%–6%（高档需订阅/重锁仓） | **Adequate**：1%–2% 无条件档为主流 |
| cap 后的有效返现率 | **Weak**：年 cap 拉满仅 ≈0.42%，名义与有效落差大 | **Weak**：月 cap $50–$200 同样触顶快 | **Adequate**：CRO 档无 cap（但高档订阅成本对冲） | **Adequate**：无条件档普遍无 cap，类别档有 cap |
| 分档获取门槛 | **Adequate**：纯消费额分档，无需锁仓/订阅——获取逻辑最干净，但 $120k/$360k 门槛极高 | **Weak**：要求组合持 10% NEXO 平台币 | **Weak**：订阅费或 CRO 锁仓，2025-09 改版免费档返现归零 | **Adequate**：多数无门槛或按资产余额 |

### 对 XAUE 的启示（Cashback）

1. **机制全部维持**（parity 已达成）：递延出账时计算 = statement-close 正统；只认清算流水 = 全行业共同底线且我方实现最简（免 clawback）；USDT 记账抵扣 = 传统卡"账单抵扣"惯例的 crypto 版，优于竞品的平台币返现。
2. **年 cap 是差异化也是暴露面**：竞品全是月 cap（crypto 系）或类别 cap（传统系），年 cap 对高频小额用户更宽松，可作卖点；但 cap 拉满后有效返现率 ≈0.42%，营销文案若只讲 "3% cashback" 会重演 Crypto.com "费用不透明" 式差评——建议账单页返现明细（B-5 已定的"本期返现 + 年度累计/cap 两行"）就是正确解法，落地时把 cap 进度做成显性进度条。
3. **一个待补规则**：跨账单周期的清算流水归属——交易在账单日前发生、账单日后才清算的，计入下一期（Chase 惯例"周期末交易顺延 1 个周期"是现成参考），PRD 修订 §5 账单时应写明。
4. **档位门槛沟通**：$120k/$360k 年消费门槛远超普通用户（对标的是私行级消费力），符合黄金卡高净值定位，但 2% 基础档已是行业上沿，营销重心应放在"基础档即顶级水位 + 无锁仓无订阅"上——这是对 Nexo（要锁平台币）和 Crypto.com（要订阅）的直接打点。

---

## 4 · XAUT Staking Reward（独立 APY 奖励）

### 4.1 竞品黄金代币收益产品全景（2026-07-22 逐条核实）

| 平台 | 资产 | 利率 | 收益来源模式 | 发放周期/币种 | 锁定 | 合规文案口径 |
|---|---|---|---|---|---|---|
| Kraken Auto-Earn | XAUT | **0.1% APY** | 未披露（rewards program），平台抽佣 | 周付 / 原币 XAUT | 活期 | 写死 0.1% + "subject to change"；美国/EEA 不可用 |
| Nexo Earn | XAUT | **up to 5.5%**（旧口径 6.25% 疑已下调） | 借贷（credit line 放贷分息） | 活期日复利 / 定期到期付 | 两档 | 全部 "up to" + "rates subject to change and may vary by region, loyalty tier" |
| Falcon Finance | XAUT | 3–5% APR（公告口径） | **未披露**（第三方解读为 XAUT 抵押跑稳定币策略） | 周付 / USDf（自家稳定币） | 180 天 | 零风险披露，反称 "predictable"——反面教材 |
| OKX | XAUT | **13% bonus APR**（旧数据 12% 不准）| 借贷市场 + 营销补贴 | 原币 XAUT | 活期 | 限 0.1 XAUT/人（≈$340）、一生一次、抽佣 15%——纯获客补贴 |
| YouHodler | PAXG | 8.2% APR（官方博客写死） | 自营（loyalty/reward program） | 未核实（第三方称周付） | 活期 | 产品页不标数字只说 "earn high rewards"；美国资格未明说 |
| Binance | PAXG | 0.65% APY（第三方口径） | 借贷 | 原币 | 活期 | 官方页未核实 |

来源：[Kraken 官方页](https://www.kraken.com/features/auto-earn/tether-gold)、[Kraken Support](https://support.kraken.com/articles/overview-of-auto-earn-on-kraken)、[Nexo 官方页](https://nexo.com/earn-crypto/tether-gold)（JS 渲染，经索引 snippet + [PAXG 页](https://nexo.com/earn-crypto/pax-gold-paxg) + [Bitcompare 第三方](https://bitcompare.net/coins/tether-gold/staking-rewards)互证）、[Falcon 官方公告](https://falcon.finance/news/falcon-finance-tether-tokenized-gold-xaut-vault-announcement)、[Phemex（第三方）](https://phemex.com/news/article/falcon-finance-introduces-xaut-staking-vault-with-35-apy-44047)、[OKX 官方 help](https://www.okx.com/en-us/help/xaut-simple-earn-flexible-bonus-policy-update)、[OKX Simple Earn FAQ](https://www.okx.com/en-ar/help/simple-earn-faq)、[YouHodler 官方博客](https://www.youhodler.com/blog/pax-gold-paxg-youhodler)。

**结构判读**：黄金代币的"真实"收益极低（Kraken 0.1% / Binance PAXG 0.65%——黄金本身不生息，XAUT 是 ERC-20 凭证无协议收益，[Tether 官方站](https://gold.tether.to/)全站无任何 yield 字样）；中高收益全部来自借贷（Nexo up to 5.5%）、自营策略（Falcon 3–5%）或营销补贴（OKX 13% 限额一生一次）。

### 4.2 信用卡/信贷质押品付息先例（我方定位的直接对照）

1. **Nexo Instant Crypto Credit Line**（Nexo Card credit mode 同一质押池）：官方 KB 明确**质押品不产息**——资产转入 Credit Line Wallet 后停止计息，唯二例外是自家 NEXO token 和 NETH（"the only two assets that will continue to get you daily payouts even when held as collateral"，[nexo.how KB](https://nexo.how/kb/how-to-transfer-assets-between-the-savings-wallet-and-the-credit-line-wallet/)）；移回 Savings Wallet 需 24–52h cool-off 才恢复计息。→ 最直接竞品对信用卡质押品的答案是 **Absent**。
2. **Binance Flexible Loan**（[官方 FAQ](https://www.binance.com/en/support/faq/what-is-binance-flexible-loan-and-frequently-asked-questions-1c9dddb774054983992b8977ae36577a)）：质押品留在 Simple Earn Flexible 仓位内**继续赚 Real-Time APR，但不享 Bonus Tiered APR 和空投**。→ 市场上最接近「质押品拿低于正常档的收益」的分层先例，直接支撑我方"独立低 APY"的定价逻辑：**基础息照拿、增益档剥离**。
3. **未查到**任何平台给"信用卡质押品"单设 APY 档——该位置行业空白。

### 4.3 功能对比矩阵（Strong / Adequate / Weak / Absent）

| 能力项 | XAUE（设计） | Nexo（Credit Line/Card） | Binance Flexible Loan | Kraken / Falcon（纯 Earn） |
|---|---|---|---|---|
| 信用卡/信贷质押品付息 | **Strong**：独立 APY，行业唯一给信用卡质押品设收益档 | **Absent**：质押品明确不产息（仅自家 NEXO/NETH 例外） | **Adequate**：借贷质押品保留基础息、剥离 bonus 档（非信用卡场景） | —（无信贷场景，不适用） |
| 收益感知与记账 | **Strong**：记账流水形式逐笔提示（B-7） | **Adequate**：Earn 侧日复利可见，质押侧无收益无感知 | **Adequate**：奖励滚入质押仓位 | Kraken 周付流水可见 **Adequate** / Falcon 周付 USDf **Adequate** |
| 收益率竞争力 | 待定（D-1）——低于自家 stake 4% 的独立档 | Earn 档 up to 5.5%（借贷模式上沿） | PAXG 0.65% 量级（第三方口径） | Kraken 0.1%（真实收益锚）/ Falcon 3–5%（策略档） |
| 合规文案成熟度 | 待建——有自家拆解报告的证券属性警示做底线 | **Strong**："up to" + 区域/tier 免责，全行业最稳 | **Adequate**：real-time APR 浮动口径 | Kraken **Strong**（subject to change + 风险披露）/ Falcon **Weak**（零披露，反面教材） |
| 发放币种价值稳定性 | 建议随 cashback 统一 USDT（C-4，待定） | 原币或 NEXO（收 NEXO 有加成=平台币绑定） | 原币 | Kraken 原币 / Falcon USDf（自家稳定币，脱锚风险） |

### 对 XAUE 的启示（Staking Reward）

1. **定位 = 空白位置，放大而非对齐**：「信用卡质押品也有收益」在 crypto 卡圈无人做（Nexo 明确不给），是可直接写进营销文案的差异化——"质押黄金获得消费力，质押品还在生息"恰好补上 Nexo "spend without selling" 叙事缺的最后一块（Nexo 用户 spend 时抵押品停止 earning）。
2. **"低于 stake 产品"的定价逻辑有先例背书**：Binance Flexible Loan 的"基础息保留、增益剥离"结构证明市场接受质押品收益打折；我方口径可表述为"质押品同时换取了授信功能，收益率低于纯 stake 是合理对价"。
3. **APY 数值区间参考**（D-1 决策输入）：地板 = 真实收益锚 0.1%–0.65%（Kraken/Binance），天花板 = 自家 stake 产品 4%（必须低于它，mentor 硬约束）；行业借贷模式 3%–5.5% 是纯 Earn 产品水位、不适合直接对标。**合理落点 1%–2.5% 量级**：显著高于 Kraken 0.1%（有感知）、与 stake 4% 拉开明确差距（保住"stake 产品更划算"的产品梯度）。具体数值待业务按资金成本定。
4. **文案纪律（合规底线）**：学 Nexo/Kraken——"up to X%" + "rates subject to change"，绝不写死固定 APY；自家拆解报告已把 "earns yield from day one / 4% APY" 列为证券属性危险信号，建议统一 "may receive distributions" 口径；Falcon 的零披露是反面教材不可学。
5. **发放币种建议**：随 cashback 统一 USDT 记账（C-4 方向正确）——避免 Falcon 用自家稳定币付息的脱锚风险敞口，也避免 Nexo 用平台币加成的深度绑定模式（我方无平台币）。

---

## 5 · 出金白名单 + 入金 AML

### 5.1 出金白名单交互对比（2026-07-22 抓取）

| 平台 | 开关形态 | 登记验证 | 新地址冷静期 | 变更/删除 | 数量上限 |
|---|---|---|---|---|---|
| **XAUE（设计）** | **登记制：只向白名单地址付款**（最严形态） | 登记时过 WaaS AML 检测（行业未见先例） | **未设计**（D-4 待定） | 未设计（D-4 待定） | 未设计（D-4 待定） |
| Binance | 可选开关；开启后只能提到白名单地址 | 2FA | 可选 24/48/72h 三档（期内禁向新地址提现） | 关闭白名单触发同档位提现暂停 | 200 个 |
| Kraken | **地址簿强制**：必须先添加确认才能提现 | 邮件确认链接（1 小时有效）；受信设备自动批准 | 常态无；改密码等风险事件后 hold 12–24h（两处官方文章口径不一） | 确认邮件内附一键锁定账户入口 | 未公开 |
| Coinbase（零售） | 可选 Allowlisting | 2FA（启用/停用/使用均需） | 新地址 **48h** 后才可用 | **关闭功能同样 48h 延迟**（双向，防攻击者先关白名单再提币） | 未公开 |
| OKX | 可选 Allowlist | 手机 + 邮件验证 | 可选新地址 24h 禁提 | 关闭锁需手机+邮件验证 | 未公开 |
| Bybit | 可选两档 | 白名单地址提现**免** 2FA（验证前移到登记时） | 新地址 24h 禁提 | 未查到 | 未公开 |
| Fireblocks（托管侧） | 白名单地址须 **Admin Quorum 多管理员审批**，任一否决不生效 | — | — | — | — |

来源：[Binance Whitelist FAQ](https://www.binance.com/en/support/faq/1d08944f103b4fc78d3519913b600086)、[Kraken 添加提现地址](https://support.kraken.com/articles/7631228462484-adding-and-confirming-a-new-cryptocurrency-withdrawal-address)、[Coinbase Allowlist](https://help.coinbase.com/en/coinbase/managing-my-account/other/address-book-allowlist)（官方页对爬虫 403，经索引摘要核对）、[OKX Allowlist](https://www.okx.com/en-us/help/how-do-i-enable-allowlist-app)、[Bybit 地址簿](https://www.bybit.com/en/help-center/article/How-to-Manage-Your-Withdrawal-Address-Book)、[Fireblocks Whitelist docs](https://developers.fireblocks.com/docs/whitelist-addresses)。

### 5.2 入金 AML 命中后的处置惯例

**交易所处置**：通行模式 =「先上账后冻结（或挂起不上账）→ 人工审查 → 索要资金来源证明（SoF）→ 放行/退回/上报」，没有一家公开承诺自动退回原地址：

- Kraken（官方）：合规命中的入金标「**On Hold**」，部分到期自动放行、部分需人工介入（[官方 help](https://support.kraken.com/articles/360000380966-why-is-my-deposit-or-withdrawal-on-hold-)）；第三方入金要求来源可验证、可索要 SoF（[官方](https://support.kraken.com/articles/360000382423-deposits-from-and-withdrawals-to-third-parties)）。
- Binance：官方仅承认 "withdrawal under review"（[官方 FAQ](https://www.binance.com/en/support/faq/360038583951)）；冻结 + 索要 SoF、审查数天到数周为第三方口径（[AMLBot](https://blog.amlbot.com/why-crypto-exchanges-freeze-deposits-after-aml-checks/)）。
- Coinbase（第三方口径）：prohibited source 触发「Account Restricted」+ 站内引导上传证明。
- **直接对标我方"废址换新"的先例**：Binance 会主动**退役充值地址**（deposit address retirement）——通知 30 天后旧地址过期，后续入金不自动上账、需用户手动认领（[官方 FAQ](https://www.binance.com/en-IN/support/faq/why-is-my-deposit-address-being-retired-853aaedc7f45430283abc7a25713c428)）。
- **误伤教训（权威案例）**：2022-08 Tornado Cash 被 OFAC 制裁后，dYdX 的合规供应商按"几跳内沾染"筛查导致大量仅间接沾染微量资金的账户被封，随后被迫解封并公开澄清"封禁≠没收"（[CoinDesk](https://www.coindesk.com/business/2022/08/11/crypto-exchange-dydx-blocked-accounts-that-received-even-small-amounts-from-tornado-cash)）。

**用户端提示共性**：余额可见但功能受限 + 站内状态标记（如 "On Hold"/"Account Restricted"）+ 邮件为正式沟通渠道——与我方"用户端 + 管理后台风险提示"方向一致。

**WaaS 层能力**（我方技术路线 B-8 的行业验证）：

- **Fireblocks**（官方）：与 Chainalysis、Elliptic 直接集成，入金/出金实时自动筛查；策略引擎支持 **Auto-freeze**——命中即自动冻结、挂起待人工复核（[AML Policies docs](https://developers.fireblocks.com/docs/define-aml-policies)）。
- **Cobo**（官方，与我方托管底层同源）：Cobo Screening 应用，集成 CipherOwl（开箱即用）+ Elliptic（配 API key），四档处置：Approve / Approve with Alert / **Reject（冻结资金）** / Manual Review（[官方手册](https://manuals.cobo.com/en/apps/screening/introduction)）；「命中后禁止归集」的明文表述未查到，Reject-冻结机制与之等价。
- 行业规范锚点：FATF Travel Rule（Recommendation 16）要求 VASP 在转账 ≥ USD/EUR 1,000 时采集核验传输双方信息（[FATF PDF](https://www.fatf-gafi.org/content/dam/fatf/documents/recommendations/Targeted-Update-Implementation-FATF%20Standards-Virtual-Assets-VASPs.pdf)）。

### 5.3 功能对比矩阵（Strong / Adequate / Weak / Absent）

| 能力项 | XAUE（设计） | Binance | Kraken | Coinbase | WaaS（Fireblocks/Cobo） |
|---|---|---|---|---|---|
| 白名单强制力 | **Strong**：登记制、只向白名单付款 | **Adequate**：可选开关 | **Strong**：地址簿强制 | **Adequate**：可选 | **Strong**：Admin Quorum 审批 |
| 登记环节风控 | **Strong**：登记时过 AML 检测（零售所未见先例） | **Adequate**：2FA | **Adequate**：邮件确认 + 受信设备 | **Adequate**：2FA | **Strong**：多人审批 |
| 新地址冷静期 | **Absent**：未设计 | **Adequate**：可选 24/48/72h | **Weak**：常态无（仅风险事件后 hold） | **Strong**：双向 48h（最严参考） | —（审批制替代） |
| 入金命中处置前置度 | **Strong**：禁归集 + 隔离 + 废址换新（比"上账后冻结"更前置） | **Adequate**：上账后审查冻结；有地址退役先例 | **Adequate**：On Hold 挂起 | **Adequate**：账户受限流程 | **Strong**：Auto-freeze / Reject 原生支持 |
| 用户端风险提示 | **Adequate**：用户端 + 管理后台提示（对齐行业：状态标记 + 通知） | **Adequate**：状态文案 + 邮件 | **Strong**：On Hold 状态 + 原因描述 | **Adequate**：banner + 站内流程 | —（面向机构） |
| 误伤复核/申诉通道 | **Absent**：未设计 | **Adequate**：Deposit Status Query 自助 + 客服 | **Adequate**：Support 人工介入 | **Adequate**：Resolve restriction 站内流程 | **Adequate**：Manual Review 档 |

### 对 XAUE 的启示（白名单 + AML）

1. **该抄谁**：出金白名单交互抄 **Coinbase 的双向 48h 冷静期**（新地址延迟生效 + 关闭/删除同样延迟——防"攻击者先改白名单再提款"，是行业最严也最完整的模型）+ **Bybit 的验证前移**（白名单地址付款免逐笔重验，把安全成本一次性放在登记环节——与我方"登记时过 AML"天然契合）。D-4 待定项应补齐：冷静期 24–48h、变更/删除对称冷静期、地址数量上限（Binance 200 个可作参考上界，我方场景 5–10 个足够）。
2. **入金处置我方比行业更前置，保留但补申诉**：行业通行"先上账后冻结"，我方"禁止归集上账 + 隔离记录"把风险挡在账本外，配合 Binance 地址退役先例（废址换新有官方可行性背书），机制上站得住；但 dYdX 误伤案例是硬教训——**必须配 Manual Review 档 + 用户申诉通道**（管理后台人工复核界面 + 用户端"联系客服提交材料"入口），否则 WaaS 阈值一刀切会复制大规模误伤。
3. **WaaS 路线验证通过**：Fireblocks/Cobo 均原生支持"命中自动冻结/拒绝 + 人工复核"，我方"直接接入 WaaS AML、以其阈值为准"（B-8/B-11）是行业标准架构，不自建 Chainalysis 的决定成立；对接清单（D-2）应确认 WaaS 是否支持 Cobo Screening 的四档处置粒度（尤其 Manual Review 档）。
4. **用户端文案惯例**：命中提示学 Kraken——给状态 + 原因类别（不透露风控细节），资金"隔离≠没收"要在文案里讲清楚（dYdX 教训的另一半：沟通不当会放大恐慌）。

---

## 6 · 总决策建议：Differentiate vs Parity

| 动作 | 内容 | 依据 |
|---|---|---|
| **Parity（对齐惯例，照 B 层落地）** | cashback 递延到账单出账 + 只认清算流水 + 账单内抵扣；staking reward "up to/variable" 文案口径；白名单登记制 + WaaS AML 架构 + 用户端状态提示 | 传统卡 statement-close 正统（§3.1）；Nexo/Kraken 文案惯例（§4.1）；行业白名单三件套与 WaaS 能力（§5.1/5.2） |
| **Differentiate（保留/放大）** | ① 年消费阶梯 + 年 cap（竞品全是月 cap/类别 cap/锁仓订阅分档，"无锁仓无订阅、基础档 2% 即行业上沿"是对 Nexo/Crypto.com 的直接打点）；② **信用卡质押品独立 APY——行业空白位，主打卖点**（补上 Nexo "spend without selling" 缺的"质押品继续生息"）；③ USDT 返现/奖励记账（vs 竞品平台币波动 + 追回风险）；④ 入金"禁归集 + 隔离 + 废址换新"前置处置；⑤ 白名单登记时过 AML | §3.2/3.3、§4.2（Binance Flexible Loan 定价先例 + Nexo Absent）、§5.2（Binance 地址退役先例） |
| **补缺（设计缺口，落地前必须补）** | ① 白名单新地址冷静期 + 变更/删除对称冷静期（抄 Coinbase 双向 48h）+ 地址数量上限；② AML 误伤申诉通道 + Manual Review 档；③ cashback 跨周期清算流水归属规则（抄 Chase 顺延惯例）；④ staking reward 合规文案（"may receive distributions"，禁写死 APY） | §5.3 两个 Absent 格子；§3 启示 3；§4 启示 4 |
| **校准（数值，非机制）** | staking reward APY 落点建议 1%–2.5% 量级（地板 = Kraken 0.1% 真实收益锚，天花板 = 自家 stake 4% 硬约束）；cashback 营销口径必须带 cap 进度展示，防"名义 3% vs 有效 0.42%"落差被攻击 | §4 启示 3；§3.2 水位判读 |
| **Deprioritize** | 不跟 Crypto.com 的订阅/锁仓分档（与"无锁仓"卖点冲突）；不跟 OKX 式高补贴营销档（13% 限 0.1 XAUT——获客噱头，与黄金卡稳健定位不符）；不学 Falcon 零风险披露 | §3.3、§4.1 |
| **监控清单（建议月度）** | Nexo：是否给质押品开收益（噩梦场景——其已有 NEXO/NETH 例外机制，扩到 PAXG/XAUT 技术上一步之遥）；Nexo Earn 黄金档利率变动；Crypto.com 美国信用卡 rewards 条款迭代；WaaS/Cobo Screening 处置档位更新 | §4.2、既有简报 §7.6 |

---

## 7 · 信息缺口与置信度声明

以下为明确未查到/未公开项，简报未做推测填充：

| # | 缺口 | 影响 |
|---|---|---|
| 1 | Nexo Card 退款 clawback 条文、pending 交易处理 | §3.3 矩阵相应格子按"未查到"评级 |
| 2 | Nexo Earn 黄金档实时利率（JS 渲染，"up to 5.5%" 经索引 snippet + 第三方互证；美国版黄金档利率未查到） | §4.1 数值以官方索引口径为准，落决策前建议 app 内核对 |
| 3 | Falcon 实时 APR（需 app 内查看）及其收益来源（官方从未披露） | §4.1 标注公告口径 |
| 4 | Kraken 抽佣比例、Binance PAXG 利率官方值（仅第三方口径） | §4.1 已标注 |
| 5 | Coinbase Allowlist 官方页对爬虫 403（经 Google 索引摘要核对）；Kraken hold 时长两处官方文章口径不一（12h vs 24h） | §5.1 已并列标注 |
| 6 | Cobo Screening「命中后禁止归集」的明文表述（Reject-冻结机制等价但措辞未见） | §5.2 已标注；D-2 对接时直接问 WaaS |
| 7 | Discover 退款追回官方条文（仅第三方口径佐证通例） | §3.1 已标注 |

---

## 8 · 主要来源

**我方**：`三条新需求—需求总参考.md`（需求权威）、`需求解读-XAUT-staking-reward.md`（行业数据起点）、`竞品简报-充提还规则-2026-07-22.md`（公司层基本面复用）
**传统卡**：chase.com（Freedom Flex Rewards Agreement / 退款教育页）、citi.com、americanexpress.com（Rewards FAQ）、capitalone.com（Help Center）、discover.com（Cashback Bonus）、stripe.com、marqeta.com、creditcards.com、nerdwallet.com、wallethub.com、thepointsguy.com、ir.usbank.com、cnbc.com、robinhood.com
**Crypto 卡**：nexo.com（crypto-card / 官方博客 / earn-crypto 系列）、nexo.how（官方 KB 镜像）、crypto.com/document/us_credit_card（Program Terms PDF）、help.crypto.com、coinbase.com/legal/creditcard/rewards、help.wirexapp.com
**黄金代币收益**：kraken.com + support.kraken.com、falcon.finance、phemex.com、okx.com help、youhodler.com、bitcompare.net、cefirates.com、gold.tether.to、coindesk.com（Nexo 重返美国）、cointelegraph.com
**白名单/AML**：binance.com support FAQ（白名单 / 地址退役 / withdrawal review / Flexible Loan）、support.kraken.com（提现地址 / On Hold / 第三方入金）、help.coinbase.com、okx.com、bybit.com、developers.fireblocks.com、fireblocks.com、manuals.cobo.com、fatf-gafi.org、elliptic.co、blog.amlbot.com、coindesk.com + cointelegraph.com（dYdX–Tornado Cash 案例）

> 本简报由 AI 依公开信息编制；费率/利率/冷静期类数字时效性强，落决策前建议对 §7 缺口 2/3 做一次 app 内人工核对。
