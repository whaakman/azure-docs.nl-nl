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
ms.openlocfilehash: ad9bba53390e3c4262f999ebcc57ab354f1e3d69
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67537623"
---
# <a name="build-a-training-data-set-for-a-custom-model"></a>Een set trainingsgegevens voor een aangepast model bouwen

Wanneer u het aangepaste formulier herkenning-model, Geef uw eigen trainingsgegevens, zodat het model voor uw branche-specifieke formulieren trainen kunt. U kunt een model trainen met vijf ingevuld in formulieren of een leeg formulier (u moet het woord 'leeg' in de bestandsnaam opnemen) plus twee ingevuld in formulieren. Zelfs als er voldoende formulieren ingevuld met de trein, kan een leeg formulier toe te voegen aan uw set trainingsgegevens de nauwkeurigheid van het model verbeteren.

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

## <a name="upload-your-training-data"></a>Uw trainingsgegevens uploaden

Wanneer u hebt geplaatst samen de set van formulier-documenten die u voor training gebruiken gaat, moet u dit uploaden naar een Azure blob storage-container. Als u niet hoe u een Azure storage-account maakt met een container weet, volgt u de [Azure Storage-quickstart voor Azure-portal](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal).

### <a name="organize-your-data-in-subfolders-optional"></a>Uw gegevens indelen in submappen (optioneel)

Standaard de [Train Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/TrainCustomModel) API wordt alleen gebruik van formulier-documenten die zich bevinden in de hoofdmap van uw storage-container. U kunt echter trainen met gegevens in submappen als u het opgeeft in de API-aanroep. Normaal gesproken de hoofdtekst van de [Train Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/TrainCustomModel) aanroep heeft de volgende notatie, waarbij `<SAS URL>` is de Shared access signature-URL van de container:

```json
{
  "source":"<SAS URL>"
}
```

Als u de volgende inhoud aan de hoofdtekst van de aanvraag toevoegen, wordt de API trainen met documenten in de submappen. De `"prefix"` veld is optioneel en wordt de set trainingsgegevens op bestanden waarvan paden met de opgegeven tekenreeks beginnen beperken. Dus een waarde van `"Test"`, bijvoorbeeld zorgt ervoor dat de API om te kijken naar alleen de bestanden of mappen die met het woord 'Test beginnen'.

```json
{
  "source": "<SAS URL>",
  "sourceFilter": {
    "prefix": "<prefix string>",
    "includeSubFolders": true
  }
}
```

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd over het bouwen van een set trainingsgegevens, gaat u als volgt een snelstartgids voor het trainen van een aangepast formulier herkenning-model en gaan gebruiken in uw formulieren.

* [Snelstart: Een model te trainen en ophalen van gegevens met behulp van cURL](./quickstarts/curl-train-extract.md)
* [Snelstart: Een model te trainen en extraheren van gegevens met behulp van de REST-API met Python](./quickstarts/python-train-extract.md)

