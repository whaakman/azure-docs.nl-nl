---
title: Integreer SIM-gegevens in de bewaking van oplossing voor externe - Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe gegevens Telefonica SIM integreren met de oplossing voor externe controle.
services: iot-suite
suite: iot-suite
author: hegate
manager: corywink
ms.author: hegate
ms.service: iot-suite
ms.date: 04/30/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: c82bb8ff3d0f903e1de627f13337ae5fc633458c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="integrate-sim-data-in-the-remote-monitoring-solution"></a>Integreer SIM-gegevens in de oplossing voor externe controle

## <a name="overview"></a>Overzicht
IoT-apparaten is vaak verbinding maken met de cloud met behulp van een SIM-kaart waarmee ze gegevensstromen verzenden vanaf elke locatie. De externe controle van Azure IoT-oplossing kan de integratie van SIM-beheergegevens, zodat de operators kunnen ook de status van het apparaat via de gegevens van de SIM bijhouden. Externe controle biedt buiten de box-integratie met Telefonica IoT, zodat klanten die gebruikmaken van het Platform van de connectiviteit IoT synchroniseren van hun apparaat SIM-kaarten connectiviteitsgegevens de oplossingen. Deze oplossing kan worden uitgebreid ter ondersteuning van andere providers van het bedrijf telefoon via GitHub-opslagplaats.
In deze zelfstudie leert u het volgende:
* Integreer SIM-gegevens in de oplossing voor externe controle
* Realtime telemetrie van paginaweergaven
* SIM-gegevens weergeven 

## <a name="telefonica-iot-integration-setup"></a>Telefonica IoT integratie-instellingen

### <a name="prerequisites"></a>Vereisten
Volg deze stappen voor het synchroniseren van uw connectiviteitsgegevens in een oplossing voor externe controle Azure:

1.  Vullen van de aanvraag op [Telefonica van site](https://iot.telefonica.com/contact), selecteert u de optie **Azure externe controle**, met inbegrip van uw contactgegevens.
2.  Telefonica wordt uw account te activeren. 
3.  Als u nog niet een client Telefónica en u wilt profiteren van dit of andere IoT connectiviteit Cloud gereed services, gaat u naar [Telefonica van site](https://iot.telefonica.com/contact) en selecteer de optie **connectiviteit**.

### <a name="telefonica-sim-setup"></a>Telefonica SIM setup
Telefónica SIM & Azure Twin ID apparaatkoppeling wordt gebaseerd op de eigenschap 'alias' Telefónica IoT SIM. 

Navigeer naar [Telefónica IoT connectiviteit Platform Portal](https://m2m-movistar-es.telefonica.com/) > SIM-inventarisatie > Selecteer de SIM-kaart, en elke SIM 'alias' bijwerken met de gewenste Twin deviceID. 

Deze taak kan ook worden uitgevoerd in bulkmodus (Zie Telefónica IoT connectiviteit Platform handleidingen)

![Telefonica Update](media/iot-suite-remote-monitoring-telefonica/telefonica_site.png)

Als u wilt uw apparaat aansluit op de externe controle, kunt u deze zelfstudies met [C](iot-suite-connecting-devices-linux.md) of [knooppunt](iot-suite-connecting-devices-node.md). 

## <a name="view-device-telemetry-and-sim-properties"></a>De apparaattelemetrie en SIM-eigenschappen weergeven
Wanneer uw account Telefonica correct is geconfigureerd en uw apparaat is verbonden, kunt u de Apparaatdetails van het en SIM-gegevens kunt weergeven.
De volgende parameters voor de verbinding kunnen worden gepubliceerd:
* ICCID
* IP
* Netwerk aanwezigheid
* SIM-Status
* Locatie op het netwerk
* Verbruikte gegevensverkeer

![Dashboard](media/iot-suite-remote-monitoring-telefonica/dashboard.png)
 
## <a name="next-steps"></a>Volgende stappen

Nu dat u een overzicht van het integreren van SIM-gegevens in Azure IoT externe controle hebt, moet u hier voorgestelde Vervolgstappen voor oplossingen accelerators zijn:

* [De oplossing voor externe controle van Azure IoT werkt](iot-suite-remote-monitoring-explore.md)
* [Geavanceerde bewaking uitvoeren](iot-suite-remote-monitoring-monitor.md)
* [Uw apparaten beheren](iot-suite-remote-monitoring-manage.md)
* [Problemen met een apparaat oplossen](iot-suite-remote-monitoring-maintain.md)

