---
title: Informatie over het beheren van databaseaccounts in Azure Cosmos DB
description: Informatie over het beheren van databaseaccounts in Azure Cosmos DB
services: cosmos-db
author: christopheranderson
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/17/2018
ms.author: chrande
ms.openlocfilehash: 9fe8142c4fce45adaa3697023eab3ac9dbb55f37
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52635659"
---
# <a name="manage-database-accounts-in-azure-cosmos-db"></a>Databaseaccounts beheren in Azure Cosmos DB

In dit artikel wordt beschreven hoe u uw Azure Cosmos DB-account beheert. U leert hoe u multi-homing instelt, een regio toevoegt of verwijdert, meerdere schrijfregio's configureert en failover-prioriteiten instelt. 

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
// Create a new connection policy.
ConnectionPolicy policy = new ConnectionPolicy
    {
        // Note: These aren't required settings for multi-homing,
        // just suggested defaults.
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true,
    };
// Add regions to preferred locations.
// The name of the location will match what you see in the portal, etc.
policy.PreferredLocations.Add("East US");
policy.PreferredLocations.Add("North Europe");

// Pass the connection policy with the preferred locations on it to the client.
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
// Set up the connection policy with your preferred regions.
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.PreferredLocations = ["West US", "Australia East"];

// Pass that connection policy to the client.
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

2. Als u regio's wilt toevoegen, selecteert u de zeshoeken op de kaart met het label **+** dat overeenkomt met de gewenste regio. Als u een regio wilt toevoegen, selecteert u de optie **+ Regio toevoegen** en kiest u een regio in de vervolgkeuzelijst.

3. Als u regio's wilt verwijderen, wist u een of meer regio's van de kaart door de blauwe zeshoeken met vinkjes te selecteren. Of selecteer het prullenbakpictogram (🗑) rechts van de regio.

4. Selecteer **OK** om uw wijzigingen op te slaan.

   ![Menu Regio's toevoegen of verwijderen](./media/how-to-manage-database-account/add-region.png)

In de schrijfmodus voor één regio kunt u de schrijfregio niet verwijderen. U dient dan failover toe te passen naar een andere regio voordat u de huidige schrijfregio kunt verwijderen.

In de schrijfmodus voor meerdere regio's kunt u elke regio toevoegen of verwijderen als er ten minste één regio is.

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

De volgende JSON-code is een voorbeeld van een Azure Resource Manager-sjabloon. U kunt deze gebruiken om een Azure Cosmos DB-account te implementeren met een consistentiebeleid van gebonden veroudering. Het maximale verouderingsinterval is ingesteld op 5 seconden. Het maximum aantal verouderingsaanvragen dat is toegelaten, is ingesteld op 100. Zie [Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) voor informatie over de indeling en syntaxis van de Resource Manager-sjabloon.

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


## <a id="manual-failover"></a>Handmatige failover inschakelen voor uw Azure Cosmos DB-account

### <a id="enable-manual-failover-via-portal"></a>Azure-portal

1. Ga naar uw Azure Cosmos DB-account en open het menu **Gegevens globaal repliceren**.

2. Selecteer bovenaan het menu de optie **Handmatige failover**.

   ![Menu Gegevens globaal repliceren](./media/how-to-manage-database-account/replicate-data-globally.png)

3. Selecteer in het menu **Handmatige failover** de nieuwe schrijfregio. Schakel het selectievakje in om aan te geven dat u begrijpt dat met deze optie de schrijfregio wordt gewijzigd.

4. Selecteer **OK** om de failover te activeren.

   ![Portalmenu Handmatige failover](./media/how-to-manage-database-account/manual-failover.png)

### <a id="enable-manual-failover-via-cli"></a>Azure CLI

```bash
# Given your account currently has regions with priority like so: 'eastus=0 westus=1'
# Change the priority order to trigger a failover of the write region
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'eastus=1 westus=0'
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

## <a name="set-failover-priorities-for-your-azure-cosmos-db-account"></a>Failoverprioriteiten instellen voor uw Azure Cosmos DB-account

### <a id="set-failover-priorities-via-portal"></a>Azure-portal

1. Open vanuit uw Azure Cosmos DB-account het deelvenster **Gegevens globaal repliceren**. 

2. Selecteer bovenin het deelvenster de knop **Automatische failover**.

   ![Menu Gegevens globaal repliceren](./media/how-to-manage-database-account/replicate-data-globally.png)

3. Controleer in het deelvenster **Automatische failover** of **Automatische failover inschakelen** is ingesteld op **AAN**. 

4. Als u de failoverprioriteit wilt wijzigen, sleept u de leesregio's met behulp van de drie puntjes aan de linkerkant van de rij, die worden weergegeven wanneer u de muisaanwijzer erboven houdt. 

5. Selecteer **Opslaan**.

   ![Portalmenu Automatische failover](./media/how-to-manage-database-account/automatic-failover.png)

In dit menu kunt u niet de schrijfregio wijzigen. U moet een handmatige failover uitvoeren als u de schrijfregio handmatig wilt wijzigen.

### <a id="set-failover-priorities-via-cli"></a>Azure CLI

```bash
az cosmosdb failover-priority-change --name <Azure Cosmos account name> --resource-group <Resource Group name> --failover-policies 'eastus=0 westus=2 southcentralus=1'
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het beheer van consistentieniveaus en gegevensconflicten in Azure Cosmos DB. Zie de volgende artikelen:

* [Consistentie beheren](how-to-manage-consistency.md)
* [Conflicten tussen regio 's beheren](how-to-manage-conflicts.md)

