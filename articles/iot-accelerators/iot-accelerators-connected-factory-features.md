---
title: Verbonden Factory-oplossing functies - Azure | Microsoft Docs
description: Een overzicht van de functies van de verbonden Factory vooraf geconfigureerde oplossing.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: dobett
ms.openlocfilehash: af2a2c84f9eb420a7ca9a8bd5909cbf856d29a5e
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53309192"
---
# <a name="what-is-connected-factory-iot-solution-accelerator"></a>Wat is verbonden Factory IoT-oplossingsversnellers?

Verbonden Factory is een implementatie van Microsoft Azure industriële IoT-referentiearchitectuur, verpakt als op de open-source oplossing. U kunt deze gebruiken als uitgangspunt voor een commercieel product. U kunt een vooraf gemaakte versie van de verbonden Factory-oplossing implementeren in uw Azure-abonnement van [Azure IoT-oplossingsversnellers](https://www.azureiotsolutions.com/#solutions/types/CF).

![Dashboard van oplossing voor verbonden Factory](./media/iot-accelerators-connected-factory-features/dashboard.png)

De oplossingsversneller voor verbonden Factory [code is beschikbaar op GitHub](https://github.com/Azure/azure-iot-connected-factory).

Verbonden Factory bevat de volgende functies:

## <a name="industrial-device-interoperability"></a>Interoperabiliteit van industriële apparaten

- Verbinding maken met industriële activa met een OPC UA-interface.
- Gebruik de gesimuleerde productielijnen (met OPC UA-servers in Docker-containers) om te zien van live telemetrie van deze.
- Blader door het model van de OPC UA informatie van de OPC UA-servers uit een cloud-dashboard.

## <a name="remote-management"></a>Extern beheer

- Configureer uw OPC UA-activa van de cloud-dashboard (aanroep methoden, lezen en schrijven van gegevens).
- Publiceren en telemetriegegevens van uw OPC UA-assets van een cloud-dashboard de publicatie ongedaan maken.

## <a name="cloud-dashboard"></a>Cloud-dashboard

- Telemetrie previews rechtstreeks in een cloud-dashboard weergeven.
- Trends weergeven in de telemetrische gegevens en correlaties met behulp van de Time Series Insights Explorer-dashboard maken.
- Berekende algemene Equipment Efficiency (OEE) en Key Performance Indicators (KPI's) in een cloud-dashboard zien.
- Weergave industriële activa hiërarchieën in een boomstructuur-topologie, maar ook op een interactieve kaart.
- Weergeven, erkent en waarschuwingen uit een cloud-dashboard sluiten.

## <a name="azure-time-series-insights"></a>Azure Time Series Insights

- [Azure Time Series Insights](../time-series-insights/time-series-insights-overview.md) is gebouwd voor het opslaan, visualiseren en query's van grote hoeveelheden gegevens van de time series. Verbonden Factory maakt gebruik van deze service.
- Verbonden Factory's kan worden geïntegreerd met deze service waarmee u uitgebreide, in realtime analyse van de apparaatgegevens van uw uitvoeren.

## <a name="rules-and-alerts"></a>Regels en waarschuwingen

[Regels op basis van een drempelwaarde voor waarschuwingen configureren](iot-accelerators-connected-factory-configure.md).

## <a name="end-to-end-security"></a>End-to-end beveiliging

- Configureren van beveiligingsmachtigingen voor gebruikers met behulp van toegangsbeheer op basis van rollen (RBAC).
- End-to-end versleuteling is geïmplementeerd met behulp van de OPC UA-verificatie (met behulp van X.509-certificaten), evenals de beveiligingstokens.

## <a name="customizability"></a>Aanpassingsmogelijkheden

- De oplossing om te voldoen aan specifieke zakelijke behoeften aanpassen.
- Volledige oplossing-broncode is beschikbaar op GitHub. Zie de [vooraf geconfigureerde oplossing voor verbonden Factory](https://github.com/Azure/azure-iot-connected-factory) opslagplaats.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de oplossing Connected Factory preconfigured door te lezen van de volgende artikelen:

* [Overzicht van de oplossing voor vooraf geconfigureerde Connected Factory](iot-accelerators-connected-factory-sample-walkthrough.md)
* [Een gateway implementeren voor Verbonden Factory]( iot-accelerators-connected-factory-gateway-deployment.md)
