---
title: Hybride ontwerp van DRM subsystem(s) met Azure Media Services | Microsoft Docs
description: Dit onderwerp wordt besproken hybride ontwerp van DRM subsystem(s) met Azure Media Services.
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.assetid: 18213fc1-74f5-4074-a32b-02846fe90601
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: willzhan;juliako
ms.openlocfilehash: 841b1164db6fd1a2c029b98392509c15f23158e2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="hybrid-design-of-drm-subsystems"></a>Hybride ontwerp van DRM subsystem(s)

Dit onderwerp wordt besproken hybride ontwerp van DRM subsystem(s) met Azure Media Services.

## <a name="overview"></a>Overzicht

Azure Media Services biedt ondersteuning voor de volgende drie DRM-systeem:

* PlayReady
* Widevine (modulaire)
* FairPlay

De DRM-ondersteuning omvat DRM-versleuteling (dynamische versleuteling) en licentie-levering met Azure Media Player alle 3 DRM's ondersteunen als een browser speler SDK.

Zie voor een uitgebreide behandeling van DRM/CENC subsysteem ontwerp en de implementatie, het document met de titel [CENC met Multi-DRM en toegangsbeheer](media-services-cenc-with-multidrm-access-control.md).

Hoewel we volledige ondersteuning voor drie DRM-systemen bieden, moeten soms klanten diverse onderdelen van hun eigen infrastructuur/subsystemen naast Azure Media Services gebruiken voor het bouwen van een hybride DRM-subsysteem.

Hieronder worden enkele veelgestelde vragen gesteld door klanten:

* "Kan ik mijn eigen DRM-licentieservers gebruiken?" (In dit geval klanten hebben geïnvesteerd in DRM licentie-serverfarm met ingesloten bedrijfslogica).
* 'Kan ik gebruiken alleen levering van uw licentie DRM in Azure Media Services zonder het hosten van inhoud in AMS?'

## <a name="modularity-of-the-ams-drm-platform"></a>Modulariteit van het AMS DRM-platform

Als onderdeel van een uitgebreide video cloudplatform heeft Azure Media Services DRM een ontwerp met flexibiliteit en modulariteit in gedachten. U kunt Azure Media Services gebruiken met een van de volgende combinaties die worden beschreven in de onderstaande tabel (een uitleg van de notatie die wordt gebruikt in de tabel volgt). 

|**Inhoud die als host fungeert & oorsprong**|**Versleuteling van inhoud**|**Levering van DRM-licentie**|
|---|---|---|
|AMS|AMS|AMS|
|AMS|AMS|Van derden|
|AMS|Van derden|AMS|
|AMS|Van derden|Van derden|
|Van derden|Van derden|AMS|

### <a name="content-hosting--origin"></a>Inhoud die als host fungeert & oorsprong

* AMS: video asset wordt gehost in AMS en streaming is via AMS streaming-eindpunten (maar niet noodzakelijkerwijs dynamische pakketten).
* Derde: video is gehost en op een derde partij streaming platform buiten AMS geleverd.

### <a name="content-encryption"></a>Versleuteling van inhoud

* AMS: versleuteling van inhoud is uitgevoerd dynamisch/op aanvraag door AMS dynamische versleuteling.
* Derde partij: versleuteling van inhoud wordt uitgevoerd buiten de AMS met een vooraf verwerken van de werkstroom.

### <a name="drm-license-delivery"></a>Levering van DRM-licentie

* AMS: DRM-licentie is geleverd door de AMS-licentieservice levering.
* Derde: DRM-licentie wordt geleverd door een derde partij DRM-licentieserver buiten AMS.

## <a name="configure-based-on-your-hybrid-scenario"></a>Configureren op basis van uw hybride scenario

### <a name="content-key"></a>Inhoudssleutel

U kunt de volgende kenmerken van AMS dynamische versleuteling en de AMS-licentieservice levering beheren via een configuratie van een inhoudssleutel:

* De inhoudssleutel voor dynamische DRM versleuteling gebruikt.
* Inhoud van DRM-licentie moet worden geleverd door de services voor het leveren van licenties: rechten, inhoudssleutel en beperkingen.
* Type **inhoud autorisatiebeleid beleidsbeperking**: openen, IP- of tokenbeperking.
* Als **token** type **autorisatiebeleid beleidsbeperking wordt gebruikt**, wordt de **inhoud autorisatiebeleid beleidsbeperking** moet worden voldaan voordat een licentie wordt verleend.

### <a name="asset-delivery-policy"></a>Leveringsbeleid voor Assets

Door de configuratie van een leveringsbeleid voor Assets, kunt u de volgende kenmerken die door dynamische packager AMS en dynamische versleuteling van een streaming-eindpunt voor AMS beheren:

* Streaming-protocol en DRM versleuteling combinatie, zoals DASH onder CENC (PlayReady en Widevine), vloeiend streaming onder PlayReady, HLS met PlayReady- of Widevine.
* De standaard/ingesloten levering van URL's voor elk van de betrokken DRM's van licentie.
* Licentie-of overname URL's (LA_URLs) in MPD DASH of HLS afspeellijst queryreeks van sleutel-ID (KID) bevatten voor Widevine en FairPlay, respectievelijk.

## <a name="scenarios-and-samples"></a>Scenario's en voorbeelden

Op basis van de uitleg in de vorige sectie, de volgende vijf hybride scenario's gebruiken respectieve **inhoudssleutel**-**leveringsbeleid voor Assets** configuratiecombinaties (de steekproeven vermeld in de laatste kolom volgen in de tabel):

|**Inhoud die als host fungeert & oorsprong**|**DRM-versleuteling**|**Levering van DRM-licentie**|**Inhoudssleutel configureren**|**Leveringsbeleid voor Assets configureren**|**Voorbeeld**|
|---|---|---|---|---|---|
|AMS|AMS|AMS|Ja|Ja|Voorbeeld 1|
|AMS|AMS|Van derden|Ja|Ja|Voorbeeld 2|
|AMS|Van derden|AMS|Ja|Nee|Voorbeeld 3|
|AMS|Van derden|Buiten|Nee|Nee|Voorbeeld 4|
|Van derden|Van derden|AMS|Ja|Nee|    

In de voorbeelden PlayReady protection werkt voor zowel DASH en smooth streaming. De onderstaande video URL's zijn smooth streaming-URL's. Als u de bijbehorende DASH-URL's, net toevoegen ' (format = mpd-time-csf) '. U kunt de [azure media player testen](http://aka.ms/amtest) om te testen in een browser. Hiermee kunt u configureren welke streaming protocol moet worden gebruikt, onder welke tech. Ondersteuning voor PlayReady via EME IE11 en MS-Edge van Windows 10. Zie voor meer informatie [meer informatie over het hulpprogramma test](https://blogs.msdn.microsoft.com/playready4/2016/02/28/azure-media-test-tool/).

### <a name="sample-1"></a>Voorbeeld 1

* Bron-URL (basis): https://willzhanmswest.streaming.mediaservices.windows.net/1efbd6bb-1e66-4e53-88c3-f7e5657a9bbd/RussianWaltz.ism/manifest 
* PlayReady LA_URL (DASH & smooth): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/ 
* Widevine-LA_URL (KOPPELTEKEN): https://willzhanmswest.keydelivery.mediaservices.windows.net/Widevine/?kid=78de73ae-6d0f-470a-8f13-5c91f7c4 
* FairPlay LA_URL (HLS): https://willzhanmswest.keydelivery.mediaservices.windows.net/FairPlay/?kid=ba7e8fb0-ee22-4291-9654-6222ac611bd8 

### <a name="sample-2"></a>Voorbeeld 2

* Bron-URL (basis): http://willzhanmswest.streaming.mediaservices.windows.net/1a670626-4515-49ee-9e7f-cd50853e41d8/Microsoft_HoloLens_TransformYourWorld_816p23.ism/Manifest 
* PlayReady LA_URL (DASH & smooth): http://willzhan12.cloudapp.net/PlayReady/RightsManager.asmx 

### <a name="sample-3"></a>Voorbeeld 3

* Bron-URL: https://willzhanmswest.streaming.mediaservices.windows.net/8d078cf8-d621-406c-84ca-88e6b9454acc/20150807-bridges-2500.ism/manifest 
* PlayReady LA_URL (DASH & smooth): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/ 

### <a name="sample-4"></a>Voorbeeld 4

* Bron-URL: https://willzhanmswest.streaming.mediaservices.windows.net/7c085a59-ae9a-411e-842c-ef10f96c3f89/20150807-bridges-2500.ism/manifest 
* PlayReady LA_URL (DASH & smooth): https://willzhan12.cloudapp.net/playready/rightsmanager.asmx 

## <a name="summary"></a>Samenvatting

Kortom, Azure Media Services DRM-componenten flexibel zijn, kunt u ze in een hybride scenario door te configureren inhoudssleutel en leveringsbeleid voor Assets, zoals beschreven in dit onderwerp.

## <a name="next-steps"></a>Volgende stappen
Weergave Media Services-leertrajecten.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

