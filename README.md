ShdowSocks-Setup
===
## 1. install shadowsocks-libev
[official instruction](https://github.com/shadowsocks/shadowsocks-libev#debian--ubuntu)<br>
### Ubuntu 16.10+ <br>
sudo apt update <br>
sudo apt install shadowsocks-libev <br>
<br>
## 2. install node.js & npm
sudo apt install nodejs<br>
(if npm not installed, run: sudo apt install npm)<br>
## 3. install shadowsocks-manager
[official instruction](https://github.com/shadowsocks/shadowsocks-manager)<br>
From npm:<br>
sudo npm i -g shadowsocks-manager-hk --safe-perm

ss.yml
type: s
empty: false
shadowsocks:
        address: 127.0.0.1:4000

manager:
        address: 0.0.0.0:4001
        password: '12345'

db:
        host: '127.0.0.1'
        user: 'root'
        password: 'deadly1943'
        database: 'ssdb'

ssmgr-ss.service
[Unit]
Description=ss.yml service
Documentation=man:shadowsocks-libev(8)
After=shadowsocks-libev.service

[Service]
Type=simple
User=root
Group=root
ExecStart=/usr/bin/ssmgr -c /home/kediqi/.ssmgr/ss.yml

[Install]
WantedBy=multi-user.target

type: m

manager:
    address: 127.0.0.1:4001
    password: '12345'
		# 这部分的IP地址需要改为你服务器的IP地址，端口和密码要跟上一步 ss.xml 文件中 manager 参数里的保持一致，以便 webgui 服务连接 type s 部分监听的 tcp 端口。
plugins:
    flowSaver:
        use: true
    user:
        use: true
    group:
        #群组
        use: true
    account:
        use: true
    macAccount:
        use: true
    giftcard:
        #礼品卡
        use: true
    email:
        use: true
        type: 'smtp'
        username: 'postmaster@mg.redseagull.live'
        password: '2c08dd61de260e8e5f04117b356683dd-b0aac6d0-67e301db'
        host: 'smtp.mailgun.org'
        # 这部分的邮箱和密码是用于发送注册验证码等邮件，必须配置
    webgui:
        use: true
        host: '0.0.0.0'
        port: '80'
        site: 'http://bpr.redseagull.live'
        # site要修改为你服务器的IP地址或域名
        # cdn: 'http://xxx.com' # 静态资源cdn地址，可省略
        # icon: 'icon.png' # 自定义首页图标，默认路径在 ~/.ssmgr 可省略
        # googleAnalytics: 'UA-xxxxxxxx-x' # Google Analytics ID，可省略
        #不知道是干啥的
        #gcmSenderId: '456102641793'
        #gcmAPIKey: 'AAAAGzzdqrE:XXXXXXXXXXXXXX'
    alipay:
        # 如果不使用支付宝，这段可以去掉，这里默认关闭
        use: false
        appid: 2015012108272442
        notifyUrl: 'http://yourwebsite.com/api/user/alipay/callback'
        merchantPrivateKey: 'xxxxxxxxxxxx'
        alipayPublicKey: 'xxxxxxxxxxx'
        gatewayUrl: 'https://openapi.alipay.com/gateway.do'
    paypal:
        # 如果不使用paypal，这段可以去掉，这里默认关闭
        use: false
        mode: 'live' # sandbox or live
        client_id: 'xxxxxxxxxxxxx'
        client_secret: 'xxxxxxxxxxxx'
    	# db: 'webgui.sqlite'
db:
    host: '127.0.0.1'
    user: 'root'
    password: 'deadly1943'
    database: 'webguidb'
#
ssmgr-webgui.service
[Unit]
Description=webgui.yml service
Documentation=man:shadowsocks-libev(8)
After=ssmgr-ss.service

[Service]
Type=simple
User=root
Group=root
ExecStart=/usr/bin/ssmgr -c /home/kediqi/.ssmgr/webgui.yml

[Install]
WantedBy=multi-user.target