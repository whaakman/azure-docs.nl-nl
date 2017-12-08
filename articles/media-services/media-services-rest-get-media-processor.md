---
title: Het ophalen van een Mediaprocessor-exemplaar met behulp van REST | Microsoft Docs
description: Informatie over het maken van een onderdeel van de processor media als u wilt coderen, indeling converteren, versleutelen of ontsleutelen van media-inhoud voor Azure Media Services.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: f9ff1997-0da6-4528-aaed-792837e5be41
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: 4ad90ad979c5bd74fc55155098c88d5c13cb12e2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-get-a-media-processor-instance"></a>Het ophalen van een exemplaar van de Media-Processor
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

>[!NOTE]
>Bij het openen van entiteiten in Media Services, moet u specifieke header-velden en waarden instellen in uw HTTP-aanvragen. Zie voor meer informatie [Setup voor het ontwikkelen van Media Services REST API](media-services-rest-how-to-use.md).

## <a name="connect-to-media-services"></a>Verbinding met Media Services maken

Zie voor meer informatie over de verbinding maken met de AMS API [toegang tot de API van Azure Media Services met Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md). 

>[!NOTE]
>Na het correct verbinding maakt met https://media.windows.net, ontvangt u een 301 omleiding opgeven van een andere URI van de Media Services. U moet de volgende aanroepen naar de nieuwe URI.

## <a name="get-a-media-processor"></a>Ophalen van een Mediaprocessor

De volgende REST-aanroep laat zien hoe u een exemplaar van de processor media op naam (in dit geval **Media Encoder Standard**). 

Aanvraag:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.11
    Host: media.windows.net

Antwoord:

    . . .

    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }


## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Volgende stappen
Nu dat u hoe een exemplaar van de processor media ophalen weet, gaat u naar de [hoe een Asset coderen](media-services-rest-get-started.md) onderwerp dat wordt beschreven hoe u met Media Encoder Standard een asset coderen.

