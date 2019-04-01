---
title: 'Quickstart: Probeer het Content Moderator op het web - Content Moderator'
titlesuffix: Azure Cognitive Services
description: In deze snelstartgids gebruikt u het online Content Moderator-controlehulpprogramma de basisfunctionaliteit van Content Moderator testen zonder code te schrijven.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: d1c65b590fd68f68fdaeb110c0fd00e0c5a6649a
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756468"
---
# <a name="quickstart-try-content-moderator-on-the-web"></a>Quickstart: Probeer het Content Moderator op het web

In deze snelstartgids gebruikt u het online Content Moderator-controlehulpprogramma de basisfunctionaliteit van Content Moderator testen zonder code te schrijven. Als u deze service sneller integreren in uw app wilt, raadpleegt u de andere snelstarts in de [Vervolgstappen](#next-steps) sectie.

## <a name="prerequisites"></a>Vereisten

- Een webbrowser

## <a name="set-up-the-review-tool"></a>Instellen van het beoordelingsprogramma
De Content Moderator-controlehulpprogramma is een Webhulpprogramma waarmee menselijke revisoren de cognitive service bij het nemen van beslissingen. In deze handleiding gaat u door de korte proces van het instellen van het beoordelingsprogramma zodat u kunt zien hoe de Content Moderator-service werkt. Ga naar de [Content Moderator-controlehulpprogramma](https://contentmoderator.cognitive.microsoft.com/) site en zich registreren.

![Startpagina van inhoud Moderator](images/homepage.PNG)

## <a name="create-a-review-team"></a>Een beoordelingsteam maken

Maak vervolgens een beoordelingsteam. Dit is in een werkende scenario, de groep van mensen die het beheer van beslissingen te nemen van de service handmatig controleren. Nu moet u alleen de naam van een team maken. Als u uitnodigen van collega's aan het team wilt, kunt u dit doen door in te voeren van hun e-mailadressen.

![Teamlid uitnodigen](images/QuickStart-2-small.png)

## <a name="upload-sample-content"></a>Voorbeeldinhoud uploaden

U bent nu klaar om te uploaden voorbeelden voor inhoud. Selecteer **probeer > installatiekopie**, **probeer > tekst**, of **probeer > Video**.

![Probeer afbeelding of het beheer van tekst](images/tryimagesortext.png)

Dien uw inhoud voor beheer. Het beoordelingsprogramma roept intern, de afbeeldingstoezicht-API's voor het scannen van uw inhoud. Nadat de scan voltooid is, ziet u een bericht gemeld dat er resultaten wacht op uw Raadpleeg zijn.

![Gemiddeld bestanden](images/submitted.png)

## <a name="review-moderation-tags"></a>Toezicht op tags bekijken

Controleer de toegepaste toezicht-codes. U kunt zien welke labels zijn toegepast op uw inhoud en de score is in elke categorie. Zie de [installatiekopie](image-moderation-api.md), [tekst](text-moderation-api.md), en [Video](video-moderation-api.md) toezicht onderwerpen voor meer informatie over wat de andere inhoud tags wordt aangegeven.

![Resultaat controleren](images/reviewresults_text.png)

In een project kunt u of uw beoordelingsteam wijzigen van deze tags of meer tags indien nodig toevoegen. U zult deze wijzigingen met verzenden de **volgende** knop. Als uw business-toepassing-de Moderator-API's aanroepen, wordt de gecodeerde inhoud in de wachtrij plaatsen hier, klaar om te worden gecontroleerd door de teams voor menselijke beoordeling. U kunt snel grote hoeveelheden inhoud met behulp van deze benadering bekijken.

Op dit moment hebt u het Content Moderator-controlehulpprogramma gebruikt om te zien wat de Content Moderator-service kunt doen. Volgende, u kunt ofwel meer informatie over het beoordelingsprogramma en het integreren met een softwareproject met behulp van de API bekijken, of kunt u doorgaan met de [Vervolgstappen](#next-steps) sectie voor informatie over het gebruik van de beheer-API's zelf in uw app.

## <a name="learn-more-about-the-review-tool"></a>Meer informatie over het beoordelingsprogramma

Voor meer informatie over het gebruik van het hulpprogramma Content Moderator bekijken, bekijk de [beoordelingsprogramma](Review-Tool-User-Guide/human-in-the-loop.md) begeleiden en zien het beoordelingsprogramma API's voor informatie over het afstemmen van de ervaring voor menselijke beoordeling:
- De [taak API](try-review-api-job.md) scant uw inhoud met behulp van de afbeeldingstoezicht-API's en beoordelingen worden gegenereerd in het beoordelingsprogramma. 
- De [revisie API](try-review-api-review.md) rechtstreeks maakt op afbeeldingen, tekst of een video beoordelingen voor menselijke moderators zonder eerst de inhoud wordt gescand. 
- De [werkstroom API](try-review-api-workflow.md) wordt gemaakt, bijgewerkt en krijgt u informatie over de aangepaste werkstromen die uw team maakt.

Of Ga door met de volgende stappen aan de slag met behulp van de beheer-API's in uw code.

## <a name="next-steps"></a>Volgende stappen

Informatie over het gebruik van de beheer-API's zelf in uw app.
- Implementeren van afbeeldingen. Gebruik de [API-console](try-image-api.md) of de [ C# snelstartgids](image-moderation-quickstart-dotnet.md) opgehaald om te scannen van installatiekopieën en detecteren van mogelijke erotische en ongepaste inhoud met behulp van tags, vertrouwen scores en andere informatie.
- Beheer van tekst implementeren. Gebruik de [API-console](try-text-api.md) of gebruik de [ C# snelstartgids](text-moderation-quickstart-dotnet.md) om te scannen op tekstinhoud voor potentieel grof taalgebruik en ongewenste tekst geautomatiseerd classificatie (preview), persoonlijke gegevens.
- Videotoezicht implementeren. Ga als volgt de [procedures voor het beheer van Video-handleiding voor C# ](video-moderation-api.md) om te scannen van video's en detecteren van mogelijke erotische en ongepaste inhoud. 
