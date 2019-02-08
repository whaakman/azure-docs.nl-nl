---
title: Migreren van Custom Speech Service naar spraakservices
titlesuffix: Azure Cognitive Services
description: De Custom Speech Service maakt nu deel uit van de Speech-Service. Schakel over naar de Speech-Service om te profiteren van de meest recente kwaliteit en functie-updates.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 698962aa0e3d72b204c4e990aa1384b44bf3896f
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55856886"
---
# <a name="migrate-from-the-custom-speech-service-to-the-speech-service"></a>Migreren van de Custom Speech Service naar de Speech-Service

Gebruik dit artikel voor het migreren van uw toepassingen uit de Custom Speech Service met de spraak-Service.

De Custom Speech Service maakt nu deel uit van de Speech-Service. Schakel over naar de Speech-Service om te profiteren van de meest recente kwaliteit en functie-updates.

## <a name="migration-for-new-customers"></a>Migratie voor nieuwe klanten

Het prijsmodel is eenvoudiger, met behulp van een prijsmodel op basis van een uur voor de Speech-Service.  

1. Maak een Azure-resource in elke regio waar uw toepassing beschikbaar is. De naam van de Azure-resource is **spraak**. U kunt een enkel Azure-resource gebruiken voor de volgende services in dezelfde regio bevinden, in plaats van afzonderlijke resources maken:

    * Spraak-naar-tekst
    * Aangepaste spraak-naar-tekst
    * Tekst naar spraak
    * Spraakomzetting

2. Download de [spraak SDK](speech-sdk.md).

3. Volg de snelstartgidsen en de SDK-voorbeelden voor het gebruik van de juiste API's. Als u de REST API's gebruikt, moet u ook de juiste eindpunten en resource-sleutels gebruiken.

4. De clienttoepassing Speech Service en API's gebruiken voor bijwerken.

## <a name="migration-for-existing-customers"></a>Migratie voor bestaande klanten

Uw bestaande resource sleutels migreren naar de Speech-Service op de Service voor spraak-portal. Voer de volgende stappen uit:

> [!NOTE]
> Resource-sleutels kunnen alleen worden gemigreerd binnen dezelfde regio.

1. Aanmelden bij de [cris.ai](http://www.cris.ai) portal en selecteer het abonnement in het menu rechtsboven.

2. Selecteer **geselecteerde abonnement migreren**.

3. De abonnementssleutel invoeren in het tekstvak in en selecteer **migreren**.

## <a name="next-steps"></a>Volgende stappen

* [Speech Service gratis uitproberen](get-started.md).
* Informatie over [spraak naar tekst](./speech-to-text.md) concepten.

## <a name="see-also"></a>Zie ook

* [Wat is de Speech-Service](overview.md)
* [Spraakservice en de SDK-documentatie](speech-sdk.md#get-the-sdk)
