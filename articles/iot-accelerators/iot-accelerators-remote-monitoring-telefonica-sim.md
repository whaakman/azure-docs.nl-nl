---
title: Integreer SIM-gegevens in the Remote Monitoring Solution - Azure | Microsoft Docs
description: Dit artikel wordt beschreven hoe u Telefónica SIM-gegevens integreert in de oplossing voor externe controle.
author: hegate
manager: ''
ms.author: hegate
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 05/15/2018
ms.topic: conceptual
ms.openlocfilehash: c453998eea2a747b2cb608482f0ef9c1ee197ee0
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185428"
---
# <a name="integrate-sim-data-in-the-remote-monitoring-solution"></a>SIM-gegevens in de oplossing voor externe controle integreren

IoT-apparaten is vaak verbinding maken met de cloud met behulp van een SIM-kaart waarmee ze gegevensstromen verzenden vanaf elke locatie. De externe controle van Azure IoT-oplossing maakt de integratie van beheerde connectiviteit van IoT-gegevens, zodat operators kunnen ook de status van het apparaat wordt via de gegevens van de IoT SIM bijhouden.

Externe controle biedt buiten de box-integratie met Telefónica IoT-connectiviteit, zodat klanten die gebruikmaken van het Platform van IoT-connectiviteit hun apparaat connectiviteitsgegevens van SIM-kaarten om hun oplossingen te synchroniseren. Deze oplossing kan worden uitgebreid ter ondersteuning van andere leveranciers van IoT-connectiviteit via GitHub [opslagplaats](http://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet).

In deze zelfstudie leert u het volgende:

* Integreer Telefónica IoT SIM-gegevens in de oplossing voor externe controle
* Telemetrie in realtime weergeven
* SIM-gegevens weergeven

## <a name="telefnica-iot-integration-setup"></a>Installatie van de integration Telefónica IoT

### <a name="prerequisites"></a>Vereisten

Deze extra bewaking op afstand-functie is momenteel in preview. Als u wilt synchroniseren uw verbindingsgegevens in de oplossing voor externe bewaking Azure, de volgende stappen uit:

1. Vul een verzoek in via [Telefónica van site](https://iot.telefonica.com/contact), selecteert u de optie **Azure Remote Monitoring**, met inbegrip van de gegevens van uw contactpersonen.
2. Telefónica wordt geactiveerd voor uw account.
3. Als u nog niet een client Telefónica en u wilt profiteren van deze of andere IoT-connectiviteit Cloud gereed services, gaat u naar [Telefónica van site](https://iot.telefonica.com/) en selecteer de optie **connectiviteit**.

### <a name="telefnica-sim-setup"></a>Telefónica SIM-installatie
Telefónica SIM & Azure dubbele apparaat-ID-koppeling is gebaseerd op de eigenschap 'alias' Telefónica IoT SIM. 

Navigeer naar [Telefónica IoT connectiviteit Platform Portal](https://m2m-movistar-es.telefonica.com/) > SIM-inventarisatie > Selecteer de SIM-kaart en elk SIM 'alias' werken met uw gewenste dubbele apparaat-id. Deze taak kan ook worden uitgevoerd in bulkmodus (Zie handleidingen Telefónica IoT connectiviteit Platform).

Deze taak kan ook worden uitgevoerd in bulkmodus (Zie Telefónica IoT connectiviteit Platform-handleidingen)

![Telefónica Update](./media/iot-accelerators-remote-monitoring-telefonica-sim/telefonica_site.png)

Als u wilt uw apparaat aansluiten op de externe controle, kunt u deze met behulp van zelfstudies volgen [C](iot-accelerators-connecting-devices-linux.md) of [knooppunt](iot-accelerators-connecting-devices-node.md). 

## <a name="view-device-telemetry-and-sim-properties"></a>Telemetrie van apparaten weergeven en SIM-eigenschappen

Nadat uw account Telefónica correct is geconfigureerd en uw apparaat is verbonden, kunt u de Apparaatdetails van het en SIM-gegevens kunt weergeven.

De volgende parameters voor de verbindingen worden gepubliceerd:

* ICCID
* IP
* Netwerk aanwezigheid
* SIM-Status
* Op het netwerk locatie
* Verbruikte gegevens-verkeer

![Dashboard](./media/iot-accelerators-remote-monitoring-telefonica-sim/dashboard.png)

## <a name="next-steps"></a>Volgende stappen

Nu dat u een overzicht van hoe u SIM-gegevens in Azure IoT Remote Monitoring integreert hebt, moet u hier voorgestelde volgende stappen voor oplossingen accelerators zijn:

* [De oplossing Azure IoT Remote Monitoring werken](quickstart-remote-monitoring-deploy.md)
* [Geavanceerde bewaking uitvoeren](iot-accelerators-remote-monitoring-monitor.md)
* [Uw apparaten beheren](iot-accelerators-remote-monitoring-manage.md)
* [Problemen met een apparaat oplossen](iot-accelerators-remote-monitoring-maintain.md)

