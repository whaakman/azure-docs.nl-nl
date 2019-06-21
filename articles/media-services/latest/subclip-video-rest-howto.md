---
title: Subclip een video als codering met Azure Media Services REST API
description: In dit onderwerp wordt beschreven hoe u een video subclip wanneer codering met Azure Media Services met behulp van REST
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/10/2019
ms.author: juliako
ms.openlocfilehash: df8c8a4040b4aae4379b4bfe0e9a16337588dd1b
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67305190"
---
# <a name="subclip-a-video-when-encoding-with-media-services---rest"></a>Subclip een video als codering met Media Services - REST

U kunt trim of een video subclip wanneer codering met behulp van een [taak](https://docs.microsoft.com/rest/api/media/jobs). Deze functionaliteit werkt met een [transformeren](https://docs.microsoft.com/rest/api/media/transforms) die is gebouwd met behulp van de [BuiltInStandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) presets, of de [StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) voorinstellingen. 

De REST-voorbeeld in dit onderwerp wordt een taak die een video worden verwijderd omdat deze een coderingstaak indient. 

## <a name="prerequisites"></a>Vereisten

Als u wilt de in dit onderwerp beschreven stappen hebt voltooid, hebt u naar:

- [Maak een Azure Media Services-account](create-account-cli-how-to.md).
- [Postman configureren voor Azure Media Services REST API-aanroepen](media-rest-apis-with-postman.md).
    
    Zorg ervoor dat u de laatste stap in het onderwerp [Azure AD Token ophalen](media-rest-apis-with-postman.md#get-azure-ad-token). 
- Maken van een transformatie- en uitvoer activa. U kunt zien over het maken van een transformatie- en uitvoer activa in de [een extern bestand op basis van URL coderen en streamen van de video - REST](stream-files-tutorial-with-rest.md) zelfstudie.
- Controleer de [Encoding concept](encoding-concept.md) onderwerp.

## <a name="create-a-subclipping-job"></a>Een taak sub-clipping maken

1. Selecteer in de Postman-verzameling die u hebt gedownload, **transformaties en taken** -> **taak maken met knippen Sub**.
    
    De **plaatsen** aanvraag er als volgt uitzien:
    
    ```
    https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/transforms/:transformName/jobs/:jobName?api-version={{api-version}}
    ```
1. Werk de waarde van "transformName" omgevingsvariabele met de transformatienaam van de. 
1. Selecteer de **hoofdtekst** tabblad en de 'myOutputAsset' bijwerken met de uitvoer de naam van de Asset.

    ```
    {
      "properties": {
        "description": "A Job with transform cb9599fb-03b3-40eb-a2ff-7ea909f53735 and single clip.",
       
        "input": {
          "@odata.type": "#Microsoft.Media.JobInputHttp",
          "baseUri": "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/",
          "files": [
                "Ignite-short.mp4"
            ],
          "start": {
            "@odata.type": "#Microsoft.Media.AbsoluteClipTime",
            "time": "PT10S"
          },
          "end": {
            "@odata.type": "#Microsoft.Media.AbsoluteClipTime",
            "time": "PT40S"
          }
        },
      
        "outputs": [
          {
            "@odata.type": "#Microsoft.Media.JobOutputAsset",
            "assetName": "myOutputAsset"
          }
        ],
        "priority": "Normal"
      }
    }
    ```
1. Druk op **Verzenden**.

    U ziet de **antwoord** met de informatie over een taak die is gemaakt en verzonden en status van de taak. 

## <a name="next-steps"></a>Volgende stappen

[Coderen met een aangepaste transformatie](custom-preset-rest-howto.md) 
