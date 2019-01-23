---
title: Gesimuleerd apparaat gedrag in de oplossing voor externe controle - Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe u JavaScript gebruiken om te bepalen het gedrag van een gesimuleerd apparaat in de oplossing voor externe controle.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: c2151a4b1eb2a853ed343f6720b4f53af5e5e449
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54449517"
---
# <a name="implement-the-device-model-behavior"></a>Gedrag van het Apparaatmodel implementeren

Het artikel [schema van het Apparaatmodel begrijpen](iot-accelerators-remote-monitoring-device-schema.md) beschreven van het schema dat u een gesimuleerde apparaat-model definieert. Dit artikel waarnaar wordt verwezen naar twee typen van JavaScript-bestand die het gedrag van een gesimuleerd apparaat implementeren:

- **Status** JavaScript-bestanden die worden uitgevoerd met vaste intervallen om bij te werken van de interne status van het apparaat.
- **Methode** JavaScript-bestanden die worden uitgevoerd wanneer de oplossing een methode op het apparaat roept.

> [!NOTE]
> Model apparaatgedrag zijn alleen voor de gesimuleerde apparaten die worden gehost in de device simulatie-service. Als u maken van een echt apparaat wilt, Zie [uw apparaat aansluiten op de oplossingsverbetering voor externe controle](iot-accelerators-connecting-devices.md).

In dit artikel leert u het volgende:

>[!div class="checklist"]
> * Beheren van de status van een gesimuleerd apparaat
> * Definiëren hoe een gesimuleerd apparaat reageert op een methodeaanroep van de oplossing voor externe controle
> * Fouten opsporen in uw scripts

[!INCLUDE [iot-accelerators-device-behavior](../../includes/iot-accelerators-device-behavior.md)]

## <a name="next-steps"></a>Volgende stappen

Dit artikel wordt beschreven hoe u definieert het gedrag van uw eigen aangepaste gesimuleerde apparaat-model. In dit artikel hebt u geleerd hoe aan:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Beheren van de status van een gesimuleerd apparaat
> * Definiëren hoe een gesimuleerd apparaat reageert op een methodeaanroep van de oplossing voor externe controle
> * Fouten opsporen in uw scripts

Nu dat u hebt geleerd om op te geven van het gedrag van een gesimuleerd apparaat, de voorgestelde volgende stap is te leren hoe u [een gesimuleerd apparaat maakt](iot-accelerators-remote-monitoring-create-simulated-device.md).

Zie voor meer informatie voor ontwikkelaars over de oplossing voor externe controle:

* [Snelzoekgids voor ontwikkelaars](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Ontwikkelaarsgids voor het oplossen van problemen](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->
