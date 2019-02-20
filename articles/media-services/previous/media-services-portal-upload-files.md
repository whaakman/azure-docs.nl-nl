---
title: Bestanden uploaden naar een Media Services-account via Azure Portal | Microsoft Docs
description: In deze zelfstudie wordt stapsgewijs uitgelegd hoe u bestanden uploadt naar een Media Services-account via Azure Portal.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 3ad3dcea-95be-4711-9aae-a455a32434f6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/08/2019
ms.author: juliako
ms.openlocfilehash: 7730d15b0d3be9c0ffc7c7fbafe40fece713afbd
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55976789"
---
# <a name="upload-files-to-a-media-services-account-in-the-azure-portal-legacy"></a>Bestanden uploaden naar een Media Services-account via Azure Portal (verouderd)

> [!div class="op_single_selector"]
> * [Portal](media-services-portal-upload-files.md)
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> 
> [!NOTE]
> U hebt een Azure-account nodig om deze zelfstudie te voltooien. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie. 
> 

In Azure Media Services uploadt u de digitale bestanden naar een asset. De asset kan video, audio, afbeeldingen, verzamelingen van miniaturen, tekstsporen en ondertitelingsbestanden (en de metagegevens voor deze bestanden) bevatten. Nadat de bestanden zijn geüpload, wordt uw inhoud veilig opgeslagen in de cloud voor verdere verwerking en streaming.

> [!NOTE]
> Media Services heeft een maximale bestandsgrootte voor het verwerken van bestanden. Zie [Media Services quotas and limitations](media-services-quotas-and-limitations.md) (Quota en beperkingen voor Media Services) voor meer informatie over maximale bestandsgrootte.
>

## <a name="upload-files"></a>Bestanden uploaden
1. Selecteer uw Azure Media Services-account in [Azure Portal](https://portal.azure.com/).
2. Selecteer **Instellingen** > **Assets**. Selecteer de knop **Uploaden**.
   
    ![Bestanden uploaden](./media/media-services-portal-vod-get-started/media-services-upload.png)
   
    Het venster **Videoasset uploaden** wordt weergegeven.
   
   > [!NOTE]
   > Media Services heeft geen maximale bestandsgrootte voor het uploaden van video's.
 
3. Ga op uw computer naar de video die u wilt uploaden. Selecteer de video en selecteer vervolgens **OK**.  
   
    Het uploaden wordt gestart. U kunt de voortgang onder de bestandsnaam bekijken.  

Wanneer het uploaden is voltooid, wordt de nieuwe asset in het deelvenster **Assets** weergegeven. 

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [coderen van de geüploade assets](media-services-portal-encode.md).

* U kunt ook Azure Functions gebruiken om een coderingstaak te activeren wanneer er een bestand binnenkomt in de geconfigureerde container. Zie voor meer informatie het voorbeeld in [Media Services: Azure Media Services integreren met Azure Functions en Logic Apps](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/).


