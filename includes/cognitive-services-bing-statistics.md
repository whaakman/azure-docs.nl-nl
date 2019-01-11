---
title: bestand opnemen
description: bestand opnemen
services: cognitive-services
author: scottwhi-msft
ms.service: cognitive-services
ms.topic: include
ms.date: 04/09/2018
ms.author: scottwhi
ms.custom: include file
ms.openlocfilehash: 6016b13fe7d3e1f3b673bd2446d2f68b04878cd6
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54193361"
---
Bing statistieken biedt analytics voor Bing zoeken-API's. Analytics bevat aanroepvolume, top queryreeksen en geografische verdeling. Om in te schakelen Bing statistieken in uw Bing Search betaald abonnement, gaat u naar uw [Azure-dashboard](https://portal.azure.com/#create/Microsoft.CognitiveServicesBingSearch-v7), selecteert u uw betaalde abonnement en klikt u op inschakelen Bing statistieken. Het aantal abonnementen iets inschakelen van Bing statistieken verhoogt (Zie [prijzen](https://aka.ms/bingstatisticspricing)).

> [!NOTE]
> Bing statistieken is beschikbaar voor betaalde abonnementen alleen - is niet beschikbaar met gratis proefabonnementen.

> [!NOTE]
> U mag geen gegevens beschikbaar zijn via het dashboard Bing statistieken niet gebruiken om het maken van toepassingen om te worden gedistribueerd naar derden beperkt.

Bing analytics-gegevens om de 24 uur bijgewerkt en onderhouden van op 13 maanden aan geschiedenis.

## <a name="accessing-your-analytics"></a>Toegang tot uw analyses

Voor toegang tot uw analytics-dashboard, gaat u naar https://bingapistatistics.com. Zorg ervoor dat u bent aangemeld met behulp van hetzelfde Microsoft-account (MSA) waarmee u uw betaalde abonnement.

## <a name="filtering-the-data"></a>Gegevens filteren

Standaard wordt alle metrische gegevens die u toegang tot hebt weer door de diagrammen en grafieken. U kunt de gegevens weergegeven in de diagrammen en grafieken door te selecteren van de resources, markten, eindpunten filteren en rapportageperiode u geïnteresseerd bent in. De diagrammen en grafieken wijzigen om de toegepaste filters weer te geven. De volgende beschrijven de filters die u kunt wijzigen.

- **Resource-ID**: De unieke resource-ID waarmee uw Azure-abonnement. De lijst bevat meerdere id's als u zich op meer dan één Bing zoeken-API-laag abonneert. Standaard worden alle resources geselecteerd.  
  
- **Markten**: De markten waar de resultaten afkomstig zijn uit. Bijvoorbeeld, en-us (Engels, Verenigde Staten). Overal zijn standaard ingeschakeld. Houd er rekening mee dat de markt en WW de markt die Bing gebruikt als de aanroep een markt niet is opgegeven en Bing is niet mogelijk te bepalen van de gebruiker markt.  
  
- **Eindpunten**: De Bing zoeken-API-eindpunten. De lijst bevat alle eindpunten waarvoor u een betaald abonnement hebt. Alle eindpunten zijn standaard ingeschakeld.  

- **Periode**: De rapportageperiode. U kunt opgeven:
  - Alle&mdash;voorziet naar 13 maanden aan gegevens van maximaal  
  - Afgelopen 24 uur&mdash;bevat analyses van de afgelopen 24 uur  
  - Afgelopen week&mdash;bevat analyse van de afgelopen zeven dagen  
  - Afgelopen maand&mdash;bevat analyses van de afgelopen 30 dagen  
  - Een aangepast datumbereik&mdash;bevat analyses van het opgegeven datumbereik, indien beschikbaar  

  > [!NOTE]  
  > Het duurt maximaal 24 uur voor metrische gegevens naar het ontwerpoppervlak voor pijplijnen op het dashboard. Het dashboard ziet u de datum en tijd die de gegevens voor het laatst is bijgewerkt.  

  > [!NOTE]  
  > Metrische gegevens zijn beschikbaar vanaf het moment dat u de invoegtoepassing Bing statistieken inschakelen.

## <a name="charts-and-graphs"></a>Diagrammen en grafieken

Het dashboard laat diagrammen en grafieken van de metrische gegevens beschikbaar zijn voor het geselecteerde eindpunt. Niet alle metrische gegevens zijn beschikbaar voor alle eindpunten. De diagrammen en grafieken voor elk eindpunt statisch zijn (u kunt niet selecteren de diagrammen en grafieken om weer te geven). Het dashboard laat alleen diagrammen en grafieken die er gegevens zijn.

<!--
For example, if you don't include the User-Agent header in your calls, the dashboard will not include device-related graphs.
-->

Hier volgen de mogelijke metrische gegevens. Elke metrische opmerkingen bij de eindpunt-beperkingen.

- **Aanroepen van Volume**: Geeft het aantal aanroepen tijdens de rapportageperiode. Als de rapportageperiode voor een dag is, toont de grafiek het aantal aanroepen per uur. Anders wordt toont de grafiek het aantal aanroepen per dag van de rapportageperiode.  
  
  > [!NOTE]
  > De aanroepvolume kan afwijken van de facturering rapporten, waaronder over het algemeen alleen geslaagde aanroepen.

- **Meest gebruikte query**: Toont de belangrijkste query's en het aantal exemplaren van elke query tijdens de rapportageperiode. U kunt het aantal query's die worden weergegeven. U kunt bijvoorbeeld de top 25, 50 of 75 query's weergeven. Top-query's is niet beschikbaar voor de volgende eindpunten:  

  - /images/trending
  - / afbeeldingen/details
  - / afbeeldingen/visualsearch
  - /videos/trending
  - / video's / details
  - /News
  - / nieuws/trendingtopics
  - /Suggestions  
  
  > [!NOTE]  
  > Querytermen kunnen worden onderdrukt als u wilt verwijderen van vertrouwelijke informatie, zoals e-mailberichten, telefoonnummers, SSN, enzovoort.

- **Geografische verdeling**: De markten waar de resultaten afkomstig zijn uit. Bijvoorbeeld, en-us (Engels, Verenigde Staten). Bing gebruikt de `mkt` queryparameter om te bepalen van de markt, indien opgegeven. Anders gebruikt Bing signalen, zoals IP-adres van de oproepende functie om te bepalen van de markt.

- **Responscode**: De HTTP-statuscodes van alle aanroepen tijdens de rapportageperiode.

- **Aanroepen van oorsprong distributie**: De typen browsers die worden gebruikt door de gebruikers. Bijvoorbeeld, Microsoft Edge, Chrome Safari en FireFox. Aanroepen van buiten een browser, zoals bots, Postman of met curl vanuit een console-app zijn gegroepeerd onder bibliotheken. De oorsprong wordt bepaald met behulp van de aanvraag gebruikersagent header-waarde. Als de aanvraag bevat geen koptekst van de gebruikersagent, probeert Bing worden afgeleid van de oorsprong van andere signalen.  

- **Veilig zoeken distributie**: De distributie van veilig zoekwaarden. Bijvoorbeeld, uitgeschakeld, Gemiddeld of strikte. De `safeSearch` parameter bevat de waarde als de opgegeven query. Bing standaard anders de waarde te nemen.  

- **Antwoorden aangevraagd distributie**: De webzoekopdrachten-API, beantwoordt u aangevraagd de `responseFilter` queryparameter.  

- **Antwoorden geretourneerd distributie**: De antwoorden die webzoekopdrachten-API in het antwoord geretourneerd.

- **Antwoord Server distributie**: De toepassingsserver die uw API-aanvragen wordt behandeld. De mogelijke waarden zijn Bing.com (voor verkeer worden aangeleverd vanuit desktop en laptop-apparaten) en Bing.com-mobile (voor verkeer worden aangeleverd vanuit de mobiele apparaten). De server wordt bepaald met behulp van de aanvraag gebruikersagent header-waarde. Als de aanvraag bevat geen koptekst van de gebruikersagent, probeert Bing worden afgeleid van de server van andere signalen.

Hieronder ziet u de analyse die beschikbaar voor elk eindpunt zijn.

![Distributie per eindpunt ondersteuningsmatrix](./media/cognitive-services-bing-statistics/bing-statistics-matrix.PNG)
