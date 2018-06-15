---
title: Reageren op Azure Media Services-gebeurtenissen | Microsoft Docs
description: Gebruik Azure gebeurtenis raster om u te abonneren op Media Services-gebeurtenissen.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 969957d53824bd70440e5529b83bc830bb5d9cc4
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788149"
---
# <a name="reacting-to-media-services-events"></a>Reageren op Media Services-gebeurtenissen

Media Services-gebeurtenissen kunnen toepassingen om te reageren op verschillende gebeurtenissen (bijvoorbeeld: de taak statuswijzigingsgebeurtenis) met behulp van moderne zonder server architecturen. Zonder de noodzaak van complexe code of goedkoper en inefficiënt polling-services. In plaats daarvan gebeurtenissen worden gepusht via [Azure gebeurtenis raster](https://azure.microsoft.com/services/event-grid/) naar gebeurtenis-handlers zoals [Azure Functions](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), of deze zelfs uw eigen Webhook en u enige betalen voor wat u gebruikt. Zie voor meer informatie over prijzen [gebeurtenis raster prijzen](https://azure.microsoft.com/pricing/details/event-grid/).

Beschikbaarheid voor Media Services-gebeurtenissen is gekoppeld aan de gebeurtenis raster [beschikbaarheid](../../event-grid/overview.md) en zijn binnenkort beschikbaar in andere regio's zoals het raster gebeurtenis.  

## <a name="available-media-services-events"></a>Beschikbare Media Services-gebeurtenissen

Gebruikt voor gebeurtenis raster [gebeurtenisabonnementen](../../event-grid/concepts.md#event-subscriptions) gebeurtenis om berichten te routeren naar abonnees.  Abonnementen voor Media Services-gebeurtenissen kunnen op dit moment is het volgende gebeurtenistype bevatten:  

|De naam van gebeurtenis|Beschrijving|
|----------|-----------|
| Microsoft.Media.JobStateChange| Deze gebeurtenis treedt op wanneer de status van de wijzigingen van de taak. |

## <a name="event-schema"></a>Gebeurtenis-Schema

Media Services-gebeurtenissen bevatten alle informatie die u nodig hebt om te reageren op wijzigingen in uw gegevens.  Omdat de eigenschap eventType begint met 'Microsoft.Media.', kunt u een Media Services-gebeurtenis identificeren.

Zie voor meer informatie [Media Services-schema's voor gebeurtenis](media-services-event-schemas.md).

## <a name="practices-for-consuming-events"></a>Procedures voor het gebruiken van gebeurtenissen

Toepassingen die Media Services-gebeurtenissen verwerken moeten volgen enkele aanbevolen procedures:

* Als u meerdere abonnementen kunnen worden geconfigureerd op route-gebeurtenissen naar de dezelfde gebeurtenis-handler, is het belangrijk niet te mogen nemen gebeurtenissen zijn van een bepaalde bron, maar om te controleren van het onderwerp van het bericht om ervoor te zorgen dat ze afkomstig zijn van het opslagaccount dat u verwacht.
* Op dezelfde manier, Controleer of de eventType één u klaarstaan om te verwerken en niet vanuit gegaan voor dat alle gebeurtenissen die u ontvangt zal de typen die u verwacht.
* Velden die u niet begrijpt negeren.  Deze oefening kunt zorgen ervoor dat u robuuste op nieuwe functies die in de toekomst kunnen worden toegevoegd.
* De overeenkomsten van voorvoegsel en het achtervoegsel 'onderwerp' gebruiken om te beperken van gebeurtenissen naar een bepaalde gebeurtenis.

## <a name="next-steps"></a>Volgende stappen

[Taak statusgebeurtenissen ophalen](job-state-events-cli-how-to.md)
