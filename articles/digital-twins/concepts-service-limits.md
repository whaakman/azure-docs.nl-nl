---
title: Azure digitale dubbels openbare preview-Servicelimieten | Microsoft Docs
description: Understanding Azure digitale dubbels openbare preview-Servicelimieten
author: dwalthermsft
manager: deshner
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: dwalthermsft
ms.openlocfilehash: f9a3d934de47630ac3fd2356001014d006c2a4eb
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212264"
---
# <a name="public-preview-service-limits"></a>Servicebeperkingen van de openbare preview

Tijdens de **Public Preview**, Azure digitale dubbels heeft tijdelijke-abonnement, exemplaar en frequentielimieten die hieronder worden beschreven.

Deze beperkingen bestaan om te leren over de nieuwe service en de vele functies vereenvoudigen.

> [!NOTE]
> Deze limieten zal worden verhoogd en/of verwijderd door **algemene beschikbaarheid** (**GA**).

## <a name="per-subscription-limits"></a>Limieten voor een per abonnement

Tijdens de **Public Preview**, elk Azure-abonnement kunt maken of precies één exemplaar van Azure digitale dubbels uitgevoerd op een tijdstip.

> [!TIP]
> Verwijderen van uw exemplaar kunt u een nieuwe maken.

## <a name="per-instance-limits"></a>Limieten per exemplaar

Op zijn beurt kan elk exemplaar van Azure digitale dubbels hebben:

- Een **IoTHub** Resource
- Een **EventHub** -eindpunt voor het gebeurtenistype **DeviceMessage**
- Maximaal drie **EventHub**, **ServiceBus**, of **EventGrid** eindpunten van het gebeurtenistype **SensorChange**, **SpaceChange** , **TopologyOperation**, of **UdfCustom**

## <a name="management-api-limits"></a>Beheer-API-limieten

De aanvraag frequentielimieten toe voor uw API Management zijn:

- 100 aanvragen per seconde voor Management-API
- Een enkele Management-API-query kan maximaal 1000 objecten retourneren

> [!IMPORTANT]
> Als u de limiet van 1000 object overschrijdt, kunt u ontvangt een foutmelding en moet uw query te vereenvoudigen.

## <a name="udf-rate-limits"></a>Frequentielimieten UDF

De volgende limieten instellen voor het totale aantal alle de gebruiker gedefinieerde functieaanroepen naar uw Azure digitale Twins-exemplaar:

- 400 client library-aanroepen per seconde
- 100 **SendNotification** aanroepen per seconde

> [!NOTE]
> De volgende acties kunnen leiden tot extra frequentielimieten tijdelijk worden toegepast:
> - Topologie object metagegevens bewerken
> - UDF-definitie-updates
> - Apparaten verzenden van telemetrie voor de eerste keer

## <a name="device-telemetry-limits"></a>Apparaatlimieten telemetrie

De limieten hieronder cap het totale aantal alle berichten die uw apparaten kunnen verzenden naar uw Azure digitale Twins-exemplaar:

- 100 berichten per seconde

## <a name="next-steps"></a>Volgende stappen

Als u wilt uitproberen een voorbeeld van Azure digitale Twins, gaat u naar [Snelstartgids voor het vinden van beschikbare ruimten](./quickstart-view-occupancy-dotnet.md).