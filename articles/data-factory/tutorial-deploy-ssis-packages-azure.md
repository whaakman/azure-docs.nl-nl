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
ms.openlocfilehash: 85777e2a4d1dea5d148a543acd068f8aa1a2335c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-sql-server-integration-services-packages-to-azure"></a>Pakketten van SQL Server Integration Services implementeren in Azure
Azure Data Factory is een cloudgebaseerde gegevensintegratieservice waarmee u gegevensgestuurde werkstromen kunt maken in de cloud. Op deze manier kunt u de verplaatsing en transformatie van gegevens indelen en automatiseren. Met Azure Data Factory kunt u gegevensgestuurde werkstromen (ook wel pijplijnen) maken en plannen die gegevens uit verschillende gegevensarchieven kunnen opnemen en de gegevens kunnen verwerken/transformeren met behulp van rekenservices zoals Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics en Azure Machine Learning. Daarnaast kunt u de uitvoergegevens publiceren naar gegevensarchieven zoals Azure SQL Data Warehouse, zodat BI-toepassingen (business intelligence) ze kunnen gebruiken. 

Deze zelfstudie bevat de stappen voor het inrichten van een Azure SSIS Integration Runtime (IR) in Azure Data Factory. Vervolgens kunt u SQL Server Data Tools (SSDT) of SQL Server Management Studio (SSMS) gebruiken om pakketten van SQL Server Integration Services (SSIS) te implementeren in deze runtime van Azure. In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een data factory maken
> * Een Azure SSIS Integration Runtime maken
> * De Azure SSIS Integration Runtime starten
> * SSIS-pakketten implementeren
> * Het volledige script bekijken

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint. Zie [Overzicht van Integration Runtime in Azure-SSIS](concepts-integration-runtime.md#azure-ssis-integration-runtime) voor algemene informatie over Azure-SSIS IR.

## <a name="prerequisites"></a>Vereisten
- **Azure SQL Database-server**. Als u nog geen databaseserver hebt, maakt u die in Azure Portal voordat u begint. Deze server fungeert als host voor de SSIS-catalogusdatabase (SSISDB). Het wordt aangeraden om de databaseserver in dezelfde Azure-regio te maken als de Integration Runtime. Met deze configuratie kan de Integration Runtime uitvoeringslogboeken wegschrijven naar SSISDB zonder dat hierbij Azure-regio's worden overschreden. 
- **Azure PowerShell**. Volg de instructies in [How to install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Azure PowerShell installeren en configureren). U gebruikt PowerShell om een script uit te voeren voor het inrichten van een Azure SSIS Integration Runtime die SSIS-pakketten uitvoert in de cloud. 

## <a name="launch-windows-powershell-ise"></a>Windows PowerShell ISE starten
Start **Windows PowerShell ISE** met beheerdersbevoegdheden. 

## <a name="create-variables"></a>Variabelen maken
Kopieer en plak het volgende script: geef waarden op voor de variabelen. Voor een lijst met ondersteunde **prijscategorieën** voor Azure SQL Database raadpleegt u [SQL Database resource limits](../sql-database/sql-database-resource-limits.md) (Limieten voor SQL Database-resources).

```powershell
$SubscriptionName = "<Azure subscription name>"
$ResourceGroupName = "<Azure resource group name>"
# Data factory name. Must be globally unique
$DataFactoryName = "<Data factory name>" 
# In public preview, only EastUS amd EastUS2 are supported.
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "<Specify a name for your Azure-SSIS IR>"
$AzureSSISDescription = "<Specify description for your Azure-SSIS IR"
# In public preview, only EastUS and NorthEurope are supported.
$AzureSSISLocation = "EastUS" 
 # In public preview, only Standard_A4_v2, Standard_A8_v2, Standard_D1_v2, Standard_D2_v2, Standard_D3_v2, Standard_D4_v2 are supported
$AzureSSISNodeSize = "Standard_A4_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# In public preview, only 1-8 parallel executions per node are supported.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "<Azure SQL server name>.database.windows.net"
$SSISDBServerAdminUserName = "<Azure SQL server - user name>"
$SSISDBServerAdminPassword = "<Azure SQL server - user password>"
# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "<pricing tier of your Azure SQL server. Examples: Basic, S0, S1, S2, S3, etc.>" 
```

## <a name="validate-the-connection-to-database"></a>Valideren van verbinding met de database
Voeg het volgende script toe om server.database.windows.net van uw Azure SQL Database-server te valideren. 

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

## <a name="log-in-and-select-subscription"></a>Aanmelden en abonnement selecteren
Voeg de volgende code toe aan het script om u aan te melden en uw Azure-abonnement te selecteren: 

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt op de locatie `westeurope`.

Als uw resourcegroep al bestaat, hoeft u deze code niet naar het script te kopiëren. 

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
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode 
```

## <a name="start-integration-runtime"></a>Integration Runtime starten
Voer de volgende opdracht uit om de Azure SSIS Integration Runtime te starten: 

```powershell
write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")                                  
```
Het duurt **20-30 minuten** voordat deze opdracht is voltooid. 

## <a name="deploy-ssis-packages"></a>SSIS-pakketten implementeren
Gebruik nu SQL Server Data Tools (SSDT) of SQL Server Management Studio (SSMS) om uw SSIS-pakketten te implementeren in Azure. Maak verbinding met de Azure SQL-server waarop de SSIS-catalogus (SSISDB) is opgeslagen. De naam van de Azure SQL-server moet deze indeling hebben: `<servername>.database.windows.net` (voor Azure SQL Database). 

Raadpleeg de volgende artikelen uit de SSIS-documentatie: 

- [Deploy, run, and monitor an SSIS package on Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) (SSIS-pakket implementeren, uitvoeren en bewaken in Azure)   
- [Connect to SSIS catalog on Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) (Verbinding maken met een SSIS-catalogus in Azure)
- [Schedule package execution on Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) (Pakketuitvoering plannen in Azure)
- [Connect to on-premises data sources with Windows authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Verbinding maken met on-premises gegevensbronnen met Windows-verificatie) 

## <a name="full-script"></a>Volledige script
In deze versie moet u PowerShell gebruiken om een instantie van de Azure SSIS Integration Runtime in te richten die SSIS-pakketten uitvoert in de cloud. Het is momenteel niet mogelijk om deze runtime in te richten met behulp van Azure Portal. 

Met het PowerShell-script in deze sectie wordt er een instantie van de Azure SSIS Integration Runtime geconfigureerd die SSIS-pakketten uitvoert in de cloud. Nadat u dit script hebt uitgevoerd, kunt u SSIS-pakketten implementeren en uitvoeren in de Microsoft Azure-cloud waarbij SSISDB wordt gehost in Azure SQL Database.

1. Start Windows PowerShell ISE (Integrated Scripting Environment).
2. Voer in ISE de volgende opdracht uit vanaf de opdrachtprompt.    
    ```powershell
    Set-ExecutionPolicy Unrestricted -Scope CurrentUser
    ```
3. Kopieer het PowerShell-script in deze sectie en plak het in ISE.
4. Geef de juiste waarden op voor de scriptparameters in de sectie 'SSIS in Azure specifications' aan het begin van het script. Deze parameters worden in de volgende sectie beschreven.
5. Voer het script uit. De opdracht `Start-AzureRmDataFactoryV2IntegrationRuntime` aan het einde van het script neemt **20-30 minuten** in beslag.

> [!NOTE]
> Het script maakt verbinding met uw Azure SQL-database om de SSIS-catalogusdatabase (SSISDB) voor te bereiden. Het script configureert ook machtigingen en instellingen voor uw VNet, indien opgegeven, en verbindt de nieuwe instantie van de Azure SSIS Integration Runtime met het VNet.

Voor een lijst met ondersteunde **prijscategorieën** voor Azure SQL Database raadpleegt u [SQL Database resource limits](../sql-database/sql-database-resource-limits.md) (Limieten voor SQL Database-resources).

```powershell
$SubscriptionName = "<Azure subscription name>"
$ResourceGroupName = "<Azure resource group name>"
# Data factory name. Must be globally unique
$DataFactoryName = "<Data factory name>" 
# In public preview, only EastUS amd EastUS2 are supported.
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "<Specify a name for your Azure-SSIS (IR)>"
$AzureSSISDescription = "<Specify description for your Azure-SSIS IR"
# In public preview, only EastUS and NorthEurope are supported.
$AzureSSISLocation = "EastUS" 
 # In public preview, only Standard_A4_v2, Standard_A8_v2, Standard_D1_v2, Standard_D2_v2, Standard_D3_v2, Standard_D4_v2 are supported
$AzureSSISNodeSize = "Standard_A4_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# In public preview, only 1-8 parallel executions per node are supported.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "<Azure SQL server name>.database.windows.net"
$SSISDBServerAdminUserName = "<Azure SQL server - user name>"
$SSISDBServerAdminPassword = "<Azure SQL server - user password>"
# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "<pricing tier of your Azure SQL server. Examples: Basic, S0, S1, S2, S3, etc.>" 

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

Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                        -Location $DataFactoryLocation `
                        -Name $DataFactoryName

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
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode

write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="join-azure-ssis-ir-to-a-vnet"></a>Azure-SSIS IR toevoegen aan een VNet
Als u een Azure SQL Managed Instance (private preview) gebruikt voor het hosten van de catalogus van SQL Server Integration Services (SSIS) binnen een virtueel netwerk (VNet), moet u uw Azure-SSIS Integration Runtime ook toevoegen aan hetzelfde virtuele netwerk. In Azure Data Factory versie 2 (preview) kunt u uw Azure-SSIS Integration Runtime toevoegen aan een klassiek VNet. Zie [Join an Azure-SSIS integration runtime to a virtual network](join-azure-ssis-integration-runtime-virtual-network.md) (Een Azure-SSIS Integration Runtime toevoegen aan een virtueel netwerk) voor meer informatie.

Zie [Een Azure SSIS Integration Runtime maken](create-azure-ssis-integration-runtime.md) voor een volledig script voor het maken van een Azure-SSIS Runtime die wordt toegevoegd aan een VNet.

## <a name="monitor-and-manage-azure-ssis-ir"></a>Azure-SSIS IR controleren en beheren
Zie de volgende artikelen voor meer informatie over het controleren en beheren van een Azure-SSIS IR. 

- [Monitor an Azure-SSIS integration runtime](monitor-integration-runtime.md#azure-ssis-integration-runtime) (Een Azure SSIS Integration Runtime controleren)
- [Manage an Azure-SSIS integration runtime](manage-azure-ssis-integration-runtime.md) (Een Azure SSIS Integration Runtime beheren)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd: 

> [!div class="checklist"]
> * Een data factory maken.
> * Een Azure SSIS Integration Runtime maken
> * De Azure SSIS Integration Runtime starten
> * SSIS-pakketten implementeren
> * Het volledige script bekijken

Ga naar de volgende zelfstudie als u wilt weten hoe u on-premises gegevens kopieert naar de cloud: 

> [!div class="nextstepaction"]
>[Gegevens in de cloud kopiëren](tutorial-copy-data-dot-net.md)
