---
title: Video Indexer scènes, foto's en hoofdframes - Azure
titlesuffix: Azure Media Services
description: In dit onderwerp biedt een overzicht van de Video Indexer-scènes, foto's en hoofdframes.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: dfa41dc695cf6ab357a9cd4cdbd32454b6dd107d
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896408"
---
# <a name="scenes-shots-and-keyframes"></a>Schermen, foto's en hoofdframes

Video Indexer biedt ondersteuning voor segmenteren video's in de tijdelijke eenheden op basis van structurele en semantische eigenschappen. Deze mogelijkheid kan klanten eenvoudig zoeken, beheren en hun video-inhoud op basis van verschillende granulaties bewerken. Bijvoorbeeld, gebaseerd op de schermen, foto's en hoofdframes, in dit onderwerp beschreven. De **scènedetectie** functie is momenteel in preview.   

![Schermen, foto's en hoofdframes](./media/scenes-shots-keyframes/scenes-shots-keyframes.png)

## <a name="scene-detection-preview"></a>Scènedetectie (preview)

Video Indexer bepaalt wanneer een scène wijzigingen in de video op basis van visuele aanwijzingen. Een scène ziet u één gebeurtenis en het bestaat uit een reeks opeenvolgende slagen, die semantisch zijn gerelateerd. Een scène miniatuur is de eerste sleutelframes van de onderliggende shot. Video indexer segmenten van een video in schermen op basis van kleur samenhang tussen opeenvolgende foto's en haalt de begin- en eindtijd van elke scène. Scènedetectie is een lastige taak beschouwd als hierbij kwantificeren semantische aspecten van video's.

> [!NOTE]
> Van toepassing op video's die ten minste 3 schermen bevatten.

## <a name="shot-detection"></a>Opnamedetectie

Video Indexer bepaalt wanneer een schermopname wordt gewijzigd in de video die op basis van visuele aanwijzingen, door bij te houden van abrupte en geleidelijk overgangen in het kleurenschema van de aangrenzende frames. Metagegevens van de schermopname bevat een start en eindtijd, evenals de lijst met hoofdframes opgenomen in deze schermopname. De foto's zijn opeenvolgende frames afkomstig uit de dezelfde camera op hetzelfde moment.

## <a name="keyframe-detection"></a>Detectie van sleutelframes

Hiermee selecteert u de frames die het beeld goed vertegenwoordigen. Hoofdframes zijn de representatieve frames hebt geselecteerd in de hele video op basis van aesthetic eigenschappen (bijvoorbeeld contrast en stableness). Video Indexer haalt een lijst van sleutelframes id's als onderdeel van de metagegevens van de foto, op basis van welke klanten de miniatuur sleutelframes kunnen extraheren. 

Hoofdframes zijn gekoppeld aan opnamen in de uitvoer-JSON. 

## <a name="next-steps"></a>Volgende stappen

[Bekijk de Video Indexer-uitvoer geproduceerd door de API](video-indexer-output-json-v2.md#scenes)