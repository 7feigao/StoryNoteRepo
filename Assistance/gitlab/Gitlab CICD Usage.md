---
title: Gitlab CICD Usage 
tags: gitlab,cicd
grammar_cjkRuby: true
---

### 使用docker配置gitlab runner。
1. 注册一个runner。

 ```sh
 docker run --rm -t -i -v /srv/gitlab-runner/config:/etc/gitlab-runner gitlab/gitlab-runner register
 ```
 2. 运行runner

```
docker run --rm -t -i -v /srv/gitlab-runner/config:/etc/gitlab-runner gitlab/gitlab-runner run
```
结果：
![enter description here](https://raw.githubusercontent.com/110011010/StoryNoteRepo/master/StoryNoteImg/1591428174705.png_6)
#### 问题
1. 在注册并运行runner后，发现执行sampleCI仍然失败：

![](./images/1591428256225.png)

解决办法： 打开windows host 文件， 写入一行新的记录。IP地址为gitlab服务的host地址。
```
192.168.51.241 gitlab.example.com
```

### Gitlab CICD的一些设置

#### Setting->CI/CD->Gerneral pipelines.
1. 配置pipeline获取代码的方式。·`git clone`或者`git fetch`
	* 区别：https://segmentfault.com/a/1190000017030384
1. `Git shallow clone`:配置shallow clone的最大深度。
2. Timeout: 配置pipeline的超时时间。
3. Custom CI configuration path：配置pipeline执行脚本在repo中的位置。
	4. pipeline 脚本可以位于另外一个repo中，这样可以增加访问控制权限。
4. Public pipelines：配置pipeline job信息的可见性。
5. Auto-cancel redundant, pending pipelines：新的pipeline会自动取消旧的还没有执行完的pipeline。


变量|GitLab|Runner|描述
---|---|---|---
CI_COMMIT_TAG|9.0|0.5|commit tag名，只有当按照tag构建的时候出现。

## [.gitlab-ci.yml](https://docs.gitlab.com/ee/ci/yaml/)
1. [`Jobs`](https://docs.gitlab.com/ee/ci/yaml/) 是`.gitlab-ci.yml`中最基本的元素。
	* `Jobs`中包含了在何种条件下执行的有限状态。
	* yml中的顶层元素，可以任意命名，并且必须包含`script`子元素。
	* Job定义的数量不受限。
最简单的yml文件,定义了两个job，并且会并行执行，互不干扰：
```yml
job1:
  script: "execute-script-for-job1"

job2:
  script: "execute-script-for-job2"
```

2. Job可以被任意命名，但是有些保留关键字不能被用作job名。
	| 关键字|
	|--|
	| image|
	| services|
	| stages|
	| types|
	| before_scipt|
	| after_script|
	| variables|
	| cache|
	| include|
	

### [Config 中的参数](https://docs.gitlab.com/ee/ci/yaml/#configuration-parameters)
| 参数| 描述|
|---|---|
| script| Runner要执行的shell script|
| image| 使用docker 镜像，也可以使用image:name and image:entrypoint.|
| services| 使用docker服务镜像，也可以使用：services:name, services:alias, services:entrypoint, and services:command
| before_script| 定义一组在执行job之前执行的一组命令，将会覆盖默认命令。|
| after_script| 定义一组在执行job之后执行的一组命令，将会覆盖默认的命令。|
| stage| 定义执行job的阶段，默认阶段为`test`|
| only| 定义在何种情况下创建并执行job|
| excep| 定义在何种情况下不创建执行job|


#### `stage`定义在每一个job中，并且依赖于全局定义的`stages`,  使用`stage`可以对不同的job