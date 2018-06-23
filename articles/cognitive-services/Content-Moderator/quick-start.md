---
title: Azure Content beheerder aan de slag | Microsoft Docs
description: Klik hier voor meer informatie over het aan de slag met Azure Content-beheerder.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/15/2018
ms.author: sajagtap
ms.openlocfilehash: ae4333047ebd95733c7baaed0323a0c2c477d323
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344802"
---
# <a name="get-started-with-content-moderator"></a>Aan de slag met Content Moderator

U aan de slag met inhoud beheerder API's en het hulpprogramma controleren in de volgende manieren:

- [Beginnen met het hulpprogramma voor beoordeling](#start-with-the-review-tool) om zowel de API-sleutels en een evaluatie-team te maken. Verken het hulpprogramma voor beoordeling en informatie over het integreren met de inhoud beheerder-API's.
- [Abonneren op inhoud beheerder](#start-with-the-apis) in de Azure portal. U moet nog steeds online aanmelden voor het maken van een team controleren.
- [De stroom-connector en sjablonen gebruiken](https://flow.microsoft.com/connectors/shared_cognitiveservicescontentmoderator/content-moderator/) uitchecken van een breed scala aan integraties met de ontwerpfunctie voor een eenvoudig te gebruiken.

Ongeacht welke optie u kiest, Zie de [Referentiebeheer](review-tool-user-guide/credentials.md) artikel om te zoeken naar uw API-referenties.

## <a name="start-with-the-review-tool"></a>Beginnen met het hulpprogramma controleren
[Aanmelden](http://contentmoderator.cognitive.microsoft.com/) op de website van inhoud beheerder revisie hulpprogramma.

![Startpagina van inhoud beheerder](images/homepage.PNG)

### <a name="create-a-review-team"></a>Een evaluatie-team maken
Geef een naam op van uw team. Als u uw collega's uit te nodigen wilt, kunt u dit doen door te voeren hun e-mailadressen.

![Teamlid uitnodigen](images/QuickStart-2-small.png)

### <a name="upload-images-or-enter-text"></a>Uploaden van afbeeldingen of tekst invoeren
Klik op **probeer > installatiekopie** of **probeer > tekst**. Uploaden van installatiekopieën van maximaal vijf voorbeeld of geef een voorbeeldtekst voor controle.

![Probeer de installatiekopie of tekst toezicht](images/tryimagesortext.png)

### <a name="submit-for-automated-moderation"></a>Verzenden voor de automatische controle
De inhoud voor geautomatiseerde toezicht indienen. Het hulpprogramma voor beoordeling roept intern, het toezicht API's voor het scannen van uw inhoud. Zodra de scan voltooid is, ziet u een bericht dat u wordt geïnformeerd over de resultaten die wacht op uw.

![Gemiddeld bestanden](images/submitted.png)

### <a name="review-and-confirm-results"></a>Controleer en Bevestig resultaten
Bekijk de labels automatisch afgehandelde, desgewenst wijzigen en verzenden met behulp van de **volgende** knop. Als de zakelijke toepassing de API's van beheerder de gecodeerde inhoud wordt gestart roept, queuing klaar moet worden geëvalueerd door de teams menselijke controleren. U grote hoeveelheden inhoud met behulp van deze benadering voor het snel bekijken.

![Resultaat controleren](images/reviewresults.png)

Informatie over het gebruik van alle de [Bekijk de functies van het hulpprogramma](Review-Tool-User-Guide/human-in-the-loop.md) of verder gaan met de volgende sectie voor meer informatie over de API's. De aanmelding overslaan omdat u beschikt over de API-sleutel voor u ingericht in het hulpprogramma voor beoordeling, zoals wordt weergegeven in de [Referentiebeheer](review-tool-user-guide/credentials.md) artikel.

### <a name="use-the-apis"></a>Gebruik de API 's

Nu dat u hebt de inhoud toezicht verkend en hulpprogramma ervaring, informatie over het inhoud beheerder integreren met uw zakelijke toepassingen. Gebruik de volgende sectie voor meer informatie en fast track uw kennis met de SDK's en voorbeelden.

## <a name="start-with-the-apis"></a>Beginnen met de API 's

[Abonneren op inhoud beheerder](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) in de Azure portal. Beginnen met een van de volgende API's:

### <a name="image-moderation"></a>Beheer van afbeeldingen

Beginnen met de [API console](try-image-api.md) of gebruik de [.NET Quick Start](image-moderation-quickstart-dotnet.md) opgehaald om te scannen van afbeeldingen en potentiële inhoud voor volwassenen en mooie detecteren met behulp van tags, vertrouwen scores en andere informatie.

### <a name="text-moderation"></a>Beheer van tekst

Beginnen met de [API console](try-text-api.md) of gebruik de [.NET Quick Start](text-moderation-quickstart-dotnet.md) tekstinhoud voor potentiële taalgebruik, ongewenste tekst machine assisted-classificatie (preview) te scannen en persoonsgegevens worden aangemerkt gegevens worden verzameld. 


### <a name="video-moderation"></a>Beheer van video

Beginnen met de [.NET Quick Start](video-moderation-api.md) om te scannen video's en potentiële inhoud voor volwassenen en mooie detecteren. 


### <a name="review-apis"></a>Beoordelings-API's

Begin hier door te kiezen uit de taak, controleren en werkstroom API's.

- De [taak API](try-review-api-job.md) scant u uw inhoud met behulp van de controle-API's en beoordelingen genereert in het hulpprogramma voor beoordeling. 
- De [revisie API](try-review-api-review.md) rechtstreeks maakt op de installatiekopie, tekst of video beoordelingen voor menselijke moderators zonder eerst de inhoud wordt gescand. 
- De [werkstroom API](try-review-api-workflow.md) maakt, updates en informatie over de aangepaste werkstromen die uw team maakt opgehaald.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over inhoud toezicht vanaf de [installatiekopie toezicht API](image-moderation-api.md).
