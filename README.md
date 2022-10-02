# rclone

[![Release](https://img.shields.io/github/v/release/gaoyb7/rclone-release?display_name=tag)](https://github.com/gaoyb7/rclone-release/releases)
[![Downloads](https://img.shields.io/github/downloads/gaoyb7/rclone-release/total)](https://github.com/gaoyb7/rclone-release/releases)

rclone 改版，兼容支持 115 网盘，对比 115drive-webdav 功能更强大，支持 WebDav 服务，本地磁盘挂载，文件批量下载到本地等功能。

代码目录：https://github.com/gaoyb7/rclone/tree/feat-115-drive

## 下载

下载地址：https://github.com/gaoyb7/rclone-release/releases

## 配置生成
```
# 根据提示生成对应的 115 配置，生成配置后，可进行 rclone WebDav 服务启动，磁盘挂载等操作
# 网上教程很多自行查阅
./rclone config

# 下面的命令均假设生成的配置名为 115drive，根据实际情况修改
```

## WebDav 服务启动
参考：https://rclone.org/commands/rclone_serve_webdav/
```
# 示例：将网盘挂载为本地 WebDav 服务，端口号 8081，支持局域网访问
./rclone serve webdav --addr :8081  -v 115drive:
```

## 本地磁盘挂载
参考：https://rclone.org/commands/rclone_mount/
```
# 示例：将网盘挂载到本地磁盘 /mnt/115drive 目录
./rclone mount -v \
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
* Windows、macOS 暂不支持挂载

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

## Docker 运行
TODO
