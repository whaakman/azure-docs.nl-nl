---
title: Veelgestelde vragen voor de Computer Vision-API
titlesuffix: Azure Cognitive Services
description: Krijg antwoorden op veelgestelde vragen over de Computer Vision-API in Azure Cognitive Services.
services: cognitive-services
author: KellyDF
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: kefre
ms.openlocfilehash: 5c0a4e981a290b9a758c8401a75e546c61618b45
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45983898"
---
# <a name="computer-vision-api-frequently-asked-questions"></a>Veelgestelde vragen over de computer Vision-API

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-computer-vision-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Als u antwoorden op uw vragen niet in deze Veelgestelde vragen vinden, misschien dat de Computer Vision-API-community op [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) of neem contact op met [Help en ondersteuning op UserVoice](https://cognitive.uservoice.com/)

-----

**Vraag**: *kan ik trainen Computer Vision-API om aangepaste labels te gebruiken?  Ik wil graag bijvoorbeeld feed in foto's van kattenrassen de AI trainen en vervolgens de waarde binnen de branche op een AI-aanvraag ontvangen.*

**Antwoord**: deze functie is momenteel niet beschikbaar. Echter, onze technici werken om deze functionaliteit voor Computer Vision.

-----

**Vraag**: *Computer Vision kan lokaal worden gebruikt zonder internetverbinding?*

**Antwoord**: op dit moment bieden We geen een on-premises of lokale oplossing.

-----

**Vraag**: *welke talen worden ondersteund met Computer Vision?*

**Antwoord**: ondersteunde talen zijn onder andere:

| | | Ondersteunde talen | | |
|---------------- |------------------ |------------------ |--------------------------- |--------------------
| Deens (da-DK)  | Nederlands (nl-NL)     | Nederlands           | Fins (fi-FI)            |Frans (fr-FR)
| Duits (nl-nl)  | Grieks (el-GR)     | Hongaars (hu-HU) | Italiaans (it-IT)            | Japans (ja-JP)
| Koreaans (ko-KR)  | Noors (nb-NO) | Pools (pl-PL)    | Portugees (pt-BR) (pt-PT) | Russisch (ru-RU/s)
| Spaans (es-ES)   | Zweeds (sv-SV)     | Turks (tr-TU)   |                            |

-----

**Vraag**: *Computer Vision kan worden gebruikt om te lezen platen van de licentie?*

**Antwoord**: de Vision-API biedt goede tekst-detectie met OCR, maar deze momenteel niet is geoptimaliseerd voor licentie-elementen. We zijn voortdurend gewerkt aan onze services te verbeteren en OCR voor automatische licentie plaat erkenning aan onze lijst met functieaanvragen hebt toegevoegd.

-----

**Vraag:** *welke talen worden ondersteund voor handschriftherkenning?*

**Antwoord**: op dit moment alleen Engels ondersteund.

-----

**Vraag**: *welke typen oppervlakken schrijven worden ondersteund voor handschriftherkenning?*

**Antwoord**: de technologie werkt met verschillende soorten oppervlakken, met inbegrip van whiteboards en wit papier, gele plaknotities.

-----

**Vraag**: *hoe lang de herkenning van handgeschreven tekst bewerking duurt?*

**Antwoord**: de hoeveelheid tijd die nodig is, is afhankelijk van de lengte van de tekst. Voor langere teksten, kan het duren in enkele seconden. Dus nadat de handgeschreven tekst herkennen-bewerking is voltooid, mogelijk moet u wachten voordat u de resultaten met behulp van de bewerking ophalen handgeschreven tekst bewerkingsresultaat kunt ophalen.

-----

**Vraag**: *hoe betekent de handschriftherkenning erkenning technologie ingang tekst die is ingevoegd met behulp van een caret-teken in het midden van een regel?*

**Antwoord**: deze tekst wordt geretourneerd als een afzonderlijke regel door de bewerking van de spraakherkenning aanpassen van handschriftherkenning.

-----

**Vraag**: *hoe wordt de technologie van de spraakherkenning aanpassen van handschriftherkenning verwerkt Gekruiste-out woorden of regels?*

**Antwoord**: als de woorden worden overschreden om met meerdere regels om weer te geven dat deze niet wordt herkend, de werking van de spraakherkenning aanpassen van handschriftherkenning niet ophalen. Als de woorden worden overschreden om met behulp van één regel, dat kruisende wordt beschouwd als ruis, en de woorden nog steeds ophalen opgehaald door de bewerking van de spraakherkenning aanpassen van handschriftherkenning.

-----

**Vraag**: *welke standen tekst worden ondersteund voor de herkenning van handgeschreven tekstherkenningstechnologie?*

**Antwoord**: tekst die zijn gericht op hoeken van maximaal ongeveer 30 graden 40 graden kan ophalen opgehaald door de bewerking van de spraakherkenning aanpassen van handschriftherkenning.

-----
