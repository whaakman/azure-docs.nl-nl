---
title: Beoordelingen, werkstromen, en taken concepten - Content Moderator
titlesuffix: Azure Cognitive Services
description: Meer informatie over beoordelingen, werkstromen en taken
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: sajagtap
ms.openlocfilehash: c1d4ef640e2ae072dacba7a665b6689e3224c55c
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756299"
---
# <a name="content-moderation-reviews-workflows-and-jobs"></a>Beoordelingen voor inhoudstoezicht, werkstromen en taken

Content Moderator combineert geautomatiseerd beheer met human-in-the-loop mogelijkheden voor het maken van een optimale toezichtproces voor echte scenario's. Dit gebeurt via de cloud-gebaseerde [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com). In deze handleiding leert u over de basisconcepten van het beoordelingsprogramma: beoordelingen, werkstromen en taken.

## <a name="reviews"></a>Beoordelingen

In een beoordeling inhoud naar het beoordelingsprogramma is geüpload en wordt weergegeven onder de **bekijken** tabblad. Gebruikers kunnen hier de toegepaste labels wijzigen en hun eigen aangepaste tags waar nodig toepassen. Wanneer een gebruiker een beoordeling indient, de resultaten worden verzonden naar een opgegeven retouraanroep-eindpunt en de inhoud van de site wordt verwijderd.

![Beoordeling hulpprogramma website kan worden geopend in een browser op het tabblad controleren](./Review-Tool-user-Guide/images/image-workflow-review.png)

Zie de [Review hulpprogramma guide](./review-tool-user-guide/review-moderated-images.md) beoordelingen maken of Raadpleeg de [REST-API-handleiding](./try-review-api-review.md) voor informatie over het programmatisch doen.

## <a name="workflows"></a>Workflows

Een werkstroom is een cloud-gebaseerde aangepaste filter voor inhoud. Werkstromen kunnen verbinding maken met tal van services voor het filteren van inhoud op verschillende manieren en vervolgens de juiste actie ondernemen. Met de Content Moderator-connector, een werkstroom automatisch beheer van tags toepassen en beoordelingen met ingediende inhoud maken.

### <a name="view-workflows"></a>Werkstromen weergeven

Als u uw bestaande werkstromen, gaat u naar de [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com/) en selecteer **instellingen** > **werkstromen**.

![Standaardwerkstroom](images/default-workflow-listed.PNG)

Werkstromen kunnen volledig worden beschreven als JSON-tekenreeksen, waardoor ze toegankelijk zijn via een programma. Als u selecteert de **bewerken** optie voor uw werkstroom en selecteer vervolgens de **JSON** tabblad ziet u een JSON-expressie als volgt uit:

```json
{
    "Type": "Logic",
    "If": {
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
```

Zie de [Review hulpprogramma guide](./review-tool-user-guide/workflows.md) aan de slag maken en gebruiken van werkstromen, of Raadpleeg de [REST-API-handleiding](./try-review-api-workflow.md) voor informatie over het programmatisch doen.

## <a name="jobs"></a>Taken

Een taak toezicht fungeert als een soort-wrapper voor de functionaliteit van inhoudstoezicht, werkstromen en recensies. De taak scant uw inhoud met behulp van de Content Moderator afbeeldingstoezicht-API of tekst toezicht-API en vervolgens gecontroleerd op basis van de aangewezen werkstroom. Op basis van de resultaten van de werkstroom, mogelijk is of niet kunnen maken van een beoordeling voor de inhoud van de [beoordelingsprogramma](./review-tool-user-guide/human-in-the-loop.md). Terwijl zowel beoordelingen en werkstromen kunnen worden gemaakt en geconfigureerd met hun respectieve API's, wordt de taak API kunt u een gedetailleerd rapport van het hele proces (die kan worden verzonden naar een eindpunt opgegeven callback) verkrijgen.

Zie de [REST-API-handleiding](./try-review-api-job.md) aan de slag met behulp van taken.

## <a name="next-steps"></a>Volgende stappen

* U kunt uitproberen de [taak API-console](try-review-api-job.md), en gebruikt u de REST-API-codevoorbeelden. Als u bekend met Visual Studio en C# bent, Lees ook de [Snelstartgids voor .NET-taken](moderation-jobs-quickstart-dotnet.md). 
* Voor beoordelingen, aan de slag met de [revisie API-console](try-review-api-review.md), en gebruikt u de REST-API-codevoorbeelden. Vervolgens ziet de [Snelstartgids voor .NET beoordelingen](moderation-reviews-quickstart-dotnet.md).
* Voor video beoordelingen, gebruikt u de [Video revisie snelstartgids](video-reviews-quickstart-dotnet.md), en leer hoe u [Transcripten toevoegen aan de video beoordeling](video-transcript-reviews-quickstart-dotnet.md).
