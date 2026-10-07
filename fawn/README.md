# Fawn Write-up | 报告
<details>
  <summary>Click to view in Chinese (点击查看中文版)</summary>

---



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
