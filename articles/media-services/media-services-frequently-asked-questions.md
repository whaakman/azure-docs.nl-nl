---
title: Veelgestelde vragen over Azure Media Services | Microsoft Docs
description: Veelgestelde vragen (FAQ's)
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 5374f7f4-c189-43ef-8b7f-f2f4141e2748
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: juliako
ms.openlocfilehash: ab66994b0212593aff1384b0801f3359eb0a3751
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="frequently-asked-questions"></a>Veelgestelde vragen

In dit artikel komen Veelgestelde vragen die worden gegenereerd door de gebruikersgemeenschap Azure Media Services (AMS).

## <a name="general-ams-faqs"></a>Algemene AMS Veelgestelde vragen

V: hoe u stream op Apple iOS-apparaten

A: toevoegen ' (format = m3u8-aapl) ' pad naar het gedeelte '/ Manifest' van de URL van de bronserver streaming naar vorige HLS inhoud voor consumptie op Apple iOS systeemeigen apparaten (voor meer informatie Zie (leveren van inhoud) [vertellen Media-services-afleveren-inhoud-overview.md]),

V: hoe u de schaal van indexeren?

A: de gereserveerde eenheden zijn hetzelfde voor codering en indexering taken. Volg de instructies op [How to Scale met gereserveerde Coderingseenheden](media-services-scale-media-processing-overview.md). **Opmerking** prestaties van de indexeerfunctie wordt niet beïnvloed door gereserveerde eenheidstype.

V: ik geüpload, gecodeerd en u een video gepubliceerd. Wat is de reden van de video niet afgespeeld wanneer ik probeer om te streamen deze?

A: een van de meest voorkomende redenen is er geen het streaming-eindpunt van waaruit u probeert te afspelen in de **met** status.  

V: kan ik samenstellen op een live stream doen?

A: samenstellen op live gegevensstromen momenteel niet wordt aangeboden op Azure Media Services, dus u moet voor het opstellen van vooraf op uw computer.

V: kan ik Azure CDN gebruiken met Live Streaming

A: Media Services ondersteunt de integratie met Azure CDN (Zie voor meer informatie [Streaming-eindpunten in een Media Services-Account](media-services-portal-manage-streaming-endpoints.md)).  U kunt Live streamen met CDN. Azure Media Services biedt Smooth Streaming, HLS en MPEG-DASH-uitvoer. Alle deze indelingen HTTP gebruiken voor het overbrengen van gegevens en voordelen van het HTTP-caching krijgen. In de live streaming werkelijke video en audio-gegevens worden verdeeld op fragmenten en deze afzonderlijke fragmenten ophalen in de cache opgeslagen in CDN. Gegevensbehoeften worden vernieuwd, is de manifest-gegevens. CDN worden regelmatig vernieuwd manifest gegevens.

V: biedt Azure Media services ondersteuning voor installatiekopieën van opslaan?

A: als u alleen op zoek bent voor het opslaan van JPEG of PNG-installatiekopieën, moet u die in Azure Blob Storage behouden. Er is geen voordeel in uw Media Services-account plaatsen, tenzij u wilt behouden die zijn gekoppeld aan uw Video of Audio activa. Of als u wellicht nodig is de afbeeldingen als overlays in de video encoder gebruikt. Media Encoder Standard ondersteunt hierbij afbeeldingen boven op video's en dat deze lijsten JPEG- en PNG-ondersteunde indelingen invoer. Zie voor meer informatie [Overlays maken](media-services-advanced-encoding-with-mes.md#overlay).

V: hoe kan ik activa van een Media Services-account naar de andere kopiëren.

A: elementen te kopiëren van een Media Services-account naar een andere gebruiken met .NET, [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) uitbreidingsmethode beschikbaar in de [Azure Media Services .NET SDK Extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions/) opslagplaats. Zie voor meer informatie [dit](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) forum thread.

V: wat zijn de ondersteunde tekens voor het benoemen van bestanden wanneer u werkt met AMS?

A: Media Services gebruikt de waarde van de eigenschap IAssetFile.Name tijdens het bouwen van URL's voor de streaming-inhoud (bijvoorbeeld http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Om deze reden is procent codering niet toegestaan. De waarde van de **naam** eigenschap kan niet een van de volgende hebben [procent-encoding-gereserveerde tekens](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] '. Bovendien kunnen alleen er een '.' voor de bestandsnaamextensie.

V: hoe u verbinding maakt met behulp van REST?

A: Zie voor meer informatie over de verbinding maken met de AMS API [toegang tot de API van Azure Media Services met Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md). 

V: hoe kan ik een video draaien tijdens de codering.

A: de [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) draaihoek door hoeken van 180-90/270 ondersteunt. De standaardinstelling is 'Auto', waarbij wordt geprobeerd om te detecteren van de metagegevens van de draaihoek in het inkomende MP4/MOV-bestand en compenseren voor het. De volgende **bronnen** element op een van de json-standaardinstellingen gedefinieerd [hier](media-services-mes-presets-overview.md):

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
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
