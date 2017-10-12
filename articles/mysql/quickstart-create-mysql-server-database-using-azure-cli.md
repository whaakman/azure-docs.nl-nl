---
title: 'Quick Start: een Azure-database voor MySQL-server maken - Azure CLI | Microsoft Docs'
description: In deze Quick Start wordt beschreven hoe u Azure CLI gebruikt om een Azure-database voor MySQL-server in een Azure-resourcegroep te maken.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: hero-article
ms.date: 06/13/2017
ms.custom: mvc
ms.openlocfilehash: 3d66efa6935150c665a3f568e60c35ddbd70e8be
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-azure-database-for-mysql-server-using-azure-cli"></a>Een Azure-database voor MySQL-server maken met behulp van Azure CLI
In deze Quick Start wordt beschreven hoe u Azure CLI gebruikt om binnen ongeveer vijf minuten een Azure-database voor MySQL-server in een Azure-resourcegroep te maken. De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

Als u meerdere abonnementen hebt, kiest u het abonnement waarin de resource is opgenomen of wordt gefactureerd. Selecteer een specifiek abonnements-ID in uw account met de opdracht [az account set](/cli/azure/account#set).
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een [Azure-resourcegroep](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) met de opdracht [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd.

In het volgende voorbeeld wordt een resourcegroep met de naam `myresourcegroup` gemaakt op de locatie `westus`.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Een Azure-database voor MySQL-server maken
Als u een Azure-database voor MySQL-server wilt maken, gebruikt u de opdracht **az mysql server create**. Een server kan meerdere databases beheren. Een aparte database wordt doorgaans gebruikt voor elk project of voor elke gebruiker.

In het volgende voorbeeld wordt een Azure-database voor MySQL-server gemaakt die zich in `westus` bevindt in de resourcegroep `myresourcegroup` met de naam `myserver4demo`. De server heeft aanmeldgegevens voor de beheerder met de naam `myadmin` en het wachtwoord `Password01!`. De server wordt gemaakt met de **Basic**-prestatielaag en **50** rekeneenheden die tussen alle databases op de server worden gedeeld. U kunt berekeningen en opslag omhoog of omlaag schalen afhankelijk van de behoeften van de toepassing.

```azurecli-interactive
az mysql server create --resource-group myresourcegroup --name myserver4demo --location westus --admin-user myadmin --admin-password Password01! --performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a>Firewallregel configureren
Maak een Azure-database-firewallregel op MySQL-serverniveau met de opdracht **az mysql server firewall-rule create**. Een firewallregel op serverniveau maakt een externe toepassing mogelijk, zoals het opdrachtregelprogramma **mysql.exe** of MySQL Workbench om verbinding te maken met uw server via de MySQL-servicefirewall van Azure. 

In het volgende voorbeeld wordt een firewallregel gemaakt voor een vooraf gedefinieerd adresbereik. In dit voorbeeld is dit het volledige bereik van IP-adressen.

```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server myserver4demo --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
## <a name="configure-ssl-settings"></a>SSL-instellingen configureren
Standaard worden SSL-verbindingen tussen uw server en clienttoepassingen afgedwongen.  Hiermee wordt voor beveiliging van gegevens 'in beweging' gezorgd door de gegevensstroom via internet t versleutelen.  Om deze Quickstart makkelijker te maken, schakelen we SSL-verbindingen uit voor uw server.  Dit wordt afgeraden voor productieservers.  Zie [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](./howto-configure-ssl.md) (SSL-connectiviteit in uw toepassing configureren om veilig verbinding te maken met Azure-database voor MySQL) voor meer informatie.

In het volgende voorbeeld wordt het afdwingen van SSL op uw MySQL-server uitgeschakeld.
 
 ```azurecli-interactive
 az mysql server update --resource-group myresourcegroup --name myserver4demo -g -n --ssl-enforcement Disabled
 ```

## <a name="get-the-connection-information"></a>De verbindingsgegevens ophalen

Als u verbinding met uw server wilt maken, moet u hostgegevens en toegangsreferenties opgeven.

```azurecli-interactive
az mysql server show --resource-group myresourcegroup --name myserver4demo
```

Het resultaat wordt in JSON-indeling weergegeven. Noteer de **fullyQualifiedDomainName** en de **administratorLogin**.
```json
{
  "administratorLogin": "myadmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "myserver4demo.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforMySQL/servers/myserver4demo",
  "location": "westus",
  "name": "myserver4demo",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 50,
    "family": null,
    "name": "MYSQLS2M50",
    "size": null,
    "tier": "Basic"
  },
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforMySQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

## <a name="connect-to-the-server-using-the-mysqlexe-command-line-tool"></a>Verbinding maken met de server met het opdrachtregelprogramma mysql.exe
Maak verbinding met de server met behulp van het opdrachtregelprogramma **mysql.exe**. U kunt MySQL [hier](https://dev.mysql.com/downloads/) downloaden en vervolgens op uw computer installeren. In plaats daarvan kunt u ook op de knop **Try It** klikken in codevoorbeelden, of op `>_` op de werkbalk rechtsboven in Azure Portal, en de **Azure Cloud Shell** starten.

Typ de volgende opdrachten: 

1. Maak verbinding met de server met behulp van het opdrachtregelprogramma **mysql**:
```azurecli-interactive
 mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

2. Bekijk de status van de server:
```sql
 mysql> status
```
Als alles goed gaat, geeft het opdrachtregelprogramma de volgende tekst weer:

```dos
C:\Users\>mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
Enter password: ***********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65512
Server version: 5.6.26.0 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.6.35, for Win64 (x86_64)

Connection id:          65512
Current database:
Current user:           myadmin@116.230.243.143
SSL:                    Not in use
Using delimiter:        ;
Server version:         5.6.26.0 MySQL Community Server (GPL)
Protocol version:       10
Connection:             myserver4demo.mysql.database.azure.com via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    gbk
Conn.  characterset:    gbk
TCP port:               3306
Uptime:                 2 days 9 hours 47 min 20 sec

Threads: 4  Questions: 34833  Slow queries: 2  Opens: 84  Flush tables: 4  Open tables: 1  Queries per second avg: 0.167
--------------

mysql>
```

> [!TIP]
> Zie [hoofdstuk 4.5.1 in de Engelstalige naslaghandleiding van MySQL 5.7](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) voor aanvullende opdrachten.

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a>Maak verbinding met de server met het MySQL Workbench GUI-hulpprogramma
1.  Start de toepassing MySQL Workbench op uw clientcomputer. U kunt MySQL Workbench [hier](https://dev.mysql.com/downloads/workbench/) downloaden en installeren.

2.  In het dialoogvenster **Nieuwe verbinding instellen** voert u de volgende gegevens in op het tabblad **Parameters**:

   ![nieuwe verbinding instellen](./media/quickstart-create-mysql-server-database-using-azure-cli/setup-new-connection.png)

| **Instelling** | **Voorgestelde waarde** | **Beschrijving** |
|---|---|---|
|   Verbindingsnaam | Mijn verbinding | Geef een label op voor deze verbinding (dit kan van alles zijn) |
| Verbindingsmethode | kies Standard (TCP/IP) | TCP/IP-protocol gebruiken om verbinding te maken met Azure Database voor MySQL> |
| Hostnaam | myserver4demo.mysql.database.azure.com | De servernaam die u eerder hebt genoteerd. |
| Poort | 3306 | De standaardpoort voor MySQL wordt gebruikt. |
| Gebruikersnaam | myadmin@myserver4demo | De aanmeldgegevens van een serverbeheerder die u eerder hebt genoteerd. |
| Wachtwoord | **** | Gebruik het wachtwoord voor het beheerdersaccount dat u eerder hebt geconfigureerd. |

Klik op **Verbinding testen** om te controleren of alle parameters juist zijn geconfigureerd.
U kunt nu op de verbinding klikken om verbinding te maken met de server.

## <a name="clean-up-resources"></a>Resources opschonen
Als u deze resources niet voor een andere Quickstart of zelfstudie nodig hebt, kunt u ze verwijderen met de volgende opdracht: 

```azurecli-interactive
az group delete --name myresourcegroup
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een MySQL-database ontwerpen met Azure CLI](./tutorial-design-database-using-cli.md)
