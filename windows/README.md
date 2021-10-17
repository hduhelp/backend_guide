# Windows 环境配置

## WSL 简介

<https://docs.microsoft.com/zh-cn/windows/wsl/>

## 安装 WSL

在 Windows（10 及以上）的操作系统中[安装WSL](https://docs.microsoft.com/zh-cn/windows/wsl/install)（建议使用 [WSL2](https://docs.microsoft.com/zh-cn/windows/wsl/compare-versions#whats-new-in-wsl-2) ，本文档内容也以 WSL2 为基础）

推荐使用 Ubuntu 20.04 LTS，Microstoft Store [安装链接](https://www.microsoft.com/en-us/p/ubuntu-2004-lts/9n6svws3rx71)

使用WSL后系统环境将变得简单，其他相关的环境配置可以参考本导引[Unix内容](./../unix/)

### WSL 不继承 Windows 环境变量

避免出现一些不可控情况

```bash
sudo nano /etc/wsl.conf #编辑文件`/etc/wsl.conf`
```

加入以下内容

```toml
[interop]
appendWindowsPath = false
```

在PowerShell中重启 WSL

```CMD
WSL --shutdown
```

### WSL 内部 Proxy 环境脚本

将下面函数加入到`~/.profile`中，记得编辑端口

```bash
export hostip=$(cat /etc/resolv.conf |grep -oP '(?<=nameserver\ ).*') #获得网关地址
export proxyPort=7890 #端口
alias proxy='
    export https_proxy="http://${hostip}:${proxyPort}";
    export http_proxy="http://${hostip}:${proxyPort}";
    export all_proxy="http://${hostip}:${proxyPort}";
    echo -e "Acquire::http::Proxy \"http://${hostip}:${proxyPort}\";" | sudo tee -a /etc/apt/apt.conf.d/proxy.conf > /dev/null;
    echo -e "Acquire::https::Proxy \"http://${hostip}:${proxyPort}\";" | sudo tee -a /etc/apt/apt.conf.d/proxy.conf > /dev/null;
'
alias unproxy='
    unset https_proxy;
    unset http_proxy;
    unset all_proxy;
    sudo sed -i -e "/Acquire::http::Proxy/d" /etc/apt/apt.conf.d/proxy.conf;
    sudo sed -i -e "/Acquire::https::Proxy/d" /etc/apt/apt.conf.d/proxy.conf;
'
```

测试可用性

```bash
source ~/.profile #加载环境
proxy #执行函数 代理生效
curl https://www.google.com #请求Google 网址
```

## [安装 Golang 语言环境](./../jetBrains/)

## Visual Studio Code

<https://code.visualstudio.com>
