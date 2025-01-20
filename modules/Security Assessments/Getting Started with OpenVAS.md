- [Getting Started with OpenVAS](#getting-started-with-openvas)
  - [**Installing Package**](#installing-package)
  - [**Starting OpenVas**](#starting-openvas)
# Getting Started with OpenVAS

[OpenVAS](https://openvas.org/), by Greenbone Networks, is a publicly available vulnerability scanner. Greenbone Networks has an entire Vulnerability Manager, part of which is the OpenVAS scanner. Greenbone's Vulnerability Manager is also open to the public and free to use. OpenVAS has the capabilities to perform network scans, including authenticated and unauthenticated testing.

![https://academy.hackthebox.com/storage/modules/108/openvas/Greenbone_Security_Assistant.png](https://academy.hackthebox.com/storage/modules/108/openvas/Greenbone_Security_Assistant.png)

We will get started with using OpenVAS by following the installation instruction below for Parrot Security. The tool is pre-installed on the host provided in a later section.

---

## **Installing Package**

First, we can start by installing the tool:

```
th1nyunb0y@htb[/htb]$ sudo apt-get update && apt-get -y full-upgrade

th1nyunb0y@htb[/htb]$ sudo apt-get install gvm && openvas
```

Next, to begin the installation process, we can run the following command below:

```
th1nyunb0y@htb[/htb]$ gvm-setup
```

This will begin the setup process and take up to 30 minutes.

![https://academy.hackthebox.com/storage/modules/108/openvas/gvmsetup.png](https://academy.hackthebox.com/storage/modules/108/openvas/gvmsetup.png)

---

## **Starting OpenVas**

Finally, we can start OpenVas:

```
th1nyunb0y@htb[/htb]$ gvm-start
```

![https://academy.hackthebox.com/storage/modules/108/openvas/gvmstart.png](https://academy.hackthebox.com/storage/modules/108/openvas/gvmstart.png)