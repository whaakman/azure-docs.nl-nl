---
title: Hybride ontwerp van DRM-subsystem(s) met Azure Media Services | Microsoft Docs
description: In dit onderwerp worden hybride ontwerp van DRM-subsystem(s) met Azure Media Services.
services: media-services
documentationcenter: ''
author: willzhan
manager: femila
editor: ''
ms.assetid: 18213fc1-74f5-4074-a32b-02846fe90601
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: willzhan;juliako
ms.openlocfilehash: 5c86a49cd9dc26f724de12ed2e5e77e645e4ab53
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57886708"
---
# <a name="hybrid-design-of-drm-subsystems"></a>Hybride ontwerp van DRM-subsystemen 

In dit onderwerp worden hybride ontwerp van DRM-subsystem(s) met Azure Media Services.

## <a name="overview"></a>Overzicht

Azure Media Services biedt ondersteuning voor de volgende drie DRM-systeem:

* PlayReady
* Widevine (modulaire)
* FairPlay

De DRM-ondersteuning omvat DRM-versleuteling (dynamische versleuteling) en de levering van de licentie, met Azure Media Player ondersteunt alle 3 DRM's als een browser player SDK.

Raadpleeg het document met de titel voor een uitgebreide behandeling van DRM/CENC subsysteem ontwerpen en implementeren, [CENC met Multi-DRM en Access Control](media-services-cenc-with-multidrm-access-control.md).

Hoewel we volledige ondersteuning voor drie DRM-systemen bieden, moeten soms klanten verschillende onderdelen van hun eigen infrastructuur/subsystemen naast Azure Media Services gebruiken voor het bouwen van een hybride DRM-subsysteem.

Hieronder worden enkele veelgestelde vragen gesteld door klanten:

* 'Kan ik mijn eigen DRM-licentieservers gebruiken?' (In dit geval klanten hebben geïnvesteerd in DRM-licentie-serverfarm met ingesloten bedrijfslogica).
* "Kan ik gebruiken alleen de levering van DRM-licentie in Azure Media Services zonder het hosten van inhoud in AMS?"

## <a name="modularity-of-the-ams-drm-platform"></a>Modulariteit mogelijk te maken van het AMS DRM-platform

Als onderdeel van een video met uitgebreide cloudplatform heeft Azure Media Services DRM een ontwerp met flexibiliteit en modulariteit mogelijk te maken in gedachten. U kunt Azure Media Services gebruiken met een van de volgende combinaties die worden beschreven in de onderstaande tabel (een uitleg van de notatie die wordt gebruikt in de tabel volgt). 

|**Hosting van inhoud & oorsprong**|**Versleuteling van inhoud**|**Levering van DRM-licentie**|
|---|---|---|
|AMS|AMS|AMS|
|AMS|AMS|Van derden|
|AMS|Van derden|AMS|
|AMS|Van derden|Van derden|
|Van derden|Van derden|AMS|

### <a name="content-hosting--origin"></a>Hosting van inhoud & oorsprong

* AMS: video-asset wordt gehost in AMS en streaming is via het streaming-eindpunten van het AMS (maar niet per se dynamische pakketten).
* Derde partij: video is die wordt gehost en op een derde partij streamingplatform buiten AMS geleverd.

### <a name="content-encryption"></a>Versleuteling van inhoud

* AMS: versleuteling van inhoud is uitgevoerd dynamisch/op aanvraag met AMS dynamische versleuteling.
* Derde partij: versleuteling van inhoud wordt uitgevoerd buiten AMS met een vooraf verwerken. de werkstroom.

### <a name="drm-license-delivery"></a>Levering van DRM-licentie

* AMS: DRM-licentie wordt geboden door de AMS-licentieservice levering.
* Van derden: DRM-licentie wordt geboden door een derde partij DRM-licentieserver buiten AMS.

## <a name="configure-based-on-your-hybrid-scenario"></a>Configureren op basis van uw hybride scenario

### <a name="content-key"></a>Inhoudssleutel

Bij het configureren van een inhoudssleutel, kunt u de volgende kenmerken van AMS dynamische versleuteling en de AMS-licentieservice levering beheren:

* De inhoudssleutel wordt gebruikt voor dynamische DRM-versleuteling.
* DRM-licentie inhoud die moet worden geleverd door services voor het leveren van licenties: rechten, inhoudssleutel en beperkingen.
* Type **inhoud autorisatiebeleid beleidsbeperking**: lopend, IP- of tokenbeperking.
* Als **token** type **autorisatiebeleid beleidsbeperking wordt gebruikt**, wordt de **inhoud autorisatiebeleid beleidsbeperking** moet worden voldaan voordat een licentie wordt uitgegeven.

### <a name="asset-delivery-policy"></a>Leveringsbeleid voor Assets

Bij het configureren van een leveringsbeleid voor Assets, kunt u de volgende kenmerken die worden gebruikt door dynamische packager AMS en dynamische versleuteling van een streaming-eindpunt voor AMS beheren:

* Streaming-protocol en de combinatie van DRM-versleuteling, zoals DASH onder CENC (PlayReady en Widevine), smooth streaming onder PlayReady, HLS onder Widevine of PlayReady.
* De standaard-/ ingesloten levering van URL's voor elk van de betrokken DRM's van licentie.
* Licentie-of overname URL's (LA_URLs) in MPD DASH of HLS afspeellijst queryreeks van sleutel-ID (kinderen) bevatten voor Widevine en FairPlay, respectievelijk.

## <a name="scenarios-and-samples"></a>Scenario's en voorbeelden

Op basis van de uitleg in de vorige sectie, de volgende vijf hybride scenario's gebruiken respectieve **inhoudssleutel**-**leveringsbeleid voor Assets** configuratiecombinaties (de voorbeelden vermeld in de laatste kolom volgen in de tabel):

|**Hosting van inhoud & oorsprong**|**DRM-versleuteling**|**Levering van DRM-licentie**|**Inhoudssleutel configureren**|**Leveringsbeleid voor Assets configureren**|**Voorbeeld**|
|---|---|---|---|---|---|
|AMS|AMS|AMS|Ja|Ja|Voorbeeld 1|
|AMS|AMS|Van derden|Ja|Ja|Voorbeeld 2|
|AMS|Van derden|AMS|Ja|Nee|Voorbeeld 3|
|AMS|Van derden|Buiten|Nee|Nee|Voorbeeld 4|
|Van derden|Van derden|AMS|Ja|Nee|    

In de voorbeelden werkt PlayReady-bescherming voor DASH en smooth streaming. De onderstaande video URL's zijn smooth streaming-URL's. Als u de bijbehorende DASH-URL's, alleen toevoegen ' (format = mpd-time-csf) '. U kunt de [azure media player testen](https://aka.ms/amtest) om te testen in een browser. Hiermee kunt u configureren welke streaming-protocol wilt gebruiken, onder welke tech. Ondersteuning voor PlayReady via EME IE11 en Microsoft Edge op Windows 10. Zie voor meer informatie, [meer informatie over het hulpprogramma test](https://blogs.msdn.microsoft.com/playready4/2016/02/28/azure-media-test-tool/).

### <a name="sample-1"></a>Voorbeeld 1

* URL van de bron (basis): https://willzhanmswest.streaming.mediaservices.windows.net/1efbd6bb-1e66-4e53-88c3-f7e5657a9bbd/RussianWaltz.ism/manifest 
* PlayReady LA_URL (DASH & smooth): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/ 
* Widevine LA_URL (DASH): https://willzhanmswest.keydelivery.mediaservices.windows.net/Widevine/?kid=78de73ae-6d0f-470a-8f13-5c91f7c4 
* FairPlay LA_URL (HLS): https://willzhanmswest.keydelivery.mediaservices.windows.net/FairPlay/?kid=ba7e8fb0-ee22-4291-9654-6222ac611bd8 

### <a name="sample-2"></a>Voorbeeld 2

* URL van de bron (basis): https://willzhanmswest.streaming.mediaservices.windows.net/1a670626-4515-49ee-9e7f-cd50853e41d8/Microsoft_HoloLens_TransformYourWorld_816p23.ism/Manifest 
* PlayReady LA_URL (DASH & smooth): http://willzhan12.cloudapp.net/PlayReady/RightsManager.asmx 

### <a name="sample-3"></a>Voorbeeld 3

* Bron-URL: https://willzhanmswest.streaming.mediaservices.windows.net/8d078cf8-d621-406c-84ca-88e6b9454acc/20150807-bridges-2500.ism/manifest 
* PlayReady LA_URL (DASH & smooth): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/ 

### <a name="sample-4"></a>Voorbeeld 4

* Bron-URL: https://willzhanmswest.streaming.mediaservices.windows.net/7c085a59-ae9a-411e-842c-ef10f96c3f89/20150807-bridges-2500.ism/manifest 
* PlayReady LA_URL (DASH & smooth): https://willzhan12.cloudapp.net/playready/rightsmanager.asmx 

## <a name="summary"></a>Samenvatting

Kortom, Azure Media Services DRM-componenten zijn flexibel, kunt u ze in een hybride scenario door een inhoudssleutel en leveringsbeleid voor Assets, correct te configureren zoals beschreven in dit onderwerp.

## <a name="next-steps"></a>Volgende stappen
Weergave Media Services-leertrajecten.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

