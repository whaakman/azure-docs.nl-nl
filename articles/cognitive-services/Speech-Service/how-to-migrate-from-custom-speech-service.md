---
title: Migreren van Custom Speech Service naar spraak-service
titlesuffix: Azure Cognitive Services
description: Meer informatie over het migreren van de Custom Speech service met de spraak-Service.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 10/01/2018
ms.author: panosper
ms.openlocfilehash: b5f23cf5a9dc3a675d9103bb32f686d387fda837
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48239931"
---
# <a name="migrate-from-the-custom-speech-service-to-the-speech-service"></a>Migreren van de Custom Speech service naar de Speech-service

Gebruik dit artikel voor het migreren van uw toepassingen uit de Custom Speech service met de spraak-Service.

De Custom Speech Service maakt nu deel uit van de Speech-Service. Schakel over naar de Speech-Service om te profiteren van de meest recente kwaliteit en functie-updates.
 
## <a name="migration-for-new-customers"></a>Migratie voor nieuwe klanten

Het prijsmodel is eenvoudiger, verplaatsen naar een prijsmodel op basis van een uur voor de Speech-Service.   

1. Maak een Azure-resource in elke regio waar uw toepassing beschikbaar is. De naam van de Azure-resource is **spraak**. U kunt een enkel Azure-resource gebruiken voor de volgende services in dezelfde regio bevinden, in plaats van afzonderlijke resources maken:

    * Spraak-naar-tekst
    * Aangepaste spraak-naar-tekst
    * Tekst naar spraak
    * Spraakomzetting

2. Download de [spraak SDK](speech-sdk.md). 

3. Volg de snelstartgidsen en de SDK-voorbeelden voor het gebruik van de juiste API's. Als u de REST API's gebruikt, moet u ook de juiste eindpunten en resource-sleutels gebruiken. 

4. Werk de clienttoepassing de spraakservice en API's gebruiken. 

> [!NOTE]
> * LUIS - inschakeling spraak in Language Understanding (LUIS), één LUIS resource in dezelfde regio werkt voor LUIS, evenals de spraakservices. Zie [intents van gesproken inhoud herkennen](how-to-recognize-intents-from-speech-csharp.md) documentatie.
> * Tekst-naar-tekst vertaling maakt geen deel uit van de spraak-service. Er is een eigen Azure-resource-abonnement nodig.
  


## <a name="migration-for-existing-customers"></a>Migratie voor bestaande klanten

Bestaande klanten zijn vereist voor het migreren van hun bestaande resource sleutels naar de nieuwe service in de portal Speech-Service. Voer de volgende stappen uit: 

> [!NOTE] 
> Resource-sleutels kunnen alleen worden gemigreerd binnen dezelfde regio. 

1. Aanmelden bij de [cris.ai](http://www.cris.ai) portal en selecteer het abonnement in het menu rechtsboven. 

2. Selecteer **geselecteerde abonnement migreren**.

3. De abonnementssleutel invoeren in het tekstvak in en selecteer **migreren**.

    ![Voer van abonnement als u wilt migreren](./media/migrate-from-custom-speech-service/migrate-dialog.png)

## <a name="next-steps"></a>Volgende stappen

* [Speech Service gratis uitproberen](get-started.md)
* Informatie over [spraak naar tekst](./speech-to-text.md) concepten

## <a name="see-also"></a>Zie ook

* [Wat is de Speech-service](overview.md)
* [Spraakservice en de SDK-documentatie](speech-sdk.md#get-the-sdk)