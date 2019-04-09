---
title: Informatie over het beheren van databaseaccounts in Azure Cosmos DB
description: Informatie over het beheren van databaseaccounts in Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 04/08/2019
ms.author: rimman
ms.openlocfilehash: b2b5e58ca480aa3abaa0766319977b8d1160ebeb
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59282998"
---
# <a name="manage-an-azure-cosmos-account"></a>Een Azure Cosmos-account beheren

In dit artikel wordt beschreven hoe u uw Azure Cosmos-account beheren. U leert hoe u multihoming instellen, toevoegen of verwijderen van een regio, meerdere schrijfregio's configureren en instellen van failover-prioriteiten. 

## <a name="create-a-database-account"></a>Een databaseaccount maken

### <a id="create-database-account-via-portal"></a>Azure-portal

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

### <a id="create-database-account-via-cli"></a>Azure CLI

```bash
# Create an account
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group Name>
```

## <a name="configure-clients-for-multi-homing"></a>Clients configureren voor multi-homing

### <a id="configure-clients-multi-homing-dotnet"></a>.NET SDK v2

```csharp
ConnectionPolicy policy = new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true
    };
policy.SetCurrentLocation("West US 2");

// Pass the connection policy with the preferred locations on it to the client.
DocumentClient client = new DocumentClient(new Uri(this.accountEndpoint), this.accountKey, policy);
```

### <a id="configure-clients-multi-homing-dotnet-v3"></a>.NET SDK v3 (preview)

```csharp
CosmosConfiguration config = new CosmosConfiguration("endpoint", "key");
config.UseCurrentRegion("West US");
CosmosClient client = new CosmosClient(config);
```

### <a id="configure-clients-multi-homing-java-async"></a>Java Async SDK

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setUsingMultipleWriteLocations(true);
policy.setPreferredLocations(Collections.singletonList(region));

AsyncDocumentClient client =
    new AsyncDocumentClient.Builder()
        .withMasterKeyOrResourceToken(this.accountKey)
        .withServiceEndpoint(this.accountEndpoint)
        .withConsistencyLevel(ConsistencyLevel.Eventual)
        .withConnectionPolicy(policy).build();
```

### <a id="configure-clients-multi-homing-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.UseMultipleWriteLocations = true;
connectionPolicy.PreferredLocations = [region];

const client = new CosmosClient({
  endpoint: config.endpoint,
  auth: { masterKey: config.key },
  connectionPolicy,
  consistencyLevel: ConsistencyLevel.Eventual
});
```

### <a id="configure-clients-multi-homing-python"></a>Python SDK

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.UseMultipleWriteLocations = True
connection_policy.PreferredLocations = [region]

client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Session)
```

## <a name="addremove-regions-from-your-database-account"></a>Regio's toevoegen aan of verwijderen uit uw databaseaccount

### <a id="add-remove-regions-via-portal"></a>Azure-portal

1. Ga naar uw Azure Cosmos-account en open de **gegevens globaal repliceren** menu.

2. Selecteer de Zeshoeken op de kaart met regio's toevoegen, de **+** label die overeenkomen met uw gewenste regio('s). U kunt ook om toe te voegen een regio, selecteer de **+ toevoegen regio** optie en kiest u een regio in de vervolgkeuzelijst.

3. Als u regio's wilt verwijderen, wist u een of meer regio's van de kaart door de blauwe zeshoeken met vinkjes te selecteren. Of selecteer het prullenbakpictogram (🗑) rechts van de regio.

4. Selecteer **OK** om uw wijzigingen op te slaan.

   ![Menu Regio's toevoegen of verwijderen](./media/how-to-manage-database-account/add-region.png)

Schrijf in één regio modus, dat kunt u de schrijfregio niet verwijderen. U moet een failover naar een andere regio voordat u de huidige schrijfregio kunt verwijderen.

In een meerdere regio's schrijven modus, u kunt toevoegen of verwijderen van elke gewenste regio, hebt u ten minste één regio.

### <a id="add-remove-regions-via-cli"></a>Azure CLI

```bash
# Create an account with 1 region
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations eastus=0

# Add a region
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations eastus=0 westus=1

# Remove a region
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations westus=0
```

## <a name="configure-multiple-write-regions"></a>Meerdere schrijfregio's configureren

### <a id="configure-multiple-write-regions-portal"></a>Azure-portal

Als u een databaseaccount maakt, zorg er dan voor dat de instelling **Multi-region Writes** is ingeschakeld.

![Schermopname van het maken van een Azure Cosmos DB-account](./media/how-to-manage-database-account/account-create.png)

### <a id="configure-multiple-write-regions-cli"></a>Azure CLI

```bash
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-multiple-write-locations true
```

### <a id="configure-multiple-write-regions-arm"></a>Resource Manager-sjabloon

De volgende JSON-code is een voorbeeld van een [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) sjabloon. U kunt deze gebruiken voor het implementeren van een Azure Cosmos-account met [gebonden veroudering consistentieniveau](consistency-levels.md). De maximale veroudering-interval is ingesteld op 5 seconden. Het maximum aantal verlopen aanvragen dat is toegestaan is ingesteld op 100. Zie [Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) voor informatie over de indeling en syntaxis van de Resource Manager-sjabloon.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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
                "consistencyPolicy": {
                    "defaultConsistencyLevel": "BoundedStaleness",
                    "maxIntervalInSeconds": 5,
                    "maxStalenessPrefix": 100
                },
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


## <a id="manual-failover"></a>Handmatige failover inschakelen voor uw Azure Cosmos-account

### <a id="enable-manual-failover-via-portal"></a>Azure-portal

1. Ga naar uw Azure Cosmos-account en open de **gegevens globaal repliceren** menu.

2. Selecteer bovenaan het menu de optie **Handmatige failover**.

   ![Menu Gegevens globaal repliceren](./media/how-to-manage-database-account/replicate-data-globally.png)

3. Selecteer in het menu **Handmatige failover** de nieuwe schrijfregio. Schakel het selectievakje in om aan te geven dat u begrijpt dat met deze optie de schrijfregio wordt gewijzigd.

4. Selecteer **OK** om de failover te activeren.

   ![Portalmenu Handmatige failover](./media/how-to-manage-database-account/manual-failover.png)

### <a id="enable-manual-failover-via-cli"></a>Azure CLI

```bash
# Given your account currently has regions with priority: eastus=0 westus=1
# Change the priority order to trigger a failover of the write region
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations westus=0 eastus=1
```

## <a id="automatic-failover"></a>Automatische failover inschakelen voor uw Azure Cosmos DB-account

### <a id="enable-automatic-failover-via-portal"></a>Azure-portal

1. Open vanuit uw Azure Cosmos DB-account het deelvenster **Gegevens globaal repliceren**. 

2. Selecteer bovenin het deelvenster de knop **Automatische failover**.

   ![Menu Gegevens globaal repliceren](./media/how-to-manage-database-account/replicate-data-globally.png)

3. Controleer in het deelvenster **Automatische failover** of **Automatische failover inschakelen** is ingesteld op **AAN**. 

4. Selecteer **Opslaan**.

   ![Portalmenu Automatische failover](./media/how-to-manage-database-account/automatic-failover.png)

U kunt in dit menu ook uw failoverprioriteiten instellen.

### <a id="enable-automatic-failover-via-cli"></a>Azure CLI

```bash
# Enable automatic failover on account creation
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-automatic-failover true

# Enable automatic failover on an existing account
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-automatic-failover true

# Disable automatic failover on an existing account
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-automatic-failover false
```

## <a name="set-failover-priorities-for-your-azure-cosmos-account"></a>Failoverprioriteiten instellen voor uw Azure Cosmos-account

### <a id="set-failover-priorities-via-portal"></a>Azure-portal

1. Open in uw Azure Cosmos-account, de **gegevens globaal repliceren** deelvenster. 

2. Selecteer bovenin het deelvenster de knop **Automatische failover**.

   ![Menu Gegevens globaal repliceren](./media/how-to-manage-database-account/replicate-data-globally.png)

3. Controleer in het deelvenster **Automatische failover** of **Automatische failover inschakelen** is ingesteld op **AAN**. 

4. Als u de failoverprioriteit wilt wijzigen, sleept u de leesregio's met behulp van de drie puntjes aan de linkerkant van de rij, die worden weergegeven wanneer u de muisaanwijzer erboven houdt. 

5. Selecteer **Opslaan**.

   ![Portalmenu Automatische failover](./media/how-to-manage-database-account/automatic-failover.png)

U kunt in dit menu geen schrijfregio wijzigen. U moet een handmatige failover uitvoeren als u de schrijfregio handmatig wilt wijzigen.

### <a id="set-failover-priorities-via-cli"></a>Azure CLI

```bash
# Assume region order is initially eastus=0 westus=1 automatic failover on account creation
az cosmosdb failover-priority-change --name <Azure Cosmos account name> --resource-group <Resource Group name> --failover-policies westus=0 eastus=1
```

## <a name="next-steps"></a>Volgende stappen

Lees de volgende artikelen:

* [Consistentie beheren](how-to-manage-consistency.md)
* [Conflicten tussen regio 's beheren](how-to-manage-conflicts.md)
* [Wereldwijde distributie - achter de schermen](global-dist-under-the-hood.md)
* [Meerdere masters in uw toepassingen configureren](how-to-multi-master.md)
* [Clients configureren voor multihoming](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Toevoegen of verwijderen van regio's van uw Azure Cosmos DB-account](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Aangepast conflictoplossingsbeleid maken](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)

