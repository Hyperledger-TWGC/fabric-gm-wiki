# 概述

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



## 约定规则

| 约定定义 | 说明                                                         | 备注                                                         |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| BCCSP    | Blockchain Cryptographic Service Provider 区块链密码服务提供者 | 通过BCCSP切换加载密码算法                                    |
| SW       | Software 软算法                                              | 在以前定义中这个代表软算法                                   |
| PKCS11   | Public-Key Cryptography Standards 11 公钥加密标准11          | PKCS#11标准定义了与密码令牌（如硬件安全模块（HSM）和智能卡）的独立于平台的API |
| GMSW     | GUOMI Software 国密软算法                                    | 国密算法/国家商用密码算法：SM2/SM3/SM4等                     |
| SDF      | 密码设备应用接口规范                                         | 参考国密规范GMT0018                                          |

说明:在使用sm2/sm3/sm4定义时候，建议不必使用gmsm2这种描述



# 国密基础库

## 同济基础库

​	苏州同济区块链研究院有限公司开源基础库:[tjfoc-gm](https://github.com/Hyperledger-TWGC/tjfoc-gm)

### Feature 功能支持列表

| SM2功能          | 支持范围             |
| ---------------- | -------------------- |
| Generate KeyPair | `是`                 |
| Sign             | `是`                 |
| Verify           | `是`                 |
| PEM格式导出      | `私钥/公钥/CSR/证书` |
| PEM格式导入      | `私钥/公钥/CSR/证书` |
| PEM文件加密      | RFC5958              |

| SM4功能          | 支持范围                       |
| ---------------- | ------------------------------ |
| Generate Key     | `是`                           |
| Encrypt, Decrypt | `是`                           |
| PEM格式导出      | `是`                           |
| PEM文件加密      | golang: `x509.EncryptPEMBlock` |
| 分组模式         | ECB/CBC/CFB/OFB/CTR            |


| SM3功能              | 支持范围 |
| -------------------- | -------- |
| 当前语言Hash接口兼容 | `是`     |

| SSL/TLS        | 支持范围                  |
| -------------- | ------------------------- |
| 国密TLS        | `是`                      |
| 支持单证书     | 是                        |
| 支持双证书     | 是                        |
| 密码套件       | ECC-SM4-SM3/ECDHE-SM4-SM3 |
| 与ccs-gm联调试 | 失败                      |
| 与tassl联调试  | 失败                      |





## 网安基础库

​	中国网安go语言国密基础库:[ccs-gm](https://github.com/Hyperledger-TWGC/ccs-gm)

### Feature 功能支持列表

| SM2功能          | 支持范围       |
| ---------------- | -------------- |
| Generate KeyPair | 是             |
| Sign             | 是             |
| Verify           | 是             |
| PEM格式导出      | 私钥/公钥/证书 |
| PEM格式导入      | 私钥/公钥/证书 |
| PEM文件加密      | RFC5958        |

| SM4功能          | 支持范围                       |
| ---------------- | ------------------------------ |
| Generate Key     | 是                             |
| Encrypt, Decrypt | 是                             |
| PEM格式导出      |                                |
| PEM文件加密      | golang: `x509.EncryptPEMBlock` |
| 分组模式         | ECB/CBC                        |


| SM3功能              | 支持范围 |
| -------------------- | -------- |
| 当前语言Hash接口兼容 | `是`     |


| SSL/TLS       | 支持范围                  |
| ------------- | ------------------------- |
| 国密TLS       | `是`                      |
| 支持单证书    | 是                        |
| 支持双证书    | 是                        |
| 密码套件      | ECC-SM4-SM3/ECDHE-SM4-SM3 |
| 与tjfoc联调试 | 失败                      |
| 与tassl联调试 | 失败                      |



## 北大基础库

​	北京大学go语言国密基础库：[pku-gm](https://github.com/Hyperledger-TWGC/pku-gm)

### Feature 功能支持列表

//待确认

| SM2功能          | 支持范围       |
| ---------------- | -------------- |
| Generate KeyPair | 是             |
| Sign             | 是             |
| Verify           | 是             |
| PEM格式导出      | 私钥/公钥/证书 |
| PEM格式导入      | 私钥/公钥/证书 |
| PEM文件加密      | RFC5958        |

| SM4功能          | 支持范围                       |
| ---------------- | ------------------------------ |
| Generate Key     | 是                             |
| Encrypt, Decrypt | 是                             |
| PEM格式导出      |                                |
| PEM文件加密      | golang: `x509.EncryptPEMBlock` |
| 分组模式         | ECB/CBC                        |

| SM3功能              | 支持范围 |
| -------------------- | -------- |
| 当前语言Hash接口兼容 | `是`     |





# Fabric 本体改造计划

## 近期方向

​	现在方案请参考[Fabric2.0.0国密改造代码分析](https://github.com/Hyperledger-TWGC/fabric-gm-wiki/blob/master/%E7%8E%B0%E6%9C%89%E6%96%B9%E6%A1%88/Fabric2.0.0%E5%9B%BD%E5%AF%86%E6%94%B9%E9%80%A0%E4%BB%A3%E7%A0%81%E5%88%86%E6%9E%90.pdf)

使用最新Fabric2.2.0 LTS版本进行“硬”方式先进行改造:

- 支持国密算法


- 支持x509国密算法证书

- 
  支持国密SSL/TLS 双证书模式(待定)


- 支持SDF改造(待定)


- 暂时不考虑兼容，第二阶段进行


- ”硬改“方案进行cli级别测试覆盖




## 长期方向

​	我们会遵从测试驱动开发的理念来进行fabric本体的国密改造。
​	因此我们会首先生成国密的测试案例，之后再将测试案例全部通过。

### 1 动态生成测试数据


目前Fabric测试的代码中，所有的测试用到的加密学有关证书均为Hard Code的代码或文件，基于ECDSA，在现有基础上为了使得测试案例覆盖国密，我们需要将这部分Hard Code的代码或文件调整为动态生成。
实现在每个测试集中生成临时文件的案例，范围Fabric所有测试集合。（单元测试和集成测试）

- 动态生成测试文件改造

- 动态生成ECDSA，国密测试证书
  参考案例：
  您可以在[这里](https://github.com/Hyperledger-TWGC/tape/blob/v0.1.1/e2e/util.go#L25)找到一个动态生成ecdsa密钥的案例。
  您可以在[这里](https://github.com/Hyperledger-TWGC/tape/blob/v0.1.1/e2e/e2e_test.go#L27)找到如何在ginkgo框架中的`before`, `after`配合临时文件（`temp`）来

  

### 2 hash 下沉

为了更好的支持国密以及移除Fabric自身代码中的TODO item，我们正在将bccsp以及Fabric设计签名的部分从当前逻辑：

hash下沉涉及两部分：

- 将hash256()再次封装

  将hash256()封装在新加hash()方法中，然后Fabric业务代码使用hash256()将替换成hash(), 最好在hash()根据需要进行不同sha256\sm3\sha512替换

```
摘要 = hash（正文）
签名（摘要）
```

目前 David已经提供了部分实现，[参考](
https://github.com/Hyperledger-TWGC/fabric/pull/1), 我们正在招募志愿者协助实现这个改动。

- 国密签名特殊性


目前fabric中签名sign(hash)入参是hash值，但是国密签名需要在签名sign(data)在sign方法中再进行hash计算，为了保持一致性，需要将现在fabric中sign(hash)改为sign(data)

```
func 新接口（正文）{
    摘要 = sha256（正文）
    return ecdsa.sign（摘要）
}
func 新接口（正文）{
    摘要 = sm3 （正文）
    return sm2.sign（摘要）
}
```


针对上面问题未来还有一个解决方法：fabric下bccsp会结构回进行调整，bccsp提供算法密码服务提供者使用rpc进行整体替换，进而间接解决了上述问题



### 3 国密适配

​	在完成了国密测试集的改造（或者说对测试集进行国密改造）与接口改造工作后。我们就需要进行国密版本的bccsp实现以通过所有的国密测试场景。完成测试驱动开发的过程。

#### 3.1 BCCSP改造

- 现阶段改造目标

目前BCCSP现有支持模式SW和PKCS11， 国密改造计划支持SW对应的国密算法GMSW模式：对接三个不同密码基础库(甚至更多), 支持PKCS11对应国密标准SDF接口。

```
BCCSP {
		SW: --> golang crypto
		PKCS11: --> hsm(PKCS11 api)
		GMSW: --> {
					tjfoc-gm
					ccs-gm
					pku-gm
					...
		}
		SDF: --> HSM(SDF API) //GMT 0018-2012 密码设备应用接口规范 //
}
```

​		SDF将使用[三未信安密码机产品SDK](https://github.com/Hyperledger-TWGC/sansec-sdk)进行适配调试




- 长期目标

  针对现在设计不足，未来将会使用rpc方案，将整个替换BCCSP

​		GMSW对接不同国密算法基础库方式：rpc（待定） 
```
rpc {
	bccspa:
	bccspb:
	bccspc:
	....
}
```

#### 3.2、有关TLS双证书

- 理论上讲，对于tls双证书的改造，也是遵循测试驱动开发流程并包含在上述开发过程中的。
    1. 为TLS双证书添加测试集合，这里的测试集合需添加在集成测试范围内。
    
    1. 实现TLS双证书库的替换，以通过测试案例。 
    
       

#### 3.3、GRPC

​	TWGC已实现已实现国密版[GRPC](https://github.com/Hyperledger-TWGC/grpc), 定期支持新版本。

​    

#### 3.4、SDF SDK

​	SDF参考规范GMT 0018-2012 密码设备应用接口规范 

​	三未信安密码机产品SDK   //自己实现sdk中不能使用私有库源码，可以放so

​	实现方式：GO SDF SDK  --> C SDK  //GO SDF SDK新建仓库



### 4、 双证书适配



### 5、import适配不同算法库应用

将会使用rpc方案进行改造实现



# SDK改造计划

## Java SDK

​	实现属于自己的class替换？

1. [java gateway项目也需要我们实现一份](https://github.com/hyperledger/fabric-gateway/blob/master/java/src/main/java/org/hyperledger/fabric/client/identity/ECDSAPrivateKeySigner.java)
1. sdk-java[测试目前手写的，因此我们需要一份手写的测试集](https://github.com/hyperledger/fabric-sdk-java/blob/master/src/test/java/org/hyperledger/fabric/sdk/security/CryptoPrimitivesTest.java#L82)
1. twgc-sdk-java fork的ci貌似没触发。[以及我们貌似要开一个项目做我们自己的java-sdk的实现？](https://chat.hyperledger.org/channel/fabric-sdk-java?msg=g4AamwBcdpBp7AJP7)

 *java*-gm refactor



### twgc自实现库

Crypto:  https://github.com/Hyperledger-TWGC/java-gm.git



### Java TLS/SSL库实现方式1

 

Netty使用aliyun开源[Java gm-jsse](https://github.com/Hyperledger-TWGC/gm-jsse)库替换openssl, gm-jsse已经实现套件ECC-SM4-SM3(e013)  



### 关于 TLS/SSL库实现方式2

Netty openssl替换成国密版openssl(国密版)



#### 其它收集库

1、Fisco-bcos通过netty-sm-ssl-context在java中调用国密tls，或许java sdk的国密改造可以利用netty-sm-ssl-context。
https://mvnrepository.com/artifact/org.fisco-bcos/netty-sm-ssl-context

2、聚龙链



# 

## Go-SDK

待定

## Node-SDK

待定，参考基础https://github.com/Hyperledger-TWGC/node-gm





# Fabric-CA

​		暂定针对某个版本“硬改”

关于https国密支持：

​		Fabric-ca Https支持参考国密版本net库[net-go-gm](https://github.com/Hyperledger-TWGC/net-go-gm) 

 关于Cfssl 证书调用底层库：

​		//可以开源，看需要

​		Cfssl是Fabric-ca和GO-SDK都使用到




# 参考记录
https://github.com/Hyperledger-TWGC/fabric-gm-wiki/wiki/11%E6%9C%8820%E6%97%A5%E4%BC%9A%E8%AE%AE%E8%AE%B0%E5%BD%95
Jay: 先写UAT（/integration）测试（作为最高标准），作为一个标准的测试文档，然后通过测试驱动开发，
Jay：breakdown，1.bccsp 2. align to new interface 3. 新接口可以接入国密
Paul，Jay：提PR：动态生成test data

https://github.com/Hyperledger-TWGC/fabric-gm-wiki/wiki/12%E6%9C%8804%E6%97%A5%E4%BC%9A%E8%AE%AE%E8%AE%B0%E5%BD%95
fabric本体改造路线：哈希下沉，bccsp，test
