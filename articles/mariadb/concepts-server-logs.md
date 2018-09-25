---
title: Serverlogboeken voor Azure Database voor MariaDB
description: Beschrijving van de logboeken die beschikbaar zijn in Azure Database voor MariaDB en de beschikbare parameters voor het inschakelen van van verschillende logboekregistratieniveaus.
author: rachel-msft
ms.author: raagyema
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: d982af64517ed452d907f62a39e975d472951392
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46992980"
---
# <a name="server-logs-in-azure-database-for-mariadb"></a>Serverlogboeken in Azure Database voor MariaDB
In Azure Database voor MariaDB is het logboek voor langzame query's beschikbaar voor gebruikers. Toegang tot het transactielogboek wordt niet ondersteund. Het logboek voor langzame query's kan worden gebruikt om knelpunten in de prestaties voor het oplossen van problemen.

Zie voor meer informatie over het logboek voor langzame query's, de MariaDB-documentatie voor [logboek voor langzame query's](https://mariadb.com/kb/en/library/slow-query-log-overview/).

## <a name="access-server-logs"></a>Serverlogboeken openen
U kunt weergeven en downloaden van Azure Database voor MariaDB serverlogboeken met behulp van de Azure-portal en de Azure CLI.

Selecteer uw Azure Database voor MariaDB-server in de Azure-portal. Onder de **bewaking** kop, selecteer de **serverlogboeken** pagina.

<!-- For more information on Azure CLI, see [Configure and access server logs using Azure CLI](howto-configure-server-logs-in-cli.md).-->

## <a name="log-retention"></a>Logboekbehoud
Logboeken zijn beschikbaar voor maximaal zeven dagen van hun maken. Als de totale grootte van de beschikbare logboeken 7 GB overschrijdt, worden de oudste bestanden verwijderd totdat ruimte beschikbaar is.

Logboeken zijn geroteerd elke 24 uur of 7 GB, afhankelijk van wat eerst komt.

## <a name="configure-logging"></a>Logboekregistratie configureren
Het logboek voor langzame query's is standaard uitgeschakeld. Als u wilt inschakelen, stelt u slow_query_log op ON.

Er zijn andere parameters die u kunt aanpassen:

- **long_query_time**: als een query langer duurt dan long_query_time (in seconden) deze query wordt geregistreerd. De standaardwaarde is 10 seconden.
- **log_slow_admin_statements**: als u op administratieve instructies zoals ALTER_TABLE en ANALYZE_TABLE in de instructies naar de slow_query_log geschreven bevat.
- **log_queries_not_using_indexes**: Hiermee bepaalt u of query's die geen gebruik maken van indexen worden geregistreerd in de slow_query_log
- **log_throttle_queries_not_using_indexes**: deze parameter beperkt het aantal niet-index-query's die kunnen worden geschreven naar het logboek voor langzame query's. Deze parameter wordt van kracht wanneer log_queries_not_using_indexes is ingesteld op ON.

Zie de MariaDB [trage query-documentatie voor log](https://mariadb.com/kb/en/library/slow-query-log-overview/) voor een volledige beschrijving van de parameters voor het vastleggen van langzame query's.

## <a name="next-steps"></a>Volgende stappen
- [Over het configureren en serverlogboeken openen vanuit de Azure-portal](howto-configure-server-logs-portal.md).
