---
title: Maken en te gebruiken een aangepaste Azure Provider
description: In deze zelfstudie gaat over het maken en te gebruiken een aangepaste provider.
author: jjbfour
ms.service: managed-applications
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: 65a8e60d8216e1da16af987c9e699e24ecaec3ec
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799999"
---
# <a name="authoring-a-restful-endpoint-for-custom-providers"></a>Het ontwerpen van een RESTful-eindpunt voor aangepaste providers

Aangepaste providers kunnen u werkstromen in Azure aanpassen. Een aangepaste provider is een overeenkomst tussen Azure en een `endpoint`. In deze zelfstudie doorloopt het proces voor het maken van een aangepaste provider. Als u niet bekend met aangepaste Providers van Azure bent, Zie [het overzicht van aangepaste resourceproviders](./custom-providers-overview.md).

In deze zelfstudie is onderverdeeld in de volgende stappen uit:

- Wat is een aangepaste provider
- Aangepaste acties en resources definiëren
- Implementatie van de aangepaste provider

In deze zelfstudie bouwt op de volgende zelfstudies:

- [Het ontwerpen van een RESTful-eindpunt voor aangepaste providers](./tutorial-custom-providers-function-authoring.md)

## <a name="creating-a-custom-provider"></a>Het maken van een aangepaste provider

> [!NOTE]
> In deze zelfstudie gaat niet over het ontwerpen van een eindpunt. Volg de [zelfstudie over het ontwerpen van RESTful-eindpunten](./tutorial-custom-providers-function-authoring.md) als u geen een RESTful-eindpunt.

Zodra de `endpoint` is gemaakt, kunt u het maken van een aangepaste provider voor het genereren van een overeenkomst tussen het genereren en de `endpoint`. Een aangepaste provider kunt u een lijst opgeven van eindpuntdefinities.

```JSON
{
  "name": "myEndpointDefinition",
  "routingType": "Proxy",
  "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
}
```

Eigenschap | Verplicht | Description
---|---|---
name | *yes* | De naam van de eindpuntdefinitie. Azure wordt deze naam via de API onder weergegeven ' /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/<br>resourceProviders/{resourceProviderName}/{endpointDefinitionName}'
routingType | *no* | Bepaalt het type overeenkomst met de `endpoint`. Indien niet opgegeven, wordt standaard 'Proxy'.
endpoint | *yes* | Het eindpunt voor het routeren van de aanvragen voor. Hiermee wordt het antwoord, evenals de nadelen van de aanvraag verwerkt.

In dit geval de `endpoint` is de trigger-URL van de Azure-functie. De `<yourapp>`, `<funcname>`, en `<functionkey>` moet worden vervangen door waarden voor de functie hebt gemaakt.

## <a name="defining-custom-actions-and-resources"></a>Aangepaste acties en resources definiëren

De aangepaste provider bevat een overzicht van eindpuntdefinities gemodelleerd onder `actions` en `resourceTypes`. `actions` Wijs deze toe aan de aangepaste acties door de aangepaste provider beschikbaar gesteld, terwijl `resourceTypes` zijn de aangepaste resources. Voor deze zelfstudie, definiëren we een aangepaste provider met een `action` met de naam `myCustomAction` en een `resourceType` met de naam `myCustomResources`.

```JSON
{
  "properties": {
    "actions": [
      {
        "name": "myCustomAction",
        "routingType": "Proxy",
        "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
      }
    ],
    "resourceTypes": [
      {
        "name": "myCustomResources",
        "routingType": "Proxy",
        "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
      }
    ]
  },
  "location": "eastus"
}
```

Vervang `endpoint` met de trigger-URL van de functie die eerder in de vorige zelfstudie hebt gemaakt.

## <a name="deploying-the-custom-provider"></a>Implementatie van de aangepaste provider

> [!NOTE]
> De `endpoint` moet worden vervangen door de functie-URL.

De bovenstaande aangepaste provider kan worden geïmplementeerd met behulp van een Azure Resource Manager-sjabloon.

```JSON
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.CustomProviders/resourceProviders",
            "name": "myCustomProvider",
            "apiVersion": "2018-09-01-preview",
            "location": "eastus",
            "properties": {
                "actions": [
                    {
                        "name": "myCustomAction",
                        "routingType": "Proxy",
                        "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
                    }
                ],
                "resourceTypes": [
                    {
                        "name": "myCustomResources",
                        "routingType": "Proxy",
                        "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
                    }
                ]
            }
        }
    ]
}
```

## <a name="using-custom-actions-and-resources"></a>Met behulp van aangepaste acties en resources

Nadat we een aangepaste provider hebt gemaakt, kunnen we gebruikmaken van de nieuwe Azure-API's. De volgende sectie wordt uitgelegd hoe aanroepen en gebruikmaken van een aangepaste provider.

### <a name="custom-actions"></a>Aangepaste acties

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

> [!NOTE]
> De `{subscriptionId}` en `{resourceGroupName}` moet worden vervangen door het abonnement en de resourcegroep waarin de aangepaste provider is geïmplementeerd.

```azurecli-interactive
az resource invoke-action --action myCustomAction \
                          --ids /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider \
                          --request-body
                            '{
                                "hello": "world"
                            }'
```

Parameter | Verplicht | Description
---|---|---
Actie | *yes* | De naam van de actie die is gedefinieerd in de gemaakte aangepaste provider.
ids | *yes* | De resource-ID van de gemaakte aangepaste provider.
hoofdtekst van de aanvraag | *no* | De hoofdtekst van de aanvraag die wordt verzonden naar de `endpoint`.

# <a name="templatetabtemplate"></a>[Sjabloon](#tab/template)

Geen.

---

### <a name="custom-resources"></a>Aangepaste resources

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

> [!NOTE]
> De `{subscriptionId}` en `{resourceGroupName}` moet worden vervangen door het abonnement en de resourcegroep waarin de aangepaste provider is geïmplementeerd.

Maak een aangepaste resource:

```azurecli-interactive
az resource create --is-full-object \
                   --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider/myCustomResources/myTestResourceName1 \
                   --properties
                    '{
                        "location": "eastus",
                        "properties": {
                            "hello" : "world"
                        }
                    }'
```

Parameter | Verplicht | Description
---|---|---
is-full-object | *yes* | Geeft aan dat het object voor eigenschappen een andere opties, zoals locatie, tags, sku en/of plan bevat.
id | *yes* | De resource-ID van de aangepaste resource. Dit moet bestaan uit de gemaakte aangepaste provider.
properties | *yes* | De hoofdtekst van de aanvraag die wordt verzonden naar de `endpoint`.

Een aangepaste Azure-Resource verwijderen:

```azurecli-interactive
az resource delete --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider/myCustomResources/myTestResourceName1
```

Parameter | Verplicht | Description
---|---|---
id | *yes* | De resource-ID van de aangepaste resource. Dit moet bestaan uit de gemaakte aangepaste provider.

Een aangepaste Azure-Resource ophalen:

```azurecli-interactive
az resource show --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider/myCustomResources/myTestResourceName1
```

Parameter | Verplicht | Description
---|---|---
id | *yes* | De resource-ID van de aangepaste resource. Dit moet bestaan uit de gemaakte aangepaste provider.

# <a name="templatetabtemplate"></a>[Sjabloon](#tab/template)

Voorbeeld Azure Resource Manager-sjabloon:

```JSON
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
            "name": "myCustomProvider/myTestResourceName1",
            "apiVersion": "2018-09-01-preview",
            "location": "eastus",
            "properties": {
                "hello": "world"
            }
        }
    ]
}
```

Parameter | Verplicht | Description
---|---|---
resourceTypeName | *yes* | De `name` van de *resourceType* gedefinieerd in de aangepaste provider.
resourceProviderName | *yes* | De naam van de aangepaste provider-exemplaar.
customResourceName | *yes* | De naam van de aangepaste resource.

---

> [!NOTE]
> Wanneer u klaar bent met het implementeren en gebruiken van de aangepaste provider, vergeet niet om alle gemaakte resources, met inbegrip van de Azure Function op te schonen.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over aangepaste providers. Ga naar het volgende artikel voor het maken van een aangepaste provider.

- [Procedure: Aangepaste acties toe te voegen aan Azure REST-API](./custom-providers-action-endpoint-how-to.md)
- [Procedure: Aangepaste Resources toe te voegen aan Azure REST-API](./custom-providers-resources-endpoint-how-to.md)
