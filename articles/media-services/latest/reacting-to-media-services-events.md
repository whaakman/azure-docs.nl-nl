---
title: Reageren op gebeurtenissen voor Azure Media Services | Microsoft Docs
description: Gebruik Azure Event Grid om u te abonneren op gebeurtenissen voor Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/12/2019
ms.author: juliako
ms.openlocfilehash: cb5d6474a0c830933c712e1008015b5220617c96
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57850901"
---
# <a name="handling-event-grid-events"></a>Event Grid-gebeurtenissen verwerken

Media Services-gebeurtenissen kunnen toepassingen om te reageren op verschillende gebeurtenissen (bijvoorbeeld de taak statuswijzigingsgebeurtenis) met behulp van moderne architecturen zonder servers. Dit gebeurt zonder de noodzaak voor complexe code of kostbaar en inefficiënt polling-services. In plaats daarvan de gebeurtenissen worden gepusht via [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) naar gebeurtenis-handlers zoals [Azure Functions](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), of zelfs naar uw eigen Webhook en u betaalt voor wat u gebruikt. Zie voor meer informatie over de prijzen [prijzen van Event Grid](https://azure.microsoft.com/pricing/details/event-grid/).

De beschikbaarheid van Media Services-gebeurtenissen is gekoppeld aan Event Grid [beschikbaarheid](../../event-grid/overview.md) en worden pas beschikbaar in andere regio's als Event Grid.  

## <a name="media-services-events-and-schemas"></a>Media Services-gebeurtenissen en schema 's

Maakt gebruik van Event grid [gebeurtenisabonnementen](../../event-grid/concepts.md#event-subscriptions) gebeurtenis om berichten te routeren voor abonnees. Media Services-gebeurtenissen bevatten alle informatie die u nodig hebt om te reageren op wijzigingen in uw gegevens. Omdat de eigenschap type gebeurtenis begint met "Microsoft.Media.", kunt u een Media Services-gebeurtenis identificeren.

Zie voor meer informatie, [Media Services-gebeurtenisschema](media-services-event-schemas.md).

## <a name="practices-for-consuming-events"></a>Procedures voor het gebruik van gebeurtenissen

Toepassingen die Media Services-gebeurtenissen verwerken moeten volgen enkele aanbevolen procedures:

* Als u meerdere abonnementen kunnen worden geconfigureerd om gebeurtenissen routeren naar de dezelfde gebeurtenis-handler, is het belangrijk niet wordt ervan uitgegaan dat gebeurtenissen worden van een specifieke bron, maar om te controleren of het onderwerp van het bericht om ervoor te zorgen dat het afkomstig is van het opslagaccount dat u verwacht.
* Op dezelfde manier, Controleer of het type gebeurtenis een u bent voorbereid om te verwerken en kan niet vanuit gegaan is dat alle gebeurtenissen die u ontvangt zal de typen die u verwacht.
* Velden die u niet machtig bent negeren.  Met deze procedure krijgt u flexibele houden bij de nieuwe functies die in de toekomst kunnen worden toegevoegd.
* De overeenkomsten van voorvoegsel en het achtervoegsel 'onderwerp' gebruiken om te beperken van gebeurtenissen naar een bepaalde gebeurtenis.

## <a name="next-steps"></a>Volgende stappen

[Taak statusgebeurtenissen ophalen](job-state-events-cli-how-to.md)
