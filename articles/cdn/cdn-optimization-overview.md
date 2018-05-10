---
title: Azure content delivery voor uw scenario optimaliseren
description: Het optimaliseren van de levering van uw inhoud voor specifieke scenario 's
services: cdn
documentationcenter: ''
author: dksimpson
manager: ''
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: rli
ms.openlocfilehash: de748f7fa33b0250b1572dd5ae55cddf15003a0e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="optimize-azure-content-delivery-for-your-scenario"></a>Azure content delivery voor uw scenario optimaliseren

Wanneer u inhoud aan een grote wereldwijde doelgroep leveren, is het essentieel om te controleren of de geoptimaliseerde levering van uw inhoud. Azure inhoud Delivery Network (CDN) kunt optimaliseren van de ervaring voor de levering op basis van het type inhoud dat u hebt. Inhoud is een website, een live stream, een video of een groot bestand downloaden. Wanneer u een CDN-eindpunt maakt, geeft u een scenario in de **geoptimaliseerd voor** optie. Uw keuze bepaalt welke optimalisatie wordt toegepast op de inhoud van het CDN-eindpunt geleverd.

Optimalisatie keuzes zijn ontworpen om best practice gedrag gebruiken om de prestaties leveren van inhoud te verbeteren en betere oorsprong offload. Uw keuzes scenario invloed hebben op prestaties door het wijzigen van configuraties voor gedeeltelijke caching, verdeling in segmenten object en probeer het opnieuw foutbeleid van de oorsprong. 

Dit artikel bevat een overzicht van diverse functies voor optimalisatie en wanneer u deze moet gebruiken. Zie voor meer informatie over de functies en beperkingen de respectieve artikelen op elk type afzonderlijke optimalisatie.

> [!NOTE]
> Uw **geoptimaliseerd voor** kunnen variëren op basis van de provider die u selecteert. CDN-providers toepassen uitbreiding op verschillende manieren, afhankelijk van het scenario. 

## <a name="provider-options"></a>Opties van de provider

**Azure CDN Standard van Microsoft** ondersteunt de volgende optimalisaties:

* [Algemene webtoepassingen levering](#general-web-delivery). Deze optimalisatie wordt ook gebruikt voor mediastreaming en grote bestanden downloaden.


**Azure CDN Standard van Verizon**, en **Azure CDN Premium van Verizon** ondersteunen de volgende optimalisaties:

* [Algemene webtoepassingen levering](#general-web-delivery). Deze optimalisatie wordt ook gebruikt voor mediastreaming en grote bestanden downloaden.

* [Dynamische siteversnelling](#dynamic-site-acceleration) 


**Azure CDN Standard van Akamai** ondersteunt de volgende optimalisaties:

* [Algemene webtoepassingen levering](#general-web-delivery) 

* [Algemene mediastreaming](#general-media-streaming)

* [Video-on-demand mediastreaming](#video-on-demand-media-streaming)

* [Grote bestanden downloaden](#large-file-download)

* [Dynamische siteversnelling](#dynamic-site-acceleration) 

Microsoft raadt aan variaties tussen verschillende providers selecteren van de optimale provider voor de levering van de prestaties te testen.

## <a name="select-and-configure-optimization-types"></a>Selecteer en optimalisatie typen configureren

Selecteer een type optimalisatie die het meest geschikt is voor het scenario en type inhoud dat u wilt dat het eindpunt te leveren voor het maken van een nieuw eindpunt. **Algemene webtoepassingen levering** is standaard geselecteerd. Voor bestaande **Azure Content Delivery Network van Akamai** eindpunten, kunt u de optimalisatieoptie op elk gewenst moment bijwerken. Deze wijziging wordt niet onderbroken levering van Azure CDN. 

1. Binnen een **Azure CDN Standard van Akamai** profiel, selecteert u een eindpunt.

    ![Selectie van het eindpunt ](./media/cdn-optimization-overview/01_Akamai.png)

2. Selecteer onder instellingen **optimalisatie**. Selecteer een type van de **geoptimaliseerd voor** vervolgkeuzelijst.

    ![Optimalisatie en type selectie](./media/cdn-optimization-overview/02_Select.png)

## <a name="optimization-for-specific-scenarios"></a>Optimalisatie voor specifieke scenario 's

U kunt het CDN-eindpunt voor een van deze scenario's optimaliseren. 

### <a name="general-web-delivery"></a>Algemene webweergave

Algemene webtoepassingen levering is de meest voorkomende optimalisatieoptie. Het ontworpen voor algemene web content optimalisatie, zoals webpagina's en webtoepassingen. Deze optimalisatie kan ook worden gebruikt voor het bestand en video downloadt.

Een typische website bevat statische en dynamische inhoud. Statische inhoud bevat afbeeldingen, JavaScript-bibliotheken en StyleSheets die kunnen worden opgeslagen in de cache en heeft geleverd aan andere gebruikers. Dynamische inhoud is aangepast voor een afzonderlijke gebruiker, zoals nieuwsitems die zijn aangepast aan een gebruikersprofiel. Dynamische inhoud is niet in cache opgeslagen omdat deze uniek voor elke gebruiker, zoals een winkelwagen winkelwagen inhoud. Algemene webtoepassingen levering kunt optimaliseren van uw gehele website. 

> [!NOTE]
> Als u **Azure CDN Standard van Akamai**, kunt u deze optimalisatie gebruiken als de gemiddelde grootte kleiner dan 10 MB is. Als de gemiddelde grootte groter dan 10 MB is, selecteert u **grote bestanden downloaden** van de **geoptimaliseerd voor** vervolgkeuzelijst.

### <a name="general-media-streaming"></a>Algemene mediastreaming

Als u moet het eindpunt te gebruiken voor live streamen en video-on-demand streaming, raden wij algemene mediastreaming optimalisatie.

Het is tijd gevoelige, streaming media, omdat pakketten die laat op de client binnenkomen leiden een ervaring verslechterde weer te geven tot kunnen, zoals frequente buffer van video-inhoud. Optimalisatie van mediastreaming vermindert de latentie van media leveren van inhoud en biedt een smooth streaming ervaring voor gebruikers. 

Dit scenario is gebruikelijk voor Azure media service-klanten. Als u Azure mediaservices gebruikt, krijgt u een streaming-eindpunt dat kan worden gebruikt voor live en on-demand streaming. Met dit scenario hoeft klanten niet overschakelen naar een ander eindpunt wanneer ze van live streaming on demand wijzigt. Algemene media streaming optimalisatie ondersteunt dit type scenario.

Voor **Azure CDN Standard van Microsoft**, **Azure CDN Standard van Verizon**, en **Azure CDN Premium van Verizon**, het type Algemeen web levering optimalisatie te gebruiken algemene streaming mediainhoud leveren.

Zie voor meer informatie over media streaming optimalisatie [mediastreaming optimalisatie](cdn-media-streaming-optimization.md).

### <a name="video-on-demand-media-streaming"></a>Video-on-demand mediastreaming

Streaming media-optimalisatie video-on-demand verbetert video-on-demand streaming inhoud. Als u een eindpunt voor streaming video-on-demand gebruikt, is het raadzaam deze optie wilt gebruiken.

Voor **Azure CDN Standard van Microsoft**, **Azure CDN Standard van Verizon**, en **Azure CDN Premium van Verizon**, het type Algemeen web levering optimalisatie te gebruiken video-on-demand streaming media-inhoud leveren.

Zie voor meer informatie over media streaming optimalisatie [mediastreaming optimalisatie](cdn-media-streaming-optimization.md).

> [!NOTE]
> Als het CDN-eindpunt voornamelijk video-on-demand inhoud fungeert, gebruikt u dit type optimalisatie. Het belangrijkste verschil tussen deze optimalisatie en de optimalisatie van de algemene mediastreaming is de verbindingstime-out probeer het opnieuw. De time-outwaarde is veel korter werken met live streaming scenario's.

### <a name="large-file-download"></a>Grote bestanden downloaden

Voor **Azure CDN Standard van Akamai**, downloads van grote bestanden zijn geoptimaliseerd voor inhoud groter is dan 10 MB. Als uw gemiddelde bestandsgrootte kleiner dan 10 MB is, gebruikt u algemene webtoepassingen levering. Als de gemiddelde bestandsgrootte consistent groter dan 10 MB zijn, is dit mogelijk efficiënter om een afzonderlijke eindpunt voor grote bestanden te maken. Firmware of software-updates zijn bijvoorbeeld meestal grote bestanden. Voor het leveren van bestanden groter dan 1,8 GB, hoeft de optimalisatie van grote bestanden downloaden.

Voor **Azure CDN Standard van Microsoft**, **Azure CDN Standard van Verizon**, en **Azure CDN Premium van Verizon**, het type Algemeen web levering optimalisatie te gebruiken bieden een groot bestand downloaden van inhoud. Er geldt geen beperking op bestandsgrootte downloaden.

Zie voor meer informatie over een groot bestand optimalisatie [groot bestand optimalisatie](cdn-large-file-optimization.md).

### <a name="dynamic-site-acceleration"></a>Dynamische siteversnelling

 Dynamische site-versnelling is beschikbaar voor **Azure CDN Standard van Akamai**, **Azure CDN Standard van Verizon**, en **Azure CDN Premium van Verizon** profielen. Deze optimalisatie omvat een extra kosten worden gebruikt. Zie voor meer informatie [Content Delivery Network prijzen](https://azure.microsoft.com/pricing/details/cdn/).

Dynamische site-versnelling bevat verschillende technieken die profiteren van de latentie en de prestaties van dynamische inhoud. Technieken zijn onder andere route- en optimalisatie en optimalisatie van TCP. 

Deze optimalisatie kunt u een web-app met talrijke antwoorden die geen caching geschikte versnellen. Voorbeelden zijn zoekresultaten, afhandeling transacties of realtime-gegevens. U kunt blijven gebruiken belangrijkste CDN caching-mogelijkheden voor statische gegevens. 

Zie voor meer informatie over dynamische site-versnelling [dynamische site-versnelling](cdn-dynamic-site-acceleration.md).



