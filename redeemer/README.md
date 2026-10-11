# Redeemer Write-up | 报告

<details>
  <summary>Click to view in Chinese (点击查看中文版)</summary>

---

这是我对 [Redeemer](https://app.hackthebox.com/machines/Redeemer) 实验的write-up。

# 概述

这个实验是关于 Redis 数据库的。它包含 10 个问题，我们需要获取flag。

> Redis（Remote Dictionary Server，远程字典服务器）是一种内存中的键值数据库，用作分布式缓存和消息代理，并具有可选的持久化功能。由于它将所有数据保存在内存中，并且由于其设计，Redis 提供低延迟的读写，使其特别适合需要缓存的用例。

# 问题与答案

### 1. 机器上开放的是哪个 TCP 端口？

⎯ `6379`。我用 Nmap 工具扫描了机器IP：

> Nmap（Network Mapper）是一款免费、开源的网络扫描工具，用于主机发现、端口扫描、服务检测、操作系统指纹识别和安全审计。

```
sudo nmap -sC -sV -p- --min-rate 5000 10.129.225.227
```

> 在这里，我们使用 `-p-` 参数来获取端口信息，为了让它稍微快一点，我添加了 `--min-rate 5000`

<img width="1254" height="445" alt="image" src="https://github.com/user-attachments/assets/56a171b2-edc9-4b6b-a5db-ee3bad85dea8" />

我们可以看到有一个开放的 TCP 端口：`6379`，它运行着 `redis` 服务。

### 2. 机器上开放的端口运行着什么服务？

⎯ `redis`

### 3. Redis 是什么类型的数据库？从以下选项中选择：（i）内存数据库，（ii）传统数据库

⎯ 内存数据库。

> Redis（Remote Dictionary Server）是一种**内存中的**键值数据库，用作……

### 4. 哪个命令行实用程序用于与 Redis 服务器交互？输入你在终端中不带任何参数输入的程序名称。

⎯ `redis-cli`

我默认没有它，所以我通过运行 `sudo apt install redis-tools` 下载了它。

### 5. Redis 命令行实用程序中使用哪个参数来指定主机名？

⎯ `-h`

在这个问题之后，我尝试用刚学到的知识自己连接到数据库。我通过运行以下命令连接：

```
redis-cli -h machine_ip
```

### 6. 一旦连接到 Redis 服务器，使用哪个命令来获取有关 Redis 服务器的信息和统计信息？

⎯ `info`

起初，我不知道如何在那里定位，因为它不像基本 shell 那样构建。所以我尝试即兴发挥：

<img width="656" height="631" alt="image" src="https://github.com/user-attachments/assets/c0ae1383-de42-430d-8e7c-68d880f43e87" />

<img width="991" height="879" alt="image" src="https://github.com/user-attachments/assets/e7ed2473-9262-4d1c-9b20-1d1348d514be" />

我使用 `help @server` 获取命令列表。在这里我找到了 `INFO` 命令及其描述：

<img width="578" height="71" alt="image" src="https://github.com/user-attachments/assets/f94afc87-7b06-452d-93c7-ea12a666bca1" />

### 7. 目标机器上使用的 Redis 服务器版本是什么？

⎯ `5.0.7`。我运行了 `info` 命令并得到了这个输出：

<img width="283" height="76" alt="image" src="https://github.com/user-attachments/assets/64537ab7-c3ab-402a-83f8-68ba1ad381ef" />

### 8. 在 Redis 中使用哪个命令来选择所需的数据库？

⎯ `select`

### 9. 索引为 0 的数据库中有多少个键？

⎯ 4 个。在 `info` 命令输出的末尾，我们有 `# Keyspace`，在那里我们可以看到所有键的信息：

<img width="322" height="75" alt="image" src="https://github.com/user-attachments/assets/c9fe1c85-bc8d-41a3-9c97-bcff83ac469e" />

`db0` 代表"数据库 0"（0 是默认值）

### 10. 使用哪个命令来获取数据库中的所有键？

⎯ `keys *`

<img width="297" height="126" alt="image" src="https://github.com/user-attachments/assets/969bbaca-b76d-4b52-8295-f4bd112bb64e" />

### 提交flag

⎯ 在 `keys *` 命令输出中你可以看到"flag"键。通过运行 `get` 命令读取它。在我们的例子中是：`get flag`。

## 经验教训

很好的实验，用于提升你在不同系统之间导航的技能。

</details>

---

This is my write-up for the [Redeemer](https://app.hackthebox.com/machines/Redeemer) lab. 

# Overview

This lab is about Redis database. It includes 10 questions and we need to obtain the flag.

> Redis (Remote Dictionary Server) is an in-memory key–value database, used as a distributed cache and message broker, with optional durability. Because it holds all data in memory and because of its design, Redis offers low-latency reads and writes, making it particularly suitable for use cases that require a cache.

# Questions & Answers

### 1. Which TCP port is open on the machine?

⎯ `6379`. I scanned machine IP with Nmap tool:

> Nmap (Network Mapper) is a free, open-source network scanning tool used for host discovery, port scanning, service detection, operating system fingerprinting, and security auditing.

```
sudo nmap -sC -sV -p- --min-rate 5000 10.129.225.227
```

> Here, we use `-p-` flag to get ports info, and to make it a little faster I added `--min-rate 5000`

<img width="1254" height="445" alt="image" src="https://github.com/user-attachments/assets/56a171b2-edc9-4b6b-a5db-ee3bad85dea8" />

And we can see that there is one open TCP port: `6379`, which is running `redis` service.

### 2. Which service is running on the port that is open on the machine?

⎯ `redis`

### 3. What type of database is Redis? Choose from the following options: (i) In-memory Database, (ii) Traditional Database

⎯ In-memory Database.

> Redis (Remote Dictionary Server) is an **in-memory** key–value database, used as...

### 4. Which command-line utility is used to interact with the Redis server? Enter the program name you would enter into the terminal without any arguments.

⎯ `redis-cli`

I didn't had it by default, so I downloaded it by running `sudo apt install redis-tools`

### 5. Which flag is used with the Redis command-line utility to specify the hostname?

⎯ `-h`

After this question I tried to connect to the database by myself, using knowledge I just learnt. And I connected by running:

```
redis-cli -h machine_ip
```

### 6. Once connected to a Redis server, which command is used to obtain the information and statistics about the Redis server?

⎯ `info`

At first, I didn't know how to locate there because it is not built like basic shell.  So I tried improvizing:

<img width="656" height="631" alt="image" src="https://github.com/user-attachments/assets/c0ae1383-de42-430d-8e7c-68d880f43e87" />

<img width="991" height="879" alt="image" src="https://github.com/user-attachments/assets/e7ed2473-9262-4d1c-9b20-1d1348d514be" />

I used `help @server` to get a list of commands. And here I found `INFO` command with description:

<img width="578" height="71" alt="image" src="https://github.com/user-attachments/assets/f94afc87-7b06-452d-93c7-ea12a666bca1" />

### 7. What is the version of the Redis server being used on the target machine?

⎯ `5.0.7`. I ran `info` command and got this output:

<img width="283" height="76" alt="image" src="https://github.com/user-attachments/assets/64537ab7-c3ab-402a-83f8-68ba1ad381ef" />

### 8. Which command is used to select the desired database in Redis?

⎯ `select`

### 9. How many keys are present inside the database with index 0?

⎯ 4. At the end of the `info` command output, we have `# Keyspace` where we see all the keys info:

<img width="322" height="75" alt="image" src="https://github.com/user-attachments/assets/c9fe1c85-bc8d-41a3-9c97-bcff83ac469e" />

`db0` stands for the "database 0" (0 is default) 

### 10. Which command is used to obtain all the keys in a database?

⎯ `keys *`

<img width="297" height="126" alt="image" src="https://github.com/user-attachments/assets/969bbaca-b76d-4b52-8295-f4bd112bb64e" />

### Submit the flag

⎯ In the `keys *` command output you can see "`flag`" key. Read it by running `run` command. In our case it's: `get flag`.

## Lessons Learned

Good lab for improving your skills in navigation between different systems.
