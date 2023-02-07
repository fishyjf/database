
## 简介

EPEL (Extra Packages for Enterprise Linux), 是由 Fedora Special Interest Group 维护的 Enterprise Linux（RHEL、CentOS）中经常用到的包。

下载地址：[https://mirrors.aliyun.com/epel/](https://mirrors.aliyun.com/epel/)

## 配置方法

### 1. 备份(如有配置其他epel源)

mv /etc/yum.repos.d/epel.repo /etc/yum.repos.d/epel.repo.backup
mv /etc/yum.repos.d/epel-testing.repo /etc/yum.repos.d/epel-testing.repo.backup

### 2. 下载新repo 到/etc/yum.repos.d/

**epel(RHEL 8)**  
1）安装 epel 配置包

yum install -y https://mirrors.aliyun.com/epel/epel-release-latest-8.noarch.rpm

2）将 repo 配置中的地址替换为阿里云镜像站地址

sed -i 's|^#baseurl=https://download.example/pub|baseurl=https://mirrors.aliyun.com|' /etc/yum.repos.d/epel* sed -i 's|^metalink|#metalink|' /etc/yum.repos.d/epel*

**epel(RHEL 7)**

wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo

**epel(RHEL 6)** 

wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-archive-6.repo