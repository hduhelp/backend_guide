# BREW

## 简介

适用于个人Linux，不建议在服务端部署使用Brew

Brew 主要使用 类 git 的方式来进行软件的更新

## 安装

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Linux 还需要

```bash
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> ~/.profile
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
```

## 测试

```bash
brew install hello
```
