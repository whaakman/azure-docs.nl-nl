---
title: Maken en beheren van Azure Database voor MariaDB-firewallregels met behulp van Azure CLI
description: In dit artikel wordt beschreven hoe u maken en beheren van Azure Database voor MariaDB-firewallregels met behulp van Azure CLI-opdrachtregel.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 11/10/2018
ms.openlocfilehash: f457246b429062625e6542cfdaf00f3526a85209
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58083229"
---
# <a name="create-and-manage-azure-database-for-mariadb-firewall-rules-by-using-the-azure-cli"></a>Maken en beheren van Azure Database voor MariaDB-firewallregels met behulp van de Azure CLI
Firewallregels op serverniveau kunnen beheerders toegang tot een Azure Database voor MariaDB-Server beheren vanaf een specifiek IP-adres of een bereik van IP-adressen. Met behulp van handige Azure CLI-opdrachten, kunt u maken, bijwerken, verwijderen, lijst, en firewallregels voor het beheren van uw server weergeven. Zie voor een overzicht van Azure Database voor MariaDB firewalls, [Azure Database voor MariaDB server firewall-regels](./concepts-firewall-rules.md)

## <a name="prerequisites"></a>Vereisten
* [Installeer Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).
* Een [Azure Database voor MariaDB-server en database](quickstart-create-mariadb-server-database-using-azure-cli.md).

## <a name="firewall-rule-commands"></a>Firewall-regel opdrachten:
De **az mariadb server firewall-rule** opdracht van de Azure CLI wordt gebruikt voor het maken, verwijderen, vermelden, weergeven en firewallregels bijwerken.

Opdrachten:
- **Maak**: Maak een firewallregel voor Azure MariaDB-server.
- **Verwijder**: Een Azure MariaDB server firewall-regel verwijderen.
- **Lijst met**: Lijst met de Azure-MariaDB server firewall-regels.
- **weergeven**: De details van een Azure-MariaDB-server firewall-regel weergeven.
- **Update**: Een firewallregel voor Azure MariaDB-server bijwerken.

## <a name="log-in-to-azure-and-list-your-azure-database-for-mariadb-servers"></a>Meld u aan bij Azure en de lijst met uw Azure Database voor MariaDB-Servers
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

5. Vraag de Databases op Azure voor MariaDB-servers voor uw abonnement en de resourcegroep groep als u niet zeker van de namen bent. Gebruik de [az mariadb-serverlijst](/cli/azure/mariadb/server#az-mariadb-server-list) opdracht.

   ```azurecli-interactive
   az mariadb server list --resource-group myresourcegroup
   ```

   Houd er rekening mee het naamkenmerk in het overzicht, u opgeven de MariaDB-server moet voor gebruik op. Als het nodig is, controleert u of de details voor die server en het gebruik van het kenmerk name om te controleren of dat deze juist is. Gebruik de [az mariadb server show](/cli/azure/mariadb/server#az-mariadb-server-show) opdracht.

   ```azurecli-interactive
   az mariadb server show --resource-group myresourcegroup --name mydemoserver
   ```

## <a name="list-firewall-rules-on-azure-database-for-mariadb-server"></a>Lijst met firewallregels voor Azure Database voor MariaDB-Server 
Met de naam van de en naam van de resourcegroep, de bestaande server firewall-regels op de server weergeven. Gebruik de [az mariadb-serverlijst firewall](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-list) opdracht.  U ziet dat het kenmerk van de server name is opgegeven in de **--server** overschakelen en niet in de **--naam** overschakelen. 
```azurecli-interactive
az mariadb server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver
```
De uitvoer bevat de regels, indien aanwezig, in JSON-indeling (standaard). U kunt de **--uitvoertabel** overschakelen naar de resultaten in een beter leesbare tabelindeling.
```azurecli-interactive
az mariadb server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver --output table
```
## <a name="create-a-firewall-rule-on-azure-database-for-mariadb-server"></a>Maak een firewallregel op Azure Database voor MariaDB-Server
Met de naam van de Azure-MariaDB-server en de naam van de resourcegroep, een nieuwe firewallregel maken op de server. Gebruik de [az mariadb-serverfirewall maken](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-create) opdracht. Geef een naam voor de regel, evenals de begin-IP en laatste IP-adres (voor toegang tot een bereik van IP-adressen) voor de regel.
```azurecli-interactive
az mariadb server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```

Als u wilt toestaan dat toegang voor één IP-adres, bevatten hetzelfde IP-adres als de eerste IP- en eind-IP-adres, zoals in dit voorbeeld.
```azurecli-interactive
az mariadb server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```

Geef het IP-adres 0.0.0.0 als de eerste IP- en eind-IP-adres, zoals in dit voorbeeld zodat toepassingen vanuit Azure IP-adressen te verbinden met uw Azure Database voor MariaDB-server.
```azurecli-interactive
az mariadb server firewall-rule create --resource-group myresourcegroup --server mariadb --name "AllowAllWindowsAzureIps" --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!IMPORTANT]
> Met deze optie configureert u de firewall zo dat alle verbindingen vanuit Azure zijn toegestaan, inclusief verbindingen vanuit de abonnementen van andere klanten. Wanneer u deze optie selecteert, zorg dan dat uw aanmeldings- en gebruikersmachtigingen de toegang beperken tot alleen geautoriseerde gebruikers.
> 

Implementatie is geslaagd maakt elke opdracht uitvoer toont de details van de firewallregel die u hebt gemaakt, in JSON-indeling (standaard). Als er een fout is, ziet de uitvoer in plaats daarvan tekst van foutberichten.

## <a name="update-a-firewall-rule-on-azure-database-for-mariadb-server"></a>Een firewallregel op Azure Database voor MariaDB-server bijwerken 
Met de naam van de Azure-MariaDB-server en de naam van de resourcegroep, een bestaande firewallregel op de server worden bijgewerkt. Gebruik de [az mariadb serverupdate firewall](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-update) opdracht. Geef de naam van de bestaande firewallregel als invoer, evenals het begin IP- en IP-kenmerken om bij te werken.
```azurecli-interactive
az mariadb server firewall-rule update --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
Implementatie is geslaagd bevat de uitvoer van de opdracht de details van de firewallregel die u hebt bijgewerkt, in JSON-indeling (standaard). Als er een fout is, ziet de uitvoer in plaats daarvan tekst van foutberichten.

> [!NOTE]
> Als de firewallregel niet bestaat, wordt de regel wordt gemaakt door de opdracht update.

## <a name="show-firewall-rule-details-on-azure-database-for-mariadb-server"></a>Firewall regeldetails weergeven op Azure Database voor MariaDB-Server
Met de naam van de Azure-MariaDB-server en de naam van de resourcegroep, de firewall van de bestaande regeldetails weergeven van de server. Gebruik de [az mariadb server firewall show](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-show) opdracht. Geef de naam van de bestaande firewallregel als invoer.
```azurecli-interactive
az mariadb server firewall-rule show --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1
```
Implementatie is geslaagd bevat de uitvoer van de opdracht de details van de firewallregel die u hebt opgegeven, in JSON-indeling (standaard). Als er een fout is, ziet de uitvoer in plaats daarvan tekst van foutberichten.

## <a name="delete-a-firewall-rule-on-azure-database-for-mariadb-server"></a>Een firewallregel op Azure Database voor MariaDB-Server verwijderen
Met de naam van de Azure-MariaDB-server en de naam van de resourcegroep, een bestaande firewallregel verwijderen van de server. Gebruik de [az mariadb-serverfirewall verwijderen](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-delete) opdracht. Geef de naam van de bestaande firewallregel.
```azurecli-interactive
az mariadb server firewall-rule delete --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1
```
Implementatie is geslaagd wordt er geen uitvoer. Als de mislukt, tekst van foutberichten worden weergegeven.

## <a name="next-steps"></a>Volgende stappen
- Meer inzicht [Azure Database voor MariaDB Server firewall-regels](./concepts-firewall-rules.md).
- [Maken en beheren van Azure Database voor MariaDB-firewallregels met behulp van de Azure-portal](./howto-manage-firewall-portal.md).
