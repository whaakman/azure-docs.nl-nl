---
title: Azure digitale dubbels openbare preview-Servicelimieten | Microsoft Docs
description: Informatie over dat Azure digitale dubbels openbare preview-Servicelimieten
author: dwalthermsft
manager: deshner
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: dwalthermsft
ms.openlocfilehash: 86ae75118dd1311ea2ae92fb718fe4c58b8e5673
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50961752"
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

- Een **IoTHub** resource.
- Een **EventHub** -eindpunt voor het gebeurtenistype **DeviceMessage**.
- Maximaal drie **EventHub**, **ServiceBus**, of **EventGrid** eindpunten van het gebeurtenistype **SensorChange**, **SpaceChange** , **TopologyOperation**, of **UdfCustom**.

## <a name="management-api-limits"></a>Beheer-API-limieten

De aanvraag frequentielimieten toe voor uw API Management zijn:

- 100 aanvragen per seconde naar de API Management.
- Maximaal 1000 objecten geretourneerd door een enkele Management-API-query. 

> [!IMPORTANT]
> Als u de limiet van 1000-object overschrijdt, kunt u een foutbericht krijgt en moet Vereenvoudig de query.

## <a name="udf-rate-limits"></a>Frequentielimieten UDF

De volgende limieten instellen voor het totale aantal alle de gebruiker gedefinieerde functieaanroepen naar uw Azure digitale Twins-exemplaar:

- 400 client library-aanroepen per seconde
- 100 **SendNotification** aanroepen per seconde

> [!NOTE]
> De volgende acties kunnen leiden tot extra frequentielimieten tijdelijk worden toegepast:
> - Wijzigingen in de metagegevens van een object topologie
> - Wijzigingen in de UDF-definitie
> - Apparaten die telemetrie voor de eerste keer verzenden

## <a name="device-telemetry-limits"></a>Apparaatlimieten telemetrie

De volgende limieten gegevenslimiet het totale aantal alle berichten die uw apparaten naar uw Azure digitale Twins-exemplaar verzenden kunnen:

- 100 berichten per seconde

## <a name="next-steps"></a>Volgende stappen

Als u wilt uitproberen een voorbeeld van Azure digitale Twins, gaat u naar [Snelstartgids voor het vinden van beschikbare ruimten](./quickstart-view-occupancy-dotnet.md).