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
* 消息认证
    - 基于对称加密
    - 无法证明消息来源
* 数字签名
    - 基于非对称加密
    - 一个典型的场景是， Alice 通过信道发给 Bob 一个文件（一份信息）， Bob 如何获知所收到的文件即为 Alice 发出的原始版本？ Alice 可以先对文件内容进行摘要，然后用自己的私钥对摘要进行加密（签名），之后同时将文件和签名都发给 Bob。 Bob 收到文件和签名后，用 Alice 的公钥来解密签名，得到数字摘要，与收到文件进行摘要后的结果进行比对。如果一致，说明该文件确实是 Alice 发过来的（别人无法拥有 Alice 的私钥），并且文件内容没有被修改过（摘要结果一致）。
    - 盲签名 blind signature ：签名内容保护
    - 多重签名 multiple signature ： 多人决策
    - 群签名 group signature （环签名ring） ： 群内匿名
##数字证书
* 加密数字证书 & 签名验证数字证书 & CA （certification authority）
* 证书规范与格式 （略）
* 证书信任链 ： 根 CA 实现信任基础
##PKI体系
* CA直接生成公私钥给用户
* 用户自己生成公私钥，由CA来签名
    - 优：保持私钥的私密性
    - 劣：私钥丢失无法恢复，加密内容无法破解
* 撤销
    - 过期作废 & 主动撤销
    - 第三方验证时，首先查证是否撤销
##Merkle 树结构（哈希树）
* 底层叶节点包含数据及hash & 非叶节点持孩子节点内容的hash
* 应用
    - 快速比较大量数据
    - 快速定位修改
    - 零知识证明
##布隆过滤器
* 多个hash函数计算出多个地址，求或
* 存在误报，但绝不会漏报
##同态加密
* 全同态 & 算数同态 & 特定同态
* 基于理想格（ideal lattice）
* 基于整数近似GCD
* 基于带扰动学习
* 函数加密 ：不存在多个通用函数的任意多秘钥方案
##other
* 零知识证明
* 量子密码学
* 社交工程学
#Bitcoin
