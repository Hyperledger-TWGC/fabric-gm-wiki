# 我们决定用PKCS#8作为我们导出SM2 Key的标准

## 选用PKCS#8的理由
1. PKCS#8又名PKI语法标准（Private-Key Information Syntax Standard)
1. [简介/Introduction](https://tools.ietf.org/html/rfc5208#section-1)
1. [PKI语法/Private-Key Information Syntax](https://tools.ietf.org/html/rfc5208#section-5)
1. [加密PKI语法/Encrypted Private-Key Information Syntax](https://tools.ietf.org/html/rfc5208#section-6)
1. [安全考量/Security Considerations](https://tools.ietf.org/html/rfc5208#section-7)

## 不选用PKCS#1的理由
1. PKCS#1又名RSA加密规范（RSA Cryptography Specifications)
1. [简介/Introduction](https://tools.ietf.org/html/rfc2437#section-1)
1. [密钥类型/Key types](https://tools.ietf.org/html/rfc2437#section-3)

#### 参考：
- [rfc5208](https://tools.ietf.org/html/rfc5208)
- [rfc2437](https://tools.ietf.org/html/rfc2437#section-1)