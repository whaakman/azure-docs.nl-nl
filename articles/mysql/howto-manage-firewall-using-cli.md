---
title: Maken en beheren van Azure-Database voor firewallregels MySQL met Azure CLI | Microsoft Docs
description: In dit artikel wordt beschreven hoe maken en beheren van Azure-Database voor firewallregels MySQL met Azure CLI-opdrachtregel.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 01/18/2018
ms.openlocfilehash: 1738fdd85391135357d34fefa878538866f21b91
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-by-using-the-azure-cli"></a>Maken en beheren van Azure-Database voor firewallregels MySQL met behulp van de Azure CLI
Firewallregels op serverniveau kunnen beheerders toegang tot een Azure-Database voor de MySQL-Server beheren vanaf een specifiek IP-adres of een bereik met IP-adressen. U handige Azure CLI-opdrachten gebruikt, kunt u maken, bijwerken, verwijderen, de lijst en firewallregels voor het beheren van uw server weergegeven. Zie voor een overzicht van Azure-Database voor MySQL firewalls [Azure Database voor de MySQL-firewallregels voor server](./concepts-firewall-rules.md)

## <a name="prerequisites"></a>Vereisten
* [Installeer Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).
* Installeer de Azure Python SDK voor PostgreSQL en MySQL-Services.
* Installeer de Azure CLI-onderdeel voor PostgreSQL en MySQL-services.
* Maak een Azure-Database voor de MySQL-server.

## <a name="firewall-rule-commands"></a>Firewall-regel opdrachten:
De **az mysql server-firewallregel** opdracht wordt gebruikt met Azure CLI voor het maken, verwijderen, lijst, weergeven en bijwerken van firewallregels.

Opdrachten:
- **Maak**: een firewallregel op Azure MySQL-server maken.
- **verwijderen**: verwijderen van een firewallregel voor Azure MySQL-server.
- **lijst**: lijst van de firewallregels voor Azure MySQL-server.
- **weergeven**: de details van een server op Azure MySQL firewallregel weergeven.
- **bijwerken**: een firewallregel voor Azure MySQL-server bijwerken.

## <a name="log-in-to-azure-and-list-your-azure-database-for-mysql-servers"></a>Aanmelden bij Azure en de lijst met uw Azure-Database voor MySQL-Servers
Azure CLI veilig verbinding met uw Azure-account met behulp van de **az aanmelding** opdracht.

1. Voer de volgende opdracht vanaf de opdrachtregel:
```azurecli
az login
```
Met deze opdracht wordt de uitvoer een code te gebruiken in de volgende stap.

2. Een webbrowser gebruiken om de pagina te openen [https://aka.ms/devicelogin](https://aka.ms/devicelogin), en voer de code.

3. Meld u via het venster aan met uw Azure-referenties.

4. Nadat uw aanmelding is geautoriseerd, wordt een lijst met abonnementen in de console afgedrukt. Kopieer de ID van het gewenste abonnement voor het instellen van het huidige abonnement gebruiken. Gebruik de [az account set](/cli/azure/account#az_account_set) opdracht.
   ```azurecli-interactive
   az account set --subscription <your subscription id>
   ```

5. Lijst van de Azure-Databases voor een MySQL-servers voor uw abonnement en de resource-groep als u niet zeker van de namen bent. Gebruik de [az mysql serverlijst](/cli/azure/mysql/server#az_mysql_server_list) opdracht.

   ```azurecli-interactive
   az mysql server list --resource-group myResourceGroup
   ```

   Let op het kenmerk name in de aanbieding, u de MySQL-server om te werken moet op opgeven. Indien nodig, Controleer de details voor die server en het gebruik van het kenmerk name om te controleren of dat deze juist is. Gebruik de [az mysql server weergeven](/cli/azure/mysql/server#az_mysql_server_show) opdracht.

   ```azurecli-interactive
   az mysql server show --resource-group myResourceGroup --name mysqlserver4demo
   ```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a>Lijst met firewallregels voor Azure-Database voor de MySQL-Server 
Met de naam van de server en de naam van de resourcegroep, lijst van de bestaande firewallregels voor server op de server. Gebruik de [az mysql serverlijst firewall](/cli/azure/mysql/server/firewall-rule#az_mysql_server_firewall_rule_list) opdracht.  U ziet dat het kenmerk server name is opgegeven in de **--server** switch en niet in de **--naam** overschakelen. 
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server-name mysqlserver4demo
```
De uitvoer bevat de regels, indien aanwezig, in JSON (standaard formatteren). U kunt de **--uitvoertabel** overschakelen naar de resultaten in een beter leesbare tabelindeling.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server-name mysqlserver4demo --output table
```
## <a name="create-a-firewall-rule-on-azure-database-for-mysql-server"></a>Een firewallregel maken in Azure-Database voor de MySQL-Server
Met de naam van de Azure MySQL-server en de naam van de resourcegroep, een nieuwe firewallregel maken op de server. Gebruik de [az mysql serverfirewall maken](/cli/azure/mysql/server/firewall-rule#az_mysql_server_firewall_rule_create) opdracht. Geef een naam voor de regel, evenals de begin-IP en eindigen IP (voor het bieden van toegang tot een bereik met IP-adressen) voor de regel.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup --server-name mysqlserver4demo --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```

Om toegang te verlenen voor een enkel IP-adres, bieden de hetzelfde IP-adres als de eerste IP- en eind-IP, zoals in dit voorbeeld.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup --server-name mysqlserver4demo --name FirewallRule1 --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```

Geef het IP-adres 0.0.0.0 als de eerste IP- en eind-IP, zoals in dit voorbeeld zodat toepassingen van Azure IP-adressen te verbinden met uw Azure-Database voor de MySQL-server.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  
--server mysql --name "AllowAllWindowsAzureIps" --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!IMPORTANT]
> Met deze optie configureert u de firewall zo dat alle verbindingen vanuit Azure zijn toegestaan, inclusief verbindingen vanuit de abonnementen van andere klanten. Wanneer u deze optie selecteert, zorg dan dat uw aanmeldings- en gebruikersmachtigingen de toegang beperken tot alleen geautoriseerde gebruikers.
> 

Als dit lukt, moet elke opdracht uitvoer bevat de details van de firewallregel die u hebt gemaakt, in JSON-indeling (standaard) maken. Als er een storing, bevat de uitvoer van de tekst van het foutbericht in plaats daarvan.

## <a name="update-a-firewall-rule-on-azure-database-for-mysql-server"></a>Een firewallregel op Azure-Database voor de MySQL-server bijwerken 
Met de naam van de Azure MySQL-server en de naam van de resourcegroep, een bestaande firewallregel op de server worden bijgewerkt. Gebruik de [az mysql serverupdate firewall](/cli/azure/mysql/server/firewall-rule#az_mysql_server_firewall_rule_update) opdracht. Geef de naam van de bestaande firewallregel als invoer, evenals het begin IP- en end IP-kenmerken om bij te werken.
```azurecli-interactive
az mysql server firewall-rule update --resource-group myResourceGroup --server-name mysqlserver4demo --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
Als dit lukt bevat de uitvoer van de opdracht de details van de firewallregel die u hebt bijgewerkt, in JSON-indeling (standaard). Als er een storing, bevat de uitvoer van de tekst van het foutbericht in plaats daarvan.

> [!NOTE]
> Als de firewallregel die niet bestaat, wordt de regel wordt gemaakt door de opdracht update.

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a>Firewall regeldetails weergeven in Azure-Database voor de MySQL-Server
Met de naam van de Azure MySQL-server en de naam van de resourcegroep, de bestaande firewall regeldetails weergegeven van de server. Gebruik de [az mysql server firewall weergeven](/cli/azure/mysql/server/firewall-rule#az_mysql_server_firewall_rule_show) opdracht. Geef de naam van de bestaande firewallregel als invoer.
```azurecli-interactive
az mysql server firewall-rule show --resource-group myResourceGroup --server-name mysqlserver4demo --name FirewallRule1
```
Als dit lukt bevat de uitvoer van de opdracht de details van de firewallregel die u hebt opgegeven, in JSON-indeling (standaard). Als er een storing, bevat de uitvoer van de tekst van het foutbericht in plaats daarvan.

## <a name="delete-a-firewall-rule-on-azure-database-for-mysql-server"></a>Een firewallregel op Azure-Database voor de MySQL-Server verwijderen
Met de naam van de Azure MySQL-server en de naam van de resourcegroep, een bestaande firewallregel verwijderen van de server. Gebruik de [az mysql serverfirewall verwijderen](/cli/azure/mysql/server/firewall-rule#az_mysql_server_firewall_rule_delete) opdracht. Geef de naam van de bestaande firewallregel.
```azurecli-interactive
az mysql server firewall-rule delete --resource-group myResourceGroup --server-name mysqlserver4demo --name FirewallRule1
```
Als dit lukt wordt er geen uitvoer. Bij storingen tekst van het foutbericht wordt weergegeven.

## <a name="next-steps"></a>Volgende stappen
- Meer inzicht te geven over [Azure Database voor firewallregels voor MySQL Server](./concepts-firewall-rules.md).
- [Maken en beheren van Azure-Database voor firewallregels MySQL met de Azure portal](./howto-manage-firewall-using-portal.md).
