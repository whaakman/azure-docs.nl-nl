---
title: Veelgestelde vragen - Computer Vision
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
ms.custom: seodec18
ms.openlocfilehash: 3146650280af80923a4ed9cbb78a5ce6d43e5bce
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53584272"
---
# <a name="computer-vision-api-frequently-asked-questions"></a>Veelgestelde vragen over de computer Vision-API

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-computer-vision-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Als u antwoorden op uw vragen niet in deze Veelgestelde vragen vinden, misschien dat de Computer Vision-API-community op [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) of neem contact op met [Help en ondersteuning op UserVoice](https://cognitive.uservoice.com/)

-----

**Vraag**: *Kan ik trainen met Computer Vision-API om aangepaste labels te gebruiken?  Ik wil graag bijvoorbeeld feed in foto's van kattenrassen de AI trainen en vervolgens de waarde binnen de branche op een AI-aanvraag ontvangen.*

**antwoord**: Deze functie is momenteel niet beschikbaar. Echter, onze technici werken om deze functionaliteit voor Computer Vision.

-----

**Vraag**: *Kan Computer Vision lokaal worden gebruikt zonder internetverbinding?*

**antwoord**: Momenteel bieden we geen een on-premises of lokale oplossing.

-----

**Vraag**: *Welke talen worden ondersteund met Computer Vision?*

**antwoord**: Enkele ondersteunde talen:

| | | Ondersteunde talen | | |
|---------------- |------------------ |------------------ |--------------------------- |--------------------
| Deens (da-DK)  | Nederlands (nl-NL)     | Nederlands           | Fins (fi-FI)            |Frans (fr-FR)
| Duits (nl-nl)  | Grieks (el-GR)     | Hongaars (hu-HU) | Italiaans (it-IT)            | Japans (ja-JP)
| Koreaans (ko-KR)  | Noors (nb-NO) | Pools (pl-PL)    | Portugees (pt-BR) (pt-PT) | Russisch (ru-RU/s)
| Spaans (es-ES)   | Zweeds (sv-SV)     | Turks (tr-TR)   |                            |

-----

**Vraag**: *Kan Computer Vision worden gebruikt om te lezen licentie platen?*

**antwoord**: De Vision-API biedt goede tekst-detectie met OCR, maar deze momenteel niet is geoptimaliseerd voor licentie-elementen. We zijn voortdurend gewerkt aan onze services te verbeteren en OCR voor automatische licentie plaat erkenning aan onze lijst met functieaanvragen hebt toegevoegd.

-----

**Vraag:** *Welke talen worden ondersteund voor handschriftherkenning?*

**antwoord**: Op dit moment wordt alleen Engels ondersteund.

-----

**Vraag**: *Welke typen oppervlakken schrijven worden ondersteund voor handschriftherkenning?*

**antwoord**: De technologie werkt met verschillende soorten oppervlakken, met inbegrip van whiteboards en wit papier, gele plaknotities.

-----

**Vraag**: *Hoe lang duurt de bewerking van de spraakherkenning aanpassen van handschriftherkenning?*

**antwoord**: De hoeveelheid tijd die nodig is, is afhankelijk van de lengte van de tekst. Voor langere teksten, kan het duren in enkele seconden. Dus nadat de handgeschreven tekst herkennen-bewerking is voltooid, mogelijk moet u wachten voordat u de resultaten met behulp van de bewerking ophalen handgeschreven tekst bewerkingsresultaat kunt ophalen.

-----

**Vraag**: *Hoe wordt de herkenning van handgeschreven tekstherkenningstechnologie die is ingevoegd met behulp van een caret-teken in het midden van een regel verwerkt?*

**antwoord**: Deze tekst wordt geretourneerd als een afzonderlijke regel door de bewerking van de spraakherkenning aanpassen van handschriftherkenning.

-----

**Vraag**: *Hoe wordt de technologie van de spraakherkenning aanpassen van handschriftherkenning Gekruiste-out woorden of regels verwerkt?*

**antwoord**: Als de woorden worden overschreden om met meerdere regels om weer te geven dat deze niet wordt herkend, ophalen de werking van de spraakherkenning aanpassen van handschriftherkenning niet. Als de woorden worden overschreden om met behulp van één regel, dat kruisende wordt beschouwd als ruis, en de woorden nog steeds ophalen opgehaald door de bewerking van de spraakherkenning aanpassen van handschriftherkenning.

-----

**Vraag**: *Welke standen tekst worden ondersteund voor de herkenning van handgeschreven tekstherkenningstechnologie?*

**antwoord**: Tekst die zijn gericht op hoeken van maximaal ongeveer 30 graden 40 graden kan door de bewerking van de spraakherkenning aanpassen van handschriftherkenning ophalen opgehaald.

-----
