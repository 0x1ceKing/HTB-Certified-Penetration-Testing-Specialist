# Password Reuse / Default Passwords
- [Password Reuse / Default Passwords](#password-reuse--default-passwords)
  - [**Credential Stuffing**](#credential-stuffing)
    - [**Credential Stuffing - Hydra Syntax**](#credential-stuffing---hydra-syntax)
    - [**Credential Stuffing - Hydra**](#credential-stuffing---hydra)
    - [**Google Search - Default Credentials**](#google-search---default-credentials)

## **Credential Stuffing**

There are various databases that keep a running list of known default credentials. One of them is the [DefaultCreds-Cheat-Sheet](https://github.com/ihebski/DefaultCreds-cheat-sheet). Here is a small excerpt from the entire table of this cheat sheet:

| **Product/Vendor** | **Username** | **Password** |
| --- | --- | --- |
| Zyxel (ssh) | zyfwp | PrOw!aN_fXp |
| APC UPS (web) | apc | apc |
| Weblogic (web) | system | manager |
| Weblogic (web) | system | manager |
| Weblogic (web) | weblogic | weblogic1 |
| Weblogic (web) | WEBLOGIC | WEBLOGIC |
| Weblogic (web) | PUBLIC | PUBLIC |
| Weblogic (web) | EXAMPLES | EXAMPLES |
| Weblogic (web) | weblogic | weblogic |
| Weblogic (web) | system | password |
| Weblogic (web) | weblogic | welcome(1) |
| Weblogic (web) | system | welcome(1) |
| Weblogic (web) | operator | weblogic |
| Weblogic (web) | operator | password |
| Weblogic (web) | system | Passw0rd |
| Weblogic (web) | monitor | password |
| Kanboard (web) | admin | admin |
| Vectr (web) | admin | 11_ThisIsTheFirstPassword_11 |
| Caldera (web) | admin | admin |
| Dlink (web) | admin | admin |
| Dlink (web) | 1234 | 1234 |
| Dlink (web) | root | 12345 |
| Dlink (web) | root | root |
| JioFiber | admin | jiocentrum |
| GigaFiber | admin | jiocentrum |
| Kali linux (OS) | kali | kali |
| F5 | admin | admin |
| F5 | root | default |
| F5 | support |  |
| ... | ... | ... |

Default credentials can also be found in the product documentation, as they contain the steps necessary to set up the service successfully. Some devices/applications require the user to set up a password at install, but others use a default, weak password. Attacking those services with the default or obtained credentials is called [Credential Stuffing](https://owasp.org/www-community/attacks/Credential_stuffing). This is a simplified variant of brute-forcing because only composite usernames and the associated passwords are used.

We can imagine that we have found some applications used in the network by our customers. After searching the internet for the default credentials, we can create a new list that separates these composite credentials with a colon (`username:password`). In addition, we can select the passwords and mutate them by our `rules` to increase the probability of hits.

### **Credential Stuffing - Hydra Syntax**

```
th1nyunb0y@htb[/htb]$ hydra -C <user_pass.list> <protocol>://<IP>
```

### **Credential Stuffing - Hydra**

```
th1nyunb0y@htb[/htb]$ hydra -C user_pass.list ssh://10.129.42.197

...
```

Here, OSINT plays another significant role. Because OSINT gives us a "feel" for how the company and its infrastructure are structured, we will understand which passwords and user names we can combine. We can then store these in our lists and use them afterward. In addition, we can use Google to see if the applications we find have hardcoded credentials that can be used.

### **Google Search - Default Credentials**

![https://academy.hackthebox.com/storage/modules/147/Google-default-creds.png](https://academy.hackthebox.com/storage/modules/147/Google-default-creds.png)

Besides the default credentials for applications, some lists offer them for routers. One of these lists can be found [here](https://www.softwaretestinghelp.com/default-router-username-and-password-list/). It is much less likely that the default credentials for routers are left unchanged. Since these are the central interfaces for networks, administrators typically pay much closer attention to hardening them. Nevertheless, it is still possible that a router is overlooked or is currently only being used in the internal network for test purposes, which we can then exploit for further attacks.

| **Router Brand** | **Default IP Address** | **Default Username** | **Default Password** |
| --- | --- | --- | --- |
| **3Com** | **http://192.168.1.1** | **admin** | **Admin** |
| **Belkin** | **http://192.168.2.1** | **admin** | **admin** |
| **BenQ** | **http://192.168.1.1** | **admin** | **Admin** |
| **D-Link** | **http://192.168.0.1** | **admin** | **Admin** |
| **Digicom** | **http://192.168.1.254** | **admin** | **Michelangelo** |
| **Linksys** | **http://192.168.1.1** | **admin** | **Admin** |
| **Netgear** | **http://192.168.0.1** | **admin** | **password** |
| **...** | **...** | **...** | **...** |