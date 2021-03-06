# Debian操作系统安装Docker
##支持的版本
- Debian testing stretch (64-bit)
- Debian 8.0 Jessie (64-bit)
- Debian 7.7 Wheezy (64-bit)

##预安装

###确认内核版本符合要求

　　Docker支持64位、内核高于3.10的Debian操作系统，内核低于3.10将导致数据丢失和系统不稳定等问题。
查看内核版本使用以下命令：
```
$ uname -r
```
###更新APT仓库

　　Docker的APT仓库包含了1.7.1及以上版本的Docker，安装前需要更新APT设置，来使用新的仓库：

 1. 清理旧的仓库信息(如果不是首次安装的话)
 ```sh
 $ apt-get purge lxc-docker*
 $ apt-get purge docker.io*
 ```
 2. 更新和安装软件包
 ```sh
 $ apt-get update
 $ apt-get install apt-transport-https ca-certificates
 ```
 3. 添加GPG键
```
 $ apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
```
 4. 添加APT源 
 
   编辑文件 ```/etc/apt/sources.list.d/docker.list```,清理已存在的信息，写入APT源地址内容。以下以Debian Jessie为例，非Jessie版本的系统注意修改为自己对应的代号。
```sh
$ sudo cat <<EOF > /etc/apt/sources.list.d/docker.list
deb https://apt.dockerproject.org/repo debian-jessie main
EOF
```
其他两个版本(wheezy,stretch)内容：
```
deb https://apt.dockerproject.org/repo debian-wheezy main
```
```
deb https://apt.dockerproject.org/repo debian-stretch main
```
 5. 更新APT本地包索引
```
$ apt-get update
```
 6. 校验设置安装结果，确认APT可以从正确的仓库重下载
```
 $ apt-cache policy docker-engine
docker-engine:
  Installed: 1.11.0-0~jessie
  Candidate: 1.11.0-0~jessie
  Version table:
 *** 1.11.0-0~jessie 0
        500 https://apt.dockerproject.org/repo/ debian-jessie/main amd64 Packages
        100 /var/lib/dpkg/status
  .....
```
以后，当执行```apt-get upgrade```等命令时，将使用新设置的的APT源。

##安装Docker

 1. 更新APT本地包索引
```
$ sudo apt-get update
```
 2. 安装Docker
```
$ sudo apt-get install docker-engine
```
 3. 启动docker守护进程
```
sudo service docker start
```
 4. 校验安装结果
```
$ sudo docker run hello-world
```

## 使用非root用户管理Docker

 1. 如果没有就建立一个Docker组.
```
$ sudo groupadd docker
```

 2. 增加一个用户（用真实的名字替换下面的${USER}）到docker组,需用户重新登陆来生效。
```
$ sudo gpasswd -a ${USER} docker
```
 3. 重启docker服务
```
$ sudo service docker restart
```

##更新Docker

```
$ apt-get upgrade docker-engine
```
##卸载Docker


 1. 卸载软件包
```sh
$ sudo apt-get purge docker-engine
```
 2. 卸载依赖包
```sh
$ sudo apt-get autoremove --purge docker-engine
```
 3. 如有必要，执行以下命令，删除全部镜像、容器、数据卷和其他docker相关用户信息:
```sh
$ rm -rf /var/lib/docker
```

