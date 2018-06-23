---
title: Azure Content beheerder - interrupts werkstromen | Microsoft Docs
description: Werkstromen met inhoud toezicht gebruiken.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 02/04/2018
ms.author: sajagtap
ms.openlocfilehash: 079fcd119f1536f9e76ca57fccc76538b3c3ed78
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344568"
---
# <a name="moderation-workflows"></a>Controle-werkstromen

Inhoud beheerder bevat hulpprogramma's en API's voor het beheren van werkstromen. Gebruik van werkstromen met de [taakbewerkingen bekijken van de API](review-api.md) om human in de lus revisie maken op basis van uw inhoud beleid en de drempelwaarden te automatiseren.

De controle-API biedt de volgende manieren menselijke toezicht opnemen in uw inhoud toezichtproces:

1. De **taak** bewerkingen voor het starten van de machine-ondersteunde wijzigen en human bekijken maken als één stap.
1. De **bekijken** bewerkingen voor human buiten de controle-stap het maken, bekijken.
1. De **werkstroom** bewerkingen voor het beheren van werkstromen automatiseren scannen met drempelwaarden voor het maken van bekijken.

In dit artikel bevat informatie over de **werkstroom** bewerkingen. Lees de [taken en beoordeelt](review-api.md) overzicht voor meer informatie over inhoud toezicht taken en bekijkt.

Uitchecken van het **standaard** werkstroom is de beste manier aan de slag op informatie over werkstromen in inhoud beheerder.

## <a name="your-first-workflow"></a>Uw eerste werkstroom

Uw eerste werkstroom wordt geleverd met uw [hulpprogramma team controleren](https://contentmoderator.cognitive.microsoft.com/). Meld u als u dit nog niet hebt gedaan.

Navigeer naar de [bekijken van tool-werkstromen](Review-Tool-User-Guide/Workflows.md) scherm op het tabblad instellingen. U ziet een **standaard** werkstroom zoals weergegeven in de volgende afbeelding:

![Inhoud beheerder werkstromen](Review-Tool-User-Guide/images/2-workflows-1.png)

### <a name="open-the-default-workflow"></a>De standaardwerkstroom openen

Gebruik de **bewerken** optie voor het openen van de werkstroom pagina bewerken, zoals wordt weergegeven in de volgende afbeelding: ![inhoud beheerder standaardwerkstroom](images/default-workflow-listed.PNG)

### <a name="the-designer-view"></a>De ontwerpfunctie weergave

U ziet de **Designer** tabblad voor de werkstroom. De ontwerpfunctie weergave bevat de volgende stappen uit:

1. De **voorwaarde** voor de werkstroom moet worden geëvalueerd. In dit geval wordt de werkstroom oproepen de beheerder van de inhoud van een installatiekopie API en wordt gecontroleerd of de `isAdult` gelijk is aan de uitvoer `true`.
1. De **actie** moeten worden uitgevoerd als de voorwaarde wordt voldaan. In dit geval wordt de werkstroom maakt een beoordeling in het hulpprogramma controleren als de `isAdult` uitvoer is `true`.

![Inhoud beheerder standaardworkflow - ontwerpfunctie](images/default-workflow-designer.png)

### <a name="the-json-view"></a>De JSON-weergave

Selecteer de **JSON** tabblad voor een overzicht van de JSON-definitie van de werkstroom.

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

### <a name="key-learning"></a>Sleutel learning

De werkstromen in inhoud beheerder zijn eenvoudig te configureren en flexibele. Als de ingebouwde designer voldoet niet aan uw vereisten, de werkstroomdefinitie schrijven in de **JSON** indeling. Gebruik vervolgens de JSON-definitie met de [werkstroom API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) maken en beheren van de werkstroom van uw toepassing.

## <a name="define-a-custom-workflow"></a>Een aangepaste werkstroom definiëren

Inhoud beheerder werkstroommogelijkheden toestaan definiëren en gebruik van aangepaste werkstromen. Gebruik de [tool-werkstromen how-to bekijken](Review-Tool-User-Guide/Workflows.md) artikel voor het definiëren van een aangepaste werkstroom. Deze werkstroom gebruikt inhoud beheerder OCR mogelijkheid voor tekst ophalen uit een voorbeeld van afbeelding. Vervolgens maakt u een beoordeling in het hulpprogramma voor beoordeling.

### <a name="the-sample-image"></a>De installatiekopie van het voorbeeld

Sla de [voorbeeldafbeelding](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) naar uw lokale schijf. Deze installatiekopie moet u voor uw oefening.

### <a name="the-designer-view"></a>De ontwerpfunctie weergave

Selecteer de **Designer** tabblad en de [werkstroom maken-zelfstudie](Review-Tool-User-Guide/Workflows.md) voor het definiëren van een aangepaste werkstroom. De volgende afbeelding toont de ontwerpfunctie **voorwaarde** weergeven. Raadpleeg de handleiding voor een overzicht van de rest van de stappen.

![Inhoud beheerder - werkstroom voorwaarde](Review-Tool-User-Guide/images/ocr-workflow-step-2-condition.PNG)

### <a name="the-json-view"></a>De JSON-weergave

Selecteer de **JSON** tabblad voor een overzicht van de volgende JSON-definitie van uw aangepaste werkstroom. U ziet hoe de **als dan** instructies in de JSON-definitie overeenkomen met de stappen die u hebt gedefinieerd met behulp van de ontwerpfunctie weergave.

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

### <a name="workflow-result"></a>Resultaat van de werkstroom

Nadat u de werkstroom vanaf het scherm werkstromen hebt getest, wordt de volgende evaluatie gemaakt. Navigeer naar de **installatiekopie** tabblad onder **bekijken** om te zien van uw beoordeling.
De werkstroom gemaakt, de controle omdat de primaire voorwaarde getest positief op de aanwezigheid van tekst. De evaluatie gemarkeerd, zodat u de **`a`** tag op in de evaluatie van de installatiekopie.

![Inhoud beheerder - eenvoudige werkstroom-uitvoer](images/ocr-sample-image-workflow1.PNG)


## <a name="advanced-workflow-with-combination"></a>Geavanceerde werkstroom met een combinatie

### <a name="the-sample-image"></a>De installatiekopie van het voorbeeld

Gebruik van dezelfde [voorbeeldafbeelding](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) die is gebruikt in de vorige sectie.

Dit keer ongeveer wijzigen echter uw primaire voorwaarde in een combinatie van twee controles. Naast het controleren van tekst, Controleer of de tekst eventuele taalgebruik heeft. De werkstroom maakt u een overzicht als tekst wordt gevonden **en** taalgebruik in het detecteert.

### <a name="the-designer-view"></a>De ontwerpfunctie weergave

Wijzigen van de **voorwaarde** naar een **combinatie**, de werkstroom wijzigen. De volgende afbeelding toont de nieuwe weergave u ziet in de ontwerpfunctie.

![Inhoud beheerder - gewijzigde werkstroom voorwaarde](images/ocr-workflow-2-designer.PNG)

### <a name="the-json-view"></a>De JSON-weergave

Selecteer de **JSON** tabblad voor een overzicht van de volgende JSON-definitie van de gewijzigde, aangepaste werkstroom. U ziet hoe de **als dan** instructies in de JSON-definitie overeenkomen met de nieuwe stappen die u hebt toegevoegd aan de werkstroom.

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

    
### <a name="workflow-result"></a>Resultaat van de werkstroom

Nadat u de werkstroom opnieuw hebt getest, kunt u vinden dat geen revisie is gemaakt. Om te bevestigen gebrek aan herzieningen, gaat u naar de **installatiekopie** tabblad onder **controleren**.
De werkstroom heeft de controle niet maken omdat deze niet taalgebruik gedetecteerd in de uitgepakte tekst.

![Inhoud beheerder - gewijzigde werkstroom-uitvoer](images/ocr-workflow-2-result.PNG)


## <a name="the-workflow-api"></a>De Workflow-API

De [werkstroombewerkingen](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) de programmeerinterface tot de workflow-mogelijkheden bieden. U werkstromen maken, Werkstroomdetails ophalen en bijwerken van werkstroomdefinities met behulp van de API van de werkstroom.

### <a name="get-all-workflow-details"></a>Details van de workflow GET [All]

De **werkstroom Get-** bewerking accepteert de volgende invoer:

- **team**: de team-ID die u hebt gemaakt bij het instellen van uw [hulpprogramma account controleren](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: de naam van uw werkstroom. Gebruik `default` beginnen.
- **OCP-Apim-Subscription-Key**: zich op de **instellingen** tabblad. Zie voor meer informatie [overzicht](overview.md).

Als de bewerking is geslaagd, de **antwoordstatus** is `200 OK` en de **antwoordinhoud** vak de werkstroomdefinitie wordt weergegeven in de JSON-indeling.
Lees voor meer informatie de [werkstroom API console Quick Start](try-review-api-job.md).

### <a name="create-or-update-workflow"></a>Maken of bijwerken van de werkstroom

Het maken en de update opnieuw kunt maken van de werkstroom van de API.

De **werkstroom maken of bijwerken** bewerking accepteert de volgende invoer:

- **team**: de team-ID die u hebt gemaakt bij het instellen van uw [hulpprogramma account controleren](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: de naam van uw werkstroom. Gebruik `default` beginnen.
- **OCP-Apim-Subscription-Key**: zich op de **instellingen** tabblad. Zie voor meer informatie [overzicht](overview.md).

Als de bewerking is geslaagd, de **antwoordstatus** is `200 OK` en de **antwoordinhoud** bevat `true`. Voor meer informatie, [uitprobeert de `Create` bewerking](try-review-api-job.md).

## <a name="next-steps"></a>Volgende stappen

Bekijk voor meer informatie over het maken van aangepaste werkstromen, de [bekijken van het hulpprogramma werkstroom zelfstudie](Review-Tool-User-Guide/Workflows.md). 

Uitprobeert de [werkstroom API console](try-review-api-job.md) en de REST-API-codevoorbeelden gebruiken. 

Gebruik tot slot uw aangepaste werkstromen met de **taak** bewerkingen shon in [taak API console](try-review-api-job.md) en de [taken .NET Quick Start](moderation-jobs-quickstart-dotnet.md).
