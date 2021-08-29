### 安装vps监控



项目地址： https://github.com/cokemine/ServerStatus-Hotaru

服务端：

```shell
wget https://raw.githubusercontent.com/cokemine/ServerStatus-Hotaru/master/status.sh
# wget https://cokemine.coding.net/p/hotarunet/d/ServerStatus-Hotaru/git/raw/master/status.sh 若服务器位于中国大陆建议选择Coding.net仓库
bash status.sh s
```

客户端：

```shell
bash status.sh c
```


如果要修改网页标题或者网页顶部公告内容，打开/usr/local/ServerStatus/web/index.html文件修改即可，很显眼。
