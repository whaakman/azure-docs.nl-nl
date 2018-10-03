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
ROBOTS: NOINDEX
ms.openlocfilehash: 6c1c4b8e5c2701f3c419a58bc3fdc33f7e629bbd
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48238514"
---
# <a name="emotion-api-frequently-asked-questions"></a>Veelgestelde vragen over de emotion-API

> [!IMPORTANT]
> De Emotion-API wordt op 15 februari 2019 worden afgeschaft. De mogelijkheid van de herkenning van emoties is nu algemeen beschikbaar is als onderdeel van de [Face-API](https://docs.microsoft.com/azure/cognitive-services/face/).

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-emotion-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Als u antwoorden op uw vragen niet in deze Veelgestelde vragen vinden, misschien dat de Emotion-API-community op [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) of neem contact op met Help en ondersteuning op [UserVoice](https://cognitive.uservoice.com/).  

-----

**Vraag**: *welke typen installatiekopieën krijgt het beste resultaat is van de Emotion-API?*

**Antwoord**: obstakels, volledige voorzijde gezichtsherkenning installatiekopieën gebruiken voor de beste resultaten. Betrouwbaarheid afneemt met gedeeltelijke voorzijde gezichten en emoties in afbeeldingen waar het gezicht meer dan 45 graden is gedraaid mogelijk niet herkend door de Emotion-API.

-----

**Vraag**: *hoeveel emoties kunt Emotion-API identificeren?*

**Antwoord**: Emotion-API herkent acht verschillende universeel aanvaard emoties:
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

**Antwoord**: dit bericht betekent dat de algoritme is een opgetreden bij het parseren van de installatiekopie.  
* De afbeelding ondersteunde indelingen omvatten JPEG, PNG-, GIF-bestand (het eerste frame), BMP
* De grootte van de installatiekopie-bestand moet niet groter zijn dan 4 MB
* Het bereik van de grootte detecteerbare gezicht is 36 x 36 naar 4096 x 4096 pixels. Gezichten buiten dit bereik wordt niet gedetecteerd
* Vanwege technische problemen, bijvoorbeeld grote face hoeken (hoofd-houding), grote bedekking mogelijk niet sommige gezichten gedetecteerd. Gezichten voorzijde en in de buurt binnen handbereik hebben de beste resultaten

-----
