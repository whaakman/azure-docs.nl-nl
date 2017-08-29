---
title: " Bestanden uploaden naar een Media Services-account via Azure Portal | Microsoft Docs"
description: In deze zelfstudie wordt stapsgewijs uitgelegd hoe u bestanden uploadt naar een Media Services-account via Azure Portal
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 3ad3dcea-95be-4711-9aae-a455a32434f6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: juliako
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: 8b9a7a266259077d49a38d51d7f801f420cc6315
ms.contentlocale: nl-nl
ms.lasthandoff: 08/08/2017

---
# <a name="upload-files-into-a-media-services-account-using-the-azure-portal"></a>Bestanden uploaden naar een Media Services-account via Azure Portal
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-upload-files.md)
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> 
> [!NOTE]
> U hebt een Azure-account nodig om deze zelfstudie te voltooien. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie. 
> 


In Media Services uploadt u de digitale bestanden naar (of neemt u deze op in) een asset. De asset kan video, audio, afbeeldingen, verzamelingen van miniaturen, tekstsporen en ondertitelingsbestanden (en de metagegevens voor deze bestanden) bevatten. Zodra de bestanden zijn geüpload, wordt uw inhoud veilig opgeslagen in de cloud voor verdere verwerking en streaming.


## <a name="upload-files"></a>Bestanden uploaden

>[!NOTE]
>Er is een limiet voor de maximale bestandsgrootte die wordt ondersteund voor verwerking in Media Services. Raadpleeg [dit onderwerp](media-services-quotas-and-limitations.md) voor meer informatie over de maximale bestandsgrootte.
>

1. Selecteer uw Azure Media Services-account in [Azure Portal](https://portal.azure.com/).
2. Klik in de blade **Instellingen** op **Assets**.
   
    ![Bestanden uploaden](./media/media-services-portal-vod-get-started/media-services-upload.png)
3. Klik op de knop **Uploaden**.
   
    Het venster **Videoasset uploaden** wordt weergegeven.
   
   > [!NOTE]
   > Er geldt geen beperking voor de bestandsgrootte.
   > 
   > 
4. Blader naar de gewenste video op uw computer, selecteer deze en klik op OK.  
   
    Het uploaden wordt gestart en u ziet de voortgang onder de bestandsnaam.  

Nadat het uploaden is voltooid, ziet u de nieuwe asset in het venster **Assets**. 

## <a name="next-steps"></a>Volgende stappen
U kunt nu de geüploade assets coderen. Zie [Assets coderen](media-services-portal-encode.md) voor meer informatie.

U kunt ook Azure Functions gebruiken om een coderingstaak te activeren op basis van een bestand dat binnenkomt in de geconfigureerde container. Zie [dit voorbeeld](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ) voor meer informatie.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


