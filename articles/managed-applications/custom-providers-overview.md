---
title: Overzicht van aangepaste Providers Azure Preview
description: Beschrijft de concepten voor het maken van een aangepaste resourceprovider met Azure Resource Manager
author: MSEvanhi
ms.service: managed-applications
ms.topic: conceptual
ms.date: 05/01/2019
ms.author: evanhi
ms.openlocfilehash: bbfb10f612690af0f4fd3683e0f58986a21048d8
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65159853"
---
# <a name="azure-custom-providers-preview-overview"></a>Overzicht van de Azure Preview-versie van aangepaste Providers

U kunt Azure om te werken met uw service uitbreiden met Azure aangepaste Providers. Maakt u uw eigen resourceprovider, met inbegrip van aangepaste resourcetypen en acties. De aangepaste provider is geïntegreerd met Azure Resource Manager. U kunt van Resource Manager-functies, zoals sjabloonimplementaties en op rollen gebaseerd toegangsbeheer gebruiken om te implementeren en beveiligen van uw service.

Dit artikel bevat een overzicht van aangepaste providers en de mogelijkheden ervan. De volgende afbeelding ziet u de werkstroom voor resources en acties die zijn gedefinieerd in een aangepaste provider.

![Overzicht van de aangepaste provider](./media/custom-providers-overview/overview.png)

> [!IMPORTANT]
> Aangepaste Providers is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="define-your-custom-provider"></a>Uw aangepaste provider definiëren

U start doordat uw aangepaste provider op de hoogte van Azure Resource Manager. U wilt implementeren in Azure een aangepaste provider-resource, die gebruikmaakt van het resourcetype van **Microsoft.CustomProviders/resourceProviders**. In de resource definieert u de resources en acties voor uw service.

Bijvoorbeeld, als uw service moet een resourcetype met de naam **gebruikers**, u dat resourcetype opnemen in de definitie van de aangepaste provider. Voor elk resourcetype geeft u een eindpunt met de REST-bewerkingen (PUT, ophalen, verwijderen) voor dat resourcetype. Het eindpunt kan worden gehost op elke omgeving en bevat de logica voor hoe uw service omgaat met bewerkingen op het resourcetype.

U kunt ook aangepaste acties voor de resourceprovider definiëren. Acties zijn POST-bewerkingen. Acties voor bewerkingen, zoals starten, stoppen of opnieuw gebruiken. Geeft u een eindpunt die verantwoordelijk is voor de aanvraag.

Het volgende voorbeeld ziet hoe u een aangepaste provider met een actie en een resourcetype definieert.

```json
{
  "apiVersion": "2018-09-01-preview",
  "type": "Microsoft.CustomProviders/resourceProviders",
  "name": "[parameters('funcName')]",
  "location": "[parameters('location')]",
  "properties": {
    "actions": [
      {
        "name": "ping",
        "routingType": "Proxy",
        "endpoint": "[concat('https://', parameters('funcName'), '.azurewebsites.net/api/{requestPath}')]"
      }
    ],
    "resourceTypes": [
      {
        "name": "users",
        "routingType": "Proxy,Cache",
        "endpoint": "[concat('https://', parameters('funcName'), '.azurewebsites.net/api/{requestPath}')]"
      }
    ]
  }
},
```

Voor **routingType**, de geaccepteerde waarden zijn `Proxy` en `Cache`. Proxy: aanvragen voor het resourcetype of actie worden verwerkt door het eindpunt. De cache-instelling wordt alleen ondersteund voor resourcetypen, niet-acties. Als u de cache, moet u ook proxyserver opgeven. Cache betekent antwoorden van het eindpunt voor het optimaliseren van leesbewerkingen worden opgeslagen. Met behulp van de cache-instelling, maakt het gemakkelijker voor het implementeren van een API die consistente en compatibel is met andere Resource Manager-services.

## <a name="deploy-your-resource-types"></a>De resourcetypen implementeren

Na het definiëren van uw aangepaste provider, kunt u uw aangepaste resourcetypen implementeren. Het volgende voorbeeld toont de JSON die u in uw sjabloon voor het implementeren van het resourcetype voor uw aangepaste provider opnemen. Dit resourcetype kan worden geïmplementeerd in dezelfde sjabloon met andere Azure-resources.

```json
{
    "apiVersion": "2018-09-01-preview",
    "type": "Microsoft.CustomProviders/resourceProviders/users",
    "name": "[concat(parameters('rpname'), '/santa')]",
    "location": "[parameters('location')]",
    "properties": {
        "FullName": "Santa Claus",
        "Location": "NorthPole"
    }
}
```

## <a name="manage-access"></a>Toegang beheren

Gebruik Azure [op rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md) voor het beheren van toegang tot uw bronprovider. U kunt toewijzen [ingebouwde rollen](../role-based-access-control/built-in-roles.md) zoals eigenaar, bijdrager of lezer voor gebruikers. Of u kunt definiëren [aangepaste rollen](../role-based-access-control/custom-roles.md) die specifiek zijn voor de bewerkingen in uw bronprovider.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over aangepaste providers. Ga naar het volgende artikel voor het maken van een aangepaste provider.

> [!div class="nextstepaction"]
> [Zelfstudie: Aangepaste provider maken en implementeren van aangepaste resources](create-custom-provider.md)
