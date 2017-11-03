---
title: Snelheid en gelijktijdigheid van uw codering met Azure Media Services beheren | Microsoft Docs
description: Dit artikel bevat een kort overzicht van hoe u de snelheid en gelijktijdigheid van uw codering taken/taken met Azure Media Services kunt beheren.
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 676313f8-a158-4e3a-a99b-2c29a341ecc9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: juliako
ms.openlocfilehash: 0463904fd9bf1138587d0d214e572ddd38cc2184
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
#  <a name="manage-speed-and-concurrency-of-your-encoding"></a>Snelheid en gelijktijdigheid van uw codering beheren

Dit artikel bevat een kort overzicht van hoe u de snelheid en gelijktijdigheid van uw codering taken/taken kunt beheren.

## <a name="overview"></a>Overzicht

In Media Services een **gereserveerde eenheidstype** bepaalt de snelheid waarmee uw media het verwerken van de taken worden verwerkt. U kunt kiezen uit de volgende gereserveerde-eenheidstypen: **S1**, **S2** en **S3**. Een coderingstaak wordt bijvoorbeeld sneller uitgevoerd wanneer u het gereserveerde-eenheidstype **S2** gebruikt (in vergelijking met het type **S1**). De [codering eenheden schalen](media-services-scale-media-processing-overview.md) onderwerp bevat een tabel waarmee u de beslissing nemen bij de keuze tussen verschillende codering snelheid te koppelen.

Naast het opgeven van het type gereserveerde eenheid, kunt u opgeven voor het inrichten van een account met **gereserveerde eenheden**. Op basis van het aantal ingerichte, gereserveerde eenheden wordt bepaald hoeveel mediataken tegelijk kunnen worden verwerkt voor een bepaald account. Bijvoorbeeld, als uw account vijf gereserveerde eenheden, heeft en vervolgens vijf media taken wordt gelijktijdig zo lang worden uitgevoerd als er zijn taken moeten worden verwerkt. De resterende taken in de wachtrij moeten wachten en wordt ophalen opgepikt sequentieel worden verwerkt wanneer een actieve taak is voltooid. Als een account heeft geen geen gereserveerde eenheden die zijn ingericht, zullen vervolgens taken worden opgepikt sequentieel worden verwerkt. In dit geval afhankelijk de wachttijd tussen één taak voltooid en de volgende begin van de beschikbaarheid van resources in het systeem.

Zie voor gedetailleerde informatie en voorbeelden die laten zien hoe u codering schaaleenheden [dit](media-services-scale-media-processing-overview.md) onderwerp.

## <a name="next-step"></a>Volgende stap

[Codering schaaleenheden](media-services-scale-media-processing-overview.md)

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

