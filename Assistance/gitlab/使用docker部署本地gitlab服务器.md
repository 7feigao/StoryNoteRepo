---
title: 使用docker部署本地gitlab服务器
tags: gitlab,docker,server
grammar_cjkRuby: true
---

1. pull gitlab docker镜像到本地:https://hub.docker.com/r/gitlab/gitlab-ee。

```sh
docker pull gitlab/gitlab-ee
```

2. 运行镜像生成容器:https://docs.gitlab.com/omnibus/docker/。

```sh
docker run --detach --hostname gitlab.example.com --publish 443:443 --publish 80:80 --publish 22:22 --name gitlab --restart always gitlab/gitlab-ee
```

3. 输入运行docker服务器的网址查看到如下界面。

![enter description here](https://raw.githubusercontent.com/110011010/StoryNoteRepo/master/StoryNoteImg/1591198922836.png_3)

