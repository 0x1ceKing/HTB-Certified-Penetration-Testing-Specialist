# John The Ripper
- [John The Ripper](#john-the-ripper)
	- [**Encryption Technologies**](#encryption-technologies)
	- [**Attack Methods**](#attack-methods)
		- [**Dictionary Attacks**](#dictionary-attacks)
		- [**Brute Force Attacks**](#brute-force-attacks)
		- [**Rainbow Table Attacks**](#rainbow-table-attacks)
	- [**Cracking Modes**](#cracking-modes)
		- [**Single Crack Mode**](#single-crack-mode)
		- [**Cracking with John**](#cracking-with-john)
		- [**Wordlist Mode**](#wordlist-mode)
		- [**Incremental Mode**](#incremental-mode)
		- [**Incremental Mode in John**](#incremental-mode-in-john)
- [**Cracking Files**](#cracking-files)
		- [**Cracking Files with John**](#cracking-files-with-john)

[John the Ripper](https://github.com/openwall/john) (`JTR` or `john`) is an essential pentesting tool used to check the strength of passwords and crack encrypted (or hashed) passwords using either brute force or dictionary attacks. It is open-source software initially developed for UNIX-based systems and first released in 1996. It has become a staple of security professionals due to its various capabilities. The "Jumbo" variant is recommended for those in the security field, as it has performance optimizations and additional features such as multilingual word lists and support for 64-bit architectures. This version is more effective in cracking passwords with greater accuracy and speed.

With this, we can use various tools to convert different types of files and hashes into a format that is usable by John. Additionally, the software is regularly updated to keep up with the current security trends and technologies, ensuring user security.

---

## **Encryption Technologies**

| **Encryption Technology** | **Description** |
| --- | --- |
| `UNIX crypt(3)` | Crypt(3) is a traditional UNIX encryption system with a 56-bit key. |
| `Traditional DES-based` | DES-based encryption uses the Data Encryption Standard algorithm to encrypt data. |
| `bigcrypt` | Bigcrypt is an extension of traditional DES-based encryption. It uses a 128-bit key. |
| `BSDI extended DES-based` | BSDI extended DES-based encryption is an extension of the traditional DES-based encryption and uses a 168-bit key. |
| `FreeBSD MD5-based` (Linux & Cisco) | FreeBSD MD5-based encryption uses the MD5 algorithm to encrypt data with a 128-bit key. |
| `OpenBSD Blowfish-based` | OpenBSD Blowfish-based encryption uses the Blowfish algorithm to encrypt data with a 448-bit key. |
| `Kerberos/AFS` | Kerberos and AFS are authentication systems that use encryption to ensure secure entity communication. |
| `Windows LM` | Windows LM encryption uses the Data Encryption Standard algorithm to encrypt data with a 56-bit key. |
| `DES-based tripcodes` | DES-based tripcodes are used to authenticate users based on the Data Encryption Standard algorithm. |
| `SHA-crypt hashes` | SHA-crypt hashes are used to encrypt data with a 256-bit key and are available in newer versions of Fedora and Ubuntu. |
| `SHA-crypt` and `SUNMD5 hashes` (Solaris) | SHA-crypt and SUNMD5 hashes use the SHA-crypt and MD5 algorithms to encrypt data with a 256-bit key and are available in Solaris. |
| `...` | and many more. |

---

## **Attack Methods**

### **Dictionary Attacks**

Dictionary attacks involve using a pre-generated list of words and phrases (known as a dictionary) to attempt to crack a password. This list of words and phrases is often acquired from various sources, such as publicly available dictionaries, leaked passwords, or even purchased from specialized companies. The dictionary is then used to generate a series of strings which are then used to compare against the hashed passwords. If a match is found, the password is cracked, providing an attacker access to the system and the data stored within it. This type of attack is highly effective. Therefore, it is essential to take the necessary steps to ensure that passwords are kept secure, such as using complex and unique passwords, regularly changing them, and using two-factor authentication.

### **Brute Force Attacks**

Brute force attacks involve attempting every conceivable combination of characters that could form a password. This is an extremely slow process, and using this method is typically only advisable if there are no other alternatives. It is also important to note that the longer and more complex the password, the more difficult it is to crack and the longer it will take to exhaust every combination. For this reason, it is highly recommended that passwords be at least 8 characters in length, with a combination of letters, numbers, and symbols.

### **Rainbow Table Attacks**

Rainbow table attacks involve using a pre-computed table of hashes and their corresponding plaintext passwords, which is a much faster method than a brute-force attack. However, this method is limited by the rainbow table size – the larger the table, the more passwords, and hashes it can store. Additionally, due to the nature of the attack, it is impossible to use rainbow tables to determine the plaintext of hashes not already included in the table. As a result, rainbow table attacks are only effective against hashes already present in the table, making the larger the table, the more successful the attack.

---

## **Cracking Modes**

`Single Crack Mode` is one of the most common John modes used when attempting to crack passwords using a single password list. It is a brute-force attack, meaning all passwords on the list are tried, one by one, until the correct one is found. This method is the most basic and straightforward way of cracking passwords and is thus a popular choice for those wishing to gain access to a secure system. It is, however, far from the most efficient method since it can take an indefinite amount of time to crack a password, depending on the length and complexity of the password in question. The basic syntax for the command is:

### **Single Crack Mode**

```
[!bash!]$ john --format=<hash_type> <hash or hash_file>
```

For example, if we have a file named `hashes_to_crack.txt` that contains `SHA-256` hashes, the command to crack them would be:

```
[!bash!]$ john --format=sha256 hashes_to_crack.txt
```

- `john` is the command to run the John the Ripper program
- `-format=sha256` specifies that the hash format is SHA-256
- `hashes.txt` is the file name containing the hashes to be cracked

When we run the command, John will read the hashes from the specified file, and then it will try to crack them by comparing them to the words in its built-in wordlist and any additional wordlists specified with the `--wordlist` option. Additionally, It will use any rules set with the `--rules` option (if any rules are given) to generate further candidate passwords.

The process of cracking the passwords can be `very time-consuming`, as the amount of time required to crack a password depends on multiple factors, such as the complexity of the password, machine configuration, and the size of the wordlist. Cracking passwords is almost a matter of luck. Because the password itself can be elementary, but if we use a wrong list where the word is not present or cannot be generated by John, we will eventually fail to crack the password.

John will output the cracked passwords to the console and the file "john.pot" (`~/.john/john.pot`) to the current user's home directory. Furthermore, it will continue cracking the remaining hashes in the background, and we can check the progress by running the `john --show` command. To maximize the chances of success, it is important to ensure that the wordlists and rules used are comprehensive and up to date.

### **Cracking with John**

| **Hash Format** | **Example Command** | **Description** |
| --- | --- | --- |
| afs | `john --format=afs hashes_to_crack.txt` | AFS (Andrew File System) password hashes |
| bfegg | `john --format=bfegg hashes_to_crack.txt` | bfegg hashes used in Eggdrop IRC bots |
| bf | `john --format=bf hashes_to_crack.txt` | Blowfish-based crypt(3) hashes |
| bsdi | `john --format=bsdi hashes_to_crack.txt` | BSDi crypt(3) hashes |
| crypt(3) | `john --format=crypt hashes_to_crack.txt` | Traditional Unix crypt(3) hashes |
| des | `john --format=des hashes_to_crack.txt` | Traditional DES-based crypt(3) hashes |
| dmd5 | `john --format=dmd5 hashes_to_crack.txt` | DMD5 (Dragonfly BSD MD5) password hashes |
| dominosec | `john --format=dominosec hashes_to_crack.txt` | IBM Lotus Domino 6/7 password hashes |
| EPiServer SID hashes | `john --format=episerver hashes_to_crack.txt` | EPiServer SID (Security Identifier) password hashes |
| hdaa | `john --format=hdaa hashes_to_crack.txt` | hdaa password hashes used in Openwall GNU/Linux |
| hmac-md5 | `john --format=hmac-md5 hashes_to_crack.txt` | hmac-md5 password hashes |
| hmailserver | `john --format=hmailserver hashes_to_crack.txt` | hmailserver password hashes |
| ipb2 | `john --format=ipb2 hashes_to_crack.txt` | Invision Power Board 2 password hashes |
| krb4 | `john --format=krb4 hashes_to_crack.txt` | Kerberos 4 password hashes |
| krb5 | `john --format=krb5 hashes_to_crack.txt` | Kerberos 5 password hashes |
| LM | `john --format=LM hashes_to_crack.txt` | LM (Lan Manager) password hashes |
| lotus5 | `john --format=lotus5 hashes_to_crack.txt` | Lotus Notes/Domino 5 password hashes |
| mscash | `john --format=mscash hashes_to_crack.txt` | MS Cache password hashes |
| mscash2 | `john --format=mscash2 hashes_to_crack.txt` | MS Cache v2 password hashes |
| mschapv2 | `john --format=mschapv2 hashes_to_crack.txt` | MS CHAP v2 password hashes |
| mskrb5 | `john --format=mskrb5 hashes_to_crack.txt` | MS Kerberos 5 password hashes |
| mssql05 | `john --format=mssql05 hashes_to_crack.txt` | MS SQL 2005 password hashes |
| mssql | `john --format=mssql hashes_to_crack.txt` | MS SQL password hashes |
| mysql-fast | `john --format=mysql-fast hashes_to_crack.txt` | MySQL fast password hashes |
| mysql | `john --format=mysql hashes_to_crack.txt` | MySQL password hashes |
| mysql-sha1 | `john --format=mysql-sha1 hashes_to_crack.txt` | MySQL SHA1 password hashes |
| NETLM | `john --format=netlm hashes_to_crack.txt` | NETLM (NT LAN Manager) password hashes |
| NETLMv2 | `john --format=netlmv2 hashes_to_crack.txt` | NETLMv2 (NT LAN Manager version 2) password hashes |
| NETNTLM | `john --format=netntlm hashes_to_crack.txt` | NETNTLM (NT LAN Manager) password hashes |
| NETNTLMv2 | `john --format=netntlmv2 hashes_to_crack.txt` | NETNTLMv2 (NT LAN Manager version 2) password hashes |
| NEThalfLM | `john --format=nethalflm hashes_to_crack.txt` | NEThalfLM (NT LAN Manager) password hashes |
| md5ns | `john --format=md5ns hashes_to_crack.txt` | md5ns (MD5 namespace) password hashes |
| nsldap | `john --format=nsldap hashes_to_crack.txt` | nsldap (OpenLDAP SHA) password hashes |
| ssha | `john --format=ssha hashes_to_crack.txt` | ssha (Salted SHA) password hashes |
| NT | `john --format=nt hashes_to_crack.txt` | NT (Windows NT) password hashes |
| openssha | `john --format=openssha hashes_to_crack.txt` | OPENSSH private key password hashes |
| oracle11 | `john --format=oracle11 hashes_to_crack.txt` | Oracle 11 password hashes |
| oracle | `john --format=oracle hashes_to_crack.txt` | Oracle password hashes |
| pdf | `john --format=pdf hashes_to_crack.txt` | PDF (Portable Document Format) password hashes |
| phpass-md5 | `john --format=phpass-md5 hashes_to_crack.txt` | PHPass-MD5 (Portable PHP password hashing framework) password hashes |
| phps | `john --format=phps hashes_to_crack.txt` | PHPS password hashes |
| pix-md5 | `john --format=pix-md5 hashes_to_crack.txt` | Cisco PIX MD5 password hashes |
| po | `john --format=po hashes_to_crack.txt` | Po (Sybase SQL Anywhere) password hashes |
| rar | `john --format=rar hashes_to_crack.txt` | RAR (WinRAR) password hashes |
| raw-md4 | `john --format=raw-md4 hashes_to_crack.txt` | Raw MD4 password hashes |
| raw-md5 | `john --format=raw-md5 hashes_to_crack.txt` | Raw MD5 password hashes |
| raw-md5-unicode | `john --format=raw-md5-unicode hashes_to_crack.txt` | Raw MD5 Unicode password hashes |
| raw-sha1 | `john --format=raw-sha1 hashes_to_crack.txt` | Raw SHA1 password hashes |
| raw-sha224 | `john --format=raw-sha224 hashes_to_crack.txt` | Raw SHA224 password hashes |
| raw-sha256 | `john --format=raw-sha256 hashes_to_crack.txt` | Raw SHA256 password hashes |
| raw-sha384 | `john --format=raw-sha384 hashes_to_crack.txt` | Raw SHA384 password hashes |
| raw-sha512 | `john --format=raw-sha512 hashes_to_crack.txt` | Raw SHA512 password hashes |
| salted-sha | `john --format=salted-sha hashes_to_crack.txt` | Salted SHA password hashes |
| sapb | `john --format=sapb hashes_to_crack.txt` | SAP CODVN B (BCODE) password hashes |
| sapg | `john --format=sapg hashes_to_crack.txt` | SAP CODVN G (PASSCODE) password hashes |
| sha1-gen | `john --format=sha1-gen hashes_to_crack.txt` | Generic SHA1 password hashes |
| skey | `john --format=skey hashes_to_crack.txt` | S/Key (One-time password) hashes |
| ssh | `john --format=ssh hashes_to_crack.txt` | SSH (Secure Shell) password hashes |
| sybasease | `john --format=sybasease hashes_to_crack.txt` | Sybase ASE password hashes |
| xsha | `john --format=xsha hashes_to_crack.txt` | xsha (Extended SHA) password hashes |
| zip | `john --format=zip hashes_to_crack.txt` | ZIP (WinZip) password hashes |

### **Wordlist Mode**

`Wordlist Mode` is used to crack passwords using multiple lists of words. It is a dictionary attack which means it will try all the words in the lists one by one until it finds the right one. It is generally used for cracking multiple password hashes using a wordlist or a combination of wordlists. It is more effective than Single Crack Mode because it utilizes more words but is still relatively basic. The basic syntax for the command is:

```
[!bash!]$ john --wordlist=<wordlist_file> --rules <hash_file>
```

First, we specify the wordlist file or files to use for cracking the password hashes. The wordlist(s) can be in plain text format, with one word per line. Multiple wordlists can be specified by separating them with a comma. Then we can specify a rule set or apply the built-in mangling rules to the words in the wordlist. These rules generate candidate passwords using transformations such as appending numbers, capitalizing letters and adding special characters.

### **Incremental Mode**

`Incremental Mode` is an advanced John mode used to crack passwords using a character set. It is a hybrid attack, which means it will attempt to match the password by trying all possible combinations of characters from the character set. This mode is the most effective yet most time-consuming of all the John modes. This mode works best when we know what the password might be, as it will try all the possible combinations in sequence, starting from the shortest one. This makes it much faster than the brute force attack, where all combinations are tried randomly. Moreover, the incremental mode can also be used to crack weak passwords, which may be challenging to crack using the standard John modes. The main difference between incremental mode and wordlist mode is the source of the password guesses. Incremental mode generates the guesses on the fly, while wordlist mode uses a predefined list of words. At the same time, the single crack mode is used to check a single password against a hash.

The syntax for running John the Ripper in incremental mode is as follows:

### **Incremental Mode in John**

```
[!bash!]$ john --incremental <hash_file>
```

Using this command we will read the hashes in the specified hash file and then generate all possible combinations of characters, starting with a single character and incrementing with each iteration. It is important to note that this mode is `highly resource intensive` and can take a long time to complete, depending on the complexity of the passwords, machine configuration, and the number of characters set. Additionally, it is important to note that the default character set is limited to `a-zA-Z0-9`. Therefore, if we attempt to crack complex passwords with special characters, we need to use a custom character set.

---

# **Cracking Files**

It is also possible to crack even password-protected or encrypted files with John. We use additional tools that process the given files and produce hashes that John can work with. It automatically detects the formats and tries to crack them. The syntax for this can look like this:

### **Cracking Files with John**

```
cry0l1t3@htb:~$ <tool> <file_to_crack> > file.hash
cry0l1t3@htb:~$ pdf2john server_doc.pdf > server_doc.hash
cry0l1t3@htb:~$ john server_doc.hash
								# OR
cry0l1t3@htb:~$ john --wordlist=<wordlist.txt> server_doc.hash
```

Additionally, we can use different modes for this with our personal wordlists and rules. We have created a list that includes many but not all tools that can be used for John:

| **Tool** | **Description** |
| --- | --- |
| `pdf2john` | Converts PDF documents for John |
| `ssh2john` | Converts SSH private keys for John |
| `mscash2john` | Converts MS Cash hashes for John |
| `keychain2john` | Converts OS X keychain files for John |
| `rar2john` | Converts RAR archives for John |
| `pfx2john` | Converts PKCS#12 files for John |
| `truecrypt_volume2john` | Converts TrueCrypt volumes for John |
| `keepass2john` | Converts KeePass databases for John |
| `vncpcap2john` | Converts VNC PCAP files for John |
| `putty2john` | Converts PuTTY private keys for John |
| `zip2john` | Converts ZIP archives for John |
| `hccap2john` | Converts WPA/WPA2 handshake captures for John |
| `office2john` | Converts MS Office documents for John |
| `wpa2john` | Converts WPA/WPA2 handshakes for John |