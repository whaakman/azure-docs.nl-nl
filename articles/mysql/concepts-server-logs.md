---
title: Serverlogboeken voor Azure Database for MySQL
description: Beschrijving van de logboeken die beschikbaar zijn in Azure Database voor MySQL en de beschikbare parameters voor het inschakelen van van verschillende logboekregistratieniveaus.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2019
ms.openlocfilehash: bc3dd038a21786a75d6b519ef8523dc328829705
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57192885"
---
# <a name="server-logs-in-azure-database-for-mysql"></a>Serverlogboeken in Azure Database for MySQL
In Azure Database voor MySQL is het logboek voor langzame query's beschikbaar voor gebruikers. Toegang tot het transactielogboek wordt niet ondersteund. Het logboek voor langzame query's kan worden gebruikt om knelpunten in de prestaties voor het oplossen van problemen. 

Zie voor meer informatie over het MySQL-logboek voor langzame query's, de MySQL-handleiding [uplogboekgedeelte van de query vertragen](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html).

## <a name="access-server-logs"></a>Serverlogboeken openen
U kunt weergeven en downloaden van Azure Database voor MySQL-server-logboeken met behulp van de Azure-portal en de Azure CLI.

Selecteer uw Azure Database voor MySQL-server in de Azure-portal. Onder de **bewaking** kop, selecteer de **serverlogboeken** pagina.

Zie voor meer informatie over Azure-CLI [configureren en -server-logboeken met behulp van Azure CLI](howto-configure-server-logs-in-cli.md).

## <a name="log-retention"></a>Logboekbehoud
Logboeken zijn beschikbaar voor maximaal zeven dagen van hun maken. Als de totale grootte van de beschikbare logboeken 7 GB overschrijdt, worden de oudste bestanden verwijderd totdat ruimte beschikbaar is. 

Logboeken zijn geroteerd elke 24 uur of 7 GB, afhankelijk van wat eerst komt.


## <a name="configure-logging"></a>Logboekregistratie configureren 
Het logboek voor langzame query's is standaard uitgeschakeld. Als u wilt inschakelen, stelt u slow_query_log op ON.

Er zijn andere parameters die u kunt aanpassen:

- **long_query_time**: als een query langer duurt dan long_query_time (in seconden) deze query wordt geregistreerd. De standaardwaarde is 10 seconden.
- **log_slow_admin_statements**: als u op administratieve instructies zoals ALTER_TABLE en ANALYZE_TABLE in de instructies naar de slow_query_log geschreven bevat.
- **log_queries_not_using_indexes**: Hiermee bepaalt u of query's die geen gebruik maken van indexen worden geregistreerd in de slow_query_log
- **log_throttle_queries_not_using_indexes**: Deze parameter beperkt het aantal niet-index-query's die kunnen worden geschreven naar het logboek voor langzame query's. Deze parameter wordt van kracht wanneer log_queries_not_using_indexes is ingesteld op ON.

Zie de MySQL [trage query-documentatie voor log](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html) voor een volledige beschrijving van de parameters voor het vastleggen van langzame query's.

## <a name="diagnostic-logs"></a>Diagnostische logboeken
Azure Database voor MySQL is geïntegreerd met Azure Monitor diagnostische logboeken. Als u Logboeken voor langzame query's hebt ingeschakeld op uw MySQL-server, kunt u deze naar Azure Monitor-Logboeken, Event Hubs of Azure Storage verzonden. Zie voor meer informatie over het inschakelen van diagnostische logboeken, het gedeelte van de [diagnostische logboeken documentatie](../azure-monitor/platform/diagnostic-logs-overview.md).

> [!IMPORTANT]
> Deze diagnostische functies voor server-Logboeken is alleen beschikbaar in het algemeen gebruik en geoptimaliseerd voor geheugen [Prijscategorieën](concepts-pricing-tiers.md).

De volgende tabel wordt beschreven wat er in elk logboek. Afhankelijk van de uitvoermethode, zijn de velden die zijn opgenomen en de volgorde waarin ze worden weergegeven kunnen verschillen.

| **Eigenschap** | **Beschrijving** |
|---|---|---|
| TenantId | Uw tenant-ID |
| SourceSystem | `Azure` |
| TimeGenerated [UTC] | Tijdstempel wanneer het logboek is vastgelegd in UTC |
| Type | Het type van het logboek. Altijd `AzureDiagnostics` |
| SubscriptionId | GUID voor het abonnement waartoe de server behoort |
| ResourceGroup | Naam van de resourcegroep die de server behoort |
| ResourceProvider | De naam van de resourceprovider. Altijd `MICROSOFT.DBFORMYSQL` |
| ResourceType | `Servers` |
| ResourceId | Resource-URI |
| Resource | Naam van de server |
| Categorie | `MySqlSlowLogs` |
| OperationName | `LogEvent` |
| Logical_server_name_s | Naam van de server |
| start_time_t [UTC] | Tijd die de query is begonnen |
| query_time_s | Totale tijd dat de query duurde om uit te voeren |
| lock_time_s | Totale tijd dat de query is vergrendeld |
| user_host_s | Gebruikersnaam |
| rows_sent_s | Aantal rijen dat is verzonden |
| rows_examined_s | Aantal rijen onderzocht |
| last_insert_id_s | [last_insert_id](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_last-insert-id) |
| insert_id_s | Id ingevoegd |
| sql_text_s | Volledige-query |
| server_id_s | Id van de server |
| thread_id_s | Thread-id |
| \_ResourceId | Resource-URI |

## <a name="next-steps"></a>Volgende stappen
- [Over het configureren en serverlogboeken openen vanuit de Azure CLI](howto-configure-server-logs-in-cli.md).
