---
title: 'Snelstart: een Azure Database for MySQL-server maken - Azure CLI'
description: In deze Quick Start wordt beschreven hoe u Azure CLI gebruikt om een Azure-database voor MySQL-server in een Azure-resourcegroep te maken.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.topic: quickstart
ms.date: 04/01/2018
ms.custom: mvc
ms.openlocfilehash: 3ccf2f2e6fbad6c1a1acc4b3e827a3072d8a0cb7
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36293812"
---
# <a name="create-an-azure-database-for-mysql-server-using-azure-cli"></a>Een Azure-database voor MySQL-server maken met behulp van Azure CLI
In deze Quick Start wordt beschreven hoe u Azure CLI gebruikt om binnen ongeveer vijf minuten een Azure-database voor MySQL-server in een Azure-resourcegroep te maken. De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

Als u meerdere abonnementen hebt, kiest u het abonnement waarin de resource is opgenomen of wordt gefactureerd. Selecteer een specifiek abonnements-ID in uw account met de opdracht [az account set](/cli/azure/account#az_account_set).
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een [Azure-resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) met de opdracht [az group create](/cli/azure/group#az_group_create). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd.

In het volgende voorbeeld wordt een resourcegroep met de naam `myresourcegroup` gemaakt op de locatie `westus`.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Een Azure-database voor MySQL-server maken
Maak een Azure Database for MySQL-server met de opdracht **[az mysql server create](/cli/azure/mysql/server#az_mysql_server_create)**. Een server kan meerdere databases beheren. Een aparte database wordt doorgaans gebruikt voor elk project of voor elke gebruiker.

In het volgende voorbeeld wordt een server in VS West gemaakt met de naam `mydemoserver` in uw resourcegroep `myresourcegroup` met aanmeldgegevens van de serverbeheerder `myadmin`. Dit is een **Gen 4**-server voor **Algemeen gebruik** met twee **vCores**. De naam van een server komt overeen met een DNS-naam en moet dus globaal uniek zijn in Azure. Vervang het `<server_admin_password>` door uw eigen waarde.
```azurecli-interactive
az mysql server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen4_2 --version 5.7
```

## <a name="configure-firewall-rule"></a>Firewallregel configureren
Maak een Azure Database for MySQL-firewallregel op serverniveau met de opdracht **[az mysql server firewall-rule create](/cli/azure/mysql/server/firewall-rule#az_mysql_server_firewall_rule_create)**. Een firewallregel op serverniveau maakt een externe toepassing mogelijk, zoals het opdrachtregelprogramma **mysql.exe** of MySQL Workbench om verbinding te maken met uw server via de MySQL-servicefirewall van Azure. 

In het volgende voorbeeld wordt een firewallregel met de naam `AllowMyIP` gemaakt, die verbindingen van een specifiek IP-adres, 192.168.0.1, toestaat. Vervang dit adres door het IP-adres of de reeks IP-adressen die overeenkomen met het IP-adres waarmee u verbinding gaat maken. 

```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server mydemoserver --name AllowMyIP --start-ip-address 192.168.0.1 --end-ip-address 192.168.0.1
```

> [!NOTE]
> Verbindingen met Azure Database voor MySQL communiceren via poort 3306. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 3306 mogelijk niet toegestaan. In dat geval kunt u alleen verbinding maken met uw server als uw IT-afdeling poort 3306 openstelt.
> 


## <a name="configure-ssl-settings"></a>SSL-instellingen configureren
Standaard worden SSL-verbindingen tussen uw server en clienttoepassingen afgedwongen. Deze standaardinstelling zorgt voor beveiliging van gegevens 'in beweging' door de gegevensstroom via internet te versleutelen. Om deze Quickstart makkelijker te maken, schakelen we SSL-verbindingen uit voor uw server. Dit wordt afgeraden voor productieservers. Zie [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](./howto-configure-ssl.md) (SSL-connectiviteit in uw toepassing configureren om veilig verbinding te maken met Azure-database voor MySQL) voor meer informatie.

In het volgende voorbeeld wordt het afdwingen van SSL op uw MySQL-server uitgeschakeld.
 
 ```azurecli-interactive
 az mysql server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Disabled
 ```

## <a name="get-the-connection-information"></a>De verbindingsgegevens ophalen

Als u verbinding met uw server wilt maken, moet u hostgegevens en toegangsreferenties opgeven.

```azurecli-interactive
az mysql server show --resource-group myresourcegroup --name mydemoserver
```

Het resultaat wordt in JSON-indeling weergegeven. Noteer de **fullyQualifiedDomainName** en de **administratorLogin**.
```json
{
  "administratorLogin": "myadmin",
  "earliestRestoreDate": null,
  "fullyQualifiedDomainName": "mydemoserver.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforMySQL/servers/mydemoserver",
  "location": "westus",
  "name": "mydemoserver",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 2,
    "family": "Gen4",
    "name": "GP_Gen4_2",
    "size": null,
    "tier": "GeneralPurpose"
  },
  "sslEnforcement": "Enabled",
  "storageProfile": {
    "backupRetentionDays": 7,
    "geoRedundantBackup": "Disabled",
    "storageMb": 5120
  },
  "tags": null,
  "type": "Microsoft.DBforMySQL/servers",
  "userVisibleState": "Ready",
  "version": "5.7"
}
```

## <a name="connect-to-the-server-using-the-mysqlexe-command-line-tool"></a>Verbinding maken met de server met het opdrachtregelprogramma mysql.exe
Maak verbinding met de server met behulp van het opdrachtregelprogramma **mysql.exe**. U kunt MySQL [hier](https://dev.mysql.com/downloads/) downloaden en vervolgens op uw computer installeren. In plaats daarvan kunt u ook op de knop **Try It** klikken in codevoorbeelden, of op `>_` op de werkbalk rechtsboven in Azure Portal, en de **Azure Cloud Shell** starten.

Typ de volgende opdrachten: 

1. Maak verbinding met de server met behulp van het opdrachtregelprogramma **mysql**:
```azurecli-interactive
 mysql -h mydemoserver.mysql.database.azure.com -u myadmin@mydemoserver -p
```

2. Bekijk de status van de server:
```sql
 mysql> status
```
Als alles goed gaat, geeft het opdrachtregelprogramma de volgende tekst weer:

```dos
C:\Users\>mysql -h mydemoserver.mysql.database.azure.com -u myadmin@mydemoserver -p
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
Connection:             mydemoserver.mysql.database.azure.com via TCP/IP
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
| Hostnaam | mydemoserver.mysql.database.azure.com | De servernaam die u eerder hebt genoteerd. |
| Poort | 3306 | De standaardpoort voor MySQL wordt gebruikt. |
| Gebruikersnaam | myadmin@mydemoserver | De aanmeldgegevens van een serverbeheerder die u eerder hebt genoteerd. |
| Wachtwoord | **** | Gebruik het wachtwoord voor het beheerdersaccount dat u eerder hebt geconfigureerd. |

Klik op **Verbinding testen** om te controleren of alle parameters juist zijn geconfigureerd.
U kunt nu op de verbinding klikken om verbinding te maken met de server.

## <a name="clean-up-resources"></a>Resources opschonen
Als u deze resources niet voor een andere Quickstart of zelfstudie nodig hebt, kunt u ze verwijderen met de volgende opdracht: 

```azurecli-interactive
az group delete --name myresourcegroup
```

Als u alleen de zojuist gemaakte server wilt verwijderen, kunt u de opdracht **[az mysql server delete](/cli/azure/mysql/server#az_mysql_server_delete)** uitvoeren.
```azurecli-interactive
az mysql server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een MySQL-database ontwerpen met Azure CLI](./tutorial-design-database-using-cli.md)
