---
title: Inhoud beheerder Azure - Video toezicht | Microsoft Docs
description: Video toezicht machine ondersteund en menselijke revisie hulpprogramma's gebruiken gemiddeld ongeschikte inhoud
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/20/2018
ms.author: sajagtap
ms.openlocfilehash: fb26c9af55381c80a3f520b1a0068d8f72c91061
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344457"
---
# <a name="video-moderation"></a>Beheer van video

Gebruik inhoud beheerder machine ondersteunde [video toezicht](video-moderation-api.md) en [menselijke revisie hulpprogramma](Review-Tool-User-Guide/human-in-the-loop.md) gemiddeld video's en transcripties voor volwassene (expliciete) en mooie (suggestieve) inhoud ophalen van de beste resultaten voor uw bedrijf.

## <a name="video-trained-classifier"></a>Video getraind classificatie

Video classificatie machine ondersteunde wordt ofwel met installatiekopie getraind modellen of video getraind modellen bereikt. In tegenstelling tot de video classificaties installatiekopie getraind is van Microsoft volwassenen en mooie video classificatie getraind met video's. Deze methode resulteert in betere kwaliteit van de overeenkomst.

## <a name="shot-detection"></a>Schermopname detectie

Bij het uitvoeren van de details van de classificatie, kunt u extra video intelligence meer flexibiliteit bij het analyseren van video's. In plaats van alleen de frames uitvoeren, biedt de service van Microsoft video toezicht informatie voor de opname te. U hebt nu de optie voor het analyseren van uw video's op de schermopname niveau en de frame.
 
## <a name="key-frame-detection"></a>Sleutelframe detectie

In plaats van frames met regelmatige tussenpozen worden uitgevoerd, wordt de video toezicht service identificeert en levert mogelijk alleen volledige (goed) frames. De functie kunt efficiënt frame generatie voor de analyse volwassenen en mooie frame-niveau.

Het volgende extract ziet u een gedeeltelijk antwoord met mogelijke schermafbeeldingen, keyframes en volwassenen en mooie scores:

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


## <a name="visualization-for-human-reviews"></a>De visualisatie voor menselijke revisies

Voor meer nuanced gevallen bedrijven een menselijke revisie-oplossing nodig voor het opbouwen van de video, de frames en labels machine is toegewezen. De menselijke moderators controleren video's en frames een compleet overzicht van de inzichten te verkrijgen, labels wijzigen en hun beslissingen verzenden.

![video revisie hulpprogramma standaardweergave](images/video-review-default-view.png)

## <a name="player-view-for-video-level-review"></a>Player weergave voor het controleren van de video-niveau

Video-niveau binaire beslissingen met weergave-speler ziet u potentiële volwassenen en mooie frames mogelijk. De menselijke revisoren gaat de video met verschillende snelheid opties voor het onderzoeken van de schermen. Zij bevestigen hun beslissingen door het uitschakelen van de labels.

![video revisie hulpprogramma player weergeven](images/video-review-player-view.PNG)

## <a name="frames-view-for-detailed-reviews"></a>Frames weergeven voor gedetailleerde revisies

Een gedetailleerde video revisie voor analyse van de frame-voor-frame wordt gemaakt met een weergave op basis van het frame mogelijk. De menselijke revisoren bekijken en selecteer een of meer frames en labels om te bevestigen dat hun beslissingen in-of uitschakelen. Een optionele volgende stap is redactie van de aanstootgevende frames of de inhoud.

![video revisie hulpprogramma frames weergeven](images/video-review-frames-view-apply-tags.PNG)

## <a name="transcript-moderation"></a>De tekst van toezicht

Video's hebben doorgaans voice over die controle ook nodig voor aanstootgevende spraak. U kunt de service Azure Media Indexer spraak naar tekst wilt converteren en inhoud beheerder revisie API gebruikt voor het verzenden van de tekst voor de controle van de tekst in het hulpprogramma voor beoordeling van de.

![de tekst van de weergave in video controleren](images/video-review-transcript-view.png)

## <a name="next-steps"></a>Volgende stappen

Aan de slag met de [video toezicht Quick Start](video-moderation-api.md). 

Meer informatie over het genereren van [video bekijkt](video-reviews-quickstart-dotnet.md) voor uw menselijke revisoren van de gecontroleerde uitvoer.

Voeg [de tekst van de video bekijkt](video-transcript-reviews-quickstart-dotnet.md) op uw video beoordelingen.

Bekijk de gedetailleerde zelfstudie over het ontwikkelen van een [voltooien video toezicht oplossing](video-transcript-moderation-review-tutorial-dotnet.md). 
