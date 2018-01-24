## 一、 安装 asch-cli 

作为 Asch 提供的命令行开发工具， asch-cli 提供了一系列在命令行下操作区块链的方法，本篇主要用到 dapp 相关的命令，所以需要先安装 asch-cli . 

```
npm install -g asch-cli
```

## 二、 创建受托人账户

每个 Dapp 都有独立的受托人， 这些受托人负责区块的生产、资产的转账并且获取手续费。 一个Dapp最少有5个受托人， 最多101个。 在注册Dapp之前， 需要先生成受托人，收集受托人的公钥。 

使用`asch-cli crypto -g` 来生成账户，需要输入想要生产的账户个数。

```
root@iZ2ze7j0sus9exe3j2whbcZ:~# asch-cli crypto -g
? Enter number of accounts to generate 5
[ { address: 'AQ1FeHhiv1FQ2cvhNiuceWVcH7vVHMkv9s',
    secret: 'olympic cushion neither ill like dutch sound attract panel nation road recall',
    publicKey: '0352486d87c928918638c8a13c7e4765f2c9fa075318bd680b8d95e1cf1d616f' },
  { address: 'A8RDwpDFL4kiJ6u81wRUqPFSJqmeq3ZHaZ',
    secret: 'hobby gather joke draft crisp author mixed media wide target romance save',
    publicKey: 'cbb3671d343628fe03fba2f0139b783a7b86445f79ee55c99bf5bbe380e4fb46' },
  { address: 'A33Pnj7wDAJKfG2RcWJdomWdUmcnw52MP6',
    secret: 'broccoli arm wine festival suffer supply tourist brown jump plug base caught',
    publicKey: '2f23a9659e32032910b8e078c0c980c6cb7c3a052a235a59079bfd6d608ecc95' },
  { address: 'AM5Y6wsb7n5Qu6X2Qfu8vqNYoncD1L6YRy',
    secret: 'meadow mesh virus video envelope defy fire price feel board together chat',
    publicKey: '351c081c470f41620f4709b6b3ca3721dc920d4e528b27b8fa4d88635e53e128' },
  { address: 'A2MpzHYG6oRnbsyrh7AZPg6oDtG8Drzgci',
    secret: 'crisp sudden segment magnet concert leisure tank iron satisfy hint story floor',
    publicKey: 'cbd808111a08081f7dc93aafd9fe26a70216e3f42d927660ab8d15b7bdf8998c' } ]
Done
```

## 三、 创建模板应用

asch-cli dapps 提供了一系列管理 dapps的命令

```
root@iZ2zec1phkw1g90eglqdw8Z:~# asch-cli dapps -h

  Usage: dapps [options]

  manage your dapps

  Options:

    -a, --add         add new dapp
    -d, --deposit     deposit funds to dapp
    -w, --withdrawal  withdraw funds from dapp
    -i, --install     install dapp
    -u, --uninstall   uninstall dapp
    -g, --genesis     create genesis block
    -h, --help        output usage information
```

我们这里使用 -a 来添加一个新应用

```
创建 dapp 目录
mkdir asch-dapp-test
cd asch-dapp-test

创建应用
root@iZ2ze7j0sus9exe3j2whbcZ:~/asch-dapp-test# asch-cli dapps -a
Copying template to the current directory ...
? Enter DApp name helloworld
? Enter DApp description demo
? Enter DApp tags asch
? Choose DApp category Common
? Enter DApp link http://yourdomain/dapp.zip
? Enter DApp icon url http://yourdomain/logo.png
? Enter public keys of dapp delegates - hex array, use ',' for separator 0352486d87c928918638c8a13c7e4765f2c9fa075318bd680b8d95e1cf1d616f,cbb3671d343628fe03fba2f0139b783a7b86445f79ee55c99bf5bbe380e4fb46,2f23a9659e32032910b8e078c0c980c6cb7c3a052a235a59079bfd6d608ecc95,351c081c470f41620f4709b6b3ca3721dc920d4e528b27b8fa4d88635e53e128,cbd808111a08081f7dc93aafd9fe26a70216e3f42d927660ab8d15b7bdf8998c
? How many delegates are needed to unlock asset of a dapp? 3
DApp meta information is saved to ./dapp.json ...
? Enter master secret of your genesis account [hidden]
? Do you want publish a inbuilt asset in this dapp? Yes
? Enter asset name, for example: BTC, CNY, USD, MYASSET XCT
? Enter asset total amount 100000000
? Enter asset precision 8
New genesis block is created at: ./genesis.json
```
上面的命令完成后，创建一个 DAPP 所需的文件基本都生产了。

说明：

DApp link: 为了方便用户自动安装，链接必须以 .zip 结尾。 这个链接不要求一定存在， 如果未打算开源可以输入无效地址。

DApp icon url: 该选项表示应用的 logo ， 用于在阿希的应用中心展示， 要求以 .jpg 或 .png 结尾。 如果该链接失效，则应用中心会展示一个默认图标。

How many delegates are needed to unlock asset of a dapp： 这个选项表示从dapp跨链转账资产时需要多少个受托人联合签名，该数字必须大于等于3、小于等于你配置的受托人公钥个数且小于等于101，数字越大越安全，但效率和费用越高。

Enter master secret of your genesis account： 这里是创世账户，如果发行内置资产那最初的资产都在这个账户里。

Do you want publish a inbuilt asset in this dapp： dapp 可以创建内置资产，但该内置资产只能在 dapp 内部使用，无法转回到 Asch 主链。 一般建议在主链发行 UIA(用户自定义资产)， 然后将资产充值到 dapp中， 充分享受 Asch 侧链架构的优势。

## 四、在主链注册 DApp
在正式部署dapp之前，需要先在主链注册。 注册需要消耗 100 XAS. 这里我们先生产一个账号， 然后对其充值。 用这个账号来注册 dapp。 

1. 生成一个账号
```
root@iZ2ze7j0sus9exe3j2whbcZ:~# asch-cli crypto -g
? Enter number of accounts to generate 1
[ { address: 'AcL2PfkURQHWzFi6iKRZGsESzhWUTvtNn',
    secret: 'remove sand cushion hobby royal voice game pony bounce sketch enact blast',
    publicKey: '5ce5d531b3cf4168e4487d985a0c9fd52c96fe74b3edd1ec8a9ac159c85e5289' } ]
Done
```
2. 使用localnet  的创世账号对其转账
```
root@iZ2ze7j0sus9exe3j2whbcZ:~# asch-cli sendmoney -e "someone manual strong movie roof episode eight spatial brown soldier soup motor" -a 100000000000 -t AcL2PfkURQHWzFi6iKRZGsESzhWUTvtNn
1c368edaa65316611e5ae8e774943ea97104f1b0d703a4a0812e533bc4ccedca
```
返回交易id， 这里 -a 后面的单位为最小单位。 
查询余额
```
root@iZ2ze7j0sus9exe3j2whbcZ:~# asch-cli getbalance AcL2PfkURQHWzFi6iKRZGsESzhWUTvtNn
100000000000
```
3. 注册应用
```
root@iZ2ze7j0sus9exe3j2whbcZ:~/asch-dapp-test# asch-cli registerdapp -f dapp.json -e "remove sand cushion hobby royal voice game pony bounce sketch enact blast"
7251e913394b521409d13310c50a938ad8a0a76e185a7d055a80b4d02e490c52
```
返回应用id， 这个一定要记住。
4. 访问应用信息
在浏览器访问http://39.107.83.45:4096/api/dapps/get?id=7251e913394b521409d13310c50a938ad8a0a76e185a7d055a80b4d02e490c52 。 返回信息如下：

```
{
success: true,
dapp: {
name: "helloworld",
description: "ff",
tags: "gg",
link: "http://test.zip",
type: 0,
category: 1,
icon: "http://test.log.png",
delegates: [
"0352486d87c928918638c8a13c7e4765f2c9fa075318bd680b8d95e1cf1d616f",
"cbb3671d343628fe03fba2f0139b783a7b86445f79ee55c99bf5bbe380e4fb46",
"2f23a9659e32032910b8e078c0c980c6cb7c3a052a235a59079bfd6d608ecc95",
"351c081c470f41620f4709b6b3ca3721dc920d4e528b27b8fa4d88635e53e128",
"cbd808111a08081f7dc93aafd9fe26a70216e3f42d927660ab8d15b7bdf8998c"
],
unlockDelegates: 3,
transactionId: "7251e913394b521409d13310c50a938ad8a0a76e185a7d055a80b4d02e490c52"
}
}
```

表明应用已经注册成功。

## 五、部署应用到节点
1. 拷贝应用到asch目录
```
cp -r asch-dapp-test asch/dapps/7251e913394b521409d13310c50a938ad8a0a76e185a7d055a80b4d02e490c52
```
2. 修改受托人信息
这里填写上面生成的5个受托人的主密码。
```
root@iZ2ze7j0sus9exe3j2whbcZ:~/asch# cat dapps/7251e913394b521409d13310c50a938ad8a0a76e185a7d055a80b4d02e490c52/config.json
{
        "secrets": [
            "olympic cushion neither ill like dutch sound attract panel nation road recall",
            "hobby gather joke draft crisp author mixed media wide target romance save",
            "broccoli arm wine festival suffer supply tourist brown jump plug base caught",
            "meadow mesh virus video envelope defy fire price feel board together chat",
            "crisp sudden segment magnet concert leisure tank iron satisfy hint story floor"
        ]
}
```
3. 重启节点，访问DApp
```
./aschd restart
```
此时访问http://39.107.83.45:4096/dapps/7251e913394b521409d13310c50a938ad8a0a76e185a7d055a80b4d02e490c52/ 界面应该如下。

![1](http://asch-public.oss-cn-beijing.aliyuncs.com/markdown/_1516613173_27972.png)

## 六、DApp 的充值和转账
使用我们在创建dapp时填写的创世账户登录dapp， 可以看到我们发行的内置资产。
![2](http://asch-public.oss-cn-beijing.aliyuncs.com/markdown/_1516613208_26020.png)

其他主链上已经拥有的资产也可以充值的侧链里。主要有两周方式： 1. web 钱包； 2. asch-cli 。 这里演示一下如何使用asch-cli 向dapp 充值。
```
root@iZ2ze7j0sus9exe3j2whbcZ:~/asch# asch-cli deposit -e "remove sand cushion hobby royal voice game pony bounce sketch enact blast" -d 7251e913394b521409d13310c50a938ad8a0a76e185a7d055a80b4d02e490c52 -c XAS -a 10000000000
665d16d12b0434481c87ca702631f51da477ef107cee45a35dab8ffe600dfb8e
```
充值完成后，使用相同的主密码登录dapp， 可以看到XAS已经从主链转移到了dapp内部。
![3](http://asch-public.oss-cn-beijing.aliyuncs.com/markdown/_1516613959_17598.png)

