---
title: Een Azure-database voor PostgreSQL maken met de Azure CLI | Microsoft-documenten
description: Quick Start-gids voor het maken en beheren van Azure Database voor PostgreSQL-server met behulp van Azure CLI (opdrachtregelinterface).
services: postgresql
author: sanagama
ms.author: sanagama
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: quickstart
ms.date: 06/13/2017
ms.openlocfilehash: d78243abc140c7b3f0b99bdf56821b7920568550
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-azure-database-for-postgresql-using-the-azure-cli"></a>Een Azure-database voor PostgreSQL maken met de Azure CLI
Azure Database voor PostgreSQL is een beheerde service waarmee u PostgreSQL-databases met hoge beschikbaarheid in de cloud kunt uitvoeren, beheren en schalen. De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. Deze Quick Start laat zien hoe u een Azure-database voor PostgreSQL-server kunt maken in een [Azure resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) met de Azure CLI.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

Als u meerdere abonnementen hebt, kiest u het juiste abonnement waarin de resource zal worden gefactureerd. Selecteer een specifiek abonnements-ID in uw account met de opdracht [az account set](/cli/azure/account#set).
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) met de opdracht [az group create](/cli/azure/group#create). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam `myresourcegroup` gemaakt op de locatie `westus`.
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-postgresql-server"></a>Een Azure-database voor PostgreSQL-server maken

Maak een [Azure-database voor PostgreSQL-server](overview.md) met behulp van de opdracht [az postgres server create](/cli/azure/postgres/server#create). Een server bevat een groep met databases die worden beheerd als groep. 

In het volgende voorbeeld wordt een server gemaakt met de naam `mypgserver-20170401` in uw resourcegroep `myresourcegroup` met aanmeldgegevens van de serverbeheerder `mylogin`. De naam van een server komt overeen met een DNS-naam en moet dus globaal uniek zijn in Azure. Vervang het `<server_admin_password>` door uw eigen waarde.
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401  --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> De beheerdersaanmelding bij de server en het wachtwoord die u hier opgeeft, zijn vereist voor aanmelding bij de server en de bijbehorende databases verderop in deze Quick Start. Onthoud of noteer deze informatie voor later gebruik.

De database **postgres** wordt standaard gemaakt op uw server. De database [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) is een standaarddatabase die kan worden gebruikt door gebruikers, hulpprogramma's en toepassingen van derden. 


## <a name="configure-a-server-level-firewall-rule"></a>Een serverfirewallregel configureren

Maak een Azure PostgreSQL-firewallregel op serverniveau met de opdracht [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create). Met een firewallregel op serverniveau kan een externe toepassing, zoals [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) of [PgAdmin](https://www.pgadmin.org/) verbinding maken met uw server via de firewall van de Azure PostgreSQL-service. 

U kunt een firewallregel voor een IP-bereik instellen, zodat u vanaf uw netwerk verbinding kunt maken. In het volgende voorbeeld wordt [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) gebruikt om een firewallregels te maken `AllowAllIps` voor een IP-adresbereik. Als u alle IP-adressen wilt openen, gebruikt u 0.0.0.0 als beginadres en 255.255.255.255 als eindadres.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> De Azure PostgreSQL-server communiceert via poort 5432. Als u verbinding maakt vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 5432 mogelijk niet toegestaan door de firewall van uw netwerk. Vraag aan de IT-afdeling of ze poort 5432 willen openen, zodat u verbinding kunt maken met uw Azure SQL Database-server.

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

## <a name="connect-to-postgresql-database-using-psql"></a>Verbinding maken met een PostgreSQL-database met behulp van psql

Als op uw clientcomputer PostgreSQL is geïnstalleerd, kunt u een lokale instantie van [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) gebruiken om verbinding te maken met een Azure PostgreSQL-server. We gaan nu het opdrachtregelprogramma psql gebruiken om verbinding te maken met de Azure PostgreSQL-server.

1. Voer de volgende psql-opdracht uit om verbinding te maken met een Azure-database voor PostgreSQL-server
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  Met de volgende opdracht maakt u bijvoorbeeld verbinding met de standaarddatabase **postgres** op uw PostgreSQL-server **mypgserver-20170401.postgres.database.azure.com** met behulp van toegangsreferenties. Voer het `<server_admin_password>` in dat u koos toen u werd gevraagd om een wachtwoord.
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
```

2.  Wanneer u met de server bent verbonden, maakt u bij de prompt een lege database.
```sql
CREATE DATABASE mypgsqldb;
```

3.  Voer na de prompt de volgende opdracht uit om verbinding te maken met de zojuist gemaakte database **mypgsqldb**:
```sql
\c mypgsqldb
```

## <a name="connect-to-postgresql-database-using-pgadmin"></a>Verbinding maken met een PostgreSQL-database met behulp van pgAdmin

Verbinding maken met een Azure PostgreSQL-server met behulp van het GUI-hulpprogramma _pgAdmin_
1.  Start de toepassing _pgAdmin_ op uw clientcomputer. U kunt _pgAdmin_ installeren via http://www.pgadmin.org/.
2.  Selecteer **Nieuwe Server toevoegen** via het menu **Snelkoppelingen**.
3.  In het dialoogvenster **Maken - Server**, tabblad **Algemeen**, voert u een unieke beschrijvende naam in voor de server. Bijvoorbeeld **Azure PostgreSQL Server**.
 ![pgAdmin-hulpprogramma - Maken - Server](./media/quickstart-create-server-database-azure-cli/1-pgadmin-create-server.png)
4.  In het dialoogvenster **Maken - Server**, tabblad **Verbinding**:
    - Geef de volledige servernaam op (bijvoorbeeld **mypgserver-20170401.postgres.database.azure.com**) in het vak **Hostnaam/-adres**. 
    - Typ 5432 in het vak **Poort**. 
    - Typ de **Aanmeldgegevens voor de serverbeheerder (user@mypgserver)** die u eerder hebt verkregen in deze Quick Start en het wachtwoord dat u aanmaakte toen u de server maakte, in de respectieve vakken **Gebruikersnaam** en **Wachtwoord**.
    - Selecteer **SSL-modus** als **Vereist**. Standaard worden alle Azure PostgreSQL-servers gemaakt met de optie SSL afdwingen ingeschakeld. Zie [SSL afdwingen](./concepts-ssl-connection-security.md) als u het afdwingen van SSL wilt uitschakelen.

    ![pgAdmin - Maken - Server](./media/quickstart-create-server-database-azure-cli/2-pgadmin-create-server.png)
5.  Klik op **Opslaan**.
6.  In het linkerdeelvenster Browser vouwt u de optie **Servergroepen** uit. Kies uw **Azure PostgreSQL-server**.
7.  Kies de **server** waarmee u verbonden bent en selecteer daaronder **Databases**. 
8.  Klik met de rechtermuisknop op **Databases** om een database te maken.
9.  Kies de databasenaam **mypgsqldb** en voer de eigenaar in als serverbeheerder (**mylogin**).
10. Klik op **Opslaan** om een lege database te maken.
11. In **Browser** vouwt u **Servers**-groep uit. Vouw de server uit die u hebt gemaakt en u ziet daaronder de database **mypgsqldb**.
 ![pgAdmin - Maken - Database](./media/quickstart-create-server-database-azure-cli/3-pgadmin-database.png)


## <a name="clean-up-resources"></a>Resources opschonen

Verwijder alle resources die u in deze Quick Start hebt gemaakt door de [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) te verwijderen.

> [!TIP]
> Andere Quick Starts in deze verzameling zijn op deze Quick Start gebaseerd. Als u van plan bent om door te gaan met andere Quick Starts, verwijdert u de resources die u in deze Quick Start hebt gemaakt niet. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources te verwijderen die tijdens deze Quick Start in de Azure CLI zijn gemaakt.

```azurecli-interactive
az group delete --name myresourcegroup
```

Als u alleen de zojuist gemaakte server wilt verwijderen, kunt u de opdracht [az postgres server delete](/cli/azure/postgres/server#delete) uitvoeren.
```azurecli-interactive
az postgres server delete --resource-group myresourcegroup --name mypgserver-20170401
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Een database migreren met behulp van Exporteren en importeren](./howto-migrate-using-export-and-import.md)
