# rclone

[![Release](https://img.shields.io/github/v/release/gaoyb7/rclone-release?display_name=tag)](https://github.com/gaoyb7/rclone-release/releases)
[![Downloads](https://img.shields.io/github/downloads/gaoyb7/rclone-release/total)](https://github.com/gaoyb7/rclone-release/releases)

rclone 改版，兼容支持 115 网盘，对比 115drive-webdav 功能更强大，支持 WebDav 服务，本地磁盘挂载，文件批量下载到本地等功能。

代码目录：https://github.com/gaoyb7/rclone/tree/feat-115-drive

## 下载

https://github.com/gaoyb7/rclone-release/releases

* 不支持文件上传功能

## 配置生成
### 方式一：通过 rclone config 生成本地配置
```
# 根据提示生成对应的 115 配置，生成配置后，可进行 rclone WebDav 服务启动，磁盘挂载等操作
# 网上教程很多自行查阅
./rclone config

# 下面的命令均假设生成的配置名为 115drive，根据实际情况修改
```

### 方式二：通过环境变量获取配置
参考：https://rclone.org/docs/#config-file

rclone 支持从环境变量生成并读取配置，具体环境变量的格式为 `RCLONE_CONFIG_{{remote}}_{{param}}`，其中 `{{remote}}` 为配置名，`{{param}}` 为对应的参数名，均为大写。如配置名为 115drive，参数 uid，对应的环境变量为 `RCLONE_CONFIG_115DRIVE_UID`。其中 `RCLONE_CONFIG_{{remote}}_TYPE` 用于指定网盘类型，115 网盘对应为 115

## WebDav 服务启动
参考：https://rclone.org/commands/rclone_serve_webdav/
```
# 示例：将网盘挂载为本地 WebDav 服务，端口号 8081，支持局域网访问
# 命令行方式运行
./rclone serve webdav --addr :8081  -v 115drive:

# Docker 方式运行，从环境变量读取配置
docker run -d \
    -p 8081:8081 \
    -e RCLONE_VERBOSE=1 \
    -e RCLONE_ADDR=0.0.0.0:8081 \
    -e RCLONE_CONFIG_115DRIVE_TYPE=115 \
    -e RCLONE_CONFIG_115DRIVE_UID=<your uid> \
    -e RCLONE_CONFIG_115DRIVE_CID=<your cid> \
    -e RCLONE_CONFIG_115DRIVE_SEID=<your seid> \
    --restart unless-stopped \
    gaoyb7/rclone:latest serve webdav 115drive:
```

* Docker 方式运行无需 rclone config 生成配置
* `RCLONE_CONFIG_115DRIVE_UID`、`RCLONE_CONFIG_115DRIVE_CID`、`RCLONE_CONFIG_115DRIVE_SEID` 参数替换成对应的 Cookie UID、CID、SEID

## 本地磁盘挂载
参考：https://rclone.org/commands/rclone_mount/
```
# 示例：将网盘挂载到本地磁盘 /mnt/115drive 目录
# 命令行方式运行
./rclone mount -v \
        --allow-other \
        --read-only \
        --vfs-cache-mode=full \
        --vfs-cache-max-size=4G \
        --vfs-read-chunk-size=8M \
        --buffer-size=32M \
        115drive: /mnt/115drive

# 命令行方式运行，Windows 系统，挂载为 X: 盘
.\rclone.exe mount -v --read-only --vfs-cache-mode=full --vfs-cache-max-size=4G --vfs-read-chunk-size=8M --buffer-size=32M --network-mode 115drive: X:

# Docker 方式运行，从环境变量读取配置
docker run -d \
    -e RCLONE_VERBOSE=1 \
    -e RCLONE_CONFIG_115DRIVE_TYPE=115 \
    -e RCLONE_CONFIG_115DRIVE_UID=<your uid> \
    -e RCLONE_CONFIG_115DRIVE_CID=<your cid> \
    -e RCLONE_CONFIG_115DRIVE_SEID=<your seid> \
    --volume /mnt:/mnt:shared \
    --device /dev/fuse \
    --cap-add SYS_ADMIN \
    --security-opt apparmor:unconfined \
    --restart unless-stopped \
    gaoyb7/rclone:latest mount \
    --allow-other \
    --allow-non-empty \
    --read-only \
    --vfs-cache-mode=full \
    --vfs-cache-max-size=4G \
    --vfs-read-chunk-size=8M \
    --buffer-size=32M \
    115drive: /mnt/115drive
```

* Linux 版本需要安装 fuse 依赖
  * Debian 系如 Ubuntu: `apt-get install -y fuse3`
  * RedHat 系如 CentOS: `yum install -y fuse3`
* Windows 版本需要安装 WinFsp 依赖，https://winfsp.dev/
* macOSX 版本需要安装 macFUSE 依赖，https://osxfuse.github.io/

## 文件批量下载
参考：https://rclone.org/commands/rclone_copy/
```
# 示例：将网盘的 data/movies 文件夹下载到本地 /data/downloads/movies
./rclone copy -P --multi-thread-streams=2 --transfers=5 115drive:/data/movies /data/downloads/movies
```

* 115 网盘最多支持 5 个文件同时下载，对应的参数为 `--transfers=5`
* 115 网盘单文件最多支持 2 线程下载，对应的参数为 `--multi-thread-streams=2`

## 其他功能
参考 https://rclone.org/docs/

