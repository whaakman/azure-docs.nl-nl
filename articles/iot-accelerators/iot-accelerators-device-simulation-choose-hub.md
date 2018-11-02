---
title: Gebruik een bestaande IoT-hub met de Apparaatsimulatie-oplossing - Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe u de oplossingsversneller voor Apparaatsimulatie voor het gebruik van een IoT Hub configureren.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/25/2018
ms.topic: conceptual
ms.openlocfilehash: 38cde750ce07741a433baa1b8607a584f94ad9b1
ms.sourcegitcommit: 3dcb1a3993e51963954194ba2a5e42260d0be258
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50753913"
---
# <a name="use-an-existing-iot-hub-with-the-device-simulation-solution-accelerator"></a>Gebruik een bestaande IoT-hub met de oplossingsversnellers Apparaatsimulatie

Wanneer u Apparaatsimulatie implementeren, kunt u ook kiezen voor het implementeren van een IoT-hub te gebruiken in uw simulatie. Deze optie wordt geïmplementeerd een [S2-laag IoT-hub met een één schaaleenheid](../iot-hub/iot-hub-scaling.md). Als u deze optionele IoT hub implementeert, kunt u nog steeds gericht op een andere IoT-Hub voor het uitvoeren van de simulatie.

Als u niet de optionele IoT-Hub implementeren, moet u uw eigen hub gebruiken voor elke simulaties uitvoeren.

Als u een IoT-hub hebt, kunt u altijd een nieuw label van maken de [Azure-portal](https://portal.azure.com).

Voor het gebruik van een bestaande IoT-hub, moet u de verbindingsreeks voor de **iothubowner** gedeeld toegangsbeleid. U kunt deze verbindingsreeks uit de [Azure-portal](https://portal.azure.com):

1. Klik op de configuratiepagina van de hub in de portal op **beleid voor gedeelde toegang**.

1. Klik op **iothubowner**.

1. Kopieer de primaire of secundaire verbindingsreeks.

[![Verbindingsreeks ophalen](./media/iot-accelerators-device-simulation-choose-hub/connectionstring-inline.png)](./media/iot-accelerators-device-simulation-choose-hub/connectionstring-expanded.png#lightbox)

Gebruik de verbindingsreeks die u bij het configureren van de simulatie gekopieerd:

![Simulatie configureren](./media/iot-accelerators-device-simulation-choose-hub/configuresimulation.png)

### <a name="next-steps"></a>Volgende stappen

In deze handleiding, hebt u geleerd hoe u een bestaande IoT-hub in de simulatie. Vervolgens kunt u leren hoe u [maken van een geavanceerde Apparaatmodel](iot-accelerators-device-simulation-advanced-device.md) voor een simulatie.
