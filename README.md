## English Summary

These notes examine one question: can computational methods reliably assign new indications to molecules that have already paid for their safety, pharmacokinetics, and manufacturing. The ten pieces move through cost accounting by remaining uncertainty, a four-layer evidence chain linking molecule, selectivity, phenotype, and clinic, transcriptome fingerprint matching, diffusion models for drug-target interaction under cold start, entropy-based selectivity scoring, phenotype validation at cell-line scale, signals from million-person electronic health records, the unit economics of screening versus validation, and a closing falsification boundary. Each claim carries a boundary number together with its definition, a source grade, and the observation that would overturn it. They are written for engineers and researchers evaluating this field, and for anyone who needs to keep retrospective benchmark scores separate from prospective replication rates.

> 中文简介见下 ↓

# AI 老药新用与计算重定位 · 十件研究笔记

一句话定位：这是一组长期跟踪「用计算手段给已上市分子寻找新适应症」这一技术方向写下的研究笔记——记录它的方法论骨架、关键数字与证伪条件，不承诺它一定成立。

## 两性费曼：怎么读这套笔记

每件笔记在标题之后都有两块固定结构，两层互相制约：

- **性一（12 岁也能懂）**——这是什么 / 与我何干 / 我该怎么办。判据是：不允许出现未解释的术语；「与我何干」只允许落在 省钱 / 赚钱 / 避险 / 暂无 四选一，并给出理由；「我该怎么办」必须是一个 ≤2 小时、当天可启动的具体动作。
- **性二（同行可验证）**——机制 / 边界数 / 反例 / 可复现。判据是：不允许出现无法验证的断言；边界数必须裸写来源等级 [一手] / [二手] / [推算]，不得用反引号包裹；反例必须写清「什么证据能推翻这条结论」。

只过一层等于没过：大众层说不清，说明作者没真懂；专业层给不出数，说明大众层只是好听。完整协议见 [FEYNMAN.md](./FEYNMAN.md)。

## 十件笔记索引

| 编号 | 标题 | 一句话 | 关键边界数 | 证据等级 |
|---|---|---|---|---|
| #1 | [第一性原理：为什么「旧分子」是最便宜的创新](./notes/01-first-principles-old-molecules.md) | 研发成本按「还剩多少不确定性」计价，不按「多新」计价；旧分子已为安全性、药代与工艺付过费 | 比新分子实体快 30-50% 获批；临床前到临床从约 10-15 年压缩到 3-6 年（有利情形） | [二手] |
| #2 | [证据链四层：分子-选择-表型-临床的 LinkD 范式](./notes/02-four-layer-evidence-linkd.md) | 把「一个药对一个病成立」拆成四层独立可验证命题，出错时能指出错在哪一层 | 14,981 药 × 20,385 人源靶点；RMSE 0.447-0.699；恢复 95.3%；约 1,150 万人 EHR | [二手] |
| #3 | [转录组指纹匹配：DrugMine 的逆向思维](./notes/03-transcriptome-fingerprint-drugmine.md) | 先比对「病造成的表达偏移」与「药造成的表达扰动」是否反向，再倒推机理 | 约 1.4 万套、20 万组样本转录组数据；首款乳腺增生候选药 2026 年申报临床 | [二手] |
| #4 | [扩散模型做 DTI：LinkD-Bind 的冷启动优势](./notes/04-diffusion-dti-cold-start.md) | DTI 模型的价值不在有历史数据时的精度，而在没有历史数据时的相对稳定性 | 九项基准八项第一；RMSE 0.447-0.699；冷启动条件下增益最大 | [二手] |
| #5 | [选择性的熵：LinkD-Select 恢复 95.3% 已知配对](./notes/05-selectivity-entropy.md) | 选择性是信噪比不是强度，用信息熵把「专一性」变成可计算的连续量 | 恢复 95.3% 已知选择性药物-靶点对；HR 0.82 / 0.92（相对 metoprolol） | [二手] |
| #6 | [表型验证：960 细胞系 + CRISPR 依赖性一致性](./notes/06-phenotype-validation-960-lines.md) | 不预设机理，用「药物敏感性谱」与「基因依赖性谱」的一致性生成机制假设 | 960 种癌细胞系；34 个新药物-基因对；Top50 候选恢复约 85% 已知靶点 | [二手] |
| #7 | [临床信号：1150 万人 EHR 中的 β 受体阻滞剂信号](./notes/07-clinical-signal-11m-ehr.md) | 已上市药物在数百万人身上跑了数十年的自然实验，信号早就在病历里躺着 | 约 1,150 万人 EHR；propranolol HR 0.82、carvedilol HR 0.92（5 年结局） | [二手] |
| #8 | [成本经济学：$50K-$500K/年平台费 vs $1M-$5M/适应症](./notes/08-cost-economics.md) | 软件是月票，验证是过路费，两者之间隔着 20-100 倍杠杆 | $50K-$500K/年 vs $1M-$5M/适应症；首批验证命中约 4-9 个月；市场约 12.6% CAGR（至 2034） | [二手] |
| #9 | [证伪边界：只有 10-30% 的计算预测能在体外重复](./notes/09-falsification-10-30-percent.md) | 回顾性恢复度量的是记忆，前瞻命中度量的是泛化，两者之间是结构性鸿沟 | 前瞻重复率 10-30%；回顾对照 95.3% 与约 85%；单次成功期望成本约 $3M-$50M | [二手] / [推算] |
| #10 | [迁移思维：存量资产重新发现价值](./notes/10-transfer-legacy-assets.md) | 资产价值 = 已验证属性 × 未被开发的场景；一人公司拥有全部存量资产的完整上下文 | 扫描节奏季度级；预测登记簿结算周期 30-90 天；领域侧锚点 10-30% 命中率 | [推算] |

## 这套笔记不主张什么

1. **不主张计算预测等于实验结论：仅 10-30% 的顶级计算预测能在体外重复。** 本套笔记中的所有回顾性指标（恢复 95.3%、Top50 恢复约 85%、九项基准八项第一）都不能被读成前瞻命中率。
2. **不主张「四层证据链对齐」等于因果成立。** 四层可能共享同一个未声明的隐藏假设；观察性 EHR 信号自带适应证混淆（channeling bias），HR 0.82 不等于因果效应。
3. **不主张成本优势等于商业成功。** composition-of-matter 专利封锁、适应症红海、毒性档案与新人群冲突，任何一条都能让省下的成本归零——成本优势是必要条件，不是充分条件。
4. **不主张覆盖度等于适用域。** 14,981 × 20,385 是覆盖率数字；变构结合、蛋白降解剂等机制类别不在其声明范围内，覆盖率不能替代对适用域的说明。
5. **不主张表型一致性等于机制结论。** 药物敏感性与基因依赖性一致至少有三种竞争解释（直接抑制该基因产物、激活合成致死通路、应激导致依赖度被动升高），未经 rescue 或正交实验前它只是假设。
6. **不主张低命中率否定领域价值，也不主张高回顾指标证明领域价值。** 若前瞻重复率在特定任务定义下系统性高于 30%，本套笔记的区间估算需上修；若低到计算筛选的期望值不如随机试错，则「把计算当筛子」这一策略结论被推翻。
7. **不主张任何数字可以脱离定义引用。** 10-30% 会随任务定义（分子层 vs 表型层）、疾病领域与「重复」的判定标准（部分复现 vs 完全复现）显著移动。
8. **不主张存量迁移是免费午餐。** 场景切换成本必须单独计价；扫描必然产出假阳性通胀，若不执行四层过筛与预测结算，重定位会退化成「不断开新坑」，比不做更糟。

## 证据纪律

- [一手]——原始论文、官方技术文档、本人实测得到的数字。
- [二手]——综述、行业分析、公开报道或他人转述中的数字。
- [推算]——由已知数字推导而来，推导过程必须在文中可见。
- **无源数字不得进入对外文本。** 无法标源的数字一律标注为待验，或直接删除。
- 同一事实同时存在一手与二手来源时，以一手为准，并注明两者差异。
- 关键数字必须连同其定义一起引用（含单位、对照条件、判定标准），只报最优值的引用视为无效引用。

## 免责声明

本套笔记为技术分析与写作训练材料，**仅供技术分析，不构成医疗建议**，也不构成用药、诊疗、停药或投资决策的依据。文中出现的药物名、适应症、风险比与细胞实验数据，仅用于说明计算方法与证据结构。任何与健康相关的决定，请咨询具备资质的专业人员并遵循当地监管要求。

## 许可

MIT License。作者署名：chenhz01。

## 同系列笔记（五个领域）

| 领域 | 仓库 | 一句话 |
|---|---|---|
| 神经形态计算与事件驱动传感 | [neuromorphic-event-sensing](https://github.com/chenhz01/neuromorphic-event-sensing) | 永远在线的智能为什么必须抛弃「帧」与全局时钟，以及省 100 倍的前提条件 |
| AI 老药新用与计算重定位 | [ai-drug-repurposing](https://github.com/chenhz01/ai-drug-repurposing) | 用计算手段给已上市分子找新适应症，以及 10-30% 的前瞻重复率边界 |
| 本地优先软件与 CRDT 同步 | [local-first-crdt](https://github.com/chenhz01/local-first-crdt) | 把数据主副本放回用户设备，冲突由数学保证收敛 |
| 后量子密码迁移 | [post-quantum-migration](https://github.com/chenhz01/post-quantum-migration) | 从 Shor 的威胁模型走到一人公司能执行的换算法顺序 |
| 软体与折纸机器人 | [soft-origami-robotics](https://github.com/chenhz01/soft-origami-robotics) | 柔性结构能否把安全、顺应与几何编程做成工程能力 |

五套笔记各自独立，互不依赖，可任选入口进入。

## 费曼速览：60 秒判断这个领域值不值得你花时间

**一句话**：像在已经通关的老游戏里翻隐藏关卡——这个药已经证明过对人体安全，现在只是试试它还能治别的病；哪里不像：游戏里的隐藏关卡是设计好的，而一个药对这种病有效，不代表对下一种也有效——只有 10-30% 的计算预测能在体外重复。

**三个数**：

1. 仅 10-30% 的顶级计算预测能在体外重复 [二手]
2. LinkD-Select 恢复 95.3% 的已知选择性药物-靶点对（回顾性指标，非前瞻命中率）[二手]
3. 商业平台订阅费约 $50K-$500K/年，而每个适应症含验证约 $1M-$5M [二手]

**不成立的三个条件**：

1. 若前瞻重复率在特定任务定义下系统性高于 30%，本套笔记的 10-30% 区间需上修，「把计算当筛子」的期望值结论随之改变。
2. 若回顾性指标（95.3%、Top50 约 85%）被证明与前瞻命中率之间不存在系统性落差，则「回顾指标不能外推」这一核心警告失效。
3. 若 composition-of-matter 专利封锁、适应症红海或新人群毒性档案让省下的成本归零，则「成本优势 → 商业成功」的推论不成立。

这套笔记的每篇都有完整两层费曼拆解，方法见 [FEYNMAN.md](./FEYNMAN.md)。
