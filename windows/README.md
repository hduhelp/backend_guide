# Windows 环境配置

## WSL 简介 

https://docs.microsoft.com/zh-cn/windows/wsl/

## 安装 WSL

在 Windows（10 及以上）的操作系统中安装WSL

### WSL 不继承 Windows 环境变量

避免出现一些不可控情况

编辑文件`/etc/wsl.conf`，加入以下内容

```bash
sudo nano /etc/wsl.conf
```

```toml
[interop]
appendWindowsPath = false
```

在PowerShell中重启 WSL

```CMD
WSL --shutdown
```

### WSL 内部 Proxy 环境脚本

将下面函数加入到`~/.bashrc`中，记得编辑端口配置

```bash
export hostip=$(cat /etc/resolv.conf |grep -oP '(?<=nameserver\ ).*')
alias proxy='
    export https_proxy="http://${hostip}:7890";
    export http_proxy="http://${hostip}:7890";
    export all_proxy="http://${hostip}:7890";
    echo -e "Acquire::http::Proxy \"http://${hostip}:7890\";" | sudo tee -a /etc/apt/apt.conf.d/proxy.conf > /dev/null;
    echo -e "Acquire::https::Proxy \"http://${hostip}:7890\";" | sudo tee -a /etc/apt/apt.conf.d/proxy.conf > /dev/null;
'
alias unproxy='
    unset https_proxy;
    unset http_proxy;
    unset all_proxy;
    sudo sed -i -e '/Acquire::http::Proxy/d' /etc/apt/apt.conf.d/proxy.conf;
    sudo sed -i -e '/Acquire::https::Proxy/d' /etc/apt/apt.conf.d/proxy.conf;
'
```

测试可用性

```bash
source ~/.bashrc
proxy
curl https://www.google.com
```

## 安装 Golang 语言环境

### JetBrains Toolbox

https://www.jetbrains.com/toolbox-app/

#### 安装 Goland IDE