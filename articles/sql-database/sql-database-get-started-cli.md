---
title: 'Azure CLI: een SQL-database maken | Microsoft Docs'
description: Meer informatie over hoe u met behulp van de Azure CLI een logische SQL Database-server, een firewallregel op serverniveau en databases maakt.
keywords: zelfstudie over sql-database, een sql-database maken
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: quick start create
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: 06b6830b28745b0f6574d7bca5cca7907db8ecb1
ms.lasthandoff: 04/18/2017

---

# <a name="create-a-single-azure-sql-database-using-the-azure-cli"></a>Een individuele Azure SQL-database maken met de Azure CLI

De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. Deze handleiding geeft meer informatie over het gebruik van de Azure CLI voor het implementeren van een Azure SQL-database in een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) in een [logische Azure SQL Database-server](sql-database-features.md).

Controleer of de nieuwste versie van [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) is geïnstalleerd om deze Quick Start te voltooien. 

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht [az login](/cli/azure/#login) en volg de instructies op het scherm.

```azurecli
az login
```

## <a name="define-variables"></a>Variabelen definiëren

Definieer variabelen voor gebruik in de scripts in deze Quick Start.

```azurecli
# The data center and resource name for your resources
resourcegroupname = myResourceGroup
location = westeurope
# The logical server name: Use a random value or replace with your own value (do not capitalize)
servername = server-$RANDOM
# Set an admin login and password for your database
adminlogin = ServerAdmin
password = ChangeYourAdminPassword1
# The ip address range that you want to allow to access your DB
startip = "0.0.0.0"
endip = "0.0.0.1"
# The database name
databasename = mySampleDatabase
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) met de opdracht [az group create](/cli/azure/group#create). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt op de locatie `westeurope`.

```azurecli
az group create --name $resourcegroupname --location $location
```
## <a name="create-a-logical-server"></a>Een logische server maken

Als u een [logische Azure SQL Database-server](sql-database-features.md) wilt maken, gebruikt u de opdracht [az sql server create](/cli/azure/sql/server#create). Een logische server bevat een groep met databases die worden beheerd als groep. In het volgende voorbeeld wordt een server met een willekeurige naam gemaakt in de resourcegroep met een beheerdersaccount met de gebruikersnaam `ServerAdmin` en het wachtwoord `ChangeYourAdminPassword1`. U kunt deze vooraf gedefinieerde waarden vervangen.

```azurecli
az sql server create --name $servername --resource-group $resourcegroupname --location $location \
    --admin-user $adminlogin --admin-password $password
```

## <a name="configure-a-server-firewall-rule"></a>Een serverfirewallregel configureren

Maak een [Azure SQL Database-firewallregel op serverniveau](sql-database-firewall-configure.md) met de opdracht [az sql server firewall create](/cli/azure/sql/server/firewall-rule#create). Een firewallregel op serverniveau kan een externe toepassing, zoals SQL Server Management Studio of het hulpprogramma SQLCMD verbinding laten maken met een SQL-database via de firewall van de SQL Database-service. In het volgende voorbeeld wordt de firewall alleen geopend voor andere Azure-resources. Voor externe connectiviteit wijzigt u het IP-adres in een correct adres voor uw omgeving. Als u alle IP-adressen wilt openen, gebruikt u 0.0.0.0 als beginadres en 255.255.255.255 als eindadres.  

```azurecli
az sql server firewall-rule create --resource-group $resourcegroupname --server $servername \
    -n AllowYourIp --start-ip-address $startip --end-ip-address $endip
```

> [!NOTE]
> SQL Database communiceert via poort 1433. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 1433 mogelijk niet toegestaan door de firewall van uw netwerk. In dat geval kunt u alleen verbinding maken met uw Azure SQL Database-server als uw IT-afdeling poort 1433 openstelt.
>

## <a name="create-a-database-in-the-server-with-sample-data"></a>Een database op de server maken met voorbeeldgegevens

Maak een database met een [prestatieniveau van S0](sql-database-service-tiers.md) op de server met de opdracht [az sql db create](/cli/azure/sql/db#create). In het volgende voorbeeld wordt een database met de naam `mySampleDatabase` gemaakt en worden de gegevens uit het AdventureWorksLT-voorbeeld in deze database geladen. U kunt deze vooraf gedefinieerde waarden desgewenst vervangen. (Andere Quick Starts in deze verzameling zijn echter op de waarden in deze Quick Start gebaseerd.)

```azurecli
az sql db create --resource-group $resourcegroupname --server $servername \
    --name $databasename --sample-name AdventureWorksLT --service-objective S0
```

## <a name="clean-up-resources"></a>Resources opschonen

Andere Quick Starts in deze verzameling zijn op deze Quick Start gebaseerd. Als u van plan bent om door te gaan met andere Quick Starts of met de zelfstudies, verwijdert u de resources die u in deze Quick Start hebt gemaakt niet. Als u niet wilt doorgaan, gebruikt u de volgende opdracht om alle resources die via deze Quick Start zijn gemaakt, te verwijderen.

```azurecli
az group delete --name $resourcegroupname
```

## <a name="next-steps"></a>Volgende stappen

- Als u verbinding wilt maken en query's wilt uitvoeren met behulp van SQL Server Management Studio, raadpleegt u [Verbinding maken en query's uitvoeren met SSMS](sql-database-connect-query-ssms.md)
- Zie [Verbinding maken en query's uitvoeren met Visual Studio Code](sql-database-connect-query-vscode.md) als u verbinding wilt maken en query’s wilt uitvoeren met Visual Studio Code.
- Zie [Verbinding maken en query’s uitvoeren met .NET](sql-database-connect-query-dotnet.md) als u verbinding wilt maken en query’s wilt uitvoeren met .NET.
- Zie [Verbinding maken en query's uitvoeren met PHP](sql-database-connect-query-php.md) als u verbinding wilt maken en query's wilt uitvoeren met PHP.
- Zie [Verbinding maken en query's uitvoeren met Node.js](sql-database-connect-query-nodejs.md) als u verbinding wilt maken en query's wilt uitvoeren met Node.js.
- Zie [Verbinding maken en query's uitvoeren met Java](sql-database-connect-query-java.md) als u verbinding wilt maken en query's wilt uitvoeren met Java.
- Zie [Verbinding maken en query's uitvoeren met Python](sql-database-connect-query-python.md) als u verbinding wilt maken en query's wilt uitvoeren met Python.
- Zie [Verbinding maken en query's uitvoeren met Ruby](sql-database-connect-query-ruby.md) als u verbinding wilt maken en query's wilt uitvoeren met Ruby.

