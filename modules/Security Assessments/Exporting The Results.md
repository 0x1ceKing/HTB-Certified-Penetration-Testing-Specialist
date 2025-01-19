# Exporting The Results
- [Exporting The Results](#exporting-the-results)
  - [**Exporting Formats**](#exporting-formats)

OpenVAS provides the scan results in a report that can be accessed when you are on the `Scans` page, as shown below.

![https://academy.hackthebox.com/storage/modules/108/openvas/viewingreport.png](https://academy.hackthebox.com/storage/modules/108/openvas/viewingreport.png)

Once you click the report, you can view the scan results and operating system information, open ports, services, etc., in other tabs in the scan report.

![https://academy.hackthebox.com/storage/modules/108/openvas/openvas_reports.png](https://academy.hackthebox.com/storage/modules/108/openvas/openvas_reports.png)

---

## **Exporting Formats**

There are various export formats for reporting purposes, including XML, CSV, PDF, ITG, and TXT. If you choose to export your report out as an XML, you can leverage various XML parsers to view the data in an easier to read format.

![https://academy.hackthebox.com/storage/modules/108/openvas/reportformat.png](https://academy.hackthebox.com/storage/modules/108/openvas/reportformat.png)

We will export our results in XML and use the [openvasreporting](https://github.com/TheGroundZero/openvasreporting) tool by the TheGroundZero. The `openvasreporting` tool offers various options when generating output. We are using the standard option for an Excel file for this report.

```
th1nyunb0y@htb[/htb]$ python3 -m openvasreporting -i report-2bf466b5-627d-4659-bea6-1758b43235b1.xml -f xlsx
```

This command will generate an excel document similar to the one below:

![https://academy.hackthebox.com/storage/modules/108/openvas/openvas_report.png](https://academy.hackthebox.com/storage/modules/108/openvas/openvas_report.png)

![https://academy.hackthebox.com/storage/modules/108/openvas/report_toc.png](https://academy.hackthebox.com/storage/modules/108/openvas/report_toc.png)