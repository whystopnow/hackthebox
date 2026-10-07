# Fawn Write-up | 报告
<details>
  <summary>Click to view in Chinese (点击查看中文版)</summary>

---

这是我对 [Fawn](https://app.hackthebox.com/machines/Fawn) 实验的write-up。

# 概述

这个实验是关于 FTP——文件传输协议的。它包含 11 个问题，我们需要获取 flag.txt。

> 文件传输协议（FTP）是一种标准通信协议，用于通过计算机网络将计算机文件从服务器传输到客户端。

# 问题与答案

### 1. 三字母缩写 FTP 代表什么？

⎯ File Transfer Protocol（文件传输协议）

### 2. FTP 服务通常监听哪个端口？

⎯ 21 端口

### 3. FTP 以明文发送数据，没有任何加密。后来设计的一个协议用于提供与 FTP 类似的功能但更安全，作为 SSH 协议的扩展，它使用什么缩写？

⎯ SFTP。有两种类型的安全 FTP 服务器：SFTP 和 FTPS。为了进行保护用户名和密码并加密内容的安全传输，FTP 通常用 SSL/TLS 保护（FTPS）或替换为 SSH 文件传输协议（SFTP）

### 4. 我们可以使用什么命令发送 ICMP 回显请求来测试与目标的连接？

⎯ ``ping``

### 5. 根据你的扫描，目标上运行的 FTP 是什么版本？

⎯ ``vsftpd 3.0.3``。为了找出来，我用 Nmap 工具扫描了机器IP：

> Nmap（Network Mapper）是一款免费、开源的网络扫描工具，用于主机发现、端口扫描、服务检测、操作系统指纹识别和安全审计。

```
sudo nmap -sC -sV -O -p 21 -Pn machine_ip
```

<img width="1801" height="630" alt="image" src="https://github.com/user-attachments/assets/ed3b4a73-7177-483d-bf21-d1c764e8d9bd" />

你可以在顶部看到 PORT、STATE、SERVICE、VERSION。版本是 ``vsftpd 3.0.3``

### 6. 根据你的扫描，目标上运行的是什么操作系统类型？

⎯ Unix。在 nmap 扫描输出的底部我们可以看到 ``Service Info: OS: Unix``。

<img width="639" height="149" alt="image" src="https://github.com/user-attachments/assets/55c5ffa9-b3fc-4d2a-82d9-864826a303f5" />

### 7. 我们需要运行什么命令来显示 'ftp' 客户端的帮助菜单？

⎯ ``ftp -?``

### 8. 当你想在没有账户的情况下登录时，FTP 上使用的用户名是什么？

⎯ anonymous（匿名）

> 这个功能被称为匿名 FTP，允许在不需要唯一凭据的情况下公开访问文件

我运行了这个命令：

```
ftp anonymous@machine_ip
```

当它要求输入密码时只需按 Enter，你就进去了。（并非所有 FTP 服务器都有这个匿名功能）

<img width="378" height="149" alt="image" src="https://github.com/user-attachments/assets/78d14945-3e6f-4bfd-a1b3-76d20fd9cd09" />

### 9. FTP 消息 'Login successful'（登录成功）我们得到的响应代码是什么？

⎯ ``230``（当我们以 anonymous 身份登录时确实能看到它）

### 10. 我们可以使用几个命令来列出 FTP 服务器上可用的文件和目录。一个是 dir。另一个在 Linux 系统上列出文件的常用方式是什么？

⎯ ``ls``

### 11. 用于下载我们在 FTP 服务器上找到的文件的命令是什么？

⎯ ``get``。如果你在 FTP 服务器中运行 ``ls``，你会看到我们这里有一个 flag：

<img width="647" height="103" alt="image" src="https://github.com/user-attachments/assets/7e9b6a25-61f3-4bae-923b-320ead0dc385" />

但我们不能用 ``cat`` 读取它。所以如果你想看看可以使用哪些命令，输入 ``?`` 并按 Enter。

<img width="1701" height="276" alt="image" src="https://github.com/user-attachments/assets/58f9313b-5b19-45cf-b5ca-dd9a3c03b5de" />

这里是你所有可以使用的命令。``get`` 命令用于下载文件。

### 提交位于 FTP 服务器上的 flag。

⎯ 使用 ``get`` 下载 flag.txt，然后通过 ``exit`` 命令离开 FTP 服务器，并用任何你想要的方式读取 flag。

<img width="647" height="188" alt="image" src="https://github.com/user-attachments/assets/f9ccddc2-ff86-4f77-be90-f4c77475e843" />

## 经验教训

这个实验对于开始学习 FTP 服务器真的很有帮助。

</details>

---

This is my write-up for the [Fawn](https://app.hackthebox.com/machines/Fawn) lab. 

# Overview

This lab is about FTP - File Transfer Protocol.  It includes 11 questions and we need to get the flag.txt.

> The File Transfer Protocol (FTP) is a standard communication protocol used for the transfer of computer files from a server to a client over a computer network.

# Questions & Answers

### 1. What does the 3-letter acronym FTP stand for?

⎯ File Transfer Protocol

### 2. Which port does the FTP service listen on usually?

⎯ Port 21

### 3. FTP sends data in the clear, without any encryption. What acronym is used for a later protocol designed to provide similar functionality to FTP but securely, as an extension of the SSH protocol?

⎯ SFTP. There's two type of secure FTP servers: SFTP & FTPS. For secure transmission that protects the username and password, and encrypts the content, FTP is often secured with SSL/TLS (FTPS) or replaced with SSH File Transfer Protocol (SFTP)

### 4. What is the command we can use to send an ICMP echo request to test our connection to the target?

⎯ ``ping``

### 5. From your scans, what version is FTP running on the target?

⎯ ``vsftpd 3.0.3``. To find it out, I scanned machine IP with Nmap tool:

> Nmap (Network Mapper) is a free, open-source network scanning tool used for host discovery, port scanning, service detection, operating system fingerprinting, and security auditing.

```
sudo nmap -sC -sV -O -p 21 -Pn machine_ip
```

<img width="1801" height="630" alt="image" src="https://github.com/user-attachments/assets/ed3b4a73-7177-483d-bf21-d1c764e8d9bd" />

You can see PORT, STATE, SERVICE, VERSION at the top. The version is ``vsftpd 3.0.3``

### 6. From your scans, what OS type is running on the target?

⎯ Unix. At the bottom of the nmap scan output we can see ``Service Info: OS: Unix``.

<img width="639" height="149" alt="image" src="https://github.com/user-attachments/assets/55c5ffa9-b3fc-4d2a-82d9-864826a303f5" />

### 7. What is the command we need to run in order to display the 'ftp' client help menu?

⎯ ``ftp -?``

### 8. What is username that is used over FTP when you want to log in without having an account?

⎯ anonymous

> This feature, known as anonymous FTP, allows public access to files without requiring unique credentials

I ran this command:

```
ftp anonymous@machine_ip
```

Just press Enter when it asks for a password, and you're in. (Not all FTP servers have this anonymous feature)

<img width="378" height="149" alt="image" src="https://github.com/user-attachments/assets/78d14945-3e6f-4bfd-a1b3-76d20fd9cd09" />

### 9. What is the response code we get for the FTP message 'Login successful'?

⎯ ``230`` (We liretally see it when we login as anonymous)

### 10. There are a couple of commands we can use to list the files and directories available on the FTP server. One is dir. What is the other that is a common way to list files on a Linux system.

⎯ ``ls``

### 11. What is the command used to download the file we found on the FTP server?

⎯ ``get``. If you run ``ls`` in the FTP server, you'll see that we have a flag here:

<img width="647" height="103" alt="image" src="https://github.com/user-attachments/assets/7e9b6a25-61f3-4bae-923b-320ead0dc385" />

But we can't read it with ``cat``. So if you want to see what commands you can use, type ``?`` and hit Enter. 

<img width="1701" height="276" alt="image" src="https://github.com/user-attachments/assets/58f9313b-5b19-45cf-b5ca-dd9a3c03b5de" />

Here's all the commands that you can use.  The ``get`` command is used to download the files.

### Submit the flag located on the FTP server.

⎯ Use ``get`` to download the flag.txt, then leave the FTP server with via ``exit`` command, and read the flag with whatever you want.

<img width="647" height="188" alt="image" src="https://github.com/user-attachments/assets/f9ccddc2-ff86-4f77-be90-f4c77475e843" />

## Lessons Learned

This lab is really helpful to start learning about FTP servers.
