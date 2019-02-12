---
title: Over het verkrijgen van een Mediaprocessor-exemplaar met behulp van REST | Microsoft Docs
description: Informatie over het maken van een onderdeel van de processor media om te coderen, converteren indeling, versleutelen of ontsleutelen van media-inhoud voor Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: f9ff1997-0da6-4528-aaed-792837e5be41
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: cc9f65caabe7acaf99952692389838b808af0750
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55996335"
---
# <a name="how-to-get-a-media-processor-instance"></a>Over het verkrijgen van een exemplaar van Mediaprocessor
> [!div class="op_single_selector"]
> * [.NET](media-services-get-media-processor.md)
> * [REST](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a>Overzicht
Media-Processors zijn een component die verantwoordelijk is voor een specifieke video of audio verwerkingstaak, zoals codering, Indelingsconversie, versleutelen of ontsleutelen media-inhoud. Alle taken die worden verzonden naar Media Services moet een Mediaprocessor coderen, versleutelen of de video of audio-inhoud te converteren. 

## <a name="azure-media-processors"></a>Azure media-processors 

Het volgende onderwerp bevat een lijst met media-processors:

* [Mediaprocessors coderen](scenarios-and-availability.md#encoding-media-processors)
* [Mediaprocessors voor analyse](scenarios-and-availability.md#analytics-media-processors)

>[!NOTE]
>Bij het openen van entiteiten in Media Services, moet u specifieke header-velden en waarden instellen in uw HTTP-aanvragen. Zie voor meer informatie, [instellen voor het ontwikkelen van Media Services REST API](media-services-rest-how-to-use.md).

## <a name="connect-to-media-services"></a>Verbinding met Media Services maken

Zie voor meer informatie over het verbinding maken met de AMS-API [toegang tot de API van Azure Media Services met Azure AD-verificatie](media-services-use-aad-auth-to-access-ams-api.md). 


## <a name="get-a-media-processor"></a>Een Mediaprocessor ophalen

De volgende REST-aanroep laat zien hoe u een exemplaar van de processor media ophalen met de naam (in dit geval **Media Encoder Standard**). 

Aanvraag:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.17
    Host: media.windows.net

Reactie:

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
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Volgende stappen
Nu dat u hoe u een exemplaar van de processor media ophalen weet, gaat u naar de [een Asset coderen](media-services-rest-get-started.md) artikel die laat zien hoe u een asset coderen Media Encoder Standard gebruikt.

