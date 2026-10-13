目录
系统基础
文件与目录管理
用户与权限管理
进程管理
systemd 服务管理
磁盘与存储管理
网络管理
软件包管理
Shell 与 Bash
文本处理三剑客
计划任务 Cron
日志管理
SSH 远程管理
性能监控与调优
安全加固
故障排查
命令速查表
---
1. 系统基础
1.1 系统信息查看
# 系统版本
cat /etc/os-release
cat /etc/redhat-release        # CentOS/RHEL
lsb_release -a                 # Ubuntu/Debian
uname -a                       # 内核信息
uname -r                       # 内核版本
hostnamectl                    # 主机名和系统信息

# 硬件信息
lscpu                          # CPU 信息
cat /proc/cpuinfo              # CPU 详细信息
free -h                        # 内存信息
dmidecode -t memory            # 内存硬件信息
lsblk                          # 块设备（磁盘）
fdisk -l                       # 磁盘分区
lspci                          # PCI 设备
lsusb                          # USB 设备
lshw                           # 全部硬件信息

# 系统运行信息
uptime                         # 运行时间和负载
date                           # 当前时间
timedatectl                    # 时区信息
whoami                         # 当前用户
id                             # 用户和组信息
w                              # 登录用户和活动
last                           # 登录历史
1.2 主机名与域名
# 查看主机名
hostname
hostnamectl

# 设置主机名
hostnamectl set-hostname myserver

# 临时修改
hostname newname

# /etc/hosts 文件
cat /etc/hosts
# 127.0.0.1   localhost
# 192.168.1.100 myserver myserver.local

# DNS 配置
cat /etc/resolv.conf
# nameserver 8.8.8.8
# nameserver 8.8.4.4
1.3 时间与时区
# 查看当前时间
date
date '+%Y-%m-%d %H:%M:%S'

# 查看时区
timedatectl

# 设置时区
timedatectl set-timezone Asia/Shanghai

# 同步时间（NTP）
timedatectl set-ntp true
chronyc tracking              # chrony 时间同步状态
systemctl status chronyd      # chrony 服务状态

# 手动设置时间
date -s "2024-01-15 10:30:00"
hwclock --systohc             # 同步到硬件时钟
1.4 关机与重启
# 关机
shutdown -h now               # 立即关机
shutdown -h +10               # 10 分钟后关机
shutdown -h 22:00             # 22:00 关机
poweroff                      # 立即关机
halt                          # 停止系统

# 重启
shutdown -r now               # 立即重启
reboot                        # 立即重启
init 6                        # 重启

# 取消关机计划
shutdown -c

# 切换运行级别
systemctl isolate multi-user.target   # 命令行模式
systemctl isolate graphical.target    # 图形界面模式

# 查看当前运行级别
systemctl get-default
---
2. 文件与目录管理
2.1 目录结构
Linux 目录结构（FHS 标准）：
/                   根目录
├── bin             基本命令（ls, cp, mv 等）
├── boot            启动文件（内核、引导加载器）
├── dev             设备文件
├── etc             配置文件
├── home            用户主目录
├── lib             共享库
├── lib64           64 位共享库
├── media           可移动媒体挂载点
├── mnt             临时挂载点
├── opt             第三方软件
├── proc            进程和内核信息（虚拟文件系统）
├── root            root 用户主目录
├── run             运行时数据
├── sbin            系统管理命令
├── srv             服务数据
├── sys             内核和设备信息（虚拟文件系统）
├── tmp             临时文件
├── usr             用户程序和数据
│   ├── bin         用户命令
│   ├── lib         库文件
│   ├── local       本地安装的软件
│   ├── sbin        系统管理命令
│   └── share       共享数据
└── var             可变数据
    ├── log         日志文件
    ├── spool       邮件、打印队列
    ├── cache       缓存数据
    └── lib         可变状态数据
2.2 基本文件操作
# 目录操作
pwd                            # 当前目录
cd /path                       # 切换目录
cd ~                           # 回到主目录
cd -                           # 回到上次目录
cd ..                          # 上级目录
ls                             # 列出文件
ls -la                         # 详细列表（含隐藏文件）
ls -lh                         # 人性化大小显示
ls -lt                         # 按时间排序
ls -lS                         # 按大小排序
tree                           # 树状显示
tree -L 2                      # 只显示 2 层

# 创建目录
mkdir dir                      # 创建目录
mkdir -p a/b/c                 # 递归创建目录
mkdir -m 755 dir               # 创建并设置权限

# 创建文件
touch file.txt                 # 创建空文件或更新时间戳
touch -t 202401151030 file     # 设置时间戳

# 复制
cp file1 file2                 # 复制文件
cp -r dir1 dir2                # 递归复制目录
cp -a dir1 dir2                # 归档复制（保留所有属性）
cp -i file1 file2              # 覆盖前确认
cp -u file1 file2              # 只复制更新的文件

# 移动/重命名
mv file1 file2                 # 重命名
mv file1 /path/                # 移动文件
mv -i file1 file2              # 覆盖前确认

# 删除
rm file                        # 删除文件
rm -r dir                      # 递归删除目录
rm -rf dir                     # 强制递归删除（危险！）
rm -i file                     # 删除前确认

# 链接
ln -s target link_name         # 创建符号链接（软链接）
ln target link_name            # 创建硬链接
readlink -f link               # 查看链接目标
2.3 文件查找
# find 命令
find / -name "file.txt"                    # 按文件名查找
find / -name "*.log"                       # 通配符查找
find / -iname "File.TXT"                   # 忽略大小写
find / -type f -name "*.conf"              # 只查找文件
find / -type d -name "config"              # 只查找目录
find / -user username                      # 按所有者查找
find / -group groupname                    # 按组查找
find / -size +100M                         # 大于 100MB
find / -size -1M                           # 小于 1MB
find / -mtime -7                           # 7 天内修改的
find / -mtime +30                          # 30 天前修改的
find / -mmin -60                           # 60 分钟内修改的
find / -perm 755                           # 按权限查找
find / -empty                              # 空文件/目录

# 查找并执行操作
find / -name "*.log" -exec rm {} \;        # 查找并删除
find / -name "*.log" -exec gzip {} \;      # 查找并压缩
find / -name "*.tmp" -delete               # 直接删除
find / -name "*.conf" -exec cp {} /backup/ \;  # 复制到备份目录

# locate 命令（基于数据库，更快）
locate file.txt
updatedb                             # 更新数据库

# which 和 whereis
which ls                             # 查找命令路径
whereis ls                           # 查找命令、源码、手册

# file 命令（查看文件类型）
file file.txt
file image.jpg
file binary_file
2.4 文件内容查看
# cat（显示全部内容）
cat file.txt
cat -n file.txt                  # 显示行号
cat -A file.txt                  # 显示特殊字符

# less（分页查看，推荐）
less file.txt                    # 空格翻页，q 退出
# /keyword 向下搜索
# ?keyword 向上搜索
# n 下一个匹配
# N 上一个匹配

# more（分页查看，较旧）
more file.txt

# head（查看前几行）
head file.txt                    # 默认前 10 行
head -n 20 file.txt              # 前 20 行
head -c 100 file.txt             # 前 100 字节

# tail（查看后几行）
tail file.txt                    # 默认后 10 行
tail -n 20 file.txt              # 后 20 行
tail -f file.txt                 # 实时跟踪（查看日志常用）
tail -F file.txt                 # 跟踪（文件轮转后继续）
tail -f file.txt | grep error    # 实时过滤

# wc（统计）
wc file.txt                      # 行数 单词数 字节数
wc -l file.txt                   # 行数
wc -w file.txt                   # 单词数
wc -c file.txt                   # 字节数

# diff（比较文件）
diff file1 file2
diff -u file1 file2              # 统一格式
vimdiff file1 file2              # 可视化比较
2.5 文件压缩与解压
# gzip / gunzip
gzip file.txt                    # 压缩（原文件被替换）
gunzip file.txt.gz               # 解压
gzip -d file.txt.gz              # 解压
zcat file.txt.gz                 # 查看压缩文件内容

# bzip2 / bunzip2
bzip2 file.txt
bunzip2 file.txt.bz2

# xz / unxz
xz file.txt
unxz file.txt.xz

# tar（打包工具）
tar -cvf archive.tar dir/        # 打包
tar -xvf archive.tar             # 解包
tar -czvf archive.tar.gz dir/    # 打包并 gzip 压缩
tar -xzvf archive.tar.gz         # 解压 gzip
tar -cjvf archive.tar.bz2 dir/   # 打包并 bzip2 压缩
tar -xjvf archive.tar.bz2        # 解压 bzip2
tar -xzvf archive.tar.gz -C /dest/  # 解压到指定目录
tar -tzvf archive.tar.gz         # 查看压缩包内容

# zip / unzip
zip -r archive.zip dir/          # 压缩目录
unzip archive.zip                # 解压
unzip archive.zip -d /dest/      # 解压到指定目录
unzip -l archive.zip             # 查看内容

# 7z
7z a archive.7z dir/             # 压缩
7z x archive.7z                  # 解压
2.6 文件权限
# 权限说明
# -rw-r--r-- 1 root root 1024 Jan 15 10:00 file.txt
# │├─┤├─┤├─┤
# │ │   │  └── 其他用户权限
# │ │   └───── 所属组权限
# │ └───────── 所有者权限
# └─────────── 文件类型（- 文件, d 目录, l 链接, b 块设备, c 字符设备）

# 权限值
# r (读) = 4
# w (写) = 2
# x (执行) = 1
# - (无) = 0

# chmod（修改权限）
chmod 755 file                   # rwxr-xr-x
chmod 644 file                   # rw-r--r--
chmod u+x file                   # 给所有者添加执行权限
chmod g-w file                   # 移除组写权限
chmod o=r file                   # 其他用户只读
chmod a+r file                   # 所有人添加读权限
chmod -R 755 dir                 # 递归修改

# chown（修改所有者）
chown user file                  # 修改所有者
chown user:group file            # 修改所有者和组
chown -R user:group dir          # 递归修改

# chgrp（修改组）
chgrp group file

# umask（默认权限掩码）
umask                            # 查看当前 umask
umask 022                        # 设置 umask
# 文件默认权限 = 666 - umask = 644
# 目录默认权限 = 777 - umask = 755

# 特殊权限
chmod u+s file                   # SUID（以所有者身份执行）
chmod g+s dir                    # SGID（目录下新建文件继承组）
chmod +t dir                     # Sticky Bit（只有所有者能删除文件）
# 例如：/tmp 目录有 sticky bit

# ACL（访问控制列表）
getfacl file                     # 查看 ACL
setfacl -m u:user:rw file        # 设置用户 ACL
setfacl -m g:group:r file        # 设置组 ACL
setfacl -x u:user file           # 删除用户 ACL
setfacl -b file                  # 清除所有 ACL
---
3. 用户与权限管理
3.1 用户操作
# 创建用户
useradd username                 # 创建用户
useradd -m username              # 创建并创建主目录
useradd -m -s /bin/bash username # 指定 shell
useradd -m -g groupname username # 指定主组
useradd -m -G group1,group2 username  # 指定附加组
useradd -m -d /custom/home username   # 指定主目录
useradd -m -e 2024-12-31 username     # 设置过期日期
useradd -M -s /sbin/nologin username  # 系统用户，无主目录，不能登录

# 修改用户
usermod -l newname oldname       # 修改用户名
usermod -d /new/home username    # 修改主目录
usermod -s /bin/zsh username     # 修改 shell
usermod -aG groupname username   # 添加附加组（-a 追加）
usermod -G group1,group2 username # 设置附加组（覆盖）
usermod -L username              # 锁定用户
usermod -U username              # 解锁用户
usermod -e 2024-12-31 username   # 设置过期日期

# 删除用户
userdel username                 # 删除用户（保留主目录）
userdel -r username              # 删除用户和主目录

# 设置密码
passwd username                  # 设置用户密码
passwd -l username               # 锁定密码
passwd -u username               # 解锁密码
passwd -e username               # 强制下次登录修改密码
chage -l username                # 查看密码策略
chage -M 90 username             # 密码最长有效期 90 天
chage -I 10 username             # 密码过期 10 天后锁定

# 查看用户信息
id username                      # 用户和组信息
finger username                  # 用户详细信息
whoami                           # 当前用户
who                              # 登录用户
w                                # 登录用户和活动
last                             # 登录历史
lastlog                          # 最后登录时间
3.2 组操作
# 创建组
groupadd groupname
groupadd -g 1001 groupname       # 指定 GID

# 修改组
groupmod -n newname oldname      # 重命名组
groupmod -g 1002 groupname       # 修改 GID

# 删除组
groupdel groupname

# 查看组
cat /etc/group
groups username                  # 用户所属组
id username                      # 用户和组信息

# 管理组成员
gpasswd -a username groupname    # 添加用户到组
gpasswd -d username groupname    # 从组移除用户
3.3 sudo 配置
# 编辑 sudoers 文件
visudo                           # 安全编辑

# /etc/sudoers 配置示例
# 允许用户执行所有命令
username ALL=(ALL:ALL) ALL

# 允许用户执行特定命令
username ALL=(ALL) /usr/bin/systemctl restart nginx

# 允许组执行命令
%groupname ALL=(ALL) ALL

# 免密 sudo
username ALL=(ALL) NOPASSWD: ALL
username ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart nginx

# 查看 sudo 权限
sudo -l                          # 当前用户的 sudo 权限
sudo -l -U username              # 指定用户的 sudo 权限

# sudo 日志
tail -f /var/log/secure          # CentOS/RHEL
tail -f /var/log/auth.log        # Ubuntu/Debian
3.4 相关文件
/etc/passwd    用户账户信息
  格式：用户名:密码:UID:GID:注释:主目录:Shell

/etc/shadow    用户密码信息
  格式：用户名:加密密码:最后修改:最小间隔:最大间隔:警告:不活动:过期:保留

/etc/group     组信息
  格式：组名:密码:GID:成员列表

/etc/gshadow   组密码信息

/etc/sudoers   sudo 配置
---
4. 进程管理
4.1 查看进程
# ps 命令
ps aux                           # 所有进程（BSD 风格）
ps -ef                           # 所有进程（System V 风格）
ps aux | grep nginx              # 过滤进程
ps -eo pid,ppid,user,cmd --sort=-%mem | head  # 按内存排序
ps -eo pid,ppid,user,cmd --sort=-%cpu | head  # 按 CPU 排序

# top 命令（实时监控）
top
# 交互命令：
# P — 按 CPU 排序
# M — 按内存排序
# k — 杀死进程
# 1 — 显示每个 CPU 核心
# q — 退出

# htop（增强版 top）
htop

# pstree（进程树）
pstree
pstree -p                        # 显示 PID

# pgrep（按名称查找 PID）
pgrep nginx
pgrep -l nginx                   # 显示名称
pgrep -u username                # 按用户查找
4.2 进程控制
# 后台运行
command &                        # 后台运行
nohup command &                  # 后台运行，不受终端关闭影响
nohup command > output.log 2>&1 &  # 后台运行并重定向输出

# jobs 命令
jobs                             # 查看后台任务
jobs -l                          # 显示 PID

# 前台/后台切换
Ctrl+Z                           # 暂停当前进程
bg %1                            # 暂停的进程放到后台运行
fg %1                            # 后台进程调到前台

# kill 命令
kill PID                         # 发送 SIGTERM（15）
kill -9 PID                      # 发送 SIGKILL（9，强制杀死）
kill -HUP PID                    # 发送 SIGHUP（1，重新加载配置）
kill -USR1 PID                   # 发送 USR1（自定义信号）
killall nginx                    # 按名称杀死所有进程
pkill -f "python app.py"         # 按命令行模式杀死

# 信号列表
# SIGHUP  (1)   挂起（重新加载配置）
# SIGINT  (2)   中断（Ctrl+C）
# SIGQUIT (3)   退出
# SIGKILL (9)   强制杀死（不可捕获）
# SIGTERM (15)  终止（默认信号）
# SIGSTOP (19)  暂停（不可捕获）
# SIGCONT (18)  继续
# SIGUSR1 (10)  用户自定义 1
# SIGUSR2 (12)  用户自定义 2
4.3 系统资源监控
# 内存
free -h                          # 内存使用
free -m                          # 以 MB 显示
cat /proc/meminfo                # 详细内存信息

# CPU
uptime                           # 负载（1/5/15 分钟）
cat /proc/loadavg                # 负载详情
mpstat                           # CPU 使用率（需安装 sysstat）
mpstat -P ALL                    # 所有 CPU 核心

# 磁盘 I/O
iostat                           # 磁盘 I/O（需安装 sysstat）
iostat -x 1                      # 每秒刷新
iotop                            # 按进程显示 I/O

# 综合监控
vmstat 1                         # 每秒刷新
# r — 运行队列
# b — 阻塞进程
# swpd — 虚拟内存使用
# free — 空闲内存
# buff — 缓冲区
# cache — 缓存
# si/so — 换入/换出
# bi/bo — 块输入/块输出
# us — 用户 CPU
# sy — 系统 CPU
# id — 空闲 CPU
# wa — 等待 I/O

# sar（系统活动报告）
sar -u 1 10                      # CPU 使用率（每秒，10 次）
sar -r 1 10                      # 内存使用
sar -d 1 10                      # 磁盘 I/O
sar -n DEV 1 10                  # 网络接口
sar -q 1 10                      # 负载
---
5. systemd 服务管理
5.1 systemctl 命令
# 服务操作
systemctl start nginx            # 启动服务
systemctl stop nginx             # 停止服务
systemctl restart nginx          # 重启服务
systemctl reload nginx           # 重新加载配置
systemctl status nginx           # 查看状态

# 开机自启
systemctl enable nginx           # 启用开机自启
systemctl disable nginx          # 禁用开机自启
systemctl is-enabled nginx       # 检查是否自启
systemctl enable --now nginx     # 启用并立即启动

# 查看服务
systemctl list-units --type=service              # 所有服务
systemctl list-units --type=service --state=running  # 运行中的服务
systemctl list-unit-files --type=service         # 所有服务文件

# 查看服务日志
journalctl -u nginx              # 服务日志
journalctl -u nginx -f           # 实时跟踪
journalctl -u nginx --since "1 hour ago"  # 最近 1 小时
journalctl -u nginx --since "2024-01-15" --until "2024-01-16"

# 服务依赖
systemctl list-dependencies nginx
systemctl list-dependencies --reverse nginx

# 重新加载 systemd（修改 service 文件后）
systemctl daemon-reload
5.2 编写 Service 文件
# /etc/systemd/system/myapp.service
[Unit]
Description=My Application
Documentation=https://example.com/docs
After=network.target
Requires=postgresql.service
Wants=redis.service

[Service]
Type=simple
User=appuser
Group=appgroup
WorkingDirectory=/opt/myapp
Environment=APP_ENV=production
EnvironmentFile=/opt/myapp/.env
ExecStartPre=/opt/myapp/pre-start.sh
ExecStart=/opt/myapp/server --config /opt/myapp/config.yaml
ExecStartPost=/opt/myapp/post-start.sh
ExecReload=/bin/kill -HUP $MAINPID
ExecStop=/opt/myapp/stop.sh
Restart=always
RestartSec=5
StartLimitBurst=3
StartLimitIntervalSec=60
TimeoutStartSec=30
TimeoutStopSec=30
KillMode=mixed
KillSignal=SIGTERM

# 安全加固
NoNewPrivileges=true
PrivateTmp=true
ProtectSystem=strict
ProtectHome=true
ReadWritePaths=/opt/myapp/data /var/log/myapp
CapabilityBoundingSet=CAP_NET_BIND_SERVICE

# 资源限制
LimitNOFILE=65535
LimitNPROC=4096
MemoryMax=2G
CPUQuota=200%

[Install]
WantedBy=multi-user.target
# 定时器（替代 cron）
# /etc/systemd/system/mybackup.timer
[Unit]
Description=Daily Backup Timer

[Timer]
OnCalendar=*-*-* 02:00:00
Persistent=true
RandomizedDelaySec=600

[Install]
WantedBy=timers.target

# /etc/systemd/system/mybackup.service
[Unit]
Description=Daily Backup

[Service]
Type=oneshot
ExecStart=/opt/scripts/backup.sh
5.3 日志管理（journalctl）
# 查看所有日志
journalctl

# 按服务查看
journalctl -u nginx
journalctl -u nginx -u mysql     # 多个服务

# 按时间查看
journalctl --since "2024-01-15 10:00:00"
journalctl --since "1 hour ago"
journalctl --since "today"
journalctl --since yesterday --until today

# 按优先级查看
journalctl -p err                 # 错误及以上
journalctl -p warning             # 警告及以上
# 优先级：emerg, alert, crit, err, warning, notice, info, debug

# 按 PID 查看
journalctl _PID=1234

# 按启动查看
journalctl -b                     # 当前启动
journalctl -b -1                  # 上次启动

# 实时跟踪
journalctl -f
journalctl -u nginx -f

# 磁盘使用
journalctl --disk-usage

# 清理日志
journalctl --vacuum-time=7d       # 保留 7 天
journalctl --vacuum-size=500M     # 保留 500MB

# 配置持久化
mkdir -p /var/log/journal
systemd-tmpfiles --create --prefix /var/log/journal
---
6. 磁盘与存储管理
6.1 磁盘信息
# 查看磁盘
lsblk                            # 块设备树
lsblk -f                         # 显示文件系统
fdisk -l                         # 磁盘分区表
blkid                            # 块设备 UUID 和类型
df -h                            # 文件系统使用情况
df -i                            # inode 使用情况
du -sh /path                     # 目录大小
du -sh *                         # 当前目录各文件/目录大小
du -sh * | sort -rh | head -10   # 按大小排序
6.2 分区与格式化
# fdisk 分区（MBR）
fdisk /dev/sdb
# n — 新建分区
# d — 删除分区
# p — 打印分区表
# t — 更改分区类型
# w — 保存退出
# q — 不保存退出

# parted 分区（GPT，推荐）
parted /dev/sdb
# mklabel gpt
# mkpart primary ext4 0% 100%
# print

# 格式化
mkfs.ext4 /dev/sdb1              # ext4
mkfs.xfs /dev/sdb1               # xfs
mkfs.btrfs /dev/sdb1             # btrfs
mkfs.vfat /dev/sdb1              # FAT32

# 创建交换分区
mkswap /dev/sdb2
swapon /dev/sdb2
6.3 挂载与卸载
# 挂载
mount /dev/sdb1 /mnt/data
mount -t ext4 /dev/sdb1 /mnt/data
mount -o ro /dev/sdb1 /mnt/data  # 只读挂载
mount -o remount,rw /mnt/data    # 重新挂载为读写

# 卸载
umount /mnt/data
umount -l /mnt/data              # 延迟卸载（忙碌时）
umount -f /mnt/data              # 强制卸载

# 查看挂载
mount
df -h
findmnt

# 永久挂载（/etc/fstab）
# 格式：设备  挂载点  文件系统  选项  dump  fsck
/dev/sdb1  /mnt/data  ext4  defaults  0  2
UUID=xxx   /mnt/data  ext4  defaults  0  2

# 重新加载 fstab
mount -a

# 查看 UUID
blkid
6.4 LVM（逻辑卷管理）
# 创建物理卷
pvcreate /dev/sdb /dev/sdc
pvdisplay
pvs

# 创建卷组
vgcreate vg_data /dev/sdb /dev/sdc
vgdisplay
vgs

# 创建逻辑卷
lvcreate -L 100G -n lv_data vg_data
lvcreate -l 100%FREE -n lv_data vg_data
lvdisplay
lvs

# 格式化和挂载
mkfs.ext4 /dev/vg_data/lv_data
mount /dev/vg_data/lv_data /mnt/data

# 扩展逻辑卷
lvextend -L +50G /dev/vg_data/lv_data
resize2fs /dev/vg_data/lv_data   # ext4
xfs_growfs /mnt/data              # xfs

# 一步完成扩展
lvextend -r -L +50G /dev/vg_data/lv_data

# 缩减逻辑卷（仅 ext4）
umount /mnt/data
e2fsck -f /dev/vg_data/lv_data
resize2fs /dev/vg_data/lv_data 50G
lvreduce -L 50G /dev/vg_data/lv_data
mount /dev/vg_data/lv_data /mnt/data

# 扩展卷组
vgextend vg_data /dev/sdd

# 快照
lvcreate -L 10G -s -n snap_data /dev/vg_data/lv_data

# 恢复快照
lvconvert --merge /dev/vg_data/snap_data
---
7. 网络管理
7.1 网络配置
# 查看网络接口
ip addr show
ip a
ip -br addr show                 # 简洁显示
ifconfig                         # 旧命令

# 查看路由
ip route show
ip route get 8.8.8.8             # 查看到达目标的路由
route -n                         # 旧命令

# 查看 ARP
ip neigh show

# DNS 查询
nslookup example.com
dig example.com
host example.com
7.2 NetworkManager（nmcli）
# 查看连接
nmcli connection show
nmcli device status

# 设置静态 IP
nmcli connection modify "Wired connection 1" \
  ipv4.method manual \
  ipv4.addresses "192.168.1.100/24" \
  ipv4.gateway "192.168.1.1" \
  ipv4.dns "8.8.8.8 8.8.4.4"

# 设置 DHCP
nmcli connection modify "Wired connection 1" ipv4.method auto

# 启用/禁用
nmcli connection up "Wired connection 1"
nmcli connection down "Wired connection 1"

# 添加连接
nmcli connection add type ethernet \
  con-name "static-eth0" \
  ifname eth0 \
  ipv4.method manual \
  ipv4.addresses "192.168.1.100/24" \
  ipv4.gateway "192.168.1.1"

# WiFi
nmcli device wifi list
nmcli device wifi connect "SSID" password "password"
7.3 Netplan（Ubuntu）
# /etc/netplan/01-netcfg.yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: false
      addresses:
        - 192.168.1.100/24
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
sudo netplan apply
sudo netplan try                 # 测试配置（超时自动回滚）
7.4 网络诊断
# 测试连通性
ping 8.8.8.8
ping -c 5 8.8.8.8               # 发送 5 个包
ping -i 0.2 8.8.8.8             # 间隔 0.2 秒

# 路由追踪
traceroute example.com
mtr example.com                  # 实时路由追踪

# 端口测试
telnet 192.168.1.100 80
nc -zv 192.168.1.100 80
nc -zv 192.168.1.100 1-1000     # 扫描端口范围

# 网络连接
ss -tlnp                         # 监听端口
ss -tan                          # 所有连接
ss -s                            # 连接统计
netstat -tlnp                    # 旧命令

# 抓包
tcpdump -i eth0
tcpdump -i eth0 -nn port 80
tcpdump -i eth0 host 192.168.1.100
---
8. 软件包管理
8.1 YUM/DNF（CentOS/RHEL）
# 安装
yum install package
dnf install package               # CentOS 8+/RHEL 8+

# 卸载
yum remove package

# 更新
yum update                        # 更新所有
yum update package                # 更新指定包

# 搜索
yum search keyword
yum list available | grep keyword

# 查看信息
yum info package

# 查看已安装
yum list installed

# 清理缓存
yum clean all

# 仓库管理
yum repolist
yum-config-manager --add-repo URL

# 安装本地 RPM
yum localinstall package.rpm

# 组安装
yum groupinstall "Development Tools"
yum groupinstall "Server with GUI"
8.2 APT（Ubuntu/Debian）
# 更新包索引
apt update

# 安装
apt install package

# 卸载
apt remove package
apt purge package                 # 卸载并删除配置

# 更新
apt upgrade                       # 更新所有
apt full-upgrade                  # 更新（可删除旧包）

# 搜索
apt search keyword

# 查看信息
apt show package

# 清理
apt autoremove                    # 删除不需要的依赖
apt clean                         # 清理缓存

# 仓库管理
add-apt-repository ppa:user/repo

# 安装本地 DEB
dpkg -i package.deb
apt install -f                    # 修复依赖
---
9. Shell 与 Bash
9.1 变量
# 定义变量
name="hello"
NAME="Hello World"               # 建议大写（环境变量）
readonly PI=3.14                 # 只读变量

# 使用变量
echo $name
echo ${name}

# 环境变量
export APP_ENV="production"      # 导出为环境变量
env                              # 查看所有环境变量
printenv                         # 同上
unset APP_ENV                    # 删除变量

# 特殊变量
$0    脚本名称
$1-$9 命令行参数
$#    参数个数
$*    所有参数（一个字符串）
$@    所有参数（多个字符串）
$?    上一个命令的退出码
$$    当前脚本的 PID
$!    后台运行的最后一个进程的 PID

# 字符串操作
${#name}                         # 字符串长度
${name:0:3}                      # 子串（从 0 开始，长度 3）
${name/old/new}                  # 替换第一个
${name//old/new}                 # 替换所有
${name:-default}                 # 未设置时使用默认值
${name:=default}                 # 未设置时赋默认值
${name:+value}                   # 已设置时使用 value
9.2 条件判断
# if 语法
if [ condition ]; then
    commands
elif [ condition ]; then
    commands
else
    commands
fi

# 条件表达式
[ -f file ]         # 文件存在且是普通文件
[ -d dir ]          # 目录存在
[ -e path ]         # 路径存在
[ -r file ]         # 文件可读
[ -w file ]         # 文件可写
[ -x file ]         # 文件可执行
[ -s file ]         # 文件非空
[ -L file ]         # 文件是符号链接

[ -z "$str" ]       # 字符串为空
[ -n "$str" ]       # 字符串非空
[ "$a" = "$b" ]     # 字符串相等
[ "$a" != "$b" ]    # 字符串不等

[ $a -eq $b ]       # 数值相等
[ $a -ne $b ]       # 数值不等
[ $a -gt $b ]       # 大于
[ $a -ge $b ]       # 大于等于
[ $a -lt $b ]       # 小于
[ $a -le $b ]       # 小于等于

[ cond1 -a cond2 ]  # AND
[ cond1 -o cond2 ]  # OR
[ ! cond ]          # NOT

# [[ ]] 增强版（推荐）
[[ "$str" == *pattern* ]]     # 模式匹配
[[ "$str" =~ regex ]]         # 正则匹配
[[ $a -gt 0 && $a -lt 100 ]] # 逻辑运算

# 文件比较
[ file1 -nt file2 ]           # file1 更新
[ file1 -ot file2 ]           # file1 更旧

# 示例
if [[ -f /etc/nginx/nginx.conf ]]; then
    echo "Nginx 已安装"
fi

if [[ "$1" == "" ]]; then
    echo "用法: $0 <参数>"
    exit 1
fi
9.3 循环
# for 循环
for i in 1 2 3 4 5; do
    echo $i
done

for i in {1..100}; do
    echo $i
done

for i in $(seq 1 2 100); do   # 步长 2
    echo $i
done

for file in /var/log/*.log; do
    echo "处理: $file"
done

for ((i=0; i<10; i++)); do
    echo $i
done

# while 循环
while [[ $i -lt 10 ]]; do
    echo $i
    ((i++))
done

# 读取文件
while IFS= read -r line; do
    echo "$line"
done < file.txt

# 读取命令输出
while IFS= read -r line; do
    echo "$line"
done < <(ps aux)

# until 循环
until [[ $i -ge 10 ]]; do
    echo $i
    ((i++))
done

# 无限循环
while true; do
    echo "running..."
    sleep 1
done

# break 和 continue
for i in {1..10}; do
    [[ $i -eq 5 ]] && continue  # 跳过 5
    [[ $i -eq 8 ]] && break      # 到 8 停止
    echo $i
done
9.4 函数
# 定义函数
function greet() {
    echo "Hello, $1!"
}

# 或
greet() {
    local name=$1               # local 局部变量
    echo "Hello, $name!"
    return 0                    # 返回值（0-255）
}

# 调用
greet "World"

# 获取返回值
greet "World"
echo $?                          # 退出码

# 返回字符串（通过 echo）
get_value() {
    echo "result"
}
result=$(get_value)

# 带默认参数
greet() {
    local name=${1:-"World"}
    echo "Hello, $name!"
}
9.5 数组
# 定义数组
arr=(one two three four five)

# 访问元素
echo ${arr[0]}                   # 第一个元素
echo ${arr[@]}                   # 所有元素
echo ${#arr[@]}                  # 数组长度

# 添加元素
arr+=(six)

# 切片
echo ${arr[@]:1:3}               # 从索引 1 开始，取 3 个

# 关联数组（Bash 4+）
declare -A map
map[name]="张三"
map[age]=25
echo ${map[name]}
echo ${!map[@]}                  # 所有键
echo ${map[@]}                   # 所有值

# 遍历数组
for item in "${arr[@]}"; do
    echo "$item"
done

for key in "${!map[@]}"; do
    echo "$key = ${map[$key]}"
done
9.6 实用脚本模板
#!/bin/bash
# 脚本模板

set -euo pipefail                # 出错退出、未定义变量报错、管道错误

# 颜色定义
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m'                     # 恢复默认

# 日志函数
log_info()  { echo -e "${GREEN}[INFO]${NC} $*"; }
log_warn()  { echo -e "${YELLOW}[WARN]${NC} $*"; }
log_error() { echo -e "${RED}[ERROR]${NC} $*" >&2; }

# 错误处理
trap 'log_error "脚本执行失败，行号: $LINENO"' ERR
trap 'cleanup' EXIT

cleanup() {
    # 清理临时文件
    rm -f /tmp/myapp_*.tmp
}

# 主函数
main() {
    log_info "开始执行..."

    # 检查参数
    if [[ $# -lt 1 ]]; then
        log_error "用法: $0 <参数>"
        exit 1
    fi

    # 检查是否 root
    if [[ $EUID -ne 0 ]]; then
        log_error "请使用 root 执行此脚本"
        exit 1
    fi

    # 执行逻辑
    log_info "执行完成"
}

main "$@"
---
10. 文本处理三剑客
10.1 grep
# 基本搜索
grep "pattern" file
grep -i "pattern" file           # 忽略大小写
grep -r "pattern" /path/         # 递归搜索
grep -n "pattern" file           # 显示行号
grep -c "pattern" file           # 匹配行数
grep -l "pattern" *.log          # 只显示文件名
grep -v "pattern" file           # 反向匹配（不包含）
grep -w "word" file              # 全词匹配
grep -A 5 "pattern" file         # 匹配后 5 行
grep -B 5 "pattern" file         # 匹配前 5 行
grep -C 3 "pattern" file         # 匹配前后 3 行

# 正则表达式
grep -E "pattern1|pattern2" file # 扩展正则（ERE）
grep -P "\d+" file               # Perl 正则（PCRE）
grep "^[0-9]" file               # 以数字开头
grep "error\|warn" file          # 基本正则（BRE）

# 实用示例
grep -rn "TODO" /path/           # 递归搜索 TODO
grep "error" /var/log/syslog | tail -20
ps aux | grep nginx
cat /etc/passwd | grep -v "nologin"
10.2 sed
# 替换
sed 's/old/new/' file            # 替换每行第一个
sed 's/old/new/g' file           # 替换所有
sed 's/old/new/2' file           # 替换每行第二个
sed -i 's/old/new/g' file        # 直接修改文件
sed -i.bak 's/old/new/g' file   # 修改前备份

# 删除
sed '3d' file                    # 删除第 3 行
sed '2,5d' file                  # 删除 2-5 行
sed '/pattern/d' file            # 删除匹配行
sed '/^$/d' file                 # 删除空行

# 插入和追加
sed '3i\新行内容' file           # 在第 3 行前插入
sed '3a\新行内容' file           # 在第 3 行后追加

# 打印
sed -n '5p' file                 # 打印第 5 行
sed -n '2,5p' file               # 打印 2-5 行
sed -n '/pattern/p' file         # 打印匹配行

# 实用示例
sed 's/^[[:space:]]*//' file     # 删除行首空格
sed 's/[[:space:]]*$//' file     # 删除行尾空格
sed '/^#/d' file                 # 删除注释行
sed '/^$/d' file                 # 删除空行
sed -n '10,20p' file             # 打印 10-20 行
sed 's/"/\x27/g' file            # 替换引号
10.3 awk
# 基本语法：awk 'pattern {action}' file

# 打印列
awk '{print $1}' file            # 第一列
awk '{print $1, $3}' file        # 第一列和第三列
awk '{print $NF}' file           # 最后一列
awk '{print NR, $0}' file        # 行号和整行

# 分隔符
awk -F: '{print $1}' /etc/passwd # 指定分隔符
awk -F',' '{print $2}' file.csv  # CSV 文件

# 条件过滤
awk '$3 > 100' file              # 第三列大于 100
awk '/pattern/' file             # 匹配行
awk '$1 == "root"' /etc/passwd   # 第一列等于 root

# 内置变量
NR    行号
NF    字段数
FS    输入字段分隔符
OFS   输出字段分隔符
RS    输入记录分隔符
ORS   输出记录分隔符
FILENAME  当前文件名

# BEGIN 和 END
awk 'BEGIN {print "开始"} {print} END {print "结束"}' file

# 统计
awk '{sum+=$1} END {print sum}' file          # 求和
awk '{sum+=$1; count++} END {print sum/count}' file  # 平均值

# 格式化输出
awk '{printf "%-20s %10d\n", $1, $2}' file

# 数组
awk '{count[$1]++} END {for(k in count) print k, count[k]}' file

# 实用示例
# 统计 IP 访问次数
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -10

# 统计 HTTP 状态码
awk '{print $9}' access.log | sort | uniq -c | sort -rn

# 打印特定范围
awk 'NR>=10 && NR<=20' file

# 多个分隔符
awk -F'[,;:]' '{print $1}' file
---
11. 计划任务 Cron
11.1 crontab
# 编辑计划任务
crontab -e

# 查看计划任务
crontab -l

# 删除所有计划任务
crontab -r

# 指定用户
crontab -u username -e
crontab -u username -l

# cron 表达式格式：
# ┌───── 分钟 (0-59)
# │ ┌───── 小时 (0-23)
# │ │ ┌───── 日期 (1-31)
# │ │ │ ┌───── 月份 (1-12)
# │ │ │ │ ┌───── 星期 (0-7, 0和7都是周日)
# │ │ │ │ │
# * * * * * command

# 示例
# 每天凌晨 2 点执行
0 2 * * * /opt/scripts/backup.sh

# 每 5 分钟执行
*/5 * * * * /opt/scripts/check.sh

# 每小时执行
0 * * * * /opt/scripts/cleanup.sh

# 每天 8:30 执行
30 8 * * * /opt/scripts/report.sh

# 每周一 9:00 执行
0 9 * * 1 /opt/scripts/weekly.sh

# 每月 1 号 0:00 执行
0 0 1 * * /opt/scripts/monthly.sh

# 每天 8-18 点每小时执行
0 8-18 * * * /opt/scripts/hourly.sh

# 工作日执行
0 9 * * 1-5 /opt/scripts/workday.sh

# 特殊字符串
@reboot    启动时执行
@yearly    每年（0 0 1 1 *）
@monthly   每月（0 0 1 * *）
@weekly    每周（0 0 * * 0）
@daily     每天（0 0 * * *）
@hourly    每小时（0 * * * *）
11.2 /etc/cron.d
# 系统级计划任务
# /etc/cron.d/myapp
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

0 2 * * * root /opt/scripts/backup.sh
*/5 * * * * root /opt/scripts/check.sh

# /etc/cron.daily/    每天执行的脚本
# /etc/cron.weekly/   每周执行的脚本
# /etc/cron.monthly/  每月执行的脚本
# /etc/cron.hourly/   每小时执行的脚本
11.3 anacron
# anacron 用于不能 24 小时运行的系统
# /etc/anacrontab
# period  delay  job-identifier  command
1         5      daily-backup    /opt/scripts/backup.sh
7         10     weekly-report   /opt/scripts/report.sh
---
12. 日志管理
12.1 日志文件位置
/var/log/
├── syslog           系统日志（Ubuntu/Debian）
├── messages         系统日志（CentOS/RHEL）
├── auth.log         认证日志（Ubuntu/Debian）
├── secure           认证日志（CentOS/RHEL）
├── boot.log         启动日志
├── cron             计划任务日志
├── maillog          邮件日志
├── dmesg            内核日志
├── nginx/           Nginx 日志
├── mysql/           MySQL 日志
└── journal/         systemd 日志
12.2 日志查看
# 实时查看
tail -f /var/log/syslog
tail -f /var/log/messages
tail -f /var/log/auth.log

# 过滤日志
grep "error" /var/log/syslog
grep -i "failed" /var/log/secure
journalctl -u nginx --since "1 hour ago"

# 日志轮转
cat /etc/logrotate.conf
cat /etc/logrotate.d/nginx

# logrotate 配置示例
# /etc/logrotate.d/myapp
/var/log/myapp/*.log {
    daily
    rotate 30
    compress
    delaycompress
    missingok
    notifempty
    create 0644 appuser appgroup
    sharedscripts
    postrotate
        systemctl reload myapp
    endscript
}

# 手动轮转
logrotate -f /etc/logrotate.d/myapp
12.3 rsyslog 配置
# /etc/rsyslog.conf
# 将所有日志发送到远程服务器
*.* @@192.168.1.100:514          # TCP
*.* @192.168.1.100:514           # UDP

# 按设施和级别过滤
auth,authpriv.*                  /var/log/auth.log
*.*;auth,authpriv.none           /var/log/syslog
mail.*                           /var/log/mail.log
---
13. SSH 远程管理
13.1 SSH 基本使用
# 连接
ssh user@hostname
ssh user@192.168.1.100
ssh -p 2222 user@hostname        # 指定端口
ssh -i ~/.ssh/id_rsa user@host   # 指定密钥

# 执行远程命令
ssh user@host "ls -la /tmp"
ssh user@host "df -h && free -h"

# 密钥认证
ssh-keygen -t ed25519            # 生成密钥（推荐）
ssh-keygen -t rsa -b 4096        # 生成 RSA 密钥
ssh-copy-id user@host            # 复制公钥到远程

# SCP 文件传输
scp file.txt user@host:/path/    # 上传
scp user@host:/path/file.txt ./  # 下载
scp -r dir/ user@host:/path/     # 递归复制目录

# SFTP
sftp user@host
# get file.txt
# put file.txt
# ls, cd, pwd, mkdir, rm

# SSH 隧道（端口转发）
ssh -L 8080:localhost:80 user@host       # 本地转发
ssh -R 9090:localhost:80 user@host       # 远程转发
ssh -D 1080 user@host                    # 动态转发（SOCKS 代理）

# SSH 跳板
ssh -J jumpuser@jumphost user@target
ssh -o ProxyCommand="ssh -W %h:%p jumpuser@jumphost" user@target
13.2 SSH 配置
# /etc/ssh/sshd_config（服务端配置）
Port 22                          # 监听端口
PermitRootLogin no               # 禁止 root 登录
PasswordAuthentication no        # 禁用密码认证
PubkeyAuthentication yes         # 启用公钥认证
MaxAuthTries 3                   # 最大认证尝试次数
ClientAliveInterval 60           # 心跳间隔
ClientAliveCountMax 3            # 心跳次数
AllowUsers user1 user2           # 允许的用户
DenyUsers root                   # 禁止的用户
AllowGroups sshusers             # 允许的组
X11Forwarding no                 # 禁用 X11 转发

# 重新加载配置
systemctl reload sshd

# ~/.ssh/config（客户端配置）
Host myserver
    HostName 192.168.1.100
    User admin
    Port 2222
    IdentityFile ~/.ssh/id_rsa

Host jumphost
    HostName jump.example.com
    User jumpuser

Host target
    HostName 192.168.1.200
    User admin
    ProxyJump jumphost
---
14. 性能监控与调优
14.1 性能分析工具
# CPU
top / htop
mpstat -P ALL 1
pidstat -u 1

# 内存
free -h
vmstat 1
slabtop                          # 内核 slab 缓存

# 磁盘 I/O
iostat -x 1
iotop
pidstat -d 1

# 网络
iftop                            # 网络流量
nethogs                          # 按进程显示网络
nload                            # 网络带宽
sar -n DEV 1

# 综合
dstat                            # 多功能资源统计
glances                          # 综合监控
nmon                             # 性能监控
14.2 内核参数调优
# /etc/sysctl.conf

# 网络优化
net.core.somaxconn = 65535
net.ipv4.tcp_max_syn_backlog = 65535
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_fin_timeout = 30
net.ipv4.tcp_keepalive_time = 600
net.ipv4.ip_local_port_range = 1024 65535

# 文件描述符
fs.file-max = 655350

# 内存
vm.swappiness = 10
vm.overcommit_memory = 0

# 应用
sysctl -p

# 查看当前值
sysctl net.ipv4.tcp_tw_reuse
cat /proc/sys/net/ipv4/tcp_tw_reuse
14.3 资源限制
# /etc/security/limits.conf
* soft nofile 65535
* hard nofile 65535
* soft nproc 65535
* hard nproc 65535
root soft nofile 65535
root hard nofile 65535

# 查看当前限制
ulimit -a
ulimit -n                        # 文件描述符限制
ulimit -u                        # 进程数限制

# 临时修改
ulimit -n 65535

# systemd 服务限制
# 在 service 文件中设置
# LimitNOFILE=65535
# LimitNPROC=65535
---
15. 安全加固
15.1 系统安全
# 1. 更新系统
yum update -y                    # CentOS
apt update && apt upgrade -y     # Ubuntu

# 2. 禁用 root SSH 登录
# /etc/ssh/sshd_config
PermitRootLogin no

# 3. 禁用密码认证
PasswordAuthentication no

# 4. 配置防火墙
firewall-cmd --permanent --add-service=ssh
firewall-cmd --permanent --add-service=http
firewall-cmd --permanent --add-service=https
firewall-cmd --reload

# 或使用 ufw
ufw allow ssh
ufw allow http
ufw allow https
ufw enable

# 5. 禁用不需要的服务
systemctl disable telnet
systemctl disable ftp

# 6. 设置密码策略
# /etc/login.defs
PASS_MAX_DAYS 90
PASS_MIN_DAYS 7
PASS_MIN_LEN 12
PASS_WARN_AGE 14

# 7. 配置 fail2ban
apt install fail2ban
systemctl enable fail2ban
15.2 fail2ban 配置
# /etc/fail2ban/jail.local
[DEFAULT]
bantime = 3600
findtime = 600
maxretry = 5

[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
bantime = 86400

[nginx-http-auth]
enabled = true
port = http,https
filter = nginx-http-auth
logpath = /var/log/nginx/error.log
maxretry = 3
# fail2ban 命令
fail2ban-client status
fail2ban-client status sshd
fail2ban-client set sshd unbanip 192.168.1.100
fail2ban-client set sshd banip 192.168.1.100
15.3 SELinux
# 查看状态
getenforce
sestatus

# 设置模式
setenforce 0                     # 临时关闭（Permissive）
setenforce 1                     # 临时开启（Enforcing）

# 永久关闭
# /etc/selinux/config
SELINUX=disabled

# 管理上下文
chcon -R -t httpd_sys_content_t /var/www/html
restorecon -R /var/www/html

# 布尔值
getsebool -a | grep httpd
setsebool -P httpd_can_network_connect on
---
16. 故障排查
16.1 系统无法启动
1. 进入单用户模式/救援模式
2. 检查磁盘空间：df -h
3. 检查 inode：df -i
4. 检查文件系统：fsck /dev/sda1
5. 检查日志：journalctl -xb
6. 检查 /etc/fstab 是否有错误
16.2 磁盘空间满
# 查找大文件
du -sh /* | sort -rh | head -20
find / -type f -size +100M -exec ls -lh {} \; 2>/dev/null

# 查找大目录
du -sh /var/log/*
du -sh /tmp/*

# 清理
journalctl --vacuum-size=500M    # 清理日志
yum clean all                    # 清理包缓存
apt clean                        # 清理包缓存
rm -rf /tmp/*                    # 清理临时文件
docker system prune              # 清理 Docker
16.3 内存不足
# 查看内存使用
free -h
cat /proc/meminfo

# 查找占用内存的进程
ps aux --sort=-%mem | head -20

# 清理缓存
sync; echo 3 > /proc/sys/vm/drop_caches

# 查看 OOM Killer 日志
dmesg | grep -i "oom\|killed"
journalctl -k | grep -i "oom"
16.4 网络故障
# 检查网络接口
ip addr show
ip link show

# 检查路由
ip route show
ping -c 3 192.168.1.1            # ping 网关
ping -c 3 8.8.8.8                # ping 外网
nslookup example.com             # DNS 测试

# 检查防火墙
iptables -L -n
firewall-cmd --list-all

# 检查端口
ss -tlnp
netstat -tlnp

# 检查网络连接
ss -tan | wc -l
ss -tan state established | wc -l
---
17. 命令速查表
17.1 文件操作
命令	用途
ls -la	列出文件
cd /path	切换目录
pwd	当前目录
cp -r src dst	复制
mv src dst	移动/重命名
rm -rf dir	删除
mkdir -p dir	创建目录
touch file	创建文件
find / -name "*.log"	查找文件
chmod 755 file	修改权限
chown user:group file	修改所有者
ln -s target link	创建软链接
tar -czvf a.tar.gz dir/	压缩
tar -xzvf a.tar.gz	解压
du -sh /path	目录大小
df -h	磁盘使用
cat / less / head / tail	查看文件
17.2 系统管理
命令	用途
systemctl start/stop/restart/status	服务管理
systemctl enable/disable	开机自启
journalctl -u service	查看日志
ps aux	查看进程
top / htop	进程监控
kill PID	杀死进程
free -h	内存使用
uptime	负载
uname -a	系统信息
hostnamectl	主机名
timedatectl	时间时区
ulimit -a	资源限制
crontab -e	计划任务
17.3 网络命令
命令	用途
ip addr show	IP 地址
ip route show	路由表
ping host	测试连通性
traceroute host	路由追踪
ss -tlnp	监听端口
curl url	HTTP 请求
wget url	下载文件
ssh user@host	远程连接
scp src dst	文件传输
nslookup domain	DNS 查询
tcpdump	抓包
iptables -L	防火墙规则
17.4 文本处理
命令	用途
grep pattern file	搜索
sed 's/old/new/g' file	替换
awk '{print $1}' file	列提取
sort file	排序
uniq -c	去重统计
wc -l file	行数
cut -d: -f1 file	切割
tr 'a-z' 'A-Z'	字符转换
xargs command	参数传递
tee file	输出到文件和屏幕
---
附录
A. 常用配置文件
/etc/hostname          主机名
/etc/hosts             主机名解析
/etc/resolv.conf       DNS 配置
/etc/fstab             文件系统挂载
/etc/passwd            用户信息
/etc/shadow            密码信息
/etc/group             组信息
/etc/sudoers           sudo 配置
/etc/ssh/sshd_config   SSH 服务端配置
/etc/sysctl.conf       内核参数
/etc/security/limits.conf  资源限制
/etc/crontab           计划任务
/etc/logrotate.conf    日志轮转
/etc/yum.repos.d/      YUM 仓库配置
/etc/apt/sources.list  APT 仓库配置
B. 故障排查清单
系统问题：
□ 检查磁盘空间：df -h
□ 检查 inode：df -i
□ 检查内存：free -h
□ 检查负载：uptime
□ 检查进程：ps aux
□ 检查日志：journalctl -xe
□ 检查服务状态：systemctl status
□ 检查网络：ip addr, ping
□ 检查防火墙：iptables -L
□ 检查 SELinux：getenforce
---
📌 提示：Linux 命令众多，建议熟练掌握常用命令，遇到问题时善用 man command 或 command --help 查看帮助。
📌 建议：生产环境操作前做好备份，使用 screen 或 tmux 防止 SSH 断开导致操作中断。
