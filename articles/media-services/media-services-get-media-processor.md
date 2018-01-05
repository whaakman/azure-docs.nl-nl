---
title: Het maken van een Mediaprocessor met Azure Media Services SDK voor .NET | Microsoft Docs
description: Informatie over het maken van een onderdeel van de processor media als u wilt coderen, indeling converteren, versleutelen of ontsleutelen van media-inhoud voor Azure Media Services. Codevoorbeelden zijn geschreven in C# en gebruiken van de Media Services SDK voor .NET.
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: dbf9496f-c6f0-42a7-aa36-70f89dcb8ea2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: c2cbe41b71afa8acc184f9d7f4cfe94686de783e
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="how-to-get-a-media-processor-instance"></a>Procedure: een exemplaar van de Processor Media
> [!div class="op_single_selector"]
> * [.NET](media-services-get-media-processor.md)
> * [REST](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a>Overzicht
In Media Services die een Mediaprocessor een component die verantwoordelijk is voor een specifieke verwerkingstaak is, zoals de codering, versleutelen of ontsleutelen van media-inhoud-conversie-indeling. U doorgaans maken een Mediaprocessor tijdens het maken van een taak voor het coderen, versleutelen of de indeling van media-inhoud te converteren.

## <a name="azure-media-processors"></a>Azure media-processors 

Het volgende onderwerp bevat een lijst met media processors:

* [Codering van media-processors](scenarios-and-availability.md#encoding-media-processors)
* [Analytics-mediaprocessoren](scenarios-and-availability.md#analytics-media-processors)

## <a name="get-media-processor"></a>Ophalen van Mediaprocessor

De volgende methode laat zien hoe een exemplaar van de processor media. In het voorbeeld wordt ervan uitgegaan dat het gebruik van een module-niveau variabele met de naam **_context** om te verwijzen naar de servercontext zoals beschreven in de sectie [hoe: verbinding maken met Media Services programmatisch](media-services-use-aad-auth-to-access-ams-api.md).

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

        return processor;
    }


## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Volgende stappen
Nu dat u hoe een exemplaar van de processor media ophalen weet, gaat u naar de [hoe een Asset coderen](media-services-dotnet-encode-with-media-encoder-standard.md) onderwerp dat wordt beschreven hoe u met Media Encoder Standard een asset coderen.

