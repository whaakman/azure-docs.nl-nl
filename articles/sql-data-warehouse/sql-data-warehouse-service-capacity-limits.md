---
title: Capaciteitslimieten - Azure SQL Data Warehouse | Microsoft Docs
description: Maximale waarden voor verschillende onderdelen van Azure SQL Data Warehouse is toegestaan.
services: sql-data-warehouse
author: sachinpMSFT
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 07/26/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: df3220936673e508e0fa01a283270e1b4ca4753c
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39283432"
---
# <a name="sql-data-warehouse-capacity-limits"></a>Capaciteitslimieten voor SQL Data Warehouse
Maximale waarden voor verschillende onderdelen van Azure SQL Data Warehouse is toegestaan.

## <a name="workload-management"></a>Werklastbeheer
| Categorie | Beschrijving | Maximum |
|:--- |:--- |:--- |
| [Datawarehouse Units (DWU)](what-is-a-data-warehouse-unit-dwu-cdwu.md) |Maximale DWU voor een enkele SQL Data Warehouse | Gen1: DW6000<br></br>Gen2: DW30000c |
| [Datawarehouse Units (DWU)](what-is-a-data-warehouse-unit-dwu-cdwu.md) |Standaard DTU per server |54,000<br></br>Elke SQL-server (bijvoorbeeld myserver.database.windows.net) heeft standaard een DTU-quotum van 54.000, waarmee maximaal DW6000c. Dit quotum is gewoon een veiligheidsbeperking. U kunt uw quotum door verhogen [het maken van een ondersteuningsticket](sql-data-warehouse-get-started-create-support-ticket.md) en selecteren *quotum* als het aanvraagtype.  Voor het berekenen van uw DTU moeten, de vermenigvuldigt u 7,5 met het totale aantal DWU die nodig zijn, of 9.0 vermenigvuldigen met de totale cDWU die nodig zijn. Bijvoorbeeld:<br></br>DW6000 x 7.5 = 45.000 dtu's<br></br>DW6000c x 9.0 = 54.000 dtu's.<br></br>U kunt uw huidige DTU-verbruik van de SQL server-optie weergeven in de portal. Zowel onderbroken als niet-onderbroken databases tellen mee voor het DTU-quotum. |
| De verbinding met database |Gelijktijdige sessies op openen |1024<br/><br/>Elk van de actieve sessies 1024 kan aanvragen met een SQL Data Warehouse-database verzenden op hetzelfde moment. Opmerking: Er zijn beperkingen voor het aantal query's die gelijktijdig kan worden uitgevoerd. Wanneer de limiet voor gelijktijdigheid wordt overschreden, de aanvraag wordt verwerkt in een interne wachtrij waar wacht om te worden verwerkt. |
| De verbinding met database |Maximale hoeveelheid geheugen voor voorbereide instructies |20 MB |
| [Werklastbeheer](resource-classes-for-workload-management.md) |Maximum aantal gelijktijdige query 's |128<br/><br/> SQL Data Warehouse kan een maximum van 128 gelijktijdige query's en wachtrijen resterende query's worden uitgevoerd.<br/><br/>Het aantal gelijktijdige query's kan afnemen wanneer gebruikers worden toegewezen aan hogere resourceklassen of wanneer het SQL Data Warehouse heeft een lagere [datawarehouse unit](memory-and-concurrency-limits.md) instelling. Sommige query's, zoals DMV-query's, zijn altijd toegestaan te worden uitgevoerd en niet van invloed op de limiet voor gelijktijdige query's. Zie voor meer informatie over het uitvoeren van gelijktijdige query's, de [gelijktijdigheid maximumwaarden](memory-and-concurrency-limits.md#concurrency-maximums) artikel. |
| [TempDB](sql-data-warehouse-tables-temporary.md) |Maximale GB |399 GB per DW100. Daarom wordt bij DWU1000, tempdb 3,99 TB grootte. |

## <a name="database-objects"></a>Databaseobjecten
| Categorie | Beschrijving | Maximum |
|:--- |:--- |:--- |
| Database |Maximale grootte | Gen1: 240 TB gecomprimeerd op schijf. Deze ruimte is onafhankelijk van tempdb- of logboekpad ruimte, en daarom deze ruimte wordt toegewezen aan de permanente tabellen.  Geclusterde columnstore-compressie wordt geschat op 5 X.  Deze compressie kan de database uitbreiden tot ongeveer 1 PB wanneer alle tabellen de geclusterde columnstore (de standaard-tabeltype worden). <br/><br/> Gen2: 240TB voor rowstore en onbeperkte opslag voor columnstore-tabellen |
| Tabel |Maximale grootte |60 TB gecomprimeerd op schijf |
| Tabel |Tabellen per database |10.000 |
| Tabel |Kolommen per tabel |1024 kolommen |
| Tabel |Bytes per kolom |Afhankelijk van de kolom [gegevenstype](sql-data-warehouse-tables-data-types.md). De limiet is 8000 voor de gegevenstypen char, 4000 voor nvarchar of 2 GB voor de MAX-gegevenstypen. |
| Tabel |Bytes per rij, gedefinieerde grootte |8060 bytes<br/><br/>Het aantal bytes per rij wordt berekend op dezelfde manier als voor SQL Server bij de pagina, compressie. Als SQL Server en SQL Data Warehouse biedt ondersteuning voor overloop van rij-opslag, waarmee **kolommen met variabele lengte** buiten een rij worden gepusht. Wanneer rijen met variabele lengte zijn buiten een rij worden doorgestuurd, worden alleen 24 uur per dag-byte-hoofdmap wordt opgeslagen in de belangrijkste record. Zie voor meer informatie, [overloop rij gegevens van meer dan 8 KB](https://msdn.microsoft.com/library/ms186981.aspx). |
| Tabel |Partities per tabel |15,000<br/><br/>Voor hoge prestaties, raden we het minimaliseren van het aantal partities moet u terwijl nog steeds ondersteuning van uw zakelijke vereisten. Als het aantal partities groeit, wordt de overhead voor Data Definition Language (DDL) en gegevens manipuleren taal (DML) toeneemt en zorgt ervoor dat de tragere prestaties. |
| Tabel |Tekens per partitie grenswaarde. |4000 |
| Index |Niet-geclusterde indexen per tabel. |50<br/><br/>Van toepassing op alleen rowstore-tabellen. |
| Index |Geclusterde indexen per tabel. |1<br><br/>Van toepassing op zowel rowstore en columnstore-tabellen. |
| Index |De grootte van de index. |900 bytes.<br/><br/>Van toepassing op alleen rowstore-indexen.<br/><br/>Indexen op varchar-kolommen met een maximale grootte van meer dan 900 bytes kunnen worden gemaakt als de bestaande gegevens in de kolommen niet groter is dan 900 bytes wanneer de index is gemaakt. Echter later invoegen of UPDATE-acties in de kolommen die ertoe leiden dat de totale grootte meer dan 900 bytes mislukken. |
| Index |De sleutelkolommen per index. |16<br/><br/>Van toepassing op alleen rowstore-indexen. Geclusterde columnstore-indexen bevatten alle kolommen. |
| statistieken |Grootte van de gecombineerde kolomwaarden. |900 bytes. |
| statistieken |Kolommen per statistieken-object. |32 |
| statistieken |Statistieken maken voor kolommen per tabel. |30,000 |
| Opgeslagen procedures |Maximum aantal geneste niveaus. |8 |
| Weergave |Kolommen per weergeven |1,024 |

## <a name="loads"></a>Belasting
| Categorie | Beschrijving | Maximum |
|:--- |:--- |:--- |
| Polybase-Loads |MB per rij |1<br/><br/>Polybase laadt alleen op rijen die kleiner zijn dan 1 MB en kunnen niet worden geladen naar VARCHAR(MAX), NVARCHAR(MAX) of VARBINARY(MAX).<br/><br/> |

## <a name="queries"></a>Query's
| Categorie | Beschrijving | Maximum |
|:--- |:--- |:--- |
| Query’s uitvoeren |In de wachtrij query's voor gebruikerstabellen. |1000 |
| Query’s uitvoeren |Gelijktijdige query's op systeemweergaven. |100 |
| Query’s uitvoeren |In de wachtrij-query's op systeemweergaven |1000 |
| Query’s uitvoeren |Maximum aantal parameters |2098 |
| Batch |Maximale grootte |65,536*4096 |
| Selecteer resultaten |Kolommen per rij |4096<br/><br/>U kunt nooit meer dan 4096 kolommen per rij hebben in het resultaat selecteren. Er is geen garantie dat u kunt altijd 4096 hebben. Als het queryplan is een tijdelijke tabel vereist, kunnen de 1024 kolommen per tabel maximale toepassen. |
| SELECT |Geneste subquery 's |32<br/><br/>U kunt nooit meer dan 32 geneste subquery's hebben in een instructie SELECT. Er is geen garantie dat u kunt altijd 32 hebben. Bijvoorbeeld, kunt een JOIN een subquery in het queryplan introduceren. Het aantal subquery's kan ook worden beperkt door het beschikbare geheugen. |
| SELECT |Kolommen per JOIN |1024 kolommen<br/><br/>U kunt nooit meer dan 1024 kolommen hebben in de JOIN. Er is geen garantie dat u kunt altijd 1024 hebben. Als het JOIN-plan een tijdelijke tabel met meer kolommen dan de JOIN-resultaten vereist, is de 1024-limiet van toepassing op de tijdelijke tabel. |
| SELECT |Aantal bytes per GROEPEREN op kolommen. |8060<br/><br/>De kolommen in de component GROUP BY kunnen een maximum van 8060 bytes hebben. |
| SELECT |Bytes per ORDER BY kolommen |8060 bytes<br/><br/>De kolommen in de component ORDER BY mag maximaal van 8060 bytes hebben. |
| Id's per instructie |Aantal waarnaar wordt verwezen, id 's |65,535<br/><br/>SQL Data Warehouse, beperkt het aantal id's die kunnen worden opgenomen in één expressie van een query. Meer dan dit aantal resultaten in SQL Server-fout 8632. Zie voor meer informatie [interne fout: een expressie services limiet is bereikt] [interne fout: een expressie services limiet is bereikt]. |
| Letterlijke tekenreeks | Nummer van de letterlijke tekenreeks in een instructie | 20,000 <br/><br/>SQL Data Warehouse, beperkt het aantal tekenreeksconstanten in één expressie van een query. Meer dan dit aantal resultaten in SQL Server-fout 8632.|

## <a name="metadata"></a>Metagegevens
| Systeemweergave | Maximum aantal rijen |
|:--- |:--- |
| sys.dm_pdw_component_health_alerts |10.000 |
| sys.dm_pdw_dms_cores |100 |
| sys.dm_pdw_dms_workers |Totaal aantal DMS webwerkrollen voor de meest recente 1000 aanvragen voor SQL. |
| sys.dm_pdw_errors |10.000 |
| sys.dm_pdw_exec_requests |10.000 |
| sys.dm_pdw_exec_sessions |10.000 |
| sys.dm_pdw_request_steps |Totaal aantal stappen voor de meest recente versie van 1000 SQL aanvragen die zijn opgeslagen in sys.dm_pdw_exec_requests. |
| sys.dm_pdw_os_event_logs |10.000 |
| sys.dm_pdw_sql_requests |De meest recente 1000 SQL-aanvragen die zijn opgeslagen in sys.dm_pdw_exec_requests. |

## <a name="next-steps"></a>Volgende stappen
Zie voor aanbevelingen over het gebruik van SQL Data Warehouse de [Cheat Sheet](cheat-sheet.md).
