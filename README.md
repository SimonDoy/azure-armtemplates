# Introduction
This repo provides some examples of Azure Resource Manager Templates.

I hope you find it useful.

# Samples
## Complete Sample
This ARM template provides an example which deploys the following:
- Sql Server.
- App Service Plan.
- App Service for API.
- App Service for User Interface.
- App Service for Background job processor.
- App Service for Azure Functions.
- Application Insights.
- Storage accounts.
- Azure Media Services account.
- Azure Service Bus.

## Key Vault Sample
This ARM template provides an example which deploys the following:
- Storage Account.
- Key Vault.
- Storage Account connection string to Key Vault.


# Deploying ARM Templates
To start deploying ARM templates using PowerShell. A PowerShell shell will need to be running. The Azure RM or the newer Az module will need to be installed. This can be installed or download from [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureRM/6.13.1)

Login to the Azure Subscription using Login-AzureRmAccount.
Ensure the PowerShell shell's is running in the directory with the ARM Template and Template parameter files.

Ensure the correct Azure Subscription has been selected.

If a resource group does not already exist to host your infrastructure then create one by:
- New-AzureRmResourceGroup -Name "[name of resource group]" -Location "[location]";
- e.g New-AzureRmResourceGroup -Name i365-arm-test -Location "North Europe";

Run the resource group deployment to deploy the ARM template:

- New-AzureRmResourceGroupDeployment -Name "[deployment-name]" -ResourceGroupName [name of resource group] -Mode Incremental -TemplateFile .\[arm template file name] -TemplateParameterFile .\[arm template parameter file name];

- e.g. New-AzureRmResourceGroupDeployment -Name "i365-arm-test-keyvault" -ResourceGroupName i365-arm-test -Mode Incremental -TemplateFile .\i365-keyvaultsample-components.template.json -TemplateParameterFile .\i365-keyvaultsample-live.parameters.json;

Sometimes, the ARM template has secrets that need to be provided for example sql admin passwords. For those I suggest that they are supplied as direct parameters to the New-AzureRmResourceGroupDeployment cmd. Using [Tab] you will be able to cycle through the parameters for the ARM template.

For secrets / passwords you will need to convert the password string to a Secure-String using ConverTo-SecureString.
Here is an example:
- $sqladminpwd = ConvertTo-SecureString -Force -AsPlainText -String "mypasswordhere";

e.g. New-AzureRmResourceGroupDeployment -Name "i365-arm-test-keyvault" -ResourceGroupName i365-arm-test -Mode Incremental -TemplateFile .\i365-keyvaultsample-components.template.json -TemplateParameterFile .\i365-keyvaultsample-live.parameters.json -sql-pwd $sqladminpwd;
