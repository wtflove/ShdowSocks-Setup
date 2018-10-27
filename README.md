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
[Official Instruction](https://github.com/shadowsocks/shadowsocks-manager)<br>
From npm:<br>
sudo npm i -g shadowsocks-manager-hk --safe-perm<br>
<br>
ss.yml<br>
type: s<br>
empty: false<br>
shadowsocks:<br>
        address: 127.0.0.1:4000<br>
<br>
manager:<br>
        address: 0.0.0.0:4001<br>
        password: '12345'<br>
<br>
db:<br>
        host: '127.0.0.1'<br>
        user: 'root'<br>
        password: 'deadly1943'<br>
        database: 'ssdb'<br>
<br>
ssmgr-ss.service<br>
<br>
[Unit]<br>
Description=ss.yml service<br>
Documentation=man:shadowsocks-libev(8)<br>
After=shadowsocks-libev.service<br>
<br>
[Service]<br>
Type=simple<br>
User=root<br>
Group=root<br>
ExecStart=/usr/bin/ssmgr -c /home/kediqi/.ssmgr/ss.yml<br>
<br>
[Install]<br>
WantedBy=multi-user.target<br>
<br>
type: m<br>
<br>
manager:<br>
    address: 127.0.0.1:4001<br>
    password: '12345'<br>
		# 这部分的IP地址需要改为你服务器的IP地址，端口和密码要跟上一步 ss.xml 文件中 manager 参数里的保持一致，以便 webgui 服务连接 type s 部分监听的 tcp 端口。<br>
plugins:<br>
    flowSaver:<br>
        use: true<br>
    user:<br>
        use: true<br>
    group:<br>
        #群组<br>
        use: true<br>
    account:<br>
        use: true<br>
    macAccount:<br>
        use: true<br>
    giftcard:<br>
        #礼品卡<br>
        use: true<br>
    email:<br>
        use: true<br>
        type: 'smtp'<br>
        username: 'postmaster@mg.redseagull.live'<br>
        password: '2c08dd61de260e8e5f04117b356683dd-b0aac6d0-67e301db'<br>
        host: 'smtp.mailgun.org'<br>
        # 这部分的邮箱和密码是用于发送注册验证码等邮件，必须配置<br>
    webgui:<br>
        use: true<br>
        host: '0.0.0.0'<br>
        port: '80'<br>
        site: 'http://bpr.redseagull.live'<br>
        # site要修改为你服务器的IP地址或域名<br>
        # cdn: 'http://xxx.com' # 静态资源cdn地址，可省略<br>
        # icon: 'icon.png' # 自定义首页图标，默认路径在 ~/.ssmgr 可省略<br>
        # googleAnalytics: 'UA-xxxxxxxx-x' # Google Analytics ID，可省略<br>
        #不知道是干啥的<br>
        #gcmSenderId: '456102641793'<br>
        #gcmAPIKey: 'AAAAGzzdqrE:XXXXXXXXXXXXXX'<br>
    alipay:<br>
        # 如果不使用支付宝，这段可以去掉，这里默认关闭<br>
        use: false<br>
        appid: 2015012108272442<br>
        notifyUrl: 'http://yourwebsite.com/api/user/alipay/callback'<br>
        merchantPrivateKey: 'xxxxxxxxxxxx'<br>
        alipayPublicKey: 'xxxxxxxxxxx'<br>
        gatewayUrl: 'https://openapi.alipay.com/gateway.do'<br>
    paypal:<br>
        # 如果不使用paypal，这段可以去掉，这里默认关闭<br>
        use: false<br>
        mode: 'live' # sandbox or live<br>
        client_id: 'xxxxxxxxxxxxx'<br>
        client_secret: 'xxxxxxxxxxxx'<br>
    	# db: 'webgui.sqlite'<br>
db:<br><br>
    host: '127.0.0.1'<br>
    user: 'root'<br>
    password: 'deadly1943'<br>
    database: 'webguidb'<br>
<br>
ssmgr-webgui.service<br>
[Unit]<br>
Description=webgui.yml service<br>
Documentation=man:shadowsocks-libev(8)<br>
After=ssmgr-ss.service<br>
<br>
[Service]<br>
Type=simple<br>
User=root<br>
Group=root<br>
ExecStart=/usr/bin/ssmgr -c /home/kediqi/.ssmgr/webgui.yml<br>
<br>
[Install]<br>
WantedBy=multi-user.target<br>