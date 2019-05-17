---
title: Aanpassen van een persoon-model in Video Indexer - Azure
titlesuffix: Azure Media Services
description: In dit artikel biedt een overzicht van wat een persoon-model in Video Indexer is en hoe u kunt aanpassen.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: c74b913fc3ac35039d914fc97c9c438d2e4a3069
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65799428"
---
# <a name="customize-a-person-model-in-video-indexer"></a>Aanpassen van een persoon-model in Video Indexer

Video Indexer biedt ondersteuning voor herkenning van beroemdheden in uw video's. De functie van de herkenning van beroemdheden bevat informatie over ongeveer één miljoen gezichten op basis van veelvuldig opgevraagde gegevensbron zoals IMDB Wikipedia en bovenste LinkedIn testteam. Gezichten die niet worden herkend door Video Indexer nog steeds worden gedetecteerd, maar u nog geen naam. Klanten kunnen aangepaste persoon modellen bouwen en inschakelen van Video Indexer voor het herkennen van gezichten die niet standaard worden herkend. Klanten kunnen deze persoon-modellen bouwen door de naam van een persoon afbeeldingsbestanden van de persoon gezicht te koppelen.  

Als uw account caters naar verschillende use cases, kunt u profiteren van de mogelijkheid om meerdere modellen voor persoon per account te maken. Als de inhoud in uw account is bedoeld om te worden gesorteerd op verschillende kanalen, wilt u misschien een afzonderlijke persoon-model voor elk kanaal maken. 

> [!NOTE]
> Elke persoon model ondersteunt maximaal 1 miljoen mensen en elk account heeft een limiet van 50 persoon modellen. 

Nadat een model is gemaakt, kunt u deze kunt gebruiken door op te geven van de model-ID van een specifieke persoon model bij het uploaden/indexeren of een video opnieuw indexeren. Het specifieke aangepaste model dat de video is gekoppeld aan een nieuw gezicht voor een video training, worden bijgewerkt. 

Als u niet de ondersteuning voor meerdere model persoon hoeft, wijs geen een persoon model-ID toe aan uw video bij het uploaden/indexeren of opnieuw indexeren. Video Indexer gebruikt in dit geval het standaardmodel persoon in uw account. 

U kunt de website Video Indexer gebruiken gezichten die zijn gedetecteerd in een video bewerken en beheren van meerdere modellen in de aangepaste persoon in uw account, zoals beschreven in de [aanpassen van een persoon-model met behulp van een website](customize-person-model-with-website.md) onderwerp. U kunt ook de API, zoals beschreven in [aanpassen van een persoon-model met behulp van API's](customize-person-model-with-api.md).