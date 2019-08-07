---
title: Video Indexer scènes, afbeeldingen en keyframes-Azure
titlesuffix: Azure Media Services
description: In dit onderwerp vindt u een overzicht van de Video Indexer scènes, afbeeldingen en keyframes.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 07/05/2019
ms.author: juliako
ms.openlocfilehash: cdabc1b6bfed519098f656710ef49a946e676cf2
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68815653"
---
# <a name="scenes-shots-and-keyframes"></a>Scènes, opnamen en sleutelframes

Video Indexer ondersteunt het segmenteren van Video's in tijdelijke eenheden op basis van structurele en semantische eigenschappen. Met deze mogelijkheid kunnen klanten eenvoudig hun video-inhoud zoeken, beheren en bewerken op basis van verschillende granulariteit. Bijvoorbeeld, op basis van scènes, afbeeldingen en keyframes, zoals beschreven in dit onderwerp.   

![Scènes, opnamen en sleutelframes](./media/scenes-shots-keyframes/scenes-shots-keyframes.png)
 
## <a name="scene-detection"></a>Scène detectie  
 
Video Indexer bepaalt wanneer een scène wordt gewijzigd in video op basis van visuele hints. In een scène wordt één gebeurtenis weer gegeven en deze bestaat uit een reeks opeenvolgende opnamen, die semantisch verwant zijn. Een scène miniatuur is het eerste keyframe van de onderliggende afbeelding. Video indexer segmenteert een video in scènes op basis van kleur consistentie op opeenvolgende Foto's en haalt de begin-en eind tijd van elke scène op. Scène detectie wordt beschouwd als een uitdagende taak, aangezien het gaat om semantische aspecten van Video's te kwantificeren.

> [!NOTE]
> Van toepassing op Video's die ten minste drie scènes bevatten.

## <a name="shot-detection"></a>Opnamedetectie

Video Indexer bepaalt wanneer een foto in de video wordt gewijzigd op basis van visuele hints door zowel abrupte als geleidelijke overgangen in het kleuren schema van aangrenzende frames bij te houden. De meta gegevens van de opname bevatten een begin-en eind tijd, evenals de lijst met hoofd frames die in de afbeelding zijn opgenomen. De moment opnamen zijn opeenvolgende frames die tegelijkertijd zijn gemaakt van dezelfde camera.

## <a name="keyframe-detection"></a>Detectie van keyframe

Selecteert de frames die het beste de afdruk vertegenwoordigen. Keyframes zijn de representatieve frames die zijn geselecteerd uit de volledige video op basis van esthetische eigenschappen (bijvoorbeeld contrast en stabiele gegevens). Video Indexer haalt een lijst met hoofd frame-Id's op als onderdeel van de meta gegevens van de opname, op basis waarvan klanten de hoofd frame miniatuur kunnen uitpakken. 

Keyframes zijn gekoppeld aan opnamen in de uitvoer-JSON. 

## <a name="next-steps"></a>Volgende stappen

[Bekijk de Video Indexer uitvoer die de API produceert](video-indexer-output-json-v2.md#scenes)
