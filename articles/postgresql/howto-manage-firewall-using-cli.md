---
title: Maken en beheren van Azure-Database voor firewallregels PostgreSQL met Azure CLI
description: In dit artikel wordt beschreven hoe maken en beheren van Azure-Database voor firewallregels PostgreSQL via Azure CLI-opdrachtregel.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 05/4/2018
ms.openlocfilehash: ba5533184331b3692882b224b77ad1f38e970661
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-azure-cli"></a>Maken en beheren van Azure-Database voor firewallregels PostgreSQL met Azure CLI
Firewallregels op serverniveau kunnen beheerders om toegang tot een Azure-Database voor PostgreSQL-Server beheren vanaf een specifiek IP-adres of een bereik van IP-adressen. U handige Azure CLI-opdrachten gebruikt, kunt u maken, bijwerken, verwijderen, de lijst en firewallregels voor het beheren van uw server weergegeven. Zie voor een overzicht van Azure-Database voor firewallregels PostgreSQL [Azure Database voor firewallregels voor PostgreSQL Server](concepts-firewall-rules.md)

## <a name="prerequisites"></a>Vereisten
Stap in deze handleiding instructies, wilt u het volgende nodig:
- Installeer [Azure CLI 2.0](/cli/azure/install-azure-cli) opdrachtregelprogramma of gebruik de Azure-Cloud-Shell in de browser.
- Een [Azure Database voor PostgreSQL-server en database](quickstart-create-server-database-azure-cli.md).

## <a name="configure-firewall-rules-for-azure-database-for-postgresql"></a>Firewallregels configureren voor Azure-Database voor PostgreSQL
De [az postgres server-firewallregel](/cli/azure/postgres/server/firewall-rule) opdrachten worden gebruikt voor het configureren van firewallregels.

## <a name="list-firewall-rules"></a>Lijst met firewall-regels 
Uitvoeren als de bestaande server firewallregels wilt weergeven, de [az postgres firewallregel serverlijst](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_list) opdracht.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver
```
De uitvoer bevat de firewallregels als een standaard in JSON opmaken. U kunt de switch `--output table` voor een beter leesbare tabelindeling als uitvoer.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver --output table
```
## <a name="create-firewall-rule"></a>Firewallregel maken
U maakt een nieuwe firewallregel op de server, voeren de [az postgres server-firewallregel maken](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_create) opdracht. 

Door op te geven 0.0.0.0 als de `--start-ip-address` en 255.255.255.255 als de `--end-ip-address` bereik, het volgende voorbeeld kan alle IP-adressen voor toegang tot de server **mydemoserver.postgres.database.azure.com**
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
Geef hetzelfde adres in voor toegang tot een enkel IP-adres, de `--start-ip-address` en `--end-ip-address`, zoals in dit voorbeeld.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name AllowSingleIpAddress --start-ip-address 13.83.152.1 --end-ip-address 13.83.152.1
```
Geef het IP-adres 0.0.0.0 als de eerste IP- en eind-IP, zoals in dit voorbeeld zodat toepassingen van Azure IP-adressen te verbinden met uw Azure-Database voor PostgreSQL-server.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name AllowAllAzureIps --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!IMPORTANT]
> Met deze optie configureert u de firewall zo dat alle verbindingen vanuit Azure zijn toegestaan, inclusief verbindingen vanuit de abonnementen van andere klanten. Wanneer u deze optie selecteert, zorg dan dat uw aanmeldings- en gebruikersmachtigingen de toegang beperken tot alleen geautoriseerde gebruikers.
> 

Als dit lukt bevat de uitvoer van de opdracht de details van de firewallregel die u hebt gemaakt, in JSON-indeling standaard. Als er een fout is, ziet de uitvoer in plaats daarvan een foutbericht weergegeven.

## <a name="update-firewall-rule"></a>Firewallregel bijwerken 
Bijwerken van een bestaande firewallregel op de server met [az postgres serverupdate firewallregel](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_update) opdracht. Geef de naam van de bestaande firewallregel als invoer en het begin IP- en end IP-kenmerken om bij te werken.
```azurecli-interactive
az postgres server firewall-rule update --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.255
```
Als dit lukt bevat de uitvoer van de opdracht de details van de firewallregel die u hebt bijgewerkt, standaard in JSON-indeling. Als er een fout is, ziet de uitvoer in plaats daarvan een foutbericht weergegeven.
> [!NOTE]
> Als de firewallregel die niet bestaat, haalt het is gemaakt door de opdracht update.

## <a name="show-firewall-rule-details"></a>Firewall regeldetails weergeven
U kunt de details van een bestaande firewallregel op serverniveau ook weergeven door te voeren [az postgres server-firewallregel weergeven](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_show) opdracht.
```azurecli-interactive
az postgres server firewall-rule show --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange
```
Als dit lukt bevat de uitvoer van de opdracht de details van de firewallregel die u hebt opgegeven, standaard in JSON-indeling. Als er een fout is, ziet de uitvoer in plaats daarvan een foutbericht weergegeven.

## <a name="delete-firewall-rule"></a>Firewallregel verwijderen
Als u wilt intrekken van toegang voor een IP-adresbereik op de server, verwijdert u een bestaande firewallregel door het uitvoeren van de [az postgres server firewall-regel verwijderen](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_delete) opdracht. Geef de naam van de bestaande firewallregel.
```azurecli-interactive
az postgres server firewall-rule delete --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange
```
Als dit lukt wordt er geen uitvoer. Bij storingen wordt tekst van het foutbericht geretourneerd.

## <a name="next-steps"></a>Volgende stappen
- Op deze manier kunt u een webbrowser [maken en beheren van Azure-Database voor firewallregels PostgreSQL met de Azure portal](howto-manage-firewall-using-portal.md).
- Meer inzicht te geven over [Azure Database voor firewallregels voor PostgreSQL Server](concepts-firewall-rules.md).
- Zie voor meer informatie in de verbinding te maken met een Azure-Database voor PostgreSQL server [verbindingsbibliotheken voor Azure-Database voor PostgreSQL](concepts-connection-libraries.md).
