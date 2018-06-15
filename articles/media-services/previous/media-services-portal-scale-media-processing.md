---
title: Schaal media verwerking met de Azure portal | Microsoft Docs
description: Deze zelfstudie leert u de stappen voor het vergroten/verkleinen media verwerken met behulp van de Azure-portal.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: e500f733-68aa-450c-b212-cf717c0d15da
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: 51973916c97282ac93032ab833402d9d1356647e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33790312"
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
> Zie [Aan de slag met de SDK voor Java-clients voor Azure Media Services](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use) om de nieuwste versie van de Java-SDK op te halen en te ontwikkelen met Java. <br/>
> Als u de nieuwste PHP-SDK voor Media Services wilt downloaden, zoekt u versie 0.5.7 van het Microsoft/WindowAzure-pakket in de [Packagist-opslagplaats](https://packagist.org/packages/microsoft/windowsazure#v0.5.7).  

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
   
    U kunt het aantal gereserveerde eenheden voor het type geselecteerde gereserveerde eenheid wijzigen met de **geleverd Media-eenheden** schuifregelaar aan de bovenkant van het scherm.
   
    Wijzigen van de **gereserveerde EENHEIDSTYPE**, klikt u op de **snelheid van gereserveerde verwerkingseenheden** balk. Selecteer de prijscategorie die u nodig: S1, S2 of S3.
   
3. Klik op de knop OPSLAAN om uw wijzigingen op te slaan.
   
    De nieuwe gereserveerde eenheden zijn toegewezen wanneer u op opslaan.

## <a name="next-steps"></a>Volgende stappen
Media Services-leertrajecten bekijken.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

