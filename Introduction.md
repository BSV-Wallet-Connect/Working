# BSV WalletConnect介绍

## 1.目标

BSV WalletConnect是一个开放协议，用于钱包和DApp的安全交互。这个协议创建一个远程链接。并且提供一些标准API，用于实现转帐，签名等常见用途。

DApp开发者不需要理解UTXO，输入输出等等概念，只需要专注于业务实现。而由钱包负责这些事情。

BSV WalletConnect is an open protocol to communicate securely between Wallets and BSV Dapps. The protocol establishes a remote connection between two apps and/or devices. and provide some API to send payment, sign message, encrypt data and more.

The developer of DApp don't need care of UTXO, transaction inputs and outputs. Wallet will process transaction correctly.

## 2.设计课题

### 2.1关于连接

我们倾向于支持无服务器（Serverless）的网页或者手机App提供的DApp服务。网页浏览器可能通过LAN Cable等多种方式连入互联网。钱包则是通过4G上网的手机（比如HandCash或者SimplyCash钱包），或者是一个中心化服务器提供的钱包服务（比如DotWallet或者MoneyButton）。因此DApp同钱包并不在直接互联的环境之中。由DApp提供登录用二维码或者登录字符串链接，钱包识别链接，进而完成登录。

The connection is initiated by one peer displaying a QR Code or deep link with a standard WalletConnect URI and is established when the counter-party approves this connection request.

为了实现DApp同钱包的通信，我们需要设计一个桥接服务（Bridge Server）。目前有几个方案正在考察。我们需要同时考虑通信效率和安全性。

- DAPP自己构建基于开源代码的websocket服务器，是一个可行的技术方案。但可能存在单点故障。

- 基于BSV网络的Payment Channel是我们目前考虑的革新方案。如果可以通过比特币节点做桥接，则使用去中心化的服务防止了单点故障。

- 通过Google Firebase的Cloud Firestore服务。但在一些国家（比如:中国）无法访问Google服务器

另外也要考虑钱包作为一个浏览器插件或者就是浏览器本身的情况（比如Maxthon6浏览器）。DApp在浏览器环境之中，在实现上应该自动检测并且支持这种宿主连接的方式。

### 2.2关于标准API

#### 2.2.1支付

进行P2PKH的标准转账。指定接收方地址（1开始的地址，或者paymail地址），转帐金额（Satoshi单位），同时可以指定上链文字memo。

#### 2.2.2创建签名

用钱包私钥对一段文字的哈希进行签名，哈希是标准SHA256。

#### 2.2.3校验签名

检查一段签名是否有效。

#### 2.2.4模版支付

支持BIP270交易。

#### 2.2.4Token支付

TODO

#### 2.2.5多输出上链

可以对一段或者多段字节数组上链。方便内容上链的需求。

#### 2.2.6ECIES加密解密（可选）

对一段文字ECC加密以及解密

### 2.3关于推送通知

在智能手机钱包的情况下，DAPP在调用API时，应该有办法通知钱包或者打开钱包，进行用户确认。

It also includes an optional Push server to allow Native applications to notify the user of incoming payloads for established connections.

## 3.参考

[ETh WalletConnect](https://walletconnect.org/)
