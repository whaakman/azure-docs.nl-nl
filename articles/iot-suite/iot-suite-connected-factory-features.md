---
title: Verbonden onderdelen in de Factory-oplossing - Azure | Microsoft Docs
description: Een overzicht van de functies van de fabriek verbonden vooraf geconfigureerde oplossing.
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: ''
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/20/2018
ms.author: dobett
ms.openlocfilehash: fea9ccc53bd019039cf1e989d72db7a218e4517c
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="what-is-azure-iot-suite-connected-factory"></a>Wat is Azure IoT Suite verbonden Factory?

Verbonden Factory is een implementatie van Microsoft industriële Azure-IoT-referentiearchitectuur kan verpakt als op de open source-oplossing. U kunt deze gebruiken als uitgangspunt voor een commerciële product. U kunt een vooraf samengestelde versie van de verbonden Factory-oplossing implementeren in uw Azure-abonnement van [Azure IoT Suite](https://www.azureiotsuite.com/#solutions/types/CF).

![Dashboard van de oplossing verbonden Factory](media/iot-suite-connected-factory-features/dashboard.png)

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
- Verbonden factory kan worden geïntegreerd met deze service kunt u uitvoeren grondige, realtime analyse van de apparaatgegevens van uw.

## <a name="rules-and-alerts"></a>Regels en waarschuwingen

[Regels op basis van drempelwaarden voor waarschuwingen configureren](iot-suite-connected-factory-configure.md).

## <a name="end-to-end-security"></a>End-to-end-beveiliging

- Configureren van beveiligingsmachtigingen voor gebruikers met behulp van op rollen gebaseerde toegangsbeheer (RBAC).
- End-to-end-versleuteling wordt geïmplementeerd met OPC UA-verificatie (met X.509-certificaten), evenals beveiligingstokens.

## <a name="customizability"></a>Aanpassingsmogelijkheden

- [Aanpassen](iot-suite-v1-guidance-on-customizing-preconfigured-solutions.md) de oplossing te voldoen aan specifieke zakelijke vereisten.
- Volledige oplossing-broncode is beschikbaar op GitHub. Zie de [verbonden factory vooraf geconfigureerde oplossing](https://github.com/Azure/azure-iot-connected-factory) opslagplaats.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de verbonden factory vooraf geconfigureerde oplossing door te lezen van de volgende artikelen:

* [Overzicht van verbonden factory vooraf geconfigureerde oplossing](iot-suite-connected-factory-sample-walkthrough.md)
* [Implementeer een gateway voor verbonden factory]( iot-suite-connected-factory-gateway-deployment.md)
