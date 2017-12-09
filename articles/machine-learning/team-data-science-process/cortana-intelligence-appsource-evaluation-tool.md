---
title: Hulpprogramma voor het evalueren van oplossing Cortana Intelligence | Microsoft Docs
description: Als een Microsoft-Partner vindt hier u alle stappen die u moet volgen om te publiceren van uw oplossing Cortana Intelligence naar AppSource.
services: machine-learning
documentationcenter: 
author: AnupamMicrosoft
manager: jhubbard
editor: cgronlun
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: anupams;v-bruham;garye
ms.openlocfilehash: c9ea8164d5866662a7ed81672ee1ba776603b193
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="cortana-intelligence-solution-evaluation-tool"></a>Evaluatieprogramma voor Cortana Intelligence-oplossing
## <a name="overview"></a>Overzicht
U kunt de Cortana Intelligence oplossing evaluation tool gebruiken om uw oplossingen geavanceerde analyses voor compatibiliteit met Microsoft aanbevolen best practices vast te stellen. Microsoft werkt samen met onze partners is enthousiast (ISV's / SIs) om oplossingen van hoge kwaliteit voor klanten, resellers en implementatie. Deze handleiding wordt het proces van het hulpprogramma voor het evalueren van de oplossing voor gebruik met uw oplossing doorlopen en beschrijven de specifieke procedures in controles voor.

## <a name="getting-started"></a>Aan de slag
Neem [downloaden](https://aka.ms/aa-evaluation-tool-download) en de Cortana Intelligence oplossing evaluation tool te installeren.

Vereisten:
- Windows 10: [officiële site voor Windows 10](https://www.microsoft.com/en-us/windows)
- Azure Powershell: [installeren en configureren van Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).

## <a name="identifying-your-app"></a>Identificeren van uw app
Nadat de installatie is voltooid, opent u het hulpprogramma en uw eerste evaluatie starten.

![Hulpprogramma voor het Open evalueren](./media/cortana-intelligence-appsource-evaluation-tool/1-open-evaluation-tool.png)

Geef de identificatiegegevens over uw oplossing.

![Verbinding maken met Azure-abonnement](./media/cortana-intelligence-appsource-evaluation-tool/2-connect-azure-subscription.png)

Verbinding maken met uw Azure-abonnement en geef de resourcegroep met uw app.

![Selecteer resources](./media/cortana-intelligence-appsource-evaluation-tool/3-select-resources.png)

Zodra de resourcegroep zijn geladen, selecteer de resources die zijn opgenomen in uw oplossing en de toegankelijkheid van de resources van de gegevens als identificeren:
- Opname
- Verbruik
- Intern

We gebruiken deze gegevens om beter te begrijpen hoe uw oplossing wordt gebruikt door verschillende onderdelen en om ervoor te zorgen gebruikersgerichte onderdelen consistent zijn met best practices.

### <a name="ingestion"></a>Opname
Opname in dit geval betekent alle gegevensbronnen die worden gebruikt voor het ophalen van gegevens van buiten de oplossing of alle services buiten de oplossing gebruiken voor de push-gegevens in de App.

### <a name="consumption"></a>Verbruik
Verbruik houdt in dat geval alle gegevenssets die worden gebruikt voor de push-gegevens aan eindgebruikers, direct of indirect. Bijvoorbeeld:
- Gegevenssets gebruikt in de directe query van Power BI.
- Gegevenssets in een WebApp opgevraagd.

>[!NOTE]
Als een specifieke bron wordt gebruikt voor zowel opname en het verbruik van, kies **verbruik**.

### <a name="internal"></a>Intern
Interne voor resources die gegevens die worden gebruikt in de verwerking van de interne toepassing alleen gebruiken.

Vervolgens wordt u gevraagd geldige referenties opgeven voor alle databases in de vorige stap hebt opgegeven:

![Set test vereisten](./media/cortana-intelligence-appsource-evaluation-tool/4-set-test-prerequisites.png)

## <a name="solution-test-cases"></a>Testcases van de oplossing
Het hulpprogramma oplossing wordt een verzameling van automatische tests uitvoeren op uw oplossing.

![Uitvoering van test](./media/cortana-intelligence-appsource-evaluation-tool/5-set-test-execution.png)

Nadat de tests zijn voltooid, wordt u gevraagd om een uitleg of reden voor uw oplossing waarom niet aan de vereiste voldoet te geven.

![Zakelijke reden opgeven](./media/cortana-intelligence-appsource-evaluation-tool/6-provide-business-justification.png)

Bijvoorbeeld, als uw oplossing naar Azure SQL DW publiceert, moeten de evaluatie-tests u ook publiceren naar Azure Analysis Services. 

Uw oplossing kan IaaS virtuele machines met Sql Server Analysis Services in plaats van Azure Analysis Services gebruiken. Zou dit een acceptabele reden voor fout van de test.
## <a name="packaging-your-evaluation-results"></a>De resultaten van evaluatie van verpakking
Na het voltooien van de testcases uw evaluatiepakket worden geëxporteerd naar een zip-bestand en u feedback wilt geven over de evaluation tool gevraagd. 

U moet deze test resultaten zip-bestand delen met Microsoft voor uw oplossing moet worden geëvalueerd voordat u de goedkeuring om te worden toegevoegd aan AppSource

![Hulpprogramma voor het evalueren van klasse](./media/cortana-intelligence-appsource-evaluation-tool/7-grade-evaluation-tool.png)

Boven de sectie van dit artikel bevat informatie over de verschillende functies van het hulpprogramma, nu typen best practices die dit hulpprogramma wordt geëvalueerd als laat het ons te bekijken.

## <a name="security-evaluation-considerations"></a>Beveiligingsoverwegingen voor evaluatie
### <a name="databases-should-use-azure-active-directory-authentication"></a>Databases moeten u Azure Active Directory-verificatie gebruiken
Azure SQL- of Azure SQL DW bronnen in de sloution moeten worden ingeschakeld met Azure Active Directory (AAD)-verificatie. AAD biedt er één plaats is voor het beheren van alle identiteiten en rollen.

| Voor meer informatie over | Raadpleeg dit artikel |
| --- | --- |
| AAD met SQL-Database en SQL datawarehouse | [Azure Active Directory-verificatie gebruiken voor verificatie met SQL-Database of SQL Data Warehouse](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) |
| Configureren en beheren van AAD | [Configureren en beheren van Azure Active Directory-verificatie met SQL-Database of SQL Data Warehouse](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) |
| Azure WebApps-verificatie | [Verificatie en autorisatie in Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) |
| WebApps configureren met AAD | [Het configureren van uw App Service-toepassing voor het gebruik van Azure Active Directory-aanmelding](https://docs.microsoft.com/azure/app-service/app-service-mobile-how-to-configure-active-directory-authentication)|

### <a name="datasets-accessible-to-end-users-should-support-role-based-access-control"></a>Gegevenssets die toegankelijk is voor eindgebruikers moeten kunnen ondersteunen op rollen gebaseerde toegangsbeheer
Tijdens het uitvoeren van de evaluation tool, wordt u gevraagd om op te geven van een rapport of publiceren van bronnen. Ervan wordt uitgegaan dat deze informatie is bedoeld voor toegang door eindgebruikers, niet ontwikkelaars. Deze resources dient op rollen gebaseerde toegangsbeheer (RBAC) om ervoor te zorgen dat eindgebruikers alleen toegang tot geautoriseerde gegevens zijn.

In het bijzonder een van de volgende Azure-resources kunnen worden geconfigureerd met RBAC en als geldig beschouwd:
- Beveiligen van HDInsight, Zie [een inleiding tot Hadoop-beveiliging met HDInsight-clusters domein](https://docs.microsoft.com/azure/hdinsight/hdinsight-domain-joined-introduction)
- Azure SQL, Zie [AAD-verificatie met Azure SQL]( https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication)
- Azure Analysis Services, Zie [databaserollen en gebruikers beheren voor Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-database-users)
- Azure SQL Data Warehouse (zijn op de hoogte dat omdat SQL DW RBAC ondersteunt, het is niet raadzaam om direct door eindgebruikers toegang te krijgen.)

Als u een ander resourcetype die ondersteuning biedt voor RBAC gebruikt, geeft u die in de rechtvaardiging Testscenario.

### <a name="azure-data-lake-store-should-use-at-rest-encryption"></a>Azure Data Lake Store moet met rest versleuteling gebruiken
Azure Data Lake Store (ADLS) ondersteunt in rust codering standaard gebruik van coderingssleutels worden ADLS-beheerd. U kunt ook configureren met Azure Key Vault versleuteling.

Voor informatie over het opgeven van instellingen voor codering ADLS [een Azure Data Lake Store-account maken](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal#create-an-azure-data-lake-store-account).

### <a name="azure-sql-and-azure-sql-data-warehouse-should-use-encryption"></a>Azure SQL en Azure SQL Data Warehouse moet codering gebruiken
Azure SQL en Azure SQL DW ondersteund transparante gegevens codering (TDE), waarmee u realtime versleuteling en ontsleuteling van gegevens en de logboekbestanden.

| Voor meer informatie over | Raadpleeg dit artikel |
| --- | --- |
| Transparante gegevensversleuteling (TDE) | [Transparante gegevensversleuteling](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) |
| Azure SQL datawarehouse met TDE | [SQL Data Warehouse Encrption TDE TSQL](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-encryption-tde-tsql) |
| Azure SQL met TDE configureren | [Transparante gegevensversleuteling met Azure SQL Database](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database) |
| Configureer Azure SQL met altijd versleuteld. | [SQL-Database wordt altijd versleuteld Azure Sleutelkluis](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault)|

Naast TDE, Azure SQL ondersteunt ook altijd versleuteld, een nieuwe data encryption technologie die ervoor zorgt dat gegevens worden versleuteld niet alleen in rust en tijdens het verkeer tussen client en server, maar ook tijdens van gegevens wordt gebruikt tijdens het uitvoeren van opdrachten op de server.

### <a name="any-virtual-machines-must-be-deployed-from-the-azure-marketplace"></a>Alle virtuele Machines moeten worden geïmplementeerd vanuit Azure Marketplace
Als u wilt een consistente beveiligingsniveau bieden over AppSource, moet dat alle virtuele machines geïmplementeerd als onderdeel van een Cortana Intelligence-oplossing worden gecertificeerd en gepubliceerd op de Azure Marketplace.

Om te zoeken naar de huidige lijst van Azure Marketplace-installatiekopieën, Zie [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute).

Zie voor meer informatie over het publiceren van de installatiekopie van een virtuele machine voor Azure Marketplace [handleiding voor het maken van de installatiekopie van een virtuele machine voor Azure Marketplace](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation).

## <a name="scalability-evaluation-considerations"></a>Aandachtspunten voor schaalbaarheid evaluatie
### <a name="cortana-intelligence-solutions-should-include-a-scalable-big-data-platform"></a>Cortana Intelligence-oplossingen bevatten een schaalbare big data-platform
Cortana Intelligence-oplossingen wordt geschaald naar grootten voor zeer grote hoeveelheden gegevens. Azure, betekent dit dat ze bevatten een van de twee Petabyte scale gegevens platforms:
- Azure Data Lake Store
- Azure SQL Data Warehouse

Als uw oplossing geen ondersteuning voor deze gegevensgroottes vereist of als u een alternatieve gegevensplatform, uitgelegd u dit in de rechtvaardiging Testscenario.
### <a name="cortana-intelligence-solutions-should-include-dedicated-ingestion-data-environments"></a>Cortana Intelligence-oplossingen bevatten speciale opname gegevens omgevingen
Cortana Intelligence-oplossingen Vermijd gegevens rechtstreeks in relationele gegevensbronnen invoegen. In plaats daarvan moeten onbewerkte gegevens worden opgeslagen in een niet-gestructureerde-omgeving met idempotent inserts/updates in een relationele winkels met behulp van Azure Data Factory.

Voor meer informatie over het kopiëren van gegevens met Azure Data Factory [zelfstudie: een pijplijn maken met de Kopieeractiviteit in Visual Studio](https://docs.microsoft.com/azure/data-factory/v1/data-factory-copy-activity-tutorial-using-visual-studio).

### <a name="azure-sql-data-warehouse-should-use-polybase-for-data-ingestion"></a>Azure SQL Data Warehouse PolyBase voor gegevensopname moet worden gebruikt
Azure SQL DW ondersteunt PolyBase voor zeer schaalbaar, parallelle gegevensopname. PolyBase kunt u met Azure SQL DW probleem query's voor externe gegevenssets die zijn opgeslagen in Azure Blob Storage of Azure Data Lake Store. Dit biedt betere prestaties van alternatieve methoden voor bulksgewijze updates.

Zie voor instructies over het aan de slag met PolyBase en Azure SQL DW, [gegevens laden met PolyBase in SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-load-with-polybase).

Zie voor meer informatie over best practices met PolyBase en Azure SQL DW [handleiding voor het gebruik van PolyBase in SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-polybase-guide).

## <a name="availability-evaluation-considerations"></a>Overwegingen voor evaluatie van beschikbaarheid

### <a name="datasets-accessible-to-end-users-should-support-a-large-volume-of-concurrent-users"></a>Gegevenssets die toegankelijk is voor eindgebruikers moet een groot aantal gelijktijdige gebruikers ondersteunen
Tijdens het uitvoeren van de evaluation tool, wordt u gevraagd om op te geven van een rapport of publiceren van bronnen. Ervan wordt uitgegaan dat deze informatie is bedoeld voor toegang door eindgebruikers, niet ontwikkelaars. Deze resources moeten normaal grote aantallen gelijktijdige gebruikers ondersteunen.

Azure SQL Data Warehouse moet in het bijzonder niet de enige gegevensbron beschikbaar is voor eindgebruikers. Als Azure SQL DW is opgegeven als een resource voor Power Users, moet Azure Analysis Services beschikbaar worden gesteld voor typische gebruikers.

Zie voor meer informatie over de Azure SQL DW gelijktijdigheid limieten [gelijktijdigheid van taken en de belasting van beheer in SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-concurrency).

Zie voor meer informatie over Azure Analysis Services [Analysis Services-overzicht](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview).

### <a name="azure-sql-resources-should-have-a-read-only-replica-for-failover"></a>Azure SQL-resources moet een replica is alleen-lezen voor failover
Azure SQL-databases. ondersteunen geo-replicatie naar een secundaire exemplaar. Dit exemplaar kan vervolgens worden gebruikt als een failover-exemplaar waarmee toepassingen met hoge beschikbaarheid.

Zie voor meer informatie over geo-replicatie voor Azure SQL-databases, [SQL Database GEO-replicatie-overzicht](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview).

Zie voor instructies over het configureren van geo-replicatie voor Azure SQL [actieve geo-replicatie configureren voor Azure SQL Database met Transact-SQL](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-transact-sql).

### <a name="azure-sql-data-warehouse-should-have-geo-redundant-backups-enabled"></a>Azure SQL Data Warehouse hebt geografisch redundante back-ups die zijn ingeschakeld
Azure SQL DW biedt ondersteuning voor dagelijkse back-ups naar geografisch redundante opslag. Deze geo-replicatie zorgt ervoor dat u het datawarehouse zelfs in situaties waar u geen toegang momentopnamen die zijn opgeslagen in de primaire regio tot kunt herstellen. Deze functie is standaard ingeschakeld en moet niet uitschakelen voor de Cortana Intelligence-oplossingen.

Zie hier voor meer informatie over Azure SQL DW-back-ups en herstellen, [back-ups van SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-backups).

### <a name="virtual-machines-should-be-configured-with-availability-sets"></a>Virtuele machines moet worden geconfigureerd met beschikbaarheidssets
Virtuele machines in Azure moet worden geconfigureerd in beschikbaarheidssets om te beperken van het effect van geplande en ongeplande onderhoud gebeurtenissen.

Zie voor meer informatie over de beschikbaarheid van de virtuele machine van Azure [de beschikbaarheid van Windows virtuele machines in Azure beheren](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability).

## <a name="other-evaluation-considerations"></a>Andere overwegingen evaluatie
### <a name="cortana-intelligence-apps-should-use-a-centralized-tool-for-data-orchestration"></a>Cortana Intelligence apps moeten een gecentraliseerde hulpprogramma gebruiken voor gegevens orchestration
Met één hulpprogramma voor het beheren en plannen van de verplaatsing van gegevens en transformatie gezorgd voor consistentie rond essentiële gegevens. Het biedt een duidelijke logica rond Pogingslogica, Afhankelijkheidsbeheer, waarschuwing/logboekregistratie, enzovoort. We raden het gebruik van [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/v1/data-factory-introduction) voor beheer van de gegevens in Azure.

Als u van een hulpprogramma dan Azure Data Factory voor gegevens orchestration gebruikmaakt, beschrijf welke hulpprogramma of de hulpprogramma's.
### <a name="azure-machine-learning-models-should-be-retrained-using-azure-data-factory"></a>Azure Machine Learning-modellen moeten worden retrained met behulp van Azure Data Factory
Azure Machine Learning (AzureML), biedt eenvoudig te gebruiken hulpprogramma's voor het maken en de implementatie van voorspellende modellen en machine learning-pijplijnen. Het is echter belangrijk dat productie-implementaties van deze modellen AzureML niet is gebaseerd op een enkele vaste gegevensset, maar in plaats daarvan wordt aangepast aan de verschuivende dynamiek van echte verschijnselen.

Zie voor meer informatie over het maken van de retraining-webservices in AzureML [Retrain Machine Learning-modellen programmatisch](https://docs.microsoft.com/azure/machine-learning/machine-learning-retrain-models-programmatically).

Zie voor meer informatie over het automatiseren van het model trainen-proces gebruik van Azure Data Factory [bijwerken van Azure Machine Learning-modellen Update Resource activiteit](https://docs.microsoft.com/azure//data-factory/v1/data-factory-azure-ml-update-resource-activity).

## <a name="existing-documentation"></a>Bestaande documentatie
[Microsoft Azure is gecertificeerd voor uw bedrijf cloud uitbreiden](https://azure.microsoft.com/en-us/marketplace/programs/certified/)

[Microsoft Azure is gecertificeerd voor Cortana Intellignece](https://azure.microsoft.com/en-us/marketplace/programs/certified/cortana/)

