---
title: Uw eerste Azure Database voor MySQL-database ontwerpen - Azure CLI | Microsoft Docs
description: In deze zelfstudie wordt uitgelegd hoe u een Azure Database voor MySQL-server en -database maakt en beheert via Azure CLI 2.0 vanaf de opdrachtregel.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: tutorial
ms.date: 11/28/2017
ms.custom: mvc
ms.openlocfilehash: 5f323086ce66a504188c1834d20873a52a990311
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>Uw eerste Azure Database voor MySQL-database ontwerpen

Azure Database voor MySQL is een relationele databaseservice in de Microsoft Cloud die is gebaseerd op de MySQL Community Edition database engine. In deze zelfstudie gebruikt u Azure CLI (Command Line Interface of opdrachtregelinterface in goed Nederlands) en andere hulpprogramma's om deze bewerkingen uit te voeren:

> [!div class="checklist"]
> * Een Azure Database voor MySQL maken
> * De serverfirewall configureren
> * Het [opdrachtregelprogramma mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) gebruiken om een database te maken
> * Voorbeeldgegevens laden
> * Querygegevens
> * Gegevens bijwerken
> * Gegevens terugzetten

U kunt de Azure Cloud Shell gebruiken in de browser of [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli) op uw eigen computer om de codeblokken in deze zelfstudie uit te voeren.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

Als u meerdere abonnementen hebt, kiest u het abonnement waarin de resource is opgenomen of wordt gefactureerd. Selecteer een specifiek abonnements-ID in uw account met de opdracht [az account set](/cli/azure/account#az_account_set).
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een [Azure-resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) met de opdracht [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd.

In het volgende voorbeeld wordt een resourcegroep met de naam `mycliresource` gemaakt op de locatie `westus`.

```azurecli-interactive
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Een Azure-database voor MySQL-server maken
Maak een Azure Database for MySQL-server met de opdracht az mysql server create. Een server kan meerdere databases beheren. Een aparte database wordt doorgaans gebruikt voor elk project of voor elke gebruiker.

In het volgende voorbeeld wordt een Azure-database voor MySQL-server gemaakt die zich in `westus` bevindt in de resourcegroep `mycliresource` met de naam `mycliserver`. De server heeft aanmeldgegevens voor de beheerder met de naam `myadmin` en het wachtwoord `Password01!`. De server wordt gemaakt met de **Basic**-prestatielaag en **50** rekeneenheden die tussen alle databases op de server worden gedeeld. U kunt berekeningen en opslag omhoog of omlaag schalen afhankelijk van de behoeften van de toepassing.

```azurecli-interactive
az mysql server create --resource-group mycliresource --name mycliserver --location westus --admin-user myadmin --admin-password Password01! --performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a>Firewallregel configureren
Maak een Azure Database for MySQL-firewallregel op serverniveau met de opdracht az mysql server firewall-rule create. Een firewallregel op serverniveau stelt een externe toepassing, zoals het opdrachtregelprogramma **mysql.exe** of MySQL Workbench, in staat om verbinding te maken met uw server via de firewall van de MySQL-service van Azure. 

In het volgende wordt een firewallregel gemaakt voor een vooraf gedefinieerd adresbereik. Dit voorbeeld laat het volledige bereik van IP-adressen zien dat kan worden gebruikt.

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
Gebruik het [opdrachtregelprogramma mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) om een verbinding tot stand te brengen met uw Azure Database voor MySQL-server. In dit voorbeeld is dit de opdracht:
```cmd
mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

## <a name="create-a-blank-database"></a>Een lege database maken
Zodra u met de server bent verbonden, maakt u een lege database.
```sql
mysql> CREATE DATABASE mysampledb;
```

Voer bij de prompt de volgende opdracht uit om verbinding te maken met de zojuist gemaakte database:
```sql
mysql> USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Tabellen maken in de database
U weet nu hoe u verbinding kunt maken met de Azure Database voor MySQL-database en dus is het tijd om enkele eenvoudige taken uit te voeren:

We gaan eerst een tabel maken en hierin enkele gegevens laden. Laten we een tabel maken waarin voorraadgegevens worden opgeslagen.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>Gegevens laden in de tabellen
De volgende stap bestaat uit het toevoegen van gegevens aan de tabel. Voer bij de opdrachtprompt de volgende query uit om enkele rijen met gegevens in te voegen.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

We hebben nu twee rijen met voorbeeldgegevens in de tabel die u eerder hebt gemaakt.

## <a name="query-and-update-the-data-in-the-tables"></a>De gegevens in de tabellen opvragen en bijwerken
Voer de volgende query uit om gegevens op te halen uit de databasetabel.
```sql
SELECT * FROM inventory;
```

U kunt ook de gegevens in de tabellen bijwerken.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

De rij wordt dan meteen bijgewerkt wanneer u gegevens ophaalt.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Een database herstellen naar een eerder tijdstip
Stel dat u deze tabel per ongeluk hebt verwijderd. Dit is iets wat u niet eenvoudig kunt herstellen. In Azure Database voor SQL kunt u echter gedurende een periode van maximaal 35 dagen teruggaan naar een bepaald tijdstip om vanaf dat moment de gegevens te herstellen op een nieuwe server. U kunt deze nieuwe server dan gebruiken om de verwijderde gegevens te herstellen. Met de volgende stappen wordt de voorbeeldserver hersteld naar een punt voordat de tabel is toegevoegd.

Voor deze herstelbewerking hebt u de volgende gegevens nodig:

- Herstelpunt: selecteer een tijdstip voorafgaand aan het moment waarop de server is gewijzigd. De tijd moet eerder zijn dan of gelijk zijn aan het tijdstip van de oudste back-up van de brondatabase.
- Doelserver: geef de naam op van de nieuwe server waarnaar u wilt herstellen.
- Bronserver: geef de naam op van de server waarvan u gegevens wilt terugzetten.
- Locatie: u kunt de regio niet selecteren, standaard is deze hetzelfde als die van de bronserver.

```azurecli-interactive
az mysql server restore --resource-group mycliresource --name mycliserver-restored --restore-point-in-time "2017-05-4 03:10" --source-server-name mycliserver
```

Klik op OK om de server te [herstellen naar een eerder tijdstip](./howto-restore-server-portal.md), voordat de tabel werd toegevoegd. Als u een server herstelt naar een eerder tijdstip, wordt er een nieuw exemplaar gemaakt van de oorspronkelijke server met de inhoud zoals die aanwezig was op het tijdstip dat u opgeeft. Dit tijdstip moet wel binnen de bewaarperiode liggen die wordt gehanteerd voor uw [servicelaag](./concepts-service-tiers.md).

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u het volgende geleerd:
> [!div class="checklist"]
> * Een Azure Database voor MySQL maken
> * De serverfirewall configureren
> * Het [opdrachtregelprogramma mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) gebruiken om een database te maken
> * Voorbeeldgegevens laden
> * Querygegevens
> * Gegevens bijwerken
> * Gegevens terugzetten

> [!div class="nextstepaction"]
> [Azure Database voor MySQL - Azure CLI-voorbeelden](./sample-scripts-azure-cli.md)
