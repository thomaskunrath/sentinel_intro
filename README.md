# sentinel_intro
Microsoft Sentinel is an advanced, cloud-based security event and information management (SEIM) system that delivers real-time analysis of security alerts for both your cloud and on-premises resources.
Here I will try to demonstrate:
* What you should know.
* How to get Started: Lab setup
* Onboarding to Sentinel - bare with me, I am not a expert!
   * A quick overview of it's Menus and Sub-Menus
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

And here I finish with the Onboarding, covering:
* What you should know.
* How to get Started: Lab setup
* Menu and Sub-Menu explanation
********************************************************************************************************************
In this next section I will talk about KQL, Kusto Query Language. KQL is the language Azure Log Analytics and MS Sentinel uses. It is used proactively in its Hunting Queries as well as out-of-box Rules which a built-in within Sentinel instance that we just depolyed. More can be explored at dataexplorer.azure.com.

By accessing dataexplorer.azure.com we can find samples data sets to work with. In this case I will pick PopulationData table. To get in there, on the left-hand side hit help > Samples > Storm_Events > PopulationData. By inputting "PopulationData" into the box and hitting "Run" it will return all the records on the table which is 52 records.

![image](https://github.com/user-attachments/assets/ff1395b0-8a58-4b4f-b192-f15f2b0d159a)

If I, for example use the "Pipe Operator" ```( | )``` and add it to my previous string (which was PopulationData) along for example, ```count``` it would summarise for me the amount or occurencies. That's usefull when looking at very long tables.
So then, in this case we go from ```PopulationData``` to ```PopulationData | count```, then hit "Run".

![image](https://github.com/user-attachments/assets/f95147a5-e5bb-462c-8c16-ef59b59d6bc5)

I can also filter events by metrics that are relevant to it. Let's say I want to display the top 10 events in the States of that region. 
So my string would be:
```
PopulationData
| top 10 by State
```
I will then return to me:

![image](https://github.com/user-attachments/assets/912d666a-4f0e-4ca2-847c-610f3b73fcd5)

As we can notice, its got a "auto-fill" function:

![image](https://github.com/user-attachments/assets/ee04400c-9309-4edb-8d5d-e640827c7d5b)

Let's add that to see what is going to happen.
So our code would be then:
```
PopulationData
| top 10 by State asc
```
And that will list out 10 States is ascending order by name as per image below.

![image](https://github.com/user-attachments/assets/22e3e1f4-19c6-4419-8606-38fb90c23b1b)

We can also filter a little further where. In this example let's try to show the top 10 stated which have more than a million people.
So our code should be:
```
PopulationData
| top 10 by State asc
| where Population > 1000000
```
As we can see if the image below, the returned search only lists the States that have over a million people living in there.

![image](https://github.com/user-attachments/assets/4cebe809-e01a-4f3d-b02c-88ab7467980c)

Of course, as noted, this example is not about Hunting threats, it is just a overview on how KQL works, and can be used to Threat Hunting. 

********************************************************************************************************************

Now I will touch bases with Connecting MS Services, which in a nutshell is how we are going to ingest data into Sentinel via Data Connectors.
That can be done via the menu Content Management > Content Hub.

![image](https://github.com/user-attachments/assets/f8dcf80f-458e-42f5-a220-91496c423075)

In this example I will demonstrate how to install MS Defender for Endpoint.
As the library of connectors is quite large, and we know what we want to install, and provider, let's narrow down the search.
First, hit "Provider" uncheck all and type Microsoft, tick it. That will limit out search around MS only products.
Then on the Seach Box input "Defender". 

![image](https://github.com/user-attachments/assets/4b0cd5f9-720d-466a-b5ad-679ee81eb39e)

Click on the Connector we spoke about, a button will show up allowing to install it. Also take some time to read what is in the Description pane, it will tell you what you are bringing in by installing a certain Connector.

![image](https://github.com/user-attachments/assets/7cde9872-ae52-4827-abcc-137674e7fca1)

Once I hit Install (bottom right corner of the picture above), then it's done, and installed in a matter of seconds.

![image](https://github.com/user-attachments/assets/95cf0c1f-276f-4c12-87fa-87f594a40ccf)

Now that MS Defender for EndPoint is installed we can go over to Configuration > Data Connectors and it will be there. I won't go in it's configuration because due to my account limitations (I think) I am not able to further set it up.

![image](https://github.com/user-attachments/assets/b607803d-84be-497c-b6df-a9f03fe7b9bb)

But let's try another Data Connector, one that provides a Workbook to be able to take a look at it. In this case let's install Sentinel SOAR (Security Orchestration, Automation, and Response). Similarly as above let's take the same steps to try to find it amongst all those connectors. 

![image](https://github.com/user-attachments/assets/d45eade4-a11b-4a9c-a1d7-bd18d20e2259)

Once installed, click on Manage (bottom right of your screen). Let's install the "PostMessageSlack-OnAlert" and take a look at the Workbook. Why? Well, this is a home lab. I'm experimenting. 
What does the "PostMessageSlack-OnAlert" do? It will post a message in a Slack channel when an alert is created in MS Sentinel. 

![image](https://github.com/user-attachments/assets/7a600ce9-32c5-4938-b7f1-1564fc81de4b)

Once you hit the Configuration button you will be taken to this screen:

![image](https://github.com/user-attachments/assets/dd9b6a72-eff3-41b2-a47e-442750b06d01)

Then hit Create Playbook. You will be prompted to setup the Basics, Connections, then you will be able to Review it and then finally create it. 

![image](https://github.com/user-attachments/assets/491d17d3-23dc-46bf-8834-1d8a351ffbc1)

Connections: - As expected, Sentinel and Slack.

![image](https://github.com/user-attachments/assets/a4bdb707-d1f1-4cf7-b4ee-87f805ea9ac7)

Then hit Create Playbook.

![image](https://github.com/user-attachments/assets/e4cb2920-0234-4039-b982-72b56b2c1cf2)

You will get this message while it is being deployed:

![image](https://github.com/user-attachments/assets/3effea65-57e4-40d4-a1f9-51e6da408238)

********************************************************************************************************************

This is a ongoin study Lab. As I progress it will be updated.
Thank you for your time for glancing over it.














