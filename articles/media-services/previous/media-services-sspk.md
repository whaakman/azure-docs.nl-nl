---
title: Licentieverlening Microsoft® Smooth Streaming Client Porting Kit
description: Meer informatie over hoe u aan de Microsoft® Smooth Streaming Client Porting Kit-licentieverlening.
services: media-services
documentationcenter: ''
author: xpouyat
manager: cfowler
editor: ''
ms.assetid: e3b488e7-8428-4c10-a072-eb3af46c82ad
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: xpouyat
ms.openlocfilehash: 71cb4ff7c5948067dfb039caf69241cfa9e22408
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51243893"
---
# <a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a>Licentieverlening Microsoft® Smooth Streaming Client Porting Kit
## <a name="overview"></a>Overzicht
Microsoft Smooth Streaming Client Porting Kit (**SSPK** kortweg) is een op Smooth Streaming client-implementatie die is geoptimaliseerd om te helpen fabrikanten van ingesloten apparaten, kabel en mobiele operators, inhoud serviceproviders, telefoon fabrikanten, onafhankelijke softwareleveranciers (ISV's) en leveranciers van oplossingen te maken van producten en services voor adaptieve streaminginhoud in Smooth Streaming-indeling. SSPK is een apparaat en platform-onafhankelijke implementatie van Smooth Streaming client die kan worden geïmplementeerd door de licentienemer op alle apparaten en platforms. 

Hieronder is een architectuur op hoog niveau en IIS Smooth Streaming Porting Kit in de implementatie van de Smooth Streaming Client geleverd door Microsoft en is voorzien van de corelogica voor het afspelen van Smooth Streaming-inhoud. Deze inhoud wordt vervolgens door partners voor een specifiek apparaat of platform overgezet door het juiste interfaces implementeren. 

![SSPK](./media/media-services-sspk/sspk-arch.png)

## <a name="description"></a>Beschrijving
SSPK wordt gelicentieerd op termen die uitstekende bedrijfswaarde bieden. SSPK licentie biedt de bedrijfstak met:

* Smooth Streaming Porting Kit-bron in C++ 
  * Smooth Streaming Client functionaliteit implementeert
  * voegt indeling parseren, methodiek, bufferen van logica, enzovoort.
* Spelertoepassing API 's 
  * API's voor interactie met een mediaspelertoepassing
* Platform Abstraction Layer (PAL)-Interface 
  * API's voor interactie met het besturingssysteem (threads, sockets)
* Hardware Abstraction Layer (HAL)-Interface 
  * programming interfaces voor interactie met hardware A / V decoders (decoderen, rendering)
* Digital Rights Management (DRM)-Interface 
  * API's voor het verwerken van DRM via de DRM Abstraction Layer (DAL)
  * Microsoft PlayReady Porting Kit afzonderlijk wordt geleverd, maar kan worden geïntegreerd via deze interface. Klik voor meer informatie over de licentieverlening voor Microsoft PlayReady Device [hier](https://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).
* Voorbeelden van implementatie 
  * voorbeeld PAL implementatie voor Linux
  * HAL Voorbeeldimplementatie voor GStreamer

## <a name="licensing-options"></a>Licentieopties
Microsoft Smooth Streaming Client Porting Kit is beschikbaar gesteld voor licentiehouders onder twee afzonderlijke licentieovereenkomsten: één voor het ontwikkelen van Smooth Streaming Client Interim-producten en andere voor het distribueren van Smooth Streaming Client eindproducten aan eindgebruikers.

* Voor de moederbordchipset fabrikanten, systeemintegratie of independent software vendors (ISV) die behoefte hebben aan een bron overzetten kit voor het ontwikkelen van tussentijdse producten, een Microsoft Smooth Streaming Client Porting Kit code **Interim productlicentie** moet worden uitgevoerd.
* Voor fabrikanten van apparaten of ISV's die behoefte hebben aan distributierechten voor Smooth Streaming Client eindproducten aan eindgebruikers, Microsoft Smooth Streaming Client Porting Kit **laatste productlicentie** moet worden uitgevoerd.

### <a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a>Microsoft Smooth Streaming Client Porting Kit Interim productlicentie
Onder deze licentie biedt Microsoft een Smooth Streaming Client Porting Kit en de benodigde intellectuele eigendomsrechten te ontwikkelen en Smooth Streaming Client Interim producten distribueren aan andere Smooth Streaming Client Porting Kit apparaat licentiehouders die Smooth Streaming Client eindproducten distribueren.

#### <a name="fee-structure"></a>Kostenstructuur
Een eenmalige licentietarief van US $50.000 biedt toegang tot de Smooth Streaming Client Porting Kit. 

### <a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a>Microsoft Smooth Streaming Client Porting Kit definitief productlicentie
Onder deze licentie biedt Microsoft alle benodigde intellectuele-eigendomsrechten Smooth Streaming Client Interim producten van andere licentiehouders Smooth Streaming Client Porting Kit ontvangen en te distribueren huisstijl van bedrijf Smooth Streaming Client laatste Producten aan eindgebruikers.

#### <a name="fee-structure"></a>Kostenstructuur
De Smooth Streaming Client uiteindelijke Product wordt aangeboden in een model royalty als onder:

* $0,10 per apparaatimplementatie verzonden
* De royalty wordt beperkt tot 50.000 per jaar
* Er is geen royalty voor eerste 10.000 apparaat implementaties per jaar 

## <a name="licensing-procedure-and-sspk-access"></a>Licentieverlening Procedure en SSPK toegang
E-mailbericht [ sspkinfo@microsoft.com ](mailto:sspkinfo@microsoft.com) licentieverlening voor alle query's.

De [SSPK distributie portal](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) is toegankelijk voor geregistreerde Interim-licenties.

Ondertussen en uiteindelijke SSPK licentiehouders technische vragen om te kunnen indienen [ smoothpk@microsoft.com ](mailto:smoothpk@microsoft.com).

## <a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a>Microsoft Smooth Streaming Client tussentijdse Product overeenkomst licentiehouders
* Adroit Business Solutions, Inc.
* Geavanceerde digitale uitzending SA
* AirTies Kablosuz Iletism Sanayive Dis Ticaret W.S.
* Albis Technologies Ltd.
* Alticast Corporation
* Amazon digitale Services, Inc.
* Arion technologie, Inc.
* AVC Multimedia Software Co., Ltd.
* Cavium, Inc.
* EchoStar aanschaffen Corporation
* Enseo, Inc.
* Fluendo S.A.
* HANDAN BroadInfoCom Co., Ltd.
* Infomir GMBH
* Irdeto VS Inc.
* iWEDIA S.A. 
* Liberty globale Services BV
* MediaTek Inc.
* MStar Co, Ltd
* NINTENDO Co., Ltd.
* OpenTV, Inc.
* Saffraan digitale beperkt
* Sichuan Changhong Electric Co., Ltd.
* SoftAtHome
* Sony Corporation
* Tatung Technology Inc.
* TCL technologie (Huizhou) Electronics Co., Ltd.
* Top overwinning investeringen, Ltd.
* Vestel Elektronik Sanayi ve Ticaret W.S.
* VisualOn, Inc.
* ZTE Corporation

## <a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a>Microsoft Smooth Streaming Client eindproduct overeenkomst licentiehouders
* Geavanceerde digitale uitzending SA
* AirTies Kablosuz Iletism Sanayive Dis Ticaret W.S.
* Albis Technologies Ltd.
* Amazon digitale Services, Inc.
* AmTRAN technologie Co., Ltd.
* Arcadyan Technology Corporation
* Arion technologie, Inc.
* ATMACA ELEKTRONİK SAN. VE TİC. A.Ş
* Brits Sky Broadcasting beperkt
* CastPal technologie Inc., Shenzhen
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
* Homecast Co., Ltd.
* Hon Hai precisie branche Co., Ltd.
* Infomir GMBH
* Kaonmedia Co., Ltd.
* KDDI Corporation
* NINTENDO Co., Ltd.
* Oranje SA
* Saffraan digitale beperkt
* Sagemcom breedband SAS
* Shenzhen Coship Electronics CO., Ltd.
* Shenzhen Jiuzhou Electric Co., Ltd.
* Shenzhen Skyworth digitale technologie Co., Ltd.
* Sichuan Changhong Electric Co., Ltd.
* Skardin industriële Corp.
* Sky Deutschland Fernsehen GmbH & Co. KG
* SmarDTV S.A.
* SoftAtHome
* Sony Corporation
* Beperkte TCL buitenland Marketing (Offshore Macau commerciële)
* SAS-technologieën levering Technicolor
* Tongfang globale Ltd.
* Top overwinning investeringen, Ltd.
* Toshiba levensstijl-producten en Services Corporation
* Universele Media Corporation /Slovakia/ s.r.o.
* VIZIO, Inc.
* Wistron Corporation
* ZTE Corporation


## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

