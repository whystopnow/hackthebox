# Redeemer Write-up | 报告

<details>
  <summary>Click to view in Chinese (点击查看中文版)</summary>

---



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

⎯ In the `keys *` command output you can see "flag" key. Read it by running `run` command. In our case it's: `get flag`.

## Lessons Learned

Good lab for improving your skills in navigation between different systems.
