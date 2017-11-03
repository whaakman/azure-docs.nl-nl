---
title: Overzicht en een vergelijking van Azure op aanvraag media encoders | Microsoft Docs
description: In dit onderwerp biedt een overzicht en een vergelijking van Azure op aanvraag media coderingsprogramma's.
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: e6bfc068-fa46-4d68-b1ce-9092c8f3a3c9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: juliako
ms.openlocfilehash: 538a6ab60168735c2626a93cdeedd8d4999a6efc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a>Overzicht en een vergelijking van Azure op aanvraag media coderingsprogramma 's
## <a name="encoding-overview"></a>Overzicht van de codering
Azure Media Services biedt meerdere opties voor de codering van media in de cloud.

Wanneer u begint met Media Services, is het belangrijk om het verschil tussen codecs en bestandsindelingen.
Codecs zijn de software die u de algoritmen compressie/decompressie implementeert dat bestandsindelingen zijn containers waarin de gecomprimeerde video.

Media Services biedt dynamische pakketten zodat u uw adaptive bitrate MP4- of Smooth Streaming gecodeerde inhoud kunt in de streaming-indelingen die worden ondersteund door Media Services (MPEG DASH, HLS, Smooth Streaming) leveren zonder dat u opnieuw verpakken in een van deze streaming-indelingen hoeft.

>[!NOTE]
>Wanneer uw AMS-account is gemaakt, wordt er een **standaardstreaming-eindpunt** met de status **Gestopt** toegevoegd aan uw account. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben. Om te profiteren van [dynamische pakketten](media-services-dynamic-packaging-overview.md), moet u het volgende doen:
>
>Bovendien moet u uw bronbestand coderen in een set adaptive bitrate MP4-bestanden of adaptive bitrate Smooth Streaming-bestanden (de coderingsstappen worden verderop in deze zelfstudie uitgelegd).

Media Services ondersteunt de volgende op aanvraag coderingsprogramma's die worden beschreven in dit artikel:

* [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
* [Media Encoder Premium Workflow](media-services-encode-asset.md#media-encoder-premium-workflow)

Dit artikel bevat een kort overzicht van op verzoek media coderingsprogramma's en koppelingen naar artikelen waarmee u meer gedetailleerde informatie. Het onderwerp bevat ook een vergelijking van het coderingsprogramma's.

>[!NOTE]
>Standaard hebben elke Media Services-account een actieve codering taak tegelijk. U kunt codering eenheden waarmee u kunt meerdere codering taken gelijktijdig uitgevoerd, één voor elke codering gereserveerde eenheid aankoop reserveren. Zie voor informatie [codering eenheden schalen](media-services-scale-media-processing-overview.md).

## <a name="media-encoder-standard"></a>Media Encoder Standard
### <a name="how-to-use"></a>Gebruiksinstructies
[Het coderen met Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md)

### <a name="formats"></a>Indelingen
[Indelingen en codecs](media-services-media-encoder-standard-formats.md)

### <a name="presets"></a>Standaardinstellingen
Media Encoder Standard is geconfigureerd met een van de encoder standaardinstellingen beschreven [hier](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

### <a name="input-and-output-metadata"></a>Invoer en uitvoer van metagegevens
De invoer metagegevens coderingsprogramma's wordt beschreven [hier](media-services-input-metadata-schema.md).

De metagegevens van de uitvoer coderingsprogramma's wordt beschreven [hier](media-services-output-metadata-schema.md).

### <a name="generate-thumbnails"></a>Genereren van miniaturen
Zie voor informatie [het genereren van miniaturen met Media Encoder Standard](media-services-advanced-encoding-with-mes.md#thumbnails).

### <a name="trim-videos-clipping"></a>Trim video's (paginaknipsel)
Zie voor informatie [hoe video's met Media Encoder Standard trim](media-services-advanced-encoding-with-mes.md#trim_video).

### <a name="create-overlays"></a>Overlays maken
Zie voor informatie [maken met behulp van Media Encoder Standard overlays](media-services-advanced-encoding-with-mes.md#overlay).

### <a name="see-also"></a>Zie ook
[De Media Services-blog](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)

## <a name="media-encoder-premium-workflow"></a>Media Encoder Premium Workflow
### <a name="overview"></a>Overzicht
[Inleiding tot Premium codering in Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

### <a name="how-to-use"></a>Gebruiksinstructies
Werkstroom voor Media Encoder Premium is geconfigureerd met behulp van complexe werkstromen. Werkstroombestanden kan worden gemaakt en bijgewerkt met de [Workflow Designer](media-services-workflow-designer.md) hulpprogramma.

[Het gebruik van Premium codering in Azure Media Services](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

### <a name="known-issues"></a>Bekende problemen
Als uw invoervideo geen bevat nog ondertiteling, de uitvoer van de Asset bevatten steeds een leeg TTML-bestand.


## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Verwante artikelen:
* [Geavanceerde codering taken uitvoeren met Media Encoder Standard standaardinstellingen aanpassen](media-services-custom-mes-presets-with-dotnet.md)
* [Quota's en beperkingen](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
