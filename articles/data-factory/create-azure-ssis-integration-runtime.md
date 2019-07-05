---
title: Azure-SSIS Integratieruntime maken in Azure Data Factory | Microsoft Docs
description: Informatie over het maken van Azure-SSIS Integration Runtime in Azure Data Factory, zodat u kunt implementeren en uitvoeren van SSIS-pakketten in Azure.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/26/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: fb5335c8dfd94006ba3f0d8d6b890869dd9f3717
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67484822"
---
# <a name="create-azure-ssis-integration-runtime-in-azure-data-factory"></a>Azure-SSIS Integratieruntime in Azure Data Factory maken

Deze zelfstudie bevat de stappen voor het inrichten van een Azure-SQL Server Integration Services (SSIS) Integration Runtime (IR) in Azure Data Factory (ADF). Azure-SSIS IR ondersteunt pakketten die zijn geïmplementeerd in de SSIS-catalogus (SSISDB) die wordt gehost door Azure SQL Database-server/beheerd exemplaar (Project-implementatiemodel) en die zijn geïmplementeerd in systemen/bestand uitvoeren shares/Azure-bestanden (pakket-implementatiemodel). Zodra het Azure-SSIS IR is ingericht, kunt u vervolgens vertrouwde hulpprogramma's, zoals SQL Server Data Tools (SSDT) gebruiken / SQL Server Management Studio (SSMS) en opdracht regel u hulpprogramma's, zoals `dtinstall` / `dtutil` / `dtexec`, naar implementeren en uw-pakketten uitvoeren in Azure.

De [zelfstudie: Inrichten van Azure-SSIS IR](tutorial-create-azure-ssis-runtime-portal.md) ziet u hoe u een Azure-SSIS IR via de Azure portal/ADF-app maken en Azure SQL Database-server/beheerd exemplaar op host SSISDB (optioneel) gebruiken. In dit artikel gaat verder in op de zelfstudie en ziet u hoe u het volgende doen:

- (Optioneel) gebruik van Azure SQL Database-server met virtual network service-eindpunten/beheerd exemplaar in een virtueel netwerk naar host SSISDB. Als een vereiste moet u machtigingen/instellingen van virtuele netwerken voor uw Azure-SSIS IR om lid van een virtueel netwerk te configureren.

- Azure Active Directory (AAD)-verificatie met de beheerde identiteit voor uw ADF (optioneel) gebruiken voor verbinding met Azure SQL Database-server/beheerd exemplaar. Als een vereiste moet u de beheerde identiteit voor uw ADF toevoegen als een geschikt voor het maken van SSISDB databasegebruiker.

## <a name="overview"></a>Overzicht

In dit artikel ziet u inrichting Azure-SSIS IR op verschillende manieren:

- [Azure-portal](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [Azure Resource Manager-sjabloon](#azure-resource-manager-template)

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure-abonnement**. Als u nog geen een abonnement, kunt u een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/) account.
- **Azure SQL Database-server/Managed Instance (optioneel)** . Als u nog geen een database-server, maakt u een in de Azure-portal voordat u begint. ADF maakt op zijn beurt SSISDB op deze databaseserver. Het wordt aangeraden om de databaseserver in dezelfde Azure-regio te maken als de Integration Runtime. Deze configuratie kan de integratieruntime uitvoeringslogboeken in SSISDB uitvoeringslogboeken wegschrijven zonder dat Azure-regio's. 
    - Op basis van de geselecteerde databaseserver kan SSISDB namens u worden gemaakt als een enkele database, als onderdeel van een elastische pool of in een beheerd exemplaar. Deze is toegankelijk in een openbaar netwerk of kan worden toegevoegd aan een virtueel netwerk. Zie voor richtlijnen bij het kiezen van het type van de database-server host SSISDB [vergelijken Azure SQL Database single database/elastische pool/beheerd exemplaar](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). Als u Azure SQL Database-server met virtual network-service-eindpunten/beheerd exemplaar in een virtueel netwerk hebt gebruikt voor het hosten van SSISDB of toegang tot on-premises gegevens nodig hebben, moet u uw Azure-SSIS IR toevoegen aan een virtueel netwerk, Zie [lid worden van Azure-SSIS IR aan een virtueel netwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).
    - Controleer of de instelling **Toegang tot Azure-services toestaan** is ingeschakeld voor de databaseserver. Dit is niet van toepassing wanneer u Azure SQL Database-server met een virtueel netwerk service-eindpunten/beheerd exemplaar in een virtueel netwerk naar SSISDB-host. Zie [Secure your Azure SQL database](../sql-database/sql-database-security-tutorial.md#create-firewall-rules) (Azure SQL-database beveiligen) voor meer informatie. Zie voor meer informatie over het inschakelen van deze instelling met behulp van PowerShell [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule).
    - Het IP-adres van client-computer of een bereik van IP-adressen met het IP-adres van de clientcomputer naar de lijst met client IP-adres in de firewall-instellingen voor de database-server toevoegen. Zie [Overzicht van firewallregels op Azure SQL Database-serverniveau en -databaseniveau](../sql-database/sql-database-firewall-configure.md) voor meer informatie.
    - U kunt verbinding maken met de database-server met behulp van SQL-verificatie met uw referenties voor de serverbeheerder of Azure Active Directory (AAD) met de beheerde identiteit voor uw ADF.  Voor de laatste, moet u de beheerde identiteit voor uw ADF toevoegen aan een AAD-groep met machtigingen voor toegang tot de database-server, Zie [inschakelen AAD-verificatie voor Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir).
    - Bevestig dat de database-server beschikt niet over een SSISDB al. Het inrichten van een Azure-SSIS-IR biedt geen ondersteuning met behulp van een bestaande SSISDB.
- **Azure Resource Manager-netwerk (optioneel)** . U hebt een virtueel netwerk van Azure Resource Manager als ten minste één van de volgende voorwaarden voldaan wordt:
    - U kunt SSISDB worden gehost op Azure SQL Database-server met virtual network-service-eindpunten/beheerd exemplaar in een virtueel netwerk.
    - U wilt verbinding maken met on-premises gegevens gegevensarchieven vanuit SSIS-pakketten die worden uitgevoerd op uw Azure-SSIS-IR.
- **Azure PowerShell (optioneel)** . Volg de instructies op [hoe u Azure PowerShell installeren en configureren](/powershell/azure/install-az-ps), als u uitvoeren van een PowerShell-script wilt voor het inrichten van uw Azure-SSIS-IR.

### <a name="region-support"></a>Ondersteuning voor regio

Zie voor een overzicht van Azure-regio's, waarin ADF en Azure-SSIS IR momenteel beschikbaar is zijn, [ADF + SSIS-IR beschikbaarheid per regio](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all).

### <a name="compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance"></a>SQL Database single database/elastische pool en SQL Database Managed Instance vergelijken

De volgende tabel vergelijkt bepaalde functies van Azure SQL Database-server en de Managed Instance zoals ze betrekking op Azure-SSIR IR hebben:

| Functie | één database/elastische pool| Beheerd exemplaar |
|---------|--------------|------------------|
| **Planning** | SQL Server Agent is niet beschikbaar.<br/><br/>Zie [plant de uitvoering van een pakket in ADF pijplijn](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages?view=sql-server-2017#activity).| Beheerde exemplaar Agent is beschikbaar. |
| **Verificatie** | U kunt SSISDB maken met een ingesloten databasegebruiker voor een AAD-groep met de beheerde identiteit van de ADF als een lid in de **db_owner** rol.<br/><br/>Zie [inschakelen Azure AD-verificatie SSISDB maken in Azure SQL Database-server](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database). | U kunt de SSISDB maken met een ingesloten databasegebruiker voor de beheerde identiteit van de ADF. <br/><br/>Zie [inschakelen Azure AD-verificatie SSISDB maken in Azure SQL Database Managed Instance](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database-managed-instance). |
| **Servicelaag** | Als u Azure-SSIS IR met uw Azure SQL Database-server maken, selecteert u de servicelaag voor SSISDB. Er zijn meerdere service-lagen. | Wanneer u Azure-SSIS IR met uw beheerde exemplaar maken, kunt u de servicelaag niet selecteren voor SSISDB. Alle databases in uw beheerde exemplaar delen dezelfde resource toegewezen aan deze instantie. |
| **Virtueel netwerk** | Biedt ondersteuning voor virtuele netwerken alleen Azure Resource Manager voor uw Azure-SSIS IR moeten worden samengevoegd als u Azure SQL Database-server met service-eindpunten voor virtueel netwerk gebruiken of toegang tot on-premises gegevensopslagexemplaren nodig hebben. | Biedt ondersteuning voor virtuele netwerken alleen Azure Resource Manager voor uw Azure-SSIS IR om toe te voegen. Het virtuele netwerk is altijd vereist.<br/><br/>Als u uw Azure-SSIS IR aan hetzelfde virtuele netwerk bevinden als uw beheerde exemplaar toevoegen, zorg ervoor dat uw Azure-SSIS IR in een ander subnet dan uw beheerde exemplaar is. Als u uw Azure-SSIS IR aan een ander virtueel netwerk dan het beheerde exemplaar toevoegen, raden wij een peering op virtueel netwerk of een virtueel netwerk met virtuele netwerk. Zie [verbinding maken met uw toepassing naar Azure SQL Database Managed Instance](../sql-database/sql-database-managed-instance-connect-app.md). |
| **Gedistribueerde transacties** | Ondersteund via elastische transacties. Microsoft Distributed Transaction Coordinator (MSDTC)-transacties worden niet ondersteund. Als uw SSIS-pakketten MSDTC gebruiken gedistribueerde transacties te coördineren, kunt u migreren naar elastische transacties voor Azure SQL Database. Zie voor meer informatie, [gedistribueerde transacties tussen clouddatabases](../sql-database/sql-database-elastic-transactions-overview.md). | Wordt niet ondersteund. |
| | | |

## <a name="azure-portal"></a>Azure Portal

In deze sectie gebruikt u Azure portal, specifiek ADF gebruikersinterface (UI) / app, om Azure-SSIS IR te maken

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

2. Voer op de pagina **Algemene instellingen** van **Installatie van integratieruntime** de volgende stappen uit:

   ![Algemene instellingen](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    a. Voer bij **Naam** de naam van de integratieruntime in.

    b. Voer bij **beschrijving** de beschrijving van de integratieruntime in.

    c. Selecteer bij **Locatie** de locatie voor de integratieruntime. Alleen ondersteunde locaties worden weergegeven. We raden u aan dezelfde locatie van uw databaseserver te selecteren voor het hosten van SSISDB.

    d. Selecteer bij **Knooppuntgrootte** de grootte van knooppunt in het integratieruntimecluster. Alleen ondersteunde knooppuntgrootten worden weergegeven. Selecteer een grote knooppuntgrootte (omhoog schalen) als u veel reken-/geheugenintensieve pakketten wilt uitvoeren.

    e. Selecteer bij **Aantal knooppunten** het aantal knooppunten in het integratieruntimecluster. Alleen ondersteunde knooppuntaantallen worden weergegeven. Selecteer een groot cluster met veel knooppunten (uitschalen), als u veel pakketten parallel wilt uitvoeren.

    f. Selecteer bij **Editie/licentie** de SQL Server-editie/licentie voor uw integratie runtime: Standard of Enterprise. Selecteer Enterprise als u geavanceerde/premium functies in de integratieruntime wilt gebruiken.

    g. Selecteer bij **Geld besparen** de optie Azure Hybrid Benefit (AHB) voor uw integratieruntime: Ja of Nee. Selecteer Ja als u uw eigen SQL Server-licentie met Software Assurance wilt gebruiken om te profiteren van de kostenbesparingen met hybride gebruik.

    h. Klik op **volgende**.

3. Voer op de pagina **SQL-instellingen** de volgende stappen uit:

   ![SQL-instellingen](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    a. Op **maken SSIS-catalogus...**  selectievakje, selecteer de implementatie voor pakketten uit te voeren op uw Azure-SSIS IR model: Project-implementatiemodel wanneer pakketten worden geïmplementeerd in de SSISDB wordt gehost door de database-server of pakket implementatiemodel wanneer pakketten worden geïmplementeerd in uw bestand systemen/file shares/Azure-bestanden. Als u dit selectievakje inschakelt, moet u uw eigen database-server op de host SSISDB die we maken en beheren namens brengen.
   
    b. Selecteer bij **Abonnement** het Azure-abonnement dat uw databaseserver heeft voor het hosten van SSISDB. 

    c. Selecteer bij **Locatie** de locatie van uw databaseserver voor het hosten van SSISDB. We raden u aan dezelfde locatie van uw integratieruntime te selecteren. 

    d. Selecteer bij het **Eindpunt voor de Catalog-databaseserver** het eindpunt van uw databaseserver voor het hosten van SSISDB. Op basis van de geselecteerde databaseserver kan SSISDB namens u worden gemaakt als een enkele database, als onderdeel van een elastische pool of in een beheerd exemplaar. Deze is toegankelijk in een openbaar netwerk of kan worden toegevoegd aan een virtueel netwerk. Zie voor richtlijnen bij het kiezen van het type van de database-server host SSISDB [vergelijken Azure SQL Database single database/elastische pool/beheerd exemplaar](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). Als u Azure SQL Database-server met virtual network service-eindpunten/beheerd exemplaar in een virtueel netwerk selecteert voor het hosten van SSISDB of toegang tot on-premises gegevens nodig hebben, moet u uw Azure-SSIS IR toevoegen aan een virtueel netwerk, Zie [lid worden van Azure-SSIS IR met een virtueel netwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). 

    e. Selecteer op het selectievakje **AAD-verificatie gebruiken...**  de verificatiemethode voor de databaseserver voor het hosten van SSISDB: SQL-verificatie of AAD-verificatie met de beheerde identiteit voor uw ADF. Als u dit selectievakje inschakelt, moet u de beheerde identiteit voor uw ADF toevoegen aan een AAD-groep met machtigingen voor toegang tot uw databaseserver, Zie [inschakelen AAD-verificatie voor Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir). 

    f. Voer bij **Gebruikersnaam van beheerder** de gebruikersnaam voor SQL-verificatie voor uw databaseserver voor het hosten van SSISDB in. 

    g. Voer bij **Beheerderswachtwoord** het wachtwoord voor SQL-verificatie voor uw databaseserver voor het hosten van SSISDB in. 

    h. Selecteer bij **Serverlaag catalogusdatabase** de servicelaag voor uw databaseserver voor het hosten van SSISDB: Basic-/Standard-/Premium-laag of de naam van een elastische pool. 

    i. Klik op **Verbinding testen**. Als dit lukt, klikt u op **Volgende**. 

4. Voer op de pagina **Geavanceerde instellingen** de volgende stappen uit:

    ![Geavanceerde instellingen](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

    a. Selecteer bij het **maximale aantal parallelle uitvoeringen per knooppunt** het maximum aantal pakketten dat gelijktijdig per knooppunt kan worden uitgevoerd in het integratieruntimecluster. Alleen ondersteunde pakketaantallen worden weergegeven. Selecteer een lage waarde als u meer dan één kern gebruiken wilt voor het uitvoeren van een enkele grote/zware-weight-pakket dat compute/geheugen-intensief. Selecteer een groot aantal als u een of meer kleine/lichte pakketten in één kern wilt uitvoeren.

    b. Bij **SAS URI aangepaste installatie container**  voert u desgewenst de SAS (Shared Access Signature) URI (Uniform Resource Identifier) van uw Azure Storage Blob-container in waar uw installatiescript en de bijbehorende bestanden zijn opgeslagen. Zie [Aangepaste installatie voor Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

5. Op **een virtueel netwerk selecteren...**  selectievakje, selecteer of u wilt uw integratieruntime toevoegen aan een virtueel netwerk. Bekijk het als u Azure SQL Database-server met virtual network service-eindpunten/beheerd exemplaar in een virtueel netwerk naar host SSISDB of toegang tot on-premises gegevens; vereisen dat wil zeggen, on-premises gegevensbronnen/bestemmingen hebt in uw SSIS-pakketten, Zie [deelnemen aan Azure-SSIS IR aan een virtueel netwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Als u dit selectievakje inschakelt, kunt u de volgende stappen:

   ![Geavanceerde instellingen met een virtueel netwerk](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)

    a. Voor **abonnement**, selecteer het Azure-abonnement waaraan uw virtuele netwerk.

    b. Voor **locatie**, de locatie van uw integratieruntime is geselecteerd.

    c. Voor **Type**, selecteer het type van het virtuele netwerk: Klassieke of Azure Resource Manager. U wordt aangeraden dat u Azure Resource Manager virtueel netwerk selecteert, sinds het klassieke virtuele netwerk binnenkort worden afgeschaft.

    d. Voor **VNet-naam**, selecteert u de naam van het virtuele netwerk. Dit virtuele netwerk moet hetzelfde account voor Azure SQL Database-server met virtual network-service-eindpunten/beheerd exemplaar in een virtueel netwerk wordt gebruikt voor het hosten van SSISDB of het bestand dat is verbonden met uw on-premises netwerk.

    e. Voor **subnetnaam**, selecteert u de naam van het subnet voor het virtuele netwerk. Dit moet een ander subnet dan de versie die wordt gebruikt voor beheerd exemplaar in een virtueel netwerk naar SSISDB-host.

6. Klik op **VNet validatie** en als dit lukt, klikt u op **voltooien** om te beginnen met het maken van uw Azure-SSIS integratieruntime.

    > [!NOTE]
    > Met uitzondering van elk gewenst moment aangepaste installatie dit proces moet binnen vijf minuten worden voltooid, maar kan ongeveer 20-30 minuten duren voor Azure-SSIS IR toevoegen van een virtueel netwerk.
    >
    > Als u gebruikmaakt van SSISDB, verbinding ADF-service met uw databaseserver SSISDB voorbereiden. Ook machtigingen en instellingen voor het virtuele netwerk configureert als is opgegeven en lid wordt van uw Azure-SSIS IR aan het virtuele netwerk.
    > 
    > Als u een Azure-SSIS-IR inricht, worden de Access Redistributable en Azure Feature Pack voor SSIS opdrachtprompts ook geïnstalleerd. Deze onderdelen bieden connectiviteit met Excel/toegang tot bestanden en verschillende Azure-gegevensbronnen, naast de gegevensbronnen die al worden ondersteund door de ingebouwde onderdelen. U kunt ook extra onderdelen installeren Zie [aangepaste instellingen voor Azure-SSIS IR](how-to-configure-azure-ssis-ir-custom-setup.md).

7. Ga in het venster **Verbindingen** naar **Integratieruntimes**, indien nodig. Klik op **Vernieuwen** om de status te vernieuwen.

   ![Status van het maken](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

8. Gebruik de koppelingen onder **acties** kolom om te stoppen/starten, bewerken of verwijderen van de integratieruntime. Gebruik de laatste koppeling om JSON-code weer te geven voor de integratieruntime. De knoppen Bewerken en Verwijderen zijn alleen beschikbaar wanneer de IR is gestopt.

   ![Azure SSIS-IR - acties](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Azure SSIS-integratieruntimes in de portal

1. Ga in de Azure Data Factory-UI naar het tabblad **Bewerken**, klik op **Verbindingen** en ga vervolgens naar het tabblad **Integratieruntimes** om bestaande integratieruntimes in uw gegevensfactory weer te geven.

   ![Bestaande IR's weergeven](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

2. Klik op **Nieuw** om een nieuwe Azure SSIS-IR te maken.

   ![Integratieruntime via menu](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

3. Als u een Azure SSIS-integratieruntime wilt maken, klikt u op **Nieuw** zoals wordt weergegeven in de afbeelding.

4. Selecteer in het venster Installatie van integratieruntime de optie voor het **verplaatsen van bestaande SSIS-pakketten voor uitvoering in Azure** en klik vervolgens op **Volgende**.

   ![Geef het type integratieruntime op](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

5. Zie de sectie [Een Azure SSIS-integratieruntime inrichten](#provision-an-azure-ssis-integration-runtime) voor de resterende stappen voor het instellen van een Azure SSIS-IR.

## <a name="azure-powershell"></a>Azure PowerShell

In deze sectie maakt u de Azure PowerShell gebruiken om een Azure-SSIS-IR te maken

### <a name="create-variables"></a>Variabelen maken

Kopieer en plak het volgende script: Geef waarden op voor de variabelen. 

```powershell
### Azure Data Factory information
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - Must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported, but for other nodes, up to (2 x number of cores) are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database server with virtual network service endpoints/Managed Instance in a virtual network/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use the same subnet as the one used with your Azure SQL Database server with virtual network service endpoints or a different subnet than the one used for your Managed Instance in a virtual network

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or Managed Instance name.DNS prefix.database.windows.net or Managed Instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, please ensure that there is no existing SSISDB on your database server, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc., see https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for Managed Instance]"
```

### <a name="sign-in-and-select-subscription"></a>Aanmelden en abonnement selecteren

Voeg de volgende code in en selecteer uw Azure-abonnement met het script:

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName
```

### <a name="validate-the-connection-to-database-server"></a>Valideren van de verbinding met database-server

Voeg het volgende script voor het valideren van uw Azure SQL Database-server/beheerd exemplaar.

```powershell
# Validate only if you use SSISDB and do not use VNet or AAD authentication
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
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
}
```

### <a name="configure-virtual-network"></a>Virtueel netwerk configureren

Voeg het volgende script toe om automatisch machtigingen/instellingen voor het virtuele netwerk te configureren waarmee uw Azure SSIS Integration Runtime moet worden verbonden.

```powershell
# Make sure to run this script against the subscription to which the virtual network belongs
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) met behulp van de [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) opdracht. Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd.

Als uw resourcegroep al bestaat, moet u deze code niet naar uw script kopiëren. 

```powershell
New-AzResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

### <a name="create-a-data-factory"></a>Een gegevensfactory maken

Voer de volgende opdracht uit om een data factory te maken.

```powershell
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

### <a name="create-an-integration-runtime"></a>Een Integration Runtime maken

Voer de volgende opdrachten voor het maken van een Azure-SSIS integratieruntime die SSIS-pakketten in Azure uitvoert.

Als u niet SSISDB, kunt u parameters CatalogServerEndpoint, CatalogPricingTier en CatalogAdminCredential weglaten.

Als u geen Azure SQL Database-server met virtual network service-eindpunten/beheerd exemplaar in een virtueel netwerk naar host SSISDB gebruiken of toegang tot on-premises gegevens nodig hebben, kunt u parameters VNetId en Subnet weglaat of lege waarden doorgeven voor hen. Anders kunnen niet laat ze en moet geldige waarden doorgeven van de configuratie van uw virtuele netwerk, Zie [deelnemen aan Azure-SSIS IR aan een virtueel netwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).

Als u Managed Instance naar host SSISDB, kunt u CatalogPricingTier parameter weglaat of een lege waarde voor het doorgeven. Anders u kan geen achterwege en moet een geldige waarde doorgeven in de lijst van ondersteunde Prijscategorieën voor Azure SQL Database, Zie [SQL Database-resourcebeperkingen](../sql-database/sql-database-resource-limits.md).

Als u Azure Active Directory (AAD)-verificatie met de beheerde identiteit voor uw ADF verbinding maken met de database-server gebruikt, kunt u de parameter CatalogAdminCredential weglaten, maar u moet de beheerde identiteit voor uw ADF toevoegen in een AAD-groep met toegang machtigingen voor de database-server, Zie [inschakelen AAD-verificatie voor Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir). Anders kan geen achterwege en een geldig object gevormd door uw server-beheerdersgebruikersnaam en wachtwoord voor SQL-verificatie moet slagen.

```powershell
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
       
# Add CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword)) # Add CatalogAdminCredential parameter if you do not use AAD authentication
    {
        $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
        $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                   -DataFactoryName $DataFactoryName `
                                                   -Name $AzureSSISName `
                                                   -CatalogAdminCredential $serverCreds
    }
}

# Add SetupScriptContainerSasUri parameter if you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}
```

### <a name="start-integration-runtime"></a>Integration Runtime starten

Voer de volgende opdrachten om te starten van de Azure-SSIS integratieruntime.

```powershell
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

> [!NOTE]
> Met uitzondering van elk gewenst moment aangepaste installatie dit proces moet binnen vijf minuten worden voltooid, maar kan ongeveer 20-30 minuten duren voor Azure-SSIS IR toevoegen van een virtueel netwerk.
>
> Als u gebruikmaakt van SSISDB, verbinding ADF-service met uw databaseserver SSISDB voorbereiden. Ook machtigingen en instellingen voor het virtuele netwerk configureert als is opgegeven en lid wordt van uw Azure-SSIS IR aan het virtuele netwerk.
> 
> Als u een Azure-SSIS-IR inricht, worden de Access Redistributable en Azure Feature Pack voor SSIS opdrachtprompts ook geïnstalleerd. Deze onderdelen bieden connectiviteit met Excel/toegang tot bestanden en verschillende Azure-gegevensbronnen, naast de gegevensbronnen die al worden ondersteund door de ingebouwde onderdelen. U kunt ook extra onderdelen installeren Zie [aangepaste instellingen voor Azure-SSIS IR](how-to-configure-azure-ssis-ir-custom-setup.md).

### <a name="full-script"></a>Volledige script

Dit is het volledige script dat wordt gemaakt van een Azure-SSIS integratieruntime.

```powershell
### Azure Data Factory information
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - Must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported, but for other nodes, up to (2 x number of cores) are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database server with virtual network service endpoints/Managed Instance in a virtual network/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use the same subnet as the one used with your Azure SQL Database server with virtual network service endpoints or a different subnet than the one used for your Managed Instance in a virtual network

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or Managed Instance name.DNS prefix.database.windows.net or Managed Instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, please ensure that there is no existing SSISDB on your database server, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc., see https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for Managed Instance]"

### Sign in and select subscription
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName

### Validate the connection to database server
# Validate only if you use SSISDB and do not use VNet or AAD authentication
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
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
}

### Configure virtual network
# Make sure to run this script against the subscription to which the virtual network belongs
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}

### Create a data factory
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName

### Create an integration runtime
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
       
# Add CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword)) # Add CatalogAdminCredential parameter if you do not use AAD authentication
    {
        $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
        $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                   -DataFactoryName $DataFactoryName `
                                                   -Name $AzureSSISName `
                                                   -CatalogAdminCredential $serverCreds
    }
}

# Add SetupScriptContainerSasUri parameter when you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}

### Start integration runtime
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
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
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {},
        "variables": {},
        "resources": [{
            "name": "<Specify a name for your data factory>",
            "apiVersion": "2018-06-01",
            "type": "Microsoft.DataFactory/factories",
            "location": "East US",
            "properties": {},
            "resources": [{
                "type": "integrationruntimes",
                "name": "<Specify a name for your Azure-SSIS IR>",
                "dependsOn": [ "<The name of the data factory you specified at the beginning>" ],
                "apiVersion": "2018-06-01",
                "properties": {
                    "type": "Managed",
                    "typeProperties": {
                        "computeProperties": {
                            "location": "East US",
                            "nodeSize": "Standard_D8_v3",
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

2. Voor het implementeren van de Azure Resource Manager-sjabloon, nieuw-AzResourceGroupDeployment opdracht zoals wordt weergegeven in het volgende voorbeeld, waarbij ADFTutorialResourceGroup is de naam van de resourcegroep en ADFTutorialARM.json is het bestand met JSON-definitie voor uitvoeren uw data factory en Azure-SSIS IR.

    ```powershell
    New-AzResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json
    ```

    Deze opdracht maakt u uw data factory en Azure-SSIS IR erin, maar dit niet de IR wordt gestart

3. Voor het starten van uw Azure-SSIS-IR, voert u Start AzDataFactoryV2IntegrationRuntime-opdracht uit:

    ```powershell
    Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName "<Resource Group Name>" `
                                                 -DataFactoryName "<Data Factory Name>" `
                                                 -Name "<Azure SSIS IR Name>" `
                                                 -Force
    ```

> [!NOTE]
> Met uitzondering van elk gewenst moment aangepaste installatie dit proces moet binnen vijf minuten worden voltooid, maar kan ongeveer 20-30 minuten duren voor Azure-SSIS IR toevoegen van een virtueel netwerk.
>
> Als u gebruikmaakt van SSISDB, verbinding ADF-service met uw databaseserver SSISDB voorbereiden. Ook machtigingen en instellingen voor het virtuele netwerk configureert als is opgegeven en lid wordt van uw Azure-SSIS IR aan het virtuele netwerk.
> 
> Als u een Azure-SSIS-IR inricht, worden de Access Redistributable en Azure Feature Pack voor SSIS opdrachtprompts ook geïnstalleerd. Deze onderdelen bieden connectiviteit met Excel/toegang tot bestanden en verschillende Azure-gegevensbronnen, naast de gegevensbronnen die al worden ondersteund door de ingebouwde onderdelen. U kunt ook extra onderdelen installeren Zie [aangepaste instellingen voor Azure-SSIS IR](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="deploy-ssis-packages"></a>SSIS-pakketten implementeren

Als u gebruikmaakt van SSISDB, kunt u uw pakketten implementeren in deze en voer ze uit op Azure-SSIS IR met SSDT/SSMS-hulpprogramma's die verbinding met uw database-server via het servereindpunt maken.  Voor Azure SQL Database-server/beheerd exemplaar in een virtueel netwerk-/ MI met een openbaar eindpunt, de indeling van de server-eindpunt is `<server name>.database.windows.net` / `<server name>.<dns prefix>.database.windows.net` / `<server name>.public.<dns prefix>.database.windows.net,3342`, respectievelijk. Als u niet SSISDB gebruikt, kunt u uw pakketten implementeren in bestand systemen/file shares/Azure bestanden en voer ze uit over het gebruik van Azure-SSIS IR `dtinstall` / `dtutil` / `dtexec` opdrachtregelprogramma's. Zie voor meer informatie, [implementeren van SSIS-pakketten](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server). In beide gevallen kunt u uw geïmplementeerde pakketten ook uitvoeren op Azure-SSIS IR met behulp van de activiteit uitvoeren van SSIS-pakket in ADF pijplijnen, Zie [aanroepen SSIS-pakketuitvoering als een eersteklas ADF-activiteit](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

## <a name="next-steps"></a>Volgende stappen

Zie ook andere Azure-SSIS IR-onderwerpen in deze documentatie:

- [Azure-SSIS Integratieruntime](concepts-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel bevat informatie over integratieruntimes in het algemeen met inbegrip van Azure-SSIS IR.
- [Azure-SSIS IR controleren](monitor-integration-runtime.md#azure-ssis-integration-runtime). Dit artikel leest u hoe u kunt ophalen en meer informatie over gegevens over uw Azure-SSIS-IR.
- [Azure-SSIS IR beheren](manage-azure-ssis-integration-runtime.md). Dit artikel leest u hoe om te stoppen, starten of verwijderen van uw Azure-SSIS-IR - deze ook wordt beschreven hoe u uw Azure-SSIS IR uitbreiden door meer knooppunten toe te voegen.
- [Azure-SSIS IR toevoegen aan een virtueel netwerk](join-azure-ssis-integration-runtime-virtual-network.md). In dit artikel bevat informatie over uw Azure-SSIS IR toevoegen aan een virtueel netwerk.