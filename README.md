# sentinel_intro
Microsoft Sentinel is an advanced, cloud-based security event and information management (SEIM) system that delivers real-time analysis of security alerts for both your cloud and on-premises resources.
Here I will try to demonstrate:
* What you should know.
* How to get Started: Lab setup
* Onboarding to Sentinel - bare with me, I am not a expert!
* KQL quick start
* Connecting MS Services
* Connecting External Services
* Integrating TI
* Detecting Threats
* Some Automation
* SOAR + UEBA
* Threat Hunting
  * Basics of TH
  * Hunting with Bookmarks
  * Hunting with Notebooks
  * Workbooks and Dashboards
  * Overview of the integration with MS Defender and Purview
********************************************************************************************************************
* What you should know:
    * Understanding of SIEM and Security Concepts
    * Some knowledge of KQL (Kust Query Language) - I don't have much, but MS Documentation and Google are our friends here!
    * A idea of how Python works - Same as above, Google/Forums or even the popular "assistent" is handy here.
    * Working knowledge of Windows.
    * Working knowledge of Linux. - Not really necessary, I am going to use a VM based on Windows. But, if you are on Linux the same steps can be applied.
********************************************************************************************************************
Frist things first. 

Get hold of a Virtual Machine by installing [VirtualBox](https://www.virtualbox.org/wiki/Downloads). 
There are alternatives like VMware Workstation Player, VMware Workstation Pro, Hyper-V, QEMU, Parallels Desktop, KVM (Kernel-based Virtual Machine), Proxmox VE, Xen, but I have been using VirtualBox for years, it is free and painless.

Secondly, get a [WinDev2407Eval.VirtualBox](https://aka.ms/windev_VM_virtualbox) which is basically a Windows 11 Development Environment with the latest versions of Windows, the developer tools, SDKs, and samples ready to go.
Having both files already downloaded, install VirtualBox, and UNZIP the WinDev2407Eval.VirtualBox.
It's basically Next > Next > Next. No drama.
The initial page looks like this once you click on the .exe file:

![image](https://github.com/user-attachments/assets/debf4ec2-8491-451f-a6fb-521983d25916)

Use the DEFAULT options, you don't need to change anything here:

![image](https://github.com/user-attachments/assets/422e89d7-a31c-4941-82a6-805d296fd65a)

Next you are going to get a WARNING, don't worry about it, just hit Yes. In case you are downloading a large file, or playing some online games, be aware your connection will be cut-off for a minute or so.

![image](https://github.com/user-attachments/assets/5295fa0b-ecbe-4352-a639-dcb5228fbabd)

Depending on what you have (or not) already running on your Windows host machine (Linux might trigger different dependencies) it might prompt you to install extra files like the image below. Just hit Yes.

![image](https://github.com/user-attachments/assets/b3df8972-d6ac-443a-be53-ca8fd3a7304a)

Next page is as follows, hit Next again.

![image](https://github.com/user-attachments/assets/1dda1232-1011-4513-afc3-eb25ac6da4cd)

Then hit Install.

![image](https://github.com/user-attachments/assets/b7ec67fe-f063-4e47-b841-544dcb9c6bde)

Once installed, that's your final installation page, which once you hit Finish, it will load up VirtualBox.

![image](https://github.com/user-attachments/assets/56d64b05-784d-4aa9-a84d-a2056581bc8f)
        
And that's how it looks like:

![image](https://github.com/user-attachments/assets/520ad62b-c404-40c6-891c-8f8da433ebe1)

With VirtualBox installed, and the WinDev2407Eval.VirtualBox.zip UNZIPPED, just double click on the extracted file, "WinDev2407Eval.ova".
It will summon VirtualBox automatically and will install a Windows VM within VB but it won't launch it by itself. It will look like this:

![image](https://github.com/user-attachments/assets/a11f3716-ebd9-4083-adfe-0f604e4ebe7f)

Just hit Finish and that's it.

Don't mind the Kali-Linux machine in the list.
Select WinDev2407Eval and hit Start. VirtualBox will pop-up another screen where it will load a Windows 11 instance. Just wait for it to finish.

![image](https://github.com/user-attachments/assets/dbb804a8-5d04-410f-ae67-630d5ff20eac)

Once done you will end up with Windows 11 loaded up like below. Resize it to your liking.

![image](https://github.com/user-attachments/assets/4177407b-9871-4ea3-8117-baca76211af2)










