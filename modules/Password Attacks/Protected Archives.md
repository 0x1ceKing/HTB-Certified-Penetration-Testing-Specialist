# Protected Archives
- [Protected Archives](#protected-archives)
    - [**Download All File Extensions**](#download-all-file-extensions)
  - [**Cracking Archives**](#cracking-archives)
  - [**Cracking ZIP**](#cracking-zip)
    - [**Using zip2john**](#using-zip2john)
    - [**Viewing the Contents of zip.hash**](#viewing-the-contents-of-ziphash)
    - [**Cracking the Hash with John**](#cracking-the-hash-with-john)
    - [**Viewing the Cracked Hash**](#viewing-the-cracked-hash)
  - [**Cracking OpenSSL Encrypted Archives**](#cracking-openssl-encrypted-archives)
    - [**Listing the Files**](#listing-the-files)
    - [**Using file**](#using-file)
    - [**Using a for-loop to Display Extracted Contents**](#using-a-for-loop-to-display-extracted-contents)
    - [**Listing the Contents of the Cracked Archive**](#listing-the-contents-of-the-cracked-archive)
  - [**Cracking BitLocker Encrypted Drives**](#cracking-bitlocker-encrypted-drives)
    - [**Using bitlocker2john**](#using-bitlocker2john)
    - [**Using hashcat to Crack backup.hash**](#using-hashcat-to-crack-backuphash)
    - [**Viewing the Cracked Hash**](#viewing-the-cracked-hash-1)
    - [**Windows - Mounting BitLocker VHD**](#windows---mounting-bitlocker-vhd)

Besides standalone files, there is also another format of files that can contain not only data, such as an Office document or a PDF, but also other files within them. This format is called an `archive` or `compressed file` that can be protected with a password if necessary.

Let us assume an employee's role in an administrative company and imagine that our customer wants to summarize analysis in different formats, such as Excel, PDF, Word, and a corresponding presentation. One solution would be to send these files individually, but if we extend this example to a large company dealing with several projects running simultaneously, this type of file transfer can become cumbersome and lead to individual files being lost. In these cases, employees often rely on archives, which allow them to split all the necessary files in a structured way according to the projects (often in subfolders), summarize them, and pack them into a single file.

There are many types of archive files. Some common file extensions include, but are not limited to:

| **`tar`** | **`gz`** | **`rar`** | **`zip`** |
| --- | --- | --- | --- |
| **`vmdb/vmx`** | **`cpt`** | **`truecrypt`** | **`bitlocker`** |
| **`kdbx`** | **`luks`** | **`deb`** | **`7z`** |
| **`pkg`** | **`rpm`** | **`war`** | **`gzip`** |

An extensive list of archive types can be found on [FileInfo.com](https://fileinfo.com/filetypes/compressed). However, instead of manually typing them out, we can also query them using a one-liner, filter them out, and save them to a file if needed. At the time of writing, there are `337`archive file types listed on fileinfo.com.

### **Download All File Extensions**

```
th1nyunb0y@htb[/htb]$ curl -s https://fileinfo.com/filetypes/compressed | html2text | awk '{print tolower($1)}' | grep "\." | tee -a compressed_ext.txt

.mint
.htmi 
.tpsr
.mpkg  
.arduboy
.ice
.sifz 
.fzpz 
.rar     
.comppkg.hauptwerk.rar
...SNIP...
```

It is important to note that not all of the above archives support password protection. Other tools are often used to protect the corresponding archives with a password. For example, with `tar`, the tool `openssl` or `gpg` is used to encrypt the archives.

---

## **Cracking Archives**

Given the number of different archives and the combination of tools, we will show only some of the possible ways to crack specific archives in this section. When it comes to password-protected archives, we typically need certain scripts that allow us to extract the hashes from the protected files and use them to crack the password of those.

The .zip format is often heavily used in Windows environments to compress many files into one file. The procedure we have already seen remains the same except for using a different script to extract the hashes.

---

## **Cracking ZIP**

### **Using zip2john**

```
h1nyunb0y@htb[/htb]$ zip2john ZIP.zip > zip.hash

ver 2.0 efh 5455 efh 7875 ZIP.zip/flag.txt PKZIP Encr: 2b chk, TS_chk, cmplen=42, decmplen=30, crc=490E7510
```

By extracting the hashes, we will also see which files are in the ZIP archive.

### **Viewing the Contents of zip.hash**

```
th1nyunb0y@htb[/htb]$ cat zip.hash 

ZIP.zip/customers.csv:$pkzip2$1*2*2*0*2a*1e*490e7510*0*42*0*2a*490e*409b*ef1e7feb7c1cf701a6ada7132e6a5c6c84c032401536faf7493df0294b0d5afc3464f14ec081cc0e18cb*$/pkzip2$:customers.csv:ZIP.zip::ZIP.zip
```

Once we have extracted the hash, we can now use `john` again to crack it with the desired password list. Because if `john` cracks it successfully, it will show us the corresponding password that we can use to open the ZIP archive.

### **Cracking the Hash with John**

```
th1nyunb0y@htb[/htb]$ john --wordlist=rockyou.txt zip.hash

Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
1234             (ZIP.zip/customers.csv)
1g 0:00:00:00 DONE (2022-02-09 09:18) 100.0g/s 250600p/s 250600c/s 250600C/s 123456..1478963
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

### **Viewing the Cracked Hash**

```
th1nyunb0y@htb[/htb]$ john zip.hash --show

ZIP.zip/customers.csv:1234:customers.csv:ZIP.zip::ZIP.zip

1 password hash cracked, 0 left
```

---

## **Cracking OpenSSL Encrypted Archives**

Furthermore, it is not always directly apparent whether the archive found is password-protected, especially if a file extension is used that does not support password protection. As we have already discussed, `openssl` can be used to encrypt the `gzip` format as an example. Using the tool `file`, we can obtain information about the specified file's format. This could look like this, for example:

### **Listing the Files**

```
th1nyunb0y@htb[/htb]$ ls

GZIP.gzip  rockyou.txt
```

### **Using file**

```
th1nyunb0y@htb[/htb]$ file GZIP.gzip 

GZIP.gzip: openssl enc'd data with salted password
```

When cracking OpenSSL encrypted files and archives, we can encounter many different difficulties that will bring many false positives or even fail to guess the correct password. Therefore, the safest choice for success is to use the `openssl` tool in a `for-loop` that tries to extract the files from the archive directly if the password is guessed correctly.

The following one-liner will show many errors related to the GZIP format, which we can ignore. If we have used the correct password list, as in this example, we will see that we have successfully extracted another file from the archive.

### **Using a for-loop to Display Extracted Contents**

```
th1nyunb0y@htb[/htb]$ for i in $(cat rockyou.txt);do openssl enc -aes-256-cbc -d -in GZIP.gzip -k $i 2>/dev/null| tar xz;done

gzip: stdin: not in gzip format
tar: Child returned status 1
tar: Error is not recoverable: exiting now

gzip: stdin: not in gzip format
tar: Child returned status 1
tar: Error is not recoverable: exiting now

<SNIP>
```

Once the for-loop has finished, we can look in the current folder again to check if the cracking of the archive was successful.

### **Listing the Contents of the Cracked Archive**

```
th1nyunb0y@htb[/htb]$ ls

customers.csv  GZIP.gzip  rockyou.txt
```

---

## **Cracking BitLocker Encrypted Drives**

[BitLocker](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-device-encryption-overview-windows-10) is an encryption program for entire partitions and external drives. Microsoft developed it for the Windows operating system. It has been available since Windows Vista and uses the `AES` encryption algorithm with 128-bit or 256-bit length. If the password or PIN for BitLocker is forgotten, we can use the recovery key to decrypt the partition or drive. The recovery key is a 48-digit string of numbers generated during BitLocker setup that also can be brute-forced.

Virtual drives are often created in which personal information, notes, and documents are stored on the computer or laptop provided by the company to prevent access to this information by third parties. Again, we can use a script called `bitlocker2john` to extract the hash we need to crack. [Four different hashes](https://openwall.info/wiki/john/OpenCL-BitLocker) will be extracted, which can be used with different Hashcat hash modes. For our example, we will work with the first one, which refers to the BitLocker password.

### **Using bitlocker2john**

```
th1nyunb0y@htb[/htb]$ bitlocker2john -i Backup.vhd > backup.hashes
th1nyunb0y@htb[/htb]$ grep "bitlocker\$0" backup.hashes > backup.hash
th1nyunb0y@htb[/htb]$ cat backup.hash

$bitlocker$0$16$02b329c0453b9273f2fc1b927443b5fe$1048576$12$00b0a67f961dd80103000000$60$d59f37e...SNIP...70696f7eab6b
```

Both `John` and `Hashcat` can be used for this purpose. This example will look at the procedure with `Hashcat`. The Hashcat mode for cracking BitLocker hashes is `-m 22100`. So we provide Hashcat with the file with the one hash, specify our password list, and specify the hash mode. Since this is robust encryption (`AES`), cracking can take some time, depending on the hardware used. Additionally, we can specify the filename in which the result should be stored.

### **Using hashcat to Crack backup.hash**

```
th1nyunb0y@htb[/htb]$ hashcat -m 22100 backup.hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt -o backup.cracked

hashcat (v6.1.1) starting...

<SNIP>

$bitlocker$0$16$02b329c0453b9273f2fc1b927443b5fe$1048576$12$00b0a67f961dd80103000000$60$d59f37e70696f7eab6b8f95ae93bd53f3f7067d5e33c0394b3d8e2d1fdb885cb86c1b978f6cc12ed26de0889cd2196b0510bbcd2a8c89187ba8ec54f:1234qwer
                                                 
Session..........: hashcat
Status...........: Cracked
Hash.Name........: BitLocker
Hash.Target......: $bitlocker$0$16$02b329c0453b9273f2fc1b927443b5fe$10...8ec54f
Time.Started.....: Wed Feb  9 11:46:40 2022 (1 min, 42 secs)
Time.Estimated...: Wed Feb  9 11:48:22 2022 (0 secs)
Guess.Base.......: File (/opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:       28 H/s (8.79ms) @ Accel:32 Loops:4096 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests
Progress.........: 2880/6163 (46.73%)
Rejected.........: 0/2880 (0.00%)
Restore.Point....: 2816/6163 (45.69%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:1044480-1048576
Candidates.#1....: chemical -> secrets

Started: Wed Feb  9 11:46:35 2022
Stopped: Wed Feb  9 11:48:23 2022
```

### **Viewing the Cracked Hash**

Protected Archives

```
th1nyunb0y@htb[/htb]$ cat backup.cracked 

$bitlocker$0$16$02b329c0453b9273f2fc1b927443b5fe$1048576$12$00b0a67f961dd80103000000$60$d59f37e70696f7eab6b8f95ae93bd53f3f7067d5e33c0394b3d8e2d1fdb885cb86c1b978f6cc12ed26de0889cd2196b0510bbcd2a8c89187ba8ec54f:1234qwer
```

Once we have cracked the password, we will be able to open the encrypted drives. The easiest way to mount a BitLocker encrypted virtual drive is to transfer it to a Windows system and mount it. To do this, we only have to double-click on the virtual drive. Since it is password protected, Windows will show us an error. After mounting, we can again double-click BitLocker to prompt us for the password.

### **Windows - Mounting BitLocker VHD**

![https://academy.hackthebox.com/storage/modules/147/bitlocker.png](https://academy.hackthebox.com/storage/modules/147/bitlocker.png)