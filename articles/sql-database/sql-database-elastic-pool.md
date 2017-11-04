---
title: Beheer van meerdere SQL-databases met elastische pools-Azure | Microsoft Docs
description: "Beheren en schalen van meerdere SQL-databases - honderden en duizendtallen - met elastische pools. Één prijs voor resources die u distribueren kunt waar nodig."
keywords: meerdere databases, databaseresources en prestaties van de database
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: b46e7fdc-2238-4b3b-a944-8ab36c5bdb8e
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.date: 10/11/2017
ms.author: carlrab
ms.workload: Active
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 2f1ff7a7c2ecf04069ffa6afcc66e2f0f9915b35
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="elastic-pools-help-you-manage-and-scale-multiple-azure-sql-databases"></a>Elastische pools helpen u te beheren en schalen van meerdere Azure SQL-databases

SQL-Database elastische pools zijn een eenvoudige, rendabele oplossing voor het beheren en schalen van meerdere databases die voldoen aan verschillende en onvoorspelbare veeleisende hebben. De databases in een elastische pool op één Azure SQL Database-server en een aantal bronnen delen ([elastische Database Transaction Units](sql-database-what-is-a-dtu.md) (Edtu)) op een vaste prijs. Met elastische groepen in Azure SQL Database kunnen SaaS-ontwikkelaars de prijsprestaties voor een groep databases binnen een voorgeschreven budget optimaliseren en flexibele prestaties voor elke database leveren. 

> [!NOTE]
> Elastische pools zijn algemeen beschikbaar in alle Azure-regio's, behalve in West-India, waar deze zich momenteel in de previewfase bevinden. Algemene beschikbaarheid van elastische pools in deze regio zal zo snel mogelijk plaatsvinden.
>

## <a name="what-are-sql-elastic-pools"></a>Wat zijn de elastische pools SQL? 

SaaS-ontwikkelaars ontwikkelen toepassingen boven op grootschalige gegevenslagen die uit meerdere databases bestaan. Een algemeen patroon is de inrichting van een individuele database voor elke klant. Maar verschillende klanten hebben vaak verschillende en onvoorspelbare gebruikspatronen, en de resourcevereisten van elke gebruiker van een individuele database zijn moeilijk te voorspellen. Traditioneel moest u twee opties: 

- Te veel bronnen op basis van piekgebruik en via salaris, ter beschikking stellen of
- Onder inrichten op te slaan kosten ten koste van de prestaties en klanttevredenheid tijdens pieken. 

Elastische pools oplossen dit probleem door ervoor te zorgen dat databases ophalen van de prestaties-bronnen die ze nodig hebben. Ze bieden een eenvoudig mechanisme voor het toewijzen van resources binnen een voorspelbaar budget. Zie [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) voor meer informatie over ontwerppatronen voor SaaS-toepassingen met elastische groepen.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Elastic-databases-helps-SaaS-developers-tame-explosive-growth/player]
>

Elastische pools inschakelen de ontwikkelaar om aan te schaffen [elastische Database Transaction Units](sql-database-what-is-a-dtu.md) (edtu's) voor een groep die wordt gedeeld door meerdere databases voor onvoorspelbare perioden van geheugengebruik door afzonderlijke databases. De eDTU-vereiste voor een groep wordt bepaald door het gezamenlijke gebruik van de databases. Het aantal eDTU's dat beschikbaar is voor de groep, wordt bepaald door het budget van de ontwikkelaar. De ontwikkelaar voegt gewoon databases aan de groep toe, stelt het minimum en maximum aantal eDTU's voor de databases in en stelt vervolgens de eDTU van de groep in op basis van het budget. Met groepen kan een ontwikkelaar services naadloos met een alsmaar groeiende schaal uitbreiden van een kleine startende ondernemer tot een volwassen bedrijf.

Binnen de pool hebben afzonderlijke databases de flexibiliteit om de schaal automatisch aan te passen binnen ingestelde parameters. Bij zware belasting kan een database meer eDTU's verbruiken om aan de vraag te voldoen. Databases verbruiken minder bij lichte belasting en verbruiken geen eDTU's als er geen belasting is. De inrichting van resources voor de hele pool in plaats van afzonderlijke databases vereenvoudigt uw beheertaken. Bovendien hebt u een voorspelbaar budget voor de pool. Er kunnen meer eDTU's zonder uitvaltijd van de database aan een bestaande pool worden toegevoegd. Maar de databases moeten dan mogelijk worden verplaatst om te voorzien in de extra rekenresources voor de nieuwe eDTU-reservering. En als de extra eDTU's niet meer nodig zijn, kunnen ze op elk moment uit een bestaande pool worden verwijderd. Daarnaast kunt u databases aan de groep toevoegen of uit de groep verwijderen. Als een database naar verwachting minder resources nodig heeft, kunt u deze verwijderen.

## <a name="when-should-you-consider-a-sql-database-elastic-pool"></a>Wanneer moet u rekening houden met een elastische pool in SQL-Database?

Pools zijn geschikt voor een groot aantal databases met specifieke gebruikspatronen. Voor een bepaalde database wordt dit patroon gekenmerkt door een laag gemiddeld gebruik met relatief incidentele gebruikspieken.

Hoe meer databases u aan een groep kunt toevoegen, des te groter worden de besparingen. Afhankelijk van uw gebruikspatroon van toepassingen is het mogelijk om besparingen te zien met slechts twee S3-databases. 

In de volgende secties ziet u hoe u kunt bepalen of het voor uw specifieke verzameling databases voordelig is om deel uit te maken van een groep. In de voorbeelden wordt gebruik gemaakt van Standard-groepen, maar dezelfde principes gelden ook voor Basic- en Premium-groepen.

### <a name="assessing-database-utilization-patterns"></a>Databasegebruikspatronen beoordelen

De volgende afbeelding toont een voorbeeld van een database die doorgaans weinig wordt gebruikt, maar bij soms ook extreem actief is. Dit is een gebruikspatroon dat geschikt is voor een groep:

   ![een individuele database die geschikt is voor een groep](./media/sql-database-elastic-pool/one-database.png)

Gedurende de geïllustreerde periode van vijf minuten piekt DB1 tot 90 DTU's, maar het gemiddelde gebruik is nog geen vijf DTU's. Er is een S3-prestatieniveau vereist om deze werkbelasting in een individuele database uit te voeren, maar in dat geval worden de meeste resources tijdens perioden van weinig activiteit niet benut.

Met een groep kunnen deze ongebruikte DTU’s worden gedeeld met meerdere databases, wat de benodigde DTU's en de totale kosten vermindert.

Stel dat er in het vorige voorbeeld meer databases zijn die een soortgelijk gebruikspatroon hebben als DB1. In de volgende twee afbeeldingen is het gebruik van vier databases en twintig databases gelaagd over dezelfde grafiek om de niet-overlappende aard van het gebruik na verloop van tijd te illustreren:

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

Als de som van de DTU's van prestatieniveaus voor individuele databases meer dan 1,5 keer het aantal eDTU's is dat nodig is voor de groep, is een elastische groep rendabeler. Zie [eDTU- en opslaglimieten voor elastische pools en elastische databases](sql-database-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels) voor beschikbare grootten.

***Voorbeeld***<br>
Er zijn ten minste twee S3-databases of ten minste 15 S0-databases nodig wil een groep van 100 eDTU's rendabeler zijn dan het gebruik van de prestatieniveaus voor individuele databases.

### <a name="maximum-number-of-concurrently-peaking-databases"></a>Maximum aantal gelijktijdig piekende databases

Wanneer eDTU's worden gedeeld, kunnen niet alle databases in een groep de eDTU's gebruiken tot de limiet die beschikbaar is wanneer prestatieniveaus voor individuele databases worden gebruikt. Hoe minder databases gelijktijdig pieken, des te lager de groeps-eDTU kan worden ingesteld en des te rendabeler de groep wordt. In het algemeen zou niet meer dan 2/3 (of 67%) van de databases in de groep tegelijkertijd tot de maximale eDTU mogen pieken.

***Voorbeeld***<br>
Om de kosten voor drie S3-databases in een groep van 200 eDTU's te verlagen, kunnen maximaal twee van deze databases tegelijkertijd pieken in hun gebruik. Of, als meer dan twee van deze vier S3-databases gelijktijdig pieken, zou de groep moeten worden uitgebreid tot meer dan 200 eDTU's. Als de groep wordt uitgebreid tot meer dan 200 eDTU's, moeten er meer S3-databases aan de groep worden toegevoegd om de kosten lager te houden dan bij prestatieniveaus voor individuele databases.

In dit voorbeeld wordt geen rekening gehouden met het gebruik van andere databases in de groep. Als alle databases voortdurend in enige mate gebruik maken van eDTU's, kan minder dan 2/3 (of 67%) van de databases tegelijkertijd pieken.

### <a name="dtu-utilization-per-database"></a>DTU-gebruik per database
Een groot verschil tussen het piek- en gemiddelde gebruik van een database geeft langere perioden van laag gebruik en korte perioden hoog gebruik aan. Dit gebruikspatroon is ideaal voor het delen van resources met meerdere databases. Een database zou een geschikte kandidaat voor een groep kunnen zijn als het piekgebruik ongeveer 1,5 keer groter is dan het gemiddelde gebruik.

***Voorbeeld***<br>
Een S3-database die piekt tot 100 DTU's en gemiddeld 67 DTU's of minder gebruikt, is een goede kandidaat voor het delen van eDTU's in een groep. Ook een S1-database die piekt tot 20 DTU's en gemiddeld 13 DTU's of minder gebruikt, is een goede kandidaat voor een groep.

## <a name="how-do-i-choose-the-correct-pool-size"></a>Hoe kies ik de juiste poolgrootte?

De optimale grootte voor een groep hangt af van de totale eDTU's en opslagresources die nodig zijn voor alle databases in de groep. Hiervoor moet u de grootste waarde van de volgende factoren bepalen:

* Het maximum aantal DTU's dat door alle databases in de groep wordt gebruikt.
* De maximum opslag in bytes die door alle databases in de groep wordt gebruikt.

Zie [eDTU- en opslaglimieten voor elastische pools en elastische databases](sql-database-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels) voor beschikbare grootten.

SQL Database evalueert automatisch het historisch resourcegebruik van databases in een bestaande SQL Database-server en op basis daarvan wordt de juiste poolconfiguratie in de Azure-portal aanbevolen. Naast de aanbevelingen wordt met een geïntegreerde functie het eDTU-gebruik voor een aangepaste groep databases op de server geschat. Hiermee kunt u een 'wat als'-analyse uitvoeren door interactief databases aan de groep toe te voegen of uit de groep te verwijderen, om zo een analyse van het resourcegebruik en formaatadvies te krijgen voordat u uw wijzigingen doorvoert. Zie [Monitor, manage, and size an elastic pool](sql-database-elastic-pool-manage-portal.md) (Een elastische groep bewaken, beheren en van formaat wijzigen) voor instructies.

Als u het hulpprogramma niet kunt gebruiken, kunnen de volgende stappen u helpen om te schatten of een groep rendabeler is dan individuele databases:

1. Maak als volgt een schatting van het aantal eDTU's dat nodig is voor de groep:

   MAX(<*Totaal aantal DB's* X *gemiddelde DTU-gebruik per DB*>,<br>
   <*aantal gelijktijdig piekende databases* X *piek-DTU-gebruik per DB*)
2. Schat hoeveel opslagruimte de groep nodig heeft door het aantal bytes op te tellen dat nodig is voor alle databases in de groep. Bepaal daarna hoe groot de eDTU-groep moet zijn om aan deze hoeveelheid opslag te voldoen. Zie [eDTU- en opslaglimieten voor elastische pools en elastische databases](sql-database-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels) voor poolopslaglimieten op basis van de eDTU-poolomvang.
3. Gebruik de grootste eDTU-schatting uit stap 1 en stap 2.
4. Raadpleeg de [pagina Prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/) en zoek de kleinste eDTU-groepsomvang die groter is dan de schatting van stap 3.
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

Er zijn twee manieren waarop u een elastische pool in de Azure portal kunt maken. U kunt dit zelf doen, als u weet welke instellingen u voor uw groep wilt gebruiken, of u kunt gebruikmaken van wat de service u aanraadt. SQL-Database beschikt over ingebouwde intelligentie die raadt aan om een elastische groep is ingesteld als het kostenefficiënte op basis van de gebruikstelemetrie van uw databases. 

Een elastische pool te maken van een bestaande serverpagina in de portal is de eenvoudigste manier om te verplaatsen van bestaande databases in een elastische pool. U kunt ook een elastische pool maken door te zoeken naar **elastische SQL-groep** in de **Marketplace** of klikken op **+ toevoegen** op de pagina van de elastische pools SQL. U bent een nieuwe of bestaande server via deze werkstroom van toepassingen opgeven.

> [!NOTE]
> U kunt meerdere groepen maken op een server, maar u kunt geen databases van verschillende servers toevoegen aan dezelfde pool.
> 

De prijscategorie van de pool bepaalt welke functies beschikbaar zijn voor de elastics in de groep en het maximale aantal edtu's (eDTU MAX) en opslag (GB) beschikbaar zijn voor elke database. Zie voor meer informatie [Resource beperkt voor elastische pools](sql-database-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels).

Klik op **Prijscategorie** om de prijscategorie van de pool te wijzigen. Klik op de gewenste prijscategorie en vervolgens op **Selecteren**.

> [!IMPORTANT]
> Nadat u de prijscategorie gekozen heeft en uw wijzigingen heeft opgeslagen door in de laatste stap op **OK** te klikken, kunt u de prijscategorie van de groep niet meer wijzigen. De prijscategorie voor een bestaande elastische pool niet wijzigen, een elastische pool maken in de gewenste prijscategorie en migreert de databases naar deze nieuwe pool.
>

Als de gebruikstelemetrie van de databases waar u mee werkt genoeg is, worden de grafiek **Geschat eDTU- en GB-gebruik** en het staafdiagram **Werkelijk eDTU-gebruik** bijgewerkt zodat u betere beslissingen kunt nemen met betrekking tot de configuratie. De service kan ook een bericht weergeven met een aanbeveling zodat u de groep het juiste formaat kunt geven.

De SQL Database-service beoordeelt de gebruiksgeschiedenis en beveelt een of meerdere groepen aan wanneer deze kosteneffectiever zijn dan het gebruik van individuele databases. Elke aanbeveling wordt geconfigureerd met een unieke subset van de databases van de server die het meest geschikt zijn voor de groep.

![aanbevolen groep](./media/sql-database-elastic-pool-create-portal/recommended-pool.png) 

De aanbevelingen voor de groep bestaan uit:

- Een prijscategorie voor de pool (Basic, Standard, Premium of RS Premium)
- De juiste **eDTU’s voor de groep** (ook wel Max eDTU's per groep)
- De **eDTU MAX** en **eDTU Min** per database
- De lijst met aanbevolen databases voor de groep

> [!IMPORTANT]
> De service houdt rekening met de laatste 30 dagen telemetrie bij het aanbevelen van groepen. Voor een database te worden beschouwd als kandidaat voor een elastische pool, moet er ten minste 7 dagen bestaan. Databases die zich al in een elastische pool bevinden, worden niet aanbevolen voor een elastische pool.
>

De service beoordeelt wat de resource nodig heeft en hoe kosteneffectief het is om de individuele databases in elke servicecategorie te verplaatsen naar groepen van dezelfde categorie. Alle Standard databases op een server worden bijvoorbeeld beoordeeld op hoe ze in een Standard elastische groep passen. Dit betekent dat de service geen aanbevelingen doet voor het verplaatsen van databases naar een andere categorie, zoals het verplaatsen van een Standard database naar een Premium groep.

Nadat databases zijn toegevoegd aan de groep, worden aanbevelingen dynamisch gegenereerd op basis van het gebruik van de databases die u hebt geselecteerd. Deze aanbevelingen worden weergegeven in de eDTU- en GB-gebruiksgrafiek en in een banner aan de bovenkant van de **pool configureren** pagina. Deze aanbevelingen zijn bedoeld om u te helpen bij het maken van een elastische pool die geoptimaliseerd is voor uw specifieke databases.

![dynamische aanbevelingen](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

### <a name="manage-and-monitor-an-elastic-pool"></a>Beheren en controleren van een elastische pool

U kunt het gebruik van een elastische groep en de databases binnen die groep bewaken in de Azure portal. U kunt ook een aantal wijzigingen aanbrengen in uw elastische groep en alle wijzigingen tegelijk verzenden. Deze wijzigingen omvatten toevoegen of verwijderen van databases, elastische pool-instellingen wijzigen of database-instellingen wijzigen.

De volgende afbeelding toont een voorbeeld van de elastische groep. De weergave bevat:

* De grafieken voor het brongebruik van de elastische groep en de databases die zijn opgenomen in de groep.
* De **configureren** knop wijzigingen aanbrengen in de elastische groep van toepassingen.
* De **database maken** knop waarmee een database wordt gemaakt en toegevoegd aan de huidige elastische groep.
* Groot aantal databases beheren elastische taken die u helpen door te voeren van Transact-SQL-scripts op alle databases in een lijst.

![Toepassingen weergeven](./media/sql-database-elastic-pool-manage-portal/basic.png)

U kunt gaan naar een bepaalde groep om te zien van de bronnen beter worden benut. Standaard worden de toepassingen is geconfigureerd voor het weergeven van opslag en het eDTU-gebruik voor het afgelopen uur. De grafiek kan worden geconfigureerd voor andere metrische gegevens weergeven over verschillende tijdvensters. Klik op de **Resourcegebruik** grafiek onder **elastische pool bewaking** wilt weergeven, een gedetailleerde weergave van de opgegeven metrische gegevens op de opgegeven periode.

![Bewaking van de elastische groep](./media/sql-database-elastic-pool-manage-portal/basic-2.png)

![Metrische pagina](./media/sql-database-elastic-pool-manage-portal/metric.png)

### <a name="to-customize-the-chart-display"></a>De grafiekweergave aanpassen

U kunt de grafiek en weer te geven van andere metrische gegevens zoals CPU-percentage, gegevens-IO-percentage en logboek IO percentage gebruikt van de metrische pagina bewerken.

![Klik op bewerken](./media/sql-database-elastic-pool-manage-portal/edit-metric.png)

Op de **grafiek bewerken** formulier, selecteert u een tijdsbereik (voorbij vandaag de dag, uur of afgelopen week) of klik op **aangepaste** datumbereik selecteren in de afgelopen twee weken. U kunt kiezen tussen een staaf- of een lijndiagram en selecteer vervolgens de bronnen om te controleren.

> [!Note]
> Metrische gegevens met de dezelfde eenheid kunnen worden weergegeven in de grafiek op hetzelfde moment. Bijvoorbeeld, als u 'eDTU percentage' selecteert kunt vervolgens u alleen selecteren andere metrische gegevens met percentage als de maateenheid.
>

![Klik op bewerken](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

### <a name="manage-and-monitor-databases-in-an-elastic-pool"></a>Beheren en controleren van de databases in een elastische pool

Afzonderlijke databases kunnen ook worden gecontroleerd voor potentiële problemen. Onder **elastische Database bewaken**, er is een diagram waarin metrische gegevens voor vijf databases. Standaard het diagram toont de bovenste 5 databases in de groep door gemiddeld eDTU-gebruik in het afgelopen uur. 

![Bewaking van de elastische groep](./media/sql-database-elastic-pool-manage-portal/basic-3.png)

Klik op de **eDTU-gebruik voor de databases voor het afgelopen uur** onder **elastische database bewaken**. Hiermee opent u **Database Resourcegebruik** en biedt een gedetailleerde weergave van het Databasegebruik in de groep. Het raster in het onderste gedeelte van de pagina kunt u geen databases selecteren in de groep om het gebruik ervan in de grafiek (maximaal 5 databases) weer te geven. U kunt ook de metrische gegevens en het tijdstip venster wordt weergegeven in de grafiek door te klikken op **grafiek bewerken**.

![Database-gebruik bronpagina](./media/sql-database-elastic-pool-manage-portal/db-utilization.png)

### <a name="to-customize-the-view"></a>De weergave aanpassen

U kunt een tijdsbereik (voorbij uur of afgelopen 24 uur) of klik op de grafiek bewerken **aangepaste** selecteren van een andere dag in de afgelopen twee weken om weer te geven.

![Klik op de grafiek bewerken](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

![Klik op aangepast](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

U kunt ook klikken op de **vergelijken databases door** dropdown selecteren van een andere waarde voor gebruik bij het vergelijken van databases.

![De grafiek bewerken](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="to-select-databases-to-monitor"></a>Databases bewaken selecteren

In de lijst van de database op de **Database Resourcegebruik** pagina kunt u bepaalde databases vinden door te zoeken door de pagina's in de lijst of typ in de naam van een database. Gebruik de selectievakjes om de database te selecteren.

![Zoeken naar databases bewaken](./media/sql-database-elastic-pool-manage-portal/select-dbs.png)


### <a name="add-an-alert-to-an-elastic-pool-resource"></a>Een waarschuwing toevoegen aan een resource voor de elastische groep

U kunt regels toevoegen aan een elastische groep die e-mail verzenden naar personen of waarschuwing tekenreeksen met URL-eindpunten wanneer de elastische groep treffers een drempelwaarde voor overbenutting die u hebt ingesteld.

**Een waarschuwing toevoegen aan een resource:**

1. Klik op de **Resourcegebruik** grafiek te openen de **metriek** pagina, klikt u op **waarschuwing toevoegen**, en geef vervolgens de informatie in de **een waarschuwingsregeltoevoegen** pagina (**Resource** wordt automatisch ingesteld tot worden de toepassingen waarmee u werkt).
2. Typ een **naam** en **beschrijving** die de waarschuwing u en de ontvangers identificeert.
3. Kies een **metriek** die u wilt een waarschuwing in de lijst.

   De grafiek wordt dynamisch Resourcegebruik voor deze metriek bij het kiezen van een drempelwaarde.

4. Kies een **voorwaarde** (groter dan maximaal, enzovoort) en een **drempelwaarde**.
5. Kies een **periode** tijdsperiode waarin de meetwaarde regel moet worden voldaan voordat de waarschuwing triggers.
6. Klik op **OK**.

Zie voor meer informatie [waarschuwingen van de SQL-Database maken in Azure portal](sql-database-insights-alerts-portal.md).

### <a name="move-a-database-into-an-elastic-pool"></a>Een database verplaatsen naar een elastische pool

U kunt toevoegen of verwijderen van databases uit een bestaande groep. De databases kunnen zich in andere toepassingen. U kunt echter alleen databases die zich op dezelfde logische server toevoegen.

 ![Klik op pool configureren](./media/sql-database-elastic-pool-manage-portal/configure-pool.png)

![Klik op toevoegen aan groep](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)

![Selecteer de databases toevoegen](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

![In behandeling pool-toevoegingen](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

![Op Opslaan klikken](./media/sql-database-elastic-pool-manage-portal/click-save.png)

### <a name="move-a-database-out-of-an-elastic-pool"></a>Een database verplaatst buiten een elastische pool

![databases weergeven](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

### <a name="change-performance-settings-of-an-elastic-pool"></a>Instellingen van de prestaties van een elastische groep wijzigen

Als u het Resourcegebruik van een elastische pool bewaken, ontdekt u dat bepaalde aanpassingen nodig zijn. De groep moet mogelijk een wijziging in de limieten van de prestaties of de opslag. Mogelijk wilt u de database-instellingen in de groep wijzigen. U kunt de installatie van de groep wijzigen op elk gewenst moment om op te halen van de beste balans van prestaties en kosten. Zie [wanneer moet een elastische pool worden gebruikt?](sql-database-elastic-pool.md) voor meer informatie.

De limieten voor edtu's of opslag per groep en edtu's per database wijzigen:

![Resourcegebruik van de elastische groep](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

## <a name="manage-elastic-pools-and-databases-using-powershell"></a>Elastische pools en databases met behulp van PowerShell beheren

Als u wilt maken en beheren van elastische pools van de SQL-Database met Azure PowerShell, gebruik de volgende PowerShell-cmdlets. Als u wilt installeren of upgraden van PowerShell, Zie [Installeer Azure PowerShell-module](/powershell/azure/install-azurerm-ps). Als u wilt maken en beheren van databases, servers en firewall-regels, Zie [maken en beheren van Azure SQL Database-servers en -databases met behulp van PowerShell](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-powershell). 

> [!TIP]
> Zie voor PowerShell-voorbeeldscripts, [maken van elastische pools en databases verplaatsen tussen groepen en van een groep met behulp van PowerShell](scripts/sql-database-move-database-between-pools-powershell.md) en [Gebruik PowerShell om te bewaken en schalen van een elastische SQL-groep in Azure SQL Database](scripts/sql-database-monitor-and-scale-pool-powershell.md).
>

| Cmdlet | Beschrijving |
| --- | --- |
|[Nieuwe AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool)|Een pool voor elastische database maakt op een logische SQL-server.|
|[Get-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/get-azurermsqlelasticpool)|Elastische pools en hun eigenschapswaarden op een logische SQL-server opgehaald.|
|[Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool)|Hiermee wijzigt u de eigenschappen van een pool voor elastische database op een logische SQL-server. Gebruik bijvoorbeeld de **StorageMB** eigenschap te wijzigen van de maximale opslag van een elastische pool.|
|[Remove-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/remove-azurermsqlelasticpool)|Hiermee verwijdert u een pool voor elastische database op een logische SQL-server.|
|[Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity)|Hiermee wordt de status van bewerkingen in een elastische pool op een logische SQL-server opgehaald.|
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Maakt een nieuwe database in een bestaande pool of als een individuele database. |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Hiermee haalt u een of meer databases.|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Stelt eigenschappen van een database of een bestaande database verplaatst naar buiten of tussen elastische pools.|
|[Verwijder AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Hiermee verwijdert u een database.|


> [!TIP]
> Maken van veel databases in een elastische pool kan duren wanneer het wordt gedaan via de portal of PowerShell-cmdlets die slechts één database tegelijkertijd maken. Als u wilt maken voor een elastische pool automatiseren, Zie [CreateOrUpdateElasticPoolAndPopulate](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).
>

## <a name="manage-elastic-pools-and-databases-using-the-azure-cli"></a>Elastische pools en met de Azure CLI-databases beheren

Maken en beheren van elastische pools van de SQL-Database met de [Azure CLI](/cli/azure/overview), gebruikt u de volgende [Azure CLI SQL Database](/cli/azure/sql/db) opdrachten. Gebruik de [Cloud Shell](/azure/cloud-shell/overview) om de CLI in uw browser uit te voeren of [installeer](/cli/azure/install-azure-cli) de CLI op macOS, Linux of Windows. 

> [!TIP]
> Zie voor Azure CLI-voorbeeldscripts, [gebruik CLI verplaatsen van een Azure SQL database in een elastische SQL-groep](scripts/sql-database-move-database-between-pools-cli.md) en [gebruik Azure CLI voor het schalen van een elastische SQL-groep in Azure SQL Database](scripts/sql-database-scale-pool-cli.md).
>

| Cmdlet | Beschrijving |
| --- | --- |
|[elastische sql-AZ-groep maken](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_create)|Maakt een elastische pool.|
|[lijst met AZ sql elastische pool](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list)|Retourneert een lijst met elastische pools in een server.|
|[AZ sql elastische pool lijst-databases](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list_dbs)|Retourneert een lijst met databases in een elastische pool.|
|[AZ sql elastische pool-edities](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list_editions)|Omvat ook beschikbaar DTU groepsinstellingen, opslaglimieten, en per database-instellingen. Om te reduceren uitgebreidheid aanvullende opslaglimieten en per database instellingen standaard zijn verborgen.|
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
|[DATABASE (Transact-SQL) verwijderen](/sql/t-sql/statements/drop-database-transact-sql)|Hiermee verwijdert u een database.|
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
|[Aanbevolen elastische Pools - ophalen](/rest/api/sql/recommendedelasticpools/get)|Hiermee haalt u een recommented elastische pool.|
|[Aanbevolen elastische Pools - lijst per Server](/rest/api/sql/recommendedelasticpools/listbyserver)|Retourneert aanbevolen elastische pools.|
|[Aanbevolen elastische Pools - lijst metrische gegevens](/rest/api/sql/recommendedelasticpools/listmetrics)|Retourneert recommented elastische pool metrische gegevens.|
|[Activiteiten van de elastische groep](/rest/api/sql/elasticpoolactivities)|Retourneert de elastische groep activiteiten.|
|[Elastische Pool databaseactiviteiten](/rest/api/sql/elasticpooldatabaseactivities)|Retourneert de activiteit op databases binnen een elastische pool.|
|[Databases - maken of bijwerken](/rest/api/sql/databases/createorupdate)|Een nieuwe database maken of bijwerken van een bestaande database.|
|[Databases - Get](/rest/api/sql/databases/get)|Hiermee haalt u een database.|
|[Databases - verkrijgen door de elastische groep](/rest/api/sql/databases/getbyelasticpool)|Hiermee haalt u een database in een elastische pool.|
|[Databases - verkrijgen door aanbevolen elastische Pool](/rest/api/sql/databases/getbyrecommendedelasticpool)|Hiermee haalt u een database binnen een recommented elastische pool.|
|[Databases - lijst door de elastische groep](/rest/api/sql/databases/listbyelasticpool)|Retourneert een lijst met databases in een elastische pool.|
|[Databases - lijst met aanbevolen elastische Pool](/rest/api/sql/databases/listbyrecommendedelasticpool)|Retourneert een lijst met databases binnen een recommented elastische pool.|
|[Databases - lijst per Server](/rest/api/sql/databases/listbyserver)|Retourneert een lijst met databases in een server.|
|[Databases - Update](/rest/api/sql/databases/update)|Een bestaande database-updates.|

## <a name="next-steps"></a>Volgende stappen

* Zie voor een video [video loop van de Microsoft Virtual Academy op elastische mogelijkheden van Azure SQL Database](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
* Zie [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) voor meer informatie over ontwerppatronen voor SaaS-toepassingen met elastische groepen.
* Zie voor een SaaS-zelfstudie met elastische pools, [Inleiding tot de Wingtip SaaS-toepassing](sql-database-wtp-overview.md).
