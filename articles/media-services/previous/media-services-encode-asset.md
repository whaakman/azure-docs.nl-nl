---
title: Overzicht en vergelijking van Azure op media coderingsprogramma | Microsoft Docs
description: Dit onderwerp bevat een overzicht en vergelijking van Azure op aanvraag media coderingsprogramma's.
services: media-services
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.assetid: e6bfc068-fa46-4d68-b1ce-9092c8f3a3c9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: juliako
ms.openlocfilehash: f44f5cffd105d958c7d6552a170150623a0701ea
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2018
ms.locfileid: "34638448"
---
# <a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a>Overzicht en vergelijking van Azure op de media-coderingsprogramma
## <a name="encoding-overview"></a>Overzicht van versleuteling
Azure Media Services biedt meerdere opties voor de versleuteling van media in de cloud.

Wanneer u begint met Media Services, is het belangrijk dat u het verschil tussen codecs en bestandsindelingen.
Codecs zijn de software die de algoritmen compressie/decompressie implementeert dat bestandsindelingen zijn containers waarin de gecomprimeerde video.

Media Services biedt dynamische pakketten waarmee u uw adaptive bitrate MP4- of Smooth Streaming gecodeerde inhoud in de streaming-indelingen die worden ondersteund door Media Services (MPEG DASH, HLS, Smooth Streaming) leveren zonder dat u hoeft op te opnieuw in een van deze streaming-indelingen.

> [!NOTE]
> Wanneer uw AMS-account is gemaakt, wordt er een **standaardstreaming-eindpunt** met de status **Gestopt** toegevoegd aan uw account. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben. 

Media Services ondersteunt de volgende op aanvraag coderingsprogramma's die in dit artikel worden beschreven:

* [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
* [Media Encoder Premium Workflow](media-services-encode-asset.md#media-encoder-premium-workflow)

In dit artikel biedt een kort overzicht van de on-demand media coderingsprogramma's en vindt u koppelingen naar artikelen met meer gedetailleerde informatie. Het onderwerp bevat ook een vergelijking van het coderingsprogramma's.

>[!NOTE]
>Standaard kan elke Media Services-account een actieve coderingstaak tegelijk hebben. Encoding-eenheden waarmee u kunt meerdere coderingstaken gelijktijdig worden uitgevoerd, één voor elke codering gereserveerde eenheid die u koopt hebt, kunt u reserveren. Zie voor meer informatie, [encoding-eenheden schalen](media-services-scale-media-processing-overview.md).

## <a name="media-encoder-standard"></a>Media Encoder Standard
### <a name="how-to-use"></a>Gebruiksinstructies
[Coderen met Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md)

### <a name="formats"></a>Indelingen
[Indelingen en codecs voor](media-services-media-encoder-standard-formats.md)

### <a name="presets"></a>Voorinstellingen
Media Encoder Standard is geconfigureerd met behulp van een van de encoder voorinstellingen beschreven [hier](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

### <a name="input-and-output-metadata"></a>Invoer en uitvoer van metagegevens
De metagegevens van de invoer van de coderingsprogramma's wordt beschreven [hier](media-services-input-metadata-schema.md).

De metagegevens van de uitvoer coderingsprogramma's wordt beschreven [hier](media-services-output-metadata-schema.md).

### <a name="generate-thumbnails"></a>Miniatuurweergaven genereren
Zie voor meer informatie, [hoe miniatuurweergaven genereren met Media Encoder Standard](media-services-advanced-encoding-with-mes.md#thumbnails).

### <a name="trim-videos-clipping"></a>Trim video's (knippen)
Zie voor meer informatie, [hoe u video's met Media Encoder Standard trim](media-services-advanced-encoding-with-mes.md#trim_video).

### <a name="create-overlays"></a>Overlays maken
Zie voor meer informatie, [over het maken van overlays met Media Encoder Standard](media-services-advanced-encoding-with-mes.md#overlay).

### <a name="see-also"></a>Zie ook
[De Media Services-blog](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)

## <a name="media-encoder-premium-workflow"></a>Media Encoder Premium Workflow
### <a name="overview"></a>Overzicht
[Introductie van Premium Encoding in Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

### <a name="how-to-use"></a>Gebruiksinstructies
Media Encoder Premium Workflow is geconfigureerd met behulp van complexe werkstromen. Werkstroombestanden kan worden gemaakt en bijgewerkt met de [Workflow Designer](media-services-workflow-designer.md) hulpprogramma.

[Het gebruik van Premium Encoding in Azure Media Services](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

### <a name="known-issues"></a>Bekende problemen
Als uw invoervideo geen bevat bevat ondertiteling, de uitvoer Asset nog steeds een leeg TTML-bestand.


## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Verwante artikelen:
* [Geavanceerde codering taken uitvoeren door aan te passen voorinstellingen voor Media Encoder Standard](media-services-custom-mes-presets-with-dotnet.md)
* [Quota en beperkingen](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
