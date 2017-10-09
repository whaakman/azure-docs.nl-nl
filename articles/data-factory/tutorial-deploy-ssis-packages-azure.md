---
title: SSIS-pakketten implementeren in Azure | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u SSIS-pakketten implementeert in een Azure SSIS Integration Runtime van Azure Data Factory.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: powershell
ms.topic: hero-article
ms.date: 09/06/2017
ms.author: spelluru
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: b16bb831f7901c5c690ca20c24eb059188c68750
ms.contentlocale: nl-nl
ms.lasthandoff: 09/25/2017

---
# <a name="deploy-sql-server-integration-services-packages-to-azure"></a>Pakketten van SQL Server Integration Services implementeren in Azure
Azure Data Factory is een cloudgebaseerde service voor gegevensintegratie waarmee u gegevensgestuurde werkstromen kunt maken in de cloud. Op deze manier kunt u de verplaatsing en transformatie van gegevens indelen en automatiseren. Met Azure Data Factory kunt u gegevensgestuurde werkstromen (ook wel pijplijnen) maken en plannen die gegevens uit verschillende gegevensarchieven kunnen opnemen en de gegevens kunnen verwerken/transformeren met behulp van rekenservices zoals Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics en Azure Machine Learning. Daarnaast kunt u de uitvoergegevens publiceren naar gegevensarchieven zoals Azure SQL Data Warehouse, zodat BI-toepassingen (business intelligence) ze kunnen gebruiken. 

Deze zelfstudie bevat de stappen voor het inrichten van een Azure SSIS Integration Runtime in Azure Data Factory. Vervolgens kunt u SQL Server Data Tools (SSDT) of SQL Server Management Studio (SSMS) gebruiken om pakketten van SQL Server Integration Services (SSIS) te implementeren in deze runtime van Azure. In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een data factory maken
> * Een Azure SSIS Integration Runtime maken
> * De Azure SSIS Integration Runtime starten
> * SSIS-pakketten implementeren
> * Het volledige script bekijken

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten
- **Azure SQL Database server** of **SQL Server Managed Instance (private preview) (Extended Private Preview)**. Als u nog geen databaseserver hebt, maakt u die in Azure Portal voordat u begint. Deze server fungeert als host voor de SSIS-catalogusdatabase (SSISDB). Het wordt aangeraden om de databaseserver in dezelfde Azure-regio te maken als de Integration Runtime. Met deze configuratie kan de Integration Runtime uitvoeringslogboeken wegschrijven naar SSISDB zonder dat hierbij Azure-regio's worden overschreden. 
- **Klassiek virtueel netwerk (VNet) (optioneel)**. U hebt een virtueel netwerk (VNet) in Azure nodig als ten minste aan een van de volgende voorwaarden wordt voldaan:
    - U host de SSIS-catalogusdatabase in een SQL Server Managed Instance (private preview) die deel uitmaakt van een VNet.
    - U wilt verbinding maken met on-premises gegevensarchieven vanuit SSIS-pakketten die worden uitgevoerd in een Azure SSIS Integration Runtime.
- **Azure PowerShell**. Volg de instructies in [How to install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Azure PowerShell installeren en configureren). U gebruikt PowerShell om een script uit te voeren voor het inrichten van een Azure SSIS Integration Runtime die SSIS-pakketten uitvoert in de cloud. 

## <a name="create-variables"></a>Variabelen maken
Definieer variabelen voor gebruik in het script in deze zelfstudie:

```powershell
# Azure Data Factory version 2 information
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$DataFactoryLocation = "EastUS" # In public preview, only EastUS|EastUS2 are supported.

# Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS integration runtime name]"
$AzureSSISDescription = "This is my Azure-SSIS integration runtime"
$AzureSSISLocation = "EastUS" # In public preview, only EastUS|NorthEurope are supported.
$AzureSSISNodeSize = "Standard_A4_v2" # In public preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeNumber = 2 # In public preview, only 1-10 nodes are supported.
$AzureSSISMaxParallelExecutionsPerNode = 2 # In public preview, only 1-8 parallel executions per node are supported.
$VnetId = "[your VNet resource ID or leave it empty]" # OPTIONAL: In public preview, only classic virtual network (VNET) is supported.
$SubnetName = "[your subnet name or leave it empty]" # OPTIONAL: In public preview, only classic VNet is supported.

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or your Azure SQL Managed Instance (private preview) server endpoint]"
$SSISDBServerAdminUserName = "[your server admin username]"
$SSISDBServerAdminPassword = "[your server admin password]"
$SSISDBPricingTier = "[your Azure SQL Database pricing tier, e.g. S3, or leave it empty for Azure SQL Managed Instance (private preview)]" # Not applicable for Azure SQL Managed Instance (private preview)
```

## <a name="validate-the-connection-to-database"></a>Valideren van verbinding met de database
Voeg het volgende script toe voor de validatie van de verbinding met server.database.windows.net of het eindpunt voor uw Azure SQL Managed Instance (private preview) voor uw Azure SQL Database server. 

```powershell
$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID="+ $SSISDBServerAdminUserName +";Password="+ $SSISDBServerAdminPassword
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL DB logical server/Azure SQL MI server, exception: $_"  ;
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}
```

## <a name="login-and-select-subscription"></a>Aanmelden en abonnement selecteren
Voeg de volgende code toe aan het script om u aan te melden en uw Azure-abonnement te selecteren: 

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName
```

## <a name="configure-virtual-network"></a>Virtueel netwerk configureren
Voeg het volgende script toe om automatisch machtigingen/instellingen voor het virtuele netwerk te configureren waarmee uw Azure SSIS Integration Runtime moet worden verbonden.

```powershell
# Register to Azure Batch resource provider
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName "MicrosoftAzureBatch").Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    # Assign VM contributor role to Microsoft.Batch
    New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
}
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt op de locatie `westeurope`.

```powershell
New-AzureRmResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

## <a name="create-a-data-factory"></a>Een data factory maken
Voer de volgende opdracht uit om een data factory te maken:

```powershell
Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

## <a name="create-an-integration-runtime"></a>Een Integration Runtime maken
Voer de volgende opdracht uit om een Azure SSIS Integration Runtime te maken die SSIS-pakketten uitvoert in Azure: 

```powershell
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -CatalogServerEndpoint $SSISDBServerEndpoint `
                                            -CatalogAdminCredential $serverCreds `
                                            -CatalogPricingTier $SSISDBPricingTier `
                                            -Description $AzureSSISDescription `
                                            -Location $AzureSSISLocation `
                                            -NodeSize $AzureSSISNodeSize `
                                            -NodeCount $AzureSSISNodeNumber `
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                            -VnetId $VnetId `
                                            -Subnet $SubnetName
```

## <a name="start-integration-runtime"></a>Integration Runtime starten
Voer de volgende opdracht uit om de Azure SSIS Integration Runtime te starten: 

```powershell
write-host("##### Starting #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")                                  
```
Het duurt **20-30 minuten** voordat deze opdracht is voltooid. 

## <a name="deploy-ssis-packages"></a>SSIS-pakketten implementeren
Gebruik nu SQL Server Data Tools (SSDT) of SQL Server Management Studio (SSMS) om uw SSIS-pakketten te implementeren in Azure. Maak verbinding met de Azure SQL-server waarop de SSIS-catalogus (SSISDB) is opgeslagen. De naam van de Azure SQL-server moet deze indeling hebben: &lt;servernaam&gt;. database.windows.net (voor Azure SQL Database). Zie het artikel [Pakketten implementeren](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server) voor instructies. 

## <a name="full-script"></a>Volledige script
In deze versie moet u PowerShell gebruiken om een instantie van de Azure SSIS Integration Runtime in te richten die SSIS-pakketten uitvoert in de cloud. Het is momenteel niet mogelijk om deze runtime in te richten met behulp van Azure Portal. 

Met het PowerShell-script in deze sectie wordt er een instantie van de Azure SSIS Integration Runtime geconfigureerd die SSIS-pakketten uitvoert in de cloud. Nadat u hebt dit script wordt uitgevoerd, kunt u SSIS-pakketten implementeren en uitvoeren in de Microsoft Azure-cloud met SSISDB gehost in Azure SQL Database of in een SQL Server Managed Instance (private preview).

1. Start Windows PowerShell ISE (Integrated Scripting Environment).
2. Voer in ISE de volgende opdracht uit vanaf de opdrachtprompt.    
    ```powershell
    Set-ExecutionPolicy Unrestricted -Scope CurrentUser
    ```
3. Kopieer het PowerShell-script in deze sectie en plak het in ISE.
4. Geef de juiste waarden op voor de scriptparameters in de sectie 'SSIS in Azure specifications' aan het begin van het script. Deze parameters worden in de volgende sectie beschreven.
5. Voer het script uit. De opdracht `Start-AzureRmDataFactoryV2IntegrationRuntime` aan het einde van het script neemt **20-30 minuten** in beslag.

> [!NOTE]
> Het script maakt verbinding met uw Azure SQL-database of SQL Server Managed Instance (private preview) voor het voorbereiden van de SSIS-catalogusdatabase (SSISDB). Het script configureert ook machtigingen en instellingen voor uw VNet, indien opgegeven, en verbindt de nieuwe instantie van de Azure SSIS Integration Runtime met het VNet.


```powershell
# If your inputs contain PSH special characters, e.g. "$", please precede it with the escape character "`" like "`$". 
# Azure Data Factory v2 information
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$DataFactoryLocation = "EastUS" # In Public Preview, only EastUS|EastUS2 are supported for now

# Azure-SSIS Integration Runtime info - This is Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS Integration Runtime name]"
$AzureSSISDescription = "This is my Azure-SSIS Integration Runtime"
$AzureSSISLocation = "EastUS" # In Public Preview, only EastUS|NorthEurope are supported for now
$AzureSSISNodeSize = "Standard_A4_v2" # In Public Preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported for now
$AzureSSISNodeNumber = 2 # In Public Preview, only 1-10 nodes are supported for now
$AzureSSISMaxParallelExecutionsPerNode = 2 # In Public Preview, only 1-8 parallel executions per node are supported for now
$VnetId = "[your VNet resource ID or leave it empty]" # OPTIONAL: In Public Preview, only Classic VNet is supported for now
$SubnetName = "[your subnet name or leave it empty]" # OPTIONAL: In Public Preview, only Classic VNet is supported for now

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or your Azure SQL Managed Instance (private preview) server endpoint]"
$SSISDBServerAdminUserName = "[your server admin username]"
$SSISDBServerAdminPassword = "[your server admin password]"
$SSISDBPricingTier = "[your Azure SQL Database pricing tier, e.g. S3, or leave it empty for Azure SQL Managed Instance (private preview)]" # Not applicable for Azure SQL Managed Instance (private preview)

##### Validate your Azure SQL Database/Managed Instance (private preview) server ##### 
$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID="+ $SSISDBServerAdminUserName +";Password="+ $SSISDBServerAdminPassword
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL DB logical server/Azure SQL MI server, exception: $_"  ;
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}

##### Login and and select your Azure subscription #####
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

##### Automatically configure VNet permissions/settings for your Azure-SSIS Integration Runtime to join ##### 
# Register to Azure Batch resource provider
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName "MicrosoftAzureBatch").Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    # Assign VM contributor role to Microsoft.Batch
    New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
}

##### Provision your Azure Data Factory  + Azure-SSIS Integration Runtime ##### 
New-AzureRmResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory

$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -CatalogServerEndpoint $SSISDBServerEndpoint `
                                            -CatalogAdminCredential $serverCreds `
                                            -CatalogPricingTier $SSISDBPricingTier `
                                            -Description $AzureSSISDescription `
                                            -Location $AzureSSISLocation `
                                            -NodeSize $AzureSSISNodeSize `
                                            -NodeCount $AzureSSISNodeNumber `
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                            -VnetId $VnetId `
                                            -Subnet $SubnetName
write-host("##### Starting #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force
write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")

##### Query/monitor your Azure-SSIS Integration Runtime #####
#Get-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Status

##### Reconfigure your Azure-SSIS Integration Runtime, e.g. stopping/scaling out to 5 nodes/starting #####
# Stopping your Azure-SSIS Integration Runtime will release its nodes and stop billing
#Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName 

# Scaling out your Azure-SSIS Integration Runtime to 5 nodes
#Set-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -NodeCount 5

# Starting your Azure-SSIS Integration Runtime will allocate its nodes and start billing
#Start-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName

##### Clean up ######
#Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
#Remove-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
#Remove-AzureRmDataFactoryV2 -Name $DataFactoryName -ResourceGroupName $ResourceGroupName -Force
#Remove-AzureRmResourceGroup -Name $ResourceGroupName -Force

```
## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd: 

> [!div class="checklist"]
> * Een data factory maken
> * Een Azure SSIS Integration Runtime maken
> * De Azure SSIS Integration Runtime starten
> * SSIS-pakketten implementeren
> * Het volledige script bekijken

Ga naar de volgende zelfstudie als u wilt weten hoe u on-premises gegevens kopieert naar de cloud: 

> [!div class="nextstepaction"]
>[Gegevens in de cloud kopiëren](tutorial-copy-data-dot-net.md)

