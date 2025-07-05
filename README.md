# gitlab-mac
## 使用 Docker 容器安装（推荐）
此方法简单高效，适合本地开发和测试。

步骤：
安装 Docker Desktop

下载地址：https://www.docker.com/products/docker-desktop

安装后启动 Docker，确保状态栏显示 Docker 图标。

拉取 GitLab 镜像
打开终端执行：
```bash
docker pull gitlab/gitlab-ce:latest
```
运行 GitLab 容器

```bash
docker run --detach \
  --hostname gitlab.example.com \
  --publish 443:443 --publish 80:80 --publish 22:22 \
  --name gitlab \
  --restart always \
  --volume $HOME/gitlab/config:/etc/gitlab \
  --volume $HOME/gitlab/logs:/var/log/gitlab \
  --volume $HOME/gitlab/data:/var/opt/gitlab \
  gitlab/gitlab-ce:latest
```
参数说明：

--publish: 绑定端口（HTTP:80, HTTPS:443, SSH:22）

--volume: 持久化配置、日志和数据（将 $HOME/gitlab 替换为你的本地路径）

等待初始化完成

首次启动需 2-5 分钟，可通过日志监控进度：

```bash
docker logs -f gitlab
```
当看到 GitLab is ready! 表示启动成功。

访问 GitLab

浏览器打开：http://localhost

首次登录需设置 root 密码（长度至少 8 位）。
自动生成的随机密码（仅限某些安装方式）
如果您使用 Docker 安装且没有手动设置，密码可能存储在配置文件中：

```bash
# 进入 Docker 容器
docker exec -it gitlab bash

# 查看初始随机密码（如果存在）
cat /etc/gitlab/initial_root_password
```
