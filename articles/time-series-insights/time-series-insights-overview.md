---
title: Overzicht van Azure Time Series Insights | Microsoft Docs
description: Inleiding tot Azure Time Series Insight, een nieuwe service voor de analyse van tijdreeksgegevens en IoT-oplossingen
keywords: 
services: tsi
documentationcenter: 
author: op-ravi
manager: jhubbard
editor: 
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/20/2017
ms.author: omravi
ms.openlocfilehash: 1814459e47280af62450a4093140ab6ab9b765fc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-azure-time-series-insights"></a>Wat is Azure Time Series Insights?

Azure Time Series Insights is een beheerde cloudservice met opslag-, analyse- en visualisatieonderdelen waarmee u gemakkelijk miljarden gebeurtenissen tegelijkertijd kunt ophalen, opslaan, verkennen en analyseren. Time Series Insights biedt u een integraal overzicht van uw gegevens, helpt u snel uw IoT-oplossing te valideren en kostbare stilstand van apparaten te vermijden door verborgen trends te ontdekken en afwijkingen te detecteren, en laat u in bijna realtime oorzaak-gevolganalyses uitvoeren. Time Series Insights haalt tijdreeksgegevens op van gebeurtenis-brokers (bijvoorbeeld IoT Hubs of Event Hubs), indexeert de gegevens en neemt gegevens uit gebruik op basis van een configureerbaar bewaarbeleid. Gebruikers gebruiken de gegevens via een intuïtieve UX of via REST-query-API's.

![Overzicht van Time Series Insight](media/overview/time-series-insights-overview-flow.png)

## <a name="primary-scenarios"></a>Primaire scenario's

* Bewaak en valideer IoT-oplossingen binnen enkele minuten.
* Visualiseer en analyseer grote hoeveelheden IoT-gegevens tegelijkertijd.
* Versnel hoofdoorzaakanalyse en afwijkingsdetectie.
* Stel een integraal overzicht samen van meerdere apparaten, locaties en gegevens.

## <a name="capabilities-and-benefits"></a>Mogelijkheden en voordelen

* **Eenvoudig aan de slag gaan**: Azure Time Series Insights is zeer snel en vereist geen gegevensvoorbereiding vooraf. U maakt in slechts enkele minuten verbinding met miljarden gebeurtenissen in uw Azure IoT Hub of Event Hub. Na het tot stand brengen van de verbinding kunt u binnen luttele seconden sensorgegevens visualiseren en bewerken om uw IoT-oplossing snel te valideren. Time Series Insights is gemakkelijk te gebruiken; u kunt met uw gegevens werken zonder ook maar één regel code te hoeven schrijven.  Bovendien hoeft u geen nieuwe taal te leren, omdat Time Series een gedetailleerde queryruimte voor vrije tekst biedt voor ervaren gebruikers, evenals een wijs-en-klik-mogelijkheid voor beginners.

* **Bijna realtime inzichten**: Time Series Insights kan honderden miljoenen sensorgebeurtenissen per dag opnemen, met slechts één minuut latentie, zodat u snel op wijzigingen kunt reageren. Time Series Insights biedt u meer inzicht in uw sensorgegevens, doordat u trends en afwijkingen snel herkent en efficiënt complexe hoofdoorzaakanalyses kunt uitvoeren. Zo voorkomt u dure uitvaltijd. Door kruiscorrelatie mogelijk te maken tussen realtime en historische gegevens, stelt Time Series Insights u in staat om verborgen trends in uw gegevens bloot te leggen.

* **Aangepaste oplossingen bouwen**: integreer Azure Time Series Insights-gegevens in uw bestaande toepassingen of maak nieuwe aangepaste oplossingen met de Time Series Insights REST-API’s. U kunt bovendien persoonlijke weergaven maken en delen, zodat ook anderen uw ontdekkingen kunnen bekijken.

* **Schaalbaarheid**: Time Series Insights ondersteunt zowel kleine als zeer grote hoeveelheden IoT-gegevens. In preview kunnen er 1 miljoen tot 100 miljoen gebeurtenissen per dag worden opgenomen, met een standaardbewaartermijn van 31 dagen. U hebt de mogelijkheid om live gegevensstromen bijna in realtime te visualiseren en te analyseren, en beschikt ook over enorme hoeveelheden historische gegevens. Op termijn zullen de opnamehoeveelheden en bewaartermijnen verder toenemen om te voldoen aan de alsmaar groeiende behoeften van grote bedrijven.

## <a name="time-series-insights-glossary"></a>Time Series Insights-terminologie

* **Omgeving**: een omgeving is een Azure-resource met inkomend verkeer en opslagcapaciteit.  Klanten richten via Azure Portal hun omgeving in met de vereiste capaciteit.
* **Gebeurtenisbron**: een gebeurtenisbron is afgeleid van een gebeurtenis-broker, zoals Azure Event Hubs.  Time Series Insights maakt rechtstreeks verbinding met gebeurtenisbronnen en neemt de gegevensstroom op zonder dat hiervoor code hoeft te worden geschreven. Op dit moment biedt Time Series Insights ondersteuning voor Azure Event Hubs en Azure IoT Hubs.
* **Referentiegegevens**: Time Series Insights biedt gebruikers de mogelijkheid om tijdreeksgegevens samen te voegen met referentiegegevens.  Referentiegegevens kunnen metagegevens bevatten over de apparaten of andere statische gegevens die relatief weinig veranderen. Time Series Insights voegt de referentiegegevens samen met gegevensstromen, zodat gebruikers deze gegevens in bijna realtime kunnen visualiseren en analyseren.
