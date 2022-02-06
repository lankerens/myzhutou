#### 安装PM2 步骤

<br>

----------

<br>



>1.安装npm

```shell
yum install npm -y
```

<br>

>2.npm卸载npm

```shell
npm uninstall npm -g
```

<br>

>3.安装gcc

```shell
yum install gcc gcc-c++
```

<br>

>4.自建一个文件夹下载node

```shell
mkdir npm
cd npm
wget https://npm.taobao.org/mirrors/node/v10.14.1/node-v10.14.1-linux-x64.tar.gz
```

<br>

>5.解压

```shell
tar -xvf  node-v10.14.1-linux-x64.tar.gz
mv node-v10.14.1-linux-x64 node
```

<br>

>6.添加环境变量

```shell
vi /etc/profile
  在其中添加

export NODE_HOME=/root/npm/node  
export PATH=$NODE_HOME/bin:$PATH
//注意！！  NODE_HOME后面的值要添加到自己的node解压的路径
```

<br>

>7.刷新配置

```shell
source /etc/profile
```

<br>

> 8.看看自己的版本情况

```shell
node -v
npm -v
```

<br>

>9.安装pm2

```shell
npm install -g pm2

pm2 list
```


<br>


<br>

-------

> Tip: 注意路径