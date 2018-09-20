---
title: Veelgestelde vragen - Emotion-API
titlesuffix: Azure Cognitive Services
description: Krijg antwoorden op veelgestelde vragen over de Emotion-API.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: anroth
ms.openlocfilehash: 56b3f2899f1e77c1a2b840285e2c69bdb8987ff4
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46363085"
---
# <a name="emotion-api-frequently-asked-questions"></a>Veelgestelde vragen over de emotion-API
 
> [!IMPORTANT]
> Voorbeeld van de video-API eindigt op 30 oktober 2017. Probeer de nieuwe [Video Indexer API Preview](https://azure.microsoft.com/services/cognitive-services/video-indexer/) eenvoudig inzichten ophalen uit video's en inhoud sneller wordt ontdekt, zoals de lijst met zoekresultaten verbeteren door het detecteren van gesproken woorden, gezichten, tekens en emoties. [Meer informatie](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-emotion-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Als u antwoorden op uw vragen niet in deze Veelgestelde vragen vinden, misschien dat de Emotion-API-community op [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) of neem contact op met Help en ondersteuning op [UserVoice](https://cognitive.uservoice.com/).  

-----

**Vraag**: *welke typen installatiekopieën krijgt het beste resultaat is van de Emotion-API?*

**Antwoord**: obstakels, volledige voorzijde gezichtsherkenning installatiekopieën gebruiken voor de beste resultaten. Betrouwbaarheid afneemt met gedeeltelijke voorzijde gezichten en emoties in afbeeldingen waar het gezicht 45 graden gedraaid of meer is mogelijk niet herkend door Emotion-API.

-----

**Vraag**: *hoeveel emoties kunt Emotion-API identificeren?*

**Antwoord**: Emotion-API herkent emoties van acht verschillende universeel geaccepteerd: 
* Geluk
* Droefheid
* Verrassing
* Boosheid
* Angst
* Minachting
* Walging 
* Neutraal 

-----

**Vraag**: *Is er manier om een live videostream doorgeven aan de API en tegelijkertijd het resultaat ophalen?*

**Antwoord**: gebruik van de installatiekopie op basis van emotion-API en deze op elk frame aanroepen of overgeslagen frames voor prestaties.  Video Frame-voor-Frame Analysis-voorbeelden zijn beschikbaar.

-----

**Vraag**: *ik ben doorgeven van de binaire gegevens in, maar het geeft me: "ongeldig gezicht.**

**Antwoord**: dit betekent dat de algoritme is een opgetreden bij het parseren van de installatiekopie.  
* De ondersteunde bestandsindelingen voor invoer bevat JPEG, PNG-, GIF-bestand (het eerste frame), BMP. 
* De grootte van de installatiekopie-bestand moet niet groter zijn dan 4MB
* Het bereik van de grootte detecteerbare gezicht is 36 x 36 naar 4096 x 4096 pixels. Gezichten buiten dit bereik niet gedetecteerd
* Sommige gezichten worden mogelijk niet gedetecteerd vanwege technische problemen, bijvoorbeeld zeer grote face hoeken (hoofd-houding), grote bedekking. Gezichten voorzijde en in de buurt binnen handbereik hebben de beste resultaten

-----
