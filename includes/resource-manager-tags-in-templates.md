---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 03/11/2019
ms.author: tomfitz
ms.openlocfilehash: 104bd13a0cf97a8605670adde479c2a2eeb29c15
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57786325"
---
Als u wilt een bron tijdens de implementatie van labels, toevoegen de `tags` element aan de resource die u implementeert. Geef de naam en waarde van de tag op.

### <a name="apply-a-literal-value-to-the-tag-name"></a>Een letterlijke waarde toepassen op de tagnaam
In het volgende voorbeeld wordt een opslagaccount weergegeven met twee tags (`Dept` en `Environment`) die zijn ingesteld op letterlijke waarden:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

Als u wilt een tag op een datum / tijdwaarde, gebruikt de [utcNow functie](../articles/azure-resource-manager/resource-group-template-functions-string.md#utcnow).

### <a name="apply-an-object-to-the-tag-element"></a>Een object toepassen op het tagelement
U kunt een objectparameter definiëren waarmee verschillende tags worden opgeslagen en dit object vervolgens toepassen op het tagelement. Elke eigenschap in het object wordt een afzonderlijke tag voor de resource. Het volgende voorbeeld heeft een parameter met de naam `tagValues` die wordt toegepast op het tagelement.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "tagValues": {
      "type": "object",
      "defaultValue": {
        "Dept": "Finance",
        "Environment": "Production"
      }
    }
  },
  "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "tags": "[parameters('tagValues')]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {}
    }
  ]
}
```

### <a name="apply-a-json-string-to-the-tag-name"></a>Een JSON-tekenreeks toepassen op de tagnaam

Als u veel waarden wilt opslaan in een enkele tag, past u een JSON-tekenreeks toe die de waarden vertegenwoordigt. De volledige JSON-tekenreeks wordt opgeslagen als een tag met maximaal 256 tekens. Het volgende voorbeeld heeft een enkele tag met de naam `CostCenter` die verschillende waarden uit een JSON-tekenreeks bevat:  

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "tags": {
        "CostCenter": "{\"Dept\":\"Finance\",\"Environment\":\"Production\"}"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```