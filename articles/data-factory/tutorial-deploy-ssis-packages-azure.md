---
title: De Azure-SSIS-integratieruntime inrichten| Microsoft Docs
description: Informatie over het inrichten van de Azure-SSIS-integratieruntime in Azure Data Factory, zodat u SSIS-pakketten in Azure kunt implementeren en uitvoeren.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: tutorial
ms.date: 06/26/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: bd2c055d2f7f1e90918cb160cfdebfe88f7f8ea4
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67484796"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory"></a>De Azure-SSIS-integratieruntime inrichten in Azure Data Factory

Deze zelfstudie bevat de stappen voor het gebruik van de Azure-portal voor het inrichten van een Azure-SQL Server Integration Services (SSIS) Integration Runtime (IR) in Azure Data Factory (ADF). Azure-SSIS IR ondersteunt pakketten die zijn geïmplementeerd in de SSIS-catalogus (SSISDB) die wordt gehost door Azure SQL Database-server/beheerd exemplaar (Project-implementatiemodel) en die zijn geïmplementeerd in systemen/bestand uitvoeren shares/Azure-bestanden (pakket-implementatiemodel). Zodra het Azure-SSIS IR is ingericht, kunt u vervolgens vertrouwde hulpprogramma's, zoals SQL Server Data Tools (SSDT) gebruiken / SQL Server Management Studio (SSMS) en opdracht regel u hulpprogramma's, zoals `dtinstall` / `dtutil` / `dtexec`, naar implementeren en uw-pakketten uitvoeren in Azure. Zie [Overzicht van integratieruntime in Azure-SSIS](concepts-integration-runtime.md#azure-ssis-integration-runtime) voor algemene informatie over een Azure-SSIS IR.

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een data factory maken.
> * Een Azure-SSIS-integratieruntime inrichten.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure-abonnement**. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.
- **Azure SQL Database-server (optioneel)** . Als u nog geen een database-server, maakt u een in de Azure-portal voordat u begint. ADF maakt op zijn beurt SSISDB op deze databaseserver. Het wordt aangeraden om de databaseserver in dezelfde Azure-regio te maken als de Integration Runtime. Deze configuratie kan de integratieruntime uitvoeringslogboeken in SSISDB uitvoeringslogboeken wegschrijven zonder dat Azure-regio's. 
    - Op basis van de geselecteerde databaseserver kan SSISDB namens u worden gemaakt als een enkele database, als onderdeel van een elastische pool of in een beheerd exemplaar. Deze is toegankelijk in een openbaar netwerk of kan worden toegevoegd aan een virtueel netwerk. Zie voor richtlijnen bij het kiezen van het type van de database-server host SSISDB [vergelijken Azure SQL Database single database/elastische pool/beheerd exemplaar](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). Als u Azure SQL Database-server met virtual network-service-eindpunten/beheerd exemplaar in een virtueel netwerk hebt gebruikt voor het hosten van SSISDB of toegang tot on-premises gegevens nodig hebben, moet u uw Azure-SSIS IR toevoegen aan een virtueel netwerk, Zie [Azure-SSIS IR maken in een virtueel netwerk](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).
    - Controleer of de instelling **Toegang tot Azure-services toestaan** is ingeschakeld voor de databaseserver. Dit is niet van toepassing wanneer u Azure SQL Database-server met een virtueel netwerk service-eindpunten/beheerd exemplaar in een virtueel netwerk naar SSISDB-host. Zie [Secure your Azure SQL database](../sql-database/sql-database-security-tutorial.md#create-firewall-rules) (Azure SQL-database beveiligen) voor meer informatie. Zie voor meer informatie over het inschakelen van deze instelling met behulp van PowerShell [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule).
    - Het IP-adres van client-computer of een bereik van IP-adressen met het IP-adres van de clientcomputer naar de lijst met client IP-adres in de firewall-instellingen voor de database-server toevoegen. Zie [Overzicht van firewallregels op Azure SQL Database-serverniveau en -databaseniveau](../sql-database/sql-database-firewall-configure.md) voor meer informatie.
    - U kunt verbinding maken met de database-server met behulp van SQL-verificatie met uw referenties voor de serverbeheerder of Azure Active Directory (AAD) met de beheerde identiteit voor uw ADF. Voor het laatste moet u de beheerde identiteit voor de ADF toevoegen aan een AAD-groep met machtigingen voor toegang tot de databaseserver. Zie [Azure-SSIS IR met AAD-verificaties maken](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).
    - Bevestig dat de database-server beschikt niet over een SSISDB al. Het inrichten van een Azure-SSIS-IR biedt geen ondersteuning met behulp van een bestaande SSISDB.

> [!NOTE]
> - Zie voor een lijst van Azure-regio's waarin ADF en Azure-SSIS IR op dit moment beschikbaar zijn, [ADF + SSIS-IR beschikbaarheid per regio](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all). 

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Start de webbrowser **Microsoft Edge** of **Google Chrome**. Op dit moment wordt de Data Factory-gebruikersinterface alleen ondersteund in de webbrowsers Microsoft Edge en Google Chrome. 
1. Meld u aan bij [Azure Portal](https://portal.azure.com/). 
1. Selecteer **Nieuw** in het menu links, selecteer **Gegevens en analyses** en selecteer vervolgens **Data Factory**. 

   ![Selectie van Data Factory in het deelvenster Nieuw](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)

1. Voer op de pagina **Nieuwe gegevensfactory** **MyAzureSsisDataFactory** in voor **Name**. 

   ![Pagina Nieuwe data factory](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)

   De naam van de Azure-gegevensfactory moet *wereldwijd uniek* zijn. Als u het volgende foutbericht krijgt, wijzigt u de naam van de gegevensfactory (bijvoorbeeld in **&lt;uwnaam&gt;MyAzureSsisDataFactory**) en probeert u het opnieuw. Zie het artikel [Data factory - Naamgevingsregels](naming-rules.md) voor naamgevingsregels voor Data Factory-artefacten. 

   `Data factory name “MyAzureSsisDataFactory” is not available`

1. Selecteer voor **Abonnement** het Azure-abonnement waarin u de gegevensfactory wilt maken. 
1. Voer een van de volgende stappen uit voor **Resourcegroep**: 

   - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de lijst. 
   - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in. 

   Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie. 
1. Selecteer **V2 (Preview)** voor **Versie**. 
1. Selecteer voor **Locatie** een locatie voor de data factory. De lijst bevat alleen locaties die worden ondersteund voor het maken van gegevensfactory’s. 
1. Selecteer **Vastmaken aan dashboard**. 
1. Selecteer **Maken**. 
1. Op het dashboard ziet u de volgende tegel met de status: **Data Factory implementeren**: 

   ![Tegel Data Factory implementeren](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)

1. Zodra de bewerking is voltooid, ziet u de pagina **Data Factory**. 

   ![Startpagina voor de gegevensfactory](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)

1. Selecteer **Maken en controleren** om de gebruikersinterface van Data Factory te openen op een nieuw tabblad. 

## <a name="create-an-azure-ssis-integration-runtime"></a>Een Azure SSIS Integration Runtime maken

### <a name="from-the-data-factory-overview"></a>Vanuit het overzicht van Data Factory

1. Selecteer op de pagina **Aan de slag** de tegel **SSIS-integratieruntime configureren**. 

   ![De tegel SSIS-integratieruntime configureren](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

1. Zie de sectie [Een Azure-SSIS-integratieruntime inrichten](#provision-an-azure-ssis-integration-runtime) voor de resterende stappen voor het instellen van een Azure-SSIS-IR. 

### <a name="from-the-authoring-ui"></a>Vanuit de gebruikersinterface Ontwerpen

1. Ga in de gebruikersinterface van Azure Data Factory naar het tabblad **Bewerken**, selecteer **Verbindingen** en ga vervolgens naar het tabblad **Integratieruntimes** om bestaande integratieruntimes in uw gegevensfactory weer te geven. 

   ![Selecties voor het weergeven van bestaande IR’s](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

1. Selecteer **Nieuw** om een Azure-SSIS-IR te maken. 

   ![Integratieruntime via menu](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

1. Selecteer in het venster **Installatie van integratieruntime** de**optie voor het verplaatsen van bestaande SSIS-pakketten voor uitvoering in Azure** en selecteer vervolgens **Volgende**. 

   ![Geef het type integratieruntime op](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

1. Zie de sectie [Een Azure-SSIS-integratieruntime inrichten](#provision-an-azure-ssis-integration-runtime) voor de resterende stappen voor het instellen van een Azure-SSIS-IR. 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Een Azure-SSIS-integratieruntime inrichten

1. Voer op de pagina **Algemene instellingen** van **Installatie van integratieruntime** de volgende stappen uit: 

   ![Algemene instellingen](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

   a. Voer bij **Naam** de naam van de integratieruntime in. 

   b. Voer bij **beschrijving** de beschrijving van de integratieruntime in. 

   c. Selecteer bij **Locatie** de locatie voor de integratieruntime. Alleen ondersteunde locaties worden weergegeven. We raden u aan dezelfde locatie van uw databaseserver te selecteren voor het hosten van SSISDB. 

   d. Selecteer bij **Knooppuntgrootte** de grootte van knooppunt in het integratieruntimecluster. Alleen ondersteunde knooppuntgrootten worden weergegeven. Selecteer een grote knooppuntgrootte (omhoog schalen) als u veel reken-/geheugenintensieve pakketten wilt uitvoeren. 

   e. Selecteer bij **Aantal knooppunten** het aantal knooppunten in het integratieruntimecluster. Alleen ondersteunde knooppuntaantallen worden weergegeven. Selecteer een groot cluster met veel knooppunten (uitschalen), als u veel pakketten parallel wilt uitvoeren. 

   f. Selecteer bij **Editie/licentie** de SQL Server-editie/licentie voor uw integratie runtime: Standard of Enterprise. Selecteer Enterprise als u geavanceerde/premium functies in de integratieruntime wilt gebruiken. 

   g. Selecteer bij **Geld besparen** de optie Azure Hybrid Benefit (AHB) voor uw integratieruntime: Ja of Nee. Selecteer Ja als u uw eigen SQL Server-licentie met Software Assurance wilt gebruiken om te profiteren van de kostenbesparingen met hybride gebruik. 

   h. Klik op **volgende**. 

1. Voer op de pagina **SQL-instellingen** de volgende stappen uit: 

   ![SQL-instellingen](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

   a. Op **maken SSIS-catalogus...**  selectievakje, selecteer de implementatie voor pakketten uit te voeren op uw Azure-SSIS IR model: Project-implementatiemodel wanneer pakketten worden geïmplementeerd in de SSISDB wordt gehost door de database-server of pakket implementatiemodel wanneer pakketten worden geïmplementeerd in uw bestand systemen/file shares/Azure-bestanden. Als u dit selectievakje inschakelt, moet u uw eigen database-server op de host SSISDB die we maken en beheren namens brengen.
   
   b. Selecteer bij **Abonnement** het Azure-abonnement dat uw databaseserver heeft voor het hosten van SSISDB. 

   c. Selecteer bij **Locatie** de locatie van uw databaseserver voor het hosten van SSISDB. We raden u aan dezelfde locatie van uw integratieruntime te selecteren. 

   d. Selecteer bij het **Eindpunt voor de Catalog-databaseserver** het eindpunt van uw databaseserver voor het hosten van SSISDB. Op basis van de geselecteerde databaseserver kan SSISDB namens u worden gemaakt als een enkele database, als onderdeel van een elastische pool of in een beheerd exemplaar. Deze is toegankelijk in een openbaar netwerk of kan worden toegevoegd aan een virtueel netwerk. Zie voor richtlijnen bij het kiezen van het type van de database-server host SSISDB [vergelijken Azure SQL Database single database/elastische pool/beheerd exemplaar](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). Als u Azure SQL Database-server met virtual network service-eindpunten/beheerd exemplaar in een virtueel netwerk selecteert voor het hosten van SSISDB of toegang tot on-premises gegevens nodig hebben, moet u uw Azure-SSIS IR toevoegen aan een virtueel netwerk, Zie [maken-Azure-SSIS IR in een virtueel netwerk](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 

   e. Selecteer op het selectievakje **AAD-verificatie gebruiken...**  de verificatiemethode voor de databaseserver voor het hosten van SSISDB: SQL-verificatie of AAD-verificatie met de beheerde identiteit voor uw ADF. Als u dit selectievakje inschakelt, moet u de beheerde identiteit voor uw ADF toevoegen aan een AAD-groep met machtigingen voor toegang tot uw databaseserver, Zie [Azure-SSIS IR maken met AAD-verificatie](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 

   f. Voer bij **Gebruikersnaam van beheerder** de gebruikersnaam voor SQL-verificatie voor uw databaseserver voor het hosten van SSISDB in. 

   g. Voer bij **Beheerderswachtwoord** het wachtwoord voor SQL-verificatie voor uw databaseserver voor het hosten van SSISDB in. 

   h. Selecteer bij **Serverlaag catalogusdatabase** de servicelaag voor uw databaseserver voor het hosten van SSISDB: Basic-/Standard-/Premium-laag of de naam van een elastische pool. 

   i. Klik op **Verbinding testen**. Als dit lukt, klikt u op **Volgende**. 

1. Voer op de pagina **Geavanceerde instellingen** de volgende stappen uit: 

   ![Geavanceerde instellingen](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

   a. Selecteer bij het **maximale aantal parallelle uitvoeringen per knooppunt** het maximum aantal pakketten dat gelijktijdig per knooppunt kan worden uitgevoerd in het integratieruntimecluster. Alleen ondersteunde pakketaantallen worden weergegeven. Selecteer een klein aantal als u meer dan één kern wilt gebruiken om een enkel groot/zwaar pakket uit te voeren dat reken-/geheugenintensief is. Selecteer een groot aantal als u een of meer kleine/lichte pakketten in één kern wilt uitvoeren. 

   b. Bij **SAS URI aangepaste installatie container**  voert u desgewenst de SAS (Shared Access Signature) URI (Uniform Resource Identifier) van uw Azure Storage Blob-container in waar uw installatiescript en de bijbehorende bestanden zijn opgeslagen. Zie [Aangepaste installatie voor Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup). 

   c. Geef via het selectievakje **Een VNet selecteren...**  aan of u de integratieruntime aan een virtueel netwerk wilt toevoegen. Controleer het als u Azure SQL Database-server met virtual network service-eindpunten/beheerd exemplaar in een virtueel netwerk naar host SSISDB of toegang nodig tot on-premises gegevens, Zie [Azure-SSIS IR maken in een virtueel netwerk](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 

1. Klik op **Voltooien** om te beginnen met het maken van de integratieruntime. 

   > [!NOTE]
   > Met uitzondering van elk gewenst moment aangepaste installatie, moet dit proces binnen vijf minuten worden voltooid.
   >
   > Als u gebruikmaakt van SSISDB, verbinding ADF-service met uw databaseserver SSISDB voorbereiden. 
   > 
   > Als u een Azure-SSIS-IR inricht, worden de Access Redistributable en Azure Feature Pack voor SSIS opdrachtprompts ook geïnstalleerd. Deze onderdelen bieden connectiviteit met Excel/toegang tot bestanden en verschillende Azure-gegevensbronnen, naast de gegevensbronnen die al worden ondersteund door de ingebouwde onderdelen. U kunt ook extra onderdelen installeren Zie [aangepaste instellingen voor Azure-SSIS IR](how-to-configure-azure-ssis-ir-custom-setup.md).

1. Ga op het tabblad **Verbindingen** indien nodig naar **Integratieruntimes**. Selecteer **Vernieuwen** om de status te vernieuwen. 

   ![De status vernieuwen](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

1. Gebruik de koppelingen in de kolom **Acties** om de integratieruntime te stoppen/starten, te bewerken of te verwijderen. Gebruik de laatste koppeling om JSON-code weer te geven voor de integratieruntime. De knoppen Bewerken en Verwijderen zijn alleen beschikbaar wanneer de IR is gestopt. 

   ![Koppelingen in de kolom 'Acties'](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png) 

## <a name="deploy-ssis-packages"></a>SSIS-pakketten implementeren

Als u gebruikmaakt van SSISDB, kunt u uw pakketten implementeren in deze en voer ze uit op Azure-SSIS IR met SSDT/SSMS-hulpprogramma's die verbinding met uw database-server via het servereindpunt maken. Voor Azure SQL Database server-/ MI met een openbaar eindpunt, de indeling van de server-eindpunt is `<server name>.database.windows.net` / `<server name>.public.<dns prefix>.database.windows.net,3342`, respectievelijk. Als u niet SSISDB gebruikt, kunt u uw pakketten implementeren in bestand systemen/file shares/Azure bestanden en voer ze uit over het gebruik van Azure-SSIS IR `dtinstall` / `dtutil` / `dtexec` opdrachtregelprogramma's. Zie voor meer informatie, [implementeren van SSIS-pakketten](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server). In beide gevallen kunt u uw geïmplementeerde pakketten ook uitvoeren op Azure-SSIS IR met behulp van de activiteit uitvoeren van SSIS-pakket in ADF pijplijnen, Zie [aanroepen SSIS-pakketuitvoering als een eersteklas ADF-activiteit](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity). 

Zie ook de volgende artikelen uit de SSIS-documentatie: 

- [Implementeren, uitvoeren en bewaken van SSIS-pakketten in Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) 
- [Verbinding maken met SSISDB in Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) 
- [Uitvoeringen van de schema-pakket in Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) 
- [Connect to on-premises data sources with Windows authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Verbinding maken met on-premises gegevensbronnen met Windows-verificatie) 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd: 

> [!div class="checklist"]
> * Een data factory maken.
> * Een Azure-SSIS-integratieruntime inrichten.
> * SSIS-pakketten implementeren

Voor meer informatie over uw Azure-SSIS-integratieruntime, gaat u verder met het volgende artikel: 

> [!div class="nextstepaction"]
> [Azure-SSIS IR aanpassen](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)