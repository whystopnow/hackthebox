# Dancing Write-up | 报告
<details>
  <summary>Click to view in Chinese (点击查看中文版)</summary>

---



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
