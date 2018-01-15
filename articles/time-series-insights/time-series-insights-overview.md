---
title: Wat is Azure Time Series Insights? | Microsoft Docs
description: Inleiding tot Azure Time Series Insights, een nieuwe service voor het time series-gegevensanalyse en IoT-oplossingen.
services: time-series-insights
ms.service: time-series-insights
author: ashannon7
ms.author: anshan
manager: jhubbard
editor: MarkMcGeeAtAquent
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 11/15/2017
ms.openlocfilehash: 2a0d3e829ed28912b56a022aa6aa82ade1718e35
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/13/2018
---
# <a name="what-is-azure-time-series-insights"></a>Wat is Azure Time Series Insights?

Time Series Insights is gebouwd voor het opslaan, visualiseren en opvragen van grote hoeveelheden tijd reeksgegevens, zoals die worden gegenereerd door de IoT-apparaten.  Als u wilt opslaan, beheren, opvragen of visualiseren time series-gegevens in de cloud, is Time Series Insights geschikt voor u.  

Time Series Insights heeft vier belangrijke taken:

- Eerst, het volledig geïntegreerd met cloud-gateways zoals Azure IoT Hub en Azure Event Hubs. Eenvoudig verbinding maakt met deze bronnen van gebeurtenissen en JSON parseert van berichten en -structuren die gegevens in schone rijen en kolommen hebben. Het lid wordt van de metagegevens met Telemetrie en indexen van uw gegevens in een store, op kolommen.
- Ten tweede beheert Time Series Insights de opslag van uw gegevens. Gegevens altijd eenvoudig toegankelijk is, zodat gegevens worden opgeslagen uw van geheugen en SSD van maximaal 400 dagen. U kunt interactief miljarden gebeurtenissen (in seconden): op aanvraag opvragen.
- Tot slot biedt Time Series Insights out-of-the-box-visualisatie via de Verkenner TSI.  
- Time Series Insights biedt vierde, een queryservice, zowel in de Verkenner TSI en met behulp van API's die eenvoudig kunnen worden geïntegreerd voor het insluiten van de tijd reeksgegevens in aangepaste toepassingen.  

Als u bij het bouwen van een toepassing, voor intern verbruik of voor externe klanten moet worden gebruikt, kan Time Series Insights worden gebruikt als een back-end voor indexeren, opslaan en samenvoegen van reeksgegevens. U kunt een aangepaste visualisatie en gebruikerservaring samenstellen op de voorgrond.  Time Series Insights beschrijft Query-API's om dit scenario.  

Als u zeker weet of uw gegevens tijdreeks is, is wat u moet weten.  Tijd van reeksgegevens vertegenwoordigt hoe een actief of proces gedurende een bepaalde periode verandert.  Deze is uniek in dat er een tijdstempel en meest zinvolle als een as is.  Tijd van reeksgegevens doorgaans binnenkomt in volgorde van de tijd en meestal wordt behandeld als een invoeging in plaats van een update voor uw database.  Omdat de Time Series Insights legt vast en slaat elke nieuwe gebeurtenis als een rij, wordt wijziging gemeten gedurende een periode, zodat u achterwaarts zoeken en te voorspellen van toekomstige wijziging.  In grote volumes, kunt opslaan, het indexeren, het uitvoeren van query's, analyseren en gegevens te visualiseren reeks lastig zijn.  

## <a name="primary-scenarios"></a>Primaire scenario's

- Time series gegevens opslaan in een schaalbare manier.  
  - De kern heeft Time Series Insights een database die is ontworpen met een reeks tijdgegevens in gedachten.  Omdat het schaalbare en volledig beheerde, verwerkt Time Series Insights het werk van opslaan en beheren van gebeurtenissen.

- Bijna realtime gegevensverkenning.  
  - Time Series Insights biedt een explorer dat alle gegevens in een omgeving streaming visualiseren.  Kort nadat u verbinding maakt met een gebeurtenisbron, kunt u gebeurtenisgegevens bekijken, explored en binnen de Time Series Insights van de query.  De gegevens zijn handig voor het valideren of een apparaat dat gegevens zoals verwacht en bewaking van een activum IoT voor status, de productiviteit en de algehele effectiviteit.  

- Hoofdoorzaak analyse en afwijkingsdetectie van de detectie.
  - Time Series Insights bevat hulpprogramma's zoals patronen en Perspectiefweergaven voor het uitvoeren en opslaan van meerdere stappen hoofdoorzaak worden geanalyseerd.  Bovendien Time Series Insights werkt in combinatie met services zoals Azure Stream Analytics waarschuwingen, zodat waarschuwingen en gedetecteerde afwijkingen kunnen worden weergegeven near-realtime in tijd reeks Insights explorer.  

- Een globale weergave van tijd reeksgegevens streaming vanaf verschillende locaties voor vergelijking van meerdere asset/site.
  - U kunt meerdere bronnen van gebeurtenissen verbinden met een tijd reeks Insights-omgeving.  Dit betekent dat gegevens streaming vanaf meerdere, verschillende locaties kunnen worden bekeken samen bijna realtime.  Gebruikers kunnen profiteren van deze zichtbaarheid gegevens delen met leidinggevenden en inschakelen van betere samenwerking met domein deskundigen die hun expertise voor het oplossen van problemen, kunt toepassen toepassen van best practices en geleerde lessen delen.

- Opbouwen van een toepassing boven op tijd reeks Insights. 
  - Time Series Insights beschrijft de REST-API-Query's, zodat u kunt het ontwikkelen van toepassingen die gebruikmaken van tijd reeksgegevens.

## <a name="capabilities"></a>Functionaliteit

- **Snel aan de slag:** Azure Time Series Insights zonder gegevensvoorbereiding vooraf vereist. Verbinding maken met miljoenen gebeurtenissen in uw Azure IoT Hub of Event Hub in minuten. Eenmaal zijn verbonden, visualiseren en communiceren met sensorgegevens uw IoT-oplossingen snel te valideren. U kunt werken met uw gegevens zonder code te schrijven.
Er is geen nieuwe taal voor meer informatie over; Time Series Insights biedt een gedetailleerde, vrije tekst query oppervlak voor ervaren gebruikers en en klik op verkennen.
- **In de buurt van realtime-inzichten:** Time Series Insights miljoenen gebeurtenissen per dag, sensor kan opnemen met een latentie van één minuut. Time Series Insights kunt u inzicht in uw sensorgegevens helpen u trends analyseren en afwijkingen, hoofdoorzaak analyses uitvoeren en dure uitvaltijd voorkomen. Door kruiscorrelatie mogelijk te maken tussen realtime en historische gegevens, stelt Time Series Insights u in staat om verborgen trends in uw gegevens bloot te leggen.
- **Aangepaste oplossingen bouwen:** gegevens insluiten Azure Time Series inzicht in uw bestaande toepassingen of maken van nieuwe aangepaste oplossingen met Time Series Insights REST-API's. Maak persoonlijke weergaven dat kunt u inzicht in uw gegevens verkennen anderen delen.
- **Schaalbaarheid:** Time Series Insights is ontworpen ter ondersteuning van IoT op grote schaal. Kan het inkomend van 1 miljoen tot 100 miljoen gebeurtenissen per dag met een bewaartermijn standaard span met 31 dagen. U kunt visualiseren en analyseren van live gegevens stromen in near-realtime naast historische gegevens. U verder gaat, wordt inkomende en retentie tarieven voor ondernemingen verhoogd.

## <a name="getting-started"></a>Aan de slag
Aan de slag duurt minder dan 5 minuten. 

1.  Om op te halen is gestart, inrichting een omgeving Time Series inzicht in de Azure portal. 
2.  Verbinding maken met een gebeurtenisbron zoals een Azure-IoT-Hub of Event Hub.  
3.  Upload referentiegegevens (dit is geen aanvullende service).
4.  Zie de gegevens in minuten met de Time Series Insights explorer.

## <a name="time-series-insights-explorer"></a>Time Series Insights Explorer
Dit diagram toont een voorbeeld van de tijdreeks inzichtgegevens weergegeven via de Verkenner:! [Time Series Insights explorer] (media/time-series-insights-explorer/explorer4.png)


## <a name="next-steps"></a>Volgende stappen
 - [Time Series Insights explorer gebruiken in een omgeving met demonstratie verkennen](./time-series-quickstart.md)
 - [Uw eigen omgeving Time Series Insights plannen](time-series-insights-environment-planning.md)

