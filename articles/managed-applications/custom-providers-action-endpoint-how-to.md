---
title: Aangepaste acties toe te voegen aan Azure REST-API
description: Informatie over het toevoegen van aangepaste acties aan de Azure REST API. In dit artikel helpt bij de vereisten en best practices voor eindpunten die voor het implementeren van aangepaste acties.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 06/20/2019
ms.openlocfilehash: 1bfc0be81d42e922c47755543fb65aa413ec73a9
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478756"
---
# <a name="adding-custom-actions-to-azure-rest-api"></a>Aangepaste acties toe te voegen aan Azure REST-API

In dit artikel gaat via de vereisten en best practices voor het maken van aangepaste Resourceprovider van Azure-eindpunten die aangepaste acties implementeren. Als u niet bekend met Azure aangepaste Resource Providers bent, Zie [het overzicht van aangepaste resourceproviders](./custom-providers-overview.md).

## <a name="how-to-define-an-action-endpoint"></a>Over het definiëren van een actie-eindpunt

Een **eindpunt** een URL die verwijst naar een service, waarmee de onderliggende overeenkomst tussen deze en Azure wordt geïmplementeerd. Het eindpunt kan is gedefinieerd in de aangepaste resourceprovider en een openbaar toegankelijke URL zijn. Het onderstaande voorbeeld is een **actie** met de naam `myCustomAction` geïmplementeerd door `endpointURL`.

Voorbeeld **ResourceProvider**:

```JSON
{
  "properties": {
    "actions": [
      {
        "name": "myCustomAction",
        "routingType": "Proxy",
        "endpoint": "https://{endpointURL}/"
      }
    ]
  },
  "location": "eastus",
  "type": "Microsoft.CustomProviders/resourceProviders",
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}",
  "name": "{resourceProviderName}"
}
```

## <a name="building-an-action-endpoint"></a>Het bouwen van een actie-eindpunt

Een **eindpunt** die implementeert een **actie** moet afhandelen van de aanvraag en respons voor de nieuwe API in Azure. Wanneer een aangepaste resourceprovider met een **actie** is gemaakt, wordt er een nieuwe set API's gegenereerd in Azure. In dit geval wordt de actie genereert een nieuwe Azure-actie API voor `POST` aanroepen:

``` JSON
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomAction
```

Azure API binnenkomende aanvraag:

``` HTTP
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomAction?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json

{
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
}
```

Deze aanvraag wordt vervolgens doorgestuurd naar de **eindpunt** in het formulier:

``` HTTP
POST https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomAction

{
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
}
```

Op dezelfde manier het antwoord van de **eindpunt** vervolgens terug naar de klant wordt doorgestuurd. Het antwoord van het eindpunt moet worden geretourneerd:

- Een geldig JSON-object-document. Alle matrices en tekenreeksen moeten worden genest onder een top-object.
- De `Content-Type` header moet worden ingesteld op 'application/json; charset = utf-8 ".

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
}
```

Aangepaste Azure-Resource-antwoord van Provider:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
}
```

## <a name="calling-a-custom-action"></a>Aanroepen van een aangepaste actie

Er zijn twee manieren van het aanroepen van een aangepaste actie van een aangepaste resourceprovider:

- Azure-CLI
- Azure Resource Manager-sjablonen

### <a name="azure-cli"></a>Azure-CLI

```azurecli-interactive
az resource invoke-action --action {actionName} \
                          --ids /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName} \
                          --request-body \
                            '{
                                "myProperty1": "myPropertyValue1",
                                "myProperty2": {
                                    "myProperty3": "myPropertyValue3"
                                }
                            }'
```

Parameter | Vereist | Description
---|---|---
action | *yes* | De naam van de actie die is gedefinieerd in de **ResourceProvider**.
ids | *yes* | De resource-ID van de **ResourceProvider**.
hoofdtekst van de aanvraag | *no* | De hoofdtekst van de aanvraag die wordt verzonden naar de **eindpunt**.

### <a name="azure-resource-manager-template"></a>Azure Resource Manager-sjabloon

> [!NOTE]
> Acties in beperkte mate ondersteunt in Azure Resource Manager-sjablonen. In de volgorde voor de actie die moet worden aangeroepen vanuit een sjabloon, moet deze bevatten de [ `list` ](../azure-resource-manager/resource-group-template-functions-resource.md#list) voorvoegsel in de naam ervan.

Voorbeeld **ResourceProvider** met lijstactie:

```JSON
{
  "properties": {
    "actions": [
      {
        "name": "listMyCustomAction",
        "routingType": "Proxy",
        "endpoint": "https://{endpointURL}/"
      }
    ]
  },
  "location": "eastus"
}
```

Voorbeeld Azure Resource Manager-sjabloon:

``` JSON
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "resourceIdentifier": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}",
        "apiVersion": "2018-09-01-preview",
        "functionValues": {
            "myProperty1": "myPropertyValue1",
            "myProperty2": {
                "myProperty3": "myPropertyValue3"
            }
        }
    },
    "resources": [],
    "outputs": {
        "myCustomActionOutput": {
            "type": "object",
            "value": "[listMyCustomAction(variables('resourceIdentifier'), variables('apiVersion'), variables('functionValues'))]"
        }
    }
}
```

Parameter | Vereist | Description
---|---|---
resourceIdentifier | *yes* | De resource-ID van de **ResourceProvider**.
apiVersion | *yes* | De API-versie van de resource-runtime. Dit moet altijd '2018-09-01-preview'.
functionValues | *no* | De hoofdtekst van de aanvraag die wordt verzonden naar de **eindpunt**.

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van Azure aangepaste Resourceproviders](./custom-providers-overview.md)
- [Zelfstudie: De aangepaste Resource Provider Azure maken en implementeren van aangepaste resources](./create-custom-provider.md)
- [Procedure: Aangepaste Resources toe te voegen aan Azure REST-API](./custom-providers-resources-endpoint-how-to.md)
