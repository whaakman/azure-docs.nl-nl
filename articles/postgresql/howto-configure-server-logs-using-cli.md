---
title: Configureren en toegang tot server-logboeken voor PostgreSQL met Azure CLI | Microsoft Docs
description: Dit artikel wordt beschreven hoe u kunt configureren en toegang tot de server-Logboeken in Azure-Database voor PostgreSQL via Azure CLI-opdrachtregel.
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 26f8e12c493904f722cad5191ee053feff20f7fc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a>Configureren en toegang tot de server-logboeken met Azure CLI
U kunt de PostgreSQL server-foutenlogboeken met behulp van de opdrachtregelinterface (Azure CLI) downloaden. Toegang tot de transactielogboeken wordt echter niet ondersteund. 

## <a name="prerequisites"></a>Vereisten
Stap in deze handleiding instructies, wilt u het volgende nodig:
- Een [Azure-Database voor PostgreSQL-server](quickstart-create-server-database-azure-cli.md)
- Installeer [Azure CLI 2.0](/cli/azure/install-azure-cli) opdrachtregelprogramma of gebruik de Azure-Cloud-Shell in de browser.

## <a name="configure-logging-for-azure-database-for-postgresql"></a>Logboekregistratie voor Azure-Database voor PostgreSQL configureren
U kunt de server voor toegang tot de querylogboeken van de en foutenlogboeken configureren. Foutenlogboeken kunnen automatisch onderdruk-, verbindings-en controlepunten bevatten.
1. Logboekregistratie inschakelen
2. Update-logboek\_-instructie en logboekbestanden\_min\_duur\_instructie query logboekregistratie inschakelen
3. Bewaarperiode bijwerken

Zie voor meer informatie [configuratieparameters server aanpassen](howto-configure-server-parameters-using-cli.md).

## <a name="list-logs-for-azure-database-for-postgresql-server"></a>Logboeken van de lijst voor de Azure-Database voor PostgreSQL-server
Uitvoeren als de beschikbare logboekbestanden voor uw server wilt weergeven, de [az postgres serverlogboeken lijst](/cli/azure/postgres/server-logs#list) opdracht.

U kunt geven lijsten van logboekbestanden voor server **mypgserver 20170401.postgres.database.azure.com** onder de resourcegroep **myresourcegroup**, en het omleiden naar een tekstbestand aangeroepen **logboek\_bestanden\_lijst.txt.**
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mypgserver-20170401 > log_files_list.txt
```
## <a name="download-logs-locally-from-the-server"></a>Lokaal logboeken downloaden van de server
De [az postgres serverlogboeken downloaden](/cli/azure/postgres/server-logs#download) opdracht kunt u afzonderlijke logboekbestanden voor uw server downloaden. 

In dit voorbeeld downloadt de specifiek logboekbestand voor de server **mypgserver 20170401.postgres.database.azure.com** onder de resourcegroep **myresourcegroup** op uw lokale omgeving.
```azurecli-interactive
az postgres server-logs download --name 20170414-mypgserver-20170401-postgresql.log --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over de serverlogboeken [Server-logboeken in Azure-Database voor PostgreSQL](concepts-server-logs.md)
- Zie voor meer informatie over parameters van de server [server configuratieparameters met Azure CLI aanpassen](howto-configure-server-parameters-using-cli.md)
