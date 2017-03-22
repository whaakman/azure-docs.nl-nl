---
title: "Azure CLI: één SQL-database maken en er query&quot;s op uitvoeren | Microsoft Docs"
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
ms.devlang: cli
ms.topic: hero-article
ms.date: 03/13/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: 8322e46f462b6c940f9808411d99aa1cee0beea5
ms.lasthandoff: 03/15/2017

---

# <a name="create-and-query-a-single-azure-sql-database-with-the-azure-cli"></a>Eén Azure SQL-database maken en er query's op uitvoeren met de Azure CLI

De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. Deze handleiding geeft u meer informatie over het implementeren van een Azure SQL-database met behulp van de Azure CLI.

Voordat u begint, moet u controleren of de Azure-CLI is geïnstalleerd. Zie voor meer informatie de [Installatiehandleiding van de Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht [az login](/cli/azure/#login) en volg de instructies op het scherm.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt op de locatie `westeurope`.

```azurecli
az group create --name myResourceGroup --location westeurope
```
## <a name="create-a-logical-server"></a>Een logische server maken

Maak een logische server met de opdracht [az sql server create](/cli/azure/sql/server#create). In het volgende voorbeeld wordt een server met een willekeurige naam gemaakt in de resourcegroep met een beheerdersaccount met de naam `ServerAdmin` en het wachtwoord `ChangeYourAdminPassword1`. U kunt deze vooraf gedefinieerde waarden vervangen.

```azurecli
servername=server-$RANDOM
az sql server create --name $servername --resource-group myResourceGroup --location westeurope \
    --admin-user ServerAdmin --admin-password ChangeYourAdminPassword1
```

## <a name="configure-a-server-firewall-rule"></a>Een serverfirewallregel configureren

Maak een firewallregel op serverniveau met de opdracht [az sql server firewall create](/cli/azure/sql/server/firewall-rule#create). Een firewallregel op serverniveau kan een externe toepassing, zoals SQL Server Management Studio of het hulpprogramma SQLCMD verbinding laten maken met een SQL-database via de firewall van de SQL Database-service. In het volgende voorbeeld wordt een firewallregel gemaakt voor een vooraf gedefinieerd adresbereik. In dit voorbeeld is dit het volledige bereik van IP-adressen. Vervang deze vooraf gedefinieerde waarden door de waarden voor uw externe IP-adres of het IP-adresbereik. 

```azurecli
az sql server firewall-rule create --resource-group myResourceGroup --server $servername \
    -n AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

## <a name="create-a-database-in-the-server"></a>Een database op de server maken

Maak een database op de server met de opdracht [az sql db create](/cli/azure/sql/db#create). In het volgende voorbeeld wordt een lege database genaamd `mySampleDatabase` gemaakt. Vervang deze vooraf gedefinieerde waarde door een waarde naar wens.

```azurecli
az sql db create --resource-group myResourceGroup --server $servername \
    --name mySampleDatabase --service-objective S0
```

## <a name="clean-up-resources"></a>Resources opschonen

Voer de volgende opdracht uit als u alle resources wilt verwijderen die door deze snelstartgids zijn gemaakt:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

- Als u verbinding wilt maken en query's wilt uitvoeren met behulp van SQL Server Management Studio, raadpleegt u [Verbinding maken en query's uitvoeren met SSMS](sql-database-connect-query-ssms.md)
- Zie [Verbinding maken en query's uitvoeren met Visual Studio](sql-database-connect-query.md) als u verbinding wilt maken met Visual Studio.
- Zie voor een technisch overzicht van de SQL Database [Over de SQL Database-service](sql-database-technical-overview.md).

