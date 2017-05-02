---
title: Overzicht van Azure Time Series Insights | Microsoft Docs
description: Inleiding tot Azure Time Series Insight, een nieuwe service voor de analyse van tijdreeksgegevens en IoT-oplossingen
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: santoshb
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/20/2017
ms.author: omravi
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: cc35c00bec18ee1d9ed52b29b68ddacc1136dc70
ms.lasthandoff: 04/25/2017

---

# <a name="overview-of-azure-time-series-insights"></a>Overzicht van Azure Time Series Insights

Azure Time Series Insights is een volledig beheerde cloudservice met opslag-, analyse- en visualisatieonderdelen waarmee u zeer gemakkelijk miljarden gebeurtenissen tegelijkertijd kunt ophalen, opslaan, verkennen en analyseren. Time Series Insights biedt u een integraal overzicht van uw gegevens, helpt u snel uw IoT-oplossing te valideren en kostbare stilstand van essentiële apparaten te vermijden door verborgen trends te ontdekken en afwijkingen te detecteren, en laat u in bijna realtime oorzaak-gevolganalyses uitvoeren. Time Series Insights haalt tijdreeksgegevens op van gebeurtenis-brokers (bijvoorbeeld IoT Hubs of Event Hubs), indexeert de gegevens en neemt gegevens uit gebruik op basis van een configureerbaar bewaarbeleid. Gebruikers gebruiken de gegevens voor hun bedrijfsscenario via een intuïtieve UX of REST Query API's.

![Overzicht van Time Series Insight](media/overview/time-series-insights-overview-flow.png)

## <a name="primary-scenarios"></a>Primaire scenario's

* Validatie en bewaking van IoT-oplossingen in enkele minuten
* Intuïtieve visualisatie en analyse van IoT-gegevens op de gewenste schaal
* Snellere hoofdoorzaakanalyse en afwijkingsdetectie
* Een integraal overzicht maken van meerdere apparaten, locaties en gegevens

## <a name="key-capabilities-and-benefits"></a>Belangrijkste mogelijkheden en voordelen

* **Gemakkelijk om aan de slag te gaan**: Azure Time Series Insight vereist geen gegevensvoorbereiding vooraf en is zeer snel, zodat u binnen enkele minuten verbinding kunt maken met miljarden gebeurtenissen in uw Azure IoT Hub of Event Hub. Nadat u verbinding hebt gemaakt, kunt u razendsnel visualiseren en communiceren met uw sensorgegevens om uw IoT-oplossing snel te valideren. Time Series Insights is uiterst gemakkelijk en eenvoudig te gebruiken; u kunt met uw gegevens werken zonder ook maar één regel code te hoeven schrijven.  Bovendien hoeft u geen nieuwe taal te leren, omdat Time Series een gedetailleerde queryruimte voor vrije tekst biedt voor ervaren gebruikers, evenals een wijs-en-klik-mogelijkheid voor beginners.

* **Bijna realtime inzicht in seconden**: haal meer uit uw tijdreeksgegevens met de onderdelen voor opslag, analyse en visualisatie van Time Series Insights, allemaal op één locatie. Time Series Insights kan honderden miljoenen sensorgebeurtenissen per dag opnemen, met slechts één minuut vertraging, zodat u snel op wijzigingen kunt reageren. Time Series Insights biedt u meer inzicht in uw sensorgegevens, doordat u trends en afwijkingen snel kunt herkennen, en vervolgens eenvoudig complexe hoofdoorzaakanalyses kunt uitvoeren en dure uitvaltijd kunt voorkomen. Door kruiscorrelatie mogelijk te maken tussen realtime en historische gegevens, stelt Time Series Insights gebruikers in staat om verborgen trends in hun gegevens bloot te leggen.

* **Aangepaste oplossingen bouwen**: integreer Azure Time Series Insights-gegevens in uw bestaande toepassingen of maak nieuwe aangepaste oplossingen met de Time Series Insights REST-API’s.  Bovendien is het maken en delen van persoonlijke weergaven een koud kunstje, waardoor anderen eenvoudig de ontdekkingen die u hebt gedaan, kunnen bekijken.

* **Schaalbaarheid**: Time Series Insights ondersteunt de IoT-schaal. In preview kan het 1 miljoen tot 100 miljoen gebeurtenissen per dag opnemen, met een standaardbewaartermijn van 31 dagen. Time Series Insights biedt gebruikers de mogelijkheid live gegevensstromen bijna in realtime te visualiseren en te analyseren, naast grote hoeveelheden historische gegevens. Op termijn zullen de opnamehoeveelheden en bewaartermijnen toenemen om te voldoen aan de alsmaar groeiende behoeften van grote bedrijven.

## <a name="taxonomy-of-time-series-insights"></a>Taxonomie van Time Series Insights

* **Omgeving**: een omgeving is een Azure-resource met inkomend verkeer en opslagcapaciteit.  Klanten richten omgevingen met de vereiste capaciteit in via Azure Portal.
* **Gebeurtenisbron**: een gebeurtenisbron is afgeleid van een gebeurtenis-broker, zoals Azure Event Hubs.  Time Series Insights maakt rechtstreeks verbinding met gebeurtenisbronnen en neemt de gegevensstroom op zonder dat gebruikers ook maar één coderegel hoeven te schrijven. Op dit moment ondersteunt Time Series Insights Azure Event Hubs en Azure IoT Hubs. In de toekomst worden meer gebeurtenisbronnen toegevoegd.
* **Referentiegegevens**: Time Series Insights biedt gebruikers de mogelijkheid om tijdreeksgegevens samen te voegen met referentiegegevens.  Referentiegegevens kunnen metagegevens bevatten over de apparaten of andere statische gegevens die relatief weinig veranderen. Time Series Insights voegt de referentiegegevens samen met gegevensstromen, zodat gebruikers deze gegevens in bijna realtime kunnen visualiseren en analyseren.

