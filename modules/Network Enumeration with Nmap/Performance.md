# Performance
- [Performance](#performance)
  - [**Timeouts**](#timeouts)
    - [**Default Scan**](#default-scan)
    - [**Optimized RTT**](#optimized-rtt)
  - [**Max Retries**](#max-retries)
    - [**Default Scan**](#default-scan-1)
    - [**Reduced Retries**](#reduced-retries)
  - [**Rates**](#rates)
    - [**Default Scan**](#default-scan-2)
    - [**Optimized Scan**](#optimized-scan)
    - [**Default Scan - Found Open Ports**](#default-scan---found-open-ports)
    - [**Optimized Scan - Found Open Ports**](#optimized-scan---found-open-ports)
  - [**Timing**](#timing)
    - [**Default Scan**](#default-scan-3)
    - [**Insane Scan**](#insane-scan)
    - [**Default Scan - Found Open Ports**](#default-scan---found-open-ports-1)
    - [**Insane Scan - Found Open Ports**](#insane-scan---found-open-ports)

Scanning performance plays a significant role when we need to scan an extensive network or are dealing with low network bandwidth. We can use various options to tell `Nmap` how fast (`-T <0-5>`), with which frequency (`--min-parallelism <number>`), which timeouts (`--max-rtt-timeout <time>`) the test packets should have, how many packets should be sent simultaneously (`--min-rate <number>`), and with the number of retries (`--max-retries <number>`) for the scanned ports the targets should be scanned.

---

## **Timeouts**

When Nmap sends a packet, it takes some time (`Round-Trip-Time` - `RTT`) to receive a response from the scanned port. Generally, `Nmap` starts with a high timeout (`--min-RTT-timeout`) of 100ms. Let us look at an example by scanning the whole network with 256 hosts, including the top 100 ports.

### **Default Scan**

```
th1nyunb0y@htb[/htb]$ sudo nmap 10.129.2.0/24 -F<SNIP>
Nmap done: 256 IP addresses (10 hosts up) scanned in 39.44 seconds
```

### **Optimized RTT**

```
th1nyunb0y@htb[/htb]$ sudo nmap 10.129.2.0/24 -F --initial-rtt-timeout 50ms --max-rtt-timeout 100ms<SNIP>
Nmap done: 256 IP addresses (8 hosts up) scanned in 12.29 seconds
```

| **Scanning Options** | **Description** |
| --- | --- |
| `10.129.2.0/24` | Scans the specified target network. |
| `-F` | Scans top 100 ports. |
| `--initial-rtt-timeout 50ms` | Sets the specified time value as initial RTT timeout. |
| `--max-rtt-timeout 100ms` | Sets the specified time value as maximum RTT timeout. |

When comparing the two scans, we can see that we found two hosts less with the optimized scan, but the scan took only a quarter of the time. From this, we can conclude that setting the initial RTT timeout (`--initial-rtt-timeout`) to too short a time period may cause us to overlook hosts.

---

## **Max Retries**

Another way to increase scan speed is by specifying the retry rate of sent packets (`--max-retries`). The default value is `10`, but we can reduce it to `0`. This means if Nmap does not receive a response for a port, it won't send any more packets to that port and will skip it.

### **Default Scan**

```
th1nyunb0y@htb[/htb]$ sudo nmap 10.129.2.0/24 -F | grep "/tcp" | wc -l23
```

### **Reduced Retries**

```
th1nyunb0y@htb[/htb]$ sudo nmap 10.129.2.0/24 -F --max-retries 0 | grep "/tcp" | wc -l21
```

| **Scanning Options** | **Description** |
| --- | --- |
| `10.129.2.0/24` | Scans the specified target network. |
| `-F` | Scans top 100 ports. |
| `--max-retries 0` | Sets the number of retries that will be performed during the scan. |

Again, we recognize that accelerating can also have a negative effect on our results, which means we can overlook important information.

---

## **Rates**

During a white-box penetration test, we may get whitelisted for the security systems to check the systems in the network for vulnerabilities and not only test the protection measures. If we know the network bandwidth, we can work with the rate of packets sent, which significantly speeds up our scans with `Nmap`. When setting the minimum rate (`--min-rate <number>`) for sending packets, we tell `Nmap` to simultaneously send the specified number of packets. It will attempt to maintain the rate accordingly.

### **Default Scan**

```
th1nyunb0y@htb[/htb]$ sudo nmap 10.129.2.0/24 -F -oN tnet.default<SNIP>
Nmap done: 256 IP addresses (10 hosts up) scanned in 29.83 seconds
```

### **Optimized Scan**

```
th1nyunb0y@htb[/htb]$ sudo nmap 10.129.2.0/24 -F -oN tnet.minrate300 --min-rate 300<SNIP>
Nmap done: 256 IP addresses (10 hosts up) scanned in 8.67 seconds
```

| **Scanning Options** | **Description** |
| --- | --- |
| `10.129.2.0/24` | Scans the specified target network. |
| `-F` | Scans top 100 ports. |
| `-oN tnet.minrate300` | Saves the results in normal formats, starting the specified file name. |
| `--min-rate 300` | Sets the minimum number of packets to be sent per second. |

---

### **Default Scan - Found Open Ports**

```
th1nyunb0y@htb[/htb]$ cat tnet.default | grep "/tcp" | wc -l23
```

### **Optimized Scan - Found Open Ports**

```
th1nyunb0y@htb[/htb]$ cat tnet.minrate300 | grep "/tcp" | wc -l23
```

---

## **Timing**

Because such settings cannot always be optimized manually, as in a black-box penetration test, `Nmap` offers six different timing templates (`-T <0-5>`) for us to use. These values (`0-5`) determine the aggressiveness of our scans. This can also have negative effects if the scan is too aggressive, and security systems may block us due to the produced network traffic. The default timing template used when we have defined nothing else is the normal (`-T 3`).

- `T 0` / `T paranoid`
- `T 1` / `T sneaky`
- `T 2` / `T polite`
- `T 3` / `T normal`
- `T 4` / `T aggressive`
- `T 5` / `T insane`

These templates contain options that we can also set manually, and have seen some of them already. The developers determined the values set for these templates according to their best results, making it easier for us to adapt our scans to the corresponding network environment. The exact used options with their values we can find here: [https://nmap.org/book/performance-timing-templates.html](https://nmap.org/book/performance-timing-templates.html)

### **Default Scan**

```
th1nyunb0y@htb[/htb]$ sudo nmap 10.129.2.0/24 -F -oN tnet.default <SNIP>
Nmap done: 256 IP addresses (10 hosts up) scanned in 32.44 seconds
```

### **Insane Scan**

```
th1nyunb0y@htb[/htb]$ sudo nmap 10.129.2.0/24 -F -oN tnet.T5 -T 5<SNIP>
Nmap done: 256 IP addresses (10 hosts up) scanned in 18.07 seconds
```

| **Scanning Options** | **Description** |
| --- | --- |
| `10.129.2.0/24` | Scans the specified target network. |
| `-F` | Scans top 100 ports. |
| `-oN tnet.T5` | Saves the results in normal formats, starting the specified file name. |
| `-T 5` | Specifies the insane timing template. |

---

### **Default Scan - Found Open Ports**

```
th1nyunb0y@htb[/htb]$ cat tnet.default | grep "/tcp" | wc -l23
```

### **Insane Scan - Found Open Ports**

```
th1nyunb0y@htb[/htb]$ cat tnet.T5 | grep "/tcp" | wc -l23
```

---

More information about scan performance we can find at [https://nmap.org/book/man-performance.html](https://nmap.org/book/man-performance.html)