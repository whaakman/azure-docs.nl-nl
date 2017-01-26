---
title: Wat is een elastische groep van Azure? | Microsoft Docs
description: Beheer honderden of duizenden databases met een groep. Een enkele prijs voor een reeks prestatie-eenheden die kan worden verdeeld over de groep. Plaats databases naar wens binnen of buiten de groep.
keywords: elastische pool, sql-databases
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: b46e7fdc-2238-4b3b-a944-8ab36c5bdb8e
ms.service: sql-database
ms.custom: multiple databases
ms.devlang: NA
ms.date: 01/11/2017
ms.author: CarlRabeler
ms.workload: data-management
ms.topic: get-started-article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 2681dd3792a351fecc0c72eb7fe546113a451d24
ms.openlocfilehash: 412c3f9c34e399ecdffd939b5b72e687a847b8e1


---
# <a name="what-is-an-azure-elastic-pool"></a>Wat is een elastische groep van Azure?
Elastische groepen met SQL-databases zijn een eenvoudige en voordelige oplossing voor het beheer van de prestatiedoelen voor databases met breed uiteenlopende en onvoorspelbare gebruikspatronen.

> [!NOTE]
> Elastische pools zijn algemeen beschikbaar in alle Azure-regio's, behalve in West-India, waar deze zich momenteel in de previewfase bevinden.  Algemene beschikbaarheid van elastische pools in deze regio zal zo snel mogelijk plaatsvinden.
>
>

## <a name="how-it-works"></a>Hoe werkt het?
Een algemeen SaaS-toepassingspatroon is het databasemodel met één tenant: elke klant krijgt zijn eigen database. Elke klant (database) heeft onvoorspelbare resourcevereisten voor geheugen, I/O en CPU. Hoe wijst u resources op efficiënte en rendabele wijze toe bij een dergelijke onvoorspelbare vraag? Vroeger had u twee opties: (1) een te royale inrichting van resources op basis van piekgebruik, waarbij u dus te veel betaalt, of (2) een te voorzichtige inrichting om kosten te besparen, ten koste van prestaties en klanttevredenheid tijdens pieken. Elastische groepen lossen dit probleem op door ervoor te zorgen dat databases de prestaties krijgen die ze nodig hebben, wanneer ze deze nodig hebben. Ze bieden een eenvoudig mechanisme voor het toewijzen van resources binnen een voorspelbaar budget. Zie [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) voor meer informatie over ontwerppatronen voor SaaS-toepassingen met elastische groepen.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Elastic-databases-helps-SaaS-developers-tame-explosive-growth/player]
>

In SQL Database wordt de relatieve mate waarin een database de vraag naar resources kan afhandelen, uitgedrukt in DTU's (Database Transaction Units) voor individuele databases en elastische DTU's (eDTU's) voor individuele databases in een elastische pool. Zie de [Inleiding tot SQL Database](sql-database-technical-overview.md) voor meer informatie over DTU's en eDTU's.

Een pool krijgt een bepaald aantal eDTU's voor een vaste prijs. Binnen de pool hebben afzonderlijke databases de flexibiliteit om de schaal automatisch aan te passen binnen ingestelde parameters. Bij zware belasting kan een database meer eDTU's verbruiken om aan de vraag te voldoen. Databases verbruiken minder bij lichte belasting en verbruiken geen eDTU's als er geen belasting is. De inrichting van resources voor de hele pool in plaats van afzonderlijke databases vereenvoudigt uw beheertaken. Bovendien hebt u een voorspelbaar budget voor de pool.

Er kunnen meer eDTU's zonder uitvaltijd van de database aan een bestaande pool worden toegevoegd. Maar de databases moeten dan mogelijk worden verplaatst om te voorzien in de extra rekenresources voor de nieuwe eDTU-reservering. En als de extra eDTU's niet meer nodig zijn, kunnen ze op elk moment uit een bestaande pool worden verwijderd.

Daarnaast kunt u databases aan de groep toevoegen of uit de groep verwijderen. Als een database naar verwachting minder resources nodig heeft, kunt u deze verwijderen.

## <a name="which-databases-go-in-a-pool"></a>Welke databases gaan in een groep?
![SQL-databases die eDTU's delen in een elastische pool.][1]

Databases die goede kandidaten zijn voor elastische groepen hebben meestal perioden van activiteit en perioden van inactiviteit. In het bovenstaande voorbeeld ziet u de activiteit van een individuele database, vier databases en ten slotte een elastische groep met twintig databases. Databases met variërende activiteit gedurende een bepaalde periode zijn goede kandidaten voor elastische groepen omdat ze niet allemaal actief zijn op hetzelfde moment en eDTU's kunnen delen. Dit patroon is niet op alle databases van toepassing. Databases met een constantere vraag naar resources zijn meer geschikt voor de servicelagen Basic, Standard en Premium, waar resources afzonderlijk worden toegewezen.

[Prijs- en prestatieoverwegingen voor een elastische groep](sql-database-elastic-pool-guidance.md).

## <a name="edtu-and-storage-limits-for-elastic-pools"></a>eDTU- en opslaglimieten voor elastische pools

De volgende tabel beschrijft de kenmerken van de Basic-, Standaard- en Premium-servicelaag voor elastische pools.

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Als alle DTU's van een elastische groep zijn gebruikt, ontvangt elke database in de groep een gelijke hoeveelheid resources om query's te verwerken.  De service SQL Database verdeelt resources eerlijk over databases door gelijke hoeveelheden rekentijd te garanderen. Gelijke verdeling van resources in een elastische groep vindt plaats naast een bepaalde hoeveelheid resources die sowieso voor elke database wordt gegarandeerd wanneer het aantal DTU's per database is ingesteld op een andere waarde dan nul.

## <a name="elastic-pool-properties"></a>Eigenschappen van elastische pools

De volgende tabellen bevatten een beschrijving van de limieten voor elastische pools en gepoolde databases.

### <a name="limits-for-elastic-pools"></a>Limieten voor elastische groepen
| Eigenschap | Beschrijving |
|:--- |:--- |
| Servicelaag |Basic, Standard of Premium. De servicelaag bepaalt het prestatiebereik en de opslaglimieten die kunnen worden geconfigureerd en de opties voor bedrijfscontinuïteit. Elke database in een groep heeft dezelfde servicelaag als de groep. Een servicelaag wordt ook wel een editie genoemd. |
| eDTU's per groep |Het maximum aantal eDTU's dat door databases in de groep kan worden gedeeld. Het totale aantal eDTU's dat door databases in de groep wordt gebruikt, kan deze limiet op een bepaald moment in de tijd niet overschrijden. |
| Maximale opslag per groep (GB) |De maximale hoeveelheid opslag in GB die door databases in de groep kan worden gedeeld. De totale opslag die door databases in de groep wordt gebruikt, kan deze limiet niet overschrijden. Deze limiet wordt bepaald door het aantal eDTU's per groep. Als deze limiet wordt overschreden, worden alle databases alleen-lezen. De maximumopslag per pool verwijst naar de maximale opslag van de gegevensbestanden in de pool. De ruimte die logboekbestanden innemen, wordt niet meegerekend. |
| Maximaal aantal databases per groep |Het maximum aantal databases dat is toegestaan per groep. |
| Maximaal aantal gelijktijdige werknemers per groep |Het maximum aantal gelijktijdige werkers (aanvragen) dat beschikbaar is voor alle databases in de groep. |
| Maximaal aantal gelijktijdige aanmeldingen per groep |Het maximum aantal gelijktijdige aanmeldingen voor alle databases in de groep. |
| Maximaal aantal gelijktijdige sessies per groep |Het maximum aantal sessies dat beschikbaar is voor alle databases in de groep. |

### <a name="limits-for-pooled-databases"></a>Limieten voor databases in pools
| Eigenschap | Beschrijving |
|:--- |:--- |
| Maximaal aantal eDTU’s per database |Het maximum aantal eDTU's dat elke database in de groep mag gebruiken, is afhankelijk van het gebruik door andere databases in de groep.  Het maximum aantal eDTU's per database is geen resourcegarantie voor een database.  Het is een algemene instelling voor alle databases in de groep. Stel het maximum aantal eDTU's per database hoog genoeg zijn om pieken in databasegebruik te kunnen afhandelen. Enige mate van overtoewijzing is normaal, omdat de groep in het algemeen uitgaat van pieken en dalen in gebruikspatronen voor databases, waarbij alle databases niet tegelijkertijd pieken. Stel, het piekgebruik per database is twintig eDTU's en slechts 20% van de 100 databases in de groep pieken op hetzelfde moment.  Als het maximum aantal eDTU's per database is ingesteld op twintig eDTU's, kan de groep redelijkerwijs vijf keer worden overtoegewezen en kan het aantal eDTU's per groep worden ingesteld op 400. |
| Minimaal aantal eDTU’s per database |Het minimum aantal eDTU's dat voor elke database in de groep wordt gegarandeerd.  Het is een algemene instelling voor alle databases in de groep. Het minimum aantal eDTU’s per database kan worden ingesteld op 0. Dat is ook de standaardwaarde. Deze eigenschap wordt ingesteld op een waarde tussen 0 en het gemiddelde eDTU-gebruik per database. Het product van het aantal databases in de groep en het minimum aantal eDTU's per database mag niet groter zijn dan het aantal eDTU's per groep.  Als een groep bijvoorbeeld twintig databases heeft en het minimum aantal eDTU's per database is ingesteld op tien eDTU's, dan moet het aantal eDTU's per groep minimaal 200 eDTU's zijn. |
| Maximale opslag per database (GB) |De maximale opslag voor een database in een groep. Databases in pools delen de poolopslag. Database-opslag is daarom beperkt tot de resterende poolopslag en de maximumopslag per database, afhankelijk van wat het kleinste is. De maximumopslag per database verwijst naar de maximale opslag van de gegevensbestanden, exclusief de ruimte die logboekbestanden innemen. |

## <a name="elastic-jobs"></a>Elastische taken
Met een groep worden beheertaken vereenvoudigd door scripts in **[elastische taken](sql-database-elastic-jobs-overview.md)** uit te voeren. Een elastische taak elimineert de meeste saaie handelingen die komen kijken bij grote aantallen databases. Zie [Getting started with Elastic Database jobs](sql-database-elastic-jobs-getting-started.md) (Aan de slag met taken voor elastische databases) om aan de slag te gaan.

Zie [Scaling out with Azure SQL Database](sql-database-elastic-scale-introduction.md) (Uitbreiden met Azure SQL Database) voor meer informatie over andere databasehulpprogramma's voor het werken met meerdere databases.

## <a name="business-continuity-features-for-databases-in-a-pool"></a>Bedrijfscontinuïteitsfuncties voor databases in een groep
Gepoolde databases ondersteunen in het algemeen dezelfde [bedrijfscontinuïteitsfuncties](sql-database-business-continuity.md) die beschikbaar zijn voor individuele databases.

### <a name="point-in-time-restore"></a>Herstel naar een bepaald tijdstip
Herstel naar een bepaald tijdstip gebruikt automatische databaseback-ups om een database in een groep te herstellen naar een bepaald herstelpunt. Zie [Herstel naar een bepaald tijdstip](sql-database-recovery-using-backups.md#point-in-time-restore)

### <a name="geo-restore"></a>Geo-herstel
Geo-herstel biedt de standaardoptie voor herstel wanneer een database niet beschikbaar is vanwege een incident in de regio waar de database wordt gehost. Zie [Restore an Azure SQL Database or failover to a secondary](sql-database-disaster-recovery.md) (Een Azure SQL Database of herstellen of een failover uitvoeren naar een secundaire server)

### <a name="active-geo-replication"></a>Actieve Geo-replicatie
Voor toepassingen die zwaardere herstelvereisten hebben dan Geo-herstel kan bieden, kunt u actieve geo-replicatie configureren door middel van [Azure Portal](sql-database-geo-replication-portal.md), [PowerShell](sql-database-geo-replication-powershell.md) of [Transact-SQL](sql-database-geo-replication-transact-sql.md).

## <a name="additional-resources"></a>Aanvullende bronnen
* [Videocursus van Microsoft Virtual Academy over de elastische mogelijkheden van Azure SQL Database](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)

<!--Image references-->
[1]: ./media/sql-database-elastic-pool/databases.png



<!--HONumber=Jan17_HO2-->


