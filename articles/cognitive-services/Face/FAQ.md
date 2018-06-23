---
title: Veelgestelde vragen over de Face-API-Service | Microsoft Docs
description: Hier vindt u antwoorden op de meest populaire vragen over de Face-API-Service.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 01/26/2017
ms.author: sbowles
ms.openlocfilehash: da2f75deef8a8beea3ba23b6a39eb6d2fe104b54
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344466"
---
# <a name="face-api-frequently-asked-questions"></a>Face-API Veelgestelde vragen
### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-face-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Als u antwoorden op uw vragen in deze Veelgestelde vragen vinden kunt, probeert de Face-API-community op vragen [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) of neem contact op met Help en ondersteuning op [UserVoice](https://cognitive.uservoice.com/).

-----
**Vraag**: nauwkeurigheid Face-API voor herkenning, verificatie of Zoek vergelijkbare welke factoren kan verminderen?

**Antwoord**: in het algemeen is dezelfde gevallen waar mensen identificeren iemand met inbegrip van moeilijkheden;
* Een of beide ogen blokkeren obstakels
* Scherp verlichting bijvoorbeeld ernstige sterk
* Wijzigingen aan haar stijl of gezichtsbehandeling haar
* Wijzigingen vanwege leeftijd
* Extreme bedacht (bijvoorbeeld Juichende)

Face-API is vaak in lastig in dergelijke gevallen geslaagd, maar nauwkeurigheid kan worden verkleind. Als u wilt maken voor krachtiger en deze uitdagingen, training van uw personen met foto's met een uiteenlopende hoeken en licht.

-----
**Vraag**: ophalen van een fout 'ongeldig face-installatiekopie', maar ik ben doorgeven van de binaire gegevens in.

**Antwoord**: dit betekent dat de algoritme een probleem bij het parseren van de installatiekopie heeft. Oorzaken zijn:
* De indelingen ondersteunde invoer installatiekopie bevat JPEG-, PNG, GIF (het eerste frame), BMP.
* De grootte van de installatiekopie-bestand mag niet groter zijn dan 4MB zijn
* Het bereik van de grootte waarneembaar face is 36 x 36 naar 4096 x 4096 pixels. Vlakken buiten dit bereik wordt niet gedetecteerd.
* Sommige vlakken kunnen niet worden herkend vanwege technische problemen, bijvoorbeeld zeer grote face hoeken (head-ven) groot Occlusie. Voorzijde en in de buurt binnen handbereik hebt de beste resultaten

-----
