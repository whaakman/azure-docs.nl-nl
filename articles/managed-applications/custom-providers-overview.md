---
title: Overzicht van Azure aangepaste Resourceproviders
description: Meer informatie over Azure aangepaste Resourceproviders en voor het uitbreiden van het vlak van het Azure-API als u wilt aanpassen aan uw werkstromen.
author: jjbfour
ms.service: managed-applications
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: f418cd6c5470740ce123448ddbbe54cb6e89dabe
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67475943"
---
# <a name="azure-custom-resource-providers-overview"></a>Overzicht van aangepaste Azure-Resource-Providers

Azure aangepaste Resource Providers is een platform uitbreiden naar Azure. Hiermee kunt dat u definiëren naar aangepaste API's die kunnen worden gebruikt om het verrijken van de standaard Azure-ervaring. Deze documentatie wordt beschreven:

- Over het bouwen en implementeren van een Azure aangepaste Resource Provider.
- Klik hier voor meer informatie over het gebruik van Azure aangepaste Resource Providers om uit te breiden bestaande werkstromen.
- Waar vind ik handleidingen en codevoorbeelden aan de slag.

![Overzicht van de aangepaste provider](./media/custom-providers-overview/overview.png)

> [!IMPORTANT]
> Aangepaste Providers is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="what-can-custom-resource-providers-do"></a>Wat kunnen aangepaste resourceproviders doen

Hier volgen enkele voorbeelden van wat u met Azure aangepaste Resource Providers bereiken kunt:

- Azure Resource Manager REST API om op te nemen van de interne en externe services uitbreiden.
- Aangepaste scenario's boven op bestaande Azure werkstromen inschakelen.
- Beheer van Azure Resource Manager-sjablonen en effect aanpassen.

## <a name="what-is-a-custom-resource-provider"></a>Wat is een aangepaste resourceprovider

Azure aangepaste Resourceproviders worden gemaakt door het maken van een overeenkomst tussen Azure en een eindpunt. Deze overeenkomst definieert een lijst met nieuwe resources en acties via een nieuwe resource **Microsoft.CustomProviders/resourceProviders**. De aangepaste resourceprovider wordt vervolgens weergegeven voor deze nieuwe API's in Azure. Azure aangepaste Resourceproviders bestaan uit drie delen: aangepaste resourceprovider, **eindpunten**, en aangepaste resources.

## <a name="how-to-build-custom-resource-providers"></a>Over het bouwen van aangepaste resourceproviders

Aangepaste resourceproviders zijn een lijst met overeenkomsten tussen Azure en eindpunten. Deze overeenkomst wordt beschreven hoe Azure met een eindpunt moet werken. De resource provider fungeert als een proxy en stuurt aanvragen en antwoorden naar en van de opgegeven **eindpunt**. Een resourceprovider kunt twee soorten overeenkomsten opgeven: [ **brontypen** ](./custom-providers-resources-endpoint-how-to.md) en [ **acties**](./custom-providers-action-endpoint-how-to.md). Deze worden ingeschakeld via eindpuntdefinities. De eindpuntdefinitie van een bestaat uit drie velden: **naam**, **routingType**, en **eindpunt**.

Voorbeeld-eindpunt:

```JSON
{
  "name": "{endpointDefinitionName}",
  "routingType": "Proxy",
  "endpoint": "https://{endpointURL}/"
}
```

Eigenschap | Vereist | Description
---|---|---
name | *yes* | De naam van de eindpuntdefinitie. Azure wordt deze naam via de API onder weergegeven ' /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/<br>resourceProviders/{resourceProviderName}/{endpointDefinitionName}'
routingType | *no* | Bepaalt het type overeenkomst met de **eindpunt**. Indien niet opgegeven, wordt standaard 'Proxy'.
endpoint | *yes* | Het eindpunt voor het routeren van de aanvragen voor. Hiermee wordt het antwoord, evenals de nadelen van de aanvraag verwerkt.

### <a name="building-custom-resources"></a>Het bouwen van aangepaste resources

**Brontypen** nieuwe aangepaste resources die zijn toegevoegd aan Azure beschreven. Deze eenvoudige RESTful CRUD-methoden worden blootgesteld. Zie [meer informatie over het maken van aangepaste resources](./custom-providers-resources-endpoint-how-to.md)

Voorbeeld van een aangepaste Resource Provider met **brontypen**:

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
  "location": "eastus"
}
```

API's toegevoegd aan Azure voor het bovenstaande voorbeeld:

HttpMethod | Voorbeeld van URI | Description
---|---|---
PUT | /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomResources/{customResourceName}?api-version=2018-09-01-preview | De Azure REST API-aanroep om een nieuwe resource te maken.
DELETE | /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomResources/{customResourceName}?api-version=2018-09-01-preview | De Azure REST API-aanroep te verwijderen van een bestaande resource.
GET | /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomResources/{customResourceName}?api-version=2018-09-01-preview | De Azure REST API-aanroep om op te halen van een bestaande resource.
GET | /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomResources? api-version = 2018-09-01-preview | De Azure REST API-aanroep om op te halen van de lijst met bestaande resources.

### <a name="building-custom-actions"></a>Het bouwen van aangepaste acties

**Acties** nieuwe acties die zijn toegevoegd aan Azure beschreven. Deze kunnen worden weergegeven boven op de resourceprovider of genest onder een **resourceType**. Zie [meer informatie over het maken van aangepaste acties](./custom-providers-action-endpoint-how-to.md)

Voorbeeld van een aangepaste Resource Provider met **acties**:

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
  "location": "eastus"
}
```

API's toegevoegd aan Azure voor het bovenstaande voorbeeld:

HttpMethod | Voorbeeld van URI | Description
---|---|---
POST | /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomAction?api-version=2018-09-01-preview | De Azure REST API-aanroep voor het activeren van de actie.

## <a name="looking-for-help"></a>Hulp nodig hebt

Hebt u vragen over Azure-Resourceprovider voor aangepaste ontwikkeling, misschien op [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-custom-providers). Een vergelijkbaar vraag kan al zijn gevraagd en hebt beantwoord, dus probeer eerst voordat plaatsen. Voeg het label ```azure-custom-providers``` om op te halen van een snelle respons!

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over aangepaste providers. Ga naar het volgende artikel voor het maken van een aangepaste provider.

- [Zelfstudie: De aangepaste Resource Provider Azure maken en implementeren van aangepaste resources](./create-custom-provider.md)
- [Procedure: Aangepaste acties toe te voegen aan Azure REST-API](./custom-providers-action-endpoint-how-to.md)
- [Procedure: Aangepaste Resources toe te voegen aan Azure REST-API](./custom-providers-resources-endpoint-how-to.md)
