---
title: Wanneer moet een pool voor Elastic Database worden gebruikt?
description: Een pool voor Elastic Database is een verzameling beschikbare resources die worden gedeeld door een groep elastische databases. Dit document bevat richtlijnen aan de hand waarvan u de geschiktheid van het gebruik van een pool voor Elastic Database voor een groep databases kunt beoordelen.
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 3d3941d5-276c-4fd2-9cc1-9fe8b1e4c96c
ms.service: sql-database
ms.custom: sharded databases pool; app development
ms.devlang: NA
ms.date: 08/08/2016
ms.author: sstein
ms.workload: data-management
ms.topic: get-started-article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 867f06c1fae3715ab03ae4a3ff4ec381603e32f7
ms.openlocfilehash: 408cf315f8b44c9ebf852c3ccbf2ed93c70ef22f


---
# <a name="when-should-an-elastic-database-pool-be-used"></a>Wanneer moet een pool voor Elastic Database worden gebruikt?
Bepaal of het gebruik van een pool voor Elastic Database rendabel is op basis van databasegebruikspatronen en prijsverschillen tussen een pool voor Elastic Database en individuele databases. Meer richtlijnen zijn beschikbaar om u te helpen bij het bepalen van de huidige groepsgrootte die vereist is voor een bestaande set met SQL-databases.  

* Zie [Pools voor Elastic Database](sql-database-elastic-pool.md) (Pools voor Elastic Database met SQL Database) voor een overzicht van groepen.

> [!NOTE]
> Elastische pools zijn algemeen beschikbaar in alle Azure-regio's, behalve in West-India, waar deze zich momenteel in de previewfase bevinden.  De algemene beschikbaarheid van elastische pools in deze regio wordt zo snel mogelijk gerealiseerd.
> 
> 

## <a name="elastic-database-pools"></a>Pools voor Elastic Database
SaaS-ontwikkelaars ontwikkelen toepassingen boven op grootschalige gegevenslagen die uit meerdere databases bestaan. Een algemeen patroon is de inrichting van een individuele database voor elke klant. Maar verschillende klanten hebben vaak verschillende en onvoorspelbare gebruikspatronen, en de resourcevereisten van elke gebruiker van een individuele database zijn moeilijk te voorspellen. Daarom zal de ontwikkelaar misschien tegen hoge kosten voor een overinrichting van resources zorgen om een gewenste doorvoer en responstijden voor alle databases te garanderen. Of misschien heeft de ontwikkelaar minder te besteden en loopt hij liever het risico van slechtere prestaties voor de klanten. Zie [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) voor meer informatie over ontwerppatronen voor SaaS-toepassingen met elastische groepen.

Met elastische groepen in Azure SQL Database kunnen SaaS-ontwikkelaars de prijsprestaties voor een groep databases binnen een voorgeschreven budget optimaliseren en flexibele prestaties voor elke database leveren. Groepen bieden de ontwikkelaar de mogelijkheid om eDTU's (elastic Database Transaction Units) te kopen voor een groep die door meerdere databases wordt gedeeld, om tegemoet te komen aan onvoorspelbare perioden van gebruik door afzonderlijke databases. De eDTU-vereiste voor een groep wordt bepaald door het gezamenlijke gebruik van de databases. Het aantal eDTU's dat beschikbaar is voor de groep wordt bepaald door het budget van de ontwikkelaar. Met groepen heeft de ontwikkelaar rationele argumenten ten aanzien van de impact van het budget op de prestaties en vice versa voor de groep. De ontwikkelaar voegt gewoon databases aan de groep toe, stelt het minimum en maximum aantal eDTU's voor de databases in en stelt vervolgens de eDTU van de groep in op basis van het budget. Met groepen kan een ontwikkelaar services naadloos met een alsmaar groeiende schaal uitbreiden van een kleine startende ondernemer tot een volwassen bedrijf.  

## <a name="when-to-consider-a-pool"></a>Wanneer is een groep interessant
Pools zijn geschikt voor een groot aantal databases met specifieke gebruikspatronen. Voor een bepaalde database wordt dit patroon gekenmerkt door een laag gemiddeld gebruik met relatief incidentele gebruikspieken.

Hoe meer databases u aan een groep kunt toevoegen, des te groter worden de besparingen. Afhankelijk van uw gebruikspatroon van toepassingen is het mogelijk om besparingen te zien met slechts twee S3-databases.  

In de volgende secties ziet u hoe u kunt bepalen of het voor uw specifieke verzameling databases voordelig is om deel uit te maken van een groep. In de voorbeelden wordt gebruik gemaakt van Standard-groepen, maar dezelfde principes gelden ook voor Basic- en Premium-groepen.

### <a name="assessing-database-utilization-patterns"></a>Databasegebruikspatronen beoordelen
De volgende afbeelding toont een voorbeeld van een database die doorgaans weinig wordt gebruikt, maar bij soms ook extreem actief is. Dit is een gebruikspatroon dat geschikt is voor een groep:

   ![een individuele database die geschikt is voor een groep](./media/sql-database-elastic-pool-guidance/one-database.png)

Gedurende de periode van vijf minuten die hierboven wordt geïllustreerd, piekt DB1 tot 90 DTU's, maar het gemiddelde gebruik is nog geen vijf DTU's. Er is een S3-prestatieniveau vereist om deze werkbelasting in een individuele database uit te voeren, maar in dat geval worden de meeste resources tijdens perioden van weinig activiteit niet benut.

Met een groep kunnen deze ongebruikte DTU’s worden gedeeld met meerdere databases, wat de totale hoeveelheid benodigde DTU's en de totale kosten vermindert.

Stel dat er in het vorige voorbeeld meer databases zijn die een soortgelijk gebruikspatroon hebben als DB1. In de volgende twee afbeeldingen is het gebruik van vier databases en twintig databases gelaagd over dezelfde grafiek om de niet-overlappende aard van het gebruik na verloop van tijd te illustreren:

   ![vier databases met een gebruikspatroon dat geschikt is voor een groep](./media/sql-database-elastic-pool-guidance/four-databases.png)

   ![twintig databases met een gebruikspatroon dat geschikt is voor een groep](./media/sql-database-elastic-pool-guidance/twenty-databases.png)

Het gezamenlijke DTU-gebruik van alle twintig databases wordt aangegeven door de zwarte lijn in bovenstaande afbeelding. U ziet dat het totale DTU-gebruik nooit hoger is dan honderd DTU’s en dat de twintig databases gedurende deze periode honderd eDTU's kunnen delen. Dit resulteert in een twintigvoudige vermindering in DTU's en een dertienvoudige prijsvermindering ten opzichte van wanneer elke database in S3-prestaties voor individuele databases wordt geplaatst.

Dit voorbeeld is om de volgende redenen ideaal:

* Er zijn grote verschillen tussen piekgebruik en gemiddeld gebruik per database.  
* Het piekgebruik voor elke database vindt plaats op verschillende momenten.
* eDTU's worden gedeeld door een groot aantal databases.

De prijs van een groep is een functie van de groep eDTU's. Hoewel de prijs per eDTU voor een groep 1,5 x groter is dan de prijs per DTU voor een individuele database, **kunnen eDTU's in een groep door veel databases worden gedeeld en zijn er dus in veel gevallen minder eDTU's nodig**. Deze verschillen in prijsbepaling en het delen van eDTU's vormen de basis van de mogelijke prijsbesparing die groepen kunnen bieden.  

De volgende vuistregels voor databaseaantallen en databasegebruik helpen ervoor te zorgen dat een groep lagere kosten met zich meebrengt, vergeleken met het gebruik van prestatieniveaus voor individuele databases.

### <a name="minimum-number-of-databases"></a>Minimum aantal databases
Als de som van de DTU's van prestatieniveaus voor individuele databases meer dan 1,5 keer het aantal eDTU's is dat nodig is voor de groep, is een elastische groep rendabeler. Zie [eDTU and storage limits for elastic database pools and elastic databases](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools-and-elastic-databases) (eDTU- en opslaglimieten voor pools voor Elastic Database en elastische databases) voor beschikbare formaten.

***Voorbeeld***<br>
Er zijn ten minste twee S3-databases of ten minste 15 S0-databases nodig wil een groep van 100 eDTU's rendabeler zijn dan het gebruik van de prestatieniveaus voor individuele databases.

### <a name="maximum-number-of-concurrently-peaking-databases"></a>Maximum aantal gelijktijdig piekende databases
Wanneer eDTU's worden gedeeld, kunnen niet alle databases in een groep de eDTU's gebruiken tot de limiet die beschikbaar is wanneer prestatieniveaus voor individuele databases worden gebruikt. Hoe minder databases gelijktijdig pieken, des te lager kan de groeps-eDTU worden ingesteld en des te rendabeler wordt de groep. In het algemeen zou niet meer dan 2/3 (of 67%) van de databases in de groep tegelijkertijd tot de maximale eDTU mogen pieken.

***Voorbeeld***<br>
Om de kosten voor drie S3-databases in een groep van 200 eDTU's te verlagen, kunnen maximaal twee van deze databases tegelijkertijd pieken in hun gebruik.  Of, als meer dan twee van deze vier S3-databases gelijktijdig pieken, zou de groep moeten worden uitgebreid tot meer dan 200 eDTU's.  En als de groep wordt uitgebreid tot meer dan 200 eDTU's, moeten er meer S3-databases aan de groep worden toegevoegd om de kosten lager te houden dan bij prestatieniveaus voor individuele databases.  

In dit voorbeeld wordt geen rekening gehouden met het gebruik van andere databases in de groep. Als alle databases voortdurend in enige mate gebruik maken van eDTU's, kan minder dan 2/3 (of 67%) van de databases tegelijkertijd pieken.

### <a name="dtu-utilization-per-database"></a>DTU-gebruik per database
Een groot verschil tussen het piek- en gemiddelde gebruik van een database geeft langere perioden van laag gebruik en korte perioden hoog gebruik aan. Dit gebruikspatroon is ideaal voor het delen van resources met meerdere databases. Een database zou een geschikte kandidaat voor een groep kunnen zijn als het piekgebruik ongeveer 1,5 keer groter is dan het gemiddelde gebruik.

***Voorbeeld***<br>
Een S3-database die piekt tot 100 DTU's en gemiddeld 67 DTU's of minder gebruikt, is een goede kandidaat voor het delen van eDTU's in een groep.  Ook een S1-database die piekt tot 20 DTU's en gemiddeld 13 DTU's of minder gebruikt, is een goede kandidaat voor een groep.

## <a name="sizing-an-elastic-pool"></a>Het formaat van een elastische groep bepalen
De optimale grootte voor een groep hangt af van de totale eDTU's en opslagresources die nodig zijn voor alle databases in de groep. Hiervoor moet u de grootste waarde van de volgende factoren bepalen:

* Het maximum aantal DTU's dat door alle databases in de groep wordt gebruikt.
* De maximum opslag in bytes die door alle databases in de groep wordt gebruikt.

Zie [eDTU and storage limits for elastic database pools and elastic databases](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools-and-elastic-databases) (eDTU- en opslaglimieten voor pools voor Elastic Database en elastische databases) voor beschikbare formaten.

SQL Database evalueert automatisch het historisch resourcegebruik van databases in een bestaande SQL Database-server en op basis daarvan wordt de juiste poolconfiguratie in de Azure-portal aanbevolen. Naast de aanbevelingen wordt met een geïntegreerde functie het eDTU-gebruik voor een aangepaste groep databases op de server geschat. Hiermee kunt u een 'wat als'-analyse uitvoeren door interactief databases aan de groep toe te voegen of uit de groep te verwijderen, om zo een analyse van het resourcegebruik en formaatadvies te krijgen voordat u uw wijzigingen doorvoert. Zie [Monitor, manage, and size an elastic pool](sql-database-elastic-pool-manage-portal.md) (Een elastische groep bewaken, beheren en van formaat wijzigen) voor instructies.

Voor flexibelere beoordelingen van resourcegebruik die ad-hoc schattingen van grootten mogelijk maken voor servers die ouder zijn dan V12, en voor het schatten van grootten voor databases op verschillende servers, raadpleegt u de [Powershell script for identifying databases suitable for an elastic database pool](sql-database-elastic-pool-database-assessment-powershell.md) (Powershell-script voor het identificeren van databases die geschikt zijn voor een pool voor Elastic Database).

| Mogelijkheid | Portalervaring | PowerShell-script |
|:--- |:--- |:--- |
| Granulariteit |15 seconden |15 seconden |
| Houdt rekening met prijsverschillen tussen een groep en prestatieniveaus voor individuele databases |Ja |Nee |
| Hierbij kan de lijst van geanalyseerde databases worden aangepast |Ja |Ja |
| Hierbij kan de periode die in de analyse wordt gebruikt, worden aangepast |Nee |Ja |
| Hierbij kan de lijst met databases die over verschillende servers worden geanalyseerd, worden aangepast |Nee |Ja |
| Hierbij kan de lijst met databases die op v11-servers worden geanalyseerd, worden aangepast |Nee |Ja |

Als u het hulpprogramma niet kunt gebruiken, kunnen de volgende stappen u helpen om te schatten of een groep rendabeler is dan individuele databases:

1. Maak als volgt een schatting van het aantal eDTU's dat nodig is voor de groep:
   
   MAX(<*Totaal aantal DB's* X *gemiddelde DTU-gebruik per DB*>,<br>
   <*aantal gelijktijdig piekende databases* X *piek-DTU-gebruik per DB*)
2. Schat hoeveel opslagruimte de groep nodig heeft door het aantal bytes op te tellen dat nodig is voor alle databases in de groep.  Bepaal daarna hoe groot de eDTU-groep moet zijn om aan deze hoeveelheid opslag te voldoen.  Zie [eDTU and storage limits for elastic database pools and elastic databases](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools-and-elastic-databases) (eDTU- en opslaglimieten voor pools voor Elastic Database en elastische databases) voor groepsopslaglimieten op basis van eDTU-groepsomvang.
3. Gebruik de grootste eDTU-schatting uit stap 1 en stap 2.
4. Raadpleeg de [pagina Prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/) en zoek de kleinste eDTU-groepsomvang die groter is dan de schatting van stap 3.
5. Vergelijk de prijs van de groep uit stap 5 met de prijs voor het gebruik van de juiste prestatieniveaus voor individuele databases.

## <a name="summary"></a>Samenvatting
Niet alle individuele databases zijn geschikte kandidaten voor groepen. Databases met gebruikspatronen die worden gekenmerkt door een laag gemiddeld gebruik en relatief onregelmatige gebruikspieken zijn uitstekende kandidaten. Gebruikspatronen van toepassingen zijn dynamisch. Gebruik daarom de informatie en hulpmiddelen die in dit artikel worden beschreven om een eerste beoordeling te doen om te zien of een groep een goede keuze is voor enkele of al uw databases. Dit artikel is slechts een uitgangspunt om u te helpen bij het beslissen of een elastische groep geschikt is. Denk eraan dat u het historisch resourcegebruik voortdurend moet bewaken en de prestatieniveaus van alle databases constant opnieuw moet beoordelen. Onthoud dat u databases eenvoudig van en naar elastische groepen kunt verplaatsen, en als u een zeer groot aantal databases hebt kunt u met meerdere groepen van verschillende grootten werken waarover u uw databases kunt verdelen.

## <a name="next-steps"></a>Volgende stappen
* [Create an elastic database pool](sql-database-elastic-pool-create-portal.md) (Een pool voor elastische database maken)
* [Monitor, manage, and size an elastic database pool](sql-database-elastic-pool-manage-portal.md) (Een pool voor Elastic Database bewaken, beheren en van formaat veranderen)
* [SQL Database options and performance: understand what's available in each service tier](sql-database-service-tiers.md) (Opties en prestaties van SQL Database: inzicht in wat er beschikbaar is in elke servicelaag)
* [PowerShell script for identifying databases suitable for an elastic database pool](sql-database-elastic-pool-database-assessment-powershell.md) (PowerShell-script voor het identificeren van databases die geschikt zijn voor een pool voor Elastic Database)




<!--HONumber=Dec16_HO1-->


