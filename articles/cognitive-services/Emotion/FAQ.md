---
title: Veelgestelde vragen over emotion-API | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over de Emotion-API in cognitieve Services.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 01/26/2017
ms.author: anroth
ms.openlocfilehash: 8532d7c00fd8d7b01d84b5e55cb9bbc60241789c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344472"
---
# <a name="emotion-api-frequently-asked-questions"></a>Veelgestelde vragen over de emotion-API
 
> [!IMPORTANT]
> Video API Preview eindigt op 30 oktober 2017. Het nieuwe [Video indexeerfunctie API Preview](https://azure.microsoft.com/services/cognitive-services/video-indexer/) eenvoudig inzichten extraheren van video's en ervaringen van inhoud, zoals de lijst met zoekresultaten verbeteren doordat gesproken woorden, vlakken tekens en emoties. [Meer informatie](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-emotion-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Als u antwoorden op uw vragen in deze Veelgestelde vragen vinden kunt, probeert de Emotion-API-community op vragen [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) of neem contact op met Help en ondersteuning op [UserVoice](https://cognitive.uservoice.com/).  

-----

**Vraag**: *welke typen installatiekopieën Profiteer van het beste resultaat is van een Emotion-API?*

**Antwoord**: obstakels, volledige voorzijde analyseert installatiekopieën gebruiken voor de beste resultaten. Betrouwbaarheid afneemt met gedeeltelijke voorzijde vlakken en Emotion-API kan emoties in afbeeldingen waarbij de face 45 graden gedraaid of meer is niet herkend.

-----

**Vraag**: *hoeveel emoties kunt Emotion-API identificeren?*

**Antwoord**: Emotion-API herkent acht verschillende universeel geaccepteerd emoties: 
* Geluk
* Droefheid
* Verrassing
* Boosheid
* Angst
* Minachting
* Walging 
* Neutraal 

-----

**Vraag**: *Is er een manier om een live videostream doorgeven aan de API en het resultaat tegelijkertijd ophalen?*

**Antwoord**: de installatiekopie op basis emotion-API gebruiken en deze op elk frame aanroepen of frames voor prestaties overslaan.  Video Frame voor Frame Analysis voorbeelden zijn beschikbaar.

-----

**Vraag**: *ik ben doorgeven van de binaire gegevens in, maar geeft mij: 'ongeldig face-installatiekopie.**

**Antwoord**: dit betekent dat de algoritme een probleem bij het parseren van de installatiekopie heeft.  
* De indelingen ondersteunde invoer installatiekopie bevat JPEG-, PNG, GIF (het eerste frame), BMP. 
* De grootte van de installatiekopie-bestand mag niet groter zijn dan 4MB zijn
* Het bereik van de grootte waarneembaar face is 36 x 36 naar 4096 x 4096 pixels. Vlakken buiten dit bereik wordt niet gedetecteerd.
* Sommige vlakken kunnen niet worden herkend vanwege technische problemen, bijvoorbeeld zeer grote face hoeken (head-ven) groot Occlusie. Voorzijde en in de buurt binnen handbereik hebt de beste resultaten

-----
