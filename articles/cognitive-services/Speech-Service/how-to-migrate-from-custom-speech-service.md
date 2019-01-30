---
title: Migreren van Custom Speech Service naar spraakservices
titlesuffix: Azure Cognitive Services
description: De Custom Speech Service maakt nu deel uit van de Speech-Service. Schakel over naar de Speech-Service om te profiteren van de meest recente kwaliteit en functie-updates.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 594233b9e345f9578c218b042a64ea167d50addb
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55211029"
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

> [!NOTE]
> * Als u spraak in Language Understanding (LUIS) hebt ingeschakeld, wordt één LUIS resource in dezelfde regio werkt voor LUIS, evenals de spraakservices. Zie voor meer informatie, [intents van gesproken inhoud herkennen](how-to-recognize-intents-from-speech-csharp.md).
> * Tekst-naar-tekst vertaling maakt geen deel uit van de Speech-Service. Deze functionaliteit is een eigen Azure-resourceabonnement vereist.
 


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
