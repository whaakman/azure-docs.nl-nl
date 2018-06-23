---
title: Veelgestelde vragen over de Computer Vision-API | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over de Computer Vision-API in cognitieve Microsoft-Services.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 01/26/2017
ms.author: kefre
ms.openlocfilehash: 5c862dd2fb26a005f4e785673a4e9358ecf9286f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344461"
---
# <a name="computer-vision-api-frequently-asked-questions"></a>Computer Vision-API Veelgestelde vragen
### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-computer-vision-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Als u uw vragen worden beantwoord in deze Veelgestelde vragen vinden kunt, probeert u gevraagd de Computer Vision-API-community op [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) of neem contact op met [Help en ondersteuning op UserVoice](https://cognitive.uservoice.com/)

-----

**Vraag**: *kan ik trainen Computer Vision-API om aangepaste labels te gebruiken?  Bijvoorbeeld: ik wil feed in de afbeeldingen van kattenrassen de AI trainen, vervolgens ontvangt de RAS-waarde in een AI-aanvraag.*

**Antwoord**: deze functie is momenteel niet beschikbaar. Onze technici werken echter ervoor zorgt dat deze functionaliteit op de Computer Vision.

-----

**Vraag**: *Computer Vision kan lokaal worden gebruikt zonder internetverbinding?*

**Antwoord**: momenteel bieden We geen een on-premises of lokale oplossing.

-----

**Vraag**: *welke talen worden ondersteund met Computer Vision?*

**Antwoord**: ondersteunde talen zijn onder andere:

| | | Ondersteunde talen | | |
|---------------- |------------------ |------------------ |--------------------------- |--------------------
| Deens (da-DK)  | Nederlands (nl-NL)     | Nederlands           | Fins (fi-FI)            |Frans (fr-FR)
| Duits (nl-nl)  | Grieks (el-GR)     | Hongaars (hu-HU) | Italiaans (it-IT)            | Japans (ja-JP)
| Koreaans (ko-KR)  | Noors (nb-NO) | Pools (pl-PL)    | Portugees (pt-BR) (pt-PT) | Russisch (ru-RU)
| Spaans (es-ES)   | Zweeds (sv-SV)     | Turks (tr TU)   |                            |

-----

**Vraag**: *Computer Vision kunnen worden gebruikt om te lezen overschrijven?*

**Antwoord**: de Vision-API biedt goede tekst detectie met OCR, maar deze momenteel niet is geoptimaliseerd voor overschrijven. We zijn voortdurend probeert te verbeteren van onze services en OCR voor automatische nummerbord opname aan onze lijst met functieaanvragen hebt toegevoegd.

-----

**Vraag:** *welke talen worden ondersteund voor handschriftherkenning?*

**Antwoord**: alleen Engels wordt momenteel ondersteund.

-----

**Vraag**: *welke soorten verwerkingsinformatie schrijven worden ondersteund voor handschriftherkenning?*

**Antwoord**: de technologie werkt met verschillende soorten verwerkingsinformatie, inclusief whiteboards, witboek en opmerkingen bij de gele vergrendeld.

-----

**Vraag**: *hoe lang duurt de bewerking van de erkenning handschriftherkenning voordat?*

**Antwoord**: de hoeveelheid tijd die nodig is, is afhankelijk van de lengte van de tekst. Voor langere teksten kan het duren enkele seconden. Daarom nadat de geschreven tekst herkennen-bewerking is voltooid, wellicht u moet wachten voordat u de resultaten met de bewerking ophalen geschreven tekst bewerkingsresultaat kunt ophalen.

-----

**Vraag**: *hoe gaat de handschriftherkenning erkenning technologie ingang tekst die wordt ingevoegd met behulp van een dakje in het midden van een regel?*

**Antwoord**: dergelijke tekst wordt geretourneerd als een afzonderlijke regel door de bewerking van handschriftherkenning herkenning.

-----

**Vraag**: *hoe wordt de opname-technologie van handschriftherkenning verwerkt in gekruist out woorden of regels?*

**Antwoord**: als de woorden worden overschreden uit met meerdere regels om weer te geven ze onherkenbare, de bewerking van de erkenning handschriftherkenning niet ophalen. Echter als de woorden worden overschreden uit met behulp van één regel, die kruisende wordt behandeld als ruis en de woorden in die nog steeds ophalen opgehaald door de bewerking van handschriftherkenning herkenning.

-----

**Vraag**: *welke richtingen tekst worden ondersteund voor de opname-technologie van handschriftherkenning?*

**Antwoord**: tekst onder een hoek van maximaal ongeveer 30 graden en 40 graden georiënteerde ophalen opgepikt door de bewerking van handschriftherkenning herkenning.

-----
