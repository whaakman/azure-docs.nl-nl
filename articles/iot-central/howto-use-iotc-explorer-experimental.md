---
title: Apparaat-connectiviteit controleren met behulp van Azure IoT Central Explorer
description: Bewaken van apparaat-berichten en bekijk device twin wijzigingen via de IoT Central Explorer CLI.
author: viv-liu
ms.author: viviali
ms.date: 02/20/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 167a7b008af521c5f4e403a3786b004138d69da9
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57311148"
---
# <a name="monitor-device-connectivity-using-the-azure-iot-central-explorer"></a>Apparaat-connectiviteit controleren met behulp van Azure IoT Central Explorer

*In dit onderwerp is van toepassing op builders en beheerders.*

Zie uw apparaten bij IoT Central verzenden en houd rekening met wijzigingen op het dubbele IoT Hub-berichten met behulp van IoT Central Explorer CLI. Deze open-source-hulpprogramma kunt u beter inzicht krijgen in de status van de connectiviteit van apparaten en problemen van apparaat-berichten niet bereiken van de cloud of niet reageert op wijzigingen van de dubbele apparaten.

[Ga naar de iotc-explorer-opslagplaats in GitHub.](https://aka.ms/iotciotcexplorercligithub)

## <a name="prerequisites"></a>Vereisten

+ Node.js versie 8.x of hoger: https://nodejs.org
+ Een beheerder van uw toepassing moet een toegangstoken voor gebruik in iotc-explorer genereren

## <a name="install-iotc-explorer"></a>Installeer iotc-explorer

Voer de volgende opdracht uit vanaf de opdrachtregel om te installeren:

```cmd/sh
npm install -g iotc-explorer
```

> [!NOTE]
> Normaal gesproken moet u uitvoeren de installatieopdracht met `sudo` in Unix-achtige omgevingen.

## <a name="run-iotc-explorer"></a>Iotc explorer uitvoeren

De volgende secties worden veelgebruikte opdrachten en opties die u gebruiken kunt tijdens het uitvoeren van `iotc-explorer`. Als u de volledige set opdrachten en opties, doorgeven `--help` naar `iotc-explorer` of een van de subopdrachten.

### <a name="login"></a>Aanmelden

Voordat u aan de slag gaat, moet u een beheerder van uw IoT Central-toepassing in een toegangstoken voor gebruik. De beheerder heeft de volgende stappen uit:

1. Navigeer naar **beheer** vervolgens **toegangstokens**.
1. Selecteer **Token genereren**.
    ![Schermafbeelding van pagina-token toegang](media/howto-use-iotc-explorer-experimental/accesstokenspage.png)

1. Voer de Tokennaam van een, selecteert u **volgende**, en vervolgens **kopie**.
    > [!NOTE]
    > De waarde voor token wordt alleen slechts één keer worden weergegeven, zodat deze moet worden gekopieerd voordat u het dialoogvenster te sluiten. Na het sluiten van het dialoogvenster, wordt deze nooit opnieuw weergegeven.

    ![Schermafbeelding van dialoogvenster-token toegang kopiëren](media/howto-use-iotc-explorer-experimental/copyaccesstoken.png)

U kunt het token gebruiken voor aanmelding bij de CLI als volgt:

```cmd/sh
iotc-explorer login "<Token value>"
```

Als u liever geen het token wordt bewaard in de geschiedenis van uw shell, kunt u het token out en deze in plaats daarvan opgeven wanneer u hierom wordt gevraagd:

```cmd/sh
iotc-explorer login
```

### <a name="monitor-device-messages"></a>Monitor voor apparaat-berichten

U kunt controleren of de berichten die afkomstig zijn van een specifiek apparaat of alle apparaten in uw toepassing met de `monitor-messages` opdracht. Met deze opdracht start een watcher die voortdurend nieuwe berichten uitvoert wanneer ze binnenkomen:

Als u wilt bekijken van alle apparaten in uw toepassing, moet u de volgende opdracht uitvoeren:

```cmd/sh
iotc-explorer monitor-messages
```

Uitvoer:

![uitvoer van de opdracht Monitor-berichten](media/howto-use-iotc-explorer-experimental/monitormessages.png)

Als u wilt een specifiek apparaat bekijken, moet u alleen de apparaat-id toevoegen aan het einde van de opdracht:

```cmd/sh
iotc-explorer monitor-messages <your-device-id>
```

U kunt ook een meer machine-vriendelijk-indeling van uitvoer door toe te voegen de `--raw` optie met de opdracht:

```
iotc-explorer monitor-messages --raw
```

### <a name="get-device-twin"></a>Ophalen van apparaatdubbel

U kunt de `get-twin` opdracht voor het ophalen van de inhoud van het dubbele voor een apparaat met IoT Central. Voer de volgende opdracht uit om dit te doen:

```cmd/sh
iotc-explorer get-twin <your-device-id>
```

Uitvoer:

![uitvoer van de opdracht Get-twin](media/howto-use-iotc-explorer-experimental/getdevicetwin.png)

Net als bij `monitor-messages`, krijgt u een meer machine-vriendelijk uitvoer door door te geven de `--raw` optie:

```cmd/sh
iotc-explorer get-twin <your-device-id> --raw
```

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u met de IoT Central Explorer, de voorgestelde volgende stap is om te verkennen [beheer van apparaten IoT Central](howto-manage-devices-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).
