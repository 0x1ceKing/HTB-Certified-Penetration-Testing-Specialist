# Laudanum, One Webshell to Rule Them All
- [Laudanum, One Webshell to Rule Them All](#laudanum-one-webshell-to-rule-them-all)
  - [**Working with Laudanum**](#working-with-laudanum)
  - [**Laudanum Demonstration**](#laudanum-demonstration)
    - [**Move a Copy for Modification**](#move-a-copy-for-modification)
    - [**Modify the Shell for Use**](#modify-the-shell-for-use)
    - [**Take Advantage of the Upload Function**](#take-advantage-of-the-upload-function)
    - [**Navigate to Our Shell**](#navigate-to-our-shell)
    - [**Shell Success**](#shell-success)

Laudanum is a repository of ready-made files that can be used to inject onto a victim and receive back access via a reverse shell, run commands on the victim host right from the browser, and more. The repo includes injectable files for many different web application languages to include `asp, aspx, jsp, php,` and more. This is a staple to have on any pentest. If you are using your own VM, Laudanum is built into Parrot OS and Kali by default. For any other distro, you will likely need to pull a copy down to use. You can get it [here](https://github.com/jbarcia/Web-Shells/tree/master/laudanum). Let's examine Laudanum and see how it works.

---

## **Working with Laudanum**

The Laudanum files can be found in the `/usr/share/laudanum` directory. For most of the files within Laudanum, you can copy them as-is and place them where you need them on the victim to run. For specific files such as the shells, you must edit the file first to insert your `attacking` host IP address to ensure you can access the web shell or receive a callback in the instance that you use a reverse shell. Before using the different files, be sure to read the contents and comments to ensure you take the proper actions.

---

## **Laudanum Demonstration**

Now that we understand what Laudanum is and how it works, let's look at a web application we have found in our lab environment and see if we can run a web shell. If you wish to follow along with this demonstration, you will need to add an entry into your `/etc/hosts` file on your attack VM or within Pwnbox for the host we are attacking. That entry should read: `<target ip> status.inlanefreight.local`. Once this is done, you can play and explore this demonstration as long as you are on the VPN or using Pwnbox.

### **Move a Copy for Modification**

```
th1nyunb0y@htb[/htb]$ cp /usr/share/laudanum/aspx/shell.aspx /home/tester/demo.aspx
```

Add your IP address to the `allowedIps` variable on line `59`. Make any other changes you wish. It can be prudent to remove the ASCII art and comments from the file. These items in a payload are often signatured on and can alert the defenders/AV to what you are doing.

### **Modify the Shell for Use**

![https://academy.hackthebox.com/storage/modules/115/modify-shell.png](https://academy.hackthebox.com/storage/modules/115/modify-shell.png)

We are taking advantage of the upload function at the bottom of the status page(`Green Arrow`) for this to work. Select your shell file and hit upload. If successful, it should print out the path to where the file was saved (Yellow Arrow). Use the upload function. Success prints out where the file went, navigate to it.

### **Take Advantage of the Upload Function**

![https://academy.hackthebox.com/storage/modules/115/laud-upload.png](https://academy.hackthebox.com/storage/modules/115/laud-upload.png)

Once the upload is successful, you will need to navigate to your web shell to utilize its functions. The image below shows us how to do it. As seen from the last image, our shell was uploaded to the `\\files\` directory, and the name was kept the same. This won't always be the case. You may run into some implementations that randomize filenames on upload that do not have a public files directory or any number of other potential safeguards. For now, we are lucky that's not the case. With this particular web application, our file went to `status.inlanefreight.local\\files\demo.aspx` and will require us to browse for the upload by using that \ in the path instead of the / like normal. Once you do this, your browser will clean it up in your URL window to appear as `status.inlanefreight.local//files/demo.aspx`.

### **Navigate to Our Shell**

![https://academy.hackthebox.com/storage/modules/115/laud-nav.png](https://academy.hackthebox.com/storage/modules/115/laud-nav.png)

We can now utilize the Laudanum shell we uploaded to issue commands to the host. We can see in the example that the `systeminfo` command was run.

### **Shell Success**

![https://academy.hackthebox.com/storage/modules/115/laud-success.png](https://academy.hackthebox.com/storage/modules/115/laud-success.png)