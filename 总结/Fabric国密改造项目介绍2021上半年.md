# 背景

本文旨在介绍TWGC hyperledger fabric国密改造小组中各位社区开发者2021年贡献代码和参与讨论相关话题，以帮忙关注本项目朋友能够了解目前2021年本项目的状态。

# 基础库

1.ccs-gm

- pkcs#8 export，互操作库ci修复。

- 小优化

- SM2完善sign和verify，



2.tjfoc-gm

- 优化

- 修复了密文序列化当中的bug

- 修复非国密TLS通信失败的问题

- SM3优化完成起草，暂未提交

- SM2完善sign和verify

- 同济库密钥协商ECDHE完成

- 基础库密钥文件加密方法discussion中两种方式验证都可以



3.pku-gm

- SM2完善sign和verify



# 基础库互操作

-  添加java 互操作

- fabric-GM-plugins -> GM-interop ，准备移除bccsp以及interface

- [ccs] import 无法满足互操作，go语言库之间互相校验不再写成文件

- 互操作库重构开发已完成

- GM-interopt: FIX TJ-CCS-ENCRYPT
  [TODO] SM4 ECB/CBC among TJFOC/ccs



# 基础库padding

- 各个golang基础库ci流程中添加benchmark测试

- Tls 测试

  tjfoc-gm tls双证书测试成功
  tjfoc-gm tls单证书测试失败
  ccs-gm tls双证书测试成功
  ccs-gm tls单证书测试成功
  ccs-gm和tjfoc-gm双证书互操作失败

- 汇编能优化，但是arm有限制，因此也影响了合库的提案



# SDF

- sansec-sdk库提交zip包，可执行测试Demo和测试程序源码

- 三味信安加密机的golang调用程序主体基本架构完成。共计78个接口

- 分享SDF Go语言接口实现

- sdf 贡献到TWGC



# Java-gm

-  java-gm refactor, remove static and potential singleton design

- 验证javax.crypto.Cipher是非线程安全的
- 使用commons-pool2对Cipher进行池化管理，解决线程安全问题；

- Cipher线程安全的编码设计

- SM4模式补充: CFB，OFB，CTR

- java国密颁发证书已完成部分，还有并发需要编写

- 提交SM2Util x509证书相关pr



# GRPC

- 版本经过实践可以用于fabric国密改造

  

# net-go

- 国密版可以用fabric-ca国密改造



# fabric

- fabric有客户时间改造私钥不落地（私钥不能完全存储在本地） KMS方案

- hash options

- 重申方向 本体改造bccsp； TLS验证； sdf-go单独成立



# 约定规则

- SM4 ECB 作为最小测试基准



# Node.js

- 无法绕过typescript



# Fabric-ca

- Fabric-CA已经通过了开源硬改国密版



# 输出文件和总结

- 整理国密组整体路线图

- RFC进度缓慢

- Global Forum Proposal: TWGC GM time slot 30 mins

  Title: TWGC国密改造小组的旅程故事
  Abstract: TWGC 国密改造小组距成立已经近一年的时间了，在这三十分钟时间里，David会带着大家回顾国密组从想法，构思，讨论提案到如今多达12个TWGC国密相关代码仓库的奇妙旅程。立足现在，放眼未来，国密组的长期发展和规划将基于开源社区的协作共建，我们总是有很多未知亟待探索和实现。
  Guest: David Liu, Oracle
  Slides content language: English
  语言：中文
  初衷，过去的时间线
  简要介绍 我们做了什么的，现状如何
  Why interopt，
  做的过程当中抽象，认知，收获的内容。golang库，http，grpc库flexibility问题
  讲roadmap，大方向，如何发展，
  Good First for new contribution， RFC for discussion



# 话题

## tls1.3

由于TLS1.3协议设计主要遵循了精简化的原则，整个协议目前变得非常清爽、简单，密钥交换只有两个选择（ECDHE、DHE），对称密码算法只有（AES128、AES256、CHACHA20），加密模式只有（GCM、CCM、POLY1305），目前已经有一些文章关于这块儿写的很详细了：譬如 https://www.oschina.net/translate/rfc-8446-aka-tls-1-3 以及 https://www.jianshu.com/p/8c65c0f562f2 如果想深入看的话直接看RFC会是比较好的选择 https://tools.ietf.org/html/rfc8446 https://tools.ietf.org/html/rfc5246

https://tools.ietf.org/html/rfc8998 联系作者
Qin Long; zhuolong.lq@antfin.com
Kepeng Li; kepeng.lkp@antfin.com
Ke Zeng; william.zk@antfin.com
Han Xiao; han.xiao@antfin.com
Zhi Guan; guan@pku.edu.cn
Status: informational 还没到 PROPOSED STANDARD状态



## 金融分布式账本规范解析

- Hyperledger-TWGC/fabric-evaluation-JRT0184 更新到17章



## java国密基础库

- 阿里的netty-handle库里面好多方法都没有实现，替换估计行不通， BCOS类似的情况



## 证书

- CFCA签发的证书本身会携带根证书并形成证书链，Go标准库没有没有生成证书链的功能，只有读取证书链的功能



## 其它

- Azure Blockchain Service tainted
  不经意传输











