# biggates.github.io
personal blog

## 安装 Jekyll

基本上就是在 wsl 里面装 gem 和依赖项。

首先安装 ruby，在此由于 ppa 的源连不上，改用 Ubuntu 自己的源。按此操作时安装的是 ruby 2.7

```bash
sudo apt-get update -y && sudo apt-get upgrade -y
sudo apt-get install ruby ruby-dev build-essential dh-autoreconf libffi-dev -y
```

设置 gem 改用 ruby-china 的 rubygems 源。

```bash
sudo gem sources --remove https://rubygems.org/
sudo gem sources --add https://gems.ruby-china.com/
sudo gem update
sudo gem install bundler

bundle install
```

see https://jekyllrb.com/docs/installation/windows/

## 使用 Jekyll

使用 `bundle exec jekyll serve` 开启本地服务（默认值是 `localhost:4000` ）

## 升级依赖项

```bash
bundle update
```
