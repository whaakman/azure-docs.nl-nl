---
title: Een asset coderen met behulp van Media Encoder Standard in Azure portal | Microsoft Docs
description: Deze zelfstudie leert u de stappen van een asset coderen met behulp van Media Encoder Standard in Azure portal.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 107d9e9a-71e9-43e5-b17c-6e00983aceab
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: juliako
ms.openlocfilehash: 958c53108c024cb349922a1bd10b2cdc2dba41a3
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50247279"
---
# <a name="encode-an-asset-by-using-media-encoder-standard-in-the-azure-portal"></a>Een asset coderen met behulp van Media Encoder Standard in Azure portal

> [!NOTE]
> U hebt een Azure-account nodig om deze zelfstudie te voltooien. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie. 
> 
> 

Een van de meest voorkomende scenario's in het werken met Azure Media Services is adaptive bitrate streaming aan uw clients geleverd. Media Services ondersteunt de volgende adaptive bitrate streaming-technologieën: Apple HTTP Live Streaming (HLS), Smooth Streaming van Microsoft en Dynamic Adaptive Streaming via HTTP (DASH, ook wel MPEG-DASH genoemd). Om voor te bereiden uw video's voor adaptive bitratestreaming, moet u eerst de bronvideo coderen als multi-bitrate-bestanden. U kunt Azure Media Encoder Standard gebruiken om te coderen van uw video's.  

Media Services biedt dynamische pakketten. Met dynamische verpakking, kunt u uw multi-bitrate MP4s in HLS, Smooth Streaming en MPEG-DASH, leveren zonder opnieuw te verpakken in deze streaming-indelingen. Wanneer u dynamische pakketten, kunt u opslaan en betaalt voor de bestanden in één opslagindeling. Media Services bouwt en levert de juiste reactie op basis van de aanvraag van een client.

U moet het bronbestand coderen in een set multi-bitrate MP4-bestanden om van dynamische pakketten gebruik te maken. De coderingsstappen worden verderop in dit artikel.

Zie voor meer informatie over mediaverwerking schalen, [met behulp van de Azure-portal mediaverwerking schalen](media-services-portal-scale-media-processing.md).

## <a name="encode-in-the-azure-portal"></a>Coderen in Azure portal

Uw inhoud codeert met behulp van Media Encoder Standard:

1. Selecteer uw Azure Media Services-account in [Azure Portal](https://portal.azure.com/).
2. Selecteer **Instellingen** > **Assets**. Selecteer de asset die u wilt coderen.
3. Selecteer de knop **Coderen**.
4. Selecteer in het deelvenster **Een asset coderen** de processor **Media Encoder Standard** en een standaardinstelling. Zie [automatisch een bitrate ladder genereren](media-services-autogen-bitrate-ladder-with-mes.md) en [Standaardinstellingen voor taken in Media Encoder Standard](media-services-mes-presets-overview.md) voor informatie over standaardinstellingen. Het is belangrijk om de standaardinstelling te kiezen die het meest geschikt is voor uw invoervideo. Als u bijvoorbeeld weet dat uw invoervideo een resolutie van 1920 x 1080 pixels heeft, kunt u de standaardinstelling **H264 Multiple Bitrate 1080p** gebruiken. Als u een video met lage resolutie hebt (640 x 360), kunt u beter niet de standaardinstelling **H264 Multiple Bitrate 1080p** gebruiken.
   
   U kunt de naam van de uitvoerasset en de naam van de taak bewerken, zodat u uw resources beter kunt beheren.
   
   ![Assets coderen](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Selecteer **Maken**.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Volgende stappen
* [De voortgang van de coderingstaak](media-services-portal-check-job-progress.md) in Azure portal.  

