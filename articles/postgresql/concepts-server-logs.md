---
title: Serverlogboekbestanden in Azure-Database voor PostgreSQL | Microsoft Docs
description: Query's en fout-Logboeken in Azure-Database voor PostgreSQL genereert.
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 09/26/2017
ms.openlocfilehash: 696af85cd5609171a719a7e77efbfcdeba0aaaaa
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2017
---
# <a name="server-logs-in-azure-database-for-postgresql"></a>Serverlogboekbestanden in Azure-Database voor PostgreSQL 
Azure-Database voor PostgreSQL query en de fout genereert logboeken. Toegang tot de transactielogboeken wordt echter niet ondersteund. Query's en fout logboeken kunnen worden gebruikt om te bepalen, oplossen en herstellen van fouten in de configuratie en suboptimale prestaties. Zie voor meer informatie [foutrapportage en de logboekregistratie](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html).

## <a name="access-server-logs"></a>Serverlogboeken openen
U kunt weergeven en downloaden met de Azure portal, server-foutlogboeken voor Azure PostgreSQL [Azure CLI](howto-configure-server-logs-using-cli.md), en Azure REST-API's.

## <a name="log-retention"></a>Logboek bewaren
U kunt de bewaarperiode instellen voor wordt vastgelegd met behulp van de **logboek\_bewaren\_periode** parameter die is gekoppeld aan uw server. De eenheid voor deze parameter is dagen. De standaardwaarde is 3 dagen. De maximumwaarde is 7 dagen. De server voldoende moet toegewezen opslag voor de terugkerende logboekbestanden bevatten.
De logboekbestanden roteren elke één uur of de grootte van 100 MB, afhankelijk van wat zich het eerste voordoet.

## <a name="configure-logging-for-azure-postgresql-server"></a>Logboekregistratie voor Azure PostgreSQL-server configureren
U kunt queryregistratie en foutenregistratie inschakelen voor uw server. Foutenlogboeken kunnen automatisch onderdruk-, verbindings-en controlepunten bevatten.

U kunt queryregistratie voor uw PostgreSQL-database-exemplaar inschakelen door het instellen van de twee serverparameters: `log\_statement` en `log\_min\_duration\_statement`.

De **logboek\_instructie** parameter bepaalt welke SQL-instructies worden geregistreerd. Het is raadzaam als deze parameter op ***alle*** aan te melden alle instructies; de standaardwaarde is geen.

De **logboek\_min\_duur\_instructie** parameter stelt de limiet in milliseconden van een instructie moeten worden vastgelegd. Alle SQL-instructies die langer is dan de parameterinstelling voor de uitgevoerd worden geregistreerd. Deze parameter is uitgeschakeld en wordt standaard ingesteld op min 1 (-1). Inschakelen van deze parameter is handig bij het volgen van niet-geoptimaliseerde query's in uw toepassingen.

De **logboek\_min\_berichten** kunt u om te bepalen welke niveaus bericht naar het logboek van de server worden geschreven. De standaardwaarde is een waarschuwing. 

Zie voor meer informatie over deze instellingen [foutrapportage en de logboekregistratie](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html) documentatie. Zie voor vooral Azure-Database configureren voor parameters van de server PostgreSQL, [aanpassen configuratieparameters server met Azure CLI](howto-configure-server-parameters-using-cli.md).

## <a name="next-steps"></a>Volgende stappen
- Voor toegang tot logboeken via Azure CLI-opdrachtregelinterface, Zie [configureren en access server-logboeken met Azure CLI](howto-configure-server-logs-using-cli.md).
- Zie voor meer informatie over parameters van de server [aanpassen configuratieparameters server met Azure CLI](howto-configure-server-parameters-using-cli.md).
