---
title: Azure digitale dubbels openbare preview-Servicelimieten | Microsoft Docs
description: Krijg inzicht in dat Azure digitale dubbels openbare preview-Servicelimieten.
author: dwalthermsft
manager: deshner
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/03/2019
ms.author: dwalthermsft
ms.openlocfilehash: 7d9686b9bcc6cb89fabf4fdaa79bf5b8c6c45ddc
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54020622"
---
# <a name="public-preview-service-limits"></a>Servicebeperkingen van de openbare preview

Tijdens de openbare preview heeft digitale dubbels Azure de volgende tijdelijke abonnement, exemplaar en frequentielimieten toe.

Deze beperkingen bestaan om te leren over de nieuwe service en de vele functies vereenvoudigen.

> [!NOTE]
> Deze limieten worden verhoogd of verwijderd door de algemene beschikbaarheid (GA).

## <a name="per-subscription-limits"></a>Limieten voor een per abonnement

Elk Azure-abonnement kunt maken of slechts één exemplaar van Azure digitale dubbels tegelijk uitgevoerd tijdens de openbare preview.

> [!TIP]
> Als u uw exemplaar hebt verwijderd, kunt u een nieuwe maken.

## <a name="per-instance-limits"></a>Limieten per exemplaar

Op zijn beurt kan elk exemplaar van Azure digitale dubbels hebben:

- Precies één **IoTHub** resource.
- Precies één **EventHub** -eindpunt voor het gebeurtenistype **DeviceMessage**.
- Maximaal drie **EventHub**, **ServiceBus**, of **EventGrid** eindpunten van het gebeurtenistype **SensorChange**, **SpaceChange** , **TopologyOperation**, of **UdfCustom**.

> [!NOTE]
> Sommige parameters die gewoonlijk zijn gedefinieerd in het maken van de bovenstaande Azure IoT-entiteiten zijn niet vereist tijdens de openbare preview.
> - Raadpleeg de [Swagger-referentiedocumentatie](./how-to-use-swagger.md) voor de meest recente API-specificaties.

## <a name="azure-digital-twins-management-api-limits"></a>Azure digitale dubbels Management API-limieten

De aanvraag frequentielimieten toe voor uw Azure API voor beheer van digitale dubbels zijn:

- 100 aanvragen per seconde naar de Azure API voor beheer van digitale dubbels.
- Maximaal 1000 objecten geretourneerd door een enkele Azure-API voor het beheer van digitale Twins-query.

> [!IMPORTANT]
> Als u de limiet van 1000-object overschrijdt, kunt u een foutbericht krijgt en moet Vereenvoudig de query.

## <a name="user-defined-functions-rate-limits"></a>De gebruiker gedefinieerde functies frequentielimieten

De volgende limieten instellen voor het totale aantal alle de gebruiker gedefinieerde functieaanroepen naar uw Azure digitale Twins-exemplaar:

- 400 client library-aanroepen per seconde
- 100 **SendNotification** aanroepen per seconde

> [!NOTE]
> De volgende acties kunnen leiden tot extra frequentielimieten tijdelijk worden toegepast:
> - Wijzigingen in de metagegevens van een object topologie
> - Wijzigingen in de definitie van de gebruiker gedefinieerde functie
> - Apparaten die telemetrie voor de eerste keer verzenden

## <a name="device-telemetry-limits"></a>Apparaatlimieten telemetrie

De volgende limieten gegevenslimiet het totale aantal alle berichten die uw apparaten naar uw Azure digitale Twins-exemplaar verzenden kunnen:

- 100 berichten per seconde

## <a name="next-steps"></a>Volgende stappen

Als u wilt uitproberen een voorbeeld van Azure digitale Twins, gaat u naar [Snelstartgids voor het vinden van beschikbare ruimten](./quickstart-view-occupancy-dotnet.md).