---
title: Capaciteitslimieten - Azure SQL Data Warehouse | Microsoft Docs
description: De maximumwaarde is toegestaan voor de verschillende onderdelen van Azure SQL Data Warehouse.
services: sql-data-warehouse
author: antvgski
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: b79d928f3c1c3d81fbca0b8d676d4a4cbf83369a
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34839636"
---
# <a name="sql-data-warehouse-capacity-limits"></a>SQL Data Warehouse Capaciteitslimieten
De maximumwaarde is toegestaan voor de verschillende onderdelen van Azure SQL Data Warehouse.

## <a name="workload-management"></a>Werklastbeheer
| Category | Beschrijving | Maximum |
|:--- |:--- |:--- |
| [Datawarehouse Units (DWU)](what-is-a-data-warehouse-unit-dwu-cdwu.md) |Maximale DWU voor een enkele SQL Data Warehouse | Gen1: DW6000<br></br>Gen2: DW30000c |
| [Datawarehouse Units (DWU)](what-is-a-data-warehouse-unit-dwu-cdwu.md) |Standaard DTU per server |54,000<br></br>Elke SQL-server (bijvoorbeeld myserver.database.windows.net) heeft een DTU-quotum 54.000, waardoor maximaal DW6000c standaard. Dit quotum is gewoon een veiligheidsbeperking. U kunt uw quotum door verhogen [een ondersteuningsticket maken](sql-data-warehouse-get-started-create-support-ticket.md) en te selecteren *quotum* als het aanvraagtype.  Voor het berekenen van uw DTU heeft, de 7.5 vermenigvuldigen met het totale aantal dat DWU nodig of 9.0 vermenigvuldigen met de totale cDWU nodig. Bijvoorbeeld:<br></br>DW6000 x 7.5 = 45,000 dtu's<br></br>DW6000c x 9.0 = 54.000 dtu's.<br></br>U kunt uw huidige DTU-verbruik van de SQL server-optie weergeven in de portal. Zowel onderbroken als niet-onderbroken databases tellen mee voor het DTU-quotum. |
| Databaseverbinding |Gelijktijdige sessies actief |1024<br/><br/>Elk van de actieve sessies 1024 kunt aanvragen met een SQL Data Warehouse-database verzenden op hetzelfde moment. Let op: Er zijn limieten van het aantal query's die gelijktijdig kan worden uitgevoerd. Wanneer de gelijktijdigheid limiet wordt overschreden, gaat de aanvraag u naar een interne wachtrij waar wacht om te worden verwerkt. |
| Databaseverbinding |Maximale hoeveelheid geheugen voor voorbereide instructies |20 MB |
| [Werklastbeheer](resource-classes-for-workload-management.md) |Maximum aantal gelijktijdige query 's |32<br/><br/> SQL Data Warehouse kan standaard maximaal 32 gelijktijdige query's en wachtrijen resterende query's worden uitgevoerd.<br/><br/>Het aantal gelijktijdige query's kan afnemen wanneer gebruikers worden toegewezen aan hogere resource klassen of wanneer SQL Data Warehouse heeft een lagere [datawarehouse unit](memory-and-concurrency-limits.md) instelling. Sommige query's, zoals DMV-query's, zijn altijd toegestaan om uit te voeren. |
| [TempDB](sql-data-warehouse-tables-temporary.md) |Maximale GB |399 GB per DW100. Tempdb wordt daarom op DWU1000 aangepast 3,99 TB. |

## <a name="database-objects"></a>Databaseobjecten
| Category | Beschrijving | Maximum |
|:--- |:--- |:--- |
| Database |Max. grootte | Gen1: 240 TB gecomprimeerd op schijf. Deze ruimte is onafhankelijk van tempdb- of logboekbestand ruimte, en daarom deze ruimte wordt toegewezen aan permanente tabellen.  De geclusterde columnstore-compressie schatting op 5 X.  Deze compressie kan de database uitbreiden tot ongeveer 1 PB wanneer alle tabellen geclusterde columnstore (de standaard tabeltype) zijn. <br/><br/> Gen2: 240TB voor rowstore en onbeperkte opslag voor de columnstore-tabellen |
| Tabel |Max. grootte |60 TB gecomprimeerd op schijf |
| Tabel |Tabellen per database |10.000 |
| Tabel |Kolommen per tabel |1024 kolommen |
| Tabel |Bytes per kolom |Afhankelijk van de kolom [gegevenstype](sql-data-warehouse-tables-data-types.md). De limiet is 8000 voor de gegevenstypen char, 4000 voor nvarchar of 2 GB voor de gegevenstypen MAX. |
| Tabel |Bytes per rij, gedefinieerde grootte |8060 bytes<br/><br/>Het aantal bytes per rij wordt berekend op dezelfde manier als voor SQL Server bij de pagina compressie. Zoals SQL Server ondersteunt SQL Data Warehouse overloop rij-opslag, waardoor **kolommen met variabele lengte** moet buiten een rij worden gepusht. Wanneer de rijen met variabele lengte zijn buiten de rij gepusht, wordt alleen 24-byte-hoofdmap opgeslagen in de belangrijkste record. Zie voor meer informatie [overloop rij gegevens van meer dan 8 KB](https://msdn.microsoft.com/library/ms186981.aspx). |
| Tabel |Partities per tabel |15,000<br/><br/>Voor hoge prestaties raden we het nummer voor het minimaliseren van partities moet u terwijl toch op uw zakelijke vereisten. Wanneer het aantal partities groeit, wordt de overhead voor bewerkingen Data Definition Language (DDL) en gegevens manipulatie taal (DML) groeit en zorgt ervoor dat de tragere prestaties. |
| Tabel |Tekens per partitie grenswaarde. |4000 |
| Index |Niet-geclusterde indexen per tabel. |50<br/><br/>Van toepassing op alleen rowstore-tabellen. |
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
| SELECTEREN |Geneste subquery 's |32<br/><br/>U kunt nooit meer dan 32 geneste subquery's in een SELECT-instructie hebben. Er is geen garantie dat u kunt altijd 32 hebben. Een JOIN kan bijvoorbeeld een subquery in het queryplan introduceren. Het aantal subquery's kan ook worden beperkt door het beschikbare geheugen. |
| SELECTEREN |Kolommen per JOIN |1024 kolommen<br/><br/>U kunt nooit meer dan 1024 kolommen hebben in de JOIN. Er is geen garantie dat u kunt altijd 1024 hebben. Als het plan JOIN een tijdelijke tabel met meer kolommen dan de JOIN-resultaat vereist, geldt de 1024-limiet voor de tijdelijke tabel. |
| SELECTEREN |Aantal bytes per GROEPEREN op kolommen. |8060<br/><br/>De kolommen in de component GROUP BY kunnen maximaal 8060 bytes hebben. |
| SELECTEREN |Aantal bytes per ORDER BY kolommen |8060 bytes<br/><br/>De kolommen in de component ORDER BY kunnen maximaal van 8060 bytes hebben. |
| Id's per instructie |Aantal waarnaar wordt verwezen id 's |65,535<br/><br/>SQL Data Warehouse beperkt het aantal id's die kunnen worden opgenomen in één expressie van een query. Dit nummer resulteert in een SQL Server-fout 8632 overschrijdt. Zie voor meer informatie [interne fout: een expressie services limiet is bereikt.] [interne fout: een expressie services limiet is bereikt]. |
| Letterlijke tekenreeks | Aantal letterlijke tekenreeks in een instructie | 20,000 <br/><br/>SQL Data Warehouse beperkt het aantal tekenreeksconstanten in één expressie van een query. Dit nummer resulteert in een SQL Server-fout 8632 overschrijdt.|

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
Zie voor aanbevelingen voor het gebruik van SQL Data Warehouse de [cheats blad](cheat-sheet.md).
