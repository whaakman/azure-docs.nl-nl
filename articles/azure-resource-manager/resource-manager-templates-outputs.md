---
title: Uitvoer van Azure Resource Manager-sjabloon | Microsoft Docs
description: Beschrijft hoe u voor het definiëren van uitvoer voor een Azure Resource Manager-sjablonen met behulp van declaratieve JSON-syntaxis.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/14/2019
ms.author: tomfitz
ms.openlocfilehash: 92e5dd5190a76bd09e33ea4c40a5b5cc2d66bc7b
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56301125"
---
# <a name="outputs-section-in-azure-resource-manager-templates"></a>De uitvoersectie in Azure Resource Manager-sjablonen

In de sectie uitvoer geeft u waarden die zijn geretourneerd na de implementatie. U kunt bijvoorbeeld de URI voor toegang tot een geïmplementeerde resource retourneren. Gebruik het optionele `condition` eigenschap om op te geven of de uitvoerwaarde is geretourneerd.

## <a name="define-and-use-output-values"></a>Definiëren en uitvoerwaarden

Het volgende voorbeeld laat zien hoe geretourneerd van de resource-ID voor een openbaar IP-adres:

```json
"outputs": {
  "resourceID": {
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

Het volgende voorbeeld laat zien hoe voorwaardelijk geretourneerd de resource-ID voor een openbaar IP-adres op basis van of een nieuw een is geïmplementeerd:

```json
"outputs": {
  "resourceID": {
    "condition": "[equals(parameters('publicIpNewOrExisting'), 'new')]",
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

Na de implementatie, kunt u de waarde met het script ophalen. Gebruik voor PowerShell:

```powershell
(Get-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -Name <deployment-name>).Outputs.resourceID.value
```

Gebruik voor Azure CLI:

```azurecli-interactive
az group deployment show -g <resource-group-name> -n <deployment-name> --query properties.outputs.resourceID.value
```

U kunt de uitvoerwaarde van een gekoppelde sjabloon ophalen met behulp van de [verwijzing](resource-group-template-functions-resource.md#reference) functie. Als u een uitvoerwaarde op basis van een gekoppelde sjabloon, halen de waarde van de eigenschap met de syntaxis, zoals: `"[reference('deploymentName').outputs.propertyName.value]"`.

Bij het ophalen van een eigenschap van een uitvoer van een gekoppelde sjabloon, kan niet de eigenschapsnaam van de een streepje bestaan.

U kunt bijvoorbeeld het IP-adres op een load balancer instellen door het ophalen van een waarde van een gekoppelde sjabloon.

```json
"publicIPAddress": {
    "id": "[reference('linkedTemplate').outputs.resourceID.value]"
}
```

U kunt geen gebruiken de `reference` functie in de uitvoersectie van een [geneste sjabloon](resource-group-linked-templates.md#link-or-nest-a-template). Als u wilt de waarden voor een geïmplementeerde resource in een geneste sjabloon, uw geneste sjabloon te converteren naar een gekoppelde sjabloon.

## <a name="available-properties"></a>Beschikbare eigenschappen

Het volgende voorbeeld ziet u de structuur van de uitvoerdefinitie van een:

```json
"outputs": {
    "<outputName>" : {
        "condition": "<boolean-value-whether-to-output-value>",
        "type" : "<type-of-output-value>",
        "value": "<output-value-expression>"
    }
}
```

| De naam van element | Vereist | Description |
|:--- |:--- |:--- |
| outputName |Ja |De naam van de uitvoerwaarde. Moet een geldige JavaScript-id. |
| voorwaarde |Nee | Booleaanse waarde die aangeeft of deze uitvoer waarde wordt geretourneerd. Wanneer `true`, de waarde is opgenomen in de uitvoer voor de implementatie. Wanneer `false`, de uitvoerwaarde wordt overgeslagen voor deze implementatie. Als niet is opgegeven, is de standaardwaarde `true`. |
| type |Ja |Type van de uitvoerwaarde. Uitvoerwaarden ondersteuning van de dezelfde typen als sjabloon invoerparameters die zijn opgegeven. |
| waarde |Ja |De sjabloontaalexpressie dat wordt geëvalueerd en geretourneerd als de uitvoerwaarde. |

Zie voor meer informatie over het toevoegen van opmerkingen [opmerkingen in sjablonen](resource-group-authoring-templates.md#comments).

## <a name="example-templates"></a>Voorbeeldsjablonen

|Template  |Description  |
|---------|---------|
|[Kopieer variabelen](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) | Complexe variabelen maakt en deze waarden weergeeft. Niet alle resources niet implementeren. |
|[Openbaar IP-adres](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) | Hiermee maakt u een openbaar IP-adres en de uitvoer van de resource-ID. |
|[Load balancer](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) | Koppelingen naar de voorgaande sjabloon. Maakt gebruik van de resource-ID in de uitvoer bij het maken van de load balancer. |


## <a name="next-steps"></a>Volgende stappen
* Zie de [Azure-snelstartsjablonen](https://azure.microsoft.com/documentation/templates/) voor volledige sjablonen voor verschillende soorten oplossingen.
* Zie voor meer informatie over de functies die u uit in een sjabloon gebruiken kunt [Azure Resource Manager-sjabloonfuncties](resource-group-template-functions.md).
* Zie voor aanbevelingen over het maken van sjablonen, [aanbevolen procedures voor Azure Resource Manager-sjabloon](template-best-practices.md).
