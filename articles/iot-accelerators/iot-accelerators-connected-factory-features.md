---
title: Verbonden onderdelen in de Factory-oplossing - Azure | Microsoft Docs
description: Een overzicht van de functies van de fabriek verbonden vooraf geconfigureerde oplossing.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: dobett
ms.openlocfilehash: 3478217771418ab31772d6a42a7ed8d8a2e8069a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34626493"
---
# <a name="what-is-connected-factory-iot-solution-accelerator"></a>Wat is verbonden Factory IoT oplossingsverbetering?

Verbonden Factory is een implementatie van Microsoft industriële Azure-IoT-referentiearchitectuur kan verpakt als op de open source-oplossing. U kunt deze gebruiken als uitgangspunt voor een commerciële product. U kunt een vooraf samengestelde versie van de verbonden Factory-oplossing implementeren in uw Azure-abonnement van [Azure IoT-oplossing accelerators](https://www.azureiotsolutions.com/#solutions/types/CF).

![Dashboard van de oplossing verbonden Factory](./media/iot-accelerators-connected-factory-features/dashboard.png)

Verbonden Factory omvat de volgende functies:

## <a name="industrial-device-interoperability"></a>Interoperabiliteit van industriële apparaat

- Verbinding maken met industriële activa met een OPC UA-interface.
- Gebruik de gesimuleerde productie-regels (uitgevoerd OPC UA servers in Docker-containers) om live telemetrie van deze.
- Blader in het informatiemodel OPC UA OPC UA-servers in een cloud-dashboard.

## <a name="remote-management"></a>Extern beheer

- Configureer uw assets OPC UA vanuit het dashboard cloud (aanroep methoden, lezen en schrijven van gegevens).
- Publish and unpublish of telemetriegegevens van uw assets OPC UA uit een cloud-dashboard.

## <a name="cloud-dashboard"></a>Cloud-dashboard

- Telemetrie previews rechtstreeks in een cloud-dashboard weergeven.
- Weergeven van trends in telemetriegegevens en correlaties met het Time Series Insights Explorer-dashboard maken.
- Zie berekende algehele apparatuur efficiëntie (OEE) en Key Performance Indicators (KPI's) van een cloud-dashboard.
- Weergave industriële asset hiërarchieën in een structuur-topologie, evenals op een interactieve kaart.
- Weergeven, erkent en sluiten van waarschuwingen van een cloud-dashboard.

## <a name="azure-time-series-insights"></a>Azure Time Series Insights

- [Inzicht van Azure Time Series](../time-series-insights/time-series-insights-overview.md) is gebouwd voor het opslaan en opvragen van grote hoeveelheden timeseries gegevens te visualiseren. Verbonden Factory maakt gebruik van deze service.
- Verbonden Factory kan worden geïntegreerd met deze service kunt u uitvoeren grondige, realtime analyse van de apparaatgegevens van uw.

## <a name="rules-and-alerts"></a>Regels en waarschuwingen

[Regels op basis van drempelwaarden voor waarschuwingen configureren](iot-accelerators-connected-factory-configure.md).

## <a name="end-to-end-security"></a>End-to-end-beveiliging

- Configureren van beveiligingsmachtigingen voor gebruikers met behulp van op rollen gebaseerde toegangsbeheer (RBAC).
- End-to-end-versleuteling wordt geïmplementeerd met OPC UA-verificatie (met X.509-certificaten), evenals beveiligingstokens.

## <a name="customizability"></a>Aanpassingsmogelijkheden

- De oplossing om te voldoen aan specifieke zakelijke vereisten aanpassen.
- Volledige oplossing-broncode is beschikbaar op GitHub. Zie de [verbonden Factory vooraf geconfigureerde oplossing](https://github.com/Azure/azure-iot-connected-factory) opslagplaats.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de verbonden Factory vooraf geconfigureerde oplossing door te lezen van de volgende artikelen:

* [Oplossing walkthrough over vooraf geconfigureerde verbonden Factory](iot-accelerators-connected-factory-sample-walkthrough.md)
* [Een gateway implementeren voor Verbonden Factory]( iot-accelerators-connected-factory-gateway-deployment.md)
