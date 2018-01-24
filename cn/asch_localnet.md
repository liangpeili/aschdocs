# 如何配置一个Asch localnet 节点

## 一、 建议配置

操作系统： Ubuntu 16.06 x64
Node.js 版本： 8.x.x

## 二、 安装依赖

1. 更新系统软件
```
apt update 
apt upgrade
```
2. 安装依赖
```
apt install curl sqlite3 ntp wget git libssl-dev openssl make gcc g++ autoconf automake python build-essential -y
apt install libtool libtool-bin -y
```
3. 安装node.js (已安装的可跳过)
```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" 
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

nvm install 8
```

## 三、 安装asch 
1. 从 github 克隆源代码
```
git clone https://github.com/AschPlatform/asch && cd asch && chmod u+x aschd
```
2. 安装依赖包
```
npm install
```
上一步如果安装失败，请删除目录node_modules，多试几次。
3. 启动应用
```
./aschd start 
```
如果前面几步安装正确，此时aschd应该正常启动了。可以使用 netstat -nltp 检查4096端口是否启动。
4. 访问前端页面
此时访问http://your-ip:4096，返回内容如下：
```
{
  success: false,
  error: "Error: Failed to lookup view "wallet.html" in views directory "/root/asch/public/dist""
}
```

## 四、构建钱包前端界面

1. 安装依赖
```
npm install -g bower
npm install -g gulp
npm install browserify -g
```
2. 构建前端页面
Asch 的前端界面在源码的asch/public目录
```
cd you-path-to-asch/public
npm install
npm run build
gulp build-main
```
3. 前端页面
此时登录http://your-ip:4096， 就可以看到和主网一样的钱包界面了。

## 五、localnet 的账户和受托人

所有的 localnet 里，使用主密码"someone manual strong movie roof episode eight spatial brown soldier soup motor" 登录之后会有1亿个XAS。 101个受托人信息记录在 config.json， 受托人的余额随着区块的锻造而不断增长。