## 项目背景和目标

密码是解决网络与信息安全最有效、最可靠、最经济的方式，是维护网络与信息安全的核心技术和基础支撑。

国密算法是国家通用密码算法的简称，是国家密码管理局制定的自主可控的国产算法，包括SM1、SM2、SM3 、SM4、SM7、SM9、祖冲之密码算法（ZUC)等。

密码算法在区块链系统中起着举足轻重的作用，这对于Hyperledger Fabric来说也不例外。Hyperledger Fabric是具有国际影响力的企业级区块链平台，其默认密码算法为国际标准密码算法，但是对世界各国企业而言，区块链项目存在根据行业规范或当地法律法规调整加密算法或实施细节的需要。另外，Fabric的密码套件虽然是可插拔式的，但是密码算法扩展性上还是存在一些限制。为了更好地推进Hyperlegder Fabric项目对不同加密算法实现的支持，降低企业使用其作为区块链解决方案时对于密码套件改造可能的二次开发成本，提高国际化友好性，Fabric国密改造项目应运而生。Fabric国密改造项目由Hyperlegder中国工作组（TWGC）发起执行，旨在构建支持国密算法且密码算法可灵活扩展的Fabric平台，方便世界各国开发者快速接入自定义的密码算法。

本文旨在介绍TWGC hyperledger fabric国密改造小组自2020年6月17日成立后，各位社区开发者2021年贡献代码和参与讨论相关话题，方便感兴趣的朋友能够了解目前2021年本项目的状态。

## 基础库通用方案和讨论
2021年上半年，对于各个基础库的通用方案和讨论基本围绕以下几点展开，这里也是社区和工作小组中大家广泛讨论和关心的问题：

- pkcs#8
经过国密小组商讨和研究，对于各个基础库，我们目前采用并已经统一实现了基于pkcs#8的密钥/证书导入导出标准。

- ci中加benchmark
大家普遍关心国密改造后是否会带来性能问题，因此国密工作组与性能工作组合作，在各个golang的国密基础库上添加了benchmark测试和对于内存泄漏在内的性能相关检测工作。以期待通过国密算法对比Fabric本身的算法在密码学操作（散列/加密/解密/签名/验证）上的性能对比，从而回答国密算法改造对于Fabric本身的性能影响，以及验证各个国密基础库的算法运行效率。并且在上半年我们对于各个国密库的一些性能相关问题进行了部分优化工作。

- 汇编加速有关的讨论
目前在部分基础库代码实现层面，涉及到一些汇编优化方案。由于arm有限制，因此也带来了一些讨论和问题。
比如社区朋友有希望在不同CPU架构下实现国密算法改造，因为汇编实现而不兼容的情况。考虑作为社区项目很难兼容到到不同CPU架构，主要是设备兼容问题，我们将会维持目前的CPU架构的支持策略。

- SM4 ECB作为各个国密库的最小测试基准

## 基础库目前进展状况

总体来看，上半年我们在各个国密基础库的功能完善，性能优化，和bug fix上均有不同程度的进展。具体的细节请大家参考各个国密库的发行日志。这里只给出状态表如下：

| 国密基础库    | SM2    | SM3    | SM4    | TLS    |
| ------------ | ------ | ------ | ------ | ------ |
| 北大国密库    | 成熟   | 成熟   | 成熟    |        |
| 网安国密库    | 成熟   | 成熟   | 成熟    |  孵化   |
| 同济国密库    | 成熟   | 成熟   | 成熟    |        |
| Java国密库    | 成熟  | 成熟   | 成熟    |  孵化   |
| NodeJS国密库  | beta | 孵化     |       |        |

> **成熟**：可以使用，并且包含了项目内部单元测试和互操作测试。  
> **beta**：功能待完善，开发过程中。  
> **孵化**：调研中。  


## 基础库之间的互操作认证

![CI测试结果](../图片/互操作库CI状态.png)

如图，我们将项目名称从fabric-GM-plugins更名为GM-interop我们主要完成了
- 与java 国密库互操作认证
- 主要国密库之间的SM2/SM3的互操作验证

后续工作
- 准备移除bccsp以及interface
- SM4相关的互操作认证工作

有关TLS互操作测试部分
- tjfoc-gm tls双证书测试成功
- tjfoc-gm tls单证书测试失败
- ccs-gm tls双证书测试成功
- ccs-gm tls单证书测试成功
- ccs-gm和tjfoc-gm双证书互操作失败  

## SDF相关工作

感谢三未信安贡献的测试环境和对应的技术支持，使得国密工作小组可以基于可执行测试Demo和测试程序源码对SDF国密基础库进行设计，开发，验证工作。目前该项目状态如下：

- 三未信安加密机的golang调用程序主体基本架构完成。共计78个接口

- 分享SDF Go语言接口实现

- 贡献到TWGC

## 国密库通讯改造
有关GRPC通讯改造，经过实践可以用于fabric国密改造，目前还处于开发状态。
有关net-go的通讯改造，国密版可以用fabric-ca国密改造。已经完成并用于fabric-ca的参考实现中。

## Fabric，Fabric-CA改造
Fabric-CA改造的参考实现已经完成并开源在社区。

## 下一步工作计划
我们整理了国密组整体路线图，接下来会在进一步完善国密基础库的基础上进行开源国密改造工作，以推动RFC在社区的工作进度。欢迎大家踊跃贡献。


## 参与国密改造项目你将收获

- 个人

  - 一群志同道合的朋友
  - 技术能力提升
  - 开源项目经验
  - 大平台的交流机会，比如[在超级账本全球论坛上分享《TWGC国密改造小组的旅程故事》](https://github.com/Hyperledger-TWGC/fabric-gm-wiki/blob/master/%E4%BC%9A%E8%AE%AE%E8%AE%B0%E5%BD%95/A%20journey%20with%20the%20TWGC%20CryptoGraphy%20Team%20-%20David%20Liu%2C%20Oracle.pdf)

- 企业

  - 认知度提升
  - 更多的合作机会
  - 更好的人才储备

Fabric国密改造项目是由Hyperledger TWGC发起执行的开源项目，需要对该项目感兴趣的开发者一起协作完成。我们TWGC现已招募到了50+项目志愿者。
欢迎企业或个人参与Fabric国密改造项目，为开源社区贡献自己的一份力量。人人为我，我为人人。


### 联系方式
-------------
- [加入TWGC Github组织, 给国密项目做出代码贡献](https://github.com/Hyperledger-TWGC) 
- 国密讨论微信群：微信联络David Liu（davidkhala），Sam Yuan（oe19901019），肖慧（luoyu_276354421）进群。
- [TWGC在Hyperledger的联系渠道](https://wiki.hyperledger.org/display/TWGC/Technical+Working+Group+China)
- [参加国密改造周例会](https://github.com/Hyperledger-TWGC/fabric-gm-wiki/wiki/%E6%AF%8F%E5%91%A8%E4%BE%8B%E4%BC%9A%E4%BF%A1%E6%81%AF)


### 参考链接
-------------
北京大学信息安全实验室 GMSSL 系列
https://github.com/Hyperledger-TWGC/pku-gm

中国网安 CCS-GM 系列
https://github.com/Hyperledger-TWGC/ccs-gm

苏州同济区块链研究院 tjfoc-gm
https://github.com/Hyperledger-TWGC/tjfoc-gm

java-gm
https://github.com/Hyperledger-TWGC/java-gm

node-gm
https://github.com/Hyperledger-TWGC/node-gm

向Fabric社区提交了有关bccsp的改造方案的RFC文档
https://github.com/hyperledger/fabric-rfcs/pull/34


### 鸣谢
-------------
Guijun Chen: https://github.com/guijunchen

Sam Yuan: https://github.com/SamYuan1990











