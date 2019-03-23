---
title: 'Azure Resource Manager: Een individuele database maken - Azure SQL Database | Microsoft Docs'
description: Een individuele database maken in Azure SQL Database met behulp van de Azure Resource Manager-sjabloon.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: carlrab
manager: craigg
ms.date: 03/22/2019
ms.openlocfilehash: 01e14f86b16db6d998d60e74211ae5ad77381461
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58373019"
---
# <a name="quickstart-create-a-single-database-in-azure-sql-database-using-the-azure-resource-manager-template"></a>Quickstart: Een individuele database maken in Azure SQL Database met behulp van de Azure Resource Manager-sjabloon

Het maken van [één database](sql-database-single-database.md) is de snelste een eenvoudigste implementatieoptie om een database te maken in Azure SQL Database. Deze quickstart laat zien hoe u een individuele database maken met de Azure Resource Manager-sjabloon. Zie voor meer informatie, [Azure Resource Manager-documentatie](/azure/azure-resource-manager/).

Als u nog geen abonnement op Azure hebt, [maak dan een gratis account](https://azure.microsoft.com/free/).

## <a name="create-a-single-database"></a>Een individuele database maken

Een individuele database bevat een gedefinieerde set reken-, geheugen-, IO- en opslagresources die gebruikmaakt van één van twee [aankoopmodellen](sql-database-purchase-models.md). Wanneer u een individuele database maakt, definieert u ook een [SQL Database-server](sql-database-servers.md) om die te beheren en in een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) in een opgegeven regio te plaatsen.

De sjabloon die wordt gebruikt in deze Quick Start is afkomstig van [Azure Quickstart-sjablonen](https://azure.microsoft.com/resources/templates/201-sql-threat-detection-server-policy-optional-db/). De volgende JSON-bestand is de sjabloon die wordt gebruikt in dit artikel. Meer voorbeelden van Azure SQL database-sjabloon kunnen vinden [hier](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Sql&pageNumber=1&sort=Popular).

[!code-json[create-azure-sql-database](~/quickstart-templates/201-sql-threat-detection-server-policy-optional-db/azuredeploy.json)]

1. Selecteer de volgende afbeelding om u aan te melden bij Azure en een sjabloon te openen.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-sql-threat-detection-server-policy-optional-db%2Fazuredeploy.json"><img src="./media/sql-database-single-database-get-started-template/deploy-to-azure.png" alt="deploy to azure"/></a>

2. Typ of selecteer de volgende waarden.  

    ![Resource Manager-sjabloon een azure sql-database maken](./media/sql-database-single-database-get-started-template/create-azure-sql-database-resource-manager-template.png)

    Tenzij deze is opgegeven, gebruikt u de standaardwaarden.

    * **Abonnement**: selecteer een Azure-abonnement.
    * **Resourcegroep**: Selecteer **nieuw**, voer een unieke naam voor de resourcegroep en klik vervolgens op **OK**. 
    * **Locatie**: selecteer een locatie.  Bijvoorbeeld **US - centraal**.
    * **De gebruiker met beheerdersrechten**: Geef een gebruikersnaam met SQL database server-beheerder.
    * **Beheerderswachtwoord**: Geef een administrator-wachtwoord. 
    * **Ik ga akkoord met de bovenstaande voorwaarden**: Selecteren.
3. Selecteer **Aankoop**.

## <a name="query-the-database"></a>Een query uitvoeren op de database

Als u wilt zoeken in de database, Zie [zoeken in de database](./sql-database-single-database-get-started.md#query-the-database).

## <a name="clean-up-resources"></a>Resources opschonen

Behoud deze resourcegroep, databaseserver en individuele database als u naar de [Volgende stappen](#next-steps) wilt gaan. De volgende stappen laten zien hoe u verbinding maakt met en een query uitvoert op uw database met behulp van verschillende methoden.

De resourcegroep verwijderen met behulp van Azure CLI of Azure Powershell:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName 
```

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName 
```

## <a name="next-steps"></a>Volgende stappen

- Maak een firewallregel op serverniveau om via on-premises of externe hulpprogramma's verbinding met de individuele database te maken. Zie [Een serverfirewallregel maken](sql-database-server-level-firewall-rule.md) voor meer informatie.
- Nadat u een serverfirewallregel hebt gemaakt, kunt u met verschillende hulpprogramma's en programmeertalen [verbinding maken met uw database en query's uitvoeren](sql-database-connect-query.md) op uw database.
  - [Verbinding maken en query's uitvoeren met behulp van SQL Server Management Studio](sql-database-connect-query-ssms.md)
  - [Verbinding maken en query's uitvoeren met behulp van Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Zie [Azure CLI-voorbeelden](sql-database-cli-samples.md) voor het maken van individuele databases met behulp van Azure CLI.
- Zie [Azure PowerShell-voorbeelden](sql-database-powershell-samples.md) voor het maken van individuele databases met behulp van Azure PowerShell.
