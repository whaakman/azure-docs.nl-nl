---
title: Meerdere masters voor Azure Cosmos DB-accounts inschakelen
description: In dit artikel wordt beschreven hoe u ondersteuning voor meerdere masters tijdens het maken van een Azure Cosmos DB-account met Azure portal, PowerShell, CLI of een Azure Resource Manager-sjabloon.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: b6fc04fc728f753dc8a3900b26c6c01f03cc7710
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49077235"
---
# <a name="enable-multi-master-for-azure-cosmos-db-accounts"></a>Meerdere masters voor Azure Cosmos DB-accounts inschakelen

Ondersteuning voor meerdere masters is ingeschakeld tijdens het maken van een Azure Cosmos DB-account. Een Azure Cosmos DB-account kan worden gemaakt met de Azure-portal, PowerShell, CLI of een Azure Resource Manager-sjabloon.

> [!IMPORTANT]
> Ondersteuning voor meerdere masters kan op dit moment worden ingeschakeld voor nieuwe Azure Cosmos DB-accounts. Bestaande Azure Cosmos DB-accounts functie niet gebruiken. We zijn bezig om aan te bieden ondersteuning voor bestaande accounts en deze ondersteuning zal aankondigen wanneer deze beschikbaar is.

Nadat u een Azure Cosmos DB-account met ondersteuning voor meerdere masters hebt gemaakt, kunt u maken van databases, containers, documenten uploaden en conflict resolutie beleid toewijzen. Zie voor conflictoplossing in meerdere masters en codevoorbeelden, [meerdere masters codevoorbeelden](multi-master-conflict-resolution.md#code-samples) artikel.

## <a name="enable-multi-master-using-azure-portal"></a>Meerdere masters van inschakelen met behulp van Azure portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com/)

2. Klik op selecteren **een resource maken > Databases > Azure Cosmos DB**.

3. In de **nieuw Account** deelvenster, voer de volgende instellingen voor een nieuw Azure Cosmos DB-account:

   |**Instelling**  |**Voorgestelde waarde** |**Beschrijving**|
   |---------|---------|---------|
   |Abonnement   | {Uw abonnement}  |Selecteer het Azure-abonnement moet worden gebruikt voor dit Azure Cosmos DB-account.  |
   |Resourcegroep  |   {De naam van de groep}    |  Selecteer een bestaande resourcegroep of selecteer **nieuw**, voert u een nieuwe resource-group-naam voor uw account. |
   |Accountnaam | {de naam van uw account}   |  Voer een unieke naam voor uw Azure Cosmos DB-account.        |
   |API  |   Alle   |  Selecteer een API-type.   |
   |Locatie  | Een willekeurige regio selecteren   | Selecteer de geografische locatie waar u het Azure Cosmos DB-account gaat hosten. U kunt elke regio omdat dit account in meerdere regio's.  |
   |Georedundantie inschakelen   |  Inschakelen  |  Selecteer op het inschakelen van multi-master die hieronder worden geselecteerd.   |
   |Meerdere masters inschakelen | Inschakelen  | Selecteer op het inschakelen van multi-model voor dit account. |


## <a name="using-multi-master-in-sdks"></a>Met behulp van meerdere masters in SDK 's

Met een account meerdere masters ingeschakeld kunt in uw toepassingen, u profiteren van meerdere mastering door gebruik te maken van de ConnectionPolicy zoals hieronder wordt weergegeven.

```csharp
ConnectionPolicy policy = new ConnectionPolicy
{
   ConnectionMode = ConnectionMode.Direct,
   ConnectionProtocol = Protocol.Tcp,
   UseMultipleWriteLocations = true,
};
policy.PreferredLocations.Add(LocationNames.WestUS);
policy.PreferredLocations.Add(LocationNames.NorthEurope);
policy.PreferredLocations.Add(LocationNames.SoutheastAsia);
```

## <a name="enable-multi-master-using-powershell"></a>Schakel meerdere masters met behulp van PowerShell

U kunt ook meerdere masters ingeschakeld Cosmos DB-account maken door in te stellen de `enableMultipleWriteLocations` parameter op 'true'. Voor het maken van een Azure Cosmos DB-account met meerdere masters ingeschakeld, open een PowerShell-venster en voer het volgende script:

```azurepowershell-interactive
$locations = @(@{"locationName"="East US"; "failoverPriority"=0},
             @{"locationName"="West US"; "failoverPriority"=1})

$iprangefilter = "<ip-range-filter>"

$consistencyPolicy = @{"defaultConsistencyLevel"="Session";
                       "maxIntervalInSeconds"= "10";
                       "maxStalenessPrefix"="200"}

$CosmosDBProperties = @{"databaseAccountOfferType"="Standard";
                        "locations"=$locations;
                        "consistencyPolicy"=$consistencyPolicy;
                        "ipRangeFilter"=$iprangefilter;
                        "enableMultipleWriteLocations"="true"}

New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
  -ApiVersion "2015-04-08" `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -Name "myCosmosDbAccount" `
  -Properties $CosmosDBProperties
```

## <a name="enable-multi-master-using-cli"></a>Schakel meerdere masters met behulp van CLI

U kunt meerdere masters inschakelen door de parameter inschakelen-meerdere-schrijven-locaties op 'true'. Open Azure CLI voor het maken van een Azure Cosmos DB-account met meerdere masters ingeschakeld, of in de cloud shell en voer de volgende opdracht uit:

```azurecli-interactive
az cosmosdb create \
   –-name "myCosmosDbAccount" \
   --resource-group "myResourceGroup" \
   --default-consistency-level "Session" \
   --enable-automatic-failover "true" \
   --locations "EastUS=0" "WestUS=1" \
   --enable-multiple-write-locations true \
```

## <a name="enable-multi-master-using-resource-manager-template"></a>Inschakelen met behulp van meerdere masters Resource Manager-sjabloon

De volgende JSON-code is een voorbeeld van de Resource Manager-sjabloon die u gebruiken kunt voor het implementeren van een Azure Cosmos DB-account. Zie voor meer informatie over de sjabloonindeling van Resource Manager-en de syntaxis, [Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) documentatie. De sleutelparameter zult in deze sjabloon is 'enableMultipleWriteLocations': ' True '.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "name": {
            "type": "String"
        },
        "location": {
            "type": "String"
        },
        "locationName": {
            "type": "String"
        },
        "defaultExperience": {
            "type": "String"
        }
    },
    "resources": [
        {
            "type": "Microsoft.DocumentDb/databaseAccounts",
            "kind": "GlobalDocumentDB",
            "name": "[parameters('name')]",
            "apiVersion": "2015-04-08",
            "location": "[parameters('location')]",
            "tags": {
                "defaultExperience": "[parameters('defaultExperience')]"
            },
            "properties": {
                "databaseAccountOfferType": "Standard",
                "locations": [
                    {
                        "id": "[concat(parameters('name'), '-', parameters('location'))]",
                        "failoverPriority": 0,
                        "locationName": "[parameters('locationName')]"
                    }
                ],
                "isVirtualNetworkFilterEnabled": false,
                "enableMultipleWriteLocations": true,
                "virtualNetworkRules": [],
                "dependsOn": []
            }
        }
    ]
}
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u meerdere masters ondersteuning voor Azure Cosmos DB-accounts inschakelen. Bekijk vervolgens de volgende bronnen:

* [Met behulp van meerdere masters met open-source NoSQL-databases](multi-master-oss-nosql.md)

* [Informatie over conflictoplossing in Azure Cosmos DB](multi-master-conflict-resolution.md)
