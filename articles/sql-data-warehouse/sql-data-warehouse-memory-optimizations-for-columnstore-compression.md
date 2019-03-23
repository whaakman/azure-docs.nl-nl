---
title: Verbeter prestaties in de columnstore-index - Azure SQL Data Warehouse | Microsoft Docs
description: Geheugenvereisten verlagen of te verhogen van het beschikbare geheugen voor het maximaliseren van het aantal rijen die een columnstore-index in elke rijgroep comprimeren.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 03/22/2019
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: e7ab09522184f5c2d1c5168b24b2948f58e5189e
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58368958"
---
# <a name="maximizing-rowgroup-quality-for-columnstore"></a>Rijgroep kwaliteit voor columnstore maximaliseren

Rijgroep kwaliteit wordt bepaald door het aantal rijen in een rijgroep. Het beschikbare geheugen te verhogen, kan het aantal rijen die een columnstore-index in elke rijgroep comprimeren maximaliseren.  Deze methoden gebruiken voor betere compressie tarieven en queryprestaties voor columnstore-indexen.

## <a name="why-the-rowgroup-size-matters"></a>Waarom de grootte van de rijgroep belangrijk is
Omdat een columnstore-index een tabel zoekt door te scannen kolomsegmenten van afzonderlijke rijgroepen, verbetert maximaliseren van het aantal rijen in elke rijgroep de prestaties van query's. Wanneer Rijgroepen een groot aantal rijen hebben, verbetert de compressie van gegevens wat inhoudt dat er minder gegevens te lezen van de schijf.

Zie voor meer informatie over Rijgroepen [gids Columnstore-indexen](https://msdn.microsoft.com/library/gg492088.aspx).

## <a name="target-size-for-rowgroups"></a>Doelgrootte berekenen voor rijgroepen
Voor de beste queryprestaties, wordt het doel is om het aantal rijen per rijgroep in een columnstore-index te maximaliseren. Een rijgroep kan maximaal 1.048.576 rijen hebben. Het is goed dat u hebt het maximum aantal rijen per rijgroep. Columnstore-indexen bereiken van goede prestaties wanneer Rijgroepen ten minste 100.000 rijen hebben.

## <a name="rowgroups-can-get-trimmed-during-compression"></a>Rijgroepen kunt ophalen afgekapt tijdens compressie

Tijdens een bulksgewijs laden of columnstore-indexen, soms er niet voldoende geheugen beschikbaar is voor alle rijen die zijn aangewezen voor elke rijgroep comprimeren. Wanneer er geheugendruk, trim columnstore-indexen de rijgroep-grootten, zodat de compressie in de columnstore kan slagen. 

Wanneer er onvoldoende geheugen aan ten minste 10.000 rijen in elke rijgroep comprimeren, wordt er een fout gegenereerd in SQL Data Warehouse.

Zie voor meer informatie over het bulksgewijs laden, [bulksgewijs laden in een geclusterde columnstore-index](https://msdn.microsoft.com/library/dn935008.aspx#Bulk ).

## <a name="how-to-monitor-rowgroup-quality"></a>Rijgroep kwaliteit controleren

De DMV-sys.dm_pdw_nodes_db_column_store_row_group_physical_stats ([sys.dm_db_column_store_row_group_physical_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql) bevat de weergavedefinitie van de die overeenkomt met de SQL-database naar SQL Data Warehouse) die nuttige informatie wordt aangegeven zoals het aantal rijen in Rijgroepen en de reden voor het trimmen als er werd bijsnijden. U kunt de volgende weergave maken als een handige manier om op te vragen van deze DMV voor informatie over het inkorten van rijgroep.

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

De trim_reason_desc geeft aan of de rijgroep is verkleind (trim_reason_desc = NO_TRIM impliceert dat er is geen inkorten en Rijengroep is van de optimale kwaliteit). De volgende redenen knippen geven voortijdige inkorten van de rijgroep:
- BULKLOAD: Knippen om die reden wordt gebruikt wanneer de binnenkomende batch rijen voor het laden van minder dan 1 miljoen rijen heeft. De engine maakt gecomprimeerde Rijgroepen nodig als er meer dan 100.000 rijen worden ingevoegd (in plaats van in de store delta invoegen), maar wordt de beperkende reden ingesteld op BULKLOAD. In dit scenario kunt u toenemende uw batch-belasting om op te nemen meer rijen. Evalueer ook uw partitieschema om te controleren of dat dit is niet te gedetailleerde omdat Rijgroepen kunnen niet de grenzen van partities omvatten.
- MEMORY_LIMITATION: Een bepaalde hoeveelheid werkgeheugen is vereist voor het maken van Rijgroepen met 1 miljoen rijen, door de engine. Wanneer het beschikbare geheugen van de sessie laden is kleiner dan het vereiste geheugen van de werkende, krijgen voortijdig Rijgroepen bijgesneden. In de volgende secties wordt uitgelegd hoe u schat geheugen die nodig is en meer geheugen toewijzen.
- DICTIONARY_SIZE: Knippen daarom geeft aan dat rijgroep trimming opgetreden omdat er ten minste één kolom met tekenreeksen met brede en/of hoge kardinaliteit tekenreeksen. Grootte van de bibliotheek is beperkt tot 16 MB in het geheugen en zodra deze limiet is bereikt de rijgroep is gecomprimeerd. Als u in deze situatie uitvoert, kunt u overwegen de problematische kolom in een afzonderlijke tabel isoleren.

## <a name="how-to-estimate-memory-requirements"></a>Hoe u schat geheugenvereisten

<!--
To view an estimate of the memory requirements to compress a rowgroup of maximum size into a columnstore index, download and run the view [dbo.vCS_mon_mem_grant](). This view shows the size of the memory grant that a rowgroup requires for compression in to the columnstore.
-->

De maximale vereiste hoeveelheid geheugen voor het comprimeren van één rijgroep is ongeveer

- 72 MB +
- \#rijen \* \#kolommen \* 8 bytes +
- \#rijen \* \#korte tekenreekskolommen \* 32 bytes +
- \#lange tekenreekskolommen \* 16 MB voor compressie woordenlijst

waarbij de gegevenstypen string van gebruik maken van korte-tekenreeks-kolommen < = 32 bytes en gebruik van lange tekenreekskolommen tekenreeks gegevenstypen > 32 bytes.

Lange tekenreeksen worden gecomprimeerd met een compressiemethode die is ontworpen voor het comprimeren van tekst. Deze compressiemethode maakt gebruik van een *woordenlijst* voor het opslaan van patronen. De maximale grootte van een woordenlijst is 16 MB. Er is slechts één woordenlijst voor elke kolom lange tekenreeks in de rijgroep.

Zie de video voor een uitgebreide beschrijving van de geheugenvereisten columnstore [Azure SQL Data Warehouse schalen: configuratie en richtlijnen](https://channel9.msdn.com/Events/Ignite/2016/BRK3291).

## <a name="ways-to-reduce-memory-requirements"></a>Manieren om te beperken geheugenvereisten

De volgende technieken gebruiken om te beperken van de geheugenvereisten voor het comprimeren van Rijgroepen in columnstore-indexen.

### <a name="use-fewer-columns"></a>Minder kolommen gebruiken
Ontwerp, indien mogelijk, de tabel met minder kolommen. Wanneer een rijgroep is in de columnstore gecomprimeerd, de columnstore-index elke kolomsegment afzonderlijk gecomprimeerd. Daarom de geheugenvereisten voor het comprimeren van een rijgroep worden groter naarmate het aantal kolommen toeneemt.


### <a name="use-fewer-string-columns"></a>Minder tekenreekskolommen gebruiken
Kolommen van de gegevenstypen string vereisen meer geheugen dan numerieke en datum-gegevenstypen. Als u wilt verkleinen geheugenvereisten, houd rekening met verwijderen van tekenreekskolommen in feitentabellen en u ze in een kleinere dimensietabellen.

Extra geheugen nodig voor de compressie van de tekenreeks:

- De gegevenstypen String maximaal 32 tekens kunnen 32 aanvullende bytes per waarde vereisen.
- De gegevenstypen String met meer dan 32 tekens worden gecomprimeerd met behulp van methoden van de woordenlijst.  Elke kolom in de rijgroep kan maximaal 16 MB extra vereisen voor het bouwen van de woordenlijst.

### <a name="avoid-over-partitioning"></a>Voorkomen dat te veel partities

Columnstore-indexen maken van een of meer Rijgroepen per partitie. In SQL Data Warehouse toeneemt het aantal partities snel omdat de gegevens worden gedistribueerd en elke distributie is gepartitioneerd. Als de tabel te veel partities heeft, er mogelijk niet genoeg rijen om in te vullen van de Rijgroepen. Het ontbreken van rijen maakt geen geheugendruk tijdens compressie, maar dit leidt tot Rijgroepen die de beste prestaties van de columnstore-query kan niet worden bereikt.

Een andere reden om te voorkomen dat te veel partities is er een geheugen overhead voor het laden van rijen in een columnstore-index in een gepartitioneerde tabel. Tijdens een belasting kunnen veel partities ontvangen van de binnenkomende rijen, die in het geheugen worden bewaard totdat elke partitie onvoldoende rijen heeft moeten worden gecomprimeerd. Aanvullende geheugendruk te veel partities worden gemaakt.

### <a name="simplify-the-load-query"></a>Vereenvoudig de query laden

De database deelt de geheugentoekenning voor een query uit alle operators in de query. Wanneer een query load ingewikkeld gesorteerd en joins heeft, wordt het geheugen dat beschikbaar is voor de compressie verlaagd.

Ontwerp van de load-query om u te richten op het laden van de query. Als u uitvoeren van transformaties op de gegevens wilt, los van de load-query uitvoeren. Bijvoorbeeld, fase van de gegevens in een heap-tabel, de transformaties uitvoeren en vervolgens de tijdelijke tabel in de columnstore-index te laden. U kunt ook eerst de gegevens te laden en vervolgens de MPP-systeem gebruiken om de gegevens te transformeren.

### <a name="adjust-maxdop"></a>MAXDOP aanpassen

Elke distributie comprimeert Rijgroepen in de columnstore parallel wanneer er meer dan één CPU-kern beschikbaar per distributiepunt. De parallelle uitvoering vereist extra geheugenbronnen, wat tot geheugendruk en rijgroep inkorten leiden kunnen.

Als u wilt verkleinen geheugendruk, kunt u de queryhint MAXDOP om af te dwingen de bewerking voor het laden worden uitgevoerd in seriële modus binnen elke distributie.

```sql
CREATE TABLE MyFactSalesQuota
WITH (DISTRIBUTION = ROUND_ROBIN)
AS SELECT * FROM FactSalesQuota
OPTION (MAXDOP 1);
```

## <a name="ways-to-allocate-more-memory"></a>Manieren om meer geheugen toewijzen

DWU-grootte en de gebruiker resourceklasse samen bepalen hoeveel geheugen beschikbaar is voor de aanvraag voor een gebruiker. Om te vergroten de geheugentoekenning voor een load-query, kunt u het aantal dwu's verhogen of verhogen van de resourceklasse.

- Als u wilt vergroten de dwu's, Zie [hoe schalen prestaties?](quickstart-scale-compute-portal.md)
- De resourceklasse van een query, Zie [wijzigen van een voorbeeld van een gebruiker resource klasse](resource-classes-for-workload-management.md#change-a-users-resource-class).

## <a name="next-steps"></a>Volgende stappen

Meer manieren voor het verbeteren van de prestaties van SQL Data Warehouse, Zie de [prestatieoverzicht](sql-data-warehouse-overview-manage-user-queries.md).

