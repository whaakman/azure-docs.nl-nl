---
title: Integreer SIM-gegevens in de bewaking van oplossing voor externe - Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe gegevens Telefónica SIM integreren met de oplossing voor externe controle.
author: hegate
manager: ''
ms.author: hegate
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 05/15/2018
ms.topic: conceptual
ms.openlocfilehash: ae8751f429cb4b11199bd8da9d8c2e08c0b98b35
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628280"
---
# <a name="integrate-sim-data-in-the-remote-monitoring-solution"></a>Integreer SIM-gegevens in de oplossing voor externe controle

IoT-apparaten is vaak verbinding maken met de cloud met behulp van een SIM-kaart waarmee ze gegevensstromen verzenden vanaf elke locatie. De externe controle van Azure IoT-oplossing kan de integratie van IoT beheerd connectiviteitsgegevens, zodat de operators kunnen ook de status van het apparaat via de gegevens van de IoT SIM bijhouden.

Externe controle biedt buiten de box-integratie met Telefónica IoT connectiviteit, zodat klanten die gebruikmaken van het Platform van de connectiviteit IoT hun apparaat connectiviteitsgegevens SIM-kaarten om hun oplossingen te synchroniseren. Deze oplossing kan worden uitgebreid ter ondersteuning van andere IoT-connectiviteitsproviders via GitHub [opslagplaats](http://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet).

In deze zelfstudie leert u het volgende:

* Integreer Telefónica IoT SIM-gegevens in de oplossing voor externe controle
* Realtime telemetrie van paginaweergaven
* SIM-gegevens weergeven

## <a name="telefnica-iot-integration-setup"></a>Telefónica IoT integratie-instellingen

### <a name="prerequisites"></a>Vereisten

Deze extra externe controle-functie is momenteel in preview. Volg deze stappen voor het synchroniseren van uw connectiviteitsgegevens in een oplossing voor externe controle Azure:

1. Vullen van de aanvraag op [Telefónica van site](https://iot.Telefónica.com/contact), selecteert u de optie **Azure externe controle**, met inbegrip van uw contactgegevens.
2. Telefónica wordt geactiveerd voor uw account.
3. Als u nog niet een client Telefónica en u wilt profiteren van dit of andere IoT connectiviteit Cloud gereed services, gaat u naar [Telefónica van site](https://iot.Telefónica.com/contact) en selecteer de optie **connectiviteit**.

### <a name="telefnica-sim-setup"></a>Telefónica SIM setup
Telefónica SIM & Azure Twin ID apparaatkoppeling is gebaseerd op de eigenschap 'alias' Telefónica IoT SIM. 

Navigeer naar [Telefónica IoT connectiviteit Platform Portal](https://m2m-movistar-es.telefonica.com/) > SIM-inventarisatie > Selecteer de SIM-kaart, en elke SIM 'alias' bijwerken met de gewenste Twin deviceID. Deze taak kan ook worden uitgevoerd in bulkmodus (Zie handleidingen Telefónica IoT connectiviteit Platform).

Deze taak kan ook worden uitgevoerd in bulkmodus (Zie Telefónica IoT connectiviteit Platform handleidingen)

![Telefónica Update](./media/iot-accelerators-remote-monitoring-telefonica-sim/telefonica_site.png)

Als u wilt uw apparaat aansluit op de externe controle, kunt u deze zelfstudies met [C](iot-accelerators-connecting-devices-linux.md) of [knooppunt](iot-accelerators-connecting-devices-node.md). 

## <a name="view-device-telemetry-and-sim-properties"></a>De apparaattelemetrie en SIM-eigenschappen weergeven

Wanneer uw account Telefónica correct is geconfigureerd en uw apparaat is verbonden, kunt u de Apparaatdetails van het en SIM-gegevens kunt weergeven.

De volgende parameters voor de connectiviteit worden gepubliceerd:

* ICCID
* IP
* Netwerk aanwezigheid
* SIM-Status
* Locatie op het netwerk
* Verbruikte gegevensverkeer

![Dashboard](./media/iot-accelerators-remote-monitoring-telefonica-sim/dashboard.png)

## <a name="next-steps"></a>Volgende stappen

Nu dat u een overzicht van het integreren van SIM-gegevens in Azure IoT externe controle hebt, moet u hier voorgestelde Vervolgstappen voor oplossingen accelerators zijn:

* [De oplossing voor externe controle van Azure IoT werkt](iot-accelerators-remote-monitoring-explore.md)
* [Geavanceerde bewaking uitvoeren](iot-accelerators-remote-monitoring-monitor.md)
* [Uw apparaten beheren](iot-accelerators-remote-monitoring-manage.md)
* [Problemen met een apparaat oplossen](iot-accelerators-remote-monitoring-maintain.md)

