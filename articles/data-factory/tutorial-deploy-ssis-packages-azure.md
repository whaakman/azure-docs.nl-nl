---
title: De Azure-SSIS-integratieruntime inrichten| Microsoft Docs
description: Informatie over het inrichten van de Azure-SSIS-integratieruntime in Azure Data Factory, zodat u SSIS-pakketten in Azure kunt implementeren en uitvoeren.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: hero-article
ms.date: 06/27/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 89cf9c9034c03b6ca51aca4bd2c4cd6edb8bcc13
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37084226"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory"></a>De Azure-SSIS-integratieruntime inrichten in Azure Data Factory
Deze zelfstudie bevat de stappen voor het gebruik van Azure Portal om een Azure SSIS-integratieruntime (IR) in te richten in Azure Data Factory. U kunt SQL Server Data Tools of SQL Server Management Studio dan gebruiken om pakketten van SQL Server Integration Services (SSIS) te implementeren en uit te voeren in deze runtime van Azure. Zie [Overzicht van integratieruntime in Azure-SSIS](concepts-integration-runtime.md#azure-ssis-integration-runtime) voor algemene informatie over een Azure-SSIS IR.

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een data factory maken.
> * Een Azure-SSIS-integratieruntime inrichten.

## <a name="prerequisites"></a>Vereisten
- **Azure-abonnement**. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint. 
- **Azure SQL Database-server**. Als u nog geen databaseserver hebt, maakt u die in Azure Portal voordat u begint. Azure Data Factory maakt de SSIS-catalogus (SSISDB-database) op deze databaseserver. Het wordt aangeraden om de databaseserver in dezelfde Azure-regio te maken als de Integration Runtime. Met deze configuratie kan de integratieruntime uitvoeringslogboeken wegschrijven naar de SSISDB-database zonder dat hierbij Azure-regio's worden overschreden. 
- Op basis van de geselecteerde databaseserver kan SSISDB namens u worden gemaakt als een enkele database, als onderdeel van een elastische pool of in een beheerd exemplaar (preview). Deze is toegankelijk in een openbaar netwerk of kan worden toegevoegd aan een virtueel netwerk. Als u SQL Database gebruikt met eindpunten van een virtueel netwerk/een beheerd exemplaar (preview) voor het hosten van SSISDB of toegang tot on-premises gegevens vereist, moet u uw Azure-SSIS IR toevoegen aan een virtueel netwerk. Zie [Azure-SSIS IR in een virtueel netwerk maken](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). 
- Controleer of de instelling **Toegang tot Azure-services toestaan** is ingeschakeld voor de databaseserver. Dit is niet van toepassing wanneer u Azure SQL Database met eindpunten van een virtueel netwerk/een beheerd exemplaar (preview) gebruikt voor het hosten van SSISDB. Zie [Secure your Azure SQL database](../sql-database/sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal) (Azure SQL-database beveiligen) voor meer informatie. Zie [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule?view=azurermps-4.4.1) om deze instelling met behulp van PowerShell in te schakelen. 
- Voeg het IP-adres van de clientcomputer (of een reeks IP-adressen dat het IP-adres van de clientcomputer bevat) toe aan de lijst met client-IP-adressen in de instellingen van de firewall voor de databaseserver. Zie [Overzicht van de firewallregels voor Azure SQL Database](../sql-database/sql-database-firewall-configure.md) voor meer informatie. 
- U kunt verbinding maken met de databaseserver via SQL-verificatie met de beheerdersreferenties van uw server of AAD-verificaties (Azure Active Directory) met uw ADF (Azure Data Factory) Managed Service Identity (MSI).  Voor het laatste moet u uw ADF MSI toevoegen aan een AAD-groep met machtigingen voor toegang tot de databaseserver. Zie [Azure-SSIS IR met AAD-verificaties maken](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). 
- Controleer of uw Azure SQL Database-server geen SSIS-catalogus (SSISDB-database) heeft. Het inrichten van een Azure-SSIS-IR biedt geen ondersteuning voor het gebruik van een bestaande SSIS-catalogus. 

> [!NOTE]
> - Voor een lijst met Azure-regio’s waarin Data Factory momenteel beschikbaar is, selecteert u op de volgende pagina de regio’s waarin u geïnteresseerd bent, vouwt u vervolgens **Analytics** uit en gaat u naar **Data Factory**: [Beschikbare producten per regio](https://azure.microsoft.com/global-infrastructure/services/). 
> - Voor een lijst met Azure-regio’s waarin Azure-SSIS Integration Runtime momenteel beschikbaar is, selecteert u op de volgende pagina de regio’s waarin u geïnteresseerd bent. Vervolgens vouwt u **Analytics** uit en gaat u naar **SSIS Integration Runtime**: [Beschikbare producten per regio](https://azure.microsoft.com/global-infrastructure/services/). 

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Start de webbrowser **Microsoft Edge** of **Google Chrome**. Op dit moment wordt de Data Factory-gebruikersinterface alleen ondersteund in de webbrowsers Microsoft Edge en Google Chrome. 
2. Meld u aan bij [Azure Portal](https://portal.azure.com/). 
3. Selecteer **Nieuw** in het menu links, selecteer **Gegevens en analyses** en selecteer vervolgens **Data Factory**. 

   ![Selectie van Data Factory in het deelvenster Nieuw](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)

4. Voer op de pagina **Nieuwe gegevensfactory** **MyAzureSsisDataFactory** in voor **Name**. 

   ![Pagina Nieuwe data factory](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)

   De naam van de Azure-gegevensfactory moet *wereldwijd uniek* zijn. Als u het volgende foutbericht krijgt, wijzigt u de naam van de gegevensfactory (bijvoorbeeld in **&lt;uwnaam&gt;MyAzureSsisDataFactory**) en probeert u het opnieuw. Zie het artikel [Data factory - Naamgevingsregels](naming-rules.md) voor naamgevingsregels voor Data Factory-artefacten. 

   `Data factory name “MyAzureSsisDataFactory” is not available`

5. Selecteer voor **Abonnement** het Azure-abonnement waarin u de gegevensfactory wilt maken. 
6. Voer een van de volgende stappen uit voor **Resourcegroep**: 

   - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de lijst. 
   - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in. 

   Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie. 
7. Selecteer **V2 (Preview)** voor **Versie**. 
8. Selecteer voor **Locatie** een locatie voor de data factory. De lijst bevat alleen locaties die worden ondersteund voor het maken van gegevensfactory’s. 
9. Selecteer **Vastmaken aan dashboard**. 
10. Selecteer **Maken**. 
11. Op het dashboard ziet u de volgende tegel met de status: **Data Factory implementeren**: 

   ![Tegel Data Factory implementeren](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)

12. Zodra de bewerking is voltooid, ziet u de pagina **Data Factory**. 

   ![Startpagina voor de gegevensfactory](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)

13. Selecteer **Maken en controleren** om de gebruikersinterface van Data Factory te openen op een nieuw tabblad. 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Een Azure-SSIS-integratieruntime inrichten

1. Selecteer op de pagina **Aan de slag** de tegel **SSIS-integratieruntime configureren**. 

   ![De tegel SSIS-integratieruntime configureren](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

2. Voer op de pagina **Algemene instellingen** van **Installatie van integratieruntime** de volgende stappen uit: 

   ![Algemene instellingen](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

   a. Voer bij **Naam** de naam van de integratieruntime in. 

   b. Voer bij **beschrijving** de beschrijving van de integratieruntime in. 

   c. Selecteer bij **Locatie** de locatie voor de integratieruntime. Alleen ondersteunde locaties worden weergegeven. We raden u aan dezelfde locatie van uw databaseserver te selecteren voor het hosten van SSISDB. 

   d. Selecteer bij **Knooppuntgrootte** de grootte van knooppunt in het integratieruntimecluster. Alleen ondersteunde knooppuntgrootten worden weergegeven. Selecteer een grote knooppuntgrootte (omhoog schalen) als u veel reken-/geheugenintensieve pakketten wilt uitvoeren. 

   e. Selecteer bij **Aantal knooppunten** het aantal knooppunten in het integratieruntimecluster. Alleen ondersteunde knooppuntaantallen worden weergegeven. Selecteer een groot cluster met veel knooppunten (uitschalen), als u veel pakketten parallel wilt uitvoeren. 

   f. Selecteer bij **Editie/licentie** de SQL Server-editie/licentie voor uw integratie runtime: Standard of Enterprise. Selecteer Enterprise als u geavanceerde/premium functies in de integratieruntime wilt gebruiken. 

   g. Selecteer bij **Geld besparen** de Azure Hybrid Benefit (AHB)-optie voor uw integratieruntime: Ja of Nee. Selecteer Ja als u uw eigen SQL Server-licentie met Software Assurance wilt gebruiken om te profiteren van de kostenbesparingen met hybride gebruik. 

   h. Klik op **Volgende**. 

3. Voer op de pagina **SQL-instellingen** de volgende stappen uit: 

   ![SQL-instellingen](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

   a. Selecteer bij **Abonnement** het Azure-abonnement dat uw databaseserver heeft voor het hosten van SSISDB. 

   b. Selecteer bij **Locatie** de locatie van uw databaseserver voor het hosten van SSISDB. We raden u aan dezelfde locatie van uw integratieruntime te selecteren. 

   c. Selecteer bij het **Eindpunt voor de Catalog-databaseserver** het eindpunt van uw databaseserver voor het hosten van SSISDB. Op basis van de geselecteerde databaseserver kan SSISDB namens u worden gemaakt als een zelfstandige database, als onderdeel van een elastische pool of in een beheerd exemplaar (preview) en is deze toegankelijk in een openbaar netwerk of kan deze worden toegevoegd aan een virtueel netwerk. Zie [SQL Database en een beheerd exemplaar (preview) vergelijken](create-azure-ssis-integration-runtime.md#compare-sql-database-and-managed-instance-preview) voor hulp bij het kiezen van het type databaseserver om SSISDB te hosten. Als u Azure SQL Database gebruikt met eindpunten van een virtueel netwerk/een beheerd exemplaar (preview) om SSISDB te hosten, of als toegang tot on-premises gegevens is vereist, moet u uw Azure-SSIS IR toevoegen aan een virtueel netwerk. Zie [Azure-SSIS IR maken in een virtueel netwerk](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). 

   d. Selecteer bij het selectievakje **AAD-verificaties gebruiken...**  de verificatiemethode voor uw databaseserver voor het hosten van SSISDB: SQL of Azure Active Directory (AAD) met uw Azure Data Factory (ADF) Managed Service Identity (MSI). Als u deze inschakelt, moet u uw ADF MSI toevoegen aan een AAD-groep met machtigingen voor toegang tot de databaseserver. Zie [Azure-SSIS IR met AAD-verificaties maken](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). 

   e. Voer bij **Gebruikersnaam van beheerder** de gebruikersnaam voor SQL-verificatie voor uw databaseserver voor het hosten van SSISDB in. 

   f. Voer bij **Beheerderswachtwoord** het wachtwoord voor SQL-verificatie voor uw databaseserver voor het hosten van SSISDB in. 

   g. Selecteer bij de **servicelaag van de Catalog-database** de servicelaag voor uw databaseserver voor het hosten van SSISDB: Basic-/Standard-/Premium-laag of de naam van de elastische pool. 

   h. Klik op **Verbinding testen**. Als dit lukt, klikt u op **Volgende**. 

4. Voer op de pagina **Geavanceerde instellingen** de volgende stappen uit: 

   ![Geavanceerde instellingen](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

   a. Selecteer bij het **maximale aantal parallelle uitvoeringen per knooppunt** het maximum aantal pakketten dat gelijktijdig per knooppunt kan worden uitgevoerd in het integratieruntimecluster. Alleen ondersteunde pakketaantallen worden weergegeven. Selecteer een klein aantal als u meer dan één kern wilt gebruiken om een enkel groot/zwaar pakket uit te voeren dat reken-/geheugenintensief is. Selecteer een groot aantal als u een of meer kleine/lichte pakketten in één kern wilt uitvoeren. 

   b. Bij **SAS URI aangepaste installatie container**  voert u desgewenst de SAS (Shared Access Signature) URI (Uniform Resource Identifier) van uw Azure Storage Blob-container in waar uw installatiescript en de bijbehorende bestanden zijn opgeslagen. Zie [Aangepaste installatie voor Azure-SSIS IR](https://docs.microsoft.com/en-us/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup). 

   c. Geef via het selectievakje **Een VNet selecteren...**  aan of u de integratieruntime aan een virtueel netwerk wilt toevoegen. U moet dit selectievakje inschakelen als u SQL Database gebruikt met eindpunten van een virtueel netwerk/een beheerd exemplaar (preview) voor het hosten van SSISDB of toegang tot on-premises gegevens vereist. Zie [Azure-SSIS IR in een virtueel netwerk maken](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). 

5. Klik op **Voltooien** om te beginnen met het maken van de integratieruntime. 

   > [!IMPORTANT]
   > Het voltooien van dit proces duurt ongeveer 20 tot 30 minuten.
   >
   > De Data Factory-service maakt verbinding met uw Azure SQL Database-server om de SSIS-catalogus (SSISDB-database) voor te bereiden. 
   > 
   > Wanneer u een exemplaar van een Azure-SSI-IR inricht, worden ook het Azure Feature Pack voor SSIS en de Access Redistributable geïnstalleerd. Deze onderdelen bieden connectiviteit met Excel- en Access-bestanden en met verschillende Azure-gegevensbronnen, naast de gegevensbronnen die worden ondersteund door de ingebouwde onderdelen. U kunt ook extra onderdelen installeren. Zie [Aangepaste instelling voor de Azure-SSIS-integratieruntime](how-to-configure-azure-ssis-ir-custom-setup.md) voor meer informatie. 

6. Ga op het tabblad **Verbindingen** indien nodig naar **Integratieruntimes**. Selecteer **Vernieuwen** om de status te vernieuwen. 

   ![De status vernieuwen](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

7. Gebruik de koppelingen in de kolom **Acties** om de integratieruntime te stoppen/starten, te bewerken of te verwijderen. Gebruik de laatste koppeling om JSON-code weer te geven voor de integratieruntime. De knoppen Bewerken en Verwijderen zijn alleen beschikbaar wanneer de IR is gestopt. 

   ![Koppelingen in de kolom 'Acties'](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png) 

## <a name="create-an-azure-ssis-integration-runtime"></a>Een Azure SSIS Integration Runtime maken

1. Ga in de gebruikersinterface van Azure Data Factory naar het tabblad **Bewerken**, selecteer **Verbindingen** en ga vervolgens naar het tabblad **Integratieruntimes** om bestaande integratieruntimes in uw gegevensfactory weer te geven. 

   ![Selecties voor het weergeven van bestaande IR’s](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

2. Selecteer **Nieuw** om een Azure-SSIS-IR te maken. 

   ![Integratieruntime via menu](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

3. Selecteer in het venster **Installatie van integratieruntime** de**optie voor het verplaatsen van bestaande SSIS-pakketten voor uitvoering in Azure** en selecteer vervolgens **Volgende**. 

   ![Geef het type integratieruntime op](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

4. Zie de sectie [Een Azure-SSIS-integratieruntime inrichten](#provision-an-azure-ssis-integration-runtime) voor de resterende stappen voor het instellen van een Azure-SSIS-IR. 

## <a name="deploy-ssis-packages"></a>SSIS-pakketten implementeren
Gebruik nu SQL Server Data Tools (SSDT) of SQL Server Management Studio (SSMS) om uw SSIS-pakketten te implementeren in Azure. Maak verbinding met de Azure SQL Database-server waarop de SSIS-catalogus (SSISDB-database) wordt gehost. De naam van de Azure SQL Database-server heeft de notatie `<servername>.database.windows.net`. 

Raadpleeg de volgende artikelen uit de SSIS-documentatie: 

- [Deploy, run, and monitor an SSIS package on Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) (SSIS-pakket implementeren, uitvoeren en bewaken in Azure) 
- [Connect to the SSISDB Catalog database on Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) (Verbinding maken met de SSISDB-catalogusdatabase in Azure) 
- [Schedule package execution on Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) (Pakketuitvoering plannen in Azure) 
- [Connect to on-premises data sources with Windows authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Verbinding maken met on-premises gegevensbronnen met Windows-verificatie) 

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd: 

> [!div class="checklist"]
> * Een data factory maken.
> * Een Azure-SSIS-integratieruntime inrichten.

Ga verder met de volgende zelfstudie als u wilt weten hoe u on-premises gegevens kopieert naar de cloud: 

> [!div class="nextstepaction"]
> [Gegevens kopiëren in de cloud](tutorial-copy-data-portal.md)
