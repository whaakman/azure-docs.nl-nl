---
title: Over het bouwen van een set trainingsgegevens voor een aangepast model - formulier-herkenning
titleSuffix: Azure Cognitive Services
description: Leer hoe u om te controleren of uw gegevensset training is geoptimaliseerd voor het trainen van een formulier herkenning-model.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: pafarley
ms.openlocfilehash: 611d5f7983c61fab12c55a46fedf35a3c420c4c8
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454816"
---
# <a name="build-a-training-data-set-for-a-custom-model"></a>Een set trainingsgegevens voor een aangepast model bouwen

Wanneer u het aangepaste formulier herkenning-model, Geef uw eigen trainingsgegevens, zodat het model voor uw branche-specifieke formulieren trainen kunt. U kunt een model trainen met vijf ingevuld in formulieren of een leeg formulier (inclusief het woord 'lege' in de bestandsnaam) plus twee ingevuld in formulieren. Zelfs als er voldoende formulieren ingevuld met de trein, kan een leeg formulier toe te voegen aan uw set trainingsgegevens de nauwkeurigheid van het model verbeteren.

## <a name="training-data-tips"></a>Tips voor training-gegevens

Het is belangrijk om te gebruiken van een gegevensset die geoptimaliseerd voor training. Gebruik de volgende tips om ervoor te zorgen dat u de beste resultaten van krijgt de [Train Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/TrainCustomModel) bewerking:

* Gebruik indien mogelijk, PDF-documenten op basis van tekst in plaats van documenten op basis van een installatiekopie. Gescande PDF-bestanden worden behandeld als afbeeldingen.
* Een leeg formulier en twee ingevuld in formulieren gebruiken als u deze beschikbaar hebt.
* Gebruik voor ingevuld in formulieren voorbeelden waarvan alle bijbehorende velden ingevuld.
* Gebruik formulieren met verschillende waarden in elk veld.
* Als uw formulier installatiekopieën van een lagere kwaliteit, kunt u een grotere set gegevens (bijvoorbeeld 10-15 afbeeldingen) gebruiken.

## <a name="general-input-requirements"></a>Algemene vereisten voor invoer

Zorg ervoor dat uw gegevensset training volgt ook de input vereisten voor alle inhoud van het formulier herkenning.
[!INCLUDE [input requirements](./includes/input-requirements.md)]

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd over het bouwen van een set trainingsgegevens, gaat u als volgt een snelstartgids voor het trainen van een aangepast formulier herkenning-model en gaan gebruiken in uw formulieren.

* [Snelstart: Een model te trainen en ophalen van gegevens met behulp van cURL](./quickstarts/curl-train-extract.md)
* [Snelstart: Een model te trainen en extraheren van gegevens met behulp van de REST-API met Python](./quickstarts/python-train-extract.md)

