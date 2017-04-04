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
ms.custom: quick start
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: hero-article
ms.date: 03/13/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 3d642447526c5562ae3bfad5e4a4592e33e766aa
ms.lasthandoff: 03/25/2017

---

# <a name="create-a-single-azure-sql-database-using-the-azure-cli"></a>Een individuele Azure SQL-database maken met de Azure CLI

De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. Deze handleiding geeft meer informatie over het gebruik van de Azure CLI voor het implementeren van een Azure SQL-database in een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) in een [logische Azure SQL Database-server](sql-database-features.md).

Voordat u begint, moet u controleren of de Azure-CLI is geïnstalleerd. Zie voor meer informatie de [Installatiehandleiding van de Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht [az login](/cli/azure/#login) en volg de instructies op het scherm.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) met de opdracht [az group create](/cli/azure/group#create). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt op de locatie `westeurope`.

```azurecli
az group create --name myResourceGroup --location westeurope
```
## <a name="create-a-logical-server"></a>Een logische server maken

Als u een [logische Azure SQL Database-server](sql-database-features.md) wilt maken, gebruikt u de opdracht [az sql server create](/cli/azure/sql/server#create). Een logische server bevat een groep met databases die worden beheerd als groep. In het volgende voorbeeld wordt een server met een willekeurige naam gemaakt in de resourcegroep met een beheerdersaccount met de gebruikersnaam `ServerAdmin` en het wachtwoord `ChangeYourAdminPassword1`. U kunt deze vooraf gedefinieerde waarden vervangen.

```azurecli
servername=server-$RANDOM
az sql server create --name $servername --resource-group myResourceGroup --location westeurope \
    --admin-user ServerAdmin --admin-password ChangeYourAdminPassword1
```

## <a name="configure-a-server-firewall-rule"></a>Een serverfirewallregel configureren

Maak een [Azure SQL Database-firewallregel op serverniveau](sql-database-firewall-configure.md) met de opdracht [az sql server firewall create](/cli/azure/sql/server/firewall#create). Een firewallregel op serverniveau kan een externe toepassing, zoals SQL Server Management Studio of het hulpprogramma SQLCMD verbinding laten maken met een SQL-database via de firewall van de SQL Database-service. In het volgende voorbeeld wordt een firewallregel gemaakt voor een vooraf gedefinieerd adresbereik. In dit voorbeeld is dit het volledige bereik van IP-adressen. Vervang deze vooraf gedefinieerde waarden door de waarden voor uw externe IP-adres of het IP-adresbereik. 

```azurecli
az sql server firewall-rule create --resource-group myResourceGroup --server $servername \
    -n AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

## <a name="create-a-database-in-the-server-with-sample-data"></a>Een database op de server maken met voorbeeldgegevens

Maak een database met een [prestatieniveau van S0](sql-database-service-tiers.md) op de server met de opdracht [az sql db create](/cli/azure/sql/db#create). In het volgende voorbeeld wordt een database met de naam `mySampleDatabase` gemaakt en worden de gegevens uit het AdventureWorksLT-voorbeeld in deze database geladen. U kunt deze vooraf gedefinieerde waarden desgewenst vervangen. (Andere Quick Starts in deze verzameling zijn echter op de waarden in deze Quick Start gebaseerd.)

```azurecli
az sql db create --resource-group myResourceGroup --server $servername \
    --name mySampleDatabase --sample-name AdventureWorksLT --service-objective S0
```

## <a name="clean-up-resources"></a>Resources opschonen

Andere Quick Starts in deze verzameling zijn op deze Quick Start gebaseerd. Als u van plan bent om door te gaan met andere Quick Starts of met de zelfstudies, verwijdert u de resources die u in deze Quick Start hebt gemaakt niet. Als u niet wilt doorgaan, gebruikt u de volgende opdracht om alle resources die via deze Quick Start zijn gemaakt, te verwijderen.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

- Als u verbinding wilt maken en query's wilt uitvoeren met behulp van SQL Server Management Studio, raadpleegt u [Verbinding maken en query's uitvoeren met SSMS](sql-database-connect-query-ssms.md)
- Zie [Verbinding maken en query's uitvoeren met Visual Studio](sql-database-connect-query.md) als u verbinding wilt maken met Visual Studio.
- Zie voor een technisch overzicht van de SQL Database [Over de SQL Database-service](sql-database-technical-overview.md).

