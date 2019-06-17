---
title: Overzicht van Azure Media Services Streaming-eindpunt | Microsoft Docs
description: In dit onderwerp biedt een overzicht van Azure Media Services streaming-eindpunten.
services: media-services
documentationcenter: ''
author: Juliako
writer: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: a45e2af6f2cb9c105c084585a03a6de615fa1397
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64573037"
---
# <a name="streaming-endpoints-overview"></a>Overzicht van streaming eindpunten  

> [!NOTE]
> Er worden geen nieuwe functies of functionaliteit meer aan Media Services v2. toegevoegd. <br/>Maak kennis met de nieuwste versie, [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/). Zie ook [hulp bij de migratie van v2 naar v3](../latest/migrate-from-v2-to-v3.md)

In Microsoft Azure Media Services (AMS), een **Streaming-eindpunt** vertegenwoordigt een streamingservice waarmee inhoud kan worden geleverd rechtstreeks naar een clientafspeeltoepassing of aan een Content Delivery Network (CDN) voor verdere distributie. Media Services biedt ook een naadloze integratie van Azure CDN. De uitgaande stroom van een service streamingendpoint zo mag een live stream, een video op aanvraag of progressief downloaden van uw activa in Media Services-account. Elk Azure Media Services-account is inclusief een standaard streamingendpoint zo. Extra door kunnen worden gemaakt onder het account. Er zijn twee versies van door, 1.0 en 2.0. Beginnen met 10 januari-2017, bevatten nieuwe AMS-accounts versie 2.0 **standaard** streamingendpoint zo. Aanvullende streaming-eindpunten die u aan dit account toevoegt worden ook versie 2.0. Deze wijziging heeft geen gevolgen voor de bestaande accounts; bestaande door versie 1.0 zijn en kunnen worden bijgewerkt naar versie 2.0. Met deze wijziging zullen er wijzigingen in gedrag, facturering en -functie (Zie voor meer informatie de **Streaming typen en versies** sectie hieronder beschreven).

De volgende eigenschappen Azure Media Services toegevoegd aan de entiteit Streaming-eindpunt: **CdnProvider**, **CdnProfile**, **FreeTrialEndTime**, **StreamingEndpointVersion**. Zie voor een gedetailleerd overzicht van deze eigenschappen [dit](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

Wanneer u een Azure Media Services-account standaard standard streaming-eindpunt is gemaakt voor u maakt de **gestopt** staat. U kunt het standaardstreaming-eindpunt niet verwijderen. Afhankelijk van de beschikbaarheid van Azure CDN in de betreffende regio, wordt standaard een nieuw gemaakt standaard streaming-eindpunt bevat ook 'StandardVerizon' CDN provider-integratie. 
                
> [!NOTE]
> Integratie van Azure CDN kan worden uitgeschakeld voordat u begint met het streaming-eindpunt. De `hostname` en de streaming-URL blijft hetzelfde, ongeacht of u CDN inschakelt of niet.

In dit onderwerp biedt een overzicht van de belangrijkste functies die worden geleverd door het streaming-eindpunten.

## <a name="naming-conventions"></a>Naamconventies

Voor het standaardeindpunt: `{AccountName}.streaming.mediaservices.windows.net`

Voor elke extra eindpunten: `{EndpointName}-{AccountName}.streaming.mediaservices.windows.net`

## <a name="streaming-types-and-versions"></a>Streaming-typen en versies

### <a name="standardpremium-types-version-20"></a>Standard/Premium-typen (versie 2.0)

Beginnen met de release van januari 2017 van Media Services, hebt u twee streaming typen: **Standard** (preview) en **Premium**. Deze typen zijn onderdeel van de Streaming-eindpunt versie '2.0'.


|Type|Description|
|--------|--------|  
|**Standard**|De Streaming-eindpunt wordt standaard een **Standard** typt, kan worden gewijzigd in het type Premium door aan te passen van streaming-eenheden.|
|**Premium** |Deze optie is geschikt voor professionele scenario's waarvoor een hogere schaal of het besturingselement. U verplaatst naar een **Premium** type door aan te passen van streaming-eenheden.<br/>Toegewezen Streaming-eindpunten bevinden zich in de geïsoleerde omgeving en niet concurreren om bronnen.|

Voor klanten die voor het leveren van inhoud naar grote doelgroepen van het internet, wordt u aangeraden dat u de CDN op het Streaming-eindpunt inschakelen.

Zie voor meer informatie, de [vergelijken Streaming typen](#comparing-streaming-types) volgende sectie.

### <a name="classic-type-version-10"></a>Klassieke type (versie 1.0)

Voor gebruikers die vóór de release van januari, 10 2017 AMS-accounts hebt gemaakt, hebt u een **klassieke** type van een streaming-eindpunt. Dit type maakt deel uit van de versie voor streaming endpoint "1.0".

Als uw **versie "1.0"** streaming-eindpunt is > = 1 premium-streaming-eenheden (SU), het premium-streaming-eindpunt is en wordt alle AMS-functies (net als bij de **Standard/Premium** type) zonder eventuele aanvullende configuratiestappen.

>[!NOTE]
>**Klassieke** streaming-eindpunten (versie "1.0" en 0 SU), biedt beperkte functies en bevat geen SLA. Het verdient aanbeveling om te migreren naar **Standard** type om een betere ervaring en het gebruik van functies zoals dynamische pakketten of -versleuteling en andere functies die worden geleverd met de **Standard** type. Om te migreren naar de **Standard** type, Ga naar de [Azure-portal](https://portal.azure.com/) en selecteer **Opt-in standaard**. Zie voor meer informatie over het migreren van de [migratie](#migration-between-types) sectie.
>
>Houd er rekening mee dat deze bewerking kan niet worden teruggedraaid en invloed op de prijzen heeft.
>
 
## <a name="comparing-streaming-types"></a>Streaming typen vergelijken

### <a name="versions"></a>Versies

|Type|StreamingEndpointVersion|ScaleUnits|CDN|Billing|
|--------------|----------|-----------------|-----------------|-----------------|
|Klassiek|1.0|0|N.V.T.|Gratis|
|Standard Streaming-eindpunt (preview)|2.0|0|Ja|Betaald|
|Premium streamingeenheden|1.0|>0|Ja|Betaald|
|Premium streamingeenheden|2.0|>0|Ja|Betaald|

### <a name="features"></a>Functies

Functie|Standard|Premium
---|---|---
Eerste 15 dagen gratis <sup>1</sup>| Ja |Nee
Doorvoer |Maximaal 600 Mbps en krijgt een veel hogere effectieve doorvoer wanneer een CDN wordt gebruikt.|200 Mbps per streaming-eenheid (SU). Krijgt u een veel hogere effectieve doorvoer wanneer een CDN wordt gebruikt.
CDN|Azure CDN, van derden CDN of er is geen CDN.|Azure CDN, van derden CDN of er is geen CDN.
Facturering is Pro rata| Dagelijks|Dagelijks
Dynamische versleuteling|Ja|Ja
Dynamische verpakking|Ja|Ja
Schalen|Automatisch omhoog kan worden opgeschaald naar de betreffende doorvoer.|Aanvullende streaming-eenheden.
IP-filtering/G20/aangepast host <sup>2</sup>|Ja|Ja
Progressief downloaden|Ja|Ja
Aanbevolen gebruik |Aanbevolen voor de meeste streaming-scenario's.|Professionele gebruik. 

<sup>1</sup> de gratis proefversie alleen van toepassing op nieuwe media services-accounts en het standaard Streaming-eindpunt.<br/>
<sup>2</sup> alleen rechtstreeks op het Streaming-eindpunt wordt gebruikt wanneer het CDN niet is ingeschakeld op het eindpunt.<br/>

Voor SLA-informatie, Zie [prijzen en SLA](https://azure.microsoft.com/pricing/details/media-services/).

## <a name="migration-between-types"></a>Migratie tussen verschillende typen

Van | Naar | Bewerking
---|---|---
Klassiek|Standard|Moet u aanmelden
Klassiek|Premium| Schalen (extra streaming-eenheden)
Standard/Premium|Klassiek|Niet beschikbaar (als streaming endpoint versie 1.0 is. Het is toegestaan om te wijzigen in klassiek bij het instellen van scaleunits op '0')
Standard (met/zonder CDN)|Premium met dezelfde configuratie|Toegestaan in de **gestart** staat. (via Azure portal)
Premium (met/zonder CDN)|Standard met dezelfde configuratie|Toegestaan in de **gestart** status (via Azure portal)
Standard (met/zonder CDN)|Premium met andere configuratie|Toegestaan in de **gestopt** status (via Azure portal). Niet toegestaan in de status running doorbrengt.
Premium (met/zonder CDN)|Standard met andere configuratie|Toegestaan in de **gestopt** status (via Azure portal). Niet toegestaan in de status running doorbrengt.
Versie 1.0 met SU > = 1 met CDN|Standard/Premium met geen CDN|Toegestaan in de **gestopt** staat. Niet toegestaan in de **gestart** staat.
Versie 1.0 met SU > = 1 met CDN|Standard met/zonder CDN|Toegestaan in de **gestopt** staat. Niet toegestaan in de **gestart** staat. Versie 1.0 CDN worden verwijderd en nieuwe één gemaakt en gestart.
Versie 1.0 met SU > = 1 met CDN|Premium met/zonder CDN|Toegestaan in de **gestopt** staat. Niet toegestaan in de **gestart** staat. Klassieke CDN worden verwijderd en nieuwe één gemaakt en gestart.

## <a name="next-steps"></a>Volgende stappen
Media Services-leertrajecten bekijken.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

