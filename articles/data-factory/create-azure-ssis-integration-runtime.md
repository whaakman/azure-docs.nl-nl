---
title: Azure-SSIS integratieruntime maken in Azure Data Factory | Microsoft Docs
description: Informatie over het maken van een Azure-SSIS integratieruntime in Azure Data Factory, zodat u kunt implementeren en uitvoeren van SSIS-pakketten in Azure.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/23/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: a7ba62a28b65d1cd7152c793bc303e747057cdf8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991467"
---
# <a name="create-the-azure-ssis-integration-runtime-in-azure-data-factory"></a>De Azure-SSIS integratieruntime in Azure Data Factory maken
In dit artikel bevat stappen voor het inrichten van een Azure-SSIS integratieruntime in Azure Data Factory. Vervolgens kunt u SSDT (SQL Server Data Tools) of SSMS (SQL Server Management Studio) gebruiken om SSIS-pakketten (SQL Server Integration Services) te implementeren en uit te voeren in deze runtime van Azure. 

De zelfstudie [zelfstudie: SQL Server Integration Services-pakketten (SSIS) implementeren in Azure](tutorial-create-azure-ssis-runtime-portal.md) ziet u hoe u een Azure-SSIS Integration Runtime (IR) maken met behulp van Azure SQL Database voor het hosten van de SSIS-catalogus. In dit artikel gaat verder in op de zelfstudie en ziet u hoe u het volgende doen: 

- (Optioneel) gebruik Azure SQL Database met virtual network service-eindpunten/beheerd exemplaar als de databaseserver voor het hosten van uw SSIS-catalogus (SSISDB-database). Zie voor richtlijnen bij het kiezen van het type van de database-server host SSISDB [logische Vergelijk SQL Database-server en SQL Database Managed Instance](create-azure-ssis-integration-runtime.md#compare-sql-database-logical-server-and-sql-database-managed-instance). Als een vereiste moet u uw Azure-SSIS IR toevoegen aan een virtueel netwerk en het configureren van virtueel Netwerkmachtigingen en instellingen indien nodig. Zie [deelnemen aan Azure-SSIS IR aan een virtueel netwerk](https://docs.microsoft.com/en-us/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). 

- (Optioneel) gebruik van Azure Active Directory (AAD)-verificatie met uw Azure Data Factory Managed Service Identity (MSI) voor Azure-SSIS IR verbinding maken met de database-server. Als een vereiste, moet u uw Data Factory-MSI in een AAD-groep met machtigingen voor toegang tot de database-server toevoegen, Zie [inschakelen AAD-verificatie voor Azure-SSIS IR](https://docs.microsoft.com/en-us/azure/data-factory/enable-aad-authentication-azure-ssis-ir). 

## <a name="overview"></a>Overzicht
In dit artikel ziet u een Azure-SSIS IR wordt ingericht op verschillende manieren: 

- [Azure Portal](#azure-portal) 
- [Azure PowerShell](#azure-powershell) 
- [Azure Resource Manager-sjabloon](#azure-resource-manager-template) 

Wanneer u een Azure-SSIS IR hebt gemaakt, maakt Data Factory verbinding met uw Azure SQL-Database voorbereiden van de SSIS-catalogusdatabase (SSISDB). Het script ook machtigingen en instellingen voor het virtuele netwerk geconfigureerd als is opgegeven en het nieuwe exemplaar van Azure-SSIS integratieruntime verbinding met het virtuele netwerk. 

Wanneer u een exemplaar van een Azure-SSI-IR inricht, worden ook het Azure Feature Pack voor SSIS en de Access Redistributable geïnstalleerd. Deze onderdelen bieden connectiviteit met Excel- en Access-bestanden en met verschillende Azure-gegevensbronnen, naast de gegevensbronnen die worden ondersteund door de ingebouwde onderdelen. U kunt ook extra onderdelen installeren. Zie [Aangepaste instelling voor de Azure-SSIS-integratieruntime](how-to-configure-azure-ssis-ir-custom-setup.md) voor meer informatie. 

## <a name="prerequisites"></a>Vereisten 
- **Azure-abonnement**. Als u geen abonnement hebt, kunt u een [gratis proefaccount](http://azure.microsoft.com/pricing/free-trial/) maken. 

- **Logische server van Azure SQL Database of Managed Instance**. Als u nog geen databaseserver hebt, maakt u die in Azure Portal voordat u begint. Deze server fungeert als host voor de SSIS-catalogusdatabase (SSISDB). Het wordt aangeraden om de databaseserver in dezelfde Azure-regio te maken als de Integration Runtime. Met deze configuratie kan de Integration Runtime uitvoeringslogboeken wegschrijven naar SSISDB zonder dat hierbij Azure-regio's worden overschreden. Op basis van de geselecteerde database-server, kan SSISDB worden gemaakt uit uw naam als een individuele database, onderdeel van een elastische pool of in een beheerd exemplaar en toegankelijk zijn in openbaar netwerk of door het toevoegen van een virtueel netwerk. Zie voor een lijst van ondersteunde Prijscategorieën voor Azure SQL Database, [SQL Database-resourcebeperkingen](../sql-database/sql-database-resource-limits.md). 

    Zorg ervoor dat uw logische Azure SQL Database-server of een beheerd exemplaar al geen SSIS-catalogus (SSIDB-database). Het inrichten van Azure-SSIS-IR biedt geen ondersteuning voor het gebruik van een bestaande SSIS-catalogus. 

- **Klassieke of een virtueel netwerk van Azure Resource Manager (optioneel)**. U hebt een Azure-netwerk als ten minste één van de volgende voorwaarden voldaan wordt: 
    - Als u beheert de SSIS-catalogusdatabase in Azure SQL Database met een virtueel netwerk service-eindpunten/beheerd exemplaar in een virtueel netwerk. 
    - U wilt verbinding maken met on-premises gegevensarchieven vanuit SSIS-pakketten die worden uitgevoerd in een Azure SSIS Integration Runtime. 

- **Azure PowerShell**. Volg de instructies in [hoe u Azure PowerShell installeren en configureren](/powershell/azure/install-azurerm-ps), als u PowerShell gebruiken om een script uitgevoerd om het inrichten van Azure-SSIS integratieruntime die SSIS-pakketten in de cloud uitvoert. 

### <a name="region-support"></a>Ondersteuning voor regio
Voor een lijst met Azure-regio’s waarin Data Factory momenteel beschikbaar is, selecteert u op de volgende pagina de regio’s waarin u geïnteresseerd bent, vouwt u vervolgens **Analytics** uit en gaat u naar **Data Factory**: [Beschikbare producten per regio](https://azure.microsoft.com/global-infrastructure/services/).

Voor een lijst van Azure-regio's waarin de Azure-SSIS Integration Runtime momenteel beschikbaar is is, selecteert u de regio's die u interesseren op de volgende pagina uit en vouw vervolgens **Analytics** vinden **SSIS-Integratieruntime** : [Producten beschikbaar per regio](https://azure.microsoft.com/global-infrastructure/services/). ### SQL-Database vergelijken en beheerd exemplaar

### <a name="compare-sql-database-logical-server-and-sql-database-managed-instance"></a>Logische SQL Database-server en SQL Database Managed Instance vergelijken

De volgende tabel vergelijkt bepaalde functies van de logische SQL Database-server en SQL Database Managed Instance zoals ze betrekking op de Azure-SSIR IR hebben:

| Functie | Logische SQL Database-server| SQL Database - beheerd exemplaar |
|---------|--------------|------------------|
| **Planning** | SQL Server Agent is niet beschikbaar.<br/><br/>Zie [plannen van een pakket als onderdeel van een Azure Data Factory-pijplijn](/sql/integration-services/lift-shift/ssis-azure-schedule-packages#activity).| SQL Server Agent is beschikbaar. |
| **Verificatie** | U kunt een database maken met een ingesloten database-gebruikersaccount dat staat voor een Azure Active Directory-gebruiker in de **dbmanager** rol.<br/><br/>Zie [inschakelen van Azure AD in Azure SQL Database](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database). | U kunt een database maken met een ingesloten database-gebruikersaccount dat staat voor een Azure Active Directory-gebruiker dan een Azure AD-beheerder. <br/><br/>Zie [inschakelen van Azure AD op beheerd exemplaar voor Azure SQL Database](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database-managed-instance). |
| **Servicelaag** | Als u de Azure-SSIS-IR in SQL-Database maakt, selecteert u de servicelaag voor SSISDB. Er zijn meerdere lagen van de services. | Wanneer u de Azure-SSIS-IR op een beheerd exemplaar maakt, kunt u de servicelaag niet selecteren voor SSISDB. Alle databases op de dezelfde Managed Instance delen dezelfde resource toegewezen aan deze instantie. |
| **Virtueel netwerk** | Biedt ondersteuning voor Azure Resource Manager en klassieke virtuele netwerken. | Biedt alleen ondersteuning voor virtueel netwerk van Azure Resource Manager. Het virtuele netwerk is vereist.<br/><br/>Als u uw Azure-SSIS IR aan hetzelfde virtuele netwerk bevinden als het beheerde exemplaar toevoegen, zorg ervoor dat de Azure-SSIS IR in een ander subnet dan het beheerde exemplaar is. Als u de Azure-SSIS IR aan een ander virtueel netwerk dan het beheerde exemplaar toevoegen, raden wij peering op virtueel netwerk (dit is beperkt tot dezelfde regio bevinden) of een virtueel netwerk met virtuele netwerk. Zie [verbinding maken met uw toepassing naar Azure SQL Database Managed Instance](../sql-database/sql-database-managed-instance-connect-app.md). |
| **Gedistribueerde transacties** | Microsoft Distributed Transaction Coordinator (MSDTC)-transacties worden niet ondersteund. Als uw pakketten MSDTC gedistribueerde transacties te coördineren, kunt u mogelijk een tijdelijke oplossing implementeren met behulp van elastische transacties voor SQL-Database. Op dit moment geen SSIS ingebouwde ondersteuning voor elastische transacties. Voor het gebruik van elastische transacties in uw SSIS-pakket, moet u aangepaste ADO.NET-code in de taak van een Script schrijven. Deze taak script moet het begin en einde van de transactie en de acties die plaatsvinden binnen de transactie moeten bevatten.<br/><br/>Zie voor meer informatie over het coderen van elastische transacties [elastische transacties met Azure SQL Database](https://azure.microsoft.com/en-us/blog/elastic-database-transactions-with-azure-sql-database/). Zie voor meer informatie over elastische transacties in het algemeen [gedistribueerde transacties tussen clouddatabases](../sql-database/sql-database-elastic-transactions-overview.md). | Wordt niet ondersteund. |
| | | |

## <a name="azure-portal"></a>Azure Portal
In deze sectie gebruikt u de Azure portal, specifiek de Data Factory-gebruikersinterface om een Azure-SSIS-IR te maken 

### <a name="create-a-data-factory"></a>Een gegevensfactory maken 
1. Start de webbrowser **Microsoft Edge** of **Google Chrome**. Op dit moment wordt de Data Factory-gebruikersinterface alleen ondersteund in de webbrowsers Microsoft Edge en Google Chrome. 
1. Meld u aan bij [Azure Portal](https://portal.azure.com/). 
1. Klik op **Nieuw** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**. 

   ![Nieuw -> DataFactory](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)

1. Voer op de pagina **Nieuwe gegevensfactory** **MyAzureSsisDataFactory** in als de **naam**. 

   ![De pagina Nieuwe data factory](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)

   De naam van de Azure-gegevensfactory moet **wereldwijd uniek** zijn. Als u het volgende foutbericht krijgt, wijzigt u de naam van de gegevensfactory (bijvoorbeeld uwnaamMyAzureSsisDataFactory) en probeert u het opnieuw. Zie het artikel [Data factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten. 

   `Data factory name “MyAzureSsisDataFactory” is not available`

1. Selecteer het Azure-**abonnement** waarin u de gegevensfactory wilt maken. 
1. Voer een van de volgende stappen uit voor de **Resourcegroep**: 

   - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst. 
   - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in. 

   Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie. 

1. Selecteer **V2** als de **versie**. 
1. Selecteer de **locatie** voor de gegevensfactory. In de lijst zie u alleen locaties die worden ondersteund voor het maken van gegevensfactory’s. 
1. Selecteer **Vastmaken aan dashboard**. 
1. Klik op **Create**. 
1. Op het dashboard ziet u de volgende tegel met de status: **Gegevensfactory implementeren**. 

    ![tegel met de status 'gegevensfactory implementeren'](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)

1. Na het aanmaken ziet u de pagina **Data Factory** zoals weergegeven in de afbeelding. 

    ![Startpagina van de gegevensfactory](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)

1. Klik op **Maken en controleren** om de gebruikersinterface (UI) van Azure Data Factory te openen op een afzonderlijk tabblad. 

### <a name="provision-an-azure-ssis-integration-runtime"></a>Een Azure SSIS-integratieruntime inrichten 
1. Klik op de pagina Aan de slag op de tegel **SSIS-integratieruntime configureren**. 

   ![De tegel SSIS-integratieruntime configureren](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

1. Voer op de pagina **Algemene instellingen** van **Installatie van integratieruntime** de volgende stappen uit: 

   ![Algemene instellingen](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    a. Voer bij **Naam** de naam van de integratieruntime in. 

    b. Voer bij **beschrijving** de beschrijving van de integratieruntime in. 

    c. Selecteer bij **Locatie** de locatie voor de integratieruntime. Alleen ondersteunde locaties worden weergegeven. We raden u aan dezelfde locatie van uw databaseserver te selecteren voor het hosten van SSISDB. 

    d. Selecteer bij **Knooppuntgrootte** de grootte van knooppunt in het integratieruntimecluster. Alleen ondersteunde knooppuntgrootten worden weergegeven. Selecteer een grote knooppuntgrootte (omhoog schalen) als u veel reken-/geheugenintensieve pakketten wilt uitvoeren. 

    e. Selecteer bij **Aantal knooppunten** het aantal knooppunten in het integratieruntimecluster. Alleen ondersteunde knooppuntaantallen worden weergegeven. Selecteer een groot cluster met veel knooppunten (uitschalen), als u veel pakketten parallel wilt uitvoeren. 

    f. Selecteer bij **Editie/licentie** de SQL Server-editie/licentie voor uw integratie runtime: Standard of Enterprise. Selecteer Enterprise als u geavanceerde/premium functies in de integratieruntime wilt gebruiken. 

    g. Selecteer bij **Geld besparen** de Azure Hybrid Benefit (AHB)-optie voor uw integratieruntime: Ja of Nee. Selecteer Ja als u uw eigen SQL Server-licentie met Software Assurance wilt gebruiken om te profiteren van de kostenbesparingen met hybride gebruik. 

    h. Klik op **Volgende**. 

1. Voer op de pagina **SQL-instellingen** de volgende stappen uit: 

   ![SQL-instellingen](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    a. Selecteer bij **Abonnement** het Azure-abonnement dat uw databaseserver heeft voor het hosten van SSISDB. 

    b. Selecteer bij **Locatie** de locatie van uw databaseserver voor het hosten van SSISDB. We raden u aan dezelfde locatie van uw integratieruntime te selecteren. 

    c. Selecteer bij het **Eindpunt voor de Catalog-databaseserver** het eindpunt van uw databaseserver voor het hosten van SSISDB. Op basis van de geselecteerde database-server, kan SSISDB worden gemaakt uit uw naam als een individuele database, onderdeel van een elastische pool of in een beheerd exemplaar en toegankelijk zijn in openbaar netwerk of door het toevoegen van een virtueel netwerk. 

    d. Op **gebruik AAD-verificatie...**  selectievakje, selecteer de verificatiemethode voor de database-server host SSISDB: SQL- of Azure Active Directory (AAD) met uw Azure Data Factory Managed Service Identity (MSI). Als u dit selectievakje inschakelt, moet u uw Data Factory-MSI in een AAD-groep met machtigingen voor toegang tot de database-server toevoegen, Zie [inschakelen AAD-verificatie voor Azure-SSIS IR](https://docs.microsoft.com/en-us/azure/data-factory/enable-aad-authentication-azure-ssis-ir). 

    e. Voer bij **Gebruikersnaam van beheerder** de gebruikersnaam voor SQL-verificatie voor uw databaseserver voor het hosten van SSISDB in. 

    f. Voer bij **Beheerderswachtwoord** het wachtwoord voor SQL-verificatie voor uw databaseserver voor het hosten van SSISDB in. 

    g. Voor **Catalog Database-servicelaag**, selecteert u de servicelaag voor uw database-server host SSISDB: naam van Basic/Standard/Premium-laag of elastische pool. 

    h. Klik op **Verbinding testen**. Als dit lukt, klikt u op **Volgende**. 

1.  Voer op de pagina **Geavanceerde instellingen** de volgende stappen uit: 

    ![Geavanceerde instellingen](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

    a. Selecteer bij het **maximale aantal parallelle uitvoeringen per knooppunt** het maximum aantal pakketten dat gelijktijdig per knooppunt kan worden uitgevoerd in het integratieruntimecluster. Alleen ondersteunde pakketaantallen worden weergegeven. Selecteer een lage waarde als u meer dan één kern gebruiken wilt voor het uitvoeren van een enkele grote/zware-weight-pakket dat compute/geheugen-intensief. Selecteer een groot aantal als u een of meer kleine/lichte pakketten in één kern wilt uitvoeren. 

    b. Bij **SAS URI aangepaste installatie container**  voert u desgewenst de SAS (Shared Access Signature) URI (Uniform Resource Identifier) van uw Azure Storage Blob-container in waar uw installatiescript en de bijbehorende bestanden zijn opgeslagen. Zie [Aangepaste installatie voor Azure-SSIS IR](https://docs.microsoft.com/en-us/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup). 

1. Op **een virtueel netwerk selecteren...**  selectievakje, selecteer of u wilt uw integratieruntime toevoegen aan een virtueel netwerk. Bekijk het als u gebruik van Azure SQL Database met een virtueel netwerk service-eindpunten/beheerd exemplaar naar SSISDB-host of toegang tot on-premises gegevens; vereisen dat wil zeggen, on-premises gegevensbronnen/bestemmingen hebt in uw SSIS-pakketten, Zie [deelnemen aan Azure-SSIS IR aan een virtueel netwerk](https://docs.microsoft.com/en-us/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Als u dit selectievakje inschakelt, kunt u de volgende stappen: 

   ![Geavanceerde instellingen met een virtueel netwerk](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)

    a. Voor **abonnement**, selecteer het Azure-abonnement waaraan uw virtuele netwerk. 

    b. Voor **locatie**, de locatie van uw integratieruntime is geselecteerd. 

    c. Voor **Type**, selecteer het type van het virtuele netwerk: klassiek of Azure Resource Manager. U wordt aangeraden dat u Azure Resource Manager virtueel netwerk selecteert, sinds het klassieke virtuele netwerk binnenkort worden afgeschaft. 

    d. Voor **VNet-naam**, selecteert u de naam van het virtuele netwerk. Dit virtuele netwerk moet worden gebruikt voor Azure SQL Database met virtual network service-eindpunten/beheerd exemplaar voor het hosten van SSISDB en of het certificaat dat is verbonden met uw on-premises netwerk hetzelfde virtuele netwerk. 

    e. Voor **subnetnaam**, selecteert u de naam van het subnet voor het virtuele netwerk. Dit moet een ander subnet dan de versie die wordt gebruikt voor beheerd exemplaar als host voor SSISDB. 

1. Klik op **VNet validatie** en als dit lukt, klikt u op **voltooien** om te beginnen met het maken van uw Azure-SSIS integratieruntime. 

    > [!IMPORTANT]
    > - Dit proces duurt ongeveer 20-30 minuten om te voltooien
    > - De Data Factory-service maakt verbinding met uw Azure SQL-database om de SSISDB (SSIS Catalog Database) voor te bereiden. Het script ook machtigingen en instellingen voor het virtuele netwerk geconfigureerd als is opgegeven en het nieuwe exemplaar van Azure-SSIS integratieruntime verbinding met het virtuele netwerk. 

1. Ga in het venster **Verbindingen** naar **Integratieruntimes**, indien nodig. Klik op **Vernieuwen** om de status te vernieuwen. 

   ![Status van het maken](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

1. Gebruik de koppelingen onder **acties** kolom om te stoppen/starten, bewerken of verwijderen van de integratieruntime. Gebruik de laatste koppeling om JSON-code weer te geven voor de integratieruntime. De knoppen Bewerken en Verwijderen zijn alleen beschikbaar wanneer de IR is gestopt. 

   ![Azure SSIS-IR - acties](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Azure SSIS-integratieruntimes in de portal
1. Ga in de Azure Data Factory-UI naar het tabblad **Bewerken**, klik op **Verbindingen** en ga vervolgens naar het tabblad **Integratieruntimes** om bestaande integratieruntimes in uw gegevensfactory weer te geven. 

   ![Bestaande IR's weergeven](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

1. Klik op **Nieuw** om een nieuwe Azure SSIS-IR te maken. 

   ![Integratieruntime via menu](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

1. Als u een Azure SSIS-integratieruntime wilt maken, klikt u op **Nieuw** zoals wordt weergegeven in de afbeelding. 
1. Selecteer in het venster Installatie van integratieruntime de optie voor het **verplaatsen van bestaande SSIS-pakketten voor uitvoering in Azure** en klik vervolgens op **Volgende**. 

   ![Geef het type integratieruntime op](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

1. Zie de sectie [Een Azure SSIS-integratieruntime inrichten](#provision-an-azure-ssis-integration-runtime) voor de resterende stappen voor het instellen van een Azure SSIS-IR. 

## <a name="azure-powershell"></a>Azure PowerShell
In deze sectie maakt u de Azure PowerShell gebruiken om een Azure-SSIS-IR te maken

### <a name="create-variables"></a>Variabelen maken
Definieer variabelen voor gebruik in het script in deze zelfstudie:

```powershell
### Azure Data Factory information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$DataFactoryLocation = "EastUS" 

### Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[specify a description for your Azure-SSIS IR]"
$AzureSSISLocation = "EastUS" 
# Only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_D4_v2"
# Only 1-10 nodes are supported.
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
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database with virtual network service endpoints/Managed Instance/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon    
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use a different subnet than the one used for your Managed Instance

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name or Managed Instance name.DNS prefix].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database or leave it empty for Managed Instance]"
```

### <a name="log-in-and-select-subscription"></a>Aanmelden en abonnement selecteren
Voeg de volgende code het script aan te melden en selecteer uw Azure-abonnement: 

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName
```

### <a name="validate-the-connection-to-database"></a>Valideren van verbinding met de database
Voeg het volgende script voor het valideren van het eindpunt voor uw Azure SQL Database-server. 

```powershell
# Validate only when you do not use VNet nor AAD authentication
if([string]::IsNullOrEmpty($VnetId) -and [string]::IsNullOrEmpty($SubnetName))
{
    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
    {
        $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword    
        $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
        Try
        {
            $sqlConnection.Open();
        }
        Catch [System.Data.SqlClient.SqlException]
        {
            Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
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
# Make sure to run this script against the subscription to which the virtual network belongs.
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
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
Voer de volgende opdrachten voor het maken van een Azure-SSIS integratieruntime die SSIS-pakketten in Azure uitvoert. 

Als u niet gebruik van Azure SQL Database met een virtueel netwerk service-eindpunten/beheerd exemplaar naar SSISDB-host of toegang tot on-premises gegevens nodig hebben, kunt u VNetId en Subnet parameters weglaat of lege waarden doorgeven voor hen. Anders kunnen niet laat ze en moet geldige waarden doorgeven van de configuratie van uw virtuele netwerk, Zie [deelnemen aan Azure-SSIS IR aan een virtueel netwerk](https://docs.microsoft.com/en-us/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). 

Als u Managed Instance naar host SSISDB, kunt u CatalogPricingTier parameter weglaat of een lege waarde voor het doorgeven. Anders u kan geen achterwege en moet een geldige waarde doorgeven in de lijst van ondersteunde Prijscategorieën voor Azure SQL Database, Zie [SQL Database-resourcebeperkingen](../sql-database/sql-database-resource-limits.md). 

Als u Azure Active Directory (AAD)-verificatie met uw Azure Data Factory Managed Service Identity (MSI) verbinding maken met de database-server, kunt u de parameter CatalogAdminCredential weglaten, maar u moet uw Data Factory-MSI-bestand toevoegen aan een AAD-groep met toegang machtigingen voor de database-server, Zie [inschakelen AAD-verificatie voor Azure-SSIS IR](https://docs.microsoft.com/en-us/azure/data-factory/enable-aad-authentication-azure-ssis-ir). Anders kan geen achterwege en een geldig object gevormd door uw server-beheerdersgebruikersnaam en wachtwoord voor SQL-verificatie moet slagen.

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
Dit is het volledige script dat wordt gemaakt van een Azure-SSIS integratieruntime. 

```powershell
### Azure Data Factory information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$DataFactoryLocation = "EastUS" 

### Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[specify a description for your Azure-SSIS IR]"
$AzureSSISLocation = "EastUS" 
# Only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_D4_v2"
# Only 1-10 nodes are supported.
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
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database with virtual network service endpoints/Managed Instance/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon    
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use a different subnet than the one used for your Managed Instance

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name or Managed Instance name.DNS prefix].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database or leave it empty for Managed Instance]"

### Log in and select subscription
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

### Validate the connection to database
# Validate only when you do not use VNet nor AAD authentication
if([string]::IsNullOrEmpty($VnetId) -and [string]::IsNullOrEmpty($SubnetName))
{
    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
    {
        $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword    
        $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
        Try
        {
            $sqlConnection.Open();
        }
        Catch [System.Data.SqlClient.SqlException]
        {
            Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
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
# Make sure to run this script against the subscription to which the virtual network belongs.
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
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
In deze sectie kunt u de Azure Resource Manager-sjabloon gebruiken voor Azure-SSIS integratieruntime maken. Hier volgt een voorbeeld-overzicht: 

1. Maak een JSON-bestand met de volgende Azure Resource Manager-sjabloon. Waarden in de punthaken (tijdelijke aanduidingen) vervangen door uw eigen waarden. 

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
    
1. Voer de opdracht New-AzureRmResourceGroupDeployment zoals wordt weergegeven in het volgende voorbeeld, waarbij ADFTutorialResourceGroup is de naam van de resourcegroep en ADFTutorialARM.json is het bestand met JSON te implementeren de Azure Resource Manager-sjabloon de definitie voor uw data factory en Azure-SSIS IR. 

    ```powershell
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json
    ```

    Deze opdracht maakt u uw data factory en Azure-SSIS IR erin, maar dit niet de IR wordt gestart 

1. Voor het starten van uw Azure-SSIS-IR, voert u Start-AzureRmDataFactoryV2IntegrationRuntime-opdracht uit: 

    ```powershell
    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName "<Resource Group Name>" `
                                                 -DataFactoryName "<Data Factory Name>" `
                                                 -Name "<Azure SSIS IR Name>" `
                                                 -Force
    ``` 

## <a name="deploy-ssis-packages"></a>SSIS-pakketten implementeren
Gebruik nu SQL Server Data Tools (SSDT) of SQL Server Management Studio (SSMS) om uw SSIS-pakketten te implementeren in Azure. Verbinding maken met uw database-server die als host fungeert voor de SSIS-catalogus (SSISDB). De naam van database-server is in de indeling: &lt;Azure SQL Database-servernaam&gt;. database.windows.net of &lt;Managed Instance-naam. DNS-voorvoegsel&gt;. database.windows.net. Zie het artikel [Pakketten implementeren](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server) voor instructies. 

## <a name="next-steps"></a>Volgende stappen
Zie de andere Azure-SSIS IR-onderwerpen in deze documentatie: 

- [Azure-SSIS Integratieruntime](concepts-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel bevat algemene informatie over integratieruntimes in het algemeen met inbegrip van de Azure-SSIS-IR. 
- [Zelfstudie: SSIS-pakketten implementeren in Azure](tutorial-create-azure-ssis-runtime-portal.md). Dit artikel biedt stapsgewijze instructies voor het maken van een Azure-SSIS IR en maakt gebruik van een Azure SQL database voor het hosten van de SSIS-catalogus. 
- [Een Azure-SSIS IR controleren](monitor-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel leest u hoe u informatie over een Azure-SSIS IR ophaalt. Daarnaast bevat het artikel beschrijvingen van statuswaarden die worden gebruikt in de geretourneerde informatie. 
- [Een Azure-SSIS IR beheren](manage-azure-ssis-integration-runtime.md). In dit artikel leest u hoe u een Azure-SSIS IR stopt, start of verwijdert. Er wordt ook uitgelegd hoe u een Azure-SSIS IR kunt uitschalen door meer knooppunten toe te voegen aan de IR. 
- [Een Azure-SSIS-integratieruntime toevoegen aan een virtueel netwerk](join-azure-ssis-integration-runtime-virtual-network.md). In dit artikel bevat algemene informatie over uw Azure-SSIS IR toevoegen aan een Azure-netwerk. Er wordt ook beschreven hoe u Azure Portal gebruikt om een virtueel netwerk te configureren voor het deelnemen van Azure-SSIS IR aan het virtueel netwerk. 
