**Cybersecurity Homelab: Tracking attackers by geo location**

<img width="1920" height="599" alt="map screenshot" src="https://github.com/user-attachments/assets/cc03a59a-83ef-4a31-adc8-419d65ddfe99" />


Description:

This cybersecurity project involves deploying a virtual machine using Azure, disabling it's firewall to make it vulnerable to attackers, and then using Azure's Log Analytics Workspace to query the attacks. The final result is a dashboard of a geo map of the attacks by location, which is created using a .json script.

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

2\. Create a resource group:

![]
<img width="1920" height="834" alt="screenshot1" src="https://github.com/user-attachments/assets/d93a2103-2ce5-4ae5-9701-fdd6810be611" />

A resource group is like a folder that stores your virtual machine(s).
Search for resource groups, then select. This will take you to a list of resource groups. If new to Azure, this will be empty. To create your first resource group, go to Create.


<img width="1199" height="719" alt="screenshot2" src="https://github.com/user-attachments/assets/dd463927-08c7-4395-94c7-a204b46f180a" />

Name it: RG-SOC-Lab
Under subscription will be your Azure subscription that you created. Under region, choose your region, or the closest region to you.
