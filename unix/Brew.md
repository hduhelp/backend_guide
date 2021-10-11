# BREW 

## 简介

适用于个人Linux，不建议在服务端部署使用Brew

## 安装

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Linux 还需要

```bash
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
```

## 测试

```bash
brew install hello
```