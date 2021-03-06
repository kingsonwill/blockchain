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
##原理设计
* 地址 ： 两次hash公钥的结果
* UTXO ： 未使用的交易输出，bitcoin没有余额的概念，均由每笔输入输出交易表示
* STO ： 已经被引用的输出，无法作为合法输入
* 交易
    - 信息
        - 付款人地址
        - 付款人的签字
        - 付款人资金来源ID `？要是多笔小额收入如何汇成大额支付？`
        - 交易金额
        - 收款人地址
        - 收款人公钥
        - 时间戳
    - 检查 ：检查通过标记为合法未确认交易，在全网广播
        - 是否已处理
        - 是否合法
        - 是否输入大于输出
    - 6个块可信确认，约一个钟头
* 交易脚本
    - scriptPubKey & scriptSig
    - 非图灵完备，基于栈的语言
    - http://www.8btc.com/understand-bitcoin-script
    - https://blog.csdn.net/pony_maggie/article/details/73656597
* 区块 —— 极像IP
* 创新设计
    - 避免作恶 —— 挖矿pow
    - 负反馈调节 —— 矿工越多系统越稳定，但挖到矿的概率降低，反之亦反
    - 共识机制
        - pow
            - 最长链原则
            - 大量资源被浪费
            - 51% 攻击
        - pos
            - 1/3 左右结果
            - DPOS 推选董事会代理记账
        - poa
        - pob
        - pot
* 设计权衡
    - 区块容量
    - 出块时间间隔
    - 脚本支持程度
* 分叉 ：协议改动
    - 硬分叉
    - 软分叉
* 交易延展性
##挖矿
* 分布式维护
* 挖矿奖励是为了前期没有交易也能有利益驱动，矿采完全由手续费获得
* 定期（2016个区块）调整hash难度，维持10分钟一区块
##闪电网络
* 解决BTC交易性能问题
* RSMC（recoverable sequence maturity contract）
    - 构建资金池或叫微支付通道（BTC没有余额概念），微交易只在双方内确认，提现时才写入区块（可理解为涉及多方变动或者资金整理）
    - 罚没机制：若另一方证明方案已作废，则资金罚没给质疑方
    - 链外交易鼓励：双方确认提现，提出方资金晚到帐
* HTLC（hashed timelock contract）
    - 通过智能合约，双方约定转账方先冻结一笔钱，并提供一个哈希值，如果在一定时间内有人能提出一个字符串，使得它哈希后的值与已知值匹配（实际上意味着转账方授权了接收方来提现），则这笔钱转给接收方。
* RSMC 保障了交易链下完成，HTLC 提供了一条支付通道
##侧链
* 个人理解是闪电网络的拓展
* BTC作为主链，其他区块作为侧链，挂钩BTC，当BTC在侧链的时候主链上的会被锁定，直到回到主链
* SPV （simplified payment verification）：以较小代价判断交易已被验证 & 得到算力保护
* 双向挂钩
    - 主链向侧链转移时，现在主链创建交易，待转币发往一个特定输出，使得BTC被锁定
    - 等待一段确认期，使得上述交易获得足够的工作量确认；
    - 用户在侧链创建交易提取比特币，需要在这笔交易的输入指明上述主链被锁定的输出，并提供足够的 SPV 证明；
    - 等待一段竞争期，防止双重花费攻击；
    - 比特币在侧链上自由流通；
    - 当用户想让比特币返回主链时，采取类似的动作。首先在侧链创建交易，待返回的比特币被发往一个特殊的输出。 等待一段确认期后，在主链用足够的对侧链输出的 SPV 证明来解锁最早被锁定的输出。等待一段竞争期后，主链比特币恢复流通。
#Ethereum
* 特点
    - 支持图灵完备的智能合约，设计了编程语言 Solidity 和 虚拟机 EVM；
    - 选用了内存需求较高的哈希函数，避免出现强算力矿机、 矿池攻击；
    - 叔块（ uncle block） 激励机制， 降低矿池的优势， 并减少了区块产生间隔（ 10 分钟降低到 15 秒左右）；
    - 采用账户系统和世界状态，而不是 UTXO， 容易支持更复杂的逻辑；
    - 通过 Gas 限制代码执行指令数， 避免循环执行攻击；
    - 支持 PoW 共识算法， 并计划支持效率更高的 PoS 算法。
* 核心架构
    - smart contract
    - 账户概念
        - 不同于BTC的utxo，以太坊直接用账户记录系统状态
        - 合约账户 —— 储存合约代码，只能外部账户激活调用
        - 外部账户 —— ether 账户
        - 合约账户被调用时，矿工执行只能合约，并消耗gas
    - 交易
    - ether
    - gas —— 每执行一条合约指令会消耗固定gas， gas 消耗完还未结束，合约执行终止并回滚状态
* 优化
    - 使用账户
    - 共识改进
    - gas 对抗恶意合约
    - 扩展性 sharding
#Hyperledger
* Fabric
