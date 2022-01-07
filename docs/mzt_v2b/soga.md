因为sspanel的metron主题的后台管理中，不支持单端口多用户的ase-128-gcm 等加密方式，在众多后端中，比如xrayr只支持ase-128-gcm等三种，metron里都不支持，还有air后端也没发现，
最后发现soga后端支持metron也支持的ase-128-cfb,果断选择了，虽然只能有88的有效用户。
但也是目前的一个临时解决方案

---------

<br>

soga后端文档

> https://soga.yougotme.cc/


<br>

目前尚能用的一键安装脚本

> bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/soga/master/install.sh)


<br>

配置文件的位置

> /etc/soga/soga.conf


<br>

-------


<br>

> 最小配置

```yaml

# 基础配置
type=sspanel-uim    #必填这个
server_type=ssr    #必填这个
node_id=4,5,6     
soga_key=

# webapi 或 db 对接任选一个
api=webapi

# webapi 对接信息
webapi_url=https://my.xxxx.com   # webapi url，填写面板主页地址
webapi_key=token   # webapi 密钥，sspanel 配置中的 mukey
 
```

<br>




启动soga

> soga start



