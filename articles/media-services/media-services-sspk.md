---
title: "Licentieverlening Microsoft® Smooth Streaming Client Kit overdragen"
description: "Meer informatie over het in de Microsoft® Smooth Streaming Client overdragen Kit licentieverlening voor."
services: media-services
documentationcenter: 
author: xpouyat
manager: cfowler
editor: 
ms.assetid: e3b488e7-8428-4c10-a072-eb3af46c82ad
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: xpouyat
ms.openlocfilehash: 87a5a1981b05722f25a70fcb73a06db65bcbe0fd
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a>Licentieverlening Microsoft® Smooth Streaming Client Kit overdragen
## <a name="overview"></a>Overzicht
Microsoft Smooth Streaming Client overdragen Kit (**SSPK** voor korte) is een Smooth Streaming clientimplementatie die is geoptimaliseerd om u te helpen bij embedded-apparaatfabrikanten, kabel en mobiele operators, inhoud serviceproviders, telefoon fabrikanten, onafhankelijke softwareleveranciers (ISV's) en oplossingsproviders voor het maken van producten en services voor streaming adaptieve inhoud in Smooth Streaming-indeling. SSPK is een apparaat en platformonafhankelijk uitvoering van Smooth Streaming-client die kan worden geïmplementeerd door de licentiehouder aan een apparaat en het platform. 

Hieronder vindt u is een architectuur op hoog niveau en IIS Smooth Streaming overdragen Kit is de implementatie van de Smooth Streaming Client geleverd door Microsoft en bevat alle corelogica voor het afspelen van inhoud Smooth Streaming. Deze inhoud wordt vervolgens door partners voor een specifiek apparaat of platform overgezet door het juiste interfaces implementeren. 

![SSPK](./media/media-services-sspk/sspk-arch.png)

## <a name="description"></a>Beschrijving
SSPK is in licentie gegeven op voorwaarden die worden geboden door uitstekende bedrijfswaarde. SSPK licentie verleend voor de branche met:

* Smooth Streaming overdragen Kit-bron in C++ 
  * functionaliteit Smooth Streaming Client implementeert
  * voegt indeling parseren, methodiek, buffer logica, enzovoort.
* Spelertoepassing API 's 
  * API's voor interactie met een media player-toepassing
* Platform Abstraction Layer (PAL)-Interface 
  * API's voor interactie met het besturingssysteem (threads, sockets)
* Hardware Abstraction Layer (HAL)-Interface 
  * programming interfaces voor interactie met hardware A / V decoders (decoderen, rendering)
* Beheerinterface van digitale rechten (DRM) 
  * API's voor het verwerken van DRM via DRM Abstraction Layer (DAL)
  * Microsoft PlayReady overdragen Kit wordt afzonderlijk worden geleverd, maar kan worden geïntegreerd via deze interface. Klik voor meer informatie over licenties voor Microsoft PlayReady Device [hier](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).
* Implementatie-voorbeelden 
  * Voorbeeldimplementatie PAL voor Linux
  * Voorbeeldimplementatie HAL voor GStreamer

## <a name="licensing-options"></a>Licentieopties
Microsoft Smooth Streaming Client overdragen Kit wordt beschikbaar gemaakt aan licentiehouders onder twee afzonderlijke licentieovereenkomsten: één voor het ontwikkelen van Smooth Streaming Client Interim-producten en andere voor het distribueren van Smooth Streaming Client eindproducten aan eindgebruikers.

* Voor chipsetfabrikanten, systeemintegrators of independent software vendors (ISV's) die behoefte hebben aan een bron overzetten kit om te zetten Interim-producten, een Microsoft Smooth Streaming Client overdragen Kit code **Interim productlicentie** moet worden uitgevoerd.
* Voor fabrikanten van apparaten of ISV's die distributierechten voor Smooth Streaming Client eindproducten nodig aan eindgebruikers, Microsoft Smooth Streaming Client overdragen Kit hebben **laatste productlicentie** moet worden uitgevoerd.

### <a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a>Microsoft Smooth Streaming Client Kit Interim productlicentie overdragen
Onder deze licentie biedt Microsoft een Smooth Streaming Client overdragen Kit en de benodigde intellectuele eigendomsrechten te ontwikkelen en distribueren Smooth Streaming Client Interim-producten aan andere Smooth Streaming Client overdragen Kit apparaat licentiehouders die Smooth Streaming Client eindproducten distribueert.

#### <a name="fee-structure"></a>Kostenstructuur
Een VS 50.000 eenmalige licentiekosten biedt toegang tot de Smooth Streaming Client overdragen Kit. 

### <a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a>Microsoft Smooth Streaming Client Kit Final productlicentie overdragen
Microsoft biedt onder deze licentie alle benodigde intellectuele eigendomsrechten Smooth Streaming Client Interim producten van andere licentiehouders Smooth Streaming Client overdragen Kit ontvangt en voor het distribueren van de huisstijl van het bedrijf Smooth Streaming Client eindproducten aan eindgebruikers.

#### <a name="fee-structure"></a>Kostenstructuur
Het Smooth Streaming Client uiteindelijke Product wordt aangeboden onder een model royalty als onder:

* $0.10 per apparaat uitvoering is verzonden
* De royalty is beperkt tot 50.000 jaarlijks
* Er is geen royalty voor eerste 10.000 apparaat implementaties jaarlijks 

## <a name="licensing-procedure-and-sspk-access"></a>Licentieverlening Procedure en SSPK toegang
E-mailadres [ sspkinfo@microsoft.com ](mailto:sspkinfo@microsoft.com) licentieverlening voor alle query's.

De [SSPK distributie portal](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) toegankelijk is voor de geregistreerde Interim-licenties.

Tussentijdse en definitieve SSPK licentiehouders technische vragen om te kunnen indienen [ smoothpk@microsoft.com ](mailto:smoothpk@microsoft.com).

## <a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a>Microsoft Smooth Streaming Client tussentijdse Product overeenkomst licentiehouders
* Adroit Business oplossingen, Inc
* Geavanceerde digitale uitzending SA
* AirTies Kablosuz Iletism Sanayive Dis Ticaret W.S.
* Albis Technologies Ltd.
* Alticast Corporation
* Amazon digitale Services, Inc.
* Arion-technologie, Inc.
* AVC Multimedia Software Co., Ltd.
* Cavium, Inc.
* EchoStar aanschaffen Corporation
* Enseo, Inc.
* Fluendo S.A.
* HANDAN BroadInfoCom Co., Ltd.
* Infomir GMBH
* Irdeto Verenigde Staten Inc.
* iWEDIA S.A. 
* Liberty globale Services BW
* MediaTek Inc.
* MStar Co Ltd
* NINTENDO Co., Ltd.
* OpenTV, Inc.
* Saffraan digitale beperkt
* Sichuan Changhong elektrische Co. Ltd
* SoftAtHome
* Sony Corporation
* Tatung Technology Inc.
* TCL technologie Electronics (Huizhou) Co., Ltd.
* Top overwinning investeringen, Ltd.
* Vestel Elektronik Sanayi ve Ticaret W.S.
* VisualOn, Inc.
* ZTE Corporation

## <a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a>Microsoft Smooth Streaming Client uiteindelijke Product overeenkomst licentiehouders
* Geavanceerde digitale uitzending SA
* AirTies Kablosuz Iletism Sanayive Dis Ticaret W.S.
* Albis Technologies Ltd.
* Amazon digitale Services, Inc.
* AmTRAN technologie Co., Ltd.
* Arcadyan Technology Corporation
* Arion-technologie, Inc.
* ATMACA ELEKTRONİK SAN. VE TİC. A.Ş
* Brits Sky Broadcasting beperkt
* CastPal Technology Inc. Shenzhen
* Compal Electronics, Inc.
* Dongguan digitale AV-technologie Corp., Ltd.
* EchoStar aanschaffen Corporation
* Enseo, Inc.
* Filmflex films beperkt
* Fluendo S.A.
* Gibson innovaties beperkt
* Haier informatie Applicantion S.R.L
* HANDAN BroadInfoCom Co., Ltd.
* Hisense International Co., Ltd. 
* Homecast Co. Ltd
* Hon Hai precisie bedrijfstak Co., Ltd.
* Infomir GMBH
* Kaonmedia Co., Ltd.
* KDDI Corporation
* NINTENDO Co., Ltd.
* Oranje SA
* Saffraan digitale beperkt
* Sagemcom breedband SAS
* Shenzhen Coship Electronics CO. LTD
* Shenzhen Jiuzhou elektrische Co. Ltd
* Shenzhen Skyworth digitale technologie Co. Ltd
* Sichuan Changhong elektrische Co., Ltd.
* Skardin industriële Corp.
* Sky Deutschland Fernsehen GmbH & Co. KG
* SmarDTV S.A.
* SoftAtHome
* Sony Corporation
* Beperkte TCL overzeese Marketing (Offshore Macau commerciële)
* Technicolor levering technologieën, SAS
* Tongfang globale Ltd.
* Top overwinning investeringen, Ltd.
* Toshiba Lifestyle-producten en Services Corporation
* Universele Media Corporation /Slovakia/ s.r.o.
* VIZIO, Inc.
* Wistron Corporation
* ZTE Corporation


## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

