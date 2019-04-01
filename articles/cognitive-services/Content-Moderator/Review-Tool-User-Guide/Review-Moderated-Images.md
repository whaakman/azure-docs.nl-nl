---
title: Beoordelingen van inhoud via het beoordelingsprogramma - Content Moderator gebruiken
titlesuffix: Azure Cognitive Services
description: Meer informatie over hoe het beoordelingsprogramma kunt menselijke moderators om te controleren van afbeeldingen in een webportal.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 03/15/2019
ms.author: sajagtap
ms.openlocfilehash: a482ecf4a0d321525ab7e392695d2c4c0eebeadc
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758491"
---
# <a name="create-human-reviews"></a>Onlinebeoordelingen door mensen maken

In deze handleiding leert u hoe u voor het instellen van [beoordeelt](../review-api.md#reviews) op de website van het hulpprogramma voor beoordeling. Beoordelingen opslaan en weergeven van inhoud voor menselijke moderators om vast te stellen. Moderators kunnen de toegepaste labels wijzigen en hun eigen aangepaste tags waar nodig toepassen. Wanneer een gebruiker is een beoordeling voltooid, de resultaten worden verzonden naar een opgegeven retouraanroep-eindpunt en de inhoud van de site wordt verwijderd.

## <a name="prerequisites"></a>Vereisten

- Meld u aan of maak een account op de Content Moderator [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com/) site.

## <a name="image-reviews"></a>Beoordelingen van afbeeldingen

1. Ga naar de [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com/), selecteer de **probeer** tabblad, en het uploaden van enkele afbeeldingen om te controleren.
1. Zodra de geüploade afbeeldingen zijn verwerkt, gaat u naar de **revisie** tabblad en selecteer **installatiekopie**.

    ![Chrome-browser het beoordelingsprogramma waarin wordt weergegeven met de installatiekopie controleren-optie is gemarkeerd](images/review-images-1.png)

    De afbeeldingen weergegeven met de labels die zijn toegewezen door het toezichtproces van automatische. De installatiekopieën die u hebt verzonden via het beoordelingsprogramma zijn niet zichtbaar voor andere revisoren.

1. (Optioneel) Verplaats de **weer te geven beoordelingen** schuifregelaar (1) naar het aantal afbeeldingen die worden weergegeven op het scherm. Klik op de **gelabelde** of **niet-gecodeerde** knoppen (2) de afbeeldingen dienovereenkomstig te sorteren. Klik op een label in het venster (3) om uit te schakelen het in- of uitschakelen.

    ![Het beoordelingsprogramma met gelabelde afbeeldingen voor controle met chrome-browser](images/review-images-2.png)

1. Als u meer informatie over een afbeelding, klik op het beletselteken in de miniatuur en selecteer **details weergeven**. U kunt een installatiekopie van een toewijzen aan een subteams bepalen met de **verplaatsen naar** optie (Zie de [teams](./configure.md#manage-team-and-subteams) sectie voor meer informatie over subteams).

    ![Een afbeelding met de weergave details-optie is gemarkeerd](images/review-images-3.png)

1. Blader door de informatie over de controle van afbeelding op de pagina met details.

    ![Een afbeelding met beheer van gegevens die worden vermeld in een apart venster](images/review-images-4.png)

1. Nadat u hebt bekeken en zo nodig de tag-toewijzingen bijgewerkt, klikt u op **volgende** om in te dienen uw beoordelingen. Nadat u hebt ingediend, hebt u ongeveer vijf seconden klikt u op de **vorig** knop terug naar het vorige scherm en afbeeldingen opnieuw beoordelen. Hierna de afbeeldingen zijn niet meer in de wachtrij verzenden en de **vorig** knop is niet meer beschikbaar.

## <a name="text-reviews"></a>Beoordelingen van teksten

Tekst controleert functie op soortgelijke wijze aan beoordelingen van afbeeldingen. In plaats van inhoud te uploaden, u gewoon schrijven of plak tekst (maximaal 1024 tekens). Vervolgens Content Moderator analyseert de tekst en tags (naast andere beheer-informatie, zoals grof taalgebruik en persoonlijke gegevens) van toepassing is. In beoordelingen van teksten, u kunt de toegepaste labels in-/ uitschakelen en/of aangepaste labels toevoegen voordat u de beoordeling verzendt.

![Schermafbeelding van de beoordeling hulpprogramma weergegeven tekst in een Chrome-browservenster gemarkeerd](../images/reviewresults_text.png)

## <a name="next-steps"></a>Volgende stappen

In deze handleiding, hebt u geleerd hoe u kunt instellen en gebruiken van beoordelingen van de Content Moderator [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com). Hieronder staan de [REST-API-handleiding](../try-review-api-review.md) of de [.NET SDK-handleiding](../moderation-reviews-quickstart-dotnet.md) voor meer informatie over het maken van beoordelingen via een programma.