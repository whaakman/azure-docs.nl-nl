---
title: SQL voor het referentiemateriaal voor querytaal Azure Log Analytics | Microsoft Docs
description: Algemene functies te gebruiken voor verschillende scenario's in Log Analytics-query's.
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
ms.component: na
ms.openlocfilehash: 9ca8bb04748a1fd94c227627516be0e8ca83409d
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52679763"
---
# <a name="sql-to-log-analytics-query-language-cheat-sheet"></a>SQL voor het referentiemateriaal voor querytaal Log Analytics 

De onderstaande tabel helpt gebruikers die bekend met SQL zijn voor meer informatie over de querytaal van Log Analytics. Bekijk de T-SQL-opdracht voor het oplossen van een algemene scenario's en het equivalent met behulp van Log Analytics hebben.

## <a name="sql-to-log-analytics"></a>SQL naar Log Analytics

Beschrijving                             |SQL-Query                                                                                          |Azure Log Analytics-Query
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
Afzonderlijke                                |`SELECT DISTINCT name, type  FROM dependencies`                                                    |<code>dependencies <br>&#124; summarize by name, type </code>
Groeperen, aggregatie                   |`SELECT name, AVG(duration) FROM dependencies GROUP BY name`                                       |<code>dependencies <br>&#124; summarize avg(duration) by name </code>
Kolomaliassen uitbreiden                  |`SELECT operation_Name as Name, AVG(duration) as AvgD FROM dependencies GROUP BY name`             |<code>dependencies <br>&#124; summarize AvgD=avg(duration) by operation_Name <br>&#124; project Name=operation_Name, AvgD</code>
Bovenste n recrods door meting                |`SELECT TOP 100 name, COUNT(*) as Count FROM dependencies GROUP BY name ORDER BY Count asc`        |<code>dependencies <br>&#124; summarize Count=count() by name <br>&#124; top 100 by Count asc</code>
Union                                   |`SELECT * FROM dependencies UNION SELECT * FROM exceptions`                                        |<code>union dependencies, exceptions</code>
Union: met de voorwaarden                  |`SELECT * FROM dependencies WHERE value > 4 UNION SELECT * FROM exceptions value < 5`              |<code>dependencies <br>&#124; where value > 4 <br>&#124; union (exceptions <br>&#124; where value < 5)</code>
Koppelen                                    |`SELECT * FROM dependencies JOIN exceptions ON dependencies.operation_Id = exceptions.operation_Id`|<code>dependencies <br>&#124; join (exceptions) on operation_Id == operation_Id</code>


## <a name="next-steps"></a>Volgende stappen

- Ga via een les op de [schrijven van query's in Log Analytics](get-started-queries.md).