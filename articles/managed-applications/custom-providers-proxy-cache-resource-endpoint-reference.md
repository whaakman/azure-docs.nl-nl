---
title: Referentiemateriaal voor aangepaste resource-cache
description: Aangepaste resource cache referentie voor Azure aangepaste Resource Providers. In dit artikel gaat via de vereisten voor implementatie van de aangepaste resources cache-eindpunten.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 06/20/2019
ms.openlocfilehash: 6a3d570d9695516a293b601b3d34c2bcba6b058d
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478886"
---
# <a name="custom-resource-cache-reference"></a>Aangepaste Resource Cache verwijzing

In dit artikel gaat via de vereisten voor implementatie van de aangepaste resources cache-eindpunten. Als u niet bekend met Azure aangepaste Resource Providers bent, Zie [het overzicht van aangepaste resourceproviders](./custom-providers-overview.md).

## <a name="how-to-define-a-cache-resource-endpoint"></a>Over het definiëren van het eindpunt van een cache-bron

Een proxy-bron kan worden gemaakt door op te geven de **routingType** naar 'Proxy, Cache'.

Voorbeeld van aangepaste resourceprovider:

```JSON
{
  "properties": {
    "resourceTypes": [
      {
        "name": "myCustomResources",
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

## <a name="building-proxy-resource-endpoint"></a>Building proxy-resource-eindpunt

Een **eindpunt** die een 'Proxy, Cache' resource implementeert **eindpunt** moet afhandelen van de aanvraag en respons voor de nieuwe API in Azure. In dit geval de **resourceType** genereert een nieuwe Azure-resource API voor `PUT`, `GET`, en `DELETE` CRUD uitvoeren op één resource, evenals `GET` om op te halen van alle bestaande resources:

> [!NOTE]
> De API van Azure worden de aanvraagmethoden genereren `PUT`, `GET`, en `DELETE`, maar de cache **eindpunt** hoeft u alleen voor het afhandelen van `PUT` en `DELETE`.
> Het wordt aangeraden dat de **eindpunt** ook implementeert `GET`.

### <a name="create-a-custom-resource"></a>Een aangepaste resource maken

Azure API binnenkomende aanvraag:

``` HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json

{
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

Deze aanvraag wordt vervolgens doorgestuurd naar de **eindpunt** in het formulier:

``` HTTP
PUT https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}

{
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

Op dezelfde manier het antwoord van de **eindpunt** vervolgens terug naar de klant wordt doorgestuurd. Het antwoord van het eindpunt moet worden geretourneerd:

- Een geldig JSON-object-document. Alle matrices en tekenreeksen moeten worden genest onder een top-object.
- De `Content-Type` header moet worden ingesteld op 'application/json; charset = utf-8 ".
- De aangepaste resourceprovider overschrijft de `name`, `type`, en `id` velden voor de aanvraag.
- De aangepaste resourceprovider retourneert alleen velden onder de `properties` object voor het eindpunt van een cache.

**Eindpunt** reactie:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

De `name`, `id`, en `type` velden automatisch worden gegenereerd voor de aangepaste resource door de aangepaste resourceprovider.

Aangepaste Azure-Resource-antwoord van Provider:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "name": "{myCustomResourceName}",
    "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}",
    "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

### <a name="remove-a-custom-resource"></a>Een aangepaste resource verwijderen

Azure API binnenkomende aanvraag:

``` HTTP
Delete https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json
```

Deze aanvraag wordt vervolgens doorgestuurd naar de **eindpunt** in het formulier:

``` HTTP
Delete https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}
```

Op dezelfde manier het antwoord van de **eindpunt** vervolgens terug naar de klant wordt doorgestuurd. Het antwoord van het eindpunt moet worden geretourneerd:

- Een geldig JSON-object-document. Alle matrices en tekenreeksen moeten worden genest onder een top-object.
- De `Content-Type` header moet worden ingesteld op 'application/json; charset = utf-8 ".
- De Resourceprovider van Azure aangepaste wordt alleen het item verwijderd uit de cache als een niveau 200-respons wordt geretourneerd. Zelfs als de bron niet bestaat, de **eindpunt** 204 moet retourneren.

**Eindpunt** reactie:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

Aangepaste Azure-Resource-antwoord van Provider:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

### <a name="retrieve-a-custom-resource"></a>Een aangepaste resource ophalen

Azure API binnenkomende aanvraag:

``` HTTP
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json
```

De aanvraag wordt **niet** worden doorgestuurd naar de **eindpunt**.

Aangepaste Azure-Resource-antwoord van Provider:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "name": "{myCustomResourceName}",
    "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}",
    "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

### <a name="enumerate-all-custom-resources"></a>Het inventariseren van alle aangepaste resources

Azure API binnenkomende aanvraag:

``` HTTP
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json
```

Deze aanvraag wordt **niet** worden doorgestuurd naar de **eindpunt**.

Aangepaste Azure-Resource-antwoord van Provider:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "value" : [
        {
            "name": "{myCustomResourceName}",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}",
            "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
            "properties": {
                "myProperty1": "myPropertyValue1",
                "myProperty2": {
                    "myProperty3" : "myPropertyValue3"
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van Azure aangepaste Resourceproviders](./custom-providers-overview.md)
- [Zelfstudie: De aangepaste Resource Provider Azure maken en implementeren van aangepaste resources](./create-custom-provider.md)
- [Procedure: Aangepaste acties toe te voegen aan Azure REST-API](./custom-providers-action-endpoint-how-to.md)
- [Naslaginformatie over: Proxy-referentie voor aangepaste Resource](./custom-providers-proxy-resource-endpoint-reference.md)
