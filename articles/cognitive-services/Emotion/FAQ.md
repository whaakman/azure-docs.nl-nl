---
title: Veelgestelde vragen - Emotion-API
titlesuffix: Azure Cognitive Services
description: Krijg antwoorden op veelgestelde vragen over de Emotion-API.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 3654d315867cfe60c277806d4246469adf0a791f
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56238263"
---
# <a name="emotion-api-frequently-asked-questions"></a>Veelgestelde vragen over de emotion-API

> [!IMPORTANT]
> De Emotion-API wordt op 15 februari 2019 afgeschaft. De mogelijkheid voor de herkenning van emoties is nu algemeen beschikbaar als onderdeel van de [Face-API](https://docs.microsoft.com/azure/cognitive-services/face/).

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-emotion-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Als u antwoorden op uw vragen niet in deze Veelgestelde vragen vinden, misschien dat de Emotion-API-community op [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) of neem contact op met Help en ondersteuning op [UserVoice](https://cognitive.uservoice.com/).  

-----

**Vraag**: *Welke soorten afbeeldingen krijgt de beste resultaten van de Emotion-API?*

**antwoord**: Obstakels, volledige voorzijde gezichtsherkenning installatiekopieën gebruiken voor de beste resultaten. Betrouwbaarheid afneemt met gedeeltelijke voorzijde gezichten en emoties in afbeeldingen waar het gezicht meer dan 45 graden is gedraaid mogelijk niet herkend door de Emotion-API.

-----

**Vraag**: *Hoeveel emoties kunt identificeren door Emotion-API?*

**antwoord**: Emotion-API herkent acht verschillende universeel aanvaard emoties:
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

**antwoord**: Gebruik de installatiekopie van het op basis van de emotion-API en deze op elk frame aanroepen of overgeslagen frames voor prestaties.  Video Frame-voor-Frame Analysis-voorbeelden zijn beschikbaar.

-----

**Vraag**: *Ik ben doorgeven van de binaire gegevens in, maar het geeft me: "Ongeldige gezicht.**

**antwoord**: Dit bericht geeft aan dat de algoritme is een opgetreden bij het parseren van de installatiekopie.  
* De afbeelding ondersteunde indelingen omvatten JPEG, PNG-, GIF-bestand (het eerste frame), BMP
* De grootte van de installatiekopie-bestand moet niet groter zijn dan 4 MB
* Het bereik van de grootte detecteerbare gezicht is 36 x 36 naar 4096 x 4096 pixels. Gezichten buiten dit bereik wordt niet gedetecteerd
* Vanwege technische problemen, bijvoorbeeld grote face hoeken (hoofd-houding), grote bedekking mogelijk niet sommige gezichten gedetecteerd. Gezichten voorzijde en in de buurt binnen handbereik hebben de beste resultaten

-----
