---
title: Gesimuleerd apparaat gedrag in de oplossing in de apparaat-simulatie - Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe u JavaScript gebruiken om te bepalen het gedrag van een gesimuleerd apparaat in de oplossing voor het simuleren van apparaten.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: 43edbc653ddbd55aab5e722071de1f2cf4bcd1c4
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344513"
---
# <a name="implement-the-device-model-behavior"></a>Gedrag van het Apparaatmodel implementeren

Het artikel [schema van het Apparaatmodel begrijpen](iot-accelerators-device-simulation-device-schema.md) beschreven van het schema dat u een gesimuleerde apparaat-model definieert. Dit artikel waarnaar wordt verwezen naar twee typen van JavaScript-bestand die het gedrag van een gesimuleerd apparaat implementeren:

- **Status**: JavaScript-bestanden die worden uitgevoerd met vaste intervallen om bij te werken van de interne status van het apparaat.
- **Methode**: JavaScript-bestanden die worden uitgevoerd wanneer de oplossing een methode op het apparaat roept.

In dit artikel leert u het volgende:

>[!div class="checklist"]
> * Beheren van de status van een gesimuleerd apparaat
> * Definiëren hoe een gesimuleerd apparaat reageert op een methodeaanroep van de IoT-hub die verbonden met
> * Fouten opsporen in uw scripts

[!INCLUDE [iot-accelerators-device-schema](../../includes/iot-accelerators-device-schema.md)]

## <a name="next-steps"></a>Volgende stappen

Dit artikel wordt beschreven hoe u definieert het gedrag van uw eigen aangepaste gesimuleerde apparaat-model. In dit artikel hebt u geleerd hoe aan:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Beheren van de status van een gesimuleerd apparaat
> * Definiëren hoe een gesimuleerd apparaat reageert op een methodeaanroep van de IoT-hub die verbonden met
> * Fouten opsporen in uw scripts

Nu dat u hebt geleerd om op te geven van het gedrag van een gesimuleerd apparaat, de voorgestelde volgende stap is te leren hoe u [een gesimuleerd apparaat maakt](iot-accelerators-device-simulation-create-simulated-device.md).

Zie voor meer informatie voor ontwikkelaars over de Apparaatsimulatie-oplossing, de [Snelzoekgids voor ontwikkelaars](https://github.com/Azure/device-simulation-dotnet/wiki/Simulation-Service-Developer-Reference-Guide).
