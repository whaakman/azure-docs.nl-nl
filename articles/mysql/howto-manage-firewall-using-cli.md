---
title: Maken en beheren van Azure Database voor MySQL-firewallregels met behulp van Azure CLI
description: In dit artikel wordt beschreven hoe u maken en beheren van Azure Database voor MySQL-firewallregels met behulp van Azure CLI-opdrachtregel.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 45df284d29ea2d5eb799697b22deeab03cb66622
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956647"
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-by-using-the-azure-cli"></a>Maken en beheren van Azure Database voor MySQL-firewallregels met behulp van de Azure CLI
Firewallregels op serverniveau kunnen beheerders toegang tot een Azure Database voor MySQL-Server beheren vanaf een specifiek IP-adres of een bereik van IP-adressen. Met behulp van handige Azure CLI-opdrachten, kunt u maken, bijwerken, verwijderen, lijst, en firewallregels voor het beheren van uw server weergeven. Zie voor een overzicht van Azure Database voor MySQL-firewalls, [Azure Database for MySQL server firewall-regels](./concepts-firewall-rules.md)

## <a name="prerequisites"></a>Vereisten
* [Installeer Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).
* Een [Azure Database for MySQL-server en database](quickstart-create-mysql-server-database-using-azure-cli.md).

## <a name="firewall-rule-commands"></a>Firewall-regel opdrachten:
De **az mysql server firewall-rule** opdracht van de Azure CLI wordt gebruikt voor het maken, verwijderen, vermelden, weergeven en firewallregels bijwerken.

Opdrachten:
- **Maak**: een Azure MySQL server firewall-regel maken.
- **Verwijder**: een Azure MySQL server firewall-regel verwijderen.
- **lijst met**: overzicht van de Azure MySQL server firewall-regels.
- **weergeven**: de details van een Azure MySQL server firewall-regel weergeven.
- **Update**: bijwerken van een Azure MySQL server firewall-regel.

## <a name="log-in-to-azure-and-list-your-azure-database-for-mysql-servers"></a>Meld u aan bij Azure en de lijst met uw Azure Database voor MySQL-Servers
Veilig verbinding maken met Azure CLI met uw Azure-account met behulp van de **az login** opdracht.

1. Voer de volgende opdracht uit vanaf de opdrachtregel:
```azurecli
az login
```
Met deze opdracht voert een die moet worden gebruikt in de volgende stap.

2. Gebruik een webbrowser om de pagina te openen [ https://aka.ms/devicelogin ](https://aka.ms/devicelogin), en voer de code.

3. Meld u via het venster aan met uw Azure-referenties.

4. Nadat uw aanmelding is geautoriseerd, wordt een lijst met abonnementen in de console afgedrukt. Kopieer de ID van het gewenste abonnement om in te stellen van het huidige abonnement te gebruiken. Gebruik de [az account set](/cli/azure/account#az-account-set) opdracht.
   ```azurecli-interactive
   az account set --subscription <your subscription id>
   ```

5. Vraag de Databases op Azure voor MySQL-servers voor uw abonnement en de resourcegroep groep als u niet zeker van de namen bent. Gebruik de [az mysql serverlijst](/cli/azure/mysql/server#az-mysql-server-list) opdracht.

   ```azurecli-interactive
   az mysql server list --resource-group myresourcegroup
   ```

   Houd er rekening mee naamkenmerk in de aanbieding die u nodig hebt om op te geven van de MySQL-server voor gebruik op. Als het nodig is, controleert u of de details voor die server en het gebruik van het kenmerk name om te controleren of dat deze juist is. Gebruik de [az mysql server show](/cli/azure/mysql/server#az-mysql-server-show) opdracht.

   ```azurecli-interactive
   az mysql server show --resource-group myresourcegroup --name mydemoserver
   ```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a>Lijst met firewallregels voor Azure Database voor MySQL-Server 
Met de naam van de en naam van de resourcegroep, de bestaande server firewall-regels op de server weergeven. Gebruik de [lijst met az mysql server firewall](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-list) opdracht.  U ziet dat het kenmerk van de server name is opgegeven in de **--server** overschakelen en niet in de **--naam** overschakelen. 
```azurecli-interactive
az mysql server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver
```
De uitvoer bevat de regels, indien aanwezig, in JSON-indeling (standaard). U kunt de **--uitvoertabel** overschakelen naar de resultaten in een beter leesbare tabelindeling.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver --output table
```
## <a name="create-a-firewall-rule-on-azure-database-for-mysql-server"></a>Maak een firewallregel op Azure Database voor MySQL-Server
Met de naam van de Azure MySQL-server en de naam van de resourcegroep, een nieuwe firewallregel maken op de server. Gebruik de [az mysql serverfirewall maken](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-create) opdracht. Geef een naam voor de regel, evenals de begin-IP en laatste IP-adres (voor toegang tot een bereik van IP-adressen) voor de regel.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```

Als u wilt toestaan dat toegang voor één IP-adres, bevatten hetzelfde IP-adres als de eerste IP- en eind-IP-adres, zoals in dit voorbeeld.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```

Geef het IP-adres 0.0.0.0 als de eerste IP- en eind-IP-adres, zoals in dit voorbeeld zodat toepassingen vanuit Azure IP-adressen te verbinden met uw Azure Database voor MySQL-server.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server mysql --name "AllowAllWindowsAzureIps" --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!IMPORTANT]
> Met deze optie configureert u de firewall zo dat alle verbindingen vanuit Azure zijn toegestaan, inclusief verbindingen vanuit de abonnementen van andere klanten. Wanneer u deze optie selecteert, zorg dan dat uw aanmeldings- en gebruikersmachtigingen de toegang beperken tot alleen geautoriseerde gebruikers.
> 

Implementatie is geslaagd maakt elke opdracht uitvoer toont de details van de firewallregel die u hebt gemaakt, in JSON-indeling (standaard). Als er een fout is, ziet de uitvoer in plaats daarvan tekst van foutberichten.

## <a name="update-a-firewall-rule-on-azure-database-for-mysql-server"></a>Een firewallregel op Azure Database voor MySQL-server bijwerken 
Met de naam van de Azure MySQL-server en de naam van de resourcegroep, een bestaande firewallregel op de server worden bijgewerkt. Gebruik de [az mysql server firewall-update](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-update) opdracht. Geef de naam van de bestaande firewallregel als invoer, evenals het begin IP- en IP-kenmerken om bij te werken.
```azurecli-interactive
az mysql server firewall-rule update --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
Implementatie is geslaagd bevat de uitvoer van de opdracht de details van de firewallregel die u hebt bijgewerkt, in JSON-indeling (standaard). Als er een fout is, ziet de uitvoer in plaats daarvan tekst van foutberichten.

> [!NOTE]
> Als de firewallregel niet bestaat, wordt de regel wordt gemaakt door de opdracht update.

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a>Firewall regeldetails weergeven op Azure Database voor MySQL-Server
Met de naam van de Azure MySQL-server en de naam van de resourcegroep, de firewall van de bestaande regeldetails weergeven van de server. Gebruik de [az mysql server firewall show](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-show) opdracht. Geef de naam van de bestaande firewallregel als invoer.
```azurecli-interactive
az mysql server firewall-rule show --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1
```
Implementatie is geslaagd bevat de uitvoer van de opdracht de details van de firewallregel die u hebt opgegeven, in JSON-indeling (standaard). Als er een fout is, ziet de uitvoer in plaats daarvan tekst van foutberichten.

## <a name="delete-a-firewall-rule-on-azure-database-for-mysql-server"></a>Een firewallregel op Azure Database voor MySQL-Server verwijderen
Met de naam van de Azure MySQL-server en de naam van de resourcegroep, een bestaande firewallregel verwijderen van de server. Gebruik de [az mysql serverfirewall verwijderen](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-delete) opdracht. Geef de naam van de bestaande firewallregel.
```azurecli-interactive
az mysql server firewall-rule delete --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1
```
Implementatie is geslaagd wordt er geen uitvoer. Als de mislukt, tekst van foutberichten worden weergegeven.

## <a name="next-steps"></a>Volgende stappen
- Meer inzicht [Azure Database for MySQL Server firewall-regels](./concepts-firewall-rules.md).
- [Maken en beheren van Azure Database voor MySQL-firewallregels met behulp van de Azure-portal](./howto-manage-firewall-using-portal.md).
