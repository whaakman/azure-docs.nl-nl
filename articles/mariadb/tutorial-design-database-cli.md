---
title: 'Zelfstudie: een Azure Database for MariaDB ontwerpen met Azure CLI'
description: In deze zelfstudie wordt uitgelegd hoe u een Azure Database for MariaDB-server en -database maakt en beheert met behulp van Azure CLI vanaf de opdrachtregel.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 11/10/2018
ms.custom: mvc
ms.openlocfilehash: 548f4f10758b2d69bf4fda00f8bf52d33d20306c
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57999159"
---
# <a name="tutorial-design-an-azure-database-for-mariadb-using-azure-cli"></a>Zelfstudie: een Azure Database for MariaDB ontwerpen met Azure CLI

Azure Database for MariaDB is een relationele databaseservice in de Microsoft Cloud die is gebaseerd op de MariaDB Community Edition-database-engine. In deze zelfstudie gebruikt u Azure CLI (Command Line Interface of opdrachtregelinterface in goed Nederlands) en andere hulpprogramma's om deze bewerkingen uit te voeren:

> [!div class="checklist"]
> * Een Azure Database for MariaDB maken
> * De serverfirewall configureren
> * Het [opdrachtregelprogramma mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) gebruiken om een database te maken
> * Voorbeeldgegevens laden
> * Querygegevens
> * Gegevens bijwerken
> * Gegevens terugzetten

Als u nog geen abonnement op Azure hebt, maak dan een [gratis Azure-account](https://azure.microsoft.com/free/) aan voordat u begint.

U kunt de Azure Cloud Shell gebruiken in de browser of [Azure CLI installeren]( /cli/azure/install-azure-cli) op uw eigen computer om de codeblokken in deze zelfstudie uit te voeren.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. 

Als u meerdere abonnementen hebt, kiest u het abonnement waarin de resource is opgenomen of wordt gefactureerd. Selecteer een specifiek abonnements-ID in uw account met de opdracht [az account set](/cli/azure/account#az-account-set).
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een [Azure-resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) met de opdracht [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd.

In het volgende voorbeeld wordt een resourcegroep met de naam `myresourcegroup` gemaakt op de locatie `westus`.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-mariadb-server"></a>Een Azure Database for MariaDB-server maken
Maak een Azure Database for MariaDB-server met de opdracht `az mariadb server create`. Een server kan meerdere databases beheren. Een aparte database wordt doorgaans gebruikt voor elk project of voor elke gebruiker.

In het volgende voorbeeld wordt een Azure Database for MariaDB-server gemaakt die zich in `westus` bevindt in de resourcegroep `myresourcegroup` met de naam `mydemoserver`. De server heeft aanmeldgegevens voor de beheerder met de naam `myadmin`. Dit is een Gen 5-server voor Algemeen gebruik met twee vCores. Vervang het `<server_admin_password>` door uw eigen waarde.

```azurecli-interactive
az mariadb server create --resource-group myresourcegroup --name mydemoserver --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 --version 10.2
```
De parameterwaarde voor de sku-naam volgt de conventie {prijscategorie} \_{compute-generatie}\_{vCores}, zoals in de onderstaande voorbeelden:
+ `--sku-name B_Gen5_4` komt overeen met Basic, Gen 5 en 4 vCores.
+ `--sku-name GP_Gen5_32` komt overeen met Algemeen gebruik, Gen 5 en 32 vCores.
+ `--sku-name MO_Gen5_2` komt overeen met Geoptimaliseerd voor geheugen, Gen 5 en 2 vCores.

Raadpleeg de documentatie over [prijscategorieën](./concepts-pricing-tiers.md) om de geldige waarden per regio en categorie te begrijpen.

> [!IMPORTANT]
> De beheerdersaanmelding bij de server en het wachtwoord die u hier opgeeft, zijn vereist voor aanmelding bij de server en de bijbehorende databases verderop in deze snelstartgids. Onthoud of noteer deze informatie voor later gebruik.


## <a name="configure-firewall-rule"></a>Firewallregel configureren
Maak een firewallregel op MariaDB-serverniveau voor Azure Database met de opdracht `az mariadb server firewall-rule create`. Met een firewallregel op serverniveau kan een externe toepassing, zoals het opdrachtregelprogramma **mysql** of MySQL Workbench, verbinding maken met uw server via de MariaDB-servicefirewall van Azure.

In het volgende voorbeeld wordt een firewallregel met de naam `AllowMyIP` gemaakt, die verbindingen van een specifiek IP-adres, 192.168.0.1, toestaat. Vervang dit adres door het IP-adres of de reeks IP-adressen die overeenkomen met het IP-adres waarmee u verbinding gaat maken.

```azurecli-interactive
az mariadb server firewall-rule create --resource-group myresourcegroup --server mydemoserver --name AllowMyIP --start-ip-address 192.168.0.1 --end-ip-address 192.168.0.1
```

## <a name="get-the-connection-information"></a>De verbindingsgegevens ophalen

Als u verbinding met uw server wilt maken, moet u hostgegevens en toegangsreferenties opgeven.
```azurecli-interactive
az mariadb server show --resource-group myresourcegroup --name mydemoserver
```

Het resultaat wordt in JSON-indeling weergegeven. Noteer de **fullyQualifiedDomainName** en de **administratorLogin**.
```json
{
  "administratorLogin": "myadmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mydemoserver.mariadb.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforMariaDB/servers/mydemoserver",
  "location": "westus",
  "name": "mydemoserver",
  "resourceGroup": "myresourcegroup",
 "sku": {
    "capacity": 2,
    "family": "Gen5",
    "name": "GP_Gen5_2",
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
  "type": "Microsoft.DBforMariaDB/servers",
  "userVisibleState": "Ready",
  "version": "10.2"
}
```

## <a name="connect-to-the-server-using-mysql"></a>Verbinding maken met de server met behulp van mysql
Gebruik het [opdrachtregelprogramma mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) om een verbinding tot stand te brengen met uw Azure Database for MariaDB-server. In dit voorbeeld is dit de opdracht:
```cmd
mysql -h mydemoserver.database.windows.net -u myadmin@mydemoserver -p
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
U weet nu hoe u verbinding kunt maken met de Azure Database for MariaDB-database en dus is het tijd om enkele eenvoudige taken uit te voeren.

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
Stel dat u deze tabel per ongeluk hebt verwijderd. Dit is iets wat u niet eenvoudig kunt herstellen. In Azure Database for MariaDB kunt u echter gedurende een periode van maximaal 35 dagen teruggaan naar een bepaald tijdstip om vanaf dat moment de gegevens te herstellen op een nieuwe server. U kunt deze nieuwe server dan gebruiken om de verwijderde gegevens te herstellen. Met de volgende stappen wordt de voorbeeldserver hersteld naar een punt voordat de tabel is toegevoegd.

Voor deze herstelbewerking hebt u de volgende gegevens nodig:

- Herstelpunt: selecteer een tijdstip voorafgaand aan het moment waarop de server is gewijzigd. De tijd moet eerder zijn dan of gelijk zijn aan het tijdstip van de oudste back-up van de brondatabase.
- Doelserver: geef de naam op van de nieuwe server waarnaar u wilt herstellen
- Bronserver: geef de naam op van de server waarvan u gegevens wilt terugzetten
- Locatie: u kunt de regio niet selecteren, standaard is deze hetzelfde als die van de bronserver

```azurecli-interactive
az mariadb server restore --resource-group myresourcegroup --name mydemoserver-restored --restore-point-in-time "2017-05-4 03:10" --source-server-name mydemoserver
```

De opdracht `az mariadb server restore` vereist de volgende parameters:

| Instelling | Voorgestelde waarde | Description  |
| --- | --- | --- |
| resource-group |  myResourceGroup |  De resourcegroep waarin de bronserver bestaat.  |
| name | mydemoserver-restored | De naam van de nieuwe server die door de opdracht restore is gemaakt. |
| restore-point-in-time | 2017-04-13T13:59:00Z | Selecteer een bepaald tijdstip om naar te herstellen. Deze datum en tijd moet binnen de back-upretentieperiode van de bronserver vallen. Gebruik ISO8601-notatie voor datum en tijd. U kunt bijvoorbeeld uw eigen lokale tijdzone, zoals `2017-04-13T05:59:00-08:00`, gebruiken of de UTC Zulu-notatie `2017-04-13T13:59:00Z`. |
| source-server | mydemoserver | De naam of ID van de bronserver voor het herstellen. |

Wanneer u een server naar een bepaald tijdstip herstelt, wordt er een nieuwe server gemaakt, die een kopie is van de oorspronkelijke server vanaf het opgegeven tijdstip. De locatie en prijscategorie van de herstelde server zijn hetzelfde als die van de bronserver.

De opdracht is synchroon en keert terug nadat de server is hersteld. Nadat het herstel is voltooid, zoekt u de nieuwe server. Controleer of de gegevens zijn hersteld zoals verwacht.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u het volgende geleerd:
> [!div class="checklist"]
> * Een Azure Database for MariaDB-server maken
> * De serverfirewall configureren
> * Het [opdrachtregelprogramma mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) gebruiken om een database te maken
> * Voorbeeldgegevens laden
> * Querygegevens
> * Gegevens bijwerken
> * Gegevens terugzetten
