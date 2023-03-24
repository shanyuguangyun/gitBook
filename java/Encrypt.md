# 加密

### 1. Base64
> Base64并不是加密算法，只是一种编码格式。但是由于经常使用，所以稍微提一下。

> 原理：将字符转成ascii码对应的索引，然后二进制，将二进制每6bit一组，然后找到base64码表的索引。对应的就是他编码后的字符了。

![image.png](https://cdn.nlark.com/yuque/0/2021/png/454950/1634734090770-bbef3345-77bd-4f24-9217-8369c23b1396.png#clientId=u5e57a78c-eaad-4&from=paste&height=1187&id=u9d13e4c5&name=image.png&originHeight=1187&originWidth=1008&originalType=binary&ratio=1&size=165292&status=done&style=none&taskId=u45d70beb-731f-4191-84d6-eb5745c172c&width=1008)

![image.png](https://cdn.nlark.com/yuque/0/2021/png/454950/1634734130584-190e93fd-f132-4a67-9bfb-a7ba5073a393.png#clientId=u5e57a78c-eaad-4&from=paste&height=594&id=u55c2a1c4&name=image.png&originHeight=594&originWidth=879&originalType=binary&ratio=1&size=67150&status=done&style=none&taskId=u27d931af-03f3-48ab-9da2-21febcffbdc&width=879)

如 YNX 这三个字符转换过程如下

Y		N		X
89		88		78
01011001	01011000	01001110
010110 010101 100001 001110
22	   21		33	14
W	   V		h	O		

转换后就是WVhO这个字符了


### 加密演进

> 网上已经有了很多很好的文章讲解加密，大概逻辑就是如下。

> 1、 A发消息给B，但由于数据在网络是明文传输，所以不安全。

A -> B

> 2、A给了个密钥发送给B，说以后都用这个密钥依赖对称加密算法进行加密、解密。 
> 但是由于密钥约定的时候也可能被人截取，所以也不行。
> 

A -> (encrypt by key) -> B
 
> 3、商量了后搞了非对称加密，就是A、B两个人都有公钥和私钥。 A给B发消息的时候用B的公钥进行加密，B收到消息后用自己的私钥进行解密。同理B给A发消息也是一样，用A的公钥进行加密...
> 

A -> (encrypt by B's public key)  -> B
B decrypt by B's private key
B -> (encrypt by A's public key) -> A
A decrypt by A's private key

> 4、这里解决了加密的问题，但是身份的问题如何解决呢，假如C使用了B的公钥给B发了消息，B一样解密后不知道那头是不是之前一直来往的那个人。这里搞了个数字签名。A给B发消息的时候，同时A用自己私钥对自己的消息内容摘要进行加密，这个叫数字签名。而B收到消息后会用A的公钥进行验签。用A的公钥解密获取消息摘要后判断和自己计算的消息摘要是否一致。（非对称加密就是公钥加密可以用私钥解密，私钥加密可以用公钥解密）用来证明这个发消息的人是唯一指定的那个人。 同时消息内容B用自己的私钥解密获取。

A -> (encrypt by B's public key && signal by A's private key) -> B

> 5、但是到这一步了还是不行，上一步只能确定消息来往的是同一个人，但是怎么确定这个人是你认知上的那个人呢。这个时候就需要可信的公证机构，来给每个人一个认知上的身份。这就是CA(Certificate Authority) 数字证书认证机构。 A给B发消息时先自己申请个证书，里面有自己的身份信息和自己的公钥，这部分是用CA的私钥进行加密的。发消息时会将证书也发出去。 然后B本地安装个CA证书，这里面有CA的公钥。收到消息时会解密消息过来的证书。获取到A的身份信息。

A -> (... && CA) -> B


### 不可逆加密

> 由上面可知，不可逆加密基本都只是用来做对消息内容进行处理，将其变成消息摘要

#### 1. MD5

#### 2. SHA1 SHA256 SHA512

#### 3. HMAC

### 可逆加密

#### 1. 对称加密


#### 2. 非对称加密