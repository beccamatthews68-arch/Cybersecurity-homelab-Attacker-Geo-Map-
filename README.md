**Cybersecurity Homelab: Tracking attackers by geo location**

<img width="1920" height="599" alt="map screenshot" src="https://github.com/user-attachments/assets/cc03a59a-83ef-4a31-adc8-419d65ddfe99" />


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

2\. Create a resource group:

![]
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

