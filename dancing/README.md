# Dancing Write-up | 报告
<details>
  <summary>Click to view in Chinese (点击查看中文版)</summary>

---

这是我对 [Dancing](https://app.hackthebox.com/machines/Dancing) 实验的write-up。

# 概述

这个实验是关于 SMB——服务器消息块的。它包含 7 个问题，我们必须提交flag。

> SMB（服务器消息块）是一种客户端-服务器网络协议，主要用于在网络上共享文件、打印机和串行端口。

# 问题与答案

### 1. 三字母缩写 SMB 代表什么？

⎯ Server Message Block（服务器消息块）。

### 2. SMB 使用哪个端口来运行？

⎯ SMB 在 TCP 445 端口上运行（SMB v3.0+）

### 3. 在我们的 Nmap 扫描中出现的 445 端口的服务名称是什么？

⎯ ``microsoft-ds``。我用 Nmap 工具扫描了机器IP：

> Nmap（Network Mapper）是一款免费、开源的网络扫描工具，用于主机发现、端口扫描、服务检测、操作系统指纹识别和安全审计。

```
sudo nmap -sC -sV -O -p 445 -Pn machine_ip
```

<img width="1917" height="548" alt="image" src="https://github.com/user-attachments/assets/ca7154b1-e1e8-47c2-a563-f2b49a776f29" />

你可以在输出的开头看到 PORT、STATE、SERVICE 和 VERSION。服务名称是 ``microsoft-ds``。

### 4. 我们可以用 smbclient 实用程序的什么 'flag' 或 'switch' 来 '列出' Dancing 上可用的 SMB 共享？

⎯ ``-L``。你可以使用 ``smbclient`` 实用程序获取一些关于 SMB 开放端口的信息。要查找可用的 SMB 共享，使用这个命令：

```
smbclient -L machine_ip
```

它会要求你输入密码，但你可以直接按 Enter。

<img width="528" height="207" alt="image" src="https://github.com/user-attachments/assets/65c9c374-17c4-486c-9045-1c8867364b22" />

### 5. Dancing 上有多少个共享？

⎯ 4 个。``ADMIN$``、``C$``、``IPC$``、``WorkShares``。

### 6. 我们最终能够用空密码访问的共享名称是什么？

⎯ ``WorkShares``。它也没有注释。

### 7. 我们可以在 SMB shell 中使用什么命令来下载我们找到的文件？

⎯ ``get``。要检查它，我们可以通过 ``smbclient`` 实用程序使用 SMB shell 连接到这个 SMB 端口。方法如下：

```
smbclient //machine_ip/ShareName -U username
```

> 在这里，把"ShareName"改成你要使用的名称（不要改变/替换"username"）。在我们的例子中，它是"WorkShares"，所以命令将是这样：

```
smbclient //machine_ip/WorkShares -U username
```

<img width="837" height="299" alt="image" src="https://github.com/user-attachments/assets/5a837719-6a19-4c9a-a9dd-98d77aaf3dff" />

然后我们进去了。要检查我们可以使用哪些命令，只需输入 ``?`` 并按 Enter。

<img width="793" height="438" alt="image" src="https://github.com/user-attachments/assets/3c36dddc-75ff-43b9-ae28-1c757e95f57d" />

你会看到所有可用命令的列表。其中一个是 ``get``，用于下载文件。

### 提交位于 SMB 共享上的 flag。
⎯ 我在 ``James.P\`` 目录中找到了 ``flag.txt``。

<img width="1117" height="210" alt="image" src="https://github.com/user-attachments/assets/51cb1b86-c47c-4304-bc30-e5e125b2f16f" />

## 经验教训

非常简单的房间，用于认识 SMB 协议、获取共享名称的信息以及获得 SMB shell。

</details>

---

This is my write-up for the [Dancing](https://app.hackthebox.com/machines/Dancing) lab. 

# Overview

This lab is about SMB - Server Message Block. It includes 7 questions and we have to submit the flag.

> SMB (Server Message Block) is a client-server network protocol primarily used for sharing files, printers, and serial ports across a network.

# Questions & Answers

### 1. What does the 3-letter acronym SMB stand for?

⎯ Server Message Block.

### 2. What port does SMB use to operate at?

⎯ SMB operates on TCP port 445 (SMB v3.0+)

### 3. What is the service name for port 445 that came up in our Nmap scan?

⎯ ``microsoft-ds``. I scanned machine IP with Nmap tool:

> Nmap (Network Mapper) is a free, open-source network scanning tool used for host discovery, port scanning, service detection, operating system fingerprinting, and security auditing.

```
sudo nmap -sC -sV -O -p 445 -Pn machine_ip
```

<img width="1917" height="548" alt="image" src="https://github.com/user-attachments/assets/ca7154b1-e1e8-47c2-a563-f2b49a776f29" />

Here you can see PORT, STATE, SERVICE and VERSION at the beginning of the output. The service name is ``microsoft-ds``.

### 4. What is the 'flag' or 'switch' that we can use with the smbclient utility to 'list' the available SMB shares on Dancing?

⎯ ``-L``. You can use the ``smbclient`` utility to get some info about SMB open ports. To find the available SMB shares, use this command:

```
smbclient -L machine_ip
```

It will ask you for a password, but you can just press Enter.

<img width="528" height="207" alt="image" src="https://github.com/user-attachments/assets/65c9c374-17c4-486c-9045-1c8867364b22" />

### 5. How many shares are there on Dancing?

⎯ 4. ``ADMIN$``, ``C$``, ``IPC$``, ``WorkShares``.

### 6. What is the name of the share we are able to access in the end with a blank password?

⎯ ``WorkShares``. It also have no comment.

### 7. What is the command we can use within the SMB shell to download the files we find?

⎯ ``get``. To check it, we can connect to this SMB port through SMB shell using ``smbclient`` utility. Here's how:

```
smbclient //machine_ip/ShareName -U username
```
> Here, change the "ShareName" with the name that you're going to use (don't change/replace the "username"). In our case, its "WorkShares", so the command is going to be like this:

```
smbclient //machine_ip/WorkShares -U username
```

<img width="837" height="299" alt="image" src="https://github.com/user-attachments/assets/5a837719-6a19-4c9a-a9dd-98d77aaf3dff" />

And we're in. To check what commands we can use, just type ``?`` and hit Enter.

<img width="793" height="438" alt="image" src="https://github.com/user-attachments/assets/3c36dddc-75ff-43b9-ae28-1c757e95f57d" />

You'll see the list of all available commands. One of them is ``get``, used to download the files.

### Submit the flag located on the SMB share.
⎯ I found the ``flag.txt`` in ``James.P\`` directory.

<img width="1117" height="210" alt="image" src="https://github.com/user-attachments/assets/51cb1b86-c47c-4304-bc30-e5e125b2f16f" />

## Lessons Learned

Very easy room to meet the SMB protocol, get the info of shared names and gaining SMB shell.
