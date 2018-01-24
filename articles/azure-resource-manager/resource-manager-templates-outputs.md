---
title: Azure Resource Manager-sjabloon levert | Microsoft Docs
description: "Beschrijft hoe voor het definiëren van de uitvoer voor een Azure Resource Manager-sjablonen met behulp van declaratieve JSON-syntaxis."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/14/2017
ms.author: tomfitz
ms.openlocfilehash: 64d7a0ea72b2f629160f31e4bc1fb4a90f10653d
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="outputs-section-in-azure-resource-manager-templates"></a>Sectie van de uitvoer in Azure Resource Manager-sjablonen
In de sectie uitvoer geeft u de waarden die worden geretourneerd van de implementatie. U kan bijvoorbeeld de URI voor toegang tot een geïmplementeerde resource geretourneerd.

## <a name="define-and-use-output-values"></a>Definieer en het gebruik van de uitvoerwaarden

Het volgende voorbeeld ziet u hoe te retourneren van de resource-ID voor een openbaar IP-adres:

```json
"outputs": {
  "resourceID": {
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

Na de implementatie kunt u de waarde met een script ophalen. Gebruik voor PowerShell:

```powershell
(Get-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -Name <deployment-name>).Outputs.resourceID.value
```

Gebruik voor Azure CLI:

```azurecli-interactive
az group deployment show -g <resource-group-name> -n <deployment-name> --query properties.outputs.resourceID.value
```

U kunt de uitvoerwaarde van een gekoppelde sjabloon ophalen met behulp van de [verwijzing](resource-group-template-functions-resource.md#reference) functie. Als u een waarde voor de uitvoer van een gekoppelde sjabloon, halen de waarde van de eigenschap syntaxis: `"[reference('<name-of-deployment>').outputs.<property-name>.value]"`.

U kunt bijvoorbeeld het IP-adres instellen voor een load balancer door op te halen van een waarde van een gekoppelde sjabloon.

```json
"publicIPAddress": {
    "id": "[reference('linkedTemplate').outputs.resourceID.value]"
}
```

U kunt geen gebruiken de `reference` functie in de sectie uitvoer van een [geneste sjabloon](resource-group-linked-templates.md#link-or-nest-a-template). Als u wilt de waarden voor een geïmplementeerde resource in een geneste sjabloon, de sjabloon voor geneste niet converteren naar een gekoppelde sjabloon.

## <a name="available-properties"></a>Beschikbare eigenschappen

Het volgende voorbeeld ziet u de structuur van de definitie van een uitvoer:

```json
"outputs": {
    "<outputName>" : {
        "type" : "<type-of-output-value>",
        "value": "<output-value-expression>"
    }
}
```

| Elementnaam | Vereist | Beschrijving |
|:--- |:--- |:--- |
| outputName |Ja |De naam van de uitvoerwaarde. Moet een geldige JavaScript-id. |
| type |Ja |Type van de uitvoerwaarde. Uitvoerwaarden ondersteuning voor dezelfde typen als sjabloon invoerparameters. |
| waarde |Ja |De sjabloontaalexpressie dat wordt geëvalueerd en geretourneerd als de waarde voor uitvoer. |

## <a name="recommendations"></a>Aanbevelingen

Als u een sjabloon voor het maken van openbare IP-adressen gebruikt, moet u een uitvoer-sectie die gegevens van het IP-adres en de volledig gekwalificeerde domeinnaam (FQDN retourneert) opnemen. Uitvoerwaarden kunt u eenvoudig ophalen van informatie over de openbare IP-adressen en FQDN's na de implementatie.

```json
"outputs": {
    "fqdn": {
        "value": "[reference(parameters('publicIPAddresses_name')).dnsSettings.fqdn]",
        "type": "string"
    },
    "ipaddress": {
        "value": "[reference(parameters('publicIPAddresses_name')).ipAddress]",
        "type": "string"
    }
}
```

## <a name="example-templates"></a>Voorbeeld-sjablonen


|Template  |Beschrijving  |
|---------|---------|
|[Variabelen kopiëren](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) | Complexe variabelen maakt en deze waarden levert. Niet alle resources is geïmplementeerd. |
|[Openbaar IP-adres](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) | Hiermee maakt u een openbaar IP-adres en levert de resource-ID. |
|[Load balancer](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) | Koppelingen naar de voorgaande sjabloon. Maakt gebruik van de resource-ID in de uitvoer bij het maken van de load balancer. |


## <a name="next-steps"></a>Volgende stappen
* Zie de [Azure-snelstartsjablonen](https://azure.microsoft.com/documentation/templates/) voor volledige sjablonen voor verschillende soorten oplossingen.
* Zie voor meer informatie over de functies die u van binnen een sjabloon gebruiken kunt [Azure Resource Manager-sjabloonfuncties](resource-group-template-functions.md).
* Als u wilt combineren meerdere sjablonen tijdens de implementatie, Zie [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).
* Wellicht moet u bronnen gebruiken die zijn opgeslagen in een andere resourcegroep. Dit scenario is gebruikelijk dat wanneer u werkt met opslagaccounts of virtuele netwerken die worden gedeeld door meerdere resourcegroepen. Zie voor meer informatie de [resourceId functie](resource-group-template-functions-resource.md#resourceid).