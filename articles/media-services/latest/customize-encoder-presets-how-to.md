---
title: Codeer aangepaste transformeren met behulp van Media Services v3 - Azure | Microsoft Docs
description: In dit onderwerp laat zien hoe Azure Media Services v3 gebruiken om een aangepaste transformatie.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
ms.date: 12/08/2018
ms.author: juliako
ms.openlocfilehash: c62d9132cdd7eb2ebcbecc3c417ad30d368a278a
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53138701"
---
# <a name="how-to-encode-with-a-custom-transform"></a>Coderen met een aangepaste transformatie

Wanneer u met Azure Media Services encoding, u kunt snel aan de slag met een van de aanbevolen ingebouwde voorinstellingen gebaseerd op best practices, zoals geïllustreerd in de [Streaming bestanden](stream-files-tutorial-with-api.md) zelfstudie, of u kunt ervoor kiezen om te maken van een aangepaste voorinstelling voor uw specifieke vereisten voor scenario of het apparaat. 

> [!Note]
> In Azure Media Services v3 zijn alle van de codering bitsnelheden in bits per seconde. Dit is anders dan de REST-v2 voorinstellingen van Media Encoder Standard. Bijvoorbeeld, de bitrate in v2 zou worden opgegeven als 128, maar in v3 128000 zou zijn.

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Kloon een GitHub-opslagplaats met het volledige .NET Core-voorbeeld op de computer met de volgende opdracht:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```
 
De aangepaste voorinstelling voorbeeld bevindt zich in de [EncodeCustomTransform](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/EncodeCustomTransform/) map.

## <a name="create-a-transform-with-a-custom-preset"></a>Maken van een transformatie met een aangepaste voorinstelling 

Bij het maken van een nieuwe [transformeren](https://docs.microsoft.com/rest/api/media/transforms), moet u opgeven wat u wilt dat als uitvoer produceren. De vereiste parameter is een **TransformOutput**-object, zoals weergegeven in de onderstaande code. Elke **transformatie-uitvoer** bevat een **voorinstelling**. **Voorinstelling** bevat de stapsgewijze instructies van de video- en/of audioverwerkingen die moeten worden gebruikt voor het genereren van de gewenste **TransformOutput**. De volgende **TransformOutput** aangepaste codec en laag uitvoerinstellingen maakt.

Bij het maken van een [transformatie](https://docs.microsoft.com/rest/api/media/transforms) moet u controleren of er al een bestaat met de methode **Ophalen** zoals weergegeven in de volgende code.  In Media Services v3, **ophalen** methoden op entiteiten retourneren **null** als de entiteit bestaat niet (een niet-hoofdlettergevoelige controle op de naam).

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/EncodeCustomTransform/MediaV3ConsoleApp/Program.cs#EnsureTransformExists)]

## <a name="next-steps"></a>Volgende stappen

[Streaming-bestanden](stream-files-tutorial-with-api.md) 
