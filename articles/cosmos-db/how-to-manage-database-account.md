---
title: Informatie over het beheren van databaseaccounts in Azure Cosmos DB
description: Informatie over het beheren van databaseaccounts in Azure Cosmos DB
services: cosmos-db
author: christopheranderson
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/17/2018
ms.author: chrande
ms.openlocfilehash: 0683516d16bf1501eee83901c5171811b8c0e44d
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621544"
---
# <a name="manage-database-accounts-in-azure-cosmos-db"></a>Databaseaccounts beheren in Azure Cosmos DB

In dit artikel wordt beschreven hoe u uw Azure Cosmos DB-account beheert voor het instellen van multi-homing, het toevoegen/verwijderen van een regio, het configureren van meerdere schrijfregio's, en het instellen van failover-prioriteiten. 

## <a name="create-a-database-account"></a>Een databaseaccount maken

### <a id="create-database-account-via-portal"></a>Azure-portal

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

### <a id="create-database-account-via-cli"></a>Azure CLI

```bash
# Create an account
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group Name>
```

## <a name="configure-clients-for-multi-homing"></a>Clients configureren voor multi-homing

### <a id="configure-clients-multi-homing-dotnet"></a>.NET SDK

```csharp
// Create a new Connection Policy
ConnectionPolicy policy = new ConnectionPolicy
    {
        // Note: These aren't required settings for multi-homing,
        // just suggested defaults
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true,
    };
// Add regions to Preferred locations
// The name of the location will match what you see in the portal/etc.
policy.PreferredLocations.Add("East US");
policy.PreferredLocations.Add("North Europe");

// Pass the Connection policy with the preferred locations on it to the client.
DocumentClient client = new DocumentClient(new Uri(this.accountEndpoint), this.accountKey, policy);
```

### <a id="configure-clients-multi-homing-java-async"></a>Java Async SDK

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setPreferredLocations(Collections.singleton("West US"));
AsyncDocumentClient client =
        new AsyncDocumentClient.Builder()
                .withMasterKey(this.accountKey)
                .withServiceEndpoint(this.accountEndpoint)
                .withConnectionPolicy(policy).build();
```

### <a id="configure-clients-multi-homing-java-sync"></a>Java Sync SDK

```java
ConnectionPolicy connectionPolicy = new ConnectionPolicy();
Collection<String> preferredLocations = new ArrayList<String>();
preferredLocations.add("Australia East");
connectionPolicy.setPreferredLocations(preferredLocations);
DocumentClient client = new DocumentClient(accountEndpoint, accountKey, connectionPolicy);
```

### <a id="configure-clients-multi-homing-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
// Set up the connection policy with your preferred regions
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.PreferredLocations = ["West US", "Australia East"];

// Pass that connection policy to the client
const client = new CosmosClient({
  endpoint: config.endpoint,
  auth: { masterKey: config.key },
  connectionPolicy
});
```

### <a id="configure-clients-multi-homing-python"></a>Python SDK

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.PreferredLocations = ['West US', 'Japan West']
client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy)

```

## <a name="addremove-regions-from-your-database-account"></a>Regio's toevoegen aan of verwijderen uit uw databaseaccount

### <a id="add-remove-regions-via-portal"></a>Azure-portal

1. Ga naar uw Azure Cosmos DB-account en open het menu **Gegevens globaal repliceren**.

2. Als u regio's wilt toevoegen, selecteert u een of meer regio's op de kaart door te klikken op de lege zeshoeken met het label **"+"**, overeenkomend met de gewenste regio. U kunt een regio ook toevoegen door de optie **+ Regio toevoegen** te selecteren en een regio in de vervolgkeuzelijst te kiezen.

3. Als u regio's wilt verwijderen, deselecteert u een of meer regio's op de kaart door op de blauwe zeshoeken met een vinkje te klikken of door naast de regio aan de rechterkant het pictogram met de 'prullenmand' (🗑) te selecteren.

4. Klik op Opslaan om de wijzigingen op te slaan.

   ![Menu Regio's toevoegen/verwijderen](./media/how-to-manage-database-account/add-region.png)

In de schrijfmodus voor één regio kunt u de schrijfregio niet verwijderen. U dient dan failover toe te passen naar een andere regio voordat u de huidige schrijfregio kunt verwijderen.

In de schrijfmodus voor meerdere regio's, kunt u elke regio toevoegen of verwijderen, zolang er ten minste één regio is.

### <a id="add-remove-regions-via-cli"></a>Azure CLI

```bash
# Given an account created with 1 region like so
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'eastus=0'

# Add a new region by adding another region to the list
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'eastus=0 westus=1'

# Remove a region by removing a region from the list
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'westus=0'
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

De volgende JSON-code is een voorbeeld van een Resource Manager-sjabloon. U kunt de code gebruiken om een Azure Cosmos-account te implementeren met een consistentiebeleid als Gebonden veroudering, een maximaal verouderingsinterval van 5 seconden en een maximum aantal getolereerde verouderde aanvragen van 100. Zie de documentatie over [Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) voor informatie over de indeling en de syntaxis van de Resource Manager-sjabloon.

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

1. Ga naar uw Azure Cosmos-account en open het menu **Gegevens globaal repliceren**.

2. Klik boven in het menu op de knop **Handmatige failover**.

   ![Menu Gegevens globaal repliceren](./media/how-to-manage-database-account/replicate-data-globally.png)

3. Selecteer in het menu **Handmatige failover** de nieuwe schrijfregio en schakel het vakje in om aan te geven dat u begrijpt dat u met deze optie de schrijfregio wijzigt.

4. Klik op OK om de failover te activeren.

   ![Portalmenu Handmatige failover](./media/how-to-manage-database-account/manual-failover.png)

### <a id="enable-manual-failover-via-cli"></a>Azure CLI

```bash
# Given your account currently has regions with priority like so: 'eastus=0 westus=1'
# Change the priority order to trigger a failover of the write region
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'eastus=1 westus=0'
```

## <a id="automatic-failover"></a>Automatische failover inschakelen voor uw Azure Cosmos-account

### <a id="enable-automatic-failover-via-portal"></a>Azure-portal

1. Ga naar uw Azure Cosmos DB-account en open het deelvenster **Gegevens globaal repliceren**. 

2. Klik boven in het deelvenster op de knop **Automatische failover**.

   ![Menu Gegevens globaal repliceren](./media/how-to-manage-database-account/replicate-data-globally.png)

3. Controleer in het deelvenster **Automatische failover** of **Automatische failover inschakelen** is ingesteld op **AAN**. 

4. Klik op Opslaan onder aan het menu.

   ![Portalmenu Automatische failover](./media/how-to-manage-database-account/automatic-failover.png)

U kunt uw failoverprioriteiten ook in dit menu instellen.

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

1. Ga naar uw Azure Cosmos DB-account en open het deelvenster **Gegevens globaal repliceren**. 

2. Klik boven in het deelvenster op de knop **Automatische failover**.

   ![Menu Gegevens globaal repliceren](./media/how-to-manage-database-account/replicate-data-globally.png)

3. Controleer in het deelvenster **Automatische failover** of **Automatische failover inschakelen** is ingesteld op **AAN**. 

4. U kunt de failoverprioriteit wijzigen door de leesregio's te slepen. Dit doet u via de drie puntjes aan de linkerkant van de rij die wordt weergegeven als u de muisaanwijzer erboven houdt. 

5. Klik op Opslaan onder aan het menu.

   ![Portalmenu Automatische failover](./media/how-to-manage-database-account/automatic-failover.png)

U kunt in dit menu geen schrijfregio wijzigen. U moet een handmatige failover uitvoeren als u de schrijfregio handmatig wilt wijzigen.

### <a id="set-failover-priorities-via-cli"></a>Azure CLI

```bash
az cosmosdb failover-priority-change --name <Azure Cosmos account name> --resource-group <Resource Group name> --failover-policies 'eastus=0 westus=2 southcentralus=1'
```

## <a name="next-steps"></a>Volgende stappen

In de volgende documenten kunt u meer informatie vinden over het beheren van consistentieniveaus en gegevensconflicten in Azure Cosmos DB:

* [Consistentie beheren](how-to-manage-consistency.md)
* [Conflicten tussen regio 's beheren](how-to-manage-conflicts.md)

