---
title: Azure-SSIS-integratie runtime maken in Azure Data Factory | Microsoft Docs
description: Informatie over het maken van een Azure-SSIS-integratie runtime, zodat u SSIS-pakket in de Azure-cloud uitvoeren kunt.
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
ms.date: 01/22/2018
ms.author: spelluru
ms.openlocfilehash: 6eec344761df67fd8e8b3c6fceedb8ee68e85b9a
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="create-an-azure-ssis-integration-runtime-in-azure-data-factory"></a>Een Azure-SSIS-integratie runtime maken in Azure Data Factory
Dit artikel bevat stappen voor het inrichten van een Azure-SSIS-integratie runtime in Azure Data Factory. Vervolgens kunt u SQL Server Data Tools (SSDT) of SQL Server Management Studio (SSMS) gebruiken om pakketten van SQL Server Integration Services (SSIS) te implementeren in deze runtime van Azure.

De zelfstudie: [zelfstudie: pakketten van SQL Server Integration Services (SSIS) implementeren in Azure](tutorial-deploy-ssis-packages-azure.md) hebt u geleerd hoe u een Azure-SSIS integratie Runtime (IR) maken met behulp van Azure SQL Database als archief voor SSIS-catalogus. Dit artikel wordt op de zelfstudie wordt uitgebreid en ziet u hoe u het volgende doen: 

- Azure SQL beheerd exemplaar (afgeschermd voorbeeld) gebruiken voor het hosten van een catalogus SSIS (SSISDB-database).
- Azure-SSIS-IR lid maken van een Azure-netwerk (VNet). Zie voor informatie over het samenvoegen van een Azure-SSIS-IR met een VNet en een VNet configureren in Azure-portal, [Azure SSIS-IR Join naar VNet](join-azure-ssis-integration-runtime-virtual-network.md). 

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Documentatie van versie 1 van Data Factory](v1/data-factory-introduction.md).


## <a name="overview"></a>Overzicht
In dit artikel bevat verschillende manieren voor het leveren van een Azure-SSIS-IR:

- [Azure Portal](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [Azure Resource Manager-sjabloon](#azure-resource-manager-template)

Wanneer u een Azure-SSIS-IR maakt, is Data Factory maakt verbinding met uw Azure SQL Database voorbereiden van de catalogus SSIS-database (SSISDB). Het script configureert ook machtigingen en instellingen voor uw VNet, indien opgegeven, en verbindt de nieuwe instantie van de Azure SSIS Integration Runtime met het VNet.

Wanneer u een exemplaar van van Azure SSIS-IR inricht, worden ook de functie Azure Pack voor SSIS en toegang Redistributable geïnstalleerd. Deze onderdelen bieden connectiviteit naar Excel en toegang tot bestanden en met verschillende Azure gegevensbronnen, naast de gegevensbronnen die wordt ondersteund door de ingebouwde onderdelen. U kunt onderdelen van derden voor SSIS niet installeren op dit moment (met inbegrip van de onderdelen van derden van Microsoft, zoals de Oracle- en Teradata door Attunity als de componenten SAP BI).

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement**. Als u geen abonnement hebt, kunt u een [gratis proefaccount](http://azure.microsoft.com/pricing/free-trial/) maken.
- **Azure SQL Database server** of **SQL Server Managed Instance (private preview) (Extended Private Preview)**. Als u nog geen databaseserver hebt, maakt u die in Azure Portal voordat u begint. Deze server fungeert als host voor de SSIS-catalogusdatabase (SSISDB). Het wordt aangeraden om de databaseserver in dezelfde Azure-regio te maken als de Integration Runtime. Met deze configuratie kan de Integration Runtime uitvoeringslogboeken wegschrijven naar SSISDB zonder dat hierbij Azure-regio's worden overschreden. Noteer de prijscategorie van uw Azure SQL-server. Zie voor een lijst met ondersteunde Prijscategorieën voor Azure SQL Database, [limieten voor SQL-Database](../sql-database/sql-database-resource-limits.md).

    Bevestig dat uw Azure SQL Database-server of beheerde exemplaar van SQL Server (uitgebreide afgeschermd voorbeeld) beschikt niet over een catalogus SSIS (SSIDB database). Het inrichten van Azure SSIS-IR biedt geen ondersteuning met behulp van een bestaande SSIS-catalogus.
- **Klassieke of Azure Resource Manager virtuele Network(VNet) (optioneel)**. U hebt een virtueel netwerk (VNet) in Azure nodig als ten minste aan een van de volgende voorwaarden wordt voldaan:
    - U host de SSIS-catalogusdatabase in een SQL Server Managed Instance (private preview) die deel uitmaakt van een VNet.
    - U wilt verbinding maken met on-premises gegevensarchieven vanuit SSIS-pakketten die worden uitgevoerd in een Azure SSIS Integration Runtime.
- **Azure PowerShell**. Volg de instructies in [How to install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Azure PowerShell installeren en configureren). U gebruikt PowerShell om een script uit te voeren voor het inrichten van een Azure SSIS Integration Runtime die SSIS-pakketten uitvoert in de cloud. 

> [!NOTE]
> Zie [Producten beschikbaar per regio](https://azure.microsoft.com/regions/services/) voor een lijst met regio's die worden ondersteund door Azure Data Factory V2 en Azure-SSIS Integration Runtime. Vouw **Gegevens en analyses** uit om **Data Factory V2** en **SSIS Integration Runtime** te zien.

## <a name="azure-portal"></a>Azure Portal
In deze sectie maakt u de Azure portal, specifiek de Data Factory-gebruikersinterface gebruiken voor het maken van een Azure-SSIS-IR 

### <a name="create-a-data-factory"></a>Gegevensfactory maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).    
2. Klik op **Nieuw** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**. 
   
   ![Nieuw -> DataFactory](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. Voer op de pagina **Nieuwe gegevensfactory** **MyAzureSsisDataFactory** in als de **naam**. 
      
     ![De pagina Nieuwe data factory](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   De naam van de Azure-gegevensfactory moet **wereldwijd uniek** zijn. Als u het volgende foutbericht krijgt, wijzigt u de naam van de gegevensfactory (bijvoorbeeld uwnaamMyAzureSsisDataFactory) en probeert u het opnieuw. Zie het artikel [Data factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
  
       `Data factory name “MyAzureSsisDataFactory” is not available`
3. Selecteer het Azure-**abonnement** waarin u de gegevensfactory wilt maken. 
4. Voer een van de volgende stappen uit voor de **Resourcegroep**:
     
      - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst. 
      - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in.   
         
      Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
4. Selecteer **V2 (Preview)** als de **versie**.
5. Selecteer de **locatie** voor de gegevensfactory. In de lijst zie u alleen locaties die worden ondersteund voor het maken van gegevensfactory’s.
6. Selecteer **Vastmaken aan dashboard**.     
7. Klik op **Create**.
8. Op het dashboard ziet u de volgende tegel met de status: **Gegevensfactory implementeren**. 

    ![tegel met de status 'gegevensfactory implementeren'](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
9. Na het aanmaken ziet u de pagina **Data Factory** zoals weergegeven in de afbeelding.
   
   ![Startpagina van de gegevensfactory](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
10. Klik op **Maken en controleren** om de gebruikersinterface (UI) van Azure Data Factory te openen op een afzonderlijk tabblad. 

### <a name="provision-an-azure-ssis-integration-runtime"></a>Een Azure SSIS-integratieruntime inrichten

1. Klik op de pagina Aan de slag op de tegel **SSIS-integratieruntime configureren**. 

   ![De tegel SSIS-integratieruntime configureren](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)
2. Voer op de pagina **Algemene instellingen** van **Installatie van integratieruntime** de volgende stappen uit: 

   ![Algemene instellingen](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    1. Geef een **naam** op voor de zelf-hostende integratieruntime.
    2. Geef de **locatie** op voor de zelf-hostende integratieruntime. Alleen ondersteunde locaties worden weergegeven.
    3. Selecteer de **grootte van het knooppunt** dat moet worden geconfigureerd met de SSIS-runtime.
    4. Geef het **aantal knooppunten** in het cluster op.
    5. Klik op **Volgende**. 
1. Voer in de **SQL-instellingen** de volgende stappen uit: 

    ![SQL-instellingen](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    1. Geef het **Azure-abonnement** op dat de Azure SQL-server bevat. 
    2. Selecteer uw Azure SQL-server als het **eindpunt voor de Catalog-databaseserver**.
    3. Voer de gebruikersnaam van de **beheerder** in.
    4. Voer het **wachtwoord** voor de beheerder in.  
    5. Selecteer de **servicelaag** voor de SSISDB-database. De standaardwaarde is Basic.
    6. Klik op **Volgende**. 
1.  Selecteer op de pagina **Geavanceerde instellingen** een waarde voor het **maximale aantal parallelle uitvoeringen per knooppunt**.   

    ![Geavanceerde instellingen](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)    
5. Deze stap is **optioneel**. Als u een klassiek VNet hebt (virtueel netwerk) waaraan u de integratieruntime wilt koppelen, selecteert u de optie voor het **selecteren van een VNet om uw Azure SSIS-integratieruntime aan te koppelen en om toe te staan dat via Azure-services VNet-machtigingen/-instellingen worden geconfigureerd**. Voer hierna de volgende stappen uit: 

    ![Geavanceerde instellingen met VNet](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)    

    1. Geef het **abonnement** op dat het klassieke VNet bevat. 
    2. Selecteer het **VNet**. <br/>
    4. Selecteer het **Subnet**.<br/> 
1. Klik op **Voltooien** om te beginnen met het maken van de Azure SSIS-integratieruntime. 

    > [!IMPORTANT]
    > - Het duurt ongeveer 20 minuten om dit proces te voltooien
    > - De Data Factory-service maakt verbinding met uw Azure SQL-database om de SSISDB (SSIS Catalog Database) voor te bereiden. Het script configureert ook machtigingen en instellingen voor uw VNet, indien opgegeven, en verbindt de nieuwe instantie van de Azure SSIS Integration Runtime met het VNet.
7. Ga in het venster **Verbindingen** naar **Integratieruntimes**, indien nodig. Klik op **Vernieuwen** om de status te vernieuwen. 

    ![Status van het maken](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)
8. Gebruik de koppelingen onder de kolom **Acties** om de integratieruntime te controleren, te stoppen/starten, te bewerken of te verwijderen. Gebruik de laatste koppeling om JSON-code weer te geven voor de integratieruntime. De knoppen Bewerken en Verwijderen zijn alleen beschikbaar wanneer de IR is gestopt. 

    ![Azure SSIS-IR - acties](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)        
9. Klik op de koppeling **Controleren** onder **Acties**.  

    ![Azure SSIS-IR - details](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-details.png)
10. Als er een **fout** optreedt die is gerelateerd aan de Azure SSIS-IR, ziet u het aantal fouten op deze pagina en een koppeling naar de details over deze fout. Als de SSIS Catalog al bestaat op de databaseserver, ziet u bijvoorbeeld een foutmelding waarin het bestaan van de SSISDB-database wordt aangegeven.  
11. Klik bovenaan op **Integratieruntimes** om terug te gaan naar de vorige pagina met alle integratieruntimes die zijn gekoppeld aan de gegevensfactory.  

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Azure SSIS-integratieruntimes in de portal

1. Ga in de Azure Data Factory-UI naar het tabblad **Bewerken**, klik op **Verbindingen** en ga vervolgens naar het tabblad **Integratieruntimes** om bestaande integratieruntimes in uw gegevensfactory weer te geven. 
    ![Bestaande IR’s weergeven](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)
1. Klik op **Nieuw** om een nieuwe Azure SSIS-IR te maken. 

    ![Integratieruntime via menu](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)
2. Als u een Azure SSIS-integratieruntime wilt maken, klikt u op **Nieuw** zoals wordt weergegeven in de afbeelding. 
3. Selecteer in het venster Installatie van integratieruntime de optie voor het **verplaatsen van bestaande SSIS-pakketten voor uitvoering in Azure** en klik vervolgens op **Volgende**.

    ![Geef het type integratieruntime op](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)
4. Zie de sectie [Een Azure SSIS-integratieruntime inrichten](#provision-an-azure-ssis-integration-runtime) voor de resterende stappen voor het instellen van een Azure SSIS-IR.

## <a name="azure-powershell"></a>Azure PowerShell
In deze sectie kunt u Azure PowerShell gebruiken voor het maken van een Azure-SSIS-IR

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
$AzureSSISNodeSize = "Standard_D3_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or your Azure SQL Managed Instance (private preview) server endpoint]"
$SSISDBServerAdminUserName = "[your server admin username]"
$SSISDBServerAdminPassword = "[your server admin password]"

# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "[your Azure SQL Database pricing tier. Examples: Basic, S0, S1, S2, S3, etc.]"

## These two parameters apply if you are using a VNet and an Azure SQL Managed Instance (private preview) 
# Specify information about your classic or Azure Resource Manager virtual network (VNet). 
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
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt op de locatie `westeurope`.

```powershell
New-AzureRmResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

### <a name="create-a-data-factory"></a>Gegevensfactory maken
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
$SubscriptionName = "<Azure subscription name>"
$ResourceGroupName = "<Azure resource group name>"
# Data factory name. Must be globally unique
$DataFactoryName = "<Data factory name>" 
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "<Specify a name for your Azure-SSIS (IR)>"
$AzureSSISDescription = "<Specify description for your Azure-SSIS IR"
$AzureSSISLocation = "EastUS" 
 # In public preview, only Standard_A4_v2, Standard_A8_v2, Standard_D1_v2, Standard_D2_v2, Standard_D3_v2, Standard_D4_v2 are supported
$AzureSSISNodeSize = "Standard_D3_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
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

## <a name="azure-resource-manager-template"></a>Azure Resource Manager-sjabloon
In deze sectie kunt u een Azure Resource Manager-sjabloon maken van een Azure-SSIS-integratie-runtime gebruiken. Hier volgt een voorbeeld-overzicht: 

1. Maak een JSON-bestand met de volgende Resource Manager-sjabloon. Waarden in de punthaken (plaats houders) vervangen door uw eigen waarden. 

    ```json
    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {},
        "variables": {},
        "resources": [{
            "name": "<Specify a name for your data factory>",
            "apiVersion": "2017-09-01-preview",
            "type": "Microsoft.DataFactory/factories",
            "location": "East US",
            "properties": {},
            "resources": [{
                "type": "integrationruntimes",
                "name": "<Specify a name for the Azure SSIS IR>",
                "dependsOn": [ "<The name of the data factory you specified at the beginning>" ],
                "apiVersion": "2017-09-01-preview",
                "properties": {
                    "type": "Managed",
                    "typeProperties": {
                        "computeProperties": {
                            "location": "East US",
                            "nodeSize": "Standard_D1_v2",
                            "numberOfNodes": 1,
                            "maxParallelExecutionsPerNode": 1
                        },
                        "ssisProperties": {
                            "catalogInfo": {
                                "catalogServerEndpoint": "<Azure SQL server>.database.windows.net",
                                "catalogAdminUserName": "<Azure SQL user",
                                "catalogAdminPassword": {
                                    "type": "SecureString",
                                    "value": "<Azure SQL Password>"
                                },
                                "catalogPricingTier": "Basic"
                            }
                        }
                    }
                }
            }]
        }]
    }
    ```
2. Voor het implementeren van de Resource Manager-sjabloon, voert u de opdracht New-AzureRmResourceGroupDeployment zoals weergegeven in het volgende voorbeeld. In dit voorbeeld is ADFTutorialResourceGroup de naam van de resourcegroep. ADFTutorialARM.json is het bestand bevat een JSON-definitie voor de gegevensfactory en de Azure-SSIS-IR 

    ```powershell
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json
    ```

    Deze opdracht maakt u de gegevensfactory en maakt u een Azure-SSIS-IR erin, maar de IR niet wordt gestart 
3. Voer de opdracht Start-AzureRmDataFactoryV2IntegrationRuntime voor het starten van de Azure-SSIS-IR: 

    ```powershell
    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName "<Resource Group Name> `
                                             -DataFactoryName <Data Factory Name> `
                                             -Name <Azure SSIS IR Name> `
                                             -Force
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