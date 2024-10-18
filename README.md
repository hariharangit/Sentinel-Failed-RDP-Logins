Mapping failed logins in Azure Sentinel using IP Geolocation:

Cyber attacks leveraging Remote Desktop Protocol (RDP) are more common. In this blog, we'll discuss about this by setting up a honeypot virtual machine (VM) and utilizing Azure Sentinel with third-party APIs, we can effectively track RDP attack attempts from around the globe.

Introduction:

We'll use Azure Sentinel, IP Geolocation API, PowerShell script to create this honeypot.
By configuring a VM and connecting the Log Analytics Workspace to the sentinel and retrieving the IP of the failed logins using a PowerShell script and mapping the IP using a third party API.

Gearing up the VM:

Before we dive into the details, make sure you have an Azure account. If you don’t have one yet, you can easily sign up for a free account. If you have an active student email, you can receive a $100 credit to explore Azure and try out more products in the Azure portal. Additionally, remember to delete any resource groups created in this VM to prevent unwanted usage of your credits.

Creating a VM and Log Analytics Workspace in Azure Sentinel

We start by setting up a new Virtual Machine (VM) in Azure, which will function as our “honeypot.”
we’ll choose an operating system that is frequently targeted; Windows servers are a popular option. Next, we’ll configure the networking settings for our VM. It’s essential for a honeypot to be reachable by potential attackers, so we’ll adjust the network security group to permit inbound Remote Desktop Protocol (RDP) connections. After configuring the networking, we’ll deploy the VM. Once Azure finishes the deployment, our honeypot will be operational and poised to attract interest from threat actors online.

Creating VM - Basic 5.png
Deployed VM  2.pngAfter setting up our functional honeypot, it’s crucial to establish a system for gathering and analyzing its data. This can be done by creating an Azure Log Analytics workspace and leveraging Microsoft Defender for Cloud to capture all raw data as logs.

The next step is to link the newly created Log Analytics workspace to our virtual machine. By making this connection, the workspace serves as the primary location for the VM’s logs, streamlining the log collection process for further analysis.

Connecting the VM data with Azure Sentinel:

Azure Sentinel is a cloud-native Security Information and Event Management (SIEM) service from Microsoft. To configure Sentinel, we access Azure Sentinel and integrate it with our current Log Analytics workspace. Next, we connect to our honeypot VM and disable the firewall settings to allow unrestricted internet access. We can confirm this by pinging the system's IP address using the command prompt within the VM.

Using Powershell to retrieve data and map the IP:

A PowerShell script is created to analyze failed RDP attack logs from the Windows Event Viewer on the VM and enhance these logs with geographic data about the attackers using a third-party API (ipgeolocation.io).
VM - Event Viewer Security Panel.pngInitially, the script reviews the Windows event logs for unsuccessful RDP login attempts. For every failed attempt, it retrieves the IP address of the suspected attacker. Then, it forwards these IP addresses to the third-party API, which provides the geographical information associated with each IP address.
API - Geo Location of Users trying to connect.pngA custom log is set up in our Log Analytics workspace to connect the failed RDP attack logs with geolocation data from our VM. After the custom log is established and synchronized with the VM, we can access the event viewer data that shows the failed RDP logs along with geolocation details by querying the custom log within our workspace.

Next, we head to Azure Sentinel to create a new workbook for our threat map. Utilizing Kusto Query Language (KQL) within this workbook, we formulate a query that retrieves failed RDP attempts along with the corresponding IP addresses and geographic locations from our custom log.
Data Extraction - 1st.png![[Map Workbook Config.png

Example Map:
Map.pngIf we wait for few more hours, we can cature more data:
Sample Map Josh.png

Conclusion:
Since the honeypot VM presents vulnerabilities to the internet, it rapidly attracts attention from attackers globally. Examining the gathered data uncovers trends, frequency, and geographic spread of cyber threats. By tracking the number of attacks originating from particular IP addresses or ranges, we can identify repeat offenders, helping us to comprehend their strategies and possibly mitigate these ongoing threats. Remember, experimenting with security projects like this can consume Azure credits quickly. It’s important to delete all the resources when you’re done.

Credits:
This guide has provided a detailed walkthrough of the intriguing project shared by JoshMadakor.
Thank you for your Guidance.

Thank you for reading, and I hope you found this information helpful!
