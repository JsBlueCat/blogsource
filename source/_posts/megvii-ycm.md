---
title: ycm hack
date: 2021-01-23 22:28:23
tags:
    - ycm
categories: 
    - megvii
---

# 安装ycm的时候出现 mrab-regex socks5 错误

原因是因为国内的网

修改办法就是

修改 .git/config/third_parity/ycm/config 里面 修改 github 的 地址
然后  git submodule --recursive sync 同步一下就解决了
最后 在跑 update 的时候 加入 --remote
```
git submodule update --remote --force third_party/mrab-regex/
```
