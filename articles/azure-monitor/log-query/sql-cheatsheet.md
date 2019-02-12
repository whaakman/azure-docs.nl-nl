---
title: SQL naar Azure Monitor log-query-Cheatsheet | Microsoft Docs
description: Help voor gebruikers die bekend zijn met SQL in Logboeken-query's schrijven in Azure Monitor.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: bwren
ms.openlocfilehash: 121f9ff602907e64bba0c98342e38477109e9294
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55993658"
---
# <a name="sql-to-azure-monitor-log-query-cheat-sheet"></a>SQL naar Azure Monitor log-query-Cheatsheet 

De onderstaande tabel helpt gebruikers die bekend met SQL zijn voor meer informatie over de Data Explorer-querytaal voor het schrijven van Logboeken-query's in Azure Monitor. Bekijk de T-SQL-opdracht voor het oplossen van een algemene scenario's en de overeenkomstige waarde in een query voor Azure Monitor hebben.

## <a name="sql-to-azure-monitor"></a>SQL Azure monitor

Description                             |SQL-Query                                                                                          |Azure Monitor log-query
----------------------------------------|---------------------------------------------------------------------------------------------------|----------------------------------------
Alle gegevens uit een tabel selecteren            |`SELECT * FROM dependencies`                                                                       |<code>dependencies</code>
Selecteer specifieke kolommen uit een tabel    |`SELECT name, resultCode FROM dependencies`                                                        |<code>dependencies <br>&#124; project name, resultCode</code>
100 records uit een tabel selecteren         |`SELECT TOP 100 * FROM dependencies`                                                               |<code>dependencies <br>&#124; take 100</code>
Null-evaluatie                         |`SELECT * FROM dependencies WHERE resultCode IS NOT NULL`                                          |<code>dependencies <br>&#124; where isnotnull(resultCode)</code>
Vergelijking van de tekenreeks: gelijkheid             |`SELECT * FROM dependencies WHERE name = "abcde"`                                                  |<code>dependencies <br>&#124; where name == "abcde"</code>
Vergelijking van de tekenreeks: subtekenreeks            |`SELECT * FROM dependencies WHERE like "%bcd%"`                                                    |<code>dependencies <br>&#124; where name contains "bcd"</code>
Vergelijking van de tekenreeks: jokertekens             |`SELECT * FROM dependencies WHERE name like "abc%"`                                                |<code>dependencies <br>&#124; where name startswith "abc"</code>
Datum van de vergelijking: afgelopen dag             |`SELECT * FROM dependencies WHERE timestamp > getdate()-1`                                         |<code>dependencies <br>&#124; where timestamp > ago(1d)</code>
Datum van de vergelijking: datumbereik             |`SELECT * FROM dependencies WHERE timestamp BETWEEN '2016-10-01' AND '2016-11-01'`                 |<code>dependencies <br>&#124; where timestamp between (datetime(2016-10-01) .. datetime(2016-10-01))</code>
Boole-vergelijking                      |`SELECT * FROM dependencies WHERE !(success)`                                                      |<code>dependencies <br>&#124; where success == "False" </code>
Sorteren                                    |`SELECT name, timestamp FROM dependencies ORDER BY timestamp asc`                                  |<code>dependencies <br>&#124; order by timestamp asc </code>
Distinct                                |`SELECT DISTINCT name, type  FROM dependencies`                                                    |<code>dependencies <br>&#124; summarize by name, type </code>
Grouping, Aggregation                   |`SELECT name, AVG(duration) FROM dependencies GROUP BY name`                                       |<code>dependencies <br>&#124; summarize avg(duration) by name </code>
Kolomaliassen uitbreiden                  |`SELECT operation_Name as Name, AVG(duration) as AvgD FROM dependencies GROUP BY name`             |<code>dependencies <br>&#124; summarize AvgD=avg(duration) by operation_Name <br>&#124; project Name=operation_Name, AvgD</code>
Bovenste n recrods door meting                |`SELECT TOP 100 name, COUNT(*) as Count FROM dependencies GROUP BY name ORDER BY Count asc`        |<code>dependencies <br>&#124; summarize Count=count() by name <br>&#124; top 100 by Count asc</code>
Union                                   |`SELECT * FROM dependencies UNION SELECT * FROM exceptions`                                        |<code>union dependencies, exceptions</code>
Union: met de voorwaarden                  |`SELECT * FROM dependencies WHERE value > 4 UNION SELECT * FROM exceptions value < 5`              |<code>dependencies <br>&#124; where value > 4 <br>&#124; union (exceptions <br>&#124; where value < 5)</code>
Koppelen                                    |`SELECT * FROM dependencies JOIN exceptions ON dependencies.operation_Id = exceptions.operation_Id`|<code>dependencies <br>&#124; join (exceptions) on operation_Id == operation_Id</code>


## <a name="next-steps"></a>Volgende stappen

- Ga via een les op de [logboeken-query's schrijven in Azure Monitor](get-started-queries.md).