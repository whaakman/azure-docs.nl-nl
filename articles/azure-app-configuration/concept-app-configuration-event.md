---
title: Kruisreagerende naar Azure App-sleutel / waarde-Configuratiegebeurtenissen | Microsoft Docs
description: Gebruik Azure Event Grid om u te abonneren op gebeurtenissen van App-configuratie.
services: azure-app-configuration,event-grid
author: jimmyca
ms.author: jimmyca
ms.date: 05/30/2019
ms.topic: article
ms.service: azure-app-configuration
ms.openlocfilehash: 601124aef37d2b285db71130f5c63b3620c7768f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66735644"
---
# <a name="reacting-to-azure-app-configuration-events"></a>Reageren op gebeurtenissen van Azure App-configuratie

Azure App-configuratie-gebeurtenissen kunnen toepassingen om te reageren op wijzigingen in waarden van de sleutel. Dit gebeurt zonder de noodzaak voor complexe code of kostbaar en inefficiënt polling-services. In plaats daarvan de gebeurtenissen worden gepusht via [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) voor abonnees zoals [Azure Functions](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), of zelfs naar uw eigen aangepaste http-listener en u alleen Betaal voor wat u gebruikt.

Azure App-configuratie-gebeurtenissen worden verzonden naar de Azure Event Grid waarmee u betrouwbare van leveringsservices aan uw toepassingen door middel van uitgebreide opnieuw beleid en de dead-letter uitvoeren voor levering. Zie voor meer informatie, [bezorging van berichten van Event Grid en probeer het opnieuw](https://docs.microsoft.com/azure/event-grid/delivery-and-retry).

Algemene app-configuratie event-scenario's omvatten vernieuwen van de configuratie van de toepassing, implementaties, of een configuratie-georiënteerde werkstroom activeren. Wanneer wijzigingen incidentele zijn, maar uw scenario direct reactietijd is vereist, kan architectuur op basis van gebeurtenissen met name efficiënt zijn.

Kijk eens [Route Azure App-configuratie-gebeurtenissen naar een aangepaste web-eindpunt - CLI](./howto-app-configuration-event.md) voor een kort voorbeeld. 

![Event Grid-Model](./media/event-grid-functional-model.png)

## <a name="available-azure-app-configuration-events"></a>Beschikbare Azure-App-Configuratiegebeurtenissen
Maakt gebruik van Event grid [gebeurtenisabonnementen](../event-grid/concepts.md#event-subscriptions) gebeurtenis om berichten te routeren voor abonnees. Azure App-configuratie gebeurtenisabonnementen kunnen twee soorten gebeurtenissen opnemen:  

> |De naam van gebeurtenis|Description|
> |----------|-----------|
> |`Microsoft.AppConfiguration.KeyValueModified`|Geactiveerd wanneer een sleutel / waarde wordt gemaakt of vervangen|
> |`Microsoft.AppConfiguration.KeyValueDeleted`|Geactiveerd wanneer een sleutel / waarde-wordt verwijderd|

## <a name="event-schema"></a>Gebeurtenisschema
Gebeurtenissen van Azure App-configuratie bevatten alle informatie die u nodig hebt om te reageren op wijzigingen in uw gegevens. U kunt een gebeurtenis voor app-configuratie kunt identificeren, omdat de eigenschap type gebeurtenis met "Microsoft.AppConfiguration begint". Meer informatie over het gebruik van de eigenschappen van Event Grid-gebeurtenis wordt gedocumenteerd in [Event Grid-gebeurtenisschema](../event-grid/event-schema.md).  

> |Eigenschap|Type|Description|
> |-------------------|------------------------|-----------------------------------------------------------------------|
> |topic|string|Volledige Azure Resource Manager-id van de appconfiguratie van de die u de gebeurtenis verzendt.|
> |subject|string|De URI van de sleutel-waarde die het onderwerp van de gebeurtenis.|
> |eventTime|string|De datum/tijd die de gebeurtenis is gegenereerd, in ISO 8601-notatie.|
> |eventType|string|"Microsoft.AppConfiguration.KeyValueModified" of 'Microsoft.AppConfiguration.KeyValueDeleted'.|
> |Id|string|Een unieke id van deze gebeurtenis.|
> |dataVersion|string|De schemaversie van het gegevensobject.|
> |metadataVersion|string|De schemaversie van de eigenschappen van het hoogste niveau.|
> |data|object|Verzamelen van gegevens van Azure App-configuratie-specifieke gebeurtenis|
> |data.key|string|De sleutel van de sleutel-waarde die is gewijzigd of verwijderd.|
> |data.label|string|Het label, indien van toepassing, van de sleutel-waarde die is gewijzigd of verwijderd.|
> |data.etag|string|Voor `KeyValueModified` de etag van de nieuwe sleutel-waarde. Voor `KeyValueDeleted` de etag van de sleutel-waarde die is verwijderd.|

Hier volgt een voorbeeld van een gebeurtenis KeyValueModified:
```json
[{
  "id": "84e17ea4-66db-4b54-8050-df8f7763f87b",
  "topic": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/microsoft.appconfiguration/configurationstores/contoso",
  "subject": "https://contoso.azconfig.io/kv/Foo?label=FizzBuzz",
  "data": {
    "key": "Foo",
    "label": "FizzBuzz",
    "etag": "FnUExLaj2moIi4tJX9AXn9sakm0"
  },
  "eventType": "Microsoft.AppConfiguration.KeyValueModified",
  "eventTime": "2019-05-31T20:05:03Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]

```

Zie voor meer informatie, [Azure App-configuratie het gebeurtenissenschema](../event-grid/event-schema-app-configuration.md).

## <a name="practices-for-consuming-events"></a>Procedures voor het gebruik van gebeurtenissen
Toepassingen die werken met app-Configuratiegebeurtenissen moeten volgen enkele aanbevolen procedures:
> [!div class="checklist"]
> * Als u meerdere abonnementen kunnen worden geconfigureerd om gebeurtenissen routeren naar de dezelfde gebeurtenis-handler, is het belangrijk niet wordt ervan uitgegaan dat gebeurtenissen worden van een specifieke bron, maar om te controleren of het onderwerp van het bericht om ervoor te zorgen dat het afkomstig is van de appconfiguratie van de die u verwacht.
> * Op dezelfde manier, Controleer of het type gebeurtenis een u bent voorbereid om te verwerken en kan niet vanuit gegaan is dat alle gebeurtenissen die u ontvangt zal de typen die u verwacht.
> * Als berichten kunnen niet de juiste volgorde binnenkomen, en na enige tijd duren, gebruikt u de etag-velden om te begrijpen of uw gegevens over objecten nog steeds up-to-date is.  De velden van de sequencer ook gebruiken om te begrijpen van de volgorde van gebeurtenissen op een bepaald object.
> * Het onderwerpveld gebruiken voor toegang tot de sleutel-waarde die is gewijzigd.


## <a name="next-steps"></a>Volgende stappen

Meer informatie over Event Grid en Azure App-Configuratiegebeurtenissen een probeer:

- [Over Event Grid](../event-grid/overview.md)
- [Azure App-configuratie gebeurtenissen routeren naar een aangepaste web-eindpunt](./howto-app-configuration-event.md)