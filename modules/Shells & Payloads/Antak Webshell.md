# Antak Webshell
- [Antak Webshell](#antak-webshell)
  - [**ASPX and a Quick Learning Tip**](#aspx-and-a-quick-learning-tip)
  - [**ASPX Explained**](#aspx-explained)
  - [**Antak Webshell**](#antak-webshell-1)
  - [**Working with Antak**](#working-with-antak)
  - [**Antak Demonstration**](#antak-demonstration)
    - [**Move a Copy for Modification**](#move-a-copy-for-modification)
    - [**Modify the Shell for Use**](#modify-the-shell-for-use)
    - [**Shell Success**](#shell-success)
    - [**Issuing Commands**](#issuing-commands)

## **ASPX and a Quick Learning Tip**

Before diving into aspx shell concepts and exercises, we should take the time to cover a learning resource that can help reinforce most of the concepts covered here in HTB Academy. Occasionally it can be a challenge to visualize a concept using just one learning method. It is good to supplement reading with watching demonstrations and performing hands-on as we have been doing thus far. Video walkthroughs can be an amazing way to learn concepts, plus they can be consumed casually (eating lunch, laying in bed, sitting on the couch, etc.). One great resource to use in learning is `IPPSEC's` blog site [ippsec.rocks](https://ippsec.rocks/?#). The site is a powerful learning tool. Take, for example, the concept of web shells. We can use his site to type in the concept we want to learn, like aspx.

![https://academy.hackthebox.com/storage/modules/115/ippsecrocks.png](https://academy.hackthebox.com/storage/modules/115/ippsecrocks.png)

His site crawls the descriptions of each of the videos he has posted on YouTube and recommends a timestamp associated with that keyword. When we click one of the links, it will take us to that section of the video where this concept is demonstrated. It's like a search engine to learn hacking skills. To gain a good basic understanding of what an aspx web shell is, let's watch the short portion of IPPSEC's demonstration of the retired box [Cereal](https://www.youtube.com/watch?v=04ZBIioD5pA&t=4677s). The link should start us at the 1-hour 17-minute mark. Watch from 1 hour 17-minute mark to the 1-hour 20-minute mark.

We will notice that he uploaded the file via HTTP then navigated to the file using the web browser. This gave him the capability to send commands to and receive output from the underlying Windows operating system.

`How does aspx work?`

---

## **ASPX Explained**

`Active Server Page Extended` (`ASPX`) is a file type/extension written for [Microsoft's ASP.NET Framework](https://docs.microsoft.com/en-us/aspnet/overview). On a web server running the ASP.NET framework, web form pages can be generated for users to input data. On the server side, the information will be converted into HTML. We can take advantage of this by using an ASPX-based web shell to control the underlying Windows operating system. Let's witness this first-hand by utilizing the Antak Webshell.

---

## **Antak Webshell**

Antak is a web shell built-in ASP.Net included within the [Nishang project](https://github.com/samratashok/nishang). Nishang is an Offensive PowerShell toolset that can provide options for any portion of your pentest. Since we are focused on web applications for the moment, let's keep our eyes on `Antak`. Antak utilizes PowerShell to interact with the host, making it great for acquiring a web shell on a Windows server. The UI is even themed like PowerShell. It's time to dive in and experiment with Antak.

---

## **Working with Antak**

The Antak files can be found in the `/usr/share/nishang/Antak-WebShell` directory.

```
th1nyunb0y@htb[/htb]$ ls /usr/share/nishang/Antak-WebShellantak.aspx  Readme.md
```

Antak web shell functions like a Powershell Console. However, it will execute each command as a new process. It can also execute scripts in memory and encode commands you send. As a web shell, Antak is a pretty powerful tool.

---

## **Antak Demonstration**

Now that we understand what Antak is and how it works let's put it to the test against the same web application from the Laudanum section. If you wish to follow along with this demonstration, you will need to add an entry into your `/etc/hosts` file on your attack VM or within Pwnbox for the host we are attacking. That entry should read: `<target ip> status.inlanefreight.local`. Once this is done, as long as you are on the VPN or using Pwnbox, you can also play and explore this demonstration.

### **Move a Copy for Modification**

```
th1nyunb0y@htb[/htb]$ cp /usr/share/nishang/Antak-WebShell/antak.aspx /home/administrator/Upload.aspx
```

Make sure you set credentials for access to the web shell. Modify `line 14`, adding a user (green arrow) and password (orange arrow). This comes into play when you browse to your web shell, much like Laudanum. This can help make your operations more secure by ensuring random people can't just stumble into using the shell. It can be prudent to remove the ASCII art and comments from the file. These items in a payload are often signatured on and can alert the defenders/AV to what you are doing.

### **Modify the Shell for Use**

![https://academy.hackthebox.com/storage/modules/115/antak-changes.png](https://academy.hackthebox.com/storage/modules/115/antak-changes.png)

For the sake of demonstrating the tool, we are uploading it to the same status portal we used for Laudanum. That host was a Windows host, so our shell should work just fine with PowerShell. Upload the file and then navigate to the page for use. It will give you a user and password prompt. Remember, with this web application, the files are stored in the `\\files\` directory. When you navigate to the `upload.aspx` file, you should see a prompt as we have below.

### **Shell Success**

![https://academy.hackthebox.com/storage/modules/115/antak-creds-prompt.png](https://academy.hackthebox.com/storage/modules/115/antak-creds-prompt.png)

As seen in the following image, we will be granted access if our credentials are entered properly.

![https://academy.hackthebox.com/storage/modules/115/antak-success.png](https://academy.hackthebox.com/storage/modules/115/antak-success.png)

Now that we have access, we can utilize PowerShell commands to navigate and take actions against the host. We can issue basic commands from the Antak shell window, upload and download files, encode and execute scripts, and much more (green arrow below). This is an excellent way to utilize a Webshell to deliver us a callback to our command and control platform. We could upload the payload via the Upload function or use a PowerShell one-liner to download and execute the shell for us. If you feel unsure where to start, issue the command `help` in the prompt window (orange arrow ) below.

### **Issuing Commands**

![https://academy.hackthebox.com/storage/modules/115/antak-commands.png](https://academy.hackthebox.com/storage/modules/115/antak-commands.png)