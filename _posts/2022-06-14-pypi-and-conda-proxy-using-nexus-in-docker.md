---
title: 使用 nexus + docker 搭建 pypi 和 conda 镜像
layout: post
---

# 使用 nexus + docker 搭建 pypi 和 conda-forge 镜像


## 安装和配置 docker

略。用 docker desktop 挺方便的。用 wsl2 的 docker 好像更方便。

## 使用 docker 启动 nexus

在下面的例子中，将 nexus 安装到 `http://localhost:8082`。 为了省事就直接上 docker compose 了。

```yaml
version: "3.9"
services:
  nexus:
    restart: always
    image: sonatype/nexus3
    container_name: nexus
    ports:
        - 8082:8081
    volumes:
        - ./data:/nexus-data
    environment:
        TZ: Asia/Shanghai

```

使用 `docker compose up -d` 启动。

首次启动还有点慢，启动后会将初始密码写入 `admin.password` 文件。在浏览器中打开 http://localhost:8082 即可打开，按提示改密码即可。

## 配置 nexus

按如下信息建立两个 repository：

* `pypi`
  * name: `pypi`
  * Remote Storage: `https://pypi.org/` 这个官网说明里就有
* `conda-forge`
  * name: `conda-forge`
  * Remote Storage: `https://conda.anaconda.org/conda-forge/` 这个着实找了挺久。

配置完成后，在 nexus 中查看的两个 repo 的地址分别为:

* pypi : `http://localhost:8082/repository/pypi/`
* conda : `http://localhost:8082/repository/conda-forge/main`

## pip

注意此处要在 nexus 给出的地址后加 `/simple`。

```bash
$ python -m pip config --global set index-url http://localhost:8082/repository/pypi/simple
```

或在 `~/pip/pip.ini` 中:

```ini
[global]
index-url = http://localhost:8082/repository/pypi/simple
```

## conda-forge

注意此处并不需要在 nexus 给出的地址后加 `/main`

```bash
(base)$ conda config --add channels http://localhost:8082/repository/conda-forge
```

或在 `~/.condarc` 中:

```ini
channels:
  - http://localhost:8082/repository/conda-forge
  - defaults
show_channel_urls: true
```

## 手动下载依赖项

配好之后可以通过手动指定 nexus 地址的方式，让 nexus 去查询并进行缓存。例如:

```bash
$ pip install --index-url http://localhost:8082/repository/pypi/simple --default-timeout=255 scipy numpy
```

```bash
(base)$ conda create -n test2 --channel http://localhost:8082/repository/conda-forge python=3.8 build pytest scipy numpy
```