---
title: GPG 入门教程
date: 2024-01-18 18:34:57
tags: GPG
Categories: Software
---
如何使用这个最流行、最好用的加密工具之一
<!-- more -->
# 什么是GPG
1991年，程序员 [Phil Zimmermann](https://en.wikipedia.org/wiki/Phil_Zimmermann) 为了避开政府监视，开发了加密软件PGP。这个软件非常好用，迅速流传开来，成了许多程序员的必备工具。但是，它是商业软件，不能自由使用。所以，自由软件基金会决定，开发一个 PGP 的替代品，取名为 GnuPG，也就是 GPG
# 安装
GPG有两种安装方式。可以[下载源码](http://www.gnupg.org/download/index.en.html)，自己编译安装
```zsh
./configure
make
make install
```
也可以安装编译好的二进制包
```zsh
pacman -S gnupg # Arch Linux
apt install gnupg # Debian 系
dnf in gnupg # RH 系
zypper in gnupg # 蜥蜴系
```
# 生成密钥
安装成功后，使用gen-ken参数生成自己的密钥
```zsh
gpg --gen-key
```
会输出
```text
gpg (GnuPG) 2.4.3; Copyright (C) 2023 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

注意：使用 “gpg --full-generate-key” 以获得一个全功能的密钥生成对话框。

GnuPG 需要构建用户标识以辨认您的密钥。

真实姓名：
```
这里需要按照问题填，填了回车，后按`O`确定
```text
gpg (GnuPG) 2.4.3; Copyright (C) 2023 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

注意：使用 “gpg --full-generate-key” 以获得一个全功能的密钥生成对话框。

GnuPG 需要构建用户标识以辨认您的密钥。

真实姓名： zlicdt
电子邮件地址： xkicdt@163.com
您选定了此用户标识：
    “zlicdt <xkicdt@163.com>”

更改姓名（N）、注释（C）、电子邮件地址（E）或确定（O）/退出（Q）？
```
然后它会让你“输入密码以保护您的新密钥”，**这个密码最好填8位以上的**，否则会
```text
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│ 警告：您输入了一个不安全的密码                                                               │
│                                                                                         │
│ A passphrase should be at least 8 characters long.                                      │
│                                                                                         │
│ <无论如何都使用这个>                                           <输入新的密码>                │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```
然后，它会
```text
我们需要生成大量的随机字节。在质数生成期间做些其他操作（敲打键盘
、移动鼠标、读写硬盘之类的）将会是一个不错的主意；这会让随机数
发生器有更好的机会获得足够的熵。
```
最终生成一个有效期为三年的 GPG key
# 高级密钥生成
```zsh
gpg --full-gen-key
```
```text
gpg (GnuPG) 2.4.3; Copyright (C) 2023 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

请选择您要使用的密钥类型：
   (1) RSA 和 RSA
   (2) DSA 和 Elgamal
   (3) DSA（仅用于签名）
   (4) RSA（仅用于签名）
   (9) ECC（签名和加密） *默认*
  (10) ECC（仅用于签名）
 （14）卡中现有密钥
您的选择是？
```
这里建议直接选择默认的 ECC
```text
请选择您想要使用的椭圆曲线：
   (1) Curve 25519 *默认*
   (4) NIST P-384
   (6) Brainpool P-256
您的选择是？
请设定这个密钥的有效期限。
         0 = 密钥永不过期
      <n>  = 密钥在 n 天后过期
      <n>w = 密钥在 n 周后过期
      <n>m = 密钥在 n 月后过期
      <n>y = 密钥在 n 年后过期
密钥的有效期限是？(0)
密钥永远不会过期
这些内容正确吗？ (y/N) y

GnuPG 需要构建用户标识以辨认您的密钥。

真实姓名：
```
后来就和上部分一样了
# 密钥管理
## 列出密钥
```zsh
gpg --list-keys
```
结果如下：
```text
pub   ed25519 2024-01-18 [SC] [有效至：2027-01-17]
      F624D1B3CC89BF5760DCF150A30A42DE01DE13B5
uid             [ 绝对 ] zlicdt <xkicdt@163.com>
sub   cv25519 2024-01-18 [E] [有效至：2027-01-17]
```
第一行显示公钥特征（算法，Hash字符串和生成时间），第二行显示"用户ID"，第三行显示私钥特征

如果你要从密钥列表中删除某个密钥，可以使用delete-key参数：
```zsh
gpg --delete-key [用户ID]
```
**不过这样的话，如果它对应一个私钥的话，需要先删掉私钥，它会给出命令提示**
## 输出密钥
公钥文件以二进制形式储存，armor 参数可以将其转换为 ASCII 码显示
```zsh
gpg --armor --export [用户ID]
```
类似地，export-secret-keys 参数可以转换私钥
```zsh
gpg --armor --export-secret-keys
```
## 上传公钥
公钥服务器是网络上专门储存用户公钥的服务器。send-keys 参数可以将公钥上传到服务器
```zsh
gpg --send-keys [用户ID] --keyserver https://keys.openpgp.org
```
使用上面的命令，你的公钥就被传到了服务器 subkeys.pgp.net，然后通过交换机制，所有的公钥服务器最终都会包含你的公钥

由于公钥服务器没有检查机制，任何人都可以用你的名义上传公钥，所以没有办法保证服务器上的公钥的可靠性。通常，你可以在网站上公布一个公钥指纹，让其他人核对下载到的公钥是否为真。fingerprint 参数生成公钥指纹
```zsh
gpg --fingerprint [用户ID]
```
## 导入密钥
除了生成自己的密钥，还需要将他人的公钥或者你的其他密钥输入系统。这时可以使用 import 参数
```zsh
gpg --import [密钥文件]
```
为了获得他人的公钥，可以让对方直接发给你，或者到公钥服务器上寻找
```zsh
gpg --keyserver https://keys.openpgp.org --search-keys [用户ID]
```
# 加密和解密
## 加密
```zsh
gpg --recipient [用户ID] --encrypt demo.txt
```
这会在当前目录下生成一个 demo.txt.gpg 文件，cat 其中的内容发现是已经经过加密的，不可读取

这时候传输文件就是安全的
## 解密
对方收到加密文件以后，就用自己的私钥解密
```zsh
gpg --decrypt demo.txt.gpg
```
此时它会在 terminal 里直接输出文件内容
GPG 允许省略 decrypt 参数，直接可以达到相同的效果
# 签名
## 对文件签名
有时我们不需要加密文件，只需要对文件签名，表示这个文件确实是我本人发出的。
sign 参数用来签名
```zsh
gpg --sign demo.txt
```
运行上面的命令后，当前目录下生成 demo.txt.gpg 文件，这就是签名后的文件。这个文件默认采用二进制储存，如果想生成 ASCII 码的签名文件，可以使用 clearsign 参数
```zsh
gpg --clearsign demo.txt
```
运行上面的命令后 ，当前目录下生成demo.txt.asc文件，后缀名asc表示该文件是ASCII码形式的，现在这个可读了

如果想生成单独的签名文件，与文件内容分开存放，可以使用 detach-sign 参数
```zsh
gpg --detach-sign demo.txt
```
运行上面的命令后，当前目录下生成一个单独的签名文件 demo.txt.sig。该文件是二进制形式的，如果想采用 ASCII 码形式，要加上 armor 参数
```zsh
gpg --armor --detach-sign demo.txt
```
## 签名+加密
上一节的参数，都是只签名不加密。如果想同时签名和加密，可以使用下面的命令
```zsh
gpg --local-user [发信者ID] --recipient [接收者ID] --armor --sign --encrypt demo.txt
```
local-user 参数指定用发信者的私钥签名，recipient 参数指定用接收者的公钥加密，armor 参数表示采用 ASCII 码形式显示，sign 参数表示需要签名，encrypt 参数表示指定源文件
## 验证签名
我们收到别人签名后的文件，需要用对方的公钥验证签名是否为真。verify 参数用来验证
```zsh
gpg --verify demo.txt.asc demo.txt
```
# 参考文档
1. Paul Heinlein, [GPG Quick Start](http://www.madboa.com/geek/gpg-quickstart/)
2. Ubuntu help, [GnuPrivacyGuardHowto](https://help.ubuntu.com/community/GnuPrivacyGuardHowto)
3. KNL, [GnuPG Tutorial](http://www.bitflop.com/document/129)
4. Alan Eliasen. [GPG Tutorial](http://futureboy.us/pgp.html)
5. [The GNU Privacy Handbook](http://www.gnupg.org/gph/en/manual.html)
6. [GPG入门教程 - 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2013/07/gpg.html)