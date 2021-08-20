### 后端

-------

?> 之前使用了很多的一键式脚本，甚至用完后cpu一直处于 100% 的状态，让我对某些一键式脚本产生了些忧虑，于是，尽量不用就不用 <br>  所以，使用 docker 安装后端



第一步重要

同步时间

Debian/Ubuntu

```shell
apt-get install -y ntp
service ntp restart
```


CentOS/RHEL

```shell
yum -y install ntpdate
timedatectl set-timezone Asia/Shanghai
ntpdate ntp1.aliyun.com
```

关闭防火墙

```shell
systemctl disable firewalld
systemctl stop firewalld
```


<br>


tips： 有时候 ll 返回说ll命令不支持，那么就通过输入 alisa ll = "ls -l" 来执行。 两个等价的，然后通过ll来简化了 ls -l


<br>




#### 安装docker

**CentOS**

```bash
yum install -y yum-utils
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
yum install docker-ce docker-ce-cli containerd.io -y
systemctl start docker
systemctl enable docker
```

**Debian/Ubuntu**

```shell
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
sudo apt-get install docker-ce docker-ce-cli containerd.io -y
systemctl start docker
systemctl enable docker
```



有时候执行上面的中间会到些错，但是不要紧，我们安装上 docker 就行了

**还有其他的方式1**

```shell
#脚本安装最新docker
wget -qO- https://get.docker.com/ | sh
```


**还有其他的方式2**

```bash
apt update
apt install -y  apt-transport-https ca-certificates curl gnupg2 software-properties-common
curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/debian/gpg | apt-key add -
echo "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/debian stretch stable" >>/etc/apt/sources.list
apt update
apt install -y docker-ce
systemctl start docker
systemctl enable docker
```


#### 安装Docker-compose

```shell
curl -fsSL https://get.docker.com | bash -s docker
curl -L "https://github.com/docker/compose/releases/download/1.26.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

有时候执行上面的中间会到些错，但是不要紧，我们安装上 docker-compose 就行了




#### 后端安装

1. `git clone https://github.com/XrayR-project/XrayR-release`
2. cd XrayR-release/config/
3. 编辑配置文件：config.yml，详见：配置文件说明
4. 启动docker：docker-compose up -d



<br>


?> 至此，完成。



<br>


?> 关于配置trojan节点 时 证书的配置 <br>
如果你不懂，你就把两个文件都放到config/cert文件夹里面 <br>
然后文件路径填/etc/XrayR/cert/XXXXXXX  <br>
就可以正常启动了

（ 好像还要在客户端那里配上允许不安全连接




<br>


<br>




------

<br>



>配置文件说明 <br>
> 每个节点是一个独立的配置，互相不会影响，XrayR支持单实例多节点启动，同时对接多个节点。


```yml
Log:
  Level: none # Log level: none, error, warning, info, debug  日志显示级别，none为不显示
  AccessPath: # ./access.Log   Access日志的保存路径
  ErrorPath: # ./error.log     Error日志的保存路径
DnsConfigPath: # ./dns.json Path to dns config
ConnetionConfig:   # 自定义DNS配置文件的路径
  Handshake: 4 # Handshake time limit, Second     连接建立时的握手时间限制。单位为秒。默认值为 4。在入站代理处理一个新连接时，在握手阶段如果使用的时间超过这个时间，则中断该连接。
  ConnIdle: 10 # Connection idle time limit, Second   连接空闲的时间限制。单位为秒。默认值为 10。如果在 ConnIdle 时间内，没有任何数据被传输（包括上行和下行数据），则中断该连接。减少该值有可能可以优化内存占用，但是会导致用户连接延时变高。
  UplinkOnly: 2 # Time limit when the connection downstream is closed, Second   当连接下行线路关闭后的时间限制。单位为秒。默认值为 2。当服务器（如远端网站）关闭下行连接时，出站代理会在等待UplinkOnly时间后中断连接。
  DownlinkOnly: 4 # Time limit when the connection is closed after the uplink is closed, Second   当连接上行线路关闭后的时间限制。单位为秒。默认值为 4。当服务器（如远端网站）关闭上行连接时，出站代理会在等待DownlinkOnly时间后中断连接。
  BufferSize: 64 # The internal cache size of each connection, kB   每个连接的内部缓存大小。单位为 kB。当值为 0 时，内部缓存被禁用。减少该值有可能可以优化内存占用，但有可能导致CPU占用上升
Nodes:
  -
    PanelType: "V2board" # Panel type: SSpanel, V2board, PMpanel    # 选择你的面板类型
    ApiConfig:
      ApiHost: "https://baidu.com"              # 你的面板地址
      ApiKey: "89b2d5327edc52a"                 # 面板与后端对接密钥 （ 你自己设置的
      NodeID: 1                                 # 面板上添加的节点id
      NodeType: V2ray # Node type: V2ray, Shadowsocks, Trojan   # 该节点的类型
      Timeout: 30 # Timeout for the api request    设定单次访问API超时时间，默认5秒
      EnableVless: false # Enable Vless for V2ray Type  是否给V2ray启用Vless协议
      EnableXTLS: false # Enable XTLS for V2ray and Trojan  是否使用XTLS
      SpeedLimit: 0 # Mbps, Local settings will replace remote settings, 0 means disable  单位Mbps, 本地限速设置，会覆盖远程设置，0为不启用
      DeviceLimit: 0 # Local settings will replace remote settings, 0 means disable   本地设备限制，会覆盖远程设置，0为不启用
      RuleListPath: # ./rulelist Path to local rulelist file  本地规则设置，指定本地规则文件路径，规则文件格式
    ControllerConfig:
      ListenIP: 0.0.0.0 # IP address you want to listen  选择监听的IP地址，0.0.0.0会同时监听v6和v4
      SendIP: 0.0.0.0 # IP address you want to send pacakage  用于发送数据的 IP 地址
      UpdatePeriodic: 60 # Time to update the nodeinfo, how many sec.  从前端更新节点、用户信息和上报用户使用信息的间隔，默认60秒
      EnableDNS: false # Use custom DNS config, Please ensure that you set the dns.json well  是否为当前节点启用自定义DNS，默认使用系统DNS
      DNSType: AsIs # AsIs, UseIP, UseIPv4, UseIPv6, DNS strategy  DNS解析类型，AsIs：使用系统DNS，UseIP,UseIPv4,UseIPv6为使用自定义DNS，请确保EnableDNS为true，且正确配置了DnsConfigPath
      EnableProxyProtocol: false # Only works for WebSocket and TCP  是否为当前节点启用ProxyProtocol获取中转IP，只对TCP和WS有效
      EnableFallback: false # Only support for Trojan and Vless   是否为当前节点启用Fallback，只对Vless和Trojan协议有效
      FallBackConfigs:  # Support multiple fallbacks  Fallback 相关配置，请查看 Fallback功能说明
        -
          SNI: # TLS SNI(Server Name Indication), Empty for any
          Path: # HTTP PATH, Empty for any
          Dest: 80 # Required, Destination of fallback, check https://xtls.github.io/config/fallback/ for details.
          ProxyProtocolVer: 0 # Send PROXY protocol version, 0 for dsable
      CertConfig:
        CertMode: none # Option about how to get certificate: none, file, http, dns. Choose "none" will forcedly disable the tls config.   获取证书的方式。file:手动提供，并制定路径。http：通过http申请，需要80端口。dns：使用dns模式申请，需要制定相关dns服务商配置。none：强制关闭tls设置，交由nginx或者caddy处理。
        CertDomain: "test.zhutou.cyou" # Domain to cert  申请证书域名
        CertFile: ./cert/node1.test.zhutou.cyou.cert # Provided if the CertMode is file 手动指定的证书路径
        KeyFile: ./cert/node1.test.zhutou.cyou.key    # 手动指定的私钥路径
        Provider: namesilo # DNS cert provider, Get the full support list here: https://go-acme.github.io/lego/dns/  dns提供商，所有支持的dns提供商请在此获取：https://go-acme.github.io/lego/dns/
        Email: zhuzhu@qq.com
        DNSEnv: # DNS ENV option used by DNS provider   采用DNS申请证书需要的环境变量，请参考上文链接内，自己的dns提供商所需要的参数，填写于此。请注意一行一个，填写时需符合yaml文件格式。
          NAMESILO_API_KEY: aaaa
          #ALICLOUD_SECRET_KEY: bbb
  # -
  #   PanelType: "V2board" # Panel type: SSpanel, V2board
  #   ApiConfig:
  #     ApiHost: "http://127.0.0.1:668"
  #     ApiKey: "123"
  #     NodeID: 4
  #     NodeType: Shadowsocks # Node type: V2ray, Shadowsocks, Trojan
  #     Timeout: 30 # Timeout for the api request
  #     EnableVless: false # Enable Vless for V2ray Type
  #     EnableXTLS: false # Enable XTLS for V2ray and Trojan
  #     SpeedLimit: 0 # Mbps, Local settings will replace remote settings
  #     DeviceLimit: 0 # Local settings will replace remote settings
  #   ControllerConfig:
  #     ListenIP: 0.0.0.0 # IP address you want to listen
  #     UpdatePeriodic: 10 # Time to update the nodeinfo, how many sec.
  #     EnableDNS: false # Use custom DNS config, Please ensure that you set the dns.json well
  #     CertConfig:
  #       CertMode: dns # Option about how to get certificate: none, file, http, dns
  #       CertDomain: "node1.test.com" # Domain to cert
  #       CertFile: ./cert/node1.test.com.cert # Provided if the CertMode is file
  #       KeyFile: ./cert/node1.test.com.pem
  #       Provider: alidns # DNS cert provider, Get the full support list here: https://go-acme.github.io/lego/dns/
  #       Email: test@me.com
  #       DNSEnv: # DNS ENV option used by DNS provider
  #         ALICLOUD_ACCESS_KEY: aaa
  #         ALICLOUD_SECRET_KEY: bbb
```










<br>


<br>

------------------------


<br>

一键安装

```shell
bash <(curl -Ls https://raw.githubusercontent.com/XrayR-project/XrayR-release/master/install.sh)
```

配置文件路径：`/etc/XrayR` 

软件更新 `XrayR update`


<br>


----------------------

<br>



<br>

-------

<p align="right">最近更新： {docsify-updated}</p>




