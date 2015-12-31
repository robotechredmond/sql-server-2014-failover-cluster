# Create a SQL Server 2014 AlwaysOn FCI Cluster in an existing Azure VNET and an existing Active Directory instance

This template will create a SQL Server 2014 Always On FCI Cluster using the PowerShell DSC Extension in an existing Azure Virtual Network and Active Directory environment.

This template creates the following resources:

+	Three Storage Accounts for VM disks
+ One Storage Account for Azure Files shares for database and logs
+	One Internal Load Balancer
+	Three VMs in a Windows Server Cluster, two VMs run SQL Server 2014 and the third is a File Share Witness for the Cluster
+	One Availability Set for the SQL and Witness VMs, configured with three Update Domains and three Fault Domains

A a SQL Server client listener is created in the cluster using the Internal Load Balancer.

To deploy the required Azure VNET and Active Directory infrastructure, if not already in place, you may use <a href="https://github.com/Azure/azure-quickstart-templates/tree/master/active-directory-new-domain-ha-2-dc">this template</a>.

# Known Issues

This template is serial in nature for deploying some of the resources, due to some issues between the platform agent and the DSC extension which cause problems when multiple VM and\or extension resources are deployed concurrently.

## Notes

+ 	The default settings for compute require that you have at least 9 cores of free quota to deploy.

+ 	The images used to create this deployment are
	+ 	SQL Server - Latest SQL Server 2014 SP1 on Windows Server 2012 R2 Image
	+ 	Witness - Latest Windows Server 2012 R2 Image

+ 	The scripts that configure this deployment have only been tested with these image versions and may not work on other images.

+	To successfully deploy this template, be sure that the subnet to which the SQL VMs are being deployed already exists on the specified Azure virtual network, AND this subnet should be defined in Active Directory Sites and Services for the appropriate AD site in which the closest domain controllers are configured.

+ To deploy the required Azure VNET and Active Directory infrastructure, if not already in place, you may use <a href="https://github.com/Azure/azure-quickstart-templates/tree/master/active-directory-new-domain-ha-2-dc">this template</a>.

Click the button below to deploy from the portal

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Frobotechredmond%2Fsql-server-2014-failover-cluster%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>


## Deploying from PowerShell

For details on how to install and configure Azure Powershell see [here].(https://azure.microsoft.com/en-us/documentation/articles/powershell-install-configure/)

Launch a PowerShell console

Ensure that you are in Resource Manager Mode

```PowerShell

Switch-AzureMode AzureResourceManager

```
Change working folder to the folder containing this template

```PowerShell

New-AzureResourceGroup -Name "<new resourcegroup name>" -Location "<new resourcegroup location>"  -TemplateParameterFile .\azuredeploy-parameters.json -TemplateFile .\azuredeploy.json

```
