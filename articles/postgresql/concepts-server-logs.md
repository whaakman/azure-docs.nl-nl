---
title: Serverlogboeken in Azure Database for PostgreSQL
description: Dit artikel wordt beschreven hoe Azure Database voor PostgreSQL logboeken-query en de fout genereert, en hoe melden bewaarperiode is geconfigureerd.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: bcca8ce8d11482dd8517992297b7e8a5b94ac8b1
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37435487"
---
# <a name="server-logs-in-azure-database-for-postgresql"></a>Serverlogboeken in Azure Database for PostgreSQL 
Azure Database voor PostgreSQL-query en de fout genereert logboeken. Toegang tot de transactielogboeken wordt echter niet ondersteund. Query's en fout-logboeken kunnen worden gebruikt om te bepalen, oplossen en herstellen van fouten in de configuratie en optimale prestaties. Zie voor meer informatie, [foutrapportage en de logboekregistratie](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html).

## <a name="access-server-logs"></a>Serverlogboeken openen
U kunt weergeven en downloaden van de Azure PostgreSQL-server-foutenlogboeken met behulp van Azure portal, [Azure CLI](howto-configure-server-logs-using-cli.md), en Azure REST API's.

## <a name="log-retention"></a>Logboekbehoud
U kunt de bewaarperiode instellen voor systeem-logboeken met behulp van de **log\_retentie\_periode** parameter die is gekoppeld aan uw server. De eenheid voor deze parameter is dagen. De standaardwaarde is 3 dagen. De maximumwaarde is 7 dagen. De server moet er voldoende opslag bevat de logboekbestanden van de ingehouden toegewezen.
De logboekbestanden draaien om één uur of de grootte van 100 MB, afhankelijk van wat het eerste komt.

## <a name="configure-logging-for-azure-postgresql-server"></a>Logboekregistratie voor Azure PostgreSQL-server configureren
U kunt query logboekregistratie en logboekregistratie van fouten inschakelen voor uw server. Foutenlogboeken kunnen automatisch onderdruk, de verbinding en de controlepunten informatie bevatten.

U kunt query logboekregistratie inschakelen voor uw PostgreSQL-database-exemplaar door in te stellen twee parameters van de server: `log_statement` en `log_min_duration_statement`.

De **log\_instructie** parameter bepaalt welke SQL-instructies worden geregistreerd. Het is raadzaam deze parameter instelt op ***alle*** om aan te melden alle instructies; de standaardwaarde is ingesteld op geen.

De **log\_min\_duur\_instructie** parameter stelt de limiet in milliseconden van een instructie moeten worden vastgelegd. Alle SQL-instructies die langer zijn dan de instelling van de parameter worden geregistreerd. Deze parameter is uitgeschakeld en is standaard ingesteld op min 1 (-1). Inschakelen van deze parameter is handig bij het controleren van niet-geoptimaliseerde query's in uw toepassingen.

De **log\_min\_berichten** kunt u om te bepalen welke bericht niveaus worden geschreven naar het logboek voor de server. De standaardwaarde is een waarschuwing. 

Zie voor meer informatie over deze instellingen [foutrapportage en de logboekregistratie](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html) documentatie. Zie voor met name Azure Database voor PostgreSQL-serverparameters configureert, [met behulp van Azure CLI parameters voor serverconfiguratie aanpassen](howto-configure-server-parameters-using-cli.md).

## <a name="next-steps"></a>Volgende stappen
- Voor toegang tot logboeken met behulp van Azure CLI-opdrachtregelinterface, Zie [configureren en -server-logboeken met behulp van Azure CLI](howto-configure-server-logs-using-cli.md).
- Zie voor meer informatie over serverparameters [met behulp van Azure CLI parameters voor serverconfiguratie aanpassen](howto-configure-server-parameters-using-cli.md).
