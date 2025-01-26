# Passwd, Shadow & Opasswd
- [Passwd, Shadow \& Opasswd](#passwd-shadow--opasswd)
- [**Passwd File**](#passwd-file)
    - [**Passwd Format**](#passwd-format)
    - [**Editing /etc/passwd - Before**](#editing-etcpasswd---before)
    - [**Editing /etc/passwd - After**](#editing-etcpasswd---after)
    - [**Root without Password**](#root-without-password)
  - [**Shadow File**](#shadow-file)
    - [**Shadow Format**](#shadow-format)
    - [**Algorithm Types**](#algorithm-types)
  - [**Opasswd**](#opasswd)
    - [**Reading /etc/security/opasswd**](#reading-etcsecurityopasswd)
  - [**Cracking Linux Credentials**](#cracking-linux-credentials)
    - [**Unshadow**](#unshadow)
    - [**Hashcat - Cracking Unshadowed Hashes**](#hashcat---cracking-unshadowed-hashes)
    - [**Hashcat - Cracking MD5 Hashes**](#hashcat---cracking-md5-hashes)

Linux-based distributions can use many different authentication mechanisms. One of the most commonly used and standard mechanisms is [Pluggable Authentication Modules](https://web.archive.org/web/20220622215926/http://www.linux-pam.org/Linux-PAM-html/Linux-PAM_SAG.html) (`PAM`). The modules used for this are called `pam_unix.so` or `pam_unix2.so` and are located in `/usr/lib/x86_x64-linux-gnu/security/` in Debian based distributions. These modules manage user information, authentication, sessions, current passwords, and old passwords. For example, if we want to change the password of our account on the Linux system with `passwd`, PAM is called, which takes the appropriate precautions and stores and handles the information accordingly.

The `pam_unix.so` standard module for management uses standardized API calls from the system libraries and files to update the account information. The standard files that are read, managed, and updated are `/etc/passwd` and `/etc/shadow`. PAM also has many other service modules, such as LDAP, mount, or Kerberos.

---

# **Passwd File**

The `/etc/passwd` file contains information about every existing user on the system and can be read by all users and services. Each entry in the `/etc/passwd` file identifies a user on the system. Each entry has seven fields containing a form of a database with information about the particular user, where a colon (`:`) separates the information. Accordingly, such an entry may look something like this:

### **Passwd Format**

| **`cry0l1t3`** | **`:`** | **`x`** | **`:`** | **`1000`** | **`:`** | **`1000`** | **`:`** | **`cry0l1t3,,,`** | **`:`** | **`/home/cry0l1t3`** | **`:`** | **`/bin/bash`** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Login name |  | Password info |  | UID |  | GUID |  | Full name/comments |  | Home directory |  | Shell |

The most interesting field for us is the Password information field in this section because there can be different entries here. One of the rarest cases that we may find only on very old systems is the hash of the encrypted password in this field. Modern systems have the hash values stored in the `/etc/shadow` file, which we will come back to later. Nevertheless, `/etc/passwd` is readable system-wide, giving attackers the possibility to crack the passwords if hashes are stored here.

Usually, we find the value `x` in this field, which means that the passwords are stored in an encrypted form in the `/etc/shadow` file. However, it can also be that the `/etc/passwd` file is writeable by mistake. This would allow us to clear this field for the user `root` so that the password info field is empty. This will cause the system not to send a password prompt when a user tries to log in as `root`.

### **Editing /etc/passwd - Before**

```
root:x:0:0:root:/root:/bin/bash
```

### **Editing /etc/passwd - After**

```
root::0:0:root:/root:/bin/bash
```

### **Root without Password**

```
[cry0l1t3@parrot]─[~]$ head -n 1 /etc/passwd

root::0:0:root:/root:/bin/bash

[cry0l1t3@parrot]─[~]$ su

[root@parrot]─[/home/cry0l1t3]#
```

Even though the cases shown will rarely occur, we should still pay attention and watch for security gaps because there are applications that require us to set specific permissions for entire folders. If the administrator has little experience with Linux or the applications and their dependencies, the administrator may give write permissions to the `/etc` directory and forget to correct them.

---

## **Shadow File**

Since reading the password hash values can put the entire system in danger, the file `/etc/shadow` was developed, which has a similar format to `/etc/passwd` but is only responsible for passwords and their management. It contains all the password information for the created users. For example, if there is no entry in the `/etc/shadow` file for a user in `/etc/passwd`, the user is considered invalid. The `/etc/shadow` file is also only readable by users who have administrator rights. The format of this file is divided into `nine fields`:

### **Shadow Format**

| **`cry0l1t3`** | **`:`** | **`$6$wBRzy$...SNIP...x9cDWUxW1`** | **`:`** | **`18937`** | **`:`** | **`0`** | **`:`** | **`99999`** | **`:`** | **`7`** | **`:`** | **`:`** | **`:`** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Username |  | Encrypted password |  | Last PW change |  | Min. PW age |  | Max. PW age |  | Warning period | Inactivity period | Expiration date | Unused |

```
[cry0l1t3@parrot]─[~]$ sudo cat /etc/shadow

root:*:18747:0:99999:7:::
sys:!:18747:0:99999:7:::
...SNIP...
cry0l1t3:$6$wBRzy$...SNIP...x9cDWUxW1:18937:0:99999:7:::
```

If the password field contains a character, such as `!` or `*`, the user cannot log in with a Unix password. However, other authentication methods for logging in, such as Kerberos or key-based authentication, can still be used. The same case applies if the `encrypted password` field is empty. This means that no password is required for the login. However, it can lead to specific programs denying access to functions. The `encrypted password` also has a particular format by which we can also find out some information:

- `$<type>$<salt>$<hashed>`

As we can see here, the encrypted passwords are divided into three parts. The types of encryption allow us to distinguish between the following:

### **Algorithm Types**

- `$1$` – MD5
- `$2a$` – Blowfish
- `$2y$` – Eksblowfish
- `$5$` – SHA-256
- `$6$` – SHA-512

By default, the SHA-512 (`$6$`) encryption method is used on the latest Linux distributions. We will also find the other encryption methods that we can then try to crack on older systems. We will discuss how the cracking works in a bit.

---

## **Opasswd**

The PAM library (`pam_unix.so`) can prevent reusing old passwords. The file where old passwords are stored is the `/etc/security/opasswd`. Administrator/root permissions are also required to read the file if the permissions for this file have not been changed manually.

### **Reading /etc/security/opasswd**

```
th1nyunb0y@htb[/htb]$ sudo cat /etc/security/opasswd

cry0l1t3:1000:2:$1$HjFAfYTG$qNDkF0zJ3v8ylCOrKB0kt0,$1$kcUjWZJX$E9uMSmiQeRh4pAAgzuvkq1
```

Looking at the contents of this file, we can see that it contains several entries for the user `cry0l1t3`, separated by a comma (`,`). Another critical point to pay attention to is the hashing type that has been used. This is because the `MD5` (`$1$`) algorithm is much easier to crack than SHA-512. This is especially important for identifying old passwords and maybe even their pattern because they are often used across several services or applications. We increase the probability of guessing the correct password many times over based on its pattern.

---

## **Cracking Linux Credentials**

Once we have collected some hashes, we can try to crack them in different ways to get the passwords in cleartext.

### **Unshadow**

```
th1nyunb0y@htb[/htb]$ sudo cp /etc/passwd /tmp/passwd.bak 
th1nyunb0y@htb[/htb]$ sudo cp /etc/shadow /tmp/shadow.bak 
th1nyunb0y@htb[/htb]$ unshadow /tmp/passwd.bak /tmp/shadow.bak > /tmp/unshadowed.hashes
```

### **Hashcat - Cracking Unshadowed Hashes**

```
th1nyunb0y@htb[/htb]$ hashcat -m 1800 -a 0 /tmp/unshadowed.hashes rockyou.txt -o /tmp/unshadowed.cracked
```

### **Hashcat - Cracking MD5 Hashes**

```
th1nyunb0y@htb[/htb]$ cat md5-hashes.list

qNDkF0zJ3v8ylCOrKB0kt0
E9uMSmiQeRh4pAAgzuvkq1
```

```
th1nyunb0y@htb[/htb]$ hashcat -m 500 -a 0 md5-hashes.list rockyou.txt
```