---
title: Serverlogboeken voor Azure Database voor MariaDB
description: Beschrijving van de logboeken die beschikbaar zijn in Azure Database voor MariaDB en de beschikbare parameters voor het inschakelen van van verschillende logboekregistratieniveaus.
author: rachel-msft
ms.author: raagyema
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/12/2019
ms.openlocfilehash: 7a517be49a249b0b73c901137381bd05946aa4cc
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67065704"
---
# <a name="slow-query-logs-in-azure-database-for-mariadb"></a>Logboeken van de langzame query's in Azure Database voor MariaDB
In Azure Database voor MariaDB is het logboek voor langzame query's beschikbaar voor gebruikers. Toegang tot het transactielogboek wordt niet ondersteund. Het logboek voor langzame query's kan worden gebruikt om knelpunten in de prestaties voor het oplossen van problemen.

Zie voor meer informatie over het logboek voor langzame query's, de MariaDB-documentatie voor [logboek voor langzame query's](https://mariadb.com/kb/en/library/slow-query-log-overview/).

## <a name="access-slow-query-logs"></a>Langzame-querylogboeken van toegang
U kunt weergeven en downloaden van Azure Database voor MariaDB van Logboeken voor langzame query's met behulp van de Azure-portal en de Azure CLI.

Selecteer uw Azure Database voor MariaDB-server in de Azure-portal. Onder de **bewaking** kop, selecteer de **serverlogboeken** pagina.

Zie voor meer informatie over Azure-CLI [configureren en -server-logboeken met behulp van Azure CLI](howto-configure-server-logs-cli.md).

## <a name="log-retention"></a>Logboekbehoud
Logboeken zijn beschikbaar voor maximaal zeven dagen van hun maken. Als de totale grootte van de beschikbare logboeken 7 GB overschrijdt, worden de oudste bestanden verwijderd totdat ruimte beschikbaar is.

Logboeken zijn geroteerd elke 24 uur of 7 GB, afhankelijk van wat eerst komt.

## <a name="configure-slow-query-logging"></a>Logboekregistratie voor langzame query's configureren
Het logboek voor langzame query's is standaard uitgeschakeld. Als u wilt inschakelen, stelt u slow_query_log op ON.

Er zijn andere parameters die u kunt aanpassen:

- **long_query_time**: als een query langer duurt dan long_query_time (in seconden) deze query wordt geregistreerd. De standaardwaarde is 10 seconden.
- **log_slow_admin_statements**: als u op administratieve instructies zoals ALTER_TABLE en ANALYZE_TABLE in de instructies naar de slow_query_log geschreven bevat.
- **log_queries_not_using_indexes**: Hiermee bepaalt u of query's die geen gebruik maken van indexen worden geregistreerd in de slow_query_log
- **log_throttle_queries_not_using_indexes**: Deze parameter beperkt het aantal niet-index-query's die kunnen worden geschreven naar het logboek voor langzame query's. Deze parameter wordt van kracht wanneer log_queries_not_using_indexes is ingesteld op ON.

Zie de MariaDB [trage query-documentatie voor log](https://mariadb.com/kb/en/library/slow-query-log-overview/) voor een volledige beschrijving van de parameters voor het vastleggen van langzame query's.

## <a name="diagnostic-logs"></a>Diagnostische logboeken
Azure Database voor MariaDB is geïntegreerd met Azure Monitor diagnostische logboeken. Als u Logboeken voor langzame query's hebt ingeschakeld op uw server MariaDB, kunt u deze naar Azure Monitor-Logboeken, Event Hubs of Azure Storage verzonden. Zie voor meer informatie over het inschakelen van diagnostische logboeken, het gedeelte van de [diagnostische logboeken documentatie](../azure-monitor/platform/diagnostic-logs-overview.md).

> [!IMPORTANT]
> Deze diagnostische functies voor server-Logboeken is alleen beschikbaar in het algemeen gebruik en geoptimaliseerd voor geheugen [Prijscategorieën](concepts-pricing-tiers.md).

De volgende tabel wordt beschreven wat er in elk logboek. Afhankelijk van de uitvoermethode, zijn de velden die zijn opgenomen en de volgorde waarin ze worden weergegeven kunnen verschillen.

| **Eigenschap** | **Beschrijving** |
|---|---|
| `TenantId` | Uw tenant-ID |
| `SourceSystem` | `Azure` |
| `TimeGenerated` [UTC] | Tijdstempel wanneer het logboek is vastgelegd in UTC |
| `Type` | Het type van het logboek. Altijd `AzureDiagnostics` |
| `SubscriptionId` | GUID voor het abonnement waartoe de server behoort |
| `ResourceGroup` | Naam van de resourcegroep die de server behoort |
| `ResourceProvider` | De naam van de resourceprovider. Altijd `MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | Resource-URI |
| `Resource` | Naam van de server |
| `Category` | `MySqlSlowLogs` |
| `OperationName` | `LogEvent` |
| `Logical_server_name_s` | Naam van de server |
| `start_time_t` [UTC] | Tijd die de query is begonnen |
| `query_time_s` | Totale tijd dat de query duurde om uit te voeren |
| `lock_time_s` | Totale tijd dat de query is vergrendeld |
| `user_host_s` | Gebruikersnaam |
| `rows_sent_s` | Aantal rijen dat is verzonden |
| `rows_examined_s` | Aantal rijen onderzocht |
| `last_insert_id_s` | [last_insert_id](https://mariadb.com/kb/en/library/last_insert_id/) |
| `insert_id_s` | ID ingevoegd |
| `sql_text_s` | Volledige-query |
| `server_id_s` | Server-ID |
| `thread_id_s` | Thread-ID |
| `\_ResourceId` | Resource-URI |

## <a name="next-steps"></a>Volgende stappen
- [Over het configureren en serverlogboeken openen vanuit de Azure-portal](howto-configure-server-logs-portal.md).
