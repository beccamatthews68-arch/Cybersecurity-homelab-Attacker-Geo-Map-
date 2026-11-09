**Cybersecurity Homelab: Tracking attackers by geo location**

<img width="3840" height="1198" alt="map screenshot" src="https://github.com/user-attachments/assets/cc03a59a-83ef-4a31-adc8-419d65ddfe99" />


Description:

This cybersecurity project involves deploying a virtual machine using Azure, disabling it's firewall to make it vulnerable to attackers, and then using Azure's Log Analytics Workspace to query the attacks. The final result is a dashboard showcasing a geo map of the attacks by location, which is created using a .json script.


Environments and technologies used:

- Microsoft Azure
- Remote Desktop Connection
- Powershell
- Log Analytics Workspace
- Microsoft Sentinel
- Microsoft Defender Workbook

Operating Systems used:

- Windows 10
- Windows 11 Pro


**Step 1: deploy a virtual machine in Azure**

1\. Create your Azure Subscription.

Go to portal.azure.com and create an account. Then in the search bar, search 'subscriptions'. Finally, hit 'add'. You can leave all settings as default-then hit review + create.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a209843e-e5c1-43de-b72a-93b68df07d15" />



2\. Create a resource group:


<img width="1920" height="834" alt="screenshot1" src="https://github.com/user-attachments/assets/d93a2103-2ce5-4ae5-9701-fdd6810be611" />

A resource group is like a folder that stores your virtual machine(s).
Search for resource groups, then select. This will take you to a list of resource groups. If new to Azure, this will be empty. To create your first resource group, go to Create.


<img width="1199" height="719" alt="screenshot2" src="https://github.com/user-attachments/assets/dd463927-08c7-4395-94c7-a204b46f180a" />

Name it: RG-SOC-Lab
Under subscription will be your Azure subscription that you created. Under region, choose your region, or the closest region to you.

3. Create your virtual machine

<img width="1920" height="663" alt="screenshot3" src="https://github.com/user-attachments/assets/a9f8ee78-4ea6-4e7d-bec3-df69e445ef2a" />

In the search bar, search for ‘virtual machines’. Then select ‘create’.

<img width="1107" height="1080" alt="screenshot4" src="https://github.com/user-attachments/assets/a1380a38-8afe-42c4-9ad0-56955e15c7be" />

Azure will automatically list it in the resource group you just created.
Name the virtual machine something ambiguous to attackers, such as ‘CORP-NET-EAST-1’. This sounds like a realistic corporate virtual machine name.

For region, find the nearest one to you with available space. The closer your virtual machine to your location, the less expensive it is to run.

Try to choose a size of 2vcpus or above, as this will make the machine run quicker. Don’t worry about the pricing listed-this is only an estimate for if the virtual machine was running 24/7-we will only run it for the duration of the project.

<img width="1112" height="1080" alt="screenshot5" src="https://github.com/user-attachments/assets/b2eab767-487d-4a84-a341-8281e741df59" />
Under Administrator account: set your username as something simple such as ‘azureuser’, then choose a strong password. I would recommend noting these down in notepad (but do not do this in real life!).

<img width="1920" height="1080" alt="screenshot6" src="https://github.com/user-attachments/assets/399f9348-7380-49b8-b952-349458cddb06" />
Under networking, Azure will automatically create a new Virtual network. Ensure your subnet falls under that network in the drop down menu.

Make sure the ‘I accept the license terms’ box is checked under the basic tab, then hit review + create, then create.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3822dbaf-f371-4118-aae0-8a48d29f8be6" />

Now the virtual machine is being deployed…

After it is deployed, hit ‘go to resource’. You’ll be greeted by this page overview of the virtual machine:

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fadda7c8-4a95-4d3e-b6a1-4c10b250bca1" />

Step 2: Configure the virtual machine to be vulnerable to attackers 


1. First, lets connect to your new VM, then test its connectivity to the internet.
   
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/97607ebd-758d-4be4-9860-d4e5985e9f12" />


On the VM’s overview page, find the public IP address. Then, if on a Windows Operating System, open Remote Desktop Connection by searching for it in the Windows Search bar. Mac users use a different software for this.
Enter the IP address, followed by the username and password you created for the VM.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/30b70abd-c998-42b4-9413-0e73c8e5ade6" />

Once logged into the VM, go to the start menu and type ‘powershell’. Choose to run as administrator.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/dda5bb38-9866-4c6d-a0fc-c7a517cd88f2" />

Inside powershell, type ‘ping 8.8.8.8’

The ‘ping’ command is used to test connectivity between a device and another, or the wider network. Here we used the IP address 8.8.8.8 as it is google’s ip address, but you can try this with any other common IP address.

You should get four lines of replies from the google IP address, meaning your VM is connected to the network.

2.  Disable the firewall externally via Azure

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/747a2124-3660-4ee5-a752-62203a81f032" />

Find your virtual machine by searching ‘virtual machines’ in Azure.
Go into the Network settings, and you will see a rule that allows the firewall to block inbound traffic. Delete this rule by clicking the trash can symbol. Then select ‘create new rule-inbound’.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6ac86750-3bc1-45de-8ac9-4a6a217e0784" />

Under source, choose Any, because we want to allow any inbound traffic from any source (good or bad!). For Source Port ranges, put * as this means all. The same applies for Destination, and Destination Port ranges.

For priority, put 100-This is a system that lists rules by priority-the higher the number, the higher the priority.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/656d434e-6ee7-4d9b-a939-3c33f3b2cdd4" />

Name the rule something dramatic such as ‘DANGER_AllowAnyCustomPorts’ as this makes it easy for us to spot that it is a rule that makes our VM vulnerable to attackers.

3. Disable the firewall internally inside of the Virtual Machine
   
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0a0f5caa-8053-4b3f-823f-911d781bb8a8" />

In the windows search bar, open ‘Windows Firewall’ and disable the firewalls. Now your virtual machine is fully vulnerable to attackers.

4. Observe failed log in attempts via Event Viewer

Next, we are going to get an understanding of the Event Viewer. The Event Viewer logs activity relating to the Virtual Machine, such as successful and failed log in attempts.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b6299dda-dfae-46e5-9fc5-0e8b142c1acd" />

Start by logging out of your VM. Then purposely fail to log in 4 times using a random username and password.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/34d7619d-0645-4a61-a024-117732be6654" />

After this, log in with your correct credentials, then inspect the Event Viewer. You should see 4 activity logs with the Event ID 4625. This ID means a failed log in attempt.




Stage 3: Querying the log in attempts

1. Create Log Analytics Workspace

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/046bcaa1-05db-4cf4-9ac7-7734dd33dd49" />

2. Create Sentinel and connect it to the LAW 

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b468409c-7cb6-4eda-947d-016b5352b9e7" />

You may run into this error when attempting to create a Microsoft Sentinel. This error means that the region for the LAW that you have chosen is not supported in Sentinel. My workaround for this was creating a LAW in a supported region-you may need to google which these are, but the region of France Central worked for me, as shown below.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2350153b-3551-46cd-80f7-e15da2a41501" />


3. Install ‘Windows Security Events via AMA’ to the Sentinel

This creates an agent that acts as the link between your virtual machine and the Microsoft Sentinel.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ce9f4737-0c1c-4941-b3f0-fbc9248a475e" />

Once your Microsoft Sentinel is installed, go to Content Manager > Content Hub.
In the content hub, search ‘Windows Security Events’ and click install.

Once this is installed, the status will update to ‘installed’. Now select ‘Windows Security Events via AMA’ (this is the agent) and hit ‘manage’. 

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b7ce9862-2bff-49d6-92a0-6673c6039caa" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e5da7afd-d458-43e9-ad01-b8364154aeaa" />

Select ‘Create data collection rule’ . Name the rule ‘DCR Wizard’, and ensure that it is added to the correct subscription and resource group.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/811ddea3-18e0-4b42-bd65-850ead449c78" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9297b381-4e12-437e-ae9b-7513149e4f7d" />

Under the VM’s extensions and applications, the agent we created should now appear.


4. Querying the data in KQL 

Return to your Log Analytics Workspace, open log, choose the KQL mode (this is the query language, another example of a query language is SQL) from simple mode, and run this query: SecurityEvent

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a81c0e11-1a14-4135-8273-2e76eb0bbd76" />

This will return all of the security events on the virtual machine, in the form of a table spreadsheet.

Next, run this query:

SecurityEvent
| where EventID ==4625

This will filter the results where the EventID is 4625, meaning that it will only return events where the log in attempt failed. This is because we are only interested in data from our attackers!

We can also narrow the columns down to what we are interested in, such as TimeGenerated, Account, Computer, EventID, Activity, and IpAddress:

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bb311498-1d84-4519-95d0-994f2e761659" />


Stage 4: converting Ip addresses to geo locations

1. Creating a watchlist in Microsoft Sentinel and adding the watchlist to our KQL query

In order to create our geo map of attackers, we need to convert our attacker’s ipaddresses to locations. Luckily, there is a document that achieves just this:

https://raw.githubusercontent.com/joshmadakor1/lognpacific-public/refs/heads/main/misc/geoip-summarized.csv 
Next, create a Watchlist within Microsoft Sentinel:

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/349f4eca-4f18-46fb-874f-8517d5b5776c" />

Name and alias the watchlist ‘geoip’

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2daaf8d8-52b0-4c8c-bfe6-d979d4ec72f8" />

Under source, select Local File. The file type we downloaded was a csv file. Leave the number of lines before row with headings as 0. For SearchKey, choose network.


Now lets return to our query, adding in the new geoip watchlist (this adds in data such as country, city, which we need for our geo map. Notice that the original WindowsSecurity table from our Virtual Machine had IpAddress data but no location data-this is why we needed to add the geoip document).

<img width="800" height="400" alt="cropped query" src="https://github.com/user-attachments/assets/e7ab19fc-e23c-4b1e-be87-e3f196b0dbf2" />



Lets break this query down to understand it.

Let GeoIPDB_FULL = _GetWatchlist(“geoip”);
-retrieves the geoip watchlist

let WindowsEvents = SecurityEvent
-Creates a table for our data

| where EventID == 4625
-filters EventID by failed log in attempts only

| order by TimeGenerated desc
-orders the data by the time that the event occurred, in descending order (most recent event first)

| evaluate ipv4_lookup(GeoIPDB_FULL, IpAddress, network);
 -performs a geo-location lookup of an IPv4 address against a specific database to determine its associated network segment. 

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4dc49e94-0246-42fd-9780-bfaf28e5260e" />

We can also narrow our search down to one specific IpAddress-pick a random one and input it into a ‘where’ clause, similar to how we filtered results by EventID = 4625.


Final stage: Uploading a map.json script into a workbook to generate our geoip map


1. First, return to your Microsoft Sentinel, and create a new workbook.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ad7f6150-9e6d-486b-86ad-2e5603f6ae77" />

2. Go into edit mode to delete the current query, then add data source + visualisation.

Then, go into the advanced editor, and paste this .json script:

```
{
        "type": 3,
        "content": {
        "version": "KqlItem/1.0",
        "query": "let GeoIPDB_FULL = _GetWatchlist(\"geoip\");\nlet WindowsEvents = SecurityEvent;\nWindowsEvents | where EventID == 4625\n| order by TimeGenerated desc\n| evaluate ipv4_lookup(GeoIPDB_FULL, IpAddress, network)\n| summarize FailureCount = count() by IpAddress, latitude, longitude, cityname, countryname\n| project FailureCount, AttackerIp = IpAddress, latitude, longitude, city = cityname, country = countryname,\nfriendly_location = strcat(cityname, \" (\", countryname, \")\");",
        "size": 3,
        "timeContext": {
                "durationMs": 2592000000
        },
        "queryType": 0,
        "resourceType": "microsoft.operationalinsights/workspaces",
        "visualization": "map",
        "mapSettings": {
                "locInfo": "LatLong",
                "locInfoColumn": "countryname",
                "latitude": "latitude",
                "longitude": "longitude",
                "sizeSettings": "FailureCount",
                "sizeAggregation": "Sum",
                "opacity": 0.8,
                "labelSettings": "friendly_location",
                "legendMetric": "FailureCount",
                "legendAggregation": "Sum",
                "itemColorSettings": {
                "nodeColorField": "FailureCount",
                "colorAggregation": "Sum",
                "type": "heatmap",
                "heatmapPalette": "greenRed"
                }
        }
        },
        "name": "query - 0"
}
```

3. Select ‘done editing’ and your attacker map should materialise! Name it something dramatic such as ‘Attacker geo map’.


Congratulations-you are now officially a spy!

<img width="3840" height="1198" alt="map screenshot" src="https://github.com/user-attachments/assets/cc03a59a-83ef-4a31-adc8-419d65ddfe99" />


