---
title: 'Snelstart: Een Azure Database for MariaDB-server maken- Azure CLI'
description: In deze snelstartgids wordt beschreven hoe u met Azure CLI een Azure Database for MariaDB-server maakt in een Azure-resourcegroep.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.devlang: azure-cli
ms.topic: quickstart
ms.date: 09/24/2018
ms.custom: mvc
ms.openlocfilehash: 9e87dacb80aa7fc5f5b073e631fc06ae74b3ad00
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46996601"
---
# <a name="create-an-azure-database-for-mariadb-server-using-azure-cli"></a>Een Azure Database for MariaDB-server maken met behulp van Azure CLI
In deze snelstartgids wordt beschreven hoe u Azure CLI gebruikt om binnen ongeveer vijf minuten een Azure Database for MariaDB-server te maken in een Azure-resourcegroep. De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

Als u meerdere abonnementen hebt, kiest u het abonnement waarin de resource is opgenomen of wordt gefactureerd. Selecteer een specifiek abonnements-ID in uw account met de opdracht [az account set](/cli/azure/account#az-account-set).
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een [Azure-resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) met de opdracht [az group create](/cli/azure/group#az-group-create). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd.

In het volgende voorbeeld wordt een resourcegroep met de naam `myresourcegroup` gemaakt op de locatie `westus`.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-mariadb-server"></a>Een Azure Database for MariaDB-server maken
U maakt een Azure Database for MariaDB-server met de opdracht **[az mariadb server create](/cli/azure/mariadb/server#az-mariadb-server-create)**. Een server kan meerdere databases beheren. Een aparte database wordt doorgaans gebruikt voor elk project of voor elke gebruiker.

**Instelling** | **Voorbeeldwaarde** | **Beschrijving**
---|---|---
naam | mydemoserver | Kies een unieke naam ter identificatie van de Azure Database for MariaDB-server. De servernaam mag alleen kleine letters, cijfers en het koppelteken (-) bevatten. en moet 3 tot 63 tekens lang zijn.
resource-group | myResourceGroup | Geef de naam op van de Azure-resourcegroep.
sku-name | GP_Gen5_2 | De naam van de SKU. Volgt de verkorte notatie voor conventie {prijscategorie}_{compute-generatie}_{vCores}. Zie onder deze tabel voor meer informatie over de parameter sku-name.
backup-retention | 7 | Hoe lang een back-up moet worden bewaard. De eenheid is dagen. Het bereik is 7-35. 
geo-redundant-backup | Uitgeschakeld | Of geografisch redundante back-ups moeten worden ingeschakeld voor deze server. Toegestane waarden: Enabled, Disabled.
location | westus | De Azure-locatie voor de server.
ssl-enforcement | Ingeschakeld | Of ssl moet worden ingeschakeld voor deze server. Toegestane waarden: Enabled, Disabled.
storage-size | 51200 | De opslagcapaciteit van de server (eenheid is MB). De minimale opslaggrootte is 5120 MB en deze kunt u ophogen in stappen van 1024 MB. Zie het document met [prijsinformatie](./concepts-pricing-tiers.md) voor meer informatie over de opslaglimieten. 
versie | 10.2 | De versie van de hoofd-engine van MariaDB.
admin-user | myadmin | De gebruikersnaam voor aanmelding als beheerder. De aanmeldingsnaam van de beheerder mag niet **azure_superuser**, **admin**, **administrator**, **root**, **guest** of **public** zijn.
admin-password | Password123 | Het wachtwoord van het beheerdersaccount. Dit wachtwoord moet tussen 8 en 128 tekens bevatten. Het wachtwoord moet tekens bevatten uit drie van de volgende categorieën: hoofdletters, kleine letters, cijfers en niet-alfanumerieke tekens.

De parameterwaarde voor de sku-naam volgt de conventie {prijscategorie} \_{compute-generatie}\_{vCores}, zoals in de onderstaande voorbeelden:
+ `--sku-name B_Gen5_4` komt overeen met Basic, Gen 5 en 4 vCores.
+ `--sku-name GP_Gen5_32` komt overeen met Algemeen gebruik, Gen 5 en 32 vCores.
+ `--sku-name MO_Gen5_2` komt overeen met Geoptimaliseerd voor geheugen, Gen 5 en 2 vCores.

Raadpleeg de documentatie over [prijscategorieën](./concepts-pricing-tiers.md) om de geldige waarden per regio en categorie te begrijpen.

In het volgende voorbeeld wordt een server in US - west gemaakt met de naam `mydemoserver` in uw resourcegroep `myresourcegroup` met aanmeldgegevens van de serverbeheerder `myadmin`. Dit is een **Gen 5**-server voor **algemeen gebruik** met twee **vCores**. De naam van een server komt overeen met een DNS-naam en moet dus globaal uniek zijn in Azure. Vervang het `<server_admin_password>` door uw eigen waarde.
```azurecli-interactive
az mariadb server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 --version 10.2
```

## <a name="configure-firewall-rule"></a>Firewallregel configureren
U kunt met de opdracht **[az mariadb server firewall rule create](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-create)** een firewallregel op serverniveau maken voor een Azure Database for MariaDB-server. Een firewallregel op serverniveau stelt een externe toepassing, zoals het opdrachtregelprogramma **mysql** of MySQL Workbench, in staat om verbinding te maken met uw server via de MariaDB-servicefirewall van Azure. 

In het volgende voorbeeld wordt een firewallregel met de naam `AllowMyIP` gemaakt, die verbindingen van een specifiek IP-adres, 192.168.0.1, toestaat. Vervang dit adres door het IP-adres of de reeks IP-adressen die overeenkomen met het IP-adres waarmee u verbinding gaat maken. 

```azurecli-interactive
az mariadb server firewall-rule create --resource-group myresourcegroup --server mydemoserver --name AllowMyIP --start-ip-address 192.168.0.1 --end-ip-address 192.168.0.1
```

> [!NOTE]
> Verbindingen met Azure Database for MariaDB communiceren via poort 3306. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 3306 mogelijk niet toegestaan. In dat geval kunt u alleen verbinding maken met uw server als uw IT-afdeling poort 3306 openstelt.
> 

## <a name="configure-ssl-settings"></a>SSL-instellingen configureren
Standaard worden SSL-verbindingen tussen uw server en clienttoepassingen afgedwongen. Deze standaardinstelling zorgt voor beveiliging van gegevens 'in beweging' door de gegevensstroom via internet te versleutelen. Om deze Quickstart makkelijker te maken, schakelen we SSL-verbindingen uit voor uw server. Dit wordt afgeraden voor productieservers. Zie [Configure SSL connectivity in your application to securely connect to Azure Database for MariaDB](./howto-configure-ssl.md) (SSL-connectiviteit in uw toepassing configureren om veilig verbinding te maken met Azure Database for MariaDB) voor meer informatie.

In het volgende voorbeeld wordt het afdwingen van SSL op uw MariaDB-server uitgeschakeld.
 
 ```azurecli-interactive
 az mariadb server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Disabled
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
  "earliestRestoreDate": null,
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

## <a name="connect-to-server-using-mysql-command-line"></a>Verbinding maken met server met behulp van het opdrachtregelprogramma mysql
Maak verbinding met de server met behulp van het opdrachtregelprogramma **mysql**. U kunt het programma [hier](https://dev.mysql.com/downloads/) downloaden en vervolgens op uw computer installeren. In plaats daarvan kunt u ook op de knop **Try It** klikken in codevoorbeelden, of op `>_` op de werkbalk rechtsboven in Azure Portal, en de **Azure Cloud Shell** starten.

Typ de volgende opdrachten: 

1. Maak verbinding met de server met behulp van het opdrachtregelprogramma **mysql**:
```azurecli-interactive
 mysql -h mydemoserver.mariadb.database.azure.com -u myadmin@mydemoserver -p
```

2. Serverstatus bekijken op de opdrachtprompt van mysql:
```sql
status
```
Als alles goed gaat, geeft het opdrachtregelprogramma de volgende tekst weer:

```bash
C:\Users\>mysql -h mydemoserver.mariadb.database.azure.com -u myadmin@mydemoserver -p
Enter password: ***********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65512
Server version: 5.6.39.0 MariaDB Server

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.7.23, for Linux (x86_64)

Connection id:          64681
Current database:
Current user:           myadmin@40.118.201.21
SSL:                    Cipher in use is AES256-SHA
Current pager:          stdout
Using outfile:          ''
Using delimiter:        ;
Server version:         5.6.39.0 MariaDB Server
Protocol version:       10
Connection:             mydemoserver.mariadb.database.azure.com via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    utf8
Conn.  characterset:    utf8
TCP port:               3306
Uptime:                 1 day 3 hours 28 min 50 sec

Threads: 10  Questions: 29002  Slow queries: 0  Opens: 33  Flush tables: 3  Open tables: 1  Queries per second avg: 0.293
--------------

mysql>
```

> [!TIP]
> Zie [hoofdstuk 4.5.1 in de Engelstalige naslaghandleiding van MySQL 5.7](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) voor aanvullende opdrachten.

## <a name="connect-to-server-using-mysql-workbench"></a>Verbinding maken met de server met behulp van MySQL Workbench
1.  Start de toepassing MySQL Workbench op uw clientcomputer. U kunt MySQL Workbench [hier](https://dev.mysql.com/downloads/workbench/) downloaden en installeren.

2.  In het dialoogvenster **Nieuwe verbinding instellen** voert u de volgende gegevens in op het tabblad **Parameters**:

   ![nieuwe verbinding instellen](./media/quickstart-create-mariadb-server-database-using-azure-cli/setup-new-connection.png)

| **Instelling** | **Voorgestelde waarde** | **Beschrijving** |
|---|---|---|
|   Verbindingsnaam | Mijn verbinding | Geef een label op voor deze verbinding (dit kan van alles zijn) |
| Verbindingsmethode | kies Standard (TCP/IP) | TCP/IP-protocol gebruiken om verbinding te maken met Azure Database for MariaDB |
| Hostnaam | mydemoserver.mariadb.database.azure.com | De servernaam die u eerder hebt genoteerd. |
| Poort | 3306 | De standaardpoort voor MariaDB wordt gebruikt. |
| Gebruikersnaam | myadmin@mydemoserver | De aanmeldgegevens van een serverbeheerder die u eerder hebt genoteerd. |
| Wachtwoord | **** | Gebruik het wachtwoord voor het beheerdersaccount dat u eerder hebt geconfigureerd. |

Klik op **Verbinding testen** om te controleren of alle parameters juist zijn geconfigureerd.
U kunt nu op de verbinding klikken om verbinding te maken met de server.

## <a name="clean-up-resources"></a>Resources opschonen
Als u deze resources niet voor een andere Quickstart of zelfstudie nodig hebt, kunt u ze verwijderen met de volgende opdracht: 

```azurecli-interactive
az group delete --name myresourcegroup
```

Als u alleen de zojuist gemaakte server wilt verwijderen, kunt u de opdracht **[az mariadb server delete](/cli/azure/mariadb/server#az-mariadb-server-delete)** uitvoeren.
```azurecli-interactive
az mariadb server delete --resource-group myresourcegroup --name mydemoserver
```

<!--
## Next steps

> [!div class="nextstepaction"]
> [Design a MariaDB Database with Azure CLI](./tutorial-design-database-using-cli.md)
-->