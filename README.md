# rclone

[![Release](https://img.shields.io/github/v/release/gaoyb7/rclone-release?display_name=tag)](https://github.com/gaoyb7/rclone-release/releases)
[![Downloads](https://img.shields.io/github/downloads/gaoyb7/rclone-release/total)](https://github.com/gaoyb7/rclone-release/releases)

rclone 改版，对比 115drive-webdav 功能更强大，支持 WebDav 服务，本地磁盘挂载，文件批量下载到本地等功能

## 配置生成
```
# 根据提示生成对应的 115 配置，生成配置后，可进行 rclone WebDav 服务启动，磁盘挂载等操作
# 网上教程很多自行查阅
./rclone config
```

## WebDav 服务启动
```
./rclone serve webdav --addr :8081  -v 115drive:
```

## 本地磁盘挂载
```
./rclone mount -v \
        --allow-other \
        --read-only \
        --vfs-cache-mode=full \
        --vfs-cache-max-size=4G \
        --vfs-read-chunk-size=8M \
        --cache-dir=/data/.cache/rclone \
        --buffer-size=32M \
        115drive: /path/to/local
```

## 文件批量下载
```
./rclone copy -P --multi-thread-streams=2 --transfers=5 115drive:/path/to/remote ./path/to/local
```

## 其他功能
参考 https://rclone.org/docs/

## Docker 运行
TODO
