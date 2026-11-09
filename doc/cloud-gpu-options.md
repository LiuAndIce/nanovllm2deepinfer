# 支持支付宝的云 GPU 候选

核查日期：2026-11-09。用途：1.1 的单卡 Qwen3-0.6B 基线实验。仅查阅公开官方资料，未充值、租机或验证本项目云端运行。

## 建议

优先考虑 AutoDL 普通容器实例的按量计费模式，备选 Featurize。两家官方文档均明确支持支付宝充值，可替代当前 Modal 的绑卡支付路径；这不代表已核实平台实名认证要求，也不能保证用户自己的支付宝账户无需任何支付验证。

第一轮优先比较单张 RTX 3090 24 GB 与 RTX 4090 24 GB。这是实验环境候选，不是已完成兼容性验收；需实际检查 CUDA、PyTorch、Triton 和 FlashAttention 版本并跑通最小推理。不同 GPU 的性能数字不能直接与文档 L4 基线比较，也不能据此认为后续 H100/多卡章节均可替代。

这两家主要提供可 SSH 的租赁实例，不是 Modal Python 函数式运行接口的直接替换。代码需同步到远程 Linux，再运行项目命令；需要自行停止或归还实例。

## AutoDL：首选

- 支付：官方明确列出微信和支付宝充值，官网是唯一充值入口。[充值与计费](https://www.autodl.com/docs/price/)
- 资源：官方 GPU 表列出 3090 24 GB 和 4090 24 GB；GPU 独占。具体空闲资源及价格以租用页面为准。[GPU 选型](https://www.autodl.com/docs/gpu/)
- 使用：支持镜像选择、JupyterLab、SSH，Windows PowerShell 可直接连接；有 PyCharm 使用文档。[快速开始](https://www.autodl.com/docs/quick_start/) / [SSH](https://www.autodl.com/docs/ssh/)
- 计算计费：普通按量容器从开机计费到关机结束，按秒计算；不是按实际调用 GPU 的时间计费。关网页、退出 SSH、结束 Python 都不等于关机。[充值与计费](https://www.autodl.com/docs/price/)
- 存储风险：付费扩容数据盘关机仍计费；容器实例 Pro 的系统盘单独计费。初次不要默认选 Pro 或扩容，也不要把充值余额当硬性总费用上限，官方存在存储欠费规则。[充值与计费](https://www.autodl.com/docs/price/)
- 数据风险：普通实例连续关机 15 天会释放并删除数据；主机下架也可能释放。需将源码和报告备份到本地。[实例数据](https://www.autodl.com/docs/instance_data/)
- 自动关机：官方提供程序结束后执行 `/usr/bin/shutdown` 的做法；启用前应保存日志并确认控制台状态。[省钱绝招](https://www.autodl.com/docs/save_money/)

## Featurize：备选

- 支付：官方明确列出支付宝、微信支付和对公转账。[充值及退款](https://docs.featurize.cn/docs/user/charging)
- 资源：官网列出 RTX 3090 和 RTX 4090。首页优惠价不是本次账户下单价，不据此作费用承诺。[官网](https://www.featurize.cn/)
- 使用：支持 JupyterLab 工作区、SSH、VS Code 和 PyCharm 远程连接。[连接实例](https://docs.featurize.cn/docs/manual/instance-connection)
- 计费：按量租用即开始计费，以分钟为单位；用完应在控制台归还实例，不是仅断开 SSH。长租如果不选到期自动归还，可能转按量继续收费。[实例租用](https://docs.featurize.cn/docs/manual/instance-rent)
- 数据风险：归还或重置会删除同步盘之外的数据；重要代码、模型和报告应放到 `/home/featurize/work` 并另做本地备份。[实例租用](https://docs.featurize.cn/docs/manual/instance-rent)
- 存储：同步盘文档标明默认 30 GB、超额计费、付费扩容续费规则以及长期不用清理规则；该页示例使用了不同配额，具体账户配额应再核实。归还实例不意味着存储必定免费。[同步盘](https://docs.featurize.cn/docs/manual/work)

## 充值前仍需确认

1. 用户支付宝能否实际支付、平台实名认证要求、最低充值金额和退款限制。
2. 当时可用单卡的总小时价、CPU/内存配额、免费磁盘容量与独立存储费用。
3. 可用 Linux 镜像及 CUDA/PyTorch 版本，FlashAttention 的安装途径。
4. 选定机器能否下载模型与依赖；尚未验证 Hugging Face/GitHub 连通性。
5. 若需后续 Nsight Systems 性能分析，另查容器 profiling 权限，不以 SSH 可用推断 profiler 可用。

先注册并查看市场、价格与镜像，不先启动机器。选择确认后仅充值足够短时验证的金额；不购买长租。首次安装与下载同样可能产生实例费用。
