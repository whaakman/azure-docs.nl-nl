---
title: Azure content delivery voor uw scenario optimaliseren
description: Het optimaliseren van de levering van uw inhoud voor specifieke scenario 's
services: cdn
documentationcenter: 
author: dksimpson
manager: 
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: rli
ms.openlocfilehash: fa4cf306eeb1a8372da5b2a86ac73fbba2832666
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="optimize-azure-content-delivery-for-your-scenario"></a>Azure content delivery voor uw scenario optimaliseren

Wanneer u inhoud aan een grote wereldwijde doelgroep leveren, is het essentieel om te controleren of de geoptimaliseerde levering van uw inhoud. Het Azure-netwerk voor het leveren van inhoud kunt optimaliseren van de ervaring voor de levering op basis van het type inhoud dat u hebt. Inhoud is een website, een live stream, een video of een groot bestand downloaden. Wanneer u een content delivery network (CDN)-eindpunt maakt, geeft u een scenario in de **geoptimaliseerd voor** optie. Uw keuze bepaalt welke optimalisatie wordt toegepast op de inhoud van het CDN-eindpunt geleverd.

Optimalisatie keuzes zijn ontworpen om best practice gedrag gebruiken om de prestaties leveren van inhoud te verbeteren en betere oorsprong offload. Uw keuzes scenario invloed hebben op prestaties door het wijzigen van configuraties voor gedeeltelijke caching, verdeling in segmenten object en probeer het opnieuw foutbeleid van de oorsprong. 

Dit artikel bevat een overzicht van diverse functies voor optimalisatie en wanneer u deze moet gebruiken. Zie voor meer informatie over de functies en beperkingen de respectieve artikelen op elk type afzonderlijke optimalisatie.

> [!NOTE]
> Uw **geoptimaliseerd voor** kunnen variëren op basis van de provider die u selecteert. CDN-providers toepassen uitbreiding op verschillende manieren, afhankelijk van het scenario. 

## <a name="provider-options"></a>Opties van de provider

**Azure Content Delivery Network van Akamai** ondersteunt de volgende optimalisaties:

* [Algemene webtoepassingen levering](#general-web-delivery) 

* [Algemene mediastreaming](#general-media-streaming)

* [Video-on-demand mediastreaming](#video-on-demand-media-streaming)

* [Grote bestanden downloaden](#large-file-download)

* [Dynamische siteversnelling](#dynamic-site-acceleration) 

**Azure Content Delivery Network van Verizon** ondersteunt de volgende optimalisaties:

* [Algemene webtoepassingen levering](#general-web-delivery) (kan ook worden gebruikt voor mediastreaming en inhoud van grote bestanden downloaden)

* [Dynamische siteversnelling](#dynamic-site-acceleration) 

Het is raadzaam variaties tussen verschillende providers selecteren van de optimale provider voor de levering van de prestaties te testen.

## <a name="select-and-configure-optimization-types"></a>Selecteer en optimalisatie typen configureren

Selecteer een type optimalisatie die het meest geschikt is voor het scenario en type inhoud dat u wilt dat het eindpunt te leveren voor het maken van een nieuw eindpunt. **Algemene webtoepassingen levering** is standaard geselecteerd. Voor bestaande **Azure Content Delivery Network van Akamai** eindpunten, kunt u de optimalisatieoptie op elk gewenst moment bijwerken. Deze wijziging wordt niet onderbroken levering vanaf de CDN. 

1. Binnen een **Azure Content Delivery Network van Akamai** profiel, selecteert u een eindpunt.

    ![Selectie van het eindpunt ](./media/cdn-optimization-overview/01_Akamai.png)

2. Onder **instellingen**, selecteer **optimalisatie**. Selecteer een type van de **geoptimaliseerd voor** vervolgkeuzelijst.

    ![Optimalisatie en type selectie](./media/cdn-optimization-overview/02_Select.png)

## <a name="optimization-for-specific-scenarios"></a>Optimalisatie voor specifieke scenario 's

U kunt het CDN-eindpunt voor een van deze scenario's optimaliseren. 

### <a name="general-web-delivery"></a>Algemene webweergave

Algemene webtoepassingen levering is de meest voorkomende optimalisatieoptie. Het ontworpen voor algemene web content optimalisatie, zoals webpagina's en webtoepassingen. Deze optimalisatie kan ook worden gebruikt voor het bestand en video downloadt.

Een typische website bevat statische en dynamische inhoud. Statische inhoud bevat afbeeldingen, JavaScript-bibliotheken en StyleSheets die kunnen worden opgeslagen in de cache en heeft geleverd aan andere gebruikers. Dynamische inhoud is aangepast voor een afzonderlijke gebruiker, zoals nieuwsitems die zijn aangepast aan een gebruikersprofiel. Dynamische inhoud is niet in cache opgeslagen omdat deze uniek voor elke gebruiker, zoals een winkelwagen winkelwagen inhoud. Algemene webtoepassingen levering kunt optimaliseren van uw gehele website. 

> [!NOTE]
> Als u **Azure Content Delivery Network van Akamai**, kunt u deze optimalisatie gebruiken als de gemiddelde grootte kleiner dan 10 MB is. Als de gemiddelde grootte groter dan 10 MB is, selecteert u **grote bestanden downloaden** van de **geoptimaliseerd voor** vervolgkeuzelijst.

### <a name="general-media-streaming"></a>Algemene mediastreaming

Als u moet het eindpunt te gebruiken voor live streamen en video-on-demand streaming, raden wij algemene mediastreaming optimalisatie.

Het is tijd gevoelige, streaming media, omdat pakketten die laat op de client binnenkomen leiden een ervaring verslechterde weer te geven tot kunnen, zoals frequente buffer van video-inhoud. Optimalisatie van mediastreaming vermindert de latentie van media leveren van inhoud en biedt een smooth streaming ervaring voor gebruikers. 

Dit scenario is gebruikelijk voor Azure media service-klanten. Als u Azure mediaservices gebruikt, krijgt u een streaming-eindpunt dat kan worden gebruikt voor live en on-demand streaming. Met dit scenario hoeft klanten niet overschakelen naar een ander eindpunt wanneer ze van live streaming on demand wijzigt. Algemene media streaming optimalisatie ondersteunt dit type scenario.

**Azure inhoud Delivery Network van Verizon** gebruikt van het type Algemeen web levering optimalisatie voor streaming media-inhoud.

Zie voor meer informatie over media streaming optimalisatie [mediastreaming optimalisatie](cdn-media-streaming-optimization.md).

### <a name="video-on-demand-media-streaming"></a>Video-on-demand mediastreaming

Streaming media-optimalisatie video-on-demand verbetert video-on-demand streaming inhoud. Als u een eindpunt voor streaming video-on-demand gebruikt, is het raadzaam deze optie wilt gebruiken.

**Azure inhoud Delivery Network van Verizon** gebruikt van het type Algemeen web levering optimalisatie voor streaming media-inhoud.

Zie voor meer informatie over media streaming optimalisatie [mediastreaming optimalisatie](cdn-media-streaming-optimization.md).

> [!NOTE]
> Als het eindpunt voornamelijk video-on-demand inhoud fungeert, gebruikt u dit type optimalisatie. Het belangrijkste verschil tussen deze optimalisatie en de optimalisatie van de algemene mediastreaming is de verbindingstime-out probeer het opnieuw. De time-outwaarde is veel korter werken met live streaming scenario's.

### <a name="large-file-download"></a>Grote bestanden downloaden

Als u **Azure Content Delivery Network van Akamai**, moet u grote bestanden downloaden om te leveren bestanden groter dan 1,8 GB. **Azure Content Delivery Network van Verizon** geen een beperking op downloaden bestandsgrootte in de algemene webtoepassingen leveringsoptimalisatie.

Als u **Azure inhoud Delivery Network van Akamai**, downloads van grote bestanden zijn geoptimaliseerd voor inhoud groter is dan 10 MB. Als de gemiddelde grootte kleiner dan 10 MB is, is het raadzaam algemene webtoepassingen levering gebruiken. Als de gemiddelde bestandsgrootte consistent groter dan 10 MB zijn, is dit mogelijk efficiënter om een afzonderlijke eindpunt voor grote bestanden te maken. Firmware of software-updates zijn bijvoorbeeld meestal grote bestanden.

**Azure inhoud Delivery Network van Verizon** maakt gebruik van het type Algemeen web levering optimalisatie leveren van inhoud van grote bestanden downloaden.

Zie voor meer informatie over een groot bestand optimalisatie [groot bestand optimalisatie](cdn-large-file-optimization.md).

### <a name="dynamic-site-acceleration"></a>Dynamische siteversnelling

 Dynamische site-versnelling is beschikbaar in beide **Azure Content Delivery Network van Akamai** en **Azure Content Delivery Network van Verizon** profielen. Deze optimalisatie omvat een extra kosten worden gebruikt. Zie voor meer informatie [Content Delivery Network prijzen](https://azure.microsoft.com/pricing/details/cdn/).

Dynamische site-versnelling bevat verschillende technieken die profiteren van de latentie en de prestaties van dynamische inhoud. Technieken zijn onder andere route- en optimalisatie en optimalisatie van TCP. 

Deze optimalisatie kunt u een web-app met talrijke antwoorden die geen caching geschikte versnellen. Voorbeelden zijn zoekresultaten, afhandeling transacties of realtime-gegevens. U kunt blijven gebruiken belangrijkste CDN caching-mogelijkheden voor statische gegevens. 

Zie voor meer informatie over dynamische site-versnelling [dynamische site-versnelling](cdn-dynamic-site-acceleration.md).



