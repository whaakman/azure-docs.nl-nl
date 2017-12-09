---
title: Ontwerp van uw eerste Azure-Database voor de MySQL-database. Azure CLI | Microsoft Docs
description: Deze zelfstudie wordt uitgelegd hoe maken en beheren van Azure-Database voor MySQL-server en database met behulp van Azure CLI 2.0 vanaf de opdrachtregel.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.topic: tutorial
ms.date: 11/28/2017
ms.custom: mvc
ms.openlocfilehash: 2d23c37688ab7f19beba920f7ddd4043cd117503
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>Ontwerp van uw eerste Azure-Database voor de MySQL-database

Azure MySQL-Database is een relationele database-service in de Microsoft-cloud op basis van de database-engine MySQL Community Edition. In deze zelfstudie maakt u Azure CLI (opdrachtregelinterface) en andere hulpprogramma's voor meer informatie over hoe:

> [!div class="checklist"]
> * Een Azure-Database maken voor MySQL
> * De serverfirewall configureren
> * Gebruik [mysql opdrachtregelprogramma](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) om een database te maken
> * Voorbeeldgegevens laden
> * Querygegevens
> * Gegevens bijwerken
> * Gegevens terugzetten

U kunt de Azure-Cloud-Shell gebruiken in de browser of [2.0 voor Azure CLI installeren]( /cli/azure/install-azure-cli) op uw eigen computer uitvoeren van de codeblokken in deze zelfstudie.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

Als u meerdere abonnementen hebt, kiest u het abonnement waarin de resource is opgenomen of wordt gefactureerd. Selecteer een specifiek abonnements-ID in uw account met de opdracht [az account set](/cli/azure/account#az_account_set).
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een [Azure-resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) met [az groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) opdracht. Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd.

In het volgende voorbeeld wordt een resourcegroep met de naam `mycliresource` gemaakt op de locatie `westus`.

```azurecli-interactive
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Een Azure-database voor MySQL-server maken
Een Azure-Database maken voor MySQL-server met de server van de mysql az opdracht maken. Een server kan meerdere databases beheren. Een aparte database wordt doorgaans gebruikt voor elk project of voor elke gebruiker.

In het volgende voorbeeld wordt een Azure-database voor MySQL-server gemaakt die zich in `westus` bevindt in de resourcegroep `mycliresource` met de naam `mycliserver`. De server heeft aanmeldgegevens voor de beheerder met de naam `myadmin` en het wachtwoord `Password01!`. De server wordt gemaakt met de **Basic**-prestatielaag en **50** rekeneenheden die tussen alle databases op de server worden gedeeld. U kunt berekeningen en opslag omhoog of omlaag schalen afhankelijk van de behoeften van de toepassing.

```azurecli-interactive
az mysql server create --resource-group mycliresource --name mycliserver --location westus --admin-user myadmin --admin-password Password01! --performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a>Firewallregel configureren
Een Azure-Database maken voor MySQL serverniveau firewallregel met de az mysql-serverfirewallregel opdracht maken. Een firewallregel op serverniveau kan een externe toepassing, zoals **mysql** opdrachtregelprogramma of MySQL Workbench verbinding maken met uw server via de firewall van de service Azure MySQL. 

Het volgende voorbeeld wordt een firewallregel voor een vooraf gedefinieerde-adresbereik. Dit voorbeeld toont de hele mogelijke bereik van IP-adressen.

```azurecli-interactive
az mysql server firewall-rule create --resource-group mycliresource --server mycliserver --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

## <a name="get-the-connection-information"></a>De verbindingsgegevens ophalen

Als u verbinding met uw server wilt maken, moet u hostgegevens en toegangsreferenties opgeven.
```azurecli-interactive
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

## <a name="connect-to-the-server-using-mysql"></a>Verbinding maken met de server met behulp van mysql
Gebruik de [mysql opdrachtregelprogramma](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) geen verbinding met uw Azure-Database voor de MySQL-server. In dit voorbeeld is de opdracht:
```cmd
mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

## <a name="create-a-blank-database"></a>Een lege database maken
Als u met de server verbonden bent, moet u een lege database maken.
```sql
mysql> CREATE DATABASE mysampledb;
```

Voer de volgende opdracht om de verbinding overschakelen naar de nieuwe database bij de opdrachtprompt:
```sql
mysql> USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Tabellen maken in de database
Nu dat u hoe u verbinding maken met de Azure-Database voor de MySQL-database weet, een aantal algemene taken uitvoeren:

Eerst een tabel maken en deze met enkele gegevens te laden. We maken een tabel met inventarisatie-informatie.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>Gegevens laden in de tabellen
Nu dat u een tabel hebt, moet u enkele gegevens invoegen in het. Voer de volgende query voor het invoegen van een aantal rijen van de gegevens in het venster opdrachtprompt openen.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

U hebt nu twee rijen van de voorbeeldgegevens in de tabel die u eerder hebt gemaakt.

## <a name="query-and-update-the-data-in-the-tables"></a>Vragen en de gegevens in de tabellen bijwerken
De volgende query om informatie te halen uit de databasetabel.
```sql
SELECT * FROM inventory;
```

U kunt ook de gegevens in de tabellen bijwerken.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

De rij wordt dienovereenkomstig bijgewerkt wanneer u gegevens ophaalt.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Een database herstellen naar een eerder tijdstip
Stel dat u deze tabel per ongeluk hebt verwijderd. Dit is iets dat die u eenvoudig niet vanuit herstellen. Azure MySQL-Database kunt u teruggaan naar een willekeurig punt in tijd in het laatste tot 35 dagen en dit punt in tijd naar een nieuwe server herstellen. U kunt deze nieuwe server gebruiken om uw verwijderde gegevens te herstellen. Voordat u de tabel is toegevoegd, de volgende stappen uit de voorbeeldserver herstellen naar een punt.

Voor het herstellen moet u de volgende informatie:

- Herstelpunt: Selecteer een point-in-time die deze gebeurtenis treedt op voordat de server is gewijzigd. Moet groter zijn dan of gelijk zijn aan de oudste back-waarde van de brondatabase.
- Doelserver: Geef een nieuwe servernaam die u terugzetten wilt naar
- Bronserver: Geef de naam van de server die u terugzetten wilt vanaf
- Locatie: U kunt de regio niet selecteren, standaard is dit hetzelfde als de bronserver

```azurecli-interactive
az mysql server restore --resource-group mycliresource --name mycliserver-restored --restore-point-in-time "2017-05-4 03:10" --source-server-name mycliserver
```

Om de server te herstellen en [terugzetten naar punt in tijd](./howto-restore-server-portal.md) voordat de tabel is verwijderd. Herstellen van een server naar een ander punt in tijd maakt een dubbele nieuwe server als de oorspronkelijke server vanaf het punt in tijd die u opgeeft, mits dit binnen de bewaarperiode voor uw [servicelaag](./concepts-service-tiers.md).

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd:
> [!div class="checklist"]
> * Een Azure-Database maken voor MySQL
> * De serverfirewall configureren
> * Gebruik [mysql opdrachtregelprogramma](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) om een database te maken
> * Voorbeeldgegevens laden
> * Querygegevens
> * Gegevens bijwerken
> * Gegevens terugzetten

> [!div class="nextstepaction"]
> [Azure-Database voor de MySQL - voorbeelden van Azure CLI](./sample-scripts-azure-cli.md)
