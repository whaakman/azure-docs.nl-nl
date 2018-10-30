---
title: Beheren van de snelheid en gelijktijdigheid van uw codering met Azure Media Services | Microsoft Docs
description: Dit artikel geeft een kort overzicht van hoe u de snelheid en gelijktijdigheid van uw taken/coderingstaken met Azure Media Services kunt beheren.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 676313f8-a158-4e3a-a99b-2c29a341ecc9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/29/2018
ms.author: juliako
ms.openlocfilehash: 3f358be630d9a50d0f048d66c9f79e168c8adcdf
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50230196"
---
#  <a name="manage-speed-and-concurrency-of-your-encoding"></a>De snelheid en gelijktijdigheid van uw codering beheren

Dit artikel geeft een kort overzicht van hoe u de snelheid en gelijktijdigheid van uw taken/coderingstaken kunt beheren.

## <a name="overview"></a>Overzicht

In Media Services, een **gereserveerde-eenheidstype** bepaalt de snelheid waarmee uw multimedia standaardtaken voor de verwerking worden verwerkt. U kunt kiezen uit de volgende gereserveerde-eenheidstypen: **S1**, **S2** en **S3**. Een coderingstaak wordt bijvoorbeeld sneller uitgevoerd wanneer u het gereserveerde-eenheidstype **S2** gebruikt (in vergelijking met het type **S1**). De [encoding-eenheden schalen](media-services-scale-media-processing-overview.md) onderwerp bevat een tabel die het helpt u besluit bij de keuze tussen verschillende snelheden voor codering.

Naast het opgeven van de gereserveerde-eenheidstype, kunt u opgeven voor het inrichten van uw account met **gereserveerde eenheden**. Op basis van het aantal ingerichte, gereserveerde eenheden wordt bepaald hoeveel mediataken tegelijk kunnen worden verwerkt voor een bepaald account. Bijvoorbeeld, als uw account vijf gereserveerde eenheden, heeft en vervolgens vijf mediataken gelijktijdig zo lang zal worden uitgevoerd als er zijn taken die moeten worden verwerkt. De resterende taken in de wachtrij wordt gewacht en wordt u opgehaald voor het verwerken van sequentieel worden verwerkt wanneer een actieve taak is voltooid. Als een account heeft geen alle gereserveerde eenheden worden ingericht, klikt u vervolgens taken opgehaald sequentieel worden verwerkt. In dit geval afhankelijk de wachttijd tussen één taak is voltooid en de volgende begin van de beschikbaarheid van resources in het systeem.

Zie voor gedetailleerde informatie en voorbeelden die laten zien hoe encoding-eenheden schalen [dit](media-services-scale-media-processing-overview.md) onderwerp.

## <a name="next-step"></a>Volgende stap

[Encoding-eenheden schalen](media-services-scale-media-processing-overview.md)

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

