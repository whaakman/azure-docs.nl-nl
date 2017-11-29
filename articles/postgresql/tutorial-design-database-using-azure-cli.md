---
title: Ontwerp van uw eerste Azure-Database voor PostgreSQL met Azure CLI | Microsoft Docs
description: Deze zelfstudie laat zien hoe uw eerste Azure-Database voor PostgreSQL ontwerpen met Azure CLI.
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: azure-cli
ms.topic: tutorial
ms.date: 11/27/2017
ms.openlocfilehash: 97299ae904115d08c5d03be38be263203552b84b
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="design-your-first-azure-database-for-postgresql-using-azure-cli"></a>Ontwerp van uw eerste Azure-Database voor PostgreSQL met Azure CLI 
In deze zelfstudie maakt u Azure CLI (opdrachtregelinterface) en andere hulpprogramma's voor meer informatie over hoe:
> [!div class="checklist"]
> * Een Azure-database voor PostgreSQL-server maken
> * De serverfirewall configureren
> * Gebruik [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) hulpprogramma voor het maken van een database
> * Voorbeeldgegevens laden
> * Querygegevens
> * Gegevens bijwerken
> * Gegevens terugzetten

U kunt de Azure-Cloud-Shell gebruiken in de browser of [Installeer Azure CLI 2.0]( /cli/azure/install-azure-cli) op uw eigen computer uitvoeren van de opdrachten in deze zelfstudie.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

Als u meerdere abonnementen hebt, kiest u het abonnement waarin de resource is opgenomen of wordt gefactureerd. Selecteer een specifiek abonnements-ID in uw account met de opdracht [az account set](/cli/azure/account#az_account_set).
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) met de opdracht [az group create](/cli/azure/group#az_group_create). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam `myresourcegroup` gemaakt op de locatie `westus`.
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-postgresql-server"></a>Een Azure-database voor PostgreSQL-server maken
Maak een [Azure-database voor PostgreSQL-server](overview.md) met behulp van de opdracht [az postgres server create](/cli/azure/postgres/server#az_postgres_server_create). Een server bevat een groep met databases die worden beheerd als groep. 

Het volgende voorbeeld wordt een server met de naam `mypgserver-20170401` in uw resourcegroep `myresourcegroup` met aanmeldgegevens van serverbeheerder `mylogin`. De naam van een server komt overeen met een DNS-naam en moet dus globaal uniek zijn in Azure. Vervang het `<server_admin_password>` door uw eigen waarde.
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401 --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> De beheerdersaanmelding bij de server en het wachtwoord die u hier opgeeft, zijn vereist voor aanmelding bij de server en de bijbehorende databases verderop in deze Quick Start. Onthoud of noteer deze informatie voor later gebruik.

De database **postgres** wordt standaard gemaakt op uw server. De database [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) is een standaarddatabase die kan worden gebruikt door gebruikers, hulpprogramma's en toepassingen van derden. 


## <a name="configure-a-server-level-firewall-rule"></a>Een serverfirewallregel configureren

Maak een Azure PostgreSQL-firewallregel op serverniveau met de opdracht [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_create). Met een firewallregel op serverniveau kan een externe toepassing, zoals [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) of [PgAdmin](https://www.pgadmin.org/) verbinding maken met uw server via de firewall van de Azure PostgreSQL-service. 

U kunt een firewallregel voor een IP-bereik zodat u vanaf uw netwerk verbinding kunt maken. Het volgende voorbeeld wordt [az postgres server-firewallregel maken](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_create) voor het maken van een firewallregel `AllowAllIps` waarmee de verbinding van elk IP-adres. Als u alle IP-adressen wilt openen, gebruikt u 0.0.0.0 als beginadres en 255.255.255.255 als eindadres.

Om toegang te beperken met uw Azure-PostgreSQL-server met alleen het netwerk, kunt u de firewallregel alleen betrekking op uw bedrijfsnetwerk IP-adresbereik instellen.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> De Azure PostgreSQL-server communiceert via poort 5432. Als u verbinding maakt vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 5432 mogelijk niet toegestaan door de firewall van uw netwerk. Vraag aan de IT-afdeling of ze poort 5432 willen openen, zodat u verbinding kunt maken met uw Azure SQL Database-server.
>

## <a name="get-the-connection-information"></a>De verbindingsgegevens ophalen

Als u verbinding met uw server wilt maken, moet u hostgegevens en toegangsreferenties opgeven.
```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mypgserver-20170401
```

Het resultaat wordt in JSON-indeling weergegeven. Noteer de **aanmeldgegevens van de beheerder** en **fullyQualifiedDomainName**.
```json
{
  "administratorLogin": "mylogin",
  "fullyQualifiedDomainName": "mypgserver-20170401.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforPostgreSQL/servers/mypgserver-20170401",
  "location": "westus",
  "name": "mypgserver-20170401",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 50,
    "family": null,
    "name": "PGSQLS2M50",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": null,
  "storageMb": 51200,
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": "9.6"
}
```

## <a name="connect-to-azure-database-for-postgresql-database-using-psql"></a>Verbinding maken met Azure-Database voor PostgreSQL-database met behulp van psql
Als de clientcomputer PostgreSQL geïnstalleerd heeft, kunt u een lokaal exemplaar van [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html), of de Azure-Cloud-Console verbinding maken met een Azure-PostgreSQL-server. U gaat nu het opdrachtregelprogramma psql gebruiken om verbinding te maken met de Azure Database voor PostgreSQL-server.

1. Voer de volgende opdracht psql verbinding maken met een Azure-Database voor PostgreSQL-database:
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  Met de volgende opdracht maakt u bijvoorbeeld verbinding met de standaarddatabase **postgres** op uw PostgreSQL-server **mypgserver-20170401.postgres.database.azure.com** met behulp van toegangsreferenties. Voer het `<server_admin_password>` in dat u koos toen u werd gevraagd om een wachtwoord.
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 ---dbname=postgres
```

2.  Zodra u met de server verbonden bent, moet u een lege database maken bij de opdrachtprompt:
```sql
CREATE DATABASE mypgsqldb;
```

3.  Voer na de prompt de volgende opdracht uit om verbinding te maken met de zojuist gemaakte database **mypgsqldb**:
```sql
\c mypgsqldb
```

## <a name="create-tables-in-the-database"></a>Tabellen maken in de database
Nu dat u hoe u verbinding maken met de Azure-Database voor PostgreSQL weet, kunnen we gaan over hoe u enkele eenvoudige taken uitvoeren.

We kunnen eerst een tabel maken en deze met enkele gegevens te laden. We gaan een tabel maken die inventarisatie-informatie houdt:
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

U kunt nu de zojuist gemaakte tabel in de lijst met tabellen zien door te typen:
```sql
\dt
```

## <a name="load-data-into-the-table"></a>Gegevens laden in de tabel
Nu dat we een tabel hebben, kunnen we sommige gegevens invoegen in het. Voer de volgende query voor het invoegen van een aantal rijen van de gegevens in het venster opdrachtprompt openen:
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

U hebt nu twee rijen van voorbeeldgegevens toegevoegd in de tabel die u eerder hebt gemaakt.

## <a name="query-and-update-the-data-in-the-tables"></a>Vragen en de gegevens in de tabellen bijwerken
De volgende query voor het ophalen van gegevens uit de tabel inventarisatie uitvoeren: 
```sql
SELECT * FROM inventory;
```

U kunt ook de gegevens in de inventaris-tabel bijwerken:
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Wanneer u de gegevens ophaalt, kunt u de bijgewerkte waarden zien:
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Een database herstellen naar een eerder tijdstip
Stel dat u een tabel per ongeluk hebt verwijderd. Dit is iets dat die u eenvoudig niet vanuit herstellen. Azure PostgreSQL-Database kunt u terugkeren naar een punt-in-tijd (maximaal 7 dagen in Basic) en 35 dagen in de standaard- en dit punt in tijd herstellen naar een nieuwe server. U kunt deze nieuwe server gebruiken om uw verwijderde gegevens te herstellen. 

De volgende opdracht worden de voorbeeldserver hersteld naar een punt voordat de tabel is toegevoegd:
```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

De `az postgres server restore` opdracht moet de volgende parameters:
| Instelling | Voorgestelde waarde | Beschrijving  |
| --- | --- | --- |
| --resourcegroep |  myResourceGroup |  De resourcegroep waarin de bronserver bestaat.  |
| --naam | mypgserver hersteld | De naam van de nieuwe server die door de opdracht restore wordt gemaakt. |
| herstel punt in tijd | 2017-04-13T13:59:00Z | Selecteer een point-in-time om naar te herstellen. Deze datum en tijd moet binnen de back-up bewaarperiode van de bronserver. Gebruik ISO8601-indeling voor datum en tijd. Bijvoorbeeld, u kunt uw eigen lokale tijdzone, zoals `2017-04-13T05:59:00-08:00`, of Zulu UTC-notatie gebruiken `2017-04-13T13:59:00Z`. |
| ---bronserver | mypgserver 20170401 | De naam of ID van de bronserver in om te herstellen. |

Herstellen van een server naar een punt in tijd, maakt een nieuwe server, als de oorspronkelijke server vanaf het punt in tijd die u hebt gekopieerd. De locatie en de prijscategorie laag waarden voor de herstelde server zijn hetzelfde als de bronserver.

De opdracht is synchroon en wordt geretourneerd nadat de server is hersteld. Nadat het herstel is voltooid, zoek de nieuwe server die is gemaakt. Controleer of dat de gegevens is hersteld, zoals verwacht.


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u Azure CLI (opdrachtregelinterface) en andere hulpprogramma's om te gebruiken:
> [!div class="checklist"]
> * Een Azure-database voor PostgreSQL-server maken
> * De serverfirewall configureren
> * Gebruik [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) hulpprogramma voor het maken van een database
> * Voorbeeldgegevens laden
> * Querygegevens
> * Gegevens bijwerken
> * Gegevens terugzetten

Vervolgens leert u hoe u met de Azure-portal hetzelfde werk, controleert u in deze zelfstudie: [ontwerpen van uw eerste Azure-Database voor PostgreSQL met de Azure portal](tutorial-design-database-using-azure-portal.md)
