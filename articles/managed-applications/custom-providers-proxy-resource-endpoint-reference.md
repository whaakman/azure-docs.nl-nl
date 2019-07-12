---
title: Referentiemateriaal voor aangepaste resource-proxy
description: Aangepaste resource proxy-referentie voor aangepaste Resource Providers van Azure. In dit artikel gaat via de vereisten voor implementatie van de aangepaste resources proxy-eindpunten.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 06/20/2019
ms.openlocfilehash: fe470165d11b24ffb1df704ecaa9804663fc9e5c
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67796037"
---
# <a name="custom-resource-proxy-reference"></a>Proxy-referentie voor aangepaste Resource

In dit artikel gaat via de vereisten voor implementatie van de aangepaste resources proxy-eindpunten. Als u niet bekend met Azure aangepaste Resource Providers bent, Zie [het overzicht van aangepaste resourceproviders](./custom-providers-overview.md).

## <a name="how-to-define-a-proxy-resource-endpoint"></a>Over het definiëren van een proxy-resource-eindpunt

Een proxy-bron kan worden gemaakt door op te geven de **routingType** naar 'Proxy'.

Voorbeeld van aangepaste resourceprovider:

```JSON
{
  "properties": {
    "resourceTypes": [
      {
        "name": "myCustomResources",
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

## <a name="building-proxy-resource-endpoint"></a>Building proxy-resource-eindpunt

Een **eindpunt** die een 'Proxy'-resource implementeert **eindpunt** moet afhandelen van de aanvraag en respons voor de nieuwe API in Azure. In dit geval de **resourceType** genereert een nieuwe Azure-resource API voor `PUT`, `GET`, en `DELETE` CRUD uitvoeren op één resource, evenals `GET` om op te halen van alle bestaande resources.

> [!NOTE]
> De `id`, `name`, en `type` velden zijn niet vereist, maar zijn nodig voor het integreren van de aangepaste resource met bestaande Azure-ecosysteem.

Voorbeeld van bron:

``` JSON
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

Parameter-referentie:

Eigenschap | Voorbeeld | Description
---|---|---
name | '{myCustomResourceName}' | De naam van de aangepaste resource.
type | 'Microsoft.CustomProviders/resourceProviders/{resourceTypeName}' | De naamruimte van het type resource.
id | '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomResources/{myCustomResourceName}' | De resource-ID.

### <a name="create-a-custom-resource"></a>Een aangepaste resource maken

Azure API binnenkomende aanvraag:

``` HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resource-provider-name}/myCustomResources/{myCustomResourceName}?api-version=2018-09-01-preview
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

**Eindpunt** reactie:

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

- Ongeldig JSON-object-document. Alle matrices en tekenreeksen moeten worden genest onder een top-object.
- De `Content-Type` header moet worden ingesteld op 'application/json; charset = utf-8 ".

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

Deze aanvraag wordt vervolgens doorgestuurd naar de **eindpunt** in het formulier:

``` HTTP
GET https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}
```

Op dezelfde manier het antwoord van de **eindpunt** vervolgens terug naar de klant wordt doorgestuurd. Het antwoord van het eindpunt moet worden geretourneerd:

- Een geldig JSON-object-document. Alle matrices en tekenreeksen moeten worden genest onder een top-object.
- De `Content-Type` header moet worden ingesteld op 'application/json; charset = utf-8 ".

**Eindpunt** reactie:

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

Deze aanvraag wordt vervolgens doorgestuurd naar de **eindpunt** in het formulier:

``` HTTP
GET https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources
```

Op dezelfde manier het antwoord van de **eindpunt** vervolgens terug naar de klant wordt doorgestuurd. Het antwoord van het eindpunt moet worden geretourneerd:

- Een geldig JSON-object-document. Alle matrices en tekenreeksen moeten worden genest onder een top-object.
- De `Content-Type` header moet worden ingesteld op 'application/json; charset = utf-8 ".
- De lijst met resources moet worden geplaatst onder de hoogste `value` eigenschap.

**Eindpunt** reactie:

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
- [Snelstart: De aangepaste Resource Provider Azure maken en implementeren van aangepaste resources](./create-custom-provider.md)
- [Zelfstudie: Maken van aangepaste acties en resources in Azure](./tutorial-custom-providers-101.md)
- [Procedure: Aangepaste acties toe te voegen aan Azure REST-API](./custom-providers-action-endpoint-how-to.md)
- [Naslaginformatie over: Aangepaste Resource Cache verwijzing](./custom-providers-proxy-cache-resource-endpoint-reference.md)
