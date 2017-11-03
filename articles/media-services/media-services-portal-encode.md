---
title: Een asset coderen met behulp van Media Encoder Standard in de Azure portal | Microsoft Docs
description: Deze zelfstudie wordt u door de stappen voor een asset coderen met behulp van Media Encoder Standard in de Azure-portal.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 107d9e9a-71e9-43e5-b17c-6e00983aceab
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: ae5f4fd391cbf62b41d1a65f1d8107cefe3a5df3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="encode-an-asset-by-using-media-encoder-standard-in-the-azure-portal"></a>Een asset coderen met behulp van Media Encoder Standard in de Azure-portal

> [!NOTE]
> U hebt een Azure-account nodig om deze zelfstudie te voltooien. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie. 
> 
> 

Een van de meest voorkomende scenario's voor het werken met Azure Media Services wordt gewerkt, adaptive bitrate streaming aan uw clients geleverd. Media Services ondersteunt de volgende adaptive bitrate streaming-technologieën: Apple HTTP Live Streaming (HLS), Smooth Streaming van Microsoft en dynamische adaptief streamen via HTTP (KOPPELTEKEN, ook wel MPEG-DASH genoemd). Als u uw video's voorbereiden voor adaptive bitrate streaming, moet u eerst de bronvideo coderen als multi-bitrate-bestanden. U kunt Azure Media Encoder Standard gebruiken voor het coderen van uw video's.  

Media Services biedt dynamische pakketten. Dynamische pakketten, kunt u uw multi-bitrate MP4s in HLS, Smooth Streaming en MPEG-DASH leveren zonder opnieuw te verpakken in deze indelingen. Als u dynamische pakketten gebruikt, kunt u deze kunt opslaan en betaalt voor de bestanden in één opslagindeling. Media Services bouwt en dient de juiste reactie op basis van de aanvraag van een client.

U moet het bronbestand coderen in een set multi-bitrate MP4-bestanden om van dynamische pakketten gebruik te maken. De coderingsstappen worden verderop in dit artikel uitgelegd.

Zie voor informatie over het schalen van de verwerking van de media, [schalen media verwerken met behulp van de Azure-portal](media-services-portal-scale-media-processing.md).

## <a name="encode-in-the-azure-portal"></a>Coderen in de Azure portal

Uw inhoud codeert met behulp van Media Encoder Standard:

1. Selecteer uw Azure Media Services-account in [Azure Portal](https://portal.azure.com/).
2. Selecteer **Instellingen** > **Assets**. Selecteer de asset die u wilt coderen.
3. Selecteer de knop **Coderen**.
4. Selecteer in het deelvenster **Een asset coderen** de processor **Media Encoder Standard** en een standaardinstelling. Zie [automatisch een bitrate ladder genereren](media-services-autogen-bitrate-ladder-with-mes.md) en [Standaardinstellingen voor taken in Media Encoder Standard](media-services-mes-presets-overview.md) voor informatie over standaardinstellingen. Het is belangrijk om de standaardinstelling te kiezen die het meest geschikt is voor uw invoervideo. Als u bijvoorbeeld weet dat uw invoervideo een resolutie van 1920 x 1080 pixels heeft, kunt u de standaardinstelling **H264 Multiple Bitrate 1080p** gebruiken. Als u een video met lage resolutie hebt (640 x 360), kunt u beter niet de standaardinstelling **H264 Multiple Bitrate 1080p** gebruiken.
   
   U kunt de naam van de uitvoerasset en de naam van de taak bewerken, zodat u uw resources beter kunt beheren.
   
   ![Assets coderen](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Selecteer **Maken**.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Volgende stappen
* [De voortgang van de coderingstaak](media-services-portal-check-job-progress.md) in de Azure portal.  

