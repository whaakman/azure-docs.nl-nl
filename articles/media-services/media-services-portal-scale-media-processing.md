---
title: Schaal media verwerking met de Azure portal | Microsoft Docs
description: Deze zelfstudie leert u de stappen voor het vergroten/verkleinen media verwerken met behulp van de Azure-portal.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: e500f733-68aa-450c-b212-cf717c0d15da
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: d2312803a4471e207d3696ca8350a86e3c4761e6
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="change-the-reserved-unit-type"></a>Het gereserveerde eenheidstype wijzigen
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [Portal](media-services-portal-scale-media-processing.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

> [!NOTE]
> Om de nieuwste versie van Java SDK en te beginnen met ontwikkelen met behulp van Java, Zie [aan de slag met de SDK voor Java-clients voor Media Services](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use). <br/>
> Voor het downloaden van de meest recente PHP SDK voor Media Services, zoekt u versie 0.5.7 van het pakket Microsoft/WindowAzure in de [Packagist opslagplaats](https://packagist.org/packages/microsoft/windowsazure#v0.5.7).  

## <a name="overview"></a>Overzicht

Media Services-accounts worden gekoppeld aan een gereserveerde-eenheidstype, waarmee wordt bepaald hoe snel de mediaverwerkingstaken worden verwerkt. U kunt kiezen uit de volgende gereserveerde-eenheidstypen: **S1**, **S2** en **S3**. Een coderingstaak wordt bijvoorbeeld sneller uitgevoerd wanneer u het gereserveerde-eenheidstype **S2** gebruikt (in vergelijking met het type **S1**).

Naast het opgeven van het gereserveerde-eenheidstype kunt u opgeven dat uw account moet worden ingericht met **gereserveerde eenheden** (RUs). Op basis van het aantal ingerichte RU's wordt bepaald hoeveel mediataken tegelijk kunnen worden verwerkt voor een bepaald account.

>[!NOTE]
>RU's werken voor het parallel verwerken van alle media, waaronder indexeringstaken via Azure Media Indexer. Indexeringstaken worden echter niet sneller verwerkt met snellere gereserveerde eenheden, terwijl dit bij coderen wel het geval is.

> [!IMPORTANT]
> Leest de [overzicht](media-services-scale-media-processing-overview.md) onderwerp voor meer informatie over het schalen van media onderwerp verwerken.
> 
> 

## <a name="scale-media-processing"></a>Mediaverwerking schalen
Als u wilt wijzigen van het type gereserveerde eenheid en het aantal gereserveerde eenheden, het volgende doen:

1. Selecteer uw Azure Media Services-account in [Azure Portal](https://portal.azure.com/).
2. In de **instellingen** Selecteer **gereserveerde Media-eenheden**.
   
    U kunt het aantal gereserveerde eenheden voor het type geselecteerde gereserveerde eenheid wijzigen met de **geleverd Media-eenheden** schuifregelaar.
   
    Wijzigen van de **gereserveerde EENHEIDSTYPE**, drukt u op S1, S2 of S3.
   
    ![Pagina processors](./media/media-services-portal-scale-media-processing/media-services-scale-media-processing.png)
3. Klik op de knop OPSLAAN om uw wijzigingen op te slaan.
   
    De nieuwe gereserveerde eenheden zijn toegewezen wanneer u op opslaan.

## <a name="next-steps"></a>Volgende stappen
Media Services-leertrajecten bekijken.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

