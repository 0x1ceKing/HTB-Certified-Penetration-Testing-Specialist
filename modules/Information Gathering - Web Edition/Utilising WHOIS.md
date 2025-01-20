- [Utilising WHOIS](#utilising-whois)
  - [**Using WHOIS**](#using-whois)
# Utilising WHOIS

## **Using WHOIS**

Before using the `whois` command, you'll need to ensure it's installed on your Linux system. It's a utility available through linux package managers, and if it's not installed, it can be installed simply with

```
th1nyunb0y@htb[/htb]$ sudo apt updateth1nyunb0y@htb[/htb]$ sudo apt install whois -y
```

The simplest way to access WHOIS data is through the `whois` command-line tool. Let's perform a WHOIS lookup on `facebook.com`:

```
th1nyunb0y@htb[/htb]$ whois facebook.com

   Domain Name: FACEBOOK.COM
   Registry Domain ID: 2320948_DOMAIN_COM-VRSN
   Registrar WHOIS Server: whois.registrarsafe.com
   Registrar URL: http://www.registrarsafe.com
   Updated Date: 2024-04-24T19:06:12Z
   Creation Date: 1997-03-29T05:00:00Z
   Registry Expiry Date: 2033-03-30T04:00:00Z
   Registrar: RegistrarSafe, LLC
   Registrar IANA ID: 3237
   Registrar Abuse Contact Email: abusecomplaints@registrarsafe.com
   Registrar Abuse Contact Phone: +1-650-308-7004
   Domain Status: clientDeleteProhibited https://icann.org/epp#clientDeleteProhibited   Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited   Domain Status: clientUpdateProhibited https://icann.org/epp#clientUpdateProhibited   Domain Status: serverDeleteProhibited https://icann.org/epp#serverDeleteProhibited   Domain Status: serverTransferProhibited https://icann.org/epp#serverTransferProhibited   Domain Status: serverUpdateProhibited https://icann.org/epp#serverUpdateProhibited   Name Server: A.NS.FACEBOOK.COM
   Name Server: B.NS.FACEBOOK.COM
   Name Server: C.NS.FACEBOOK.COM
   Name Server: D.NS.FACEBOOK.COM
   DNSSEC: unsigned
   URL of the ICANN Whois Inaccuracy Complaint Form: https://www.icann.org/wicf/
>>> Last update of whois database: 2024-06-01T11:24:10Z <<<

[...]
Registry Registrant ID:
Registrant Name: Domain Admin
Registrant Organization: Meta Platforms, Inc.
[...]
```

The WHOIS output for `facebook.com` reveals several key details:

1. `Domain Registration`:
    - `Registrar`: RegistrarSafe, LLC
    - `Creation Date`: 1997-03-29
    - `Expiry Date`: 2033-03-30
    
    These details indicate that the domain is registered with RegistrarSafe, LLC, and has been active for a considerable period, suggesting its legitimacy and established online presence. The distant expiry date further reinforces its longevity.
    
2. `Domain Owner`:
    - `Registrant/Admin/Tech Organization`: Meta Platforms, Inc.
    - `Registrant/Admin/Tech Contact`: Domain Admin
    
    This information identifies Meta Platforms, Inc. as the organization behind `facebook.com`, and "Domain Admin" as the point of contact for domain-related matters. This is consistent with the expectation that Facebook, a prominent social media platform, is owned by Meta Platforms, Inc.
    
3. `Domain Status`:
    - `clientDeleteProhibited`, `clientTransferProhibited`, `clientUpdateProhibited`, `serverDeleteProhibited`, `serverTransferProhibited`, and `serverUpdateProhibited`
    
    These statuses indicate that the domain is protected against unauthorized changes, transfers, or deletions on both the client and server sides. This highlights a strong emphasis on security and control over the domain.
    
4. `Name Servers`:
    - `A.NS.FACEBOOK.COM`, `B.NS.FACEBOOK.COM`, `C.NS.FACEBOOK.COM`, `D.NS.FACEBOOK.COM`
    
    These name servers are all within the `facebook.com` domain, suggesting that Meta Platforms, Inc. manages its DNS infrastructure. It is common practice for large organizations to maintain control and reliability over their DNS resolution.
    

Overall, the WHOIS output for `facebook.com` aligns with expectations for a well-established and secure domain owned by a large organization like Meta Platforms, Inc.

While the WHOIS record provides contact information for domain-related issues, it might not be directly helpful in identifying individual employees or specific vulnerabilities. This highlights the need to combine WHOIS data with other reconnaissance techniques to understand the target's digital footprint comprehensively.