---
title: Codeer aangepaste transformeren met behulp van Media Services v3 .NET - Azure | Microsoft Docs
description: In dit onderwerp laat zien hoe Azure Media Services v3 gebruiken om een aangepaste transformeren met behulp van .NET.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
ms.date: 03/11/2019
ms.author: juliako
ms.openlocfilehash: ed2ae50aa9d7a26ed6e0569264ee981f7be35525
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58754563"
---
# <a name="how-to-encode-with-a-custom-transform---net"></a>Coderen met een aangepaste transformatie - .NET

Wanneer u met Azure Media Services encoding, u kunt snel aan de slag met een van de aanbevolen ingebouwde voorinstellingen gebaseerd op best practices, zoals geïllustreerd in de [Streaming bestanden](stream-files-tutorial-with-api.md) zelfstudie. U kunt ook een aangepaste voorinstelling wilt richten op uw specifieke vereisten voor scenario of het apparaat maken.

## <a name="considerations"></a>Overwegingen

Bij het maken van aangepaste voorinstellingen, de volgende overwegingen zijn van toepassing:

* Alle waarden voor de hoogte en breedte in AVC inhoud moet een meervoud van 4.
* In Azure Media Services v3 zijn alle van de codering bitsnelheden in bits per seconde. Dit wijkt af van het voorbeelddiagram met onze v2 API's, dat kilobits per seconde als de eenheid gebruikt. Bijvoorbeeld, als de bitrate in v2 is opgegeven als 128 (kilobits per seconde), zou in v3 deze worden ingesteld op 128000 (bits per seconde).

## <a name="prerequisites"></a>Vereisten 

[Een Azure Media Services-account maken](create-account-cli-how-to.md). <br/>Zorg ervoor dat u de naam van de resourcegroep en de naam van de Media Services-account. 

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Kloon een GitHub-opslagplaats met het volledige .NET Core-voorbeeld op de computer met de volgende opdracht:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```
 
De aangepaste voorinstelling voorbeeld bevindt zich in de [EncodeCustomTransform](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/EncodeCustomTransform/) map.

## <a name="create-a-transform-with-a-custom-preset"></a>Maken van een transformatie met een aangepaste voorinstelling 

Bij het maken van een nieuwe [transformeren](https://docs.microsoft.com/rest/api/media/transforms), moet u opgeven wat u wilt dat als uitvoer produceren. De vereiste parameter is een [TransformOutput](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#transformoutput)-object, zoals weergegeven in de onderstaande code. Elke **transformatie-uitvoer** bevat een **voorinstelling**. **Voorinstelling** bevat de stapsgewijze instructies van de video- en/of audioverwerkingen die moeten worden gebruikt voor het genereren van de gewenste **TransformOutput**. De volgende **TransformOutput** aangepaste codec en laag uitvoerinstellingen maakt.

Bij het maken van een [transformatie](https://docs.microsoft.com/rest/api/media/transforms) moet u controleren of er al een bestaat met de methode **Ophalen** zoals weergegeven in de volgende code. In Media Services v3, **ophalen** methoden op entiteiten retourneren **null** als de entiteit bestaat niet (een niet-hoofdlettergevoelige controle op de naam).

### <a name="example"></a>Voorbeeld

Het volgende voorbeeld definieert een set van uitvoer die we worden gegenereerd willen wanneer deze transformatie wordt gebruikt. We eerst toevoegen een AacAudio-laag voor de audio codering en twee H264Video lagen voor de videocodering. In de video lagen toewijzen we labels, zodat ze kunnen worden gebruikt in de namen van de uitvoer. Vervolgens gaan we de uitvoer naar alle miniaturen. In het onderstaande voorbeeld geven we afbeeldingen in PNG-indeling, die worden gegenereerd op 50% van de omzetting van de invoervideo en op drie tijdstempels - {% 25, 50%, 75} van de lengte van de invoervideo. Tot slot geven we de indeling voor de uitvoerbestanden: één voor video en audio, en een andere voor de miniaturen. Aangezien we meerdere H264Layers hebben, hebben we het gebruik van macro's die unieke namen per laag produceren. Kunnen we gebruiken een `{Label}` of `{Bitrate}` macro, in het voorbeeld ziet u de vorige.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/EncodeCustomTransform/MediaV3ConsoleApp/Program.cs#EnsureTransformExists)]

## <a name="next-steps"></a>Volgende stappen

[Streaming-bestanden](stream-files-tutorial-with-api.md) 
