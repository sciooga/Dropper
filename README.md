# Dropper
Dropper(投掷器) 基于七牛云对象存储的文件分享工具

### 快速上手

1. [注册登录七牛云](https://sso.qiniu.com/)
2. [创建对象存储空间](https://portal.qiniu.com/kodo/bucket?shouldCreateBucket=true)
3. 在空间设置处将默认首页开启
4. 下载 [index.html](https://raw.githubusercontent.com/sciooga/Dropper/main/index.html) 并上传至空间
5. 绑定或直接使用测试域名访问即可
6. 在 Dropper 页面右上角输入[密钥](https://portal.qiniu.com/user/key)及空间名即可

### 原理

* 七牛的安全机制涉及上传凭证、下载凭证、管理凭证，其中[上传凭证](https://developer.qiniu.com/kodo/1208/upload-token)可以按算法在前端生成。
* 前端 localStorage 保存用户的密钥及空间名，可以生成上传凭证并将文件上传至对应空间获得资源地址(下载地址)
* 由于前端页面和资源处于统一域，所以只需将资源地址路径加密即可
* 前端生成随机密码（同时也是后续下载时的文件提取码），加密逻辑很简单 `文件名 + '@' + 密码`
* 前端将密码加盐(分享到期时间戳)进行 160724 次 MD5 获得 `密码 hash`
* 生成文件分享链接 `http(s)://domain.com?s={密码 hash}&n={文件名}&dt={分享到期时间戳}`
* 提取文件时，前端将用户输入的提取码加盐(分享到期时间戳)进行 160724 次 MD5 获得 `提取码 hash` 和 `密码 hash` 进行对比一致则通过
* 如分享时间未到期则构造下载地址新窗口打开进行下载 `http(s)://domain.com/{文件名}@{提取码}?attname=${文件名}`


### 提示

* 有效期及提取码只能相对安全的保护文件，重要文件分享后请到七牛删除
* 使用测试域名请注意[使用规范](https://developer.qiniu.com/fusion/kb/1319/test-domain-access-restriction-rules)
* 七牛有一定量的[免费额度](https://developer.qiniu.com/af/kb/1574/free-credit-information)
