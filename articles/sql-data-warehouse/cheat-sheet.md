---
title: Cheats blad Azure SQL Data Warehouse | Microsoft Docs
description: Vindt u koppelingen, aanbevolen procedures snel uw Azure SQL Data Warehouse om oplossingen te maken.
services: sql-data-warehouse
documentationcenter: NA
author: acomet
manager: jhubbard
editor: 
ms.assetid: 51f1e444-9ef7-4e30-9a88-598946c45196
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 12/14/2017
ms.author: acomet
ms.openlocfilehash: 2d17385ff255ddf7b85baa81600a2af60d015540
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="cheat-sheet-for-azure-sql-data-warehouse"></a>Blad cheats voor Azure SQL Data Warehouse
Deze pagina kunt u voor de belangrijkste taak uw datawarehouse-oplossing gevallen te ontwerpen. Deze referentieoverzicht moet een uitstekende ondersteuning in uw reis voor het bouwen van een hoogwaardige datawarehouse maar ten zeerste aanbevolen meer leren over elke stap in de details. Eerst wordt aangeraden deze geweldige artikel over welke SQL Data Warehouse lezen  **[is en geen]**.

Hier volgt een schema van het proces dat u volgen moet wanneer u begint met het ontwerp van SQL Data Warehouse.

![Schema]

## <a name="queries-and-operations-across-tables"></a>Query's en bewerkingen op tabellen

Het is echt belangrijk dat u van tevoren weten de belangrijkste bewerkingen en query's die plaatsvinden in uw datawarehouse. Uw datawarehouse-architectuur voorrang voor deze bewerkingen. Algemene voorbeelden van bewerkingen kunnen zijn:
* Lid worden van een of twee feitentabellen met dimensie tabellen, voor het filteren van deze tabel voor een bepaalde periode en de resultaten in een datamart toevoegen
* Maken van uw verkoop feit grote of kleine updates
* Alleen gegevens toevoegen aan uw tabellen

Weten van het type van bewerkingen helpt u bij het ontwerp van uw tabellen te optimaliseren.

## <a name="data-migration"></a>Gegevensmigratie

We raden je aan eerst zijn geladen uw gegevens  **[in ADLS]**  of Azure Blob Storage. Vervolgens gebruikt u Polybase uw gegevens laadt in SQL Data Warehouse in een tijdelijke tabel. U wordt aangeraden de volgende configuratie:

| Ontwerpen | Aanbeveling |
|:--- |:--- |
| Distributie | Round robin |
| Indexeren | Object-heap |
| Partitionering | None |
| Bronklasse | largerc of xlargerc |

Meer informatie over  **[gegevensmigratie], [gegevens laden]**  met  **[diepere richtlijnen] op laden**. 

## <a name="distributed-or-replicated-tables"></a>Gedistribueerde of gerepliceerde tabellen

U wordt aangeraden de volgende strategieën, afhankelijk van de eigenschappen van de tabel:

| Type | Goede geschikt is voor | Als u uitkijken...|
|:--- |:--- |:--- |
| Gerepliceerd | • Kleine dimensietabellen in een sterschema met minder dan 2 GB aan opslagruimte na compression (compressie ~ 5 x) |• Veel transacties voor tabel schrijven (bijvoorbeeld: invoegen, upsert, verwijderen, update)<br></br>• Data Warehouse Units (DWU) vaak inrichting wijzigen<br></br>• U alleen gebruiken 2-3 kolommen en de tabel heeft veel kolommen<br></br>• Index van een gerepliceerde tabel |
| Round Robin (standaard) | • Tijdelijke/faseringstabel<br></br> • Niet duidelijk sleutel of een goede kandidaat kolom toevoegen |• Trage prestaties vanwege een gegevensverplaatsing |
| Hash | • Feitentabellen<br></br>• Grote dimensietabellen |• De distributiesleutel kan niet worden bijgewerkt |

**Tips:**
* Beginnen met Round-Robin maar aspire voor een hash-distributiestrategie om te profiteren van een grootschalige parallelle architectuur
* Zorg ervoor dat algemene hash-sleutels dezelfde gegevensindeling hebben
* Niet distribueren op varchar-indeling
* Dimensietabellen met algemene hash-sleutel aan een feitentabel met regelmatige join-bewerkingen kunnen worden gedistribueerd hash
* Gebruik  *[sys.dm_pdw_nodes_db_partition_stats]*  asymmetrisch in de gegevens te analyseren
* Gebruik  *[sys.dm_pdw_request_steps]*  voor het analyseren van gegevens verplaatsingen achter query's, de tijd uitzending bewaken en bewerkingen in willekeurige volgorde. Dit is handig om te controleren van uw distributiestrategie voor.

Meer informatie over  **[gerepliceerde tabellen] en [tabellen gedistribueerd]**.

## <a name="indexing-your-table"></a>Indexeren van de tabel

Indexeren is **geweldige** voor het lezen van snel tabellen. Er is een unieke set van technologieën die kunt u op basis van uw behoeften:

| Type | Goede geschikt is voor | Als u uitkijken...|
|:--- |:--- |:--- |
| Object-heap | • Fasering/tijdelijke tabel<br></br>• Kleine tabellen met kleine zoekacties |• Een lookup scant de volledige tabel |
| Geclusterde Index | • Maximaal 100 miljoen rijen tabel<br></br>• Grote tabellen (meer dan 100 miljoen rijen) met slechts 1-2-kolommen intensief wordt gebruikt |• Gebruikt in een gerepliceerde tabel<br></br>• Complexe query's met betrekking tot meerdere Join, Group By operations<br></br>• Zorg updates op de geïndexeerde kolommen, duurt het geheugen |
| Geclusterde Columnstore-Index (GVI) (standaard) | • Grote tabellen (meer dan 100 miljoen rijen) | • Gebruikt in een gerepliceerde tabel<br></br>• U grootschalige bewerkingen voor uw tabel bijwerken<br></br>• De tabel te veel partitie: Rijgroepen niet verdeeld over via een ander distributiepunt knooppunten en partities |

**Tips:**
* Mogelijk wilt u niet-geclusterde Index toevoegen aan een kolom die veel worden gebruikt voor het filter voor boven op een geclusterde Index. 
* Wees voorzichtig hoe u het geheugen van een tabel met CCI beheren. Wanneer u gegevens laadt, wilt u de gebruiker (of de query) om te profiteren van een grote resourceklasse. U om te voorkomen dat bijsnijden en het maken van veel kleine gecomprimeerde Rijgroepen controleren
* Geoptimaliseerd voor Compute laag rotsen met CCI
* Voor CCI, trage prestaties kan worden veroorzaakt door slechte compressie van uw Rijgroepen raadzaam opnieuw samenstellen of ordenen van uw CCI. Wilt u ten minste 100 k rijen per gecomprimeerde Rijgroepen. De ideale is 1-m rijen in een rijgroep.
* Op basis van de frequentie van incrementele belasting en grootte, die u wilt automatiseren wanneer u opnieuw indelen of uw indexen opnieuw samenstellen. Spring reinigen is altijd handig.
* Strategische zijn wanneer u wilt knippen van een rijgroep: hoe groot de open Rijgroepen zijn? Hoeveel gegevens verwacht u in de komende dagen laden?

Meer informatie over  **[indexen]**.

## <a name="partitioning"></a>Partitionering
U kunt uw tabel partitioneren wanneer er een grote feitentabellen (> 1B rijtabel). 99% van de gevallen moet de partitiesleutel zijn gebaseerd op datum. Let niet te veel partitie, vooral wanneer u een geclusterde Columnstore-Index hebben.
Met tabellen waarvoor ETL fasering, kunt u profiteren van partitioneren. Het vergemakkelijkt beheer van levenscyclus.
Zorg ervoor dat u niet overpartition van uw gegevens, vooral op een geclusterde Columnstore-Index.

Meer informatie over  **[partities]**.

## <a name="incremental-load"></a>Incrementele laden

Eerst moet u ervoor zorgen dat u groter resource klassen toewijst voor het laden van uw gegevens. U kunt het beste Polybase en ADF V2 gebruikt voor het automatiseren van uw pijplijnen ETL in SQL DW.

Voor een grote batch van updates in uw historische gegevens, wordt u aangeraden eerst de betrokken gegevens te verwijderen. Vervolgens kunt u een bulksgewijs invoegen van de nieuwe gegevens. Deze aanpak 2 stap is het efficiënter.

## <a name="maintain-statistics"></a>Statistieken bijhouden
Automatische statistieken zijn zal binnenkort beschikbaar zijn in het algemeen. SQL Data Warehouse vereist tot handmatige onderhoudswerkzaamheden van statistieken. Het is belangrijk om bij te werken van statistieken als **aanzienlijke** wijzigingen optreden in uw gegevens. Dit helpt uw query's optimaliseren. Als u dat het duurt te lang vindt voor het onderhouden van de statistieken, wilt u mogelijk nauwkeuriger welke kolommen statistieken hebben. U kunt ook de frequentie van de updates definiëren. Zo wilt u datumkolommen, waar nieuwe gegevens kunnen zijn toegevoegd, misschien dagelijks bijwerken. De meeste benefit krijgt u statistieken over voor kolommen die zijn betrokken in joins, kolommen die worden gebruikt in de component WHERE en kolommen gevonden in de GROUP BY.

Meer informatie over  **[statistieken]**.

## <a name="resource-class"></a>Bronklasse
SQL Data Warehouse gebruikt resourcegroepen om geheugen toe te wijzen aan query’s. Als u meer geheugen voor het verbeteren van de query of het laden van de snelheid, kunt u hogere resource klassen moet toewijzen. De zijde spiegelen met grotere resource klassen heeft gevolgen voor gelijktijdigheid van taken. U wilt dat het in aanmerking te nemen voordat u alle gebruikers naar een grote resourceklasse.

Als u merkt dat query's te lang duurt, controleert u dat uw gebruikers niet in grote resource klassen worden uitgevoerd. Grote resource klassen verbruiken veel gelijktijdigheid sleuven. Ze kunnen leiden tot andere query's naar de wachtrij.

Ten slotte als u de laag Compute geoptimaliseerd, haalt elke bronklasse 2,5 x meer geheugen dan op de elastische geoptimaliseerd laag.

Meer informatie over het werken met  **[resource klassen en gelijktijdigheid]**.

## <a name="lower-your-cost"></a>Uw kosten verlagen
Een belangrijke functie van SQL Data Warehouse is de mogelijkheid om het te onderbreken wanneer u het niet gebruikt, wat voorkomt dat rekenresources in rekening worden gebracht. Een andere kernfunctie is het schalen van resources. Onderbreken en schalen kunnen via de Azure Portal of met PowerShell-opdrachten worden uitgevoerd.

Automatisch geschaald nu op het moment dat u wilt met Azure Functions:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwTimerScaler%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

## <a name="optimize-you-architecture-for-performance"></a>U architectuur voor prestaties optimaliseren

U wordt aangeraden de SQL-database en Azure Analysis Services in een Hub en spaken architectuur overwegen. Deze oplossing kunt werkbelasting isolatie bieden tussen verschillende gebruikersgroepen, terwijl sommige ook gebruik van beveiligingsfuncties van SLQ DB geavanceerde en Azure Analysis Services. Dit is ook een manier bieden oneindig gelijktijdigheid van taken aan uw gebruikers.

Meer informatie over  **[typische architecturen gebruik van SQL DW]**.

Implementeren in een klik uw spaken in SQL DB-databases van SQL DW:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwSpokeDbTemplate%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>


<!--Image references-->
[Schema]:media/sql-data-warehouse-cheat-sheet/picture-flow.png

<!--Article references-->
[gegevens laden]:./design-elt-data-loading.md
[diepere richtlijnen]: ./guidance-for-loading-data.md
[indexen]:./sql-data-warehouse-tables-index.md
[partities]:./sql-data-warehouse-tables-partition.md
[statistieken]:./sql-data-warehouse-tables-statistics.md
[resource klassen en gelijktijdigheid]:./sql-data-warehouse-develop-concurrency.md

<!--MSDN references-->


<!--Other Web references-->
[typische architecturen gebruik van SQL DW]: https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/common-isv-application-patterns-using-azure-sql-data-warehouse/
[is en geen]:https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/azure-sql-data-warehouse-workload-patterns-and-anti-patterns/
[gegevensmigratie]:https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
[gerepliceerde tabellen]:https://docs.microsoft.com/azure/sql-data-warehouse/design-guidance-for-replicated-tables
[tabellen gedistribueerd]:https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute
[in ADLS]: https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-store
[sys.dm_pdw_nodes_db_partition_stats]: https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql
[sys.dm_pdw_request_steps]:https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql
