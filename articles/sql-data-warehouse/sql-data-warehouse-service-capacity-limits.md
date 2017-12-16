---
title: SQL Data Warehouse Capaciteitslimieten | Microsoft Docs
description: Maximale waarden voor verbindingen, databases, tabellen en query's voor SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: e1eac122-baee-4200-a2ed-f38bfa0f67ce
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: reference
ms.date: 12/14/2017
ms.author: kevin;barbkess
ms.openlocfilehash: 3a8edb3806f981ebb6f8c1ca6c994ae198df2ec2
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/16/2017
---
# <a name="sql-data-warehouse-capacity-limits"></a>SQL Data Warehouse Capaciteitslimieten
De volgende tabellen bevatten de maximumwaarden die is toegestaan voor de verschillende onderdelen van Azure SQL Data Warehouse.

## <a name="workload-management"></a>Werklastbeheer
| Category | Beschrijving | Maximum |
|:--- |:--- |:--- |
| [Datawarehouse Units (DWU)][Data Warehouse Units (DWU)] |Maximale DWU voor een enkele SQL Data Warehouse | Geoptimaliseerd voor elasticiteit [prestatielaag](performance-tiers.md): DW6000<br></br>Geoptimaliseerd voor Compute [prestatielaag](performance-tiers.md): DW30000c |
| [Datawarehouse Units (DWU)][Data Warehouse Units (DWU)] |Standaard DTU per server |54,000<br></br>Elke SQL-server (bijvoorbeeld myserver.database.windows.net) heeft een DTU-quotum 54.000, waardoor maximaal DW6000c standaard. Dit quotum is gewoon een veiligheidsbeperking. U kunt uw quotum door verhogen [een ondersteuningsticket maken] [ creating a support ticket] en te selecteren *quotum* als het aanvraagtype.  Voor het berekenen van uw DTU heeft, de 7.5 vermenigvuldigen met het totale aantal dat DWU nodig of 9.0 vermenigvuldigen met de totale cDWU nodig. Bijvoorbeeld:<br></br>DW6000 x 7.5 = 45,000 dtu's<br></br>DW600c x 9.0 = 54.000 dtu's.<br></br>U kunt uw huidige DTU-verbruik van de SQL server-optie weergeven in de portal. Onderbroken en hervat databases meetellen voor de DTU-quotum. |
| Databaseverbinding |Gelijktijdige sessies actief |1024<br/><br/>Elk van de actieve sessies 1024 kunt aanvragen met een SQL Data Warehouse-database verzenden op hetzelfde moment. Let op: Er zijn limieten van het aantal query's die gelijktijdig kan worden uitgevoerd. Wanneer de gelijktijdigheid limiet wordt overschreden, gaat de aanvraag u naar een interne wachtrij waar wacht om te worden verwerkt. |
| Databaseverbinding |Maximale hoeveelheid geheugen voor voorbereide instructies |20 MB |
| [Beheer van de werkbelasting][Workload management] |Maximum aantal gelijktijdige query 's |32<br/><br/> SQL Data Warehouse kan standaard maximaal 32 gelijktijdige query's en wachtrijen resterende query's worden uitgevoerd.<br/><br/>Het aantal gelijktijdige query's kunt descrease wanneer gebruikers worden toegewezen aan hogere resource klassen of wanneer SQL Data Warehouse heeft een lagere [service level](performance-tiers.md#service-levels). Sommige query's, zoals DMV-query's, zijn altijd toegestaan om uit te voeren. |
| [TempDB][Tempdb] |Maximale GB |399 GB per DW100. Daarom op DWU1000, wordt tempdb aangepast 3,99 TB |

## <a name="database-objects"></a>database-objecten
| Category | Beschrijving | Maximum |
|:--- |:--- |:--- |
| Database |Max. grootte |240 TB gecomprimeerd op schijf<br/><br/>Deze ruimte is onafhankelijk van tempdb- of logboekbestand ruimte, en daarom deze ruimte wordt toegewezen aan permanente tabellen.  De geclusterde columnstore-compressie schatting op 5 X.  Deze compressie kan de database uitbreiden tot ongeveer 1 PB wanneer alle tabellen geclusterde columnstore (de standaard tabeltype) zijn. |
| Tabel |Max. grootte |60 TB gecomprimeerd op schijf |
| Tabel |Tabellen per database |2 miljard |
| Tabel |Kolommen per tabel |1024 kolommen |
| Tabel |Bytes per kolom |Afhankelijk van de kolom [gegevenstype][data type].  De limiet is 8000 voor de gegevenstypen char, 4000 voor nvarchar of 2 GB voor de gegevenstypen MAX. |
| Tabel |Bytes per rij, gedefinieerde grootte |8060 bytes<br/><br/>Het aantal bytes per rij wordt berekend op dezelfde manier als voor SQL Server bij de pagina compressie. Zoals SQL Server ondersteunt SQL Data Warehouse overloop rij-opslag, waardoor **kolommen met variabele lengte** moet buiten een rij worden gepusht. Wanneer de rijen met variabele lengte zijn buiten de rij gepusht, wordt alleen 24-byte-hoofdmap opgeslagen in de belangrijkste record. Zie voor meer informatie de [overloop rij gegevens van meer dan 8 KB][Row-Overflow Data Exceeding 8 KB]. |
| Tabel |Partities per tabel |15,000<br/><br/>Voor hoge prestaties raden we het nummer voor het minimaliseren van partities moet u terwijl toch op uw zakelijke vereisten. Wanneer het aantal partities groeit, wordt de overhead voor bewerkingen Data Definition Language (DDL) en gegevens manipulatie taal (DML) groeit en zorgt ervoor dat de tragere prestaties. |
| Tabel |Tekens per partitie grenswaarde. |4000 |
| Index |Niet-geclusterde indexen per tabel. |999<br/><br/>Van toepassing op alleen rowstore-tabellen. |
| Index |Geclusterde indexen per tabel. |1<br><br/>Geldt voor zowel rowstore als columnstore-tabellen. |
| Index |Indexsleutel. |900 bytes.<br/><br/>Van toepassing op slechts een rowstore-indexen.<br/><br/>Indexen voor kolommen met een maximale grootte van meer dan 900 bytes varchar kunnen worden gemaakt als de bestaande gegevens in de kolommen niet groter is dan 900 bytes wanneer de index is gemaakt. Echter, later invoegen updatebewerkingen voor de kolommen die ertoe leiden dat de totale grootte zijn dan 900 bytes, anders mislukt. |
| Index |De sleutelkolommen per index. |16<br/><br/>Van toepassing op slechts een rowstore-indexen. Geclusterde columnstore-indexen bevatten alle kolommen. |
| statistieken |Grootte van de gecombineerde kolomwaarden. |900 bytes. |
| statistieken |Kolommen per object statistieken. |32 |
| statistieken |Statistieken maken voor kolommen per tabel. |30,000 |
| Opgeslagen procedures |Maximum aantal geneste niveaus. |8 |
| Weergave |Kolommen per weergeven |1,024 |

## <a name="loads"></a>Belasting
| Category | Beschrijving | Maximum |
|:--- |:--- |:--- |
| Polybase-belasting |MB per rij |1<br/><br/>Polybase laadt alleen op rijen die kleiner zijn dan 1 MB en kunnen niet worden geladen naar VARCHAR(MAX), NVARCHAR(MAX) of VARBINARY(MAX).<br/><br/> |

## <a name="queries"></a>Query's
| Category | Beschrijving | Maximum |
|:--- |:--- |:--- |
| Query’s uitvoeren |In de wachtrij query's voor gebruikerstabellen. |1000 |
| Query’s uitvoeren |Gelijktijdige query's over systeemweergaven. |100 |
| Query’s uitvoeren |In de wachtrij-query's in system-weergaven |1000 |
| Query’s uitvoeren |Maximum aantal parameters |2098 |
| Batch |Maximale grootte |65,536*4096 |
| Selecteer resultaten |Kolommen per rij |4096<br/><br/>U kunt nooit meer dan 4096 kolommen per rij in de SELECT resultaat hebben. Er is geen garantie dat u kunt altijd 4096 hebben. Als het queryplan is een tijdelijke tabel vereist, kunnen de 1024 kolommen per tabel maximale toepassen. |
| SELECTEER |Geneste subquery 's |32<br/><br/>U kunt nooit meer dan 32 geneste subquery's in een SELECT-instructie hebben. Er is geen garantie dat u kunt altijd 32 hebben. Een JOIN kan bijvoorbeeld een subquery in het queryplan introduceren. Het aantal subquery's kan ook worden beperkt door het beschikbare geheugen. |
| SELECTEER |Kolommen per JOIN |1024 kolommen<br/><br/>U kunt nooit meer dan 1024 kolommen hebben in de JOIN. Er is geen garantie dat u kunt altijd 1024 hebben. Als het plan JOIN een tijdelijke tabel met meer kolommen dan de JOIN-resultaat vereist, geldt de 1024-limiet voor de tijdelijke tabel. |
| SELECTEER |Aantal bytes per GROEPEREN op kolommen. |8060<br/><br/>De kolommen in de component GROUP BY kunnen maximaal 8060 bytes hebben. |
| SELECTEER |Aantal bytes per ORDER BY kolommen |8060 bytes.<br/><br/>De kolommen in de component ORDER BY kunnen maximaal 8060 bytes hebben. |
| Id's en constanten per instructie |Het aantal waarnaar wordt verwezen id's en constanten. |65,535<br/><br/>SQL Data Warehouse beperkt het aantal id's en constanten die kunnen worden opgenomen in één expressie van een query. Deze limiet is 65.535. Dit nummer resulteert in een SQL Server-fout 8632 overschrijdt. Zie voor meer informatie [interne fout: een expressie services limiet is bereikt][Internal error: An expression services limit has been reached]. |

## <a name="metadata"></a>Metagegevens
| Het systeemweergave | Maximumaantal rijen |
|:--- |:--- |
| sys.dm_pdw_component_health_alerts |10.000 |
| sys.dm_pdw_dms_cores |100 |
| sys.dm_pdw_dms_workers |Totaal aantal werknemers voor de meest recente 1000 DMS SQL-aanvragen. |
| sys.dm_pdw_errors |10.000 |
| sys.dm_pdw_exec_requests |10.000 |
| sys.dm_pdw_exec_sessions |10.000 |
| sys.dm_pdw_request_steps |Totaal aantal stappen voor de meest recente 1000 SQL aanvragen die zijn opgeslagen in sys.dm_pdw_exec_requests. |
| sys.dm_pdw_os_event_logs |10.000 |
| sys.dm_pdw_sql_requests |De meest recente 1000 SQL-aanvragen die zijn opgeslagen in sys.dm_pdw_exec_requests. |

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie, [SQL Data Warehouse verwijzing overzicht][SQL Data Warehouse reference overview].

<!--Image references-->

<!--Article references-->
[Data Warehouse Units (DWU)]: ./sql-data-warehouse-overview-what-is.md
[SQL Data Warehouse reference overview]: ./sql-data-warehouse-overview-reference.md
[Workload management]: ./sql-data-warehouse-develop-concurrency.md
[Tempdb]: ./sql-data-warehouse-tables-temporary.md
[data type]: ./sql-data-warehouse-tables-data-types.md
[creating a support ticket]: /sql-data-warehouse-get-started-create-support-ticket.md

<!--MSDN references-->
[Row-Overflow Data Exceeding 8 KB]: https://msdn.microsoft.com/library/ms186981.aspx
[Internal error: An expression services limit has been reached]: https://support.microsoft.com/kb/913050
