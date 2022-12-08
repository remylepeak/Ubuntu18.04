---
description: Programs/Tools that were manually configured before enumeration
---

# What was added to the VMs

## **Shortlist**

Through enumeration and ssh techniques, multiple programs and tools were added to both Host and Victim VM:

* Metasploit
* Nmap
* Apache Web Server
* SSH
* User Configured Backdoor
* Username/Password list&#x20;
* Auditd
* CRON
* Private User Information
* Additional Users

Prior to running any commands/enumeration ^/apt-get update/ and similar commands were applied to both Ubuntu18.04 VMs.

## **On HostUbuntu VM**

### **Metasploit**

<figure><img src="../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>

Metasploit will be the tool that generates our attack, by using different scripts that will run on our Host VM. This is a very powerful tool that when combined with others, such as Nmap, can be a deadly and rather easy form of reconnaissance and further attack development.&#x20;

The easiest way to install the Metasploit framework is from the Metasploit installer, which comes with all dependencies and tools required to run all in one package. This way you can avoid individually installing the ruby gems and bundler, for example, which really just saves time and future problems.

This command will download the Metasploit Installer: ^/curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

Make it executable with ^/chmod 755 msfinstall

Create and initialize the msf database with ^/msfbd init

Finally, launch the framework and load into Metasploit with ^/msfconsole

<figure><img src="../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

### **SSH**

SSH will be necessary once initial Metasploit scripts have been executed in order to gain remote access to our victim. In some cases like this, OpenSSH-client was automatically installed when the VM was created, and needs to be deleted in order to install OpenSSH-server. The command ^/apt install openssh-server will enable SSH. Then check the status with ^/systemctl status ssh

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

### **Apache** **Web Server**

This web server will act as the way to download the backdoor because it will be manually staged there to transmit to the victim. Run the command ^/apt install apache2&#x20;

<figure><img src="../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

Certain ports must be open for the webpage to communicate from the outside, so the firewall rules must be adjusted. Use the following commands to enable the firewall or add more if you want to test the available VM on your own.

* ^/sudo ufw enable
* ^/sudo ufw status
* ^/sudo ufw allow tcp
* ^/sudo ufw allow 'SSH'
* ^/sudo ufw reload

## &#x20;**On FinalUbuntu(Victim) VM**

### **Auditd**

This tool was installed so that anyone who downloads the VM will be able to search for specific logs within the victim VM. First install bash if it is not present with ^/apt install bash-completion

Then install auditd with ^/apt-get install auditd

****![](<../.gitbook/assets/image (68).png>)****

Check the status of Auditd with ^/systemctl status auditd

****![](<../.gitbook/assets/image (6) (1).png>)****

### Nmap

This tool is a powerful network scanner, that will be used to discover open ports and IP addresses on a particular network. It reflects real-life examples of how a threat actor might run default scans to gain as much reconnaissance about a target as possible before striking. Run the command to install Nmap ^/apt-get install nmap

<figure><img src="../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

Check the version to make sure it is installed correctly with ^/nmap --version

<figure><img src="../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

### **SSH**

SSH will need to be installed on both Victim and Host VM so that the connection can complete and be accurate

<figure><img src="../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>

### **CRON**

Cron is driven by crontab, which is a software utility that uses a time-based scheduler in Unix OS. Cron gives the ability to create commands and store them inside the system. These are stored in the crontab(cron table) file, a configuration file that specifies said shell commands to run periodically on a given schedule.

Use the following command to install cron ^/apt install cron

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

Then make sure it is enabled to be running in the background with ^/systemctl enable cron

<figure><img src="../.gitbook/assets/image (73).png" alt=""><figcaption></figcaption></figure>

Cron jobs are recorded and managed in the special file known as crontab. These jobs are composed of syntax which can be broken down into two components: the schedule and the command to run. More details will come on Cron jobs here:

{% content-ref url="how-this-vm-was-compromised.md" %}
[how-this-vm-was-compromised.md](how-this-vm-was-compromised.md)
{% endcontent-ref %}

**User Information**

A username and password list are stored in the document file section containing&#x20;

commonly used account credentials.&#x20;

<figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

![](<../.gitbook/assets/image (52).png>)![](<../.gitbook/assets/image (44).png>)

**Additional Users**

Two new users were created and stored on the Victim: hacker and hacked.

<figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>



