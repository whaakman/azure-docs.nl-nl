---
title: Taalondersteuning - Computer Vision
titleSuffix: Azure Cognitive Services
description: Een lijst van natuurlijke talen die worden ondersteund door de Computer Vision-functies.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: article
ms.date: 04/17/2019
ms.author: pafarley
ms.openlocfilehash: 1a70d1b2ea504d0ccfba925810a2d19d0c7583cc
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "60012645"
---
# <a name="language-support-for-computer-vision"></a>Taalondersteuning voor Computer Vision

Sommige functies van de Computer Vision ondersteuning voor meerdere talen. Engels wordt alleen ondersteund door een van de functies die hier niet worden vermeld.

## <a name="text-recognition"></a>Tekstherkenning

Computer Vision kan herkennen tekst in vele talen. Met name de [OCR](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) API ondersteunt een groot aantal talen, terwijl de [lezen](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/2afb498089f74080d7ef85eb) API en [tekst herkennen](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) API ondersteunt alleen Engels. Zie [gedrukte en handgeschreven tekstherkenning](concept-recognizing-text.md) voor meer informatie over deze functionaliteit en de voordelen van elke API.

OCR detecteert automatisch de taal van de invoer-materiaal, zodat er geen nodig om op te geven van een taalcode die in de API-aanroep is. Taalcodes worden echter altijd geretourneerd als de waarde van de `"language"` knooppunt in het JSON-antwoord.

|Taal| Taalcode | OCR-API |
|:-----|:----:|:-----:|
|Arabisch | `ar`|✔ |
|Chinees (vereenvoudigd) | `zh-Hans`|✔ |
|Chinees (traditioneel) | `zh-Hant`|✔ |
|Tsjechisch | `cs` |✔ |
|Deens | `da` |✔ |
|Nederlands | `nl` |✔ |
|Engels | `en` |✔ |
|Fins | `fi` |✔ |
|Frans | `fr` |✔ |
|Duits | `de` |✔ |
|Grieks | `el` |✔ |
|Hongaars | `hu` |✔ |
|Italiaans | `it` |✔ |
|Japans | `ja` |✔ |
|Koreaans | `ko` |✔ |
|Noors | `nb` |✔ |
|Pools | `pl` |✔ |
|Portugees | `pt` |✔ |
|Roemeens | `ro` |✔ |
|Russisch | `ru` |✔ |
|Servisch (Cyrillisch) | `sr-Cyrl` |✔ |
|Servisch (Latijns) | `sr-Latn` |✔ |
|Slowaaks | `sk` |✔ |
|Spaans | `es` |✔ |
|Zweeds | `sw` |✔ |
|Turks | `tr` |✔ |

## <a name="image-analysis"></a>Analyse van de afbeelding

Sommige acties van de [analyseren - Image](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API kan resultaten retourneren in andere talen wordt opgegeven met de `language` queryparameter. Andere acties retourneren resultaten in het Engels, ongeacht welke taal is opgegeven en anderen genereert een uitzondering voor niet-ondersteunde talen. Acties zijn opgegeven met de `visualFeatures` en `details` queryparameters; Zie de [overzicht](home.md) voor een lijst van alle acties die u met de analyse van de afbeelding doen kunt.

|Taal | Taalcode | Categories | Tags | Description | Volwassene | Merken | Kleur | Gezichten | ImageType | Objecten | Beroemdheden | Oriëntatiepunten |
|:---|:---:|:----:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Chinees | `zh`    | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|Engels | `en`   | ✔ | ✔| ✔|✔|✔|✔|✔|✔|✔|✔|✔|
|Japans | `ja`   | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|Portugees | `pt` | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|Spaans | `es`    | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|

## <a name="next-steps"></a>Volgende stappen

Aan de slag met behulp van de Computer Vision-functies die worden vermeld in deze handleiding.

* [Analyseren van een lokale installatiekopie (REST)](./quickstarts/csharp-analyze.md)
* [Haal de gedrukte tekst (REST)](./quickstarts/csharp-print-text.md)