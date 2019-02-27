---
title: Azure CDN voor het type van de levering van inhoud optimaliseren
description: Azure CDN voor het type van de levering van inhoud optimaliseren
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2018
ms.author: magattus
ms.openlocfilehash: 5be1835de2aa1631c4ec2115dd34ea85f0267403
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56870887"
---
# <a name="optimize-azure-cdn-for-the-type-of-content-delivery"></a>Azure CDN voor het type van de levering van inhoud optimaliseren

Wanneer u inhoud aan een grote algemene doelgroep leveren, is het is essentieel om te controleren of de geoptimaliseerde levering van uw inhoud. [Azure Content Delivery Network (CDN)](cdn-overview.md) de delivery-ervaring op basis van het type inhoud dat u hebt kunt optimaliseren. De inhoud is een website, een live stream, een video of een grote bestanden downloaden. Wanneer u een CDN-eindpunt maakt, geeft u een scenario in de **geoptimaliseerd voor** optie. Uw keuze bepaalt welke optimalisatie wordt toegepast op de inhoud van het CDN-eindpunt geleverd.

Keuzen voor optimalisatie zijn ontworpen om aanbevolen procedures gedrag gebruiken om prestaties van de levering van inhoud te verbeteren en betere origin-offload. Uw keuzes scenario invloed hebben op prestaties door het wijzigen van configuraties voor gedeeltelijke caching, object logische groepen te verdelen en het beleid voor opnieuw proberen van oorsprong fout. 

Dit artikel bevat een overzicht van verschillende functies voor optimalisatie en wanneer u deze moet gebruiken. Zie voor meer informatie over functies en beperkingen, de respectieve artikelen op elke afzonderlijke optimalisatietype.

> [!NOTE]
> Wanneer u een CDN-eindpunt, maakt de **geoptimaliseerd voor** opties kunnen variëren op basis van het type van het profiel dat het eindpunt is gemaakt in. Azure CDN-providers toepassen uitbreiding op verschillende manieren, afhankelijk van het scenario. 

## <a name="provider-options"></a>Provideropties

**Azure CDN Standard van Microsoft** profielen ondersteunt de volgende optimaliseringen:

* [Algemene Webweergave](#general-web-delivery). Deze optimalisatie wordt ook gebruikt voor streaming van media en downloaden van grote bestanden.


**Azure CDN Standard van Verizon** en **Azure CDN Premium van Verizon** profielen ondersteunen de volgende optimaliseringen:

* [Algemene Webweergave](#general-web-delivery). Deze optimalisatie wordt ook gebruikt voor streaming van media en downloaden van grote bestanden.

* [Dynamische siteversnelling](#dynamic-site-acceleration) 


**Azure CDN Standard van Akamai** profielen ondersteunen de volgende optimaliseringen:

* [Algemene webweergave](#general-web-delivery) 

* [Algemene mediastreaming](#general-media-streaming)

* [Video on demand streaming van media](#video-on-demand-media-streaming)

* [Grote bestanden downloaden](#large-file-download)

* [Dynamische siteversnelling](#dynamic-site-acceleration) 

Microsoft raadt aan dat u variaties van de prestaties tussen verschillende providers te selecteren van de optimale provider voor de levering van uw test.

## <a name="select-and-configure-optimization-types"></a>Selecteert en configureert u optimalisatietypes

Wanneer u een CDN-eindpunt maakt, selecteert u een optimalisatietype die het beste het scenario en type inhoud dat u wilt dat het eindpunt te leveren. **Algemene Webweergave** is standaard geselecteerd. Voor bestaande **Azure CDN Standard van Akamai** eindpunten, kunt u de optimalisatieoptie op elk gewenst moment bijwerken. Deze wijziging niet-levering vanuit Azure CDN worden onderbroken. 

1. In een **Azure CDN Standard van Akamai** profiel, selecteer een eindpunt.

    ![Eindpunt selecteren](./media/cdn-optimization-overview/01_Akamai.png)

2. Selecteer onder instellingen **optimalisatie**. Selecteer een type in de **geoptimaliseerd voor** vervolgkeuzelijst.

    ![Selectie van optimalisatie en type](./media/cdn-optimization-overview/02_Select.png)

## <a name="optimization-for-specific-scenarios"></a>Optimalisatie voor specifieke scenario 's

U kunt het CDN-eindpunt voor een van deze scenario's optimaliseren. 

### <a name="general-web-delivery"></a>Algemene webweergave

Algemene webweergave is de meest voorkomende optimalisatieoptie. Het ontworpen voor algemene web content optimalisatie, zoals webpagina's en web-apps. Deze optimalisatie kan ook worden gebruikt voor het bestand en video worden gedownload.

Een typische website bevat statische en dynamische inhoud. Statische inhoud bevat afbeeldingen, JavaScript-bibliotheken en opmaakmodellen die kunnen worden opgeslagen in de cache en geleverd aan andere gebruikers. Dynamische inhoud is aangepast voor een afzonderlijke gebruiker, zoals nieuwsitems die zijn afgestemd op een gebruikersprofiel. Dynamische inhoud, zoals winkelwagentjes winkelwagen inhoud, is niet in cache opgeslagen omdat het is uniek voor elke gebruiker. Algemene webweergave kan uw hele website te optimaliseren. 

> [!NOTE]
> Als u een **Azure CDN Standard van Akamai** profiel, selecteert u dit type eindpuntoptimalisatie als de gemiddelde grootte kleiner is dan 10 MB. Anders selecteert u als de gemiddelde grootte groter dan 10 MB is, **grote bestanden downloaden** uit de **geoptimaliseerd voor** vervolgkeuzelijst.

### <a name="general-media-streaming"></a>Algemene mediastreaming

Als u nodig hebt om het eindpunt te gebruiken voor live streaming en video on demand streaming, de algemene streaming optimalisatie mediatype selecteren.

Mediastreaming is tijdgevoelig, omdat de pakketten die laat op de client binnenkomen, zoals regelmatig bufferen van video-inhoud, kunnen leiden tot een gaan van de weergave-ervaring. Optimalisatie van mediastreaming vermindert de latentie van de levering van media-inhoud en biedt een smooth streaming-ervaring voor gebruikers. 

In dit scenario is gebruikelijk voor Azure media service-klanten. Als u Azure mediaservices gebruiken, ontvangt u een één streaming-eindpunt dat kan worden gebruikt voor het streamen van live en on-demand. Met dit scenario moeten klanten niet overschakelen naar een ander eindpunt wanneer ze op aanvraag streamen van live wijzigen. Algemene optimalisatie van mediastreaming ondersteunt dit type scenario.

Voor **Azure CDN Standard van Microsoft**, **Azure CDN Standard van Verizon**, en **Azure CDN Premium van Verizon**, het optimalisatietype voor levering van gewoon op Internet te gebruiken algemene streaming mediainhoud leveren.

Zie voor meer informatie over optimalisatie van mediastreaming, [optimalisatie van mediastreaming](cdn-media-streaming-optimization.md).

### <a name="video-on-demand-media-streaming"></a>Video on demand streaming van media

Optimalisatie van mediastreaming video-on-demand verbetert de video on demand streaming-inhoud. Als u een eindpunt voor het streamen van video on demand gebruikt, moet u deze optie gebruiken.

Voor **Azure CDN Standard van Microsoft**, **Azure CDN Standard van Verizon**, en **Azure CDN Premium van Verizon** profielen, gebruikt u de optimalisatie van de levering van algemene web Typ voor het leveren van video on demand streaming media-inhoud.

Zie voor meer informatie over optimalisatie van mediastreaming, [optimalisatie van mediastreaming](cdn-media-streaming-optimization.md).

> [!NOTE]
> Als het CDN-eindpunt voornamelijk video-on-demand-inhoud fungeert, gebruikt u dit type eindpuntoptimalisatie. Het belangrijkste verschil tussen deze optimalisatietype en de algemene mediastreaming optimalisatietype is de verbindingstime-out probeer het opnieuw. De time-outperiode is veel korter om te werken met live streaming-scenario's.
>

### <a name="large-file-download"></a>Grote bestanden downloaden

Voor **Azure CDN Standard van Akamai** profielen, grote bestanden downloads zijn geoptimaliseerd voor inhoud die groter zijn dan 10 MB. Als de gemiddelde grootte kleiner dan 10 MB is, gebruik algemene webweergave. Als uw gemiddelde bestanden-grootten consistent groter is dan 10 MB zijn, kan het zijn efficiënter te maken van een afzonderlijk eindpunt voor grote bestanden. Firmware of software-updates zijn bijvoorbeeld meestal grote bestanden. De optimalisatie van grote bestanden downloaden is met het oog op bestanden die groter zijn dan 1.8 GB vereist.

Voor **Azure CDN Standard van Microsoft**, **Azure CDN Standard van Verizon**, en **Azure CDN Premium van Verizon** profielen, gebruikt u de optimalisatie van de levering van algemene web Typ voor het leveren van inhoud van grote bestanden downloaden. Er geldt geen beperking op downloadgrootte van het bestand.

Zie voor meer informatie over optimalisatie van grote bestanden, [optimalisatie van grote bestanden](cdn-large-file-optimization.md).

### <a name="dynamic-site-acceleration"></a>Dynamische siteversnelling

 Dynamic site acceleration (DSA) is beschikbaar voor **Azure CDN Standard van Akamai**, **Azure CDN Standard van Verizon**, en **Azure CDN Premium van Verizon** profielen. Deze optimalisatie worden extra kosten moet gebruiken. Zie voor meer informatie, [prijzen van Content Delivery Network](https://azure.microsoft.com/pricing/details/cdn/).

DSA bevat verschillende technieken die profiteren van de latentie en de prestaties van dynamische inhoud. Technieken zijn onder andere route en netwerk-optimalisatie en optimalisatie van TCP. 

Deze optimalisatie kunt u een web-app met verschillende antwoorden die niet gecachet kan worden te versnellen. Voorbeelden zijn zoekresultaten, afhandeling transacties of realtime-gegevens. U kunt echter ook doorgaan met de belangrijkste Azure CDN caching-mogelijkheden voor statische gegevens. 

Zie voor meer informatie over dynamische siteversnelling [dynamische siteversnelling](cdn-dynamic-site-acceleration.md).



