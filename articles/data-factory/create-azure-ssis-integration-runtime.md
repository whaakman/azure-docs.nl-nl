---
title: Host zichzelf integratie runtime maken in Azure Data Factory | Microsoft Docs
description: Meer informatie over hoe u de SQL Server opgeslagen Procedure activiteit kunt gebruiken om aan te roepen, een opgeslagen procedure in een Azure SQL Database of een Azure SQL Data Warehouse van een Data Factory-pijplijn.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: spelluru
ms.openlocfilehash: 7796df75d811ad34967aee66478eae992fd449fe
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="create-an-azure-ssis-integration-runtime-in-azure-data-factory"></a>Een Azure-SSIS-integratie runtime maken in Azure Data Factory
Dit artikel bevat stappen voor het inrichten van een Azure-SSIS-integratie runtime in Azure Data Factory. Vervolgens kunt u SQL Server Data Tools (SSDT) of SQL Server Management Studio (SSMS) gebruiken om pakketten van SQL Server Integration Services (SSIS) te implementeren in deze runtime van Azure.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Documentatie van versie 1 van Data Factory](v1/data-factory-introduction.md).

De zelfstudie: [zelfstudie: pakketten van SQL Server Integration Services (SSIS) implementeren in Azure](tutorial-deploy-ssis-packages-azure.md) ziet u hoe u een Azure-SSIS integratie Runtime (IR) maken met behulp van Azure SQL Database als archief voor SSIS-catalogus. Dit artikel wordt op de zelfstudie wordt uitgebreid en ziet u hoe u het volgende doen: 

- Azure SQL beheerd exemplaar (afgeschermd voorbeeld) gebruiken voor het hosten van een catalogus SSIS (SSISDB-database).
- Azure-SSIS-IR lid maken van een Azure-netwerk (VNet). 

Zie voor informatie over het samenvoegen van een Azure-SSIS-IR met een VNet en een VNet configureren in Azure-portal, [Azure SSIS-IR Join naar VNet](join-azure-ssis-integration-runtime-virtual-network.md). 

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement**. Als u geen abonnement hebt, kunt u een [gratis proefaccount](http://azure.microsoft.com/pricing/free-trial/) maken.
- **Azure SQL Database server** of **SQL Server Managed Instance (private preview) (Extended Private Preview)**. Als u nog geen databaseserver hebt, maakt u die in Azure Portal voordat u begint. Deze server fungeert als host voor de SSIS-catalogusdatabase (SSISDB). Het wordt aangeraden om de databaseserver in dezelfde Azure-regio te maken als de Integration Runtime. Met deze configuratie kan de Integration Runtime uitvoeringslogboeken wegschrijven naar SSISDB zonder dat hierbij Azure-regio's worden overschreden. Noteer de prijscategorie van uw Azure SQL-server. Zie voor een lijst met ondersteunde Prijscategorieën voor Azure SQL Database, [limieten voor SQL-Database](../sql-database/sql-database-resource-limits.md).
- **Klassiek virtueel netwerk (VNet) (optioneel)**. U hebt een virtueel netwerk (VNet) in Azure nodig als ten minste aan een van de volgende voorwaarden wordt voldaan:
    - U host de SSIS-catalogusdatabase in een SQL Server Managed Instance (private preview) die deel uitmaakt van een VNet.
    - U wilt verbinding maken met on-premises gegevensarchieven vanuit SSIS-pakketten die worden uitgevoerd in een Azure SSIS Integration Runtime.
- **Azure PowerShell**. Volg de instructies in [How to install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Azure PowerShell installeren en configureren). U gebruikt PowerShell om een script uit te voeren voor het inrichten van een Azure SSIS Integration Runtime die SSIS-pakketten uitvoert in de cloud. 

> [!NOTE]
> Zie [Producten beschikbaar per regio](https://azure.microsoft.com/regions/services/) voor een lijst met regio's die worden ondersteund door Azure Data Factory V2 en Azure-SSIS Integration Runtime. Vouw **Gegevens en analyses** uit om **Data Factory V2** en **SSIS Integration Runtime** te zien.

## <a name="use-azure-portal"></a>Azure Portal gebruiken

### <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).    
2. Klik op **Nieuw** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**. 
   
   ![Nieuw -> DataFactory](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. In de **nieuwe gegevensfactory** pagina **MyAzureSsisDataFactory** voor de **naam**. 
      
     ![Nieuwe data factory-pagina](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   De naam van de Azure-gegevensfactory moet **wereldwijd uniek** zijn. Als u de volgende fout ontvangen, wijzig de naam van de gegevensfactory (bijvoorbeeld yournameMyAzureSsisDataFactory) en probeert u het opnieuw. Zie [Data Factory - naamgevingsregels](naming-rules.md) artikel voor naamgevingsregels voor Data Factory-artefacten.
  
       `Data factory name “MyAzureSsisDataFactory” is not available`
3. Selecteer het Azure-**abonnement** waarin u de gegevensfactory wilt maken. 
4. Voer een van de volgende stappen uit voor de **Resourcegroep**:
     
      - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst. 
      - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in.   
         
      Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
4. Selecteer **V2 (Preview)** als de **versie**.
5. Selecteer de **locatie** voor de gegevensfactory. Alleen de locaties die worden ondersteund voor het maken van data Factory worden weergegeven in de lijst.
6. Selecteer **Vastmaken aan dashboard**.     
7. Klik op **Create**.
8. Op het dashboard ziet u de volgende tegel met de status: **Gegevensfactory implementeren**. 

    ![tegel met de status 'gegevensfactory implementeren'](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
9. Nadat het maken voltooid is, ziet u de **Data Factory** pagina zoals in de afbeelding.
   
   ![Startpagina van de gegevensfactory](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
10. Klik op **auteur & Monitor** starten van de Data Factory gebruikersinterface (UI) op een afzonderlijke tabblad. 

### <a name="provision-an-azure-ssis-integration-runtime"></a>Een Azure-SSIS-integratie runtime inrichten

1. Klik in de pagina aan de slag, **configureren SSIS-integratie Runtime** tegel. 

   ![SSIS-integratie Runtime tegel configureren](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)
2. In de **algemene instellingen** pagina van **integratie Runtime-instellingen**, moet u de volgende stappen uitvoeren: 

   ![Algemene instellingen](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    1. Geef een **naam** voor de integratie-runtime.
    2. Selecteer de **locatie** voor de integratie-runtime. Alleen ondersteunde locaties worden weergegeven.
    3. Selecteer de **grootte van het knooppunt** die is geconfigureerd met de SSIS-runtime.
    4. Geef de **aantal knooppunten** in het cluster.
    5. Klik op **Volgende**. 
1. In de **SQL-instellingen**, moet u de volgende stappen uitvoeren: 

    ![SQL-instellingen](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    1. Geef de Azure **abonnement** die de Azure SQL-server is. 
    2. Selecteer uw Azure SQL-server voor de **catalogus Database servereindpunt**.
    3. Voer de **beheerder** gebruikersnaam.
    4. Voer de **wachtwoord** voor de beheerder.  
    5. Selecteer de **servicelaag** voor de SSISDB-database. De standaardwaarde is Basic.
    6. Klik op **Volgende**. 
1.  In de **geavanceerde instellingen** pagina, selecteert u een waarde op voor de **maximale parallelle uitvoeringen Per knooppunt**.   

    ![Geavanceerde instellingen](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)    
5. Deze stap is **optionele**. Als u een klassiek virtueel netwerk (VNet) die u wilt dat de runtime integratie hebt wilt koppelen, selecteert u de **selecteert u een VNet voor uw Azure-SSIS-integratie-runtime voor het koppelen van Azure-services voor het configureren van VNet machtigingsinstellingen toestaan** optie en voer de volgende stappen uit: 

    ![Geavanceerde instellingen met VNet](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)    

    1. Geef de **abonnement** is met het klassieke VNet. 
    2. Selecteer de **VNet**. <br/>
    4. Selecteer de **Subnet**.<br/> 
1. Klik op **voltooien** starten van het maken van Azure SSIS-integratie runtime. 

    > [!IMPORTANT]
    > - Dit proces duurt ongeveer 20 minuten om te voltooien
    > - De Data Factory-service maakt verbinding met uw Azure SQL Database voorbereiden van de catalogus SSIS-database (SSISDB). Het script configureert ook machtigingen en instellingen voor uw VNet, indien opgegeven, en verbindt de nieuwe instantie van de Azure SSIS Integration Runtime met het VNet.
7. In de **verbindingen** venster overschakelen naar **integratie Runtimes** indien nodig. Klik op **vernieuwen** de status te vernieuwen. 

    ![Status van het maken](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)
8. Gebruik de koppelingen onder **acties** kolom die u wilt bewaken, stoppen/starten, bewerken of verwijderen van de integratie-runtime. Gebruik de laatste koppeling om weer te geven van JSON-code voor de integratie-runtime. De knoppen bewerken en verwijderen zijn alleen beschikbaar wanneer de IR is gestopt. 

    ![Azure SSIS-IR - acties](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)        
9. Klik op **Monitor** koppeling onder **acties**.  

    ![Azure SSIS-IR - details](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-details.png)
10. Als er een **fout** gekoppeld met de Azure SSIS-IR, ziet u het aantal fouten op deze pagina en de koppeling om weer te geven om details over de fout. Als de SSIS-catalogus al op de databaseserver bestaat, ziet u bijvoorbeeld een fout die aangeeft het bestaan van de SSISDB-database dat.  
11. Klik op **integratie Runtimes** boven om te navigeren wilt maken van de vorige pagina overzicht van alle integratie runtimes die zijn gekoppeld aan de gegevensfactory.  

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Azure SSIS-integratie runtimes in de portal

1. In de Azure Data Factory-gebruikersinterface overschakelen naar de **bewerken** tabblad **verbindingen**, en ga vervolgens naar **integratie Runtimes** tabblad om bestaande integratie runtimes in uw Data factory. 
    ![Bestaande belastingdienst weergeven](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)
1. Klik op **nieuw** voor het maken van een nieuwe Azure SSIS-IR 

    ![Integratie runtime via het menu](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)
2. Klik op om een Azure-SSIS-integratie runtime **nieuw** zoals weergegeven in de afbeelding. 
3. Selecteer in het venster integratie Runtime-instellingen **Lift en shift bestaande SSIS-pakketten worden uitgevoerd in Azure**, en klik vervolgens op **volgende**.

    ![Geef het type van integratie runtime](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)
4. Zie de [inrichten van een Azure-SSIS-integratie runtime](#provision-an-azure-ssis-integration-runtime) sectie voor de resterende stappen om een Azure-SSIS-IR in te stellen

## <a name="use-azure-powershell"></a>Azure PowerShell gebruiken

### <a name="create-variables"></a>Variabelen maken
Definieer variabelen voor gebruik in het script in deze zelfstudie:

```powershell
# Azure Data Factory version 2 information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS integration runtime name]"
$AzureSSISDescription = "This is my Azure-SSIS integration runtime"
$AzureSSISLocation = "EastUS" 
# In public preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_A4_v2" 
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# In public preview, only 1-8 parallel executions per node are supported.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or your Azure SQL Managed Instance (private preview) server endpoint]"
$SSISDBServerAdminUserName = "[your server admin username]"
$SSISDBServerAdminPassword = "[your server admin password]"

# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "[your Azure SQL Database pricing tier. Examples: Basic, S0, S1, S2, S3, etc.]"

# Remove these the following two OPTIONAL variables if you are using Azure SQL Database. 
# These two parameters apply if you are using VNet and Azure SQL Managed Instance (private preview). 
# Get the following information from the properties page for your Classic Virtual Network in the Azure portal
# It should be in the format: $VnetId = "/subscriptions/<Azure Subscription ID>/resourceGroups/<Azure Resource Group>/providers/Microsoft.ClassicNetwork/virtualNetworks/<Class Virtual Network Name>"

# OPTIONAL: In public preview, only classic virtual network (VNet) is supported.
$VnetId = "[your VNet resource ID or leave it empty]" 
$SubnetName = "[your subnet name or leave it empty]" 

```
### <a name="log-in-and-select-subscription"></a>Aanmelden en abonnement selecteren
Voeg de volgende code het script aan te melden en selecteer uw Azure-abonnement: 

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName
```

### <a name="validate-the-connection-to-database"></a>Valideren van verbinding met de database
Het volgende script voor het valideren van uw Azure SQL Database-server server.database.windows.net of uw eindpunt beheerde exemplaar van Azure SQL (afgeschermd voorbeeld) server toevoegen. 

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

### <a name="configure-virtual-network"></a>Virtueel netwerk configureren
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

### <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt op de locatie `westeurope`.

```powershell
New-AzureRmResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

### <a name="create-a-data-factory"></a>Een gegevensfactory maken
Voer de volgende opdracht uit om een data factory te maken:

```powershell
Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

### <a name="create-an-integration-runtime"></a>Een Integration Runtime maken
Voer de volgende opdracht voor het maken van een Azure-SSIS-integratie runtime die SSIS-pakketten in Azure wordt uitgevoerd: het script te gebruiken in het gedeelte op basis van het type database (vs Azure SQL Database. Azure SQL Managed-exemplaar (private preview)) die u gebruikt. 

#### <a name="azure-sql-database-to-host-the-ssisdb-database-ssis-catalog"></a>Azure SQL Database voor het hosten van de SSISDB-database (SSIS-catalogus) 

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

U hoeft niet te waarden doorgeven voor VNetId en Subnet, tenzij u toegang tot lokale gegevens moet, dat wil zeggen, hebt u lokale gegevens bronnen/bestemmingen in uw SSIS-pakketten. U moet de waarde voor de parameter CatalogPricingTier doorgeven. Zie voor een lijst met ondersteunde Prijscategorieën voor Azure SQL Database, [limieten voor SQL-Database](../sql-database/sql-database-resource-limits.md).

#### <a name="azure-sql-managed-instance-private-preview-to-host-the-ssisdb-database"></a>Azure SQL beheerd-exemplaar (afgeschermd voorbeeld) voor het hosten van de SSISDB-database

```powershell
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -CatalogServerEndpoint $SSISDBServerEndpoint `
                                            -CatalogAdminCredential $serverCreds `
                                            -Description $AzureSSISDescription `
                                            -Location $AzureSSISLocation `
                                            -NodeSize $AzureSSISNodeSize `
                                            -NodeCount $AzureSSISNodeNumber `
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                            -VnetId $VnetId `
                                            -Subnet $SubnetName
```

U moet waarden voor VnetId en Subnet parameters met Azure SQL beheerd-instantie (afgeschermd voorbeeld) die lid wordt van een VNet doorgeven. De parameter CatalogPricingTier geldt niet voor Azure SQL beheerd-exemplaar. 

### <a name="start-integration-runtime"></a>Integration Runtime starten
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


### <a name="full-script"></a>Volledige script
Hier is de volledige-script dat wordt gemaakt van een Azure-SSIS-IR en deze gekoppeld is aan een VNet. Dit script wordt ervan uitgegaan dat u van Azure SQL beheerd exemplaar (MI) gebruikmaakt voor het hosten van de SSIS-catalogus. 

```powershell
# Azure Data Factory version 2 information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS integration runtime name]"
$AzureSSISDescription = "This is my Azure-SSIS integration runtime"
$AzureSSISLocation = "EastUS" 
# In public preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_A4_v2" 
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# In public preview, only 1-8 parallel executions per node are supported.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or your Azure SQL Managed Instance (private preview) server endpoint]"
$SSISDBServerAdminUserName = "[your server admin username]"
$SSISDBServerAdminPassword = "[your server admin password]"

# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "[your Azure SQL Database pricing tier. Examples: Basic, S0, S1, S2, S3, etc.]"

## Remove these two OPTIONAL variables if you are using Azure SQL Database. 
## These two parameters apply if you are using VNet and Azure SQL Managed Instance (private preview). 
# In public preview, only classic virtual network (VNet) is supported.
$VnetId = "[your VNet resource ID or leave it empty]" 
$SubnetName = "[your subnet name or leave it empty]" 

Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

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
```



## <a name="deploy-ssis-packages"></a>SSIS-pakketten implementeren
Gebruik nu SQL Server Data Tools (SSDT) of SQL Server Management Studio (SSMS) om uw SSIS-pakketten te implementeren in Azure. Maak verbinding met de Azure SQL-server waarop de SSIS-catalogus (SSISDB) is opgeslagen. De naam van de Azure SQL-server moet deze indeling hebben: &lt;servernaam&gt;. database.windows.net (voor Azure SQL Database). Zie het artikel [Pakketten implementeren](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server) voor instructies. 

## <a name="next-steps"></a>Volgende stappen
Zie de andere Azure-SSIS-IR-onderwerpen in deze documentatie:

- [Azure-SSIS-integratie Runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel bevat conceptuele informatie over de integratie runtimes in het algemeen met inbegrip van de Azure-SSIS-IR 
- [Zelfstudie: SSIS-pakketten implementeren in Azure](tutorial-deploy-ssis-packages-azure.md). Dit artikel biedt stapsgewijze instructies voor het maken van een Azure-SSIS IR en maakt gebruik van een Azure SQL database voor het hosten van de SSIS-catalogus. 
- [Een Azure-SSIS IR controleren](monitor-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel leest u hoe u informatie over een Azure-SSIS IR ophaalt. Daarnaast bevat het artikel beschrijvingen van statuswaarden die worden gebruikt in de geretourneerde informatie. 
- [Een Azure-SSIS IR beheren](manage-azure-ssis-integration-runtime.md). In dit artikel leest u hoe u een Azure-SSIS IR stopt, start of verwijdert. Er wordt ook uitgelegd hoe u een Azure-SSIS IR kunt uitschalen door meer knooppunten toe te voegen aan de IR. 
- [Een Azure-SSIS IR toevoegen aan een VNet](join-azure-ssis-integration-runtime-virtual-network.md). Dit artikel bevat algemene informatie over het toevoegen van een Azure-SSIS IR aan een virtueel netwerk (VNet) van Azure. Er wordt ook beschreven hoe u Azure Portal gebruikt om VNet te configureren voor het deelnemen van een Azure-SSIS IR aan het VNet. 