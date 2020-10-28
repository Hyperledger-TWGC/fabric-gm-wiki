> GM-LIB-README-TEMPLATE 国密基础库README模板

# <项目名称>

此处为项目基本介绍

![](https://img.shields.io/badge/至少提供一个Azure%20Pipelines-succeeded-success)
![](https://img.shields.io/badge/可以选择在此放置其他badge-状态-success)

## Feature 功能支持列表



|  SM2功能   | 支持范围  | 
|  ----  | ----  |
| Generate Key  | `是|否` |
| Sign  | `是|否` |
| Verify | `是|否` |
| PEM格式导出 | `私钥|公钥|CSR|证书`|
| PEM格式导入 | `私钥|公钥|CSR|证书` |
| PEM文件加密 | RFC5958 |  

|  SM4功能   | 支持范围  | 
|  ----  | ----  |
| Generate Key |  |
| Encrypt, Decrypt |  |
| PEM格式导出 |   |
| PEM文件加密 | golang: `x509.EncryptPEMBlock` |


|  SM3功能   | 支持范围  | 
|  ----  | ----  |
| 当前语言Hash接口兼容 | `是|否` |

## 注解
- SM2: 国密椭圆曲线算法库
- SM3: 国密hash算法库
- SM4: 国密分组密码算法库

## License
Hyperledger Project source code files are made available under the Apache License, Version 2.0 (Apache-2.0), located in the [LICENSE](LICENSE) file.