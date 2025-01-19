- [Nessus Scan](#nessus-scan)
  - [**New Scan**](#new-scan)
  - [**Discovery**](#discovery)
  - [**Assessment**](#assessment)
  - [**Advanced**](#advanced)
# Nessus Scan

A new Nessus scan can be configured by clicking `New Scan`, and selecting a scan type. Scan templates fall into three categories: `Discovery`, `Vulnerabilities`, and `Compliance`.

**Note:** The scans shown in this section have already been pre-run to save you the time of waiting for them to finish. If you re-run the scan, it's best to go through vulnerabilities as they come, instead of waiting for the scan to finish, as they can take 1-2 hours to finish.

---

## **New Scan**

Here we have options for a basic `Host Discovery` scan to identify live hosts/open ports or a variety of scan types such as the `Basic Network Scan`, `Advanced Scan`, `Malware Scan`, `Web Application Tests`, as well as scans targeted at specific CVEs and audit & compliance standards. A description of each scan type can be found [here](https://docs.tenable.com/nessus/Content/ScanAndPolicyTemplates.htm).

![https://academy.hackthebox.com/storage/modules/108/nessus/nessus_scan_types.png](https://academy.hackthebox.com/storage/modules/108/nessus/nessus_scan_types.png)

For the purposes of this exercise, we will choose the **`Basic Network Scan`** option, and we can enter our targets:

![https://academy.hackthebox.com/storage/modules/108/nessus/general.png](https://academy.hackthebox.com/storage/modules/108/nessus/general.png)

**Note:** For this module, the Windows target will be `172.16.16.100` and the Linux target will be `172.16.16.160`.

---

## **Discovery**

In the **`Discovery`** section, under **`Host Discovery`** , we're presented with the option to enable scanning for fragile devices. Scanning devices such as network printers often result in them printing out reams of paper with garbage text, leaving the devices unusable. We can leave this setting disabled:

![https://academy.hackthebox.com/storage/modules/108/nessus/options.png](https://academy.hackthebox.com/storage/modules/108/nessus/options.png)

In **`Port Scanning`** , we can choose whether to scan common ports, all ports, or a self-defined range, depending on our requirements:

![https://academy.hackthebox.com/storage/modules/108/nessus/discovery.png](https://academy.hackthebox.com/storage/modules/108/nessus/discovery.png)

Within the `Service Discovery` subsection, the `Probe all ports to find services` option is selected by default. It's possible that a poorly designed application or service could crash as a result of this probing, but most applications should be robust enough to handle this. Searching for SSL/TLS services is also enabled by default on a custom scan, and Nessus can additionally be instructed to identify expiring and revoked certificates.

---

## **Assessment**

Under the **`Assessment`** category, web application scanning can also be enabled if required, and a custom user agent and various other web application scanning options can be specified (e.g., a URL for Remote File Inclusion (RFI) testing):

![https://academy.hackthebox.com/storage/modules/108/nessus/webapp.png](https://academy.hackthebox.com/storage/modules/108/nessus/webapp.png)

If desired, Nessus can attempt to authenticate against discovered applications and services using provided credentials (if running a credentialed scan), or else can perform a brute-force attack with the provided username and password lists:

![https://academy.hackthebox.com/storage/modules/108/nessus/hydra.png](https://academy.hackthebox.com/storage/modules/108/nessus/hydra.png)

User enumeration can also be performed using various techniques, such as RID Brute Forcing:

![https://academy.hackthebox.com/storage/modules/108/nessus/userenum.png](https://academy.hackthebox.com/storage/modules/108/nessus/userenum.png)

If we opt to perform RID Brute Forcing, we can set the starting and ending UIDs for both domain and local user accounts:

![https://academy.hackthebox.com/storage/modules/108/nessus/ridbf.png](https://academy.hackthebox.com/storage/modules/108/nessus/ridbf.png)

---

## **Advanced**

On the **`Advanced`** tab, safe checks are enabled by default. This prevents Nessus from running checks that may negatively impact the target device or network. We can also choose to slow or throttle the scan if Nessus detects any network congestion, stop attempting to scan any hosts that become unresponsive, and even choose to have Nessus scan our target IP list in random order:

![https://academy.hackthebox.com/storage/modules/108/nessus/advanced.png](https://academy.hackthebox.com/storage/modules/108/nessus/advanced.png)