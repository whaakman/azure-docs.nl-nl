---
title: Videotoezicht met menselijke beoordeling - Content Moderator
titlesuffix: Azure Cognitive Services
description: Geautomatiseerd beheer van video- en programma's voor menselijke beoordeling gebruiken om te modereren ongepaste inhoud melden
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/20/2018
ms.author: sajagtap
ms.openlocfilehash: 43a43ddcbfc656a3eb5a274e1bb63a473b7c89a2
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55867712"
---
# <a name="video-moderation-with-human-review"></a>Videotoezicht met menselijke beoordeling

Gebruik Content Moderator geautomatiseerd [videotoezicht](video-moderation-api.md) en [hulpprogramma voor menselijke beoordeling](Review-Tool-User-Guide/human-in-the-loop.md) matige video's en Transcripten voor volwassenen (expliciete) en ongepaste inhoud (suggestieve) de beste resultaten voor uw bedrijf.

## <a name="video-trained-classifier-preview"></a>Video-getraind classificatie (preview)

Video classificatie geautomatiseerd wordt ofwel met afbeelding getrainde modellen of video getrainde modellen bereikt. In tegenstelling tot video classificaties getraind met een installatiekopie wordt Microsofts erotische en ongepaste video classificatie getraind met video's. Deze methode resulteert in betere kwaliteit van de overeenkomst.

## <a name="shot-detection"></a>Opnamedetectie

Bij het uitvoeren van de details van de classificatie, kunt u aanvullende video intelligence meer flexibiliteit bij het analyseren van video's. In plaats van alleen de frames die worden uitgevoerd, biedt van Microsoft-service voor videotoezicht shot-niveau te. U hebt nu de optie voor het analyseren van uw video's op het niveau van de schermopname en het niveau van het kader.
 
## <a name="key-frame-detection"></a>Detectie van sleutel

In plaats van frames met regelmatige tussenpozen worden uitgevoerd, wordt de service voor videotoezicht identificeert en mogelijk alleen volledige (goed) frames levert. De functie kunt efficiënt frame genereren voor de analyse erotische en ongepaste frame-niveau.

Een gedeeltelijk antwoord met potentiële foto's, belangrijke frames en erotische en ongepaste scores ziet u het volgende uittreksel:

    "fragments": [
    {
      "start": 0,
      "duration": 18000
    },
    {
      "start": 18000,
      "duration": 3600,
      "interval": 3600,
      "events": [
        [
          {
            "reviewRecommended": false,
            "adultScore": 0.00001,
            "racyScore": 0.03077,
            "index": 5,
            "timestamp": 18000,
            "shotIndex": 0
          }
        ]
      ]
    },
    {
      "start": 18386372,
      "duration": 119149,
      "interval": 119149,
      "events": [
        [
          {
            "reviewRecommended": true,
            "adultScore": 0.00000,
            "racyScore": 0.91902,
            "index": 5085,
            "timestamp": 18386372,
            "shotIndex": 62
          }
        ]
      ]


## <a name="visualization-for-human-reviews"></a>Visualisatie voor onlinebeoordelingen door mensen

Voor meer genuanceerde gevallen bedrijven nodig hebben een oplossing voor menselijke beoordeling voor het renderen van de video, de frames en tags machine toegewezen. De menselijke moderators redigeren van video's en de frames een compleet beeld van de inzichten te verkrijgen, labels wijzigen en het verzenden van hun beslissingen.

![video revisie hulpprogramma standaardweergave](images/video-review-default-view.png)

## <a name="player-view-for-video-level-review"></a>Weergave van de speler voor video-niveau van controle

Video-niveau binaire beslissingen mogelijk is met een videospeler weergave ziet u mogelijk erotische en ongepaste frames. De menselijke revisoren gaat u de video met verschillende snelheid opties voor het onderzoeken van de schermen. Zij bevestigen hun beslissingen door het omschakelen van de labels.

![video revisie hulpprogramma player weergeven](images/video-review-player-view.PNG)

## <a name="frames-view-for-detailed-reviews"></a>Weergave voor gedetailleerde recensies frames

Een gedetailleerde video beoordeling van frame-voor-frame analyse wordt mogelijk is met een weergave op basis van een frame uitgevoerd. De menselijke revisoren bekijken en selecteer een of meer frames en tags om te bevestigen van hun beslissingen in-of uitschakelen. Een optionele volgende stap is onherkenbaar maken van de aanstootgevende frames of de inhoud.

![video revisie hulpprogramma frames weergeven](images/video-review-frames-view-apply-tags.PNG)

## <a name="transcript-moderation"></a>Transcriptie beoordelen

Video's hebben doorgaans voice over die beheer ook nodig voor aanstootgevende spraak heeft. U kunt de service Azure Media Indexer Converteer spraak naar tekst en Content Moderator revisie API gebruiken voor het indienen van het transcript voor beheer van tekst binnen het beoordelingsprogramma.

![video revisie hulpprogramma transcript weergeven](images/video-review-transcript-view.png)

## <a name="next-steps"></a>Volgende stappen

Aan de slag met de [videotoezicht snelstartgids](video-moderation-api.md). 

Meer informatie over het genereren van [video bekijkt](video-reviews-quickstart-dotnet.md) voor uw menselijke revisoren van de gecontroleerde uitvoer.

Voeg [videotranscriptie beoordeelt](video-transcript-reviews-quickstart-dotnet.md) op uw video beoordelingen.

Bekijk de gedetailleerde zelfstudie over het ontwikkelen van een [videotoezicht oplossing voltooien](video-transcript-moderation-review-tutorial-dotnet.md). 
