<p align="center">
  <img src="https://github.com/user-attachments/assets/3735c0e1-2d24-4189-bf99-ea8d490f33c9" width="150" height="auto">
  <h1 align="center">Implementing Azure Backup and Disaster Recovery</h1>
</p>

#### *In this tutorial, we will:*
*- Configure Azure Backup for VM protection and recovery.*

*- Set up VM replication for disaster recovery.*

*- Monitor backups and analyze logs for management.*

#### Tasks:
 1. Configuring Azure Backup Storage with a Recovery Vault.
 2. Enabling and Managing Virtual Machine Backups.
 3. Tracking and Analyzing Azure Backup Performance.
 4. Configuring Disaster Recovery through VM Replication.

## Task 1: Configuring Azure Backup Storage with a Recovery Vault

This task involves creating a Recovery Services vault to manage VM backups securely. We already have a VM and a Vnet deployed.

1.  In the Azure portal, search for and select Recovery Services vaults and, on the Recovery Services vaults blade, click "+ Create".
2.  On the Create Recovery Services vault blade, specify Subscription, Resource group, Vault Name, Region (for this case is the same region where our VMs are located).

<p align="center">
<img src="https://github.com/user-attachments/assets/d114e8db-17f1-4dae-8be7-132993f3bf57">
</p>

3.  We will leave the defaults for everything else. Click "Review + Create", ensure that the validation passes and then click "Create". The deployment should take a couple of minutes. When the deployment is completed, click "Go to Resource".
4. In the Settings section, click "Properties" and select the "Update" link under "Backup Configuration" label.

<p align="center">
<img src="https://github.com/user-attachments/assets/ccdf319b-8b95-4730-aa29-af10e9812ea9">
</p>

5. On the "Backup Configuration" blade, ensure that the setting of Geo-redundant is in place. This setting can be configured only if there are no existing backup items. The Cross Region Restore option allows you to restore data in a secondary, Azure paired region. Click "Apply".

<p align="center">
<img src="https://github.com/user-attachments/assets/75a6b592-2545-47ae-a537-90804c9d772e">
</p>

6.  Select the "Update" link under "Security Settings" > "Soft Delete and security" settings label. Leave the settings as follows, Enable soft delete for cloud workloads: Unchecked, Enable soft delete and security settings for hybrid workloads: Unchecked (we will have a custom Disaster Recovery Strategy, which has its own replication and failover mechanisms. Disabling Soft Delete ensures that our recovery functions without conflicts when performing failover, failback, or cleanup operations). Click "Update".

<p align="center">
<img src="https://github.com/user-attachments/assets/be87de72-456d-4ea5-bbdf-e1011ac9538d">
</p>

## Task 2: Enabling and Managing Virtual Machine Backups.

This task covers enabling VM backups and defining customized retention policies.

1.  On the Recovery Services vault blade, click "Overview", then click "+ Backup". On the "Backup Goal" blade, specify the following settings, Where is your workload running?: Azure, What do you want to backup?: Virtual machine (you could backup a file share or an SQL Database also). Then select "Backup".

<p align="center">
<img src="https://github.com/user-attachments/assets/511ab692-9c26-48f4-918d-591b587ee5c2">
</p>

2.  Notice there are two Policy sub types: Enhanced and Standard, for this project we only need Standard. In "Backup policy", select "Create a new policy" and we'll use the following settings, Policy name: Backup1, Frequency: Daily, Time: 12:00 AM, Timezone: the name of your local time zone, Retain instant recovery snapshot(s) for: 2 Days(s). Click "OK" to create the policy and then, in the Virtual Machines section, select "Add". Select the VM, click "OK", and then back on the Backup blade, click "Enable backup". This should take approximately 2 minutes.

<p align="center">
<img src="https://github.com/user-attachments/assets/73420444-c237-410c-b95a-30c603c27192">
</p>

<p align="center">
<img src="https://github.com/user-attachments/assets/f402c007-6a82-4df6-8dc8-b10b0471b5da">
</p>

<p align="center">
<img src="https://github.com/user-attachments/assets/a2e5e6c3-1bfd-4746-b034-88c1b9ca8946">
</p>

3.  Select "Go to resource" and in the Protected items section, click "Backup items", and then click the "Azure virtual machine" entry. Select the "View details" link for the VM, and review the values of the Backup Pre-Check and Last Backup Status entries. Notice the backup is pending. Select "Backup now", accept the default value in the Retain Backup Till drop-down list, and click "OK".

<p align="center">
<img src="https://github.com/user-attachments/assets/21b60186-8f6d-4e8c-a8aa-f70d17e84e5f">
</p>

<p align="center">
<img src="https://github.com/user-attachments/assets/76284a2f-f24d-4f9f-ba66-b72ae63ef8c6">
</p>

## Task 3: Tracking and Analyzing Azure Backup Performance.

This task involves setting up a storage account to collect backup logs for monitoring and reporting.

1.  From the Azure portal, search for and select "Storage accounts". On the Storage accounts page, select "Create". Fill the settings making sure the storage is in the same region as our previous tasks. Select "Review + create", then select "Create".

<p align="center">
<img src="https://github.com/user-attachments/assets/a448fbfe-1d7c-46fd-bed0-88485aed56e0">
</p>

2.  Search and select your Recovery Services vault. In the Monitoring blade, select "Diagnostic Settings" and then select "Add diagnostic setting". We will name the setting "Info Vault" and place a checkmark next to the following "Logs" and "Metrics" categories: Azure Backup Reporting Data, Addon Azure Backup Job Data, Addon Azure Backup Alert Data, Azure Site Recovery Jobs, Azure Site Recovery Events, Health. Also in the "Destination details" place a checkmark next to Archive to a storage account. Select the storage account that you deployed in this task. Select "Save".


<p align="center">
<img src="https://github.com/user-attachments/assets/ae92bf60-696d-4d86-af68-f701dc257e5d">
</p>

<p align="center">
<img src="https://github.com/user-attachments/assets/bc443050-6607-4b6d-96fc-964bc34cae13">
</p>

3.  Return to your Recovery Services vault, in the Monitoring blade select "Backup jobs". Locate and review the backup operation for the VM.

<p align="center">
<img src="https://github.com/user-attachments/assets/ce426584-f7ec-4474-a29f-429145fe8a39">
</p>

## Task 4: Configuring Disaster Recovery through VM Replication.

In this taks, we will enable virtual machine replication to ensure disaster recovery and minimize downtime during failures.

1.  In the Azure portal, search for and select "Recovery Services vaults" and, on the Recovery Services vaults blade, click "+ Create". Fill the settings and make sure that this vault will be on a different Region as our previous tasks. Click "Review + Create", ensure that the validation passes and then click "Create". The deployment should take a couple of minutes.

<p align="center">
<img src="https://github.com/user-attachments/assets/72748036-20d5-4fc7-8691-16cf25239691">
</p>

2.  Search for and select the VM. In the "Backup + Disaster recovery" blade, select "Disaster recovery". On the Basics tab, notice that we are accessing the Azure Site Recovery service.

<p align="center">
<img src="https://github.com/user-attachments/assets/871f7d0c-4d87-43db-9c14-762b97993f65">
</p>

3.  Move to the "Advanced settings" tab. Resource selections have been made for you, it is important to review them. Verify your subscription, vm resource group, virtual network, and availability. In Storage settings select "Show details" and review, Churn for the vm: Normal churn, Cache storage account: auto populated. It is important that both of these settings be populated, or the validation will fail. In Replication settings select Show details. Notice your recovery resources vault in region 2 was automatically selected. Select "Review + Start replication" and then "Start replication" (Enabling replication will take 20-30 minutes).

<p align="center">
<img src="https://github.com/user-attachments/assets/033feb44-eb03-44bc-8719-d0cbc11efec9">
</p>

<p align="center">
<img src="https://github.com/user-attachments/assets/6185cd18-aa25-4036-b1de-280b89d697db">
</p>

4.  Once the replication is complete, search for and locate your Recovery Services Vault for the second region. In the Protected items section, select Replicated items. Check that the virtual machine is showing as healthy for the replication health. Note that the status will show the synchronization (starting at 0%) status and ultimately show Protected after the initial synchronization completes.

<p align="center">
<img src="https://github.com/user-attachments/assets/d5da0a36-b3c6-46a5-8d8d-41d52a566eb3">
</p>
