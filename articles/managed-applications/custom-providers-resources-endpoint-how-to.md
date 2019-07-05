---
title: Aangepaste resources toe te voegen aan Azure REST-API
description: Leer hoe u aangepaste resources toevoegen aan de Azure REST API. In dit artikel helpt bij de vereisten en best practices voor eindpunten die voor het implementeren van aangepaste resources.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 06/20/2019
ms.openlocfilehash: b94d59b55a62797e142768dc84ec499d714bd067
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67479016"
---
# <a name="adding-custom-resources-to-azure-rest-api"></a>Aangepaste Resources toe te voegen aan Azure REST-API

In dit artikel gaat via de vereisten en best practices voor het maken van aangepaste Resourceprovider van Azure-eindpunten die aangepaste resources implementeert. Als u niet bekend met Azure aangepaste Resource Providers bent, Zie [het overzicht van aangepaste resourceproviders](./custom-providers-overview.md).

## <a name="how-to-define-a-resource-endpoint"></a>Over het definiëren van een resource-eindpunt

Een **eindpunt** een URL die verwijst naar een service, waarmee de onderliggende overeenkomst tussen deze en Azure wordt geïmplementeerd. Het eindpunt kan is gedefinieerd in de aangepaste resourceprovider en een openbaar toegankelijke URL zijn. Het onderstaande voorbeeld is een **resourceType** met de naam `myCustomResource` geïmplementeerd door `endpointURL`.

Voorbeeld **ResourceProvider**:

```JSON
{
  "properties": {
    "resourceTypes": [
      {
        "name": "myCustomResource",
        "routingType": "Proxy, Cache",
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

## <a name="building-a-resource-endpoint"></a>Het bouwen van een resource-eindpunt

Een **eindpunt** die implementeert een **resourceType** moet afhandelen van de aanvraag en respons voor de nieuwe API in Azure. Wanneer een aangepaste resourceprovider met een **resourceType** is gemaakt, wordt er een nieuwe set API's gegenereerd in Azure. In dit geval de **resourceType** genereert een nieuwe Azure-resource API voor `PUT`, `GET`, en `DELETE` CRUD uitvoeren op één resource, evenals `GET` om op te halen van alle bestaande resources:

Bewerken van één Resource (`PUT`, `GET`, en `DELETE`):

``` JSON
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResource/{myCustomResourceName}
```

Alle Resources ophalen (`GET`):

``` JSON
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResource
```

Voor aangepaste bronnen, aangepaste resourceproviders bieden twee typen **routingTypes**: "`Proxy`'en'`Proxy, Cache`'.

### <a name="proxy-routing-type"></a>proxy-routeringstype

De '`Proxy`" **routingType** proxy's alle methoden voor aanvraag de **eindpunt** opgegeven in de aangepaste resourceprovider. Wanneer u '`Proxy`':

- Volledige controle over het antwoord is vereist.
- Het integreren van systemen met bestaande resources.

Voor meer informatie over '`Proxy`"resources, Zie [de proxy-referentie voor aangepaste resource](./custom-providers-proxy-resource-endpoint-reference.md)

### <a name="proxy-cache-routing-type"></a>Proxy-cache-routeringstype

De '`Proxy, Cache`" **routingType** proxy's alleen `PUT` en `DELETE` methoden om te vragen de **eindpunt** opgegeven in de aangepaste resourceprovider. De aangepaste resourceprovider automatisch geretourneerd `GET` aanvragen is afhankelijk van wat wordt opgeslagen in de cache. Als een aangepaste resource is gemarkeerd met de cache, wordt de aangepaste resourceprovider ook toevoegen / overschrijven velden in het antwoord op de Azure-API's voldoen aan het beleid te maken. Wanneer u '`Proxy, Cache`':

- Het maken van een nieuw systeem die heeft geen bestaande resources.
- Werken met bestaande Azure-ecosysteem.

Voor meer informatie over '`Proxy, Cache`"resources, Zie [de aangepaste resource cache-verwijzing](./custom-providers-proxy-cache-resource-endpoint-reference.md)

## <a name="creating-a-custom-resource"></a>Het maken van een aangepaste resource

Er zijn twee manieren voor het maken van een aangepaste resource van een aangepaste resourceprovider:

- Azure-CLI
- Azure Resource Manager-sjablonen

### <a name="azure-cli"></a>Azure-CLI

Maak een aangepaste resource:

```azurecli-interactive
az resource create --is-full-object \
                   --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/{resourceTypeName}/{customResourceName} \
                   --properties \
                    '{
                        "location": "eastus",
                        "properties": {
                            "myProperty1": "myPropertyValue1",
                            "myProperty2": {
                                "myProperty3": "myPropertyValue3"
                            }
                        }
                    }'
```

Parameter | Vereist | Description
---|---|---
is-full-object | *yes* | Geeft aan dat het object voor eigenschappen een andere opties, zoals locatie, tags, sku en/of plan bevat.
id | *yes* | De resource-ID van de aangepaste resource. Dit moet bestaan uit van de **ResourceProvider**
properties | *yes* | De hoofdtekst van de aanvraag die wordt verzonden naar de **eindpunt**.

Een aangepaste Azure-Resource verwijderen:

```azurecli-interactive
az resource delete --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/{resourceTypeName}/{customResourceName}
```

Parameter | Vereist | Description
---|---|---
id | *yes* | De resource-ID van de aangepaste resource. Dit moet bestaan uit van de **ResourceProvider**.

Een aangepaste Azure-Resource ophalen:

```azurecli-interactive
az resource show --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/{resourceTypeName}/{customResourceName}
```

Parameter | Vereist | Description
---|---|---
id | *yes* | De resource-ID van de aangepaste resource. Dit moet bestaan uit van de **ResourceProvider**

### <a name="azure-resource-manager-template"></a>Azure Resource Manager-sjabloon

> [!NOTE]
> Resources worden vereist dat het antwoord bevat een geschikte `id`, `name`, en `type` uit de **eindpunt**.

Azure Resource Manager-sjablonen vereisen dat `id`, `name`, en `type` correct worden geretourneerd van de downstream-eindpunt. Een geretourneerde resource-antwoord moet zich in het formulier:

Voorbeeld **eindpunt** reactie:

``` JSON
{
  "properties": {
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3": "myPropertyValue3"
    }
  },
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{customResourceName}",
  "name": "{customResourceName}",
  "type": "Microsoft.CustomProviders/resourceProviders/{resourceTypeName}"
}
```

Voorbeeld Azure Resource Manager-sjabloon:

```JSON
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.CustomProviders/resourceProviders/{resourceTypeName}",
            "name": "{resourceProviderName}/{customResourceName}",
            "apiVersion": "2018-09-01-preview",
            "location": "eastus",
            "properties": {
                "myProperty1": "myPropertyValue1",
                "myProperty2": {
                    "myProperty3": "myPropertyValue3"
                }
            }
        }
    ]
}
```

Parameter | Vereist | Description
---|---|---
resourceTypeName | *yes* | De **naam** van de **resourceType** gedefinieerd in de aangepaste provider.
resourceProviderName | *yes* | De naam van de instantie in de aangepaste resource provider.
customResourceName | *yes* | De naam van de aangepaste resource.

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van Azure aangepaste Resourceproviders](./custom-providers-overview.md)
- [Zelfstudie: De aangepaste Resource Provider Azure maken en implementeren van aangepaste resources](./create-custom-provider.md)
- [Procedure: Aangepaste acties toe te voegen aan Azure REST-API](./custom-providers-action-endpoint-how-to.md)
- [Naslaginformatie over: Proxy-referentie voor aangepaste Resource](./custom-providers-proxy-resource-endpoint-reference.md)
- [Naslaginformatie over: Aangepaste Resource Cache verwijzing](./custom-providers-proxy-cache-resource-endpoint-reference.md)
