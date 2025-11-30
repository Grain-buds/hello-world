## 大语言模型推理优化技术综述

### LLM 的推理过程包含：
1、Prefill 阶段（预填充）：
- 此阶段是 LLM 推理的初始阶段，负责处理输入的提示（prompt）。
- 其主要任务是将输入的文本转换为模型可以理解的内部表示，即 Key/Value (KV) 缓存。
- Prefill 阶段的计算量通常较大，尤其是在处理长提示时。
  2、解码阶段
- 此阶段是 LLM 推理的生成阶段，负责根据 Prefill 阶段生成的 KV 缓存，逐一生成后续的 token。
- Decode 阶段是一个迭代的过程，每次生成一个 token，并将其添加到已生成的序列中，直到达到预定的生成长度或生成结束符。
- Decode 阶段的显存消耗通常较大，因为 KV 缓存会随着生成过程不断增长。

参考：https://www.cnblogs.com/menkeyi/p/18767869

https://blog.csdn.net/Baihai_IDP/article/details/148280472






### 其他
https://blog.csdn.net/Baihai_IDP/article/details/148280472

多头注意力机制


## 大模型部署

参考：https://www.cnblogs.com/ExMan/p/18729550

PagedAttention显存管理机制：
https://www.cnblogs.com/zackstang/p/19036108


## vLLM的KV高效缓存的机制
https://cloud.tencent.com/developer/article/2529636
https://cloud.tencent.com/developer/article/2529756
https://www.cnblogs.com/zackstang/p/19036108#_label2
https://www.cnblogs.com/alisystemsoftware/p/19059385




# 意图识别+工具调用任务场景单模块的评测：
范围模型本身
关注点能力、准确性、安全性
评测重点生成质量、推理能力
方法自动化 + 人工打分
## 单意图识别
### 实现方案
### 评测方式
### 线上持续监控优化
## 多意图识别
### 实现方案
### 评测方式
### 线上持续监控优化

## 全链路的评测
关注点：功能、性能、体验、集成
评测重点：任务完成率、延迟、稳定性
方法：A/B测试 + 用户调研 + 日志分析





2、指令遵循能力（Instruction Following）
是否准确理解并执行复杂指令
对模糊指令的鲁棒性


3、
Prompt 设计要点
明确角色：让模型知道自己是“意图识别助手”。
列出可选工具：清晰定义所有可能的意图及其参数。
结构化输出格式：便于后续系统解析（如 JSON）。
包含置信度：帮助下游判断是否需要人工干预。
处理模糊输入：提示模型进行合理推断或返回“无匹配”。
避免歧义：参数命名清晰，避免自然语言描述。

4、
进阶优化建议
加入上下文记忆：如果是多轮对话，可加入历史对话上下文。
支持模糊匹配：如“提醒我下午三点开会” → 映射到“设置提醒”。
参数提取增强：可结合命名实体识别（NER）提升参数抽取准确率。
可扩展性：工具列表可通过变量注入，便于动态更新。


三、评估维度与指标
1. 意图准确率（Intent Accuracy）
   定义：预测意图 == 正确意图 的比例
   公式：正确识别的样本数 / 总样本数
   目标：> 95%（高要求场景）
2. 参数 F1 分数（Parameter F1）
   对每个参数字段计算：
   精确率（Precision）：提取的参数中正确的比例
   召回率（Recall）：应提取的参数中被正确提取的比例
   F1 = 2 * P * R / (P + R)
   可计算平均参数 F1 或完全匹配率（所有参数都正确）
3. 格式合规率（Format Compliance）
   定义：输出是否符合指定格式（如有效 JSON、字段完整）
   可通过 json.loads() 验证 + 字段检查
   目标：> 98%
4. 置信度校准（可选）
   比较“高置信”样本的准确率 vs “低置信”样本
   若高置信样本错误多，说明置信度不可信
5. 拒绝率合理性
   对非工具相关输入，是否正确返回“无匹配工具”
   错误：将闲聊误判为工具调用



八、持续监控与迭代
部署后建议建立：

线上日志采样 + 定期评估
用户反馈闭环：标记“工具调用失败”样本
A/B 测试：不同 Prompt 版本对比准确率


评估指标加权设计:相关*30%+指令跟随*30%+




https://aws.amazon.com/cn/blogs/china/agent-quality-evaluation/