
## 📁 文件和目录操作

### 1. 目录操作

```bash
# 查看当前目录
pwd

# 列出文件和目录
ls                    # 基本列表
ls -l                 # 详细信息
ls -a                 # 显示隐藏文件
ls -lh                # 人性化显示大小
ls -lt                # 按时间排序
ls -lS                # 按大小排序
ls -R                 # 递归显示

# 切换目录
cd /path/to/dir       # 切换到指定目录
cd ~                  # 切换到家目录
cd -                  # 切换到上一个目录
cd ..                 # 切换到上级目录
cd ../..              # 切换到上两级目录

# 创建目录
mkdir dirname         # 创建单个目录
mkdir -p a/b/c        # 创建多级目录
mkdir -m 755 dirname  # 创建并设置权限

# 删除目录
rmdir dirname         # 删除空目录
rm -r dirname         # 递归删除目录
rm -rf dirname        # 强制递归删除（危险！）
```

---

### 2. 文件操作

```bash
# 创建文件
touch file.txt        # 创建空文件或更新时间戳
echo "content" > file.txt    # 创建并写入内容
cat > file.txt        # 创建并输入内容（Ctrl+D 结束）

# 复制文件
cp file1 file2        # 复制文件
cp -r dir1 dir2       # 复制目录
cp -p file1 file2     # 保留属性复制
cp -i file1 file2     # 交互式复制（覆盖前询问）

# 移动/重命名
mv file1 file2        # 重命名文件
mv file1 /path/       # 移动文件
mv -i file1 file2     # 交互式移动

# 删除文件
rm file.txt           # 删除文件
rm -f file.txt        # 强制删除
rm -i file.txt        # 交互式删除
rm *.txt              # 删除所有 .txt 文件

# 查找文件
find /path -name "*.txt"              # 按名称查找
find /path -type f                    # 查找文件
find /path -type d                    # 查找目录
find /path -size +100M                # 查找大于 100M 的文件
find /path -mtime -7                  # 查找 7 天内修改的文件
find /path -name "*.log" -delete      # 查找并删除

# 定位文件
locate filename       # 快速查找文件（需要 updatedb）
which command         # 查找命令位置
whereis command       # 查找命令、源码、手册位置
```

---

## 📄 文件查看和编辑

### 1. 查看文件内容

```bash
# 查看全部内容
cat file.txt          # 显示全部内容
cat -n file.txt       # 显示行号

# 分页查看
more file.txt         # 向下分页查看
less file.txt         # 可上下翻页查看（推荐）

# 查看部分内容
head file.txt         # 查看前 10 行
head -n 20 file.txt   # 查看前 20 行
tail file.txt         # 查看后 10 行
tail -n 20 file.txt   # 查看后 20 行
tail -f file.txt      # 实时查看文件更新（常用于日志）

# 查看文件信息
file file.txt         # 查看文件类型
stat file.txt         # 查看文件详细信息
wc file.txt           # 统计行数、字数、字节数
wc -l file.txt        # 只统计行数
```

---

### 2. 文本编辑器

```bash
# vim 编辑器（推荐）
vim file.txt          # 打开文件
# i - 进入插入模式
# Esc - 退出插入模式
# :w - 保存
# :q - 退出
# :wq - 保存并退出
# :q! - 强制退出不保存
# dd - 删除当前行
# yy - 复制当前行
# p - 粘贴
# /keyword - 搜索

# nano 编辑器（简单）
nano file.txt         # 打开文件
# Ctrl+O - 保存
# Ctrl+X - 退出
# Ctrl+K - 剪切行
# Ctrl+U - 粘贴

# sed 流编辑器
sed 's/old/new/g' file.txt           # 替换文本
sed -i 's/old/new/g' file.txt        # 直接修改文件
sed -n '10,20p' file.txt             # 显示 10-20 行
```

---

## 🔍 文本处理

```bash
# grep - 搜索文本
grep "keyword" file.txt              # 搜索关键字
grep -i "keyword" file.txt           # 忽略大小写
grep -r "keyword" /path              # 递归搜索目录
grep -n "keyword" file.txt           # 显示行号
grep -v "keyword" file.txt           # 反向匹配（不包含）
grep -E "pattern1|pattern2" file.txt # 多个模式

# awk - 文本处理
awk '{print $1}' file.txt            # 打印第一列
awk -F: '{print $1}' /etc/passwd     # 指定分隔符
awk '$3 > 100' file.txt              # 条件过滤

# sort - 排序
sort file.txt                        # 排序
sort -r file.txt                     # 反向排序
sort -n file.txt                     # 数字排序
sort -u file.txt                     # 去重排序

# uniq - 去重
uniq file.txt                        # 去除相邻重复行
sort file.txt | uniq                 # 完全去重
uniq -c file.txt                     # 统计重复次数

# cut - 剪切
cut -d: -f1 /etc/passwd              # 按分隔符剪切
cut -c1-10 file.txt                  # 剪切字符

# tr - 转换
tr 'a-z' 'A-Z' < file.txt            # 小写转大写
tr -d '\r' < file.txt                # 删除字符
```

---

## 🔐 权限管理

```bash
# 查看权限
ls -l file.txt        # 查看文件权限

# 修改权限
chmod 755 file.txt    # 数字方式
chmod u+x file.txt    # 符号方式（用户添加执行权限）
chmod g-w file.txt    # 组去除写权限
chmod o+r file.txt    # 其他用户添加读权限
chmod -R 755 dir/     # 递归修改目录权限

# 修改所有者
chown user file.txt           # 修改所有者
chown user:group file.txt     # 修改所有者和组
chown -R user:group dir/      # 递归修改

# 修改组
chgrp group file.txt          # 修改组
chgrp -R group dir/           # 递归修改
```

**权限说明：**
- r (read) = 4
- w (write) = 2
- x (execute) = 1
- 755 = rwxr-xr-x (所有者全部权限，组和其他只读执行)
- 644 = rw-r--r-- (所有者读写，组和其他只读)

---

## 💾 压缩和解压

```bash
# tar 打包
tar -cvf archive.tar files/          # 打包
tar -czvf archive.tar.gz files/      # 打包并 gzip 压缩
tar -cjvf archive.tar.bz2 files/     # 打包并 bzip2 压缩

# tar 解包
tar -xvf archive.tar                 # 解包
tar -xzvf archive.tar.gz             # 解压 gzip
tar -xjvf archive.tar.bz2            # 解压 bzip2
tar -xvf archive.tar -C /path        # 解压到指定目录

# 查看压缩包内容
tar -tvf archive.tar                 # 查看内容

# zip/unzip
zip archive.zip files                # 压缩
zip -r archive.zip dir/              # 递归压缩目录
unzip archive.zip                    # 解压
unzip archive.zip -d /path           # 解压到指定目录
unzip -l archive.zip                 # 查看内容

# gzip/gunzip
gzip file.txt                        # 压缩（会删除原文件）
gzip -k file.txt                     # 压缩保留原文件
gunzip file.txt.gz                   # 解压
```

---

## 🖥️ 系统信息

```bash
# 系统信息
uname -a              # 系统信息
uname -r              # 内核版本
hostname              # 主机名
uptime                # 运行时间和负载
date                  # 当前日期时间
cal                   # 日历
whoami                # 当前用户
id                    # 用户 ID 信息

# 硬件信息
lscpu                 # CPU 信息
free -h               # 内存使用情况
df -h                 # 磁盘使用情况
du -sh /path          # 目录大小
du -h --max-depth=1   # 查看子目录大小
lsblk                 # 块设备信息
fdisk -l              # 磁盘分区信息

# 系统资源
top                   # 实时进程监控
htop                  # 更友好的进程监控（需安装）
ps aux                # 查看所有进程
ps -ef                # 查看所有进程（另一种格式）
vmstat                # 虚拟内存统计
iostat                # IO 统计
```

---

## 🔄 进程管理

```bash
# 查看进程
ps aux                        # 查看所有进程
ps aux | grep java            # 查找特定进程
pgrep java                    # 查找进程 ID
pidof java                    # 查找进程 ID

# 杀死进程
kill PID                      # 终止进程
kill -9 PID                   # 强制杀死进程
killall process_name          # 杀死所有同名进程
pkill -f pattern              # 按模式杀死进程

# 后台运行
command &                     # 后台运行
nohup command &               # 后台运行（不挂断）
nohup command > log.txt 2>&1 &  # 后台运行并记录日志

# 任务控制
jobs                          # 查看后台任务
fg %1                         # 将任务调到前台
bg %1                         # 将任务放到后台
Ctrl+Z                        # 暂停当前任务
Ctrl+C                        # 终止当前任务

# 进程优先级
nice -n 10 command            # 以指定优先级运行
renice -n 5 -p PID            # 修改进程优先级
```

---

## 🌐 网络操作

```bash
# 网络配置
ifconfig                      # 查看网络接口（旧）
ip addr                       # 查看 IP 地址（新）
ip link                       # 查看网络接口
ip route                      # 查看路由表

# 网络连接
ping host                     # 测试连通性
ping -c 4 host                # ping 4 次
traceroute host               # 追踪路由
netstat -tulnp                # 查看端口监听
ss -tulnp                     # 查看端口监听（新）
lsof -i :8080                 # 查看端口占用

# 下载文件
wget url                      # 下载文件
wget -c url                   # 断点续传
curl url                      # 获取内容
curl -O url                   # 下载文件
curl -I url                   # 查看 HTTP 头

# 远程连接
ssh user@host                 # SSH 连接
ssh -p 2222 user@host         # 指定端口
scp file user@host:/path      # 复制文件到远程
scp user@host:/path/file .    # 从远程复制文件
```

---

## 👤 用户管理

```bash
# 用户操作
useradd username              # 创建用户
useradd -m -s /bin/bash user  # 创建用户并指定 shell
passwd username               # 设置密码
userdel username              # 删除用户
userdel -r username           # 删除用户及家目录
usermod -aG group user        # 添加用户到组

# 组操作
groupadd groupname            # 创建组
groupdel groupname            # 删除组
groups username               # 查看用户所属组

# 切换用户
su - username                 # 切换用户
sudo command                  # 以 root 权限执行
sudo -i                       # 切换到 root
exit                          # 退出当前用户
```

---

## 📦 软件包管理

### CentOS/RHEL (yum/dnf)

```bash
# 查询
yum search package            # 搜索软件包
yum info package              # 查看包信息
yum list installed            # 列出已安装的包

# 安装/卸载
yum install package           # 安装软件包
yum install -y package        # 自动确认安装
yum remove package            # 卸载软件包
yum update                    # 更新所有软件包
yum update package            # 更新指定软件包

# 清理
yum clean all                 # 清理缓存
```

---

### Ubuntu/Debian (apt)

```bash
# 更新
apt update                    # 更新软件包列表
apt upgrade                   # 升级所有软件包

# 查询
apt search package            # 搜索软件包
apt show package              # 查看包信息
apt list --installed          # 列出已安装的包

# 安装/卸载
apt install package           # 安装软件包
apt install -y package        # 自动确认安装
apt remove package            # 卸载软件包
apt purge package             # 卸载并删除配置
apt autoremove                # 删除不需要的依赖

# 清理
apt clean                     # 清理缓存
```

---

## 🔧 系统服务管理

```bash
# systemctl（systemd 系统）
systemctl start service       # 启动服务
systemctl stop service        # 停止服务
systemctl restart service     # 重启服务
systemctl reload service      # 重新加载配置
systemctl status service      # 查看服务状态
systemctl enable service      # 设置开机自启
systemctl disable service     # 取消开机自启
systemctl list-units          # 列出所有服务

# service（旧系统）
service service_name start    # 启动服务
service service_name stop     # 停止服务
service service_name restart  # 重启服务
service service_name status   # 查看状态
```

---

## 📊 磁盘管理

```bash
# 查看磁盘
df -h                         # 查看磁盘使用情况
df -i                         # 查看 inode 使用情况
du -sh /path                  # 查看目录大小
du -h --max-depth=1 /path     # 查看子目录大小

# 挂载
mount /dev/sdb1 /mnt          # 挂载分区
umount /mnt                   # 卸载
mount -a                      # 挂载 /etc/fstab 中的所有

# 磁盘分区
fdisk -l                      # 查看分区
fdisk /dev/sdb                # 分区工具
parted /dev/sdb               # 高级分区工具

# 格式化
mkfs.ext4 /dev/sdb1           # 格式化为 ext4
mkfs.xfs /dev/sdb1            # 格式化为 xfs
```

---

## 🔒 防火墙

```bash
# firewalld（CentOS 7+）
firewall-cmd --state                    # 查看状态
firewall-cmd --list-all                 # 查看规则
firewall-cmd --add-port=8080/tcp        # 开放端口（临时）
firewall-cmd --add-port=8080/tcp --permanent  # 永久开放
firewall-cmd --reload                   # 重新加载
firewall-cmd --remove-port=8080/tcp --permanent  # 关闭端口

# iptables（旧系统）
iptables -L                             # 查看规则
iptables -A INPUT -p tcp --dport 8080 -j ACCEPT  # 开放端口
service iptables save                   # 保存规则
```

---

## 📝 日志查看

```bash
# 系统日志
tail -f /var/log/messages     # 实时查看系统日志（CentOS）
tail -f /var/log/syslog       # 实时查看系统日志（Ubuntu）
journalctl -f                 # 实时查看 systemd 日志
journalctl -u service_name    # 查看特定服务日志
journalctl --since "1 hour ago"  # 查看最近 1 小时日志

# 应用日志
tail -f /var/log/nginx/access.log     # Nginx 访问日志
tail -f /var/log/nginx/error.log      # Nginx 错误日志
tail -f /var/log/mysql/error.log      # MySQL 错误日志
```

---

## 🔄 定时任务

```bash
# crontab
crontab -e                    # 编辑定时任务
crontab -l                    # 查看定时任务
crontab -r                    # 删除所有定时任务

# crontab 格式
# 分 时 日 月 周 命令
# * * * * * command
# 0 2 * * * /path/script.sh   # 每天凌晨 2 点执行

# at（一次性任务）
at 10:00                      # 在 10:00 执行
at now + 1 hour               # 1 小时后执行
atq                           # 查看待执行任务
atrm job_id                   # 删除任务
```

---

## 🔗 快捷键

```bash
# 终端快捷键
Ctrl + C                      # 终止当前命令
Ctrl + Z                      # 暂停当前命令
Ctrl + D                      # 退出当前 shell
Ctrl + L                      # 清屏
Ctrl + A                      # 光标移到行首
Ctrl + E                      # 光标移到行尾
Ctrl + U                      # 删除光标前的内容
Ctrl + K                      # 删除光标后的内容
Ctrl + W                      # 删除光标前的单词
Ctrl + R                      # 搜索历史命令
Tab                           # 自动补全
!!                            # 执行上一条命令
!$                            # 上一条命令的最后一个参数
```

---

## 💡 实用技巧

```bash
# 命令历史
history                       # 查看命令历史
history | grep keyword        # 搜索历史命令
!100                          # 执行第 100 条历史命令
!!                            # 执行上一条命令

# 管道和重定向
command1 | command2           # 管道
command > file                # 输出重定向（覆盖）
command >> file               # 输出重定向（追加）
command 2> file               # 错误重定向
command > file 2>&1           # 标准输出和错误都重定向
command < file                # 输入重定向

# 命令组合
command1 && command2          # command1 成功后执行 command2
command1 || command2          # command1 失败后执行 command2
command1 ; command2           # 顺序执行

# 别名
alias ll='ls -lh'             # 创建别名
alias                         # 查看所有别名
unalias ll                    # 删除别名

# 环境变量
echo $PATH                    # 查看 PATH
export VAR=value              # 设置环境变量
env                           # 查看所有环境变量
```

---

## 📚 帮助命令

```bash
man command                   # 查看命令手册
command --help                # 查看命令帮助
info command                  # 查看详细信息
whatis command                # 查看命令简介
apropos keyword               # 搜索相关命令
```

---

## 🎯 常用命令速查表

| 类别 | 命令 | 说明 |
|------|------|------|
| 文件 | `ls`, `cd`, `pwd`, `cp`, `mv`, `rm` | 基本操作 |
| 查看 | `cat`, `less`, `head`, `tail` | 查看文件 |
| 搜索 | `find`, `grep`, `locate` | 查找文件/内容 |
| 权限 | `chmod`, `chown`, `chgrp` | 权限管理 |
| 压缩 | `tar`, `zip`, `gzip` | 压缩解压 |
| 系统 | `top`, `ps`, `kill`, `df`, `free` | 系统监控 |
| 网络 | `ping`, `netstat`, `ssh`, `wget` | 网络操作 |
| 用户 | `useradd`, `passwd`, `su`, `sudo` | 用户管理 |
