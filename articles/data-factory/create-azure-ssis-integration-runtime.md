---
title: Azure-SSIS-integratie runtime maken in Azure Data Factory | Microsoft Docs
description: Informatie over het maken van een Azure-SSIS-integratie runtime in Azure Data Factory, zodat u kunt implementeren en uitvoeren van SSIS-pakketten in Azure.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/11/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: f75d08074fc6020541226318d6422da373136a2d
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268148"
---
# <a name="create-the-azure-ssis-integration-runtime-in-azure-data-factory"></a>Maken van de runtime Azure SSIS-integratie in Azure Data Factory
Dit artikel bevat stappen voor het inrichten van een Azure-SSIS-integratie runtime in Azure Data Factory. Vervolgens kunt u SQL Server Data Tools (SSDT) of SQL Server Management Studio (SSMS) implementeren en uitvoeren van SQL Server Integration Services (SSIS) pakketten in deze runtime in Azure.

De zelfstudie [zelfstudie: pakketten van SQL Server Integration Services (SSIS) implementeren in Azure](tutorial-create-azure-ssis-runtime-portal.md) ziet u hoe u een Azure-SSIS integratie Runtime (IR) maken met behulp van Azure SQL Database voor het hosten van de SSIS-catalogus. Dit artikel wordt op de zelfstudie wordt uitgebreid en ziet u hoe u het volgende doen: 

- Optioneel gebruik Azure SQL Database met een virtueel netwerk service-eindpunten/beheerde exemplaar (Preview) als de databaseserver voor het hosten van de catalogus SSIS (SSISDB-database). Als een vereiste, moet u uw Azure-SSIS-IR toevoegen aan een virtueel netwerk en virtueel Netwerkmachtigingen en instellingen configureren als nodig, Zie [Join Azure SSIS IR met een virtueel netwerk](https://docs.microsoft.com/en-us/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).  
- Optioneel gebruik Azure Active Directory (AAD) verificatie met uw Azure Data Factory beheerde Service identiteit (MSI) voor Azure SSIS-IR verbinding maken met de databaseserver. Als een vereiste, moet u uw Data Factory-MSI toevoegen aan een AAD-groep met machtigingen voor toegang tot de databaseserver, Zie [inschakelen AAD-verificatie voor Azure SSIS-IR](https://docs.microsoft.com/en-us/azure/data-factory/enable-aad-authentication-azure-ssis-ir).

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Documentatie van versie 1 van Data Factory](v1/data-factory-introduction.md).


## <a name="overview"></a>Overzicht
In dit artikel bevat verschillende manieren voor het leveren van een Azure-SSIS-IR:

- [Azure Portal](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [Azure Resource Manager-sjabloon](#azure-resource-manager-template)

Wanneer u een Azure-SSIS-IR maakt, is Data Factory maakt verbinding met uw Azure SQL Database voorbereiden van de catalogus SSIS-database (SSISDB). Het script ook machtigingen en instellingen voor het virtuele netwerk configureert, indien opgegeven en het nieuwe exemplaar van Azure SSIS-integratie runtime koppelt aan het virtuele netwerk.

Wanneer u een exemplaar van een Azure-SSI-IR inricht, worden ook het Azure Feature Pack voor SSIS en de Access Redistributable geïnstalleerd. Deze onderdelen bieden connectiviteit met Excel- en Access-bestanden en met verschillende Azure-gegevensbronnen, naast de gegevensbronnen die worden ondersteund door de ingebouwde onderdelen. U kunt ook extra onderdelen installeren. Zie [Aangepaste instelling voor de Azure-SSIS-integratieruntime](how-to-configure-azure-ssis-ir-custom-setup.md) voor meer informatie.

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement**. Als u geen abonnement hebt, kunt u een [gratis proefaccount](http://azure.microsoft.com/pricing/free-trial/) maken.
- **Azure SQL Database-server/beheerde exemplaar (Preview)**. Als u nog geen databaseserver hebt, maakt u die in Azure Portal voordat u begint. Deze server fungeert als host voor de SSIS-catalogusdatabase (SSISDB). Het wordt aangeraden om de databaseserver in dezelfde Azure-regio te maken als de Integration Runtime. Met deze configuratie kan de Integration Runtime uitvoeringslogboeken wegschrijven naar SSISDB zonder dat hierbij Azure-regio's worden overschreden. Op basis van de geselecteerde database-server, SSISDB kan worden gemaakt op uw naam als een zelfstandige database, onderdeel van een elastische Pool, of in een beheerde-instantie (Preview) is en toegankelijk is in openbaar netwerk of door het toevoegen van een virtueel netwerk. Zie voor een lijst met ondersteunde Prijscategorieën voor Azure SQL Database, [limieten voor SQL-Database](../sql-database/sql-database-resource-limits.md).

    Bevestig uw Azure SQL Database-server/beheerde exemplaar (Preview) heeft geen een catalogus SSIS (SSIDB database). Het inrichten van Azure-SSIS-IR biedt geen ondersteuning voor het gebruik van een bestaande SSIS-catalogus.
- **Virtueel netwerk klassieke/Azure Resource Manager (optioneel)**. Als ten minste één van de volgende voorwaarden waar is, moet u een Azure-netwerk hebben:
    - Als u beheert de catalogus SSIS-database in Azure SQL Database met virtueel netwerk service-eindpunten/beheerde exemplaar (Preview) dat is opgenomen in een virtueel netwerk.
    - U wilt verbinding maken met on-premises gegevensarchieven vanuit SSIS-pakketten die worden uitgevoerd in een Azure SSIS Integration Runtime.
- **Azure PowerShell**. Volg de instructies in [installeren en configureren van Azure PowerShell](/powershell/azure/install-azurerm-ps), als u PowerShell gebruiken voor het uitvoeren van een script te richten Azure SSIS-integratie runtime die SSIS-pakketten in de cloud wordt uitgevoerd. 

> [!NOTE]
> - U kunt een gegevensfactory van versie 2 maken in de volgende regio's: VS - oost, VS - oost 2, Zuidoost-Azië en West-Europa. 
> - U kunt een Azure-SSIS-IR maken in de volgende regio's: VS - oost, VS - oost 2, VS - midden, VS - west 2, Noord-Europa, West-Europa, UK - zuid en Australië - oost.

## <a name="azure-portal"></a>Azure Portal
In deze sectie maakt u de Azure portal, specifiek de Data Factory-gebruikersinterface gebruiken voor het maken van een Azure-SSIS-IR 

### <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Start de webbrowser **Microsoft Edge** of **Google Chrome**. Op dit moment wordt de Data Factory-gebruikersinterface alleen ondersteund in de webbrowsers Microsoft Edge en Google Chrome.
2. Meld u aan bij [Azure Portal](https://portal.azure.com/).    
3. Klik op **Nieuw** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**. 
   
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

    a. Voor **naam**, voer de naam van uw integratie-runtime.
    
    b. Voor **beschrijving**, geef de beschrijving van uw integratie-runtime.
    
    c. Voor **locatie**, selecteer de locatie van uw integratie-runtime. Alleen ondersteunde locaties worden weergegeven. Het is raadzaam dat u dezelfde locatie van uw database-server op host SSISDB selecteert.
    
    d. Voor **Knooppuntgrootte**, selecteer de grootte van knooppunt in het cluster integratie-runtime. Alleen ondersteunde knooppuntgrootten worden weergegeven. Selecteer een grote knooppuntgrootte (opschaling van) als u wilt uitvoeren van veel compute/geheugen – intensieve pakketten.
    
    e. Voor **knooppuntnummer**, selecteert u het aantal knooppunten in uw integratie runtime-cluster. Alleen ondersteunde knooppunt getallen worden weergegeven. Selecteer een grote cluster met veel knooppunten (scale-out), als u wilt veel pakketten parallel worden uitgevoerd.
     
    f. Voor **licentieEdition/**, selecteert u SQL Server-editie/licentie voor uw integratie runtime: Standard of Enterprise. Selecteer Enterprise, als u wilt gebruiken van geavanceerde/premium-functies op uw integratie-runtime. 
    
    g. Voor **opslaan geld**, selecteert u de optie Azure hybride voordeel (AHB) voor uw integratie runtime: Ja of Nee. Selecteer Ja als u uw eigen SQL Server-licentie met Software Assurance wilt profiteren van de kostenbesparingen met hybride gebruik brengen.
    
    h. Klik op **Volgende**. 
3. Voer op de pagina **SQL-instellingen** de volgende stappen uit: 

    ![SQL-instellingen](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    a. Voor **abonnement**, selecteer de Azure-abonnement met uw database-server op host SSISDB. 
    
    b. Voor **locatie**, selecteer de locatie van uw database-server op host SSISDB. Het is raadzaam dat u dezelfde locatie van de runtime van uw integratie selecteert.
    
    c. Voor **catalogus Database servereindpunt**, selecteert u het eindpunt van uw database-server op host SSISDB. Op basis van de geselecteerde database-server, SSISDB kan worden gemaakt op uw naam als een zelfstandige database, onderdeel van een elastische Pool, of in een beheerde-instantie (Preview) is en toegankelijk is in openbaar netwerk of door het toevoegen van een virtueel netwerk.
    
    d. Op **gebruik AAD-verificaties...**  selectievakje, selecteer de verificatiemethode voor de databaseserver host SSISDB: SQL- of Azure Active Directory (AAD) met uw Azure Data Factory beheerde Service identiteit (MSI). Als u dit selectievakje inschakelt, moet u uw Data Factory-MSI toevoegen aan een AAD-groep met machtigingen voor toegang tot de databaseserver, Zie [inschakelen AAD-verificatie voor Azure SSIS-IR](https://docs.microsoft.com/en-us/azure/data-factory/enable-aad-authentication-azure-ssis-ir).
    
    e. Voor **Admin Username**, geef gebruikersnaam voor SQL-verificatie voor uw database-server als host SSISDB.
    
    f. Voor **beheerderswachtwoord**, Geef wachtwoord voor SQL-verificatie voor uw database-server als host SSISDB.  
    
    g. Voor **catalogus Database servicelaag**, selecteert u de servicetier voor uw database-server op host SSISDB: Standard-Basic/Premium-laag of de naam van de elastische groep.
    
    h. Klik op **testverbinding** en als dit lukt, klikt u op **volgende**. 
4.  Op de **geavanceerde instellingen** pagina, de volgende stappen uit:   

    ![Geavanceerde instellingen](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)    
    
    a. Voor **maximale parallelle uitvoeringen Per knooppunt**, selecteert u het maximum aantal pakketten gelijktijdig uitvoeren per knooppunt in het cluster integratie-runtime. Alleen ondersteund pakket getallen worden weergegeven. Selecteer een lage waarde als u meer dan één kern gebruiken wilt voor het uitvoeren van een afzonderlijk groot/zware-gewicht-pakket dat is compute/geheugen-intensief. Selecteer een groot aantal als u wilt uitvoeren van een of meer kleine/lichte pakketten in één kern. 
    
    b. Voor **aangepaste Setup Container SAS URI**, eventueel Shared Access Signature (SAS) id URI (Uniform Resource) van uw Azure Storage-Blob-container opgeven waar het setup-script en de bijbehorende bestanden zijn opgeslagen, Zie [Aangepaste installatie voor Azure SSIS-IR](https://docs.microsoft.com/en-us/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup). 
5. Op **een virtueel netwerk selecteert...**  uit, schakel of u wilt deelnemen aan uw runtime integratie met een virtueel netwerk. Als u met Azure SQL Database met een virtueel netwerk service-eindpunten/beheerde exemplaar (Preview) SSISDB hosten of toegang tot on-premises gegevens; vereisen controleren dat wil zeggen, u hebt lokale gegevens bronnen/bestemmingen in uw SSIS-pakketten, Zie [Azure SSIS-IR koppelen aan een virtueel netwerk](https://docs.microsoft.com/en-us/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Als u dit selectievakje inschakelt, kunt u de volgende stappen:


    ![Geavanceerde instellingen met het virtuele netwerk](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)    

    a. Voor **abonnement**, selecteer de Azure-abonnement met het virtuele netwerk. 
    
    b. Voor **locatie**, dezelfde locatie van uw integratie-runtime is geselecteerd.
    
    b. Voor **Type**, selecteer het type van het virtuele netwerk: klassieke of Azure Resource Manager. U wordt aangeraden dat u Azure Resource Manager virtueel netwerk selecteert, sinds klassiek virtueel netwerk snel wordt afgeschaft.
    
    c. Voor **VNet-naam**, selecteer de naam van het virtuele netwerk. Dit virtuele netwerk moet hetzelfde virtuele netwerk voor Azure SQL Database met een virtueel netwerk service-eindpunten/beheerde exemplaar (Preview) gebruikt als host voor SSISDB en of de versie die is verbonden met uw on-premises netwerk.

    d. Voor **subnetnaam**, selecteer de naam van het subnet voor het virtuele netwerk. Dit virtuele netwerk moet een ander subnet bevindt dan de versie die wordt gebruikt voor beheerde exemplaar (Preview) naar host SSISDB.
6. Klik op **VNet validatie** en als dit lukt, klikt u op **voltooien** starten van het maken van uw Azure-SSIS-integratie-runtime. 

    > [!IMPORTANT]
    > - Het duurt ongeveer 20 minuten om dit proces te voltooien
    > - De Data Factory-service maakt verbinding met uw Azure SQL-database om de SSISDB (SSIS Catalog Database) voor te bereiden. Het script ook machtigingen en instellingen voor het virtuele netwerk configureert, indien opgegeven en het nieuwe exemplaar van Azure SSIS-integratie runtime koppelt aan het virtuele netwerk.
7. Ga in het venster **Verbindingen** naar **Integratieruntimes**, indien nodig. Klik op **Vernieuwen** om de status te vernieuwen. 

    ![Status van het maken](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)
8. Gebruik de koppelingen onder **acties** kolom die u wilt stoppen/starten, bewerken of verwijderen van de integratie-runtime. Gebruik de laatste koppeling om JSON-code weer te geven voor de integratieruntime. De knoppen Bewerken en Verwijderen zijn alleen beschikbaar wanneer de IR is gestopt. 

    ![Azure SSIS-IR - acties](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)        

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
### Azure Data Factory version 2 information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
# You can create a data factory of version 2 in the following regions: East US, East US 2, Southeast Asia, and West Europe. 
$DataFactoryLocation = "EastUS" 

### Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[specify a description for your Azure-SSIS IR]"
# You can create an Azure-SSIS IR in the following regions: East US, East US 2, Central US, West US 2, North Europe, West Europe, UK South, and Australia East.
$AzureSSISLocation = "EastUS" 
# In public preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_D4_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
$AzureSSISMaxParallelExecutionsPerNode = 8 
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database with virtual network service endpoints/Managed Instance (Preview)/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = “[your subnet name or leave it empty]" # WARNING: Please use a different subnet than the one used for your Managed Instance (Preview)

### SSISDB info
$SSISDBServerEndpoint = “[your Azure SQL Database server name or Managed Instance (Preview) name.DNS prefix].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database or leave it empty for Managed Instance (Preview)]"
```

### <a name="log-in-and-select-subscription"></a>Aanmelden en abonnement selecteren
Voeg de volgende code het script aan te melden en selecteer uw Azure-abonnement: 

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName
```

### <a name="validate-the-connection-to-database"></a>Valideren van verbinding met de database
Voeg het volgende script voor het valideren van uw Azure SQL Database-server-eindpunt. 

```powershell
if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
{
    if(![string]::IsNullOrEmpty($SSISDBPricingTier))
    {
        $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=“ + $SSISDBServerAdminPassword
        $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
        Try
        {
            $sqlConnection.Open();
        }
        Catch [System.Data.SqlClient.SqlException]
        {
            Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_"  ;
            Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
            $yn = Read-Host
            if(!($yn -ieq "Y"))
            {
                Return;
            } 
        }
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
Maak een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd. 

```powershell
New-AzureRmResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

### <a name="create-a-data-factory"></a>Een gegevensfactory maken
Voer de volgende opdracht uit om een data factory te maken.

```powershell
Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

### <a name="create-an-integration-runtime"></a>Een Integration Runtime maken
Voer de volgende opdrachten voor het maken van een Azure-SSIS-integratie runtime die SSIS-pakketten in Azure wordt uitgevoerd. 

Als u niet Azure SQL Database met een virtueel netwerk service-eindpunten/beheerde exemplaar (Preview gebruikt) voor het hosten van SSISDB of toegang nodig voor on-premises gegevens, kunt u VNetId en Subnet parameters weglaten of lege waarden doorgeven voor deze. Anders kunnen niet deze weglaten en moet geldige waarden doorgeven van de configuratie van uw virtuele netwerk, Zie [Azure SSIS-IR koppelen aan een virtueel netwerk](https://docs.microsoft.com/en-us/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).

Als u beheerde exemplaar (Preview) naar host SSISDB gebruikt, kunt u de parameter CatalogPricingTier of een lege waarde voor het doorgeven. Anders u kan niet het weglaten en moet doorgeven van een geldige waarde uit de lijst met ondersteunde Prijscategorieën voor Azure SQL Database, Zie [limieten voor SQL-Database](../sql-database/sql-database-resource-limits.md). 

Als u Azure Active Directory (AAD) verificatie met uw Azure Data Factory beheerde Service identiteit (MSI) verbinding maken met de database-server, kunt u de parameter CatalogAdminCredential weglaten, maar moet u uw Data Factory-MSI in een AAD-groep met toegang toevoegen machtigingen voor de database-server, Zie [inschakelen AAD-verificatie voor Azure SSIS-IR](https://docs.microsoft.com/en-us/azure/data-factory/enable-aad-authentication-azure-ssis-ir).

```powershell               
Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Type Managed `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -SetupScriptContainerSasUri $SetupScriptContainerSasUri `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogPricingTier $SSISDBPricingTier

if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
{
    $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
    $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogAdminCredential $serverCreds
}
```

U moet waarden voor VnetId en Subnet parameters met Azure SQL beheerd-instantie (Preview) die lid wordt van een virtueel netwerk doorgeven. De parameter CatalogPricingTier geldt niet voor Azure SQL beheerd-exemplaar (Preview). 

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

Hier is de volledige-script dat wordt gemaakt van een Azure-SSIS-integratie-runtime. 

```powershell
### Azure Data Factory version 2 information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
# You can create a data factory of version 2 in the following regions: East US, East US 2, Southeast Asia, and West Europe. 
$DataFactoryLocation = "EastUS" 

### Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[specify a description for your Azure-SSIS IR]"
# You can create an Azure-SSIS IR in the following regions: East US, East US 2, Central US, West US 2, North Europe, West Europe, UK South, and Australia East.
$AzureSSISLocation = "EastUS" 
# In public preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_D4_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
$AzureSSISMaxParallelExecutionsPerNode = 8 
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database with virtual network service endpoints/Managed Instance (Preview)/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = “[your subnet name or leave it empty]" # WARNING: Please use a different subnet than the one used for your Managed Instance (Preview)

### SSISDB info
$SSISDBServerEndpoint = “[your Azure SQL Database server name or Managed Instance (Preview) name.DNS prefix].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database or leave it empty for Managed Instance (Preview)]"

### Log in and select subscription
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

### Validate the connection to database
if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
{
    if(![string]::IsNullOrEmpty($SSISDBPricingTier))
    {
        $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=“ + $SSISDBServerAdminPassword
        $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
        Try
        {
            $sqlConnection.Open();
        }
        Catch [System.Data.SqlClient.SqlException]
        {
            Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_"  ;
            Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
            $yn = Read-Host
            if(!($yn -ieq "Y"))
            {
                Return;
            } 
        }
    }
}

### Configure virtual network
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

### Create a data factory
Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName

### Create an integration runtime
Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Type Managed `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -SetupScriptContainerSasUri $SetupScriptContainerSasUri `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogPricingTier $SSISDBPricingTier

if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
{
    $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
    $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogAdminCredential $serverCreds
}

### Start integration runtime   
write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="azure-resource-manager-template"></a>Azure Resource Manager-sjabloon
In deze sectie kunt u de Azure Resource Manager-sjabloon maken van Azure SSIS-integratie-runtime gebruiken. Hier volgt een voorbeeld-overzicht: 

1. Maak een JSON-bestand met de volgende Azure Resource Manager-sjabloon. Waarden in de punthaken (plaats houders) vervangen door uw eigen waarden. 

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
                "name": "<Specify a name for your Azure-SSIS IR>",
                "dependsOn": [ "<The name of the data factory you specified at the beginning>" ],
                "apiVersion": "2017-09-01-preview",
                "properties": {
                    "type": "Managed",
                    "typeProperties": {
                        "computeProperties": {
                            "location": "East US",
                            "nodeSize": "Standard_D4_v2",
                            "numberOfNodes": 1,
                            "maxParallelExecutionsPerNode": 8
                        },
                        "ssisProperties": {
                            "catalogInfo": {
                                "catalogServerEndpoint": "<Azure SQL Database server name>.database.windows.net",
                                "catalogAdminUserName": "<Azure SQL Database server admin username>",
                                "catalogAdminPassword": {
                                    "type": "SecureString",
                                    "value": "<Azure SQL Database server admin password>"
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
    
2. De Azure Resource Manager-sjabloon implementeren, voert u New-AzureRmResourceGroupDeployment opdracht zoals weergegeven in het volgende voorbeeld, waarin waarbij ADFTutorialResourceGroup is de naam van de resourcegroep en ADFTutorialARM.json is het bestand JSON de definitie voor de data factory en in Azure SSIS-IR 

    ```powershell
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json
    ```

    Deze opdracht maakt u uw gegevensfactory en de Azure-SSIS-IR erin, maar de IR niet wordt gestart 
3. Voer voor het starten van uw Azure-SSIS-IR Start AzureRmDataFactoryV2IntegrationRuntime-opdracht: 

    ```powershell
    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName "<Resource Group Name>" `
                                                 -DataFactoryName "<Data Factory Name>" `
                                                 -Name "<Azure SSIS IR Name>" `
                                                 -Force
    ``` 

## <a name="deploy-ssis-packages"></a>SSIS-pakketten implementeren
Gebruik nu SQL Server Data Tools (SSDT) of SQL Server Management Studio (SSMS) om uw SSIS-pakketten te implementeren in Azure. Verbinding maken met uw database-server die als host fungeert voor de catalogus SSIS (SSISDB). De naam van de database-server is in de indeling: &lt;Azure SQL Database-servernaam&gt;. database.windows.net of &lt;beheerd (Preview) exemplaarnaam. DNS-voorvoegsel&gt;. database.windows.net. Zie het artikel [Pakketten implementeren](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server) voor instructies.

## <a name="next-steps"></a>Volgende stappen
Zie de andere Azure-SSIS-IR-onderwerpen in deze documentatie:

- [Azure-SSIS-integratie Runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel bevat conceptuele informatie over de integratie runtimes in het algemeen met inbegrip van de Azure-SSIS-IR 
- [Zelfstudie: SSIS-pakketten implementeren in Azure](tutorial-create-azure-ssis-runtime-portal.md). Dit artikel biedt stapsgewijze instructies voor het maken van een Azure-SSIS IR en maakt gebruik van een Azure SQL database voor het hosten van de SSIS-catalogus. 
- [Een Azure-SSIS IR controleren](monitor-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel leest u hoe u informatie over een Azure-SSIS IR ophaalt. Daarnaast bevat het artikel beschrijvingen van statuswaarden die worden gebruikt in de geretourneerde informatie. 
- [Een Azure-SSIS IR beheren](manage-azure-ssis-integration-runtime.md). In dit artikel leest u hoe u een Azure-SSIS IR stopt, start of verwijdert. Er wordt ook uitgelegd hoe u een Azure-SSIS IR kunt uitschalen door meer knooppunten toe te voegen aan de IR. 
- [Een Azure-SSIS-IR toevoegen aan een virtueel netwerk](join-azure-ssis-integration-runtime-virtual-network.md). In dit artikel bevat conceptuele informatie over uw Azure-SSIS-IR toevoegen aan een virtuele Azure-netwerk. Het bevat ook stappen voor het gebruik van Azure-portal voor het virtuele netwerk configureren zodat Azure SSIS-IR kunt deelnemen aan het virtuele netwerk. 

