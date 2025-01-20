# Getting Started with Nessus
- [Getting Started with Nessus](#getting-started-with-nessus)
  - [**Downloading Nessus**](#downloading-nessus)
  - [**Requesting Free License**](#requesting-free-license)
  - [**Installing Package**](#installing-package)
  - [**Starting Nessus**](#starting-nessus)
  - [**Accessing Nessus**](#accessing-nessus)

## **Downloading Nessus**

To download Nessus, we can navigate to its [Download Page](https://www.tenable.com/downloads/nessus?loginAttempted=true) to download the correct Nessus binary for our system. We will be downloading the Debian package for **`Ubuntu`** for this walkthrough.

![https://academy.hackthebox.com/storage/modules/108/openvas/deb.png](https://academy.hackthebox.com/storage/modules/108/openvas/deb.png)

---

## **Requesting Free License**

Next, we can visit the [Activation Code Page](https://www.tenable.com/products/nessus/activation-code) to request a Nessus Activation Code, which is necessary to get the free version of Nessus:

![https://academy.hackthebox.com/storage/modules/108/nessus/register.png](https://academy.hackthebox.com/storage/modules/108/nessus/register.png)

![https://academy.hackthebox.com/storage/modules/108/nessus/registrationcode.png](https://academy.hackthebox.com/storage/modules/108/nessus/registrationcode.png)

---

## **Installing Package**

With both the binary and activation code in hand, we can now install the Nessus package:

```
th1nyunb0y@htb[/htb]$ dpkg -i Nessus-8.15.1-ubuntu910_amd64.deb

Selecting previously unselected package nessus.
(Reading database ... 132030 files and directories currently installed.)
Preparing to unpack Nessus-8.15.1-ubuntu910_amd64.deb ...
Unpacking nessus (8.15.1) ...
Setting up nessus (8.15.1) ...
Unpacking Nessus Scanner Core Components...
Created symlink /etc/systemd/system/nessusd.service → /lib/systemd/system/nessusd.service.
Created symlink /etc/systemd/system/multi-user.target.wants/nessusd.service → /lib/systemd/system/nessusd.service.

```

---

## **Starting Nessus**

Once we have Nessus installed, we can start the Nessus Service:

```
th1nyunb0y@htb[/htb]$ sudo systemctl start nessusd.service
```

---

## **Accessing Nessus**

To access Nessus, we can navigate to **`https://localhost:8834`**. Once we arrive at the setup page, we should select **`Nessus Essentials`** for the free version, and then we can enter our activation code:

![https://academy.hackthebox.com/storage/modules/108/nessus/essentials.png](https://academy.hackthebox.com/storage/modules/108/nessus/essentials.png)

Once we enter our activation code, we can set up a user with a **`secure`** password for our Nessus account. Then, the plugins will begin to compile once this step is completed:

![https://academy.hackthebox.com/storage/modules/108/nessus/init.png](https://academy.hackthebox.com/storage/modules/108/nessus/init.png)

**Note:** The VM provided at the `Nessus Skills Assessment` section has Nessus pre-installed and the targets running. You can go to that section and start the VM and use Nessus throughout the module, which can be accessed at `https:// < IP >:8834`. The Nessus credentials are: `htb-student`:`HTB_@cademy_student!`. You may also use these credentials to SSH into the target VM to configure Nessus.

Finally, once the setup is complete, we can start creating scans, scan policies, plugin rules, and customizing settings. The `Settings` page has a wealth of options such as setting up a Proxy Server or SMTP server, standard account management options, and advanced settings to customize the user interface, scanning, logging, performance, and security options.

![https://academy.hackthebox.com/storage/modules/108/nessus/nessus_settings.png](https://academy.hackthebox.com/storage/modules/108/nessus/nessus_settings.png)