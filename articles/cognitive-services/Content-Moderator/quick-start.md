---
title: Aan de slag met Content Moderator
titlesuffix: Azure Cognitive Services
description: Hoe u aan de slag met Content Moderator.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: sajagtap
ms.openlocfilehash: c2ac0ccd89b5f1436a151e3d69c5d7423090f244
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47225291"
---
# <a name="get-started-with-content-moderator"></a>Aan de slag met Content Moderator

U aan de slag met Content Moderator in de volgende manieren:

- [Beginnen met het beoordelingsprogramma](#start-with-the-review-tool) voor het ophalen van de API-sleutel en een beoordelingsteam maken. Het voordeel is dat u de API-sleutel gebruiken kunt om aan te roepen van het afbeeldingstoezicht-API's voor het scannen van inhoud en de beoordelings-API's voor het genereren van beoordelingen, zonder extra stappen uitvoeren.
- [Abonneren op Content Moderator](#start-with-the-apis) in Azure om op te halen van de API-sleutel. Bekijk de [API-verwijzing](api-reference.md) en de [SDK's](sdk-and-samples.md#sdks-for-python-java-nodejs-and-net). U moet wel voor het online aanmelden bij het maken van een beoordelingsteam.
- [Gebruik de Flow-connector en sjablonen](https://flow.microsoft.com/connectors/shared_cognitiveservicescontentmoderator/content-moderator/) uitchecken van een breed scala aan integraties met een ontwerper eenvoudig te gebruiken.

Ongeacht welke optie u kiest, Zie de [Referentiebeheer](review-tool-user-guide/credentials.md) artikel om te zoeken van uw API-referenties.

## <a name="start-with-the-review-tool"></a>Beginnen met het beoordelingsprogramma
[Meld u](http://contentmoderator.cognitive.microsoft.com/) op de website Content Moderator revisie hulpprogramma.

![Startpagina van inhoud Moderator](images/homepage.PNG)

### <a name="create-a-review-team"></a>Een beoordelingsteam maken
Geef een naam op van uw team. Als u uw collega's uitnodigen wilt, kunt u dit doen door in te voeren van hun e-mailadressen.

![Teamlid uitnodigen](images/QuickStart-2-small.png)

### <a name="upload-images-or-enter-text"></a>Afbeeldingen uploaden of tekst invoeren
Klik op **probeer > installatiekopie** of **probeer > tekst**. Uploaden van maximaal vijf voorbeeldafbeeldingen of geef een voorbeeldtekst voor beheer.

![Probeer afbeelding of het beheer van tekst](images/tryimagesortext.png)

### <a name="submit-for-automated-moderation"></a>Indienen voor geautomatiseerd toezicht
Dien uw inhoud voor geautomatiseerd toezicht. Het beoordelingsprogramma roept intern, de afbeeldingstoezicht-API's voor het scannen van uw inhoud. Nadat de scan voltooid is, ziet u een bericht over de resultaten wacht op uw Raadpleeg gemeld.

![Gemiddeld bestanden](images/submitted.png)

### <a name="review-and-confirm-results"></a>Controleren en de resultaten bevestigen
Bekijk de labels automatisch gecontroleerd, indien nodig, wijzigen en indienen met behulp van de **volgende** knop. Als uw business-toepassing-de Moderator-API's, de gecodeerde inhoud wordt gestart, queuing aanroepen kan worden gecontroleerd door de teams voor menselijke beoordeling. U grote hoeveelheden inhoud met behulp van deze benadering voor het snel bekijken.

![Resultaat controleren](images/reviewresults.png)

Informatie over het gebruik van alle de [Bekijk de functies van het hulpprogramma](Review-Tool-User-Guide/human-in-the-loop.md) of gaat u verder met de volgende sectie voor meer informatie over de API's. De registratie overslaan omdat u de API-sleutel voor u ingericht in het controlehulpprogramma, zoals wordt weergegeven in de [Referentiebeheer](review-tool-user-guide/credentials.md) artikel.

### <a name="use-the-apis"></a>Gebruik de API 's

Meer informatie over het Content Moderator integreren met uw zakelijke toepassingen. Bekijk de [API-verwijzing](api-reference.md) en de [SDK's](sdk-and-samples.md#sdks-for-python-java-nodejs-and-net).

## <a name="subscribe-in-the-azure-portal"></a>Abonneer u in Azure portal

[Abonneren op Content Moderator](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) in Azure portal. Begin met een van de volgende API's:

### <a name="image-moderation"></a>Beheer van afbeeldingen

Beginnen met de [API-console](try-image-api.md) of gebruik de [Snelstartgids voor .NET](image-moderation-quickstart-dotnet.md) opgehaald om te scannen van installatiekopieën en detecteren van mogelijke erotische en ongepaste inhoud met behulp van tags, vertrouwen scores en andere informatie.

### <a name="text-moderation"></a>Beheer van tekst

Beginnen met de [API-console](try-text-api.md) of gebruik de [Snelstartgids voor .NET](text-moderation-quickstart-dotnet.md) om te scannen op tekstinhoud voor potentieel grof taalgebruik, ongewenste tekst geautomatiseerd classificatie (preview), en persoonlijk identificeerbare informatie (PII). 


### <a name="video-moderation"></a>Beheer van video

Beginnen met de [Snelstartgids voor .NET](video-moderation-api.md) om te scannen van video's en detecteren van mogelijke erotische en ongepaste inhoud. 


### <a name="review-apis"></a>Beoordelings-API's

Begin hier door te kiezen uit de taak, controle en werkstroom API's.

- De [taak API](try-review-api-job.md) scant uw inhoud met behulp van de afbeeldingstoezicht-API's en beoordelingen worden gegenereerd in het beoordelingsprogramma. 
- De [revisie API](try-review-api-review.md) rechtstreeks maakt op afbeeldingen, tekst of een video beoordelingen voor menselijke moderators zonder eerst de inhoud wordt gescand. 
- De [werkstroom API](try-review-api-workflow.md) wordt gemaakt, bijgewerkt en krijgt u informatie over de aangepaste werkstromen die uw team maakt.

## <a name="next-steps"></a>Volgende stappen

Bekijk de [API-verwijzing](api-reference.md) en de [SDK's](sdk-and-samples.md#sdks-for-python-java-nodejs-and-net). Aan de slag met uw integratie met de [SDK voor .NET-voorbeelden](sdk-and-samples.md#net-sdk-samples), [REST-API-voorbeelden in C#](https://github.com/sanjeev3/azure-docs-pr/blob/master/articles/cognitive-services/Content-Moderator/sdk-and-samples.md#rest-api-samples-in-c) en [zelfstudies](sdk-and-samples.md#tutorials).
