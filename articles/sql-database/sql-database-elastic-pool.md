---
title: Beheer van meerdere SQL-databases met elastische pools-Azure | Microsoft Docs
description: Beheren en schalen van meerdere SQL-databases - honderden en duizendtallen - met elastische pools. Één prijs voor resources die u distribueren kunt waar nodig.
keywords: meerdere databases, databaseresources en prestaties van de database
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.date: 06/07/2018
ms.author: ninarn
ms.topic: conceptual
ms.openlocfilehash: 6e58d3ed84771cedda126511e868ad264db88606
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34850488"
---
# <a name="elastic-pools-help-you-manage-and-scale-multiple-azure-sql-databases"></a>Elastische pools helpen u te beheren en schalen van meerdere Azure SQL-databases

SQL-Database elastische pools zijn een eenvoudige, rendabele oplossing voor het beheren en schalen van meerdere databases die voldoen aan verschillende en onvoorspelbare veeleisende hebben. De databases in een elastische pool zijn op één Azure SQL Database-server en een aantal bronnen op een vaste prijs delen. Met elastische groepen in Azure SQL Database kunnen SaaS-ontwikkelaars de prijsprestaties voor een groep databases binnen een voorgeschreven budget optimaliseren en flexibele prestaties voor elke database leveren.

## <a name="what-are-sql-elastic-pools"></a>Wat zijn de elastische pools SQL?

SaaS-ontwikkelaars ontwikkelen toepassingen boven op grootschalige gegevenslagen die uit meerdere databases bestaan. Een algemeen patroon is de inrichting van een individuele database voor elke klant. Maar verschillende klanten hebben vaak verschillende en onvoorspelbare gebruikspatronen, en de resourcevereisten van elke gebruiker van een individuele database zijn moeilijk te voorspellen. Traditioneel moest u twee opties:

- Te veel bronnen op basis van piekgebruik en via salaris, ter beschikking stellen of
- Onder inrichten op te slaan kosten ten koste van de prestaties en klanttevredenheid tijdens pieken.

Elastische pools oplossen dit probleem door ervoor te zorgen dat databases ophalen van de prestaties-bronnen die ze nodig hebben. Ze bieden een eenvoudig mechanisme voor het toewijzen van resources binnen een voorspelbaar budget. Zie [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) voor meer informatie over ontwerppatronen voor SaaS-toepassingen met elastische groepen.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Elastic-databases-helps-SaaS-developers-tame-explosive-growth/player]
>

Elastische pools inschakelen kopen van resources voor een groep die wordt gedeeld door meerdere databases voor onvoorspelbare perioden van geheugengebruik door afzonderlijke databases van de ontwikkelaar. U kunt resources configureren voor de groep op op basis van de [aankoopmodel op basis van het DTU](sql-database-service-tiers-dtu.md) of de [vCore gebaseerde aankoopmodel (preview)](sql-database-service-tiers-vcore.md). De Resourcevereiste voor een pool wordt bepaald door het totale gebruik van de databases. De hoeveelheid resources beschikbaar voor de pool wordt bepaald door de ontwikkelaar budget. De ontwikkelaar gewoon databases toegevoegd aan de groep, stelt u de minimale en maximale resources voor de databases (minimum en maximum aantal dtu's of minimum of maximum vCores, afhankelijk van uw keuze op voor het model resourcing), en stelt u de resources van de groep op basis van hun budget. Met groepen kan een ontwikkelaar services naadloos met een alsmaar groeiende schaal uitbreiden van een kleine startende ondernemer tot een volwassen bedrijf.

Binnen de pool hebben afzonderlijke databases de flexibiliteit om de schaal automatisch aan te passen binnen ingestelde parameters. Een database kan meer bronnen om te voldoen aan de vraag gebruiken onder een zware belasting. Databases onder lichte lading verbruiken kleiner en databases onder geen load geen resources gebruiken. De inrichting van resources voor de hele pool in plaats van afzonderlijke databases vereenvoudigt uw beheertaken. Bovendien hebt u een voorspelbare budget voor de groep. Aanvullende resources kunnen worden toegevoegd aan een bestaande toepassingen zonder uitvaltijd database, behalve dat de databases moet mogelijk worden verplaatst naar de aanvullende rekenresources voor de nieuwe eDTU-reservering bieden. Op dezelfde manier als extra resources niet meer nodig zijn kunnen ze worden verwijderd uit een bestaande pool op elk gewenst moment in de tijd. Daarnaast kunt u databases aan de groep toevoegen of uit de groep verwijderen. Als een database naar verwachting minder resources nodig heeft, kunt u deze verwijderen.

## <a name="when-should-you-consider-a-sql-database-elastic-pool"></a>Wanneer moet u rekening houden met een elastische pool in SQL-Database?

Pools zijn geschikt voor een groot aantal databases met specifieke gebruikspatronen. Voor een bepaalde database wordt dit patroon gekenmerkt door een laag gemiddeld gebruik met relatief incidentele gebruikspieken.

Hoe meer databases u aan een groep kunt toevoegen, des te groter worden de besparingen. Afhankelijk van uw gebruikspatroon van toepassingen is het mogelijk om besparingen te zien met slechts twee S3-databases.

In de volgende secties ziet u hoe u kunt bepalen of het voor uw specifieke verzameling databases voordelig is om deel uit te maken van een groep. In de voorbeelden wordt gebruik gemaakt van Standard-groepen, maar dezelfde principes gelden ook voor Basic- en Premium-groepen.

### <a name="assessing-database-utilization-patterns"></a>Databasegebruikspatronen beoordelen

De volgende afbeelding toont een voorbeeld van een database die doorgaans weinig wordt gebruikt, maar bij soms ook extreem actief is. Dit is een gebruikspatroon dat geschikt is voor een groep:

   ![een individuele database die geschikt is voor een groep](./media/sql-database-elastic-pool/one-database.png)

Gedurende de geïllustreerde periode van vijf minuten piekt DB1 tot 90 DTU's, maar het gemiddelde gebruik is nog geen vijf DTU's. Er is een S3-prestatieniveau vereist om deze werkbelasting in een individuele database uit te voeren, maar in dat geval worden de meeste resources tijdens perioden van weinig activiteit niet benut.

Met een groep kunnen deze ongebruikte DTU’s worden gedeeld met meerdere databases, wat de benodigde DTU's en de totale kosten vermindert.

Stel dat er in het vorige voorbeeld meer databases zijn die een soortgelijk gebruikspatroon hebben als DB1. In de volgende twee onderstaande afbeeldingen, worden het gebruik van vier databases en 20 databases gelaagd naar dezelfde grafiek ter illustratie van de niet-overlappende aard van het-gebruik gedurende een periode met behulp van het DTU-aankoopmodel:

   ![vier databases met een gebruikspatroon dat geschikt is voor een groep](./media/sql-database-elastic-pool/four-databases.png)

   ![twintig databases met een gebruikspatroon dat geschikt is voor een groep](./media/sql-database-elastic-pool/twenty-databases.png)

Het gezamenlijke DTU-gebruik van alle 20 databases wordt aangegeven door de zwarte lijn in voorgaande afbeelding. U ziet dat het totale DTU-gebruik nooit hoger is dan honderd DTU’s en dat de twintig databases gedurende deze periode honderd eDTU's kunnen delen. Dit resulteert in een twintigvoudige vermindering in DTU's en een dertienvoudige prijsvermindering ten opzichte van wanneer elke database in S3-prestaties voor individuele databases wordt geplaatst.

Dit voorbeeld is om de volgende redenen ideaal:

* Er zijn grote verschillen tussen piekgebruik en gemiddeld gebruik per database.
* Het piekgebruik voor elke database vindt plaats op verschillende momenten.
* eDTU‘s worden gedeeld tussen meerdere databases.

De prijs van een groep is een functie van de groep eDTU's. Hoewel de prijs per eDTU voor een groep 1,5 x groter is dan de prijs per DTU voor een individuele database, **kunnen eDTU's in een groep door veel databases worden gedeeld en zijn er dus minder eDTU's nodig**. Deze verschillen in prijsbepaling en het delen van eDTU's vormen de basis van de mogelijke prijsbesparing die groepen kunnen bieden.

De volgende vuistregels voor databaseaantallen en databasegebruik helpen ervoor te zorgen dat een groep lagere kosten met zich meebrengt, vergeleken met het gebruik van prestatieniveaus voor individuele databases.

### <a name="minimum-number-of-databases"></a>Minimum aantal databases

Als de totale hoeveelheid resources voor individuele databases meer dan 1,5 x de benodigde resources voor de groep, is wordt een elastische pool rendabeler is.

***Voorbeeld aankopen model op basis van DTU***<br>
Er zijn ten minste twee S3-databases of ten minste 15 S0-databases nodig wil een groep van 100 eDTU's rendabeler zijn dan het gebruik van de prestatieniveaus voor individuele databases.

### <a name="maximum-number-of-concurrently-peaking-databases"></a>Maximum aantal gelijktijdig piekende databases

Door het delen van resources, kunnen niet alle databases in een groep resources tot maximaal de maximaal beschikbare tegelijkertijd gebruik voor individuele databases. De minder databases dat gelijktijdig piekuren, des te lager de resources van toepassingen kunnen worden ingesteld en wordt het meest efficiënt is de groep. In het algemeen moeten niet meer dan 2/3 (of 67%) van de databases in de groep tegelijkertijd pieksnelheden aan hun limiet resources.

***Voorbeeld aankopen model op basis van DTU***<br>
Om de kosten voor drie S3-databases in een groep van 200 eDTU's te verlagen, kunnen maximaal twee van deze databases tegelijkertijd pieken in hun gebruik. Of, als meer dan twee van deze vier S3-databases gelijktijdig pieken, zou de groep moeten worden uitgebreid tot meer dan 200 eDTU's. Als de groep wordt uitgebreid tot meer dan 200 eDTU's, moeten er meer S3-databases aan de groep worden toegevoegd om de kosten lager te houden dan bij prestatieniveaus voor individuele databases.

In dit voorbeeld wordt geen rekening gehouden met het gebruik van andere databases in de groep. Als alle databases voortdurend in enige mate gebruik maken van eDTU's, kan minder dan 2/3 (of 67%) van de databases tegelijkertijd pieken.

### <a name="resource-utilization-per-database"></a>Resourcegebruik per database
Een groot verschil tussen het piek- en gemiddelde gebruik van een database geeft langere perioden van laag gebruik en korte perioden hoog gebruik aan. Dit gebruikspatroon is ideaal voor het delen van resources met meerdere databases. Een database zou een geschikte kandidaat voor een groep kunnen zijn als het piekgebruik ongeveer 1,5 keer groter is dan het gemiddelde gebruik.

***Voorbeeld aankopen model op basis van DTU***<br>
Een S3-database die piekt tot 100 DTU's en gemiddeld 67 DTU's of minder gebruikt, is een goede kandidaat voor het delen van eDTU's in een groep. Ook een S1-database die piekt tot 20 DTU's en gemiddeld 13 DTU's of minder gebruikt, is een goede kandidaat voor een groep.

## <a name="how-do-i-choose-the-correct-pool-size"></a>Hoe kies ik de juiste poolgrootte?

De aanbevolen grootte voor een groep is afhankelijk van de totale benodigde resources voor alle databases in de groep. Dit omvat het volgende bepalen:

* Maximum aantal bronnen worden gebruikt door alle databases in de groep (maximum aantal dtu's of maximale vCores, afhankelijk van uw keuze op voor het model resourcing).
* De maximum opslag in bytes die door alle databases in de groep wordt gebruikt.

Zie voor beschikbare Servicelagen voor elk resourcemodel, de [aankoopmodel op basis van het DTU](sql-database-service-tiers-dtu.md) of de [vCore gebaseerde aankoopmodel (preview)](sql-database-service-tiers-vcore.md).

SQL Database evalueert automatisch het historisch resourcegebruik van databases in een bestaande SQL Database-server en op basis daarvan wordt de juiste poolconfiguratie in de Azure-portal aanbevolen. Naast de aanbevelingen wordt met een geïntegreerde functie het eDTU-gebruik voor een aangepaste groep databases op de server geschat. Hiermee kunt u een 'wat als'-analyse uitvoeren door interactief databases aan de groep toe te voegen of uit de groep te verwijderen, om zo een analyse van het resourcegebruik en formaatadvies te krijgen voordat u uw wijzigingen doorvoert. Zie [Monitor, manage, and size an elastic pool](#monitor-an-elastic-pool-and-its-databases) (Een elastische groep bewaken, beheren en van formaat wijzigen) voor instructies.

Als u het hulpprogramma niet kunt gebruiken, kunnen de volgende stappen u helpen om te schatten of een groep rendabeler is dan individuele databases:

1. Schat de edtu's of vCores die nodig zijn voor de groep als volgt:

   Voor op basis van DTU aankoopmodel: MAX (<*totaal aantal databases* X *gemiddelde DTU-gebruik per DB*>,<br>
   <*aantal gelijktijdig piekende databases* X *piek-DTU-gebruik per DB*)

   Voor op basis van vCore aankoopmodel (preview): MAX (<*totaal aantal databases* X *vCore gebruik per DB gemiddelde*>,<br>
   <*Aantal gelijktijdig piek databases* X *piek vCore gebruik per DB*)

2. Schat hoeveel opslagruimte de groep nodig heeft door het aantal bytes op te tellen dat nodig is voor alle databases in de groep. Bepaal daarna hoe groot de eDTU-groep moet zijn om aan deze hoeveelheid opslag te voldoen.
3. Voor de DTU gebaseerde aankoopmodel, worden de grootste van de geschatte eDTU uit stap 1 en stap 2. Voor de vCore gebaseerde aankoopmodel (preview), nemen de schatting van het vCore uit stap 1.
4. Zie de [pagina met prijzen SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/) en zoeken naar de kleinste groep grootte die groter is dan de schatting van stap 3.
5. Vergelijk de prijs van de groep uit stap 5 met de prijs voor het gebruik van de juiste prestatieniveaus voor individuele databases.

## <a name="using-other-sql-database-features-with-elastic-pools"></a>Gebruik van andere functies van de SQL-Database met elastische pools

### <a name="elastic-jobs-and-elastic-pools"></a>Elastische taken en elastische pools

Met een groep worden beheertaken vereenvoudigd door scripts in **[elastische taken](sql-database-elastic-jobs-overview.md)** uit te voeren. Een elastische taak elimineert de meeste saaie handelingen die komen kijken bij grote aantallen databases. Zie [Getting started with Elastic Database jobs](sql-database-elastic-jobs-getting-started.md) (Aan de slag met taken voor elastische databases) om aan de slag te gaan.

Zie [Scaling out with Azure SQL Database](sql-database-elastic-scale-introduction.md) (Uitbreiden met Azure SQL Database) voor meer informatie over andere databasehulpprogramma's voor het werken met meerdere databases.

### <a name="business-continuity-options-for-databases-in-an-elastic-pool"></a>Opties voor bedrijfscontinuïteit voor databases in een elastische pool
Gepoolde databases ondersteunen in het algemeen dezelfde [bedrijfscontinuïteitsfuncties](sql-database-business-continuity.md) die beschikbaar zijn voor individuele databases.

- **Punt in tijd terugzetten**: punt in tijd terugzetten automatische databaseback-ups gebruikt voor het herstellen van een database in een groep naar een bepaald punt in tijd. Zie [Herstel naar een bepaald tijdstip](sql-database-recovery-using-backups.md#point-in-time-restore)

- **Geo-restore**: Geo-herstel biedt de standaardoptie voor herstel wanneer een database is niet beschikbaar vanwege een incident in de regio waar de database wordt gehost. Zie [Restore an Azure SQL Database or failover to a secondary](sql-database-disaster-recovery.md) (Een Azure SQL Database of herstellen of een failover uitvoeren naar een secundaire server)

- **Actieve geo-replicatie**: configureren voor toepassingen met agressievere herstelvereisten dan geo-herstel te kan bieden, [actieve geo-replicatie](sql-database-geo-replication-overview.md).

## <a name="manage-elastic-pools-and-databases-using-the-azure-portal"></a>Elastische pools en databases die gebruikmaken van de Azure-portal beheren

### <a name="creating-a-new-sql-database-elastic-pool-using-the-azure-portal"></a>Maken van een nieuwe SQL-Database elastische pool met de Azure portal

Er zijn twee manieren waarop u een elastische pool in de Azure portal kunt maken.
1. U kunt een elastische pool maken door te zoeken naar **elastische SQL-groep** in de **Marketplace** of klikken op **+ toevoegen** Blader op de SQL-elastische pools blade. U bent een nieuwe of bestaande server via deze werkstroom van toepassingen opgeven.
2. Of u kunt een elastische pool maken door te navigeren naar een bestaande SQL server en op **groep maken** voor het maken van een groep rechtstreeks in die server. Hier het enige verschil is dat u de stap overslaan waar u de server tijdens de werkstroom van toepassingen opgeven.

> [!NOTE]
> U kunt meerdere groepen maken op een server, maar u kunt geen databases van verschillende servers toevoegen aan dezelfde pool.

De servicelaag van de pool bepaalt welke functies beschikbaar zijn voor de elastics in de groep en de maximale hoeveelheid resources beschikbaar voor elke database. Zie voor meer informatie, limieten voor elastische pools in de [DTU model](sql-database-dtu-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels) en de [vCore model](sql-database-vcore-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels).

Voor het configureren van de resources en prijzen van de groep, klikt u op **pool configureren**. Selecteer vervolgens een servicelaag, databases toevoegen aan de groep en configureren van de resource-limieten voor de groep en de databases.

Wanneer u klaar bent met het configureren van de groep, kunt u Klik op toepassen, de naam van de groep, en klik op 'OK' om te maken van de groep.

### <a name="monitor-an-elastic-pool-and-its-databases"></a>Een elastische groep en de databases bewaken

U kunt het gebruik van een elastische groep en de databases binnen die groep bewaken in de Azure portal. U kunt ook een aantal wijzigingen aanbrengen in uw elastische groep en alle wijzigingen tegelijk verzenden. Deze wijzigingen omvatten toevoegen of verwijderen van databases, elastische pool-instellingen wijzigen of database-instellingen wijzigen.

Als u wilt bewaken van de elastische pool, zoeken en openen van een elastische pool in de portal. U ziet eerst een scherm weergegeven dat u een van de status van de elastische groep overzicht. Dit omvat:

* Bewaking van grafieken met gebruik van bronnen van de elastische groep
* Recente waarschuwingen en aanbevelingen, indien beschikbaar, voor de elastische groep

De volgende afbeelding toont een voorbeeld van de elastische groep:

![Toepassingen weergeven](./media/sql-database-elastic-pool-manage-portal/basic.png)

Als u meer informatie over de toepassingen op een van de beschikbare informatie in dit overzicht kunt u wilt. Te klikken op de **Resourcegebruik** grafiek gaat u naar de weergave Azure Monitoring waar u de metrische gegevens en het tijdstip venster weergegeven in het diagram kunt aanpassen. Op alle beschikbare meldingen te klikken, gaat u naar een blade die de volledige details van deze waarschuwing of de aanbeveling bevat.

Als u bewaken van de databases binnen uw pool wilt, kunt u klikt u op **Resourcegebruik van de Database** in de **bewaking** gedeelte van de resource-menu aan de linkerkant.

![Database-gebruik bronpagina](./media/sql-database-elastic-pool-manage-portal/db-utilization.png)

#### <a name="to-customize-the-chart-display"></a>De grafiekweergave aanpassen

U kunt de grafiek en weer te geven van andere metrische gegevens zoals CPU-percentage, gegevens-IO-percentage en logboek IO percentage gebruikt van de metrische pagina bewerken.

Op de **grafiek bewerken** formulier, kunt u een vaste tijd liggen, of klik op **aangepaste** een 24-uurs venster in de afgelopen twee weken selecteren en selecteer vervolgens de bronnen om te controleren.

#### <a name="to-select-databases-to-monitor"></a>Databases bewaken selecteren

Standaard wordt de grafiek in de **Database Resourcegebruik** blade ziet de databases top 5 DTU of CPU (afhankelijk van uw servicelaag). U kunt overschakelen van de databases in deze grafiek door te selecteren en de selectie opheffen van de databases uit de lijst onder de grafiek via de selectievakjes in aan de linkerkant.

U kunt ook meer metrische gegevens naar de weergave naast elkaar in deze databasetabel een volledig overzicht van de prestaties van uw databases ophalen selecteren.

Zie voor meer informatie [waarschuwingen van de SQL-Database maken in Azure portal](sql-database-insights-alerts-portal.md).

### <a name="manage-an-elastic-pool-and-its-databases"></a>Een elastische groep en de databases beheren

Instellingen voor alle toepassingen vindt u op één plek: de **pool configureren** blade. Als u hier, een elastische groep niet vinden in de portal en klik op **pool configureren** vanaf de bovenkant van de blade of vanuit het menu resource aan de linkerkant.

Hier kunt u een combinatie van de volgende wijzigingen en sla ze allemaal in één batch:
1. De servicelaag van de groep wijzigen
2. Omhoog of omlaag schalen van de prestaties (DTU of vCores) en opslag
3. Toevoegen of verwijderen van de databases uit de groep
4. Stel een min (gegarandeerd) en de maximale prestatielimiet voor voor de databases van de groepen
5. Overzicht van de kosten om eventuele wijzigingen in uw factuur als gevolg van de nieuwe instellingen weer te geven

![Elastische groep configuratie-blade](./media/sql-database-elastic-pool-manage-portal/configure-pool.png)

## <a name="manage-elastic-pools-and-databases-using-powershell"></a>Elastische pools en databases met behulp van PowerShell beheren

Als u wilt maken en beheren van elastische pools van de SQL-Database met Azure PowerShell, gebruik de volgende PowerShell-cmdlets. Als u wilt installeren of upgraden van PowerShell, Zie [Installeer Azure PowerShell-module](/powershell/azure/install-azurerm-ps). Als u wilt maken en beheren van databases, servers en firewall-regels, Zie [maken en beheren van Azure SQL Database-servers en -databases met behulp van PowerShell](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-powershell).

> [!TIP]
> Zie voor PowerShell-voorbeeldscripts, [maken van elastische pools en databases verplaatsen tussen groepen en van een groep met behulp van PowerShell](scripts/sql-database-move-database-between-pools-powershell.md) en [Gebruik PowerShell om te bewaken en schalen van een elastische SQL-groep in Azure SQL Database](scripts/sql-database-monitor-and-scale-pool-powershell.md).
>

| Cmdlet | Beschrijving |
| --- | --- |
|[New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool)|Een pool voor elastische database maakt op een logische SQL-server.|
|[Get-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/get-azurermsqlelasticpool)|Elastische pools en hun eigenschapswaarden op een logische SQL-server opgehaald.|
|[Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool)|Hiermee wijzigt u de eigenschappen van een pool voor elastische database op een logische SQL-server. Gebruik bijvoorbeeld de **StorageMB** eigenschap te wijzigen van de maximale opslag van een elastische pool.|
|[Remove-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/remove-azurermsqlelasticpool)|Hiermee verwijdert u een pool voor elastische database op een logische SQL-server.|
|[Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity)|Hiermee wordt de status van bewerkingen in een elastische pool op een logische SQL-server opgehaald.|
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Maakt een nieuwe database in een bestaande pool of als een individuele database. |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Hiermee haalt u een of meer databases op.|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Stelt eigenschappen van een database of een bestaande database verplaatst naar buiten of tussen elastische pools.|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Hiermee verwijdert u een database.|


> [!TIP]
> Maken van veel databases in een elastische pool kan duren wanneer het wordt gedaan via de portal of PowerShell-cmdlets die slechts één database tegelijkertijd maken. Als u wilt maken voor een elastische pool automatiseren, Zie [CreateOrUpdateElasticPoolAndPopulate](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).
>

## <a name="manage-elastic-pools-and-databases-using-the-azure-cli"></a>Elastische pools en met de Azure CLI-databases beheren

Maken en beheren van elastische pools van de SQL-Database met de [Azure CLI](/cli/azure), gebruikt u de volgende [Azure CLI SQL Database](/cli/azure/sql/db) opdrachten. Gebruik de [Cloud Shell](/azure/cloud-shell/overview) om de CLI in uw browser uit te voeren of [installeer](/cli/azure/install-azure-cli) de CLI op macOS, Linux of Windows.

> [!TIP]
> Zie voor Azure CLI-voorbeeldscripts, [gebruik CLI verplaatsen van een Azure SQL database in een elastische SQL-groep](scripts/sql-database-move-database-between-pools-cli.md) en [gebruik Azure CLI voor het schalen van een elastische SQL-groep in Azure SQL Database](scripts/sql-database-scale-pool-cli.md).
>

| Cmdlet | Beschrijving |
| --- | --- |
|[elastische sql-AZ-groep maken](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_create)|Maakt een elastische pool.|
|[lijst met AZ sql elastische pool](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list)|Retourneert een lijst met elastische pools in een server.|
|[AZ sql elastische pool lijst-databases](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list_dbs)|Retourneert een lijst met databases in een elastische pool.|
|[az sql elastic-pool list-editions](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list_editions)|Omvat ook beschikbaar DTU groepsinstellingen, opslaglimieten, en per database-instellingen. Om te reduceren uitgebreidheid aanvullende opslaglimieten en per database instellingen standaard zijn verborgen.|
|[update van de elastische pool AZ sql](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update)|Een elastische pool-updates.|
|[AZ sql elastische groep verwijderen](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_delete)|Hiermee wordt de elastische groep verwijderd.|

## <a name="manage-databases-within-elastic-pools-using-transact-sql"></a>Databases binnen elastische pools met Transact-SQL beheren

Maken en het verplaatsen van databases binnen bestaande elastische pools of om informatie over de elastische groep van een SQL-Database met Transact-SQL te retourneren, gebruikt u de volgende T-SQL-opdrachten. U kunt deze opdrachten met de Azure portal, de opdracht [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), of een ander programma dat kan verbinding maken met een Azure SQL Database-server en doorgeven Transact-SQL-opdrachten. Als u wilt maken en beheren van databases, servers en firewall-regels, Zie [maken en beheren van Azure SQL Database-servers en met Transact-SQL-databases](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-transact-sql).

> [!IMPORTANT]
> U niet kunt maken, bijwerken of verwijderen van een Azure SQL Database elastische pool met Transact-SQL. U kunt toevoegen of verwijderen van de databases uit een elastische pool en u kunt DMV's gebruiken om informatie over bestaande elastische pools te retourneren.
>

| Opdracht | Beschrijving |
| --- | --- |
|[DATABASE (Azure SQL Database) maken](/sql/t-sql/statements/create-database-azure-sql-database)|Maakt een nieuwe database in een bestaande pool of als een individuele database. U moet verbonden zijn met de database master om een nieuwe database te maken.|
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |Een database verplaatsen naar, uit of tussen elastische pools.|
|[DROP DATABASE (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Hiermee verwijdert u een database.|
|[sys.elastic_pool_resource_stats (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)|Retourneert de gebruiksstatistieken resource voor alle pools voor elastische databases in een logische server. Voor elke elastische databasegroep moet er één rij voor elke 15 seconden reporting venster (vier rijen per minuut). Dit omvat CPU, IO, logboek, opslagverbruik en gelijktijdige aanvraag/sessie gebruik door alle databases in de groep.|
|[sys.database_service_objectives (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Retourneert de edition (servicelaag), de servicedoelstelling (prijscategorie) en de naam van de elastische groep, indien aanwezig, voor een Azure SQL database of een Azure SQL Data Warehouse. Als u aangemeld bent op de database master in een Azure SQL Database-server, retourneert de informatie voor alle databases. Voor Azure SQL Data Warehouse, moet u verbonden zijn met de database master.|

## <a name="manage-elastic-pools-and-databases-using-the-rest-api"></a>Elastische pools en databases die gebruikmaken van de REST-API beheren

Groepen gebruiken voor het maken en beheren van de elastische SQL-Database deze REST-API-aanvragen.

| Opdracht | Beschrijving |
| --- | --- |
|[Elastische Pools - maken of bijwerken](/rest/api/sql/elasticpools/createorupdate)|Een nieuwe elastische pool maken of bijwerken van een bestaande elastische pool.|
|[Elastische Pools - verwijderen](/rest/api/sql/elasticpools/delete)|Hiermee wordt de elastische groep verwijderd.|
|[Elastische Pools - ophalen](/rest/api/sql/elasticpools/get)|Hiermee haalt u een elastische pool.|
|[Elastische Pools - lijst per Server](/rest/api/sql/elasticpools/listbyserver)|Retourneert een lijst met elastische pools in een server.|
|[Elastische Pools - Update](/rest/api/sql/elasticpools/update)|Een bestaande elastische pool-updates.|
|[Aanbevolen elastische Pools - ophalen](/rest/api/sql/recommendedelasticpools/get)|Hiermee haalt u een aanbevolen elastische pool.|
|[Aanbevolen elastische Pools - lijst per Server](/rest/api/sql/recommendedelasticpools/listbyserver)|Retourneert aanbevolen elastische pools.|
|[Aanbevolen elastische Pools - lijst metrische gegevens](/rest/api/sql/recommendedelasticpools/listmetrics)|Retourneert aanbevolen elastische pool metrische gegevens.|
|[Activiteiten van de elastische groep](/rest/api/sql/elasticpoolactivities)|Retourneert de elastische groep activiteiten.|
|[Elastische Pool databaseactiviteiten](/rest/api/sql/elasticpooldatabaseactivities)|Retourneert de activiteit op databases binnen een elastische pool.|
|[Databases - maken of bijwerken](/rest/api/sql/databases/createorupdate)|Een nieuwe database maken of bijwerken van een bestaande database.|
|[Databases - Get](/rest/api/sql/databases/get)|Hiermee haalt u een database.|
|[Databases - verkrijgen door de elastische groep](/rest/api/sql/databases/getbyelasticpool)|Hiermee haalt u een database in een elastische pool.|
|[Databases - verkrijgen door aanbevolen elastische Pool](/rest/api/sql/databases/getbyrecommendedelasticpool)|Hiermee haalt u een database binnen een aanbevolen elastische pool.|
|[Databases - lijst door de elastische groep](/rest/api/sql/databases/listbyelasticpool)|Retourneert een lijst met databases in een elastische pool.|
|[Databases - lijst met aanbevolen elastische Pool](/rest/api/sql/databases/listbyrecommendedelasticpool)|Retourneert een lijst met databases binnen een aanbevolen elastische pool.|
|[Databases - lijst per Server](/rest/api/sql/databases/listbyserver)|Retourneert een lijst met databases in een server.|
|[Databases - Update](/rest/api/sql/databases/update)|Een bestaande database-updates.|

## <a name="next-steps"></a>Volgende stappen

* Zie voor een video [video loop van de Microsoft Virtual Academy op elastische mogelijkheden van Azure SQL Database](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
* Zie [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) voor meer informatie over ontwerppatronen voor SaaS-toepassingen met elastische groepen.
* Zie voor een SaaS-zelfstudie met elastische pools, [Inleiding tot de Wingtip SaaS-toepassing](sql-database-wtp-overview.md).
