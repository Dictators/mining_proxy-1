## 特性

- 没有开发者费率(无论你抽水或者不抽水)
- 展示抽水份额
- 支持不同代理池抽水到一个池的一个钱包
- Rustlang 编写。性能高。延迟低。
- 支持双端加密传输。无法识别行为。
- Bug反馈 ：https://t.me/+ZkUDlH2Fecc3MGM1

## 乞讨地址

Eth+BSC+HECO+Matic: 0x3602b50d3086edefcd9318bcceb6389004fb14ee

接受一切可以卖的出去的币种。就是这么可怜！！！！

目前支持币种：

- ETH

## 1. 使用教程

### 0. 一键安装脚本

```shell
sudo apt install -y wget
## 或
sudo yum install -y wget

#执行下面的命令
bash <(curl -s -L https://raw.githubusercontent.com/dothinkdone/mining_proxy/main/script/install.sh)
## 或
wget -c https://raw.githubusercontent.com/dothinkdone/mining_proxy/main/script/install.sh
sh install.sh
```

### 1.linux 

- 油管:   https://youtu.be/YEB-rXnPI2A

### 2. windows 

油管:   https://youtu.be/uzDzCF0OXmo

### 3. linux docker

TODO

## 2. 使用说明

#### 1.支持及BUG反馈
- TG : [TG](https://t.me/+ZkUDlH2Fecc3MGM1)
- QQ群 : 724855814

#### 2.系统支持
- windows 含32位及64位
- Linux

#### 3. 启动方式
下载链接在

https://github.com/dothinkdone/minerProxy/releases

启动命令为

./proxy -c config.yaml

不传入-c 命令。默认查找当前目录下的default.yaml

此方式可使用相对路径日志路径及证书路径等。

##### 后台常驻内存方式
```shell
nohup ./proxy > stdout.log &
```
##### docker 模式

目录结构

```shell
.
├── identity.p12
└── logs
```

可以启动多个矿池进行转发,修改相应配置就可以。没有添加wallet地址。自己添加一下。

```shell
docker run -d \
--name=ethermine \
-e PROXY_NAME="ethermine" \
-e PROXY_LOG_LEVEL=2 \
-e PROXY_LOG_PATH="/var/logs/" \
-e PROXY_TCP_PORT=8800 \
-e PROXY_SSL_PORT=14443 \
-e PROXY_ENCRYPT_PORT=0 \
-e PROXY_POOL_ADDRESS="tcp://asia2.ethermine.org:5555" \
-e PROXY_SHARE_TCP_ADDRESS="tcp://asia2.ethermine.org:14444" \
-e PROXY_SHARE_WALLET="" \  ## 这里钱包没填。要替换成自己的。
-e PROXY_SHARE_RATE=0.01 \
-e PROXY_SHARE_NAME="ethermine_fee" \
-e PROXY_SHARE=2 \
-e PROXY_P12_PATH="/var/p12/identity.p12" \
-e PROXY_P12_PASS="mypass" \
-e PROXY_SHARE_ALG=1 \
-e PROXY_KEY="523B607044E6BF7E46AF75233FDC1278B7AA0FC42D085DEA64AE484AD7FB3664" \
-e PROXY_IV="275E2015B9E5CA4DDB87B90EBC897F8C" \
-v $(pwd)/identity.p12:/var/p12/identity.p12 \
-v $(pwd)/logs/:/var/logs/ \
yusongwang/eth-proxy:v0.2.0
```



##### docker-compose 模式多矿池
```shell

git clone https://github.com/dothinkdone/minerProxy.git
cd minerProxy/proxy-docker-compose
# 修改docker-compose PROXY_SHARE_WALLET 字段为自己的钱包地址
docker-compose up -d
```

#### 目前已验证支持
- ethermine
- 币安
- 币印
- 2miners
- f2pool
- flexpool



### 加密客户端

需要运行在机器同局域网下。将机器的链接地址以TCP方式链接到此机器。

此机器运行程序命令

```shell
encrypt -i 275E2015B9E5CA4DDB87B90EBC897F8C -k 523B607044E6BF7E46AF75233FDC1278B7AA0FC42D085DEA64AE484AD7FB3664 -p 8855 -s 中转服务器的IP + 端口
```

其中-i -k 需要与服务器端一致

#### 配置文件说明

```yaml
name: "ethermine" #日志名称为多矿池方便打印日志
log_level: 2 #日志等级 2=INFO 1=DEBUG
log_path: "logs" # 日志路径。支持绝对路径
ssl_port: 8443 # SSL监听地址
tcp_port: 14444 # TCP监听地址
encrypt_port: 14445 #加密通讯端口
pool_address: 
  - "tcp://asia2.ethermine.org:5555" #矿池SSL地址. 例如: "asia2.ethermine.org:5555"
  - "tcp://asia1.ethermine.org:5555"
share_address: 
  - "tcp://asia-eth.2miners.com:2020" #抽水 矿池TCP地址. 例如: "asia2.ethermine.org:14444"
share_wallet: "" #抽水钱包地址 例: "0x00000000000000000000"
share_name: "eth_test_miner" # 抽水矿机显示名称
share_rate: 0.05 # 抽水率 支持千分位0.001 就是千分之一。百分之1就是0.01,没有上限
share: 1 #抽水矿池链接方式0=不抽水 1=TCP池
share_alg: 0 #抽水算法。 0 为随机算法 1 为固定份额算法。
p12_path: "./identity.p12" # p12证书地址 可用脚本generate-certificate.sh生成
p12_pass: "mypass" #默认generate-certificate.sh 中密码为mypass如果修改了脚本中得密码需要同步修改配置文件中的密码
key: "523B607044E6BF7E46AF75233FDC1278B7AA0FC42D085DEA64AE484AD7FB3664" #秘钥
iv: "275E2015B9E5CA4DDB87B90EBC897F8C" # 秘钥向量
```
