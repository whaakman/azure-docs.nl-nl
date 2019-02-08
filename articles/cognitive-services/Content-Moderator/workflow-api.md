---
title: Beheer van werkstromen - Content Moderator
titlesuffix: Azure Cognitive Services
description: Werkstromen met bewerkingen van de taak van de beoordeling-API gebruiken voor het automatiseren van human-in-the-loop beoordelingen op basis van uw inhoud en drempelwaarden.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 56b3bffc7aca1ace4dfae78cdcd4394daae9360e
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55872791"
---
# <a name="automate-moderation-reviews-with-workflows"></a>Toezicht op beoordelingen met werkstromen automatiseren

Content Moderator bevat hulpprogramma's en API's voor het beheren van werkstromen. Gebruik van werkstromen met de [taakbewerkingen bekijken API's](review-api.md) om human-in-the-loop beoordeling maken op basis van uw inhoud beleid en de drempelwaarden te automatiseren.

De beoordeling-API biedt de volgende manieren op te nemen van menselijk toezicht in uw inhoudstoezicht-proces:

1. De **taak** bewerkingen voor het starten van computer-ondersteund toezicht en human bekijken maken als één stap.
1. De **bekijken** bewerkingen voor human maken buiten de stap toezicht bekijken.
1. De **werkstroom** bewerkingen voor het beheren van werkstromen automatiseren van scannen met drempelwaarden voor het maken bekijken.

In dit artikel bevat informatie over de **werkstroom** bewerkingen. Lees de [taken en beoordeelt](review-api.md) overzicht voor meer informatie over inhoudstoezicht toegangsbeoordelingen en taken.

Controleren van de **standaard** werkstroom is de beste manier om aan de slag met informatie over werkstromen in de Content Moderator.

## <a name="your-first-workflow"></a>Uw eerste werkstroom

Uw eerste werkstroom wordt geleverd met uw [bekijken hulpprogramma team](https://contentmoderator.cognitive.microsoft.com/). Meld u als u dit nog niet hebt gedaan.

Navigeer naar de [bekijken van de tool-werkstromen](Review-Tool-User-Guide/Workflows.md) scherm onder het tabblad instellingen. U ziet een **standaard** werkstroom zoals wordt weergegeven in de volgende afbeelding:

![Content Moderator-werkstromen](Review-Tool-User-Guide/images/2-workflows-1.png)

### <a name="open-the-default-workflow"></a>Open de standaardwerkstroom

Gebruik de **bewerken** optie voor het openen van de werkstroom pagina bewerkt, zoals wordt weergegeven in de volgende afbeelding: ![Content Moderator standaardwerkstroom](images/default-workflow-listed.PNG)

### <a name="the-designer-view"></a>De designer-weergave

U ziet de **Designer** tabblad voor de werkstroom. De ontwerpfunctie weergave bevat de volgende stappen uit:

1. De **voorwaarde** voor de werkstroom moet worden geëvalueerd. In dit geval wordt de werkstroom aanroepen de Content Moderator van API en controles of afbeelding de `isAdult` uitvoer is gelijk aan `true`.
1. De **actie** moeten worden uitgevoerd als de voorwaarde wordt voldaan. In dit geval wordt de werkstroom maakt een beoordeling in het beoordelingsprogramma als de `isAdult` uitvoer is `true`.

![Content Moderator standaardwerkstroom - ontwerpfunctie](images/default-workflow-designer.png)

### <a name="the-json-view"></a>De JSON-weergave

Selecteer de **JSON** tabblad om te bekijken van de JSON-definitie van de werkstroom.

    {
        "Type": "Logic",
        If": {
            "ConnectorName": "moderator",
            "OutputName": "isAdult",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition"
        },
        "Then": {
        "Perform": [
        {
            "Name": "createreview",
            "CallbackEndpoint": null,
            "Tags": []
        }
        ],
        "Type": "Actions"
        }
    }

### <a name="key-learning"></a>Belangrijkste learning

De werkstromen in de Content Moderator zijn eenvoudig te configureren en flexibel. Als de ingebouwde ontwerpfunctie voldoet niet aan uw vereisten, schrijft u de definitie van de werkstroom in de **JSON** indeling. Gebruik vervolgens de JSON-definitie met de [werkstroom API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) maken en beheren van de werkstroom van uw toepassing.

## <a name="define-a-custom-workflow"></a>Een aangepaste werkstromen worden gedefinieerd

Dankzij Content Moderator kunnen definiëren en gebruiken van aangepaste werkstromen. Gebruik de [tool-werkstromen procedures bekijken](Review-Tool-User-Guide/Workflows.md) artikel om een aangepaste werkstroom te definiëren. Content Moderator OCR mogelijkheid deze werkstroom gebruikt om tekst te extraheren uit een voorbeeldafbeelding. Vervolgens wordt een beoordeling in het controlehulpprogramma gemaakt.

### <a name="the-sample-image"></a>De installatiekopie van het voorbeeld

Sla de [voorbeeldafbeelding](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) naar uw lokale schijf. Deze installatiekopie moet u voor uw oefening.

### <a name="the-designer-view"></a>De designer-weergave

Selecteer de **Designer** tabblad en de [werkstroom maken zelfstudie](Review-Tool-User-Guide/Workflows.md) om een aangepaste werkstroom te definiëren. De volgende afbeelding ziet u van de ontwerpfunctie **voorwaarde** weergeven. Raadpleeg de zelfstudie om te zien van de rest van de stappen.

![Content Moderator - werkstroom voorwaarde](Review-Tool-User-Guide/images/ocr-workflow-step-2-condition.PNG)

### <a name="the-json-view"></a>De JSON-weergave

Selecteer de **JSON** tabblad om te bekijken van de volgende JSON-definitie van uw aangepaste werkstroom. U ziet hoe de **als dan** instructies in de JSON-definitie komen overeen met de stappen die u hebt gedefinieerd met behulp van de ontwerpweergave.

    {
        "Type": "Logic",
        "If": {
            "ConnectorName": "moderator",
            "OutputName": "hasText",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition"
        },
        "Then": {
        "Perform": [
        {
            "Name": "createreview",
            "CallbackEndpoint": null,
            "Tags": [
            {
                "Tag": "a",
                "IfCondition": {
                    "ConnectorName": "moderator",
                    "OutputName": "hasText",
                    "Operator": "eq",
                    "Value": "true",
                    "Type": "Condition"
                }
            }
            ]
        }
        ],
        "Type": "Actions"
        }
    }

### <a name="workflow-result"></a>Resultaat werkstroom

Nadat u de werkstroom van het scherm werkstromen hebt getest, wordt de volgende controle wordt gemaakt. Navigeer naar de **installatiekopie** tabblad onder **bekijken** om te zien van uw beoordeling.
De werkstroom gemaakt, de beoordeling omdat de primaire voorwaarde getest op de aanwezigheid van tekst positief. De beoordeling gemarkeerd, zodat u de **`a`** tag op in de evaluatie van de installatiekopie.

![Content Moderator - eenvoudige werkstroom-uitvoer](images/ocr-sample-image-workflow1.PNG)


## <a name="advanced-workflow-with-combination"></a>Geavanceerde werkstroom met combinatie

### <a name="the-sample-image"></a>De installatiekopie van het voorbeeld

Gebruik dezelfde [voorbeeldafbeelding](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) die is gebruikt in de vorige sectie.

Dit keer ongeveer wijzigen echter uw primaire voorwaarde in een combinatie van twee controles. Behalve dat wordt gecontroleerd voor tekst, Controleer of de tekst er ongepast taalgebruik heeft. De werkstroom een beoordeling wordt gemaakt als deze tekst wordt gevonden met **en** grof taalgebruik gedetecteerd in het.

### <a name="the-designer-view"></a>De designer-weergave

Wijzigen van de **voorwaarde** naar een **combinatie**, de werkstroom wijzigen. De volgende afbeelding toont de nieuwe weergave u ziet in de ontwerpfunctie.

![Content Moderator - gewijzigde werkstroom voorwaarde](images/ocr-workflow-2-designer.PNG)

### <a name="the-json-view"></a>De JSON-weergave

Selecteer de **JSON** tabblad om te bekijken van de volgende JSON-definitie van de gewijzigde, aangepaste werkstroom. U ziet hoe de **als dan** instructies in de JSON-definitie komen overeen met de nieuwe stappen die u hebt toegevoegd aan de werkstroom.

    {
        "Type": "Logic",
        "If": {
        "Left": {
            "ConnectorName": "moderator",
            "OutputName": "hasText",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition"
            },
        "Right": {
            "ConnectorName": "moderator",
            "OutputName": "text.HasProfanity",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition",
            "AlternateInput": "moderator.ocrText"
            },
        "Combine": "AND",
        "Type": "Combine"
        },
        "Then": {
        "Perform": [
        {
            "Name": "createreview",
            "CallbackEndpoint": null,
            "Tags": [
            {
                "Tag": "a",
                "IfCondition": {
                    "ConnectorName": "moderator",
                    "OutputName": "hasText",
                    "Operator": "eq",
                    "Value": "true",
                    "Type": "Condition"
                }
            }
            ]
        }
        ],
        "Type": "Actions"
        }
    }

    
### <a name="workflow-result"></a>Resultaat werkstroom

Nadat u de werkstroom opnieuw hebt getest, kunt u vinden dat er geen beoordeling wordt gemaakt. Als u wilt controleren of het ontbreken van een beoordeling, gaat u naar de **installatiekopie** tabblad onder **bekijken**.
De werkstroom is de revisie niet maken omdat er geen Detecteer grof taalgebruik in de geëxtraheerde tekst.

![Content Moderator - gewijzigde werkstroomuitvoer](images/ocr-workflow-2-result.PNG)


## <a name="the-workflow-api"></a>De werkstroom API

De [werkstroombewerkingen](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) bieden de programmeerinterface tot de werkstroommogelijkheden. U werkstromen maken, Werkstroomdetails van ophalen en bijwerken van werkstroomdefinities van de met behulp van de werkstroom-API.

### <a name="get-all-workflow-details"></a>Details van de werkstroom opvragen [All]

De **werkstroom Get** bewerking accepteert de invoer van de volgende:

- **team**: Het team-ID die u hebt gemaakt bij het instellen van uw [hulpprogramma account controleren](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: De naam van uw werkstroom. Gebruik `default` begint met.
- **Ocp-Apim-Subscription-Key**: Zich op de **instellingen** tabblad. Zie [Overzicht](overview.md) voor meer informatie.

Als de bewerking is geslaagd, de **antwoordstatus** is `200 OK` en de **antwoordinhoud** in het definitie van de werkstroom weergegeven in de JSON-indeling.
Lees voor meer informatie de [werkstroom API-console snelstartgids](try-review-api-job.md).

### <a name="create-or-update-workflow"></a>Maken of bijwerken van de werkstroom

De bewerking voor het maken en bijwerken kunt het maken van de werkstroom van de API.

De **werkstroom maken of bijwerken** bewerking accepteert de invoer van de volgende:

- **team**: Het team-ID die u hebt gemaakt bij het instellen van uw [hulpprogramma account controleren](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: De naam van uw werkstroom. Gebruik `default` begint met.
- **Ocp-Apim-Subscription-Key**: Zich op de **instellingen** tabblad. Zie [Overzicht](overview.md) voor meer informatie.

Als de bewerking is geslaagd, de **antwoordstatus** is `200 OK` en de **antwoordinhoud** bevat `true`. Voor meer informatie, [testen de `Create` bewerking](try-review-api-job.md).

## <a name="next-steps"></a>Volgende stappen

Bekijk voor meer informatie over het maken van aangepaste werkstromen, de [bekijken van het hulpprogramma werkstroom zelfstudie](Review-Tool-User-Guide/Workflows.md). 

U kunt uitproberen de [werkstroom API-console](try-review-api-job.md) en gebruiken van de REST-API-codevoorbeelden. 

Gebruik ten slotte uw aangepaste werkstromen met de **taak** bewerkingen uitvoeren als shon in [taak API-console](try-review-api-job.md) en de [Snelstartgids voor .NET-taken](moderation-jobs-quickstart-dotnet.md).
