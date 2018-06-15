---
title: 'Snelstart: een Azure Database for PostgreSQL maken via Azure CLI'
description: Quick Start voor het maken en beheren van Azure Database voor PostgreSQL-server met behulp van Azure CLI (opdrachtregelinterface).
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: quickstart
ms.date: 04/01/2018
ms.custom: mvc
ms.openlocfilehash: daa7ea345abb6228bee2d1ca5bfcc3850aaff9c3
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
ms.locfileid: "31415038"
---
# <a name="quickstart-create-an-azure-database-for-postgresql-using-the-azure-cli"></a>Snelstart: een Azure Database for PostgreSQL maken via Azure CLI
Azure Database voor PostgreSQL is een beheerde service waarmee u PostgreSQL-databases met hoge beschikbaarheid in de cloud kunt uitvoeren, beheren en schalen. De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. Deze Quick Start laat zien hoe u een Azure-database voor PostgreSQL-server kunt maken in een [Azure resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) met de Azure CLI.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer de opdracht `az --version` uit om de geïnstalleerde versie te zien. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

Als u de CLI lokaal uitvoert, moet u zich aanmelden bij uw account met behulp van de opdracht [az login](/cli/azure/authenticate-azure-cli?view=interactive-log-in). Let op de **id**-eigenschap van de opdrachtuitvoer voor de naam van het desbetreffende abonnement.
```azurecli-interactive
az login
```

Als u meerdere abonnementen hebt, kiest u het juiste abonnement waarin de resource moet worden gefactureerd. Selecteer de specifieke abonnements-id in uw account met de opdracht [az account set](/cli/azure/account#az_account_set). Gebruik de **id**-eigenschap uit de **az login**-uitvoer voor uw abonnement in de tijdelijke aanduiding voor de abonnement-id.
```azurecli-interactive
az account set --subscription <subscription id>
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) met de opdracht [az group create](/cli/azure/group#az_group_create). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd. U moet een unieke naam opgeven. In het volgende voorbeeld wordt een resourcegroep met de naam `myresourcegroup` gemaakt op de locatie `westus`.
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-postgresql-server"></a>Een Azure-database voor PostgreSQL-server maken

Maak een [Azure-database voor PostgreSQL-server](overview.md) met behulp van de opdracht [az postgres server create](/cli/azure/postgres/server#az_postgres_server_create). Een server bevat een groep met databases die worden beheerd als groep. 

In het volgende voorbeeld wordt een server in VS West gemaakt met de naam `mydemoserver` in uw resourcegroep `myresourcegroup` met aanmeldgegevens van de serverbeheerder `myadmin`. Dit is een **Gen 4**-server voor **Algemeen gebruik** met twee **vCores**. De naam van een server komt overeen met een DNS-naam en moet dus globaal uniek zijn in Azure. Vervang het `<server_admin_password>` door uw eigen waarde.
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen4_2 --version 9.6
```

> [!IMPORTANT]
> De beheerdersaanmelding bij de server en het wachtwoord die u hier opgeeft, zijn vereist voor aanmelding bij de server en de bijbehorende databases verderop in deze quickstart. Onthoud of noteer deze informatie voor later gebruik.

De database **postgres** wordt standaard gemaakt op uw server. De database [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) is een standaarddatabase die kan worden gebruikt door gebruikers, hulpprogramma's en toepassingen van derden. 


## <a name="configure-a-server-level-firewall-rule"></a>Een serverfirewallregel configureren

Maak een Azure PostgreSQL-firewallregel op serverniveau met de opdracht [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_create). Met een firewallregel op serverniveau kan een externe toepassing, zoals [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) of [PgAdmin](https://www.pgadmin.org/) verbinding maken met uw server via de firewall van de Azure PostgreSQL-service. 

U kunt een firewallregel voor een IP-bereik instellen, zodat u vanaf uw netwerk verbinding kunt maken. In het volgende voorbeeld wordt [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_create) gebruikt om een firewallregels te maken `AllowAllIps` voor een IP-adresbereik. Als u alle IP-adressen wilt openen, gebruikt u 0.0.0.0 als beginadres en 255.255.255.255 als eindadres.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mydemoserver --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> De Azure PostgreSQL-server communiceert via poort 5432. Als u verbinding maakt vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 5432 mogelijk niet toegestaan door de firewall van uw netwerk. Vraag aan de IT-afdeling of ze poort 5432 willen openen, zodat u verbinding kunt maken met uw Azure PostgreSQL-server.

## <a name="get-the-connection-information"></a>De verbindingsgegevens ophalen

Als u verbinding met uw server wilt maken, moet u hostgegevens en toegangsreferenties opgeven.
```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mydemoserver
```

Het resultaat wordt in JSON-indeling weergegeven. Noteer de **aanmeldgegevens van de beheerder** en **fullyQualifiedDomainName**.
```json
{
  "administratorLogin": "myadmin",
  "earliestRestoreDate": null,
  "fullyQualifiedDomainName": "mydemoserver.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforPostgreSQL/servers/mydemoserver",
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

  Met de volgende opdracht maakt u bijvoorbeeld verbinding met de standaarddatabase **postgres** op uw PostgreSQL-server **mydemoserver.postgres.database.azure.com** met behulp van toegangsreferenties. Voer het `<server_admin_password>` in dat u koos toen u werd gevraagd om een wachtwoord.
  
  ```azurecli-interactive
psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=myadmin@mydemoserver --dbname=postgres
```

2.  Wanneer u met de server bent verbonden, maakt u bij de prompt een lege database.
```sql
CREATE DATABASE mypgsqldb;
```

3.  Voer na de prompt de volgende opdracht uit om verbinding te maken met de zojuist gemaakte database **mypgsqldb**:
```sql
\c mypgsqldb
```

## <a name="connect-to-the-postgresql-server-using-pgadmin"></a>Verbinding maken met de PostgreSQL-server met behulp van pgAdmin

pgAdmin is een open-source hulpprogramma voor PostgreSQL. U kunt pgAdmin installeren vanaf de [pgAdmin-website](http://www.pgadmin.org/). Mogelijk gebruikt u een andere versie van pgAdmin dan de versie in deze snelstart. Raadpleeg de documentatie van pgAdmin als u extra hulp nodig hebt.

1. Start de toepassing pgAdmin op uw clientcomputer.

2. Ga naar **Object** op de werkbalk, wijs **Create** aan met de muisaanwijzer en selecteer **Server**.

3. In het dialoogvenster **Create - Server** gaat u naar het tabblad **General** en voert u een unieke beschrijvende naam in voor de server, zoals **mydemoserver**.

   ![Het tabblad General](./media/quickstart-create-server-database-azure-cli/9-pgadmin-create-server.png)

4. In het dialoogvenster **Create - Server** gaat u naar het tabblad **Connection** en vult u de tabel met instellingen in.

   ![Het tabblad Connection](./media/quickstart-create-server-database-azure-cli/10-pgadmin-create-server.png)

    Parameter pgAdmin |Waarde|Beschrijving
    ---|---|---
    Host name/address | Servernaam | De servernaam die u hebt gebruikt toen u eerder de Azure Database for PostgreSQL-server hebt gemaakt. De server in ons voorbeeld is **mydemoserver.postgres.database.azure.com**. Gebruik de FQDN (Fully Qualified Domain Name) (**\*. postgres.database.azure.com**) zoals weergegeven in het voorbeeld. Als u de servernaam niet meer weet, volgt u de stappen in de vorige sectie om de verbindingsgegevens op te halen. 
    Poort | 5432 | De poort die moet worden gebruikt wanneer u verbinding maakt met de Azure Database for PostgreSQL-server. 
    Onderhoudsdatabase | *postgres* | De door het systeem gegenereerde standaarddatabasenaam.
    Gebruikersnaam | Aanmeldingsnaam van serverbeheerder | De aanmeldingsnaam van de serverbeheerder die u hebt opgegeven toen u de Azure Database for PostgreSQL-server eerder hebt gemaakt. Als u de gebruikersnaam niet meer weet, volgt u de stappen in de vorige sectie om de verbindingsgegevens op te halen. De indeling is *username@servername*.
    Wachtwoord | Uw beheerderswachtwoord | Het wachtwoord dat u hebt gekozen toen u eerder in deze Quick Start de server maakte.
    Rol | Leeg laten | U hoeft op dit moment geen rolnaam op te geven. Laat het veld leeg.
    SSL-modus | *Require* | U kunt de SSL-modus instellen op het tabblad SSL van pgAdmin. Bij het maken van een Azure Database for PostgreSQL-server wordt standaard geforceerd SSL ingeschakeld. Raadpleeg [SSL afdwingen](./concepts-ssl-connection-security.md) als u geforceerd SSL wilt uitschakelen.
    
5. Selecteer **Opslaan**.

6. Vouw in het linkerdeelvenster **Browser** het knooppunt **Server** uit. Selecteer uw server, bijvoorbeeld **mydemoserver**. Klik erop om er verbinding mee te maken.

7. Vouw het knooppunt Servers uit en vouw vervolgens **Databases** eronder uit. De lijst moet uw bestaande *postgres*-database bevatten, plus eventuele andere databases die u hebt gemaakt. U kunt met Azure Database for PostgreSQL meerdere databases per server maken.

8. Klik met de rechtermuisknop op **Databases**, kies het menu **Create** en selecteer **Database**.

9. Typ een naam voor de database in het veld **Database**, bijvoorbeeld **mypgsqldb2**.

10. Selecteer bij **Owner** de eigenaar voor de database uit de keuzelijst. Kies de aanmeldingsnaam van de serverbeheerder, zoals **my admin** uit het voorbeeld.

   ![Een database maken in pgadmin](./media/quickstart-create-server-database-azure-cli/11-pgadmin-database.png)

11. Klik op **Save** om een nieuwe, lege database te maken.

12. In het deelvenster **Browser** kunt u de database die u hebt gemaakt, zien in de lijst met databases onder de naam van uw server.




## <a name="clean-up-resources"></a>Resources opschonen

Verwijder alle resources die u in deze Quick Start hebt gemaakt door de [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) te verwijderen.

> [!TIP]
> Andere Quick Starts in deze verzameling zijn op deze Quick Start gebaseerd. Als u door wilt gaan met andere Quick Starts, verwijdert u de resources die u in deze Quick Start hebt gemaakt, niet. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources te verwijderen die tijdens deze Quick Start in de Azure CLI zijn gemaakt.

```azurecli-interactive
az group delete --name myresourcegroup
```

Als u alleen de zojuist gemaakte server wilt verwijderen, kunt u de opdracht [az postgres server delete](/cli/azure/postgres/server#az_postgres_server_delete) uitvoeren.
```azurecli-interactive
az postgres server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Een database migreren met behulp van Exporteren en importeren](./howto-migrate-using-export-and-import.md)

