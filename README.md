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

Now comes the fun. MS allows you to have a go at Azure, and Sentinel as trial and won't charge anything for it. I did not need to input any sort of CC detail.
Once you gone through the process of registering for a Trial Account, log-in to the Azure Portal and you will be presented with this screen.

![image](https://github.com/user-attachments/assets/a0b43f90-e86e-470b-8f4a-dc970af98a42)


It is a good idea to go through the Tour. If you are impatient, skip the tour, hit the Search bar and look for "Sentinel", click on it. You are going to be asked to start a free trial. Go through the process. 

You will end up like in the image below. Hit "Create MS Sentinel".

![image](https://github.com/user-attachments/assets/669c2c41-352e-4808-b941-50c34f681821)

Then create a Workspace. Create a Resouce Group. In Instance details, give a Name for your Workspace, and select the Region, which should match with the Region your infrastructure is located.

![image](https://github.com/user-attachments/assets/0a686509-f51f-4906-b8f9-8fd38fb02933)

Hit Review + Create.

![image](https://github.com/user-attachments/assets/6262bfff-87ba-42d7-b47a-fcdd3059c307)


There are few reasons why creating a new login analytics instance. 
A new instance can allow you to tighly control who has access to what, and here we are talking about sensitive data in the Sentinel instance.
Also, Service Isolation, this is more towards a Technical Consideration. And the last reason is, well, a fresh start. 

So here I have my Login Analytics Instance is created:

![image](https://github.com/user-attachments/assets/bbf8df8b-0af4-483a-a86a-8eed0948cefc)

Hitting the "Add" button on the end of the page (not shown) would tell me that Sentinel was added with success. 

Then now I have access to its options:
* General
   * Overview
   * Logs
   * News & Guides
   * Search
* Threat Management
   * Incidents
   * Workbooks
   * Hunting
   * Notebooks
   * Entity Behavior
   * Threat Intelligence
   * MITRE ATT&CK

 And many menus that are used to configure Sentinel.

![image](https://github.com/user-attachments/assets/51888baa-97b7-450b-825b-490d1da580bf)

Under General:
* Overview - A summary dashboard that presents key metrics, alerts, and insights about your security environment, helping you quickly assess the current security posture and identify areas that may require attention.
* Logs - A centralized repository of log data from various sources, enabling you to search, analyze, and investigate security events and activities to enhance threat detection and response capabilities.
* News & Guides - A section that provides the latest updates, features, and helpful guides for using Microsoft Sentinel effectively, ensuring you stay informed about new capabilities and best practices.
* Search - A powerful search functionality that allows you to query and filter through logs and data to find specific events or patterns, facilitating in-depth investigations and analysis of security incidents.



![image](https://github.com/user-attachments/assets/5429559d-af3f-48e8-9cca-1d1cf6e02beb)


Under Threat Management we've got Pro-Active and Rule Based Hunting functionality:
* Incidents - A centralized view of security incidents, allowing you to investigate and respond to threats effectively by aggregating related alerts and providing context.
* Workbooks - Customizable dashboards that visualize data and insights from your security environment, enabling you to monitor and analyze security metrics and trends.
* Hunting - A proactive approach to identifying potential threats by searching for anomalies and suspicious activities across your data, using queries and analytics.
* Notebooks - Interactive documents that combine code, visualizations, and narrative text, allowing security analysts to document investigations, share findings, and collaborate on threat analysis.
* Entity behaviour - Analysis of user and entity behavior to detect deviations from normal patterns, helping to identify potential insider threats or compromised accounts.
* Threat Intelligence - Integration of external threat data to enhance your security posture, providing context and insights into emerging threats and vulnerabilities.
* MITRE ATT&CK - A framework that categorizes and describes adversary tactics and techniques, helping security teams understand and respond to threats based on real-world attack patterns.

![image](https://github.com/user-attachments/assets/c6221141-3031-4b16-a614-46f8ffe1514b)

Then we have Content Management:
* Content Hub -  A centralized location for accessing and managing various security content, such as analytics rules, workbooks, and playbooks, allowing you to enhance your security operations with pre-built and customizable resources.
* Repositories -  Provides access to different repositories of security content, including templates and shared resources, enabling you to easily find and implement relevant security solutions tailored to your environment.
* Community - A platform for collaboration and knowledge sharing among users, where you can access community-contributed content, best practices, and insights, fostering a collaborative approach to improving security measures and responses.

![image](https://github.com/user-attachments/assets/d96e0a15-bcf8-4358-bb78-6a1d6876ad04)
  
And finally, Configuration:
* Workspace Manager - A tool for managing and configuring your Sentinel workspace, including settings related to data retention, user access, and resource allocation.
* Data Connectors - A feature that allows you to integrate various data sources into Microsoft Sentinel, enabling the collection and analysis of security data from multiple platforms.
* Analytics - A section for configuring and managing analytics rules that detect threats and generate alerts based on predefined criteria and patterns.
* Summary Rules - A feature that allows you to create and manage summary rules to aggregate alerts and incidents, providing a streamlined view of security events.
* Watchlist -  A customizable list of entities or indicators that you want to monitor closely, helping to prioritize investigations and responses to specific threats.
* Automation - A tool for setting up automated responses and workflows to security alerts, enhancing efficiency and reducing response times.
* Settings - A section for configuring various system settings, including user permissions, notification preferences, and integration options, to tailor Microsoft Sentinel to your organization's needs.

![image](https://github.com/user-attachments/assets/cf90a6d7-1578-42a7-bd67-ceadcbb18594)





