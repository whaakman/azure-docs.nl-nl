---
title: Server-logboeken voor de Azure-Database voor MySQL | Microsoft Docs
description: Beschrijft de logboeken die beschikbaar zijn in Azure-Database voor MySQL en de beschikbare parameters voor het inschakelen van van verschillende logboekregistratieniveaus.
services: mysql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/18/2017
ms.openlocfilehash: 13b30df82c1a6c4c45a621a1f7a40148a55a7648
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2017
---
# <a name="server-logs-in-azure-database-for-mysql"></a>Serverlogboekbestanden in Azure-Database voor MySQL
Het querylogboek traag is in de Azure-Database voor MySQL, beschikbaar voor gebruikers. Toegang tot het transactielogboek wordt niet ondersteund. Het querylogboek traag kan worden gebruikt voor het identificeren van knelpunten voor het oplossen van problemen. 

Zie voor meer informatie over het trage MySQL-querylogboek, de MySQL-handleiding [uplogboekgedeelte van de query vertragen](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html).

## <a name="access-server-logs"></a>Serverlogboeken openen
U kunt weergeven en Azure Database downloaden voor MySQL server-logboeken met behulp van de Azure-portal en de Azure CLI.

Selecteer uw Azure-Database voor de MySQL-server in de Azure-portal. Onder de **bewaking** kop, selecteer de **serverlogboeken** pagina.

Zie voor meer informatie over Azure CLI [configureren en access server-logboeken met Azure CLI](howto-configure-server-logs-in-cli.md).

## <a name="log-retention"></a>Logboek bewaren
Er zijn maximaal zeven dagen hun maken van logboeken beschikbaar. Als de totale grootte van de beschikbare logboeken 7.5 GB overschrijdt, worden de oudste bestanden verwijderd totdat ruimte beschikbaar is. 

Logboeken zijn geroteerd elke 24 uur of 7.5 GB, afhankelijk van wat het eerst wordt bereikt.


## <a name="configure-logging"></a>Logboekregistratie configureren 
Het querylogboek traag is standaard uitgeschakeld. Instellen zodat deze slow_query_log op ON.

Er zijn andere parameters die kunt u aanpassen:

- **long_query_time**: als een query langer dan long_query_time (in seconden duurt) die query wordt vastgelegd. De standaardwaarde is 10 seconden.
- **log_slow_admin_statements**: als ON beheerdersrechten instructies zoals ALTER_TABLE en ANALYZE_TABLE in de instructies die is geschreven naar de slow_query_log bevat.
- **log_queries_not_using_indexes**: Hiermee wordt bepaald of de query's die geen van indexen gebruikmaken worden geregistreerd in de slow_query_log
- **log_throttle_queries_not_using_indexes**: deze parameter beperkt het aantal query's voor niet-index die kunnen worden geschreven naar het logboek van de query wordt vertraagd. Deze parameter wordt van kracht wanneer log_queries_not_using_indexes is ingesteld op ON.

Zie de MySQL [trage query logboek documentatie](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html) voor een volledige beschrijving van de trage queryparameters logboek.

## <a name="next-steps"></a>Volgende stappen
- [Het configureren en server-Logboeken openen vanuit de Azure CLI](howto-configure-server-logs-in-cli.md).
