---
title: Mediaverwerking schalen met Azure portal | Microsoft Docs
description: Deze zelfstudie leert u de stappen voor het vergroten/verkleinen mediaverwerking met behulp van de Azure portal.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: e500f733-68aa-450c-b212-cf717c0d15da
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/05/2018
ms.author: juliako
ms.openlocfilehash: 2aabdc503a694de46796b7c3afeedfcdb2f39ca9
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51037859"
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

## <a name="overview"></a>Overzicht

Media Services-accounts worden gekoppeld aan een gereserveerde-eenheidstype, waarmee wordt bepaald hoe snel de mediaverwerkingstaken worden verwerkt. U kunt kiezen uit de volgende gereserveerde-eenheidstypen: **S1**, **S2** en **S3**. Een coderingstaak wordt bijvoorbeeld sneller uitgevoerd wanneer u het gereserveerde-eenheidstype **S2** gebruikt (in vergelijking met het type **S1**).

Naast het opgeven van het gereserveerde-eenheidstype kunt u opgeven dat uw account moet worden ingericht met **gereserveerde eenheden** (RUs). Op basis van het aantal ingerichte RU's wordt bepaald hoeveel mediataken tegelijk kunnen worden verwerkt voor een bepaald account.

>[!NOTE]
>RU's werken voor het parallel verwerken van alle media, waaronder indexeringstaken via Azure Media Indexer. Indexeringstaken worden echter niet sneller verwerkt met snellere gereserveerde eenheden, terwijl dit bij coderen wel het geval is.

> [!IMPORTANT]
> Controleer de [overzicht](media-services-scale-media-processing-overview.md) onderwerp voor meer informatie over het onderwerp mediaverwerking schalen.
> 
> 

## <a name="scale-media-processing"></a>Mediaverwerking schalen
Als u wilt wijzigen van de gereserveerde-eenheidstype en het aantal gereserveerde eenheden, het volgende doen:

1. Selecteer uw Azure Media Services-account in [Azure Portal](https://portal.azure.com/).
2. In de **instellingen** venster **gereserveerde Media-eenheden**.
   
    U kunt het aantal gereserveerde eenheden voor de geselecteerde gereserveerde-eenheidstype wijzigen met de **geleverd Media-eenheden** schuifregelaar aan de bovenkant van het scherm.
   
    Wijzigen van de **gereserveerde-EENHEIDSTYPE**, klikt u op de **snelheid van de gereserveerde verwerkingseenheden** balk. Selecteer de prijscategorie die u nodig hebt: S1, S2 of S3.
   
3. Klik op de knop OPSLAAN om uw wijzigingen op te slaan.
   
    De nieuwe gereserveerde eenheden worden toegewezen als u druk op opslaan.

## <a name="next-steps"></a>Volgende stappen
Media Services-leertrajecten bekijken.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

