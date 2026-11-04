# Rollout Survey · 论文库

> LLM Post-training / RL Rollout 性能优化调研（perpetual-learning 专项）的全部文献 PDF 归档。
> 文献矩阵由 `perpetual-learning/notebook/rollout-performance-research.md` §5 维护，数据截至 2026-10-29。

## 概览

- 条目总数：**126** 篇（arXiv 正文 + 会议论文）
- 已落盘：**126** 篇，合计 **235.6 MB**
- 阅读深度：T1 深读 20 · T2 中等 84 · T3 元数据 21 · 未分级 1
- 抓取时间：2026-11-04（全部 126 篇一次抓取成功，无重试失败项）

| 分组 | 篇数 |
|---|---|
| 5.1 L4 — RL Pipeline / Async / Disaggregation | 45 |
| 5.2 L3 — Long-tail / Scheduling / Cluster | 13 |
| 5.3 L1/L2 — RL-aware Speculative Rollout（serving机制的迁移条件） | 19 |
| 5.4 Agentic Rollout | 29 |
| 5.5 Serving 基础（supporting corpus，含迁移边界，不标 rollout-native） | 20 |

## 命名规范

```
{tier}_{yyyymm}_{ShortTitle}.pdf
T1_202505_AReaL.pdf            # T1 深读，2025 年 5 月投稿
T2_USENIX-ATC24_PUZZLE.pdf     # 非 arXiv，用会议标识占位
NA_202409_HybridFlow-veRL.pdf
```

- `tier`：阅读深度，非质量排名。`T1` 逐篇深读（机制+代码+报值口径核对）、`T2` 中等深度、`T3` 仅元数据 triage、`NA` 未分级（HybridFlow 深读进行中）。
- `yyyymm`：投稿年月四+二位，如 `202505` = 2025 年 5 月。完整 arXiv 编号与版本留在下表 `arXiv / 会议` 列，需要精确溯源查表即可。
- `ShortTitle`：论文简称，取自标题括号前主体，仅保留 ASCII 以保证跨平台安全。
- 同一论文在矩阵中的层级（L1–L4）、类型（A/B/C/supporting）、机制标注见下表，逐字取自 §5。

## 文献索引

### 5.1 L4 — RL Pipeline / Async / Disaggregation

| 文件 | 标题 | arXiv / 会议 | 层级 | 深度 | 类型 | 机制要点 |
|---|---|---|---|---|---|---|
| [`T1_202505_AReaL.pdf`](T1_202505_AReaL.pdf) | AReaL | 2505.24298v5 | L2/L4 | T1 | B（系统；混版本／目标另列） | 生成准入限流与旧样本优先；中断换权重重算KV，decoupled PPO区分behavior／proximal |
| [`T1_202604_DORA.pdf`](T1_202604_DORA.pdf) | DORA | 2604.26256v2 | L2/L3/L4 | T1 | B（跨步集合已核：保留全部样本、K界staleness） | 单轨迹固定版本、旧版本排空后移窗，同版本KV迁移；不丢弃自述非同步等价证明 |
| [`T1_202504_StreamRL.pdf`](T1_202504_StreamRL.pdf) | StreamRL | 2504.15930v1 | L3/L4 | T1 | B（异步语义另列） | stream与ranker调度分治气泡；强制长度吞吐实验与真实收敛分开 |
| [`T2_202507_DistFlow.pdf`](T2_202507_DistFlow.pdf) | DistFlow | 2507.13833v4 | L4 | T2 | B | DAG串行化＋分布式数据面，缓存中间张量非prefix KV；512卡为随batch增长的弱扩展，不当固定job加速 |
| [`NA_202409_HybridFlow-veRL.pdf`](NA_202409_HybridFlow-veRL.pdf) | HybridFlow / veRL | 2409.19256v2 | L4 | NA | B | 混合单／多controller与分层API，3D-HybridEngine用于训练和生成间actor resharding |
| [`T1_202606_RolloutPipe.pdf`](T1_202606_RolloutPipe.pdf) | RolloutPipe | 2606.26997v2 | L3/L4 | T1 | B（过滤／顺序已核：FGD前沿准入重塑完成顺序、dynamic filtering为物化数据准备、轮内无组丢弃） | 固定本轮rollout权重，完整group先送训；每U组更新、全R组消费后发布权重 |
| [`T1_202510_Laminar.pdf`](T1_202510_Laminar.pdf) | Laminar | 2510.12633v1 | L3/L4 | T1 | B（buffer策略已核：可定制API、容量不足才淘汰） | CPU relay与同版本repack；低陈旧性是观测，不是显式硬上限 |
| [`T2_202409_RLHFuse-Stage-Fusion.pdf`](T2_202409_RLHFuse-Stage-Fusion.pdf) | RLHFuse／Stage Fusion | 2409.13221 | L3/L4 | T2 | B | 聚尾KV迁移后提前评分、actor／critic双向流水；保留训前屏障，融合对同栈Base为1.2–1.4×非3.7× |
| [`T2_202406_ReaL.pdf`](T2_202406_ReaL.pdf) | ReaL（v1名ReaLHF） | 2406.14088 | L4 | T2 | B | 函数级mesh／并行度搜索与参数重分配；MCMC不保最优，主测随机prompt强制满长度、20步吞吐非收敛时间 |
| [`T2_202405_OpenRLHF.pdf`](T2_202405_OpenRLHF.pdf) | OpenRLHF | 2405.11143v6 | L2/L4 | T2 | B；**C**（DAPO筛组另列） | Ray／vLLM／ZeRO＋AutoTP／Ring；PDF表1原始时间几何平均复算1.146／1.181／1.393×，不沿用汇总列 |
| [`T2_202308_DeepSpeed-Chat.pdf`](T2_202308_DeepSpeed-Chat.pdf) | DeepSpeed-Chat | 2308.01320v1 | L1/L2/L4 | T2 | A/B；**C**（可选mixture） | Hybrid Engine在TP生成／ZeRO训练间切换并管理内存；Step3吞吐与SFT＋RM＋PPO全流程时间、历史价格分账 |
| [`T2_202506_ROLL.pdf`](T2_202506_ROLL.pdf) | ROLL | 2506.06122v1 | L3/L4 | T2 | B（recipe／过滤另列） | 单controller、ParallelWorker与逐样本reward／env流水；质量曲线不当系统吞吐证据 |
| [`T2_202505_LlamaRL.pdf`](T2_202505_LlamaRL.pdf) | LlamaRL | 2505.24034v2 | L4 | T2 | B（AIPO／FP8另列） | Executor独立并行度与DDMA权重传输；10.7×含异步／并行度／FP8组合，非纯异步收益 |
| [`T3_202606_ASymPO.pdf`](T3_202606_ASymPO.pdf) | ASymPO（证明错误警示） | 2606.03070v3 | L4边界（暂） | T3 | 算法边界，不硬归B/C | current-policy-only的非对称loss尺度归一化；页面注明证明有误、v2曾撤回，见§5.13 |
| [`T2_202605_Missing-Old-Logits-Asynchronous-Agentic-RL.pdf`](T2_202605_Missing-Old-Logits-Asynchronous-Agentic-RL.pdf) | Missing Old Logits in Asynchronous Agentic RL | 2605.12070v3 | L4边界 | T2 | 系统成本／算法修正，mask有效集合已核（§13.10：式3 advantage-sign mask＋阈值交互经 active-token 集合） | 精确old logits分解训推差异与版本陈旧性；EWMA是近似，快照成本窗口有Ray故障终止 |
| [`T2_202410_Asynchronous-RLHF.pdf`](T2_202410_Asynchronous-RLHF.pdf) | Asynchronous RLHF | 2410.18252v3 | L4 | T2 | B；**C**（off-policy／选样） | Cleanba式前一批数据训练与当前权重生成重叠；best/worst、减步数及KL代价分账，非纯调度2.5× |
| [`T2_202510_Defeating-Training-Inference-Mismatch-FP16.pdf`](T2_202510_Defeating-Training-Inference-Mismatch-FP16.pdf) | Defeating the Training-Inference Mismatch via FP16 | 2510.26788v1 | L1/L4边界 | T2 | 数值诊断，不硬归A/B | 训推同时FP16＋loss scaling降低失配，仍有框架尖峰；稳定性非bit-exact或通用精度建议 |
| [`T2_USENIX-ATC24_PUZZLE.pdf`](T2_USENIX-ATC24_PUZZLE.pdf) | PUZZLE | USENIX ATC'24 | L4 | T2 | B | 无依赖模型时分重叠＋训练／生成并行布局转换；短256-token PPO及混栈比较，不外推长链 |
| [`T2_202507_AsyncFlow.pdf`](T2_202507_AsyncFlow.pdf) | AsyncFlow | 2507.01663v2 | L3/L4 | T2 | B（版本／目标已核：延迟一步安装、off-policy契约） | TransferQueue字段／微批流水及延迟一步安装权重；Ascend结果与串行同栈消融分账 |
| [`T2_202609_Rethinking-Training-Inference-Mismatch.pdf`](T2_202609_Rethinking-Training-Inference-Mismatch.pdf) | Rethinking Training-Inference Mismatch（CIS） | 2609.32444v1 | L1/L4边界 | T2 | 算法修正，不硬归B/C | confidence-aware单侧截断并加数值floor；只修观测prefix条件下token分布，有偏非轨迹精确校正 |
| [`T2_202509_RLinf.pdf`](T2_202509_RLinf.pdf) | RLinf | 2509.15965v2 | L3/L4 | T2 | B | M2Flow时空重组与profile计划，GRPO保留整prompt组；空间拆分长序列7B有负收益 |
| [`T2_202605_Diagnosing-Training-Inference-Mismatch.pdf`](T2_202605_Diagnosing-Training-Inference-Mismatch.pdf) | Diagnosing Training Inference Mismatch（VeXact） | 2605.14220v2 | L1/L4边界 | T2 | 诊断；RS已核（§13.10：§4.2 序列级拒绝按 q_t 依赖推理路径概率、改变有效训练集合） | batch-invariant参考隔离TIM，聚合KL可掩盖失败；TIS＋RS近似跟踪非严格目标等价 |
| [`T2_USENIX-OSDI26_DynaRL.pdf`](T2_USENIX-OSDI26_DynaRL.pdf) | DynaRL | USENIX OSDI'26 | L3/L4 | T2 | B | RLinf上动态hypergraph／安全点迁移／多轮优先路由；1.98×为32B对verl，不是对RLinf |
| [`T2_202405_NeMo-Aligner.pdf`](T2_202405_NeMo-Aligner.pdf) | NeMo-Aligner | 2405.01481v2 | L1/L2/L4 | T2 | A/B | TRT-LLM常驻engine原地refit与reshard、PyTriton异步RPC；96→192卡扩展不当固定卡收益 |
| [`T2_202510_ROLL-Flash.pdf`](T2_202510_ROLL-Flash.pdf) | ROLL Flash（Part II） | 2510.11345v1 | L3/L4 | T2 | B；**C**（冗余／过滤已核：§13.10 "terminates rollout once a predefined number of trajectories has been collected"、零方差组过滤） | 样本年龄约束下混版本续跑，queue／env异步；先够数即停的冗余采样另C，5.45×为模拟 |
| [`T3_202512_Each-Prompt-Matters.pdf`](T3_202512_Each-Prompt-Matters.pdf) | Each Prompt Matters（CompassMax-V3-Thinking） | 2512.07710v1 | L1/L3/L4（暂） | T3 | A/B／**C**（联合算法） | FP8 rollout、reward重叠与长度调度；另有zero-variance过滤、ESPO及MoE Router Replay |
| [`T2_202606_PrefixFlow.pdf`](T2_202606_PrefixFlow.pdf) | PrefixFlow | 2606.01143v4 | L4训练侧边界 | T2 | B（训练调度） | prefix前向一次、suffix累积gK/gV后延迟反向；长prefix全步1.90×来自actor更新非生成 |
| [`T2_202608_psRL.pdf`](T2_202608_psRL.pdf) | psRL | 2608.25683v1 | L4训练侧边界 | T2 | B（训练调度；训练端优化已核纳入本专项边界跟踪，与生成侧prefix cache分开） | 静态样本的prefix共享、token微批与KV生命周期；update至5.2×、整RL迭代至2.1×分账 |
| [`T2_202605_SparseRL-Sync.pdf`](T2_202605_SparseRL-Sync.pdf) | SparseRL-Sync | 2605.07330v1 | L4 | T2 | B | post-cast索引／新值覆盖；106B同步延迟实测、671B外推，约100×压缩不当全训练加速 |
| [`T2_202508_GLM-4.5.pdf`](T2_202508_GLM-4.5.pdf) | GLM-4.5报告（Slime §3.5） | 2508.06471v1 | L1/L3/L4 | T2 | A/B；**C**（数值／任务过滤） | Megatron／SGLang-Router／Data Buffer两种模式；在线block FP8与task过滤另列，无独立系统计时 |
| [`T1_202601_StaleFlow.pdf`](T1_202601_StaleFlow.pdf) | StaleFlow | 2601.12784v2 | L3/L4 | T1 | B；冗余／过滤另C（已核：冗余中止+主动过滤双C、主评估禁用） | Reserve／Occupy／Consume界定轨迹年龄，TS／PS协调路由；主评估禁用冗余 |
| [`T2_202602_PULSE.pdf`](T2_202602_PULSE.pdf) | PULSE（Understanding and Exploiting Weight Update Sparsity） | 2602.03839v2 | L4／训练通信边界 | T2 | B（Sync）；LoCo优化另列 | Sync覆盖BF16新值，LoCo发送FP32 pseudo-gradient并留误差反馈；130×为上传字节比，非同一通道／算法精确等价 |
| [`T2_202507_MindSpeed-RL.pdf`](T2_202507_MindSpeed-RL.pdf) | MindSpeed RL（Ascend平台） | 2507.19017v1 | L3/L4 | T2 | B | transfer dock与allgather-swap；主16 NPU、按设备归一TPS含prompt，不外推CUDA或384卡主测 |
| [`T1_202601_FP8-RL.pdf`](T1_202601_FP8-RL.pdf) | FP8-RL | 2601.18150v2 | L1/L2/L4 | T1 | A/B；**C**（数值policy／TIS） | 真FP8 policy／KV及每步scale刷新；token TIS不保证BF16轨迹等价，MoE长跑仍有崩溃（已核：式3 clip阈值C=2、MIS/RRR为建议非实证） |
| [`T2_202610_NeMo-DCR.pdf`](T2_202610_NeMo-DCR.pdf) | NeMo-DCR | 2610.08430v1 | L4 | T2 | B | canonical投影／native loader放置，XOR去重及overwrite恢复／joint commit；refit等价不含KV，延迟压测不当1T训练 |
| [`T2_202610_PivotRL.pdf`](T2_202610_PivotRL.pdf) | PivotRL（Efficient Resource-Shiftable RL Post-training with Generative Rewards） | 2610.16866v1 | L3/L4 | T2 | B | GPS／role context切换＋保token重prefill迁移；planner模拟不等主评估模拟，收益限含GenRM三角色 |
| [`T2_202610_ThunderSyncRL.pdf`](T2_202610_ThunderSyncRL.pdf) | ThunderSyncRL | 2610.05935v1 | L4 | T2 | B | 固定快照先累计reward加权／未加权score梯度，整组归一、整批一次更新；数学等价不当bit-exact |
| [`T2_202512_RobustRL.pdf`](T2_202512_RobustRL.pdf) | RobustRL（Role-Based Fault Tolerance System for LLM RL Post-Training） | 2512.22492v1 | L4 | T2 | B（容错） | 角色恢复／同构rollout备用／UCX relay；每10步注入trainer故障，CPU管理角色未获容错 |
| [`T2_202602_Beyond-Precision.pdf`](T2_202602_Beyond-Precision.pdf) | Beyond Precision | 2602.01826v1 | L1/L4边界 | T2 | 优化诊断；数据过滤已核（§13.10：附录C "Rejection sampling: True (Ignore the samples with all 0/1 rewards)"） | 长度激增时点指导定期LR减半；1.8×为启发式，稳定性改善不等引擎精确一致或系统提速 |
| [`T2_202509_PipelineRL.pdf`](T2_202509_PipelineRL.pdf) | PipelineRL | 2509.19128v2 | L2/L4 | T2 | B；**C**（mixed-policy／旧KV） | in-flight换权重保留旧KV，ESS近似on-policy非零陈旧；约2×对模拟并修正的Conventional RL基线 |
| [`T1_202602_ECHO-2.pdf`](T1_202602_ECHO-2.pdf) | ECHO-2 | 2602.02192v6 | L3/L4 | T1 | B／**C**（过旧丢弃） | overlap容量约束下按c/μ激活worker，分块链式广播与版本窗口丢弃；优化cost–quality |
| [`T1_202609_PACE.pdf`](T1_202609_PACE.pdf) | PACE（Where Does Staleness Accumulate?） | 2609.36830v1 | L4 | T1 | B／**C**（拒收） | pool超额占用→EMA拒收预算，Kwait＋相对prefix漂移加权Kgen排序；不拒绝在途轨迹 |
| [`T2_202510_RLBoost.pdf`](T2_202510_RLBoost.pdf) | RLBoost | 2510.19225v3 | L3/L4 | T2 | B | 自适应partial seeding／pull权重／token迁移；主吞吐为真实trace在按需卡回放，租价估算非spot账单 |
| [`T2_202507_Long-RL.pdf`](T2_202507_Long-RL.pdf) | Long-RL（Scaling RL to Long Videos／MR-SP） | 2507.07966v4 | L1/L2/L4 | T2 | A/B（MR-SP）；数据筛选C | 分片视觉编码与embedding复用、policy／reference prefill用SP；8 A100的2.1×是forward窗口，非全步 |
| [`T2_202604_Relax.pdf`](T2_202604_Relax.pdf) | Relax（Omni-Modal Post-Training） | 2604.11554v2 | L3/L4 | T2 | B（异步语义已核：producer侧staleness门控s<max_staleness、单一参数覆盖全谱；门控off-by-one与1.20× on/off-policy口径不一致保留） | TQ字段／microbatch流水与producer门控、encoder布局；Actor／Rollout等故障仍全局重启，隐藏计算非免费 |

### 5.2 L3 — Long-tail / Scheduling / Cluster

| 文件 | 标题 | arXiv / 会议 | 层级 | 深度 | 类型 | 机制要点 |
|---|---|---|---|---|---|---|
| [`T1_202509_RollPacker.pdf`](T1_202509_RollPacker.pdf) | RollPacker | 2509.21009v1 | L3/L4 | T1 | B／**C**（响应选择／评分预算；跨round重排不改变样本集合为原文自述） | tail batching延后prompt但短轮保留最快响应；流式算梯度、整轮后更新，自适应sandbox超时另改奖励边界 |
| [`T1_202603_SortedRL.pdf`](T1_202603_SortedRL.pdf) | SortedRL | 2603.23414v1 | L3/L4 | T1 | B／**C**（批构成／更新次序） | 长度分批且清空prompt池后再补新；on-policy重跑未完响应，partial续token并缓存行为logp；micro-curriculum |
| [`T1_202605_DARTS.pdf`](T1_202605_DARTS.pdf) | DARTS | 2605.30859v1 | L3/L4 | T1 | B／**C**（主动塑形） | 7短＋1长组选择、预算饱和改shortest-only；token流水仅前向提前，整轨迹后奖励／反向 |
| [`T2_202511_Seer.pdf`](T2_202511_Seer.pdf) | Seer | 2511.14617v3 | L1/L2/L3 | T2 | A/B | chunk与共享KV池、组内probe调度＋suffix推测；尾时段是最后10%请求，不是P95 |
| [`T2_202607_WAR.pdf`](T2_202607_WAR.pdf) | WAR | 2607.17299v1 | L1/L3 | T2 | A/B（超时边界另核→已核：TTL到期终止轨迹产生incomplete rollout） | 低负载suffix draft、高负载TTL／cache调度；计时筛选工作量差≤5%的运行，小batch调度可负收益 |
| [`T2_202506_Infinite-Sampling.pdf`](T2_202506_Infinite-Sampling.pdf) | Infinite Sampling | 2506.22950v1 | L2/L3/L4 | T2 | B（dynamic-slot集合另列） | 固定slot复用KV与SJF refill、全组reward后反向；decode rounds不当墙钟，FPTAS保证疑点保留（算法2无近似证明，见§6.23.6） |
| [`T2_202608_TailSieve.pdf`](T2_202608_TailSieve.pdf) | TailSieve | 2608.22788v2 | L1/L3/L4 | T2 | A/B；**C**（跨轮集合） | 当前policy重生成旧尾组＋本轮最早bulk组，稳态无额外长度偏差不等每批集合不变 |
| [`T2_202605_ExpThink.pdf`](T2_202605_ExpThink.pdf) | ExpThink（CoT塑形边界） | 2605.07501v2 | workload／算法边界 | T2 | **C**（reward／advantage） | 每题最短正确长度统计塑形、按正确数缩放优势；77.18%是平均相对token减量，非同工作量提速 |
| [`T2_202608_Scheduling-Mixed-RL-Rollouts.pdf`](T2_202608_Scheduling-Mixed-RL-Rollouts.pdf) | Scheduling Mixed RL Rollouts（MISA-T） | 2608.11152v1 | L3 | T2 | B（准入；混合条件另列） | KV block-time类预算＋active/pending准入；50迭代全步降22.8%，混合TV仍2.71个百分点非零 |
| [`T2_202606_CERO.pdf`](T2_202606_CERO.pdf) | CERO（Cross-Epoch Adaptive Rollout Optimization） | 2606.05606v1 | workload／算法边界 | T2 | **C**（预算分配） | posterior信息代理＋跨epoch预算价格决定采样，固定utility regret不等非平稳实现保证；保留7B负项 |
| [`T2_USENIX-OSDI26_Weave.pdf`](T2_USENIX-OSDI26_Weave.pdf) | Weave（Efficient Co-Scheduling for Disaggregated RL Post-Training） | USENIX OSDI'26 | L3/L4 | T2 | B | 跨job组内交错＋host warm切换；局部最优有负载前提，trace回放成本与模拟近最优分账 |
| [`T2_202609_CadenceRL.pdf`](T2_202609_CadenceRL.pdf) | CadenceRL（Reshaping Rollout Workloads） | 2609.36899v1 | L2/L3/L4 | T2 | B（消费／年龄另核→已核：credit模型η界非逐轨迹上限、置换轨迹保留并原版本恢复） | 同版本pending续跑与聚尾，credit非逐轨迹年龄界；跨硬件KV不兼容时re-prefill |
| [`T1_202509_APRIL.pdf`](T1_202509_APRIL.pdf) | APRIL | 2509.18521v3 | L3/L4 | T1 | **C**（混版本；主评估即用续跑） | 超发后按完整prompt组收齐配额，未完token跨版本续跑为主评估机制；"不丢轨迹"指未完成续跑，非采样集合不变 |

### 5.3 L1/L2 — RL-aware Speculative Rollout（serving机制的迁移条件）

| 文件 | 标题 | arXiv / 会议 | 层级 | 深度 | 类型 | 机制要点 |
|---|---|---|---|---|---|---|
| [`T1_202510_ReSpec.pdf`](T1_202510_ReSpec.pdf) | ReSpec | 2510.26475v1 | L1/L2/L4 | T1 | A（验证契约已核） | 按active batch选SD配置；reward加权的是drafter KD，非直接actor目标，分布偏差归因留疑 |
| [`T1_202511_DAS.pdf`](T1_202511_DAS.pdf) | DAS（Beat the long tail） | 2511.13841v1 | L1/L3 | T1 | A/B（验证契约已核；式7与式6代数不一致保留） | 近期同题后缀树＋动态长短draft预算；PDF式7与约束代数不一致，不据此断言实现错误 |
| [`T2_202509_SPEC-RL.pdf`](T2_202509_SPEC-RL.pdf) | SPEC-RL | 2509.23232v3 | L1/L2 | T2 | A/C | 历史轨迹作draft；默认lenience改采样分布，Algorithm 1拒绝后直接采当前policy亦不足证ℓ=1精确 |
| [`T1_202605_BubbleSpec.pdf`](T1_202605_BubbleSpec.pdf) | BubbleSpec | 2605.08862v1 | L1/L3/L4 | T1 | A/B（精确验证条件） | 仅用inter-GPU气泡预生成下一批draft；逐token接受／残差校正＋统一短query attention |
| [`T2_202512_RLHFSpec.pdf`](T2_202512_RLHFSpec.pdf) | RLHFSpec | 2512.04752v2 | L1/L3/L4 | T2 | A/B | profile选树与固定请求迁移；局部TPS非全局makespan最优 |
| [`T1_202606_EfficientRollout.pdf`](T1_202606_EfficientRollout.pdf) | EfficientRollout | 2606.18967v2 | L1/L2 | T1 | A（验证／KV契约；一次开启自述已核，无continuous batching延伸） | 每步RTN W4 draft＋roofline一次开启＋跨步调长度；target覆盖临时KV并拒绝校正 |
| [`T2_202511_TLT.pdf`](T2_202511_TLT.pdf) | TLT（Taming the Long-Tail） | 2511.16665v3 | L1/L2/L3/L4 | T2 | A/B | 尾部Spot Trainer＋分桶graph/MAB；三步计时用已训draft，旧长响应只训draft不改actor屏障 |
| [`T2_202601_SRT.pdf`](T2_202601_SRT.pdf) | SRT | 2601.09083v1 | L1/L3 | T2 | A/B | per-prompt树由当前policy验证；主计时无run-ahead，维护增益模拟与实测分账 |
| [`T2_202606_Bebop.pdf`](T2_202606_Bebop.pdf) | Bebop（Breaking Entropy Bounds） | 2606.12370v1 | L1/L4 | T2 | A | 预RL多步TV＋拒绝采样；熵单调性非一般定理，默认无需online MTP |
| [`T2_202509_FastGRPO.pdf`](T2_202509_FastGRPO.pdf) | FastGRPO | 2509.21792v1 | L1/L2/L4 | T2 | A/B | profile并发配置＋在线draft；总收益与AR提前退出对照分账 |
| [`T1_202508_RhymeRL.pdf`](T1_202508_RhymeRL.pdf) | RhymeRL（History Rhymes） | 2508.18588v1 | L1/L2/L3/L4 | T1 | A/B／**C**（跨步集合） | reward-aware历史树只供draft验证；HistoPipe一步陈旧及跨步迁移已核（§13.9：一步陈旧为引用式证明、跨步迁移明确改训练步归属），不混同步等价 |
| [`T2_202604_System-Integrated-SD.pdf`](T2_202604_System-Integrated-SD.pdf) | System-Integrated SD（Iso等） | 2604.26779v1 | L1/L4 | T2 | A/B | 固定EAGLE3实测与大规模预测分开，接受长度不等速度 |
| [`T2_202609_Online-Draft-Co-Training.pdf`](T2_202609_Online-Draft-Co-Training.pdf) | Online Draft Co-Training（Large-Scale Long-Context RL） | 2609.07108v1 | L1/L2/L4 | T2 | A/B | stop-gradient共训，branch CP与TapChannel；256K算子／RL全步分账 |
| [`T2_202610_S-R.pdf`](T2_202610_S-R.pdf) | S³R | 2610.13202v1 | L1/L2/L4 | T2 | A/B（稀疏直采对照已核：§13.10 附录C.5 "Direct sparse + mismatch correction"消融、摘要逐字确认不稳定） | sparse draft＋dense KV重算／残差采样，完整组同快照；KV压力限流，稀疏读不减完整KV存储 |
| [`T2_202610_NgramRL.pdf`](T2_202610_NgramRL.pdf) | NgramRL | 2610.32881v1 | L1/L2 | T2 | A/B | 近期per-prompt LUT＋验证结果分支预取；TPS为token/forward，1.38×相对同步预取基线非全训练 |
| [`T2_202608_SPARK.pdf`](T2_202608_SPARK.pdf) | SPARK（v1名SpecRoll） | 2608.04962v2 | L1/L2 | T2 | A | prompt-local反馈仅改draft；精确分布依赖因果q与独立验证随机数 |
| [`T2_202610_ScaleRLT.pdf`](T2_202610_ScaleRLT.pdf) | ScaleRLT | 2610.34427v1 | L1/L2/L4 | T2 | A/B；**C**（混合深度读出） | 固定读出下defer深层计算、schedule概率值＋dense梯度；改原生读出有质量负项，bitwise非梯度保证 |
| [`T2_202511_SpecActor.pdf`](T2_202511_SpecActor.pdf) | SpecActor（Fast LLM Post-training） | 2511.16193v3 | L1/L3/L4 | T2 | A/B（验证契约已核：exact matching+target逐路径验证、FoN为draft方法选择、算法等价为断言非证明） | 解耦窗口＋draft ladder／FoN；抽样trace计时非连续完整训练 |
| [`T2_ACL-Findings26_MTP-RL.pdf`](T2_ACL-Findings26_MTP-RL.pdf) | MTP-RL（Acceleration of Reinforcement Learning Rollouts with Policy-Aligned Multi-Token Prediction） | ACL Findings'26 | L1/L4 | T2 | A；**C（主模型辅助目标）** | 优势门控KL仅限MTP项，但辅助loss回传主模型，非纯目标不变加速 |

### 5.4 Agentic Rollout

| 文件 | 标题 | arXiv / 会议 | 层级 | 深度 | 类型 | 机制要点 |
|---|---|---|---|---|---|---|
| [`T2_202603_ProRL-Agent.pdf`](T2_202603_ProRL-Agent.pdf) | ProRL Agent | 2603.18815v1 | L3/L4 | T2 | B/C | HTTP生命周期／三阶段池／token透传；DAPO过滤、错误样本排除与足数取消另C（已核 §13.11：零方差组过滤+错误排除梯度+Early Termination 足数取消，未完跨迭代保留、原文不声称 exactly-once），接口非exactly-once保证 |
| [`T1_202603_Heddle.pdf`](T1_202603_Heddle.pdf) | Heddle | 2603.28101v1 | L3/L4 | T1 | B（采样已核不丢弃；版本语义原文未给定待源码） | 长轨迹优先抢占、排序DP放置、工具间隙迁KV＋MP退火；最优性限代理假设、开销非总能全藏 |
| [`T2_202512_RollArt.pdf`](T2_202512_RollArt.pdf) | RollArt（v1摘要名RollArc） | 2512.22560v2 | L2/L4 | T2 | B；**C**（过旧逐出／冗余终止） | 静态domain硬件映射、轨迹异步与serverless；混卡／外部reward成本、逐出及混版本续跑分别核 |
| [`T2_202511_SkyRL-Agent.pdf`](T2_202511_SkyRL-Agent.pdf) | SkyRL-Agent | 2511.16108v1 | L4 | T2 | B；**C（recipe／梯度屏蔽）** | 三阶段队列重叠与工具recipe分账；超外部限制轨迹不算梯度 |
| [`T2_202605_AstraFlow.pdf`](T2_202605_AstraFlow.pdf) | AstraFlow | 2605.15565v1 | L4 | T2 | B；**C**（可组合数据策略） | dataflow／RaaS／trainer解耦；2.74×双policy迭代对verl，弹性省GPU-hours不等墙钟更快 |
| [`T1_202610_ObsSpec.pdf`](T1_202610_ObsSpec.pdf) | ObsSpec | 2610.15246v1 | L4 | T1 | A/B（精确匹配／执行门槛；lossless限机制范围已核） | observation匹配才保留预生成动作、核验后才顺序执行；WM离线重放训练，额外占8卡 |
| [`T2_202510_EARL.pdf`](T2_202510_EARL.pdf) | EARL | 2510.05943v1 | L4 | T2 | B | rollout间切TP与reference-logp直接分发；局部9.7–11.2×非全流程 |
| [`T2_202603_AcceRL.pdf`](T2_202603_AcceRL.pdf) | AcceRL（VLA边界） | 2603.18464v3 | L4 | T2 | B／**C** | 三段解耦B；GIPO／延迟norm／WM另C，离线轨迹1000／2000冲突已确认（摘要/图5 1000 vs §6.4 2000，v3未调和） |
| [`T2_202608_Rollplex.pdf`](T2_202608_Rollplex.pdf) | Rollplex | 2608.14498v1 | L2/L4 | T2 | B（主同步路径） | 同snapshot的prefix–decode重叠，保autograd与全局update屏障；仅兼容布局共享 |
| [`T2_202606_AsyncWebRL.pdf`](T2_202606_AsyncWebRL.pdf) | AsyncWebRL | 2606.05597v3 | L4 | T2 | B／**C** | 常驻pool／截图引用B；step归一化、混合结果组过滤C，联合吞吐非等工作量 |
| [`T2_202608_Agent-Lightning-v1.0.pdf`](T2_202608_Agent-Lightning-v1.0.pdf) | Agent Lightning v1.0（2026） | 2608.17528v1 | L4 | T2 | B／**C** | exact-token-prefix合样本；rollout级adv／token-mean另C（已核 §13.11：组级 advantage+式16 rollout 内 token 池后跨 rollout 均匀平均，改变梯度归一化非轨迹集合→归训练侧边界；选样 SWEsmith 清理/难度筛选 C），非无条件on-policy |
| [`T2_202602_AReaL-DTA.pdf`](T2_202602_AReaL-DTA.pdf) | AReaL-DTA（训练侧边界） | 2602.00482v2 | L1/L3/L4（训练侧） | T2 | A/B（训练侧） | DFS共享前后向＋连续分区；局部训练8.31×不作全步，低共享可负益 |
| [`T2_202610_FlashNext-RL.pdf`](T2_202610_FlashNext-RL.pdf) | FlashNext-RL | 2610.28998v1 | L1/L4 | T2 | A/B；**C（冻结层／零优势组）** | offload与模型kernel优化；开发配置变更／回放与live run分开 |
| [`T2_202605_MARLaaS.pdf`](T2_202605_MARLaaS.pdf) | MARLaaS | 2605.08527v1 | L4 | T2 | B | 跨租户LoRA交错、租户内等新版本；十任务makespan不等单任务迭代 |
| [`T2_202605_ROSE.pdf`](T2_202605_ROSE.pdf) | ROSE | 2605.06534v2 | L3/L4 | T2 | B；DAPO选样另列 | serving优先共享及turn重路由，零差COO传权重；扩资源收益不当固定卡效率 |
| [`T2_202508_Agent-Lightning.pdf`](T2_202508_Agent-Lightning.pdf) | Agent Lightning（2025） | 2508.03680v1 | L4 | T2 | B／**C** | 执行／训练接口B；逐调用credit与AIR shaping C，无受控吞吐表 |
| [`T2_202605_InfoTree.pdf`](T2_202605_InfoTree.pdf) | InfoTree（Rollout Informativeness） | 2605.05262v1 | L3/L4 | T2 | **C**／B | 树选样／预算C；过期评分重验B，时间到目标非每步更快；理论前提已核（Theorem 1塌缩持续、Proposition 2次模(1−1/e)、UUCB一阶展开紧致性依赖经验验证） |
| [`T2_202608_TideRL.pdf`](T2_202608_TideRL.pdf) | TideRL | 2608.10402v1 | L2/L3/L4 | T2 | B；**C（弹性batch）** | CTB／ready驱动ref-actor与rank重分配；跨版本重prefill，变批大小非固定更新集合 |
| [`T2_202609_QwenGyre.pdf`](T2_202609_QwenGyre.pdf) | QwenGyre | 2609.33848v2 | L3/L4 | T2 | B／**C** | cell弹性与整批一次update；角色路径cap／共享target mask／部分评分另C（已核 §13.11：Jmax=5 角色优先级上限、无有效 reward 排除+部分成果可评有效分、execution 内 token 均值再跨 execution 平均；D2 已闭合共享 target 只训一次） |
| [`T2_202607_Rollout-Infrastructure-Tax-Coding-Agent-Reinforc.pdf`](T2_202607_Rollout-Infrastructure-Tax-Coding-Agent-Reinforc.pdf) | The Rollout Infrastructure Tax in Coding-Agent Reinforcement Learning | 2607.01415v1 | L4环境执行边界 | T2 | B（成本测量） | 四底座固定命令测启动／执行／reset；1.8×外推worker-hours不含推理／reward／训练，非完整RL加速 |
| [`T2_202603_ARL-Tangram.pdf`](T2_202603_ARL-Tangram.pdf) | ARL-Tangram | 2603.13019v1 | L3/L4 | T2 | B（工具语义已核：pytest并行无等义验证、警示保留） | action级弹性与CPU／GPU／API池；平均ACT不等RL尾延迟，pytest并行需核等义 |
| [`T2_202607_SAO.pdf`](T2_202607_SAO.pdf) | SAO（Single-Rollout Asynchronous Optimization for Agentic Reinforcement Learning） | 2607.07508v1 | L4／算法边界 | T2 | **C**／算法改动 | 128题×1对GRPO16题×8；DIS区间外mask、critic双更新与冻结attention，非吞吐等价优化 |
| [`T2_202609_PEARL.pdf`](T2_202609_PEARL.pdf) | PEARL | 2609.35158v1 | L2/L3/L4 | T2 | B | PD配置＋训练卡借用、增量切换；四rollout卡强拆PD负项及非零交接成本保留 |
| [`T2_202608_MCP-Universe-RL.pdf`](T2_202608_MCP-Universe-RL.pdf) | MCP-Universe RL（MCP-U RL） | 2608.22167v1 | L3/L4 | T2 | B（placement非严格on-policy） | Acquire／Run／Eval限并发；3 H200五步局部测量，2.8×与2×异步placement分账 |
| [`T2_202510_AgentRL.pdf`](T2_202510_AgentRL.pdf) | AgentRL | 2510.04206v1 | L4 | T2 | B；**C**（cross-policy／task-norm） | 有界队列／动态batch与环境生命周期；主动逐action混新旧policy、task内token归一不当被动同步延迟 |
| [`T2_202608_Belayer.pdf`](T2_202608_Belayer.pdf) | Belayer | 2608.14635v2 | L4 | T2 | B（容错） | 独立server持有权重／raw KV空间，shadow健康检查后重建请求KV；容器本地prefix一致恢复非外部exactly-once |
| [`T2_202602_FlexMARL.pdf`](T2_202602_FlexMARL.pdf) | FlexMARL | 2602.09578v1 | L3/L4 | T2 | B | store驱动micro-batch梯度缓存、整批统一更新；7.3×对naive而非MARTI |
| [`T2_202610_BIRD-RL.pdf`](T2_202610_BIRD-RL.pdf) | BIRD-RL | 2610.29426v1 | L4 | T2 | B/C（系统与curriculum） | trajectory绑定数据库副本、终态释放与脏感知lazy reset；12.8×是在线生命周期I/O降幅 |
| [`T2_202509_DART.pdf`](T2_202509_DART.pdf) | DART（DART-GUI） | 2509.23866v1 | L3/L4 | T2 | B／**C** | 逐worker换权重B；动态频率／长度、成功回放、高熵step及步数加权adv另C（已核 §13.11：">0.6 降频率"、全败组成功池补旧样本、历史成功长度截断、top-80% 高熵 step、组内全 step 归一） |

### 5.5 Serving 基础（supporting corpus，含迁移边界，不标 rollout-native）

| 文件 | 标题 | arXiv / 会议 | 层级 | 深度 | 类型 | 机制要点 |
|---|---|---|---|---|---|---|
| [`T3_USENIX-OSDI22_Orca.pdf`](T3_USENIX-OSDI22_Orca.pdf) | Orca | USENIX OSDI'22 | Serving | T3 | supporting | iteration-level scheduling及selective batching；本行用会议记录，不沿用错配arXiv ID |
| [`T3_202309_vLLM-PagedAttention.pdf`](T3_202309_vLLM-PagedAttention.pdf) | vLLM / PagedAttention | 2309.06180v1 | Serving | T3 | supporting | 分页KV及共享；并发收益仍取决于rollout布局／驻留，不等减少每步最长响应 |
| [`T3_202205_FlashAttention.pdf`](T3_202205_FlashAttention.pdf) | FlashAttention | 2205.14135v2 | Serving | T3 | supporting | IO-aware精确attention；kernel加速不等整条rollout makespan收益 |
| [`T3_202501_FlashInfer.pdf`](T3_202501_FlashInfer.pdf) | FlashInfer | 2501.01005v2 | Serving | T3 | supporting | 面向不同KV布局、可定制attention与负载均衡；整引擎收益另核（已核 §13.11：SGLang v0.3.4/MLC-Engine 接入端到端 ITL/TTFT 指标+kernel 级利用率分层报告、vLLM 无端到端数据、训练前向-only） |
| [`T3_202312_SGLang-RadixAttention.pdf`](T3_202312_SGLang-RadixAttention.pdf) | SGLang / RadixAttention | 2312.07104v2 | Serving | T3 | supporting | 结构化程序与KV复用；迁到rollout仍需核工作负载与版本约束 |
| [`T3_202401_DistServe.pdf`](T3_202401_DistServe.pdf) | DistServe | 2401.09670v3 | Serving | T3 | supporting | prefill/decode分离、TTFT／TPOT约束goodput；不是trainer/rollout分离的同义词 |
| [`T3_202407_Mooncake.pdf`](T3_202407_Mooncake.pdf) | Mooncake | 2407.00079v5 | Serving | T3 | supporting | KV中心化PD分离及过载拒绝；rollout不能无条件继承请求丢弃语义 |
| [`T3_202307_FlashAttention-2.pdf`](T3_202307_FlashAttention-2.pdf) | FlashAttention-2 | 2307.08691v1 | Serving | T3 | supporting | 改work partitioning与并行度；与FA1分开计数，不当RL-native论文 |
| [`T3_202411_SuffixDecoding.pdf`](T3_202411_SuffixDecoding.pdf) | SuffixDecoding | 2411.04975v3 | Serving | T3 | supporting | suffix tree缓存prompt／历史输出，适配推测长度；RL跨步／跨worker复用已核为原文无该语义（§13.9），历史draft须当前target验证 |
| [`T3_202401_EAGLE.pdf`](T3_202401_EAGLE.pdf) | EAGLE | 2401.15077v3 | Serving | T3 | supporting | feature-level草稿预测；只是原始EAGLE，不混作EAGLE-2／3或RL drafter适配 |
| [`T3_202211_Fast-Inference-Transformers.pdf`](T3_202211_Fast-Inference-Transformers.pdf) | Fast Inference from Transformers（Leviathan等） | 2211.17192v2 | Serving | T3 | supporting | 近似模型draft与target验证／修正采样；输出分布正确性须核实现前提 |
| [`T3_202302_Speculative-Sampling.pdf`](T3_202302_Speculative-Sampling.pdf) | Speculative Sampling（Chen等） | 2302.01318v1 | Serving | T3 | supporting | 修改拒绝采样以保target分布，原文带hardware numerics条件；非bitwise相同承诺 |
| [`T3_202403_Sarathi-Serve.pdf`](T3_202403_Sarathi-Serve.pdf) | Sarathi-Serve | 2403.02310v3 | Serving | T3 | supporting | chunked prefill与stall-free混批；需按真实prefill/decode构成重评，不假定rollout prompt占比高 |
| [`T3_USENIX-ATC24_CachedAttention.pdf`](T3_USENIX-ATC24_CachedAttention.pdf) | CachedAttention | USENIX ATC'24 | Serving | T3 | supporting | 分层KV、逐层预载／异步保存及scheduler-aware换入换出；context-window有效性不等policy更新后缓存仍有效 |
| [`T3_202601_CONCUR.pdf`](T3_202601_CONCUR.pdf) | CONCUR | 2601.22705v1 | Serving | T3 | supporting | agent级准入与cache信号控制并发、缓解middle-phase thrashing；RL整组／陈旧性已核为原文无该语义（§13.9），准入为暂停非丢弃 |
| [`T2_202408_Intelligent-Router.pdf`](T2_202408_Intelligent-Router.pdf) | Intelligent Router | 2408.13510v2 | Serving | T2 | supporting | 输出长度桶＋硬件profile引导DQN向同模型实例路由／等待；11.43%是E2E延迟改善，非TPS或RL rollout收益 |
| [`T3_202511_Continuum.pdf`](T3_202511_Continuum.pdf) | Continuum | 2511.02230v7 | Serving | T3 | supporting | 工具等待中按reload／排队成本设置KV TTL并结合program-level FCFS；TTL到期是缓存回收，不等轨迹终止 |
| [`T3_202603_Helium.pdf`](T3_202603_Helium.pdf) | Helium（Efficient LLM Serving for Agentic Workflows） | 2603.16104v1 | Serving | T3 | supporting | workflow视作query plan，proactive缓存与cache-aware调度；缓存仅限确定性/greedy算子，随机采样独立性未提供（§13.3） |
| [`T3_202401_Medusa.pdf`](T3_202401_Medusa.pdf) | Medusa | 2401.10774v3 | Serving | T3 | supporting | 多预测头与树状验证；backbone是否冻结及接受规则不可混用 |
| [`T3_202602_Aurora.pdf`](T3_202602_Aurora.pdf) | Aurora（When RL Meets Adaptive Speculative Training） | 2602.06932v5 | Serving | T3 | supporting | 由live inference traces在线RL训练speculator、热换更新；不是post-training policy rollout系统 |

## 缺失清单

无。全部条目均已落盘。

## 使用约定

- **引用前核对版本**：本目录保存抓取时点的最新版，矩阵里的页数/公式/报值以文中标注的版本为准（`§5` 记有首次与最新版日期）。
- **非 arXiv 条目**来自 USENIX / ACL Anthology 官方 PDF，与 arXiv 版可能存在排版与附录差异。
- **tier 不是质量评价**：T3 只做过元数据 triage，不代表论文价值低；`§5` 的 `conf` 字段记录了是否核过 primary 元数据。
- **已知拼写差异**：`T3_202511_Continuum.pdf` 文件名沿用矩阵标题，PDF 原文题作「Continnum」（双 n），检索时两种写法都试。
- 新增文献时：先在 `§5` 立项（分配 section / layer / tier），再按同一命名规范落盘并更新本 README 的索引表。

## 相关材料

- 文献矩阵（canonical）：`perpetual-learning/notebook/rollout-performance-research.md` §5
- 可检索网页版：`perpetual-learning/web/presentations/rollout-papers.html`
- 综述大纲：`perpetual-learning/web/presentations/rollout-survey-outline.html`
