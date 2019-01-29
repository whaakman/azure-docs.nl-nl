---
title: Maken en beheren van Azure Database voor PostgreSQL-firewallregels met behulp van Azure CLI
description: In dit artikel wordt beschreven hoe u maken en beheren van Azure Database voor PostgreSQL-firewallregels met behulp van Azure CLI-opdrachtregel.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 05/4/2018
ms.openlocfilehash: d450b8d154e920bfc9a82314d34f20a52af71dab
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55181993"
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-azure-cli"></a>Maken en beheren van Azure Database voor PostgreSQL-firewallregels met behulp van Azure CLI
Firewallregels op serverniveau kunnen beheerders toegang tot een Azure Database for PostgreSQL-Server beheren vanaf een specifiek IP-adres of IP-adresbereik. Met behulp van handige Azure CLI-opdrachten, kunt u maken, bijwerken, verwijderen, lijst, en firewallregels voor het beheren van uw server weergeven. Zie voor een overzicht van Azure Database voor PostgreSQL-firewallregels, [Azure Database for PostgreSQL-Server firewall-regels](concepts-firewall-rules.md)

## <a name="prerequisites"></a>Vereisten
Als u wilt in deze gebruiksaanwijzing kunt doorlopen, hebt u het volgende nodig:
- Installeer [Azure CLI](/cli/azure/install-azure-cli) opdrachtregelprogramma of gebruik de Azure Cloud Shell in de browser.
- Een [Azure Database for PostgreSQL-server en database](quickstart-create-server-database-azure-cli.md).

## <a name="configure-firewall-rules-for-azure-database-for-postgresql"></a>Firewall-regels configureren voor Azure Database for PostgreSQL
De [az postgres server firewall-rule](/cli/azure/postgres/server/firewall-rule) opdrachten worden gebruikt voor het configureren van firewallregels.

## <a name="list-firewall-rules"></a>Lijst met firewall-regels 
Als u de bestaande server firewall-regels, voer de [az postgres server firewall-rule list](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_list) opdracht.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver
```
De uitvoer worden de firewallregels, als een standaard in JSON-indeling. U kunt de switch `--output table` voor een beter leesbare tabelindeling als de uitvoer.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver --output table
```
## <a name="create-firewall-rule"></a>Firewallregel maken
Voer voor het maken van een nieuwe firewallregel op de server de [az postgres server firewall-regel maken](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_create) opdracht. 

```
To allow access to a singular IP address, provide the same address in the `--start-ip-address` and `--end-ip-address`, as in this example, replacing the IP shown here with your specific IP.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name AllowSingleIpAddress --start-ip-address 13.83.152.1 --end-ip-address 13.83.152.1
```
Geef het IP-adres 0.0.0.0 als de eerste IP- en eind-IP-adres, zoals in dit voorbeeld zodat toepassingen vanuit Azure IP-adressen te verbinden met uw Azure Database for PostgreSQL-server.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name AllowAllAzureIps --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!IMPORTANT]
> Met deze optie configureert u de firewall zo dat alle verbindingen vanuit Azure zijn toegestaan, inclusief verbindingen vanuit de abonnementen van andere klanten. Wanneer u deze optie selecteert, zorg dan dat uw aanmeldings- en gebruikersmachtigingen de toegang beperken tot alleen geautoriseerde gebruikers.
> 

Implementatie is geslaagd bevat de uitvoer van de opdracht de details van de firewallregel die u hebt gemaakt, wordt standaard in JSON-indeling. Als er een fout is, wordt de uitvoer in plaats daarvan een foutbericht weergegeven.

## <a name="update-firewall-rule"></a>Update-firewallregel 
Bijwerken van een bestaande firewallregel op de server met [az postgres server firewall-rule update](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_update) opdracht. Geef de naam van de bestaande firewallregel als invoer en het begin IP- en IP-kenmerken om bij te werken.
```azurecli-interactive
az postgres server firewall-rule update --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.0
```
Implementatie is geslaagd bevat de uitvoer van de opdracht de details van de firewallregel die u hebt bijgewerkt, wordt standaard in JSON-indeling. Als er een fout is, wordt de uitvoer in plaats daarvan een foutbericht weergegeven.
> [!NOTE]
> Als de firewallregel niet bestaat, wordt deze gemaakt met de opdracht update.

## <a name="show-firewall-rule-details"></a>Firewall regeldetails weergeven
U kunt de details van een bestaande firewallregel op serverniveau ook weergeven door te voeren [az postgres server firewall-rule show](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_show) opdracht.
```azurecli-interactive
az postgres server firewall-rule show --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange
```
Implementatie is geslaagd bevat de uitvoer van de opdracht de details van de firewallregel die u hebt opgegeven, wordt standaard in JSON-indeling. Als er een fout is, wordt de uitvoer in plaats daarvan een foutbericht weergegeven.

## <a name="delete-firewall-rule"></a>Firewallregel verwijderen
Als u wilt intrekken van toegang voor een IP-adresbereik op de server, verwijdert u een bestaande firewallregel door het uitvoeren van de [az postgres server firewall-regel verwijderen](/cli/azure/postgres/server/firewall-rule) opdracht. Geef de naam van de bestaande firewallregel.
```azurecli-interactive
az postgres server firewall-rule delete --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange
```
Implementatie is geslaagd wordt er geen uitvoer. Als de mislukt, wordt de tekst van foutberichten geretourneerd.

## <a name="next-steps"></a>Volgende stappen
- Op deze manier kunt u een webbrowser naar [maken en beheren van Azure Database voor PostgreSQL-firewallregels met behulp van de Azure-portal](howto-manage-firewall-using-portal.md).
- Meer inzicht [Azure Database for PostgreSQL-Server firewall-regels](concepts-firewall-rules.md).
- Zie voor hulp bij het verbinding maken met een Azure Database for PostgreSQL-server, [verbindingsbibliotheken voor Azure Database for PostgreSQL](concepts-connection-libraries.md).
