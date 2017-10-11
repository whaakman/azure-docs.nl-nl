---
title: Columnstore-index de prestaties verbeteren in Azure SQL | Microsoft Docs
description: Verminder de geheugenvereisten of vergroot de hoeveelheid beschikbaar geheugen om het aantal rijen comprimeren van een columnstore-index in elke rijgroep maximaliseren.
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: ef170f39-ae24-4b04-af76-53bb4c4d16d3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 6/2/2017
ms.author: shigu;barbkess
ms.openlocfilehash: f0e0b839b4a0c216eee2eb5134d43b91d8f83289
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="maximizing-rowgroup-quality-for-columnstore"></a>Het optimaliseren van rijgroep kwaliteit voor columnstore

Rijgroep kwaliteit wordt bepaald door het aantal rijen in een rijgroep. Verminder de geheugenvereisten of vergroot de hoeveelheid beschikbaar geheugen om het aantal rijen comprimeren van een columnstore-index in elke rijgroep maximaliseren.  Deze methoden gebruiken compressie tarieven verbeteren en prestaties voor columnstore-indexen opvragen.

## <a name="why-the-rowgroup-size-matters"></a>Waarom de rijgroep-grootte van belang is
Omdat een columnstore-index een tabel scant door kolomsegmenten van afzonderlijke rowgroups scannen, verbetert het optimaliseren van het aantal rijen in elke rijgroep de prestaties van query's. Wanneer rowgroups hebt een groot aantal rijen, verbetert wat betekent dat er minder gegevens te lezen van de schijf is gegevenscompressie de.

Zie voor meer informatie over rowgroups [Columnstore-indexen handleiding](https://msdn.microsoft.com/library/gg492088.aspx).

## <a name="target-size-for-rowgroups"></a>Doelgrootte voor rowgroups
Voor optimale queryprestaties is het doel om te maximaliseren van het aantal rijen per rijgroep in een columnstore-index. Een rijgroep kan een maximum van 1.048.576 rijen hebben. Dit klopt niet het maximum aantal rijen per rijgroep hebben. Columnstore-indexen bereiken van goede prestaties wanneer rowgroups ten minste 100.000 rijen hebben.

## <a name="rowgroups-can-get-trimmed-during-compression"></a>Rowgroups kunt ophalen afgekapte tijdens compressie

Tijdens het bulksgewijs laden of columnstore-index opnieuw opbouwen is soms er onvoldoende geheugen beschikbaar is voor het comprimeren van de rijen die zijn aangewezen voor elke rijgroep. Wanneer er geheugendruk, trim columnstore-indexen de grootten rijgroep compressie in de columnstore kan wel. 

Wanneer er onvoldoende geheugen voor het comprimeren van minimaal 10.000 rijen in elke rijgroep, wordt er een fout gegenereerd in SQL Data Warehouse.

Zie voor meer informatie over bulksgewijs laden [bulksgewijs laden in een geclusterde columnstore-index](https://msdn.microsoft.com/en-us/library/dn935008.aspx#Bulk load into a clustered columnstore index).

## <a name="how-to-monitor-rowgroup-quality"></a>Het bewaken van rijgroep kwaliteit

Er is een DMV (sys.dm_pdw_nodes_db_column_store_row_group_physical_stats) die nuttige informatie zoals het aantal rijen in rowgroups en de reden voor bijsnijden als er was u beschikbaar maakt. U kunt de volgende weergave maken als een handige manier om op te vragen deze DMV om informatie over hoe rijgroep worden ingekort.

```sql
create view dbo.vCS_rg_physical_stats
as 
with cte
as
(
select   tb.[name]                    AS [logical_table_name]
,        rg.[row_group_id]            AS [row_group_id]
,        rg.[state]                   AS [state]
,        rg.[state_desc]              AS [state_desc]
,        rg.[total_rows]              AS [total_rows]
,        rg.[trim_reason_desc]        AS trim_reason_desc
,        mp.[physical_name]           AS physical_name
FROM    sys.[schemas] sm
JOIN    sys.[tables] tb               ON  sm.[schema_id]          = tb.[schema_id]                             
JOIN    sys.[pdw_table_mappings] mp   ON  tb.[object_id]          = mp.[object_id]
JOIN    sys.[pdw_nodes_tables] nt     ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[dm_pdw_nodes_db_column_store_row_group_physical_stats] rg      ON  rg.[object_id]     = nt.[object_id]
                                                                            AND rg.[pdw_node_id]   = nt.[pdw_node_id]
                                        AND rg.[distribution_id]    = nt.[distribution_id]                                          
)
select *
from cte;
```

De trim_reason_desc geeft aan of de rijgroep is verkleind (trim_reason_desc = NO_TRIM impliceert er is geen inkorten en rijgroep van optimale kwaliteit is). De volgende redenen voor het vrijmaken van opslagruimte voortijdige worden ingekort door de rijgroep aangeven:
- BULKLOAD: Deze trim reden wordt gebruikt wanneer de binnenkomende batch rijen voor de belasting van minder dan 1 miljoen rijen heeft. De engine voor het gecomprimeerde Rijgroepen maakt als er meer dan 100.000 rijen worden ingevoegd (in plaats van in het archief delta invoegen), maar stelt de beperkende reden op BULKLOAD. In dit scenario kunt u uw venster voor het laden van batch opzij meer rijen verhogen. Herzie ook uw partitieschema om te controleren of dat het is niet te gedetailleerde zoals Rijgroepen niet meerdere grenzen van partities omvatten.
- MEMORY_LIMITATION: Rijgroepen maakt met 1 miljoen rijen, een bepaalde hoeveelheid werkende geheugen is vereist door de engine. Wanneer het beschikbare geheugen van de sessie laden kleiner is dan het vereiste werkende geheugen, krijgen voortijdig Rijgroepen bijgesneden. De volgende secties wordt uitgelegd hoe schatten geheugen vereist en meer geheugen toewijzen.
- DICTIONARY_SIZE: Voor deze trim reden geeft aan dat rijgroep bijsnijden omdat er ten minste één kolom met tekenreeksen met een hoge en/of hoge kardinaliteit tekenreeks is opgetreden. Grootte van de bibliotheek is beperkt tot 16 MB in het geheugen en als deze limiet is bereikt de rijgroep is gecomprimeerd. Als u in deze situatie uitvoert, kunt u overwegen de problematisch kolom in een afzonderlijke tabel isoleren.

## <a name="how-to-estimate-memory-requirements"></a>Hoe u schat geheugenvereisten

<!--
To view an estimate of the memory requirements to compress a rowgroup of maximum size into a columnstore index, download and run the view [dbo.vCS_mon_mem_grant](). This view shows the size of the memory grant that a rowgroup requires for compression in to the columnstore.
-->

De maximale hoeveelheid vereiste geheugen moeten worden gecomprimeerd één rijgroep is ongeveer

- 72 MB +
- \#rijen \* \#kolommen \* 8 bytes +
- \#rijen \* \#korte tekenreekskolommen \* 32 bytes +
- \#lange tekenreekskolommen \* 16 MB voor compressiebibliotheek

waar korte tekenreekskolommen gebruiken voor de gegevenstypen string van < = 32 bytes en het gebruik van lange tekenreekskolommen tekenreeks gegevenstypen > 32 bytes.

Lange tekenreeksen zijn gecomprimeerd met een compressiemethode die is ontworpen voor het comprimeren van tekst. Deze compressiemethode gebruikt een *woordenlijst* voor het opslaan van tekstpatronen. De maximale grootte van een woordenboek is 16 MB. Er is slechts één woordenlijst voor elke kolom lange tekenreeks in de rijgroep.

Zie de video voor een gedetailleerdere beschrijving van de geheugenvereisten columnstore [Azure SQL Data Warehouse schalen: configuratie en richtlijnen](https://myignite.microsoft.com/videos/14822).

## <a name="ways-to-reduce-memory-requirements"></a>Manieren om te verminderen geheugenvereisten

De volgende technieken gebruiken om te beperken van de geheugenvereisten voor rowgroups comprimeren en columnstore-indexen.

### <a name="use-fewer-columns"></a>Gebruik minder kolommen
Ontwerp, indien mogelijk, de tabel met minder kolommen. Wanneer een rijgroep in de columnstore is gecomprimeerd, de columnstore-index elk kolomsegment afzonderlijk gecomprimeerd. Daarom is de geheugenvereisten voor het comprimeren van een rijgroep verhogen als het aantal kolommen toeneemt.


### <a name="use-fewer-string-columns"></a>Gebruik minder tekenreekskolommen
Kolommen van de gegevenstypen string vereisen meer geheugen dan numerieke en datum-gegevenstypen. Overweeg om te beperken geheugenvereisten, tekenreekskolommen verwijderen uit feitentabellen en ze in kleinere dimensietabellen te plaatsen.

Extra geheugen nodig voor de compressie van de tekenreeks:

- De gegevenstypen String maximaal 32 tekens kunnen 32 aanvullende bytes per waarde vereisen.
- De gegevenstypen String met meer dan 32 tekens zijn gecomprimeerd met behulp van methoden van de woordenlijst.  Elke kolom in de rijgroep kan maximaal 16 MB extra vereisen voor het bouwen van de woordenlijst.

### <a name="avoid-over-partitioning"></a>Te veel partitioneren voorkomen

Columnstore-indexen maken een of meer rowgroups per partitie. In SQL Data Warehouse groeit het aantal partities snel omdat de gegevens wordt gedistribueerd en elke distributie is gepartitioneerd. Als de tabel te veel partities heeft, er mogelijk niet voldoende rijen voor het vervullen van de rowgroups. Het ontbreken van rijen maakt geen geheugendruk tijdens compressie, maar dit leidt tot rowgroups die de beste prestaties van de columnstore-query kan niet worden bereikt.

Een andere reden om te voorkomen dat te veel partitionering is er een geheugen overhead voor het laden van rijen in een columnstore-index in een gepartitioneerde tabel. Tijdens een werklast kunnen veel partities ontvangen de binnenkomende rijen die in het geheugen worden bewaard totdat elke partitie onvoldoende rijen heeft moeten worden gecomprimeerd. Aanvullende geheugendruk met te veel partities worden gemaakt.

### <a name="simplify-the-load-query"></a>Vereenvoudig de query laden

De database deelt de geheugentoekenning voor een query tussen de operators in de query. Wanneer een query load complexe sorteerbewerkingen en joins heeft, wordt het geheugen dat beschikbaar is voor de compressie verlaagd.

Ontwerp van de load-query om zich te richten alleen voor het laden van de query. Als u wilt de transformaties uitvoeren op de gegevens, los van de load-query uitvoeren. Bijvoorbeeld, fase van de gegevens in een tabel heap, de transformaties uitvoeren en vervolgens de faseringstabel in de columnstore-index te laden. U kunt ook de gegevens eerst laden en gebruik vervolgens de MPP-systeem om de gegevens te transformeren.

### <a name="adjust-maxdop"></a>MAXDOP aanpassen

Elk distributiepunt comprimeert rowgroups in de columnstore parallel wanneer er meer dan één CPU-kern beschikbaar per distributiepunt. De parallelle uitvoering vereist extra geheugenbronnen, wat tot geheugendruk en rijgroep bijsnijden leiden kunnen.

Als u geheugendruk, kunt u de query MAXDOP hint om af te dwingen de laadbewerking worden uitgevoerd in seriële modus binnen elk distributiepunt.

```
CREATE TABLE MyFactSalesQuota
WITH (DISTRIBUTION = ROUND_ROBIN)
AS SELECT * FROM FactSalesQUota
OPTION (MAXDOP 1);
```

## <a name="ways-to-allocate-more-memory"></a>Manieren om meer geheugen toewijzen

DWU-grootte en de resource gebruikersklasse samen bepalen hoeveel geheugen beschikbaar is voor de aanvraag voor een gebruiker. Voor een verhoging van de geheugentoekenning voor een load-query kunt u Verhoog het aantal dwu's of vergroot de bronklasse.

- Zie voor een verhoging van het aantal dwu's [hoe schalen prestaties?](sql-data-warehouse-manage-compute-overview.md#scale-compute)
- Zie het wijzigen van de bronklasse voor een query [wijzigen van een voorbeeld van een gebruiker resource klasse](sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).

Bijvoorbeeld op DWU 100 kunt een gebruiker in de bronklasse smallrc 100 MB aan geheugen voor elk distributiepunt. Zie voor meer informatie, [gelijktijdigheid in SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md).

Stel dat u vastgesteld dat er 700 MB geheugen voor het ophalen van hoge kwaliteit rijgroep grootten. Deze voorbeelden laten zien hoe u de load-query kunt uitvoeren met onvoldoende geheugen.

- DWU-1000 en mediumrc gebruikt, is uw geheugentoekenning 800 MB
- DWU 600 en largerc gebruikt, is uw geheugentoekenning 800 MB.


## <a name="next-steps"></a>Volgende stappen

Ga voor meer manieren om de prestaties verbeteren in SQL Data Warehouse, Zie de [prestatieoverzicht](sql-data-warehouse-overview-manage-user-queries.md).

<!--Image references-->

<!--Article references-->


<!--MSDN references-->

<!--Other Web references-->
