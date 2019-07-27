---
title: Veelgestelde vragen-Computer Vision
titleSuffix: Azure Cognitive Services
description: Krijg antwoorden op veelgestelde vragen over de Computer Vision-API in azure Cognitive Services.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: e8ab205a43e5cb1e8e2b96dbd9600e1fceb29403
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68564606"
---
# <a name="computer-vision-api-frequently-asked-questions"></a>Veelgestelde vragen over Computer Vision-API

> [!TIP]
> Als u in deze veelgestelde vragen geen antwoorden op uw vragen kunt vinden, kunt u de Computer Vision-API Community vragen op [stack overflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) of contact opnemen met de [Help en ondersteuning op UserVoice](https://cognitive.uservoice.com/)

---

**Vraag**: *Kan ik Computer Vision-API trainen om aangepaste labels te gebruiken?  Ik wil bijvoorbeeld dat ik in afbeeldingen van kat rassen de AI ' Train ', en vervolgens de RAS-waarde op een AI-aanvraag kan ontvangen.*

**Antwoord**: Deze functie is momenteel niet beschikbaar. Onze technici werken echter aan om deze functionaliteit te Computer Vision.

---

**Vraag**: *Kunnen Computer Vision lokaal worden gebruikt zonder een Internet verbinding?*

**Antwoord**: We bieden momenteel geen on-premises of lokale oplossing.

---

**Vraag**: *Kan Computer Vision worden gebruikt voor het lezen van de licentie platen?*

**Antwoord**: De Vision-API biedt een goede detectie van tekst met OCR, maar is momenteel niet geoptimaliseerd voor licentie platen. We proberen onze services voortdurend te verbeteren en hebben OCR toegevoegd voor de erkenning van automatische licentie plaat aan onze lijst met functie aanvragen.

---

**Vraag**: *Welke typen schrijf oppervlakken worden ondersteund voor handschrift herkenning?*

**Antwoord**: De technologie werkt met verschillende soorten Opper vlakken, waaronder white boards, wit papier en gele plak notities.

---

**Vraag**: *Hoe lang duurt de bewerking voor handschrift herkenning?*

**Antwoord**: Hoe lang het duurt, hangt af van de lengte van de tekst. Voor langere teksten kan het enkele seconden duren. Nadat de bewerking handgeschreven tekst herkennen is voltooid, moet u daarom mogelijk wachten voordat u de resultaten kunt ophalen met behulp van de bewerking handgeschreven tekst bewerking ophalen.

---

**Vraag**: *Hoe verwerkt de technologie voor handschrift herkenning tekst die is ingevoegd met een caret in het midden van een regel?*

**Antwoord**: Deze tekst wordt geretourneerd als afzonderlijke regel door de bewerking voor handschrift herkenning.

---

**Vraag**: *Hoe worden door de technologie voor handschrift herkenning cross-out woorden of lijnen afgehandeld?*

**Antwoord**: Als de woorden worden gepasseerd met meerdere regels om ze onherkenbaar weer te geven, worden ze niet door de bewerking voor handschrift herkenning gekozen. Als de woorden echter worden gepasseerd met behulp van één regel, wordt die kruising beschouwd als ruis en de woorden worden nog steeds opgehaald door de bewerking voor handschrift herkenning.

---

**Vraag**: *Welke tekst standen worden ondersteund voor de technologie voor handschrift herkenning?*

**Antwoord**: Tekst gericht op hoeken van Maxi maal 30 graden tot 40 graden kan worden opgehaald door de bewerking voor handschrift herkenning.

---
