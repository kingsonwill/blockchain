[TOC]
#分布式系统基础
##一致性
* 强一致 & 弱一致
* 传统是并行操作串行化
##共识算法
* crash fault 一半
* Byzantine fault  1/3
##FLP不可能原理
* No completely asynchronous consensus protocol can tolerate even a single unannounced process death
*
##CAP原理
* 一致性consistency & 可用性availability & 分区容忍性partition
* CAP不可同时确保，必须弱化一项
*
##ACID原则
* 原子性atomicity & 一致性 & 隔离性isolation & 持久性durability
* BASE原则：牺牲一致性（最终一致）换取可用性
##Paxos 算法
* 两段提交 prepare & commit
* https://blog.csdn.net/21aspnet/article/details/50700123
* https://www.cnblogs.com/endsock/p/3480093.html
## Raft算法
* Leader 选举： 开始 所有 节点 都是 Follower， 在 随机 超时 发生 后 未收 到 来自 Leader 或 Candidate 消息， 则 转变 角色 为 Candidate， 提出 选举 请求。 最近 选举 阶段（ Term） 中 得票 超过 一半 者 被选为 Leader； 如果 未 选出， 随机 超时 后进 入 新的 阶段 重试。 Leader 负责 从 客户 端接 收 log， 并 分发 到 其他 节点；
* 同步 日志： Leader 会 找到 系统 中日 志 最新 的 记录， 并 强制 所有 的 Follower 来 刷新 到这 个 记录， 数据 的 同步 是 单向 的。
##拜占庭问题
* N > 3F
* BFT 拜占庭容错算法
    - PBFT 复杂度多项式级
    - https://blog.csdn.net/landai2011/article/details/52266610
    - https://www.jianshu.com/p/fb5edf031afd
##可靠性指标
#密码学与安全技术
##Hash （digest）
* 特点 ： 正向快速 & 逆向困难 & 输入敏感 & 冲突避免
* 至少用SHA2-256， SHA-3 已提出，
* 是否计算敏感
    - 利：能硬件加速吞吐量
    - 弊：算力攻击
##加解密算法
* 对称加密
    - 分组加密 AES IDEA
    - 序列密码（流密码）
* 非对称加密
    - 公钥 & 秘钥
    - 选择明文攻击
* 混合加密 —— HTTPS协议
##消息认证 & 数字签名
