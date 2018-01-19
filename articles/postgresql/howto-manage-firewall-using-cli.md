---
title: Maken en beheren van Azure-Database voor firewallregels PostgreSQL met Azure CLI | Microsoft Docs
description: In dit artikel wordt beschreven hoe maken en beheren van Azure-Database voor firewallregels PostgreSQL via Azure CLI-opdrachtregel.
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 01/18/2018
ms.openlocfilehash: 233479bff0a0d331161b52d33f31b45c5714362b
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-azure-cli"></a>Maken en beheren van Azure-Database voor firewallregels PostgreSQL met Azure CLI
Firewallregels op serverniveau kunnen beheerders om toegang tot een Azure-Database voor PostgreSQL-Server beheren vanaf een specifiek IP-adres of een bereik van IP-adressen. U handige Azure CLI-opdrachten gebruikt, kunt u maken, bijwerken, verwijderen, de lijst en firewallregels voor het beheren van uw server weergegeven. Zie voor een overzicht van Azure-Database voor firewallregels PostgreSQL [Azure Database voor firewallregels voor PostgreSQL Server](concepts-firewall-rules.md)

## <a name="prerequisites"></a>Vereisten
Stap in deze handleiding instructies, wilt u het volgende nodig:
- Een [Azure Database voor PostgreSQL-server en database](quickstart-create-server-database-azure-cli.md).
- Installeer [Azure CLI 2.0](/cli/azure/install-azure-cli) opdrachtregelprogramma of gebruik de Azure-Cloud-Shell in de browser.

## <a name="configure-firewall-rules-for-azure-database-for-postgresql"></a>Firewallregels configureren voor Azure-Database voor PostgreSQL
De [az postgres server-firewallregel](/cli/azure/postgres/server/firewall-rule) opdrachten worden gebruikt voor het configureren van firewallregels.

## <a name="list-firewall-rules"></a>Lijst met firewall-regels 
Uitvoeren als de bestaande server firewallregels wilt weergeven, de [az postgres firewallregel serverlijst](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_list) opdracht.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mypgserver-20170401
```
De uitvoer bevat de firewallregels als een standaard in JSON opmaken. U kunt de switch `--output table` voor een beter leesbare tabelindeling als uitvoer.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mypgserver-20170401 --output table
```
## <a name="create-firewall-rule"></a>Firewallregel maken
U maakt een nieuwe firewallregel op de server, voeren de [az postgres server-firewallregel maken](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_create) opdracht. 

Door op te geven 0.0.0.0 als de `--start-ip-address` en 255.255.255.255 als de `--end-ip-address` bereik, het volgende voorbeeld kan alle IP-adressen voor toegang tot de server **mypgserver 20170401.postgres.database.azure.com**
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mypgserver-20170401 --name AllowIpRange --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
Geef hetzelfde adres in voor toegang tot een enkel IP-adres, de `--start-ip-address` en `--end-ip-address`, zoals in dit voorbeeld.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mypgserver-20170401 --name AllowSingleIpAddress --start-ip-address 13.83.152.1 --end-ip-address 13.83.152.1
```
Als dit lukt bevat de uitvoer van de opdracht de details van de firewallregel die u hebt gemaakt, in JSON-indeling standaard. Als er een fout is, ziet de uitvoer in plaats daarvan een foutbericht weergegeven.

## <a name="update-firewall-rule"></a>Firewallregel bijwerken 
Bijwerken van een bestaande firewallregel op de server met [az postgres serverupdate firewallregel](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_update) opdracht. Geef de naam van de bestaande firewallregel als invoer en het begin IP- en end IP-kenmerken om bij te werken.
```azurecli-interactive
az postgres server firewall-rule update --resource-group myresourcegroup --server-name mypgserver-20170401 --name AllowIpRange --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.255
```
Als dit lukt bevat de uitvoer van de opdracht de details van de firewallregel die u hebt bijgewerkt, standaard in JSON-indeling. Als er een fout is, ziet de uitvoer in plaats daarvan een foutbericht weergegeven.
> [!NOTE]
> Als de firewallregel die niet bestaat, haalt het is gemaakt door de opdracht update.

## <a name="show-firewall-rule-details"></a>Firewall regeldetails weergeven
U kunt de details van een bestaande firewallregel op serverniveau ook weergeven door te voeren [az postgres server-firewallregel weergeven](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_show) opdracht.
```azurecli-interactive
az postgres server firewall-rule show --resource-group myresourcegroup --server-name mypgserver-20170401 --name AllowIpRange
```
Als dit lukt bevat de uitvoer van de opdracht de details van de firewallregel die u hebt opgegeven, standaard in JSON-indeling. Als er een fout is, ziet de uitvoer in plaats daarvan een foutbericht weergegeven.

## <a name="delete-firewall-rule"></a>Firewallregel verwijderen
Als u wilt intrekken van toegang voor een IP-adresbereik op de server, verwijdert u een bestaande firewallregel door het uitvoeren van de [az postgres server firewall-regel verwijderen](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_delete) opdracht. Geef de naam van de bestaande firewallregel.
```azurecli-interactive
az postgres server firewall-rule delete --resource-group myresourcegroup --server-name mypgserver-20170401 --name AllowIpRange
```
Als dit lukt wordt er geen uitvoer. Bij storingen wordt tekst van het foutbericht geretourneerd.

## <a name="next-steps"></a>Volgende stappen
- Op deze manier kunt u een webbrowser [maken en beheren van Azure-Database voor firewallregels PostgreSQL met de Azure portal](howto-manage-firewall-using-portal.md).
- Meer inzicht te geven over [Azure Database voor firewallregels voor PostgreSQL Server](concepts-firewall-rules.md).
- Zie voor meer informatie in de verbinding te maken met een Azure-Database voor PostgreSQL server [verbindingsbibliotheken voor Azure-Database voor PostgreSQL](concepts-connection-libraries.md).
