---
title: Overzicht van Azure Media Services-Streaming-eindpunt | Microsoft Docs
description: In dit onderwerp geeft een overzicht van Azure Media Services streaming-eindpunten.
services: media-services
documentationcenter: 
author: Juliako
writer: juliako
manager: cfowler
editor: 
ms.assetid: 097ab5e5-24e1-4e8e-b112-be74172c2701
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: e454778c558b9c17c47ad9eb651737aa0b5e2605
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="streaming-endpoints-overview"></a>Streaming-eindpunten-overzicht 

##<a name="overview"></a>Overzicht

In Microsoft Azure Media Services (AMS), een **Streaming-eindpunt** vertegenwoordigt een streaming-service die inhoud rechtstreeks naar een clienttoepassing player of naar een inhoud Delivery Network (CDN) voor verdere distributie leveren kunt. Media Services biedt ook naadloze integratie van Azure CDN. De uitgaande stroom van een service StreamingEndpoint mag een live stream, video op aanvraag of progressief downloaden van de activa in uw Media Services-account. Elke Azure Media Services-account bevat standaard StreamingEndpoint. Aanvullende streaming-eindpunten kunnen worden gemaakt onder het account. Er zijn twee versies van streaming-eindpunten, 1.0 en 2.0. Vanaf januari 10 2017, bevatten nieuwe accounts AMS versie 2.0 **standaard** StreamingEndpoint. Aanvullende streaming-eindpunten die u aan dit account toevoegt worden ook versie 2.0. Deze wijziging heeft geen invloed op de bestaande accounts; bestaande streaming-eindpunten versie 1.0 zijn en kunnen worden bijgewerkt naar versie 2.0. Met deze wijziging zal er wijzigingen in gedrag, facturering en functie (Zie voor meer informatie de **Streaming typen en versies** sectie hieronder).

Bovendien de volgende eigenschappen beginnen met de versie van het 2.15 (uitgebracht in januari 2017), Azure Media Services toegevoegd aan de entiteit Streaming-eindpunt: **CdnProvider**, **CdnProfile**, **FreeTrialEndTime**, **StreamingEndpointVersion**. Zie voor een gedetailleerd overzicht van deze eigenschappen [dit](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

Wanneer u een Azure Media Services-account standaard streaming-eindpunt is gemaakt voor u in standaard maakt de **gestopt** status. U kunt het standaard streaming-eindpunt niet verwijderen. Afhankelijk van de beschikbaarheid van Azure CDN in de betreffende regio, standaard een nieuw gemaakt standaard streaming-eindpunt bevat ook 'StandardVerizon' CDN provider-integratie. 

>[!NOTE]
>Integratie van Azure CDN kan worden uitgeschakeld voordat u begint het streaming-eindpunt.

In dit onderwerp geeft een overzicht van de belangrijkste functies die worden geleverd door het streaming-eindpunten.

## <a name="streaming-types-and-versions"></a>Streaming-typen en versies

### <a name="standardpremium-types-version-20"></a>Standaard/Premium typen (versie 2.0)

Vanaf de release januari 2017 van Media Services kunt u twee streaming typen hebben: **standaard** en **Premium**. Deze typen maken deel uit van de Streaming-eindpunt versie '2.0'.

Type|Beschrijving
---|---
**Standard**|Dit is de standaardoptie voor het merendeel van de scenario's.<br/>Met deze optie, krijgt u vaste/beperkt SLA, eerste 15 dagen na het starten van het streaming-eindpunt is gratis.<br/>Als u meer dan één streaming-eindpunten, alleen de eerste gratis voor de eerste 15 dagen is, zijn de andere in rekening gebracht zodra ze worden gestart. <br/>Houd er rekening mee gratis proefversie is alleen van toepassing op de zojuist gemaakte media services-accounts en standaardstreaming-eindpunt. Bestaande streaming-eindpunten en bovendien gemaakte streaming-eindpunten niet gratis proefperiode bevat zelfs ze kunnen worden bijgewerkt naar versie 2.0 of ze zijn gemaakt als versie 2.0.
**Premium**|Deze optie is geschikt voor professionals scenario's waarvoor hogere schaal of het besturingselement.<br/>Variabele SLA die is gebaseerd op premium streaming-eenheid (SU) capaciteit hebt aangeschaft, speciale streaming-eindpunten bevinden zich in een geïsoleerde omgeving en niet van invloed zijn voor bronnen.

Voor meer informatie gedetailleerde, Zie de **vergelijken Streaming typen** volgende sectie.

### <a name="classic-type-version-10"></a>Klassieke type (versie 1.0)

Voor gebruikers die AMS accounts vóór de release van januari 10 2017 gemaakt, die u hebt een **klassieke** type van een streaming-eindpunt. Dit type maakt deel uit van de versie voor streaming endpoint "1.0".

Als uw **versie "1.0"** streaming-eindpunt is > = 1 premium streaming-eenheden (SU), deze worden premium streaming-eindpunt en bieden alle AMS-functies (net als bij de **standaard/Premium** type) zonder aanvullende configuratiestappen.

>[!NOTE]
>**Klassieke** streaming-eindpunten (versie "1.0" en 0 SU), biedt beperkte onderdelen en een SLA niet opgenomen. Het verdient aanbeveling om te migreren naar **standaard** type voor een betere ervaring met en gebruik van functies zoals dynamische pakketten of -versleuteling en andere functies die worden geleverd met de **standaard** type. Om te migreren naar de **standaard** type, gaat u naar de [Azure-portal](https://portal.azure.com/) en selecteer **Opt-in op standaard**. Zie voor meer informatie over de migratie van de [migratie](#migration-between-types) sectie.
>
>Houd er rekening mee dat deze bewerking kan niet worden teruggedraaid en gevolgen voor de prijscategorie heeft.
>
 
## <a name="comparing-streaming-types"></a>Streaming typen vergelijken

### <a name="versions"></a>Versies

|Type|StreamingEndpointVersion|ScaleUnits|CDN|Facturering|SLA| 
|--------------|----------|-----------------|-----------------|-----------------|-----------------|    
|Klassiek|1.0|0|N.v.t.|Gratis|N.v.t.|
|Standard streaming-eindpunt|2.0|0|Ja|Betaald|Ja|
|Premium streamingeenheden|1.0|>0|Ja|Betaald|Ja|
|Premium streamingeenheden|2.0|>0|Ja|Betaald|Ja|

### <a name="features"></a>Functies

Functie|Standard|Premium
---|---|---
Eerste 15 dagen gratis| Ja |Nee
Doorvoer |Maximaal 600 Mbps wanneer Azure CDN wordt niet gebruikt. Kan worden geschaald met CDN.|200 Mbps per streaming-eenheid (SU). Kan worden geschaald met CDN.
SLA | 99.9|99,9 (200 Mbps per SU).
CDN|Azure CDN van derden CDN of er is geen CDN.|Azure CDN van derden CDN of er is geen CDN.
Facturering is verdeeld.| Dagelijks|Dagelijks
Dynamische versleuteling|Ja|Ja
Dynamische verpakking|Ja|Ja
Schalen|Automatisch kan worden geschaald tot de betreffende doorvoer.|Aanvullende streaming-eenheden
IP-filtering/G20/aangepast host|Ja|Ja
Progressief downloaden|Ja|Ja
Aanbevolen gebruik |Aanbevolen voor de meeste scenario's voor streamen.|Het gebruik van Professional.<br/>Als u denkt dat wellicht u behoeften buiten standaard. Contact met ons opnemen (amsstreaming op microsoft.com) als u verwacht de grootte van een gelijktijdige doelgroep groter is dan 50.000 viewers dat.


## <a name="migration-between-types"></a>Migratie tussen typen

Van | Tot | Actie
---|---|---
Klassiek|Standard|Moeten zich aanmelden
Klassiek|Premium| Schaal (extra streaming-eenheden)
Standaard/Premium|Klassiek|Niet beschikbaar (als streaming endpoint versie 1.0 is. Het is toegestaan te wijzigen naar de klassieke bij het instellen van scaleunits op '0')
Standard (met/zonder CDN)|Premium met dezelfde configuratie|Toegestaan in de **gestart** status. (via Azure portal)
Premium (met/zonder CDN)|Standard met dezelfde configuratie|Toegestaan in de **gestart** status (via Azure portal)
Standard (met/zonder CDN)|Premium met andere configuratie|Toegestaan in de **gestopt** status (via Azure portal). Niet toegestaan in de actieve status.
Premium (met/zonder CDN)|Standard met andere configuratie|Toegestaan in de **gestopt** status (via Azure portal). Niet toegestaan in de actieve status.
Versie 1.0 met SU > = 1 met CDN|Standaard/Premium met geen CDN|Toegestaan in de **gestopt** status. Niet toegestaan in de **gestart** status.
Versie 1.0 met SU > = 1 met CDN|Standard met/zonder CDN|Toegestaan in de **gestopt** status. Niet toegestaan in de **gestart** status. Versie 1.0 van het CDN zal worden verwijderd en nieuwe gemaakt en gestart.
Versie 1.0 met SU > = 1 met CDN|Premium met/zonder CDN|Toegestaan in de **gestopt** status. Niet toegestaan in de **gestart** status. Klassieke CDN worden verwijderd en nieuwe één gemaakt en gestart.

## <a name="next-steps"></a>Volgende stappen
Media Services-leertrajecten bekijken.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

