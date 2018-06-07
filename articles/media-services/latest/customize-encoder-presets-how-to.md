---
title: Aangepaste transformatie met Azure Media Services v3 coderen | Microsoft Docs
description: Dit onderwerp leest hoe u Azure Media Services v3 gebruikt voor het coderen van een aangepaste transformatie.
services: media-services
documentationcenter: ''
author: Juliako
manager: cflower
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 05/17/2018
ms.author: juliako
ms.openlocfilehash: d298070877a366d04b2df1ef8ac63b08f8771de0
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655195"
---
# <a name="how-to-encode-with-a-custom-transform"></a>Het coderen met een aangepaste transformatie

Wanneer u met Azure Media Services-codering, u kunt snel aan de slag met een van de aanbevolen ingebouwde standaardinstellingen gebaseerd op best practices, zoals wordt beschreven in de [Streaming-bestanden](stream-files-tutorial-with-api.md) zelfstudie, of u kunt kiezen voor het bouwen van een aangepast voorinstelling toe te passen van uw specifieke vereisten voor scenario of het apparaat. 

> [!Note]
> Alle van de codering bitsnelheden zijn in Azure Media Services-v3 in bits per seconde. Dit is anders dan de REST-v2 die Media Encoder Standard voorinstellingen. Bijvoorbeeld, de bitrate in v2 zou worden opgegeven als 128, maar in v3-128000 zou zijn.

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Klonen van een GitHub-opslagplaats met het volledige voorbeeld .NET Core aan op de computer met de volgende opdracht:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```
 
De aangepaste vooraf ingestelde voorbeeld bevindt zich in de [EncodeCustomTransform](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/EncodeCustomTransform/) map.

## <a name="create-a-transform-with-a-custom-preset"></a>Een transformatie maken met een aangepaste voorinstelling 

Bij het maken van een nieuwe [transformeren](https://docs.microsoft.com/rest/api/media/transforms), moet u opgeven wat u wilt maken als uitvoer. De vereiste parameter is een **TransformOutput**-object, zoals weergegeven in de onderstaande code. Elke **transformatie-uitvoer** bevat een **voorinstelling**. **Voorinstelling** bevat de stapsgewijze instructies van de video- en/of audioverwerkingen die moeten worden gebruikt voor het genereren van de gewenste **TransformOutput**. De volgende **TransformOutput** aangepaste instellingen voor codec en laag uitvoer maakt.

Bij het maken van een [transformatie](https://docs.microsoft.com/rest/api/media/transforms) moet u controleren of er al een bestaat met de methode **Ophalen** zoals weergegeven in de volgende code.  In Media Services-v3 **ophalen** methoden op entiteiten retourneren **null** als de entiteit (een niet-hoofdlettergevoelige controle op de naam) niet bestaat.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/EncodeCustomTransform/MediaV3ConsoleApp/Program.cs#EnsureTransformExists)]

## <a name="next-steps"></a>Volgende stappen

[Streaming-bestanden](stream-files-tutorial-with-api.md) 
