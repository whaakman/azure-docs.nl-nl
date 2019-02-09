---
title: Beheer van meerdere SQL-databases met elastische pools-Azure | Microsoft Docs
description: Beheer en schaal van meerdere SQL-databases - honderden en duizenden - met behulp van elastische pools. Een enkele prijs voor resources die u distribueren kunt waar nodig.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: ninarn, carlrab
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: 4e4de6d487aaad8a0d036928fe59b47f25157bf6
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55963842"
---
# <a name="elastic-pools-help-you-manage-and-scale-multiple-azure-sql-databases"></a>Elastische pools kunt u beheren en schalen van meerdere Azure SQL-databases

Elastische pools in SQL Database zijn eenvoudige, kosteneffectieve oplossingen voor het beheren en schalen van meerdere databases die een verschillend en onvoorspelbaar verbruik hebben. De databases in een elastische pool op een enkele Azure SQL Database-server en delen van een bepaald aantal resources in een vaste prijs. Met elastische groepen in Azure SQL Database kunnen SaaS-ontwikkelaars de prijsprestaties voor een groep databases binnen een voorgeschreven budget optimaliseren en flexibele prestaties voor elke database leveren.

## <a name="what-are-sql-elastic-pools"></a>Wat zijn elastische SQL-groepen

SaaS-ontwikkelaars ontwikkelen toepassingen boven op grootschalige gegevenslagen die uit meerdere databases bestaan. Een algemeen patroon is de inrichting van een individuele database voor elke klant. Maar verschillende klanten hebben vaak verschillende en onvoorspelbare gebruikspatronen, en de resourcevereisten van elke gebruiker van een individuele database zijn moeilijk te voorspellen. Vroeger had u twee opties:

- Te veel inrichten van resources op basis van piekgebruik en via betalen, of
- Een te voorzichtige inrichting om kosten, ten koste van prestaties en klanttevredenheid tijdens pieken te besparen.

Elastische groepen lossen dit probleem op door ervoor te zorgen dat databases de prestaties die ze nodig hebben krijgen wanneer ze deze nodig hebben. Ze bieden een eenvoudig mechanisme voor het toewijzen van resources binnen een voorspelbaar budget. Zie [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) voor meer informatie over ontwerppatronen voor SaaS-toepassingen met elastische groepen.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Elastic-databases-helps-SaaS-developers-tame-explosive-growth/player]
>

> [!IMPORTANT]
> Er zijn geen kosten per database voor elastische pools. U wordt gefactureerd voor elk uur dat een pool bestaat op het hoogste edtu's of vCores, ongeacht het gebruik of of de groep korter dan een uur actief was.

Elastische pools bieden de ontwikkelaar om aan te schaffen resources voor een groep die wordt gedeeld door meerdere databases aan onvoorspelbare perioden van gebruik door afzonderlijke databases. U kunt resources configureren voor de groep op basis van ofwel de [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) of de [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md). De Resourcevereiste voor een groep wordt bepaald door de gezamenlijke gebruik van de bijbehorende databases. De hoeveelheid resources die beschikbaar is voor de groep wordt bepaald door het budget van de ontwikkelaar. De ontwikkelaar gewoon databases aan de groep toegevoegd, Hiermee stelt u de minimale en maximale beschikbare resources voor de databases (de minimale en maximale aantal dtu's of minimum of maximum aantal vCores, afhankelijk van uw keuze van het model resourcing), en stelt vervolgens de resources van de groep op basis van hun budget. Met groepen kan een ontwikkelaar services naadloos met een alsmaar groeiende schaal uitbreiden van een kleine startende ondernemer tot een volwassen bedrijf.

Binnen de pool hebben afzonderlijke databases de flexibiliteit om de schaal automatisch aan te passen binnen ingestelde parameters. Onder zware belasting kan verbruiken een database meer resources om te voldoen aan de vraag. Databases met een lichte belasting minder verbruiken en databases zonder enige belasting geen resources verbruiken. De inrichting van resources voor de hele pool in plaats van afzonderlijke databases vereenvoudigt uw beheertaken. Bovendien hebt u een voorspelbaar budget voor de pool. Aanvullende bronnen kunnen worden toegevoegd aan een bestaande pool met uitvaltijd, behalve dat de databases moeten mogelijk worden verplaatst om te voorzien van de extra rekenresources voor de nieuwe eDTU-reservering. Op dezelfde manier als extra resources niet meer nodig zijn kunnen ze worden verwijderd uit een bestaande pool op elk gewenst moment in-time. Daarnaast kunt u databases aan de groep toevoegen of uit de groep verwijderen. Als een database naar verwachting minder resources nodig heeft, kunt u deze verwijderen.

> [!NOTE]
> Bij het verplaatsen van databases naar of vanuit de elastische pool, is er geen uitvaltijd, met uitzondering van een korte periode (orde van grootte seconden) aan het einde van de bewerking wanneer databaseverbindingen worden verwijderd.

## <a name="when-should-you-consider-a-sql-database-elastic-pool"></a>Wanneer moet u rekening houden met een SQL Database elastische pool

Pools zijn geschikt voor een groot aantal databases met specifieke gebruikspatronen. Voor een bepaalde database wordt dit patroon gekenmerkt door een laag gemiddeld gebruik met relatief incidentele gebruikspieken.

Hoe meer databases u aan een groep kunt toevoegen, des te groter worden de besparingen. Afhankelijk van uw gebruikspatroon van toepassingen is het mogelijk om besparingen te zien met slechts twee S3-databases.

In de volgende secties ziet u hoe u kunt bepalen of het voor uw specifieke verzameling databases voordelig is om deel uit te maken van een groep. In de voorbeelden wordt gebruik gemaakt van Standard-groepen, maar dezelfde principes gelden ook voor Basic- en Premium-groepen.

### <a name="assessing-database-utilization-patterns"></a>Databasegebruikspatronen beoordelen

De volgende afbeelding toont een voorbeeld van een database die doorgaans weinig wordt gebruikt, maar bij soms ook extreem actief is. Dit is een gebruikspatroon dat geschikt is voor een groep:

   ![een individuele database die geschikt is voor een groep](./media/sql-database-elastic-pool/one-database.png)

Gedurende de geïllustreerde periode van vijf minuten piekt DB1 tot 90 DTU's, maar het gemiddelde gebruik is nog geen vijf DTU's. Een S3-compute-grootte is vereist voor het uitvoeren van deze werkbelasting in een individuele database, maar in dat geval de meeste resources niet-gebruikte tijdens perioden van weinig activiteit.

Met een groep kunnen deze ongebruikte DTU’s worden gedeeld met meerdere databases, wat de benodigde DTU's en de totale kosten vermindert.

Stel dat er in het vorige voorbeeld meer databases zijn die een soortgelijk gebruikspatroon hebben als DB1. In de volgende twee afbeeldingen, het gebruik van vier databases en 20 databases gelaagd over dezelfde grafiek ter illustratie van de niet-overlappende aard van het gebruik na verloop van tijd met behulp van het op DTU gebaseerde aankoopmodel:

   ![vier databases met een gebruikspatroon dat geschikt is voor een groep](./media/sql-database-elastic-pool/four-databases.png)

   ![twintig databases met een gebruikspatroon dat geschikt is voor een groep](./media/sql-database-elastic-pool/twenty-databases.png)

Het gezamenlijke DTU-gebruik van alle 20 databases wordt aangegeven door de zwarte lijn in voorgaande afbeelding. U ziet dat het totale DTU-gebruik nooit hoger is dan honderd DTU’s en dat de twintig databases gedurende deze periode honderd eDTU's kunnen delen. Dit resulteert in 20 maal zo in dtu's en een 13 prijsvermindering ten opzichte van wanneer elke database in S3 compute-grootten voor individuele databases.

Dit voorbeeld is om de volgende redenen ideaal:

- Er zijn grote verschillen tussen piekgebruik en gemiddeld gebruik per database.
- Het piekgebruik voor elke database vindt plaats op verschillende momenten.
- eDTU‘s worden gedeeld tussen meerdere databases.

De prijs van een groep is een functie van de groep eDTU's. Hoewel de prijs per eDTU voor een groep 1,5 x groter is dan de prijs per DTU voor een individuele database, **kunnen eDTU's in een groep door veel databases worden gedeeld en zijn er dus minder eDTU's nodig**. Deze verschillen in prijsbepaling en het delen van eDTU's vormen de basis van de mogelijke prijsbesparing die groepen kunnen bieden.

De volgende vuistregels voor databaseaantallen en Databasegebruik helpen ervoor te zorgen dat een pool lagere kosten in vergelijking met het gebruik van compute-grootten voor individuele databases.

### <a name="minimum-number-of-databases"></a>Minimum aantal databases

Als de totale hoeveelheid resources voor individuele databases meer dan 1,5 keer de resources die nodig zijn voor de groep, en vervolgens een elastische pool rendabeler is.

***Voorbeeld van aankopen model op basis van DTU***<br>
Ten minste twee S3-databases of ten minste 15 S0-databases nodig zijn voor een groep van 100 edtu's rendabeler zijn dan het gebruik van compute-grootten voor individuele databases.

### <a name="maximum-number-of-concurrently-peaking-databases"></a>Maximum aantal gelijktijdig piekende databases

Resources worden gedeeld, niet alle databases in een groep gebruiken resources tot de limiet die beschikbaar voor individuele databases. Hoe minder databases gelijktijdig pieken, des te lager de resources van de groep kunnen worden ingesteld en des te rendabeler de groep wordt. In het algemeen zou moeten niet meer dan 2/3 (of 67%) van de databases in de pool tegelijkertijd pieken tot hun resources-limiet.

***Voorbeeld van aankopen model op basis van DTU***

Om de kosten voor drie S3-databases in een groep van 200 eDTU's te verlagen, kunnen maximaal twee van deze databases tegelijkertijd pieken in hun gebruik. Of, als meer dan twee van deze vier S3-databases gelijktijdig pieken, zou de groep moeten worden uitgebreid tot meer dan 200 eDTU's. Als de groep wordt uitgebreid tot meer dan 200 edtu's, moeten er meer S3-databases worden toegevoegd aan de groep kosten lager zijn dan compute grootten voor individuele databases te houden.

In dit voorbeeld wordt geen rekening gehouden met het gebruik van andere databases in de groep. Als alle databases voortdurend in enige mate gebruik maken van eDTU's, kan minder dan 2/3 (of 67%) van de databases tegelijkertijd pieken.

### <a name="resource-utilization-per-database"></a>Resourcegebruik per database

Een groot verschil tussen het piek- en gemiddelde gebruik van een database geeft langere perioden van laag gebruik en korte perioden hoog gebruik aan. Dit gebruikspatroon is ideaal voor het delen van resources met meerdere databases. Een database zou een geschikte kandidaat voor een groep kunnen zijn als het piekgebruik ongeveer 1,5 keer groter is dan het gemiddelde gebruik.

**Voorbeeld van aankopen model op basis van DTU**: Een S3-database die piekt tot 100 DTU's en gemiddeld 67 DTU's of minder gebruikt, is een goede kandidaat voor het delen van eDTU's in een groep. Ook een S1-database die piekt tot 20 DTU's en gemiddeld 13 DTU's of minder gebruikt, is een goede kandidaat voor een groep.

## <a name="how-do-i-choose-the-correct-pool-size"></a>Hoe kies ik de juiste poolgrootte

De aanbevolen grootte voor een groep is afhankelijk van de cumulatieve resources die nodig zijn voor alle databases in de groep. Dit omvat het bepalen van het volgende:

- Maximum aantal bronnen die door alle databases in de groep (maximum aantal dtu's of maximale vCores, afhankelijk van uw keuze van het model resourcing) wordt gebruikt.
- De maximum opslag in bytes die door alle databases in de groep wordt gebruikt.

Zie voor de beschikbare service-lagen voor elke resourcemodel, de [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) of de [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md).

Als u het hulpprogramma niet kunt gebruiken, kunnen de volgende stappen u helpen om te schatten of een groep rendabeler is dan individuele databases:

1. Maak een schatting van de edtu's of vCores die nodig zijn voor de groep als volgt:

   Voor DTU gebaseerde aankoopmodel: MAX(<*Totaal aantal DB's* X *gemiddelde DTU-gebruik per DB*>,<br>  
   <*aantal gelijktijdig piekende databases* X *piek-DTU-gebruik per DB*)

   Voor op vCore gebaseerde aankoopmodel: MAX (<*totaal aantal DB's* X *Gemiddeld gebruik per DB vCore*>,<br>  
   <*Aantal gelijktijdig piekende databases* X *vCore piekgebruik per DB*)

2. Schat hoeveel opslagruimte de groep nodig heeft door het aantal bytes op te tellen dat nodig is voor alle databases in de groep. Bepaal daarna hoe groot de eDTU-groep moet zijn om aan deze hoeveelheid opslag te voldoen.
3. Voor de DTU gebaseerde aankoopmodel, zet u de grootste waarde van de eDTU-schatting uit stap 1 en stap 2. Voor het op vCore gebaseerde aankoopmodel, nemen de vCore-schatting uit stap 1.
4. Zie de [pagina met prijzen van SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/) en zoek de kleinste groepsomvang die groter is dan de schatting van stap 3.
5. Vergelijk de prijs van de groep uit stap 5 met de prijs van het gebruik van de juiste Reken-grootten voor individuele databases.

## <a name="using-other-sql-database-features-with-elastic-pools"></a>Met behulp van andere functies van SQL Database met elastische pools

### <a name="elastic-jobs-and-elastic-pools"></a>Elastische taken en elastische pools

Met een groep worden beheertaken vereenvoudigd door scripts in **[elastische taken](sql-database-elastic-jobs-overview.md)** uit te voeren. Een elastische taak elimineert de meeste saaie handelingen die komen kijken bij grote aantallen databases. Zie [Getting started with Elastic Database jobs](sql-database-elastic-jobs-getting-started.md) (Aan de slag met taken voor elastische databases) om aan de slag te gaan.

Zie [Scaling out with Azure SQL Database](sql-database-elastic-scale-introduction.md) (Uitbreiden met Azure SQL Database) voor meer informatie over andere databasehulpprogramma's voor het werken met meerdere databases.

### <a name="business-continuity-options-for-databases-in-an-elastic-pool"></a>Opties voor bedrijfscontinuïteit voor databases in een elastische pool

Gepoolde databases ondersteunen in het algemeen dezelfde [bedrijfscontinuïteitsfuncties](sql-database-business-continuity.md) die beschikbaar zijn voor individuele databases.

- **Point-in-time restore**

  Point-in-time restore maakt gebruik van automatische databaseback-ups te herstellen van een database in een pool naar een bepaald punt in tijd. Zie [Herstel naar een bepaald tijdstip](sql-database-recovery-using-backups.md#point-in-time-restore)

- **Geo-restore**

  Geo-herstel biedt de standaardoptie voor herstel wanneer een database is niet beschikbaar vanwege een incident in de regio waar de database wordt gehost. Zie [Restore an Azure SQL Database or failover to a secondary](sql-database-disaster-recovery.md) (Een Azure SQL Database of herstellen of een failover uitvoeren naar een secundaire server)

- **Actieve Geo-replicatie**

  Voor toepassingen die zwaardere herstelvereisten dan geo-herstel kan bieden, configureert u [actieve geo-replicatie](sql-database-active-geo-replication.md) of een [automatische-failovergroep](sql-database-auto-failover-group.md).

## <a name="creating-a-new-sql-database-elastic-pool-using-the-azure-portal"></a>Het maken van een nieuwe SQL Database elastische pool met behulp van de Azure portal

Er zijn twee manieren kunt u een elastische pool maken in Azure portal.

1. U kunt een elastische pool maken door te zoeken naar **elastische SQL-groep** in de **Marketplace** of klikken op **+ toevoegen** blade voor browsen op de elastische SQL-pools. U zijn kunt opgeven van een nieuwe of bestaande server via de werkstroom voor het inrichten van deze toepassingen.
2. Of u kunt een elastische pool maken door te klikken en te navigeren naar een bestaande SQL server **pool maken** te maken van een groep rechtstreeks naar die server. Het enige verschil is hier is dat u slaat u de stap waarin u de server opgeven tijdens de werkstroom voor het inrichten van toepassingen.

> [!NOTE]
> U kunt meerdere groepen maken op een server, maar u kunt geen databases van verschillende servers toevoegen aan dezelfde groep.

De servicelaag van de pool bepaalt welke functies beschikbaar zijn voor de elastic in de groep en de maximale hoeveelheid resources die beschikbaar zijn voor elke database. Zie voor meer informatie, Resource-limieten voor elastische pools in de [DTU-model](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-compute-sizes). Zie voor vCore gebaseerde resourcelimieten voor elastische pools, [vCore gebaseerde resourcelimieten - elastische pools](sql-database-vcore-resource-limits-elastic-pools.md).

Configureer de bronnen en prijzen van de groep, klikt u op **groep configureren**. Selecteer vervolgens een servicelaag, databases aan de groep toevoegen en configureren van de resourcelimieten voor de groep en de bijbehorende databases.

Wanneer u klaar bent met het configureren van de groep, kunt u 'Toepassen', de naam van de groep, klikt u op en klik op 'OK' voor het maken van de groep.

## <a name="monitor-an-elastic-pool-and-its-databases"></a>Een elastische pool en de bijbehorende databases bewaken

U kunt het gebruik van een elastische pool en de databases in die groep controleren in de Azure-portal. U kunt ook een set wijzigingen aanbrengen in de elastische groep en alle wijzigingen op hetzelfde moment te verzenden. Deze wijzigingen omvatten toevoegen of verwijderen van databases, elastische pool-instellingen wijzigen of uw database-instellingen wijzigen.

Voor het starten van de elastische pool bewaken, zoeken en openen van een elastische pool in de portal. Eerst ziet u een scherm dat u een van de status van de elastische pool overzicht. Dit omvat:

- Grafieken met Resourcegebruik van de elastische groep controleren
- Recente waarschuwingen en aanbevelingen, indien beschikbaar, voor de elastische pool

De volgende afbeelding toont een voorbeeld van de elastische pool:

![Toepassingen weergeven](./media/sql-database-elastic-pool-manage-portal/basic.png)

Als u meer informatie over de groep die u kunt klikken op een van de beschikbare informatie in dit overzicht wilt. Te klikken op de **Resourcegebruik** grafiek, gaat u naar de Azure Monitoring weergave waar u de metrische gegevens en het tijdstip venster wordt weergegeven in de grafiek kunt aanpassen. Op alle beschikbare meldingen te klikken, gaat u naar een blade ziet u de volledige details van deze waarschuwing of een aanbeveling.

Als u controleren van de databases op uw pool wilt, kunt u klikken op **Database Resourcegebruik** in de **bewaking** sectie van de resourcemenu aan de linkerkant.

![Pagina database resource-gebruik](./media/sql-database-elastic-pool-manage-portal/db-utilization.png)

### <a name="to-customize-the-chart-display"></a>De grafiekweergave aanpassen

U kunt de metrische pagina om weer te geven van andere metrische gegevens zoals CPU-percentage, gegevens-IO-percentage en logboek-IO-percentage gebruikt en de grafiek bewerken.

Op de **grafiek bewerken** formulier, kunt u een vaste tijd bereik of klik op **aangepaste** een 24-uurs tijdvenster selecteren in de afgelopen twee weken en selecteer vervolgens de resources om te controleren.

### <a name="to-select-databases-to-monitor"></a>Databases selecteren om te controleren

Standaard wordt de grafiek in de **databasebron** blade ziet u de databases van de top 5 DTU of CPU (afhankelijk van uw servicelaag). U kunt overschakelen van de databases in deze grafiek door te selecteren en op te heffen databases in de lijst onder de grafiek via de selectievakjes in aan de linkerkant.

U kunt ook meer metrische gegevens naar de weergave naast elkaar in deze database-tabel voor een volledig overzicht van de prestaties van uw databases selecteren.

Zie voor meer informatie, [waarschuwingen van de SQL-Database maken in Azure portal](sql-database-insights-alerts-portal.md).

## <a name="next-steps"></a>Volgende stappen

- Als u wilt schalen elastische pools, Zie [schalen van elastische pools](sql-database-elastic-pool.md) en [schalen van een elastische pool - voorbeeldcode](scripts/sql-database-monitor-and-scale-pool-powershell.md)
- Zie voor een video [videocursus van Microsoft Virtual Academy over elastische mogelijkheden van Azure SQL Database](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
- Zie [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) voor meer informatie over ontwerppatronen voor SaaS-toepassingen met elastische groepen.
- Zie voor een SaaS-zelfstudie met elastische pools, [Inleiding tot de Wingtip SaaS-toepassing](sql-database-wtp-overview.md).
