---
title: Veelgestelde vragen over Azure Media Services | Microsoft Docs
description: Veelgestelde vragen (FAQ's)
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: bf5bc66ce55b0b9d6095cd395a11f68b40af1639
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51685715"
---
# <a name="frequently-asked-questions"></a>Veelgestelde vragen

In dit artikel komen de antwoorden op veelgestelde vragen die worden gegenereerd door de gebruikerscommunity van Azure Media Services (AMS).

## <a name="general-ams-faqs"></a>Veelgestelde vragen over de algemene AMS

V: hoe u streamen naar Apple iOS-apparaten

A: toevoegen ' (format = m3u8-aapl) ' pad naar het gedeelte '/ Manifest' van de URL om te zien van de server voor streaming oorsprong om terug te keren back HLS-inhoud voor gebruik op Apple iOS native-apparaten (Zie voor meer informatie, [leveren van inhoud](media-services-deliver-content-overview.md)),

V: hoe u schalen indexeren?

A: de gereserveerde eenheden zijn voor codering en indexering taken hetzelfde. Volg de instructies op [hoe schaal gereserveerde Encoding-eenheden](media-services-scale-media-processing-overview.md). **Houd er rekening mee** prestaties van de indexeerfunctie wordt niet beïnvloed door gereserveerde-eenheidstype.

V: ik geüpload, gecodeerd en een video gepubliceerd. Wat is de reden van de video niet afgespeeld wanneer ik probeer te streamen?

A: een van de meest voorkomende oorzaken is dat u hebt geen het streaming-eindpunt van waaruit u probeert om af te spelen in de **met** staat.  

V: kan ik samenstellen op een live stream?

A: samenstellen voor live streams wordt momenteel niet aangeboden in Azure Media Services, dus u moet voor het opstellen van vooraf op uw computer.

V: kan ik Azure CDN gebruiken met Live Streaming

A: Media Services biedt ondersteuning voor integratie met Azure CDN (Zie voor meer informatie, [Streaming-eindpunten beheren in een Media Services-Account](media-services-portal-manage-streaming-endpoints.md)).  U kunt Live streamen met CDN. Azure Media Services biedt Smooth Streaming, HLS en MPEG-DASH-uitvoer. Alle deze indelingen HTTP gebruiken voor het overbrengen van gegevens en profiteer van de voordelen van HTTP-caching. In de live streaming werkelijke video en audio-gegevens worden verdeeld aan fragmenten en deze afzonderlijke fragmenten ophalen in de cache opgeslagen in het CDN. Alleen gegevens moeten worden vernieuwd, is het manifest gegevens. CDN vernieuwt periodiek de manifest-gegevens.

V: biedt Azure Media services ondersteunt afbeeldingen opslaan?

A: als u alleen voor het opslaan van JPEG of PNG-afbeeldingen, moet u die in Azure Blob Storage. Er is geen voordeel voor u ze in een Media Services-account, tenzij u wilt behouden die zijn gekoppeld aan uw Video of Audio-Assets. Of als u behoefte aan de installatiekopieën gebruiken als overlays in de video encoder mogelijk. Media Encoder Standard hierbij afbeeldingen boven op video's ondersteunt, en dat is wat u ziet hier JPEG- of PNG ondersteund invoer indelingen. Zie voor meer informatie, [maken Overlays](media-services-advanced-encoding-with-mes.md#overlay).

V: hoe kan ik elementen van een Media Services-account naar een andere kopiëren.

A: elementen te kopiëren van een Media Services-account naar de andere met behulp van .NET, gebruiken [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) uitbreidingsmethode beschikbaar in de [Azure Media Services .NET SDK Extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions/) opslagplaats. Zie voor meer informatie, [dit](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) forumthread vindt.

V: wat zijn de ondersteunde tekens in voor het benoemen van bestanden bij het werken met AMS?

A: Media Services wordt de waarde van de eigenschap IAssetFile.Name bij het bouwen van URL's voor de streaming-inhoud (bijvoorbeeld http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Om deze reden is procent codering niet toegestaan. De waarde van de **naam** eigenschap kan niet een van de volgende hebben [procent-encoding-gereserveerde tekens](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] '. Bovendien kunnen alleen er een '.' voor de bestandsnaamextensie.

V: hoe kan ik verbinding maken met behulp van REST?

A: voor informatie over hoe u verbinding maken met de AMS-API's Zie [toegang tot de API van Azure Media Services met Azure AD-verificatie](media-services-use-aad-auth-to-access-ams-api.md). 

V: hoe kan ik een video draaien tijdens de codering.

A: de [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) draaien door de hoeken van 180-90/270 ondersteunt. Het standaardgedrag is 'Auto', waarbij wordt geprobeerd om te detecteren van de metagegevens van de rotatie in het inkomende MP4/MOV-bestand en compenseren voor het. De volgende **bronnen** element op een van de json-voorinstellingen gedefinieerd [hier](media-services-mes-presets-overview.md):

    "Version": 1.0,
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [

    ...


## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
