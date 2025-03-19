<p align="center">
  <img src="https://github.com/user-attachments/assets/3735c0e1-2d24-4189-bf99-ea8d490f33c9" width="150" height="auto">
  <h1 align="center">Implementing Azure Backup and Disaster Recovery</h1>
</p>

#### *In this tutorial, we will:*
*•	Configure Azure Backup for VM protection and recovery.*

*•	Set up VM replication for disaster recovery.*

*•	Monitor backups and analyze logs for management.*

#### Tasks:
 1. Configuring Azure Backup Storage with a Recovery Vault.
 2. Enabling and Managing Virtual Machine Backups.
 3. Tracking and Analyzing Azure Backup Performance.
 4. Configuring Disaster Recovery through VM Replication.

## Task 1: Configuring Azure Backup Storage with a Recovery Vault

Create a Recovery Services vault (a management entity for backup and recovery operations) to securely store VM backups. Assumes a VM and virtual network (VNet) are already deployed.

1.	In the Azure portal, search for and select "Recovery Services vaults".
2.	On the "Recovery Services vaults" blade, click "+ Create".
3.	On the "Create Recovery Services vault" blade, configure:
    -  Subscription: Select your subscription.
    -  Resource group: Choose the resource group with your VM.
    -  Vault name: Enter a unique name (e.g., "BackupVault").
    -  Region: Select the same region as your VM (e.g., "(US) East US").

<p align="center">
<img src="https://github.com/user-attachments/assets/d114e8db-17f1-4dae-8be7-132993f3bf57">
</p>

4.	Leave other settings at their defaults, then click "Review + Create".
5.	After validation passes, click "Create". (Deployment takes a few minutes.)
6.	Once deployed, click "Go to Resource".
7.	In the "Settings" section, click "Properties".
8.	Under the "Backup Configuration" label, click "Update".

<p align="center">
<img src="https://github.com/user-attachments/assets/ccdf319b-8b95-4730-aa29-af10e9812ea9">
</p>

9.	On the "Backup Configuration" blade, verify "Geo-redundant" is selected (replicates data to a secondary region for redundancy). Note: This can only be set if no backup items exist. Enable "Cross Region Restore" to allow restoration in a paired region, then click "Apply".

<p align="center">
<img src="https://github.com/user-attachments/assets/75a6b592-2545-47ae-a537-90804c9d772e">
</p>

10.	Under "Security Settings" > "Soft Delete and security settings", click "Update".
11.	Configure:
    -  Enable soft delete for cloud workloads: Uncheck (disables soft delete).
    -  Enable soft delete and security settings for hybrid workloads: Uncheck. (Disabling soft delete avoids conflicts with custom disaster recovery replication and failover.)
12.	Click "Update".

<p align="center">
<img src="https://github.com/user-attachments/assets/be87de72-456d-4ea5-bbdf-e1011ac9538d">
</p>

## Task 2: Enabling and Managing Virtual Machine Backups.

Enable backups for a VM and define a custom retention policy using the Recovery Services vault.

1.	On the "Recovery Services vault" blade, click "Overview".
2.	Click "+ Backup".
3.	On the "Backup Goal" blade, configure:
    -  Where is your workload running?: Select "Azure".
    -  What do you want to backup?: Select "Virtual machine".
4.	Click "Backup".

<p align="center">
<img src="https://github.com/user-attachments/assets/511ab692-9c26-48f4-918d-591b587ee5c2">
</p>

5.	In "Backup policy", select "Create a new policy" and configure:
    -  Policy name: Enter "Backup1".
    -  Frequency: Select "Daily".
    -  Time: Set to "12:00 AM".
    -  Timezone: Select your local time zone.
    -  Retain instant recovery snapshot(s) for: Set to "2 Day(s)". (Note: "Standard" policy is sufficient; "Enhanced" is optional.)
6.	Click "OK".
7.	In the "Virtual Machines" section, click "Add".
8.	Select your VM, click "OK", then on the "Backup" blade, click "Enable backup". (This takes about 2 minutes.)

<p align="center">
<img src="https://github.com/user-attachments/assets/73420444-c237-410c-b95a-30c603c27192">
</p>

<p align="center">
<img src="https://github.com/user-attachments/assets/f402c007-6a82-4df6-8dc8-b10b0471b5da">
</p>

<p align="center">
<img src="https://github.com/user-attachments/assets/a2e5e6c3-1bfd-4746-b034-88c1b9ca8946">
</p>

9.	Click "Go to resource".
10.	In the "Protected items" section, click "Backup items", then click the "Azure virtual machine" entry.
11.	Select "View details" for the VM and review "Backup Pre-Check" and "Last Backup Status". (The backup should be pending.)
12.	Click "Backup now", accept the default "Retain Backup Till" date, and click "OK".

<p align="center">
<img src="https://github.com/user-attachments/assets/21b60186-8f6d-4e8c-a8aa-f70d17e84e5f">
</p>

<p align="center">
<img src="https://github.com/user-attachments/assets/76284a2f-f24d-4f9f-ba66-b72ae63ef8c6">
</p>

## Task 3: Tracking and Analyzing Azure Backup Performance.

Set up a storage account to collect backup logs and monitor performance using diagnostic settings.

1.	In the Azure portal, search for and select "Storage accounts".
2.	Click "Create".
3.	Configure the storage account settings, ensuring it’s in the same region as your VM and vault (e.g., "(US) East US"), then click "Review + create".
4.	Click "Create".

<p align="center">
<img src="https://github.com/user-attachments/assets/a448fbfe-1d7c-46fd-bed0-88485aed56e0">
</p>

5.	Search for and select your Recovery Services vault (e.g., "BackupVault").
6.	In the "Monitoring" blade, click "Diagnostic settings".
7.	Click "Add diagnostic setting".
8.	Configure:
    -  Name: Enter "Info Vault".
    -  Logs and Metrics categories: Check: "Azure Backup Reporting Data". "Addon Azure Backup Job Data". "Addon Azure Backup Alert Data". "Azure Site Recovery Jobs". "Azure Site Recovery Events". "Health".
    -  Destination details: Check "Archive to a storage account" and select the storage account created in this task.
9.	Click "Save".

<p align="center">
<img src="https://github.com/user-attachments/assets/ae92bf60-696d-4d86-af68-f701dc257e5d">
</p>

<p align="center">
<img src="https://github.com/user-attachments/assets/bc443050-6607-4b6d-96fc-964bc34cae13">
</p>

10.	Return to the "Monitoring" blade and click "Backup jobs".
11.	Locate and review the backup operation for your VM.

<p align="center">
<img src="https://github.com/user-attachments/assets/ce426584-f7ec-4474-a29f-429145fe8a39">
</p>

## Task 4: Configuring Disaster Recovery through VM Replication.

Enable VM replication to a secondary region using Azure Site Recovery (a disaster recovery service) to minimize downtime during failures.

1.	In the Azure portal, search for and select "Recovery Services vaults".
2.	Click "+ Create".
3.	On the "Create Recovery Services vault" blade, configure:
    -  Subscription: Select your subscription.
    -  Resource group: Choose or create a resource group.
    -  Vault name: Enter a unique name (e.g., "DRVault").
    -  Region: Select a different region from your VM (e.g., "(US) West US").
4.	Click "Review + Create". After validation passes, click "Create". (Deployment takes a few minutes.)

<p align="center">
<img src="https://github.com/user-attachments/assets/72748036-20d5-4fc7-8691-16cf25239691">
</p>

5.	Search for and select your VM.
6.	In the "Backup + Disaster recovery" blade, click "Disaster recovery".
7.	On the "Basics" tab, note that Azure Site Recovery is being used.

<p align="center">
<img src="https://github.com/user-attachments/assets/871f7d0c-4d87-43db-9c14-762b97993f65">
</p>

8.	Click the "Advanced settings" tab.
9.	Review the auto-populated resource selections:
    -  Subscription: Verify your subscription.
    -  VM resource group: Verify your VM’s resource group.
    -  Virtual network: Verify your VNet.
    -  Availability: Verify availability settings.
10.	In "Storage settings", click "Show details" and review:
    -  Churn for the VM: "Normal churn".
    -  Cache storage account: Auto-populated. (Both must be populated to pass validation.)
11.	In "Replication settings", click "Show details" and confirm the recovery vault in the secondary region (e.g., "DRVault") is selected.
12.	Click "Review + Start replication", then click "Start replication". (Replication takes 20-30 minutes.)

<p align="center">
<img src="https://github.com/user-attachments/assets/033feb44-eb03-44bc-8719-d0cbc11efec9">
</p>

<p align="center">
<img src="https://github.com/user-attachments/assets/6185cd18-aa25-4036-b1de-280b89d697db">
</p>

13.	Once replication completes, search for and select your secondary region’s Recovery Services vault (e.g., "DRVault").
14.	In the "Protected items" section, click "Replicated items".
15.	Verify the VM shows "Healthy" under "Replication health". (The status starts at 0% synchronization and updates to "Protected" after initial synchronization.)

<p align="center">
<img src="https://github.com/user-attachments/assets/d5da0a36-b3c6-46a5-8d8d-41d52a566eb3">
</p>
