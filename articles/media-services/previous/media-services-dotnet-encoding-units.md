---
title: Schalen mediaverwerking door toe te voegen encoding-eenheden - Azure |  Microsoft Docs
description: Meer informatie over het toevoegen van encoding-eenheden met .NET
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 33f7625a-966a-4f06-bc09-bccd6e2a42b5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/05/2018
ms.author: juliako;milangada;
ms.openlocfilehash: 8f17d5e6d45b678f5c4a0c4318e74a18c42ff0c8
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51035713"
---
# <a name="how-to-scale-encoding-with-net-sdk"></a>Codering schalen met .NET SDK
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-scale-media-processing.md)
> * [.NET](media-services-dotnet-encoding-units.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a>Overzicht
> [!IMPORTANT]
> Controleer de [overzicht](media-services-scale-media-processing-overview.md) voor meer informatie over het mediaverwerking schalen.
> 
> 

Als u wilt wijzigen van de gereserveerde-eenheidstype en het aantal gereserveerde coderingseenheden met .NET SDK, het volgende doen:

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);

    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();

    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

## <a name="opening-a-support-ticket"></a>Een Ondersteuningsticket openen

Standaard kan elke Media Services-account worden geschaald tot maximaal 10 S2 of S3 gereserveerde Media-eenheden (groepsbeleidsinstelling) of 25 S1 groepsbeleidsinstelling en 5 On-Demand Streaming gereserveerde eenheden. U kunt een hogere limiet aanvragen door een ondersteuningsticket te openen.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

