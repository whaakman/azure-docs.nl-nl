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
ms.openlocfilehash: 4e19c1afefdc5bcacebcb0d495193b48c7a6d724
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313707"
---
Bing statistieken voorziet in analyse voor Bing zoeken-API's. Analytics bevat Belvolume, bovenste queryreeksen en geografische verdeling. Om in te schakelen Bing statistieken in uw Bing-zoekopdracht betaald abonnement, gaat u naar uw [Azure-dashboard](https://portal.azure.com/#create/Microsoft.CognitiveServicesBingSearch-v7), selecteer uw betaalde abonnement en klikt u op inschakelen Bing statistieken. De snelheid van uw abonnement inschakelen Bing statistieken enigszins verhoogt (Zie [prijzen](https://aka.ms/bingstatisticspricing)).


> [!NOTE]
> Bing statistieken is beschikbaar voor betaalde abonnementen alleen - is niet beschikbaar met gratis proefabonnementen. 

> [!NOTE]
> U mag geen gegevens beschikbaar via het dashboard met Bing-statistieken niet gebruiken voor het maken van toepassingen voor distributie naar derden.

Bing analytische gegevens per 24 uur bijgewerkt en onderhoudt aan 13 maanden aan geschiedenis.

## <a name="accessing-your-analytics"></a>Toegang tot uw analyses

Voor toegang tot uw dashboard met analytische, gaat u naar https://bingapistatistics.com. Zorg ervoor dat u bent aangemeld bij het gebruik van hetzelfde Microsoft-account (MSA) waarmee u uw betaalde abonnement.


## <a name="filtering-the-data"></a>De gegevens filteren

Standaard weerspiegelen de grafieken en diagrammen alle metrische gegevens die u toegang tot hebt. U kunt de gegevens weergegeven in de grafieken en diagrammen door het selecteren van de resources, markten, eindpunten filteren en rapportageperiode u geïnteresseerd bent in. De grafieken en diagrammen aangepast aan de toegepaste filters. De volgende beschrijven de filters die u kunt wijzigen.

- **De resource-ID**: de unieke resource-ID waarmee u uw Azure-abonnement. De lijst bevat meerdere id's als u zich op meer dan één Bing zoeken-API-laag abonneert. Standaard worden alle bronnen geselecteerd.  
  
- **Markten**: de herkomst van de resultaten van markten. Bijvoorbeeld en-us (Engels, Verenigde Staten). Standaard worden alle markten geselecteerd. Houd er rekening mee dat de markt en WW de markt dat Bing gebruikt als de oproep geen een markt geeft en Bing kan niet vaststellen van de gebruiker markt.  
  
- **Eindpunten**: de Bing zoeken-API-eindpunten. De lijst bevat alle eindpunten waarvoor u een betaald abonnement hebt. Standaard worden alle eindpunten geselecteerd.  

- **Tijdsbestek**: de rapportageperiode. U kunt opgeven:  
  
  - Alle&mdash;up 13 maanden aan gegevens bevat  
  - Afgelopen 24 uur&mdash;omvat analytics van de afgelopen 24 uur  
  - Afgelopen week&mdash;omvat analytics van de afgelopen zeven dagen  
  - Afgelopen maand&mdash;omvat analytics van de afgelopen 30 dagen  
  - Een aangepast datumbereik&mdash;analytics uit het opgegeven datumbereik bevat, indien beschikbaar  
  
  > [!NOTE]  
  > Het duurt maximaal 24 uur voor metrische gegevens om op te halen op het dashboard. Het dashboard bevat de datum en tijd die de gegevens voor het laatst is bijgewerkt.  
  
  > [!NOTE]  
  > Metrische gegevens zijn beschikbaar vanaf het moment dat u de statistieken van Bing-invoegtoepassing inschakelen. 


## <a name="charts-and-graphs"></a>Grafieken

Het dashboard toont de grafieken van de metrische gegevens beschikbaar voor het geselecteerde eindpunt. Niet alle metrische gegevens zijn beschikbaar voor alle eindpunten. De grafieken en diagrammen voor elk eindpunt zijn statisch (u mogelijk niet selecteren de grafieken en diagrammen om weer te geven). Het dashboard toont alleen grafieken en grafieken waarvoor gegevens is. 

<!--
For example, if you don't include the User-Agent header in your calls, the dashboard will not include device-related graphs.
-->

Hier volgen de mogelijke metrische gegevens. Elke metrische opmerkingen bij de eindpunt-beperkingen. 

- **Aanroepen van Volume**: toont het aantal aanroepen tijdens de rapportageperiode. Als de rapportageperiode voor een dag, ziet de grafiek u het aantal aanroepen per uur. Anders wordt toont de grafiek het aantal aanroepen per dag van de rapportperiode.  
  
  > [!NOTE]
  > Het Belvolume kan afwijken van facturering rapporten, waaronder doorgaans alleen geslaagde aanroepen. 
  
-  **Meest gebruikte query**: toont de top-query's en het aantal exemplaren van elke query tijdens de rapportageperiode. U kunt het aantal query's die worden weergegeven. U kunt bijvoorbeeld de eerste 25, 50 of 75 query's weergeven. Top-query's is niet beschikbaar voor de volgende eindpunten:  
  
  - /images/trending
  - installatiekopieën/details
  - installatiekopieën/visualsearch
  - /videos/trending
  - video's / details
  - /News
  - nieuws/trendingtopics
  - /Suggestions  
  
  > [!NOTE]  
  > Querytermen kunnen worden onderdrukt als u wilt verwijderen van vertrouwelijke gegevens zoals e-mailberichten, telefoonnummers, sofi-nummer, enzovoort.  

- **Geografische verdeling**: de herkomst van de resultaten van markten. Bijvoorbeeld en-us (Engels, Verenigde Staten). Bing gebruikt de `mkt` queryparameter om te bepalen van de markt, indien opgegeven. Anders gebruikt Bing signalen zoals IP-adres van de oproepende functie om te bepalen van de markt.  
  
- **Antwoord Code distributie**: de HTTP-statuscodes van alle aanroepen tijdens de rapportageperiode.  
  
- **Aanroepen van de oorsprong distributie**: de typen browsers die door de gebruikers worden gebruikt. Bijvoorbeeld, rand, Chrome Safari en FireFox. Aanroepen van buiten een browser, zoals bots, Postman of met curl vanuit een console-app zijn gegroepeerd onder bibliotheken. De oorsprong wordt bepaald met behulp van de aanvraag gebruikersagent header-waarde. Als de aanvraag niet de gebruikersagent-koptekst bevat, probeert de Bing worden afgeleid van de oorsprong van andere signalen.  
  
- **Veilige distributie van de zoekopdracht**: de distributie van veilige zoekactie waarden. Bijvoorbeeld, uitschakelen, Gemiddeld of strict. De `safeSearch` parameter bevat de waarde als de opgegeven query. Bing standaard anders de waarde te nemen.  
  
- **Distributie aangevraagd beantwoordt**: zoeken om de Web API beantwoordt u aangevraagd de `responseFilter` queryparameter.  
  
- **Verdeling geretourneerd beantwoordt**: de antwoorden die Web-API voor zoeken in het antwoord geretourneerd.  
  
- **Antwoord Server distributie**: de toepassingsserver die uw API-aanvragen in behandeling genomen. De mogelijke waarden zijn Bing.com (voor verkeer aangeboden via apparaten desktops en laptops) en Bing.com-mobile (voor verkeer aangeboden via mobiele apparaten). De server is bepaald met behulp van de aanvraag gebruikersagent header-waarde. Als de aanvraag niet de gebruikersagent-koptekst bevat, probeert de Bing worden afgeleid van de server van andere signalen.  
  


Hieronder ziet u de analyse die beschikbaar voor elk eindpunt zijn.

![Distributie door de ondersteuningsmatrix voor eindpunt](./media/cognitive-services-bing-statistics/bing-statistics-matrix.PNG)


