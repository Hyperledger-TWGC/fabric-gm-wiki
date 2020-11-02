## 项目背景和目标

密码是解决网络与信息安全最有效、最可靠、最经济的方式，是维护网络与信息安全的核心技术和基础支撑。

国密算法是国家通用密码算法的简称，是国家密码管理局制定的自主可控的国产算法，包括SM1、SM2、SM3 、SM4、SM7、SM9、祖冲之密码算法（ZUC)等。

密码算法在区块链系统中起着举足轻重的作用，这对于Hyperledger Fabric来说也不例外。Hyperledger Fabric是具有国际影响力的企业级区块链平台，其默认密码算法为国际标准密码算法，但是对世界各国企业而言，区块链项目存在根据行业规范或当地法律法规调整加密算法或实施细节的需要。另外，Fabric的密码套件虽然是可插拔式的，但是密码算法扩展性上还是存在一些限制。为了更好地推进Hyperlegder Fabric项目对不同加密算法实现的支持，降低企业使用其作为区块链解决方案时对于密码套件改造可能的二次开发成本，提高国际化友好性，Fabric国密改造项目应运而生。Fabric国密改造项目由Hyperlegder中国工作组（TWGC）发起执行，旨在构建支持国密算法且密码算法可灵活扩展的Fabric平台，方便世界各国开发者快速接入自定义的密码算法。

接下来，本文会从不同方面来阐述Fabric国密改造项目。

## 方案设计

​	Hyperlegder Fabric项目主要包括三个部分：Fabric、Fabric-CA和Fabric SDK。根据改造需求分析，Fabric国密改造方案由三个部分构成：

- 国密算法基础库收集与改造

  构建完善可用的国密算法基础库是Fabric国密改造的首要事项。本次改造涉及的国密算法包括：SM2、SM3和SM4。另外，TLS协议也需要进行对应的国密改造。

  实现国密算法的编程语言包括：Golang、NodeJS和Java，各语言实现的国密算法之间需要进行互操作验证。

  国密算法基础库的构建有两个途径
    1. 收集已有的国密算法开源实现项目，最终收集并审核加入的成熟基础库包括
        - [北京大学信息安全实验室 GMSSL 系列](https://github.com/Hyperledger-TWGC/pku-gm)
        - [中国网安 CCS-GM 系列](https://github.com/Hyperledger-TWGC/ccs-gm)
        - [苏州同济区块链研究院 tjfoc-gm](https://github.com/Hyperledger-TWGC/tjfoc-gm)
    1. 自己实现的国密算法。
        - [java-gm](https://github.com/Hyperledger-TWGC/java-gm)
        - [node-gm](https://github.com/Hyperledger-TWGC/node-gm)

- Fabric本体改造

  Fabric本体改造包括：Fabirc改造和Fabric-CA改造，主要是重构Fabric密码套件接入方式，以便开发者灵活接入自定义的密码算法。完成对Fabric的国密算法接入改造，主要涉及国密算法的Golang实现以及改造出符合国密标准的TLS通信加密组件。

- Fabric SDK改造

  Fabric SDK改造包括：Fabric客户端SDK改造，这部分涉及的语言分别是Golang、NodeJS和Java。当Fabric的国密改造完成时，对应的客户端程序中部分涉及加密学的部分也需要进行国密改造以适配Fabric网络的国密PKI体系。

## 项目进度

​	基于上述改造方案，改造项目实施路线分为两个阶段：准备阶段、实施阶段、整理阶段。

1. ​	准备阶段（已完成）

   - 国密改造项目收集：该项目发起之前，已经有一些企业或个人对国密算法、Fabric国密改造进行了研究和编码，项目实施前需要将这些项目进行收集整理、参考学习，避免重复造轮子。

    - 改造需求收集：收集广大用户的国密改造需求，明确国密改造目标。  主要有：
        - 采用规范的非入侵式的改造设计，希望国密改造能够与Fabric升级持续兼容；
        - 开发支持多语言且完善的国密库和通讯套件；
        - 实现Fabric-SDK多语言的国密改造；

1. ​	实施阶段

   好的开始是成功的一半，那么成功的另一半则是脚踏实地的执行。实施阶段是方案落实执行阶段，主要涉及项目管理工作，包括：执行跟踪和测试验收。

   - 执行跟踪：通过周例会的形式，国密改造小组同步各个项目的进展情况，并就执行过程中存在的问题进行分析讨论。

        | 国密基础库    | SM2    | SM3    | SM4    | TLS    |
        | ------------ | ------ | ------ | ------ | ------ |
        | 北大国密库    | 成熟   | 成熟   | 成熟    |        |
        | 网安国密库    | 成熟   | 成熟   | 成熟    |  孵化  |
        | 同济国密库    | 成熟   | 成熟   | 成熟    |       |
        | Java国密库    | beta  | beta   | beta   |  孵化  |
        | NodeJS国密库  | beta | 孵化     |       |        |

        > - 成熟：可以使用，并且包含了项目内部单元测试和互操作测试。
        > - beta：功能待完善，开发过程中。
        > - 孵化：调研中。

   - 测试验收：Fabric国密改造联调测试，主要是各语言SDK与Fabric、Fabric-CA的联调测试。

1. ​	整理阶段

   整理阶段主要复盘国密改造的整体工作，整理开发文档和迁移兼容指南，为对Fabric国密的开发者用户提供详细准确的参考。



## 项目特点

国密工作组至今的工作有很多其他开源项目少有的方式和特点。主要体现在国密社区多样化的实现方式、互操作认证以及对于开源的贡献上。


多样化的实现方式
-------------
密码学是一个门槛比较高的领域，开源领域支持国密算法及其通讯套件的项目凤毛麟角，而且国密算法的开发者生态是不完整的，能够完善支持国密的密码库和通讯套件的流行的编程语言和框架非常少（目前Java的bouncycastle库已支持国密算法）。

对于国密工作组而言，鉴于国密算法生态如此，在Hyperledger Fabric上进行国密算法改造是非常困难的。为了解决这个问题，国密工作组至今并继续努力实现多语言的国密算法基础库，目前规划有Golang，Java，NodeJS等不同语言的国密算法基础库。

互操作认证
-------------
由上述特点引申出一个问题，如何在跨语言的国密算法实现库之间验证其兼容性。

Hyperledger Fabric的项目是多方参与的区块链解决方案，由于国密算法是一套标准，而在实际实施过程中，可能会有不同的实现方式。比如Fabric其本身基于Golang语言，使用Golang实现的国密算法，而应用端使用Java或NodeJS等其他语言实现的国密算法。

为了解决这个问题，国密工作组基于不同语言国密算法实现密钥可以交叉验证的宗旨，创建了fabric-gm-plugins这个工程来确保不同语言之间的国密算法基础库之间可以进行互操作。同时，我们对于各个项目库内部的测试，接口与文档规范也有一定的要求，比如在Golang的实现库中覆盖了有关性能的benchMark测试案例。

社区影响力
-------------
随着区块链的推广，国密算法在各个行业的应用也会变得普及，从而保护各行各业的信息安全。项目实施过程中遵守开发规范，不断复盘整理框架结构和项目代码，力争对外形成统一规范，树立标杆形象。为了和业界已有的规范标准相匹配，在实现过程中同时也参考了[TWGC对于《金融分布式账本技术安全规范》的解析（beta版本）](https://docs.qq.com/doc/DV1VMenFiQXBpeFZK)。此外，国密工作组积极参与开源标准制定，比如：向Fabric社区提交了有关bccsp的改造方案的[RFC文档](https://github.com/hyperledger/fabric-rfcs/pull/34)。

## 参与国密改造项目你将收获

- 个人

  - 一群志同道合的朋友
  - 技术能力提升
  - 开源项目经验
  - 大平台的交流机会

- 企业

  - 认知度提升
  - 更多的合作机会
  - 更好的人才储备

Fabric国密改造项目是由Hyperledger TWGC发起执行的开源项目，需要对该项目感兴趣的开发者一起协作完成。我们TWGC现已招募到了50+项目志愿者。
欢迎企业或个人参与Fabric国密改造项目，为开源社区贡献自己的一份力量。人人为我，我为人人。

联系方式
-------------
- [加入TWGC Github组织, 给国密项目做出代码贡献](https://github.com/Hyperledger-TWGC) 
- 国密支持开发微信群：微信联络David Liu(davidkhala)，Scott Long(hncslwx)进群。
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

TWGC对于《金融分布式账本技术安全规范》的解析（beta版本）
https://docs.qq.com/doc/DV1VMenFiQXBpeFZK

向Fabric社区提交了有关bccsp的改造方案的RFC文档
https://github.com/hyperledger/fabric-rfcs/pull/34

TWGC Github组织, 给国密项目做出代码贡献
https://github.com/Hyperledger-TWGC

TWGC在Hyperledger的联系渠道 
https://wiki.hyperledger.org/display/TWGC/Technical+Working+Group+China

参加国密改造周例会
https://github.com/Hyperledger-TWGC/fabric-gm-wiki/wiki/%E6%AF%8F%E5%91%A8%E4%BE%8B%E4%BC%9A%E4%BF%A1%E6%81%AF

### 鸣谢
-------------
David Liu（https://github.com/davidkhala）

Xiao hui （https://github.com/xiaohui249）

Sam Yuan（https://github.com/SamYuan1990）
