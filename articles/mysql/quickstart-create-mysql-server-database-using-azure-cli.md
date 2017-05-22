---
title: 'Quick Start: een Azure-database voor MySQL-server maken - Azure CLI | Microsoft Docs'
description: In deze Quick Start wordt beschreven hoe u Azure CLI gebruikt om een Azure-database voor MySQL-server in een Azure-resourcegroep te maken.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: mysql-database
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: portal
ms.workload: 
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 574299dd64120d75a1a36cb2ded0fdd269292570
ms.contentlocale: nl-nl
ms.lasthandoff: 05/10/2017

---

# <a name="create-an-azure-database-for-mysql-server-using-azure-cli"></a>Een Azure-database voor MySQL-server maken met behulp van Azure CLI
In deze Quick Start wordt beschreven hoe u Azure CLI gebruikt om binnen ongeveer vijf minuten een Azure-database voor MySQL-server in een Azure-resourcegroep te maken. De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts.

Controleer of de nieuwste versie van [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) is geïnstalleerd om deze Quick Start te voltooien. 

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht [az login](/cli/azure/#login) en volg de instructies op het scherm.

```azurecli
az login
```
Volg de instructies in de opdrachtprompt om https://aka.ms/devicelog in uw browser te openen en voer de code in die in de **opdrachtprompt** wordt gegenereerd.

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een [Azure-resourcegroep](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) met de opdracht [az group create](https://docs.microsoft.com/cli/azure/group#create). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd.

In het volgende voorbeeld wordt een resourcegroep met de naam `mycliresource` gemaakt op de locatie `westus`.

```azurecli
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Een Azure-database voor MySQL-server maken
Als u een Azure-database voor MySQL-server wilt maken, gebruikt u de opdracht **az mysql server create**. Een server kan meerdere databases beheren. Een aparte database wordt doorgaans gebruikt voor elk project of voor elke gebruiker.

In het volgende voorbeeld wordt een Azure-database voor MySQL-server gemaakt die zich in `westus` bevindt in de resourcegroep `mycliresource` met de naam `mycliserver`. De server heeft aanmeldgegevens voor de beheerder met de naam `myadmin` en het wachtwoord `Password01!`. De server wordt gemaakt met de **Basic**-prestatielaag en **50** rekeneenheden die tussen alle databases op de server worden gedeeld. U kunt berekeningen en opslag omhoog of omlaag schalen afhankelijk van de behoeften van de toepassing.

```azurecli
az mysql server create --resource-group mycliresource --name mycliserver--location westus --user myadmin --password Password01! --performance-tier Basic --compute-units 50
```

![Een Azure-database voor MySQL-server maken met behulp van Azure CLI](./media/quickstart-create-mysql-server-database-using-azure-cli/3_az-mysq-server-create.png)

## <a name="configure-firewall-rule"></a>Firewallregel configureren
Maak een Azure-database-firewallregel op MySQL-serverniveau met de opdracht **az mysql server firewall-rule create**. Een firewallregel op serverniveau maakt een externe toepassing mogelijk, zoals het opdrachtregelprogramma **mysql.exe** of MySQL Workbench om verbinding te maken met uw server via de MySQL-servicefirewall van Azure. 

In het volgende voorbeeld wordt een firewallregel gemaakt voor een vooraf gedefinieerd adresbereik. In dit voorbeeld is dit het volledige bereik van IP-adressen.

```azurecli
az mysql server firewall-rule create --resource-group mycliresource --server mycliserver --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
## <a name="configure-ssl-settings"></a>SSL-instellingen configureren
Standaard worden SSL-verbindingen tussen uw server en clienttoepassingen afgedwongen.  Hiermee wordt voor beveiliging van gegevens 'in beweging' gezorgd door de gegevensstroom via internet t versleutelen.  We schakelen SSL-verbindingen voor uw server uit om deze Quick Start gemakkelijk te maken.  Dit wordt niet aangeraden voor productieservers.  Zie [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](./howto-configure-ssl.md) (SSL-connectiviteit in uw toepassing configureren om veilig verbinding te maken met Azure-database voor MySQL) voor meer informatie.

In het volgende voorbeeld wordt het afdwingen van SSL op uw MySQL-server uitgeschakeld.
 
 ```azurecli
 az mysql server update --resource-group mycliresource --name mycliserver -g -n --ssl-enforcement Disabled
 ```

## <a name="get-the-connection-information"></a>De verbindingsgegevens ophalen

Als u verbinding met uw server wilt maken, moet u hostgegevens en toegangsreferenties opgeven.

```azurecli
az mysql server show --resource-group mycliresource --name mycliserver
```

Het resultaat wordt in JSON-indeling weergegeven. Noteer de **fullyQualifiedDomainName** en de **administratorLogin**.
```json
{
  "administratorLogin": "myadmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mycliserver.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mycliresource/providers/Microsoft.DBforMySQL/servers/mycliserver",
  "location": "westus",
  "name": "mycliserver",
  "resourceGroup": "mycliresource",
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
Als u verbinding wilt maken met uw server met behulp van het opdrachtregelprogramma **mysql.exe**, zorgt u ervoor dat u MySQL op uw computer hebt geïnstalleerd.  U kunt MySQL [hier](https://dev.mysql.com/downloads/) downloaden.

Open de opdrachtprompt en voer het volgende in: 

1. Maak verbinding met de server met behulp van het opdrachtregelprogramma **mysql**:
```dos
 mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

2. Bekijk de status van de server:
```dos
 mysql> status
```
Als alles goed gaat, geeft het opdrachtregelprogramma de volgende uitvoer weer:

```dos
C:\Users\v-chenyh>mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
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
Connection:             mycliserver.database.windows.net via TCP/IP
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
> Zie [MySQL 5.6 Reference Manual - Chapter 4.5.1](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) (MySQL 5.6-handleiding, hoofdstuk 4.5.1) voor aanvullende opdrachten.

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a>Verbinding maken met de server met het MySQL Workbench GUI-hulpprogramma
1.    Start de toepassing MySQL Workbench op uw clientcomputer. U kunt MySQL Workbench [hier](https://dev.mysql.com/downloads/workbench/) downloaden en installeren.

2.    In het dialoogvenster **Nieuwe verbinding instellen** voert u de volgende gegevens in op het tabblad **Parameters**:

| **Parameters** | **Beschrijving** |
|----------------|-----------------|
|    *Verbindingsnaam* | geef een naam op voor deze verbinding (dit kan van alles zijn) |
| *Verbindingsmethode* | kies Standard (TCP/IP) |
| *Hostnaam* | mycliserver.database.windows.net (SERVERNAAM die u eerder hebt genoteerd) |
| *Poort* | 3306 |
| *Gebruikersnaam* | myadmin@mycliserver (AANMELDGEGEVENS VAN SERVERBEHEERDER die u eerder hebt genoteerd) |
| *Wachtwoord* | u kunt het wachtwoord van het beheerdersaccount in de kluis opslaan |

![nieuwe verbinding instellen](./media/quickstart-create-mysql-server-database-using-azure-cli/setup-new-connection.png)

3.    Klik op **Verbinding testen** om te controleren of alle parameters juist zijn geconfigureerd.

4.    U kunt nu op de verbinding klikken die zojuist is gemaakt om verbinding te maken met de server.

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze resources niet voor een andere Quick Start/zelfstudie nodig hebt, kunt u ze verwijderen door het volgende te doen: 

```azurecli
az group delete --name mycliresource
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een MySQL-database ontwerpen met Azure CLI](./tutorial-design-database-using-cli.md)

