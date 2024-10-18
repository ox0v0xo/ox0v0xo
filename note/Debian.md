# Debian

> in VMware Workstation

- [Debian](#debian)
  - [sudo](#sudo)
  - [配置 NAT 虚拟机 IP](#配置-nat-虚拟机-ip)
  - [Windows 清除 ssh](#windows-清除-ssh)
  - [允许 root 登录 ssh](#允许-root-登录-ssh)
  - [Docker](#docker)
  - [rust](#rust)
  - [java](#java)

## sudo

```bash
su -
apt install sudo
usermod -aG sudo oo
su - oo
```

## 配置 NAT 虚拟机 IP

> VMware NAT 虚拟机, 设置 IP 地址为 192.168.8.8

Windows 10 (主机): 设置 -> 网络和 Internet -> 状态 -> 更改适配器选项 -> 右键 VMnet8 -> 属性 -> Internet 协议版本 4 -> 属性

> 使用下面的 IP 地址 & DNS 服务器地址

|   NAME   |       IP        |      INFO      |
| :------: | :-------------: | :------------: |
| IP 地址  |   192.168.8.1   |    主机 IP     |
| 子网掩码 |  255.255.255.0  |    子网掩码    |
| 默认网关 |   192.168.8.2   |   虚拟机网关   |
| 首选 DNS |     8.8.8.8     | 虚拟机首选 DNS |
| 备用 DNS | 114.114.114.114 | 虚拟机备用 DNS |

> VMware

VMware Workstation: 编辑 -> 虚拟网络编辑器 -> VMnet8 -> 更改设置 -> VMnet8 -> 子网 IP: 192.168.8.0 -> 勾选使用本地 DHCP 服务将 IP 地址分配给虚拟机 -> DHCP 设置 -> 起始 IP 地址: 192.168.8.8 -> 确定

> 进入虚拟机 root

```bash
sudo vi /etc/network/interfaces

# ---/etc/network/interfaces--- #
[-] iface ens33 inet dhcp
[+] # iface ens33 inet dhcp
iface ens33 inet static
address 192.168.8.8
netmask 255.255.255.0
gateway 192.168.8.2
# ---/etc/network/interfaces--- #

sudo systemctl restart networking
```

改后文件

```interfaces
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
allow-hotplug ens33
#iface ens33 inet dhcp

iface ens33 inet static
address 192.168.8.8
netmask 255.255.255.0
gateway 192.168.8.2
```

## Windows 清除 ssh

删除文件 `C:\\Users\\username\\.ssh\\known_hosts`

## 允许 root 登录 ssh

```bash
sudo vi /etc/ssh/sshd_config

# ---/etc/ssh/sshd_config--- #
PermitRootLogin yes
# ---/etc/ssh/sshd_config--- #

sudo systemctl restart sshd
```

## Docker

```bash
sudo apt install docker.io
sudo apt install docker-compose

# 非 root 免 sudo 用 docker
sudo usermod -aG docker $USER
```

添加镜像:

```json
// sudo vi /etc/docker/daemon.json
{
  "registry-mirrors": [
    "https://docker.1panel.live/",
    "https://hub.rat.dev/",
    "https://hub.uuuadc.top/",
    "https://docker.anyhub.us.kg/",
    "https://docker.chenby.cn/",
    "https://dockerhub.jobcher.com/",
    "https://docker.ckyl.me/"
  ]
}
```

```bash
# 重启 docker
sudo systemctl restart docker

# docker 开机自启
sudo systemctl enable docker
```

## rust

```shell
# 安装工具
sudo apt install make build-essential curl

# 配置国内源
echo 'export RUSTUP_DIST_SERVER="https://rsproxy.cn"' >> ~/.bashrc
echo 'export RUSTUP_UPDATE_ROOT="https://rsproxy.cn/rustup"' >> ~/.bashrc
source ~/.bashrc

# 安装rust
# curl --proto '=https' --tlsv1.2 -sSf https://rsproxy.cn/rustup-init.sh | sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source "$HOME/.cargo/env"

# 查看版本
rustc --version
cargo --version

# 配置国内的crate源
cat > ~/.cargo/config << EOF
[source.crates-io]
replace-with = 'rsproxy-sparse'
[source.rsproxy]
registry = "https://rsproxy.cn/crates.io-index"
[source.rsproxy-sparse]
registry = "sparse+https://rsproxy.cn/index/"
[registries.rsproxy]
index = "https://rsproxy.cn/crates.io-index"
[net]
git-fetch-with-cli = true
EOF
```

## java

```shell
# 查看系统版本
$ cat /etc/os-release

# 安装 openjdk17
$ sudo apt install -y openjdk-17-jdk

# 安装 maven
$ sudo apt install maven
```
