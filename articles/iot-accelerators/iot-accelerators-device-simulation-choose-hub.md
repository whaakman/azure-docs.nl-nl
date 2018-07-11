---
title: Gebruik een bestaande IoT-hub met de Apparaatsimulatie-oplossing - Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe u de oplossingsversneller voor Apparaatsimulatie voor het gebruik van een IoT Hub configureren.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 07/06/2018
ms.topic: conceptual
ms.openlocfilehash: ee96173ca5f36dee0f08c38e4b6e29da6fee804e
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967534"
---
# <a name="use-an-existing-iot-hub-with-the-device-simulation-solution-accelerator"></a>Gebruik een bestaande IoT-hub met de oplossingsversnellers Apparaatsimulatie

Wanneer u de oplossingsversneller voor Apparaatsimulatie inricht, kunt u kiezen voor het implementeren van een IoT-hub in de solution accelerator resourcegroep waarin u wilt gebruiken in uw simulatie.

Als u niet de optionele IoT-Hub implementeren, moet u uw eigen hub gebruiken voor elke simulaties uitvoeren. Als u kiest de optionele IoT-Hub implementeren, kunt u deze optionele hub of uw eigen hub te gebruiken.

Als u een IoT-hub hebt, kunt u altijd een nieuw label van maken de [Azure-portal](https://portal.azure.com).

Als u een reeds bestaande IoT-hub, moet u een verbindingsreeks voor de **iothubowner** gedeeld toegangsbeleid. U kunt deze verbindingsreeks uit de [Azure-portal](https://portal.azure.com):

1. Klik op de configuratiepagina van de hub in de portal op **beleid voor gedeelde toegang**.
1. Klik op **iothubowner**.
1. Kopieer de primaire of secundaire verbindingsreeks.

[![Verbindingsreeks ophalen](./media/iot-accelerators-device-simulation-choose-hub/connectionstring-inline.png)](./media/iot-accelerators-device-simulation-choose-hub/connectionstring-expanded.png#lightbox)

Gebruik de verbindingsreeks die u bij het configureren van de simulatie gekopieerd:

[![Simulatie configureren](./media/iot-accelerators-device-simulation-choose-hub/configuresimulation-inline.png)](./media/iot-accelerators-device-simulation-choose-hub/configuresimulation-expanded.png#lightbox)

## <a name="next-steps"></a>Volgende stappen

In deze handleiding, hebt u geleerd hoe u een bestaande IoT-hub in de simulatie. Vervolgens kunt u leren hoe u [configureren van een aangepaste Apparaatmodel](iot-accelerators-device-simulation-custom-model.md) voor een simulatie.
