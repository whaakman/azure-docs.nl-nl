---
title: Verbonden Factory-oplossing functies - Azure | Microsoft Docs
description: Een overzicht van de functies van de verbonden Factory vooraf geconfigureerde oplossing.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: dobett
ms.openlocfilehash: 2a11640959a8c7fdd0d238aba92698eb47934969
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67080454"
---
# <a name="what-is-connected-factory-iot-solution-accelerator"></a>Wat is verbonden Factory IoT-oplossingsversnellers?

Verbonden Factory is een implementatie van Microsoft Azure industriële IoT-referentiearchitectuur, verpakt als op de open-source oplossing. U kunt deze gebruiken als uitgangspunt voor een commercieel product. U kunt een vooraf gemaakte versie van de verbonden Factory-oplossing implementeren in uw Azure-abonnement van [Azure IoT-oplossingsversnellers](https://www.azureiotsolutions.com/#solutions/types/CF).

![Dashboard van oplossing voor verbonden Factory](./media/iot-accelerators-connected-factory-features/dashboard.png)

De oplossingsversneller voor verbonden Factory [code is beschikbaar op GitHub](https://github.com/Azure/azure-iot-connected-factory).

Verbonden Factory bevat de volgende functies:

## <a name="industrial-device-interoperability"></a>Interoperabiliteit van industriële apparaten

- Maak verbinding met industriële activa met een OPC UA-interface.
- Gebruik de gesimuleerde productielijnen (met OPC UA-servers in Docker-containers) om te zien van live telemetrie van deze.
- Blader door het model van de OPC UA informatie van de OPC UA-servers uit een cloud-dashboard.

## <a name="remote-management"></a>Extern beheer

- Configureer uw OPC UA-activa vanuit het clouddashboard (aanroepmethoden, lezen en schrijven van gegevens).
- Publiceer telemetriegegevens van uw OPC UA-activa en maak de publicatie hiervan ongedaan vanuit een clouddashboard.

## <a name="cloud-dashboard"></a>Clouddashboard

- Telemetrie previews rechtstreeks in een cloud-dashboard weergeven.
- Geef trends in de telemetriegegevens weer en maak correlaties met het Time Series Insights Explorer-dashboard.
- Berekende algemene Equipment Efficiency (OEE) en Key Performance Indicators (KPI's) in een cloud-dashboard zien.
- Weergave industriële activa hiërarchieën in een boomstructuur-topologie, maar ook op een interactieve kaart.
- Weergeven, erkent en waarschuwingen uit een cloud-dashboard sluiten.

## <a name="azure-time-series-insights"></a>Azure Time Series Insights

- [Azure Time Series Insights](../time-series-insights/time-series-insights-overview.md) is gebouwd voor het opslaan, visualiseren en query's van grote hoeveelheden gegevens van de time series. Verbonden Factory maakt gebruik van deze service.
- Verbonden Factory's kan worden geïntegreerd met deze service waarmee u uitgebreide, in realtime analyse van de apparaatgegevens van uw uitvoeren.

## <a name="rules-and-alerts"></a>Regels en waarschuwingen

[Regels op basis van een drempelwaarde voor waarschuwingen configureren](iot-accelerators-connected-factory-configure.md).

## <a name="end-to-end-security"></a>End-to-end beveiliging

- Configureer beveiligingsmachtigingen voor gebruikers met op rollen gebaseerd toegangsbeheer (RBAC).
- End-to-end versleuteling is geïmplementeerd met behulp van de OPC UA-verificatie (met behulp van X.509-certificaten), evenals de beveiligingstokens.

## <a name="customizability"></a>Aanpassingsmogelijkheden

- De oplossing om te voldoen aan specifieke zakelijke behoeften aanpassen.
- Volledige oplossing-broncode is beschikbaar op GitHub. Zie de [vooraf geconfigureerde oplossing voor verbonden Factory](https://github.com/Azure/azure-iot-connected-factory) opslagplaats.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de oplossingsverbetering voor verbonden Factory, de snelstartgids [een cloud-gebaseerde oplossing voor het beheren van mijn industriële IoT-apparaten proberen](quickstart-connected-factory-deploy.md).
