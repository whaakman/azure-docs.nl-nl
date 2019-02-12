---
title: Over het maken van een Mediaprocessor met behulp van de Azure Media Services SDK voor .NET | Microsoft Docs
description: Informatie over het maken van een onderdeel van de processor media om te coderen, converteren indeling, versleutelen of ontsleutelen van media-inhoud voor Azure Media Services. Codevoorbeelden zijn geschreven in C# en de Media Services SDK voor .NET gebruiken.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: dbf9496f-c6f0-42a7-aa36-70f89dcb8ea2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 0a8cb1178ec70d4e50f2a45834f9592c4708c5af
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55998634"
---
# <a name="how-to-get-a-media-processor-instance"></a>Procedure: Een Mediaprocessor-exemplaar ophalen
> [!div class="op_single_selector"]
> * [.NET](media-services-get-media-processor.md)
> * [REST](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a>Overzicht
In Media Services is die een Mediaprocessor een onderdeel die verantwoordelijk is voor een specifieke verwerkingstaak is, zoals de encoding,-indeling converteren, versleutelen of ontsleutelen van media-inhoud. Doorgaans maakt u een Mediaprocessor bij het maken van een taak om te coderen, versleutelen of de indeling van media-inhoud te converteren.

## <a name="azure-media-processors"></a>Azure media-processors 

Het volgende onderwerp bevat een lijst met media-processors:

* [Mediaprocessors coderen](scenarios-and-availability.md#encoding-media-processors)
* [Mediaprocessors voor analyse](scenarios-and-availability.md#analytics-media-processors)

## <a name="get-media-processor"></a>Ophalen van Mediaprocessor

De volgende methode laat zien hoe een exemplaar van de processor media. De voorbeeldcode wordt ervan uitgegaan dat het gebruik van een module-niveau variabele met de naam **_context** om te verwijzen naar de servercontext zoals beschreven in de sectie [het: Verbinding maken met mediaservices via een programma](media-services-use-aad-auth-to-access-ams-api.md).

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

        return processor;
    }


## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Volgende stappen
Nu dat u hoe u een exemplaar van de processor media ophalen weet, gaat u naar de [een Asset coderen](media-services-dotnet-encode-with-media-encoder-standard.md) onderwerp leert u hoe u de Media Encoder Standard voor een asset coderen.

