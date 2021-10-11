# Golang 安装

## 使用`Brew`

```bash
brew install golang
```

## 从二进制文件安装

```bash
cd ~
wget https://mirrors.ustc.edu.cn/golang/go1.17.2.linux-amd64.tar.gz
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf go1.17.2.linux-amd64.tar.gz
echo "export PATH=\$PATH:/usr/local/go/bin" >> ~/.profile
echo "export PATH=\$PATH:\$HOME/go/bin" >> ~/.profile
source ~/.profile
```

## 使用`goproxy`代理依赖

```bash
go env -w GOPROXY=https://goproxy.cn,direct
```
