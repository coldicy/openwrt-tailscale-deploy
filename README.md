# openwrt-tailscale-deploy
openwrt系统安装tailscal

#tailscale #路由器 #内网穿透

---

## 概述

家庭网络拓扑为主路由（openwrt）+旁路由（istoreOS）模式，局域网内 DHCP 服务器由旁路由充当。希望通过在旁路由上安装 tailscale 实现外部访问整个家庭网络内的设备

## 步骤

安装过程参考[OpenWrt 安装配置 tailscale](https://pfchina.org/?p=9151](https://pfchina.org/?p=9151)

### 创建目录

```shell
mkdir -p /opt/software/tailscale
cd /opt/software/tailscale
```

### 下载软件

```shell
wget https://github.com/adyanth/openwrt-tailscale-enabler/releases/download/v1.60.0-e428948-autoupdate/openwrt-tailscale-enabler-v1.60.0-e428948-autoupdate.tgz
```

### 解压

```shell
tar x -zvC / -f openwrt-tailscale-enabler-v1.60.0-e428948-autoupdate.tgz
```

### 安装依赖

```shell
opkg update opkg
opkg install libustream-openssl ca-bundle kmod-tun
```

### 设置开机自启动

```shell
/etc/init.d/tailscale enable
```

### 检查开机启动项

```shell
ls /etc/rc.d/S*tailscale*
```

### 启动 tailscale

```shell
/etc/init.d/tailscale start
```

### 查看版本

```shell
tailscale version
```

### 后续升级

```shell
tailscale update
```

### 启用 tailscale

```shell
tailscale up --advertise-routes=192.168.124.0/24 --accept-dns=false --advertise-exit-node  --accept-routes=true --reset
```

## 其他

后续需要多局域网组网可参考：[OpenWrt 安装 Tailscale 设置内网穿透+科学出国+外网互访局域网设备](https://wp.gxnas.com/14248.html)
自行安装 derp 中转服务器参考：[大内网战略(6)：自建 Tailscale DERP 中继服务器 保姆级教程](https://zhuanlan.zhihu.com/p/638910565) 后续证书过期重新申请证书上传到原先位置并重启服务即可。
```shell
systemctl stop derper
systemctl start derper
```

