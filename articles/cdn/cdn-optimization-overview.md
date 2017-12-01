---
title: Azure content delivery voor uw scenario optimaliseren
description: Het optimaliseren van de levering van uw inhoud voor specifieke scenario 's
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: v-semcev
ms.openlocfilehash: 3544112b025f5df10e6f67c8e2e02f4bb587b4e0
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="optimize-azure-content-delivery-for-your-scenario"></a>Azure content delivery voor uw scenario optimaliseren

Wanneer u inhoud aan een grote wereldwijde doelgroep leveren, is het essentieel om te controleren of de geoptimaliseerde levering van uw inhoud. Het Azure-netwerk voor het leveren van inhoud kunt optimaliseren van de ervaring voor de levering op basis van het type inhoud dat u hebt. Inhoud is een website, een live stream, een video of een groot bestand downloaden. Wanneer u een content delivery network (CDN)-eindpunt maakt, geeft u een scenario in de **geoptimaliseerd voor** optie. Uw keuze bepaalt welke optimalisatie wordt toegepast op de inhoud van het CDN-eindpunt geleverd.

Optimalisatie keuzes zijn ontworpen om best practice gedrag gebruiken om de prestaties leveren van inhoud te verbeteren en betere oorsprong offload. Uw keuzes scenario invloed hebben op prestaties door het wijzigen van configuraties voor gedeeltelijke caching, verdeling in segmenten object en probeer het opnieuw foutbeleid van de oorsprong. 

Dit artikel bevat een overzicht van diverse functies voor optimalisatie en wanneer u deze moet gebruiken. Zie voor meer informatie over de functies en beperkingen de respectieve artikelen op elk type afzonderlijke optimalisatie.

> [!NOTE]
> Uw **geoptimaliseerd voor** kunnen variëren op basis van de provider die u selecteert. CDN-providers toepassen uitbreiding op verschillende manieren, afhankelijk van het scenario. 

## <a name="provider-options"></a>Opties van de provider

Het Azure Content Delivery Network van Akamai ondersteunt:

* Algemene webweergave 

* Algemene mediastreaming

* Video-on-demand mediastreaming

* Grote bestanden downloaden

* Dynamische siteversnelling 

Het Azure Content Delivery Network van Verizon ondersteunt alleen algemene webtoepassingen levering. Het kan worden gebruikt voor de video op aanvraag en grote bestanden te downloaden. U hoeft niet te selecteren van een type optimalisatie.

Het is raadzaam variaties tussen verschillende providers selecteren van de optimale provider voor de levering van de prestaties te testen.

## <a name="select-and-configure-optimization-types"></a>Selecteer en optimalisatie typen configureren

Selecteer een type optimalisatie die het meest geschikt is voor het scenario en type inhoud dat u wilt dat het eindpunt te leveren voor het maken van een nieuw eindpunt. **Algemene webtoepassingen levering** is standaard geselecteerd. U kunt de optimalisatieoptie voor voor een bestaande Akamai-eindpunt op elk gewenst moment bijwerken. Deze wijziging wordt niet onderbroken levering vanaf de CDN. 

1. Selecteer een eindpunt binnen een standaard Akamai-profiel.

    ![Selectie van het eindpunt ](./media/cdn-optimization-overview/01_Akamai.png)

2. Onder **instellingen**, selecteer **optimalisatie**. Selecteer een van de **geoptimaliseerd voor** vervolgkeuzelijst.

    ![Optimalisatie en type selectie](./media/cdn-optimization-overview/02_Select.png)

## <a name="optimization-for-specific-scenarios"></a>Optimalisatie voor specifieke scenario 's

U kunt het CDN-eindpunt voor een van de volgende scenario's optimaliseren. 

### <a name="general-web-delivery"></a>Algemene webweergave

Algemene webtoepassingen levering is de meest voorkomende optimalisatieoptie. Het ontworpen voor algemene web content optimalisatie, zoals webpagina's en webtoepassingen. Deze optimalisatie kan ook worden gebruikt voor het bestand en video downloadt.

Een typische website bevat statische en dynamische inhoud. Statische inhoud bevat afbeeldingen, JavaScript-bibliotheken en StyleSheets die kunnen worden opgeslagen in de cache en heeft geleverd aan andere gebruikers. Dynamische inhoud is aangepast voor een afzonderlijke gebruiker, zoals nieuwsitems die zijn aangepast aan een gebruikersprofiel. Dynamische inhoud is niet in cache opgeslagen omdat deze uniek voor elke gebruiker, zoals een winkelwagen winkelwagen inhoud. Algemene webtoepassingen levering kunt optimaliseren van uw gehele website. 

> [!NOTE]
> Als u het Azure Content Delivery Network van Akamai gebruikt, is het raadzaam deze optimalisatie gebruiken als de gemiddelde grootte kleiner dan 10 MB is. Als de gemiddelde grootte groter dan 10 MB is, selecteert u **grote bestanden downloaden** van de **geoptimaliseerd voor** vervolgkeuzelijst.

### <a name="general-media-streaming"></a>Algemene mediastreaming

Als u moet het eindpunt te gebruiken voor live streamen en video-on-demand streaming, raden wij algemene mediastreaming optimalisatie.

Het is tijd gevoelige, streaming media, omdat pakketten die laat op de client binnenkomen leiden een ervaring verslechterde weer te geven tot kunnen, zoals frequente buffer van video-inhoud. Optimalisatie van mediastreaming vermindert de latentie van media leveren van inhoud en biedt een smooth streaming ervaring voor gebruikers. 

Dit scenario is gebruikelijk voor Azure media service-klanten. Als u Azure mediaservices gebruikt, krijgt u een streaming-eindpunt dat kan worden gebruikt voor live en on-demand streaming. Met dit scenario hoeft klanten niet overschakelen naar een ander eindpunt wanneer ze van live streaming on demand wijzigt. Algemene media streaming optimalisatie ondersteunt dit type scenario.

Het Azure inhoud Delivery Network van Verizon maakt gebruik van het type Algemeen web levering optimalisatie streaming media-inhoud leveren.

Zie voor meer informatie over optimalisatie streaming media, [mediastreaming optimalisatie](cdn-media-streaming-optimization.md).

### <a name="video-on-demand-media-streaming"></a>Video-on-demand mediastreaming

Streaming media-optimalisatie video-on-demand verbetert video-on-demand streaming inhoud. Als u een eindpunt voor streaming video-on-demand gebruikt, is het raadzaam deze optie wilt gebruiken.

Het Azure inhoud Delivery Network van Verizon maakt gebruik van het type Algemeen web levering optimalisatie streaming media-inhoud leveren.

Zie voor meer informatie over optimalisatie streaming media, [mediastreaming optimalisatie](cdn-media-streaming-optimization.md).

> [!NOTE]
> Als het eindpunt voornamelijk video-on-demand inhoud fungeert, gebruikt u dit type optimalisatie. Het belangrijkste verschil tussen deze optimalisatie en de optimalisatie van de algemene mediastreaming is de verbindingstime-out probeer het opnieuw. De time-outwaarde is veel korter werken met live streaming scenario's.

### <a name="large-file-download"></a>Grote bestanden downloaden

Als u het Azure Content Delivery Network van Akamai gebruikt, moet u grote bestanden downloaden om te leveren bestanden groter dan 1,8 GB. Het Azure Content Delivery Network van Verizon beschikt niet over een beperking in het bestand downloaden van de grootte in de algemene webtoepassingen leveringsoptimalisatie.

Als u het Azure inhoud Delivery Network van Akamai gebruikt, worden downloads van grote bestanden zijn geoptimaliseerd voor inhoud groter is dan 10 MB. Als de gemiddelde grootte kleiner dan 10 MB is, is het raadzaam algemene webtoepassingen levering gebruiken. Als de gemiddelde bestandsgrootte consistent groter dan 10 MB zijn, is dit mogelijk efficiënter om een afzonderlijke eindpunt voor grote bestanden te maken. Firmware of software-updates zijn bijvoorbeeld meestal grote bestanden.

Het Azure inhoud Delivery Network van Verizon maakt gebruik van het type Algemeen web levering optimalisatie leveren van inhoud van grote bestanden downloaden.

Zie voor meer informatie over de optimalisatie van grote bestanden, [groot bestand optimalisatie](cdn-large-file-optimization.md).

### <a name="dynamic-site-acceleration"></a>Dynamische siteversnelling

 Dynamische site-versnelling is beschikbaar via zowel Akamai en Verizon Content Delivery Network-profielen. Deze optimalisatie omvat een extra kosten te gebruiken. Zie voor meer informatie de pagina met prijzen.

Dynamische site-versnelling bevat verschillende technieken die profiteren van de latentie en de prestaties van dynamische inhoud. Technieken zijn onder andere route- en optimalisatie en optimalisatie van TCP. 

Deze optimalisatie kunt u een web-app met talrijke antwoorden die geen caching geschikte versnellen. Voorbeelden zijn zoekresultaten, afhandeling transacties of realtime-gegevens. U kunt blijven gebruiken belangrijkste CDN caching-mogelijkheden voor statische gegevens. 



