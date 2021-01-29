# Fabric 本体改造计划

## 知识储备
以下知识点会帮助您更好的理解本文中提到的一些具体细节。
- 测试驱动开发理念

golang开发库：
- [ginkgo](https://github.com/onsi/ginkgo)
- [text/template](https://pkg.go.dev/text/template?utm_source=gopls)
- [temp dir](https://pkg.go.dev/io/ioutil?utm_source=gopls)
- [gexec](https://pkg.go.dev/github.com/onsi/gomega@v1.10.1/gexec)
- 等等...

## 如何下手

- 参与国密项目组周会。
- 我们近期会考虑开放github issue来供大家认领。
- 动手实践。

## 大方向

我们会遵从测试驱动开发的理念来进行fabric本体的国密改造。
因此我们会首先生成国密的测试案例，之后再将测试案例全部通过。
为此我们将要：

1. 动态生成测试数据

    目前Fabric测试的代码中，所有的测试用到的加密学有关证书均为Hard Code的代码或文件，基于ECDA，在现有基础上为了使得测试案例覆盖国密，我们需要将这部分Hard Code的代码或文件调整为动态生成。
    实现在每个测试集中生成临时文件的案例，范围Fabric所有测试集合。（单元测试和集成测试）
    - 动态生成测试文件改造
    - 动态生成ECDSA，国密测试证书
    参考案例：
    您可以在[这里](https://github.com/Hyperledger-TWGC/tape/blob/v0.1.1/e2e/util.go#L25)找到一个动态生成ecdsa密钥的案例。
    您可以在[这里](https://github.com/Hyperledger-TWGC/tape/blob/v0.1.1/e2e/e2e_test.go#L27)找到如何在ginkgo框架中的`before`, `after`配合临时文件（`temp`）来

1. bccsp 改造 与 hash 下沉
    
    为了更好的支持国密以及移除Fabric自身代码中的TODO item，我们正在将bccsp以及Fabric设计签名的部分从当前逻辑：
    ```
    摘要 = 计算hash （正文）
    签名（摘要）
    ```
    替换为
    ```
    新接口（正文）

    func 新接口（正文）{
        摘要 = 计算hash （正文）
        return 签名（摘要）
    }
    ```
    参考国密概要逻辑：
    ```
    新接口（正文）

    func 新接口（正文）{
        摘要 = sm3 （正文）
        return sm2.sign（摘要）
    }
    ```
    目前 David已经提供了部分实现，[参考](
https://github.com/Hyperledger-TWGC/fabric/pull/1), 我们正在招募志愿者协助实现这个改动。

1. 国密适配

    在完成了国密测试集的改造（或者说对测试集进行国密改造）与接口改造工作后。我们就需要进行国密版本的bccsp实现以通过所有的国密测试场景。完成测试驱动开发的过程。

1. 有关TLS双证书
    
    - 理论上讲，对于tls双证书的改造，也是遵循测试驱动开发流程并包含在上述开发过程中的。
        1. 为TLS双证书添加测试集合，这里的测试集合需添加在集成测试范围内。
        1. 实现TLS双证书库的替换，以通过测试案例。 


### 参考记录
https://github.com/Hyperledger-TWGC/fabric-gm-wiki/wiki/11%E6%9C%8820%E6%97%A5%E4%BC%9A%E8%AE%AE%E8%AE%B0%E5%BD%95
Jay: 先写UAT（/integration）测试（作为最高标准），作为一个标准的测试文档，然后通过测试驱动开发，
Jay：breakdown，1.bccsp 2. align to new interface 3. 新接口可以接入国密
Paul，Jay：提PR：动态生成test data

https://github.com/Hyperledger-TWGC/fabric-gm-wiki/wiki/12%E6%9C%8804%E6%97%A5%E4%BC%9A%E8%AE%AE%E8%AE%B0%E5%BD%95
fabric本体改造路线：哈希下沉，bccsp，test
