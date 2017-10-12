---
title: Overzicht van Azure Media Services | Microsoft Docs
description: Dit onderwerp bevat een overzicht van Azure Media Services
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 7a5e9723-c379-446b-b4d6-d0e41bd7d31f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/04/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 2a175aed40b9775d9a4de6877eb3467b43819568
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-media-services-overview"></a>Overzicht van Azure Media Services 

Microsoft Azure Media Services is een uitbreidbaar cloudplatform waarmee ontwikkelaars schaalbare toepassingen voor mediabeheer en -levering kunnen ontwikkelen. Media Services is gebaseerd op de REST API's waarmee u veilig video- of audio-inhoud kunt uploaden, opslaan, coderen en verpakken, zowel voor levering on demand als levering via livestreaming aan verschillende clients (bijvoorbeeld tv, pc en mobiele apparaten).

U kunt end-to-end-werkstromen volledig met Media Services bouwen. U kunt er ook voor kiezen om onderdelen van derde partijen voor sommige onderdelen van uw werkstroom te gebruiken. U kunt bijvoorbeeld coderen met een coderingsprogramma van een derde partij. Vervolgens kunt u uploaden, beveiligen, verpakken en leveren met Media Services.

U kunt uw inhoud live streamen of on-demand leveren. Het onderwerp bevat ook koppelingen naar andere relevante onderwerpen.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
U kunt hier de AMS-leertrajecten bekijken:

* [Werkstroom voor AMS Live Streaming](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
* [Werkstroom voor AMS On-Demand Streaming](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

## <a name="prerequisites"></a>Vereisten

Als u Azure Media Services wilt gaan gebruiken, moet u over het volgende beschikken:

* Een Azure-account. Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com) voor meer informatie.
* Een Azure Media Services-account. Zie [Een account maken](media-services-portal-create-account.md) voor meer informatie.
* (Optioneel) Instellen van de ontwikkelomgeving. Kies .NET of REST API voor uw ontwikkelomgeving. Zie [De omgeving instellen](media-services-dotnet-how-to-use.md) voor meer informatie.

    Leer ook hoe u via [programmacode verbinding kunt maken met de AMS-API](media-services-use-aad-auth-to-access-ams-api.md).
* Een streaming-eindpunt (Standard of Premium) in de status Gestart.  Zie [Managing streaming endpoints](media-services-portal-manage-streaming-endpoints.md) (Streaming-eindpunten beheren) voor meer informatie.

## <a name="sdks-and-tools"></a>SDK's en hulpprogramma's

Als u Media Services-oplossingen wilt maken, kunt u het volgende gebruiken:

* [Media Services REST API](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference)
* Een van de beschikbare client-SDK's:
    * [Azure Media Services SDK voor .NET](https://github.com/Azure/azure-sdk-for-media-services)
    * [Azure SDK voor Java](https://github.com/Azure/azure-sdk-for-java)
    * [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php)
    * [Azure Media Services voor Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Dit is een niet-Microsoft-versie van een Node.js SDK. Deze wordt onderhouden door een community en biedt nog geen 100% dekking voor AMS API's).
* Bestaande hulpprogramma's:
    * [Azure Portal](https://portal.azure.com/)
    * [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) is een Winforms-/C#-toepassing voor Windows)

## <a name="concepts-and-overview"></a>Concepten en overzicht
Zie [Concepten](media-services-concepts.md) voor Azure Media Services-concepten.

Zie [Stapsgewijze zelfstudies voor Azure Media Services](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series) voor een reeks procedures waarin u kennis kunt maken met de belangrijkste onderdelen van Azure Media Services. Deze reeks biedt een goed overzicht van de concepten en maakt gebruik van het AMSE-hulpprogramma om AMS-taken te demonstreren. Het AMSE-hulpprogramma is een Windows-hulpprogramma. Dit hulpprogramma ondersteunt de meeste taken die u programmatisch kunt uitvoeren met [AMS SDK voor .NET](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK voor Java](https://github.com/Azure/azure-sdk-for-java) of [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php).

## <a name="supported-scenarios-and-availability-of-media-services-across-data-centers"></a>Ondersteunde scenario's en de beschikbaarheid van Media Services in datacenters

Zie voor gedetailleerde informatie [Scenarios and availability of Media Services features across datacenters](scenarios-and-availability.md) (Scenario's en beschikbaarheid van Media Services-functies via datacenters).

## <a name="service-level-agreement-sla"></a>Service Level Agreement (SLA)

* Voor Media Services-codering wordt een beschikbaarheid van 99,9% voor REST API-transacties gegarandeerd.
* Wanneer er een Standard- of Premium-streaming-eindpunt is gekocht, worden service-aanvragen voor streaming beantwoord met een beschikbaarheidsgarantie van 99,9% voor bestaande media-inhoud.
* Voor livekanalen wordt gegarandeerd dat de actieve kanalen minimaal 99,9% van de tijd over een externe verbinding beschikken.
* Voor Content Protection wordt gegarandeerd dat de sleutelaanvragen minimaal 99,9% van de tijd worden vervuld.
* Voor de indexeerfunctie worden de aanvragen voor indexeertaken 99,9% van de tijd verwerkt met een gereserveerde eenheid codering.

Zie [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/) voor meer informatie.

Zie de sectie [Beschikbaarheid](scenarios-and-availability.md#availability) voor informatie over beschikbaarheid in datacenters.

## <a name="support"></a>Ondersteuning

[Ondersteuning van Azure](https://azure.microsoft.com/support/options/) biedt ondersteuningsopties voor Azure, met inbegrip van Media Services.

## <a name="provide-feedback"></a>Feedback geven

[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
