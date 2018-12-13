---
title: Aanpassen van een persoon-model in Video Indexer - Azure
titlesuffix: Azure Media Services
description: In dit artikel biedt een overzicht van wat een persoon-model in Video Indexer is en hoe u kunt aanpassen.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 12/05/2018
ms.author: anzaman
ms.openlocfilehash: 073cff22f17f496c2ff85cfbf716751dfea1e03e
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53285023"
---
# <a name="customize-a-person-model-in-video-indexer"></a>Aanpassen van een persoon-model in Video Indexer


Video Indexer biedt ondersteuning voor gezichtsdetectie en herkenning van beroemdheden voor video-inhoud. De functie van de herkenning van beroemdheden bevat informatie over ongeveer 1.000.000 gezichten op basis van veelvuldig opgevraagde gegevensbron zoals IMDB Wikipedia en bovenste LinkedIn testteam. Gezichten die niet worden herkend door de functie voor de herkenning van beroemdheden worden gevonden. ze blijven echter naamloze. Nadat u uw video met Video Indexer uploaden en resultaten van de vorige ophalen, kunt u teruggaan en de naam van de gezichten die niet zijn herkend. Wanneer u een gezicht met de naam van een label, het gezicht en de naam toegevoegd naar persoon-model van uw account. Video Indexer herkent vervolgens deze gezicht in uw toekomstige video's en de afgelopen video's.

U kunt de Video Indexer-website of de API gebruiken om te bewerken van gezichten die zijn gedetecteerd in een video in uw account, zoals beschreven in de volgende onderwerpen:

- [Persoon model met behulp van API's aanpassen](customize-person-model-with-api.md)
- [Persoon model met behulp van de website aanpassen](customize-person-model-with-website.md)
