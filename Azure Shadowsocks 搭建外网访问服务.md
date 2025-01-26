# Azure Shadowsocks 搭建外网访问服务

## 1. 什么是Azure Shadowsocks？
Azure Shadowsocks是一种基于Shadowsocks协议的代理服务，它在微软的Azure云平台上运行。Shadowsocks被广泛用于翻越网络限制和保护用户的网络隐私。通过在Azure上搭建Shadowsocks服务器，用户可以享受到更高的网络速度和更好的稳定性。

### 1). Azure Shadowsocks的工作原理
Azure Shadowsocks利用客户端和服务器之间的加密连接来确保数据的安全传输。当用户访问被封锁的网站时，Shadowsocks客户端将数据请求发送到Azure上的Shadowsocks服务器，服务器再将请求转发到目标网站，最终将结果返回给客户端。这种方式能够有效隐藏用户的真实IP地址，并避免审查和干扰。
![image](https://github.com/user-attachments/assets/dca4ce6d-2376-4ab7-a454-d49d5f6e1931)

### 2). Azure Shadowsocks的优势
使用Azure Shadowsocks有以下几个优势：
- 高稳定性：Azure提供的基础设施可靠且高效。
- 优质速度：由于使用的是云服务器，网络速度相对较快。
- 安全性：通过加密技术，用户的个人信息和数据更加安全。
- 跨平台支持：支持多种设备和操作系统的客户端。

### 3). Azure Shadowsocks可以用来做什么？
Azure Shadowsocks可用于：
- 翻越地理限制，访问被封锁的网站。
- 保护个人隐私，隐藏真实IP地址。
- 进行网络测试和流量分析。

## 2. Azure Shadowsocks服务器配置

### 1). 注册Azure账户
在开始之前，您需要访问Azure官网并注册一个Azure账户。通常，Azure会提供一定的免费额度供新用户使用。

### 2). 创建虚拟机
#### A. 选择操作系统(Ubuntu)
- 登录Azure门户。点击“创建资源”，选择“计算”，然后选择“虚拟机”。
- 根据需要选择合适的操作系统（推荐使用Ubuntu或Debian）。 配置虚拟机的基本信息，如名称、地区、大小等。
![image](https://github.com/user-attachments/assets/b9021017-8d50-496c-b798-32deb9e8f1ee)

#### B. 选择SSH端口(22), 等待Azure为您部署虚拟机。
![image](https://github.com/user-attachments/assets/028f6cc5-216d-48d0-afd1-334e8a6668a7)

#### C. 创建完成后，手动修改为 8388.
![image](https://github.com/user-attachments/assets/3f339a03-bb24-49d3-9348-5c258de5d918)

#### D. 添加Network Rule, 把8388端口对外开放
![image](https://github.com/user-attachments/assets/8fb23a4f-8989-421d-871e-46929da87578)


### 3). 安装Shadowsocks包

- 连接到虚拟机：使用SSH工具连接到刚刚创建的虚拟机。（如上图：通过 SSH using Azure CLI 连接到虚拟机）
- 更新系统：运行 sudo apt-get update
- 安装Shadowsocks：使用命令 sudo apt-get install shadowsocks-libev 安装Shadowsocks。
- 配置Shadowsocks：编辑配置文件，设置服务器端口、密码及加密方式。
执行：sudo vim /etc/shadowsocks-libev/config.json

```json
{
    "server":["::", "0.0.0.0"],
    "mode":"tcp_and_udp",
    "server_port":8388,
    "local_port":1080,
    "password":"your password",
    "timeout":86400,
    "method":"aes-256-gcm"
}
```
其中：8388为对外的端口，aes-256-gcm 是 Windows/MacOS 都支持的加密方法。更改其中的 password 值

- 启动Shadowsocks服务：使用命令 sudo systemctl start shadowsocks-libev 启动服务。
- 查看Shadowsocks状态：使用命令 sudo systemctl status shadowsocks-libev 。
其中8388端口在被监听中
![image](https://github.com/user-attachments/assets/de8b4ee5-c0cf-456e-acfa-715e487fb677)


## 3. Shadowsocks客户端配置
在完成Azure Shadowsocks服务器的搭建后，接下来需要在您的设备上配置Shadowsocks客户端：
- 下载并安装[Shadowsocks客户端](https://www.tang-seo.com/2370.html)（适用于Windows、macOS、Android、iOS等）。
或者也可通过百度网盘下载 [Shadowsocks客户端](https://www.tang-seo.com/2370.html)（使用于Windows、macOS）
- 打开客户端，输入服务器地址(ip)、端口、密码和加密方式。
![image](https://github.com/user-attachments/assets/cc33b47a-6c7e-41ba-b4ed-0a32cddf4bc5)
- 保存配置并连接。
- 确认连接成功后，您可以访问被限制的网站。（如 google.com）

## 参考
- 1. [如何使用Azure Shadowsocks进行安全翻墙](https://clashhk.com/27388.html)
- 2. [Shadowsocks影梭各平台(win、mac、Android)客户端下载和使用教程](https://www.tang-seo.com/2370.html)
