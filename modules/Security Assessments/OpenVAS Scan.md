# OpenVAS Scan
- [OpenVAS Scan](#openvas-scan)
  - [**Configuration**](#configuration)
  - [**Setting Up a Scan**](#setting-up-a-scan)

The OpenVAS Greenbone Security Assistant application has various tabs that you can interact with. For this section, we will be digging into the scans. If you navigate to the `Scans` tab shown below, you will see the scans that have run in the past. You will also be able to see how to create a new task to run a scan. The tasks work off of the scanning configurations that the user sets up.

**Note:** The scans shown in this section have already been pre-run to save you the time of waiting for them to finish. If you re-run the scan, it's best to go through vulnerabilities as they come, instead of waiting for the scan to finish, as they can take 1-2 hours to finish.

![https://academy.hackthebox.com/storage/modules/108/openvas/creatingscan1.png](https://academy.hackthebox.com/storage/modules/108/openvas/creatingscan1.png)

**Note:** For this module, the Windows target will be `172.16.16.100` and the Linux target will be `172.16.16.160`.

![https://academy.hackthebox.com/storage/modules/108/openvas/scanconfigs.png](https://academy.hackthebox.com/storage/modules/108/openvas/scanconfigs.png)

---

## **Configuration**

Before setting up any scans, it is best to configure the targets for the scan. If you navigate to the `Configurations` tab and select `Targets`, you will see targets that have been already added to the application.

![https://academy.hackthebox.com/storage/modules/108/openvas/targets.png](https://academy.hackthebox.com/storage/modules/108/openvas/targets.png)

To add your own, click the icon highlighted below and add an individual target or a host list. You also can configure other options such as the ports, authentication, and methods of identifying if the host is reachable. For the `Alive Test`, the `Scan Config Default` option from OpenVAS leverages the `NVT Ping Host` in the `NVT Family`. You can learn about the NVT Family [here](https://docs.greenbone.net/GSM-Manual/gos-22.04/en/scanning.html#creating-a-target).

![https://academy.hackthebox.com/storage/modules/108/openvas/addingtarget.png](https://academy.hackthebox.com/storage/modules/108/openvas/addingtarget.png)

Typically, an `authenticated scan` leverages a high privileged user such as `root` or `Administrator`. Depending on the permission level for the user, if it's the highest permission level, you'll retrieve the maximum amount of information back from the host in regards to the vulnerabilities present since you would have full access.

**Note:** To run a credentialed scan on the target, use the following credentials: `htb-student_adm`:`HTB_@cademy_student!` for Linux, and `administrator`:`Academy_VA_adm1!` for Windows. These scans have already been set up in the OpenVAS target to save you time.

Once you have added your target, they will appear in the list below:

![https://academy.hackthebox.com/storage/modules/108/openvas/targetsview.png](https://academy.hackthebox.com/storage/modules/108/openvas/targetsview.png)

---

## **Setting Up a Scan**

Multiple scan configurations leverage OpenVAS Network Vulnerability Test (NVT) Families, which consist of many different categories of vulnerabilities, such as ones for Windows, Linux, Web Applications, etc. You can see a few different types of families shown below:

![https://academy.hackthebox.com/storage/modules/108/openvas/nvt2.png](https://academy.hackthebox.com/storage/modules/108/openvas/nvt2.png)

OpenVAS has various scan configurations to choose from for scanning a network. We recommend only leveraging the ones below, as other options could cause system disruptions on a network:

- `Base`: This scan configuration is meant to enumerate information about the host's status and operating system information. This scan configuration does not check for vulnerabilities.
- `Discovery`: This scan configuration is meant to enumerate information about the system. The configuration identifies the host's services, hardware, accessible ports, and software being used on the system. This scan configuration also does not check for vulnerabilities.
- `Host Discovery`: This scan configuration solely tests whether the host is alive and determines what devices are `active` on the network. This scan configuration does not check for vulnerabilities as well. *OpenVAS leverages ping to identify if the host is alive.*
- `System Discovery`: This scan enumerates the target host further than the 'Discovery Scan' and attempts to identify the operating system and hardware associated with the host.
- `Full and fast`: This configuration is recommended by OpenVAS as the safest option and leverages intelligence to use the best NVT checks for the host(s) based on the accessible ports.

You can create your own scan by navigating to the 'Scans' tab and clicking the wizard icon.

![https://academy.hackthebox.com/storage/modules/108/openvas/creatingscan2.png](https://academy.hackthebox.com/storage/modules/108/openvas/creatingscan2.png)

Once you click the wizard icon, the panel shown below will pop up and allow you to configure your scan.

![https://academy.hackthebox.com/storage/modules/108/openvas/Newscan.png](https://academy.hackthebox.com/storage/modules/108/openvas/Newscan.png)

We will configure the scan with the options below, which targets `172.16.16.160` and then run our scan, which can take `30-60 minutes` to finish.

![https://academy.hackthebox.com/storage/modules/108/openvas/linux_basic.png](https://academy.hackthebox.com/storage/modules/108/openvas/linux_basic.png)

![https://academy.hackthebox.com/storage/modules/108/openvas/linux_unauthedtarget.png](https://academy.hackthebox.com/storage/modules/108/openvas/linux_unauthedtarget.png)