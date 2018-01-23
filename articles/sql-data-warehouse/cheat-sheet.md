---
title: Blad cheats voor Azure SQL Data Warehouse | Microsoft Docs
description: Vinden van koppelingen en best practices snel uw Azure SQL Data Warehouse om oplossingen te maken.
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
ms.openlocfilehash: a16c2230c26865913285cb8cbd5b0f81426acdd1
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="cheat-sheet-for-azure-sql-data-warehouse"></a>Blad cheats voor Azure SQL Data Warehouse
Deze referentieoverzicht biedt nuttige tips en aanbevolen procedures voor het bouwen van uw Azure SQL Data Warehouse-oplossingen. Voordat u begint, meer informatie over elke stap in detail door te lezen [antivirusprogramma patronen met een Azure SQL Data Warehouse werkbelasting patronen en](https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/azure-sql-data-warehouse-workload-patterns-and-anti-patterns), waarin wordt uitgelegd wat SQL Data Warehouse is en wat niet.

De volgende afbeelding ziet u het proces van het ontwerpen van een datawarehouse:

![Schema]

## <a name="queries-and-operations-across-tables"></a>Query's en bewerkingen op tabellen

Als u weet op voorhand voor de primaire bewerkingen en query's kunnen worden uitgevoerd in uw datawarehouse, kunt u uw datawarehouse-architectuur voor deze bewerkingen kunt prioriteiten aanbrengen. Deze query's en bewerkingen kunnen onder andere:
* Lid worden van een of twee feitentabellen met dimensietabellen, de gecombineerde tabel filteren en de resultaten vervolgens toe te voegen in een datamart.
* Grote of kleine updates aan te brengen in uw verkoop feit.
* Alleen gegevens toevoegt aan uw tabellen.

De typen bewerkingen van tevoren weten, kunt u het ontwerp van uw tabellen te optimaliseren.

## <a name="data-migration"></a>Gegevensmigratie

Uw gegevens in eerst laden [Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-data-lake-store) of Azure Blob-opslag. Gebruik vervolgens PolyBase uw gegevens laadt in SQL Data Warehouse in een tijdelijke tabel. Gebruik de volgende configuratie:

| Ontwerpen | Aanbeveling |
|:--- |:--- |
| Distributie | Round robin |
| Indexeren | Object-heap |
| Partitionering | Geen |
| Bronklasse | largerc of xlargerc |

Meer informatie over [gegevensmigratie], [gegevens laden], en de [extraheren, laden en transformeren (ELT) proces](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/design-elt-data-loading). 

## <a name="distributed-or-replicated-tables"></a>Gedistribueerde of gerepliceerde tabellen

Gebruik de volgende strategieën, afhankelijk van de eigenschappen van de tabel:

| Type | Goede geschikt is voor...| Als u uitkijken...|
|:--- |:--- |:--- |
| Gerepliceerd | • Kleine dimensietabellen in een sterschema met minder dan 2 GB aan opslagruimte na compression (compressie ~ 5 x) |• Veel transacties schrijven zijn voor tabel (zoals invoegen, upsert, verwijderen, update)<br></br>• U Data Warehouse Units (DWU) vaak inrichting wijzigen<br></br>• U 2-3-kolommen alleen maar de tabel heeft veel kolommen<br></br>• Index van een gerepliceerde tabel |
| Round Robin (standaard) | • Tijdelijke/faseringstabel<br></br> • Niet duidelijk sleutel of een goede kandidaat kolom toevoegen |• Prestaties vanwege een gegevensverplaatsing traag is |
| Hash | • Feitentabellen<br></br>• Grote dimensietabellen |• De distributiesleutel kan niet worden bijgewerkt |

**Tips:**
* Beginnen met Round-Robin, maar aspire naar een hash-distributiestrategie om te profiteren van een massively parallelle architectuur.
* Zorg ervoor dat algemene hash-sleutels dezelfde gegevensindeling hebben.
* Niet distribueren op varchar-indeling.
* Dimensietabellen met een algemene hash-sleutel naar een feitentabel met regelmatige join-bewerkingen kunnen worden gedistribueerd hash.
* Gebruik  *[sys.dm_pdw_nodes_db_partition_stats]*  asymmetrisch in de gegevens te analyseren.
* Gebruik  *[sys.dm_pdw_request_steps]*  om gegevens te analyseren verplaatsingen achter query's, de tijd broadcast bewaken en bewerkingen in willekeurige volgorde. Dit is handig om te controleren van uw distributiestrategie voor.

Meer informatie over [gerepliceerde tabellen] en [tabellen gedistribueerd].

## <a name="index-your-table"></a>Index van de tabel

Indexeren is handig voor het lezen van tabellen snel. Er is een unieke reeks technologieën waarmee u kunt op basis van uw behoeften:

| Type | Goede geschikt is voor... | Als u uitkijken...|
|:--- |:--- |:--- |
| Object-heap | • Fasering/tijdelijke tabel<br></br>• Kleine tabellen met kleine zoekacties |• Een lookup scant de volledige tabel |
| Geclusterde index | • Tabellen met maximaal 100 miljoen rijen<br></br>• Grote tabellen (meer dan 100 miljoen rijen) met slechts 1-2 kolommen intensief wordt gebruikt |• Gebruikt in een gerepliceerde tabel<br></br>• Hebt u complexe query's met betrekking tot meerdere join en Group By-bewerkingen<br></br>• U updates op de geïndexeerde kolommen: wordt geheugen |
| Geclusterde columnstore-index (GVI) (standaard) | • Grote tabellen (meer dan 100 miljoen rijen) | • Gebruikt in een gerepliceerde tabel<br></br>• U grootschalige bewerkingen voor uw tabel bijwerken<br></br>• U uw tabel overpartition: Rijgroepen niet verdeeld over via een ander distributiepunt knooppunten en partities |

**Tips:**
* Mogelijk wilt u een niet-geclusterde index toevoegen aan een kolom die veel worden gebruikt voor het filteren van boven op een geclusterde index. 
* Wees voorzichtig hoe u het geheugen van een tabel met CCI beheren. Wanneer u gegevens laadt, wilt u de gebruiker (of de query) om te profiteren van een grote resourceklasse. Zorg ervoor dat bijsnijden en het maken van veel kleine gecomprimeerde Rijgroepen voorkomen.
* Geoptimaliseerd voor Compute laag rotsen met CCI.
* Voor CCI, trage prestaties kan het gevolg slechte compressie van de Rijgroepen. Als dit het geval is, opnieuw samenstellen of ordenen van uw CCI. Wilt u ten minste 100.000 rijen per gecomprimeerde Rijgroepen. De ideale is 1 miljoen rijen in een rijgroep.
* Op basis van de frequentie van incrementele belasting en grootte, die u wilt automatiseren wanneer u opnieuw indelen of uw indexen opnieuw samenstellen. Spring reinigen is altijd handig.
* Als u wilt knippen van een rijgroep, worden strategische. Hoe groot zijn de open rijgroepen? Hoeveel gegevens verwacht u in de komende dagen laden?

Meer informatie over [indexen].

## <a name="partitioning"></a>Partitionering
U kunt uw tabel partitioneren wanneer er een grote feitentabel (groter dan 1 miljard rijen). De partitiesleutel in 99 procent van de gevallen, moet worden gebaseerd op datum. Zorg ervoor dat geen overpartition, vooral wanneer u een geclusterde columnstore-index hebt.

Met tabellen waarvoor ELT fasering, kunt u profiteren van partitioneren. Het vergemakkelijkt beheer van levenscyclus.
Zorg ervoor dat u niet overpartition van uw gegevens, vooral op een geclusterde columnstore-index.

Meer informatie over [partities].

## <a name="incremental-load"></a>Incrementele laden

Als u uw gegevens stapsgewijs laden, moet u eerst controleren of groter resource klassen voor het laden van uw gegevens toe te wijzen. U kunt het beste PolyBase en ADF V2 gebruikt voor het automatiseren van uw pijplijnen ELT in SQL Data Warehouse.

Voor een grote batch-updates in uw historische gegevens, moet u eerst de betrokken gegevens verwijderen. Stelt u een bulksgewijs invoegen van de nieuwe gegevens. Deze benadering in twee stappen is efficiënter.

## <a name="maintain-statistics"></a>Statistieken bijhouden
 Tot automatisch statistieken over het algemeen beschikbaar zijn, moet SQL Data Warehouse handmatige onderhoudswerkzaamheden van statistieken. Het is belangrijk om bij te werken van statistieken als *aanzienlijke* wijzigingen optreden in uw gegevens. Dit helpt uw query's optimaliseren. Als u het onderhouden van de statistieken te lang duurt vindt, worden meer selectieve over welke kolommen statistieken hebben. 

U kunt ook de frequentie van de updates definiëren. U wilt bijwerken datumkolommen, waarbij nieuwe waarden kunnen worden toegevoegd, per dag. De meeste benefit krijgt u statistieken over voor kolommen die betrokken zijn in joins, kolommen die worden gebruikt in de component WHERE en kolommen gevonden in de GROUP BY.

Meer informatie over [statistieken].

## <a name="resource-class"></a>Bronklasse
SQL Data Warehouse gebruikt resourcegroepen om geheugen toe te wijzen aan query’s. Als u meer geheugen voor het verbeteren van de query of het laden van de snelheid, kunt u hogere resource klassen moet toewijzen. De zijde spiegelen met grotere resource klassen heeft gevolgen voor gelijktijdigheid van taken. U wilt dat in aanmerking te nemen voordat u alle gebruikers naar een grote resourceklasse.

Als u merkt dat query's te lang duurt, controleert u dat uw gebruikers niet in grote resource klassen worden uitgevoerd. Grote resource klassen verbruiken veel gelijktijdigheid sleuven. Ze kunnen leiden tot andere query's naar de wachtrij.

Ten slotte opgehaald elke bronklasse met behulp van de laag geoptimaliseerd Compute 2,5 keer zoveel geheugen op de elastische geoptimaliseerd laag.

Meer informatie over het werken met [resource klassen en gelijktijdigheid].

## <a name="lower-your-cost"></a>Uw kosten verlagen
Een belangrijke functie van SQL Data Warehouse is de mogelijkheid om te pauzeren wanneer je niet gebruikt, die de facturering van rekenresources stopt. Een andere kernfunctie is het schalen van resources. Het onderbreken en schalen kunnen worden gedaan via de Azure-portal of via PowerShell-opdrachten.

Automatisch schalen die nu op het moment dat u wilt met Azure Functions:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwTimerScaler%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

## <a name="optimize-your-architecture-for-performance"></a>De architectuur voor prestaties optimaliseren

U wordt aangeraden overwegen van een SQL-Database en Azure Analysis Services in een hub en spoke-architectuur. Deze oplossing kunt werkbelasting isolatie bieden tussen verschillende gebruikersgroepen tijdens het ook gebruik van geavanceerde beveiligingsfuncties van SQL-Database en Azure Analysis Services. Dit is ook een manier bieden oneindig gelijktijdigheid van taken aan uw gebruikers.

Meer informatie over [typische architecturen die van SQL Data Warehouse gebruikmaken](https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/common-isv-application-patterns-using-azure-sql-data-warehouse/).

Implementeren met één muisklik uw spaken in SQL-databases van SQL Data Warehouse:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwSpokeDbTemplate%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>


<!--Image references-->
[Sketch]:media/sql-data-warehouse-cheat-sheet/picture-flow.png

<!--Article references-->
[gegevens laden]:./design-elt-data-loading.md
[deeper guidance]: ./guidance-for-loading-data.md
[indexen]:./sql-data-warehouse-tables-index.md
[partities]:./sql-data-warehouse-tables-partition.md
[statistieken]:./sql-data-warehouse-tables-statistics.md
[resource klassen en gelijktijdigheid]:./sql-data-warehouse-develop-concurrency.md

<!--MSDN references-->


<!--Other Web references-->
[typical architectures that take advantage of SQL Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/common-isv-application-patterns-using-azure-sql-data-warehouse/
[is and is not]:https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/azure-sql-data-warehouse-workload-patterns-and-anti-patterns/
[gegevensmigratie]:https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
[gerepliceerde tabellen]:https://docs.microsoft.com/en-us/azure/sql-data-warehouse/design-guidance-for-replicated-tables
[tabellen gedistribueerd]:https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute
[Azure Data Lake Store]: https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-data-lake-store
[sys.dm_pdw_nodes_db_partition_stats]: https://docs.microsoft.com/en-us/sql/relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql
[sys.dm_pdw_request_steps]:https://docs.microsoft.com/en-us/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql
