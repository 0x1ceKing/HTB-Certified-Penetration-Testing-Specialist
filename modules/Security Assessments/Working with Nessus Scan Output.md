# Working with Nessus Scan Output
- [Working with Nessus Scan Output](#working-with-nessus-scan-output)
  - [**Nessus Reports**](#nessus-reports)
  - [**Exporting Nessus Scans**](#exporting-nessus-scans)

Nessus gives us the option to export scan results in a variety of report formats as well as the option to export raw Nessus scan results to be imported into other tools, archived, or passed to tools, such as [EyeWitness](https://github.com/FortyNorthSecurity/EyeWitness), which can be used to take screenshots of all web applications identified by Nessus and greatly assist us with working through the results and finding more value in them.

---

## **Nessus Reports**

Once a scan is completed we can choose to export a report in `.pdf`, `.html`, or `.csv` formats. The .pdf and .html reports give the option for either an Executive Summary or a custom report. The Executive Summary report provides a listing of hosts, a total number of vulnerabilities discovered per host, and a `Show Details` option to see the severity, CVSS score, plugin number, and name of each discovered issue. The plugin number contains a link to the full plugin writeup from the Tenable plugin database. The PDF option provides the scan results in a format that is easier to share. The CSV report option allows us to select which columns we would like to export. This is particularly useful if importing the scan results into another tool such as Splunk if a document needs to be shared with many internal stakeholders responsible for remediation of the various assets scanned or to perform analytics on the scan data.

![https://academy.hackthebox.com/storage/modules/108/nessus/exportreport.png](https://academy.hackthebox.com/storage/modules/108/nessus/exportreport.png)

**Note:** These scan reports should only be shared as either an appendix or supplementary data to a custom penetration test/vulnerability assessment report. They should not be given to a client as the final deliverable for any assessment type.

An example of the HTML report is shown below:

![https://academy.hackthebox.com/storage/modules/108/nessus/htmlreport.png](https://academy.hackthebox.com/storage/modules/108/nessus/htmlreport.png)

It is best to always make sure the vulnerabilities are grouped together for a clear understanding of each issue and the assets affected.

---

## **Exporting Nessus Scans**

Nessus also gives the option to export scans into two formats `Nessus (scan.nessus)` or `Nessus DB (scan.db)`. The `.nessus` file is an `.xml` file and includes a copy of the scan settings and plugin outputs. The `.db` file contains the `.nessus` file and the scan's KB, plugin Audit Trail, and any scan attachments. More information about the `KB` and `Audit Trail` can be found [here](https://community.tenable.com/s/article/What-is-included-in-a-nessus-db-file).

Scripts such as the [nessus-report-downloader](https://raw.githubusercontent.com/eelsivart/nessus-report-downloader/master/nessus6-report-downloader.rb) can be used to quickly download scan results in all available formats from the CLI using the Nessus REST API:

```
th1nyunb0y@htb[/htb]$ ./nessus_downloader.rb 

Nessus 6 Report Downloader 1.0

Enter the Nessus Server IP: 127.0.0.1
Enter the Nessus Server Port [8834]: 8834
Enter your Nessus Username: admin
Enter your Nessus Password (will not echo):

Getting report list...
Scan ID Name                                               Last Modified                  Status
------- ----                                               -------------                  ------
1     Windows_basic                                Aug 22, 2020 22:07 +00:00      completed

Enter the report(s) your want to download (comma separate list) or 'all': 1

Choose File Type(s) to Download:
[0] Nessus (No chapter selection)
[1] HTML
[2] PDF
[3] CSV (No chapter selection)
[4] DB (No chapter selection)
Enter the file type(s) you want to download (comma separate list) or 'all': 3

Path to save reports to (without trailing slash): /assessment_data/inlanefreight/scans/nessus

Downloading report(s). Please wait...

[+] Exporting scan report, scan id: 1, type: csv
[+] Checking export status...
[+] Report ready for download...
[+] Downloading report to: /assessment_data/inlanefreight/scans/nessus/inlanefreight_basic_5y3hxp.csv

Report Download Completed!
```

We can also write our own scripts to automate many Nessus features.